<properties
    pageTitle="Azure'i valitsuse dokumentidele | Microsoft Azure'i"
    description="See pakub võrdlus funktsioonid ja juhised arendada Azure'i Government rakendusi."
    services="Azure-Government"
    cloud="gov"
    documentationCenter=""
    authors="ryansoc"
    manager="zakramer"
    editor=""/>

<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="10/25/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-monitoring-and-management"></a>Azure'i Government jälgimise ja halduse

Selles artiklis kirjeldatakse jälgimise ja halduse teenuseid variatsioonid ja Azure Governmenti keskkonnas peaksite arvesse võtma.

## <a name="automation"></a>Automatiseerimine

Automaatika on üldiselt saadaval Azure Government.

### <a name="variations"></a>Variatsioonid

Järgmised automatiseerimise funktsioonid pole praegu saadaval Azure Government.

+ Teenuse põhimõtet mandaadi autentimine loomine

Lisateavet leiate [automatiseerimise avaliku dokumentatsioonist](../automation/automation-intro.md).

## <a name="log-analytics"></a>Log Analytics

Log Analytics on üldiselt saadaval Azure Government.

### <a name="variations"></a>Variatsioonid

Järgmised Logi Analytics funktsioone ja lahendusi pole praegu saadaval Azure valitsuse.

+ Lahendusi, mis on eelvaade Microsoft Azure, sh:
  - Võrgu jälgimis lahendus
  - Rakenduse sõltuvus jälgimise lahendus
  - Office 365 lahendus
  - Windows 10 versioonitäiendus Analytics lahendus
  - Rakenduse ülevaateid lahendus
  - Azure'i Networking Analytics lahendus
  - Azure'i automaatika Analytics lahendus
  - Klahv Vault Analytics lahendus
+ Lahenduste ja funktsioone, mis nõuavad värskenduste tarkvara, sh:
  - Integreerimine süsteemi Center toimingute Manager 2016 (varasemates versioonides loodud Toiminguhalduri on toetatud)
  - Arvutite rühmade System Center Configuration Manager
  - Surface'i jaoturi lahendus
+ Funktsioonid, mis on eelvaates avaliku Azure, sh:
  - Power BI andmete eksportimine
+ Azure'i mõõdikute ja Azure diagnostika
+ Toimingute halduse mobiilirakenduste

URL-ide Log Analyticsi Azure Governmenti teistsugused.

| Azure'i avaliku | Azure'i Government | Märkmete |
|--------------|------------------|-------|
| MMS.microsoft.com | OMS.microsoft.us | Log Analytics portaal |
| *workspaceId*. ods.opinsights.azure.com | *workspaceId*. ods.opinsights.azure.us | [Andmekoguja API](../log-analytics/log-analytics-data-collector-api.md) 
| \*. ods.opinsights.azure.com | \*. ods.opinsights.azure.us | Agent teatis – [tulemüüri sätete konfigureerimine](../log-analytics/log-analytics-proxy-firewall.md) |
| \*. oms.opinsights.azure.com | \*. oms.opinsights.azure.us | Agent teatis – [tulemüüri sätete konfigureerimine](../log-analytics/log-analytics-proxy-firewall.md) |
| \*. blob.core.windows.net | \*. blob.core.usgovcloudapi.net | Agent teatis – [tulemüüri sätete konfigureerimine](../log-analytics/log-analytics-proxy-firewall.md) |


Azure'i valitsuse käituvad teisiti Log Analytics järgmisi funktsioone.

+ Windows Agent tuleb alla laadida [Log Analytics portaalis](https://oms.microsoft.us) Azure valitsuse.
+ Oma System Center Operations Manager management serveri ühenduse Log Analytics peate alla laadima ja importimine värskendatud halduse paketid.
  1. Laadige alla ja salvestage [värskendatud halduse pakendit](http://go.microsoft.com/fwlink/?LinkId=828749).
  2. Allalaaditud faili pakkige see lahti.
  3. Saate importida Toiminguhalduri halduse paketid. Teavet selle kohta, kuidas importida halduspakett toiminguhalduri kettal, vaadake, [Kuidas importida toimingute halduri halduspaketi](http://technet.microsoft.com/library/hh212691.aspx) Microsoft TechNeti veebisaidil.
  4. Log Analytics Toiminguhalduri ühenduse juhiste [Ühenduse Toiminguhalduri Log Analytics](../log-analytics/log-analytics-om-agents.md).


## <a name="frequently-asked-questions"></a>Korduma kippuvad küsimused

+ Saate migreerida andmete Log Analytics Microsoft Azure Azure'i valitsuse?
  - Ei. Ei saa andmeid või tööruumi Microsoft Azure'i Azure Governmenti liikuda.
+ Saate vaheldumisi aktiveerida Microsoft Azure ja Azure Governmenti tööruumide portaalist toimingud halduse komplekti Log Analytics?
  - Ei. Portaalide Microsoft Azure ja Azure Governmenti on eraldi ja teavet ühiskasutusse anda.

Lisateavet leiate [Log Analytics avalike dokumentatsioonist](../log-analytics/log-analytics-overview.md).

## <a name="next-steps"></a>Järgmised sammud

Lisateave ja värskendused, tellimine on <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure'i Government ajaveeb.</a>
