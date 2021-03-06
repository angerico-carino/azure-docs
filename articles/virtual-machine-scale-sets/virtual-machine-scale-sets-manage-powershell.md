﻿---
title: Manage Virtual Machine Scale Sets with Azure PowerShell | Microsoft Docs
description: Common Azure PowerShell cmdlets to manage Virtual Machine Scale Sets, such as how to start and stop an instance, or change the scale set capacity.
services: virtual-machine-scale-sets
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
tags: azure-resource-manager

ms.assetid: d35fa77a-de96-4ccd-a332-eb181d1f4273
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/13/2017
ms.author: iainfou

---
# Manage a virtual machine scale set with Azure PowerShell
Throughout the lifecycle of a virtual machine scale set, you may need to run one or more management tasks. Additionally, you may want to create scripts that automate various lifecycle-tasks. This article details some of the common Azure PowerShell cmdlets that let you perform these tasks.

To complete these management tasks, you need the latest Azure PowerShell module. For information on how to install and use the latest version, see [Getting started with Azure PowerShell](/powershell/azure/get-started-azureps). If you need to create a virtual machine scale set, you can [create a scale set in the Azure portal](virtual-machine-scale-sets-portal-create.md).


## View information about a scale set
To view the overall information about a scale set, use [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss). The following example gets information about the scale set named *myScaleSet* in the *myResourceGroup* resource group. Enter your own names as follows:

```powershell
Get-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet"
```


## View VMs in a scale set
To view a list of VM instance in a scale set, use [Get-AzureRmVmssVM](/powershell/module/azurerm.compute/get-azurermvmssvm). The following example list all VM instances in the scale set named *myScaleSet* and in the *myResourceGroup* resource group. Provide your own values for these names:

```powershell
Get-AzureRmVmssVM -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet"
```

To view additional information about a specific VM instance, add the `-InstanceId` parameter to [Get-AzureRmVmssVM](/powershell/module/azurerm.compute/get-azurermvmssvm) and specify an instance to view. The following example views information about VM instance *0* in the scale set named *myScaleSet* and the *myResourceGroup* resource group. Enter your own names as follows:

```powershell
Get-AzureRmVmssVM -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "0"
```


## Change the capacity of a scale set
The preceding commands showed information about your scale set and the VM instances. To increase or decrease the number of instances in the scale set, you can change the capacity. The scale set automatically creates or removes the required number of VMs, then configures the VMs to receive application traffic.

First, create a scale set object with [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss), then specify a new value for `sku.capacity`. To apply the capacity change, use [Update-AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss). The following example updates *myScaleSet* in the *myResourceGroup* resource group to a capacity of *5* instances. Provide your own values as follows:

```powershell
# Get current scale set
$vmss = Get-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet"

# Set and update the capacity of your scale set
$vmss.sku.capacity = 5
Update-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -VirtualMachineScaleSet $vmss 
```

If takes a few minutes to update the capacity of your scale set. If you decrease the capacity of a scale set, the VMs with the highest instance IDs are removed first.


## Stop and start VMs in a scale set
To stop one or more VMs in a scale set, use [Stop-AzureRmVmss](/powershell/module/azurerm.compute/stop-azurermvmss). The `-InstanceId` parameter allows you to specify one or more VMs to stop. If you do not specify an instance ID, all VMs in the scale set are stopped. To stop multiple VMs, separate each instance ID with a comma.

The following example stops instance *0* in the scale set named *myScaleSet* and the *myResourceGroup* resource group. Provide your values as follows:

```powershell
Stop-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "0"
```

By default, stopped VMs are deallocated and do not incur compute charges. If you wish the VM to remain in a provisioned state when stopped, add the `-StayProvisioned` parameter to the preceding command. Stopped VMs that remain provisioned incur regular compute charges.


### Start VMs in a scale set
To start one or more VMs in a scale set, use [Start-AzureRmVmss](/powershell/module/azurerm.compute/start-azurermvmss). The `-InstanceId` parameter allows you to specify one or more VMs to start. If you do not specify an instance ID, all VMs in the scale set are started. To start multiple VMs, separate each instance ID with a comma.

The following example starts instance *0* in the scale set named *myScaleSet* and the *myResourceGroup* resource group. Provide your values as follows:

```powershell
Start-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "0"
```


## Restart VMs in a scale set
To restart one or more VMs in a scale set, use [Retart-AzureRmVmss](/powershell/module/azurerm.compute/restart-azurermvmss). The `-InstanceId` parameter allows you to specify one or more VMs to restart. If you do not specify an instance ID, all VMs in the scale set are restarted. To restart multiple VMs, separate each instance ID with a comma.

The following example restarts instance *0* in the scale set named *myScaleSet* and the *myResourceGroup* resource group. Provide your values as follows:

```powershell
Restart-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "0"
```


## Remove VMs from a scale set
To remove one or more VMs in a scale set, use [Remove-AzureRmVmss](/powershell/module/azurerm.compute/remove-azurermvmss). The `-InstanceId` parameter allows you to specify one or more VMs to remove. If you do not specify an instance ID, all VMs in the scale set are removed. To remove multiple VMs, separate each instance ID with a comma.

The following example removes instance *0* in the scale set named *myScaleSet* and the *myResourceGroup* resource group. Provide your values as follows:

```powershell
Remove-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "0"
```


## Next steps
Other common tasks for scale sets include how to [deploy an application](virtual-machine-scale-sets-deploy-app.md), and [upgrade VM instances](virtual-machine-scale-sets-upgrade-scale-set.md). You can also use Azure PowerShell to [configure auto-scale rules](virtual-machine-scale-sets-autoscale-overview.md).