<properties
    pageTitle="Ülevaade Microsoft Azure jälgimine | Microsoft Azure'i"
    description="Microsoft Azure teatised, webhooks, autoscale jm diagnostika-ja ülemise taseme ülevaade."
    authors="rboucher"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"l
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/11/2016"
    ms.author="robb"/>

# <a name="overview-of-monitoring-in-microsoft-azure"></a>Ülevaade Microsoft Azure jälgimine

Selles artiklis antakse kontseptuaalne ülevaade jälgimise Azure ressursse. Pakub ressursse kindlat tüüpi teabe viitu.  Üksikasjalik teave jälgimine vt rakenduse-Azure'i seisukohast, [jälgimine ja diagnostika juhised](../best-practices-monitoring.md).

Keerukate, paljude liikuvad osad on pilve rakendused. Jälgimine pakub andmed, et tagada, et teie taotlus jääb üles ja töötab terve olekus. Lisaks aitab hoida ära võimalikke probleeme või varasemaid tõrkeotsing. Lisaks saate andmeid saada sügav ülevaateid rakenduse kohta. Teadmisi abil saate parandada jõudlust rakenduse või hooldatavust või toiminguid, mis muidu nõuaks manuaalset automatiseerida.

Järgmisel joonisel on esitatud asumist Azure jälgimine, sh logid saate koguda ja mida saate teha seda andmete tüüpi.   

![Loogilise mudeli jälgimiseks ja diagnostika-Arvuta ressursid](./media/monitoring-overview/MonitoringAzureResources-non-compute_v3.png)

Joonis 1: Kontseptuaalne mudel jälgimiseks ja diagnostika-Arvuta ressursid

<br/>

![Arvuta ressursid diagnostika-ja loogikaväärtused mudel](./media/monitoring-overview/MonitoringAzureResources-compute_v3.png)

Joonis 2: Kontseptuaalne mudel Arvuta ressursid diagnostika-ja


## <a name="monitoring-sources"></a>Allikate jälgimine
### <a name="activity-logs"></a>Logid
Tegevuste Logi (varem nimetati töökorras või auditilogid) saate otsida teavet oma ressursi, mida näevad Azure'i infrastruktuuri kohta. Logi sisaldab andmeid, nt korda, kui ressursid on loodud või hävitatud.  

### <a name="host-vm"></a>Host VM
**Arvuta ainult**


Mõned ressursid pilveteenustega, Virtuaalmasinates, nt arvutada ja teenuse struktuuri on spetsiaalne Host VM nad suhelda. Host VM on samaväärne Root VM Hyper-V hypervisor mudel. Sel juhul saate koguda mõõdikute lihtsalt Host VM Lisaks Külastajate OS.  

Muud Azure'i teenuste jaoks pole tingimata 1:1 vastendamine oma ressursside ja kindla hosti VM vahel nii, et host VM mõõdikute pole saadaval.


### <a name="resource---metrics-and-diagnostics-logs"></a>Allika - mõõdikute ja diagnostika logid
Sissenõutavaks mõõdikute sõltuvad ressursi tüüp. Näiteks Virtuaalmasinates annab statistika kettale IO ja protsenti CPU. Kuid nende statistika pole olemas teenuse siini järjekorra, mis pakub hoopis mõõdikute, näiteks järjekorra suurus ja sõnumi läbilaskevõime.

Arvuta ressursid saate hankida mõõdikute Külastajate OS ja diagnostika moodulid nagu Azure'i diagnostika. Azure'i diagnostika aitab kogumine ja marsruutimine kogumine Diagnostikaandmete muudesse asukohtadesse, sh Azure salvestusruumi.

Praegu sissenõutavaks mõõdikute loend on saadaval veebisaidil [mõõdikud toetatud](monitoring-supported-metrics.md).

### <a name="application---diagnostics-logs-application-logs-and-metrics"></a>Rakenduse - diagnostika logid, logid ja mõõdikud
**Arvuta ainult**

Rakenduste saab käitada Külastajate OS Arvuta mudeli peal. Need eraldavad oma logid ja mõõdikute kogum.

Mõõdikute tüübid

- Jõudluse hinnale
- Logid
- Windowsi sündmuste logid
- .NET Sündmuse allikas
- IIS-i logid
- Näidata vastavalt ETW
- Puistab ootamatult sulguda
- Kliendi Tõrkelogide


## <a name="uses-for-monitoring-data"></a>Andmete jälgimine kasutusalad

### <a name="visualize"></a>Visualiseerimine
Graafika ja tabelitega jälgimisega seotud andmete visualiseerimine aitab leida trendide palju kiiremini kui otsides andmed ise.  

Visualiseeringu paar meetodit on järgmised.

- Azure portaali kasutamine
- Marsruudi andmete Azure'i rakenduse ülevaated
- Microsoft PowerBI marsruudi andmete
- Andmete marsruutimine visualiseeringu kolmanda osapoole tööriista, kasutades kas reaalajas streaming või lasta lugeda arhiivi Azure storage tööriist

### <a name="archive"></a>Arhiivimine
Andmete on tavaliselt kirjutatud Azure salvestusruumi ja seal hoida, kuni need kustutada.

Kasutage andmed on mitu võimalust:

- Kui kirjutada, saate määrata muud tööriistad või Azure väljaspool seda lugeda ja töödelda.
- Laadige kohalikult kohaliku arhiivi andmeid või muuta oma säilituspoliitika, et hoida andmeid pikema perioodi.  
- Jätate andmed Azure laos lõputult, kui teil on eest Azure hoiate andmehulga põhjal.
-

### <a name="query"></a>Päringu
Azure'i kuvari REST API rist platvormi käsurea liides (CLI) käsud, PowerShelli cmdlet-käskude või .NET SDK abil saate andmed süsteemi või Azure salvestusruumi juurde

Näited:

-  Kohandatud jälgimise rakendus, mida olete kirjutanud andmete hankimine
-  Kohandatud päringute loomise ja saatmise andmed kolmanda osapoole rakendus.

### <a name="route"></a>Marsruutimiseks
Voogesituseks jälgimisega seotud andmete reaalajas muudesse asukohtadesse.

Näited:

- Saatke rakenduse ülevaated abil saate visualiseerimise tööriistad olemas.
- Saatke sündmuse jaoturi nii, et te saate marsruutida kolmanda osapoole tööriistu reaalajas analüüsi sooritamiseks.

### <a name="automate"></a>Automatiseerimine
Saate kasutada päästik sündmuste jälgimisega seotud andmed või isegi kogu protsesside näited:

- Kasutamine andmete autoscale arvutada eksemplari, üles või alla rakenduse laadi põhjal.
- Saada e-kirju mõõdiku ületab läve lülitame.
- Kõne toimingu käivitada süsteemi väljaspool Azure web URL (webhook)
- Alustamiseks on käitusjuhendi sordi ülesannete täitmiseks Azure automatiseerimine

## <a name="methods-of-use"></a>Kasutamise viisid
Üldiselt saab töödelda andmete jälgimine, marsruutimist ja andmetoomisteenused, kasutades ühte järgmistest. Kõik meetodid on saadaval kõigi toimingute või andmetüübid.

- [Azure'i portaal](https://portal.azure.com)
- [PowerShelli](insights-powershell-samples.md)  
- [Mitu platvormi käsurea liides (CLI)](insights-cli-samples.md)
- [REST API-GA](https://msdn.microsoft.com/library/dn931943.aspx)
- [.NET SDK](https://msdn.microsoft.com/library/dn802153.aspx)

## <a name="azures-monitoring-offerings"></a>Azure pakkumisi jälgimine
Azure'i on saadaval jälgida oma rakenduse telemeetria tühjal metallist taristu teenuseid pakkumisi. Kõige paremini jälgimise strateegia ühendab kasutamine kõigi kolme teenuste seisundi täielik, üksikasjaliku ülevaate saada.

- [Azure'i kuvari](http://aka.ms/azmondocs) – pakutakse visualiseerimise, päringu, marsruutimine, teavitamine, autoscale ja automatiseerimise andmete nii Azure'i infrastruktuuri (tegevuste Log) ja iga üksiku Azure ressursi (Diagnostikalogid). See artikkel on osa dokumendist Azure'i jälgimine. Azure'i kuvari nimi ilmus mai 25 Ignite 2016.  Eelmise nimi oli "Azure ülevaated."  
- [Rakenduse ülevaated](https://azure.microsoft.com/documentation/services/application-insights/) – sisaldab rikkaliku tuvastamise ja diagnostika probleemide rakenduse kiht oma teenuse hästi integreeritud peal andmeid Azure'i jälgimine. See on vaikimisi diagnostika platvormi rakenduse teenuse Web Apps.  Saate marsruutida sellele muude teenuste andmeid.  
- [Log Analytics](https://azure.microsoft.com/documentation/services/log-analytics/) osa [Operations Management Suite](https://www.microsoft.com/cloud-platform/operations-management-suite) – pakub terviklikuks IT-halduse lahendus nii kohapealse ja muude tootjate pilvepõhist infrastruktuuri (nt AWS) Lisaks Azure ressursse.  Azure'i kuvari andmeid marsruuditakse otse Log Analytics nii, et näete mõõdikute ja logid kogu keskkonna ühes kohas.     


## <a name="next-steps"></a>Järgmised sammud
Lisateave

- [Video: Ignite 2016 Azure jälgimine](https://myignite.microsoft.com/videos/4977)
- [Alustamine: Azure'i jälgimine](monitoring-get-started.md)
- Kui proovite oma pilveteenuses, virtuaalse masina või teenuse struktuuri rakenduse probleemide väljaselgitamiseks [Azure'i diagnostika](../azure-diagnostics.md) .
- Kui proovite diagnostika probleeme oma rakenduse teenuse Web App [Rakenduse ülevaated](https://azure.microsoft.com/documentation/services/application-insights/) .
- [Tõrkeotsing Azure Storage](../storage/storage-e2e-troubleshooting.md) salvestusruumi plekid, tabelite või järjekorrad kasutamisel
- [Log analüüsi](https://azure.microsoft.com/documentation/services/log-analytics/) - ja [Operations Management Suite](https://www.microsoft.com/cloud-platform/operations-management-suite)
