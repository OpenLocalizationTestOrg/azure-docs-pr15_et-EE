<properties
   pageTitle="Usaldusväärne osalejate märkmeid näitleja tippige sariväljaanne | Microsoft Azure'i"
   description="Käsitletakse nõuetele sarjadesse jaotatav tunnid, teenuse struktuuri usaldusväärne osalejate Ühendriigid ja liideste määratlemiseks kasutatavate määratlemine"
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
   ms.date="10/19/2016"
   ms.author="vturecek"/>

# <a name="notes-on-service-fabric-reliable-actors-type-serialization"></a>Märkmete teenuse struktuuri usaldusväärne osalejatele tippige sariväljaanne


Kõik meetodid, tulemitüübid tagastatud iga meetodi näitleja liides, tööülesannete ja mõne osaleja olekus halduri talletatud objektide argumendid peavad olema [Andmed lepingu sarjadesse jaotatav](https://msdn.microsoft.com/library/ms731923.aspx). See kehtib ka argumendid määratletud [näitleja sündmuse liideste](service-fabric-reliable-actors-events.md#actor-events)meetoditest. (Näitleja sündmuse kasutajaliidese meetodite alati tagasi kehtetuks.)

## <a name="custom-data-types"></a>Kohandatud andmetüübid

Selles näites järgmised näitleja kasutajaliidese määratleb meetod, mis tagastab kohandatud andmetüüpi, mida nimetatakse `VoicemailBox`.

```csharp
public interface IVoiceMailBoxActor : IActor
{
    Task<VoicemailBox> GetMailBoxAsync();
}
```

Kasutajaliideses on impelemented näitleja, mis kasutab State halduri talletamiseks soovitud `VoicemailBox` objekti:

```csharp
[StatePersistence(StatePersistence.Persisted)]
public class VoiceMailBoxActor : Actor, IVoicemailBoxActor
{
    public VoiceMailBoxActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task<VoicemailBox> GetMailboxAsync()
    {
        return this.StateManager.GetStateAsync<VoicemailBox>("Mailbox");
    }
}

```

Selles näites on `VoicemailBox` objekt on seeriasertide kui:
 - Objekti edastatakse näitleja eksemplar ja helistaja vahel.
 - Objekt on salvestatud olekus Manager, kui see on jätkunud ketas ja kopeeritud sõlmi.
 
Usaldusväärne näitleja raames kasutab DataContract sariväljaanne. Seetõttu kohandatud andmeobjektid ja nende liikmed, peab olema märgitud atribuudid **DataContract** ja **selleDataMember** vastavalt

```csharp
[DataContract]
public class Voicemail
{
    [DataMember]
    public Guid Id { get; set; }

    [DataMember]
    public string Message { get; set; }

    [DataMember]
    public DateTime ReceivedAt { get; set; }
}
```

```csharp
[DataContract]
public class VoicemailBox
{
    public VoicemailBox()
    {
        this.MessageList = new List<Voicemail>();
    }

    [DataMember]
    public List<Voicemail> MessageList { get; set; }

    [DataMember]
    public string Greeting { get; set; }
}
```

## <a name="next-steps"></a>Järgmised sammud
 - [Näitleja elutsükli ja rämpsfailide kogumine](service-fabric-reliable-actors-lifecycle.md)
 - [Näitleja taimerid ja meeldetuletused](service-fabric-reliable-actors-timers-reminders.md)
 - [Näitleja sündmused](service-fabric-reliable-actors-events.md)
 - [Näitleja reentrancy](service-fabric-reliable-actors-reentrancy.md)
 - [Näitleja polümorfismi ja mustrid objekti suunatud kujundamine](service-fabric-reliable-actors-polymorphism.md)
 - [Näitleja diagnostika- ja jõudluse jälgimine](service-fabric-reliable-actors-diagnostics.md)
