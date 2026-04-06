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

### Stage 2: `data/pains.csv` -- Discovered Pains
Real pain points found on social media and the web — people complaining, asking for help, or wishing a tool existed.

| Column | Type | Description |
|--------|------|-------------|
| id | int | Auto-incrementing |
| pain_summary | string | Short summary of the pain (e.g. "Realtors manually writing listing descriptions") |
| source_url | string | Direct URL to the post/thread/tweet where the pain was expressed |
| source_platform | string | reddit / linkedin / twitter / website |
| description | string | 1-3 sentence description of the pain, who has it, and context |
| pain_intensity | Critical/High/Moderate/Low | How badly people need this solved (based on engagement, urgency, willingness to pay) |
| date_added | YYYY-MM-DD | When added |

### Stage 3: `data/opportunities.csv` -- Opportunities & Solutions
Opportunities derived from pains — things you could build (SaaS, website, tool, service, automation workflow).

| Column | Type | Description |
|--------|------|-------------|
| id | int | Auto-incrementing |
| pain_id | int | FK to pains.csv |
| opportunity_description | string | What to build or offer to solve this pain |
| source_url | string | URL to supporting evidence (success story, case study, or the original pain post) |
| source_platform | string | reddit / linkedin / twitter / website |
| solution_type | string | SaaS / Website / Automation / Service / Tool / API |
| pain_intensity | Critical/High/Moderate/Low | Inherited from the pain or upgraded based on validation |
| date_added | YYYY-MM-DD | When added |

## Research Workflow

### Autonomous Continuous Research Loop
When asked to research, run **continuously in a loop** until the research queue is exhausted. Do NOT stop to ask the user questions. Do NOT wait for confirmation between batches. Make your own judgment calls and keep moving. If a source fails to load, skip it and try another. If you're unsure about a score, make your best estimate -- you can always adjust later.

**Each loop iteration (one batch):**

1. **Check the queue**: Read `data/research_queue.md` for next topic/search to explore.
2. **Check explored sources**: Read `data/sources.csv` to avoid re-exploring the same URLs.
3. **Search social media and the web**: Use DuckDuckGo MCP with `site:` operators, Playwright MCP for JS-heavy pages, and Fetch MCP for standard pages. Explore 3-5 sources per batch, mixing platforms (Reddit, LinkedIn, Twitter, forums, blogs).
4. **Extract pains**: For each source, identify specific automation pain points. Add each as a row in `data/pains.csv` with pain_intensity rating.
5. **Log the source**: Add the explored URL to `data/sources.csv` with findings count.
6. **Create opportunities**: For High/Critical intensity pains, think about what could be built (SaaS, tool, website, API, service) and add to `data/opportunities.csv`.
7. **Update the queue**: Check off explored items in `data/research_queue.md`, add newly discovered leads.
8. **Log the session**: Append a session summary to `logs/sessions.md`. Never read this file back -- it is for the user's review only.
9. **Loop**: Go back to step 1 and start the next batch. Keep going until the queue is empty or all items have been explored.

**Error handling**: If a fetch fails, log it and move on. If a search returns no results, try alternative search terms. If an entire queue item yields nothing after 3 sources, mark it as explored with a note and move to the next item. Never stop to ask the user.

### Pain Intensity Guidelines
- **Critical**: Multiple people complaining, willing to pay NOW, no good solution exists
- **High**: Clear frustration, active searching for solutions, some people paying for workarounds
- **Moderate**: People mention it as annoying, some discussion, partial solutions exist
- **Low**: Occasional mentions, nice-to-have, people cope with manual process

Assess intensity from: engagement levels (upvotes/likes), urgency language, willingness to pay, frequency of similar posts across platforms, and whether people have tried and rejected existing solutions.

## Rules
- Always **append** new rows to CSVs; never overwrite existing data.
- Always check `data/sources.csv` before exploring a URL to avoid duplicates.
- Keep descriptions concise but specific -- name the exact method, not vague categories like "freelancing" or "e-commerce."
- When exploring the web, summarize findings before adding to CSVs.
- Use DuckDuckGo MCP (`mcp__duckduckgo__*`) to search the web with `site:` operators for platform-specific results.
- Use Playwright MCP (`mcp__playwright__*`) to browse and scrape JS-heavy pages (Reddit, LinkedIn, Twitter).
- Use Fetch MCP (`mcp__fetch__*`) for standard web pages.
- Log every explored source, even if it yields zero findings (set findings_count=0).
- When in doubt about an opportunity's validity, add it as `raw` and revisit later.
- Use proper CSV quoting: wrap fields containing commas in double quotes, escape internal double quotes by doubling them.
- Always append to `logs/sessions.md` after each research batch. Never read it -- it is for the user's review only.
- **Bias toward actionable**: every graduated quick win should have a clear "do this tomorrow" implementation plan.
- **Proof over theory**: prioritize opportunities where you can find evidence of real people making real money, not just theoretical possibilities.
