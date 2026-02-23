# Best Outgoing Student - Setup Guide for New Instances

This repository contains the clean, production-ready source code for the "Best Outgoing Student" application form and faculty evaluation portal.

Follow these steps to deploy this application with a **new** Google Service Account, **new** Google Sheets database, and under your college's **new** GitHub repository.

---

## 1. Prerequisites

1. **A New Google Sheet**
   - Create a blank Google Sheet.
   - Extract the `SHEET_ID` from the URL.
     *(e.g., `https://docs.google.com/spreadsheets/d/YOUR_SHEET_ID_HERE/edit`)*

2. **A Google Service Account**
   - Go to the [Google Cloud Console](https://console.cloud.google.com/).
   - Create a new project and enable the **Google Sheets API**.
   - Create a **Service Account** and generate a new **JSON key**.
   - **Crucial Step:** Open your new Google Sheet, click "Share", and share the sheet as an "Editor" with the `client_email` found inside your Service Account JSON key.

## 2. Environment Configuration

1. In the root of this project, create a `.env.local` file (do not commit this file to GitHub).
2. Populate it with the following values:

```env
# Google Sheets Integration
GOOGLE_SERVICE_ACCOUNT_EMAIL="your-service-account@your-project.iam.gserviceaccount.com"
GOOGLE_PRIVATE_KEY="-----BEGIN PRIVATE KEY-----\nYour\nVery\nLong\nKey\nHere\n-----END PRIVATE KEY-----\n"
GOOGLE_SHEET_ID="YOUR_SHEET_ID_HERE"

# Admin Portal Authentication
ADMIN_PASSWORD="ChooseAStrongAdminPassword123"
SESSION_SECRET="GenerateARandomSecretStringHere"

# Application Settings
NEXT_PUBLIC_APP_URL="http://localhost:3000"
```
*(Make sure to properly format the private key with `\n` replacing line breaks if you inline it, or quote the multiline string).*

## 3. Initializing the Database (Google Sheets Data Seeding)

The application automatically manages its own tables, but you need to seed the initial column headers.

1. **Install dependencies and start the app locally:**
   ```bash
   npm install
   npm run dev
   ```

2. **Initialize Custom Award Sheets:**
   Open your browser and navigate to:
   `http://localhost:3000/api/seed-awards?headersOnly=true`
   *(This will connect to your Google Sheet and automatically create 6 new tabs with the correct column headers for every specialized award).*

3. **Initialize the Main Form Sheet (`BO_Main`):**
   Navigate to the Admin portal:
   `http://localhost:3000/admin`
   Log in with your `ADMIN_PASSWORD`.
   *(Once the admin portal loads, it will detect that the main Best Outgoing database is missing and automatically generate the `BO_Main` tab with over 40 column headers).*

You can now open your Google Sheet and verify that all 7 tabs have been created successfully.

## 4. Pushing to your College GitHub Repository

Once you have verified the setup works locally:

1. Create a new empty repository on your college's GitHub account.
2. Initialize and push this folder:
   ```bash
   git init
   git add .
   git commit -m "Initial commit for Best Outgoing Student Application"
   git branch -M main
   git remote add origin https://github.com/YOUR_COLLEGE_ORG/YOUR_REPO_NAME.git
   git push -u origin main
   ```

## 5. Deployment

You can seamlessly deploy this application using [Vercel](https://vercel.com/):
1. Connect your Vercel account to the new GitHub repository.
2. During the Vercel project setup, copy all 5 variables from your `.env.local` into Vercel's **Environment Variables** section.
3. Click **Deploy**.

Your Best Outgoing Student portal is now fully isolated, secure, and ready for applicants!
