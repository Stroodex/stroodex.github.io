---
title: "How I Approach AWS GuardDuty Alert Investigations"
date: 2026-05-29 12:00:00 -0700
categories: [Portfolio]
tags: [aws, guardduty, cloud-security, soc, incident-response, splunk]
image:
  path: /assets/img/posts/aws-guardduty-investigation-banner.png
---

# How I Approach AWS GuardDuty Alert Investigations

AWS GuardDuty alerts were one of the more interesting cloud security detections I learned to investigate in a SOC environment.

Endpoint detections usually focus on a device, process tree, file path, command line, and user activity on a machine. Cloud alerts are different. A GuardDuty alert may involve an AWS account, region, IAM principal, API call, source IP address, S3 bucket, access key, resource policy, or cloud service action.

Because of that, cloud investigations require a slightly different mindset. The goal is not only to ask, "Was this malicious?" The better question is, "Does this activity make sense for this user, account, resource, and business function?"

This post explains a **general SOC investigation approach** for thinking through AWS GuardDuty alerts. It is not a company-specific runbook or internal procedure. It does not include internal screenshots, company names, account IDs, user information, private detection data, or exact internal queries.

## Confidentiality Note

This post is a sanitized overview of a general investigation workflow.

I intentionally do not include:

- Internal company screenshots
- AWS account IDs
- Usernames or email addresses
- Internal Slack channel names
- Ticket numbers
- Exact detection payloads
- Private Splunk queries
- Internal runbook language
- Customer or business data

The purpose of this post is to explain my thought process and investigation approach, not to share internal procedures or sensitive information.

## Starting from the Alert

The investigation usually begins when a GuardDuty alert appears in a ticketing queue and is also surfaced in a team alerting channel.

The first step is acknowledging the alert so the rest of the team knows someone has eyes on it. This is a small step, but it matters in a SOC environment because it helps prevent duplicate work and makes ownership clear.

After acknowledging the alert, I review the ticket and begin looking at the basic details.

Typical alert details may include:

- Finding type
- Severity
- AWS account name
- Account owner
- Account ID
- Region
- Event type
- Resource type
- Affected resource
- Source IP address
- User or IAM principal
- Detection timestamp
- Additional finding details
- Log source reference

At this point, I am not trying to close the ticket quickly. I am trying to understand what the alert is actually saying.

## Understanding the Finding Type

The finding type is the first major clue.

For example, a ticket may describe activity such as public access being granted to an S3 bucket, unusual API usage, suspicious IAM activity, or changes to cloud resources that require review.

A finding like public S3 bucket access requires different questions than a finding involving suspicious IAM credentials. For S3, I would want to know whether the bucket is meant to be public, whether Block Public Access settings were changed, whether a bucket policy or ACL was modified, and whether sensitive data could be exposed.

For IAM-related alerts, I would pay closer attention to actions such as:

- Creating access keys
- Updating policies
- Attaching privileged permissions
- Creating login profiles
- Assuming unusual roles
- Using root credentials
- Calling APIs from unexpected locations
- Creating suspicious access paths

The finding type helps guide the investigation path.

## Reviewing Cloud Context

After reading the ticket, I move into the cloud security platform and review recent cloud alerts for the affected account.

I usually focus on the last several days to understand whether the alert is isolated or part of a larger pattern.

Some questions I ask are:

- Has this account had similar alerts recently?
- Is this resource frequently modified?
- Is the same user involved in other alerts?
- Are there multiple findings around the same time?
- Is this account owned by an engineering, cloud, or development team?
- Does the activity line up with normal cloud work?

Context matters a lot in cloud security. A change that looks suspicious in one account may be normal in another account depending on the team's job function.

## Confirming Account and Owner Information

Next, I verify the account and owner information.

This usually involves checking an approved identity or directory source to confirm details such as:

- Account owner
- User email
- Department
- Job role
- Manager or team
- Whether the user is technical
- Whether the resource belongs to the expected team

This helps determine whether the activity fits the user's responsibilities. For example, a cloud engineer modifying an S3 bucket policy may make more sense than a non-technical user performing the same action.

That does not automatically make the activity safe, but it helps shape the analysis.

## Reviewing Source IP and Location

The source IP address is another important part of the investigation.

I check the IP address using WHOIS or an IP reputation/source lookup to understand whether it appears to come from a reasonable location or provider.

Some questions I ask include:

- Does the IP align with the user's normal location?
- Is it associated with a VPN, cloud provider, ISP, or known suspicious infrastructure?
- Is it from an unexpected country or region?
- Has the same IP appeared in other alerts?
- Does the IP match other successful activity from the same user?

Most alerts are not decided by IP alone. However, source IP helps support or challenge the story being built from the rest of the evidence.

## Reviewing Raw Logs and API Activity

The most important part of the analysis is reviewing the raw event details.

This may involve reviewing logs from a SIEM or cloud security platform. I focus on the API action, resource, identity, region, timestamp, and source context.

I usually look for:

- The exact API call
- The IAM principal or role involved
- The account and region
- The affected resource
- Whether the action succeeded or failed
- The source IP address
- User agent details
- Related API calls before and after the alert
- Whether the activity matches expected job function
- Whether privileged permissions were changed
- Whether access was granted externally
- Whether new access keys or access paths were created

This is where I try to separate normal cloud administration from suspicious behavior.

## Example: Public S3 Access Alert

One common type of cloud alert involves S3 bucket exposure or policy changes.

For this type of alert, I would review:

- Bucket name and owner
- Bucket purpose
- Whether the bucket is expected to be public
- Bucket policy changes
- ACL changes
- Block Public Access settings
- Object-level exposure risk
- Whether the change was made by an approved user
- Whether the timing matches a deployment or change request
- Whether sensitive data may be involved

If the bucket is intentionally public for a website or approved application, the alert may be benign after confirmation. If the bucket contains sensitive data or the public access change is unexpected, that would require escalation.

## Example: Suspicious IAM or Access Key Activity

IAM-related alerts require extra attention because they can indicate a potential path toward persistence or privilege escalation.

I would be more cautious if I saw actions such as:

- New access key creation
- Access key usage from an unusual location
- Policy attachment to a user or role
- Privilege escalation-related API calls
- New user or login profile creation
- Unusual role assumption
- External account access
- Activity from root credentials
- Changes that create long-term access

These types of changes can be more serious because they may allow continued access even after the initial activity is discovered.

## Determining Whether the Activity Is Expected

After reviewing the finding, account owner, user context, source IP, raw events, and API actions, I begin forming an initial conclusion.

The activity may fall into categories such as:

- Expected cloud administration
- Engineering or development work
- Approved deployment activity
- Misconfiguration requiring correction
- Suspicious but unconfirmed activity
- Policy violation
- Potential credential compromise
- Escalation required

This part requires judgment. The same API call may be expected in one context and suspicious in another.

## User Follow-Up

If the activity is unclear, I reach out to the user or resource owner for confirmation.

The message should be professional, clear, and non-accusatory. The goal is to confirm whether the user recognizes the activity and whether it was related to approved work.

I would usually ask questions such as:

- Did you perform this action?
- Was this related to an approved change, deployment, or test?
- Is this S3 bucket intended to be public?
- Was this access key or permission change expected?
- Were you working from the location or IP range observed?
- Should this resource remain in its current state?

If the user does not respond, I follow up after an appropriate amount of time. If the finding is high severity or risky enough, I would not wait too long before escalating.

## When I Would Escalate

Not every GuardDuty alert becomes a major incident, but some activity should be escalated quickly.

Examples that would make me more likely to escalate include:

- The user denies the activity
- The source IP is clearly suspicious
- Public access exposes sensitive resources
- New access keys were created unexpectedly
- Privileged policies were attached without approval
- Root credentials were used unexpectedly
- Activity comes from an unusual country or provider
- Multiple related alerts occur in a short period
- API activity suggests backdoor creation
- Logs show data access or attempted exfiltration
- The affected team cannot justify the activity

In those cases, escalation may involve creating an incident ticket, notifying the cloud security team, requesting containment, rotating keys, removing public access, disabling credentials, or applying other remediation steps.

## Documenting the Ticket

Once the investigation is complete, I document the analysis in the ticket.

Good notes should include:

- Summary of the finding
- Account and resource reviewed
- User or owner context
- Source IP and location review
- Raw event or API details
- Whether the activity matched job function
- User confirmation or lack of response
- Remediation actions taken
- Final disposition
- Reason for closure or escalation

Good documentation matters because someone else should be able to open the ticket later and understand exactly why the alert was closed or escalated.

## Closing the Alert

The alert can be closed when the evidence supports the conclusion.

Common closure reasons include:

- Confirmed expected activity
- Confirmed engineering or cloud development work
- Approved deployment or change
- Misconfiguration corrected
- User confirmed legitimate activity
- No suspicious follow-up activity found
- Ticket escalated to the correct team for remediation

If the finding is confirmed as malicious or cannot be explained, the alert should not simply be closed. It should be escalated and handled through the appropriate incident response process.

## Lessons Learned

AWS GuardDuty investigations taught me that cloud alerts are heavily context-driven.

The analyst needs to understand not just what happened, but whether the action makes sense for the user, account, resource, region, source IP, and business purpose.

I also learned that cloud investigations require strong communication. Many alerts involve engineers, cloud developers, application owners, or infrastructure teams. The SOC analyst has to ask clear questions and document the response in a way that supports the final decision.

## Final Thoughts

Investigating AWS GuardDuty alerts helped me become more comfortable with cloud security triage.

The workflow is different from endpoint investigations, but the core mindset is similar: start with the alert, gather context, review the evidence, contact the right person if needed, document the analysis, and escalate when the risk is too high.

The biggest takeaway for me was that cloud security is not just about tools. It is about understanding identity, permissions, API activity, ownership, and business context.

That combination is what helps turn a GuardDuty finding into a complete SOC investigation.
