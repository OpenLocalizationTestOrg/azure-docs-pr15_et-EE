<properties 
    pageTitle="Avanced Media kodeerija Premium töövoo õppematerjalid" 
    description="See dokument sisaldab juhendavad tutvustused, mis näitab, kuidas täpsemalt Media kodeerija Premium töövoo ülesannete täitmiseks ja ka kuidas luua keerukate töövoogude töövoodisaineris." 
    services="media-services" 
    documentationCenter="" 
    authors="xstof" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/03/2016"  
    ms.author="xstof;xpouyat;juliako"/>

#<a name="advanced-media-encoder-premium-workflow-tutorials"></a>Täiustatud Media kodeerija Premium töövoo õppetükid

##<a name="overview"></a>Ülevaade 

See dokument sisaldab juhendavad tutvustused, mis näitab, kuidas kohandada töövooge, **Töövoodisaineris**. Saate otsida faile tegelik töövoo [siin](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/PremiumEncoderWorkflowSamples).  

##<a name="toc"></a>SISUKORD

Käsitletakse järgmisi teemasid:

- [Ühe bitrate MP4 MXF kodeerimine](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4)
    - [Uue töövoo käivitamine](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_start_new) 
    - [Kasutage Media fail sisestusviisi](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_file_input)
    - [Kontrollimise voogu](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_streams)
    - [Lisades video kodeerija jaoks. MP4 faili loomisel](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_file_generation)
    - [Heli voo kodeerimine](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_audio)
    - [Heli ja Video multipleksimise arvesse ka MP4 container](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_audio_and_fideo)
    - [MP4 faili kirjutamisel](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_writing_mp4)
    - [Väljundfail Media Services varade loomine](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_asset_from_output)
    - [Kohalik lõpetanud töövoo testimiseks](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_test)
- [Kodeeritud MXF multibitrate MP4s - dünaamiline pakendit lubatud](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging)
    - [Ühe või mitme täiendava MP4 väljundeid lisamine](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_more_outputs)
    - [Väljundi failinimed konfigureerimine](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_conf_output_names)
    - [Eraldi heli jälituse lisamine](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_audio_tracks)
    - [Lisamise funktsiooni. ISM SMIL fail](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_ism_file)
- [Kodeeritud MXF multibitrate MP4 - täiustatud näidis](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4)
    - [Töövoo ülevaade täiustamiseks](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_overview)
    - [Failinimede](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_file_naming)
    - [Avaldamise peale töövoo juurkausta komponendi atribuudid](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_publishing)
    - [On loodud nimed toetuvad avaldatud kinnisvarahindade väljundfail](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_output_files)
- [Multibitrate MP4 väljundi pisipildid lisamine](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4)
    - [Pildil, et lisada töövoo ülevaade](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to_multibitrate_MP4_overview)
    - [Lisada JPG kodeerimine](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4__with_jpg)
    - [Värvi ruumi teisendamine tegelemine](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_color_space)
    - [Pisipildi kirjutamine](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_writing_thumbnails)
    - [Töövoo vigade tuvastamiseks](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_errors)
    - [Lõpetatud töövoog](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_finish)
- [Multibitrate MP4 väljundi ajapõhiste korrastamine](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim)
    - [Töövoo ülevaade alustada lisamist kärpimise abil](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_start)
    - [Voo Trimmer abil](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_use_stream_trimmer)
    - [Lõpetatud töövoog](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_finish)
- [Skriptitud komponent tutvustus](media-services-media-encoder-premium-workflow-tutorials.md#scripting)
    - [Töövoo sees skriptimise: Tere, maailm](media-services-media-encoder-premium-workflow-tutorials.md#scripting_hello_world)
- [Multibitrate MP4 väljundi paneeli vastavalt korrastamine](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim)
    - [Näidis ülevaade alustada lisamist kärpimise abil](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_start)
    - [Lõikepildi loendi XML-i abil](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_clip_list)
    - [Lõikepildi skriptitud komponent loendi muutmine](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_modify_clip_list)
    - [Atribuut ClippingEnabled mugavuse lisamine](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_clippingenabled_prop)

##<a id="MXF_to_MP4"></a>Ühe bitrate MP4 MXF kodeerimine
 
Ülevaate loome ühe bitrate. MP4 faili AAC-HE kodeeritud heli on. MXF Sisestuskeel fail. 

###<a id="MXF_to_MP4_start_new"></a>Uue töövoo käivitamine 

Avage töövoo disaineri ja valige "Fail"-"uue tööruumi"-"Transcode näidis" 

Uue töövoo kuvatakse 3 elemendid: 

- Esmane lähtefail
- Lõikepildi loendi XML-i
- Väljundi faili/vara  

![Uue kodeering töövoo](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-transcode-blueprint.png)

*Uue kodeering töövoo*

###<a id="MXF_to_MP4_with_file_input"></a>Kasutage Media fail sisestusviisi

Selleks, et aktsepteerida meie Sisestuskeel meediumifail, üks algab lisamine Media fail Sisestuskeel komponent. Soovite töövoo lisada komponent, otsige see hoidla otsinguväljale ja lohistage kujundaja paanil soovitud kirje. Tehke seda meediumi Sisestuskeel ja komponendi esmane lähtefail ühenduse Filename Sisestuskeel PIN-koodi kaudu meediumi Sisestuskeel.

![Ühendatud Media fail sisendit](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-file-input.png)

*Ühendatud Media fail sisendit*

Enne, kui palju muud teha vajame esmalt töökaaslastele töövoo disaineri mis näidisfaili soovime kujundamiseks meie töövoogu kasutada. Selleks klõpsake kujundaja paanil tausta ja otsige parempoolse atribuudipaani atribuudi esmane lähtefail. Valige soovitud fail nimega töövoog testimiseks klõpsake kaustaikooni. Niipea, kui see on lõpule jõudnud, Media fail sisendit komponent kontrolli faili ja selle väljundi sõrmed kajastamiseks see kontrolli faili asustada.

![Täidetud Media fail sisendit](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-populated-media-file-input.png)

*Täidetud Media fail sisendit*

Kuigi seda määrab mis sisend soovime töötada, siis ei öelda, ent kui encoded väljundi pöörduma. Sarnaselt kuidas esmane lähtefail on konfigureeritud, nüüd konfigureerida väljund kausta muutuv, vaid selle all.

![Konfigureeritud sisestus- ja väljundi atribuudid](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-configured-io-properties.png)

*Konfigureeritud sisestus- ja väljundi atribuudid*

###<a id="MXF_to_MP4_streams"></a>Kontrollimise voogu

Sageli on soovitud teada, kuidas voo tundub, et puhul töövoo kaudu. Voo töövoo igal ajal kontrollida, klõpsake lihtsalt väljundi või Sisestuskeel PIN-koodi, mis tahes komponendid. Sel juhul proovige, klõpsates tihendamata Video väljund PIN-koodi kaudu panuse Media fail. Dialoogiboksi avab võimaldava kontrollimiseks väljamineva video.

![Kontrollimise tihendamata Video väljund PIN-kood](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-inspecting-uncompressed-video-output.png)

*Kontrollimise tihendamata Video väljund PIN-kood*

Meie puhul see ütleb meile, näiteks, et tegemist on 1920 x 1080 sisendi juures 24 raamid sekundis 4:2:2 valimite video peaaegu kaks minutit.

###<a id="MXF_to_MP4_file_generation"></a>Lisades video kodeerija jaoks. MP4 faili loomisel

Pange tähele, et nüüd tihendamata Video ja mitme tihendamata heli väljundi sõrmed jaoks on saadaval, kasutage meie Media fail sisestatud. Selleks, et kodeerida sissetuleva video, peame kodeering asetamist – sel juhul loomiseks. MP4 faile.

H.264 videovoo kodeerida lisada kujundaja pind AVC Video kodeerija komponent. Selle komponendi võtab pakkida videovoo sisendina ja pakub AVC tihendatud video voogu klõpsake selle väljundi PIN-koodi.

![Ühendatud AVC kodeerija](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-unconnected-avc-encoder.png)

*Ühendatud AVC kodeerija*

Selle atribuudid määravad, kuidas toimub täpselt kodeering. Vaatame mõningaid tähtsamatele sätted:

- Väljundi laius ja kõrgus väljundi: need määratleda kodeeritud video eraldusvõime. Meie puhul esmalt avage 640 x 360
- Paneeli: kui määra läbipääs see võtab lihtsalt allika paneeli määr, on võimalik, kuigi see tühistada. Pange tähele, et sellist sekundis teisendamine on ei liikumistee-lennuliinide.
- Profiili ja tase: need kindlaks AVC profiil ja tase. Mugavalt erinevaid tasemeid ja profiilide kohta lisateabe saamiseks klõpsake ikooni küsimärki AVC Video kodeerija komponent ja abi leht kuvatakse iga taset rohkem üksikasju. Meie valimi esmalt tutvustatakse põhi profiiliga tasemel 3,2 (vaikesäte).
- Juhtelemendi režiimi ja Bitrate (KB/s): meie stsenaariumi me valida püsival bitrate (CBR) 1200 kbit väljund
- Video vormindamine: see on VUI (Video kasutatavuse teave), mis toob kirjutatud H.264 voo (küljel teave, mis võib olla kasutab kodeerimise, Kuva suurendamiseks, kuid pole oluline, et õigesti dekodeerida) kohta:
- NTSC (tavaline USA või Jaapan, kasutades 30 sekundis)
- PAL (tavaline Europe, 25 sekundis abil)
- GOP suurus režiim: me tuleb konfigureerida GOP kindlaksmääratud suurusega klahvi intervall kaks sekundit koos suletud GOPs meie eesmärkide puhul. See tagab ühilduvuse dünaamiline pakendit Azure Media Servicesi pakub.

Kanali meie AVC kodeerija, ühendage tihendamata Video väljund kinnitamine Media fail Sisestuskeel komponent tihendamata Video Sisestuskeel kinnitamine: AVC kodeerija.

![Ühendatud AVC kodeerija](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-avc-encoder.png)

*Ühendatud AVC põhi kodeerija*

###<a id="MXF_to_MP4_audio"></a>Heli voo kodeerimine

Selles etapis me on kodeeritud video, kuid tihendamata algse heli voo vajab tihendada. Seda me minna AAC kodeeringuga komponendi AAC kodeerija (Dolby). Lisage see töövoog.

![Ühendatud AVC kodeerija](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-unconnected-aac-encoder.png)

*Ühendatud AAC kodeerija*

Nüüd ole: on ainult üks tihendamata heli Sisestuskeel PIN-koodi kaudu AAC kodeerija ajal rohkem kui tõenäoliselt meediumi Sisestuskeel on kaks erinevat tihendamata helisalvestust saadaval: üks vasak heli kanal ja üks paremale. (Kui tegemist ruumilist heli, mis on 6 kanalit.) Seega ei ole võimalik otse ühendust heli lähtekoha Media fail sisendit AAC heli kodeerija. AAC komponent eeldab, et nn "üksteise" heli voo: ühe voo, mis sisaldab nii vasakule ja õige kanalitega üksteise omavahel. Kui me kuulda meie meediumi lähtefail on mis heli jälgib mis positsioon allika me saame luua sellised üksteise heli voo õigesti määratud kõlari positsiooniga vasakule ja paremale.

Esmalt üks soovite loodud üksteise voogu nõutav allikas audio kanalid. Heli voo Interleaver komponent käsitleda seda meile. Lisage see töövoog ja ühenduse heli väljundid Media fail sisend sinna.

![Ühendatud heli voo Interleaver](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-audio-stream-interleaver.png)

*Ühendatud heli voo Interleaver*

Nüüd kus oleme üksteise heli voogu, me ikka ei määrata, millisest vasakule või paremale kõlari positsioonide, et määrata. Määrata seda, et saaksime saate kasutada kõlari asukoha määraja.

![Kõlari asukoha määraja lisamine](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-adding-speaker-position-assigner.png)

*Kõlari asukoha määraja lisamine*

Läbi mõne kodeerija eelmääratud filtri "Kohandatud" ja lülitada Sisestuskeel voo kasutamiseks kõlari asukoha määraja konfigureerimine ja kanali eelmääratud nimega "2.0 (L, R)". (See on määrata vasakul kõneleja positsiooni 1 kanal ja õige kõneleja positsiooni kanal 2.)

Kõlari asukoha määraja väljund ühenduse AAC kodeerija sisestatud. Seejärel kindlaks AAC kodeerija töötamiseks on "2.0 (L, R)" kanali eelmääratud, et see teaksid stereoheli sisendina lahendusvõimaluste kohta.

###<a id="MXF_to_MP4_audio_and_fideo"></a>Heli ja Video multipleksimise arvesse ka MP4 container

Meie AVC antud encoded videovoo ja meie AAC kodeeritud heli voo, me saate jäädvustada nii sisse ka. MP4 ümbrises. Protsessi segamine erinevate voogu üheks nimetatakse "multipleksimise" (või "muxing"). Kui olete vahelduvuse heli- ja videovoo ühtse igaks. MP4 pakett. Osa, mis seda koordinaadid on. MP4 container nimetatakse multiplekseri ISO MPEG-4. Lisada ühe kujundaja pind ja ühendage AVC Video kodeerija nii AAC kodeerija oma sisendeid.

![Ühendatud MPEG-4 multiplekseri](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-mpeg4-multiplexer.png)

*Ühendatud MPEG-4 multiplekseri*

###<a id="MXF_to_MP4_writing_mp4"></a>MP4 faili kirjutamisel

Väljund faili kirjutamisel kasutatakse faili väljund komponent. See et ühendada väljund multiplekseri ISO MPEG-4, et selle väljundi saab kirjutada kettale. Selle tegemiseks ühenduse kirjutamine Sisestuskeel PIN-koodi faili väljund Container (MPEG-4) väljundi PIN-koodi.

![Ühendatud faili väljund](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-file-output.png)

*Ühendatud faili väljund*

Faili nimi, mida kasutatakse määratakse soovitud atribuuti. Atribuudi võivad olla antud väärtuseks kõva, soovite tõenäoliselt üks Määrake selle avaldise läbi.

Töövoo automaatselt määrata väljund on faili nime atribuudi avaldise, klõpsake dialoogiboksi (kõrval asuvat kaustaikooni) faili nime kõrval. Seejärel valige rippmenüüst "Avaldis". See toob avaldise redaktor. Tühjendage esmalt redaktori sisu.

![Tühja avaldise redaktor](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-empty-expression-editor.png)

*Tühja avaldise redaktor*

Avaldise editor võimaldab sisestage sõnasõnaline väärtus, mis on segatud ühe või mitme muutujaga. Muutujate alustada dollarimärk. Kui vajutate klahvi $, kuvatakse redaktori saadaolevate muutujate valik ripploend ruut. Meie puhul kasutame väljundmuutuja directory ja base Sisestuskeel faili nimi muutujana kombinatsiooni:

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}.MP4

![Täidetud välja avaldise redaktor](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-expression-editor.png)

*Täidetud välja avaldise redaktor*

>[AZURE.NOTE]Selleks, et näha väljund faili vaadata oma kodeering töö Azure peab võimaldama redaktoris avaldise väärtus. 

Kui see on pihta ok avaldise kinnitamiseks atribuudi akna preview mis väärtus faili atribuudi lahendab sel hetkel.

![Faili avaldis lahendab väljundi dir](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-file-expression-resolves-output-dir.png)

*Faili avaldis lahendab väljundi dir*

###<a id="MXF_to_MP4_asset_from_output"></a>Väljundfail Media Services varade loomine

Kuigi me kirjutanud MP4 väljundi faili, läheb vaja veel näitamaks, et selle faili kuulub väljundi varade, mis loob media Servicesi tulemusena töövoo käivitamist. Selleks kasutatakse väljundi faili/varade sõlm töövoo lõuend. Kõik sissetulevad failid selle sõlme teeb tulemuseks Azure Media Servicesi vara osa.

Ühenduse faili väljund komponent väljundi faili/varade komponent selle töövoo lõpetada.

![Lõpetatud töövoog](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow.png)

*Lõpetatud töövoog*

###<a id="MXF_to_MP4_test"></a>Kohalik lõpetanud töövoo testimiseks

Kohalik töövoo testimiseks tabas ülaosas tööriistaribal nuppu Esita. Lõpetades töövoo käivitamist, kontrolli konfigureeritud väljund kausta loodud väljund. Näete lõpetanud MP4 väljundfail kodeeritud failist MXF sisendandmete allikas.

##<a id="MXF_to_MP4_with_dyn_packaging"></a>Kodeerimine MXF MP4 - multibitrate dünaamiline pakendit lubatud

Ülevaate loome failikogumi mitme bitrate MP4 koos AAC kodeeritud ühte heli. MXF Sisestuskeel fail.

Kui mitme bitikiirusega varade väljund on soovitud kasutamine pakutud Azure Media Servicesi, mitu GOP joondatud MP4 faili iga eri bitrate ja eraldusvõime on vaja luua dünaamiline pakendit funktsioone. Selleks [Kodeerimine MXF ühe bitrate MP4](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4) kiirtutvustus annab meile alustada.

![Töövoo käivitamine](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-starting-workflow.png)

*Töövoo käivitamine*

###<a id="MXF_to_MP4_with_dyn_packaging_more_outputs"></a>Ühe või mitme täiendava MP4 väljundeid lisamine

Iga MP4 faili meie tulemuseks Azure Media Servicesi vara toetab erinevate bitrate ja eraldusvõime. Vaatame ühe või mitme MP4 väljundi faile lisada töövoo.

Veendumaks, et meil on meie samade sätetega loodud video andurid, on kõige mugavam dubleerimine juba olemasoleva AVC Video kodeerija ja konfigureerimiseks teise kombinatsiooni resolutsioon ja bitrate (vaatame lisada ühte 960 x 540 2,5 Mbps sekundis 25 raamid). Olemasoleva kodeerija kopeerimiseks Kopeeri kleepige see kujundaja pinna.

Meie uut AVC komponent tihendamata Video väljund PIN-koodi Media fail sisend ühendada.

![Teine AVC kodeerija ühendatud](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-second-avc-encoder-connected.png)

*Teine AVC kodeerija ühendatud*

Nüüd kohandada meie uut AVC kodeerija väljund 960 x 540 2,5 Mbps konfigureerimine. (Kasutada oma atribuudid "väljundi laius", "Väljundi kõrgus" ja "Bitrate (KB/s)" nii.)

Antud soovime kasutada tulemuseks vara koos Azure Media Servicesi dünaamiline pakendit, streaming lõpp-punkti tuleb need MP4 failidest HLS/killustunud MP4/MÕTTEKRIIPSU fragmendid joondatud täpselt omavahel selliselt, et kliendid, kes on vahetamine erinevate bitikiirus saada ühe sujuv pidev video ja heli kogemus tekitada. Veendumaks, mis tehakse, tuleb tagada, et nii AVC kodeerimisseadmetest GOP ("Piltide rühmitamine") atribuutide nii MP4 failide maht on seatud kaks sekundit all, mida saab teha:

- säte GOP suurus režiimi fikseeritud GOP suurus ja
- Klahv raami intervall kaks sekundit.
- määrata ka GOP IDR juhtelemendi suletud GOP, et tagada kõigi GOP on alalise oma ilma sõltuvused

Et meie töövoo mugav mõista, ümber nimetada esimese AVC kodeerija "AVC Video kodeerija 640 x 360 1200kbps" ja teine AVC kodeerija "AVC Video kodeerija 960 x 540 2 500 kbps".

Nüüd lisage teine ISO MPEG-4 multiplekseri ja teine faili väljund. Funktsiooni multiplekseri ühendamine uue AVC kodeerija ja veenduge, et selle väljundi suunatakse faili väljund. Ühendage AAC heli kodeerija väljund uue multiplekseri on sisestatud. Faili väljund omakorda seejärel saab ühendada väljundi faili/varade sõlm lisamiseks Media Services varade, mis luuakse.

![Teine Muxer ja ühendatud faili väljund](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-second-muxer-file-output-connected.png)

*Teine Muxer ja ühendatud faili väljund*

Azure Media Servicesi dünaamiline pakendit ühilduvust konfigureerimine on multiplekseri kogumi režiimi GOP arv või kestus ja GOPs kogumi kohta 1. (See peaks olema vaikimisi.)

![Muxer kogumi režiimi](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-muxer-chunk-modes.png)

*Muxer kogumi režiimi*

Märkus: võite kombinatsioonide mis tahes muud bitrate ja eraldusvõime soovite varade väljundi lisanud seda toimingut korrata.

###<a id="MXF_to_MP4_with_dyn_packaging_conf_output_names"></a>Väljundi failinimed konfigureerimine

Meil on rohkem kui üks ühte faili, lisatakse väljundi vara. See pakub vaja veenduge, et iga väljund faili failinimed on erinevad ja võib-olla isegi rakendada faili nime panemise mess, et oleks selge faili nime, mida olete tegelevad.

Väljundi failinimede saab kontrollida avaldiste designer. Ühe faili väljund komponendid atribuudi paani avamine ja avage fail atribuudi avaldise redaktor. Meie esimese väljundfail konfigureeritud järgmine avaldis kaudu (vt õpetuse läheb [MXF, et ühe bitrate MP4 väljundi](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4)jaoks):

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}.MP4

See tähendab, et meie filename määrab kahte muutujat: väljundkausta kirjutada ja lähtefaili base nimi. On avatud atribuudi töövoo juur ja sissetuleva faili määratakse. Pange tähele, et kataloogi väljundi kasutate kohaliku katsetamiseks; atribuut alistab töövoo mootori kui töövoog käivitatakse pilvepõhist meediumi protsessor Azure Media Services abil.
Anda nii meie väljund faili ühtsete väljundi nime panemise, esimese faili nime panemise avaldis muutmiseks tehke järgmist.

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_640x360_1.MP4

ja teine.

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_960x540_2.MP4

Käivitada vahe katse veendumaks, et nii MP4 väljundi faile on loodud õigesti.

###<a id="MXF_to_MP4_with_dyn_packaging_audio_tracks"></a>Eraldi heli jälituse lisamine

Kui me näeme hiljem, kui me luua .ism faili minna meie MP4 väljund faili, me nõuavad ka ainult MP4 faili nimega heliriba meie kohandatava streaming. Selle faili loomiseks lisada ka täiendavaid muxer töövoo (multiplekseri ISO-MPEG-4) ja AAC kodeerija väljundi PIN-koodi kasutajaga selle Sisestuskeel PIN-koodi Jälita 1.

![Lisatud heli Muxer](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-audio-muxer-added.png)

*Lisatud heli Muxer*

Kolmanda faili väljund komponent väljund väljaminev voo kaudu muxer ja konfigureerimiseks väljendamine failinimede loomiseks tehke järgmist.
    
    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_128kbps_audio.MP4

![Heli Muxer faili väljund loomine](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-audio-muxer-creating-file-output.png)

*Heli Muxer faili väljund loomine*

###<a id="MXF_to_MP4_with_dyn_packaging_ism_file"></a>Lisamise funktsiooni. ISM SMIL fail

Dünaamiline pakendit nii MP4 failid (ja ainult MP4) meie teenuste vara koos töötamiseks, läheb vaja ka manifestfailile (nimetatakse ka "SMIL" faili: sünkroonitud MMS integreerimine Language). Selle faili näitab Azure Media Servicesi, mis MP4 failid on saadaval dünaamiline pakendit ning millised neist heli streaming silmas pidada. Tüüpilised manifestfailile hulk MP4 tema ühe heli voo näeb välja umbes järgmine:
    
    <?xml version="1.0" encoding="utf-8" standalone="yes"?>
    <smil xmlns="http://www.w3.org/2001/SMIL20/Language">
      <head>
        <meta name="formats" content="mp4" />
      </head>
      <body>
        <switch>
          <video src="H264_1900kbps_AAC_und_ch2_96kbps.mp4" />
          <video src="H264_1300kbps_AAC_und_ch2_96kbps.mp4" />
          <video src="H264_900kbps_AAC_und_ch2_96kbps.mp4" />
          <audio src="AAC_ch2_96kbps.mp4" title="AAC_und_ch2_96kbps" />
        </switch>
      </body>
    </smil>

.Ism fail sisaldab vahetamise lause viite iga üksiku MP4 video faile ja lisaks nende ka üks (või rohkem) helifail viited MP4, mis sisaldab ainult heli.

Genereerimine Avaldamisfail meie hulk MP4 kasutaja saab teha komponenti "AMS näidata kirjutaja" kaudu. Seda kasutada, lohistage see pind ja kolm faili väljund komponendid "Kirjutamine terviklik" väljundi sõrmed ühenduse AMS näidata kirjutaja sisestatud. Seejärel veenduge, et ühenduse AMS näidata kirjutaja väljund väljundi faili/vara.

Sarnaselt on meie muud faili väljundi komponendid, konfigureerida .ism väljundi failinime avaldise abil.

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}_manifest.ism

Meie lõpetanud töövoo, mis näeb välja nagu on allpool:

![Lõpetanud MXF multibitrate MP4 töövoogu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-mxf-to-multibitrate-mp4-workflow.png)

*Lõpetanud MXF multibitrate MP4 töövoogu*

##<a id="MXF_to__multibitrate_MP4"></a>Kodeeritud MXF multibitrate MP4 - täiustatud näidis

[Eelmise töövoo kiirtutvustus](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging) olete näha, kuidas ühe MXF Sisestuskeel varade saab ümber väljundi vara mitme bitikiirusega MP4 faile, ainult MP4 faili ja manifestifaili kasutamiseks koos Azure Media Servicesi dünaamiline pakendit.

Juhendis näitab, kuidas mõned aspektid saate täiustatud ja tehtud mugavamaks.

###<a id="MXF_to_multibitrate_MP4_overview"></a>Töövoo ülevaade täiustamiseks

![Multibitrate MP4 töövoo täiustamiseks](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-multibitrate-mp4-workflow-to-enhance.png)

*Multibitrate MP4 töövoo täiustamiseks*

###<a id="MXF_to__multibitrate_MP4_file_naming"></a>Failinimede

Eelmise töövoo me määratud lihtsa avaldise loomiseks väljundi failinimedes alusena. Meil on mõned kordamise küll: kõik selle üksikute väljund faili osad määratud sellist avaldist.

Näiteks meie faili väljundi komponent esimese videofaili on konfigureeritud järgmine avaldis:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_640x360_1.MP4

Kuigi teine väljund video, on meil sellist avaldist:

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}_960x540_2.MP4

Kas ei oleks puhtam veaväärtuse, kuna võivad tekkida kirjavead ja mugavam, kui me võib osa sellest kordamise eemaldada ja teha veel konfigureeritav hoopis vähem? Õnneks saame: autori avaldis võimalusi koos võimalus luua kohandatud atribuutide meie töövoo juurkaustas annab meile mugavuse lisatakse kiht.

Oletame, et me saate juhtida filename konfiguratsiooni üksikute MP4 failide bitikiirus. Nende bitikiirus konfigureerimine ühe keskne koht (juur meie graafik), kus saate neile juurde konfigureerimiseks ja ketas faili nime loomisel kuvatakse eesmärk. Selleks Alustame avaldamise bitrate atribuut nii AVC kodeerimisseadmetest meie töövoo juurkaustas nii, et see saab alates AVC kodeerimisseadmetest ka mõlemad juurkausta kaudu juurdepääsetavad. (Isegi juhul, kui kuvatakse kaks eri kohti, on ainult üks aluseks olev väärtus.)

###<a id="MXF_to__multibitrate_MP4_publishing"></a>Avaldamise peale töövoo juurkausta komponendi atribuudid

Avage esimene AVC kodeerija, minge atribuudi Bitrate (KB/s) ja valige rippmenüüst käsk Avalda.

![Avaldamise bitrate atribuut](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-bitrate-property.png)

*Avaldamise bitrate atribuut*

Konfigureerida avalda dialoogiboksi avalda meie töövoo graph juurkaustas loetavad kuvatav nimi on "Video 1 Bitrate" ja "video1bitrate" avaldatud nimi. Konfigureerida kohandatud rühma nime nimega "Streaming bitikiirus" ja vajutage avalda.

![Avaldamise bitrate atribuut](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-dialog-for-bitrate-property.png)

*Avaldamise bitrate atribuutide dialoogiboks*

Korrake sama bitrate atribuut teise AVC kodeerija ja nime "video2bitrate" koos kuvatav nimi on "Video 2 Bitrate", "Streaming bitikiirus" sama kohandatud rühma.

Kui me nüüd kontrolli töövoo root atribuudid, näeme meie kohandatud rühma koos kahe avaldatud atribuute kuvada. Nende vastavate AVC kodeerija bitrate väärtus on kajastavad.

![Avaldatud bitrate jaoks vajalike materjalide töövoo juurkaustas](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-published-bitrate-props-on-workflow-root.png)

Iga kord, kui soovime juurdepääs atribuutidest koodi või avaldis, me saame seda teha järgmiselt:

- Tekstisisene koodi osa all juurkausta kaudu: node.getPropertyAsString('.. / video1bitrate', null)
- avaldise sees: ${ROOT_video1bitrate}
 
Vaatame Täitke jaotises "Streaming bitikiirus" meie heliriba bitrate seda ka avaldamise kaudu. AAC kodeerija atribuutide, otsige üles säte Bitrate ja valige rippmenüüst selle kõrval avalda. Graafik ja nime "audio1bitrate" root avaldamine ja kuvada oma kohandatud rühmas "Streaming bitikiirus" nimi "Heli 1 Bitrate".

![Avaldamise dialoogiboksi heli bitikiirus](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-dialog-for-audio-bitrate.png)

*Avaldamise dialoogiboksi heli bitikiirus*

![Tulemiks oleva video ja heli jaoks vajalike materjalide juurkaustas](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-resulting-video-and-audio-props-on-root.png)

*Tulemiks oleva video ja heli jaoks vajalike materjalide juurkaustas*

Pange tähele, et muuta need kolm väärtust ka uuesti konfigureerib muutub vastav komponentide nad on seotud väärtused (ja avaldatud kaudu).

###<a id="MXF_to__multibitrate_MP4_output_files"></a>On loodud nimed toetuvad avaldatud kinnisvarahindade väljundfail

Asemel hardcoding meie loodud failinimed saame nüüd muuta meie filename avaldis iga faili väljund osad toetuvad me lihtsalt avaldatud Graphi root bitrate atribuudid. Alustades meie esimese faili väljund, otsige üles soovitud atribuuti ja redigeerida umbes järgmine avaldis:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_video1bitrate}kbps.MP4

See avaldis eri parameetrite saate juurde ja sisestatud pihta dollarimärk, klaviatuuri aknas asudes avaldis. Üks saadaval parameetrid on meie video1bitrate atribuut, mida me varem avaldatud.

![Juurdepääs avaldise parameetreid](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-accessing-parameters-within-an-expression.png)

*Juurdepääs avaldise parameetreid*

Tehke sama faili väljund meie teine video.

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_video2bitrate}kbps.MP4

ja ainult faili väljund:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_audio1bitrate}bps_audio.MP4

Kui me nüüd muuta bitrate video- või faile, kuvatakse vastav kodeerija ümber konfigureerida ja bitrate asuva faili nimi mess olla au automaatselt.

##<a id="thumbnails_to__multibitrate_MP4"></a>Multibitrate MP4 väljundi pisipildid lisamine

Alates töövoo, mis loob [sisestusmeetodi lisamine multibitrate MP4 väljund on MXF](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), me nüüd otsima pisipildid lisamine väljund.

###<a id="thumbnails_to__multibitrate_MP4_overview"></a>Pildil, et lisada töövoo ülevaade

![Multibitrate MP4 töövoo käivitamine](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-multibitrate-mp4-workflow-to-start-from.png)

*Multibitrate MP4 töövoo käivitamine*

###<a id="thumbnails_to__multibitrate_MP4__with_jpg"></a>Lisada JPG kodeerimine

Meie pisipiltide loomise asub saab JPG kodeerija komponent võimalik väljund JPG-faile.

![JPG kodeerija](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-jpg-encoder.png)

*JPG kodeerija*

Me ei saa siiski otse ühendust meie tihendamata Video voo Media fail panust JPG kodeerija sisse. Selle asemel loodab antakse üksikud raamid. See, saame Video raam Gate komponendi kaudu.

![Paneeli gate ühenduse JPG kodeerija](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connect-frame-gate-to-jpg-encoder.png)

*Paneeli gate ühenduse JPG kodeerija*

Paneeli värava iga nii mitme sekundi järel või raamid võimaldab video raam edasi. Intervalli ja offset abil, mis see juhtub aja on atribuudid konfigureerida.

![Video raam Gate atribuudid](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-video-frame-gate-properties.png)

*Video raam Gate atribuudid*

Loome pisipildi iga minut aeg (sekundites) ja intervall 60 režiimi seadmisega.

###<a id="thumbnails_to__multibitrate_MP4_color_space"></a>Värvi ruumi teisendamine tegelemine

Kuigi tundub loogiline nii tihendamata Video sõrmed paneeli värava ja Media fail sisend nüüd olema ühendatud, saame kui me teeks oleks hoiatus.

![Sisestuskeel värvi ruumi tõrge](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-input-color-space-error.png)

*Sisestuskeel värvi ruumi tõrge*

Selle põhjuseks on soovitud viisil, mis värv teave on esindatud meie algse töötlemata tihendamata videovoo, pärit meie MXF erineb JPG kodeerija ootate. Täpsemalt on nn "värvi ruumi", "RGB" või "Halliskaalas" eeldatakse, et meilivoo sisse. See tähendab, et sissetuleva Video raam Gate videovoo pead on rakendatud kohta oma värvi ruumi esmalt teisendus.

Lohistage töövoo värvi ruumi muundur - Inteli ja ühendage see meie paneeli gate.

![Ühenduse värvi ruumi konverterit](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connect-color-space-convertor.png)

*Ühenduse loomine värvi ruumi konverterit*

Valige loendist valmissäte BGR 24 kirje aken Atribuudid.

###<a id="thumbnails_to__multibitrate_MP4_writing_thumbnails"></a>Pisipildi kirjutamine

Erinevate meie MP4 video, JPG kodeerija komponent väljund mitu faili. Selle lahendamiseks saab kasutada stseeni otsingu JPG faili kirjutaja komponent: see tegema sissetulevate JPG pisipildid ja kirjutage neid, on tähe mõne muu arvuga faili nimi. (Arv, tavaliselt näidatud sekundit/üksuste arv voos, mis oli saadud pisipilti.)


![Stseeni otsingu JPG faili kirjutaja tutvustus](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scene-search-jpg-file-writer.png)

*Stseeni otsingu JPG faili kirjutaja tutvustus*

Selle väljundi kausta tee konfigureerida: ${ROOT_outputWriteDirectory} 

ja faili nimi eesliite atribuuti. 

    ${ROOT_sourceFileBaseName}_thumb_

Eesliite määravad, kuidas pisipiltide failid on nimega. Nad saavad tähe abil soovitud pöial positsioon voo näitava arvu.


![Stseeni otsingu JPG faili kirjutaja atribuudid](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scene-search-jpg-file-writer-properties.png)

*Stseeni otsingu JPG faili kirjutaja atribuudid*

Stseeni otsingu JPG faili kirjutaja ühenduse väljundi faili/varade sõlm.

###<a id="thumbnails_to__multibitrate_MP4_errors"></a>Töövoo vigade tuvastamiseks

Värvi ruumi muunduri sisend ühenduse töötlemata tihendamata video väljund. Nüüd teha kohaliku test töövoo käivitada. On hea võimalus töövoo ootamatult Peata ja näitab punane äärisega osa, mis ilmnes tõrge:

![Värvi ruumi muunduri tõrge](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-color-space-converter-error.png)

*Värvi ruumi muunduri tõrge*

Ikooni väike punane "E" ülemises paremas nurgas näha, milline on põhjus kodeering proovi värvi ruumi muunduri komponent nurjus.

![Värvi ruumi muunduri tõrke dialoogiboks](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-color-space-converter-error-dialog.png)

*Värvi ruumi muunduri tõrke dialoogiboks*

Selgub, nagu näete, et sissetulevad värvi ruumi standard värvi ruumi muundur peab olema rec601 YUV meie nõutud teisendamiseks RGB. Ilmselt ei meie voo näitab, et see on rec601. (Rec 601 on standard põimitud analoog video signaale digitaalselt video kodeerimine. Seda määrab aktiivne piirkond 720 heleduse näidiseid ja 360 chrominance näidised rea kohta. Süsteemi kodeerimine värvi nimetatakse YCbCr 4:2:2.)

Selle probleemi lahendamiseks saate näitavad metaandmeid, meie voo, et tegemist on rec601 sisu kohta. Selleks kasutame Video andmete tüüp Updater component, millele Panime meie töötlemata lähte- ja värv ruumi teisendamise komponenti vahepeal. Selle andmete tüüp updater võimaldab käsitsi värskendamine teatud video andmed tüüp atribuudid. Seadistage see näitab soovitud värv ruumi Standard "Rec 601". See võib põhjustada Video andmete tüüp Updater voo "Rec 601" värvi ruumi, kui ei ole värvi ruumi veel määratletud silt. (Pole alistab kõik olemasolevad metaandmed, kui märgitud on ruut Alista.)

![Värvi ruumi Standard andmete tüüp Updater värskendamine](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-update-color-space-standard-on-data-type.png)

*Värvi ruumi Standard andmete tüüp Updater värskendamine*

###<a id="thumbnails_to__multibitrate_MP4_finish"></a>Lõpetatud töövoog

Nüüd, kui meie meie töövoog on lõpetatud, teise testi Käivita see edasi.

![Mitme-mp4 väljundi pisipiltidega lõpetanud töövoo](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow-for-multi-mp4-thumbnails.png)

*Mitme-mp4 väljundi pisipiltidega lõpetanud töövoo*

##<a id="time_based_trim"></a>Multibitrate MP4 väljundi ajapõhiste korrastamine

Alates töövoo, mis loob [sisestusmeetodi lisamine multibitrate MP4 väljund on MXF](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), saame nüüd otsima andmeallika põhjal ajatemplid video trimmimine.

###<a id="time_based_trim_start"></a>Töövoo ülevaade alustada lisamist kärpimise abil

![Kärpimise, et lisada töövoo käivitamine](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-starting-workflow-to-add-trimming.png)

*Kärpimise, et lisada töövoo käivitamine*

###<a id="time_based_trim_use_stream_trimmer"></a>Voo Trimmer abil

Voo Trimmer komponent võimaldab trimmimise algus ja lõpp Sisestuskeel voogu base ajastamist andmed (sekundid, minuti,...). Trimmer ei toeta paneeli vastavalt korrastamine.

![Voo Trimmer](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-stream-trimmer.png)

*Voo Trimmer*

Asemel linkimine AVC andurid ja kõlari asukoha määraja Media fail sisend otse, kuvatakse Panime need vahepeal voo trimmer. (Üks signaal video ja heli üksteise signaal jaoks.)

![Sellele voo Trimmer vahel](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-put-stream-trimmer-in-between.png)

*Sellele voo Trimmer vahel*

Konfigureerige trimmer nii, et me ainult protsessi video ja heli aega 15 minutit ja video 60 sekundi järel.

Minge videot voo Trimmer atribuutide ja nii alguskellaaeg (15s) ja lõpuaeg (60s) atribuutide konfigureerimine. Tehke sama algus-ja lõppaeg väärtused alati konfigureeritud nii meie heli- ja trimmer me avaldada need töövoo juurkausta.

![Alusta aja atribuudi voo Trimmer avaldamine](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-start-time-from-stream-trimmer.png)

*Alusta aja atribuudi voo Trimmer avaldamine*

![Atribuutide dialoogiboks avaldada algus](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-dialog-for-start-time.png)

*Atribuutide dialoogiboks avaldada algus*

![Atribuutide dialoogiboks lõppkellaaeg avaldamine](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-dialog-for-end-time.png)

*Atribuutide dialoogiboks lõppkellaaeg avaldamine*


Kui me nüüd kontrolli meie töövoo root, on nii atribuudid korralikult kuvatud ja konfigureeritav seal.

![Avaldatud juurkaustas saadaolevad atribuudid](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-published-properties-available-on-root.png)

*Avaldatud juurkaustas saadaolevad atribuudid*

Nüüd heli trimmer kärpimise atribuutide avamine ja konfigureerimine nii algus- ja lõpuaegade avaldis, mis viitab avaldatud atribuutide meie töövoo juur.

Heli trimmimine alguskellaaeg:

    ${ROOT_TrimmingStartTime}

ja selle lõppkellaaeg:

    ${ROOT_TrimmingEndTime}

###<a id="time_based_trim_finish"></a>Lõpetatud töövoog

![Lõpetatud töövoog](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow-time-base-trimming.png)

*Lõpetatud töövoog*


##<a id="scripting"></a>Skriptitud komponent tutvustus

Skriptitud komponendid saab teostada suvalise skripte täitmise etapid meie töövoo käigus. Leidub neli erinevate skriptide, mida saab kasutada, iga teatud omadusi ja töövoo elutsükli oma kohta:

- **commandScript**
- **realizeScript**
- **processInputScript**
- **lifeCycleScript**

Dokumentatsiooni skriptitud komponendi läheb iga eespool üksikasjalikumalt. [Järgmises jaotises](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim)kasutatakse **realizeScript** skriptimine komponent ehitada cliplist XML-i pealt töövoo käivitamisel. Mis juhtub ainult siis, kui see on elutsükli komponent häälestamise ajal nimetatakse see skript.


###<a id="scripting_hello_world"></a>Töövoo sees skriptimise: Tere, maailm

Lohistage skriptitud komponent kujundaja pind ja pange sellele (nt "SetClipListXML").

![Lisades skriptitud komponent](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-scripted-comp.png)

*Lisades skriptitud komponent*

Kui skriptitud komponendi atribuute, neli skripti eri tüüpi on näha, iga konfigureeritav erinevate skripti.

![Skriptitud komponendi atribuudid](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scripted-comp-properties.png)

*Skriptitud komponendi atribuudid*

Tühjendage soovitud processInputScript ja avage redaktor jaoks soovitud realizeScript. Nüüd on korras ja skriptimise käivitamiseks valmis.

Skriptide kirjutatakse Groovy Java platvorm, mis säilitab ühilduvuse Java dünaamiliselt kompileeritud skript keel. Tegelikult enamik Java kood on kehtiv tore.

Vaatame kirjutada lihtsa Tere tulemast maailma tore skripti meie realizeScript kontekstis. Sisestage redaktoris järgmist:

    node.log("hello world");

Nüüd käivitada kohaliku katse. Pärast seda Käivita kontrolli (kaudu skriptitud komponent vahekaardil süsteemi) atribuudi logid.

![Tere, maailm log väljund](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-output.png)

*Tere, maailm log väljund*

Me kutsume Logi meetod, sõlm objekti viitab meie praeguse "sõlme" või me ei skriptimise jooksul komponent. Iga komponendi seetõttu on võimalus väljundi logimine andmete vahekaardil süsteemi kaudu. Selles näites me väljund tähistab "Tere, maailm". Oluline on aru, et see võib osutuda eriti abiks juhul silumine tööriista, mis teile pakkuda ülevaate script on tegelikult seda.

Kaudu meie skriptimine keskkonnas meil on juurdepääs atribuudid muud komponendid. Proovige järgmist.


    //inspect current node: 
    def nodepath = node.getNodePath(); 
    node.log("this node path: " + nodepath);
    
    //walking up to other nodes: 
    def parentnode = node.getParentNode(); 
    def parentnodepath = parentnode.getNodePath(); 
    node.log("parent node path: " + parentnodepath);
    
    //read properties from a node: 
    def sourceFileExt = parentnode.getPropertyAsString( "sourceFileExtension", null ); 
    def sourceFileName = parentnode.getPropertyAsString("sourceFileBaseName", null); 
    node.log("source file name with extension " + sourceFileExt + " is: " + sourceFileName);

Meie Logi aken näitab meile järgmist:

![Log väljundi juurdepääsuks sõlm teed](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-output2.png)

*Log väljundi juurdepääsuks sõlm teed*


##<a id="frame_based_trim"></a>Multibitrate MP4 väljundi paneeli vastavalt korrastamine

Alates töövoo, mis loob [sisestusmeetodi lisamine multibitrate MP4 väljund on MXF](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), saame nüüd otsima põhjal paneeli loendab allikas video trimmimine.

###<a id="frame_based_trim_start"></a>Näidis ülevaade alustada lisamist kärpimise abil

![Töövoo lisamise kärpimise, et alustada](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-workflow-start-adding-trimming-to.png)

*Töövoo lisamise kärpimise, et alustada*

###<a id="frame_based_trim_clip_list"></a>Lõikepildi loendi XML-i abil

Kõik eelmise töövoo õppematerjalid kasutasime meie video Sisestuskeel allikana Media fail Sisestuskeel komponent. Kindla stsenaariumi puhul küll, me kasutame klipi loendis Allikas komponendi asemel. Pange tähele, et see peaks olema eelistatud viis töötamine; kasutada ainult Clip loendis allikas, kui seal on reaal põhjust seda teha (nagu on allpool juhul, kui me teha klipi loendi kärpimise võimaluste kasutamine).

Asemel panuse Media fail klipi loendis allikas, lohistage kujunduspinna klipi loendis Allikas komponendi ja ühendage klipi loendi XML-i PIN-koodi klipi loendi XML-i töövoo disaineri sõlme. See peaks asustada Clip loendis Allikas väljundi sõrmed, kus vastavalt panuse video. Nüüd ühendavad tihendamata Video ja heli tihendamata kaudu soovitud lõikepildi loendis Allikas vastav AVC kodeerimisseadmetest ja heli voo Interleaver. Nüüd eemaldada Media fail sisestatud.

![Media fail sisendit asendada Clip loendis Allikas](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-replaced-media-file-with-clip-source.png)

*Media fail sisendit asendada Clip loendis Allikas*

Lõikepildi loendis Allikas komponendi võtab selle sisendina "Klipi loendi XML". Lähtefaili test kohalikult valimisel selle klipi loendi XML-i on teie jaoks automaatselt lisatud.

![Automaatselt lisatud klipi loendi XML-i atribuut](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-auto-populated-clip-list-xml-property.png)

*Automaatselt lisatud klipi loendi XML-i atribuut*

XML-veidi lähemalt vaadata, see on, kuidas see näeb välja nagu:

![Lõikepildi loendi dialoogiboksi redigeerimine](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-edit-clip-list-dialog.png)

*Lõikepildi loendi dialoogiboksi redigeerimine*

See siiski kajastavad klipi loendi XML-i võimaluste. Meil on üheks võimaluseks on lisada "Trim" element nii video ja heli allika, umbes järgmine:

![Lisage trim elemendi klipi loendisse](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-adding-trim-element-to-clip-list.png)

*Lisage trim elemendi klipi loendisse*

Muutmisel klipi loendi xml sellist kohale ja teha kohaliku katse, kuvatakse teile video õigesti on 10 – 20 sekundit video lõigatud.

Vastuolus, mis juhtub, kui teete kohaliku Käivita küll, ei oleks selle sama cliplist XML-i töövoo, mis töötab Azure Media Servicesi rakendatud sama mõju. Azure'i Premium kodeerija käivitamisel luuakse cliplist XML-i iga kord uuesti Sisestuskeel faili kodeering töö anti põhjal. See tähendab, et saaksime teha XML-i muudatused kahjuks seda tühistada.

Cliplist XML-i on kustutamise on kodeering töökohtade käivitamisel tõrjumiseks me uuesti luua pealt kohe pärast meie töövoo algust. Sellise kohandatud toiminguid võib võtta nn "Skriptitud komponent" kaudu. Lisateabe saamiseks lugege teemat [skriptitud komponent tutvustus](media-services-media-encoder-premium-workflow-tutorials.md#scripting).


Lohistage skriptitud komponent kujundaja pind ja pange selle nimeks "SetClipListXML".

![Lisades skriptitud komponent](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-scripted-comp.png)

*Lisades skriptitud komponent*

Kui skriptitud komponendi atribuute, neli skripti eri tüüpi on näha, iga konfigureeritav erinevate skripti.

![Skriptitud komponendi atribuudid](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scripted-comp-properties.png)

*Skriptitud komponendi atribuudid*


###<a id="frame_based_trim_modify_clip_list"></a>Lõikepildi skriptitud komponent loendi muutmine

Enne töövoo käivitamisel luuakse cliplist XML-i saame uuesti kirjutada, vajame juurdepääs cliplist XML-i atribuut ja sisu. Me saame seda teha järgmiselt:

    // get cliplist xml: 
    def clipListXML = node.getProperty("../clipListXml");
    node.log("clip list xml coming in: " + clipListXML);

![Sissetuleva klipi loend on sisse logitud](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-incoming-clip-list-logged.png)

*Sissetuleva klipi loend on sisse logitud*

Kõigepealt on vaja viis kindlaks teha, millised punktist kuni millisel soovime video trimmimine. Et muuta see mugav vähem-tehnilise kasutaja töövoo, avaldada graafik juurkaustas kaks atribuudid. Selleks paremklõpsake päringukujundaja pind ja valige "Lisada atribuudi":

- Esimesele atribuudile: "ClippingTimeStart" tüüpi: "Time Code"
- Teine atribuut: "ClippingTimeEnd" tüüpi: "Time Code"

![Atribuutide dialoogiboks lõikamine alguskellaaeg lisamine](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-clip-start-time.png)

*Atribuutide dialoogiboks lõikamine algusaja lisamine*

![Avaldatud töövoo juurkaustas aja jaoks vajalike materjalide lõikamine](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-clip-time-props.png)

*Avaldatud töövoo juurkaustas aja jaoks vajalike materjalide lõikamine*

Konfigureerida nii atribuudid sobiva väärtuse.

![Konfigureerimine lõikamine algus ja lõpu atribuudid](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-configure-clip-start-end-prop.png)

*Konfigureerimine lõikamine algus ja lõpu atribuudid*

Nüüd kaudu sees meie script me pääsete juurde nii atribuudid umbes järgmine:

    
    // get start and end of clipping:
    def clipstart = node.getProperty("../ClippingTimeStart").toString();
    def clipend = node.getProperty("../ClippingTimeEnd").toString();
    
    node.log("clipping start: " + clipstart);
    node.log("clipping end: " + clipend);

![Logi aken, kus on näha alguses ja lõpus lõikamine](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-show-start-end-clip.png)

*Logi aken, kus on näha alguses ja lõpus lõikamine*

Vaatame sõeluda Code stringid mugavam vorm, kasutades lihtsat Lihtavaldise kasutada:
    
    //parse the start timing: 
    def startregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipstart); 
    startregresult.matches(); 
    def starttimecode = startregresult.group(1); 
    node.log("timecode start is: " + starttimecode); 
    def startframerate = startregresult.group(2); 
    node.log("framerate start is: " + startframerate);
    
    //parse the end timing: 
    def endregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipend); 
    endregresult.matches(); 
    def endtimecode = endregresult.group(1); 
    node.log("timecode end is: " + endtimecode); 
    def endframerate = endregresult.group(2); 
    node.log("framerate end is: " + endframerate);

![Logi aken, kus väljundit sõelutud Code](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-output-parsed-timecode.png)

*Logi aken, kus väljundit sõelutud Code*

Selle teabe käepärast, me nüüd muuta cliplist XML-i kajastamiseks soovitud paneeli täpne lõikamine filmi algus- ja lõpukellaajad.

![Skripti koodi lisamine Kärbi elemendid](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-trim-elements.png)

*Skripti koodi lisamine Kärbi elemendid*

Seda tehti tavaline stringi stringimanipuleerimisfunktsioonide toimingute kaudu. Tulemiks oleva muudetud pildi lisanud XML-i kirjutatakse tagasi atribuudi clipListXML töövoo juur meetodit "Sea_atribuut" kaudu. Log akna kui teise testi käivitada näitaks meile järgmist:

![Logimise klipi loend](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-result-clip-list.png)

*Logimise klipi loend*

Tehke katse, et näha, kuidas video ja heli voogu kärbitud. Nagu saate teha rohkem kui üks katse erinevaid väärtusi kärpimise punktid, märkate, et need ei võeta arvesse siiski! On põhjus on selles, et designer, erinevalt Azure käitusajal alistada cliplist XML-i iga Käivita. See tähendab, et ainult esimene kord olete tekstivormingus ja punktide, põhjustab XML-i muuta, kõik muud korda meie valvur klausel (kui (clipListXML.indexOf ("<trim>") == -1)) takistavad töövoo lisamise teise trim element, kui seal on juba üks Esita.

Meie töövoo testimiseks kohalikult mugav tegemiseks parim lisame mõned maja pidamise kood, mis kontrollib kui trim elemendi oli juba kohal. Sel juhul me saate selle eemaldada enne jätkamist muutes XML-i uusi väärtusi. Selle asemel, kasutades lihtsat stringi toiminguid, siis on tõenäoliselt seda teha otse XML-i objektimudeli sõelumine kaudu.

Enne sellist koodi lisame küll, peate esmalt lisada arvu impordi laused meie skripti alguses:
    
    import javax.xml.parsers.*; 
    import org.xml.sax.*; 
    import org.w3c.dom.*;
    import javax.xml.*;
    import javax.xml.xpath.*; 
    import javax.xml.transform.*; 
    import javax.xml.transform.stream.*; 
    import javax.xml.transform.dom.*;

Pärast seda, lisame nõutav puhastamine kood:

    //for local testing: delete any pre-existing trim elements from the clip list xml by parsing the xml into a DOM:
    DocumentBuilderFactory factory=DocumentBuilderFactory.newInstance();
    DocumentBuilder builder=factory.newDocumentBuilder();
    InputSource is=new InputSource(new StringReader(clipListXML)); 
    Document dom=builder.parse(is);

    //find the trim element inside videoSource and audioSource and remove it if it exists already: 
    XPath xpath = XPathFactory.newInstance().newXPath();
    String findAllTrimElements = "//trim"; 
    NodeList trimelems = xpath.evaluate(findAllTrimElements,dom,XPathConstants.NODESET);

    //copy trim nodes into a "to-be-deleted" collection 
    Set<Element> elementsToDelete = new HashSet<Element>(); 
    for (int i = 0; i < trimelems.getLength(); i++) { 
        Element e = (Element)trimelems.item(i); 
        elementsToDelete.add(e); 
    }

    node.log("about to delete any existing trim nodes");
     //delete the trim nodes: 
    elementsToDelete.each{ 
        e -> e.getParentNode().removeChild(e);
    }; 
    node.log("deleted any existing trim nodes");
    
    //serialize the modified clip list xml dom into a string: 
    def transformer = TransformerFactory.newInstance().newTransformer();
    StreamResult result = new StreamResult(new StringWriter());
    DOMSource source = new DOMSource(dom);
    transformer.transform(source, result); 
    clipListXML = result.getWriter().toString();
    
Järgmine kood läheb kohal kohani, kus lisame trim elementide cliplist XML-i.

Sel hetkel, saame käivitamine ja meie töövoo nii palju, kui kunagi varem tehtud muudatuste meelepärased soovime ajal muuta.    

###<a id="frame_based_trim_clippingenabled_prop"></a>Atribuut ClippingEnabled mugavuse lisamine

Nagu pole alati võiksite juhtub korrastamine, vaatame lõpetada ära meie töövoo, lisades mugav kahendmuutujaga lipp, mis näitab, kas soovime lubamiseks korrastamine / lõikamine.

Samamoodi nagu enne, avaldada uue atribuudi juurkausta meie töövoo nimega "ClippingEnabled", tippige "LOOGILINE".

![Avaldatud atribuudi lubamine lõikamine](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-enable-clip.png)

*Avaldatud atribuudi lubamine lõikamine*

Koos selle all lihtsa guard-klauslis, saame kontrollida, kui on vaja teha kärpimise ja otsustada, kui meie loendi sellisena vaja muuta või mitte.

    //check if clipping is required: 
    def clippingrequired = node.getProperty("../ClippingEnabled"); 
    node.log("clipping required: " + clippingrequired.toString()); 
    if(clippingrequired == null || clippingrequired == false) 
    {
        node.setProperty("../clipListXml",clipListXML); 
        node.log("no clipping required"); 
        return; 
    }


###<a id="code"></a>Täielik kood

    import javax.xml.parsers.*; 
    import org.xml.sax.*; 
    import org.w3c.dom.*;
    import javax.xml.*;
    import javax.xml.xpath.*; 
    import javax.xml.transform.*; 
    import javax.xml.transform.stream.*; 
    import javax.xml.transform.dom.*;
    
    // get cliplist xml: 
    def clipListXML = node.getProperty("../clipListXml");
    node.log("clip list xml coming in: \n" + clipListXML);
    // get start and end of clipping: 
    def clipstart = node.getProperty("../ClippingTimeStart").toString();
    def clipend = node.getProperty("../ClippingTimeEnd").toString();
    
    //parse the start timing:
    def startregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipstart); 
    startregresult.matches(); 
    def starttimecode = startregresult.group(1);
    node.log("timecode start is: " + starttimecode);
    def startframerate = startregresult.group(2);
    node.log("framerate start is: " + startframerate);
    
    //parse the end timing: 
    def endregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipend);
    endregresult.matches(); 
    def endtimecode = endregresult.group(1); 
    node.log("timecode end is: " + endtimecode); 
    def endframerate = endregresult.group(2);

    node.log("framerate end is: " + endframerate);
    
    //for local testing: delete any pre-existing trim elements 
    //from the clip list xml by parsing the xml into a DOM:
    
    DocumentBuilderFactory factory=DocumentBuilderFactory.newInstance();
    DocumentBuilder builder=factory.newDocumentBuilder(); 
    InputSource is=new InputSource(new StringReader(clipListXML)); 
    Document dom=builder.parse(is);

    //find the trim element inside videoSource and audioSource and remove it if it exists already:
    XPath xpath = XPathFactory.newInstance().newXPath(); 
    String findAllTrimElements = "//trim"; 
    NodeList trimelems = xpath.evaluate(findAllTrimElements, dom, XPathConstants.NODESET);

    //copy trim nodes into a "to-be-deleted" collection 
    Set<Element> elementsToDelete = new HashSet<Element>(); 
    for (int i = 0; i < trimelems.getLength(); i++) { 
        Element e = (Element)trimelems.item(i); 
        elementsToDelete.add(e); 
    }
    
    node.log("about to delete any existing trim nodes");
    //delete the trim nodes:
    elementsToDelete.each{ e -> 
        e.getParentNode().removeChild(e); 
    };
    node.log("deleted any existing trim nodes");

    //serialize the modified clip list xml dom into a string:
    def transformer = TransformerFactory.newInstance().newTransformer();
    StreamResult result = new StreamResult(new StringWriter());
    DOMSource source = new DOMSource(dom);
    transformer.transform(source, result);
    clipListXML = result.getWriter().toString();

    //check if clipping is required:
    def clippingrequired = node.getProperty("../ClippingEnabled");
    node.log("clipping required: " + clippingrequired.toString()); 
    if(clippingrequired == null || clippingrequired == false) 
    {
        node.setProperty("../clipListXml",clipListXML);
        node.log("no clipping required");
        return; 
    }

    //add trim elements to cliplist xml 
    if ( clipListXML.indexOf("<trim>") == -1 ) 
    {
        //trim video 
        clipListXML = clipListXML.replace("<videoSource>","<videoSource>\n <trim>\n <inPoint fps=\""+ 
            startframerate +"\">" + starttimecode + 
            "</inPoint>\n" + "<outPoint fps=\"" + endframerate +"\"> " + endtimecode + 
            " </outPoint>\n </trim> \n"); 
        //trim audio 
        clipListXML = clipListXML.replace("<audioSource>","<audioSource>\n <trim>\n <inPoint fps=\""+ 
            startframerate +"\">" + starttimecode + 
            "</inPoint>\n" + "<outPoint fps=\""+ endframerate +"\">" + 
            endtimecode + "</outPoint>\n </trim>\n");
        node.log( "clip list going out: \n" +clipListXML ); 
        node.setProperty("../clipListXml",clipListXML); 
    }


##<a name="also-see"></a>Vt ka 

[Premium kodeerimine Azure Media Services tutvustus](http://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services)

[Kuidas kasutada Premium kodeerimine Azure Media Services](http://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services)

[Nõudmisel sisu Azure Media teenusega kodeerimine](media-services-encode-asset.md#media_encoder_premium_workflow)

[Kodeerija Premium töövoo meediumivorminguid ja kodekite](media-services-premium-workflow-encoder-formats.md)

[Töövoo näidisfailide](http://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows)

[Azure Media Services Exploreri tööriist](http://aka.ms/amse)

##<a name="media-services-learning-paths"></a>Media Servicesi Õppeteed

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
