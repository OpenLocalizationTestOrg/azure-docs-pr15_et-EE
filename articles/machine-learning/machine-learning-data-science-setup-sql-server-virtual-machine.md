<properties
    pageTitle="SQL serveri virtuaalse masina nimega serveriks IPython märkmiku häälestamine | Microsoft Azure'i"
    description="Määrake üles andmete teadus virtuaalse masina SQL serveri ja IPython serveriga."
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
    ms.date="09/19/2016"
    ms.author="xibingao;bradsev" />

# <a name="set-up-an-azure-sql-server-virtual-machine-as-an-ipython-notebook-server-for-advanced-analytics"></a>Häälestamine on Azure SQL serveri virtuaalse masina nimega serveriks IPython märkmiku täiustatud analüüsi jaoks

Selles teemas kirjeldatakse, kuidas ette valmistada ja konfigureerimiseks SQL serveri virtual masina pilvepõhist andmete teadus keskkonna osana kasutada. Windows virtual arvuti on konfigureeritud, mis toetab näiteks IPython märkmik, Azure'i salvestusruumi Explorer ja AzCopy ning muud Utiliidid, mis on abiks andmete teadus projektid. Azure'i salvestusruumi Exploreris ja AzCopy, näiteks pakkuda mugavat võimalust andmete üleslaadimiseks Azure'i bloobimälu oma kohalikust arvutist või laadige see oma arvutisse alla bloobimälu.

Azure virtuaalse masina Galerii sisaldab mitu pilti, mis sisaldavad Microsoft SQL Server. Valige SQL serveri VM pilt, mis sobib teie andmete vajadustele. Soovitatav pildid on:

- SQL Server 2012 SP2 Enterprise väikestele ja keskmise suurusega andmete suuruse
- SQL Server 2012 SP2 ettevõtte jaoks optimeeritud DataWarehousing töökoormus suur väga suurte andmete suurused

 > [AZURE.NOTE] SQL Server 2012 SP2 Enterprise pilt **ei sisalda andmeid kettale**. Peate lisamine ja/või lisada ühe või mitme virtuaalse kõvaketta andmete talletamiseks. Azure'i virtuaalarvuti loomisel on vastendatud C-ketas operatsioonisüsteemi kettal ja ajutine kettal vastendatud D ketas. Ärge kasutage D ketas andmete talletamiseks. Nagu nimi viitab, pakub ainult ajutine salvestusruum. Kuna see ei asu Azure storage pakub pole liigsed või varundamine.


##<a name="Provision"></a>Ühenduse Azure klassikaline portaali ja mõnda SQL serveri virtuaalse masina ettevalmistamine

1.  [Azure'i klassikaline portaali](http://manage.windowsazure.com/) konto kaudu sisse logida.
    Kui teil pole Azure'i konto, külastage [Azure'i tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/).

2.  Azure'i klassikaline portaalis alumises vasakus nurgas veebileht, valige **+ Uus**, klõpsake **ARVUTADA**, klõpsake **VIRTUAALSE masina**ja klõpsake **Saatja Galerii**.

3.  Lehel **Loo virtuaalse masina** valige virtuaalse masina pilt, mis sisaldavad SQL serveri andmetega vajaduste ja seejärel klõpsake järgmise lehe paremas allnurgas asuvat noolt. Kõige ajakohasemat teavet toetatud Azure SQL serveri pildid, teemat [Alustamine SQL Server Azure'i Virtuaalmasinates](http://go.microsoft.com/fwlink/p/?LinkId=294720) [SQL Server Azure'i Virtuaalmasinates](http://go.microsoft.com/fwlink/p/?LinkId=294719) dokumentatsiooni seadmine.

    ![Valige SQL serveri VM][1]

4.  Sisestage lehel esimese **Virtuaalse masina konfiguratsioon** järgmine teave:

    -   Sisestage **VIRTUAALSE masina nimi**.
    -   Tippige **Uue kasutaja nimi** väljale nimi kordumatu kasutajanimi VM kohaliku administraatorikonto.
    -   Tippige **Uus parool** väljale keerukas parool. Lisateabe saamiseks vt [Tugevad paroolid](http://msdn.microsoft.com/library/ms161962.aspx).
    -   Tippige parool uuesti väljale **Parooli kinnitamine** .
    -   Valige sobiv **suurus** rippmenüüst.

     > [AZURE.NOTE] Virtuaalse masina maht on määratud ajal ettevalmistamise: A2 on soovitatav tootmise töökoormus väikseim suurus. Minimaalne soovitatav suurus virtuaalse masina jaoks on A3 kasutamisel SQL Server Enterprise Edition. Valige A3 või uuem versioon, kasutades SQL Server Enterprise Edition. Valige A4, kasutades SQL Server 2012 või 2014 Enterprise optimeeritud selgituseks töökoormus piltide.
Valige A7 kasutamisel SQL Server 2012 või 2014 Enterprise optimeeritud andmed lao töökoormus pilte. Valitud suurusega andmete ketast, saate konfigureerida arv on piiratud. Kõige ajakohasemat teavet saadaval virtuaalse masina suurused ja andmete ketast, mille saate manustada virtuaalse masina arv, lugege teemat [Azure virtuaalse masina suurused](http://msdn.microsoft.com/library/azure/dn197896.aspx). Hindade teavet, lugege teemat [Virtuaalse Macines hinnad](https://azure.microsoft.com/pricing/details/virtual-machines/).

    Klõpsake parempoolses allnurgas jätkata järgmise noolt.

    ![VM konfigureerimine][2]

5.  Konfigureerida **virtuaalse masina konfiguratsiooni** teisel lehel, võrgunduse ja salvestusruumi kättesaadavus ressursid.

    -   Valige dialoogiboksis **Pilveteenuses** **Loo uus pilveteenuses**.
    -   **Pilveteenuse teenuse DNS-i nimi** väljale nimi Sisestage esimene osa teie valitud DNS-i nimi, et see on lõpule jõudnud vormingus **TESTNAME.cloudapp.net** nimi
    -   Valige dialoogiboksis **Piirkond/osaleja rühma/VIRTUAL võrgu** piirkonnas, kus see virtuaalne pilt olema majutatud.
    -   **Salvestusruumi konto**, valige salvestusruumi konto või automaatselt loodud sortimisloendeid.
    -   Valige väljal **Kättesaadavus** **(pole)**.
    -   Lugege ja nõustuge hinnakirjad teave.

6.  **Lõpp-punktid** jaotises tühja rippmenüüst **nime**all nuppu ja valige **MSSQL** seejärel tippige pordinumber andmebaasimootor (vaikimisi eksemplari**1433** ) eksemplari.

7.  Teie SQL serveri VM võib olla IPython märkmiku Server, mis konfigureeritakse toimub hiljem.
    Saate lisada uue lõpp-punkti määramiseks pordi oma serveri IPython märkmiku kasutamiseks. Sisestage veeru **nimi** nimi, valige oma valik avalik pordi ja 9999 privaatne pordi pordi number.

    Klõpsake parempoolses allnurgas jätkata järgmise noolt.

    ![Valige MSSQL ja IPython pordid][3]

8.  **Installige VM agent** vaikesuvand märgitud, ja klõpsake nuppu Aktsepteeri on märge alumises paremas nurgas VM ettevalmistamise protsessi lõpuleviimiseks viisardit.

    `![VM Valikud][4]

9.  Oodake, kuni Azure'i valmistab virtual arvuti. Virtuaalne seadme olekut läbida oodata:

    -   Alates (ettevalmistamise)
    -   Peatatud
    -   Alates (ettevalmistamise)
    -   Töötab (ettevalmistamise)
    -   Töötab


##<a name="RemoteDesktop"></a>Avage virtuaalse masina kaugtöölaua ja häälestamise lõpuleviimiseks abil

1.  Kui ettevalmistamise on lõpule jõudnud, klõpsake nuppu arvuti virtuaalne ARMATUURLAUA lehe nime. Klõpsake lehe allosas nuppu **Ühenda**.

2.  Windowsi kaugtöölaua programmi abil rpd faili avamiseks valige (`%windir%\system32\mstsc.exe`).

3.  Dialoogiboksi **Windowsi turvalisus** pakkuda mõne varasema juhises määratud kohaliku administraatorikonto parooli.
    (Võimalik teil palutakse kinnitamiseks virtuaalse masina identimisteabe.)

4.  Esmakordsel sisselogimisel selle virtuaalse masina, mitme protsessi võib tekkida vajadus lõpule viia, sh oma töölaua, Windowsi värskenduste ja lõpetamist Windows esialgne konfiguratsioon ülesanded (sysprep) häälestamine. Pärast Windowsi sysprep on lõpule jõudnud, SQL Serveri install on lõpule viidud konfigureerimistoimingud. Järgmised toimingud võib põhjustada viivituse paar minutit, samal ajal, kui nad täita. `SELECT @@SERVERNAME`võib tagastada õige nime, kuni SQL Serveri install on lõpule viidud ja SQL Server Management Studio ei pruugi avalehel failid.

Kui olete loonud ühenduse, virtuaalse masina Windows kaugtöölaua, virtuaalse masina töötab palju nagu teise arvutisse. Ühenduse loomine SQL Server Management Studio (töötab virtuaalse masina) SQL serveri vaikimisi eksemplari tavapärasel viisil.


##<a name="InstallIPython"></a>Installige IPython märkmiku ja muud täiendavad tööriistad

Teie uus SQL serveri VM olla serveriks IPython märkmiku ja installima täiendavad täiendavad tööriistad sellise AzCopy, Azure salvestusruumi Explorer, kasulik andmete teadus Python paketid ja teised konfigureerimiseks teisiti kohandamine skripti pakutakse teile. Installimiseks tehke järgmist.

- Paremklõpsake **Windowsi avakuva** ikooni ja klõpsake **käsku Käsuviip (Admin)**
- Järgmised käsud kopeerige ja kleepige Käsuviip.

        set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/MachineSetup/Azure_VM_Setup_Windows.ps1'
        @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

- Küsimise korral sisestage parool luua IPython märkmiku server.
- Skripti kohandamine automatiseerib pärast installi mitu korda, mis sisaldavad:
    + Installimine ja häälestus IPython märkmiku serveri
    + Windowsi tulemüüri jaoks lõpp-punktid varem loodud TCP-pordid avamiseks tehke järgmist.
    + SQL serveri remote connectivity jaoks
    + IPython märkmiku serveri remote connectivity jaoks
    + Valimi IPython märkmike ja SQL-i skriptide tõmbamine
    + Allalaadimine ja installimine kasulik andmete teadus Python paketid
    + Alla laadida ja installida Azure tööriistad, nt AzCopy ja Azure salvestusruumi Exploreris  
<br>
- Võite juurdepääsu ja käivitage IPython märkmiku mis tahes kohaliku või kaugandmebaasiga brauseris URL-i vormi `https://<virtual_machine_DNS_name>:<port>`, kus on pordi IPython avaliku pordi ettevalmistamise virtuaalse masina valitud.
- IPython märkmiku server töötab taustal teenust ja taaskäivitatakse automaatselt virtuaalse masina taaskäivitamisel.

##<a name="Optional"></a>Manusta andmed kettale vastavalt vajadusele

Kui teie VM pilt ei sisalda andmeid ketast, st ketast peale C-ketas (OS ketas) ja D ketas (ajutine ketas), peate lisama ühe või mitme andmete plaati salvestada oma andmed. SQL Server 2012 SP2 ettevõtte jaoks optimeeritud DataWarehousing töökoormus VM pilt tuleb eelnevalt konfigureeritud täiendavad ketast SQL serveri andmeid ja log failide jaoks.

 > [AZURE.NOTE] Ärge kasutage D ketas andmete talletamiseks. Nagu nimi viitab, pakub ainult ajutine salvestusruum. Kuna see ei asu Azure storage pakub pole liigsed või varundamine.

Täiendavate andmete ketast lisamiseks järgige kirjeldatud [manustamine andmete kettapuhastusriista abil Windowsi virtuaalse masina kohta](virtual-machines-windows-classic-attach-disk.md), mis juhatab teid läbi:

1. Manustamise tühja kontrollimiseks virtuaalse masina ette valmistatud varasemates toimingutes
2. Uue ketta virtual kohapeal lähtestamine


##<a name="SSMS"></a>Ühenduse loomine SQL Server Management Studio ja kombineeritud režiimi autentimist kasutava lubamine

SQL serveri andmebaasi mootor ei saa ilma domeenikeskkonna kasutada Windowsi autentimist. Andmebaasimootor ühenduse ühest arvutist teise SQL serveri konfigureerimine kombineeritud režiimi autentimist. Kombineeritud režiimi autentimist kasutava võimaldab SQL serveri autentimine ja Windowsi autentimist. SQL-i autentimisrežiim nõutakse neelata andmed otse oma SQL serveri VM andmebaaside [Azure seadme õ Studio](https://studio.azureml.net) andmete importimine mooduli kasutamise.

1.  Kui loote ühenduse virtuaalse masina kaugtöölaua, kasutage paani Windowsi **Otsing** ja tippige **SQL Server Management Studio** (SMSe). Klõpsake nuppu Käivita SQL Server Management Studio (SSMS). Kui soovite SSMS edaspidiseks kasutamiseks töölaua otsetee lisamiseks.

    ![SSMS käivitamine][5]

    Esimest korda, kui avate Management Studio peate looma kasutajate Management Studio keskkonnas. See võib võtta mõne hetke aega.

2.  Avamisel, esitatakse Management Studio dialoogiboks **Loo ühendus serveriga** . Tippige väljale **Serveri nimi** ühenduse andmebaasimootor Exploreriga objekti virtuaalse masina nimi.
    (Asemel virtuaalse masina nimi saate kasutada ka **(local)** või ühe perioodi **Serveri nimi**. Valige **Windowsi autentimist**ja jätke ** *oma\_VM\_nimi*\\oma\_kohaliku\_administraator** väljale **kasutajanimi** . Klõpsake nuppu **Loo ühendus**.

    ![Serveriga][6]

    <br>

     > [AZURE.TIP] Saate muuta autentimisrežiim SQL serveri abil Windowsi klahv muuta registrit või SQL Server Management Studio abil. Autentimisrežiim abil muuta registrit muutmiseks käivitage **Uus päring** ja käivitada järgmise skripti:

        USE master
        go

        EXEC xp_instance_regwrite N'HKEY_LOCAL_MACHINE', N'Software\Microsoft\MSSQLServer\MSSQLServer', N'LoginMode', REG_DWORD, 2
        go


    SQL Server management Studio abil autentimisrežiim muutmiseks tehke järgmist.

3.  **SQL Server Management Studio objekti Explorer**, paremklõpsake SQL serveri (virtuaalse masina nimi) eksemplari nimi ja seejärel klõpsake käsku **Atribuudid**.

    ![Dokumendiserveri atribuudid][7]

4.  Lehel **Turvalisus** jaotises **autentimine**, valige **SQL Server ja Windowsi autentimisrežiimis**ja seejärel klõpsake nuppu **OK**.

    ![Valige autentimisrežiim][8]

5.  Klõpsake dialoogiboksis **SQL Server Management Studio** **OK** tunnistame nõue SQL serveri taaskäivitama.

6.  **Objekti Explorer**, paremklõpsake oma serveri, ja klõpsake **uuesti**. (Kui töötab SQL Server Agent, seda ka taaskäivitada.)

    ![Taaskäivitage][9]

7.  Klõpsake dialoogiboksis **SQL Server Management Studio** **Jah** nõus, kelle soovite SQL Server taaskäivitada.

##<a name="Logins"></a>SQL serveri autentimine sisselogimise loomine

Ühenduse andmebaasimootor mõnest teisest arvutist, peate looma vähemalt üks SQL serveri autentimine Logi sisse.  

Saate luua uue SQL serveri sisselogimise programmiliselt või SQL Server Management Studio abil. Luua uus kasutaja süsteemiadministraator SQL-i autentimise programmiliselt, käivitage **Uus päring** ja käivitada järgmine skript. Asendage < uus kasutaja nimi\> ja < uus parool\> oma valiku *kasutajanimi* ja *parool*. 


    USE master
    go

    CREATE LOGIN <new user name> WITH PASSWORD = N'<new password>',
        CHECK_POLICY = OFF,
        CHECK_EXPIRATION = OFF;

    EXEC sp_addsrvrolemember @loginame = N'<new user name>', @rolename = N'sysadmin';


Reguleerige parooli poliitika (selle proovi kood lülitab välja poliitika kontrollimine ja parooli aegumise). SQL serveri sisselogimise kohta leiate lisateavet teemast [loomine sisselogimist](http://msdn.microsoft.com/library/aa337562.aspx).  

Uue SQL serveri sisselogimise SQL Server Management Studio abil loomiseks tehke järgmist.

1.  **SQL Server Management Studio objekti Explorer**, laiendage kaust, kus soovite luua uue login serveri eksemplari.

2.  Paremklõpsake kausta **Turvalisus** , osutage käsule **Uus**ja valige **Logi sisse …**.

    ![Uue sisselogimine][10]

3.  Sisestage dialoogiboksi **Login - uue** lehel **Üldine** uus kasutaja **sisselogimisnimi** väljale nimi.

4.  Valige **SQL serveri autentimine**.

5.  Sisestage väljale **parool** uus kasutaja parooli. Sisestage selle parool uuesti väljale **Parooli kinnitus** .

6.  Poliitika paroolisuvandeid keerukuse ja jõustamiseks märkige **Jõusta paroolipoliitika** (soovitatav). See on vaikimisi, kui SQL serveri autentimine on valitud.

7.  Jõusta parooli aegumise poliitika suvandid, valige **Jõusta parooli aegumise** (soovitatav). Jõusta paroolipoliitika peab olema märgitud ruut lubamiseks. See on vaikimisi, kui SQL serveri autentimine on valitud.

8.  Jõusta kasutajal luua uue parooli pärast esimest sisselogimist login kasutatakse, valige **kasutaja tuleb muuta parooli järgmise login** (soovitatav juhul, kui see sisselogimine on keegi teine kasutada. Kui kasutajanimi on teie kasutamiseks, valige see suvand.) Jõusta parooli aegumise peab olema märgitud ruut lubamiseks. See on vaikimisi, kui SQL serveri autentimine on valitud.

9.  Valige loendist **vaikimisi andmebaasi** vaikimisi andmebaasi jaoks login. **juhtslaidi** on vaikimisi on see suvand. Kui te pole veel kasutaja andmebaasi loonud, jätke selleks väärtuseks **juhtslaidi**.

10. Loendis **Vaikekeele** jätke **vaikimisi** väärtusena.

    ![Sisselogimise atribuudid][11]

11. Kui see on pärast esimest sisselogimist loote, võite määrata see sisselogimine SQL serveri administraator. Sel juhul **Serverirollide** , kontrollige lehe **süsteemiadministraator**.

    > [AZURE.IMPORTANT] Süsteemiadministraator fikseeritud serveri rolli liikmetel andmebaasimootor üle täielik kontroll. Turvalisuse huvides peaks hoolikalt piirata selle rolli kuuluvus.

    ![süsteemiadministraator][12]

12. Klõpsake nuppu OK.

##<a name="DNS"></a>DNS-i virtuaalse masina nimi

SQL serveri andmebaasi mootor ühendamiseks ühest arvutist teise peate teadma virtuaalse masina nimi süsteemi (DNS).

(See on Interneti-ühendus kasutab tuvastamiseks virtuaalse masina nimi. Saate kasutada IP-aadressi, kuid IP-aadress võib muutuda, kui Azure'i liigub ressursid liigsed või hooldustööd. DNS-i nimi on ühed kuna see saate suunata uue IP-aadressi.)

1.  Azure'i klassikaline portaalis (või eelmises juhises), valige **VIRTUAALMASINATES**.

2.  Lehel **VIRTUAALSE masina eksemplarid** veerus **DNS-i nimi** otsimine ja kopeerige virtuaalse masina, mis kuvatakse eelneb **http://**DNS-i nimi. (Kasutajaliidese ei kuvata kogu nime, kuid saate paremklõpsake seda ja valige Kopeeri.)

##<a name="cde"></a>Ühenduse andmebaasimootor ühest arvutist teise

1.  Avage arvutis Interneti-ühendus, SQL Server Management Studio.

2.  Sisestage dialoogiboksis **ühenduse Server** või **ühenduse andmebaasimootor** väljal **Serveri nimi** , virtuaalse masina (määratud eelmise toimingu) ja avaliku lõpp-punkti pordi number vormingus *dnsnameWindows portnumber* , nt **tutorialtestVM.cloudapp.net,57500**DNS-i nimi.

3.  Valige väljal **autentimist** **SQL serveri autentimine**.

4.  Tippige väljale **Login** Logi sisse, et olete loonud mõne varasema ülesande nime.

5.  Tippige väljale **parool** parool mõne varasema tööülesande loodavad login.

6.  Klõpsake nuppu **Loo ühendus**.

##<a name="amlconnect"></a>Ühenduse loomiseks andmebaasimootor Azure seadme õpetused

Andmete meeskonna teadus protsessi hilisemate, kuvatakse [Azure seadme Õppekeskuse Studio](https://studio.azureml.net) abil luua ja juurutada masina õppe mudelid. Neelata andmeid oma andmebaasi SQL serveri VM otse Azure seadme õ koolitus või hinded, kasutage **Andmete importimine** moodulit uue [Azure seadme õ Studio](https://studio.azureml.net) katse. Selles teemas käsitletakse rohkem üksikasju meeskonnatöö andmete teadus protsessi juhend linkide kaudu. Vaadake lühitutvustust, [mis on Azure seadme õ Studio?](machine-learning-what-is-ml-studio.md).

2.  [Andmete importimine mooduli](https://msdn.microsoft.com/library/azure/dn905997.aspx)paanil **Atribuudid** , valige ripploendist **Andmeallika** **Azure'i SQL-andmebaasi** .

3.  Sisestage väljale **Andmebaasiserveri nimi**`tcp:<DNS name of your virtual machine>,1433`

4.  Sisestage kasutajanimi SQL **serveri kasutajakonto nimi** tekstiväljale.

5.  Sisestage väljale **serveri kasutajakonto parooli** tekst SQL-i kasutaja parooli.

    ![Azure'i ML impordi andmed][13]

##<a name="shutdown"></a>Sulgemine ja deallocate virtuaalse masina kui ei ole kasutusel

Azure'i Virtuaalmasinates võetakse **maksma ainult, mida te kasutate**. Selleks, et teil on pole mille eest tuleb tasuda kasutamisel ei virtual arvuti, peab olema **(Deallocated) peatatud** olekus.

> [AZURE.NOTE] Sulgemise virtuaalse masina kaudu sees (Windowsi power suvandite abil), VM on peatatud, kuid jääb eraldatud. Te olete tasulised tagamiseks alati peatada virtuaalmasinates [Azure klassikaline portaali](http://manage.windowsazure.com/). Soovi korral saate VM PowerShelli kaudu peatada, helistades ShutdownRoleOperation koos "PostShutdownAction" võrdub "StoppedDeallocated".

Sulgeda ja deallocate virtuaalse masina:

1. [Azure'i klassikaline portaali](http://manage.windowsazure.com/) konto kaudu sisse logida.  

2. Valige vasakul asuvalt navigeerimisribalt **VIRTUAALMASINATES** .

3. Virtuaalmasinates loendis klõpsake virtual arvuti nimi, siis avage **ARMATUURLAUA** leht.

4. Klõpsake lehe allosas nuppu **SULGUMIST**.

![VM sulgemine][15]

Virtuaalse masina deallocated kuid neid ei kustutata. Virtuaalne arvuti uuesti käivitamist võib igal ajal Azure'i klassikaline portaalist.

## <a name="your-azure-sql-server-vm-is-ready-to-use-whats-next"></a>Oma Azure SQL serveri VM on kasutamiseks valmis: mis saab edasi?

Virtuaalne arvuti on nüüd valmis kasutama oma andmete teadus harjutused. Virtuaalse masina on ka Azure seadme õppimine ja meeskonnatöö andmete teadus protsess (TDSP) uurimise ja andmete ja muude toimingute koos serveriks IPython märkmiku kasutamiseks valmis.

Järgmiste juhiste juurde andmete teadus protsessi on vastendatud [Meeskonnatöö andmete teadus protsess](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) ja võivad sisaldada toiminguid, mis andmete viimine Hdinsightiga, töötlemine ja kuulake seal õ andmete Azure seadme õppe ettevalmistamiseks.


[1]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/selectsqlvmimg.png
[2]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/4vm-config.png
[3]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/sqlvmports.png
[4]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/vmpostopts.png
[5]:./media/machine-learning-data-science-setup-sql-server-virtual-machine/searchssms.png
[6]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/19connect-to-server.png
[7]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/20server-properties.png
[8]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/21mixed-mode.png
[9]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/22restart2.png
[10]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/23new-login.png
[11]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/24test-login.png
[12]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/25sysadmin.png
[13]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/amlreader.png
[15]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/vmshutdown.png
 
