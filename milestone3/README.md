FranchiseOps AI — RAG Knowledge Base Builder

 This notebook collects information from curated web and PDF sources, extracts text, adds operational SOP knowledge, converts the content into embeddings, and stores the embeddings in a FAISS vector index for semantic retrieval.

Note: The provided notebook implements the knowledge-base and retrieval layer of a RAG system. It retrieves relevant source passages but does not contain a full LLM answer-generation chain.

🎯 Objectives

🔎 Build a reliable knowledge base for the FranchiseOps RAG system.

🌐 Collect relevant information from web pages and PDF documents covering marketing, customer experience, HR, food safety, labour laws, workplace safety, and related business domains.

📄 Extract and store useful text from scraped webpages and downloadable PDFs.

📚 Combine external source material with curated FranchiseOps operational SOPs.

✂️ Split documents into manageable overlapping chunks for semantic retrieval.

🧠 Convert document chunks into numerical embeddings using a sentence-transformer model.

🗂️ Store the embeddings in a FAISS vector database for fast similarity search.

💬 Retrieve the most relevant knowledge for operational questions and provide source/SOP metadata.

🧪 Validate the retrieval pipeline using predefined FranchiseOps test queries.

Features

Scrapes configured HTML sources.

Automatically discovers PDF links embedded in scraped webpages.

Downloads and extracts text from PDF documents using PyMuPDF.

Stores extracted content as .txt files.

Maintains a manifest.json to track successful, skipped, and failed sources.

Loads scraped documents into LangChain Document objects.

Adds curated FranchiseOps operational SOPs with IDs such as KB-101 through KB-125.

Splits documents into overlapping chunks.

Generates semantic embeddings using all-MiniLM-L6-v2.

Builds a FAISS vector index for similarity search.

Runs retrieval dry-runs against operational questions.

Returns the retrieved source and SOP ID when available.

RAG Architecture

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

🔧 Technology

📌 Purpose

🐍 Python

Main implementation language

☁️ Google Colab

Notebook execution environment

💾 Google Drive

Persistent storage for RAG documents

🌐 Requests

Web requests and PDF downloading

🧹 BeautifulSoup

HTML parsing and PDF-link discovery

📄 PyMuPDF (fitz)

PDF text extraction

🔗 LangChain

Document processing and vector-store workflow

✂️ RecursiveCharacterTextSplitter

Document chunking

🧠 Sentence Transformers

Semantic text embeddings

🤗 all-MiniLM-L6-v2

Embedding model

🗃️ FAISS

Vector similarity search and storage

📝 TextBlob

Text-processing dependency

💭 VADER Sentiment

Sentiment-analysis dependency

⏳ tqdm

Progress bars during processing
