<properties
   pageTitle="Loomine ja haldamine olekus | Microsoft Azure'i"
   description="Jõudlusprobleemi määratlemine ja teenuse oleku teenuse struktuuri haldamine"
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

# <a name="service-state"></a>Teenuse olek
**Teenuse olek** tähendab see teenus vajab töötamiseks andmeid. See sisaldab andmestruktuurid ja muutujate teenuse loeb ja kirjutab töötada.

Kaaluge antud kalkulaator teenuse, näiteks. See teenus võtab kahe arvu ja tagastab nende summa. See on puhtalt kodakondsuseta teenus, mis on seotud andmed.

Nüüd leiavad sama kalkulaator, kuid lisaks arvuti sum, on ka see on arvutatud viimase summa esitus meetod. See teenus on nüüd stateful – see sisaldab mõnda märkida, et see kirjutab (kui see avaldis arvutab uue summa) ja loeb (kui see tagastab viimase arvutatud summa).

Azure teenuse struktuuri, nimetatakse esimese teenuse kodakondsuseta teenus. Teine teenuse nimetatakse stateful teenus.

## <a name="storing-service-state"></a>Salvestamise teenuse olek
Maakond saate externalized või paiknevad koos koodis on käsitsemiseks olek. Hajutamise olek on tavaliselt teinud, kasutades väliste andmebaas või pood. Kalkulaator näites see võib olla SQL-andmebaasiga, kus talletatakse tabelis praeguse tulemi. Iga taotlus arvutada summa teeb värskendus, klõpsake selle rea.

Riik võib olla ka järgmine kood manipuleerib koodis koostööd asub. Teenuse struktuuri stateful teenused on loodud mudeli. Teenuse struktuuri pakub taristu tagamaks, et see on väga kättesaadav ja vea salliv tõrke korral.

## <a name="next-steps"></a>Järgmised sammud

Teenuse struktuuri põhimõtet kohta lisateabe saamiseks vaadake järgmist:

- [Teenuse struktuuri teenuste kättesaadavus](service-fabric-availability-services.md)

- [Teenuse struktuuri teenuste skaleeritavus.](service-fabric-concepts-scalability.md)

- [Eraldamine teenuse struktuuri teenused](service-fabric-concepts-partitioning.md)
