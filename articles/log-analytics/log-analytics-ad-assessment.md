<properties
    pageTitle="Keskkonna Active Directory Assessment lahendus Log Analytics optimeerimine | Microsoft Azure'i"
    description="Saate Active Directory Assessment lahendus tavaline intervalli risk ja oma serveri keskkonnas seisundi hindamiseks."
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
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="optimize-your-environment-with-the-active-directory-assessment-solution-in-log-analytics"></a>Keskkonna Active Directory Assessment lahendusega Log Analytics optimeerimine

Saate Active Directory Assessment lahendus tavaline intervalli risk ja oma serveri keskkonnas seisundi hindamiseks. See artikkel aitab teil installida ja kasutada nii, et saaksite korrektiivläätsed toimingud võimalike probleemide lahendus.

See lahendus loetletakse tähtsuse soovitusi, mis on seotud teie juurutatud serveris taristu. Soovitused on liigitatud üle neli fookuse valdkondades, mis aitavad teil kiiresti mõista riski ja tegutsema.

Soovitused põhinevad teadmised ja kogemused Microsofti inseneride tuhandete klientide külastamine. Iga soovitus leiate juhised selle kohta, miks probleemi võib oluline on ja kuidas soovitatud muudatused rakendada.

Saate fookuse valdkondades, mis on kõige olulisemad teie organisatsioonile ja suunas töötab risk tasuta ja terve keskkonna edenemise jälgimine.

Pärast lisamist lahendus ja on lõpule viidud, kokkuvõte kuvatakse teave fookuse alade **AD Assessment** armatuurlaual infrastruktuuri teie keskkonnas. Järgmistes jaotistes kirjeldatakse **AD Assessment** armatuurlaud, kus saate kuvada ja seejärel tehke Soovitatavad toimingud teie Active Directory serveri infrastruktuuri teabe kasutamine.

![SQL-i Assessment paani pilt](./media/log-analytics-ad-assessment/ad-tile.png)

![SQL-i Assessment armatuurlaua pilt](./media/log-analytics-ad-assessment/ad-assessment.png)


## <a name="installing-and-configuring-the-solution"></a>Installimine ja konfigureerimine lahendus
Kasutage järgmist teavet, installida ja konfigureerida lahendused.

- Agentide peab olema installitud domeenikontrolleritesse, mis kuuluvad domeeni hinnatav.
- Active Directory Assessment lahendus nõuab .NET Framework 4 on OMS agent igasse arvutisse installitud.
- Active Directory Assessment lahendus lisada oma OMS tööruumi [lisada Log Analytics lahenduste lahendusegaleriist](log-analytics-add-solutions.md)kirjeldatud protsessi abil.  On pole veel konfigureerimine vajalik.

    >[AZURE.NOTE] Kui olete lisanud lahenduse, lisatakse serverite agentide AdvisorAssessment.exe faili. Otsingukonfiguratsiooni andmete lugemine ja seejärel saadetakse OMS teenuse pilveteenuses töötlemiseks. Loogika on saadud andmetele rakendatud ja pilveteenusesse andmed.

## <a name="active-directory-assessment-data-collection-details"></a>Active Directory hindamise andmete kogumise üksikasjad

Active Directory Assessment kogub WMI andmeid, registriandmete ja tulemustega seotud andmete abil agentide, mida teil on lubatud.

Järgmises tabelis kuvatakse andmete kogumise meetodid agentide, kas toimingute Manager (SCOM) on nõutav, ja kuidas sageli andmeid kogutakse agent.

| platvorm | Otsest Agent | SCOM agent | Azure'i salvestusruum | SCOM on nõutav? | Rühma kaudu saadetud SCOM agendi andmed | saidikogumi sagedus |
|---|---|---|---|---|---|---|
|Windows|![Jah](./media/log-analytics-ad-assessment/oms-bullet-green.png)|![Jah](./media/log-analytics-ad-assessment/oms-bullet-green.png)|![Ei](./media/log-analytics-ad-assessment/oms-bullet-red.png)|   ![Ei](./media/log-analytics-ad-assessment/oms-bullet-red.png)|![Jah](./media/log-analytics-ad-assessment/oms-bullet-green.png)| 7 päeva|


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

**Versiooniuuenduse migreerimine ja juurutamise** – see ala kuvatakse soovitused aitavad täiendada, migreerimine ja juurutada Active Directory oma olemasoleva taristu.

### <a name="should-you-aim-to-score-100-in-every-focus-area"></a>Peaks teie eesmärk koguda iga ala 100%?

Ei pruugi olla. Soovitused põhinevad teadmised ja kogemused Microsofti insenerid tuhandete klientide külastamine. Siiski pole kahe serveri infrastruktuuri on samad, ja teatud soovitused võivad olla rohkem või vähem seotud. Mõned soovitused turvalisus võib olla näiteks vähem oluline, kui teie virtuaalmasinates puudub Interneti-ühendus. Mõned soovitused kättesaadavus võib vähem teenuseid, mis pakuvad ebaoluliste sihtotstarbelise andmete kogumine ja aruandlus. Probleemid, mis on olulised küps business võib olla väiksem oluline sisselülitamisel. Kui soovite tuvastada, milliseid liikumine on teie prioriteedid ja siis vaatame, kuidas oma hinded aja jooksul muutuda.

Iga soovitus sisaldab juhised selle kohta, miks on oluline. Kasutage juhised hindamaks, kas soovituse rakendamisega on sobiv, laadi oma IT-teenuseid ja business teie ettevõtte vajadustele.

## <a name="use-assessment-focus-area-recommendations"></a>Kasutage hindamise fookuse soovitused

Enne kui saate lahenduse hindamise OMS, peab teil olema installitud lahendus. Lisateavet lugeda Lisateavet lahenduste installimist [lisada Log Analytics lahenduste lahendusegaleriist](log-analytics-add-solutions.md). Kui see on installitud, saate vaadata Kokkuvõte soovituste abil paani Assessment ülevaade lehel OMS.

Saate vaadata summeeritud vastavuse läbivaatust taristu ja seejärel drill üheks soovitusi.


### <a name="to-view-recommendations-for-a-focus-area-and-take-corrective-action"></a>Soovitused fookus ala vaatamiseks ja võtmine

1. Klõpsake lehe **Ülevaade** oma serveri taristu **Assessment** paani.
2. Lehel **hindamine** ühes fookuse ala labad Kokkuvõte teave üle ja seejärel klõpsake selle ala soovitused kuvamiseks.
3. Mis tahes fookuse ala, saate vaadata keskkonna tähtsuse soovitusi. Klõpsake soovitust **Mõjuta objektide** miks soovituse üksikasjade kuvamiseks klõpsake jaotises.  
    ![pildi hindamise soovitusi](./media/log-analytics-ad-assessment/ad-focus.png)
4. Soovitatud **Soovitatavad toimingud**korrektiivläätsed toiminguid saate teha. Kui üksus on adresseeritud kirje hiljem hinnangute, mis võeti toimingud ja teie nõuetele vastavus Keskmine suureneb soovitatav. Parandatud üksused kuvatakse **Objektid on möödas**.

## <a name="ignore-recommendations"></a>Soovitused ignoreerimine

Kui teil on soovitusi, mida soovite ignoreerida, saate luua tekstifaili kasutavate OMS soovitusi kuvamist oma tulemuste vältimiseks.

### <a name="to-identify-recommendations-that-you-will-ignore"></a>Soovitused, mis te ignoreerivad tuvastamiseks

1.  Logige sisse oma tööruumi ja avage Logi otsing. Kasutada järgmist päringut loendist soovitused, mis ei ole arvutites teie keskkonnas.

    ```
    Type=ADAssessmentRecommendation RecommendationResult=Failed | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```

    Siin on nähtaval Log otsingupäringu kuvatõmmis: ![nurjus soovitused](./media/log-analytics-ad-assessment/ad-failed-recommendations.png)

2.  Valige soovitused, mida soovite ignoreerida. Saate kasutada väärtuste jaoks RecommendationId järgneva käigus.


### <a name="to-create-and-use-an-ignorerecommendationstxt-text-file"></a>Luua ja kasutada IgnoreRecommendations.txt tekstifaili

1.  Looge fail nimega IgnoreRecommendations.txt.
2.  Kleepige või tippige iga RecommendationId iga soovituse, et soovite Logi analüüsi, et eraldi reale ignoreerida ja seejärel salvestage ja sulgege fail.
3.  Salvestage fail järgmises kaustas igas arvutis, kuhu OMS ignoreerida soovitusi.
    - Microsoft Agenti jälgimine (ühendatud otse või Toiminguhalduri kaudu) - arvutite *SystemDrive*: \Programmifailid\Microsoft jälgimis Agent\Agent
    - Toiminguhalduri management server - *SystemDrive*: \Programmifailid\Microsoft System Center 2012 R2\Operations Manager\Server

### <a name="to-verify-that-recommendations-are-ignored"></a>Kinnitamaks, et ignoreeritakse soovitused

Pärast Järgmine ajastatud hindamise käivitamist vaikimisi iga 7 päeva, määratud soovitusi märgitakse *ignoreeritavad* ja armatuurlaual assessment ei kuvata.

1. Log otsingupäringute abil saate loendi ignoreeritud soovitusi.

    ```
    Type=ADAssessmentRecommendation RecommendationResult=Ignored | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```

2.  Kui otsustate hiljem, et soovite näha ignoreeritud soovitusi, mis tahes IgnoreRecommendations.txt failide eemaldamine või saate neid RecommendationIDs eemaldada.

## <a name="ad-assessment-solutions-faq"></a>AD Assessment lahenduste FAQ

*Kui sageli hinnang käivitada?*
- Hindamine käivitatakse iga 7 päeva.

*Kas on võimalik konfigureerida hindamise käivitumissagedust?*
- Pole sel ajal.

*Kui teise on pärast lisatud assessment lahenduse, määratakse?*
- Jah, kui seda hinnatakse iga 7 päeva, siis avastatakse.

*Kui server on kasutuselt, kui see eemaldatakse hindamise?*
- Kui server ei esita andmete 3 nädalat, eemaldatakse see.

*Mis on nimi, mis teeb andmete kogumise protsessi?*
- AdvisorAssessment.exe

*Kui kaua võtab andmete jaoks?*
- Tegelike andmete kogumise serveris kulub umbes 1 tund. Võib kuluda rohkem serverites, mis on suur hulk Active Directory serverite.

*Mis tüüpi andmeid kogutakse?*
- Järgmist tüüpi andmete kogumine:
    - WMI
    - Registri
    - Jõudluse hinnale

*Kas on võimalik konfigureerida, kui andmed on kogutud?*
- Pole sel ajal.

*Miks ei kuvata ainult ülemine 10 soovitused?*
- Asemel annab teile hinnatavate suur tööülesannete loendit, soovitame teil tähtsuse soovituste esmalt keskenduda. Pärast nende pöörduda lisasoovitused muutuvad kättesaadavaks. Kui soovite näha üksikasjalik loend, saate vaadata kõiki soovitusi Log otsingu abil.

*Kas on võimalik soovitus ignoreerida?*
- Jah, vt [Ignoreeri soovitused](#ignore-recommendations) ülaltoodud.


## <a name="next-steps"></a>Järgmised sammud

- [Log otsingud Log Analytics](log-analytics-log-searches.md) abil saate vaadata üksikasjalikku AD hindamise andmed ja soovitusi.
