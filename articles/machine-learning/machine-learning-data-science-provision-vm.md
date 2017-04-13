<properties 
    pageTitle="Microsoft andmete teadus virtuaalse masina ettevalmistamine | Microsoft Azure'i" 
    description="Konfigureerige ja luua andmete teadus virtuaalse masina Azure teha analüüsi ja masina õ." 
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/07/2016" 
    ms.author="bradsev" />


# <a name="provision-the-microsoft-data-science-virtual-machine"></a>Microsoft Data Science virtuaalse masina ettevalmistamine

Microsoft andmete teadus virtuaalse masina on Azure virtuaalse masina (VM) pilt eelnevalt installinud ja konfigureerinud mitu populaarsed tööriistu, mis on tavaliselt kasutatakse andmete analüüsimise ja arvuti koolitus. On tööriistu.

- Microsoft R Server arendaja Edition
- Anaconda Python jaotuse.
- Jupyter märkmiku (koos R, Python tuumad)
- Visual Studio ühenduse Edition
- Power BI Desktopi
- SQL Server 2016 arendaja Edition
- Õppekeskuse tööriistad
    - [Arvutuslik võrgu tööriistakomplekti (CNTK)](https://github.com/Microsoft/CNTK): on suure Õppekeskuse tarkvara tööriistakomplekti Microsoft Researchi kaudu.
    - [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit): õppe süsteemi toetavad võrgus, loob rakendus, allreduce, vähendamine, learning2search, aktiivne, näiteks kiire arvuti ja interaktiivse õpetused.
    - [XGBoost](https://xgboost.readthedocs.org/en/latest/): vahend, mis on kiire ja täpne võimendatud puu rakendamist.
    - [Rattle](http://rattle.togaware.com/) (selle R Analytical tööriista abil saate teada, hõlpsasti): tööriista, mis teeb andmete analüüsimise ja seadme Õppekeskuse r lihtne – põhise andmete uurimine ja modelleerimine koos R koodi automaatseks töötamise alustamine.
    - [mxnet](https://github.com/dmlc/mxnet): sügav õ raamistiku mõeldud nii tõhusust ja paindlikkust
- Teekide ja Python jaoks kasutada Azure seadme õppimine ja muud Azure teenused
- Git, sh Git Bash source code hoidlate sh GitHub, Visual Studio Team Services töötamiseks
- Windowsi pordid mitu populaarsed Linuxi käsurea utiliidid (sh awk, sed, perl, grep, otsing, wget, curl jne) kaudu Käsuviip. 


Tehke andmete teadus hõlmab teel tööülesannete jada: otsimine, laadimine ja eelnevalt töötlemiseks andmete koostamise ja testimine mudelite juurutamine näidised tarbimine nutikad rakendustes. Andmeteadlaste kasutada mitmesuguseid tööriistu nende toimingute tegemiseks. See võib olla üsna aeganõudev vastav tarkvara, versioonide otsimine ja seejärel laadige alla ja installige need. Microsoft andmete teadus virtuaalse masina saab kasutada valmis pilt, mida saab ette valmistatud Azure pakkuda kõik mitme levinud tööriista eelnevalt installinud ja konfigureerinud leevendada seda koormust. 

Microsoft andmete teadus virtuaalse masina jump-starts analytics projekti. See võimaldab töötada tööülesannete erinevates keeltes, sh R, Python, SQL-i ja C#. Visual Studio pakub on IDE arendamise ja testige oma kood, mis on lihtne kasutada. Azure'i SDK kaasatud VM võimaldab teil luua rakenduste abil mitmesuguste Microsofti pilveteenuste platvormil. 

On tarkvara maksud andmete teadus VM pilt. Maksate Azure kasutuse tasud mis olete ette virtuaalse masina suurus sõltub. [Andmete teadus virtuaalse masina](https://azure.microsoft.com/marketplace/partners/microsoft-ads/standard-data-science-vm/) lehel jaotises hinnakirjad üksikasjad leiate üksikasjalikumat teavet Arvuta tasud. 


## <a name="prerequisites"></a>Eeltingimused

Enne kui saate luua Microsoft andmete teadus virtuaalse masina, peab teil olema järgmised:

- **An Azure'i tellimus**: saamiseks, leiate [Azure'i saada tasuta prooviversioon](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

*   **An Azure storage konto**: loomiseks leiate [Azure'i salvestusruumi konto loomine](storage-create-storage-account.md#create-a-storage-account). Teise võimalusena salvestusruumi konto loomist VM luua, kui te ei soovi kasutada konto osana.


## <a name="create-your-microsoft-data-science-virtual-machine"></a>Microsoft andmete teadus virtuaalse masina loomine

Siin on toimingud, Microsoft andmete teadus virtuaalse masina eksemplari loomine.

1.  Liikuge virtuaalse masina loetelu [Azure'i](https://portal.azure.com/#create/microsoft-ads.standard-data-science-vmstandard-data-science-vm)portaalis.
2.   Valige viisardi tuleb arvestada allservas nuppu **Loo** . ![konfigureerimine-andmete-teadus-vm](./media/machine-learning-data-science-provision-vm/configure-data-science-virtual-machine.png)
3.   Microsoft andmete teadus virtuaalse masina loomiseks kasutatud viisardi nõuab **sisendina** iga sellel joonisel paremal loetletud **viis juhist** . Siin on vaja konfigureerida neid juhiseid iga sisendeid.
    
    1.   **Põhitõed**
        1.   **Nimi**: teie andmete teadus serveri nimi.
        2.   **Kasutajanimi**: administraatori konto sisselogimise id.
        3.   **Parooli**: administraatori konto parooli.
        4.   **Tellimus**: kui teil on mitu tellimust, valige üks seade on loodud ja arve.
        5.   **Ressursirühm**: saate luua uue või olemasoleva rühma kasutada.
        6.   **Asukoht**: valige data Centeri kaudu, mis on kõige sobivam. Tavaliselt on data Centeri kaudu, mis on andmete või on kõige lähemal füüsilise asukoha jaoks kiireim võrgule juurdepääsu.
         
    2.   **Suurus**: saate valida ühe serveritüübid, mis vastab teie otstarbekas nõue ja maksumus piiranguid. Saate VM suuruses rohkem valikuid, valides "Kuva kõik".
    
    3.   **Sätted**:
        1.   **Ketas tüüp**: valige Premium kui eelistate välkdraiv (SSD) veel valige "Standard".
        2.   **Salvestusruumi konto**: saate luua uue Azure storage konto teie tellimus või **alused** etapil viisardi kasutamine olemasoleva valiti samasse *asukohta* .
        3.   **Muud parameetrid**: tavaliselt lihtsalt kasutage vaikeväärtust. Saate kursoriga üle teavitamise lingi abi kindlad väljad juhul võite soovida-vaikeväärtuste kasutamine.

    4.   **Kokkuvõte**: Veenduge, et kõik sisestatud teave on õige.
    5.   **Osta**: klõpsake nuppu **osta** soovitud ettevalmistamise käivitamiseks. Tehingu tingimuste on toodud link. VM ei saa mis tahes lisatasu lisaks Arvuta **suurus** juhises valitud serveri suurus. 


>[AZURE.NOTE] Funktsiooni ettevalmistamise võtma umbes 10 – 20 minutit. Selle ettevalmistamise olek kuvatakse Azure'i portaalis.

## <a name="how-to-access-the-microsoft-data-science-virtual-machine"></a>Kuidas pääseda juurde Microsoft andmete teadus virtuaalse masina

Kui VM on loodud, saate sinna kasutades eelmises jaotises **põhialused** konfigureeritud administraatori mandaat Kaugtöölaud. 

Kui teie VM on loodud ja ette valmistatud, olete valmis tööriistu, mis on installinud ja konfigureerinud seda kasutama hakata. On start menüü paanid ja töölaua ikoonide paljude tööriistade jaoks. 

## <a name="how-to-create-a-strong-password-on-the-jupyter-notebook-server"></a>Kuidas serveris Jupyter märkmiku keeruka parooli loomine 

Keeruka parooli loomine Jupyter märkmiku serveri arvutisse installitud, käivitage järgmine käsk käsureale – klõpsake andmete teadus virtuaalse masina.

    c:\anaconda\python.exe -c "import IPython;print IPython.lib.passwd()"

Valige keeruka parooli küsimise.

Näete parooli räsi vormingus "sha1:xxxxxx" väljund. Kopeerige see parooli räsi ja asendada olemasoleva räsi, mis asub teie märkmik config failid, mis asuvad: **C:\ProgramData\jupyter\jupyter_notebook_config.py** parameetri nimi ***c.NotebookApp.password***abil.

Ainult ([kuupäev]) osa olemasoleva räsi väärtus, mis on sees jutumärkidega asendada. Tsitaadid ja ***sha1:*** eesliide parameetri väärtus mõlemad tuleb säilitada.

Lõpetuseks, peate uuesti käivitada Jupyter server, mis töötab Windowsi ajastatud nimetatakse **Start_IPython_Notebook**VM. Uue parooli vastuvõtmata pärast selle toimingu taaskäivitada, proovige tappes kõik töötavad python protsessid kaudu Tegumihaldur ja seejärel taaskäivitage Ajastatud ülesanne. Vaheldumisi, proovige taaskäivitus virtuaalse masina.

## <a name="tools-installed-on-the-microsoft-data-science-virtual-machine"></a>Klõpsake Microsoft andmete teadus virtuaalse masina installitud tööriistad

### <a name="microsoft-r-server-developer-edition"></a>Microsoft R Server arendaja Edition
Kui soovite kasutada oma analytics R, VM on installitud Microsoft R Server arendaja edition. Äriteave R on üldjoontes käivituva äriklassi analytics platvorm põhjal R, mis on toetatud, scalable ja turvaline. Toetavad erinevaid suur andmete statistika, ennustava modelleerimine ja seadme Õppekeskuse võimalusi, R Server toetab kõiki analytics – avastamine, analüüsi, visualiseerimine ja modelleerimine. Ulatub avatud allika R ja kasutades Microsoft R Server on R skriptide, funktsioonid ja CRAN pakettide enterprise skaala andmete analüüsimiseks, ühilduda. Samuti käsitletakse avatud allika R-mälu piirangud, lisades paralleelselt ja chunked andmete töötlemiseks. See võimaldab teil käivitamiseks analytics andmete palju suuremad kui, mis sobib peamised mälu.  Visual Studio ühenduse väljaanne sisaldab VM sisaldab R Tools for Visual Studio laiend, mis pakub täielikku IDE töötamiseks R. Saate alla laadida ja kasutada muid interaktiivset samuti nagu [RStudio](http://www.rstudio.com). 

### <a name="python"></a>Python
Kasutades Python arengu Anaconda Python jaotuse 2.7 ja 3.5 on installitud. See jaotus sisaldab base Python koos umbes 300 kõige populaarsemate matemaatika, matemaatika ja andmete analüüsi paketid. Saate kasutada Python tööriistad for Visual Studio (PTVS) Visual Studio 2015 ühenduse edition või mõne kombineeritud interaktiivset Anaconda nagu JÕUDEOLEKU või Spyder abil installitud. Saate ühe neist otsinguriba otsingu kaudu käivitada (**võidavad** + klahvi**S** ).

>[AZURE.NOTE] Osutama Python tööriistad Visual Studio Anaconda Python 2.7 ja 3.5, peate looma kohandatud keskkonnas iga versiooni. Liikuge seadmiseks Visual Studio 2015 ühenduse väljaande need keskkonna teed **Tööriistad** -> **Python tööriistad** -> **Python keskkonnas** ja seejärel klõpsake nuppu **+ kohandatud**. 

Anaconda Python 2.7 on installitud jaotises C:\Anaconda ja Anaconda Python 3.5 on installitud jaotises c:\Anaconda\envs\py35. Üksikasjalikud juhised [PTVS](https://github.com/Microsoft/PTVS/wiki/Selecting-and-Installing-Python-Interpreters#hey-i-already-have-an-interpreter-on-my-machine-but-ptvs-doesnt-seem-to-know-about-it) dokumentatsioonist. 

### <a name="jupyter-notebook"></a>Jupyter märkmik
Anaconda jaotuse kaasas Jupyter märkmikku, koodi ja analüüsi jagamiseks keskkonnas. Jupyter märkmiku server on konfigureeritud eelnevalt Python 2, Python 3 ja R tuumad. Töölaua ikooni, nimega "Jupyter märkmik brauseri märkmiku serverit käivitada. Kui kasutate VM kaudu kaugtöölaua, võite ka külastada [https://localhost:9999 /](https://localhost:9999/) Jupyter märkmiku serverit, kui VM sisse logitud.
 
>[AZURE.NOTE] Jätkake, kui saate serdi hoiatusi. 

Meil on pakitud valimi märkmike Python ja R. Märkmike Jupyter näitab, kuidas töötada Microsoft R Server, SQL Server 2016 R Services (-andmebaasi analytics), Python ja Azure tehnoloogiad, kui logite Jupyter. Näete linki näidised märkmiku avalehel pärast saate autentida Jupyter märkmikku mõne varasema juhises loodud parooli abil. 

### <a name="visual-studio-2015-community-edition"></a>Visual Studio 2015 ühenduse edition
Visual Studio ühenduse edition installitud VM. See on tasuta versiooni populaarsed IDE Microsofti kasutatavad hindamise ja väikestel meeskondadel. Saate vaadata selle litsentsitingimused läbi [siin](https://www.visualstudio.com/support/legal/mt171547).  Visual Studio avamiseks topeltklõpsake töölaual ikooni või menüü **Start** . Saate otsida ka programmid **võidavad** + **S** ja sisestamisega "Visual Studio". Kui seal saate luua keeltes nagu C#, Python, R, node.js. Lisandmoodulid on installitud ka mis muudavad selle mugav Azure teenuseid, nagu Azure'i andmekataloogi, Windows Azure Hdinsightiga (Hadoopi, säde) ja Azure andmete Lake töötamiseks. 

>[AZURE.NOTE] Võidakse kuvada teade selle kohta, et teie hindamise tähtaja möödumist. Sisestage oma Microsofti konto identimisteave või saada juurdepääsu Visual Studio ühenduse Edition tasuta uue konto loomine. 

### <a name="sql-server-2016-developer-edition"></a>SQL Server 2016 arendaja edition
Arendaja versiooni SQL Server 2016 R teenustega-andmebaasi analytics käivitamiseks on saadaval VM. R teenuseid pakkuda platvormi arendamise ja juurutamine nutikad rakendused. Saate rikas ja võimas R keele- ja palju pakette ühenduse loomiseks mudelite ja luua prognoose SQL serveri andmeid. Hoiate analytics andmete lähedale, kuna R Services (klõpsake andmebaasi) R keele integreerida SQL serveri. See kaob kulusid ja turvalisuse riske andmete liikumine.

>[AZURE.NOTE] SQL Server 2016 arendaja väljaande saab kasutada ainult arengu ja katsete tegemiseks. Teil on vaja seda käivitamiseks valmistamisel litsentsi. 

Pääsete SQL serveri **SQL Server Management Studio**käivitades. Teie VM nime sisustatakse nime. Kasutage Windows sisse logitud administraatorina sisse Windowsi autentimist. Kui olete SQL Server Management Studio saate luua teistele kasutajatele, luua andmebaase, andmete importimine ja käivitage SQL-päringud. 

Kasutades Microsoft R-andmebaasi analytics lubamiseks käivitage järgmine käsk on üks kord toimingu SQL Server management Studio pärast sisselogimist nagu serveri administraator. 

        CREATE LOGIN [%COMPUTERNAME%\SQLRUserGroup] FROM WINDOWS 
        
        (Please replace the %COMPUTERNAME% with your VM name)


### <a name="azure"></a>Azure'i 
VM on installitud mitu Azure'i Tööriistad:

- Töölaua otsetee Azure'i SDK dokumentatsiooni juurdepääsuks on. 
- **AzCopy**: Microsoft Azure salvestusruumi konto ja sealt andmete teisaldamiseks kasutada. Kui soovite vaadata kasutus, tippige käsuviibale kasutamine kuvamiseks **Azcopy** . 
- **Microsoft Azure'i salvestusruumi Explorer**: objektid, mis on talletatud sees ja sealt Azure storage Azure Storage konto ja edastamine andmete sirvimiseks kasutada. Saate **Salvestusruumi Exploreri** Tippige väljale Otsi või leida selle Windowsi menüü Start tööriistale juurdepääsemiseks. 
- **Adlcopy**: Azure'i andmed Lake andmete teisaldamiseks kasutada. Tippige käsuviiba **adlcopy** kasutuse vaatamiseks. 
- **dtui**: kasutatud andmete teisaldamiseks ja sealt Azure'i DocumentDB NoSQL andmebaasi pilve. Tippige käsuviiba **dtui** . 
- **Microsofti Andmehalduslüüs**: võimaldab andmete liikumine andmeallikate kohapealse ja pilveteenuse vahel. Seda kasutatakse tööriistad nagu Azure'i andmed Factory sees. 
- **Microsoft Azure'i PowerShelli**: tööriista haldamise skriptimiskeele installitud on ka teie VM PowerShellis oma Azure ressursse. 

###<a name="power-bi"></a>Power BI

Abil saate koostada armatuurlaudade ja hea visualiseeringuid **Power BI Desktopi** on installitud. Kasutage seda tööriista pull andmetele erinevatest allikatest, oma armatuurlaudade ja aruannete koostamine ja avaldamine pilve. Lisateavet leiate teemast [Power BI](http://powerbi.microsoft.com) saidile. Klõpsake menüü Start leiate Power BI Desktopi. 

>[AZURE.NOTE] Teil on vaja Office 365 konto juurdepääsu Power BI. 

## <a name="additional-microsoft-development-tools"></a>Täiendavad Microsoft arengu tööriistad
[**Microsoft Web platvormi Installer**](https://www.microsoft.com/web/downloads/platform.aspx) saab avastada ja muude Microsofti arengu tööriistad. Olemas on ka tööriist, Microsoft andmete teadus virtuaalse masina töölaua otsetee.  

## <a name="important-directories-on-the-vm"></a>Oluliste kataloogide VM

| Üksuse                          | Kataloog |
| ------------------------------| ---------------- |
|Jupyter märkmiku serveri konfiguratsiooni | C:\ProgramData\jupyter |
|Jupyter märkmiku näidised home kataloog| c:\dsvm\notebooks|
|Muud näidised | c:\dsvm\samples|
| Anaconda (vaikimisi: Python 2.7) | c:\Anaconda |
| Anaconda Python 3.5 keskkonnas | c:\Anaconda\envs\py35|
|Autonoomse R serveri eksemplari directory (vaikimisi R eksemplari) | C:\Program Files\Microsoft SQL Server\130\R_SERVER |
| R Server-andmebaasi eksemplari kataloog | C:\Program Files\Microsoft SQL Server\MSSQL13. MSSQLSERVER\R_SERVICES |
| Mitmesugused tööriistad | c:\dsvm\tools|

>[AZURE.NOTE] Eksemplari, Microsoft andmete teadus virtuaalse masina enne 1.5.0 (mai 3, 2016) enne kasutada veidi teistsugused kataloogi struktuuri, kui eelneva tabelis. 

## <a name="next-steps"></a>Järgmised sammud
Siin on mõned järgmised toimingud jätkata oma õppimine ja uurimine. 

* Tutvuge erinevate Andmeriistad teadus andmete teadus VM, klõpsates menüü start ja väljamöllimiseks loetletud menüü Tööriistad.
* Liikuge **C:\Program Files\Microsoft SQL Server\130\R_SERVER\library\RevoScaleR\demoScripts** RevoScaleR teegi r andmeanalüüsi ettevõtte tasandil toetab proovi.  
* Lugege artiklit: [10 saate teha andmete Science virtuaalse masina](http://aka.ms/dsvmtenthings)
* Saate teada, kuidas luua lõpuni analytical lahendusi korrapäraselt [Meeskonnatöö andmete teadus protsessi](https://azure.microsoft.com/documentation/learning-paths/data-science-process/)abil.
* Külastage kohapeal õppimine ja andmete analytics näidised, mida kasutada Cortana ärianalüüsi komplekti [Cortana ärianalüüsi Galerii](http://gallery.cortanaintelligence.com) . Oleme ka andnud ikoon menüüs **Start** ja töölaual virtuaalse masina selle galerii.

