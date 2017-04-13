## <a name="virtual-network"></a>Virtuaalse võrgu
Virtuaalne võrkude (VNET) ja alamvõrku ressursid määratlemine turvalisus piiri töötab Azure'i töökoormus. Mõne VNet iseloomustab aadress tühikud, määratletud CIDR plokid kogum. 

>[AZURE.NOTE] Võrgu administraatoreid tuttavaid CIDR märke. Kui olete tuttav CIDR, [Lugege lisateavet selle kohta](http://whatismyipaddress.com/cidr).

![VNet mitu alamvõrku abil](./media/resource-groups-networking/Figure4.png)

VNets sisaldavad järgmised atribuudid.

|Atribuut|Kirjeldus|Näidisväärtuste|
|---|---|---|
|**addressSpace**|Saidikogumi aadressi eesliidete tähised, mis moodustavad VNet CIDR märke.|192.168.0.0/16|
|**alamvõrku**|Saidikogumi alamvõrku, mis moodustavad selle VNet.|lugege teemat [alamvõrku](#Subnets) .|
|**Sordib**|IP-aadress määratud objekti. See on kirjutuskaitstud atribuudi.|104.42.233.77|

### <a name="subnets"></a>Alamvõrku
Mõne alamvõrgu on lapse ressurss on VNet ja aitab määratleda segmente aadress ruumide CIDR plokk, kasutades IP address eesliiteid sees. NICs saate lisatud alamvõrku ja ühendatud VMs, pakkudes ühenduvuse eri töökoormus.

Alamvõrku sisaldavad järgmised atribuudid. 

|Atribuut|Kirjeldus|Näidisväärtuste|
|---|---|---|
|**addressPrefix**|Ühe aadress eesliide, mis moodustavad alamvõrgu CIDR märke|192.168.1.0/24|
|**networkSecurityGroup**|Rakendatud alamvõrgu NSG|vt [NSGs](#Network-Security-Group)|
|**routeTable**|Rakendatud alamvõrgu marsruutimiseks tabel|vt [UDR](#Route-table)|
|**ipConfigurations**|Saidikogumi IP alamvõrgu ühendatud NICs configruation objektid|vt [UDR](#Route-table)|


Valimi VNet JSON-vormingus:

    {
        "name": "TestVNet",
        "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet",
        "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
        "type": "Microsoft.Network/virtualNetworks",
        "location": "westus",
        "tags": {
            "displayName": "VNet"
        },
        "properties": {
            "provisioningState": "Succeeded",
            "resourceGuid": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            "addressSpace": {
                "addressPrefixes": [
                    "192.168.0.0/16"
                ]
            },
            "subnets": [
                {
                    "name": "FrontEnd",
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                    "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                    "properties": {
                        "provisioningState": "Succeeded",
                        "addressPrefix": "192.168.1.0/24",
                        "networkSecurityGroup": {
                            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd"
                        },
                        "routeTable": {
                            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/routeTables/UDR-FrontEnd"
                        },
                        "ipConfigurations": [
                            {
                                "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB1/ipConfigurations/ipconfig1"
                            },
                            ...]
                    }
                },
                ...]
        }
    }

### <a name="additional-resources"></a>Lisaressursid

- [VNet](../articles/virtual-network/virtual-networks-overview.md)kohta lisateabe saamiseks.
- Lugege VNets [REST API dokumentides](https://msdn.microsoft.com/library/azure/mt163650.aspx) .
- Lugege [REST API dokumentides](https://msdn.microsoft.com/library/azure/mt163618.aspx) alamvõrku.