# Introduction to Generative AI

## What is Generative AI?

**Generative AI (Gen AI)** refers to artificial intelligence systems capable of **creating new content** — text, images, code, audio, video, and more — that is original and contextually relevant.

Unlike **discriminative AI** (which classifies or predicts), Generative AI **learns the underlying distribution of data** and generates new samples from it.

```
Discriminative AI:  Input → Label
                    (Is this email spam? Yes/No)

Generative AI:      Prompt → New Content
                    ("Write me an email about X" → complete email)
```

---

## The Generative AI Landscape

```
Generative AI
    ├── Text Generation
    │       └── LLMs: GPT-4, Gemini, Claude, Llama, Mistral
    ├── Image Generation
    │       └── DALL-E 3, Stable Diffusion, Midjourney, Imagen
    ├── Code Generation
    │       └── GitHub Copilot, CodeLlama, Gemini Code
    ├── Audio Generation
    │       └── ElevenLabs (voice), Suno/Udio (music)
    ├── Video Generation
    │       └── Sora (OpenAI), Runway ML, Pika
    └── Multimodal
            └── GPT-4o, Gemini Pro Vision, Claude Vision
```

---

## Key Concepts

### 1. Large Language Models (LLMs)

LLMs are massive neural networks trained on **billions of tokens** (words/subwords) from the internet, books, and code.

| Model Family  | Developer  | Key Strengths                    |
| ------------- | ---------- | -------------------------------- |
| GPT-4o        | OpenAI     | Multimodal, reasoning, code      |
| Gemini        | Google     | Integration with Google, large context |
| Claude        | Anthropic  | Long context, safety-focused     |
| Llama 3       | Meta       | Open-source, customizable        |
| Mistral       | Mistral AI | Fast, efficient, open-source     |

### 2. Tokenization

LLMs don't read words — they read **tokens** (pieces of words).

```python
from transformers import AutoTokenizer

tokenizer = AutoTokenizer.from_pretrained("gpt2")
text = "Data Science is amazing!"
tokens = tokenizer.tokenize(text)
print(tokens)  # ['Data', 'Ġscience', 'Ġis', 'Ġamazing', '!']
print(f"Words: {len(text.split())} | Tokens: {len(tokens)}")
# Words: 4 | Tokens: 5
```

### 3. Embeddings

Words are represented as **dense vectors** — similar words have similar vectors.

```python
from sentence_transformers import SentenceTransformer
import numpy as np

model = SentenceTransformer('all-MiniLM-L6-v2')

sentences = [
    "Machine learning is a field of AI",
    "Artificial intelligence includes machine learning",
    "Python is a programming language",
    "Deep learning is a subset of machine learning"
]

embeddings = model.encode(sentences)
print(f"Embedding shape: {embeddings.shape}")  # (4, 384)

# Cosine similarity
def cosine_similarity(a, b):
    return np.dot(a, b) / (np.linalg.norm(a) * np.linalg.norm(b))

sim_0_1 = cosine_similarity(embeddings[0], embeddings[1])
sim_0_2 = cosine_similarity(embeddings[0], embeddings[2])
print(f"AI sentences similarity: {sim_0_1:.3f}")    # ~0.85 (similar)
print(f"Different topic similarity: {sim_0_2:.3f}") # ~0.30 (different)
```

### 4. Temperature & Sampling Parameters

```python
from openai import OpenAI

client = OpenAI(api_key="your-key")

# Temperature controls creativity/randomness
# 0.0 = deterministic, 1.0 = creative, 2.0 = chaotic

# Formal/deterministic response
formal_response = client.chat.completions.create(
    model="gpt-4o",
    messages=[{"role": "user", "content": "Explain recursion"}],
    temperature=0.0,   # Very consistent
    max_tokens=200
)

# Creative response
creative_response = client.chat.completions.create(
    model="gpt-4o",
    messages=[{"role": "user", "content": "Explain recursion"}],
    temperature=0.9,   # More varied and creative
    max_tokens=200,
    top_p=0.9          # Nucleus sampling
)
```

---

## Prompt Engineering Techniques

### 1. Zero-Shot Prompting
```
Classify the sentiment: "This movie was incredible!"
Answer: Positive
```

### 2. Few-Shot Prompting
```
Classify sentiment:
"The food was terrible." → Negative
"Service was excellent!" → Positive
"It was okay I guess."  → Neutral
"This is the best day ever!" → ???
```

### 3. Chain-of-Thought (CoT)
```
Solve step by step:
Q: A train leaves at 9am, travels 120km at 60km/h. 
   When does it arrive?

A: Let me think step by step.
Step 1: Distance = 120km, Speed = 60km/h
Step 2: Time = Distance / Speed = 120/60 = 2 hours
Step 3: Arrival = 9am + 2 hours = 11am
Answer: 11:00 AM
```

### 4. ReAct (Reasoning + Acting)
```
Q: What's the population of the capital of France?
Thought: I need to find the capital of France first.
Action: Search("capital of France")
Observation: Paris
Thought: Now I need Paris's population.
Action: Search("Paris population 2024")
Observation: ~2.1 million (city), ~12 million (metro)
Answer: Paris, France's capital, has ~2.1M city population.
```

---

## Using LLM APIs

### Google Gemini API

```python
import google.generativeai as genai

genai.configure(api_key="YOUR_GEMINI_API_KEY")

# ── Basic Generation ───────────────────────────────────────────────────────────
model = genai.GenerativeModel("gemini-1.5-flash")

response = model.generate_content("Explain machine learning in 3 sentences.")
print(response.text)

# ── Multi-turn Conversation ───────────────────────────────────────────────────
chat = model.start_chat(history=[])

messages = [
    "What is a neural network?",
    "How does backpropagation work?",
    "Give me a simple Python example."
]

for message in messages:
    response = chat.send_message(message)
    print(f"\nQ: {message}")
    print(f"A: {response.text}")

# ── System Instructions ───────────────────────────────────────────────────────
model_with_system = genai.GenerativeModel(
    model_name="gemini-1.5-flash",
    system_instruction="""You are DataBot, a friendly data science tutor.
    Always explain concepts with real-world examples.
    Use simple language. Be encouraging."""
)
response = model_with_system.generate_content("What is overfitting?")
print(response.text)
```

### OpenAI GPT API

```python
from openai import OpenAI

client = OpenAI(api_key="YOUR_OPENAI_API_KEY")

# ── Simple Completion ─────────────────────────────────────────────────────────
response = client.chat.completions.create(
    model="gpt-4o",
    messages=[
        {"role": "system", "content": "You are a helpful data science tutor."},
        {"role": "user", "content": "What is the difference between AI and ML?"}
    ],
    temperature=0.7,
    max_tokens=300
)
print(response.choices[0].message.content)

# ── Streaming Response ────────────────────────────────────────────────────────
stream = client.chat.completions.create(
    model="gpt-4o",
    messages=[{"role": "user", "content": "Write a Python function to sort a list"}],
    stream=True
)
for chunk in stream:
    if chunk.choices[0].delta.content:
        print(chunk.choices[0].delta.content, end='', flush=True)
```

---

## Hugging Face Transformers

```python
from transformers import pipeline

# ── Text Generation ───────────────────────────────────────────────────────────
generator = pipeline("text-generation", model="gpt2")
result = generator("Deep learning is", max_length=50, num_return_sequences=2)
for r in result:
    print(r['generated_text'])

# ── Question Answering ────────────────────────────────────────────────────────
qa = pipeline("question-answering",
              model="distilbert-base-uncased-distilled-squad")
result = qa(
    question="Who invented Python?",
    context="Python was created by Guido van Rossum and first released in 1991. "
             "It is now maintained by the Python Software Foundation."
)
print(f"Answer: {result['answer']} (Score: {result['score']:.3f})")

# ── Text Summarization ────────────────────────────────────────────────────────
summarizer = pipeline("summarization", model="facebook/bart-large-cnn")
article = """
Generative AI is transforming how we interact with technology. Large language models 
like GPT-4 and Gemini can now write code, answer complex questions, and even generate
creative content like stories and poems. This technology has the potential to 
revolutionize industries from healthcare to education.
"""
summary = summarizer(article, max_length=60, min_length=20, do_sample=False)
print(summary[0]['summary_text'])

# ── Sentiment Analysis ────────────────────────────────────────────────────────
sentiment = pipeline("sentiment-analysis")
texts = [
    "I absolutely love this course!",
    "This is so confusing and frustrating.",
    "The content is okay, nothing special."
]
for text in texts:
    result = sentiment(text)[0]
    print(f"{text[:40]:<40} → {result['label']} ({result['score']:.3f})")
```

---

## Vector Databases & Semantic Search

Vector databases store embeddings and enable **semantic search** (find similar content).

```python
import chromadb
from sentence_transformers import SentenceTransformer

# ── Setup ─────────────────────────────────────────────────────────────────────
client  = chromadb.Client()
collection = client.create_collection("course_docs")
embed_model = SentenceTransformer('all-MiniLM-L6-v2')

# ── Add Documents ──────────────────────────────────────────────────────────────
documents = [
    "Machine learning is a subset of artificial intelligence",
    "Neural networks are inspired by the human brain",
    "Python is widely used for data science",
    "Deep learning requires large amounts of data",
    "SQL is used to query relational databases",
    "Pandas is a popular data manipulation library"
]

embeddings = embed_model.encode(documents).tolist()
collection.add(
    documents=documents,
    embeddings=embeddings,
    ids=[f"doc_{i}" for i in range(len(documents))]
)

# ── Semantic Search ────────────────────────────────────────────────────────────
query = "How do AI systems learn from data?"
query_embedding = embed_model.encode([query]).tolist()

results = collection.query(
    query_embeddings=query_embedding,
    n_results=3
)

print(f"Query: {query}\n")
print("Top 3 relevant documents:")
for doc, distance in zip(results['documents'][0], results['distances'][0]):
    print(f"  [{1-distance:.3f}] {doc}")
```

---

## Ethical Considerations

| Issue               | Description                                          | Mitigation                        |
| ------------------- | ---------------------------------------------------- | --------------------------------- |
| **Hallucination**   | LLMs generate confidently wrong information          | RAG, fact-checking, human review  |
| **Bias**            | Reflects biases in training data                     | Diverse training data, RLHF       |
| **Privacy**         | Risk of leaking PII from training data               | Privacy-preserving training       |
| **Misinformation**  | Easy to generate fake but convincing content         | Watermarking, content detection   |
| **Job Displacement**| Automation of knowledge work                         | Reskilling, responsible deployment|
| **Copyright**       | Training on copyrighted content                      | Licensing, attribution            |

---

## 🎯 Student Tasks – Gen AI Introduction

### Task 1: LLM API Explorer (Easy)
**Objective**: Get comfortable using LLM APIs.

**Instructions**:
Using Gemini API (free tier) or Hugging Face (free):
1. Set up API access and test connection.
2. Write prompts for 5 different tasks:
   - Explain a data science concept in simple terms
   - Generate Python code for a specific task
   - Summarize a paragraph of text
   - Translate a sentence to Tamil and Hindi
   - Create 5 quiz questions about machine learning
3. Experiment with `temperature=0.1` vs `temperature=0.9` for one prompt.
4. Count tokens used (from API response metadata).
5. Print a formatted report of all responses.

**Expected Output**:
```
=== LLM API Test Report ===
Model: gemini-1.5-flash

Task 1: Concept Explanation
  Prompt: "Explain overfitting to a 12-year-old"
  Response: "Imagine you studied only last year's exam..."
  Tokens: 187

Task 2: Code Generation
  Prompt: "Write Python to find all duplicates in a list"
  Response: def find_duplicates(lst): ...
  
Temperature Comparison:
  T=0.1: "Overfitting occurs when a model learns..."
  T=0.9: "Think of it like a student who memorizes..."
```

---

### Task 2: Multi-turn Chatbot (Medium)
**Objective**: Build a conversational AI assistant.

**Instructions**:
Build a Python chatbot with the following features:
1. Persona: "You are DataBot, a friendly data science tutor for beginners."
2. **Memory**: Maintain full conversation history (last 10 exchanges).
3. **Commands**:
   - `/clear` → reset conversation
   - `/history` → show last 5 messages
   - `/topic <topic>` → switch to explaining that topic
   - `/quiz` → generate 3 quiz questions on current topic
   - `/exit` → end session
4. **Session Summary**: When exiting, print total messages and topics discussed.
5. Save conversation log to `chat_log.txt`.

**Expected Output**:
```
DataBot: Hi! I'm DataBot. What data science topic shall we explore?

You: What is a decision tree?
DataBot: A decision tree is like a flowchart that makes decisions...

You: /quiz
DataBot: Quiz time! 3 questions on Decision Trees:
  1. What is the main advantage of decision trees?
  2. What is pruning in decision trees?
  3. What is the difference between Gini impurity and entropy?

You: /exit
DataBot: Great session! Summary:
  - Messages: 12
  - Topics: Decision Trees, Random Forest
  Conversation saved to chat_log.txt
```

---

### Task 3: Semantic Search System (Challenge)
**Objective**: Build a semantic search engine over a document corpus.

**Instructions**:
1. **Dataset**: Collect 50+ text documents (Wikipedia articles, course notes, etc.).
2. **Preprocessing**: Chunk each document into 300-char segments with 50-char overlap.
3. **Embedding**: Use `sentence-transformers/all-MiniLM-L6-v2` to embed all chunks.
4. **Storage**: Store in ChromaDB with metadata (source, chunk_id, date).
5. **Search Interface**:
   - Accept a natural language query
   - Return top-5 relevant chunks with similarity score
   - Display source document name
6. **Evaluation**: Test with 10 queries, manually evaluate relevance (1-5 scale).
7. **Bonus**: Add a Gemini/GPT call to synthesize an answer from retrieved chunks (RAG).

**Expected Output**:
```
=== Semantic Search System ===
Documents: 50 | Chunks: 347 | Embeddings: 347 (dim=384)

Query: "How does gradient descent work?"
───────────────────────────────────────
Result 1 (score=0.91): gradient_descent.md, chunk 3
  "Gradient descent is an optimization algorithm that minimizes
   the loss function by iteratively moving in the direction of..."

Result 2 (score=0.87): neural_networks.md, chunk 7
  "During backpropagation, gradients are computed and weights
   are updated using gradient descent..."

RAG Answer (Gemini):
  "Gradient descent works by computing the gradient of the loss
   function and updating model weights in the opposite direction..."

Evaluation:
  10 queries tested | Average relevance: 4.2/5
```

---
