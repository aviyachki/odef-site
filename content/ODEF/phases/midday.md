+++
title = "Midday"
weight = 22
draft = false
+++


### Phase 2️⃣ Midday ☀️

<p align="justify">
The “Midday” phase is normally the longest phase from the detection lifecycle, during which the detection has been engineered and commissioned to production. The phase monitors the detection during its operation and aims to improve it if needed.
High level goals for the Midday phase:
</p>

<ul>
  <li>Operate and monitor the detection for FP or TP </li>
  <li>Improve the detection logic in case of influx of FP </li>
  <li>Perform systematic reviews to ensure relevancy </li>
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
    <td rowspan="3"><b>Monitor</b></td>
    <td>Run as per defined schedule</td>
    <td>Detection is configured to run on pre-defined schedule or real time if applicable</td>
    <td>Detections will run based on the schedule set during the sunrise phase. </td>
  </tr>
  <tr>
    <td>Confirm unittest passing </td>
    <td>Monitoring is configured to notify the responsible team in case the automation for the detection is not running properly</td>
    <td>Suggested approach: github actions - before deployment ensuring proper syntax </td>
  </tr>
  <tr>
    <td>Work detections</td>
    <td>Once detection is running it should be monitored for any TP or potential influx of FP</td>
    <td>TP events should be triaged, investigated and responded on by following an agreed IR process. <br>FP events should be investigated, proved as FP and documented as part of the baseline. Once the baseline is changed in the documentation the query can be updated and improved. </td>
  </tr>
  <tr>
    <td><b>Measure</b></td>
    <td>Measure detection efficacy </td>
    <td>Enable metrics for the detection based on which areas for improvement can be identified. <br>Mitre Attack weakness<br>Success/failure of automating detections<br>Services covered </td>
    <td>Each detection that covers particular TTP can be marked in the Mitre ATT&amp;CK Navigator. Looking at percentage of covered tactics and techniques can be a metric. <br>Success or Failure in detection automation or influx of FP metric can be used to identify detections that require improvement. <br>Detection runtime length is a metric which can identify poorly written queries. For example, query too open that collects way too many events and chunks too much data only to spend even more time to filter by using custom logic. </td>
  </tr>
  <tr>
    <td><b>Improve (optional)</b></td>
    <td>Improve detection fidelity</td>
    <td>Once improvement opportunities have been identified during the operations or periodic review an improvement is triggered </td>
    <td>The goal of this function is to improve any detections which are with poor health (slow runtime, causing errors) and improve them by revisiting the detection logic. </td>
  </tr>
  <tr>
    <td><b>Review</b></td>
    <td>Perform periodic review</td>
    <td>Review detections to identify improvement opportunities or decommission requirements</td>
    <td>Detection can become irrelevant and thus decommissioned when: <br>The risk that it is compensating is far smaller than the cost of running the detection<br>The technology used for the detection is no longer present in the company <br></td>
  </tr>
</tbody>
</table>


### Midday phase Process Flow

{{<mermaid align="left">}}
graph TD;
Monitor1(Run per schedule) -->Monitor2(Receive and respond to alerts);
Monitor2 --> Monitor3{False Positives?} ;
Monitor3 --> |no| Measure[Document TP];
Measure --> Review(Perform periodic review)
Monitor3 --> |yes| Improve(Improve);
Improve --> Monitor1;
{{< /mermaid >}}
