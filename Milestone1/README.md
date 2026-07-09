Infosys Franchise Analytics & Management Portal
An enterprise-grade, secure business intelligence and operational management system built for monitoring franchise performance metrics, system health indices, and user authentication management.

🚀 Features
Secure JWT Token Authentication: Session management backed by strong JSON Web Tokens with automated token validation.

Cryptographic Security Core: Production-level credential protection using bcrypt salting/hashing.

Dual-Layer Multi-Tenant Architecture: Integrated separation of concerns for Standard Users and Administrative Staff.

Flexible Multi-Factor Account Recovery: Dynamic password reset pipelines running over both Gmail App Password OTP pipelines and structural Security Questions.

Interactive High-Fidelity Data Visualizations: Real-time KPI micro-cards paired with responsive Plotly gauge telemetry.

🛠️ Tech Stack & Prerequisites
Before setting up the repository environment, ensure you have the following frameworks and tools available:

Core Engine: Python 3.10+

Database: SQLite3

Frontend UI Framework: Streamlit

Data Visualization: Plotly Open Source

Network Tunneling Core: Pyngrok

📂 Installation & Deployment Guide
Follow these sequential terminal configurations to get your dashboard environment fully operational.

1. Environment Setup & Dependency Installation
Clone your repository and install the structural software bundles:

Bash
# Clone your repository
git clone https://github.com/your-username/smart_parking_rag.git
cd smart_parking_rag

# Install all requisite platform dependencies
pip install streamlit streamlit-option-menu pyngrok pyjwt bcrypt plotly
2. Environment Variables Configuration
The production engine reads structural mail secrets straight out of your machine's environment parameters. Run these configuration steps before calling the execution block:

Bash
# For Windows environments (PowerShell)
$env:EMAIL_PASSWORD="your_gmail_app_password"

# For Linux/Mac/Google Colab Secrets
export EMAIL_PASSWORD="your_gmail_app_password"
3. Launching the Local Engine
Execute the core script using the Streamlit deployment module:

Bash
streamlit run app.py --server.port 8501
📸 Interface Showcases & Visual Proofs
🔐 Authentication Flow
User Sign In Screen
Provides an authoritative portal with rigorous string cleansing algorithms validating inputs dynamically.

Secure Account Creation
Includes client-side regex validations enforcing length constraints, case matching, numerical injections, and explicit special characters.

🛡️ Credential & Account Recovery Pipelines
Multi-Option Password Reset Hub
Users can dynamically choose their path to authentication recovery.

Recovery Path A: Security Questions
Queries the encrypted local database layer directly to match hashed answers.

Recovery Path B: Automated Mail Engine OTP
Dispatches responsive, corporate-styled HTML emails via a secure SMTP tunnel.

📊 Application Workspace
Standard User Analytics Dashboard
An interactive space displaying micro-KPI tracking tiles alongside an operational health index canvas.

Administrative Management Control Pane
Elevated dashboards designed for complete database audit tracking and account control.

🏗️ Architecture Design & Data Flow
[ User Browser ] <---> [ Streamlit Frontend Engine ] <---> [ Security Layer (JWT & Bcrypt) ]
                                                                   |
                                                                   v
                                                       [ SQLite3 Relational DB ]
Presentation Layer: Built with Streamlit components, styled comprehensively via a modern CSS injection pipeline for uniform branding.

Security & Cryptography Module: interceptors authenticate structural integrity by decoding standard HS256 signatures before updating active component matrices.

Storage Framework: Uses an isolated SQLite system connection thread pools to commit rows without racing states.

🔒 Security Specifications
Admin Default Access Keys: Username: admin | Password: Admin@123 (Note: Please migrate these default keys to custom variables before deployment to public production clusters).

OTP Lifetime Limits: Tokens automatically decay and invalidate within 5 minutes of creation to mitigate intercept vector loops.
