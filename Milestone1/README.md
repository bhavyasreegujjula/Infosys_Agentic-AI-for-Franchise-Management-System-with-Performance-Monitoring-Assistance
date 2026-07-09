FRANCHISE ANALYTICS AND MANAGEMENT 

OVERVIEW:

Milestone 1 is the first complete working version of the Infosys Franchise Analytics & Management authentication and dashboard application.
This milestone focuses on building a secure login system inside a single Streamlit app. It supports user registration, login, password recovery, JWT-based session handling, a user analytics dashboard, and a separate admin dashboard for managing registered users.
The application is designed to run in Google Colab and is exposed publicly using ngrok.


What Was Built
The application includes three main public pages and two protected dashboards.

1. Login Page
The Login page allows users to sign in using either their username or email address.
Fields:
Username / Email
Password
Login behavior:
Both fields are mandatory.
On successful login, a JWT session token is created.
The JWT token is stored in Streamlit session state.
The dashboard is shown only when a valid JWT token exists.
If login fails, the app shows one generic error message.
The error does not reveal whether the username/email or password was incorrect.


3. Signup Page
The Signup page allows new users to create an account.
Fields:
Username
Email
Password
Confirm Password
Security Question
Security Answer
Signup behavior:
All fields are mandatory.
Username must be unique.
Email must be unique.
The admin username is reserved and cannot be used by normal users.
Security question is selected from a fixed list.
Security answer is hashed before storing.
Passwords are hashed before storing.
After successful signup, the user is redirected to the Login page.
Signup does not issue a JWT token.
4. Forgot Password Page
The Forgot Password page supports two password recovery methods.
Security Question Reset
The user enters their registered email address and selects the security question route. If the email exists, the stored security question is displayed. The user must answer correctly before setting a new password.
OTP Reset
The user enters their registered email address and selects the OTP route. A 6-digit OTP is generated and sent to the user's email through Gmail SMTP. The user must verify the OTP before setting a new password.


6. User Dashboard
After a normal user logs in successfully, the app displays the User Dashboard.
The dashboard includes:
Analytics dashboard header
User name badge
Documents Indexed card
Searches Today card
Efficiency Score card
Security Status card
System Health Index gauge chart


8. Admin Dashboard
The Admin Dashboard uses a separate login that is defined directly in the code.
Admin credentials:
Username: admin
Password: Admin@123
The admin account is not created from the Signup page.
After admin login, the Admin Dashboard displays all registered users with only:
Username
Email
The admin dashboard never displays password data, password hashes, security answers, OTP values, or JWT tokens.
Validation and Security Rules
Mandatory Fields
No form is submitted if a required field is empty. This applies to:
Login
Signup
Forgot Password
Security question reset
OTP reset
Email Format Rule
The email must follow this structure:
At least 2 letters before @
At least 2 letters between @ and .
At least 2 letters after the final .
Example:
ab@cd.ef
Password Policy
Passwords must include:
Minimum 8 characters
At least one lowercase letter
At least one uppercase letter
At least one number
At least one special character
Example:
Bhavya@123
During password reset, users cannot reuse their current password.
JWT Session Handling
JWT is used for session management.
JWT is issued only after successful login.
JWT is stored in Streamlit session state.
Signup does not issue a JWT.
Password reset does not issue a JWT.
Dashboard access requires a valid JWT.
Invalid or missing JWT redirects the user to Login.
Tech Stack Used
Technology	Purpose
Python	Main programming language
Streamlit	Web application interface
SQLite	Local user database
bcrypt	Password and security answer hashing
PyJWT	JWT session and OTP token handling
Plotly	Dashboard gauge chart
pyngrok	Public URL for the Colab app
Google Colab	Notebook runtime environment
Gmail SMTP	OTP email delivery

Project Structure:

Milestone1/
|
|-- README.md
|-- app.py
`-- screenshots/
    |-- login.png
    |-- signup.png
    |-- forgot-password-security-question.png
    |-- forgot-password-otp.png
    |-- otp-email.png
    |-- user-dashboard.png
    `-- admin-dashboard.png
How To Run The Notebook
Step 1: Install Dependencies
Run this cell in Google Colab:
!pip install -q streamlit streamlit-option-menu pyngrok pyjwt bcrypt plotly
Step 2: Create the Streamlit App
Run the notebook cell that writes the full Streamlit application into app.py.
The cell should start with:
%%writefile app.py
Step 3: Add Colab Secrets
Open the Secrets tab in Google Colab and add:
NGROK_AUTHTOKEN
EMAIL_PASSWORD
NGROK_AUTHTOKEN is used to generate the public ngrok URL.
EMAIL_PASSWORD must be a Gmail App Password, not the normal Gmail account password.
Step 4: Start the App
Run the Streamlit/ngrok cell. The app runs on port 8501, and ngrok generates a public URL.
Open the generated URL in your browser.
Login Details
Admin Login
Username: admin
Password: Admin@123
Normal User Login
Normal users must first create an account from the Signup page. After signup, they can log in using either their username or email address.
Screenshots
Add the following screenshots inside the screenshots folder.
Login Page

Signup Page

Forgot Password - Security Question Route

Forgot Password - OTP Route

OTP Email

User Dashboard

Admin Dashboard

Security Notes
Passwords are never stored as plain text.
Passwords are hashed using bcrypt.
Security answers are also hashed.
JWT is used to control dashboard access.
Admin credentials are separate from signup accounts.
Admin dashboard only displays username and email.
Password hashes and security answers are never displayed.
OTP verification is required before OTP-based password reset.
Users cannot reset their password to the same old password.
Milestone Summary
Milestone 1 delivers a complete authentication and dashboard workflow. It includes secure signup, login, forgot password recovery using security question and OTP, JWT session handling, password validation, a user analytics dashboard, and a separate admin dashboard.
This milestone creates the foundation for future improvements such as advanced franchise analytics, user activity logs, database deployment, role permissions, and production hosting.
