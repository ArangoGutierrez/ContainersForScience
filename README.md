# ContainersForScience
Workshop at The 12th Colombian Conference on Computing (12CCC)

>  Hands on Linux Containers

## Speaker
> Eng.Carlos Eduardo Arango
> carlos.arango.gutierrez@correounivalle.edu.co

The speaker is a PhD student in computer science currently working on building a computing platform for application of models of atmospheric science applied to precision farming in Valle del Cauca.

Carlos is also an engineer and Researcher with a background in computer programming, HPC, atmospheric physics, and environmental modeling. He is currently working on a cloud computing framework for atmospheric science.

Scientific HPC with Linux Containers

## Topics:
  -  Cloud Computing
  -  Distributed systems
  -  Distributed operating systems
  -  Cloud Middleware
  -  HPC
  -  Linux Containers
  -  Portability
  -  Experimental reproducibility

## Motivation
Virtualization has been a key technology in the development of  cloud computing.  However virtual machines (VMs) for high performance computing (HPC) are not an attractive (or possible) option. A VM gives users freedom of choosing an operating system, software stack and for the scientific community they give repetitivity and reproducibility for their work. Said that, with VMs the researchers can ensure that they work will run on every single machine, reviewers will be able to assess and reproduce third party work and thus avoiding the common issue of “it worked on my workstation”.
The well-known solutions for cloud computing, both commercial (Amazon Cloud, Google Cloud, OpenShitf, etc.) and open-source (OpenStack), provide platforms for running a single VM or  multiple VMs. This approach comes with drawbacks, which include reduced performance when running code inside of a VM, increased complexity of cluster management, followed by a high level of administrative overhead, as well as the need to learn new tools and protocols to manage the clusters. These solutions are not geared, designed, or commonly utilized on HPC centers.
Recently a new player has come to the game of virtualization, adopting the name of containers. Linux Containers make use of Linux Namespaces to manage resource isolation for processes and Linux cgroups to manage resources for a group of processes. Container technologies allows the isolation and mange of system resources (e.g.CPU and memory) according to site policies.
The implementation of containers is having a warm welcome on the enterprise and web scenarios where the development, ease distribution and deployment of applications have been eased. Led by projects such as Docker and Linux Containers (LXC). Nevertheless Its implications for scientific computing including HPC are still on doubt.

Projects like Singularity (Kurtzer, G. M. 2016) which focus on utilizing container technologies to create portable environments are enabling computational science to move to the cloud. Containers are proving to be an extremely valuable technology for science delivering portability and reproducibility to the users. Containers can emulate a single program and can be executed directly without the overhead that comes with running a virtual kernel and hardware. With little effort a HPC cluster with an installed workload manager such as Slurm, HTCondor or Torque can deploy  an application or code via Singularity containers with no drawbacks on performance.  

Singularity containers are purpose built and can include a simple binary and library stack or a complicated workflow that includes both network and file system access (or anything in between). The Singularity container images are then completely portable to any binary compatible version of Linux with the only dependency being Singularity running on the target system.

Singularity blocks privilege escalation within the container so if a user want’s to be root inside the container, it must be root outside the container. This usage paradigm mitigates many of the security concerns that exists with containers on multi-tenant shared resources. You can directly call programs inside the container from outside the container fully incorporating pipes, standard IO, file system access, X11, and MPI.

Keywords:
> HPC, Linux Containers, Singularity, Docker, LXC, portability

## Objectives
 - Give an introduction to Linux containers and how to implement them in HPC processes.
 - Run an application that was built for a different distribution of Linux than the host OS.
 - Reproduce an environment to run a workflow created by someone else.
 - Run a series of applications (a 'pipeline') that includes applications built on different platforms.

## Table of contents
### Day 1
 - Introduction to Linux Containers  (Containers are NOT VMs)
 - Working with Containers (Build, Ship, Run)
 - But Why?
 - Getting started
 - Q & A

### Day 2

 - Developing a Simple Web App
 - Developing a Multi-OS web app
 - Deploy a local Image Registry
 - Q & A

## Audience profile
This meeting aim is the education and formation of computer science researchers, IT professionals, system administrators, computer science researchers and engineer students, able to propose solutions for distributed systems and HPC problems in their projects, or whose research will use/develop HPC or cloud computing applications.


## References
 - Kurtzer, G. M. (2016). Singularity 2.1.2 - Linux application and environment containers for science. Zenodo. http://doi.org/10.5281/zenodo.60736
 - LinuxContainers.org Infrastructure for container projects. Project sponsored by Canonical Ltd. https://linuxcontainers.org/
