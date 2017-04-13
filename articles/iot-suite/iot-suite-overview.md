<properties
    pageTitle="Microsoft Azure'i asjade komplekti ülevaade | Microsoft Azure'i"
    description="Ülevaade sellest, kuidas Azure'i asjade komplekt, mis pakub Interneti asju eelkonfigureeritud lahendusi koguda, analüüsida ja andmete talletamiseks, sisestage visualiseeringud ja muudest süsteemidest integreerida."
    services=""
    suite="iot-suite"
    documentationCenter=""
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-suite"
     ms.devlang="na"
     ms.topic="get-started-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/09/2016"
     ms.author="dobett"/>

# <a name="what-is-azure-iot-suite"></a>Mis on Azure asjade komplekti?

Azure'i Interneti asjade teenuste pakub mitmesuguseid võimalusi. Need enterprise hinde teenused võimaldavad:

- Andmete kogumine seadmed
- Andmete voogu liikumise analüüs
- Salvestamine ja päringu suurte andmekogumite
- Reaalaja- ja ajalooliste andmete visualiseerimine
- Tagasi-Office'i süsteemide integreerimine

Esitamisel järgmisi võimalusi, Azure'i asjade komplekti paketid koos mitme Azure'i teenuste kohandatud laiendiga *eelkonfigureeritud*lahendusi. Need eelkonfigureeritud lahendused on levinud asjade lahenduse mustreid, mis aitab vähendada oma asjade lahenduste ajal base rakendusi. Kasutades [asjade tarkvara arengu komplektid][lnk-sdks], saate kohandada ja laiendada oma nõuete täitmiseks järgmisi lahendusi. Saate kasutada järgmisi lahendusi näiteid või mallide väljatöötamisel uusi asjade lahendusi.

Järgmisest videost leiate Azure'i asjade komplekti tutvustus:

> [AZURE.VIDEO azurecon-2015-introducing-the-microsoft-azure-iot-suite]

## <a name="azure-iot-services-in-azure-iot-suite"></a>Azure'i asjade teenuste Azure'i asjade komplektis

Eelkonfigureeritud lahendused kasutage tavaliselt järgmisi teenuseid.

- Azure'i asjade komplektile on [Azure asjade jaoturi] [ lnk-iot-hub] teenus. See teenus pakub seadme pilve ja pilveteenuse-seadme sõnumside võimaluste ja toimib lüüsi pilveteenuses ja muud võtme asjade komplekti teenused. Teenuse võimaldab sõnumeid, mis teie seadmest tasandil ja käsud saada seadmetes.

- [Azure'i voo Analytics] [ lnk-asa] pakub liikumise Andmeanalüüs. Asjade komplekti mõjutab see teenus töötlemine sissetulevate telemeetria, teha koondamine ja tuvasta sündmused. Eelkonfigureeritud lahenduste kasutada ka voo analytics töötlemine informatiivsed teated, mis sisaldavad andmeid, näiteks seadmete metaandmed või käsu vastused. Lahendused kasutage voo Analytics sõnumite teie seadmest ja muude teenuste need sõnumid pakkuda.

- [Azure'i salvestusruumi] [ lnk-azure-storage] ja [Azure DocumentDB] [ lnk-document-db] andmete salvestusruumi võimalusi pakkuda. Eelkonfigureeritud lahendused kasutage bloobimälu telemeetria talletamiseks ja analüüsi jaoks kättesaadavaks. Lahendused kasutage DocumentDB salvestada seadme metaandmete ja lubada seadme halduse võimaluste lahendusi.

- [Azure'i veebirakenduste] [ lnk-web-apps] ja [Microsoft Power BI] [ lnk-power-bi] andmete visualiseerimise võimalusi pakkuda. Power BI paindlikkust abil saate kiiresti koostada oma interaktiivsed armatuurlauad asjade komplekti andmeid kasutavate.

Tüüpilised asjade lahenduse arhitektuur ülevaate leiate [Microsoft Azure'i ja selle Internet, asjade][iot-suite-what-is-azure-iot].

## <a name="preconfigured-solutions"></a>Eelkonfigureeritud lahendused

Asjade komplekti sisaldab eelkonfigureeritud lahendusi, mis võimaldavad kiiresti alustada ning uurida asjade Levinumad stsenaariumid, nt *jälgida* ja *sõnastikupõhise hooldustööd*, mis võimaldab Azure'i asjade komplekti. Saate juurutada järgmisi lahendusi Azure tellimuse ja seejärel käivitage täielik, lõpuni asjade stsenaarium.

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui teil on ülevaate asjade komplekti saate teha ja millised on selle komponendid, saate teavet eelkonfigureeritud lahenduste asjade komplektis kohta leiate teemast [mis on Azure asjade eelkonfigureeritud lahendusi?][lnk-what-are-preconfig]

[lnk-sdks]: https://azure.microsoft.com/documentation/articles/iot-hub-sdks-summary/
[lnk-iot-hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-azure-storage]: https://azure.microsoft.com/documentation/services/storage/
[lnk-document-db]: https://azure.microsoft.com/documentation/services/documentdb/
[lnk-power-bi]: https://powerbi.microsoft.com/
[lnk-web-apps]: https://azure.microsoft.com/documentation/services/app-service/web/
[iot-suite-what-is-azure-iot]: iot-suite-what-is-azure-iot.md
[lnk-what-are-preconfig]: iot-suite-what-are-preconfigured-solutions.md
