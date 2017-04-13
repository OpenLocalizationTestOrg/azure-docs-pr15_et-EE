<properties
    pageTitle="Azure'i sündmuse jaoturi arhiivi kiirtutvustus | Microsoft Azure'i"
    description="Näide, mis kasutab Azure Python SDK näidata sündmuse jaoturi arhiivi funktsiooni abil."
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

# <a name="event-hubs-archive-walkthrough-python"></a>Sündmuse jaoturi arhiivi tutvustust: Python

Sündmuse jaoturi arhiivi on uus funktsioon ürituse jaoturi, mis võimaldab teil automaatselt esitamisel voo andmeid oma ürituse keskuse kaudu konto Azure'i bloobimälu oma valik. See on lihtne teha Pakktöötlus reaalajas voogesituse andmete põhjal. Selles artiklis kirjeldatakse, kuidas kasutada Python sündmuse jaoturi arhiivi. Sündmuse jaoturi arhiivi kohta leiate lisateavet teemast [ülevaate artikli](event-hubs-archive-overview.md).

See näide kasutab Azure Python SDK näitamaks arhiivimisfunktsiooni abil. Funktsiooni sender.py saadab jäljendatud keskkonna telemeetria sündmuse jaoturi JSON-vormingus. Sündmuse jaoturi on konfigureeritud kasutama arhiivimisfunktsiooni kirjutamiseks Bloobivahemälu salvestusruumi pakettidena andmed. Seejärel soovitud archivereader.py loeb need plekid ja loob lisa faili seadme kohta ja kirjutab andmed CSV-failid.

Mida saab teha

1.  Azure'i bloobimälu konto ja portaalis Azure'i bloobimälu container, loomine

2.  Sündmuse jaoturi nimeruumi, mis Azure portaali loomine

3.  Mõni sündmus keskuse loomine arhiivi funktsioon, mis on sisse lülitatud, Azure'i portaalis

4.  Andmete saatmine sündmuse keskuses Python script

5.  Lugeda faile arhiivi ja töödelda teise Python skripti

Eeltingimused

1.  Python 2.7.x

2.  Azure'i tellimuse

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="create-an-azure-storage-account"></a>Azure Storage konto loomine

1.  [Azure'i portaali][]sisse logima.

2.  Portaali Vasakpoolsel navigeerimispaanil nuppu Uus, seejärel klõpsake andmete + salvestusruumi ja klõpsake salvestusruumi konto.

3.  Täitke väljad tera salvestusruumi konto ja klõpsake nuppu **Loo**.

    ![][1]

4.  Kui näete teadet **Juurutuste õnnestus** , klõpsake uue salvestusruumi konto ja klõpsake **Essentialsi** tera **plekid**. **Bloobimälu teenuse** tera avanemisel klõpsake **+ Container** ülaosas. Container **arhiivi**nime ja seejärel sulgege **Bloobivahemälu teenuse** tera.

5.  Klõpsake vasakul tera **Pääsuklahvide** ja kopeerige salvestusruumi konto nimi ja **võti1**väärtus. Salvestage need väärtused Notepad või mõne muu ajutine asukoht.

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

## <a name="create-a-python-script-to-send-events-to-your-event-hub"></a>Python skript sündmuste saata oma sündmuse keskuse loomine

1.  Avage oma lemmik Python redaktor, nt [Visual Studio kood][].

2.  Luua skripti, mida nimetatakse **sender.py**. See skript saadab 200 sündmuste oma sündmuse keskuses. Need on lihtne keskkonna näite JSON saadetud.

3.  Kleepige sender.py järgmine kood:

    ```
    import uuid
    import datetime
    import random
    import json
    from azure.servicebus import ServiceBusService
    
    sbs = ServiceBusService(service_namespace='INSERT YOUR NAMESPACE NAME', shared_access_key_name='RootManageSharedAccessKey', shared_access_key_value='INSERT YOUR KEY')
    devices = []
    for x in range(0, 10):
        devices.append(str(uuid.uuid4()))
    
    for y in range(0,20):
        for dev in devices:
            reading = {'id': dev, 'timestamp': str(datetime.datetime.utcnow()), 'uv': random.random(), 'temperature': random.randint(70, 100), 'humidity': random.randint(70, 100)}
            s = json.dumps(reading)
            sbs.send\_event('myhub', s)
        print y
    ```
4.  Värskendage eelmise koodi kasutada oma nimeruumi nimi ja ürituse jaoturi nimeruumi loomisel saadava väärtused.

## <a name="create-a-python-script-to-read-your-archive-files"></a>Python skript lugeda arhiivi failide loomine

1.  Täitke tera ja klõpsake nuppu **Loo**.

2.  Luua skripti, mida nimetatakse **archivereader.py**. See skript loeb arhiivi failid ja seadme ainult seadme jaoks andmete kirjutamiseks kohta faili loomine.

3.  Kleepige archivereader.py järgmine kood:

    ```
    import os
    import string
    import json
    import avro.schema
    from avro.datafile import DataFileReader, DataFileWriter
    from avro.io import DatumReader, DatumWriter
    from azure.storage.blob import BlockBlobService
    
    def processBlob(filename):
        reader = DataFileReader(open(filename, 'rb'), DatumReader())
        dict = {}
        for reading in reader:
            parsed\_json = json.loads(reading["Body"])
            if not 'id' in parsed\_json:
                return
            if not dict.has\_key(parsed\_json['id']):
            list = []
            dict[parsed\_json['id']] = list
        else:
            list = dict[parsed\_json['id']]
            list.append(parsed\_json)
        reader.close()
        for device in dict.keys():
            deviceFile = open(device + '.csv', "a")
            for r in dict[device]:
                deviceFile.write(", ".join([str(r[x]) for x in r.keys()])+'\\n')

    def startProcessing(accountName, key, container):
        print 'Processor started using path: ' + os.getcwd()
        block\_blob\_service = BlockBlobService(account\_name=accountName, account\_key=key)
        generator = block\_blob\_service.list\_blobs(container)
        for blob in generator:
            if blob.properties.content\_length != 0:
                print('Downloaded a non empty blob: ' + blob.name)
                cleanName = string.replace(blob.name, '/', '\_')
                block\_blob\_service.get\_blob\_to\_path(container, blob.name, cleanName)
                processBlob(cleanName)
                os.remove(cleanName)
            block\_blob\_service.delete\_blob(container, blob.name)
    startProcessing('YOUR STORAGE ACCOUNT NAME', 'YOUR KEY', 'archive')
    ```

4.  Veenduge, et kleepida väärtused salvestusruumikonto nimi ja klahvi kõne `startProcessing`.

## <a name="run-the-scripts"></a>Skriptide käivitamine

1.  Avage käsuviip, mis on Python teest ja seejärel käivitage järgmised käsud Python eelnevalt nõutud pakettide installimiseks.

    ```
    pip install azure-storage
    pip install azure-servicebus
    pip install avro
    ```
  
    Kui teil on kas azure-salvestusruumi varasemas versioonis või azure võib-olla peate kasutama funktsiooni **--täiendamine** suvand

    Samuti peate käivitage järgmine (enamikus arvutites pole vajalik):

    ```
    pip install cryptography
    ```

2.  Liikuge, kuhu salvestasite sender.py ja archivereader.py ja käivitage see käsk:

    ```
    start python sender.py
    ```
    
    See loob uue Python protsessi käivitamiseks saatja.

3. Nüüd oodake paar minutit käivitamiseks Arhiiv. Algne käsk aknasse tippige järgmine käsk:

    ```
    python archivereader.py
    ```

See arhiivi protsessor kasutab kohalikku kausta alla laadida kõik plekid salvestusruumi konto või ümbrises. See töötleb mõnda, mis pole tühjad ja kirjutage tulemusi kohaliku kataloog .csv-failina.

## <a name="next-steps"></a>Järgmised sammud

Te saate lisateavet ürituse jaoturi külastades järgmisi linke:

- [Sündmuse jaoturi ülevaade arhiivimine][]
- Täieliku [valimi rakendus, mis kasutab sündmuse jaoturi][].
- [Välja sündmuse töötlemiseks sündmuse jaoturi skaala][] valimi.
- [Sündmuse jaoturi ülevaade][]
 

[Azure'i portaal]: https://portal.azure.com/
[Sündmuse jaoturi ülevaade arhiivimine]: event-hubs-archive-overview.md
[1]: ./media/event-hubs-archive-python/event-hubs-python1.png
[About Azure storage accounts]: https://azure.microsoft.com/en-us/documentation/articles/storage-create-storage-account/
[Visual Studio kood]: https://code.visualstudio.com/
[Sündmuse jaoturi ülevaade]: event-hubs-overview.md
[valimi rakendus, mis kasutab sündmuse jaoturi]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Välja sündmuse töötlemiseks sündmuse jaoturi skaala]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
