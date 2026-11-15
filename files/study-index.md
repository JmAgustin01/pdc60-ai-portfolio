# Study Index

Lessons processed in STUDY mode, oldest first. Entries are added and never removed.
Connections are marked **[Source]** when a lecture file states them, and **[Agent]** when the study agent added them.

---

## IT312 — Cloud Computing

### Lecture 5: Amazon EC2 and Virtual Servers (2026-10-12)
- **Source file:** `01 Courses/IT312/IT312 2026-10-12 Amazon EC2 Lecture.pdf` (7 pages)
- **Notes:** `05 Study Materials/Notes/IT312 2026-10-12 Amazon EC2 Study Notes.md` (also `.docx`)
- **Review questions:** `05 Study Materials/Review Questions/IT312 2026-10-12 Amazon EC2 Review Questions.md`
- **Answer key:** `05 Study Materials/Answer Keys/IT312 2026-10-12 Amazon EC2 Answer Key.md`
- **Key concepts:** EC2, IaaS, AMI, instance type and families, burstable T instances, key pair, security group (stateful, allow-only), EBS, instance store, instance lifecycle (stop vs. terminate), Elastic IP, On-Demand / Savings Plans & Reserved Instances / Spot, user data
- **Prerequisites:** none stated in the lecture. No earlier lessons are in the workspace yet (IT312 Lectures 1–4 not added).
- **Follow-up topics:**
  - IT312 Lecture 6: Amazon RDS (2026-10-19) **[Source]**. RDS slide 3 lists "Lecture 5: Amazon EC2 (instances, security groups)" as a prerequisite. *Studied 2026-11-02.*
- **Related concepts:**
  - IT305 Access Control Fundamentals (2026-10-14): **least privilege**. Restricting SSH to your own IP is an application of it. **[Agent]** *Not yet studied.*

### Lecture 6: Amazon RDS and Managed Databases (2026-10-19)
- **Source file:** `01 Courses/IT312/IT312 2026-10-19 Amazon RDS Lecture Slides.pptx` (12 slides)
- **Notes:** `05 Study Materials/Notes/IT312 2026-10-19 Amazon RDS Study Notes.md` (also `.docx`)
- **Notes, Version 2 (2026-11-02):** `05 Study Materials/Notes/IT312 2026-10-19 Amazon RDS Study Notes v2.md` (also `.docx`). The encryption at rest section separates the slide wording from Agent Explanation.
- **Review questions:** `05 Study Materials/Review Questions/IT312 2026-10-19 Amazon RDS Review Questions.md`
- **Answer key:** `05 Study Materials/Answer Keys/IT312 2026-10-19 Amazon RDS Answer Key.md`
- **Key concepts:** managed database, RDS engines, endpoint and port, shared responsibility (AWS vs. customer), Multi-AZ (synchronous standby, automatic failover), read replicas (asynchronous, read scaling, promotion), automated backups (1–35 days, point-in-time recovery), manual snapshots, restore creates a new instance, RDS security best practices, WordPress on EC2 + RDS
- **Prerequisites:**
  - IT312 Lecture 5: Amazon EC2 (2026-10-12) **[Source]**. Slide 3 lists it. It links through the responsibility model (slide 5 vs. EC2 p. 2) and security groups referencing security groups (slides 9–10 vs. EC2 p. 4). *Studied 2026-11-02.*
  - Basic SQL and relational database concepts **[Source]**, slide 3. *No lesson file in the workspace.*
- **Follow-up topics:**
  - Amazon S3 and object storage, the next lecture **[Source]**, slide 12. *No lesson file in the workspace.*
- **Related concepts:**
  - IT305 Access Control Fundamentals (2026-10-14): **least privilege**. Allowing 3306 only from the app server's security group applies it. **[Agent]** *Not yet studied.*
