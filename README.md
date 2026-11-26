# agent-security-lab
Intentionally vulnerable AI agent with Gmail, Drive, and web access, built to test prompt injection, data exfiltration, and credential exposure. Each phase attacks the agent, adds a defense, and measures the change in attack success rate. Fake data only. Weekly write-ups at FDSecE.com.

Agent Security Lab

A hands-on lab for testing how AI agents with real tool access can be attacked, and how to defend them.

The agent in this repo can read a Gmail inbox, read Google Drive files, and fetch web pages. Those three tools are enough to cause real damage if an attacker can steer the agent. This lab builds that agent, attacks it, and then measures how much each defense actually helps.

Warning: This agent is intentionally vulnerable. It runs only against a test Google Workspace user seeded with fake data. Do not connect it to a real inbox, real files, or production credentials.

Why this exists

Companies are giving AI agents access to email, documents, and internal systems faster than their security teams can review them. Most guidance on agent security stays at the level of "watch out for prompt injection." This lab tries to answer more specific questions:

What can an over-permissioned agent actually reach?
Which attacks work against it, and how often?
Which defenses reduce the attack success rate, and by how much?
Status
Week	Focus	Status
1	Build the target agent and threat model	In progress
2	Build the attack harness and record results	Not started
3	Add defenses and measure before and after	Not started
Threat model (summary)

The full threat model is in docs/threat-model.md.

Attack path	How it works	What's at risk
Indirect prompt injection	A poisoned email or document contains instructions the agent follows	Agent behavior
Data exfiltration	The agent is tricked into sending data out through the fetch tool	Email and file contents
Credential exposure	The agent reads a secret from a file and repeats it in its output	API keys and tokens

A fake API key (a canary) is planted in the test data. If it ever shows up in agent output or outbound requests, the attack succeeded.

How it works
User prompt
    |
    v
 Agent (LLM) ----> read_gmail   (test inbox, fake data)
    |       ----> read_drive   (test Drive, fake data)
    |       ----> fetch_url    (outbound web requests)
    v
 Response

Week 1 version has no guardrails on purpose: broad OAuth scopes, no approval step, and no output filtering. Weeks 2 and 3 break it and then fix it.

Repo layout
agent-security-lab/
  agent/          Agent code and tool definitions
  attacks/        Attack cases and test harness (week 2)
  defenses/       Guardrails and controls (week 3)
  data/           Scripts to seed the test Workspace user with fake data
  docs/           Threat model, weekly memos, results
  README.md
Setup

Setup steps will be added once the agent runs end to end. Requirements so far:

Python 3.11 or later
OpenAI Agents SDK
A Google Workspace test user and a Google Cloud project with the Gmail and Drive APIs enabled
OAuth credentials for the test user only
Weekly write-ups

Each week ends with a short memo written for a security leader, not an engineer. Memos are in docs/ and also published at fdsece.com.

Week 1: What this agent can touch and why that should worry you (coming soon)
Author

James B. Lewis, PhD, CFE - Security AI Architect focused on agent security, detection engineering, and SaaS security. FDSecE.com

License

MIT
