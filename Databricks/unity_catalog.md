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
