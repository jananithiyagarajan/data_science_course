# API & FastAPI for Data Science & AI Applications

## What is an API?

An **API (Application Programming Interface)** is a set of rules that allows different software applications to communicate with each other.

> Think of an API like a waiter in a restaurant — you (client) tell the waiter (API) what you want, and the waiter brings it from the kitchen (server/database) back to you.

### Types of APIs

| Type       | Description                                    | Example                    |
| ---------- | ---------------------------------------------- | -------------------------- |
| **REST**   | HTTP-based, uses JSON, most common             | Twitter API, OpenAI API    |
| **GraphQL**| Query exactly what you need                    | GitHub GraphQL API         |
| **gRPC**   | Fast binary protocol, used in microservices    | Google internal services   |
| **WebSocket**| Real-time bidirectional communication        | Chat apps, live dashboards |

---

## REST API Basics

### HTTP Methods

| Method   | Purpose             | Example                             |
| -------- | ------------------- | ----------------------------------- |
| **GET**  | Retrieve data       | Get a list of users                 |
| **POST** | Create new resource | Register a new user                 |
| **PUT**  | Update entire resource | Update user profile               |
| **PATCH**| Update partial resource | Change only user email           |
| **DELETE**| Remove resource    | Delete a user                       |

### HTTP Status Codes

| Code | Meaning                         |
| ---- | ------------------------------- |
| 200  | OK — Success                    |
| 201  | Created — Resource created      |
| 400  | Bad Request — Invalid input     |
| 401  | Unauthorized — Need to log in   |
| 403  | Forbidden — No permission       |
| 404  | Not Found — Resource not found  |
| 422  | Unprocessable — Validation error|
| 500  | Internal Server Error           |

---

## Introduction to FastAPI

**FastAPI** is a modern, high-performance Python web framework for building APIs. It is:
- ⚡ **Fast**: On par with NodeJS and Go (based on Starlette + Pydantic)
- 🔧 **Easy**: Automatic OpenAPI/Swagger docs
- ✅ **Type-safe**: Pydantic models for validation
- 📦 **Production-ready**: Used by Microsoft, Uber, Netflix

### Installation

```bash
pip install fastapi uvicorn[standard]

# Run the server
uvicorn main:app --reload
```

---

## Your First FastAPI App

```python
# main.py
from fastapi import FastAPI
from pydantic import BaseModel
from typing import Optional

app = FastAPI(
    title="Data Science API",
    description="A sample API for data science operations",
    version="1.0.0"
)

# ── Root Endpoint ──────────────────────────────────────────────────────────────
@app.get("/")
def root():
    return {"message": "Welcome to the Data Science API!", "status": "healthy"}

# ── GET with Path Parameter ───────────────────────────────────────────────────
@app.get("/users/{user_id}")
def get_user(user_id: int):
    return {"user_id": user_id, "name": f"User_{user_id}"}

# ── GET with Query Parameters ─────────────────────────────────────────────────
@app.get("/items/")
def get_items(skip: int = 0, limit: int = 10, search: Optional[str] = None):
    items = [f"item_{i}" for i in range(100)]
    if search:
        items = [item for item in items if search in item]
    return items[skip : skip + limit]
```

Visit `http://localhost:8000/docs` for auto-generated Swagger UI!

---

## Request Body with Pydantic Models

```python
from fastapi import FastAPI, HTTPException
from pydantic import BaseModel, Field, validator
from typing import Optional, List
from datetime import datetime

app = FastAPI()

# ── Define Data Models ─────────────────────────────────────────────────────────
class Student(BaseModel):
    name: str = Field(..., min_length=2, max_length=50)
    age: int = Field(..., ge=5, le=100)
    email: str
    score: float = Field(..., ge=0, le=100)
    grade: Optional[str] = None

    @validator('grade', always=True, pre=True)
    def calculate_grade(cls, v, values):
        score = values.get('score', 0)
        if score >= 90: return 'A+'
        elif score >= 80: return 'A'
        elif score >= 70: return 'B'
        elif score >= 60: return 'C'
        else: return 'F'

class StudentResponse(Student):
    id: int
    created_at: datetime

# ── In-memory Database ─────────────────────────────────────────────────────────
db: List[dict] = []
student_counter = 0

# ── CRUD Endpoints ─────────────────────────────────────────────────────────────
@app.post("/students/", response_model=StudentResponse, status_code=201)
def create_student(student: Student):
    global student_counter
    student_counter += 1
    student_dict = student.dict()
    student_dict["id"] = student_counter
    student_dict["created_at"] = datetime.now()
    db.append(student_dict)
    return student_dict

@app.get("/students/", response_model=List[StudentResponse])
def get_all_students():
    return db

@app.get("/students/{student_id}", response_model=StudentResponse)
def get_student(student_id: int):
    for student in db:
        if student["id"] == student_id:
            return student
    raise HTTPException(status_code=404, detail=f"Student {student_id} not found")

@app.put("/students/{student_id}", response_model=StudentResponse)
def update_student(student_id: int, updated: Student):
    for i, student in enumerate(db):
        if student["id"] == student_id:
            db[i].update(updated.dict())
            return db[i]
    raise HTTPException(status_code=404, detail="Student not found")

@app.delete("/students/{student_id}")
def delete_student(student_id: int):
    for i, student in enumerate(db):
        if student["id"] == student_id:
            db.pop(i)
            return {"message": f"Student {student_id} deleted"}
    raise HTTPException(status_code=404, detail="Student not found")
```

---

## Machine Learning Model Serving with FastAPI

```python
from fastapi import FastAPI, HTTPException
from pydantic import BaseModel
import numpy as np
import joblib
import pandas as pd

app = FastAPI(title="ML Prediction API", version="1.0.0")

# ── Load Model on Startup ─────────────────────────────────────────────────────
@app.on_event("startup")
def load_model():
    global model, scaler
    model  = joblib.load("model.pkl")
    scaler = joblib.load("scaler.pkl")
    print("Model loaded successfully!")

# ── Request/Response Schemas ──────────────────────────────────────────────────
class HouseFeatures(BaseModel):
    sqft: float
    bedrooms: int
    bathrooms: int
    age: int
    location_score: float

class PredictionResponse(BaseModel):
    predicted_price: float
    confidence_range: dict
    model_version: str = "v1.0"

# ── Prediction Endpoint ───────────────────────────────────────────────────────
@app.post("/predict/price", response_model=PredictionResponse)
def predict_price(features: HouseFeatures):
    try:
        # Prepare input
        X = pd.DataFrame([features.dict()])
        X_scaled = scaler.transform(X)

        # Predict
        prediction = model.predict(X_scaled)[0]

        return PredictionResponse(
            predicted_price=round(float(prediction), 2),
            confidence_range={
                "lower": round(float(prediction * 0.9), 2),
                "upper": round(float(prediction * 1.1), 2)
            }
        )
    except Exception as e:
        raise HTTPException(status_code=500, detail=str(e))

# ── Model Info ────────────────────────────────────────────────────────────────
@app.get("/model/info")
def model_info():
    return {
        "model_type": type(model).__name__,
        "features": ["sqft", "bedrooms", "bathrooms", "age", "location_score"],
        "target": "house_price",
        "version": "1.0.0"
    }
```

---

## LLM API Endpoint

```python
from fastapi import FastAPI
from pydantic import BaseModel
import google.generativeai as genai

genai.configure(api_key="YOUR_GEMINI_API_KEY")
llm = genai.GenerativeModel("gemini-1.5-flash")

app = FastAPI(title="LLM API")

class ChatRequest(BaseModel):
    message: str
    system_prompt: str = "You are a helpful assistant."
    temperature: float = 0.7

class ChatResponse(BaseModel):
    response: str
    tokens_used: int

@app.post("/chat/", response_model=ChatResponse)
def chat(request: ChatRequest):
    model = genai.GenerativeModel(
        model_name="gemini-1.5-flash",
        system_instruction=request.system_prompt
    )
    response = model.generate_content(request.message)
    return ChatResponse(
        response=response.text,
        tokens_used=response.usage_metadata.total_token_count
    )
```

---

## Authentication with API Keys

```python
from fastapi import FastAPI, Security, HTTPException, status
from fastapi.security import APIKeyHeader

app = FastAPI()

API_KEY_HEADER = APIKeyHeader(name="X-API-Key")
VALID_API_KEYS = {"key-123-secret", "key-456-admin"}

def verify_api_key(api_key: str = Security(API_KEY_HEADER)):
    if api_key not in VALID_API_KEYS:
        raise HTTPException(
            status_code=status.HTTP_403_FORBIDDEN,
            detail="Invalid API Key"
        )
    return api_key

@app.get("/protected/data")
def protected_endpoint(api_key: str = Security(verify_api_key)):
    return {"message": "You have access!", "key": api_key[:6] + "***"}
```

---

## Calling External APIs (Requests Library)

```python
import requests
import json

# ── Calling a REST API ────────────────────────────────────────────────────────
response = requests.get(
    "https://api.example.com/data",
    params={"limit": 10, "offset": 0},
    headers={"Authorization": "Bearer YOUR_TOKEN"},
    timeout=10
)

if response.status_code == 200:
    data = response.json()
    print(data)
else:
    print(f"Error: {response.status_code} — {response.text}")

# ── POST Request ──────────────────────────────────────────────────────────────
payload = {
    "name": "Arun",
    "email": "arun@example.com",
    "score": 95.5
}

response = requests.post(
    "https://api.example.com/students",
    json=payload,
    headers={"Content-Type": "application/json"}
)

print(f"Status: {response.status_code}")
print(f"Response: {response.json()}")
```

---

## Deploying FastAPI

### Docker Deployment

```dockerfile
# Dockerfile
FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 8000
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```

```bash
# Build and run
docker build -t my-fastapi-app .
docker run -p 8000:8000 my-fastapi-app
```

---

## 🎯 Student Tasks – API & FastAPI Module

### Task 1: Basic REST API (Easy)
**Objective**: Build your first FastAPI application.

**Instructions**:
1. Install FastAPI and Uvicorn.
2. Create a `main.py` with a `Product` data model:
   - `product_id`, `name`, `price` (>0), `category`, `in_stock` (bool)
3. Implement CRUD:
   - `GET /products/` — list all (with `?category=filter` support)
   - `GET /products/{id}` — get one
   - `POST /products/` — create
   - `PUT /products/{id}` — update
   - `DELETE /products/{id}` — delete
4. Add a `GET /products/stats/` endpoint returning count per category and average price.
5. Test all endpoints via Swagger UI at `/docs`.
6. Write a `test_api.py` using `requests` that tests all 5+ endpoints.

**Expected Output**:
```
Server running at http://localhost:8000

Swagger UI: http://localhost:8000/docs

Test Results:
  POST /products/ → 201 Created ✓
  GET  /products/ → 200 OK, 5 products ✓
  GET  /products/1 → 200 OK ✓
  PUT  /products/1 → 200 OK, price updated ✓
  DELETE /products/3 → 200 OK ✓
  GET  /products/?category=Electronics → 200 OK, 2 items ✓
  GET  /products/stats/ → 200 OK ✓
```

---

### Task 2: ML Model API (Medium)
**Objective**: Serve a machine learning model through an API.

**Instructions**:
1. Train a model (e.g., titanic survival prediction or house price).
2. Save model and scaler using `joblib`.
3. Build FastAPI app:
   - `POST /predict` — accepts features, returns prediction + confidence
   - `GET /health` — returns model status and version
   - `GET /model/info` — returns features list and model type
   - `POST /predict/batch` — accepts list of inputs, returns list of predictions
4. Add input validation with Pydantic.
5. Add error handling (invalid inputs, model not loaded).
6. Test with valid and invalid inputs.

**Expected Output**:
```
GET /health:
  {"status": "healthy", "model": "RandomForestClassifier", "version": "1.0"}

POST /predict:
  Input: {"pclass": 1, "age": 35, "fare": 100, "sex": "female"}
  Output: {"prediction": 1, "label": "Survived", "probability": 0.91}

POST /predict (invalid):
  Input: {"pclass": 99}  ← missing required fields
  Output: 422 Unprocessable Entity
```

---

### Task 3: AI-Powered API with Authentication (Challenge)
**Objective**: Build a secure, production-quality AI API.

**Instructions**:
1. Build a FastAPI app with:
   - **Authentication**: API key header (`X-API-Key`) with tiered access (free/premium)
   - **Rate Limiting**: Free tier = 10 requests/day, Premium = unlimited
   - **Endpoints**:
     - `POST /ai/summarize` — summarize text using Gemini/GPT
     - `POST /ai/classify` — classify text sentiment
     - `POST /ai/translate` — translate to specified language
     - `GET /usage` — return current user's API usage stats
2. **Database**: Store API keys and usage in SQLite using SQLAlchemy.
3. **Logging**: Log all requests to `api.log` with timestamp, endpoint, user, tokens used.
4. **Background Tasks**: After each request, update usage stats asynchronously.
5. Deploy with Docker.

**Expected Output**:
```
POST /ai/summarize
Headers: {"X-API-Key": "key-abc-free"}

Input: {"text": "...500 word article...", "max_length": 100}
Output: {
  "summary": "This article discusses...",
  "tokens_used": 187,
  "tier": "free",
  "requests_remaining_today": 7
}

GET /usage
Output: {
  "tier": "free",
  "today_requests": 3,
  "daily_limit": 10,
  "total_tokens_used": 512
}

Denied request (rate limited):
  {"error": "Daily limit reached. Upgrade to Premium for unlimited access."}
```

---
