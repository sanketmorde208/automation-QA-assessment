# Task 2: n8n API Integration Workflow
**Author:** Sanket Morde  
**Date:** May 26, 2026

---

## Overview
This workflow fetches trending GitHub repositories, enriches the data with additional details, applies conditional logic, and sends a formatted digest to Discord every hour.

---

## APIs Used

| API | Endpoint | Purpose |
|-----|----------|---------|
| GitHub REST API | `/search/repositories` | Fetch top 10 JavaScript repos by stars |
| GitHub REST API | `/repos/{owner}/{repo}/readme` | Fetch README of top repository |

**Why GitHub API?**
- No API key required for public data
- Real-world use case (monitoring trending repos)
- Reliable and well-documented
- Perfect for demonstrating multi-step API integration

---

## Workflow Nodes

1. **Schedule Trigger** — Fires every 1 hour automatically
2. **HTTP Request** — Calls GitHub API to fetch top 10 JavaScript repositories sorted by stars
3. **Code in JavaScript** — Filters results to keep only top 5 repos, extracts key fields (name, URL, stars, description, language, owner)
4. **HTTP Request1** — Fetches the README content of the top repository for enrichment
5. **IF Node** — Checks if top repository has more than 1000 stars
6. **Send a Message (Discord)** — Sends formatted digest to Discord channel
7. **Error Trigger + No Operation** — Catches any workflow errors silently without crashing

---

## Transformation Logic
- Fetches 10 repositories from GitHub API
- Filters down to top 5 by star count
- Extracts only required fields to keep the payload clean
- Enriches the top result with its README content via a second API call

---

## Conditional Logic
- **Condition:** `stargazers_count > 1000`
- **TRUE path** → Sends full repository details to Discord
- **FALSE path** → Sends a lower-priority notification to Discord
- **Purpose:** Demonstrates routing based on data threshold

---

## Error Handling
- **Error Trigger node** sits separately on the canvas and automatically catches any error from any node in the workflow
- **No Operation node** is connected to Error Trigger to handle errors gracefully without crashing
- Workflow never fails silently — all errors are captured

---

## Credentials
- GitHub API used without authentication (public endpoints, 60 requests/hour limit is sufficient for hourly trigger)
- Discord Bot Token stored securely in **n8n Credentials store** — not hardcoded anywhere in the workflow

---

## Execution Details
- **Trigger:** Every 1 hour
- **Average execution time:** ~2–3 seconds
- **Status:** Tested and working successfully
