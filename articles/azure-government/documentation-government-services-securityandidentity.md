<properties
    pageTitle="Azure'i valitsuse dokumentidele | Microsoft Azure'i"
    description="See pakub võrdlus funktsioonid ja juhiseid Azure'i valitsuse rakenduste arendamise kohta"
    services="Azure-Government"
    cloud="gov"
    documentationCenter=""
    authors="ryansoc"
    manager="zakramer"
    editor=""/>

<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="10/12/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-security-and-identity"></a>Azure'i Government turbe- ja identiteet

##  <a name="key-vault"></a>Võtme Vault
Selle teenuse ja kuidas seda kasutada kohta täpsema teabe saamiseks vt selle <a href="https://azure.microsoft.com/documentation/services/key-vault">Azure klahvi Vault avalik dokumentatsiooni.</a>

### <a name="data-considerations"></a>Andmete kaalutlused
Järgmine teave tuvastab Azure Governmenti äärist Azure'i klahvi Vault jaoks:

| Lubatud reguleeritud kontrolli all andmed | Andmete reguleeritud kontrolli all pole lubatud |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Kõik andmed, mis on Azure klahvi Vault võti krüptitud võib sisaldada reguleeritud/kontrollitud andmeid. | Azure'i klahvi Vault metaandmete on ei tohi sisaldada ekspordi kontrollitud andmed. Selle metaandmed sisaldab kõik sisestatud loomisel ja säilitada oma võti Vault konfiguratsiooni andmed.  Sisestage andmete reguleeritud/järgmised väljad: ressursside rühma nimed, võti Vault nimed, tellimuse nimi |

Klahv Vault on üldiselt saadaval Azure Government. Avalik, nagu on ilma laiendita nii, et klahv Vault on saadaval PowerShelli kaudu CLI ainult.

## <a name="next-steps"></a>Järgmised sammud

Lisateave ja värskendused, tellimine on <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure'i Government ajaveeb.</a>
