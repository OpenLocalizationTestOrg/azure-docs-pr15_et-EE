
<properties
    pageTitle="Kinnitage Azure'i VNET kasutada Azure RemoteAppi | Microsoft Azure'i"
    description="Siit saate teada, kuidas tagada, et teie Azure'i VNET on Azure RemoteApp kasutamiseks valmis"
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



# <a name="validate-the-azure-vnet-to-use-with-azure-remoteapp"></a>Kinnitage Azure'i VNET kasutamiseks Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp katkeb. Lugege lisateavet [teadaanne](https://go.microsoft.com/fwlink/?linkid=821148) .

Enne Azure RemoteApp on Azure VNET kasutamisel võite selle VNET kinnitamiseks. See aitab vältida probleeme Ühenduvus.

Kinnitage oma Azure'i VNET, tehke järgmist.

1. Looge Azure'i virtuaalarvuti Azure VNET, mida soovite kasutada Azure RemoteAppi alamvõrgu sees.

2. Ühenduse loomine selle VM haldusportaal **ühendamine** suvandi abil.
3. Sama domeen, mida soovite kasutada Azure RemoteAppi virtuaalse masina liita. Kui loote hübriid kogum, mis loob ühenduse kohapealse võrgu, liituda virtuaalse masina kohaliku domeeni.

Kui see õnnestub, on Azure VNET RemoteAppi kasutamiseks valmis.

Hübriid lõpuni kogumise töövoo kohta lisateabe saamiseks lugege järgmisi artikleid:

- [Kuidas Azure RemoteApp virtuaalse võrgu kavandamine](remoteapp-planvnet.md)
- [Hübriidjuurutuse saidikogumi loomine](remoteapp-create-hybrid-deployment.md)
- [Azure RemoteApp saidikogumi juurutada Azure virtuaalse võrgu (mis toetab ExpressRoute)](http://blogs.msdn.com/b/rds/archive/2015/04/23/deploy-azure-remoteapp-collection-to-your-azure-virtual-network-with-support-for-expressroute.aspx)
