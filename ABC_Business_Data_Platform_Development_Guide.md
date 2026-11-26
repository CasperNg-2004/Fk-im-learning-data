# Development Environment & Practical Project Plan

## 1. Objective

Build a practical development environment and one integrated portfolio
project that develops skills across:

-   **Database Administration (DBA):** database design, users and
    permissions, backups, recovery, indexing and performance.
-   **Data Engineering:** data extraction, cleaning, validation, loading
    and pipeline automation.
-   **Data Analytics:** SQL analysis, data modelling, Power BI
    dashboards and business reporting.
-   **AI integration:** later, use an LLM-powered assistant to answer
    questions about business data.

### User environment and constraints

-   Operating system: Windows 64-bit
-   Editor: Visual Studio Code
-   AI coding assistant: Claude Code
-   Docker experience: None
-   Career objective: DBA + Data Engineering + Data Analytics + AI
-   Laptop: Intel Core i5-11400H, 12 GB RAM

Start with lightweight tools and one database service at a time. Avoid
running several resource-heavy services simultaneously.

------------------------------------------------------------------------

## 2. Integrated practical project

### Project name

**ABC Business Data Platform**

Simulate the data environment of a Malaysian company, ABC Business
Solutions Sdn. Bhd.

The project will use sample business information such as:

-   Customers
-   Employees
-   Invoices
-   Payments
-   Sales records

### High-level architecture

``` text
Business data sources
(CSV files, APIs, sample business records)
                 |
                 v
Python data pipeline
(Extract -> Clean -> Validate -> Load)
                 |
                 v
PostgreSQL database
(Tables, relationships, security, backups)
                 |
                 v
Analytics and reporting
(SQL, Python, Power BI)
                 |
                 v
Future AI assistant
(Ask questions about business information)
```

This project should grow gradually. Do not try to build the entire
architecture at once.

------------------------------------------------------------------------

## 3. Installation order

    Order Tool                 Purpose
  ------- -------------------- -------------------------------------------------
        1 Visual Studio Code   Main editor for coding and debugging
        2 Git + GitHub         Version control and remote project storage
        3 Python               Data processing, ETL and analysis
        4 Claude Code          AI coding assistant
        5 WSL 2 + Ubuntu       Linux environment on Windows
        6 Docker Desktop       Run PostgreSQL and other services in containers
        7 PostgreSQL           Main relational database
        8 DBeaver              Visual database management and SQL queries
        9 Power BI Desktop     Business intelligence and dashboards
       10 Obsidian             Technical notes and learning records
       11 Postman              Test APIs

### Tools to learn later

Do not install these at the beginning. Introduce them when the basic
project works.

  Tool             Future purpose
  ---------------- ---------------------------------------------------------
  dbt              Transform and test data in a warehouse
  Apache Airflow   Schedule and orchestrate data pipelines
  n8n              Connect applications and automate workflows
  Kafka            Real-time event streaming
  Ollama           Run supported open-weight LLMs locally
  Hugging Face     Explore models, datasets and Transformers
  MLflow           Track and evaluate machine-learning and LLM experiments
  Terraform        Infrastructure as Code
  Grafana          Monitoring and visualisation

------------------------------------------------------------------------

## 4. Configuration steps

### 4.1 Visual Studio Code

Download: https://code.visualstudio.com/

Install the Windows User Installer. Recommended extensions:

-   Python
-   Pylance
-   Jupyter
-   SQLTools
-   Docker
-   GitLens
-   YAML
-   WSL

Use the integrated terminal with \`Ctrl + \`\`.

### 4.2 Git and GitHub

Download Git for Windows: https://git-scm.com/install/win

Create a GitHub account: https://github.com/

Verify Git and configure your commit identity:

``` powershell
git --version

git config --global user.name "Your Name"
git config --global user.email "your-email@example.com"
```

Use an email associated with your GitHub account if you want commits
attributed to that account.

### 4.3 Python

Download: https://www.python.org/downloads/windows/

During installation, enable **Add Python to PATH** if offered.

Verify:

``` powershell
python --version
pip --version
```

Create the project and virtual environment:

``` powershell
mkdir ABC-Business-Data-Platform
cd ABC-Business-Data-Platform

python -m venv .venv
.venv\Scripts\Activate.ps1

python -m pip install --upgrade pip
pip install pandas sqlalchemy "psycopg[binary]" requests python-dotenv
```

If PowerShell blocks activation, run this in the current terminal
session:

``` powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
```

Then activate the environment again.

### 4.4 Claude Code

Official documentation: https://code.claude.com/docs/en/overview

Open PowerShell and run the official Windows installer:

``` powershell
irm https://claude.ai/install.ps1 | iex
```

Close and reopen the terminal, then verify:

``` powershell
claude --version
```

Open the project folder and start Claude Code:

``` powershell
cd ABC-Business-Data-Platform
claude
```

Sign in using an account or supported authentication method with Claude
Code access. Installing the CLI does not itself provide a subscription
or API allowance.

Create `CLAUDE.md` in the project root:

``` markdown
# Project Instructions

- Explain important changes before implementing them.
- Do not delete or overwrite existing files without asking.
- Use Python for data engineering tasks.
- Use PostgreSQL for the database.
- Keep code modular and readable.
- Include error handling and logging.
- Do not hardcode passwords or API keys.
- Explain SQL queries in simple terms.
- Do not introduce additional frameworks without explaining why.
```

Safe working practices:

1.  Ask Claude to inspect the project before editing.
2.  Ask it to explain a proposed change.
3.  Review code and commands before accepting them.
4.  Commit working code before larger changes.
5.  Never give it credentials, API keys or confidential company records.

### 4.5 WSL 2 and Docker Desktop

WSL installation guide:
https://learn.microsoft.com/en-us/windows/wsl/install

Open PowerShell as Administrator:

``` powershell
wsl --install
```

Restart if requested. Open Ubuntu from the Start menu and create a Linux
username and password.

Verify:

``` powershell
wsl --version
wsl --status
```

Download Docker Desktop: https://www.docker.com/products/docker-desktop/

During setup:

1.  Use the WSL 2 backend.
2.  Start Docker Desktop.
3.  Open Settings -\> Resources -\> WSL Integration.
4.  Enable integration for Ubuntu.
5.  Apply changes and restart Docker Desktop if requested.

Verify Docker:

``` powershell
docker --version
docker compose version
docker run hello-world
```

Core Docker concepts:

  Concept     Meaning
  ----------- --------------------------------------------------
  Image       Packaged template for software
  Container   Running instance of an image
  Volume      Persistent storage for container data
  Port        Network access between the container and host
  Compose     Configuration for running one or more containers

### 4.6 PostgreSQL through Docker

PostgreSQL: https://www.postgresql.org/

DBeaver: https://dbeaver.io/download/

Use Docker for the project database rather than installing a second
PostgreSQL server directly on Windows.

Create `compose.yaml`:

``` yaml
services:
  database:
    image: postgres:17
    container_name: abc-postgres
    restart: unless-stopped
    environment:
      POSTGRES_DB: abc_business
      POSTGRES_USER: abc_admin
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
```

Create `.env`:

``` env
POSTGRES_PASSWORD=replace_with_a_local_password
DATABASE_URL=postgresql+psycopg://abc_admin:replace_with_a_local_password@localhost:5432/abc_business
```

Use the same password in both values. This is for local practice only;
use a strong unique password and appropriate secret management for real
deployments.

Start PostgreSQL:

``` powershell
docker compose up -d
docker ps
```

DBeaver connection settings:

  Field      Value
  ---------- -----------------------------------
  Host       `localhost`
  Port       `5432`
  Database   `abc_business`
  Username   `abc_admin`
  Password   The password configured in `.env`

Click **Test Connection**, then Finish.

If port 5432 is already in use, stop the other PostgreSQL service or
change the host-side mapping to `5433:5432`. If using port 5433, update
the `DATABASE_URL` accordingly.

### 4.7 Power BI Desktop

Download:
https://www.microsoft.com/en-us/power-platform/products/power-bi/desktop

Later, connect Power BI to PostgreSQL and learn:

-   Power Query: clean and transform data.
-   Data modelling: define relationships between tables.
-   DAX: create calculations and measures.
-   Visualisations: build charts, KPI cards and dashboards.

### 4.8 Obsidian

Download: https://obsidian.md/

Create a vault named `Development Knowledge`.

Suggested structure:

``` text
Development Knowledge/
├── 01 Database Management/
│   ├── SQL
│   ├── PostgreSQL
│   ├── Database Security
│   └── Backup and Recovery
├── 02 Data Engineering/
│   ├── Python
│   ├── ETL and ELT
│   ├── APIs
│   └── Docker
├── 03 Data Analytics/
│   ├── SQL Analytics
│   ├── Power BI
│   └── Statistics
├── 04 AI/
│   ├── Claude Code
│   ├── LLM
│   └── AI Applications
└── 05 Projects/
    └── ABC Business Platform/
```

Record tool purposes, configuration steps, commands, errors and
solutions.

### 4.9 Postman

Download: https://www.postman.com/downloads/

Use Postman to test APIs before integrating them into Python. Learn:

-   `GET`: retrieve information.
-   `POST`: create information.
-   `PUT`: update information.
-   `DELETE`: remove information.

------------------------------------------------------------------------

## 5. Project folder structure

Create this structure in VS Code:

``` text
ABC-Business-Data-Platform/
│
├── data/
│   ├── raw/
│   │   └── invoices.csv
│   └── processed/
│
├── database/
│   ├── schema.sql
│   └── queries.sql
│
├── src/
│   ├── extract.py
│   ├── transform.py
│   └── load.py
│
├── notebooks/
├── dashboard/
├── logs/
├── tests/
│
├── .env
├── .env.example
├── .gitignore
├── CLAUDE.md
├── compose.yaml
├── requirements.txt
└── README.md
```

Create `.gitignore`:

``` gitignore
.venv/
__pycache__/
.env
*.pyc
logs/
data/processed/
.ipynb_checkpoints/
```

Keep non-confidential sample data in Git when appropriate. Never commit
real customer information or credentials.

Create `requirements.txt`:

``` text
pandas
sqlalchemy
psycopg[binary]
requests
python-dotenv
```

Install dependencies with:

``` powershell
pip install -r requirements.txt
```

------------------------------------------------------------------------

## 6. First practical exercise: Invoice ETL pipeline

### Goal

Read invoice records from a CSV file, clean and validate the records
using Python, load them into PostgreSQL, and answer business questions
using SQL.

### Step 1: Create sample data

Create `data/raw/invoices.csv`:

``` csv
invoice_id,customer_name,invoice_date,amount,status
INV001,Alpha Sdn Bhd,2026-11-01,1200,Paid
INV002,Beta Trading,2026-11-02,850,Unpaid
INV003,Alpha Sdn Bhd,2026-11-05,450,Paid
INV004,Gamma Tech,2026-11-08,2100,Unpaid
INV005,Beta Trading,2026-11-10,650,Overdue
```

### Step 2: Create the database table

Run in DBeaver:

``` sql
CREATE TABLE invoices (
    invoice_id VARCHAR(20) PRIMARY KEY,
    customer_name VARCHAR(100) NOT NULL,
    invoice_date DATE NOT NULL,
    amount NUMERIC(12,2) NOT NULL,
    status VARCHAR(20) NOT NULL
);
```

The invoice ID is the primary key, so each invoice must have a unique
identifier.

### Step 3: Write the ETL script

Create `src/etl_invoices.py`:

``` python
import pandas as pd
import os

from dotenv import load_dotenv
from sqlalchemy import create_engine

load_dotenv()

# EXTRACT
df = pd.read_csv("data/raw/invoices.csv")

print("Original data:")
print(df)

# TRANSFORM
df["customer_name"] = df["customer_name"].str.strip()
df["status"] = df["status"].str.strip().str.title()

df["invoice_date"] = pd.to_datetime(
    df["invoice_date"], errors="coerce"
)

df["amount"] = pd.to_numeric(
    df["amount"], errors="coerce"
)

df = df.drop_duplicates(subset=["invoice_id"])

df = df.dropna(
    subset=["invoice_id", "customer_name", "invoice_date", "amount"]
)

df = df[df["amount"] >= 0]

os.makedirs("data/processed", exist_ok=True)

df["invoice_date"] = df["invoice_date"].dt.date

df.to_csv(
    "data/processed/clean_invoices.csv",
    index=False
)

# LOAD
database_url = os.getenv("DATABASE_URL")
engine = create_engine(database_url)

df.to_sql(
    "invoices",
    engine,
    if_exists="append",
    index=False
)

print("ETL completed successfully!")
print(f"Records loaded: {len(df)}")
```

Run from the project root:

``` powershell
python src/etl_invoices.py
```

Expected first-run result: five cleaned records loaded into PostgreSQL.

**Important:** This first script is intended to run once against an
empty table. Running it again may cause duplicate primary-key errors.
Later, improve the pipeline with upserts or another idempotent loading
strategy.

### Step 4: Analyse the invoices with SQL

Total invoice amount:

``` sql
SELECT SUM(amount) AS total_invoice_amount
FROM invoices;
```

Expected: **RM 5,250.00**

Outstanding invoices:

``` sql
SELECT SUM(amount) AS outstanding_amount
FROM invoices
WHERE status IN ('Unpaid', 'Overdue');
```

Expected: **RM 3,600.00**

Total amount by customer:

``` sql
SELECT
    customer_name,
    SUM(amount) AS total_amount
FROM invoices
GROUP BY customer_name
ORDER BY total_amount DESC;
```

Expected results:

  Customer             Total
  --------------- ----------
  Gamma Tech        RM 2,100
  Alpha Sdn Bhd     RM 1,650
  Beta Trading      RM 1,500

### Step 5: Commit the project

After checking that the pipeline works:

``` powershell
git init
git add .
git status
git commit -m "Create first invoice ETL pipeline"
```

Check `git status` before committing. Confirm that `.env` and other
private files are not included.

------------------------------------------------------------------------

## 7. Seven-day development setup schedule

The seven-day setup is scheduled to begin on **27 November 2026 at
approximately 9:00 AM Malaysia time**, with one lesson per day through
**3 December 2026**.

  ------------------------------------------------------------------------
                           Day Topic                 Practical outcome
  ---------------------------- --------------------- ---------------------
                             1 VS Code setup         Configure extensions,
                                                     project folder and
                                                     terminal

                             2 Git and GitHub        Configure Git, create
                                                     repository and first
                                                     commit

                             3 Python                Install Python,
                                                     create virtual
                                                     environment and read
                                                     a CSV

                             4 Claude Code           Install and
                                                     authenticate, create
                                                     `CLAUDE.md`, practise
                                                     safe code review

                             5 WSL 2 and Docker      Install Ubuntu and
                                                     Docker Desktop; run
                                                     `hello-world`

                             6 PostgreSQL and        Start PostgreSQL
                               DBeaver               container, connect
                                                     DBeaver and create a
                                                     table

                             7 Invoice ETL project   Clean CSV data, load
                                                     PostgreSQL, run SQL
                                                     analysis and commit
                                                     the project
  ------------------------------------------------------------------------

Each daily lesson should include:

-   The day's objective.
-   Exact Windows PowerShell commands or interface steps.
-   Expected results.
-   Common troubleshooting.
-   A short completion check.

The lessons should adapt to installation progress rather than assume
every previous step succeeded.

------------------------------------------------------------------------

## 8. Learning roadmap after the first week

  Stage   Learning focus     Practical outcome
  ------- ------------------ -----------------------------------------------
  1       Python and SQL     CSV-to-database ETL
  2       DBA fundamentals   Roles, permissions, backup and restore
  3       Data engineering   API integration and automated pipelines
  4       Data warehousing   Fact tables, dimension tables and star schema
  5       Data analytics     Power BI management dashboard
  6       Automation         Schedule and monitor data pipelines
  7       AI integration     Ask business questions using an LLM

Suggested pace: approximately **6--8 hours per week**. Adjust based on
coursework and available time.

### DBA topics

-   Database architecture and relational modelling
-   SQL and transaction management
-   Users, roles and permissions
-   Backup and recovery
-   Indexes and query optimisation
-   Monitoring and security

### Data engineering topics

-   Python and SQL
-   CSV, JSON and API ingestion
-   ETL and ELT
-   Data validation and quality
-   Docker and environment management
-   dbt and Airflow
-   Data warehouses and orchestration

### Data analytics topics

-   SQL aggregation, CTEs and window functions
-   Data cleaning and exploratory analysis
-   Statistics and KPI design
-   Power Query and DAX
-   Power BI dashboard design
-   Business reporting and interpretation

### AI topics

-   AI, machine learning and deep learning fundamentals
-   LLMs, tokens, embeddings and vector search
-   Ollama and Hugging Face
-   Retrieval-Augmented Generation (RAG)
-   AI application frameworks
-   Evaluation, privacy and responsible use

------------------------------------------------------------------------

## 9. Installation checklist

Use this checklist to track setup progress:

-   [ ] VS Code installed and configured
-   [ ] Git installed and connected to GitHub
-   [ ] Python installed and virtual environment working
-   [ ] Claude Code installed and authenticated
-   [ ] WSL 2 and Ubuntu installed
-   [ ] Docker Desktop working with `hello-world`
-   [ ] DBeaver installed
-   [ ] PostgreSQL container running
-   [ ] Power BI Desktop installed
-   [ ] Obsidian vault created
-   [ ] Postman installed
-   [ ] Invoice ETL exercise completed

------------------------------------------------------------------------

## 10. Important working principles

1.  **Learn progressively:** do not install or run every tool at once.
2.  **Understand the code:** use Claude Code as an assistant, not as a
    replacement for learning.
3.  **Use Git frequently:** commit working versions before significant
    changes.
4.  **Protect data:** use synthetic records for practice and never
    expose confidential company data, passwords or API keys to external
    AI tools or public repositories.
5.  **Test everything:** verify SQL results, pipeline output and
    database records.
6.  **Document your work:** keep setup instructions, errors and
    solutions in Obsidian and the project README.
7.  **Build a portfolio:** preserve the architecture diagram, SQL
    scripts, Python pipeline, tests, dashboard and documentation.

## 11. Useful official resources

-   VS Code: https://code.visualstudio.com/
-   Git for Windows: https://git-scm.com/install/win
-   GitHub: https://github.com/
-   Python: https://www.python.org/downloads/windows/
-   Claude Code: https://code.claude.com/docs/en/overview
-   WSL: https://learn.microsoft.com/en-us/windows/wsl/install
-   Docker Desktop: https://www.docker.com/products/docker-desktop/
-   PostgreSQL: https://www.postgresql.org/
-   DBeaver: https://dbeaver.io/download/
-   Power BI Desktop:
    https://www.microsoft.com/en-us/power-platform/products/power-bi/desktop
-   Obsidian: https://obsidian.md/
-   Postman: https://www.postman.com/downloads/

------------------------------------------------------------------------

## Final target

By completing the setup and progressively developing the ABC Business
Data Platform, you should have a practical project demonstrating
database administration fundamentals, data engineering pipelines,
business analytics and an eventual AI integration.

**The immediate priority is to get VS Code, Git, Python and Claude Code
working first. Then introduce Docker and PostgreSQL before attempting
the invoice ETL exercise.**
