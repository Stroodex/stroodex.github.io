---
title: Setting Up My Offensive Security Toolkit with Kali Linux
date: 2026-02-15 12:00:00 -0700
categories: [Course Projects]
tags: [kali-linux, burp-suite, owasp-zap, greenbone, nessus, nikto, dirbuster]
image:
  path: /assets/img/posts/lab1-offensive-security-toolkit-banner.png
pin: false
---

# Setting Up My Offensive Security Toolkit with Kali Linux

As part of my senior Information Security coursework, I configured a Kali Linux environment to serve as the main offensive security platform for future labs. The goal was not just to install tools, but to understand how each one fits into a security assessment workflow.

This setup became the foundation for later work involving reconnaissance, vulnerability scanning, enumeration, web application testing, and a final red team capstone project against a simulated enterprise environment.

---

## Objectives

For this project, my team focused on preparing a reliable Kali-based testing environment. The main objectives were to:

- Configure Firefox to work with Burp Suite
- Verify Burp Suite was running properly
- Install and launch OWASP ZAP
- Validate directory enumeration tools such as DirBuster
- Run Nikto and review its basic scanner output
- Set up vulnerability scanning tools such as Nessus Essentials and Greenbone/OpenVAS
- Build a repeatable toolkit for future penetration testing labs

---

## Nessus Essentials

Nessus Essentials was one of the first tools I reviewed because vulnerability scanning is a major part of security assessments. Nessus works by checking systems and services against a large plugin database to identify known vulnerabilities, risky configurations, and outdated software.

![Nessus Essentials Vulnerability Scanner Interface](/assets/img/posts/lab1-nessus-essentials.png)
_Nessus Essentials running in the Kali environment._

What I liked about Nessus is that it organizes findings in a way that is easy to understand from a risk perspective. Instead of only showing raw technical output, it helps prioritize issues based on severity, which is important when communicating findings to both technical and non-technical audiences.

---

## Nikto

Nikto is a web server vulnerability scanner that is useful during the early stages of web reconnaissance. It can identify common security issues such as outdated software, risky default files, exposed directories, and insecure server configurations.

![Nikto Web Server Vulnerability Scanner](/assets/img/posts/lab1-nikto-scanner.png)
_Nikto help output confirming the tool was installed and usable._

Even though Nikto is straightforward to run, it helped reinforce the importance of checking web servers for common misconfigurations before moving into deeper testing.

---

## DirBuster

DirBuster is used for directory and file enumeration against web applications. This is useful because many web servers contain hidden directories, backup files, administrative panels, or resources that are not directly linked from the main website.

![DirBuster Directory Enumeration Tool](/assets/img/posts/lab1-dirbuster.png)
_DirBuster running in Kali Linux._

This tool helped me understand why enumeration is such an important part of a security assessment. Sometimes the most valuable information is not exposed on the homepage, but still exists somewhere on the server.

---

## OWASP ZAP

OWASP ZAP is an open-source web application security testing tool. It can be used as both a proxy and scanner, allowing testers to observe requests, analyze responses, and identify common web application vulnerabilities.

![OWASP ZAP Running on Kali Linux](/assets/img/posts/lab1-owasp-zap.png)
_OWASP ZAP running successfully on the Kali virtual machine._

ZAP was useful because it gave me another perspective on web application testing outside of Burp Suite. Using multiple tools also helped me compare how scanners report different types of findings.

---

## Burp Suite and FoxyProxy

Burp Suite was configured with Firefox using FoxyProxy so browser traffic could be routed through Burp. This setup allows HTTP requests and responses to be intercepted, reviewed, and modified during web application testing.

![Burp Suite Proxy Configuration Using FoxyProxy](/assets/img/posts/lab1-burp-foxyproxy.png)
_Burp Suite proxy configuration through FoxyProxy in Firefox._

This was one of the most important parts of the lab because proxying traffic is a foundational skill for web application security testing. It gives visibility into what the browser is actually sending to the application, which is often where security issues become easier to understand.

---

## Why This Setup Matters

Before performing any type of security assessment, it is important to have a stable and organized testing environment. If the tools are not configured correctly, it becomes harder to trust scan results or troubleshoot issues later.

This lab helped me build a foundation in:

- Kali Linux administration
- Web proxy configuration
- Vulnerability scanner setup
- Web application scanning
- Directory enumeration
- Offensive security workflow planning

---

## Lessons Learned

The biggest takeaway from this project was that tools are only useful when you understand their purpose. Burp Suite, ZAP, Nikto, DirBuster, Nessus, and Greenbone all support different parts of the assessment process. Some are better for web traffic analysis, some for directory discovery, and others for vulnerability prioritization.

This lab also helped me see how penetration testing workflows are built in layers. Before exploitation or advanced testing, there has to be a reliable environment, working tools, and an understanding of what each tool contributes.

---

## Final Thoughts

This project was the starting point for the rest of my Information Security coursework. By configuring Kali Linux and validating the offensive security tools, my team was prepared for later labs involving network scanning, vulnerability assessment, enumeration, exploitation, and the final Westfin red team capstone.

Looking back, this was a simple but important step. A strong technical environment makes the rest of the assessment process smoother and allows more time to focus on methodology, findings, and defensive takeaways.
