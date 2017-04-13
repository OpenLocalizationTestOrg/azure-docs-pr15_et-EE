## <a name="traffic-manager-profile"></a>Liikluse haldur profiil

Liikluse juhataja ja selle lapse lõpp-punkti ressurss luba DNS-i marsruutimiseks Azure ja Azure väljaspool lõpp-punktid. Sellise liikluse jaotuse reguleerib marsruutimise poliitika meetoditest. Liikluse haldur võimaldab jälgida lõpp-punkti tervis ja suunatakse õigesti liikluse põhjal seisundi lõpp. 

| Atribuut | Kirjeldus |
|---|---|
|**trafficRoutingMethod**| võimalikud väärtused on *jõudlus*ja *kaalutud* *Priority (prioriteet)* | 
| **dnsConfig** | Profiili FQDN | 
| **Protocol (protokoll)** | järelevalve protokoll, võimalikud väärtused on *HTTP* ja *HTTPS*|
| **Port** | pordi jälgimine |  
| **Tee** | tee jälgimine |
| **Lõpp-punktid** |  Container lõpp-punkti ressursid | 

### <a name="endpoint"></a>Lõpp-punkti 

Lõpp-punkti on lapse ressurss liikluse haldur profiili. See tähistab teenuse või web lõpp-punkti, mille kasutaja liikluse levitatakse vastavalt konfigureeritud poliitika liikluse haldur profiili ressurss. 

| Atribuut | Kirjeldus | 
|---|---| 
| **Tüüp** |  tüüp lõpp-punkti, võimalikud väärtused on *Azure End osutage*, *Väline lõpp-punkt*ja *Pesastatud lõpp-punkti* | 
| **targetResourceId** |  avaliku IP-aadress teenuse või web lõpp-punkti. See võib olla Azure või välise lõpp. | 
| **Weight (kaal)** | lõpp-punkti paksus korraldamise kasutatud. | 
| **Priority (prioriteet)** | lõpp-punkti abil saate määratleda Tõrkesiirde toimingu Priority (prioriteet) |

Valimi liikluse haldur Json-vormingus: 


        {
            "apiVersion": "[variables('tmApiVersion')]",
            "type": "Microsoft.Network/trafficManagerProfiles",
            "name": "VMEndpointExample",
            "location": "global",
            "dependsOn": [
                "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'), '0')]",
                "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'), '1')]",
                "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'), '2')]",
            ],
            "properties": {
                "profileStatus": "Enabled",
                "trafficRoutingMethod": "Weighted",
                "dnsConfig": {
                    "relativeName": "[parameters('dnsname')]",
                    "ttl": 30
                },
                "monitorConfig": {
                    "protocol": "http",
                    "port": 80,
                    "path": "/"
                },
                "endpoints": [
                    {
                        "name": "endpoint0",
                        "type": "Microsoft.Network/trafficManagerProfiles/azureEndpoints",
                        "properties": {
                            "targetResourceId": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('publicIPAddressName'), 0))]",
                            "endpointStatus": "Enabled",
                            "weight": 1
                        }
                    },
                    {
                        "name": "endpoint1",
                        "type": "Microsoft.Network/trafficManagerProfiles/azureEndpoints",
                        "properties": {
                            "targetResourceId": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('publicIPAddressName'), 1))]",
                            "endpointStatus": "Enabled",
                            "weight": 1
                        }
                    },
                    {
                        "name": "endpoint2",
                        "type": "Microsoft.Network/trafficManagerProfiles/azureEndpoints",
                        "properties": {
                            "targetResourceId": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('publicIPAddressName'), 2))]",
                            "endpointStatus": "Enabled",
                            "weight": 1
                        }
                    }
                ]
            }
        }

 
## <a name="additional-resources"></a>Lisaressursid

Lugege lisateavet [REST API dokumentatsiooni liikluse haldur](https://msdn.microsoft.com/library/azure/mt163664.aspx) .
