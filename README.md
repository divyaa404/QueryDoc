# 📄 QueryDoc — RAG-Based Document Question Answering System

> **Ask your documents. Get grounded answers.**

QueryDoc is a **Retrieval-Augmented Generation (RAG)** based document question-answering system that allows users to upload a PDF and ask questions about its content.

Instead of relying only on the knowledge stored inside an LLM, QueryDoc retrieves the most relevant information from the uploaded document and provides it to the LLM as context before generating an answer.

The goal of this project is to understand and implement the **complete RAG pipeline from scratch**.

---

## 🚀 Overview

Large Language Models can answer questions about general knowledge, but they may not know the contents of a user's private documents.

QueryDoc solves this problem by combining:

* 📑 Document processing
* ✂️ Text chunking
* 🧠 Text embeddings
* 🔎 Semantic similarity search
* 🗄️ Vector storage
* 🤖 Large Language Models
* 📚 Source/page references

### Basic Workflow

```text
                PDF DOCUMENT
                     │
                     ▼
              Text Extraction
                     │
                     ▼
                 Chunking
                     │
                     ▼
                Embeddings
                     │
                     ▼
              Vector Database
                   (FAISS)
                     │
                     │
                     ▼
              User Question
                     │
                     ▼
             Question Embedding
                     │
                     ▼
             Similarity Search
                     │
                     ▼
           Relevant Document Chunks
                     │
                     ▼
             Context + Question
                     │
                     ▼
                   Gemini
                     │
                     ▼
              Generated Answer
                     │
                     ▼
               📚 Source Pages
```

---

# ✨ Features

### 📤 PDF Upload

Upload a PDF document directly through the application.

### 📖 PDF Text Extraction

Extract text from individual PDF pages while preserving page information.

### ✂️ Intelligent Text Chunking

Large documents are divided into smaller chunks so that relevant information can be efficiently retrieved.

### 🧠 Semantic Embeddings

Text chunks are converted into numerical vector representations using an embedding model.

### 🔎 Semantic Search

QueryDoc searches the vector database to find the chunks most relevant to the user's question.

### 🗄️ Vector Storage

FAISS is used for efficient similarity search over document embeddings.

### 🤖 AI-Powered Answers

The retrieved document context is passed to a Large Language Model to generate a natural-language answer.

### 🛡️ Grounded Responses

The system is instructed to answer using the retrieved document context instead of relying on unrelated information.

### 📚 Source References

Relevant page numbers are displayed with answers so users can identify where the information came from.

---

# 🧩 Technology Stack

| Component            | Technology            |
| -------------------- | --------------------- |
| Programming Language | Python                |
| User Interface       | Streamlit             |
| PDF Processing       | PyPDF                 |
| Embeddings           | Sentence Transformers |
| Vector Database      | FAISS                 |
| LLM                  | Google Gemini API     |
| Version Control      | Git & GitHub          |

---

# 🏗️ Project Architecture

```text
QueryDoc
│
├── app.py
│   └── Streamlit user interface
│
├── pdf_processor.py
│   ├── PDF loading
│   ├── Text extraction
│   └── Text cleaning
│
├── embeddings.py
│   └── Generate embeddings for text
│
├── vector_store.py
│   ├── Store vectors
│   └── Similarity search using FAISS
│
├── rag.py
│   ├── Retrieve relevant chunks
│   ├── Build context
│   ├── Call Gemini
│   └── Generate grounded response
│
├── data/
│   └── Uploaded documents
│
├── requirements.txt
└── README.md
```

---

# 🔄 RAG Pipeline

QueryDoc follows a standard Retrieval-Augmented Generation architecture.

## 1. Document Loading

The user uploads a PDF.

```text
PDF
 ↓
PyPDF
 ↓
Extracted text
```

Each page is processed separately so that page numbers can later be used as sources.

---

## 2. Text Chunking

The extracted text is divided into smaller sections.

```text
Large Document
      ↓
 ┌──────────┐
 │ Chunk 1  │
 ├──────────┤
 │ Chunk 2  │
 ├──────────┤
 │ Chunk 3  │
 ├──────────┤
 │   ...    │
 └──────────┘
```

Each chunk contains metadata such as:

```text
chunk_id
page_number
text
```

This allows QueryDoc to retrieve information while maintaining its original location.

---

## 3. Embedding Generation

Every chunk is converted into a vector representation.

```text
Text Chunk
    ↓
Embedding Model
    ↓
Numerical Vector
```

Example:

```text
"Deadlock occurs when processes wait indefinitely..."

        ↓

[0.12, -0.42, 0.81, 0.19, ...]
```

These vectors capture the semantic meaning of the text.

---

## 4. Vector Storage

The generated embeddings are stored in a FAISS index.

```text
Chunk 1 → Vector 1
Chunk 2 → Vector 2
Chunk 3 → Vector 3
...
Chunk N → Vector N
```

FAISS allows QueryDoc to efficiently search for vectors that are similar to a user's question.

---

## 5. Query Processing

When a user asks a question:

```text
User Question
      ↓
Embedding Model
      ↓
Question Vector
```

The question vector is compared with the stored document vectors.

---

## 6. Retrieval

QueryDoc retrieves the most relevant chunks.

For example:

```text
Question:
"What are the necessary conditions for deadlock?"

Retrieved:

Page 12 → Chunk 35
Page 13 → Chunk 36
Page 14 → Chunk 41
```

These chunks become the context for the LLM.

---

## 7. Context Augmentation

The retrieved information and user question are combined:

```text
Retrieved Context
       +
User Question
       ↓
Prompt
```

The prompt instructs the LLM to answer using the retrieved document context.

---

## 8. Answer Generation

The final prompt is sent to Gemini.

```text
Context + Question
        ↓
      Gemini
        ↓
     Answer
```

The answer is then displayed to the user along with relevant source pages.

---

# 🎯 Example

### Uploaded document

```text
Operating Systems Notes.pdf
```

### User question

```text
What are the four necessary conditions for deadlock?
```

### QueryDoc

```text
1. Converts the question into an embedding
2. Searches FAISS
3. Retrieves relevant chunks
4. Sends the chunks + question to Gemini
5. Generates the answer
6. Displays source pages
```

### Output

```text
The four necessary conditions for deadlock are:

1. Mutual Exclusion
2. Hold and Wait
3. No Preemption
4. Circular Wait

Sources:
📄 Page 12
📄 Page 13
```

---

# 🛡️ Handling Unknown Questions

QueryDoc is designed to avoid answering questions that cannot be supported by the uploaded document.

For example:

```text
User:
Who invented the telephone?
```

If the information is not available in the uploaded document:

```text
I couldn't find this information in the uploaded document.
```

This helps reduce unsupported or hallucinated answers.

---

# 💻 Installation

## 1. Clone the Repository

```bash
git clone https://github.com/YOUR-USERNAME/QueryDoc.git
```

```bash
cd QueryDoc
```

---

## 2. Create a Virtual Environment

### Windows

```bash
python -m venv venv
```

Activate it:

```bash
venv\Scripts\activate
```

### macOS / Linux

```bash
python3 -m venv venv
```

```bash
source venv/bin/activate
```

---

## 3. Install Dependencies

```bash
pip install -r requirements.txt
```

---

# 🔑 API Configuration

QueryDoc uses the Google Gemini API for answer generation.

Create an API key and store it as an environment variable.

For local development, you can use a `.env` file:

```text
GEMINI_API_KEY=your_api_key_here
```

⚠️ **Never commit your API key to GitHub.**

Add the following to `.gitignore`:

```text
.env
venv/
__pycache__/
*.pyc
```

---

# ▶️ Run the Application

Start the Streamlit application:

```bash
streamlit run app.py
```

The application will open in your browser.

---

# 📂 Example Usage

### Step 1

Upload a PDF.

```text
📄 Upload Document
[ Select PDF ]
```

### Step 2

Process the document.

```text
[ Process Document ]
```

### Step 3

Ask a question.

```text
What is normalization in DBMS?
```

### Step 4

QueryDoc retrieves relevant content.

### Step 5

Gemini generates a grounded answer.

### Step 6

Relevant source pages are displayed.

---

# 📊 Current Project Scope

The initial version focuses on:

* Single PDF processing
* Text extraction
* Text chunking
* Embedding generation
* FAISS vector search
* Question answering
* Source/page references
* Basic hallucination prevention

---

# 🔮 Future Improvements

The project can be extended into a more advanced RAG system.

### 📚 Multiple Documents

Allow users to upload and search across multiple PDFs.

### 🏷️ Metadata Filtering

Filter documents based on:

* Subject
* Topic
* Date
* Document type
* Page

### 🔎 Hybrid Search

Combine:

```text
Keyword Search
       +
Vector Search
       ↓
Better Retrieval
```

### 🎯 Reranking

Retrieve a larger number of chunks and use a reranker to select the most relevant context.

### 🧠 Query Rewriting

Rewrite ambiguous questions before retrieval.

### 💬 Conversation Memory

Allow users to ask follow-up questions while maintaining conversation context.

### 📊 RAG Evaluation

Evaluate:

* Retrieval accuracy
* Context relevance
* Answer faithfulness
* Answer quality

### 🖼️ Multimodal RAG

Support:

* Images
* Tables
* Charts
* Scanned PDFs

### ☁️ Deployment

Deploy QueryDoc as a publicly accessible web application.

---

# 🧪 Learning Objectives

This project is primarily designed as a hands-on learning project for understanding RAG.

Through QueryDoc, the following concepts are explored:

```text
LLM
 ↓
Prompt Engineering
 ↓
Embeddings
 ↓
Vector Representations
 ↓
Cosine Similarity
 ↓
Vector Databases
 ↓
Document Chunking
 ↓
Semantic Retrieval
 ↓
Context Augmentation
 ↓
RAG
 ↓
Grounded Generation
```

---

# 📚 Key Concepts Learned

| Concept            | Purpose                                      |
| ------------------ | -------------------------------------------- |
| LLM                | Generates natural-language responses         |
| Embeddings         | Represent text as numerical vectors          |
| Chunking           | Break large documents into manageable pieces |
| Vector Database    | Store and search embeddings                  |
| Similarity Search  | Find relevant document sections              |
| Retrieval          | Select useful context                        |
| Prompt Engineering | Control how the LLM uses context             |
| RAG                | Combine retrieval with generation            |
| Grounding          | Keep answers connected to source information |
| Metadata           | Track document/page information              |

---

# ⚠️ Limitations

The initial version may have limitations such as:

* PDF extraction quality depends on document structure
* Scanned PDFs may require OCR
* Complex tables may not be extracted correctly
* Retrieval quality depends on chunking strategy
* Very ambiguous questions may retrieve irrelevant content
* LLM responses depend on the quality of retrieved context

These limitations provide opportunities for future RAG improvements.

---

# 🔐 Security

* API keys should never be committed to the repository.
* User documents should be handled securely.
* Sensitive documents should not be uploaded to an untrusted deployment.
* Production deployments should implement authentication and access control.

---

# 📈 Future Architecture

The project can eventually evolve from:

```text
Basic RAG
```

to:

```text
                 Query
                   │
                   ▼
             Query Rewriter
                   │
                   ▼
          ┌─────────────────┐
          │ Hybrid Retrieval│
          └────────┬────────┘
                   ↓
               Reranker
                   ↓
           Relevant Context
                   ↓
                 LLM
                   ↓
          Answer + Citations
                   ↓
             Evaluation
```

---

# 🤝 Contributing

Contributions, suggestions, and improvements are welcome.

### Steps

```bash
git clone https://github.com/YOUR-USERNAME/QueryDoc.git
```

Create a new branch:

```bash
git checkout -b feature/new-feature
```

Make your changes and commit:

```bash
git add .
git commit -m "Add new feature"
```

Push your branch:

```bash
git push origin feature/new-feature
```

Then open a Pull Request.

---

# 📜 License

This project is intended for educational and research purposes.

A suitable open-source license can be added when the project is ready for public distribution.

---

# 👨‍💻 Author

**Your Name**

Built as a hands-on project to understand and implement **Retrieval-Augmented Generation (RAG)** from the ground up.

---

# ⭐ Project Vision

QueryDoc started as a simple PDF question-answering system and is designed to evolve into a complete **document intelligence platform**.

> **From documents to knowledge — QueryDoc makes information searchable, understandable, and grounded.**

⭐ If you find this project useful, consider giving the repository a star.
