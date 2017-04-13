
Seadme VPN konfigureerimiseks peate avaliku IP-aadressi virtuaalse võrgu lüüsi konfigureerida kohapealse VPN seadme. Töötamine oma seadme tootjalt teatud konfiguratsiooniteavet ja seadme konfigureerimine. Vaadake lisateavet töötada ka Azure VPN seadmete [VPN seadmete](../articles/vpn-gateway/vpn-gateway-about-vpn-devices.md) .

PowerShelli kaudu virtuaalse võrgu lüüsi avaliku IP-aadressi otsimiseks saate kasutada järgmises näites:

    Get-AzureRmPublicIpAddress -Name GW1PublicIP -ResourceGroupName TestRG

Saate ka vaadata avaliku IP-aadressi jaoks virtuaalse võrgu lüüsi Azure portaali kaudu. Liikuge **Virtual võrgu lüüsid**ja seejärel klõpsake lüüsi nime.