# Streamlit — Build AI & Data Science Web Apps

## What is Streamlit?

**Streamlit** is an open-source Python framework for building beautiful, interactive web applications for data science and AI — all in **pure Python**. No HTML, CSS, or JavaScript required.

> "Turn your Python scripts into shareable web apps in minutes."

### Why Streamlit for Data Scientists?

| Feature         | Traditional Web Dev | Streamlit         |
| --------------- | ------------------- | ----------------- |
| Language        | HTML + CSS + JS     | Pure Python       |
| Learning curve  | High                | Very Low          |
| Deploy time     | Days/Weeks          | Minutes           |
| ML integration  | Complex             | Native            |
| Data display    | Manual              | Auto (DataFrames, plots) |

---

## Installation and Setup

```bash
pip install streamlit

# Run your app
streamlit run app.py

# App opens at http://localhost:8501
```

---

## 1. Basic Streamlit App

```python
# app.py
import streamlit as st
import pandas as pd
import numpy as np

# ── Page Config ────────────────────────────────────────────────────────────────
st.set_page_config(
    page_title="My Data App",
    page_icon="📊",
    layout="wide",
    initial_sidebar_state="expanded"
)

# ── Title & Header ─────────────────────────────────────────────────────────────
st.title("📊 Data Science Dashboard")
st.markdown("Welcome to the **Jeevi Academy** interactive data platform.")
st.divider()

# ── Text Elements ──────────────────────────────────────────────────────────────
st.header("1. Text Elements")
st.subheader("Subheader")
st.write("This is normal text with **bold** and *italic* support.")
st.markdown("## Markdown works too!\n- Item 1\n- Item 2")
st.code("import pandas as pd\ndf = pd.read_csv('data.csv')", language='python')
st.latex(r"E = mc^2")

# ── Data Display ───────────────────────────────────────────────────────────────
st.header("2. Data Display")
df = pd.DataFrame({
    'Name': ['Arun', 'Priya', 'Kumar', 'Meena'],
    'Score': [88, 92, 75, 96],
    'Grade': ['B', 'A', 'C', 'A+']
})
st.dataframe(df, use_container_width=True)     # Interactive table
st.table(df)                                    # Static table
st.metric("Average Score", "87.75", delta="+2.5 from last month")

# ── Charts ────────────────────────────────────────────────────────────────────
st.header("3. Charts")
data = pd.DataFrame(np.random.randn(20, 3), columns=['Python', 'SQL', 'ML'])
st.line_chart(data)
st.bar_chart(data)
st.area_chart(data)

# Run: streamlit run app.py
```

---

## 2. Input Widgets

```python
import streamlit as st

st.header("Interactive Widgets")

# ── Sidebar ────────────────────────────────────────────────────────────────────
with st.sidebar:
    st.header("⚙️ Settings")
    name     = st.text_input("Your Name", value="Student")
    age      = st.slider("Your Age", min_value=10, max_value=80, value=25)
    language = st.selectbox("Favourite Language", ["Python", "SQL", "Java", "R"])
    skills   = st.multiselect("Your Skills",
                              ["ML", "DL", "NLP", "SQL", "Visualization", "GenAI"])
    learning = st.radio("Learning Mode", ["Online", "Offline", "Hybrid"])
    subscribe = st.checkbox("Subscribe to updates", value=True)

# ── Main Area Widgets ──────────────────────────────────────────────────────────
col1, col2 = st.columns(2)

with col1:
    score    = st.number_input("Enter your test score", 0, 100, 75)
    date     = st.date_input("Select exam date")
    uploaded = st.file_uploader("Upload CSV file", type=['csv'])
    color    = st.color_picker("Pick a theme color", "#1f77b4")

with col2:
    st.write(f"**Hello, {name}!** You are {age} years old.")
    st.write(f"**Language:** {language}")
    st.write(f"**Skills:** {', '.join(skills) if skills else 'None selected'}")
    st.write(f"**Score:** {score}/100 → {'Pass ✅' if score >= 50 else 'Fail ❌'}")

# ── Button Actions ─────────────────────────────────────────────────────────────
if st.button("🚀 Submit", type='primary'):
    st.success(f"Submitted! Name: {name}, Score: {score}")
    st.balloons()

if uploaded:
    import pandas as pd
    df = pd.read_csv(uploaded)
    st.write(f"File uploaded: {df.shape[0]} rows × {df.shape[1]} columns")
    st.dataframe(df.head(10))
```

---

## 3. ML Model Dashboard

```python
import streamlit as st
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import classification_report, confusion_matrix
import joblib

st.set_page_config(page_title="ML Classifier", page_icon="🤖", layout="wide")
st.title("🤖 Iris Flower Classifier")

# ── Sidebar Controls ───────────────────────────────────────────────────────────
with st.sidebar:
    st.header("Model Settings")
    n_estimators = st.slider("Number of Trees", 10, 200, 100, 10)
    max_depth    = st.slider("Max Depth", 1, 20, 5)
    test_size    = st.slider("Test Size", 0.1, 0.4, 0.2, 0.05)
    train_button = st.button("🚀 Train Model", type='primary')

# ── Load Data ──────────────────────────────────────────────────────────────────
iris = load_iris()
df   = pd.DataFrame(iris.data, columns=iris.feature_names)
df['target'] = iris.target
df['species'] = df['target'].map({0: 'Setosa', 1: 'Versicolor', 2: 'Virginica'})

tab1, tab2, tab3 = st.tabs(["📊 Data", "🤖 Model", "🔮 Predict"])

with tab1:
    st.subheader("Dataset Overview")
    col1, col2, col3 = st.columns(3)
    col1.metric("Rows",    df.shape[0])
    col2.metric("Columns", df.shape[1])
    col3.metric("Classes", 3)

    st.dataframe(df.head(10), use_container_width=True)

    # Pairplot
    st.subheader("Feature Distributions")
    fig, axes = plt.subplots(1, 2, figsize=(14, 5))
    for i, feature in enumerate(['sepal length (cm)', 'petal length (cm)']):
        for species in df['species'].unique():
            subset = df[df['species'] == species][feature]
            axes[i].hist(subset, bins=15, alpha=0.6, label=species)
        axes[i].set_title(f'{feature} Distribution')
        axes[i].legend()
    st.pyplot(fig)

with tab2:
    if train_button:
        with st.spinner("Training model..."):
            X = iris.data
            y = iris.target
            X_train, X_test, y_train, y_test = train_test_split(
                X, y, test_size=test_size, random_state=42)

            model = RandomForestClassifier(
                n_estimators=n_estimators, max_depth=max_depth, random_state=42)
            model.fit(X_train, y_train)
            y_pred = model.predict(X_test)
            acc = (y_pred == y_test).mean()

            # Save model in session state
            st.session_state['model'] = model
            st.session_state['accuracy'] = acc

        st.success(f"✅ Model trained! Accuracy: {acc:.4f} ({acc*100:.2f}%)")

        col1, col2 = st.columns(2)
        with col1:
            # Confusion Matrix
            cm = confusion_matrix(y_test, y_pred)
            fig, ax = plt.subplots(figsize=(6, 4))
            sns.heatmap(cm, annot=True, fmt='d', cmap='Blues', ax=ax,
                        xticklabels=iris.target_names,
                        yticklabels=iris.target_names)
            ax.set_title('Confusion Matrix')
            ax.set_xlabel('Predicted')
            ax.set_ylabel('Actual')
            st.pyplot(fig)

        with col2:
            # Feature Importance
            importance = pd.DataFrame({
                'Feature': iris.feature_names,
                'Importance': model.feature_importances_
            }).sort_values('Importance', ascending=True)
            fig, ax = plt.subplots(figsize=(6, 4))
            ax.barh(importance['Feature'], importance['Importance'],
                    color='steelblue', edgecolor='white')
            ax.set_title('Feature Importance')
            st.pyplot(fig)
    else:
        st.info("👈 Configure settings and click 'Train Model' in the sidebar.")

with tab3:
    st.subheader("Make a Prediction")
    if 'model' not in st.session_state:
        st.warning("⚠️ Please train the model first (Tab 2).")
    else:
        col1, col2 = st.columns(2)
        with col1:
            sepal_len = st.slider("Sepal Length (cm)", 4.0, 8.0, 5.5, 0.1)
            sepal_wid = st.slider("Sepal Width (cm)", 2.0, 4.5, 3.0, 0.1)
        with col2:
            petal_len = st.slider("Petal Length (cm)", 1.0, 7.0, 3.5, 0.1)
            petal_wid = st.slider("Petal Width (cm)", 0.1, 2.5, 1.2, 0.1)

        if st.button("🔮 Predict Species"):
            features = np.array([[sepal_len, sepal_wid, petal_len, petal_wid]])
            pred  = st.session_state['model'].predict(features)[0]
            proba = st.session_state['model'].predict_proba(features)[0]
            species = iris.target_names[pred]

            st.markdown(f"## 🌸 Predicted: **{species}**")
            prob_df = pd.DataFrame({
                'Species': iris.target_names,
                'Probability': proba
            })
            st.bar_chart(prob_df.set_index('Species'))
```

---

## 4. LLM Chatbot with Streamlit

```python
import streamlit as st
import google.generativeai as genai

st.set_page_config(page_title="AI Tutor", page_icon="🤖")
st.title("🤖 DataBot — AI Data Science Tutor")

# Configure API
genai.configure(api_key=st.secrets.get("GEMINI_API_KEY", "your-key-here"))
model = genai.GenerativeModel(
    model_name="gemini-1.5-flash",
    system_instruction="You are DataBot, a friendly data science tutor. "
                       "Explain concepts clearly with examples and code snippets."
)

# ── Session State for Chat History ────────────────────────────────────────────
if "messages" not in st.session_state:
    st.session_state.messages = []
if "chat" not in st.session_state:
    st.session_state.chat = model.start_chat(history=[])

# ── Sidebar ────────────────────────────────────────────────────────────────────
with st.sidebar:
    st.header("💬 Chat Options")
    if st.button("🗑️ Clear Chat"):
        st.session_state.messages = []
        st.session_state.chat = model.start_chat(history=[])
        st.rerun()

    st.markdown("### Quick Topics")
    for topic in ["What is ML?", "Explain pandas", "Write a CNN", "What is RAG?"]:
        if st.button(topic):
            st.session_state.quick_topic = topic

# ── Display Chat History ───────────────────────────────────────────────────────
for msg in st.session_state.messages:
    with st.chat_message(msg["role"]):
        st.markdown(msg["content"])

# ── Chat Input ────────────────────────────────────────────────────────────────
prompt = st.chat_input("Ask me anything about data science...")

if hasattr(st.session_state, 'quick_topic'):
    prompt = st.session_state.quick_topic
    del st.session_state.quick_topic

if prompt:
    # Add user message
    st.session_state.messages.append({"role": "user", "content": prompt})
    with st.chat_message("user"):
        st.markdown(prompt)

    # Get AI response
    with st.chat_message("assistant"):
        with st.spinner("Thinking..."):
            response = st.session_state.chat.send_message(prompt)
        st.markdown(response.text)
        st.session_state.messages.append({"role": "assistant", "content": response.text})
```

---

## 5. Deploying to Streamlit Cloud

```bash
# 1. Push your app to GitHub
git init
git add app.py requirements.txt
git commit -m "Initial Streamlit app"
git push origin main

# 2. Go to https://share.streamlit.io
#    → Sign in with GitHub
#    → New app → Select repo → Branch: main → File: app.py
#    → Deploy!

# requirements.txt
streamlit>=1.30.0
pandas>=2.0.0
numpy>=1.24.0
matplotlib>=3.7.0
seaborn>=0.12.0
scikit-learn>=1.3.0
google-generativeai>=0.5.0
```

---

## 🎯 Student Tasks – Streamlit Module

### Task 1: Your First Streamlit App (Easy)
**Objective**: Build an interactive data explorer.

**Instructions**:
Create `data_explorer.py`:
1. Add a file uploader that accepts CSV files.
2. When file is uploaded, show:
   - Shape (rows × columns)
   - Column names and data types
   - First 10 rows (interactive table)
   - Basic statistics (describe())
   - Missing values count per column
3. Add a sidebar with:
   - Column selector (selectbox) — on change, show that column's distribution chart
   - Numeric column: histogram with KDE
   - Categorical column: bar chart of value counts
4. Add a "Download Cleaned Data" button that removes NaN rows and allows download.

**Expected App UI**:
```
📊 CSV Data Explorer

[Upload CSV file]
  ├── Shape: 891 rows × 12 columns
  ├── Interactive table (first 10 rows)
  ├── Statistics table
  └── Missing values: age=177, cabin=687

Sidebar: Select Column → [Age ▼]
  → Histogram of Age distribution

[Download Cleaned Data] button
```

---

### Task 2: ML Model Dashboard (Medium)
**Objective**: Build a complete interactive ML training dashboard.

**Instructions**:
Create `ml_dashboard.py`:
1. **Dataset Tab**: Let user upload CSV or select from preloaded datasets (Iris, Titanic, Housing).
2. **EDA Tab**: Auto-generate 4 visualizations:
   - Correlation heatmap
   - Target distribution
   - Missing values heatmap
   - Feature histograms (first 4 numeric features)
3. **Model Tab**:
   - Sidebar: select algorithm (Decision Tree, Random Forest, Logistic Regression)
   - Sidebar: tune 2 hyperparameters per algorithm using sliders
   - Train and show: accuracy, confusion matrix, classification report
4. **Predict Tab**: Input sliders for each feature → show prediction + confidence bar chart.
5. **Export Tab**: Download trained model as `.pkl` file.

---

### Task 3: AI-Powered Streamlit App (Challenge)
**Objective**: Build a production-quality AI application.

**Instructions**:
Build one of the following (choose your interest):

**Option A — Data Analysis Chatbot**:
- Users upload a CSV file
- An AI (Gemini/GPT) answers questions about the data
- The AI can suggest and generate visualizations
- Shows chat history in sidebar
- Includes a "Generate Report" button that creates a PDF summary

**Option B — Resume Analyzer**:
- Upload resume PDF
- AI extracts: skills, experience, education
- Matches against a job description (paste or upload)
- Shows match score (%), missing skills, and improvement suggestions
- "Generate Cover Letter" button using AI

**Option C — Document Q&A (RAG)**:
- Upload 1–5 PDF documents
- System chunks and embeds them into ChromaDB
- Users ask questions — AI answers from documents
- Highlights source paragraph with similarity score
- Chat history preserved in session

Requirements for all options:
- Clean, professional UI with page config
- Loading spinners for API calls
- Error handling (empty input, API failures)
- Deployed to Streamlit Cloud (public URL)

**Expected Output**:
```
Option A — Data Analysis Chatbot:

App URL: https://your-app.streamlit.app

Features:
  ✅ CSV upload with preview
  ✅ Chat interface (Gemini-powered)
  ✅ Dynamic chart generation from AI suggestions
  ✅ PDF report download
  
Sample interaction:
  User: "Which column has the most missing values?"
  AI:   "The 'cabin' column has 687 missing values (77%)..."
  
  User: "Show me a chart of survival by gender"
  AI:   [Generates and displays chart]
```

---
