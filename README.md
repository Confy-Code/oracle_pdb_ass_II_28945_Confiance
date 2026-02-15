# Oracle Pluggable Databases(PDB) Management
### Author: Jean Confiance ISINGIZWE
### ID : 28945
### Institution : Adventist University of Central Africa(AUCA)

---

#### Repository link : `https://github.com/Confy-Code/oracle_pdb_ass_II_28945_Confiance.git`
#### PDB Name created : `co_pdb_28945`
#### Issues Encountered: Yes

---

## Overview of tasks

This project demonstrates the installation, configuration, and management of Oracle Database 21c using the Multitenant Architecture (CDB + PDB).  

The tasks included:

- Creating and managing Pluggable Databases (PDBs)
- Creating and managing database users
- Deleting a PDB completely
- Configuring and accessing Oracle Enterprise Manager (OEM)

The objective was to build a fully functional Oracle environment and perform essential DBA operations successfully.

---

## Oracle Environment Used

- **Oracle Version:** Oracle Database 21c (Enterprise Edition)
- **Architecture:** Multitenant (CDB + PDB)
- **Container Database (CDB):** ORCL
- **Primary PDB:** `CO_PDB_28945`
- **Temporary PDB:** `CO_TO_DELETE_PDB_28945`
- **Client Tool:** Oracle SQL Developer
- **OEM Access:** https://localhost:5065/em
- **Operating System:** Windows 10

---

# Task 1: PDB Creation and User Setup

## Objective
Create a new Pluggable Database following the required naming convention and create a user inside it.

## Steps Performed

1. Connected as `SYS` with `SYSDBA` role.
2. Created the PDB:

```sql
CREATE PLUGGABLE DATABASE CO_PDB_28945
ADMIN USER pdbadmin IDENTIFIED BY Oracle123
FILE_NAME_CONVERT = ('pdbseed','CO_PDB_28945');
```

<img width="1178" height="678" alt="PDB_created" src="https://github.com/user-attachments/assets/dfea1e05-625b-4896-8c84-59c98e7efe1b" />

3. Opened the PDB and we switched to it:

```sql
ALTER PLUGGABLE DATABASE CO_PDB_28945 OPEN;
ALTER SESSION SET CONTAINER = CO_PDB_28945;
```

<img width="1208" height="681" alt="open_mode_rw" src="https://github.com/user-attachments/assets/beb2562c-8d42-47c0-a636-c9b76df201d7" />

4. Created required user and granted him all some privileges:

```sql
CREATE USER confiance_plsqlauca_28945 IDENTIFIED BY 1234;
GRANT CONNECT, RESOURCE TO confiance_plsqlauca_28945;
```

<img width="1131" height="650" alt="user_created" src="https://github.com/user-attachments/assets/c2e3d92d-ae6f-4022-be2a-79c24d5a7bdb" />

---

# Task 2: Temporary PDB Creation and Deletion
## Objective
Create a temporary PDB and delete it completely.

## Steps Performed

1. Created temporary PDB:

```sql
CREATE PLUGGABLE DATABASE CO_TO_DELETE_PDB_28945
ADMIN USER pdbadmin IDENTIFIED BY Oracle123
FILE_NAME_CONVERT = ('pdbseed','CO_TO_DELETE_PDB_28945');
```

<img width="1366" height="751" alt="temporal_pdb" src="https://github.com/user-attachments/assets/76273920-1035-4733-9c55-80708d7e7d20" />


2. Opened and closed the PDB before dropping it:

```sql
ALTER PLUGGABLE DATABASE CO_TO_DELETE_PDB_28945 OPEN;
ALTER PLUGGABLE DATABASE CO_TO_DELETE_PDB_28945 CLOSE IMMEDIATE;
```

<img width="1366" height="768" alt="pdb_to_delete_working" src="https://github.com/user-attachments/assets/386c5bb2-a6ae-44f6-a04c-2904a34b837e" />

3. Dropped the PDB including datafiles, and verified its execution:

```sql
DROP PLUGGABLE DATABASE CO_TO_DELETE_PDB_28945 INCLUDING DATAFILES;
SELECT name FROM v$pdbs;
```
<img width="1366" height="768" alt="pdb_dropped" src="https://github.com/user-attachments/assets/5dc7b6b9-3bab-4b02-a7cf-8b040c8f2007" />

<img width="1366" height="768" alt="pdb_deleted_proof" src="https://github.com/user-attachments/assets/34bd959a-9066-4ec8-bc97-79286c4ee273" />

### OUTPUT:

- Temporary PDB successfully created.
- PDB completely removed including datafiles.
- Verification confirmed it no longer exists.

---

# Task 3: Oracle Enterprise Manager (OEM)
## Objective
Configure and access Oracle Enterprise Manager (OEM).

## Steps Performed

1.Verified HTTPS port to check already-used ports:

```sql
SELECT DBMS_XDB_CONFIG.GETHTTPSPORT FROM dual;
```

2. Assigned port 5065(unused port) to the HTTPS port:

```sql
EXEC dbms_xdb_config.sethttpsport(5065);
```

3. Used the assigned port to access the webpage of the OEM via `https://localhost:5065/em`.

### Dashboard Verification

The OEM dashboard successfully displayed:
- The Oracle Database environment, CDB, PDB tasks, Listener status, Host information and our username(SYS), 

This confirms that OEM was properly configured and functioning.

<img width="1366" height="724" alt="OEM_dashboard" src="https://github.com/user-attachments/assets/95ed53ea-fd8a-4332-8186-3b1d4d0fb2d2" />

<img width="1366" height="728" alt="Resources_tasks_OEM" src="https://github.com/user-attachments/assets/88e4b5f5-60c3-4a6f-a0c6-c131eb438f3b" />

---

## Challenges Faced & Solutions
### 1. Invalid Container Name (CDB$ROOT)
Yhe issue was the Login error when entering CDB$ROOT as container name.

Solution:
Ensured correct login format and verified available containers using:

```sql
SHOW PDBS
```
This showed us that the correct name to enter was instead the `CO_PDB_28945` container.

### 2. View or Table Not Found (v$pdbs)
Error occurred when querying v$pdbs. This was caused by the fact that the user we were using had no such privileges.

Solution:
Logged in as SYS with SYSDBA privileges

### 3. OEM Login Popup Loop
Browser login popup repeatedly appeared. The Chrome's security certificate constantly enforced us to re-enter our credentials
till forever.

Solution:
At first we were entering the container's name as `co_pdb_28945` in small letters, just like how we declared it when created. However,
by querying `SELECT name FROM v$pdbs`, the console showed us that the correct name was `CO_PDB_28945` in capital letters. This 
minor misunderstanding caused a serious issue, but later it was handled successfully.

---

### References
- Oracle Corporation. (2019). Administering a CDB and PDBs : `https://docs.oracle.com/en/database/oracle/oracle-database/19/multi/`
- Youtube - Oracle Database 21c Installation - Enterprise Edition - Connect with SQL Developer-Unlock HR, Scott : `https://youtu.be/62loxIrfxzk?si=6G7hvDRRN9ZBWK83`
- Google's Gemini AI
- Lecturer Eric's notes

---
All sources were properly cited. Implementation and analysis represent original work. No AI-generated content was copied without attribution or adaption.





    

   







