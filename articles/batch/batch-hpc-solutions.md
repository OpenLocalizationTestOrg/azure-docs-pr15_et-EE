<properties
   pageTitle="Paketi ja HPC lahenduste pilveteenuses | Microsoft Azure'i"
   description="Lisateavet paketi ja suure jõudlusega arvutuste (HPC ning suur arvutada) stsenaariumid ning Azure lahenduse suvandid"
   services="batch, virtual-machines, cloud-services"
   documentationCenter=""
   authors="dlepow"
   manager="timlt"
   editor=""/>

<tags
   ms.service="batch"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="big-compute"
   ms.date="07/27/2016"
   ms.author="danlep"/>

# <a name="batch-and-hpc-solutions-in-the-azure-cloud"></a>Azure'i pilves paketi ja HPC lahendused

Azure'i pakub tõhusa, scalable pilve lahendusi paketi ja suure jõudlusega arvutuste (HPC) - ehk *Suur arvutada*. Siin teavet suur arvutada töökoormus ja Azure teenused toetavad neid või [lahenduse stsenaariumid](#scenarios) selle artikli liikuda. See artikkel on mõeldud peamiselt tehnilise haridusvaldkonna esindajad, IT-juhid ja sõltumatu tarkvara tarnijate, kuid muud arendajad ja IT-spetsialistidele saate tutvuda järgmisi lahendusi.

Organisatsioonidel on suuremahuliste arvuti probleemide: tehnika kujundus ja analüüsi, piltide renderdus, keerukate modelleerimine, Roc simulatsioonid, rahandus risk arvutusi ja teised. Azure'i aitab ettevõtted ressursse, skaala ja need tuleb ajakava neid probleeme lahendada. Azure'i ettevõtted teha järgmist:

* Looge hü lahendusi, mis ulatub mõne kohapealse HPC tipp töökoormus pilveteenusesse et kobar

* Käivitage HPC kobar tööriistade ja töökoormus täiesti Azure

* Arvuta mahukat töökoormus käivitada ilma juurutada ja hallata Arvuta taristu hallatavate ja scalable Azure teenused, nt [paketi](https://azure.microsoft.com/documentation/services/batch/) abil

Kuigi käesolevas artiklis ei käsitleta Azure'i ka pakub arendajad ja partnerite täielik kogum, võimaluste, arhitektuur valikuid ja arengu tööriistad koostamiseks suuremahuliste, kohandatud suur arvutada töövood. Ja kasvab partnerite ökosüsteem on valmis teid aitama saate muuta oma suur arvutada töökoormus Azure'i pilves tõhusamalt töötada.


## <a name="batch-and-hpc-applications"></a>Paketi ja HPC rakendused

Erinevalt web rakendused ja palju ärivaldkonna rakenduste, paketi ja HPC rakendused on määratletud algus- ja lõppaeg ning ta saab käitada ajakava või nõudmisel mõnikord tundi või enam. Enamik jagunevad kahte: *otseselt paralleelselt* (mõnikord nimetatakse "piinlikult rööpselt", kuna need lahendada probleeme võimalda töötab samal ajal mitmes arvutis või protsessorid) ja *kindlalt haagitud*. Järgmist tüüpi rakenduste kohta leiate järgmisest tabelist. Mõned Azure lahendus lähenedes paremini ühe või teise.

>[AZURE.NOTE] Paketi ja HPC lahendusi, käivitatud eksemplari rakenduse nimi on tavaliselt *töö*ja iga töö võib saada jagatud *Tööülesanded*. Ja Kobartulpdiagrammi Arvuta ressursse rakenduse nimetatakse sageli *arvutada sõlmed*.

Tüüp | Omadused | Näited
------------- | ----------- | ---------------
**Otseselt paralleelselt**<br/><br/>![Otseselt paralleelselt][parallel] |Käivitage rakendus loogika sõltumatult • arvutite<br/><br/> • Lisamise arvutite rakendusel skaala ja kiirem arvutus<br/><br/>• Rakenduse koosneb eraldi täitmisfailid või on jagatud rühma teenuseid kasutada mõnda muud klienti (teenusele suunatud arhitektuuri või SOA rakendus) |• Finantsalased risk modelleerimine<br/><br/>• Pildi renderdamine ja pilditöötluse<br/><br/>• Meediumide kodeerimise ja transkodeerida<br/><br/>• Roc simulatsioonid<br/><br/>• Tarkvara testimine
**Kindlalt haagitud**<br/><br/>![Kindlalt haagitud][coupled] |• Rakenduse jaoks on vaja Arvuta sõlmed nendega või exchange vahe tulemused<br/><br/>• Arvuta sõlmed võib edastada sõnumi läbides kasutajaliidese (näitaja tulemuse vahel), suhtlus meilikontot paralleelselt arvuti abil<br/><br/>• Rakendus on tundlik võrgu latentsus ja läbilaskevõime<br/><br/>• Rakenduse jõudlust parandada nagu InfiniBand ja serveri otsest mälu juurdepääs (RDMA) kiire võrgu tehnoloogiate alusel |• Oil ja bensiinikulu veehoidla modelleerimine<br/><br/>• Matemaatika erifunktsioonid kujundus ja analüüsi, nt arvutuslik vedeliku dynamics<br/><br/>• Füüsilise simulatsioonil, näiteks auto jookseb ja tuumareaktsioone<br/><br/>• Ilmaennustuse

### <a name="considerations-for-running-batch-and-hpc-applications-in-the-cloud"></a>Kaalutlused töötab paketi ja HPC rakenduste pilveteenuses

Saate hõlpsasti migreerida palju rakendusi, mis on mõeldud töötama kohapealse HPC rühmades Azure'i või hübriidkeskkonna (asukohaga). Siiski võib mõned piirangud või peaksite arvesse võtma, sh:


* **Pilveteenuse ressursside** - cloud Arvuta ressursid kasutate, sõltuvalt teie ei pruugi usaldada pidev masina-saadavus töö käitamise ajal. Maakond töötlemine ja edenemise kuvamiseks märkige ruut osutamine on levinud tehnikate siirdamiseks ebaõnnestumisi ja veel vajadusel käsitlema cloud ressursside kasutamisel.


* **Juurdepääs andmetele** - andmed Accessi tehnika üldiselt kättesaadavate enterprise rühmades, nt NFS, võib olla vaja hõlmata pilveteenuses. Või peate erinevat tüüpi andmete Accessi headest tavadest ja mustrid pilveteenuses vastu võtta.

* **Andmete liikumine** - rakenduste andmete teisaldamiseks salvestusruumi ja arvutada ressursid on vaja protsessi suurte andmehulkade, strateegiad. Peate võib-olla kiire asukohaga, nt [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/)networking. Samuti võtke arvesse, legal, regulatiivsete või poliitika piirangud salvestamiseks või juurdepääs andmeid.


* **Hulgilitsentsimise** - sisse hulgilitsentsimise või muude piirangute pilveteenuses töötab trükikojas taotlemise tarnijaga. Kõik müüjad pakuvad pensionitingimustega litsentsimine. Võib juhtuda kavandamine hulgilitsentsimise server teie lahendus pilveteenuses või kohapealse litsentsi serveriga ühendust.


### <a name="big-compute-or-big-data"></a>Suur Arvuta või Big Data?

Suur arvutada ja Big Data rakenduste eraldusjoon ei ole alati selge ja mõni rakendus võib olla nii omaduste. Nii kaasata suuremahuliste arvutuste, tavaliselt töötavate kogumite arvutitesse. Kuid lahenduse lähenemisel ja täiendavad tööriistad võivad olla erinevad.

• **Suur arvutada** tavaliselt kaasata CPU võimsus ja mälu, nt engineering simulatsioonid, rahandus risk modelleerimine ja digitaalse renderdamise rakendused. Suur arvutada lahenduse taristu võivad arvutite eriotstarbelisi mitmesoonelised protsessorid töötlemata arvutus ja eriotstarbelisi, Kiire riistvara arvuteid ühendada.

• **Big Data** lahendab andmete analüüsi probleemid, mis on seotud suurte andmehulkade, mida ei saa hallata, ühe arvuti või andmebaasi haldamine süsteemi. Näiteks suurel hulgal web logid või muid business intelligence andmeid. Big Data sageli rohkem toetuvad ketta maht ja I/O jõudlust kui CPU power. On ka spetsiaalseid Big Data tööriistu, nt Apache Hadoop kobar ja sektsiooni andmete haldamiseks. (Windows Azure Hdinsightiga ja teiste Azure'i Hadoopi lahenduste kohta leiate teavet teemast [Hadoopi](https://azure.microsoft.com/solutions/hadoop/)).

## <a name="compute-management-and-job-scheduling"></a>Arvuta haldus ja töö planeerimine

Paketi ja HPC rakenduste sisaldab sageli *halduri* ja *projektiplaanur* hõlbustamiseks rühmitatud Arvuta ressursside haldamine ja eraldatava käitamist rakendusi. Need funktsioonid võivad saavutada eraldi tööriistu, või integreeritud tööriista või teenuse.

* **Halduri** - sätted, väljastab ja haldab arvutada ressursid (või arvutada sõlmed). Kobar haldur võib automatiseerida installi operatsioonisüsteemi pilte ja Arvuta sõlmed, rakendusi ulatuse Arvuta ressursse vastavalt nõudmistele ja sõlmed jõudluse jälgimist.

* **Projektiplaanur** – saate määrata ressursse (nt protsessorite või mälu) rakenduse vajadustele ja tingimused, kui see töötab. Töö scheduler väidab töökohtade järjekorda ning eraldab neile on määratud prioriteet või muid omadusi.

Rühmitamise ja töö planeerimine tools for Windows ja Linux kogumite saate migreerida ka Azure. Näiteks [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029), Microsofti tasuta Arvuta kobar lahendus Windows ja Linux HPC töökoormus, pakub mitmeid võimalusi Azure töötab. Samuti saate koostada Linuxi kogumite avatud lähtekoodi tööriistad, nt pöördemomendi ja SLURM käivitamiseks. Saate tuua ka trükikojas ruudustiku lahenduste Azure, nt [TIBCO DataSynapse GridServer](http://www.tibco.com/company/news/releases/2016/tibco-to-accelerate-cloud-adoption-of-banking-and-capital-markets-customers-via-microsoft-collaboration), [IBM platvormi Symphony](http://www-01.ibm.com/support/docview.wss?uid=isg3T1023592)ja [Univa ruudustiku Engine](http://www.univa.com/products/grid-engine).

Nagu on näidatud järgmises jaotises, saate ka ära Arvuta ressursid haldamiseks ja ajastada töö ilma (või lisaks) traditsiooniline kobar Haldusriistad Azure'i teenuste.


## <a name="scenarios"></a>Stsenaariumid

Siin on kolm levinud stsenaariumi käivitamiseks Azure'i töökoormus suur arvutada, kasutades olemasolevaid HPC kobar lahendusi, Azure'i teenuste või nende kahe kombinatsioonist. Peaksite arvesse võtma valimise iga stsenaariumi puhul on loetletud, kuid pole täielik. Veel saadaval teie lahendus saab kasutada Azure teenuste kohta on selle artikli hilisem.

  | Stsenaarium | Miks seda valida?
------------- | ----------- | ---------------
**Burst on Azure HPC kobar**<br/><br/>[! [Kobar lõhkemist] [burst_cluster]](./media/batch-hpc-solutions/burst_cluster.png) <br/><br/> Lisateave:<br/>• [Burst Azure töötaja eksemplarides HPC Pack](https://technet.microsoft.com/library/gg481749.aspx)<br/><br/>• [Häälestamine hübriid arvutada HPC Pack kobar](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md)<br/><br/>• [Burst Azure paketti HPC Pack](https://technet.microsoft.com/library/mt612877.aspx)<br/><br/>|• Oma [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) või muude kohapealse kobar kombineerida Azure lisaressursid hübriidjuurutuse lahendus.<br/><br/>• Laiendamine suur arvutada töökoormus platvormi (PaaS) teenust virtuaalse masina eksemplaride käitamiseks (praegu Windows Server ainult).<br/><br/>• Juurde kohapealse, litsentsi serveri või andmete poe valikuline Azure virtuaalse võrgu kaudu|• Teil on mõne olemasoleva HPC kobar ja vajate veel ressursse <br/><br/>Te ei soovi osta ja täiendavad HPC kobar taristu haldamise •<br/><br/>Teil on ajutine tippväärtus vajadusel perioodide või eriprojektid
**Mõne HPC kobar luua täiesti Azure**<br/><br/>[! [Kobar rakenduses IaaS] [iaas_cluster]](./media/batch-hpc-solutions/iaas_cluster.png)<br/><br/>Lisateave:<br/>• [HPC kobar lahenduste Azure](./big-compute-resources.md)<br/><br/>|• Kiiresti ja pidevalt juurutada rakendusi ja kobar tööriistad standardseid või kohandatud Windowsi või Linuxi taristu on teenus (IaaS) virtuaalmasinates nimega.<br/><br/>• Käivitada erinevate suur arvutada töökoormus teie valitud lahenduse plaanimine töö abil.<br/><br/>• Täieliku pilvepõhist lahenduste loomiseks kasutada Azure lisateenuse, sh võrgunduse ja salvestusruumi. |Te ei soovi osta ja täiendavad Linux või Windows HPC kobar taristu haldamise •<br/><br/>Teil on ajutine tippväärtus vajadusel perioodide või eriprojektid<br/><br/>• Vajate täiendavaid klaster aega, kuid te ei soovi investeerida arvutite ja juurutada selle ruumi<br/><br/>Soovite, et Arvuta mahukat rakenduse nii, et see töötab teenus täiesti pilves •
**Välja paralleelselt rakenduse Azure skaala**<br/><br/>[! [Azure'i paketi] [batch_proc]](./media/batch-hpc-solutions/batch_proc.png)<br/><br/>Lisateave:<br/>• [Põhitõdesid Azure'i paketi](./batch-technical-overview.md)<br/><br/>• [Azure'i paketi teegi .net-i jaoks kasutamise alustamine](./batch-dotnet-get-started.md)|• Töötada koos [Azure'i paketi](https://azure.microsoft.com/documentation/services/batch/) välja erinevate suur arvutada töökoormus käitamist kaustu, Windowsi või Linuxi virtuaalmasinates skaala.<br/><br/>• Azure'i platvormi teenuse abil hallata juurutamist ja autoscaling virtuaalmasinates, töö planeerimine, katastroofiabi, andmete liikumine, sõltuvus haldus ja rakenduse juurutamine.|Te ei soovi haldamine • arvutada ressursid või projektiplaanur; selle asemel, mida soovite keskenduda töötavad teie rakendused<br/><br/>• Soovite, et Arvuta mahukat rakenduse nii, et see töötab teenus pilveteenuses<br/><br/>•, Mida soovite automaatselt mastaapida oma Arvuta ressursse, mis vastavad Arvuta töökoormus


## <a name="azure-services-for-big-compute"></a>Suur arvutada Azure'i teenused

Siit saate lisateavet Arvuta, andmed, võrgunduse ja seotud teenuste saab kombineerida lahendused suurt arvutada ja töövoogude kohta. Leiate Azure'i teenuste põhjalikud juhised leiate Azure'i teenuste [dokumentatsiooni](https://azure.microsoft.com/documentation/). Selle artikli [stsenaariumid](#scenarios) vaid mõned näited nende teenuste kasutamisel.

>[AZURE.NOTE] Azure'i regulaarselt tutvustab uusi teenuseid, mis võib olla kasulik, milline olukord teie. Kui teil on küsimusi, pöörduge on [Azure partneri](https://pinpoint.microsoft.com/en-US/search?keyword=azure) või e-posti *bigcompute@microsoft.com*.

### <a name="compute-services"></a>Arvutage teenused

Azure'i Arvuta teenused on suur arvutada lahenduse ja muu Arvuta teenuste pakkumise eeliseid erinevaid stsenaariumeid lähemalt. Põhilised tasemel nende teenuste pakkuda erineval viisil rakendusi käivitada virtuaalse masina vastavalt arvutada eksemplarid, mis on Azure pakub Windows Server Hyper-V tehnoloogia abil. Järgmiste eksemplaride saab käitada standard- ja kohandatud Linuxi ja Windowsi operatsioonisüsteemid ja tööriistu. Azure'i annab teile valik [eksemplari](../virtual-machines/virtual-machines-windows-sizes.md) suuruses eri konfiguratsioone CPU südamikud, mälu, ketta maht ja muid omadusi. Sõltuvalt teie vajadustele, saate mastaapimiseks eksemplarid tuhandetes valdkond ja siis skaala, kui teil on vaja vähem ressursse.

>[AZURE.NOTE] Ära Arvuta mahukat Azure jõudluse parandamiseks linnanimede ja skaleeritavus, sh paralleelselt MPI rakendusi, mis nõuavad madal latentsus ja võrgu läbilaskevõime kõrge rakenduse HPC töökoormus võtta. Lugege [H-sari ja Arvuta mahukat A-sarja VMs](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md).  

Teenus | Kirjeldus
------------- | -----------
**[Virtuaalmasinates](https://azure.microsoft.com/documentation/services/virtual-machines/)**<br/><br/> |• Pakkuda arvutada taristu teenus (IaaS) kasutades Microsoft Hyper-V tehnoloogia<br/><br/>• Võimaldavad paindlikult ettevalmistamine ja hallata püsivate pilve arvutite standard Windows Server või Linuxi piltide [Azure'i turuplatsi](https://azure.microsoft.com/marketplace/), või pildid ja teie andmete ketast<br/><br/>• Saate juurutada ja hallata kirjetena [VM skaala komplektid](https://azure.microsoft.com/documentation/services/virtual-machine-scale-sets/) koostamiseks suuremahuliste teenused ühesuguste virtuaalmasinates, autoscaling suurendamiseks või vähendamiseks võimsus automaatselt<br/><br/>• Käivita kohapealse Arvuta kobar tööriistade ja rakenduste täiesti pilveteenuses<br/><br/>
**[Pilveteenused](https://azure.microsoft.com/documentation/services/cloud-services/)**<br/><br/> |• Saab käitada suur arvutada rakenduste töötaja rolli aknad, ja haldab täiesti Azure'i virtuaalmasinates versiooni Windows Server<br/><br/>• Lubada scalable, usaldusväärseid rakendusi, kus töötab teenus (PaaS) mudel platvormi madal üldhalduskulusid<br/><br/>• Võib olla nõutav täiendavad tööriistad või arengu integreerida kohapealse HPC kobar lahendused
**[Paketi](https://azure.microsoft.com/documentation/services/batch/)**<br/><br/> |• Töötab samal ajal ja paketi suuremahuliste töökoormus täielikult hallatav teenus<br/><br/>• Pakub töö plaanimis- ja autoscaling virtuaalmasinates hallatavate kogumi.<br/><br/>• Võimaldab arendajatel abil saate koostada ja teenust kasutada rakendusi või pilveteenuses – luba olemasolevaid rakendusi<br/>

### <a name="storage-services"></a>Salvestusruumi teenused

Suur arvutada lahenduse tavaliselt toimib kogumi sisendandmete ja loob selle tulemuste andmed. Mõned kasutatud lahendused suurt arvutada Azure storage teenused on järgmised:

* [Bloobimälu, tabeli ja järjekorda salvestusruumi](https://azure.microsoft.com/documentation/services/storage/) - struktureerimata andmed, NoSQL andmed ja sõnumite töövoo ja suhtlus, suure hulga vastavalt haldamine. Näiteks võite kasutada bloobimälu tehnilise suurte andmekogumite, või Sisestuskeel pilte või meediumifailid oma protsesse. Võite kasutada järjekorrad asünkroonse suhtluse lahendus. Lugege [artikleid Sissejuhatus rakendusse Microsoft Azure'i salvestusruumi](../storage/storage-introduction.md).

* [Azure'i failide talletamine](https://azure.microsoft.com/services/storage/files/) - jagab levinud failide ja andmete Azure kasutada standard SMB protokolli, mida on vaja mõned HPC kobar lahendused.

* [Lake andmesalve](https://azure.microsoft.com/services/data-lake-store/) – pakub hyperscale Apache Hadoop hajutatud failisüsteemi pilves, abiks paketi reaalajas, ja interaktiivsed analytics.

### <a name="data-and-analysis-services"></a>Andmed ja analysis services

Mõnel juhul suur arvutada kaasata suuremahuliste andmete või andmeid, mis tuleb edasiseks töötlemiseks või analüüsi luua. Azure'i pakub mitu andmed ja analüüsi teenuseid, sh:

* [Andmete Factory](https://azure.microsoft.com/documentation/services/data-factory/) - järgud Andmepõhiste töövoogude (välistrasside) selle Liitu, liitmise ja transformatsioon andmeid asutusesiseselt, pilvepõhist ja Interneti-andmete poed.

* [SQL-andmebaasi](https://azure.microsoft.com/documentation/services/sql-database/) - klahv pakub Microsoft SQL Server relatsiooniandmebaasist korraldamise süsteemi hallatav teenus.

* [Hdinsightiga](https://azure.microsoft.com/documentation/services/hdinsight/) - kasutajaks ja sätted Windows Server või Linuxi-põhiste Apache Hadoop kogumite, et hallata, analüüsida ja suur andmete aruanne.

* [Seadme õppe](https://azure.microsoft.com/documentation/services/machine-learning/) - aitab luua, testida, kasutada ja täielikult hallatav teenus sõnastikupõhise analüütiliste lahenduste haldamine.

### <a name="additional-services"></a>Täiendavad teenused

Teie suur arvutada lahendus võib-olla muude Azure'i teenuste ühenduse kohapealse ressursid või muus keskkonnas. Näited:

* [Virtuaalse võrgu](https://azure.microsoft.com/documentation/services/virtual-network/) - loob loogiliselt eraldatud jaotise Azure ühenduse Azure'i ressursid või teie asutusesisese andmekeskuse. Asutusesiseses virtuaalse võrguga suur arvutada rakendused pääsevad kohapealsed andmed, Active Directory teenused ja litsentsi

* [ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/) – loob privaatse Microsofti andmekeskuste ja taristu, mis on kohapealse või kaasautorluse kohta keskkonnas. ExpressRoute pakub suurem turvalisus, rohkem usaldusväärsus, kiirem kiirust ning väiksem kui tüüpiline ühendused latentsused Interneti kaudu.

* [Teenuse siini](https://azure.microsoft.com/documentation/services/service-bus/) - pakub käsutuses rakenduste suhelda või vahetada andmeid, kas need asuvad Azure, teise pilve platvormi või andmekeskuse.

## <a name="next-steps"></a>Järgmised sammud

* Vt [Tehnilise ressursid paketi ja HPC](big-compute-resources.md) tehnilisi juhiseid luua oma lahenduse leidmiseks.

* Partnerid, sh tsükkeldiagramm arvuti- ja UberCloud arutada Azure'i võimalusi.

* Lugege [Towers Watson](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18222), [Altair](https://azure.microsoft.com/blog/availability-of-altair-radioss-rdma-on-microsoft-azure/), [ANSYS](https://azure.microsoft.com/blog/ansys-cfd-and-microsoft-azure-perform-the-best-hpc-scalability-in-the-cloud/)ja [d3VIEW](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=22088)Azure suur arvutada lahenduste kohta.

* Uudised, vt [Microsoft HPC ja paketi meeskonna ajaveebi](http://blogs.technet.com/b/windowshpc/) ja [Azure ajaveebi](https://azure.microsoft.com/blog/tag/hpc/).

<!--Image references-->
[parallel]: ./media/batch-hpc-solutions/parallel.png
[coupled]: ./media/batch-hpc-solutions/coupled.png
[iaas_cluster]: ./media/batch-hpc-solutions/iaas_cluster.png
[burst_cluster]: ./media/batch-hpc-solutions/burst_cluster.png
[batch_proc]: ./media/batch-hpc-solutions/batch_proc.png
