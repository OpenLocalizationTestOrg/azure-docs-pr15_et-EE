<properties
 pageTitle="Tõrkeotsing | Microsoft Azure'i"
 description="Lehe Vaarika Pi Node.js kogemuse tõrkeotsing"
 services="iot-hub"
 documentationCenter=""
 authors="shizn"
 manager="timlt"
 tags=""
 keywords=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/21/2016"
 ms.author="xshi"/>

# <a name="troubleshooting"></a>Tõrkeotsing

## <a name="hardware-issues"></a>Riistvara probleemid

### <a name="the-application-runs-well-but-the-led-is-not-blinking"></a>Rakendus töötab hästi, kuid LED on välkuvad

See probleem on seotud alati riistvara ringi Ühenduvus. Järgmiste juhiste abil saate tuvastada probleeme.

1. Märkige ruut, kui valite oma tahvli õige **GPIO** . Kahe pordid, klõpsake selle õppetüki tuleks **GPIO GND (PIN-koodi 6)** ja **GPIO 04 (PIN-koodi 7)**.
2. Kontrollige, kas teie LED polaarsust on õige. Rohkem lisamaterjalidele peaks sisaldama **positiivne**, anoodi PIN-koodi.
3. Kasutada oma Vaarika Pi 3 **3.3V kinnitamine** ja **GND PIN-koodi** . Näiteks Põhiliselt Power loeb oma pii. Kontrollige, kas LED töötab korralikult.

![LED spec](media/iot-hub-raspberry-pi-lessons/troubleshooting/led_spec.png)

### <a name="other-hardware-issues"></a>Muud riistvara probleemid

Vaarika Pi 3 levinud probleemide kohta leiate teavet vaadake [tõrkeotsingu ametlik lehekülg](http://elinux.org/R-Pi_Troubleshooting).

## <a name="nodejs-package-issues"></a>Node.js paketi probleemid

### <a name="no-response-during-gulp-tasks"></a>Vastust ajal suutäis tööülesanded

Kui teil tekib probleeme, mis töötab suutäis tööülesanded, saate lisada soovitud `--verbose` suvand silumine. Proovige lõpetada praeguse suutäis ülesannete `Ctrl + C` ja seejärel käivitage järgmine käsk oma konsooli aknas kuvamiseks silumine sõnumeid. Võite näha üksikasjalikku tõrketeated, mis on teie konsooli väljund printida. 

```bash
gulp --verbose
```

### <a name="device-discovery-issues"></a>Seadme discovery probleemid

Levinud probleemide tõrkeotsingu abi saamiseks selle `devdisco` käsk, märkige ruut [readme](https://github.com/Azure/device-discovery-cli/blob/develop/readme.md).

### <a name="other-npm-issues"></a>Muud küsimused NPM

Proovige värskendada oma NPM paketi järgmine käsk:

```bash
npm install -g npm
```

Kui probleem on endiselt olemas, sisestage oma kommentaarid käesoleva artikli lõpus või looge Github probleemi meie [Valimi hoidla](https://github.com/Azure-Samples/iot-hub-node-raspberrypi-getting-started)

## <a name="remote-debugging"></a>Remote silumine

### <a name="run-the-sample-application-in-debug-mode"></a>Valimi rakenduse käivitada silumine režiimis

```bash
gulp run --debug
```

Silumine engine on valmis, peaksite nägema ```Debugger listening on port 5858``` konsooli väljund.

### <a name="configure-vs-code-to-connect-to-the-remote-device"></a>VS koodi seadmes ühenduse konfigureerimine

Avage **silumine** paani vasakus servas.

Klõpsake nuppu roheline **Käivitamine silumine** (F5). VS koodi avatakse **launch.json** fail, mida soovite värskendada.

Värskendage **launch.json** faili sisu on järgmine. Asendage `[device hostname or IP address]` tegelik seadme IP-aadress või hostname (hostinimi).   

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Attach",
            "type": "node",
            "request": "attach",
            "port": 5858,
            "address": "[device hostname or IP address]",
            "restart": false,
            "sourceMaps": false,
            "outDir": null,
            "localRoot": "${workspaceRoot}",
            "remoteRoot": null
        }
    ]
}
```

![Remote silumine konfigureerimine](media/iot-hub-raspberry-pi-lessons/troubleshooting/remote_debugging_configuration.png)

### <a name="attach-to-the-remote-application"></a>Remote taotluse manustamine

Olevat rohelist **Käivitamine silumine** (F5) käivitamiseks nuppu silumine. 

Lugege lisateavet siluri [JavaScripti koodi VS](https://code.visualstudio.com/docs/languages/javascript#_debugging) .

![Remote silumine interaktiivsed](media/iot-hub-raspberry-pi-lessons/troubleshooting/remote_debugging_interactive.png)

## <a name="azure-cli-issues"></a>Azure'i CLI probleemid

Azure'i CLI on eelvaade koostamine. Viidata saate otsida lahendusi [Preview installida juhend](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md) .

Kui teil tekib vigu tööriistaga, failide [probleemi](https://github.com/Azure/azure-cli/issues) GitHub repo **probleemide** osas.

Levinud probleemide tõrkeotsingu abi, märkige ruut [readme](https://github.com/Azure/azure-cli/blob/master/README.rst).

## <a name="python-installation-issues"></a>Python installimise probleemid

### <a name="legacy-installation-issues-macos"></a>Pärand installiprobleemide (macOS)

**Pip**installimisel Expression.Error õiguste tõrke, kui on pärand pakette, mis installitakse koos **su** õigused. Olukord põhjuseks varasem install Python pruulima (macOS) abil desinstallitakse täielikult. Mõni varasem install **pip** paketid on loodud juures mis põhjustab õiguste tõrke. Lahendus on need paketid installitud juurkausta eemaldada. Selle toimingu tegemiseks tehke järgmist:

1. Minge /usr/local/lib/python2.7/site-packages
2. Loendi pakettide juurkausta loomiseks tehke järgmist.`ls -l | grep root`
3. Step2 pakettide desinstallimine:`sudo rm -rf {package name}`
4. Uuesti Python.

## <a name="azure-iot-hub-issues"></a>Azure'i asjade jaoturi probleemid

Kui te olete ette valmistatud Azure'i asjade jaoturi abil `azure-cli`, ja teil on vaja tööriista ühenduse oma asjade jaoturi seadmete haldamine, proovige järgmisi tööriistu:

### <a name="device-explorer"></a>Seadme Explorer

[Seadme Exploreri](https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md) oma Windowsi kohalikus arvutis töötab ja loob teie asjade jaoturi Azure. See suhtleb järgmisi [asjade jaoturi lõpp-punktid](iot-hub-devguide.md):

- *Seadme identiteedi haldamine* ette ja seadmete haldamine registreeritud oma asjade jaoturi.
- Selleks, et saaksite jälgida oma asjade jaoturi seadme kaudu saadetud sõnumite *vastuvõtu seadme pilve* .
- Selleks, et saaksite oma seadmed sõnumeid saata oma asjade keskuse kaudu *saata cloud-seade* .

Konfigureerige oma `IoT hub connection string` sees see tööriist kasutada kõigi oma võimalustega.

### <a name="iot-hub-explorer"></a>Asjade jaoturi Explorer

[Asjade jaoturi Explorer](https://github.com/Azure/azure-iot-sdks/blob/master/tools/iothub-explorer/readme.md) on valimi mitmeplatvormilist CLI tööriist ja seadme klientide haldamine. Tööriist võimaldab seadmete registris identiteedi haldamine, seadme pilve sõnumite jälgimine ja saata cloud-seadme käsud.

Iothub-Exploreri tööriista (väljalaske-eelse) uusima versiooni installimiseks käivitage käsurea keskkond järgmine käsk:

```
npm install -g iothub-explorer@latest
```

Saate kõik iothub-Exploreri käsud ja nende parameetrite kohta täiendava abi saamiseks järgmine käsk:

```bash
iothub-explorer help
```

### <a name="use-azure-portal-to-manage-your-resources"></a>Azure'i portaali abil saate hallata oma ressursid

Kõik õpitu saadetakse täielik CLI kogemus luua ja hallata oma Azure ressursse. Samuti võite kasutada [Azure portaali](../azure-portal-overview.md) aitab säte, hallata ning oma Azure ressursse silumine.

## <a name="azure-storage-issues"></a>Azure'i salvestusruumi probleemid

[Microsoft Azure'i salvestusruumi Explorer (eelvaade)](http://storageexplorer.com) on eraldi rakendus Microsoft, mis võimaldab töötada Azure'i andmed, mille Windows, macOS ja Linux. See tööriist võimaldab teil oma tabelisse ühenduse loomiseks ja andmete seda vaadata. Selle tööriista abil saate oma Azure Storage probleemide tõrkeotsing.
