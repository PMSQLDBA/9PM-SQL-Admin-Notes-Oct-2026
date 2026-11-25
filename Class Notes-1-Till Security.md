# **SQL Server Database Administration – Structured Training Notes**

Beginner to Intermediate | Lesson-wise curriculum | Microsoft-aligned technical guidance

Training material: 
Starting from SQL Server fundamentals and progressing through installation, configuration, database administration, backup and recovery, and SQL Server security.

## **Learning structure**

1. **Module 1 – SQL Server Fundamentals**

   SQL Server introduction, RDBMS, DBA responsibilities, versions, editions and architecture.
2. **Module 2 – Operating Systems and Virtualization**

   Windows Server, Linux, VMware Workstation, VirtualBox and lab infrastructure.
3. **Module 3 – SQL Server Installation**

   Prerequisites, installation methods, components, instances and configuration.
4. **Module 4 – SQL Server Tools and Architecture**

   SSMS, Configuration Manager, services, ports, error logs and system databases.
5. **Module 5 – Database Administration**

   Database creation, database files, filegroups, recovery models and database states.
6. **Module 6 – Backup and Recovery**

   Full, differential, transaction log, copy-only, restore sequences and point-in-time recovery.
7. **Module 7 – SQL Server Security**

   Windows and SQL logins, users, groups, SIDs, roles, permissions and orphaned users.

**Scope:** 
This document covers your requested foundation through SQL Server Security. SQL Agent automation, maintenance, performance tuning, HA/DR, patching and migration should follow as separate advanced modules.

# **Module 1 – SQL Server Fundamentals**

## **Lesson 1.1 – Introduction to SQL Server**

1. SQL Server is a Relational Database Management System (RDBMS) developed by Microsoft.
2. It is used to store, manage, retrieve and secure structured data.
3. SQL Server supports:
   - Data storage.
   - Transaction processing.
   - Data retrieval.
   - Database security.
   - Backup and recovery.
   - High availability and disaster recovery.
4. SQL Server uses Transact-SQL (T-SQL) to interact with databases.

### **Database-related technologies**

| Technology           | Purpose                          |
| -------------------- | -------------------------------- |
| Microsoft SQL Server | RDBMS                            |
| Oracle Database      | RDBMS                            |
| PostgreSQL           | Open-source RDBMS                |
| MongoDB              | Document-oriented NoSQL database |

### **Operating systems**

- Windows Server.
- Linux distributions supported by Microsoft SQL Server.

**Important:** SQL Server 2017 introduced Linux support. SQL Server is not restricted to Windows Server installations.

## **Lesson 1.2 – SQL Developer vs SQL DBA**

| SQL Developer                 | SQL Database Administrator       |
| ----------------------------- | -------------------------------- |
| Develops stored procedures    | Manages SQL Server instances     |
| Writes SQL queries            | Configures SQL Server            |
| Creates tables and views      | Manages database files           |
| Develops functions            | Configures backup and recovery   |
| Optimizes application queries | Monitors database performance    |
| Implements business logic     | Manages security and permissions |
| Works with application teams  | Handles database incidents       |

**Real-time scenario:**

An application team reports that a customer search query is taking 30 seconds.

- Developer: Investigates query logic, joins and execution plans.
- DBA: Investigates blocking, CPU, memory, I/O, indexing and instance-level resource pressure.

Both teams may need to collaborate.

## **Lesson 1.3 – SQL Server Versions and Internal Version Numbers**

| SQL Server version | Internal major version |
| ------------------ | ---------------------- |
| SQL Server 2000    | 8.0                    |
| SQL Server 2005    | 9.0                    |
| SQL Server 2008    | 10.0                   |
| SQL Server 2008 R2 | 10.5                   |
| SQL Server 2012    | 11.0                   |
| SQL Server 2014    | 12.0                   |
| SQL Server 2016    | 13.0                   |
| SQL Server 2017    | 14.0                   |
| SQL Server 2019    | 15.0                   |
| SQL Server 2022    | 16.0                   |
| SQL Server 2025    | 17.0                   |

### **Important DBA commands**

```
-- Identify SQL Server instance name
SELECT @@SERVERNAME;

-- Identify SQL Server version and build
SELECT @@VERSION;

-- Get detailed product information
SELECT
    SERVERPROPERTY('ProductVersion') AS ProductVersion,
    SERVERPROPERTY('ProductLevel') AS ProductLevel,
    SERVERPROPERTY('Edition') AS Edition,
    SERVERPROPERTY('ProductMajorVersion') AS MajorVersion;
```

**DBA responsibility:** Always identify the SQL Server version, edition, build number and patch level before performing maintenance or troubleshooting.

## **Lesson 1.4 – SQL Server Editions**

| Edition    | Primary usage                            |
| ---------- | ---------------------------------------- |
| Express    | Small applications and limited workloads |
| Developer  | Development and testing                  |
| Standard   | Production workloads                     |
| Enterprise | Advanced production workloads            |
| Evaluation | Temporary evaluation and testing         |

Important distinctions:

- Developer Edition provides Enterprise Edition functionality but is licensed for development and testing, not production.
- Express Edition is free and has resource and feature limitations.
- Standard and Enterprise are commercial production editions.
- Evaluation Edition is time-limited.

**Correction to your original notes:** Standard Edition is not exclusively for production, and Enterprise Edition is not mandatory for production. 
Edition selection depends on workload requirements, features, scalability and licensing.

## **Lesson 1.5 – SQL Server Instance Fundamentals**

An instance is an installation of the SQL Server Database Engine.

A Windows Server can host multiple SQL Server instances, subject to operating system and resource limitations.

### **Types of instances**

**1. Default instance**

Example:

```
HYDERABAD
```

Default instance service name:

```
MSSQLSERVER
```

**2. Named instance**

Examples:

```
HYDERABAD\ProdSQL01
HYDERABAD\ProdSQL02
HYDERABAD\ProdSQL49
```

Named instance service name:

```
MSSQL$ProdSQL01
```

### **Important points**

- One default Database Engine instance can exist on a Windows server.
- Multiple named instances can coexist.
- Each instance has its own system databases and configuration.
- Each instance requires its own SQL Server service.
- Instances can use different TCP ports.
- Resource allocation must be planned for multiple instances.

## **Lesson 1.6 – SQL Server Network Connectivity**

Default TCP port:

```
TCP 1433
```

Example:

```
Server Name: HYDERABAD
Port: 1433
```

Named instance:

```
HYDERABAD\ProdSQL01
```

Named instances may use dynamic ports or explicitly configured static ports.

Example:

```
HYDERABAD,56789
```

### **DBA responsibilities**

1. Enable TCP/IP using SQL Server Configuration Manager.
2. Configure a static TCP port when required.
3. Configure Windows Firewall rules.
4. Validate network connectivity.
5. Confirm SQL Server is listening on the expected port.
6. Coordinate with network and infrastructure teams.

**Important:** TCP 1433 is the conventional default for a default instance, not a mandatory port for every SQL Server instance.

# **Module 2 – Operating Systems and Virtualization**

## **Lesson 2.1 – Windows Server Fundamentals**

Windows Server provides the operating system environment required for SQL Server deployments.

Common Windows Server releases:

- Windows Server 2012.
- Windows Server 2016.
- Windows Server 2019.
- Windows Server 2022.
- Windows Server 2025.

### **Windows Server editions**

1. Standard.
2. Datacenter.

### **Server deployment models**

- Standalone server.
- Domain-joined member server.
- Windows Server Failover Cluster (WSFC).
- Virtualized server.

## **Lesson 2.2 – Standalone Server vs Domain Environment**

### **Standalone environment**

- Local Windows accounts are created on the server.
- Local security groups manage access.
- Suitable for isolated or small environments.

### **Domain environment**

Example:

```
Domain: Hyderabad.xyz.com

Member Servers:
    Telangana.xyz.bank.com
    AP.xyz.bank.com
    Kerala.xyz.bank.com
```

Active Directory is used to centrally manage:

- Windows user accounts.
- Security groups.
- Computer accounts.
- Authentication.
- Group policies.

### **Real-time scenario: New DBA joins an organization**

1. HR raises an employee onboarding request.
2. Windows team creates the employee's domain account.
3. Windows team adds the employee to the required AD group.
4. DBA team grants the AD group access to SQL Server.
5. Access is validated.
6. The request is documented and closed.

## **Lesson 2.3 – Virtualization**

Virtualization allows multiple virtual machines to run on one physical computer.

Common virtualization platforms:

- VMware Workstation.
- Oracle VirtualBox.

### **Suggested training lab**

| Virtual machine | Purpose              |
| --------------- | -------------------- |
| DC01            | Domain Controller    |
| VMSQL01         | Primary SQL Server   |
| VMSQL02         | Secondary SQL Server |

Your original lab configuration uses approximately 8 GB RAM. This is extremely constrained for three Windows Server VMs and SQL Server HA/DR testing.

For a comfortable lab, use more RAM if possible, or reduce the number of simultaneously running VMs.

## **Lesson 2.4 – SQL Server Lab Disk Configuration**

Suggested drive allocation:

| Drive | Purpose                                    |
| ----- | ------------------------------------------ |
| C:\\  | Windows OS                                 |
| D:\\  | SQL Server installation media and software |
| E:\\  | SQL Server data files                      |
| L:\\  | SQL Server transaction log files           |
| T:\\  | TempDB files                               |
| H:\\  | SQL Server backup files                    |

These are logical storage-planning conventions, not mandatory Microsoft requirements.

### **Infrastructure team responsibilities**

- Provision virtual machines.
- Install Windows Server.
- Allocate CPU and RAM.
- Configure IP addresses.
- Provision storage.
- Configure DNS and domain connectivity.
- Provide SQL Server installation media.
- Configure required firewall access.

# **Module 3 – SQL Server Installation and Configuration**

## **Lesson 3.1 – SQL Server Installation Prerequisites**

Before installing SQL Server, the DBA must validate hardware and software requirements.

### **Hardware requirements**

1. CPU and processor architecture.
2. Available RAM.
3. Free disk space.
4. Storage performance.
5. Network connectivity.

### **Software requirements**

1. Supported Windows Server or Windows operating system.
2. Required .NET Framework version.
3. SQL Server installation media.
4. Required service accounts.
5. Appropriate installation permissions.

### **Important**

SQL Server 2025 supports Windows Server 2019, 2022 and 2025, subject to edition-specific compatibility. Always check the version-specific compatibility matrix before installation.

Reference: [SQL Server 2025 hardware and software requirements](https://learn.microsoft.com/en-us/sql/sql-server/install/hardware-and-software-requirements-for-installing-sql-server-2025?view=sql-server-linux-2017\).

## **Lesson 3.2 – SQL Server Installation Methods**

Three common installation methods:

**1. GUI installation**

- Launch SQL Server Setup.
- Select New SQL Server stand-alone installation.
- Select the required edition.
- Accept license terms.
- Install setup support files.
- Select required features.
- Configure the instance.
- Configure service accounts.
- Configure Database Engine authentication.
- Configure data directories.
- Complete installation.

**2. Command-line installation**

- Use command-line parameters.
- Specify instance name.
- Specify features.
- Configure authentication.
- Configure service accounts.
- Specify installation directories.

**3. Configuration file installation**

- Create a SQL Server configuration file.
- Define installation parameters.
- Reuse the configuration file for repeatable installations.
- Maintain consistent configurations across environments.

## **Lesson 3.3 – SQL Server Installation Components**

| Component                   | Purpose                            |
| --------------------------- | ---------------------------------- |
| Database Engine Services    | Stores and processes database data |
| SQL Server Replication      | Replicates database data           |
| Full-Text Search            | Supports full-text queries         |
| Integration Services (SSIS) | Data integration and ETL           |
| Analysis Services (SSAS)    | Analytical workloads               |
| Reporting Services (SSRS)   | Report generation                  |

Note: SSIS, SSAS and SSRS are separate components with their own installation and deployment considerations. They are not automatically installed with the Database Engine.

## **Lesson 3.4 – Default vs Named Instance Installation**

### **Default instance**

Example:

```
Instance Name: MSSQLSERVER
Server Name: HYDERABAD
```

### **Named instance**

Example:

```
Instance Name: ProdSQL01
Server Name: HYDERABAD\ProdSQL01
```

During installation, configure:

- Instance ID.
- Instance root directory.
- SQL Server service accounts.
- Database Engine startup type.
- Authentication mode.
- SQL Server administrators.
- Data and log directories.
- TempDB directories.

## **Lesson 3.5 – SQL Server Authentication Modes**

Two authentication modes are available.

**1. Windows Authentication**

- Uses Windows or Active Directory identities.
- Supports Windows users and groups.
- Preferred for domain-based enterprise environments.

**2. Mixed Mode Authentication**

- Supports Windows Authentication.
- Supports SQL Server Authentication.
- SQL logins use SQL Server-managed credentials.

Example of an authentication error from your notes:

```
Login failed for user 'sa'.

Server is configured for Integrated
authentication only.
```

Explanation:

The SQL Server instance is configured for Windows Authentication only. SQL Server Authentication is not enabled.

Resolution:

1. Connect using an authorized Windows login.
2. Open Server Properties.
3. Select Security.
4. Select SQL Server and Windows Authentication mode.
5. Apply the change.
6. Restart the SQL Server Database Engine service.
7. Validate the required SQL login.

**Security recommendation:** Prefer Windows Authentication and individual named accounts. Avoid routine application or DBA access through the `sa` account.

## **Lesson 3.6 – SQL Server Post-Installation Configuration**

After installation, perform the following checks.

| No. | Configuration           | Validation                   |
| --- | ----------------------- | ---------------------------- |
| 1   | Version and edition     | SERVERPROPERTY               |
| 2   | Database Engine service | Running                      |
| 3   | SQL Server Agent        | Running where required       |
| 4   | TCP/IP                  | Enabled if required          |
| 5   | TCP port                | Correct configured port      |
| 6   | Firewall                | Required access allowed      |
| 7   | Service accounts        | Approved accounts            |
| 8   | Memory                  | Max Server Memory configured |
| 9   | TempDB                  | Appropriate files and sizing |
| 10  | Data directories        | Approved storage             |
| 11  | Backup directories      | Accessible storage           |
| 12  | Error logs              | Reviewed                     |
| 13  | Database Mail           | Configured where required    |
| 14  | Database integrity      | CHECKDB strategy planned     |
| 15  | Backup strategy         | Approved and tested          |

### **Example: Max Server Memory**

Your notes use this example:

```
Total Server RAM = 8 GB
SQL Server allocation = 70%
SQL Server memory = 5.6 GB
```

This is an illustrative calculation, not a universal Microsoft recommendation.

The DBA must reserve memory for:

- Windows operating system.
- SQL Server Agent.
- Backup processes.
- Monitoring tools.
- Other applications.
- Additional SQL Server instances.

Example configuration:

```
EXEC sys.sp_configure
    'show advanced options', 1;
GO

RECONFIGURE;
GO

EXEC sys.sp_configure
    'max server memory (MB)', 5734;
GO

RECONFIGURE;
GO
```

The value 5734 MB is approximately 5.6 GiB. Adjust it according to actual server requirements.

## **Lesson 3.7 – SQL Server Configuration Manager**

SQL Server Configuration Manager is used to manage SQL Server-related services and network configuration.

Major activities:

1. Start and stop SQL Server services.
2. Configure service startup types.
3. Configure service accounts.
4. Enable TCP/IP.
5. Configure TCP ports.
6. Configure SQL Server aliases.
7. Review network protocols.

Important SQL Server services:

| Service                         | Purpose                                  |
| ------------------------------- | ---------------------------------------- |
| SQL Server Database Engine      | Database processing                      |
| SQL Server Agent                | Job scheduling and automation            |
| SQL Server Browser              | Provides instance connection information |
| SQL Server Integration Services | ETL services, where installed            |

## **Lesson 3.8 – SQL Server Startup Parameters**

Your notes contain these startup parameters.

| Parameter | Purpose                          |
| --------- | -------------------------------- |
| `-d`      | Location of master data file     |
| `-l`      | Location of master log file      |
| `-e`      | Location of SQL Server error log |

Example:

```
-dC:\Program Files\Microsoft SQL Server\
MSSQL15.MSSQLSERVER\MSSQL\DATA\master.mdf

-lC:\Program Files\Microsoft SQL Server\
MSSQL15.MSSQLSERVER\MSSQL\DATA\mastlog.ldf

-eC:\Program Files\Microsoft SQL Server\
MSSQL15.MSSQLSERVER\MSSQL\Log\ERRORLOG
```

These are instance startup parameters. Incorrect changes can prevent SQL Server from starting.

# **Module 4 – SQL Server Tools and Architecture**

## **Lesson 4.1 – SQL Server Management Tools**

### **1. SQL Server Management Studio (SSMS)**

[SQL Server Management Studio (SSMS)](https://images.openai.com/static-rsc-4/5SlcHnAQxYid5i7dF4B7rTJt5GqIU6Weavg84Lc56qZGXOJ16l9pCtk5giB4lprlrY0zHU8C6atYmmYtdm9DzM6la75IVSYlDTMMswgO67blY4DSVYMWaK-Y1mb50IpG8KVrzg1NNjwVIprd7ZbOJhLPAjltfn54wKy1nSQ0bFo?purpose=inline)

[quackit.com](https://www.quackit.com/sql_server/sql_server_2016/tutorial/sql_server_management_studio.cfm)

SSMS is the graphical client tool used to administer SQL Server.

Activities:

- Connect to SQL Server.
- Create and manage databases.
- Execute T-SQL queries.
- Manage logins and permissions.
- Configure backup and restore.
- Manage SQL Server Agent jobs.
- Review execution plans.
- Monitor database activity.

### **2. SQL Server Configuration Manager**

Used for service and network configuration.

### **3. SQL Server Error Log**

Used to investigate:

- SQL Server startup failures.
- Login failures.
- Backup failures.
- Database recovery messages.
- Deadlock-related messages where logged.
- Server-level errors.

Read the current error log:

```
EXEC sys.xp_readerrorlog;
```

Search for login failures:

```
EXEC sys.xp_readerrorlog
    0, 1, N'Login failed';
```

### **4. Windows Event Viewer**

[How To View And Analyze Logs With Windows Event Viewer | Better Stack Community](https://images.openai.com/static-rsc-4/yozx5i58YRk4zF2f2i-T031vZ2IdpTzyKq8IaElQK-nGekq--sYFbc-c-TYqq1Z8KnfpqHwAdfRh_BxPkYZTZLD25UVzomLqXbhY0RIB63DCZfE_-xDv-uEMEgyl_PS8gvV5iduEqEEWngtXfCpLjT4eioFARh6hLXCblymsy2A?purpose=inline)

[betterstack.com](https://betterstack.com/community/guides/logging/how-to-view-and-analyze-logs-with-windows-event-viewer/)

Used to investigate:

- Windows service failures.
- Operating system errors.
- Storage issues.
- Application errors.
- System crashes.
- Unexpected restarts.

Relevant logs:

- Application.
- System.
- Security.

**Real-time DBA scenario:** SQL Server stops unexpectedly.

Investigation sequence:

1. Check SQL Server service status.
2. Review SQL Server ERRORLOG.
3. Review Windows System Event Log.
4. Check storage availability.
5. Check memory and CPU pressure.
6. Identify the root cause.
7. Restore service following the approved incident procedure.

## **Lesson 4.2 – SQL Server Database Engine Architecture**

SQL Server architecture has two major components.

**1. Relational Engine**

Responsible for:

- Query parsing.
- Query optimization.
- Execution plans.
- Query execution coordination.

**2. Storage Engine**

Responsible for:

- Data access.
- Buffer management.
- Transaction processing.
- Locking and latching.
- Data and transaction log operations.

### **Basic query processing example**

```
SELECT *
FROM employee
WHERE empname = 'Jay';
```

Execution flow:

1. SQL Server receives the query.
2. Parser validates syntax.
3. Algebrizer performs binding and semantic checks.
4. Query optimizer evaluates execution alternatives.
5. Execution engine executes the selected plan.
6. Storage Engine retrieves the required data.
7. Results are returned to the client.

## **Lesson 4.3 – SQL Server Database Files**

A SQL Server database consists of data files and transaction log files.

| Extension | Description          |
| --------- | -------------------- |
| MDF       | Primary data file    |
| NDF       | Secondary data file  |
| LDF       | Transaction log file |

Example:

```
Demodb
    Demodb.mdf
    Demodb_log.ldf
```

A database can contain:

- One primary data file.
- Multiple secondary data files.
- One or multiple transaction log files.

Important:

- MDF and NDF files contain database data.
- LDF files contain transaction log records.
- A database is not simply a pair of files; it is a logical database structure backed by physical files.

## **Lesson 4.4 – System Databases**

SQL Server has four primary system databases and one hidden resource database.

| Database | Purpose                                                       |
| -------- | ------------------------------------------------------------- |
| master   | Instance-level configuration and metadata                     |
| model    | Template for newly created databases                          |
| msdb     | SQL Agent jobs, backup history and other operational metadata |
| tempdb   | Temporary objects and internal workspace                      |
| Resource | System object definitions                                     |

### **Important DBA responsibilities**

**master**

- Protect the database.
- Back up after relevant instance-level changes.
- Understand its recovery requirements.

**model**

- Configure defaults for newly created databases.
- Avoid unnecessary modifications.

**msdb**

- Monitor backup history.
- Manage SQL Agent jobs and schedules.
- Protect job and operational metadata.

**tempdb**

- Monitor file growth.
- Configure appropriate file sizing.
- Monitor contention and space usage.

## **Lesson 4.5 – SQL Server Data Storage Hierarchy**

Understand these storage units:

```
TB
GB
MB
KB
Bytes
Bits
```

SQL Server storage concepts:

1. Database.
2. Filegroup.
3. Data file.
4. Extent.
5. Page.
6. Row.

Important concepts:

- SQL Server data pages are 8 KB.
- An extent consists of eight contiguous pages.
- An extent is therefore 64 KB.

### **Practical example: Index concept**

Suppose an employee table contains 10 million records.

```
SELECT *
FROM employee
WHERE empname = 'Jay';
```

Without an appropriate index, SQL Server may need to examine many rows.

With a suitable index, SQL Server may locate matching records more efficiently.

An index is similar to the index at the back of a book: it helps locate information without scanning every page.

However, indexes also introduce storage and maintenance overhead.

# **Module 5 – Database Administration**

## **Lesson 5.1 – Database Environments**

Common enterprise environments:

| Environment | Purpose                          |
| ----------- | -------------------------------- |
| Development | Application development          |
| QA          | Quality assurance                |
| Testing     | Functional and technical testing |
| UAT         | User acceptance testing          |
| Staging     | Pre-production validation        |
| Production  | Live business operations         |

### **Real-time environment example**

```
SQL Server Infrastructure

Total Servers: 200

Production: 150
Non-Production: 50
```

Another example:

```
Small Environment:
100 SQL Servers

Medium Environment:
200 SQL Servers

Large Environment:
500 SQL Servers
```

These are illustrative infrastructure sizes, not fixed industry standards.

## **Lesson 5.2 – Database Creation**

Example:

```
CREATE DATABASE iphone18;
GO
```

Check database properties:

```
EXEC sys.sp_helpdb 'iphone18';
```

Check database status:

```
SELECT
    name,
    state_desc,
    user_access_desc,
    recovery_model_desc,
    compatibility_level
FROM sys.databases;
```

## **Lesson 5.3 – Database States**

Important database states:

| State            | Meaning                                                   |
| ---------------- | --------------------------------------------------------- |
| ONLINE           | Database is available                                     |
| OFFLINE          | Database is intentionally offline                         |
| RESTORING        | Restore sequence is incomplete                            |
| RECOVERING       | Database recovery is in progress                          |
| RECOVERY_PENDING | Recovery cannot start because of a resource-related issue |
| SUSPECT          | Recovery did not complete successfully                    |
| EMERGENCY        | Restricted troubleshooting state                          |

Also understand database access modes:

- MULTI_USER.
- SINGLE_USER.
- RESTRICTED_USER.

And updateability:

- READ_WRITE.
- READ_ONLY.

## **Lesson 5.4 – Database Recovery Models**

SQL Server supports three recovery models.

### **1. SIMPLE**

- Does not support transaction log backups.
- Log space can be reused after checkpoint when eligible.
- Supports full database backups.
- Supports differential database backups.
- Does not support point-in-time recovery.

Common use cases:

- Development.
- Test.
- QA databases where the business accepts the recovery limitations.

### **2. FULL**

- Supports full backups.
- Supports differential backups.
- Supports transaction log backups.
- Supports point-in-time recovery when the required backup chain is available.
- Requires regular transaction log backups to manage log reuse.

Common use cases:

- Banking applications.
- Financial databases.
- Healthcare applications.
- Business-critical production databases.

### **3. BULK_LOGGED**

- Similar to FULL recovery.
- Supports transaction log backups.
- Allows minimal logging for qualifying bulk operations.
- Point-in-time recovery is unavailable if the relevant log backup contains bulk-logged changes.

Common use cases:

- Certain bulk data-loading operations.
- Large data imports.
- Approved bulk processing activities.

### **Important correction**

The statement that FULL recovery always causes slower performance is incorrect.

Recovery model selection does not automatically make a database slow. Workload, logging operations, storage performance and backup frequency influence performance.

Reference: [Microsoft – Recovery Models](https://learn.microsoft.com/en-us/sql/relational-databases/backup-restore/recovery-models-sql-server?view=sql-server-ver17\).

## **Lesson 5.5 – Change Database Recovery Model**

```
USE master;
GO

ALTER DATABASE iphone18
SET RECOVERY FULL;
GO
```

Change to SIMPLE:

```
ALTER DATABASE iphone18
SET RECOVERY SIMPLE;
GO
```

Change to BULK_LOGGED:

```
ALTER DATABASE iphone18
SET RECOVERY BULK_LOGGED;
GO
```

Important: After changing from SIMPLE to FULL or BULK_LOGGED, take a full database backup to establish the required log backup chain before relying on transaction log backups.

## **Lesson 5.6 – Database File Management**

Inspect database file information:

```
USE iphone18;
GO

SELECT
    name AS LogicalFileName,
    physical_name,
    type_desc,
    size * 8 / 1024 AS SizeMB,
    growth,
    is_percent_growth
FROM sys.database_files;
```

### **File management activities**

1. Create data files.
2. Create transaction log files.
3. Configure filegroups.
4. Configure initial file size.
5. Configure autogrowth.
6. Monitor disk space.
7. Plan file placement.

### **Autogrowth**

Avoid unnecessarily small autogrowth increments.

For example:

```
Poor planning:
Growth = 1 MB

Better planning:
Growth = 512 MB
```

The actual growth setting must be based on database workload and storage capacity.

Use fixed-size growth where practical and monitor growth events.

## **Lesson 5.7 – Database Metadata and System Views**

Important system catalog views:

| View                      | Purpose                                 |
| ------------------------- | --------------------------------------- |
| sys.databases             | Database information                    |
| sys.database_files        | Current database file information       |
| sys.master_files          | Instance-wide database file information |
| sys.server_principals     | Server security principals              |
| sys.database_principals   | Database security principals            |
| sys.server_role_members   | Server role membership                  |
| sys.database_role_members | Database role membership                |

### **Database inventory query**

```
SELECT
    name,
    database_id,
    state_desc,
    recovery_model_desc,
    user_access_desc,
    compatibility_level
FROM sys.databases
ORDER BY name;
```

### **Instance-wide database file inventory**

```
SELECT
    DB_NAME(database_id) AS DatabaseName,
    name AS LogicalFileName,
    physical_name,
    type_desc,
    size * 8 / 1024 AS SizeMB
FROM sys.master_files
ORDER BY DatabaseName;
```

# **Module 6 – Backup and Recovery**

This is one of the most important SQL Server DBA responsibilities.

## **Lesson 6.1 – Backup Fundamentals**

A database backup is a copy of database data or transaction log information that can be used to restore a database.

Main backup types:

1. Full backup.
2. Differential backup.
3. Transaction log backup.
4. Copy-only backup.

Other specialized types include file, filegroup and partial backups.

### **Important extensions**

| Backup type     | Common extension |
| --------------- | ---------------- |
| Full            | .bak             |
| Differential    | .diff or .bak    |
| Transaction log | .trn             |

These extensions are conventions. SQL Server does not require a particular extension.

## **Lesson 6.2 – Full Database Backup**

A full backup contains the database data required to restore the database to a consistent state at the backup completion point.

Example:

```
BACKUP DATABASE iphone18
TO DISK =
'L:\MSSQL\SQLBkps\iphone_full.bak'
WITH
    COMPRESSION,
    CHECKSUM,
    STATS = 10;
GO
```

### **DBA responsibilities**

- Verify backup completion.
- Check backup file size.
- Validate backup history.
- Monitor backup duration.
- Monitor destination disk space.
- Test restore procedures.

## **Lesson 6.3 – Differential Database Backup**

A differential backup captures extents changed since the relevant differential base full backup.

Example:

```
BACKUP DATABASE iphone18
TO DISK =
'L:\MSSQL\SQLBkps\iphone_diff.diff'
WITH
    DIFFERENTIAL,
    COMPRESSION,
    CHECKSUM,
    STATS = 10;
GO
```

Important:

- Differential backups depend on their differential base.
- A normal full backup establishes a new differential base.
- Differential backups can reduce restore time compared with restoring many individual log backups.

## **Lesson 6.4 – Transaction Log Backup**

Transaction log backups are supported in FULL and BULK_LOGGED recovery models.

Example:

```
BACKUP LOG iphone18
TO DISK =
'L:\MSSQL\SQLBkps\iphone_tlog.trn'
WITH
    CHECKSUM,
    STATS = 10;
GO
```

Important:

- SIMPLE recovery does not support log backups.
- A full backup must exist before the first transaction log backup.
- Log backups must be maintained regularly.
- Log backup frequency depends on the business RPO.
- Log backups help manage transaction log reuse.

## **Lesson 6.5 – Recovery Point Objective and Recovery Time Objective**

**RPO – Recovery Point Objective**

Maximum acceptable data loss measured in time.

Example:

```
RPO = 15 minutes
```

The business accepts losing no more than approximately 15 minutes of data.

**RTO – Recovery Time Objective**

Maximum acceptable time to restore the service.

Example:

```
RTO = 2 hours
```

The business expects service restoration within two hours.

### **Example backup strategy**

| Backup type     | Frequency        |
| --------------- | ---------------- |
| Full            | Weekly           |
| Differential    | Every 6 hours    |
| Transaction log | Every 15 minutes |

This is an example, not a universal backup schedule. The final schedule must meet the application's RPO, RTO, workload and storage constraints.

## **Lesson 6.6 – Copy-Only Backup**

A copy-only backup is independent of the regular backup sequence.

Use cases:

- Ad hoc backup.
- Backup before a production change.
- Creating a separate copy for testing.
- Taking a backup without changing the regular differential backup base.

Example:

```
BACKUP DATABASE iphone18
TO DISK =
'L:\MSSQL\SQLBkps\iphone_copyonly.bak'
WITH
    COPY_ONLY,
    COMPRESSION,
    CHECKSUM,
    STATS = 10;
GO
```

Important: A copy-only full backup does not reset the differential base.

## **Lesson 6.7 – Backup Compression**

Example:

```
BACKUP DATABASE dmartdb
TO DISK =
'L:\MSSQL\SQLBkps\dmartdb_compressed.bak'
WITH
    COMPRESSION,
    CHECKSUM,
    STATS = 10;
GO
```

Benefits:

- Reduced backup storage consumption.
- Reduced backup file size.
- Potentially reduced backup I/O.

Considerations:

- CPU overhead.
- Data compressibility.
- Backup throughput.
- Restore performance.

**Important:** Do not assume every database will compress to the same percentage. Compression depends on the actual data.

## **Lesson 6.8 – Backup Retention and Storage Planning**

Example:

```
Backup Drive Capacity = 1 TB

Retention Period = 7 Days

Full Backups = 150 GB per day
```

A seven-day full backup retention would require approximately 1,050 GB, excluding other backup files and overhead.

Therefore, the backup retention strategy must consider:

1. Backup size.
2. Compression ratio.
3. Backup frequency.
4. Differential backup size.
5. Transaction log backup size.
6. Retention period.
7. Free disk capacity.
8. Offsite backup requirements.

### **Microsoft-aligned recommendation**

Do not store the only backup copy on the same physical storage as the database files.

Maintain separate backup storage and an appropriate offsite or immutable backup strategy.

Reference: [Microsoft – Back up and restore SQL Server databases](https://learn.microsoft.com/en-us/sql/relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases/).

## **Lesson 6.9 – Backup Validation Commands**

### **1. RESTORE VERIFYONLY**

```
RESTORE VERIFYONLY
FROM DISK =
'L:\MSSQL\SQLBkps\iphone_full.bak';
GO
```

Checks whether the backup set is readable and appears complete.

It does not replace a test restore.

### **2. RESTORE FILELISTONLY**

```
RESTORE FILELISTONLY
FROM DISK =
'L:\MSSQL\SQLBkps\iphone_full.bak';
GO
```

Displays logical file names and physical file information.

This is important before restoring a database to a different location.

### **3. RESTORE HEADERONLY**

```
RESTORE HEADERONLY
FROM DISK =
'L:\MSSQL\SQLBkps\iphone_full.bak';
GO
```

Displays backup header metadata.

## **Lesson 6.10 – Database Restore Fundamentals**

Two important restore options:

**WITH NORECOVERY**

- Keeps the database in RESTORING state.
- Allows additional backup files to be restored.
- Does not make the database available to users.

**WITH RECOVERY**

- Completes the restore sequence.
- Performs recovery.
- Brings the database online when recovery succeeds.
- Prevents additional backup restores in that restore sequence.

### **Important correction to your notes**

The formula `n-1 = NORECOVERY; n = RECOVERY` is only a simplified classroom explanation.

The actual rule is:

- Restore every required backup in the correct sequence using NORECOVERY.
- Apply RECOVERY only after the final required backup has been restored.

## **Lesson 6.11 – Full, Differential and Log Restore**

Suppose you have these four backups:

```
1. Full Backup
2. Differential Backup
3. Transaction Log Backup 1
4. Transaction Log Backup 2
```

Restore sequence:

1. **Restore Full Backup**

   NORECOVERY

   Database remains in RESTORING state.
2. **Restore Differential Backup**

   NORECOVERY

   Database remains in RESTORING state.
3. **Restore Transaction Log 1**

   NORECOVERY

   Database remains in RESTORING state.
4. **Restore Transaction Log 2**

   RECOVERY

   Database becomes ONLINE.

### **Example T-SQL**

```
-- Step 1: Full backup
RESTORE DATABASE advworks
FROM DISK =
'L:\MSSQL\SQLBkps\Advworks_full.bak'
WITH NORECOVERY, STATS = 5;
GO

-- Step 2: Differential backup
RESTORE DATABASE advworks
FROM DISK =
'L:\MSSQL\SQLBkps\Advworks_diff.diff'
WITH NORECOVERY, STATS = 5;
GO

-- Step 3: Transaction log 1
RESTORE LOG advworks
FROM DISK =
'L:\MSSQL\SQLBkps\Advworks_tlog1.trn'
WITH NORECOVERY, STATS = 5;
GO

-- Step 4: Transaction log 2
RESTORE LOG advworks
FROM DISK =
'L:\MSSQL\SQLBkps\Advworks_tlog2.trn'
WITH RECOVERY, STATS = 5;
GO
```

The differential backup must belong to the restored full backup's differential base. The log backups must continue the correct LSN sequence.

Reference: [Microsoft – Complete database restores](https://learn.microsoft.com/en-us/sql/relational-databases/backup-restore/complete-database-restores-full-recovery-model?view=sql-server-ver17\).

## **Lesson 6.12 – Database Restore with MOVE**

Use `MOVE` when restoring a database to different physical file locations.

First, identify logical file names:

```
RESTORE FILELISTONLY
FROM DISK =
'L:\MSSQL\SQLBkps\Advworks_full.bak';
GO
```

Then restore:

```
RESTORE DATABASE Samsungdb
FROM DISK =
'L:\MSSQL\SQLBkps\Advworks_full.bak'
WITH
    RECOVERY,
    MOVE 'AdventureWorks2019'
        TO 'E:\MSSQL\SQLDATA\Samsungdb.mdf',
    MOVE 'AdventureWorks2019_log'
        TO 'H:\MSSQL\SQLLOGS\Samsungdb_log.ldf',
    STATS = 5;
GO
```

Important:

- `AdventureWorks2019` is a logical file name.
- `Samsungdb.mdf` is a physical file name.
- Logical file names must match the backup metadata.
- The destination directories must exist.
- SQL Server service account must have appropriate file-system permissions.

## **Lesson 6.13 – Restore with REPLACE**

Scenario: Refresh an existing database with a production backup.

Example:

```
USE master;
GO

ALTER DATABASE advworks
SET SINGLE_USER WITH ROLLBACK IMMEDIATE;
GO

RESTORE DATABASE advworks
FROM DISK =
'L:\MSSQL\SQLBkps\Advworks_full.bak'
WITH
    REPLACE,
    RECOVERY,
    STATS = 5;
GO

ALTER DATABASE advworks
SET MULTI_USER;
GO
```

Production caution

`WITH REPLACE` can overwrite an existing database. Always validate the target database, source backup, destination files, approvals and recovery requirements before executing it.

Also, use `SINGLE_USER` only when required; it is not mandatory for every restore.

## **Lesson 6.14 – Point-in-Time Recovery (PITR)**

Scenario:

A user accidentally deletes important employee records at 9:16 PM.

The business wants the database restored to 9:15 PM.

Requirements:

- Full recovery model.
- Valid full backup.
- Applicable differential backup, if used.
- Complete transaction log backup chain.
- Required log backups available.

Example:

```
RESTORE LOG advworks
FROM DISK =
'L:\MSSQL\SQLBkps\Advworks_tlog.trn'
WITH
    STOPAT = '2026-11-11T21:15:00',
    RECOVERY,
    STATS = 10;
GO
```

This assumes the preceding full, differential and earlier transaction log backups have already been restored correctly using NORECOVERY.

For a multi-log restore, apply `STOPAT` to the appropriate final log restore operation and maintain the correct sequence.

### **Important BULK_LOGGED limitation**

If the log backup contains bulk-logged changes, SQL Server cannot restore to an arbitrary point within that backup.

Reference: [Microsoft – Point-in-time restore and recovery](https://learn.microsoft.com/en-us/sql/relational-databases/backup-restore/complete-database-restores-full-recovery-model).

## **Lesson 6.15 – LSN Fundamentals**

LSN means Log Sequence Number.

LSNs identify the position of log records in the transaction log.

Important terms:

- FirstLSN.
- LastLSN.
- CheckpointLSN.
- DatabaseBackupLSN.
- DifferentialBaseLSN.

LSN information is useful for:

- Validating backup chains.
- Identifying backup dependencies.
- Troubleshooting restore failures.
- Understanding transaction log continuity.

Do not assume that backup filenames or timestamps alone prove that a restore chain is valid.

## **Lesson 6.16 – Real-Time Database Refresh**

Scenario:

```
Source Server: Delhi
Database: XYZ

Source Data:
E:\XYZ.mdf

Source Log:
H:\XYZ_log.ldf
```

Destination:

```
Server: Chennai

Destination Data:
P:\SQLDATA\

Destination Log:
Q:\SQLLOGS\
```

### **Database refresh procedure**

1. Receive approved refresh request.
2. Confirm source and destination database names.
3. Confirm the source backup is valid.
4. Identify database logical file names.
5. Transfer the backup securely.
6. Validate destination disk capacity.
7. Obtain application-team confirmation.
8. Restore the database using MOVE.
9. Validate database status.
10. Review database users and logins.
11. Fix orphaned users if necessary.
12. Validate application connectivity.
13. Inform stakeholders.

Example:

```
RESTORE DATABASE America
FROM DISK =
'L:\MSSQL\SQLBkps\Advworks_full.bak'
WITH
    RECOVERY,
    MOVE 'AdventureWorks2019'
        TO 'P:\SQLDATA\America.mdf',
    MOVE 'AdventureWorks2019_log'
        TO 'Q:\SQLLOGS\America_log.ldf',
    STATS = 5;
GO
```

### **Post-refresh validation**

```
SELECT
    name,
    state_desc,
    recovery_model_desc
FROM sys.databases
WHERE name = 'America';
GO

USE America;
GO

DBCC CHECKDB;
GO
```

Also validate application objects, data, users, permissions and connectivity.

# **Module 7 – SQL Server Security**

Security is one of the most important responsibilities of a SQL Server DBA.

## **Lesson 7.1 – SQL Server Security Fundamentals**

SQL Server security controls:

1. Who can connect to SQL Server?
2. Who can access a particular database?
3. Who can read database objects?
4. Who can insert, update or delete records?
5. Who can execute stored procedures?
6. Who can administer the SQL Server instance?

### **Security hierarchy**

**Windows / Active Directory Identity**

**SQL Server Login**

Server-level authentication

**Database User**

Database-level identity

**Database Roles / Permissions**

**Tables | Views | Stored Procedures | Functions**

## **Lesson 7.2 – Authentication vs Authorization**

### **Authentication**

Authentication verifies the identity of the person or application connecting to SQL Server.

Examples:

- Windows Authentication.
- SQL Server Authentication.

### **Authorization**

Authorization determines what the authenticated identity can do.

Examples:

- SELECT.
- INSERT.
- UPDATE.
- DELETE.
- EXECUTE.
- ALTER.
- CONTROL.

**Interview point:** Authentication answers who you are. Authorization answers what you are allowed to do.

## **Lesson 7.3 – Windows Login Creation**

Your training scenario:

```
SQL Server: HYDERABAD

DBA Team:
1. Mohd
2. Deepak
3. Praveen
4. Jyothi
```

Instead of creating individual SQL Server logins for every DBA, the organization creates an Active Directory security group.

Example:

```
HYDERABAD\SQLDBATeam

Members:
    Deepak
    Jyothi
    Mohd
    Praveen
```

### **SQL Server login creation**

```
USE master;
GO

CREATE LOGIN [HYDERABAD\SQLDBATeam]
FROM WINDOWS;
GO
```

Advantages:

1. Centralized access management.
2. Easier employee onboarding.
3. Easier employee offboarding.
4. Reduced individual permission administration.
5. Consistent access management.
6. Better auditability.

## **Lesson 7.4 – Windows Groups and SQL Server Access**

Recommended workflow:

**Windows Team**

1. Creates a Windows account.
2. Creates or identifies the AD security group.
3. Adds employees to the group.

**SQL DBA Team**

1. Creates a SQL Server login for the AD group.
2. Creates a database user for the group login.
3. Adds the database user to appropriate database roles.
4. Grants required permissions.
5. Validates access.

Example:

```
USE master;
GO

CREATE LOGIN [HYDERABAD\SQLDBATeam]
FROM WINDOWS;
GO
```

Create database user:

```
USE iphone18;
GO

CREATE USER [HYDERABAD\SQLDBATeam]
FOR LOGIN [HYDERABAD\SQLDBATeam];
GO
```

Grant read access:

```
ALTER ROLE db_datareader
ADD MEMBER [HYDERABAD\SQLDBATeam];
GO
```

Grant write access only if required:

```
ALTER ROLE db_datawriter
ADD MEMBER [HYDERABAD\SQLDBATeam];
GO
```

Do not grant `sysadmin` or `db_owner` to the entire team simply because they are DBAs. Design access according to actual job responsibilities.

## **Lesson 7.5 – SQL Server Login Creation**

SQL Server Authentication example:

```
USE master;
GO

CREATE LOGIN AppLogin
WITH PASSWORD = '<STRONG_UNIQUE_SECRET>';
GO
```

Create corresponding database user:

```
USE iphone18;
GO

CREATE USER AppUser
FOR LOGIN AppLogin;
GO
```

Grant read permission:

```
GRANT SELECT
ON dbo.employee
TO AppUser;
GO
```

Grant stored procedure execution:

```
GRANT EXECUTE
ON dbo.usp_GetEmployee
TO AppUser;
GO
```

Security recommendation:

- Use strong unique credentials.
- Never store passwords in scripts or shared notes.
- Avoid hardcoding credentials in application code.
- Use managed secrets where appropriate.
- Prefer Windows Authentication where feasible.

## **Lesson 7.6 – Server-Level Roles**

SQL Server provides fixed server roles.

| Server role   | Purpose                                      |
| ------------- | -------------------------------------------- |
| sysadmin      | Full instance administration                 |
| serveradmin   | Server-wide configuration                    |
| securityadmin | Login and security management                |
| processadmin  | Manage SQL Server processes                  |
| setupadmin    | Manage linked servers and startup procedures |
| bulkadmin     | Perform bulk insert operations               |
| diskadmin     | Manage disk files                            |
| dbcreator     | Create, alter, drop and restore databases    |
| public        | Default role for all logins                  |

### **Important**

`sysadmin` members have unrestricted control over the SQL Server instance.

Avoid granting `sysadmin` unnecessarily.

SQL Server 2022 introduced additional fixed server roles designed for more granular permissions.

Reference: [Microsoft – Server-level roles](https://learn.microsoft.com/en-us/sql/relational-databases/security/authentication-access/server-level-roles?view=sql-server-ver17\).

## **Lesson 7.7 – Database-Level Roles**

Important fixed database roles:

| Database role     | Purpose                                         |
| ----------------- | ----------------------------------------------- |
| db_owner          | Full database administration                    |
| db_datareader     | Read all user tables and views                  |
| db_datawriter     | Insert, update and delete data in user tables   |
| db_ddladmin       | Execute permitted DDL operations                |
| db_securityadmin  | Manage database permissions and role membership |
| db_accessadmin    | Manage database access                          |
| db_backupoperator | Perform database backups                        |
| db_denydatareader | Deny data reading                               |
| db_denydatawriter | Deny data modifications                         |
| public            | Default role for all database users             |

### **Example**

```
USE iphone18;
GO

ALTER ROLE db_datareader
ADD MEMBER AppUser;
GO
```

Remove membership:

```
ALTER ROLE db_datareader
DROP MEMBER AppUser;
GO
```

**Important:** Fixed database roles provide predefined permissions. Use custom database roles when a business requirement needs more precise access.

## **Lesson 7.8 – Object-Level Permissions**

Object-level permissions allow access to specific database objects.

Example:

```
USE iphone18;
GO

GRANT SELECT
ON dbo.employee
TO AppUser;
GO
```

Grant INSERT:

```
GRANT INSERT
ON dbo.employee
TO AppUser;
GO
```

Grant UPDATE:

```
GRANT UPDATE
ON dbo.employee
TO AppUser;
GO
```

Grant DELETE:

```
GRANT DELETE
ON dbo.employee
TO AppUser;
GO
```

Revoke permission:

```
REVOKE DELETE
ON dbo.employee
FROM AppUser;
GO
```

Deny permission:

```
DENY DELETE
ON dbo.employee
TO AppUser;
GO
```

### **Difference between GRANT, DENY and REVOKE**

| Command | Meaning                           |
| ------- | --------------------------------- |
| GRANT   | Gives permission                  |
| DENY    | Explicitly denies permission      |
| REVOKE  | Removes an explicit grant or deny |

Remember: DENY generally overrides GRANT, subject to SQL Server's permission and ownership rules.

## **Lesson 7.9 – SID Fundamentals**

SID means Security Identifier.

Every Windows or SQL Server security principal has a SID.

Important catalog views:

```
SELECT
    name,
    type_desc,
    sid
FROM sys.server_principals;
```

Database users:

```
USE iphone18;
GO

SELECT
    name,
    type_desc,
    sid
FROM sys.database_principals;
```

### **Login SID vs Database User SID**

A database user mapped to a SQL Server login has a SID that corresponds to the login's SID.

This mapping allows SQL Server to associate the database user with the server login.

### **Real-time scenario**

Production database is restored to a development SQL Server.

The database user exists, but its corresponding login is missing or has a different SID.

Result:

The database user may become orphaned.

## **Lesson 7.10 – Orphaned Users**

An orphaned user occurs when a database user that depends on a server login has no matching login SID on the destination SQL Server instance.

### **Identify potential orphaned users**

Run inside the affected database:

```
SELECT
    dp.name AS UserName,
    dp.type_desc,
    dp.sid
FROM sys.database_principals dp
LEFT JOIN sys.server_principals sp
    ON dp.sid = sp.sid
WHERE sp.sid IS NULL
AND dp.authentication_type_desc = 'INSTANCE';
```

Review the results before taking corrective action. 
Not every unmatched database principal is an error; contained users and other special principals need separate treatment.

### **Fix orphaned users**

If the appropriate login already exists:

```
USE iphone18;
GO

ALTER USER AppUser
WITH LOGIN = AppLogin;
GO
```

This remaps the database user to the intended login.

**Important:** `sp_change_users_login` is deprecated. Use `ALTER USER ... WITH LOGIN` for supported login-based user remapping.

Reference: [Microsoft – Troubleshoot orphaned users](https://learn.microsoft.com/en-us/sql/sql-server/failover-clusters/troubleshoot-orphaned-users-sql-server?view=sql-server-ver17\).

## **Lesson 7.11 – Windows Group Membership Validation**

Your original example:

```
EXEC xp_logininfo
    'HYDERABAD\SQLDBATeam',
    'members';
GO
```

This can display Windows group membership information recognized by SQL Server.

Expected conceptual output:

| Account | Type | Permission path |
| ------- | ---- | --------------- |
| Deepak  | User | SQLDBATeam      |
| Jyothi  | User | SQLDBATeam      |
| Mohd    | User | SQLDBATeam      |
| Praveen | User | SQLDBATeam      |

Actual output depends on Windows domain configuration and SQL Server permissions.

## **Lesson 7.12 – SQL Server Security Audit**

A DBA should periodically review:

1. Existing SQL Server logins.
2. Disabled logins.
3. Windows group logins.
4. SQL authentication logins.
5. Server role membership.
6. Database role membership.
7. Object-level permissions.
8. Orphaned users.
9. Unused accounts.
10. Excessive privileges.
11. Service account permissions.
12. Authentication failures.

### **Query: SQL Server logins**

```
SELECT
    name,
    type_desc,
    is_disabled,
    create_date,
    modify_date
FROM sys.server_principals
WHERE type IN ('S', 'U', 'G')
ORDER BY name;
```

### **Query: Server role membership**

```
SELECT
    RoleName = r.name,
    MemberName = m.name
FROM sys.server_role_members rm
JOIN sys.server_principals r
    ON rm.role_principal_id = r.principal_id
JOIN sys.server_principals m
    ON rm.member_principal_id = m.principal_id
ORDER BY r.name, m.name;
```

### **Query: Database role membership**

```
USE iphone18;
GO

SELECT
    RoleName = r.name,
    MemberName = m.name
FROM sys.database_role_members drm
JOIN sys.database_principals r
    ON drm.role_principal_id = r.principal_id
JOIN sys.database_principals m
    ON drm.member_principal_id = m.principal_id
ORDER BY r.name, m.name;
```

## **Lesson 7.13 – SQL Server Security Best Practices**

Follow these principles:

1. Implement least-privilege access.
2. Prefer Windows Authentication.
3. Use AD security groups for centralized administration.
4. Avoid shared DBA accounts.
5. Avoid unnecessary `sysadmin` membership.
6. Avoid granting `db_owner` to application users.
7. Review login and user permissions regularly.
8. Disable unused accounts following approval.
9. Audit privileged access.
10. Protect credentials and service accounts.
11. Separate application and DBA access.
12. Maintain access-request and approval records.
13. Review access after employee transfers and exits.
14. Use encrypted connections where required.
15. Protect backup files because they contain sensitive database information.

Microsoft reference: 
[Get started with Database Engine permissions](https://learn.microsoft.com/en-us/sql/relational-databases/security/authentication-access/getting-started-with-database-engine-permissions?view=sql-server-ver17\).


# **Recommended Practical Assignments**

Use these exercises to make the lessons practical rather than purely theoretical.

**Student practical checklist**

0/27 completed

**1. SQL Server Fundamentals**

Identify SQL Server version, build and edition.

Identify default and named instances.

Explain SQL Server authentication modes.

**2. Installation and Configuration**

Install SQL Server 2019 or 2022 Developer Edition.

Install SSMS separately.

Configure TCP/IP and a static port.

Configure service accounts and Max Server Memory.

Complete post-installation validation.

**3. Database Administration**

Create iphone18 database.

Create employee table.

Configure data and log file locations.

Change database recovery models.

Inspect database properties and system views.

**4. Backup and Recovery**

Take a compressed full backup.

Take a differential backup.

Configure FULL recovery and transaction log backups.

Restore full, differential and transaction log backups.

Perform a point-in-time restore in a lab.

Restore a database under a different name using MOVE.

**5. SQL Server Security**

Create Windows login.

Create Windows group login.

Create SQL Server login.

Create database users.

Configure server and database roles.

Grant object-level permissions.

Identify orphaned users.

Fix orphaned users using ALTER USER.

Reset checklist

# **Recommended Microsoft Documentation**

These are the official references students should use alongside the training.

1. [SQL Server Installation Guide](https://learn.microsoft.com/en-us/sql/database-engine/install-windows/install-sql-server?view=sql-server-ver16\)
2. [SQL Server 2025 Hardware and Software Requirements](https://learn.microsoft.com/en-us/sql/sql-server/install/hardware-and-software-requirements-for-installing-sql-server-2025?view=sql-server-linux-2017\)
3. [SQL Server Recovery Models](https://learn.microsoft.com/en-us/sql/relational-databases/backup-restore/recovery-models-sql-server?view=sql-server-ver17\)
4. [SQL Server Backup and Restore](https://learn.microsoft.com/en-us/sql/relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases/)
5. [Complete Database Restores](https://learn.microsoft.com/en-us/sql/relational-databases/backup-restore/complete-database-restores-full-recovery-model?view=sql-server-ver17\)
6. [SQL Server Database Engine Permissions](https://learn.microsoft.com/en-us/sql/relational-databases/security/authentication-access/getting-started-with-database-engine-permissions?view=sql-server-ver17\)
7. [SQL Server Server-Level Roles](https://learn.microsoft.com/en-us/sql/relational-databases/security/authentication-access/server-level-roles?view=sql-server-ver17\)
8. [Troubleshoot Orphaned Users](https://learn.microsoft.com/en-us/sql/sql-server/failover-clusters/troubleshoot-orphaned-users-sql-server?view=sql-server-ver17\)


Next curriculum section:
SQL Server Agent, DBA automation, maintenance jobs, monitoring, performance tuning, patching, migration and HA/DR.
