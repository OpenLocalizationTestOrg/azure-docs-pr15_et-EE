<properties
   pageTitle="Usaldusväärne osalejate sündmused | Microsoft Azure'i"
   description="Sissejuhatus sündmuste teenuse struktuuri usaldusväärne osalejatele."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/30/2016"
   ms.author="amanbha"/>


# <a name="actor-events"></a>Näitleja sündmused
Näitleja sündmuste võimalda kõige paremini-peegeldav teatiste osaleja saatmiseks klientidele. Näitleja sündmused on mõeldud näitleja-kliendi suhtlemine ja näitleja-näitleja suhtlemiseks kasutada.

Järgmised Koodilõigud Kuva näitleja sündmused rakenduse kasutamine.

Määratleda liides, mis kirjeldab sündmusi, mis on avaldatud osaleja. Selle kasutajaliidese peavad olema pärit on `IActorEvents` kasutajaliidese. Argumendid meetoditest peavad olema [andmed lepingu sarjadesse jaotatav](service-fabric-reliable-actors-notes-on-actor-type-serialization.md). Meetodid peab andma kehtetu, sündmuste teatised on üks võimalus ja parima.

```csharp
public interface IGameEvents : IActorEvents
{
    void GameScoreUpdated(Guid gameId, string currentScore);
}
```

Avaldatud näitleja liideses osaleja sündmuste deklareerida.

```csharp
public interface IGameActor : IActor, IActorEventPublisher<IGameEvents>
{
    Task UpdateGameStatus(GameStatus status);

    Task<string> GetGameScore();
}
```

Kliendi poolel, rakendada sündmuseohjuri.

```csharp
class GameEventsHandler : IGameEvents
{
    public void GameScoreUpdated(Guid gameId, string currentScore)
    {
        Console.WriteLine(@"Updates: Game: {0}, Score: {1}", gameId, currentScore);
    }
}
```

Klient, luua mõne osaleja, mis avaldab sündmuse-puhverserver ja tellimine oma sündmused.

```csharp
var proxy = ActorProxy.Create<IGameActor>(
                    new ActorId(Guid.Parse(arg)), ApplicationName);

await proxy.SubscribeAsync<IGameEvents>(new GameEventsHandler());
```

Korral failovers, osaleja ei pruugi üle sõlm või muu protsess. Puhverserveri näitleja haldab aktiivne tellimused ja automaatselt uuesti tellib need. Saate määrata, uuesti tellimuse intervalli kaudu soovitud `ActorProxyEventExtensions.SubscribeAsync<TEvent>` API. Tellimuse, kasutage funktsiooni `ActorProxyEventExtensions.UnsubscribeAsync<TEvent>` API.

Osaleja, klõpsake lihtsalt avaldada sündmuste toimuvaga. Kui sündmusele tellijad, saata osalejatele käitusaja neid teate.

```csharp
var ev = GetEvent<IGameEvents>();
ev.GameScoreUpdated(Id.GetGuidId(), score);
```

## <a name="next-steps"></a>Järgmised sammud
 - [Näitleja reentrancy](service-fabric-reliable-actors-reentrancy.md)
 - [Näitleja diagnostika- ja jõudluse jälgimine](service-fabric-reliable-actors-diagnostics.md)
 - [Näitleja API dokumentides](https://msdn.microsoft.com/library/azure/dn971626.aspx)
 - [Proovi kood](https://github.com/Azure/servicefabric-samples)
