---
title: "Privilege Escalation and Lateral Movement in a Simulated Enterprise Environment"
date: 2026-04-05 12:00:00 -0700
categories: [Portfolio]
tags: [privilege-escalation, lateral-movement, active-directory, pfsense, red-team, blue-team]
image:
  path: /assets/img/posts/lab5-banner.png
---

# Privilege Escalation and Lateral Movement in a Simulated Enterprise Environment

After gaining initial access in the previous phase of my Information Security coursework, the next step was understanding how a single foothold could turn into broader control inside a simulated enterprise network.

This lab focused on advanced exploitation concepts, including privilege escalation, lateral movement, pivoting, impact validation, detection opportunities, and remediation planning. Instead of treating exploitation as the final goal, this phase emphasized what happens after access is gained and how defenders can detect and contain that activity.

The environment simulated a financial organization called Westfin, where systems such as a domain controller, firewall, file server, and internal services were connected across segmented networks.

## Objectives

The main goals of this phase were to:

- Review previously obtained footholds
- Validate privileged access
- Demonstrate lateral movement concepts
- Understand how compromised credentials can expand impact
- Evaluate pfSense as a potential pivot point
- Identify business impact from domain and firewall compromise
- Document detection and remediation opportunities

## Why Post-Exploitation Matters

Initial access is important, but it usually only tells part of the story.

In real-world incidents, attackers rarely stop after compromising one system. They often attempt to escalate privileges, discover credentials, move laterally, identify high-value assets, and establish control over infrastructure that supports the rest of the environment.

This lab helped me understand why defenders must monitor beyond the initial alert. A single compromised host can quickly become a larger incident if privileged accounts, firewall access, or internal trust relationships are abused.

## Foothold Review and Attack Planning

The first step was reviewing access gained from the previous lab. One of the most important systems was the domain controller, DC01, because compromise of a domain controller can affect authentication, authorization, and administrative control across the entire environment.

Another high-value target was the pfSense firewall. Since pfSense controlled traffic between network segments, administrative access to it could provide visibility into internal routing, firewall rules, and reachable hosts.

![pfSense Reconnaissance](/assets/img/posts/lab5-pfsense-recon.png)

*Figure 1: Reviewing exposed pfSense services and their significance inside the simulated environment.*

## Privilege Escalation and Domain Impact

One of the most important findings involved privileged access to the domain controller. After validating access, the team confirmed that the session had administrative privileges inside the Westfin domain.

From a security perspective, this represented a severe impact. Domain administrator access means an attacker may be able to manage users, access systems, modify policies, retrieve credentials, and move across the network with trusted privileges.

![Domain Controller Access Validation](/assets/img/posts/lab5-dc-access-validation.png)

*Figure 2: Validating administrative access on the domain controller in the lab environment.*

## pfSense Administrative Access

The pfSense firewall was another critical system. The firewall served as a gateway between the external and internal network segments, which made it an extremely valuable target from both an offensive and defensive perspective.

Once administrative access was obtained, the dashboard exposed system details such as the hostname, interface configuration, version information, and installed services.

![pfSense Login and Dashboard](/assets/img/posts/lab5-pfsense-login-dashboard.png)

*Figure 3: pfSense administrative access confirmed inside the simulated environment.*

## Root-Level Command Execution

A major issue with administrative access to pfSense was the ability to execute operating system commands through the web interface. This demonstrated that compromise of the management console was not limited to configuration visibility. It could also lead to direct control over the underlying firewall operating system.

From a defensive perspective, this is why administrative consoles must be restricted, monitored, and protected with strong authentication controls.

![pfSense Command Execution](/assets/img/posts/lab5-pfsense-root-command.png)

*Figure 4: Command execution through the pfSense administrative interface.*

## Lateral Movement

After privileged credentials were available, the next step was validating whether access could be reused across other systems.

This phase demonstrated how credentials from one compromised system can enable access to additional internal hosts. In the lab environment, administrative access was validated against other systems, showing how quickly an attacker could expand control once domain-level privileges are available.

![Lateral Movement](/assets/img/posts/lab5-lateral-movement.png)

*Figure 5: Lateral movement validation against additional systems in the simulated network.*

## Network Enumeration Through pfSense

With firewall-level access, pfSense became a valuable point for internal network visibility. Network interface details, routing tables, ARP entries, and connectivity tests helped map the internal environment from the firewall's perspective.

This was one of the biggest lessons from the lab: a compromised firewall is not just another server. It can become a visibility point, a routing control point, and a potential pivot point into internal systems.

![Network Enumeration](/assets/img/posts/lab5-network-enum.png)

*Figure 6: Enumerating network interfaces from pfSense.*

![Routing and ARP Information](/assets/img/posts/lab5-routing-arp.png)

*Figure 7: Reviewing routing and ARP information from the firewall.*

## Pivot Validation

To validate pfSense as a pivot point, connectivity to internal systems was tested from the firewall. Successful communication confirmed that the firewall could reach internal hosts and had visibility into both external and internal network segments.

From a blue-team perspective, this is exactly why gateway devices require strong monitoring. If a firewall begins initiating unusual internal traffic, that behavior should stand out.

![Pivot Validation](/assets/img/posts/lab5-pivot-validation.png)

*Figure 8: Validating pfSense connectivity to internal systems.*

## Multi-Step Attack Chain

The most important part of the lab was tying everything together into an attack narrative.

The chain began with privileged access to critical infrastructure, followed by credential reuse, lateral movement, firewall access, network enumeration, and impact validation. This helped me understand how individual weaknesses can combine into a much larger security issue when layered defenses are missing.

![Attack Chain](/assets/img/posts/lab5-attack-chain.png)

*Figure 9: Multi-step attack chain documentation from the lab report.*

## Business Impact

The business impact of this lab was significant.

Compromise of the domain controller represented a potential full-domain compromise. This could affect user accounts, authentication, group policy, internal access, sensitive data, and system availability.

Compromise of pfSense was also critical because the firewall controlled traffic between network segments. An attacker with access to the firewall could potentially monitor traffic, modify rules, expose internal services, disrupt business operations, or use the device as a pivot point.

From a business standpoint, this type of attack could impact:

- Confidentiality through exposed credentials and sensitive data access
- Integrity through unauthorized changes to accounts, systems, and firewall rules
- Availability through service disruption or ransomware deployment
- Reputation through loss of trust after a major security incident

## Detection and Defensive Considerations

One of the strongest parts of this lab was shifting from offensive activity to defensive thinking.

Several events could have been detected through Windows logs, firewall logs, SIEM alerts, IDS signatures, and authentication monitoring. Examples included unusual domain controller activity, suspicious administrative logins, abnormal WinRM usage, firewall rule changes, and unexpected internal traffic originating from the firewall.

![Detection Considerations](/assets/img/posts/lab5-detection-considerations.png)

*Figure 10: Detection and defensive considerations documented during the lab.*

## Remediation and Hardening

The remediation plan focused on reducing the likelihood and impact of similar attack paths.

Important recommendations included:

- Apply security patches for known critical vulnerabilities
- Rotate exposed credentials
- Restrict WinRM and administrative access
- Disable or restrict NTLM where possible
- Segment domain controllers from general network access
- Enforce multi-factor authentication for privileged accounts
- Change default pfSense credentials immediately
- Restrict firewall web management to trusted management networks
- Enable centralized logging and SIEM alerting
- Tune IDS/IPS rules for scanning and suspicious behavior
- Monitor firewall rule changes and administrative sessions

![Remediation Planning](/assets/img/posts/lab5-remediation.png)

*Figure 11: Remediation and hardening recommendations from the lab.*

## Lessons Learned

This lab helped me understand how post-exploitation activity connects individual findings into a larger attack path.

The biggest takeaway was that privilege escalation and lateral movement are not isolated technical steps. They represent the point where a security incident can grow from one compromised system into a domain-wide or network-wide compromise.

I also learned that the blue-team perspective is just as important as the offensive perspective. Every action taken during the lab created potential detection opportunities, whether through authentication logs, command execution logs, network telemetry, firewall logs, or SIEM correlation.

## Final Thoughts

This project strengthened my understanding of advanced exploitation methodology, lateral movement, pivoting, and defensive analysis.

More importantly, it showed how critical it is for organizations to build layered defenses. Patching, segmentation, strong authentication, logging, monitoring, and least privilege all work together to prevent a single weakness from becoming a full environment compromise.

The lessons from this lab directly supported the final Westfin capstone engagement, where my team brought together reconnaissance, enumeration, exploitation, lateral movement, business impact analysis, and remediation planning into a full red team-style assessment.
