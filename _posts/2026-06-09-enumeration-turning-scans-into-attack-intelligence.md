---
title: "Enumeration: Turning Scan Results into Actionable Attack Intelligence"
date: 2026-06-09 10:00:00 -0700
categories: [Portfolio]
tags: [enumeration, gobuster, smb, ftp, pfsense, offensive-security]
image: /assets/img/posts/lab3-banner.png
---

# Enumeration: Turning Scan Results into Actionable Attack Intelligence

Following reconnaissance and vulnerability assessment, the next phase of my Information Security coursework focused on enumeration. While scanning identifies hosts and services, enumeration transforms that information into meaningful intelligence by uncovering users, hidden directories, application functionality, service banners, and configuration details that may later support exploitation.

This phase demonstrated that successful penetration testing depends just as much on careful information gathering as technical exploitation. Small pieces of information collected from multiple systems can often be combined to reveal realistic attack paths that would otherwise remain hidden.

## Objectives

- Enumerate Windows and Linux systems
- Identify SMB shares and authentication methods
- Discover hidden web directories
- Enumerate exposed network services
- Identify potential information leakage
- Develop attack hypotheses based on collected intelligence

## Why Enumeration Matters

Enumeration bridges the gap between reconnaissance and exploitation.

During the previous assessment phase, my team identified hosts, ports, and services across the Westfin lab environment. Enumeration took that information a step further by asking more useful questions: What does each service reveal? Are there usernames or banners exposed? Are there hidden directories that are not visible through normal browsing? Are administrative portals reachable from the network?

While vulnerability scanners identify potential weaknesses, enumeration focuses on understanding how systems actually operate. By collecting usernames, banners, hidden directories, authentication portals, and configuration information, security professionals gain a much deeper understanding of an organization's attack surface.

Many successful penetration tests rely more on careful enumeration than sophisticated exploitation techniques.

---

## Target Selection

Based on earlier scanning results, my team focused on three systems that had interesting services exposed:

- `172.26.4.229`, which exposed FTP, SMB, RDP, WinRM, and HTTP services
- `172.26.4.227`, which exposed SSH, HTTP, RPCBind, and MySQL
- `172.26.4.1`, which hosted pfSense services including SSH, DNS, and HTTP

Each target represented a different type of attack surface. One appeared to be a Windows host with remote administration services, another was a Linux-based application server, and the last was a firewall/router platform with administrative functionality exposed over HTTP.

This selection gave us a good mix of Windows, Linux, web application, and network infrastructure enumeration.

---

## SMB Enumeration

The first phase involved enumerating SMB services using tools such as **enum4linux**, **smbclient**, and **CrackMapExec**.

The goal was to determine whether SMB exposed anonymous shares, weak permissions, or domain-related information. Anonymous share access was restricted, which was a good defensive sign. However, the enumeration process still revealed useful domain-related information, authentication behavior, and system details that could assist future assessment activities.

Understanding SMB exposure remains important because improperly configured file shares continue to be a common source of information disclosure within enterprise environments. Even when anonymous access is blocked, SMB can still reveal authentication requirements, domain naming patterns, and account-related details depending on configuration.

![SMB Enumeration](/assets/img/posts/lab3-smb-enum.png)

*Figure 1: SMB enumeration performed against Windows systems.*

From a defensive standpoint, SMB should be restricted to only systems that require it, monitored for enumeration behavior, and configured to prevent anonymous information disclosure.

---

## Web Application Enumeration

After identifying web services during reconnaissance, additional enumeration was performed using **Gobuster** to discover hidden directories and resources.

This step was important because web applications often expose more than what is visible through normal browsing. Hidden paths, administrative panels, backup files, debug consoles, and application-specific directories can significantly increase risk if they are reachable without proper access controls.

On one host, Gobuster identified hidden paths such as `/chat` and `/console`. These endpoints were not obvious from the main page, but they provided additional insight into how the application was structured.

![Gobuster Enumeration](/assets/img/posts/lab3-gobuster-chat.png)

*Figure 2: Gobuster identifying hidden application directories.*

Additional enumeration against SuiteCRM exposed multiple application paths and resources. Some directories returned successful responses, others redirected, and several returned forbidden responses. Even when access is denied, these results are still useful because they confirm that certain directories exist and may be worth revisiting during later testing phases.

![SuiteCRM Enumeration](/assets/img/posts/lab3-suitecrm-gobuster.png)

*Figure 3: SuiteCRM directory enumeration results.*

This phase reinforced that directory enumeration is not just about finding pages that immediately load. It is also about understanding the application's structure, identifying protected resources, and building a map of potential attack surface.

---

## pfSense Administrative Interface

Enumeration also identified an exposed **pfSense** administrative portal.

Administrative interfaces should always be reviewed carefully because they often become high-value targets during penetration tests if misconfigured or exposed to untrusted networks. A firewall or router management interface is especially sensitive because it can provide insight into routing, interfaces, DNS settings, and internal network structure.

The pfSense login portal confirmed that an administrative web interface was reachable from the testing environment.

![pfSense Login](/assets/img/posts/lab3-pfsense-login.png)

*Figure 4: pfSense administrative login portal.*

After accessing the dashboard within the authorized lab environment, the interface revealed system information such as platform type, version, interface addresses, DNS settings, and uptime.

![pfSense Dashboard](/assets/img/posts/lab3-pfsense-dashboard.png)

*Figure 5: pfSense administrative interface.*

From a security perspective, this type of information can be valuable because it helps an attacker understand network layout and device configuration. For defenders, administrative interfaces should be limited to a management network, protected with strong authentication, and monitored for suspicious access attempts.

---

## Service-Level Enumeration

Beyond web applications, additional services including FTP, SSH, RPCBind, WinRM, DNS, and MySQL were enumerated to gather version information and configuration details.

Banner grabbing identified application versions and welcome messages that may provide useful intelligence during future security assessments. Even when direct exploitation is not possible, exposed service banners can reveal valuable information about internal infrastructure.

The FTP service returned a banner identifying the software and referencing a financial document portal. Although anonymous login was not allowed, the banner still confirmed the role of the service and suggested that the host may be related to document handling.

![FTP Enumeration](/assets/img/posts/lab3-ftp-banner.png)

*Figure 6: FTP banner enumeration results.*

MySQL enumeration confirmed that the database service was reachable but implemented protections that limited unauthorized enumeration. Several attempts returned blocked or limited responses, which suggested that the service was configured to reduce information exposure after repeated connection attempts.

![MySQL Enumeration](/assets/img/posts/lab3-mysql-enum.png)

*Figure 7: MySQL enumeration results.*

This was a useful reminder that not every exposed service immediately results in useful data. Defensive controls such as authentication requirements, rate limiting, connection blocking, and restricted metadata disclosure can reduce the value of enumeration attempts.

---

## Information Leakage

One of the most valuable outcomes of enumeration was identifying information leakage.

Application banners, usernames, authentication portals, service versions, and exposed directories collectively created a much clearer picture of the simulated enterprise environment. While each individual finding appeared relatively minor, combining multiple sources of information significantly increased our understanding of the overall attack surface.

User enumeration was especially important because discovered usernames can influence future testing decisions. Even without passwords, usernames reduce uncertainty and may become useful during later authentication testing, password policy review, or incident response analysis.

![User Enumeration](/assets/img/posts/lab3-user-enum.png)

*Figure 8: User and service information gathered through enumeration.*

From a blue-team perspective, this highlights why organizations should reduce unnecessary information exposure. Usernames, banners, and administrative paths may not be vulnerabilities by themselves, but they can support attack planning when combined with other weaknesses.

---

## Attack Hypotheses

Using the intelligence collected throughout enumeration, several potential attack paths could be identified for later testing phases.

For the Windows-based host, exposed services such as RDP, WinRM, SMB, and FTP suggested that authentication and remote administration paths would be worth investigating later. For the Linux-based application server, exposed SSH, HTTP, RPCBind, and MySQL services created multiple areas for additional testing. For pfSense, the exposed administrative interface stood out as a high-value target because network infrastructure devices often have broad visibility and control over an environment.

This process demonstrated how enumeration directly supports offensive security methodology by guiding future decision making rather than relying on guesswork.

![Attack Hypothesis](/assets/img/posts/lab3-attack-hypothesis.png)

*Figure 9: Enumeration findings used to develop potential attack paths.*

A strong penetration test is not just a list of tools and outputs. It is a process of forming hypotheses, validating assumptions, and documenting why certain systems deserve more attention.

---

## Blue Team Takeaways

Enumeration also provided several defensive lessons:

- Restrict administrative interfaces to trusted management networks
- Disable unnecessary services when they are not required
- Limit banner disclosure and verbose error messages
- Prevent anonymous SMB enumeration
- Monitor for directory brute forcing and service enumeration
- Apply network segmentation to reduce unnecessary exposure
- Review exposed web directories and remove unused application paths

From a defensive perspective, enumeration activity often creates detectable patterns. Repeated requests to hidden directories, unusual SMB queries, connection attempts to multiple services, and banner grabbing behavior can all be monitored through logs and security tooling.

---

## Lessons Learned

Enumeration proved to be one of the most valuable phases of the assessment.

Rather than relying solely on automated vulnerability scanners, manually exploring services and applications provided significantly deeper insight into the environment and revealed information that would later support exploitation planning.

This exercise reinforced the importance of patience, observation, and systematic methodology during offensive security engagements. It also showed that seemingly small details can become meaningful when combined with other findings.

---

## Final Thoughts

This project strengthened my understanding of enterprise enumeration techniques and demonstrated how seemingly insignificant pieces of information can combine into meaningful attack intelligence.

The knowledge gained during this phase became the foundation for later coursework involving initial access, privilege escalation, lateral movement, and the final Westfin red team capstone engagement.
