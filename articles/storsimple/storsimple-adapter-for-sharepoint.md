<properties 
   pageTitle="SharePointi StorSimple adapterit | Microsoft Azure'i"
   description="Kirjeldab, kuidas installida ja konfigureerida või eemaldada StorSimple adapterit SharePointi serveripargi SharePointi."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="07/11/2016"
   ms.author="v-sharos" />

# <a name="install-and-configure-the-storsimple-adapter-for-sharepoint"></a>Installige ja konfigureerige StorSimple SharePointi

## <a name="overview"></a>Ülevaade

SharePointi StorSimple adapterit on osa, mis võimaldab Microsoft Azure'i StorSimple paindlik salvestusruumi ja SharePoint Serveri parkides andmekaitse. Saate selle adapterit SQL serveri sisuandmebaaside kahendarvu suure objekti (BLOOBIMÄLU) sisu teisaldamiseks mäluseadmesse Microsoft Azure'i StorSimple hübriid pilve.

SharePointi StorSimple adapterit toimib Kaug-BLOOBIMÄLU salvestusruumi (RBS) pakkuja ja kasutab funktsiooni SQL serveri kaug-bloobimälu struktureerimata andmed SharePointi sisu talletamiseks (plekid kujul) faili serveris, mis toetavad StorSimple seadme.

>[AZURE.NOTE] SharePointi StorSimple adapterit toetab SharePoint Server 2010 Remote BLOOBIMÄLU salvestusruumi (RBS). See ei toeta SharePoint Server 2010 väline BLOOBIMÄLU salvestusruumi (EBS).

- StorSimple adapterit SharePointi, minge [SharePointi StorSimple adapterit] [ 1] Microsoft Download Center.

- Minge RBS ja RBS piirangud kavandamise kohta leiate teavet [võimaliku segmentimise otsus kasutamine rakenduses SharePoint 2013 RBS] [ 2] või [kavandamine (SharePoint Server 2010) RBS][3].

See ülevaade ülejäänud kirjeldab lühidalt SharePoint ja SharePointi ja tulemuslikkuse piirangud, mis peaks olema teadlik enne installimist ja konfigureerige StorSimple adapterit roll. Kui vaatate seda teavet, minge [SharePointi installi StorSimple adapterit](#storsimple-adapter-for-sharepoint-installation) selle adapterit häälestamise alustamiseks.

### <a name="storsimple-adapter-for-sharepoint-benefits"></a>StorSimple adapterit SharePointi eelised

SharePointi saidi sisu on salvestatud BLOOBIMÄLU struktureerimata andmeid ühe või mitme sisuandmebaaside. Vaikimisi on majutatud andmebaasidele arvutites, kus töötab SQL serveri ja SharePoint Serveri serveripargi asuvad. Plekid suurendamiseks saate kiiresti tarbimine suure hulga kohapealse salvestusruumi suurus. Seetõttu võiksite teise, odavamate salvestusruumi lahenduse leidmiseks. SQL serveri pakub tehnoloogia Remote bloobimälu salvestusruumi (RBS) talletamist võimaldava BLOOBIMÄLU sisu failisüsteemi väljaspool SQL serveri andmebaasi. RBS, plekid võivad asuda failisüsteemi arvutis, kus töötab SQL Server või neid saab talletada failisüsteemi serveri mõnes muus arvutis.

RBS jaoks on vaja kasutada RBS pakkuja juures StorSimple adapterit SharePointi, nt SharePointis RBS lubamine. SharePointi StorSimple adapterit töötab RBS, lastes teil teisaldada plekid server Microsoft Azure'i StorSimple süsteem varundada. Microsoft Azure'i StorSimple siis salvestab andmed BLOOBIMÄLU kohalikult või pilves, kasutuse põhjal. Plekid, mis on väga aktiivne (nimetatakse tavaliselt kiht 1 või kuum andmete) asuvad kohalikult. Vähem aktiivne andmed ja arhiivimine andmed asuvad pilveteenuses. Pärast RBS lubamine sisuandmebaasi SharePointis loonud uue BLOOBIMÄLU sisu talletatakse StorSimple seadme ja sisuandmebaasi.

Microsoft Azure'i StorSimple rakendamise RBS pakub järgmised eelised:

- Jooksva BLOOBIMÄLU sisu eraldi serveris, vähendab päringu SQL Server, mida saate parandada SQL serveri tundlikkuse koormus. 

- Azure'i StorSimple kasutab korduste eemaldamise ja detailsuse andmete mahu vähendamiseks.

- Azure'i StorSimple pakub andmekaitse vormi kohalike ja pilveteenuse pilte. Lisaks, kui te andmebaasi ise StorSimple seadmes, saate varundada sisuandmebaasi ja plekid koos crash kord ühtemoodi. (Teisaldada sisuandmebaasi seade on toetatud ainult StorSimple 8000 sarja seade. Seda funktsiooni ei toetata sarja 5000 või 7000.)

- Azure'i StorSimple sisaldab katastroofi taastamise funktsioone, sh Tõrkesiirde, (sh testi taastamine) faili- ja helitugevuse taastamine ja andmete kiire taastamise.

- Saate andmete taastamine tarkvara, nagu Kroll Ontrack PowerControls, StorSimple hetktõmmiste BLOOBIMÄLU andmeid taastada, Üksusetaseme SharePointi sisu. (See andmete taastamine tarkvara on eraldi osta).

- StorSimple adapterit SharePointi ühendatakse SharePointi administreerimiskeskuse portaalis, mis võimaldab teil oma kogu SharePointi lahendus hallata ühes keskses kohas.

Jooksev BLOOBIMÄLU sisu failisüsteemi saate sisestada muude kokkuhoiu ja eelised. Näiteks RBS abil saate vähendamiseks kallis taseme 1 salvestusruumi ja, kuna see ühel vaja sisuandmebaasi, RBS saate SharePointi serveripargi nõutav andmebaaside arvu vähendamiseks. Aga muud tegurid, näiteks andmebaasi mahupiirangud ja summa-RBS sisu, võib mõjutada ka mäluruumi. Kulude ja RBS kasutamise eeliste kohta leiate lisateavet teemast [kavandamine (SharePoint Foundation 2010) RBS] [ 4] ja [võimaliku segmentimise otsus kasutamine rakenduses SharePoint 2013 RBS][5].

### <a name="capacity-and-performance-limits"></a>Ametinimetus ja jõudluse piirangud

Enne kui arvate, et teie SharePointi lahendus RBS kasutamine, peaks olema teadlik testita jõudlus ja võimsus piirangud SharePoint Server 2010 ja SharePoint Server 2013 ja kuidas need piirangud on seotud aktsepteeritav jõudlus. Lisateavet leiate teemast [tarkvara piirmäärad ja limiidid SharePoint 2013](https://technet.microsoft.com/library/cc262787.aspx).

Enne kui saate konfigureerida RBS, vaadake järgmist:

- Veenduge, et ei ületa suuruse sisu (sisuandmebaasi suurus) ja mis tahes seotud externalized plekid suurust ei toeta SharePointi RBS suuruspiirangut. See on 200 GB. 

    **Sisuandmebaasi mõõt ja BLOOBIMÄLU suurus**

     1. Käivitage see päring keskse administreerimiskeskuse WFE. Käivitage SharePoint halduskesta ja sisestage järgmine Windows PowerShelli käsk saada sisu andmebaaside mahu:

        `Get-SPContentDatabase | Select-Object -ExpandProperty DiskSizeRequired`

         Selles etapis tuleb saab kettal sisu andmebaasi mahtu.

     2. Käivitage järgmine SQL-päringud üks SQL Management Studio SQL server ruut iga sisu andmebaasi ja lisada tulemi arv, mis on saadud etappi 1.

        SharePoint 2013 sisuandmebaaside, sisestage.

        `SELECT SUM([Size]) FROM [ContentDatabaseName].[dbo].[DocStreams] WHERE [Content] IS NULL`

        Sisuandmebaaside SharePoint 2010, sisestage.

        `SELECT SUM([Size]) FROM [ContentDatabaseName].[dbo].[AllDocs] WHERE [Content] IS NULL`

        Selles etapis tuleb saab plekid, mis on antud externalized suurust.

- Soovitame salvestada kõik BLOOBIMÄLU ja andmebaasi sisu kohalikult StorSimple seadme. StorSimple seade on kaks sõlme kobar jaoks kõrge kättesaadavus. Sisuandmebaaside ja plekid pannes StorSimple seadme pakub kõrge kättesaadavus.

    StorSimple seadme sisuandmebaasi liikumiseks kasutage traditsiooniline SQL serveri migreerimise põhitõed. Andmebaasi alles siis, kui kõik BLOOBIMÄLU sisu andmebaas on teisaldatud faili ühiskasutusse andmine kaudu RBS liikumine Kui valite sisuandmebaasi liikumiseks StorSimple seade, soovitame konfigureerida sisuandmebaasi salvestusruumi seadme nimega esmane helitugevust.

- Microsoft Azure'i StorSimple, kui kasutades astmeline maht, ei ole nii, et tagada selle sisu, mis on talletatud kohalik StorSimple, seade on sõltuvad pole Microsoft Azure'i pilve. Seega soovitame abil StorSimple kohalikult kinnitatud mahud koos SharePointi RBS. See tagab, et kõik BLOOBIMÄLU sisu jääb kohalikult StorSimple seadme ja Microsoft Azure ei teisaldata.

- Kui salvestate ei StorSimple seadme sisu andmebaase, kasutage traditsiooniline SQL serveri kõrge-saadavus head tavad RBS toetavad. SQL serveri rühmitamise toetab RBS, peegelduse ajal SQL Server ei ole. 

>[AZURE.WARNING] Kui teil on lubatud RBS, me ei soovita teisaldada sisuandmebaasi StorSimple seade. See on testimata konfiguratsioon.
 
## <a name="storsimple-adapter-for-sharepoint-installation"></a>SharePointi installi StorSimple adapterit

Enne installimist SharePointi StorSimple adapterit, peate konfigureerima StorSimple ja veenduge, et SharePointi serveripargi ja SQL Serveri eksemplari loomise vastavad kõik eeltingimused. Selles õppetükis kirjeldatakse konfiguratsiooni nõuded, samuti installimine ja täiendamine StorSimple adapterit SharePointi toiminguid. 

## <a name="configure-prerequisites"></a>Eeltingimused konfigureerimine

Enne kui saate installida SharePointi StorSimple adapterit, veenduge, et StorSimple seadme, SharePointi serveripargi ja SQL Serveri eksemplari loomise vastama järgmistele tingimustele.

### <a name="system-requirements"></a>Süsteeminõuded

SharePointi StorSimple adapterit töötab järgmist riist- ja tarkvara:

- Toetatud operatsioonisüsteem – Windows Server 2008 R2 SP1, Windows Server 2012 või Windows Server 2012 R2 

- Toetatud SharePointi versioonid – SharePoint Server 2010 või SharePoint Server 2013

- Toetatud SQL serveri versiooni – Enterprise'i SQL Server 2008, SQL Server 2008 R2 Enterprise Edition või SQL Server 2012 Enterprise Edition

- Toetatud seadmed StorSimple – StorSimple 8000 sarja, StorSimple 7000 sarja või StorSimple 5000 sarja.

### <a name="storsimple-device-configuration-prerequisites"></a>StorSimple seadme konfiguratsiooni eeltingimused

StorSimple seade on blokeerimine seade ja sellisena nõuab failiserverisse, kus saate andmeid majutatud. Soovitame kasutada eraldi server, mitte olemasoleva serveri SharePointi serveripargi kaudu. See fail server peab olema sama kohtvõrgu (LAN) kui SQL serveri arvuti, mis majutab sisu andmebaase. 

>[AZURE.TIP] 
>
>- Kui teie SharePointi serveripargis kõrge saadavalolekut konfigureerimiseks peaks faili server kõrge-saadavus ka juurutamine.
>
>- Kui salvestate pole seadmes StorSimple sisuandmebaasi, kasutage traditsiooniline kõrge-saadavus põhitõed, mis toetavad RBS. SQL serveri rühmitamise toetab RBS, peegelduse ajal SQL Server ei ole. 

Veenduge, et seadme StorSimple on õigesti konfigureeritud ning et üldine toetama teie SharePointi juurutusega on konfigureeritud ja SQL serveri arvuti kaudu juurdepääsetavad. Kui teil on veel juurutada ja konfigureerida StorSimple seadme, minge [Deploy kohapealse StorSimple seadme](storsimple-deployment-walkthrough.md) . Pange tähele, IP-aadressi StorSimple seadme; peate selle käigus StorSimple adapterit SharePointi installi. 

Lisaks veenduge, et helitugevuse kasutatakse BLOOBIMÄLU hajutamise vastab järgmistele nõuetele.

- Maht peab olema vormindatud 64 KB jaotuse ühiku maht.

- Teie web esiosa (WFE) ja rakenduste serverid peab olema helitugevuse üldise nimetamistava (UNC) tee kaudu juurde pääseda. 

- SharePointi serveripargi tuleb konfigureerida kirjutamise maht.

>[AZURE.NOTE] Pärast installimist ja konfigureerige, kõik BLOOBIMÄLU hajutamise tuleb läbida StorSimple seadme (seade on esitada mahud SQL serveri ja storage astme haldamine). Te ei saa kasutada mis tahes muud sihtkohtade BLOOBIMÄLU hajutamise.
 
Kui kavatsete kasutada StorSimple hetktõmmise halduri BLOOBIMÄLU ja andmebaasi andmete hetktõmmiste tegemiseks, installige kindlasti StorSimple hetktõmmise halduri andmebaasiserveri nii, et seda saab kasutada SQL-i kirjutaja teenuse kasutusele Windows helitugevuse vari Copy Service (VSS). 

>[AZURE.IMPORTANT] StorSimple hetktõmmise haldur ei toeta SharePointi VSS-kirjuti ja ei saa teha rakenduse ühtsete SharePointi andmete hetktõmmiste. SharePointi stsenaariumi StorSimple hetktõmmise Manager pakub ainult krahh ühtsete varukoopiad. 
 
## <a name="sharepoint-farm-configuration-prerequisites"></a>SharePointi serveripargi konfiguratsiooni eeltingimused

Veenduge, et teie SharePointi serveripargis on õigesti konfigureeritud, järgmiselt:

- Veenduge, et teie SharePointi serveripargis oleks terve olekus ja kontrollige järgmist. 

- Kõik SharePointi WFE ja rakenduste serverid serveripargis registreeritud töötab ja saate pingis, mille installite StorSimple adapterit SharePointi serverist.

- Iga WFE server ja rakenduse server töötab SharePointi ajastiteenuse (SPTimerV3 või SPTimerV4).

- IIS-rakenduste komplekti, millise SharePointi administreerimiskeskuse saidi töötab nii SharePointi ajastiteenuse on administraatoriõigused. 

- Veenduge, et Internet Explorer täiustatud turvalisus kontekstis (IE paoklahvi (ESC)) on keelatud. Järgmiste juhiste keelamiseks IE paoklahvi (ESC).

    1. Sulgege kõik Internet Exploreri aknad.

    2. Käivitage Server Manager.

    3. Klõpsake vasakul paanil **Kohaliku serveri**.

    4. Klõpsake parempoolsel paanil kõrval **IE täiustatud turvalisus konfiguratsiooni** **kohta**.

    5. Klõpsake jaotises **Administraatorid**, klõpsake **välja**.

    6. Klõpsake nuppu **OK**.

## <a name="remote-blob-storage-rbs-prerequisites"></a>Remote BLOOBIMÄLU salvestusruumi (RBS) eeltingimused

Veenduge, et kasutate toetatud SQL serveri versiooni. Ainult järgmised versioonid on toetatud ja RBS kasutada.

- SQL Server 2008 Enterprise Edition

- SQL Server 2008 R2 Enterprise Edition

- SQL Server 2012 Enterprise Edition

Plekid saab externalized ainult draividel, mis StorSimple seadme esitab SQL serveriga. Muud eesmärgid BLOOBIMÄLU hajutamise jaoks on toetatud.

Kui olete kõik eelnev konfigureerimine juhiseid lõpule viinud, minge [SharePointi StorSimple adapterit installida](#install-the-storsimple-adapter-for-sharepoint).

## <a name="install-the-storsimple-adapter-for-sharepoint"></a>Installige SharePoint StorSimple adapterit

Kasutage StorSimple adapterit rakendusele SharePoint installimiseks toimige järgmiselt. Kui teil on tarkvara uuesti installida, lugege teemat [täiendamine või uuesti StorSimple adapterit SharePointi](#upgrade-or-reinstall-the-storsimple-adapter-for-sharepoint). Installimiseks on vaja aeg sõltub teie SharePointi serveripargis SharePointi andmebaasidega koguarv.

[AZURE.INCLUDE [storsimple-install-sharepoint-adapter](../../includes/storsimple-install-sharepoint-adapter.md)]


## <a name="configure-rbs"></a>RBS konfigureerimine

Pärast installimist SharePointi StorSimple adapterit, konfigureerida RBS järgnevalt kirjeldatud.

>[AZURE.TIP] SharePointi StorSimple adapterit ühendatakse SharePointi administreerimiskeskuse lehel lubamisel RBS lubatud või keelatud iga sisuandmebaasi SharePointi serveripargis. Siiski lubamine või keelamine RBS sisuandmebaasi põhjustab IIS-i lähtestamine, mis teie serveripargi konfiguratsioonist saate hetkeks häirida SharePointi web esiosa (WFE). (Tegureid, näiteks eesprotsessor laadi koormusetasakaalustusteenuse, praegune serveri töökoormus ja jne, saab piirata või selle katkemist kõrvaldamiseks.) Kasutajate kaitsmiseks häireid soovitame, et saate lubada või keelata RBS ainult ajal aken kavandatud hooldustööd.

[AZURE.INCLUDE [storsimple-sharepoint-adapter-configure-rbs](../../includes/storsimple-sharepoint-adapter-configure-rbs.md)]
 

## <a name="configure-garbage-collection"></a>Prügikoristus konfigureerimine

Kui kustutatakse SharePointi saidi kaudu, nad ei automaatselt kustutatakse RBS poe maht. Selle asemel on asünkroonne, tausta hooldustööd programmi kustutab orbsisutüübi plekid faili salvestada. Süsteemi administraatorid saavad ajastada selle protsessi käivitamiseks perioodiliselt või nad saaksid alustada selle vajaduse korral.

See programm (Microsoft.Data.SqlRemoteBlobs.Maintainer.exe) installitakse automaatselt SharePointi WFE serverid ja rakenduste serverid RBS lubamisel. Programm on installitud järgmises kohas: *buutimine ketas*: \Programmifailid\Microsoft SQL serveri bloobimälu 10.50\Maintainer\

Teavet konfigureerimise kohta ja hooldus programmiga, vt [Säilitada RBS rakenduses SharePoint Server 2013][8].

>[AZURE.IMPORTANT] RBS hooldaja programm on ressursimahukas. Teil peaks plaanida selle SharePointi serveripargi ainult hele tegevuse ajal käivitada.

### <a name="delete-orphaned-blobs-immediately"></a>Kustutage orbsisutüübi plekid kohe

Kui teil on vaja kustutada orbsisutüübi plekid kohe, saate kasutada järgmisi juhiseid. Võtke arvesse, et need juhised on näide sellest, kuidas seda saab teha SharePoint 2013 keskkonnas, kus järgmised komponendid.

- Sisuandmebaasi nimi on WSS_Content.
- SQL serveri nimi on SHRPT13-SQL12\SHRPT13.
- Web rakenduse nimi on SharePoint – 80.

[AZURE.INCLUDE [storsimple-sharepoint-adapter-garbage-collection](../../includes/storsimple-sharepoint-adapter-garbage-collection.md)]


## <a name="upgrade-or-reinstall-the-storsimple-adapter-for-sharepoint"></a>Versiooni täiendamine või uuesti StorSimple adapterit SharePointi

Järgmise toiminguga versioonile SharePoint server ja siis uuesti StorSimple adapterit SharePointi ümber lihtsalt täiendamine või uuesti adapterit olemasoleva SharePointi serveripargi sisse. 

>[AZURE.IMPORTANT] Enne kui proovite uuendada oma SharePointi tarkvara ja/või või uuesti StorSimple adapterit SharePointi, vaadake järgmist teavet:
>
>- Kõik failid, mis varem teisaldati välise salvestusruumi RBS kaudu ei ole saadaval, kuni uuesti on lõpule jõudnud ja RBS funktsioon on lubatud uuesti. Mis tahes täiendamine või uuesti aken kavandatud hooldustööde ajal teha mõju kasutajale piiramiseks.
>
>- Versioonitäienduse/uuesti aega, saate erinevad sõltuvalt SharePointi andmebaaside SharePointi serveripargi koguarv.
>
>- Kui versioonitäiendus ja uuesti installimine on lõpule jõudnud, peate selle sisuandmebaaside RBS lubamine. Lisateabe saamiseks vaadake [RBS konfigureerimine](#configure-rbs) .
>
>- Kui olete konfigureerinud RBS SharePointi serveripargi, mis on väga suure hulga andmebaasid (üle 200), võib **SharePointi administreerimiskeskuse** lehel ajalõpuni. Kui see toimub, värskendage lehte. See ei mõjuta konfiguratsiooni protsess.

[AZURE.INCLUDE [storsimple-upgrade-sharepoint-adapter](../../includes/storsimple-upgrade-sharepoint-adapter.md)]
 
## <a name="storsimple-adapter-for-sharepoint-removal"></a>StorSimple adapterit SharePointi eemaldamine

Järgmised toimingud kirjeldavad plekid tagasi liikumiseks SQL serveri sisuandmebaaside ja seejärel desinstallima StorSimple adapterit SharePointi. 

>[AZURE.IMPORTANT] Teil on teisaldamiseks plekid tagasi sisu andmebaaside enne desinstallimist adapterit tarkvara. 

### <a name="before-you-begin"></a>Enne alustamist 

Enne tagasi andmete teisaldamine SQL serveri sisuandmebaaside ja alustada adapterit eemaldamise protsessi, koguda järgmist teavet:

- Kõik andmebaasid, mille jaoks on lubatud RBS nimed
- Üldise Nimetamistava teed konfigureeritud BLOOBIMÄLU pood.

### <a name="move-the-blobs-back-to-the-content-databases"></a>Liikumine tagasi plekid sisu andmebaasid

Enne desinstallimist StorSimple adapterit SharePointi tarkvara, tuleb kõik plekid, mis olid externalized naasmiseks SQL serveri sisuandmebaaside migreerida. Kui proovite desinstallida StorSimple adapterit SharePointi enne, kui teisaldate kõik plekid uuesti sisu andmebaasidele, kuvatakse järgmine hoiatusteade.

![Hoiatusteade](./media/storsimple-adapter-for-sharepoint/sasp1.png)
 
####<a name="to-move-the-blobs-back-to-the-content-databases"></a>Plekid tagasi liikumiseks sisu andmebaasid 

1. Laadige externalized objekte.

2. **SharePointi administreerimiskeskuse** lehe avamiseks ja liikuge sirvides **Süsteemisätetest**. 

3. Klõpsake jaotises **Azure StorSimple**, **StorSimple adapterit konfigureerimine**.

4. Lehel **Konfigureerimine StorSimple adapterit** nuppu **Keela** iga sisu andmebaase, mida soovite eemaldada välise bloobimälu all. 

5. Objektide kustutamine SharePoint ja seejärel laadida neid uuesti.

Teise võimalusena saate kasutada Microsoft` RBS Migrate()` PowerShelli cmdleti SharePointiga kaasas. Lisateavet leiate teemast [migreerimine sisu või sealt välja RBS](https://technet.microsoft.com/library/ff628255.aspx).

Pärast seda, kui teisaldate plekid sisuandmebaasi uuesti, minge järgmise juhise juurde: [desinstallimine funktsiooni adapterit](#uninstall-the-adapter).

### <a name="uninstall-the-adapter"></a>Funktsiooni adapterit desinstallimine

Pärast seda, kui teisaldate plekid tagasi SQL serveri sisuandmebaaside, kasutage desinstallimiseks StorSimple adapterit SharePointi üks järgmistest suvanditest.

#### <a name="to-use-the-installation-program-to-uninstall-the-adapter"></a>Installiprogrammi abil soovitud adapterit desinstallimine 

1. (WFE) ees veebiserverisse sisse logimiseks kasutage administraatori õigustega kontot.
2. Topeltklõpsake StorSimple adapterit SharePointi installer. Häälestusviisard käivitatakse.

    ![Häälestusviisard](./media/storsimple-adapter-for-sharepoint/sasp2.png)

3. Klõpsake nuppu **edasi**. Kuvatakse järgmine leht.

    ![Häälestamise viisardi lehel eemaldamine](./media/storsimple-adapter-for-sharepoint/sasp3.png)

4. Klõpsake käsku **Eemalda** eemaldamise protsessi valimiseks. Kuvatakse järgmine leht.

    ![Häälestamise viisardi kinnituslehel](./media/storsimple-adapter-for-sharepoint/sasp4.png)

5. Klõpsake käsku **Eemalda** eemaldamise kinnitamiseks. Kuvatakse järgmised edenemise leht.

    ![Häälestamine pooleli Viisardilehe](./media/storsimple-adapter-for-sharepoint/sasp5.png)

6. Kui eemaldamine on lõpule jõudnud, kuvatakse lehe valmis. Klõpsake viisardi sulgemiseks **valmis** .

#### <a name="to-use-the-control-panel-to-uninstall-the-adapter"></a>Soovitud adapterit desinstallimine juhtpaneeli abil 

1. Avage juhtpaneel ja seejärel klõpsake nuppu **programmid ja funktsioonid**.

2. Valige **SharePointi StorSimple adapterit**ja seejärel klõpsake käsku **desinstalli**. 

## <a name="next-steps"></a>Järgmised sammud

[Lisateavet leiate teemast StorSimple kohta](storsimple-overview.md).

<!--Reference links-->
[1]: https://www.microsoft.com/download/details.aspx?id=44073
[2]: https://technet.microsoft.com/library/ff628583(v=office.15).aspx
[3]: https://technet.microsoft.com/library/ff628583(v=office.14).aspx
[4]: https://technet.microsoft.com/library/ff628569(v=office.14).aspx
[5]: https://technet.microsoft.com/library/ff628583(v=office.15).aspx
[8]: https://technet.microsoft.com/en-us/library/ff943565.aspx
