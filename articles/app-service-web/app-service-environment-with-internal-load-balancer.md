<properties
    pageTitle="Loomine ja kasutamine on sisemine laadi koormusetasakaalustusteenuse rakenduse teenuse keskkond | Microsoft Azure'i"
    description="Loomise ja kasutamise on ASE on ILB"
    services="app-service"
    documentationCenter=""
    authors="ccompy"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="ccompy"/>

# <a name="using-an-internal-load-balancer-with-an-app-service-environment"></a>Rakenduse teenuse keskkonnas on sisemine laadi koormusetasakaalustusteenuse kasutamine #

Funktsiooni rakenduse teenuse Environments(ASE) on Premium teenuse valik Azure rakenduse teenus, mis pakub täiustatud konfiguratsiooni võimalus, mis pole saadaval mitme rentniku templite.  Funktsiooni ASE põhiosas kasutab Azure rakenduse teenuse oma Azure virtuaalse Network(VNet).  Suurem mõistmiseks rakenduse teenuse keskkonnas vt [mis on rakenduse teenuse keskkonnas] pakutavaid võimalusi[ WhatisASE] dokumentatsiooni.  Kui te ei tea, töötama mõne VNet eeliste vt [Azure virtuaalse võrgu KKK][virtualnetwork].  


## <a name="overview"></a>Ülevaade ##


Mõne ASE saab kasutada Interneti puuetega inimestele juurdepääsetavate lõpp või oma VNet IP-aadressi.  Selleks, et määrata oma ASE koos mõne sisemise laadi Balancer(ILB) juurutamiseks VNet aadress IP-aadress.  Kui teie ASE on konfigureeritud ka ILB esitate:

- oma domeeni või alamdomeeni.  Teha lihtne, eeldab see dokument alamdomeen, kuid saate konfigureerida mõlemal viisil.  
- HTTPS-i jaoks kasutatavat serti
- DNS-i haldamise oma alamdomeen.  


Tagasi, saate teha järgmisi toiminguid:

- Host sisevõrgu rakendustes, nagu näiteks ärirakenduste turvaliselt pilves, millele pääsete juurde saidi saidile või ExpressRoute VPN-i kaudu
- pilveteenuses minirakendusi, mis on loetletud avaliku DNS-serverid
- luua eraldi Interneti kirjutamata rakendused, mis esiosa rakendusi saate turvaliselt integreerimine


#### <a name="disabled-functionality"></a>Keelatud funktsioonid ####

On mõned asjad, mida ei saa teha ka ILB ASE kasutamisel.  Nende järgmist.

- IPSSL abil
- teatud rakenduste IP-aadresside määramine
- osta ja serdiga rakendusega portaali kaudu.  Muidugi veel saate hankida otse sertimisorgan serdid ja seda kasutada koos oma rakendused, mitte Azure portaali kaudu.


## <a name="creating-an-ilb-ase"></a>Mõne ILB ASE loomine ##

Mõne ILB ASE loomine pole palju erineb loomine mõne ASE tavalisel viisil.  Süvitsi arutelu on ASE loomise kohta Loe [Kuidas luua rakenduse teenuse keskkonnas][HowtoCreateASE].  Protsess on ILB ASE loomiseks on sama loomise on VNet ASE loomisel või olemasoleva VNet valimise vahel.  Mõne ILB ASE loomiseks tehke järgmist. 

1.  Azure'i portaalis valige **uus -> Web + mobiil -> rakenduse teenuse keskkonnas**
2.  Valige oma tellimuse
3.  Valige või ressursside rühma loomine
4.  Valige või looge on VNet
5.  Luua mõne alamvõrku, valides mõne VNet
6.  Valige **virtuaalse võrgukoha-> VNet konfiguratsioon** ja Internal VIP tüübi määramine
7.  Sisestage alamdomeeni nimi (selle saab kasutada rakendusi, mis on loodud selle ASE alamdomeen)
8.  Klõpsake nuppu Ok ja seejärel looge


![][1]


Virtuaalse võrgu tera sees on VNet konfigureerimist.  See võimaldab teil valida välise VIP- või VIP sisemise vahel.  Vaikimisi on väline.  Kui teil on väline see siis oma ASE kasutatakse mis Internetis puuetega inimestele juurdepääsetavate VIP.  Kui valite Internal, konfigureeritakse oma ASE on ILB sees oma VNet IP-aadressi.  


Pärast valiku Internal, võimalus lisada mitu IP-aadressid oma ASE eemaldatakse ja selle asemel tuleb sisestada ASE alamdomeen.  Mõne ASE koos mõne välise VIP ASE nimi on kasutusel alamdomeen selle ASE loodud rakendused.  
Kui teie ASE kutsuti ***contosotest*** ja rakenduse, et ASE kutsuti ***mytest*** seejärel alamdomeen oleks vorming ***contosotest.p.azurewebsites.net*** ja URL-i rakenduse oleks ***mytest.contosotest.p.azurewebsites.net***.  
Kui seate Internal VIP tüüp, teie nimi ASE ei kasutata alamdomeen ASE jaoks.  Saate määrata alamdomeen otseselt.  Kui teie alamdomeen ***contoso.corp.net*** ja tehtud rakendus, et ASE nimega ***timereporting*** siis URL-i rakenduse oleks ***timereporting.contoso.corp.net***.


## <a name="apps-in-an-ilb-ase"></a>Rakendused on ILB ASE ##

Loomise rakendus on ILB ASE on sama, mis loomise rakendus on ASE tavalisel viisil.  

1. Azure'i portaalis valige **uus -> Web + mobiil -> Web** või **mobiiltelefon** või **API rakenduse**
2. Sisestage rakenduse nimi
2. Valige tellimus
3. Valige või looge ressursirühm
4. Valige või looge rakenduse teenuse Plan(ASP).  Kui luua uus ASP siis oma ASE asukohaks ja valige töötaja kausta, mida soovite oma ASP luua.  Kui loote ASP valige asukoht ja töötaja pool oma ASE.  Kui määrate rakenduse nime näete, et oma rakenduse nime all alamdomeen asendatakse alamdomeen oma ASE.   
5. Valige Loo.  **Armatuurlaud PIN-kood** tuleks märkige kui soovite rakenduse kuvamiseks klõpsake armatuurlauale.  

![][2]


Rakenduse nime all alamdomeeni nimi saab värskendada oma ASE alamdomeen kajastamiseks.  


## <a name="post-ilb-ase-creation-validation"></a>Postituse ILB ASE loomise kontrollimine ##

Mõne ILB ASE on veidi teistsugused kui mitte - ILB ASE.  Nagu juba märgitud, peate oma DNS-i haldamise ja tuleb sisestada oma serdi HTTPS-ühenduste jaoks.  


Pärast loomist oma ASE te teate, et alamdomeen näitab alamdomeen määratud ja menüüs **säte** nimega **ILB sert**on uue üksuse.  Mis hõlbustab HTTPS testimiseks iseallkirjastatud serdi luuakse ASE.  Portaali ütleb teile, et peate sisestama oma serdi https, kuid see on teil on serdi, mis sobib teie alamdomeen juhtida.  

![][3]


Kui proovite lihtsalt asju ja ei tea, kuidas luua sert, saate ise allkirjastatud serdi loomine IIS-i MMC konsooli rakendus.  Pärast selle loomist saate eksportida pfx-fail ja laadige ILB serdi UI. Kui kasutate iseallkirjastatud serdiga turvatud saidi, brauseri teile, et teil on juurdepääs saidile ei ole turvaline tõttu ei suuda serdi valideerimiseks hoiatus.  Kui soovite vältida hoiatus peate õigesti allkirjastatud sert, mis vastab teie alamdomeen ja usalda, mis tuvastab brauseri ahelas.

![][6]

Kui soovite proovida oma sertifikaatide voogu ja testige oma ASE HTTP ja HTTPS juurdepääsu.

1.  Minge ASE UI ASE loomise järel **ASE -> Sätted -> ILB serdid**
2.  Määramine, valides pfx serdifail ILB serdi ja sisestage parool.  See toiming võtab protsess pisut aega ja kuvatakse teade, et skaleerimise toiming on pooleli.
3.  Saada ILB aadressi oma ASE (**ASE -> Atribuudid -> virtuaalse IP-aadress**)
4.  Pärast loomist ASE web appi loomine 
5.  Luua VM, kui teil pole üks selle VNET (mitte sisse sama alamvõrku ASE või asju leheküljepiiri nimega)
6.  Saate seada oma alamdomeen DNS-i.  Saate kasutada metamärke koos oma alamdomeen DNS-i või kui soovite teha mõned lihtsad katsete muuta hosts faili oma VM web appi nimi VIP IP-aadressi määramiseks.  Kui teie ASE oleks alamdomeeni nimi. ilbase.com ja tehtud web app mytestapp nii, et oleks suunatud mytestapp.ilbase.com siis määrata, et hosts-faili.  (Windows hosts-faili on C:\Windows\System32\drivers\etc\)
7.  Kasutage brauserit, et VM ja minge http://mytestapp.ilbase.com (või mis tahes oma web app nimi on teie alamdomeen)
8.  Kasutage brauserit, et VM ja minge https://mytestapp.ilbase.com peate nõustuma turvalisus puudumine kui iseallkirjastatud serdiga.  


Teie ILB IP-aadress on loetletud teie atribuudid virtuaalse IP-aadress

![][4]


## <a name="using-an-ilb-ase"></a>Mõne ILB ASE abil ##

#### <a name="network-security-groups"></a>Võrgu turberühmad ####

Mõne ILB ASE võimaldab võrgu eraldamise rakenduste rakendused ei ole puuetega inimestele juurdepääsetavate või isegi tuntud Interneti-ühendus.  See on suurepärane sisevõrgu saitide nagu ärirakenduste majutamiseks.  Kui soovite piirata juurdepääsu veelgi täpsemaks saate siiski kasutada võrgu turvalisus Groups(NSGs) tasemel võrgu juurdepääsu kontrollimiseks. 


Kui soovite piirata juurdepääsu NSGs abil peate veenduge, et katkestate suhtlus, mida ASE vajab tegutsemiseks.  Isegi juhul, kui HTTP-või HTTPS juurdepääs on ainult ILB, mis kasutavad ASE ASE endiselt sõltub väljaspool olevat VNet ressursi.  Millist võrgupääs on vaja otsida dokumendis [Juhtimine sissetulev liiklus keskkonnas rakenduse teenuse] kohta teabe kuvamiseks[ ControlInbound] [Võrgu konfiguratsiooni üksikasjad rakenduse teenuse keskkonnad ExpressRoute]dokumenti ja[ExpressRoute].  


Saate konfigureerida oma NSGs peaksite teadma, et hallata oma ASE Azure'i kasutatava IP-aadressi.  IP-aadress on teie ASE väljaminevate IP-aadressi, kui see muudab Interneti-päringud.  See IP-aadressi otsimine aadress minge **Sätted -> Atribuudid** ja **Väljaminevate IP-aadress**.  

![][5]


#### <a name="general-ilb-ase-management"></a>Üldine ILB ASE haldus ####

Mõne ILB ASE haldamine on sama, mis on ASE tavaliselt haldamine.  Peate oma töötaja rühmituse majutada rohkem ASP eksemplarid ja skaala kuni oma esiosa serveritega käsitlema hulga HTTP-või HTTPS-liikluse skaalal.  Üldist teavet haldamise konfiguratsiooni, mis ASE lugeda dokumendi [rakenduse teenuse keskkonna konfigureerimise]kohta[ASEConfig].  


Täiendavad halduse üksused on serdi haldus ja DNS-i haldus.  Peate hankida ja laadige üles sert, mida kasutatakse HTTPS pärast ILB ASE loomist ja selle asendada enne selle lõppemist.  Kuna Azure'i omanik base domeeni pakume praeguseks koos mõne välise VIP serdid.  Kuna kasutatavad mõne ILB ASE alamdomeen võib olla midagi, peate oma serdi ette HTTPS. 


#### <a name="dns-configuration"></a>DNS-i konfigureerimine ####

Kui kasutate mõnda välise VIP Azure'i haldab DNS-i.  Azure'i DNS-i, mis on avalik DNS-i lisatakse automaatselt teie ASE loodud rakendust.  Mõne ILB ASE tuleb hallata oma DNS-i.  Näiteks contoso.corp.net antud alamdomeeni jaoks peate looma DNS-i A kirjed, mis viitavad oma ILB aadress:

    * 
    *.SCM ftp avaldamine 


## <a name="getting-started"></a>Alustamine
Kõik artiklid ja kuidas-'s rakenduse teenuse keskkonnas on saadaval [seletusfail rakenduse teenuse keskkonna jaoks](../app-service/app-service-app-service-environments-readme.md).

Alustamine rakenduse teenuse keskkonnas, lugege teemat [Sissejuhatus rakenduse teenuse keskkonnas][WhatisASE]

Azure'i rakendust Service platform kohta leiate lisateavet teemast [Azure rakenduse teenuse][AzureAppService].

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]
 

<!--Image references-->
[1]: ./media/app-service-environment-with-internal-load-balancer/ilbase-createilbase.png
[2]: ./media/app-service-environment-with-internal-load-balancer/ilbase-createapp.png
[3]: ./media/app-service-environment-with-internal-load-balancer/ilbase-newase.png
[4]: ./media/app-service-environment-with-internal-load-balancer/ilbase-vip.png
[5]: ./media/app-service-environment-with-internal-load-balancer/ilbase-externalvip.png
[6]: ./media/app-service-environment-with-internal-load-balancer/ilbase-ilbcertificate.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[ControlInbound]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[ExpressRoute]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/
[vnetnsgs]: http://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[ASEConfig]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
