<properties
   pageTitle="Usaldusväärne teenus arhitektuur | Microsoft Azure'i"
   description="Usaldusväärne teenus arhitektuur stateful ja kodakondsuseta teenuste ülevaade"
   services="service-fabric"
   documentationCenter=".net"
   authors="AlanWarwick"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="03/30/2016"
   ms.author="alanwar"/>

# <a name="architecture-for-stateful-and-stateless-reliable-services"></a>Arhitektuur stateful ja kodakondsuseta usaldusväärsed teenused

Azure'i teenus struktuuri usaldusväärne teenus ei pruugi olla stateful või kodakondsuseta. Igat tüüpi teenuse käivitatakse teatud arhitektuur sees. Selles artiklis kirjeldatakse neid arhitektuurides.
Vt [töökindlat ülevaade](service-fabric-reliable-services-introduction.md) stateful ja kodakondsuseta teenuste vaheliste erinevuste kohta lisateavet.

## <a name="stateful-reliable-services"></a>Stateful usaldusväärsed teenused

### <a name="architecture-of-a-stateful-service"></a>Stateful teenuse arhitektuur
![Stateful teenuse arhitektuur skeem](./media/service-fabric-reliable-services-platform-architecture/reliable-stateful-service-architecture.png)

### <a name="stateful-reliable-service"></a>Stateful töökindlat

Stateful töökindlat saate tulenevad StatefulService või StatefulServiceBase klassi. Nii need base klassid on esitatud teenuse struktuuri. Nad pakuvad tugi ja võtmiseks stateful teenuse kasutajaliides, kus teenuse struktuuri--ja osaleda teenust jooksul teenuse struktuuri kobar erinevaid tasemeid.

StatefulService tuleneb StatefulServiceBase. StatefulServiceBase pakub rohkem paindlikkust, kuid nõuab aru saada, teenuse struktuuri sees.
Leiate lisateavet klõpsake soovitud üksikasjad kirjutamise teenuste StatefulService ja StatefulServiceBase tunnid abil [usaldusväärne ülevaade](service-fabric-reliable-services-introduction.md) ja [töökindlat Täpsemad kasutus](service-fabric-reliable-services-advanced-usage.md) .

Nii base klassi hallata neid elu jooksul ja teenuse rakendamise roll. Teenuse rakendamise võib virtuaalse meetodid, kas base klassi alistada, kui teenuse rakendamise on tööd teha neid teenuse rakendamist elutsükli--punkte või kui soovite luua side kuulajale objekti. Pange tähele, et kuigi teenuse rakendamise võivad rakendada oma side kuulajale objekti ICommunicationListener, asetades eeltoodud diagrammi side kuulajale rakendatakse teenuse struktuuri--nagu teenuse rakendamise kasutab side kuulajale, mis rakendatakse teenuse struktuuri.

Stateful töökindlat kasutab ära kogumite, usaldusväärseid usaldusväärne olekus ülemus. Usaldusväärne saidikogumid on teenus--, mis on väga saadaolevate kohaliku andmestruktuurid, nad on alati saadaval, olenemata teenuse failovers. Erinevat tüüpi usaldusväärne saidikogumi rakendatakse usaldusväärne oleku pakkuja.
Usaldusväärne saidikogumite kohta leiate lisateavet teemast [usaldusväärne saidikogumid ülevaade](service-fabric-reliable-services-reliable-collections.md).

### <a name="reliable-state-manager-and-state-providers"></a>Usaldusväärne olekus juhataja ja state pakkujad

Usaldusväärne olekus manager on objekti, mida haldab usaldusväärne olekus pakkujad. See on funktsioonide loomine, kustutamine, loetlemine ja tagada usaldusväärne riigi pakkujate nõutud ja väga kättesaadav. Usaldusväärne olekus pakkuja eksemplari tähistab eksemplari nõutud ja tugevalt saadaval andmestruktuur, nt sõnastiku või järjekorda.

Iga usaldusväärne oleku pakkuja seab liidest, mida kasutatakse stateful teenuse suhtlevad usaldusväärne oleku pakkuja. Näiteks kasutatakse IReliableDictionary usaldusväärne sõnastiku, kasutatakse IReliableQueue liides liides usaldusväärne järjekorda. Usaldusväärne riigi kõigi teenusepakkujate rakendada IReliableState kasutajaliidese.

Usaldusväärne olekus manager on nimega IReliableStateManager, mis lubab juurdepääsu sellele stateful teenusest kasutajaliides. Usaldusväärne olekus pakkujate liidesed tagastatakse IReliableStateManager kaudu.

Usaldusväärne olekus ülemus kasutab lisandmooduli arhitektuur, et uut tüüpi usaldusväärne saidikogumid saate olema ühendatud dünaamiliselt.

Usaldusväärne sõnastik ja usaldusväärne järjekorda ehitatud suure jõudlusega, versiooniga erinevat poe rakendamine.

### <a name="transactional-replicator"></a>Selgituseks paljundaja

Selgituseks paljundaja osa on tagama, et (ehk teisisõnu öeldes olek on usaldusväärne olekus haldur ja usaldusväärne saidikogumid) teenuse olek on ühesugune kõikides kõik koopiad, kus töötab teenus. See tagab ka, et olek on jätkunud Logi sisse. Usaldusväärne olekus halduri liidesed selgituseks paljundaja kaudu privaatne süsteem.

Selgituseks paljundaja kasutab võrgu protokolli olek muude koopiad eksemplari suhelda nii, et kõik koopiad on ajakohast teavet.

Selgituseks paljundaja kasutab Logi püsida teavet nii, et riik teavet survives protsessi või sõlm jookseb. Logi kasutajaliides on privaatne süsteemi kaudu.

### <a name="log"></a>Log

Log komponent pakub suure jõudlusega püsiv pood, mida saab optimeerida kirjutamiseks juurde ketramiseks või tahkis kettale.  Püsivate Storage (st kõvaketta) peab olema kohaliku sõlmi, kus töötab teenus stateful Logi kujundus. See võimaldab madal latentsused ja kõrge läbilaskevõime, võrreldes remote püsiv hoidla, mis ei ole kohaliku sõlme.

Log komponent kasutab mitmest logifailist. On sõlm kogu ühiskasutusega logifaili kasutavate kõigi koopiate võib see pakkuda latentsus madalaimate ja kõrgeimate läbilaskevõime andmete talletamiseks. Vaikimisi paigutatakse teenuse struktuuri sõlm töö directory ühiskasutusega Logi, kuid see võib olla konfigureeritud ka mõnes teises asukohas ideaalvariandis mitte ainult ühiskasutusega Logi reserveeritud kettal paigutada. Iga teenuse jaoks koopia on ka asjakohast logifaili ja sihtotstarbeline log paigutatakse teenuse töö Directory. On pole süsteemi konfigureerida sihtotstarbeline Logi muusse asukohta paigutada.

Ühiskasutusega log on ülemineku ala koopia olekus leiate sihtotstarbeline logifaili on kui see on jätkunud sihtkohta. See kujundus on filtreerimisoleku teavet esmalt kirjutatud ühiskasutusega logifaili ja laisalt teisaldatakse sihtotstarbeline logifaili taustal. Sel viisil oleks kirjutamine ühiskasutusega Logi madalam latentsus ja kõrgeim läbilaskevõime, mis võimaldab teenuse edenemise kiiremini teha.

Loeb ja kirjutab ühiskasutusega log on valmis kaudu otse IO süsteemimälus ühiskasutusega logifaili jaoks vaba ruumi. Optimaalse kasutamise kettaruumi draivil sihtotstarbeline logid lubamiseks sihtotstarbeline logifaili luuakse NTFS vähe failina. Pange tähele, et see lubab overprovisioning kettaruumi ja OS näitab abil palju kettaruumi, kui tegelikult kasutatakse asjakohast logifailid.

Peale minimaalsete kasutaja-režiimis liidest Logi Logi kirjutatakse tuumrežiimi draiver. Tuum-režiimis juht töötab, saate Logi anda kõik teenused, mis seda kasutada suurima jõudlus.

Logi konfigureerimise kohta leiate lisateavet teemast [Seadistamine stateful usaldusväärsed teenused](service-fabric-reliable-services-configuration.md).

## <a name="stateless-reliable-service"></a>Kodakondsuseta töökindlat

### <a name="architecture-of-a-stateless-service"></a>Kodakondsuseta teenuse arhitektuur
![Kodakondsuseta teenuse arhitektuur skeem](./media/service-fabric-reliable-services-platform-architecture/reliable-stateless-service-architecture.png)

### <a name="stateless-reliable-service"></a>Kodakondsuseta töökindlat

Kodakondsuseta teenuse rakendusi tulenevad StatelessService või StatelessServiceBase klassi. Klassi StatelessServiceBase võimaldab rohkem paindlikkust kui StatelessService klassi.
Nii base klassi hallata neid elu jooksul ja teenuse roll.

Teenuse rakendamise võib virtuaalse meetodid, kas base klassi alistada, kui teenus on tööd teha neid teenuse elutsükli--punkte või kui soovite luua side kuulajale objekti. Pange tähele, et kuigi teenuse võivad rakendada oma side kuulajale objekti ICommunicationListener, asetades eeltoodud diagrammi side kuulajale rakendatakse teenuse struktuuri, nagu selle teenuse rakendamise kasutab side kuulajale, mis rakendatakse teenuse struktuuri.

Leiate lisateavet klõpsake soovitud üksikasjad kirjutamise teenuseid kasutades StatelessService ja StatelessServiceBase tunnid [usaldusväärne ülevaade](service-fabric-reliable-services-introduction.md) ja [töökindlat Täpsemad kasutus](service-fabric-reliable-services-advanced-usage.md) .

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Järgmised sammud

Teenuse struktuuri kohta leiate lisateavet teemast:

[Usaldusväärne teenus ülevaade](service-fabric-reliable-services-introduction.md)

[Lühijuhend](service-fabric-reliable-services-quick-start.md)

[Usaldusväärne saidikogumid ülevaade](service-fabric-reliable-services-reliable-collections.md)

[Täpsemad kasutus töökindlat](service-fabric-reliable-services-advanced-usage.md)

[Usaldusväärne teenuse konfigureerimine](service-fabric-reliable-services-configuration.md)  
