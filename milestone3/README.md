FranchiseOps AI — RAG Knowledge Base Builder

 This notebook collects information from curated web and PDF sources, extracts text, adds operational SOP knowledge, converts the content into embeddings, and stores the embeddings in a FAISS vector index for semantic retrieval.



##🎯 Objectives

🔎 Build a reliable knowledge base for the FranchiseOps RAG system.

🌐 Collect relevant information from web pages and PDF documents covering marketing, customer experience, HR, food safety, labour laws, workplace safety, and related business domains.

📄 Extract and store useful text from scraped webpages and downloadable PDFs.

📚 Combine external source material with curated FranchiseOps operational SOPs.

✂️ Split documents into manageable overlapping chunks for semantic retrieval.

🧠 Convert document chunks into numerical embeddings using a sentence-transformer model.

🗂️ Store the embeddings in a FAISS vector database for fast similarity search.

💬 Retrieve the most relevant knowledge for operational questions and provide source/SOP metadata.

🧪 Validate the retrieval pipeline using predefined FranchiseOps test queries.

## ✨ Features & Capabilities

### 📚 1. Document RAG Engine & PDF Studio
| Feature | Description |
| :--- | :--- |
| **PDF Discovery & Parsing** | Scrapes and downloads PDF manuals and operational guidelines using `BeautifulSoup` and `Requests`. |
| **Text Extraction & Chunking** | Extracts raw text via `PyMuPDF` (`fitz`) and creates optimized overlapping chunks using `RecursiveCharacterTextSplitter`. |
| **Vector Store & Retrieval** | Converts text into semantic embeddings with `all-MiniLM-L6-v2` and indexes them in `FAISS` for fast similarity search. |
| **Grounded AI Generation** | Feeds relevant document chunks into the LLM context to generate accurate answers with source citations and avoid hallucinations. |

---

### 📊 2. Kaggle Data Pipeline
| Feature | Description |
| :--- | :--- |
| **Automated Dataset Ingestion** | Fetches enterprise HR, logistics, and sales datasets automatically via the Kaggle API. |
| **Data Cleaning & Normalization** | Cleans missing fields, handles malformed inputs, and formats columns for downstream agent consumption. |
| **Non-Destructive Refresh** | Updates internal data storage tables seamlessly without resetting active user sessions or authentication records. |

---

### 🤖 3. AI Copilot & Domain Intelligence
| Feature | Description |
| :--- | :--- |
| **RAG-Backed Copilot** | Answers user queries in real-time by drawing factual knowledge directly from indexed franchise documents. |
| **Sentiment & Feedback Analysis** | Evaluates customer feedback and operational logs using `VADER Sentiment` and `TextBlob`. |
| **Multi-Agent Data Feeds** | Passes refreshed pipeline data directly to domain agents (Workforce, Outlet Tiering, Inventory Safety). |

## 🏗 RAG System Architecture
Configured HTML Sources + PDF Sources
                |
                v
       Web Scraping / PDF Harvesting
                |
                v
       PDF Download + Text Extraction
                |
                v
          Text Documents (.txt)
                |
                +------ Curated Franchise SOPs
                |
                v
        LangChain Document Objects
                |
                v
       Recursive Text Chunking
        chunk_size = 1000
        overlap    = 100
                |
                v
     HuggingFace Sentence Embeddings
          all-MiniLM-L6-v2
                |
                v
          FAISS Vector Store
                |
                v
       Similarity Search / Retrieval
                |
                v
       Relevant Source Snippet

      
 🛠️ Technologies Used

| 🔧 Technology | 📌 Purpose |
| :--- | :--- |
| **🐍 Python** | Main implementation language |
| **☁️ Google Colab** | Notebook execution environment |
| **💾 Google Drive** | Persistent storage for RAG documents |
| **🌐 Requests** | Web requests and PDF downloading |
| **🧹 BeautifulSoup** | HTML parsing and PDF-link discovery |
| **📄 PyMuPDF (fitz)** | PDF text extraction |
| **🔗 LangChain** | Document processing and vector-store workflow |
| **✂️ RecursiveCharacterTextSplitter** | Document chunking |
| **🧠 Sentence Transformers** | Semantic text embeddings |
| **🤗 all-MiniLM-L6-v2** | Embedding model |
| **🗃️ FAISS** | Vector similarity search and storage |
| **📝 TextBlob** | Text-processing dependency |
| **💭 VADER Sentiment** | Sentiment-analysis dependency |
| **⏳ tqdm** | Progress bars during processing |


📂 Project Structure
FranchiseOps_AI/ │ ├── rag_documents/ │ ├── html_.txt │ ├── pdf_.txt │ ├── manifest.json │ ├── kb_franchise.json │ ├── franchiseops_faiss_index/ │ ├── RAG_KnowledgeBase.ipynb │ └── README.md


Running the Notebook

1. Open in Google Colab

Open:

FranchiseOps_RAG_Builder.ipynb

The notebook is configured for a Python 3 kernel and uses Google Drive for persistent storage.

2. Mount Google Drive

The notebook mounts Google Drive and creates:

/content/drive/MyDrive/FranchiseOps_AI/rag_documents

This directory is used for scraped text files and the source manifest.

3. Install dependencies

Run the package-installation cell before executing the remaining cells.

4. Collect source material

The notebook:

Processes configured HTML sources.

Extracts readable webpage text.

Discovers PDF links embedded in webpages.

Merges discovered PDFs with the static PDF list.

Downloads PDFs.

Extracts PDF text.

Saves successful content as .txt files.

The scraper uses retries, exponential backoff, request timeouts, and an SSL fallback.

5. Load documents

Scraped text files longer than 50 characters are loaded as LangChain Document objects.

Each scraped document receives metadata such as:

{
    "source": "filename.txt",
    "type": "scraped"
}

Curated SOP documents additionally contain an SOP ID.

6. Create embeddings and FAISS index

Documents are split using:

RecursiveCharacterTextSplitter(
    chunk_size=1000,
    chunk_overlap=100
)

Embeddings are generated with:

HuggingFaceEmbeddings(
    model_name="all-MiniLM-L6-v2"
)

The FAISS index is then created and saved locally as:

franchiseops_faiss_index

Retrieval

The notebook performs semantic similarity search with:

vectorstore.similarity_search(query, k=1)

Example questions tested by the notebook include:

What is the minimum freezer temperature?
How many staff are required per shift?
What is the handwashing procedure?
