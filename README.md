
# README: Oracle Pluggable Database (PDB) Creation and Management

## Introduction

A **Pluggable Database (PDB)** is a self-contained, fully functional Oracle database that resides within a **Container Database (CDB)**. PDBs allow easy provisioning, management, and consolidation of multiple databases. In this guide, we'll create a PDB, perform alterations, and manage users. This README will walk you through the key SQL operations and their outputs.

---

## Enter into SQLPlus

Before starting, enter into SQLPlus as the **SYSDBA** user with the following command:

```bash
sqlplus sys/excuse@localhost:1521/xe AS SYSDBA
```

This logs you into Oracle as a DBA, allowing you to manage the databases.

---

## Check Existing PDBs

To check the list of Pluggable Databases (PDBs) within the Container Database (CDB), use:

```sql
show pdbs;
```

**Output:**
```
CON_ID CON_NAME     OPEN MODE  RESTRICTED
------- ------------ ---------- ----------
2       PDB$SEED     READ ONLY  NO
3       XEPDB1       READ WRITE NO
```

---

## Create a New Pluggable Database (PDB)

We will now create a new PDB called `plsql_class2024db`. The `FILE_NAME_CONVERT` clause is used to specify the location of the data files for the new PDB.

```sql
CREATE PLUGGABLE DATABASE plsql_class2024db
  FROM XEPDB1
  FILE_NAME_CONVERT =
  ('C:\app\Administrator\product\21c\oradata\XE\XEPDB1',
   'C:\app\Administrator\product\21c\oradata\XE\plsql_class2024db');
```

**Output:**
```
Pluggable database created.
```

---

## Open the New PDB

To use the PDB, you need to open it. The following command will open the `plsql_class2024db` PDB:

```sql
ALTER PLUGGABLE DATABASE plsql_class2024db OPEN;
```

**Output:**
```
Pluggable database altered.
```

---

## Save the State of the PDB

To ensure the PDB automatically opens when the CDB is restarted, you must save its state:

```sql
ALTER PLUGGABLE DATABASE plsql_class2024db SAVE STATE;
```

**Output:**
```
Pluggable database altered.
```

---

## Create a New User in the PDB

Now that the PDB is open, let's create a new user `mu_plsqlauca` for database operations:

```sql
CREATE USER mu_plsqlauca IDENTIFIED BY admin;
```

**Output:**
```
User created.
```

---

## Create and Delete a PDB

### 1. Create a PDB to be Deleted

Let's create another PDB named `mu_to_delete_pdb`, which we will later delete:

```sql
CREATE PLUGGABLE DATABASE mu_to_delete_pdb
  FROM XEPDB1
  FILE_NAME_CONVERT =
  ('C:\app\Administrator\product\21c\oradata\XE\XEPDB1',
   'C:\app\Administrator\product\21c\oradata\XE\mu_to_delete_pdb');
```

**Output:**
```
Pluggable database created.
```

### 2. Delete the PDB

Before dropping a PDB, it must be closed. First, we close the `mu_to_delete_pdb` PDB:

```sql
ALTER PLUGGABLE DATABASE mu_to_delete_pdb CLOSE IMMEDIATE;
```

**Output:**
```
Pluggable database altered.
```

Finally, drop the `mu_to_delete_pdb` along with its associated data files:

```sql
DROP PLUGGABLE DATABASE mu_to_delete_pdb INCLUDING DATAFILES;
```

**Output:**
```
Pluggable database dropped.
```

---

## Conclusion

This guide demonstrates how to create, open, manage, and delete pluggable databases in Oracle SQLPlus. You now have a functional PDB named `plsql_class2024db` and a new user `mu_plsqlauca` ready for operations.



