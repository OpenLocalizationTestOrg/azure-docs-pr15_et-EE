<properties
   pageTitle="Polümorfismi, usaldusväärseid osalejate raames | Microsoft Azure'i"
   description="Koostada hierarhiad .NET liideste ja tüüpi usaldusväärne osalejate raames uuesti kasutada funktsioone ja API määratlusi."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/07/2016"
   ms.author="seanmck"/>

# <a name="polymorphism-in-the-reliable-actors-framework"></a>Polümorfismi, usaldusväärseid osalejate raames

Usaldusväärne osalejate raames võimaldab teil koostada kasutavad paljud sama meetodid, mida soovite kasutada objekti rakendusse kujundus. Üks neid võtteid on polümorfismi, mis võimaldab ja liidesed, mis pärivad Lisateavet üldiste vanemad. Üldiselt järgib pärimise, usaldusväärseid osalejate raames .NET mudel mõned täiendavad piirangud.

## <a name="interfaces"></a>Liidesed

Usaldusväärne osalejate raames nõuab vähemalt üks kasutajaliidese rakendab oma näitleja tüübi määratlemine. Selle kasutajaliidese kasutatakse puhverserveri ainekursuse, mida saab kasutada klientide suhelda oma osalejad. Liideste saate pärivad muude liideste, kui iga kasutajaliides, mis rakendatakse soovitud tüüpi näitleja ja kõik selle vanemad lõpuks tulenevad IActor. IActor on platvormi määratletud base kasutajaliidese osalejatele. Seetõttu võib klassikaline polümorfismi näide abil kujundeid välja nägema umbes selline:

![Kujundi osalejatele kasutajaliidese hierarhia][shapes-interface-hierarchy]


## <a name="types"></a>Tüübid

Samuti saate luua hierarhia näitleja tüübid, mis on saadud base näitleja klassi, mis on esitatud platvormi. Puhul kujunditel, peate võib-olla alus `Shape` tüüp:

```csharp
public abstract class Shape : Actor, IShape
{
    public abstract Task<int> GetVerticeCount();

    public abstract Task<double> GetAreaAsync();
}
```

Alatüüpide `Shape` saate alistada meetodite alus.

```csharp
[ActorService(Name = "Circle")]
[StatePersistence(StatePersistence.Persisted)]
public class Circle : Shape, ICircle
{
    public override Task<int> GetVerticeCount()
    {
        return Task.FromResult(0);
    }

    public override async Task<double> GetAreaAsync()
    {
        CircleState state = await this.StateManager.GetStateAsync<CircleState>("circle");

        return Math.PI *
            state.Radius *
            state.Radius;
    }
}
```

Märkus selle `ActorService` atribuut näitleja tüüp. Atribuudi ütleb usaldusväärne näitleja raames, et see peaks automaatselt luua hosting osalejatega seda tüüpi teenus. Mõnel juhul võite base tüüp, mis on mõeldud ainult ühiselt alamtüüpi funktsioonid ja väärtustada konkreetsete osalejate kunagi kasutada. Sel juhul tuleks kasutada funktsiooni `abstract` märksõna, mis näitab, et loote selle tüüp osalejale kunagi.


## <a name="next-steps"></a>Järgmised sammud

- Vaadake, [Kuidas usaldusväärset osalejate raames mõjutab teenuse struktuuri platvormi](service-fabric-reliable-actors-platform.md) töökindluse, skaleeritavus ja ühtsed.
- Lisateavet [näitleja elutsükli](service-fabric-reliable-actors-lifecycle.md).

<!-- Image references -->

[shapes-interface-hierarchy]: ./media/service-fabric-reliable-actors-polymorphism/Shapes-Interface-Hierarchy.png
