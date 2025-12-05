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

## GCP VM Setup (First Time)

### 1. SSH into your VM
```bash
gcloud compute ssh rickwilson@biochar-rag-vm --zone=us-west1-b
```

### 2. Install Git LFS
```bash
sudo apt-get update && sudo apt-get install -y git-lfs
git lfs install
```

### 3. Clone the data repo
```bash
cd ~/biochar-rag
rm -rf data  # Remove old data directory if exists
git clone https://github.com/rickwilson1/biochar-rag-data.git data
```

### 4. Extract attachments
```bash
cd data
tar -xzf attachments.tar.gz
```

### 5. Verify and restart
```bash
ls -lh  # Should show all files
sudo systemctl restart biochar-api
```

---

## Updating Data on VM

When you push new embeddings to GitHub:

```bash
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
cp /path/to/new/embeddings.npy .
cp /path/to/new/faiss.index .
cp /path/to/new/chunks.parquet .
cp /path/to/new/attachments.tar.gz .  # If attachments changed

# Push to GitHub
git add -A
git commit -m "Update embeddings"
git push
```

---

## Directory Structure on VM

```
~/biochar-rag/
├── api/           # API code (from biochar-rag-api)
├── app/           # App code (from biochar-rag-app)  
├── data/          # Data files (from biochar-rag-data)
│   ├── embeddings.npy
│   ├── faiss.index
│   ├── chunks.parquet
│   ├── attachments.tar.gz
│   └── attachments/   # Extracted attachment files
```
