# WriteLab — Austrian AHS B2 Matura Writing Practice

A browser-based writing practice app for 8th-class AHS English. Students can generate tasks for the six B2 text types and submit writing for structured AI feedback, highlighted language issues, a revised version and an estimated Austrian grade.

## Easiest hosting route (no terminal required)

This project is designed to be deployed as **one Render web service**. Render serves the HTML page and runs the small Node.js backend on the same address, so the review button works without configuring a separate GitHub Pages site. GitHub stores the project files; Render hosts the live app.

### 1. Upload the project to GitHub

1. Sign in at https://github.com and create a **Public** repository, e.g. `writelab-matura`.
2. Open the repository and choose **Add file → Upload files**.
3. Upload all project files while keeping their names and folder structure: `index.html`, `server.js`, `package.json`, and `README.md`.
4. Commit the upload.

### 2. Create the hosted app on Render

1. A teacher or parent should create/sign in to the hosting account at https://render.com.
2. In the Render dashboard choose **New → Web Service**.
3. Connect GitHub and select the `writelab-matura` repository.
4. Use these settings:
   - **Name:** `writelab-matura` (or another available name)
   - **Runtime:** Node
   - **Build Command:** `npm install`
   - **Start Command:** `npm start`
   - **Instance type:** Free, if available and suitable
5. Under **Environment Variables**, add:
   - `GEMINI_API_KEY` = the private Gemini API key created and managed by the teacher or parent
   - `GEMINI_MODEL` = `gemini-3.5-flash` (default; change only to a model enabled for your Gemini API key)
   - `CLASS_ACCESS_CODE` = a class-only code chosen by the teacher (not a personal password)
   - Optional `DAILY_REVIEW_LIMIT` = `100`
   - Optional `PER_IP_HOURLY_LIMIT` = `5`
6. Create the Web Service and wait for the first deploy to finish.
7. Open the `onrender.com` URL shown in the Render dashboard. Share that one link and the class access code with students.

The app and API are served from the same domain. **Do not create a GitHub Pages site for this full-stack version**; GitHub Pages cannot run the Node backend.

### 3. Updating the app

When you upload a changed file to the same GitHub repository and commit it, Render can automatically redeploy the service.

## Important safety, privacy and cost notes

- **Adult setup:** An adult (teacher or parent) should own the hosting account, create/manage the API key and review billing. API use may cost money; set a spending limit/budget in the Google AI Studio usage dashboard where available. ChatGPT subscriptions and API billing are separate.
- **Keep the key private:** Put `GEMINI_API_KEY` only in Render's Environment settings. Never paste it into `index.html`, GitHub, a screenshot or a class chat.
- **Class access code:** The optional class code is checked by the server. Students need it to submit writing. It is not a replacement for strong abuse controls, so the server also applies basic per-IP hourly and whole-app daily limits. These counters are in memory and reset when the service restarts; they are lightweight safeguards, not a guarantee against all abuse.
- **Student privacy:** Tell students that their submitted text is sent to Google Gemini for feedback. Google’s Gemini API free tier may use submitted content to improve Google products; the teacher should review the current data terms and school policy before using real student work. They should not include full names, email addresses, phone numbers, school IDs or other personal/sensitive information. The app does not intentionally save submitted essays to a database. The hosting provider and AI provider may process request data under their own terms and retention settings; the teacher should review those before classroom use.
- **Free hosting:** Free hosting plans can sleep when unused, have usage limits and may change. The first visit after inactivity can take longer. Check current Render plan details before relying on it for a lesson.
- **Grades:** The 1–5 grade is an AI-generated practice estimate, not an official Matura result or a teacher's grade.

## Local development (optional)

Requires Node.js 20+. Set `GEMINI_API_KEY`, then run `npm start` and open http://localhost:3000.

## Contents

- `index.html` — student-facing app.
- `server.js` — Node.js server and secure server-side AI request.
- `package.json` — start script and runtime metadata.

The app uses the Austrian B2 AHS text types: essay, email, report, article, blog post and blog comment.
