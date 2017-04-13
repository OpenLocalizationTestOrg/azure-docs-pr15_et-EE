<properties 
    pageTitle="Kuidas luua rakenduse teenuse keskkonnas" 
    description="Rakenduse teenuse keskkonnas loomine meilivoo kirjeldus" 
    services="app-service" 
    documentationCenter="" 
    authors="ccompy" 
    manager="stefsch" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/22/2016" 
    ms.author="ccompy"/>

# <a name="how-to-create-an-app-service-environment"></a>Kuidas luua rakenduse teenuse keskkonnas #

### <a name="overview"></a>Ülevaade ###

Rakenduse teenuse keskkonnas (ASE) on Premium teenuse valik Azure rakenduse teenus, mis pakub täiustatud konfiguratsiooni võimalus, mis pole saadaval templite mitme rentnikuga.  Funktsiooni ASE põhiosas kasutab Azure'i rakendust Service kliendi virtuaalse võrku.  Rakenduse teenuse keskkonnas lugeda, [mis on rakenduse teenuse keskkonnas] pakutud võimaluste suurem mõistmiseks[ WhatisASE] dokumentatsiooni.

### <a name="before-you-create-your-ase"></a>Enne kui saate luua oma ASE ###

See on oluline, tuleb arvestada asjad, mida te ei saa muuta.  Neid ei saa muuta oma ASE kohta pärast selle loomist tegureid:

- Asukoht
- Tellimuse
- Ressursirühm
- Kasutatud VNet
- Alamvõrgu kasutatud 
- Alamvõrgu suurus

Kui valides mõne VNet ja täpsustades on alamvõrku, veenduge, et see on piisavalt suur, et kõik kasvu tulevikus.  

### <a name="creating-an-app-service-environment"></a>Rakenduse teenuse keskkonna loomine ###

On kaks võimalust ASE loomine UI juurdepääsu.  Saab ***rakenduse teenuse*** keskkonnas Azure'i turuplatsil otsingu kaudu leitud või -läbi uus > Web + mobiil -> rakenduse teenuse keskkonnas.  Mõne ASE loomiseks tehke järgmist.

1. Sisestage oma ASE nimi.  Nimi, mis on määratud ASE kasutatakse ASE loodud rakendused.  Kui ASE nimi on appsvcenvdemo oleks alamdomeeni nimi. *appsvcenvdemo.p.azurewebsites.net*.  Kui olete loonud Seega rakendus nimega *mytestapp* , siis oleks adresseeritavad *mytestapp.appsvcenvdemo.p.azurewebsites.net*juures.  Te ei saa kasutada tühja oma ASE nimi.  Kui kasutate nimi läbivate suurtähtedega märke, on domeeninimi kokku väike versioon selle nime.  Kui kasutate mõnda ILB siis oma ASE nimi oma alamdomeen ei kasutata, kuid selle asemel selgesõnaliselt ASE loomise ajal

    ![][1]

2. Valige oma tellimus.  Kasutada oma ASE tellimus on üks, mis luuakse rakenduses selle ASE kõik rakendused, mille.  Ei saa oma ASE paigutamiseks VNet, mis on mõne muu tellimus

3. Valige või määrake uue ressursirühma.  Kasutada oma ASE ressursirühma peab olema sama, mida kasutatakse teie VNet.  Kui valite olemasoleva VNet uuendatakse ressursi rühma valiku oma ASE kajastamiseks, mis teie VNet.

    ![][2]

4. Tehke soovitud valikud virtuaalse võrgu ja asukoht.  Saate luua uue VNet või valige olemasoleva VNet.  Kui valite uue VNet saate määrata nimi ja asukoht Uue VNet on aadress vahemiku 192.268.250.0/23 ja alamvõrku, mis on **vaikimisi** , mis on määratletud 192.168.250.0/24 nimega.  Võite valida ka lihtsalt olemasoleva klassikaline või ressursside halduri VNet.  Määrab VIP tüüp valik, kui teie ASE pääseb otse Interneti-ühendus (väline) või kasutab mõnda sisemise laadi koormusetasakaalustusteenuse (ILB).  Lisateavet lähemalt neid lugeda, [kasutades rakenduse teenuse keskkond on sisemine laadi koormusetasakaalustusteenuse][ILBASE].  Kui valite väline VIP tüüp saate valida mitu välise IP-aadresside süsteem on loodud IPSSL eesmärgil.  Kui valite Internal peate määrata oma ASE kasutavate alamdomeen.  Praeguseks saab kasutada virtuaalse võrkudes, mis kasutavad *kas* avaliku aadresside vahemikud, *või* RFC1918 aadress tühikute (st sisse Privaatne aadressid).  Avaliku aadressi vahemikus virtuaalse võrgu kasutamiseks peate loomiseks VNet ette valmistada.  Kui valite olemasoleva VNet peate looge uus alamvõrgu ASE loomise ajal.  **Te ei saa kasutada portaali varem loodud alamvõrgu.  Kui loote oma ASE ressursi halduri malli abil saate luua olemasoleva alamvõrgu on ASE.**  Luua ka ASE malli kasutamine teabe siin [loomine malli põhjal teenuse keskkonnas rakenduse] [ ILBAseTemplate] ja siin, [ILB rakenduse teenuse keskkonna malli loomine][ASEfromTemplate].

### <a name="details"></a>Üksikasjad ###

Mõne ASE luuakse 2 lõpeb esi- ja 2 töötajate.  Eesmise lõpeb toimivad HTTP-või HTTPS lõpp-punktid ja saata liiklust töötajate, mis on teie minirakendusi rollid.   Saate reguleerida kogus pärast ASE loomist ja saate isegi häälestamine autoscale reeglid järgmiste ressursside kaustu.  Lisateavet ümber käsitsi skaleerimist haldamise ja järelevalve keskkonnas rakenduse teenuse vaadake teemat: [Kuidas konfigureerida rakenduse teenuse keskkonnas][ASEConfig] 

Ainult ühe ASE saate kasutada ASE alamvõrgu olemas.  Alamvõrgu ei saa kasutada midagi muud kui ASE

### <a name="after-app-service-environment-creation"></a>Pärast rakenduse teenuse keskkond loomine ###

Pärast ASE loomist saate kohandada.

- Kogus ees lõpeb (minimaalse: 2)
- Töötajate arvu (minimaalse: 2)
- Arvu IP-aadresside IP SSL-i jaoks saadaval
- Ressursi suurused ees lõpeb või töötajate kasutab Arvutage (miinimummaht ees on P2)


On rohkem üksikasju ümber käsitsi skaala haldus ja järelevalve rakenduse teenuse siin: [Kuidas konfigureerida rakenduse teenuse keskkonnas][ASEConfig] 

Lisateavet autoscaling on siin juhend: [Kuidas konfigureerida rakenduse teenuse keskkonnale autoscale][ASEAutoscale]

On täiendavaid sõltuvused, mis pole saadaval, nt andmebaas ja salvestusruumi kohandamine.  Need on käsitleja oli Azure ja süsteemiga.  Süsteemi salvestusruumi toetab kuni 500 GB kogu rakenduse teenuse keskkonnas ja andmebaas on reguleeritud Azure'i vajadusel süsteemi ulatust.


## <a name="getting-started"></a>Alustamine
Kõik artiklid ja kuidas-'s rakenduse teenuse keskkonnas on saadaval [seletusfail rakenduse teenuse keskkonna jaoks](../app-service/app-service-app-service-environments-readme.md).

Alustamine rakenduse teenuse keskkonnas, lugege teemat [Sissejuhatus rakenduse teenuse keskkonnas][WhatisASE]

Azure'i rakendust Service platform kohta leiate lisateavet teemast [Azure rakenduse teenuse][AzureAppService].

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]
 

<!--Image references-->
[1]: ./media/app-service-web-how-to-create-an-app-service-environment/asecreate-basecreateblade.png
[2]: ./media/app-service-web-how-to-create-an-app-service-environment/asecreate-vnetcreation.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[ASEConfig]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/ 
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[ILBASE]: http://azure.microsoft.com/documentation/articles/app-service-environment-with-internal-load-balancer/
[ILBAseTemplate]: http://azure.microsoft.com/documentation/templates/201-web-app-ase-create/
[ASEfromTemplate]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-create-ilb-ase-resourcemanager/
