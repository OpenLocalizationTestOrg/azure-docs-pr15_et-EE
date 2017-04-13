<properties
    pageTitle="Värskendage halduse lahenduse OMS | Microsoft Azure'i"
    description="Selles artiklis on mõeldud aidata teil mõista, kuidas see lahendus abil saate hallata oma Windowsi ja Linux arvutite värskendusi."
    services="operations-management-suite"
    documentationCenter=""
    authors="MGoedtel"
    manager="jwhit"
    editor=""
    />
<tags
    ms.service="operations-management-suite"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/14/2016"
    ms.author="magoedte"/>

# <a name="update-management-solution-in-omsmediaoms-solution-update-managementupdate-management-solution-iconpng-update-management-solution-in-oms"></a>![Halduse lahenduse OMS värskendamine](./media/oms-solution-update-management/update-management-solution-icon.png) Värskenduse halduse lahenduse OMS

OMS lahendus haldus võimaldab teil hallata oma Windowsi ja Linux arvutite värskendused.  Saate kiiresti hinnata saadaolevad värskendused agent kõigisse arvutitesse olekut ja installida vajalikud värskendused serverite algatada. 

## <a name="prerequisites"></a>Eeltingimused

-   Kas tuleb konfigureerida Windowsi agentide suhelda serveris Windows Server Update (WSUS) või Microsoft Update'i juurde.  

    >[AZURE.NOTE] Windows agent ei saa hallata samaaegselt System Center Configuration Manager.  
  
-   Linux agentide peab olema juurdepääs on värskendus hoidla.  OMS Agent Linuxi jaoks saate alla laadida [GitHub](https://github.com/microsoft/oms-agent-for-linux). 

## <a name="configuration"></a>Konfigureerimine

Järgmiste toimingute Update halduse lahenduse lisamine oma OMS tööruumi ja Linux agentide lisada.  Windowsi agentide lisatakse automaatselt ei konfiguratsiooni.

1.  Lisage oma OMS tööruumi abil [lisada OMS lahenduste](../log-analytics/log-analytics-add-solutions.md) lahendusegaleriist kirjeldatud lahendus haldus.  
2.  Valige **sätted** ja seejärel **Allikad ühendatud**OMS portaalis.  Pange tähele, et **Tööruumi ID** ning selle **Primaarvõtme** või **Teisese võtme**.
3.  Iga Linux arvutis tehke järgmist.

    lisamine.  Installige uusim versioon OMS agendi Linuxi käivitades järgmised käsud.  Asendage <Workspace ID> tööruumi ID-ga ja <Key> esmase või teisese võtme.

        cd ~
        wget https://github.com/Microsoft/OMS-Agent-for-Linux/releases/download/v1.2.0-75/omsagent-1.2.0-75.universal.x64.sh
        sudo bash omsagent-1.2.0-75.universal.x64.sh --upgrade -w <Workspace ID> -s <Key>

     b. Eemaldada agent, käivitage järgmine käsk.

        sudo bash omsagent-1.2.0-75.universal.x64.sh --purge

## <a name="management-packs"></a>Juhtimise tuvastamine

Kui teie süsteemi keskmist Toiminguhalduri rühma OMS tööruumis on ühendatud, siis järgmised halduse paketid installitakse Toiminguhalduri kui lisate seda lahendust. Ei ole konfiguratsiooni või halduse komplektid nõutav hooldustööd. 

-   Microsoft System Center Advisor Update Assessment ärianalüüsi pakett (Microsoft.IntelligencePacks.UpdateAssessment)
-   Microsoft.IntelligencePack.UpdateAssessment.Configuration (Microsoft.IntelligencePack.UpdateAssessment.Configuration)
-   Juurutamise MP värskendamine

Lahendus halduse paketid värskendamise kohta leiate lisateavet teemast [Ühenduse Toiminguhalduri Log Analytics](../log-analytics/log-analytics-om-agents.md).

## <a name="data-collection"></a>Andmete kogumine

### <a name="supported-agents"></a>Toetatud agentide.

Järgmises tabelis kirjeldatakse ühendatud allikad, mis ei toeta seda lahendust.

Ühendatud allikas | Toetatud | Kirjeldus|
----------|----------|----------|
Windowsi agentide | Jah | Lahendus kogub teavet süsteemi värskendused Windows agentide ja käivitab nõutav värskenduste installimine.|
Linux agentide | Jah | Lahendus kogub teavet süsteemi värskendused Linux agentide.|
Toimingute halduri rühma | Jah | Lahendus kogub teavet süsteemi uuendused kaudu ühendatud haldus jaotises.<br>Log Kasutusanalüüsi Toiminguhalduri agendi otsene ühendus ei ole vaja. Andmed edasi OMS hoidla haldus rühmast.|
Azure storage konto | Ei | Azure'i salvestusruumi ei sisalda teavet süsteemi värskendused.|  

### <a name="collection-frequency"></a>Saidikogumi sagedus

Iga hallatava Windows arvutis toimub kaks korda päevas skannida.  Kui värskendus on installitud, värskendatakse selle teabe 15 minuti jooksul.  

Iga hallatava Linux arvutis toimub kontrolli iga 3 tunni järel.  

## <a name="using-the-solution"></a>Lahendus abil

Värskenduste haldamine lahenduse lisamisel oma OMS tööruumi paani **Haldus** lisatakse OMS armatuurlauale. Sellel paanil kuvatakse teie keskkonnas praegu on vaja süsteemi uuendused count ja graafiliselt arvutite arvu.<br><br>
![Värskendage Kokkuvõte paan haldus](media/oms-solution-update-management/update-management-summary-tile.png)  

## <a name="viewing-update-assessments"></a>Vaatamise Update hindamine

Klõpsake paani avamiseks **Update halduse** armatuurlaua **Haldus** . Armatuurlaua sisaldab järgmises tabelis veerud. Igas veerus kuni kümme üksust määratud ulatuse ja ajavahemiku selle veeru kriteeriumidele vastavaid. Saate käivitada log otsing, mis tagastab kõik kirjed, klõpsates **näha kõiki** allosas veerg või klõpsake selle veeru päist.

Veerg | Kirjeldus|
----------|----------|
**Puuduvad värskendused arvutites** ||
Kriitiline või turbevärskendusi | Loendite ülaosas on puudu kümme arvutites värskendab sorditud värskenduste arvu need on puudu. Arvuti nimi esitus kõigi kirjete arvuti värskendamine log otsingu käivitamiseks klõpsake nuppu.|
Kriitiline või turbevärskendusi, mis on vanemad kui 30 päeva| Arvutid, mis on arvu tuvastab puuduvad kriitilised või turbevärskendusi, mis on rühmitatud aeg, kuna värskenduse on avaldatud. Üks esitus kõigi puuduvate ja kriitiliste värskenduste log otsingu käivitamiseks klõpsake nuppu.|
**Nõutav puuduvad värskendused**||
Kriitiline või turbevärskendusi | Loetleb liigitusi värskenduste, et arvutis on puudu sorditud arvutite puuduvad värskendused kategooria arvu. Klõpsake nuppu log esitus kõik sellele kirjete värskendamise otsingu käivitamiseks liigitus.|
**Juurutuste värskendamine**||
Juurutuste värskendamine | Käivitage praegu ajastatud värskendamise juurutuste ja kestus järgmise määratud arv.  Klõpsake paani vaate ajakavade, praegu töötavate ja lõplikus värskenduste või kavandada uue juurutamise.|  
<br>  
![Värskendage halduse armatuurlaua Kokkuvõte](./media/oms-solution-update-management/update-management-deployment-dashboard.png)<br>  
<br>
![Halduse armatuurlaua arvuti kuva värskendamine](./media/oms-solution-update-management/update-management-assessment-computer-view.png)<br>  
<br>
![Värskendage halduse armatuurlaua paketi vaade](./media/oms-solution-update-management/update-management-assessment-package-view.png)<br>  

## <a name="installing-updates"></a>Värskenduste installimine

Kui värskendused on hinnatud kõigi Windows arvutites teie keskkonnas, võib vajalik värskendused on installitud, luues on *Rakendamine*.  Mõne rakendamine on nõutav värskenduste ühe või mitme Windows arvutis ajastatud installi.  Saate määrata kuupäeva ja kellaaja juurutamiseks Lisaks arvutit või arvutites, mis tuleb kaasata.  

Azure'i automaatika tegevusraamatud installitud värskenduste.  Te ei saa praegu vaadata nende tegevusraamatud ja nad ei nõua konfiguratsiooni.  Kui ka värskendada juurutamise on loodud, loob see ajakava selle käivitamise juhtslaidi update käitusjuhendi määratud ajal kaasatud arvutites.  Selle juhtslaidi käitusjuhendi alustatakse lapse käitusjuhendi iga Windows agent, mis sooritavad nõutav värskenduste installimine.  

### <a name="viewing-update-deployments"></a>Vaatamise update juurutuste

Klõpsake paani **Rakendamine** olemasoleva Update juurutuste loendi kuvamiseks.  Need on rühmitatud olek – **ajastatud**, **kus töötab**ja **lõpule viidud**.<br><br> ![Värskendage lehel juurutuste ajakava](./media/oms-solution-update-management/update-updatedeployment-schedule-page.png)<br>  

Järgmises tabelis on kirjeldatud atribuudid kuvatakse iga rakendamine.

Atribuut | Kirjeldus|
----------|----------|
Nimi | Värskenduse juurutamise nimi.|
Ajakava | Ajakava tüüp.  *OneTime* praegu ainult võimalik väärtus.|
Algus|Kuupäev ja kellaaeg, mille käivitamiseks on ajastatud värskendamine juurutamise.|
Kestus | Värskenduse juurutuse lubatud minutite arv.  Kui kõik värskendused on installitud sees selle kestus, siis ülejäänud värskendused ootama, kuni järgmise värskenduse juurutamine.|
Serverid | Värskenduse juurutamise poolt mõjutatud arvutite arv.|
Olek | Praegune värskendus juurutamise olek.<br><br>Võimalikud väärtused on:<br>-Pole alustatud<br>-Töötab<br>-Viimistletud|  

Klõpsake nuppu Värskenda positsioonidel vaadata oma üksikasjalikult, mis sisaldab veergude järgmises tabelis.  Need veerud pole olema täidetud järgmised, kui värskenduse juurutamise pole veel alanud.<br>

Veerg | Kirjeldus|
----------|----------|
**Arvuti tulemused**||
Lõpule viidud | Loendite värskendamine juurutuse arvutite arvu oleku järgi.  Klõpsake mõnda olekut esitus kõik värskendada kirjeid selle olekuga juurutamiseks värskendamine log otsingu käivitamiseks.|
Arvuti installioleku| Loetleb kaasatud Update juurutus- ja värskendustega, mis on installitud protsent arvutites. Üks esitus kõigi puuduvate ja kriitiliste värskenduste log otsingu käivitamiseks klõpsake nuppu.|
**Eksemplari tulemuste värskendamine**||
Eksemplari installioleku | Loetleb liigitusi värskenduste, et arvutis on puudu sorditud arvutite puuduvad värskendused kategooria arvu. Klõpsake nuppu log esitus kõigi kirjete arvuti värskendamine otsingu käivitamiseks arvutis.|  
<br><br> ![Värskenduse juurutamise tulemuste ülevaade](./media/oms-solution-update-management/update-la-updaterunresults-page.png)

### <a name="creating-an-update-deployment"></a>Luua mõne rakendamine

Uue värskenduse juurutamise loomiseks klõpsake ikooni **Uus rakendamine** lehe avamiseks ekraani ülaosas nuppu **Lisa** .  Väärtused peate sisestama järgmise tabeli atribuudid.

Atribuut | Kirjeldus|
----------|----------|
Nimi | Värskenduse juurutamise tuvastamiseks kordumatu nimi.|
Ajavöönd | Ajavööndi kasutamiseks algusaeg.|
Algus | Käivitage värskendamine juurutamise kuupäev ja kellaaeg.|
Kestus | Värskenduse juurutuse lubatud minutite arv.  Kui kõik värskendused on installitud sees selle kestus, siis ülejäänud värskendused ootama, kuni järgmise värskenduse juurutamine.|
Arvutites | Nimed, arvutit või arvuti rühmad Update juurutuse kaasamiseks.  Valige rippmenüüst üks või mitu kirjet.|
<br><br> ![Uute värskenduste juurutamise lehel](./media/oms-solution-update-management/update-newupdaterun-page.png)

### <a name="time-range"></a>Ajavahemiku

Vaikimisi on analüüsitud Update halduse lahenduse andmete kõigi ühendatud haldus rühmadest, mis on loodud viimase 1 päeva jooksul. 

Ajavahemiku andmete muutmiseks valige **andmete põhjal** armatuurlaua ülaosas. Valige kirjed, mis on loodud või värskendatud viimase 7 päeva, 1 päeva ja 6 tunni jooksul. Või saate valida **kohandatud** ja määrata kohandatud ajavahemiku lõikes.<br><br> ![Kohandatud ajavahemiku suvand](./media/oms-solution-update-management/update-la-time-range-scope-databasedon.png)  

## <a name="log-analytics-records"></a>Kasutusanalüüsi kirjed

Värskenduse halduse lahenduse loob OMS hoidla kahte tüüpi kirjeid.

### <a name="update-records"></a>Kirjete värskendamine

Iga värskenduse, mis on installitud või vaja igas arvutis on loodud kirje tüüpi **värskendada** . Järgmises tabelis on atribuutide värskendada kirjeid.

Atribuut | Kirjeldus|
----------|----------|
Tüüp | *Värskendamine*|
SourceSystem | Kinnitatud värskenduse installimist allikas.<br>Võimalikud väärtused on:<br>– Microsoft Update<br>– Windows Update.<br>-SCCM<br>-Linux Servers (tuua paketi haldurid)|
Kinnitatud | Saate määrata, kas värskendus on kinnitatud installi.<br> Linux serverite see pole praegu kohustuslik nimega OMS lappimine ei Halda.|
Windowsi liigitamine | Värskenduse liigitus.<br>Võimalikud väärtused on:<br>-Rakendused<br>-Kriitilised värskendused<br>– Värskendused määratlus<br>-Funktsioon tuvastamine<br>– Värskendused turvalisus<br>-Hoolduspaketid<br>-Värskenduskomplektide<br>-Värskendused|
Linuxi liigitamine | Värskenduse cassification.<br>Võimalikud väärtused on:<br>-Kriitilised värskendused<br>– Värskendused turvalisus<br>-Muud värskendused|
Arvuti | Arvuti nimi.|
InstallTimeAvailable | Saate määrata, kas installimise ajal on saadaval muude agentide, mis sama värskenduse installimist.|
InstallTimePredictionSeconds | Hinnanguline installimise aeg sekundites, mis põhineb agentide, mis sama värskenduse installimist.|
KBID | KB artikkel, mis kirjeldab värskenduse ID-d.|
ManagementGroupName | SCOM agentide haldus rühma nimi.  Muude töötajatega, see on AOI -<workspace ID>.|
MSRCBulletinID | ID, mis kirjeldab värskenduse Microsoft turvabülletääni.|
MSRCSeverity | Microsoft turvabülletääni tõsidust.<br>Võimalikud väärtused on:<br>-Kriitilise<br>-Olulised<br>-Modereerimine|
Valikuline. | Saate määrata, kas värskendamine on valikuline.|
Toote | Värskenduse toote nimi on.  Klõpsake **vaate** avamiseks brauseris artikkel.|
PackageSeverity | Fikseeritud selle värskenduse Linux distributsiooni tarnijate esitatud haavatavuse tõsidust. | 
PublishDate | Kuupäev ja kellaaeg, mis see värskendus installiti.|
RebootBehavior | Saate määrata, kui värskenduse jõustab uuesti.<br>Võimalikud väärtused on:<br>-canrequestreboot<br>-neverreboots|
RevisionNumber | Redaktsioonide arv värskenduse.|
SourceComputerId | GUID arvuti tuvastamiseks.|
TimeGenerated | Kuupäev ja kellaaeg viimati värskendati kirje.|
Pealkiri | Värskenduse nime.|
UpdateID | GUID värskenduse tuvastamiseks.|
UpdateState | Saate määrata, kas see arvutisse on installitud värskendus.<br>Võimalikud väärtused on:<br>-Installitud - värskenduse on sellesse arvutisse installitud.<br>-Vaja - värskendus on installitud ja selles arvutis pole vaja.|  

<br>
Mis tahes Logi otsing, mis tagastab kirjed, millel **värskenduse** tüüp täitmisel saate valida **värskenduste** vaade, mis kuvab kogumi paanid kokkuvõtmine tagastatud Otsi värskendusi. Võite klõpsata kannete **kadunud ja rakendatud värskendused** ja **kohustuslike ning valikuliste värskenduste** paanide ulatus vaate selle kogumisse värskendused. Valige **loendist** või **tabelist** vaade üksikkirjete tagastamiseks.<br> 

![Logi otsing värskendada vaadata värskendusega kirje tüüp](./media/oms-solution-update-management/update-la-view-updates.png)  

**Tabeli** vaates, võite klõpsata **KBID** iga kirje puhul teabebaasi artikkel brauseris avada. See võimaldab teil kiiresti lugeda kindla värskenduse üksikasjad.<br> 

![Logi otsing tabelivaade paanid kirjetüübi värskendused](./media/oms-solution-update-management/update-la-view-table.png)

**Loendivaates,** saate linki **Kuva** avamiseks teabebaasi artikkel KBID kõrval.<br>

![Logi otsing loendivaate paanid kirjetüübi värskendused](./media/oms-solution-update-management/update-la-view-list.png)


###<a name="updatesummary-records"></a>UpdateSummary kirjed

**UpdateSummary** tüüpi kirje on loodud iga Windowsi agent arvuti. Seda kirjet värskendatakse iga kord, kui arvuti on skannitud värskendusi. **UpdateSummary** kirjed on järgmise tabeli atribuudid.

Atribuut | Kirjeldus|
----------|----------|
Tüüp | UpdateSummary|
SourceSystem | OpsManager |
Arvuti | Arvuti nimi.|
CriticalUpdatesMissing | Puuduvad arvutis kriitiliste värskenduste arv.|
ManagementGroupName | SCOM agentide haldus rühma nimi. Muude töötajatega, see on AOI -<workspace ID>.|
NETRuntimeVersion | Arvutisse installitud .NET Runtime'i versiooni.|
OldestMissingSecurityUpdateBucket | Avaldatud kopp liigitamiseks aeg, kuna selle arvuti värskendamine vanimast puuduvad turvalisus.<br>Võimalikud väärtused on:<br>-Vanem<br>-180 päeva tagasi<br>-150 päeva tagasi<br>-120 päeva tagasi<br>-90 päeva tagasi<br>– 60 päeva tagasi<br>-Avage 30 päeva<br>-Tehtud|
OldestMissingSecurityUpdateInDays | Päevade, kuna selle arvuti värskendamine vanimast puuduvad turvalisus on avaldatud.|
OsVersion | Arvutisse installitud operatsioonisüsteemi versiooni.|
OtherUpdatesMissing | Muud värskendused arvutis puuduvad arv.|
SecurityUpdatesMissing | Turbevärskendusi, mis on puudu arvutis arv.|
SourceComputerId | GUID arvuti tuvastamiseks.|
TimeGenerated | Kuupäev ja kellaaeg viimati värskendati kirje.|
TotalUpdatesMissing |Arvutis puuduvad värskendused koguarv.|
WindowsUpdateAgentVersion | Windows Update agent arvutis versiooninumber.|
WindowsUpdateSetting | Kuidas arvuti installib olulised värskendused säte.<br>Võimalikud väärtused on:<br>-Keelatud<br>-Teavitada enne installimist<br>-Ajastatud installi|
WSUSServer | URL-i, WSUS server, kui arvuti on konfigureeritud kasutama ühte.|  

## <a name="sample-log-searches"></a>Valimi log otsingud

Järgmises tabelis on ära toodud valimi log otsib värskendada kirjeid kogutud see lahendus. 

Päringu | Kirjeldus|
----------|----------|
Kõigi arvutite puuduvad värskendused | Tüüp = Update UpdateState = vaja valikuline = false & #124; Valige arvuti, pealkirja, KBID, liigitamine, UpdateSeverity, PublishedDate|
Puuduvad värskendused arvuti "COMPUTER01.contoso.com" (asendaja arvuti nimi) | Tüüp = Update UpdateState = vaja valikuline = false arvuti = "COMPUTER01.contoso.com" & #124; Valige arvuti, pealkirja, KBID, toote, UpdateSeverity, PublishedDate|
Kõik arvutid puuduva kriitilised või turbevärskendusi | Tüüp = Update UpdateState = vaja valikuline = false (liigitamine = "Turbevärskendusi" või liigitamine = "Kriitilised värskendused")|
Kriitiline või turberühma vaja masinad, kus värskenduste käsitsi rakendatakse värskendused | Tüüp = Update UpdateState = vaja valikuline = false (liigitamine = "Turbevärskendusi" või liigitamine = "Kriitilised värskendused") arvuti tolli {tüüp = UpdateSummary WindowsUpdateSetting = käsitsi & #124; Erinevate arvuti} & #124; Erinevate KBID|
Tõrge sündmuste masinad, mis on puudu kriitilised või turvalisus vajalikud värskendused | Tüüp = sündmuse EventLevelName tõrge arvuti tolli = {tüüp = Update (liigitamine = "Turbevärskendusi" või liigitamine = "Kriitilised värskendused") UpdateState = vaja valikuline = false & #124; Erinevate arvutis}|
Kõik arvutid puuduva värskenduskomplektide | Tüüp = valikuline värskendus = false liigitamine = "Värskenduskomplektide" UpdateState = vaja & #124; Valige arvuti, pealkirja, KBID, liigitamine, UpdateSeverity, PublishedDate|
Erinevate puuduvad värskendused üle kõik arvutid | Tüüp = Update UpdateState = vaja valikuline = false & #124; Erinevate tiitel|
WSUS arvuti liikmelisus | Tüüp = UpdateSummary & #124; Mõõtke count() WSUSServer järgi|
Automaatne uuendamine konfigureerimine | Tüüp = UpdateSummary & #124; Mõõtke count() WindowsUpdateSetting järgi|
Arvutid automaatne uuendamine on keelatud. | Tüüp = UpdateSummary WindowsUpdateSetting = käsitsi|  
Loendi kõigi Linux masinad, mis on saadaval paketi värskendus | Tüüp = värskenduse ja OSType = Linux ja UpdateState! = "Pole vaja" & #124; Mõõtke count() arvuti|
Kõik Linux masinad, mis on saadaval paketi värskendus loend, mis kriitiline või turberühma haavatavuse aadressid | Tüüp = värskenduse ja OSType = Linux ja UpdateState! = "Pole vaja" ja (liigitamine = "Kriitiliste värskenduste" või liigitamine = "Turbevärskendusi") & #124; Mõõtke count() arvuti|
Loendi kõigi paketid, mis on saadaval värskendus | Tüüp = värskenduse ja OSType = Linux ja UpdateState! = "Pole vaja"|
Loendi kõigi paketid, mis on saadaval värskendus, mis kriitiline või turberühma haavatavuse aadressid | Tüüp = värskenduse ja OSType = Linux ja UpdateState! = "Pole vaja" ja (liigitamine = "Kriitilised värskendused" või liigitamine = "Turbevärskendusi")|
"Ubuntu" masinad saadaval värskendusega loend | Tüüp = värskenduse ja OSType = Linux ja OSName = Ubuntu & #124; Mõõtke count() arvuti|

## <a name="next-steps"></a>Järgmised sammud

- [Log Analytics](../log-analytics/log-analytics-log-searches.md) Log otsingute abil vaadata üksikasjalikku Värskenda andmed.

- [Oma armatuurlaudade loomine](../log-analytics/log-analytics-dashboards.md) kuvav update vastavuse hallatava arvutite jaoks.

- [Teatiste loomine](../log-analytics/log-analytics-alerts.md) kui kriitilised värskendused on avastatud puuduvad arvutit või arvuti on keelatud automaatvärskendused.  




