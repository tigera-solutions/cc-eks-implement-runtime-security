
# Module 1 - Connect your EKS cluster to Calico Cloud

> :warning: To complete this module, you will need a [Calico Cloud account](https://www.calicocloud.io/). If you are participating in a live workshop, you will receive an invite with the information to login into an active Calico Cloud environment. Once logged in,  join your EKS cluster there.
If you are running this workshop in a self-paced mode, you can create a Calico Cloud environment following the steps [here](submodule-1.1-create-calicloud.md).  

Issues with being unable to navigate menus in the UI are often due to browsers blocking scripts - please ensure that you disable all blocker scripts.

## Step 1 - Accept the Invitation

1. During the workshop, you will receive an invitation to connect to a Calico Cloud organization, just like in the image below:

   <img width="500" alt="invite email" src="https://github.com/tigera-solutions/cc-eks-visualize-identify-security-gaps/assets/104035488/05838246-63b7-4d91-bf83-2166efdfe1df">

2. Click on the link ACCEPT INVITATION and create a password ① and click `Continue` ② to access the Calico Cloud.

   <img width="500" alt="create a password" src="https://github.com/tigera-solutions/cc-eks-visualize-identify-security-gaps/assets/104035488/830dda1f-c7cd-49f0-949b-d7d6d2d73b5d">

3. Once you have access to your **Calico Cloud** environment, go to step 2:

## Step 2 - Connect your cluster to Calico Cloud.

1. The welcome screen will allow you to choose among four use cases and will provide a guided tour for each use case. After that, you can proceed to connect your first cluster. This option directs you to the **Managed Clusters** section. Click on the "**Connect Cluster**" button to start the process.

   The Connect Cluster window will allow you to choose a name to identify your cluster in Calico Cloud and select which platform you are running the cluster on. The next window presents a link for you to review the cluster requirements for Calico Cloud. Calico Cloud will generate a kubectl command to run the installation script, and you need to copy and apply this command in your cluster.

   > **:warning: Attention**: During the live workshop, the Calico Cloud cluster will be shared. To avoid confusion, you can create a random prefix for your cluster name. This name can be achieved using the following command on the Cloud9 prompt:

    ```bash
    echo $RANDOM-tigera-workshop
    ```

   ![connect-a-cluster](https://github.com/tigera-solutions/cc-eks-visualize-identify-security-gaps/assets/104035488/c657798e-8828-4532-b0d1-57ba59194b66)
  
2. Run the installation script in your cluster. Script should look similar to this:

    <pre>
    kubectl apply -f https://installer.calicocloud.io/manifests/cc-operator/lat
    est/deploy.yaml && curl -H "Authorization: Bearer a7c2oex34:00llxrhcq:1ga2c
    z69d7ug81yjgakpyclv6o3eu8o97kp7t2483lmwajslu47xed94e4ic8ywn" "https://www.c
    alicocloud.io/api/managed-cluster/deploy.yaml" | kubectl apply -f -
    </pre>

    > Output should look similar to:
    <pre>
    namespace/calico-cloud created
    customresourcedefinition.apiextensions.k8s.io/installers.operator.calicocloud.io created
    serviceaccount/calico-cloud-controller-manager created
    role.rbac.authorization.k8s.io/calico-cloud-leader-election-role created
    clusterrole.rbac.authorization.k8s.io/calico-cloud-metrics-reader created
    clusterrole.rbac.authorization.k8s.io/calico-cloud-proxy-role created
    rolebinding.rbac.authorization.k8s.io/calico-cloud-leader-election-rolebinding created
    clusterrolebinding.rbac.authorization.k8s.io/calico-cloud-installer-rbac created
    clusterrolebinding.rbac.authorization.k8s.io/calico-cloud-proxy-rolebinding created
    configmap/calico-cloud-manager-config created
    service/calico-cloud-controller-manager-metrics-service created
    deployment.apps/calico-cloud-controller-manager created
    % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                    Dload  Upload   Total   Spent    Left  Speed
    100   355  100   355    0     0    541      0 --:--:-- --:--:-- --:--:--   541
    secret/api-key created
    installer.operator.calicocloud.io/aks-cc-repo created
    </pre>

    Joining the cluster to Calico Cloud can take a few minutes. Meanwhile the Calico resources can be monitored until they are all reporting `Available` as `True`.

    ```bash
    kubectl get tigerastatus                                    
    ```

    > Output should look similar to:
    <pre>
    NAME                            AVAILABLE   PROGRESSING   DEGRADED   SINCE
    apiserver                       True        False         False      96s
    calico                          True        False         False      16s
    compliance                      True        False         False      21s
    intrusion-detection             True        False         False      41s
    log-collector                   True        False         False      21s
    management-cluster-connection   True        False         False      51s
    monitor                         True        False         False      2m1s
    </pre>

    You can also monitor your cluster installation on the Calico Cloud UI. Go to the "**Managed Clusters**" section, select your cluster and expand the timestamp dropdown to see the installation logs.
    In a few minutes, the status will change from **Installing** to **Connected**. Congratulations! You successfully connected your cluster to Calico Cloud.

    ![installed](https://github.com/tigera-solutions/cc-eks-visualize-identify-security-gaps/assets/104035488/c6feedba-ffb9-4101-be8d-47c9488d93a2)

## STEP 3 - Select your cluster

Once the installation is completed, you can start interacting with your cluster from the Calico Cloud interface. Calico Cloud provides a single pane of glass for managing multiple clusters. If you followed the previous steps, you would have two clusters connected to Calico Cloud at this point: Your cluster and a pre-configured lab cluster that allows you to explore some of the features in Calico Cloud.

You can switch between clusters by following the steps below:

1. Navigate to the Dashboard section - the second icon under the Calico Cat on the top-left of the UI.

2. Click on the **Cluster** dropdown button on the top-right of the UI.

3. Select your recently added cluster.

   ![select-cluster](https://github.com/tigera-solutions/cc-eks-visualize-identify-security-gaps/assets/104035488/66cc3cd5-9661-47c6-a4ab-921ae43e44c3)

The "**Cluster**" dropdown button will always be visible across the Calico Cloud UI, no matter which section you are viewing. You can change the cluster you want to interact with at any moment. 
When you change the cluster, the Calico Cloud context will change immediately to reflect the information regarding the currently selected cluster.

--- 

[:arrow_right: Module 2 - Customize Security Guardrails for Network-based Threats](module-2-security-guardrails.md)  <br>

[:arrow_left: Back to Main](../README.md)
