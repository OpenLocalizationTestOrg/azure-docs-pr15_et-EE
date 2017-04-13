<properties
   pageTitle="Windowsi kliendi piltide kasutamise katse arendaja stsenaariumid | Microsoft Azure'i"
   description="Visual Studio tellimuse eeliste abil saate juurutada Windows 7 ja 8/10 Azure'i arendaja katse stsenaariumid"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="iainfou"/>

# <a name="using-windows-client-in-azure-for-devtest-scenarios"></a>Kliendi Windows Azure kasutamise arendaja katse stsenaariumid

Saate kasutada Windows 7, Windows 8 või Windows 10 Azure arendaja katse stsenaariumid tingimusel, et teil on asjakohane Visual Studio (varasema nimega MSDN) tellimus. See artikkel kirjeldab töötava Windowsi kliendi Azure ja Azure Galerii piltide kasutamise nõuded.


## <a name="subscription-eligibility"></a>Tellimuse nõuetele vastavuse
Aktiivse Visual Studio tellijad (inimesed, kes on omandanud Visual Studio abonemendi litsentsi) saate kasutada Windowsi kliendi arendamise ja testimiseks. Windowsi kliendi saab kasutada oma riistvara ja Azure'i virtuaalmasinates töötab mis tahes tüüpi Azure'i tellimus. Windowsi klient võib-olla ei juurutatud või kasutada Azure Tavatootmisaeg kasutamiseks ega kasutavad inimesed, kes pole aktiivne Visual Studio tellijad.

Teie mugavuse huvides meil on kättesaadav teatud Windows 10 piltide jooksul [õigus arendaja katse pakub](#eligible-offers)Azure'i galeriist. Visual Studio tellijad jooksul mis tahes tüüpi pakkumise saate ka [piisavalt ettevalmistamine ja luua](virtual-machines-windows-prepare-for-upload-vhd-image.md) 64-bitine Windows 7, Windows 8 või Windows 10 pilt ja seejärel [Azure'i üles laadida](virtual-machines-windows-upload-image.md). Kasuta jääb piirdub arendaja katse aktiivne Visual Studio tellijad.


## <a name="eligible-offers"></a>Õigus pakkumised
Järgmises tabelis üksikasjade pakkumine ID-d, mis on Windows 10 Azure'i Galerii juurutamine. Windows 10 pildid on ainult nähtavate järgmisi pakub. Visual Studio tellijad, kellel on vaja käivitamiseks Windows kliendi pakkumine eri tüüpi nõuavad [piisavalt ettevalmistamine](virtual-machines-windows-prepare-for-upload-vhd-image.md) ja luua 64-bitine Windows 7, Windows 8 või Windows 10 pilt ja [seejärel Azure'i üles laadida](virtual-machines-windows-upload-image.md).

| Pakkumise nimi | Pakkumine arv | Saadaval kliendi pildid |
|:-----------|:------------:|:-----------------------:|
| [Grupikindlustusleping arendaja katsetamiseks](https://azure.microsoft.com/offers/ms-azr-0023p/)                          | 0023P | Windows 10 |
| [Visual Studio Enterprise (MPN) tellijad](https://azure.microsoft.com/offers/ms-azr-0029p/)      | 0029P | Windows 10 |
| [Visual Studio Professional tellijad](https://azure.microsoft.com/offers/ms-azr-0059p/)          | 0059P | Windows 10 |
| [Visual Studio Test Professional tellijad](https://azure.microsoft.com/offers/ms-azr-0060p/)     | 0060P | Windows 10 |
| [Visual Studio Premium MSDN-(kasu)](https://azure.microsoft.com/offers/ms-azr-0061p/)       | 0061P | Windows 10 |
| [Visual Studio Enterprise tellijad](https://azure.microsoft.com/offers/ms-azr-0063p/)            | 0063P | Windows 10 |
| [Visual Studio Enterprise (BizSpark) tellijad](https://azure.microsoft.com/offers/ms-azr-0064p/) | 0064P | Windows 10 |
| [Ettevõtte arendaja katsetamiseks](https://azure.microsoft.com/ofers/ms-azr-0148p/)                              | 0148P | Windows 10 |


## <a name="check-your-azure-subscription"></a>Azure'i tellimuse kontrollimine
Kui te ei tea oma pakkumise ID-d, saate selle hankida Azure portaali või portaali konto kaudu.

'Tellimused' enne Azure portaali märgitakse pakkumise Tellimuse ID:

![Azure'i portaalis ID üksikasjade pakkumine](./media/virtual-machines-windows-client-images/offer_id_azure_portal.png) 

Saate vaadata pakkumise ID ['Tellimused' menüü](http://account.windowsazure.com/Subscriptions) Azure'i konto portaali kaudu:

![Portaalist Azure'i konto ID üksikasjade pakkumine](./media/virtual-machines-windows-client-images/offer_id_azure_account_portal.png) 


## <a name="next-steps"></a>Järgmised sammud
Nüüd saate oma VMs [PowerShelli](virtual-machines-windows-ps-create.md), [ressursihaldur Mallid](virtual-machines-windows-ps-template.md)või [Visual Studio](../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md)juurutada.
