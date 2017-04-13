<properties 
    pageTitle="Kasutades .NET Azure Media Servicesi sisu avaldamine" 
    description="Saate teada, kuidas luua locator, mis saab koostada streaming URL-i. Koodinäiteid C# kirjutada ja kasutada Media Services SDK .net-i jaoks." 
    authors="juliako" 
    manager="erikre" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/30/2016"
    ms.author="juliako"/>


# <a name="publish-azure-media-services-content-using-net"></a>Kasutades .NET Azure Media Servicesi sisu avaldamine
 
> [AZURE.SELECTOR]
- [ÜLEJÄÄNUD](media-services-rest-deliver-streaming-content.md)
- [.NET-I](media-services-deliver-streaming-content.md)
- [Portaal](media-services-portal-publish.md)

##<a name="overview"></a>Ülevaade

Voogesituseks on kohandatava bitrate MP4 määratud kuvatakse nõudmisel streaming locator loomise ja koostamise streaming URL. [Vara kodeerimine](media-services-encode-asset.md) teema näitab, kuidas kodeerida on kohandatava bitrate MP4 seadmine. 

>[AZURE.NOTE]Kui teie sisu on krüptitud, konfigureerida varade kohaletoimetamise poliitika ( [selles](media-services-dotnet-configure-asset-delivery-policy.md) teemas kirjeldatud) enne loomist on locator. 

Saate ka nõudmisel streaming locator koostamiseks URL-id, mis viitavad MP4 faile, mida saab järk-järgult alla laadida.  

Selles teemas kirjeldatakse, kuidas luua mõne nõudmisel streaming locator avaldada oma varade ja koostada sujuva, MPEG MÕTTEKRIIPSU ja HLS streaming URL-id. See näitab kuum järk-järgult allalaadimine URL-ide koostamiseks. 
     
##<a name="create-an-ondemand-streaming-locator"></a>Kuvatakse nõudmisel streaming locator loomine

Luua nõudmisel, streaming locator ja saada URL-ide peate tegema järgmist:

   1. Kui sisu on krüptitud, määratleda Accessi poliitika.
   2. Saate luua ka nõudmisel streaming locator.
   3. Kui plaanite voona, saate streaming Avaldamisfail (.ism) vara. 
        
    Kui plaanite järk-järgult alla laadida, saate vara MP4 failide nimesid.  
   4. URL-ide koostada manifestifaili või MP4 faile. 
   

###<a name="use-media-services-net-sdk"></a>Kasutage Media Servicesi .NET SDK 

Koostage Streaming URL-id 

    private static void BuildStreamingURLs(IAsset asset)
    {
    
        // Create a 30-day readonly access policy. 
        // You cannot create a streaming locator using an AccessPolicy that includes write or delete permissions.
        IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
            TimeSpan.FromDays(30),
            AccessPermissions.Read);
    
        // Create a locator to the streaming content on an origin. 
        ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
            policy,
            DateTime.UtcNow.AddMinutes(-5));
    
        // Display some useful values based on the locator.
        Console.WriteLine("Streaming asset base path on origin: ");
        Console.WriteLine(originLocator.Path);
        Console.WriteLine();
    
        // Get a reference to the streaming manifest file from the  
        // collection of files in the asset. 
        var manifestFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                                    EndsWith(".ism")).
                                    FirstOrDefault();
        
        // Create a full URL to the manifest file. Use this for playback
        // in streaming media clients. 
        string urlForClientStreaming = originLocator.Path + manifestFile.Name + "/manifest";
        Console.WriteLine("URL to manifest for client streaming using Smooth Streaming protocol: ");
        Console.WriteLine(urlForClientStreaming);
        Console.WriteLine("URL to manifest for client streaming using HLS protocol: ");
        Console.WriteLine(urlForClientStreaming + "(format=m3u8-aapl)");
        Console.WriteLine("URL to manifest for client streaming using MPEG DASH protocol: ");
        Console.WriteLine(urlForClientStreaming + "(format=mpd-time-csf)"); 
        Console.WriteLine();
    }

Koodi väljundid:
    
    URL to manifest for client streaming using Smooth Streaming protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest
    URL to manifest for client streaming using HLS protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest(format=m3u8-aapl)
    URL to manifest for client streaming using MPEG DASH protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest(format=mpd-time-csf)
    

>[AZURE.NOTE]Voogesituseks sisu SSL-ühenduse kaudu. Selleks, veenduge, et HTTPS Alusta voogesituse URL-ide. 

Koostage järk-järgult allalaadimine URL-id 

    private static void BuildProgressiveDownloadURLs(IAsset asset)
    {
        // Create a 30-day readonly access policy. 
        IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
            TimeSpan.FromDays(30),
            AccessPermissions.Read);
    
        // Create an OnDemandOrigin locator to the asset. 
        ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
            policy,
            DateTime.UtcNow.AddMinutes(-5));
    
        // Display some useful values based on the locator.
        Console.WriteLine("Streaming asset base path on origin: ");
        Console.WriteLine(originLocator.Path);
        Console.WriteLine();
    
        // Get MP4 files.
        IEnumerable<IAssetFile> mp4AssetFiles = asset
            .AssetFiles
            .ToList()
            .Where(af => af.Name.EndsWith(".mp4", StringComparison.OrdinalIgnoreCase));
                
        // Create a full URL to the MP4 files. Use this to progressively download files.
        foreach (var pd in mp4AssetFiles)
            Console.WriteLine(originLocator.Path + pd.Name);
    }

Koodi väljundid:
    
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4
    
    . . . 

###<a name="use-media-services-net-sdk-extensions"></a>Kasutage Media Services .NET SDK laiendid

Järgmine kood kõned .NET SDK laiendid viise, mida on locator loomine ja kohandatav voogesitus sujuva voogesituse, HLS ja MPEG-KRIIPSJOONE URL-ide genereerimine.

    // Create a loctor.
    _context.Locators.Create(
        LocatorType.OnDemandOrigin,
        inputAsset,
        AccessPermissions.Read,
        TimeSpan.FromDays(30));
    
    // Get the streaming URLs.
    Uri smoothStreamingUri = inputAsset.GetSmoothStreamingUri();
    Uri hlsUri = inputAsset.GetHlsUri();
    Uri mpegDashUri = inputAsset.GetMpegDashUri();
    
    Console.WriteLine(smoothStreamingUri);
    Console.WriteLine(hlsUri);
    Console.WriteLine(mpegDashUri);


##<a name="media-services-learning-paths"></a>Media Servicesi Õppeteed

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Vt ka

[Laadige alla varad](media-services-deliver-asset-download.md)
[konfigureerimine varade kohaletoimetamise poliitika](media-services-dotnet-configure-asset-delivery-policy.md)
