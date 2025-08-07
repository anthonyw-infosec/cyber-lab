# 📚 Module Summary — Secure Azure Storage with RBAC & Key Vault

> **Type:** Microsoft Learn reading module (no manual resource deployment)

---

## 🔍 Module Overview
The module walks through Microsoft’s recommended approach to securing an Azure Storage account by combining **Role-Based Access Control (RBAC)** and **Azure Key Vault**. All steps are demonstrated in screenshots and text within the Learn platform.

---

## 🧠 Key Takeaways
| Topic | Insight |
|-------|---------|
| RBAC | Assign the smallest role (e.g., **Storage Blob Data Reader**) at the narrowest scope to enforce least-privilege access. |
| Key Vault | Customer-managed keys stored in Key Vault let you separate key management from data storage, and support rotation & auditing. |
| Encryption | Enabling encryption at rest with a Key Vault key satisfies stricter compliance requirements (HIPAA, PCI-DSS, etc.). |
| Shared Responsibility | Microsoft secures the platform; **you** configure RBAC, keys, and monitoring. |

---

## 🚀 Next Hands-On Steps
1. **Re-create** this setup manually in a personal Azure subscription.
2. **Enable logging** with Azure Monitor or Sentinel to alert on unauthorized access.
3. **Add Defender for Storage** to test threat detection.

---

*Completed:*

![Completion Badge](./screenshots/module-complete.png)
