# Dedup

### Aktive Deduping-Jobs anzeigen

*Wenn mit diesen Befehl nichts angezeigt wird, dann läuft kein
Dedup-Job.*

`Get-DedupJob`

### Momentaner Dedup-Status suchen

`Get-DedupStatus`

### GarbageCollection ausführen

*Mit diesem Befehl werden Blöcke entfernt, auf die nicht verwiesen wird,
sowie Container komprimiert, die mehr als 5 % an Daten enthalten, auf
die nicht verwiesen wird. Bei Angabe des –full-Parameters komprimiert
der Auftrag alle Container mit der höchstmöglichen.*

`Start-DedupJob GarbageCollection `<Laufwerk Buchstabe>` `

### Dedup Optimierung

`Start-DedupJob Optimization `<Laufwerk Buchstabe>` `

[Kategorie:PowerShell](Kategorie:PowerShell "wikilink")