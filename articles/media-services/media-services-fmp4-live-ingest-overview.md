<properties 
    pageTitle="Azure Media Servicesi killustatud MP4 live neelata määratlus | Microsoft Azure'i" 
    description="See määratlus kirjeldatakse Protocol (protokoll) ja killustatud MP4 vastavalt reaalajas streaming manustamisest Microsoft Azure Media Servicesi vorming. Microsoft Azure Media Servicesi pakub reaalajas streaming teenus, mis võimaldab klientidel voona reaalajas sündmused ja leviedastus sisu reaalajas kasutades Microsoft Azure'i platvormi pilve. Selles dokumendis käsitletakse ka kõige paremini tavade koostamise väga liigsete ja robustne live neelata menetlustele." 
    services="media-services" 
    documentationCenter="" 
    authors="cenkdin" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"     
    ms.author="cenkdin;juliako"/>

#<a name="azure-media-services-fragmented-mp4-live-ingest-specification"></a>Azure Media Servicesi killustatud MP4 live neelata määratlus

See määratlus kirjeldatakse Protocol (protokoll) ja killustatud MP4 vastavalt reaalajas streaming manustamisest Microsoft Azure Media Servicesi vorming. Microsoft Azure Media Servicesi pakub reaalajas streaming teenus, mis võimaldab klientidel voona reaalajas sündmused ja leviedastus sisu reaalajas kasutades Microsoft Azure'i platvormi pilve. Selle dokumendi käsitletakse ka head tavad koostamise väga liigsete ja robustne live neelata menetlustele.


##<a name="1-conformance-notation"></a>1. vastavuse märke

Võtmesõnad "peab", "Ei", "Nõutav", "Peab", "Ei tohi", "Peaks", "Ei peaks", "Soovitatav", "Võib" ja "Valikuline" selles dokumendis on tõlgendada, nagu on kirjeldatud RFC 2119.

##<a name="2-service-diagram"></a>2 teenuse diagramm 

Microsoft Azure Media Servicesi kuvatakse reaalajas streaming teenuse kõrge taseme arhitektuur alloleval joonisel:

1.  Reaalajas kodeerija sunnib reaalajas kanalite kanalit, mis on loodud ja ette valmistatud rakendusse Microsoft Azure Media Services SDK kaudu.
2.  Kanalite, programmide ja Streaming lõpp-punkti rakenduses Microsoft Azure Media Servicesi pidet kõik reaalajas neelata, vorming, sh streaming funktsioonid cloud DVR, turvalisus, skaleeritavus ja koondamise.
3.  Soovi korral võib kliendid valida CDN kiht Streaming lõpp-punkti ja kliendi lõpp-punktid.
4.  Kliendi lõpp-punktid voo voo lõpp-punkti abil HTTP kohandatav voogesitus Protokollid (nt sujuva voogesituse, MÕTTEKRIIPSU, HDS või HLS) kaudu.

![image1][image1]


##<a name="3-bit-stream-format--iso-14496-12-fragmented-mp4"></a>3. bitine voo vorming – ISO 14496-12 killustatud MP4

Kaabel vormingu live streaming neelata kirjeldatud selles dokumendis on aluseks [ISO-14496-12]. Vaadake [[MS-SSTR]](http://msdn.microsoft.com/library/ff469518.aspx) üksikasjaliku selgituse killustatud MP4-vormingus ja laiendid mõlemad video-on-demand failid ja live streaming manustamisest.

###<a name="live-ingest-format-definitions"></a>Live neelata vorming määratlused 

Järgnevalt on toodud erivormingu määratlused, mis rakenduvad live loendi neelata rakendusse Microsoft Azure Media Servicesi:

1. 'Ftyp', LiveServerManifestBox ja 'moov' väljale tuleb saata iga taotlusega (HTTP POST).  See tuleb saata voo alguses ja iga kord kodeerija peab uuesti jätkamiseks voo neelata.  Lugege jaotise 6 [1] üksikasjalikumat teavet.
2. [1] jaotist 3.3.2 määratleb on valikuline väljale nimega StreamManifestBox jaoks reaalajas neelata. Microsoft Azure'i laadi koormusetasakaalustusteenuse marsruutimine loogika, kuna kasutamist see ruut on aegunud ja peaks ei saa teenusesse Microsoft Azure Media söömisega kohal. Kui see ruut on olemas, Azure Media Servicesi vaikselt ignoreerib seda.
3. TrackFragmentExtendedHeaderBox, mis on määratletud 3.2.3.2 [1] peab olema iga fragment jaoks.
4. Versiooni 2 TrackFragmentExtendedHeaderBox tuleks kasutada mitme andmekeskuste meediumi segmente koos identse URL-ide loomiseks. Fragmendid väli index on vaja rist-andmekeskuse Tõrkesiirde vastavalt streaming vormingud, nt Apple HTTP Live Streaming (HLS) ja vastavalt MPEG-kriips.  Rist-andmekeskuse Tõrkesiirde lubamiseks fragment registri tuleb sünkroonida mitmesse kodeerimisseadmetest ja suurendamine 1 jaoks iga järjestikuste meediumi fragmendid, isegi mitmest kodeerija taaskäivitamisel või tõrkeid.
5. [1] jaotist 3.3.6 määratleb lahtrisse nimega MovieFragmentRandomAccessBox ('mfra), mis saadetakse reaalajas manustamisest lõpus töökaaslastele EOS (Lõpeta, voo) kanal. Tõttu neelata loogika Azure Media Servicesi, EOS (Lõpeta, voo) kasutamist on aegunud ja välja "mfra" kohta reaalajas peaks ei saadeta. Kui saadetud, Azure Media Servicesi vaikselt ignoreerib seda. Soovitatav on kasutada [Kanali lähtestamine](https://msdn.microsoft.com/library/azure/dn783458.aspx#reset_channels) Lähtesta olek neelata punkti ja ka on soovitatav kasutada esitluse lõpetamiseks voo [Programmi lõpetada](https://msdn.microsoft.com/library/azure/dn783463.aspx#stop_programs) .
6. MP4 fragment kestus peaks olema pidev kliendi manifestid mahu vähendamiseks ja parandada kliendi allalaadimine heuristika korrake siltide abil.  Kestus võib selleks, et kompenseeri mittetäisarvuliste paneeli määr müügitehingu muutuda.
7. MP4 fragment kestus peaks olema umbes 2 ja 6 sekundit.
8. MP4 fragment ajatemplid ja registrite (TrackFragmentExtendedHeaderBox fragment_ absolute_ aja ja fragment_index) kohale kasvavas järjestuses.  Kuigi Azure Media Servicesi on olles dubleeritud fragmendid, on väga piiratud võimalus fragmendid vastavalt meediumi ajaskaala järjestust.

##<a name="4-protocol-format--http"></a>4. protokoll vorming – http kaudu

ISO killustatud MP4 vastavalt live neelata standard pikaajalise HTTP POST taotlemine encoded meediumi andmed pakitud killustatud MP4-vormingus teenusega Microsoft Azure Media Servicesi kasutuseks. Iga HTTP POST saadab on lõpule viidud killustatud MP4 kiire ("voo") alates alustades päiseväli ("ftyp", "Live Server näidata kasti" ja 'moov' kast) ja jätkates jada fragmendid (väljad "moof" ja "mdat"). Vaadake jaotist 9,2 tolli [1] jaoks URL-i süntaksi HTTP POST taotlus. POSTITUSE URL-i näide on järgmine: 

    http://customer.channel.mediaservices.windows.net/ingest.isml/streams(720p)

###<a name="requirements"></a>Nõuded

Siin on üksikasjalikke nõudeid.

1. Kodeerija peab algama leviedastuse saatmisega HTTP POST-taotluse on tühi "tekst" (sisu nullpikkusega) manustamisest sama URL-i abil. See aitab kiiresti tuvastada, kui reaalajas manustamisest lõpp-punkti on lubatud ja kui mis tahes autentimist või muid tingimusi, mis on nõutav. HTTP-protokolli kohta server ei saa tagasi HTTP vastuse saata, kuni kogu taotluse, sh POST keha. Arvestades pikaajalise live sündmus, ilma seda toimingut kodeerija ei saa tuvastada viga, kuni see lõpetab kõik andmed saata.
2. Kodeerija tuleb toime vigu või autentimine toimetulekuks tulemusena (1). Kui (1) õnnestus 200 vastusega jätkata.
3. Kodeerija peab algama killustatud MP4 voo HTTP POSTITA Uus koosolekukutse.  Funktsiooni last peab algama fragmendid, millele järgneb päise väljadele.  Märkus (selles järjestuses) "ftyp", "Live Server näidata kasti" ja 'moov' välja saata iga taotlusega isegi juhul, kui kodeerija peab taastada, kuna eelmise taotluse on peatatud enne selle voo. 
4. Kodeerija peate kasutama Chunked edastamine kodeerimine üles, kuna see on võimalik prognoosida kogu sisu pikkus live sündmus.
5. Kui sündmus on lõppenud, pärast saatmist viimase fragment lõpus kodeerija peab olema nõtkelt Chunked edastamine kodeerimine sõnumite järjestuse (enamik HTTP kliendi virnadena hakkama automaatselt). Kodeerija peate ootama teenuse, et tagastada lõpliku vastuse koodi ja seejärel klõpsake ühenduse lõpetada. 
6. Kodeerija ei tohi kasutada Events() nimisõna, nagu on kirjeldatud kohta reaalajas rakendusse Microsoft Azure Media Servicesi 9,2 in [1].
7. Kui HTTP POST taotluse lõpeb või enne voo TCP-tõrge ilmneb ajalõpp, kodeerija uue postituse taotlus abil uus ühendus ja järgige ülal esitatud nõuetele täiendavad nõue, et kodeerija peab uuesti muusikapalade voos eelmise kahe MP4 fragmendid ja jätkamine ilma arvutamismeetodi meediumi ajaskaala tutvustus.  Uuestisaatmine kaks viimast MP4 fragmendid jaoks muusikapalade tagab on andmete kaotamata.  Teisisõnu, kui voo sisaldab nii heli- ja jälitamine ja praeguse postituse taotlus nurjub, peab kodeerija uuesti ja uuesti teile kaks viimast fragmendid jaoks, mis olid varem saadetud ja kaks viimast fragmendid video tee, mis on varem edukalt saadetud, selleks, et tagada, et andmeid kaotamata.  Kodeerija tuleb säilitada "edasi" puhvri meediumi fragmendid, mis saadab uuesti, kui taastamine.

##<a name="5-timescale"></a>5. ajaskaala 

[[MS-SSTR]](https://msdn.microsoft.com/library/ff469518.aspx) kirjeldatakse "Ajaskaala" kasutamist SmoothStreamingMedia (jaotis 2.2.2.1), StreamElement (jaotis 2.2.2.3), StreamFragmentElement(2.2.2.6) ja LiveSMIL (jaotis 2.2.7.3.1). Kui ajaskaala väärtus puudub, kasutatakse vaikeväärtust, milleks on 10.000.000 (10 MHz). Kuigi sujuva voogesituse vormingu täpsustus ei blokeerida kasutamist muude ajaskaala väärtuste enamik kodeerija rakendusi kasutab seda vaikeväärtust (10 MHz) luua sujuva voogesituse neelata andmed. Tõttu [Azure Media dünaamiline pakendit](media-services-dynamic-packaging-overview.md) funktsioon, mis on 90 kHz ajakava videovoo ja 44,1 või 48.1 kHz heli voogu kasutamiseks soovitame. Kui ajakava erinevate väärtuste kasutatakse erinevate voole, voo taseme ajaskaala saata. Vaadake [[MS-SSTR]](https://msdn.microsoft.com/library/ff469518.aspx).     

##<a name="6-definition-of-stream"></a>6. "Voo" määratlus  

Toimingu reaalajas manustamisest koostamise esitlusel, streaming Tõrkesiirde ja koondamise stsenaariumid põhiühik on "Voo". "Voo" on määratletud ühe kordumatu killustatud MP4 bitine-voo mis võib sisaldada ühe muusikapala või mitme jälitab. Täielik esitlusel võib sisaldada ühe või mitme voogu reaalajas encoder(s) konfiguratsioonist. Allpool toodud näidetest stream(s) abil saate koostada täielikku esitlusel erinevaid võimalusi.

**Näide:** 

Klient soovib reaalajas streaming esitluse, mis sisaldab järgmist heli-ja videokõnede bitikiirus loomiseks:

Video – 3000kbps, 1500kbps, 750kbps

Heli – 128kbps

###<a name="option-1-all-tracks-in-one-stream"></a>Variant 1: Kõik lood ühe voo

See suvand ühe kodeerija genereeritud kõik heli-ja videokõnede lood ja need üheks killustatud MP4 bitine – mis ühe postituse HTTP-ühenduse kaudu, siis saadetakse esitlusega sidumiseks. Selles näites on ainult üks voo selle reaalajas esitluse:

![image2][image2]

###<a name="option-2-each-track-in-a-separate-stream"></a>Variant 2: Muusikapalade eraldi voo

Selle suvandi encoder(s), panna ainult jälgida iga Fragment MP4 bitine voo sisse ja pärast kogu välja üle mitme eraldi HTTP ühendused. Seda saab teha kodeerimisseadmetest ühe või mitme kodeerimisseadmetest. Reaalajas manustamisest seisukohast koosneb selle esitlusel neli voogu.

![image3][image3]

###<a name="option-3-bundle-audio-track-with-the-lowest-bitrate-video-track-into-one-stream"></a>Võimalus 3: Kogumi heliriba teega madalaimate bitrate video üheks

See suvand, valib kliendi heliriba teega madalaimate bitrate video üheks Fragment MP4 bitine-ja jätke muude kaks video lugusid iga on eraldi voo. 


![image4][image4]


###<a name="summary"></a>Kokkuvõte

Mis on eespool näidatud pole täielik loetelu kõigi võimalike manustamisest suvandite selles näites. Tegelikult, mis tahes rühmitamise lood voogu toetab reaalajas manustamisest. Kliendid ja dekodeerimise müüjad, saate valida oma rakendusi engineering keerukuse, kodeerija, koondamise ja ja Tõrkesiirde kaalutluste põhjal. Siiski tuleb märkida, enamasti on ainult üks heliriba live kogu esitluse nii, et see on oluline tagada neelata voo, mis sisaldab heliriba tervislikkuse. See tasu sageli, on tulemuseks heliriba kasutusele oma voo (nagu variant 2) või ühendamisega selle teega madalaimate bitrate video (nagu suvand 3). Ka paremaks koondamise ja tõrketaluvust saata sama heli jälgida kahe eri voole (variant 2 abil liigsete audio rajad) või ühendamisega madalam bitrate video triipudega (heliga on vähemalt kaks videovoo kombineeritud suvand 3) vähemalt kaks heliriba on soovitatav live neelata rakendusse Microsoft Azure Media Servicesi.

##<a name="7-service-failover"></a>7. Tõrkesiirde teenuse 

Laadi reaalajas streaming, arvestades tagada teenuse kättesaadavus äärmiselt oluline on hea Tõrkesiirde tugi. Microsoft Azure Media Servicesi on kavandatud kasutama erinevat tüüpi tõrkeid, sh võrgu tõrkeid, serveri tõrkeid, salvestusruumi probleeme jne. Kui kasutada koos proper Tõrkesiirde loogika reaalajas kodeerija poolt, klientide võite saavutada väga usaldusväärne reaalajas streaming teenuse pilvest.

Selles jaotises me arutada teenuse Tõrkesiirde stsenaariumid. Sel juhul selle juhtub kuskil teenuses ja väljendub nimega võrgu tõrge. Siin on mõned soovitused käsitlemise teenuse Tõrkesiirde rakendamiseks kodeerija.


1. 10 teine ajalõpp TCP-ühenduse loomiseks kasutada.  Kui ühenduse loomise katse kulub üle 10 sekundi, tegevuse katkestamiseks ja proovige uuesti. 
2. Kasutage lühike ajalõpp HTTP taotluse saatnud sõnumi tükkideks.  Kui target MP4 fragment kestab N sekundi, kasutage saada ajalõpp vahel N ja 2 n sekundit; näiteks kasutada ajalõpp 6 – 12 sekundit, kui MP4 fragment kestab 6 sekundit.  Kui ilmneb ajalõpp, lähtestamiseks ühendust, avage uus ühendus ja jätkamine voo neelata uus ühendus. 
3. Jooksva puhvri, milles on kaks viimast, jaoks muusikapalade, mis on edukalt ja täielikult saadud teenuse säilitada.  Kui HTTP POST taotlus voo lõpetatakse või enne ajalõpp lõppu voo, avada uue ühenduse alustage teise HTTP POST taotlus, uuesti voo päised, uuesti kaks viimast fragmendid jaoks iga jälitamine ja jätkamine voo ilma tutvustus on katkeda meediumi ajaskaala.  See vähendada andmete kaotsimineku võimalust.
4. Soovitatav on kodeerija arvu piirata ühendust luua või jätkamine streaming TCP tõrke ilmnemisel korduste arv.
5. Pärast TCP tõrge:
    1. Praeguse ühenduse tuleb sulgeda ja Uus koosolekukutse HTTP POST tuleb luua uus ühendus.
    2. Uue HTTP postituse URL-i peab olema sama mis algse postituse URL-i.
    3. Uue HTTP postituse peab sisaldama voo päised ("ftyp", "Live Server näidata kasti" ja 'moov' kast) identsed voo päiste algse postituse.
    4. Kaks viimast fragmendid, mis on saadetud muusikapalade peab aga uuesti saata, ja streaming jätkata ilma tutvustus on katkeda meediumi ajaskaala.  MP4 fragment ajatemplid peab suurendada pidevalt, isegi mitmest HTTP POST taotlused.
6. Kodeerija peaks HTTP POST taotluse lõpetada, kui andmed on saadetud pole vastavuses MP4 fragment kestus määra.  HTTP POST taotluse, et saata andmed saate takistada Azure Media Servicesi kiiresti katkestamine kodeerija korral teenuse update.  Seetõttu HTTP postituse jaoks vähe (ad signaal) lood peaks olema lühike elas, lõpetatakse kohe, kui vähe fragment saadetakse.

##<a name="8-encoder-failover"></a>8. Tõrkesiirde kodeerija

Kodeerija Tõrkesiirde on teist tüüpi Tõrkesiirde stsenaarium, mis peaks käsitlema lõpuni reaalajas streaming kohaletoimetamiseks. Selle stsenaariumi korral kuvatakse tõrketeade, mis juhtus kodeerija küljel. 

![image5][image5]


Allpool on reaalajas manustamisest lõpp ootustele, kui kodeerija Tõrkesiirde juhtub:

1. Uue kodeerija eksemplari luuakse jätkamiseks voogesitus, nagu on näidatud ülaltoodud joonisel (voo 3000 k kriipsjoontega video).
2. Uue kodeerija tuleb kasutada sama URL-i HTTP POST taotluste nurjunud eksemplari.
3. Uue kodeerija postituse taotlus peab sisaldama sama killustatud MP4 päise ruudud vastavalt nurjunud eksemplar.
4. Uue kodeerija peab olema õigesti sünkroonitud koos kõigi muude töötava kodeerimisseadmetest sama reaalajas esitluse loomiseks sünkroonitud heli-ja videokõnede näidised joondatud fragment piirid.
5. Uue voo peab olema semantiliselt võrdväärse koos eelmise voo ja vahetatavad päise- ja fragment tasemel.
6. Uue kodeerija peaksite proovima andmekao minimeerimiseks.  Fragment_absolute_time ja fragment_index meediumi fragmendid tuleks suurendada punktist, kus kodeerija viimati peatatud.  Pidev mood tuleks suurendada fragment_absolute_time ja fragment_index, kuid on lubatud võtta kasutusele mõne katkeda vajaduse korral.  Azure Media Servicesi ignoreerivad fragmendid, mis on juba saanud ja töödeldav, mistõttu on parem eelkõige silmas pidada uuestisaatmine fragmendid, kui soovite tutvustamine arvutamismeetodi meediumi ajaskaala. 

##<a name="9-encoder-redundancy"></a>9. koondamise kodeerija 

Teatud kriitiliste live sündmusi, et nõudmisel isegi suurem kättesaadavus ja kogemusi, on soovitatav tööle aktiivne aktiivne liigsete kodeerimisseadmetest saavutamiseks sujuvalt Tõrkesiirde kaotamata andmeid.

![image6][image6]

Ülaltoodud joonisel näidatud on kaks rühma kodeerimisseadmetest lükkamine kaks eksemplari iga voo korraga reaalajas teenusesse. See häälestus on toetatud, kuna Microsoft Azure Media Servicesi on võimalus dubleeritud fragmendid voo ID-d ja fragment ajatempli põhjal filtreerida. Tulemuseks reaalajas voo ja arhiivi saab ühe eksemplari kogu välja, mis on parim võimalik liitmine kahe allikatest. Näiteks hüpoteetilist äärmiselt juhul nii kaua, kui ühe kodeerija (ei pea olema sama ühe) töötab mis tahes ajaks iga voo tulemuseks reaalajas voo teenuse on pidev, ilma andmete kaotsimineku. 

Selle stsenaariumi nõue on peaaegu sama kodeerija Tõrkesiirde puhul, välja arvatud kodeerimisseadmetest teine rühm töötab samal ajal kui esmane kodeerimisseadmetest nõuetele.

##<a name="10-service-redundancy"></a>10. teenuse koondamine  

Väga liigsete levitamist, see on vahel peab olema rist-regiooni varundamise käsitlema piirkondliku katastroofi. Laiendatakse "Kodeerija koondamise" topoloogia, kliendid saate valida on üleliigsed teenuste juurutamine teises regioonis, mis on seotud kodeerimisseadmetest 2 kogumi. Kliendid võivad töötada ka CDN pakkuja lisamine GTM (globaalne liikluse Manager) juurutamiseks ette sujuv marsruutida liikluse kliendi kahe teenuse juurutuste. Koodritega nõuded on sama, mis "Kodeerija koondamise" puhul, mis kodeerimisseadmetest teine komplekt, mis on vaja mõnda muusse tuua, live erandiks neelata lõpp-punkt. Alloleval joonisel on kuvatud selle häälestus.

![image7][image7]

##<a name="11-special-types-of-ingestion-formats"></a>11. teisiti tüüpi manustamisest vormingud 

Selles jaotises kirjeldatakse mõned eri tüüpi reaalajas manustamisest vormingud, mis on loodud tegelema mõne konkreetse stsenaariumi.

###<a name="sparse-track"></a>Vähe jälitamine

Rikkaliku kliendi kogemusega streaming esitlusel esitamisel on sageli vaja edastada aja sünkroonitud sündmuste või signaale riba sisse peamisi meediumi andmetega. Üks näide on dünaamiline reaalajas reklaamid järjepunkti. Seda tüüpi sündmuse-signaal erineb tavalise heli-ja videokõnede streaming selle vähe laadist. Teisisõnu, signalisatsiooni andmed tavaliselt ei juhtu pidevalt ja intervalli võib olla keeruline prognoosida. Vähe Jälita mõiste oli spetsiaalselt neelata ja leviedastus-riba signalisatsiooni andmed.

Allpool on soovitatav rakendamiseks söömisega vähe jälitus.

1. Luua eraldi killustatud MP4 bitine-videovoo mis sisaldab ainult vähe rööbe ilma heli-ja videokõnede jälitab.
2. Väljale"Live Server näidata" jaotis 6 määratletud [1], "parentTrackName" parameetri abil saate määrata ema Jälita nime. Lugege lisateavet jaotise 4.2.1.2.1.2 [1].
3. Väljale"Live Server näidata" manifestOutput peab olema seatud "TRUE".
4. Arvestades vähe signalisatsiooni sündmus, on soovitatav:
    1. Reaalajas sündmuse alguses kodeerija saadab algse päiseväli teenus, mis võimaldaks registreerida vähe Jälita manifestis kliendi teenuse.
    2. Kodeerija peaks HTTP POST taotluse lõpetada, kui andmed on ei saadeta.  Pikaajalise HTTP-POST, et saata andmed saate takistada Azure Media Servicesi kiiresti katkestamine kodeerija korral teenuste värskendamine või server uuesti, nagu media server on ajutiselt blokeeritud vastuvõtu kasutusel olevasse kohta. 
    3. Ajal, kui signalisatsiooni andmed pole saadaval, kodeerija peaks Sule HTTP postituse koosolekukutse.  POSTITUSE kutse on aktiivne, kodeerija peaks andmete saatmine 
    4. Vähe fragmendid saatmisel kodeerija saate seada konkreetsete sisu pikkus päis, kui see on saadaval.
    5. Vähe fragment koos uue ühenduse saatmisel kodeerija peab algama uus fragmendid, millele järgneb päise väljadele saatmine. See on juhul kui Tõrkesiirde juhtunud vahel ja uus vähe ühendus on loodud uue server, mis on näha vähe Jälita enne käsitlema.
    6. Vähe Jälita fragment tehakse kättesaadavaks kliendile kui vastavate ema jälgida fragmente, mis on võrdne või suurem ajatempli väärtus on kättesaadav kliendile. Näiteks kui vähe fragment on ajatempel t = 1000, on tõenäoline pärast kliendi näeb video (eeldades ema Jälita nime on video) fragment ajatempli 1000 või kaugemale, saate alla laadida vähe fragment t = 1000. Pange tähele, et tegelik signaal võib hästi kasutada erinevate paigutus ajaskaala esitluse jaoks sihtotstarbelise. Eeltoodud näites on võimalik, et vähe fragment t = 1000 on XML-i last, mis on reklaami lisamiseks, mis on mõne hetke hiljem.
    7. Last vähe Jälita fragment võib olla erinevad eri vormingutes (nt XML-i või teksti või kahendarvuks jne) sõltuvalt erinevaid stsenaariumeid lähemalt. 


###<a name="redundant-audio-track"></a>Liigsete heliriba

Tüüpilised HTTP kohandatav voogesitus stsenaariumi (nt sujuva voogesituse või MÕTTEKRIIPSU) on sageli ainult üks heliriba kogu esitluses. Erinevalt video lugusid, mis on mitu kvaliteet tõrke tingimustega valida klient, saab heliriba ühtne tõrge, kui voo, mis sisaldab heliriba manustamisest on katkenud. 

Probleemi lahendamiseks Microsoft Azure Media Servicesi toetab reaalajas manustamisest liigsete audio rajad. Mõte on, et sama heliriba saab saata mitu korda erinevate voogu. Kuigi teenuse saavad registreeruda ainult heliriba üks kord kliendi manifestis, on võimalus kasutada liigsete audio rajad varukoopiate toomine heli fragmendid, kui esmane heliriba on probleeme. Selleks, et neelata liigsete heli lugusid, peab kodeerija.

1. Luua sama heliriba mitme Fragment MP4 bitine-voogu. Liigsete audio rajad peavad olema semantiliselt võrdväärse koos täpselt sama fragment ajatemplid ja vahetatavad päise- ja fragment tasemel.
2. Veenduge, et "audio" kirje on Live Server näidata (jaotis 6 [1]) olema sama kõik liigsed heli rajad.

Allpool on soovitatav rakendamiseks liigsete audio rajad:

1. Saada iga kordumatu heliriba voo ise.  Lisaks saata liigsete voo iga nende heliriba voole, kui 2 voo erineb vaid identifikaator HTTP postituse URL-is 1.: {protokoll} :// {serveri aadress} / {avaldamise punkti path}/Streams({identifier}).
2. Kasutage eraldi voogu kahe alumise video bitikiirus saatmiseks. Kõik need voole peaks sisaldama ka iga kordumatu heliriba koopia.  Näiteks kui mitmes keeles on toetatud, nende voole peaks sisaldama audio rajad iga keele jaoks.
3. Eraldi serveri (kodeerija) eksemplarides abil kodeerida ja saata liigsete voole, mainitud (1) ja (2). 



##<a name="media-services-learning-paths"></a>Media Servicesi Õppeteed

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


[image1]: ./media/media-services-fmp4-live-ingest-overview/media-services-image1.png
[image2]: ./media/media-services-fmp4-live-ingest-overview/media-services-image2.png
[image3]: ./media/media-services-fmp4-live-ingest-overview/media-services-image3.png
[image4]: ./media/media-services-fmp4-live-ingest-overview/media-services-image4.png
[image5]: ./media/media-services-fmp4-live-ingest-overview/media-services-image5.png
[image6]: ./media/media-services-fmp4-live-ingest-overview/media-services-image6.png
[image7]: ./media/media-services-fmp4-live-ingest-overview/media-services-image7.png

 