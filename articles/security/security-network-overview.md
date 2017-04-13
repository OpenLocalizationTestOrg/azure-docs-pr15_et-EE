<properties
   pageTitle="Azure'i võrgu turvalisuse ülevaade | Microsoft Azure'i"
   description=" Selles artiklis hõlbustab teil mõista, mida Microsoft Azure'i on võrgu turvalisuse alal pakkuda. Pakume basic selgitused keskendudes võrgu Turve ja nõuetele ning teave Azure'i pakkuda kõigi järgmiste alade kohta. "
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="MBaldwin"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/09/2016"
   ms.author="terrylan"/>

# <a name="azure-network-security-overview"></a>Azure'i võrgu turvalisuse ülevaade

Microsoft Azure'i sisaldab robustne võrgu infrastruktuur oma rakenduste ja teenuste ühenduvuse nõuete toetamiseks. Võrguühendus on võimalik ressursse, mis asub Azure, asutusesisese ja Azure majutatud ressursse, ja ja sealt Interneti- ja Azure.

Käesoleva artikli eesmärk on hõlpsam mõista, mida Microsoft Azure'i on võrgu turvalisuse alal pakkuda. Siin pakume basic selgitused keskendudes võrgu Turve ja nõuetele. Me ka teavet te mida Azure on kõigi järgmiste alade pakkuda. Seal on palju muud sisu, mis võimaldab teil saada täpsemat mõistmine alal, mis teile huvi lingid.

Azure'i võrgu turvalisuse ülevaade selles artiklis keskendutakse järgmist:

- Azure'i võrgunduse
- Võrgu juurdepääsu reguleerimine
- Turvaline juurdepääs ja asukohaga remote connectivity
- Kättesaadavus
- Logimine
- Nimelahendus
- DMZ arhitektuur
- Azure'i turbekeskus

## <a name="azure-networking"></a>Azure'i võrgunduse

Virtuaalmasinates vaja võrguühendus. See tingimus toetamiseks nõuab Azure'i virtuaalmasinates Azure virtuaalse võrku ühendatud. Muu Azure virtuaalse kaudu on loogiline ehitada, ehitatud füüsilise Azure võrgu struktuuri. Iga loogiline Azure virtuaalse võrgu on eraldatud kõik muud Azure virtuaalse võrgu. See aitab tagada, et teie juurutuste võrguliiklust ei ole puuetega inimestele juurdepääsetavate teistele Microsoft Azure'i klientidele.

Lisateave:

- [Virtuaalse võrgu ülevaade](../virtual-network/virtual-networks-overview.md)

## <a name="network-access-control"></a>Võrgu juurdepääsu reguleerimine
Võrgu juurdepääsu reguleerimine on piirata Ühenduvus ja kindlate seadmete või alamvõrku on Azure virtuaalse võrgustikus toiming. Võrgu juurdepääsu reguleerimine eesmärk on veenduge, et teie virtuaalmasinates ja teenused on saadaval ainult kasutajatele ja seadmetele, millele soovite need puuetega inimestele juurdepääsetavate. Accessi juhtelemendid põhinevad sisse lubada või keelata otsuseid ühendused ja oma virtuaalse masina või teenuse.

Azure'i toetab mitut tüüpi võrgu juurdepääsu reguleerimine. Järgmised:

- Võrgu kiht juhtelement
- Juhtelemendi marsruutimine ja sunnitud tunneling
- Virtuaalse võrgu turvalisus seadmed

### <a name="network-layer-control"></a>Võrgu Layer juhtelement
Mis tahes turvaline juurutamise jaoks on vaja mõned mõõtmine võrgu juurdepääsu reguleerimine. Võrgu juurdepääsu reguleerimine eesmärk on veenduge, et teie virtuaalmasinates ja nende virtuaalmasinates töötavad võrguteenuste saaksid suhelda ainult muude võrguseadmete, et neil on vaja suhelda ja kõigi ühenduse katsete on blokeeritud.

Kui teil on vaja põhilised võrgu taseme juurdepääsu reguleerimine (vastavalt IP-aadress ja TCP või UDP protokollid), saate kasutada võrgu turberühmad. Võrgu turvalisus jaotises (NSG) on tavaline stateful paketi filtreerimine tulemüür ja võimaldab põhjal [5-kordse](https://www.techopedia.com/definition/28190/5-tuple)juurdepääsu kontrollimiseks. NSGs ei paku rakenduse kiht kontrolli või autenditud Accessi juhtelemendid.

Lisateave:

- [Võrgu turberühmad](../virtual-network/virtual-networks-nsg.md)

### <a name="route-control-and-forced-tunneling"></a>Marsruutimiseks juhtelement ja jõustatud Tunneling
Võimalus määrata marsruutimise käitumine Azure virtuaalse võrguga on kriitiliste turbe ja juurdepääsu juhtimine võimalus. Kui Marsruutimine on valesti konfigureeritud, rakenduste ja teenuste majutatud virtual arvuti ühendada seadmed, mida te ei soovi neid ühenduse loomiseks, sh omavad ja juhivad võimalikud ründajad.

Azure'i võrgunduse toetab võimalus võrguliikluse kohta Azure virtuaalse võrguga marsruutimise käitumist. See võimaldab teil muuta vaikimisi marsruutimise tabeli kirjed Azure virtuaalse võrgu. Juhtimise marsruutimise käitumine aitab teil tagada, et kõik liiklus seadme või seadmete rühma sisestab või jätab Azure virtuaalse võrgu kaudu kindlasse asukohta.

Näiteks peate virtuaalse võrgu turvalisus seadme, võrgu Azure virtuaalse. Soovite veenduda, et kõik liiklust ja Azure virtuaalse võrgu kaudu läheb virtuaalse turvalisus selle seadme kaudu. Saate seda teha, kui [Kasutaja määratletud marsruudib](../virtual-network/virtual-networks-udr-overview.md) Azure konfigureerimine.

[Sunnitud tunneling](https://www.petri.com/azure-forced-tunneling) on abil saate tagada, et teenuste ei ole lubatud algatada seadmed Interneti-ühendus. Pange tähele, et see on erinev vastu sissetulevaid ühendusi ja siis neile vastata. Veebi serverid on vaja vastata Interneti hosts ja nii hangitud Interneti-liikluse lubatud sissetulev need web serverid ja web-server pole lubatud vastata.

Te ei soovi, et lubada on algatada taotluse Väljamineva meili server veebi. Taotlused võivad kujutada ohtu turvalisusele, kuna need ühendused võivad kasutada ründevara allalaadimiseks. Isegi juhul, kui soovite need ees serverid algatada väljaminev taotlusi Interneti-ühendus, mida soovite sundige neid läbida oma kohapealse web puhverserverite nii, et saate ära kasutada URL-i filtreerimine ja logimine.

Soovite soovite selle asemel kasutada jõustatud tunneling selle ära. Jõustatud tunneling lubamisel sunnitakse kõik ühendused Interneti kaudu oma kohapealse lüüsi. Saate konfigureerida jõustatud tunneling kasutaja määratletud marsruudib ära.

Lisateave:

- [Mis on kasutaja määratletud marsruudib ja IP edastamine](../virtual-network/virtual-networks-udr-overview.md)

### <a name="virtual-network-security-appliances"></a>Virtuaalse võrgu turvalisus seadmed
Võrgu turberühmad, kasutaja määratletud marsruudib ja jõustatud tunneling paku järgu võrk ja transport kihid [OSI mudeli](https://en.wikipedia.org/wiki/OSI_model)turvalisus, võib juhtuda, kui soovite lubada üle võrgu turvalisus.

Näiteks oma turbenõuetele olla:

- Autentimis- ja Luba juurdepääs teie lubamisel
- Sissetungi avastamine ja sissetungi vastus
- Rakenduse kiht kontrolli üksikasjalik Protokollid
- URL-i filtreerimine
- Võrgu taseme viirusetõrje
- Anti robot kaitse
- Rakenduse juurdepääsu reguleerimine
- Täiendavad DDoS kaitse (kohal olevat DDoS kaitse esitatud Azure struktuuri ise)

Azure'i partneri lahenduse abil pääsete juurde täiustatud võrgu funktsioonide. Leiate Azure'i uusimad partneri võrgu lahendustele külastades [Azure'i turuplatsi](https://azure.microsoft.com/marketplace/) ja märksõna "Turvalisus" ja "võrgu turvalisus".

## <a name="secure-remote-access-and-cross-premises-connectivity"></a>Turvaline Kaugpöördusteenuse ja rist tööruumide Ühenduvus
Seadistamine, konfigureerimine ja haldamine oma Azure ressursse vaja teha kaugühenduse teel. Lisaks võite soovida juurutada [hübriid IT](http://social.technet.microsoft.com/wiki/contents/articles/18120.hybrid-cloud-infrastructure-design-considerations.aspx) lahendusi, mis on asutusesisene komponendid ja Azure avaliku pilve. Need stsenaariumid nõuab turvalist Kaugpöördusteenuse.

Azure'i võrgunduse toetab turvaline Kaugpöördusteenuse järgmistel juhtudel:

- Üksikute töökohtade Azure virtuaalse võrguga ühenduse
- Azure'i virtuaalse võrgu VPN kohapealse võrgu ühendamine
- Kohapealse võrgu ühenduse Azure virtuaalse võrgu sihtotstarbeline WAN link
- Üksteise Azure virtuaalne võrkude ühendamine

### <a name="connect-individual-workstations-to-an-azure-virtual-network"></a>Üksikute töökohtade Azure virtuaalse võrguga ühenduse
Võib juhtuda, kui soovite lubada üksikuid arendajate või toimingute personali virtuaalmasinates ja Azure teenuste haldamine. Näiteks juurdepääsu saamiseks virtuaalse masina Azure virtuaalse võrgus ja teie turbepoliitika ei luba üksikute virtuaalmasinates RDP või SSH Kaugpöördusteenuse. Sel juhul saate kasutada punkti saidi VPN-ühendus.

Punkti saidi VPN-ühendus kasutab selleks, et saaksite luua eraldi ja turvalise seost kasutaja ja Azure virtuaalse võrgu [SSTP VPN](https://technet.microsoft.com/library/cc731352.aspx) -protokolli. Kui VPN-ühendus on loodud, kasutaja saab RDP või SSH VPN-lingi mis tahes virtuaalse masina Azure virtuaalse võrgus sisse (eeldades, et kasutaja saab autentimiseks ja see on lubatud).

Lisateave:

- [Punkti saidi virtuaalse võrgu PowerShelli kaudu ühenduse konfigureerimine](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

### <a name="connect-your-on-premises-network-to-an-azure-virtual-network-with-a-vpn"></a>Azure virtuaalse võrgusüsteem VPN kohapealse võrgu ühendamine
Kui soovite kogu ettevõtte võrgus või selle osade Azure virtuaalse võrguga ühenduse. See on levinud hübriid IT stsenaariumid kus ettevõtted [laiendamine oma kohapealse andmekeskusesse Azure'i sisse](https://gallery.technet.microsoft.com/Datacenter-extension-687b1d84). Paljudel juhtudel kuvatakse ettevõtete majutada osade teenust Azure ja osade asutusesiseselt, näiteks kui lahenduse sisaldab veebi serverid Azure'i ja kohapealsete andmebaasid. "Asukohaga" ühendused sellised muuta ka haldamine asub Azure ressursse rohkem turvata ja lubada stsenaariumid, nt Azure Active Directory domeenikontrollerid laiendada.

Üks viis selleks on kasutada [VPN saidilt](https://www.techopedia.com/definition/30747/site-to-site-vpn). Vahe-saidilt VPN-i ja punkti saidi VPN on, et punkti saidi VPN loob ühe seadme Azure virtuaalse võrku ajal VPN saidilt loob kogu võrgu (nt kohapealse võrgu) Azure virtuaalse võrku. Azure'i virtuaalse võrgu-saidilt VPN kasutada turvaline IPsec tunneliga režiimi VPN-protokolli.

Lisateave:

- [Ressursihaldur VNet portaalis Azure-saidilt VPN-ühenduse loomine](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Kavandamise ja kujunduse VPN-lüüsi jaoks](../vpn-gateway/vpn-gateway-plan-design.md)

### <a name="connect-your-on-premises-network-to-an-azure-virtual-network-with-a-dedicated-wan-link"></a>Kohapealse võrgu ühenduse Azure virtuaalse võrgusüsteem sihtotstarbeline WAN-lingi
Punkti saidi ja saidilt VPN-ühendused on tõhus asutusesiseses ühenduvuse lubamine. Siiski mõni ettevõte kaaluge nende on järgmised puudused.

- VPN-ühendused andmete teisaldamine Interneti-see seab nende võimalikud probleemid turvalisus seotud andmete teisaldamine avaliku võrgu ühendused. Lisaks ei saa tagada töökindlust ja kättesaadavus Interneti-ühendused.
- Azure'i virtuaalne võrkude VPN-ühenduste võib käsitleda läbilaskevõime piiratud teatud rakenduste ja eesmärgil, nagu nad max välja juures 200Mbps ümber.

Ettevõtted, mis tuleb kõrgeima turbe-saadavus ja nende asutusesiseses ühenduste tavaliselt sihtotstarbeline WAN linkide abil ühenduse loomine remote saidid. Azure'i pakub teile võimalust kasutada sihtotstarbeline WAN-lingi, mille abil saate ka Azure virtuaalse võrguga ühenduse loomiseks kohapealse võrgu. See on lubatud Azure ExpressRoute kaudu.

Lisateave:

- [ExpressRoute tehniline ülevaade](../expressroute/expressroute-introduction.md)

### <a name="connect-azure-virtual-networks-to-each-other"></a>Azure'i ühenduse üksteisest
On võimalik oma juurutuste palju Azure virtuaalse võrgu kasutamiseks. On palju põhjuseid, miks võiksite teha. Üheks põhjuseks võib olla lihtsustada haldamist; teise võib olla turvalisuse põhjustel. Sõltumata motivatsiooni või põhjuseid lihtsate ressursid erinevates Azure virtuaalse võrkudes, võib olla soovite ressursse iga võrgu omavahel ühendada.

Üheks võimaluseks oleks teenuste ühe Azure virtuaalse võrgu "hoidke tagasi" teel ühenduse loomiseks muu Azure virtuaalse võrgu teenuste Interneti kaudu. Ühenduse alustamine ühe Azure virtuaalse võrgu, avage Interneti kaudu ja siis naaske Azure virtuaalse võrgu sihtkohta. See suvand seab turvalisuse probleemid, mis on omased Interneti-põhiste side ühendus.

Parem valik võib olla luua mõne Azure virtuaalse võrgu Azure virtuaalne võrgu-saidilt VPN. See Azure virtuaalse võrgu Azure virtuaalne võrgu-saidilt VPN kasutab sama [IPsec tunneliga režiimi](https://technet.microsoft.com/library/cc786385.aspx) protokolli asutusesiseses-saidilt VPN-ühendus eespool nimetatud.

On see eelis, kasutades mõnda Azure virtuaalse võrgu Azure virtuaalne võrgu-saidilt VPN, et VPN-ühendus on loodud üle Azure võrgu struktuuri; ei saa ühendust Interneti kaudu. See pakub teile mõne eest kiht võrreldes-saidilt VPN, mis Internetis ühenduse turvalisus.

Lisateave:

- [Azure'i ressursihaldur ja PowerShelli abil VNet-VNet ühenduse konfigureerimine](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

## <a name="availability"></a>Kättesaadavus
Kättesaadavus on oluline osa mis tahes programmi, Turve. Kui oma kasutajate ja süsteemid ei pääse, mida nad peavad juurde pääsema võrgu kaudu, pidada teenuse kahjustada. Azure'i on võrgu tehnoloogiaid, mis toetavad kõrge-saadavus järgmisi elemente:

- HTTP-põhine laadi tasakaalustamiseks
- Võrgu taseme laadi tasakaalustamiseks
- Globaalne tasakaalustamiseks laadimine

Koormusetasakaalustuseks on mõeldud jaotada võrdselt ühendused mitmes seadmes süsteem. Eesmärgid koormusetasakaalustuseks on:

- Suurendada kättesaadavus – Topeltdegressiivse ühendused laadimist mitmes seadmes, üks või mitu seadmete võib muutuda saadaval ja ülejäänud online seadmetes töötavad teenused, saate jätkata sisu teenuse
- Jõudluse suurendamine – Topeltdegressiivse ühendused avamisel mitmes seadmes ühe seade pole võtta protsessor tulemus. Selle asemel töötlemine ja mälu nõudmistele serveeritakse sisu on levinud mitmes seadmes.

### <a name="http-based-load-balancing"></a>HTTP-põhine laadi tasakaalustamiseks
Ettevõtted, mis töötavad veebi-põhiste teenuste sageli soov on soovitud laadi HTTP-põhine koormusetasakaalustusteenuse ette need aitavad kindlustada piisava taseme jõudlus ja kõrge-saadavus veebiteenused. Vastupidiselt traditsiooniline võrgupõhine koormus soolise, tasakaalustamiseks otsuste HTTP-põhine laadi koormus soolise põhinevad omaduste HTTP protokolli, mitte võrk ja transport layer protokollid.

Teile HTTP-põhine Laadi veebi-põhiste teenuste jaoks, pakub Azure'i Azure rakenduste portaali. Lüüsi Azure'i rakendus toetab.

- HTTP-põhine laadimine tasakaalustamiseks – koormust tasakaalustavad otsuseid tehakse vastavalt omadus teisiti HTTP-protokoll
- Selle võimaluse abil seansi küpsise vastavalt osaleja – veenduge, et ühendused, mis on loodud üks serverid taha seda laadi koormusetasakaalustusteenuse jääb samaks kliendi ja serveri vahel. See kindlustab stabiilsuse tehingute.
- SSL-i offload – kui kliendi ühendus on loodud koos laadi koormusetasakaalustusteenuse, et kliendi ja laadi koormusetasakaalustusteenuse vahel on krüptitud on HTTPS (SSL /) Protocol (protokoll). Jõudluse suurendamiseks teil siiski võimalus on koormusetasakaalustusteenuse laadimine ja laadi koormusetasakaalustusteenuse kasutamine (krüptimata) HTTP-protokolli veebiserver vahelise ühenduse. Seda nimetatakse "SSL offload" Kuna taha laadi koormusetasakaalustusteenuse web-server ei pea seotud krüptimise protsessor kohal kogemusi ja seetõttu peaks oskama hooldustaotlused kiiremini.
- URL-i-põhine sisu marsruutimine – see funktsioon võimaldab laadi koormusetasakaalustusteenuse kohta, kuhu edasi ühendused, mis põhineb URL-i siht otsuseid. See pakub palju rohkem paindlikkust kui laadimine tasakaalustavad otsuseid IP-aadresside lahendused.

Lisateave:

- [Rakenduse lüüsi ülevaade](../application-gateway/application-gateway-introduction.md)

### <a name="network-level-load-balancing"></a>Võrgu taseme laadi tasakaalustamiseks
Erinevalt HTTP-põhise koormusetasakaalustuseks, teeb võrgu taseme koormusetasakaalustuseks koormust tasakaalustavad otsuseid IP-aadress ja pordi (TCP või UDP) arve alusel.
Pääsete võrgu taseme laadi tasakaalustamiseks Azure, kasutades Azure laadimine koormusetasakaalustusteenuse eelised. Azure'i laadi koormusetasakaalustusteenuse põhitunnused mõni järgmised.

- Võrgu taseme koormusetasakaalustuseks IP-aadresside ja portide arve alusel
- Rakenduse kiht protokolli tugi
- Laadi saldo, et Azure'i virtuaalmasinates ja cloud services rolli aknad
- Saate kasutada nii Interneti-ühendusega (väline koormusetasakaalustuseks) ja mitte-Interneti vastastikuste (sisemine koormus tasakaalustamiseks) rakenduste ja virtuaalmasinates
- Lõpp-punkti jälgimine, mida kasutatakse kindlaks teha, kui kõiki teenuseid taha laadi koormusetasakaalustusteenuse on muutunud pole saadaval

Lisateave:

- [Laadi koormusetasakaalustusteenuse Internet vastastikuste mitme Virtuaalmasinates või teenuste](../load-balancer/load-balancer-internet-overview.md)
- [Sisemise koormuse koormusetasakaalustusteenuse ülevaade](../load-balancer/load-balancer-internal-overview.md)

### <a name="global-load-balancing"></a>Globaalne tasakaalustamiseks laadimine
Mõni ettevõte soovite kättesaadavus kõrgeima taseme võimalikud. Host rakenduste globaalselt hajutatud andmekeskuste on üks võimalus eesmärgi saavutamiseks. Kui rakendus on majutatud andmekeskuste asub kogu maailmas, on võimalik jaoks kogu geopoliitiliste piirkonna muutuvad kättesaamatuks ja on endiselt rakendus ja käivitatud.

Lisaks saate globaalselt hajutatud andmekeskuste rakenduste hallates kättesaadavus eeliseid, samuti saate avada jõudlus. Abil, mis suunab taotluste teenust andmekeskusesse, mis on lähima seadme, mille teeb taotluse saab jõudluse järgmisi eeliseid.

Globaalne koormusetasakaalustuseks pakub teile nii järgmisi eeliseid. Azure'i, pääsete globaalne laadi tasakaalustamiseks, kasutades Azure liiklus Manager eeliste.

Lisateave:

- [Mis on liikluse haldur?](../traffic-manager/traffic-manager-overview.md)

## <a name="logging"></a>Logimine
Võrgu tasemel logimine on mis tahes Võrk Turvalisus stsenaarium klahv funktsioon. Azure'i, saate sisse logida võrgu turberühmad saada võrgu taseme teave logimise kohta saadud teavet. NSG logimine, kuvatakse järgmise teabe:

- Auditilogide – need logid kasutatakse kõik toimingud, mis on esitatud Azure tellimuste kuvamiseks. Need logid on vaikimisi sisse lülitatud ja seda saab kasutada Azure portaali.
- Sündmuselogide – need logid teavet mis NSG reeglid on rakendatud.
- Vastuolus logid – need logid teile teada, kui mitu korda iga NSG reegel rakendatud keelamiseks või liikluse lubamiseks.

Samuti saate [Microsoft Power BI](https://powerbi.microsoft.com/what-is-power-bi/), võimas andmete visualiseerimise tööriist, vaatamiseks ja analüüsimiseks need logid.

Lisateave:

- [Log Analytics võrgu turberühmad (NSGs)](../virtual-network/virtual-network-nsg-manage-log.md)

## <a name="name-resolution"></a>Nimelahendus
Nimelahendus on kriitiline funktsioon kõigi teenuste majutate Azure. Turvalisuse seisukohast kompromiss funktsiooni nimi eraldusvõime võib põhjustada ründaja ümbersuunamist taotluste ründaja saidi saitide hulgast. Turvaline nimelahendus on kõik teie majutatud pilveteenustega nõue.

On kahte tüüpi te peate tegelema nimelahendus.

- Sisemise nimelahendus – sisemise nimelahendus kasutab teenust Azure virtuaalse võrguga, kohapealse võrguga või mõlemad. Sisemise nimelahendus nimed ei pääse Interneti kaudu. Parima turvalisuse on oluline, et teie ettevõtte nimi eraldusvõime kava ei ole välistele kasutajatele kättesaadavaks.
- Välise nimelahendus – välise nimelahendus kasutavad inimesed ja seadmed väljaspool oma kohapealse ja Azure virtuaalse võrgu. Need on nimed, mis on nähtavad Interneti-ühendus ja kasutatakse ühenduse pilvepõhiseid teenuseid.

Sisemise nimelahendus, on teil kaks võimalust:

- Ettevõtte Azure virtuaalse võrgu DNS-i serveris – kui loote uue Azure virtuaalse võrgu, DNS-i server on teie jaoks loodud. See DNS-i server võib lahendada asub selles Azure virtuaalse võrgus masinad nimed. See DNS-i server ei saa konfigureerida ja haldab Azure struktuuri ülemus, mistõttu turvaline nimi eraldusvõime lahenduse.
- Tuua oma DNS-i server – teil on võimalus panna oma valiku Azure virtuaalse võrgu DNS-i serveri. See DNS-i server võib olla Active Directory integreeritud DNS-i server või sihtotstarbeline DNS-i server lahendus pakub Azure'i partner, mille saate hankida Azure'i turuplatsilt.

Lisateave:

- [Virtuaalse võrgu ülevaade](../virtual-network/virtual-networks-overview.md)
- [Halda DNS-serverid kasutada virtuaalse võrgu (VNet)](../virtual-network/virtual-networks-manage-dns-in-vnet.md)

Välise DNS-i resolvimise, on teil kaks võimalust:

- Majutada oma välise DNS-i server asutusesisese
- Majutada oma välise DNS-i server teenuse pakkuja

Paljude suurettevõtete kuvatakse majutada oma DNS-i serverid asutusesisese. Nad saavad teha võrgu teadmiste ja esindused seda teha.

Enamikul juhtudel on parem majutada oma DNS-i nimi eraldusvõime teenuste osutaja juures. Teenuse pakkuja on võrgu teadmiste ja esindused väga kõrge eraldusvõime nimi teenused kättesaadavuse tagamiseks. Kättesaadavus on oluline DNS-teenused, kuna kui nime eraldusvõime teenuste ei õnnestu, keegi ei jõudnud oma vastastikuste services Internet.

Azure'i pakub väga kättesaadav ja kiire välise DNS-i lahendus Azure'i DNS-i kujul. Selle välise nimi eraldusvõime lahenduse ära worldwide Azure'i DNS-i taristu. See võimaldab teil majutada oma domeeni Azure samasse mandaat, API-d, tööriistade abil ja arveldamine oma Azure teenused. Azure'i osana ka pärib platvormi sisse ehitatud tugev turbemeetmed.

Lisateave:

- [Azure'i DNS-i ülevaade](../dns/dns-overview.md)

## <a name="dmz-architecture"></a>DMZ arhitektuur
Mitme ettevõtte ettevõtted abil DMZs segmendi oma võrkude loomiseks puhvri zone Interneti- ja nende teenuste vahel. Võrgu DMZ osa peetakse madal turvalisus tsooni ja suure väärtusega varad paigutatakse selle võrgu lõigu. Tavaliselt näete turvalisus loovate võrguseadmete on võrgu liidese DMZ segmendi ja muu võrgu liidese ühendatud võrku, mis on virtuaalmasinates ja teenuseid, mis Aktsepteeri sissetulevaid ühendusi Interneti kaudu.

On mitmeid muudatuste juurutamiseks on DMZ DMZ kujundus ja seejärel DMZ kasutada, kui te otsustate kasutada ühte, mis tüüpi sõltub teie võrgu turbenõuetele.

Lisateave:

- [Microsofti pilveteenuste ja võrgu Turve](../best-practices-network-security.md)

## <a name="azure-security-center"></a>Azure'i turbekeskus
Turbekeskus aitab vältida, avastada ja ohtude vastamine ja pakub saate suurendada nähtavus, ja juhtida, oma Azure ressursse turvalisus. Integreeritud turvalisus jälgimine ja rühmapoliitika haldus pakub oma Azure tellimustes, aitab tuvastada ohud, mis võivad muidu märkamata ja töötab laialdane ökosüsteemi turvalisus lahendusi.

Azure'i turbekeskus aitab teil optimeerida ja jälgida võrgu turvalisuse järgi:

- Võrgu turvalisus soovituste andmine
- Võrgu konfiguratsioonist seisundi kontrollimine
- Lõpp-punkti ja võrgu network vastavalt ohtudega nii teavitamine

Lisateave:

- [Azure'i turbekeskus tutvustus](../security-center/security-center-intro.md)
