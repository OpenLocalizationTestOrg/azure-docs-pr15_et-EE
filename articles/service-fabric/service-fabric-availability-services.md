<properties
   pageTitle="Teenuse struktuuri teenuste olemasolu | Microsoft Azure'i"
   description="Kirjeldab viga tuvastamise, Tõrkesiirde ja teenuste taastamine"
   services="service-fabric"
   documentationCenter=".net"
   authors="appi101"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/10/2016"
   ms.author="aprameyr"/>

# <a name="availability-of-service-fabric-services"></a>Teenuse struktuuri teenuste kättesaadavus
Azure teenuse struktuuri teenuste võib olla stateful või kodakondsuseta. Selles artiklis antakse ülevaade sellest, kuidas teenuse struktuuri väidab kättesaadavuse teenuse ebaõnnestumise korral.

## <a name="availability-of-service-fabric-stateless-services"></a>Teenuse struktuuri kodakondsuseta teenuste kättesaadavus
Kodakondsuseta teenus on rakenduse teenus, mis ei ole [kohaliku püsiv olek](service-fabric-concepts-state.md).

Loomise kodakondsuseta teenus nõuab määratlemine eksemplari arv, mis on kodakondsuseta teenus, mis peaks olema installitud klaster eksemplaride arv. See on rakenduse loogika, mis luuakse klaster eksemplaride arv. Eksemplaride arvu on soovitatav viis kodakondsuseta teenuse ülespoole.

Tõrgete tuvastamisel kodakondsuseta teenuse eksemplaris luuakse uus mõned õigus sõlme klaster.

## <a name="availability-of-service-fabric-stateful-services"></a>Teenuse struktuuri stateful teenuste kättesaadavus
Stateful teenus on mõned sellega seotud olekus. Klõpsake teenuse struktuuri, stateful teenus on kujundatud koopiad kogumina. Iga koopia on teenus, mis on koopia riigi kood eksemplari. Lugemine ja kirjutamine on läbi ühe koopia (nimetatakse esmast). Muudatused, kust kirjutada, lähevad *kopeeritud* mitme muude koopiatega (nn aktiivse sekundaaride). Põhi- ja aktiivne teisene koopiad kombinatsioon on teenuse koopia määramine.

Võib olla ainult üks peamine koopia teenindamine lugemine ja kirjutamine kutsed, kuid ei saa mitme aktiivse teisene koopiad. Aktiivse teisene koopiad on konfigureeritav ja vastuste suurem arv saate luba suurema hulga samaaegseid tarkvara ja riistvara tõrkeid.

Korral (kui see peamine koopia läheb alla) viga, teenuse struktuuri teeb ühe aktiivse teisene koopiad uue peamine koopia. Selle aktiivne teise koopia on juba värskendatud versiooni olek (kaudu *dispersioonanalüüs*) ja seda jätkata töötlemine edasise lugeda ja kirjutada.

Selle kontseptsiooni--koopia, primaarne või aktiivne teisese--nimetatakse koopia roll.

### <a name="replica-roles"></a>Koopia rollid
Roll koopia abil saate hallata elutsükli riigi haldavad selle koopia. Koopia, kelle rolli on esmane teenuste lugeda taotlused. IT teenused kirjutamiseks taotlused värskendamise seisu ja imitatsiooniga muudatused aktiivse sekundaaride selle koopia komplekti. Roll on aktiivne teisese on see peamine koopia on kopeeritud muutuste vastu ja värskendada oma ülevaate.

>[AZURE.NOTE] Kõrgema taseme programmeerimise mudelid nagu [usaldusväärne osalejate framework](service-fabric-reliable-actors-introduction.md) abstraktne ära mõiste koopia rolli arendaja.

## <a name="next-steps"></a>Järgmised sammud

Teenuse struktuuri põhimõtet kohta lisateabe saamiseks vaadake järgmist:

- [Teenuse struktuuri teenuste skaleeritavus.](service-fabric-concepts-scalability.md)

- [Eraldamine teenuse struktuuri teenused](service-fabric-concepts-partitioning.md)

- [Loomine ja haldamine olek](service-fabric-concepts-state.md)
