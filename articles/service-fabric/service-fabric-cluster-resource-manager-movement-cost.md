<properties
   pageTitle="Teenuse struktuuri kobar ressursihaldur: liikumine maksumus | Microsoft Azure'i"
   description="Liikumine maksumus teenuse struktuuri teenuste ülevaade"
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

# <a name="service-movement-cost-for-influencing-cluster-resource-manager-choices"></a>Teenuse liikumine hind mõjutavad kobar ressursihaldur Valikud
On oluline silmas pidada? kui proovite määrata, milliseid muudatusi teha arvutikobaras ja Keskmine lahendus on kogukulu lahenduse saavutamiseks.

Jooksev teenuse eksemplari või koopiad kulud CPU aega ja võrgu läbilaskevõime vähemalt. Stateful teenuste, samuti maksab vaheline kettal, mida peate looma olek koopia enne sulgemist vana koopiad. Soovite soovite selgelt lahenduse, tuleb Azure teenuse struktuuri kobar ressursihaldur minimeerida. Kuid ka ei soovi lahendusi, mis on eraldatud ressursside klaster oluliselt parandab ignoreerima.

Kobar ressursihaldur on kaks võimalust IT-kulud ja piirata, isegi siis, kui püüab vastavalt oma eesmärkide saavutamise klaster juhtida. Esimene on, et kavandamisel kobar ressursihaldur on uue paigutuse klaster, loeb iga liikuda, et see oleks. Antud juhul, kui saate kaks lahendusi sama kohta üldist jääk lõpus (keskmine) ja seejärel võtta üks madalam maksumus (viib koguarv).

See töötab päris hästi. Kuid koos vaike- või staatilise laadimise tõenäoliselt igal keerukate süsteemi, et kõik liikumine on võrdsed. Mõned on tõenäoliselt hoopis rohkem.

## <a name="changing-a-replicas-move-cost-and-factors-to-consider"></a>Muutke soovitud koopia Teisalda maksumus ja mõjutavad tegurid
Nimega aruandluse laadimine (teise funktsiooni kobar ressursihaldur), annate teenuse viis omas aruandlus, kuidas kulukas teenus on teatud ajal liikuda.

Kood:

```csharp
this.ServicePartition.ReportMoveCost(MoveCost.Medium);
```

MoveCost on neli taset: null, madal, Keskmine ja kõrge. Need on üksteise, välja arvatud null. Null tähendab, et teisaldada koopia on tasuta ja peaks ei arvestata lahendus Keskmine. On luua oma Teisalda maksumus kõrge *pole* garantii, et koopia ei liikuda lihtsalt, et seda ei teisaldata juhul, kui on head põhjust.

![Teisalda kulu kui tegur valimisel koopiad liikumine][Image1]

MoveCost abil saate otsida lahendusi, mis vähemalt häirida üldine on lihtsam saavutada endiselt saabuma võrdväärse saldo. Teenuse mõiste maksumus võib olla paljud asjad suhtes. Kõige levinum tegurid arvutamisel oma Teisalda maksumus on:

- Maakond või andmeid, mida soovite teisaldada teenuse summa.
- Klientide katkestamise maksumus. Liigub peamine koopia maksumus on tavaliselt suuremad kui kulud, teisaldamine teise koopia.
- Maksumus lennu toimingu katkestada. Mõned toimingud andmete talletamiseks taset või kliendi kõne vastuseks toimingud on kulukad. Pärast teatud punkti, te ei soovi neid peatada, kui teil pole vaja. Nii toimingu kestel saate tõstab kulu vähendamiseks tõenäosus, et liikuda teenuse koopia või eksemplari. Kui toiming on lõpule jõudnud, saate panna tagasi tavaline.

## <a name="next-steps"></a>Järgmised sammud
- Teenuse struktuuri kobar ressursi sõim kasutab mõõdikute tarbimine ja võimsus klaster juhtida. Mõõdikute ja nende konfigureerimise kohta lisateabe saamiseks vaadake [haldamine ressursside tarbimine ja teenuse struktuuri koos mõõdikute koormus](service-fabric-cluster-resource-manager-metrics.md).
- Kui soovite teada, kuidas kobar ressursihaldur haldab ja laadi klaster saldo, tutvuge [teenuse struktuuri klaster tasakaalustamiseks](service-fabric-cluster-resource-manager-balancing.md).

[Image1]:./media/service-fabric-cluster-resource-manager-movement-cost/service-most-cost-example.png
