# travel-reimbursement-agent
Travel Reimbursement Approval Agent: a program that reads an employee’s expense claim, checks it against a company policy, and decides whether to pay it.

# The business problem
When employees travel, they submit expense claims (flights, hotels, meals, etc.). Someone normally checks each claim against policy: are the items allowed, are receipts attached, are amounts within limits, who needs to approve? Your agent automates that first-pass review.

# Requirements: Python 3.10+, pandas, matplotlib, groq (installed by the first code cell) and, optionally, ipywidgets for the interactive UI.

Environment variable: GROQ_API_KEY (a free key is available from console.groq.com).

Google Colab: add it under Secrets (key icon) and enable notebook access.
Local Jupyter: export GROQ_API_KEY=... before starting Jupyter.
How to run: open the notebook in Colab or Jupyter and choose Run all. No manual steps are needed.

Modes (AGENT_MODE in the configuration cell)

AUTO (default): use Groq when a key is available, otherwise use the local fallback pipeline. If Groq fails for a claim, that claim falls back to the local pipeline and the reason is recorded.
GROQ: strict mode. Any Groq failure raises an error, so nothing is hidden.
LOCAL: never call an LLM.
How to confirm the LLM really ran: see the Agent trace section. It lists the orchestrator used for each claim (groq or local_fallback), the tools the model chose to call, the model's own reasoning, and whether Python had to override the proposal.

Troubleshooting

400 tool_use_failed: the model formatted its final answer badly. The code recovers the answer automatically; if it still fails, re-run the cell.
429 rate limit: wait a minute and re-run.
orchestrator = local_fallback in the trace: check fallback_reason (usually a missing key or an API error).
Key design choice: the LLM proposes and chooses tools, while Python is authoritative for every amount and policy outcome. See Design Notes & Reasoning below.
