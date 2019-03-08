# Register all Azure Resource Providers at once

**Objective**</br>
Register all Azure Resource Providers in a given Azure Subscription.

Registering a resource provider configures your subscription to work with the resource provider. The scope for registration is always the subscription. By default, many resource providers are automatically registered. However, you may need to manually register some resource providers. To register a resource provider, you must have permission to perform the /register/action operation for the resource provider. This operation is included in the Subscription Contributor and Owner roles. 

Note that You cannot unregister a resource provider when you still have resource types from that resource provider in your subscription.\. </br>

**Requirements:**
- The security principal that executes this script must be in a Azure Subscription Owner or Contributor role
- This script assumes you have the Azure PowerShell AZ cmdlets installed. More information here https://docs.microsoft.com/en-us/powershell/azure/new-azureps-module-az?view=azps-1.4.0
- The script assumes you have are authenticated

**Caveat**
Enable all Azure Resource Providers via this script is A) one-time operation. Newly created Resource Providers will NOT be automatically enabled without re-executing the script and B) enabling all Resource Providers will allow all resources to be provisioned without explcit registration of the necessary Resource Providers. Some may consider this a risk as resources may be provisioned that are considered undersirable by the business. 

**Script**
Execute Script Below.
<pre lang="...">
#set variable and Get Azure Resource Provider list of objects that are not registered
$rps = Get-AzResourceProvider -ListAvailable | Where-Object {$_.RegistrationState -eq 'NotRegistered'} | Select-Object ProviderNamespace

#Execute For Each loop to process through results from Get command and register Resource Providers
ForEach ($rp in $rps) {Register-AzResourceProvider -ProviderNamespace $rp.ProviderNamespace}
</pre>
