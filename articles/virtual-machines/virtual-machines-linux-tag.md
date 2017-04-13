<properties
   pageTitle="Kuidas sildistamiseks Linux virtuaalse masina | Microsoft Azure'i"
   description="Siit saate teada, Linux virtuaalse masina Azure ressursihaldur juurutamise mudeli abil loodud sildistamise kohta."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="mmccrory"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="07/05/2016"
   ms.author="memccror"/>

# <a name="how-to-tag-a-linux-virtual-machine-in-azure"></a>Kuidas Linux virtuaalse masina Azure sildistamine

Selles artiklis kirjeldatakse sildistamiseks Linux virtuaalse masina Azure läbi ressursihaldur juurutamise mudeli erineval viisil. Sildid on kasutaja määratletud võti ja väärtuse paarideks, mille saab paigutada otse ressursi või ressursirühma. Azure'i toetab praegu kuni 15 siltide ressursside ja ressursirühma kohta. Siltide võib pandud ressursi loomise ajal või lisatakse mõne olemasoleva ressursi. Pange tähele, et sildid on toetatud ressursse, mis on loodud ressursihaldur juurutamise mudeli ainult kaudu.

[AZURE.INCLUDE [virtual-machines-common-tag](../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-azure-cli"></a>Azure'i CLI sildistamine

Alustage, [installida ja konfigureerida Azure CLI](../xplat-cli-azure-resource-manager.md) ja veenduge, et olete ressursihaldur režiimis (`azure config mode arm`).

Antud virtuaalse masina, siltide, sh selle käsu abil saate kuvada kõik atribuudid:

        azure vm show -g MyResourceGroup -n MyTestVM

Saate lisada uue VM silt Azure'i CLI kaudu, kuvatakse `azure vm set` käsk koos sildi parameeter **-t**:

        azure vm set -g MyResourceGroup -n MyTestVM –t myNewTagName1=myNewTagValue1;myNewTagName2=myNewTagValue2

Kõigi siltide eemaldamiseks saate kasutada **– T** parameeter on `azure vm set` käsk.

        azure vm set – g MyResourceGroup –n MyTestVM -T


Nüüd kus oleme rakendanud oma ressursse Azure CLI ja portaali sildid, vaatame pilk kasutusteavet siltide arvelduse portaalis kuvamiseks.

[AZURE.INCLUDE [virtual-machines-common-tag-usage](../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a>Järgmised sammud

* Oma Azure ressursse sildistamise kohta leiate lisateavet teemast [Azure ressursihaldur ülevaade][] ja [Korraldada oma Azure ressursse siltide abil][].
* Kuidas Sildid aitavad teil hallata oma Azure kasutamisele, leiate [Azure'i arve mõistmine][] ja [saada ülevaate oma Microsoft Azure'i ressursside tarbimine][].





[Azure CLI environment]: ./xplat-cli-azure-resource-manager.md
[Azure'i ressursihaldur ülevaade]: ../azure-resource-manager/resource-group-overview.md
[Siltide abil saate korraldada oma Azure'i ressursid]: ../resource-group-using-tags.md
[Azure'i arve mõistmine]: ../billing/billing-understand-your-bill.md
[Saada ülevaate oma Microsoft Azure'i ressursside tarbimine]: ../billing-usage-rate-card-overview.md
