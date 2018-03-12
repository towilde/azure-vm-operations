# VM Operation - Hands on Lab Guide

**Authors:**

Tom Wilde - Microsoft 

Dan Baker - Microsoft
 
# Contents

**[Prerequisites](#prereqs)**

**[Lab Introduction](#intro)**

**[Lab 1: Auto-shutdown](#as)**

**[Lab 1.1: Configure Auto-shutdown](#cas)**

**[Lab 2: Backup](#backup)**

**[Lab 2.1: Protecting a VM with Backup](#prot)**

**[Lab 2.2: Restoring a VM from Azure Backup](#restore)**

**[Lab 3: Disaster Recovery](#dr)**

**[Lab 3.1: Enabling a VM for Disaster Recovery](#edr)**

**[Lab 3.2: Configuring Test Failover](#tfo)**

**[Lab 4: Update Management](#um)**

**[Lab 4.1: Configure Update Management](#cum)**

**[Lab 5: Inventory](#inv)**

**[Lab 5.1: Configure Inventory Management](#cim)**

**[Lab 6: Change Tracking](#ct)**

**[Lab 6.1: View Change Tracking](#vct)**

**[Conclusion](#conclusion)**

**[Useful References](#ref)**

# Prerequisites <a name="prereqs"></a>

To complete this workshop, the following will be required:

- A valid subscription to Azure. If you don't currently have a subscription, consider setting up a free trial. If this workshop is being hosted by a Microsoft Cloud Solution Architect, Azure passes should be provided.

- At least one virtual machine to apply virtual machine operations on. 

# Lab Introduction <a name="intro"></a>

Contoso have recently migrated several of their on-premise resources to Microsoft Azure. These resources include virtual machines (both Windows 2016 and Ubuntu Linux), virtual networks and storage accounts. Unfortunately, as this is the first migration carried out, Contoso are somewhat unfamiliar with Azure (and public cloud platforms in general) – as a result, they haven't applied any virtual machine operations.

At the point of writing, virtual machine operations consist of [Auto-Shutdown](#as), [Backup](#backup), [Disaster Recovery](#dr), [Update Management](#um), [Inventory](#inv) and [Change Tracking](#cr).

![VM Operations](https://github.com/towilde/azure-vm-operations/blob/master/images/vm-operations.png "VM Operations")

**Figure 1:** VM Operations

The Contoso IT team have requested your help to manage the infrastructure resources that they have migrated to Azure. Current challenges:
1. Some virtual machines are not used outside working hours
2. Some virtual machines have critical data on them but have no backup configured at all
3. Critical virtual machines have no DR strategy
4. Update Management WSUS
5. Change Tracking


# Lab 1: Auto-shutdown <a name="as"></a>

Auto-shutdown speaks for itself, it's great from a cost saving perspective and commonly used in dev test scenarios where virtual machines are not needed after a specific time.

# Lab 1.1: Confugre Auto-shutdown <a name="cas"></a>

In this lab, we’ll use the auto-shutdown operation to schedule a virtual machine to shutdown at a specific time. 

**1)** In the Azure portal, go to your VM.

**2)** Under OPERATIONS click Auto-shutdown

**3)** Enable the feature, schedule the shutdown time for 5 minutes from now, confirm the correct local time zone is set, click save.

![Auto-Shutdown](https://github.com/towilde/azure-vm-operations/blob/master/images/auto-shutdown.png "Auto-Shutdown")

**Figure 2:** Auto-Shutdown

**4)** You can also get notifications via a webhook or email before the VM shuts down, around 15 minutes before the shutdown time.

Note: The auto-shutdown option has also been added as an optional extension when creating a new virtual machine using the Azure Portal.

# Lab 2: Backup  <a name="backup"></a>

Azure Backup enables you to back up and restore your data in the Microsoft cloud by replacing your existing on-premises or off-site backup solution with a reliable, secure, and cost-competitive alternative.

# Lab 2.1: Protecting a VM with Backup  <a name="prot"></a>

In this exercise we will enable Azure Backup on a virtual machine.

**1)** In the Azure portal, go to your VM.

**2)** Under OPERATIONS click Backup

**3)** Azure Backup requires a recovery vault. Create a new Recovery Services Vault named “vmbackup” and a new Backup Policy accepting the defaults. 

![Create Vault and backup Policy](https://github.com/towilde/azure-vm-operations/blob/master/images/Create-Vault-backup-Policy.png "Create Vault and backup Policy")

**Figure 3:** Create Vault and backup Policy

**4)** After a few minutes the configuration will be complete, and we’ll be able to test. To achieve this, head back into the VM blade and once again select “Backup” under Operations. From this window select “Backup Now” accepting the default retention before pressing the “OK” Button. 

![Backup now](https://github.com/towilde/azure-vm-operations/blob/master/images/backup-now.png "Backup now")

**Figure 4:** Backup now

**5)** From “Notifications” Select the “Triggering backup for the VM" notification to open up the Backup Jobs. Notice that the Backup of VM is in progress. 

![Backup in progress](https://github.com/towilde/azure-vm-operations/blob/master/images/backup-in-progress.png "Backup in progress")

**Figure 5:** Backup in progress

Note: This normally takes around 30 minutes to complete for an average size VM.

# Lab 2.2: Restoring a VM from Azure Backup  <a name="restore"></a>

In this exercise we’ll restore a Virtual Machine from the Azure Backup.

**1)** In the Azure portal, go to your VM.

**2)** Under OPERATIONS click Backup

**3)** In the top Menu select “Restore VM” and in the new blade that opens, select a restore point and the “OK” button.

![Restore VM](https://github.com/towilde/azure-vm-operations/blob/master/images/restore-vm.png "Restore VM")

**Figure 6:** Restore VM

**4)** In the “Restore Configuration” pane we can select our “Restore Type” From here we can either choose to “Create a New Virtual Machine” or “Restore Disks” to a staging location. For this exercise we will choose to “Create a New Virtual Machine” with a name of “VM2Restore”. 

![Restore Configuration](https://github.com/towilde/azure-vm-operations/blob/master/images/restore-configuration.png "Restore Configuration")

**Figure 7:** Restore Configuration

**5)** Click on the job Notification to launch the Backup Jobs window and Monitor Progress. 

![Restore Progress](https://github.com/towilde/azure-vm-operations/blob/master/images/restore-Progress.png "Restore Progress")

**Figure 8:** Restore Progress

Note: This normally takes around 30 minutes to complete for an average size VM.

**6)** Once complete, navigate to “Virtual Machines” from the left-hand side of the Azure portal. Notice that we now have a new VM present titled “VM2Restore” 

![VM Restored](https://github.com/towilde/azure-vm-operations/blob/master/images/vm-restored.png "VM Restored")

**Figure 9:** VM Restored

**7)** To test that the restoration has completed successfully attempt to log into “VM2Restore”.

# Lab 3: Disaster Recovery  <a name="dr"></a>

Azure Site Recovery helps ensure business continuity by keeping business apps and workloads running during outages. Site Recovery replicates workloads running on physical and virtual machines (VMs) from a primary site to a secondary location. 

# Lab 3.1: Enabling a VM for Disaster Recovery  <a name="edr"></a>

**1)** In the Azure portal, go to your VM.

**2)** Under OPERATIONS click Disaster Recovery, this will open the Disaster Recovery Configuration Pane. Notice the default Disaster Recovery Settings, including target Resource Group, Network, Disk and Replication pairs. When Ready click on the “Enable Replication” Button.

![Configure DR](https://github.com/towilde/azure-vm-operations/blob/master/images/configure-dr.png "Configure DR")

**Figure 10:** Configure DR

# Lab 3.2: Configuring Test Failover  <a name="tfo"></a>

**1)** When Replication has completed, in the Azure portal, go to your VM.

**2)** Under OPERATIONS click Disaster Recovery. Notice in the new blade that opens, you’re able to both Failover and Test Failover

![Failover Options](https://github.com/towilde/azure-vm-operations/blob/master/images/test-failover-dr.png "Failover options")

**Figure 11:** VM Restored

**3)** Click on the “Test Failover” button which will open a new blade.

**4)** From here notice how you can select the Recovery Point and Virtual Network. In this instance we will keep the latest recovery point and select the network.

**5)**  Click the “OK” Button to begin test failover

![Create failover test](https://github.com/towilde/azure-vm-operations/blob/master/images/run-test-failover.png "Create failover test")

**Figure 12:** Create failover test

**6)** When Test Failover Completes, click on the notification to open the job summary. Notice the stages the failover went through, in creating a test VM.

![Failover test stages](https://github.com/towilde/azure-vm-operations/blob/master/images/failover-test-details.png "Failover test stages")

**Figure 13:** Failover test stages

**7)** This proves that failover is supported and configured correctly so let’s navigate back into the Disaster Recovery section under the Operations Heading of our Virtual machine and select the “Clean up test failover” button.

**8)** From here type test completed in the Notes dialogue box and select to delete the test failover virtual machine(s) before pressing the “OK” button.

![Failover test clean-up](https://github.com/towilde/azure-vm-operations/blob/master/images/dr-failover-test-cleanup.png "Failover test clean-up")

**Figure 14:** Failover test clean-up

# Lab 4: Update Management  <a name="um"></a>

Azure Update Management allows you to manage operating system security updates for your Windows and Linux computers deployed in Azure, on-premises environments, or other cloud providers. You can quickly assess the status of available updates on all agent computers and manage the process of installing.
Computers managed by update management use the following configurations for performing assessment and update deployments:
•	Microsoft Monitoring agent for Windows or Linux
•	PowerShell Desired State Configuration (DSC) for Linux
•	Automation Hybrid Runbook Worker
•	Microsoft Update or Windows Server Update Services for Windows computers

# Lab 4.1: Configure Update Management  <a name="cum"></a>

In this exercise we will Configure Update Management for a Virtual Machine.

**1)** In the Azure portal, go to your VM.

**2)** Under OPERATIONS click Update Management

**3)** From the Update Management the blade that opens accept the defaults for the Log Analytics Workspace and Automation Account before pressing the “Enable” button. 

![Configure Update Management](https://github.com/towilde/azure-vm-operations/blob/master/images/enable-update-management.png "Configure Update Management")

**Figure 15** Configure Update Management


**4)** Notice the message stating that the solution will take a few minutes to provision. *"The 'Update Management' solution is being enabled on this virtual machine. This can take a few minutes. You can do other work while this is in progress."* And then another stating that the Virtual Machine is being configured.
*"The components for the 'Update Management' solution have been enabled, and now this virtual machine is being configured."* Please be patient, as this can sometimes take up to 15 minutes

**5)** Once complete you will notice that the “Update Management” blade lists many “Missing Updates” including links to the associated Knowledgebase Articles. 

![Missing updates](https://github.com/towilde/azure-vm-operations/blob/master/images/update-manager-missing-updates.png "Missing updates")

**Figure 16** Missing updates

**6)** From here, move up to the top menu bar and select “Schedule Update Deployment”. In the new blade that opens, enter a name, choose not to exclude updates, configure the deployment to start 20 Minutes after the “Current Time” and leave the Maintenance Window to the default 120 Minutes. Once configured, press “OK” followed by “Create” to continue.

![Schedule updates](https://github.com/towilde/azure-vm-operations/blob/master/images/schedule-updates.png "Schedule updates")

**Figure 17** Schedule updates

**7)** Back into the Update Management blade, notice under “Scheduled Update Deployments” how the schedule is now listed.

![Scheduled updates](https://github.com/towilde/azure-vm-operations/blob/master/images/scheduled-updates.png "Scheduled updates")

**Figure 18** Scheduled updates

**8)** After 20 minutes, click onto the “Update Deployments” tab to see the scheduled maintenance window open (Remember you have a 2 hour window)

![Scheduled updates in progress](https://github.com/towilde/azure-vm-operations/blob/master/images/scheduled-updatesip.png "Scheduled updates in progress")

**Figure 19** Scheduled updates in progress

# Lab 5: Inventory  <a name="inv"></a>

Get inventory of in-guest resources for visibility into installed applications as well as custom-defined configuration items. Azure Inventory allows rich reporting and search capability to help you quickly find detailed information and understand everything that is configured within your hybrid environment.

# Lab 5.1: Configure inventory Management  <a name="cim"></a>

In this exercise we will Configure Inventory Management.

**1)** In the Azure portal, go to your VM.

**2)** Under OPERATIONS click inventory

**3)** From the inventory blade enable the service by clicking on the “Enable” button. 

![Enable inventory](https://github.com/towilde/azure-vm-operations/blob/master/images/enable-inventry.png "Enable inventory")

**Figure 18** Enable inventory

Note: This can take up to 15 minutes

**4)** Once Provisioned, it will already be configured to collect data on the virtual machine. To enable data collection on other machines, select “Manage Multiple Machines” at the top of the blade and then “Add Azure VM” or “Add Non-Azure Machine” for cross cloud or on premises solutions.  

Note: Due to the collection time, the results may not be visible for about up to 60 minutes.

**5)** When configured, you will notice that the blade lists the VMs' software, Files, Registry and Services. 

![VM inventory](https://github.com/towilde/azure-vm-operations/blob/master/images/vm-inventry.png "VM inventory")

**Figure 19** VM inventory

# Lab 6: Change Tracking  <a name="ct"></a>

In this exercise we will view Change Management which allows you to track changes across services, daemons, software, registry, and files.

Note: This will only be visible once the Update Management Lab has completed and may have to be set as further study. 

# Lab 6.1: View Change Tracking  <a name="vct"></a>

**1)** In the Azure portal, go to your VM.

**2)** Under OPERATIONS click Change Tracking to see the results of our earlier lab. 

![Change Tracking](https://github.com/towilde/azure-vm-operations/blob/master/images/vm-inventry.png "Change Tracking")

**Figure 19** Change Tracking

# Conclusion <a name="conclusion"></a>

Well done, you made it to the end of the lab! Hopefully this guide has given you a good grounding in virtual machine operations. We could have gone into much more detail but space is limited! We hope you enjoyed running through the lab and that you learnt a few useful things from it. Don't forget to delete your resources after you have finished!

# Useful References <a name="ref"></a>



- **Azure DevTest Labs (auto-shutdown)**
https://azure.microsoft.com/en-us/services/devtest-lab/

- **Azure Backup:** https://docs.microsoft.com/en-us/azure/backup/backup-introduction-to-azure-backup

- **Azure Site Recovery (disaster recovery):** 
https://docs.microsoft.com/en-us/azure/site-recovery/site-recovery-overview

- **Azure Update Management**
https://docs.microsoft.com/en-us/azure/automation/automation-update-management

- **Azure inventory**
https://docs.microsoft.com/en-us/azure/automation/automation-vm-inventory

- **Azure Change Tracking**
https://docs.microsoft.com/en-us/azure/automation/automation-change-tracking
