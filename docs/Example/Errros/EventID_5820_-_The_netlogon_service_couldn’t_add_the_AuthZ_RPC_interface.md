---
icon: :octicons-alert-24:
---

## Error

**EventID 5820: The Netlogon service could not add the AuthZ RPC
interface. The service was terminated. The following error occurred:
'The parameter is incorrect.**
**Test-ComputerSecureChannel : Cannot verify the secure channel for the
local computer. Operation failed with the following exception: "The
interface is unknown".**

## Cause

*Unkown*

## Solution

  - Take the backup of your registry.
  - Delete the key **Internet** under
    **HKEY_LOCAL_MACHINE\\Software\\Microsoft\\RPC**.
  - Restart the server.

[Kategorie:Windows](Kategorie:Windows "wikilink")