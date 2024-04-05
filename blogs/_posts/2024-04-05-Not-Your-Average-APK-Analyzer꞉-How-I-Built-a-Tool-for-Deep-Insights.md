---
layout: post
title: "Not Your Average APK Analyzer꞉ How I Built a Tool for Deep Insights"
sitemap: true
hide_last_modified: true
---

When I joined Cyfinoid Research Pvt Ltd in January 2023, my backend development skills were rudimentary. But through determination and the opportunity to build an end-to-end fast and extensive Android APK analysis tool, I've laid a strong groundwork for my career.

Android has become the world's dominant mobile operating system, powering billions of devices. Ensuring the security and optimization of Android apps is paramount. However, existing APK analysis tools often face a critical tradeoff: speed versus comprehensiveness. My project aims to bridge this gap, presenting a novel approach to Android APK analysis that delivers both efficiency and in-depth insights.

But how is it different to any other apk analysis tool like “Pithus”? Traditional Android APK analysis tools (such as Pithus) can have excessively long processing times (10-15 minutes). This delay impedes developer productivity and compromises the timely identification of security risks**.** But our tool not just make the analysis process faster but lets you choose the specific tools you want to run.

Lets study how it works.

List of open source tools that I integrated to build this : 

System design:

![sysArch.png](/assets/img/ss/sysArch.png)

By decoupling tools into separate Docker containers, we've built a modular and scalable analysis solution.  This approach simplifies CI/CD pipelines and allows us to seamlessly upgrade or replace individual tools for an enhanced developer experience. Moreover, this will be convenient if we optimize any open source tool.

How publicly available apk analysis tools are dependent on each other: 

![tools.png](/assets/img/ss/tools.png)

To boost our tool's speed, we've implemented two key optimizations.  First, we've re-engineered the tool to run all analyses asynchronously, maximizing efficiency.  Secondly, we're directly integrating APKiD from our side, allowing it to run in parallel with other analysis tools instead of being confined within MobSF's sequential execution. This bypasses a significant bottleneck and further accelerates the analysis process for our users.

To streamline analysis results and make them easily searchable, we've adopted Elasticsearch. By receiving JSON documents directly from each analysis tool and unifying them within Elasticsearch, we gain several advantages.  Elasticsearch's schema-less nature accommodates potentially varying data structures from different tools. Plus, its powerful search capabilities and speed ensure users can quickly find relevant insights within the vast amount of analysis data the tool generates.

A glimpse of tool:
![upload](/assets/img/ss/1.png)
![upload](/assets/img/ss/2.png)

*This tool is going public very soon, contact me if you want to discuss more about this, I will soon attach the github link here.*  

Building this tool has been an incredibly rewarding experience, both professionally and personally. I've learned a tremendous amount about backend development, optimization techniques, and the complexities of Android analysis. I'm deeply grateful for the opportunity to take on this challenging project, and I especially want to thank [Mr. Anant Shrivastava](https://anantshri.info/), CEO of Cyfinoid, for entrusting me with this responsibility. It has been a fantastic way to kickstart my development career!