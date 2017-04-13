<properties
   pageTitle="Teenuse struktuuri kobar ressursihaldur - paigutuse poliitikate | Microsoft Azure'i"
   description="Täiendavad paigutuse poliitikad ja reeglid teenuse struktuuri teenuste ülevaade"
   services="service-fabric"
   documentationCenter=".net"
   authors="masnider"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/19/2016"
   ms.author="masnider"/>

# <a name="placement-policies-for-service-fabric-services"></a>Paigutuse poliitikad teenuse struktuuri teenuste jaoks
On palju täiendavad reeglid, mida võite lõpuks hooliv kui teenuse struktuuri klaster on ulatuvad üle geograafilised kaugused, öelda mitme andmekeskuste või Azure regioonid, või kui teie keskkonnas ulatub üle mitme alade geopoliitiliste juhtelemendi (või mõne juhul, kui teil on teid huvitab legal või poliitika piirmäärad või seotud kaugused tegeliku jõudluse/latentsus mõjutada). Enamik neist saanud konfigureerida sõlm atribuudid ja paigutuse piiranguid kaudu, kuid mõned on keerulisem. Asjad, mida lihtsustamiseks pakume need täiendavad commmands. Nii nagu muude piirangutega paigutuse paigutuse poliitikaid saab konfigureerida teenuse kohta nimega eksemplari alusel.

## <a name="specifying-invalid-domains"></a>Vigaste domeenide määramine
InvalidDomain paigutuse poliitika võimaldab teil määrata, et teatud viga domeeni ei sobi see töökoormus. Selle poliitika tagab, et konkreetse teenuse töötab kunagi kindla ala, näiteks geopoliitilisi või ettevõtte poliitika põhjustel. Mitu sobimatu domeeni kaudu eraldi poliitika määrata.

![Sobimatu domeeni näide][Image1]

Kood:

```csharp
ServicePlacementInvalidDomainPolicyDescription invalidDomain = new ServicePlacementInvalidDomainPolicyDescription();
invalidDomain.DomainName = "fd:/DCEast"; //regulations prohibit this workload here
serviceDescription.PlacementPolicies.Add(invalidDomain);
```

PowerShelli:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("InvalidDomain,fd:/DCEast”)
```
## <a name="specifying-required-domains"></a>Nõutav domeenide määramine
Nõutav domeeni paigutuse poliitika nõuab kõigi stateful koopiad või kodakondsuseta eksemplare teenuse kohal määratud domeeni. Nõutav mitu domeeni saab määrata eraldi poliitikate kaudu.

![Nõutav domeeni näide][Image2]

Kood:

```csharp
ServicePlacementRequiredDomainPolicyDescription requiredDomain = new ServicePlacementRequiredDomainPolicyDescription();
requiredDomain.DomainName = "fd:/DC01/RK03/BL2";
serviceDescription.PlacementPolicies.Add(requiredDomain);
```

PowerShelli:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("RequiredDomain,fd:/DC01/RK03/BL2")
```

## <a name="specifying-a-preferred-domain-for-the-primary-replicas"></a>Eelistatud domeeni esmane koopiate määramine
Eelistatud Põhidomeen on huvitav juhtelement, kuna see lubab valiku viga domeeni, kus tuleks paigutada esmane, kui see on võimalik teha. Kui kõik on terved esmast lõpuks selles domeenis. Kui domeeni või peamine koopia fail või sulgeda mingil põhjusel esmast migreeritakse mõnda teise asukohta. Kui selle asukoha pole eelistatud domeeni, siis võimalusel kobar ressursihaldur liigub selle tagasi eelistatud domeeni. Loomulikult see säte on mõttekas ainult stateful teenuste jaoks. See on kõige enam kasu rühmades, mis on Azure piirkondade või mitmes andmekeskuses jagada. Sel juhul te kasutate kõigi kohtade jaoks koondamise, kuid te eelistate, et teatud asukohta paigutada esmane koopiad alumise latentsus toimingute puhul, mida minge esmast pakkumiseks (kirjutab ja ka vaikimisi kõik loeb pakutakse esmast järgi).

![Eelistatud esmane domeenid ja Tõrkesiirde][Image3]

```csharp
ServicePlacementPreferPrimaryDomainPolicyDescription primaryDomain = new ServicePlacementPreferPrimaryDomainPolicyDescription();
primaryDomain.DomainName = "fd:/EastUS/";
serviceDescription.PlacementPolicies.Add(invalidDomain);
```

PowerShelli:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("PreferredPrimaryDomain,fd:/EastUS")
```

## <a name="requiring-replicas-to-be-distributed-among-all-domains-and-disallowing-packing"></a>Nõudva koopiad levitatakse kõigi domeenid ja lubades pakkimine
Saate määrata mõne muu poliitika on vaja koopiad levitatakse alati saadaval viga domeenide vahel. See juhtub vaikimisi enamikel juhtudel, kus klaster on terve siiski on degenereerunud juhul, kui koopiate jaoks määratud sektsioon ajutiselt lõpuks pakitakse üksiku vea või täiendamine domeeni. Näiteks Oletame, et kuigi klaster on 9 sõlmed 3 viga domeenides (0, 1 ja 2) ja teenust on 3 koopiad, läks sõlmed, mida kasutati nende koopiate viga domeenide 1 ja 2 ning võimsus probleemide tõttu ükski sõlmi need domeenid ei sobi. Kui koostada asendamine nende koopiate teenuse struktuuri, kobar ressursihaldur oleks panna need viga domeeni 0, kuid mis loob olukorda, kus viga domeeni piirang on rikutud. Lisaks suureneb võimalus, et kogu koopia määramine võivad kaduma (kui on kadunud permananently FD 0). (Piiranguid ja piirangu kohta lisateabe saamiseks prioriteedid üldiselt, vaadake [selle teema](service-fabric-cluster-resource-manager-management-integration.md#constraint-priorities) )

Kui olete kunagi näinud seisund hoiatus nagu "Laadi koormusetasakaalustusteenuse tuvastas piirangu rikkumine selle koopia: struktuuri jaoks: /<some service name> teisene Partition <some partition ID> rikub piirangut: FaultDomain" klõpsamist olete see tingimus või midagi selle. Tavaliselt järgmist asjaolu on ajutine (sõlmed ei jää alla vana või kui nad ja tuleb koostada asendused seal on õige viga domeenides, mis kehtivad sõlmed), kuid on mõned töökoormus, et sooviksite pigem kättesaadavus riski kaotada kõigi nende koopiad. Me saame seda teha, määrates "RequireDomainDistribution" poliitika, mis tagavad pole kaks koopiad samasse sektsiooni kunagi lubatakse sama viga või täiendamine domeeni.

Mõned töökoormus pigem on kogu aeg (kihlveo vastu kokku domeeni tõrkeid ja teab, et nad tavaliselt tagasi kohaliku oleku), vastuste (osariik eksemplari) target arv teistele sooviksite pigem võtma soovitud tööseisakute varem risk õigsuse ja dataloss probleemid. Kuna enamik tootmise teenustest rohkem kui 3 koopiad, vaikeväärtus on ei nõua domeeni jaotuse ja lasta tasakaalustamiseks ja Tõrkesiirde toime juhul tavaliselt isegi juhul, kui see tähendab, et ajutiselt domeen on see pakitakse mitme koopiad.

Kood:

```csharp
ServicePlacementRequireDomainDistributionPolicyDescription distributeDomain = new ServicePlacementRequireDomainDistributionPolicyDescription();
serviceDescription.PlacementPolicies.Add(distributeDomain);
```

PowerShelli:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("RequiredDomainDistribution")
```

Nüüd oleks võimalik kasutada neid konfiguratsioone kobar, mis on geograafiliselt ulatuvad teenused? Kindel, et saaksite! Kuid ei ole hea põhjus liiga – eriti nõutav, sobimatu ja eelistatud domeeni konfiguratsioone säästmiseks juhul, kui teil on tegelikult geograafiliselt jaotatud kobar - ei mõttekas mis tahes üritage on antud töökoormus käivitamiseks ühe sektsioon või eelistan mõned lõigust kohaliku klaster üle teise erinevat tüüpi riistvara või töökoormus osadeks toimub välja , ja juhul võib käsitleda tavaline paigutuse piiranguid kaudu.

## <a name="next-steps"></a>Järgmised sammud
- Teenuste konfigureerida saadaolevate suvandite kohta lisateabe saamiseks vaadake teemat teema kohta muude kobar ressursihaldur konfiguratsioone saadaval [Services konfigureerimise kohta lugege](service-fabric-cluster-resource-manager-configure-services.md)

[Image1]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-invalid-placement-domain.png
[Image2]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-required-placement-domain.png
[Image3]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-preferred-primary-domain.png
