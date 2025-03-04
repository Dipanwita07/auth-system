
# User Authentication System

This is a backend project for user authentication with signup, login, and password reset features. It uses Node.js, Express, MongoDB, and JWT tokens. Here’s how to run it on your computer.

---

## What You Need
- **Node.js**: Get it from `https://nodejs.org/` (I used version 20+).
- **MongoDB**: Download from `https://www.mongodb.com/try/download/community` (local server).
- **Postman**: Grab it from `https://www.postman.com/downloads/` (for testing).
- A **Gmail account** (set up in `.env` for password reset emails).

---

## How to Run It

Follow these steps to get it working. I’ve made this as easy as possible!

### Step 1: Open the Project
1. Open Command Prompt.
2. Go to your project folder:
   ```
   cd path\to\your\auth-system
   ```
   - Replace `path\to\your\auth-system` with where your folder path.

### Step 2: Set Up the Project
1. Type this in Command Prompt and press Enter:
```
npm init -y
```
- This makes a `package.json` file to track the project.

### Step 3: Install the Packages
1. Type this in Command Prompt and press Enter:
   ```
   npm install express mongoose bcryptjs jsonwebtoken express-validator nodemailer dotenv
   ```
2. Wait—it’ll grab all the stuff like Express and MongoDB tools. You’ll see a `node_modules` folder when it’s done.

### Step 4: Check Your `.env` File
1. Open the `.env` file in your project folder with Notepad or any editor.
2. Make sure it looks like this:
   ```
   PORT=5000
   MONGO_URI=mongodb://localhost:27017/authdb
   JWT_SECRET=your-secret-key
   EMAIL_USER=your-email@gmail.com
   EMAIL_PASS=your-email-password
   ```
3. Double-check these:
   - `your-secret-key`: Can be anything (like `mysecret123`).
   - `your-email@gmail.com`: Your real Gmail address.
   - `your-email-password`: Your Gmail password.
     - **If you have 2-Step Verification**: Go to `myaccount.google.com` > “Security” > “App passwords”, make one for “Mail”, and use that 16-character code instead (no spaces).


### Step 5: Start MongoDB
1. Open a new Command Prompt.
2. Type this and press Enter:
   ```
   mongod
   ```
3. Leave it running—it’s your database. If it says “waiting for connections on port 27017”, it’s good.

### Step 5: Run the App
1. Go back to the Command Prompt in your project folder.
2. Type this and press Enter:
   ```
   node server.js
   ```
3. You should see:
   ```
   MongoDB connected
   Server running on port 5000
   ```

---

## Testing It
Use Postman or any tool to try it out. Here’s how I test it:

### 1. Sign Up
1. Open Postman 
2. Click “New” (top left) > “HTTP Request”.
3. Set the dropdown next to the URL bar to `POST`.
4. Type this URL: `http://localhost:5000/api/auth/register`.
5. Click the “Body” tab below the URL.
6. Click “raw” (it’s next to options like “form-data”).
7. Change the dropdown next to “raw” from “Text” to “JSON”.
8. In the text box, type this exactly:
   ```
   {"email": "test@example.com", "password": "123456"}
   ```
9. Click the “Send” button.
10. At the bottom, you’ll see:
    ```
    {"token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."}
    ```
    - That’s your JWT token—signup worked!

### 2. Log In
1. Click “New” > “HTTP Request”.
2. Set it to `POST`.
3. Type this URL: `http://localhost:5000/api/auth/login`.
4. Click “Body” tab.
5. Click “raw” and set it to “JSON”.
6. Type this:
   ```
   {"email": "test@example.com", "password": "123456"}
   ```
7. Click “Send”.
8. You’ll get:
   ```
   {"token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."}
   ```
   - Another token means login’s good.

### 3. Reset Password (Step-by-Step)
#### Part 1: Request the Reset Email
1. Click “New” > “HTTP Request”.
2. Set it to `POST`.
3. Type this URL: `http://localhost:5000/api/auth/reset-password`.
4. Click “Body” tab.
5. Click “raw” and set to “JSON”.
6. Type this:
   ```
   {"email": "test@example.com"}
   ```
7. Click “Send”.
8. You’ll see:
   ```
   {"msg": "Password reset email sent"}
   ```

#### Part 2: Get the Token from Gmail
1. Go to `https://mail.google.com`.
2. Log in with the Gmail from your `.env` file (`EMAIL_USER`).
3. Check your inbox—or spam folder.
4. Look for an email titled “Password Reset”.
5. Open it—it’ll have a link like:
   ```
   http://localhost:5000/api/auth/reset/eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
   ```
6. Copy the long string after `/reset/` (starts with `eyJhbGci...`)—that’s the reset token.

#### Part 3: Use the Token to Reset Password
1. In Postman, click “New” > “HTTP Request”.
2. Set it to `POST`.
3. Type this URL, but swap `TOKEN_HERE` with the token you copied:
   ```
   http://localhost:5000/api/auth/reset/TOKEN_HERE
   ```
   - Example: `http://localhost:5000/api/auth/reset/eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...`.
4. Click “Body” tab.
5. Click “raw” and set to “JSON”.
6. Type this:
   ```
   {"password": "newpassword123"}
   ```
7. Click “Send”.
8. You’ll see:
   ```
   {"msg": "Password reset successful"}
   ```
9. To confirm, test login again:
   - New `POST` to `http://localhost:5000/api/auth/login`.
   - Body (raw, JSON): `{"email": "test@example.com", "password": "newpassword123"}`.
   - Send it—you’ll get a token, proving the reset worked!

