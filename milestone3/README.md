**FranchiseOps AI — RAG Knowledge Base Builder**

 This notebook collects information from curated web and PDF sources, extracts text, adds operational SOP knowledge, converts the content into embeddings, and stores the embeddings in a FAISS vector index for semantic retrieval.

## 🎯 Objectives & Deliverables

The primary objective of this module is to construct a robust, high-performance knowledge retrieval pipeline that powers the **FranchiseOps RAG System** with grounded, domain-specific intelligence.

| # | Icon | Objective | Target Description | Primary Tool / Tech |
| :-: | :-: | :--- | :--- | :--- |
| **1** | 🔎 | **Knowledge Base Construction** | Build a reliable, structured knowledge base tailored for the FranchiseOps RAG system. | `LangChain`, `Google Drive` |
| **2** | 🌐 | **Multi-Domain Web Scraping** | Collect relevant operational data across marketing, CX, HR, food safety, labor laws, and workplace safety. | `Requests`, `BeautifulSoup` |
| **3** | 📄 | **Text Extraction & Storage** | Parse and extract clean text from web pages and downloadable PDFs into persistent `.txt` files. | `PyMuPDF (fitz)` |
| **4** | 📚 | **SOP Integration** | Merge extracted external source materials with curated FranchiseOps operational SOPs and metadata. | `LangChain Document` |
| **5** | ✂️ | **Document Chunking** | Divide long-form text into manageable, overlapping chunks to maintain semantic context during retrieval. | `RecursiveCharacterTextSplitter` |
| **6** | 🧠 | **Semantic Embeddings** | Convert document text chunks into 384-dimensional dense vector representations. | `all-MiniLM-L6-v2` |
| **7** | 🗂️ | **Vector Indexing** | Index and store vector embeddings in a high-speed similarity database for $k$-NN search. | `FAISS` |
| **8** | 💬 | **Grounded Retrieval & Metadata** | Retrieve top-matching knowledge chunks for user queries along with traceable source/SOP citations. | `FAISS Retriever`, `Qwen2.5-3B` |.

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


## 🚀 Running the Notebook (`FranchiseOps_RAG_Builder.ipynb`)

Follow these step-by-step instructions to execute the RAG pipeline notebook, crawl source documents, build vector embeddings, and perform similarity retrieval.

---

### ⚙️ Step-by-Step Execution Workflow

| Step | Phase | Details & Action |
| :-: | :--- | :--- |
| **1** | **Open in Google Colab** | Launch `FranchiseOps_RAG_Builder.ipynb` in **Google Colab** (configured for a **Python 3** kernel and **T4 GPU** acceleration). |
| **2** | **Mount Google Drive** | Mount Google Drive to establish a persistent document directory:<br>`/content/drive/MyDrive/FranchiseOps_AI/rag_documents`<br>*(Stores downloaded `.txt` files, crawled outputs, and the source manifest)*. |
| **3** | **Install Dependencies** | Execute the package installation cell to install core RAG and web scraping libraries:<br>`!pip install pymupdf beautifulsoup4 requests langchain sentence-transformers faiss-cpu tqdm` |
| **4** | **Collect Source Material** | **Automated Web & PDF Scraper Executed:**<br>• Processes configured HTML sources and extracts clean text.<br>• Discovers embedded PDF links and merges them with static PDF source lists.<br>• Downloads PDFs and extracts text using `PyMuPDF` (`fitz`).<br>• Saves successful extracted content as `.txt` files in Drive.<br>• *Includes retries, exponential backoff, request timeouts, and SSL fallback routines.* |
| **5** | **Load & Label Documents** | Text files exceeding 50 characters are loaded into **LangChain `Document` objects** and assigned metadata attributes: |

#### 🏷️ Document Metadata Structure
```json
{
    "source": "filename.txt",
    "type": "scraped",
    "sop_id": "SOP-104"  // Added for curated FranchiseOps SOP documents
}


6. Vector Store & Index Generation
Documents are split into overlapping text chunks and embedded into a similarity search index:

# Document Chunking
text_splitter = RecursiveCharacterTextSplitter(
    chunk_size=1000,
    chunk_overlap=100
)

# Dense Embeddings Model
embeddings = HuggingFaceEmbeddings(
    model_name="all-MiniLM-L6-v2"
)

# Persistent Vector Database Output
vectorstore = FAISS.from_documents(docs, embeddings)
vectorstore.save_local("franchiseops_faiss_index")

<img width="1600" height="590" alt="m3 2" src="https://github.com/user-attachments/assets/7bd0e130-282c-4473-917d-23be43ff1797" />
<img width="1600" height="552" alt="m3 1" src="https://github.com/user-attachments/assets/66fa891d-53ff-43e8-b6f9-cb70bcb752f5" />
