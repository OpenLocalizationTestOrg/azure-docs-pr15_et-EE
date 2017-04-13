<properties
    pageTitle="SharePoint Serveri parkides loomine | Microsoft Azure'i"
    description="Kiiresti luua uut SharePoint 2013 või 2016 SharePointi serveripargi Azure."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="JoeDavies-MSFT"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="josephd"/>

# <a name="create-sharepoint-server-farms"></a>SharePoint Serveri parkides loomine

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]klassikaline mudel.

## <a name="sharepoint-2013-farms"></a>SharePoint 2013 parkides

Portaali Microsoft Azure'i turuplatsilt, saate kiiresti luua eelnevalt konfigureeritud SharePoint Server 2013 parkides. See saate säästa palju aega, kui teil on vaja tavaline või kõrge-saadavus SharePointi serveripargi arendaja/testimiskeskkonnas või kui SharePoint Server 2013 koostöö lahendus teie asutuse jaoks.

> [AZURE.NOTE] **SharePointi serveripargi** üksuse Azure'i turuplatsi Azure portaali on eemaldatud. See on asendatud **SharePoint 2013 mitte HA serveripargi** ja **SharePoint 2013 HA serveripargi** üksused.

Tavaline SharePointi serveripargi koosneb kolmest virtuaalmasinates selle konfiguratsiooni.

![sharepointfarm](./media/virtual-machines-windows-sharepoint-farm/Non-HAFarm.png)

Saate selle serveripargi konfiguratsiooni lihtsustatud häälestamine SharePointi rakenduste arendamise või oma esimest korda hindamisest SharePoint 2013.

Tavaline (kolme-server) SharePointi serveripargi loomiseks tehke järgmist.

1. Klõpsake [siin](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-nonha/).
2. Klõpsake nuppu **juurutamine**.
3. Klõpsake soovitud **SharePoint 2013 mitte-HA serveripargi** paani, klõpsake nuppu **Loo**.
4. Sätete määramiseks klõpsake juhiseid soovitud **loomine SharePoint 2013 mitte-HA serveripargi** paanil ja klõpsake seejärel nuppu **Loo**.

Kõrge-saadavus SharePointi serveripargi koosneb üheksa virtuaalmasinates selle konfiguratsiooni.

![sharepointfarm](./media/virtual-machines-windows-sharepoint-farm/HAFarm.png)

Saate selle serveripargi konfiguratsiooni testimiseks suurema kliendi laadimise, välise SharePointi saidi kättesaadavuse ja SQL serveri Kättesaadavusrühmad SharePointi serveripargi. Selle konfiguratsiooni saate kasutada ka SharePointi rakenduse arendamise kõrge-saadavus keskkonnas.

Kõrge-saadavus (üheksa-server) SharePointi serveripargi loomiseks tehke järgmist.

1. Klõpsake [siin](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-ha/).
2. Klõpsake nuppu **juurutamine**.
3. **SharePoint 2013 HA serveripargi** paanil, klõpsake nuppu **Loo**.
4. Sätete määramiseks klõpsake paani **Loomine SharePoint 2013 HA serveripargi** seitse juhiseid ja seejärel klõpsake nuppu **Loo**.

> [AZURE.NOTE] Ei saa luua **SharePoint 2013 mitte HA serveripargi** või **SharePoint 2013 HA serveripargi** on Azure tasuta prooviversioon.

Azure'i portaalis loob nii nende parkides vaid virtuaalse võrgu on Interneti-ühendusega web kohalolek. Ei ole-saidilt VPN- või ExpressRoute seost tagasi, et teie asutuse võrku.

> [AZURE.NOTE] Kui loote peamisi või kõrge-saadavus SharePointi parkides Azure'i portaalis, ei saa määrata ressursside olemasolevasse rühma. Seda piirangut lahendamiseks luua neid parkides Azure PowerShelli abil. Lisateavet leiate teemast [loomine SharePoint 2013 arendaja katse parkides, kus on Azure PowerShelli](https://technet.microsoft.com/library/mt743093.aspx#powershell).

## <a name="sharepoint-2016-farms"></a>SharePointi 2016 parkides

Vt [käesoleva artikli](https://technet.microsoft.com/library/mt723354.aspx) juhiste koostamiseks järgmised ühe-SharePoint Server 2016 serveripargi.

![sharepointfarm](./media/virtual-machines-windows-sharepoint-farm/SP2016Farm.png)

## <a name="managing-the-sharepoint-farms"></a>SharePointi parkides haldamine

Saate hallata neid parkides kaudu kaugtöölaua serverid. Lisateavet leiate teemast [virtuaalse masina sisse logida](virtual-machines-windows-hero-tutorial.md#log-on-to-the-virtual-machine).

Keskse administreerimiskeskuse SharePointi saidilt, saate konfigureerida isiklike saitide, SharePointi rakendused ja muud funktsioonid. Lisateavet leiate teemast [Konfigureerimine SharePoint](http://technet.microsoft.com/library/ee836142.aspx).

## <a name="next-steps"></a>Järgmised sammud

- Vaadake täiendavaid [SharePoint konfiguratsioone](https://technet.microsoft.com/library/dn635309.aspx) Azure taristu teenused.
