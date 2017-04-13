<properties
    pageTitle="Azure'i rakendust Service, Virtuaalmasinates, teenuse struktuuri ja pilveteenustega võrdlus | Microsoft Azure'i"
    description="Saate teada, kuidas valida Azure'i rakendust Service, Virtuaalmasinates, teenuse struktuuri ja pilveteenustega hosting veebirakenduste."
    services="app-service\web, virtual-machines, cloud-services"
    documentationCenter=""
    authors="tdykstra"
    manager="wpickett"
    editor="jimbe"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/07/2016"
    ms.author="tdykstra"/>

# <a name="azure-app-service-virtual-machines-service-fabric-and-cloud-services-comparison"></a>Azure'i rakendust Service, Virtuaalmasinates, teenuse struktuuri ja pilveteenustega võrdlus

## <a name="overview"></a>Ülevaade

Azure'i pakub mitmeid võimalusi host veebisaitidelt: [Azure'i rakendust Service][], [Virtuaalmasinates][], [Teenuse struktuuri][]ja [Pilveteenustega][]. See artikkel aitab teil mõista suvandid ja veenduge, et teie veebirakenduse jaoks õige valik.

Azure'i rakenduse teenus on parim valik enamik web apps. Juurutus- ja integreeritakse platvormi, saitide saate kiiresti mastaapimiseks veebiaadresside raskusi ja sisseehitatud tasakaalustamiseks ja liikluse haldur pakkuda kõrge kättesaadavus. Saate teisaldada Azure'i rakenduse teenusesse olemasolevatel saitidel hõlpsasti, kus on [online Migreerimistööriista](https://www.migratetoazure.net/)galeriist Web rakendus on avatud lähtekoodi rakenduse kasutamine või framework ja teie valitud tööriistade abil uue saidi loomine. Funktsiooni [WebJobs][] lihtne lisada tausta töö rakenduse teenuse veebirakendusse töötlemine.

Teenuse struktuuri on hea valik, kui olete uue rakenduse loomiseks või uuesti kirjalikult olemasoleva rakenduse kasutamiseks microservice arhitektuur. Rakendusi, mis töötab masinad ühiskasutusega kogumi, saate alustada väike ja kasvada suuri skaala sadu või tuhandeliste masinad vastavalt vajadusele. Stateful services oleks hõlpsam süsteemne ja usaldusväärselt rakenduse laoseisu ja teenuse struktuuri haldab automaatselt teenuse eraldamine, suurust ja kättesaadavus eest.  Teenuse struktuuri toetab samuti .net-i (OWIN) ja ASP.net-i Core WebAPI avatud Web kasutajaliides.  Võrreldes rakenduse teenus, teenuse struktuuri pakub rohkem kontrolli või otsest juurdepääsu, aluseks infrastruktuuri. Saate oma servereid remote või konfigureerida serveri käivitus tööülesanded. Pilveteenustega on sarnane teenuse struktuuri kontrolli kasutuslihtsus, ja klõpsake, kuid see on nüüd pärand teenus ja teenuse struktuuri on soovitatav uus areng.

Kui teil on olemasolev rakendus, mis nõuavad olulisi muudatusi läbiviimisel teenuse rakenduse või teenuse struktuuri, võib valida Virtuaalmasinates lihtsustamiseks migreerimine pilve. Siiski õigesti konfigureerida, turvamine ja säilitamine VMs nõuab palju aega ja teadmiste võrreldes Azure'i rakendust Service ja teenuse struktuuri. Kui kaalute Azure'i Virtuaalmasinates, veenduge, et te arvestama poolelioleva hooldustööd konsolideerimise paik, värskendamine ja haldamine VM keskkonna. Azure'i Virtuaalmasinates on taristu-kui-a-Service (IaaS) rakenduse teenuse ja teenuse struktuuri on platvormi-kui-a-Service (Paas). 

## <a name="features"></a>Funktsioonide võrdlus

Järgmises tabelis võrreldakse rakenduse teenus, pilveteenustega, Virtuaalmasinates ja teenuse struktuuri võimaluste aitavad teha parim valik. Praeguse SLA iga suvandi kohta leiate teemast [Azure taseme lepingute](/support/legal/sla/).

Funktsioon|Rakendust Service (veebirakenduste)|Pilveteenustega (web rollid)|Virtuaalmasinates|Teenuse struktuuri|Märkmete
---|---|---|---|---|---
Lähedal kiirsõnumivestluses juurutamine|X|||X|Rakenduse või rakenduse värskenduse juurutamine pilveteenusesse, või luua VM, suunab minuti vähemalt; rakenduse web appi juurutamine võtab sekundit.
Skaala kuni suuremat masinad ilma ümberkorraldamine|X|||X|
Sisu ja konfiguratsiooni, mis tähendab, et teil pole vaja ümberkorraldamine või konfigureerida, kui muudate ühiskasutus web serveri eksemplari.|X|||X|
Mitme juurutamise keskkonnas (tootmine ja lavastus)|X|X||X|Teenuse struktuuri võimaldab teil on mitu keskkonnas rakenduste või juurutada erinevaid versioone oma rakenduse-kõrvuti.
Automaatse OS haldus|X|X|||Automaatvärskenduste OS kavandatakse jaoks tulevaste teenuse struktuuri versioon.
Üleminek sujuvalt platvormi (32-bitine ja 64-bitise vahel hõlpsalt liikumine)|X|X|||
Koodi GIT FTP juurutamine|X||X||
Juurutamine koodi koos Web juurutamine|X||X||Pilveteenustega toetab Web juurutada värskenduste juurutamiseks üksikute rolli aknad. Aga seda ei saa kasutada algset juurutamiseks rolli ja kui kasutate Web juurutada värskendust teil juurutada eraldi iga eksemplari rollid. Pilveteenuse teenuse SLA tootmise keskkondades saamiseks on nõutavad mitmes eksemplaris.
WebMatrix tugi|X||X||
Juurdepääs teenustele nagu teenuse siini, salvestusruumi, SQL-andmebaas|X|X|X|X|
Host veebis või web services astme mitmekihilise arhitektuuri|X|X|X|X|
Host keskmisel astme mitmekihilise arhitektuuri|X|X|X|X|Rakenduse teenuse veebirakenduste saate hõlpsalt majutada REST API keskmisel taseme ja funktsiooni [WebJobs](http://go.microsoft.com/fwlink/?linkid=390226) majutada töid tausta. Käivitage WebJobs sihtotstarbeline veebisaidi sõltumatu skaleeritavus jaoks soovitud taseme saavutamiseks. [API rakenduste](../app-service-api/app-service-api-apps-why-best-platform.md) Eelvaatefunktsioon pakub rohkem funktsioone ülejäänud majutusteenuste.
Integreeritud MySQL-kui-a-service tugi|X|X|X||Pilveteenustega saate integreerida MySQL-kui-a-service kaudu ClearDB's pakkumisi, kuid mitte Azure portaali töövoo osana.
ASP, Node.js, PHP, Python ASP.net-i, klassikaline kasutajatugi|X|X|X|X|Teenuse struktuuri toetab ees web [ASP.net-i 5](../service-fabric/service-fabric-add-a-web-frontend.md) või saate abil saate juurutada mis tahes tüüpi rakendus (Node.js, Java jne) [Külastajate käivitatava](../service-fabric/service-fabric-deploy-existing-app.md).
Skaala välja mitmes eksemplaris ilma ümberkorraldamine|X|X|X|X|Virtuaalmasinates saab skaala välja mitmes eksemplaris, kuid töötab nende teenuste tuleb kirjutada selle skaala-out käsitlema. Tuleb konfigureerida laadi koormusetasakaalustusteenuse marsruutimine taotlused on arvutites ja samaaegne taaskäivitamist kõik eksemplaride hooldustööd või riistvara tõrgete tõttu vältimiseks osaleja rühma loomine.
SSL-i tugi|X|X|X|X|Rakenduse teenuse veebirakendustes SSL-i jaoks kohandatud domeeninime on toetatud ainult põhi- ja Standard režiim. SSL-i veebirakenduste kasutamise kohta leiate teavet teemast [Azure veebisait jaoks SSL-serdi konfigureerimine](../app-service-web/web-sites-configure-ssl-certificate.md).
Visual Studio integreerimine|X|X|X|X|
Remote silumine|X|X|X||
Koodi TFS juurutamine|X|X|X|X|
Võrgu eraldamise [Azure virtuaalse](/services/virtual-network/) võrguga|X|X|X|X|Vt ka [Azure veebisaitide virtuaalse võrgu integreerimine](/blog/2014/09/15/azure-websites-virtual-network-integration/)
[Azure'i liikluse haldur](/services/traffic-manager/) tugi|X|X|X|X|
Integreeritud lõpp-punkti jälgimine|X|X|X||
Kaugtöölaua serverid juurdepääs||X|X|X|
Mis tahes kohandatud MSI installimine||X|X|X|Teenuse struktuuri abil saate majutada mis tahes täitmisfail [Külastajate käivitatava](../service-fabric/service-fabric-deploy-existing-app.md) või rakendust, saate installida VMs.
Võimalus käivitamise ülesannete täitmiseks/määratlemine||X|X|X|
Saate kuulata ETW sündmused||X|X|X|

## <a name="scenarios"></a>Stsenaariumid ja soovitused

Siin on mõned levinud rakenduse stsenaariumid koos soovitustega selle kohta, millised Azure web hosting suvand võib olla sobivaim iga.

- [Mul on vaja web esiosa tausta töötlemine ja andmebaasi kirjutamata käivitada äri rakendusi eeldusel varad integreeritud.](#onprem)
- [Vajan usaldusväärne viis majutada oma ettevõtte veebisaiti, mida ka skaala ja pakutakse globaalne saavutamiseks.](#corp)
- [Mul on rakenduse IIS6, kus töötab Windows Server 2003.](#iis6)
- [Ma olen väikeettevõtte omanik ja odav viis minu saidi hosti on vaja, kuid meeles kasvu tulevikus.](#smallbusiness)
- [Ma olen veebis või kujundaja ja tahan kujundada ja koostada veebisaitide oma kliendi jaoks.](#designer)
- [Minu mitmekihilise rakenduse Web ees migreerimisse pilveteenuses põhjus](#multitier)
- [Minu sõltub väga kohandatud Windowsi või Linuxi keskkonnas ja tahan teisaldada pilve.](#custom)
- [Minu saidi kasutab avage ja soovin selle Azure majutada.](#oss)
- [Mul on ka ärivaldkonna rakendus, mis peab teie ettevõtte võrguga.](#lob)
- [Soovin majutada REST API-ga või veebiteenus mobiiliklientide jaoks.](#mobile)


### <a id="onprem"></a>Mul on vaja web esiosa tausta töötlemine ja andmebaasi kirjutamata käivitada äri rakendusi eeldusel varad integreeritud.

Azure'i rakenduse teenus on suurepärane lahendus keerukate ärirakenduste. Saate välja töötada ka selliseid rakendusi, mis mastaapimiseks automaatselt laadi tasakaalustatud platvormi, Active Directory on turvatud ja ühenduse oma kohapealse ressursse. See muudab need tipptasemel portaali ja API-de kaudu lihtne rakenduste haldamine ja võimaldab teil saada aimu, kuidas kliendid kasutavad neid app ülevaade tööriistad. Funktsiooni [Webjobs][] saate käivitada taustaprotsessid ja tööülesannete osana oma web taseme, samal ajal hübriid Ühenduvus ja VNET funktsioonid oleks hõlpsam ühenduse uuesti kohapealse ressursid. Azure'i rakenduse teenus pakub kolme 9 SLA veebirakenduste ja võimaldab teil:

* Käivitage rakenduste usaldusväärselt iseparandavat, automaatne lappimine pilve platvormi.
* Globaalne võrguühendust, andmekeskuste skaala automaatselt.
* Varundamine ja taastamine katastroofiabi.
* Olema ISO, SOC2 ja PCI nõuetele.
* Active Directory integreerimine

### <a id="corp"></a>Vajan usaldusväärne viis majutada oma ettevõtte veebisaiti, mida ka skaala ja pakutakse globaalne saavutamiseks.

Azure'i rakenduse teenus on suurepärane lahendus ettevõtte veebisaidi majutuse. See võimaldab veebirakenduste mastaapimiseks kiiresti ja kerge vaevaga rahuldamiseks andmekeskuste globaalse võrgu üle. See pakub kohaliku REACHi, tõrketaluvust ja nutikad liikluse haldus. Kõik klõpsake platvorm, mis pakub tipptasemel Haldusriistad, mis võimaldab teil ülevaate saidi seisundi ja saidi kiiresti ja kerge vaevaga. Azure'i rakenduse teenus pakub kolme 9 SLA veebirakenduste ja võimaldab teil:

* Käivitage veebisaidiga usaldusväärselt platvormil iseparandavat, automaatne lappimine pilve.
* Globaalne võrguühendust, andmekeskuste skaala automaatselt.
* Varundamine ja taastamine katastroofiabi.
* Logide hallata ja liiklus integreeritud tööriistu.
* Olema ISO, SOC2 ja PCI nõuetele.
* Active Directory integreerimine

### <a id="iis6"></a>Mul on rakenduse IIS6, kus töötab Windows Server 2003.

Azure'i rakendust Service lihtne vältida rakendustega seotud migreerimine vanemate IIS6 taristu kulusid. Microsoft on loonud [lihtne kasutada tööriistu ja üksikasjalik migreerimise juhiseid](https://www.movemetowebsites.net/) mis võimaldavad teil kontrolli ühilduvust ja kindlaks teha muudatusi, mida tuleb teha. Visual Studio ja TFS levinud CMS tööriistad integreerimine on lihtne juurutada IIS6 rakendused otse pilveteenusesse. Kui juurutada, Azur portaali pakub töökindlaid Haldusriistad, mis võimaldavad teil hallata kulud ja kuni koosolekud nõuda vastavalt vajadusele mahtu. Migreerimistööriista saate teha järgmist.

* Saate kiiresti ja hõlpsalt migreerida pärand Windows Server 2003 veebirakenduses pilveteenusesse.
* Valida, kas teie manustatud SQL-i andmebaasi kohapealne hübriid teenuserakenduse loomine jäta.
* Automaatselt teisaldada pärand taotlus SQL-i andmebaasist.

### <a id="smallbusiness"></a>Ma olen väikeettevõtte omanik ja odav viis minu saidi hosti on vaja, kuid meeles kasvu tulevikus.

Azure'i rakenduse teenus on suurepärane lahendus selle stsenaariumi korral, sest saate hakata seda kasutama tasuta ja seejärel lisage veel võimalusi, kui neid vajate. Iga tasuta web appi on esitatud Azure domeeniga (*your_company*. azurewebsites.net), ja platvorm sisaldab integreeritud juurutus- ja tööriistu, kui ka rakenduse Galerii, mis on lihtne alustada. On palju muud teenused ja skaleerimise suvandid, mis võimaldavad töötada koos suurema kasutaja nõudmisel saidile. Azure'i rakendust Service, saate teha järgmist.

- Alustage tasuta taseme ja siis skaala kuni vastavalt vajadusele.
- Rakenduse Galerii abil saate kiiresti häälestada populaarsed veebirakendusi, nt WordPress.
- Lisage Azure lisateenuse ja funktsioonid rakenduse vastavalt vajadusele.
- Kinnitage oma veebirakenduse https-iga.

### <a id="designer"></a>Ma olen veebis või kujundaja ja tahan kujundada ja koostada veebisaitide oma kliendi jaoks

Web arendajate ja kujundajate, Azure'i rakendust Service hõlpsalt integreerub raamistiku mitmesuguseid tööriistu, sisaldab juurutamise tuge Git ja FTP ja pakub tihedalt integreerimine tööriistad ja Visual Studio ja SQL-andmebaasi. Rakenduse teenus, saate teha järgmist.

- Käsurea tööriistu saate kasutada [automatiseeritud]tööülesannete[scripting].
- Populaarsed keeled, nt [.net]töötamine[dotnet], [PHP][], [Node.js][nodejs], ja [Python][].
- Valige kolme skaleerimise erinevat jaoks, et väga kõrge võimalusi ülespoole.
- Integreerida teisi Azure teenuseid, näiteks [SQL-andmebaasi][sqldatabase], [Teenuse siini] [ servicebus] ja [salvestusruumi][]või partnerite pakkumisi [Azure'i poe][azurestore], nt MySQL-i ja MongoDB.
- Näiteks Visual Studios, Git, WebMatrix, WebDeploy, TFS ja FTP integreerida.

### <a id="multitier"></a>Ma olen oma mitmekihilise rakenduse Web ees pilveteenusesse migreerimine

Kui teil on mitmekihilise rakenduse, nt veebiserverile, mis ühendab andmebaasiga, Azure'i rakenduse teenus on hea valik, mis pakub tihedalt integreerimine Azure SQL-andmebaas. Ja saate kasutada funktsiooni WebJobs jaoks töötavate protsesside taustväärtus.

Valige teenuse struktuuri ühe või mitme oma astme, kui teil on vaja rohkem juhtida serveri keskkonnas, näiteks võimalust remote oma server konfigureerimine serveri käivitus tööülesanded.

Kui soovite kasutada oma arvuti pildi või serveritarkvara või teenuseid, mida te ei saa konfigureerida teenuse struktuuri käivitada, valige Virtuaalmasinates ühe või mitme oma astme.

### <a id="custom"></a>Minu sõltub väga kohandatud Windowsi või Linuxi keskkonnas ja tahan teisaldada pilve.

Kui teie rakendus nõuab keerukate installimist või tarkvara ja operatsioonisüsteemi konfigureerimine, Virtuaalmasinates ilmselt parim lahendus. Virtuaalmasinates, saate teha järgmist.

- Alustage operatsioonisüsteemi Windows või Linux, nt virtuaalse masina galerii abil ja seejärel Kohandage oma rakenduse nõuded.
- Luua ja üles laadida olemasoleva kohapealse server Azure'i virtuaalse masina käivitada kohandatud pildi.

### <a id="oss"></a>Minu saidi kasutab avage ja soovin selle hosti Azure

Kui teie avatud allika framework on toetatud keelte rakenduse teenuses ja rakenduse jaoks vajalik raamistik on konfigureeritud teie jaoks automaatselt. Rakenduse teenus, mis võimaldab teil.

- Paljude populaarsete avatud allikas keelte kasutamine, nt [.NET][dotnet], [PHP][], [Node.js][nodejs], ja [Python][].
- WordPress, Drupal, Umbraco, DNN ja paljude muude tootjate veebirakenduste häälestamine.
- Migreerimine taotluse või looge uus rakendus galeriist.

Kui rakenduse teenus ei toeta teie avatud allika framework, saate selle käivitada ühele muude Azure web hosting suvandid. Koos Virtuaalmasinates, installimist ja konfigureerimist tarkvara seadme pilt, mis võib olla Windowsi või Linuxi-ga.

### <a id="lob"></a>Mul on ka ärivaldkonna rakendus, mis peab teie ettevõtte võrguga

Kui soovite luua rakenduse ärivaldkonna, veebisaidi nõuda otsest juurdepääsu teenustele või andmetele ettevõtte võrgus. See on võimalik rakenduse teenus, teenuse struktuuri ja Virtuaalmasinates [Azure virtuaalse võrguteenuse](/services/virtual-network/)abil. Rakenduse teenuse saate kasutada funktsiooni [VNET integreerimise funktsioon](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/), mis võimaldab Azure rakenduste käivitamiseks, kui nad teie ettevõtte võrgus.

### <a id="mobile"></a>Soovin majutada REST API-ga või veebiteenus mobiiliklientide jaoks

HTTP-põhine veebiteenused võimaldavad toetavad erinevaid kliendid, sh mobiilikliendid. ASP.net-i veebi-API nagu raamistiku integreerida Visual Studio lihtsam luua ja kasutada ülejäänud teenused.  Nende teenuste puutuvad web lõpp-punkti, nii et see on võimalik kasutada mis tahes web hosting tehnika Azure toeta seda stsenaariumi. Siiski rakenduse teenus on hea valik hosting REST API-d. Rakenduse teenus, saate teha järgmist.

- Kiiresti luua [mobiilirakenduse](../app-service-mobile/app-service-mobile-value-prop.md) või [API rakenduse](../app-service-api/app-service-api-apps-why-best-platform.md) HTTP veebiteenuse ühes Azure globaalselt hajutatud andmekeskuste majutada.
- Migreerimine olemasolevad teenused ega uusi faile luua.
- Saavutada SLA kättesaadavus abil ühte eksemplari või mitme sihtotstarbeline masinad välja skaala.
- HTTP klientidele, sh mobiilikliendid REST API-d pakuvad avaldatud saidi abil.

> [AZURE.NOTE]
> Kui soovite alustada Azure'i rakendust Service enne konto kasutajaks, avage <a href="https://trywebsites.azurewebsites.net/">https://trywebsites.azurewebsites.net</a>, kus saate kohe luua lühiajaline starter rakenduse teenuses Azure rakenduse tasuta. Pole vaja krediitkaarti, kohustusi.

## <a id="nextsteps"></a>Järgmised sammud

Lisateavet kolm web hosting võimalusi, leiate [Azure'i tutvustus](../fundamentals-introduction-to-azure.md).

Arvelduskuupäeva, valite rakenduse kasutamise alustamine, leiate järgmistest teemadest:

* [Azure'i rakendust Service](/documentation/services/app-service/)
* [Azure pilveteenused](/documentation/services/cloud-services/)
* [Azure'i Virtuaalmasinates](/documentation/services/virtual-machines/)
* [Teenuse struktuuri](/documentation/services/service-fabric)

<!-- URL List -->

[Azure'i rakendust Service]: /services/app-service/
[Pilveteenused]: http://go.microsoft.com/fwlink/?LinkId=306052
[Virtuaalmasinates]: http://go.microsoft.com/fwlink/?LinkID=306053
[Teenuse struktuuri]: /services/service-fabric
[ClearDB]: http://www.cleardb.com/
[WebJobs]: http://go.microsoft.com/fwlink/?linkid=390226&clcid=0x409
[Configuring an SSL certificate for an Azure Website]: http://www.windowsazure.com/develop/net/common-tasks/enable-ssl-web-site/
[azurestore]: http://www.windowsazure.com/gallery/store/
[scripting]: http://www.windowsazure.com/documentation/scripts/?services=web-sites
[dotnet]: http://www.windowsazure.com/develop/net/
[nodejs]: http://www.windowsazure.com/develop/nodejs/
[PHP]: http://www.windowsazure.com/develop/php/
[Python]: http://www.windowsazure.com/develop/python/
[servicebus]: http://www.windowsazure.com/documentation/services/service-bus/
[sqldatabase]: http://www.windowsazure.com/documentation/services/sql-database/
[Salvestusruumi]: http://www.windowsazure.com/documentation/services/storage/

<!-- IMG List -->

[ChoicesDiagram]: ./media/choose-web-site-cloud-service-vm/Websites_CloudServices_VMs_3.png
