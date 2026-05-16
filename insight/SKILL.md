---
name: insight
description: Generate a privacy-conscious usage, workflow, or weekly activity insight report with a polished HTML output. Use when the user asks for an insight report, reflective analysis of how an agent session unfolded, a weekly report, workflow patterns, wins, friction, or next-step recommendations.
---

# Insight

Use this skill when the user wants a polished reflective report, especially if they mention `/insight`, `/insights`, `/weekly_insight`, `/weekly_insights`, session analysis, workflow patterns, or a beautiful end report.

The skill follows a four-phase report flow:
1. Collect session/source material.
2. Extract structured facets.
3. Generate concise narrative sections.
4. Render a polished HTML report.

## Privacy Defaults

This shareable version is designed not to expose machine-specific or personal details by default.

- Do not include full home-directory paths, usernames, private repo paths, tokens, emails, API keys, or account identifiers in generated reports.
- Use coarse labels like "Agent Tooling", "Product Work", "Web/Product", or "General Build Work" unless the user explicitly asks for project names.
- Treat session snippets as potentially sensitive. Quote sparingly and paraphrase when enough context exists.
- Keep generated reports local unless the user explicitly asks to share or publish them.

## What to Analyze

Build the report from the best available source material:
- The current conversation and actions taken in this turn.
- Files, diffs, logs, notes, or transcripts the user points to.
- Session artifacts in local folders if the user explicitly asks for historical analysis.
- Weekly session artifacts when the user asks for an overall weekly report.

If no historical corpus is available, generate the report from the current task and say that clearly in the subtitle or intro.

## Required Workflow

1. Gather evidence.

Read only the material needed to understand:
- What the user was trying to do
- How the work unfolded
- What worked well
- Where friction appeared
- What repeatable workflows or upgrades are suggested by the evidence

2. Build structured report data.

Create a JSON file matching `references/report_schema.md`.

Default output path:
- `/tmp/agent-insight-report.json`

3. Render the HTML report.

Run from the installed skill directory:

```bash
python3 scripts/render_insight_report.py /tmp/agent-insight-report.json -o /tmp/agent-insight-report.html
```

4. Validate the output.

Confirm the HTML file exists and is non-empty.

5. Respond with:
- A short summary in chat
- The path to the HTML report
- The highest-value insight or recommendation

## Weekly Report Mode

For an overall weekly report, run from the installed skill directory:

```bash
python3 scripts/build_weekly_insight_report.py
```

The weekly builder reads local session JSONL files, creates `/tmp/agent-weekly-insight-report.json`, and renders `/tmp/agent-weekly-insight-report.html`.

For explicit ranges:

```bash
python3 scripts/build_weekly_insight_report.py --start YYYY-MM-DD --end YYYY-MM-DD
```

For non-Codex session roots or exported JSONL logs:

```bash
python3 scripts/build_weekly_insight_report.py --sessions-root /path/to/sessions
```

Treat the output as a grounded scaffold. It is useful for "what did we do this week?" and "where should we deepen next?", but important claims should be deepened by reading the underlying session artifacts.

## Section Design

Prefer these sections:
- At a Glance
- What You Work On
- How You Use the Agent
- Impressive Things You Did
- Where Things Go Wrong
- Existing Features to Try
- New Usage Patterns
- On the Horizon
- A closing memorable note

Optional:
- Charts or bar groups
- Suggested instruction additions
- Copyable prompts or commands

## Tone

Use second person where natural.
Be specific and grounded.
Do not be gushy.
Do not invent evidence.
If something is inferred rather than directly observed, state that.

## Report Quality Bar

The report should feel polished, not like raw notes:
- Strong title and subtitle
- Compact top-line summary
- Card-based sections
- Clear contrast between wins, friction, and recommendations
- Copyable prompts/commands when useful

## Files

- Read `references/report_schema.md` before writing report JSON by hand.
- Use `scripts/render_insight_report.py` to generate focused HTML reports.
- Use `scripts/build_weekly_insight_report.py` for `/weekly_insight` and weekly-overview requests.
