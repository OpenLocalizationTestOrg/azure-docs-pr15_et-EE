
<properties
    pageTitle="Tõrkeotsing RemoteApp cloud saidikogumid - loomine | Microsoft Azure'i"
    description="Saate teada, kuidas probleemide tõrkeotsing RemoteApp cloud saidikogumi loomine"
    services="remoteapp"
    documentationCenter=""
    authors="vkbucha"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="troubleshoot-creating-remoteapp-cloud-collections"></a>Tõrkeotsing loomise RemoteApp cloud saidikogumid

> [AZURE.IMPORTANT]
> Azure RemoteApp katkeb. Lugege lisateavet [teadaanne](https://go.microsoft.com/fwlink/?linkid=821148) .

Kui teil on probleeme cloud saidikogumi loomine, vaadake järgmist teavet.

## <a name="your-image-is-invalid"></a>Pilt ei sobi ##
Kui ootate Azure'i ettevalmistamise oma saidikogumi, kuvatakse järgmine teade, "GoldImageInvalid", see tähendab, et teie malli pilti ei vasta, [määratletud pilt nõuded](remoteapp-imagereqs.md). Avage nii, et lugeda nende [nõuete](remoteapp-imagereqs.md), lahendada oma pilt ja proovige uuesti oma saidikogumi loomiseks.

## <a name="common-errors-seen-in-the-azure-management-portal"></a>Levinud vigade Azure haldusportaali näha

    DNS server could not be reached
    ProvisioningTimeout

Pilvepõhine saidikogumid sageli fail loomisel, kuna te kasutate kohandatud pilte.  Kui näete ühte ülaltoodud tõrgete ja kasutate selle saidikogumi loomiseks kohandatud pilt, kontrollige järgmisi asju:

- Veenduge, et saate üles laadida kohandatud pildi vastab pilt.
- Levinud probleem on kõige sagedamini, et pilt ei ole õigesti syspreped.  
- Kontrollige pildi saate käivitada Hyper-V sees või proovige mõne IAAS VM otse Azure tellimuse abil pildi loomine. Kui VM ei käivitu ja ei käivitu, siis see näitab tavaliselt kohandatud pildi oli valmis õigesti.  Veenduge, et kohandatud pildi ehitatud jälgimise kuidas luua kohandatud malli pilti RemoteApp

Kui kasutate ühte Microsoft pildid kaasas tellimuse, proovige saidikogumi uuesti luua. Kui probleem ei lahene, siis pöörduge Microsofti tugiteenuste.

    PlatformImageTrialModeOnly

Kui see viga kuvatakse see tähendab tavaliselt, et makstud konto versioon on täiendatud, kuid soovite kasutada Microsofti esitatud pilt, mis on lubatud ainult ajal prooviversiooni režiimi teenuse. Sel juhul proovige cloud saidikogumi uuesti luua, kuid veenduge, et määrata õige pilt.
