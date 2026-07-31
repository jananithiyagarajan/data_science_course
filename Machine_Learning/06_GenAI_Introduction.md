# Generative AI (GenAI) — Introduction

## What is Generative AI?

**Generative AI** refers to AI systems that can **create new content** — text, images, audio, video, code, and more — that mimics human-created content.

Unlike traditional AI (which classifies or predicts), Generative AI **generates** new outputs based on learned patterns from massive datasets.

### Examples of Generative AI

| Type              | Examples                                     |
| ----------------- | -------------------------------------------- |
| Text Generation   | ChatGPT, Gemini, Claude, Llama               |
| Image Generation  | DALL-E, Stable Diffusion, Midjourney         |
| Code Generation   | GitHub Copilot, Amazon CodeWhisperer         |
| Audio Generation  | ElevenLabs (voice), Suno (music)             |
| Video Generation  | Sora (OpenAI), Runway ML                     |
| Multimodal        | GPT-4o, Gemini Pro Vision                    |

---

## 1. How Generative AI Works

### The Core Idea: Training on Massive Data

```
Massive Text Data (internet, books, code)
         ↓
Pre-training (learn language patterns)
         ↓
Fine-tuning (specialize for tasks)
         ↓
RLHF (Reinforcement Learning from Human Feedback)
         ↓
Final Model (GPT, Gemini, etc.)
```

### Key Architecture: Transformers

The **Transformer** architecture (introduced in 2017 paper "Attention is All You Need") powers all modern LLMs.

Key components:
- **Self-Attention**: Words understand their relationship to all other words in the sentence
- **Multi-Head Attention**: Multiple attention heads look at different patterns simultaneously
- **Positional Encoding**: Preserves word order information
- **Feed-Forward Networks**: Process the attended representations

---

## 2. Large Language Models (LLMs)

**LLMs** are large neural networks trained on billions of text tokens, capable of generating coherent, contextually relevant text.

### How LLMs Generate Text

LLMs predict the next most likely token (word/subword) given the context:

```
Input: "Data Science is a"
Output: "fascinating field that combines statistics, programming..."
```

This is done **auto-regressively** — one token at a time.

### Key Concepts

| Term                | Meaning                                                    |
| ------------------- | ---------------------------------------------------------- |
| **Token**           | A chunk of text (word, subword, or character)              |
| **Context Window**  | Max tokens the model can "see" at once (e.g., 128k tokens) |
| **Temperature**     | Controls randomness (0=deterministic, 1=creative)          |
| **Top-p / Top-k**   | Controls diversity of output                               |
| **Hallucination**   | When LLM generates confident but incorrect information     |
| **Fine-tuning**     | Training a pre-trained model on specific data              |
| **RAG**             | Retrieval-Augmented Generation — ground LLM in facts       |

---

## 3. Prompt Engineering

**Prompt Engineering** is the art of crafting inputs to get the best output from an LLM.

### Types of Prompts

#### Zero-Shot
No examples — directly ask the question.
```
Prompt: "Translate 'Hello, how are you?' to Tamil."
Output: "வணக்கம், நீங்கள் எப்படி இருக்கிறீர்கள்?"
```

#### Few-Shot
Provide 2–3 examples before your question.
```
Prompt:
Translate English to Tamil:
Hello → வணக்கம்
Thank you → நன்றி
Good morning → ?

Output: காலை வணக்கம்
```

#### Chain-of-Thought (CoT)
Ask the model to think step by step.
```
Prompt:
"Think step by step: If I have 5 apples and give 2 to my friend, 
then buy 3 more, how many do I have?"

Output:
Step 1: Start with 5 apples
Step 2: Give 2 away → 5 - 2 = 3
Step 3: Buy 3 more → 3 + 3 = 6
Answer: 6 apples
```

#### Persona / Role-Based
```
Prompt:
"You are an expert Python developer. Review the following code and 
suggest improvements for performance and readability:
[code here]"
```

### Prompt Engineering Best Practices

1. **Be specific**: Clear, detailed prompts → better outputs
2. **Give context**: Tell the model what you need and why
3. **Specify format**: "Respond in a table", "List 5 bullet points"
4. **Use delimiters**: Use `"""`, `---`, `<tags>` to separate input sections
5. **Iterate**: Refine prompts based on output quality

---

## 4. Using LLMs with Python APIs

### OpenAI GPT API

```python
from openai import OpenAI

client = OpenAI(api_key="your-api-key-here")

# Simple chat completion
response = client.chat.completions.create(
    model="gpt-4o",
    messages=[
        {"role": "system", "content": "You are a helpful data science tutor."},
        {"role": "user", "content": "Explain the difference between supervised and unsupervised learning."}
    ],
    temperature=0.7,
    max_tokens=500
)

print(response.choices[0].message.content)
```

### Google Gemini API

```python
import google.generativeai as genai

genai.configure(api_key="your-gemini-api-key")
model = genai.GenerativeModel("gemini-1.5-flash")

response = model.generate_content(
    "Explain deep learning to a 10-year-old."
)
print(response.text)

# With chat history
chat = model.start_chat(history=[])
response = chat.send_message("What is machine learning?")
print(response.text)
response = chat.send_message("Give me an example.")
print(response.text)
```

### Hugging Face Transformers

```python
from transformers import pipeline

# Text generation
generator = pipeline("text-generation", model="gpt2")
output = generator("Python is a great programming language because", max_length=100)
print(output[0]["generated_text"])

# Summarization
summarizer = pipeline("summarization", model="facebook/bart-large-cnn")
text = """
Machine learning is a method of data analysis that automates analytical model building.
It is based on the idea that systems can learn from data, identify patterns and make 
decisions with minimal human intervention.
"""
summary = summarizer(text, max_length=50, min_length=10)
print(summary[0]["summary_text"])

# Question Answering
qa = pipeline("question-answering", model="distilbert-base-uncased-distilled-squad")
context = "Python was created by Guido van Rossum. It was first released in 1991."
result = qa(question="Who created Python?", context=context)
print(result["answer"])
```

---

## 5. RAG — Retrieval-Augmented Generation

**RAG** combines LLMs with a retrieval system to ground responses in real, up-to-date documents.

```
User Query
    ↓
Search relevant documents (vector database)
    ↓
Add retrieved documents as context to the prompt
    ↓
LLM generates answer based on retrieved facts
    ↓
Accurate, grounded response
```

### Simple RAG with LangChain

```python
# Install: pip install langchain openai chromadb
from langchain_openai import ChatOpenAI, OpenAIEmbeddings
from langchain_community.vectorstores import Chroma
from langchain.text_splitter import RecursiveCharacterTextSplitter
from langchain.chains import RetrievalQA
from langchain.document_loaders import TextLoader

# Load documents
loader = TextLoader("my_docs.txt")
docs = loader.load()

# Split into chunks
splitter = RecursiveCharacterTextSplitter(chunk_size=500, chunk_overlap=50)
splits = splitter.split_documents(docs)

# Create vector store
embeddings = OpenAIEmbeddings()
vectorstore = Chroma.from_documents(splits, embeddings)

# Create RAG chain
llm = ChatOpenAI(model="gpt-4o", temperature=0)
qa_chain = RetrievalQA.from_chain_type(
    llm=llm,
    retriever=vectorstore.as_retriever(search_kwargs={"k": 3})
)

# Ask questions
answer = qa_chain.run("What is the main topic of these documents?")
print(answer)
```

---

## 6. Fine-Tuning LLMs

**Fine-tuning** adapts a pre-trained model to a specific domain or task using your own data.

```python
# Using Hugging Face Trainer for fine-tuning
from transformers import (
    AutoModelForSequenceClassification,
    AutoTokenizer,
    TrainingArguments,
    Trainer
)
from datasets import Dataset
import torch

# Load pre-trained model
model_name = "distilbert-base-uncased"
tokenizer = AutoTokenizer.from_pretrained(model_name)
model = AutoModelForSequenceClassification.from_pretrained(model_name, num_labels=2)

# Tokenize dataset
def tokenize(examples):
    return tokenizer(examples["text"], truncation=True, padding=True, max_length=512)

# Training arguments
training_args = TrainingArguments(
    output_dir="./results",
    num_train_epochs=3,
    per_device_train_batch_size=16,
    per_device_eval_batch_size=16,
    warmup_steps=500,
    weight_decay=0.01,
    evaluation_strategy="epoch",
    save_strategy="epoch",
    load_best_model_at_end=True,
)

# Trainer
trainer = Trainer(
    model=model,
    args=training_args,
    train_dataset=train_dataset,
    eval_dataset=eval_dataset,
    tokenizer=tokenizer,
)

trainer.train()
```

---

## 7. Responsible AI & AI Ethics

### Key Challenges with Generative AI

| Challenge         | Description                                                |
| ----------------- | ---------------------------------------------------------- |
| **Hallucination** | LLMs generate confident but incorrect information          |
| **Bias**          | Models reflect biases in training data                     |
| **Privacy**       | Risk of leaking private information from training data     |
| **Misinformation**| Generated content can be used to spread fake information   |
| **Copyright**     | Training on copyrighted content raises legal questions     |
| **Job Impact**    | Automation of knowledge work                               |

### Best Practices for Responsible GenAI Use

1. **Verify outputs** — never trust LLM output blindly for critical decisions
2. **Use RAG** — ground LLMs in verified, up-to-date documents
3. **Human oversight** — maintain human review for high-stakes decisions
4. **Transparency** — disclose when AI-generated content is used
5. **Bias testing** — test models for demographic and cultural bias
6. **Privacy protection** — never send sensitive PII data to external APIs

---

## 🎯 Student Tasks – Module 22 Part 2: Generative AI

### Task 1: Prompt Engineering Explorer (Easy)
**Objective**: Practice different prompting strategies.

**Instructions**:
Use Gemini API or OpenAI API (or a free alternative like Ollama/Groq).

Write prompts for each technique:
1. **Zero-Shot**: Ask the model to write a Python function to reverse a string.
2. **Few-Shot**: Provide 2 examples of Tamil-to-English translation, then ask for a new one.
3. **Chain-of-Thought**: Ask the model to solve a multi-step math problem step by step.
4. **Persona**: Ask as a "senior data scientist reviewing a junior's analysis".
5. **Format control**: Ask for output as a markdown table.

Compare outputs across techniques. Which gave the best result?

**Expected Output**:
```
Zero-Shot Result:
def reverse_string(s): return s[::-1]

Few-Shot Translation:
கணிணி → Computer

CoT Math (Step by Step):
Step 1: ...
Step 2: ...
Answer: 42

Persona Review:
"As a senior data scientist, I notice the model has high variance..."

Table Output:
| Feature | Value | Observation |
|---------|-------|-------------|
```

---

### Task 2: Build a Simple Q&A Chatbot (Medium)
**Objective**: Create a conversational AI assistant.

**Instructions**:
Using Google Gemini API or Hugging Face:
1. Create a Python script that:
   - Maintains conversation history
   - Has a persona: "You are a friendly data science tutor named DataBot."
   - Supports multi-turn conversations
   - Has a `/clear` command to reset history
   - Has a `/exit` command to quit
2. Test with at least 5 exchanges.
3. Print the full conversation log at the end.

**Expected Output**:
```
DataBot: Hello! I'm DataBot, your data science tutor. Ask me anything!

You: What is overfitting?
DataBot: Overfitting occurs when a model learns the training data too well...

You: Give me an example.
DataBot: Sure! Imagine a student who memorizes all practice questions...

You: /clear
DataBot: Conversation cleared!

You: /exit
DataBot: Goodbye! Happy learning!

--- Conversation Log saved to chat_log.txt ---
```

---

### Task 3: RAG-based Document QA System (Challenge)
**Objective**: Build a complete RAG pipeline to answer questions from documents.

**Instructions**:
1. Gather 3–5 text documents (e.g., Wikipedia articles, your course notes, research papers).
2. Build a RAG system:
   - **Chunk** documents (e.g., 500 chars, 50 overlap)
   - **Embed** chunks using `sentence-transformers` (`all-MiniLM-L6-v2`)
   - **Store** in a vector database (`ChromaDB` or `FAISS`)
   - **Retrieve** top-3 relevant chunks for each query
   - **Generate** answer using Gemini or GPT
3. Test with 10 questions from the documents.
4. Compare: Does RAG give better answers than direct LLM (without context)?
5. Print a report with: question, retrieved context snippet, and final answer.

**Expected Output**:
```
=== RAG System Ready ===
Documents loaded: 5
Total chunks: 143
Vector store: ChromaDB

Query 1: "What is the transformer architecture?"
Retrieved chunks: 3 (similarity > 0.75)
Answer: "The transformer architecture, introduced in 2017, uses..."

[Comparison]
Without RAG: "Transformers are a type of neural network..."
With RAG:    "According to the Attention is All You Need paper (2017),
              transformers use self-attention mechanisms to..."

RAG Answer was more specific and accurate.
```

---
