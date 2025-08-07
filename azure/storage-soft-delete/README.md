# 🔐 Azure Storage – Blob Soft Delete Lab

Hands-on lab completed in my **Azure for Students** subscription to demonstrate basic
data-protection controls. Enables **Blob Soft Delete**, verifies that a deleted object
can be recovered, and then cleans up to avoid cost.

> *Date completed: 2025-08-06*  
> *Resource group used: `storage-lab-rg` (deleted after lab)*

## Key learnings
- Soft Delete keeps blobs recoverable for a chosen retention window (7 days in this lab).
- Restoring a deleted blob is a single click—useful against accidental or malicious deletion.
- Cleaning up resources immediately prevents any billing in a student subscription.

## Folder contents
| File | Purpose |
|------|---------|
| `summary.md` | Step-by-step actions & lessons learned |
| `notes.md`   | CLI commands and extra tips |
