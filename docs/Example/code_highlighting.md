## Backupjobs

### Quick Backup starten

#### VM auf Hyper-V Host

``` powershell
Find-VBRHvEntity -Name <VMName> | Start-VBRQuickBackup
```

#### VM auf ESXi Host

``` powershell
Find-VBRViEntity -Name <VMName> | Start-VBRQuickBackup
```

### Disable, Export and Re-Enable all Jobs

#### Disable & Export

``` powershell
Add-PSSnapin veeam*
$AllVBRJobs = Get-VBRJob | select Name, JobType, IsScheduleEnabled
$AllVBRJobs | Export-Csv -Path "C:\temp\All-VBRJobs.csv"
Disable-VBRJob -Job $AllVBRJobs.Name
```

#### Enable all before-enabled-Jobs

``` powershell
$BeforeEnabledVBRJobs = Get-Content "C:\temp\All-VBRJobs.csv" | ConvertFrom-Csv | ? {$_.IsScheduleEnabled -match "True"}
Enable-VBRJob -Job $BeforeEnabledVBRJobs.Name
```

## VTL-Tapes

### Informationen eines VTL-Tapes anzeigen (Beispiel Tape "N00069")

``` powershell
Get-VBRTapeMedium -Name "N00069"
```

### VTL-Tape Name ändern

*Beispiel wenn der "Barcode" eines Tapes nicht mit dem "Namen" übereinstimmt.*

``` powershell
Get-VBRTapeMedium -Id 039ff1d3-0544-4d9e-a2d2-f1f5fc7e2466  | Set-VBRTapeMedium -Name "N00068"
```

## Konfiguration

### Backup Repository (Limit Maximum concurrent tasks to (Bsp. 6))

``` powershell
Get-VBRBackupRepository -Name <Backupjob> | Set-VBRBackupRepository -MaxConcurrentJobs 6
```

[Kategorie:Veeam](Kategorie:Veeam "wikilink") [Kategorie:PowerShell](Kategorie:PowerShell "wikilink")