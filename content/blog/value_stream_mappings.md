+++
showdate = true
title = "Value Stream Maps (VSM) DevOps Basics - Part 6"
date = 2022-12-23
url = "/blog/devops_basics_6"
mermaid = true
+++

# [What is a VSM?]()
A value stream map is a popular tool for DevOps transformation. Basically it is a flowchart like technique which identifies all the relevant application and systems and visualize the end to end delivery process. It provides a visibilty of work for the organization. You can create a value stream map by gathering stakeholders from every part of the product development value stream: the business line, design, testing, QA, operations, and support. You break the value stream into 5 to 15 process blocks. In each block, you record the activity that's performed.

# An Example :)
Let's take an example with the following flowchart. Here I have kinda broken down a general application development scenario. 
{{<mermaid>}}
%%fig-height: 100
flowchart LR
    A[Customer]
    B(Design & Analysis)
    C(Approval)
    D("Release Planning & Estimation")
    E("Development & Dev Testing (including test automation)")
    F("Showcase & UAT")
    G("Exploratory & perf testing")
    H("Change approval")
    I("Production deployment")

    A ===> B
    I ===> A

    F -...-> H
    
    subgraph "Line Of Buisness"
        B -..-> C
    end

    subgraph "Project Management Office"
        C -...-> D
    end
    
    subgraph "Engineering"
        D -...-> E
    end

    subgraph "QA"
        E -...-> F
        F -...-> G
    end


    subgraph "IT Ops"
        H -...-> I
    end

    G -...-> I
{{</mermaid>}}

Next, we analyze the state of work within the value stream, gathering the information to determine barriers to flow. In particular, for each process block, you measure the following key metrics:

- Lead time: the time from the point a process accepts a piece of work to the point it hands that work off to the next downstream process.
- Process time: the time it would take to complete a single item of work if the person performing it had all the necessary information and resources to complete it and could work uninterrupted.
- Percent complete and accurate (%C/A): the proportion of times that a process receives something from an upstream process that it can use without requiring rework.

You always record the state of the processes as they really are on the day the exercise is performed. Make sure that you determine the actual metrics, not what people would like the metrics to be.

