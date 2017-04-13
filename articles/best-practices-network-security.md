<properties
   pageTitle="Microsofti pilveteenuste teenuste ja võrgu turvalisus | Microsoft Azure'i"
   description="Siit saate teada, mõned võtme funktsioonid saadaval Azure abil saate luua turvalist võrgu keskkonnas"
   services="virtual-network"
   documentationCenter="na"
   authors="tracsman"
   manager="rossort"
   editor=""/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/14/2016"
   ms.author="jonor;sivae"/>

# <a name="microsoft-cloud-services-and-network-security"></a>Microsofti pilveteenuste teenuste ja võrgu turvalisus

Microsofti pilveteenustega pakkuda hyperscale teenuste ja taristu, äriklassi võimalused ja palju valikuid hübriid Ühenduvus. Kliendid saate valida, kas Interneti kaudu või Azure'i ExpressRoute, mis pakub privaatse võrguühendus nende teenuste kasutamiseks. Microsoft Azure'i platvormi võimaldab sujuvalt laiendamine nende taristu pilve ja koostada Mitmejärguline arhitektuur. Lisaks saate tootjatele lubada täiustatud võimalused pakuvad turvalisust ja virtuaalne seadmed. See lühiülevaade antakse ülevaade turbe- ja arhitektuuri probleemid, mis kliendid peavad arvesse võtma, kui kasutate Microsofti pilveteenustega ExpressRoute kaudu. See hõlmab ka Azure'i turvalisemad teenuste loomine.

## <a name="fast-start"></a>Kiire alustamine
Järgmised loogika diagrammi saate suunab teid konkreetse näite palju turvalisus tehnika Azure'i platvormil saadaval. Kiirülevaate, otsige üles näide, mis sobib kõige paremini teie puhul. Täielikuma selgitused, jätkake lugemist paber.
![Turvalisus suvandid vooskeem][0]

[Näide 1: Koostada perimeetri võrgu (tuntud ka kui DMZ, demilitariseeritud tsoon ja läbivaadatud alamvõrgu) kaitsmiseks rakenduste võrgu turberühmad (NSGs).](#example-1-build-a-simple-dmz-with-nsgs)</br>
[Näide 2: Koostada perimeetri võrgu rakendused, tulemüüri ja NSGs kaitsmiseks.](#example-2-build-a-dmz-to-protect-applications-with-a-firewall-and-nsgs)</br>
[Näide 3: Koostada perimeetri võrgu tulemüüri, kasutaja määratletud marsruutimiseks (UDR) ja NSG võrkude kaitsmiseks.](#example-3-build-a-dmz-to-protect-networks-with-a-firewall-udr-and-nsg)</br>
[Näide 4: Lisage-saidilt, virtuaalse seadme virtuaalse privaatvõrgu (VPN) ühenduse hübriid.](#example-4-adding-a-hybrid-connection-with-a-site-to-site-virtual-appliance-vpn)</br>
[Näide 5: Lisada VPN-saidilt, Azure lüüsi ühenduse hübriid.](#example-5-adding-a-hybrid-connection-with-a-site-to-site-azure-gateway-vpn)</br>
[Näide 6: Lisada ExpressRoute ühenduse hübriid.](#example-6-adding-a-hybrid-connection-with-expressroute)</br>
Näited lisamise seoseid virtuaalne võrkude, kõrge-saadavus ja teenuse Aheldamise lisatakse mõne järgmise kuu jooksul selles dokumendis.

## <a name="microsoft-compliance-and-infrastructure-protection"></a>Microsofti nõuetele vastavus ja taristu kaitse
Microsoft on võtnud liidripositsioon paigutus, mis toetavad vastavuse algatused nõutud ettevõtte kliendid. Järgmiselt on toodud mõned Azure vastavuse kinnitamine: ![Azure'i vastavuse märgid][1]

Lisateavet leiate [Microsoft Usalduskeskus](https://azure.microsoft.com/support/trust-center/compliance/)vastavuse teavet vaadata:

Microsoft on terviklik käivitamiseks hyperscale globaalne teenuste pilvetaristu kaitsta. Microsofti pilvetaristu sisaldab riistvara, tarkvara, võrkude ja haldus ja toimingute töötajad lisaks füüsilise andmekeskuste.

![Azure'i turbefunktsioonid][2]

Seda moodust pakub turvalisemaks aluse juurutamiseks teenuseid Microsofti pilveteenuse klientidele. Järgmiseks on mõeldud klientidele, kujundamine ja luua turbearhitektuur nende teenuste kaitsmiseks.

## <a name="traditional-security-architectures-and-perimeter-networks"></a>Traditsiooniline turvalisuse arhitektuur ja perimeetri võrkude
Kuigi tugevalt investeerib Microsofti pilveteenuste kaitsmine, peab kliendid kaitsta oma pilveteenustega ja ressursside rühmad. Lähenemisviisi mitmekihilise turvalisuse pakub parim kaitse. Perimeetri võrgu turvalisus zone kaitseb ressursid sisemise ebausaldusväärsete võrgu. Võrgu perimeetri viitab servad või võrgu osad, et istuda Interneti- ja kaitstud ettevõtte vahel IT taristu.

Tüüpilised ettevõtte võrgus, core taristu on tugevalt analüüsitavad piirid, mitme kihid seadmeid. Kihiti äärist koosneb seadmed ja poliitika jõustamine punkte. Seadmete hõlmama järgmist: tulemüürid, jaotatud eitamine, Service (DDoS) vältimine, sissetungi tuvastamise või süsteemide (ID ja IP-d) ja VPN seadmed. Teabehalduspoliitika rakendamine võivad olla tulemüüri poliitika, pääsuloendid (ACL) või teatud marsruutimist. Esimese rea kaitsevallid võrgus, otse vastu võtmist liiklust Internet, on need võimaldada Blokeeri eest ja kahjulike liikluse kombinatsioon lubades õigustatud taotlusi täpsemaks võrku. See liikumisteede otse seostuvatele ressurssidele perimeetri võrku. Selle ressursi võib seejärel "rääkida" ressursid süvitsi võrgus, läbivad järgmise äärist valideerimisreeglite esimese. Välimine nimetatakse võrgu perimeetri, kuna see osa võrgu on avatud Interneti, tavaliselt mingi kaitse mõlemal. Järgmisel joonisel kujutatud ühe alamvõrgu perimeetri võrgu ettevõtte võrgus, kaks turvalisus piirid.

![Ettevõtte võrgus perimeetri võrgus][3]

On mitme arhitektuurides rakendada perimeetri võrgus, kaudu lihtne laadi koormusetasakaalustusteenuse ette web pargis mitme alamvõrgu perimeetri võrku, millel on erinevad iga piiri blokeerida liikluse ja kaitsmiseks süvitsi kihid ettevõtte võrgus. Kuidas perimeetri võrk on ehitatud sõltub erivajadustega organisatsiooni ja selle üldine risk hälbe.

Kui kliendid saavad töökoormus teisaldamiseks avaliku pilved, on oluline toetada sarnaseid funktsioone perimeetri võrgu arhitektuur Azure nõuetele vastavus ja turvalisus, nõuetele. Selle dokumendi leiate juhised, kuidas kliendid saate koostada turvaline keskkonnast Azure kohta. See keskendub võrgu perimeetri, kuid sisaldab ka põhjalik arutelu mitme võrgu turvalisus. Järgmised teavitavad selle arutelu:

- Kuidas saab koostada perimeetri võrgu Azure?
- Mis on Azure funktsioone perimeetri võrgu koostada?
- Kuidas tagaandmebaas töökoormus kaitstud?
- Kuidas on Interneti-side kontrollitud töökoormus Azure?
- Kuidas saab kohapealse võrgu kaudu juurutuste Azure kaitstud?
- Millal tuleks kohalikke Azure turbefunktsioonid võrreldes muude tootjate seadmete või teenuseid kasutada?

Järgmisel joonisel on esitatud kihtide väärtpaberi Azure pakub klientidele. Need kihid on nii native Azure'i platvormi ise ja klientide määratletud funktsioonid:

![Azure'i turbearhitektuur][4]

Sissetulev Internetist, Azure'i DDoS aitab kaitsta suuremahuliste eest Azure'i vastu. Järgmise kiht on kliendi määratletud avaliku lõpp-punktid, mida kasutatakse määramiseks, mis läbivad pilveteenusesse virtuaalse võrku. Kohalikke Azure virtuaalse võrgu eraldamise tagab kõigi muude võrkude väliskontaktidega täiesti eraldi ja, et kasutaja, teed ja meetodite liikluse ainult juhtimine. Need teed ja meetodid on järgmine kiht, kus NSGs, UDR ja võrgu virtuaalse seadmete saab luua turvalisus piirmäärad kaitsmiseks rakenduse kasutuselevõttu kaitstud võrku.

Järgmises jaotises antakse ülevaade Azure'i. Nende virtuaalne võrkude on loodud kliendid ja on, mida nende juurutatud töökoormus on ühendatud. Kõigi võrgu turvalisuse funktsioonid nõutav perimeetri võrgu kaitsmiseks kliendi juurutuste Azure aluseks olevad virtuaalse võrgu.

## <a name="overview-of-azure-virtual-networks"></a>Azure'i ülevaade
Enne Interneti-liikluse pääsevad Azure virtuaalse võrgu, on kaks kihti turvalisus Azure'i platvormi juurde.

1.  **DDoS kaitse**: DDoS kaitse on kiht Azure füüsilise võrgu, mida kaitseb Azure platvormile suuremahuliste Interneti-põhiste eest. Nende eest kasutada mitme "Robot" sõlmed katse uputama Interneti-teenuse. Azure'i on robustne DDoS kaitse võrk, kõigi sissetulevate Interneti-ühendus. See DDoS kaitse kiht on pole kasutaja konfigureeritav atribuudid ja see pole saadaval kliendile. See kaitseb Azure'i platvormi suuremahuliste eest, kuid ei kaitse otse kliendi rakendus. Täiendavad kihid paindlikkuse saab konfigureerida lokaliseeritud rünnakuvastane kliendi poolt. Kui kliendi A rünnati koos suuremahuliste DDoS rünnak avalike lõpp-punkti, blokeerib Azure'i teenuse ühendused. Klientide A võib nurjuda üle teise virtuaalse võrgu või teenuse lõpp-punkti rünnak taastamine teenus pole kaasatud. Tuleb märkida, et kuigi A klient võib mõjutada selle lõpp-punkti kohta, muude teenuste väljaspool selle lõpp-punkti mõjutaks. Lisaks teiste klientide ja teenuste näeksid selle rünnaku ei mõjuta.
2.  **Teenuse lõpp-punktid**: lõpp-punktid luba cloud services või ressursside rühmad on avaliku Interneti IP-aadressid ja avatud pordid. Lõpp-punkti kasutatakse Address Translation (NAT) abil saate marsruutida liiklust sisemine aadress ja pordi Azure virtuaalse võrgus. See on esmane tee välise liiklus virtuaalse võrku edasi. Teenuse lõpp-punktid on kasutaja konfigureeritav määratlemaks, mis on möödunud, ja kuidas ja kus see on tõlgitud virtuaalse võrgus.

Kui liikluse jõuab virtuaalse võrgu, on mitmeid funktsioone, mis tulevad mängu. Azure'i on fondi manustamine nende töökoormus ja kus võrgu taseme turvalisuse kehtib klientide. Azure'i privaatvõrk (virtuaalne võrgu ülekate) on klientidele omadusi ja järgmised funktsioonid:
 
- **Liikluse eraldamise**: virtuaalse võrk on liikluse eraldamise piiri Azure'i platvormil. Ühe virtuaalse võrgu virtuaalmasinates (VMs) ei saa suhelda otse VMs eri virtuaalse võrku, isegi siis, kui mõlemad virtuaalne võrkude luuakse sama kliendi poolt. See on kriitilist atribuut, mis tagab kliendi VMs ja suhtlus jääb virtuaalse võrgustikus privaatne.
- **Multitier topoloogia**: virtuaalne võrkude luba klientide määratlemine multitier topoloogia eraldamise alamvõrku ja määratakse eraldi aadressi tühikuid erinevate elementide või "astme", nende töökoormus. Need loogiline rühmitustega topoloogiatest võimaldada määratleda eri juurdepääsu poliitika põhinevate töökoormus ja määrata ka liikluse puhul kasutatakse vahel.
- **Asutusesiseses Ühenduvus**: kliendid saate luua asukohaga virtuaalse võrgu ja mitme kohapealse saitidel või muude virtuaalse võrgu Ühenduvus Azure. Selle tegemiseks kliendid saate kasutada Azure VPN lüüsid või muude tootjate võrgu virtuaalne seadmed. Azure'i toetab saidi saidi (S2S) VPN standard IPsec/IKE protokollid ja ExpressRoute privaatne ühenduvuse abil.
- **NSG** võimaldab koostada reegleid (ACL) granulaarsus soovitud tasemel: network liideste, üksikute VMs või virtuaalse alamvõrku. Kliente saate reguleerida juurdepääsu lubamine või keelamisega seotud töökoormus virtuaalse võrgustikus Systemsi kliendi võrkudes asutusesiseses Ühenduvus kaudu suhtlemine või otsene Interneti-side.
- **UDR** ja **IP edastamine** võimaldab määratleda side teed erinevatele tasemetele virtuaalse võrgustikus kliendid. Kliendid saate juurutada tulemüüri, ID ja IP-d ja muud virtuaalse seadmed ja marsruutimine võrguliikluse kaudu nende turvalisus seadmete piiri turbepoliitika jõustamine, ja kontrollimise jaoks.
- Azure'i turuplatsi **võrgu virtuaalne seadmed** : turvalisus seadmed, nt tulemüürid, koormus soolise ja ID ja IP-d on saadaval Azure'i turuplatsi ja Pildigalerii VM. Klientide saate juurutada multitiered turvaline keskkonnast lõpuleviimiseks neid seadmeid oma virtuaalse võrkude, ja täpsemalt oma turvalisus piirid (sh perimeetri alamvõrku).

Neid funktsioone ja võimalusi, kuidas perimeetri võrgu arhitektuur võib olla ehitatud Azure'i üks näide on järgmine:

![Azure virtuaalse võrgu perimeetri võrgus][5]

## <a name="perimeter-network-characteristics-and-requirements"></a>Perimeetri võrgu omadused ja nõuded
Võrgu perimeetri on mõeldud esiosa võrgu, otse liides side Interneti kaudu. Sissetulevate pakettide tuleks enne tagaandmebaas serverid – turvalisus seadmed, nt tulemüüri, ID ja IP-d, meilivoo. Internet seotud paketid on teenustest saate ka flow – turvalisus seadmete perimeetri võrku teabehalduspoliitika rakendamine, kontroll ja Auditeerimispoliitika eesmärgil, lahkudes võrgu. Lisaks saate perimeetri võrgu majutada asutusesiseses VPN lüüside kliendi virtuaalse võrkude ja kohapealse võrgu vahel.

### <a name="perimeter-network-characteristics"></a>Perimeetri võrgu omadused
Mõned hea perimeetri võrgu omaduste viitamine eelmisel joonisel, on järgmised:

- Interneti-ühendusega:
    - Perimeetri alamvõrku, ise on Interneti-ühendusega otse suhtlemine Interneti-ühendus.
    - Avaliku IP-d, VIP ja/või teenuse lõpp-punktid edastada Interneti-liikluse ees võrgu- ja seadmed.
    - Interneti kaudu Sissetuleva liikluse läbib seadmeid enne muud ressursid ees võrgus.
    - Kui Väljamineva meili Turve on lubatud, liikluse läbib seadmeid, sammuna lõplik enne läbimise Interneti-ühendus.
- Kaitstud võrgu:
    - On otsene tee Interneti kaudu core infrastruktuuri.
    - Kanalite core taristu tuleb läbida seadmeid nagu NSGs, tulemüürid või VPN seadmete kaudu.
    - Muud seadmed peab ületada pole Interneti- ja core taristu.
    - Interneti-ühendusega ja vastastikuste piirmäärad perimeetri võrgu (nt kaks tulemüüri ikoonid eelmisel joonisel) kaitstud võrgu seadmeid võib tegelikult ühe virtuaalse seadme liigendatud reeglite või liideste jaoks iga äärist. (St ühe seadme loogiliselt eraldatud, töötlemise laadi nii piirmäärad perimeetri võrgu jaoks.)
- Muud levinud tavade ja piiranguid.
    - Töökoormus peab pole äriteabe kriitiline.
    - Juurdepääs ja värskendustega perimeetri võrgu konfiguratsiooni ja juurutamise jaoks on piiratud ainult volitatud administraatorid.

### <a name="perimeter-network-requirements"></a>Perimeetri võrgu nõuded
Järgige neid juhiseid nende näitajate lubamiseks rakendada eduka perimeetri võrgu virtuaalse võrgu nõuete kohta.

- **Alamvõrgu arhitektuur:** Määrake virtuaalse võrgu nii, et ka kogu alamvõrgu eesmärk on jätkuvalt sama perimeetri, eraldatud muude alamvõrku virtuaalse samasse võrku. See tagab liikluse perimeetri võrgu ja muude sise- või alamvõrgu astme puhul läbi tulemüüri või virtuaalse seadme ID ja IP alamvõrgu piirmäärad kasutaja määratletud protsesside vahel.
- **NSG:** Perimeetri alamvõrku, ise peaks olema avatud Interneti-suhtluse lubamine, kuid see ei tähenda, et kliendid peaks mööda NSGs. Järgige turvalisus tavad võrgu pinna Internetis minimeerimiseks. Serveri aadress vahemikud lubatud juurdepääs kasutuselevõttu või konkreetse rakenduse protokollid ja avatud pordid lukustada. Võib olla olukordades, kus see pole alati võimalik. Näiteks kui kliendid on Azure välisele veebisaidile, perimeetri võrgu tuleks lubada sissetulevate web nõuab mis tahes avaliku IP-aadressid, kuid peaksite avama ainult web rakenduse pordid: TCP:80 ja TCP:443.
- **Marsruutimine tabel:** Perimeetri alamvõrku, ise peaks oskama suhelda otse Interneti-ühendus, kuid tuleks lubada otsest side ja sealt tagasi end-klahv või kohapealse võrgu ilma tulemüüri või turberühma seadme.
- **Turvalisuse seadme konfiguratsiooni:** Marsruutimine ja kontrolli paketid perimeetri võrgu ja ülejäänud kaitstud võrke, Turve seadmed, nt tulemüüri, ID ja IP-d vahel võib seadmete mitme kasutajaid. Neil võib olla eraldi NICs perimeetri võrgu-ja tagaandmebaas alamvõrku. NICs perimeetri võrgus suhelda otse ja Internet, koos vastavate NSGs ja perimeetri võrgu marsruutimise tabel. Ühenduse tagaandmebaas alamvõrku NICs on piiratud NSGs ja vastavate tagaandmebaas alamvõrku marsruutimise tabeleid.
- **Turvalisuse seadme funktsioonid:** Turvalisus seadmete juurutanud perimeetri võrgus tavaliselt teha järgmised funktsioonid:
    - Tulemüüri: Jõustamine tulemüüri reeglid või juurdepääsupoliitikaid juhtelemendi sissetulevad taotlused.
    - Automaattuvastus ja vältimise: tuvastamine ja leevendada ründetarkvara eest Interneti kaudu.
    - Auditi- ja logimine: üksikasjalik logid auditeerimine ja analüüsi haldamine.
    - Puhverserveri tühistada: ümbersuunamine sissetulevad taotlused vastavate tagaandmebaas serverid. See hõlmab vastenduse ja tõlkimiseks sihtkoha aadressid ees seadmetes, tavaliselt tulemüürid tagaandmebaas serveri aadressi.
    - Puhverserveri edasi: esitada NAT ja läbimiseks auditeerimine edastamiseks Interneti-ühendus virtuaalse võrgustikus algatada.
    - Ruuteri: Ümbersuunamine sissetulevate ja rist-alamvõrgu liikluse sees virtuaalse võrgus.
    - VPN seadme: abistas asutusesiseses VPN lüüside asutusesiseses VPN Ühenduvus kliendi kohapealse võrgu ja Azure'i jaoks.
    - VPN-serveri: VPN kliendid Microsoft Azure'i vastu võtmist.

>[AZURE.TIP] Järgmised kaks rühmad eraldi hoida: inimestele juurdepääs perimeetri võrgu turvalisus hammasratta ja üksikisikute lubatud rakenduste arendamise, juurutamise või toimingute administraatorid. Nende rühmade eraldi hoida võimaldab eraldamine ülesannete ja üks isik takistab nii rakenduste Turve ja võrgu turbemeetmed.

### <a name="questions-to-be-asked-when-building-network-boundaries"></a>Esitatavad küsimused kui hoone võrgu piirmäärad
Selles jaotises mainitud, termin "võrgud" viitab Azure virtuaalse võrgud tellimuse administraatori loodud. Termini ei viita aluseks füüsilise võrgustikke Azure'is.

Lisaks Azure'i kasutatakse sageli traditsiooniline kohapealse võrgu laiendada. On võimalik lisada saidilt või ExpressRoute hübriid võrgu lahenduste perimeetri võrgu arhitektuur. See on oluline hoone võrgu turvalisus piirid.

Vastata, kui olete hoone võrgu perimeetri võrgu ja mitme turvalisus piirmäärad on kolm järgmistele küsimustele.

#### <a name="1-how-many-boundaries-are-needed"></a>1) mitu piirmäärad on vaja?
Esimene otsus on otsustada mitu turvalisus piirmäärad on vaja antud stsenaariumi:

- Ühe piiri: üks ees perimeetri võrgus, virtuaalse võrgu ja Interneti vahel.
- Kahe piirmäärad: ühte Interneti servas perimeetri võrgu ja teise alamvõrku perimeetri ja tagaandmebaas alamvõrku Azure virtuaalse võrkudes vahel.
- Kolm piirmäärad: üks servas Internet võrgu perimeetri, üks vahel perimeetri võrgu- ja tagaandmebaas alamvõrku ja üks tagaandmebaas alamvõrku ja kohapealse võrgu vahel.
- N piirmäärad: hulk. Sõltuvalt turbenõuetele, on väga suvalist arvu turvalisus piirmäärad, mida saate rakendada võrku.

Arv ja tüüp piiride vaja sõltub ettevõtte risk hälbe ja teatud stsenaarium, mis rakendatakse. See on sageli ühine otsus tehtud mitu rühma ettevõttes sageli sh riski ja nõuetele vastavus meeskonnatöö, võrgus ja platvormi meeskonnatöö ja soovitud rakenduse arendusmeeskonnale. Turvalisus, seotud andmed ja kasutatava tehnoloogia tundmine inimestele peaks olema seda otsust ja veenduge, et iga rakendamiseks vastavat turvalisus kurssi öelda.

>[AZURE.TIP] Vähim arv piirmäärad turvalisus nõuetele olukorra jaoks kasutada. Lisateavet piirid raskem toimingute ja tõrkeotsingu saab samuti halduse pea kohal kaasatud mitme piiri poliitikate haldamine aja jooksul. Siiski piisavalt piirmäärad suurendada riski. Äärmiselt oluline on leida saldo.

![Hübriid võrku kolme turvalisus piirmäärad][6]

Eelmisel joonisel kolm turvalisuse piiri võrgu ületaseme vaadet. Piirid on perimeetri võrgu ja Interneti-ühendus, Azure ees- ja isiklike alamvõrku ja Azure tagaandmebaas alamvõrgu ja kohapealse ettevõtte võrgus.

#### <a name="2-where-are-the-boundaries-located"></a>(2) piirid asukohta?
Kui arvu piirmäärad on otsustanud, kus neid rakendada on järgmine otsust. Tavaliselt on kolm valikut.
- Interneti-põhiste vahendaja teenust (näiteks pilvepõhist Web rakenduse tulemüüri, mis pole käsitletakse seda dokumenti) abil
- Azure'i kohalikke funktsioonid ja/või võrgu virtuaalse seadmete kasutamine
- Kohapealse võrgu füüsilise seadmete kasutamine

Puhtalt Azure võrkudes Valikud on kohalikke Azure funktsioonid (nt Azure koormus soolise) või võrgu virtuaalse seadmete rikkaliku partnerite ökosüsteem Azure (nt sisse punkti tulemüürid).

Azure'i ja mõne kohapealse võrgu vahel piiri vajadusel turvalisus seadmete võib asuda mõlemal pool ühenduse (või mõlemal pool). Seetõttu tuleb asukoha turvalisus hammasratta viia otsust.

Eelmisel joonisel Interneti-perimeetri võrgu ees-back-end piirmäärad täielikult sisaldub Azure'is ja peab olema kohalikke Azure funktsioone või võrgu virtuaalne seadmed. Azure'i (tagaandmebaas alamvõrgu) ja ettevõtte võrku sisse seadmeid võib olla Azure küljel või kohapealse küljel või isegi kombinatsiooni seadmete mõlemalt. Ei saa olla märkimisväärsed eelised ja puudused kas suvand, mis tuleb arvestada oluliselt.

Näiteks abil olemasoleva füüsilise turvalisuse hammasratta kohapealse võrgu küljel on see eelis, et pole uue hammasratta on vaja. See lihtsalt peab ümberkonfigureerimist. Kahjuks, aga, et kõik liikluse peab ta tagasi tuleb Azure kohapealse võrgu turvalisus hammasratta näinud. Seega Azure'i Azure liikluse võib põhjustada märgatavat latentsus ja jõudlust mõjutavad rakenduse ja kasutajale võimalusi, kui see on taas kohapealse võrgu jaoks turbepoliitika jõustamine.

#### <a name="3-how-are-the-boundaries-implemented"></a>3) kuidas rakendatakse piirid?
Iga turvalisus piiri tõenäoliselt eri võimalus nõuete (nt ID-d ja tulemüüri reeglid Internet servas perimeetri võrgus, kuid ainult ACL perimeetri võrgu ja tagaandmebaas alamvõrgu vahel). Otsustada, millised seadmed kasutada sõltub stsenaarium ja turvalisus. Järgmises jaotises näited 1, 2 ja 3 käsitletakse ka võimalusi, mida võib kasutada. Azure'i kohalikke võrgu funktsioonide ja seadmete saadaval Azure partnerite ökosüsteem läbivaatamise kuvatakse hulgaliselt võimalusi lahendada praktiliselt kõigist stsenaariumi.

Mõne muu rakendamise otsust punkti teada, kuidas Azure'i kohapealse võrgu suhtlemiseks. Kui te kasutate Azure virtuaalse lüüsi või võrgu virtuaalse seadme? Need suvandid käsitletakse lähemalt järgmises jaotises (näited 4, 5 ja 6).

Lisaks virtuaalne võrkude Azure'is vahel võib olla vajalik. Need stsenaariumid lisatakse hiljem.

Kui teate, et vastused küsimustele, [Kiire alustamine](#fast-start) jaotises aitab tuvastada, millised on kõige kohasem antud stsenaarium.

## <a name="examples-building-security-boundaries-with-azure-virtual-networks"></a>Näited: Koosteüksuse turvalisus piirmäärad Azure'i abil
### <a name="example-1-build-a-perimeter-network-to-help-protect-applications-with-nsgs"></a>Näide 1: Koostada perimeetri võrgu rakenduste NSGs kaitsmiseks
[Tagasi, et kiiresti alustada](#fast-start) | [koostada üksikasjalikud juhised selle näite puhul][Example1]

![Sissetulev perimeetri võrgu ja NSG][7]

#### <a name="environment-description"></a>Keskkonna kirjeldus
Selles näites on tellimus, mis sisaldab järgmist:

- Kaks pilveteenused: "FrontEnd001" ja "BackEnd001"
- Virtuaalse võrk, "CorpNetwork" koos kahe alamvõrku: "FrontEnd" ja "Taustväärtus"
- Mis on rakendatud nii alamvõrku võrgu turberühma
- Windows server, mis tähistab rakenduse veebiserverisse ("IIS01")
- Kaks Windows serverid, mis tähistab rakenduse tagaandmebaas serverid ("AppVM01", "AppVM02")
- Windows server, mis tähistab DNS server ("DNS01")

Skripte ja Azure ressursihaldur malli, leiate [üksikasjaliku Koosta juhiseid][Example1].

#### <a name="nsg-description"></a>NSG kirjeldus
Selles näites on ka NSG rühma ehitatud ja seejärel koormatud kuus reeglid.

>[AZURE.TIP] Üldiselt, tuleb teil luua teatud "Luba" reeglid esmalt rohkem üldine "Keela" reeglid. Antud prioriteet ütleb, millised reeglid hinnatakse esmalt. Kui liikluse on leitud kindla reegli rakendamiseks, väärtustatakse pole veel reeglid. Kas sissetulevaid või väljaminevaid suunas (alamvõrgu seisukohast) saate rakendada NSG reeglid.

Deklaratiivse, ehitatakse sissetulev liiklus järgmisi reegleid:

1.  Sisemise DNS-i liikluse (port 53) on lubatud.
2.  RDP-liikluse (port 3389) Interneti kaudu mis tahes virtuaalse masina on lubatud.
3.  HTTP-liikluse (port 80) Interneti kaudu veebiserverisse (IIS01) on lubatud.
4.  Mis tahes liikluse (kõik pordid) kaudu IIS01 AppVM1 on lubatud.
5.  Mis tahes liikluse (kõik pordid) Interneti kaudu kogu virtuaalse võrku (nii alamvõrku) on keelatud.
6.  Mis tahes liiklus (kõik pordid) ees alamvõrgu tagaandmebaas alamvõrgu on keelatud.

Reegleid kindlasti iga alamvõrku, kui HTTP-päring on sissetuleva Interneti kaudu veebiserverisse, nii reeglite 3 (luba) ja 5 (keelamiseks) tuleks rakendada. Kuid kuna reegli 3 on kõrgem prioriteet, ainult seda ja reegli 5 ei tule mängu. Seega veebiserverisse lubatud HTTP-päring. Kui see sama liikluse üritab DNS01 serverisse, artikkel 5 (keelamiseks) on esimene rakendamiseks ja liiklus oleks lubatud edastamiseks server. Reegli 6 (keelamiseks) blokeerib ees alamvõrgu räägib tagaandmebaas alamvõrgu (v.a lubatud liikluse reeglite 1 – 4). See kaitseb tagaandmebaas võrgu juhuks, kui ründaja kompromisse veebirakenduse kohta esiosa. Ründaja oleks piiratud juurdepääsu tagaandmebaas "kaitstud" (ainult seostuvatele ressurssidele esitatud AppVM01 serveris).

On vaikimisi Väljaminev reegel, mis lubab liikluse välja Interneti-ühendus. Selle näite puhul me lubamisel väljaminev liiklus ja pole muutmine mis tahes väljaminev reeglid. Lukustada liikluse mõlemast suunast, kasutaja määratletud marsruutimine on nõutav (vt näide 3).

#### <a name="conclusion"></a>Kokkuvõte
See on üsna lihtne ja lihtne viis tagaandmebaas alamvõrgu sissetulev liiklus eraldamiseks. Lisateabe saamiseks lugege [üksikasjalikku Koosta juhiseid][Example1]. Need juhised on järgmised.

- Kuidas koostada perimeetri võrku PowerShelli skriptide abil.
- Kuidas koostada perimeetri võrku on Azure ressursihaldur malli abil.
- Üksikasjalikku kirjeldust iga NSG käsu.
- Üksikasjalik liikluse kulgemist stsenaariumid, kuidas liikluse lubatud või keelatud iga kiht.


 ### <a name="example-2-build-a-perimeter-network-to-help-protect-applications-with-a-firewall-and-nsgs"></a>Näide 2: Koostada perimeetri võrgu rakendused, tulemüüri ja NSGs kaitsmiseks
[Kiire algus naasmiseks](#fast-start) | [koostada üksikasjalikud juhised selle näite puhul][Example2]

![Sissetulev perimeetri võrgu Dessandi ja NSG][8]

#### <a name="environment-description"></a>Keskkonna kirjeldus
Selles näites on tellimus, mis sisaldab järgmist:

- Kaks pilveteenused: "FrontEnd001" ja "BackEnd001"
- Virtuaalse võrk, "CorpNetwork" koos kahe alamvõrku: "FrontEnd" ja "Taustväärtus"
- Mis on rakendatud nii alamvõrku võrgu turberühma
- Võrgu virtuaalse seadme sel juhul tulemüüri, on ühendatud ees alamvõrgu
- Windows server, mis tähistab rakenduse veebiserverisse ("IIS01")
- Kaks Windows serverid, mis tähistab rakenduse tagaandmebaas serverid ("AppVM01", "AppVM02")
- Windows server, mis tähistab DNS server ("DNS01")

Skripte ja Azure ressursihaldur malli, leiate [üksikasjaliku Koosta juhiseid][Example2].

#### <a name="nsg-description"></a>NSG kirjeldus
Selles näites on ka NSG rühma ehitatud ja seejärel koormatud kuus reeglid.

>[AZURE.TIP] Üldiselt, tuleb teil luua teatud "Luba" reeglid esmalt rohkem üldine "Keela" reeglid. Antud prioriteet ütleb, millised reeglid hinnatakse esmalt. Kui liikluse on leitud kindla reegli rakendamiseks, väärtustatakse pole veel reeglid. Kas sissetulevaid või väljaminevaid suunas (alamvõrgu seisukohast) saate rakendada NSG reeglid.

Deklaratiivse, ehitatakse sissetulev liiklus järgmisi reegleid:

1.  Sisemise DNS-i liikluse (port 53) on lubatud.
2.  RDP-liikluse (port 3389) Interneti kaudu mis tahes virtuaalse masina on lubatud.
3.  Mis tahes Interneti-liikluse (kõik pordid) võrgu virtuaalse seadmele (tulemüür) on lubatud.
4.  Mis tahes liikluse (kõik pordid) kaudu IIS01 AppVM1 on lubatud.
5.  Mis tahes liikluse (kõik pordid) Interneti kaudu kogu virtuaalse võrku (nii alamvõrku) on keelatud.
6.  Mis tahes liiklus (kõik pordid) ees alamvõrgu tagaandmebaas alamvõrgu on keelatud.

Reegleid kindlasti iga alamvõrku, kui HTTP-päring on sissetuleva meili Internetist tulemüüri, nii reeglite 3 (luba) ja 5 (keelamiseks) tuleks rakendada. Kuid kuna reegli 3 on kõrgem prioriteet, ainult seda ja reegli 5 ei tule mängu. Seega HTTP-päringu lubatakse tulemüüri. Kui selle sama liikluse üritab IIS01 serverisse, isegi kui see on ees alamvõrgu, artikkel 5 (keelamiseks) soovite rakendada, ja liiklus oleks lubatud edastamiseks server. Reegli 6 (keelamiseks) blokeerib ees alamvõrgu räägib tagaandmebaas alamvõrgu (v.a lubatud liikluse reeglite 1 – 4). See kaitseb tagaandmebaas võrgu juhuks, kui ründaja kompromisse veebirakenduse kohta esiosa. Ründaja oleks piiratud juurdepääsu tagaandmebaas "kaitstud" (ainult seostuvatele ressurssidele esitatud AppVM01 serveris).

On vaikimisi Väljaminev reegel, mis lubab liikluse välja Interneti-ühendus. Selle näite puhul me lubamisel väljaminev liiklus ja pole muutmine mis tahes väljaminev reeglid. Lukustada liikluse mõlemast suunast, kasutaja määratletud marsruutimine on nõutav (vt näide 3).

#### <a name="firewall-rule-description"></a>Tulemüüri reegli kirjeldus
Tulemüüri, tuleks luua edasisaatmise reegleid. Kuna see näide ainult marsruudib Interneti-liikluse sisse seotud Firewall ja seejärel veebiserverisse, ainult üks ümbersuunamine network aadress tõlge (NAT) reegel on vaja.

Selle reegli aktsepteerib mis tahes sissetuleva allika aadress, mis tabab tulemüüri jõua HTTP (https port 80 ja 443). See on tulemüüri kohaliku kasutajaliidese välja saadetud ja ümber veebiserver 10.0.1.5 IP-aadressiga.

#### <a name="conclusion"></a>Kokkuvõte
See on üsna lihtne viis rakenduse tulemüüri kaitsmine ja tagaandmebaas alamvõrgu sissetulev liiklus eraldamiseks. Lisateabe saamiseks lugege [üksikasjalikku Koosta juhiseid][Example2]. Need juhised on järgmised.

- Kuidas koostada perimeetri võrku PowerShelli skriptide abil.
- Kuidas koostada selles näites on Azure ressursihaldur malli abil.
- Üksikasjalikku kirjeldust iga NSG käsu ja tulemüüri reegel.
- Üksikasjalik liikluse kulgemist stsenaariumid, kuidas liikluse lubatud või keelatud iga kiht.

### <a name="example-3-build-a-perimeter-network-to-help-protect-networks-with-a-firewall-udr-and-nsg"></a>Näide 3: Koostada perimeetri võrgu tulemüüri, UDR ja NSG võrkude kaitsmiseks
[Kiire algus naasmiseks](#fast-start) | [koostada üksikasjalikud juhised selle näite puhul][Example3]

![Kahesuunalise perimeetri võrgu Dessandi, NSG ja UDR][9]

#### <a name="environment-description"></a>Keskkonna kirjeldus
Selles näites on tellimus, mis sisaldab järgmist:

- Kolm pilveteenused: "SecSvc001", "FrontEnd001" ja "BackEnd001"
- Virtuaalse võrgu, "CorpNetwork" kolme alamvõrku: "SecNet", "FrontEnd" ja "Taustväärtus"
- Võrgu virtuaalse seadme sel juhul tulemüüri, on ühendatud SecNet alamvõrgu
- Windows server, mis tähistab rakenduse veebiserverisse ("IIS01")
- Kaks Windows serverid, mis tähistab rakenduse tagaandmebaas serverid ("AppVM01", "AppVM02")
- Windows server, mis tähistab DNS server ("DNS01")

Skripte ja Azure ressursihaldur malli, leiate [üksikasjaliku Koosta juhiseid][Example3].

#### <a name="udr-description"></a>UDR kirjeldus
Vaikimisi on järgmine süsteemi marsruudib määratletud järgmiselt:

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.0.0/16}     VNETLocal                            Active   Default    
         {0.0.0.0/0}       Internet                             Active   Default    
         {10.0.0.0/8}      Null                                 Active   Default    
         {100.64.0.0/10}   Null                                 Active   Default    
         {172.16.0.0/12}   Null                                 Active   Default    
         {192.168.0.0/16}  Null                                 Active   Default

Funktsiooni VNETLocal on alati virtuaalse võrgu selle teatud võrgu jaoks määratletud aadress prefix(es) (ehk teisisõnu öeldes tabelduskohta virtuaalse võrgust virtuaalse võrku, olenevalt sellest, kuidas iga virtuaalse võrgu on määratletud). Ülejäänud süsteemi marsruudib on staatiline ja vaikimisi, nagu on näidatud tabelis.

Selles näites marsruutimise tabelitel on loodud, üks iga ees- ja alamvõrku. Staatilise protsesside vastav antud alamvõrgu laaditakse iga tabeli. Selleks, et selles näites on iga tabeli kolme marsruudib, mis suunaks liikluse kõik (0.0.0.0/0) läbi tulemüüri (järgmine hop = virtuaalse seadme IP-aadress):

1. Kohaliku alamvõrgu liiklus järgmise Hop määratletud kohaliku alamvõrgu liiklust tulemüüri lubama.
2. Virtuaalne võrguliiklust järgmise Hop määratletud tulemüüri; See alistab vaikimisi reeglit, mida võimaldab kohaliku virtuaalse võrguliikluse marsruutimiseks otse.
3. Kõigi ülejäänud liikluse (0/0) koos järgmise Hop määratletud tulemüüri.

>[AZURE.TIP] Ei ole kohaliku alamvõrgu kirje klõpsake soovitud UDR rikub kohaliku alamvõrgu suhtlus. 
> - Selles näites osutab VNETLocal 10.0.1.0/24 on muidu paketi lahkumiseks veebiserver (10.0.1.4) sinna mõne muu objekti (näiteks) 10.0.1.25 nurjub, kui need saadetakse üle Dessandi, mis saadab selle alamvõrgu, kriitiline ja alamvõrgu uuesti saadab selle funktsiooni Dessandi jne.
> - Marsruutimise tsükkel võimalusi on tavaliselt kõrgema mitme-nic seadmed, mis on ühendatud otse iga alamvõrgu nad suhtlevad, mis on sageli traditsiooniline, asutusesiseselt, seadmed. 

Kui marsruutimise tabelid on loodud, nad on seotud oma alamvõrku. Ees alamvõrgu marsruutimise tabel, kui loodud ja seotud alamvõrgu, näeks välja järgmine:

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.1.0/24}     VNETLocal                            Active 
         {10.0.0.0/16}     VirtualAppliance 10.0.0.4            Active    
         {0.0.0.0/0}       VirtualAppliance 10.0.0.4            Active

>[AZURE.NOTE] Nüüd saab rakendada UDR lüüsi alamvõrku, millel on ühendatud ExpressRoute ringi.
>
> Näiteid, kuidas lubada ExpressRoute või-saidilt võrgunduse võrgu perimeetri on toodud näited 3 ja 4.


#### <a name="ip-forwarding-description"></a>Kirjeldus IP edastamine
IP edastamine on UDR companion funktsiooni. See on virtuaalse seadme, mis võimaldab saada liikluse konkreetselt adresseeritud seadet, ja seejärel edastab selle liikluse lõpliku säte.

Näiteks kui liikluse aadressilt AppVM01 taotleb DNS01 server, UDR oleks marsruutida see tulemüüri. IP edastamine lubatud, kus liiklus DNS01 sihtkoha (10.0.2.4) aktsepteerinud seadme (10.0.0.4) ja seejärel edastatakse ultimate sihtkohta (10.0.2.4). Ilma IP edastamine tulemüüri sisse lülitatud, liikluse ei võeta seadet, ehkki marsruutimiseks tabelil on nimega kuvatakse järgmise sõnumihüppe kohta, pärast tulemüüri. Virtuaalse seadme kasutamiseks on oluline meeles pidada, et lubada UDR koos IP edastamine.

#### <a name="nsg-description"></a>NSG kirjeldus
Selles näites on ka NSG rühma ehitatud ja seejärel laadida ühe reegli abil. Selles rühmas on seotud seejärel üksnes ees- ja alamvõrku (mitte SecNet). Deklaratiivse ehitatakse järgmise reegli:

- Mis tahes liikluse (kõik pordid) Interneti kaudu kogu virtuaalse võrku (kõik alamvõrku) on keelatud.

Kuigi selles näites kasutatakse NSGs, selle peamine eesmärk on sekundaarne kiht vastu käsitsi vale konfiguratsioon. Eesmärk on blokeeri kogu sissetulev liiklus ees- või tagaandmebaas alamvõrku Interneti kaudu. Liikluse peaks ainult kuni SecNet alamvõrgu Firewall flow (ja seejärel vajadusel ees- või tagaandmebaas alamvõrku edasi). Lisaks UDR reegleid kohas, kus kõik liiklus, mis ei tee see ees- või tagaandmebaas alamvõrku oleks suunata välja tulemüüri (tänu UDR). Tulemüüri näeksid see on asümmeetriline kulgemist ja langeb väljaminev liiklus. Seega on kolme kihti kaitsmine on alamvõrku turvalisus.
- Pole avatud lõpp-punkte FrontEnd001 ja BackEnd001 cloud services.
- NSGs keelamisega seotud liikluse Interneti kaudu.
- Tulemüüri langetamine asümmeetriline liikluse.

Ühe huvitav kohta NSG selles näites on see, et see sisaldab ainult üks reegel, mis on Interneti-liikluse kogu virtuaalse võrku, sh turbe alamvõrgu keelamiseks. Kuna on NSG on seotud üksnes ees- ja alamvõrku, reegel ei töödelda, liikluse sissetuleva meili Turve alamvõrgu. Seetõttu saab liikluse tehingust turvalisus alamvõrgu.

#### <a name="firewall-rules"></a>Tulemüüri reeglid
Tulemüüri, tuleks luua edasisaatmise reegleid. Kuna tulemüüri blokeerimine või ümbersuunamine kogu Sissetulev, Väljaminev ja siseseid virtuaalse võrguliiklust, on vaja palju tulemüüri reeglid. Ka kogu sissetulev liiklus tabab turvalisuse teenuse avaliku IP-aadress (erinevad pordid), tulemüüri töötlemiseks. Hea tava on diagramm loogilise puhul enne selle alamvõrku häälestamist ja tulemüüri reegleid, et vältida töödelda hiljem. Järgmisel joonisel on loogiline vaade tulemüüri reegleid, näiteks:
 
![Loogika vaate tulemüüri reeglid][10]

>[AZURE.NOTE] Võrgu virtuaalse seadme kasutatud põhjal, erinevad pordid haldus. Selles näites viitab Barracuda NextGen tulemüüri, mis kasutab pordid 22, 801 ja 807. Lugege seadme tarnija dokumentatsiooni haldamiseks kasutatava seadme täpse pordid leidmiseks.

#### <a name="firewall-rules-description"></a>Tulemüüri reeglite kirjeldus
Eelmise loogiline skemaatilise diagrammi turvalisus alamvõrgu ei kuvata. Selle põhjuseks tulemüüri on ainult ressurss selle alamvõrgu ja diagramm näitab tulemüüri reeglid ja kuidas need loogiliselt lubada või keelata liikluse puhul, mitte tegelik suunatud tee. Ka välise valitud RDP liikluse on suurem ulatus pordid (8014 – 8026) ja valitud mõnevõrra joondamiseks kaks viimast octets kohalik IP-aadressi lihtsam loetavuse (nt kohaliku serveri aadress 10.0.1.4 on seostatud välise pordi 8014). Siiski võib kasutada mis tahes kõrgema vastuoluliste pordid.

Selle näite puhul läheb vaja seitse tüüpi reegleid:

- Välised reeglid (sissetulev liiklus):
  1.    Tulemüüri haldus reegel: see rakendus ümber suunata reegel võimaldab liikluse halduse pordid võrgu virtuaalse seadme edasi.
  2.    (Iga Windows server) RDP reeglid: neli reegleid (üks iga server) luba üksikute serverite kaudu RDP haldus. See võib kombineeritud sisse üks reegel, olenevalt kasutatava võrgu virtuaalse seadme võimalustest.
  3.    Rakenduse liikluse reeglid: kahe reegleid veebi liikluse esimene ja teine tagaandmebaas liikluse (nt veebiserver soovite andmete esimese taseme). Konfiguratsiooni reegleid sõltub võrgu arhitektuur (kui teie serverid suunatakse) ja liiklus puhul (liikluse puhul mis pordid kasutatakse, millises suunas).
      - Esimese reegli võimaldab tegeliku rakenduse liikluse rakenduse serverisse. Turvalisus ja haldamise lubada muude reeglitega, rakenduse liikluse reeglite ajal mis luba väliskasutajad või teenuste juurde pääseda soovitud rakendustes. Selle näite puhul on ühe veebiserverisse port 80. Seega ühe tulemüüri rakenduse reegli suunab sissetulev liiklus väline IP, web servers sisemise IP-aadress. Sisemise serveri kaudu NAT soovite tõlkida liikluse ümber seansi.
      - Teise reegli on mis tahes pordi tagaandmebaas reegli veebiserver rääkida AppVM01 server (kuid mitte AppVM02) lubamiseks.
- Sisemise reegleid (sisesed virtuaalse võrguliikluse) jaoks
  4.    Väljamineva Internet reeglit: see reegel võimaldab liikluse mis tahes võrk edastamiseks valitud võrgu kaudu. See reegel on tavaliselt vaikimisi reegel juba tulemüüri, kuid keelatud olekusse. Selles näites see reegel peaks olema lubatud.
  5.    DNS-i reegel: see reegel võimaldab ainult DNS-i (port 53) liiklus DNS server edasi. Selles keskkonnas enamik esiosa tagasi lõppu liikluse on blokeeritud. See reegel võimaldab konkreetselt DNS-i mis tahes kohaliku alamvõrgu.
  6.    Alamvõrgu alamvõrgu reeglil: see reegel on lubada mis tahes serveri tagaandmebaas alamvõrgu ees alamvõrgu (kuid mitte vastupidi) mis tahes serveriga ühenduse loomiseks.
- Tõrkekindel reeglit (liikluse, mis ei vasta, mis tahes eelmise):
  7.    Keela kõik liikluse reegel: see tuleb alati lõplik reegli (prioriteet) osas ja sellisena kui liikluse vool ei sobi ühegi eelmise reeglite, see kõrvaldatakse selle reegli alusel. See on vaikimisi reegel ja tavaliselt aktiveeritud. Vaja on üldiselt muutmata.

>[AZURE.TIP] Teine liikluse reegel lihtsustamiseks käesoleva näite korral mis tahes port on lubatud. Reaal stsenaariumi kõige kindla pordi ja aadresside vahemikud võib kasutada vähendada rünnak pind see reegel.

Kui kõik eelmise reeglid on loodud, on oluline üle vaadata iga tagada liikluse on lubatud või keelatud vastavalt soovile reegli prioriteedi. Selle näite puhul on reegleid tähtsuse järjekorras.

#### <a name="conclusion"></a>Kokkuvõte
See on keerulisem, kuid täielikuma viis kaitsmine ja eraldamiseks võrku kui eelmistes näidetes. (Näide 2 kaitseb vaid rakendus ja näide 1 lihtsalt isoleerib alamvõrku). Seda kujundust võimaldab järelevalve liikluse mõlemast suunast ja kaitseb mitte ainult rakenduse sissetuleva meili server, kuid jõustab võrgu turbepoliitika kõikides serverites selle võrgu jaoks. Lisaks sõltuvalt kasutatud seadme täielik liikluse auditeerimine ja teadlikkuse saab saavutada. Lisateabe saamiseks lugege [üksikasjalikku Koosta juhiseid][Example3]. Need juhised on järgmised.

- Kuidas koostada selles näites perimeetri võrgu PowerShelli skriptide abil.
- Kuidas koostada selles näites on Azure ressursihaldur malli abil.
- Üksikasjaliku kirjelduse iga UDR NSG käsu ja tulemüüri reegel.
- Üksikasjalik liikluse kulgemist stsenaariumid, kuidas liikluse lubatud või keelatud iga kiht.

### <a name="example-4-add-a-hybrid-connection-with-a-site-to-site-virtual-appliance-virtual-private-network-vpn"></a>Näide 4: Lisa-saidilt, virtuaalse seadme virtuaalse privaatvõrgu (VPN) ühenduse hübriid
[Kiire algus naasmiseks](#fast-start) | Üksikasjalikud Koosta juhistega varsti

![Perimeetri võrgu Dessandi võrguga ühenduses hü][11]

#### <a name="environment-description"></a>Keskkonna kirjeldus
Hübriidjuurutuse võrgunduse võrgu virtuaalse seadme (Dessandi) abil saate lisada mõne perimeetri võrgu tüüpi kirjeldatud näited 1, 2 või 3.

Nagu eelmisel joonisel näha, kasutatakse VPN-ühendus (saidi saidi) Interneti kaudu ühenduse loomine mõne kohapealse võrgu Azure virtuaalse võrgu kaudu mõne Dessandi.

>[AZURE.NOTE] Kui kasutate ExpressRoute lubatud Azure avalike silmitsemine suvandiga, tuleks luua staatilise marsruutimiseks. See peaks marsruutida Dessandi VPN IP-aadress ja teie ettevõtte Interneti kaudu ExpressRoute WAN. Nõutavad ExpressRoute Azure avaliku silmitsemine suvand NAT saate leheküljepiiri VPN seanss.

Kui VPN on loodud, kuvatakse Dessandi muutub keskne jaoturiga kõigi võrgu ja alamvõrku. Tulemüüri edasisaatmise reegleid kindlaks teha, millised liikluse puhul lubatud, on tõlgitud NAT kaudu, suunatakse või kõrvaldatakse (isegi jaoks liikluse puhul kohapealse võrgu ja Azure vahel).

Liikluse puhul hoolikalt kaaluma, kui need saab optimeerida või selle kujundus mustri halvenenud konkreetsest kasutamise korral.

Näide 3 ehitatud keskkonna abil ja seejärel lisada-saidilt VPN hübriid võrguühendust, tekitab järgmised kujundus.

![Dessandi perimeetri võrku ühendatud-saidilt VPN-i abil][12]

Kohapealse ruuteri või muud võrgu seade, mis ühildub oma Dessandi VPN-i, oleks VPN klient. Selle seadme vastutaks alustamist ja säilitamise VPN-ühendus oma Dessandi.

Loogiliselt Dessandi, et võrgu näeb välja nagu neli eraldi "Turbetsoonid" eeskirjadele Dessandi, vahel tsoonid esmane juhataja on:

![Loogilise võrgu Dessandi seisukohalt][13]

#### <a name="conclusion"></a>Kokkuvõte
Azure virtuaalse võrguga võrguühenduse saidilt VPN hübriid lisamine laiendada Azure'i sisse kohapealse võrgu turvaline viisil. VPN-ühenduse kasutamine oma liiklus on krüptitud ja marsruudib Interneti kaudu. Dessandi selles näites toodud Jõusta ja turbepoliitika hallata ühes keskses kohas. Lisateabe saamiseks lugege üksikasjalikku Koosta juhiseid (tulevased). Need juhised on järgmised.

- Kuidas koostada selles näites perimeetri võrgu PowerShelli skriptide abil.
- Kuidas koostada selles näites on Azure ressursihaldur malli abil.
- Üksikasjalik liikluse kulgemist stsenaariumid, mis näitab, kuidas liikluse liigub sujuvalt selle kujunduse kaudu.

### <a name="example-5-add-a-hybrid-connection-with-a-site-to-site-azure-gateway-vpn"></a>Näide 5: Lisada VPN-saidilt, Azure lüüsi ühenduse hübriid
[Kiire algus naasmiseks](#fast-start) | Üksikasjalikud Koosta juhistega varsti

![Perimeetri võrgu lüüsi võrguga ühenduses hübriid][14]

#### <a name="environment-description"></a>Keskkonna kirjeldus
Hübriidjuurutuse võrgunduse on Azure VPN-lüüsi abil saate lisada mõlemal perimeetri võrgu tüüp kirjeldatud näited 1 või 2.

Nagu eelmisel joonisel näha, kasutatakse VPN-ühendus (saidi saidi) Interneti kaudu ühenduse loomine mõne kohapealse võrgu Azure virtuaalse võrgu kaudu on Azure VPN-lüüsi.

>[AZURE.NOTE] Kui kasutate ExpressRoute lubatud Azure avalike silmitsemine suvandiga, tuleks luua staatilise marsruutimiseks. See peaks marsruutida Dessandi VPN IP-aadress ja teie ettevõtte Interneti kaudu ExpressRoute WAN. Nõutavad ExpressRoute Azure avaliku silmitsemine suvand NAT saate leheküljepiiri VPN seanss.

Järgneval joonisel on kujutatud kahe võrgu servad selle suvandi. Esimese serva Dessandi ja NSGs juhtida liikluse puhul siseseid Azure võrgu ja Azure ja Interneti vahel. Teise serva on Azure VPN-lüüsi, mis on täielikult eraldi ja eraldatud võrgu serva kohapealse ja Azure vahel.

Liikluse puhul hoolikalt kaaluma, kui need saab optimeerida või selle kujundus mustri halvenenud konkreetsest kasutamise korral.

Näide 1 ehitatud keskkonna abil ja seejärel lisada-saidilt VPN hübriid võrguühendust, tekitab järgmised kujundus.

![Perimeetri võrku ühendatud ExpressRoute ühenduse abil lüüsi][15]

#### <a name="conclusion"></a>Kokkuvõte
Azure virtuaalse võrguga võrguühenduse saidilt VPN hübriid lisamine laiendada Azure'i sisse kohapealse võrgu turvaline viisil. Kohalikke Azure'i VPN-lüüsi abil oma liiklus on krüptitud IPSec ja marsruudib Interneti kaudu. Ka Azure VPN-lüüsi abil saate sisestada alumise maksumus suvand (pole täiendavaid litsentsimise sarnaselt muude tootjate NVAs maksumus). See on odavaimal näites 1, kus kasutatakse pole Dessandi. Lisateabe saamiseks lugege üksikasjalikku Koosta juhiseid (tulevased). Need juhised on järgmised.

- Kuidas koostada selles näites perimeetri võrgu PowerShelli skriptide abil.
- Kuidas koostada selles näites on Azure ressursihaldur malli abil.
- Üksikasjalik liikluse kulgemist stsenaariumid, mis näitab, kuidas liikluse liigub sujuvalt selle kujunduse kaudu.

### <a name="example-6-add-a-hybrid-connection-with-expressroute"></a>Näide 6: Lisada ExpressRoute ühenduse hübriid
[Kiire algus naasmiseks](#fast-start) | Üksikasjalikud Koosta juhistega varsti

![Perimeetri võrgu lüüsi võrguga ühenduses hübriid][16]

#### <a name="environment-description"></a>Keskkonna kirjeldus
Hü võrgunduse ExpressRoute privaatne silmitsemine ühenduse abil saate lisada kas perimeetri võrgu tüüpi kirjeldatud näited 1 või 2.

Nagu eelmisel joonisel näha, ExpressRoute privaatne silmitsemine pakub otsest kohapealse võrgu ja Azure virtuaalse võrgu vahelise ühenduse. Liikluse transiitvedude ainult teenuse pakkuja võrgu ja Microsoft Azure'i võrgu, mitte kunagi puudutades Interneti-ühendus.

>[AZURE.NOTE] UDR kasutamisel koos ExpressRoute, dünaamiline marsruutimine kasutatud Azure virtuaalse lüüsi keerukuse tõttu on teatud piirangud. Need on järgmised:
>
>- Lüüsi alamvõrku, millel on ühendatud ExpressRoute lingitud Azure virtuaalse lüüsi UDR rakendatakse.
>- ExpressRoute lingitud Azure virtuaalse lüüsi ei saa NextHop seadme jaoks muud UDR seotud alamvõrku.
>
>
<br />

>[AZURE.TIP] Kasutades ExpressRoute hoiab ettevõtte võrguliiklust off Internetis parema turvalisuse eesmärgil ja oluliselt suurendada jõudlust. See võimaldab ka teenuse teenusetaseme lepinguid ExpressRoute teenusepakkujalt. Azure'i lüüsi saab edastada kuni 2 Gb/s ExpressRoute, VPN saidilt, kus on Azure lüüsi maksimaalse läbilaskevõime 200 Mb/s.

Nagu järgmisel joonisel näha, kui see suvand keskkonna on nüüd kaks võrgu servad. Dessandi ja NSG juhtida liikluse puhul siseseid Azure võrgu ja Azure ja Internet, lüüsi on täielikult eraldi ja eraldatud võrgu serva kohapealse ja Azure vahel.

Liikluse puhul hoolikalt kaaluma, kui need saab optimeerida või selle kujundus mustri halvenenud konkreetsest kasutamise korral.

Näide 1 ehitatud keskkonna abil ja seejärel lisada ExpressRoute hübriid võrguühendust kasutavad tekitab järgmised kujundus.

![Perimeetri võrku ühendatud ExpressRoute ühenduse abil lüüsi][17]

#### <a name="conclusion"></a>Kokkuvõte
Lisaks ka ExpressRoute era Peering võrguühenduse saate laiendada kohapealse võrgu Azure'i sisse turvaline, alumises latentsuse kõrgem läbimiseks viisil. Lisaks kohalikke Azure'i lüüsi, nagu järgmises näites kasutamine tagab alumise maksumus suvand (no täiendavate litsentsimise sarnaselt muude tootjate NVAs). Lisateabe saamiseks lugege üksikasjalikku Koosta juhiseid (tulevased). Need juhised on järgmised.

- Kuidas koostada selles näites perimeetri võrgu PowerShelli skriptide abil.
- Kuidas koostada selles näites on Azure ressursihaldur malli abil.
- Üksikasjalik liikluse kulgemist stsenaariumid, mis näitab, kuidas liikluse liigub sujuvalt selle kujunduse kaudu.

## <a name="references"></a>Viited
### <a name="helpful-websites-and-documentation"></a>Kasulik veebilehed ja dokumentatsioon
- Juurdepääs Azure'i Azure ressursihaldur:
- Juurdepääs Azure'i PowerShelli abil: [https://azure.microsoft.com/documentation/articles/powershell-install-configure/](./powershell-install-configure.md)
- Virtuaalse võrgu dokumentatsiooni: [https://azure.microsoft.com/documentation/services/virtual-network/](https://azure.microsoft.com/documentation/services/virtual-network/)
- Võrgu turvalisus jaotises dokumendid: [https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/](./virtual-network/virtual-networks-nsg.md)
- Kasutaja määratletud marsruutimise dokumentatsiooni: [https://azure.microsoft.com/documentation/articles/virtual-networks-udr-overview/](./virtual-network/virtual-networks-udr-overview.md)
- Azure virtuaalse lüüside: [https://azure.microsoft.com/documentation/services/vpn-gateway/](https://azure.microsoft.com/documentation/services/vpn-gateway/)
- VPN saidilt: [https://azure.microsoft.com/documentation/articles/vpn-gateway-create-site-to-site-rm-powershell](./vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)
- ExpressRoute dokumentatsiooni (kindlasti vaadake jaotist "Alustamine" ja "Kuidas"): [https://azure.microsoft.com/documentation/services/expressroute/](https://azure.microsoft.com/documentation/services/expressroute/)

<!--Image References-->
[0]: ./media/best-practices-network-security/flowchart.png "Turvalisus suvandid vooskeem"
[1]: ./media/best-practices-network-security/compliancebadges.png "Azure'i vastavuse märgid"
[2]: ./media/best-practices-network-security/azuresecurityfeatures.png "Azure'i turbefunktsioonid"
[3]: ./media/best-practices-network-security/dmzcorporate.png "Mõne DMZ ettevõtte võrgus"
[4]: ./media/best-practices-network-security/azuresecurityarchitecture.png "Azure'i turbearhitektuur"
[5]: ./media/best-practices-network-security/dmzazure.png "Mõne DMZ Azure virtuaalse võrgus"
[6]: ./media/best-practices-network-security/dmzhybrid.png "Hübriid võrku kolme turvalisus piirmäärad"
[7]: ./media/best-practices-network-security/example1design.png "Sissetulev DMZ NSG abil"
[8]: ./media/best-practices-network-security/example2design.png "Sissetulev DMZ Dessandi ja NSG"
[9]: ./media/best-practices-network-security/example3design.png "Kahesuunalise DMZ Dessandi, NSG ja UDR"
[10]: ./media/best-practices-network-security/example3firewalllogical.png "Loogika vaate tulemüüri reeglid"
[11]: ./media/best-practices-network-security/example4designoptions.png "DMZ koos Dessandi ühendatud hübriid võrku"
[12]: ./media/best-practices-network-security/example4designs2s.png "DMZ koos Dessandi ühendatud-saidilt VPN-i abil"
[13]: ./media/best-practices-network-security/example4networklogical.png "Loogilise võrgu Dessandi seisukohalt"
[14]: ./media/best-practices-network-security/example5designoptions.png "DMZ Azure Gateway ühendatud-saidilt hübriid võrku"
[15]: ./media/best-practices-network-security/example5designs2s.png "DMZ Azure Gateway-saidilt VPN-i abil"
[16]: ./media/best-practices-network-security/example6designoptions.png "DMZ Azure Gateway ühendatud ExpressRoute hübriid võrku"
[17]: ./media/best-practices-network-security/example6designexpressroute.png "DMZ Azure Gateway ExpressRoute ühenduse abil"

<!--Link References-->
[Example1]: ./virtual-network/virtual-networks-dmz-nsg-asm.md
[Example2]: ./virtual-network/virtual-networks-dmz-nsg-fw-asm.md
[Example3]: ./virtual-network/virtual-networks-dmz-nsg-fw-udr-asm.md
[Example4]: ./virtual-network/virtual-networks-hybrid-s2s-nva-asm.md
[Example5]: ./virtual-network/virtual-networks-hybrid-s2s-agw-asm.md
[Example6]: ./virtual-network/virtual-networks-hybrid-expressroute-asm.md
[Example7]: ./virtual-network/virtual-networks-vnet2vnet-direct-asm.md
[Example8]: ./virtual-network/virtual-networks-vnet2vnet-transit-asm.md
