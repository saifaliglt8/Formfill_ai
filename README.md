# FormFill AI — Chrome Extension

> **Intelligently autofills job and internship application forms from your CV using Google Gemini AI.**
> Built for students and job seekers who want to spend less time copy-pasting and more time on what matters.

---

## What it does

FormFill AI reads your CV once, builds a structured profile, and then fills application forms for you — name, email, phone, address, education, work experience, skills, LinkedIn, GitHub, cover letter, and more.

It works on standard HTML forms, Google Forms, Oracle HCM, Workday, Greenhouse, Lever, and most other job application platforms. You review every suggested answer before anything is applied, so you stay in control at all times.

---

## Screenshots

> *(Add your own screenshots here after installation — recommended: popup scan view, review panel, and a filled form)*

---

## Features

- **CV upload** — PDF, DOCX, or TXT. Text is extracted locally on your machine.
- **AI profile building** — Gemini AI structures your CV into a clean profile (name, email, phone, address, work history, education, skills, links, certifications, languages).
- **Local fallback parser** — Works even without an API key or when quota is exceeded. Pure local regex extraction at zero cost.
- **Smart field matching** — Local heuristic engine resolves ~70% of fields instantly (no API call). Only genuinely ambiguous fields are sent to Gemini.
- **Response caching** — Gemini responses are cached for 24 hours so you don't burn quota re-scanning the same form.
- **Broad platform support** — Standard HTML, Google Forms, Oracle HCM, Workday, Greenhouse, Lever, Taleo, and more.
- **iframe support** — Works on forms that load inside nested iframes (Oracle, some Workday configurations).
- **Review panel** — Edit, skip, or approve every suggested answer before applying. Confidence scores shown per field.
- **SPA support** — MutationObserver watches for new fields added dynamically. Works on React, Vue, and Angular apps.
- **Never auto-submits** — The extension fills fields. You submit. Always.
- **Dark mode** — UI adapts to your system theme automatically.
- **Free to use** — Only cost is your Gemini API calls (free tier: 1,500 requests/day).

---

## Supported platforms

| Platform | Support level |
|---|---|
| Standard HTML forms | ✅ Full |
| Google Forms | ✅ Full |
| Oracle HCM (`oraclecloud.com`) | ✅ Full |
| Workday (`myworkdayjobs.com`) | ✅ Full |
| Greenhouse (`boards.greenhouse.io`) | ✅ Full |
| Lever (`jobs.lever.co`) | ✅ Full |
| LinkedIn Easy Apply | ✅ Full |
| Ashby HQ | ✅ Full |
| Taleo | ✅ Full |
| SmartRecruiters | ✅ Full |
| File upload fields (resume attachment) | ❌ Not possible — browser security restriction |

---

## Installation (no coding required)

### Step 1 — Download

Download the latest release from the [Releases page](../../releases) and unzip it. You will get a folder called **`formfill-ai-fixed`**.

Put this folder somewhere permanent — your Desktop, Documents, or a dedicated folder. **Do not move or delete it after loading** — Chrome reads the extension files from this folder directly.

### Step 2 — Open Chrome Extensions

In Chrome, type this in the address bar and press Enter:

```
chrome://extensions
```

### Step 3 — Enable Developer Mode

In the top-right corner of the Extensions page, toggle **Developer mode** to **ON**.

Three buttons will appear: *Load unpacked*, *Pack extension*, and *Update*.

### Step 4 — Load the extension

Click **Load unpacked**.

A file picker opens. Navigate to the `formfill-ai-fixed` folder you unzipped in Step 1 and click **Select Folder** (Windows/Linux) or **Open** (Mac).

> ⚠️ Select the folder itself — not a file inside it. The folder must contain `manifest.json` directly inside it.

The FormFill AI card appears in the Extensions list. If there is a red error badge, see the Troubleshooting section below.

### Step 5 — Pin to toolbar

Click the puzzle-piece icon 🧩 in the Chrome toolbar. Find **FormFill AI** and click the pin icon. The extension icon now stays in your toolbar.

---

## Setup (first time)

### Get a free Gemini API key

1. Go to [https://aistudio.google.com/app/apikey](https://aistudio.google.com/app/apikey)
2. Sign in with any Google account
3. Click **Create API key**
4. Copy the key — it starts with `AIza` and is about 39 characters

> The API key is stored locally on your device only. It is never shared with anyone except Google's API.

### Add the key to the extension

1. Right-click the FormFill AI icon → **Options** (or go to `chrome://extensions` → FormFill AI → Details → Extension options)
2. Click the **API Key** tab
3. Paste your key into the field
4. Select a model from the dropdown:

| Model | Free tier | Recommendation |
|---|---|---|
| `gemini-1.5-flash` | 1,500 req/day | ✅ Best choice for most users |
| `gemini-1.5-flash-8b` | 1,500 req/day | Faster, slightly less accurate |
| `gemini-1.5-pro` | 50 req/day | Most accurate, very limited |
| `gemini-2.0-flash` | Paid only | ❌ Will hit quota on free tier |

5. Click **Save key**
6. Click **Test connection** — you should see a green "✓ API key is valid" message

### Upload your CV

1. Click the **Profile** tab in the Settings sidebar
2. Click **Choose file** or drag your CV onto the upload zone
3. Supported formats: **PDF**, **DOCX**, **TXT**
4. Wait 5–15 seconds — the extension extracts your text locally, then Gemini structures it
5. A banner confirms whether AI or local parsing was used
6. Review every field — correct anything wrong
7. Click **Save profile**

> **No API key?** The extension still works using the local parser. It extracts your name, email, phone, address, LinkedIn, GitHub, skills, education, and experience using pattern matching. Results are slightly less polished but fully functional. Add your key later for better accuracy.

---

## Using the extension

### Filling a form

1. Navigate to any job or internship application page
2. Make sure the form is fully loaded and visible on screen
3. Click the **FormFill AI** icon in the toolbar
4. Click **Scan this page**
5. The popup shows detected fields with suggested answers and confidence percentages
6. Click **Review all** to open the full side panel — edit any answer, skip fields you want to fill manually
7. Click **Apply to form** — fields fill with a brief green highlight

### Understanding confidence scores

- 🟢 **80–100%** — High confidence. The field matched a clear profile key.
- 🟡 **60–79%** — Medium confidence. Worth a quick glance.
- 🔴 **Below 60%** — Low confidence. Review this field carefully before applying.

Fields below the confidence threshold (default 70%) are highlighted in amber in the Review panel.

### Behavior settings

Go to **Options → Behavior** to configure:

- **Skip already filled fields** — Don't overwrite values already on the page
- **Show confidence scores** — Display % next to each suggestion
- **Confidence threshold** — Fields below this % are flagged for review (40–95%)

---

## How it works — technical overview

```
Your CV (PDF/DOCX/TXT)
    │
    ▼
Local text extraction (pdfjs / mammoth / FileReader)
    │
    ▼
Local regex parser — extracts name, email, phone, address,
LinkedIn, GitHub, skills, experience, education (free, instant)
    │
    ▼ (if API key present)
Gemini AI — enriches and corrects the local result
    │
    ▼
Profile stored in chrome.storage.local
    │
    ▼
User clicks Scan on a form page
    │
    ▼
Field detector — reads DOM labels, aria-label, aria-labelledby,
aria-describedby, placeholder, name/id attributes, nearby text
    │
    ├── Standard fields (input, select, textarea)
    ├── Google Forms (container-based detection)
    ├── Oracle HCM (oj-* components, data-bind)
    └── Workday (data-automation-id attributes)
    │
    ▼
Local heuristic matcher — resolves ~70% of fields instantly
    │
    ▼ (remaining unmatched fields only)
Gemini AI field mapper — single batched API call, result cached
    │
    ▼
Review panel — user edits/approves answers
    │
    ▼
Autofill engine — fills fields, fires input/change/blur events
(React native setter, ARIA textbox, contenteditable, combobox)
```

### Architecture

```
formfill-ai-fixed/
├── manifest.json              Chrome extension manifest (MV3)
├── popup.html / popup.css     Toolbar popup UI
├── options.html / options.css Settings page (profile, API key, behavior)
├── review-panel.html / .css   Side panel for reviewing answers
└── dist/
    ├── background.js          Service worker — orchestrates all AI calls
    ├── content.js             Injected into every page — detects and fills fields
    ├── popup.js               Popup controller
    ├── review-panel.js        Review panel controller
    ├── options.js             Settings page controller
    └── pdf.worker.mjs         PDF.js worker for PDF text extraction
```

---

## Privacy

- Your CV text and profile are stored only in `chrome.storage.local` on your own device
- The API key is stored locally and sent only to `generativelanguage.googleapis.com` (Google's API)
- CV text is sent to Gemini only once during profile creation, and only if you have an API key set
- Form field labels are sent to Gemini only for fields the local matcher couldn't resolve
- No data is sent to any server other than Google's Gemini API
- No analytics, no tracking, no third-party services

---

## Troubleshooting

**"No active tab" when clicking Scan**

Click directly on the application form tab first to make sure it's the active tab, then click the FormFill AI icon and scan. If the issue persists, refresh the form page and try again.

**"No form fields detected" on a visible form**

Some pages load form fields after a delay (SPAs, lazy loading). Scroll to make the form visible, wait 2–3 seconds, then click Scan again. The extension watches for dynamically added fields automatically.

**Form fields fill but immediately revert (React forms)**

This happens on some heavily controlled React inputs. The extension fires the correct native event sequence, but some React versions still revert. Try clicking the field manually first, then click Apply — or use the Review panel to manually paste values.

**PDF upload shows an error**

Save your CV as a `.txt` file first: open it in any PDF reader, Select All, copy, paste into Notepad or TextEdit, save as `.txt`. Upload the `.txt` version to build your profile.

**Gemini quota exceeded (429 error)**

Switch the model in Settings → API Key to `gemini-1.5-flash` which has 1,500 free requests per day. The extension will also fall back to local parsing automatically and show a yellow warning banner — your profile will still be created, just without AI enrichment.

**API Key section is blank in Settings**

Click the **API Key** text in the left sidebar to navigate to that section. If the section still doesn't appear, remove and reload the extension from `chrome://extensions`.

**Extension disappeared from toolbar**

Chrome keeps the extension installed but removes the pin on restart. Click the puzzle piece 🧩 and pin it again.

**After reloading the extension, forms still say "could not connect"**

After reloading the extension, you must also **refresh any open tabs** that you want to scan. The new content script only runs in tabs opened or refreshed after the reload.

---

## Building from source

If you want to modify the code yourself:

### Prerequisites

- [Node.js 18+](https://nodejs.org/)
- npm (comes with Node.js)

### Steps

```bash
# 1. Clone or download the repository
git clone https://github.com/YOUR_USERNAME/formfill-ai.git
cd formfill-ai

# 2. Install dependencies
npm install

# 3. Build the extension
npm run build

# 4. Load the project folder into Chrome (same as installation steps above)
```

For development with automatic rebuilding on save:

```bash
npm run dev
```

After each rebuild, go to `chrome://extensions` and click the reload icon on the FormFill AI card. Then refresh the tab you're testing on.

### Project structure (source)

```
src/
├── background/
│   └── service-worker.ts      Central message router, AI orchestrator
├── content/
│   ├── content-main.ts        Page entry point, MutationObserver, SPA nav
│   ├── field-detector.ts      DOM scanner for all form platforms
│   └── autofill-engine.ts     Fills fields, fires events, handles all types
├── popup/
│   ├── popup.html/ts/css       Toolbar popup
├── review/
│   ├── review-panel.html/ts/css   Side panel for reviewing answers
├── options/
│   ├── options.html/ts/css     Settings page
├── ai/
│   ├── cv-parser.ts           CV extraction + local parser + Gemini normalisation
│   └── gemini-helper.ts       Field mapping API calls with retry and caching
├── core/
│   ├── profile-schema.ts      All TypeScript types
│   ├── storage-helper.ts      chrome.storage wrappers
│   └── field-mapper.ts        Local heuristic matching (no API)
└── utils/
    ├── dom-utils.ts            Shared DOM helpers
    └── logger.ts               Dev logging
```

---

## Known limitations

- **Resume file upload fields** — `<input type="file">` cannot be filled programmatically. This is a hard browser security restriction. You must attach your resume file manually on every application.
- **CAPTCHA fields** — Not handled. Complete them manually.
- **Highly customised React forms** — Some Workday versions use proprietary event handling that reverts field values. If this happens, use the Review panel values as a reference and type them manually.
- **Chrome only** — This extension targets Chrome (Manifest V3). Firefox uses a different extension format and is not currently supported.

---

## Contributing

Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

If you find a form platform that isn't supported, open an issue with the URL (if public) and a description of the form structure. New platform support can usually be added in `src/content/field-detector.ts`.

---

## License

MIT License — see [LICENSE](LICENSE) for details.

---

## Acknowledgements

- [PDF.js](https://mozilla.github.io/pdf.js/) — PDF text extraction
- [Mammoth.js](https://github.com/mwilliamson/mammoth.js) — DOCX text extraction
- [Google Gemini API](https://ai.google.dev/) — AI field mapping and CV normalisation
- Built with TypeScript, Webpack, and Chrome Extensions Manifest V3

---

*Made for students and job seekers. Save your time for what actually matters.*
