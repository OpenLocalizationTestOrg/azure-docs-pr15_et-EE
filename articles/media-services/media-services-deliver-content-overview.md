<properties
    pageTitle="Videosisu klientidele | Microsoft Azure'i"
    description="Selles teemas antakse ülevaade sellest, mis on seotud teie videosisu Azure Media Servicesi abil."
    services="media-services"
    documentationCenter=""
    authors="Juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="juliako"/>


# <a name="deliver-content-to-customers"></a>Sisu esitamine klientidele
Streaming või video-on-demand sisu esitamisel klientidele teie eesmärk on pakkuda kõrge kvaliteediga video erinevate seadmete erinevate võrgu tingimustel.

Selle eesmärgi saavutamiseks saate teha järgmist.

- Kodeerida oma voos, et mitme bitikiirusega (kohandatava bitrate) videovoo. See hoolitseb kvaliteedi-ja võrgu.
- Microsoft Azure Media Servicesi [dünaamiline pakendit](media-services-dynamic-packaging-overview.md) abil dünaamiliselt uuesti pakkimine oma voo erinevaid protokolle. See hoolitseb streaming erinevates seadmetes. Media Services toetab kohaletoimetamise järgmised kohandatava bitrate voogesituse tehnoloogiaid: HTTP Live Streaming (HLS), sujuva voogesituse, MPEG-KRIIPSJOONE ja HDS (Adobe Primetime Access kes ainult) jaoks.

Selles artiklis antakse ülevaade oluline sisu kohaletoimetamise põhimõtet.

Märkige ruut teadaolevad probleemid, lugege teemat [teadaolevad probleemid](media-services-deliver-content-overview.md#known-issues).

## <a name="dynamic-packaging"></a>Dünaamiliste pakendit

Dünaamiline pakendit Media Servicesi sisaldava, saate pakkuda oma kohandatava bitrate MP4 või sujuva voogesituse kodeeritud sisu streaming Media Services (MPEG-kriips, HLS, sujuva voogesituse, HDS) poolt toetatavad ilma uuesti sisse neid vorminguid streaming paketti. Soovitame pakkuda oma sisu dünaamilise pakendit.

Dünaamiliste pakendit ära, peate tehke järgmist:

- Kodeerida Standard (allikas) faili kohandatava bitrate MP4 või kohandatava bitrate sujuva voogesituse failide kogum.
- Vähemalt üks tellitav streaming ühiku saamiseks streaming lõpp-punkti, millest soovite esitada oma sisu. Lisateabe saamiseks vaadake, [Kuidas mastaapimiseks nõudmisel streaming reserveeritud üksused](media-services-portal-manage-streaming-endpoints.md).

Dünaamiliste pakendit, saate talletada ja ühe salvestusruumi-vormingus failide eest maksta. Media Servicesi ehitada ja teeni teie taotluste sobiv vastus.

Lisaks juurdepääsu dünaamilise pakendit võimalusi, nõudmisel streaming reserveeritud üksuste teile asjakohast sealt võimsus, mida saab osta 200 Mbps kerimine. Vaikimisi tellitavate voogesituse on konfigureeritud ühiskasutuses eksemplari mudelit, mis serveri ressursid (nt Arvuta või sealt läbilaskevõime) kõigi teiste kasutajatega jagada. Saate parandada mõne nõudmisel streaming läbilaskevõime ostes nõudmisel streaming reserveeritud üksused.

Lisateavet leiate teemast [dünaamiline pakendit](media-services-dynamic-packaging-overview.md).

## <a name="filters-and-dynamic-manifests"></a>Filtrid ja dünaamiline manifestid

Oma varasid Media Services abil saate määratleda filtrid. Need on Serveripoolsed reeglid, mis aitab oma klientidele, näiteks teatud osas video esitamiseks või heli- ja renderduste (asemel kõik renderduste, mis on seotud vara) oma kliendi seadme käsitlemiseks alamhulga määramine. *Dünaamiliste manifestid* , mis luuakse siis, kui teie klient soovib alusel määratud üks või mitu filtrit video voogesituseks saavutada selle filtreerimine.

Lisateavet leiate teemast [filtrid ja dünaamiline manifestid](media-services-dynamic-manifest-overview.md).

## <a name="locators"></a>Lokaatorid

Pakkuda oma kasutaja URL, mida saab kasutada voogesituseks või teie sisu alla laadida, peate esmalt avaldada oma varade lisamine locator loomisega. Mõne locator võimaldab juurde pääseda vara failid. Media Services toetab kahte tüüpi lokaatorid.

- OnDemandOrigin lokaatorid. Neid kasutatakse meediumide (nt MPEG-kriips, HLS või sujuva voogesituse) või järk-järgult faile alla laadida.
-  Ühiskasutusega juurdepääsu allkirja (SAS) URL-i lokaatorid. Neid kasutatakse meediumifailide kohalikku arvutisse alla laadida.

*Accessi poliitika* on abil saate määratleda õigused (nt lugemine, kirjutamine ja loendi) ja mille kliendil on juurdepääs konkreetse vara kestus. Pange tähele, et loendi õigus (AccessPermissions.List) ei tohi kasutada mõne OrDemandOrigin locator loomisel.

Lokaatorid on aegumiskuupäev. Azure'i portaalis seab lokaatorid aegumiskuupäeva 100 aastat tulevikus.

>[AZURE.NOTE] Kui kasutasite Azure portaali loomine lokaatorid enne märts 2015, nende lokaatorid määratud aegumise pärast kahte aastat.

Klõpsake soovitud locator aegumiskuupäeva värskendamiseks kasutada [ülejäänud](http://msdn.microsoft.com/library/azure/hh974308.aspx#update_a_locator ) või [.net-i](http://go.microsoft.com/fwlink/?LinkID=533259) API-de. Pange tähele, et SAS locator aegumiskuupäeva värskendamisel URL muutub.

Lokaatorid ei ole mõeldud haldamiseks kasutaja juurdepääsu reguleerimine. Saate anda erinevad juurdepääsuõigused üksikkasutajale, kasutades digitaalõiguste halduse (DRM) lahendusi. Lisateabe saamiseks vt [Turvaliseks meediumi](http://msdn.microsoft.com/library/azure/dn282272.aspx).

Kui loote mõnda locator, võib olla 30-sekundilist viivitust nõutav salvestusruumi ja paljundamine Azure Storage tingitud.


## <a name="adaptive-streaming"></a>Adaptive streaming

Kohandatavad bitrate tehnoloogiad lubada videopleieri võrgu läbilaskevõime määratlemine ja valida mitu bitikiirus. Kui võrguühendus laguneb klient saab valida madalam bitrate nii, et taasesitus saate jätkata alumise videokvaliteet. Võrgu läbilaskevõime parandamiseks kliendi saate aktiveerige suurem bitrate täiustatud video kvaliteet. Azure Media Servicesi toetab järgmisi kohandatava bitrate tehnoloogiaid: HTTP Live Streaming (HLS), sujuva voogesituse, MPEG-kriips ja HDS.

Kasutajatele streaming URL-id, esmalt peate looma ka OnDemandOrigin locator. Funktsiooni locator loomine annab teile base tee varade, mille sisu soovite taustal alla laadida. Siiski saama sisu voona, peate muutma selle tee põhjalikumaks. Ehitada streaming Avaldamisfail täielik URL-i, peate concatenate soovitud locator tee väärtus ja manifest (filename.ism) faili nimi. Seejärel **lisab/näidata** ja asjakohase vormingu (vajadusel) locator tee.

>[AZURE.NOTE]Voogesituseks sisu SSL-ühenduse kaudu. Selleks, veenduge, et HTTPS Alusta voogesituse URL-ide.

Ainult voogesituseks SSL kui streaming lõpp-punkti, kus saate esitada oma sisu on loodud pärast 10 mai 2014. Kui streaming URL-ide põhinevad streaming lõpp-punktid 10 mai 2014 loodud URL sisaldab "streaming.mediaservices.windows.net." SSL-i ei toeta streaming URL-id, mis sisaldavad "origin.mediaservices.windows.net" (vana vorming). Kui teie URL on vana vormingus ja soovite üle SSL-i, luua uue streaming lõpp-punkti. URL-id, mis põhineb streaming uue lõpp-punkti abil sisu voona SSL.


## <a name="streaming-url-formats"></a>Streaming URL-i vormingud

### <a name="mpeg-dash-format"></a>MPEG-kriips-vorming

{streaming lõpp-punkti nimi-media Servicesi konto name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

http://testendpoint-testaccount.streaming.mediaservices.Windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=MPD-Time-CSF)



### <a name="apple-http-live-streaming-hls-v4-format"></a>Apple HTTP Live Streaming (HLS) V4 vorming

{streaming lõpp-punkti nimi-media Servicesi konto name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

http://testendpoint-testaccount.streaming.mediaservices.Windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=m3u8-AAPL)

### <a name="apple-http-live-streaming-hls-v3-format"></a>Apple HTTP Live Streaming (HLS) V3 vorming

{streaming lõpp-punkti nimi-media Servicesi konto name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl-v3)

http://testendpoint-testaccount.streaming.mediaservices.Windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=m3u8-AAPL-v3)

### <a name="apple-http-live-streaming-hls-format-with-audio-only-filter"></a>Apple HTTP Live Streaming (HLS) vormingus ainult filtriga

Vaikimisi on HLS ainult heli lugusid kaasatud manifesti. See on vajalik Apple Store sertimine mobiilsidevõrgu. Sel juhul, kui mõnda muud klienti pole piisavalt ribalaiust või on ühendatud üle 2G-ühendust, Taasesita aktiveerib ainult heli. See aitab hoida sisu voogesitamine ilma puhverdamine, kuid ei ole video. Mõnel juhul puhverdamine mängija võib olla eelistatud üle ainult heli. Kui soovite eemaldada ainult jälitus, lisage **ainult = false** URL.

http://testendpoint-testaccount.streaming.mediaservices.Windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=m3u8-AAPL-v3,audio-Only=FALSE)

Lisateabe saamiseks vt [dünaamiline näidata koosseis tugi ja HLS väljund lisavõimalusi](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/).


### <a name="smooth-streaming-format"></a>Sujuv Streaming vorming

{streaming lõpp-punkti nimi-media Servicesi konto name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

Näide:

http://testendpoint-testaccount.streaming.mediaservices.Windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest

### <a id="fmp4_v20"></a>Sujuva voogesituse 2.0 manifesti (pärand manifest)

Sujuva voogesituse manifest vorming sisaldab vaikimisi korrake silt (r-silt). Mõned mängijad toetavad r-silt. Need mängijad klientidele saate kasutada vormingut, mida keelab r-silt.

{streaming lõpp-punkti nimi-media Servicesi konto name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=fmp4-v20)

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=fmp4-v20)

### <a name="hds-for-adobe-primetimeaccess-licensees-only"></a>HDS (jaoks Adobe PrimeTime Access kes ainult)

{streaming lõpp-punkti nimi-media Servicesi konto name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=f4m-f4f)

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=f4m-f4f)

## <a name="progressive-download"></a>Järk-järgult allalaadimine

Järk-järgult alla laadida, saate alustada esitamine media enne kogu fail on alla laaditud. Te ei saa järk-järgult .ism * (ismv, isma, ismt või ismc) faile alla laadida.

Järk-järgult sisu alla laadida, kasutage locator OnDemandOrigin tüüp. Järgmises näites on kujutatud URL, mis põhineb locator OnDemandOrigin tüüp:

    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4

Peate dekrüptida salvestusruumi krüptitud varad, soovitud voona origin teenuse järk-järgult alla laadida.

## <a name="download"></a>Laadi alla

Oma sisu kliendi seadmesse alla laadida, tuleb teil luua SAS Locator. SAS locator kaudu pääsete juurde Azure Storage ümbris, kus teie fail asub. Koostamiseks alla URL, tuleb manustada faili nimi host ja SAS allkirja vahel.

Järgmises näites on kujutatud URL, mis põhineb SAS locator:

    https://test001.blob.core.windows.net/asset-ca7a4c3f-9eb5-4fd8-a898-459cb17761bd/BigBuckBunny.mp4?sv=2012-02-12&se=2014-05-03T01%3A23%3A50Z&sr=c&si=7c093e7c-7dab-45b4-beb4-2bfdff764bb5&sig=msEHP90c6JHXEOtTyIWqD7xio91GtVg0UIzjdpFscHk%3D

Kehtivad järgmisega.

- Peate dekrüptida salvestusruumi krüptitud varad, soovitud voona origin teenuse järk-järgult alla laadida.
- Laadi alla, mis pole lõpetanud 12 tunni jooksul nurjub.

## <a name="streaming-endpoints"></a>Streaming lõpp-punktid

Streaming lõpp-punkti tähistab streaming teenus, mis suudab sisu otse kliendi Playeri rakendus või sisu kohaletoimetamise võrk (CDN) täpsemaks jaotuse. Väljamineva voo kaudu videote voogesitamise teenuse lõpp-punkti saab reaalajas voo või video-on-demand varade Media Servicesi kontol. Saate määrata ka streaming teenuse lõpp-punkti võimsus kasvab läbilaskevõime vajadustele reageerimine, reguleerides streaming reserveeritud üksused. Rakenduste tootmiskeskkonnas eraldama reserveeritud vähemalt ühe üksuse. Lisateavet leiate teemast [skaala meediumid teenus](media-services-portal-manage-streaming-endpoints.md).

## <a name="known-issues"></a>Teadaolevad probleemid

### <a name="changes-to-smooth-streaming-manifest-version"></a>Muudatuste sujuva voogesituse näidata versioon

Enne juuli 2016 teenuse Väljalaske - varad esitatud Media kodeerija Standard Media kodeerija Premium töövoo või varasemates Azure Media Encoderi olid voona, kasutades dünaamilise pakendit--sujuva voogesituse manifesti tagastatud vastaks versioonilt 2.0. Versioonis 2.0 Kasuta fragment kestused nn kordamine (r)-sildid. Näiteks:

<?xml version="1.0" encoding="UTF-8"?>
    <SmoothStreamingMedia MajorVersion="2" MinorVersion="0" Duration="8000" TimeScale="1000">
        <StreamIndex Chunks="4" Type="video" Url="QualityLevels({bitrate})/Fragments(video={start time})" QualityLevels="3" Subtype="" Name="video" TimeScale="1000">
            <QualityLevel Index="0" Bitrate="1000000" FourCC="AVC1" MaxWidth="640" MaxHeight="360" CodecPrivateData="00000001674D4029965201405FF2E02A100000030010000003032E0A000F42400040167F18E3050007A12000200B3F8C70ED0B16890000000168EB7352" />
            <c t="0" d="2000" n="0" />
            <c d="2000" />
            <c d="2000" />
            <c d="2000" />
        </StreamIndex>
    </SmoothStreamingMedia>

Juuli 2016 teenuse versioonis loodud sujuva voogesituse manifest vastab versiooni 2.2, fragment kestused korrake siltide kasutamine. Näiteks:

    <?xml version="1.0" encoding="UTF-8"?>
    <SmoothStreamingMedia MajorVersion="2" MinorVersion="2" Duration="8000" TimeScale="1000">
        <StreamIndex Chunks="4" Type="video" Url="QualityLevels({bitrate})/Fragments(video={start time})" QualityLevels="3" Subtype="" Name="video" TimeScale="1000">
            <QualityLevel Index="0" Bitrate="1000000" FourCC="AVC1" MaxWidth="640" MaxHeight="360" CodecPrivateData="00000001674D4029965201405FF2E02A100000030010000003032E0A000F42400040167F18E3050007A12000200B3F8C70ED0B16890000000168EB7352" />
            <c t="0" d="2000" r="4" />
        </StreamIndex>
    </SmoothStreamingMedia>

Mõned varasemate versioonide sujuva voogesituse kliendid võivad toetada korrake sildid ja manifest laadimine nurjub. Selle probleemi leevendada, saate kasutada pärand manifest vormingus parameetri **(vorming = fmp4-v20)** või värskendage oma kliendi uusim versioon, mis toetab korrake sildid. Lisateavet leiate teemast [sujuva voogesituse 2.0](media-services-deliver-content-overview.md#fmp4_v20).

## <a name="media-services-learning-paths"></a>Media Servicesi Õppeteed

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-topics"></a>Seotud teemad

[Värskendage Media Servicesi lokaatorid pärast salvestusruumi võtmed](media-services-roll-storage-access-keys.md)
