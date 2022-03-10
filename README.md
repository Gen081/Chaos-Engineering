# Chaos-Engineering
### Chaos Engineering with LitmusChaos on Amazon EKS


As organizations are embracing microservices-based architectures by refactoring large monolith applications into smaller, independent, and loosely coupled services. These independent services are faster to deploy and scale, enabling organizations to innovate and deliver faster. Nevertheless,as the application grows, these microservices present their own challenges. 

For example, as you deploy tens or hundreds or thousands of microservices, operational tasks such as distributed tracing, debugging, testing, dependency mapping, and so on, become challenging. 

To address these challenges, organizations are increasingly practicing Chaos Engineering to test the reliability and performance of distributed systems.


#### What is Chaos Engineering?

Chaos Engineering is the discipline of experimenting on a system in order to build confidence in the system’s capability to withstand turbulent conditions in production.


#### Chaos Engineering in Practice

This practive follows 4 steps:

**1-** Start by defining ‘steady state’ as some measurable output of a system that indicates normal behavior.

**2-** Hypothesize that this steady state will continue in both the control group and the experimental group.

**3-** Introduce variables that reflect real world events like servers that crash, hard drives that malfunction, network connections that are severed, etc.

**4-** Try to disprove the hypothesis by looking for a difference in steady state between the control group and the experimental group.







Install kubectl using native package management.  Type these commands below:


![](pics/kubectl-install.png)

![](pics/kubectl-install1.png)



Install Helm

![](pics/helm-install.png)


