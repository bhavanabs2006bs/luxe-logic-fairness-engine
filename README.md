# Emergency Supply Fairness Engine

**Team Luxe Logic** (Bhavana B S, Meghana K S) | HackSprint | Track: PS22 – AI Automation with n8n

An n8n workflow that helps officials share scarce emergency supplies (water, food, medicine) fairly across city wards, cycle after cycle. It senses shortages, scores need and road access, remembers which wards were underserved before ("fairness debt"), compares two distribution plans, writes a plain-language Fairness Report, and keeps a human in control.

## The problem
Public-sector supply data sits in silos: separate stock sheets, road logs and field reports. Decisions are made by hand and the trade-offs are invisible, so the same wards tend to be served last. This project makes those trade-offs visible and adjustable.

## What the workflow does
1. **Load data** – synthetic wards, warehouses, road status and fairness ledger (`data/sample_data.json`).
2. **Score and plan** – priority = need score × road access + λ × fairness debt − secondary-supply discount. Builds **Plan A** (need-based) and **Plan B** (minimum-coverage first).
3. **Compare and report** – total unmet need and worst-covered ward for each plan, plus a Fairness Report with a recommendation.
4. **Apply and remember** – writes the dispatch order and updates each ward's fairness debt for the next cycle.

All numbers come from deterministic n8n Code nodes. No AI model sets any allocation.

## Run it
1. Install n8n (`docker run -it --rm -p 5678:5678 -v n8n_data:/home/node/.n8n docker.n8n.io/n8nio/n8n`) or use n8n Cloud.
2. In n8n choose **Workflows → Import from File** and select `workflows/emergency-supply-fairness-engine.json`.
3. Click **Execute workflow**. Open each node to see its output. A sample final output is in `docs/sample_output.json`.
4. ## Screenshots
![Workflow](docs/n8n-workflow.png)
![Fairness Report](docs/fairness-report.png)
![Dispatch](docs/dispatch.png)
![Ledger update](docs/nextpatcher.png)

To see fairness debt at work, copy the `new_debt` values from the last node into the ledger in the *Load Sample Data* node and run again.

## Status
- Implemented: scoring, road-aware routing, Plan A / Plan B, comparison metrics, Fairness Report, dispatch output, fairness-debt ledger update, audit fields.
- Planned: Telegram approval with Approve A / Approve B / Edit buttons, LLM-written Fairness Report (numbers stay deterministic), Google Sheets as the data store, error-trigger workflow, replay view.

## Data note
All data is synthetic and for demonstration only. No personal data is used.

## Repository layout
```
workflows/   n8n workflow (import this)
data/        synthetic sample data
docs/        screenshots and sample output
ppt/         selection-round presentation
```
