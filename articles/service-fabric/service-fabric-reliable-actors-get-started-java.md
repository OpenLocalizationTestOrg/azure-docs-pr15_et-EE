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
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/25/2016"
   ms.author="vturecek"/>

# <a name="getting-started-with-reliable-actors"></a>Usaldusväärne osalejate töötamise alustamine

> [AZURE.SELECTOR]
- [C# Windowsis](service-fabric-reliable-actors-get-started.md)
- [Java Linux](service-fabric-reliable-actors-get-started-java.md)

Selles artiklis antakse ülevaade Azure teenuse struktuuri usaldusväärne osalejad ja juhatab teid läbi loomine ja juurutamine lihtne usaldusväärne näitleja rakendus Java.

## <a name="installation-and-setup"></a>Installimine ja häälestamine
Enne alustamist veenduge, et teil on teenuse struktuuri arenduskeskkond häälestamine teie arvutis.
Kui soovite selle häälestada, minge [võtnud Mac-](service-fabric-get-started-mac.md) või [Linux alustamine](service-fabric-get-started-linux.md).

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

## <a name="create-an-actor-service"></a>Teenuse näitleja loomine
Alustuseks loomine teenuse struktuuri uus rakendus. Teenuse struktuuri SDK Linuxi sisaldab väikemaaomanik generaator anda tellinguid teenuse struktuuri rakenduse kodakondsuseta teenusega. Käivitage järgmised Yeoman käsk:

```bash
$ yo azuresfjava
```

Järgige **Töökindlat näitleja**. Selles õpetuses mõeldud nime "HelloWorldActorApplication" rakendus ja osaleja "HelloWorldActor." Luuakse tellinguid järgmist:

```bash
HelloWorldActorApplication/
├── build.gradle
├── HelloWorldActor
│   ├── build.gradle
│   ├── settings.gradle
│   └── src
│       └── reliableactor
│           ├── HelloWorldActorHost.java
│           └── HelloWorldActorImpl.java
├── HelloWorldActorApplication
│   ├── ApplicationManifest.xml
│   └── HelloWorldActorPkg
│       ├── Code
│       │   ├── entryPoint.sh
│       │   └── _readme.txt
│       ├── Config
│       │   ├── _readme.txt
│       │   └── Settings.xml
│       ├── Data
│       │   └── _readme.txt
│       └── ServiceManifest.xml
├── HelloWorldActorInterface
│   ├── build.gradle
│   └── src
│       └── reliableactor
│           └── HelloWorldActor.java
├── HelloWorldActorTestClient
│   ├── build.gradle
│   ├── settings.gradle
│   ├── src
│   │   └── reliableactor
│   │       └── test
│   │           └── HelloWorldActorTestClient.java
│   └── testclient.sh
├── install.sh
├── settings.gradle
└── uninstall.sh
```

## <a name="reliable-actors-basic-building-blocks"></a>Usaldusväärne osalejate lihtsa koosteüksused

Põhimõtted, mis on kirjeldatud tõlkida põhilised koosteüksuste usaldusväärne näitleja teenuse.

### <a name="actor-interface"></a>Näitleja kasutajaliidese

See sisaldab kasutajaliidese definitsiooni osaleja. Selle kasutajaliidese määratleb näitleja lepingu, mis on jagatud näitleja rakendamist ja klientide kutsumine osaleja, et tavaliselt on mõistlik määrata kohas, kus on eraldi näitleja rakendamist ja ühiselt mitme teenuseid või klientrakendustes.

`HelloWorldActorInterface/src/reliableactor/HelloWorldActor.java`:

```java
public interface HelloWorldActor extends Actor {
    @Readonly   
    CompletableFuture<Integer> getCountAsync();

    CompletableFuture<?> setCountAsync(int count);
}
```

### <a name="actor-service"></a>Näitleja teenus 
See sisaldab teie näitleja rakendamise ja näitleja kood. Klassi näitleja rakendab näitleja kasutajaliidese. See on, kus teie näitleja ei oma töö.

`HelloWorldActor/src/reliableactor/HelloWorldActorImpl`:

```java
@ActorServiceAttribute(name = "HelloWorldActor.HelloWorldActorService")
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
public class HelloWorldActorImpl extends ReliableActor implements HelloWorldActor {
    Logger logger = Logger.getLogger(this.getClass().getName());

    protected CompletableFuture<?> onActivateAsync() {
        logger.log(Level.INFO, "onActivateAsync");

        return this.stateManager().tryAddStateAsync("count", 0);
    }

    @Override
    public CompletableFuture<Integer> getCountAsync() {
        logger.log(Level.INFO, "Getting current count value");
        return this.stateManager().getStateAsync("count");
    }

    @Override
    public CompletableFuture<?> setCountAsync(int count) {
        logger.log(Level.INFO, "Setting current count value {0}", count);
        return this.stateManager().addOrUpdateStateAsync("count", count, (key, value) -> count > value ? count : value);
    }
}
```

### <a name="actor-registration"></a>Näitleja registreerimine

Näitleja teenuse peab olema registreeritud teenuse tüüp teenuse struktuuri pikkus. Selleks näitleja teenuse oma näitleja eksemplaride käitamiseks, peab teie näitleja tüüp registreerunud näitleja teenuse. Funktsiooni `ActorRuntime` registreerimine meetod teeb selle töö osalejad.

`HelloWorldActor/src/reliableactor/HelloWorldActorHost`:

```java
public class HelloWorldActorHost {
    
    public static void main(String[] args) throws Exception {
        
        try {
            ActorRuntime.registerActorAsync(HelloWorldActorImpl.class, (context, actorType) -> new ActorServiceImpl(context, actorType, ()-> new HelloWorldActorImpl()), Duration.ofSeconds(10));

            Thread.sleep(Long.MAX_VALUE);
            
        } catch (Exception e) {
            e.printStackTrace();
            throw e;
        }
    }
}
```

### <a name="test-client"></a>Kliendi test

See on lihtne test kliendi rakendus käivitada eraldi testimiseks näitleja teenust rakendusest teenuse struktuuri. See on näide, kus saab kasutada funktsiooni ActorProxy aktiveerimiseks ja suhelda näitleja eksemplarid. Ei saa juurutada teie teenusega.

### <a name="the-application"></a>Rakendus 

Lõpuks rakenduse paketid näitleja teenuse ja muude teenuste võite lisada tulevikus koos juurutamiseks. See sisaldab näitleja teenuse paketi omanike *ApplicationManifest.xml* ja koht.

## <a name="run-the-application"></a>Käivitage rakendus

Funktsiooni Yeoman tellingud sisaldab gradle skripti rakenduse ja bash skriptide juurutada ja un-rakenduse juurutamine. Rakenduse käivitamiseks esmalt koostada gradle taotluse:

```bash
$ gradle
```

See loob teenuse struktuuri rakendusepaketi, mida saab kasutada teenuse struktuuri Azure'i CLI. Skripti install.sh sisaldab vajalikud Azure'i CLI käske rakenduse pakett. Lihtsalt käivitage install.sh skript juurutamiseks:

```bask
$ ./install.sh
```
