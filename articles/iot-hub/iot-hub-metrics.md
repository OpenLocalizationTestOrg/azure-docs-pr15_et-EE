<properties
 pageTitle="Asjade jaoturi diagnostika mõõdikute"
 description="Azure'i asjade jaoturi mõõdikute, mis võimaldab kasutajatel hinnata nende ressursside üldise seisundi ülevaade"
 services="iot-hub"
 documentationCenter=""
 authors="nberdy"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/11/2016"
 ms.author="nberdy"/>

# <a name="introduction-to-diagnostic-metrics"></a>Diagnostika mõõdikute tutvustus

Diagnostika mõõdikute teile parem andmeid Azure ressursside olek teie Azure'i tellimus. Mõõdikute võimaldavad hinnata teenuse ja ühendatud seadmete üldist tervist. Kasutaja suunatud statistika on olulised, sest need aitavad teil näha, mis toimub oma asjade jaoturi ja abi-põhjus probleemide ilma Azure tugiteenuste.

Saate lubada diagnostika mõõdikute Azure portaalist.

## <a name="how-to-enable-diagnostic-metrics"></a>Kuidas lubada diagnostika mõõdikud

1. Soovitud asjade keskuse loomine. Juhised leiate soovitud asjade jaoturi loomist saate [Alustada] [ lnk-get-started] juhend.

2. Avage oma asjade jaoturi tera. Klõpsake jaotises **diagnostika**.

    ![][1]

3. Konfigureerida oma diagnostika säte olek **sisse** ja valige konto Azure Storage diagnostika andmete talletamiseks. Märkige ruut **mõõdikute**ja vajutage **Salvesta**. Pange tähele, et Azure Storage konto peab olema loodud ette valmistada ja et teil on maksta eraldi Storage. Saate saata diagnostika andmete sündmuse jaoturi lõpp.

    ![][2]

4. Pärast diagnostika naasmiseks **Ülevaade** asjade jaoturi tera. Mõõdikute teabe sisustatakse tera jaotises **jälgimine** . Klõpsake diagrammi, avaneb mõõdikute paani, kus saate oma asjade jaoturi kokkuvõtte mõõdikute teabe vaatamine ja redigeerimine valikut mõõdikute diagrammil kuvatavate. Samuti saate konfigureerida teatiste argumendil väärtuste põhjal.

    ![][3]

## <a name="metrics-and-how-to-use-them"></a>Mõõdikute ja kuidas neid kasutada

Asjade jaoturi pakub teile ülevaate oma jaoturi ja ühendatud seadmete koguarvu mitu mõõdikute. Saate ühendada teavet mitme mõõdikute asjade jaoturi riigi suurem pilt. Järgmises tabelis kirjeldatakse iga asjade jaoturi jälitab mõõdikud ja kuidas iga mõõdiku seotud asjade jaoturi üldise oleku.

| Meetermõõdustik | Argumendil kirjeldus | Mõõdiku kasutatud |
| ---- | ---- | ---- |
| D2C.telemetry.Ingress.allProtocol | Kõigis seadmetes saadetud sõnumite arv | Ülevaade andmete sõnumi saadab |
| D2C.telemetry.Ingress.Success | Funktsiooni jaoturiga eduka kõigi sõnumite arv | Eduka sõnumi sissepääsu selle jaoturiga ülevaade |
| C2D.Commands.egress.Complete.Success | Kõigis seadmetes erineda lõpule kõik käsu sõnumite arv | Koos mõõdikute loobuma ja Hülga, annab ülevaate üldine C2D käsk edukust |
| C2D.Commands.egress.Abandon.Success | Kõigi sõnumite edukalt hüljatud erineda kõigis seadmetes arv | Võimalikud probleemid tõstab esile, kui sõnumeid saada hüljatud sagedamini oodatust |
| C2D.Commands.egress.Reject.Success | Kõigi sõnumite vastuvõtmise seadme kõigis seadmetes edukalt tagasi lükatud arv | Kui sõnumid on saada sagedamini oodatust tagasi lükatud, tõstab esile võimalikud probleemid |
| devices.totalDevices | Keskmine, min ja max asjade jaoturi registreeritud seadmete arv | Seadmete arv registreeritud keskus |
| devices.connectedDevices.allProtocol | Keskmine, min ja max samaaegne ühendatud seadmete arv | Jaoturi ühendatud seadmete arvu ülevaade |

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete näinud diagnostika mõõdikute ülevaate, tehke selle lingi Lisateave Azure'i asjade jaoturi haldamise kohta.

- [Toimingute jälgimine][lnk-monitor]

Asjade jaoturi võimaluste täpsemaks analüüsimiseks järgmistest teemadest.

- [Arendaja juhend][lnk-devguide]
- [Simuleerida seadme asjade lüüsi SDK][lnk-gateway]

<!-- Links and images -->
[1]: media/iot-hub-metrics/enable-metrics-1.png
[2]: media/iot-hub-metrics/enable-metrics-2.png
[3]: media/iot-hub-metrics/enable-metrics-3.png

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[lnk-operations-monitoring]: iot-hub-operations-monitoring.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-dr]: iot-hub-ha-dr.md

[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
