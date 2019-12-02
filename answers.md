
## 1)Indicate architecture re-design and/or changes in build flow that can help you achieve those goal and provide reasoning why;

-  About the  build times:

I believe this build times need some troubleshooting to understand why it is taking so long.
I would go to the way of or suggesting:

1.1) Is this internet speed to bad to checkout the repo ? or is it to big ? should we have this size of repo? 
  - If we need to speed up its step what about persit its repo and just update each time of the build runs instead of pull it all again?
  We may can save some minutos by keeping it here, but if it's to many repos which the use of its is not usual it would be waste of disk space and we may see
1.2)  Make your jenkis master free of work so it don't do much work wich can lead to slow builds.
1.3) The build could be running on sub-optimal hardware that needs serious upgrading and never got a priority or budget.
1.4) If there is a heavy workload you can restrict your jobs to a node label with more resources or if its is not priority send it to some work group nodes wich can have
parallelism workload.
1.5) Consider having a local cache so that all dependencies are not downloaded every time a commit happens. 
If it's the case whe a slave dies it'll have to be dowloaded again and some builds woudl take longer.
1.6) Is it using docker? if so it won't build every time since there is no change to the pakcage.json for example. It'll reuse the docker layer of the latest build.


- About  decreasing the infrastructure cost of the solution and improving it.

 
If there is a Kubernetes cluster already we would use its infrastructure to provision jenkins slaves into
kubernetes throught its automatically launch and destroy based on demand and current utilization.
(https://wiki.jenkins.io/display/JENKINS/Kubernetes+Plugin)

But if there is not a Kubernetes clusters and I should work directly with AWS EC2 instances I would suggest using
the infrastructure in the attached doc.


1.1) Use AWS ASG: 
  - So you'll have the desired number of jenkins slaves to the current workload and when there is no need of slaves because of low workload you can save money by scaling down. For example during the night.
  - You might have multiple AWS ASG with different jenkins node labels to specific workloads.
1.2) Use AWS ASG fleet:
 - It is the best way AWS provide to have a mix of instance types and make possible to work out with more spot instance since you'll pick up a range of instance types to avoid problems with spot market problems.

Some other no related improvements we could acomplish:

- Use everyhing in private subnets
- Create multiple jenkis masters
- Use Multi AZ
- 

2)Describe, taking into account that we want the lowest downtime possible, your procedure of:

I would suggest the steps below to avoid downtime, make it easir to upgrade jenkins

1) ansible role to launch an instance with OS, jenkis and plugins upgrades.
3) create new jenkins cluster with its master and asg jenkins slaves.
4) create/restore jobs with ansible(https://docs.ansible.com/ansible/latest/modules/jenkins_job_module.html)
5) validate it and update jenkins dns

Why? You'll have everything versioned, easier way to recover form disaster and to operate 
your system instead of the Headache of manually have to install, export jobs, restore it and other tiring manually activities.

manually upgrades is not tracked and if it breaks you'll have downtime and a lot of stress. 

