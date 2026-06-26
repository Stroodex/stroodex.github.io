---
title: "Building My Cybersecurity Homelab to Practice Enterprise Security"
date: 2026-04-21 12:00:00 -0700
categories: [Portfolio]
tags: [homelab, active-directory, windows-server, kali-linux, blue-team, cybersecurity]
image:
  path: /assets/img/posts/cybersecurity-homelab-banner.png
---

# Building My Cybersecurity Homelab to Practice Enterprise Security

One of the most useful projects I built for myself was a cybersecurity homelab.

I started this lab because I wanted more hands-on practice with how IT infrastructure and cybersecurity connect together. Classes and tutorials are helpful, but I wanted an environment where I could make mistakes, troubleshoot problems, break things, fix them, and understand how enterprise systems actually work.

The goal was not to build the most advanced lab right away. The goal was to build a realistic foundation using virtual machines, Windows Server, Active Directory, Windows clients, Linux, Kali Linux, firewall rules, and basic security tools.

This homelab helped me practice both IT administration and blue team thinking, which became useful later when working on red vs blue competitions, SOC investigations, endpoint detections, and security projects.

![Cybersecurity Homelab Banner](/assets/img/posts/cybersecurity-homelab-banner.png)

*Figure 1: Portfolio banner for my cybersecurity homelab project.*

## Why I Built a Homelab

I built this homelab because I wanted to get more comfortable with the systems that cybersecurity teams are responsible for protecting.

A lot of cybersecurity depends on understanding normal behavior first. Before I could properly think about attacks, alerts, or hardening, I needed to understand how systems are built and managed.

Some of my goals were to practice:

- Creating and managing virtual machines
- Configuring Windows Server
- Setting up Active Directory Domain Services
- Creating users, groups, and permissions
- Joining a Windows client to a domain
- Practicing DNS and DHCP concepts
- Understanding Windows Firewall rules
- Working with Linux and Kali Linux systems
- Preparing for red vs blue team competitions
- Learning how attackers may target common services and misconfigurations

The biggest reason I wanted a homelab was to have a safe place to learn. If I misconfigured something, I could troubleshoot it or revert a snapshot without affecting a real environment.

## Lab Environment

I built the environment in VMware Workstation.

The main virtual machines in my lab included:

- Windows Server 2016
- Windows 10 client
- Kali Linux
- Ubuntu Linux

![VMware VM List](/assets/img/posts/homelab-vmware-vm-list.jpg)

*Figure 2: VMware Workstation showing the virtual machines used in my lab environment.*

This setup gave me a small but useful enterprise-style network. The Windows Server acted as the foundation for Active Directory, while the Windows 10 machine acted as a domain-joined client. Kali Linux gave me a place to practice offensive security tools, and Ubuntu gave me another Linux system to configure and test against.

## Windows Server and Active Directory

The most important part of the lab was setting up Windows Server and Active Directory.

I configured the Windows Server as part of a domain environment and used Server Manager to manage roles such as AD DS and DNS.

![Server Manager AD DS](/assets/img/posts/homelab-ad-ds-server-manager.jpg)

*Figure 3: Server Manager showing Active Directory Domain Services in the homelab.*

Active Directory was one of the areas I wanted to understand better because it is so important in enterprise environments. Many real-world attacks target identity systems, domain users, group memberships, and misconfigured permissions.

In the lab, I practiced creating and managing:

- Domain users
- Security groups
- Computer objects
- Administrative permissions
- Domain-joined systems
- Basic account organization

![Active Directory Users](/assets/img/posts/homelab-active-directory-users.jpg)

*Figure 4: Active Directory Users and Computers showing users and groups in the test domain.*

This helped me understand how users and permissions are managed in a Windows domain. It also helped me see why account security and group membership matter so much from a cybersecurity perspective.

## Joining a Windows Client to the Domain

After setting up Active Directory, I connected a Windows 10 virtual machine to the domain.

This helped me practice the relationship between the domain controller and a client machine. Once the Windows 10 system was joined, I could see the computer object inside Active Directory.

![Domain Joined Computer](/assets/img/posts/homelab-domain-joined-computer.jpg)

*Figure 5: Windows 10 client joined to the Active Directory domain.*

This was a useful learning step because domain joining is a basic IT task, but it also connects directly to cybersecurity. If attackers compromise a domain-joined workstation, they may attempt to move laterally, steal credentials, or abuse permissions.

Understanding how the domain is structured helped me better understand what defenders need to monitor.

## Users, Groups, and Permissions

I also practiced creating users and assigning them to different groups.

Some users were placed into standard groups, while others were given elevated permissions for testing. This helped me understand how privileges change what a user can do inside a domain.

![Admin Group Membership](/assets/img/posts/homelab-admin-group-membership.jpg)

*Figure 6: Example user group membership configured in Active Directory.*

This part of the lab helped reinforce the importance of least privilege.

A normal user should not have administrative access unless there is a clear reason. In a real environment, excessive permissions can create serious risk. If a privileged account is compromised, the attacker may gain broader control over the environment.

## DNS, DHCP, and Networking Practice

Another major part of the lab was practicing networking concepts.

I worked with IP addressing, subnetting, DNS, DHCP, and network connectivity between virtual machines. This was one of the more challenging parts at first because small configuration mistakes can prevent systems from communicating properly.

Some of the issues I had to think through included:

- Correct IP ranges
- Subnet masks
- DNS server settings
- Domain controller name resolution
- Client-to-server connectivity
- DHCP assignment
- Firewall behavior
- Network adapter settings inside VMware

Networking is one of those areas where troubleshooting helped me learn more than simply following instructions. When the Windows client could not reach the domain or DNS did not resolve correctly, I had to slow down and understand what was actually happening.

## Windows Firewall and Defensive Configuration

I also spent time reviewing Windows Firewall with Advanced Security.

![Windows Firewall](/assets/img/posts/homelab-windows-firewall.jpg)

*Figure 7: Windows Firewall with Advanced Security enabled across profiles.*

This helped me understand how firewall rules affect system security and availability. In red vs blue competitions and real environments, defenders need to allow necessary services while blocking unnecessary access.

The challenge is balance. If firewall rules are too open, the system may expose unnecessary attack surface. If they are too strict, important services may stop working.

This connects directly to blue team work because defenders often have to secure systems without breaking business functionality.

## Linux and Kali Systems

Kali Linux was included so I could practice offensive security tools in a controlled way.

I used Kali as a system for learning reconnaissance, scanning, enumeration, and testing concepts that later connected to my other security labs. Ubuntu gave me another Linux system to configure and troubleshoot.

This helped me understand both sides of security:

- How attackers may scan or enumerate systems
- How defenders can reduce exposed services
- How Linux and Windows systems behave differently
- Why service configuration matters
- Why logs and network visibility are important

Having both Windows and Linux systems made the lab feel more realistic than only working with one operating system.

## Security Tools I Practiced With

I used the homelab as a place to practice with security tools and concepts from my coursework and projects.

Some tools and areas I worked with or built toward included:

- Nessus for vulnerability scanning
- pfSense for firewall and network segmentation practice
- Kali Linux tools for reconnaissance and testing
- Windows Firewall for host-based defense
- Active Directory for identity and access management
- Event Viewer for basic Windows log review
- DNS and DHCP troubleshooting
- Service configuration and hardening

The lab gave me a place to connect different skills together. For example, understanding Active Directory made red vs blue competitions easier. Practicing firewall rules made service hardening more understandable. Working with Kali helped me think about what attackers look for when scanning systems.

## What I Struggled With

The hardest part of building the homelab was troubleshooting small issues that affected the whole environment.

Some of the areas that were challenging included:

- Getting DNS configured correctly
- Making sure the Windows 10 client could join the domain
- Understanding which IP settings belonged on each VM
- Avoiding firewall rules that broke connectivity
- Keeping track of users, groups, and permissions
- Understanding why a service was not reachable
- Knowing when to troubleshoot Windows versus VMware networking

At first, it was easy to think something was broken without knowing where the issue was. Over time, I got better at checking the basics: IP address, DNS, firewall, credentials, service status, and domain connectivity.

## Lessons Learned

This homelab helped me understand that cybersecurity depends heavily on IT fundamentals.

Before defending a system, you need to understand how it works. Before investigating suspicious activity, you need to understand what normal activity looks like.

Some of my biggest takeaways were:

- Active Directory is central to enterprise security.
- DNS problems can break almost everything.
- Firewall rules require careful testing.
- User permissions need to be managed carefully.
- Windows and Linux systems require different troubleshooting approaches.
- Virtual labs are one of the best ways to learn safely.
- Blue team defense depends on understanding infrastructure first.

This project also helped me prepare for red vs blue competitions because I had already practiced working with Windows Server, domain users, firewall rules, and basic service troubleshooting.

## Future Improvements

There are several things I want to add to the homelab next.

My future improvements would include:

- Adding Splunk for centralized log collection
- Installing Sysmon on Windows systems
- Adding Wazuh for endpoint monitoring
- Expanding pfSense for network segmentation
- Creating separate attacker, user, and server networks
- Running Nessus scans against lab machines
- Practicing vulnerability remediation
- Adding Group Policy hardening
- Creating a simple incident response scenario
- Using Atomic Red Team to generate safe detection events
- Building dashboards for authentication, process creation, and network activity

These additions would make the lab more useful for detection engineering, SOC analysis, threat hunting, and incident response practice.

## Final Thoughts

Building this homelab helped me connect cybersecurity concepts with real systems.

It gave me a place to practice Active Directory, Windows Server administration, Linux, Kali, DNS, DHCP, firewall rules, vulnerability scanning, and defensive thinking.

More importantly, it helped me understand that cybersecurity is built on fundamentals. Tools are useful, but they make more sense when you understand the systems underneath them.

This homelab gave me a stronger foundation for SOC work, red vs blue competitions, endpoint investigations, and future security projects. It is something I want to keep improving as I continue learning and growing in cybersecurity.
