+++
title = "Maturity Model"
weight = 15
draft = false
+++


## Introduction

Maturity is a self-evaluation process conducted by the team. ODEF provides guidance and structure and assures that all the relevant areas are covered. The goal of the review process is to give a baseline that helps achieving a common understanding about the organization security posture.

### Dimensions

* Threat Detection Content
* Assurance
* Knowledge sharing
* Data and Visibility
* Automation (Detection as Code)

The first three dimensions are described below. The two added dimensions, and concrete criteria for scoring every dimension, are in the **[Assessment Rubric]({{% ref "rubric" %}})**. Use the rubric to place the organization; use this page to understand what each level feels like.

---
{{<mermaid align="left">}}

flowchart RL
    Assurance(Assurance) <--->Threat[Threat Detection Content]
    Knowledge[Knowledge sharing] <---> Assurance(Assurance)
    Knowledge[Knowledge sharing] <---> Threat(Threat Detection Content)
{{< /mermaid >}}

---

## Maturity levels

![demm](/images/demm.png)

### Level 1 - **Partial**

* Threat Detection Content
  * Organizational threat identification practices rely solely on external vendors to provide security content.
  * Assurance and context around alerts and detections is not provided or sufficient.
  * Risk is managed in an ad hoc and often reactive manner by relying on third parties.
* Assurance
  * There is some limited awareness of cybersecurity threat detection capabilities at the organizational level.
  * The organization implements threat validation and verification on an irregular, case-by-case basis due to varied experience or information gained from outside sources.
  * Assurance through continuous validation is not present.
* Knowledge sharing
  * The organization may not have processes to enable cybersecurity information sharing.
  * Documentation is rarely written and shared only on ad-hoc basis and it is scattered across teams.

### Level 2 - **Adequate**

* Threat Detection Content
  * Organizational threat identification practices rely on internal teams and external vendors to provide security content.
  * Context around alerts and detections is provided. Specialized teams are able to introduce new detections and security content.
  * Some security teams have a better understanding of security posture than others.

- Assurance
  * There is some awareness of cybersecurity threat detection capabilities as the organization is now building custom detections to compensate for gaps.
  * The custom detections are use case driven and validated during the detection development process.
  * Continuous validation is not enabled and the organization still relies on suppliers for most of the detection capabilities.
* Knowledge sharing
  * The organization is starting to enable knowledge sharing and promotes documentation efforts.
  * There is a central detection information repository.

### Level 3 - **Enabled (Proactive)**

* Threat Detection Content
  * Organization maintains continuous practices that provide excellent internal insights and knowledge. Context around alerts and detections is provided.
  * Any team is encouraged and capable to introduce new detection components and thus improve the security posture.
  * The security posture of the environment is well understood across the security teams.

- Assurance
  * The organization possesses a detection coverage map with a stated confidence per technique, and in-house detections cover the techniques in its threat model that vendor controls do not. Vendor detections are a known, assessed layer rather than an unknown one; the organization knows what they cover and what they miss.
  * Automation is provided to continuously validate and run the detection use cases.
  * Additional assurance is achieved by running red team exercises and automation frameworks.
* Knowledge sharing
  * Organizations possess practices to create and maintain high quality records and appropriately control and manage the access to the information.
  * Processes for socializing detections are automated and teams are informed of the development of new detections.
