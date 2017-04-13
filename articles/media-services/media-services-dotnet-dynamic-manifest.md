<properties 
    pageTitle="Filtrite Azure Media Servicesi .NET SDK loomine" 
    description="Selles teemas kirjeldatakse, kuidas luua filtrid, et teie klient saate neid kasutada voo voo kindla osa. Media Servicesi loob dünaamilise manifestid valikulise streaming saavutamiseks." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ne" 
    ms.topic="article" 
    ms.date="07/18/2016"
    ms.author="juliako;cenkdin"/>


#<a name="creating-filters-with-azure-media-services-net-sdk"></a>Filtrite Azure Media Servicesi .NET SDK loomine

> [AZURE.SELECTOR]
- [.NET-I](media-services-dotnet-dynamic-manifest.md)
- [ÜLEJÄÄNUD](media-services-rest-dynamic-manifest.md)

Alustades 2.11 väljaanne, Media Services võimaldab teil oma varasid filtrite määratlemine. Need on serveri pool reeglid, mis võimaldab valida näiteks teha klientide: taasesitus video (mitte tervet video esitamine), ainult osa või määrake heli ja video renderduste (asemel kõik renderduste, mis on seotud vara) oma kliendi seadme käsitlemiseks alamhulka. **Dünaamiliste näidata**s oma kliendi taotluse alusel määratud filtrid video voogesituseks loodud saavutada selle oma varasid filtreerimine.

Filtrite ja dünaamiline näidata seotud üksikasjalikumat teavet teemast [dünaamiline manifestid ülevaade](media-services-dynamic-manifest-overview.md).

Selles teemas kirjeldatakse, kuidas luua, värskendada ja kustutada filtrid Media Services .NET SDK abil. 


Pange tähele, kui värskendate filtri, võib kuluda kuni kaks minutit streaming lõpp-punkti värskendamiseks reegleid. Kui sisu toimetati abil filtrit (ja vahemälus talletatud puhverserverite ja CDN vahemälu), see filtri värskendamine võib põhjustada mängija ebaõnnestumist. See on soovitatav pärast värskendamist filtri vahemälu tühjendamiseks. Kui see suvand pole võimalik, kaaluge erinevate filter. 

##<a name="types-used-to-create-filters"></a>Filtrite loomiseks kasutatud tüübid

Järgmist tüüpi kasutatakse loomisel filtrid: 

- **IStreamingFilter**.  Seda tüüpi põhineb järgmise REST API [Filter](http://msdn.microsoft.com/library/azure/mt149056.aspx)
- **IStreamingAssetFilter**. Seda tüüpi põhineb järgmised REST API [AssetFilter](http://msdn.microsoft.com/library/azure/mt149053.aspx)
- **PresentationTimeRange**. Seda tüüpi põhineb järgmised REST API [PresentationTimeRange](http://msdn.microsoft.com/library/azure/mt149052.aspx)
- **FilterTrackSelectStatement** ja **IFilterTrackPropertyCondition**. Järgmist tüüpi põhinevad järgmised REST API-de [FilterTrackSelect ja FilterTrackPropertyCondition](http://msdn.microsoft.com/library/azure/mt149055.aspx)


##<a name="createupdatereaddelete-global-filters"></a>Loomine ja värskendamine/lugemine/kustutamine globaalsest filtrite

Järgmine kood kujutatakse .net-i abil saate luua, värskendada, lugeda ja kustutada varade filtrid.
    
    string filterName = "GlobalFilter_" + Guid.NewGuid().ToString();
                
    List<FilterTrackSelectStatement> filterTrackSelectStatements = new List<FilterTrackSelectStatement>();
    
    FilterTrackSelectStatement filterTrackSelectStatement = new FilterTrackSelectStatement();
    filterTrackSelectStatement.PropertyConditions = new List<IFilterTrackPropertyCondition>();
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackNameCondition("Track Name", FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackBitrateRangeCondition(new FilterTrackBitrateRange(0, 1), FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackTypeCondition(FilterTrackType.Audio, FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatements.Add(filterTrackSelectStatement);
    
    // Create
    IStreamingFilter filter = _context.Filters.Create(filterName, new PresentationTimeRange(), filterTrackSelectStatements);
    
    // Update
    filter.PresentationTimeRange = new PresentationTimeRange(timescale: 500);
    filter.Update();
    
    // Read
    var filterUpdated = _context.Filters.FirstOrDefault();
    Console.WriteLine(filterUpdated.Name);

    // Delete
    filter.Delete();


##<a name="createupdatereaddelete-asset-filters"></a>Loomine ja värskendamine/lugemine/kustutamine varade filtrid

Järgmine kood kujutatakse .net-i abil saate luua, värskendada, lugeda ja kustutada varade filtrid.

    
    string assetName = "AssetFilter_" + Guid.NewGuid().ToString();
    var asset = _context.Assets.Create(assetName, AssetCreationOptions.None);
    
    string filterName = "AssetFilter_" + Guid.NewGuid().ToString();
    
        
    // Create
    IStreamingAssetFilter filter = asset.AssetFilters.Create(filterName,
                                        new PresentationTimeRange(), 
                                        new List<FilterTrackSelectStatement>());
    
    // Update
    filter.PresentationTimeRange = 
            new PresentationTimeRange(start: 6000000000, end: 72000000000);
    
    filter.Update();
    
    // Read
    asset = _context.Assets.Where(c => c.Id == asset.Id).FirstOrDefault();
    var filterUpdated = asset.AssetFilters.FirstOrDefault();
    Console.WriteLine(filterUpdated.Name);
    
    // Delete
    filterUpdated.Delete();
    



##<a name="build-streaming-urls-that-use-filters"></a>Koostage streaming URL-id, mis kasutavad filtrid

Avaldamine ja pakkuda oma varasid kohta teabe saamiseks vt [Pakkuda sisu, et kliendid ülevaade](media-services-deliver-content-overview.md).


Järgmised näited kujutavad streaming URL-ide filtrite lisamine.

**MPEG MÕTTEKRIIPSU** 

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf, filter=MyFilter)

**Apple HTTP Live Streaming (HLS) V4**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl, filter=MyFilter)

**Apple HTTP Live Streaming (HLS) V3**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3, filter=MyFilter)

**Sujuva voogesituse**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=MyFilter)


**HDS**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=f4m-f4f, filter=MyFilter)


##<a name="media-services-learning-paths"></a>Media Servicesi Õppeteed

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="see-also"></a>Vt ka 

[Dünaamiliste manifestid ülevaade](media-services-dynamic-manifest-overview.md)
 

