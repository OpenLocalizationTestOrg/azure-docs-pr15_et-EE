<properties
   pageTitle="Tehnilised juhendid: Kohalikud tõrked Azure taastamise | Microsoft Azure'i"
   description="Artikli mõistmine ja kujundamine olles, mis on väga saadaval, tõrketaluvusega rakendused, samuti kavandamise avariitaastet värskendustest Kohalikud tõrked Azure'is."
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

#<a name="azure-resiliency-technical-guidance-recovery-from-local-failures-in-azure"></a>Azure'i paindlikkust tehnilised juhendid: Kohalikud tõrked Azure taastamine

On kaks esmane ohud rakenduste saadavus.

* Seadmeid, näiteks draivid ja serverite tõrge
* Kriitiline ressursse, näiteks Arvuta tippväärtus laadi tingimustel lõppemise

Azure'i pakub kombinatsiooni ressursside haldamine, elastsuse, koormusetasakaalustuseks ja eraldamine lubamiseks kõrge-saadavus neid asjaolusid. Mõned nendest funktsioonidest tehakse automaatselt kõik Azure'i teenuste jaoks. Mõnel juhul peab rakenduse arendaja teha mõned lisatööd kasu saada.

##<a name="cloud-services"></a>Pilveteenused

Azure'i pilveteenustega koosneb ühe või mitme veebis või töötaja rollide kogumid. Ühe või mitme eksemplari rolli saab käivitada üheaegselt. Konfiguratsiooni määratleb eksemplaride arv. Rolli aknad jälgitakse ja struktuuri kontrolleril komponenti kaudu. Struktuuri kontrolleril tuvastab ja vastab nii tarkvara ja riistvara tõrked automaatselt.

Iga rolli eksemplari töötab oma virtuaalse masina (VM) ja selle struktuuri domeenikontrolleri kaudu külaline agent suhtleb. Külalisena agent kogub ressursside ja sõlm mõõdikud, sh VM kasutus, olek, logid, ressursikasutus, erandid ja nõuded. Struktuuri kontrolleril päringute külaline agent konfigureeritav intervalliga ja selle taaskäivitamist VM kui külaline agent ei vasta. Kui riistvara, seotud struktuuri kontrolleril teisaldab probleemse rolli läbivalt kogu uue riistvara sõlm ja ümber marsruutimiseks liiklus seal võrgus konfigureeritud.

Kasu need funktsioonid, arendajate tagama, et kõik teenuse rollid Ärge hoidke ühiskaustas olekus rolli aknad. Selle asemel tuleks kasutada kõigi püsivate andmete püsival salvestusruumi, nt Azure Storage või Azure'i SQL-andmebaasi. See võimaldab rollide päringute töötlemiseks. Ka see tähendab, et rolli aknad võib minna igal ajal loomata vastuolusid teenuse ajutine või püsiv olekus.

Laoseisu väliselt rollidele nõue on mitu mõju. See tähendab, näiteks, et kõik seostuvad muudatused Azure Storage tabelisse tuleks muuta ühe üksus rühma toimingu, võimaluse korral. Muidugi alati ei saa ühe toimingu kõik muudatused. Teil tuleb eriti hoolikalt tagada, et rolli eksemplari tõrkeid põhjustada probleeme, kui nad katkestamine kahe või enama värskendused püsiv olek teenuse hõlmavaid toiminguid. Kui veel mõne rolli proovib sellise toimingu uuesti, see eeldate ja teha juhul, kui töö on osaliselt lõpule viia.

Näiteks kaaluge teenus, mis piirded andmete üle mitme poed. Kui töötaja roll läheb alla, kui see on ümber on Kildu, on Kildu ümberpaigutamiseks võib lõpule. Või ümberpaigutamiseks võib korrata algusest erinevate töötaja rolli, mis võivad tekkida orbsisutüübi andmeid või andmete rikkumist. Probleemide vältimiseks tuleb toiminguid ühte või mõlemat järgmistest:

 * *Idempotent*: korratavad ilma küljel efektid. Idempotent olevat pikaajaline toiming peaks olema sama mõju sõltumata sellest, mitu korda käitub, isegi siis, kui see on katkenud käitamise ajal.
 * *Sammhaaval restartable*: Viimatine tõrge punktist jätkata. Sammhaaval restartable olevat pikaajaline toiming peab olema väiksem atomic toimingute jada. See peaks ka salvestada edenemisega püsival laos nii, et iga järgmise kutsumise jätkab, kus kaasab on peatatud.

Lõpuks kõiki toiminguid tuleks kasutada seni, kuni ta õnnestub. Näiteks ettevalmistamise toiming võivad ja Azure paigutada, ja seejärel eemaldatakse kuhjuda töötaja rolli alusel ainult siis, kui see ei õnnestu. Prügikoristus võib olla vaja puhastada andmeid, mis segataks toimingute loomine.

###<a name="elasticity"></a>Elastsuse

Iga rolli konfiguratsiooni määratakse algse iga rolli töötavate eksemplaride arv. Esialgu peaks administraatorid konfigureerida iga rolli käivitamiseks koos kahe või enama eksemplari oodatud laadi põhjal. Kuid saate hõlpsalt skaalal rolli aknad või alla nimega kasutus mustrite muuta. Saate teha seda käsitsi Azure'i portaalis või protsessi Windows PowerShelli, teenuse juhtimise API või muude tootjate tööriistade abil automatiseerida. Lisateavet leiate teemast [rakenduse autoscale](../cloud-services/cloud-services-how-to-scale.md).

###<a name="partitioning"></a>Jagamine

Azure'i struktuuri domeenikontrolleri kasutab sektsioonid kahte tüüpi:

* Mõnda *domeeni värskendamiseks* kasutatakse versioonile teenuse rolli aknad rühmad. Azure'i kasutab eksemplaride mitu update domeeni sisse. Kohapealne värskendus, struktuuri kontrolleril toob kõik eksemplarid allapoole ühe update domain, värskendatakse neid ja seejärel taaskäivitamist need enne järgmise värskenduse domeeni. Seda moodust takistab kogu teenus on saadaval värskendamise käigus.
* *Viga domeeni* määratleb võimalikud punktide riist- või võrgu tõrge. Rolli, mis sisaldab rohkem kui ühe eksemplari, struktuuri kontrolleril tagab, et linnanimede levitatakse üle mitme viga domeeni katkestades teenuse isoleeritakse riistvara tõrgete vältimiseks. Viga domeenide haldavad kõik käes server ja kobar ebaõnnestumist.

[Azure'i - teenindustaseme leping (SLA)](https://azure.microsoft.com/support/legal/sla/) tagab kui kahe või enama web rolli aknad on juurutatud erinevate viga ja täiendamine domeenide, nendel kuvatakse välise ühenduvuse vähemalt 99,95% ajast. Erinevalt update domeenid on võimalus määrata viga domeenide arvu. Azure'i eraldab viga domeenid ja jaotab rolli aknad kogu neid automaatselt. At vähemalt kaks esimest eksemplari iga rolli paigutatakse erinevate viga ja täiendamine domeenide tagamaks, et iga rolli vähemalt kaks eksemplari rahuldab SLA. See on esitatud järgmisel diagrammil.

![Lihtsustatud vaade viga domeeni eraldustaseme](./media/resiliency-technical-guidance-recovery-local-failures/partitioning-1.png)

###<a name="load-balancing"></a>Laadi tasakaalustamiseks

Kogu sissetulev liiklus web rolli läbib kodakondsuseta laadi koormusetasakaalustusteenuse, mis jagab klientide päringutele vahel rolli aknad. Üksikute rolli aknad ei ole avaliku IP-aadressid ja neid ei saa otse adresseeritavad Interneti kaudu. Web rollid on nii, et iga kliendi taotluse marsruuditakse rolli eksemplaris. [StatusCheck](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.statuscheck.aspx) sündmuse tõstetakse iga 15 minutit. Saate kasutada seda näitab, kas roll on valmis saada liikluse, või kas see on hõivatud ja tuleb välja laadi-koormusetasakaalustusteenuse pöörde võtta.

##<a name="virtual-machines"></a>Virtuaalmasinates

Azure'i Virtuaalmasinates erineb platform teenus (PaaS) arvutada rollid seoses kõrge-saadavus mitmel viisil. Mõnel juhul peate tegema lisatööd kõrge kättesaadavuse tagamiseks.

###<a name="disk-durability"></a>Ketas Kestvus

Erinevalt PaaS rolli aknad, virtuaalse masina draivid salvestatud andmed on püsivate isegi siis, kui virtuaalse masina paigutatakse. Azure'i virtuaalmasinates kasutada plekid Azure Storage VM ketast, mis on olemas. Azure Storage kättesaadavus omaduste tõttu on väga saadaval ka virtuaalse masina draivid talletatud andmed.

Pöörake tähelepanu sellele, kettal D (Windowsi VMs) selle reegli erand. Ketas D on tegelikult füüsilise säilitamise sektsioon VM majutavas serveris ja oma andmed lähevad kaotsi, kui VM toimub töötlemine. Ketas D on mõeldud ainult ajutine salvestusruum. Linux, Azure "tavaliselt" (kuid mitte alati) seab ajutine kohalikule kettale SDB Blokeeri seade. See on sageli ühendatud Azure Linux Agent nimega /mnt/resource või/mnt haakepunktide (konfigureeritav /etc/waagent.conf kaudu).

###<a name="partitioning"></a>Jagamine

Azure'i algupäraselt mõistab astme PaaS rakenduses (web rolli ja töötaja rolli) ja seega saate õigesti laiali viga ja Värskenda domeenides. Seevastu tasemete seas on infrastruktuuri teenus (IaaS) rakendus peab olema käsitsi määratletud kättesaadavus komplekti kaudu. Jaotises IaaS SLA jaoks vajalike kättesaadavus komplektid.

![Kättesaadavus seab Azure'i virtuaalmasinates](./media/resiliency-technical-guidance-recovery-local-failures/partitioning-2.png)

Eelmisel joonisel (mis toimib web appi taseme) Internet Information Services (IIS) esimese taseme ja SQL-i taseme, (mis toimib andmete taseme) on määratud eri-saadavus komplektid. See tagab, et iga taseme läbivalt kogu on riistvara koondamise, jagades virtuaalmasinates viga domeenides ja kogu astme on võetud ajal värskendust.

###<a name="load-balancing"></a>Laadi tasakaalustamiseks

Kui VMs peab olema kogu neid jaotatud liikluse, peab rühmitate VM soovitud rakendus ja laadi saldo üle mõne kindla TCP või UDP lõpp-punkti. Lisateavet leiate teemast [koormusetasakaalustuseks virtuaalmasinates](../virtual-machines/virtual-machines-linux-load-balance.md). Kui VMs saadetakse sisestatud mõnest muust allikast (nt andmebaasitõrge kord), laadi koormusetasakaalustusteenuse ei ole vajalik. Laadi koormusetasakaalustusteenuse kasutab lihtsa seisundikontrolli, et kindlaks teha, kas liikluse saadetakse sõlme. Samuti on võimalik luua oma sondid rakendada rakendusele vastav seisund mõõdikute määratleda, kas VM peaks saama liikluse.

##<a name="storage"></a>Salvestusruumi

Azure'i salvestusruumi on Azure püsival andmeteenuse võrdlusalus. Pakub bloobimälu, tabelist, kuhjuda ja VM kettaruumi. Kopeerimine ja ressursside haldamine seda kasutab kõrge-saadavus ühe andmekeskuse sees. Azure'i salvestusruumi-saadavus SLA tagab vähemalt 99,9% ajast:

* Õigesti vormindatud taotluste lisamine, värskendamine, lugege teemat ja Kustuta andmete edukalt ja õigesti töödeldakse.
* Salvestusruumi kontod on lüüsi Interneti ühenduvus.

###<a name="replication"></a>Dispersioonanalüüs

Azure'i salvestusruumi hõlbustab andmete kestvus säilitades sõltumatu füüsilise säilitamise allsüsteemide piirkonnas üle mitme eksemplari erinevate draividel kõik andmed. Andmed on kopeeritud sünkroonselt ja kõik eksemplarid, mis on kinnitatud enne selle kirjutamine on teada. Azure'i salvestusruumi on tugevalt ühtsete, tähendab, et viimase kirjutab kajastamiseks tagada loeb. Lisaks andmete koopiad pidevalt skannitud avastada ja parandada bitine käibe tulu, on salvestatud andmete terviklus sageli tähelepanuta ohtu.

Teenuste kasu dispersioonanalüüs lihtsalt Azure Storage abil. Teenuse arendaja ei pea lisatööd kohaliku ei saa taastada.

###<a name="resource-management"></a>Ressursside haldamine

Salvestusruumi kontod, mis on loodud pärast mai 2014, võib kasvada kuni 500 TB (eelmise maksimum oli 200 TB). Kui nõutakse lisaruumi, peab rakendusi kasutada mitut salvestusruumi kontod.

###<a name="virtual-machine-disks"></a>Virtuaalse masina ketast

Virtuaalne arvuti kettale on salvestatud lehe bloobimälu Azure Storage, annab see kõiki samu kestvus ja skaleeritavus atribuudid bloobimälu nimega. Selle kujunduse teeb andmed virtuaalse masina kettal püsiv, isegi juhul, kui VM töötava serveri nurjub ja VM taaskäivitamine teise serverisse.

##<a name="database"></a>Andmebaasi

###<a name="sql-database"></a>SQL-andmebaas

SQL Azure'i andmebaas pakub andmebaasi teenust. See võimaldab rakendustele kiiresti ettevalmistamise, andmete ja päringu relatsiooniandmebaasid lisada. Pakub paljud tuttavad SQL serveri funktsioonid ja funktsionaalsus, samal ajal niisutussüsteemide koormust riistvara konfiguratsiooni, lappimine ja paindlikkust.

>[AZURE.NOTE] Üks-ühele funktsiooni tarkvarakomplektiga SQL Server Azure'i SQL-andmebaasi ei paku. See on mõeldud täita teistsugused nõuded – üks, mis on kordumatult paremini pilv rakendusi (elastne skaala, andmebaasi teenuse vähendada hoolduskulusid jne). Lisateavet leiate teemast [Valige SQL Server suvand pilv: (PaaS) Azure SQL-i andmebaasi või SQL Server Azure'i VMs (IaaS)](../sql-database/sql-database-paas-vs-sql-server-iaas.md).

####<a name="replication"></a>Dispersioonanalüüs

SQL Azure'i andmebaas pakub sisseehitatud paindlikkust sõlm taseme tõrge. Kõik kirjutab andmebaasi on automaatselt kopeeritud kahe või enama tausta sõlmed kvoorumi Kinnita tehnika kaudu. (Esmane ja vähemalt üks teisese kinnitama, et enne tehingu loetakse eduka ja tagastab kirjutatakse tegevuse tehingulogi.) Korral sõlm, andmebaasi automaatselt ei üle ühe teisene koopiad. See põhjustab siirdamiseks ühenduse katkemise klientrakendustes jaoks. Seetõttu kõik Azure'i SQL-andmebaasi kliendid rakendama mingi siirdamiseks ühenduse töötlemine. Lisateabe saamiseks lugege teemat [proovige teenuse juhised](../best-practices-retry-service-specific.md).

####<a name="resource-management"></a>Ressursside haldamine

Iga andmebaasi loomisel on konfigureeritud piirarvu upper suurus. See on praegu saadaval maksimaalne maht on 1 TB (suurus piirangud erinevad vastavalt teie teenuse kiht, lugege teemat [Azure SQL-i andmebaaside teenuse astme ja jõudlust](../sql-database/sql-database-resource-limits.md#service-tiers-and-performance-levels). Kui andmebaasi tabab ülemise mahupiirangu, ei nõustu täiendavaid lisa või VÄRSKENDA käske. (Päringute ja andmete kustutamist on võimalik.)

Andmebaasis Azure'i SQL-andmebaasi kasutab mõnda struktuuri haldamiseks ressursse. Siiski asemel struktuuri administraator, kasutab ring topoloogia vigade tuvastamiseks. Iga koopia klaster on kahe ja vastutab avastada, kui nad minna. Kui koopia läheb alla, käivitamine naabritega ümberkonfigureerimine agent uuesti luua selle mõnda teise arvutisse. Otsimootori pidurdamise on esitatud loogilises server ei kasutada liiga palju ressursse arvutisse või ületada seadme füüsilised piirid tagamiseks.

###<a name="elasticity"></a>Elastsuse

Kui rakendus nõuab rohkem kui 1 TB andmebaasi limiit, tuleb seda rakendada skaala-out lähenemine. Muudate koos Azure'i SQL-andmebaasi käsitsi eraldamine tuntud ka kui sharding üle mitme SQL-andmebaasi andmete alusel. Seda moodust skaala-out pakub võimaluse saavutada peaaegu lineaarse kulu kasvu skaala. Elastne kasvamisel või võimsus nõudmisel võib kasvada täiendavad kulud vastavalt vajadusele, kuna andmebaasid on arve põhjal Keskmine tegeliku suuruse kasutatud päevas, pole võimalik maksimummaht põhjal.

##<a name="sql-server-on-virtual-machines"></a>SQL Server Virtuaalmasinates

Kui installite Azure'i Virtuaalmasinates SQL serveri (2014 või uuem versioon), saate teha traditsiooniline-saadavus funktsioone SQL serveri. Need funktsioonid on Kättesaadavusrühmad ja andmebaasi peegeldus. Pange tähele, et Azure VMs, salvestusruumi ja võrk erinevad funktsionaalseid omadused kui asutusesisese, mitte virtualiseeritud IT taristu. Eduka rakendamise kõrge kättesaadavus/avariitaastet (HA/DR) SQL Server Azure'i lahenduse nõuab, et teil mõista nende erinevusi ja kujundamine mahutamiseks need teie lahendus.

###<a name="high-availability-nodes-in-an-availability-set"></a>Kõrge-saadavus sõlmed saadavus seadmine

Kui rakendate Azure kõrge-saadavus lahenduse, saate määrata Azure kättesaadavus koht kõrge-saadavus sõlmed eraldi viga domeenid ja täiendamine domeenide. Et oleks selge, on kättesaadavus määramine Azure mõiste. See on parim, mida peaksite järgima veenduge, et teie andmebaasid on tõepoolest väga kättesaadav, kas kasutate Kättesaadavusrühmad, andmebaasi peegeldus või midagi muud. Kui te pole hea tava, võib vale eeldusel, et teie süsteemi on väga saadaval jaotises. Kuid tegelikult teie sõlmed saate kõik korraga nurjuda sest need juhtuda sama viga domeeni Azure piirkonna paigutada.

Soovitusega pole saadaval Logi saatmine. Katastroofi taastamine funktsioonina veenduge, et serverid töötavad Azure omaette piirkondades. Määratluse regioonides on eraldi viga domeenid.

Azure'i Cloud Services vms juurutatud klassikaline portaali kaudu olema sama kättesaadavus määramine, peate need sama pilveteenuses juurutamine. Seda piirangut pole VMs juurutatud kaudu Azure'i ressursihaldur (praeguse portaal). Klassikaline portaali kasutatavatele VMs Azure'i pilveteenuses ainult sõlmed sama pilveteenuses saavad osaleda samu kättesaadavus. Lisaks Cloud Services VMs peaks olema sama virtuaalse võrgu tagab, et nad saavad IP-d isegi pärast teenuse tervendav. See aitab vältida DNS-i värskendamine häireid.

###<a name="azure-only-high-availability-solutions"></a>Azure'i ainult: kõrge-saadavus lahendused

Saate määrata kõrge-saadavus lahenduse oma SQL serveri andmebaaside Azure Kättesaadavusrühmad või andmebaasi peegeldus abil.

Järgmine diagramm näitab töötab Azure'i Virtuaalmasinates Kättesaadavusrühmad arhitektuur. Selle diagrammi võeti põhjaliku artikli sellest, [kättesaadavuse ja SQL Server Azure'i Virtuaalmasinates katastroofiabi](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).

![Microsoft Azure'i Kättesaadavusrühmad](./media/resiliency-technical-guidance-recovery-local-failures/high_availability_solutions-1.png)

Mõne Kättesaadavusrühmad juurutus--lõpuni Azure VMs kohta saate automaatselt ka AlwaysOn malli abil Azure'i portaalis ette valmistada. Lisateabe saamiseks vt [SQL serveri AlwaysOn pakub Microsoft Azure'i portaalis galeriis](https://blogs.technet.microsoft.com/dataplatforminsider/2014/08/25/sql-server-alwayson-offering-in-microsoft-azure-portal-gallery/).

Järgmine diagramm näitab andmebaasi peegeldus Azure'i Virtuaalmasinates kasutamine. See võeti [kättesaadavuse ja SQL Server Azure'i Virtuaalmasinates avariitaastet](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md)põhjalikumat teema.

![Andmebaasi peegeldus Microsoft Azure](./media/resiliency-technical-guidance-recovery-local-failures/high_availability_solutions-2.png)

>[AZURE.NOTE] Mõlema arhitektuurides jaoks on vaja domeenikontrolleri. Andmebaasi peegeldus, kus on võimalik kasutada enam vaja domeenikontrolleri serveri serdid.

##<a name="other-azure-platform-services"></a>Azure'i platvormi muude teenustega

Rakendusi, mis on loodud Azure kasu platvormi võimalusi taastada Kohalikud tõrked. Mõnel juhul saate teha teatud toiminguid teie konkreetse stsenaariumi kättesaadavuse parandamiseks.

###<a name="service-bus"></a>Teenuse siini

Leevendada ajutiste katkestuste Azure'i teenus siini vastu, kaaluge püsival kliendipoolne järjekorda. See ajutiselt kasutab mõne alternatiivse, kohaliku salvestusruumi süsteemi sõnumid, mida ei saa lisada teenuse siini järjekorda talletamiseks. Rakenduse saate otsustada, kuidas hallata ajutiselt salvestatud sõnumeid pärast teenus on taastatud. Lisateabe saamiseks vt [jõudlustäiustusi teenuse siini kasutamise head tavad vahendajaks sõnumside](../service-bus-messaging/service-bus-performance-improvements.md) - ja [teenuse (avariitaastet)](./resiliency-technical-guidance-recovery-loss-azure-region.md#other-azure-platform-services).

###<a name="hdinsight"></a>Hdinsightiga

Azure'i bloobimälu Vaikimisi talletatakse andmeid, mis on seotud Windows Azure Hdinsightiga. Azure'i salvestusruumi määrab bloobimälu kõrge-saadavus ja kestvus atribuudid. Mitme sõlme töötlemine, mis on seotud Hadoopi MapReduce töö ilmneb kohta on ajutine Hadoopi jaotatud faili süsteemi (HDFS), mis on ette valmistatud, kui Hdinsightiga vajab. Tulemuste MapReduce töö ka talletatud Azure'i bloobimälu vaikimisi nii, et töödeldud andmete püsival ja jääb tugevalt kättesaadavaks pärast Hadoopi kobar on eemaldatud. Lisateabe saamiseks vt [Hdinsightiga (avariitaastet)](./resiliency-technical-guidance-recovery-loss-azure-region.md#other-azure-platform-services).

##<a name="checklists-for-local-failures"></a>Kohalikud tõrked kontroll-loendid

###<a name="cloud-services"></a>Pilveteenused

  1. Vaadake üle selle dokumendi jaotise pilveteenustega.
  2. Konfigureerida vähemalt kaks eksemplari iga rolli.
  3. Püsida riigi püsival hoidla, mitte rolli aknad.
  4. Õigesti hakkama StatusCheck sündmus.
  5. Murdmiseks seotud muudatuste tehingud, kui võimalik.
  6. Veenduge, et töötaja rolli tööülesanded on idempotent ja restartable.
  7. Jätkake autonoomsest toimingud enne, kui neil õnnestub.
  8. Kaaluge võimalust autoscaling strateegiad.

###<a name="virtual-machines"></a>Virtuaalmasinates

  1. Vaadake üle Virtuaalmasinates jaotises selle dokumendi.
  2. Ärge kasutage ketas D püsivate Storage.
  3. Teenuse kiht masinad rühmitada mõne kättesaadavus määramine.
  4. Konfigureerige koormust tasakaalustavad ja valikuline sondid.

###<a name="storage"></a>Salvestusruumi

  1. Vaadake üle selle dokumendi jaotise salvestusruumi.
  2. Kasutage mitme salvestusruumi konto andmeid või läbilaskevõime ületab piirmäära.

###<a name="sql-database"></a>SQL-andmebaas

  1. Vaadake üle selle dokumendi jaotist SQL-i andmebaasi.
  2. Proovi uuesti siirdamiseks vigade rakendamiseks.
  3. Kasutage eraldamine sharding skaala-out strateegia.

###<a name="sql-server-on-virtual-machines"></a>SQL Server Virtuaalmasinates

  1. Vaadake üle selle dokumendi jaotist Virtuaalmasinates SQL Server.
  2. Järgige eelmise soovitused Virtuaalmasinates.
  3. Kasutage SQL serveri kõrge-saadavus funktsioone, näiteks AlwaysOn.

###<a name="service-bus"></a>Teenuse siini

  1. Vaadake üle teenuse siini jaotises selle dokumendi.
  2. Kaaluge võimalust luua püsival kliendipoolne järjekorda varukoopiana.

###<a name="hdinsight"></a>Hdinsightiga

  1. Vaadake üle Hdinsightiga jaotises selle dokumendi.
  2. Kohalikud tõrked jaoks vajalike pole kättesaadavus täiendavad juhised.

##<a name="next-steps"></a>Järgmised sammud

See artikkel on osa sarjast värskendustest [Azure paindlikkust tehnilised juhendid](./resiliency-technical-guidance.md). Järgmise artiklis toodud selle sarja on [piirkond kogu teenuse häirete taastamine](./resiliency-technical-guidance-recovery-loss-azure-region.md).
