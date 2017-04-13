<properties
   pageTitle="Log Analytics Teavita REST API-ga"
   description="Log Analytics teatiste REST API võimaldab teil luua ja hallata teatisi toimingud halduse komplekti (OMS).  Selles artiklis antakse API ja mitu näidet üksikasjade erinevate toimingute tegemiseks."
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
   ms.date="10/18/2016"
   ms.author="bwren" />

# <a name="log-analytics-alert-rest-api"></a>Log Analytics Teavita REST API-ga

Log Analytics teatiste REST API võimaldab teil luua ja hallata teatisi toimingud halduse komplekti (OMS).  Selles artiklis antakse API ja mitu näidet üksikasjade erinevate toimingute tegemiseks.

Log Analytics Search REST API on RESTful ja pääseb Azure'i ressursihaldur REST API-ga. Selle dokumendi leiate näiteid kus API pääseb PowerShelli käsurea kaudu [ARMClient](https://github.com/projectkudu/ARMClient), avage allikas käsurea tööriista, mis lihtsustab kasutada Azure ressursihaldur API. ARMClient ja PowerShelli kasutamine on üks Log Analytics otsingu API juurdepääsu palju võimalusi. Nende tööriistade abil saate kasutada rahulik Azure'i ressursihaldur API helistada OMS tööruumide ja käsud neis tegemiseks. API väljund otsingutulemite teile JSON-vormingus, mis võimaldab teil kasutada otsingutulemite programmiliselt mitmel erineval viisil.

## <a name="prerequisites"></a>Eeltingimused
Praegu teatisi saate luua ainult salvestatud otsingu Log Analytics.  Viidata saate lisateavet [Log Search REST API -ga](log-analytics-log-search-api.md) .

## <a name="schedules"></a>Ajakava
Salvestatud otsingu võib olla üks või mitu ajakava. Ajasta määratleb, kui sageli otsing on Käivita ja ajavahemik, mis on määratletud kriteeriumid.
Järgmises tabelis on atribuutide ajakava.

| Atribuut  | Kirjeldus |
|:--|:--|
| Intervall | Kui sageli käivitatakse otsing. Mõõdetud minutit. |
| QueryTimeSpan | Ajavahemiku, kus hinnatakse kriteeriumid. Peab olema intervall suurem või sellega võrdne. Mõõdetud minutit. |
| Versioon | API versioon on kasutusel.  Praegu see alati olema seatud 1. |

Näiteks kaaluge sündmuse päring intervall 15 minutit ja on kuuline ajavahemik 30 minutit. Sel juhul päringu läheks iga 15 minuti ja teatise oleks käivitamine, kui kriteeriumid on jätkuvalt lahendamiseks true üle 30 minuti jooksul.

### <a name="retrieving-schedules"></a>Ajakava toomine
Get meetodi abil saate tuua kõik ajakava salvestatud otsingu.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search  ID}/schedules?api-version=2015-03-20

Kindla ajakava salvestatud otsingu toomiseks kasutada Get meetodit ajakava ID-ga.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}?api-version=2015-03-20

Järgmine on näide vastuse ajakava.

    {
        "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/MyWorkspace/savedSearches/0f0f4853-17f8-4ed1-9a03-8e888b0d16ec/schedules/a17b53ef-bd70-4ca4-9ead-83b00f2024a8",
        "etag": "W/\"datetime'2016-02-25T20%3A54%3A49.8074679Z'\"",
        "properties": {
        "Interval": 15,
        "QueryTimeSpan": 15
    }

### <a name="creating-a-schedule"></a>Ajakava loomine
Uue ajakava loomiseks kasutada pane meetodit ajakava kordumatu ID-ga.  Pange tähele, et kahe ajakavasid ei saa sama ID isegi juhul, kui need on seostatud erinevate salvestatud otsinguid.  Kui loote ajakava OMS konsooli, GUID luuakse ajakava ID-ga.

    $scheduleJson = "{'properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/mynewschedule?api-version=2015-03-20 $scheduleJson

### <a name="editing-a-schedule"></a>Ajakava redigeerimine
Olemasoleva ajakava ID sama salvestatud otsingu pane meetodi abil saate muuta selle ajakava.  Taotluse sisu peab sisaldama etag ajakava.

    $scheduleJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A49.8074679Z'\""','properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/mynewschedule?api-version=2015-03-20 $scheduleJson


### <a name="deleting-schedules"></a>Ajakavade kustutamine
Kasutage Kustuta meetodit kustutamiseks ajakava ajakava ID-ga.

    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}?api-version=2015-03-20


## <a name="actions"></a>Toimingud
Ajakava võib olla mitu toimingut. Toimingu määrata ühe või mitme protsessi sooritamiseks, nagu posti või käivitamine on käitusjuhendi või see võib määratleda, mis määrab, millal Otsingu tulemused vastavad teatud kriteeriumide.  Mõned toimingud määratleb nii, et protsesside teostamise kui piirmäär on täidetud.

Kõik toimingud on atribuutide järgmises tabelis.  Erinevat tüüpi teatised on erinevad täiendavad atribuudid, mis on kirjeldatud allpool.

| Atribuut | Kirjeldus |
|:--|:--|
| Tüüp | Toimingu tüüp.  Võimalikud väärtused on praegu teatise ja Webhook. |
| Nimi | Teatise kuvatav nimi. |
| Versioon | API versioon on kasutusel.  Praegu see alati olema seatud 1. |

### <a name="retrieving-actions"></a>Toomine toimingud
Get meetodi abil saate tuua kõik toimingud ajakava jaoks.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search  ID}/schedules/{Schedule ID}/actions?api-version=2015-03-20

Teatud toimingu jaoks ajakava toomiseks kasutada Get meetodit toimingu ID-ga.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}/actions/{Action ID}?api-version=2015-03-20

### <a name="creating-or-editing-actions"></a>Loomisel või redigeerimisel toimingud
Toimingu ID, mis on kordumatu ajakava luua uue toimingu panna meetodi abil.  Kui loote toimingu OMS konsooli, GUID on toimingu ID-ga.

Olemasoleva toimingu ID sama salvestatud otsingu pane meetodi abil saate muuta selle ajakava.  Taotluse sisu peab sisaldama etag ajakava.

Uue toimingu loomise taotluse vorming erineb olenevalt toimingu tüüp nii, et need näited on esitatud allpool.

### <a name="deleting-actions"></a>Toimingud kustutamine
Kasutage meetodit kustutamine toimingut ID-ga kustutamine toimingut.

    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}/Actions/{Action ID}?api-version=2015-03-20

### <a name="alert-actions"></a>Teatiste toimingud
Ajakava peaks olema ainult üks teatis toiming.  Teatiste toimingud on üks või mitu jaotiste järgmises tabelis.  Iga on kirjeldatud üksikasjalikumalt allpool.

| Jaotis | Kirjeldus |
|:--|:--|
| Lävi | Kui toiming käivitatakse kriteeriumid. |  
| EmailNotification | Saata meilisõnumeid mitmele adressaadile. |
| Parandamise | Azure'i automaatika tuvastatud probleemi lahendamiseks proovida mõnda käitusjuhendi käivitada. |

#### <a name="thresholds"></a>Piirmäärad
Teatiste toimingu peaks olema ainult üks lävi.  Kui salvestatud otsingu tulemused vastavad otsing seostatud toimingu piirmäär, käivitatakse kõik muud protsessid toiming.  Toimingu võib sisaldada ainult lävi ka nii, et seda saab kasutada muud tüüpi, mis ei sisalda lävede toiminguid.

Järgmises tabelis on lävede atribuutide.

| Atribuut | Kirjeldus |
|:--|:--|
| Tehtemärk | Lävi võrdluse tehtemärk. <br> gt = suurem kui <br> lt = väiksem kui |
| Väärtus | Piirmäära väärtus. |

Näiteks kaaluge sündmuse päring intervalliga 15 minutit, on kuuline ajavahemik 30 minuti ja läve suurem kui 10. Sel juhul päringu läheks iga 15 minuti ja teatise oleks käivitamine, kui seda ei tagastatud 10 sündmused, mis on loodud üle 30 minuti jooksul.

Järgmine on valimi toimingu abil ainult lävi.  

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My threshold action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "Version": 1
    }

Uue lävi toimingu jaoks ajakava loomiseks kasutada pane meetodit toimingu kordumatu ID-ga.  

    $thresholdJson = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdJson

Olemasoleva toimingu ID panna meetodi abil saate lävi toimingut ajakava jaoks muuta.  Taotluse sisu peab sisaldama meetme etag.

    $thresholdJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdJson

#### <a name="email-notification"></a>E-posti teatis
Meiliteatised e-posti saata ühele või mitmele adressaadile.  Need suvandid on järgmised atribuudid järgmises tabelis.

| Atribuut | Kirjeldus |
|:--|:--|
| Adressaadid | Meiliaadresside loend. |
| Teema | Meili teema. |
| Manuse | Manused pole praegu toetatud, nii, et see on alati väärtus "Puudub". |

Järgmine on näide toimingu e-posti teatis koos piirmäära.  

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My email action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "EmailNotification": {
            "Recipients": [
                "recipient1@contoso.com",
                "recipient2@contoso.com"
            ],
            "Subject": "This is the subject",
            "Attachment": "None"
        },
        "Version": 1
    }

Uue e-posti toimingu jaoks ajakava loomiseks kasutada pane meetodit toimingu kordumatu ID-ga.  Järgmises näites luuakse meiliteatise piirmäära, et kui salvestatud otsingu tulemused ületavad piirmäära, saadetakse meili.

    $emailJson = "{'properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myemailaction?api-version=2015-03-20 $ emailJson

Olemasoleva toimingu ID panna meetodi abil saate muuta ajakava jaoks e-posti toimingu.  Taotluse sisu peab sisaldama meetme etag.

    $emailJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myemailaction?api-version=2015-03-20 $ emailJson

#### <a name="remediation-actions"></a>Parandamise toimingud
Remediations käivitada soovitud käitusjuhendi Azure'i automaatika, mis avaldab teatise tähistatud probleemi lahendamiseks.  Peate looma webhook, jaoks kasutatud parandamise toimingu käitusjuhendi ja seejärel määrake atribuudi WebhookUri URI.  Selle toimingu abil OMS konsooli loomisel luuakse uus webhook käitusjuhendi automaatselt.

Remediations kaasata atribuutide järgmises tabelis.

| Atribuut | Kirjeldus |
|:--|:--|
| RunbookName | Käitusjuhendi nimi. See peab vastama avaldatud käitusjuhendi automatiseerimine konto konfigureeritud automatiseerimise lahendus teie OMS tööruumis. |
| WebhookUri | Funktsiooni webhook URI.
| Aegumise | Aegumise kuupäeva ja kellaaja, kuvatakse webhook.  Kui soovitud webhook pole mõni aegumise, siis see võib olla mis tahes tulevaste kehtiv kuupäev. |

Järgmine on näide parandamise toimingu abil piirmäära.

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My remediation action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "Remediation": {
            "RunbookName": "My-Runbook",
            "WebhookUri": "https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d",
            "Expiry": "2018-02-25T18:27:20"
            },
        "Version": 1
    }

Uue parandamise toimingu jaoks ajakava loomiseks kasutada pane meetodit toimingu kordumatu ID-ga.  Järgmises näites luuakse on parandamise piirmäära, et käitusjuhendi käivitatakse kui salvestatud otsingu tulemused ületab läve.

    $remediateJson = "{'properties': { 'Type':'Alert', 'Name': 'My Remediation Action', 'Version':'1', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'Remediation': {'RunbookName': 'My-Runbook', 'WebhookUri':'https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d', 'Expiry':'2018-02-25T18:27:20Z'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myremediationaction?api-version=2015-03-20 $remediateJson

Olemasoleva toimingu ID panna meetodi abil saate muuta parandamise toimingu ajakava jaoks.  Taotluse sisu peab sisaldama meetme etag.

    $remediateJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Type':'Alert', 'Name': 'My Remediation Action', 'Version':'1', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'Remediation': {'RunbookName': 'My-Runbook', 'WebhookUri':'https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d', 'Expiry':'2018-02-25T18:27:20Z'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myremediationaction?api-version=2015-03-20 $remediateJson

#### <a name="example"></a>Näide
Järgmine on lõpule viidud näide luua uue meilisõnumi teatis.  See loob uue ajakava koos toimingu, mis sisaldab lävi ja e-posti.

    $subscriptionId = "3d56705e-5b26-5bcc-9368-dbc8d2fafbfc"
    $workspaceId    = "MyWorkspace"
    $searchId       = "51cf0bd9-5c74-6bcb-927e-d1e9080b934e"

    $scheduleJson = "{'properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/$workspaceId/savedSearches/$searchId/schedules/myschedule?api-version=2015-03-20 $scheduleJson

    $thresholdJson = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/$workspaceId/savedSearches/$searchId/schedules/myschedule/actions/mythreshold?api-version=2015-03-20 $thresholdJson

    $emailJson = "{'properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/$workspaceId/savedSearches/$searchId/schedules/myschedule/actions/myemailaction?api-version=2015-03-20 $emailJson

### <a name="webhook-actions"></a>Webhook toimingud
Webhook toimingute alustamiseks helistades URL-i ja soovi korral esitada last, tuleb saata.  Need sarnanevad parandamise toimingud, kuid need on mõeldud webhooks, mis võib kasutada protsessid peale Azure'i automaatika tegevusraamatud.  Samuti pakuvad täiendavad võimalus on last toimetada serveri protsess.

Webhook toimingud ei ole piirmäära, kuid selle asemel tuleks lisada ajakava, mis on toimingu teatis koos läve.  Saate lisada mitu Webhook toiminguid, mida saavad kõik käivitada, kui piirmäär on täidetud.

Webhook hõlmavad atribuutide järgmises tabelis.

| Atribuut | Kirjeldus |
|:--|:--|
| WebhookUri | Meili teema. |
| CustomPayload | Kohandatud last saata selle webhook.  Vormingu sõltub selle webhook ootate. |

Järgmine on valimi webhook toimingu ja koos piirmäära toimingu seotud teatis.

    {
        "__metadata": {},
        "value": [
            {
                "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/bwren/savedSearches/2d1b30fb-7f48-4de5-9614-79ee244b52de/schedules/b80f5621-7217-4007-b32d-165d14377093/Actions/72884702-acf9-4653-bb67-f42436b342b4",
                "etag": "W/\"datetime'2016-02-26T20%3A25%3A00.6862124Z'\"",
                "properties": {
                    "Type": "Webhook",
                    "Name": "My Webhook Action",
                    "WebhookUri": "https://oaaswebhookdf.cloudapp.net/webhooks?token=VfkYTIlpk%2fc%2bJBP",
                    "CustomPayload": "{\"fielld1\":\"value1\",\"field2\":\"value2\"}",
                    "Version": 1
                }
            },
            {
                "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/bwren/savedSearches/2d1b30fb-7f48-4de5-9614-79ee244b52de/schedules/b80f5621-7217-4007-b32d-165d14377093/Actions/90a27cf8-71b7-4df2-b04f-54ed01f1e4b6",
                "etag": "W/\"datetime'2016-02-26T20%3A25%3A00.565204Z'\"",
                "properties": {
                    "Type": "Alert",
                    "Name": "Threshold for my webhook action",
                    "Threshold": {
                        "Operator": "gt",
                        "Value": 10
                    },
                    "Version": 1
                }
            }
        ]
    }

#### <a name="create-or-edit-a-webhook-action"></a>Looge või webhook toimingu redigeerimine
Uue webhook toimingu jaoks ajakava loomiseks kasutada pane meetodit toimingu kordumatu ID-ga.  Järgmises näites luuakse Webhook toiming ja toimingu teatis koos piirmäära, et selle webhook käivitatakse kui salvestatud otsingu tulemused ületab läve.

    $thresholdAction = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdAction

    $webhookAction = "{'properties': {'Type': 'Webhook', 'Name': 'My Webhook", 'WebhookUri': 'https://oaaswebhookdf.cloudapp.net/webhooks?token=VrkYTKlhk%2fc%2bKBP', 'CustomPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}', 'Version': 1 }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mywebhookaction?api-version=2015-03-20 $webhookAction

Olemasoleva toimingu ID panna meetodi abil saate muuta webhook toimingu ajakava jaoks.  Taotluse sisu peab sisaldama meetme etag.

    $webhookAction = "{'etag': 'W/\"datetime'2016-02-26T20%3A25%3A00.6862124Z'\"','properties': {'Type': 'Webhook', 'Name': 'My Webhook", 'WebhookUri': 'https://oaaswebhookdf.cloudapp.net/webhooks?token=VrkYTKlhk%2fc%2bKBP', 'CustomPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}', 'Version': 1 }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mywebhookaction?api-version=2015-03-20 $webhookAction

## <a name="next-steps"></a>Järgmised sammud

- Kasutage Log Analytics [REST API log otsimiseks](log-analytics-log-search-api.md) .
