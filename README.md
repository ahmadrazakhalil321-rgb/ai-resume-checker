# ai-resume-checker
# 📄 ATS Resume Checker

A Streamlit app that scores a resume for Applicant Tracking System (ATS) compatibility and suggests concrete improvements, powered by Google's Gemini Flash models.

## Features

- Upload a resume as **PDF, DOCX or TXT** (max 5 MB)
- Optional **job description** field for tailored keyword matching
- **ATS score (0-100)** with a weighted breakdown:
  Keywords & Relevance (30%), Content & Impact (25%), Formatting (20%), Structure (15%), Grammar & Clarity (10%)
- Strengths, weaknesses and **missing keywords**
- **Prioritized improvements** (high / medium / low) and before/after bullet rewrites
- Download the full report as Markdown
- Automatic fallback to other Gemini Flash models if one is unavailable

> The score is an AI estimate based on common ATS rules. No public tool can reproduce the exact scoring of every employer's ATS, so use it as guidance.

## Project structure

```
.
├── app.py             # Streamlit app
├── requirements.txt   # Python dependencies
├── README.md
└── .gitignore
```

## Run locally

1. Get a free API key at https://aistudio.google.com/app/apikey
2. Install and run:

```bash
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
pip install -r requirements.txt

# provide the key (any ONE of these)
export GEMINI_API_KEY="your-key"            # Windows PowerShell: $env:GEMINI_API_KEY="your-key"
# or create .streamlit/secrets.toml containing:  GEMINI_API_KEY = "your-key"
# or just paste it into the sidebar when the app opens

streamlit run app.py
```

## Configuration

Model names are set at the top of `app.py` (`DEFAULT_MODEL`, `FALLBACK_MODELS`). Google retires and renames models over time, so if you see model-not-found errors, update these to a current Flash model listed at https://ai.google.dev/gemini-api/docs/models.

## Push to GitHub (using the web UI)

1. Sign in at https://github.com and click **+ → New repository**.
2. Name it (e.g. `ats-resume-checker`), choose **Public** (required for free Streamlit hosting unless you connect a private repo), and click **Create repository**.
3. On the new repo page click **uploading an existing file**.
4. Drag in `app.py`, `requirements.txt`, `README.md` and `.gitignore` (do **not** upload any `secrets.toml` or API key).
5. Write a commit message (e.g. "Initial commit") and click **Commit changes**.

## Deploy on Streamlit Community Cloud

1. Go to https://share.streamlit.io and sign in with GitHub.
2. Click **Create app** → **Deploy a public app from GitHub**.
3. Pick your repository, branch `main`, and main file path `app.py`.
4. Open **Advanced settings → Secrets** and paste:

```toml
GEMINI_API_KEY = "your-gemini-api-key"
```

5. Click **Deploy**. After a minute or two you get a public `https://<name>.streamlit.app` URL.

Every later commit to the repo redeploys the app automatically.

### Using Cloudflare

Streamlit needs a long-running Python server, which Cloudflare Pages/Workers do not host. Deploy on Streamlit Community Cloud as above. If you want your own domain, you can point a Cloudflare-managed domain at the app, or run the app on a VPS/Docker host and put Cloudflare in front as a proxy.

## Troubleshooting

| Problem | Fix |
| --- | --- |
| "Analysis failed" | Check the API key and the technical details; update model names if they are retired |
| "Very little text could be extracted" | The PDF is probably a scanned image. Export a text-based PDF or DOCX |
| Quota / 429 errors | You hit the free-tier rate limit; wait a minute or enable billing |

## Privacy

The resume text is sent to the Gemini API for analysis. The app itself does not store uploads.
