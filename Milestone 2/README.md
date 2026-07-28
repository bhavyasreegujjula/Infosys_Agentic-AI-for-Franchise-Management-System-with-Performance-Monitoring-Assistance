# FranchiseOps AI — Milestone 2

Comprehensive documentation for Milestone 2 of the FranchiseOps AI project — an enterprise multi-agent Franchise Operations platform focused on Supply Chain & Freight intelligence, inventory resilience, outlet tiering, and workforce retention.

This README lives in `milestone2/` and accompanies the Colab notebook `FreightQuote_AI_Milestone2.ipynb`, which was used to author and demo Milestone 2.

## Table of Contents

- [Project Overview](#project-overview)
- [Architecture & Design](#architecture--design)
- [File-by-File Module Guide](#file-by-file-module-guide-detailed)
- [Technology Stack](#%EF%B8%8F-technology-stack)
- [Quick Start — Colab](#quick-start--colab-recommended)
- [Quick Start — Local](#quick-start--local-developer-flow)
- [Troubleshooting & Common Gotchas](#troubleshooting--common-gotchas)
- [Security & Privacy Notes](#security--privacy-notes)
- [Contribution & License](#contribution--license)



## Project Overview

Milestone 2 delivers a working prototype of an enterprise-grade Franchise Operations assistant that integrates:

- **Local LLM Orchestration:** Powered by `Qwen2.5-3B-Instruct` (4-bit NF4) for lightweight on-device reasoning and synthesis.
- **Three Specialized Agents:**
  - **Agent 1:** Freight pricing & cost estimator (regressors trained on synthetic / Kaggle-sourced freight data).
  - **Agent 2:** Outlet territory clustering & revenue vs. weather analytics (K-Means + regressors).
  - **Agent 3:** Supply chain & inventory weather-aware reorder advisory (risk heatmaps + reorder queue).
- **Persistent SQLite Datastore:** Stores outlets, staff, inventory, ML model metadata, and chat history.
- **Full Authentication Stack:** Signup, login with progressive lockout, forgot-password via security questions or email OTP.
- **Streamlit UI:** Custom Neo-Brutalist theme with a multi-tab layout (AI Copilot, Agents, Analytics, Admin Dashboard).

---

## Architecture & Design

High-level modules and their core responsibilities:

- **`llm_engine.py`** — Loads `Qwen2.5-3B-Instruct` in 4-bit NF4 using Hugging Face and `bitsandbytes`. Provides fast helper functions (`_run()`, `generate_json()`, `orchestrate_3_agents_query()`, `generate_debate_and_synthesis()`) and background warmup.
- **`config.py`** — Centralizes secrets, storage paths, and model directory locations from Colab secrets or environment variables.
- **`auth.py`** — Authentication logic: signup, login with progressive lockout, security questions, email OTP, password strength checks, password reuse prevention (`password_history`), `bcrypt` hashing, and JWT tokens.
- **`db.py`** — SQLite database initialization and schema migration helpers (`outlets`, `staff`, `inventory_records`, `users`, `ml_models`, `notifications`, `chat_history`).
- **`ui_theme.py`** — Injects custom Neo-Brutalist CSS styling and renders standardized cards, headers, and risk badges across Streamlit pages.
- **`agent2_franchise.py` & `agent3_franchise.py`** — Interactive visualization renderers (Plotly charts, heatmaps) integrated with LLM orchestrator advisory functions.
- **`admin_dash.py`** — Admin control panel for user account management, GPU VRAM monitoring (`nvidia-smi`), ML model cards, and alert logging.
- **`seed_data.py`, `notifications.py`, `weather_context.py`** — Utilities for seeding sample data, simulated multi-channel notification dispatch, and city weather impact profiling.
- **`train_m2.py`** — Multi-algorithm ML model training pipeline comparing regressors and classifiers with automated Kaggle dataset fetching.
- **`app.py`** — Main Streamlit entrypoint wiring together navigation tabs, session state, chat history persistence, and agent pages.

---

### AI Copilot Debate View



## File-by-File Module Guide (Detailed)

> **Note:** File paths are relative to the repository root and main branch.

- **`milestone2/`** (This directory)
  - `README.md` — Project documentation and setup guide.
  - `screenshots/` — Visual UI assets and module screenshots.

- **`llm_engine.py`** — LLM Orchestration & Generation
  - **Key Functions:** `get_model()`, `warmup_llm()`, `_run()`, `generate_json()`, `orchestrate_3_agents_query()`, `generate_debate_and_synthesis()`
  - **Caching:** Honors `HF_TOKEN` and `CACHE_DIR` for Hugging Face caching to reduce redundant downloads.

- **`config.py`** — Configuration & Secret Retrieval
  - Reads secrets from Google Colab `userdata` or environment variables.
  - Defines `STORAGE_DIR`, `DB_PATH`, and directory paths where models and caches are stored.

- **`auth.py`** — Authentication Portal for Streamlit
  - `init_auth()` — Creates `users` and `password_history` tables and seeds an initial Administrator account if missing.
  - `render_auth_portal()` — Renders the Streamlit UI for Sign In, Registration, and Password Reset with progressive lockout.
  - **Security Features:** Implements `bcrypt` hashing, JWT handling, OTP expiry, resend cooldowns, and password history tracking to prevent reuse.

- **`db.py`** — Database Initialization & Utilities
  - `init_db()` — Handles idempotent creation of tables and safe schema migrations (wrapping `ALTER TABLE` in `try/except`).
  - **Helper Methods:** `save_ml_metrics()`, `load_chat_history()`, `save_chat_message()`, and `clear_chat_history()`.

- **`ui_theme.py`** — Styling & Consistent UI Rendering
  - `NEO_BRUTALIST_CSS` — Centralized styles for custom fonts, card layouts, and status badges.
  - **UI Helpers:** `inject_css()`, `apply_theme()`, `render_header()`, `render_card()`, and `risk_badge()`.

- **`agent2_franchise.py`** — Territory Clustering & Weather Analytics
  - **Visualizations:** Revenue vs. Cost scatter plots, demand surge bar charts, and revenue vs. weather correlation graphs with trendlines.
  - **Predictive Simulation:** Utilizes local heuristics/K-Means models to estimate tier clustering for new outlets.
  - **AI Advisory:** Leverages `llm_engine.orchestrate_3_agents_query()`.

- **`agent3_franchise.py`** — SKU Risk Heatmaps & Reorder Planning
  - Displays interactive SKU risk heatmaps (`plotly.imshow`).
  - Renders a reorder priority queue with urgency mapping.
  - Generates structured JSON reorder plans via `llm_engine.generate_json()`.

- **`admin_dash.py`** — Administrator Control Panel
  - **System Health:** GPU VRAM monitoring (`nvidia-smi`) and LLM status checks.
  - **User Management:** Add, unlock, or delete user accounts, and view password history tables.
  - **ML Model Card:** Inspects the `ml_models` table and provides quick summary metrics.
  - **Live Alert Log:** Reads records directly from the `notifications` table.

- **`notifications.py`** — Simulated Multi-Channel Alert Center
  - `send_alert(channel, recipient, subject, message)` — Persists alerts to the `notifications` table and prints console logs.

- **`seed_data.py`** — Sample Data Seeding
  - `seed_all()` — Populates initial sample data for outlets, staff, and inventory records, invoking `send_alert()` upon completion.

- **`weather_context.py`** — City Weather Profiles
  - `get_city_weather(city_name)` — Returns demand impact percentages, supply delays, and attrition stress indicators used in downstream analytics.

- **`train_m2.py`** — Model Training Pipeline
  - Multi-algorithm training pipeline comparing regressors and classifiers.
  - Features an automated Kaggle dataset downloader with a fallback to synthetic data generation.
    
- **app.py** — Streamlit app entrypoint
  - Sidebar with tabs: AI Copilot, Agent pages, Analytics, Admin dashboard, Sign Out
  - Handles LLM warmup, agent model loading, chat history persistence and calls to agent renderers


### 🛠️ Technology Stack

| Category | Technology / Library | Description & Usage |
| :----------------------- | :--- | :--- |
| **Execution & Compute** | `Python 3.12`, `Jupyter` / `Google Colab` | Core environment running CUDA GPU acceleration |
| **LLM & Inference Core** | `Qwen2.5-3B-Instruct`, `PyTorch` | 3B parameter model quantized with 4-bit `bitsandbytes` |
| **Machine Learning** | `scikit-learn`, `XGBoost`, `joblib` | Predictive models for attrition, clustering, & demand |
| **Data Processing** | `Pandas`, `NumPy` | Data manipulation, transformation, and tabular ops |
| **Frontend UI & Charts** | `Streamlit`, `Plotly`, `Altair` | Multi-page web dashboard with interactive analytics |
| **Database & Security** | `SQLite3`, `Bcrypt`, `PyJWT` | User authentication, token management, & password hashing |
| **Integrations & Utilities** | `pyngrok`, `Faker`, `Kaggle API` | Tunneling, synthetic data generation, & dataset fetching |
 
## Quick Start — Local (Developer Flow)

1. **Clone repository and navigate to directory:**
   ```bash
   git clone [https://github.com/bhavyasreegujjula/Infosys_FranciseOps_AI.git](https://github.com/bhavyasreegujjula/Infosys_FranciseOps_AI.git)
   cd Infosys_FranciseOps_AI/Milestone2


2. **Set up a virtual environment:**

   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate

3. **Install dependencies:**

pip install -r requirements.txt


4. **Configure environment variables:**
Create a .env file in the root directory

 1.HF_TOKEN=your_huggingface_token
 
 2.NGROK_AUTHTOKEN=your_ngrok_token
 
 3.EMAIL_ID=your_email@gmail.com
 
 4.EMAIL_PASSWORD=your_app_passwod

6. **Mount Google Drive in Colab when prompted**

 — The notebook expects to persist DB and model files under `/content/drive/MyDrive/FranchiseOps_AI`.

8. **Run the notebook sequentially:**
   
   Install dependencies → configure secrets & mount drive → verify GPU → write modules → init DB & seed     data → (optionally) train models.

10. **Launch Streamlit inside Colab and expose via ngrok :**

  !streamlit run app.py



Secrets, tokens & storage
-------------------------
- HF_TOKEN: Used by Hugging Face model loaders to access and cache model weights. If not provided, the notebook uses synthetic fallbacks or cached files in Drive if present.
- NGROK_AUTHTOKEN: Required to expose local Streamlit to the internet via ngrok. Keep private.
- KAGGLE_USERNAME / KAGGLE_KEY: Used to download datasets in train_m2.py. When absent, the training pipeline falls back to synthetic data generation.
- EMAIL_ID / EMAIL_PASSWORD: For OTP email sending via Gmail SMTP. Use app-specific passwords for security.

Storage paths
- STORAGE_DIR (config.py): Defaults to `/content/drive/MyDrive/FranchiseOps_AI` in Colab or ./data/FranchiseOps_AI locally
- Models are stored under STORAGE_DIR/models/ and the HF cache under models/hf_cache
- DB file: franchiseops.db (path configured in config.py)

Model caching and GPU notes
---------------------------
- llm_engine uses bitsandbytes 4-bit NF4 quantization to reduce VRAM usage. The code prefers sdpa attention on supported GPUs (T4/CUDA combos) for faster generation.
- Recommended GPU: NVIDIA Tesla T4 in Colab; ensure CUDA/PyTorch compatibility with bitsandbytes and transformers.
- For faster startup in a multi-user demo, call start_background_warmup() at app import time to load the LLM in a background thread.

Screenshots (placeholders)
--------------------------

### HOME PAGE
<img width="1652" height="917" alt="Home Page" src="https://github.com/user-attachments/assets/f8ae9aa8-54fe-42b7-8b5b-27de9924e98b" />


"A login page titled 'FranchiseOps AI Portal' for an Enterprise Multi-Agent Franchise Intelligence System. The page includes tabs for Sign In, Register, and Reset Password, fields for email/username and password, and a large yellow 'Sign In' button. The top-right corner displays the status '... CONNECTING'."

 ### AI COPILOT DEBATE VIEW
 - 
<img width="1920" height="913" alt="AI Copilot" src="https://github.com/user-attachments/assets/ab54ec9b-bb39-489f-b2bb-e6efe733fb32" />


"The FranchiseOps AI dashboard displays the AI Copilot module with GPU acceleration enabled on Tesla T4 for the Qwen-2.5-3B language model. The page presents a conversational interface titled 'Unified AI Copilot — Total Franchise Intelligence.' After the user asks which outlet is performing well, the AI reports that OUT-101 Mumbai shows positive revenue growth and stable performance metrics based on integrated franchise intelligence data. Navigation options for workforce, outlets, inventory, analytics, and administration are provided in the left sidebar."

### ML MODEL CARD

<img width="1433" height="839" alt="ML MODEL CARD" src="https://github.com/user-attachments/assets/c78a3e1a-9d45-4829-bfa0-9837502126d9" />

"The FranchiseOps AI Admin Dashboard displays the ML Model Card page showing machine learning model performance for three intelligent agents. A table lists model names, R² scores, training data size, and timestamps for inventory and revenue prediction models. Summary cards at the bottom report 100% accuracy for the workforce attrition model, an R² score of 0.835 for revenue prediction, and an R² score of 0.987 for inventory forecasting. The Admin Dashboard is selected in the left navigation panel."


### ML PRICING CALCULATOR

<img width="1920" height="909" alt="ML_PRICING_CALCULATOR" src="https://github.com/user-attachments/assets/d6e58ad7-f7a9-4083-898c-9a0128644587" />


The Supply Chain Ops AI dashboard displays the Agent 1: Freight Pricing & Cost Analyzer module. Users can enter shipment details including a distance of 250 kilometers, a shipment weight of 450 kilograms, traffic congestion level 2, and an Express delivery priority. Using machine learning models trained on supply chain datasets and accelerated by a Tesla T4 GPU environment, the system predicts a freight transportation cost of ₹8,089.65. The left navigation menu provides access to additional modules for route delay analysis, carrier compliance monitoring, analytics, and AI-powered supply chain intelligence.



### TRIGGERED ACCOUNT LOGOUT


<img width="1772" height="903" alt="TRIGGERED LOCKOUT" src="https://github.com/user-attachments/assets/3ee44b82-1e0f-4cf0-bb4c-d68809e1b9de" />



The image shows the User Management section of the Admin Dashboard in the FranchiseOps AI Portal. It allows administrators to manage user accounts and monitor their access status. The page displays a list of registered users along with their email addresses, assigned roles, and account statuses. Administrators can perform actions such as adding new users, unlocking locked accounts, and deleting user accounts. In the example, one user account is marked as Locked with an Unlock option available, while active users can be managed using the Delete option. This module enhances system security and access control by enabling efficient user administration and account management.


### OTP COOLDOWN

<img width="1920" height="909" alt="OTP COOLDOWN" src="https://github.com/user-attachments/assets/f9155473-b930-4a7b-9a91-42224ca0437f" />


The Reset Password page of the FranchiseOps AI Portal allows users to securely recover their accounts using either a Security Question or OTP sent via email. In this example, the user has selected OTP-based recovery. To enhance security and prevent OTP abuse, the system enforces an OTP cooldown period, displaying the message "Please wait 3 minutes before requesting another OTP." This mechanism limits repeated OTP requests, protects against spam or brute-force attempts, and ensures secure password recovery for registered users.


### ADD/DELETE/UNLOCK(Admin Dashboard)
<img width="1914" height="925" alt="ADD_DELETE_UNLOCK" src="https://github.com/user-attachments/assets/dfdf5eb6-eebf-4c5c-ac03-1b121a3cbdbf" />

This screen displays the Admin Dashboard & System Intelligence module of the FranchiseOps AI Portal. It enables administrators to manage user accounts securely through role-based access control. The dashboard shows the current status of all registered users, including their usernames, email addresses, assigned roles, and account status (Active or Locked).

Troubleshooting & common gotchas
--------------------------------

- **Model loading errors:**
  - Ensure `HF_TOKEN` is valid if private models are required.
  - Check `bitsandbytes` installation and CUDA toolkit compatibility with your PyTorch version.
- **SQLite locked / concurrent access issues:**
  - `db.get_conn` uses `check_same_thread=False` which relaxes some constraints. If you still experience locks, close other DB connections and ensure a single process writes at a time.
- **Email OTP failures:**
  - Confirm `EMAIL_ID` and `EMAIL_PASSWORD` (Gmail App Password) and ensure 2-step verification and App Passwords are configured properly.
- **ngrok token and public_url:**
  - If ngrok fails to connect, confirm the token and region limits on free plans.
 
  
## Security & Privacy Notes

This repository includes convenience defaults intended for demos only. For production deployment, complete the following before exposing the app:

Never commit secrets (HF_TOKEN, NGROK_AUTHTOKEN, EMAIL_PASSWORD) into the repository.

Use a secrets manager (Vault, AWS Secrets Manager, GitHub Actions Secrets) and environment variables for runtime.

Use HTTPS endpoints, harden the SMTP relay, and enforce strong password policies.

Consider configuring rate-limiting at the reverse proxy or API gateway level for high-traffic setups.

## Contribution & license


Contributions are welcome! Please open an issue or pull request for any bug fixes or feature requests.

Distributed under the MIT License. See LICENSE for more information.

Contributions: Open a PR with a clear description and tests where applicable. For large changes, open an issue first for design discussion.

Change Log (Milestone highlights)
---------------------------------
- M2.0: Core modules implemented (llm_engine, auth, db, agent visualisers, admin dashboard)
- M2.1: LLM 4-bit NF4 integration and background warmup thread
- M2.2: AI Copilot debate view and single-pass synthesis prompt template

