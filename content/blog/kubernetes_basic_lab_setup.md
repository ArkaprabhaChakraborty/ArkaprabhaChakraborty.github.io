+++
showdate = true
title = "Kubernetes Basics - Build a simple cluster lab on GCP"
date = 2023-02-18
url = "/blog/kubernetes_basics_1/"
+++

This is the first blog in the Kubernetes Basics series. In this blog, we will be setting up a simple Kubernetes cluster on GCP. We won't be using the GKE instead we will go for two identical Ubuntu 20.04 VMs to setup a simple lab setup. This is basically the starter point for CKS certification.

# Prerequisites:
- A GCP account
- Knowledge of basic Linux commands
- [Google Cloud CLI](https://cloud.google.com/sdk/docs/install-sdk) installed on your local machine

# Step 1: Create two identical Ubuntu 20.04 VMs on GCP
Let's Create two identical Ubuntu 20.04 VMs on GCP using the cloud CLI. I have named them `cks-master` and `cks-worker`. You can name them as you wish.
```bash
gcloud compute instances create cks-master --zone=us-central1-c \
--machine-type=e2-medium \
--image=ubuntu-2004-focal-v20220419 \
--image-project=ubuntu-os-cloud \
--boot-disk-size=50GB

gcloud compute instances create cks-worker --zone=us-central1-a \
--machine-type=e2-medium \
--image=ubuntu-2004-focal-v20220419 \
--image-project=ubuntu-os-cloud \
--boot-disk-size=50GB
```
You can do this in the [GCP console](https://cloud.google.com/compute/docs/create-linux-vm-instance) as well. Just go to the Compute Engine section and create two VMs with the same configuration.

# Step 2: Create a quick start/stop script

Now that we have our VMs ready, we need to create a quick start and stop script for VMs. Very important if you don't wanna get booked beyond your budget.

`start-cks-lab`:
```bash
gcloud compute instances start cks-master --zone=us-central1-c
gcloud compute instances start cks-worker --zone=us-central1-a
```

`stop-cks-lab`:
```bash
gcloud compute instances stop cks-master --zone=us-central1-c
gcloud compute instances stop cks-worker --zone=us-central1-a
```

# Step 3: Create short scripts to SSH into the VMs 
You can skip this step if you are comfortable with the `gcloud compute ssh` command. I personally prefer to create a short script to SSH into the VMs. This is just a personal preference.

`gssh-cks-master`:
```bash
gcloud compute ssh cks-master
```
`gssh-cks-worker`:
```bash
gcloud compute ssh cks-worker
```

# Step 4: SSH into the VMs and install Docker and Kubernetes