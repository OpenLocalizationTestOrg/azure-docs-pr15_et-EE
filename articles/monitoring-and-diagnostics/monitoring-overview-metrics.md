<properties
    pageTitle="Ülevaade: Microsoft Azure mõõdikute | Microsoft Azure'i"
    description="Mõõdikute ja nende kasutab Microsoft Azure ülevaade"
    authors="kamathashwin"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="ashwink"/>

# <a name="overview-of-metrics-in-microsoft-azure"></a>Microsoft Azure mõõdikute ülevaade 

Selles artiklis kirjeldatakse, mida mõõdikud Microsoft Azure, on oma eelised ja nende kasutamisega alustamise kohta.  

## <a name="what-are-metrics"></a>Mis on mõõdikute?

Azure'i jälgimine võimaldab teil saada nähtavus jõudlus ja teie töökoormus Azure seisundi telemeetria tarbimine. Azure'i telemeetria kõige olulisemad andmetüüp on mõõdikute (nimetatakse ka jõudluse hinnale) kiiratava kõige Azure ressursid. Azure'i kuvar on mitu võimalust ja konfigureerimiseks peab olema nende mõõdikute jälgimine ja tõrkeotsingu jaoks.


## <a name="what-can-you-do-with-metrics"></a>Mida saab teha mõõdikute?

Mõõdikute on väärtuslik allikas telemeetria ja võimaldavad teil teha järgmist:

- Oma ressursi (nt VM, veebisaidi või rakenduse loogika) joonestamist selle mõõdikute portaali diagrammi ja selle diagrammi lisamine armatuurlauale kinnitamine **jälituse jõudlus** .
- **Teatise saamine kindla probleemi** oma ressursi jõudlust mõjutavad kui mõõdiku ületab teatud ülemmäära.
- **Automaatne konfigureerimine toimingud**, nt autoscaling ressursi või süütamise on käitusjuhendi kui mõõdiku ületab teatud ülemmäära.
- **Täpsemad Kasutusanalüüsi täita** või jõudluse ega kasutus trende ressurss oma kohta.
- Ressursi **jaoks vastavuse/audit** eesmärgil jõudluse või seisundi ajaloo **arhiivi** .

##  <a name="metric-characteristics"></a>Argumendil omadused
Mõõdikute olla järgmised omadused:

- Kõik mõõdikud on **1-minutilise sagedus**. Saate argumendil väärtus iga minut oma ressurssi, võimaldades reaalajas nähtavus riigi- ja oma ressurssi seisundi lähedal.
- Mõõdikud on **saadaval kontorist-of-box vajamata nõustumiseks** või häälestamise täiendavad diagnostika.
- Pääsete iga mõõdiku **30 päeva ajalugu** . Saate kiiresti vaadata tehtud ja kuu trendide jõudluse või seisundi oma ressursi.

Sa saad:

- Lihtne leida, access ja **vaadata kõik mõõdikute** valimisel ressursi ja joonistada neid diagrammil Azure portaali kaudu. 
- Konfigureerida argumendil **reegli, mis saadab valitud isikule teatise või automatiseeritud toiming võtab** kui mõõdiku ületab läve seadistusi. Autoscale on teisiti automatiseeritud toiming, mis võimaldab teil oma ressurssi, sissetulevad taotlused või laadite oma veebisaidil või arvutada ressursid välja skaala. Saate konfigureerida mastaapimiseks välja/sisse säte reegli Autoscale piirmäära ületavate mõõdiku põhjal.
- **Arhiivi** mõõdikute kauem või neid ühenduseta aruannete jaoks kasutada. Saate marsruutida oma mõõdikute Bloobivahemälu salvestusruumi oma ressursi diagnostika sätete konfigureerimisel.
- **Voo** mõõdikute sündmuse jaoturiga, mis võimaldab teil neid Azure'i voo Analytics või kohandatud rakendused lähedal reaalajas analüüsi jaoks marsruutida. Funktsiooni abil saate teha diagnostikasätete.
- **Marsruutimiseks** kõik mõõdikud, et OMS Log Analytics avada kiirsõnumside analytics, otsingu ja kohandatud teavitamine mõõdikute andmete põhjal oma ressursse.
- **Tarbida** mõõdikute uue Azure kuvari REST API-de kaudu.
- **Päringu** mõõdikute platvormidel REST API-ga või PowerShelli cmdlet-käskude abil.

 ![Marsruutimise mõõdikute Azure monitoris.](./media/monitoring-overview-metrics/MetricsOverview0.png)

## <a name="access-metrics-via-portal"></a>Accessi mõõdikute portaali kaudu
Siit leiate Azure'i portaali kaudu argumendil diagrammi loomise kiirülevaate lühiülevaade

### <a name="view-metrics-after-creating-a-resource"></a>Vaate mõõdikute ressursi loomise järel
1. Avatud Azure'i portaal
2. Saate luua uue rakenduse teenuse - veebisaidi.
3. Pärast veebisaidi loomist avage veebisait tera ülevaade.
4. Saate vaadata uute mõõdikute paanina "Jälgimine". Saate redigeerida paani ja valige veel mõõdikud

 ![Klõpsake Azure kuvari ressurssi mõõdikud](./media/monitoring-overview-metrics/MetricsOverview1.png)    

### <a name="access-all-metrics-in-a-single-place"></a>Juurdepääs kõik mõõdikud ühes kohas
1. Avage Azure'i portaal 
2. Liikuge menüü Uus kuvar ja all suvand mõõdikud 
3. Saate tellimuse, ressursirühm ja ressursi nimi kuvatakse ripploendis. 
4. Nüüd saate vaadata saadaval mõõdikute loend. 
5. Valige meetermõõdustik olete huvitatud ja selle joonistada. 
6. Saate kinnitada see armatuurlaud klõpsates PIN-koodi, klõpsake paremas ülanurgas.

 ![Juurdepääs kõik mõõdikud Azure'i jälgida ühes kohas](./media/monitoring-overview-metrics/MetricsOverview2.png) 


>[AZURE.NOTE] Pääsete ilma mis tahes täiendavaid diagnostika häälestamise host taseme mõõdikute kaudu VMs (Azure'i ressursihaldur vastavalt) ja VM skaala komplektid. Need uue host taseme mõõdikud on saadaval Windows ja Linux eksemplarid. Need mõõdikud ei ole teil on juurdepääs Azure'i diagnostika VMs või VMSS sisselülitamisel Külastajate-OS taseme mõõdikute segi. Azure'i diagnostika konfigureerimise kohta leiate lisateavet teemast [mis on Microsoft Azure'i diagnostika](../azure-diagnostics.md).

## <a name="access-metrics-via-rest-api"></a>Accessi mõõdikute REST API kaudu
Azure'i mõõdikute pääseb Azure'i kuvari API-d. On kaks API-d, mis aitavad teil tuvastada ja mõõdikute juurde. Kasutage funktsiooni: 

- [Azure'i kuvari meetermõõdustik määratlused REST API](https://msdn.microsoft.com/library/mt743621.aspx) mõõdikute teenuse jaoks saadaval loendi.
- [Azure'i kuvari mõõdikute REST API](https://msdn.microsoft.com/library/mt743622.aspx) tegelik mõõdikute andmetele juurdepääsuks

>[AZURE.NOTE] Selles artiklis antakse ülevaade mõõdikute kaudu [Uus API mõõdikute](https://msdn.microsoft.com/library/dn931930.aspx) Azure ressursse. Uue argumendil määratlusi API API versioon on 2016-03-01 ja mõõdikute API versioon on 2016-09-01. Pärand argumendil määratlused ja mõõdikute pääseb api-versiooni 2014-04-01.

Veel üksikasjaliku selgituse abil Azure'i kuvari REST API-d, leiate [Azure'i kuvari REST API kiirtutvustus](monitoring-rest-api-walkthrough.md).

## <a name="export-options-for-metrics"></a>Mõõdikute valikuid eksport
Saate minna diagnostika logid tera vahekaardil kuvar ja mõõdikute Vaatesuvandid ekspordi. Selles artiklis eelnevalt mainitud kasutamise-juhtudel saate valida mõõdikute (ja diagnostikalogid) marsruutida bloobimälu, sündmuse jaoturi või OMS Log Analytics. 

 ![Azure'i monitoris mõõdikute valikuid eksport](./media/monitoring-overview-metrics/MetricsOverview3.png)   

Saate konfigureerida selle ressursihaldur Mallid [PowerShelli](insights-powershell-samples.md), [CLI](insights-cli-samples.md) või [REST API-de](https://msdn.microsoft.com/library/dn931943.aspx)kaudu. 

## <a name="take-action-on-metrics"></a>Tegutseda mõõdikud
Võite saada teatisi või argumendil andmete automatiseeritud toimingute tegemiseks. Saate konfigureerida teatiste reeglid või Autoscale sätteid nii, et seda teha.

### <a name="alert-rules"></a>Teatiste reeglid
Saate konfigureerida teatiste reeglid mõõdikute kohta. Teatiste reegleid saate kontrollida, kui mõõdiku ületanud teatava piiri ja märku meili teel või tule webhook, mida saab kasutada siis mis tahes kohandatud skript käivitada. Saate selle webhook toode 3 integratsioon konfigureerimiseks.

 ![Mõõdikute ja Azure jälgimine teatiste reeglite](./media/monitoring-overview-metrics/MetricsOverview4.png)

### <a name="autoscale"></a>Autoscale
Mõned Azure ressursid toetavad mitmes eksemplaris, või mastaapimiseks käsitlema oma töökoormus. Autoscale kehtib rakenduse teenused (Web apps), virtuaalse masina skaala komplektid (VMSS) ja klassikaline pilveteenustega. Saate konfigureerida mastaapida, või kui teatud mõõdiku mõjutavad oma töökoormus ületab läve teie määratud autoscale reeglid. Lisateavet leiate teemast [Ülevaade autoscaling](monitoring-overview-autoscale.md).

 ![Mõõdikute ja Azure monitoris autoscale](./media/monitoring-overview-metrics/MetricsOverview5.png)

## <a name="supported-services-and-metrics"></a>Toetatud teenused ja mõõdikud
Azure'i kuvar on uue mõõdikute taristu. See toetab Azure teenuse Azure portaali ja Azure kuvari API uus versioon:

- VMs (Azure'i ressursihaldur põhine)
- VM skaala komplektid
- Paketi
- Sündmuse jaoturi nimeruum 
- Teenuse siini nimeruum (ainult premium SKU-ga)
- SQL-i (versioon 12)
- Elastne SQL-i Pool
- Veebilehtede
- Web Server parkides
- Loogika rakendused
- Asjade jaoturi
- Redis vahemälu
- Võrgunduse: Rakenduse lüüsid
- Otsing

Saate vaadata ka üksikasjalik loend toetatud teenuste ja nende mõõdikud veebisaidil [Azure'i kuvari mõõdikud – toetatud mõõdikute ressursi liigi kohta](monitoring-supported-metrics.md). 


## <a name="next-steps"></a>Järgmised sammud

Vaadake artiklit linkides. Lisaks tutvuda.  

- [levinud mõõdikud autoscaling](insights-autoscale-common-metrics.md) kohta
- Kuidas [teatiste reeglite loomine](insights-alerts-portal.md)




