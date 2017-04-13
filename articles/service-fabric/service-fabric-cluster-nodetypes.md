<properties
   pageTitle="Teenuse struktuuri tüüpi ja VM skaala komplektid | Microsoft Azure'i"
   description="Kirjeldatakse, kuidas teenuse struktuuri tüüpi seotud VM skaala komplektid ja kuidas kaugtöölaua ühenduse VM skaala seadmine eksemplari või kobar sõlm."
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/09/2016"
   ms.author="chackdan"/>


# <a name="the-relationship-between-service-fabric-node-types-and-virtual-machine-scale-sets"></a>Teenuse struktuuri tüüpi ja virtuaalse masina skaala komplektid seos

Virtuaalse masina skaala terminikomplektid on Azure arvutada ressursi abil saate juurutada ja hallata kogumi virtuaalmasinates kogumina. Iga sõlm tüüp, mis on määratletud teenuse struktuuri kobar on seadistada eraldi VM skaala määramine. Iga sõlm tüüp saab siis ülespoole või allapoole sõltumatult, on erinevaid avatud pordid ja võib olla eri võimsuse mõõdikute.

Kuvatakse järgmine ekraanipilt klaster, mis on kahte tüüpi: FrontEnd ja Taustväärtus.  Iga sõlm tüüp on viis sõlmed.

![Kuvatõmmis kobar, mis sisaldab kahte tüüpi][NodeTypes]

## <a name="mapping-vm-scale-set-instances-to-nodes"></a>Vastendus sõlmed eksemplarid VM skaala määramine

Nagu näete kohale, VM skaala seadmine eksemplarid algavad eksemplari 0 ja seejärel läheb üles. Nummerduse kajastub nimed. Näiteks BackEnd_0 on Taustväärtus VM skaala seadmine 0 eksemplari. Selle kindla VM skaala on viis eksemplari, nimega BackEnd_0, BackEnd_1, BackEnd_2, BackEnd_3 ja BackEnd_4.

Kui muudate VM skaala seadistamine on loodud uue eksemplari. Uue VM skaala seadmine eksemplari nimi on tavaliselt VM skaala seadmine nimi + järgmise eksemplari number. Selles näites on BackEnd_5.


## <a name="mapping-vm-scale-set-load-balancers-to-each-node-typevm-scale-set"></a>Vastendamise VM skaala määratud koormus soolise iga sõlm tüüp/VM skaala määramine

Kui olete juurutanud klaster portaali kaudu või pole varem valimi ressursihaldur Mall, mida me registreerimisel, siis kuvatakse kõik ressursirühma ressursside loendi seejärel kuvatakse koormus soolise iga VM skaala seadmine või sõlm tüübi.

Nimi oleks umbes: **LB -&lt;NodeType nimi&gt;**. Näiteks LB-sfcluster4doc-0, nagu pildil näidatud:


![Ressursid][Resources]


## <a name="remote-connect-to-a-vm-scale-set-instance-or-a-cluster-node"></a>Remote ühenduse VM skaala seadmine eksemplari või kobar sõlm
Iga sõlm tüüp, mis on määratletud klaster on seadistada eraldi VM skaala määramine.  See tähendab, et sõlm tüüpi võib mastaabitud üles või alla sõltumatult ja saab erinevate VM SKU-de jaoks. Erinevalt VMs ühekordsest VM skaala seadmine eksemplarid ei saa oma virtuaalse IP-aadress. Nii see võib olla veidi keerulisem, kui otsite IP-aadress ja pordi, mille abil saate kaugtöölaua ühenduse eksemplariga.

Siin on toodud juhised võite avastada, et need.

### <a name="step-1-find-out-the-virtual-ip-address-for-the-node-type-and-then-inbound-nat-rules-for-rdp"></a>Samm 1: Teada sõlm tüüp virtuaalse IP-aadress ja seejärel RDP NAT sissetulevad reeglid

Selleks, et saada, et vajate sissetuleva NAT reeglite väärtused ressursi definitsiooni **Microsoft.Network/loadBalancers**osana määratletud.

Portaali, liikuge soovitud laadi koormusetasakaalustusteenuse blade ja seejärel valige **sätted**.

![LBBlade][LBBlade]


Klõpsake jaotises **sätted** **NAT sissetulevad reeglid**. Nüüd pakub IP-aadress ja pordi, mille abil saate kaugtöölaua ühenduse kõigepealt VM skaala määramine. Pildil on **104.42.106.156** ja **3389**

![NATRules][NATRules]

### <a name="step-2-find-out-the-port-that-you-can-use-to-remote-connect-to-the-specific-vm-scale-set-instancenode"></a>Samm 2: Teada portide, mille abil saate Remote ühenduse teatud VM skaala seadmine eksemplari/sõlm

Selle dokumendi rääkisin kuidas sõlmi kaart VM skaala seadmine eksemplaris. Kasutame mis aru saada täpse pordi.

Pordid on eraldatud tõusvas järjestuses VM skaala seadmine astme. nii, et minu FrontEnd sõlm tüübi näiteks pordid iga viis eksemplari on järgmised. Nüüd peate tegema oma VM skaala seadmine eksemplari sama vastendust.

|**Näiteks VM skaala**|**Port**|
|-----------------------|--------------------------|
|FrontEnd_0|3389|
|FrontEnd_1|3390|
|FrontEnd_2|3391|
|FrontEnd_3|3392|
|FrontEnd_4|3393|
|FrontEnd_5|3394.|


### <a name="step-3-remote-connect-to-the-specific-vm-scale-set-instance"></a>Samm 3: Remote ühenduse teatud näiteks VM skaala määramine

Järgneval pildil kasutada kaugtöölaua ühenduse loomiseks soovitud FrontEnd_1:

![RDP][RDP]

## <a name="how-to-change-the-rdp-port-range-values"></a>Kuidas muuta RDP port vahemiku väärtused

### <a name="before-cluster-deployment"></a>Enne kobar juurutamine

Kui häälestate klaster on ressursihaldur malli abil saate määrata **inboundNatPools**vahemiku.

Minge **Microsoft.Network/loadBalancers**ressursi definitsiooni. Jaotises, mille leiate **inboundNatPools**kirjeldus.  *FrontendPortRangeStart* ja *frontendPortRangeEnd* väärtuste asendamine.

![InboundNatPools][InboundNatPools]


### <a name="after-cluster-deployment"></a>Pärast kobar juurutamine
See on küll natuke keerulisem ja saada ümber töödelda VMs põhjustada. Nüüd tuleb määrata uued väärtused Azure PowerShelli kaudu. Veenduge, et teie arvutisse on installitud Azure PowerShelli 1.0 või uuem versioon. Kui te pole seda teinud enne, ma kindlalt soovitame, et järgite teemas kirjeldatud juhised [Kuidas installida ja konfigureerida Azure PowerShelli.](../powershell-install-configure.md)

Azure'i kontosse sisse logida. Kui mingil põhjusel nurjub selle PowerShelli käsu, kontrollige kas teil on õigesti installitud Azure PowerShelli.

```
Login-AzureRmAccount
```

Käivitage järgmine üksikasjade saamiseks klõpsake oma laadi koormusetasakaalustusteenuse, kuvatakse selle kirjeldust **inboundNatPools**väärtused:

```
Get-AzureRmResource -ResourceGroupName <RGname> -ResourceType Microsoft.Network/loadBalancers -ResourceName <load balancer name>
```

Nüüd seatud *frontendPortRangeEnd* ja *frontendPortRangeStart* soovitud väärtused.

```
$PropertiesObject = @{
    #Property = value;
}
Set-AzureRmResource -PropertyObject $PropertiesObject -ResourceGroupName <RG name> -ResourceType Microsoft.Network/loadBalancers -ResourceName <load Balancer name> -ApiVersion <use the API version that get returned> -Force
```


## <a name="next-steps"></a>Järgmised sammud

- [Ülevaade funktsiooni "Mis tahes juurutamine" ja Azure haldusega kogumite võrdlus](service-fabric-deploy-anywhere.md)
- [Kobar turvalisus](service-fabric-cluster-security.md)
- [Teenuse struktuuri SDK ja alustamine](service-fabric-get-started.md)


<!--Image references-->
[NodeTypes]: ./media/service-fabric-cluster-nodetypes/NodeTypes.png
[Resources]: ./media/service-fabric-cluster-nodetypes/Resources.png
[InboundNatPools]: ./media/service-fabric-cluster-nodetypes/InboundNatPools.png
[LBBlade]: ./media/service-fabric-cluster-nodetypes/LBBlade.png
[NATRules]: ./media/service-fabric-cluster-nodetypes/NATRules.png
[RDP]: ./media/service-fabric-cluster-nodetypes/RDP.png
