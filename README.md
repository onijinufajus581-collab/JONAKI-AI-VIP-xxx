# JONAKI AI Online — starter Android project

This project is a fresh Android voice-assistant starter with a separate online AI backend.

## Important capability boundaries

- The app can chat with the configured online AI backend.
- Bengali speech recognition and Android text-to-speech depend on speech services and voices installed on the phone.
- YouTube search opens a search URL.
- WhatsApp, TikTok and Facebook commands open their apps when installed.
- This starter does NOT silently send WhatsApp messages, upload TikTok videos, or click arbitrary controls in other apps. Those actions require app-specific supported flows and user review/confirmation.
- Voice listening is active only while the app is in use. It is not an always-on background microphone service.

## 1. Deploy the backend

Deploy the `backend` folder as a Node.js web service on a trusted HTTPS hosting provider.

Set these environment variables in the hosting provider's secret/environment settings:

- `OPENAI_API_KEY`: your private OpenAI API key
- `OPENAI_MODEL`: `gpt-4.1-mini` (or a model available to your API project)
- `JONAKI_APP_TOKEN`: a long random secret shared only with your app/backend

Never put `OPENAI_API_KEY` in Android source code, GitHub commits, or the APK. OpenAI keys must remain on the server.

Install and run locally:
```bash
cd backend
npm install
OPENAI_API_KEY="your-key" JONAKI_APP_TOKEN="your-long-random-token" npm start
```

## 2. Configure the Android app

Edit `app/build.gradle` and replace:
```groovy
buildConfigField "String", "BACKEND_URL", "\"https://YOUR-BACKEND-DOMAIN.example\""
```
with your deployed HTTPS backend URL.

In `MainActivity.java`, add this header to the `/chat` request after `setRequestProperty("Content-Type"...):`
```java
c.setRequestProperty("x-jonaki-token", "YOUR_JONAKI_APP_TOKEN");
```
For production, do not hardcode a shared token in a public APK. Use user authentication and short-lived server-issued tokens instead. This simple starter token is only suitable for private testing.

## 3. Build

Push the project to GitHub. GitHub Actions builds a debug APK. Open Actions, select the successful run, and download the `JONAKI-AI-APK` artifact.

## API cost

Online AI usage may incur charges according to your API provider and account. Set usage limits and monitor usage.

## Permissions

The app requests microphone permission when needed. Android may require the user to approve app permissions and confirm actions inside other apps.
