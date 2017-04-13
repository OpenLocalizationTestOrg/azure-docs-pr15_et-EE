<properties
    pageTitle="Kohandamise eelkonfigureeritud lahendused | Microsoft Azure'i"
    description="Kuidas kohandada Azure'i asjade komplekti eelkonfigureeritud lahenduste annab juhiseid."
    services=""
    suite="iot-suite"
    documentationCenter=".net"
    authors="aguilaaj"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-suite"
     ms.devlang="dotnet"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="10/11/2016"
     ms.author="aguilaaj"/>

# <a name="customize-a-preconfigured-solution"></a>Eelkonfigureeritud lahenduse kohandamine

Eelkonfigureeritud lahendusi, Azure'i asjade Suite kujutavad teenuste komplekti, koostöö-lõpuni lahenduse esitamisel. Alustuseks, on mitmesuguseid kohad, kus saate laiendada ja kohandada teatud stsenaariumide lahendus. Järgmistes jaotistes kirjeldatakse neid levinud kohandamine punkte.

## <a name="finding-the-source-code"></a>Lähtekoodi otsimine

Lähtekoodi eelkonfigureeritud lahenduste jaoks on saadaval järgmiste hoidlate github:

- Remote jälgimine: [https://www.github.com/Azure/azure-iot-remote-monitoring](https://github.com/Azure/azure-iot-remote-monitoring)
- Ennustav hooldus: [https://github.com/Azure/azure-iot-predictive-maintenance](https://github.com/Azure/azure-iot-predictive-maintenance)

Lähtekoodi eelkonfigureeritud lahenduste jaoks on esitatud mustrite ja rakendada lõpuni funktsioone lahenduse asjade Azure'i asjade komplekti kasutamise tavade. Siit leiate lisateavet selle kohta, kuidas luua ja juurutada lahenduste GitHub hoidlate.

## <a name="changing-the-preconfigured-rules"></a>Eelkonfigureeritud reeglite muutmine

Remote jälgimise lahendus sisaldab kolm [Azure'i voo Analytics](https://azure.microsoft.com/services/stream-analytics/) tööd seadme teabe, telemeetria ja reeglid loogika kuvatakse lahenduse kasutusele võtta.

Kolm voo analytics töö ja nende süntaks on kirjeldatud sügavus on [Remote eelkonfigureeritud lahenduse kiirtutvustus jälgimine](iot-suite-remote-monitoring-sample-walkthrough.md). 

Saate need otse muutma loogika tööd redigeerimine või teatud loogika lisamine stsenaariumist. Leiate voo Analytics töö järgmiselt:
 
1. Avage [Azure'i portaal](https://portal.azure.com).
2. Liikuge oma asjade lahenduseks sama nimega ressursirühma. 
3. Valige Azure voo Analytics töö, mida soovite muuta. 
4. Lõpetage töö, valides **Peata**käskude. 
5. Redigeerige sisendina, päringu ja väljundeid.

    Lihtsa muudatusega on **reeglid** töö kasutada päringut muuta mõne **"<"** asemel on **">"**. Lahendus portaali siiski **">"** kui reeglit redigeerida, kuid märkate käitumise on peegeldatud aluseks oleva töö muudatuste tõttu.

6. Töö alustamine

> [AZURE.NOTE] Remote jälgimisega seotud armatuurlaua sõltub konkreetsed andmed, muutmata tööd, võivad põhjustada armatuurlaua nurjumise.

## <a name="adding-your-own-rules"></a>Lisamine oma reeglid

Lisaks eelkonfigureeritud Azure'i voo Analytics töö muutmisele saate Azure portaali lisada uue töid või lisada uutele päringutele olemasolevate projektide.

## <a name="customizing-devices"></a>Seadmete kohandamine

Kõige levinum laiend tegevust töötab teatud stsenaariumist seadmed. On mitu võimalust töötamiseks seadmed. Nende meetodite hulka muutmata jäljendatud seade, et need vastaksid teie stsenaariumi või füüsilise seadme ühendamine lahendus [Asjade seadme SDK][] abil.

Seadmete lisamisega järelevalve Remote'i eelkonfigureeritud lahendus samm-sammult juhendi leiate teemadest [Asjade komplekti ühenduse seadmed](iot-suite-connecting-devices.md) ja [Remote jälgimisega seotud C SDK näidised](https://github.com/Azure/azure-iot-sdks/tree/master/c/serializer/samples/remote_monitoring) , mis on mõeldud töötamiseks järelevalve Remote'i eelkonfigureeritud lahendus.

### <a name="creating-your-own-simulated-device"></a>Luua oma modelleerida seade

Remote jälgimisega seotud lahenduse lähtekoodi (viidatud kohal) sisaldab, on .NET simulaatorit. See simulaatorit on üks lahendus osana ette valmistatud ja saab muuta erinevate metaandmete telemeetria saatmiseks või muu käske.

Eelkonfigureeritud simulaatorit remote jälgimisega seotud eelkonfigureeritud lahendus on jahedam seade, mis eraldab temperatuuri- ja telemeetria, saate muuta simulaatorit [Simulator.WebJob](https://github.com/Azure/azure-iot-remote-monitoring/tree/master/Simulator/Simulator.WebJob) Projectis, kui te olete kahestunud GitHub hoidla.

### <a name="available-locations-for-simulated-devices"></a>Asukoht saadaolevate asukohtade jäljendatud seadmete jaoks

Vaikimisi asukohad on Seattle/Redmond, Washington, Ameerika Ühendriigid. Saate muuta nende asukohta [SampleDeviceFactory.cs][lnk-sample-device-factory].


### <a name="building-and-using-your-own-physical-device"></a>Koostamise ja (füüsilise) seadme kaudu

[Azure'i asjade SDK-d](https://github.com/Azure/azure-iot-sdks) pakuvad teekide jaoks ühenduse mitme seadme tüüpi (keeled ja operatsioonisüsteemid) asjade lahendusi.

## <a name="modifying-dashboard-limits"></a>Muutmine armatuurlaua piirangud

### <a name="number-of-devices-displayed-in-dashboard-dropdown"></a>Armatuurlaua ripploendis kuvatavad arv

Vaikimisi on 200. Saate muuta selle arv [DashboardController.cs][lnk-dashboard-controller].

### <a name="number-of-pins-to-display-in-bing-map-control"></a>Arv kontakte kuvamiseks Bingi kaardi juhtelement

Vaikimisi on 200. Saate muuta selle arv [TelemetryApiController.cs][lnk-telemetry-api-controller-01].

### <a name="time-period-of-telemetry-graph"></a>Aja jooksul telemeetria graafik

Vaikimisi on 10 minuti. Saate muuta selle [TelmetryApiController.cs][lnk-telemetry-api-controller-02].

## <a name="manually-setting-up-application-roles"></a>Käsitsi häälestada rakenduse rollid

Järgnevalt kirjeldatakse, kuidas **administraator** ja **kirjutuskaitstud** rakenduse rollide lisamiseks eelkonfigureeritud lahenduse. Pidage meeles, et saidilt azureiotsuite.com juba ette valmistatud eelkonfigureeritud lahenduste **haldus** ja **kirjutuskaitstud** rollid.

**Kirjutuskaitstud** rolli liikmed näevad armatuurlaud ja seadmete loend, kuid ei ole lubatud seadmete lisamine, muutmine seadme atribuutide või saata käske.  **Administraatori** rolli liikmed on täielik juurdepääs kõik funktsioonid lahendus.

1. Avage [Azure'i klassikaline portaali][lnk-classic-portal].

2. Valige **Active Directory**.

3. Klõpsake selle nime, saate kasutada, kui teil on ette valmistatud teie lahendus AAD rentniku.

4. Klõpsake nuppu **rakendused**.

5. Klõpsake rakenduse, mis vastab teie eelkonfigureeritud lahenduse nimi nimi. Kui te ei näe teie rakenduste loendis, valige **minu ettevõte kuulub rakenduste** **kuvamine** rippmenüüst ja seejärel klõpsake märkeruutu.

6.  Klõpsake lehe allosas nuppu **Haldamine näidata** ja seejärel **Alla laadida näidata**.

7. See .json faili oma arvutisse alla laadida.  Avage seda faili redigeerimiseks tekstiredaktoris oma valik.

8. Kolmanda rea .json faili, leiate:

  ```
  "appRoles" : [],
  ```
  Asendage see järgmist:

  ```
  "appRoles": [
  {
  "allowedMemberTypes": [
  "User"
  ],
  "description": "Administrator access to the application",
  "displayName": "Admin",
  "id": "a400a00b-f67c-42b7-ba9a-f73d8c67e433",
  "isEnabled": true,
  "value": "Admin"
  },
  {
  "allowedMemberTypes": [
  "User"
  ],
  "description": "Read only access to device information",
  "displayName": "Read Only",
  "id": "e5bbd0f5-128e-4362-9dd1-8f253c6082d7",
  "isEnabled": true,
  "value": "ReadOnly"
  } ],
  ```

9. Salvestage värskendatud .json fail (saate üle kirjutada olemasolev fail).

10.  Azure'i haldusportaal, klõpsake lehe allosas valige **Haldamine näidata** seejärel **Laadida näidata** .json eelmises etapis salvestatud faili üles laadida.

11. Nüüd olete lisanud rakenduse **administraator** ja **kirjutuskaitstud** rollid.

12. Üks rollidest määramiseks kataloogis kasutaja, lugege teemat [azureiotsuite.com saidiõigused][lnk-permissions].

## <a name="feedback"></a>Tagasiside

Kas teil on kohandamine soovite näha lepinguga seotud selles dokumendis? Lisage funktsiooni soovitusi [Kasutaja heli](https://feedback.azure.com/forums/321918-azure-iot)või kommentaari selle artikli allpool. 

## <a name="next-steps"></a>Järgmised sammud

Suvandite kohandamine eelkonfigureeritud lahenduste kohta leiate lisateavet järgmistest teemadest.

- [Loogika rakenduse ühenduse oma Azure'i asjade komplekti jälgida eelkonfigureeritud lahendus][lnk-logicapp]
- [Dünaamiliste telemeetria järelevalve Remote'i eelkonfigureeritud lahendusega kasutamine][lnk-dynamic]
- [Seadme teabe metaandmete järelevalve Remote'i eelkonfigureeritud lahendus][lnk-devinfo]

[lnk-logicapp]: iot-suite-logic-apps-tutorial.md
[lnk-dynamic]: iot-suite-dynamic-telemetry.md
[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[Asjade seadme SDK]: https://azure.microsoft.com/documentation/articles/iot-hub-sdks-summary/
[lnk-permissions]: iot-suite-permissions.md
[lnk-dashboard-controller]: https://github.com/Azure/azure-iot-remote-monitoring/blob/3fd43b8a9f7e0f2774d73f3569439063705cebe4/DeviceAdministration/Web/Controllers/DashboardController.cs#L27
[lnk-telemetry-api-controller-01]: https://github.com/Azure/azure-iot-remote-monitoring/blob/3fd43b8a9f7e0f2774d73f3569439063705cebe4/DeviceAdministration/Web/WebApiControllers/TelemetryApiController.cs#L27
[lnk-telemetry-api-controller-02]: https://github.com/Azure/azure-iot-remote-monitoring/blob/e7003339f73e21d3930f71ceba1e74fb5c0d9ea0/DeviceAdministration/Web/WebApiControllers/TelemetryApiController.cs#L25 
[lnk-sample-device-factory]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Common/Factory/SampleDeviceFactory.cs#L40
[lnk-classic-portal]: https://manage.windowsazure.com
