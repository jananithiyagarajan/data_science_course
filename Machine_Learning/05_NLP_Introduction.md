# Natural Language Processing (NLP)

## What is NLP?

**Natural Language Processing (NLP)** is a branch of AI that enables computers to understand, interpret, and generate human language.

It bridges the gap between **human communication** and **computer understanding**.

### Real-World Applications

| Application         | Examples                                      |
| ------------------- | --------------------------------------------- |
| Sentiment Analysis  | Product reviews, social media monitoring      |
| Machine Translation | Google Translate, DeepL                       |
| Chatbots            | Customer support, virtual assistants          |
| Text Classification | Spam detection, topic classification          |
| Named Entity Recognition | Extract names, places, dates from text |
| Question Answering  | Search engines, AI assistants                 |
| Text Summarization  | News summarization, document summarization    |
| Speech to Text      | Siri, Google Assistant                        |

---

## 1. NLP Pipeline

```
Raw Text
    ↓
Text Preprocessing (clean, tokenize, normalize)
    ↓
Feature Extraction (TF-IDF, word embeddings)
    ↓
Model (ML / Deep Learning)
    ↓
Prediction (sentiment, topic, entity, etc.)
```

---

## 2. Text Preprocessing

Before feeding text to a model, we need to clean and normalize it.

```python
import re
import nltk
from nltk.corpus import stopwords
from nltk.tokenize import word_tokenize
from nltk.stem import PorterStemmer, WordNetLemmatizer

nltk.download('punkt')
nltk.download('stopwords')
nltk.download('wordnet')

text = "Hello! This is a Sample Text. It's VERY important to clean the data properly!!!"

# 1. Lowercase
text = text.lower()
print(text)  # "hello! this is a sample text. it's very important to clean the data properly!!!"

# 2. Remove punctuation and special characters
text = re.sub(r'[^a-zA-Z\s]', '', text)
print(text)  # "hello this is a sample text its very important to clean the data properly"

# 3. Tokenization — split into words
tokens = word_tokenize(text)
print(tokens)  # ['hello', 'this', 'is', 'a', 'sample', ...]

# 4. Remove Stop Words
stop_words = set(stopwords.words('english'))
tokens = [word for word in tokens if word not in stop_words]
print(tokens)  # ['hello', 'sample', 'text', 'important', 'clean', 'data', 'properly']

# 5. Stemming — reduce to root form (fast but rough)
stemmer = PorterStemmer()
stemmed = [stemmer.stem(word) for word in tokens]
print(stemmed)  # ['hello', 'sampl', 'text', 'import', 'clean', 'data', 'properli']

# 6. Lemmatization — reduce to dictionary form (accurate)
lemmatizer = WordNetLemmatizer()
lemmatized = [lemmatizer.lemmatize(word) for word in tokens]
print(lemmatized)  # ['hello', 'sample', 'text', 'important', 'clean', 'data', 'properly']
```

---

## 3. Feature Extraction

Machine learning models need numbers, not text. We convert text to numerical features.

### 3.1 Bag of Words (BoW)

```python
from sklearn.feature_extraction.text import CountVectorizer

corpus = [
    "I love data science",
    "Data science is amazing",
    "I love programming"
]

vectorizer = CountVectorizer()
X = vectorizer.fit_transform(corpus)

print(vectorizer.get_feature_names_out())
print(X.toarray())
# [[0 0 0 1 1 0 0 1]
#  [1 1 0 1 0 0 0 0]
#  [0 0 0 0 1 1 1 0]]
```

### 3.2 TF-IDF (Term Frequency-Inverse Document Frequency)

TF-IDF gives higher weight to rare but important words.

```python
from sklearn.feature_extraction.text import TfidfVectorizer

corpus = [
    "The cat sat on the mat",
    "The dog barked at the cat",
    "The cat and dog are friends"
]

tfidf = TfidfVectorizer()
X = tfidf.fit_transform(corpus)

print(tfidf.get_feature_names_out())
print(X.toarray().round(3))
```

### 3.3 Word Embeddings

Word embeddings represent words as dense vectors where similar words are close together.

#### Word2Vec with Gensim

```python
from gensim.models import Word2Vec

sentences = [
    ["king", "queen", "man", "woman"],
    ["dog", "cat", "animal", "pet"],
    ["Python", "Java", "programming", "code"]
]

model = Word2Vec(sentences, vector_size=50, window=3, min_count=1, epochs=20)

# Find similar words
print(model.wv.most_similar("cat"))

# Get word vector
print(model.wv["king"])   # 50-dimensional vector

# Arithmetic: king - man + woman ≈ queen
result = model.wv.most_similar(positive=["king", "woman"], negative=["man"])
print(result)
```

---

## 4. Text Classification with ML

### Sentiment Analysis (Naive Bayes)

```python
from sklearn.naive_bayes import MultinomialNB
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score, classification_report

# Sample data
reviews = [
    "This product is amazing, I love it!",
    "Terrible quality, waste of money",
    "Great service and fast delivery",
    "Horrible experience, never again",
    "Excellent quality, highly recommend",
    "Very bad, I'm disappointed"
]
labels = [1, 0, 1, 0, 1, 0]   # 1=positive, 0=negative

# Vectorize
tfidf = TfidfVectorizer()
X = tfidf.fit_transform(reviews)

X_train, X_test, y_train, y_test = train_test_split(X, labels, test_size=0.3, random_state=42)

# Train
model = MultinomialNB()
model.fit(X_train, y_train)

y_pred = model.predict(X_test)
print(f"Accuracy: {accuracy_score(y_test, y_pred):.2f}")
print(classification_report(y_test, y_pred, target_names=["Negative", "Positive"]))

# Predict new text
new_text = ["This is the best product ever!"]
new_vec = tfidf.transform(new_text)
pred = model.predict(new_vec)
print("Sentiment:", "Positive" if pred[0] == 1 else "Negative")
```

---

## 5. NLP with spaCy

spaCy is a fast, production-grade NLP library.

```python
import spacy

nlp = spacy.load("en_core_web_sm")

text = "Apple is opening a new store in Chennai on 25th July 2025."
doc = nlp(text)

# Tokenization
for token in doc:
    print(token.text, token.pos_, token.dep_)

# Named Entity Recognition
for ent in doc.ents:
    print(ent.text, "→", ent.label_)
# Apple → ORG
# Chennai → GPE (Geo-Political Entity)
# 25th July 2025 → DATE

# Noun chunks
for chunk in doc.noun_chunks:
    print(chunk.text)

# Similarity (requires larger model)
doc1 = nlp("I like cats")
doc2 = nlp("I love dogs")
print(f"Similarity: {doc1.similarity(doc2):.3f}")
```

---

## 6. Advanced NLP: Transformers (BERT)

Modern NLP uses **Transformer** architecture for state-of-the-art performance.

```python
from transformers import pipeline

# Sentiment Analysis using BERT
classifier = pipeline("sentiment-analysis")
result = classifier("I absolutely love this course!")
print(result)  # [{'label': 'POSITIVE', 'score': 0.9998}]

# Named Entity Recognition
ner = pipeline("ner", grouped_entities=True)
text = "Google CEO Sundar Pichai announced new AI features in San Francisco."
entities = ner(text)
for ent in entities:
    print(f"{ent['word']:20s} → {ent['entity_group']} ({ent['score']:.2f})")

# Question Answering
qa = pipeline("question-answering")
context = "Python was created by Guido van Rossum and first released in 1991."
question = "Who created Python?"
result = qa(question=question, context=context)
print(result["answer"])  # Guido van Rossum

# Text Generation
generator = pipeline("text-generation", model="gpt2")
output = generator("Data Science is", max_length=50)
print(output[0]["generated_text"])

# Summarization
summarizer = pipeline("summarization")
article = """
Data science is an interdisciplinary field that uses scientific methods, 
processes, algorithms and systems to extract knowledge and insights from 
noisy, structured and unstructured data, and apply knowledge and insights 
from data across a broad range of application domains.
"""
summary = summarizer(article, max_length=30, min_length=10)
print(summary[0]["summary_text"])
```

---

## 🎯 Student Tasks – Module 22: NLP Introduction

### Task 1: Text Preprocessing Pipeline (Easy)
**Objective**: Build a complete text cleaning pipeline.

**Instructions**:
1. Take the paragraph: `"Machine Learning is an exciting field! It uses data to train models. Models can predict, classify & cluster. This is AMAZING!!! Data Science rocks."`
2. Apply in order:
   - Lowercase
   - Remove special characters and numbers
   - Tokenize
   - Remove stop words
   - Apply stemming AND lemmatization
3. Print the result after each step.
4. Count word frequency in the final cleaned text.

**Expected Output**:
```
Step 1 (lowercase): "machine learning is an exciting field..."
Step 2 (cleaned):   "machine learning is an exciting field..."
Step 3 (tokens):    ['machine', 'learning', 'is', 'an', ...]
Step 4 (no stops):  ['machine', 'learning', 'exciting', 'field', ...]
Step 5 (stemmed):   ['machin', 'learn', 'excit', ...]
Step 5 (lemma):     ['machine', 'learning', 'exciting', ...]

Word Frequency: {'machine': 1, 'learning': 2, 'exciting': 1, ...}
```

---

### Task 2: Sentiment Analysis on Product Reviews (Medium)
**Objective**: Build a sentiment classifier using TF-IDF + ML.

**Instructions**:
1. Create a dataset of 30+ product reviews with labels (positive=1, negative=0).
2. Preprocess: clean, tokenize, remove stop words.
3. Vectorize with TF-IDF.
4. Train: Naive Bayes, Logistic Regression, SVM — compare all 3.
5. Print accuracy, precision, recall, F1 for each.
6. Build a simple function `predict_sentiment(text)` using the best model.
7. Test with 5 new custom reviews.

**Expected Output**:
```
Model Comparison:
Naive Bayes:         Accuracy=0.84 | F1=0.83
Logistic Regression: Accuracy=0.89 | F1=0.88  ← Best
SVM:                 Accuracy=0.87 | F1=0.86

Testing new reviews:
"This product is fantastic!" → POSITIVE (0.96)
"Worst purchase ever!"       → NEGATIVE (0.98)
```

---

### Task 3: Named Entity Recognition & Text Summarization (Challenge)
**Objective**: Apply advanced NLP with spaCy and Hugging Face Transformers.

**Instructions**:
1. Load a news article text (copy any online article).
2. Use **spaCy** to extract:
   - All named entities (people, organizations, locations, dates)
   - Noun chunks
   - Sentence count
3. Use **Hugging Face** `pipeline("summarization")` to generate a 2-3 sentence summary.
4. Use **Hugging Face** `pipeline("sentiment-analysis")` on the article (split into sentences).
5. Print what % of sentences are positive/negative/neutral.
6. Use **Q&A pipeline** to answer 3 questions about the article.

**Expected Output**:
```
=== Named Entities ===
OpenAI          → ORG
Sam Altman      → PERSON
San Francisco   → GPE
2025            → DATE

=== Summary ===
"OpenAI released its latest model GPT-5, which..."

=== Sentiment ===
Positive: 65% | Negative: 20% | Neutral: 15%

=== Q&A ===
Q: Who founded OpenAI?
A: Sam Altman and Elon Musk
```

---
