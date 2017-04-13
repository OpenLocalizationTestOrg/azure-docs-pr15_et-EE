<properties
   pageTitle="Töö- või koolikonto identiteedi loomist AAD | Microsoft Azure'i"
   description="Saate teada, kuidas luua töökoha või kooli identiteedi Azure Active Directory abil oma Linux virtuaalmasinates."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="squillace"
   manager="timlt"
   editor=""
   tags="azure-service-management,azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="08/23/2016"
   ms.author="rasquill"/>

# <a name="creating-a-work-or-school-identity-in-azure-active-directory-to-use-with-linux-vms"></a>Azure Active Directory abil Linux VMs töö või kooli identiteedi loomine

Kui olete loonud Azure'i konto või isiklik MSDN-i tellimus on ja Azure'i konto ära MSDN-i Azure krediiti--loodud teie *Microsofti konto* identiteedi loomiseks kasutatud. Palju suurepäraseid omadusi Azure-- [Ressursi Mallid](../azure-resource-manager/resource-group-overview.md) on üks näide - töökoha või kooli konto (identiteedi haldab Azure Active Directory) nõutav töötamiseks. Saate järgige alltoodud juhiseid, et luua uuele töö- või koolikontoga, sest Õnneks üks parimaid asju Azure isikliku konto on kaasas vaikimisi Azure Active Directory domeeni, mille abil saate luua uuele töö- või koolikontoga, mida saate kasutada Azure funktsioone, mis nõuavad.

Siiski tehtud muudatused võimaldavad mis tahes tüüpi Azure'i konto abil oma tellimuse haldamiseks soovitud `azure login` Interaktiivne sisselogimine kirjeldatud meetodiga [siin](../xplat-cli-connect.md). Võite kasutada seda süsteemi või jälgite järgige allolevaid juhiseid. Samuti saate [luua töö või kooli identiteedi Azure Active Directory Windows VMs abil](virtual-machines-windows-create-aad-work-id.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

[AZURE.INCLUDE [virtual-machines-common-create-aad-work-id](../../includes/virtual-machines-common-create-aad-work-id.md)]
