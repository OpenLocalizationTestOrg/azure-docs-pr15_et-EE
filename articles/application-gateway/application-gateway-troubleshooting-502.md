<properties
   pageTitle="Rakenduse lüüsi vale lüüs (502) tõrkeotsing | Microsoft Azure'i"
   description="Saate teada, kuidas rakenduse lüüsi 502 tõrkeotsing"
   services="application-gateway"
   documentationCenter="na"
   authors="amitsriva"
   manager="rossort"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/02/2016"
   ms.author="amitsriva" />

# <a name="troubleshooting-bad-gateway-errors-in-application-gateway"></a>Rakenduse lüüsi vale lüüs tõrgete tõrkeotsing

## <a name="overview"></a>Ülevaade

Pärast konfigureerimist Azure'i rakenduste portaali, üks tõrkeid, mis võivad ilmneda kasutajad on "serveritõrge: 502 – veebiserver sai Sobimatu vastuse toimides lüüsi või puhverserveri server". See võib juhtuda peamised järgmistel põhjustel:

- Azure'i rakenduse lüüsi tagaandmebaas rakenduskausta pole konfigureeritud või tühi.
- Terve pole ühtegi VMs või aknad VM skaala määramine.
- Tagaandmebaas VMs või eksemplari VM skaala määrata vaikimisi seisundi juures ei vasta.
- Sobimatud või valest konfiguratsioonist kohandatud seisund sondid.
- Koosolekukutse kasutaja taotlusi ajalõpu või ühenduvusprobleemide.

## <a name="empty-backendaddresspool"></a>Tühi BackendAddressPool

### <a name="cause"></a>Põhjus

Kui rakendus lüüsi VMs või VM skaala seadmine konfigureeritud tagaandmebaas aadress kogumi, ei saa suunata iga kliendi taotluse ja põhjustab vale lüüs tõrge.

### <a name="solution"></a>Lahendus

Veenduge, et rakenduskausta tagaandmebaas aadress on pole tühi. Seda saab teha, kas PowerShelli, CLI või portaali kaudu.

    Get-AzureRmApplicationGateway -Name "SampleGateway" -ResourceGroupName "ExampleResourceGroup"

Eelmise cmdlet väljund peaks sisaldama tühi tagaandmebaas aadress pool. Järgmine on näide selle kohta, kuhu tagastatakse kahe kaustu, mis on konfigureeritud FQDN või IP-aadressidele taustväärtus VMs jaoks. Funktsiooni BackendAddressPool ettevalmistamise olek peab olema "õnnestus".
    
    BackendAddressPoolsText: 
            [{
                "BackendAddresses": [{
                    "ipAddress": "10.0.0.10",
                    "ipAddress": "10.0.0.11"
                }],
                "BackendIpConfigurations": [],
                "ProvisioningState": "Succeeded",
                "Name": "Pool01",
                "Etag": "W/\"00000000-0000-0000-0000-000000000000\"",
                "Id": "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name>/backendAddressPools/pool01"
            }, {
                "BackendAddresses": [{
                    "Fqdn": "xyx.cloudapp.net",
                    "Fqdn": "abc.cloudapp.net"
                }],
                "BackendIpConfigurations": [],
                "ProvisioningState": "Succeeded",
                "Name": "Pool02",
                "Etag": "W/\"00000000-0000-0000-0000-000000000000\"",
                "Id": "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name>/backendAddressPools/pool02"
            }]


## <a name="unhealthy-instances-in-backendaddresspool"></a>Vigane aknad BackendAddressPool

### <a name="cause"></a>Põhjus

Kui kõik eksemplarid BackendAddressPool on vigane, siis rakenduse lüüsi ei oleks mis tahes tagaandmebaas marsruutimiseks kasutaja taotlusele. See võib ka juhul, kui tagaandmebaas eksemplarid on terve, kuid ei ole vajalik rakendus juurutatud.

### <a name="solution"></a>Lahendus

Veenduge, et linnanimede on terve ja rakendus on õigesti konfigureeritud. Märkige ruut, kui tagaandmebaas eksemplarid on võimalik vastata ping sama VNet teise VM. Kui konfigureeritud avaliku lõpp-punkt, veenduge, et veebirakenduse brauseri taotluse on kasutuskõlblikud.

## <a name="problems-with-default-health-probe"></a>Vaikimisi seisundi juures probleemid

### <a name="cause"></a>Põhjus

502 tõrgete võib olla sagedamini näidikud, et vaikimisi seisundi juures ei jõudnud tagaandmebaas VMs. Kui rakendus lüüsi eksemplari on ette valmistatud, konfigureerib automaatselt vaikimisi seisundi juures, et iga BackendAddressPool abil soovitud BackendHttpSetting atribuudid. Pole kasutaja sisendist on vaja määrata selle juures. Täpsemalt koormus tasakaalustamiseks reegli konfigureerimisel seos on tehtud erinevad BackendHttpSetting ja BackendAddressPool. Vaikimisi juures on konfigureeritud lubama iga Liit ja rakenduse lüüsi käivitab perioodiliste seisund sisse ühenduse iga astme BackendAddressPool BackendHttpSetting elementi määratud port juures. Järgmises tabelis on loetletud väärtused, mis on seotud vaikimisi seisundi juures.


|Atribuudi juures | Väärtus | Kirjeldus|
|---|---|---|
| Juures URL-i| http://127.0.0.1/ | URL-tee |
| Intervall | 30 | Juures intervall sekundites |
| Ajalõpp  | 30 | Juures ajalõpp sekundites |
| Vigane lävi | 3 | Sondi uuesti loendus. Tagaandmebaas server on märgitud pärast järjestikust juures tõrge count jõuab vigane lävi alla. |

### <a name="solution"></a>Lahendus

- Veenduge, et saidi vaikimisi on konfigureeritud ja on listening 127.0.0.1 juures.
- Kui BackendHttpSetting porti kui 80, vaikesaidi peaks olema konfigureeritud kuulata pordinumber.
- Kõne suunamiseks http://127.0.0.1:port peaks tagastama HTTP tulemus näeb välja umbes 200. See tuleks tagastada 30 sec ajavahemiku jooksul.
- Veenduge, et konfigureeritud port oleks avatud ja et puuduvad tulemüüri reeglid või Azure võrgu turberühmad, mis on konfigureeritud port sissetuleva või väljamineva liikluse jaoks blokeerida.
- Kui FQDN või avaliku IP kasutatakse Azure klassikaline VMs või pilveteenuses, veenduge, et vastava [lõpp-punkti](../virtual-machines/virtual-machines-windows-classic-setup-endpoints.md) on avatud.
- Kui VM on konfigureeritud Azure ressursihaldur kaudu ja [Võrgu turberühma](../virtual-network/virtual-networks-nsg.md) peab olema konfigureeritud lubama juurdepääsu soovitud port VNet, kus on juurutatud rakenduse lüüsi, jääb.


## <a name="problems-with-custom-health-probe"></a>Kohandatud seisundi juures probleemid

### <a name="cause"></a>Põhjus

Kohandatud seisund sondid paindlikult täiendavate katsetamine käitumine vaikimisi. Kohandatud sondid kasutamisel kasutajad saavad konfigureerida juures intervall, URL-i, ja tee testimiseks ja mitu nurjunud vastuste kinnitamiseks enne märkimise tagaandmebaas rakenduskausta eksemplari nimega vigane. Lisatakse järgmised täiendavad atribuudid.

|Atribuudi juures| Kirjeldus|
|---|---|
| Nimi | Nimi on juures. Selle nime on kasutatud viidata juures tagaandmebaas HTTP sätted. |
| Protocol (protokoll) | Protokoll, mida kasutatakse saata selle juures. Funktsiooni juures kasutatakse määratletud tagaandmebaas HTTP sätted protokoll |
| Host |  Hosti nimi saata selle juures. Rakendatav vaid siis, kui rakenduse lüüsi mitme saidi on konfigureeritud. See erineb VM hosti nimi.  |
| Tee | Suhteline tee on juures. Sobiv tee algab /". Funktsiooni juures saadetakse \<protokolli\>://\<hosti\>:\<port\>\<tee\> |
| Intervall | Sondi intervall sekundites. See on kaks järjestikust sondid intervall.|
| Ajalõpp | Sondi ajalõpp sekundites. Funktsiooni juures on märgitud nurjus kui kehtiv vastuse ei selle ajavahemiku jooksul. |
| Vigane lävi | Sondi uuesti loendus. Tagaandmebaas server on märgitud pärast järjestikust juures tõrge count jõuab vigane lävi alla. |


### <a name="solution"></a>Lahendus

Kinnitage, et kohandatud seisund Probe on eelneva tabelina õigesti konfigureeritud. Eelmise tõrkeotsingu juhiseid, lisaks ka järgmist.

- Veenduge, et selle juures on õigesti määratud ühe [juhend](application-gateway-create-probe-ps.md).
- Kui saidil on konfigureeritud rakenduse lüüsi, vaikimisi selle hosti nimi peaks olema määratud nimega "127.0.0.1", kui muul viisil konfigureerida kohandatud juures.
- Veenduge, et kõne http://\<hosti\>:\<port\>\<tee\> tagastab HTTP tulemus näeb välja umbes 200.
- Veenduge, et intervall, käivitub ja UnhealtyThreshold oleks piiridesse.

## <a name="request-time-out"></a>Taotluse ajalõpp

### <a name="cause"></a>Põhjus

Kasutaja taotluse saabumisel rakenduse lüüsi konfigureeritud reegleid kehtib taotlus ja marsruudib see tagaandmebaas rakenduskausta eksemplari. See ootab on konfigureeritav ajavahemiku tagaandmebaas eksemplari vastust. Vaikimisi on see intervall **30 sekundit**. Kui rakendus lüüsi ei saa vastuse tagaandmebaas rakendusest sellesse vahemikku, näeksid kasutaja taotluse 502 tõrge.

### <a name="solution"></a>Lahendus

Rakenduse lüüsi võimaldab kasutajatel konfigureerida selle sätte kaudu BackendHttpSetting, mida saate siis rakendada eri kaustu. Erinevate tagaandmebaas kaustu võib olla erinevad BackendHttpSetting ja erinev taotluse ajalõpuni konfigureeritud.

    New-AzureRmApplicationGatewayBackendHttpSettings -Name 'Setting01' -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 60

## <a name="next-steps"></a>Järgmised sammud

Kui eelmist toimingut ei lahendanud probleemi, avage [Piletite toetavad](https://azure.microsoft.com/support/options/).
