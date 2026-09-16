<div align="center">

# 🐙 GitHub Users Analysis — Chicago Developer Community

**Collects GitHub users based in Chicago with 100+ followers and their public repositories via the GitHub API, then analyses the local developer community — language preferences, activity, and engagement.**

[![Python](https://img.shields.io/badge/Python-3.10+-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![GitHub API](https://img.shields.io/badge/data-GitHub_REST_API-181717?logo=github&logoColor=white)](https://docs.github.com/en/rest)
[![pandas](https://img.shields.io/badge/analysis-pandas-150458?logo=pandas&logoColor=white)](https://pandas.pydata.org/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

</div>

> A data-collection + analysis project: query the GitHub API for a well-defined
> slice of users, clean the results into tidy CSVs, and draw conclusions about
> what the Chicago developer community looks like.

---

## Table of contents

- [What it does](#what-it-does)
- [Key findings](#key-findings)
- [Quick start](#quick-start)
- [Data dictionary](#data-dictionary)
- [How the scraping works](#how-the-scraping-works)
- [Repository map](#repository-map)

---

## What it does

1. **Search** the GitHub API for users with `location: Chicago` and **> 100 followers**.
2. **Fetch** up to 500 public repositories per user (sorted by most recently pushed).
3. **Clean** the results (e.g. normalise company names) and write two tidy CSVs.
4. **Analyse** language popularity, activity, and engagement across the community.

---

## Key findings

- **JavaScript** is the most popular language among active Chicago developers, followed by **Python** — a community leaning toward **web development** and **data science**.
- The dataset supports further analysis of engagement (followers, public-repo counts) and licensing choices across repositories.

---

## Quick start

```bash
# a GitHub token is read from the environment — never hard-coded
export GITHUB_TOKEN="your-token"

pip install requests pandas
python scarpping.py        # collects users.csv + repositories.csv
python solutions.py        # runs the analysis
```

> **Security note:** the script reads `GITHUB_TOKEN` from the environment
> (`os.environ['GITHUB_TOKEN']`). Never commit a token to source control.

---

## Data dictionary

**`users.csv`** — one row per user:

| Field | Meaning |
|-------|---------|
| `login`, `name` | GitHub handle and display name |
| `company`, `location` | normalised company; location string |
| `email`, `hireable`, `bio` | profile details |
| `public_repos`, `followers`, `following` | counts |
| `created_at` | account creation date |

**`repositories.csv`** — one row per repository:

| Field | Meaning |
|-------|---------|
| `login`, `full_name` | owner handle; `owner/repo` |
| `created_at` | repo creation date |
| `stargazers_count`, `watchers_count` | popularity |
| `language` | primary language |
| `has_projects`, `has_wiki` | feature flags |
| `license_name` | license, if any |

---

## How the scraping works

- Paginated GitHub REST calls with a short delay between pages to respect rate limits.
- Handles missing fields gracefully and cleans company names (strips a leading `@`, uppercases).
- Writes results with the standard-library `csv` module — no heavyweight dependencies for collection.

---

## Repository map

```
scarpping.py        collects users + repositories from the GitHub API → CSVs
solutions.py        analysis over the collected data
users.csv           collected user profiles
repositories.csv    collected repository metadata
```

---

<div align="center">

GitHub API data collection & community analysis · [MIT License](LICENSE)

</div>
