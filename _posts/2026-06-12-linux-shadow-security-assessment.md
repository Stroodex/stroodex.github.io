---
title: "Writing My First Linux Security Assessment Report Under Pressure"
date: 2026-06-12 12:00:00 -0700
categories: [Portfolio]
tags: [linux, password-security, security-assessment, risk-analysis, consulting]
image:
  path: /assets/img/posts/linux-shadow-assessment-banner.png
---

# Writing My First Linux Security Assessment Report Under Pressure

This project was different from the other labs I have written about on my portfolio.

Instead of being a normal class assignment, this was a timed security assessment report I completed for an interview process for a cybersecurity consulting role. I had 24 hours to review the scenario, analyze the findings, write the report, explain the business risk, and provide remediation guidance.

What made it more challenging was the timing. This assignment landed during one of the busiest weeks of my semester, when I had three midterms and three projects due at the same time. Because of that, this report became a test of both technical understanding and time management.

The assessment focused on Linux server password security, specifically around the `/etc/shadow` file and several account security controls that could affect the confidentiality and integrity of a production Linux system.

![Security Assessment Cover](/assets/img/posts/linux-shadow-report-cover.png)

*Figure 1: Cover page from the Linux server password file security assessment.*

## Why I Wanted to Include This Project

I wanted to include this on my portfolio because it shows a different side of cybersecurity compared to my red team labs.

The Westfin projects focused heavily on offensive security methodology, exploitation, and attack paths. This report was more focused on **security consulting**, where the goal was to identify risk, explain findings clearly, and provide realistic remediation steps.

That distinction matters because in consulting, the technical issue is only part of the job. The other part is communicating why the issue matters to a client and how it can be fixed.

## Assessment Focus

The report focused on a Linux server named `prod01` and reviewed the security posture of the `/etc/shadow` file. This file stores password hash information and account policy settings for local users on Linux systems.

The main areas reviewed included:

- Weak administrative password risk
- Use of an outdated MD5 password hashing algorithm
- Lack of account lockout policy
- Password aging policy concerns
- Access control around `/etc/shadow`

Each finding was assigned a risk score and written in a format that included a description, how the issue was identified, remediation steps, and prevention recommendations.

## Finding 1 — Weak Administrative Password

The highest risk finding involved a weak password associated with the administrative account.

From a consulting perspective, this is one of the easiest findings to explain to a client because the risk is direct. If an attacker can guess or crack a privileged password, they may gain administrative control of the system.

In the report, I rated this as a critical issue because administrative accounts have the highest level of access. A weak root password can lead to unauthorized access, privilege misuse, data exposure, and complete server compromise.

![Weak Password Finding](/assets/img/posts/linux-shadow-weak-password.png)

*Figure 2: Weak password finding and remediation evidence.*

The remediation focused on immediately changing the administrative password and enforcing stronger password complexity requirements. I also recommended password managers, regular password audits, and multi-factor authentication for administrative access.

Looking back, this finding helped me understand how important it is to explain risk in plain language. It is not enough to say "the password is weak." A good report should explain what an attacker could do with that weakness and what the organization should do next.

## Finding 2 — MD5 Password Hashing

The second major finding involved the use of MD5 for password hashing.

In the `/etc/shadow` format, the hash identifier can reveal which hashing algorithm is being used. In this case, the `$1$` marker indicated MD5. MD5 is considered outdated for password storage because it is fast and much more practical to attack with modern cracking tools compared to stronger password hashing approaches.

![MD5 Hash Finding](/assets/img/posts/linux-shadow-md5-hash.png)

*Figure 3: MD5 password hashing finding and suggested remediation.*

This was another critical finding because even a stronger password is weakened when stored with an outdated hashing algorithm. The remediation recommended moving to a stronger algorithm such as SHA-512 and ensuring future passwords are stored using modern hashing methods.

This finding taught me that password security is not only about user behavior. System configuration also matters. Even if users are told to create strong passwords, the underlying system still needs to store them securely.

## Finding 3 — Missing Lockout Policy

Another important issue involved the lack of an account lockout policy.

Without lockout controls, an attacker may be able to make repeated login attempts without the account being temporarily disabled. This increases exposure to brute-force attempts and password guessing attacks.

![Lockout Policy Finding](/assets/img/posts/linux-shadow-lockout-policy.png)

*Figure 4: Missing lockout policy finding and PAM-based remediation guidance.*

The remediation focused on configuring PAM controls to lock accounts after repeated failed login attempts. I also recommended documenting baseline account security policies and using configuration management tools to keep settings consistent across systems.

This finding stood out to me because it connected directly to real-world security operations. Failed login spikes, brute-force attempts, and account lockouts are common signals that SOC teams monitor. A missing lockout policy does not only increase risk; it also makes suspicious activity harder to contain.

## Finding 4 — Password Aging Policy

The next finding involved the password aging policy.

The value in the `/etc/shadow` file indicated that the password was set to remain valid for an extremely long period of time. In practical terms, this meant the password was effectively configured to never expire.

![Password Aging Finding](/assets/img/posts/linux-shadow-password-aging.png)

*Figure 5: Password aging policy finding and recommended configuration change.*

I rated this as a medium-level issue because password expiration policies can vary depending on the organization, but having no password lifecycle management at all can still increase risk. If credentials are exposed or reused, they may remain valid indefinitely.

The remediation recommended setting a reasonable password maximum age and reviewing system configuration files to ensure password aging policies are consistently enforced.

## Finding 5 — `/etc/shadow` Access Review

The final finding involved access to the `/etc/shadow` file.

In the assessment, the file was accessed from an administrative account, so this was not treated as an active critical vulnerability. However, I still documented it because improper permissions on `/etc/shadow` would be extremely serious if non-privileged users could read it.

![Shadow File Permissions](/assets/img/posts/linux-shadow-file-permissions.png)

*Figure 6: File permission review for `/etc/shadow`.*

The remediation focused on verifying correct permissions and ownership so that only authorized system accounts can access the file. I also recommended regular permission reviews and monitoring for unexpected changes.

This finding reminded me that not every observation needs to be critical to be worth documenting. Sometimes lower-risk findings still provide value because they help confirm whether important controls are in place.

## What I Learned from Writing This Report

The most valuable part of this assignment was learning how to write for a consulting audience.

A technical report cannot just list vulnerabilities. It needs to answer:

- What is the issue?
- Why does it matter?
- How was it found?
- What is the business or operational risk?
- What should be done immediately?
- How can the issue be prevented in the future?

Because I only had 24 hours and was also dealing with midterms and projects, I had to focus on being direct and organized. I learned that a clear structure matters a lot when time is limited.

## How I Would Improve It Now

Looking back, there are a few things I would improve if I were writing this report again.

First, I would make the executive summary stronger and more concise. I would separate technical detail from business impact more clearly so that a non-technical reader could quickly understand the highest-risk items.

Second, I would include a cleaner risk ranking table at the beginning of the report. This would help the reader immediately see which findings require urgent action.

Third, I would better distinguish between confirmed vulnerabilities and observations. For example, administrative access to `/etc/shadow` is expected when using root, but it is still worth validating that permissions are properly configured.

Finally, I would make remediation steps more specific to modern Linux distributions, since commands and configuration files may vary depending on the system.

## Key Takeaways

This project helped me grow in a few important ways:

- I practiced writing under a real deadline.
- I learned how to structure findings for a consulting-style report.
- I strengthened my understanding of Linux password security.
- I practiced explaining risk in a way that connects technical findings to business impact.
- I learned that clear communication is just as important as technical analysis.

## Final Thoughts

This report was one of my first experiences writing a security assessment for something outside of a normal class lab. Even though the timing was stressful, it gave me a better appreciation for what cybersecurity consultants are expected to do: analyze quickly, communicate clearly, and provide recommendations that are realistic for the client.

It also showed me that smaller security reviews can still teach important lessons. A single Linux password file can reveal a lot about password policy, hashing standards, account lockout controls, and administrative access practices.

For me, this project was less about producing a perfect report and more about learning how to think and write like a security consultant under pressure.
