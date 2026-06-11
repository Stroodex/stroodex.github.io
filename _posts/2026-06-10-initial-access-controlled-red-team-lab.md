---
title: "Initial Access in a Controlled Red Team Lab"
date: 2026-06-10 10:00:00 -0700
categories: [Portfolio]
tags: [initial-access, active-directory, pfsense, red-team, vulnerability-management]
---

# Initial Access in a Controlled Red Team Lab

After completing reconnaissance, vulnerability assessment, and enumeration, the next phase of my Information Security coursework focused on controlled initial access. This lab was performed only inside the authorized Westfin simulated enterprise environment, and the objective was to validate whether previously discovered weaknesses could realistically lead to system access.

This phase felt different from the earlier labs because it moved from identifying risk to proving impact. Instead of only saying that a vulnerability or misconfiguration looked dangerous, the goal was to demonstrate what level of access could be obtained, validate that access, and explain why the finding mattered from a business and defensive perspective.

## Objectives

- Select high-value targets based on previous reconnaissance and enumeration
- Validate vulnerabilities discovered in earlier labs
- Obtain initial access in a controlled and documented manner
- Confirm the level of access obtained on each system
- Analyze potential business impact
- Provide remediation recommendations for each finding

## Why Initial Access Matters

Initial access is one of the most important phases of a red team or penetration testing engagement because it demonstrates how an attacker could enter an environment for the first time.

Reconnaissance and enumeration help identify possibilities, but initial access validates whether those possibilities can become real impact. From a defensive perspective, this phase helps organizations understand which vulnerabilities are theoretical and which ones can be used to compromise critical systems.

In this lab, the most important lesson was that initial access does not always require complex exploitation. In some cases, a single unpatched critical vulnerability or a simple default credential can create a major security risk.

---

## Target Selection

The first major target was **DC01**, the domain controller for the Westfin environment. This system was selected because domain controllers are central to authentication, identity management, and Active Directory operations. A compromise of this system could affect the entire domain.

The second target was the **pfSense firewall**, which was exposed through a web administration interface. During enumeration, the dashboard indicated that the default administrator password was still in use, making it a high-risk configuration issue.

A third target involving SSH access was also reviewed, but the access attempt did not complete within the lab timeframe. That result was still useful because it showed the importance of time constraints, scope control, and realistic assessment planning.

---

## Research and Validation

Before attempting access, my team reviewed the relevant vulnerability information and confirmed that the findings matched the target environment.

For DC01, the key issue was **Zerologon (CVE-2020-1472)**, a critical Netlogon authentication bypass vulnerability affecting domain controllers. Since the target was a domain controller and the vulnerability had been validated during earlier testing, it became a high-priority initial access path.

![Zerologon Research](/assets/img/posts/lab4-zerologon-research.png)

*Figure 1: Research and validation material reviewed for the Zerologon finding.*

For pfSense, the issue was not a traditional CVE. Instead, it was a configuration weakness: the default administrative credentials had not been changed. This is a good reminder that misconfigurations can be just as dangerous as software vulnerabilities.

![pfSense Login](/assets/img/posts/lab4-pfsense-login.png)

*Figure 2: pfSense administrative login portal reviewed during initial access testing.*

---

## Initial Access Against DC01

The DC01 system represented the most critical target because it served as the primary domain controller. After confirming the system was vulnerable within the authorized lab, my team validated that the weakness could lead to domain-level access.

The assessment demonstrated how a critical Active Directory vulnerability can quickly escalate from a single system weakness into full domain compromise. Once access was established, additional validation confirmed the session was operating with elevated privileges.

![Zerologon Validation](/assets/img/posts/lab4-zerologon-validation.png)

*Figure 3: Validation of the domain controller vulnerability in the lab environment.*

Credential-related data was also obtained during the assessment, which demonstrated how dangerous domain controller compromise can be. In a real organization, this could create a path toward credential reuse, privilege escalation, and lateral movement.

![Credential Extraction](/assets/img/posts/lab4-credential-extraction.png)

*Figure 4: Credential-related output collected during the authorized lab assessment.*

Remote access was then validated using the compromised administrative context. This confirmed that the finding was not only theoretical but could lead to interactive access on a critical system.

![Evil-WinRM Session](/assets/img/posts/lab4-evil-winrm.png)

*Figure 5: Remote access validation against DC01 in the controlled lab environment.*

---

## Initial Access Against pfSense

The pfSense firewall represented a different type of issue. Unlike DC01, this finding did not require exploiting a complex vulnerability. Instead, the firewall was accessible through its web interface using unchanged default credentials.

Once authenticated, the dashboard provided visibility into system information, interfaces, version details, and firewall configuration. Because pfSense controls network traffic, administrative access to this system could have serious consequences in a real environment.

![pfSense Dashboard](/assets/img/posts/lab4-pfsense-dashboard.png)

*Figure 6: pfSense dashboard confirming administrative access.*

This finding reinforced one of the simplest but most important security lessons: default credentials must always be changed before a system is placed into operation.

---

## Access Validation

After access was obtained, the next step was limited validation. The purpose was not to perform full post-exploitation, privilege escalation, or lateral movement. Instead, the lab required confirming the current user context, hostname, operating system, and network configuration.

For DC01, validation confirmed administrative access on a Windows Server system functioning as the domain controller.

![DC01 System Information](/assets/img/posts/lab4-dc-systeminfo.png)

*Figure 7: System information collected from DC01 during limited validation.*

![DC01 Network Configuration](/assets/img/posts/lab4-dc-ipconfig.png)

*Figure 8: Network configuration validation from the compromised DC01 host.*

For pfSense, dashboard information confirmed access to the firewall configuration and exposed WAN and LAN interface details.

![pfSense Validation](/assets/img/posts/lab4-pfsense-validation.png)

*Figure 9: pfSense interface and system details confirming administrative access.*

---

## Business Impact

The DC01 finding represented the highest business risk because compromising a domain controller can affect the entire Active Directory environment.

Potential impact includes:

- Unauthorized control over users and groups
- Exposure of password hashes
- Abuse of privileged accounts
- Creation of persistence mechanisms
- Lateral movement to other systems
- Potential ransomware deployment
- Loss of confidentiality, integrity, and availability across the domain

The pfSense finding also represented a serious risk because the firewall controls network traffic between segments. Administrative access could allow an attacker to change firewall rules, monitor traffic, disrupt connectivity, or use the device as a pivot point into other parts of the environment.

Together, these findings demonstrated how both technical vulnerabilities and basic misconfigurations can create major organizational risk.

---

## Defensive Perspective

From a blue-team perspective, several controls could reduce the likelihood and impact of these issues:

- Apply security patches for critical vulnerabilities as soon as possible
- Enforce secure configuration baselines
- Remove or change default credentials immediately
- Restrict administrative interfaces to trusted management networks
- Monitor authentication activity for unusual behavior
- Use endpoint detection and response tools to detect suspicious activity
- Segment critical infrastructure such as domain controllers and firewalls
- Perform routine vulnerability scanning and configuration reviews

This lab made it clear that defense-in-depth matters. If one control fails, other layers should still limit how far an attacker can go.

---

## Lessons Learned

One of the biggest lessons from this project was understanding the difference between identifying a vulnerability and proving its impact. Earlier labs helped identify possible weaknesses, but this lab showed how those weaknesses could lead to real access in a controlled environment.

I also learned that not all major security issues are advanced. The pfSense finding was simple, but its impact was significant. Default credentials are easy to overlook, but they can provide immediate administrative access if not corrected.

The DC01 finding reinforced the importance of patch management, especially for systems that provide authentication and identity services. Domain controllers are among the most critical assets in an enterprise network, and vulnerabilities affecting them should be treated with the highest priority.

---

## Final Thoughts

This lab was an important turning point in the Westfin project series. The first three labs focused on building tools, discovering systems, assessing vulnerabilities, and enumerating attack surface. Lab 4 connected those earlier phases by validating which findings could actually lead to access.

The experience helped me better understand how offensive security methodology builds in stages: reconnaissance informs enumeration, enumeration supports target selection, and initial access validates risk.

Most importantly, this lab showed why documentation and remediation are just as important as technical execution. A successful assessment should not only prove that access is possible, but also explain how the organization can reduce the risk going forward.
