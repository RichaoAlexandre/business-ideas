# Internet Quick Wins Research Project

## Purpose
This project discovers **quick wins to make money fast on the internet**. The focus is on methods, loopholes, underexploited niches, arbitrage opportunities, and lean business models that can generate revenue in days or weeks -- not months or years.

We follow a **speed-first methodology**: find the opportunity, validate it's real and actionable, then plan the fastest path to first dollar. We do NOT chase ideas that require large upfront investment, long build cycles, or massive teams.

## Research Philosophy
- Look for **gaps, inefficiencies, and arbitrage** across internet platforms, marketplaces, and digital ecosystems.
- Focus on opportunities where the **time-to-first-dollar is days to weeks**, not months.
- Prioritize methods that are **repeatable and scalable** -- not one-off windfalls.
- Look for things **most people overlook**: platform quirks, underpriced assets, emerging trends before they peak, cross-platform arbitrage, underserved micro-niches.
- Explore broadly: **freelancing hacks, digital product flips, content monetization, marketplace arbitrage, AI-powered services, affiliate loopholes, micro-SaaS, automation plays, drop servicing, domain/asset flipping**, and anything else that works.
- Everything must be **legal and ethical** -- no scams, no fraud, no black-hat SEO, no terms-of-service violations that risk account bans.

## Data Pipeline

The project uses a 3-stage pipeline:

```
Sources -> Opportunities -> Quick Wins
(explore)  (extract & score)  (graduate & plan)
```

### Stage 1: `data/sources.csv` -- Explored Sources
Tracks every web source, article, thread, or video explored to avoid re-exploring.

| Column | Type | Description |
|--------|------|-------------|
| id | int | Auto-incrementing |
| url | string | Full URL |
| domain | string | Domain name |
| industry | string | Niche or category the source covers |
| date_explored | YYYY-MM-DD | When explored |
| findings_count | int | Number of opportunities extracted |
| quality | Low/Medium/High | How useful the source was |
| notes | string | Summary of findings |

### Stage 2: `data/pains.csv` -- Discovered Opportunities (Core Dataset)
Every money-making opportunity discovered, scored on multiple dimensions.

| Column | Type | Description |
|--------|------|-------------|
| id | int | Auto-incrementing |
| pain_name | string | Short descriptive name of the opportunity |
| industry | string | Niche or category |
| description | string | 1-3 sentence summary of the opportunity |
| who_suffers | string | Who is the target buyer/customer/audience |
| current_process | string | How people currently do this (the gap you exploit) |
| why_manual | string | Why this opportunity exists (what others are missing) |
| pain_cost | Low/Medium/High/Very High | Revenue potential per month |
| pain_frequency | Daily/Weekly/Monthly/Quarterly | How often you can execute |
| ai_solvability | Low/Medium/High | How much AI/automation can accelerate this |
| solution_ease | Low/Medium/High | How easy to start (High = this weekend) |
| market_size_signal | string | Market/demand size indicator |
| source_url | string | Where the opportunity was found |
| source_id | int | FK to sources.csv |
| date_added | YYYY-MM-DD | When added |
| status | raw/scored/validated/rejected | Pipeline status |

### Stage 3: `data/opportunities.csv` -- Graduated Quick Wins
Opportunities that have been validated and have a clear path to fast revenue.

| Column | Type | Description |
|--------|------|-------------|
| id | int | Auto-incrementing |
| pain_id | int | FK to pains.csv |
| proposed_solution | string | Step-by-step implementation plan |
| solution_type | string | e.g. Arbitrage, Digital Product, Service, Automation, Flip, Affiliate |
| revenue_model | string | How it makes money and expected $/month |
| target_customer | string | Who pays you |
| estimated_complexity | Low/Medium/High | Setup complexity |
| competition_notes | string | How saturated is this? What's the edge? |
| priority_score | float | Composite 1-10 score |
| date_graduated | YYYY-MM-DD | When promoted |
| notes | string | Additional thoughts, risks, and tips |

## Research Workflow

### Autonomous Continuous Research Loop
When asked to research, run **continuously in a loop** until the research queue is exhausted. Do NOT stop to ask the user questions. Do NOT wait for confirmation between batches. Make your own judgment calls and keep moving. If a source fails to load, skip it and try another. If you're unsure about a score, make your best estimate -- you can always adjust later.

**Each loop iteration (one batch):**

1. **Check the queue**: Read `data/research_queue.md` for next niches/methods to explore.
2. **Check explored sources**: Read `data/sources.csv` to avoid re-exploring the same URLs.
3. **Search for sources**: Use `duckduckgo-search` to find relevant articles, Reddit threads, YouTube breakdowns, Twitter threads, and blog posts. Then use `fetch` or `scrapling` to read the pages. Explore 3-5 sources per batch.
4. **Extract opportunities**: For each source, identify specific money-making methods. Add each as a row in `data/pains.csv` with status=`raw`.
5. **Log the source**: Add the explored URL to `data/sources.csv` with findings count.
6. **Score opportunities**: Review raw opportunities and fill in revenue potential, ai_solvability, solution_ease. Change status to `scored`.
7. **Validate best opportunities**: For opportunities with High revenue AND High ease, do additional research to validate (check if people are actually doing this, look for proof of income, check competition level). Change status to `validated` or `rejected`.
8. **Graduate quick wins**: For validated opportunities, create an entry in `data/opportunities.csv` with a step-by-step implementation plan and priority_score.
9. **Update the queue**: Check off explored items in `data/research_queue.md`, add newly discovered leads.
10. **Log the session**: Append a session summary to `logs/sessions.md` with: queue items worked, sources explored (with findings count), opportunities added/scored, decisions and observations, quick wins graduated. Never read this file back -- it is for the user's review only.
11. **Loop**: Go back to step 1 and start the next batch. Keep going until the queue is empty or all items have been explored.

**Error handling**: If a fetch fails, log it and move on. If a search returns no results, try alternative search terms. If an entire queue item yields nothing after 3 sources, mark it as explored with a note and move to the next item. Never stop to ask the user.

### Scoring Guidelines
- **pain_cost** (Revenue Potential): Very High = >$10K/month; High = $3K-$10K/month; Medium = $500-$3K/month; Low = <$500/month
- **pain_frequency**: How often you can execute or earn (Daily = recurring passive; Weekly = active hustle; Monthly = project-based; Quarterly = seasonal)
- **ai_solvability**: High = AI/automation does 80%+ of the work; Medium = AI assists but needs human input; Low = mostly manual
- **solution_ease**: High = can start this weekend with <$100; Medium = needs a few weeks and <$1K; Low = requires significant setup or capital
- **priority_score**: Weighted composite: (revenue_potential * 0.3) + (speed_to_revenue * 0.3) + (ease_of_start * 0.2) + (scalability * 0.2), scaled 1-10

### Priority Score Numeric Mapping
For computing priority_score, use: Low=1, Medium=2, High=3, Very High=4. Market size: Niche=1, Medium=2, Large=3, Massive=4.

## Rules
- Always **append** new rows to CSVs; never overwrite existing data.
- Always check `data/sources.csv` before exploring a URL to avoid duplicates.
- Keep descriptions concise but specific -- name the exact method, not vague categories like "freelancing" or "e-commerce."
- When exploring the web, summarize findings before adding to CSVs.
- Use `duckduckgo-search` to discover sources, then `fetch` or `scrapling` to read them.
- Use `fetch` for standard web pages and `scrapling` for JavaScript-heavy sites.
- Log every explored source, even if it yields zero findings (set findings_count=0).
- When in doubt about an opportunity's validity, add it as `raw` and revisit later.
- Use proper CSV quoting: wrap fields containing commas in double quotes, escape internal double quotes by doubling them.
- Always append to `logs/sessions.md` after each research batch. Never read it -- it is for the user's review only.
- **Bias toward actionable**: every graduated quick win should have a clear "do this tomorrow" implementation plan.
- **Proof over theory**: prioritize opportunities where you can find evidence of real people making real money, not just theoretical possibilities.
