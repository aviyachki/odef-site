+++
title = "The yaml file"
chapter = false
weight = 32
draft = false
+++

### Yaml file purpose

The YAML file is the machine-readable record of a detection. It is the source of truth that the pipeline deploys from, that health monitoring reads dependencies from, and that the lifecycle status lives in. One file per detection. The Markdown document next to it carries the human narrative; the YAML carries everything a system needs.

### Reference schema

```yaml
# Identity
id: "det-0001"                       # stable, never reused, referenced by alerts and exceptions
name: "{{ detection_name }}"
version: 3                           # increment on any logic change; CI enforces
type: behavioural                    # indicator | behavioural | correlation | anomaly | hunt

# Lifecycle
status: midday                       # sunrise | midday | sunset  (drives deployment behaviour)
owner: "{{ team_or_person }}"        # current accountable owner; author is in git history
created_date: "{{ created_date }}"
last_reviewed_date: ""               # set by the owner at each review; CI warns past the cadence
review_cadence_days: 180             # 90 for anomaly and correlation
sunset_reason: ""                    # required when status is sunset

# Origin and coverage
origin: intake                       # intake | incident | threat_intel | coverage_gap | hunt
references:
  - "https://..."                    # reports, write-ups, internal tickets
mitre:
  - tactic: "{{ tactic }}"
    technique: "{{ mitre_id }}"      # one entry per technique; a detection may cover several
    confidence: medium               # low | medium | high  (used in the coverage map)

# Data
data_sources:
  - name: "{{ data_source }}"        # must match the data dictionary
    location: "{{ data_location }}"  # index, table, bucket
    fields: ["user", "src_ip"]       # fields the logic depends on; drift monitoring watches these

# Logic
platform: "{{ siem_or_engine }}"     # the query dialect
query: |
  {{ query }}
schedule: "{{ schedule }}"           # cron or interval; omit for streaming
lookback: "15m"
event_limit: 0
baseline: |                          # known-good patterns excluded by the logic itself
  {{ baseline }}
enrichment:
  - source: "{{ enrichment_source }}"
    join_on: "user"

# Exceptions (owned, justified, expiring suppressions; distinct from baseline)
exceptions:
  - id: "exc-0001"
    pattern: "src_ip = 10.0.0.5"
    reason: "vulnerability scanner, ticket SEC-123"
    requested_by: "{{ team }}"
    approved_by: "{{ owner }}"
    expires: "2026-12-31"            # CI fails on expired exceptions still present

# Alert
alert:
  severity: 3                        # 0 unknown, 0.5 informational, 1 low, 2 medium, 3 high, 4 critical (impact)
  priority: high                     # high | medium | low (response capacity)
  name: "{{ alert_title }}"
  description: "This detection is monitoring for changes in any of X"
  payload_fields: ["user", "src_ip", "action", "evidence"]   # what the analyst sees first
  runbook: "./README.md#runbook"
  sla_minutes: 1440
  expected_volume_per_day: 2         # breaching 5x this triggers review

# Validation
tests:
  unit: "./tests/"                   # syntax, schema, missing-field tests
  true_positive:
    - kind: historical               # historical | emulation | synthetic
      reference: "incident INC-42, 2025-03-04"
  canary:
    kind: synthetic                  # how liveness is proven in Midday
    schedule: "daily"
```

### Field notes

**Identity and version.** The `id` is what alerts, exceptions, and the coverage map point at, so it never changes and is never reused. `version` is bumped on any change to `query`, `baseline`, or `data_sources`; the pipeline refuses a logic change without a version bump.

**Status drives the pipeline.** `sunrise` deploys to a staging or audit-only target, `midday` to production, `sunset` disables and keeps the file. Changing status is the only way to move between phases, which is what makes the lifecycle enforceable rather than aspirational.

**Owner, not author.** Git history knows who wrote it. The file records who is accountable for it now. CI can flag detections whose owner no longer exists in the team directory.

**Baseline versus exceptions.** A baseline is part of the detection logic: behaviour that is normal everywhere and would never be an alert. An exception is a specific, approved, expiring suppression requested by a responding team. Keeping them apart is what stops a detection from being hollowed out one "just ignore that host" at a time.

**Fields the logic depends on.** Listing `fields` per data source lets drift monitoring alert the owner when a field goes empty or is renamed, before the detection silently returns nothing.

**Severity and priority.** Severity is impact: what the attacker has if this is real. Priority is response: how fast the team must act given everything else it is handling. They are related but not the same field, and setting priority is a conversation with the responding team.

### What CI should check

* The file validates against the schema.
* `version` increased if logic changed.
* No exception is past its `expires` date.
* `last_reviewed_date` is within `review_cadence_days`, or the owner is warned.
* Every `data_sources.name` exists in the data dictionary.
* Every `mitre.technique` is a valid ATT&CK id.
* The `tests.unit` directory exists and its tests pass.
* A change to `query` is accompanied by a pull request reviewed by someone other than the author.
