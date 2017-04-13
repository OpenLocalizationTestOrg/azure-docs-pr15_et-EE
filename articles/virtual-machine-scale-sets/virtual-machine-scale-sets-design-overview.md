<properties
    pageTitle="Määrab kujundamise virtuaalse masina skaala skaala | Microsoft Azure'i"
    description="Teada, kuidas kujundada oma virtuaalse masina skaala komplektid skaala"
    keywords="Linux virtuaalse masina, virtuaalse masina skaala määrab" 
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gatneil"
    manager="madhana"
    editor="tysonn"
    tags="azure-resource-manager" />

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="gatneil"/>

# <a name="designing-vm-scale-sets-for-scale"></a>Määrab kujundamise VM skaala skaala

Selles teemas käsitletakse kujundamine virtuaalse masina skaala komplektid. Mis on virtuaalse masina skaala komplekti kohta leiate teavet vaadake [Virtuaalse masina skaala komplekti ülevaade](virtual-machine-scale-sets-overview.md).


## <a name="storage"></a>Salvestusruumi

Ulatuse määramine kasutab salvestusruumi kontod talletamiseks OS ketast, VMs määramine. Soovitame suhte 20 VMs salvestusruumi konto kohta või vähem. Soovitame, et te laiali tähestiku algusest märkide salvestusruumi konto nimed. Tehes aitab laadi laiali süsteemidest sisemine. Näiteks järgmist malli kasutame uniqueString ressursihaldur malli funktsioon eesliite hashes, mis on täiendatud, et salvestusruumi kontonimed loomiseks: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat).


## <a name="overprovisioning"></a>Overprovisioning

Alustades "2016 03 30" API versioon, VM skaala komplektid vaikimisi "overprovisioning" VMs. Overprovisioning sisse lülitatud, skaala seadmine tegelikult keerutab üles VMs rohkem kui teilt küsida, siis kustutab eest VMs, mis kedratud viimase üles. Overprovisioning parandab ettevalmistamise edukust. On need eest VMs pole arve ja nad ei arvestata oma limiitide.

Kuigi overprovisioning parandada ettevalmistamise edukust, võib see põhjustada selgem käitumine rakendus, mis pole mõeldud käsitlema VMs kaovad ette teatamata. Teil on järgmine string malli tagada overprovisioning välja lülitamiseks: "unarusse": "väärtus false". Lisateavet leiate [VM skaala seadmine REST API dokumentatsiooni](https://msdn.microsoft.com/library/azure/mt589035.aspx).

Kui lülitate overprovisioning, te ei pääse salvestusruumi konto kohta suuremat suhte VMs, kuid me ei soovita läheb üle 40.


## <a name="limits"></a>Piirangud
Sisseehitatud kohandatud pildile (üks teie loodud) skaala kogum peate looma kõik OS ketta VHDs ühe salvestusruumi konto. Selle tulemusena maksimaalne soovitatav kogumi skaala sisseehitatud kohandatud pildile VMs arv on 20. Kui lülitate overprovisioning, saate avada kuni 40.

Skaala kogum tugineb platvormi pilt on praegu piiratud 100 VMs (soovitatav see skaala 5 salvestusruumi kontod).

Lisateavet vms kui need piirangud lubamiseks peate juurutamine skaala mitmele kriteeriumikogumile, nagu on näidatud [selle malli](https://github.com/Azure/azure-quickstart-templates/tree/master/301-custom-images-at-scale).