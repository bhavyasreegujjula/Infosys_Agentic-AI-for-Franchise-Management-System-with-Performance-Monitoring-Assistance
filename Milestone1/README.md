# FRANCHISE ANALYTICS AND MANAGEMENT

## Overview

Milestone 1 is the first complete working version of the Infosys Franchise Analytics & Management authentication and dashboard application.

This milestone focuses on building a secure login system inside a single Streamlit app. It supports user registration, login, password recovery, JWT-based session handling, a user analytics dashboard, and a separate admin dashboard for managing registered users. The authentication module serves as the foundation for future milestones by providing secure user management and access control.

The application is designed to run in Google Colab and is exposed publicly using ngrok.

## Features

The application includes three main public pages and two protected dashboards:

1. **Login Page**
   - Sign in using either username or email address.
   - Generates a JWT session token stored in Streamlit session state upon successful authentication.
   - Displays a single generic error message on failure to prevent user enumeration.

2. **Signup Page**
   - Register new user accounts (Username, Email, Password, Confirm Password, Security Question, Security Answer).
   - Enforces unique usernames/emails and prevents registration of reserved admin usernames.
   - Hashes passwords and security answers securely before database storage.

3. **Forgot Password Page**
   - **Security Question Reset:** Select security question route, answer the stored security question, and set a new password.
   - **OTP Reset:** Receive a 6-digit verification OTP sent via Gmail SMTP and verify before setting a new password.

4. **User Dashboard**
   - Displays analytics widgets including Documents Indexed, Searches Today, Efficiency Score, Security Status, and a System Health Index gauge chart.

5. **Admin Dashboard**
   - Dedicated administrative access (`admin` / `Admin@123`).
   - Displays a clean list of registered users (Username and Email only) with zero exposure of sensitive password hashes or security credentials.
****Admin credentials:****

Username: admin
Password: Admin@123
The admin account is not created from the Signup page.
After admin login, the Admin Dashboard displays all registered users with only:
Username
Email

The admin dashboard never displays password data, password hashes, security answers, OTP values, or JWT tokens.

## Validation & Security Rules

- **Mandatory Fields:** Strict frontend validation across all authentication forms.
- **Email Format Rule:** Requires standard email structure (`ab@cd.ef`).
- **Password Policy:** Minimum 8 characters with at least 1 uppercase letter, 1 lowercase letter, 1 number, and 1 special character (e.g., `Bhavya@123`). Prevents reuse of current passwords during resets.
- **JWT Session Handling:** Required for accessing protected dashboards. Invalid or missing tokens automatically redirect to the Login page.


****Email Format Rule:****

The email must follow this structure:
At least 2 letters before @
At least 2 letters between @ and .
At least 2 letters after the final .

Example:
ab@cd.ef

****Password Policy:****

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


**Tech Stack Used:**

## 🛠️ Technology Stack

| Technology       | Purpose                                                                                                |
| ---------------- | ------------------------------------------------------------------------------------------------------ |
| **Python**       | Backend programming language used to implement the authentication logic and application functionality. |
| **Streamlit**    | Used to build the interactive web application and user interface.                                      |
| **HTML5**        | Used to create the structure and content of web pages.                                                 |
| **Bootstrap**    | Used to build a responsive and modern user interface.                                                  |
| **SQLite**       | Database used to securely store user information and authentication data.                              |
| **bcrypt**       | Used to hash and verify user passwords securely before storing them in the database.                   |
| **PyJWT**        | Used to generate and validate JSON Web Tokens (JWT) for secure authentication and session management.  |
| **SMTP (Gmail)** | Used to send OTP emails for password recovery and verification.                                        |
| **Plotly**       | Used to create interactive charts and visualizations for the dashboard.                                |
| **pyngrok**      | Used to expose the local application through a secure public URL when running in Google Colab.         |


**Project Structure:**

How To Run The Notebook:

****Step 1****: Install Dependencies

Run this cell in Google Colab:

!pip install -q streamlit streamlit-option-menu pyngrok pyjwt bcrypt plotly

****Step 2****: Create the Streamlit App


Run the notebook cell that writes the full Streamlit application into app.py.
The cell should start with:

%%writefile app.py

****Step 3****: Add Colab Secrets


Open the Secrets tab in Google Colab and add:

Secret Name             Value
JWT_SECRET            Random signing key for JWT session tokens
NGROK_AUTHTOKEN       Your Authtoken 
EMAIL_PASSWORD        Gmail App Password 
EMAIL_ADDRESS         Gmail address that sends the OTP


****Step 4****: Start the App


Run the Streamlit/ngrok cell. The app runs on port 8501, and ngrok generates a public URL.
Open the generated URL in your browser.
Login Details
Admin Login
Username: admin
Password: Admin@123
Normal User Login
Normal users must first create an account from the Signup page. After signup, they can log in using either their username or email address.


**Screenshots**:

All screenshots are stored inside the screenshots folder.

****Login Page:****
Registered users can securely log into the application.

<img width="1863" height="897" alt="Screenshot (99)" src="https://github.com/user-attachments/assets/c1b4c14c-df5e-4c6b-8b79-a5801ae7df32" />



****Signup Page:****
This page allows new users to create an account by entering their registration details.

<img width="1743" height="876" alt="Screenshot (100)" src="https://github.com/user-attachments/assets/eff59b8f-98ac-418a-9f58-74ca6c14d26f" />

****Forgot Password :****


****Forgot Password - Security Question Route****

Users verify the security question they gave at the time of account creation for resetting their password.

<img width="1768" height="903" alt="Screenshot (103)" src="https://github.com/user-attachments/assets/a23d01b4-a286-4efd-84ec-95452ff6c97d" />


****Forgot Password - OTP Route****

Users verify the OTP received through email before resetting their password.

<img width="1796" height="885" alt="Screenshot (101)" src="https://github.com/user-attachments/assets/46c64e0e-98d6-4397-a4ad-f11234fbee94" />

****OTP Email****

<img width="1526" height="758" alt="Screenshot (102)" src="https://github.com/user-attachments/assets/2f238ebc-35a1-44f1-b6b5-21c18317c541" />


****User Dashboard:****

Dashboard displayed after successful user authentication.

<img width="1901" height="910" alt="Screenshot (104)" src="https://github.com/user-attachments/assets/f056fa3f-447f-4f86-bfc6-222f6d3ea991" />


****Admin Dashboard:****

Separate dashboard providing administrative features and user management capabilities.

<img width="1920" height="929" alt="Screenshot (105)" src="https://github.com/user-attachments/assets/19306efd-93d3-4f9c-bd63-4708c1e20a48" />


## Security Notes

Passwords and security answers are stored strictly as bcrypt hashes.

Admin views restrict sensitive fields from being displayed in the UI.

Password resets enforce OTP/Security Question verification and forbid reusing current passwords.

Milestone 1 delivers a complete authentication and dashboard workflow. It includes secure signup, login, forgot password recovery using security question and OTP, JWT session handling, password validation, a user analytics dashboard, and a separate admin dashboard.
This milestone creates the foundation for future improvements such as advanced franchise analytics, user activity logs, database deployment, role permissions, and production hosting.
