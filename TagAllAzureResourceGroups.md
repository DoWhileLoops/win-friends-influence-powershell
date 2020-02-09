### Get a list of all installed modules
```powershell
Get-InstalledModule
```

### Get the PowerShell version you are running
```powershell
$PSVersionTable.PSVersion
```

### Install Az Module for CurrentUser if not present.
#### Docs Here: https://docs.microsoft.com/en-us/powershell/azure/new-azureps-module-az?view=azps-3.4.0
```powershell
Install-Module -Name Az -AllowClobber -Scope CurrentUser.
```

### Sign in to Azure
```powershell
Connect-AzAccount
```

### Get a list of all subscriptions available
```powershell
Get-InstalledModule
```

### Connect to a different subscription
```powershell
Set-AzContext -SubscriptionId "your subscription guid here"
```

### Get all resource groups in current subscription
```powershell
Get-AzResourceGroup
```

### Clear the console if desired
```powershell
Clear-Host
# To view the past commands, hit up and down at the command prompt.
# Press Enter to run these again if desired.
```

### Read all these resource groups into a variable, then print to confirm
```powershell
$allRgs = Get-AzResourceGroup;
Write-Output $allRgs;
```

### Just to confirm what we're seeing before we make and apply the tags, 
### loop through all the resource groups, parse the client name, and write the output.
#### PS uses the -gt tag instead of the > symbol (< and > are used for input/output).
```powershell
Foreach($rg in $allRgs)
{
  $rgName = $rg.ResourceGroupName
  Write-Output $rgName;

  $splitName = $rgName.Split('.')
  
  If ($splitName.Count -gt 2) {
   Write-Output $splitName[1]
  }

  Clear-Variable -Name rgName
  Clear-Variable -Name splitName
  
  Write-Output '****'
}
```

### Declare your tag name, then repeat the same loop, but make a tag and assign it to the resource group.
#### Additional logic could be put in to screen out certain resource groups.

```powershell
$tagName = 'Client'

Foreach($rg in $allRgs)
{
  $rgName = $rg.ResourceGroupName


  $splitName = $rgName.Split('.')

  If ($splitName.Count -gt 1) {
   
   Set-AzResourceGroup -Name $rgName -Tag @{$tagName=$splitName[1]}

  }

  Clear-Variable -Name rgName
  Clear-Variable -Name splitName
  
}
```