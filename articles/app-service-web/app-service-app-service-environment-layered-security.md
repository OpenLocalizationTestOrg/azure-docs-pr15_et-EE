<properties 
    pageTitle="Kihiti olevad turbearhitektuur rakenduse teenuse keskkondade" 
    description="Kihiti olevad turbearhitektuur rakenduse teenuse keskkondade rakendamine." 
    services="app-service" 
    documentationCenter="" 
    authors="stefsch" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/30/2016" 
    ms.author="stefsch"/>   

# <a name="implementing-a-layered-security-architecture-with-app-service-environments"></a>Kihiti olevad turbearhitektuur rakenduse teenuse keskkondade rakendamine

## <a name="overview"></a>Ülevaade ##
 
Kuna rakendus teenuse keskkonnas virtuaalse võrku juurutatud isoleeritakse käitusaja keskkonnas, arendajad saavad luua kihiti olevad turbearhitektuur, pakkudes võrgupääs erineva füüsilise rakenduse iga taseme jaoks.

Levinud soov on Peida API tagasi lõpetatakse Interneti juurdepääsu ja lubada ainult API-de varustava veebirakenduste kutsumist.  [Võrgu turberühmad (NSGs)] [ NetworkSecurityGroups] saab kasutada alamvõrku, mis sisaldab rakenduse teenuse keskkonnas avaliku juurdepääsu piiramiseks API rakendusi.

Alloleval joonisel kuvatakse näide arhitektuur WebAPI vastavalt rakendusega juurutatud rakenduse teenuse keskkonnas.  Kolm eraldi web appi eksemplari juurutatud kolmes eraldi rakenduse teenuse keskkonnas tagaandmebaas helistada sama WebAPI rakendus.

![Kontseptuaalne arhitektuur][ConceptualArchitecture] 

Roheline plussmärke näitavad, et võrgu turberühma alamvõrgu, mis sisaldab "apiase" võimaldab sissetulevate kõnede varustava veebirakenduste, kui ka kõnede ise.  Kuid samasse võrku turberühma selgesõnaliselt keelab juurdepääsu üldine sissetulev liiklus Interneti kaudu. 

Selles teemas ülejäänud tutvustatakse juhised, kuidas konfigureerida võrgu turberühma alamvõrgu, mis sisaldab "apiase".

## <a name="determining-the-network-behavior"></a>Võrgu käitumine määratlemine ##
Et teada saada, milliseid võrgu turvalisuse reeglid on vaja, peate kindlaks, millised võrgu klientidele lubatakse rakenduse teenuse keskkond, mis sisaldavad API rakenduse saavutamiseks ja mis kliendid on blokeeritud.

Kuna [võrgu turberühmad (NSGs)] [ NetworkSecurityGroups] alamvõrku, rakendatakse ja rakenduse teenuse keskkonnas on juurutatud üheks alamvõrku, on NSG sisalduvate reeglite rakendamine rakenduse teenuse keskkonnas töötavate **Kõik** rakendused.  Valimi arhitektuur kasutamise pärast võrgu turberühma rakendatakse alamvõrku, mis sisaldab "apiase" selles artiklis kõik rakendused töötavate "apiase" rakenduse teenuse keskkond, mis on kaitstud turvalisuse reeglid sama valiku. 

- **Varustava helistajad väljaminevate IP-aadressi määramiseks:**  Mis on IP-aadress või aadressid varustava helistajad?  Need aadressid tuleb selgesõnaliselt lubatud juurdepääs on NSG.  Kuna kõnede vahel rakenduse teenuse keskkonnas peetakse "Internet" kõned, tähendab see, väljaminevate IP-aadress määratud iga kolme varustava rakenduse teenuse keskkonnas peab olema lubatud juurdepääs NSG jaoks "apiase" alamvõrgu.   Üksikasjalikumat teavet määratlemine väljaminevate IP-aadress, töötavad rakenduse teenuse keskkonnas rakenduste kohta vt [Võrgu arhitektuur] [ NetworkArchitecture] ülevaate artikli.
- **Kas pean tagaandmebaas API rakenduse endale helistada?**  Vahel tähelepanuta jääv ja väike on stsenaarium, kus tagaandmebaas rakendus peab endale helistada.  Kui tagaandmebaas API rakendus App teenuse keskkonnas peab endale helistada, seda ka käsitletakse nimega "Internet" tegemine.  Valimi arhitektuur selleks, võimaldades juurdepääsu on "apiase" rakenduse teenuse keskkond, mis on samuti väljaminevate IP-aadress.

## <a name="setting-up-the-network-security-group"></a>Kuidas häälestada võrku turberühma ##
Kui kogumi väljaminevate IP-aadressid on teada, on järgmiseks ehitada võrgu turberühma.  Võrgu turberühmad loomist nii ressursihaldur põhineva virtuaalse võrke, samuti klassikaline virtuaalse võrgu.  Järgmised näited loomine ja konfigureerimine on NSG klassikaline virtuaalse võrgus PowerShelli kaudu.

Valimi arhitektuur, asuvad keskkondade nii, et mõnda tühja NSG luuakse selle piirkonna Lõuna keskse meile:

    New-AzureNetworkSecurityGroup -Name "RestrictBackendApi" -Location "South Central US" -Label "Only allow web frontend and loopback traffic"

Esmalt otsene luba reegel on lisatud Azure halduse infrastruktuuri nagu artiklis [Sissetulev] liiklus[ InboundTraffic] rakenduse teenuse keskkonna jaoks.

    #Open ports for access by Azure management infrastructure
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW AzureMngmt" -Type Inbound -Priority 100 -Action Allow -SourceAddressPrefix 'INTERNET' -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '454-455' -Protocol TCP
    
Järgmiseks kaks reeglid lisatakse HTTP ja HTTPS kõnede esimese varustava rakenduse teenuse keskkonnast ("fe1ase").

    #Grant access to requests from the first upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe1ase" -Type Inbound -Priority 200 -Action Allow -SourceAddressPrefix '65.52.xx.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe1ase" -Type Inbound -Priority 300 -Action Allow -SourceAddressPrefix '65.52.xx.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Loputada ja korrake teise ja kolmanda varustava rakenduse teenuse keskkondade ("fe2ase" ja "fe3ase").

    #Grant access to requests from the second upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe2ase" -Type Inbound -Priority 400 -Action Allow -SourceAddressPrefix '191.238.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe2ase" -Type Inbound -Priority 500 -Action Allow -SourceAddressPrefix '191.238.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP
    
    #Grant access to requests from the third upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe3ase" -Type Inbound -Priority 600 -Action Allow -SourceAddressPrefix '23.98.abc.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe3ase" -Type Inbound -Priority 700 -Action Allow -SourceAddressPrefix '23.98.abc.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Lõpetuseks juurdepääsu tagaandmebaas API rakenduse teenuse keskkonna väljaminevate IP-aadress, et see saate ise tagasi helistada.

    #Allow apps on the apiase environment to call back into itself
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP apiase" -Type Inbound -Priority 800 -Action Allow -SourceAddressPrefix '70.37.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS apiase" -Type Inbound -Priority 900 -Action Allow -SourceAddressPrefix '70.37.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Muud võrgu turvalisus reegleid ei vaja konfigureerida, kuna iga NSG on seatud vaikimisi reeglid, mis sissetuleva juurdepääsu Interneti kaudu vaikimisi.

Reeglite Turvalisus jaotises võrku täieliku loendi on näidatud allpool.  Pange tähele, kuidas viimase reegli, mis on esile tõstetud, blokeerib sissetuleva juurdepääsu kõik helistajad, mis on antud selgesõnaliselt juurdepääs.

![NSG konfigureerimine][NSGConfiguration] 

Viimane toiming on rakendada selle NSG alamvõrku, mis sisaldab "apiase" rakenduse teenuse keskkonnas.  

     #Apply the NSG to the backend API subnet
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'yourvnetnamehere' -SubnetName 'API-ASE-Subnet'

Koos NSG, rakendatakse alamvõrgu, ainult kolme varustava rakenduse teenuse keskkondade ja rakenduse teenuse keskkond, mis sisaldavad API tagaandmebaas on lubatud "apiase" keskkonnas sisse helistada.


## <a name="additional-links-and-information"></a>Asjakohaste linkidega ja teave ##
Kõik artiklid ja kuidas-'s rakenduse teenuse keskkonnas on saadaval [seletusfail rakenduse teenuse keskkonna jaoks](../app-service/app-service-app-service-environments-readme.md).

Teavet [võrgu turberühmad](../virtual-network/virtual-networks-nsg.md). 

Mõistmine [väljaminevate IP-aadresside] [ NetworkArchitecture] ja rakenduse teenuse keskkonnas.

[Võrgu portide] [ InboundTraffic] kasutatavaid rakenduse teenuse keskkonnas.

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[NetworkArchitecture]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-architecture-overview/
[InboundTraffic]:  https://azure.microsoft.com/en-us/documentation/articles/app-service-app-service-environment-control-inbound-traffic/

<!-- IMAGES -->
[ConceptualArchitecture]: ./media/app-service-app-service-environment-layered-security/ConceptualArchitecture-1.png
[NSGConfiguration]:  ./media/app-service-app-service-environment-layered-security/NSGConfiguration-1.png
