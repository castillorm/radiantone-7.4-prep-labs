# RadiantOne Key Terms — Exhaustive Definitions  
**File:** /docs/key-terms.md  
**Version:** 1.0  
**Purpose:** Provide detailed definitions of the foundational RadiantOne (7.3/7.4) concepts required to understand VDS, ICS, HDAP, Microsoft Graph integrations, and identity data virtualization.

Understanding these terms will make Radiant training—and real-world deployments—dramatically easier.

---

# Table of Contents
1. [Virtual View](#virtual-view)  
2. [Join View](#join-view)  
3. [Merge View](#merge-view)  
4. [Datasource](#datasource)  
5. [Connector](#connector)  
6. [Pipeline](#pipeline)  
7. [Projection](#projection)  
8. [Persistent Cache](#persistent-cache)  
9. [Delta Token](#delta-token)  
10. [Lens](#lens)

---

# Virtual View

A **Virtual View** is a dynamic, query-driven representation of identity data inside RadiantOne. It presents external sources (Azure AD, LDAP, SQL, REST, etc.) as if they were a traditional LDAP directory—even though the data may reside elsewhere.

## Key Characteristics
- Defined in **Context Builder**  
- Does **not** store data (unless cached)  
- Data is retrieved *in real time* from Datasources  
- Can include **transformations**, **filters**, and **virtual attributes**  
- Exposed over **LDAP**, **REST**, and **SCIM**  
- Forms the core of VDS (Virtual Directory Server)

## Why It Matters
Virtual Views abstract complexity and unify multiple identity systems into a single logical namespace.

---

# Join View

A **Join View** merges attributes from two or more independent sources using a shared attribute (e.g., employeeID, email, uid).

## How It Works
- A **primary source** provides the base entry  
- **Secondary sources** enrich it  
- Join logic uses a matching rule  
- Result is presented as a unified entry

## Use Cases
- Augment AD users with attributes from SQL (HR data)  
- Combine Azure AD cloud attributes with on-prem AD  
- Create full employee profiles when identity is fragmented  

---

# Merge View

A **Merge View** consolidates identity entries from multiple sources where the same user may appear more than once.

## Key Behavior
- Detects duplicate identities  
- Applies source precedence rules  
- Merges attributes according to policies  
- Outputs **one canonical identity**

## Why It Matters
Organizations often maintain multiple directories; Merge Views solve identity overlap at scale.

---

# Datasource

A **Datasource** defines how RadiantOne connects to an external system.

## Examples
- Azure AD (Microsoft Graph)  
- Active Directory / LDAP  
- SQL (Oracle, PostgreSQL, MySQL, MSSQL)  
- REST APIs  
- CSV / Flat files  
- SCIM endpoints  

## Datasource Components
- Connection URL / endpoints  
- Authentication (OAuth, Basic, Kerberos, etc.)  
- Permissions & scopes  
- Object type configuration  
- Paging, throttling, and timeout settings  
- Schema definitions  

## Purpose
Datasources are the “inputs” for both Virtual Views and ICS pipelines.

---

# Connector

A **Connector** is an ICS module that retrieves data from a Datasource using the appropriate protocol (Graph API, LDAP, SQL queries, REST calls, etc.).

## Responsibilities
- Authenticate to the source  
- Read objects in batches  
- Handle paging + throttling  
- Detect changes (delta sync)  
- Transform raw data into an ICS object model  

## Examples
- Microsoft Graph Azure AD Connector  
- LDAP Connector  
- SQL Connector  
- CSV Connector  

Every sync pipeline begins with a Connector.

---

# Pipeline

A **Pipeline** is an Identity Correlation & Synchronization (ICS) workflow that defines how data flows through RadiantOne.

## The Three Pipeline Stages
1. **Read**  
   From source via a Connector  
2. **Transform**  
   Using projections, calculated attributes, filters  
3. **Write**  
   Into HDAP, LDAP, SQL, or another target

## Pipeline Types
- Full Import  
- Delta Sync  
- Bidirectional sync (if supported)  
- Scheduled or triggered jobs  

## Purpose
Pipelines convert raw identity data into usable, normalized directory entries.

---

# Projection

A **Projection** defines how attributes from a source object map to the target object.

## What Projection Supports
- 1:1 mappings  
- Attribute renaming  
- Type conversion  
- Default values  
- Calculated attributes  
- Filtering logic  
- Attribute normalization  

### Example Mapping (Azure AD → HDAP)
| Source Attribute | Target Attribute |
|------------------|------------------|
| id               | uid              |
| displayName      | cn               |
| mail             | mail             |
| userPrincipalName | userPrincipalName |

Projections are the core of data modeling inside ICS.

---

# Persistent Cache

A **Persistent Cache** is a materialized copy of a Virtual View stored in HDAP. It improves performance by eliminating the need to query the back-end system directly.

## Without Persistent Cache
- VDS queries *live* sources  
- Dependent on remote performance  
- More latency and potential failures  

## With Persistent Cache
- Data stored locally in HDAP  
- Supports scheduled refresh  
- Faster queries (millisecond response times)  
- Works even if back-end sources are offline  

## Use Cases
- Azure AD → HDAP cache for cloud directories  
- Global employee directory serving many apps  
- Large joins/merges requiring fast local access  

---

# Delta Token

A **Delta Token** is a bookmark used to track changes in incremental syncs. It records the last-known state so ICS can retrieve only new or modified identities.

## Key Behavior
- Created automatically after a successful full sync  
- Provided to the connector on every delta run  
- Updated by ICS as changes are processed  
- Allows highly efficient incremental updates  

## Example (Microsoft Graph)
Azure AD returns:
ICS stores this delta token and uses it for the next sync.

---

# Lens

A **Lens** is a transformation layer applied **on top of a Virtual View** to create tailored views for different consumers without rewriting the underlying model.

## Lens Capabilities
- Attribute masking  
- Attribute renaming  
- DN remapping  
- Filtering (e.g., only active employees)  
- Normalization  
- Calculated attributes  
- View-specific schema adjustments  

## Why Lenses Matter
They allow you to:
- Reuse the same underlying identity store  
- Present different shapes of data to SSO, IAM, apps  
- Isolate consumers from internal schema changes  

---

# Summary

These terms represent the core mental model of RadiantOne:

- **VDS** (Virtual Views + Join/Merge + Lenses)  
- **ICS** (Connectors + Pipelines + Projections + Delta Tokens)  
- **HDAP** (Materialized Stores + Persistent Cache)  

Mastering these concepts is foundational for deeper work with identity virtualization, synchronization flows, and Azure AD integrations.


