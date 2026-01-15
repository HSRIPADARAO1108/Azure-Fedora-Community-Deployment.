# Azure Lab: Fedora Community VM Deployment & Customization

## 1. Project Objective
The goal of this project was to explore the use of **Community VM Images** in the Azure Marketplace for rapid application deployment. This involved provisioning a Fedora Cloud VM, configuring cloud networking, and installing an Nginx web server to serve a live test page.

---

## 2. Deployment Process

### Phase 1: Resource Selection
* **Platform:** Microsoft Azure
* **Image Source:** Azure Community Gallery
* **Image Name:** `Fedora-Cloud-40-x64`
* **Region:** Canada Central
* **VM Size:** `Standard_B2s` (Gen2)

![VM Overview](Screenshot_2026-01-15_185132.png)
*Figure 1: VM Essentials showing 'Running' status and Public IP 130.107.72.238.*

### Phase 2: Configuration & Networking
* **Authentication:** SSH Public Key (.pem file).
* **Network Security Group (NSG):** Configured inbound rules to allow management and web traffic.
    * **Port 22:** SSH access.
    * **Port 80:** HTTP web traffic.

![Networking Rules](Screenshot_2026-01-15_185204.png)
*Figure 2: Inbound Security Rules showing 'AllowHTTP' on Port 80.*

### Phase 3: Customization
Once provisioned, the VM was customized via SSH:
1. **System Update:** Synchronized repositories using `dnf upgrade`.
2. **Web Server:** Installed Nginx.
3. **Verification:** Validated the server response locally within the VM.

![Terminal Access](Screenshot_2026-01-15_185314.png)
*Figure 3: SSH login and local verification using 'curl -I localhost'.*

---

## 3. Challenges & Solutions (Problem Log)

### Problem 1: Marketplace Purchase Eligibility
* **Error:** `MarketplacePurchaseEligibilityFailed`.
* **Solution:** Switched from a commercial "Plan" image to a direct Community Gallery image from the Fedora Project.

### Problem 2: SSH Key Permission Denied
* **Error:** Key permissions too open for Windows users.
* **Solution:** Used `icacls` in PowerShell to restrict access to the `.pem` file to the current user only.

### Problem 3: Command Not Found (firewall-cmd)
* **Error:** `sudo: firewall-cmd: command not found`.
* **Observation:** The minimal Fedora Cloud image does not include `firewalld`.
* **Solution:** Relied on the Azure Network Security Group (NSG) for traffic filtering.

### Problem 4: Connection Timed Out
* **Error:** Browser could not reach the IP.
* **Solution:** Added a specific Inbound Security Rule (Priority 320) to allow traffic on Port 80, which was previously blocked by default rules.

---

## 4. Final Verification
The deployment was successful. The Fedora Webserver Test Page is live and accessible via the public internet.

![Live Browser View](Screenshot_2026-01-15_185112.png)
*Figure 4: The live Fedora Webserver Test Page at http://130.107.72.238.*

---

## 5. Key Insights
* **Efficiency:** Using a Community Image reduced deployment time by providing a pre-configured OS environment.
* **Security:** Layered security is essential; even with the OS firewall disabled, the Azure Cloud NSG provides a robust first line of defense.
* **Troubleshooting:** Identifying the difference between OS-level errors and Cloud-level networking blocks is a vital skill for cloud administrators.
