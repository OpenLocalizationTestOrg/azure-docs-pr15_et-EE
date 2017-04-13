<properties
  pageTitle="Azure'i asjade komplekti ja loogika rakendused | Microsoft Azure'i"
  description="Õpetus, kuidas ühendada üles loogika rakenduste Azure asjade komplektile Äriprotsess."
  services=""
  suite="iot-suite"
  documentationCenter=""
  authors="aguilaaj"
  manager="timlt"
  editor=""/>

<tags
  ms.service="iot-suite"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="na"
  ms.date="08/16/2016"
  ms.author="araguila"/>
  
# <a name="tutorial-connect-logic-app-to-your-azure-iot-suite-remote-monitoring-preconfigured-solution"></a>Õpetus: Azure'i asjade komplekti jälgida eelkonfigureeritud lahendusse loogika rakenduse ühendamine

[Microsoft Azure'i asjade komplekti] [ lnk-internetofthings] remote jälgimise eelkonfigureeritud lahendus on suurepärane viis lõpuni funktsioonide komplekt, mis selgitab asjade lahenduse kasutamist kiiresti alustada. Selles õpetuses juhendab teid rakenduse loogika lisamine oma Microsoft Azure'i asjade komplekti remote jälgimise eelkonfigureeritud lahendus. Need juhised näitavad, kuidas saate oma asjade lahendus veelgi ühendatud äriprotsessi.

_Kui otsite lühiülevaade järelevalve Remote'i eelkonfigureeritud lahenduse sätte kohta, lugege teemat [õpetus: alustamine asjade eelkonfigureeritud lahenduste][lnk-getstarted]._

Enne selle õpetuse sammudega alustamist peaksite:

- Säte remote jälgimise eelkonfigureeritud lahendus Azure tellimuse.

- Selleks, et saaksite saata meilisõnumeid, mis käivitab oma Äriprotsess SendGrid konto loomine. Saate kasutajaks tasuta prooviversiooni konto [SendGrid](https://sendgrid.com/) , klõpsates nuppu **Proovi tasuta**. Kui olete registreerunud tasuta prooviversiooni konto, peate luua mõne [API võti](https://sendgrid.com/docs/User_Guide/Settings/api_keys.html) SendGrid, mis määrab õigused saata meilisõnumeid. Te vajate selle API võti hiljem õpetuse.

Eeldades, et te olete juba ette valmistatud, teie jälgida eelkonfigureeritud lahenduse, liikuge ressursirühm lahenduse [Azure portaali][lnk-azureportal]. Ressursirühma on sama nimi nimeks lahenduse valisite, kui te oma remote jälgimise lahendus ette valmistatud. Ressursirühm, näete ettevalmistatud Azure ressursse leiate Azure'i klassikaline portaalis Azure Active Directory rakendus, välja arvatud teie lahendus. Järgmine pilt on kuvatud näide **ressursirühm** höövlitera remote jälgimise eelkonfigureeritud lahendus.

![](media/iot-suite-logic-apps-tutorial/resourcegroup.png)

Alustamiseks häälestada loogika rakenduse kasutamiseks eelkonfigureeritud lahendus.

## <a name="set-up-the-logic-app"></a>Loogika rakenduse häälestamine

1. Klõpsake nuppu __Lisa__ oma ressursi rühma blade Azure portaali ülaosas.

2. __Loogika rakendust__otsida, valige see ja seejärel klõpsake nuppu **Loo**.

3. Täita __nime__ ja kasutage sama **tellimuse** ja **ressursirühm** , kui te ette valmistatud teie remote jälgimise lahendus. Klõpsake nuppu __Loo__.

    ![](media/iot-suite-logic-apps-tutorial/createlogicapp.png)

4. Kui teie juurutusega on lõpule jõudnud, saate vaadata rakenduse loogika on loetletud teie ressursirühm ressurssi.

5. Klõpsake loogika rakendust liikuge tera loogika rakendus, valige **Tühi loogika rakenduse** malli avamiseks **Loogika rakenduste Designer**.

    ![](media/iot-suite-logic-apps-tutorial/logicappsdesigner.png)

6. Valige __koosolekukutse__. See toiming määrab, et sissetulevad HTTP-päring koos kindla JSON vormindatud last toimingute käivitamiseks.

7. Kleepige koosolekukutse kehasse JSON skeemi järgmist:

    ```
    {
      "$schema": "http://json-schema.org/draft-04/schema#",
      "id": "/",
      "properties": {
        "DeviceId": {
          "id": "DeviceId",
          "type": "string"
        },
        "measuredValue": {
          "id": "measuredValue",
          "type": "integer"
        },
        "measurementName": {
          "id": "measurementName",
          "type": "string"
        }
      },
      "required": [
        "DeviceId",
        "measurementName",
        "measuredValue"
      ],
      "type": "object"
    }
    ```
    
    > [AZURE.NOTE] Pärast salvestamist loogika rakenduse, kuid esmalt peate lisama toimingu saate kopeerida HTTP postituse URL-i.

8. Klõpsake jaotises teie käsitsi päästik __+ Uus toiming__ . Seejärel klõpsake nuppu **Lisa toiming**.

    ![](media/iot-suite-logic-apps-tutorial/logicappcode.png)

9. Otsige **SendGrid - meilisõnumeid saata** ja klõpsake seda.

    ![](media/iot-suite-logic-apps-tutorial/logicappaction.png)

10. Sisestage nimi ühendus, nt **SendGridConnection**, **SendGrid API võti** luua, kui teie konto häälestamise SendGrid ja klõpsake nuppu **Loo**.

    ![](media/iot-suite-logic-apps-tutorial/sendgridconnection.png)

11. E-posti aadressid kuulub teile **nii **kaudu** kui ka** välju lisada. Lisage **Remote jälgimise teatise [DeviceId]** väljale **teema** . Lisage väljale **E-posti sisu** **seadme [DeviceId] on teatatud [measurementName], [measuredValue] väärtusega.** Saate lisada, klõpsates **saate sisestada andmed eelmisi juhiseid** jaotises **[DeviceId]**, **[measurementName]**ja **[measuredValue]** .

    ![](media/iot-suite-logic-apps-tutorial/sendgridaction.png)

12. Klõpsake ülemises menüüs nuppu __Salvesta__ .

13. Klõpsake **koosolekukutses** päästik ja kopeerige __Http Postita seda URL-i__ väärtust. Te vajate seda URL-i hiljem selles õpetuses.

> [AZURE.NOTE] Loogika rakendused võimaldavad [palju erinevaid toimingu] käivitada[ lnk-logic-apps-actions] sealhulgas Teenusekomplektis Office 365. 

## <a name="set-up-the-eventprocessor-web-job"></a>Seadistada EventProcessor Web töö

Selles jaotises saate ühendada oma eelkonfigureeritud lahenduse loogika rakenduse loodud. Selle toimingu tegemiseks saate lisada URL-i toiming, mis käivitub, kui seadme andur väärtus ületab läve loogika rakenduse käivitamine.

1. Kasutada oma git kliendi klooni uusim versioon on [asjade azure remote jälgimise github hoidla][lnk-rmgithub]. Näiteks:

    ```
    git clone https://github.com/Azure/azure-iot-remote-monitoring.git
    ```

2. Visual Studio, avage __RemoteMonitoring.sln__ hoidla kohaliku eksemplari.

3. Avage __ActionRepository.cs__ fail on **taristu\\hoidla** kausta.

4. Värskendage **actionIds** sõnastiku __Http Postita seda URL-i__ , saate märkida rakenduste loogika järgmiselt.

    ```
    private Dictionary<string,string> actionIds = new Dictionary<string, string>()
    {
        { "Send Message", "<Http Post to this UR>" },
        { "Raise Alarm", "<Http Post to this UR> }
    };
    ```

5. Muudatuste salvestamiseks klõpsake lahenduse ja sulgege Visual Studio.

## <a name="deploy-from-the-command-line"></a>Käsurea kaudu juurutamine

Selles jaotises juurutate oma remote jälgimise lahendus asendada praegu töötavate Azure versiooni värskendatud versiooni.

1. Pärast [arendaja ülesehituse] [ lnk-devsetup] keskkonna juurutamiseks häälestamise juhiseid.

2.  Kohalik juurutada, järgige [kohaliku juurutamise] [ lnk-localdeploy] juhiseid.

3.  Võtta kasutusele cloud ja värskendage oma olemasoleva pilvepõhise juurutuse tehke [pilve juurutamise] [ lnk-clouddeploy] juhiseid. Kasutada oma algse juurutuse nime juurutamise nime. Näiteks kui algne juurutamise kutsuti **demologicapp**, kasutada järgmine käsk:

    ``
    build.cmd cloud release demologicapp
    ``
    
    Kui Koosta skript käivitub, kasutage kindlasti sama Azure'i konto, tellimus, piirkond ja Active Directory eksemplar, kui teil on ette valmistatud lahendus.

## <a name="see-your-logic-app-in-action"></a>Vaadake oma loogika rakendust toiming

Järelevalve Remote'i eelkonfigureeritud lahendus on kaks reeglid, kui olete ette lahenduse vaikimisi häälestatud. **SampleDevice001** seadmes on nii reegleid.

* Temperatuur > 38.00
* Niiskus > 48.00

Temperatuur reegel käivitab **Tõsta** alarmtoiming ja niiskus reegel käivitab toimingu **SendMessage** . Eeldades, et kasutasite sama URL-i nii toimingud **ActionRepository** ainekursust, kas reegli käivitab rakenduse loogika. Nii reeglite abil SendGrid saatke meilisõnum **aadressile teatise üksikasjad** .

> [AZURE.NOTE] Loogika rakenduse jätkuvalt käivitamine iga kord, kui piirmäär on täidetud. Et vältida mittevajalike e-kirju, saate keelata reegleid oma lahenduse portaalis või keelata loogika rakenduse [Azure portaali][lnk-azureportal].

Lisaks saate e-kirju, näete ka, kui loogika rakendus töötab portaalis:

![](media/iot-suite-logic-apps-tutorial/logicapprun.png)

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete kasutanud loogika rakenduse eelkonfigureeritud lahendus ühenduse äriprotsessi, Lisateavet leiate teemast kohandamise eelkonfigureeritud lahenduste võimaluste kohta:

- [Dünaamiliste telemeetria kasutamine järelevalve Remote'i eelkonfigureeritud lahendus][lnk-dynamic]
- [Seadme teabe metaandmete järelevalve Remote'i eelkonfigureeritud lahendus][lnk-devinfo]

[lnk-dynamic]: iot-suite-dynamic-telemetry.md
[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[lnk-internetofthings]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-getstarted]: iot-suite-getstarted-preconfigured-solutions.md
[lnk-azureportal]: https://portal.azure.com
[lnk-logic-apps-actions]: ../connectors/apis-list.md
[lnk-rmgithub]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-devsetup]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/dev-setup.md
[lnk-localdeploy]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/local-deployment.md
[lnk-clouddeploy]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/cloud-deployment.md
