## These scripts get all resource groups in a subscription, compute the monthly spend, and export it to a .csv file. If this is your first time working with Azure Powershell, visit the first section of the tutorial [here.](https://github.com/DoWhileLoops/win-friends-influence-powershell/blob/master/TagAllAzureResourceGroups.md)

### This assumes you are connected and pointed to the right directory and subscription. 
### Read all these resource groups into a variable, then print to confirm:
```powershell
$allRgs = Get-AzResourceGroup;
Write-Output $allRgs;
```

### Loop through all resource groups in the subscription, get the relevant info, and put it into an array. Use the [Get-AzConsumptionUsageDetail](https://docs.microsoft.com/en-us/powershell/module/az.billing/get-azconsumptionusagedetail?view=azps-3.5.0) cmdlet to get all the individual costs on the account. This will churn for a while depending on your number of resource groups and volume, so you may have to break this up into chunks. *A good way of doing this may be to export fixed numbers of resource groups into spreadsheets, then import them one at a time into the $allRgs object.*
```powershell
$subscriptionRgs = @()

foreach ($rg in $allRgs) 
{
	
    $myRG = [PSCustomObject]@{
        ResourceGroupName     = $rg.ResourceGroupName
        Location = $rg.Location
        Link    =  ("https://portal.azure.com/#resource" + $rg.ResourceId)
    }

	$rgInfo = Get-AzConsumptionUsageDetail -StartDate 2020-01-01 -EndDate 2020-01-31 -ResourceGroup $rg.ResourceGroupName
	
    $rgTotalCost = 0
	
	foreach($rgRow in $rgInfo){
	    $rgTotalCost += $rgRow.PretaxCost
	}

    $roundedCost = [math]::Round($rgTotalCost,2)
	
	$myRg | Add-Member -MemberType NoteProperty -Name "Monthly Cost" -Value $roundedCost

    $subscriptionRgs += $myRg
}
```

### Then use Export-Csv and provide a path for the output:
```powershell
$subscriptionRgs | Export-Csv -Path C:\Development\RgCost.csv
```

### Bask in the glory.
