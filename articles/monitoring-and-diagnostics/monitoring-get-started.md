<properties
    pageTitle="Azure'i kuvari alustamine | Microsoft Azure'i"
    description="Toimingu ressursside ülevaate ja välja andmete vastavalt tegutseda Azure'i kuvari kasutamise alustamine."
    authors="johnkemnetz"
    manager="rboucher"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="johnkem"/>

# <a name="get-started-with-azure-monitor"></a>Azure'i kuvari kasutamise alustamine

Azure'i kuvar on platform teenus, mis pakub ühe andmeallika jälgimise Azure ressursse. Azure'i monitoris, saate visualiseerida, päringu, marsruutimine, arhiivimine ja võtta mõõdikute ja ressursid Azure pärit logid. Saate töötada kuvari portaali labale [Kuvari PowerShelli cmdlet-käsud](./insights-powershell-samples.md), [Mitu platvormi CLI](insights-cli-samples.md)või [Azure kuvari REST API-de](https://msdn.microsoft.com/library/dn931943.aspx)kasutamine andmete. Selles artiklis tutvustame Azure'i kuvaris, kasutades portaali tutvustamise põhikomponentide mõne.

1. Portaali, liikuge **rohkem teenuseid** ja **kuvari** valiku. Klõpsake selle suvandi lisamiseks lemmikute loendisse nii, et see oleks alati kergesti juurdepääsetavad vasakpoolsel navigeerimisribal täheikoon.

    ![Teenuste loendis jälgimine](./media/monitoring-get-started/monitor-more-services.png)

2. Suvandit **kuvar** **kuvari** tera avada. See blade koondab kõik jälgimisega seotud sätted ja andmed ühe konsolideeritud vaade. Esmalt avaneb **tegevuste Logi** jaotis.

    ![Kuvari blade navigeerimine](./media/monitoring-get-started/monitor-blade-nav.png)

    > [AZURE.WARNING] **Teenuse teatised** ja **teatis rühmad** kohal kuvatud suvandid kuvatakse ainult neile, kes on liitunud avalikes eelvaate neid funktsioone.

    Azure'i jälgimine on lihtne kolmele jälgimise andmete: **tegevuse log**, **mõõdikute**ja **diagnostikalogid**.

3. Klõpsake **tegevuse logige** tagada tegevuste Logi jaotis kuvatakse.

    ![Tegevuste Logi blade](./media/monitoring-get-started/monitor-act-log-blade.png)

    [**Tegevuste Logi**](./monitoring-overview-activity-logs.md) kirjeldatakse teie tellimus ressursid kõigi toimingutest. Tegevuste Logi kasutamisel saate määrata selle ", mis, kes ja millal" jaoks mis tahes luua, värskendada ja kustutada toimingute ressursid teie tellimus. Näiteks tegevuste Logi ütleb teile, kui web app on peatatud ja kes on peatatud. Tegevuste sündmuste logi on talletatud platvormi ja päringu 90 päeva jooksul saadaval.
   
    Saate luua ja salvestada tavafiltrid päringuid, siis kõige olulisem päringute portaali armatuurlauale kinnitada, nii et te teate alati, kui kriteeriumidele vastavate sündmust.

4. Filtreerige vaatamiseks ressursile rühma viimase nädala jooksul ja seejärel klõpsake nuppu **Salvesta** .

    ![Tegevuste Logi päringu salvestamine](./media/monitoring-get-started/monitor-act-log-save.png)

5. Nüüd, klõpsake nuppu **Kinnita** .

    ![Klõpsake käsku Kinnita tegevuste Logi](./media/monitoring-get-started/monitor-act-log-pin.png)

    Enamik vaadetes juhendis saate kinnitatud armatuurlauale. See aitab teil luua oma teenuste ühe andmeallika teabe andmeid. 

6. Armatuurlauale naasmiseks. Nüüd saate vaadata oma armatuurlauale kuvatakse päringu (ja tulemite arv). See on kasulik, kui soovite mis tahes kõrgetasemelisi toiminguid, mis on toimunud viimati tellimuse, nt kiiresti näha. uude rolli määratud või VM on kustutatud.

    ![Tegevuste Logi kinnitatud armatuurlaud](./media/monitoring-get-started/monitor-act-log-db.png)

7. Naaske paani **jälgimine** ja klõpsake jaotise **mõõdikute** . Esmalt peate valima ressursi, filtreerimine ja valides tera ülaosas ripploendi suvandite abil.

    ![Filtri ressurss mõõdikud](./media/monitoring-get-started/monitor-met-filter.png)

    Kõik Azure ressursse eraldavad [**mõõdikute**](./monitoring-overview-metrics.md). See vaade koondab kõik mõõdikud ühe paani klaas nii saate hõlpsasti aru, kuidas oma ressursse toimivad.

8. Kui olete valinud ressursi, kuvatakse kõik saadaolevad mõõdikud on vasakul pool ketast. Saate organisatsiooniskeemi mitme mõõdikute korraga, valides mõõdikute ja Graphi vahemiku tüüp ja aja muutmine. Saate vaadata ka kõik argumendil teatised määrata selle ressursi.

    ![Argumendil blade](./media/monitoring-get-started/monitor-metric-blade.png)

    > [AZURE.NOTE] Teatud mõõdikute on saadaval ainult, võimaldades [Rakenduse ülevaated](../application-insights/app-insights-overview.md) ja/või Windowsi või Linuxi Azure'i diagnostika oma ressursi.

9. Kui olete tulemusega diagrammi, saate selle kinnitada armatuurlauale nuppu **Kinnita** .

10. Naaske **kuvari** tera ja klõpsake **diagnostikalogid**.

    ![Diagnostikalogide tera](./media/monitoring-get-started/monitor-diaglogs-blade.png)

    [**Diagnostikalogide**](monitoring-overview-of-diagnostic-logs.md) on logid kiiratav *,* ressurssi, mis pakuvad andmed sellele ressursile toimimise kohta. Näiteks võrgu turvalisus jaotises reegli hinnale ja loogika rakenduse töövoo logid on mõlemat tüüpi diagnostikalogid. Need logid saate salvestusruumi konto säilitatakse, sündmuse jaoturiga voona või saadetud [Log Analytics](../log-analytics/log-analytics-overview.md). Log Analytics on Microsofti funktsionaalseid ärianalüüsi toote täpsema otsingu ja teavitamine.
   
    Portaalis saate vaadata ja kõik ressursse loendi filtreerimine teie tellimus, kui need on lubatud diagnostikalogid.

11. Klõpsake diagnostikalogid tera ressurssi. Kui diagnostikalogid talletatakse salvestusruumi konto, kuvatakse loendi kord tunnis logid, kus saate otse alla laadida.

    ![Diagnostikalogide ühe ressurss](./media/monitoring-get-started/monitor-diaglogs-detail.png)

    Võite klõpsata ka **Diagnostikasätete**, mis võimaldab teil luua või muuta oma sätteid arhiivimine salvestusruumi konto, voogesitus sündmuse jaoturi või Log Analytics tööruumi saatmist.

    ![Diagnostikalogide lubamine](./media/monitoring-get-started/monitor-diaglogs-enable.png)

    Kui olete seadistanud diagnostikalogid, et Log Analytics, seejärel saate otsida neid portaali jaotises **Log otsing** .

12. Avage **teatiste** jaotises kuvari tera.

    ![teatiste tera avaliku](./media/monitoring-get-started/monitor-alerts-nopp.png)

    Siin saate hallata oma Azure ressursid kõik [**teatised**](./monitoring-overview-alerts.md) . See hõlmab mõõdikute, tegevuse sündmuste logi (jaotises privaatne eelvaade), rakenduse ülevaated web testide (asukohad) ja rakenduse ülevaated aktiivne diagnostika teatised. Teatiste käivitab e-posti saata või HTTP POSTITUSKOHT webhook URL-i.
   
13. Klõpsake **lisamine argumendil teatise** teatise loomine.

    ![argumendil teatise lisamine](./media/monitoring-get-started/monitor-alerts-add.png)

    Seejärel saate kinnitada teatise armatuurlaua kuvamiseks igal ajal hõlpsalt seisu.

14. Kuvari jaotis sisaldab ka [Rakenduse ülevaated](../application-insights/app-insights-overview.md) rakenduste ja lahenduste [Log Analytics](../log-analytics/log-analytics-overview.md) linke. Nende muude Microsofti toodete on sügav integreerimine Azure jälgimine.

15. Kui te ei kasuta rakenduse ülevaated või Log Analytics, tõenäoliselt, et Azure'i kuvar on koostöös oma praeguse jälgimise, logimine ja infoturbeküsimustes tooted. Lugege meie [partnerite lehel](./monitoring-partners.md) täieliku loendi leiate ja juhised, kuidas ühendada.

Järgmiste juhiste ja kõik oluline paanide lisamine armatuurlauale kinnitamine, saate luua oma rakenduse ja infrastruktuuri, nagu see täielik vaadete:

![Azure'i kuvari armatuurlaud](./media/monitoring-get-started/monitor-final-dash.png)

## <a name="next-steps"></a>Järgmised sammud
- [Ülevaade: Azure'i kuvari](./monitoring-overview.md) lugemine
