# AI Image Detector

Next.js 15 app that classifies an uploaded image as AI-generated, authentic, or uncertain, using
Hugging Face's free serverless Inference API (`umm-maybe/AI-image-detector`).

## 1. Get a free Hugging Face API token

1. Create a free account at https://huggingface.co/join (skip if you have one).
2. Go to https://huggingface.co/settings/tokens.
3. Click **New token** → name it (e.g. `ai-image-detector`) → role **Read** → **Generate**.
4. Copy the token (starts with `hf_...`).
5. (Optional but recommended) Visit the model page once while logged in so it's "warmed":
   https://huggingface.co/umm-maybe/AI-image-detector — click **Deploy → Inference API** or just
   let the first request in the app trigger the cold start.

## 2. Run locally

```bash
npm install
cp .env.local.example .env.local
# paste your token into .env.local
npm run dev
```

Visit http://localhost:3000.

## 3. Deploy to Vercel (free tier)

**Option A — via GitHub (recommended)**

1. Push this folder to a new GitHub repo.
2. Go to https://vercel.com/new and import the repo.
3. Framework preset: Next.js (auto-detected). Leave build settings as default.
4. Under **Environment Variables**, add:
   - `HUGGINGFACE_API_KEY` = your `hf_...` token
5. Click **Deploy**. Vercel builds and gives you a live URL in ~1-2 minutes.

**Option B — via Vercel CLI**

```bash
npm install -g vercel
vercel login
vercel
# follow prompts, then set the env var:
vercel env add HUGGINGFACE_API_KEY
vercel --prod
```

## Notes

- The Hugging Face free Inference API can "cold start" (HTTP 503) if the model hasn't been called
  recently — the app automatically shows a countdown and retries.
- No heavy ML library runs inside the Next.js server — the API route only makes an HTTP call to
  Hugging Face, so it stays well under Vercel's serverless function size limits.
- Scoring thresholds: **≥70%** → Likely AI-Generated, **≤30%** → Likely Authentic, **31–69%** →
  Uncertain / Inconclusive.
