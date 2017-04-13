<properties
    pageTitle="Mis on andmete teadus virtuaalse masina? | Microsoft Azure'i"
    description="Siit saate teada, loetletud tähtsaimad stsenaariumid, funktsioone, ja kuidas alustada koos andmete teadus Virtuaalmasinates, keskkond ja valmis analüüsi tööriistakomplekti."
    keywords="andmete teadus tööriistad, andmete teadus virtuaalse masina, andmete teadus, linux andmete teadus tööriistad"
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="bradsev" />


# <a name="introduction-to-the-cloud-based-data-science-virtual-machine-for-linux-and-windows"></a>Sissejuhatus abil pilvepõhise andmete teadus virtuaalse masina Windows ja Linux

Andmete teadus virtuaalse masina on kohandatud VM pilt Microsoft Azure'i pilve ehitatud spetsiaalselt tehes andmete teadus. See on paljude populaarsete andmete teadus- ja muude tööriistade eelinstallitud ja eelnevalt konfigureeritud hüpata-Start nutikad rakenduste täiustatud analüüsi jaoks. See on saadaval Windows Server 2012 või Linuxi OpenLogic 7.2 CentOS-põhine versioonid. 

Selles teemas käsitletakse, mida saab teha andmete teadus VM, esitatakse mõned VM kasutamise stsenaariumid, itemizes versioonid Windowsi ja Linux olulisi funktsioone ja antakse juhiseid nende kasutamisega alustamise kohta.


## <a name="what-can-i-do-with-the-data-science-virtual-machine"></a>Mida teha, koos andmete teadus virtuaalse masina?

Andmete teadus virtuaalse masina eesmärk on pakkuda spetsialistide kõik oskuste tasemed ja rolli hõõrdumise tasuta andmed teadus keskkonnas. See VM säästab palju aega, et teil oleks kulutada kui oli võetud sarnastes keskkonnas välja ise. Selle asemel käivitage andmete teadus projekti kohe vastloodud VM eksemplari. 

Andmete teadus VM on loodud ja konfigureeritud laialdane kasutamise stsenaariumid töötamiseks. Saate skaala keskkonna, üles või alla projekti vajadused muutuvad. Teil on võimalik kasutada oma eelistatud keeles programmi teadus tööülesannetesse. Saate installida muid tööriistu ja kohandada süsteemi jaoks teie vajadustele.
 
## <a name="key-scenarios"></a>Üles loetletud tähtsaimad stsenaariumid
Selles jaotises soovitab mõned üles loetletud tähtsaimad stsenaariumid loovad andmete teadus VM.

### <a name="preconfigured-analytics-desktop-in-the-cloud"></a>Eelkonfigureeritud analytics töölaua pilveteenuses

Andmete teadus VM pakub võrdlusalus konfiguratsiooni andmete teadus meeskonnad, kas soovite oma kohaliku lauaarvutite asendamine hallatavate cloud töölaua. See võrdlusalus tagab, et kõik andmed teadlased meeskond on ühtsete häälestamine, mille abil kontrollida katsete ja edendada koostöö. See vähendab kulud süsteemiadministraator vähendamiseks ja hinnata, installida ja hallata on vaja teha täiustatud analüüsi erinevate tarkvarapakettide aega säästa.  

### <a name="data-science-training-and-education"></a>Koolitus andmete teadus- ja

Ettevõtte treenerid ja õpetajad, et õpetavad andmete teadus tunnid pakuvad virtuaalse masina pilt tagamaks, et oma õpilasi ühtsete häälestamise ja et näidised tööta ootuspäraselt. Andmete teadus VM loob nõudmisel keskkonna ühtsete häälestamine, mis lihtsustab tugi ja ühildamatus probleeme. Juhul, kui ehitatakse sageli, eriti puhul lühemaks koolitus, peavad nendes keskkondades kasu saada.

### <a name="on-demand-elastic-capacity-for-large-scale-projects"></a>Nõudmisel elastne võimsus suuremahuliste projektide jaoks

Andmete teadus hackathons/võistlustel või suuremahuliste andmete modelleerimise ja avastamine nõua mastaabitud riistvara võimsus, tavaliselt lühiajaline välja. Andmete teadus VM aitab andmete teadus keskkonnas kiiresti nõudmisel katsete nõudva võimsusega ressursside käivitamise lubamiseks mastaabitud välja serverites korrata.

### <a name="short-term-experimentation-and-evaluation"></a>Lühiajaline katsetamine ja hindamine

Andmete teadus VM saab kasutada hinnata või lugege Microsoft R Server, SQL Server, näiteks Visual Studio tööriistad, Jupyter, sügav Õppekeskuse / ML tööriistakomplektid ja uute tööriistade populaarne ühenduse minimaalsete setup peegeldav. Kuna andmete teadus VM saate häälestada kiiresti, seda saab rakendada muid lühiajaline kasutamise stsenaariumid, nt imitatsiooniga avaldatud katsete käivitamisel demos järgmised juhendavad tutvustused online seansid või konverentsi õppematerjalid.


## <a name="whats-included-in-the-data-science-vm"></a>Mida sisaldab andmeid teadus VM?

Andmete teadus virtuaalse masina on juba installinud ja konfigureerinud paljude populaarsete andmete teadus tööriistad. See hõlmab ka tööriistu, mis muudavad töötamine erinevate Azure'i andmed ja analytics tooted. Saate uurida ja luua prognoosmudelite suurte andmekogumite kasutades Microsoft R serveri või SQL Server 2016. Host muude tööriistade avatud allikas ja Microsoft on kaasatud, samuti proovi kood ja märkmikud. Järgmises tabelis itemizes ja võrreldakse Windows ja Linux väljaannetes andmete teadus virtuaalse masina põhikomponentide.


|**Windows Edition** | **Linux väljaanne** |
|----------------|---------------|
|Microsoft R Server arendaja Edition | Microsoft R Server arendaja Edition |
|Anaconda Python 2.7, 3.5 | Anaconda Python 2.7, 3.5 |
|Jupyter märkmiku Server (R, Python) | JupyterHub: Mitme kasutaja Jupyter märkmike (R, Python, Julia) |
|SQL Server 2016 arendaja Edition: Scalable-andmebaasi analytics R teenustega | Postgres, orav SQL-i (andmebaasiriista), SQL serveri draiverid ja käsurea (bcp, sqlcmd) |
|Visual Studio ühenduse Edition 2015 (IDE) </br> -Azure Hdinsightiga (Hadoopi) andmete Lake, SQL Server Data tools </br> -R Node.js ja Python Visual Studio tööriistad |Interaktiivset ja redaktorid </br> -Eclipse Azure tööriistakomplekt lisandmooduli abil </br> -(Koos ESS, auctex) Emacs gedit |
|Power BI Desktopi | -- |
|Õppekeskuse tööriistad </br> -Integreerimine Azure seadme õpetused </br> -CNTK (sügav õ AI) </br> -Xgboost (Populaarsed ML tööriista andmete teadus võistlustel) </br> -Vowpal Wabbit (kiire online õppurite) </br> -Rattle (visuaalsed kiire alustada andmed ja veebianalüütika tööriist) </br> -Mxnet (sügav õ AI) | Õppekeskuse tööriistad </br> -Integratsioon, mille Azure seadme õpetused </br> -CNTK (sügav õ AI) </br> -Xgboost (Populaarsed ML tööriista andmete teadus võistlustel) </br> -Vowpal Wabbit (kiire online õppurite) </br> -Rattle (visuaalsed kiire alustada andmed ja veebianalüütika tööriist)  |
| SDK-d juurdepääsu Azure ja Cortana ärianalüüsi komplekti teenused | SDK-d juurdepääsu Azure ja Cortana ärianalüüsi komplekti teenused |
| Tööriistad andmete liikumise ja Azure ja Big Data vahendite haldamine: Azure'i salvestusruumi Explorer, CLI, PowerShelli, AdlCopy (Azure'i andmed Lake), AzCopy, dtui (DocumentDB) jaoks Microsofti Andmehalduslüüs | Tööriistad andmete liikumise ja Azure ja Big Data management: Azure'i salvestusruumi Explorer, CLI |
| Git, Visual Studio Team Services lisandmoodul | Git |
| Windowsi pordi kõige populaarsemate Linuxi/Unixi käsurea Utiliidid GitBash/Käsuviip kaudu. | -- |



## <a name="how-to-get-started-with-the-windows-data-science-vm"></a>Kuidas alustada Windows andmete teadus VM

- Eksemplari VM Windows [sellele](https://azure.microsoft.com/marketplace/partners/microsoft-ads/standard-data-science-vm/) lehele liikumine ja valige roheline **virtuaalse masina loomiseks** nuppu Loo.
- Logige sisse VM remote töölaualt määratud VM loomisel mandaadi abil.
- Leida ja käivitage tööriistu, mis on saadaval, klõpsake menüü **Start** .


## <a name="get-started-with-the-linux-data-science-vm"></a>Linux andmete teadus VM kasutamise alustamine

- Luua eksemplari VM Linux (OpenLogic CentOS-põhine) [sellele](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm/) lehele liikumine ja klõpsake nuppu **Loo virtuaalse masina** .
- Logige sisse VM SSH klient, nt Putty või SSH käsk, määratud VM loomisel mandaadi abil.
- Shellis, sisestage dsvm-veel-teave.
- Graafilise töölaua, laadige alla oma kliendi platvormi X2Go klienti [siin](http://wiki.x2go.org/doku.php/doc:installation:x2goclient) ja järgige juhiseid teemas Linux andmete teadus VM dokumendi [sätte Linux andmete teadus virtuaalse masina](machine-learning-data-science-linux-dsvm-intro.md#installing-and-configuring-x2go-client).


## <a name="next-steps"></a>Järgmised sammud

### <a name="for-the-windows-data-science-vm"></a>Windows andmete teadus VM

- Kindla saadaolevate tööriistade käivitamise Windowsi versiooni kohta leiate lisateavet teemast [sätte Microsoft andmete teadus virtuaalse masina](machine-learning-data-science-provision-vm.md) ja
-  Lisateavet selle kohta, kuidas sooritada erinevaid andmete teadus projekti VM Windowsi jaoks vajalik ülesandeid, leiate [kümme asja, mida saate teha andmete teadus virtuaalse masina](machine-learning-data-science-vm-do-ten-things.md).

### <a name="for-the-linux-data-science-vm"></a>Linux andmete teadus VM

- Käivitamise Linuxi versiooni teatud saadaolevate tööriistade kohta leiate lisateavet teemast [sätte Linux andmete teadus virtuaalse masina](machine-learning-data-science-linux-dsvm-intro.md).
- Lühiülevaade, mis näitab, kuidas teha mitme levinud andmete teadus ülesannete Linuxi, leiate teemast [andmete teadus kohta Linux andmete teadus virtuaalse masina](machine-learning-data-science-linux-dsvm-walkthrough.md).
