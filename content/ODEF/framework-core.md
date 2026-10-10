+++
title = "Framework Core"
weight = 19
draft = false
+++


## Framework Core

The Framework Core is a suite of activities aimed at achieving specific cybersecurity objectives, supplemented by illustrative examples to guide their implementation. The Core is structured around three lifecycle phases: *Sunrise*, *Midday*, and *Sunset* which holistically cover the detection lifecycle from inception to completion, with dedicated functions, guidelines, and goals for each phase. Together, these components provide detection engineers with a "north star" focus, enabling them to deliver high-quality detections with precision.

### Phases, Functions, Activities

ODEF uses three levels of structure. Keeping the names straight matters, because the same word used for two levels makes the framework impossible to follow.

#### **Phases**

A phase is a stage in a detection's life: Sunrise, Midday, Sunset. Every detection is in exactly one phase at any time, and the phase is recorded as the detection's status.

#### **Functions**

A function is a group of related work inside a phase, with a single purpose. Sunrise has six: Research, Prepare, Build, Validate, Automate, Share. Inspired by the single-responsibility principle, each function answers one question about the detection, for example "is there enough visibility to build this?" or "does it actually fire?".

#### **Activities**

An activity is a concrete step inside a function, with a stated goal and a description of how to do it. "Visibility Check" is an activity in the Prepare function. Activities are the level at which work is tracked and at which a team decides what to skip for a given detection type.

#### **Guidelines**

Guidelines are reference material attached to an activity: context, examples, and local practice that help achieve the activity's goal. A document describing a company's change management process is a guideline. Guidelines are where an organization adapts ODEF without changing its structure.

#### **Ownership**

Every detection has an **owner**: the person accountable for it through all three phases. The owner is named in the detection's YAML file, approves exceptions to it, and makes the Sunset decision. Authorship is historical; ownership is current. A detection whose owner has left the team is, by definition, unowned and goes to the top of the next review.

### Phases in detail

The Core encompasses three primary lifecycle phases:

* **[Sunrise]({{% ref "Sunrise" %}})**
* **[Midday]({{% ref "Midday" %}})**
* **[Sunset]({{% ref "Sunset" %}})**

These phases collectively chronicle the lifespan of a detection mechanism, from its conception to its eventual retirement/decommissioning.
