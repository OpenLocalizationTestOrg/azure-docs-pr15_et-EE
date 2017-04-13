<properties
   pageTitle="Alustamine usaldusväärsed teenused | Microsoft Azure'i"
   description="Sissejuhatus loomine Microsoft Azure teenuse struktuuri rakenduse kodakondsuseta ja stateful teenustega."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/26/2016"
   ms.author="vturecek"/>

# <a name="get-started-with-reliable-services"></a>Usaldusväärsed teenused kasutamise alustamine

> [AZURE.SELECTOR]
- [C# Windowsis](service-fabric-reliable-services-quick-start.md)
- [Java Linux](service-fabric-reliable-services-quick-start-java.md)

Selles artiklis antakse ülevaade Azure teenuse struktuuri usaldusväärsed teenused ja juhatab teid läbi loomine ja juurutamine lihtsa usaldusväärne teenuserakenduse kirjutatud Java.

## <a name="installation-and-setup"></a>Installimine ja häälestamine
Enne alustamist veenduge, et teil on teenuse struktuuri arenduskeskkond häälestamine teie arvutis.
Kui soovite selle häälestada, minge [võtnud Mac-](service-fabric-get-started-mac.md) või [Linux alustamine](service-fabric-get-started-linux.md).

## <a name="basic-concepts"></a>Põhimõtted
Alustada usaldusväärsed teenused, peate ainult mõne põhimõtted mõista:

 - **Teenuse tüüp**: see on teie teenuse rakendamist. See on määratletud klass peaksite kirjutama, mis laiendab `StatelessService` ja mis tahes muu kood või sõltuvused, nimi ja versiooninumber koos kasutada.

 - **Nimega eksemplari**: teenust käivitamiseks loote nimega eksemplarid oma konto tüüp ja palju nagu loote klassi tüüpi objekti eksemplari. Teenuse eksemplari on tegelikult objekti instantiations oma teenuse klassi, mida peaksite kirjutama. 

 - **Majutusteenus**: nimega teenuse linnanimede loomist peate käivitama host sees. Teenuse pakkuja on lihtsalt protsess kus saab käitada oma teenuseid.

 - **Teenuse registreerimine**: registreerimise koondab kõik. Teenuse struktuuri käitusaja majutusteenus lubamiseks teenuse struktuuri loomiseks eksemplari selle käivitamiseks tuleb registreerida teenuse tüüp.  

## <a name="create-a-stateless-service"></a>Kodakondsuseta teenuse loomine

Alustuseks loomine teenuse struktuuri uus rakendus. Teenuse struktuuri SDK Linuxi sisaldab väikemaaomanik generaator anda tellinguid teenuse struktuuri rakenduse kodakondsuseta teenusega. Käivitage järgmised Yeoman käsk:

```bash
$ yo azuresfjava
```

Järgige **Töökindlat kodakondsuseta**. Selles õpetuses mõeldud nime "HelloWorldApplication" rakenduste ja teenuste "HelloWorld". On tulemiks kataloogide jaoks soovitud `HelloWorldApplication` ja `HelloWorld`.

```bash
HelloWorldApplication/
├── build.gradle
├── HelloWorld
│   ├── build.gradle
│   └── src
│       └── statelessservice
│           ├── HelloWorldServiceHost.java
│           └── HelloWorldService.java
├── HelloWorldApplication
│   ├── ApplicationManifest.xml
│   └── HelloWorldPkg
│       ├── Code
│       │   ├── entryPoint.sh
│       │   └── _readme.txt
│       ├── Config
│       │   └── _readme.txt
│       ├── Data
│       │   └── _readme.txt
│       └── ServiceManifest.xml
├── install.sh
├── settings.gradle
└── uninstall.sh
```

## <a name="implement-the-service"></a>Rakendada teenus

Avage **HelloWorldApplication/HelloWorld/src/statelessservice/HelloWorldService.java**. See tund määratleb teenuse tüüp ja saate mis tahes koodi käivitada. Teenuse API pakub koodi viisidest:

 - Avatud kirje punkti meetodit, nimetatakse `runAsync()`, kus saate alustada käivitamisel kõik töökoormused, sh pikaajalisi Arvuta töökoormus.

```java
@Override
protected CompletableFuture<?> runAsync() {
    ...
}
```

 - Teatise kirje punkt kui ühendate communication virnas, valik. See on, kus saate alustada taotlusi saavate kasutajate ja muud teenused.

```java
@Override
protected List<ServiceInstanceListener> createServiceInstanceListeners() {
    ...
}
```

Selles õpetuses me keskenduda soovitud `runAsync()` punkti sisestusmeetod. See on, kus saate kohe alustada oma koodi käivitamist.

### <a name="runasync"></a>RunAsync

Platvormi kõned selle meetodi korral teenuse eksemplari paigutatud ja käivitada valmis. Teenuse eksemplari avamise/sulgemise tsükkel võib ilmneda mitu korda eluea teenuse kogu. Sellel võib olla erinevad põhjused, sh:

- Süsteemi viib oma teenuse eksemplari ressursi tasakaalustamiseks.
- Vead ilmneda koodi.
- Rakenduse või süsteemi on uuendatud.
- Aluseks oleva riistvara kogemusi mõne katkestuste.

Selle korraldamise hallatakse teenuse struktuuri hoida oma teenuse väga kättesaadav ja õigesti tasakaalustatud järgi.

#### <a name="cancellation"></a>Tühistamine

On oluline, et oma kood `runAsync()` saate peatada täitmise, kui edastatud teenuse struktuuri. Funktsiooni `CompletableFuture` tagastatud `runAsync()` tühistatakse, kui teenuse struktuuri nõuab teie teenuse peatamine täitmise. Järgmises näites näitab reageerimine tühistamise sündmuse: 

```java
    @Override
    protected CompletableFuture<?> runAsync() {

        CompletableFuture<?> completableFuture = new CompletableFuture<>();
        ExecutorService service = Executors.newFixedThreadPool(1);
        
        Future<?> userTask = service.submit(() -> {
            while (!Thread.currentThread().isInterrupted()) {
                try
                {
                   logger.log(Level.INFO, this.context().serviceName().toString());
                   Thread.sleep(1000);
                }
                catch (InterruptedException ex)
                {
                    logger.log(Level.INFO, this.context().serviceName().toString() + " interrupted. Exiting");
                    return;
                }
            }
         });
 
        completableFuture.handle((r, ex) -> {
            if (ex instanceof CancellationException) {
                userTask.cancel(true);
                service.shutdown();
            }
            return null;
        });
 
        return completableFuture;
   }
``` 

### <a name="service-registration"></a>Teenuse registreerimine

Teenuse andmetüübid peavad registreerunud teenuse struktuuri käitusaja. Teenuse tüüp on määratletud funktsiooni `ServiceManifest.xml` ja teenuse mis rakendab tunni `StatelessService`. Teenuse registreerimine toimub protsessi peamine koht. Selles näites on protsessi peamine koht `HelloWorldServiceHost.java`:

```java
public static void main(String[] args) throws Exception {
    try {
        ServiceRuntime.registerStatelessServiceAsync("HelloWorldType", (context) -> new HelloWorldService(), Duration.ofSeconds(10));
        logger.log(Level.INFO, "Registered stateless service type HelloWorldType.");
        Thread.sleep(Long.MAX_VALUE);
    } 
    catch (Exception ex) {
        logger.log(Level.SEVERE, "Exception in registration: {0}", ex.toString());
        throw ex;
    }
}
```

## <a name="run-the-application"></a>Käivitage rakendus

Funktsiooni Yeoman tellingud sisaldab gradle skripti rakenduse ja bash skriptide juurutada ja un-rakenduse juurutamine. Rakenduse käivitamiseks esmalt koostada gradle taotluse:

```bash
$ gradle
```

See loob teenuse struktuuri rakendusepaketi, mida saab kasutada teenuse struktuuri Azure'i CLI. Skripti install.sh sisaldab vajalikud Azure'i CLI käske rakenduse pakett. Lihtsalt käivitage install.sh skript juurutamiseks:

```bask
$ ./install.sh
```
