<properties
    pageTitle="DocumentDB portaali tõrkeotsing | Microsoft Azure'i"
    description="Siit saate teada DocumentDB Azure'i portaalis probleemide lahendamiseks." 
    services="documentdb"
    documentationCenter=""
    authors="mimig1"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="mimig"/>

# <a name="azure-documentdb-portal-troubleshooting-tips"></a>Azure'i DocumentDB portaali tõrkeotsingu näpunäited

Selles artiklis kirjeldatakse, kuidas Azure'i portaalis DocumentDB probleemide lahendamiseks. 

## <a name="resources-are-missing"></a>Ressursid on puudu

**Sümptom**: andmebaasides ja saidikogumite puuduvad oma portaali labad.

**Lahendus**: rakenduse kasutamine all kogumise talletusmahu maksimaalse läbilaskevõime langetada. 

**Selgitus**: portaali on rakendus, nagu kõik teised, helistamist DocumentDB andmebaasi ja saidikogumi. Kui teie taotlused on praegu on rakendus tõttu kõned tehtud eraldi rakendusest, portaali võib-olla ka olema rakendus, põhjustab ressursse, mis ei kuvata portaalis. Selle probleemi lahendamiseks kõrge läbilaskevõime kasutuse põhjus aadress ja seejärel värskendage portaali tera. [Läbilaskevõime](documentdb-performance-tips.md#throughput) osas [jõudluse näpunäiteid](documentdb-performance-tips.md) artiklist leiate teavet, kuidas mõõta ja alumise läbilaskevõime kasutuse.
 
## <a name="pages-or-blades-wont-load"></a>Lehtede või labad ei laadita

**Sümptom**: lehtede ja labad portaalis ei kuvata.

**Lahendus**: rakenduse kasutamine all kogumise talletusmahu maksimaalse läbilaskevõime langetada. 

**Selgitus**: portaali on rakendus, nagu kõik teised, helistamist DocumentDB andmebaasi ja saidikogumi. Kui teie taotlused on praegu on rakendus tõttu kõned tehtud eraldi rakendusest, portaali võib-olla ka olema rakendus, põhjustab ressursse, mis ei kuvata portaalis. Selle probleemi lahendamiseks kõrge läbilaskevõime kasutuse põhjus aadress ja seejärel värskendage portaali tera. [Läbilaskevõime](documentdb-performance-tips.md#throughput) osas [jõudluse näpunäiteid](documentdb-performance-tips.md) artiklist leiate teavet, kuidas mõõta ja alumise läbilaskevõime kasutuse.

## <a name="add-collection-button-is-disabled"></a>Lisage saidikogumi nupp on keelatud.

**Sümptom**: enne andmebaasi, nupp **Lisa saidikogumi** on keelatud.

**Selgitus**: kui Azure tellimuse on seostatud kasuks tiitrid, nagu MSDN-i tellimuse pakutakse tasuta autorid ja te olete kasutanud kõik teie krediiti kuu, ei saa luua mis tahes täiendavaid saidikogumid DocumentDB.

**Lahendus**: kulutuste limiit eemaldamine kontolt.

1. Azure'i portaalis Jumpbar, klõpsake nuppu **tellimused**, klõpsake tellimusega seostatud DocumentDB andmebaas ja **tellimuse** tera, klõpsake käsku **Halda**. 
    ![DocumentDB pakub mitme, määratletud (lõdvestunud) järjepidevuse mudelid](./media/documentdb-portal-troubleshooting/documentdb-change-billing.png)

2. Uues brauseriaknas, näete, et teil pole krediiti jäänud. Nuppu **Eemalda kulutuste limiit** eemaldada ainult praeguse arveldusperioodi eest või lõputult kulutuste. Järgige viisardi lisamiseks või kinnitage oma krediitkaardi andmed. 
    ![DocumentDB pakub mitme, määratletud (lõdvestunud) järjepidevuse mudelid](./media/documentdb-portal-troubleshooting/documentdb-remove-spending-limit.png)

 
## <a name="query-explorer-completes-with-errors"></a>Päringu Explorer lõpetab vigadega

Lugege teemat [tõrkeotsing päringu Explorer](documentdb-query-collections-query-explorer.md#troubleshoot).

## <a name="no-data-available-in-monitoring-tiles"></a>Pole andmeid, mis on saadaval paanid jälgimine

Lugege teemat [tõrkeotsing jälgimise paanid](documentdb-monitor-accounts.md#troubleshooting).

## <a name="no-documents-returned-in-document-explorer"></a>Dokumendi Exploreris tagastatud dokumendid

Vaadake teemat [tõrkeotsing dokumendi Explorer](documentdb-view-json-document-explorer.md#troubleshoot).

## <a name="next-steps"></a>Järgmised sammud

Kui teil on endiselt probleeme portaalis, saatke [askdocdb@microsoft.com](mailto:askdocdb@microsoft.com) abi või faili tugi taotlemine portaalis, klõpsates **sirvida**, **abi + tugi**, ja seejärel klõpsata käsku **Loo tugiteenuse taotluse**.
