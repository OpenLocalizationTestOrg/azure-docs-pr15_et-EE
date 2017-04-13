<properties 
    pageTitle="Sõiduki telemeetria analytics lahenduse malli PowerBI armatuurlaua häälestusjuhised | Microsoft Azure'i" 
    description="Cortana ärianalüüsi võimaluste kasutamiseks saada reaalajas ja ennustav ülevaateid sõiduki seisundi ja juhtimise tarbetu." 
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
    ms.date="09/12/2016" 
    ms.author="bradsev" />


# <a name="vehicle-telemetry-analytics-solution-template-powerbi-dashboard-setup-instructions"></a>Sõiduki telemeetria analytics lahenduse malli PowerBI armatuurlaua häälestusjuhised

See **menüü** linke selle playbook peatükke. 

[AZURE.INCLUDE [cap-vehicle-telemetry-playbook-selector](../../includes/cap-vehicle-telemetry-playbook-selector.md)]


Lahendus sõiduki telemeetria Analytics tutvustatakse, kuidas autosalongides, auto tootjad ja kindlustusseltside saate kasutada Cortana ärianalüüsi võimaluste saada reaalajas ja sõnastikupõhise teadmisi, sõiduki seisundi ja juhtimise tarbetu parandada ala kliendi võimalusi, R & D ja turunduskampaaniate. See dokument sisaldab samm-sammult juhised, kuidas saate konfigureerida PowerBI aruannete ja armatuurlaudade pärast tellimuse lahendus on juurutatud. 


## <a name="prerequisites"></a>Eeltingimused
1.  Navigeerimise [https://gallery.cortanaanalytics.com/SolutionTemplate/Vehicle-Telemetry-Analytics-3](https://gallery.cortanaanalytics.com/SolutionTemplate/Vehicle-Telemetry-Analytics-3) sõiduki telemeetria Analytics lahenduse juurutamine  
2.  [Microsoft Power BI Desktopi installimine](http://www.microsoft.com/download/details.aspx?id=45331)
3.  On [Azure tellimust](https://azure.microsoft.com/pricing/free-trial/). Kui teil pole Azure tellimuse, alustamine Azure tasuta tellimus
4.  PowerBI Microsofti konto
    

## <a name="cortana-intelligence-suite-components"></a>Cortana ärianalüüsi komplekti komponendid
Osana sõiduki telemeetria Analytics lahenduse malli Cortana ärianalüüsi järgmisi teenuseid on juurutatud tellimuse.

- **Sündmuse jaoturi** söömisega miljoneid sõiduki telemeetria sündmuste Azure'i sisse.
- **Voo analüütiline**s üha reaalajas teadmisi, sõiduki seisund ei lahene andmeid pikemaks rikkalikumat paketi Analyticsi sisse.
- **Seadme õ** normaalne tuvastamise reaalajas ja Pakktöötlus sõnastikupõhise teadmisi saada.
- **Hdinsightiga** on võimendada, et muuta andmete skaala
- **Andmete Factory** tegeleb korraldamise, kavandamine, ressursside haldamine ja Pakktöötlus müügivõimaluste jälgimine.

**Power BI** annab see lahendus rikkaliku armatuurlaua reaalajas andmed ja ennustav visualiseeringuid. 

Lahendus kasutab kaks erinevatest andmeallikatest: **jäljendatud sõiduki signaale ja diagnostika andmekomplekti** ja **sõiduki kataloogi**.

Sõiduki telemaatika simulaatorit on see lahendus kaasatud. See eraldab diagnostikateave ja vastav sõiduki olek ja juhtimise mustri antud hetkel aega. 

Sõiduki kataloogi on viide andmekomplekti sisaldavate VIN mudeli kaardistamine


## <a name="powerbi-dashboard-preparation"></a>PowerBI armatuurlaua ettevalmistamine

### <a name="deployment"></a>Juurutamine

Kui juurutamise on lõpule viidud, peaksite nägema Järgmine diagramm kõik need komponendid, mis on tähistatud rohelise värviga. 

- Et kontrollida, kas kõik need on edukalt juurutatud vastavate teenuste liikumiseks klõpsake roheline sõlmed paremas ülaservas olevat noolt.
- Andmete simulaatorit paketi allalaadimiseks klõpsake akna ülemises paremas sõlme **Sõiduki telemaatika simulaatorit** noolt. Salvestage ja ekstrakti faile kohalikult teie arvutis. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/1-deployed-components.png)

Nüüd olete valmis saada reaalajas rikkalike visualiseeringute ja PowerBI armatuurlaua konfigureerimine ja harjumused sõnastikupõhise teadmisi sõiduki seisundi ja juhtimine. Kulub tunniks kõigi aruannete loomine ja konfigureerimine armatuurlaua umbes 45 minutit. 


### <a name="setup-power-bi-real-time-dashboard"></a>Power BI reaalajas armatuurlaua häälestamine

**Jäljendatud andmete loomiseks**

1. Oma kohalikus arvutis, avage kaust, kuhu ekstraktitud sõiduki telemaatika simulaatorit paketi![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/2-vehicle-telematics-simulator-folder.png)
2.  Rakenduse ***CarEventGenerator.exe***käivitada.
3.  See eraldab diagnostikateave ja vastav sõiduki olek ja juhtimise mustri antud hetkel aega. See on avaldatud Azure'i sündmuse jaoturi eksemplari, mis on konfigureeritud juurutamise käigus.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/3-vehicle-telematics-diagnostics.png)
     
**Käivitage rakendus reaalajas armatuurlaud**

Lahendus sisaldab rakendus, mis loob PowerBI reaalajas armatuurlaud. See rakendus on kuulata sündmuse jaoturi eksemplari, millest voo Analytics avaldab sündmuste pidevalt. Iga sündmuse, mis selle rakenduse saab töötleb arvuti õ päringu vastuse hinded endpoint andmed. Tulemuseks andmekomplekti on avaldatud PowerBI tõuketeatised API-de visualiseerimiseks. 

Rakenduse allalaadimiseks tehke järgmist.

1.  Klõpsake diagrammi vaate PowerBI sõlme ja nuppu **Alla laadida reaalajas armatuurlaua rakenduse**' link paanil atribuudid.![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard-new1.png)
2.  Ekstraktida ja rakenduse kohalik salvestamine![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/4-real-time-dashboard-application.png)

3.  Rakenduse **RealtimeDashboardApp.exe** käivitada
4.  Kehtiv Power BI mandaat, logige sisse ja klõpsake nuppu **Aktsepteeri**
    
    ![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/5-sign-into-powerbi.png)
    
    ![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/6-powerbi-dashboard-permissions.png)


### <a name="configure-powerbi-reports"></a>PowerBI aruannete konfigureerimine
Reaalajas aruandeid ja armatuurlaud võtta lõpuleviimiseks umbes 30-45 minutit. Liikuge sirvides [http://powerbi.com](http://powerbi.com) ja logige sisse.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/6-1-powerbi-signin.png)

Power BI on loodud uue andmekomplekti. Klõpsake **ConnectedCarsRealtime** andmekomplekti.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/7-select-connected-cars-realtime-dataset.png)

Salvestage tühja aruande, kasutades **klahvikombinatsiooni Ctrl + s**.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/8-save-blank-report.png)

Sisestage *sõiduki telemeetria Analytics reaalaja - aruannete*aruande nimi.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/9-provide-report-name.png)

## <a name="real-time-reports"></a>Reaalajas aruanded
See lahendus on kolm reaalajas aruandeid.

1.  Sõidukite toiming
2.  Sõidukid hooldustööd
3.  Sõidukite seisund statistika

Saate konfigureerida kolme reaalajas aruanded või peatamine pärast iga etapi ja jätkake järgmises jaotises konfigureerimise paketi aruandeid. Soovitame teil luua kolm aruandeid, visualiseerida täielik lahendus reaalajas tee teadmisi.  

### <a name="1-vehicles-in-operation"></a>1. toiming sõidukite
  
Topeltklõpsake **lehel 1** ja pange sellele "Juhtimiseks toiming"  
    ![Ühendatud auto - sõidukite toiming](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4a.png)  

Valige **vin** välja **väljadelt** ja visualiseeringu tüübi nimega **"Kaart"**.  

Kaardivisualiseering on loodud, nagu on näidatud joonisel.  
    ![Ühendatud auto - valige vin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4b.png)

Klõpsake uue visualiseeringu lisada tühja ala.  

Valige väljade **linn** ja **vin** . Saate muuta visualiseeringu **"Kaart"**. Lohistage **vin** alale väärtused. Lohistage **linna** väljad alale **Legend** .   
    ![Ühendatud auto - Kaardivisualiseering](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4c.png)
  
Valige **Vorming** jaotis **visualiseeringuid**, klõpsake **pealkirja** ja muuta **teksti** **"**juhtimiseks toiming linnade järgi".  
    ![Ühendatud auto - sõidukite linna kasutusel](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4d.png)   

Lõplik visualiseeringu näeb välja nagu on näidatud joonisel.    
    ![Ühendatud auto - lõplik visualiseerimine](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4e.png)

Klõpsake uue visualiseeringu lisada tühja ala.  

Valige **linn** ja **vin**, **Rühmitatud tulpdiagrammi**visualiseeringu tüübi. Veenduge, et välja **linn** **telje ala** ja **vin** **väärtus** ala  

Diagrammi **"Vin arvu"** järgi sortimine  
    ![Ühendatud auto - vin arv](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4f.png)  

Muuta diagrammi **tiitel** **"** juhtimiseks toiming linnade järgi"  

Klõpsake jaotises **Vorming** ja seejärel valige **Andmete värvid**, klõpsake selle **"On"** **Kuva** kõik  
    ![Ühendatud auto - kuvada kõik andmed värvid](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4g.png)  

Üksikud linnade värvi muutmiseks klõpsake värvi ikooni.  
    ![Ühendatud auto - värvide muutmine](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4h.png)  

Klõpsake uue visualiseeringu lisada tühja ala.  

Valige **Rühmitatud tulpdiagrammi** visualiseeringu visualiseeringuid, lohistage väli **linn** **telje** alal **legendi** ala **mudel** ja **vin** **väärtus** ala.  
    ![Ühendatud auto - kobartulpdiagramm](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4i.png)  
    ![Ühendatud auto - renderdamiseks](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4j.png)
  
Sellel lehel kõik visualiseeringu ümber korraldada, nagu on näidatud joonisel.  
    ![Ühendatud auto - visualiseeringud](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4k.png)

"Sõidukite toiminguga" reaalajas aruande konfigureeritud. Jätkake järgmise reaalajas aruande loomine või peatada siin ja konfigureerida armatuurlaud. 

### <a name="2-vehicles-requiring-maintenance"></a>2 sõidukid hooldustööd
  
Klõpsake ![lisamine](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) lisamiseks uue aruande, nimetage see ümber **"sõidukite nõudva hooldus"**

![Ühendatud auto - sõidukid hooldustööd](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4l.png)  

Valige **vin** väli ja visualiseeringu tüübi **kaart**.  
    ![Ühendatud auto - Vin Kaardivisualiseering](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4m.png)  

Meil on nimega "MaintenanceLabel" andmekomplekti välja. Väli võib olla väärtus "0" või "1"." See on seatud Azure seadme õ mudeli lahenduse osana ette valmistatud ja integreeritud reaalajas tee. Väärtuse "1" näitab sõiduki nõuab hooldustööd. 

**Lehe tasemel** filtri jaoks nähtaval sõidukite andmeid, mis nõuavad hooldustööd lisamiseks tehke järgmist. 

1. Lohistage väli **"MaintenanceLabel"** **Lehe tasemel filtrid**.  
![Ühendatud auto - lehe tasemel filtrid](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n1.png)  

2. **Elementaarne filtreerimine** menüü Esita MaintenanceLabel lehe tasemel filtri allosas nuppu.  
![Ühendatud auto - Elementaarne filtreerimine](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n2.png)  

3.  Määrake selle filtri väärtuseks **"1"**    
![Ühendatud auto - Filter väärtus](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n3.png)  


Klõpsake uue visualiseeringu lisada tühja ala.  

Valige **Rühmitatud tulpdiagrammi** visualiseeringud  
![Ühendatud auto - Vind Kaardivisualiseering](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4o.png)  
![Ühendatud auto - kobartulpdiagramm](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4p.png)

Lohistage väli **mudeli** **Vin** **väärtus** alale **telje** alale. Seejärel sortida visualiseeringu **vin arvu**järgi.  Muuta diagrammi **tiitel** **"Hooldustööd mudeli sõidukid"**  

**Vin** väljad lohistada **Värviküllastus** juures **väljad** ![väljade](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4field.png) **visualiseeringu** vahekaart  
![Ühendatud auto - Värviküllastus](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4q.png)  

Visualiseeringutes **Vorming** jaotises **Andmete värvide** muutmine  
Miinimum värv, mida soovite muuta: **F2C812**  
Maksimum värv, mida soovite muuta: **FF6300**  
![Ühendatud auto - värvi muudatused](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4r.png)  
![Ühendatud auto - uue visualiseeringu värve](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4s.png)  

Klõpsake uue visualiseeringu lisada tühja ala.  

Valige **klaster tulpdiagrammi** visualiseeringuid, **vin** välja **väärtus** alale lohistada, **telje** alale **linn** välja lohistada. Sortige diagramm **"Count vin"**. Muuta diagrammi **tiitel** **"Sõidukid hooldustööd linnade järgi"**   
![Ühendatud auto - sõidukid hooldustööd linna järgi](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4t.png)  

Klõpsake uue visualiseeringu lisada tühja ala.  

**Mitme rea** kaardivisualiseering valida visualiseeringuid, **mudel** ja **vin** **väljad** alale lohistada.  
![Ühendatud auto - mitme rea kaart](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4u.png)    

Kõik visualiseeringu ümberkorraldamine, lõpparuanne näeb välja järgmine:  
![Ühendatud auto - mitme rea kaart](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4v.png)  

"Sõidukite nõudva hooldus" reaalajas aruande konfigureeritud. Jätkake järgmise reaalajas aruande loomine või peatada siin ja konfigureerida armatuurlaud. 

### <a name="3-vehicles-health-statistics"></a>3. sõidukite seisund statistika
  
Klõpsake ![lisamine](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) lisamiseks uue aruande, nimetage see ümber **"sõidukite seisund statistika"**  

Valige **g** visualiseeringu visualiseeringud ja seejärel lohistage **kiiruse** väljal **väärtus, väikseim väärtus, suurima väärtuse** alaks.  
![Ühendatud auto - mitme rea kaart](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4w.png)  

**Keskmine** **kiiruse** **väärtus** ala vaikimisi liitmise muutmine 

Vaikimisi liitmise **miinimumväärtus** **väikseim ala** **kiiruse** muutmine

Muuta vaikimisi liitmise **kiiruse** alas **maksimum** **suurim lubatud**

![Ühendatud auto - mitme rea kaart](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4x.png)  

Nimeta **G pealkiri** **"Keskmine kiirus"** 
 
![Ühendatud auto - g](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4y.png)  

Klõpsake uue visualiseeringu lisada tühja ala.  

Samuti lisada **g** **Keskmine engine oil**, **Keskmine kütuse**ja **Keskmine mootori mõõdukas**.  

Muuda vaikimisi liitmise väljad iga g vastavalt kohal etappide **"Keskmine kiirus"** mõõta.

![Ühendatud auto - mõõtmed](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4z.png)

Klõpsake uue visualiseeringu lisada tühja ala.

Valige **rea- ja rühmitatud tulpdiagrammi** visualiseeringuid, siis lohistage väli **linn** üheks **Ühiskasutuses telg**ja lohistage **kiirust**, **tirepressure ja engineoil väljad** alale **Väärtused veerus** , muuta nende koondamine tüüp **Keskmine**. 

Lohistage väli **engineTemperature** alale **Rea väärtused** , koondamine tüübiks valida **Keskmine**. 

![Ühendatud auto - visualiseeringute väljad](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4aa.png)

**"Keskmine kiirus, rõhk, engine oil ja mootori temperatuur"**muuta diagrammi **tiitel** .  

![Ühendatud auto - visualiseeringute väljad](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4bb.png)

Klõpsake uue visualiseeringu lisada tühja ala.

Valige **puukaart** visualiseeringu visualiseeringud, lohistage väli **mudeli** **rühma** alale ja lohistage väli **MaintenanceProbability** alale **väärtused** .

Muuta diagrammi **tiitel** **"Sõidukimudelite nõudva hooldustööd"**.

![Ühendatud auto - Muuda Diagrammi tiitel](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4cc.png)

Klõpsake uue visualiseeringu lisada tühja ala.

Valige visualiseering **100% virnlintdiagramm riba diagrammi** , lohistage välja **linn** alale **telg** ja lohistage **MaintenanceProbability** **RecallProbability** väljad alale **väärtus** .

![Ühendatud auto - lisada uue visualiseeringu](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4dd.png)

Klõpsake menüüd **Vorming**, **Andmete värvide**valimine ja **MaintenanceProbability** värvi määramine väärtuse **"F2C80F"**.

Muuta diagrammi **tiitel** **"tõenäosuse, sõiduki hooldus & tagasi kutsuda, City"**.

![Ühendatud auto - lisada uue visualiseeringu](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ee.png)

Klõpsake uue visualiseeringu lisada tühja ala.

Valige **Diagrammis** visualiseeringu visualiseeringud:, lohistage väli **mudeli** **telje** alale ja lohistage **engineOil, tirepressure ja kiiruse MaintenanceProbability** väljad alale **väärtused** . Muuta oma koondamine tüüp **"Keskmine"**. 

![Ühendatud auto - koondamine tüübi muutmine](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ff.png)

**"Keskmine engine oil kinnitus, rõhu, kiiruse ja hooldus tõenäosuse mudeli"**muuta diagrammi tiitel.

![Ühendatud auto - Muuda Diagrammi tiitel](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4gg.png)

Klõpsake uue visualiseeringu lisada tühja ala.

1. Valige **Punktdiagrammis** visualiseeringu visualiseeringuid.
2. Lohistage väli **mudeli** alale **üksikasjad** ja **Legend** .
3. Lohistage väli **kütuse** **x-telje** alale, asendage liitmise **Keskmine**.
4. Lohistage **engineTemparature** **y-telge**alale, **Keskmine** liitmise muutmine
5. Lohistage väli **vin** alale **maht** .


![Ühendatud auto - lisada uue visualiseeringu](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4hh.png)

Muuta diagrammi **tiitel** **"Keskmiste kütuse, mootori temperatuur mudeli"**.

![Ühendatud auto - Muuda Diagrammi tiitel](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ii.png)

Lõpparuanne näeb nagu allpool näidatud.

![Ühendatud auto lõpparuanne](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4jj.png)

### <a name="pin-visualizations-from-the-reports-to-the-real-time-dashboard"></a>PIN-koodi visualiseeringute reaalajas armatuurlaua aruannetes
  
Tühi armatuurlaua klõpsates plussmärgiikooni kõrval armatuurlaudade loomine. Saate nimi "Sõiduki telemeetria Analytics armatuurlaud"

![Ühendatud auto-armatuurlaud](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.5.png)

PIN-koodi visualiseeringu ülaltoodud aruanded armatuurlaud. 
 
![Ühendatud auto-armatuurlaud](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.6.png)

Armatuurlaua peaks välja järgmiselt, kui luuakse kolme aruanded ja vastavate visualiseeringud on kinnitatud armatuurlaud. Kui te pole loonud kõik aruanded, armatuurlauale välja teistsugune. 

![Ühendatud auto-armatuurlaud](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-4.0.png)

Palju õnne! Olete loonud reaalajas armatuurlaud. Kui jätkate käivitada CarEventGenerator.exe ja RealtimeDashboardApp.exe, peaksite värskendustega armatuurlaual. Kulub umbes 10 – 15 minutit, et täitke järgmised juhised.

 
##  <a name="setup-power-bi-batch-processing-dashboard"></a>Power BI paketi töötlemine armatuurlaua häälestamine

>[AZURE.NOTE] Kulub umbes kaks tundi (juurutamise edukaks) lõpuni Pakktöötlus müügivõimaluste valmis täitmise ja aasta väärt loodud andmete töötlemiseks. Nii oodake enne jätkamist järgmiste juhiste lõpuleviimiseks töötlemiseks. 

**PowerBI kujundaja faili alla laadida**

-   Eelkonfigureeritud PowerBI kujundaja fail on juurutamise kaasatud
-   Klõpsake diagrammi vaate PowerBI sõlme ja klõpsake linki **PowerBI kujundaja faili alla laadida** , klõpsake paanil atribuudid![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/9.5-download-powerbi-designer.png)

-   Kohalik salvestamine

**PowerBI aruannete konfigureerimine**

-   Avage designer fail 'VehicleTelemetryAnalytics – töölaual Report.pbix' PowerBI töölauarakenduses. Kui te pole juba, installige PowerBI töölaua [PowerBI töölaua installimine](http://www.microsoft.com/download/details.aspx?id=45331). 

-   Klõpsake nuppu **Redigeeri päringud**.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/10-edit-powerbi-query.png)

- Topeltklõpsake **Allikas**.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/11-set-powerbi-source.png)

- Värskendage serveris ühendusstring Azure SQL-i server, mis on saanud juurutamise käigus ette valmistatud. Klõpsake diagrammil sõlme Azure SQL-i ja vaadata serveri nime paanil atribuudid.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/11.5-view-server-name.png)

- Jäta **andmebaasi** *connectedcar*.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/12-set-powerbi-database.png)

- Klõpsake nuppu **OK**.
- Kuvatakse **Windowsi mandaati** menüüs valitud vaikimisi, muuta andmebaasi **identimisteabe** klõpsates **andmebaasi** vahekaardil paremal.
- Sisestage **kasutajanimi** ja **parool** selle juurutamise häälestamise ajal määratud Azure SQL-i andmebaasi.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/13-provide-database-credentials.png)

- Klõpsake **ühenduse loomine**
- Korrake ülaltoodud juhiseid iga kolme ülejäänud suhtes kohal parempoolsel paanil ja seejärel värskendage andmeallika ühendus üksikasjad.
- Klõpsake nuppu **Sule ja laadi**. Power BI Desktopi faili andmekomplektide on ühendatud SQL Azure'i andmebaasi tabelid.
- **Sulgemine** Power BI Desktopi fail.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/14-close-powerbi-desktop.png)

- Klõpsake muudatuste salvestamiseks nuppu **Salvesta** . 
 
Nüüd olete konfigureerinud kõik aruanded vastav paketi töötlemine tee lahendus. 


## <a name="upload-to-powerbicom"></a>Laadige *powerbi.com lehe kaudu*
 
1.  Liikuge http://powerbi.com ja logige sisse veebilehel PowerBI veebiportaali.
2.  Klõpsake **andmete hankimine**  
3.  Power BI Desktopi faili üles laadida.  
4.  Üles laadida, klõpsake **toomine -> Saada failid -> kohalik fail**  
5.  Liikuge **"VehicleTelemetryAnalytics – töölaual Report.pbix"**  
6.  Kui fail on üles laaditud, kas te viibite, tagasi, et oma Power BI töö ruumi.  

Teie jaoks luuakse andmekomplekt, aruande- ja tühi armatuurlaud.  
 

Olemasoleva **Power BI** **Sõiduki telemeetria Analytics armatuurlaud** armatuurlaud kinnitamine diagrammid. Valige tühi armatuurlaud, mis on loodud kohale ja liikuge **aruannete** lõik nuppu üleslaaditud aruanne.  

![Sõiduki telemeetria powerbi.com lehe kaudu](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard1.png) 


**Pange tähele, et aruanne on kuus lehekülge:**  
Lehe 1: Sõiduki tihedusfunktsiooni  
Lehe 2: Reaalajas sõiduki seisund  
Lehe 3: Agressiivselt juhitud sõidukite   
Lk 4: Meelde tuletada sõidukite  
Lehe 5: Kütuse tõhus sõidukid  
Lehe 6: Contoso Logo  

![Ühendatud auto powerbi.com lehe kaudu](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard2.png)
 

**Lehe 3**, PIN-koodi järgmist:  

1.  VIN arv  
    ![Ühendatud auto powerbi.com lehe kaudu](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard3.png) 

2.  Sõidukite agressiivselt juhitud mudeli – kaskaaddiagrammi  
    ![Sõiduki telemeetria - PIN-koodi diagrammide 4](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard4.png)

**Lehe 5**, PIN-koodi järgmist: 
 
1.  Vin arv    
    ![Sõiduki telemeetria - PIN-koodi diagrammide 5](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard5.png)  
2.  Lennukikütuse sõidukeid mudeli: kobartulpdiagramm  
    ![Sõiduki telemeetria - PIN-koodi diagrammide 6](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard6.png)

**Lk 4**, PIN-koodi järgmist:  

1.  Vin arv  
    ![Sõiduki telemeetria - PIN-koodi diagrammide 7](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard7.png) 

2.  Tagasikutsutud võrra linna mudel: puukaart  
    ![Sõiduki telemeetria - PIN-koodi diagrammide 8](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard8.png)  

**Lehe 6**, PIN-koodi järgmist:  

1.  Contoso mootorite logo  
    ![Sõiduki telemeetria - PIN-koodi diagrammide 9](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard9.png)

**Armatuurlaua korraldamine**  

1.  Liikuge armatuurlauale.
2.  Viige kursor iga diagrammi ja selle aluseks valmis armatuurlaud alloleval pildil on esitatud nimetamine Nimeta. Ka liikuda diagrammide armatuurlaud allpool näeb välja.  
    ![Sõiduki telemeetria - armatuurlaua 2 korraldamine](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-organize-dashboard2.png)  
    ![Sõiduki telemeetria powerbi.com lehe kaudu](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard.png)
3.  Kui olete loonud kõik aruanded nagu selles dokumendis, lõplik valmis saanud armatuurlaua peaks välja nägema järgmine joonis. 

![Sõiduki telemeetria - armatuurlaua 2 korraldamine](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-organize-dashboard3.png)

Palju õnne! Olete loonud aruannete ja saada reaalajas, ennustava ja partii ülevaateid sõiduki seisundi ja juhtimise armatuurlaud harjumused.  
