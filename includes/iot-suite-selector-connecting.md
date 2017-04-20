> [AZURE.SELECTOR]
- [C Windows](../articles/iot-suite/iot-suite-connecting-devices.md)
- [C Linux](../articles/iot-suite/iot-suite-connecting-devices-linux.md)
- [C mbed](../articles/iot-suite/iot-suite-connecting-devices-mbed.md)
- [Node.js](../articles/iot-suite/iot-suite-connecting-devices-node.md)

## <a name="scenario-overview"></a>Stsenaariumi ülevaade

Sel puhul loote seade, mis saadab järgmise telemeetria jälgimise [eelkonfigureeritud lahendus]Remote[lnk-what-are-preconfig-solutions]:

- Välistemperatuur
- Sisetemperatuur
- Niiskus

Lihtsuse huvides seadme kood tekitab proovi väärtusi, kuid soovitame teil laiendada proovi otse Andurite ühendamine seadme ja saatmine otse telemeetria.

Täitma käesoleva juhendaja, peate aktiivne Azure konto. Kui teil ei ole kontot, saate luua tasuta prooviperiood konto vaid paar minutit. Lisateavet [Azure tasuta prooviperiood][lnk-free-trial].

## <a name="before-you-start"></a>Enne alustamist

Enne kui kirjutad oma seadme koodi, tuleb ette valmistada oma serveri jälgimise eelkonfigureeritud lahendus ja ette valmistada uue kohandatud seadme selle lahuses.

### <a name="provision-your-remote-monitoring-preconfigured-solution"></a>Näha oma kaugseire eelkonfigureeritud lahendus

Loote õpetamisel seade saadab andmeid, näiteks [jälgida] [ lnk-remote-monitoring] eelkonfigureeritud lahendus. Kui te ei ole juba ette valmistatud kaugete eelkonfigureeritud lahendus Azure'i kontol, tehke järgmist:

1. Klõpsake lehe <https://www.azureiotsuite.com/> **+** uue lahenduse loomiseks.

2. Klõpsake **Valige** oma uue lahenduse loomiseks **kaugseire** paneeli.

3. Lehel **luua Remote jälgimise lahendus** Sisestage soovitud **lahenduse nimi** , Vali **piirkond** soovite juurutada ja valige soovitud Azure'i tellimus. Klõpsake nuppu **Loo lahendus**.

4. Oodake kuni kasutuselevõtmise protsess on lõpule viidud.

> [AZURE.WARNING] Eelkonfigureeritud lahendusi kasutada Azure'i teenuse maksumus ühes. Kindlasti, kui olete lõpetanud sellega, et vältida mis tahes asjatut eelkonfigureeritud lahenduse oma tellimusest eemaldada. Külastades <https://www.azureiotsuite.com/> lehel saate eemaldada täielikult tellimuse eelkonfigureeritud lahendus.

Remote jälgimise lahendus ebausaldusväärsete lõpulejõudmisel klõpsake **käivitada** lahendus armatuurlaud brauseris avada.

![][img-dashboard]

### <a name="provision-your-device-in-the-remote-monitoring-solution"></a>Ette valmistada seade kaugtöölaua jälgimise lahendus

> [AZURE.NOTE] Kui seade on ette valmistatud juba kaasamist lahendusse, saate selle sammu vahele jätta. Peate seadme mandaati teada kliendi taotluse loomisel.

Seadme ühendamiseks eelkonfigureeritud lahendus, ta kindlaks ise IoT keskusse kehtiv mandaadi abil. Seadme mandaadi saate tuua armatuurlaualt lahendus. Seadme mandaadi kaasata oma kliendi taotluse hilisemas õpetamisel. 

Uue seadme lisamiseks oma serveri jälgimise lahendus täitke järgmised juhised lahendus armatuurlaud:

1.  Armatuurlaua vasakus allnurgas nuppu **Lisa seade**.

    ![][1]

2.  **Kohandatud seadme** paneel, klõpsake **Lisa uus**.

    ![][2]

3.  Valida, **lubage mul määratleda oma seadme ID**, sisestage seadme ID, näiteks **mydevice**, klõpsake **Sisse ID** veenduge, et nimi pole juba kasutusel, ja seejärel klõpsake nuppu **Loo** ette valmistada seade.

    ![][3]

5. Tehke märge seadme mandaat (seadme ID, asjade Interneti jaotur Hostname ja seadme võtit), kliendi rakendus seda nõuab ühenduse loomiseks kaugtöölaua jälgimise lahendus. Klõpsake nuppu **valmis**.

    ![][4]

6. Veenduge, et seade kuvab jaotises seadmed. Seadme olek on **Ootel** , kuni seade loob ühenduse kaugtöölaua jälgimise lahendus.

    ![][5]

[img-dashboard]: ./media/iot-suite-selector-connecting/dashboard.png
[1]: ./media/iot-suite-selector-connecting/suite0.png
[2]: ./media/iot-suite-selector-connecting/suite1.png
[3]: ./media/iot-suite-selector-connecting/suite2.png
[4]: ./media/iot-suite-selector-connecting/suite3.png
[5]: ./media/iot-suite-selector-connecting/suite5.png

[lnk-what-are-preconfig-solutions]: ../articles/iot-suite/iot-suite-what-are-preconfigured-solutions.md
[lnk-remote-monitoring]: ../articles/iot-suite/iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/