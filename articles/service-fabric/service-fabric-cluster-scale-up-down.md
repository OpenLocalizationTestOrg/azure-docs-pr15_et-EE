<properties
   pageTitle="Teenuse struktuuri kobar sisse- või väljapoole mastaapimiseks | Microsoft Azure'i"
   description="Skaala teenuse struktuuri kobar sisse- või väljapoole seadmisega mastaabi automaatselt reeglid iga sõlm tüüp/VM skaala kogumi nõudmisel vastavaks. Lisage või eemaldage sõlmed teenuse struktuuri kobar"
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/09/2016"
   ms.author="chackdan"/>


# <a name="scale-a-service-fabric-cluster-in-or-out-using-auto-scale-rules"></a>Mastaapimiseks teenuse struktuuri kobar sisse- või mastaabi automaatselt reeglite kasutamine

Virtuaalse masina skaala terminikomplektid on Azure Arvuta ressurss, mille abil saate juurutada ja hallata kogumi virtuaalmasinates kogumina. Iga sõlm tüüp, mis on määratletud teenuse struktuuri kobar on häälestatud eraldi VM skaala kogum. Iga sõlm tüüp saab siis ülespoole sisse või välja sõltumatult, on avatud pordid erinevaid ja võib olla eri võimsuse mõõdikute. Lugege lisateavet selle kohta, [teenuse struktuuri nodetypes](service-fabric-cluster-nodetypes.md) dokumenti. Kuna teenuse struktuuri, sõlm tüüpi klaster on tehtud VM skaala määrab selle kirjutamata, peate igas kogumis sõlm tüüp/VM skaala mastaabi automaatselt reeglid häälestada.

>[AZURE.NOTE] Teie tellimus peab olema piisavalt tuuma lisada uue VMs, mis moodustavad see. Ei ole mudeli kinnitamine praegu pääsemiseks juurutamise aja tõrge, kui mis tahes limiitide on tulemus.

## <a name="choose-the-node-typevm-scale-set-to-scale"></a>Valige sõlm tüüp/VM skaala skaala määramine

Praegu ei ole võimalik määrata VM skaala komplektid portaalis reeglid mastaabi automaatselt, nii andke meile kasutada Azure PowerShelli (1.0 +) loendis soovitud tüüpi ja seejärel lisage mastaabi automaatselt reeglid neile.

VM skaala komplekti, mis moodustavad klaster loendi saamiseks käivitage järgmised cmdlet-käsud:

```powershell
Get-AzureRmResource -ResourceGroupName <RGname> -ResourceType Microsoft.Compute/VirtualMachineScaleSets

Get-AzureRmVmss -ResourceGroupName <RGname> -VMScaleSetName <VM Scale Set name>
```

## <a name="set-auto-scale-rules-for-the-node-typevm-scale-set"></a>Määrake mastaabi automaatselt reeglid sõlm tüüp/VM skaala määramine

Kui klaster on mitut tüüpi, siis Korrake seda iga sõlm tüüpi/VM skaala määrab, et soovite mastaapimiseks (sisse või välja). Arvesse võtta sõlmed arvu, et teil on automaatne skaleerimist häälestamiseks. Minimaalne arv sõlmed, mis peab teil olema esmane sõlm tüüp liigub valitud töökindluse tase. Lugege lisateavet [usaldusväärsuse](service-fabric-cluster-capacity.md).

>[AZURE.NOTE]  Esmane sõlm alla skaleerimist tippige alla minimaalne arv tehke ebastabiilne kobar või viia see. See võib põhjustada andmekao oma rakenduste ja teenuste süsteem.

Praegu on tingitud funktsiooni mastaabi automaatselt laadimise, mis võivad rakenduste aru teenuse struktuuri. Seega sel ajal, kuvatakse automaatselt skaala juhivad puhtalt soovitud jõudluse väidab, et iga VM põhjustel skaala määrata eksemplarid.  

Järgige neid juhiseid [iga VM skaala kogumi mastaabi automaatselt häälestada](../virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview.md).

>[AZURE.NOTE] Skaalal stsenaarium alla juhul, kui teie sõlm tüüp on kestvus kuld või hõbe peate [cmdlet-käsk Eemalda-ServiceFabricNodeState](https://msdn.microsoft.com/library/azure/mt125993.aspx) vastav sõlm nimega helistada.

## <a name="manually-add-vms-to-a-node-typevm-scale-set"></a>Käsitsi lisada VMs sõlm tüüp/VM skaala määramine

Järgige [Lühijuhend malli Galerii](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) VMs iga Nodetype numbri muutmiseks. 

>[AZURE.NOTE] Lisades vms võtab aega, nii, et oodata täiendused olema kohene. Nii planeerida võimsus lisada ka ajal, mis võimaldab 10 minuti jooksul enne selle koopiate puuduvad VM võimalused / teenuse eksemplari hankimine paigutada.

## <a name="manually-remove-vms-from-the-primary-node-typevm-scale-set"></a>Käsitsi eemaldamine VMs esmane sõlm tüüp/VM skaala määramine

>[AZURE.NOTE] Teenuse struktuuri süsteemi teenused töötavad on tippige esmane sõlm klaster. Nii, et peaksite kunagi sulgeda või mastaapimiseks seda tüüpi eksemplaride arvu saab väiksem kui töökindluse taseme. Vaadake [töökindluse astme siin üksikasju](service-fabric-cluster-capacity.md). 

Peate täita järgmised juhised ühe VM eksemplari korraga. See võimaldab süsteemi teenuseid (ja stateful teenuste) nõtkelt sulgeda VM eksemplari eemaldate ja uue loodud sõlmi koopiad.

1. Käivitage [Keela-ServiceFabricNode](https://msdn.microsoft.com/library/mt125852.aspx) tahtlikult 'RemoveNode' keelamiseks sõlme, mida te ei kavatse eemaldada (seda tüüpi sõlm kõrgeim eksemplari).

2. Käivitage [Get-ServiceFabricNode](https://msdn.microsoft.com/library/mt125856.aspx) veenduge, et sõlme transitioned tõepoolest on keelatud. Kui ei, siis oodake, kuni sõlme on keelatud. Seda toimingut ei saa kiire.

2. Järgige [Lühijuhend malli Galerii](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) muuta ühe selle Nodetype VMs arv. Eemaldatud eksemplari on kõrgeim VM eksemplari. 

3. Korrake juhiseid 1 – 3 vastavalt vajadusele, kuid kunagi mastaapimiseks esmane sõlm tüüpi vähem kui mis töökindluse taseme saab eksemplaride arvu. Vaadake [töökindluse astme siin üksikasju](service-fabric-cluster-capacity.md). 

## <a name="manually-remove-vms-from-the-non-primary-node-typevm-scale-set"></a>Käsitsi eemaldamine VMs-esmane sõlm tüüp/VM skaala määramine

>[AZURE.NOTE] Stateful teenus, tuleb teil tuleb alati kuni säilitada kättesaadavus ja säilitada oma teenuse olekut ja sõlmed arv. Väga minimaalne, peate sõlmed arv võrdne target koopia määratud sektsioon/teenuse. 

Peate execute järgmised juhised ühe VM eksemplari korraga. See võimaldab süsteemi teenuste (ja teie stateful teenused) nõtkelt sulgeda VM eksemplari eemaldate ja uue koopiad loodud veel, kus.

1. Käivitage [Keela-ServiceFabricNode](https://msdn.microsoft.com/library/mt125852.aspx) tahtlikult 'RemoveNode' keelamiseks sõlme, mida te ei kavatse eemaldada (seda tüüpi sõlm kõrgeim eksemplari).

2. Käivitage [Get-ServiceFabricNode](https://msdn.microsoft.com/library/mt125856.aspx) veenduge, et sõlme transitioned tõepoolest on keelatud. Kui mitte ootama, kuni sõlme on keelatud. Seda toimingut ei saa kiire.

2. Järgige [Lühijuhend malli Galerii](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) muuta ühe selle Nodetype VMs arv. See eemaldab nüüd kõrgeim VM eksemplari. 

3. Korrake juhiseid 1 – 3 vastavalt vajadusele, kuid kunagi mastaapimiseks esmane sõlm tüüpi vähem kui mis töökindluse taseme saab eksemplaride arvu. Vaadake [töökindluse astme siin üksikasju](service-fabric-cluster-capacity.md).

## <a name="behaviors-you-may-observe-in-service-fabric-explorer"></a>Märkate teenuste struktuuri Exploreris käitumist

Kui klaster skaalal teenuse struktuuri Exploreri kajastuvad sõlmed (VM skaala määratud eksemplaris), mis on osa klaster arv.  Kui muudate on klaster allapoole, saate vaadata eemaldatud sõlm/VM eksemplari, mis kuvatakse juhul, kui helistate [Eemalda-ServiceFabricNodeState cmd](https://msdn.microsoft.com/library/mt125993.aspx) vastav sõlm nimega on vigane olekus.   

Siin on selgitus seda käitumist.

Teenuse struktuuri Exploreris esitatud sõlmed on peegeldus, mis on teenuse struktuuri teenuste (FM spetsiaalselt) teab sõlmed klaster oli/on arv. Kui muudate VM skaala seadmine, VM on kustutatud, kuid FM süsteemiteenusel endiselt arvab sõlm (VM, mis on kustutatud vastendatud) tulevad tagasi. Nii teenuse struktuuri Exploreri jätkub kuvamiseks sõlme (kuigi seisundioleku võib olla viga või tundmatu).

Selleks, et veenduda, et sõlm VM eemaldamisel eemaldatakse, on teil kaks võimalust:

1) Valige soovitud kestvus kuld või hõbe (saadaval kiiresti) sõlm tüüpi klaster, mis annab teile taristu integreerimine. Mis seejärel automaatselt eemaldab sõlmed meie süsteemi (FM) teenuste oleku kui skaalal.
Viidata [kestvus tasemel siin üksikasjad](service-fabric-cluster-capacity.md)

2) Kui VM eksemplari on vähendatud, peate helistama [cmdlet-käsk Eemalda-ServiceFabricNodeState](https://msdn.microsoft.com/library/mt125993.aspx).

>[AZURE.NOTE] Teenuse struktuuri kogumite nõua teatud sõlmed olevat kõik ajal säilitamiseks kättesaadavus ja säilitamine riigi - edaspidi "säilitades kvoorumi." Niisiis, on tavaliselt ebaturvaliste sulgeda kõik masinad klaster juhul, kui teie tehtud esmalt [oleku täielik varukoopia](service-fabric-reliable-services-backup-restore.md).

## <a name="next-steps"></a>Järgmised sammud
Lugege ka Lisateavet kavandamise kobar võimsus üleminekut klaster ning eraldamine teenuste järgmist:

- [Teie kobar võimsus kavandamine](service-fabric-cluster-capacity.md)
- [Kobar uuendamine](service-fabric-cluster-upgrade.md)
- [Sektsiooni stateful teenuste skaala](service-fabric-concepts-partitioning.md)

<!--Image references-->
[BrowseServiceFabricClusterResource]: ./media/service-fabric-cluster-scale-up-down/BrowseServiceFabricClusterResource.png
[ClusterResources]: ./media/service-fabric-cluster-scale-up-down/ClusterResources.png
