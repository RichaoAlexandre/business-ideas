# AI Agent Opportunity Research Project

## Purpose
This project identifies **real-world business pains that are solvable by AI agents**. The focus is on tasks that are highly manual, repetitive, error-prone, and costly -- where autonomous or semi-autonomous AI agents can deliver transformative value.

We follow a **pain-first methodology**: discover the pain, validate its severity, then design the solution. We do NOT start with technology and look for a problem.

## Research Philosophy
- Look for **human-intensive processes** that exist because of regulation, legacy systems, or institutional inertia.
- Focus on pains where the cost is measurable (wasted hours, error rates, compliance fines).
- Prioritize pains where AI agents (LLMs + tool use + autonomy) are a natural fit -- document processing, multi-step workflows, data gathering and synthesis, form filling, communication routing.
- Starting focus areas: **Healthcare/Compliance** and **Back-office Operations**, but explore broadly.

## Data Pipeline

The project uses a 3-stage pipeline:

```
Sources -> Pains -> Opportunities
(explore)  (extract & score)  (graduate & plan)
```

### Stage 1: `data/sources.csv` -- Explored Sources
Tracks every web source, article, or report explored to avoid re-exploring.

| Column | Type | Description |
|--------|------|-------------|
| id | int | Auto-incrementing |
| url | string | Full URL |
| domain | string | Domain name |
| industry | string | Industry the source covers |
| date_explored | YYYY-MM-DD | When explored |
| findings_count | int | Number of pains extracted |
| quality | Low/Medium/High | How useful the source was |
| notes | string | Summary of findings |

### Stage 2: `data/pains.csv` -- Discovered Pains (Core Dataset)
Every pain discovered, scored on multiple dimensions.

| Column | Type | Description |
|--------|------|-------------|
| id | int | Auto-incrementing |
| pain_name | string | Short descriptive name |
| industry | string | Industry vertical |
| description | string | 1-3 sentence summary |
| who_suffers | string | Who bears the pain |
| current_process | string | How it is done today |
| why_manual | string | Why it has not been automated |
| pain_cost | Low/Medium/High/Very High | Financial impact |
| pain_frequency | Daily/Weekly/Monthly/Quarterly | How often the pain occurs |
| ai_solvability | Low/Medium/High | How well AI agents can address this |
| solution_ease | Low/Medium/High | How easy to build |
| market_size_signal | string | Market size indicator |
| source_url | string | Where the pain was found |
| source_id | int | FK to sources.csv |
| date_added | YYYY-MM-DD | When added |
| status | raw/scored/validated/rejected | Pipeline status |

### Stage 3: `data/opportunities.csv` -- Graduated Opportunities
Pains that have been validated and are worth building a business around.

| Column | Type | Description |
|--------|------|-------------|
| id | int | Auto-incrementing |
| pain_id | int | FK to pains.csv |
| proposed_solution | string | What the AI agent solution looks like |
| solution_type | string | e.g. Autonomous Agent, Copilot, Workflow Automation |
| revenue_model | string | How it makes money |
| target_customer | string | Specific buyer persona |
| estimated_complexity | Low/Medium/High | Build complexity |
| competition_notes | string | Competitive landscape |
| priority_score | float | Composite 1-10 score |
| date_graduated | YYYY-MM-DD | When promoted |
| notes | string | Additional thoughts |

## Research Workflow

### Autonomous Batch Research
When asked to research, follow this process:

1. **Check the queue**: Read `data/research_queue.md` for next industries/sources to explore.
2. **Check explored sources**: Read `data/sources.csv` to avoid re-exploring the same URLs.
3. **Search for sources**: Use `duckduckgo-search` to find relevant articles, reports, and forums for the queue items. Then use `fetch` or `scrapling` to read the pages. Explore 3-5 sources per batch.
4. **Extract pains**: For each source, identify specific manual/painful processes. Add each as a row in `data/pains.csv` with status=`raw`.
5. **Log the source**: Add the explored URL to `data/sources.csv` with findings count.
6. **Score pains**: Review raw pains and fill in pain_cost, ai_solvability, solution_ease. Change status to `scored`.
7. **Validate best pains**: For pains with High pain_cost AND High ai_solvability, do additional research to validate. Change status to `validated` or `rejected`.
8. **Graduate opportunities**: For validated pains, create an entry in `data/opportunities.csv` with a proposed solution and priority_score.
9. **Update the queue**: Check off explored items in `data/research_queue.md`, add newly discovered leads.
10. **Log the session**: Append a session summary to `logs/sessions.md` with: queue items worked, sources explored (with findings count), pains added/scored, decisions and observations, opportunities graduated. Never read this file back — it is for the user's review only.

### Scoring Guidelines
- **pain_cost**: Very High = >$100K/year per org wasted; High = $10K-$100K; Medium = $1K-$10K; Low = <$1K
- **pain_frequency**: How often the painful process runs
- **ai_solvability**: High = LLM + tools can handle 80%+ autonomously; Medium = needs human-in-loop; Low = needs physical/hardware intervention
- **solution_ease**: High = can build MVP in weeks; Medium = months; Low = requires deep domain integration
- **priority_score**: Weighted composite: (pain_cost_num * 0.3) + (ai_solvability_num * 0.3) + (solution_ease_num * 0.2) + (market_size_num * 0.2), scaled 1-10

### Priority Score Numeric Mapping
For computing priority_score, use: Low=1, Medium=2, High=3, Very High=4. Market size: Niche=1, Medium=2, Large=3, Massive=4.

## Rules
- Always **append** new rows to CSVs; never overwrite existing data.
- Always check `data/sources.csv` before exploring a URL to avoid duplicates.
- Keep descriptions concise but specific -- name the exact process, not vague categories.
- When exploring the web, summarize findings before adding to CSVs.
- Use `duckduckgo-search` to discover sources, then `fetch` or `scrapling` to read them.
- Use `fetch` for standard web pages and `scrapling` for JavaScript-heavy sites.
- Log every explored source, even if it yields zero findings (set findings_count=0).
- When in doubt about a pain's validity, add it as `raw` and revisit later.
- Use proper CSV quoting: wrap fields containing commas in double quotes, escape internal double quotes by doubling them.
- Always append to `logs/sessions.md` after each research batch. Never read it — it is for the user's review only.
