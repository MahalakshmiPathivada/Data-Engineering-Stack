# Unity Catalog Interview Questions & Answers

## 1. What is Unity Catalog and Why Was It Introduced?

**Answer:**

Unity Catalog is Databricks' centralized governance solution that provides:

- Unified access control
- Data discovery
- Data lineage
- Auditing
- Data sharing

across all Databricks workspaces and clouds.

### Before Unity Catalog

- Permissions were managed at the workspace level.
- Metastore was workspace-specific.
- Governance across multiple workspaces was difficult.

### After Unity Catalog

Unity Catalog creates a centralized governance layer across the organization.

---

## 2. Explain the Unity Catalog Hierarchy

```text
Metastore
│
└── Catalog
    │
    └── Schema (Database)
        │
        ├── Tables
        ├── Views
        ├── Volumes
        └── Functions
```

---

## 3. What is the Difference Between Catalog and Schema?

### Catalog

- Top-level container
- Similar to a business domain

Examples:

```text
finance
sales
marketing
```

### Schema

- Logical grouping inside a catalog

Examples:

```text
finance.raw
finance.silver
finance.gold
```

---

## 4. Managed Table vs External Table in Unity Catalog

### Managed Table

```sql
CREATE TABLE sales (
    id INT,
    amount DOUBLE
);
```

**Characteristics:**

- Data managed by Databricks
- Dropping the table removes the data

### External Table

```sql
CREATE TABLE sales_ext
USING DELTA
LOCATION 'abfss://sales@storage.dfs.core.windows.net/data';
```

**Characteristics:**

- Data remains in storage after table drop
- Databricks manages metadata only

---

## 5. What are External Locations?

An **External Location** is a Unity Catalog object that links cloud storage to Databricks.

```sql
CREATE EXTERNAL LOCATION raw_data
URL 'abfss://raw@storageaccount.dfs.core.windows.net/'
WITH STORAGE CREDENTIAL my_credential;
```

### Benefits

- Secure access
- Central governance
- No storage keys in notebooks

---

## 6. What are Storage Credentials?

Storage Credentials define the authentication mechanism used to access cloud storage.

### Azure Examples

- Managed Identity
- Service Principal

```sql
CREATE STORAGE CREDENTIAL adls_cred
WITH AZURE_MANAGED_IDENTITY;
```

---

## 7. Explain Volumes in Unity Catalog

### Senior-Level Answer

Volumes govern non-tabular files.

### Examples

- CSV files
- JSON files
- Images
- PDFs
- ML Models
- Checkpoints

### Types

#### Managed Volume

```sql
CREATE VOLUME my_volume;
```

#### External Volume

Uses external cloud storage locations.

### Important

Volumes replace legacy DBFS mounts.

---

## 8. Why Should Organizations Move Away from DBFS Mounts?

### Problems with DBFS Mounts

- Workspace-specific
- Limited governance
- Access based on storage secrets
- No centralized auditing

### Benefits of Unity Catalog Volumes

- Fine-grained permissions
- Audit logs
- Consistent governance

---

## 9. How Does Unity Catalog Handle Security?

### Catalog-Level Security

```sql
GRANT USE CATALOG
ON CATALOG finance
TO users;
```

### Schema-Level Security

```sql
GRANT USE SCHEMA
ON SCHEMA finance.gold
TO users;
```

### Object-Level Security

```sql
GRANT SELECT
ON TABLE customer
TO analyst;
```

---

## 10. Explain Row-Level Security

### Scenario

An employee should only see records belonging to their region.

```sql
CREATE ROW FILTER region_filter
AS (region = current_user_region());
```

### Benefits

- Same table for all users
- Users only see authorized rows

---

## 11. Explain Column-Level Security

Sensitive columns:

- salary
- ssn
- credit_card

```sql
GRANT SELECT(name, department)
ON TABLE employees
TO analyst;
```

Users can access only specific columns.

---

## 12. What is Dynamic Data Masking?

### Example

Actual Value:

```text
9876543210
```

Masked Value:

```text
XXXXXX3210
```

### Used For

- PII Data
- GDPR Compliance
- HIPAA Compliance

---

## 13. Explain Lineage in Unity Catalog

### Example

```text
Source Table
      ↓
Silver Table
      ↓
Gold Table
      ↓
Dashboard
```

### Benefits

- Impact Analysis
- Compliance
- Troubleshooting

### Common Interview Question

**What happens if a source column changes?**

Use lineage to identify impacted:

- Pipelines
- Tables
- Dashboards

---

## 14. How Does Unity Catalog Support Auditing?

Tracks:

- Data access
- Permission changes
- Table creation
- File access

### Helps With

- SOX Compliance
- GDPR Compliance
- HIPAA Compliance

---

## 15. How Would You Design Unity Catalog for an Enterprise?

### Example

```text
Metastore
│
├── sales_catalog
│    ├── raw
│    ├── silver
│    └── gold
│
├── finance_catalog
│    ├── raw
│    ├── silver
│    └── gold
│
└── marketing_catalog
     ├── raw
     ├── silver
     └── gold
```

### Design Pattern

Domain-driven architecture.

---

## 16. Can Multiple Workspaces Share a Single Metastore?

### Answer

Yes.

### Enterprise Architecture

```text
Dev Workspace
      │
QA Workspace
      │
Prod Workspace
      │
Unity Catalog Metastore
```

### Benefits

- Consistent governance
- Centralized permissions

---

## 17. What is Delta Sharing and Its Relationship with Unity Catalog?

Delta Sharing allows:

- Secure data sharing
- No data copies
- Cross-cloud access

Unity Catalog manages:

- Shared tables
- Permissions
- Recipients

---

## 18. How Do You Migrate Hive Metastore to Unity Catalog?

### Typical Steps

1. Assess current objects.
2. Create Unity Catalog metastore.
3. Create catalogs and schemas.
4. Migrate tables.
5. Migrate permissions.
6. Validate data.
7. Enable UC access mode clusters.
8. Retire Hive Metastore.

---

## 19. What Challenges Have You Faced Implementing Unity Catalog?

### Real-World Challenges

- Legacy DBFS mount migration
- Storage credential setup
- Access model redesign
- Large-scale permission migration
- Multi-workspace governance alignment
- Existing ETL compatibility

---

## 20. Scenario-Based Question

### Scenario

Your organization has:

- 5 Databricks workspaces
- 1000+ users
- Finance and HR data
- Regulatory requirements

### Strong Answer

- Single Unity Catalog Metastore
- Domain-based catalogs
- Separate raw/silver/gold schemas
- Managed identities
- External locations
- Row-level security
- Column masking
- Automated audit monitoring
- Group-based permissions
- Delta Sharing for external partners

---

# Why Are Volumes Needed in Unity Catalog?

Volumes manage and govern **non-tabular data**, whereas tables manage structured data.

---

## Problem Before Volumes

Organizations typically stored files in:

- ADLS Gen2
- Amazon S3
- Azure Blob Storage

Challenges:

- Limited file governance
- Separate access controls
- Difficult auditing
- Shared storage keys
- Poor security

---

## Why Volumes Were Introduced

Volumes provide a governed storage layer under Unity Catalog.

Supported file types:

- CSV
- JSON
- Images
- PDFs
- ML Models
- Configuration Files
- Checkpoint Files
- Semi-structured and unstructured data

---

## Benefits of Volumes

### 1. Centralized Governance

```sql
GRANT READ FILES
ON VOLUME sales_catalog.raw.sales_volume
TO analyst_grp;
```

### 2. Auditability

Tracks:

- File uploads
- File modifications
- File access

### 3. Fine-Grained Security

Supports:

- READ FILES
- WRITE FILES

### 4. Better Alternative to DBFS Mounts

Volumes are fully governed by Unity Catalog.

---

## Types of Volumes

### Managed Volume

Databricks manages storage.

```sql
CREATE VOLUME customer_docs;
```

### External Volume

Points to existing storage.

```sql
CREATE EXTERNAL VOLUME sales_files
LOCATION 'abfss://raw@storageaccount.dfs.core.windows.net/sales/';
```

---

# Real-World Volume Scenario

## Requirement

Daily CSV files arrive in ADLS Gen2 from external vendors.

### Traditional Approach

- Direct storage access
- Shared SAS tokens
- Service Principal credentials

Problems:

- Security risks
- Credential management overhead
- Limited auditing
- Weak governance

---

## Unity Catalog Solution

### Step 1

Create Storage Credential.

### Step 2

Create External Location.

```sql
CREATE EXTERNAL LOCATION vendor_ext_loc
URL 'abfss://vendor@companystorage.dfs.core.windows.net/'
WITH STORAGE CREDENTIAL vendor_cred;
```

### Step 3

Create External Volume.

```sql
CREATE EXTERNAL VOLUME vendor_csv_volume
LOCATION 'abfss://vendor@companystorage.dfs.core.windows.net/dailyfiles/';
```

### Step 4

Grant Permissions.

```sql
GRANT READ FILES
ON VOLUME vendor_csv_volume
TO data_analyst_team;
```

### Step 5

Access Files.

```python
df = spark.read.csv(
    "/Volumes/main/raw/vendor_csv_volume/customers.csv",
    header=True
)
```

---

# Why is Unity Catalog a Game Changer Compared to Hive Metastore?

| Feature | Hive Metastore | Unity Catalog |
|----------|---------------|---------------|
| Scope | Workspace Level | Account Level |
| Permissions | Basic Permissions | Fine-Grained Security |
| Lineage | Not Available | Built-In Lineage |
| Auditing | Limited | Centralized Auditing |
| Row-Level Security | Not Available | Supported |
| Column Masking | Not Available | Dynamic Data Masking |
| Volumes | Not Available | Governed Volumes |
| Multi-Workspace Governance | Weak | Enterprise Governance |

## Summary

Unity Catalog provides:

- Centralized Governance
- Fine-Grained Security
- Built-in Lineage
- Auditing
- Data Sharing
- File Governance through Volumes
- Enterprise-Level Multi-Workspace Management

making it the recommended governance solution for modern Databricks implementations.
