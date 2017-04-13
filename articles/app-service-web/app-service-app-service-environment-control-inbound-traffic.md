<properties 
    pageTitle="Kuidas määrata rakenduse teenuse keskkonnas sissetulev liiklus" 
    description="Lugege selle kohta, kuidas konfigureerida võrgu turvalisus eeskirjad sissetulev liiklus rakenduse teenuse keskkonnas." 
    services="app-service" 
    documentationCenter="" 
    authors="ccompy" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/02/2016" 
    ms.author="stefsch"/>   

# <a name="how-to-control-inbound-traffic-to-an-app-service-environment"></a>Kuidas määrata rakenduse teenuse keskkonnas sissetulev liiklus

## <a name="overview"></a>Ülevaade ##
Teenuse keskkonnas rakenduse saab luua sisse **kas** mõni Azure ressursihaldur virtuaalse võrgu, **või** klassikaline mudel [virtuaalse võrgu][virtualnetwork].  Uue virtuaalse võrgu ja uus alamvõrgu saate määratleda keskkonnas rakenduse teenus on loodud ajal.  Teise võimalusena teenuse keskkonnas rakenduse saab luua olemasoleva virtuaalse võrgu ja olemasoleva alamvõrgu.  Tehtud muudatuse tehtud juuni 2016, kus saate nüüd praeguseks juurutatud üheks virtuaalse võrke, mida kasutada avaliku aadresside vahemikud või RFC1918 aadress tühikute (st Privaatne aadressid).  Lisateavet rakenduse teenuse keskkonnas loomise kohta leiate [loomine keskkonnas rakenduse teenuse kohta][HowToCreateAnAppServiceEnvironment].

Rakenduse teenuse keskkonnas alati peab sees on alamvõrgu luua, kuna on alamvõrgu pakub võrgu äärist, mida saate kasutada lukustada nii, et HTTP ja HTTPS-liikluse on lubatud ainult teatud varustava IP-aadresside sissetulev liiklus taha varustava seadmed ja teenused.

Sissetulevate ja väljaminevate on alamvõrgu võrguliiklust on kontrollitud, kasutades [võrgu turberühma][NetworkSecurityGroups]. Sissetulev liiklus juhtimine nõuab võrgu turvalisus jaotises võrku turvalisus reeglite loomine ja seejärel määramine võrgu turvalisus rühmitamine alamvõrku, mis sisaldavad rakenduse teenuse keskkonnas.

Pärast võrgu turberühma määratakse soovitud alamvõrku, sissetulev liiklus rakendused rakendus teenuse keskkonnas on lubatud ja blokeeritud domeenide luba põhjal ja keelamiseks reeglid, mis on määratletud jaotises võrku turvalisus.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="network-ports-used-in-an-app-service-environment"></a>Võrgu pordid kasutada rakendust Service keskkonnas ##
Enne sissetuleva võrguliiklust võrgu turberühma lukustada, on oluline teada rakenduse teenuse keskkonnas kasutatavate kohustuslike ning valikuliste võrgu portide määramine.  Kogemata välja mõned pordid-liikluse sulgemine võib põhjustada keskkonnas rakenduse teenuse funktsionaalsuse kadu.

Järgmises rakenduse teenuse keskkonnas kasutatavate portide loendit. Kõik pordid on **TCP**, selgelt märgitud teisiti.

- 454: **nõutav pordi** kasutatavaid Azure taristu haldamise ja säilitamise rakenduse teenuse keskkonnas SSL-i kaudu.  Blokeeri selle pordi-liikluse.  Selle pordi on alati seotud mõne ASE, avaliku VIP.
- 455: **nõutav pordi** kasutatavaid Azure taristu haldamise ja säilitamise rakenduse teenuse keskkonnas SSL-i kaudu.  Blokeeri selle pordi-liikluse.  Selle pordi on alati seotud avaliku VIP, mis ASE.
- 80: vaikimisi port HTTP sissetulev liiklus rakendused, mis töötavad rakenduse teenuse lepingud keskkonnas rakenduse teenuse jaoks.  Klõpsake ILB lubatud ASE seotud selle pordi ASE ILB aadress.
- 443: vaikimisi port jaoks SSL-i sissetulev liiklus rakendused töötavad rakenduse teenuse lepingud rakenduse teenuse keskkonnas.  Klõpsake ILB lubatud ASE selle pordi on seotud ILB ASE-aadressi.
- 21: FTP juhtelemendi kanali kaudu.  Selle pordi blokeeritavate turvaline, kui FTP ei kasutata.  Klõpsake ILB lubatud ASE, saate selle pordi seotud ILB aadressil on ASE.
- 990: juhtelemendi kanali FTPS.  Selle pordi blokeeritavate turvaline kui FTPS ei kasutata.  Klõpsake ILB lubatud ASE, saate selle pordi seotud ILB aadressil on ASE.
- 10001-10020: andmekanalite FTP.  Juhtelemendi kanali kohta, sarnaselt järgmised pordid võib olla turvaline blokeeritud FTP ei kasutata.  Klõpsake ILB lubatud ASE, saate selle pordi seotud soovitud ASE ILB aadress.
- 4016: kasutatakse Kaug silumine koos Visual Studio 2012.  Selle pordi blokeeritavate turvaline, kui funktsiooni ei kasutata.  Klõpsake ILB lubatud ASE selle pordi on seotud ILB ASE-aadressi.
- 4018: kasutatakse Kaug silumine Visual Studio 2013 abil.  Selle pordi blokeeritavate turvaline, kui funktsiooni ei kasutata.  Klõpsake ILB lubatud ASE selle pordi on seotud ILB ASE-aadressi.
- 4020: kasutatakse koos Visual Studio 2015 Kaug silumine.  Selle pordi blokeeritavate turvaline, kui funktsiooni ei kasutata.  Klõpsake ILB lubatud ASE seotud selle pordi ASE ILB aadress.

## <a name="outbound-connectivity-and-dns-requirements"></a>Väljamineva meili Ühenduvus ja DNS-i nõuded ##
Rakenduse teenuse keskkonnas õigesti, see nõuab väljaminev juurdepääsu erinevad lõpp-punktid. [Võrgu konfigureerimise jaoks ExpressRoute](app-service-app-service-environment-network-configuration-expressroute.md#required-network-connectivity) artikli jaotises "Nõutav võrguühendus" on väliste lõpp-punktid kasutavad ka ASE täieliku loendi.

Rakenduse teenuse keskkonnas nõua kehtiv DNS-i taristu virtuaalse võrgu jaoks konfigureeritud.  Kui mingil põhjusel DNS-i konfigureerimine muudetakse pärast keskkonnas rakenduse teenus on loodud, saate arendajate jõustada keskkonnas rakenduse teenuse hooleks uue DNS-i konfigureerimine.  Rakenduse teenuse keskkonna haldamise tera [Azure portaali] ülaosas käivitamise jooksva keskkonna taaskäivitage arvuti "Uuesti" ikooniga[ NewPortal] põhjustab keskkonna hooleks uue DNS-i konfigureerimine.

Soovitatav, et kõik kohandatud DNS-i serverite on vnet olema häälestamise ette valmistada enne loomise rakendus teenuse keskkonnas.  Virtuaalse võrgu DNS-i konfiguratsiooni muutmise ajal rakenduse teenuse keskkonnas luuakse põhjustab rakenduse teenuse keskkonna loomise protsess ei ole.  Samamoodi, kui kohandatud DNS-i server olemas teises otsas VPN-lüüsi ja DNS-i server on kättesaamatu või pole saadaval, rakenduse teenuse keskkonna loomisprotsessi ka nurjub.

## <a name="creating-a-network-security-group"></a>Võrgu Turberühma loomine ##
Kuidas võrgu turvalisuse pakuvad töö üksikasjalik kohta vt järgmist [teavet][NetworkSecurityGroups].  Üksikasjade all puutetundlikes tõstab esile turberühmade võrgu konfigureerimist ja võrgu turberühma rakendamine alamvõrku, mis sisaldab teenuse keskkonnas rakenduse keskendudes.

**Märkus:** Võrgu turberühmad saab konfigureerida graafiliselt [Azure portaali](https://portal.azure.com) kaudu või Azure PowerShelli kaudu.

Võrgu turberühmad luuakse esmalt tellimusega seotud autonoomse üksusena. Kuna võrgu turberühmad luuakse Azure piirkonnas, veenduge, et sama piirkonna rakenduse teenuse keskkond on loodud võrgu turberühma.

Järgmine näitab võrgu Turberühma loomine:

    New-AzureNetworkSecurityGroup -Name "testNSGexample" -Location "South Central US" -Label "Example network security group for an app service environment"

Kui võrku turberühma on loodud, lisatakse selle ühe või mitme võrgu turvalisuse reeglid.  Kuna reeglite võivad aja jooksul muutuda, on soovitatav ruumi läbi suhtes nummerdusskeemis reegli prioriteedi saab hõlpsalt lisada täiendavad reeglid aja jooksul.

Järgmises näites on kuvatud reegli, mis annab konkreetselt nõutud Azure'i infrastruktuuri haldamiseks ja kasutab keskkonda rakenduse teenuse haldus pordid.  Pange tähele, et kõik halduse liikluse liigub sujuvalt üle SSL-i ja on tagatud kliendi sertide, nii, et isegi juhul, kui pordid lahti nad ei pääse mõni üksus Azure infrastruktuuri peale.


    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "ALLOW AzureMngmt" -Type Inbound -Priority 100 -Action Allow -SourceAddressPrefix 'INTERNET'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '454-455' -Protocol TCP
    

Kui lukustada juurdepääsu pordi 80 ja 443 "peitmiseks" keskkonnas rakenduse teenuse taha varustava seadmete või teenuseid, peate teadma varustava IP-aadress.  Näiteks kui kasutate tulemüüri rakenduse web (WAF), WAF on oma IP-aadress (või aadressid), mis kasutab Millal puhverdamist liikluse järgneval rakenduse teenuse keskkonnas.  Peate kasutamine *SourceAddressPrefix* parameetri võrgu turvalisus reegli IP-aadress.

Alltoodud näites sissetulev liiklus teatud varustava IP-aadress on selgesõnaliselt lubatud.  Aadressi *1.2.3.4* kasutatakse kohatäite mõnda varustava WAF IP-aadressi.  Väärtust kasutatavaid varustava seadet või teenuse aadressi sobitamiseks muuta.

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT HTTP" -Type Inbound -Priority 200 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT HTTPS" -Type Inbound -Priority 300 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP
    
FTP tugi soovi korral saab järgmised reeglid mallina juurdepääsu FTP juhtelemendi portide ja andmete kanali pordid.  Kuna FTP on stateful protokoll, ei pruugi te FTP-liikluse marsruutimiseks traditsiooniline HTTP-või HTTPS tulemüüri või puhverserveri seadme kaudu.  Sel juhul peate määratud *SourceAddressPrefix* muu väärtuse – näiteks IP address lahtrivahemik arendaja või juurutamise masinad mis FTP kliendid töötavad. 

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT FTPCtrl" -Type Inbound -Priority 400 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '21' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT FTPDataRange" -Type Inbound -Priority 500 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '10001-10020' -Protocol TCP

(**Märkus:** kanali pordi andmevahemiku võib muutuda eelvaate.)

Visual Studio Kaug silumine kasutamise korral järgmised reeglid näitavad, kuidas anda juurdepääsu.  Kuna iga versioon kasutab erinevat porti Kaug silumine on eraldi reegel iga toetatud versiooni Visual Studio.  Sarnaselt FTP-juurdepääsu korral Kaug silumine liikluse võib-olla ei flow õigesti traditsiooniline WAF või puhverserveri seadme kaudu.  *SourceAddressPrefix* saate määrata selle asemel IP address vahemikule arendaja seadmetes, kus töötab Visual Studio.

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2012" -Type Inbound -Priority 600 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4016' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2013" -Type Inbound -Priority 700 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4018' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2015" -Type Inbound -Priority 800 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4020' -Protocol TCP

## <a name="assigning-a-network-security-group-to-a-subnet"></a>Võrgu turberühma lisamine alamvõrgu määramine ##
Turberühma võrk on vaikimisi turvalisuse reegel, mis keelab juurdepääsu kõigi väliste liikluse.  Eespool kirjeldatud võrgu turvalisuse reeglid ja blokeerivad sissetulev liiklus, vaikimisi turvalisuse reegel kaudu tulem on selle ainult liikluse allikas aadressilt vahemike seostatud toimingu *Luba* on võimalik saada liikluse rakendust, millel on rakenduse teenuse keskkonnas.

Pärast võrgu turberühm lisatakse turvalisuse reeglid, tuleb määratakse alamvõrku, mis sisaldavad rakenduse teenuse keskkonnas.  Ülesande käsk viiteid nii virtuaalse võrgu, kus asub rakenduse teenuse keskkonna nimi kui ka alamvõrku, kus rakenduse teenuse keskkond, mis on loodud nime.  

Järgmises näites on kuvatud määratud alamvõrku ja virtuaalse võrgu network turberühma:


    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-test'

Pärast võrgu turvalisus rühmakuuluvus õnnestus (ülesanne on soovitud toiminguid ja võib kuluda paar minutit lõpuleviimiseks), ainult sissetulev liiklus sobitamine *Luba* reeglid jõuab edukalt rakendused rakendus teenuse keskkonnas.

Järgmises näites on kujutatud jaoks täiendavalt eemaldamine ja seega DIS seostamiseks võrgu turberühma alamvõrgu kaudu:


    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Remove-AzureNetworkSecurityGroupFromSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-test'

## <a name="special-considerations-for-explicit-ip-ssl"></a>Konkreetsete IP-SSL-silmas pidada ##
Kui rakendus on konfigureeritud konkreetsete IP-SSL aadress (rakendatav praeguseks, mis on üksnes avaliku VIP), vaikimisi IP-aadressi rakenduse teenuse keskkonna asemel HTTP ja HTTPS liikluse puhul sisse alamvõrgu üle pordid peale pordid 80 ja 443 teistsugused.

Üksikute paari pordid, mida iga IP-SSL-aadressi leiate rakenduse keskkonna üksikasjad Kasutuskogemuse blade videoportaali kasutajaliidest.  Valige "kõik sätted"--> "IP-aadressid".  "IP-aadressid" tera kuvatakse rakenduse teenuse keskkonnas koos teisiti pordi sidumiseks seostatud iga IP-SSL-aadressi HTTP ja HTTPS liikluse marsruutimiseks kasutatav tabeli kõik konkreetselt konfigureeritud IP-SSL-aadressid.  See on selle pordi paari, mis tuleb kasutada DestinationPortRange parameetrite konfigureerimisel reeglite võrgu turberühmas.

IP-SSL-i rakenduse kohta on ASE konfigureerimisel väliskliente ei kuvata ja ei pea muretsema teisiti port sidumiseks kaardistamine.  Liikluse rakendused on tavaliselt flow konfigureeritud IP-SSL-aadressi.  Tõlke sidumiseks teisiti pordi automaatselt juhtub ettevõttesiseselt ajal viimasel etapil marsruutimise liikluse alamvõrku, mis sisaldab ASE sisse. 

## <a name="getting-started"></a>Alustamine

Alustamine rakenduse teenuse keskkonnas, lugege teemat [rakenduse teenuse keskkonna tutvustus][IntroToAppServiceEnvironment]

Kõik artiklid ja kuidas-'s rakenduse teenuse keskkonnas on saadaval [seletusfail rakenduse teenuse keskkonna jaoks](../app-service/app-service-app-service-environments-readme.md).

Rakenduste turvaliselt ühenduse kirjutamata ressursi keskkonnas rakenduse teenuse ümber leiate teemast [turvaliselt ühenduse kirjutamata ressursse rakenduse teenuse keskkonnas][SecurelyConnecttoBackend]

Azure'i rakendust Service platform kohta leiate lisateavet teemast [Azure rakenduse teenuse][AzureAppService].

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[HowToCreateAnAppServiceEnvironment]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[IntroToAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[SecurelyConnecttoBackend]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-securely-connecting-to-backend-resources/
[NewPortal]:  https://portal.azure.com  

<!-- IMAGES -->
 
