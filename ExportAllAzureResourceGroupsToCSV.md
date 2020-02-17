## These scripts export a list of Resource Groups to a .csv file. You can do this through the [Portal directly](https://www.youtube.com/watch?v=Kfrx0TpCgPc), or through PowerShell if you need more fine tuning. Exporting is definitely not limited to just resource groups, though. If this is your first time working with Azure Powershell, visit the first section of the tutorial [here.](https://github.com/DoWhileLoops/win-friends-influence-powershell/blob/master/TagAllAzureResourceGroups.md)

### This assumes you are connected and pointed to the right directory and subscription. 
### Read all these resource groups into a variable, then print to confirm:
```powershell
$allRgs = Get-AzResourceGroup;
Write-Output $allRgs;
```

### Then use Export-Csv and provide a path for the output:
```powershell
$allRgs | Export-Csv -Path .\C:\Development\AllRgs.csv
```

### Or for the brevity conscious:
```powershell
Get-AzResourceGroup | Export-Csv -Path C:\Development\AllRgs.csv
```



### Indeed, it is that simple. You can customize further with the official docs [here](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/export-csv?view=powershell-7).
