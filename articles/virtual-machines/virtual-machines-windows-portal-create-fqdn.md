<properties
   pageTitle="Luua FQDN VM Azure portaali | Microsoft Azure'i"
   description="Siit saate teada, kuidas luua täielikult kvalifitseeritud domeeninime või FQDN-i ressursihaldur jaoks vastavalt virtuaalse masina Azure'i portaalis."
   services="virtual-machines-windows"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="08/24/2016"
   ms.author="iainfou"/>

# <a name="create-a-fully-qualified-domain-name-in-the-azure-portal"></a>Täielikult kvalifitseeritud domeeninime Azure portaali loomine
[Azure'i portaalis](https://portal.azure.com) ressursihaldur juurutamise näidise virtuaalse masina (VM) loomisel luuakse automaatselt avaliku IP ressursiks virtuaalse masina. IP-aadressi abil pääsete juurde VM. Kuigi portaali luua [täielik domeeninimi](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)või FQDN, vaikimisi, saate selle luua kui VM on loodud. Selles artiklis tutvustatakse juhised DNS-i nimi või FQDN loomiseks.

[AZURE.INCLUDE [virtual-machines-common-portal-create-fqdn](../../includes/virtual-machines-common-portal-create-fqdn.md)]

Saate nüüd luua ühenduse kaugühenduse teel VM abil selle DNS-i nimi nagu Kaugtöölaua protokolli (RDP) jaoks.

## <a name="next-steps"></a>Järgmised sammud
Nüüd, kui teie VM on avaliku IP- ja DNS-i nimi, saate juurutada levinud rakenduse raamistiku või teenuseid, näiteks IIS-i, SQL-i või SharePointi.

Samuti saate lugeda Lisateavet [abil ressursihaldur](../azure-resource-manager/resource-group-overview.md) näpunäiteid oma Azure juurutuste koostamise kohta.