<properties
   pageTitle="Log Analytics teatiste webhook näidis"
   description="Üks käivitada Log Analytics teatise vastuseks toimingud on mõne *webhook*, mis võimaldab teil autonoomsest mõni väline protsess ühe päringu HTTP kaudu. Selles artiklis tutvustatakse näiteks luua Logi Analytics teatise, kasutades vaikne webhook toiming."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/27/2016"
   ms.author="bwren" />

# <a name="webhooks-in-log-analytics-alerts"></a>Log Analytics teatiste Webhooks

Üks käivitada ka [Log Analytics teatise](log-analytics-alerts.md) vastuseks toimingud on mõne *webhook*, mis võimaldab teil autonoomsest ühe HTTP taotlust läbi välise käigus.  Saate lugeda teatised ja webhooks teatiste [Log Analytics](log-analytics-alerts.md) üksikasjad

Selles artiklis selgitame loomise webhook toimingu Log Analytics teatise, vaikne, mis on sõnumiteenuse kasutamise näide.

>[AZURE.NOTE] Vaikne konto selle valimi lõpuleviimiseks peab teil olema.  Saate registreeruda tasuta konto veebisaidil [slack.com](http://slack.com).

## <a name="step-1---enable-webhooks-in-slack"></a>Samm 1 - luba webhooks vaikne rakenduses
2.  Logige sisse aadressil [slack.com](http://slack.com)vaikne.
3.  Valige vasakul paanil jaotises **kanalite** kanali.  See on kanal, mis saadetakse sõnum.  Saate valida ühe vaikimisi kanalid, nt **Üldine** või **juhuslik**.  Tootmise stsenaariumi looks tõenäoliselt teisiti kanali, nt **criticalservicealerts**. <br>

    ![Vaikne kanalid](media/log-analytics-alerts-webhooks/oms-webhooks01.png)

3. Klõpsake käsku **Lisa rakendus või kohandatud integreerimise** rakendus kausta avada.
3.  Tippige väljale Otsi *webhooks* ja seejärel valige **Sissetulevate WebHooks**. <br>

    ![Vaikne kanalid](media/log-analytics-alerts-webhooks/oms-webhooks02.png)

4.  Klõpsake oma meeskonnatöö nime kõrval nuppu **Installi** .
5.  Klõpsake **Lisa konfigureerimine**.
6.  Valige soovitud kanalit, mida kavatsete selle näite puhul kasutada, ja seejärel klõpsake nuppu **Lisa sissetulevate WebHooks integreerimine**.  
6. Kopeerige **Webhook URL-i**.  Teil tuleb olla kleepides see teatiste konfigureerimine. <br>

    ![Vaikne kanalid](media/log-analytics-alerts-webhooks/oms-webhooks05.png)

## <a name="step-2---create-alert-rule-in-log-analytics"></a>Samm 2 - Log Analytics reegli loomine
1.  [Reegli loomine](log-analytics-alerts.md) järgmised sätted.
    - Päringu:```    Type=Event EventLevelName=error ```
    - Märkige ruut selle teatise iga: 5 minutit.
    - Tulemuste arv on: suurem kui 10
    - Üle selle ajaakna: 60 minutit
    - Valige **Jah** **Webhook** ja **mitte** muude toimingute jaoks.
7. Vaikne URL-i kleepida **Webhook URL-i** välja.
8. Valige suvand **kohandatud JSON last**kaasata.
9. Lõtk eeldab, et last, mis on vormindatud JSON parameetriga, nimega *teksti*.  See on see loob sõnumis kuvatav tekst.  Saate kasutada ühte või mitut teatise parameetrid, kasutades funktsiooni *#* märki selline, nagu järgmises näites.

    ```
    {
    "text":"#alertrulename fired with #searchresultcount records which exceeds the over threshold of #thresholdvalue ."
    }
    ```

    ![Näide JSON last](media/log-analytics-alerts-webhooks/oms-webhooks07.png)

9.  Klõpsake nuppu **Salvesta** reegli salvestamiseks.

10. Oodake jõuaks teatise luua, ja seejärel märkige vaikne sõnumi, mis sarnaneb järgmisega.

    ![Näide webhook vaikne rakenduses](media/log-analytics-alerts-webhooks/oms-webhooks08.png)


### <a name="advanced-webhook-payload-for-slack"></a>Täiustatud webhook last vaikne

Saate kohandada ulatuslikult sissetulevate sõnumite vaikne abil. Lisateabe saamiseks vt [Sissetuleva Webhooks](https://api.slack.com/incoming-webhooks) vaikne veebisaidil. Järgmine on märksa keerukam last luua rikkalikke sõnumi vormingu.

    {
        "attachments": [
            {
                "title":"OMS Alerts Custom Payload",
                "fields": [
                    {
                        "title": "Alert Rule Name",
                        "value": "#alertrulename"},
                    {
                        "title": "Link To SearchResults",
                        "value": "<#linktosearchresults|OMS Search Results>"},
                    {
                        "title": "Search Interval",
                        "value": "#searchinterval"},
                    {
                        "title": "Threshold Operator",
                        "value": "#thresholdoperator"},
                    {
                        "title": "Threshold Value",
                        "value": "#thresholdvalue"}
                ],
                "color": "#F35A00"
            }
        ]
    }


See tekitaks sõnumi vaikne järgmise sisuga.

![Vaikne sõnum näide](media/log-analytics-alerts-webhooks/oms-webhooks09.png)

## <a name="summary"></a>Kokkuvõte

See teatis reegel, siis oleks iga kord, kui kriteeriumitele vaikne saadetud sõnumile.  

See on ainult üks näide toiming, mille saate luua vastuseks teatise.  Saate luua webhook toiming, mis nõuab mõne muu välise teenuse, käitusjuhendi toimingu alustamiseks on käitusjuhendi Azure automatiseerimine või e-posti toimingu meilisõnumi saatmiseks endale või teistele adressaatidele.   

## <a name="next-steps"></a>Järgmised sammud

- Lisateave [Logi Analytics teatised](log-analytics-alerts.md) , sh muude toimingute kohta.
- [Loo tegevusraamatud Azure'i automaatika](../automation/automation-webhooks.md) , saate helistada on webhook.
