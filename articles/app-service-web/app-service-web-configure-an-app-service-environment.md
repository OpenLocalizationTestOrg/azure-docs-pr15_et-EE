<properties
    pageTitle="Kuidas konfigureerida rakenduse teenuse keskkonnas | Microsoft Azure'i"
    description="Konfiguratsioon, haldus ja rakenduse teenuse keskkonnas."
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


# <a name="configuring-an-app-service-environment"></a>Keskkonnas rakendust Service konfigureerimine #

## <a name="overview"></a>Ülevaade ##

Azure'i rakenduse teenuse keskkonna koosneb kõrge, mitu põhikomponentide:

- Arvuta ressursse, mis töötavad rakenduse teenuse keskkonnas majutatud teenus
- Salvestusruumi
- Andmebaasi
- Classic(V1) või ressursside Manager(V2) Azure virtuaalse võrgu (VNet) 
- Mõne alamvõrku rakenduse teenuse keskkond, mis on majutatud teenus ei tööta

### <a name="compute-resources"></a>Arvutage ressursse

Saate kasutada oma nelja ressursi kaustu Arvuta ressursse.  Iga rakenduse teenuse keskkonnas (ASE) on seatud esi lõpetatakse ja kolm võimalikku töötaja. Te ei pea kasutada kõigi kolme töötaja kaustu – kui soovite, saate kohe kasutada ühte või kahte.

Ressursi kaustadesse (esi lõpetatakse ja töötajad) hosts ei ole otsest juurdepääsu rentnikega. Ei saa neid ühendada, muuta nende ettevalmistamise Kaugtöölaua protokolli (RDP) abil või toimivad need administraatoriks.

Saate seada ressursi rakenduskausta kogus ja suurus. Teil on mõni ASE neli suvandid, mis on sildistatud P1 kuni P4. Nende suurused ja nende hinnad kohta leiate üksikasjalikumat teavet teemast [rakenduse teenuse hinnad](../app-service/app-service-value-prop-what-is.md).
Kogus või suuruse muutmist nimetatakse skaala toiming.  Korraga saab ainult üks skaala toiming on pooleli.

**Ette lõpeb**: esi on teie ASE hoitava rakenduste HTTP-või HTTPS lõpp-punktid. Töökoormus ei läbiviimisel esi lõpetatakse.

- Mõne ASE algab kahe P2s, mille piisab arendaja katse töökoormus ja madala tootmise töökoormus. Soovitame P3s jaoks mõõdukas raske tootmise töökoormus.
- Et raske tootmise töökoormus mõõdukas, soovitame, et teil on vähemalt neli P3s tagamiseks on piisavalt esi lõpetatakse töötava ajastatud hooldustööd. Ajastatud hooldustööd toob haaval allapoole ühe esiosa. See vähendab üldist ees võimsus ajal hooldustööd.
- Ei saa lisada kohe ees uue eksemplari. Nad saavad võtta 2-3 tundi ettevalmistamine.
- Jaoks täiendavaid skaala peenhäälestus jälgima CPU protsenti, mälu protsenti ja aktiivse taotluste mõõdikud ees pool. Kui CPU või mälu protsendid on 70% P3s käivitamisel, lisage veel esi lõpetatakse. Kui väärtus aktiivse taotluste keskmise kuni 20 000 taotluste kohta esiosa 15000, tuleks lisada ka rohkem esi lõpetatakse. Üldine eesmärk on säilitada CPU ja mälu protsentide allpool 70% ja aktiivse taotluste keskmistamine abil all 15000 taotluste arv ette lõpetada, kui teil on P3s.  

**Töötajate**: töötajatel, kus teie rakendused tegelikult käivitada. Kui teie plaanid rakenduse teenuse skaalal, mis kasutab töötajate seotud töötaja pool üles.

- Kiire lisada ei saa kasutada. Need võib võtta 2 kuni 3 tundi ettevalmistamine, olenemata sellest, kui palju on lisatud.
- Arvuta ressurss, mille igal pool suurust skaleerimist võtab 2-3 tundi iga update domeeni. On 20 update domeenide mõne ASE. Kui te neid töötaja pool 10 eksemplari koos Arvuta suurust, võib kuluda 20 – 30 tundi lõpuleviimiseks.
- Kui töötaja pool kasutatud Arvuta ressursside suuruse muutmiseks saate põhjustab külm käivitamise rakenduste, mis töötavad selle töötaja pargi.

Kiireim viis töötaja kausta, mis ei tööta rakendustel Arvuta ressursi suuruse muutmine on:

- Skaala alla eksemplari arv 0. Deallocate eksemplaride kulub umbes 30 minutit.
- Valige uus Arvuta suurus ja eksemplaride arv. Siit kulub 2 – 3 tundi.

Kui rakenduste suuremaks Arvuta ressurss, ei saa ära eelmise juhised. Asemel töötaja kausta, mis majutab nendes rakendustes suuruse muutmine saate asustada teise töötaja pargi soovitud töötajatega ja teisaldada rakendused selle ühendada.

- Luua teise töötaja pargi vajadusele Arvuta suurus ülejäänud esinemisjuhud. See võtab 2-3 tundi lõpuleviimiseks.
- Saate määrata oma rakenduse teenuse lepingud, millele on hostiteenuse rakendused, mis tuleb ühendada äsja konfigureeritud töötaja suuremaks. See on kiire toiming, mis peaks tegema vähem kui minutiga lõpuleviimiseks.  
- Kui enam ei vaja neid kasutamata eksemplari skaala esimese töötaja pool allapoole. See toiming võtab aega umbes 30 minutit lõpuleviimiseks.

**Autoscaling**: üks tööriistu, mis aitavad teil hallata oma Arvuta ressursside tarbimine on autoscaling. Saate kasutada autoscaling jaoks ees- või töötaja kaustu. Saate teha näiteks kasvavad igal pool tüüpi eksemplaride hommikul ja vähendada seda funktsiooni õhtused. Või võib-olla saate lisada eksemplarid, kui töötajaid, mis on saadaval töötaja rakenduskausta langeb teatud piirmäärast.

Kui soovite seada autoscale reeglid Arvuta ressursi rakenduskausta mõõdikute ümber, siis võtke arvesse aega, mida nõuab ettevalmistamise. Autoscaling rakenduse teenuse keskkonnas kohta leiate lisateavet teemast [Kuidas konfigureerida rakenduse teenuse keskkonnas autoscale][ASEAutoscale].

### <a name="storage"></a>Salvestusruumi

Iga ASE on konfigureeritud 500 GB salvestusruumi. Seda ruumi kasutatakse üle kõik rakendused ASE. See salvestusruumi on osa ASE ja praegu ei saa kasutada oma salvestusruumi vahetada. Kui olete kohanduste virtuaalse võrgu marsruutimine või turvalisus, peate endiselt Azure Storage--juurdepääsu lubamiseks või ASE ei tööta.

### <a name="database"></a>Andmebaasi

Andmebaas sisaldab teavet, mis määratleb keskkonna ning rakendused, mis töötavad kogumahu üksikasjade. Ka see osa Azure'i hoitavate tellimusega. See pole midagi, mida teil on otsene võimalus käsitsemiseks. Kui olete kohanduste virtuaalse võrgu marsruutimine või turvalisus, peate esmalt lubama endiselt SQL Azure'i--juurdepääsu või ASE ei tööta.

### <a name="network"></a>Võrgu

VNet, mida kasutatakse koos oma ASE võib olla üks, mille tegite ASE loomisel või mõni varem tehtud ette valmistada. Kui loote alamvõrgu ASE loomise ajal, see sunnib ASE olema sama nimega virtuaalse võrgu ressursirühma. Kui teil on vaja varem oma ASE erinevas oma VNet ressursirühma, siis peate looma oma ASE ressursi halduri malli abil.

On mõned piirangud virtuaalse võrk, mida on kasutatud on ASE.
 
- Virtuaalse võrgu peab olema piirkondliku virtuaalse võrgu.
- Peab olema on alamvõrgu aadressidega 8 või rohkem kui ASE on juurutatud.
- Pärast soovitud alamvõrgu kasutatakse majutada ka ASE, aadress vahemiku alamvõrgu ei saa muuta. Seetõttu soovitame, et alamvõrgu sisaldab vähemalt 64 aadressid majutada mis tahes ASE kasvu tulevikus.
- Ei saa midagi muud alamvõrgu kuid ASE.

Erinevalt majutatud teenus, mis sisaldab ASE, [virtuaalse võrgu] [ virtualnetwork] ja alamvõrgu on kasutaja kontrolli all.  Saate hallata virtuaalse võrgu virtuaalse võrgu Kasutajaliidese või PowerShelli kaudu.  Klassikaline või ressursside halduri VNet saab kasutada ka ASE.  Portaali ja API funktsioonid on pisut erinev klassikaline ja ressursihaldur VNets, kuid ASE on samad.

Majutada ka ASE kasutatava VNet abil saate kas privaatne RFC1918 IP-aadresside või saate seda kasutada avaliku IP-aadressid.  Kui soovite kasutada IP vahemik, mis ei hõlma RFC1918 (10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16) siis peate looma oma VNet ja alamvõrgu kasutada oma ASE enne ASE loomine.

Kuna seda võimalust paneb virtuaalse võrgu Azure'i rakendust Service, tähendab see, kas teie rakendused, mis on majutatud teie ASE nüüd pääseb ressursse, mis tehakse kättesaadavaks ExpressRoute või -saidilt virtuaalse võrgud (VPN) kaudu otse. Rakendused, mis pole teie rakenduse teenuse keskkonnas ei nõua täiendavad ressursid virtuaalse võrku, mis majutab rakenduse teenuse keskkonna juurdepääsu funktsiooni. See tähendab, et ei pea VNET integreerimise või hübriid ühenduste abil pääsete juurde ressursse tekstivormingus või virtuaalse võrku ühendatud. Siiski saate nii nende funktsioonide kuigi Accessi ressursid võrkudes, mis on ühendatud virtuaalse võrgu.

Näiteks saate integreerida virtuaalse võrguga, mis on teie tellimus, kuid pole ühendatud virtuaalse võrku, mis on teie ASE VNET integreerimine. Saate siiski ka hübriid ühendused Accessi ressursse, mis on muude võrkude täpselt samamoodi nagu tavaliselt saate.  

Kui teil on konfigureeritud ExpressRoute VPN-i virtuaalse võrgu, peaks olema teadlik mõned marsruutimise eripäradele, mis sisaldab ka ASE. On mõned kasutaja määratletud marsruutimiseks (UDR) konfiguratsiooni, mis ei ühildu mõne ASE. Lisateavet kohta, kuidas mõni ASE virtuaalse võrgu ExpressRoute, leiate, [töötavad rakenduse teenuse keskkond on virtuaalse võrgu ExpressRoute][ExpressRoute].

#### <a name="securing-inbound-traffic"></a>Sissetulev liiklus turvamine

On kaks esmane meetodid kontrollida oma ASE sissetulev liiklus.  Võrgu turvalisus Groups(NSGs) abil saate määrata, millised IP aadressid pääsete juurde oma ASE nagu allpool kirjeldatud, [Kuidas määrata sissetulev liiklus keskkonnas rakenduse teenuse](app-service-app-service-environment-control-inbound-traffic.md) ja saate konfigureerida oma ASE on sisemine laadi Balancer(ILB).  Neid funktsioone saab kasutada ka koos kui soovite piirata juurdepääsu NSGs, et teie ILB ASE abil.

Kui loote mõnda ASE, see loob VIP oma VNet.  On kaks VIP, sise- ja.  Mõne välise VIP on ASE loomisel siis oma rakendused oma ASE on kättesaadavad Interneti marsruuditavaid IP-aadress. Kui valite oma ASE Internal koos mõne ILB konfigureeritakse ja ei ole otse internet kättesaadav.  Mõne ILB ASE nõuab veel mõne välise VIP, kuid seda kasutatakse ainult Azure haldamine ja hooldus juurdepääsu.  

ILB ASE loomise ajal esitada kasutatavaid ILB ASE alamdomeen ja hallata oma DNS-i jaoks määratud alamdomeen.  Kuna alamdomeen nime seadmiseks peate ka haldamine HTTPS juurdepääsu kasutatavat serti.  Pärast ASE loomist teil palutakse sisestada sert.  Lisateavet rohkem loomine ja kasutamine on ILB ASE Loe [abil rakenduse teenuse keskkond on sisemine laadi koormusetasakaalustusteenuse][ILBASE]. 


## <a name="portal"></a>Portaal

Saate hallata ja jälgida rakenduse teenuse keskkonna Azure portaali Kasutajaliidese abil. Kui teil on mõni ASE, siis on tõenäoline, et näha oma vasaku rakenduse teenuse sümbol. See sümbol kasutatakse tähistada rakenduse teenuse keskkonnas Azure'i portaalis:

![Rakenduse teenuse keskkonna sümbol][1]

Kasutajaliides, mis sisaldab loendit kõigist oma rakenduse teenuse keskkonnas avamiseks ikooni või valige noolenuppu (">" sümbol) allosas külgriba valimiseks rakenduse teenuse keskkonnas. Valides ühe praeguseks, mis on loetletud, kui avate UI, mida kasutatakse jälgida ja seda hallata.

![Kasutajaliidese jälgimise ja haldamise rakenduse teenuse keskkonna][2]

Selle esimese blade kuvatakse mõned atribuudid oma ASE koos argumendil diagrammi ressursivaru kohta. Mõned **Essentialsi** blokeerimise kuvatud atribuudid on ka hüperlinke, mis avab tera, mis on sellega seotud. Näiteks saate avada virtuaalse võrgu, kus töötab teie ASE seotud UI **Virtuaalne võrgu** nimi. **Rakenduse teenuse lepingud** ja **rakendused** avada loendi need üksused, mis on teie ASE labad.  

### <a name="monitoring"></a>Jälgimine

Diagrammide võimaldavad näha erinevaid jõudluse mõõdikute iga ressursivaru. Ees pool, et saate jälgida Keskmine CPU ja mälu. Töötaja kaustu, et saate jälgida kogus, mida kasutatakse ja kogus, mis on saadaval.

Mitme rakenduse teenuse lepingud aitavad kasutamine töötajate töötaja pargis. Töökoormus on jaotatud samal viisil nagu ees serverid, et CPU ja mälu kasutus ei anna palju kasulikku teavet. Oluline veel jälgida mitu töötajate, mida olete kasutanud ja on saadaval – eriti siis, kui haldate süsteemi teistele kasutamiseks.  

Samuti saate kõik mõõdikud, mida saab jälgida diagrammides häälestada teatised. Siin teatiste seadistamine toimib sarnaselt sama rakenduse teenus. Teatise saate määrata, kas **teatisi** UI osast või süveneda just mis tahes mõõdikud UI sisse ja valige **Lisa teate**.

![Mõõdikute kasutajaliides][3]

Mõõdikud, mis olid lihtsalt on rakenduse teenuse keskkonna mõõdikud. Olemas on ka mõõdikute, mis on saadaval rakenduse teenindustaseme leping. See on, kus CPU ja mälu teeb palju tunde.

On ASE kõik rakenduse teenuse lepingud on spetsiaalne rakenduse teenuse lepingud. Mida tähendab, et ainult rakendusi, mis töötavad eraldatud hosts rakenduse teenusleping on selle rakenduse teenusleping rakendused. Üksikasjade kuvamiseks klõpsake oma rakenduse teenusleping avab oma rakenduse teenusleping loendites ASE UI või **liikuge rakenduse teenuse lepingud** (mis on loetletud kõik need).   

### <a name="settings"></a>Sätted

Labale ASE on mitu olulist võimaluste sisaldava jaotise **sätted** :

**Sätete** > **Atribuudid**: **sätted** tera avaneb automaatselt, kui teie ASE tera tuua. Ülaosas on **Atribuudid**. Mitmesuguseid siin liigsed osad **Essentialsi**üksused, kuid mis on väga kasulik on **Virtuaalse IP-aadress**, samuti **Väljaminevate IP-aadressid**.

![Sätete blade ja atribuudid][4]

**Sätete** > **IP-aadressid**: teie ASE IP Secure Sockets Layer (SSL) rakenduse loomisel peate SSL-i IP-aadressi. Üks saamiseks peab teie ASE IP SSL-i aadressid, mis see on omanik, mida saab. Kui ka ASE on loodud, on selleks üks IP SSL-i aadress, kuid saate lisada rohkem. Ei tasu jaoks SSL-i täiendavad IP-aadressid, nagu on näidatud [Rakenduse teenuse hinnad] [ AppServicePricing] (jaotises SSL-ühenduste kohta). Täiendav hind on IP SSL-i hind.

**Sätted** > **Eesserveriparki** / **Töötaja kaustu**: iga nende ressursside rakenduskausta labad pakub võimalust ainult selle ressursivaru lisaks täielikult mastaapimiseks selle ressursivaru juhtelementide kohta teabe kuvamiseks.  

Iga ressursivaru base höövlitera pakub selle ressursivaru mõõdikute diagrammi. Nii nagu keelest ASE diagrammidega saate minna diagrammi ja häälestage teatised vastavalt soovile. Teatise jaoks teatud ressursivaru keelest ASE säte ei sama mis aitab selle ressursi kaustast. Keelest töötaja rakenduskausta **sätted** pääsete juurde kõik rakendused või rakenduse teenuse lepingud töötavatest töötaja selles kaustas.

![Töötaja rakenduskausta sätted kasutajaliides][5]

### <a name="portal-scale-capabilities"></a>Portaali skaala võimalused  

On kolm skaala toimingud.

- IP-aadresside ASE mis on saadaval IP SSL-i kasutamine arvu muutmine.
- Arvuta ressurss, mida kasutatakse ressursivaru suuruse muutmine.
- Arvuta ressursse, mis on kasutusel ressursivaru, kas käsitsi või autoscaling arvu muutmine.

Portaalis on kolm võimalust määrata mitu serverit, et teil on oma ressursi kaustadesse.

- Skaala tegevuse peamine ASE keelest ülaosas. Saate teha mitu skaala konfiguratsioonimuudatuste ees- ja töötaja kaustu. Need kõik rakendatud ühe toiminguga.
- Käsitsi skaala toiming keelest üksikute ressursside pool **skaala** , mis on jaotises **sätted**.
- Autoscaling, mille seadistasite keelest üksikute ressursside pool **skaala** .

Lohistage liugur skaala ASE enne selle toimingu kasutamiseks kogus, mida soovite ja salvestage. Selle kasutajaliides toetab ka suuruse muutmine.  

![Skaala kasutajaliides][6]

Teatud ressursivaru kasutusjuhendit või autoscale võimaluste kasutamiseks valige **sätted** > **Eesserveriparki** / **Töötaja kaustu** vastavalt vajadusele. Seejärel avab kausta, mida soovite muuta. Avage **sätted** > **skaala läbi** või **sätted** > **skaalal**. **Skaala välja** tera võimaldab teil määrata eksemplari kogus. **Skaala üles** võimaldab teil määrata ressursi suurus.  

![Skaala sätteid kasutajaliides][7]

## <a name="fault-tolerance-considerations"></a>Tõrketaluvust kaalutlused

Saate konfigureerida keskkonnas rakenduse teenuse kasutamiseks 55 kogusumma Arvuta ressursid. Nende 55 Arvuta ressursside saab kasutada ainult 50 host töökoormus. Selle põhjuseks on kaks. On vähemalt 2 ees Arvuta ressursid.  Mis jätab kuni 53 töötaja-pool jaotamise toetamiseks. Tõrketaluvust pakkuda, peate olema täiendavad Arvuta ressurss, mis on eraldatud järgmiste reeglite kohaselt:

- Iga töötaja kaust peab vähemalt 1 täiendavad Arvuta ressurss, mida ei saa määrata on töökoormus.
- Kui Arvuta ressursside töötaja pargis kogus läheb teatud väärtus ülaltoodud, seejärel mõni muu Arvuta ressurss on vaja tõrketaluvust. See kehtib mitte ees pool.

Ühe töötaja igal pool, sees tõrketaluvust nõuded on mis antud väärtuse X määratud töötaja rakenduskausta ressursid:

- Kui X on 2 – 20, kasutuskõlblikud Arvuta ressursse, mida saate kasutada töökoormus on x-1.
- Kui X on 21-40, kasutuskõlblikud Arvuta ressursse, mida saate kasutada töökoormus on X-2.
- Kui X on 41 – 53, kasutuskõlblikud Arvuta ressursse, mille abil saate töökoormus on x-3.

Minimaalne andmed väiksemates vähem ruumi on 2 ees serverid ja 2 töötajate.  Ülaltoodud andmete siis siin on mõned näited selle kohta:  

- Kui teil on 30 töötajate ühe kausta, siis 28 neid saab kasutada host töökoormus.
- Kui teil on 2 töötajate ühe kausta, siis 1 saab host töökoormus.
- Kui teil on 20 töötajat ühe kausta, siis 19 saab host töökoormus.  
- Kui teil on 21 töötajate ühe kausta, siis ikka ainult 19 saab kasutada host töökoormus.  

Proportsioonid – tõrketaluvust on oluline, kuid peate pidage meeles, kui muudate ületab teatud piirmäära. Kui soovite lisada veel võimsus läheb 20 eksemplari, siis avage 22 või uuem versioon Kuna 21 ei lisa veel tegutsemine. Sama kehtib minna üle 40, kus on järgmine arv, mis lisab võimsus 42.  

## <a name="deleting-an-app-service-environment"></a>Rakenduse teenuse keskkonnas kustutamine ##

Kui soovite kustutada rakenduse teenuse keskkonnas, siis lihtsalt kasutada **kustutamine** toimingut rakenduse teenuse keskkonna tera ülaosas. Kui te seda teete, palutakse teil sisestage nimi rakenduse teenuse keskkonna kinnitada, et soovite tõepoolest tehke järgmist. Pange tähele, et keskkonnas rakenduse teenuse kustutamisel ei kustutada kogu sisu kogumahu ka.  

![Rakenduse teenuse keskkonnas UI kustutamine][9]  

## <a name="getting-started"></a>Alustamine

Alustada rakenduse teenuse keskkonnas, vaadake, [Kuidas luua rakenduse teenuse keskkond](app-service-web-how-to-create-an-app-service-environment.md).

Azure'i rakendust Service platform kohta leiate lisateavet teemast [Azure rakenduse teenust](../app-service/app-service-value-prop-what-is.md).

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!--Image references-->
[1]: ./media/app-service-web-configure-an-app-service-environment/ase-icon.png
[2]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-aseblade.png
[3]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-poolchart.png
[4]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-properties.png
[5]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-poolblade.png
[6]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-scalecommand.png
[7]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-poolscale.png
[8]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-pricingtiers.png
[9]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-deletease.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[Appserviceplans]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[HowtoScale]: http://azure.microsoft.com/documentation/articles/app-service-web-scale-a-web-app-in-an-app-service-environment/
[ControlInbound]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[ExpressRoute]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/
[ILBASE]: http://azure.microsoft.com/documentation/articles/app-service-environment-with-internal-load-balancer/
