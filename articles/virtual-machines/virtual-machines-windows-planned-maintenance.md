<properties
    pageTitle="Kavandatud hooldustööd Windows vms | Microsoft Azure'i"
    description="Aru, milliseid Azure kavandatud hooldustööde on ja kuidas see mõjutab oma Windows Azure'i töötab virtuaalmasinates"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="drewm"
    manager="timlt"
    editor=""
    tags="azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="04/26/2016"
    ms.author="drewm"/>

# <a name="planned-maintenance-for-virtual-machines-in-azure"></a>Kavandatud hooldustööde jaoks Azure'i virtuaalmasinates


Aru, milliseid Azure kavandatud hooldustööde on ja kuidas see võib mõjutada teie Windowsi virtuaalmasinates olemasolu. See artikkel kehtib ka [Linuxi](virtual-machines-linux-planned-maintenance.md)jaoks saadaval. 

Sellest artiklist leiate Azure'i kavandatud hooldustööde protsessi esmakordseks taust. Kui te ei soovi oma VM rebooted tõrkeotsing, saate [selle ajaveebi lugemine post üksikasjalikult, vaatamine VM taaskäivitage logid](https://azure.microsoft.com/blog/viewing-vm-reboot-logs/).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


## <a name="why-azure-performs-planned-maintenance"></a>Miks Azure'i sooritab kavandatud hooldustööd

Microsoft Azure'i värskendab perioodiliselt üle maailma töökindlust, jõudlust ja host infrastruktuuri, mis on aluseks virtuaalmasinates turvalisuse täiustamiseks. Paljude värskendused läbi ilma mingit mõju virtuaalmasinates või pilveteenustega, sh mälu säilitamine värskendused.

Siiski mõne värskenduse puhul vajalike värskenduste rakendamiseks infrastruktuuri oma virtuaalmasinates taaskäivitama. Funktsiooni virtuaalmasinates sulgeda ajal me paik infrastruktuuri ja seejärel soovitud virtuaalmasinates uuesti.

Pange tähele, et on kahte tüüpi hooldustööd, mis võivad mõjutada teie virtuaalmasinates kättesaadavus: plaanitud ja plaanimata. Sellel lehel kirjeldatakse, kuidas Microsoft Azure'i sooritab kavandatud hooldustööd. Planeerimata haldamise kohta leiate lisateavet teemast [mõistmine kavandatud võrreldes planeerimata hooldustööd](virtual-machines-windows-manage-availability.md).

[AZURE.INCLUDE [virtual-machines-common-planned-maintenance](../../includes/virtual-machines-common-planned-maintenance.md)]
