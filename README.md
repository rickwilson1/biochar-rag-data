# Biochar RAG Data

Data files for the Biochar RAG system. These files are tracked with Git LFS.

## Files

| File | Description |
|------|-------------|
| `embeddings.npy` | Vector embeddings (Together AI) |
| `faiss.index` | FAISS similarity search index |
| `chunks.parquet` | Text chunks with metadata |
| `attachments.tar.gz` | PDF and image attachments archive |

## Usage

### Clone with LFS
```bash
git lfs install
git clone https://github.com/rickwilson1/biochar-rag-data.git
```

### On GCP VM
```bash
cd ~/biochar-rag/data
git pull  # Updates data files
tar -xzf attachments.tar.gz  # Extract attachments
```

## Updating Data

When embeddings are regenerated:
```bash
cp /path/to/new/embeddings.npy .
cp /path/to/new/faiss.index .
cp /path/to/new/chunks.parquet .
git add -A
git commit -m "Update embeddings"
git push
```

