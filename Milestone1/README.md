** FRANCHISE ANALYTICS AND MANAGEMENT **

OVERVIEW:

Milestone 1 is the first complete working version of the Infosys Franchise Analytics & Management authentication and dashboard application.
This milestone focuses on building a secure login system inside a single Streamlit app. It supports user registration, login, password recovery, JWT-based session handling, a user analytics dashboard, and a separate admin dashboard for managing registered users.he authentication module serves as the foundation for future milestones by providing secure user management and access control.
The application is designed to run in Google Colab and is exposed publicly using ngrok.


FEATURES:

The application includes three main public pages and two protected dashboards.

1. Login Page

The Login page allows users to sign in using either their username or email address.
Fields:

Username / Email
Password
Login behavior:

Both fields are mandatory.On successful login, a JWT session token is created.
The JWT token is stored in Streamlit session state.The dashboard is shown only when a valid JWT token exists.If login fails, the app shows one generic error message.
The error does not reveal whether the username/email or password was incorrect.


2. Signup Page

The Signup page allows new users to create an account.
Fields:

1.Username
2.Email
3.Password
4.Confirm Password
5.Security Question
6.Security Answer

Signup behavior:

a.All fields are mandatory.
b.Username must be unique.
c.Email must be unique.
The admin username is reserved and cannot be used by normal users.
Security question is selected from a fixed list.
Security answer is hashed before storing.
Passwords are hashed before storing.
After successful signup, the user is redirected to the Login page.
Signup does not issue a JWT token.

3. Forgot Password Page

The Forgot Password page supports two password recovery methods.
1.Security Question Reset:

The user enters their registered email address and selects the security question route. If the email exists, the stored security question is displayed. The user must answer correctly before setting a new password.
2.OTP Reset:

The user enters their registered email address and selects the OTP route. A 6-digit OTP is generated and sent to the user's email through Gmail SMTP. The user must verify the OTP before setting a new password.


4. User Dashboard:

After a normal user logs in successfully, the app displays the User Dashboard.
The dashboard includes:
Analytics dashboard header
User name badge
Documents Indexed card
Searches Today card
Efficiency Score card
Security Status card
System Health Index gauge chart


5. Admin Dashboard

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

Email Format Rule:

The email must follow this structure:
At least 2 letters before @
At least 2 letters between @ and .
At least 2 letters after the final .
Example:
ab@cd.ef

Password Policy:

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


Tech Stack Used:

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

How To Run The Notebook:

Step 1: Install Dependencies

c
Run this cell in Google Colab:
!pip install -q streamlit streamlit-option-menu pyngrok pyjwt bcrypt plotly

Step 2: Create the Streamlit App


Run the notebook cell that writes the full Streamlit application into app.py.
The cell should start with:
%%writefile app.py

Step 3: Add Colab Secrets


Open the Secrets tab in Google Colab and add:

Secret Name             Value
JWT_SECRET            Random signing key for JWT session tokens
NGROK_AUTHTOKEN       Your Authtoken 
EMAIL_PASSWORD        Gmail App Password 
EMAIL_ADDRESS         Gmail address that sends the OTP


Step 4: Start the App


Run the Streamlit/ngrok cell. The app runs on port 8501, and ngrok generates a public URL.
Open the generated URL in your browser.
Login Details
Admin Login
Username: admin
Password: Admin@123
Normal User Login
Normal users must first create an account from the Signup page. After signup, they can log in using either their username or email address.


Screenshots:

All screenshots are stored inside the screenshots folder.
Login Page
<img width="1863" height="897" alt="Screenshot (99)" src="https://github.com/user-attachments/assets/c1b4c14c-df5e-4c6b-8b79-a5801ae7df32" />



Signup Page

<img width="1743" height="876" alt="Screenshot (100)" src="https://github.com/user-attachments/assets/eff59b8f-98ac-418a-9f58-74ca6c14d26f" />

Forgot Password 

Forgot Password - Security Question Route

<img width="1768" height="903" alt="Screenshot (103)" src="https://github.com/user-attachments/assets/a23d01b4-a286-4efd-84ec-95452ff6c97d" />


Forgot Password - OTP Route

<img width="1796" height="885" alt="Screenshot (101)" src="https://github.com/user-attachments/assets/46c64e0e-98d6-4397-a4ad-f11234fbee94" />

OTP Email

<img width="1526" height="758" alt="Screenshot (102)" src="https://github.com/user-attachments/assets/2f238ebc-35a1-44f1-b6b5-21c18317c541" />


User Dashboard

<img width="1901" height="910" alt="Screenshot (104)" src="https://github.com/user-attachments/assets/f056fa3f-447f-4f86-bfc6-222f6d3ea991" />


Admin Dashboard

<img width="1920" height="929" alt="Screenshot (105)" src="https://github.com/user-attachments/assets/19306efd-93d3-4f9c-bd63-4708c1e20a48" />


Security Notes:

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
