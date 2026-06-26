---
title: "Lessons Learned from Red vs Blue Team Cybersecurity Competitions"
date: 2025-03-22 12:00:00 -0700
categories: [Portfolio]
tags: [red-vs-blue, blue-team, incident-response, windows-server, linux, cybersecurity-competition]
image:
  path: /assets/img/posts/red-vs-blue-experience-banner.png
---

# Lessons Learned from Red vs Blue Team Cybersecurity Competitions

One of the most hands-on cybersecurity experiences I participated in was competing in school-wide Red vs Blue team competitions, also known as RvB.

These competitions brought together teams from my school and other schools to defend vulnerable systems while a red team attempted to attack, disrupt, and compromise the environment. I competed in three of these events, and each one helped me understand what blue team work feels like under pressure.

The briefing packet for one of the competitions described the scenario as defending a fictional business from a simulated advanced persistent threat. Teams earned points mainly through **service uptime** and **injects**, which meant we had to keep services running while also completing timed technical and business tasks.

## Competition Overview

At the start of the event, each team received a briefing packet with the instructions needed to access the environment, connect to the VPN, log into vSphere, open assigned virtual machines, review the scoring engine, submit injects, and request box resets if needed.

The packet explained that the competition had two major scoring areas:

- **Service uptime:** keeping assigned services online and available
- **Injects:** completing timed tasks such as technical changes, written responses, or business-style deliverables

![Competition Overview](/assets/img/posts/rvb-competition-overview.png)

*Figure 2: Sanitized summary of the competition structure.*

This made the competition more realistic because winning was not only about blocking attacks. We also had to communicate clearly, respond to tasks on time, and avoid breaking services while trying to secure them.

## The Environment

Most competitors were on the blue team defending several assigned boxes. Depending on the competition, the environment included Windows Server systems and Linux systems. In the briefing packet I reviewed, the environment included systems such as Debian, CentOS, and Windows Server 2019, with scored services such as HTTPS, SSH, DNS, SMB, and SQL.

![Environment Overview](/assets/img/posts/rvb-environment.png)

*Figure 3: Sanitized overview of systems and services commonly defended during the competition.*

I was usually responsible for one of the Windows boxes. That meant my focus was on stabilizing the system, securing accounts, checking services, enabling appropriate firewall rules, and making sure the box continued to score.

The environment was accessed through a VPN and vSphere. Once connected, we could open the virtual machines and begin defending them. There was also a scoring engine that updated regularly and showed whether our services were online or down.

## My Role on Blue Team

My role was usually defending a Windows server.

That involved balancing two goals:

1. Secure the system as quickly as possible
2. Avoid breaking the services that were being scored

That second part was important. In a normal hardening situation, I might want to lock everything down aggressively. In a competition, making the wrong change could take a service offline and cost the team points.

This forced me to think carefully before making changes.

## First-Hour Defensive Checklist

The first hour of the competition was always one of the most important parts.

Before the red team gained momentum, I tried to focus on the basics:

- Confirm I could access my assigned Windows box
- Check what services were running
- Change default or provided credentials
- Update the scoring engine if required after password changes
- Review local and domain users
- Remove or disable unauthorized local accounts when appropriate
- Turn on necessary firewall rules
- Check for obvious suspicious processes
- Look for unusual startup items or persistence
- Confirm the scoring engine still showed services online

![First-Hour Checklist](/assets/img/posts/rvb-first-hour-checklist.png)

*Figure 4: My general first-hour defensive checklist for Windows systems.*

The biggest lesson from this stage was that fundamentals matter. Passwords, users, services, firewall rules, and uptime checks may sound basic, but they can decide how well a team performs early in the competition.

## Monitoring the Scoring Engine

The scoring engine was one of the most important tools during the event.

It checked whether services were working properly and updated regularly. If a service suddenly went down, that could mean:

- The red team attacked it
- My team accidentally broke it
- A firewall rule blocked the service
- A password change was not reflected correctly
- The service crashed or was misconfigured

This made the scoring engine almost like a real-time health dashboard. I had to keep checking it while also defending the box.

One of the hardest parts was knowing whether downtime was caused by the attacker or by our own defensive changes.

## Hardening Without Breaking Services

Red vs Blue competitions taught me that blue team work is not just about turning on every security control possible.

A defender has to understand what the system is supposed to do.

For example, blocking unnecessary ports is good, but blocking the port used by a scored service can hurt the team. Changing passwords is good, but if the scoring engine needs the updated credentials and they are not submitted correctly, the service may stop scoring.

This helped me learn a valuable blue team lesson: security has to support availability.

## Watching for Red Team Activity

As the competition continued, I watched for signs that the red team may have interacted with my system.

Some things I would look for included:

- New or suspicious user accounts
- Failed login attempts
- Unexpected services
- Strange scheduled tasks
- Unknown startup items
- Suspicious PowerShell or command prompt activity
- Unusual network connections
- Modified firewall rules
- Stopped services
- Unknown files in common locations
- Unexpected remote access behavior

I also used basic defensive tools where appropriate, such as Windows Event Viewer, Task Manager, Services, firewall settings, and malware scanning tools.

The goal was not only to react after something broke. The goal was to notice signs of compromise before the scoring engine showed major damage.

## Injects and Communication

Injects were timed tasks that teams had to complete during the competition.

Some injects were technical, while others were more business-oriented. Even though I do not remember the exact injects from every competition, typical examples could include:

- Submit a brief incident status update to management
- Explain what actions were taken after suspicious activity was found
- Document a firewall or service hardening change
- Create a short response plan for a simulated outage
- Identify unauthorized users and explain remediation steps
- Prepare a business-friendly explanation of a security risk
- Report which services are critical and how they are being protected

![Inject Examples](/assets/img/posts/rvb-inject-examples.png)

*Figure 5: Generic examples of injects that fit the style of Red vs Blue competitions.*

Injects made the competition feel more realistic because cybersecurity is not only technical. Analysts also have to communicate findings, provide status updates, and explain risk to non-technical audiences.

## Box Resets and Recovery

The briefing packet also explained that teams could request box resets if a system became too broken or compromised to recover quickly.

At first, a box reset might sound like giving up, but I learned that it is actually a recovery decision. If a system is too damaged and the team is losing points, restoring the box may be the smarter move.

The tradeoff is that any hardening changes, patches, passwords, or configurations may also be reset. That means the team has to decide whether recovery is worth losing previous work.

This taught me that incident response often involves tradeoffs between speed, control, and availability.

## What I Learned from Defending Windows Systems

Being responsible for a Windows box helped me improve in several areas:

- Windows Server administration
- User and password management
- Firewall rule review
- Service troubleshooting
- Event log review
- Malware scanning
- Basic persistence checks
- Remote access awareness
- Incident response under pressure
- Documentation during stressful situations

The competition environment made these skills feel more real because everything was timed and scored.

## What I Would Do Better Now

Looking back, there are several things I would improve if I competed again.

First, I would create a stronger checklist before the competition. When pressure is high, it is easy to forget basic steps. A checklist helps keep the process organized.

Second, I would document every major change. If a service went down later, documentation would make it easier to determine whether my team caused the issue or whether the red team did.

Third, I would communicate more with the teammates defending the other boxes. The red team may attack multiple systems in similar ways, so sharing observations quickly could help the whole team.

Finally, I would spend more time learning Windows event logs before the competition. Logs are extremely valuable when trying to determine whether activity is normal, misconfigured, or malicious.

## Lessons Learned

The biggest lesson from Red vs Blue competitions was that blue team defense is about balance.

You have to secure systems, keep services online, complete injects, communicate with teammates, and respond to attacks at the same time.

![Lessons Learned](/assets/img/posts/rvb-lessons-learned.png)

*Figure 6: Key lessons I took away from competing in Red vs Blue events.*

These competitions helped me understand that defense is not only about tools. It is about decision-making, prioritization, communication, and staying calm while things are changing quickly.

## How This Connects to SOC Work

These competitions connected directly to the type of thinking needed in a SOC.

In both environments, analysts need to:

- Triage alerts or service issues
- Identify what changed
- Determine whether activity is malicious or expected
- Communicate with the team
- Document actions taken
- Keep business impact in mind
- Escalate when necessary

The difference is that RvB compresses those skills into a short, high-pressure competition.

## Final Thoughts

Participating in Red vs Blue competitions was one of the best ways for me to practice cybersecurity in a live environment.

It gave me experience defending Windows systems, monitoring service uptime, responding to pressure, completing injects, and thinking like a blue team analyst.

Most importantly, it helped me understand that security is not only about stopping attackers. It is also about keeping systems available, communicating clearly, and making good decisions when time is limited.
