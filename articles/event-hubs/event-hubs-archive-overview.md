<properties
    pageTitle="Azure'i sündmuse jaoturi arhiivi | Microsoft Azure'i"
    description="Azure'i sündmuse jaoturi arhiivimisfunktsiooni ülevaade."
    services="event-hubs"
    documentationCenter=""
    authors="djrosanova"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="darosa;sethm"/>

# <a name="azure-event-hubs-archive"></a>Azure'i sündmuse jaoturi arhiivimine

Azure'i sündmuse jaoturi arhiivi võimaldab teil automaatselt esitamisel voogesituse andmete oma sündmuse jaoturi bloobimälu salvestusruumi konto teie valitud mis on aja määrata või suuruse intervall enda valitud Meilikausta. Arhiivi häälestada on kiire, on halduskulud käivitada ja seda skaala automaatselt teie sündmuse jaoturi [läbilaskevõime üksused](event-hubs-overview.md#capacity-and-security). Sündmuse jaoturi arhiivi on kõige lihtsam viis videote voogesitamise andmete laadimiseks Azure ja võimaldab teil keskenduda töötlemise asemel andmehõive.

Azure'i sündmuse jaoturi arhiivi võimaldab töötlemine reaalajas ja paketi torujuhtmete sama voogu. See võimaldab teil luua lahendusi, mis võivad kasvada aja jooksul oma vajadustele. Kas olete hoone paketi-põhiste süsteemide täna tulevaste reaalajas töötlemine pilguga või soovite lisada mõne tõhusa külma tee olemasoleva reaalajas lahenduse, teeb sündmuse jaoturi arhiivi töötamise streaming andmed lihtsamaks.

## <a name="how-event-hubs-archive-works"></a>Sündmuse jaoturi arhiivi tööpõhimõtted

Sündmuse jaoturi on aeg-säilituspoliitika püsival puhvri telemeetria sissepääsu, sarnaselt jaotatud Logi jaoks. Sündmuse jaoturi skaala on [sektsioonitud tarbija mudel](event-hubs-overview.md#partition-key). Iga sektsiooni on mõni sõltumatu lõigust andmete ja tarbitud sõltumatult. Aja jooksul andmed aegade välja konfigureeritav säilitusperiood põhjal. Selle tulemusena antud sündmuse jaoturi kunagi saab "liiga täis."

Sündmuse jaoturi arhiivi võimaldab teil määrata oma Azure'i bloobimälu konto ja ümbris, mida kasutatakse arhiivitud andmete talletamiseks. Selle konto võib olla sama piirkonna oma sündmuse jaoturi või teistes, sündmuse jaoturi arhiivimisfunktsiooni paindlikkust lisamine.

Arhiivitud andmed on kirjutatud [Apache Avro][] -vormingus; kompaktne, Kiire, kahendarvu vormingut, mida pakub andmestruktuurid rikkaliku teksti sees skeemi. Seda vormingut kasutatakse ulatuslikult Hadoopi ökosüsteemis, samuti voo Analytics ja Azure andmete Factory. Lisateavet Avro töötamise kohta on selle artikli saadaval.

### <a name="archive-windowing"></a>Arhiivi akendesüsteemide

Sündmuse jaoturi arhiivi võimaldab teil määrata arhiivimine akna häälestamine. See aken on miinimummaht ja konfigureerimist ja "esimese võitu poliitika," tähendab, et ilmnes esimese päästiku võib põhjustada toimingu arhiivi. Kui teil on 15-minutiline 100 MB arhiivi aknas ja saada 1 MB/s, käivitab akna suuruse enne selle ajaakna. Iga sektsiooni Arhiiv sõltumatult ja kirjutab lõplikus Blokeeri bloobimälu arhiivi, ajal ajaks, millal ilmnes arhiivi intervall nimega. Nimereeglistik on järgmine:

```
<Namespace>/<EventHub>/<Partition>/<YYYY>/<MM>/<DD>/<HH>/<mm>/<ss>
```

### <a name="scaling-to-throughput-units"></a>Skaleerimist läbilaskevõime kestusühikud

Sündmuse jaoturi liikluse kontrollib [läbilaskevõime üksused](event-hubs-overview.md#capacity-and-security). Ühe läbilaskevõime ühiku võimaldab 1 MB/s või 1000 sündmuste sekundis sissepääsu ja kaks korda selle sealt summa. Standardse sündmuse jaoturi saab konfigureerida 1-20 läbilaskevõime üksused ja rohkem saab osta kvoodi suurendamiseks [toe taotlemine][]kaudu. Üle ostetud läbilaskevõime üksused on rakendus. Sündmuse jaoturi arhiivi kopeerib andmed otse sisemist sündmuse jaoturi ruumi, mööda läbilaskevõime ühiku sealt kvootide ja salvestamine oma sealt muu töötlemine lugejatele nagu voo Analytics või säde.

Kui konto on konfigureeritud, sündmuse jaoturi arhiivi käivitatakse automaatselt, kui saadate oma esimese sündmuse. See jätkub töötab igal ajal. Oleks lihtsam leida, et protsess töötab teie järgmise etapi töötlemiseks, kirjutab sündmuse jaoturi tühje faile, kui andmeid pole. See pakub prognoositavad sagedus ja tähis, mis võib teie paketi protsessorite kanali.

## <a name="setting-up-event-hubs-archive"></a>Kuidas häälestada sündmuse jaoturi arhiivimine

Sündmuse jaoturi arhiivi saab konfigureerida kaudu portaali või Azure ressursihaldur sündmuse jaoturi loomise ajal. Lubate, klõpsates nuppu **klõpsake** lihtsalt arhiivi. Saate konfigureerida salvestusruumi konto ja container **Container** jaotises tera, klõpsates. Kuna sündmuse jaoturi arhiivi kasutab salvestusruumi teenusest autentimine, pole vaja määrata salvestusruumi ühendusstringi. Ressursi valija valib ressursi URI konto salvestusruumi automaatselt. Kui kasutate Azure ressursihaldur, peate määrama selle URI otseselt stringina.

Ajaakna vaikeväärtus on viis minutit. Vähim väärtus on 1, kuni 15. Akna **suurust** on 10 – 500 MB hulk.

![][1]

## <a name="adding-archive-to-an-existing-event-hub"></a>Olemasolevate sündmuste jaoturiga arhiivi lisamine

Olemasolevate sündmuste jaoturi, mis on mõni sündmus jaoturi nimeruum kohta saab konfigureerida arhiivi. See funktsioon pole saadaval vanemat tüüpi sõnumid või segatud nimeruumid. Lubamise kohta mõne olemasoleva sündmuse jaoturi arhiivi või arhiivi sätete muutmiseks klõpsake oma nimeruum **Essentialsi** tera laadimine ja seejärel klõpsake sündmuse jaoturi, mille soovite lubada või muutke sätet arhiivi. Lõpuks klõpsake **Atribuudid** jaotises avatud tera, nagu on näidatud järgmisel joonisel.

![][2]

Samuti saate konfigureerida sündmuse jaoturi arhiivi kaudu Azure'i ressursihaldur mallid. Lisateabe saamiseks lugege [artiklit](event-hubs-resource-manager-namespace-event-hub-enable-archive.md).

## <a name="exploring-the-archive-and-working-with-avro"></a>Uurimine arhiivi ja Avro töötamine

Kui konto on konfigureeritud, loob sündmuse jaoturi arhiivi failide Azure Storage konto ja container konfigureeritud ajaakna kohta. Saate vaadata nende failide [Azure'i salvestusruumi Exploreri][]nagu mis tahes tööriistas. Saate kohalikult töötama need failid alla laadida.

Sündmuse jaoturi arhiivi toodetud failid on järgmised Avro skeemi.

![][3]

Apache purk [Avro tööriistade][] abil on lihtne viis Avro failide analüüsimiseks. Pärast allalaadimist purgist, näete skeemi Avro kindlat tüüpi failiga, käivitage järgmine käsk:

```
java -jar avro-tools-1.8.1.jar getschema \<name of archive file\>
```

See käsk tagastab

```
{

    "type":"record",
    "name":"EventData",
    "namespace":"Microsoft.ServiceBus.Messaging",
    "fields":[
                 {"name":"SequenceNumber","type":"long"},
                 {"name":"Offset","type":"string"},
                 {"name":"EnqueuedTimeUtc","type":"string"},
                 {"name":"SystemProperties","type":{"type":"map","values":["long","double","string","bytes"]}},
                 {"name":"Properties","type":{"type":"map","values":["long","double","string","bytes"]}},
                 {"name":"Body","type":["null","bytes"]}
             ]
}
```

Samuti saate Avro tööriistad JSON-vormingus faili teisendamine ja muu töötlemine tegemiseks.

Teha täpsemaid töötlemine, laadige alla ja installige Avro platvormi valikust. Kirjutamise ajal on rakendusi saadaval, C, C, C++\#, Java, NodeJS, Perl, PHP, Python ja Ruby.

Apache Avro on lõpule viidud alustamine juhendid [Java][] ja [Python][]. Leiate artiklist [Alustamine sündmuse jaoturi arhiivi](event-hubs-archive-python.md) .

## <a name="how-event-hubs-archive-is-charged"></a>Kuidas sündmuse jaoturi arhiivi ei lisandu

Sündmuse jaoturi arhiivi on Mahupõhised sarnaselt läbilaskevõime ühikut, nagu lisatasu kord tunnis. Kulu on otse läbilaskevõime ostetud ühikute summasid nimeruumi arv. Nagu läbilaskevõime üksused on kasvanud ja vähendada, sündmuse jaoturi arhiivi suurendab ja esitada kattuvad jõudlus väheneb. Meetrit tehakse paralleelselt. Sündmuse jaoturi Arhiiv tasu on $0,10 tunnis läbilaskevõime ühiku, pakutakse 50% allahindlust eelvaade perioodi kohta.

Sündmuse jaoturi Arhiiv on tõeliselt on kõige lihtsam viis Azure'i andmeid tuua. Azure'i andmed Lake, Azure'i andmed Factory ja Azure Hdinsighti abil saate teha Pakktöötlus ja muude analytics enda valitud Meilikausta igal alal, peate tuttavaid tööriistu ja platvormide abil.

## <a name="next-steps"></a>Järgmised sammud

Te saate lisateavet sündmuse jaoturi külastades järgmisi linke:

- Täieliku [valimi rakendus, mis kasutab sündmuse jaoturi][].
- [Välja sündmuse töötlemiseks sündmuse jaoturi skaala][] valimi.
- [Sündmuse jaoturi ülevaade][]

[Apache Avro]: http://avro.apache.org/
[tugiteenuse taotluse]: https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[1]: ./media/event-hubs-archive-overview/event-hubs-archive1.png
[2]: media/event-hubs-archive-overview/event-hubs-archive2.png
[Azure'i salvestusruumi Explorer]: http://azurestorageexplorer.codeplex.com/
[3]: ./media/event-hubs-archive-overview/event-hubs-archive3.png
[Avro tööriistad]: http://www-us.apache.org/dist/avro/avro-1.8.1/java/avro-tools-1.8.1.jar
[Java]: http://avro.apache.org/docs/current/gettingstartedjava.html
[Python]: http://avro.apache.org/docs/current/gettingstartedpython.html
[Sündmuse jaoturi ülevaade]: event-hubs-overview.md
[valimi rakendus, mis kasutab sündmuse jaoturi]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Välja sündmuse töötlemiseks sündmuse jaoturi skaala]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3