+++
title = "Sunset"
weight = 23
draft = false
+++

### Phase 3️⃣ Sunset 🌆

<p align="justify">
During the “Sunset” phase the detection is taken out of commission. The phase wants to ensure that resources are not spent for outdated detections that are no longer applicable and at the same time leave sufficient trace of the existence of the detection.
</p>

High level goals for the Sunset phase:
<ul>
  <li>Decommission the detection and leave it in a state that it can be resumed anytime</li>
  <li>Preserve knowledge</li>
</ul>

<table>
<thead>
  <tr>
    <th>Function</th>
    <th>Activity</th>
    <th>Description</th>
    <th>Guidelines</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td rowspan="2"><b>Decommission</b></td>
    <td>Decommission the detection</td>
    <td>The goal is to decommission the detection by following process that provides visibility </td>
    <td>The <b>owner</b> makes the Sunset decision, against the triggers listed in the Midday Review activity, and records the reason in the detection's YAML file. To decommission, change the status field to "Sunset". Assuming your pipeline is configured correctly, this disables the detection and prevents it from running.
    <br>Note: Do not remove anything from the repository as detections can be reused in future. Remove the detection's exceptions, though; they should not outlive it. </td>
  </tr>
  <tr>
    <td>Knowledge base update</td>
    <td>Create an adequate indication in the KB document that the detection is no longer active and socialize the change with your security teams.</td>
    <td>Update the coverage map by removing the coverage that the detection was providing. If this leaves a technique in the threat model uncovered, that is a new opportunity for the <a href="/odef/strategy/">strategy backlog</a>, not a silent gap. Notify the responding team so runbooks that reference the detection are retired too.</td>
  </tr>
</tbody>
</table>

### Sunset Process Flow

{{<mermaid align="center">}}
graph TD;
Review1[Review completed] --> Review2;
Review2{detection ready to decom} -->|no| End[end];
Review2{detection ready to decom} -->|yes| Preserve(Preserve knowledge);
Preserve --> Decommission(Decommission the detection);
{{< /mermaid >}}
