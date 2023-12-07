# Module 6 - Clean up

1. Delete the example application stacks.

   ```bash
   kubectl delete -f pre/005-vote-app-manifest.yaml
   ```

2. Delete the EKS cluster.

   ```bash
   source ~/workshopvars.env
   eksctl delete cluster \
     --name $CLUSTERNAME \
     --region $REGION
   ```

3. Delete this repo

   ```bash
   cd .. && rm -Rf cc-eks-implement-runtime-security
   ```

4. Delete environment variables backup file.

   ```bash
   rm ~/workshopvars.env
   ```

---

[:arrow_left: Module 5 - Quarantine Infected Workloads and Visualize KSPM](module-5-quarantine-kspm.md)  <br>

[:leftwards_arrow_with_hook: Back to Main](../README.md)
