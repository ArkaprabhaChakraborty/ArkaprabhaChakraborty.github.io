+++
showdate = true
title = "Basics of Security - Part 1 (Theory notes made by googling and reading different articles)"
date = 2022-10-04
url = "/blog/basics_of_security_1"
+++

# [What is Computer Security?](#what_is_security)

Computer security is the protection of items of value, called assets, of a computer or computer system. There are many types of assets, involving hardware, software, data, process, staff, or a combination thereof.

# Computer security:
- **Integrates into the Assets Life Cycle**: Security begins with acquiring assets. It is important that there is a chain of custody for the physical assets, up to and including when the asset is powered on.
- **Integrates into the Software Development Life Cycle**: Software development life cycle is a process that includes Design, Development, Testing, and Deployment. It is critical that security is considered during the design phase of a project.
- **Establishing Organizational Policies and Procedures**: Establishing policies and procedures reduces the risk of accidental or intentional harm.
- **Defining Roles and Responsibilities**: It is important to establish a hierarchical structure for incident response. There needs to be a defined leader or plan for assigning a leader due the strenuous nature of the job.

--------------------

# [Basic Principles:](#basic_principles)

The basic principles of computer security are:
- [**Assessment**](#assessment)
    - Assessment is the operation of determining the value of assets and the cost of implementing security to protect those assets. 
    - The focus is generally on the most valuable assets. 
    - Because of limited resources, 100% security is impossible to achieve. Priorities will be assigned to specific assets based on their value. Knowing the value of each asset and the cost of securing each item is critical in focusing the security resources.
    - Residual risks are those that are unsecured by controls post-deployment. These are generally not a priority or cost-prohibitive items that are left unaddressed in the security framework. You should document them. Examples:
        -   Single sign-on systems can introduce new security concerns.
        -   Additional security staff is itself a risk.
        -   Centralization of services introduces greater risk of downtime through single points of failure and attack.
- [**Prevention**](#prevention)
    - Prevention of known risks is the easiest and likely the most cost effective principle to incorporate into a system.
    - Prevention is the implementation of security measures, called controls, to protect assets identified during the assessment stage. Controls refer to software and strategies deployed to protect the assets.
    - There are three types of controls:
        -   Technical controls involve software and hardware to protect assets.
        -   Procedural controls involve processes and policies designed to prevent loss and damage.
        -   Physical controls involve facilities, staff, key cards, locks, and other measures.
    - For each asset, there is some aspect that needs to be protected. For example, 
        -   For services: Does it need to be available? 
        -   For data: Does it need to be confidential? 
        -   For new software: Can the integrity of the code and executables be assured?
    - There are an unlimited number of threats out in the world; they can be accidental, malicious, and directed. It is impossible to prevent them all, which is why the assessment stage is important.
- [**Detection**](#detection)
    - Detection is the process of identifying a threat to an asset.
    - Detection involves monitoring through the use of various technologies, such as remote logging, system statistics, and performance metrics. Detection can be the most expensive and can be difficult to execute effectively.
    - Intrusion Detection and Prevention Systems (IDPS) are used to identify possible incidents, create a consistent audit trail, and report attempted intrusions.
    - Current incident detection methods include signature-based, statistical anomaly-based, and stateful protocol analysis.
    - Stateful protocol analysis includes system monitoring. Statistical anomaly based detection involves creating a baseline and monitoring for anomalies. Tools like Prometheus are helpful for production-wide metrics and a time-series database to view historical information. The use of production-wide logging is also essential to understand typical usage, as well as unusual activity.
- [**Reaction**](#reaction)
    - Reaction is one of the least considered principles, and catastrophes often result from poorly planned reactions to vulnerabilities and risks. How an incident is addressed may have long term effects and must be considered carefully.
    - While detection of system risks is critically important, how an organization reacts to these vulnerabilities can determine the survival of the system and, in some cases, the organization itself. Reaction can range from adding firewall rules, adding scanners, re-implementing the systems, or shutting down certain components. An inappropriate response can be disastrous in its technical effectiveness or simply in its perceived effectiveness. If users and consumers are not confident in an organization’s reaction to a threat, they may move to other competitors.
    - Part of the reaction is ensuring business continuity. Business continuity requires knowing which components are most important for the business. This topic is also addressed during the assessment phase of any security framework.
    - It is important to maintain a focus on problem solving. Work on solutions rather than specific blame. Blame is an unproductive activity. Unexpected failures will occur no matter how much planning and implementation is effected. Root cause analysis is productive for identifying technical issues.