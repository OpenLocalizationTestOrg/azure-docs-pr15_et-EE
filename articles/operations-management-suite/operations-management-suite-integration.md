<properties
   pageTitle="Integreerimise Operations Management Suite (OMS) | Microsoft Azure'i"
   description="Lisaks kasutades OMS standard funktsioone, saate integreerida muid rakendusi ja teenuseid pakkuda hübriid halduse keskkonnas, esitada stsenaariumi kordumatu keskkonna kohandatud või kohandatud halduse kogemus oma klientidele.  Selles artiklis antakse ülevaade OMS ja üksikasjalikku tehnilist teavet artiklite linke, kust integreerimiseks oma erinevaid võimalusi."
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="bwren" />

# <a name="integrating-with-operations-management-suite-oms"></a>Integreerimise Operations Management Suite (OMS)

Operations Management Suite on Microsofti pilvepõhise IT lahendus, mis aitab hallata ja kaitsta oma kohapealse ja pilveteenuse taristu.  Lisaks kasutades OMS standard funktsioone, saate integreerida muid rakendusi ja teenuseid pakkuda hübriid halduse keskkonnas, esitada stsenaariumi kordumatu keskkonna kohandatud või kohandatud halduse kogemus oma klientidele.  Selles artiklis antakse ülevaade OMS teenuste ja linke artiklitele üksikasjalikku tehnilist teavet integreerimiseks oma erinevaid võimalusi. 



## <a name="log-analytics"></a>Log Analytics
Kogutud Log Analytics andmete haldus talletatakse hoidlas, mis on majutatud Azure.  Kõik andmed, mis on talletatud hoidla on saadaval log otsingud, mis pakuvad kiiranalüüs väga suurte andmehulkade üle.  Oma integreerimine nõuetele võib hoidla kasutatavaks analüüsi uute andmetega asustada või ekstrakti andmed hoidlas uue visualiseeringu või mõne muu haldustööriista integreerida.

Iga tekstilõigu hoidla andmeid talletatakse kirjena.  Kui te asustamine hoidla, mida peaks andma kasutajate kirje tüüp, mis kasutab teie lahendus ja selle atribuutide kirjeldus.  Kui laadite andmeid, peate seda teavet andmete, millega töötate.

![Ilma vaevata luua OMS hoidla](media/operations-management-suite-integration/repository.png)


### <a name="populate-the-log-analytics-repository"></a>Log Analytics hoidla asustamiseks
On mitu võimalust ilma vaevata luua OMS hoidla.  Kasutatav meetod sõltub näiteks kus asub lähteandmed, andmed ja mis vormingu klientide soovite toetavad.  Kui andmed on salvestatud hoidla, pole vahet kuidas saadi.

Järgmistes jaotistes kirjeldatakse asustamiseks OMS hoidla erinevaid võimalusi.

#### <a name="connected-sources-and-data-sources"></a>Ühendatud andmeallikate ja andmeallikad 
Ühendatud allikad on kohad, kus saate andmeid tuua OMS hoidla jaoks.  Andmeallikate ja lahendused töötavad allikast ühendatud ja kogutud andmeid määratleda.  Kui rakenduse kirjutab andmed nende andmeallikat, siis võite koguda selle andmeallika konfigureerimisega.  Näiteks kui teie rakendus loob sündmuste logi, siis ta saab koguda Logi andmeallika Linux agent.

- [Log Analytics andmeallikad](../log-analytics/log-analytics-data-sources.md)

#### <a name="solutions"></a>Lahendused

Lahenduste laiendavad OMS.  Lahenduse võib ühendatud andmeallika andmete kogumine või see võib sooritada analüüsi kirjed, mis on juba kogutud hoidla.  Iga lahenduse, mida Microsoft on üksikud artikkel, mis sisaldab andmeid, mida ta kogub üksikasju.

- [Log Analytics lahendused](../log-analytics/log-analytics-add-solutions.md)



#### <a name="http-data-collector-api"></a>HTTP Andmekoguja API

Log Analytics HTTP Andmekoguja API on REST API, mis võimaldab teil JSON andmete lisamiseks Logi Analytics hoidla.  Kui teil on rakendus, mis ei paku ühte andmeallikate või lahenduste kaudu saate kasutada seda API-d.  Seda saab kasutada mis tahes klient, mida saate helistada API ja ei viita saidikogumi ajakava andmeallika või lahenduse hoidla asustamiseks.

- [Logige Analytics HTTP Andmekoguja API](../log-analytics/log-analytics-data-collector-api.md)


### <a name="retrieve-data-from-the-log-analytics-repository"></a>Andmete toomine Log Analytics hoidla

On mitu võimalust andmete toomine OMS hoidla.  Võib-olla soovite kasutajatele OMS konsooli abil andmete toomiseks ja saatke neile erinevate visualiseeringute ja analüüsi.  Andmeid saate alla laadida ka välise protsessist, näiteks mõne muu lahendus.

#### <a name="log-searches"></a>Log otsingud

Kõik andmed, mis on talletatud OMS hoidla on saadaval log otsingute kaudu.  Kasutajad võivad teha oma sihtotstarbelise analüüsi OMS konsooli või Armatuurlaua loomine visualiseeringuga teatud log otsingu jaoks.  Lahendused võivad sisaldada kohandatud vaateid visualiseeringutega eelmääratletud otsingute põhjal.  Saate Log otsingu API juurdepääsu andmetele OMS hoidlas välise rakenduse või halduse tööriista.  

- [Log otsinguid Log Analytics](../log-analytics/log-analytics-log-searches.md)
- [Log Analytics log search REST API-ga](../log-analytics/log-analytics-log-search-api.md)
- [Logige Analytics cmdlet-käsud](https://msdn.microsoft.com/library/mt188224.aspx)



#### <a name="custom-views"></a>Kohandatud vaated 
Vaate Designer võimaldab teil luua kohandatud vaateid anda kasutajatele visualiseerimine ja andmed teie lahendus analüüsi OMS konsooli.  Iga vaade sisaldab paan, mida kuvatakse lehel on konsooli ja mis tahes arv visualiseeringu osad, mis põhinevad log otsingupäringutest, millega määratlete.
  
- [Log Analytics Kuvakujundaja](../log-analytics/log-analytics-view-designer.md)


#### <a name="power-bi"></a>Power BI

Log Analytics saab automaatselt ekspordi andmed OMS hoidla Power BI nii, et saate kasutada oma visualiseerimine ja andmeanalüüsi tööriistad.  Tehtav selle ekspordi ajakava nii, et andmed on ajakohane. 

- [Power BI Log Analytics andmete eksportimine](../log-analytics/log-analytics-powerbi.md)




## <a name="automation"></a>Automatiseerimine

OMS saate automatiseerida protsesside reageerida kogutud andmete või muude ülesannete haldus.  See võib rakenduse andmete kogumine ja lisada OMS hoidla või teil võib automatiseerida teadaolevate probleemide vastuseks hoidla sisalduvate andmete parandamist. 

![Automatiseerimine](media/operations-management-suite-integration/automate.png)

### <a name="runbooks"></a>Tegevusraamatud

Azure'i automaatika tegevusraamatud käivitage PowerShelli skriptide ja töövoogude Azure pilveteenuses.  Saate nende ressursside Azure või muud ressursid, millele pääseb pilvest haldamiseks.  Tegevusraamatud saab käivitada ka kohaliku andmekeskuses, kasutades hübriid Käitusjuhendi töötaja.  Azure portaali või välise protsesside mitmel viisil nagu PowerShelli või automaatika API abil saate alustada on käitusjuhendi.

- [Alates on käitusjuhendi Azure automatiseerimine](../automation/automation-starting-a-runbook.md)
- [Azure'i automatiseerimine cmdlettide](https://msdn.microsoft.com/library/dn690262.aspx)
- [Automaatika REST API](https://msdn.microsoft.com/library/mt662285.aspx)
- [.NET-automatiseerimist.](https://msdn.microsoft.com//library/mt465763.aspx)

### <a name="alerts"></a>Teatised

Teatiste reeglid automaatselt käivitada log otsingupäringutest skeemi järgi.  Kui tulemused vastavad teatud kriteeriumide tulemuseks teatise alustamiseks on käitusjuhendi Azure automatiseerimine või kõne webhook, mis saate alustada mõni väline protsess.  Nii nende vastuste detailid saate teatise, sh log otsing tagastatavad andmed.

- [Log Analytics teatised](../log-analytics/log-analytics-alerts.md)
- [Log Analytics teatiste API](../log-analytics/log-analytics-api-alerts.md)


## <a name="backup-and-site-recovery"></a>Varundamine ja taastamine saidi

Azure'i varundamine ja taastamine saidi pakkuda teenuseid oma ettevõtte andmete kaitsmiseks ja serverite ja rakenduste olemasolu.  Saate kasutada nende teenuste sellised stsenaariumid varukoopia teenuste rakenduse või alustamist virtuaalse masina Tõrkesiirde sooritamiseks.

- [Azure'i varundus cmdlet-käsud](https://msdn.microsoft.com/library/mt619253.aspx)
- [Azure'i saidi taastamine REST API-ga](https://msdn.microsoft.com/library/azure/mt750497.aspx)
- [Azure'i saidi taastamise cmdlet-käsud](https://msdn.microsoft.com/library/mt637930.aspx)

## <a name="custom-solutions"></a>Kohandatud lahendused

Saate kapseldada integreerimine loogika, ühte lahendusse kohandatud käivitada oma tööruumis või kliendi tööruumis.  Teie lahendus võib sisaldada integreerimise meetodite artiklit lisaks muid ressursse, mis pakuvad täieliku haldamise stsenaarium.  Ressursside kohta lahendus on pakitud nii, et lahendus eemaldamisel lähevad kõik selle loodud ressursid eemaldatakse OMS tööruumi ja Azure tellimuse.

Näiteks teie lahendus võib hõlmata ka automatiseerimise käitusjuhendi kogumine ja töötlemine andmete ja seejärel asustamiseks Log Analytics hoidla HTTP andmete koguja API kasutamine.  Samuti võite kaasata kohandatud vaate, mis kujutab ja analüüsib kogutud andmeid.  

- Luues kohandatud lahendused (tulekul)    

## <a name="next-steps"></a>Järgmised sammud
- Viide [OMS SDK](operations-management-suite-sdk.md) tehnilist teavet OMS teenuste automatiseerimine.  
