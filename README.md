# Workshop: Amazon EKS Security Bootcamp: </br> Implement runtime security

## Welcome

In this EKS-focused workshop, you will work with AWS and Calico Cloud to learn how to protect containers in your Kubernetes cluster from known and zero-day threats based container and network attacks running on Amazon EKS.

In an increasingly interconnected and digital world, the security of your EKS clusters is non-negotiable. This workshop equips you with the knowledge and skills to fortify your cluster's defenses comprehensively, ensuring the protection of critical workloads and sensitive data. It empowers you to customize security measures to fit your organization's unique requirements and keeps you ahead of the curve in a rapidly evolving cybersecurity landscape.

Cybersecurity expertise is in high demand and mastering the art of securing Kubernetes environments is a valuable skill set. Whether you're an IT administrator, developer, or security professional, the insights gained in this workshop will enhance your proficiency and make you an invaluable asset to your organization. Additionally, by proactively implementing robust security practices, you can save your organization from potential financial losses and reputational damage that can result from security breaches.

Don't miss this opportunity to bolster your security knowledge, protect your infrastructure, and advance your professional journey.

You will come away from this workshop with an understanding of how others in your industry are securing and observing cloud-native applications in AWS, along with best practices that you can implement in your organization.

### Time Requirements

The estimated time to complete this workshop is 60-90 minutes.

### Target Audience

- Cloud Professionals
- DevSecOps Professional
- Site Reliability Engineers (SRE)
- Solutions Architects
- Anyone interested in Calico Cloud :)

### Learning Objectives

- **Customize Security Guardrails**: Discover how to customize security guardrails to fortify your network against various threats.
- **Configure Workload-Centric WAF**: Dive into the configuration of the workload-centric Web Application Firewall (WAF) to safeguard your cluster from network-based threats.
- **Zero-Day Attack Detection**: Learn how to detect zero-day attacks by monitoring suspicious container activity, enabling you to proactively respond to emerging threats.
- **Security Policy Enforcement**: Preview and enforce security policies to quarantine infected workloads, preventing the spread of threats within your Kubernetes environment.
- **Visualize Security Posture**: Learn how to effectively visualize the security posture of your Amazon EKS cluster, allowing you to identify potential vulnerabilities and weaknesses.

## Workshop Environment Preparation

> :warning: **For this workshop, you may be expected to have access to a previously created EKS cluster. If so, follow the instructions below**

- Please, follow the instructions on the repository below if you don't have it ready:

  [Calico Cloud on EKS - Workshop Environment Preparation](https://github.com/tigera-solutions/eks-workshop-prep)

- You can run this workshop from the AWS CloudShell, as described in that repository or in your own terminal with eksctl and AWS CLI.

- To start your cluster, reload the environment variables created in your AWS Cloud Shell (or your local machine) first and then scale the nodegroup up to start the cluster. Assuming that your environment variables are saved to a file called ```workshopvars.env```, use the following command:

  ```bash
  source ~/workshopvars.env
  eksctl scale nodegroup $NGNAME \
  --cluster $CLUSTERNAME \
  --region $REGION \
  --nodes 2 \
  --nodes-max 2 \
  --nodes-min 2
  ```

## Modules

This workshop is organized in sequential modules. One module will build up on top of the previous module, so please, follow the order as proposed below.

Module 1 - [Connect the EKS cluster to Calico Cloud](mod/module-1-connect-calicocloud.md)  
Module 2 - [Customize Security Guardrails for Network-based Threats](mod/module-2-security-guardrails.md)  
Module 3 - [Configuring the Workload-Centric WAF](mod/module-3-waf.md)  
Module 4 - [Zero-Day Attacks Detection with Threat Defense](mod/module-4-threat-defense.md)  
Module 5 - [Quarantine Infected Workloads and Visualize KSPM](mod/module-5-quarantine-kspm.md)  
Module 6 - [Clean up](mod/module-6-clean-up.md)  

---

### Useful links

- [Project Calico](https://www.tigera.io/project-calico/)
- [Calico Academy - Get Calico Certified!](https://academy.tigera.io/)
- [O’REILLY EBOOK: Kubernetes security and observability](https://www.tigera.io/lp/kubernetes-security-and-observability-ebook)
- [Calico Users - Slack](https://slack.projectcalico.org/)

### Follow us on social media

- [LinkedIn](https://www.linkedin.com/company/tigera/)
- [Twitter](https://twitter.com/tigeraio)
- [YouTube](https://www.youtube.com/channel/UC8uN3yhpeBeerGNwDiQbcgw/)
- [Slack](https://calicousers.slack.com/)
- [Github](https://github.com/tigera-solutions/)
- [Discuss](https://discuss.projectcalico.tigera.io/)

> **Note**: The workshop provides examples and sample code as instructional content for you to consume. These examples will help you understand how to configure Calico Cloud and build a functional solution. Please note that these examples are not suitable for use in production environments.
