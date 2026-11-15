# IT312 Lecture 6: Amazon RDS and Managed Databases — Study Notes (Version 2)

> **Version 2 (2026-11-02):** the encryption at rest content was revised to separate the slide's exact wording from Agent Explanation (see 4.3). Version 1 is kept unchanged as *IT312 2026-10-19 Amazon RDS Study Notes.md*.

> **Citation key:** `[S N]` = slide N of **`01 Courses/IT312/IT312 2026-10-19 Amazon RDS Lecture Slides.pptx`** (12 slides). `[EC2 p. N]` = page N of **`01 Courses/IT312/IT312 2026-10-12 Amazon EC2 Lecture.pdf`**.
> Text marked **Agent Explanation** is added by the study agent to help you understand. It is **not** stated in the lecture, so don't quote it as lecture content.

---

## 1. Lesson identification

| Field | Value |
|---|---|
| Course | IT 312 – Cloud Computing [S 1] |
| Lesson | Lecture 6: Amazon RDS and Managed Databases [S 1] |
| Date | October 19, 2026 [S 1] |
| Source file | IT312 2026-10-19 Amazon RDS Lecture Slides.pptx |
| Prerequisites (stated) | Lecture 5: Amazon EC2 (instances, security groups); basic SQL and relational database concepts [S 3] |

## 2. Learning objectives
These are copied from the lecture [S 2]:
1. Explain what a managed database service is.
2. List the responsibilities of AWS vs. the customer in RDS.
3. Distinguish Multi-AZ deployments from read replicas.
4. Describe RDS backup and restore options.

## 3. Key concepts and definitions

### 3.1 Amazon RDS [S 4]
- **RDS (Relational Database Service):** a managed relational database in the cloud.
- **Supported engines:** MySQL, PostgreSQL, MariaDB, Oracle, Microsoft SQL Server, Amazon Aurora.
- You connect using an **endpoint** (hostname) and **port**, e.g., **3306 for MySQL**.
- You **cannot SSH** into the underlying database host.

> **Agent Explanation:** A "managed" service means AWS runs the server and database software for you. You use the database, but you don't administer the machine it runs on. That's why there is no SSH access.

### 3.2 Shared responsibility in RDS [S 5]

| AWS manages | Customer manages |
|---|---|
| Hardware | Database schema |
| OS patching | Queries |
| Database software installation | Users and passwords |
| Automated backups | Security group rules |
| | Choosing instance size |

- Compare: **on EC2 you would install and patch MySQL yourself** [S 5].

### 3.3 Multi-AZ deployments [S 6]
- A **standby copy** is kept in a **different Availability Zone**.
- Data is replicated **synchronously**.
- **Automatic failover** if the primary fails, and **the endpoint stays the same**.
- Purpose: **HIGH AVAILABILITY, not performance**.
- The standby **does not serve read traffic**.

### 3.4 Read replicas [S 7]
- Copies of the database that serve **read-only queries**.
- Replication is **asynchronous**, so replicas **may lag slightly behind**.
- Purpose: **SCALING READS** for read-heavy applications.
- A replica can be **promoted to a standalone database**.

### 3.5 Multi-AZ vs. read replicas at a glance [S 6, S 7, S 12]

| | Multi-AZ | Read replica |
|---|---|---|
| Purpose | High availability | Scaling reads |
| Replication | Synchronous | Asynchronous (may lag) |
| Serves reads? | No (standby) | Yes, read-only |
| On primary failure | Automatic failover, same endpoint | Not stated in the lecture; can be promoted to standalone |

### 3.6 Backups and restore [S 8]
- **Automated backups:** retention of **1 to 35 days**; enables **point-in-time recovery**.
- Setting retention to **0 disables** automated backups.
- **Manual snapshots:** kept **until you delete them**.
- **Restoring always creates a NEW DB instance with a new endpoint.**

## 4. Processes or procedures

### 4.1 Securing an RDS database (best practices) [S 9]
1. Place the database in **private subnets**; do **not** make it publicly accessible.
2. Allow **port 3306 only from the application server's security group**.
3. **Enable encryption at rest when creating the instance.** *(Slide wording. See 4.3 for what the slides do and don't say.)*
4. **Store credentials securely**, not in public code repositories.

### 4.2 Restoring from a backup [S 8]
1. Choose a point in time (within the automated backup retention window) or a manual snapshot.
2. The restore creates a **new DB instance with a new endpoint**. It does not overwrite the existing database [S 8, S 11].

> **Agent Explanation:** After a restore, your application still points to the *old* endpoint. You have to update the application's database host setting (for example `DB_HOST` in WordPress, see 5.1) to the new endpoint. The lecture states the new endpoint [S 8] and the `DB_HOST` setting [S 10] separately, and this step combines the two.

### 4.3 Encryption at rest: source statement vs. Agent Explanation

**Source statement (slide 9, exact wording):**
**"Enable encryption at rest when creating the instance"** [S 9]

- This is the **only** mention of encryption in the 12 slides. It appears in the list of security best practices [S 9].
- It is an **instruction about timing**: turn encryption at rest on **at creation**.

**What the slides do NOT state:**
- Whether encryption at rest can be turned on **after** an instance has been created.
- How encryption at rest works, or which keys it uses.
- Anything about encryption **in transit** (between the application and the database).

> **Agent Explanation (not from the slides; verify with your instructor or the AWS RDS documentation):** In AWS, you can't simply switch on encryption for an existing unencrypted RDS instance. The usual workaround is to take a snapshot, make an encrypted copy of that snapshot, and restore a new instance from the copy. That links to slide 8: restoring always creates a **new DB instance with a new endpoint**, so the application's connection setting would have to change. This is why the slide's advice to decide at creation matters in practice.

## 5. Examples from the lecture

### 5.1 WordPress on EC2 + RDS [S 10]
- **EC2 instance** runs Apache, PHP, and WordPress.
- **RDS MySQL instance** stores posts, users, and settings.
- **`wp-config.php`** points `DB_HOST` to the **RDS endpoint**.
- **RDS security group** allows inbound **3306 only from the EC2 security group**.

> **Agent Explanation:** This is a two-tier design. The web tier (EC2) is public, and the data tier (RDS) is only reachable from the web tier. It uses the EC2 lecture's rule that "rules can reference another security group instead of an IP range" [EC2 p. 4].

## 6. Agent explanations (summary)
Everything in this section is **Agent Explanation**, not lecture content:
- **Synchronous vs. asynchronous:** synchronous means a write is copied to the standby before it counts as complete, so the standby is always up to date. Asynchronous means replicas copy changes afterwards, which is why they can lag [S 7].
- **Why Multi-AZ doesn't speed up reads:** the standby only waits to take over and never answers queries [S 6]. Read capacity stays the same.
- **Encryption at rest after creation:** see 4.3. The AWS behavior described there is Agent Explanation, not slide content.
- **Snapshot vs. automated backup:** automated backups expire after the retention period (1–35 days). Manual snapshots stay until you delete them [S 8], so use them for long-term copies.

## 7. Connections to earlier and related lessons
Checked against the files in your workspace on 2026-11-02:
- **Prerequisite (stated):** **IT312 Lecture 5, Amazon EC2** (*IT312 2026-10-12 Amazon EC2 Lecture.pdf*). Slide 3 lists it. Specific links:
  - **Responsibility model:** on EC2 (IaaS) the customer manages the OS, software, and data [EC2 p. 2]. In RDS, AWS handles OS patching and database software [S 5]. Slide 5 states the comparison directly.
  - **Security groups referencing security groups:** EC2 lecture [EC2 p. 4]. Applied here as "allow 3306 only from the application server's security group" [S 9, S 10].
  - **Limiting open access:** the EC2 lecture warns against opening SSH to 0.0.0.0/0 [EC2 p. 4]. RDS says not to make the database publicly accessible [S 9].
- **Prerequisite (stated, not in workspace):** basic SQL and relational database concepts [S 3]. No lesson file covers this yet.
- **Follow-up (stated, not in workspace):** **Amazon S3 and object storage**, the next lecture [S 12].
- **Related concept (Agent Explanation):** **IT305 Access Control Fundamentals**, *least privilege* (*IT305 2026-10-14 Access Control Fundamentals Handout.docx*, section 4). Allowing 3306 only from the app server's security group gives the database the minimum access it needs. Neither source names this link; the agent added it.

## 8. Common misconceptions and cautions
**Stated in the lecture [S 11]:**
- ❌ "Multi-AZ makes my reads faster." **No**, use read replicas for that.
- ❌ "Restoring a snapshot overwrites my database." **No**, it creates a new instance.
- ❌ "Managed means AWS designs my tables." **No**, schema design is your job.

**Other cautions stated in the slides:**
- ⚠️ Setting backup retention to **0 disables** automated backups [S 8].
- ⚠️ Read replicas **may lag slightly behind** [S 7].
- ⚠️ Don't store credentials in **public code repositories** [S 9].
- ⚠️ "Enable encryption at rest when creating the instance" [S 9]. The slides don't say whether it can be enabled later; see 4.3.

## 9. Quick review summary
- RDS = managed relational DB. Six engines, connect by endpoint and port (MySQL 3306), no SSH [S 4].
- AWS: hardware, OS patching, DB software, automated backups. You: schema, queries, users/passwords, security group rules, instance size [S 5].
- **Multi-AZ = availability** (synchronous standby in another AZ, automatic failover, same endpoint, no reads). **Read replicas = read scaling** (asynchronous, read-only, may lag, can be promoted) [S 6, S 7, S 12].
- Backups: automated 1–35 days with point-in-time recovery (0 disables them), manual snapshots kept until deleted, and **restore always makes a new instance and endpoint** [S 8].
- Security: private subnets, 3306 only from the app SG, enable encryption at rest when creating the instance, credentials kept out of public repos [S 9].

## 10. Source list
1. **IT312 2026-10-19 Amazon RDS Lecture Slides.pptx**, `01 Courses/IT312/`, slides 1–12 (primary source; no speaker notes or images present).
2. **IT312 2026-10-12 Amazon EC2 Lecture.pdf**, `01 Courses/IT312/`, pages 2 and 4 (connection evidence).
3. **IT305 2026-10-14 Access Control Fundamentals Handout.docx**, `01 Courses/IT305/`, section 4 (connection evidence only).
