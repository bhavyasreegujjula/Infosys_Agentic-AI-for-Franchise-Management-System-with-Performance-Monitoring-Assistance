
Milestone 1:

Milestone 1 is the first complete working version of the authentication system and dashboard application. The goal of this milestone is to build a secure login flow inside one Streamlit app, allow users to create accounts, recover forgotten passwords, and access a dashboard only after successful login.

The app also includes a separate admin login. The admin can view the list of registered users, but password data is never shown.
This project is built as a Streamlit web application and is designed to run inside Google Colab using ngrok. Streamlit creates the web interface, SQLite stores user data, bcrypt protects passwords through hashing, JWT manages login sessions, and Gmail SMTP is used to send OTP emails for password recovery.

The app contains three main public pages:

- Login Page
- Signup Page
- Forgot Password Page

After successful login, the user is taken to either:

- User Dashboard
- Admin Dashboard

The dashboard shown depends on the type of login.


Features Built:
1. Login Page

The login page allows users to sign in using either their username or email address along with their password.

Required fields:

- Username / Email
- Password

The login form does not submit if any required field is empty.

When login succeeds, the app creates a JWT session token and stores it in Streamlit session state. This token is then used to decide whether the dashboard can be shown.

2. Signup Page

The signup page allows new users to create an account.
Required fields:
Username
Email
Password
Confirm password
Security question
Security answer
The security question is selected from a fixed list. The selected question and the hashed security answer are stored so that they can be used later during password recovery.
The username and email must be unique. If a user tries to sign up using an existing username or email, the app shows a clear error message.
After successful signup, the user is sent back to the Login page. A JWT is not issued during signup. A JWT is issued only after a successful login.

3. Forgot Password Page

The forgot password page gives users two recovery methods.
Security Question Reset
The user enters their registered email address and chooses the security question method.
If the email exists, the app displays the stored security question. The user must enter the correct security answer before creating a new password.
The new password must follow all password rules and cannot be the same as the current password.
OTP Reset
The user enters their registered email address and chooses the OTP method.
The app generates a 6-digit OTP and sends it to the user’s email address using Gmail SMTP. The OTP is stored securely inside a JWT token with an expiry time.
The user must enter the correct OTP before setting a new password.
 5.  Password Rules:
Passwords must satisfy all of the following conditions:
Minimum 8 characters
At least one lowercase letter
At least one uppercase letter
At least one number
At least one special character
#During password reset, the app also checks whether the new password is the same as the old password. If it is the same, the user is asked to choose another password.

5.Email Format Rule:

The app validates email addresses using a custom rule.
The email must have:
At least 2 letters before the @
At least 2 letters between the @ and the dot
At least 2 letters after the final dot

6 .JWT Session Handling:

JWT is used to manage sessions.
When a normal user logs in successfully, the app creates a JWT token with the user role.
When an admin logs in successfully, the app creates a JWT token with the admin role.


Tech Stack Used:

Python
Python is the main programming language used for the app logic.

Streamlit
Streamlit is used to build the web interface, pages, forms, buttons, messages, dashboard cards, and layout.

SQLite
SQLite is used as the local database. It stores registered user data such as username, email, hashed password, security question, and hashed security answer.

bcrypt
bcrypt is used to hash passwords and security answers. This means plain text passwords are not stored in the database.

PyJWT
PyJWT is used to create and verify JWT tokens for login sessions and OTP verification.

Plotly
Plotly is used to build the dashboard gauge chart.

pyngrok
pyngrok is used to expose the Streamlit app running in Google Colab through a public URL.

Google Colab
Google Colab is used as the notebook environment for running the Streamlit app.

Gmail SMTP
Gmail SMTP is used to send OTP emails during password recovery.


How To Run The Notebook:

Step 1: Install Required Packages
Run this cell in Google Colab:
!pip install -q streamlit streamlit-option-menu pyngrok pyjwt bcrypt plotly
  This installs all required Python packages for the app.

 Step 2: Create The Streamlit App 


Step 3 : Add Colab Secrets
In Google Colab, open the Secrets tab and add these secrets:
NGROK_AUTHTOKEN - to create a public ngrok URL
EMAIL_PASSWORD  - to send OTP emails. This should be a Gmail App Password, not the normal Gmail password.
EMAIL_ADDRESS   - Gmail address that sends the OTP
JWT_SECRET      - Random signing key for JWT session tokens

Step 4: Start Streamlit With ngrok
Run the notebook cell that starts Streamlit and creates the ngrok link.
ngrok then generates a public URL. Open that URL in your browser to use the app.

