# KGB — Kaya’s greatest Bildungsbegleiter

**Slogan:** “definitely not vibecoded at all trust me I’m super smart and can write html code”

KGB is a browser-based English writing practice app for 8th-class AHS students. It generates tasks for the six B2 text types and provides structured AI feedback, highlighted language issues, a revised version and an estimated Austrian grade.

## Features

- Practice tasks for essay, email, report, article, blog post and blog comment.
- AI assessment against the four Austrian B2 writing dimensions.
- Four selectable themes: Default (classic Facebook / iOS 4), Super kawaii Y2K, Frutiger Aero / Windows Vista and Cyberpunk.
- Theme choice is saved locally in the user's browser.
- Optional class password window: when `CLASS_ACCESS_CODE` is set on Render, students must enter the teacher-provided code before using KGB. The code is checked by the server and is sent with both transcription and review requests.
- Handwritten essay upload: choose up to five page photos, transcribe them into editable text, proofread the transcription, and then submit it for the usual review.
- Review progress indicator: shows an animated activity bar and live elapsed time while the AI grades a text. It does not invent a completion percentage because AI response times vary.
- Temporary Gemini overload/rate-limit responses are retried automatically up to two times with a short wait between attempts. The app then displays a clearer message if the service is still busy.
- The transcription step preserves the student's original language and wording; it does not translate or correct the essay.

## Deploy / update on Render

This is a single Node.js Web Service. Render serves the page and the API from the same address. GitHub stores the source; Render runs the app.

1. Download and unzip this project.
2. In the GitHub repository connected to Render, replace the existing `index.html`, `server.js`, `package.json` and `README.md` with the four files from this ZIP. Upload the files to the repository root (not inside an extra nested folder).
3. Commit the changes. Render should automatically redeploy the Web Service.
4. Open the existing `https://....onrender.com` address and test the app. Do not use a GitHub Pages `github.io` address for the full-stack app.

Keep the existing Render environment variables, especially `GEMINI_API_KEY`, `GEMINI_MODEL` (if already set), and `CLASS_ACCESS_CODE` (if used). The code uses the model configured in `GEMINI_MODEL`; do not change a working model setting just for this update.

## Environment

- `GEMINI_API_KEY`: required; store it only in Render's private Environment settings.
- `GEMINI_MODEL`: the Gemini model identifier already configured for the working app. If unset, the server uses its existing fallback.
- `CLASS_ACCESS_CODE`: optional server-side class code. When set, KGB displays its password window on opening and validates the code before unlocking the app. Leave it unset to make the app publicly accessible.
- `DAILY_REVIEW_LIMIT`: optional, defaults to 100 AI requests per day.
- `PER_IP_HOURLY_LIMIT`: optional, defaults to 5 AI requests per IP per hour.

No extra npm packages are required. Node.js 20+ is required.

## AI availability and review progress

- A grading request can take longer when Gemini is busy. KGB displays an animated progress indicator and elapsed time; it cannot know the exact remaining time.
- For temporary Gemini overload or rate-limit responses, the server retries automatically twice. Repeated failures produce a message asking the student to wait and try again.
- If the message mentions an API quota or billing limit, a teacher/adult should check the Gemini API project usage, quota and billing settings. Repeated retries cannot fix an exhausted quota.
- KGB keeps the configured `GEMINI_MODEL` unchanged. The model must be available to the configured API key.

## Handwriting upload notes

- Upload up to five photos in page order. The browser resizes and converts photos to JPEG before sending them to the server.
- Use clear, well-lit, straight-on photos with the whole page visible.
- KGB shows the transcription in the writing box. Students should check and correct it before clicking **Review my writing**.
- Photos and transcribed text are sent to the configured Gemini API for processing. The app does not intentionally save them in a database. Review the AI provider's current data terms and the school's rules before using real student work. Avoid names, contact details, school IDs or other sensitive information.
- The AI can misread handwriting. The transcription is a draft, not a verified copy.

## Privacy, billing and grading

- An adult (teacher or parent) should own the hosting account, manage the API key and review billing. API usage may cost money; set suitable usage limits.
- Never put the API key in `index.html`, GitHub or a class chat.
- The hosting provider and AI provider may process request data under their own terms and retention settings. The teacher should review these and school policy before classroom use.
- The 1–5 grade is an AI-generated practice estimate, not an official Matura result or a teacher's grade.

## Troubleshooting HTTP 405

A 405 response usually means the browser sent a POST request to a static host such as GitHub Pages or a Render Static Site. The page and API must run together on the Render **Web Service** URL (`onrender.com`). The review endpoint is `/api/review`; handwriting transcription uses `/api/transcribe`.
