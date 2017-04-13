<properties
   pageTitle="Kohalik jälgimine ja teenuste kirjutatud Azure teenuse struktuuri diagnoosimine | Microsoft Azure'i"
   description="Saate teada, kuidas jälgida ja diagnoosimine teenuste kirjutada, kasutades Microsoft Azure teenuse struktuuri kohaliku arengu arvutisse."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/24/2016"
   ms.author="subramar"/>


# <a name="monitor-and-diagnose-services-in-a-local-machine-development-setup"></a>Saate jälgida ja teenuste kohalikus arvutis arengu setup diagnoosimine


> [AZURE.SELECTOR]
- [Windows](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
- [Linux](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)

Teenuste minimaalsete häireid kasutusvõimalused jätkamiseks luba jälgimine, tuvastamine, diagnoosimise ja tõrkeotsing. Jälgimine ja diagnostika on juurutatud tootmisprotsessi keskkonnas. Sarnase mudeli vastuvõtmise ajal teenuste tagab, et diagnostika müügivõimaluste töötab tootmiskeskkonda teisaldamisel. Teenuse struktuuri hõlbustab teenuse arendajate diagnostika, mida saate sujuv töötamine nii ühe-seadme kohaliku arengu seadistuse ja tegelike tootmise kobar seadistuse rakendada.


## <a name="debugging-service-fabric-java-applications"></a>Teenuse struktuuri Java rakendusi silumine

Java rakenduste, [mitme logimine raamistiku](http://en.wikipedia.org/wiki/Java_logging_framework) on saadaval. Kuna `java.util.logging` on vaikimisi valitud JRE, kus on ka kasutatakse [github koodi näited](http://github.com/Azure-Samples/service-fabric-java-getting-started).  Pärast arutelu selgitab, kuidas konfigureerida selle `java.util.logging` raames. 
 
Java.util.logging abil saate suunata oma logid mälu, väljundi voole, konsooli failid või sockets. Iga need suvandid on vaikimisi käitleja juba andnud. Saate luua mõne `app.properties` faili konfigureerida faili sündmuseohjuri rakenduse kõik logid ümber kohalik fail. 

Järgmised koodilõigu sisaldab näiteks paigutus. 

```java 
handlers = java.util.logging.FileHandler
 
java.util.logging.FileHandler.level = ALL
java.util.logging.FileHandler.formatter = java.util.logging.SimpleFormatter
java.util.logging.FileHandler.limit = 1024000
java.util.logging.FileHandler.count = 10
java.util.logging.FileHandler.pattern = /tmp/servicefabric/logs/mysfapp%u.%g.log             
```

Poolt osutatud kausta selle `app.properties` faili peab olemas. Pärast selle `app.properties` fail on loodud, peate ka muuta oma kirje punkti skripti `entrypoint.sh` sisse selle `<applicationfolder>/<servicePkg>/Code/` kausta atribuudi `java.util.logging.config.file` abil `app.propertes` faili. Kirje peaks välja nägema järgmine koodilõigu.

```sh 
java -Djava.library.path=$LD_LIBRARY_PATH -Djava.util.logging.config.file=<path to app.properties> -jar <service name>.jar
```
 
 
Selle konfiguratsiooni, on tulemuseks logid kogutakse kohustused mood `/tmp/servicefabric/logs/`. **%U** ja **%g** luba loomise rohkem faile, kus failinimed mysfapp0.log ja mysfapp1.log jne. Vaikimisi kui pole sündmuseohjuri on konfigureeritud, konsooli sündmuseohjuri on registreeritud. Üks saate vaadata jaotises /var/log/syslog Logi logid.
 
Lisateabe saamiseks vaadake [github koodi näited](http://github.com/Azure-Samples/service-fabric-java-getting-started).  



## <a name="next-steps"></a>Järgmised sammud
Sama jälgimine koodi lisatakse teie rakendus töötab ka rakenduse kohta on Azure kobar diagnostika. Vaadake järgmisi artikleid, mida käsitletakse erinevaid võimalusi tööriistade ja kirjeldatakse, kuidas saate häälestada neid.
* [Kuidas koguda Azure'i diagnostika logid](service-fabric-diagnostics-how-to-setup-lad.md)
