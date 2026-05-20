# hol_test_rep_2

Hands-on Lab: **Secure by Default: Implementing Zero-Trust Identity and Access in Snowflake**

## What this repo does

This repo contains a GitHub Actions workflow that authenticates to Snowflake using **Workload Identity Federation (WIF)** — no passwords, no key pairs, no secrets stored anywhere.

GitHub Actions mints a short-lived OIDC token. Snowflake validates it. Connection established. Zero credentials.

## Setup (5 minutes)

1. **Fork this repo** to your own GitHub account
2. **Add a repository variable**:
   - Settings → Secrets and variables → Actions → Variables → New repository variable
   - Name: `SNOWFLAKE_ACCOUNT`
   - Value: your Snowflake account locator (on your lab card)
3. **Run the Snowflake SQL** (in your lab account as ACCOUNTADMIN):
   ```sql
   CREATE USER IF NOT EXISTS GITHUB_PIPELINE_SVC
       TYPE = SERVICE
       DEFAULT_ROLE = PIPELINE_SVC_ROLE
       WORKLOAD_IDENTITY = (
           TYPE    = OIDC
           ISSUER  = 'https://token.actions.githubusercontent.com'
           SUBJECT = 'repo:<YOUR_GITHUB_USERNAME>/summit26-iam-lab:ref:refs/heads/main'
       )
       COMMENT = 'WIF service user — secretless auth via GitHub Actions OIDC';
