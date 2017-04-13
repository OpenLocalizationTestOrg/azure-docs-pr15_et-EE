<properties
 pageTitle="Sõnastikupõhise hooldustööd kiirtutvustus | Microsoft Azure'i"
 description="Azure'i asjade sõnastikupõhise hooldustööd lühiülevaade eelkonfigureeritud lahendus."
 services=""
 suite="iot-suite"
 documentationCenter=""
 authors="aguilaaj"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-suite"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/17/2016"
 ms.author="araguila"/>

# <a name="predictive-maintenance-preconfigured-solution-walkthrough"></a>Eelkonfigureeritud sõnastikupõhise hooldustööd lahenduse kiirtutvustus

## <a name="introduction"></a>Sissejuhatus

Asjade komplekti sõnastikupõhise hooldustööd eelkonfigureeritud lahendus on business stsenaarium, mis prognoosib punkti kui tõrke on tõenäoline lõpuni lahendus. Saate selle eelkonfigureeritud lahendus aktiivselt näiteks optimeerimine hooldustööd. Lahendus ühendab võtme Azure'i asjade komplekti teenuste, sh on [Azure seadme õ] [ lnk_machine_learning] tööruum. Selle tööruumi sisaldab katsete põhjal prognoosida selle ülejäänud kasulik kestus (RUL) õhusõiduki mootori avaliku valimi andmehulgas. Lahendus rakendab täielikult asjade business stsenaarium saate kavandada ja rakendada lahenduse, mis vastab teie enda ettevõtte konkreetseid alguspunktina.

## <a name="logical-architecture"></a>Loogiline arhitektuur

Järgmine diagramm kirjeldab loogilise komponentide eelkonfigureeritud lahendus:

![][img-architecture]

Azure'i teenuseid, mis on ette valmistatud asukohas valite, kui olete ette eelkonfigureeritud lahendus on sinine üksused. Kas Ida-USA, Põhja-Euroopa või Ida-Aasia piirkonnas eelkonfigureeritud lahenduse saate ette valmistada.

Mõned ressursid ei ole saadaval regioonid, kuhu olete ette eelkonfigureeritud lahendus. Skeemi oranž üksuste tähistavad ette valmistatud lähima saadaval piirkonna (Lõuna keskse meile, Euroopa Lääne või Lõuna-Aasia) antud valitud piirkonna Azure teenuseid.

Roheline üksus on jäljendatud seadet, mis tähistab mõne neljasilindriline. Lisateavet leiate teemast kohta nende jäljendatud seadmete kohta järgmisest jaotisest.

Halli üksuste tähistavad komponendid, mida *seadme halduse* võimaluste rakendamiseks. Eelkonfigureeritud sõnastikupõhise hooldustööd lahenduse praeguse versiooni säte, need ressursid. Lisateavet seadme haldamine viidata selle [serveri jälgimise eelnevalt konfigureeritud lahendus][lnk-remote-monitoring].

## <a name="simulated-devices"></a>Jäljendatud seadmed

Eelkonfigureeritud lahenduse tähistab jäljendatud seade on neljasilindriline. Lahendus on ette valmistatud koos kahe mootorite, et vastendada ühe õhusõiduki. Iga mootori eraldab telemeetria nelja tüüpi: Sensor 9, Sensor 11, Sensor 14 ja andur 15 esitada kohapeal õ mudeli arvutamiseks soovitud ülejäänud kasulik kestus (RUL) mootor vajalikke andmeid. Iga jäljendatud seade saadab asjade jaoturi telemeetria järgmist teavet:

*Tsükkel loendus*. Tsükkel tähistab lõplikus flight erineva pikkusega 2-10 tundi, kus telemeetria andmed on hõivatud igal pool tundi on lennureisi ajal.

*Telemeetria*. Leidub neli andurid, mis tähistavad engine atribuute. Andurid on üldiselt märgistatud Sensor 9, Sensor 11, Sensor 14 ja andur 15. Need 4 andurid tähistavad telemeetria piisavalt õ seadme mudelist kasu tulemuste saamiseks RUL. See mudel on loodud avaliku real engine andurite andmeid sisaldava Andmekomplekt. Mudeli loomise viisist algse andmetel põhinevad kohta leiate lisateavet teemast [Cortana ärianalüüsi Galerii sõnastikupõhise hooldustööd malli][lnk-cortana-analytics].

Jäljendatud seadmed saavad hakkama soovitud asjade keskuse kaudu saadetud järgmised käsud:

| Käsk | Kirjeldus |
|---------|-------------|
| StartTelemetry | Määrab simulatsioon olek.<br/>Käivitab saatmine telemeetria seade     |
| StopTelemetry  | Määrab simulatsioon olek.<br/>Tabelduskohtade saatmine telemeetria seade |

Asjade keskus võimaldab seadme käsk kinnitus.

## <a name="azure-stream-analytics-job"></a>Azure'i voo Analytics töö

**Töö: telemeetria** sissetulevate seadme telemeetria voo abil kaks toimib. Esimene valib kõik telemeetria seadmete ja saadab andmed Bloobivahemälu salvestusruumi, kus visualiseeritakse web Appis. Teine lause avaldis arvutab Keskmine anduri väärtused üle kahe minuti libistades akna ja saadab **sündmuse protsessor**andmed sündmuse keskuse kaudu.

## <a name="event-processor"></a>Sündmuse protsessor

**Sündmuse protsessor** võtab väärtuste keskmine andur lõplikus tsükkel. See läheb neid väärtusi API, mis seab arvuti õ väljaõppe mudeli arvutamiseks RUL mootori jaoks.

## <a name="azure-machine-learning"></a>Azure'i masina õpetused

Mudeli loomise viisist algse andmetel põhinevad kohta leiate lisateavet teemast [Cortana ärianalüüsi Galerii sõnastikupõhise hooldustööd malli][lnk-cortana-analytics].

## <a name="lets-start-walking"></a>Alustame kõndimine

Selles jaotises juhendab teid lahenduse komponente, kirjeldatakse mõeldud juhul ja antakse ülevaade.

### <a name="predictive-maintenance-dashboard"></a>Sõnastikupõhise haldamise armatuurlaud

Selle lehe veebirakenduses kasutab PowerBI JavaScripti juhtelemente (vt [PowerBI-visuaale hoidla][lnk-powerbi]) visualiseerida:

- Väljundi andmed bloobimälu voo Analytics tööd.
- RUL ja tsükkeldiagramm arvu neljasilindriline kohta.

### <a name="observing-the-behavior-of-the-cloud-solution"></a>Jälgides toimimist pilve lahendus

Azure'i portaalis, liikuge ressursirühm lahenduse nimega otsustasite vaadata oma ettevalmistatud ressursse.

![][img-resource-group]

Kui olete ette eelkonfigureeritud lahenduse, kuvatakse seadme õ tööruumi lingi e-postiga. Saate ka liikuda arvuti õ tööruumi kaudu [azureiotsuite.com] [ lnk-azureiotsuite] lehe oma ettevalmistatud lahenduse, kui see on **valmis** olekus.

![][img-machine-learning]

Lahendus portaalis, näete, on valimi tähistada kahe õhusõiduki kaks mootorite lennukile, iga nelja anduritega nelja jäljendatud seadmega ette valmistatud. Kui te esmalt liikuda lahenduse portaali, simulatsiooni on peatatud.

![][img-simulation-stopped]

Klõpsake nuppu **Alusta simulatsiooni** alustamiseks simulatsiooni andur ajalugu, RUL, tsüklit, näete ja RUL ajalugu asustada armatuurlaud.

![][img-simulation-running]

Kui RUL on väiksem kui 160 (mõne läve valitud tegevused), lahenduse portaalis kuvatakse hoiatus sümbol kõrval kuvatav RUL ja tõstetakse esile neljasilindriline kollane. Pöörake tähelepanu sellele, kuidas RUL väärtused on üldised allapoole märgata, kuid on tavaliselt põrgatama üles ja alla. Selline käitumine tuleneb erineva tsükli pikkusega ja mudeli täpsuse.

![][img-simulation-warning]

Täielik simulatsioon võtab aega umbes 35 minutit 148 tsüklit lõpuleviimiseks. 160 RUL lävi on täidetud esimest korda umbes 5 minutit ja nii mootorite tabas piirmäära umbes 8 minutit.

Simulatsiooni 148 tsüklit kaudu täieliku andmekomplekti käivitub ja lahendab lõpliku RUL ja tsükkeldiagramm väärtused.

Soovi korral saate peatada mis tahes hetkel simulatsiooni, kuid klõpsake nuppu **Start simulatsioon** kordused simulatsiooni andmekomplekti algusest.

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui käivitate sõnastikupõhise hooldustööd eelkonfigureeritud lahendus soovite seda muuta, leiate [juhised kohandamise eelkonfigureeritud lahenduste][lnk-customize].

[Asjade komplekti - Kattealune - sõnastikupõhise hooldustööd](http://social.technet.microsoft.com/wiki/contents/articles/33527.iot-suite-under-the-hood-predictive-maintenance.aspx) TechNeti ajaveebipostituse pakub täiendavad üksikasjad sõnastikupõhise hooldustööd eelkonfigureeritud lahenduse kohta.

Samuti saate uurida muude funktsioonide ja võimaluste kohta asjade komplekti eelkonfigureeritud lahendused:

- [Korduma kippuvad küsimused asjade komplekti][lnk-faq]
- [Asjade turvalisuse kohalt kaudu][lnk-security-groundup]


[img-architecture]: media/iot-suite-predictive-walkthrough/architecture.png
[img-resource-group]: media/iot-suite-predictive-walkthrough/resource-group.png
[img-machine-learning]: media/iot-suite-predictive-walkthrough/machine-learning.png
[img-simulation-stopped]: media/iot-suite-predictive-walkthrough/simulation-stopped.png
[img-simulation-running]: media/iot-suite-predictive-walkthrough/simulation-running.png
[img-simulation-warning]: media/iot-suite-predictive-walkthrough/simulation-warning.png

[lnk-powerbi]: https://www.github.com/Microsoft/PowerBI-visuals
[lnk_machine_learning]: https://azure.microsoft.com/services/machine-learning/
[lnk-remote-monitoring]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-cortana-analytics]: http://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Template-3
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md
