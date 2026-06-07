# Financial RAG Project - Free Local Version

## What this version does

* Upload PDF or TXT files
* Parse document text
* Chunk the text
* Create local embeddings using `sentence-transformers`
* Store vectors in ChromaDB
* Retrieve relevant chunks for a question
* Generate an answer in one of two ways:

  * **Optional Ollama local model** if your laptop can run it
  * **Built-in extractive fallback** if Ollama is off or unavailable
* Show citations with file name, page number, and preview text

## Recommended beginner mode

For your system, start with the default setup:

* `USE\_OLLAMA=false`
* This uses the extractive fallback and avoids memory issues.

Later, if you want, you can enable Ollama with a light model like `gemma:2b`.

\---

## Project structure

```text
financial\_rag\_project\_free/
│   requirements.txt
│   .env.example
│   README.md
│
├── app/
│   ├── main.py
│   ├── config.py
│   ├── schemas.py
│   └── services/
│       ├── chunking.py
│       ├── parsing.py
│       ├── embeddings.py
│       ├── vector\_store.py
│       ├── retrieval.py
│       ├── llm.py
│       └── synthesis.py
│
├── ui/
│   └── streamlit\_app.py
│
└── data/
    ├── input/
    └── chroma/
```

\---

## Step-by-step setup on Windows

### 1\) Extract the zip

Extract the project to any folder.

Example:

```text
C:\\Users\\prani\\Desktop\\financial\_rag\_project\_free
```

### 2\) Open Command Prompt in the project folder

In the extracted folder, click the address bar, type `cmd`, and press Enter.

### 3\) Create virtual environment

```bash
python -m venv .venv
```

### 4\) Activate virtual environment

```bash
.venv\\Scripts\\activate
```

You should now see `(.venv)` at the beginning of the line.

### 5\) Install required packages

```bash
pip install -r requirements.txt
```

This may take a few minutes.

### 6\) Create `.env`

```bash
copy .env.example .env
```

### 7\) Keep default settings first

Open `.env` in Notepad and make sure this line is present:

```env
USE\_OLLAMA=false
```

That means the project will work even if your system cannot run a local LLM.

### 8\) Start the backend

```bash
uvicorn app.main:app --reload
```

Keep this terminal open.

You should see something like:

```text
Uvicorn running on http://127.0.0.1:8000
```

### 9\) Open a second Command Prompt

Go to the same project folder again.

Activate the virtual environment again:

```bash
.venv\\Scripts\\activate
```

### 10\) Start the Streamlit UI

```bash
streamlit run ui/streamlit\_app.py
```

This should open the browser automatically.

If it does not, open:

```text
http://localhost:8501
```

\---

## How to use the app

### 1\) Check backend

Open:

```text
http://127.0.0.1:8000/docs
```

If it opens, the backend is running.

### 2\) In the Streamlit page

* Upload a PDF or TXT file
* Click **Ingest uploaded files**
* Wait for success message

### 3\) Ask a question

Examples:

* `Summarize this document`
* `What are the main risk factors?`
* `What does the report say about revenue?`
* `Summarize the company’s credit risk disclosures`

\---

## How the answer works in this free version

### Default mode: extractive fallback

The app selects the most relevant sentences from the retrieved chunks and returns them as the answer.

This is lightweight and reliable for low-memory systems.

### Optional mode: Ollama

Only use this if your laptop has enough available memory.

1. Install Ollama
2. Pull a small model such as:

```bash
ollama pull gemma:2b
```

3. Update `.env`:

```env
USE\_OLLAMA=true
OLLAMA\_MODEL=gemma:2b
```

4. Restart the backend

If Ollama fails, the app will automatically fall back to extractive mode.

\---

## Where to get sample financial PDFs

You can use public SEC filings or any PDF you already have.

Examples of search terms:

* `Apple 10-K pdf sec`
* `Microsoft 10-K pdf sec`
* `Tesla 10-Q pdf sec`

You can also test with any normal PDF first.

## Next improvements you can add later

* Table extraction from financial PDFs
* Better chunk ranking / reranking
* Confidence score
* JSON structured output
* Highlight exact cited sentences
* Better summarization for financial metrics

