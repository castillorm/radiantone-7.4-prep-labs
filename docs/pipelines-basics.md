# ICS Pipelines — Core Concepts & Workflow  
**File:** /docs/pipelines-basics.md  
**Version:** 1.0  
**Purpose:** Provide engineers with a clear understanding of how RadiantOne ICS pipelines work, how data flows inside the synchronization engine, and best practices for stable identity integration.

---

# Table of Contents
1. [What Is a Pipeline?](#what-is-a-pipeline)  
2. [Three Stages of a Pipeline](#three-stages-of-a-pipeline)  
3. [Pipeline Components](#pipeline-components)  
4. [Pipeline Modes](#pipeline-modes)  
5. [Pipeline Targets](#pipeline-targets)  
6. [Delta Sync](#delta-sync)  
7. [Error Handling](#error-handling)  
8. [Best Practices](#best-practices)

---

# What Is a Pipeline?

A **pipeline** is a structured workflow that moves identity data from a source system into a target system through a series of transformations. Pipelines are part of the RadiantOne **ICS (Identity Correlation & Synchronization)** engine.

Pipelines are essential for:
- Identity aggregation  
- Attribute normalization  
- Azure AD materialization  
- High-speed HDAP storage  
- Downstream provisioning  

---

# Three Stages of a Pipeline

A pipeline always executes these three phases:

## **1. Read**
RadiantOne reads identity objects from a **Connector** tied to a **Datasource** using:
- LDAP queries  
- SQL queries  
- REST API calls  
- Microsoft Graph calls  
- File parsing  

## **2. Transform**
RadiantOne converts raw source objects into normalized identity entries using:
- Projection mappings  
- Calculated attributes  
- Data type conversions  
- Filters  
- Schema normalization  

## **3. Write**
RadiantOne stores the transformed objects into a **target**, such as:
- HDAP  
- LDAP  
- SQL  
- Application endpoints  

---

# Pipeline Components

A pipeline consists of:

### **Source Connector**
Defines how Radiant reads the source system.

### **Object Type**
Users, Groups, Devices, etc.

### **Projection Mapping**
Defines how source → target attributes map.

### **Transformation Rules**
Normalize, enrich, or restructure data.

### **Target**
Where the transformed identity objects are written.

### **Job Scheduler**
Controls execution timing.

### **Logs & History**
Provide run-level and object-level diagnostics.

---

# Pipeline Modes

## **Full Import**
Reads the full dataset from the source every time.  
Used for:
- First-time loads  
- Schema redesigns  
- Major fixes  

## **Delta Sync**
Reads only modified/added/deleted objects since last run.  
Uses delta tokens.

Delta sync is used in nearly all production scenarios.

---

# Pipeline Targets

### **1. HDAP (Most Common)**
- High-performance NoSQL directory  
- Ideal for materialization  
- Supports persistent cache  
- Works well at DHS-scale

### **2. LDAP**
Used when syncing into Active Directory or another LDAP directory.

### **3. SQL**
Writes to relational stores.

### **4. Apps / APIs**
For provisioning or notifications.

---

# Delta Sync

Delta sync uses change-tracking tokens (e.g., Graph deltaLink) to retrieve only updated objects.

Benefits:
- Lower latency  
- Minimal load on Azure AD  
- High performance at scale  
- Avoids full scans of large directories  

---

# Error Handling

Common failure types:

### **Authentication errors**
Invalid OAuth credentials, expired secrets.

### **Permission issues**
Missing API scopes.

### **Paging issues**
Large datasets not retrieved fully.

### **Schema issues**
Attributes missing or incompatible.

### **Transient API failures**
Microsoft Graph throttling or 429 errors.

ICS will:
- Retry certain operations  
- Log failures  
- Optionally stop pipeline execution  

---

# Best Practices

- Always run one **full import** before a delta.  
- Materialize cloud directories into **HDAP**.  
- Use **calculated attributes** for normalization.  
- Keep pipeline projections clean and documented.  
- Monitor logs after every pipeline change.  
- Use delta sync for ongoing operations.  
- Keep mapping schemas consistent across environments.  

---

This document provides a foundation for understanding ICS pipeline architecture and prepares engineers for hands-on Radiant training.
````

---
