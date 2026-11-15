# IT312 Lecture 6: Amazon RDS — Review Questions

**Source:** IT312 2026-10-19 Amazon RDS Lecture Slides.pptx
**Format:** 10 questions (4 recall, 3 application, 3 analysis)
**How to use:** write your answers before opening *IT312 2026-10-19 Amazon RDS Answer Key.md* in `05 Study Materials/Answer Keys`.

---

## Part A: Recall

**1.** List the six database engines that the lecture says RDS supports. How do you connect to an RDS database, and what can you *not* do with the underlying host?

**2.** In RDS, which responsibilities does AWS manage, and which does the customer manage?

**3.** What is the retention range for RDS automated backups, what feature do they enable, and what happens if retention is set to 0?

**4.** For a Multi-AZ deployment, state (a) where the standby is kept, (b) whether replication is synchronous or asynchronous, (c) its purpose, and (d) whether the standby serves read traffic.

---

## Part B: Application

**5.** Your web app's reporting page runs many heavy `SELECT` queries, and the main database is slowing down. Writes are light. Which RDS feature from the lecture should you add, and what side effect must the reports tolerate?

**6.** You are deploying WordPress on EC2 with an RDS MySQL database, as in the lecture example. Describe (a) the inbound rule for the RDS security group, (b) where the database should be placed, and (c) what `wp-config.php` must contain.

**7.** A teammate accidentally deleted important rows yesterday. Automated backups are on with 7-day retention. Can you recover the data? Describe what the restore creates, and what you must then change in the WordPress setup so the site uses the restored data.

---

## Part C: Analysis

**8.** Your instructor requires the database to "keep working with the same connection address even if its Availability Zone fails." A classmate suggests a read replica. Compare Multi-AZ and read replicas on replication type, purpose, read traffic, and failure behavior, then decide which one meets the requirement.

**9.** A classmate argues: "Let's run MySQL on EC2 instead of RDS, because with RDS AWS designs our tables and we lose control of the database." Analyze this claim. What does RDS actually take over, what stays your job, and what control do you really give up?

**10.** You review a group project and find: (1) the RDS database is set to publicly accessible, (2) its security group allows 3306 from 0.0.0.0/0, and (3) the database password is committed to a public GitHub repository. Analyze each problem using the lecture's best practices, and state the fix for each.
