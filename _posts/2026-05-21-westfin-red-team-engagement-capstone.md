---
title: "Westfin Red Team Engagement: Lessons from a Semester-Long Capstone Project"
date: 2026-05-17 12:00:00 -0700
categories: [Portfolio]
tags: [red-team, capstone, active-directory, pfsense, vulnerability-management, incident-response]
---

# Westfin Red Team Engagement: Lessons from a Semester-Long Capstone Project

This post is the final write-up in my Information Security course project series. After building an offensive security toolkit, performing reconnaissance, conducting vulnerability assessments, enumerating services, gaining initial access, and practicing lateral movement concepts, my team brought everything together in a full red team-style capstone engagement.

The environment was a simulated financial services company called **Westfin**. Our objective was to assess the organization's security posture by identifying vulnerabilities, validating realistic attack paths, documenting business impact, and building a remediation roadmap that could be understood by both technical and non-technical stakeholders.

What made this project valuable was that it did not stop at "getting access." The engagement required us to think through the full lifecycle of an assessment: planning, execution, evidence collection, impact analysis, detection opportunities, and practical remediation.

## Project Scope

The assessment focused on the Westfin lab environment and its in-scope hosts. These systems represented common enterprise components such as a perimeter firewall, web applications, a Windows file server, a customer support application, and a primary domain controller.

![Westfin Scope](/assets/img/posts/capstone-scope.png)

*Figure 1: In-scope systems documented for the Westfin simulated environment.*

## Methodology

My team approached the engagement in phases. Each phase built on the previous one, which made the project feel closer to a real security assessment instead of a collection of disconnected labs.

At a high level, our methodology included:

- Tool setup and offensive platform preparation
- Network discovery and port scanning
- Vulnerability assessment
- Web application reconnaissance
- SMB, DNS, RPC, SSH, and service enumeration
- Controlled exploitation
- Post-exploitation validation
- Lateral movement analysis
- Command-and-control implementation
- Business impact assessment
- Remediation planning

![Methodology Summary](/assets/img/posts/capstone-methodology-summary.png)

*Figure 2: Vulnerability summary table created during the assessment.*

## Reconnaissance and Vulnerability Assessment

The first major phase involved mapping the Westfin network and identifying exposed systems. Tools such as **Nmap**, **Greenbone/OpenVAS**, **Nessus**, **OWASP ZAP**, **Nikto**, **Gobuster**, and **Burp Suite** were used throughout the project.

This phase reinforced something I learned throughout the earlier labs: reconnaissance is not just about collecting output from tools. It is about building context. Each scan result helped shape our understanding of which systems were most exposed, which services were interesting, and where we should spend more time during enumeration.

One important finding was an end-of-life operating system identified by OpenVAS. While this did not automatically mean exploitation would succeed, it represented a serious risk because unsupported systems no longer receive normal security updates.

![OpenVAS Results](/assets/img/posts/capstone-openvas-eol.png)

*Figure 3: OpenVAS identifying a critical end-of-life operating system finding.*

![Vulnerability Summary](/assets/img/posts/capstone-vulnerability-summary.png)

*Figure 4: Vulnerability severity summary and web reconnaissance evidence.*

## Web Application and Service Enumeration

After identifying live systems and exposed services, enumeration helped turn scan results into actionable intelligence.

Several web-facing systems were reviewed, including pfSense, Dolibarr, SuiteCRM, Microsoft IIS, and the Westfin AI customer support application. Directory enumeration and web application testing helped identify hidden endpoints, exposed login portals, missing security headers, and application-specific risks.

![Web Application Review](/assets/img/posts/capstone-web-apps.png)

*Figure 5: Web application evidence collected during the engagement.*

A particularly interesting enumeration result involved hidden endpoints on the AI customer support system. Finding paths such as `/chat` and `/console` showed why directory enumeration matters. These types of endpoints may not be visible through normal browsing, but they can reveal important parts of an application's attack surface.

![Gobuster Enumeration](/assets/img/posts/capstone-enumeration-gobuster.png)

*Figure 6: Gobuster enumeration identifying hidden application endpoints.*

## Key Finding: Default Credentials on pfSense

One of the most important findings involved the pfSense firewall. During enumeration, the dashboard displayed a warning that the administrator account password was still set to the default value.

This was a major issue because pfSense acted as the perimeter gateway for the environment. Administrative access to a firewall is not the same as access to a normal web application. A firewall controls routing, segmentation, traffic flow, and in many cases, visibility into communication between internal and external networks.

![pfSense Default Warning](/assets/img/posts/capstone-pfsense-default-warning.png)

*Figure 7: pfSense interface showing a default password warning.*

From a real-world perspective, this finding was a reminder that simple misconfigurations can be just as dangerous as complex vulnerabilities. Changing default credentials is one of the most basic security controls, but failing to do so can expose an entire network.

## Key Finding: Domain Controller Compromise

The most critical technical finding involved the primary domain controller. Our assessment demonstrated how compromise of a domain controller can quickly become compromise of the entire Active Directory environment.

Once domain-level administrative access was validated, the impact became much larger than a single host. A domain controller is central to authentication, authorization, group policy, and identity management. If that system is compromised, user accounts, privileged groups, domain policies, and access controls can no longer be trusted.

![Domain Admin Shell](/assets/img/posts/capstone-domain-admin-shell.png)

*Figure 8: Administrative shell access validated on the domain controller.*

This part of the project helped me better understand why Active Directory security is so important in enterprise environments. Many real-world breaches become severe not because the first host was important, but because attackers eventually reach identity infrastructure.

## Post-Exploitation and Lateral Movement

After initial access, the engagement moved into post-exploitation. This included validating access levels, reviewing system information, and testing whether credentials or access could be reused across other systems.

One of the most important lessons from this phase was how quickly a weak identity environment can turn into lateral movement. Once privileged access exists, attackers may be able to move from one system to another without needing to exploit a new vulnerability every time.

![pfSense Administrative Access](/assets/img/posts/capstone-pfsense-admin.png)

*Figure 9: pfSense administrative access confirmed during the engagement.*

The pfSense firewall also became important during post-exploitation because it provided visibility into routing and internal connectivity. From a defensive perspective, this reinforced why network devices need the same level of monitoring and hardening as servers.

![pfSense Pivot Validation](/assets/img/posts/capstone-pfsense-pivot.png)

*Figure 10: Connectivity and lateral movement evidence from the simulated environment.*

## Command-and-Control Implementation

For the command-and-control portion of the capstone, my team used Empire with the Starkiller interface. The purpose was to understand how a C2 framework can be used to manage an agent on a compromised system and why this type of activity is dangerous in a real enterprise environment.

The biggest learning point for me was not simply getting a callback. It was understanding the defensive side of the activity. C2 communication, PowerShell execution, endpoint protection changes, and unusual administrative behavior all create opportunities for detection if logging and monitoring are configured correctly.

![C2 Selection](/assets/img/posts/capstone-c2-selection.png)

*Figure 11: Command-and-control planning and selection rationale.*

![C2 Evidence](/assets/img/posts/capstone-c2-evidence.png)

*Figure 12: C2 implementation evidence from the engagement.*

## Business Impact

One of the strongest parts of this capstone was tying technical findings back to business impact.

For a simulated financial services company, the findings were severe. Domain controller compromise could expose credentials, enable account manipulation, allow lateral movement, and support ransomware deployment. Firewall compromise could allow traffic monitoring, rule manipulation, service disruption, and pivoting into internal systems.

The CIA triad helped frame the impact:

- **Confidentiality:** Credentials, file server contents, customer-facing application data, and internal traffic could be exposed.
- **Integrity:** User accounts, policies, firewall rules, and files could be modified.
- **Availability:** Domain-wide disruption, ransomware deployment, or firewall outages could affect the entire environment.

This project made it clear that a red team report should not only say what was exploited. It should explain why the finding matters to the business.

## Remediation Roadmap

The remediation roadmap focused on the issues that enabled the attack chain.

The highest-priority recommendations included:

- Patch critical vulnerabilities affecting domain controllers
- Rotate exposed credentials
- Remove default credentials from network devices
- Restrict administrative interfaces to trusted management networks
- Enforce multi-factor authentication for privileged access
- Reduce NTLM and pass-the-hash exposure where possible
- Improve network segmentation
- Enable centralized logging and SIEM alerting
- Tune IDS/IPS controls for scanning, lateral movement, and suspicious authentication
- Harden endpoint protection so it cannot be easily disabled

![Remediation Roadmap](/assets/img/posts/capstone-remediation-roadmap.png)

*Figure 13: Remediation priorities from the final report.*

## Lessons Learned

This capstone helped me connect multiple areas of cybersecurity into one complete engagement.

The earlier labs taught individual skills: tool setup, scanning, enumeration, exploitation, lateral movement, and detection planning. The capstone showed how those pieces fit together into a larger assessment.

My biggest takeaways were:

- Reconnaissance gives direction to the rest of the engagement.
- Enumeration often reveals the most useful attack intelligence.
- Misconfigurations can be just as dangerous as software vulnerabilities.
- Active Directory compromise creates organization-wide risk.
- Firewall compromise can expose and control entire network segments.
- Business impact matters just as much as technical proof.
- Strong documentation is what turns technical findings into useful security recommendations.

## Final Thoughts

This project was one of the most valuable assignments I completed during my cybersecurity coursework because it forced my team to think beyond isolated tools and commands.

A successful security assessment is not just about finding a vulnerability. It is about understanding the environment, validating risk safely, explaining business impact, and recommending realistic improvements.

The Westfin capstone gave me a much better appreciation for how red team methodology, blue team detection, vulnerability management, identity security, and business risk all connect together in an enterprise environment.
