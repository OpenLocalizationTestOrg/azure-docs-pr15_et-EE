<properties
    pageTitle="Alustamine sündmuse jaoturi C ja Apache torm | Microsoft Azure'i"
    description="Järgige selle õpetuse Azure'i sündmuse jaoturi; kasutamise alustamine saata sündmuste C ja neid on Apache Storm kobar vastu võtta."
    services="event-hubs"
    documentationCenter=""
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="c"
    ms.devlang="java"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Sündmuse jaoturi kasutamise alustamine

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Sissejuhatus

Sündmuse jaoturi on väga paindlik manustamisest süsteem, mis saab tarbimine miljoneid sündmuste sekundis, lubada rakenduse töötlemine ja suurel hulgal andmete analüüsimiseks toodetud ühendatud seadmete ja rakendused. Kui kogunud sisse sündmuse jaoturi, saate muuta ja reaalajas analytics pakkuja või salvestusruumi kobar abil andmete talletamiseks.

Lisateabe saamiseks lugege [sündmuse jaoturi ülevaade].

Selles õppetükis saate teada, kuidas neelata sõnumite konsooli rakendus kasutamine C sündmuse jaoturiga ja laadida neid paralleelselt Apache Storm abil.

Selle õpetuse tegemiseks on vaja järgmist:

+ C arenduskeskkond. Selles õpetuses mõeldud me endale gcc virnas [Azure Linux VM](../virtual-machines/virtual-machines-linux-quick-create-cli.md) Ubuntu 14.04 abil. Juhised muudes antakse väliseid linke.

+ Java arendamise keskkond [Maven](http://maven.apache.org/)konfigureeritud. Selles õpetuses mõeldud me endale [Eclipse](https://www.eclipse.org/).

+ Aktiivne Azure'i konto. Kui teil pole kontot, saate luua tasuta konto vaid paar minutit. Lisateavet leiate teemast [Azure tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/).

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-c](../../includes/service-bus-event-hubs-get-started-send-c.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-storm](../../includes/service-bus-event-hubs-get-started-receive-storm.md)]

## <a name="run-the-applications"></a>Rakenduste käivitamine

Nüüd olete valmis rakendusi käivitada.

1.  Käita Eclipse **LogTopology** tunni, siis oodake selle käivitamiseks on kõik partitsioonid vastuvõtjad.

2.  Käivita programm **saatja** ja vastuvõtja aknas kuvatakse üritused.

    ![][23]

> [AZURE.NOTE] Ainult selles õpetuses kasutada Storm kohalikus režiimis arendamiseks. Vaadake [Hdinsightiga torm ülevaade] ja ametlik [Apache Storm] dokumentatsiooni kohta lisateabe saamiseks Storm juurutuste ja mustrid.

## <a name="next-steps"></a>Järgmised sammud

Järgmised ressursid on saadaval sündmuse jaoturi ja Storm integreerimine rakenduste arendamise.

- [Torm ja Hdinsightiga andur andmete analüüsimine][] on täielik stsenaarium õpetus neelata andur andmed on Hadoopi kobar sündmuse jaoturi, Storm ja HBase abil.
- [Töötada streaming andmete töötlemise rakendused SCP.NET ja C# Storm ja Hdinsightiga][] on õpetus, kuidas kirjutada Storm torujuhtmete abil C#.

<!-- Images. -->
[23]: ./media/event-hubs-c-storm-getstarted/receive-storm3.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Event Processor Host]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Sündmuse jaoturi ülevaade]: event-hubs-overview.md

[Apache torm]: https://storm.incubator.apache.org
[Hdinsightiga torm ülevaade]: ../hdinsight/hdinsight-storm-overview.md/
[Torm ja Hdinsightiga andur andmete analüüsimine]: ../hdinsight/hdinsight-storm-sensor-data-analysis.md
[Arendamise streaming andmete töötlemise rakendused SCP.NET ja C# Storm ja Hdinsightiga]: ../hdinsight/hdinsight-storm-develop-csharp-visual-studio-topology.md
