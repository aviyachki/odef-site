+++
title = "Sunrise"
weight = 21
draft = false
+++


## Phase 1️⃣ Sunrise 🌅

Sunrise is the first phase of the detection lifecycle. It marks the inception, development and deployment of the detection. During that phase there are 6 core functions that should be addressed:

* Research
* Prepare (Logging)
* Build (Detection Content)
* Validate
* Automate
* Share (Knowledge)

#### High level goals for the Sunrise phase

* Build high fidelity detection  
* Ensure detection validation
* Create documentation
* Integrate and automate in the environment
* Socialize the detection with the security organization


<table >
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
    <td rowspan="5"><b>Research</b></td>
    <td> Opportunity Identification</td>
    <td> It can be triggered from analyzing threat intelligence reports, or OSINT, or internal knowledge for a particular security gap. Document the use case and the goals of the detection as part of the opportunity identification process. </td>
    <td>  

* Document the use case that you’re building and set goals.
* Is the TTP already covered by an existing alert or detection?
* Is there sufficient knowledge to start building or additional research would be required?
* What are sources of information that will assist the research?

</td>
  </tr>
  <tr>
    <td>Prioritize</td>
    <td>Detection engineering work has to be prioritized and tracked. Work prioritization can be based on urgency and priority. Backlog of detections and security posture activities is desirable and recommended.
</td>
    <td>Prioritization criteria:

- Criticality of the system
- Highest level of threat to the organization
- Ease of Exploitation
- Past incidents</td>
  </tr>
  <tr>
    <td>  Develop Research Questions</td>
    <td>  Write your research questions that while answering you will gain understanding of the topic. </td>
    <td>  Examples:

  - Write down what you already know or don't know about the topic.
  - Use that information to develop questions. Use probing questions. (why? what if?).
  - Avoid "yes" and "no" questions </td>
  </tr>
  
  <tr>
    <td>  Information Gathering</td>
    <td>  Research and collect sufficient information in order to start understanding the detection</td>
    <td>  Provides a good overview of the topic if you are unfamiliar with it.

  - Identify important facts, dates, events, history, organizations, etc. (in case the detection is a response to a past incident.)
  - Find bibliographies which provide additional sources of information (include in the Appendix section detection document)

</td>
  </tr>

  <tr>
    <td>  Technical Context</td>
    <td>  Create and understand technical context around the detection</td>
    <td>  
      <ul>
        <li> Start putting technical writeup by summarizing the most important information from technical aspect </li>
        <li> Research the technology associated with the technique to help understand the use cases, related data sources, and detection opportunities </li>
        <li> Note: Defenders often create superficial detections because they lack an understanding of the technology involved. In case of uncertainties it is best to engage the team or engineer responsible for the management of the technology</li>
      </ul>
</td>
  </tr>
  <tr>
    <td rowspan="3"><b>Prepare</b></td>
    <td> Identify Dataset</td>
    <td> Identify the log source that will be used for the detection</td>
    <td>  <b>Know your environment</b>
      <ul>
        <li> Understand the data source and document it by creating a data dictionary.</li>
        <li> The data dictionary should grow and contain sources of data and their corresponding schemas. It can later be used to quickly refer to. </li>
      </ul>
</td>
  </tr>
  <tr>
    <td>Visibility Check</td>
    <td>  Ensure there is sufficient logging, retention and visibility in order to successfully build the detection and satisfy the use case</td>
    <td>  
      <ul>
        <li> Use the accumulated technical knowledge to identify source and identify the events required to build detection</li>
        <li> Use any historical events in order to validate that there is sufficient visibility </li>
      </ul>

</td>
  </tr>
  <tr>
    <td>Improve (optional)</td>
    <td>  Once the data is explored we can identify opportunities for improvements such as:
      <ul>
        <li>Collecting additional logs or change logging levels </li>
        <li>Create additional attributes (parsing of raw logs)</li>
        <li>Consolidation of distinct logs</li>
      </ul>
</td>
    <td>  Improvement initiatives and requests should be communicated to the responsible for the dataset in question team. For that purpose it makes sense to maintain a contact list that provides quick reference to technology, support/engineering teams and contact details.
</td>
  </tr>
  <tr>
    <td rowspan="6"><b>Build &amp; Enrich</b></td>
    <td>  Detection Creation</td>
    <td>  Create a detection query against the identified dataset</td>
    <td>  Having a good understanding of the technical context and the data source begin building queries to narrow down the data to actionable insight.</td>
  </tr>
  <tr>
    <td>Manual Testing</td>
    <td>  Perform a manual testing and ensure the query works syntax and logical perspective</td>
    <td>  
    <ul>
        <li>Ensure the query does not have any syntax errors</li>
        <li>In case the detection is built in response to past incident ensure that the query is indeed catching true positive events</li>
    </ul>
      </td>
  </tr>
  <tr>
    <td>Baseline development</td>
    <td>  Develop a baseline (if needed) that will improve the detection fidelity</td>
    <td>  
        <ul>
          <li>Baselines are sets of known and verified good behaviors and events present in the organization. Those events are normally excluded from the detection logic. </li>
          <li>Baselines decisions and considerations should be documented and clearly stated in the ADS (Alerting and Detection Strategy)</li>
          <li>Baselines are included in the hunt.yml/tf/hcl or alert.yml/tf/hcl files</li>
        </ul>
    </td>
  </tr>
  <tr>
    <td>Unittest Development</td>
    <td>  The unittest development is dependent on the type of devops pipeline. Simple goals are provided.
        </td>
    <td>  Goals for the unittesting:
          <ul>
            <li>Changes or missing data</li>
            <li>Syntax errors </li>
            <li>To confirm detection logic by performing true positive detection</li>
        </ul></td>
  </tr>
  <tr>
    <td>  Enrich</td>
    <td>  Enrich with additional data source if required</td>
    <td>   
          <ul>
          <li>Each hunt could have different enrichment requirements. In some cases HR database could be used in order to understand if a person is on vacation, other trivial cases could be lookup of a hash, ip or domain in an threat intelligence repository etc.</li>
        </ul> </td>
  </tr>
  <tr>
    <td>Document</td>
    <td>  
        <ul>
          <li>Create KB Document</li>
          <li>Complete the ADS </li>
          <li> MITRE ATT&amp;CK coverage map update</li>
        </ul>
        </td>
    <td>  
    <ul>
          <li>Central knowledge base repository is required in order to mature the detection engineering program. This can be a github repository with controlled access that provides on a need to know basis the security teams members with access.</li>
          <li>Each hunt should have a corresponding <b>README.MD</b> file that provides sufficient information and context. Consider an SOC analyst or Incident Responder responding to an event from your detection. By looking at the documentation they should be easily briefed on the premise and technicalities of the detection. </li>
        </ul></td>
  </tr>
  <tr>
    <td rowspan="3"><b>Validate</b></td>
    <td>  Confirm unittests</td>
    <td>  Confirm unittest are working </td>
    <td>  Confirmation of the unittests can be done by inspecting the implemented devops pipeline and ensuring that the actions (in the case of github) for unittests are running</td>
  </tr>
  <tr>
    <td>  True Positive validation</td>
    <td>  Validate true positive event against real dataset using the query developed earlier. </td>
    <td>  
    True positive validation can be achieved by:
        <ul>
          <li>Using historical event that exists in the central data repository</li>
          <li>Emulation of the TTP by executing it in a controlled environment </li>
        </ul>
        </td>
  </tr>
  <tr>
    <td>  False Positive Validation</td>
    <td>  Ensure no FP are produced by the query when ran against the prod dataset. </td>
    <td>  
      <ul>
          <li>False positive events are good known events which are produced as output results by the detection/hunt query. </li>
          <li>If baseline is used it should be validated that the baseline is catching those good known events.
          Splunk example:
          Splunk you can use <b>makeresult</b> command to create fake results and test your baseline and how you handle false positives. </li>
      </ul>
    </td>
  </tr>
  <tr>
    <td><b>Automate</b></td>
    <td>  Automation &amp; deployment</td>
    <td>  This step is entirely dependent on the environment and should follow the standard ci/cd or automation practices of the organization. </td>
    <td>  Integrate with devops pipeline and enable continuous deployment </td>
  </tr>
  <tr>
    <td rowspan="2"><b>Share</b></td>
    <td>  Socialize the new detection</td>
    <td>  A notification process is required and it should be created. The process can be in the form of newsletter or slack channel notification, preferably automated one.</td>
    <td>  Follow a process to communicate the newly created detection with the Security Teams and inform them about it</td>
  </tr>
  <tr>
    <td>  Update Sec Dependency Tree</td>
    <td>  This document is actually part of the repository and can be shared with data engineering and security teams. The goal of sharing it is to promote care mentality where teams would check before they change. Meaning, if data engineer is about to rename an index they should first check if the index is being used. Having dependency document as part of the repository makes it easy and seamless for them to check. </td>
    <td>  Update organization wide document showing dependencies for the detections</td>
  </tr>
</tbody>
</table>

### Process Flow

{{<mermaid align="left">}}
graph TD;
Research1(Opportunity Identification) -->Research2(Prioritize);
Research2 -->Research3(Develop Research Questions);
Research3 -->Research4(Information Gathering);
Research4 -->Research5(Collect Technical Context);
Research5 -->Prepare1(Identify Dataset);
Prepare1 -->Prepare2(Visibility Check);
Prepare2 -->Prepare3{Improve};
Prepare3 --> |yes| cis[Start security improvement initiative];
Prepare3 --> |no| Build1(Detection Query Creation);
Build1 --> Build2(Manual Testing);
Build2 --> Build3(Baseline development);
Build3 --> Build4(Automated Unittest Development);
Build4 -->Build5(Enrich);
Build5 --> Build6(Document);
Build6 -->  Validate1(Confirm unittests);
Validate1 -->val2(True/False Positive validation);
val2-->automate(Automation & deployment);
automate --> share(Socialize the new detection);
share -->share1(Update Sec Dependency Tree);
{{< /mermaid >}}

