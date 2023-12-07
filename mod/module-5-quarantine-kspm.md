# Module 5 - Quarantine Infected Workloads and Visualize KSPM

## Quarantine a Workload

Suppose you have a compromised workload in your environment and want to conduct further investigation on it. In that case, you should not terminate the workload but isolate it, so it will not be able to cause damage or spread laterally across your environment. In this situation, you should quarantine the pod by applying a security policy to it that will deny all the egress and ingress traffic and log all the communications attempts from and to that pod.

We have the `quarantine` policy created in the `security` tier. This policy has a label selector of `quarantine = true`. Let's see how it works.

1. Execute the following commands from the attacker pod (if you did quit from its shell, it got deleted. Create it again if it's the case.).

   - Test the connection to a local service

     ```bash
     curl -m3 http://vote.vote
     ```

   - Test the connectivity with the Kubernetes API

     ```bash
     curl -m3 -k https://kubernetes:443/versions
     ```  

   - Test the connectivity with the internet

     ```bash
     curl -m3 http://neverssl.com
     ```  

2. Label the attacker pod with `quarantine = true`.

   ```bash
   kubectl label pod attacker quarantine=true
   ```

3. Repeat the tests from step 1. Now, as you can see, the cannot establish communication with any of the destinations.

---

## Visualize security posture of your Kubernetes cluster

### Timeline

What changed, who did it, and when? This information is critical for security. Native Kubernetes doesn’t provide an easy way to capture audit logs for pods, namespaces, service accounts, network policies, and endpoints. The Calico Cloud timeline provides audit logs for all changes to network policy and other resources associated with your Calico Cloud deployment.

1. On the Calico Cloud GUI, navigate to `Activity` and explore the entries in the `Timeline`.

![timeline](https://github.com/tigera-solutions/cc-aks-detect-block-network-attacks/assets/104035488/27bfeaff-4c1a-4d3d-b5c4-5234ecb13a52)

### Compliance Reports

Continuous compliance means employing a continual audit that shows what traffic was allowed in your infrastructure, what traffic was denied and why, and logs of who was trying to change what and whether those changes went into effect. Continuous compliance allows teams to pinpoint any point in time, say with reasonable certainty, whether the organization was compliant, and provide documentation to prove it. Calico’s compliance reports visually describe the security controls in place in an easy-to-understand policy view. Calico also shows all workloads that are in-scope and out-of-scope with your policy.

Using the reporting feature of Calico Cloud we can create a number of reports types to ensure security and compliance requirements.

Calico Cloud supports the following built-in report types:

- Inventory
- Network Access
- Policy-Audit
- CIS Benchmark

These reports can be customized and scoped to report against a certain set of endpoints, namespace or cluster-wide.

Compliance reports provide the following high-level information:

- **Protection**
  - Endpoints explicitly protected using ingress or egress policy
  - Endpoints with Envoy enabled
- **Policies and services**
  - Policies and services associated with endpoints
  - Policy audit logs
- **Traffic**
  - Allowed ingress/egress traffic to/from namespaces
  - Allowed ingress/egress traffic to/from the internet

## Example Reports

### Daily Report - Endpoint Inventory

The following report schedules daily inventory reports.

```yaml
kubectl apply -f -<<EOF
apiVersion: projectcalico.org/v3
kind: GlobalReport
metadata:
  name: daily-inventory
spec:
  reportType: inventory
  schedule: 8 6 * * *
EOF
```

[Inventory Endpoints Example](../compliance-reporting/inventory-endpoints.csv)<br>
[Inventory Summary Example](../compliance-reporting/inventory-summary.csv)

### Daily Report - Endpoint Network Access

The following report schedules daily network-access reports for all endpoints.

```yaml
kubectl apply -f -<<EOF
apiVersion: projectcalico.org/v3
kind: GlobalReport
metadata:
  name: daily-network-access
spec:
  reportType: network-access
  schedule: 0 1 * * *
EOF
```

[Network Access Endpoints Example](../compliance-reporting/network-access-endpoints.csv)<br>
[Network Access Summary Example](../compliance-reporting/network-access-summary.csv)

### Policy Audit

The following report schedules a policy audit of the cluster at the frequency of your choosing

```yaml
kubectl apply -f -<<EOF
apiVersion: projectcalico.org/v3
kind: GlobalReport
metadata:
  name: daily-policy-audit
spec:
  reportType: policy-audit
  schedule: 0 0 * * *
EOF
```

[Policy Audit Summary Example](../compliance-reporting/policy-audit-summary.csv)

### CIS Benchmark Report

The following report schedules a CIS benchmark report of the cluster at the frequency of your choosing and user-defined scoring thresholds.

```yaml
kubectl apply -f -<<EOF
apiVersion: projectcalico.org/v3
kind: GlobalReport
metadata:
  name: daily-cis-benchmark
spec:
  reportType: cis-benchmark
  schedule: 0 0 * * *
  cis:
    highThreshold: 100
    medThreshold: 50
    includeUnscoredTests: true
    numFailedTests: 5
    resultsFilters:
      - benchmarkSelection: { kubernetesVersion: "1.27" }
        exclude: ["1.1.4", "1.2.5"]
EOF
```

[CIS Benchmark All Tests Example](../compliance-reporting/cis-benchmark-all-tests.csv)<br>
[CIS Benchmark All Tests Total Summary Example](../compliance-reporting/cis-benchmark-total-summary.csv)

### Generate reports manually

- Change values as needed in the ```generate-reports.sh``` script
- Run the script from the base repo path ```bash scripts/generate-reports.sh```
- Check that the runtime reporter pods have spun up and completed the jobs under the ```tigera-compliance``` namespace.
- Check the Calico Cloud UI for the new reports to be populated.

### Explore Compliance Reports in Calico Cloud UI

- On the Calico Cloud GUI, navigate to `Compliance`.

![compliance-reports](https://user-images.githubusercontent.com/104035488/192358634-c873ffb5-f874-495f-8ba4-79806ff84654.gif)

- Download the compliance report CSV for more details.

![cis-benchmark](https://user-images.githubusercontent.com/104035488/192358645-ab77c305-0a9d-4242-b37f-972dc22b4d84.gif)

**Congratulations! You completed this workshop!**

---

[:arrow_right: Module 6 - Clean up](module-6-clean-up.md)  <br>

[:arrow_left: Module 4 - Zero-Day Attacks Detection with Threat Defense](module-4-threat-defense.md)  <br>

[:leftwards_arrow_with_hook: Back to Main](../README.md)  
