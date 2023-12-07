# Module 2 - Customize Security Guardrails for Network-based Threats

Calico provides methods to enable fine-grained access controls between microservices and external databases, cloud services, APIs, and other applications.

In this module, we will learn how to use Calico to create network policies to control access to and from a pod. We will install the `Example Voting Application`. Once the application is deployed, we will create and test network security policies with different ingress and egress rules to demonstrate how **workload access control** is done.

1. Clone this repository in your Azure Cloud Shell.

   ```bash
   git clone https://github.com/tigera-solutions/cc-eks-implement-runtime-security.git && \
   cd cc-eks-implement-runtime-security
   ```

2. Installing the example application stacks:

   From the cloned directory, execute:

   ```bash
   kubectl apply -f pre
   ```

Included in the `pre` folder, there is an application that will be used in the exercises during the workshop. The diagram below shows how the elements of this application communicate between themselves.

![vote](https://github.com/tigera-solutions/cc-eks-compliance/assets/104035488/142ea62b-be3e-4c37-b39c-b501e1834f89)

There are also other objects that will be created. We will learn about them later in the workshop.

   > **Note**: Wait until all the pods are up and running to move to the next step.

## Service Graph and Flow Visualizer

Connect to Calico Cloud GUI. From the menu, select `Service Graph > Default`. Explore the options.

![service_graph](https://user-images.githubusercontent.com/104035488/192303379-efb43faa-1e71-41f2-9c54-c9b7f0538b34.gif)

Connect to Calico Cloud GUI. From the menu select `Service Graph > Flow Visualizations`. Explore the options.

![flow-visualization](https://user-images.githubusercontent.com/104035488/192358472-112c832f-2fd7-4294-b8cc-fec166a9b11e.gif)

## Security Policies

Calico Security Policies provide a richer set of policy capabilities than the native Kubernetes network policies, including:  

- Policies that can be applied to any endpoint: pods/containers, VMs, and/or to host interfaces
- Policies that can define rules that apply to ingress, egress, or both
- Policy rules support:
  - Actions: allow, deny, log, pass
  - Source and destination match criteria:
    - Ports: numbered, ports in a range, and Kubernetes named ports
    - Protocols: TCP, UDP, ICMP, SCTP, UDPlite, ICMPv6, protocol numbers (1-255)
    - HTTP attributes (if using Istio service mesh)
    - ICMP attributes
    - IP version (IPv4, IPv6)
    - IP or CIDR
    - Endpoint selectors (using label expression to select pods, VMs, host interfaces, and/or network sets)
    - Namespace selectors
    - Service account selectors

### The Zero Trust approach

A global default deny policy ensures that unwanted traffic (ingress and egress) is denied by default. Pods without policy (or incorrect policy) are not allowed traffic until the appropriate network policy is defined. Although the staging policy tool will help you find the incorrect or the missing policy, a global deny policy helps mitigate other lateral malicious attacks.

By default, all traffic is allowed between the pods in a cluster. Let's start by testing connectivity between application components and across application stacks. All of these tests should succeed as there are no policies in place.

We recommend creating a global default deny policy after you complete writing the policy for the traffic you want to allow. Use the stage policy feature to get your allowed traffic working as expected, then lock down the cluster to block unwanted traffic.

1. Create a staged global default-deny policy. It will show all the traffic that would be blocked if it were enforced.

   - Go to the `Policies Board`
   - On the bottom of the tier box `default` click on `Add Policy`
     - In the `Create Policy` page enter the policy name: `default-deny`
     - On the `Applies To` session, click `Add Namespace Selector`
       First, lets apply only to the `vote` namespace
       - Select Key... `kubernetes.io/metadata.name`
       - =
       - Select Value... `vote`
     - On the field `Type` select both checkboxes: Ingress and Egress.
     - You are done. Click `Stage` on the top-right of your page.

   The staged policy does not affect the traffic directly but allows you to view the policy impact if it were to be enforced. You can see the denied traffic in the staged policy.

2. Now, let's use the **Policy Recommendations** feature to create the policies for the ```vote``` application.

   - Select the correct cluster context on the top right, then in the left hamburger menu click on ```Policies > Recommendations```

    ![policy_gui_1](https://github.com/tigera-solutions/cc-eks-blueprint-secpos-workshop/assets/117195889/3980f84a-0128-4e28-b023-f79450658e56)

   - You will be presented with a page to enable the feature. Click the ```Enable Policy Recommendations``` button to instantiate the daemonset/pods for the feature.

    ![enable_policy_reco](https://github.com/tigera-solutions/cc-eks-blueprint-secpos-workshop/assets/117195889/720a7cd9-bc9b-4733-9b4d-7599d9d6c188)

     You can check that ```policy-recommendation``` shows ```True``` under the ```AVAILABLE``` column when you run ```kubectl get tigerastatus```

   ```bash
   NAME                            AVAILABLE   PROGRESSING   DEGRADED   SINCE
    apiserver                       True        False         False      123m
    calico                          True        False         False      125m
    cloud-core                      True        False         False      125m
    compliance                      True        False         False      123m
    image-assurance                 True        False         False      123m
    intrusion-detection             True        False         False      123m
    log-collector                   True        False         False      123m
    management-cluster-connection   True        False         False      123m
    monitor                         True        False         False      124m
    policy-recommendation           True        False         False      123m
    ```

   - In the Calico Cloud GUI, click on ```Global Settings``` on the top right and make the ```Stabilization Period``` and ```Processing Interval``` a bit more aggressive to have the policy recommendations show up more quickly.

    ![glob_Settings_location](https://github.com/tigera-solutions/cc-eks-blueprint-secpos-workshop/assets/117195889/f9b4a7be-0869-48cc-9337-052a3693270a)

    ![glob_Setting_short](https://github.com/tigera-solutions/cc-eks-blueprint-secpos-workshop/assets/117195889/a74a774a-4128-44cb-a4a0-65bc56adbdb3)

   - Once the traffic is analyzed and the policies show up in the ```Recommendations``` section, select the policies we are interested in for the ```vote``` application, select the ```Bulk Actions``` option and ```Add to policy board``` .

    ![add_to_policy_board](https://github.com/tigera-solutions/cc-aks-workshop-implement-runtime-security/assets/117195889/c39ab483-83a1-41ee-8c4d-f45721ba2a75)

    ![add_button](https://github.com/tigera-solutions/cc-aks-workshop-implement-runtime-security/assets/117195889/051a4d2d-1440-4c05-81c0-aaa947082e2d)

   - Navigating to the policy board should show the staged policies are in their own tier called namespace-isolation and we are ready review them by clicking on them. We have the namespace isolation policy as well as the default-deny in a ```Staged``` policy mode so that we can properly review the impact before we apply them.

    ![policy_board_full](https://github.com/tigera-solutions/cc-aks-workshop-implement-runtime-security/assets/117195889/7baada03-94e6-427a-b5de-cae1b148611a)

   - Review all the rules for the generated ```vote-``` recommended policy that we added to the board, hit ```Edit``` on the top right, and ```Enforce``` the staged policy.

    ![review_vote](https://github.com/tigera-solutions/cc-aks-workshop-implement-runtime-security/assets/117195889/0c87d34b-3c4f-4c19-8905-8fdac88cdc62)
    ![enforce_button](https://github.com/tigera-solutions/cc-aks-workshop-implement-runtime-security/assets/117195889/bcb5e135-bc72-4514-9fff-7c1285704587)

   - Review the ```default-deny``` policy to ensure that legitimate flows are no longer going to be blocked upon apply using the preview mode. Click into the policy and hit ```Edit Policy``` on the top right and then click on the ```Preview``` button to see the impact of the staged policy upon apply. Pick a recent time range since the ```vote``` recommended policy was applied.

    ![edit_def_deny](https://github.com/tigera-solutions/cc-aks-workshop-implement-runtime-security/assets/117195889/dc75128a-5dc0-488a-ba1d-4d2b7be8f4e7)

    ![policy_preview_1](https://github.com/tigera-solutions/cc-aks-workshop-implement-runtime-security/assets/117195889/4fcd5496-e0b5-4a27-82fd-61c9501f286b)

    The result should show no denied traffic within the ```vote``` namespace anymore.

    ![policy_preview_2](https://github.com/tigera-solutions/cc-aks-workshop-implement-runtime-security/assets/117195889/1d9d59fa-921a-42d1-a60a-5dbeb1b16a77)

   - Once satisfied with the result, ```Enforce``` the default-deny policy. The ```vote``` namespace is now secure.

### Bonus - About Tiers

Tiers are a hierarchical construct used to group policies and enforce higher precedence policies that other teams cannot circumvent, providing the basis for **Identity-aware micro-segmentation**.

All Calico and Kubernetes security policies reside in tiers. You can start “thinking in tiers” by grouping your teams and the types of policies within each group, such as security, platform, etc.

Policies are processed in sequential order from top to bottom.

![policy-processing](https://user-images.githubusercontent.com/104035488/206433417-0d186664-1514-41cc-80d2-17ed0d20a2f4.png)

Two mechanisms drive how traffic is processed across tiered policies:

- Labels and selectors
- Policy action rules

For more information about tiers, please refer to the Calico Cloud documentation [Understanding policy tiers](https://docs.calicocloud.io/get-started/tutorials/policy-tiers)

---

[:arrow_right: Module 3 - Configuring the Workload-Centric WAF](module-3-waf.md)  <br>

[:arrow_left: Module 1 - Connect the EKS cluster to Calico Cloud](module-1-connect-calicocloud.md) <br>

[:leftwards_arrow_with_hook: Back to Main](../README.md)  
