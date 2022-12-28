## Error

`Invoke-RestMethod : Der Remoteserver hat einen Fehler zurückgegeben: (400) Ungültige Anforderung.`
`In Zeile:88 Zeichen:9`
`+         Invoke-RestMethod -UseBasicParsing -Uri "`<https://jira.inventx>` ...`
`+         13:33, 9. Jan. 2019 (CET)13:33, 9. Jan. 2019 (CET)13:33, 9. Jan. 2019 (CET)13:33, 9. Jan. 2019 (CET)13:33, 9. Jan. 2019 (CET)13:33, 9. Jan. 2019 (CET)13:33, 9. Jan. 2019 (CET)13:33, 9. Jan. 2019 (CET)13:33, 9. Jan. 2019 (CET)13:33, 9. Jan. 2019 (CET)13:33, 9. Jan. 2019 (CET)13:33, 9. Jan. 2019 (CET)~`
`    + CategoryInfo          : InvalidOperation: (System.Net.HttpWebRequest:HttpWebRequest) [Invoke-RestMethod], WebException`
`    + FullyQualifiedErrorId : WebCmdletWebResponseException,Microsoft.PowerShell.Commands.InvokeRestMethodCommand`

## Cause

*If you use umlauts in a Invoke-RestMethod with JSON then the error can
occurs*
For example:

``` PowerShell
''Invoke-RestMethod -UseBasicParsing -Uri "https://YourURL" -Headers $header -Body $JSON -ContentType application/json -Method Post''
```

## Solution

  - Add following encoding to your JSON-Object -\>
    **(\[System.Text.Encoding\]::UTF8.GetBytes($JSON))**

For example:

``` PowerShell
''Invoke-RestMethod -UseBasicParsing -Uri "https://YourURL" -Headers $header -Body ([System.Text.Encoding]::UTF8.GetBytes($JSON)) -ContentType application/json -Method Post''
```

*More informationen:
<https://stackoverflow.com/questions/15290185/invoke-webrequest-issue-with-special-characters-in-json>*

[Kategorie:PowerShell](Kategorie:PowerShell "wikilink")