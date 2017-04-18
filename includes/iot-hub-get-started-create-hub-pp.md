## <a name="create-a-device-management-enabled-iot-hub"></a>Looge seadme halduse lubatud asjade jaoturi

Kuna asjade jaoturi mobiilsideseadmete halduse eelvaade, peate looma seadme halduse lubatud asjade jaoturi. Kui asjade jaoturi mobiilsideseadmete halduse jõuab üldiselt kättesaadav, värskendatakse selle õpetuse. Järgmised sammud näitavad, kuidas selle ülesande Azure'i portaalis.

1.  [Azure'i portaali]sisse logida.
2.  Jumpbar, klõpsake nuppu **Uus**, seejärel klõpsake **Asjade Interneti**ja klõpsake **Azure asjade jaoturi**.

    ![][img-new-hub]

3.  **Asjade jaoturi** tera, valige oma asjade jaoturi konfigureerimine.

    ![][img-configure-hub]

  -   Sisestage **nimi** väljale nimi oma asjade jaoturi. Kui **nimi** on kehtivate ja saadaval, kuvatakse roheline märge väljale **nimi** .
  -   Valige soovitud **hinnakirjad ja skaala taseme**. Selles õpetuses ei nõua teatud taseme.
  -   **Ressursirühm**luua uue ressursirühma või valige olemasoleva. Lisateavet leiate teemast [kaudu ressursside rühmade hallata oma Azure ressursse].
  -   Märkige ruut **Luba mobiilsideseadmete halduse - eelvaade**.
  -   Valige **asukoht**, majutada oma asjade jaoturi asukoht. Asjade jaoturi seadmehalduse on saadaval ainult Ida-USA, Põhja-Euroopa ja Ida-Aasia avaliku eelvaate.

    > [AZURE.NOTE]Kui te pole märkige ruut **Luba mobiilsideseadmete halduse**, näidised ei tööta.<br/>Märkides ruudu **Luba mobiilsideseadmete halduse**, saate luua asjade jaoturi toetavad ainult Ida-USA, Põhja-Euroopa ja Ida-Aasia ja muuks peamised eelvaade. Seadmete ei saa migreerida, ja sealt seadme halduse lubatud jaoturi.

4.  Kui olete valinud oma asjade jaoturi konfiguratsiooni suvandid, klõpsake nuppu **Loo**. Võib kuluda mõni minut Azure oma asjade keskuse loomiseks. Oleku, saate jälgida edenemist **Startboard** või paanil **teatised** .

    ![][img-monitor]

5.  Asjade jaoturi on loodud, avatakse automaatselt teie jaoturi höövlitera. **Hostname (hostinimi)**üles ja klõpsake **ühiskasutuses juurdepääsupoliitikaid**.

    ![][img-keys]

6.  Klõpsake **iothubowner** poliitika, ja seejärel kopeerige ja märkige üles ühendusstringi **iothubowner** tera. Kopeerige see asukohta, pääsete hiljem, sest teil on vaja seda selles õpetuses lõpuleviimiseks.

    > [AZURE.NOTE] Peamised, veenduge, et mitte **iothubowner** mandaadi abil.

    ![][img-connection]

Nüüd olete loonud mõne seadme halduse lubatud asjade jaoturi. Peate selle õpetuse lõpuleviimiseks ühendusstring.

## <a name="create-a-device-identity"></a>Seadme identiteedi loomine

Selles jaotises saate kasutada sõlm tööriista nimega [Asjade jaoturi Exploreri] [ iot-hub-explorer] seadme identiteedi jaoks selles õpetuses loomiseks.

1. Käivitage käsurea keskkonna järgmist:

    NPM installi -giothub-explorer@latest

2. Käivitage järgmine käsk abil sisselogimine uuendatakse asendada oma keskuses `{service connection string}` eelnevalt kopeeritud asjade jaoturi ühendusstringi abil:

    iothub-Exploreri login "{teenuse ühendusstringi}"

3. Lõpuks luua uue seadme identiteedi nimetatakse `myDeviceId` käsuga:

    iothub – Exploreri loomine myDeviceId--ühendusstringi –

Kirjutage seadme ühendusstringi tulemi. Seadme rakendus kasutab seda ühendusstringi ühenduse loomiseks oma asjade jaoturi seade.

![][img-identity]

Viidata [Alustamine asjade jaoturi] [ lnk-getstarted] võimalus luua seadme identiteete programmiliselt jaoks.

<!-- images and links -->
[img-new-hub]: media/iot-hub-get-started-create-hub-pp/image1.png
[img-configure-hub]: media/iot-hub-get-started-create-hub-pp/image2.png
[img-monitor]: media/iot-hub-get-started-create-hub-pp/image3.png
[img-keys]: media/iot-hub-get-started-create-hub-pp/image4.png
[img-connection]: media/iot-hub-get-started-create-hub-pp/image5.png
[img-identity]: media/iot-hub-get-started-create-hub-pp/devidentity.png

[Azure'i portaal]: https://portal.azure.com/
[iot-hub-explorer]: https://github.com/Azure/azure-iot-sdks/tree/master/tools/iothub-explorer

[lnk-getstarted]: ../articles/iot-hub/iot-hub-csharp-csharp-getstarted.md
[Ressursi rühmade abil saate hallata oma Azure ressursse]: ../articles/azure-portal/resource-group-portal.md
