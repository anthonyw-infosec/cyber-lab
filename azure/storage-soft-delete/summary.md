# ✅ Lab Summary – Blob Soft Delete

1. Created storage account `softdelete####` in RG `storage-lab-rg`.
2. Enabled Soft Delete (7-day retention) under **Data protection**.
3. Uploaded `hello.txt` to container `testcontainer`.
4. Deleted `hello.txt`, toggled *Show deleted blobs*, confirmed deletion state.
5. Undeleted the blob and verified it reappeared.
6. Clean-up: `az group delete --name storage-lab-rg --yes --no-wait`.

### What I learned
- Soft Delete is off by default—must be enabled per account.
- Retention period is configurable (1–365 days).
- Always validate controls (delete → restore) before claiming success.
