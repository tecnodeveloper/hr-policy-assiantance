📚 HR Policy Assistant

A beginner-friendly Retrieval-Augmented Generation (RAG) application that allows users to upload an HR Policy PDF and ask questions about the policy.

The application retrieves relevant sections from the uploaded document and sends those sections to an LLM to generate an answer.

🚀 Features
Upload an HR Policy PDF
Extract PDF text using PyMuPDF
Split the document into smaller chunks
Generate embeddings using Sentence Transformers
Store embeddings in a FAISS vector index
Search for relevant policy sections
Generate answers using Groq
Uses openai/gpt-oss-20b
Display source pages used for answers
Maintain chat history during the current session
Configurable chunk size
Configurable chunk overlap
Configurable number of retrieved chunks
Beginner-friendly Streamlit interface
Handles common errors gracefully
🧠 What is RAG?

RAG stands for:

Retrieval-Augmented Generation

Instead of asking an AI model to answer a question using only its general knowledge, RAG first retrieves relevant information from a specific document.

For this project:

HR Policy PDF
      ↓
Extract text
      ↓
Split into chunks
      ↓
Create embeddings
      ↓
Store in FAISS
      ↓
User asks question
      ↓
Create question embedding
      ↓
Search FAISS
      ↓
Retrieve relevant chunks
      ↓
Send chunks to Groq
      ↓
Generate answer


This makes the assistant focused on the uploaded HR policy.

🏗️ Project Architecture
hr-policy-assistant/
│
├── app.py
├── requirements.txt
├── README.md
├── .env.example
└── .gitignore

🔧 Technologies Used
Streamlit

Used to create the web application interface.

PyMuPDF

Used to extract text from PDF files.

The Python package is imported as:

import fitz

Sentence Transformers

Used to convert text into numerical vectors called embeddings.

The application uses:

all-MiniLM-L6-v2


This is a relatively lightweight embedding model suitable for a beginner project.

FAISS

FAISS is used for vector similarity search.

It allows us to find document chunks that are semantically similar to the user's question.

Groq

Groq provides the LLM used to generate the final response.

The application uses:

openai/gpt-oss-20b

💻 Installation
1. Clone or create the project

Create the following directory:

hr-policy-assistant


Place all five project files inside it.

2. Create a virtual environment

Open a terminal inside the project folder.

Windows
python -m venv venv


Activate it:

venv\Scripts\activate

macOS/Linux
python3 -m venv venv


Activate it:

source venv/bin/activate

3. Install dependencies

Run:

pip install -r requirements.txt

🔑 Groq API Key

The application requires a Groq API key.

Create a Groq API key from the Groq Console.

Then create a file called:

.env


The file should be in the same folder as app.py.

Add:

GROQ_API_KEY=your_actual_api_key_here


For example:

GROQ_API_KEY=gsk_xxxxxxxxxxxxxxxxx


Do not share your API key with anyone.

Do not commit .env to Git.

The .gitignore file included with this project already prevents .env from being committed.

▶️ Run the Application

Start Streamlit:

streamlit run app.py


Streamlit will provide a local URL, usually similar to:

http://localhost:8501


Open that address in your browser.

📄 Using the Application
Step 1 — Upload a PDF

Click:

Upload your HR Policy PDF


and select an HR policy PDF.

The application will:

Read the PDF.
Extract text from its pages.
Split the text into chunks.
Generate embeddings.
Build a FAISS index.
Step 2 — Ask a question

Once the PDF has been processed, use the chat box.

Example:

How many annual leave days do employees receive?


The application searches the uploaded policy for relevant information.

💬 Example Questions

You can ask questions such as:

How many annual leave days do employees receive?

What is the company's sick leave policy?

How many days of maternity leave are provided?

What is the policy for remote work?

What are the working hours?

What is the resignation notice period?

Who is eligible for health benefits?

What happens during the probation period?

🔎 How Retrieval Works

Suppose the user asks:

How many annual leave days do employees receive?


The application converts this question into an embedding.

FAISS compares the question embedding with the embeddings of the document chunks.

For example:

Question
   ↓
Embedding
   ↓
FAISS similarity search
   ↓
Top 5 chunks


The most relevant chunks are then provided to the LLM.

🤖 How the LLM Works

The application sends the retrieved policy information to Groq together with the user's question.

The model is instructed to:

Use only the retrieved policy information.
Not use general knowledge.
Not guess.
Not invent policy information.
Mention page numbers when useful.

If the answer cannot be found, the assistant should respond:

The information could not be found in the uploaded HR policy.

📌 Sources

After an answer, the application displays the chunks retrieved from the PDF.

For example:

Sources

Source 1 — Page 7
Similarity: 0.812

[Relevant policy text...]


This allows the user to see where the answer came from.

⚙️ Settings

The sidebar contains three settings.

Chunk size

Controls approximately how much text is placed into each chunk.

Default:

1000


Larger chunks contain more context but can contain more unrelated information.

Chunk overlap

Controls how much text is shared between neighboring chunks.

Default:

150


Overlap helps avoid losing important information that happens to be located at the boundary between two chunks.

Retrieved chunks

Controls how many chunks FAISS returns.

Default:

5


For example:

Top K = 5


means the application retrieves the five most relevant chunks.

🧩 Complete RAG Pipeline
                 HR Policy PDF
                       │
                       ▼
                 PyMuPDF / fitz
                       │
                       ▼
                 Extracted Text
                       │
                       ▼
                  Text Chunks
                       │
                       ▼
            Sentence Transformer
              all-MiniLM-L6-v2
                       │
                       ▼
                   Embeddings
                       │
                       ▼
                  FAISS Index
                       │
                       │
              User asks question
                       │
                       ▼
              Question Embedding
                       │
                       ▼
              FAISS Similarity Search
                       │
                       ▼
               Top Relevant Chunks
                       │
                       ▼
                 Groq LLM
              GPT-OSS 20B
                       │
                       ▼
                  Final Answer

🛡️ Important RAG Behavior

The assistant should not invent information.

For example, suppose the user asks:

How many annual leave days do employees receive?


If the PDF says:

Employees receive 20 days of annual leave per year.


The assistant can answer:

Employees receive 20 days of annual leave per year.


If the PDF does not contain any information about annual leave, the assistant should say:

The information could not be found in the uploaded HR policy.


It should not guess:

Employees probably receive 20 days.

🧯 Error Handling

The application handles several common problems.

No PDF

The application asks the user to upload an HR policy PDF.

Invalid PDF

A friendly error message is displayed instead of a Python traceback.

Empty PDF

The application informs the user that the PDF contains no usable pages.

Scanned PDF

If the PDF contains only images and no extractable text, the application informs the user that no text could be extracted.

OCR is not included in this beginner version.

Missing API key

If GROQ_API_KEY is missing, the application displays an error explaining that the key needs to be configured.

Groq API error

The application displays a user-friendly message instead of exposing the raw exception.

🔐 Security

Never put your real Groq API key directly inside app.py.

Do NOT do this:

client = Groq(
    api_key="gsk_your_real_key_here"
)


Instead, use:

.env


with:

GROQ_API_KEY=your_key


The .env file is ignored by Git.

📦 Dependencies

The project uses:

streamlit
pymupdf
sentence-transformers
faiss-cpu
groq
python-dotenv
numpy


Install them with:

pip install -r requirements.txt

🧪 Testing the Application

After starting the application:

streamlit run app.py


Upload a real HR policy PDF.

Then test:

What is the annual leave policy?


Check that:

The question appears in the chat.
FAISS retrieves relevant chunks.
The answer is generated.
Source pages are displayed.

Then ask something unrelated, for example:

What is the capital of France?


The application should not use general knowledge to answer the question.

It should instead respond:

The information could not be found in the uploaded HR policy.

🎓 What You Learn From This Project

This project demonstrates the fundamental components of a RAG system:

Document Loading
       ↓
Text Extraction
       ↓
Chunking
       ↓
Embeddings
       ↓
Vector Database
       ↓
Similarity Search
       ↓
Context Retrieval
       ↓
Prompt Construction
       ↓
LLM Generation


It is a good beginner project for understanding how document-based AI assistants work.

🚀 Future Improvements

Possible improvements include:

OCR support for scanned PDFs
Multiple PDF uploads
Persistent FAISS indexes
Better semantic chunking
Document metadata
Hybrid keyword + vector search
Authentication
User accounts
Conversation export
Streaming LLM responses
Better source citations
Document deletion
Cloud deployment
PostgreSQL/vector database integration
Evaluation tests for RAG accuracy
Support for DOCX and TXT files
📜 License

This project is intended for educational and demonstration purposes.

Before using it in a real HR environment, add appropriate security, privacy, authentication, auditing, and data-protection controls.

:::

## 7. How to run the project

After creating the files, your folder should look like:

```text
hr-policy-assistant/
│
├── app.py
├── requirements.txt
├── README.md
├── .env.example
└── .gitignore


Then run:

cd hr-policy-assistant


Create the virtual environment:

python -m venv venv

Windows
venv\Scripts\activate

macOS/Linux
source venv/bin/activate


Install everything:

pip install -r requirements.txt


Copy .env.example to .env and put your actual Groq key inside:

GROQ_API_KEY=your_actual_key


Then:

streamlit run app.py


The application will open in your browser.

One important detail

The implementation deliberately doesn't send the entire PDF to Groq. It first uses all-MiniLM-L6-v2 + FAISS to retrieve the most relevant pieces and only sends those pieces to openai/gpt-oss-20b. That's the key part that makes this a genuine RAG application, rather than simply a PDF chatbot.

Groq currently documents openai/gpt-oss-20b with a 131,072-token context window and supports it through the Python SDK. {"fallbackMarkdown":"(Groq Console
)","reference":{"matched_text":"","prefix":null,"start_idx":34723,"end_idx":34755,"safe_urls":["https://console.groq.com/docs/model/openai/gpt-oss-20b","https://console.groq.com/docs/model/openai/gpt-oss-20b?utm_source=chatgpt.com","https://console.groq.com/docs/models","https://console.groq.com/docs/models?utm_source=chatgpt.com"],"refs":[],"alt":"(Groq Console
)","prompt_text":null,"type":"grouped_webpages","items":[{"title":"OpenAI GPT-OSS 20B - GroqDocs","url":"https://console.groq.com/docs/model/openai/gpt-oss-20b?utm_source=chatgpt.com","attribution":"Groq Console","pub_date":null,"snippet":"","attribution_segments":null,"supporting_websites":[{"title":"Supported Models - GroqDocs","url":"https://console.groq.com/docs/models?utm_source=chatgpt.com","pub_date":null,"snippet":"","attribution":"Groq Console"}],"refs":[{"turn_index":0,"ref_type":"search","ref_index":0},{"turn_index":0,"ref_type":"search","ref_index":2}],"hue":null,"attributions":null}],"status":"done","style":null,"fallback_items":null,"error":null},"showLoginRequiredCard":false}

{"fallbackMarkdown":"Groq GPT-OSS 20B documentation
","reference":{"matched_text":"","prefix":null,"start_idx":34757,"end_idx":34848,"safe_urls":["https://console.groq.com/docs/model/openai/gpt-oss-20b","https://console.groq.com/docs/model/openai/gpt-oss-20b?utm_source=chatgpt.com"],"refs":[{"turn_index":0,"ref_type":"search","ref_index":0}],"alt":"Groq GPT-OSS 20B documentation
","prompt_text":null,"type":"url","item":{"title":"OpenAI GPT-OSS 20B - GroqDocs","url":"https://console.groq.com/docs/model/openai/gpt-oss-20b?utm_source=chatgpt.com","attribution":"console.groq.com","pub_date":null,"snippet":"","attribution_segments":null,"supporting_websites":[],"refs":[{"turn_index":0,"ref_type":"search","ref_index":0}],"hue":null,"attributions":null},"layout":null,"logo":null,"title":"Groq GPT-OSS 20B documentation"},"showLoginRequiredCard":false}

{"fallbackMarkdown":"Groq Python quickstart
","reference":{"matched_text":"","prefix":null,"start_idx":34850,"end_idx":34919,"safe_urls":["https://console.groq.com/docs/quickstart","https://console.groq.com/docs/quickstart?utm_source=chatgpt.com"],"refs":[{"turn_index":0,"ref_type":"search","ref_index":10}],"alt":"Groq Python quickstart
","prompt_text":null,"type":"url","item":{"title":"Quickstart - GroqDocs","url":"https://console.groq.com/docs/quickstart?utm_source=chatgpt.com","attribution":"console.groq.com","pub_date":null,"snippet":"","attribution_segments":null,"supporting_websites":[],"refs":[{"turn_index":0,"ref_type":"search","ref_index":10}],"hue":null,"attributions":null},"layout":null,"logo":null,"title":"Groq Python quickstart"},"showLoginRequiredCard":false}{"fallbackMarkdown":"","reference":{"matched_text":" ","prefix":null,"start_idx":34919,"end_idx":34919,"safe_urls":[],"refs":[],"alt":"","prompt_text":null,"type":"sources_footnote","sources":[{"title":"OpenAI GPT-OSS 20B - GroqDocs","url":"https://console.groq.com/docs/model/openai/gpt-oss-20b?utm_source=chatgpt.com","attribution":"Groq Console"}],"has_images":false},"showLoginRequiredCard":false}
