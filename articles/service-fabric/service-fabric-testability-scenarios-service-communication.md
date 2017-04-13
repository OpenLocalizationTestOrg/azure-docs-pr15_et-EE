<properties
   pageTitle="Testability: Teenuse side | Microsoft Azure'i"
   description="Teenusest teatis on teenuse struktuuri rakenduse kriitilised integreerimine punkti. Selles artiklis käsitletakse kujundamine ja testimise meetodite abil."
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
   ms.date="07/06/2016"
   ms.author="vturecek"/>

# <a name="service-fabric-testability-scenarios-service-communication"></a>Teenuse struktuuri testability stsenaariumid: teenuse suhtlus

Microservices ja teenusele suunatud arhitektuuri laadide pinna loomulikult Azure teenuse struktuuri. Jaotatud arhitektuurides tüüpi componentized microservice rakenduste koosnevad tavaliselt mitmes teenuses, mida on vaja üksteisega rääkida. Veelgi lihtsam juhtudel teil on üldiselt vähemalt kodakondsuseta veebiteenuse ja stateful andmete salvestusteenus, mida on vaja suhelda.

Teenusest teatis on kriitiline integreerimine punkti rakenduse, kuna iga teenuse seab remote API muude teenustega. API piirmäärad kogumi, mis hõlmab I/O üldiselt töötamise jaoks on vaja mõned ettevaatlik hea summa testimine ja valideerimine.

On mitmeid kaalutlused teha, kui need teenuse piirid on jaotatud süsteemis koos juhtmega.

 - *Protokolli*. Kas kavatsete kasutada jaoks maksimaalse läbilaskevõime HTTP jaoks suurendatakse või kohandatud kahendarvu protokolli?
 - *Tõrge töötlemise*. Püsivate ja siirdamiseks tõrgete käsitlemise? Mis juhtub, kui teenus liigub erineva sõlme?
 - *Ajalõpud ja latentsusaeg*. Multitiered rakendustes, kuidas iga teenuse kiht tegeleb latentsus kaudu virnas ja kasutajale?

Kas kasutate ühte sisseehitatud teenuse side komponendid esitatud teenuse struktuuri või koostate oma, testimine kasutusviisid teenuste vahel on oluline, et tagada paindlikkust oma rakenduse.

## <a name="prepare-for-services-to-move"></a>Teenuste liikumiseks ettevalmistamine

Teenuse eksemplari liikumine võivad aja jooksul. See kehtib eriti juhul, kui need on konfigureeritud laadi mõõdikute kohandatud kohandatud ressursside optimaalse tasakaalustamiseks. Teenuse struktuuri viib oma eksemplare maksimeerimiseks nende kättesaadavus isegi ajal uuendamine, failovers, skaala-out ja muud hajutatud süsteemi kestuse jooksul toimuvate olukordades.

Nagu klaster liikuda teenused, kliendid ja muude teenuste peaks olema valmis hakkama kahte järgmist stsenaariumi, kui nad rääkida teenuse:

- Teenuse eksemplari või sektsiooni koopia on pärast viimast rääkisin selle teisaldada. See on tavaline osa teenuse elutsükli ja see tuleb oodata rakenduse käigus tehakse.
- Teenuse eksemplari või sektsiooni koopia on protsess liigub. Kuigi Tõrkesiirde ühe sõlme teise teenuse ilmneb väga kiiresti teenuse struktuuri, võib esineda viivituse kättesaadavus kui teenust side osa on aeglane alustada.

Need stsenaariumid nõtkelt töötlemise on oluline sujuvalt süsteemi. Selleks, pidage meeles, et:

- Igal teenusel, mille saab ühendada on *meiliaadress* , mis seda kuulab (nt HTTP või WebSockets). Kui eksemplari või sektsiooni liigub, muudab oma aadressi lõpp-punkti. (See viib erinevate sõlm eri IP-aadressiga.) Kui kasutate sisseehitatud side komponendid, nad hakkama uuesti lahendamise teenuse aadressi jaoks.
- Domeenil võib olla ajutine kasv teenuse latentsus nimega teenuse eksemplari käivitab oma kuulajale uuesti. See sõltub sellest, kuidas kiiresti teenuse avab kuulajale pärast eksemplari on teisaldatud.
- Sulgeda ja uuesti avada pärast teenuse avatakse uus sõlm tuleb kõik olemasolevad ühendused. Taaskäivitus või graatsiline sõlm sulgemine võimaldab aega sulgeda nõtkelt olemasolevad ühendused.

### <a name="test-it-move-service-instances"></a>Test: eksemplaride teisaldamine

Teenuse struktuuri testability tööriistade abil saate Autor test stsenaarium testimiseks järgmist asjaolu erineval viisil:

1. Liikumine stateful teenuse peamine koopia.

    Peamine koopia stateful teenuse sektsiooni saab teisaldada mis tahes arvul põhjustel. Selle abil saate suunata peamine koopia kindlasse sektsiooni näha, kuidas teenuste reageerida teisaldamist väga kontrollitud viisil.

    ```powershell

    PS > Move-ServiceFabricPrimaryReplica -PartitionId 6faa4ffa-521a-44e9-8351-dfca0f7e0466 -ServiceName fabric:/MyApplication/MyService

    ```

2. Peatage sõlm.

    Kui sõlm on peatatud, teenuse struktuuri liigub kõik eksemplare või sektsioonid, mis olid sõlme ühele teiste saadaval sõlme klaster. Kasutage seda testimiseks olukorda, kus klaster ja kõik eksemplarid teenuse katkemise sõlm ja sõlme koopiad on liikuda.

    Soovi korral saate peatada sõlm, kasutades PowerShelli **Peata-ServiceFabricNode** cmdlet-käsk:

    ```powershell

    PS > Restart-ServiceFabricNode -NodeName Node_1

    ```

## <a name="maintain-service-availability"></a>Säilitada teenuse kättesaadavus

Platvormi, teenuse struktuuri on mõeldud teie teenuste kõrge olemasolu. Kuid äärmiselt juhtudel saate endiselt aluseks oleva taristu probleeme põhjustada andnud. See on oluline testimine need stsenaariumid liiga.

Stateful teenuste abil ise riigi jaoks kõrge-saadavus kvoorumi põhinev süsteem. See tähendab, et on koopiad peab olema saadaval kirjutamine toimingute tegemiseks. Harva, näiteks levinud riistvara tõrge, ei pruugi koopiad on saadaval. Sellisel juhul te ei saa kirjutamine toiminguid, kuid ikka saab lugeda toiminguid.

### <a name="test-it-write-operation-unavailability"></a>Test: toiming andnud kirjutamine

Teenuse struktuuri testability tööriistade abil saate annavad viga, mis põhjustab kvoorumi kaotsimineku testi. Kuigi sellise stsenaariumi on harva, on oluline, et kliendid ja teenuseid, mis sõltuvad stateful teenuse on valmis hakkama olukordades, kus nad ei saa teha, kirjutage taotluste sellele. See on oluline, et stateful teenusesse on seda võimalust teadma ja saate seda edastavad nõtkelt helistajad.

Võib põhjustada kvoorumi kaotsimineku, kasutades PowerShelli **Invoke-ServiceFabricPartitionQuorumLoss** cmdlet-käsk:

```powershell

PS > Invoke-ServiceFabricPartitionQuorumLoss -ServiceName fabric:/Myapplication/MyService -QuorumLossMode QuorumReplicas -QuorumLossDurationInSeconds 20

```

Selles näites me seadmine `QuorumLossMode` et `QuorumReplicas` näitamaks, soovime esile kvoorumi kaotsimineku võtmata alla kõik koopiad. Sellisel viisil toimingud on võimalik. Testimiseks stsenaarium, kus on kogu sektsiooni pole saadaval, saate seda parameetrit `AllReplicas`.

## <a name="next-steps"></a>Järgmised sammud

[Lisateavet testability toimingud](service-fabric-testability-actions.md)

[Lisateavet testability stsenaariumid](service-fabric-testability-scenarios.md)
