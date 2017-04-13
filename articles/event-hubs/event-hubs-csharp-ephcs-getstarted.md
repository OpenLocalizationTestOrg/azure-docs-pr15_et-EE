<properties
    pageTitle="Sündmuse jaoturi C# alustamine | Microsoft Azure'i"
    description="Järgige selle õpetuse Azure'i sündmuse jaoturi abil C# ja selle EventProcessorHost kasutamise alustamine."
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
    ms.date="09/02/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Sündmuse jaoturi kasutamise alustamine

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Sissejuhatus

Sündmuse jaoturi on teenus, mis töötleb suurte andmehulkade ürituse (telemeetria) ühendatud seadmete ja rakenduste kaudu. Pärast jõudlusandmeid koguda sisse sündmuse jaoturi, saate talletada salvestusruum kobar andmed või muuta selle abil reaalajas analytics pakkuja. Suuremahuliste sündmuste kogumine ja töötlemine võimalus on oluline osa kaasaegne rakendus arhitektuurides, sh selle Internet, asjade.

Selle õpetuse näitab, kuidas kasutada Azure klassikaline portaali mõni sündmus keskuse loomiseks. See ka näitab teile, kuidas koguda sõnumite abil kirjutatud C# konsooli rakendus sündmuse jaoturiga ja kuidas saab alla laadida paralleelselt, kasutades C# [Sündmuse protsessor hosti][] teek.

Selle õpetuse tegemiseks peate järgmist.

+ [Microsoft Visual Studio](http://visualstudio.com)

+ Aktiivne Azure'i konto. Kui teil pole ühte, saate luua tasuta konto vaid paar minutit. Lisateavet leiate teemast [Azure tasuta prooviversioon](https://azure.microsoft.com/free/).

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-csharp](../../includes/service-bus-event-hubs-get-started-send-csharp.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-ephcs](../../includes/service-bus-event-hubs-get-started-receive-ephcs.md)]

## <a name="run-the-applications"></a>Rakenduste käivitamine

Nüüd olete valmis rakendusi käivitada.

1. Jooksul Visual Studios, avage **vastuvõtja** projekti varem loodud.

2. **Vastuvõtja** lahenduse, paremklõpsake ja seejärel klõpsake nuppu **Lisa**ja klõpsake **Projekti**.
 
3. Otsige üles Sender.csproj olemasolev fail ja seejärel topeltklõpsake seda, et lisada see lahendus.
 
4. Uuesti, paremklõpsake **vastuvõtja** lahendus ja seejärel klõpsake käsku **Atribuudid**. Kuvatakse **vastuvõtja** atribuudileht.

5. Valige **Käivitus Project**ja seejärel nuppu **käivitus mitmed projektid** . Määrake **toiming** väljale soovitud **vastuvõtja** ja **saatja** projektide **alustamiseks**.

    ![][19]

6. Klõpsake nuppu **projekti sõltuvused**. Klõpsake väljal **projektide** **saatja**. Klõpsake väljal **sõltub** veenduge, et **vastuvõtja** oleks märgitud.

    ![][20]

7. Klõpsake nuppu **OK** , et jätta dialoogiboks **Atribuudid** .

1.  Vajutage klahvi F5, et käivitada **vastuvõtja** projekti Visual Studio sees, siis oodake, et alustada vastuvõtjad jaoks kõik sektsioonid.

    ![][21]

2.  **Saatja** projekt käivitub automaatselt. Vajutage sisestusklahvi **Enter** konsooli aken ja vaadake, kuvatakse aknas vastuvõtja sündmused.

    ![][22]

Vajutage **Klahvikombinatsiooni Ctrl + C** **saatja** aknas rakenduse saatja ja seejärel vajutage sisestusklahvi **Enter** vastuvõtja akna sulgeda selle rakenduse.

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete varem töötamise rakendus, mis loob mõne sündmuse jaoturi ja saadab ja võtab vastu andmeid, saate teisaldada järgmistel juhtudel:

- Täieliku [valimi rakendus, mis kasutab sündmuse jaoturi][].
- [Välja sündmuse töötlemiseks sündmuse jaoturi skaala][] valimi.
- [Sündmuse jaoturi ülevaade][]

<!-- Images. -->
[19]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj1.png
[20]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj2.png
[21]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs1.png
[22]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs2.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Sündmuse protsessor Host]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Sündmuse jaoturi ülevaade]: event-hubs-overview.md
[valimi rakendus, mis kasutab sündmuse jaoturi]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Välja sündmuse töötlemiseks sündmuse jaoturi skaala]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
[queued messaging solution]: ../service-bus-messaging/service-bus-dotnet-multi-tier-app-using-service-bus-queues.md
 
