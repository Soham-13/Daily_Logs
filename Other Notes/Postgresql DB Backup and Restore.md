
---

## A. Prerequisites

1. Install PostgreSQL client tools (psql, pg_dump, pg_restore) on your machine.  
    Example path on Windows:  
    `C:\Program Files\PostgreSQL\16\bin`
    
2. Collect database details:
    
    - Live DB host (example):  
        `aussizzxero.cdlxpupsinag.ap-southeast-2.rds.amazonaws.com`
        
    - Port (usually `5432`)
        
    - Username (example: `postgres`)
        
    - Password
        
    - Live database name: `AURA_KNOWLEDGE_HUB`
        
    - Staging database name: `AURA_KNOWLEDGE_HUB_STAGING`
        
3. All commands below are run from:
    
    ```bat
    cd "C:\Program Files\PostgreSQL\16\bin"
    ```
    

---

## B. Confirm the correct live database

1. Connect to PostgreSQL:
    
    ```bat
    psql -h aussizzxero.cdlxpupsinag.ap-southeast-2.rds.amazonaws.com -U postgres -d postgres
    ```
    
2. List all databases:
    
    ```sql
    \l
    ```
    
3. Confirm that `AURA_KNOWLEDGE_HUB` is the live Aura database (check owner, size, etc.).
    
4. Exit psql:
    
    ```sql
    \q
    ```
    

---

## C. Take a backup from the live AURA_KNOWLEDGE_HUB database

1. From Command Prompt (in the bin directory), run:
    
    ```bat
    pg_dump -h aussizzxero.cdlxpupsinag.ap-southeast-2.rds.amazonaws.com -U postgres -d AURA_KNOWLEDGE_HUB -F c -f AURA_KNOWLEDGE_HUB_LIVE.dump
    ```
    
2. Enter the password when prompted.
    
3. After it finishes, verify that the file `AURA_KNOWLEDGE_HUB_LIVE.dump` exists in:
    
    ```text
    C:\Program Files\PostgreSQL\16\bin
    ```
    

---

## D. Create a fresh empty STAGING database

1. Connect to PostgreSQL:
    
    ```bat
    psql -h aussizzxero.cdlxpupsinag.ap-southeast-2.rds.amazonaws.com -U postgres -d postgres
    ```
    
2. Create the staging database:
    
    ```sql
    CREATE DATABASE "AURA_KNOWLEDGE_HUB_STAGING"
      WITH TEMPLATE = template0
           ENCODING = 'UTF8'
           LC_COLLATE = 'en_US.utf8'
           LC_CTYPE   = 'en_US.utf8'
           OWNER = postgres;
    ```
    
3. Confirm it appears in the database list:
    
    ```sql
    \l
    ```
    
4. Exit:
    
    ```sql
    \q
    ```
    

---

## E. Restore the live backup into the staging database

1. From Command Prompt (still in the bin directory), run:
    
    ```bat
    pg_restore -h aussizzxero.cdlxpupsinag.ap-southeast-2.rds.amazonaws.com -U postgres -d AURA_KNOWLEDGE_HUB_STAGING -v AURA_KNOWLEDGE_HUB_LIVE.dump
    ```
    
2. Enter password when prompted.
    
3. Wait for the restore to complete. It should end without errors.
    

---

## F. Verify the staging database

1. Connect to the staging database:
    
    ```bat
    psql -h aussizzxero.cdlxpupsinag.ap-southeast-2.rds.amazonaws.com -U postgres -d AURA_KNOWLEDGE_HUB_STAGING -U postgres
    ```
    
2. List tables:
    
    ```sql
    \dt
    ```
    
3. Compare row counts for a few key tables with the live DB (run the same queries in both databases):
    
    Example:
    
    ```sql
    SELECT COUNT(*) FROM scrapped_news_testing;
    SELECT COUNT(*) FROM scrapped_pdfs;
    SELECT COUNT(*) FROM scrape_links;
    ```
    
4. Exit:
    
    ```sql
    \q
    ```
    

---

## G. What to do if the wrong dump was restored or partially restored

If you accidentally restore a wrong database dump into `AURA_KNOWLEDGE_HUB_STAGING` or stop a restore mid-way, always reset the staging DB before doing the correct restore.

1. Connect to postgres:
    
    ```bat
    psql -h aussizzxero.cdlxpupsinag.ap-southeast-2.rds.amazonaws.com -U postgres -d postgres
    ```
    
2. Terminate any active connections to the staging DB:
    
    ```sql
    SELECT pg_terminate_backend(pid)
    FROM pg_stat_activity
    WHERE datname = 'AURA_KNOWLEDGE_HUB_STAGING';
    ```
    
3. Drop the staging database:
    
    ```sql
    DROP DATABASE "AURA_KNOWLEDGE_HUB_STAGING";
    ```
    
4. Recreate it (same as section D):
    
    ```sql
    CREATE DATABASE "AURA_KNOWLEDGE_HUB_STAGING"
      WITH TEMPLATE = template0
           ENCODING = 'UTF8'
           LC_COLLATE = 'en_US.utf8'
           LC_CTYPE   = 'en_US.utf8'
           OWNER = postgres;
    ```
    
5. Exit:
    
    ```sql
    \q
    ```
    
6. Run the correct restore command again:
    
    ```bat
    pg_restore -h aussizzxero.cdlxpupsinag.ap-southeast-2.rds.amazonaws.com -U postgres -d AURA_KNOWLEDGE_HUB_STAGING -v AURA_KNOWLEDGE_HUB_LIVE.dump
    ```
    

---

## H. Point the application to the staging database

1. In the application configuration for staging (environment variables or config files), change:
    
    - `DB_NAME` to `AURA_KNOWLEDGE_HUB_STAGING`
        
    - `DB_HOST` to the correct RDS host (if different)
        
    - Any other connection strings as needed.
        
2. Make sure:
    
    - Scheduled jobs, scrapers, or cron tasks that should not affect production are disabled or use safe endpoints.
        
    - Any external integrations (email, SMS, WhatsApp, payments) use sandbox or are disabled in the staging environment.
        

---
