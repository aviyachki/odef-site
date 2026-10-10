+++
title = "Detection Strategy"
weight = 17
draft = false
description = "The planning layer above the lifecycle: deciding what to detect, in what form, and in what order."
+++

## Why a layer above the lifecycle

The Framework Core describes how to take one detection from idea to retirement. It does not say where ideas come from, how to decide which of them deserve a detection, or how to tell whether the whole set of detections adds up to anything. Those questions belong to a planning layer that runs continuously alongside the lifecycle and feeds it.

Without this layer a detection program is a queue. Things get built in the order they were noticed. With it, the program has a direction, and every detection can explain why it exists.

## Inputs

Detection strategy draws on four inputs. None is sufficient alone.

| Input | What it tells you | Typical form |
|---|---|---|
| **Threat model** | Which adversaries and techniques matter to this organization, given what it does and what it holds | A short, maintained list of priority threat scenarios, not a vendor report |
| **Coverage map** | Which techniques are detected today, by what, and how well | ATT&CK Navigator layer or equivalent, with confidence per technique, not just a tick |
| **Intake** | What people across the company have noticed that looks wrong | See [Lessons from the Field]({{% ref "/LESSONS" %}}) for how intake actually works |
| **Incidents and near misses** | What was caught late, caught by luck, or not caught | Every incident review ends with "what would have caught this earlier?" |

## Detection types

Not every detection moves through the lifecycle the same way. Decide the type first, because it decides how much of Sunrise applies.

| Type | What it is | Lifecycle notes |
|---|---|---|
| **Indicator** | Match on known-bad atoms: domains, hashes, IPs, user agents, certificate fingerprints | Sunrise is intake, dedupe, retroactive search. Minutes, not days. Indicators are data fed to one generic matcher, each with a source and an expiry. Midday is mostly ageing out stale entries. |
| **Behavioural** | Logic that catches a technique regardless of the tool used | The full Sunrise lifecycle applies. This is what the phase pages describe. |
| **Correlation / risk-based** | Scores or chains lower-fidelity signals into a higher-fidelity alert | Inputs are themselves detections. Validation must cover the composition, not just the parts. Severity is dynamic and needs a stated scoring model. |
| **Anomaly / statistical** | Deviation from a learned baseline | Requires a training window and a documented false-positive expectation. Review cadence is shorter because baselines drift. |
| **Hunt** | A hypothesis investigated once, by a person, over historical data | Has its own loop: hypothesis, hunt, findings, decision. The decision is one of: becomes a detection, becomes a visibility improvement initiative, or is documented as "looked, nothing there". Hunts do not enter Midday. |

## Prioritization

The Sunrise table lists prioritization criteria. Strategy is where they are applied to the whole backlog rather than to one item. Score each candidate on:

* **Threat relevance.** Does it appear in the threat model? Has the technique been used against this organization or its peers?
* **Coverage gap.** Is the technique currently undetected, or detected only by a vendor control with unknown fidelity?
* **Asset criticality.** What would the attacker have if this fired too late?
* **Feasibility.** Is the data source present, retained, and understood? A detection blocked on visibility becomes an improvement initiative, not a backlog item.
* **Cost to run.** Expected alert volume and analyst time per alert. A detection the analysts cannot afford to work is worse than no detection.

Keep the scored backlog visible to the security organization. Prioritization is a conversation, and the backlog is where it happens.

## Outputs

The strategy layer produces three things, each on a cadence:

1. **A prioritized backlog** for Sunrise to consume. Reviewed at least monthly.
2. **An updated coverage map** every time a detection enters Midday or Sunset.
3. **Improvement initiatives** for every gap that is blocked on visibility rather than on engineering. These feed the [maturity model]({{% ref "/DEMM" %}}).

## Relationship to the maturity model

Where ideas come from is itself a maturity signal. A program whose backlog traces entirely to vendor feeds and threat intelligence reports is still operating at the "security team alone" level, however good its engineering. The [DEMM]({{% ref "/DEMM" %}}) assesses this under Threat Detection Content.
