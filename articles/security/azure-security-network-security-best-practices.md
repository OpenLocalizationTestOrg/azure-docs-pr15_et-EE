<properties
   pageTitle="Azure'i võrgu parimad | Microsoft Azure'i"
   description="Selles artiklis antakse võrgu turvalisus sisseehitatud Azure võimaluste kasutamise head tavad kogum."
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="swadhwa"
   editor="TomShinder"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/25/2016"
   ms.author="TomSh"/>

# <a name="azure-network-security-best-practices"></a>Azure'i võrgu turvalisus head tavad

Microsoft Azure'i võimaldab virtuaalmasinates ja seadmete ühenduse muude võrguseadmete pannes Azure virtuaalse võrgu. Muu Azure virtuaalse kaudu on virtuaalse võrgu ehitada, mis võimaldab teil virtuaalse võrgu kasutajaliidese kaartide ühenduse virtuaalse võrgus, et lubada võrguseadmete vahelise suhtluse TCP/IP-ga. Azure'i Virtuaalmasinates Azure virtuaalse võrku ühendatud saavad seadmed samas Azure virtuaalse võrgus, erinevate Azure virtuaalse võrkude Internetis või isegi oma kohapealse võrkude ühendamiseks.

Selles artiklis me arutada Azure'i võrgu parimad kogum. Meie võrgunduse Azure kogemus on pärit järgmistest headest tavadest ja klientide kogemusi nagu ise.

Iga parimal viisil selgitame:

- Mis on parim
- Miks te soovite lubada selle parimaid tavasid
- Kui te ei luba parim, mis võib olla tingitud
- Võimalikud alternatiivid parim
- Kuidas saate teada, et lubada parim

Artiklit võrgu Azure'i parimad põhineb konsensuse arvates ja Azure'i platvormi võimaluste ja funktsioon komplekti ja olemasolevate see artikkel on kirjutatud ajal. Aja jooksul muutuda arvamust ja tehnoloogiad ja selles artiklis värskendatakse regulaarselt taguse.

Azure'i võrgu turvalisus head tavad selles artiklis kirjeldatud järgmised.

- Loogiliselt lõigu alamvõrku
- Juhtelemendi marsruutimise käitumine
- Jõustatud Tunneling lubamine
- Kasutage Virtual võrgu seadmed
- Turvalisus tsoneerimise DMZs juurutamine
- Vältida Interneti sihtotstarbeline WAN lingid
- Sees ja jõudluse optimeerimine
- Globaalne koormusetasakaalustuseks kasutamine
- Azure'i Virtuaalmasinates RDP juurdepääsu keelamine
- Luba Azure'i turbekeskus
- Azure'i sisse oma andmekeskuse laiendamine


## <a name="logically-segment-subnets"></a>Loogiliselt lõigu alamvõrku

[Azure'i virtuaalne võrkude](https://azure.microsoft.com/documentation/services/virtual-network/) sarnanevad kohapealse võrgu LAN. On Azure virtuaalse võrgu mõte on, et loote ühe IP address ruumi-põhiste privaatvõrk mille saab paigutada kõik [Azure'i Virtuaalmasinates](https://azure.microsoft.com/services/virtual-machines/). Privaatne IP address tühikuid saadaval on selle (10.0.0.0/8) A, B klassi (172.16.0.0/12) ja C-klassi vahemikud (192.168.0.0/16).

Sarnaselt mis kohapealse, mida teha, peaksite segmendi suuremat aadressiruumi alamvõrku sisse. Saate luua oma alamvõrku [CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) vastavalt subnetting põhimõtted.

Marsruutimise alamvõrku vahel juhtub automaatselt ja teil pole vaja käsitsi konfigureerida marsruudi tabelid. Vaikesäte on siiski alamvõrku, saate luua Azure virtuaalse võrgu vahel on pole võrgu Accessi juhtelemendid. Selleks, et luua võrgu juurdepääsu juhtelementide vahel alamvõrku, peate panna midagi selle alamvõrku vahel.

Üks asju, saate selle ülesande on [Võrgu turberühma](../virtual-network/virtual-networks-nsg.md) (NSG). NSGs on lihtne stateful paketi kontrollimise seadmete, mis kasutavad 5 kordne (allika IP, lähteport, sihtkoha IP, sihtport, ja layer 4 protocol) lähenemisviisi loomiseks lubada/keelata reeglite võrguliikluse jaoks. Saate lubada või keelata liiklus ja sealt ühe IP-aadress, ja mitu IP-aadressi või isegi ja kogu alamvõrku kaudu.

Võrgu juurdepääsu reguleerimine vahel alamvõrku NSGs kasutamise võimaldab teil panna ressursse, mis kuuluvad sama turvalisus tsooni või oma alamvõrku roll. Näiteks lihtsa 3-tasandi rakendus, mis on web taseme, kuvatakse rakenduse loogika taseme- ja andmebaasi taseme mõelda. Pange virtuaalmasinates, mis kuuluvad iga nende astme sisse oma alamvõrku. Seejärel kasutage NSGs määrata selle alamvõrku vahel:

- Web taseme virtuaalmasinates saab algatada ainult rakenduse loogika masinad ühendused ja ühendused Internetist nõustuda ainult
- Rakenduse loogika virtuaalmasinates saab algatada ainult andmebaasi taseme ühendusi ja saab ainult vastu web taseme ühendusi
- Andmebaasi taseme virtuaalmasinates ei saa algatada ühendust oma alamvõrgu väljaspool ja saab ainult aktsepteerige rakenduse loogika taseme ühenduse

Võrgu turberühmad ja kuidas saate neid kasutada, et loogiliselt segmendi Azure virtuaalse võrguga kohta lisateabe saamiseks lugege artiklit [mis on võrgu turberühma](../virtual-network/virtual-networks-nsg.md) (NSG).

## <a name="control-routing-behavior"></a>Juhtelemendi marsruutimise käitumine

Kui lisate virtuaalse masina Azure virtuaalse võrgus, märkate, et virtuaalse masina saate luua ühenduse virtuaalse masina samas Azure virtuaalse võrgus, isegi juhul, kui muud virtuaalmasinates on erinevat alamvõrku. Põhjus, miks see on võimalik, et on süsteemi marsruudib, mis on vaikimisi luba seda tüüpi side kogum. Vaikimisi lennuliinide luba virtuaalmasinates sama Azure virtuaalse võrgu algatada ühendused omavahel ja Internetiga (jaoks väljaminevaks andmesideks ainult Interneti-ühendus).

Kuigi vaikimisi süsteemi marsruudib on kasulik tööiga juurutus, on soovite kohandada oma juurutuste marsruutimine konfigureerimine. Need kohandused saate konfigureerida järgmise sõnumihüppe kohta, pärast aadressi teatud sihtkohta jõudmiseks.

Soovitame konfigureerida kasutaja määratletud marsruudib juurutamisel virtuaalse võrgu turvalisus seade, mida me hiljem heade tavade kohta rääkida.

> [AZURE.NOTE] kasutaja määratletud marsruudib vajalike ja vaikimisi süsteemi marsruudib töötab enamikel juhtudel.

Te saate lisateavet kasutaja määratletud marsruudib ja konfigureerimine nende artiklist, [mis on kasutaja määratletud marsruudib ja IP edastamine](../virtual-network/virtual-networks-udr-overview.md).

## <a name="enable-forced-tunneling"></a>Jõustatud Tunneling lubamine

Paremini mõistmiseks jõustatud tunneling, on kasulik, et aru saada, milliseid "tükeldatud tunneling" on.
Kõige levinum näide tükeldatud tunneling on näha VPN-ühendused. Oletage, et luua oma ettevõtte võrku teie VPN-ühendus. See ühendus võimaldab ressursse teie ettevõtte võrguga ühenduse ja teie ettevõtte võrgus ressursid kõik teatised läbida VPN tunneliga.

Mis juhtub, kui soovite ühenduse ressursid Internetis? Kui tükeldatud tunneling on lubatud, valige need ühendused otse Interneti- ja mitte läbi VPN tunneliga. Mõned turvalisus eksperdid võtke arvesse, et see võimalik risk ja Seetõttu soovitame, et tükeldatud tunneling keelatud ja kõik ühendused, mõeldud Interneti ja ettevõtte ressursid, mõeldud läbida VPN tunneliga. On see eelis, teha seda, et ühendus on seejärel läheb läbi ettevõtte võrgus turvalisus seadmeid, mis ei oleks juhul, kui Interneti-ühendus väljaspool VPN tunneliga VPN klient.

Nüüd vaatame tuua tagasi nii virtuaalse võrgus Azure'i virtuaalmasinates. Vaikimisi marsruudib Azure virtuaalse võrgu jaoks lubada virtuaalmasinates Interneti-liikluse algatada. See liiga saate kujutada ohtu turvalisusele, nagu need Väljaminevad ühendused tõstaks virtuaalse masina rünnak pind ja saab kasutada turvaohte.
Seetõttu soovitame teil lubada jõustatud tunneling sisse oma virtuaalmasinates kui teil on asutusesiseses Ühenduvus Azure virtuaalse võrgu ja kohapealse võrgu vahel. Me rääkida rist tööruumide ühenduvuse hiljem selles dokumendis Azure võrgu head tavad.

Kui teil on, rist tööruumide ühendust, veenduge, et te ära võrgu turberühmad (eespool) või Azure virtuaalse võrgu vältimiseks Väljaminevad ühendused Interneti kaudu oma Azure'i Virtuaalmasinates turvalisus seadmed (arutatakse edasi).

Lisateavet sunnitud tunneliläbindus ja kuidas seda lubada, lugege artiklit [konfigureerimine sunnitud Tunneling PowerShelli ja Azure ressursihaldur abil](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md).

## <a name="use-virtual-network-appliances"></a>Kasutage virtuaalse võrgu seadmed

Võrgu turberühmad ja kasutaja määratletud marsruutimine võib küll võrgu turvalisus võrk ja transport kihid [OSI mudeli](https://en.wikipedia.org/wiki/OSI_model)mõõtmine, seal saab olukordades, kus soovite või turvalisuse kõrgete virnas lubamiseks peate olema. Sellisel juhul soovitame juurutada virtuaalse võrgu turvalisus seadmete Azure partnerite järgi.

Azure'i võrgu turvalisus seadmete pakkuda märkimisväärselt turvalisus taset üle, mis on esitatud võrgu taseme juhtelemente. Mõned võrgu turvalisus võimaluste esitatud virtuaalse võrgu turvalisus seadmed on järgmised:

- Firewalling
- Sissetungi tuvastamise/sissetungimise vältimine
- Turvariskide juhtimine
- Rakenduste kontroll
- Võrgu-põhise normaalne automaattuvastus
- Veebi filtreerimine
- Viirusetõrje
- Robotivõrgutõrje

Kui vajate võrgu turvalisuse kõrgema kui saate hankida võrgu juurdepääsutase juhtelementidega, siis soovitame uurida ja juurutada Azure virtuaalse võrgu turvalisus seadmed.

Saate teada, millised Azure virtuaalse võrgu turvalisus on saadaval ning nende võimaluste kohta, külastage [Azure'i turuplatsi](https://azure.microsoft.com/marketplace/) ja otsida "Turvalisus" ja "võrgu turvalisus".

##<a name="deploy-dmzs-for-security-zoning"></a>Turvalisus tsoneerimise DMZs juurutamine
Mõne DMZ või "perimeetri võrk" on füüsiline või loogiline võrgu lõiku, mis on loodud mõne täiendavad kiht vahel oma varasid ja Interneti-ühendus. DMZ soovidele on viia eriotstarbelisi võrguseadmete juurdepääsu juhtimine serva DMZ võrgu nii, et ainult soovitud liikluse on lubatud viimase võrgu turvalisus seade ja teie Azure virtuaalse võrku.

DMZs on kasulik, kuna teie võrgu juurdepääsu juhtimine juhtimise, saate keskenduda järelevalve, logimine ja seadmete Azure virtuaalse võrgu servast. Siin saate tavaliselt lubada DDoS vältimise, sissetungi tuvastamise/sissetungimise vältimine süsteemid (ID ja IP-d), tulemüüri reeglid ja poliitikad, veebi filtreerimine, võrgu ründevaratõrje ja muu. Turvalisus võrguseadmete istuda Interneti-ja Azure virtuaalse võrgu ja on nii liides.

Kuigi see on lihtne kujunduse mõne DMZ, on palju erinevaid DMZ kujundusi, näiteks migreerimist, kolmeks kasutajaid, mitme kasutajaid ja teised.

Soovitame kõik turvatud juurutuste kaaluge juurutamine DMZ, suurendab võrgu turvalisuse Azure ressurssidena.

DMZs ja nende Azure juurutamise kohta lisateabe saamiseks lugege artiklit [Microsofti pilveteenustega ja võrgu Turve](../best-practices-network-security.md).

## <a name="avoid-exposure-to-the-internet-with-dedicated-wan-links"></a>Vältida Interneti sihtotstarbeline WAN lingid
Paljud organisatsioonid valinud hübriid selle protsessi. Hübriid, mõned ettevõtte teave varad on Azure, samas kui teised jäävad kohapealse. Paljudel juhtudel teatud komponendid teenuse töötab Azure muude komponentide jäävad kohapealse.

See stsenaarium hübriid on tavaliselt teatud tüüpi asutusesiseses Ühenduvus. See asutusesiseses Ühenduvus võimaldab ettevõttel oma kohapealse võrgu Azure virtuaalne võrkude ühendamiseks. See on saadaval kaks asukohaga ühenduvuse lahendusi.

- VPN saidilt
- ExpressRoute

[Saidi saidi VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md) tähistab virtuaalse privaatse seost kohapealse võrgu ja muu Azure virtuaalse kaudu. See ühendus toimub Interneti kaudu ja võimaldab teil "tunneliga" teave krüptitud seost oma võrgu ja Azure sees. Saidi saidi VPN on turvaline, küps tehnoloogia, mis on juurutatud igas suuruses ettevõtete jaoks aastat. Tunneliga krüptimise sooritatakse [IPsec tunneliga režiimi](https://technet.microsoft.com/library/cc786385.aspx)kasutamine.

Usaldusväärsete, usaldusväärseid ja loodud tehnoloogia ajal-saidilt VPN liikluse soovitud tunneliga läbida Interneti-ühendus. Lisaks on üsna läbilaskevõime piiratud kuni 200Mbps kohta.

Kui vajate oma asutusesiseses ühenduste erakorralised järgu turvalisus või jõudluse, soovitame kasutada Azure ExpressRoute oma asutusesiseses Ühenduvus. ExpressRoute on spetsiaalne WAN seost kohapealse asukoha või Exchange'i majutusteenuse pakkuja. Kuna tegemist on telco ühenduse, andmete ei reisi Interneti kaudu ja seetõttu on avatud Interneti-side seotud võimalike ohtudega.

Azure'i ExpressRoute tööpõhimõte ja juurutamise kohta lisateabe saamiseks lugege artiklit [ExpressRoute tehniline ülevaade](../expressroute/expressroute-introduction.md).

## <a name="optimize-uptime-and-performance"></a>Sees ja jõudluse optimeerimine
Konfidentsiaalsus, terviklus ja kättesaadavus (CIA) koosnevad triaad tänase kõige tegur turvalisuse mudel. Privaatsusavalduse jõustamine on kohta, kuidas veenduda, et andmed ei muutu volitamata isikud ja kättesaadavus on kohta, kuidas veenduda, et volitatud isikud on neil on juurdepääs teabe juurde pääseda konfidentsiaalsusega on krüptimise kohta. Tõrge, mis tahes neid valdkondi tuleks tähistab rikub turvalisus.

Kättesaadavus saate mõtlesin on sees ja jõudluse kohta. Kui teenus ei tööta, ei pääse teavet. Kui on nii halb, et andmed on kasutuskõlbmatuks, siis saame uurida, et kättesaamatuks andmed. Seetõttu turvalisuse seisukohalt peame mida iganes me ei veenduge, et meie teenuste on optimaalse sees ning jõudlust.
Kättesaadavus ja jõudluse parandamiseks kasutada populaarne ja tõhus meetod on kasutada koormusetasakaalustuseks. Koormusetasakaalustuseks on meetod levitamise serverites, mis on osa teenuse võrguliiklust. Näiteks, kui teil on veebi serverid raames teenust, saate koormusetasakaalustuseks levitada liikluse oma mitme veebi serverites.

Selle jaotuse liikluse suurendab kättesaadavus Kuna kui üks web-server pole saadaval, laadi koormusetasakaalustusteenuse kuvatakse serveri liikluse saatmise lõpetada ja serverid, mis on endiselt võrgus liikluse ümber suunata. Koormusetasakaalustuseks aitab jõudluse, kuna protsessor, võrgu ja mälu pea kohal serveeritakse taotlusi levitatakse üle kõik laadi tasakaalustatud serverites.

Soovitame, et te tööle koormusetasakaalustuseks iga kord, kui saate ja teenuste jaoks. Saadame asjakohasuse järgnevalt.
Azure'i pakub tasemel Azure virtuaalse võrgu koos kolme esmase laadimist tasakaalustavad suvandid.

- HTTP-põhine laadi tasakaalustamiseks
- Välise koormuse tasakaalustamiseks
- Sisemise tasakaalustamiseks laadimine

## <a name="http-based-load-balancing"></a>HTTP-põhine laadi tasakaalustamiseks
HTTP-põhise koormusetasakaalustuseks alused otsuseid selle kohta, mida server saata ühenduste abil omaduste HTTP-protokolli. Azure'i on HTTP laadi koormusetasakaalustusteenuse, mis läheb rakenduse lüüsi nimi.

Soovitame teil meile Azure'i rakenduse lüüsi kui:

- Rakendusi, mis nõuavad taotlusi sama/klient seansi saavutamiseks tagaandmebaas virtual samasse arvutisse. Näiteks soovite ostmine ostukorv rakenduste ja web meiliserverid.
- Rakendusi, mis soovite tasuta rakenduse lüüsi [SSL offload](https://f5.com/glossary/ssl-offloading) funktsiooni ära web serveri parkides SSL-i lõpetamise pea kohal.
- Sisuedastusvõrgud, näiteks, mis nõuavad mitu HTTP päringuid sama pikaajalisi TCP ühendust marsruutida või laadimine tasakaalus serveritest tagaandmebaas.

Azure'i rakenduse lüüsi tööpõhimõte ja kuidas saate seda oma juurutuste kohta leiate lisateavet lugege artiklist [Rakenduste lüüsi ülevaade](../application-gateway/application-gateway-introduction.md).

## <a name="external-load-balancing"></a>Välise koormuse tasakaalustamiseks
Välise koormusetasakaalustuseks toimub, kui sissetulevaid ühendusi Interneti kaudu on koormus tasakaalustatud oma serverid asub Azure virtuaalse võrgus. Azure'i välise koormuse koormusetasakaalustusteenuse annavad teile seda võimalust ja soovitatav, kui te ei vaja sticky seansid või SSL offload, seda kasutada.

Erinevalt HTTP-põhise koormusetasakaalustuseks, välise laadimine koormusetasakaalustusteenuse kasutab teavet veebisaidil võrk ja transport kihid OSI võrgu mudeli otsustada, millist serverisse laadida saldo ühenduse.

Soovitame kasutada välise Koormusetasakaalustuseks iga kord, kui teil on [kodakondsuseta rakenduste](http://whatis.techtarget.com/definition/stateless-app) vastu võtmist sissetulevad taotlused Interneti kaudu.

Lisateave Azure'i välise laadi koormusetasakaalustusteenuse tööpõhimõtete kohta ja kuidas saate juurutamist, Palun lugege artiklit [loomise Internet vastastikuste laadi koormusetasakaalustusteenuse rakenduses ressursihaldur PowerShelli kaudu kasutamise alustamine](../load-balancer/load-balancer-get-started-internet-arm-ps.md).

## <a name="internal-load-balancing"></a>Sisemise tasakaalustamiseks laadimine
Sisemise koormusetasakaalustuseks on sarnane välise koormusetasakaalustuseks ja kasutab sama süsteemi laadimine serveritele taga saldo ühendusi. Ainus erinevus on, et laadi koormusetasakaalustusteenuse sel juhul ei aktsepteeri ühendusi virtuaalmasinates, mis pole Interneti kaudu. Enamikul juhtudel on ühendused, mis on lubatud koormusetasakaalustuseks algataja virtuaalse võrgus Azure'i seadmetes.

Soovitame kasutada sisemise laadi stsenaariumi, mis saavad seda võimalust, näiteks kui tuleb laadimiseks saldo ühendused SQL-i serverite või sisemise web servers jaoks.

Lisateavet Azure sisemine Koormusetasakaalustuseks tööpõhimõte ja kuidas saate juurutada, leiate artiklist [loomise on sisemine laadi koormusetasakaalustusteenuse PowerShelli kaudu kasutamise alustamine](../load-balancer/load-balancer-get-started-internet-arm-ps.md#update-an-existing-load-balancer).

## <a name="use-global-load-balancing"></a>Globaalne koormusetasakaalustuseks kasutamine
Avalik pilv arvuti muudab on võimalik juurutamine globaalselt jaotatud rakendused on components asuv andmekeskuste kogu maailmas. See on võimalik Microsoft Azure Azure globaalne andmekeskuse kohaloleku tõttu. Vastupidiselt laadi tasakaalustamiseks tehnoloogiad varem mainitud, globaalne laadi tasakaalustamiseks võimaldab teenuste kättesaadavaks ka siis, kui kogu andmekeskuste võib kättesaamatuks muutuda.

Saate seda tüüpi globaalne laadi tasakaalustamiseks Azure ära [Azure'i liikluse haldur](https://azure.microsoft.com/documentation/services/traffic-manager/). Liikluse haldur teeb on võimalik laadimine saldo ühendused oma teenuste kasutaja asukoha alusel.

Näiteks kui kasutaja on taotluse teenust Liidust, suunatakse ühendus mõne EU andmekeskuse asub teenuste. See osa, liikluse haldur globaalne laadimine tasakaalustavad aitab jõudluse parandamiseks, kuna ühenduse loomine lähima andmekeskusega on kiiremini ühenduse andmekeskuste, mis on eemal.

Kättesaadavus pool globaalne koormusetasakaalustuseks muudab kindel, et teie on saadaval ka siis, kui ka kogu andmekeskuse peaks muutuvad kättesaadavaks.

Näiteks kui ka Azure andmekeskuse peaks muutuma keskkond tõttu katkestuste (nt piirkondliku võrgu ebaõnnestumist) või pole saadaval, ühendused teenust peaks olema proovivad selle lähima Online'i andmekeskuse. See globaalne koormusetasakaalustuseks täidetakse ära DNS-i poliitikad, mida saate luua liikluse haldur.

Soovitame kasutada liikluse haldur pilve lahenduse, arendate suuresti jaotatud ulatus on mitu piirkondade ja nõuab kõrgeima taseme võimalikud sees.

Azure'i liikluse juhataja ja selle juurutamise kohta lisateabe saamiseks lugege artiklit [mis on liikluse haldur](../traffic-manager/traffic-manager-overview.md).

## <a name="disable-rdpssh-access-to-azure-virtual-machines"></a>Keelake RDP SSH juurdepääs Azure'i Virtuaalmasinates
On võimalik jõuda Azure'i Virtuaalmasinates [Kaugtöölaua protokolli](https://en.wikipedia.org/wiki/Remote_Desktop_Protocol) (RDP) ja protokollid [Secure Shell](https://en.wikipedia.org/wiki/Secure_Shell) (SSH) abil. Need protokollid võimaldavad haldamiseks virtuaalmasinates remote asukohtadest ja on andmekeskuse arvuti.

Võimalikud turvalisus probleem Interneti kaudu nende Protokollid kasutades on ründajad saate mitmesuguste [jõuvõtete](https://en.wikipedia.org/wiki/Brute-force_attack) tehnikate pääsete Azure'i Virtuaalmasinates. Kui ründajad pääse, nad kasutada arvuti virtuaalne käivitada punkti kompromisse muud seadmed Azure virtuaalse võrgus või isegi rünnata võrguseadmete Azure'i väljaspool.

Seetõttu soovitame keelata RDP ja SSH otsest juurdepääsu oma Azure'i Virtuaalmasinates Interneti kaudu. Pärast RDP ja SSH pääseb otse Interneti-ühendus on keelatud, on teil muude suvandite abil saate need virtuaalmasinates serveri haldamise juurdepääsu.

- Punkti saidi VPN
- VPN saidilt
- ExpressRoute

[Punkti saidi VPN](../vpn-gateway/vpn-gateway-point-to-site-create.md) on teine tähtaeg Kaugpöördusteenuse kliendi ja serveri VPN-ühendus. Punkti saidi VPN võimaldab üksikkasutaja Azure virtuaalse võrguga ühenduse Interneti kaudu. Pärast punkti saidi ühendus on loodud, kasutaja saab kasutada mis tahes virtuaalmasinates paiknevad Azure virtuaalse punkti saidi VPN kaudu ühendatud kasutaja ühenduse RDP või SSH. See eeldab, et need virtuaalmasinates jõudmiseks, et kasutaja on lubatud.

Punkti saidi VPN on otsese RDP või SSH ühendused turvalisemaks, kuna kasutaja on enne ühenduse virtuaalse masina autentida. Kõigepealt peab kasutaja autentida (ja lubada) luua punkti saidi VPN-ühendus; Teiseks peab kasutaja autentida (ja lubada) RDP või SSH seansi loomiseks.

[Saidi saidi VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md) loob kogu võrgu teise võrku Interneti kaudu. Saate ka Azure virtuaalse võrguga ühenduse loomiseks kohapealse võrgu-saidilt VPN. Kui juurutate-saidilt VPN, kasutajate kohta kohapealse võrgu saab ühenduse virtuaalse võrgus Azure'i virtuaalmasinates RDP või SSH protokolli abil-saidilt VPN-ühenduse ja ei nõua otsest RDP või SSH juurdepääsu lubamiseks Interneti kaudu.

Sihtotstarbeline WAN-lingi abil pakkuda funktsionaalsust, mis on sarnane VPN saidilt. Peamised erinevused on 1. Sihtotstarbeline WAN-lingi ei läbida Interneti ja 2. Sihtotstarbeline WAN lingid on tavaliselt rohkem stabiilne ja kiire. Azure'i pakub teile asjakohast WAN-lingi lahenduse [ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/)kujul.

## <a name="enable-azure-security-center"></a>Luba Azure'i turbekeskus
Azure'i turbekeskus aitab vältida, avastada ja ohtude vastamine ja pakub saate suurendada nähtavus, ja juhtida, oma Azure ressursse turvalisus. Integreeritud turvalisus jälgimine ja rühmapoliitika haldus pakub oma Azure tellimustes, aitab tuvastada ohud, mis võivad muidu märkamata ja töötab laialdane ökosüsteemi turvalisus lahendusi.

Azure'i turbekeskus aitab teil optimeerida ja jälgida võrgu turvalisuse järgi:

- Võrgu turvalisus soovituste andmine
- Võrgu konfiguratsioonist seisundi kontrollimine
- Lõpp-punkti ja võrgu network vastavalt ohtudega nii teavitamine

Soovitame, et lubada Azure turbekeskus kõigi oma Azure juurutuste.

Azure'i turbekeskus ja kuidas saaks teie juurutuste kohta leiate lisateavet lugege artiklist [Sissejuhatus Azure'i turbekeskus](../security-center/security-center-intro.md).

## <a name="securely-extend-your-datacenter-into-azure"></a>Laiendage oma andmekeskuse turvaliselt Azure
Mitme ettevõtte IT ettevõtted otsivad laiendada asemel kasvavad oma kohapealse andmekeskuste pilve. See tähistab IT taristu laiendamine avaliku pilve. Ära asutusesiseses ühenduvuse suvandid on võimalik Azure virtuaalse võrguga käsitleda vaid mõne muu alamvõrgu kohta kohapealse võrgu taristule.

Siiski on palju kavandamise ja kujunduse probleemid, mis tuleb esmalt lahendada. See on eriti oluline võrgu turvalisuse. Üks parimaid viise, et aru saada, kuidas te lähenemine selline kujundus on näide.

Microsoft on loonud [Andmekeskuse laiend viide arhitektuur skeem](https://gallery.technet.microsoft.com/Datacenter-extension-687b1d84#content) ja toetavad tagatise, et aidata teil mõista, kuidas selline andmekeskuse laiendamine näeks. See pakub näide viide rakendamist, mida saate kasutada plaanimine ja kujundamine turvaline enterprise andmekeskuse laiend pilveteenusesse. Soovitame teil läbi vaadata selle dokumendi saada aimu, milline põhikomponentide turvaline lahendus.

Lisateavet selle kohta, kuidas laiendada turvaliselt oma andmekeskuse Azure'i sisse palun vaadake video [Ulatub teie andmekeskuse Microsoft Azure](https://www.youtube.com/watch?v=Th1oQQCb2KA).
