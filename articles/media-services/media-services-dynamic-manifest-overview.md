<properties 
    pageTitle="Filtrid ja dünaamiline manifestid | Microsoft Azure'i" 
    description="Selles teemas kirjeldatakse, kuidas luua filtrid, et teie klient saate neid kasutada voo voo kindla osa. Media Servicesi loob dünaamilise manifestid valikulise streaming arhiivida." 
    services="media-services" 
    documentationCenter="" 
    authors="cenkdin" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ne" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="cenkdin;juliako"/>

# <a name="filters-and-dynamic-manifests"></a>Filtrid ja dünaamiline manifestid

Alustades 2.11 väljaanne, Media Services võimaldab teil oma varasid filtrite määratlemine. Need on serveri pool reeglid, mis võimaldab valida näiteks teha klientide: taasesitus video (mitte tervet video esitamine), ainult osa või määrake heli ja video renderduste (asemel kõik renderduste, mis on seotud vara) oma kliendi seadme käsitlemiseks alamhulka. Selle oma varasid filtreerimine arhiivitud s **Dünaamiline näidata**kliendi taotluse alusel määratud filtrid video voogesituseks loodud kaudu.

Teemasid käsitletakse Levinumad stsenaariumid, kus filtrite abil oleks teie klientide ja lingid teemadele, mis näitavad, kuidas luua filtrid programmiliselt väga kasulik (praegu, saate luua filtrid REST API-de ainult).

##<a name="overview"></a>Ülevaade

Sisu esitamisel (streaming reaalajas sündmusi või video-on-demand) kliendid teie eesmärk on pakkuda kõrge kvaliteediga video erinevate seadmete erinevate võrgu tingimustel. Saavutamiseks selle eesmärk tehke järgmist:

- teie voo mitme bitikiirusega ([kohandatava bitrate](http://en.wikipedia.org/wiki/Adaptive_bitrate_streaming)) videovoo (see hoolitseb kvaliteedi-ja võrgu) Kui soovite kodeerida ja 
- Media Servicesi [Dünaamiline pakendit](media-services-dynamic-packaging-overview.md) abil dünaamiliselt uuesti pakkimine oma voo erinevaid protokolle (see hoolitseb streaming erinevates seadmetes). Media Services toetab kohaletoimetamise järgmised kohandatava bitrate voogesituse tehnoloogiaid: HTTP Live Streaming (HLS), sujuva voogesituse, MPEG MÕTTEKRIIPSU ja HDS (Adobe PrimeTime Access kes ainult) jaoks. 

###<a name="manifest-files"></a>Failide näidata 

Kui saate kodeerida vara kohandatava bitrate streaming, **näidata** (loend) fail on loodud (fail asub tekstipõhine või XML-põhine). **Näidata** fail sisaldab streaming metaandmete näiteks: saate jälitada tüüp (heli, video või teksti), jälgida nimi, algus ja lõpp, bitrate (omadused) jälituse tekst, esitluse aknas (libistades aken kindlaksmääratud kestusega), video kodek (FourCC). Ka käsib mängija tuua järgmine fragment, andes teavet järgmiste esitatava video osadega saadaval ja nende asukoht. Fragmendid (või segmente) on video sisu tegelik "osa".


Siin on näide manifestfailile: 

    
    <?xml version="1.0" encoding="UTF-8"?>  
    <SmoothStreamingMedia MajorVersion="2" MinorVersion="0" Duration="330187755" TimeScale="10000000">
    
    <StreamIndex Chunks="17" Type="video" Url="QualityLevels({bitrate})/Fragments(video={start time})" QualityLevels="8">
    <QualityLevel Index="0" Bitrate="5860941" FourCC="H264" MaxWidth="1920" MaxHeight="1080" CodecPrivateData="0000000167640028AC2CA501E0089F97015202020280000003008000001931300016E360000E4E1FF8C7076850A4580000000168E9093525" />
    <QualityLevel Index="1" Bitrate="4602724" FourCC="H264" MaxWidth="1920" MaxHeight="1080" CodecPrivateData="0000000167640028AC2CA501E0089F97015202020280000003008000001931100011EDC00002CD29FF8C7076850A45800000000168E9093525" />
    <QualityLevel Index="2" Bitrate="3319311" FourCC="H264" MaxWidth="1280" MaxHeight="720" CodecPrivateData="000000016764001FAC2CA5014016EC054808080A00000300020000030064C0800067C28000103667F8C7076850A4580000000168E9093525" />
    <QualityLevel Index="3" Bitrate="2195119" FourCC="H264" MaxWidth="960" MaxHeight="540" CodecPrivateData="000000016764001FAC2CA503C045FBC054808080A000000300200000064C1000044AA0000ABA9FE31C1DA14291600000000168E9093525" />
    <QualityLevel Index="4" Bitrate="1469881" FourCC="H264" MaxWidth="960" MaxHeight="540" CodecPrivateData="000000016764001FAC2CA503C045FBC054808080A000000300200000064C04000B71A0000E4E1FF8C7076850A4580000000168E9093525" />
    <QualityLevel Index="5" Bitrate="978815" FourCC="H264" MaxWidth="640" MaxHeight="360" CodecPrivateData="000000016764001EAC2CA50280BFE5C0548303032000000300200000064C08001E8480004C4B7F8C7076850A45800000000168E9093525" />
    <QualityLevel Index="6" Bitrate="638374" FourCC="H264" MaxWidth="640" MaxHeight="360" CodecPrivateData="000000016764001EAC2CA50280BFE5C0548303032000000300200000064C080013D60000C65DFE31C1DA1429160000000168E9093525" />
    <QualityLevel Index="7" Bitrate="388851" FourCC="H264" MaxWidth="320" MaxHeight="180" CodecPrivateData="000000016764000DAC2CA505067E7C054830303200000300020000030064C040030D40003D093F8C7076850A45800000000168E9093525" />
    
    <c t="0" d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="9600000"/>
    </StreamIndex>
    
    
    <StreamIndex Chunks="17" Type="audio" Url="QualityLevels({bitrate})/Fragments(AAC_und_ch2_128kbps={start time})" QualityLevels="1" Name="AAC_und_ch2_128kbps">
    <QualityLevel AudioTag="255" Index="0" BitsPerSample="16" Bitrate="125658" FourCC="AACL" CodecPrivateData="1210" Channels="2" PacketSize="4" SamplingRate="44100" />
    
    <c t="0" d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="6965987" /></StreamIndex>
    
    
    <StreamIndex Chunks="17" Type="audio" Url="QualityLevels({bitrate})/Fragments(AAC_und_ch2_56kbps={start time})" QualityLevels="1" Name="AAC_und_ch2_56kbps">
    <QualityLevel AudioTag="255" Index="0" BitsPerSample="16" Bitrate="53655" FourCC="AACL" CodecPrivateData="1210" Channels="2" PacketSize="4" SamplingRate="44100" />
    
    <c t="0" d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="6965987" /></StreamIndex>
    
    </SmoothStreamingMedia>
    
###<a name="dynamic-manifests"></a>Dünaamiliste manifestid

Kui teie klient vajab rohkem paindlikkust kui, mida on kirjeldatud vaikimisi varade manifestifaili on [stsenaariumid](media-services-dynamic-manifest-overview.md#scenarios) . Näiteks:

- Seadme teatud: esitamisel ainult määratud renderduste ja või määratud keele lugusid, mis ei toeta seade, mida on kasutatud taasesitus sisu ("üleviimise filtreerimine"). 
- Vähendage manifest sub klipi live sündmus ("sub klipi filtreerimine") kuvamiseks.
- Trim ("korrastamine video") video algust.
- Saate reguleerida esitluse aknas (DVR) pakkumiseks piiratud pikkus DVR akna pleieris ("seadistamine esitluse aken").
 
See paindlikkus saavutamiseks Media Servicesi pakub **Dünaamiline manifestid** põhjal eelmääratletud [filtrid](media-services-dynamic-manifest-overview.md#filters).  Kui määratlete filtrid, kliendid võivad kasutada neid voona teatud üleviimise või sub klippide video. Need soovite määrata filtrid streaming URL-is. Voo Protokollid [Dünaamiline pakendit](media-services-dynamic-packaging-overview.md)ei toeta kohandatava bitrate võib filtreid rakendada: HLS, MPEG-kriips, sujuva voogesituse ja HDS. Näiteks:

MPEG MÕTTEKRIIPSU URL-i filtreerimine

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf,filter=MyLocalFilter)

Sujuva voogesituse URL-i filtriga

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=MyLocalFilter)


Sisu esitamine ja koostada streaming URL-ide kohta leiate lisateavet teemast [toimetamine sisu ülevaade](media-services-deliver-content-overview.md).


>[AZURE.NOTE]Pange tähele, et dünaamiline manifestid pole vara ja vara vaikimisi manifest. Oma kliendi saate valida koos või ilma filtrite voo Taotle. 


###<a id="filters"></a>Filtrid 

On kahte tüüpi varade filtrid. 

- Globaalsest filtrite (saab kasutada mis tahes vara Azure Media Servicesi konto, on konto eluiga) ja 
- Kohaliku filtrid (saab rakendada ainult vara, millega on seotud pärast loomist filter, on vara kasutusaeg). 

Üld- ja filter on täpselt sama atribuudid. Kahe põhilise vahe on millised stsenaariumid mis tüüpi on filter on sobivam. Globaalsest filtrite sobivad üldiselt seadme profiilid (üleviimise filtreerimine) kui kohalik filtrid saaks kasutada trimmimine konkreetse vara.


##<a id="scenarios"></a>Levinumad stsenaariumid 

Nagu mainitud enne sisu esitamisel (streaming reaalajas sündmusi või video-on-demand) kliendid teie eesmärk on pakkuda kõrge kvaliteediga video erinevate seadmete erinevate võrgu tingimustel. Lisaks teie võivad olla muud nõuetele, mis hõlmavad filtreerimine oma varasid ja **Dünaamiline näidata**s abil. Järgmistes jaotistes anda erinevaid filtreerimise stsenaariumeid lähemalt.

- Määrake alamhulka heli ja video renderduste käsitlemiseks teatud seadmed (asemel kõik renderduste, mis on seotud vara). 
- Taasesitamisel ainult osa video (mitte tervet video esitamine).
- Saate reguleerida DVR esitluse aknas.

##<a name="rendition-filtering"></a>Pildirenderduse filtreerimine 

Võite valida mitu kodeering profiili oma vara kodeerida (H.264 võrdlusalus, H.264 kõrge, AACL, AACH, Dolby Digital Plus) ja mitme kvaliteedi bitikiirus. Siiski toetab kõiki kliendi seadmed teie vara profiilid ja bitikiirus. Näiteks vanemate Androidi seadmete toetab ainult H.264 võrdlusalus + AACL. Saatmine kõrgemate bitikiiruste seade, mis ei saa kasu, raiskab läbilaskevõime ja seadme arvutus. Selline seade peab dekodeerida kõik antud teabe, ainult mastaapimiseks kuvamiseks.

Dünaamiliste näidata, saate luua seadme profiilid mobile, nt konsooli, HD/SD jne ja sisaldavad jälitab ja omadusi, mida soovite iga profiili osa.

 
![Pildirenderduse filtreerimise näide][renditions2]

Järgmises näites kasutati kodeerija kodeerida Standard varade seitse ISO MP4s video renderduste (alates 180p 1080 p). Encoded varade saate mis tahes järgmised streaming protokollid dünaamiliselt pakitud: HLS, sujuv, MPEG MÕTTEKRIIPSU ja HDS.  Skeemi ülaosas on kuvatud HLS manifest varade pole filtritega (see sisaldab kõiki seitse renderduste).  Klõpsake vasakus allnurgas kuvatakse HLS manifesti, millele on rakendatud filter nimega "ott". "Ott" filtri määrab eemaldada kõik bitikiirus 1Mbps, mille tulemuseks all kaks kvaliteet on eemaldamisel vastuse all.  Akna paremas allservas kuvatakse HLS manifesti, millele on rakendatud filter nimega "mobile". "Mobile" filtri eemaldamiseks renderduste, kus eraldusvõime on suurem kui 720p, mille tulemuseks kaks määrab 1080p renderduste on eemaldamisel.

![Pildirenderduse filtreerimine][renditions1]

##<a name="removing-language-tracks"></a>Eemaldada keele lood

Oma varasid võivad heli mitmes keeles, nt inglise, Hispaania, prantsuse jne. Tavaliselt Playeri SDK haldurid vaikimisi heliriba valiku ja saadaval heli rajad kasutaja valiku kohta. Seda on raske arendada selliste Playeri SDK-d, see nõuab rakendatud erinevalt seadme mängija-raamistiku üle. Lisaks teatud platvormidel Playeri API-d on piiratud ja ei sisalda helivaliku funktsioon, kus kasutajad ei saa valida või muuta vaikimisi heliriba. Varade filtritega, saate määrata käitumise filtrid, mis sisaldavad ainult soovitud heli keelte loomisega.

![Keele lood filtreerimine][language_filter]


##<a name="trimming-start-of-an-asset"></a>Kärpimise start vara 

Enamik reaalajas streaming sündmusi, käivitage tehtemärkide mõned kontrollib enne sündmuse tegelikku. Näiteks, nad võivad mõnda sellist lehelt enne sündmuse algust: "Programmi hakkab hetkeks". Kui programm on arhiivimine, testimine ja kiltkivi andmete ka arhiivitakse ja kaasatakse esitlus. Siiski peaks seda teavet ei kuvata klientidele. Dünaamiliste näidata, kus saate luua filtri käivitamise aja ja soovimatute andmete eemaldamine manifest.

![Kärpimise käivitamine][trim_filter]

##<a name="creating-sub-clips-views-from-a-live-archive"></a>Reaalajas arhiivist sub klippide (kõik vaatamised) loomine

Mitme reaalajas sündmused on pikk töötab ja reaalajas arhiivi võivad olla mitu sündmused. Pärast live sündmus soovida lõpetatakse ringhäälinguorganisatsioonid lahku reaalajas arhiivi üheks loogilise programmi käivitamise ja lõpetamise järjestuse. Seejärel avaldada need virtuaalse programmid eraldi ilma postituse töötlemine reaalajas arhiivi ja ei looda (millele ei saa olemasolevaid vahemällu talletatud fragmendid kasu on CDN-ID). Selliste virtuaalse programmide (sub videoklipid) on jalgpalli või korvpalli, mänguvooru Baseball kvartalite või üksikute sündmuste pärastlõunal olümpiamängude programmi.

Dünaamiliste näidata, saate luua filtrite abil algus-ja lõppkellaaja ja ülaosas oma reaalajas arhiivi virtuaalse vaadete loomine. 

![Subclip filter][subclip_filter]

Filtreeritud vara:

![Suusatamine][skiing]

##<a name="adjusting-presentation-window-dvr"></a>Esitluse aknas (DVR) reguleerimine

Azure Media Servicesi pakub praegu, kui soovite kestuse saab konfigureerida 5 minutit – 25 tundi ringikujulise arhiivi. Manifest filtreerimine saab luua jooksva DVR akna ülemises arhiivi, ilma kustutamine. On palju stsenaariume, kus soovite ringhäälinguorganisatsioonid pakuvad piiratud DVR aken, mis viib reaalajas serv ja samal ajal hoidke suurem arhiveerimise aken. Mõne levitaja võiksite kasutada andmed, mis on välja DVR akna klippide esile tõsta või he\she võiksite pakkuda muu DVR Windowsi erinevate seadmete jaoks. Näiteks ära enamik mobiilsideseadmete käsitlemiseks suur DVR windows (võib olla mobiilsideseadmete jaoks ja 1 tund 2 minuti DVR akna töölauaklientide jaoks).

![DVR aken][dvr_filter]

##<a name="adjusting-livebackoff-live-position"></a>Reguleerimine LiveBackoff (reaalajas paan paigutus)

Manifest filtreerimine saab eemaldada mitu sekundit reaalajas programmi reaalajas servast. See võimaldab ringhäälinguorganisatsioonid vaadata esitluse eelvaade publikatsiooni punkti ja luua reklaami sisestamise punktid enne vaatajad voo (tavaliselt tagatud-off 30 sekundi) vastu võtta. Ringhäälinguorganisatsioonid seejärel push reklaamid oma kliendi raamistik nende ajal vastuvõetud ja enne reklaami võimaluse teabe töötlemine.

Lisaks reklaami tugi, saab LiveBackoff kohandamiseks kliendi reaalajas allalaadimine asukohta, et kui kliendid triivi ja reaalajas serva tabas nad saavad ikka fragmendid asemel 404 või 412 HTTP tõrketeateid serverist.

![livebackoff_filter][livebackoff_filter]


##<a name="combining-multiple-rules-in-a-single-filter"></a>Ühendab mitu reeglite ühe filtri

Saate ühendada mitu filtreerimise reeglite ühe filtri. Näiteks saate määratleda vahemiku reegli eemaldamine lehelt reaalajas arhiivi ja filtreerida ka saadaval bitikiirus. Seetõttu on mitu filtreerimise reeglid reegleid koosseis (ainult ühisosa).

![mitu-reeglid][multiple-rules]

##<a name="create-filters-programmatically"></a>Filtrite programmiliselt loomine

Järgmises teemas käsitletakse Media Servicesi üksused, mis on seotud filtrid. Teema kuvatakse ka programmiliselt filtrite loomise kohta.  

[Loo filtrid REST API-d](media-services-rest-dynamic-manifest.md).

## <a name="combining-multiple-filters-filter-composition"></a>Ühendab mitu filtrit (filter koosseis)

Samuti saate ühendada mitu filtrit ühe URL-is. 

Järgmistel näitab, miks võiksite ühendamiseks filtrid:

1. Peate oma video omadused, nt Android- või iPadi mobiilsideseadmete jaoks filtreerimiseks (video omadused piiramiseks). Et eemaldada soovimatu omadused, loote globaalne filter, mis sobib seadme profiilid. Eespool nimetatud globaalsest filtrite saab kasutada oma varasid sama media services konto ilma täiendavate seos. 
2. Mida soovite trimmimine vara aeg algus ja lõpp. Selleks oleks loomine kohaliku filtri ja määrake soovitud algus-ja lõppkellaaeg. 
3. Mida soovite ühendada nii need filtrid (ilma kombinatsioon, peate lisada kärpimise filter, mis teeb filtri kasutamine keeruline kvaliteedi filtreerimine).

Filtrite ühendamiseks peate määratud filter nimed manifesti/loend semikooloniga eraldatud URL. Oletame, et teil on filter nimega *MyMobileDevice* , et filtrite omadusi ja teil on mõne muu nimega *MyStartTime* kindla algusaja määramiseks. Saate need omavahel kombineerida umbes järgmine:

    http://teststreaming.streaming.mediaservices.windows.net/3d56a4d-b71d-489b-854f-1d67c0596966/64ff1f89-b430-43f8-87dd-56c87b7bd9e2.ism/Manifest(filter=MyMobileDevice;MyStartTime)

Saate ühendada kuni 3 filtrit. 

[Lisateabe saamiseks vt.](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/)


##<a name="know-issues-and-limitations"></a>Leida probleemide ja piirangutega

- Dünaamiliste manifesti toimib GOP piirmäärad (võtme raamid), seega korrastamise on GOP täpsuse. 
- Saate kasutada sama kohaliku ja globaalsest filtrite filtri nimi. Pange tähele kohaliku filtri on kõrgem ja alistab globaalsest filtrite.
- Kui värskendate filtri, võib kuluda kuni kaks minutit streaming lõpp-punkti värskendamiseks reegleid. Kui sisu kätte mõned filtrite abil (ja vahemällu vahemälu puhverserverite ja CDN), need filtrid värskendamine võib põhjustada mängija ebaõnnestumist. See on soovitatav pärast värskendamist filtri vahemälu tühjendamiseks. Kui see suvand pole võimalik, kaaluge erinevate filter.


##<a name="media-services-learning-paths"></a>Media Servicesi Õppeteed

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



##<a name="see-also"></a>Vt ka

[Videosisu kliendid ülevaade](media-services-deliver-content-overview.md)

[renditions1]: ./media/media-services-dynamic-manifest-overview/media-services-rendition-filter.png
[renditions2]: ./media/media-services-dynamic-manifest-overview/media-services-rendition-filter2.png

[rendered_subclip]: ./media/media-services-dynamic-manifests/media-services-rendered-subclip.png
[timeline_trim_event]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-event.png
[timeline_trim_subclip]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-subclip.png

[multiple-rules]:./media/media-services-dynamic-manifest-overview/media-services-multiple-rules-filters.png

[subclip_filter]: ./media/media-services-dynamic-manifest-overview/media-services-subclips-filter.png
[trim_event]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-event.png
[trim_subclip]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-subclip.png
[trim_filter]: ./media/media-services-dynamic-manifest-overview/media-services-trim-filter.png
[redered_subclip]: ./media/media-services-dynamic-manifests/media-services-rendered-subclip.png
[livebackoff_filter]: ./media/media-services-dynamic-manifest-overview/media-services-livebackoff-filter.png
[language_filter]: ./media/media-services-dynamic-manifest-overview/media-services-language-filter.png
[dvr_filter]: ./media/media-services-dynamic-manifest-overview/media-services-dvr-filter.png
[skiing]: ./media/media-services-dynamic-manifest-overview/media-services-skiing.png
 