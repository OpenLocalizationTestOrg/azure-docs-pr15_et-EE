<properties
    pageTitle="SQL-i Assessment lahendus Log Analytics keskkonna optimeerimine | Microsoft Azure'i"
    description="Saate SQL-i Assessment lahendus tavaline intervalli risk ja oma serveri keskkonnas seisundi hindamiseks."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="banders"/>

# <a name="optimize-your-environment-with-the-sql-assessment-solution-in-log-analytics"></a>SQL-i Assessment lahendusega Log Analytics keskkonna optimeerimine


Saate SQL-i Assessment lahendus tavaline intervalli risk ja oma serveri keskkonnas seisundi hindamiseks. See artikkel aitab teil installida nii, et saaksite korrektiivläätsed toimingud võimalike probleemide lahendus.

See lahendus loetletakse tähtsuse soovitusi, mis on seotud teie juurutatud serveris taristu. Soovitused on liigitatud üle kuus fookuse valdkondades, mis aitavad teil kiiresti mõista riski ja võtmine.

Soovituste põhinevad teadmised ja kogemused Microsofti inseneride tuhandete klientide külastamine. Iga soovitus leiate juhised selle kohta, miks probleemi võib oluline on ja kuidas soovitatud muudatused rakendada.

Saate fookuse valdkondades, mis on kõige olulisemad teie organisatsioonile ja suunas töötab risk tasuta ja terve keskkonna edenemise jälgimine.

Pärast lisamist lahendus ja on lõpule viidud, kokkuvõte fookuse alade kohta on kuvatud **SQL-i Assessment** armatuurlaua infrastruktuuri teie keskkonnas. Järgmistes jaotistes kirjeldatakse teabe kasutamine **SQL-i Assessment** armatuurlaud, kus saate kuvada ja seejärel tehke oma SQL serveri infrastruktuuri Soovitatavad toimingud.

![SQL-i Assessment paani pilt](./media/log-analytics-sql-assessment/sql-assess-tile.png)

![SQL-i Assessment armatuurlaua pilt](./media/log-analytics-sql-assessment/sql-assess-dash.png)

## <a name="installing-and-configuring-the-solution"></a>Installimine ja konfigureerimine lahendus
SQL-i Assessment töötab kõigi praegu toetatud versioonid SQL serveri Standard, arendaja ja Enterprise versioonid.

Kasutage järgmist teavet, installida ja konfigureerida lahendus.

- Agentide peab olema installitud serverites, mis on SQL Server installitud.
- SQL-i Assessment lahendus nõuab .NET Framework 4 on OMS agent igasse arvutisse installitud.
- Toiminguhalduri agent hinnanguga SQL-i kasutamisel peate kasutama toimingute halduri Run-As konto. Lisateabe saamiseks vaadake [Toiminguhalduri Käivita nimega kontod OMS](#operations-manager-run-as-accounts-for-oms) all.

    >[AZURE.NOTE] MMA agent ei toeta toimingute halduri Run-As kontod.

- SQL-i Assessment lahendus lisada oma OMS tööruumi abil [lisada Log Analytics](log-analytics-add-solutions.md)Solutions lahendusegaleriist kirjeldatud. On pole veel konfigureerimine vajalik.

>[AZURE.NOTE] Kui olete lisanud lahenduse, lisatakse serverite agentide AdvisorAssessment.exe faili. Otsingukonfiguratsiooni andmete lugemine ja seejärel saadetakse OMS teenuse pilveteenuses töötlemiseks. Loogika on saadud andmetele rakendatud ja pilveteenusesse andmed.

## <a name="sql-assessment-data-collection-details"></a>SQL-i hindamise andmete kogumise üksikasjad

SQL-i Assessment kogub WMI andmeid, registriandmete, jõudlusandmeid ja SQL serveri dünaamilise juhtimise kuvada tulemid abil agentide, mida teil on lubatud.

Järgmises tabelis kuvatakse andmete kogumise meetodid agentide, kas toimingute Manager (SCOM) on nõutav, ja kuidas sageli andmeid kogutakse agent.

| platvorm | Otsest Agent | SCOM agent | Azure'i salvestusruum | SCOM on nõutav? | Rühma kaudu saadetud SCOM agendi andmed | saidikogumi sagedus |
|---|---|---|---|---|---|---|
|Windows|![Jah](./media/log-analytics-sql-assessment/oms-bullet-green.png)|![Jah](./media/log-analytics-sql-assessment/oms-bullet-green.png)|![Ei](./media/log-analytics-sql-assessment/oms-bullet-red.png)|    ![Ei](./media/log-analytics-sql-assessment/oms-bullet-red.png)|![Jah](./media/log-analytics-sql-assessment/oms-bullet-green.png)|   7 päeva|

## <a name="operations-manager-run-as-accounts-for-oms"></a>Käivita nimega Toiminguhalduri moodustab OMS

Log Analytics rakenduses OMS kasutab Toiminguhalduri agent ja haldus jaotises koguda ja OMS teenuse andmeid saata. OMS järgud korral halduse paketid töökoormus esitada väärtus lisada teenuseid. Iga töökoormus nõuab töökoormus kohased õiguste halduse paketid eri turvalisuse kontekstis, nt domeenikonto käivitamiseks. Peate mandaadi teavet toimingute halduri käivitamine nimega konto konfigureerimisega.

Kasutage järgmist teavet SQL-i Assessment Toiminguhalduri Käivita nimega konto määramiseks.

### <a name="set-the-run-as-account-for-sql-assessment"></a>SQL-i hindamise käivitamine nimega konto määramine

 Kui kasutate juba SQL Server management pack, kasutage selle konto käivitada nimega.

#### <a name="to-configure-the-sql-run-as-account-in-the-operations-console"></a>Toimingute konsooli SQL-i käivitada nagu konto konfigureerimine

>[AZURE.NOTE] Kui kasutate OMS otsekoheseks agent, mitte SCOM agent, töötab halduspakett toiminguhalduri alati kohalik süsteem konto turvalisus kontekstis. Juhiseid 1 – 5 allpool vahele jätta ja käivitage T-SQL- või PowerShelli valimi, milles NT AUTHORITY\SYSTEM on kasutajanimi.

1. Toiminguhalduri, avage toimingud konsooli ja seejärel klõpsake nuppu **haldus**.

2. **Käivitage konfigureerimine**jaotises **profiilid**ja avada **OMS SQL-i hindamise käivitamine nimega profiili**.

3. Klõpsake lehel **Käivitada kontod** nuppu **Lisa**.

4. Valige mõne Windowsi käivitada nagu konto, mis sisaldab ka SQL serveri mandaati või nuppu **Uus** , et luua.
    >[AZURE.NOTE] Windows tuleb käivitada nimega konto tüüp. Konto käivitada tuleb osa kohalike administraatorite rühma majutusteenuse SQL Serveri eksemplari kõigis Windows serverites.

5. Klõpsake nuppu **Salvesta**.

6. Saate muuta ja seejärel käivitada järgmine T-SQL valimi iga SQL Serveri eksemplari andmiseks täitma käivitada kontole SQL-i Assessment miinimumõigused. Siiski ei pea seda teha, kui käivitada nagu konto on juba süsteemiadministraator serveri rolli kohta SQL Serveri eksemplari.

```
---
    -- Replace <UserName> with the actual user name being used as Run As Account.
    USE master

    -- Create login for the user, comment this line if login is already created.
    CREATE LOGIN [<UserName>] FROM WINDOWS

    -- Grant permissions to user.
    GRANT VIEW SERVER STATE TO [<UserName>]
    GRANT VIEW ANY DEFINITION TO [<UserName>]
    GRANT VIEW ANY DATABASE TO [<UserName>]

    -- Add database user for all the databases on SQL Server Instance, this is required for connecting to individual databases.
    -- NOTE: This command must be run anytime new databases are added to SQL Server instances.
    EXEC sp_msforeachdb N'USE [?]; CREATE USER [<UserName>] FOR LOGIN [<UserName>];'

```
#### <a name="to-configure-the-sql-run-as-account-using-windows-powershell"></a>Windows PowerShelli kaudu SQL-i käivitada nimega konto konfigureerimine

Avage PowerShelli aken ja käivitage järgmine skript pärast värskendamist seda oma andmetega.

```

    import-module OperationsManager
    New-SCOMManagementGroupConnection "<your management group name>"
     
    $profile = Get-SCOMRunAsProfile -DisplayName "OMS SQL Assessment Run As Profile"
    $account = Get-SCOMrunAsAccount | Where-Object {$_.Name -eq "<your run as account name>"}
    Set-SCOMRunAsProfile -Action "Add" -Profile $Profile -Account $Account
```

## <a name="understanding-how-recommendations-are-prioritized"></a>Kuidas on prioriteetsed soovitused mõistmine

Iga soovitus on esitatud kaalu väärtus, mis tuvastab soovitust suhteline tähtsus. Kuvatakse ainult 10 kõige olulisemad soovitused.

### <a name="how-weights-are-calculated"></a>Kuidas kaalu arvutamine

Korrigeerimine on agregaatväärtused põhjal kolme peamised tegurid.

- *Tõenäosus* , et ei tuvastatud probleemi põhjustada probleeme. Suurem tõenäosus võrdub soovitust suurem üldine keskmine.

- *Mõju* kohta oma ettevõttes kui seda probleemi põhjustada probleem. Suurema mõju võrdub soovitust suurem üldine keskmine.

- *Peegeldav* soovituse. Suurema vaeva võrdub soovitust väiksem üldine keskmine.

Iga avamise soovituse osakaalu on saadaval iga ala kokku hinde protsentidena. Näiteks kui soovituse alal Turve ja nõuetele vastavus liikumine on 5% Keskmine, rakendamise soovituse suurendab teie üldine Turve ja nõuetele vastavus 5 Keskmine %.

### <a name="focus-areas"></a>Fookuse ala

**Turve ja nõuetele vastavus** – see ala kuvatakse soovitused ohtude ja seotud rikkumiste eest, ettevõtte poliitika ja tehnilise, juriidilise ja regulatiivse vastavuse nõuetele.

**Kättesaadavus ja järjepidevuse** – see ala kuvatakse soovitused teenuse kättesaadavus, paindlikkust infrastruktuuri ja business kaitse.

**Jõudlus ja skaleeritavus** – see ala näitab soovitused, mis aitavad teie asutusel IT taristu kasvada, veenduge, et teie IT-keskkonnas vastab praeguse jõudlus ja suudab muutuvate taristu vajadustega.

**Versiooniuuenduse migreerimine ja juurutamise** – see ala kuvatakse soovitusi, et aidata teil uuendada, migreerimine ja SQL serveri juurutamine oma olemasoleva taristu.

**Toimingute ja jälgimine** – see ala näitab soovitusi tõhustada teie IT tegevusi, rakendada ennetavad hooldustööd ja jõudluse maksimeerimine.

**Muuda ja konfiguratsiooni haldus** – see ala kuvatakse soovitused aitab kaitsta igapäevaste toimingute, et muudatused ei negatiivselt oma taristu, luua Muuda juhtelemendi toimingutest, jälgimine ja audit süsteemi konfiguratsioone.

### <a name="should-you-aim-to-score-100-in-every-focus-area"></a>Peaks teie eesmärk koguda iga ala 100%?

Ei pruugi olla. Soovitused põhinevad teadmised ja kogemused Microsofti insenerid tuhandete klientide külastamine. Siiski pole kahe serveri infrastruktuuri on samad, ja teatud soovitused võivad olla rohkem või vähem seotud. Mõned soovitused turvalisus võib olla näiteks vähem oluline, kui teie virtuaalmasinates puudub Interneti-ühendus. Mõned soovitused kättesaadavus võib vähem teenuseid, mis pakuvad ebaoluliste sihtotstarbelise andmete kogumine ja aruandlus. Probleemid, mis on olulised küps business võib olla väiksem oluline sisselülitamisel. Kui soovite tuvastada, milliseid liikumine on teie prioriteedid ja siis vaatame, kuidas oma hinded aja jooksul muutuda.

Iga soovitus sisaldab juhised selle kohta, miks on oluline. Kasutage juhised hindamaks, kas soovituse rakendamisega on sobiv, laadi oma IT-teenuseid ja business teie ettevõtte vajadustele.

## <a name="use-assessment-focus-area-recommendations"></a>Kasutage hindamise fookuse soovitused

Enne kui saate lahenduse hindamise OMS, peab teil olema installitud lahendus. Lisateavet lugeda Lisateavet lahenduste installimist [lisada Log Analytics lahenduste lahendusegaleriist](log-analytics-add-solutions.md). Pärast seda, kui see on installitud, saate vaadata Kokkuvõte soovituste abil SQL-i Assessment paani OMS lehel ülevaade.

Saate vaadata summeeritud vastavuse läbivaatust taristu ja seejärel drill üheks soovitusi.

### <a name="to-view-recommendations-for-a-focus-area-and-take-corrective-action"></a>Soovitused fookus ala vaatamiseks ja võtmine

1. Klõpsake lehe **Ülevaade** **SQL-i Assessment** paani.
2. Lehel **SQL-i hindamine** ühes fookuse ala labad Kokkuvõte teave üle ja seejärel klõpsake selle ala soovitused kuvamiseks.
3. Mis tahes fookuse ala, saate vaadata keskkonna tähtsuse soovitusi. Klõpsake soovitust **Mõjuta objektide** miks soovituse üksikasjade kuvamiseks klõpsake jaotises.  
    ![SQL-i hindamise soovitused pilt](./media/log-analytics-sql-assessment/sql-assess-focus.png)
4. Soovitatud **Soovitatavad toimingud**korrektiivläätsed toiminguid saate teha. Kui üksus on adresseeritud kirje hiljem hinnangute, mis võeti toimingud ja teie nõuetele vastavus Keskmine suureneb soovitatav. Parandatud üksused kuvatakse **Objektid on möödas**.

## <a name="ignore-recommendations"></a>Soovitused ignoreerimine

Kui teil on soovitusi, mida soovite ignoreerida, saate luua tekstifaili kasutavate OMS soovitusi kuvamist oma tulemuste vältimiseks.

### <a name="to-identify-recommendations-that-you-will-ignore"></a>Soovitused, mis te ignoreerivad tuvastamiseks

1.  Logige sisse oma tööruumi ja avage Logi otsing. Kasutada järgmist päringut loendist soovitused, mis ei ole arvutites teie keskkonnas.

    ```
    Type=SQLAssessmentRecommendation RecommendationResult=Failed | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```

    Siin on nähtaval Log otsingupäringu kuvatõmmis: ![nurjus soovitused](./media/log-analytics-sql-assessment/sql-assess-failed-recommendations.png)

2.  Valige soovitused, mida soovite ignoreerida. Saate kasutada väärtuste jaoks RecommendationId järgneva käigus.


### <a name="to-create-and-use-an-ignorerecommendationstxt-text-file"></a>Luua ja kasutada IgnoreRecommendations.txt tekstifaili

1.  Looge fail nimega IgnoreRecommendations.txt.
2.  Kleepige või tippige iga RecommendationId iga soovituse soovitud OMS soovite ignoreerida eraldi reale ja seejärel salvestage ja sulgege fail.
3.  Salvestage fail järgmises kaustas igas arvutis, kuhu OMS ignoreerida soovitusi.
    - Microsoft Agenti jälgimine (ühendatud otse või Toiminguhalduri kaudu) - arvutite *SystemDrive*: \Programmifailid\Microsoft jälgimis Agent\Agent
    - Toiminguhalduri management server - *SystemDrive*: \Programmifailid\Microsoft System Center 2012 R2\Operations Manager\Server

### <a name="to-verify-that-recommendations-are-ignored"></a>Kinnitamaks, et ignoreeritakse soovitused

1.  Pärast Järgmine ajastatud hindamise käivitamist vaikimisi iga 7 päeva, määratud soovitused on tähistatud ignoreeritavad ja armatuurlaual assessment ei kuvata.
2.  Log otsingupäringute abil saate loendi ignoreeritud soovitusi.

    ```
    Type=SQLAssessmentRecommendation RecommendationResult=Ignored | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```
3.  Kui otsustate hiljem, et soovite näha ignoreeritud soovitusi, mis tahes IgnoreRecommendations.txt failide eemaldamine või saate neid RecommendationIDs eemaldada.

## <a name="sql-assessment-solution-faq"></a>SQL-i Assessment lahenduse FAQ

*Kui sageli hinnang käivitada?*
- Hindamine käivitatakse iga 7 päeva.

*Kas on võimalik konfigureerida hindamise käivitumissagedust?*
- Pole sel ajal.

*Kui teise on pärast lisatud SQL-i hindamise lahenduse, määratakse?*
- Jah, kui seda hinnatakse iga 7 päeva, siis avastatakse.

*Kui server on kasutuselt, kui see eemaldatakse hindamise?*
- Kui server ei esita andmete 3 nädalat, eemaldatakse see.

*Mis on nimi, mis teeb andmete kogumise protsessi?*
- AdvisorAssessment.exe

*Kui kaua võtab andmete jaoks?*
- Tegelike andmete kogumise serveris kulub umbes 1 tund. Võib kuluda rohkem serverites, mis on suur hulk SQL-i eksemplari või andmebaasidele.

*Mis tüüpi andmeid kogutakse?*
- Järgmist tüüpi andmete kogumine:
    - WMI
    - Registri
    - Jõudluse hinnale
    - SQL-i dünaamilise halduse vaadete (DMV).

*Kas on võimalik konfigureerida, kui andmed on kogutud?*
- Pole sel ajal.

*Miks mul käivitada nagu konto konfigureerimine*
- SQL Server, käivitatakse väheste SQL-päringud. Selleks, et neid kasutada, tuleb kasutada käivitada nagu konto SQL-i vaade serveri olekus õigustega.  Lisaks WMI päringu, et kohaliku administraatori identimisteave on nõutav.

*Miks ei kuvata ainult ülemine 10 soovitused?*
- Asemel annab teile hinnatavate suur tööülesannete loendit, soovitame teil tähtsuse soovituste esmalt keskenduda. Pärast nende pöörduda lisasoovitused muutuvad kättesaadavaks. Kui soovite näha üksikasjalik loend, saate vaadata kõiki soovitusi OMS log otsingu abil.

*Kas on võimalik soovitus ignoreerida?*
- Jah, vt [Ignoreeri soovitused](#ignore-recommendations) ülaltoodud.



## <a name="next-steps"></a>Järgmised sammud

- [Otsingu logid](log-analytics-log-searches.md) vaadata üksikasjalikku SQL-i andmeid ja soovitusi.
