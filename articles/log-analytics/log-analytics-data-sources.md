<properties 
   pageTitle="Andmeallikate Log Analytics | Microsoft Azure'i"
   description="Andmeallikate määratleda, et Log Analytics kogub agentide ja teiste ühendatud allikatest andmeid.  Selles artiklis kirjeldatakse, kuidas Log Analytics kasutab andmeallikad, selgitatakse üksikasjad, kuidas konfigureerida neid ja annab ülevaate erinevatest andmeallikatest, mis on saadaval."
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

# <a name="data-sources-in-log-analytics"></a>Log Analytics andmeallikad

Log Analytics kogub ühendatud teie OMS tööruumis allikatest pärit andmeid ja talletab selle OMS hoidla.  Iga kogutud andmed on määratletud saate konfigureerida andmeallikad.  Kirjete kogumina talletatakse andmeid OMS hoidla.  Iga andmeallika loob teatud tüüpi kirjed oma atribuutide komplekti, millel on iga tüübiga.

![Logige Analytics andmete kogumine](./media/log-analytics-data-sources/overview.png)

Andmeallikad on erinevas OMS lahendusi, mis lisaks ühendatud allikatest pärit andmete kogumise ja OMS hoidlas kirjete loomine.  Lahenduste saab lisada oma tööruumi lahendusegaleriist ja annab tavaliselt täiendavad tööriistad OMS portaalis.  

## <a name="summary-of-data-sources"></a>Andmeallikad

Järgmises tabelis on loetletud Log Analytics praegu saadaolevad andmeallikad.  Iga on link eraldi artikkel, pakkudes selle andmeallika üksikasjad.

| Andmeallikas | Sündmuse tüüp | Kirjeldus |
|:--|:--|:--|
| [Kohandatud logid](log-analytics-data-sources-custom-logs.md) | \<LogName\>_CL | Klõpsake Windowsi või Linuxi agentide, mis sisaldab Logiteave tekstifailidest. |
| [Windowsi sündmuste logid](log-analytics-data-sources-windows-events.md) | Sündmuse | Sündmuste kogutud sündmuste logi Windowsi arvuti. |
| [Windowsi jõudlus hinnale](log-analytics-data-sources-performance-counters.md) | Täiuslik | Jõudluse hinnale kogutud Windowsi arvutisse. |
| [Linux jõudluse hinnale](log-analytics-data-sources-performance-counters.md) | Täiuslik | Jõudluse hinnale kogutud Linuxi arvutites. |
| [IIS-i logid](log-analytics-data-sources-iis-logs.md) | W3CIISLog | W3C vormingus logib teenusekomplekti Internet Information Services. |
| [Logi](log-analytics-data-sources-syslog.md) | Logi | Logi sündmuste Windowsi või Linuxi arvutites. |

## <a name="configuring-data-sources"></a>Andmeallikate konfigureerimine

Saate konfigureerida andmeallikate menüü **andmed** Log Analytics **sätted**.  Konfiguratsiooni saadetakse kõigile tööruumi OMS ühendatud andmeallikatele.  Praegu ei saa mis tahes agentide välistamine selle konfiguratsiooni.

![Windowsi sündmuste konfigureerimine](./media/log-analytics-data-sources/configure-events.png)

2. Valige OMS konsooli paani **sätted** .
3. Valige **andmed**.
4. Klõpsake andmeallika konfigureerida.
5. Klõpsake linki dokumentatsioonist Lisateavet ülaltoodud tabelis iga andmeallika oma konfiguratsiooni.

## <a name="data-collection"></a>Andmete kogumine

Andmete allikas konfiguratsioone toimetatakse agentide, mis on ühendatud otse OMS mõne minuti jooksul.  Määratud andmete kogumise agendi ja otse kohale Log Analytics iga andmeallika kindla intervalliga.  Iga andmeallika need täpsemalt dokumentatsioonist.

Ühendatud haldus jaotises System Center toimingute Manager (SCOM) agentide andmete allikas konfiguratsioone on tõlkida halduse tuvastamine ja haldus rühma iga 5 minuti järel vaikimisi on esitatud.  Agent halduspaketi nagu muud allalaaditavate failide ja kogub määratud andmeid. Sõltuvalt andmeallika andmed on kas saadetud management server Log Analytics andmete andev või agent saadab Log Analytics andmete ilma management server. Vaadake lisateavet [andmete kogumise üksikasjad OMS funktsioone ja lahendusi](log-analytics-add-solutions.md#data-collection-details-for-oms-features-and-solutions) .  Saate lugeda üksikasjad SCOM ja OMS ühendamine ja sageduse muutmine selle konfiguratsiooni juures [Konfigureerimine integreerimine System Center Operations Manager](log-analytics-om-agents.md)on kohale toimetatud.

## <a name="log-analytics-records"></a>Kasutusanalüüsi kirjed

Log Analytics kogutud kõik andmed on salvestatud OMS hoidla kirjetena.  Kogutud erinevatest andmeallikatest kirjed on oma atribuutide komplekti, ja nende **tüüpi** atribuuti identifitseerib.  Vaadake teemat kindlat tüüpi kirje iga andmeallika dokumentatsioonist ja lahendus üksikasjad.


## <a name="next-steps"></a>Järgmised sammud

- Lisateavet [lahendusi](log-analytics-add-solutions.md) , mis Log Analytics funktsioone lisada ka koguda andmeid OMS hoidlasse.
- Lisateavet [log otsingud](log-analytics-log-searches.md) andmeallikate ja lahenduste kogutud andmete analüüsimiseks.  
- Konfigureerida [teatiste](log-analytics-alerts.md) aktiivselt märku kriitiliste andmete andmeallikate ja lahendusi.
