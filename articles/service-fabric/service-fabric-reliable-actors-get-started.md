<properties
   pageTitle="Alustamine teenuse struktuuri usaldusväärne osalejate | Microsoft Azure'i"
   description="Selles õpetuses juhendab teid juhiseid loomise, silumine ja juurutamine teenuse struktuuri usaldusväärne osalejate abil lihtsa näitleja põhises teenuses."
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
   ms.date="09/25/2016"
   ms.author="vturecek"/>

# <a name="getting-started-with-reliable-actors"></a>Usaldusväärne osalejate töötamise alustamine

> [AZURE.SELECTOR]
- [C# Windowsis](service-fabric-reliable-actors-get-started.md)
- [Java Linux](service-fabric-reliable-actors-get-started-java.md)

Selles artiklis antakse ülevaade Azure teenuse struktuuri usaldusväärne osalejad ja juhatab teid läbi loomise, silumine ja juurutamine lihtne usaldusväärne näitleja rakendus Visual Studios.

## <a name="installation-and-setup"></a>Installimine ja häälestamine
Enne alustamist veenduge, et teenuse struktuuri arenduskeskkond häälestamine teie arvutis on.
Kui soovite selle häälestada, lugege üksikasjalikke juhiseid kohta, [Kuidas häälestada on arenduskeskkond](service-fabric-get-started.md).

## <a name="basic-concepts"></a>Põhimõtted
Usaldusväärne osalejatega alustamiseks peate ainult mõne põhimõtted mõista:

 * **Näitleja teenus**. Usaldusväärne osalejate on pakitud usaldusväärsed teenused, teenuse struktuuri infrastruktuuri juurutatud. Näitleja eksemplarid on aktiveeritud nimega teenuse eksemplari.
 
 * **Näitleja registreerimise**. Kui usaldusväärne teenustega usaldusväärne näitleja teenuse peab olema registreerunud teenuse struktuuri käitusaja. Lisaks peab olema registreerunud näitleja käitusaja näitleja tüüp.
 
 * **Näitleja kasutajaliidese**. Näitleja kasutajaliidese kasutatakse määratleda tugevalt tipitud avaliku kasutajaliides, osalejale. Usaldusväärne näitleja mudeli terminoloogia, määratleb näitleja kasutajaliidese tüüpi sõnumid, mis saavad aru osaleja ja protsess. "" (Asünkroonselt) sõnumeid saata osaleja kasutatakse kasutajaliidese näitleja teiste osalejate ja klientrakendustes. Usaldusväärne osalejate saate rakendada mitme liidesed.
 
 * **ActorProxy klassi**. Klassi ActorProxy kasutatakse klientrakendustes autonoomsest meetodite näitleja kasutajaliidese kaudu. Klassi ActorProxy pakub kahte olulisi funktsioone.
    * Nime eraldusvõime: oleks võimalik leida osaleja klaster (kui see on majutatud klaster sõlme leidmine).
    * Tõrge töötlemine: see saab uuesti meetod manamine ja uuesti pärast, näiteks, mis nõuab osaleja määrama teise sõlme klaster tõrke lahendamiseks näitleja asukoht.

Järgmised reeglid, mis on seotud näitleja liidesed on mainida.

- Näitleja kasutajaliidese meetodite ei saa ülekoormatud.
- Näitleja kasutajaliidese meetodid ei tohi olla välja, ref või valikulised parameetrid.
- Üldise liideste ei toetata.

## <a name="create-a-new-project-in-visual-studio"></a>Visual Studio uue projekti loomine
Kui olete installinud Visual Studio teenuse struktuuri tööriistad, saate luua uue projekti tüübid. Uue projekti tüüp on **Uue projekti** dialoogiboksi kategoorias **pilve** .


![Visual Studio - uue projekti teenuse struktuuri tööriistad][1]

Järgmise dialoogiboksis saate valida tüüpi projekti, mida soovite luua.

![Teenuse struktuuri projekti Mallid][5]

Projekti HelloWorld kasutame teenuse struktuuri usaldusväärne osalejate teenus.

Kui olete loonud lahenduse, peaksite nägema järgmine struktuur:

![Teenuse struktuuri projekti struktuur][2]

## <a name="reliable-actors-basic-building-blocks"></a>Usaldusväärne osalejate lihtsa koosteüksused

Tüüpilised usaldusväärne osalejate lahenduse koosneb kolmest projektide:

* **Rakenduse project (MyActorApplication)**. See on projekt, et kõiki teenuseid koos juurutamiseks paketid. See sisaldab *ApplicationManifest.xml* ja PowerShelli skriptide haldamise rakendus.

* **Kasutajaliidese projekt (MyActor.Interfaces)**. See on projekt, mis sisaldab kasutajaliidese definitsiooni osaleja. MyActor.Interfaces projekti, saate määrata osalejate lahendus kasutatud liidesed. Saate oma näitleja liideste määratletud mis tahes nime, mis tahes Projecti aga kasutajaliideses määratleb näitleja lepingu, mis on ühiselt näitleja rakendamist ja klientide kutsumine osaleja, seega tavaliselt on mõistlik määrata eraldi näitleja rakendamist ja ühiselt mitu muud projektide komplekti.

```csharp
public interface IMyActor : IActor
{
    Task<string> HelloWorld();
}
```

* **Näitleja teenuse project (MyActor)**. See on projekti abil saate määratleda teenuse struktuuri teenus, et majutada osaleja läheb. See sisaldab osaleja rakendamine. Näitleja rakendatakse klassi, mis tuleneb alus tüüp `Actor` ja rakendab liidese/liideste, mis on määratletud MyActor.Interfaces projekt. Hõlmav tund näitleja peab rakendada ka ehitaja, mis võtab vastu ka `ActorService` eksemplari ja `ActorId` ja edastab need alus `Actor` klassi. See võimaldab ehitaja sõltuvus süsti platvormi sõltuvused.

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task<string> HelloWorld()
    {
        return Task.FromResult("Hello world!");
    }
}
```

Näitleja teenuse peab olema registreeritud teenuse tüüp teenuse struktuuri pikkus. Selleks näitleja teenuse oma näitleja eksemplaride käitamiseks, peab teie näitleja tüüp registreerunud näitleja teenuse. Funktsiooni `ActorRuntime` registreerimine meetod teeb selle töö osalejad.

```csharp
internal static class Program
{
    private static void Main()
    {
        try
        {
            ActorRuntime.RegisterActorAsync<MyActor>(
                (context, actorType) => new ActorService(context, actorType, () => new MyActor())).GetAwaiter().GetResult();

            Thread.Sleep(Timeout.Infinite);
        }
        catch (Exception e)
        {
            ActorEventSource.Current.ActorHostInitializationFailed(e.ToString());
            throw;
        }
    }
}

```

Kui alustate uue projekti Visual Studio ja teil on ainult üks näitleja määratlus, kaasatakse registreerimise vaikimisi Visual Studio genereeritud koodis. Kui määrate teenuse teisi osalejaid, peate lisama näitleja registreerimise abil:

```csharp
 ActorRuntime.RegisterActorAsync<MyOtherActor>();

```

> [AZURE.TIP] Teenuse struktuuri osalejate käitusaja eraldab mõned [näitleja meetodite seotud sündmused ja jõudluse hinnale](service-fabric-reliable-actors-diagnostics.md#actor-method-events-and-performance-counters). Need on kasulikud diagnostika- ja jõudluse jälgimine.


## <a name="debugging"></a>Silumine

Teenuse struktuuri tools for Visual Studio toetavad silumine oma kohalikus arvutis. Seansi lõpetamine silumine Alustuseks pihta klahvi F5. Visual Studio koostab (vajadusel) paketid. Lisaks kohaliku teenuse struktuuri klaster rakendus kasutab ja manustab siluri.

Juurutamise käigus saate vaadata edenemist **väljundi** aknas.

![Teenuse struktuuri silumine väljundi aknas][3]


## <a name="next-steps"></a>Järgmised sammud
 - [Kuidas kasutada usaldusväärne osalejate teenuse struktuuri platvormi](service-fabric-reliable-actors-platform.md)
 - [Näitleja olekus haldus](service-fabric-reliable-actors-state-management.md)
 - [Näitleja elutsükli ja rämpsfailide kogumine](service-fabric-reliable-actors-lifecycle.md)
 - [Näitleja API dokumentides](https://msdn.microsoft.com/library/azure/dn971626.aspx)
 - [Proovi kood](https://github.com/Azure/servicefabric-samples)


<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject.PNG
[2]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-projectstructure.PNG
[3]: ./media/service-fabric-reliable-actors-get-started/debugging-output.PNG
[4]: ./media/service-fabric-reliable-actors-get-started/vs-context-menu.png
[5]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject1.PNG
