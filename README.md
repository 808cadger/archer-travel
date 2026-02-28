# Archer Travel Service — Evolution Travel

A mobile-first, single-file web app for travel booking with AI-powered identity verification. Built with vanilla HTML, CSS, and JavaScript, using [Claude Vision](https://anthropic.com) to match a selfie against a government-issued ID.

---

## Features

- **Travel Application Form** — Personal details, trip type, destination, traveler counts, budget, accommodation preference
- **AI Identity Verification** — Camera capture of government ID + selfie, compared by Claude Vision API
- **Demo Mode** — Works without an API key using a simulated verification result
- **No Build Step** — Single HTML file, zero dependencies, runs in any modern browser

---

## Installation

### Option 1 — Open directly in a browser (simplest)

1. Save `index.html` (or the app file) to your computer.
2. Double-click the file to open it in your browser.

> **Note:** Camera access requires the page to be served over HTTPS or `localhost`. If the camera doesn't work when opening the file directly, use Option 2.

---

### Option 2 — Serve locally (recommended for camera access)

**Using Python (no install needed on macOS/Linux):**

```bash
# Python 3
python3 -m http.server 8080
```

Then open [http://localhost:8080](http://localhost:8080) in your browser.

**Using Node.js:**

```bash
npx serve .
```

Then open the URL shown in the terminal.

**Using VS Code:**

Install the [Live Server](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer) extension, right-click `index.html`, and select **Open with Live Server**.

---

### Option 3 — Deploy to the web

Because the app is a single HTML file, you can host it on any static hosting provider:

| Platform | Steps |
|----------|-------|
| **Vercel** | `npx vercel` in the project folder |
| **Netlify** | Drag and drop the HTML file at [app.netlify.com/drop](https://app.netlify.com/drop) |
| **GitHub Pages** | Push to a repo, enable Pages under **Settings → Pages** |

---

## AI Verification Setup (Optional)

The app uses [Claude Vision](https://docs.anthropic.com/en/api) to compare the user's selfie against their ID photo.

### Get an API Key

1. Sign up at [console.anthropic.com](https://console.anthropic.com)
2. Go to **API Keys** and create a new key

### Add the Key

You can add the key in two ways:

**In the app (runtime):** Paste your key into the **"Anthropic API Key"** field on the application form. It is stored in `localStorage` and never sent to any server other than `api.anthropic.com`.

**Hard-coded (optional):** Open the HTML file and find this line:

```js
const apiKey = localStorage.getItem('archer_apikey') || document.getElementById('api-key').value;
```

Replace `document.getElementById('api-key').value` with your key string for automatic use.

### Demo Mode (no key required)

Leave the API key field blank. The app will simulate a successful verification with a randomly generated confidence score (88–98%).

---

## How It Works

| Step | Description |
|------|-------------|
| **1 — Application** | User fills in personal info, trip details, traveler counts, and budget |
| **2 — ID Verification** | User photographs their ID (passport or driver's license) and takes a selfie using the device camera or file upload |
| **3 — AI Analysis** | Images are sent to Claude Vision (`claude-sonnet-4-6`) which returns a JSON match result including confidence score |
| **4 — Confirmation** | On success, a reference number is generated and booking summary is displayed |

---

## Browser Compatibility

| Feature | Requirement |
|---------|-------------|
| Camera access | HTTPS or `localhost` |
| File upload fallback | All modern browsers |
| CSS variables / Grid | Chrome 60+, Safari 10+, Firefox 55+ |

---

## Project Structure

```
archer-travel/
└── index.html    # Complete app — HTML, CSS, and JS in one file
```

---

## API Reference

The app makes a single API call to Anthropic:

```
POST https://api.anthropic.com/v1/messages
Model: claude-sonnet-4-6
```

**Prompt:** Asks Claude to compare two images (ID document + selfie) and return:

```json
{
  "match": true,
  "confidence": 94,
  "face_detected_id": true,
  "face_detected_selfie": true,
  "id_type_detected": "drivers_license",
  "notes": "Facial features match across both images with high confidence."
}
```

---

## Privacy & Security

- Photos are processed in the browser and sent **only** to `api.anthropic.com` — never to any other server
- The API key is stored in `localStorage` on the user's device only
- No data is retained after the session ends
- All form data stays client-side

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| Camera shows blank / permission denied | Open via `localhost` or HTTPS, and allow camera access when prompted |
| "API error 401" | Check your Anthropic API key is correct and has available credits |
| File upload not working | Ensure the browser supports `FileReader` (all modern browsers do) |
| App looks broken | Use a modern browser (Chrome, Safari, Firefox, Edge) |
