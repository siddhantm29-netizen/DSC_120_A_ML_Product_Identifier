# DSC_120_A_ML_Product_Identifier

Free Deployment Guide — Fashion Classifier
Stack: GitHub → Render.com (API) + Streamlit Community Cloud (UI)
Cost: $0. No credit card required.
---
Folder structure for your GitHub repo
```
your-repo/
├── app.py                    ← FastAPI backend
├── streamlit_app.py          ← Streamlit frontend
├── requirements.txt          ← API dependencies
├── requirements_streamlit.txt← UI dependencies
├── render.yaml               ← Render auto-config
├── .gitignore
└── models/                   ← NOT committed (too large)
    ├── custom_cnn.keras
    └── mobilenetv2_tl.keras
```
---
Step 1 — Push your code to GitHub
```bash
# In your project folder
git init
git add app.py streamlit_app.py requirements.txt requirements_streamlit.txt render.yaml .gitignore
git commit -m "Initial deployment"

# Create a repo on github.com, then:
git remote add origin https://github.com/YOUR_USERNAME/fashion-classifier.git
git push -u origin main
```
> ⚠️ Do NOT commit the `.keras` model files — they are hundreds of MB.
> Render cannot store them either (free tier has no persistent disk).
> See Step 2b for how to handle this.
---
Step 2a — Deploy API on Render.com (without model files)
This runs the API in demo mode (random predictions) — good for testing the full pipeline.
Go to render.com → Sign up (free, GitHub login works)
Click New → Web Service
Connect your GitHub repo
Render detects `render.yaml` automatically — click Apply
Wait ~3–5 min for the build
Your API is live at: `https://fashion-classifier-api.onrender.com`
Test it:
```bash
curl https://fashion-classifier-api.onrender.com/health
# → {"status":"ok"}

curl https://fashion-classifier-api.onrender.com/classes
# → {"classes":["Accessories","Apparel",...]}
```
---
Step 2b — Add real model weights (optional)
Render's free tier has no persistent disk, so model files must be loaded from a URL.
Option A: Hugging Face Hub (easiest, free)
Create a free account at huggingface.co
Create a new model repo (can be private)
Upload your `.keras` files there
In `app.py`, replace the local file load with:
```python
# At the top of app.py, add:
from huggingface_hub import hf_hub_download

HF_REPO = "your-username/fashion-classifier"   # set this

def get_model(name: str):
    if name not in _models:
        import tensorflow as tf
        # Download from HF Hub on first request
        path = hf_hub_download(repo_id=HF_REPO, filename=f"{name}.keras")
        _models[name] = tf.keras.models.load_model(path)
    return _models[name]
```
Add `huggingface-hub==0.23.0` to `requirements.txt`
Push & redeploy on Render
Option B: Google Drive (quick & free)
Upload `.keras` files to Google Drive → get shareable link
Use `gdown` in `app.py` to download on startup:
```python
import gdown, os
FILE_IDS = {
    "mobilenetv2_tl": "YOUR_GDRIVE_FILE_ID",
    "custom_cnn":     "YOUR_GDRIVE_FILE_ID",
}
def get_model(name):
    if name not in _models:
        import tensorflow as tf
        path = f"models/{name}.keras"
        os.makedirs("models", exist_ok=True)
        if not os.path.exists(path):
            gdown.download(id=FILE_IDS[name], output=path, quiet=False)
        _models[name] = tf.keras.models.load_model(path)
    return _models[name]
```
Add `gdown==5.2.0` to `requirements.txt`
---
Step 3 — Deploy UI on Streamlit Community Cloud
Go to share.streamlit.io → Sign in with GitHub
Click New app
Select your repo → branch: `main` → file: `streamlit_app.py`
Under Advanced settings → Secrets, add:
```toml
   API_URL = "https://fashion-classifier-api.onrender.com"
   ```
Click Deploy — live in ~2 min at:
`https://your-username-fashion-classifier.streamlit.app`
---
Free tier limits to know
Platform	Limit	Impact
Render free	512 MB RAM	Use `tensorflow-cpu`, lazy loading
Render free	Sleeps after 15 min idle	First request takes ~30 s to wake
Render free	750 hrs/month compute	Enough for a demo
Streamlit Cloud	1 GB RAM	Fine for the frontend
Hugging Face Hub	Unlimited free public repos	Store model files here
---
Quick test after deployment
```python
import requests

API = "https://fashion-classifier-api.onrender.com"

# Health
print(requests.get(f"{API}/health").json())

# Predict
with open("shirt.jpg", "rb") as f:
    r = requests.post(f"{API}/predict?model=mobilenetv2_tl",
                      files={"file": f})
print(r.json())
```
---
Useful URLs after deployment
URL	Purpose
`https://your-api.onrender.com/docs`	Swagger UI — test API in browser
`https://your-api.onrender.com/health`	Health check
`https://your-api.onrender.com/models`	Model metrics
`https://your-username.streamlit.app`	Demo UI
