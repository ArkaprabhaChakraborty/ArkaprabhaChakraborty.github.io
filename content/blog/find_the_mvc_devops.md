+++
showdate = true
title = "Where to start??? DevOps Basics - Part 5"
date = 2022-12-26
url = "/blog/devops_basics_5"
+++
# [Find your MVC](#find-your-mvc)

Minimum viable cluster (MVC) is a subset of applications that are the most important to the business and are the best candidates for a DevOps transformation. Improving the MVC leads to faster and more reliable delivery of business value.

Thge concept behind this is that of a weakest link in a chain. the weakest link in a chain defines the overall doability of a chain. Similarly the applicatiion witth lowest maturity in a cluster tends to define oyour ability to deliver.

To get the minimum viable cluster we follow the following iterative approach:
- Pick an  application having the highest priority, attained by the application portfolio analysis.
- Identify the dependencies of the application. It is important to look for dependencies related to changes - to identify systems that need to change togather to enable new functionality - Just the most dependencies will do he work.
- Once we have identified the appilcations that change togather with out first application, we have identified our first cluster. 
- We then go through all the applications in a cluster to check for new dependencies. We continue this process until we have identified all the applications that change togather, aka, a stable cluster.

**Disclaimer**: The MVC is not a static concept. It is a dynamic concept that changes over time. It is important to keep an eye on the MVC and make sure that it is still the most important cluster to focus on and doesn't become unwieldy. Adding and removing applications from the MVC is a normal part of the DevOps transformation and its not always *exact*.

# [Boost of Capabilities using MVC](#boost-of-capabilities-using-mvc)

After finding a cluster of applications a Value Stream Map (VSM) can be used to find the gaps and more details (More about this on part 6). Other than doing a VSM we can also look at the five core themes of DevOps capabilities and identify which one of them provides the most value if uplifted next. These are:
- **Configuration Management**: allows us to know what is exactly in every environment, in every server and in each piece of code and software. this helps in automating large parts of DevOps capabilities as we know exactly what configurations we need to change to enable new functionality. Without solid configuration management, the automation will fail frequently.
- **Deployment and release**: it's the process of EXACTLY WHAT IT SAYS, just add a "for software" to it :). This quite often the easiest to automate. We need to make this "*boring*" by making this have minimal manual steps, align the process into different environments and so that we can reliably follow the same process to make the deployment and release consistent. 
- **Testing**: it is a large and complex field which involves autpmated testing of applications. The common first goal is to setup an automated regression test suite and run it atleast once per agile cycle. The other aspects of testing involves security testing, performance testing, progression testing, usability testing and validation testing.
- **Environment Management**: deals with the creation and management of environment for development, testing or production purposes.  
- **Operations**: Production is what counts for customers, improving the operations capabilities is the most important part of the DevOps transformation. This involves monitoring, logging, alerting, incident management, change management, capacity management, performance management, security management, compliance management, disaster recovery and so on.

The tip is get all the applications in the cluster to the same level of capabilities first, before making further investments, so that the benefits can be reaped as eaarly as possible.

#### Up Next - [DevOps Basics - Part 6](/blog/devops_basics_6)
#### Previous - [DevOps Basics - Part 4](/blog/devops_basics_4)