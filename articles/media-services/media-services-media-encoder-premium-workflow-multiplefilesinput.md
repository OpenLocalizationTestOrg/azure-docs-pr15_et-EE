<properties
    pageTitle="Mitme failid ja komponendi atribuudid kasutamine koos Premium kodeerija | Microsoft Azure'i"
    description="Selles teemas selgitatakse, kuidas kasutada setRuntimeProperties mitme Sisestuskeel failide kasutamine ja edastama kohandatud andmete Media kodeerija Premium töövoo meediumi protsessor."
    services="media-services"
    documentationCenter=""
    authors="xpouyat"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"  
    ms.author="xpouyat;anilmur;juliako"/>

# <a name="using-multiple-input-files-and-component-properties-with-premium-encoder"></a>Mitme failid ja komponendi atribuudid kasutamine koos Premium kodeerija

## <a name="overview"></a>Ülevaade

On stsenaariumid, kus võib peab teil kohandada komponendi atribuudid, klipi loendi XML-sisu saate määrata, või mitme Sisestuskeel faile saata, kui saadate tööülesande **Media kodeerija Premium töövoo** meediumi protsessor. Mõned näited:

- Teksti pealmise video ja säte käitusajal iga Sisestuskeel video tekstiväärtuse (nt praegune kuupäev).
- Kohandamise klipi loendi XML-i (kui soovite määrata ühe või mitme lähtefailid koos või ilma kärpimise jne.).
- Pealmise logo pildi video sisendi ajal kodeeritakse video.

Lasta **Media kodeerija Premium töövoo** teada, et muudate osa atribuute töövoo tööülesande loomisel või mitme Sisestuskeel faile saata, peate selleks kasutama konfiguratsiooni string, mis sisaldab **setRuntimeProperties** ja/või **transcodeSource**. See teema selgitab, kuidas neid kasutada.


## <a name="configuration-string-syntax"></a>Konfiguratsiooni stringi süntaks

Konfiguratsiooni stringi kodeering tööülesande määramiseks kasutab XML-dokumendi, mis näeb välja umbes järgmine:

    <?xml version="1.0" encoding="utf-8"?>
    <transcodeRequest>
      <transcodeSource>
      </transcodeSource>
      <setRuntimeProperties>
        <property propertyPath="Media File Input/filename" value="MyInputVideo.mp4" />
      </setRuntimeProperties>
    </transcodeRequest>


C# koodi, mis loeb failist XML-i konfigureerimine ja edastab selle tööülesande tööd on järgmine:

    XDocument configurationXml = XDocument.Load(xmlFileName);
    IJob job = _context.Jobs.CreateWithSingleTask(
                                                  "Media Encoder Premium Workflow",
                                                  configurationXml.ToString(),
                                                  myAsset,
                                                  "Output asset",
                                                  AssetCreationOptions.None);


## <a name="customizing-component-properties"></a>Kohandamise komponendi atribuudid  

### <a name="property-with-a-simple-value"></a>Lihtne väärtusega atribuut
Mõnel juhul on kasulik komponent atribuudi koos töövoo fail, mille saab teostada Media kodeerija Premium töövoo kohandamine.

Oletame, et teie loodud töövoo teksti ülekatete videod ja tekst (nt praegune kuupäev) peaks olema seatud käitusajal. Selleks on uus väärtus tekst atribuudi ülekatet komponendi kodeering ülesande määrata teksti. See abil saate muuta muid atribuute komponendi töövoo (näiteks seisukoht või ülekatte värvi, bitrate AVC kodeerija jne.).

**setRuntimeProperties** kasutatakse alistamiseks kinnisvara töövoo komponendid.

Näide:

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="Media File Input/filename" value="MyInputVideo.mp4" />
          <property propertyPath="/primarySourceFile" value="MyInputVideo.mp4" />
          <property propertyPath="Optional Overlay/Overlay/filename" value="MyLogo.png"/>
          <property propertyPath="Optional Text Overlay/Text To Image Converter/text" value="Today is Friday the 13th of May, 2016"/>
      </setRuntimeProperties>
    </transcodeRequest>


### <a name="property-with-an-xml-value"></a>XML-i väärtusega atribuut

Atribuut, mis eeldab, et XML-i väärtuse määramiseks kapseldada abil `<![CDATA[ and ]]>`.

Näide:

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="/primarySourceFile" value="start.mxf" />
          <property propertyPath="/inactiveTimeout" value="65" />
          <property propertyPath="clipListXml" value="xxx">
          <extendedValue><![CDATA[<clipList>
            <clip>
              <videoSource>
                <mediaFile>
                  <file>start.mxf</file>
                </mediaFile>
              </videoSource>
              <audioSource>
                <mediaFile>
                  <file>start.mxf</file>
                </mediaFile>
              </audioSource>
            </clip>
            <primaryClipIndex>0</primaryClipIndex>
            </clipList>]]>
          </extendedValue>
          </property>
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>

>[AZURE.NOTE]Veenduge, et sellele reavahetuse saatja kohe pärast `<![CDATA[`.


### <a name="propertypath-value"></a>propertyPath väärtus

Eelmistes näidetes on propertyPath oli "/ Media fail sisendit/failinimi" või "/ inactiveTimeout" või "clipListXml".
See on üldiselt komponendi nimi ja seejärel selle atribuudi nimi. Tee võib olla rohkem või vähem taset, nt "/ primarySourceFile" (kuna see atribuut on töövoo root) või "/ Video töötlemine joonise ülekatet/läbipaistmatust" (kuna ülekatte rühma).    

Tee ja atribuudi nimi, kasutage selle toimingu nuppu, mis on kohe iga atribuudi kõrval. Saate selle toimingu nupu ja valige **Redigeeri**. See näitab tegelikku nime atribuudi ja kõrgemal, nimeruumi kohe.

![Toimingu/redigeerimine](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture6_actionedit.png)

![Atribuut](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture7_viewproperty.png)

## <a name="multiple-input-files"></a>Mitme failid

Iga tööülesande, mida te **Media kodeerija Premium töövoo** nõuab kahte varad tehke järgmist.

- Esimene on *Töövoo varade* , mis sisaldab töövoo faili. Saate kujundada töövoo failide [Töövoodisaineris](media-services-workflow-designer.md).
- Teine on *Meediumi varade* , mis sisaldab media failid, mida soovite kodeerida.

Kui saadate mitme meediumifailide **Media kodeerija Premium töövoo** kodeerija, kehtivad järgmised piirangud.

- Kõik meediumifailid peab olema sama *Meediumi vara*. Mitme Meediumivarad abil ei toetata.
- Peate määrama esmane faili selle meediumi vara (ideaalvariandis mitte, on see peamine video fail, mille kodeerija palutakse töötlemine).
- Tuleb läbida konfiguratsiooni andmed, mis sisaldab **setRuntimeProperties** ja/või **transcodeSource** elemendi protsessor.
  - **setRuntimeProperties** kasutatakse alistamiseks filename vara või mõne muu töövoo komponendid.
  - **transcodeSource** abil saate määrata XML-i lõikepiltide loendi sisu.

Töövoo ühendused:

 - Kui kasutate ühte või mitut Media fail sisendit komponendid ja leping **setRuntimeProperties** abil saate määrata faili nime, siis ei saa ühendust esmane faili komponent PIN-koodi need. Veenduge, et oleks seos objekti esmane fail ja Media fail kogumi.
 - Kui eelistate kasutada XML-i lõikepiltide loendi ja ühe meediumi allikas komponendi, siis saate ühendada mõlemat koos.

![Ühendamata esmane lähtefail Media fail sisendit](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture0_nopin.png)

*Kui kasutate setRuntimeProperties atribuudi nimi on pole ühenduse esmane failist Media fail sisendit komponendid.*

![Ühenduse klipi loendist XML-allikas loendi Lõikepildid](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture1_pincliplist.png)

*Saate Clip loendi XML-i ühenduse meedia allikas ja kasutada transcodeSource.*


### <a name="clip-list-xml-customization"></a>Lõikepildi loendi XML-i kohandamine
Saate Clip loendi XML-i töövoo käitusajal **transcodeSource** konfiguratsiooni stringi XML-i abil. Selleks, et XML-i lõikepiltide loendi PIN-koodi töövoo komponent meediumi Source ühendamiseks.

    <?xml version="1.0" encoding="utf-16"?>
      <transcodeRequest>
        <transcodeSource>
          <clipList>
            <clip>
              <videoSource>
                <mediaFile>
                  <file>video-part1.mp4</file>
                </mediaFile>
              </videoSource>
              <audioSource>
                <mediaFile>
                  <file>video-part1.mp4</file>
                </mediaFile>
              </audioSource>
            </clip>
            <primaryClipIndex>0</primaryClipIndex>
          </clipList>
        </transcodeSource>
        <setRuntimeProperties>
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>

Kui soovite määrata selle atribuudi abil väljund faili nimi, kasutades 'Avaldiste' /primarySourceFile, siis soovitame läbides klipi loendi XML-i atribuut *pärast seda,* kui atribuudi /primarySourceFile vältida klipi loendi eirata /primarySourceFile säte.

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="/primarySourceFile" value="c:\temp\start.mxf" />
          <property propertyPath="/inactiveTimeout" value="65" />
          <property propertyPath="clipListXml" value="xxx">
          <extendedValue><![CDATA[<clipList>
            <clip>
              <videoSource>
                <mediaFile>
                  <file>c:\temp\start.mxf</file>
                </mediaFile>
              </videoSource>
              <audioSource>
                <mediaFile>
                  <file>c:\temp\start.mxf</file>
                </mediaFile>
              </audioSource>
            </clip>
            <primaryClipIndex>0</primaryClipIndex>
            </clipList>]]>
          </extendedValue>
          </property>
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>

Koos täiendavate paneeli täpne korrastamine:

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="/primarySourceFile" value="start.mxf" />
          <property propertyPath="/inactiveTimeout" value="65" />
          <property propertyPath="clipListXml" value="xxx">
          <extendedValue><![CDATA[<clipList>
            <clip>
              <videoSource>
                <trim>
                  <inPoint fps="25">00:00:05:24</inPoint>
                  <outPoint fps="25">00:00:10:24</outPoint>
                </trim>
                <mediaFile>
                  <file>start.mxf</file>
                </mediaFile>
              </videoSource>
              <audioSource>
               <trim>
                  <inPoint fps="25">00:00:05:24</inPoint>
                  <outPoint fps="25">00:00:10:24</outPoint>
                </trim>
                <mediaFile>
                  <file>start.mxf</file>
                </mediaFile>
              </audioSource>
            </clip>
            <primaryClipIndex>0</primaryClipIndex>
            </clipList>]]>
          </extendedValue>
          </property>
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>


## <a name="example"></a>Näide

Kaaluge näide, kus soovite ülekattega logo pildi video sisendi ajal kodeeritakse video. Selles näites Sisestuskeel video nimega "MyInputVideo.mp4" ja logo nimega "MyLogo.png". Peaksite tegema järgmised toimingud:

- Töövoo varade töövoo faili loomine (vt järgmist näidet).
- Meediumi varade, mis sisaldab kahte failide loomine: MyInputVideo.mp4 esmane faili-ja MyLogo.png.
- Saata ülesande Media kodeerija Premium töövoo meediumi protsessor ülaltoodud Sisestuskeel varad ja määrake järgmised konfiguratsiooni string.

Konfiguratsioon:

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="Media File Input/filename" value="MyInputVideo.mp4" />
          <property propertyPath="/primarySourceFile" value="MyInputVideo.mp4" />
          <property propertyPath="Media File Input Logo/filename" value="MyLogo.png" />
        </setRuntimeProperties>
      </transcodeRequest>


Eeltoodud näites saadetakse Media fail sisendit komponent ja atribuudi primarySourceFile video faili nimi. Mõne muu Media fail sisend on ühendatud pildi ülekatet komponent saadetakse logo faili nimi.

>[AZURE.NOTE]Atribuudi primarySourceFile saadetakse video faili nime. Selle põhjuseks on selle atribuudi kasutamine töövoo jaoks õige väljund faili nimi, kasutades avaldisi, nt.


### <a name="step-by-step-workflow-creation-that-overlays-a-logo-on-top-of-the-video"></a>Samm-sammult töövoo loomine mis katab logo peal video     

Siit leiate juhised töövoo, mis võtab sisendina kaks faili loomiseks: video ja pilt. See on ülekattega pildi peale video.

Avage **Töövoo disaineri** ja valige **fail** > **Uue tööruumi** > **Transcode näidis**.

Uue töövoo kuvatakse kolm elemendid:

- Esmane lähtefail
- Lõikepildi loendi XML-i
- Väljundi faili/vara  

![Uue kodeering töövoo](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture9_empty.png)

*Uue kodeering töövoo*


Selleks, et meediumifail Sisestuskeel aktsepteerimiseks alustada lisamine Media fail Sisestuskeel komponent. Soovite töövoo lisada komponent, otsige see hoidla otsinguväljale ja lohistage kujundaja paanil soovitud kirje.

Järgmisena lisage videofaili oma töövoo kujundamise kasutamiseks. Selleks klõpsake paanil tausta töövoodisaineris ja otsige esmane lähtefail atribuudi atribuut Parempoolse paani. Klõpsake kausta ikooni ja valige sobiv video fail.

![Esmane lähtefail](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture10_primaryfile.png)

*Esmane lähtefail*


Järgmisena Määrake videofail Media fail sisestatud komponendi.   

![Media fail sisendandmete allikas](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture11_mediafileinput.png)

*Media fail sisendandmete allikas*


Niipea, kui see on tehtud, Media fail sisendit komponent kontrolli faili ja asustada selle väljundi sõrmed vastavalt failini, mida see kontrolli.

Järgmiseks on lisada mõne "Video andmete tüüp Updater" Rec.709 ruumi värvi määramiseks. Lisage "Video vormindamine muunduri" mis on seatud andmete paigutuse/küljendi tüüp = konfigureeritav tasapinnalised. See funktsioon teisendab videovoo sellises vormingus, mida võib võtta allikana ülekatet komponent.

![video andmete tüüp Updater ja vormingu muundur](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture12_formatconverter.png)

*Video andmete tüüp Updater ja vormingu muundur*

![Küljendi tüüp = konfigureeritav tasapinnalised](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture12_formatconverter2.png)

*Küljendi tüüp on konfigureeritav tasapinnalised*

Järgmiseks lisada Video ülekattega komponent ja ühendage video (tihendamata) PIN-koodi (tihendamata) video PIN-koodi media fail sisestatud.

Lisada mõne muu Media fail sisendit (laadimiseks logo fail), klõpsake selle komponendi ja nimetage see ümber "Media fail sisendit Logo" ja valige pilt (PNG-vormingus faili näiteks) soovitud atribuuti. Ühenduse tihendamata pilt PIN-koodi ülekatte tihendamata pilt PIN-koodi.

![Ülekatet komponent ja pildi lähtefail](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture13_overlay.png)

*Ülekatet komponent ja pildi lähtefail*


Kui soovite muuta logo video asukoha (nt võiksite paigutage see on 10 protsenti off video vasakus ülanurgas), tühjendage märkeruut "Käsitsi sisestatud". Saate seda teha, kuna kasutate Media fail sisendit ülekatet komponent logoga faili esitada.

![Ülekatet asukoht](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture14_overlay_position.png)

*Ülekatet asukoht*


H.264 videovoo kodeerida lisada kujundaja pind AVC Video kodeerija ja AAC kodeerija komponendid. Ühendavad.
AAC kodeerija häälestamine ja valige heli vormingu muutmise/valmissäte: 2.0 (L, R).

![Heli- ja kodeerimisseadmetest](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture15_encoders.png)

*Heli- ja kodeerimisseadmetest*


Nüüd lisage **ISO Mpeg-4 multiplekseri** ja **Faili väljund** komponendid ja ühendavad nagu on näidatud.

![MP4 multiplekseri ja faili väljund](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture16_mp4output.png)

*MP4 multiplekseri ja faili väljund*


Peate määrama väljund faili nimi. Klõpsake **Faili väljund** komponent ja avaldise faili redigeerimiseks.

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_withoverlay.mp4

![Failinime väljund](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture17_filenameoutput.png)

*Failinime väljund*

Kohalik, et kontrollida, kas see töötab õigesti töövoo käivitada.

Kui see on lõpule jõudnud, saate selle Azure Media Servicesi käivitada.

Vara Azure Media Servicesi kaks faile see esmalt ettevalmistamine: videofaili ja logo. Saate seda teha, kasutades .net-i või REST API-ga. Samuti saate seda teha Azure portaali või [Azure Media Services Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (AMSE) abil.

Selle õpetuse näitab, kuidas AMSE varade haldamine. On kaks võimalust vara faile lisada.

- Luua kohalikus kaustas, kopeerige kaks faili, ja pukseerige **varade** menüüd kaust.
- Videofaili vara üles laadida, varade teavet kuvada, avage menüü failid ja üles laadida faili (logo).

>[AZURE.NOTE]Veenduge, et seada esmane faili varade (põhi videofaili).

![Varade failide AMSE](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture18_assetinamse.png)

*Varade failide AMSE*


Valige vara ja selle abil Premium kodeerija kodeerida. Töövoo Lisamise kuupäev ja valige see.

Klõpsake nuppu andmete edastamiseks protsessor ja lisage järgmine XML käitusaja atribuutide seadmine:

![Klõpsake AMSE Premium kodeerija](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture19_amsepremium.png)

*Klõpsake AMSE Premium kodeerija*


Kleepige järgmine XML-andmed. Peate määrama video faili nimi Media fail sisendit ja primarySourceFile. Määrake nimi faili nimi, logo liiga.

    <?xml version="1.0" encoding="utf-16"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="Media File Input/filename" value="Microsoft_HoloLens_Possibilities_816p24.mp4" />
          <property propertyPath="/primarySourceFile" value="Microsoft_HoloLens_Possibilities_816p24.mp4" />
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>

![setRuntimeProperties](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture20_amsexmldata.png)

*setRuntimeProperties*


Kui .NET SDK abil saate luua ja käivitada tööülesanne, on see XML-andmete stringina konfiguratsiooni edasi.

    public ITask AddNew(string taskName, IMediaProcessor mediaProcessor, string configuration, TaskOptions options);

Pärast töö on lõpule jõudnud, kuvatakse MP4 faili väljund varade ülekatte!

![Video ülekattega](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture21_resultoverlay.png)

*Video ülekattega*


Töövoo näidis saate alla laadida [GitHub](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/).


## <a name="see-also"></a>Vt ka

- [Premium kodeerimine Azure Media Services tutvustus](http://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services)

- [Kuidas kasutada Azure Media Servicesi Premium kodeerimine](http://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services)

- [Nõudmisel sisu Azure Media Servicesi kodeerimine](media-services-encode-asset.md#media_encoder_premium_workflow)

- [Media kodeerija Premium töövoo vormingud ja kodekite](media-services-premium-workflow-encoder-formats.md)

- [Töövoo näidisfailide](https://github.com/AzureMediaServicesSamples/Encoding-Presets/tree/master/VoD/MediaEncoderPremiumWorkfows)

- [Azure Media Services Exploreri tööriist](http://aka.ms/amse)

## <a name="media-services-learning-paths"></a>Media Servicesi Õppeteed

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
