# FranchiseOps AI — Milestone 2

Comprehensive documentation for Milestone 2 of the FranchiseOps AI project — an enterprise multi-agent Franchise Operations platform focused on Supply Chain & Freight intelligence, inventory resilience, outlet tiering, and workforce retention.

This README lives in milestone2/ and accompanies the Colab notebook `FreightQuote_AI_Milestone2.ipynb` which was used to author and demo Milestone 2.

Table of contents
-----------------
- Project overview
- Architecture & design
- File-by-file module guide
- Quick start (Colab and local)
- Secrets, tokens & storage
- Model caching and GPU notes
- Screenshots (placeholders)
- Troubleshooting & common gotchas
- Security & privacy notes
- Contribution & license
- Contact

Project overview
----------------
Milestone 2 delivers a working prototype of an enterprise-grade Franchise Operations assistant that integrates:

- Local LLM orchestration (Qwen-2.5-3B, 4-bit NF4) for lightweight on-device reasoning and synthesis
- Three specialized agents:
  - Agent 1: Freight pricing & cost estimator (regressors trained on synthetic / Kaggle-sourced freight data)
  - Agent 2: Outlet territory clustering & revenue vs. weather analytics (KMeans + regressors)
  - Agent 3: Supply chain & inventory weather-aware reorder advisory (risk heatmaps + reorder queue)
- Persistent SQLite datastore for outlets, staff, inventory, ML model metadata and chat history
- Full authentication stack (signup/login/OTP/password history/lockout and admin controls)
- Streamlit UI with a Neo‑Brutalist theme and multi-tab layout (AI Copilot, Agents, Analytics, Admin)

Architecture & design
---------------------
High-level modules and their responsibilities:

- llm_engine.py
  - Loads the Qwen-2.5-3B Instruct LLM in 4-bit NF4 using Hugging Face + bitsandbytes
  - Provides fast helper functions: _run(), generate_json(), orchestrate_3_agents_query(), generate_debate_and_synthesis()
  - Background warmup thread to avoid blocking first user interaction

- config.py
  - Centralizes secrets, storage paths, and model file locations. Reads from Colab secrets or environment variables.

- auth.py
  - Complete authentication flows for the demo: signup, login with progressive lockout, forgot-password via security question or email OTP
  - Password strength checker and password reuse prevention (password_history table)
  - Uses bcrypt for password hashing and JWT for session tokens

- db.py
  - SQLite initialization and helper functions for data storage and migrations
  - Tables: outlets, staff, inventory_records, merged_datasets, users, ml_models, notifications, chat_history

- ui_theme.py
  - Streamlit CSS injection and helper functions for consistent look-and-feel across pages

- agent2_franchise.py, agent3_franchise.py
  - Streamlit-renderable functions that build interactive visualisations (plotly charts, heatmaps) and call LLM orchestrator for advisory responses

- admin_dash.py
  - Admin controls (create/delete/unlock users), LLM & system health diagnostics, and ML model card showing stored metrics

- seed_data.py, notifications.py, weather_context.py
  - Synthetic seeding for sample outlets/staff/inventory
  - Simulated weather impacts per city and notification logging

- train_m2.py
  - Multi-algorithm training pipeline (comparison of regressors & classifiers) with Kaggle dataset downloader fallback to synthetic data

- app.py
  - Main Streamlit application wiring everything together into sidebar tabs and pages

File-by-file module guide (detailed)
-----------------------------------
Note: file paths are relative to the repository root and main branch.

- milestone2/ (this folder)
  - README.md  (this file)
  - screenshots/  (recommended folder to add UI/visual assets)

- llm_engine.py — LLM orchestration & fast generation
  - Key functions: get_model(), warmup_llm(), _run(), generate_json(), orchestrate_3_agents_query(), generate_debate_and_synthesis()
  - Caching: honors HF_TOKEN and CACHE_DIR for Hugging Face cache to reduce re-downloads

- config.py — configuration & secret retrieval
  - Reads secrets from Colab userdata or environment variables
  - Defines STORAGE_DIR, DB_PATH and paths where models and caches are stored

- auth.py — authentication portal for Streamlit
  - init_auth(): creates users & password_history tables and a seeded Administrator account if missing
  - render_auth_portal(): Streamlit UI for Sign In / Register / Reset Password with progressive lockout
  - Security features: bcrypt, JWT, OTP expiry, resend cooldowns, password history to prevent reuse

- db.py — database initialization and utilities
  - init_db(): idempotent creation of tables, safe migrations (ALTER TABLE wrapped in try/except)
  - save_ml_metrics(), load_chat_history(), save_chat_message(), clear_chat_history()

- ui_theme.py — CSS + helper rendering functions for consistent UI
  - NEO_BRUTALIST_CSS: fonts, card styles, badges
  - inject_css(), apply_theme(), render_header(), render_card(), risk_badge()

- agent2_franchise.py — outlet territory clustering & weather analytics
  - Visualizations: revenue vs cost scatter, demand surge bar charts, revenue vs weather correlation with trendline
  - Predictive simulate: a local heuristic or KMeans model to estimate Tier cluster for new outlets
  - AI Advisory uses llm_engine.orchestrate_3_agents_query()

- agent3_franchise.py — SKU risk heatmaps & reorder plan generator
  - SKU heatmap visualisation (plotly.imshow)
  - Reorder priority queue table with urgency mapping
  - JSON reorder plan generation via llm_engine.generate_json()

- admin_dash.py — full admin control panel
  - System Health: GPU VRAM usage (nvidia-smi) and LLM status
  - User Management: add/unlock/delete users, password history table handling
  - ML Model Card: view ml_models table and quick summary metrics
  - Live Alert Log: read notifications table

- notifications.py — simulated multi-channel alert center
  - send_alert(channel, recipient, subject, message) persists into notifications table and prints a console record

- seed_data.py — pre-seed realistic sample data
  - seed_all(): populates outlets, staff, inventory_records, calls send_alert() once initialization finishes

- weather_context.py — simulated city weather profiles
  - get_city_weather(city_name): returns demand impact %, supply delays, attrition stress indicators used to influence downstream analytics

- train_m2.py — model training pipeline
  - Kaggle downloader fallback to synthetic data
  - compare_regressors() and compare_classifiers() to select the best performing algorithm and persist with joblib

- app.py — Streamlit app entrypoint
  - Sidebar with tabs: AI Copilot, Agent pages, Analytics, Admin dashboard, Sign Out
  - Handles LLM warmup, agent model loading, chat history persistence and calls to agent renderers

Quick start — Colab (recommended)
----------------------------------
1. Open `FreightQuote_AI_Milestone2.ipynb` in Google Colab.
2. Install dependencies with the notebook cell (or run locally):

   pip install streamlit pyngrok bcrypt pyjwt pandas numpy scikit-learn joblib transformers accelerate bitsandbytes plotly streamlit-option-menu faker kaggle xgboost

3. Set Colab Secrets (Runtime → Manage session → Colab Secrets or use the Colab UI):
   - HF_TOKEN (optional) — Hugging Face token for caching model weights
   - NGROK_AUTHTOKEN (optional) — to expose Streamlit over the web
   - KAGGLE_USERNAME, KAGGLE_KEY (optional) — for Kaggle dataset downloads
   - EMAIL_ID, EMAIL_PASSWORD (optional) — for email OTPs (Gmail app passwords recommended)

4. Mount Google Drive in Colab when prompted — the notebook expects to persist DB and model files under `/content/drive/MyDrive/FranchiseOps_AI`.

5. Run the notebook sequentially: install dependencies → configure secrets & mount drive → verify GPU → write modules → init DB & seed data → (optionally) train models.

6. Launch Streamlit inside Colab and expose via ngrok (not recommended for production):

   from pyngrok import ngrok
   ngrok.set_auth_token(<NGROK_AUTHTOKEN>)
   ngrok.connect(8501)
   !streamlit run app.py

Quick start — Local (developer flow)
-----------------------------------
1. Clone repository and create virtualenv:

   git clone https://github.com/bhavyasreegujjula/Infosys_FranciseOps_AI.git
   cd Infosys_FranciseOps_AI
   python -m venv .venv
   source .venv/bin/activate
   pip install -r requirements.txt  # or pip install the list from the notebook

2. Export environment variables or edit config.py to set paths/secrets for local development. For example (Linux/macOS):

   export HF_TOKEN="your_hf_token"
   export NGROK_AUTHTOKEN="your_ngrok_token"
   export EMAIL_ID="you@gmail.com"
   export EMAIL_PASSWORD="app-password"

3. Initialize DB and seed sample data (optional but recommended):

   python -c "import db, seed_data; db.init_db(); seed_data.seed_all()"

4. Start the app:

   streamlit run app.py

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

-  App Home / Login UI (show Sign In / Register tabs)
![image alt](https://github.com/bhavyasreegujjula/Infosys_FranciseOps_AI/blob/8ee83deacc48ec5e096915d83bdcaa74667b808c/Milestone%202/Screenshots/Home%20Page.png)


 - AI Copilot Debate View showing agent bullets + synthesis
![image alt](https://github.com/bhavyasreegujjula/Infosys_FranciseOps_AI/blob/10576c6bf31f3aad173e2b1647e70cd730da75aa/Milestone%202/Screenshots/AI%20Copilot.png)


- Agent 2: Demand Surge / Revenue vs Weather scatter
![image alt](https://github.com/bhavyasreegujjula/Infosys_FranciseOps_AI/blob/10576c6bf31f3aad173e2b1647e70cd730da75aa/Milestone%202/Screenshots/ML_PRICING_CALCULATOR.png)


- Agent 3: SKU criticality heatmap and reorder queue table


- Admin Dashboard ML Model Card and user management
![image alt](https://github.com/bhavyasreegujjula/Infosys_FranciseOps_AI/blob/10576c6bf31f3aad173e2b1647e70cd730da75aa/Milestone%202/Screenshots/ML%20MODEL%20CARD.png)


-Screenshot of triggered lockout due to incorrect password entry
![image alt](https://github.com/bhavyasreegujjula/Infosys_FranciseOps_AI/blob/10576c6bf31f3aad173e2b1647e70cd730da75aa/Milestone%202/Screenshots/TRIGGERED%20LOCKOUT.png)



Troubleshooting & common gotchas
--------------------------------
- Model loading errors:
  - Ensure HF_TOKEN is valid if private models are required
  - Check bitsandbytes installation and CUDA toolkit compatibility with your PyTorch version
- SQLite locked / concurrent access issues:
  - db.get_conn uses check_same_thread=False which relaxes some constraints; if you still get locks, close other DB connections and ensure a single process writes at a time
- Email OTP failures:
  - Confirm EMAIL_ID and EMAIL_PASSWORD (app password) and allow less‑secure app access or app password in Gmail settings
- ngrok token and public_url:
  - If ngrok fails to connect, confirm the token and region limits on free plans

Security & privacy notes
------------------------
This repository includes convenience defaults intended for demos only. For production deployment, do the following before exposing the app:

- Never commit secrets (HF_TOKEN, NGROK_AUTHTOKEN, EMAIL_PASSWORD) into the repo
- Use a secrets manager (Vault, AWS Secrets Manager, GitHub Actions Secrets) and environment variables for runtime
- Use HTTPS endpoints, harden the SMTP relay, and enforce strong password policies
- Consider removing or locking the OTP resend window logic for rate-limiting in high‑traffic setups

Contribution & license
----------------------
If you want a formal license added, choose one of the following and I will add it to the repo:
- MIT
- Apache-2.0
- GPL-3.0

Contributions: Open a PR with a clear description and tests where applicable. For large changes, open an issue first for design discussion.

Change Log (Milestone highlights)
---------------------------------
- M2.0: Core modules implemented (llm_engine, auth, db, agent visualisers, admin dashboard)
- M2.1: LLM 4-bit NF4 integration and background warmup thread
- M2.2: AI Copilot debate view and single-pass synthesis prompt template

Contact
-------
Maintainer: bhavyasreegujjula
Email: bhavyasreegujjula@gmail.com


---

If you'd like I can:
- Add a `milestone2/screenshots/` directory and upload placeholder images (1x transparent PNG per slot)
- Add a LICENSE file with your preferred license
- Create a short README at the repo root pointing to all milestones

Commit message: "Expand milestone2 README with detailed documentation and screenshot slots"

