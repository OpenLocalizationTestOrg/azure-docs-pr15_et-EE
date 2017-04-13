<properties
 pageTitle="Luua oma asjade jaoturi ja registreerida oma Vaarika Pi 3 | Microsoft Azure'i"
 description="Ressursi rühma loomine, on Azure asjade keskuse loomine ja registreerimist oma Pi Azure'i asjade keskuses Azure'i CLI abil."
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

# <a name="22-create-your-iot-hub-and-register-your-raspberry-pi-3"></a>2.2 luua oma asjade jaoturi ja registreerida oma Vaarika Pi 3

## <a name="221-what-you-will-do"></a>2.2.1, mida te teete

- Ressursi rühma loomine.
- Looge oma Azure'i asjade jaoturi ressursirühma.
- Azure'i asjade jaoturi abil Azure'i CLI oma Vaarika Pi 3 lisada.

Azure'i CLI kasutamisel teie pii lisamiseks oma asjade jaoturi teenuse loob teie pii autentida teenuse jaoks klahvi. Kui vastate probleeme, otsida lahendusi [lehe tõrkeotsing](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="222-what-you-will-learn"></a>2.2.2, mida saate

- Kuidas kasutada Azure CLI on Azure asjade keskuse loomiseks.
- Kuidas luua seadme identiteedi jaoks oma Pi Azure'i asjade keskuses.

## <a name="223-what-you-need"></a>2.2.3 vajalik

- Azure'i konto
- Mac-arvuti või Windowsi arvuti, kuhu on installitud Azure'i CLI

## <a name="224-create-your-azure-iot-hub"></a>2.2.4 luua oma Azure'i asjade jaoturi

Azure'i asjade jaoturi abil saate luua ühendus, jälgimine ja haldamine miljoneid asjade varad. Oma Azure'i asjade keskuse loomiseks tehke järgmist.

1. Logige sisse oma Azure'i konto, käivitage järgmine käsk:

    ```bash
    az login
    ```

    Pärast edukat sisselogimist on loetletud kõik saadaolevad Azure tellimuste.

2. Vaikesätted Azure'i tellimus, mida soovite kasutada, käivitage järgmine käsk:

    ```bash
    az account set -n {subscription id or name}
    ```

    Tellimuse ID või nime leiate väljund `az login`.

3. Registreerida pakkuja, käivitage järgmine käsk:

    ```bash
    az resource provider register -n "Microsoft.Devices"
    ```

    Registreerige pakkuja enne juurutamist pakkuja leiate Azure'i ressurss.

    > [AZURE.NOTE] Enamik teenusepakkujaid on registreeritud Azure portaali või kasutate Azure CLI järgi automaatselt, kuid mitte kõigis. Pakkuja kohta lisateabe saamiseks lugege teemat [levinud Azure juurutamise tõrkeotsing Azure'i ressursihaldur](../resource-manager-common-deployment-errors.md)

4. Looge ressursirühma nimega asjade valimiga Lääne USA piirkonna, käivitage järgmine käsk:

    ```bash
    az resource group create --name iot-sample --location westus
    ```

5. Soovitud asjade keskuse loomine asjade valimiga ressursirühm, käivitage järgmine käsk:

    ```bash
    az iot hub create --name {my hub name} --resource-group iot-sample
    ```

    Vaikimisi väljaannet asjade jaoturi loomist on **F1**, mis on **tasuta**. Lisateabe saamiseks lugege teemat [Azure asjade jaoturi hinnad](https://azure.microsoft.com/pricing/details/iot-hub/).

> [AZURE.NOTE] Oma asjade jaoturi nimi peab olema globaalselt kordumatu.
>
> Saate luua ainult üks **F1** väljaannet Azure'i asjade jaoturi Azure tellimuse alusel.

## <a name="225-register-your-pi-in-your-iot-hub"></a>2.2.5 registreerida oma Pi oma asjade keskuses

Iga seade, mis saadab/saab sõnumeid oma Azure'i asjade jaoturi peab olema registreeritud kordumatu ID-ga.

Registreerida oma Pi oma Azure'i asjade jaoturi töötava järgmine käsk:

```bash
az iot device create --device-id myraspberrypi --hub {my hub name} --resource-group iot-sample
```

## <a name="226-summary"></a>2.2.6 Kokkuvõte

Olete loonud mõne Azure'i asjade jaoturi ja registreeritud oma Pi seadme identiteedi oma Azure'i asjade keskuses. Olete valmis järgmise õppetüki saate teada, kuidas oma Pi sõnumeid saata oma asjade jaoturi liikuda.

## <a name="next-steps"></a>Järgmised sammud

Nüüd olete valmis õppetüki 3, mis algab alustada [luua rakenduse Azure funktsioon ja Azure Storage konto töötlemiseks ja asjade jaoturi sõnumite talletamiseks](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md).