<properties
    pageTitle="Azure'i valitsuse dokumentidele | Microsoft Azure'i"
    description="See pakub võrdlus funktsioonid ja juhiseid Azure'i valitsuse rakenduste arendamise kohta"
    services="Azure-Government"
    cloud="gov" 
    documentationCenter=""
    authors="scooxl"
    manager="zakramer"
    editor=""/>
<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="10/25/2016"
    ms.author="scooxl"/>
#  <a name="azure-government-management-and-security"></a>Azure'i Government haldamine ja turvalisus

## <a name="automation"></a>Automatiseerimine

Automaatika on üldiselt saadaval Azure Government.

### <a name="variations"></a>Variatsioonid

Järgmised automatiseerimise funktsioonid pole praegu saadaval Azure Government.

+ Teenuse põhimõtet mandaadi autentimine loomine

Lisateavet leiate [automatiseerimise avaliku dokumentatsioonist](../automation/automation-intro.md).


##  <a name="key-vault"></a>Võtme Vault
Selle teenuse ja kuidas seda kasutada kohta täpsema teabe saamiseks vt selle <a href="https://azure.microsoft.com/documentation/services/key-vault">Azure klahvi Vault avaliku dokumentatsiooni.</a>
### <a name="data-considerations"></a>Andmete kaalutlused
Järgmine teave tuvastab Azure Governmenti äärist Azure'i klahvi Vault jaoks:

| Lubatud reguleeritud kontrolli all andmed | Andmete reguleeritud kontrolli all pole lubatud |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Kõik andmed, mis on Azure klahvi Vault võti krüptitud võib sisaldada reguleeritud/kontrollitud andmeid. | Azure'i klahvi Vault metaandmete on ei tohi sisaldada ekspordi kontrollitud andmed. Selle metaandmed sisaldab kõik sisestatud loomisel ja säilitada oma võti Vault konfiguratsiooni andmed.  Sisestage andmete reguleeritud/järgmised väljad: ressursside rühma nimed, võti Vault nimed, tellimuse nimi |

Klahv Vault on üldiselt saadaval Azure Government. Avalik, nagu on ilma laiendita nii, et klahv Vault on saadaval PowerShelli kaudu CLI ainult.
## <a name="log-analytics"></a>Log Analytics
Log Analytics on üldiselt saadaval Azure Government. 

### <a name="variations"></a>Variatsioonid

Järgmised Logi Analytics funktsioone ja lahendusi pole praegu saadaval Azure valitsuse. See loend on värskendatud kui funktsioonide olekut / lahenduste muudatused.

+ Lahendusi, mis on eelvaates avaliku Azure, sh:
  - Võrgu jälgimis lahendus
  - Rakenduse sõltuvus jälgimine
  - Office 365 lahendus
  - Windows 10 versioonitäiendus Analytics lahendus
  - Rakenduse ülevaated
  - Azure'i Networking Analytics lahendus
  - Azure'i automaatika Analytics
  - Võtme Vault Analytics
+ Lahenduste ja funktsioone, mis nõuavad värskenduste tarkvara, sh
  - Integreerimine süsteemi Center toimingute Manager 2016 (varasemates versioonides loodud Toiminguhalduri on toetatud)
  - Arvutite rühmade System Center Configuration Manager
  - Surface'i jaoturi lahendus
+ Funktsioonid, mis on eelvaates avaliku Azure, sh
  - PowerBI andmete eksportimine
+ Azure'i mõõdikute ja Azure diagnostika
+ OMS Mobile'i rakendused

URL-ide Log Analyticsi Azure Governmenti teistsugused.

| Azure'i avaliku | Azure'i Government | Märkmete |
|--------------|------------------|-------|
| MMS.microsoft.com | OMS.microsoft.us | Log Analytics portaal |
| *workspaceId*. ods.opinsights.azure.com | *workspaceId*. ods.opinsights.azure.us | [Andmekoguja API](../log-analytics/log-analytics-data-collector-api.md) 
| \*. ods.opinsights.azure.com | \*. ods.opinsights.azure.us | Agent teatis – [tulemüüri sätete konfigureerimine](../log-analytics/log-analytics-proxy-firewall.md) |
| \*. oms.opinsights.azure.com | \*. oms.opinsights.azure.us | Agent side - [tulemüüri sätete konfigureerimine](../log-analytics/log-analytics-proxy-firewall.md) |
| \*. blob.core.windows.net | \*. blob.core.usgovcloudapi.net | Agent side - [tulemüüri sätete konfigureerimine](../log-analytics/log-analytics-proxy-firewall.md) |


Log Analytics järgmised funktsioonid on erinevate käitumine Azure Governmenti:

+ Windows agent tuleb alla laadida [Log Analytics portaalis](https://oms.microsoft.us) Azure valitsuse.
+ Oma System Center Operations Manager management serveri ühenduse Log Analytics peate alla laadima ja importimine värskendatud halduse paketid.
  1. Laadige alla ja salvestage [värskendatud halduse tuvastamine](http://go.microsoft.com/fwlink/?LinkId=828749)
  2. Pakkige lahti laaditud faili
  3. Saate importida Toiminguhalduri halduse paketid. Lisateavet selle kohta, kuidas importida halduspakett toiminguhalduri kettal, lugege teemat [Kuidas importida toimingute halduri halduspaketi](http://technet.microsoft.com/library/hh212691.aspx) Microsoft TechNeti veebisaidil.
  4. Log Analytics Toiminguhalduri loomiseks järgige [Ühenduse Toiminguhalduri Log Analytics](../log-analytics/log-analytics-om-agents.md) 



### <a name="frequently-asked-questions"></a>Korduma kippuvad küsimused

+ Andmeid saab migreerida Log Analytics avaliku Azure'i abil Azure'i valitsuse?
  - Ei. Ei saa teisaldada andmeid või tööruumi avaliku Azure'i abil Azure'i Government.
+ Saate vaheldumisi aktiveerida avaliku Azure ja Azure Governmenti tööruumide portaalist OMS Log Analytics?
  - Ei. Portaalide Azure ja Azure Governmenti on eraldi ja teavet ühiskasutusse anda. 

Lisateavet leiate [Log Analytics avalike dokumentatsioonist](../log-analytics/log-analytics-overview.md).

## <a name="next-steps"></a>Järgmised sammud

Lisateave ja värskendused, tellimine on <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure'i Government ajaveeb.</a>
 
