<properties
   pageTitle="Kõrge-saadavus Azure rakenduste | Microsoft Azure'i"
   description="Tehniline ülevaade ja kujundamise ja rakenduste jaoks kõrge-saadavus loomine Microsoft Azure kohta täpsemat teavet."
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

#<a name="high-availability-for-applications-built-on-microsoft-azure"></a>Microsoft Azure loodud kõrge kättesaadavus

Väga saadaolevate rakenduste neelab kättesaadavus, laadi ja sõltuvad teenused ja riistvara ajutine ebaõnnestumist. Rakenduse jätkuvalt töötamist on aktsepteeritav kasutaja ja süsteemne tagasiside tase, äri nõuetele või rakenduse teenusetaseme lepinguid (SLAs) määratletud.

##<a name="azure-high-availability-features"></a>Azure'i kõrge-saadavus funktsioonid

Azure'i on palju sisseehitatud platvormi funktsioone, mis toetavad tugevalt saadaolevad rakendused. Selles jaotises kirjeldatakse neid olulisi funktsioone. Põhjalikumat analüüsi platvormi, leiate [Azure'i paindlikkust tehnilised juhendid](./resiliency-technical-guidance.md).

###<a name="fabric-controller"></a>Selle domeenikontrolleri struktuuri

Azure'i struktuuri kontrolleril jälgib Azure Arvuta eksemplaride tingimus ja sätted. Struktuuri kontrolleril kontrollib riistvara olekut ja tarkvara host ja külalistele masina eksemplaride. Kui seda ei saa, jõustab SLAs paigutades automaatselt VM juhtudel. Viga ja täiendamine domeene põhjalikumaks mõistet toetab Arvuta SLA.

Mitme pilveteenuses rolli aknad juurutamisel Azure juurutamine juhul erinevate viga domeenid. Viga domeeni piiri on põhiliselt sama piirkonna erinevaid riistvara sektsioonis. Viga domeenide vähendada tõenäosuse, et lokaliseeritud riistvara tõrke katkestab teenuse rakenduse. Te ei saa hallata arv viga domeenid, mis on eraldatud töötaja või web rollid. Struktuuri domeenikontrolleri kasutab asjakohast ressursse, mis on Azure majutatava rakendustest eraldi. Kuna see on Azure süsteemi tuum on 100% sees. See jälgib ja haldab viga domeenides rolli aknad.

Järgmisel joonisel on esitatud Azure jagatud ressursid, et struktuuri domeenikontrolleri kasutab ja haldab eri viga domeenides.

![Viga domeeni eraldustaseme lihtsustatud vaade](./media/resiliency-high-availability-azure-applications/fault-domain-isolation.png)

Täiendamine on sarnane viga domeenide funktsiooni, kuid nad ei toeta uuendamine, mitte ebaõnnestumist. Kuvatakse versioonitäienduse domeen on loogilise üksuse eksemplari eraldamine, mis määratleb, milliseid eksemplarid teatud teenuses täiendatakse samal ajal aega. Vaikimisi juurutamiseks majutatud teenus viie täiendamine domeenide on määratletud. Siiski saate muuta selle teenuse definitsioonifail väärtuse. Näiteks Oletame, et teil on kaheksa eksemplari oma web rolli. Ilmneb kaks versiooni täiendamise kolme domeenides ja kaks versiooni täiendamine üks domeen. Azure'i määratleb update järjekorra, kuid see põhineb täiendamine domeenide arv. Täiendamine domeenide kohta leiate lisateavet teemast [värskendamine mõnda pilveteenusesse](../cloud-services/cloud-services-update-azure-service.md).

###<a name="features-in-other-services"></a>Muude teenuste funktsioonid

Lisaks platvormi funktsioone, mis toetavad kõrge Arvuta-saadavus, manustab Azure'i oma teenustesse kõrge-saadavus funktsioone. Näiteks Azure Storage säilitab bloobimälu, tabeli ja järjekorda andmete kolme koopiad. See lubab ka geo-dispersioonanalüüs võimalus talletada varukoopiate plekid ja tabelite teisene piirkonnas. Azure'i sisu kohaletoimetamise võrk võimaldab plekid kopeerida kogu maailmas nii koondamise ja skaleeritavus. Azure'i SQL-andmebaasi leiab ka mitu koopiad.

Vt lisaks [paindlikkust tehnilised juhendid](https://aka.ms/bctechguide) rea artikleid, [Head tavad suuremahuliste kujundus teenuste Azure'i pilveteenustega](https://azure.microsoft.com/blog/best-practices-for-designing-large-scale-services-on-windows-azure/) paberi. Need dokumendid pakuvad süvitsi arutelu Azure'i platvormi kättesaadavus funktsioone.

Kuigi Azure pakub mitme funktsioone, mis toetavad kättesaadavuse, on oluline mõista nende piirangud:

- Puhul Arvuta, Azure'i tagab, et oma ülesanded on saadaval ja töötavad, kuid ei tea, kas teie taotlus on käivitatud või ülekoormatud.
- Azure'i SQL-andmebaasi jaoks on andmed kopeeritud sünkroonselt piirkonnas. Soovi korral saate aktiivse geo-kopeerimine, mis võimaldab kuni nelja täiendavad andmebaasi koopiate sama piirkonna (või eri regioonide). Need andmebaasi koopiad ei ole punkti /-kellaaja varukoopiad. SQL-i andmebaasid andma punkti õigel ajal varukoopia võimalusi. SQL-i andmebaasi punkti /-kellaaja võimaluste kohta lisateabe saamiseks lugege [Azure SQL-i andmebaasi punkti aja taastamine](https://azure.microsoft.com/blog/azure-sql-database-point-in-time-restore/).
- Azure Storage, on kopeeritud andmete tabeli ja bloobimälu vaikimisi alternatiivse piirkonnale. Siiski ei pääse selle koopiad, kuni Microsofti valib kasutada alternatiivse saidile. Piirkonna Tõrkesiirde ainult juhul, kui pikaajaline piirkond kogu teenuse häirete ja on puudub SLA geo-Tõrkesiirde aega. See on oluline märkida, et kõik ulatub kiiresti selle koopiad.

Nende huvides peab täiendavad platvormi kättesaadavus funktsioonid rakenduse kohased-saadavus funktsioone. Rakenduse kohased-saadavus funktsioonide hulka bloobimälu pildi funktsioon punkti /-kellaaja varukoopiate bloobimälu andmete loomiseks.

###<a name="availability-sets-for-azure-virtual-machines"></a>Kättesaadavus määrab Azure'i Virtuaalmasinates

Enamik selles artiklis keskendutakse pilveteenused, kasutatakse platvormi service (PaaS) mudel. Kuid on ka kättesaadavus teatud funktsioonide jaoks Azure'i Virtuaalmasinates, mis kasutab infrastruktuuri teenus (IaaS) mudel. Kõrge kättesaadavuse Virtuaalmasinates saavutamiseks peate kasutama kättesaadavus komplektid. Mõne kättesaadavus Sea pakutakse sarnane funktsioon viga ja versioonitäienduse domeenid. Kättesaadavus on määratud, paigutab Azure'i virtuaalmasinates on nii, et kõik selle rühma masinad langetamine lokaliseeritud riistvara vead ja hooldustegevused takistab. Kättesaadavus komplektid on vajalik SLA Azure'i Virtuaalmasinates olemasolu.

Järgmisel joonisel pakub kahte kättesaadavus kujutis selle rühma web ja SQL serveri virtuaalmasinates, vastavalt.

![Kättesaadavus määrab Azure'i Virtuaalmasinates](./media/resiliency-high-availability-azure-applications/availability-set-for-azure-virtual-machines.png)

>[AZURE.NOTE] SQL serveri eelmisel joonisel on installitud ja käivitatud virtuaalmasinates kohta. See erineb Azure'i SQL-andmebaasi, mis pakub hallatavate teenuste andmebaasi eelmise arutelu.

##<a name="application-strategies-for-high-availability"></a>Rakenduse strateegiad kõrge-saadavus

Enamik rakenduse strateegiad kõrge-saadavus kaasata koondamise või eemaldamise raske sõltuvuste komponendid. Rakenduse kujundus toetama tõrketaluvust ajal juhuslik tööseisakute Azure'i või muu tootja teenused. Järgmistes jaotistes kirjeldatakse rakenduse mustrid oma pilveteenustega kättesaadavamaks.

###<a name="asynchronous-communication-and-durable-queues"></a>Asünkroonne suhtlemine ja püsival järjekorrad

Kaaluge asünkroonne suhtlemine lõdvalt seotud teenuste Azure rakendustes kättesaadavuse parandamiseks. Selle muster, kirjutage sõnumite salvestusruumi järjekorrad või Azure'i teenus siini järjekorrad hiljem töötlemiseks. Kui kirjutate sõnumi järjekorda, tagastab juhtelemendi kohe sõnumi saatja. Teise taseme rakenduse tegeleb sõnumi töötlemine, tavaliselt rakendada töötaja roll. Kui töötaja roll läheb alla, sõnumite koguda järjekorras kuni töötluse teenus on taastatud. Kui järjekord on saadaval, on pole otsene sõltuvus ees saatja ja sõnumi protsessor vahel. See kaob sünkroonse teenuse kõnesid, mis võivad olla läbilaskevõime kitsaskoht jaotatud rakendustes nõue.

Selle muudatuse kasutab Azure Storage (plekid, tabelid, järjekorrad) või teenuse siini järjekordade Tõrkesiirde asukoht nurjunud andmebaasi kõnede jaoks. Näiteks sünkroonse kõne rakenduses muusse teenusesse (nt Azure'i SQL-andmebaasi) ei ole korduvalt. Võimalik serialiseerida andmed üheks püsival salvestusruumi. Hiljem kui kasutatava teenuse või andmebaas on võrguühenduse taastamisel rakenduse saab uuesti esitama kaudu salvestusruumi kutse. See mudel vahe on vahe asukoht ei ole pidev osa rakenduse töövoog. Seda kasutatakse ainult tõrge stsenaariume.

Mõlemal juhul asünkroonne suhtlemine ja vahe salvestusruumi takistada downed tagaandmebaas teenuse kogu rakenduse langetamine. Järjekorrad olla loogiline vahendusel. Veel juhised õige andmebaasitõrge teenuse valimine, leiate [Azure'i järjekorrad ja Azure teenuse siini järjekorrad--võrreldes ja ja](../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md).

###<a name="fault-detection-and-retry-logic"></a>Vea tuvastamise ja proovige uuesti loogika

Oluline punkt rakenduse väga kättesaadav kujunduses on kasutada proovi uuesti loogika koodi käsitlema nõtkelt teenus, mis on ajutiselt maha. [Siirdamiseks viga töötlemise rakenduse blokeerimine](https://msdn.microsoft.com/library/hh680934.aspx), Microsoft Patterns ja tavad meeskond, aitab selle protsessi rakenduste arendajatele. Sõna "mööduv" tähendab, et ajutine seisund, mis kestab ainult suhteliselt lühikest aega. Selles artiklis kontekstis töötlemise siirdamiseks tõrkeid on osa tugevalt saadaolevate rakenduste arendamise. Siirdamiseks tingimuste näited vahelduva võrgu tõrkeid ja andmebaasi ühendused kaotsi.

Siirdamiseks viga töötlemise rakenduse plokk on lihtsustatud viis graatsiline töödelda oma koodi ebaõnnestumist. Saate seda kasutada, lisades töökindlad siirdamiseks viga käsitlemine loogika rakenduste kättesaadavuse parandamiseks. Enamikul juhtudel uuesti loogika tegeleb lühike katkestamine ja taastatakse ühendus pärast ühe või mitme nurjunud katsete saatja ega vastuvõtja pilti. Eduka proovi uuesti katse tavaliselt märkamata rakenduse kasutajatele.

Arendajad on kolm võimalust hallata oma proovi uuesti loogika: suureneva, fikseeritud intervall, ja eksponendi. Suureneva ootab enam enne iga uuesti suurenevad lineaarse viisil (nt 1, 2, 3 ja 4 sekundites). Fikseeritud intervalliga ootab sama palju aega vahel proovi uuesti (nt kaks sekundit all). Veel juhusliku suvandi, kuvatakse eksponentsiaalse tagasi-välja ei pea enam korduste vahel. Kasutab aga eksponentsiaalse käitumine (nt 2, 4, 8 – 16 sekundites).

Üksikasjalik strateegia oma koodi on:

1. Määratleda uuesti strateegia ja poliitika.
1. Proovige toimingut, mis võib põhjustada siirdamiseks viga.
1. Siirdamiseks vea juhul autonoomsest uuesti poliitika.
1. Kui kõik korduskatsed ei õnnestu, jaole juba lõplik erand.

Testige oma proovi uuesti loogika jäljendatud tõrkeid, tagamaks, et korduskatsed järjestikuste toimingute tulemuseks on esitatavatele ootamatutele pikk viivitus. Tehke seda enne nurjumise üldine ülesanne.

###<a name="reference-data-pattern-for-high-availability"></a>Viide andmete mustri kõrge-saadavus

Andmed on kirjutuskaitstud andmed rakenduse. Andmed on toodud business kontekstis, kus loob rakendus kandeandmete business toimingu käigus. Kandeandmete on viide andmete punkti /-kellaaja funktsioon. Seega, sõltub selle terviklus viide andmete hetktõmmist tehingu ajal. See on mõnevõrra lahti määratlus, kuid see peaks piisab meie eesmärgil siin.

Rakenduse kontekstis lähteandmed on vajalik rakendus toimimist. Vastava rakendusega luua ja hallata andmeid; põhiandmete halduse (MDM-i) süsteemide sageli seda funktsiooni. Need süsteemid on viide andmete elutsükli eest vastutav. Lähteandmed näiteks tootekataloogi, töötaja juhtslaidi, osade juhtslaidi ja seadmed juhtslaidi. Andmeid saate pärinevad väljaspool asutust, nt sihtnumbrite või käibemaksu määra. Andmete kättesaadavuse strateegiad on tavaliselt keeruline kui kandeandmete. Viide andmetel on see eelis, on enamasti püsiv.

Saate Azure web ja töötaja rollid, mis kasutavad autonoomne käitusajal lähteandmed, viide andmed koos rakenduse juurutamine. Kui kohalik salvestusruum suurus võimaldab sellise juurutus, on see on optimaalne olekus. Manustatud andmebaase (SQL-i, NoSQL) või XML-failid juurutatud kohaliku failisüsteemi aitab Azure Arvuta skaala üksuste sõltumatult. Siiski peaks olema süsteemi positsioonide nõudmata iga rolli andmete värskendamiseks. Selleks asetage viide andmetele pilveteenuse salvestusruumi lõpp-punkti (nt Azure'i bloobimälu või SQL-andmebaasi) värskendusi. Lisage koodi iga rolli, mis laadib andmed värskendused Arvuta sõlmed rolli käivitamisel sisse. Teise võimalusena lisada koodi, mis võimaldab teha jõustatud allalaadimine üheks rolli aknad.

Kättesaadavuse parandamiseks peaks rollid sisaldavad ka viide andmete salvestusruumi on alla. See võimaldab rollid alustamiseks põhilisi andmete kuni salvestusruumi ressursi värskendusi.

![Rakenduste kõrge-saadavus kaudu autonoomne arvutada sõlmed](./media/resiliency-high-availability-azure-applications/application-high-availability-through-autonomous-compute-nodes.png)

Üks tähelepanu see muster on juurutus- ja käivitus kiirust oma rollid. Kui olete juurutamine või alla suurt hulka andmeid käivitamisel, see võib suurendada aega kulub tööasendisse uue juurutuste või rolli aknad. See võib olla on aktsepteeritav Miinuseks võttes viide andmete kohe saadaval iga rolli asemel sõltuvalt välise salvestusruumi teenuste sõltumatus.

###<a name="transactional-data-pattern-for-high-availability"></a>Kõrge-saadavus kandeandmete muster

Andmed, mis loob rakendus business kontekstis on selgituseks. Kandeandmete on äriprotsesside, mis rakendab rakendus ja viide andmed, mis toetab järgmiste protsesside kombinatsiooni. Kandeandmete saate näiteks tellimuste, täpsemalt saatmise teated, arvete ja klientide seose haldus (CRM) võimalusi. Kandeandmete, seega loodud sisestatakse väliste süsteemidega andmete säilitamise või edasiseks töötlemiseks.

Pidage meeles selle andmeid saate muuta süsteemides, et andmed on viide. Seetõttu tuleb kandeandmete salvestada punkti /-kellaaja viide andmete kontekstis nii, et see on minimaalne väline sõltuvusi oma semantilise järjepidevuse. Näiteks kaaluge eemaldamist toote kataloogist paar kuud pärast tellimuse on täidetud. Parim on nii palju viide andmete kontekstis võimalusel embed tehingu. See säilitab tehingu seotud semantika isegi juhul, kui viite andmed muutub pärast tehingu tegemist.

Nagu eelnevalt mainitud, arhitektuur, mis lahti ühendada ja asünkroonse suhtluse sobi kõrgem kättesaadavus. See kehtib ka kandeandmete, kuid rakendamist on keerulisem. Traditsiooniline selgituseks mõisted tavaliselt toetuvad tagamiseks tehingu andmebaasi. Kui olete sisse kihid, peab rakenduse koodi õigesti hakkama andmete kihtide, et tagada piisav järjepidevus ja kestvus.

Pärast kirjeldatakse töövoo, mis eraldab selgituseks andmehõive selle töötlemiseks.

1. Web MSDN: Esita viitavad andmetele.
1. Väliselt: Salvesta vahe kandeandmete.
1. Web MSDN: lõppkasutaja toimingu.
1. Web MSDN: lõplikus kandeandmete koos viide andmete kontekstis, saata ajutine püsival salvestusruumi, mis on tagatud prognoositavad vastuse anda.
1. Web MSDN: märku lõppkasutaja lõpetamise tehingu.
1. Tausta arvutada sõlm: selgituseks andmete, töötlemine pärast seda vajaduse korral ja saatke see oma viimasel talletuskoht praeguse süsteemi.

Järgmisel joonisel on esitatud ühe võimalik selle kujunduse rakendamine Azure'i majutatava pilveteenuses.

![Kõrge-saadavus lahti ühendada kaudu](./media/resiliency-disaster-recovery-high-availability-azure-applications/application-high-availability-through-loose-coupling.png)

Eelmisel joonisel kriipsjoontega nooled viitavad sellele asünkroonne töötlemine. Veebi roll ei tea selle asünkroonne töötlemine. See viib selle sihtkohta viitega praeguse süsteemi tehingu salvestusruumi. Latentsus, mis tutvustab selle asünkroonne mudeli tõttu selgituseks andmed pole päringu jaoks kohe saadaval. Seetõttu tuleb iga üksuse selgituseks andmete vahemälu salvestada või kasutaja seansi kohe UI täita tuleb.

Veebis on autonoomne ülejäänud infrastruktuuri. Oma profiili kättesaadavus on web roll ja Azure kuhjuda ja pole kogu infrastruktuur kombinatsiooni. Kõrge-saadavus, võimaldab see lähenemine web roll mastaapimiseks horisontaalselt, sõltumatult tagaandmebaas salvestusruumi. See kõrge-saadavus mudel võivad mõjutada toimingute ülevaade. Nagu Azure järjekorrad ja töötaja rolli lisakomponentide mõjutavad igakuised kasutus kulud.

Pange tähele, et eelmise skeemi selle sidumata lähenemisviisi kandeandmete ühe rakendamine. On palju võimalikke rakendusi. Järgmisest loendist leiate mõned alternatiivid:

 * Töötaja roll võib vahel web roll ja salvestusruumi järjekorda paigutada.
 * Teenuse siini järjekorda saab asemel on Azure Storage järjekorda.
 * Sihtkohta võib olla Azure Storage või mõne muu andmebaasi pakkuja.
 * Azure'i vahemälu saab kihis web esitada pärast tehingut kohe vahemällu nõuetele.

###<a name="scalability-patterns"></a>Mustrite skaleeritavus.

See on oluline märkida skaleeritavus pilvepõhise teenuse mõjutab otseselt kättesaadavus. Kui koormuse põhjustab teenust ei reageeri, on kasutaja mulje, et rakendus on alla. Järgige head tavad lähtuvalt teie oodatav rakenduse laadimine ja tulevaste ootustele skaleeritavus. Kõrgeimate skaala hõlmab palju peaksite arvesse võtma, näiteks kasutamine ühe või mitme salvestusruumi kontod ühiskasutamine mitme andmebaasid ja vahemällu strateegiad. Vaadake põhjalik ülevaade nende mustrite, [Head tavad suuremahuliste kujundus teenuste Azure'i pilveteenustega](https://azure.microsoft.com/blog/best-practices-for-designing-large-scale-services-on-windows-azure/).

##<a name="next-steps"></a>Järgmised sammud

See artikkel on osa sarjast artiklite [avariitaastet](./resiliency-disaster-recovery-high-availability-azure-applications.md)ja kõrge kättesaadavus loodud Microsoft Azure. Järgmise artiklis toodud selle sarja on [jaoks2 loodud Microsoft Azure](./resiliency-disaster-recovery-azure-applications.md).
