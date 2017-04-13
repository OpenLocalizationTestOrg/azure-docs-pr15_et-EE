<properties
   pageTitle="Teenuse struktuuri kobar ressursihaldur ahendamine | Microsoft Azure'i"
   description="Siit saate teada, pidurdab, mis on esitatud, teenuse struktuuri kobar ressursihaldur konfigureerimiseks."
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


# <a name="throttling-the-behavior-of-the-service-fabric-cluster-resource-manager"></a>Käitumine teenuse struktuuri kobar ressursihaldur ahendamine
Isegi juhul, kui olete kobar ressursihaldur õigesti konfigureeritud, saate hankida katkenud klaster. Näiteks võib olla samaaegne sõlm või viga domeeni tõrgete – mis juhtub, kui leiab aset versiooniuuenduse? Ressursihaldur proovib parima väljanägemise parandamiseks kõike, kuid korda niimoodi võiksite kaaluda riskipõhistele nii, et ise klaster on võimalus tasakaalustamiseks (tulemas uuesti teha sõlmed, võrgu läbilaskevõime heal ise, parandatud bittide kasutada). Teid aidata olukordi, teenuse struktuuri kobar ressursihaldur kaasata mitu pidurdab. Tähele, et need pidurdab on üsna häiriva ja üldiselt ei tohiks kasutada juhul, kui seal on mõned ettevaatlik matemaatika valmis ümber paralleelselt töö, mida saab teha tegelikult klaster ning sagedased hulga peavad vastama need igasuguseid (ahem) planeerimata makroskoopiline ümberkonfigureerimine sündmused (AKA: "Väga halbade päeva").

Üldiselt soovitame vältida väga halbade päeva läbi muude suvandite (nt tavaline koodi värskendused ja vältida overscheduling klaster alustada) asemel ahendamine klaster abil ressursid parandada ise püüdes takistamiseks). Funktsiooni pidurdab on vaikeväärtused, mis leidsime kaudu kogemusi, et ok vaikesätted, kuid peaksite ilmselt Heitke pilk ja häälestada neid oma oodatud tegeliku laadi. Ajal pole liiga või laadimise klaster on võite määrata, et on hea tava juhtumid, mis (seni, kuni te saate need parandamiseks) kus peab teil olema pidurdab paar kohas, isegi kui see tähendab, et klaster kauem tasakaalustamiseks.

##<a name="configuring-the-throttles"></a>Funktsiooni pidurdab konfigureerimine
Mis on vaikimisi kaasatud pidurdab on:

-   GlobalMovementThrottleThreshold – seda määrab liikumise klaster koguarv teatud aja (määratletud GlobalMovementThrottleCountingInterval, väärtuse sekundites)
-   MovementPerPartitionThrottleThreshold – seda määrab liikumise jaoks mis tahes teenuse partition koguarv teatud aja (MovementPerPartitionThrottleCountingInterval, väärtuse sekundites)

``` xml
<Section Name="PlacementAndLoadBalancing">
     <Parameter Name="GlobalMovementThrottleThreshold" Value="1000" />
     <Parameter Name="GlobalMovementThrottleCountingInterval" Value="600" />
     <Parameter Name="MovementPerPartitionThrottleThreshold" Value="50" />
     <Parameter Name="MovementPerPartitionThrottleCountingInterval" Value="600" />
</Section>
```

Pange tähele, et enamasti oleme kliendid kasutada nende pidurdab on seda näinud sest seal on juba vahendid on piiratud keskkonnas (nt piiratud võrgu läbilaskevõime üksikud sõlmed või ketast, mille paralleelsete koopia nõuetele ei koostab, mis suunati neid) mis mõeldud toiminguid ei õnnestu või oleks aeglane ikkagi.  Sel juhul kliendid olid mugav teab, mis need olid potentsiaalselt ulatub aega võtab kobar saavutamiseks ühed olekusse, sh teab, et nad võivad lõpuks alumise üldine töökindluse samal ajal, kui need olid rakendus ei tööta.

## <a name="next-steps"></a>Järgmised sammud
- Kohta, kuidas kobar ressursihaldur haldab ja saldo laadi klaster teada saada, vaadake artikkel [tasakaalustamiseks laadimine](service-fabric-cluster-resource-manager-balancing.md)
- Ressursihaldur kobar on palju võimalusi klaster kirjeldamiseks. Kui soovite teada saada lisateavet nende väljamöllimine see artikkel [kirjeldab teenuse struktuuri kobar](service-fabric-cluster-resource-manager-cluster-description.md)
