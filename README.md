# Send Guest OS metrics to the Azure Monitor metric store using a Resource Manager template and PowerShell for a Windows virtual machine

As of today, Azure Monitor doesn't allow to create new alerts based on guest OS metrics.
This sample shows how to enable Azure Monitor to be able to create new alerts (Generation 2) based on guest OS metrics.


## The Solution

This solution provides two ways to enable the sinks for custom metrics / gen2 metrics for WAD in order to collect metrics and logs from the guest operating system (Guest OS) that's running as part of a virtual machine, so users can create Gen2 alerts on those metrics using Azure Monitor.

## 1- Using ARM Templates ##

This solution has been created based on this [official doc](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/collect-custom-metrics-guestos-resource-manager-vm)

See the template azuredeploy.json.


## 2- Using PowerShell ##
````
$RG = "<resourcegroup-name>"
$VirtualMachine = "<vm-name>"
$storageAccount = "<storage-account-name>"
$key = "<storage-key>"
````
````
$vm = Get-AzVM -ResourceGroupName $RG -Name $VirtualMachine
Update-AzVM -ResourceGroupName $RG -VM $vm -AssignIdentity:$SystemAssigned
````
````
Set-AzVMDiagnosticsExtension -ResourceGroupName $RG -VMName $VirtualMachine -DiagnosticsConfigurationPath "<path-to-diagnostic.xml>" -StorageAccountName $storageAccount -StorageAccountKey $key -StorageAccountEndpoint "https://core.windows.net/"
````

## Region limitations ##
As of today, the Azure Monitor sink is currently in public preview, this will be in all regions by end of June 2019.
Here are the list of 7 regions currently supported:

1- West Central US 
2- South Central US
3- East US
4- West US 2
5- Southeast Asia
6- North Europe
7- West Europe