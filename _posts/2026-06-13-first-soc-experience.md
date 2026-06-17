---
title: "What I Learned from My First Experience Working in a Security Operations Center"
date: 2026-06-13 12:00:00 -0700
categories: [Portfolio]
tags: [soc, incident-response, blue-team, phishing-analysis, crowdstrike, guardduty]
---

# What I Learned from My First Experience Working in a Security Operations Center

Before working in a Security Operations Center, I always imagined the SOC as a high-stress environment where security incidents were happening constantly and analysts were responding to threats every minute of the day.

Going into my first SOC experience, I honestly felt nervous. I knew I was interested in cybersecurity, but being placed in a real operational environment felt different from doing labs, coursework, or personal projects. This was no longer just practice. These were real alerts, real users, real tickets, and real investigations.

On my first day, I already felt a little out of place because I did not even have access to get into the SOC room yet. It was a small thing, but it made the experience feel even more real. I was stepping into a professional environment where I had to learn the workflow, understand the tools, and earn my place on the team.

Over time, that nervousness started to go away. Within the first week, I began getting situated with my team, learning the environment, and building relationships with my manager, senior manager, and the analysts around me. Having a supportive team made a huge difference and helped me feel like the SOC was the right place for me to start my cybersecurity career.

## Learning the Basics of SOC Triage

During my first two weeks, I focused mainly on learning how to triage common security tickets.

Most of the early work involved reviewing and closing out:

- Phishing emails
- Spam emails
- Security awareness training emails
- Suspicious user-reported messages

At first, even these tickets felt intimidating because I was still learning what to look for. I had to understand the difference between a real phishing attempt, general spam, a benign email, and internal security awareness training.

Some of the tools I used during this stage included:

- ServiceNow
- Proofpoint
- FortiGuard
- Palo Alto URL filtering
- WHOIS lookups
- A Windows virtual machine for investigation

This was where I started learning that SOC work is not just about using tools. It is about building a repeatable process. For each email, I had to review indicators, check links, inspect sender details, understand the context, and document the result clearly.

Over time, I started getting faster and more confident. Many of the Level 1 email tickets could be cleared quickly once I understood the pattern, but there were always some that required more careful investigation.

## Moving Into More Advanced Alerts

After the first few weeks, I started learning how to handle more complicated detections.

These included:

- Endpoint detections from CrowdStrike Falcon
- AWS GuardDuty alerts
- Lost and stolen device cases
- Suspicious activity requiring additional investigation

This was where the work became even more interesting to me. I started using tools such as:

- CrowdStrike Falcon
- Splunk
- AWS GuardDuty
- ServiceNow
- Internal IT asset and user lookup pages

At first, I needed more guidance from the senior analysts, which was expected. I was still new and learning how to properly investigate detections without jumping to conclusions.

But over time, I started becoming more independent. Eventually, I was able to work through endpoint and AWS tickets on my own and then have a senior analyst review my analysis before the ticket was closed.

That was one of the moments where I felt myself improving. I was not just clicking through tickets anymore. I was starting to understand what the alerts meant, what evidence mattered, and how to explain my reasoning.

## What Surprised Me About SOC Work

The thing that surprised me the most was how much of SOC work involved normal employees and everyday security events.

Before working in the SOC, I imagined constant major incidents and advanced attackers. In reality, a large portion of Level 1 work involved phishing emails, spam, security awareness tickets, suspicious links, and user-reported activity.

Most tickets could be handled fairly quickly once I understood the workflow, but there were always some that required deeper investigation. That 10% of tickets was where I learned the most.

I also realized that cybersecurity is not only about stopping “bad guys.” A lot of the job is understanding user behavior, business context, and whether something is truly malicious or just unusual.

## Investigating Endpoint Detections

One type of alert I found especially interesting was endpoint detection.

For example, when reviewing certain detections, I noticed that activity involving drives such as `D:\\` or `E:\\` could sometimes indicate that a USB device had been plugged into the system. In some cases, the detection was related to malware being identified on removable media.

That type of alert taught me how important context is during endpoint investigations.

Instead of only asking, “Was malware detected?” I had to think through questions like:

- Where did the file come from?
- Was it on the local disk or removable media?
- What user was involved?
- Was the file executed or only detected?
- Was the threat quarantined or remediated?
- Is additional user follow-up needed?

This helped me understand how endpoint investigations connect technical evidence with real-world user activity.

## Learning the Human Side of Cybersecurity

One of the biggest lessons I learned was that real-world cybersecurity has to be written and communicated for people.

In school and labs, it is easy to focus only on the technical side. But in a SOC, many investigations involve users, managers, IT teams, or other groups that may not understand technical security language.

That means communication matters a lot.

When documenting tickets or responding to users, I had to learn how to explain security findings clearly and professionally. Not everything should be written in overly technical terms. Sometimes the best response is one that explains the risk in a simple way while still being accurate.

This was a huge learning experience for me because it showed that cybersecurity is not only technical. It is also about communication, teamwork, and helping the business understand what is happening.

## Working with a Team

Another major part of my SOC experience was learning from the people around me.

Having a good team made the experience much better. I was able to ask questions, listen to how senior analysts explained investigations, and learn from their thought process.

I also learned how important it is to build relationships with coworkers. Some of the most valuable learning moments came from normal conversations, quick questions, and coffee chats with people who had more experience than me.

Being surrounded by people who were willing to help made me realize how much knowledge exists outside of textbooks, classes, and labs.

## What I Got Better At

Over the course of the experience, I improved in several areas:

- Understanding SOC ticket workflows
- Identifying phishing and spam patterns
- Reviewing endpoint detections
- Investigating AWS GuardDuty alerts
- Writing clearer ticket notes
- Knowing when to escalate
- Asking better questions
- Thinking through user impact
- Communicating findings in a professional way

I was not perfect when I started, but that was part of the process. Every ticket helped me improve a little more.

The more alerts I worked on, the more I started recognizing patterns. Things that felt confusing in the beginning slowly became familiar.

## Advice for Other Students Starting in a SOC

My biggest advice to another student starting in a SOC is to be a sponge.

Absorb as much information as possible. Ask questions. Take notes. Watch how senior analysts investigate. Pay attention to how they write tickets, how they explain findings, and how they decide whether something is benign or suspicious.

School, books, and labs are important, but they cannot fully replace being in a real environment with real alerts and experienced people around you.

If you get the opportunity to work in a SOC, make the most of it. Talk to your coworkers. Ask for coffee chats. Learn about their career paths. Most people have valuable advice and experiences that can help you grow faster.

## Final Thoughts

My first SOC experience showed me that cybersecurity is much more than tools and alerts.

It is about investigation, communication, teamwork, curiosity, and constantly learning. Some tickets were simple, some were confusing, and some required deeper analysis, but every one of them helped me understand what real-world security operations looks like.

Looking back, I feel lucky that my first cybersecurity role gave me the chance to work with a supportive team and learn in a real SOC environment. It helped confirm that this is the field I want to continue growing in.

What started as something intimidating became one of the best learning experiences I have had so far in cybersecurity.
