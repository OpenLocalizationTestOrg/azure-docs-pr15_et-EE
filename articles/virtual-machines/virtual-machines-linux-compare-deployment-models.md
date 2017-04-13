<properties
   pageTitle="Arvuta, võrgu ja salvestusruumi pakkujate | Microsoft Azure'i"
   description="Arvuta, võrgu ja mälu ressursi pakkujad (CRP, NRP ja SRP) Linuxi rakenduste Azure ressursihaldur juurutamise mudeli ülevaade"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager,azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="08/19/2015"
   ms.author="tomfitz"/>

# <a name="azure-compute-network-and-storage-providers-for-linux-applications-under-azure-resource-manager-deployment-model"></a>Azure'i Arvuta, võrgu ja salvestusruumi pakkujate Linuxi rakenduste Azure ressursihaldur juurutamise mudeli alla

Azure'i ressursihaldur juurutamise mudeli salvestusruumi Arvuta ja võrgu võimalusi lisamine lihtsustab põhjalikult juurutamise ja haldamise keerukate IaaS töötavad rakendused. Paljude rakenduste jaoks on vaja ressursse, sh virtuaalse võrk, salvestusruumi konto, virtuaalse masina ja võrgu liidese kombinatsiooni. Azure'i ressursihaldur juurutamise mudeli pakub võimalust ehitada JSON malli juurutada ja hallata kõik need ressursid koos ühe rakendus.

[AZURE.INCLUDE [virtual-machines-common-compare-deployment-models](../../includes/virtual-machines-common-compare-deployment-models.md)]
