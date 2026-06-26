---
title: "Digital Forensics Final Lab: Correlating Evidence Across Workstations and a Server"
date: 2026-12-05 12:00:00 -0700
categories: [Portfolio]
tags: [digital-forensics, autopsy, encase, ftk-registry-viewer, windows-forensics, incident-response]
image:
  path: /assets/img/posts/psc-forensics-banner.png
---

# Digital Forensics Final Lab: Correlating Evidence Across Workstations and a Server

This project was the final lab for my digital forensics coursework, and it felt like the point where everything I had learned finally came together.

Unlike my first forensic lab, where I was still learning the basics of FTK, Registry Viewer, RegRipper, and Windows artifacts from scratch, this project required a more complete investigation process. Instead of analyzing one image, my team had to investigate a simulated company environment involving two employee workstations and one file server.

The company in this lab was fictional and used only for educational purposes, but the format of the project was designed to simulate a real forensic report. We had to preserve evidence integrity, examine multiple forensic images, document findings, and explain how the artifacts supported the overall case.

![PSC Report Cover](/assets/img/posts/psc-report-cover.png)

*Figure 1: Cover page from the simulated digital forensics report.*

## Project Scope

The lab involved three forensic evidence images:

- A workstation image for one employee
- A workstation image for a second employee
- A server image from the simulated company environment

The goal was to investigate each image and identify evidence related to user activity, email communication, web browsing, installed programs, deleted files, and possible attempts to remove data.

This was much more realistic than looking at a single artifact in isolation. The challenge was learning how to connect evidence across different systems.

## Evidence Integrity

Before reviewing any artifacts, my team verified the forensic images using MD5 hash values.

This step was important because digital forensics depends heavily on evidence integrity. If an evidence image cannot be verified, it becomes harder to trust the results of the investigation.

![Evidence Integrity](/assets/img/posts/psc-evidence-integrity.png)

*Figure 2: Evidence image verification and MD5 hash validation.*

This was one of the biggest lessons from the lab. In normal cybersecurity projects, the focus is often on finding the answer. In forensics, the process used to reach the answer matters just as much.

## Tools Used

This project required using multiple forensic tools together, including:

- **Autopsy**
- **EnCase Acquisition**
- **FTK Registry Viewer**

Autopsy helped with reviewing forensic images, browsing the file system, and identifying web artifacts. EnCase Acquisition was used to support evidence acquisition and verification. FTK Registry Viewer was used to analyze Windows registry hives and system artifacts.

Using several tools made the investigation more reliable because each tool provided a different view of the evidence.

## Workstation One: Email and Web Activity

One of the major parts of the investigation involved reviewing email and web activity on the first workstation.

The report included Outlook evidence showing communication between the two employees. It also contained web artifacts such as Temporary Internet Files, cookies, and browsing history that supported the timeline of activity.

![Outlook Email Evidence](/assets/img/posts/psc-outlook-email-evidence.png)

*Figure 3: Email evidence recovered from the first workstation.*

This section helped me understand how email artifacts can become central evidence in a forensic investigation. A message by itself may not tell the entire story, but it can become much stronger when supported by browser history, cookies, and cached web pages.

## Temporary Internet Files

Temporary Internet Files were especially useful because they showed web content that had been cached on the workstation.

The report included artifacts showing travel-related browsing activity and cached pages connected to trip planning.

![Temporary Internet Files](/assets/img/posts/psc-temp-internet-files.png)

*Figure 4: Temporary Internet Files showing cached web activity.*

This taught me that browser artifacts are not limited to URLs. Cached pages, saved web content, and temporary files can reveal what the user was viewing and sometimes preserve content that is no longer easily accessible.

## Cookies and Web History

Cookies and web history helped support the browsing timeline.

The report documented cookies connected to websites such as Expedia, Passport, Hotmail, and other web services. Web history also showed repeated access to travel-related URLs.

![Web Cookies](/assets/img/posts/psc-web-cookies.png)

*Figure 5: Web cookie evidence recovered during analysis.*

![Web History](/assets/img/posts/psc-web-history.png)

*Figure 6: Web history evidence showing visited URLs.*

This part of the lab helped me understand why forensic analysts look at multiple browser artifacts instead of relying on one source. Cookies can support account activity, history can show visited pages, and cached files can preserve the page content itself.

## Installed Programs and Eraser Evidence

One of the most important findings involved evidence of **Eraser v5.7**, a disk-wiping utility.

The tool appeared in installed program artifacts and scheduler logs. The presence of a disk-wiping tool does not automatically prove wrongdoing, but in the context of the investigation, it became an important artifact because it suggested possible attempts to remove or overwrite data.

![Eraser Installed](/assets/img/posts/psc-eraser-installed.png)

*Figure 7: Installed program evidence related to Eraser v5.7.*

![Eraser Schedule Log](/assets/img/posts/psc-eraser-schedlog.png)

*Figure 8: Scheduler log evidence related to Eraser activity.*

This was one of the more interesting findings because it showed how tool artifacts can remain even when a user may be trying to hide activity. Installation traces, logs, deleted files, and browser history can all preserve parts of the story.

## Workstation Two: Corroborating Evidence

The second workstation helped support findings from the first system.

It contained email conversation artifacts, travel-related temporary internet files, and additional evidence related to Eraser v5.7. This made the case stronger because similar themes appeared across multiple systems.

![Second Workstation Email Evidence](/assets/img/posts/psc-leslie-email-evidence.png)

*Figure 9: Email conversation artifact from the second workstation.*

![Travel Artifacts](/assets/img/posts/psc-travel-artifacts.png)

*Figure 10: Travel-related browsing evidence from the second workstation.*

Finding related activity on both workstations helped me understand the value of corroboration. One artifact can be useful, but multiple artifacts across different machines can make a finding much stronger.

## Server Image Analysis

The server image added another layer to the investigation.

The report documented evidence of Eraser v5.7 download activity inside browser history and deleted files connected to the same tool. This connected the server to the broader investigation and showed that relevant artifacts were not limited to the two workstations.

![Server Eraser Download](/assets/img/posts/psc-server-eraser-download.png)

*Figure 11: Server browser history showing Eraser download activity.*

![Deleted Eraser Files](/assets/img/posts/psc-deleted-eraser-files.png)

*Figure 12: Deleted file evidence related to Eraser.*

This helped me understand why forensic investigations often expand beyond the first endpoint. Important context may exist on servers, shared systems, or other devices connected to the environment.

## Correlating Evidence Across Systems

The strongest part of this lab was learning how to correlate evidence across multiple machines.

The investigation combined:

- Email artifacts
- Temporary Internet Files
- Web cookies
- Web history
- Installed program evidence
- Scheduler logs
- Deleted files
- Server download history

Each artifact answered part of the question. Together, they created a more complete timeline.

This is where the project started feeling more like real digital forensics. The goal was not just to find screenshots. The goal was to explain how the evidence connected.

## What I Learned

This lab helped me grow a lot from my first digital forensics assignment.

I learned that digital forensics is not just about finding files. It is about preserving evidence, validating artifacts, documenting findings, and building a timeline that can be explained clearly.

Some of my biggest takeaways were:

- Evidence integrity comes first.
- Browser artifacts can reveal a lot about user activity.
- Temporary Internet Files can preserve useful cached content.
- Cookies can support account and browsing activity.
- Installed program artifacts can show tools used on a system.
- Deleted files may still leave evidence behind.
- Multiple systems can support the same conclusion.
- A strong report needs both technical evidence and clear explanation.

## Challenges

The hardest part of this project was organization.

There were multiple images, multiple systems, many artifacts, and a lot of screenshots. It was easy to get lost in the details if the report was not structured carefully.

Another challenge was explaining why each artifact mattered. A screenshot is not useful by itself unless it supports a conclusion. This lab forced me to think more like an examiner and less like someone simply collecting evidence.

## How This Connects to SOC and Incident Response

This project connects directly to SOC and incident response work.

In a SOC, an alert might only show one piece of suspicious activity. To understand what really happened, analysts may need to review logs, endpoint activity, user behavior, browser artifacts, file activity, and evidence from other machines.

Digital forensics helps answer questions like:

- Which user was involved?
- What system did the activity happen on?
- What tools were installed?
- What files were accessed or deleted?
- Was there related activity on other systems?
- Does the evidence support the timeline?

This lab helped me see how forensic thinking can make incident response stronger.

## Final Thoughts

This final digital forensics lab was one of the most useful projects I completed because it required me to bring together multiple tools and investigation methods.

I started the course learning the basics of registry analysis and artifact review. By the end, I was analyzing multiple systems, validating evidence, reviewing email and web artifacts, identifying installed tools, and connecting findings across workstations and a server.

Most importantly, this project taught me that strong forensic work is about building a clear story from scattered evidence.
