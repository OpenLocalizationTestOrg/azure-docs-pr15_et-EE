<properties
    pageTitle="Kuidas migreerida RemoteApp VNET on Azure VNET | Microsoft Azure'i"
    description="Saate teada, kuidas on Azure VNET RemoteApp VNET migreerimine"
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



# <a name="how-to-migrate-a-hybrid-collection-from-a-remoteapp-vnet-to-an-azure-vnet"></a>Kuidas migreerida hübriid saidikogumi RemoteApp VNET on Azure VNET

> [AZURE.IMPORTANT]
> Azure RemoteApp katkeb. Lugege lisateavet [teadaanne](https://go.microsoft.com/fwlink/?linkid=821148) .

Õiges kohas. Meil on lubatud, mida saate kasutada hübriid RemoteApp saidikogumid otse olemasoleva Azure virtuaalse võrguga (VNETs) RemoteApp kohased VNETs loomise asemel. See võimaldab teil ära VNET uusimaid funktsioone (nt ExpressRoute) ja teie hübriid saidikogumite otsese võrgu juurdepääsu andmine teiste Azure'i teenuste ja selle VNET juurutatud virtuaalmasinates.  (Selle saab teie parema jõudluse ja hõlpsam häälestamine kui VNET-VNET konfiguratsioone).


Oletame, et olete juba loonud hübriid RemoteApp saidikogumi nimetatakse *OriginalCollection* koos RemoteApp VNET nimetatakse *RemoteAppVNET*. Siit leiate juhised migreerimine uue Azure VNET nimetatakse *AzureVNET*.

1.  Klõpsake [haldusportaali](http://manage.windowsazure.com/)menüü **võrkude** loomine on VNET nimetatakse *AzureVNET*, kasutades samasse asukohta, DNS-i konfigureerimine ja aadressiruumi (jaoks olema vähemalt üks *AzureVNET* alamvõrku) saate kasutada *RemoteAppVNET*.
2.  Kas Host *AzureVNET* konfigureerimine või lasta võrguühenduse Active Directory juurutuse, mis *OriginalCollection* on ühendatud domeeni.
3.  Klõpsake menüü **Remoteappsi** luua uue RemoteApp ehk *Uue saidikogumi*. (Kasutada **koos VNET loomine** suvand **Kiiresti luua**).
3.  Konfigureerige *NewCollection* kasutusele võtta mõne alamvõrgu *AzureVNET*sisse.
4.  Konfigureerige *NewCollection* kasutada sama pilt ja domeeni Liitu teavet saate kasutada *OriginalCollection*.
5.  Mõne tunni pärast ilmub *NewCollection* teie saidikogumi loendis on aktiivne olek.

Nüüd, kui te ei vaja kasutaja teabe migreerimiseks algse uue saidikogumi, tehke järgmist edasi.

6.  Kustutage *OriginalCollection*.
7.  Kustutage *RemoteAppVNET*.

Ja olete valmis!

Vaheldumisi, kui soovite migreerida kasutajateabe algse uue saidikogumi, tehke järgmist edasi.

6.  Saada e-posti [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com?subject=Azure%20RemoteApp%20user%20information%20migration) oma Azure tellimuse ID, oma algse saidikogumi nime ja oma uue saidikogumi nimi ja paluge tal oma kasutajateabe migreerida.
7.  2 päeva jooksul RemoteApp meeskonnatöö liigub kasutajate juurdepääs loendit ja kasutajale dokumentide ja kasutajasätete algse kollektsioonist uue saidikogumi.
8.  Kustutage *OriginalCollection*.
9.  Kustutage *RemoteAppVNET*.

Ja nüüd ongi valmis!

Kui teil on küsimusi või vajate abi, saatke [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com?subject=Azure%20RemoteApp%20VNET%20migration%20help).
