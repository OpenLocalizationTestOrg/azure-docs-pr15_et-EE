<properties
    pageTitle="Ettevalmistamise Linux andmete teadus virtuaalse masina | Microsoft Azure'i"
    description="Konfigureerige ja luua Linux andmete teadus virtuaalse masina Azure teha analüüsi ja masina õ."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/12/2016"
    ms.author="bradsev" />

# <a name="provision-the-linux-data-science-virtual-machine"></a>Linux andmete teadus virtuaalse masina ettevalmistamine

Linux andmete teadus virtuaalse masina on Azure virtuaalse masina kaasneva eelinstallitud tööriistade kogum. Nende tööriistade kasutatakse tavaliselt tehes andmete analüüsimise ja masina õ. Võtme tarkvara komponendid on:

- Microsoft R Server arendaja Edition
- Anaconda Python jaotuse (versioonid 2.7 ja 3.5), sh populaarsed andmete analüüsi teegid
- JupyterHub - R-Python, Julia tuumad toetavad kasutajaga Jupyter märkmiku server
- Azure'i salvestusruumi Explorer
- Azure'i käsurea liides (CLI) haldamise Azure'i ressursid
- PostgresSQL andmebaas
- Õppekeskuse tööriistad
    - [Arvutuslik võrgu tööriistakomplekti (CNTK)](https://github.com/Microsoft/CNTK): on suure Õppekeskuse tarkvara tööriistakomplekti Microsoft Researchi kaudu.
    - [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit): õppe süsteemi toetavad võrgus, loob rakendus, allreduce, vähendamine, learning2search, aktiivne, näiteks kiire arvuti ja interaktiivse õpetused.
    - [XGBoost](https://xgboost.readthedocs.org/en/latest/): vahend, mis on kiire ja täpne võimendatud puu rakendamist.
    - [Rattle](http://rattle.togaware.com/) (selle R Analytical tööriista abil saate teada, hõlpsasti): tööriista, mis teeb andmete analüüsimise ja seadme Õppekeskuse r lihtne – põhise andmete uurimine ja modelleerimine koos R koodi automaatseks töötamise alustamine.
- Azure'i SDK Java, Python, node.js, Ruby, PHP
- Teekide ja Python jaoks kasutada Azure seadme õppimine ja muud Azure teenused
- Arengu tööriistade ja redaktorid (Eclipse, Emacs, gedit vi)

Tehke andmete teadus hõlmab teel tööülesannete jada:

1. Leidmine, laadimine ja eeltöötlus andmed
2. Koostamise ja mudelite testimine
3. Selles nutikas rakenduste mudelite juurutamine

Andmeteadlaste kasutada erinevaid vahendeid järgmiste toimingute tegemiseks. See on üsna aeganõudev vastav tarkvara versioonide otsimine ja seejärel alla laadida, koostada ja installida neid versioone.

Linux andmete teadus virtuaalse masina võivad seda koormust oluliselt leevendada. Selle abil saate hüpata-Start analytics projekti. See võimaldab töötada erinevates keeltes, sh R, Python, SQL-i, Java ja C++ tööülesanded. Eclipse pakub on IDE arendamise ja testige oma kood, mis on lihtne kasutada. Azure'i SDK kaasatud VM võimaldab teil koostada rakenduste abil mitmesuguste Linux Microsofti pilveteenuste platvormi. Lisaks on teil juurdepääs teistes keeltes nagu Ruby, Perl, PHP ja node.js, mis on ka eelnevalt installitud.

On tarkvara maksud andmete teadus VM pilt. Maksate ainult Azure riistvara kasutus tasud, mida hinnatakse virtuaalse masina pildiga VM sättes suurusest. Üksikasjalikumat teavet Arvuta tasud leiate [Azure'i turuplatsil lehele loetelu VM ](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm/).


## <a name="prerequisites"></a>Eeltingimused

Enne kui saate luua Linux andmete teadus virtuaalse masina, peab teil olema järgmist:

- **An Azure'i tellimus**: saamiseks, leiate [Azure'i saada tasuta prooviversioon](https://azure.microsoft.com/free/).
- **An Azure storage konto**: loomiseks leiate [Azure'i salvestusruumi konto loomine](storage-create-storage-account.md#create-a-storage-account). Teise võimalusena salvestusruumi konto loomist käigus loomise VM, kui te ei soovi kasutada konto.


## <a name="create-your-linux-data-science-virtual-machine"></a>Linux andmete teadus virtuaalse masina loomine

Siin on toimingud, Linux andmete teadus virtuaalse masina eksemplari loomine.

1.  Liikuge virtuaalse masina loetelu [Azure portaali](https://portal.azure.com/#create/microsoft-ads.linux-data-science-vmlinuxdsvm).
2.   Klõpsake nuppu **Loo** (allosas) avab viisard. ![konfigureerimine-andmete-teadus-vm](./media/machine-learning-data-science-linux-dsvm-intro/configure-linux-data-science-virtual-machine.png)
3.   Järgmistest jaotistest leiate Microsofti andmete teadus virtuaalse masina loomiseks kasutatud sisendeid iga (loetletud eespool näidatud joonisel paremal) viisardis kuvatavaid juhiseid. Siin on vaja konfigureerida neid juhiseid iga sisendeid.

    lisamine. **Põhitõed**:

  - **Nimi**: teie andmete teadus serveri nimi.
  - **Kasutajanimi**: esimese kontole sisselogimist ID-ga.
  - **Parooli**: esimese konto parooli (saate kasutada SSH avalik võti asemel parool).
  - **Tellimus**: kui teil on mitu tellimust, valige üks seade on loodud ja arve. Ressursside loomine õigusi selle tellimuse jaoks peab teil olema.
  - **Ressursirühm**: saate luua uue või olemasoleva rühma kasutada.
  - **Asukoht**: valige data Centeri kaudu, mis on kõige sobivam. Tavaliselt on data Centeri kaudu, mis on andmete või on kõige lähemal füüsilise asukoha jaoks kiireim võrgule juurdepääsu.

    b. **Suurus**:

  - Valige üks serveritüübid, mis vastab teie otstarbekas nõue ja maksumus piiranguid. Valige **Kuva kõik,** et kuvada rohkem valikuid VM suuruses.

    c. **Sätted**:

  - **Ketas tüüp**: valige **Premium** , kui eelistate ühtlase olekus SSD ketas. Muul juhul valige **Standard**.
  - **Salvestusruumi konto**: saate luua uue Azure storage konto teie tellimus, või kasutada mõne olemasoleva samasse asukohta, et valiti **põhitõed** järgu viisardi.
  - **Muud parameetrid**: enamasti lihtsalt saate vaikeväärtust. Kaaluge vaikeväärtused, kursorit teavitamise lingi abi kindlad väljad.

    d. **Kokkuvõte**:

  - Veenduge, et kõik sisestatud teave on õige.

    e. **Osta**:

  - Funktsiooni ettevalmistamise alustamiseks klõpsake nuppu **osta**. Tehingu tingimuste on toodud link. VM ei saa mis tahes lisatasu lisaks Arvuta **suurus** juhises valitud serveri suurus.

Funktsiooni ettevalmistamise võtma umbes 10 – 20 minutit. Selle ettevalmistamise olek kuvatakse Azure'i portaalis.

## <a name="how-to-access-the-linux-data-science-virtual-machine"></a>Kuidas pääseda juurde Linux andmete teadus virtuaalse masina

Pärast VM on loodud, saate sisselogimiseks seda SSH abil. Kasutage **põhitoimingute** osas juhist 3 teksti shell kasutajaliidese loodud mandaat. Windows, saate alla laadida ka SSH kliendi tööriista nagu [kitt](http://www.putty.org). Kui eelistate graafiline töölaua (X Windowsi süsteem), saate kasutada saatmise kitt X11 või X2Go kliendi installimine.

>[AZURE.NOTE] Kliendi X2Go läbi märkimisväärselt parem kui X11 testimine ümbersuunamine. Soovitame kasutada X2Go kliendi jaoks töölaua graafiliselt.


## <a name="installing-and-configuring-x2go-client"></a>Installimine ja konfigureerimine X2Go klient

Linux VM on juba ettevalmistatud X2Go serveriga ja kasutusvalmis vastu võtma kliendi ühendusi. Linux VM graafiline töölauarakenduse ühendamiseks tehke oma kliendi järgmist.

1. Laadige alla ja installige X2Go klienti oma kliendi platvormi [X2Go](http://wiki.x2go.org/doku.php/doc:installation:x2goclient).    
2. Käivitage X2Go klient ja valige **Uus seanss**. Nupp konfigureerimine mitme sakkidega. Sisestage konfiguratsiooni järgmisi:
    * **Seansi menüü**:
        - **Host**: hosti nimi või IP-aadress oma Linux andmeid teadus VM.
        - **Login**: Linuxi kasutaja nime.
        - **SSH Port**: jäta 22 vaikeväärtus.
        - **Seansi tüüp**: XFCE väärtust muuta. Linuxi toetab praegu ainult XFCE töölaud.
    * **Meediumi menüü**: heli tugi- ja kliendi printimist, kui te ei vaja neid kasutada saate välja lülitada.
    * **Ühiskaustad**: kataloogide soovi oma klientarvutite paigaldatud Linuxi lisada kliendi arvuti kataloogid, mida soovite ühiskasutusse anda selle vahekaardi VM.

Pärast seda, kui logite VM SSH kliendi või XFCE graafiline töölaua kaudu X2Go klient, olete valmis kasutama hakata tööriistu, mis on installinud ja konfigureerinud VM. XFCE, näete tööriistu paljude rakenduste otseteed ja töölaua ikoonid.


## <a name="tools-installed-on-the-linux-data-science-virtual-machine"></a>Klõpsake Linux andmete teadus virtuaalse masina installitud tööriistad

### <a name="microsoft-r-open"></a>Microsoft R Openi
R on üks kõige populaarsemate keeli andmeanalüüsi ja seadme õ. Kui soovite kasutada oma analytics R, VM on Microsoft R avatud (PRT) koos matemaatika tuum teek (MKL). Funktsiooni MKL optimeerib matemaatiliste toimingute levinud analytical algoritmide kohta. PRT on 100 protsenti ühildu CRAN-R ja mis tahes R teekide avaldatud CRAN saab installida soovitud PRT. Saate redigeerida ühe vaikimisi redaktorid, nt vi, Emacs või gedit R programmid. Saate alla laadida ja kasutada muid interaktiivset, nt [RStudio](http://www.rstudio.com). Teie mugavuse huvides on lihtne script (installRStudio.sh) toodud **/dsvm/tools** kataloogi, mis installitakse RStudio. Kui kasutate Emacs editor, teate, et selle Emacs paketti ESS (Emacs keeled statistika), mis lihtsustab sees Emacs editor, R failidega töötamine on eelinstallitud.

R algatada ainult tippimise **R** shell. See viib teid interaktiivse keskkonna. Arendada oma R programmi, saate kasutada tavaliselt nt Emacs või vi või gedit redaktorit ja seejärel käivitage skriptide sees R. Kui installite RStudio, on teil täielikku graafiline IDE keskkond arendada oma R programmi.

Olemas on ka R skripti saate installida [Top 20 R pakettide](http://www.kdnuggets.com/2015/06/top-20-r-packages.html) , kui soovite. Pärast seda, kui olete R interaktiivne kasutajaliides, mis saab sisestada (nagu mainitud), tippides **R** Shell käivitamist see skript.  

### <a name="python"></a>Python
Kasutades Python arengu Anaconda Python jaotuse 2.7 ja 3.5 on installitud. See jaotus sisaldab base Python koos umbes 300 kõige populaarsemate matemaatika, matemaatika ja andmete analüüsi paketid. Saate kasutada vaikimisi teksti redaktorid. Lisaks saate Spyder Python IDE, mis on kombineeritud Anaconda Python jaotuse. Spyder peab graafiline laua- või X11 ümbersuunamine. Graafilise töölaua otsetee Spyder on saadaval.

Kuna meil on nii Python 2.7 ja 3.5, peate aktiveerima spetsiaalselt soovitud Python versiooni soovite praeguse seansi töötada. Aktiveerimisprotsessi seab tee muutuja Python soovitud versioon.

Käivitage Python 2.7 aktiveerimiseks shell järgmist:

    source /anaconda/bin/activate root

Python 2.7 on arvutisse installitud */anaconda/bin*.

Käivitage Python 3.5 aktiveerimiseks shell järgmist:

    source /anaconda/bin/activate py35


Python 3.5 on arvutisse installitud */anaconda/envs/py35/bin*.

Seansi Python interaktiivsed viidata lihtsalt tippige **python** kest. Kui olete graafilist liidest või on X11 Sea ümbersuunamise häälestamine, võite tippida **spyder** Python IDE käivitada.

### <a name="jupyter-notebook"></a>Jupyter märkmik

Anaconda jaotuse kaasas Jupyter märkmikku, koodi ja analüüsi jagamiseks keskkonnas. Märkmiku Jupyter pääseb juurde JupyterHub. Teil kohaliku Linux kasutajanime ja parooliga sisse logida.

Märkmiku Jupyter server on konfigureeritud eelnevalt Python 2, Python 3 ja R tuumad. Töölaua ikooni nimega "Jupyter märkmiku" käivitada brauseri märkmiku serverit pole. Kui kasutate VM kaudu SSH või X2Go klient, võite ka külastada [https://localhost:8000 /](https://localhost:8000/) Jupyter märkmiku serverit.

>[AZURE.NOTE] Jätkake, kui saate serdi hoiatusi.

Pääsete Jupyter märkmiku server hosti. Tippige *https://\<VM DNS-i nimi või IP-aadress\>: 8000 /*

>[AZURE.NOTE] Pordi 8000 avatud tulemüüri vaikimisi VM on ette valmistatud.

Meil on pakitud valimi märkmike--ühe tekstivormingus Python ja üks R. Näete linki näidised märkmiku avalehel pärast saate autentida Jupyter märkmiku teie kohaliku Linux kasutajanime ja parooli abil. Uue märkmiku loomiseks valige **Uus**ja seejärel vastava keele tuum. Kui te ei näe nuppu **Uus** , klõpsake ikooni **Jupyter** avalehe serveri märkmiku avamiseks vasakus ülanurgas.


### <a name="ides-and-editors"></a>Interaktiivset ja redaktorid

Teil on mitu koodi redaktorid valik. See hõlmab vi/VIM, Emacs, gEdit ja Eclipse. gEdit Eclipse on graafiline redaktorid ja on vaja olema sisse logitud graafiline töölaua neid kasutada. Nende redaktorid on töölaua ja rakenduste otseteed käivitada neid.

**VIM** ja **Emacs** on tekstipõhine redaktorid. Emacs, me installitud lisandmoodul paketi, mis nimega Emacs keeled statistika (ESS) mis lihtsustab töötamise R Emacs editor sees. Lisateavet leiate [ESS](http://ess.r-project.org/).

**Eclipse** on avatud, laiendatav IDE, mis toetab mitut keelt. Java arendajad versioon on installitud VM astme. Lisandmoodulid on saadaval mitut populaarsed keelt, laiendada Eclipse keskkonnas installitud. Meil on lisandmoodul, mis on installitud nimetatakse **Azure'i tööriistakomplekt Eclipse**Eclipse. See võimaldab teil luua, arendamise, katsetamine ja juurutada Azure rakendustes, mis kasutavad keelte nagu Java toetava Eclipse arenduskeskkond. Olemas on ka ka **Azure SDK Java** , mis lubab juurdepääsu erinevad Azure teenused Java keskkonnas. Lisateavet Azure tööriistakomplekt Eclipse leiate [Azure'i tööriistakomplekt Eclipse](../azure-toolkit-for-eclipse.md).

**Lateks** on installitud texlive paketi koos mõne Emacs lisandmooduli [auctex](https://www.gnu.org/software/auctex/manual/auctex/auctex.html) paketi, mis lihtsustab loome lateks dokumentide sees Emacs kaudu.  

### <a name="databases"></a>Andmebaasid

#### <a name="postgres"></a>Postgres
Avatud lähteandmebaasi **Postgres** on saadaval VM, teenuseid ja initdb, mis on juba lõpetatud. Peate endiselt andmebaasid ja kasutajate loomine. Lisateabe saamiseks vaadake [Postgres dokumentatsiooni](https://www.postgresql.org/docs/).  

####  <a name="graphical-sql-client"></a>Graafilise SQL-i kliendile
**SQL-i orav**, graafiline SQL-i klient, on esitatud ühenduse loomiseks muu andmebaasid (nt Microsoft SQL Server, Postgres ja MySQL-i) ja käivitage SQL-päringud. Saate käivitada see on graafiline seansil (abil X2Go klient, näiteks). Autonoomsest orav SQL-i, saate selle käivitada ikooni töölaual või käivitage järgmine käsk kest.

    /usr/local/squirrel-sql-3.7/squirrel-sql.sh

Enne esimest kasutamist, draiverid ja andmebaasi pseudonüümid häälestada. JDBC draiverid asuvad:

*/usr/Share/Java/jdbcdrivers*

Lisateavet leiate teemast [Orav SQL-i](http://squirrel-sql.sourceforge.net/index.php?page=screenshots).

#### <a name="command-line-tools-for-accessing-microsoft-sql-server"></a>Microsoft SQL Server käsurea tööriistad

SQL serveri ODBC-draiveri paketis on kaasas kaks käsurea Tööriistad:

**BCP**: bcp kasuliku hulgi kopeeritakse Microsoft SQL serveri eksemplar ja andmefaili vaheline andmete kasutaja määratud vormingus. Bcp kasuliku saab suure hulga uued read SQL serveri tabelid importida või eksportida andmed välja tabelite andmefailid. Andmete importimine tabeli, peate kasutada tabeli jaoks loodud vormingus faili või mõista struktuuri tabeli ja tüüpi andmeid, mis sobivad selle veerud.

Lisateavet leiate teemast [ühenduse loomine bcp abil](https://msdn.microsoft.com/library/hh568446.aspx).

**sqlcmd**: sisestage Transact-SQL-lausete sqlcmd kasuliku, samuti süsteemi toimingute ja skript failide käsuviibale. Selle kasuliku kasutab ODBC Transact-SQL-i pakettidena käivitada.

Lisateavet leiate teemast [ühenduse loomine sqlcmd abil](https://msdn.microsoft.com/library/hh568447.aspx).

>[AZURE.NOTE] Selle kasuliku mõned erinevused Windows ja Linux platvormide jaoks loodud on. Lisateavet dokumentatsioonist.


#### <a name="database-access-libraries"></a>Andmebaasi Accessi teegid

Teekide on saadaval ja Python Accessi andmebaasid.

- R- **RODBC** paketi või **dplyr** pakett võimaldab teil päringu või SQL-lauseid andmebaasiserveri käivitada.
- Python, pakub **pyodbc** teegi aluseks kiht andmebaasi Accessi ODBC-ga.  

**Postgres**juurdepääsemiseks tehke järgmist.

- R kaudu kasutada **RPostgreSQL**;
- Kasutage kaudu Python: **Psycopg2** teek.


### <a name="azure-tools"></a>Azure'i tööriistad
VM on installitud järgmised Azure'i Tööriistad:

- **Azure'i käsurea liides**: Azure'i CLI võimaldab teil luua ja hallata Azure ressursside kaudu shell käske. Autonoomsest Azure'i tööriistad, tippige lihtsalt **azure spikker**. Lisateabe saamiseks lugege teemat [Azure CLI dokumentatsiooni leht](../virtual-machines-command-line-tools.md).
- **Microsoft Azure'i salvestusruumi Explorer**: Microsoft Azure salvestusruumi Exploreri on graafiline tööriist, mida kasutatakse objektid, mis on talletatud teie Azure storage konto, sirvida ja üles- ja allalaadimine andmed ja sealt Azure plekid. Pääsete salvestusruumi Explorer töölaua otsetee ikoon. Saate selle shell viip: autonoomsest, tippides **StorageExplorer**. Peate olema sisse logitud X2Go klient, või on X11 Sea edasisaatmise häälestamine.
- **Azure'i teekide**: järgmiselt on toodud mõned eelinstallitud teekide.

 - **Python**: The Azure'i seotud teekide Python, mis installitakse on **azure**, **azureml**, **pydocumentdb**ja **pyodbc**. Esimesed kolm teekide abil pääsete Azure storage teenused, Azure'i masina õppimine ja Azure DocumentDB (NoSQL andmebaasi Azure). Neljas teegi pyodbc (koos Microsoft ODBC-draiver SQL serveri korral), võimaldab juurdepääsu SQL Server Azure'i SQL-andmebaasi ja SQL Azure'i andmebaas Python ODBC kasutajaliidese abil. Sisestage **pip loendi** loetletud teekide kuvamiseks. Kindlasti käsu Python 2.7 nii 3,5 keskkonnas.
 - **R**: The Azure'i seotud teekide R installitud on **AzureML** ja **RODBC**.
 - **Java**: Azure'i Java teekide loendi leiate kataloogi **/dsvm/sdk/AzureSDKJava** VM. Võtme teekide on Azure salvestamine ja haldamine API-d, DocumentDB ja JDBC draiverid SQL serveri.  

Pääsete [Azure portaali](https://portal.azure.com) eelinstallitud Firefoxi brauseris. Azure'i portaalis, saate luua, hallata ja jälgida Azure ressursse.

### <a name="azure-machine-learning"></a>Azure'i masina õpetused

Azure'i masina õppimine on täielikult hallatud pilveteenuses, mis võimaldab koostada, juurutada ja ühiskasutusse anda ennustav lahendusi. Azure'i masina õ Studio koostada oma katsete ja mudelid. See pääseb andmete teadus virtual arvutisse veebibrauseri kaudu [Microsoft Azure'i masina õ](https://studio.azureml.net)külastades.

Pärast sisselogimist Azure seadme Õppekeskuse Studio, teil on juurdepääs katsetused lõuend on, kus saate luua loogiline ülesehitus masina Õppekeskuse algoritmide kohta. Lisaks Jupyter märkmiku majutatud Azure seadme õ juurde ja saate sujuvalt töötada arvuti õ Studios katsete. Kiireks õppe mudelid, mis on loodud, ümbriste neid veebiteenuse liidest seade. See võimaldab mistahes keeles autonoomsest prognoose õppe mudelid arvutist. Lisateabe saamiseks vt [õ seadme dokumentatsiooni](https://azure.microsoft.com/documentation/services/machine-learning/).

Saate luua ka oma mudelite R või Python VM, ja seejärel juurutada Azure seadme õppe valmistamisel. Meil on paigaldatud teekide R (**AzureML**) ja Python (**azureml**) selle funktsiooni lubamiseks.

Juurutamise mudelite R ja Python üheks Azure seadme õ kohta leiate teavet teemast [kümme asja, mida saate teha andmete science virtuaalse masina](machine-learning-data-science-vm-do-ten-things.md) (Täpsemalt jaotises "mudelid R või Python abil ja kiireks nende abil Azure seadme õ").

>[AZURE.NOTE] Need juhised on andmete teadus VM Windowsi versiooni jaoks kirjutatud. Kuid teave, klõpsake teenuse juurutamisel mudelite Azure seadme õ on rakendatav Linux VM.

### <a name="machine-learning-tools"></a>Õppekeskuse tööriistad

Mõne seadme Õppekeskuse tööriistad ja algoritmide kohta, mis on varem koostatud ja eelinstallitud kohalikult on VM. Järgmised:

* **CNTK** (Arvutuslik võrgu tööriistakomplekti Microsoft Researchi kaudu): on suure Õppekeskuse tööriistakomplekt.
* **Vowpal Wabbit**: kiire Online'i õ algoritmi.
* **xgboost**: tööriista, mis pakub optimeeritud, võimendatud puu algoritmide kohta.
* **Python**: Anaconda Python on ühendatud arvuti õ algoritmide teekidega nagu Scikit saate teada. Saate installida muude teekide abil soovitud `pip install` käsk.
* **R**: rikkaliku teegi õ funktsioonid on saadaval R. Mõned teegid, mis on eelinstallitud on lm glm, randomForest, rpart. Muud teegid saab installida käivitades:

        install.packages(<lib name>)

Siit leiate loendi esimese kolme masina õ tööriistade kohta leiate lisateavet.

#### <a name="cntk"></a>CNTK
See on avatud allika sügav Õppekeskuse tööriistakomplekt. See on käsurea tööriist (cntk) ja on juba tee.

Tavaline valimi käivitamiseks käivitada Shellis järgmised käsud:

    # Copy samples to your home directory and execute cntk
    cp -r /dsvm/tools/CNTK-2016-02-08-Linux-64bit-CPU-Only/Examples/Other/Simple2d cntkdemo
    cd cntkdemo/Data
    cntk configFile=../Config/Simple.cntk

Mudeli väljund on *~/cntkdemo/Output/Models*.

Lisateavet leiate jaotisest CNTK [GitHub](https://github.com/Microsoft/CNTK)ja [CNTK viki](https://github.com/Microsoft/CNTK/wiki).


#### <a name="vowpal-wabbit"></a>Vowpal Wabbit

Vowpal Wabbit on õppe süsteemi, mis kasutab võrgus, loob rakendus, allreduce, vähendamine, learning2search, aktiivne, näiteks masina ja interaktiivse õpetused.

Väga lihtne näide tööriista käivitamiseks tehke järgmist.

    cp -r /dsvm/tools/VowpalWabbit/demo vwdemo
    cd vwdemo
    vw house_dataset

Selle kausta on, suurem demos. VW kohta leiate lisateavet teemast [GitHub paigutamise](https://github.com/JohnLangford/vowpal_wabbit)ja [Vowpal Wabbit viki](https://github.com/JohnLangford/vowpal_wabbit/wiki).

#### <a name="xgboost"></a>xgboost
See on teegi, mis on loodud ja optimeeritud võimendatud (puu) algoritmide kohta. See teek eesmärk push arvutus piirangud masinad äärmused, mis on vajalik, et suuremahuliste puu suurendada, mis on scalable, jagamise ja täpne.

See on saadaval mõne käsurea kui ka mõnda R teek.

Kasutage seda teeki, r, saate mõne interaktiivse R seansi käivitamine (ainult tippides **R** kest) ja teeki.

Siin on lihtne näide käivitada R viip:

    library(xgboost)

    data(agaricus.train, package='xgboost')
    data(agaricus.test, package='xgboost')
    train <- agaricus.train
    test <- agaricus.test
    bst <- xgboost(data = train$data, label = train$label, max.depth = 2,
                    eta = 1, nthread = 2, nround = 2, objective = "binary:logistic")
    pred <- predict(bst, test$data)

Käsurea xgboost käivitamiseks on siin käivitada Shell käsud:

    cp -r /dsvm/tools/xgboost/demo/binary_classification/ xgboostdemo
    cd xgboostdemo
    xgboost mushroom.conf


Faili .model kirjutatakse määratud kataloogi. Selles näites demo teavet leiate [github](https://github.com/dmlc/xgboost/tree/master/demo/binary_classification).

Xgboost kohta leiate lisateavet teemast [xgboost dokumentatsiooni lehe](https://xgboost.readthedocs.org/en/latest/)ja oma [Github hoidla](https://github.com/dmlc/xgboost).

#### <a name="rattle"></a>Vurama
Vurama ( **R** **A**nalytical **T**ool **T**o **L**Teeni **E**asily) kasutab põhise andmete uurimine ja modelleerimine. Esitab andmete teisendusteta andmeid saab hõlpsasti modelleerida, koostab järelevalveta- ja kontrollitud mudelite andmete, tutvustab mudelite jõudlus graafiliselt, statistika ja visuaalse kokkuvõtted ja uute andmete hinded. See loob ka R koodi imitatsiooniga UI toimingud, mida saab läbiviimisel R või kasutada lähtepunktina täpsemaks analüüsimiseks.

Vurama käivitamiseks peate olema graafilise sisselogimise seansil. Sisestage terminalis, ```R``` R keskkonda. R kuvatakse vastav viip, sisestage järgmine käsk:

    library(rattle)
    rattle()

Nüüd avaneb graafilist liidest menüüde kogumit. Siit leiate juhised Lühijuhend vurama vaja kasutada valimi ilm andmekomplekt ja luua mudel. Mõned alltoodud juhiseid, palutakse automaatselt installimine ja laadimine nõutav R paketid mis ei ole juba süsteem.

>[AZURE.NOTE] Kui teil pole juurdepääsu installida paketi süsteemi kataloogis (vaikimisi), võidakse kuvada viip oma R konsooli aknas pakettide installimiseks oma isiklikku teeki. Vastake *y* , kui näete järgmisi juhiseid.

1. Klõpsake **käivitada**.
2. Dialoogiboksi hüpikteatis teilt, kui soovite kasutada näiteks ilm andmehulgas. Klõpsake nuppu **Jah** laadimiseks näide.
3. Klõpsake vahekaarti **mudel** .
4. Klõpsake nuppu **Käivita** koostamiseks Otsusepuu kuvamine.
5. Klõpsake nuppu **Joonista** kuvamiseks Otsusepuu kuvamine.
6. Klõpsake suvandinuppu **mets** ja klõpsake nuppu **Käivita** koostamiseks juhusliku mets.
7. Klõpsake vahekaarti **hinnata** .
8. Klõpsake raadionuppu **Risk** ja **Execute** kaks Risk (kumulatiivsus) jõudluse krunti kuvamiseks klõpsake nuppu.
9. Klõpsake vahekaarti **Log** Genereeri R koodi eelnev toimingute kuvamiseks.
(Vurama praeguses versioonis vea tõttu peate sisestama on *#* märk ette *ekspordi selle log...* teksti Logi.)
10. Klõpsake nuppu **ekspordi** R skriptifail *weather_script nimega salvestamine. R* home kausta.

Soovi korral võite lahkuda vurama ja R. Nüüd saate muuta loodud R skript või seda kasutada, kui see on käivitada igal ajal korrata kõike Rattle UI jooksul tehtud. Eriti algajatele r, see on lihtne kiiresti teha analüüsi ja masina õ lihtsa graafiliselt, r muutmine ja/või lugege koodi automaatselt loomisel.


## <a name="next-steps"></a>Järgmised sammud
Siin on, kuidas saate jätkata oma õppimine ja uurimine.

* [Andmete teadus kohta Linux andmete teadus virtuaalse masina](machine-learning-data-science-linux-dsvm-walkthrough.md) kiirtutvustus näitab, kuidas mitme levinud andmete teadus tööülesannete täitmiseks koos Linux andmete teadus VM siin ette valmistatud. 
* Andmete teadus VM erinevate Andmeriistad teadus uurimiseks saate proovida selles artiklis kirjeldatud tööriistu. Installitud VM tööriistade kohta lisateabe saamiseks saab käivitada ka shell virtual seadmes lihtne Sissejuhatus ja viitu *dsvm-veel-teave* .  
* Saate teada, kuidas koostada-lõpuni analytical lahenduste korrapäraselt [Meeskonnatöö andmete teadus protsessi](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/)abil.
* Külastage kohapeal õppimine ja andmete analytics näidised, mida kasutada Cortana Analytics komplekti [Cortana Analytics Galerii](http://gallery.cortanaanalytics.com) .
