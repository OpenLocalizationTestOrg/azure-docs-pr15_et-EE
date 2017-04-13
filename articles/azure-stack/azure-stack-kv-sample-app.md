<properties
    pageTitle="Luba rakenduse revtrieve Azure virnas klahvi Vault saladused | Microsoft Azure'i"
    description="Azure'i virnas klahvi Vault töötamine valimi rakenduse abil"
    services="azure-stack"
    documentationCenter=""
    authors="rlfmendes"
    manager="natmack"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/26/2016"
    ms.author="ricardom"/>

# <a name="run-the-sample-application-for-key-vault"></a>Käivitage rakendus valimi klahvi Vault jaoks 

Sellest juhendist saate kasutada proovi taotluse toomiseks saladusi ja paroolide klahvi hoidlast.

## <a name="download-the-samples-and-prepare"></a>Laadige alla näidised ja ettevalmistamine

[Azure'i klahvi Vault kliendi näidised lehe](https://www.microsoft.com/en-us/download/details.aspx?id=45343)Azure'i klahvi Vault kliendi näidised alla laadida.

Kohalikus arvutis ZIP-faili sisu ekstraktimiseks.

Lugege **README.md** faili (see on tekstifailist) ja seejärel järgige juhiseid.

## <a name="run-sample-1--hellokeyvault"></a>Valimi #1--HelloKeyVault käivitamine
HelloKeyVault on konsooli rakendus, mis juhendab loetletud tähtsaimad stsenaariumid, mida toetavad klahvi Vault.

  1. Võti (HSM või tarkvara võti) loomine või importimine
  2. Krüpti salajase sisu klahvi abil
  3. Murra sisu võti võti Vault klahvi abil
  4. Lahtipakkimiseks sisu võti
  5. Salajane dekrüptimine
  6. Salajane seadmine

Selle konsooli rakendus peaks käivitada muudatusi, välja arvatud, et vajalike seadete rakenduses App.Config värskendatakse vastavalt järgmist:

1. Rakenduse konfigureerimise sätete HelloKeyVault\App.config uuendamiseks oma vault URL-i, rakenduse põhisumma ID ja salajane. Soovi korral saab luua teabe abil **scripts\GetAppConfigSettings.ps1**kaudu.
2. Värskendage kohustuslik muutujad GetAppConfigSettings.ps1 väärtused.
3. Käivitage Windows PowerShelli aken.
4. Käivitage PowerShelli aknas sees GetAppConfigSettings.ps1 skript.
5. Kopeerige skripti tulemuste HelloKeyVault\App.config faili.


## <a name="next-steps"></a>Järgmised sammud

[Klahv Vault parooliga VM juurutamine](azure-stack-kv-deploy-vm-with-secret.md)

[Klahv Vault sertifikaadiga VM juurutamine](azure-stack-kv-push-secret-into-vm.md)