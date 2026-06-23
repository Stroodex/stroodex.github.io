---
title: "How I Approach Endpoint Detection Investigations"
date: 2026-06-16 12:00:00 -0700
categories: [Portfolio]
tags: [crowdstrike, endpoint-security, soc, incident-response, threat-detection, blue-team]
image:
  path: /assets/img/posts/crowdstrike-endpoint-investigation-banner.png
---

# How I Approach Endpoint Detection Investigations

Endpoint detections were one of the most interesting types of alerts I learned to investigate while working in a Security Operations Center.

Phishing and spam tickets helped me build a foundation, but endpoint detections felt different. They required more context, more careful analysis, and a better understanding of what was actually happening on a user's device.

A single endpoint alert can include a suspicious file, a process tree, a command line, a user account, a device, a hash, a remediation action, and sometimes several related detections. The challenge is not just reading the alert. The real work is understanding whether the activity is malicious, suspicious, expected for that user's role, or a false positive.

This post walks through the general workflow I use when approaching a CrowdStrike Falcon endpoint detection. The screenshots used here are from a public CrowdStrike demonstration, not internal company data.

## Starting from the Ticket Queue

The investigation usually starts when a ticket appears in the ServiceNow queue.

The ticket provides the initial summary of the detection. Depending on the alert, it may include details such as:

- Detection name
- File path
- Hostname or device name
- Username
- Severity
- Timestamp
- Process or command line
- File hash
- Current remediation status

At this stage, I do not assume the detection is malicious or benign. I treat the ticket as the starting point and use the available information to begin building context.

## Identifying the Device

After reviewing the ticket, the next step is to look up the device in CrowdStrike Falcon.

This helps answer basic but important questions:

- Is the device online?
- Who is the primary user?
- What is the device serial number?
- Has the device had previous detections?
- Are there multiple detections around the same time?
- Is the endpoint protected and checking in normally?
- Are there containment or remediation actions already applied?

This step matters because one alert by itself may not show the full picture. If the same device has repeated detections, recent suspicious activity, or a pattern of malware-related events, the risk may be higher.

## Understanding the User Context

The next thing I like to check is user context.

Not every suspicious-looking command is automatically malicious. Sometimes engineers, developers, administrators, or technical users may run tools or scripts that look unusual compared to normal business users.

This is where user research becomes important. I would review identity and user information in a directory platform such as Azure or an internal IT profile page to understand:

- The user's job role
- Department or business function
- Whether they are technical or non-technical
- Whether the activity matches their expected work
- Whether the detection occurred during normal working hours
- Whether the device is assigned to the correct user

For example, a PowerShell script running on a developer workstation may require a different level of context than the same script running on a finance user's laptop. The behavior still needs to be investigated, but the user's role helps guide the analysis.

## Opening the Detection

Once the device and user context are reviewed, I open the detection in CrowdStrike Falcon and focus on the process tree.

The process tree is one of the most useful parts of the investigation because it shows how the suspicious activity started and what happened afterward.

I usually look for:

- Parent process
- Child processes
- Command line arguments
- File paths
- Execution order
- User context
- Process duration
- Associated detections
- Whether the process was blocked, killed, quarantined, or allowed

![CrowdStrike Process Tree](/assets/img/posts/crowdstrike-process-tree-vssadmin.png)

*Figure 1: Example CrowdStrike process tree showing suspicious command-line activity from a public CrowdStrike demonstration.*

In the screenshot above, the process tree shows suspicious activity involving `vssadmin.exe` deleting shadow copies. That type of behavior is important because adversaries often remove backups to prevent recovery after ransomware activity.

This is where endpoint investigations become more than just checking whether a file was blocked. The process tree helps explain the behavior and intent behind the detection.

## Reviewing the Command Line

Command-line review is one of the most important parts of endpoint triage.

A file name alone can be misleading. A legitimate Windows binary can be used for malicious purposes depending on the arguments used.

Some questions I ask during command-line review include:

- What command was executed?
- Was a native Windows tool being abused?
- Was PowerShell, CMD, WMI, or another scripting tool involved?
- Was the command encoded, obfuscated, or unusually long?
- Did the command reference suspicious paths such as `Temp`, `AppData`, `Downloads`, or removable drives?
- Did the command attempt to disable security controls, delete backups, dump credentials, or modify persistence locations?

In many cases, the command line tells the story more clearly than the file name.

## Checking File Path and Hash Details

After reviewing the process behavior, I check the file path and hash details.

File path matters because suspicious files commonly appear in locations such as:

- `Downloads`
- `Temp`
- `AppData`
- User profile folders
- Removable media paths
- Unexpected script directories

Hash details can also help determine whether the file has been seen before or whether it is associated with known malicious behavior.

In CrowdStrike, hash and file information can support the investigation by showing reputation, prevalence, sandbox results, and related intelligence.

## Reviewing Falcon Actions Taken

One of the first things I look for is whether CrowdStrike already took action.

Common outcomes include:

- Process blocked
- File quarantined
- Process killed
- Detection only
- Host contained
- No action taken

![CrowdStrike Blocked Detection](/assets/img/posts/crowdstrike-payroll-blocked.png)

*Figure 2: Example of a blocked and quarantined file from a public CrowdStrike demonstration.*

If the process was blocked and the file was quarantined, that is a good sign, but it does not automatically mean the investigation is complete. I still want to understand whether anything executed before the block, whether there are related files, and whether the user needs to be contacted.

## Using Sandbox and Intelligence Details

Sandbox analysis can provide additional detail about a suspicious file or behavior.

In some cases, CrowdStrike may have sandbox or threat intelligence information showing behavioral indicators, contacted hosts, DNS requests, dropped files, extracted strings, or MITRE ATT&CK techniques.

![CrowdStrike Sandbox Report](/assets/img/posts/crowdstrike-sandbox-report.png)

*Figure 3: Example CrowdStrike sandbox report showing behavioral threat indicators from a public CrowdStrike demonstration.*

Sandbox details are useful because they can help answer questions such as:

- What behavior does the file attempt?
- Does it contact external infrastructure?
- Does it drop additional files?
- Does it attempt persistence?
- Does it inject into other processes?
- Does it match known malware behavior?
- Is the threat score high enough to support escalation?

This type of information helps turn a basic alert into a stronger analysis.

## Determining Whether the Activity Is Suspicious

After collecting the device, user, process, command line, file path, hash, and sandbox context, I begin forming the analysis.

I usually try to classify the activity into one of several categories:

- Benign work-related activity
- False positive
- Suspicious but contained
- Confirmed malware
- Policy violation
- Needs user follow-up
- Needs additional remediation
- Needs escalation to an incident ticket

This part requires judgment. A detection may be blocked, but the activity might still require follow-up. A user may be technical, but that does not mean every script they run is safe. A file may be quarantined, but there may be other artifacts or persistence mechanisms that need review.

## User Follow-Up

If the detection requires more context, I would reach out to the user or the appropriate support team.

The goal is to confirm:

- Were they aware of the activity?
- Did they download or execute the file?
- Was the activity related to approved work?
- Did they plug in removable media?
- Were they expecting the application or script to run?
- Did they notice unusual behavior on the device?

This is where communication matters. The message should be professional and understandable for a non-technical user. Instead of overwhelming them with raw detection details, I try to explain what was observed and what information is needed.

## Additional Scanning and Remediation

Depending on the detection, additional remediation may be required.

Examples include:

- Running a malware scan
- Running a cleanup/remediation action
- Removing malicious files
- Checking for related detections
- Confirming quarantine status
- Validating that no additional suspicious files remain
- Creating an incident ticket for IT support or another security team
- Requesting device reimage or hands-on support if needed

If there are too many malicious items or the system requires deeper cleanup, an incident ticket may be created so the correct team can assist with remediation.

## Documenting the Investigation

Once the analysis is complete, the findings need to be documented back in the ticket.

Good ticket notes should include:

- What triggered the detection
- Host and user context
- Relevant process tree observations
- File path and hash details
- Actions taken by CrowdStrike
- Whether the user was contacted
- User response or business justification
- Additional scans or cleanup actions
- Final disposition
- Reason for closure or escalation

This is one of the most important parts of SOC work. If another analyst opens the ticket later, they should be able to understand exactly what happened and why the ticket was closed or escalated.

## Closing or Escalating the Ticket

The ticket can be closed only after the evidence supports the conclusion.

Common closure reasons include:

- Confirmed false positive
- Confirmed work-related activity
- File blocked and quarantined successfully
- Malware removed
- User confirmed expected behavior
- Device scanned and no further malicious activity found
- Incident ticket created and remediation transferred to the appropriate team

If the investigation reveals broader compromise, repeated detections, suspicious persistence, credential access, or uncontrolled malware activity, then escalation is the better option.

## Lessons Learned

Endpoint investigations taught me that context is everything.

A detection is not just a red or green indicator. It is a starting point. To understand the risk, an analyst has to review the user, device, process tree, command line, file path, hash, prior activity, and remediation status.

I also learned that communication is just as important as technical analysis. The analyst has to explain findings clearly to users, IT teams, and other security team members.

## Final Thoughts

CrowdStrike endpoint detections helped me understand how real SOC investigations work at the endpoint level.

The process requires patience, curiosity, and attention to detail. A good investigation does not stop at "the file was blocked." It asks what happened before the block, what the user was doing, whether the behavior was expected, whether additional remediation is needed, and how to document the conclusion clearly.

This type of work helped me become more confident as a SOC analyst because it connected technical evidence with real-world decision-making.
