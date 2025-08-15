# Azure — SAS Blob Leak (Detect, Log, Remediate)

**Path:** `azure/sas-blob-leak-detect-remediate`  
**Type:** Storage security lab  
**Skills:** Blob SAS, Diagnostic settings, KQL (Log Analytics), Key rotation  
**Time:** ~20–30 min • **Cost:** pennies

## Goal
Create a **private** container, generate a **read-only SAS URL** (the “leak”), prove external access in **Incognito**, send Blob logs to **Log Analytics**, then **revoke** the leak by rotating the access key.

---

## Prereqs
- Azure subscription (Azure for Students works)
- One **Resource Group** (e.g., `cloud-sec-starter-rg`) in **eastus2**
- A **Log Analytics workspace** in the same region (e.g., `soc-workspace`)

---

## Steps

### 1) Storage account (policy-friendly)
1. Portal → **Storage accounts → Create**  
2. **RG:** `cloud-sec-starter-rg` • **Region:** `eastus2` • **Redundancy:** LRS  
3. **Advanced:** **Allow blob public access = Disabled**  
4. **Create** → open the account when done.

**Evidence:** screenshot of *Configuration* showing **Allow blob public access = Disabled**.

---

### 2) Private container + test blob
1. Storage account → **Data storage → Containers → + Container**  
   - **Name:** `private` • **Public access level:** *Private (no anonymous)*  
2. Open `private` → **Upload** a text file, e.g. `Dont Share.txt` with the line:  
   `This should NOT be shared.`

**Evidence:** container list (Access level = Private), blob details (before SAS).

---

### 3) Generate a SAS URL (the “leak”)
1. Click the blob (`Dont Share.txt`) → **Generate SAS** (or **Shared access tokens**):  
   - **Permissions:** Read  
   - **Expiry:** 30–60 minutes  
   - **Protocol:** HTTPS only  
   - **Signing key:** key1 (default)  
2. **Generate SAS token and URL** → copy the **Blob SAS URL**.  
3. Open a **new Incognito/Private** window → paste the SAS URL → the file should render.

**Evidence:** SAS panel (redact the token) + Incognito showing the file (redact everything after `?`).

> **Redaction tip:** Never commit live SAS tokens. Black-box anything after `?`.

---

### 4) Turn on logging to Log Analytics
1. If needed, create a workspace: **Log Analytics workspaces → Create** (region = `eastus2`).  
2. Storage account → **Monitoring → Diagnostic settings → + Add diagnostic setting**  
   - **Name:** `toLAW`  
   - **Destination:** **Send to Log Analytics workspace** → select your workspace  
   - **Categories:** **AllLogs** for **Blob** (Read/Write/Delete) → **Save**  
3. Hit the **same SAS URL** again in Incognito to generate a log.

**Evidence:** Diagnostic setting enabled for **Blob**.

---

### 5) Hunt in KQL (prove the SAS read)
In **Log Analytics workspace → Logs**, set **Time range: Last 4 hours**, run:

```kusto
let acct = "<YOUR_STORAGE_ACCOUNT>"; // e.g., blackicestarter123
union isfuzzy=true
(
  StorageBlobLogs
  | where OperationName == "GetBlob" and AccountName == acct
  | extend Uri = tostring(column_ifexists("Uri",""))
  | extend RespCode = toint(coalesce(column_ifexists("StatusCode",""), column_ifexists("ResponseCode","")))
  | project TimeGenerated, AuthenticationType, Uri, RespCode
),
(
  AzureDiagnostics
  | where ResourceProvider has_cs "MICROSOFT.STORAGE" and OperationName == "GetBlob" and accountName_s == acct
  | extend Uri = tostring(column_ifexists("requestUri_s",""))
  | extend RespCode = toint(column_ifexists("statusCode_s",""))
  | project TimeGenerated, AuthenticationType=tostring(authenticationType_s), Uri, RespCode
)
| order by TimeGenerated desc
