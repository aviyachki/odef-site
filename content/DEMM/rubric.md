+++
title = "Assessment Rubric"
weight = 17
draft = false
description = "Concrete criteria for placing an organization at a DEMM level, per dimension."
+++

## How to use the rubric

Score each dimension independently on the criteria below. An organization is at a level in a dimension when it meets **all** criteria for that level and every level below it. Partial credit is not awarded; the point of the rubric is that two teams scoring the same organization arrive at the same answer.

The overall level is the **lowest** dimension score. A program with excellent content and no data health is a Level 1 program with good content.

Record the evidence for each criterion. "We have a coverage map" is a claim; a link to the map, with its last-updated date, is evidence.

## Threat Detection Content

| | Level 1 Partial | Level 2 Adequate | Level 3 Enabled |
|---|---|---|---|
| **Source of detections** | Vendor-provided only | Vendor plus in-house for known gaps | In-house covers the threat model where vendors do not; vendor coverage is assessed, not assumed |
| **Threat model** | None written down | Exists, reviewed less than yearly | Maintained, reviewed at least every 6 months, drives the backlog |
| **Idea sources** | Threat intel and vendor feeds only | Plus incident reviews | Plus intake from across the company; idea source mix is measured |
| **Detection types** | Alerts only | Indicator and behavioural, handled the same way | Indicator, behavioural, correlation, and hunts, each with its own lifecycle path |
| **Ownership** | Nobody | Author is implicit owner | Every detection has a named current owner; unowned detections are flagged |

## Assurance

| | Level 1 Partial | Level 2 Adequate | Level 3 Enabled |
|---|---|---|---|
| **Validation at build** | None, or manual syntax check | True-positive validation on every detection before deploy | Plus false-positive validation against production data and unit tests in CI |
| **Continuous validation** | None | Periodic manual re-test | Automated canary per detection; canary pass rate is tracked |
| **Adversary emulation** | None | Ad hoc red team findings feed detections | Scheduled emulation of threat-model techniques; time to detect is measured on every run |
| **Coverage map** | None | Presence per technique | Confidence per technique, updated on every Midday and Sunset transition |
| **Metrics** | Alert counts | Precision and volume per detection | Full set with definitions and targets; volume ceiling per analyst is enforced |

## Knowledge Sharing

| | Level 1 Partial | Level 2 Adequate | Level 3 Enabled |
|---|---|---|---|
| **Documentation** | Ad hoc, scattered | Central repository exists; coverage incomplete | Every active detection has a current document and runbook; staleness is reviewed |
| **Runbooks** | None | Exist for some detections | Exist for every detection and are written for the responder, not the author |
| **Socialization** | None | Manual announcement of new detections | Automated notification on deploy and sunset, to the teams that respond |
| **Dependency awareness** | Data changes break detections silently | Dependency document exists | Source-to-detection map is used by data owners before changes and by health monitoring after |
| **Feedback** | Analysts have no channel back | Feedback is informal | Disposition and reason recorded on every alert; owner reviews weekly |

## Data and Visibility

| | Level 1 Partial | Level 2 Adequate | Level 3 Enabled |
|---|---|---|---|
| **Source inventory** | Unknown what is collected | List of sources exists | Data dictionary with schema, owner, retention, and the detections depending on each source |
| **Onboarding** | Sources appear when a detection needs them | Onboarding process exists | Onboarding is prioritized from the coverage map, not from the next detection |
| **Health monitoring** | None | Volume alerts on major sources | Volume and schema drift monitoring per source, routed to the owners of dependent detections |
| **Retention** | Unknown or inconsistent | Known per source | Set per source against stated detection and investigation needs |
| **Improvement initiatives** | Gaps are tolerated | Gaps are raised informally | Visibility gaps become tracked initiatives with owners and deadlines |

## Automation (Detection as Code)

| | Level 1 Partial | Level 2 Adequate | Level 3 Enabled |
|---|---|---|---|
| **Storage** | Detections live in the tool's UI | Exported to a repository after the fact | Repository is the source of truth; the tool is deployed to |
| **Review** | None | Informal | Every change is a pull request reviewed by someone other than the author |
| **Testing** | None | Syntax validation in CI | Schema validation, unit tests, and true-positive replay in CI; failing tests block deploy |
| **Deployment** | Manual, in the UI | Scripted | Automated on merge, with a staging environment and a documented rollback |
| **Status and lifecycle** | Enabled or disabled | Status field exists | Status field drives behaviour: Sunrise deploys to staging, Midday to production, Sunset disables; exceptions carry expiries that CI enforces |

## Recording the result

Produce a one-page summary per assessment: the date, the score per dimension, the lowest-scoring criterion in each, and the evidence links. The lowest-scoring criteria are the candidates for the next [security improvement initiatives]({{% ref "operational-maturity" %}}). Reassess on the cadence set in the [maturity review process]({{% ref "operational-maturity" %}}).
