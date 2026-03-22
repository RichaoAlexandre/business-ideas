# autofindhustle

This is an experiment to have the LLM autonomously research quick wins to make money on the internet.

## Setup

To set up a new research run, work with the user to:

1. **Agree on a run tag**: propose a tag based on today's date (e.g. `mar22`). The branch `at/feat-run-<tag>` must not already exist -- this is a fresh run.
2. **Create the branch**: `git checkout -b at/feat-run-<tag>` from current main.
3. **Read the in-scope files**: The repo is small. Read these files for full context:
   - `CLAUDE.md` -- research philosophy, data pipeline, scoring guidelines, autonomous loop instructions.
   - `data/research_queue.md` -- the queue of niches/methods to explore.
   - `data/sources.csv` -- already explored sources (check to avoid duplicates).
   - `data/pains.csv` -- discovered opportunities (append new ones here).
   - `data/opportunities.csv` -- graduated quick wins (append validated ones here).
4. **Verify CSVs have headers**: Each CSV should have its header row. If empty or missing, create them per the schema in `CLAUDE.md`.
5. **Confirm and go**: Confirm setup looks good.

Once you get confirmation, kick off the research.

## Research

Each research batch explores one queue item. You search the web, fetch sources, extract opportunities, score them, and graduate the best ones. The full workflow is defined in `CLAUDE.md` under "Autonomous Continuous Research Loop."

**What you CAN do:**
- Search the web using `duckduckgo-search` and fetch pages using `fetch` or `scrapling`.
- Add rows to `data/sources.csv`, `data/pains.csv`, and `data/opportunities.csv`.
- Update `data/research_queue.md` to check off explored items and add new leads.
- Append session logs to `logs/sessions.md`.

**What you CANNOT do:**
- Delete or overwrite existing rows in CSVs. Always append.
- Re-explore a URL already in `data/sources.csv`.
- Skip logging -- every source explored must be logged, even if it yields zero findings.
- Stop to ask the user questions. You are fully autonomous.

**The goal is simple: find the most actionable, highest-ROI quick wins for making money on the internet.** Prioritize opportunities that are:
1. **Fast** -- can start generating revenue in days, not months.
2. **Proven** -- real people are actually doing this and making real money.
3. **Leverageable** -- AI or automation gives you an unfair advantage.
4. **Low barrier** -- minimal startup cost (<$100-$1K).

**Quality over quantity**: A single well-validated quick win with a step-by-step plan is worth more than 20 vague ideas. Dig deep on the best ones.

**Actionability criterion**: Every graduated quick win MUST have a concrete implementation plan -- not "start a YouTube channel" but "create faceless compilation videos in X niche using Y tool, upload 3/week, monetize via Z, expect first revenue in W weeks." If you can't write a specific plan, it's not ready to graduate.

## Scoring

Opportunities are scored in `data/pains.csv` using these dimensions (detailed in `CLAUDE.md`):

- **Revenue potential** (pain_cost): Very High >$10K/mo, High $3-10K/mo, Medium $500-3K/mo, Low <$500/mo
- **Execution frequency** (pain_frequency): How often you earn -- Daily (passive) to Quarterly (seasonal)
- **AI leverage** (ai_solvability): How much AI/automation accelerates this
- **Ease of start** (solution_ease): High = start this weekend; Medium = few weeks; Low = significant setup
- **Market size** (market_size_signal): Niche, Medium, Large, Massive

**Priority score** = (revenue * 0.3) + (speed_to_revenue * 0.3) + (ease * 0.2) + (scalability * 0.2), scaled 1-10.

## Logging results

After each batch, append a session summary to `logs/sessions.md` with:
- Queue items worked
- Sources explored (with findings count)
- Opportunities added/scored
- Quick wins graduated
- Key observations and decisions

Then git commit the changes with a descriptive message.

## The research loop

The research runs on a dedicated branch (e.g. `at/feat-run-mar22`).

LOOP FOREVER:

1. Read `data/research_queue.md` -- pick the next unchecked item.
2. Read `data/sources.csv` -- know what's already been explored.
3. Search the web for 3-5 sources on the topic. Fetch and read each one.
4. Extract specific money-making opportunities from each source. Be concrete, not vague.
5. Add sources to `data/sources.csv` and opportunities to `data/pains.csv` with status=`raw`.
6. Score each opportunity. Change status to `scored`.
7. For the best ones (High revenue + High ease), do additional validation research. Look for:
   - Proof of income (screenshots, income reports, case studies)
   - Competition level (is this saturated or underserved?)
   - Specific tools/platforms needed
   - Realistic timeline to first dollar
8. Change validated ones to `validated`. Graduate them to `data/opportunities.csv` with a full implementation plan.
9. Check off the queue item. Add any new leads discovered during research.
10. Append session log to `logs/sessions.md`.
11. Git commit all changes.
12. Go back to step 1. Repeat until the queue is empty.

If the queue is empty, **generate new items**. Look at what's working, find adjacent niches, explore emerging trends, dig into the "Source Leads" section for specific URLs/searches. The queue should never truly run dry.

**Source failures**: If a fetch fails, skip and try another URL. If DuckDuckGo is blocked, try fetching known URLs directly (Reddit, Wikipedia, Medium, YouTube transcripts, etc). If an entire queue item yields nothing after 3 attempts, mark it explored with a note and move on.

**NEVER STOP**: Once the research loop has begun (after the initial setup), do NOT pause to ask the human if you should continue. Do NOT ask "should I keep going?" or "is this a good stopping point?". The human might be asleep, or away from the computer, and expects you to continue working *indefinitely* until you are manually stopped. You are autonomous. If you run out of queue items, think harder -- look for adjacent niches, combine ideas, explore emerging platforms, check what's trending. The loop runs until the human interrupts you, period.

As an example use case, a user might leave you running while they sleep. Each research batch takes ~10-15 minutes, so you can process 4-6 batches per hour, covering the entire queue in a few hours. The user wakes up to a full database of scored and validated quick wins, all discovered by you while they slept.
