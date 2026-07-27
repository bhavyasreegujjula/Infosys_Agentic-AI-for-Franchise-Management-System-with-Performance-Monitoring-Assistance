# FranchiseOps AI — Milestone 2

This directory contains Milestone 2 artifacts for the FranchiseOps AI project (Supply Chain & Freight AI — Enterprise Multi-Agent Franchise Operations Platform).

Overview
--------
Milestone 2 builds the core application modules and demo notebook that stitch together:

- llm_engine.py — local LLM (Qwen-2.5-3B) loading and orchestration helpers
- config.py — secrets & paths configuration
- auth.py — complete authentication flows (signup, login, OTP, lockout, password history)
- db.py — SQLite schema and data helpers
- ui_theme.py — shared Streamlit styling
- agent2_franchise.py — outlet clustering & weather-aware visuals
- agent3_franchise.py — inventory / procurement advisor
- admin_dash.py — admin controls and model card
- notifications.py, seed_data.py, weather_context.py — helpers & simulation data
- train_m2.py — agent training pipeline
- app.py — main Streamlit application
- FreightQuote_AI_Milestone2.ipynb — interactive Colab notebook used to build and demo Milestone 2

Quick start
-----------
1. Clone the repository:

   git clone https://github.com/bhavyasreegujjula/Infosys_FranciseOps_AI.git

2. Open the Colab notebook `FreightQuote_AI_Milestone2.ipynb` (found at the repository root) and follow the steps to install dependencies, mount Google Drive, and initialize the database.

3. Ensure secrets are set (in Colab Secrets or environment):
   - HF_TOKEN (Hugging Face token) — optional but recommended for model cache
   - NGROK_AUTHTOKEN — if you want to expose the Streamlit app via ngrok
   - KAGGLE_USERNAME / KAGGLE_KEY — optional for Kaggle dataset download
   - EMAIL_ID / EMAIL_PASSWORD — for OTP email sending (use app password for Gmail)

4. To run locally (Streamlit):

   pip install -r requirements.txt  # or install the packages listed in the notebook
   streamlit run app.py

Notes
-----
- The notebook is authored for Google Colab (GPU recommended, T4) and uses Google Drive for persistent model & DB storage by default.
- The LLM model loading code assumes a cached model directory under your Drive: `/content/drive/MyDrive/FranchiseOps_AI/models/hf_cache`. Adjust `CACHE_DIR` in `llm_engine.py` or `config.py` if needed.
- The auth module writes sensitive defaults in `config.py` for demo convenience. Do not use these defaults in production — set secrets in environment or Colab Secrets.

Screenshots
-----------
Add screenshots to illustrate Milestone 2 (UI, notebook outputs, training logs). Recommended path: `milestone2/screenshots/`.

- Screenshot 1 — App Home / Login UI

  ![App Home - Login](screenshots/01_app_login.png)

- Screenshot 2 — AI Copilot / Debate View

  ![AI Copilot - Debate View](screenshots/02_copilot_debate.png)

- Screenshot 3 — Agent 2: Revenue vs Weather chart

  ![Agent2 - Revenue vs Weather](screenshots/03_agent2_weather.png)

- Screenshot 4 — Agent 3: SKU Heatmap / Reorder Queue

  ![Agent3 - SKU Heatmap](screenshots/04_agent3_heatmap.png)

- Screenshot 5 — Admin Dashboard / ML Model Card

  ![Admin Dashboard](screenshots/05_admin_modelcard.png)

How to add screenshots
----------------------
1. Create the folder `milestone2/screenshots/` in the repo.
2. Add PNG/JPEG images named as above (01_app_login.png, 02_copilot_debate.png, ...).
3. Commit and push. The README will display them on GitHub.

License
-------
This project uses an open demo license for educational purposes. If you want a specific license file added, tell me which one (MIT, Apache-2.0, etc.) and I will add it.

Contact
-------
Maintainer: bhavyasreegujjula

