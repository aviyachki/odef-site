+++
title = "Lessons from the Field"
weight = 25
draft = true
description = "What running ODEF in production for several years taught us about the framework itself."
+++

## Why this page exists

ODEF was written in 2023 as a model of how detection engineering *should* work. Since then it has run as the backbone of a production detection platform for several years. This page is the honest record of where the model held, where it bent, and where practice quietly replaced it.

Everything here is methodology. No vendor, product, or employer specifics.

<!-- EDITOR NOTES: each section below has QUESTIONS (for you to answer) and
     CANDIDATE lessons (my inference from the existing wiki text; confirm, edit, or delete).
     Delete this comment block and the question lists before publishing. -->

## Detection ideas come from anywhere

When ODEF was written, Opportunity Identification had a tidy list of inputs: threat intelligence reports, OSINT, internal knowledge of a gap. The implied picture was a detection engineer at a desk, reading, deciding what to build next. That is how we expected the backlog to fill.

It is not how the backlog filled.

The best detection ideas arrived sideways. A platform engineer mentioning, in passing, that a certain admin action should never happen outside a deploy window. Someone noticing the same odd account pattern twice in a week. An incident responder closing a ticket with "we only caught this because someone happened to look." None of these people thought of themselves as giving us a detection. They were describing their world, and in their world the abnormal thing was obvious because they lived in the normal.

So we stopped waiting for ideas to reach us and built ways to catch them where they already were. Over time that became four intakes running side by side:

* **A bot that listens.** It watches a chat channel for conversations that look like detection opportunities and surfaces them to the team. Most of the signal was already in chat. People were describing suspicious behaviour to each other every day. The bot's job was simply to make sure the detection team heard it too.
* **A service desk board.** A request type on the company's service management board where anyone can file a detection idea or a "would you notice if..." question. It gives ideas a ticket, an owner, and a status the submitter can see.
* **A standing relationship with incident response.** Not a channel, a habit. Every incident and every near miss is a question: what would have caught this earlier, and what did we only see because someone was paying attention? Responders are the richest source of detection ideas in the company and they rarely file tickets about it unless asked.
* **The rest of the company, on purpose.** We told people, repeatedly, that noticing something odd and saying so was useful even if it turned out to be nothing. The bar to raise something is near zero and nobody needs to know what a TTP is.

The lesson is that the detection engineer is rarely the person with the best model of what normal looks like in any given system. The people who run the system are. A framework that treats research as something the security team does alone will miss most of the ideas that matter, because those ideas live in other people's heads and only come out when someone is listening.

**What this changes in ODEF.** Opportunity Identification is an intake function, not a research step that happens to have inputs. Concretely:

* Run more than one intake. Passive listening and active filing catch different people. The bot catches the ones who would never file a ticket; the board catches the ones who want to be sure it was heard.
* Treat every submission as a research trigger, not a request. Most will not become detections. All of them tell you something about where visibility is thin.
* Close the loop. Tell the person what happened with their idea, especially when it shipped. That is what keeps the next idea coming.
* Count where ideas come from. It is a maturity signal. If every detection traces back to a threat intel feed, the organization is still at the "security team alone" level no matter what else it has built.

## Indicators of compromise still work

ODEF, like most of the field in 2023, was built around behaviour. Every function in Sunrise assumes you are researching a technique, mapping it to ATT&CK, understanding the technology, and writing logic that catches the behaviour regardless of which tool the attacker used. The Pyramid of Pain had become orthodoxy: hashes, IPs, and domains are the cheap layer at the bottom, trivially changed, barely worth the effort. We believed that. The framework does not even have a function for indicator matching.

Then we ran it for a few years and kept noticing what actually fired.

Plain indicator matching kept catching real things. A known-bad domain in DNS logs. A hash from a vendor report showing up on an endpoint weeks later. An IP from an incident write-up reappearing in a different part of the environment. The behavioural detections were doing the harder, more durable work, but the indicator feed was quietly producing true positives at a cost that rounded to zero per detection.

In hindsight the reason is obvious. The Pyramid of Pain is about the attacker's cost to change an indicator. It says nothing about whether they bother. Most of what reaches a given organization is not a targeted adversary carefully rotating infrastructure. It is commodity tooling, reused infrastructure, and campaigns that run for weeks on the same handful of domains because changing them has not been necessary yet. For that traffic, an indicator is not weak evidence. It is a confession.

<!-- OPTIONAL: one concrete, vendor-neutral example of what IOC matching caught that
     nothing else would have. Which indicator type pulled its weight most: domains, hashes, IPs? -->

There is also a practical argument the framework missed. An indicator detection is the only kind you can build in minutes, with no research phase, no technical context, and no baseline. When a report lands at four in the afternoon, "are any of these in our logs, now and for the last ninety days" is the question that matters first. Behavioural coverage for the technique can follow. It usually should. But it is a different kind of work on a different timescale, and the framework conflated the two.

**What this changes in ODEF.** Indicator matching deserves to be a first-class detection type with its own lightweight path through the lifecycle:

* Sunrise for an indicator detection is intake, dedupe, and a retroactive search. It should take minutes, not days. Forcing it through the full research and documentation functions is how indicator work silently stops happening.
* Keep indicators as data, not as detection logic. One generic matching detection fed by a curated list beats a hundred hand-written rules, and it makes expiry and provenance tractable.
* Give indicators a lifespan. Midday for indicators is mostly ageing out stale ones so the list stays fast and the hits stay meaningful. Record where each came from and when.
* Measure the two types separately. Indicator hits and behavioural hits tell you different things. Mixing them in one true positive count hides the fact that the cheap layer is carrying more than its share.
* Stop treating the Pyramid of Pain as a priority order. It describes attacker cost, not defender value. Low on the pyramid is often where the highest return per hour of engineering sits.

## The lifecycle held, the phases did not weigh the same

<!-- QUESTIONS
- What share of engineering time actually went to Sunrise vs Midday vs Sunset?
- Did the "Midday is the longest phase" claim hold? Was it also the most expensive?
- Were there functions nobody ever did? Which ones got skipped first under pressure?
-->

<!-- CANDIDATE: Sunset collapsed into a status flip. The Sunset page already says
     decommissioning is "change the status field to Sunset". Was the knowledge-preservation
     half of Sunset ever done as a separate step, or did the KB document just go stale? -->

## Sunrise: research is where fidelity is decided

<!-- QUESTIONS
- Of the six Sunrise functions (Research, Prepare, Build, Validate, Automate, Share),
  which one most predicted whether a detection survived a year?
- Did "Develop Research Questions" survive as a real step, or did it fold into Technical Context?
- How often did the Visibility Check fail and spawn a logging improvement initiative?
  Did that loop (Prepare -> Improve -> back to Build) actually work, or did detections ship with known blind spots?
- Baselines: did they grow without bound? Who owned pruning them?
-->

<!-- CANDIDATE: The data dictionary paid for itself. The Prepare function says to grow a
     data dictionary "to quickly refer to". Did it exist, and was it the asset people reached for? -->

## Build: unit tests found the wrong bugs

<!-- QUESTIONS
- The wiki lists three unittest goals: missing data, syntax errors, true-positive confirmation.
  Which of the three caught real problems? Which was theatre?
- Did "true positive validation" via emulation happen, or was it almost always a historical event?
- What did the CI pipeline actually gate on by the end?
-->

## Midday: the metrics that mattered

<!-- QUESTIONS
- The Measure function proposes ATT&CK coverage %, automation success/failure, runtime length.
  Which did you keep? Which did you stop looking at?
- What was the real signal that a detection needed work: analyst complaints, FP volume, runtime, something else?
- Did periodic review happen on a schedule, or only when something broke?
-->

<!-- CANDIDATE: Runtime was the best health metric. The Midday page already calls out
     query runtime as a proxy for poorly written logic. Was that true in practice? -->

## Share: the dependency tree and the notification process

<!-- QUESTIONS
- The "Sec Dependency Tree" document: did it exist, and did anyone outside security consult it
  before renaming an index?
- What form did "socialize the detection" take, and did anyone read it?
-->

## Detection as Code: what the YAML schema got wrong

<!-- QUESTIONS
- Which fields in the published YAML template were never used?
- Which fields were missing and got added in the first six months?
- Did a single file per detection scale, or did you need a different layout?
- Was the Markdown README per detection maintained, or did the YAML become the only source of truth?
-->

## Maturity model: was self-assessment honest?

<!-- QUESTIONS
- Did the three DEMM levels (Partial, Adequate, Enabled) ever get formally assessed?
- Did the Maturity Review Process (Collect, Analyze, Prioritize, Improve) run on a cadence?
- What would you change about the levels now?
-->

## What we would change in ODEF today

<!-- Summarize into a short list once the sections above are filled. Each item should
     map to a concrete edit elsewhere on the wiki so this page drives the reboot. -->
