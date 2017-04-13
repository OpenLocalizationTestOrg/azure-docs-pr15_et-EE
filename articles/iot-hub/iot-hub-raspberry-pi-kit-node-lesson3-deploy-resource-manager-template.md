<properties
 pageTitle="Azure'i funktsioon rakendus ja Azure Storage konto loomine | Microsoft Azure'i"
 description="Azure'i funktsioon rakenduse Azure'i asjade jaoturi sündmused on kuulata, töötleb sissetulevate sõnumite ja kirjutab need Azure'i tabelimälu."
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

# <a name="31-create-an-azure-function-app-and-azure-storage-account"></a>3,1 Azure funktsioon rakendus ja Azure Storage konto loomine

[Azure'i funktsioonid](../../articles/azure-functions/functions-overview.md) on lahendus hõlpsalt töötab väikeste osade kood, mida nimetatakse "funktsioonid" pilveteenuses. Rakenduse Azure funktsioon majutab Azure oma ülesannete täitmise.

## <a name="311-what-will-you-do"></a>3.1.1, mida te teete

Azure'i ressursihaldur malli kasutamiseks Azure funktsioon rakendus ja Azure Storage konto loomiseks. Azure'i funktsioon rakenduse Azure'i asjade jaoturi sündmused on kuulata, töötleb sissetulevate sõnumite ja kirjutab need Azure'i tabelimälu. Kui vastate probleeme, otsida lahendusi [lehe tõrkeotsing](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="312-what-will-you-learn"></a>3.1.2, mida saate teada

- Kuidas kasutada [Azure ressursihaldur](../../articles/azure-resource-manager/resource-group-overview.md) juurutada Azure ressursse.
- Kuidas kasutada Azure funktsioon rakendus protsessi asjade jaoturi sõnumeid ja kirjutada need Azure'i tabelimälu tabelisse.

## <a name="313-what-do-you-need"></a>3.1.3 mida on vaja

- Peate on lõpule viidud eelmise õppetükkide: [Alustamine oma Vaarika Pi 3](iot-hub-raspberry-pi-kit-node-get-started.md) ja [oma Azure'i asjade keskuse loomine](iot-hub-raspberry-pi-kit-node-get-started.md).

## <a name="314-open-the-sample-app"></a>3.1.4 Avage rakendus näidis

Avage projekt valimi Visual Studio kood käivitades järgmised käsud:

```bash
cd Lesson3
code .
```

![Repo struktuur](media/iot-hub-raspberry-pi-lessons/lesson3/repo_structure.png)

- Funktsiooni `app.js` faili selle `app` alamkausta on klahv lähtefail. See lähtefail sisaldab sõnumi saatmine 20 korda oma asjade jaoturi ja vilgub LED iga saadab sõnumi kohta.
- Funktsiooni `arm-template.json` fail on Azure ressursihaldur Mall, mis sisaldab Azure funktsioon rakendus ja Azure Storage konto.
- Funktsiooni `arm-template-param.json` fail on Azure ressursihaldur malli kasutatav konfiguratsioonifail.
- Funktsiooni `ReceiveDeviceMessages` alamkausta sisaldab Node.js Azure funktsiooni.

## <a name="315-configure-azure-resource-manager-templates-and-create-resources-in-azure"></a>3.1.5 konfigureerida Azure ressursihaldur Mallid ja Azure ressursside loomine

Värskenduse funktsiooni `arm-template-param.json` faili Visual Studio kood.

![Azure'i ressursihaldur malli parameetrid](media/iot-hub-raspberry-pi-lessons/lesson3/arm_para.png)

- Asendage **[asjade jaoturi nimi]** [tund 2](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md)määratud **{minu jaoturi nimi}** .
- Mis tahes eesliitega, mida soovite asendada **[eesliite string uue ressursid]** . Eesliite tagab ressursi nimi globaalselt kordumatu konflikte. Ärge kasutage mõttekriipsu või arvu esialgse eesliide.

> [AZURE.NOTE] Te ei pea `azure_storage_connection_string` selles jaotises. Jätke see on.

Pärast värskendamist olevat `arm-template-param.json` faili, juurutada ressursside Azure, käivitage järgmine käsk:

```bash
az resource group deployment create --template-file-path arm-template.json --parameters-file-path arm-template-param.json -g iot-sample -n mydeployment
```

Umbes viis minutit kulub nende ressursside loomine. Töötamise ressursi loomist, saate teisaldada järgmise jaotise juurde.

## <a name="316-summary"></a>3.1.6 Kokkuvõte

Olete loonud rakenduse Azure funktsioon töödelda asjade jaoturi sõnumeid ja Azure Storage konto nende sõnumite talletamiseks. Saate liikuda järgmise jaotise juurutada ja käivitada valimi oma piiga seadme pilve sõnumite saatmiseks.

## <a name="next-steps"></a>Järgmised sammud

[3,2 käivitada valimi seadme pilve sõnumite saatmiseks Vaarika Pi 3](iot-hub-raspberry-pi-kit-node-lesson3-run-azure-blink.md)

