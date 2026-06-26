---
title: "Digital Forensics: My First Registry and Artifact Investigation"
date: 2025-10-28 12:00:00 -0700
categories: [Portfolio]
tags: [digital-forensics, registry-analysis, ftk, regripper, incident-response, windows]
image:
  path: /assets/img/posts/digital-forensics-banner.png
---

# Digital Forensics: My First Registry and Artifact Investigation

This project was my first real experience working through a digital forensics investigation from scratch. This porject was assigned to me in my computer foresnics class and the instructions and tools given were very vague and tools used were not taught but instead we were instructed to play around with it to learn how it works. 

Before this lab, I had heard of forensic artifacts like registry hives, user profiles, browser history, and USB device traces, but I had not fully understood how investigators could use them to reconstruct what happened on a system. This assignment forced me to slow down, learn the tools (FTK Plus, Encase, and Autopsy), and connect small pieces of evidence into a larger story.

The investigation was based on a forensic image named `ID THEFT.E01`. The objective was to analyze the evidence file and answer questions about the suspect's system, user activity, connected USB devices, browser artifacts, account information, and files that could support criminal activity.

What made this project valuable was that I had to learn almost everything as I went. I used tools such as **FTK Imager/FTK**, **AccessData Registry Viewer**, and **RegRipper** to extract and analyze Windows forensic artifacts.

## Tools Used

The main tools I worked with were:

- FTK / FTK Imager
- AccessData Registry Viewer
- RegRipper
- PowerShell
- Windows Registry hives
- Keyword search and file system analysis

Each tool served a different purpose. FTK helped me load and search the evidence image, Registry Viewer helped me inspect exported registry hives, and RegRipper helped parse registry data into a more readable format.

## Understanding the Evidence Image

The first step was creating a case and loading the forensic image into FTK. From there, I navigated through the evidence tree and began exporting registry hives for analysis.

This was one of the first things I learned: forensic analysis often starts with knowing where evidence lives. The data is not always obvious. It may be stored inside registry hives, user profiles, hidden folders, timestamps, or application artifacts.

## Registry Analysis: SYSTEM Hive

The first major section focused on the Windows SYSTEM hive.

One of the first tasks was determining the current control set. Using Registry Viewer, I navigated to the `Select` key and identified that the current control set was **ControlSet001**.

![Current Control Set](/assets/img/posts/df-current-control-set.png)

*Figure 1: Determining the current control set using AccessData Registry Viewer.*

This mattered because many system-level artifacts depend on the active control set. Once I knew which control set was current, I could focus my analysis on the correct paths.

## Time Zone Analysis

Another important forensic detail was the system's time zone information.

The SYSTEM hive showed a bias value of **420**, which corresponded to Mountain Standard Time / UTC-7. This was important because timestamps in forensic tools are often shown in UTC, but investigators need to convert them correctly when building a timeline.

![Time Zone Artifact](/assets/img/posts/df-timezone.png)

*Figure 2: Time zone information from the SYSTEM hive.*

This was one of the first moments where I realized how easy it is to make a mistake in forensics. A timestamp can be technically correct but still misleading if it is not converted to the correct local time.

## USB Device Artifacts

One of the most interesting parts of the lab involved identifying portable storage devices that had been connected to the suspect's computer.

By reviewing the `USBSTOR` registry key, I was able to identify several connected storage devices, including:

- USB DISK USB Device
- Apacer HandyDrive USB Device
- SanDisk ImageMate II USB Device

![USB Artifacts](/assets/img/posts/df-usb-artifacts.png)

*Figure 3: USBSTOR artifacts showing connected storage devices.*

![USB Friendly Names](/assets/img/posts/df-usb-friendly-names.png)

*Figure 4: Friendly names and device details for connected USB devices.*

This section helped me understand why USB artifacts are so useful in investigations. If a suspect is believed to have used removable media, Windows registry artifacts can provide evidence that a device was connected to the machine.

## Computer Name Evidence

The investigation also required identifying evidence that supported the computer name referenced in event logs.

Inside the SYSTEM hive, I reviewed the ComputerName registry key and found that the system name was listed as **KAL**.

![Computer Name Artifact](/assets/img/posts/df-computer-name.png)

*Figure 5: Registry evidence showing the computer name.*

This type of artifact can help connect event logs, network activity, and a physical system together. It also showed me how registry evidence can corroborate other evidence sources.

## SAM Hive and User Account Analysis

The next phase involved analyzing the SAM hive to identify local user accounts and security identifiers.

Using RegRipper's `samparse` plugin, I extracted user account information and reviewed account details such as usernames, SIDs, login counts, account status, and password-related attributes.

![SAM Registry Parsing](/assets/img/posts/df-sam-registry.png)

*Figure 6: RegRipper output from the exported SAM hive.*

One account of interest was **ID THEFT DUDE**. The analysis showed account attributes including the last login time, account status, failed login count, and whether a password was required.

![ID THEFT DUDE Login](/assets/img/posts/df-id-theft-dude-login.png)

*Figure 7: Account details and login information for the user account of interest.*

This was a major learning point for me because it showed how user account artifacts can help build a timeline and identify which account was active on a system.

## NTUSER.DAT and User Activity

The NTUSER.DAT hive was especially important because it contains user-specific registry data.

By analyzing NTUSER.DAT, I was able to identify artifacts related to user activity, including media file references, printer information, email account evidence, browser settings, and typed URLs.

One portion of the lab involved searching for evidence of specific MP3 files. Keyword searches in FTK revealed file paths pointing to audio files stored on a `D:\Music from WV\` path.

![MP3 Artifacts](/assets/img/posts/df-ntuser-mp3-artifacts.png)

*Figure 8: Keyword search results showing user-specific MP3 artifacts.*

This helped me understand that even if a file is no longer directly visible in the file system, references to it may still exist in user registry artifacts.

## Printer Artifact

Another artifact involved identifying the printer used by the suspect.

Inside NTUSER.DAT, the Printers key showed evidence of an **HP Deskjet 3820 series** printer.

![Printer Artifact](/assets/img/posts/df-printer-artifact.png)

*Figure 9: Printer artifact found inside NTUSER.DAT.*

This was interesting because it showed how the registry can link user activity to physical devices. In an investigation, printer artifacts could help connect printed documents back to a specific computer or user profile.

## Email and Browser Artifacts

The investigation also included evidence of an email account connected to a suspicious site. Keyword searching revealed artifacts related to an email address and account configuration information.

![Email Artifact](/assets/img/posts/df-email-artifact.png)

*Figure 10: Keyword search results showing email-related artifacts.*

I also reviewed Internet Explorer artifacts such as the browser homepage and typed URLs. The TypedURLs key contained several URLs that helped reconstruct the user's browsing activity.

![Typed URLs](/assets/img/posts/df-typed-urls.png)

*Figure 11: Internet Explorer TypedURLs registry evidence.*

This section made me realize how much user behavior can be reconstructed from browser artifacts alone. Typed URLs can show intent, interests, and potential connections to other evidence in the case.

## General File System Evidence

The final phase moved beyond registry analysis and into general evidence review.

Using keyword searches and file inspection, I found documents and image files that supported the investigation scenario. The evidence included references to counterfeit currency, fake identification documents, passport-related files, and credit card information.

![Counterfeit Evidence](/assets/img/posts/df-counterfeit-evidence.png)

*Figure 12: File system evidence related to counterfeit currency.*

![Passport Evidence](/assets/img/posts/df-passport-evidence.png)

*Figure 13: Evidence related to counterfeit passport and identification files.*

![Credit Card Evidence](/assets/img/posts/df-credit-card-evidence.png)

*Figure 14: Document evidence related to credit card information theft.*

This part of the lab helped me connect registry analysis with file system analysis. The registry helped establish user activity and system context, while the files provided direct supporting evidence for the case scenario.

## What I Learned

The biggest lesson from this project was that digital forensics is about connecting artifacts.

No single artifact tells the entire story. Instead, evidence comes from multiple places:

- SYSTEM hive
- SAM hive
- NTUSER.DAT
- USBSTOR keys
- Browser artifacts
- User account metadata
- File system evidence
- Keyword search results
- Timestamps

When combined, these artifacts can help reconstruct what happened on a system.

I also learned that forensic work requires patience. Unlike some cybersecurity labs where the goal is to quickly get a result, forensics requires careful documentation, validation, and explanation.

## Challenges I Faced

Since this was my first time using many of these tools, the hardest part was learning where to look.

At first, registry paths felt confusing. I had to learn what each hive was used for and why certain artifacts lived in certain locations. I also had to get used to switching between FTK, Registry Viewer, and RegRipper depending on what I was trying to answer.

Another challenge was working with timestamps. Understanding UTC, local time conversion, and time zone bias made me realize how important accuracy is when building forensic timelines.

## How This Connects to SOC and Incident Response

This lab connected directly to the type of work that matters in SOC and incident response.

In a real incident, analysts often need to answer questions such as:

- Which user account was involved?
- What devices were connected?
- What files were accessed?
- What websites were visited?
- What applications or artifacts support the timeline?
- What evidence confirms or disproves suspicious activity?

Digital forensics provides a deeper level of evidence than alert triage alone. It helps investigators move from "something happened" to "this is what happened, when it happened, and what artifacts support it."

## Final Thoughts

This project gave me my first real look into digital forensics and Windows artifact analysis.

It taught me how much information can be recovered from registry hives and forensic images, even when the evidence is scattered across different parts of the system. More importantly, it helped me understand the importance of documenting findings clearly and supporting conclusions with evidence.

Learning these tools from scratch was challenging, but it made the project more rewarding. By the end of the lab, I had a much better understanding of how forensic analysts use system artifacts to reconstruct user activity and support an investigation.
