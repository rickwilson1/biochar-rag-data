# Syncing Data to GCP VM

## First-time Setup on VM

SSH into your VM and run:

```bash
# Install Git LFS
sudo apt-get update && sudo apt-get install -y git-lfs
git lfs install

# Clone the data repo
cd ~/biochar-rag
git clone https://github.com/rickwilson1/biochar-rag-data.git data

# Extract attachments
cd data
tar -xzf attachments.tar.gz

# Verify files
ls -lh
```

## Update Data on VM

When you push new embeddings to GitHub:

```bash
cd ~/biochar-rag/data
git pull
tar -xzf attachments.tar.gz  # Re-extract if attachments changed

# Restart the API to pick up new data
sudo systemctl restart biochar-api
```

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

