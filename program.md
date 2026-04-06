# autofindhustle

This is an experiment to have the LLM autonomously discover **AI automation workflow opportunities** by scraping social media and the broader internet.

The focus is on finding **real people, real pain, real demand**:
- People **complaining** on the internet about manual processes they wish were automated
- People **asking** how to automate specific workflows
- People who **have done it** — success stories, case studies, and proven use cases of AI automation
- Businesses and freelancers **paying for** or **willing to pay for** automation solutions

Every finding is rated by the **pain intensity** it reveals — how badly people need this solved and how much they'd pay.

**The user can code** — solutions can include building a SaaS, a website, a tool, an API, or a micro-product. Not limited to no-code services.

## Available MCP Tools

The agent has access to these tools for web research:

| Tool | What it does | When to use |
|------|-------------|-------------|
| **Playwright MCP** (`mcp__playwright__*`) | Full browser automation: navigate, click, scroll, fill forms, take screenshots, extract text | Scraping Reddit threads, LinkedIn posts, Twitter/X feeds. Best for JS-heavy pages and when you need to interact with the page |
| **DuckDuckGo MCP** (`mcp__duckduckgo__*`) | Web search and page fetching | Finding relevant threads via `site:reddit.com`, `site:linkedin.com`, `site:twitter.com` queries. General web search for blog posts, case studies, forums |
| **Fetch MCP** (`mcp__fetch__*`) | Fetch and read web pages | Standard web pages, articles, blog posts |

### Platform-specific strategies

- **Reddit**: Use DuckDuckGo with `site:reddit.com` to find threads, then Playwright to read full threads with comments. Key subreddits: r/automation, r/nocode, r/smallbusiness, r/entrepreneur, r/artificial, r/ChatGPT, r/zapier, r/n8n, r/workflow
- **LinkedIn**: Use DuckDuckGo with `site:linkedin.com` to find posts, then Playwright to read them. Search for posts about automation wins, AI implementation stories, workflow pain points
- **Twitter/X**: Use DuckDuckGo with `site:twitter.com` to find threads. Use Playwright to browse and scrape. Look for threads about automation, AI workflows, "I built this with AI" stories
- **Forums & blogs**: Use DuckDuckGo + Fetch for standard pages. Look for Indie Hackers, Hacker News, Medium, dev.to, YouTube transcripts

## Setup

To set up a new research run, work with the user to:

1. **Agree on a run tag**: propose a tag based on today's date (e.g. `apr06`). The branch `at/feat-run-<tag>` must not already exist -- this is a fresh run.
2. **Create the branch**: `git checkout -b at/feat-run-<tag>` from current main.
3. **Read the in-scope files**: The repo is small. Read these files for full context:
   - `CLAUDE.md` -- research philosophy, data pipeline, scoring guidelines, autonomous loop instructions.
   - `data/research_queue.md` -- the queue of searches/topics to explore.
   - `data/sources.csv` -- already explored sources (check to avoid duplicates).
   - `data/pains.csv` -- discovered pains (append new ones here).
   - `data/opportunities.csv` -- opportunities/solutions (append new ones here).
4. **Verify MCP tools**: Confirm that at least Playwright MCP and DuckDuckGo MCP are available. If not, tell the human to configure them.
5. **Verify CSVs have headers**: Each CSV should have its header row. If empty or missing, create them per the schema in `CLAUDE.md`.
6. **Confirm and go**: Confirm setup looks good.

Once you get confirmation, kick off the research.

## Research Focus: AI Automation Workflows

This run specifically targets **AI automation workflow opportunities**. You are looking for two types of signals:

### Signal 1: Pain Points (people who NEED automation)
- People complaining about repetitive manual tasks
- People asking "how do I automate X?"
- People frustrated with existing tools that don't integrate
- Businesses drowning in manual data entry, reporting, scheduling, etc.
- Freelancers spending too much time on non-billable work
- Teams struggling with handoffs between tools/platforms

### Signal 2: Success Stories (people who HAVE automated)
- "I automated X and saved Y hours/week"
- "I built an AI workflow that does Z for my clients"
- "I charge $X/month to set up automation for businesses"
- Case studies of AI agents handling customer support, lead gen, content, etc.
- Indie hackers who built automation-focused micro-SaaS
- Agencies offering AI automation as a service

### Pain Intensity Rating

Every pain discovered must be rated for **pain intensity**:

| Intensity | Signal |
|-----------|--------|
| **Critical** | Multiple people complaining, willing to pay NOW, no good solution exists |
| **High** | Clear frustration, active searching for solutions, some people paying for workarounds |
| **Moderate** | People mention it as annoying, some discussion, partial solutions exist |
| **Low** | Occasional mentions, nice-to-have, people cope with manual process |

**How to assess intensity from social posts:**
- **Upvotes/likes/engagement**: High engagement = many people share this pain
- **Urgency language**: "desperately need", "wasting hours", "losing money" = high intensity
- **Willingness to pay**: "I'd pay for this", "take my money" = validated demand
- **Frequency of similar posts**: Same pain appearing across multiple threads/platforms = widespread
- **Existing solutions mentioned**: If people list tools they've tried and rejected = unmet need

## Data Format

### `data/pains.csv` — Discovered Pains
```
id,pain_summary,source_url,source_platform,description,pain_intensity,date_added
```
- **pain_summary**: Short summary (e.g. "Realtors manually writing listing descriptions")
- **source_url**: Direct URL to the post/thread/tweet
- **source_platform**: reddit / linkedin / twitter / website
- **description**: 1-3 sentences — who has this pain, what's manual, why it hurts
- **pain_intensity**: Critical / High / Moderate / Low

### `data/opportunities.csv` — Opportunities & Solutions
```
id,pain_id,opportunity_description,source_url,source_platform,solution_type,pain_intensity,date_added
```
- **opportunity_description**: What to build or offer (SaaS, website, tool, automation, service, API)
- **solution_type**: SaaS / Website / Automation / Service / Tool / API
- **pain_intensity**: Inherited or upgraded based on validation

### `data/sources.csv` — Explored Sources (unchanged)
```
id,url,domain,industry,date_explored,findings_count,quality,notes
```

## Research

Each research batch explores one queue item by scraping social media and the web.

**What you CAN do:**
- Search the web using DuckDuckGo MCP and scrape pages using Playwright MCP or Fetch MCP.
- Browse Reddit, LinkedIn, and Twitter/X to find and read relevant threads and posts.
- Add rows to `data/sources.csv`, `data/pains.csv`, and `data/opportunities.csv`.
- Update `data/research_queue.md` to check off explored items and add new leads.
- Append session logs to `logs/sessions.md`.

**What you CANNOT do:**
- Delete or overwrite existing rows in CSVs. Always append.
- Re-explore a URL already in `data/sources.csv`.
- Skip logging -- every source explored must be logged, even if it yields zero findings.
- Stop to ask the user questions. You are fully autonomous.

**The goal: find real automation pains that can be solved by building something (SaaS, tool, website, service, API).** The user is a developer — solutions are NOT limited to no-code or services. Building a product to solve a validated pain is the ideal outcome.

**Quality over quantity**: A single well-validated pain with clear demand signal is worth more than 20 vague mentions.

## Logging results

After each batch, append a session summary to `logs/sessions.md` with:
- Queue items worked
- Sources explored (with findings count)
- Platforms scraped (Reddit/LinkedIn/Twitter/other)
- Pains added, opportunities added
- Key observations and decisions

Then git commit the changes with a descriptive message.

## The research loop

The research runs on a dedicated branch (e.g. `at/feat-run-apr06`).

LOOP FOREVER:

1. Read `data/research_queue.md` -- pick the next unchecked item.
2. Read `data/sources.csv` -- know what's already been explored.
3. **Search social media and the web** for the topic:
   - Use DuckDuckGo MCP with `site:` operators to find relevant Reddit threads, LinkedIn posts, Twitter discussions
   - Use Playwright MCP to browse and scrape full threads with comments
   - Use Fetch MCP for standard web pages (blog posts, case studies, forums)
   - Aim for 3-5 sources per batch, mixing platforms
4. **Extract pains and success stories** from each source:
   - Read threads/posts carefully, including comments/replies
   - Identify specific automation pains: what's manual? what's broken? what do they wish existed?
   - Note success stories: what did someone automate? how much time/money did it save? what do they charge?
   - Assess pain intensity from engagement, language, and frequency
   - Be concrete — name the exact workflow, tool, or process
5. Add sources to `data/sources.csv`. Add pains to `data/pains.csv`.
6. For pains with High/Critical intensity, think about what you could **build** to solve it (SaaS, tool, website, API, automation service). Add to `data/opportunities.csv`.
7. Check off the queue item. Add any new leads discovered during research.
8. Append session log to `logs/sessions.md`.
9. Git commit all changes.
10. Go back to step 1. Repeat until the queue is empty.

If the queue is empty, **generate new items**. Look at what's working, find adjacent niches, explore emerging trends, browse new subreddits. The queue should never truly run dry.

**Source failures**: If a page is blocked or fails to load, try Playwright as a fallback (or vice versa). If a platform blocks you, switch to another platform. If DuckDuckGo is blocked, try fetching known URLs directly. If an entire queue item yields nothing after 3 attempts, mark it explored with a note and move on.

**NEVER STOP**: Once the research loop has begun, do NOT pause to ask the human. The human might be asleep and expects you to work indefinitely until manually stopped. If you run out of queue items, generate new ones. The loop runs until the human interrupts you, period.
