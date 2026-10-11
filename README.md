# SentinelForge-AI-
SentinelForge AI — Autonomous Cybersecurity Agent

# SentinelForge AI — Autonomous Cybersecurity Agent

Free • Local-first • Windows-compatible

I'll structure this as a GitHub-ready Python project that you can run locally using Ollama, with persistent investigation memory, automated security-log analysis, multiple AI agent roles, testing, and human approval controls.

The initial version will focus on defensive SOC automation, rather than unrestricted autonomous hacking.

## 1. System architecture

Security Events / Analyst Requests

JSONL logs • Windows events • SIEM exports

Security Policy & Input Validation

Allowlisted files • Size limits • Untrusted-log handling

Detection Agent

Rule-based indicators

Triage Agent

LLM-assisted analysis

Investigation Agent

Correlate evidence

Response Advisor

Containment recommendations

Local Ollama + SQLite Memory

Incident history • Audit trail • Structured findings

Security Analyst Review

Reports • Recommendations • Human approval

### Design principles

- Offline after installation: No cloud LLM or paid API is required.
- Read-only by default: The agent analyzes evidence and recommends actions but does not modify endpoints or firewalls.
- Deterministic detection: Security rules identify indicators independently of the LLM.
- Auditable: Investigations, recommendations, and approvals are recorded locally.
- Extensible: Later versions can integrate Microsoft Sentinel, Defender XDR, Splunk, QRadar, and Chronicle through scoped connectors.

The first MVP is a local investigation assistant with automated orchestration. It is not yet a production SOC platform or an independently operating 24/7 incident responder.

## 2. Download the complete working MVP

SentinelForge AI v0.1.0

Python source • Tests • Sample logs • Security documentation

&#x20;Download SentinelForge AI project (.zip)

Includes a runnable command-line application, SQLite persistence, Ollama integration, and five automated tests.

I executed the automated tests and a sample rule-only investigation in the development environment. All five tests passed. The Ollama integration is implemented but has not been tested against a running local Ollama model in this environment.

## 3. GitHub repository structure

```
sentinelforge-ai/
├── README.md
├── pyproject.toml
├── .gitignore
│
├── sentinelforge/
│   ├── __init__.py
│   ├── __main__.py
│   ├── cli.py
│   └── core.py
│
├── tests/
│   └── test_core.py
│
├── samples/
│   └── security-events.jsonl
│
├── docs/
│   └── SECURITY.md
│
└── data/
    └── sentinelforge.db  # Created at runtime
```

The project uses Python's standard library for its core functionality, including SQLite and HTTP requests. It connects to Ollama's local `/api/chat` endpoint without a paid API key.&#x20;

[image](https://www.google.com/s2/favicons?domain=https://github.com\&sz=32)

GitHub

+1



## 4. Windows installation guide

Step 1 — Install prerequisites

Install [Python](https://www.python.org/downloads/windows/) and [Ollama for Windows](https://ollama.com/download/windows). Download and extract the project ZIP into a folder such as `C:\AI\sentinelforge-ai`.

Step 2 — Open PowerShell and create your environment

```
cd C:\AI\sentinelforge-ai
py -3 -m venv .venv
.\.venv\Scripts\python.exe -m pip install -e ".[test]"
```

Step 3 — Download your local AI model

Ensure Ollama is running, then execute:

```
ollama pull llama3.2
ollama list
```

The initial model download needs internet access. After that, inference can run locally.

Step 4 — Start your first AI investigation

```
.\.venv\Scripts\python.exe -m sentinelforge scan samples/security-events.jsonl
```

The agent performs rule-based detection, requests an Ollama investigation summary, and saves the result to SQLite.

Step 5 — Run tests and review investigation history

```
.\.venv\Scripts\python.exe -m pytest -q
.\.venv\Scripts\python.exe -m sentinelforge history
.\.venv\Scripts\python.exe -m sentinelforge show 1
```

## 5. What your agent can detect today

| Detection              | Example indicator                                                  | Severity |
| ---------------------- | ------------------------------------------------------------------ | -------- |
| Repeated failed logons | Five or more failures in one input batch for the same host/user/IP | High     |
| Audit log clearing     | Windows Event ID 1102                                              | High     |
| Encoded PowerShell     | Encoded command invocation                                         | Medium   |
| Account creation       | Windows Event ID 4720                                              | Medium   |

These are initial detection leads, not confirmed malicious activity. For example, the failed-logon rule does not yet enforce a time window, and the PowerShell rule can produce false positives.

## 6. Security controls and human approval

The MVP deliberately separates AI recommendations from actual security actions.

| Control                          | MVP implementation                                       |
| -------------------------------- | -------------------------------------------------------- |
| Arbitrary shell execution        | Disabled                                                 |
| Network scanning                 | Not implemented                                          |
| Automatic endpoint isolation     | Disabled                                                 |
| Firewall rule changes            | Disabled                                                 |
| Local LLM                        | Supported                                                |
| Persistent investigation history | SQLite                                                   |
| Human approval record            | Supported                                                |
| Prompt injection resistance      | Basic instruction separation; further hardening required |
| Encrypted database               | Not yet implemented                                      |
| RBAC and immutable audit logs    | Planned                                                  |

For example, to record an analyst's approval:

```
python -m sentinelforge approve 1 `
  --action "Review suspicious account" `
  --approver "SOC-Analyst"
```

This records approval only. It does not execute containment.

## 7. Next development milestones

v0.2

SOC Detection Engine

Sigma rules, MITRE ATT&CK mapping, time-window correlation and normalized event schemas

v0.3

Multi-Agent Investigation

Separate triage, threat intelligence, forensic analysis, correlation and reporting agents

v0.4

SIEM Integrations

Read-only connectors for Microsoft Sentinel, Defender XDR, Splunk and QRadar

v0.5

Autonomous SOC Operations

Continuous ingestion, case management, scheduling and approval-gated response workflows

v1.0

Production Security Platform

Authentication, RBAC, encryption, observability, evaluation benchmarks and deployment hardening

next milestone: v0.2, a proper detection and correlation engine. That is the foundation for making SentinelForge useful in real SOC investigations before adding more autonomous behavior.

