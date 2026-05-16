# /weekly_insight

Generate a polished weekly activity report from local session artifacts.

## Arguments

- Optional date range such as `last 7 days`, `2026-05-10 to 2026-05-16`, or `this week`.

## Workflow

1. Use the `insight` skill.
2. Run the weekly report builder from the installed skill directory. Defaults to the last seven calendar days ending today:

```bash
python3 scripts/build_weekly_insight_report.py
```

3. For an explicit date range, pass `--start` and `--end`:

```bash
python3 scripts/build_weekly_insight_report.py --start YYYY-MM-DD --end YYYY-MM-DD
```

4. For exported or non-default session logs, pass `--sessions-root`:

```bash
python3 scripts/build_weekly_insight_report.py --sessions-root /path/to/sessions
```

5. Verify both outputs exist and are non-empty:

```text
/tmp/agent-weekly-insight-report.json
/tmp/agent-weekly-insight-report.html
```

6. Respond with the HTML path, session count, dominant work areas, and any confidence caveats.

## Guardrails

- Treat this as a grounded weekly scaffold, not omniscient memory.
- Do not claim semantic certainty from sparse session snippets.
- Do not include usernames, home-directory paths, private repo paths, tokens, emails, or account identifiers.
- If a section matters, deepen it by reading the named sessions before making decisions.
- Keep the report local unless the user explicitly asks to publish or share it.
