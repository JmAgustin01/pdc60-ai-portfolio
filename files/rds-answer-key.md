# IT312 Lecture 6: Amazon RDS — Answer Key

**Source:** `01 Courses/IT312/IT312 2026-10-19 Amazon RDS Lecture Slides.pptx` (cited as [S N]). `[EC2 p. N]` = `IT312 2026-10-12 Amazon EC2 Lecture.pdf`.
**Verification:** every answer was checked against the slide cited. Text marked **Agent Explanation** goes beyond the lecture and is not needed for full credit.

---

## Part A: Recall

**1. Engines and access** [S 4]
- **Engines:** MySQL, PostgreSQL, MariaDB, Oracle, Microsoft SQL Server, Amazon Aurora.
- **Connect** using an **endpoint (hostname) and port**, e.g., 3306 for MySQL.
- You **cannot SSH** into the underlying database host.
*Verified: slide 4.*

**2. Shared responsibility** [S 5]
- **AWS:** hardware, OS patching, database software installation, automated backups.
- **Customer:** database schema, queries, users and passwords, security group rules, choosing instance size.
*Verified: slide 5.*

**3. Automated backups** [S 8]
Retention of **1 to 35 days**; they enable **point-in-time recovery**. Setting retention to **0 disables** automated backups.
*Verified: slide 8, bullets 1–2.*

**4. Multi-AZ** [S 6]
(a) In a **different Availability Zone**. (b) **Synchronous**. (c) **High availability, not performance**. (d) **No**, the standby does not serve read traffic.
*Verified: slide 6.*

---

## Part B: Application

**5. Heavy reporting reads → read replicas** [S 7, S 11]
Add **read replicas** and send the report queries to them. They serve read-only queries and exist for **scaling reads** in read-heavy applications. Side effect: replication is **asynchronous**, so replicas **may lag slightly behind**, and reports may be a little out of date.
*Verified: slide 7; slide 11 ("use read replicas for that").*

**6. WordPress on EC2 + RDS** [S 9, S 10]
(a) RDS security group: allow inbound **3306 only from the EC2 (application server) security group**.
(b) Place the database in **private subnets** and do **not** make it publicly accessible.
(c) `wp-config.php` points **`DB_HOST` to the RDS endpoint**.
*Verified: slide 10 bullets 3–4; slide 9 bullets 1–2.*

**7. Recovering deleted rows** [S 8, S 10]
**Yes.** Yesterday is within the 7-day retention window, and automated backups enable **point-in-time recovery**. The restore **always creates a NEW DB instance with a new endpoint**; it does not overwrite the existing database [S 8, S 11].
To make the site use the restored data, update **`DB_HOST` in `wp-config.php`** to the **new endpoint**.
*Verified: slide 8 (retention, point-in-time recovery, new instance and endpoint); slide 10 (DB_HOST points to the RDS endpoint).*
> **Agent Explanation:** The "update DB_HOST" step combines slide 8 and slide 10; neither slide states it as one instruction. The lecture also doesn't cover copying only the deleted rows back, so treat that as outside this lesson.

---

## Part C: Analysis

**8. Multi-AZ vs. read replica for the requirement** [S 6, S 7, S 11, S 12]

| | Multi-AZ | Read replica |
|---|---|---|
| Replication | Synchronous | Asynchronous (may lag) |
| Purpose | High availability | Scaling reads |
| Read traffic | Standby serves none | Serves read-only queries |
| Failure behavior | **Automatic failover; endpoint stays the same** | Can be **promoted** to a standalone database |

**Decision: Multi-AZ.** The requirement asks for survival of an AZ failure *with the same connection address*. Slide 6 says Multi-AZ fails over automatically and the endpoint stays the same. The read replica is the wrong tool: its stated purpose is scaling reads, and the lecture doesn't describe it as automatic failover.
*Verified: slides 6, 7, 12 ("Multi-AZ = availability; read replicas = read scaling").*
> **Agent Explanation:** A promoted replica is a separate standalone database, so it would normally have its own endpoint. The lecture doesn't state that, so it isn't required for credit.

**9. "RDS means AWS designs our tables"** [S 5, S 11, S 4; EC2 p. 2]
- **The claim is a misconception** that the lecture names directly: "Managed means AWS designs my tables" is wrong, and **schema design is your job** [S 11].
- **RDS takes over:** hardware, OS patching, database software installation, automated backups [S 5].
- **Still your job:** schema, queries, users and passwords, security group rules, choosing instance size [S 5].
- **What you really give up:** you **cannot SSH** into the database host [S 4]. On EC2 you would **install and patch MySQL yourself** [S 5], because the customer manages the OS and software [EC2 p. 2].
- **Conclusion:** you keep control of the database's design and access. What you hand to AWS is server and database-software maintenance [S 12].
*Verified: slides 4, 5, 11, 12; EC2 lecture p. 2.*

**10. Three security problems** [S 9]

| Problem | Why it's wrong (lecture) | Fix |
|---|---|---|
| Publicly accessible | Best practice is to place the DB in **private subnets** and not make it publicly accessible | Turn off public access and use private subnets |
| 3306 open to 0.0.0.0/0 | Lecture: allow **3306 only from the application server's security group** | Replace the rule's source with the app server's security group |
| Password in public GitHub | Lecture: **store credentials securely, not in public code repositories** | Remove the password from the repo and store it securely |

*Verified: slide 9, bullets 1, 2, 4. The 0.0.0.0/0 danger parallels the EC2 caution on p. 4.*
> **Agent Explanation:** A password that has been pushed to a public repo should be treated as leaked and **changed**, because deleting the file doesn't erase it from git history. The lecture doesn't state this.

---

### Verification summary
| Q | Type | Source location | Status |
|---|---|---|---|
| 1 | Recall | S 4 | Verified |
| 2 | Recall | S 5 | Verified |
| 3 | Recall | S 8 | Verified |
| 4 | Recall | S 6 | Verified |
| 5 | Application | S 7, S 11 | Verified |
| 6 | Application | S 9, S 10 | Verified |
| 7 | Application | S 8, S 10, S 11 | Verified (the combined DB_HOST step is noted as Agent Explanation) |
| 8 | Analysis | S 6, S 7, S 12 | Verified |
| 9 | Analysis | S 4, S 5, S 11, S 12; EC2 p. 2 | Verified |
| 10 | Analysis | S 9; EC2 p. 4 | Verified (the password-rotation note is Agent Explanation) |

No item is marked SOURCE UNCERTAIN.
