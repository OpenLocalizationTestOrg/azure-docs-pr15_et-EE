<properties 
    pageTitle="Teenuse siini asünkroonne sõnumside | Microsoft Azure'i"
    description="Kirjeldus teenuse siini asünkroonne sõnumside."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" /> 
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/04/2016"
    ms.author="sethm" />

# <a name="asynchronous-messaging-patterns-and-high-availability"></a>Asünkroonne sõnumside mustrite ja kõrge-saadavus

Asünkroonne sõnumside saab rakendada mitmel erineval viisil. Azure'i teenus siini toetab järjekorrad, teemade ja tellimused, asynchrony salvestamine ja edasisaatmine süsteemi kaudu. Tavaline (sünkroonse) kasutusel järjekorrad ja teemade sõnumeid saata ja sõnumeid saada järjekorrad ja tellimused. Rakenduste kirjutamise sõltuvad need üksused on alati saadaval. Üksuse seisundi muutumise tõttu asjaolud, peate võimalus anda vähendatud võimalus üksus, mis suudab enamik vajadustele.

Rakenduste tavaliselt abil asünkroonne sõnumside mustrite arv side stsenaariumid. Saate luua rakendusi, kus kliendid saavad saata sõnumeid teenuseid, isegi siis, kui teenus ei tööta. Rakenduste puruneb kirjavahetuse järjekorda aitab kogemus taseme laadi esitada koht puhvri suhtlus. Lisaks saate lihtsa, kuid tegelik koormus koormusetasakaalustusteenuse mitme arvutites sõnumite saatmiseks.

Kaaluge nende üksuse kättesaadavus säilitamiseks on mitmel erineval viisil, kus saate need üksused kuvatakse püsival sõnumsidesüsteemi jaoks saadaval. Üldiselt näeme üksus, muutuvad kättesaamatuks rakendustele me kirjutada järgmist võimalust:

- Ei saa sõnumeid saata.

- Ei saa sõnumeid.

- Ei saa hallata üksuste (loomine, tuua, värskendamine või üksuste kustutamine).

- Ei saa ühendust teenusega.

Iga tõrkeid, erinevate tõrgete tüübid olemas mis võimaldavad rakenduse jätkamiseks mõned tasandi vähendatud võimalus tööd teha. Näiteks süsteemi, kus saate sõnumeid saata, kuid ei saa neid saate endiselt tellimused klientidele, kuid ei saa töödelda tellimusi. Selles teemas käsitletakse võimalikud probleemid, mis võivad tekkida, ja kuidas neid probleeme on leevendada. Teenuse siini on kasutusele arvu kergendamise, mille peate ja selles teemas käsitletakse ka nende osalemine kergendamise kasutamist reguleerivate reegleid.

## <a name="reliability-in-service-bus"></a>Teenuse siini usaldusväärsuse

On mitu võimalust sõnumi ja üksuse küsimuste ja nende kergendamise kasutamise korral tuleb arvestada. Mõistmaks suuniste, tuleb esmalt mõista, mis võib nurjuda teenuse siini sisse. Azure'i süsteemide tõttu, kõiki neid küsimusi tavaliselt lühiajaline. Kõrge, kuvatakse erinevad põhjused saada järgmiselt:

-   Pidurdamise alates millest sõltub teenuse siini välise süsteemiga. Pidurdamise ilmneb kasutusviisid salvestus- ja Arvuta ressurssidega kaudu.

-   Millest sõltub teenuse siini süsteemi probleemi. Näiteks saate salvestusruumi antud osa tekkida probleeme.

-   Tõrge teenuse siini ühe alamsüsteemi. Sellisel juhul Arvuta sõlme vastuolu sattuda ja tuleb taaskäivitada, põhjustab kõik üksused, mis on selle eesmärk on laadimine sõlmi saldo. See omakorda võib põhjustada lühikest aeglane sõnumi töötlemine.

-   Tõrge teenuse siini sees on Azure andmekeskuse. See on "katastroofiline tõrge" mil süsteemi on kättesaamatu või mitme minuti mõne tunni pärast.

> [AZURE.NOTE] Termini **salvestusruumi** saate tähendab Azure Storage nii SQL Azure'i.

Teenuse siini sisaldab mitmeid kergendamise järgmiste probleemide jaoks. Järgmistes lõikudes käsitletakse iga probleemi ja nende vastavate kergendamise.

### <a name="throttling"></a>Ahendamine

Siin teenus, mis võimaldab ahendamine koostöö sõnumi määr haldus. Iga üksiku teenuse siini sõlm maja palju üksusi. Kõigi üksuste teeb nõuab süsteemi CPU, mälu, salvestusruumi ja muude omadustega. Kui mõni järgmised omadustega kasutamise, mis ületab määratletud lävede, saate teenuse siini keelamiseks antud taotluse. Funktsiooni helistaja saab on [ServerBusyException][] ja uued katsed 10 sekundi pärast.

Kui ka vähendamiseks koodi lugeda viga ja peatada sõnumi mis tahes korduskatsed vähemalt 10 sekundit. Kuna kuvatakse tõrge võib ilmneda üle tekstilõigule kliendi rakendus, eeldatakse, et iga tekstilõigu sõltumatult aktiveeritakse uuesti loogika. Koodi saate vähendada tõenäosus on rakendus, võimaldades eraldamine järjekorda või teema.

### <a name="issue-for-an-azure-dependency"></a>Probleemi on Azure sõltuvus

Muude komponentide Azure'is aeg-ajalt võib olla seotud probleeme. Näiteks süsteemi, mis kasutab teenuse siini uuendamisel süsteemi ajutiselt võib tekkida vähendatud võimalusi. Seda tüüpi probleemide lahendamiseks teenuse siini regulaarselt uurib ja rakendab kergendamise. Kuvatakse nende kergendamise küljel efektid. Näiteks siirdamiseks probleemid salvestusruumi käsitlema teenuse siini rakendab süsteemi, mis võimaldab sõnumit saata tegevuste tööd pidevalt. Selle vähendamiseks laadist saadetud sõnumile võib kuluda kuni 15 minutit probleemse järjekorda või tellimuse kuvada ja võta vastu tööks valmis. Üldiselt ei kogenud enamiku üksuste probleemi. Siiski Azure'is teenuse siini esitatud üksuste arv, selle vähendamiseks on mõnikord vaja väikese teenuse siini kliendid.

### <a name="service-bus-failure-on-a-single-subsystem"></a>Ühe alamsüsteemi teenuse siini tõrge

Mistahes rakendusega juhtudel võib põhjustada teenuse siini saada vastuolulised kogemused sisemise osa. Kui teenus siini tuvastab see, kogub andmeid rakendusest abi diagnoosimise, kuhu on kadunud. Kui andmed on kogunud, rakenduse taaskäivitamist püüdes naasmiseks ühtsete olekus. Selle protsessi juhtub üsna kiiresti ja tulemuste kuvataks paar minutit, kuni jaoks saadaval olla, kuigi tüüpilised alla korda üksus on palju lühem.

Sellisel juhul klientrakendus loob [System.TimeoutException][] või [MessagingException][] erand. Teenuse siini sisaldab vähendamiseks, probleemile automatiseeritud kliendi proovi uuesti loogikat vorm. Kui uuesti perioodi on ära ja sõnum on esitatud, saate uurida muude funktsioonide nagu [andmepunktipaaride nimeruumid][]abil. Andmepunktipaaride nimeruumid on muud piirangud, mida selles artiklis käsitletakse.

### <a name="failure-of-service-bus-within-an-azure-datacenter"></a>Tõrge teenuse siini sees on Azure andmekeskusega

Tõenäoliselt on Azure andmekeskuses tõrke põhjuseks on nurjunud versioonitäienduse kasutuselevõtu teenuse siini või sõltuvad süsteem. Nagu platvormi on laagerdunud, seda tüüpi jätmine tõenäosus on kahanenud. Andmekeskuse tõrge võib olla ka põhjust, sisaldavad järgmist:

-   Elektrisüsteemi katkestuste (toite ja loomisel power kaovad).
-   Ühenduvus (teie klientide ja Azure paus internet).

Mõlemal juhul loodusõnnetuse või inimese probleemi põhjustasid. Selle lahendamiseks ja veenduge, et saate saata sõnumeid, saate [andmepunktipaaride nimeruumid][] saadetaks teise asukohta samas esmane asukoht on tehtud terve uuesti lubada. Lisateabe saamiseks vt [head tavad isoleeriva rakenduste teenuse siini katkestuste ja katastroofide vastu][].

## <a name="paired-namespaces"></a>Andmepunktipaaride nimeruumid

Funktsiooni [andmepunktipaaride nimeruumid][] toetab stsenaariumid, kus teenuse siini üksus või juurutamise raames andmekeskuse kättesaamatu. Kuigi see sündmus harva, hajutatud süsteemide endiselt peab olema valmis hakkama kehvem juhul stsenaariumid. Tavaliselt juhtub see sündmus, sest mõned element, mis sõltub teenuse siini esineb lühiajaline probleem. Säilitada rakenduste saadavus on katkestuste, teenuse siini kasutajad saate kasutada kahte omaette nimeruumi, parim eraldi andmekeskuste, majutada oma sõnumside üksuste. Selles jaotises ülejäänud kasutab järgmist terminid.

-   Esmane nimeruum: nimeruumi, millega rakenduse suhtleb saatmise ja vastu võtta toimingud.

-   Teisene nimeruum: nimeruumi, mis toimib esmane nimeruumi varukoopia. Rakenduse loogika ei suhelda selle nimeruumi.

-   Tõrkesiirde intervall: aja kinnitamiseks tavaline tõrkeid enne rakenduse aktiveerib esmane nimeruumi teisene nimeruumi.

Paaris nimeruumid tugi *kättesaadavusteabe saatmine*. Saatke kättesaadavus säilitab võimalus saata sõnumeid. Saada-saadavus kasutama rakenduse peab vastama järgmistele nõuetele:

1.  Esmane nimeruumi ainult saadetud meilisõnumid.

2.  Sõnumite saatmiseks antud järjekorda või teema saabuvad vales järjestuses.

3.  Kui teie rakendus kasutab seansid, seansi jooksul sõnumid saabuvad vales järjestuses. See on tavaline funktsionaalsust seansid paus. See tähendab, et teie rakendus kasutab seansid loogiliselt rühma sõnumitele. Seansi olek on ainult esmane nimeruumi säilitada.

4.  Seansi jooksul sõnumid saabuvad vales järjestuses. See on tavaline funktsionaalsust seansid paus. See tähendab, et teie rakendus kasutab seansid loogiliselt rühma sõnumitele.

5.  Seansi olek on ainult esmane nimeruumi säilitada.

6.  Esmane järjekorda saate veebis ja alustada vastu võtmist sõnumite enne teisene kuhjuda pakub kõigi sõnumite esmase järjekorda.

Järgmistes lõikudes käsitletakse selle API-d, kuidas funktsiooni API-de rakendatakse ja proovi kood näitab, et funktsiooni. Pange tähele, et selle funktsiooniga seotud arvelduse mõju.

### <a name="the-messagingfactorypairnamespaceasync-api"></a>MessagingFactory.PairNamespaceAsync API

Funktsiooni andmepunktipaaride nimeruumid sisaldab [Microsoft.ServiceBus.Messaging.MessagingFactory][] klassi [PairNamespaceAsync][] meetodit.

```
public Task PairNamespaceAsync(PairedNamespaceOptions options);
```

Kui tööülesande lõpule jõudnud, nimeruum sidumine on ka lõpetatud ja valmis jaoks mis tahes [MessageReceiver][], [QueueClient][], töötlemiseks või [TopicClient][] loodud [MessagingFactory][] eksemplari. [Microsoft.ServiceBus.Messaging.PairedNamespaceOptions][] on base klassi erinevat tüüpi sidumine, mille jaoks on saadaval kõige uuemas [MessagingFactory][] objekti. Praegu on ainus tuletatud klassi ühe nimega [SendAvailabilityPairedNamespaceOptions][], mis rakendab saatmine kättesaadavus nõuetele. [SendAvailabilityPairedNamespaceOptions][] on seatud ehitajatel, mis teineteisele koostamine. Vaadates ehitaja enamiku parameetrite abil, saavad aru muude ehitajatel toimimist.

```
public SendAvailabilityPairedNamespaceOptions(
    NamespaceManager secondaryNamespaceManager,
    MessagingFactory messagingFactory,
    int backlogQueueCount,
    TimeSpan failoverInterval,
    bool enableSyphon)
```

Järgmiste parameetrite tähendavad järgmist:

-   *secondaryNamespaceManager*: teisene nimeruumi, mille [PairNamespaceAsync][] meetodi abil saate häälestada teisene nimeruumi käivitub [NamespaceManager][] eksemplar. Nimeruumi halduri kasutatakse saada järjekorda nimeruumi loend ja veenduge, et nõutav mahajäämus järjekorrad olemas. Kui need järjekorrad pole olemas, on loonud. [NamespaceManager][] nõuab võimalus luua märgiks taotluste **haldamine** .

-   *messagingFactory*: teisene nimeruumi [MessagingFactory][] eksemplari. [MessagingFactory][] objekti saab saata ja, kui [EnableSyphon][] väärtuseks on seatud väärtusele **tõene**, mahajäämus järjekorrad sõnumeid saada.

-   *backlogQueueCount*: arvu mahajäämus järjekorda loomiseks. Väärtus peab olema vähemalt 1. Sõnumi saatmiseks mahajäämus on valitud üks nende järjekorda juhusliku ID-ga. Kui seate väärtuse 1, siis saab kunagi ainult üks järjekorda. Kui see juhtub ja ühe mahajäämus järjekord genereeritud tõrkeid, klient on ei saa proovida erinevaid mahajäämus järjekord ja ei pruugi teie sõnumi saatmiseks. Soovitame seda väärtust mõnda suuremat väärtust ja vaikimisi väärtuse 10. Saate muuta seda suurem või väiksem väärtus, sõltuvalt sellest, kui palju andmeid rakenduse saadab päevas. Iga mahajäämus järjekord võib olla kuni 5 GB sõnumeid.

-   *failoverInterval*: aja jooksul saate aktsepteerib tõrkeid, esmane nimeruumi enne mis tahes ühe üksuse üleminekut teisene nimeruumi. Failovers ilmneda üksuse eraldi olemit alusel. Üksuste ühe nimeruumi elavad sageli erinevaid sõlmi teenuse siini sees. Tõrke üks üksus ei tähenda teise tõrke. Saate seada selle väärtuse [System.TimeSpan.Zero][] abil Tõrkesiirde teisese kohe pärast oma esimese, -mööduv tõrge. Tõrkeid, mis käivitab Tõrkesiirde timer on mis tahes [MessagingException][] , kus [IsTransient][] atribuut on false, või on [System.TimeoutException][]. Muud erandid, nt [UnauthorizedAccessException][] ei põhjusta Tõrkesiirde, kuna andmeloendite et kliendi on valesti konfigureeritud. Mõne [ServerBusyException][] ei põhjusta Tõrkesiirde kuna õige muster on ootama 10 sekundit, saatke sõnum uuesti.

-   *enableSyphon*: näitab, et kindla sidumine peaks syphon ka sõnumid teisene nimeruumi tagasi esmane nimeruumi. Üldiselt rakendusi, mis saadavad sõnumeid tuleks Määrake selle väärtuseks **false**; rakendusi, mis sõnumeid, mis peaks Määrake selle väärtuseks **True**. Selle põhjuseks on sageli, on vähem sõnumi vastuvõtjad kui sõnumite saatjate. Vastuvõtjad arvule, saate valida ühe rakenduse eksemplari, toime syphon ülesandeid. Paljude vastuvõtjad abil mõjutab arvelduse iga mahajäämus järjekorras.

Esmane [MessagingFactory][] eksemplari, teisene [MessagingFactory][] eksemplari, teisene [NamespaceManager][] eksemplar ja [SendAvailabilityPairedNamespaceOptions][] eksemplari loomine koodi kasutamiseks. Kõne võib olla lihtsad, sisaldades järgmist:

```
SendAvailabilityPairedNamespaceOptions sendAvailabilityOptions = new SendAvailabilityPairedNamespaceOptions(secondaryNamespaceManager, secondary);
primary.PairNamespaceAsync(sendAvailabilityOptions).Wait();
```

Kui tagastatud [PairNamespaceAsync][] meetodit tööülesande lõpule jõudnud, on kõik häälestatud ja kasutamiseks valmis. Enne tööülesande tagastatakse, mida ei on lõpule viia kõik vajalikud sidumine töö paremale tausta tööd. Seetõttu ei tohi käivitada enne tööülesande tagastab sõnumite saatmiseks. Mis tahes tõrkeid ilmnes, näiteks halb identimisteabe või loomine mahajäämus järjekordades, visatakse erandite kui tööülesanne on lõpule jõudnud. Pärast seda, kui tööülesanne, veenduge, et järjekorrad olid leitud või loodud uurides atribuudi [BacklogQueueCount][] oma [SendAvailabilityPairedNamespaceOptions][] eksemplari. Eelmise koodi see toiming kuvatakse järgmiselt:

```
if (sendAvailabilityOptions.BacklogQueueCount < 1)
{
    // Handle case where no queues were created.
}
```

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete õppinud põhitõdesid asünkroonne sõnumside teenuse siini, lugege lisateavet [andmepunktipaaride nimeruumid][].

  [ServerBusyException]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.serverbusyexception.aspx
  [System.TimeoutException]: https://msdn.microsoft.com/library/system.timeoutexception.aspx
  [MessagingException]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingexception.aspx
  [Rakenduste teenuse siini katkestuste eest katastroofide soojustamine head tavad]: service-bus-outages-disasters.md
  [Microsoft.ServiceBus.Messaging.MessagingFactory]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx
  [MessageReceiver]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx
  [QueueClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx
  [TopicClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx
  [Microsoft.ServiceBus.Messaging.PairedNamespaceOptions]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.pairednamespaceoptions.aspx
  [MessagingFactory]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx
  [SendAvailabilityPairedNamespaceOptions]:https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions.aspx
  [NamespaceManager]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx
  [PairNamespaceAsync]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.pairnamespaceasync.aspx
  [EnableSyphon]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions.enablesyphon.aspx
  [System.TimeSpan.Zero]: https://msdn.microsoft.com/library/azure/system.timespan.zero.aspx
  [IsTransient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingexception.istransient.aspx
  [UnauthorizedAccessException]: https://msdn.microsoft.com/library/azure/system.unauthorizedaccessexception.aspx
  [BacklogQueueCount]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions.backlogqueuecount.aspx
  [andmepunktipaaride nimeruumid]: service-bus-paired-namespaces.md