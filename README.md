Get-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" | Where-object {$_.Profile -match "Public"} | Enable-NetFirewallRule

Get-NetFirewallRule -DisplayName "*Echo Request*" | Where-Object {$_.Direction -eq "Inbound" -and $_.profile -match "Public"} | Select-Object DisplayName, Enabled, Profile, Direction, Action