<properties
    pageTitle="Sündmuse jaoturi Java alustamine | Microsoft Azure'i"
    description="Järgige selle õpetuse Azure'i sündmuse jaoturi; kasutamise alustamine sündmuste Java saatmine ja vastuvõtmine nende kasutamisel on EventProcessorHost."
    services="event-hubs"
    documentationCenter=""
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="core"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Sündmuse jaoturi kasutamise alustamine

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Sissejuhatus

Sündmuse jaoturi on väga paindlik manustamisest süsteem, mis saab tarbimine miljoneid sündmuste sekundis, lubada rakenduse töötlemine ja suurel hulgal andmete analüüsimiseks toodetud ühendatud seadmete ja rakendused. Kui kogunud sisse sündmuse jaoturi, saate muuta ja andmeid mis tahes reaalajas analytics pakkuja või salvestusruumi kobar salvestada.

Lisateavet leiate teemast [sündmuse jaoturi ülevaade][].

Selle õpetuse kujutatakse neelata sõnumite konsooli rakendust Java sündmuse jaoturiga ja laadida neid paralleelselt, kasutades Java sündmuse protsessor hosti teek.

Selle õpetuse lõpuleviimiseks on vaja järgmist:

+ Java arenduskeskkond. Selles õpetuses mõeldud me endale [Eclipse](https://www.eclipse.org/).

+ Aktiivne Azure'i konto. <br/>Kui teil pole kontot, saate luua tasuta konto vaid paar minutit. Lisateavet leiate teemast <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Azure tasuta prooviversioon</a>.

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-java](../../includes/service-bus-event-hubs-get-started-send-java.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-ephjava](../../includes/service-bus-event-hubs-get-started-receive-ephjava.md)]

## <a name="run-the-applications"></a>Rakenduste käivitamine

Nüüd olete valmis rakendusi käivitada.

1.  Käivitage **vastuvõtja** projekt.

    ![][21]

2.  Käivitage project **saatja** .

    ![][22]

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete varem töötamise rakendus, mis loob mõne sündmuse jaoturi ja saadab ja võtab vastu andmeid, saate teisaldada järgmistel juhtudel:

- Täieliku [valimi rakendus, mis kasutab sündmuse jaoturi][].
- [Välja sündmuse töötlemiseks sündmuse jaoturi skaala][] valimi.

Lisateavet leiate teemast [Java Arenduskeskus](/develop/java/).

<!-- Images. -->
[21]: ./media/event-hubs-java-ephjava-getstarted/ephjava.png
[22]: ./media/event-hubs-java-ephjava-getstarted/java-send.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Sündmuse jaoturi ülevaade]: event-hubs-overview.md
[valimi rakendus, mis kasutab sündmuse jaoturi]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Välja sündmuse töötlemiseks sündmuse jaoturi skaala]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
 