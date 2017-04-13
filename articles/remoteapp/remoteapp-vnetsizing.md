
<properties
    pageTitle="Suuruse muutmise kohta on VNET Azure RemoteApp | Microsoft Azure'i"
    description="Siit saate teada, IP address nõuete Azure RemoteApp on VNET töötamise kohta"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="sizing-information-for-a-vnet-in-azure-remoteapp"></a>Teave on VNET Azure RemoteApp suuruse muutmine

> [AZURE.IMPORTANT]
> Azure RemoteApp katkeb. Lugege lisateavet [teadaanne](https://go.microsoft.com/fwlink/?linkid=821148) .

Kui kasutate Azure RemoteApp virtuaalse võrguga (VNET), kasutab RemoteApp sees alamvõrgu IP-aadressid. Põhineb skaala RemoteApp teenust, peate veenduge, et teie alamvõrku on piisavalt IP-aadresside RemoteApp virtuaalmasinates jaoks saadaval. Ajal sizing juhised pole täiuslik, kuidas RemoteApp dünaamiliselt pöörleb üles ja alla virtuaalmasinates sees kogumi keerutab, aitab see teie alamvõrku vahemiku prognoosimiseks. See on eriti oluline, sest kui RemoteApp teenuse paigutatakse on VNET, te ei saa suurendamine alamvõrgu RemoteApp eemaldamata.

Iga RemoteApp kogum, mida soovite käivitada maksimaalne võimsus peaks olema 100 IP-aadresside saadaval. Näiteks, kui teil on ühe RemoteApp saidikogumi Standard kavandamine ja soovite on kuni 500 kasutajat, peaks olema selle saidikogumi jaoks 100 IP-aadressid. Samuti vajate 100 IP-aadressid RemoteApp saidikogumi lihtsa leping, mis sisaldab 800 kasutajad. Kui plaanite on vähem kasutajat (vähem kui), saate vähendada IP-aadresside vaja saidikogumi kohta. Minimaalne alamvõrgu maht on 30 IP-aadressid (/ 27).

Vaadake järgmist teavet veenduge, et teie VNET on konfigureeritud ja töötavad Nüüdisaegsetes.

- [Isikliku VNET on Azure VNET migreerimine](remoteapp-migratevnet.md)
- [Kinnitage Azure'i VNET kasutamiseks Azure RemoteApp](remoteapp-vnet.md)
