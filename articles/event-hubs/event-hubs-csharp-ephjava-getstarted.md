<properties
    pageTitle="Sündmuse jaoturi C# alustamine | Microsoft Azure'i"
    description="Järgige selle õpetuse Azure'i sündmuse jaoturi; kasutamise alustamine saata sündmuste C# ja neid Java, kasutades funktsiooni EventProcessorHost vastu võtta."
    services="event-hubs"
    documentationCenter=""
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/27/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Sündmuse jaoturi kasutamise alustamine

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Sissejuhatus

Sündmuse jaoturi on teenus, mis töötleb suurte andmehulkade sündmuse (telemeetria) ühendatud seadmete ja rakenduste kaudu. Pärast jõudlusandmeid koguda sisse sündmuse jaoturi, saate talletada salvestusruum kobar andmed või muuta selle abil reaalajas analytics pakkuja. Suuremahuliste sündmuse kogumine ja töötlemine võimalus on oluline osa kaasaegne rakendus arhitektuurides, sh selle Internet, asjade.

Selle õpetuse näitab, kuidas kasutada Azure klassikaline portaali mõni sündmus keskuse loomiseks. See samuti näitab teile, kuidas koguda sõnumite abil kirjutatud C# konsooli rakendus sündmuse jaoturiga ja kuidas tuua neid paralleelselt, kasutades Java sündmuse protsessor hosti teek.

Selle õpetuse tegemiseks peate järgmist.

+ [Microsoft Visual Studio](http://visualstudio.com)

+ Aktiivne Azure'i konto. <br/>Kui teil pole ühte, saate luua tasuta konto vaid paar minutit. Lisateavet leiate teemast [Azure tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F target="_blank").

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-csharp](../../includes/service-bus-event-hubs-get-started-send-csharp.md)]

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
- [Sündmuse jaoturi ülevaade][]

<!-- Images. -->
[21]: ./media/event-hubs-csharp-ephjava-getstarted/ephjava.png
[22]: ./media/event-hubs-csharp-ephjava-getstarted/cs-send.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Sündmuse jaoturi ülevaade]: event-hubs-overview.md
[valimi rakendus, mis kasutab sündmuse jaoturi]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Välja sündmuse töötlemiseks sündmuse jaoturi skaala]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
