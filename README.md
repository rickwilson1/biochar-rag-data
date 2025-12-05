# Biochar RAG Data

Data files for the Biochar RAG system. These files are tracked with Git LFS.

## Files

| File | Description | Size |
|------|-------------|------|
| `embeddings.npy` | Vector embeddings (Together AI) | 886 MB |
| `faiss.index` | FAISS similarity search index | 886 MB |
| `chunks.parquet` | Text chunks with metadata | 76 MB |
| `attachments.tar.gz` | PDF and image attachments archive | 1.3 GB |

---

## GitHub Repos Overview

| Repo | Purpose | Location |
|------|---------|----------|
| `biochar-rag-data` | Data files (LFS) | GCP VM: `~/biochar-rag/data/` |
| `biochar-rag` | GCP code (api + app) | GCP VM: `~/biochar-rag/code/` |
| `biochar-rag-api` | Render API (deprecated) | Render |
| `biochar-rag-app` | Render App (deprecated) | Render |

---

## GCP VM First-Time Setup

### 1. SSH into your VM
```bash
gcloud compute ssh rickwilson@biochar-rag-vm --zone=us-west1-b
```

### 2. Install dependencies
```bash
sudo apt-get update && sudo apt-get install -y git-lfs gh
git lfs install
```

### 3. Authenticate with GitHub
```bash
gh auth login
# Select: GitHub.com → HTTPS → Yes → Login with browser
# Open the URL on your Mac and enter the code shown
```

### 4. Clone the data repo
```bash
cd ~/biochar-rag
rm -rf data
git clone https://github.com/rickwilson1/biochar-rag-data.git data
cd data && tar -xzf attachments.tar.gz
```

### 5. Clone the code repo
```bash
cd ~/biochar-rag
mv api api_backup
mv app app_backup
gh repo clone rickwilson1/biochar-rag code
ln -s code/api api
ln -s code/app app
```

### 6. Restart services
```bash
sudo systemctl restart biochar-api
sudo systemctl restart biochar-app
sudo systemctl status biochar-api
```

### 7. Test
Visit https://biocharai.org

---

## Updating Code on VM

When you push code changes to GitHub:

```bash
# On your Mac
cd ~/Documents/PyProjects/biochar_rag/gcp-deploy
git add -A && git commit -m "Your message" && git push

# On the VM
cd ~/biochar-rag/code
git pull
sudo systemctl restart biochar-api biochar-app
```

---

## Updating Data on VM

When you push new embeddings to GitHub:

```bash
# On the VM
cd ~/biochar-rag/data
git pull
tar -xzf attachments.tar.gz  # Re-extract if attachments changed
sudo systemctl restart biochar-api
```

---

## Updating Data from Mac

When embeddings are regenerated locally:

```bash
cd ~/Documents/PyProjects/biochar_rag/biochar-rag-data

# Copy new files
cp ../data/embeddings_output/combined/embeddings.npy .
cp ../data/embeddings_output/combined/faiss.index .
cp ../data/embeddings_output/combined/chunks.parquet .
cp ../data/intermediate/attachments.tar.gz .  # If attachments changed

# Push to GitHub
git add -A
git commit -m "Update embeddings"
git push
```

---

## Directory Structure on VM

```
~/biochar-rag/
├── api -> code/api        # Symlink to code repo
├── app -> code/app        # Symlink to code repo
├── code/                  # From biochar-rag repo (GitHub)
│   ├── api/
│   │   ├── main.py
│   │   ├── rag_core.py
│   │   └── email_store.py
│   └── app/
│       └── app.py
├── data/                  # From biochar-rag-data repo (GitHub LFS)
│   ├── embeddings.npy
│   ├── faiss.index
│   ├── chunks.parquet
│   ├── attachments.tar.gz
│   └── attachments/       # Extracted PDFs/images
└── venv/                  # Python virtual environment
```

---

## Live Site

**https://biocharai.org**
