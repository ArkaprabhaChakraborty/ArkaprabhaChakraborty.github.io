+++
showdate = true
title = "Containers Basics"
date = 2023-02-26
url = "/blog/containers_basics/"
+++
# What is a container?
You must have seen large ships with "Containers" - like the one shown below. Yes those colored boxes are called containers. They help to transport goods from one place to another. Each of these colored boxes are small isolated units that don't know what's inside the other and aren't even bothered by it [As if they have a life :) ]. While the containers "contain" the goods, they essentially rely on the ship for the actual transportation.

![Ship OP](https://raw.githubusercontent.com/ArkaprabhaChakraborty/ArkaprabhaChakraborty.github.io/main/data/images/what-are-container-ships-1.png)

This is exactly what a container is. Even in the world of software, a container is just a small isolated unit that doesn't know what's inside the other one and isn't even bothered by it. Containers are used to package software into small isolated units for easy deployment and scaling.

# Why do we need containers?

In a traditional deployment model, we have the hardware, the OS and the libraries and their related dependencies. Let's say we have a web application using NodeJS, MySQL, Redis and React. Mostly what we use are different libraries for the development of different web application functionalities. Every such libraries and frameworks have their own dependencies and OS-related dependencies. 

In a traditional deployment setup as shown in the image below, sometimes what can happen is that there can be some conflicts between the dependencies. This can be nightmare to debug and fix. Now let's just change the context and call the setup shown below as traditional development setup. Generally developers don't always work on the same one project. So if two projects require different versions of the same libraries or dependencies this can lead to a lot of conflicts. 
![Traditional deployment](https://raw.githubusercontent.com/ArkaprabhaChakraborty/ArkaprabhaChakraborty.github.io/main/data/images/traditional-deployment.png)

This is where containers come into the game. With containers, we can package the application and all of its dependencies into a single unit. This makes it easy to build, ship, deploy and scale our applications easily. Let's take a look at the architecture. Like traditional model we have the hardware and OS, but now we have a container engine sitting on top of our OS. This container engine is responsible for creating and managing containers. Each container have their own isolated environment with different libraries and dependencies.
![Container deployment](https://raw.githubusercontent.com/ArkaprabhaChakraborty/ArkaprabhaChakraborty.github.io/main/data/images/container-deployment.png)

# So what are the general advantages of containers?

- **Isolation**: Containers are isolated from each other and from the host. They have their own file system, their own networking, and their own isolated process tree separate from the host.
- **Resource efficiency**: Containers can run without the extra load of a hypervisor. I'll discuss this later in a separate blog.
- **Ease of installation**: Containers are easy to install and setup as compared to traditional methods where you may have to deal with a dependency hell.
- **Portability**: You can easily move containers from one environment to another. This allows developers to quickly ship their applications to the cloud.
- **Scalability**: Containers are designed to scale up and down easily. Again I will cover the scalability topic in a separate blog.
- **Developer productivity**: Containers make it easy for developers to package up their applications and dependencies into a container that can be deployed and run anywhere.
- **Easy deployment and management**: Containers make it easy to deploy and manage applications on a large scale.

# So where does this container thingy "stem" from?
