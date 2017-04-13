<properties
    pageTitle="Mitu platvormi käsurea liides (CLI) abil saate luua teatiste Azure'i teenuste jaoks | Microsoft Azure'i"
    description="Käsurea liides abil saate luua Azure teatised, mis käivitab teatisi või automatiseerimise, kui teie määratud tingimustele."
    authors="rboucher"
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
    ms.date="10/24/2016"
    ms.author="robb"/>

# <a name="use-the-cross-platform-command-line-interface-cli-to-create-alerts-for-azure-services"></a>Mitu platvormi käsurea liides (CLI) abil saate luua Azure'i teenuste teatised

> [AZURE.SELECTOR]
- [Portaal](insights-alerts-portal.md)
- [PowerShelli](insights-alerts-powershell.md)
- [CLI](insights-alerts-command-line-interface.md)

## <a name="overview"></a>Ülevaade

Selles artiklis näidatakse, kuidas häälestada Azure teatised käsurea liides (CLI) abil.

>[AZURE.NOTE] Azure'i kuvar on kuni 25 Sept 2016 mis kutsuti "Azure ülevaateid" uus nimi. Siiski soovitud nimeruumid ja seega allpool käsud sisaldavad siiski "ülevaateid".

Saate teatise jälgimisega seotud mõõdikud või sündmuste oma Azure'i teenuste kohta.

- **Argumendil väärtused** - teatis päästikute, kui määratud mõõtühiku väärtus ületab läve määrate suunda. Mis on see käivitab nii kui tingimus on täidetud esmalt ja seejärel hiljem kui see tingimus on ei ole enam täidetud.    
- **Tegevuste ja sündmuste** - teatise käivitab *iga* sündmuse, või ainult siis, kui sündmuste arv ilmneda.

Saate konfigureerida tehke järgmist, kui see käivitab teatis.

- teenuse administraator ja kaasadministraatorite meiliteatiste saatmine
- täiendavad meilisõnumid teie määratud e-posti saata.
- kõne on webhook
- Käivitage täitmine on Azure käitusjuhendi (ainult portaalist Azure sel ajal).

Saate konfigureerida ja teatiste reeglite kasutamise kohta teabe saamine

- [Azure'i portaal](insights-alerts-portal.md)
- [PowerShelli](insights-alerts-powershell.md)
- [käsurea liides (CLI)](insights-alerts-command-line-interface.md)
- [Azure'i kuvari REST API-ga](https://msdn.microsoft.com/library/azure/dn931945.aspx)


Saate alati abiks käsud käsk ja kasutuselevõtt – spikker lõpus. Näiteks:

    ```console
    azure insights alerts -help
    azure insights alerts actions email create -help
    ```

## <a name="create-alert-rules-using-the-cli"></a>Teatiste abil CLI reeglite loomine

1. Teostada eeltingimused ja Azure sisse logida. Leiate [Azure'i kuvari CLI näidised](insights-cli-samples.md). Lühidalt, installige CLI ja käivitage järgmised käsud. Need saaksite sisse logitud, tellimuse kasutate, ja valmistada Azure'i kuvari käskude Kuva.


    ```console
    azure login
    azure account show
    azure config mode arm

    ```

2.  Loendi kehtivad ressursirühma, kasutage järgmist vormi **azure ülevaateid teatiste reegel loendis** *[Valikud] &lt;resourceGroup&gt; *

    ```console
    azure insights alerts rule list myresourcegroupname

    ```
3. Reegli loomiseks peate on mitu olulisemad teabe esmalt.
    - Ressursi **Ressursi ID -d** , mida soovite teatise seadistamine
    - **Argumendil määratlused** selle ressursi jaoks saadaval

    Üks võimalus ressursi ID-d on kasutada Azure portaali. Eeldades, et ressurss on juba loonud, valige see portaalis. Järgmise tera, valige *Atribuudid* jaotises *sätted* . *RESSURSI ID -d* on järgmine tera välja. Teine võimalus on kasutada [Azure Resource Explorer](https://resources.azure.com/).

    Näide ressursi id web app on

    ```console
    /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename
    ```

    Nende ressursside eelmises näites mõõdikud jaoks saadaval mõõdikute ja üksuste loendi saamiseks kasutage CLI järgmine käsk:  

    ```console
    azure insights metrics list /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename PT1M
    ```

    _PT1M_ on granulaarsus saadaval mõõtmine (1-minuti järel). Erinevate granulaarsused kasutamine annab teile argumendil erinevaid võimalusi.


4. Teatiste meetermõõdustik-põhise reegli loomiseks kasutada käsk järgmisel kujul:

    **Azure'i ülevaateid teatised reegli meetermõõdustik seadmine** *[Valikud] &lt;ruleName&gt; &lt;asukoht&gt; &lt;resourceGroup&gt; &lt;windowSize&gt; &lt;tehtemärk&gt; &lt;lävi&gt; &lt;targetResourceId&gt; &lt;metricName&gt; &lt;timeAggregationOperator&gt; *

    Järgmises näites luuakse teatise veebisaidi ressursi. Teatiste päästikute, kui ta saab pidevalt mis tahes liikluse 5 minutit ja uuesti, kui ta saab pole liikluse 5 minutit.

    ```console
    azure insights alerts rule metric set myrule eastus myreasourcegroup PT5M GreaterThan 2 /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename BytesReceived Total

    ```

5. Luua webhook või saatke meilisõnum, kui teatise käivitub, esmalt luua e-posti ja/või webhooks. Looge reegel kohe pärast seda. Te ei saa seostada webhook või e-kirju juba loonud reeglite abil CLI.

    ```console
    azure insights alerts actions email create --customEmails myemail@contoso.com

    azure insights alerts actions webhook create https://www.contoso.com

    azure insights alerts rule metric set myrulewithwebhookandemail eastus myreasourcegroup PT5M GreaterThan 2 /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename BytesReceived Total
    ```


6. Teatis, mis käivitub luua teatud tingimus tegevuste Logi, vormi:

    **ülevaateid teatised reegli logisse** *[Valikud] &lt;ruleName&gt; &lt;asukoht&gt; &lt;resourceGroup&gt; &lt;operationName&gt; *

    Näide

    ```console
    azure insights alerts rule log set myActivityLogRule eastus myresourceGroupName Microsoft.Storage/storageAccounts/listKeys/action
    ```

    Funktsiooni operationName vastab mõne kirje tegevuste Logi sündmuse tüüp. Näiteks *Microsoft.Compute/virtualMachines/delete* ja *microsoft.insights/diagnosticSettings/write*.

    PowerShelli käsku [Get-AzureRmProviderOperation](https://msdn.microsoft.com/library/mt603720.aspx) abil võimalik operationNames loendi. Vaheldumisi, saate Azure portaali tegevuse Logi päringu ja leida kindla eelmiste toimingute kohta, mida soovite teatise luua. Pildi log vaate sõbralik nimede toimingud. Kirje JSON otsida ja tõmmake OperationName väärtus.   

7. Saate kontrollida, et teatiste on loodud õigesti vaadates üksikuid reegli.

    ```console
    azure insights alerts rule list myresourcegroup --ruleName myrule
    ```

8. Kustutage reeglid, kasutage vormi käsk:

    **ülevaateid teatised reegli kustutamine** [Valikud] &lt;resourceGroup&gt; &lt;ruleName&gt;

    Need käsud kustutage reeglid, mis on selles artiklis varem loonud.

    ```console
    azure insights alerts rule delete myresourcegroup myrule
    azure insights alerts rule delete myresourcegroup myrulewithwebhookandemail
    azure insights alerts rule delete myresourcegroup myActivityLogRule
    ```



## <a name="next-steps"></a>Järgmised sammud

* [Ülevaate Azure jälgimine,](monitoring-overview.md) sh tüüpi teavet saate koguda ja jälgida.
* Lisateavet [teatiste seadistamine webhooks](insights-webhooks-alerts.md).
* Lisateave [Azure'i automaatika tegevusraamatud](..\automation\automation-starting-a-runbook.md).
* Siit saate [ülevaate kogumiseks diagnostikalogid](monitoring-overview-of-diagnostic-logs.md) koguda üksikasjalikku suure sagedusega mõõdikute teenust.
* Siit saate [ülevaate mõõdikute saidikogumi](insights-how-to-customize-monitoring.md) veendumaks, et teie teenus on saadaval ja reageeri.
