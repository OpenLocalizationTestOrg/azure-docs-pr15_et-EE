<properties
    pageTitle="Lõpp-punktid klassikalise Windowsi VM häälestamine | Microsoft Azure'i"
    description="Vaadake, kuidas häälestada Windows Azure'i klassikaline portaalis VM lõpp-punktid lubada suhtlemine virtuaalse masina Windows Azure'i."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="cynthn"/>

# <a name="how-to-set-up-endpoints-on-a-classic-windows-virtual-machine-in-azure"></a>Kuidas häälestada klassikalise Windowsi virtual arvutisse Azure lõpp-punktid


Kõigi Windows Azure'i klassikaline juurutamise mudeli abil loodud virtuaalmasinates saab automaatselt suhelda privaatse üle võrgu kanali koos teiste sama pilveteenuses või virtuaalse võrgu virtuaalmasinates. Interneti-ühendus või mõne muu virtuaalse võrguga ühendatud arvutites nõuda virtuaalse masina sissetulevate võrgu-liikluse suunamiseks lõpp-punktid. See artikkel kehtib ka [Linuxi](virtual-machines-linux-classic-setup-endpoints.md)jaoks saadaval.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]**Ressursihaldur** juurutamise mudeli, lõpp-punktid on konfigureeritud, kasutades **Võrgu turberühmad (NSGs)**. Lisateabe saamiseks vt [välise juurdepääsu lubamine oma VM Azure portaalis] (virtual-machines-windows-nsg-quickstart-portal.md).

Kui loote virtuaalse masina Windows Azure'i klassikaline portaalis, levinud lõpp-punktid, nagu need kaugtöölaua ja Windows PowerShelli Remoting tavaliselt luuakse teie jaoks automaatselt. Saate konfigureerida täiendavaid lõpp-punktid virtuaalse masina loomisel või hiljem vastavalt vajadusele.



[AZURE.INCLUDE [virtual-machines-common-classic-setup-endpoints](../../includes/virtual-machines-common-classic-setup-endpoints.md)]

## <a name="next-steps"></a>Järgmised sammud

* Azure PowerShelli cmdlet-käsk VM endpoint häälestada, lugege [Lisa-AzureEndpoint](https://msdn.microsoft.com/library/azure/dn495300.aspx).

* Azure'i PowerShelli cmdlet-käsu abil saate hallata ka ACL lõpp-punkti, lugege teemat [haldamine juurdepääsu reguleerimine (ACL) jaoks lõpp-punktid PowerShelli abil loendeid](../virtual-network/virtual-networks-acl-powershell.md).

* Kui olete loonud virtuaalse masina ressursihaldur juurutamise mudeli, saate [luua võrgu turberühmad](../virtual-network/virtual-networks-create-nsg-arm-ps.md) juhtelement liiklust VM Azure PowerShelli.
