---
lab:
    title: 'Lab 08: Manage Virtual Machines'
    module: 'Administer Virtual Machines'
---

# Lab 08 - Manage Virtual Machines

## Lab introduction

In this lab, you create and compare virtual machines to virtual machine scale sets. You learn how to create, configure and resize a single virtual machine. You learn how to create a virtual machine scale set and configure autoscaling.

This lab requires an Azure subscription. Your subscription type may affect the availability of features in this lab. You may change the region, but the steps are written using **East US**.

## Estimated timing: 50 minutes

## Lab scenario

Your organization wants to explore deploying and configuring Azure virtual machines. First, you implement an Azure virtual machine with manual scaling. Next, you implement a Virtual Machine Scale Set and explore autoscaling.

## Interactive lab simulations

There are interactive lab simulations that you might find useful for this topic. The simulation lets you to click through a similar scenario at your own pace. There are differences between the interactive simulation and this lab, but many of the core concepts are the same. An Azure subscription is not required.

+ [Create a virtual machine in the portal](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%201). Create a virtual machine, connect and install the web server role.

+ [Deploy a virtual machine with a template](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%209). Explore the QuickStart gallery and locate a virtual machine template. Deploy the template and verify the deployment.

+ [Create a virtual machine with PowerShell](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%2010). Use Azure PowerShell to deploy a virtual machine. Review Azure Advisor recommendations.

+ [Create a virtual machine with the CLI](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%2011). Use the CLI to deploy a virtual machine. Review Azure Advisor recommendations.

## Job skills

+ Task 1: Deploy zone-resilient Azure virtual machines by using the Azure portal.
+ Task 2: Manage compute and storage scaling for virtual machines.
+ Task 3: Create and configure Azure Virtual Machine Scale Sets.
+ Task 4: Scale Azure Virtual Machine Scale Sets.
+ Task 5: Create a virtual machine using Azure PowerShell (optional 1).
+ Task 6: Create a virtual machine using the CLI (optional 2).

## Azure Virtual Machines Architecture Diagram

![Diagram of the vm architecture tasks.](../media/az104-lab08-vm-architecture.png)

## Task 1: Deploy zone-resilient Azure virtual machines by using the Azure portal

In this task, you will deploy two Azure virtual machines into different availability zones by using the Azure portal. Availability zones offer the highest level of uptime SLA for virtual machines at 99.99%. To achieve this SLA, you must deploy at least two virtual machines across different availability zones.

1. Sign in to the Azure portal - `https://portal.azure.com`.

1. Search for and select `Virtual machines`, on the **Virtual machines** blade, click **+ Create**, and then select in the drop-down **Azure virtual machine**. Notice your other choices.

<img width="653" alt="image" src="https://github.com/Arinze008/Azure-Onboard-Automator/assets/139396868/efd2e2a3-b075-4486-b35c-47e8b286be38">


1. On the **Basics** tab, in the **Availability zone** drop down menu, place a checkmark next to **Zone 2**. This should select both **Zone 1** and **Zone 2**.

<img width="652" alt="image" src="https://github.com/Arinze008/Azure-Onboard-Automator/assets/139396868/0847106e-4260-44ca-b61f-8fde6a233631">


    >**Note**: This will deploy two virtual machines in the selected region, one in each zone. You achieve the 99.99% uptime SLA because you have at least two VMs distributed across at least two zones. In the scenario where you might only need one VM, it is a best practice to still deploy the VM to another zone.

1. On the Basics tab, continue completing the configuration:

    | Setting | Value |
    | --- | --- |
    | Subscription | the name of your Azure subscription |
    | Resource group |  **az104-rg8** (If necessary, click **Create new**) |
    | Virtual machine names | `az104-vm1` and `az104-vm2` (After selecting both availability zones, select **Edit names** under the VM name field.) |
    | Region | **East US** |
    | Availability options | **Availability zone** |
    | Availability zone | **Zone 1, 2** (read the note about using virtual machine scale sets) |
    | Security type | **Standard** |
    | Image | **Windows Server 2019 Datacenter - x64 Gen2** |
    | Azure Spot instance | **unchecked** |
    | Size | **Standard D2s v3** |
    | Username | `localadmin` |
    | Password | **Provide a secure password** |
    | Public inbound ports | **None** |
    | Would you like to use an existing Windows Server license? | **Unchecked** |

<img width="662" alt="image" src="https://github.com/Arinze008/Azure-Onboard-Automator/assets/139396868/4ea10bfb-9842-465b-8853-7e280fb763bb">

<img width="629" alt="image" src="https://github.com/Arinze008/Azure-Onboard-Automator/assets/139396868/5b9c0ece-0e0a-4ed9-97e2-1634b8595462">

<img width="632" alt="image" src="https://github.com/Arinze008/Azure-Onboard-Automator/assets/139396868/0fcb465e-478c-4c44-a195-dbfd3bb4b909">


    ![Screenshot of the create vm page.](../media/az104-lab08-create-vm.png)

1. Click **Next : Disks >** , specify the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | OS disk type | **Premium SSD** |
    | Delete with VM | **checked** (default) |
    | Enable Ultra Disk compatibility | **Unchecked** |

1. Click **Next : Networking >** take the defaults but do not provide a load balancer.

    | Setting | Value |
    | --- | --- |
    | Delete public IP and NIC when VM is deleted | **Checked** |
    | Load balancing options | **None** |
<img width="700" alt="image" src="https://github.com/Arinze008/Azure-Onboard-Automator/assets/139396868/ce5c8152-db1a-4da5-a785-432a6f36f674">


1. Click **Next : Management >** and specify the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Patch orchestration options | **Azure orchestrated** |  

<img width="661" alt="image" src="https://github.com/Arinze008/Azure-Onboard-Automator/assets/139396868/93f7bebe-9960-4a16-824c-ae6b163fc5eb">


1. Click **Next : Monitoring >** and specify the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Boot diagnostics | **Disable** |

<img width="669" alt="image" src="https://github.com/Arinze008/Azure-Onboard-Automator/assets/139396868/b1049cf8-11d5-4923-a80f-61e6fbd6dc3b">


1. Click **Next : Advanced >**, take the defaults, then click **Review + Create**.

1. After the validation, click **Create**.

    >**Note:** Notice as the virtual machine deploys the NIC, disk, and public IP address (if configured) are independently created and managed resources.

1. Wait for the deployment to complete, then select **Go to resource**.

   >**Note:** Monitor the **Notification** messages.

   <img width="881" alt="image" src="https://github.com/Arinze008/Azure-Onboard-Automator/assets/139396868/2a9b149c-29ff-4556-bab1-b16ec308cca0">


## Task 2: Manage compute and storage scaling for virtual machines

In this task, you will scale a virtual machine by adjusting its size to a different SKU. Azure provides flexibility in VM size selection so that you can adjust a VM for periods of time if it needs more (or less) compute and memory allocated. This concept is extended to disks, where you can modify the performance of the disk, or increase the allocated capacity.
 ![Screenshot of the resize the virtual machine.](../media/az104-lab08-resize-vm.png)

1. On the **az104-vm1** virtual machine, in the **Availability + scale** blade, select **Size**.
<img width="901" alt="image" src="https://github.com/Arinze008/Azure-Onboard-Automator/assets/139396868/921c2a4a-ce82-4f8e-ab75-ff58f7eb00aa">


1. Set the virtual machine size to **DS1_v2** and click **Resize**. When prompted, confirm the change.

<img width="945" alt="image" src="https://github.com/Arinze008/Azure-Onboard-Automator/assets/139396868/2ccfc8ce-52d6-419e-97bf-a449e57ff2a6">


    >**Note**: Choose another size if **Standard DS1_v2** is not available. Resizing is also known as vertical scaling, up or down.

1. In the **Settings** area, select **Disks**.

1. Under **Data disks** select **+ Create and attach a new disk**. Configure the settings (leave other settings at their default values).

    | Setting | Value |
    | --- | --- |
    | Disk name | `vm1-disk1` |
    | Storage type | **Standard HDD** |
    | Size (GiB) | `32` |

<img width="889" alt="image" src="https://github.com/Arinze008/Azure-Onboard-Automator/assets/139396868/77631eaf-42ee-4a89-ab07-640c1c1180e9">


1. Click **Apply**.

1. After the disk has been created, click **Detach** (if necessary, scroll to the right to view the detach icon), and then click **Apply**.

    >**Note**: Detaching removes the disk from the VM but keeps it in storage for later use.

1. Search for and select `Disks`. From the list of disks, select the **vm1-disk1** object.

    >**Note:** The **Overview** blade also provides performance and usage information for the disk.

1. In the **Settings** blade, select **Size + performance**.

1. Set the storage type to **Standard SSD**, and then click **Save**.
<img width="775" alt="image" src="https://github.com/Arinze008/Azure-Onboard-Automator/assets/139396868/afed3303-bad5-45e3-9863-2d6d0b994f2a">

1. Navigate back to the **az104-vm1** virtual machine and select **Disks**.

1. In the **Data disk** section, select **Attach existing disks**.

1. In the **Disk name** drop-down, select **VM1-DISK1**. 

1. Verify the disk is now **Standard SSD**.

1. Select **Apply** to save your changes. 

    >**Note:** You have now created a virtual machine, scaled the SKU and the data disk size. In the next task we use Virtual Machine Scale Sets to automate the scaling process.

## Azure Virtual Machine Scale Sets Architecture Diagram

![Diagram of the vmss architecture tasks.](../media/az104-lab08-vmss-architecture.png)

## Task 3: Create and configure Azure Virtual Machine Scale Sets

In this task, you will deploy an Azure virtual machine scale set across availability zones. VM Scale Sets reduce the administrative overhead of automation by enabling you to configure metrics or conditions that allow the scale set to horizontally scale, scale in or scale out.

1. In the Azure portal, search for and select `Virtual machine scale sets` and, on the **Virtual machine scale sets** blade, click **+ Create**.

<img width="595" alt="image" src="https://github.com/Arinze008/Azure-Onboard-Automator/assets/139396868/fde34c80-d1c1-4533-979b-c0ca74f39c3b">


1. On the **Basics** tab of the **Create a virtual machine scale set** blade, specify the following settings (leave others with their default values) and click **Next : Spot >**:


    | Setting | Value |
    | --- | --- |
    | Subscription | the name of your Azure subscription  |
    | Resource group | **az104-rg8**  |
    | Virtual machine scale set name | `vmss1` |
    | Region | **(US)East US** |
    | Availability zone | **Zones 1, 2, 3** |
    | Orchestration mode | **Uniform** |
    | Security type | **Standard** |
    | Image | **Windows Server 2019 Datacenter - x64 Gen2** |
    | Run with Azure Spot discount | **Unchecked** |
    | Size | **Standard D2s_v3** |
    | Username | `localadmin` |
    | Password | **Provide a secure password**  |
    | Already have a Windows Server license? | **Unchecked** |

<img width="630" alt="image" src="https://github.com/Arinze008/Azure-Onboard-Automator/assets/139396868/18b0040e-78b6-4cc6-9b5e-cee2322400f3">


    >**Note**: For the list of Azure regions which support deployment of Windows virtual machines to availability zones, refer to [What are Availability Zones in Azure?](https://docs.microsoft.com/en-us/azure/availability-zones/az-overview)

    ![Screenshot of the create vmss page. ](../media/az104-lab08-create-vmss.png)

1. On the **Spot** tab, accept the defaults and select **Next : Disks >**.

1. On the **Disks** tab, accept the default values and click **Next : Networking >**.

1. On the **Networking** page, click the **Create virtual network** link below the **Virtual network** textbox and create a new virtual network with the following settings (leave others with their default values).  When finished, select **OK**.

    | Setting | Value |
    | --- | --- |
    | Name | `vmss-vnet` |
    | Address range | `10.82.0.0/20` (change what is there) |
    | Subnet name | `subnet0` |
    | Subnet range | `10.82.0.0/24` |

1. In the **Networking** tab, click the **Edit network interface** icon to the right of the network interface entry.

1. For **NIC network security group** section, select **Advanced** and then click **Create new** under the **Configure network security group** drop-down list.

1. On the **Create network security group** blade, specify the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Name | **vmss1-nsg** |
<img width="497" alt="image" src="https://github.com/Arinze008/Azure-Onboard-Automator/assets/139396868/a52530c1-c014-49b0-bdce-91233f4a8d0a">

1. Click **Add an inbound rule** and add an inbound security rule with the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Source | **Any** |
    | Source port ranges | * |
    | Destination | **Any** |
    | Service | **HTTP** |
    | Action | **Allow** |
    | Priority | **1010** |
    | Name | `allow-http` |

<img width="908" alt="image" src="https://github.com/Arinze008/Azure-Onboard-Automator/assets/139396868/d0f292d9-56d7-4a28-b4aa-f09123c1fdf7">


1. Click **Add** and, back on the **Create network security group** blade, click **OK**.

<img width="566" alt="image" src="https://github.com/Arinze008/Azure-Onboard-Automator/assets/139396868/c3d4ec58-5ba2-4ceb-b18f-05855c136bf3">


1. In the **Edit network interface** blade, in the **Public IP address** section, click **Enabled** and click **OK**.
<img width="359" alt="image" src="https://github.com/Arinze008/Azure-Onboard-Automator/assets/139396868/37978b40-b7c6-4d8d-bbae-ab4f55f2f9b4">


1. In the **Networking** tab, under the **Load balancing** section, specify the following (leave others with their default values).

    | Setting | Value |
    | --- | --- |
    | Load balancing options | **Azure load balancer** |
    | Select a load balancer | **Create a load balancer** |

1. On the **Create a load balancer** page, specify the load balancer name and take the defaults. Click **Create** when you are done then **Next : Scaling >**.

    | Setting | Value |
    | --- | --- |
    | Load balancer name | `vmss-lb` |
<img width="950" alt="image" src="https://github.com/Arinze008/Azure-Onboard-Automator/assets/139396868/91452fa5-67a7-4be5-818e-b957a20c04bd">

    >**Note:** Pause for a minute and review what you done. At this point, you have configured the virtual machine scale set with disks and networking. In the network configuration you have created a network security group and allowed HTTP. You have also created a load balancer with a public IP address.

1. On the **Scaling** tab, specify the following settings (leave others with their default values) and click **Next : Management >**:

    | Setting | Value |
    | --- | --- |
    | Initial instance count | `2` |
    | Scaling policy | **Manual** |

1. On the **Management** tab, specify the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Boot diagnostics | **Disable** |

1. Click **Next : Health >**.

1. On the **Health** tab, review the default settings without making any changes and click **Next : Advanced >**.

1. On the **Advanced** tab, click **Review + create**.

1. On the **Review + create** tab, ensure that the validation passed and click **Create**.

    >**Note**: Wait for the virtual machine scale set deployment to complete. This should take approximately 5 minutes. While you wait review the [documentation](https://learn.microsoft.com/azure/virtual-machine-scale-sets/overview).

## Task 4: Scale Azure Virtual Machine Scale Sets

In this task, you scale the virtual machine scale set using a custom scale rule.

1. Select **Go to resource** or search for and select the **vmss1** scale set.

1. Choose **Scaling** from the menu on the left-hand side of the scale set window.

>**Did you know?** You can **Manual scale** or **Custom autoscale**. In scale sets with a small number of VM instances, increasing or decreasing the instance count (Manual scale) may be best. In scale sets with a large number of VM instances, scaling based on metrics (Custom autoscale) may be more appropriate.

### Scale out rule

1. Select **Custom autoscale**. Then change the **Scale mode** to **Scale based on metric**. And then select **Add a rule**.

1. Let's create a rule that automatically increases the number of VM instances. This rule scales out when the average CPU load is greater than 70% over a 10-minute period. When the rule triggers, the number of VM instances is increased by 20%.

    | Setting | Value |
    | --- | --- |
    | Metric source | **Current resource (vmss1)** |
    | Metric namespace | **Virtual Machine Host** |
    | Metric name | **Percentage CPU** (review your other choices) |
    | Operator | **Greater than** |
    | Metric threshold to trigger scale action | **70** |
    | Duration (minutes) | **10** |
    | Time grain statistic | **Average** |
    | Operation | **Increase percent by** (review other choices) |
    | Cool down (minutes) | **5** |
    | Percentage | **20** |

    ![Screenshot of the scaling add rule page.](../media/az104-lab08-scale-rule.png)

1. Be sure to **Save** your changes.

### Scale in rule

1. During evenings or weekends, demand may decrease so it is important to create a scale in rule.

1. Let's create a rule that decreases the number of VM instances in a scale set. The number of instances should decrease when the average CPU load drops below 30% over a 10-minute period. When the rule triggers, the number of VM instances is decreased by 20%.

1. Select **Add a rule**, adjust the settings, then select **Add**.

    | Setting | Value |
    | --- | --- |
    | Operator | **Less than** |
    | Threshold | **30** |
    | Operation | **decrease percentage by** (review your other choices) |
    | Percentage | **20** |

1. Be sure to **Save** your changes.

### Set the instance limits

1. When your autoscale rules are applied, instance limits make sure that you do not scale out beyond the maximum number of instances or scale in beyond the minimum number of instances.

1. **Instance limits** are shown on the **Scaling** page after the rules.

    | Setting | Value |
    | --- | --- |
    | Minimum | **2** |
    | Maximum | **10** |
    | Default | **2** |

1. Be sure to **Save** your changes

1. On the **vmss1** page, select **Instances**. This is where you would monitor the number of virtual machine instances.

    >**Note:** If you are interested in using Azure PowerShell for virtual machine creation, try Task 5. If you are interested in using the CLI to create virtual machines, try Task 6.

## Task 5: Create a virtual machine using Azure PowerShell (option 1)

1. Use the icon (top right) to launch a **Cloud Shell** session. Alternately, navigate directly to `https://shell.azure.com`.

1. Be sure to select **PowerShell**. If necessary, configure the shell storage.

1. Run the following command to create a virtual machine. When prompted, provide a username and password for the VM. While you wait check out the [New-AzVM](https://learn.microsoft.com/powershell/module/az.compute/new-azvm?view=azps-11.1.0) command reference for all the parameters associated with creating a virtual machine.

    ```powershell
    New-AzVm `
    -ResourceGroupName 'az104-rg8' `
    -Name 'myPSVM' `
    -Location 'East US' `
    -Image 'Win2019Datacenter' `
    -Zone '1' `
    -Size 'Standard_D2s_v3' ` 
    -Credential (Get-Credential)
   ```
<img width="730" alt="image" src="https://github.com/Arinze008/Azure-Onboard-Automator/assets/139396868/c9d31baa-6e75-4559-8497-b54d0307aaee">

<img width="868" alt="image" src="https://github.com/Arinze008/Azure-Onboard-Automator/assets/139396868/76ebe83c-8b31-45e2-bc77-cdadf961fac6">


1. Once the command completes, use **Get-AzVM** to list the virtual machines in your resource group.

    ```powershell
    Get-AzVM `
    -ResourceGroupName 'az104-rg8' `
    -Status
    ```
<img width="683" alt="image" src="https://github.com/Arinze008/Azure-Onboard-Automator/assets/139396868/fdc02ef3-2066-43cc-821d-b8b3cd8b682a">

1. Verify your new virtual machine is listed and the **Status** is **Running**.

1. Use **Stop-AzVM** to deallocate your virtual machine. Type **Yes** to confirm.

    ```powershell
    Stop-AzVM `
    -ResourceGroupName 'az104-rg8' `
    -Name 'myPSVM' 
    ```
<img width="589" alt="image" src="https://github.com/Arinze008/Azure-Onboard-Automator/assets/139396868/9dade7a2-4df8-404c-80ee-a6487eb95de7">
<img width="589" alt="image" src="https://github.com/Arinze008/Azure-Onboard-Automator/assets/139396868/d6306fe9-b39b-4706-b5b4-4d965af951ed">

1. Use **Get-AzVM** with the **-Status** parameter to verify the machine is **deallocated**.

<img width="812" alt="image" src="https://github.com/Arinze008/Azure-Onboard-Automator/assets/139396868/f5e69aea-fa6f-414e-9d4a-51f60968ce1b">


    >**Did you know?** When you use Azure to stop your virtual machine, the status is *deallocated*. This means that any non-static public IPs are released, and you stop paying for the VM’s compute costs.

## Task 6: Create a virtual machine using the CLI (option 2)

1. Use the icon (top right) to launch a **Cloud Shell** session. Alternately, navigate directly to `https://shell.azure.com`.

1. Be sure to select **Bash**. If necessary, configure the shell storage.

1. Run the following command to create a virtual machine. When prompted, provide a username and password for the VM. While you wait check out the [az vm create](https://learn.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-create) command reference for all the parameters associated with creating a virtual machine.

    ```sh
    az vm create --name myCLIVM --resource-group az104-rg8 --image Ubuntu2204 --admin-username localadmin --generate-ssh-keys
    ```
<img width="947" alt="image" src="https://github.com/Arinze008/Azure-Onboard-Automator/assets/139396868/b89d0193-c244-46fa-949d-a2152ef303b6">


1. Once the command completes, use **az vm show** to verify your machine was created.

    ```sh
    az vm show --name  myCLIVM --resource-group az104-rg8 --show-details
    ```
<img width="938" alt="image" src="https://github.com/Arinze008/Azure-Onboard-Automator/assets/139396868/2ac6591c-9a3d-43ce-b0c4-99146d8406ff">


1. Verify the **powerState** is **VM Running**.

<img width="951" alt="image" src="https://github.com/Arinze008/Azure-Onboard-Automator/assets/139396868/3ad3f7a4-bd00-44b1-af95-3228e4806e54">


1. Use **az vm deallocate** to deallocate your virtual machine. Type **Yes** to confirm.

    ```sh
    az vm deallocate --resource-group az104-rg8 --name myCLIVM
    ```
<img width="947" alt="image" src="https://github.com/Arinze008/Azure-Onboard-Automator/assets/139396868/d5e828aa-81b9-4aa0-94a2-0aebbe3248fe">

1. Use **az vm show** to ensure the **powerState** is **VM deallocated**.

    >**Did you know?** When you use Azure to stop your virtual machine, the status is *deallocated*. This means that any non-static public IPs are released, and you stop paying for the VM’s compute costs.

## Cleanup your resources

If you are working with **your own subscription** take a minute to delete the lab resources. This will ensure resources are freed up and cost is minimized. The easiest way to delete the lab resources is to delete the lab resource group. 

+ In the Azure portal, select the resource group, select **Delete the resource group**, **Enter resource group name**, and then click **Delete**.
+ Using Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.
+ Using the CLI, `az group delete --name resourceGroupName`.

<img width="911" alt="image" src="https://github.com/Arinze008/Azure-Onboard-Automator/assets/139396868/909e9154-3195-4112-8e41-d017425f5f6c">


## Extend your learning with Copilot
Copilot can assist you in learning how to use the Azure scripting tools. Copilot can also assist in areas not covered in the lab or where you need more information. Open an Edge browser and choose Copilot (top right) or navigate to *copilot.microsoft.com*. Take a few minutes to try these prompts.

+ Provide the steps and the Azure CLI commands to create a Linux virtual machine. 
+ Review the ways you can scale virtual machines and improve performance.
+ Describe Azure storage lifecycle management policies and how they can optimize costs.

## Key takeaways

+ Azure virtual machines are on-demand, scalable computing resources.
+ Azure virtual machines provide both vertical and horizontal scaling options.
+ Configuring Azure virtual machines includes choosing an operating system, size, storage and networking settings.
+ Azure Virtual Machine Scale Sets let you create and manage a group of load balanced VMs.
+ The virtual machines in a Virtual Machine Scale Set are created from the same image and configuration.
+ In a Virtual Machine Scale Set the number of VM instances can automatically increase or decrease in response to demand or a defined schedule.
