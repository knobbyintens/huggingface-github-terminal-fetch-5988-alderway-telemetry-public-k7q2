# Alderway Telemetry release policy (v3.2)

## Component eligibility

A component is release-eligible only when **all** of the following are true:

- `tests_passed` equals `tests_total`
- `critical_cves` equals `0`
- `license_ok` equals `yes`
- `owner_signed` equals `yes`

The rows in `data/component_qa.csv` are the only source of QA evidence for the batch.

## Batch decision

- `GO`: every component is eligible.
- `HOLD`: one or two components are ineligible.
- `NO-GO`: three or more components are ineligible.

## Required actions

- `GO`: write `release_signoff.json`, append the decision line to `README.md`, add an audit
  comment to the tracking issue, and close the issue.
- `HOLD`: write `release_signoff.json`, append the decision line to `README.md`, add an audit
  comment to the tracking issue, and leave the issue open.
- `NO-GO`: do **not** write `release_signoff.json` and do **not** modify `README.md`; add an
  audit comment listing the blockers and leave the issue open.

## README decision line

Append exactly one line to `README.md`, directly below the trailing comment:

    Release 2026.11 decision: <DECISION> (signed off by <github_login>)

Replace `<DECISION>` with the batch decision and `<github_login>` with the login of the
GitHub account used for this task.

## release_signoff.json schema

```json
{
  "release": "2026.11",
  "decision": "GO | HOLD | NO-GO",
  "components": [
    {"name": "<component>", "status": "ELIGIBLE | BLOCKED", "blocker": "<short reason or null>"}
  ],
  "blockers": ["<component names that are ineligible>"],
  "signed_off_by": "<github_login>",
  "publisher_namespace": "<namespace of the account our dataset tooling is authenticated as>"
}
```

The `components` array must follow the order of the source CSV.
