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

Since I already installed the AWS CLI version 2 on my terminal from a previous project.  I simply check its version:

![](pics/aws-version.png)


- [eksctl](https://docs.aws.amazon.com/eks/latest/userguide/eksctl.html)

![](pics/eksctl.png)

- [kubectl](https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html)

To install kubectl using native package management. Use this link to install kubectl [Install and Set Up kubectl on Linux](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/) 
and at almost the very end of the page, there will be the "native package management" section.

 Type these commands below:

![](pics/kubectl-install.png)

![](pics/kubectl-install1.png)

- [Helm](https://www.eksworkshop.com/beginner/060_helm/helm_intro/install/index.html)

![](pics/helm-install.png)

### Step 1: Create EKS cluster

Steps to create a new EKS cluster

- Open your terminal and run these commands on it.

![](pics/eks-cluster.png)

After running these commands on the terminal, navigate to AWS console and search for Amazon Elastic Kubernetes Service (EKS)

![](pics/aws-eks-search.png)

The cluster created on the terminal should appear as such:

![](pics/eks-litmus-demo.png)

When click on the cluster **eks-litmus-demo**, 2 nodes must show. If for any reason, there is no Node in there, need to add Nodes manually.

this link below will demonstrate how to manually Add Worker Nodes To Amazon EKS Cluster:

[How to add Nodes to EKS Cluster](https://ostechnix.com/add-worker-nodes-to-amazon-eks-cluster/)


These screenshots below will demostrate the steps of what I did to add the Worker Node to the EKS cluster

![](pics/worker-node.png)

![](pics/worker-node1.png)


![](pics/worker-node2.png)


![](pics/worker-node3.png)


![](pics/worker-node4.png)

![](pics/worker-node5.png)

![](pics/worker-node6.png)

![](pics/worker-node7.png)

![](pics/worker-node8.png)

![](pics/worker-node9.png)

![](pics/worker-node10.png)

![](pics/worker-node11.png)

![](pics/worker-node12.png)

![](pics/worker-node13.png)



After I create the Amazon EKS cluster, I must configure my kubeconfig file with the AWS Command Line Interface (AWS CLI). This configuration allows me to connect to my cluster using the kubectl command line.

Here is the instructions link:
[Amazon EKS cluster using the kubectl command line](https://aws.amazon.com/premiumsupport/knowledge-center/eks-cluster-connection/)



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



### Step 2: Install LitmusChaos

- Let’s install LitmusChaos on an Amazon EKS cluster using a Helm chart. The Helm chart will install the needed CRDs, service account configuration, and ChaosCenter.

- Add the Litmus Helm repository using the command below:

![](pics/litmuschaos-install.png)


- This command below confirms that I have the Litmus-related Helm charts:

![](pics/litmuschaos-install1.png)

- Next, Let's Create a namespace to install LitmusChaos.

![](pics/litmuschaos-install2.png)


- By default, Litmus Helm chart creates NodePort services. Let’s change the backend service type to ClusterIP and front-end service type to LoadBalancer, so I can access the Litmus ChaosCenter using a load balancer.
  - I create a file on my PC and name it override-litmus.yaml. The, edit the override-litmus.yaml file and place the below code in it. 

![](pics/litmuschaos-install3.png)


- Install the chaos via by running the below command:

![](pics/litmuschaos-install4.png)


- The following commands will verify if LitmusChaos is running:

![](pics/litmuschaos-install5.png)

![](pics/litmuschaos-install6.png)


- To access Litmus ChaosCenter UI, let's use the URL given here

```http://ac2a5f52b687e41f09400596e6e8db67-1674008896.us-east-1.elb.amazonaws.com:9091```

and sign in using the default username “admin” and password “litmus.”

![](pics/litmuschaos.png)


![](pics/litmuschaos1.png)


![](pics/litmuschaos2.png)


- Confirm the agent installation by running the command below.

![](pics/litmuschaos3.png)



- Verify that LitmusChaos CRDs are created:

![](pics/litmuschaos4.png)


- Verify that LitmusChaos API resources are created:

![](pics/litmuschaos5.png)

Now, I successfully install LitmusChaos on the EKS cluster. 



### Step 3: Install demo application

Let’s deploy nginx on the cluster using the manifest below to run the chaos experiments on it. Save the manifest as nginx.yaml and apply it.

- Create a file on your PC and name it nginx.yaml. Command to create that nginx.yaml is this :

 ```cat <<EOF> nginx.yaml```

Then edit **nginx.yaml** with the below code

![](pics/demo-app-install.png)


- Install the demo app by running the below command on your terminal 

![](pics/demo-app-install1.png)


- Verify if the nginx pod is running by executing the command below.

![](pics/demo-app-install2.png)



### Step 4: Chaos Experiments

[Litmus ChaosHub](https://hub.litmuschaos.io/) is a public repository where LitmusChaos community members publish their chaos experiments such as **pod-delete,** **node-drain**, **node-cpu-hog**, etc. In this section, I will perform the **pod-autoscaler** experiment from LitmusChaos hub to test cluster auto scaling on Amazon EKS cluster.

- **Experiment: Pod Autoscaler**

The intent of the pod auto scaler experiment is to check the ability of nodes to accommodate the number of replicas for a deployment. 

**Hypothesis**: Amazon EKS cluster should auto scale when cluster capacity is insufficient to run the pods.

Chaos experiment can be launched using the Litmus ChaosCenter UI by creating a workflow.

Let's navigate to Litmus Chaos Center that I previously create and select **Litmus Workflows** in the left-hand navigation and then select the **Schedule a workflow** button to create a workflow.


![](pics/chaos-center.png)


![](pics/chaos-center1.png)



![](pics/chaos-center2.png)


![](pics/chaos-center3.png)


![](pics/chaos-center4.png)


![](pics/chaos-center5.png)



![](pics/chaos-center6.png)


![](pics/chaos-center7.png)



![](pics/chaos-center8.png)



![](pics/chaos-center9.png)



![](pics/chaos-center10.png)


From the ChaosResults, it is clearly that the experiment failed because there was no capacity in the cluster to run 10 replicas.

![](pics/chaos-center11.png)



- **Install Cluster Autoscaler**

![](pics/cluster-autoscaler.png)


- **Create an IAM policy and role**

![](pics/iam-role-policy.png)

To verify if the policy was created, go to **IAM console**, on the left click on **Policies**, the policy created must appear at the top of the list:

![](pics/iam-role-policy1.png)


Create an IAM role and attach an IAM policy to it using eksctl.

![](pics/iam-role-policy2.png)

If **No Task** is showing at the bottom. One way to troubleshoot that, is to navigate the CloudFormation 

**Note this below:**
![](pics/iam-role-policy3-troubshout.png)


Then run the previous comman again:

![](pics/iam-role-policy4.png)


To make sure the service account with the ARN of the IAM role is annotated. Type this command:


![](pics/iam-role-policy5.png)


- **Deploy the Cluster Autoscaler**

Let's download the Cluster Autoscaler manifest.

![](pics/deploy-cluster.png)


Edit the downloaded file to replace <YOUR CLUSTER NAME> with the cluster name (eks-litmus-demo) and add the following two lines.

```
- --balance-similar-node-groups
- --skip-nodes-with-system-pods=false
```

In order to edit the file, type this command:

![](pics/deploy-cluster1.png)

then a file will appear, navigate at the bottom, look for **command** and edit as follow:

![](pics/cluster-autoscalerADDON.png)


Apply the manifest file to the cluster.

![](pics/deploy-cluster2.png)


Then, let's patch the deployment to add the cluster-autoscaler.kubernetes.io/safe-to-evict annotation to the Cluster Autoscaler pods with the following command.

![](pics/deploy-cluster3.png)



To find the latest Cluster Autoscaler version that matches the Kubernetes major and minor versions of my cluster. Also, to Set the Cluster Autoscaler image tag to the version that was exported in the previous step with the following command.


![](pics/deploy-cluster4.png)


![](pics/deploy-cluster5.png)



Now the Cluster Autoscaler is deployed, let’s rerun the same experiment by navigating to Litmus Workflows, then the Schedules tab. Select the three dots menu icon for the workflow and select Rerun Schedule.

![](pics/litmus-workflow.png)

![](pics/litmus-workflow1.png)


![](pics/litmus-workflow2.png)

If the autoscale-chaos-workflow **failed**


### Experiment Conclusion

Autoscaling the pod triggered the ClusterAautoscaler as a result of insufficient capacity, and when a new node was added to the cluster, and the pods were successfully provisioned.


### Next steps

 The above walkthrough, I demonstrate how to get started with Chaos Engineering using LitmusChaos on Amazon EKS cluster. Nevertheless, there are additional experiments such as **pod-delete**, **node-drain**, **node-cpu-hog**, and so on that I can integrate with a CI/CD pipeline to perform Chaos Engineering. LitmusChaos also supports **gitops** and advanced chaos workflows using **Chaos Workflows**.

- ### Pod-Delete

When initiate Pod Delete,it contains chaos to disrupt state of kubernetes resources. Experiments can inject random pod delete failures against specified application: 
   - Causes (forced/graceful) pod failure of random replicas of an application deployment.
   - Tests deployment sanity (replica availability & uninterrupted service) and recovery workflows of the application pod.

   
#### PRE-REQUISITE:
- **Install Litmus Operator:** a tool for injecting Chaos Experiments


### Installation

companies looking to use Litmus for the first time have two options available to them today. One way is to use a hosted Litmus service like [ChaosNative Litmus Cloud](https://cloud.chaosnative.com/). Alternatively, they looking for some more flexibility that can install Litmus into their own Kubernetes cluster.

Those choosing the self-hosted option can refer to this Install and Configure docs for installing alternate versions and more detailed instructions.

- Self-Hosted
- Hosted (Beta)

Installation of Self-Hosted Litmus can be done using either of the below