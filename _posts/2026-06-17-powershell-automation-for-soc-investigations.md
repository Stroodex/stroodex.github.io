---
title: "PowerShell Automation for SOC Investigations"
date: 2026-06-23 12:00:00 -0700
categories: [Portfolio]
tags: [powershell, soc, automation, endpoint-security, incident-response, crowdstrike]
image:
  path: /assets/img/posts/powershell-soc-automation-banner.png
---

# PowerShell Automation for SOC Investigations

One of the most valuable projects I worked on in a SOC environment was a PowerShell automation idea that came from noticing a real workflow bottleneck during endpoint investigations.

When an endpoint detection requires network containment, the security team has to act quickly. Containing the device can help stop potential malicious activity from spreading, but it can also create a communication problem. If the user only has that one device and does not have a mobile device available, they may suddenly lose internet access without understanding why.

Before this automation idea, the workaround was to contact the user's manager and explain the situation. That process worked, but it added extra steps during an investigation where time matters.

The goal of this project was simple: create a way to communicate directly on the contained device itself by changing the desktop wallpaper to a clear message explaining that the SOC is conducting an investigation and that the device is under network containment.

This post explains the problem, the workflow I built, how I tested the script safely in a virtual machine, and what I learned from turning a small idea into a SOC automation improvement.

## The Problem I Wanted to Solve

During endpoint investigations, network containment can be necessary when there is concern about malware, suspicious execution, or activity that needs to be isolated from the network.

The problem is that containment can also confuse the user.

From the user's perspective, their device may suddenly lose connectivity. If they cannot access email, Teams, Slack, or the internet, they may not know whether their device is broken, whether the network is down, or whether security is investigating something.

That communication gap creates a bottleneck for the SOC because analysts may need to contact the user's manager or another support channel while also continuing the investigation.

I wanted to reduce that bottleneck by displaying a visible message on the endpoint itself.

## The Automation Idea

The idea was to use PowerShell to temporarily replace the user's desktop wallpaper with a SOC notification message.

The message would communicate something similar to:

> The SOC is currently conducting an investigation on this device. This device may be under network containment. Please contact the SOC if you have any questions or concerns.

The key part was that this should not be a permanent change. The script needed to support both deployment and restoration.

That meant the automation needed to:

- Back up existing wallpapers
- Change the wallpaper for all user profiles
- Adjust sleep settings so the device stays awake during investigation
- Restore the original sleep settings later
- Restore each user's original wallpaper after the investigation
- Create logs so the actions could be reviewed
- Work safely in a test environment before being considered for production use

## Breaking the Project Into Smaller Functions

At first, I did not try to build everything at once.

I broke the project into separate parts:

1. Backup current wallpapers
2. Disable sleep settings
3. Deploy the SOC notification wallpaper
4. Restore original sleep settings
5. Restore original wallpapers
6. Combine everything into a larger workflow

This made the project easier to troubleshoot because I could test each function individually before combining them.

## Backing Up User Wallpapers

The first function focused on backing up the existing desktop wallpapers.

The script created a backup folder in `C:\Temp\WallpaperBackups` and saved the wallpaper for each user profile. This was important because different users on the same endpoint may have different wallpaper settings.

![Wallpaper Backup](/assets/img/posts/powershell-wallpaper-backup.jpg)

*Figure 1: PowerShell script backing up wallpapers for multiple user profiles in a test virtual machine.*

This backup step mattered because I did not want the automation to solve one problem while creating another. If the SOC message was deployed, there needed to be a reliable way to undo the change.

## Disabling Sleep Settings During Investigation

The second function focused on sleep settings.

If a device goes to sleep during an investigation, that can slow down analysis or interrupt actions that need to run. Because of that, I created a script that captured the original sleep configuration and then temporarily changed the system sleep behavior.

![Disable Sleep Settings](/assets/img/posts/powershell-disable-sleep-settings.jpg)

*Figure 2: PowerShell script saving original sleep settings and disabling sleep behavior for investigation purposes.*

The important part was saving the original configuration before making changes. This allowed the restore script to bring the system back to its previous state once the investigation was finished.

## Deploying the SOC Notification Wallpaper

The next function applied the SOC notification wallpaper to the system and user profiles.

The script copied the wallpaper image into a system location, processed available user profiles, and triggered a wallpaper refresh where possible.

![Wallpaper Deployment](/assets/img/posts/powershell-wallpaper-deployment.jpg)

*Figure 3: PowerShell deployment script setting the investigation wallpaper for multiple users.*

This was the main function that addressed the communication gap. Instead of relying only on manager communication, the user would see a clear message directly on the endpoint.

The wallpaper approach was useful because it remained visible even if the device had limited network connectivity.

## Restoring Sleep Settings

After the investigation, the endpoint should not remain in an altered state.

The sleep restoration function reloaded the saved settings and attempted to restore the original power configuration.

![Sleep Settings Restore](/assets/img/posts/powershell-sleep-restore.jpg)

*Figure 4: PowerShell restore script returning saved sleep settings after the investigation workflow.*

This step helped make the workflow safer because it treated the automation as temporary. Any change made for investigation purposes should have a clear rollback path.

## Restoring Original Wallpapers

The final restoration function changed the wallpapers back to the original images that were backed up earlier.

![Wallpaper Restore](/assets/img/posts/powershell-combined-restore.jpg)

*Figure 5: PowerShell restore workflow returning user wallpapers back to their previous state.*

This was one of the most important parts of the project because it showed that the automation could be reversed. For a SOC workflow, reliability matters. Analysts need confidence that a script can both apply the needed change and clean up afterward.

## Combining Everything Into One Workflow

Once the individual functions worked, I started combining them into a larger workflow.

The combined version handled:

- Wallpaper backup
- Sleep setting backup
- Sleep setting modification
- Wallpaper deployment
- Logging
- Restoration support

The goal was to make the process easier for analysts to trigger without having to manually run several separate commands.

This is where the project moved from a basic script into an operational workflow idea.

## Testing in a Virtual Machine

All screenshots shown in this post are from a virtual machine test environment.

Testing in a VM was important because I wanted to validate the workflow without exposing internal systems or sensitive data. It also gave me a safe place to troubleshoot issues, test multiple user profiles, and confirm that backup and restore actions worked correctly.

This helped me learn that scripting for a real SOC workflow is different from writing a script that only works once. The script needs to be repeatable, reversible, and understandable by other analysts.

## Moving Toward Slack Command Integration

After testing the script functions, the next step was seeing how the workflow could fit into the SOC's existing process.

The goal was to have the automation integrated into a Slack command channel so that the process could be triggered more efficiently during endpoint investigations.

This required collaboration with multiple teams:

- **SOC team:** helped refine the idea and identify the operational problem
- **Integration team:** helped with implementation into the command workflow
- **Communication team:** reviewed and approved the message displayed to users

This part of the project taught me that automation is not just technical. It also requires communication, approvals, and collaboration.

## Why This Helped the SOC Workflow

The value of the automation was reducing time lost during containment communication.

Instead of relying only on manager outreach, the endpoint itself could communicate that the device was under investigation. This gave the SOC team more time to focus on analysis and remediation while still keeping the user informed.

The workflow helped with:

- Faster user awareness during network containment
- Less manual communication overhead
- Clearer investigation messaging
- Safer endpoint state changes with backup and restore
- More consistent analyst workflow
- Better use of automation during endpoint response

## What I Learned

This project taught me a lot about practical SOC automation.

The biggest lesson was that useful automation does not always need to be extremely complex. Sometimes the best automation solves a small but annoying bottleneck that analysts deal with repeatedly.

I also learned that reliability matters more than just getting a script to work once. A SOC automation script should have:

- Logging
- Error handling
- Backup and restore logic
- Clear output
- Safe testing
- Minimal user disruption
- Documentation
- A rollback plan

## What I Would Improve Next

If I continued improving this project, I would add:

- More detailed error handling
- Better validation before and after each action
- A central log location for analyst review
- Parameters for different message templates
- A dry-run mode
- Script signing
- More structured output for Slack responses
- Additional checks for offline or unreachable hosts
- Automated confirmation that wallpaper and sleep settings were restored

These improvements would make the workflow more reliable and easier to maintain in a team environment.

## Final Thoughts

This PowerShell automation project was one of my favorite SOC-related projects because it came from a real workflow problem.

It showed me that being a security analyst is not only about clearing tickets. It is also about noticing friction in the process and finding ways to improve it.

By building this script one function at a time, testing it in a virtual machine, and working with different teams to move it toward Slack command integration, I learned how technical automation can support real incident response workflows.

Most importantly, this project helped me understand that small improvements can make a big difference when analysts are working under time pressure during endpoint investigations.
