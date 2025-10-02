# Bytebase

<h1 align="center">
  <a href="https://www.bytebase.com?source=github" target="_blank">
    <img alt="Bytebase" src="https://miro.medium.com/v2/resize:fit:1400/format:webp/1*9t1xylD8wwLpdoCMEf5hEw.png" />
  </a>
</h1>

## What's Bytebase

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

## Why use Bytebase?

Normally, DB migrations can get messy:
- Devs apply SQL directly in staging/prod.
- No central history.
- Risk of running unsafe queries (e.g. dropping tables in prod).

With Bytebase, you get:

✔️ Version-controlled migrations (every change is tracked).<br>
✔️ Approval workflow (like PRs in GitHub).<br>
✔️ Safe deployment (analyze SQL before running).<br>
✔️ Multi-environment sync (keep dev/stg/prod aligned).

## With Bytebase:

1. **Create a Migration Script**

Example SQL migration file:<br>
```bash
  ALTER TABLE orders ADD COLUMN delivery_date DATE;
```

2. **Push to Git** (if GitOps mode is enabled)

- Commit the SQL file into your repo (e.g., /migrations/001_add_delivery_date.sql).
- Bytebase detects it and shows it in its UI.

3. **Review in Bytebase UI**

- Another teammate reviews it (like a Pull Request).
- Bytebase warns if the SQL has risky patterns (like dropping a column with data).

4. **Apply Migration**

- Approve → Bytebase applies it to **Dev** first.
- Later, promote the same migration to **Staging**, then **Production**.

5. **Audit Trail**

- Anyone can see: “On Oct 2, user Alice added ```delivery_date``` to ```orders``` in production.”
