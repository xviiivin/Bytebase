# 🌱 Bytebase

<h1 align="center">
  <a href="https://www.bytebase.com?source=github" target="_blank">
    <img alt="Bytebase" src="https://miro.medium.com/v2/resize:fit:1400/format:webp/1*9t1xylD8wwLpdoCMEf5hEw.png" />
  </a>
</h1>

## 📚 What's Bytebase

Think of **.Bytebase**. as a **.GitHub + CI/CD**., but for **databases**.
It’s an open-source tool that helps you:
- Manage **.database schema migrations**. (versioning your DB changes like code).
- Review and approve SQL changes (with a workflow like code review).
- Apply changes consistently across **.multiple environments**. (dev → staging → prod).
- Keep an **.audit**. log (who changed what, when).
It supports many databases like **Postgres, MySQL, MariaDB, TiDB, MongoDB, Snowflake, etc.**

<h1 align="center">
  <a href="https://www.bytebase.com?source=github" target="_blank">
    <img alt="Bytebase" src="https://mintcdn.com/dbx/vw8BbfZhlW9y-cr_/content/docs/what-is-bytebase/middleware.webp?w=1100&fit=max&auto=format&n=vw8BbfZhlW9y-cr_&q=85&s=717e37a344a48e193e8a6d20ee7844e8" />
  </a>
</h1>

---

<h1 align="center">
  <a href="https://www.bytebase.com?source=github" target="_blank">
    <img alt="Bytebase" src="https://raw.githubusercontent.com/bytebase/bytebase/main/docs/assets/fish.webp" />
  </a>
</h1>

The image shows a “big fish eating small fish” meme with different database tools:

- The smallest creatures (🐙 + tiny fish): **raw SQL CLI** → developers manually writing SQL in the terminal.
- Small fish: **Navicat, DBeaver** → GUI database clients.
- Medium fish: **Liquibase, Flyway** → schema migration and automation tools.
- Larger fish: **DataGrip** (IDE for database management).
- Even larger fish: **Redgate** (commercial DB DevOps tool).
- The biggest fish: **Bytebase**, about to eat all the others.

Bytebase positions itself as the next evolution of database tools:
- It covers what GUI tools do (easy access to DBs).
- It covers what migration tools do (version-controlled schema changes).
- It adds what IDEs do (developer-friendly SQL editing).
- And on top of that, it introduces **workflow, approval, governance, and security** that none of the others combine in one platform.

## ⚡ Usecase
1) **Change History: Identify Who Changed What in Production** <br>
Scenario:
A production database issue occurs after a new schema deployment.
The team needs to know who made the change and when.

How ByteBase Helps:

- Open **Change History** to see who executed the latest migration.
- Review detailed logs containing the SQL executed, timestamp, and results (success/failure).
- Use this information to perform **Root Cause Analysis (RCA)** immediately.

Benefit:
Reduces debugging time and ensures transparent post-incident investigation. 

2) **SQL Review: Detect Non-Compliant SQL Before Production** <br>
Scenario:
A developer writes a migration script containing a ```DROP COLUMN``` command during staging.

How ByteBase Helps:

- The SQL Review engine flags this as a policy violation for production.
- The system automatically blocks the deployment and notifies via Slack.
- The developer corrects the issue before it reaches production.

Benefit:
Prevents data loss automatically and enforces organization-wide SQL standards.

3) **Rollback Plan: Revert Instantly When Deployment Fails** <br>

Scenario:
A team deploys a migration, and the application crashes because a new column lacks a default value.

How ByteBase Helps:

- Open the latest migration entry in the **ByteBase UI**.
- Click ***“Rollback”***, and the system executes the attached rollback script.
- The database safely returns to its previous state within seconds.

Benefit:
Minimizes downtime and gives teams confidence in rapid, safe deployments.

4) **Approval Flow: Enforce Multi-Stage Deployment Controls** <br>

Scenario:
A development team wants to deploy a migration to production, but DBA approval is required first.

How ByteBase Helps:

- Define an **Approval Flow** such as: Dev → QA → DBA (Production Approver).
- Every approval action is recorded with timestamps for full traceability.
- ByteBase sends **email/Slack/Discord notifications** at each stage (request, approval, deployment).

Benefit:
Ensures controlled, compliant deployment processes with a complete audit trail.


--- 

## 💫 Database Change Management Approval Flow (Risk-based Workflow)

<h1 align="center">
  <img width="1764" height="1000" alt="image (3)" src="https://github.com/user-attachments/assets/d84ee3b5-6819-47b0-acdf-c72d49b27a78" />
</h1>


### 📍 Risk Level in Bytebase
Bytebase does not allow every SQL script to be deployed directly.
Instead, it **evaluates the risk level** first and enforces who needs to approve.
This prevents **production incidents** such as dropping a table by mistake or updating too many
rows unintentionally.

1) **Low Risk** → Auto Approve<br>
<ul>
  <li>Examples:</li>
    <ul>
      <li>ALTER TABLE in a test environment</li>
    </ul>
  <li>Reason:</li>
      <ul>
      <li>Changes in a test environment don’t affect real users. If it fails, production is safe.</li>
    </ul>
  <li>Result 
    <ul>
      <li>Bytebase auto-approves, no human involvement needed.</li>
    </ul>
  </li>
</ul>


2) **Moderate Risk** → Requires Project Owner Approval<br>
<ul>
  <li>Examples:</li>
    <ul>
      <li>CREATE TABLE in a production environment</li>
    </ul>
  <li>Reason:</li>
      <ul>
      <li>Creating a new table doesn’t impact existing data directly but still modifies the
schema.</li>
    </ul>
  <li>Result 
    <ul>
      <li>Requires approval from the Project Owner (the team lead or owner of the service).</li>
    </ul>
  </li>
</ul>


3) **High Risk** → Requires Project Owner + DBA Approval<br>
<ul>
  <li>Examples:</li>
    <ul>
      <li>ALTER TABLE in a production environment</li>
      <li>UPDATE or DELETE affecting >100 rows in production</li>
    </ul>
  <li>Reason:</li>
      <ul>
      <li>Altering schema in production can affect queries, indexes, or dependent systems.</li>
      <li>Mass updates/deletes have a high chance of mistakes (e.g., deleting customer data
without a WHERE clause).</li>
    </ul>
  <li>Result Needs two levels of approval:
    <ul>
      <li>Project Owner → validates the business necessity</li>
      <li>DBA → validates technical safety (performance, locks, constraints, index impact)</li>
    </ul>
  </li>
</ul>


**Why is this needed?**
- **Reduce Human Error**: prevents developers from running unsafe SQL in production
- **Separation of Duty**: Dev can write SQL, but can’t deploy it directly → requires Owner/DBA
approval
- **Audit & Compliance**: Every change is logged (who approved, when, and what was
changed)
- **Balance Speed & Safety**: Low-risk changes are auto-approved; high-risk changes require
multi-level review


### ✏️ When a developer creates a PR (Pull Request):

<h1 align="center">
  <img width="1764" height="1000" src="https://github.com/user-attachments/assets/3ae9bd70-ff46-4845-bff2-1a19e794f2db" />
</h1>
  

1) **Dev creates PR** <br>
<ul>
  <li>PR contains a SQL migration script in the codebase</li>
  <li>Bytebase automatically runs:</li>
      <ul>
      <li>SQL Review (lint, best practice check)</li>
      <li>Migration Check (schema policy, dry-run, row impact estimation, etc.)</li>
    </ul>
</ul>


2) **Team Lead reviews and approves PR**
- If **SQL Review + Migration Check** pass → OK
- If not → Developer must fix the SQL before merging


3) **Bytebase auto-creates an Issue**
- The Issue tracks the migration process for that SQL script
- Contains metadata: script, environment, risk level, approvers, and check results


4) **Approval (depending on Risk Level)**
-  **Low Risk** → Auto-approved
- **Moderate** → Requires Project Owner
- **High** → Requires Project Owner + DBA
- Approvers review via Bytebase UI and can click **Approve or Roll out**


5) **Deploy Migration**
-  Bytebase executes the migration script on the actual database
- Logs execution results in real-time
- Updates the Issue status → **Done when successful**
  

6) **Re-run Migration Check (if needed)**
-  If errors occur → Dev updates the script → rerun the check and re-deploy


7) **Merge PR**
-  Once both **SQL Review** and **Migration** pass, Dev can merge the PR successfully


Why this workflow?
- **Separation of Duty**: Devs write SQL but can’t deploy alone → must go through Owner/DBA
- **Risk-based Approval**: Automates safe changes, while critical changes get multi-step
review
- **Auditability**: Every migration tracked in Bytebase Issue (who, when, what)
- **Automation**: Integrated with GitHub/GitLab → enables CI/CD for databases

## 🏀 Why use Bytebase?

Normally, DB migrations can get messy:
- Devs apply SQL directly in staging/prod.
- No central history.
- Risk of running unsafe queries (e.g. dropping tables in prod).

With Bytebase, you get:

✔️ Version-controlled migrations (every change is tracked).<br>
✔️ Approval workflow (like PRs in GitHub).<br>
✔️ Safe deployment (analyze SQL before running).<br>
✔️ Multi-environment sync (keep dev/stg/prod aligned). <br>


| Feature / Tool           | Bytebase                    | Liquibase | Flyway   | Atlas   | Dbmate/Sqitch | Redgate SCA              |
| ------------------------ | ---------------------------- | --------- | -------- | ------- | ------------- | ------------------------ |
| **UI / Web Dashboard**   |  Yes✔️                        | No✖️      | No✖️    | Partial  | No✖️          | Yes✔️  (SQL Server focus) |
| **Workflow Approval**    |  Yes✔️                        | No✖️      | No✖️     | No✖️     |No✖️           | Yes✔️                    |
| **Schema Migration**     |  Yes✔️                        | Yes✔️     | Yes✔️   | Yes✔️     | Yes✔️         | Yes✔️                    |
| **Version Control**      |  Built-in✔️                   | Manual✔️  | Manual✔️ | Yes✔️    | Partial      | Yes✔️                     |
| **SQL Review / Linting** |  Yes✔️                        | No✖️      | No✖️     | Partial  | No✖️          | Partial                  |
| **Multi-Env Pipeline**   |  Yes✔️                        | Partial   | No✖️     | No✖️     | No✖️          | Partial                  |
| **Audit & Logs**         |  Yes✔️                        | Partial   | No✖️     | No✖️     | No✖️          | Yes✔️                    |
| **RBAC / Security**      |  Yes✔️                        | No✖️      | No✖️     | No✖️     | No✖️          | Yes✔️                    |
| **Databases Supported**  | Many (Postgres, MySQL, etc.) | Many      | Many     | Few     | Few           | SQL Server only          |

- For individual devs / small projects → Flyway or Dbmate is enough.
- For schema migration automation with CI/CD → Liquibase / Atlas works.
- For enterprises needing governance, approvals, audit, multi-env sync → Bytebase is the all-in-one solution.
