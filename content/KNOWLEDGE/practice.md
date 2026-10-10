+++
title = "Knowledge Practice"
weight = 18
draft = false
description = "How the detection knowledge base is organized, kept current, and actually read."
+++

## The knowledge base is a product with users

The documentation template on the next page describes what one detection's document contains. This page describes the practice around all of them: who the documents are for, how they are found, and how they stay true.

The primary reader of a detection document is not the detection engineer. It is the analyst who has just received an alert from it at two in the morning and needs, in thirty seconds, to know what it means and what to do. Every decision about the knowledge base follows from that.

## Structure

* **One directory per detection**, named by its `id`, containing the YAML file, the Markdown document, and the tests. The repository layout is the knowledge base; there is no second copy.
* **The runbook is a section of the document**, not a separate system. It is the section the analyst reads first, so it goes first.
* **Program-level documents live alongside**: the data dictionary, the dependency map, the threat model, the coverage map, and the current DEMM assessment. Each has an owner and a last-reviewed date in its front matter.
* **Index by what responders search for**: technique, data source, affected system, alert name. Nobody searches by detection id.

## Keeping it true

* The document is reviewed whenever the detection is: at the Midday review cadence, and on every logic change. A pull request that changes `query` without touching the document is sent back.
* Analyst dispositions on alerts are the staleness signal. Three "insufficient data" closes mean the runbook is wrong, whatever the document says.
* Sunset detections keep their documents, marked clearly as retired at the top, because "we used to detect this and stopped because X" is knowledge.
* Staleness is measured: fraction of active detections whose document was reviewed within cadence. It is a DEMM criterion.

## Access

Controlled, but not restrictive. The responding teams, the detection team, incident response, and the owners of monitored systems need read access; the dependency map in particular must be readable by the data and platform engineers who are expected to consult it. Write access goes through pull request review, which is the access control that matters.

## Socialization

Every deploy and every sunset produces an automated notification to the responding teams, with the alert name, the one-line goal, and a link to the runbook. Periodic human-written summaries, monthly or quarterly, cover what changed in coverage and why. The notification is how analysts learn a detection exists; the summary is how the rest of the security organization learns what the program is doing.
