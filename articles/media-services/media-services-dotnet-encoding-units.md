<properties 
    pageTitle="Kuidas lisada kodeering ühikud" 
    description="Siit saate teada, kuidas lisada kodeering üksuste .net-i kohta"  
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/01/2016"
    ms.author="juliako;milangada;gtrifonov"/>


#<a name="how-to-scale-encoding-with-net-sdk"></a>Kuidas mastaapimiseks .NET SDK kodeerimine

> [AZURE.SELECTOR]
- [Portaal](media-services-portal-scale-media-processing.md )
- [.NET-I](media-services-dotnet-encoding-units.md)
- [ÜLEJÄÄNUD](https://msdn.microsoft.com/library/azure/dn859236.aspx)
- [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
- [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)

##<a name="overview"></a>Ülevaade

>[AZURE.IMPORTANT] Veenduge, et [Ülevaade](media-services-scale-media-processing-overview.md) teema skaleerimist meediumi töötlemine teema kohta lisateabe saamiseks vaadake üle.
 
Reserveeritud üksuse tüüp ja kodeerimine kasutades .NET SDK reserveeritud üksuste arvu muutmiseks tehke järgmist.

    IEncodingReservedUnit encodingS1ReservedUnit = _context.EncodingReservedUnits.FirstOrDefault();
    encodingS1ReservedUnit.ReservedUnitType = ReservedUnitType.Basic; // Corresponds to S1
    encodingS1ReservedUnit.Update();
    Console.WriteLine("Reserved Unit Type: {0}", encodingS1ReservedUnit.ReservedUnitType);
    
    encodingS1ReservedUnit.CurrentReservedUnits = 2;
    encodingS1ReservedUnit.Update();
    
    Console.WriteLine("Number of reserved units: {0}", encodingS1ReservedUnit.CurrentReservedUnits);

##<a name="opening-a-support-ticket"></a>Tugiteenuste Piletite avamine

Vaikimisi saab iga Media Servicesi konto skaala kuni 25 kodeering ja 5 nõudmisel Streaming reserveeritud ühikut. Saate taotleda kõrgema piirmäära, avades tugi Piletite.

###<a name="open-a-support-ticket"></a>Avage tugi Piletite

Tugi avamiseks Piletite tehke järgmist.

1. Klõpsake [tootetugi](https://manage.windowsazure.com/?getsupport=true). Kui te pole logitud, palutakse teil sisestada mandaat.

1. Valige oma tellimus.

1. Valige jaotises Tüüp tugi "Tehniliste".

1. Klõpsake "Loo Piletite".

1. Valige "Azure Media Servicesi" loendis tootenumber esitatud järgmisel leheküljel.

1. Valige "probleemi tüüp" mis on asjakohane teie probleemi.

1. Klõpsake nuppu Jätka.

1. Järgige järgmisele lehele ja seejärel sisestage oma probleemi üksikasjad.

1. Klõpsake nuppu Edasta Piletite selle avamiseks.



##<a name="media-services-learning-paths"></a>Media Servicesi Õppeteed

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
