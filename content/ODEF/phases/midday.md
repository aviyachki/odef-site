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
  <li>Keep the detection healthy: it runs, its data is still there, and it still fires </li>
  <li>Feed analyst judgement back into the detection </li>
  <li>Improve the detection logic in case of influx of FP </li>
  <li>Perform systematic reviews on a stated cadence to ensure relevancy </li>
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
    <td rowspan="4"><b>Monitor</b></td>
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
    <td>Triage Feedback</td>
    <td>Analyst judgement is captured on every alert and flows back to the detection owner</td>
    <td>Each closed alert carries a disposition (true positive, benign true positive, false positive, insufficient data) and a free-text reason. The owner reviews dispositions weekly. Three "insufficient data" closes in a row is a payload problem, not an analyst problem. A benign true positive that repeats is an exception candidate, not a baseline edit. </td>
  </tr>
  <tr>
    <td rowspan="3"><b>Health</b></td>
    <td>Liveness</td>
    <td>Every detection proves it can still fire</td>
    <td>Automate a canary per detection: a synthetic event, a replayed historical true positive, or at minimum a check that the query returns rows against a known window. A detection that has not fired in 30 days is either covering a rare technique or silently broken; the canary tells you which. </td>
  </tr>
  <tr>
    <td>Data Drift</td>
    <td>The data the detection depends on is still arriving in the expected shape</td>
    <td>Monitor volume and schema per log source, not per detection. Alert the detection owner when a source drops below its usual volume, a field the detection filters on goes empty, or a field is renamed. The <b>Sec Dependency Tree</b> from Sunrise maps sources to detections so one source alarm fans out to the right owners. </td>
  </tr>
  <tr>
    <td>Exceptions</td>
    <td>Every suppression is owned, justified, and expires</td>
    <td>An exception is a documented, approved reason that a specific pattern does not alert. It is distinct from a baseline, which is part of the detection logic. Exceptions are requested by the responding team, approved by the detection owner, recorded in the detection's YAML file with a reason and an expiry (default 90 days), and reviewed at expiry. Exceptions that nobody renews are removed. An unexpiring exception is a silent sunset. </td>
  </tr>
  <tr>
    <td><b>Measure</b></td>
    <td>Measure detection efficacy </td>
    <td>Enable metrics for the detection based on which areas for improvement can be identified. <br>Mitre Attack weakness<br>Success/failure of automating detections<br>Services covered </td>
    <td>Metrics are only useful with definitions. ODEF proposes these, per detection and in aggregate, with the ones every program should have first in bold:
      <ul>
        <li><b>Precision</b>: true positives (including benign true positives) divided by all alerts, over a rolling 30 days. The primary fidelity metric.</li>
        <li><b>Alert volume per analyst-day</b>: total alerts divided by analyst capacity. The primary cost metric. Set a ceiling and treat breaching it as an incident.</li>
        <li><b>Time to deploy</b>: from opportunity identification to Midday. Measures the lifecycle itself. Track separately for indicator and behavioural detections; they should differ by an order of magnitude.</li>
        <li><b>Coverage</b>: techniques in the threat model with at least one detection at stated confidence, as a fraction. Mark in ATT&amp;CK Navigator with confidence, not just presence. Vendor controls count, at their own stated confidence.</li>
        <li>Time to detect: from first malicious event to first alert, measured on incidents and on emulation runs.</li>
        <li>Canary pass rate: liveness checks passing, over all detections.</li>
        <li>Runtime: query execution time. A runtime that trends up without a data volume change is a logic problem.</li>
        <li>Exception count and age per detection. Growth here is the leading indicator of a detection that should be rewritten or sunset.</li>
        <li>Idea source mix: share of new detections by origin (intake, incident, threat intel, coverage gap). A maturity signal, see <a href="/odef/strategy/">Detection Strategy</a>.</li>
      </ul>
      Count indicator and behavioural hits separately. Mixing them hides which layer is doing the work.
    </td>
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
    <td><b>Cadence.</b> Every detection is reviewed at least every 6 months by its owner. Anomaly and correlation detections every 3 months, because their baselines and inputs drift. Any detection that breaches its alert volume ceiling, fails its canary, or has an expired exception is reviewed immediately. Unowned detections go to the top of the next review.<br><br>
    <b>Review questions.</b> Is the technique still in the threat model? Is the data source still present and healthy? Has precision stayed above the floor? Is the runbook still accurate? Would we build this today?<br><br>
    <b>Sunset triggers.</b> The risk being compensated is far smaller than the cost of running and working the detection. The technology it monitors is no longer present. It has been superseded by a higher-fidelity detection. Its owner has left and nobody will take it. </td>
  </tr>
</tbody>
</table>


### Midday phase Process Flow

{{<mermaid align="left">}}
graph TD;
Monitor1(Run per schedule) --> Health{Canary and data checks pass?};
Health --> |no| Fix(Owner fixes or sunsets);
Health --> |yes| Monitor2(Receive and respond to alerts);
Monitor2 --> Feedback(Analyst disposition recorded);
Feedback --> Monitor3{Disposition?};
Monitor3 --> |true positive| Measure[Document TP, update metrics];
Monitor3 --> |benign, repeats| Exception(Exception with owner and expiry);
Monitor3 --> |false positive| Improve(Improve logic or baseline);
Improve --> Monitor1;
Exception --> Monitor1;
Measure --> Review(Periodic review: 6 months, 3 for anomaly/correlation);
Review --> Sunset{Sunset trigger met?};
Sunset --> |no| Monitor1;
{{< /mermaid >}}
