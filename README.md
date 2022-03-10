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



### LitmusChaos Architecture

[LitmusChaos](https://litmuschaos.io/) is a cloud-native Chaos Engineering framework for Kubernetes. It is built using the [Kubernetes Operator framework](https://sdk.operatorframework.io/). A [Kubernetes Operator](https://kubernetes.io/docs/concepts/extend-kubernetes/operator/) is a software extension to Kubernetes that makes use of [custom resource definitions (CRDs)](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/) to manage applications and their components.


Litmus takes a cloud-native approach to create, manage, and monitor chaos. Chaos is orchestrated using the following Kubernetes CRDs:

- **ChaosEngine**: A resource to link a Kubernetes application or Kubernetes node to a ChaosExperiment. ChaosEngine is watched by the Litmus ChaosOperator, which then invokes ChaosExperiments
- **ChaosExperiment**: A resource to group the configuration parameters of a chaos experiment. ChaosExperiment CRs are created by the operator when experiments are invoked by ChaosEngine.
- **ChaosResult**: A resource to hold the results of a ChaosExperiment.


For this project, I will create an Amazon EKS cluster with managed nodes. I’ll then install LitmusChaos and a demo application. After that, I will install chaos experiments to be run on the demo application and observe the behavior.


The followings are needed to complete the project:

- [AWS CLI version 2](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)
- [eksctl](https://docs.aws.amazon.com/eks/latest/userguide/eksctl.html)
- [kubectl](https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html)
- [Helm](https://www.eksworkshop.com/beginner/060_helm/helm_intro/install/index.html)





### Step 1: Create EKS cluster






Install kubectl using native package management.  Type these commands below:


![](pics/kubectl-install.png)

![](pics/kubectl-install1.png)



Install Helm

![](pics/helm-install.png)









After you create your Amazon EKS cluster, you must configure your kubeconfig file with the AWS Command Line Interface (AWS CLI). This configuration allows you to connect to your cluster using the kubectl command line.

- Verify that AWS CLI version 1.16.308 or greater is installed on your system:

```
$ aws --version
```
- Check the current identity to verify that I am using the correct credentials that have permissions for the Amazon EKS cluster:

```
aws sts get-caller-identity
```

- Create or update the kubeconfig file for your cluster:

```
aws eks --region region update-kubeconfig --name cluster_name
```

```
$ kubectl get pods --kubeconfig ./.kube/config
```

![](pics/kubeconfig.png)



