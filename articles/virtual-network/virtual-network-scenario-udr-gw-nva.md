<properties 
   pageTitle="2 rakendus ühenduse hübriid | Microsoft Azure'i"
   description="Saate teada, kuidas virtuaalne seadmed ja UDR mitmekihilise rakenduse keskkonna loomiseks Azure juurutamine"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/05/2016"
   ms.author="jdial" />

# <a name="virtual-appliance-scenario"></a>Virtuaalse seadme stsenaarium

Levinud stsenaariumi vahel suuremat Azure kliendi on vaja kahe mitmetasandilise rakendus, mis on esitatud Interneti-ühendus, võimaldades juurdepääsu tagasi taseme, on kohapealse andmekeskuse kaudu. Selle dokumendi juhendab teid stsenaarium kasutaja määratletud marsruudib (UDR), VPN-lüüsi ja võrgu virtuaalse seadmete abil juurutada kaheastmelise keskkond, mis vastab järgmistele tingimustele:

- Veebirakenduse peab olema ainult avaliku Interneti kaudu.
- Veebiserver majutusteenuse rakendus peab olema taustväärtus rakenduste serveri juurde pääseda.
- Kogu Internetis veebirakendusse liikluse tuleb läbida tulemüüri virtuaalse seadme. Selle virtuaalse seadme kasutatakse ainult Interneti-liikluse.
- Kõigi liiklus läheb serveriga rakenduse tuleb läbida tulemüüri virtuaalse seadme. Selle virtuaalse seadme kasutatakse taustväärtus end serveri juurdepääsu ja kohapealse võrgu kaudu VPN-lüüsi tulevad juurdepääsu.
- Administraatorid peavad olema hallata oma kohapealse arvuti tulemüüri virtuaalne seadmed, kasutades kolmanda tulemüür virtuaalse seadme juhtimise eesmärgil kasutatakse.

See on standardne DMZ stsenaarium on DMZ ja kaitstud võrgus. Sellisel juhul saate ehitatud Azure NSGs, tulemüüri virtuaalne seadmed või mõlema kombinatsiooni kasutades. Järgmises tabelis antakse ülevaade mõne spetsialistidele ja miinuste NSGs ja tulemüüri virtuaalse seadmete vahel.

||Spetsialistidele|Miinuste|
|---|---|---|
|NSG|Tasuta. <br/>Integreeritud Azure RBAC. <br/>ARM malle saab luua reeglid.|Keerukuse võivad olla erinevad suuremat keskkonnas. |
|Tulemüür|Andmete lennuk üle täielik kontroll. <br/>Läbi tulemüüri konsooli keskne haldamine.|Tulemüüri seadme maksumus. <br/>Pole integreeritud Azure RBAC.|

Lahendus allpool kasutab tulemüüri virtuaalne seadmed DMZ/kaitstud võrgu stsenaarium rakendada.

## <a name="considerations"></a>Kaalutlused

Saate juurutada keskkond, mis on selgitatud Azure abil erinevad täna, järgmiselt.

- **Virtuaalne võrgu (VNet)**. Mõne Azure'i VNet toimib sarnaselt kohapealse võrgu ja saate segmenditud sisse ühe või mitme alamvõrku liikluse eraldamise ja eraldamine probleemid.
- **Virtuaalne seadme**. Mitme partneriga pakuvad Azure'i turuplatsil, mida saate kasutada kolme tulemüürid, mis on kirjeldatud ülal virtuaalne seadmed. 
- **Kasutaja määratletud marsruudib (UDR)**. Marsruutimiseks tabelit võivad sisaldada UDRs kasutatavaid Azure võrgunduse abil kontrollida paketid on VNet sees. Järgmised tabelid marsruutimiseks saab rakendada alamvõrku. Üks uusimate funktsioonide Azure on võimalus GatewaySubnet, mis võimaldab edasi ühenduse hübriid Azure'i VNet tulevad virtuaalse seadme kogu liikluse marsruutimiseks tabeli rakendamine.
- **IP edastamine**. Vaikimisi Azure võrgu mootori suuna paketid virtuaalse võrgu kasutajaliidese kaardid (NICs) ainult siis, kui paketi sihtkoha IP-aadress vastab NIC IP-aadress. Seetõttu, kui on UDR määratleb, et pakett saadetaks antud virtuaalse seadme, Azure võrgu mootori langeb selle paketi. Pakett saadetakse VM (sel juhul virtuaalse seadme), mis pole selle tegeliku sihtkoha tagamiseks peate lubama IP edastamine virtuaalse seadme jaoks.
- **Võrgu turberühmad (NSGs)**. Järgmises näites ei tee NSGs kasutada, kuid võite kasutada NSGs rakendanud alamvõrku ja/või NICs see lahendus liikluse ja sealt need alamvõrku ja NICs täpsemaks filtreerimiseks.


![IPv6 ühenduvuse](./media/virtual-network-scenario-udr-gw-nva/figure01.png)

Selles näites on tellimus, mis sisaldab järgmist:

- 2 ressursside rühmad, ei kuvata soovitud diagrammi. 
    - **ONPREMRG**. Sisaldab kõik vajalikud simuleerida on kohapealse võrgu ressursse.
    - **AZURERG**. Sisaldab kõiki Azure virtuaalse keskkonnast vajalikud ressursid. 
- Mõne VNet nimega **onpremvnet** kasutatud jäljendamiseks kohapealse andmekeskuses, mis segmenditud nagu allpool.
    - **onpremsn1**. Alamvõrgu sisaldav töötab ka kohapealses serveris jäljendamiseks Ubuntu virtuaalse masina (VM).
    - **onpremsn2**. Sisaldavad töötab Ubuntu kohapealse arvuti, mis kasutavad administraator jäljendamiseks VM alamvõrgu.
- On ühe tulemüüri virtuaalse seadme nimega **OPFW** **onpremvnet** on tunneliga **azurevnet**soovite kasutada.
- Mõne VNet nimega **azurevnet** segmenditud, nagu allpool.
    - **azsn1**. Välise tulemüüri alamvõrgu välise tulemüüri kasutatakse. Kõik Interneti-liikluse tulevad selle alamvõrgu kaudu. Selle alamvõrgu sisaldab ainult NIC, mis on seotud välise tulemüüri.
    - **azsn2**. Esiosa alamvõrgu majutusteenuse VM töötab nimega veebiserverile, mis ei pääse Interneti kaudu.
    - **azsn3**. Taustväärtus alamvõrgu majutusteenuse taustväärtus application serveri, mis kuvatakse esiosa veebiserver VM.
    - **azsn4**. Juhtimise alamvõrgu kasutada ainult halduse juurdepääsu kõik tulemüüri virtuaalse seadmed. Selle alamvõrgu sisaldab ainult NIC, iga tulemüüri virtuaalse seadme kasutatav lahendus.
    - **GatewaySubnet**. Azure'i hübriid ühenduse alamvõrgu esitama ExpressRoute ja VPN-lüüsi Ühenduvus Azure VNets ja muude võrkude väliskontaktidega. 
- On 3 tulemüüri virtuaalse seadmete **azurevnet** võrku. 
    - **AZF1**. Välise tulemüüri esitatud avaliku Interneti-ühendus, kasutades avaliku IP-aadressi ressursi Azure. Peate, et teil oleks malli turuplatsilt või otse oma seadme müük, et sätete 3-NIC virtuaalse seadme.
    - **AZF2**. Sisemise tulemüüri juhitakse **azsn2** ja **azsn3**vahel. See on ka 3-NIC virtuaalse seadme.
    - **AZF3**. Juurdepääs asutusesisese andmekeskuse administraatorid ja ühendatud halduse alamvõrku, abil saate hallata kõik tulemüüri seadmed tulemüüri haldus. 2-NIC virtuaalse seadme Mallid leiate kuvatakse Marketplace'ist või taotleda ühte otse oma seadme tarnijaga.

## <a name="user-defined-routing-udr"></a>Kasutaja määratletud marsruutimise (UDR)

Iga alamvõrgu Azure saab linkida UDR tabeli abil saate määratleda, kuidas liiklust algatatud, et alamvõrgu marsruuditakse. Kui määratud pole UDRs, Azure kasutab vaikimisi marsruudib liikluse tulenevad ühe alamvõrgu teise. Külastage paremini mõistmiseks UDRs, [mis on kasutaja määratletud marsruudib ja IP edastamine](./virtual-networks-udr-overview.md#ip-forwarding).

Veenduge, et suhtlemine on teinud eespool viimase nõude õige tulemüüri seadme kaudu peate loomiseks, mis sisaldab UDRs **azurevnet**marsruutimiseks järgmises tabelis.

### <a name="azgwudr"></a>azgwudr

Selle stsenaariumi korral kasutatakse ainult liiklust tulenevad kohapealse Azure ühenduse **AZF3**tulemüürid haldamiseks ja et liiklus tuleb läbida sisemise tulemüüri, **AZF2**. Seetõttu tuleb ainult üks **GatewaySubnet** nagu allpool näidatud.

|Sihtkoht|Järgmise sõnumihüppe kohta, pärast|Selgitus|
|---|---|---|
|10.0.4.0/24|10.0.3.11|Lubab kohapealse liiklust halduse tulemüüri **AZF3** saavutamiseks|

### <a name="azsn2udr"></a>azsn2udr

|Sihtkoht|Järgmise sõnumihüppe kohta, pärast|Selgitus|
|---|---|---|
|10.0.3.0/24|10.0.2.11|Võimaldab liikluse taustväärtus alamvõrgu majutava serveri rakenduse kaudu **AZF2**|
|0.0.0.0/0|10.0.2.10|Kõik muud liiklust marsruutida läbi **AZF1** võimaldab|

### <a name="azsn3udr"></a>azsn3udr

|Sihtkoht|Järgmise sõnumihüppe kohta, pärast|Selgitus|
|---|---|---|
|10.0.2.0/24|10.0.3.10|Võimaldab liikluse **azsn2** serveri kaudu **AZF2** rakenduse serverist jätkata|

Samuti peate marsruutimiseks tabelite jaoks soovitud alamvõrku **onpremvnet** jäljendamiseks asutusesisese andmekeskuse loomine.

### <a name="onpremsn1udr"></a>onpremsn1udr

|Sihtkoht|Järgmise sõnumihüppe kohta, pärast|Selgitus|
|---|---|---|
|192.168.2.0/24|192.168.1.4|Võimaldab liikluse **onpremsn2** **OPFW** kaudu|

### <a name="onpremsn2udr"></a>onpremsn2udr

|Sihtkoht|Järgmise sõnumihüppe kohta, pärast|Selgitus|
|---|---|---|
|10.0.3.0/24|192.168.2.4|Võimaldab liikluse varundatud alamvõrgu Azure **OPFW** kaudu|
|192.168.1.0/24|192.168.2.4|Võimaldab liikluse **onpremsn1** **OPFW** kaudu|

## <a name="ip-forwarding"></a>IP ümbersuunamine 

UDR ja IP edastamine on funktsioonid, mida saate kasutada kombinatsiooni lubamiseks kasutatava kontrollivad liikluse on Azure VNet virtuaalne seadmed.  Virtuaalse seadme on midagi enamat kui VM, mis töötab rakendus, mida kasutatakse käsitlema võrguliiklust mingil viisil, nt tulemüüri või NAT seade.

Selle virtuaalse seadme VM peavad saama Sissetuleva liikluse, mis on adresseeritud ise. VM saada adresseeritud muude sihtkohtade liikluse lubamiseks peate lubama IP edastamine VM. See on mõni Azure säte, mitte säte Külastajate operatsioonisüsteemi. Oma virtuaalse seadme vajab käivitamiseks teatud tüüpi toime Sissetuleva liikluse ja tee see õigesti.

IP edastamise kohta lisateabe saamiseks külastage [mis on kasutaja määratletud marsruudib ja IP edastamine](./virtual-networks-udr-overview.md#ip-forwarding).

Näiteks Kujutage ette teil on mõni Azure vnet järgnevalt:

- Alamvõrgu **onpremsn1** sisaldab nimega **onpremvm1**VM.
- Alamvõrgu **onpremsn2** sisaldab nimega **onpremvm2**VM.
- Virtuaalse seadme, nimega **OPFW** on ühendatud **onpremsn1** ja **onpremsn2**.
- Kasutaja määratletud marsruudi ühendatud **onpremsn1** määrab, et kõik liikluse **onpremsn2** saadetakse **OPFW**.

Selles etapis kui **onpremvm1** proovib **onpremvm2**ühendust luua, kasutatakse selle UDR ja liikluse saadetakse **OPFW** nimega kuvatakse järgmise sõnumihüppe kohta, pärast. Pidage meeles, et tegelik paketi sihtkoht on on muudetud, on endiselt tekst **onpremvm2** on soovitud sihtkohta. 

Ilma IP edastamine **OPFW**lubatud, Azure virtuaalse võrgu loogika langeb pakette, kuna see lubab ainult et VM saata, kui selle VM IP-aadress on selle sihtkoha.

Koos IP suunamise, edastab Azure virtuaalse võrgu loogika paketid OPFW, muutmata selle algse Sihtaadress. **OPFW** tuleb toime paketid ja kindlaks teha, mis nendega teha.

Ülaltoodud stsenaariumi töötada, peate lubama IP saatmise NICs **OPFW**, **AZF1**, **AZF2**ja **AZF3** , mida kasutatakse marsruutimist (välja arvatud haldus alamvõrgu lingitud need kõik NICs). 

## <a name="firewall-rules"></a>Tulemüüri reeglid

Eespool kirjeldatud IP edastamine ainult tagab paketid saadetakse virtuaalne seadmed. Teie seade peab endiselt otsustada, mida teha need paketid. Ülaltoodud stsenaariumi peate oma seadmed järgmiste reeglite loomist:

### <a name="opfw"></a>OPFW

OPFW tähistab kohapealse seadmes, mis sisaldab järgmisi reegleid:

- **Marsruutimiseks**: tunneliga **ONPREMAZURE**kaudu saadetakse kogu liikluse 10.0.0.0/16 (**azurevnet**).
- **Poliitika**: luba kogu kahesuunalise liikluse **port2** ja **ONPREMAZURE**vahel.
 
### <a name="azf1"></a>AZF1

AZF1 tähistab Azure virtuaalse seadme, mis sisaldavad järgmisi reegleid:

- **Poliitika**: luba kogu kahesuunaline liiklus **port1** ja **port2**vahel.

### <a name="azf2"></a>AZF2

AZF2 tähistab Azure virtuaalse seadme, mis sisaldavad järgmisi reegleid:

- **Marsruutimiseks**: Azure'i gateway IP-aadress (st 10.0.0.1) kaudu **port1**saadetakse kogu liikluse 10.0.0.0/16 (**onpremvnet**).
- **Poliitika**: luba kogu kahesuunaline liiklus **port1** ja **port2**vahel.

## <a name="network-security-groups-nsgs"></a>Võrgu turberühmad (NSGs)

Selle stsenaariumi korral NSGs ei kasutata. Siiski võib NSGs iga alamvõrgu piirata sissetuleva ja väljamineva liikluse rakendada. Näiteks võite rakendada välise FW alamvõrgu NSG järgmised reeglid.

**Sissetuleva meili**

- Luba kogu TCP-liikluse Interneti kaudu mis tahes VM alamvõrgu 80 porti.
- Keela kõik muud liikluse Interneti kaudu.

**Väljamineva**
- Keela kõik liikluse Interneti-ühendus.

## <a name="high-level-steps"></a>Kõrge taseme juhised

Selle stsenaariumi juurutada, järgige kõrge alltoodud juhiseid.

1.  Azure'i tellimuse sisse logida.
2.  Kui soovite juurutada VNet, kohapealse võrgu jäljendamiseks, ette valmistada ressursse, mis on osa **ONPREMRG**.
3.  Ettevalmistamise ressursse, mis on osa **AZURERG**.
4.  Sätte tunneliga **onpremvnet** kaudu, et **azurevnet**.
5.  Kui kõik ressursid on ette valmistatud, **onpremvm2** sisse logida ja ping 10.0.3.101 **onpremsn2** ja **azsn3**vahelise ühenduvuse testimiseks.
