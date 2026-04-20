# Project Documentation: Shillu-BRB011

Welcome to your project documentation. This guide consolidates all essential information, operational workflows, and the technical scope of your DevOps engineering responsibilities for this initiative.

---

## 🏗️ 1. Role & Project Overview
You are working as a **DevOps Engineer** supporting enterprise-scale initiatives in a highly regulated environment. Your primary focus is navigating the full lifecycle of infrastructure provisioning, configuration management, and CI/CD operations. 

Your tech stack blends application development knowledge with deep operational automation:
* **Core:** Java, Spring Boot, Gradle
* **Automation:** Puppet, Ansible (and occasionally Chef)
* **Infrastructure:** Linux Systems, VMaaS (Virtual Machine as a Service)
* **Version Control & CI/CD:** Git, GitLab CI/CD pipelines

---

## 🚀 2. Current Initiative: Production Server Setup
You are currently tasked with handling end-to-end setups for production servers (from *VMaaS* to *Puppet* to *Ansible*). You will track your progress through assigned Jira stories (e.g., **MIAM-38005, MIAM-38006, MIAM-38007, MIAM-38008**).

### Task Breakdown for the Prod Setup lifecycle:
1. **Provision VMs:** Set up the physical instances using VMaaS.
2. **Execute Pipeline:** Leverage the VMaaS pipeline to spin up `prodmbrid*` (and similar) VMs.
3. **Puppet Configuration:** Validate that the Puppet profile runs properly—specifically ensuring it creates the `sadmin` user and configures remote SSH access.
4. **Ansible App Deployment:** Utilize Ansible to install and configure the **DS (Directory Service)** profile directly onto the servers.
5. **Replication Verification:** Check horizontal syncing to ensure server replication is fully functional.
6. **Directory Connection Validation:** Test active connections using **Apache Directory Studio** and/or verify utilizing the `ldapsearch` binary from securely connected boxes.

---

## 🛣️ 3. Standard Operating Procedures (SOPs)

### A. GitLab Merge Request (MR) Approval Pipeline
To elevate code, trigger manual pipelines, or merge updates, abide by the following MR flow:

1. **Source/Target Branches:** Ensure your source is the `Vmaas tutorial` (or specified development branch) targeting the `Master` branch.
2. **Title Naming Convention:** You **must** prefix all MRs with your Jira story ID. 
   * **Format:** `[MIAM-xxxxx] [TITLE IN ALL CAPS]`
   * **Example:** `MIAM-38005 DIRECTORY SERVICE EMPLOYEE UPGRADE TO 8.0.2`
3. **Getting Approvals:**
   * **< 100 lines of code:** Drop the MR URL into the `#mgs-gitlab` channel.
   * **> 100 lines of code:** Attend an "MR Party" gathering to secure approvals contextually.
   * *Requirement:* You need **2 solid approvals**.
4. **Merging & Validation:** You cannot merge yourself. A Queue Manager or tech lead will press **Merge**. Once merged:
   * View `Master` -> `Commits` to verify.
   * Check Puppet or the deployment portal using the deployment identity pattern to confirm VM spin-up.

### B. Triggering the Build Pipeline against Target Servers
When executing the build pipeline targeting specific boxes in GitLab:
1. Navigate to the **Build Pipeline** menu and specify the `Master` (or `Vmaas`) branch.
2. Set configuration params:
   * **Input Variable Key:** `TGT_CONFIS`
   * **Input Variable Value:** `<Your Target Servers>`
3. **Attestation/Run Verification:** 
   * Hit 'RUN' -> Attestation manual review -> Select **"CONFIRMATION YES"**.
   * Validate sizing parameters in the blueprint (e.g., `dempcts12|sat(puppet)`).
4. **Validation Buffer:** The `APPLY REVIEW` phase will run for ten to fifteen minutes. You must validate the build output within 24 hours before promoting changes further.

### C. Creating Deployment Tags (Push to Prod)
To execute the final push to production environments from `https://prodgitlab.usaa.com/grp-forgerock-vmaas`, you will utilize tags.
1. **Base Branch:** Always create tags derived strictly from `Master`.
2. **Tag Naming:** Use incremental formats indicating the release (e.g., `GitLab.vm[2.0.2]`).
3. **Tag Description:** Include the story identifier in your description.
   * *Example:* "tagging master branch to deploy the IDM changes specified in story (MIAM-38005)"
4. *Action:* Creating this tag fires the master pipeline, enacting the push to PROD.

---

## 🛠️ 4. Local Workflows & Troubleshooting 
* **Linux Handling:** Know your way around user permissions, firewalls, manual patching and the core Linux command line toolkit (Bash scripting).
* **Logs & Telemetry:** If an application state breaks down across application boundaries or the OS layer, immediately check logs for pipeline states and utilize standard observability tools.
* **Java/Gradle:** To reproduce issues or test integrations locally, use IntelliJ or Eclipse. Rely heavily on Gradle application builds and debugging features. 

*Keep this documentation updated as pipeline environments or tech specifications change over sprints!*
