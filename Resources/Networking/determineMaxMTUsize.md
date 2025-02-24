# Determine Max MTU size

Identify the network interface index number:

```powershell
Get-NetIPInterface
```

Change the MTU size of the relevant interface.

```powershell
Set-NetIPInterface -InterfaceIndex 8 -NlMtuBytes 1468
```

You can use the ping command to determine the maximum MTU size:

```powershell
ping -f 192.168.1.1 -l 1500
```