## <a name="view-device-telemetry-in-the-dashboard"></a>Vaata seadme telemeetria armatuurlaual

Armatuurlaua remote jälgimise lahendus võimaldab teil vaadata telemeetria, mis seadmete saada IoT keskusse.

1. Brauseris, remote jälgimise lahendus armatuurlaua tagasi, klõpsake nuppu **seadmed** vasakpoolsel paneelil navigeerida **seadmete loendist**.

2. **Seadmete loendi**, näed, et teie seadme olek on nüüd **töötab**.

    ![][18]

3. Klõpsake **armatuurlaual** tagasi armatuurlaud, valige **seadme vaade** rippmenüü kuvamiseks oma telemeetria. Telemeetria: proovi taotluse on sisetemperatuur 50 ühikut 55 ühikut, välistemperatuur ja niiskus 50 ühikut. Pange tähele, et vaikimisi armatuurlaual kuvatakse ainult temperatuuri- ja väärtused.

    ![][img-telemetry]

## <a name="send-a-command-to-your-device"></a>Seadmesse saata

Armatuurlaua remote jälgimise lahendus lubab saata käske läbi IoT Hub seadmete. Näiteks saate saata remote jälgimise lahendus seada sisetemperatuuri seadme käsk.

1. Remote jälgimise lahendus armatuurlaual, klõpsake vasakpoolsel paneelil navigeerida **seadmete loendist** **seadmed** .

2. Klõpsake **Seadme ID** **seadmete loendist**seade.

3. **Seadme üksikasjad** paanil klõpsake **käske**.

    ![][13]

4. Valige **SetTemperature** **käsk** rippmenüü ja **temperatuuri** sisestage uutest temperatuuri väärtust. Klõpsake **käsu** saada käsk seade.

    ![][14]

    > [AZURE.NOTE] Käskude ajalugu kuvatakse käsk seisund esialgu **Ootel**. Seade tunnistab käsk, muutub olek olekuks **edu**.

5. Armatuurlaual, veenduge, et seade on nüüd saata 75 uutest temperatuuri väärtust.

## <a name="next-steps"></a>Järgmised sammud

[Parameetrite määramine eelkonfigureeritud lahendusi] artikli[ lnk-customize] kirjeldab mõnes mõttes saab laiendada seda proovi. Korral ka kasutades otse andurid ja rakendada täiendavaid käske.

Saate lisateavet [õigustest azureiotsuite.com][lnk-permissions].

[13]: ./media/iot-suite-visualize-connecting/suite4.png
[14]: ./media/iot-suite-visualize-connecting/suite7-1.png
[18]: ./media/iot-suite-visualize-connecting/suite10.png
[img-telemetry]: ./media/iot-suite-visualize-connecting/telemetry.png
[lnk-customize]: ../articles/iot-suite/iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-permissions]: ../articles/iot-suite/iot-suite-permissions.md
