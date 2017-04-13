<properties
    pageTitle="Logige Analytics funktsioonid pakkujate | Microsoft Azure'i"
    description="Log Analytics aitab hallata teenuse pakkujad (MSPs) suurettevõtete, sõltumatu tarkvara tarnijatele (tarkvaratoode) ja majutusteenuse teenusepakkujatele hallata ja jälgida serverite kohapealse kliendi või pilvetaristu."
    services="log-analytics"
    documentationCenter=""
    authors="richrundmsft"
    manager="jochan"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/25/2016"
    ms.author="richrund"/>

# <a name="log-analytics-features-for-service-providers"></a>Logige analüüsi funktsioonide jaoks teenuse pakkujad

Log Analyticsi abil hallatavad teenusepakkujatele (MSPs), suurettevõtete, sõltumatu tarkvara tarnijate (tarkvaratoode) ja majutusteenuse teenusepakkujatele hallata ja jälgida serverite kohapealse kliendi või pilvetaristu. 

Suurtes ettevõtetes ühiskasutusse palju sarnasusi teenusepakkujatele, eriti siis, kui seal on mõne keskse see meeskonnatöö on see paljude erinevate business üksuste haldamise eest. Lihtsa, see dokument kasutab Termini *teenusepakkuja* , kuid samu funktsioone on saadaval enterprises ja teiste klientide.

## <a name="cloud-solution-provider"></a>Pilve lahenduste pakkuja

Partnerid ja teenuse pakkujad, kes kuuluvad [Pilve lahenduse pakkuja (CSP)](https://partner.microsoft.com/Solutions/cloud-reseller-overview) programmi, Log Analytics on üks Azure pakutavaid CSP tellimuse. 

Log Analyticsi, *Pilve lahenduste pakkuja* tellimuste lubatud järgmised võimalused.

*Pilve lahenduste pakkuja* saate teha järgmist.

+ Log Analytics tööruumide rentniku (klient) tellimus.
+ Accessi tööruumid, mis on loodud rentnikud. 
+ Lisada ja eemaldada kasutaja juurdepääs tööruumi kasutamise Azure'i kasutajate haldamine. Kui soovitud rentniku tööruumis OMS portaalis kasutaja halduse lehel jaotises sätted pole saadaval
  - Log Analytics ei toeta Rollipõhine pääsu veel - annab kasutaja `reader` õigus Azure'i portaalis võimaldab konfiguratsiooni muudatuste OMS portaalis

A rentniku tellimuse sisse logida, peate määrama rentniku identifikaator. Rentniku identifikaator on sageli selle Viimane osa sisselogimiseks kasutatav meiliaadress.

+ OMS portaalis lisamine `?tenant=contoso.com` portaali URL. Näiteks`mms.microsoft.com/?tenant=contoso.com`
+ Kasutage PowerShellis olevat `-Tenant contoso.com` parameetri kasutamisel `Add-AzureRmAccount` cmdlet-käsk
+ Rentniku identifikaator lisatakse automaatselt, kui kasutate funktsiooni `OMS portal` link Azure'i portaalis avamiseks ja valitud tööruumi OMS portaali sisse logida

*Kliendi* pilve lahenduste pakkuja, saate teha järgmist.

+ Tööruumide log analytics CSP tellimus
+ Accessi tööruumide loodud CSP-le
  -  Kasutage funktsiooni `OMS portal` link Azure'i portaalis avamiseks ja valitud tööruumi OMS portaali sisse logida
+ Saate vaadata ja kasutada kasutaja halduse lehel jaotises sätted OMS portaalis

>[AZURE.NOTE] Log Analyticsi varundamine ja taastamine saidi lahendused ei saa ühenduse taastamine teenuste vault ja ei saa konfigureerida CSP tellimus.

## <a name="managing-multiple-customers-using-log-analytics"></a>Mitme klientidele, kes kasutavad Log Kasutusanalüüsi haldamine 

Soovitatav on Log Analytics tööruumi iga kliendi haldate loomine. Tööruumi Log Analytics pakub.

+ Andmete talletamise geograafilise asukoha. 
+ Granulaarsus arveldamine 
+ Andmete eraldi 
+ Kordumatu konfigureerimine

Kliendi kohta tööruumi loomine, teil on võimalik iga kliendi andmeid eraldi hoida ja jälgida ka iga kliendi kasutamist.

Lisateavet selle kohta, millal ja miks mitme tööruumide on kirjeldatud [logige analytics juurdepääsu haldamine] (log-analytics-manage-access.md#determine-the-number-of-workspaces-you-need).

Loomine ja konfigureerimine kliendi tööruumid automatiseerida [PowerShelli](log-analytics-powershell-workspace-configuration.md), [ressursihaldur mallide](log-analytics-template-workspace-configuration.md)abil või [REST API](https://www.nuget.org/packages/Microsoft.Azure.Management.OperationalInsights/)abil.

Ressursihaldur mallide kasutamine tööruumi konfiguratsioon võimaldab teil on juhtslaidi konfiguratsiooni, mida saab kasutada loomiseks ja konfigureerimiseks tööruumid. Võite olla kindel, klientide loomisel tööruumide automaatselt konfigureeritud oma nõuetele. Kui värskendate oma nõuetele, mall on värskendatud ja seejärel uuesti olemasoleva tööruumide. Selle protsessi tagab isegi olemasoleva tööruumid teie uus nõuetele..    

Kui mitmest telefonilogi Analytics tööruumide haldamine soovitame iga tööruumi integreerimise teie olemasoleva piletimüügi süsteemi / toimingud konsooli kasutades [teatiste](log-analytics-alerts.md) funktsiooni. Kasutades oma olemasolevate süsteemidega, saate toe töötajate jätkake oma tuttavad protsessid. Log Analytics regulaarselt iga tööruumi saate määrata teatiste kriteeriumide kontrollib ja loob teatise, kui on vaja.

Ametlik taseme aruannete mis summeeritavaid andmeid üle tööruumide saate kasutada Log Analytics ja [PowerBI](log-analytics-powerbi.md)integreerimine. Kui vajate mõne muu süsteemi integreerida, saate otsingu API (PowerShelli või [REST](log-analytics-log-search-api.md)) kaudu päringute sooritamine ja otsingutulemite eksportimine.

## <a name="next-steps"></a>Järgmised sammud

+ Automatiseerida loomine ja konfigureerimine tööruumide [ressursihaldur mallide](log-analytics-template-workspace-configuration.md) kasutamine
+ [PowerShelli](log-analytics-powershell-workspace-configuration.md) kasutamine tööruumide loomise automatiseerida 
+ [Teatiste](log-analytics-alerts.md) abil saate integreerida olemasolevate
+ Kokkuvõte aruandeid [PowerBI](log-analytics-powerbi.md) abil
