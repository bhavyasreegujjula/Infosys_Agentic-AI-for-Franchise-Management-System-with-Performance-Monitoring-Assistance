FranchiseOps AI — RAG Knowledge Base Builder
This repository contains the Jupyter/Google Colab notebook used to scrape, curate, process, and index compliance, operational, and regulatory documentation for the FranchiseOps AI Retrieval-Augmented Generation (RAG) system.

📌 Project Overview
The Knowledge Base Builder automated pipeline collects domain-specific operational standards and regulatory compliance documentation across multiple key domains (food safety, labor laws, customer experience, marketing, HR, workplace safety). It cleans and chunks this unstructured data alongside structured internal SOPs, embeds the text, and stores the resulting vector representations in a searchable FAISS index.

🏗️ Architecture & Workflow Pipeline
Environment Setup & Drive Integration: Mounts Google Drive storage for output persistence and installs relevant NLP/RAG dependencies.

Web Scraping & PDF Harvesting (Phase 1 & 2):

Scrapes structured HTML pages using requests and BeautifulSoup.

Automatically extracts embedded PDF links from web content.

Combines harvested links with curated static PDF lists to compile a comprehensive document repository.

Document Extraction & Normalization (Phase 3):

Downloads and parses PDFs using PyMuPDF (fitz).

Cleans HTML layout artifacts, scripts, and non-content elements.

Exports extracted document text into individual raw .txt files while keeping track of processing status in a persistent manifest.json.

Curated SOP Integration: Loads and formats internal structured Standard Operating Procedures (KB-101 through KB-125) into document models.

Text Splitting & Embedding:

Uses RecursiveCharacterTextSplitter (chunk size: 1000, overlap: 100).

Generates dense vector embeddings using SentenceTransformers (all-MiniLM-L6-v2).

Vector Indexing & Dry Run Verification:

Constructs a FAISS vector store and saves the local index (franchiseops_faiss_index).

Runs verification queries against the vector database to ensure high retrieval relevance across SOPs and regulatory documents.

🛠️ Tech Stack & Requirements
Language: Python 3.x

Orchestration & Frameworks: LangChain, LangChain Community, LangChain Text Splitters

Web Scraping & Parsing: BeautifulSoup4, requests, urllib3, PyMuPDF (fitz)

Embeddings & Vector Store: sentence-transformers, FAISS (faiss-cpu)

Utilities: tqdm, TextBlob, vaderSentiment

📁 Key File & Directory Artifacts

FranchiseOps_AI/
└── rag_documents/
    ├── manifest.json                        # Tracks status (success/skipped/failed) of source URLs
    ├── html_*.txt                           # Extracted text files from web pages
    └── pdf_*.txt                            # Extracted text files from PDF documents
kb_franchise.json                            # Structured internal SOP database (JSON)
franchiseops_faiss_index/                    # Built FAISS vector database
🚀 Quickstart Guide
1. Installation
Install all required Python dependencies:

Bash
pip install -q langchain langchain-community langchain-text-splitters -U \
  langchain-core sentence-transformers faiss-cpu pymupdf \
  beautifulsoup4 requests==2.32.4 urllib3 tqdm vaderSentiment textblob
2. Execution
Run the notebook sequentially in Google Colab or a local Jupyter environment:

Cell 1–3: Mount Google Drive and initialize output directories (/content/drive/MyDrive/FranchiseOps_AI/rag_documents).

Cell 4: Define target HTML sources (FSSAI, OSHA, FDA, WHO, ILO, Labour Laws) and target PDF URLs.

Cell 5: Execute multi-phase scraping, PDF harvesting, and text extraction.

Cell 6–7: Merge scraped text files with internal curated JSON SOPs (kb_franchise.json).

Cell 8: Split text into chunks, generate embeddings via all-MiniLM-L6-v2, and save the local FAISS index (franchiseops_faiss_index).

Cell 9: Perform dry-run retrieval tests to verify query accuracy.

🔍 Validation & Dry-Run Queries
The system was validated using test queries covering key franchise operational domains:

Minimum freezer temperature rules

Shift staffing requirements

Handwashing and hygiene compliance

FSSAI non-compliance penalties

Customer complaint escalation protocols

Marketing campaign ROI thresholds

Quarterly staff performance review criteria
