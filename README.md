# Shareable Insight Skill

This package contains a privacy-conscious `insight` skill plus optional slash-command cards for:

- `/insight`
- `/insights`
- `/weekly_insight`
- `/weekly_insights`

It generates polished local HTML reports from current-work evidence, explicit report JSON, or JSONL session logs.

## Privacy Posture

This shareable copy is sanitized:

- No user home-directory paths are embedded.
- No private project names from the source machine are embedded.
- Weekly reports redact home paths, emails, and common secret-looking assignments.
- Weekly reports use coarse work categories by default.
- Generated HTML stays local unless the user explicitly chooses to share it.

Still treat session logs as sensitive. If you plan to publish a generated report, review it first.

## Install

Copy the `insight/` folder into your agent skill directory.

For Codex-style command cards, copy the files in `commands/` into your command directory.

Example layout:

```text
skills/
  insight/
    SKILL.md
    references/report_schema.md
    scripts/render_insight_report.py
    scripts/build_weekly_insight_report.py
commands/
  insight.md
  insights.md
  weekly_insight.md
  weekly_insights.md
```

## Focused Report

Create JSON using `insight/references/report_schema.md`, then run from the installed skill directory:

```bash
python3 scripts/render_insight_report.py /tmp/agent-insight-report.json -o /tmp/agent-insight-report.html
```

## Weekly Report

The weekly builder expects session logs arranged as:

```text
sessions-root/
  YYYY/
    MM/
      DD/
        session.jsonl
```

Run from the installed skill directory:

```bash
python3 scripts/build_weekly_insight_report.py --sessions-root /path/to/sessions
```

For explicit ranges:

```bash
python3 scripts/build_weekly_insight_report.py --sessions-root /path/to/sessions --start 2026-05-10 --end 2026-05-16
```

Outputs default to:

```text
/tmp/agent-weekly-insight-report.json
/tmp/agent-weekly-insight-report.html
```

## Test

From this package root:

```bash
python3 tests/test_shareable_insight.py
```

The test creates synthetic session logs, runs the weekly builder, renders HTML, and checks that personal source-machine strings are not present.
