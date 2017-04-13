<properties
   pageTitle="Usaldusväärne saidikogumid | Microsoft Azure'i"
   description="Teenuse struktuuri stateful teenuseid pakkuda usaldusväärne saidikogumid, mis võimaldavad teil kirjutamine väga saadaval, scalable ja latentsusajaga pilv rakendusi."
   services="service-fabric"
   documentationCenter=".net"
   authors="mcoskun"
   manager="timlt"
   editor="masnider,vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="10/18/2016"
   ms.author="mcoskun"/>

# <a name="introduction-to-reliable-collections-in-azure-service-fabric-stateful-services"></a>Azure teenuse struktuuri stateful teenuste usaldusväärsete saidikogumid tutvustus

Usaldusväärne saidikogumid võimaldavad kirjutamine väga saadaval, scalable ja latentsusajaga pilv rakendusi nii, nagu kirjutades ühes arvutis rakendusi. **Microsoft.ServiceFabric.Data.Collections** nimeruumi klassid on kehtestada out-of-box saidikogumid, mis automaatselt oleku väga kättesaadavaks teha. Arendajatele vaja programmi ainult usaldusväärsest saidikogumi API-de ja andke usaldusväärne saidikogumite haldamine tiražeeritud ja kohaliku olek.

Võtme vahe usaldusväärne saidikogumite ja muude kõrge-saadavus hõlbustusvahendite (nt Redis, Azure'i tabeli teenuse ja Azure järjekorda teenus) on, et riik hoitakse kohapeal eksemplari samal ajal ka väga kättesaadav. See tähendab, et:

- Kõik loeb on kohalik, mille tulemuseks on madal latentsus ja suure läbilaskevõimega loeb.
- Kõik kirjutab tekkida minimaalne arv võrgu iOS-i, mille tulemuseks on madal latentsus ja suure läbilaskevõimega kirjutab.

![Kogumite, uuendus pilt.](media/service-fabric-reliable-services-reliable-collections/ReliableCollectionsEvolution.png)

Usaldusväärne saidikogumid saate kirjeldada kui füüsiline areng **System.Collections** tunnid: uus komplekt saidikogumid, mis on mõeldud pilveteenuste ja mitme arvuti rakenduste ilma keerukuse arendaja. Sellisena, usaldusväärseid saidikogumid on:

- Kopeeritud: Muutuste on kopeeritud jaoks kõrge kättesaadavus.
- Püsis: Andmete püsis kettale kestvus vastu suuremahuliste katkestuste (nt andmekeskuse power elektrikatkestus) jaoks.
- Asünkroonne: API-d on asünkroonne tagamaks, et teemad on blokeeritud kui tekiks IO.
- Selgituseks: API-de kasutada tehingud võtmiseks nii, et saate hallata mitut usaldusväärne saidikogumid teenuse raames hõlpsalt.

Usaldusväärne saidikogumid pakuvad tugev järjepidevuse tagab välja kasti parema loetavuse arutluskäik rakenduse oleku kohta.
Tugev järjepidevuse on saavutada tagades tehingu võtab valmis alles pärast kogu tehing on sisse logitud enamik otsustusvõimeline koopiate, sh esmane.
Nõrgem järjepidevuse saavutamiseks rakendusi saate kinnituse tagasi kliendi/taotleja enne asünkroonne kinnitamine annab.

Usaldusväärne saidikogumid API-d on areng samaaegseid kogumite API-de (leitud **System.Collections.Concurrent** nimeruumi).

- Asünkroonne: Tagastab tööülesande, kuna erinevalt samaaegseid saidikogumid, toimingute kopeeritud ja säilis.
- Pole teada parameetrid: kasutab `ConditionalValue<T>` kahendmuutuja ja parameetreid, mitte väärtuse tagastamiseks. `ConditionalValue<T>`on nagu `Nullable<T>` , kuid ei nõua T olema kirjel on.
- Tehingud: Kasutab tehingu objekti lubada kasutajal toimingu mitme usaldusväärse saidikogumid jaotises toimingud.

Täna, **Microsoft.ServiceFabric.Data.Collections** sisaldab kahte saidikogumid.

- [Usaldusväärne sõnastiku](https://msdn.microsoft.com/library/azure/dn971511.aspx): tähistab /-väärtuse paarideks tiražeeritud, selgituseks ja asünkroonne kogum. Sarnane **ConcurrentDictionary**nii võti ja väärtuse võib olla mis tahes tüüpi.
- [Usaldusväärne järjekorda](https://msdn.microsoft.com/library/azure/dn971527.aspx): tähistab tiražeeritud, selgituseks ja asünkroonne range esimesena sisse, lihtjärjekorra (FIFO) järjekorda. Sarnane **ConcurrentQueue**väärtus võib olla mis tahes tüüpi.

## <a name="isolation-levels"></a>Eraldamise tasemed
Eraldamise tasemeks omakorda määratleb, millises, millele tehing peab olema eraldatud muude toimingute tehtud muudatused.
On kaks eraldamise taset, mida toetatakse usaldusväärne saidikogumid.

- **Korratavad Read**: saate määrata, et laused ei saa lugeda andmeid, mis on muudetud, kuid ei ole veel sooritatud muude toimingute ja et muid tehinguid saate muuta andmeid, mis on praeguse tehing loetud kuni praeguse kande lõpetab. Lisateavet leiate teemast [https://msdn.microsoft.com/library/ms173763.aspx](https://msdn.microsoft.com/library/ms173763.aspx).
- **Hetktõmmis**: määrab, et andmeid lugeda, mis tahes toimingu lause on ülekandega ühtsete versiooni andmed, et tehingu alguses on olemas.
Tehingu tuvastab ainult andmeid muudatusi, mis on kinnitatud enne tehingu.
Tehtud muude toimingute pärast praeguse kande andmemuudatuste ei ole nähtav laused praeguse kande käivitamist.
On nagu laused toimingu saada kinnitatud andmete hetktõmmist, nii nagu see oli tehingu alguses.
Hetktõmmiseid vastavad üle usaldusväärne saidikogumid.
Lisateavet leiate teemast [https://msdn.microsoft.com/library/ms173763.aspx](https://msdn.microsoft.com/library/ms173763.aspx).

Usaldusväärne saidikogumid automaatselt valida eraldamise tasemeks kasutamiseks antud Lugemistoiming sõltuvalt toiming ja roll koopia kande loomise ajal.
Järgmine on tabel, mis kujutab eraldamise taseme vaikesätete usaldusväärne sõnastik ja järjekorda.

| Toiming \ roll      | Esmane          | Teisese        |
| --------------------- | :--------------- | :--------------- |
| Ühe üksuse lugemine    | Korratavad Read  | Hetktõmmis         |
| Loendamine \ loendamine   | Hetktõmmis         | Hetktõmmis         |

>[AZURE.NOTE] Levinud näited ühe üksuse toimingud on `IReliableDictionary.TryGetValueAsync`, `IReliableQueue.TryPeekAsync`.

Nii usaldusväärne sõnastiku usaldusväärne kuhjuda toetavad lugege oma kirjutab.
Teisisõnu, mis tahes kirjutamine kande sees saab järgmised read, mis kuulub sama tehingu nähtavaks.

## <a name="locking"></a>Lukustamine
Usaldusväärne saidikogumid, kõik tehingud on kaks järk: tehingu ei vabasta lukud omandanud kuni tehingu lõpeb mõne katkestamist või mõne kinnitamine.

Usaldusväärne sõnastiku kasutab rea taseme kõik ühe üksuse toimingute lukustamine.
Usaldusväärne järjekorda tehinguid kokkulangevus range selgituseks FIFO atribuudi välja.
Usaldusväärne järjekorda kasutab toiming taseme lukud lubamisel ühe tehing `TryPeekAsync` ja/või `TryDequeueAsync` ja ühe tehing `EnqueueAsync` korraga.
Pange tähele, et säilitada FIFO, kui mõnda `TryPeekAsync` või `TryDequeueAsync` kunagi märgib, et usaldusväärne järjekord on tühi, nad saavad ka lukustada `EnqueueAsync`.

Kirjutage toiminguid alati eksklusiivse lukud.
Toimingud, blokeerimine sõltub paarist tegureid.
Lugege toiming, kes on valmis hetktõmmise eraldamise abil on tasuta lukustamine.
Mis tahes korratavad Read – vaikimisi toiming ühiskasutuses lukud.
Lugege toimingu, mis toetab korratavad Read, paluda kasutaja on värskendus Lock asemel ühiskasutuses lock.
Mõne värskenduse lock on on asümmeetriline Lukusta vältimiseks levinum tupikusse, mis ilmneb siis, kui mitut lukustada ressursid võimalike värskenduste hiljem kasutada.

Lukusta ühilduvus maatriksi leiate allpool:

| Taotleda \ antud | Ükski         | Ühiskasutusse antud       | Värskendamine      | Välja arvatud    |
| ----------------- | :----------- | :----------- | :---------- | :----------- |
| Ühiskasutusse antud            | Konflikti pole  | Konflikti pole  | Konflikti    | Konflikti     |
| Värskendamine            | Konflikti pole  | Konflikti pole  | Konflikti    | Konflikti     |
| Välja arvatud         | Konflikti pole  | Konflikti     | Konflikti    | Konflikti     |

Pange tähele, et ajalõpp argument on usaldusväärne saidikogumid API-d kasutatakse tupikusse avastamine.
Näiteks kahe tehingu (T1 ja T2) üritavad lugeda ja värskendada K1.
On võimalik neid tupik, kuna mõlemad lõpuks võttes ühiskasutuses lock.
Sel juhul ühe või mõlema toimingud on ajalõpuni.

Tähele, et ülaltoodud tupik stsenaarium on suurepärane näide sellest, kuidas mõni värskendus lukustamine saate vältida tupikuid.

## <a name="persistence-model"></a>Püsimine mudel
Usaldusväärne olekus halduri ja usaldusväärne saidikogumid järgige püsimine mudelit, mida nimetatakse Log ja kontrollpunkt.
See on mudel, kus iga riigi muutmine on sisse logitud kettale ja rakendatakse ainult mällu.
Täielik riik ise on säilis ainult aeg-ajalt (alias Kontrollpunkt).
Kasu, on see, et deltad on muutunud järjestikku lisamine või ainult kirjutab kettal paranenud jõudlus.

Paremini mõistmiseks Log ja kontrollpunkt mudeli, esmalt vaatame lõpmatu ketta stsenaariumi.
Usaldusväärne olekus haldur logib iga toimingu enne on kopeeritud.
See võimaldab usaldusväärne saidikogumi rakendamiseks ainult toimingu mälu.
Kuna logide säilis isegi siis, kui koopia nurjub ja tuleb taaskäivitada, on usaldusväärne olekus halduri piisavalt teavet oma logisid kordus kõik need toimingud on kadunud koopia.
Kuna ketas on lõpmatu, Logi kirjed kunagi vaja eemaldada ning usaldusväärne saidikogumi haldamiseks-Mälu olek.

Nüüd vaatame piiratud vaba stsenaariumi.
Nagu Logi kirjed koguda, on usaldusväärne olekus haldur otsa kettaruumi.
Enne, mis juhtub, usaldusväärne olekus haldur vajab kärpida selle log ruumi uuem kirjete jaoks.
Seda taotleda usaldusväärne saidikogumid kontrollpunkt abil oma-mälu olek kettale.
On usaldusväärne saidikogumid ülesanne kestab kuni selle punkti seisu.
Kui usaldusväärne saidikogumid nende postkastid, saate usaldusväärne olekus halduri Kärbi Logi kettal ruumi vabastamiseks.
Sellisel viisil, kui koopia tuleb taaskäivitada, usaldusväärseid saidikogumite taastamine oma checkpointed oleku ja usaldusväärne olekus Manager taastamine ja taasesitamine kõik pärast selle kontrollpunkt olekus muudatused.

>[AZURE.NOTE] Teine väärtus lisatakse checkpointing on, et see parandab taastamine levinud juhtudel.
Selle põhjuseks on postkastid sisaldavad ainult uusimaid versioone.

## <a name="recommendations"></a>Soovitused

- Muutke tagastatud toimingud kohandatud tüüpi objekti (nt `TryPeekAsync` või `TryGetValueAsync`). Usaldusväärne saidikogumid, nagu samaaegseid saidikogumid, tagastatakse objektide ja mitte paljundada.
- Kas sügav Kopeeri tagastatud objekt kohandatud tüüpi enne muutes. Kuna structs ja sisseehitatud tüübid ei liigu väärtuse alusel, pole vaja teha neid sügav koopia.
- Ärge kasutage `TimeSpan.MaxValue` ajalõpp jaoks. Ajalõpp tuleks kasutada tupikuid tuvastamiseks.
- Ärge kasutage tehingu pärast seda, kui see on kinnitatud, katkestatud või müüdud.
- Ärge kasutage numeratsioon see loodi tehingu väljapoole.
- Luua teise tehingu jooksul `using` lause kuna see võib põhjustada tupikuid.
- Veenduge, et teie `IComparable<TKey>` on õige. Süsteemi võtab sõltuvus selle ühendamise postkastid.
- Kasutage värskenduse Lukusta üksuse kavas abil lugemise ajal seda, et vältida teatud liiki tupikuid.
- Kaaluge varundamise ja taastamise funktsioonid on katastroofiabi.
- Vältige segamine ühe üksuse toimingud ja mitme üksuse toimingud (nt `GetCountAsync`, `CreateEnumerableAsync`) sama tehingu erinevate eraldamise tõttu.

Siin on mõned asjad, mida meeles pidada:

- Vaikimisi ajalõpu on usaldusväärne saidikogumi API 4 sekundit. Enamik kasutajaid see peaks tühistada.
- Vaikimisi loobumise luba on `CancellationToken.None` kõigi usaldusväärne saidikogumid API-de sisse.
- Usaldusväärne sõnastiku võtme parameetrit (*TKey*) rakendama õigesti `GetHashCode()` ja `Equals()`. Klahvid peab olema püsiv.
- Kõrge-saadavus saavutamiseks usaldusväärne saidikogumid iga teenuse peaks olema vähemalt target ja määrata suuruse 3 minimaalne koopia.
- Read toimingud teisese lugeda versioonid, mis pole kinnitatud kvoorumi.
See tähendab, et andmeid, mis on kirjutuskaitstud kaudu ühe teisese võib olla false jõudnud versioon.
Muidugi on alati ühed primaarne loeb: saate kunagi olla false jõudnud.

## <a name="next-steps"></a>Järgmised sammud

- [Usaldusväärne teenuste Lühijuhend](service-fabric-reliable-services-quick-start.md)
- [Usaldusväärne saidikogumid töötamine](service-fabric-work-with-reliable-collections.md)
- [Usaldusväärne teenuste teatised](service-fabric-reliable-services-notifications.md)
- [Usaldusväärne teenuste varundus ja taaste (avariitaastet)](service-fabric-reliable-services-backup-restore.md)
- [Usaldusväärne olekus halduri konfigureerimine](service-fabric-reliable-services-configuration.md)
- [Teenuse struktuuri Web API teenuste töötamise alustamine](service-fabric-reliable-services-communication-webapi.md)
- [Täpsemad programmeerimisega mudeli usaldusväärne teenuste kasutamist](service-fabric-reliable-services-advanced-usage.md)
- [Tootearendusmaterjal, usaldusväärseid saidikogumid](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
