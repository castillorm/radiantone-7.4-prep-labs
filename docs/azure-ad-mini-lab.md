# Azure AD → RadiantOne Mini Lab  
**File:** /docs/azure-ad-mini-lab.md  
**Version:** 1.0  
**Purpose:** Provide a complete, hands-on guide for synchronizing Azure Active Directory users into RadiantOne HDAP using the Microsoft Graph connector in RadiantOne 7.3/7.4.

---

# Table of Contents
1. [Overview](#overview)  
2. [Prerequisites](#prerequisites)  
3. [Architecture Summary](#architecture-summary)  
4. [Lab Part 1 — Create Azure AD Datasource](#lab-part-1--create-azure-ad-datasource)  
5. [Lab Part 2 — Create HDAP Store](#lab-part-2--create-hdap-store)  
6. [Lab Part 3 — Build ICS Pipeline](#lab-part-3--build-ics-pipeline)  
7. [Lab Part 4 — Run Full Sync](#lab-part-4--run-full-sync)  
8. [Lab Part 5 — Validate HDAP Output](#lab-part-5--validate-hdap-output)  
9. [Lab Part 6 — Build Virtual View](#lab-part-6--build-virtual-view)  
10. [Lab Part 7 — Delta Sync](#lab-part-7--delta-sync)  
11. [Bonus — Sync Groups](#bonus--sync-groups)  
12. [Troubleshooting](#troubleshooting)  

---

# Overview

This lab walks you through a complete Azure Active Directory → RadiantOne synchronization workflow. By the end of the exercise, you will:

- Connect RadiantOne to Azure AD via Microsoft Graph  
- Build a user sync pipeline  
- Materialize cloud identities into an HDAP store  
- Build a VDS virtual view  
- Run full and delta sync operations  
- Validate the final identity hierarchy  

This lab mirrors real-world Radiant deployments, including DHS-scale identity aggregation patterns.

---

# Prerequisites

## Azure AD
- Tenant ID  
- Client ID  
- Client Secret  
- API Permissions:  
  - `User.Read.All`  
  - `Directory.Read.All`  
- Admin consent granted  

## RadiantOne
- Version 7.3 or 7.4  
- Access to VDS, ICS, and HDAP in Control Panel  

---

# Architecture Summary

```text
Azure AD (Microsoft Graph API)
        │
        ▼
RadiantOne Datasource (OAuth 2.0)
        │
        ▼
ICS Pipeline (Users objectType)
        │
        ▼
HDAP Store (/hdapAzureADUsers)
        │
        ▼
Virtual View (ou=AzureAD,dc=example,dc=com)
````

---

# Lab Part 1 — Create Azure AD Datasource

1. Open
   **Control Panel → Directory Namespace → Datasources → Add Datasource**

2. Choose:
   **Microsoft Graph (Azure AD)**

3. Enter credentials:

   * Tenant ID
   * Client ID
   * Client Secret
   * Token Endpoint auto-populates

4. Click **Test Connection**

Expected result:

* Successful OAuth token retrieval
* No permission or scope errors

5. Save datasource.

---

# Lab Part 2 — Create HDAP Store

1. Navigate to:
   **Directory Namespace → HDAP Stores → Create Store**

2. Name the store:

```
hdapAzureADUsers
```

3. Keep default schema.
4. Save.

---

# Lab Part 3 — Build ICS Pipeline

1. Open:
   **Sync & Connect → Pipelines → Add Pipeline**

2. Name:

```
pipelineAzureADUsers
```

3. Select Source Connector → your Azure AD datasource

4. Choose object type: **Users**

5. Select target: **HDAP Store → hdapAzureADUsers**

6. Map attributes:

| Azure AD Field    | HDAP Attribute    |
| ----------------- | ----------------- |
| id                | uid               |
| displayName       | cn                |
| mail              | mail              |
| userPrincipalName | userPrincipalName |
| givenName         | givenName         |
| surname           | sn                |

7. Save pipeline.

---

# Lab Part 4 — Run Full Sync

1. Open the pipeline.
2. Select **Run → Full Import**.
3. Confirm processing in the logs.

Success indicators:

* “Retrieved X users from Microsoft Graph”
* “Wrote X entries to HDAP store”

---

# Lab Part 5 — Validate HDAP Output

1. Open:
   **Directory Browser**
2. Navigate to:

```
vds:///hdapAzureADUsers
```

Verify:

* Entries present
* Required attributes mapped
* No missing cn, uid, or primary identifiers

---

# Lab Part 6 — Build Virtual View

1. Go to
   **Context Builder → Add New → From Existing Store**

2. Choose:

```
hdapAzureADUsers
```

3. Build structure:

```
ou=AzureAD,dc=example,dc=com
└── ou=Users
```

4. Publish and test the view.

---

# Lab Part 7 — Delta Sync

1. Modify or create a user in Azure AD.
2. Return to the pipeline:
   **Run → Delta Import**
3. Validate:

   * Only changed entries processed
   * Delta token updated

---

# Bonus — Sync Groups

Repeat the above steps using:

* `hdapAzureADGroups`
* objectType = `Groups`

Map:

```
id → gid
displayName → cn
members → member
```

---

# Troubleshooting

### Issue: OAuth token failure

* Regenerate client secret
* Verify tenant ID
* Ensure admin consent

### Issue: No users returned

* Missing `Directory.Read.All` scope
* Ensure app registration has correct permissions

### Issue: Delta sync errors

* Run one full sync first
* Verify deltaLink returned from Graph

### Issue: Attribute mapping failures

* Check for null/empty values
* Ensure target schema supports mapped attributes

---

**End of lab.**
You now have a complete Azure AD → HDAP → VDS flow running in RadiantOne.

````