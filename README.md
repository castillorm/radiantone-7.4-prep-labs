
---

## 🚀 Recommended Starting Point

### **1. Learn the Vocabulary**
Start with:

👉 `/docs/key-terms.md`

This file explains:

- Virtual Views  
- Join/Merge Views  
- Datasources  
- Connectors  
- Pipelines  
- Projections  
- Persistent Cache  
- Delta Tokens  
- Lenses  

Mastering these terms will make the Radiant training 90% easier.

---

### **2. Perform Your First ICS Pipeline**
Start simple before Azure AD:

👉 `/labs/lab-01-csv-to-hdap/`

You will:

- Create a CSV datasource  
- Build a pipeline  
- Materialize data into an HDAP store  
- Validate results in the Directory Browser  

This builds core ICS muscle memory.

---

### **3. Complete the Azure AD Sync Lab**
Then go deeper:

👉 `/docs/azure-ad-mini-lab.md`

This lab walks you through:

- Creating a Microsoft Graph datasource  
- Running full and delta sync  
- Materializing Azure AD users into HDAP  
- Building a virtual directory view  
- Testing updates  

This lab directly matches the workflows used in modern deployments.

---

### **4. Study ICS Pipeline Architecture**
Read:

👉 `/docs/pipelines-basics.md`

You will learn:

- How ICS pipelines are structured  
- How data flows from source → transform → target  
- How connectors work  
- How delta sync is managed  
- Best practices for stability and performance  

---

### **5. Review Architecture Diagrams**
Understanding the high-level system is essential:

👉 `/docs/architecture-overview.md`

Includes:

- VDS architecture  
- ICS/HDAP flow diagrams  
- Azure AD integration patterns  
- Virtual View lifecycle  

---

### **6. Practice Troubleshooting**
Finally review:

👉 `/docs/troubleshooting.md`

Covers:

- OAuth/token issues  
- Microsoft Graph paging errors  
- Schema mismatches  
- Attribute mapping problems  
- Pipeline execution failures  
- HDAP store validation steps  

This mirrors the troubleshooting topics covered in Radiant training.

---

## 📅 Training Prep Roadmap (Optional)

A recommended schedule before your RadiantOne Sync training:

| Day | Task | Duration |
|-----|-------|----------|
| Sat | Vocabulary + CSV → HDAP lab | ~2 hours |
| Sat | UI walkthrough (VDS + ICS) | ~1 hour |
| Sun | Azure AD Pipeline lab | ~2 hours |
| Sun | Architecture + Troubleshooting review | ~1 hour |

---

## 🤝 Contributions
This repo is intentionally generic and vendor-neutral.  
Pull requests are welcome if you want to extend labs, diagrams, or documentation.

---

## 📝 License
This repository is provided for educational use. No proprietary, customer, or confidential data is included or permitted.

---

## 🔗 Contact
For feedback or collaboration, open an issue or submit a pull request.

---

Mastering these core concepts before the formal RadiantOne training ensures you arrive prepared, confident, and able to absorb advanced concepts faster and more deeply.
