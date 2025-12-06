# Automated Phishing Triage Bot (SOAR)

## Project Overview
This project is a **Security Orchestration, Automation, and Response (SOAR)** playbook designed to automate the analysis of suspicious emails. It reduces the Mean Time to Respond (MTTR) for phishing incidents from ~15 minutes (manual) to **<5 seconds** (automated).

## Architecture
The workflow ingests emails, parses content, enriches data via Threat Intelligence, and handles alerting based on severity.

![Workflow Architecture]!
[Uploading Workflow.png…]()


### The Logic Flow
1.  **Ingestion:** Monitors Gmail Inbox for reported phishing attempts.
2.  **Extraction:** Uses custom **Regex** (JavaScript) to extract URLs from HTML/Text bodies, handling obfuscation.
3.  **Enrichment:** Queries **VirusTotal API v3** to retrieve real-time reputation scores and community votes.
4.  **Decision Logic:** * If `Malicious Score > 0`: Flag as Critical -> Alert SOC Team.
    * If `Malicious Score == 0`: Flag as Safe -> Log and Close.
5.  **Response:** Sends a formatted alert to Telegram with the URL, risk score, and scan ID.

## Technical Stack
* **Platform:** n8n (Self-Hosted via Docker)
* **Language:** JavaScript (ES6) for parsing logic
* **APIs:** VirusTotal v3, Gmail API, Telegram Bot API
* **Infrastructure:** Linux Mint (Localhost)

## Evidence of Execution
**Successful Detection of Malicious Payload:**
![Telegram Alert]
![Uploading tel_msg_new.png…]()


## How to Use
1.  Import `workflow.json` into n8n.
2.  Configure Credential Nodes (Gmail OAuth2, VirusTotal API, Telegram Token).
3.  Activate workflow.

## Future Improvements
* Add **Defang** logic to neutralize URLs in alerts (e.g., `http[://]bad.com`).
* Integrate **TheHive** for case management creation.
