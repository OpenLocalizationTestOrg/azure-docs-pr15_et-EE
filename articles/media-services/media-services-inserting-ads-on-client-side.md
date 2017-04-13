<properties 
    pageTitle="Kliendipoolse reklaamid lisamine | Microsoft Azure'i" 
    description="Selles teemas kirjeldatakse, kuidas lisada reklaamid kliendis." 
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
    ms.date="09/26/2016" 
    ms.author="juliako"/>


#<a name="inserting-ads-on-the-client-side"></a>Kliendipoolse reklaamid lisamine

See artikkel sisaldab teavet, kuidas lisada mitmesuguseid reklaamid kliendi poolel.

Suletud Live voogesituse tiitrid ja ad toe kohta leiate teemast [toetatud suletud pealdis ja Ad järjepunkti standardid](media-services-live-streaming-with-onprem-encoders.md#cc_and_ads).

>[AZURE.NOTE] Praegu ei toeta Azure Media Playeri reklaamid.

##<a id="insert_ads_into_media"></a>Meediumifailide reklaamid lisamine

Azure Media Servicesi toetab ad järjepunkti Windows Media platvormi kaudu: mängija raamistik. Windows 8, Silverlighti, Windows Phone 8 ja iOS-seadmete jaoks saadaolevaid Playeri raamistiku ad toega. Iga mängija framework sisaldab proovi kood, mis näitab teile, kuidas rakendada pleieri rakendus. On kolm erinevat tüüpi reklaamid meediumi: loendisse lisada.

- **Lineaarne** – täielik paneeli reklaamid, mis peamisi video peatamiseks.
- **Mittelineaarne** – ülekatet reklaamid, mis kuvatakse peamised video esitamise, tavaliselt logo või muu staatilise pildi asetada mängija.
- **Companion** – reklaamid, mis on kuvatud mängija väljaspool.

Reklaamid paigutada põhilise video ajaskaala igal ajal. Rääkige mängija reklaami esitamise ja millised reklaamid esitamiseks. Seda saab teha mitu standardne XML-põhine faili abil: Video Ad teenuse mall (VAST), digitaalse Video mitme Ad loend (VMAP), meediumi abstraktsed järjestuse mall (MAST) ja digitaalse Video mängija Ad kasutajaliidese määratlus (VPAID). SUUR failide määrata, millised reklaamid kuvamiseks. VMAP failide määrata, millal esitada erinevad reklaamid ja sisaldavad suur XML-i. MAST failid on teine võimalus jada reklaamid, mis võib sisaldada suur XML-i ka. VPAID failide määratleda liidest videopleieri ja ad või ad serveri vahel.

Iga mängija framework töötab erinevalt ja iga käsitletakse eraldi teema. See teema kirjeldab lihtsa menetlustele, kasutatakse reklaamid lisamiseks. Videopleieri rakenduste taotleda reklaamid ad-serverist. Ad serverit vastata mitmel viisil:

- SUUR faili tagasi
- Tagastab VMAP faili (koos manustatud VAST)
- Tagastab MAST faili (koos manustatud VAST)
- SUUR faili VPAID reklaamid tagasi

 
###<a name="using-a-video-ad-service-template-vast-file"></a>Video Ad teenuse malli (suur) faili abil

SUUR faili saate määrata, millist ad või reklaamide kuvamise. Järgmine XML-i on suur faili lineaarse ad näide:
    
    <VAST version="2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="oxml.xsd">
      <Ad id="115571748">
        <InLine>
          <AdSystem version="2.0 alpha">Atlas</AdSystem>
          <AdTitle>Unknown</AdTitle>
          <Description>Unknown</Description>
          <Survey></Survey>
          <Error></Error>
          <Impression id="Atlas"><![CDATA[http://www.myserver.com/tracking-resource]]></Impression>
          <Creatives>
            <Creative id="video" sequence="0" AdID="">
              <Linear>
                <Duration>00:00:32</Duration>
                <TrackingEvents>
                  <Tracking event="start"><![CDATA[http://www.myserver.com/start-tracking-resource]]></Tracking>
                  <Tracking event="midpoint"><![CDATA[http://www.myserver.com/midpoint-tracking-resource]]></Tracking>
                  <Tracking event="complete"><![CDATA http://www.myserver.com/complete-tracking-resource]]></Tracking>
                  <Tracking event="expand"><![CDATA[http://www.myserver.com/expand-tracking-resource]]></Tracking>
                </TrackingEvents>
                <VideoClicks>
                  <ClickThrough id="Atlas Redirect"><![CDATA[http://www.myserver.com/click-resource]]></ClickThrough>
                  <ClickTracking id="Spare"></ClickTracking>
                </VideoClicks>
                <MediaFiles>
                  <MediaFile apiFramework="Windows Media" id="windows_progressive_200" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="200" width="400" height="300" type="video/x-ms-wmv">
                    <![CDATA[http://www.myserver.com/media/myad_200_4x3.wmv]]>
                  </MediaFile>
                  <MediaFile apiFramework="Windows Media" id="windows_progressive_300" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="300" width="400" height="300" type="video/x-ms-wmv">
                    <![CDATA[http://www.myserver.com/media/myad_300_4x3.wmv]]>
                  </MediaFile>
                </MediaFiles>
              </Linear>
            </Creative>
          </Creatives>
          <Extensions>
            <Extension type="Atlas">
            </Extension>
          </Extensions>
        </InLine>
      </Ad>
    </VAST>
    
Lineaarne ad on kirjeldatud funktsiooni **<Linear>** element. Seda määrab reklaami kestus, jälgimine sündmusi, klõpsake nuppu jälgimine ja mitmesuguseid kaudu **<MediaFile>** elemente. Jälgimise sündmuste määratletud funktsiooni **<TrackingEvents>** elemendi ja luba ettevõtte ad serveris jälgida reklaami kuvamise ajal erinevad sündmused. Sellisel juhul algus, keskpunkt lõpule jõudnud, ja laiendada sündmuste jälgitakse. Algus sündmuse juhul, kui reklaami ei kuvata. Sündmuse keskpunkt juhul, kui vähemalt 50% reklaami ajaskaala vaadatud. Täielik sündmus on käitamisel reklaami lõppu. Laienda sündmus juhul, kui kasutaja laiendab videopleieri Täisekraanvaate aktiveerimiseks. Clickthroughs on määratud, kus on **<ClickThrough>** elemendid on **<VideoClicks>** elemendi ja määrab URI ressursi kuvamiseks, kui kasutaja klõpsab ad. ClickTracking on määratud lisamine **<ClickTracking>** element, ka kasutamine on **<VideoClicks>** elemendi ja määrab jälitus ressursi taotleda, kui kasutaja klõpsab reklaami mängija. Funktsiooni **<MediaFile>** elementide määrata teatud kodeeringuga reklaami teavet. Kui seal on rohkem kui üks **<MediaFile>** element, videopleieri saate valida parima kodeerimine platvormi. 

Lineaarne reklaamid saab kuvada määratud järjestuses. Selleks, lisada täiendavat <Ad> elementi, et see on VAST fail ja määrata, et kasutades sequence atribuuti. Järgmine näide illustreerib järgmine:
    
    <VAST version="2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="oxml.xsd">
      <Ad id="1" sequence="0">
        <InLine>
          <AdSystem version="2.0 alpha">Atlas</AdSystem>
          <AdTitle>Unknown</AdTitle>
          <Description>Unknown</Description>
          <Survey></Survey>
          <Error></Error>
          <Impression id="Atlas"><![CDATA[http://myserver.com/Impression/Ad1trackingResouce]]></Impression>
          <Creatives>
            <Creative id="video" sequence="0" AdID="">
              <Linear>
                <Duration>00:00:32</Duration>
                <MediaFiles>
                  <!-- ... -->
                </MediaFiles>
              </Linear>
            </Creative>
          </Creatives>
        </InLine>
      </Ad>
      <Ad id="2" sequence="1">
        <InLine>
          <AdSystem version="2.0 alpha">Atlas</AdSystem>
          <AdTitle>Unknown</AdTitle>
          <Description>Unknown</Description>
          <Survey></Survey>
          <Error></Error>
          <Impression id="Atlas"><![CDATA[http://myserver.com/Impression/Ad2trackingResouce]]></Impression>
          <Creatives>
            <Creative id="video" sequence="0" AdID="">
              <Linear>
                <Duration>00:00:30</Duration>
                <MediaFiles>
                  <!-- ... -->
                </MediaFiles>
              </Linear>
            </Creative>
          </Creatives>
        </InLine>
      </Ad>
    </VAST>
    
Nonlinear reklaamid on määratud lisamine <Creative> ka. Järgmises näites on <Creative> element, mis kirjeldab mittelineaarsete ad.

    <Creative id="video" sequence="1" AdID="">
      <NonLinearAds>
        <NonLinear width="216" height="121" minSuggestedDuration="00:00:15">
          <StaticResource creativeType="image/png"><![CDATA[http://myserver/images/image.png]]></StaticResource>
          <StaticResource creativeType="image/jpg"><![CDATA[http://myserver/images/image.jpg]]></StaticResource>
        </NonLinear>
        <TrackingEvents>
             <Tracking event="acceptInvitation"><![CDATA[http://myserver/tracking/trackingID]></Tracking>
             <Tracking event="collapse"><![CDATA[http://myserver/tracking/trackingID2]]></Tracking>
         </TrackingEvents>
       </NonLinearAds>
    </Creative>

 
Funktsiooni **<NonLinearAds>** elemendi võib sisaldada ühte või mitut **<NonLinear>** elemente, millest igaüks saab kirjeldada mittelineaarsete ad. Funktsiooni **<NonLinear>** element määrab nonlinear ad ressurss. Ressursi võib olla mõne **<StaticResouce>**, mis **<IFrameResource>**, või **<HTMLResouce>**.**<StaticResource>** mitte-HTML-ressursi kirjeldatakse ja määratleb creativeType atribuut, mis määrab ressursi kuvamise viisi.

Pildi/gif, image/jpeg, image/png-ressursi kuvatakse HTML- **<img>** silt.

HTML-i <**skripti**> Tag kuvatakse rakenduse/x-javascript – ressursi.

Flash Playeri kuvatakse rakenduse/x-shockwave-flash-ressursi.

**<IFrameResource>**kirjeldatakse HTML-ressursi, mis kuvatakse IFRAME'i. **<HTMLResource>**kirjeldatakse HTML-koodi, mida saate lisada veebilehe osa. **<TrackingEvents>**Määrake jälgimise sündmuste ja URI nõuda, kui sündmus. Selles näites jälgitakse acceptInvitation ja Ahenda sündmused. Lisateavet selle **<NonLinearAds>** elemendi ja tema tütardele, lugege teemat IAB.NET/VAST. Pange tähele, et selle **<TrackingEvents>** elemendi asub selle** <NonLinearAds> ** elemendi pigem kui **<NonLinear>** element.

Companion reklaamid on määratletud on <CompanionAds> element. Funktsiooni <CompanionAds> elemendi võib sisaldada ühte või mitut <Companion> elemente. Iga <Companion> elemendi kirjeldatakse companion ad ja võib sisaldada soovitud <StaticResource>, <IFrameResource>, või <HTMLResource> mis on määratletud samamoodi nagu nonlinear ad. SUUR fail võib sisaldada mitut companion reklaamid ja rakenduse Playeri saate valida sobivaim ad kuvamiseks. VAST kohta leiate lisateavet teemast [suur 3.0](http://www.iab.net/media/file/VASTv3.0.pdf).

###<a name="using-a-digital-video-multiple-ad-playlist-vmap-file"></a>Mitme Ad loend (VMAP) faili digitaalse Video abil

VMAP faili saate määrata ad piirid ilmnemisel, kui kaua iga leheküljepiiri on, saate kuvada mitu reklaamid piiri ja reklaamid võib olla kuvatud ajal piiri. Järgmine näide VMAP faili, mis määratleb ühe ad piiri:
    
    <vmap:VMAP xmlns:vmap="http://www.iab.net/vmap-1.0" version="1.0">
      <vmap:AdBreak breakType="linear" breakId="mypre" timeOffset="start">
        <vmap:AdSource allowMultipleAds="true" followRedirects="true" id="1">
          <vmap:VASTData>
            <VAST version="2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="oxml.xsd">
              <Ad id="115571748">
                <InLine>
                  <AdSystem version="2.0 alpha">Atlas</AdSystem>
                  <AdTitle>Unknown</AdTitle>
                  <Description>Unknown</Description>
                  <Survey></Survey>
                  <Error></Error>
                  <Impression id="Atlas"><![CDATA[http://view.atdmt.com/000/sview/115571748/direct;ai.201582527;vt.2/01/634364885739970673]]></Impression>
                  <Creatives>
                    <Creative id="video" sequence="0" AdID="">
                      <Linear>
                        <Duration>00:00:32</Duration>
                        <MediaFiles>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_200" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="200" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_1_000_200_4x3.wmv]]>
                          </MediaFile>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_300" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="300" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_2_000_300_4x3.wmv]]>
                          </MediaFile>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_500" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="500" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_1_000_500_4x3.wmv]]>
                          </MediaFile>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_700" maintainAspectRatio="true" scaleable="true" delivery="progressive" bitrate="700" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_2_000_700_4x3.wmv]]>
                          </MediaFile>
                        </MediaFiles>
                      </Linear>
                    </Creative>
                  </Creatives>
                </InLine>
              </Ad>
            </VAST>
          </vmap:VASTData>
        </vmap:AdSource>
        <vmap:TrackingEvents>
          <vmap:Tracking event="breakStart">
            http://MyServer.com/breakstart.gif
          </vmap:Tracking>
        </vmap:TrackingEvents>
      </vmap:AdBreak>
    </vmap:VMAP>
     
Algab VMAP faili lisamine <VMAP> element, mis sisaldab ühte või mitut <AdBreak> elemendid, määrates ka ad leheküljepiiri. Iga ad piiri määrab leheküljepiiri tüüp, leheküljepiiri ID-d, ja aja offset. Atribuut breakType määrab ad, mida saab esitada pausi ajal tüüpi: lineaarne, nonlinear, või kuvada. Kuvatakse suur companion reklaamide reklaamid kaart. Loendis komaeraldusega (ilma tühikuteta) saab määrata rohkem kui üks ad tüüp. Funktsiooni breakID on valikuline identifikaator reklaami jaoks. Funktsiooni timeOffset saate määrata, millal peaks olema kuvatud reklaami. Saate määrata ühel järgmistest viisidest:

1. Aeg-hh:mm:ss või hh:mm:ss.mmm vormingus, kus on .mmm millisekundit. Selle atribuudi väärtust määratakse aeg ajaskaala video algusest ad leheküljepiiri alguseni.
1. Protsendi – n % vormingus kus n on video ajaskaala protsent esitamiseks enne reklaami esitamine
1. Algus ja lõpp – saate määrata, et reklaami peaks olema kuvatud enne või pärast seda, kui video on kuvatud
1. Paigutage – saate määrata ad piirid järjestuse ajastuse ad piirid on teadmata, nagu näiteks reaalajas streaming. Iga ad leheküljepiiri järjestuse on määratud #n vormingus, kus n on täisarv 1 või suurem. 1 tähendab reklaami peaks esitatakse esimesel võimalusel 2 tähendab, et reklaami peaks mängida teine võimalus jne.

<**AdBreak**> elemendi sees võib olla üks <**AdSource**> element. Elemendi <**AdSource**> sisaldab järgmisi atribuute:

1. ID – saate määrata ad allika ID.
1. allowMultipleAds – loogikaväärtus, mis määrab, kas mitme reklaamid saab kuvada ad pausi ajal
1. followRedirects – valikuline kahendväärtus, mis määrab, kui videopleieri peaks au suunab vastuse ad sees

<**AdSource**> elemendi pakub mängija vastuse Tekstisisese ad või vastuse ad viide. See võib olla üks järgmistest elementidest:

- <VASTAdData>näitab, et suur ad vastuse on manustatud faili VMAP
- <AdTagURI>teise süsteemist vastuse ad viitava URI
- <CustomAdData>-mõne suvalise tekstistringi selle respresents suur vastus

Selles näites on määratud vastuse ad rea lisamine <VASTAdData> element, mis sisaldab suur ad vastuse. Muude elementide kohta leiate lisateavet teemast [VMAP](http://www.iab.net/guidelines/508676/digitalvideo/vsuite/vmap).

<**AdBreak**> elemendi võib sisaldada ka üks <**TrackingEvents**> element. <**TrackingEvents**> element võimaldab jälgida algus- või end ka ad leheküljepiiri, või kas ad pausi ajal ilmnes tõrge. Elemendi <**TrackingEvents**> sisaldab ühte või mitut <**jälitus**> elementi, mis määrab jälitus sündmus ja saanud jälituse URI. On võimalik jälgimise sündmuste.

1. breakStart – jälitab ka ad leheküljepiiri algusse
1. breakEnd – jälgida ka ad leheküljepiiri lõppu
1. tõrge – jälitab mis ad pausi ajal ilmnes tõrge

Järgmises näites on kujutatud VMAP faili, mis määrab jälitus sündmused

    <vmap:VMAP xmlns:vmap="http://www.iab.net/vmap-1.0" version="1.0">
      <vmap:AdBreak breakType="linear" breakId="mypre" timeOffset="start">
        <vmap:AdSource allowMultipleAds="true" followRedirects="true" id="1">
          <vmap:VASTData>
            <!--Inline VAST -->
          </vmap:VASTData>
        </vmap:AdSource>
        <vmap:TrackingEvents>
          <vmap:Tracking event="breakStart">
            http://MyServer.com/breakstart.gif
          </vmap:Tracking>
          <vmap:Tracking event="breakend">
            http://MyServer.com/breakend.gif
          </vmap:Tracking>
          <vmap:Tracking event="error">
            http://MyServer.com/error.gif
          </vmap:Tracking>
        </vmap:TrackingEvents>
      </vmap:AdBreak>
    </vmap:VMAP>

<**TrackingEvents**> elemendi ja tema tütardele kohta leiate lisateavet teemast http://iab.org/VMAP.pdf

###<a name="using-a-media-abstract-sequencing-template-mast-file"></a>Meediumi kokkuvõtet, järjestuse mallifail (MAST) abil

MAST faili saate määrata päästikute, mis määratlevad, kui reklaami ei kuvata. Järgmine on näide MAST fail, mis sisaldab päästikute eelnevalt roll ad, keskel roll ad ja pärast roll ad.

    <MAST xsi:schemaLocation="http://openvideoplayer.sf.net/mast http://openvideoplayer.sf.net/mast/mast.xsd" xmlns="http://openvideoplayer.sf.net/mast" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
      <triggers>
        <trigger id="preroll" description="preroll every item"  >
          <startConditions>
            <condition type="event" name="OnItemStart" />
          </startConditions>
          <sources>
            <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
              <sources />
            </source>
          </sources>
        </trigger>
    
        <trigger id="midroll" description="midroll at 15 sec."  >
          <startConditions>
            <condition type="property" name="Position" value="00:00:15.0" operator="GEQ" />
          </startConditions>
          <endConditions>
            <condition type="event" name="OnItemEnd"/>
            <!--This 'resets' the trigger for the next clip-->
          </endConditions>
          <sources>
            <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
              <sources />
            </source>
          </sources>
        </trigger>
    
        <trigger id="postroll" description="postroll"  >
          <startConditions>
            <condition type="event" name="OnItemEnd"/>
          </startConditions>
          <sources>
            <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
              <sources />
            </source>
          </sources>
        </trigger>
      </triggers>
    </MAST>

 

Algab MAST faili lisamine **<MAST>** element, mis sisaldab ühte **<triggers>** element. Funktsiooni <triggers> element, mis sisaldab ühte või mitut **<trigger>** elemente, mis määratlevad, kui reklaami esitada. 

Funktsiooni **<trigger>** element, mis sisaldab soovitud **<startConditions>** element, kus saate määrata, millal reklaami esitamine peaks algama. Funktsiooni **<startConditions>** element, mis sisaldab ühte või mitut <condition> elemente. Kui iga <condition> väärtuseks true käivitamiseks on algatanud või tühistada, sõltuvalt sellest, kas selle <condition> sisalduvad on **< startConditions**> või **<endConditions>** elemendi vastavalt. Kui mitu <condition> elemendid on olemas, neid koheldakse kui peidetud või, põhjustab ükskõik milline tingimus tõene hindamisel päästik algatada. <condition>elementide saate pesastada. Kui lapse <condition> elementide algne, neid koheldakse kui ka peidetud ja kõigi tingimuste peavad olema väärtustatavad true päästik algatada. Funktsiooni <condition> element sisaldab järgmisi atribuute, mis määratlevad tingimus: 

1. **Tüüp** – saate määrata tingimus, sündmus või atribuudi tüüp
1. **nimi** – atribuut või sündmuse hindamisel kasutatav nimi
1. **value** -väärtus, mida hinnatakse atribuut
1. **tehtemärk** – selle toimingu kasutamiseks hindamisel: EQ (võrdsed), NEQ (ei võrdu), GTR (suurem), ≥ (suurem või võrdne), LT (alla), LEQ (väiksem või võrdne) MOD (moodul)

**<endConditions>**sisaldada ka <condition> elemente. Kui tingimus annab tulemiks true päästiku lähtestada. Selle <trigger> sisaldab ka element on <sources> element, mis sisaldab ühte või mitut <source> elemendid. Funktsiooni <source> elementide määratlemine URI ad vastus ja ad vastuse tüüp. Selles näites on esitatud URI suur vastuse. 


    <trigger id="postroll" description="postroll"  >
      <startConditions>
        <condition/>
      </startConditions>
      <sources>
        <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
          <sources />
        </source>
      </sources>
    </trigger>
 

###<a name="using-video-player-ad-interface-definition-vpaid"></a>Video mängija-Ad kasutajaliidese määratlus (VPAID) abil

VPAID on API, mis võimaldab käivitatava ad üksuste videopleieri suhelda. See võimaldab interaktiivne ad kogemusi. Kasutaja saab interaktiivselt kasutada reklaami ja reklaami saavad vastata vaataja poolt võetud. Näiteks reklaami kuvada nupud, mis võimaldavad kasutajal lisateavet ega reklaami pikem versioon. Videopleieri peab toetama VPAID API ja käivitatava ad rakendama API. Kui mängija taotleb ad ad serverist server võib vastata suur vastuse, mis sisaldab VPAID ad.

Executable reklaami luuakse koodi, mida tuleb kasutada käitusaja keskkonnas, nt Adobe Flash™ või JavaScripti, mida saab kasutada veebibrauseris. Kui ka ad server tagastab suur vastuse, mis sisaldab VPAID ad, väärtus on apiFramework atribuuti on <MediaFile> elemendi peab olema "VPAID". See atribuut määrab keskkonnas ad VPAID käivitatava ad. Tippige atribuut peab olema seatud käivitatava, näiteks "rakendus/x-shockwave-flash" või "rakenduse/x-javascript" MIME-tüüpi. Järgmine XML-i koodilõigu kuvatakse soovitud <MediaFile> elemendi suur vastust, mis sisaldab VPAID käivitatava ad. 

    
    <MediaFiles>
       <MediaFile id="1" delivery="progressive" type=”application/x-shockwaveflash”
                  width=”640” height=”480” apiFramework=”VPAID”>
           <!-- CDATA wrapped URI to executable ad -->
       </MediaFile>
    </MediaFiles>
 

Executable reklaami saate lähtestada, kasutades funktsiooni <AdParameters> elemendid on <Linear> või <NonLinear> elementide suur vastuse. Kohta lisateabe saamiseks selle <AdParameters> element, lugege teemat [suur 3.0](http://www.iab.net/media/file/VASTv3.0.pdf). VPAID API kohta leiate lisateavet teemast [VPAID 2.0](http://www.iab.net/media/file/VPAID_2.0_Final_04-10-2012.pdf).

##<a name="implementing-a-windows-or-windows-phone-8-player-with-ad-support"></a>Windowsi või Windows Phone 8 mängija Ad tugi

Microsoft Media platvorm: Mängija Framework Windows 8 ja Windows Phone 8 sisaldab kogu valimi rakendusi, mis näitab, kuidas rakendada videopleieri kasutav raames. Saate alla laadida ja Playeri raames näidised [Playeri Framework Windows 8 ja Windows Phone 8](https://playerframework.codeplex.com).

Lahendus Microsoft.PlayerFramework.Xaml.Samples avamisel kuvatakse arvu projekti kaustad. Reklaami kaust sisaldab ad toega videopleieri loomiseks olulisi proovi kood. Sees reklaami kaust on mitmeid XAML/cs faile, mis näitab, kuidas lisada reklaamid erineval viisil. Järgmises loendis kirjeldatakse iga.

- AdPodPage.xaml näitab, kuidas kuvada ka ad pod.
- AdSchedulingPage.xaml näitab, kuidas plaanida reklaamid.
- FreeWheelPage.xaml näitab, kuidas plaanida reklaamid FreeWheel lisandmooduli abil.
- MastPage.xaml näitab, kuidas plaanida reklaamid MAST faili abil.
- ProgrammaticAdPage.xaml näitab, kuidas programmiliselt ajastada reklaamid video.
- ScheduleClipPage.xaml näitab, kuidas plaanida reklaami ilma suur faili.
- VastLinearCompanionPage.xaml näitab, kuidas lisada lineaarne ja companion ad.
- VastNonLinearPage.xaml näitab, kuidas lisada mittelineaarsete ad.
- VmapPage.xaml näitab, kuidas määrata reklaamid VMAP faili abil.

Proovis kasutab Media Player klassi määratletud Playeri raames. Saate kasutada enamiku näidised lisandmoodulid, mis toetusviisi erinevate ad vastuse lisamine. Valimi ProgrammaticAdPage programmiliselt suhtleb Media Player eksemplari.

###<a name="adpodpage-sample"></a>AdPodPage näidis

See näide kasutab funktsiooni AdSchedulerPlugin määratlemiseks reklaami kuvamiseks. Selles näites on keskel roll reklaami ajastatud sekundi möödumisel esitada. Ad pod (rühma reklaamide kuvamise järjestuses) on määratud suur faili tagastatud ad-serverist. URI suur fail on määratud soovitud <RemoteAdSource> element.

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
    
        <mmppf:MediaPlayer.Plugins>
            <ads:AdSchedulerPlugin>
                <ads:AdSchedulerPlugin.Advertisements>
    
                    <ads:MidrollAdvertisement Time="00:00:05">
                        <ads:MidrollAdvertisement.Source>
                            <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_adpod.xml" Type="vast"/>
                        </ads:MidrollAdvertisement.Source>
                    </ads:MidrollAdvertisement>
    
                </ads:AdSchedulerPlugin.Advertisements>
            </ads:AdSchedulerPlugin>
            <ads:AdHandlerPlugin/>
        </mmppf:MediaPlayer.Plugins>
    </mmppf:MediaPlayer>

Funktsiooni AdSchedulerPlugin kohta leiate lisateavet teemast [reklaami Playeri raames Windows 8 ja Windows Phone 8](http://playerframework.codeplex.com/wikipage?title=Advertising&referringTitle=Windows%208%20Player%20Documentation)

###<a name="adschedulingpage"></a>AdSchedulingPage

See näide kasutab ka selle AdSchedulerPlugin. Koosolekut kavandava kolme reklaamid, eelse roll ad, keskel roll ad ja pärast roll ad. URI, et iga ad VAST on määratud mõne <RemoteAdSource> element.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>
    
                            <ads:PrerollAdvertisement>
                                <ads:PrerollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" Type="vast"/>
                                </ads:PrerollAdvertisement.Source>
                            </ads:PrerollAdvertisement>
    
                            <ads:MidrollAdvertisement Time="00:00:15">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" Type="vast"/>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>
    
                            <ads:PostrollAdvertisement>
                                <ads:PostrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" Type="vast"/>
                                </ads:PostrollAdvertisement.Source>
                            </ads:PostrollAdvertisement>
    
                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>


###<a name="freewheelpage"></a>FreeWheelPage

See näide kasutab FreeWheelPlugin, mis määrab allika atribuut, mis määrab URI, mis viitab SmartXML faili, mis määrab ad sisu kui ka ad plaanimine teabe.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:FreeWheelPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/freewheel.xml"/>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

###<a name="mastpage"></a>MastPage

See näide kasutab MastSchedulerPlugin, mis võimaldab kasutada MAST faili. Allika atribuut määrab MAST faili asukoht.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:MastSchedulerPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/mast.xml" />
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

###<a name="programmaticadpage"></a>ProgrammaticAdPage

See näide programmiliselt suhtleb soovitud Media Player. Faili ProgrammaticAdPage.xaml instantiates soovitud Media Player:

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4"/>

ProgrammaticAdPage.xaml.cs faili loob mõne AdHandlerPlugin lisab TimelineMarker, et määrata kui reklaami tuleks kuvada, ja lisab seejärel soovitud sündmuseohjuri MarkerReached sündmus, mis on suur faili URI täpsustades RemoteAdSource laadib ja seejärel esitatakse selle ad.
    
    public sealed partial class ProgrammaticAdPage : Microsoft.PlayerFramework.Samples.Common.LayoutAwarePage
        {
            AdHandlerPlugin adHandler;
    
            public ProgrammaticAdPage()
            {
                this.InitializeComponent();
                adHandler = new AdHandlerPlugin();
                player.Plugins.Add(new AdHandlerPlugin());
                player.Markers.Add(new TimelineMarker() { Time = TimeSpan.FromSeconds(5), Type = "myAd" });
                player.MarkerReached += pf_MarkerReached;
            }
    
            async void pf_MarkerReached(object sender, TimelineMarkerRoutedEventArgs e)
            {
                if (e.Marker.Type == "myAd")
                {
                    var adSource = new RemoteAdSource() { Type = VastAdPayloadHandler.AdType, Uri = new Uri("http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml") };
                    //var adSource = new AdSource() { Type = DocumentAdPayloadHandler.AdType, Payload = SampleAdDocument };
                    var progress = new Progress<AdStatus>();
                    try
                    {
                        await player.PlayAd(adSource, progress, CancellationToken.None);
                    }
                    catch { /* ignore */ }
                }
            }

###<a name="scheduleclippage"></a>ScheduleClipPage

See näide kasutab selle AdSchedulerPlugin kavandada, määrates .wmv faili, mis sisaldab reklaami keskel roll ad.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.cloudapp.net/html5/media/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>
    
                            <ads:MidrollAdvertisement Time="00:00:05">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:AdSource Type="clip">
                                        <ads:AdSource.Payload>
                                            <ads:ClipAdPayload MediaSource="http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_2_000_700_4x3.wmv" MimeType="video/x-ms-wmv" />
                                        </ads:AdSource.Payload>
                                    </ads:AdSource>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>
    
                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

###<a name="vastlinearcompanionpage"></a>VastLinearCompanionPage

See näide illustreerib, kuidas funktsiooni AdSchedulerPlugin abil saate ajastada keskel roll lineaarse ad koos companion reklaami. Funktsiooni <RemoteAdSource> element määrab suur faili asukoht.
    
    <mmppf:MediaPlayer Grid.Row="1"  x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>
    
                            <ads:MidrollAdvertisement Time="00:00:05">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear_companions.xml" Type="vast"/>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>
    
                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

###<a name="vastlinearnonlinearpage"></a>VastLinearNonLinearPage

See näide kasutab AdSchedulerPlugin plaanida lineaarne ja mittelineaarsete ad. SUUR faili asukoht on määratud koos selle <RemoteAdSource> element.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>
    
                            <ads:MidrollAdvertisement Time="00:00:05">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear_nonlinear.xml" Type="vast"/>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>
                            
                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

###<a name="vmappage"></a>VMAPPage

See näidised kasutab funktsiooni VmapSchedulerPlugin plaanida reklaamid VMAP faili abil. Allika atribuut on määratud URI VMAP faili selle <VmapSchedulerPlugin> element.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:VmapSchedulerPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/vmap.xml"/>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

##<a name="implementing-an-ios-video-player-with-ad-support"></a>IOS Video Playerit Ad tugiteenuste rakendamine


Microsoft Media platvorm: Mängija Framework iOS-i, mis sisaldab kogu valimi rakendusi, mis näitab, kuidas rakendada videopleieri kasutav raames. Saate alla laadida ja Playeri raames näidised [Azure Media Playeri raames](https://github.com/Azure/azure-media-player-framework). Github leheküljel on viki, mis sisaldab täiendavat teavet mängija raamistiku lingi ja Playeri valimi Sissejuhatus: [Azure Media Playeri viki](https://github.com/Azure/azure-media-player-framework/wiki/How-to-use-Azure-media-player-framework).


###<a name="scheduling-ads-with-vmap"></a>Reklaamid VMAP plaanimine

Järgmises näites on kujutatud ajastamine reklaamid VMAP faili abil.

    // How to schedule an Ad using VMAP.
    //First download the VMAP manifest
    
    if (![framework.adResolver downloadManifest:&manifest withURL:[NSURL URLWithString:@"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVMAP.xml"]])
            {
                [self logFrameworkError];
            }
            else
            {
                // Schedule a list of ads using the downloaded VMAP manifest
                if (![framework scheduleVMAPWithManifest:manifest])
                {
                    [self logFrameworkError];
                }          
            }

###<a name="scheduling-ads-with-vast"></a>Reklaamid plaanimist suur

Järgmises näites kujutatakse hilinenud sidumine suur ad plaanimine.
    
    //Example:3 How to schedule a late binding VAST ad.
    // set the start time for the ad
    adLinearTime.startTime = 13;
    adLinearTime.duration = 0;
    // Specify the URI of the VAST file
    NSString *vastAd1=@"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVAST.xml";
    // Create an AdInfo object
     AdInfo *vastAdInfo1 = [[[AdInfo alloc] init] autorelease];
    // set URL to VAST file
    vastAdInfo1.clipURL = [NSURL URLWithString:vastAd1];
    // set running time of ad
    vastAdInfo1.mediaTime = [[[MediaTime alloc] init] autorelease];
    vastAdInfo1.mediaTime.clipBeginMediaTime = 0;
    vastAdInfo1.mediaTime.clipEndMediaTime = 10;
    vastAdInfo1.policy = @"policy for late binding VAST";
    // specify ad type
    vastAdInfo1.type = AdType_Midroll;
    vastAdInfo1.appendTo=-1;
    adIndex = 0;
    // schedule ad
    if (![framework scheduleClip:vastAdInfo1 atTime:adLinearTime forType:PlaylistEntryType_VAST andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }
         
   Järgmises näites kujutatakse ennetähtaegse sidumine suur ad plaanimine.
Näide: 4 ajakava ennetähtaegse sidumine suur ad //Download on VAST failide kas (! [ framework.adResolver downloadManifest: & manifest withURL: [NSURL URLWithString:@"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVAST.xml"]]) {[ise logFrameworkError];} veel {adLinearTime.startTime = 7; adLinearTime.duration = 0;
        
        // Create AdInfo instance
        AdInfo *vastAdInfo2 = [[[AdInfo alloc] init] autorelease];
        vastAdInfo2.mediaTime = [[[MediaTime alloc] init] autorelease];
        vastAdInfo2.policy = @"policy for early binding VAST";
        // specify ad type
        vastAdInfo2.type = AdType_Midroll;
        vastAdInfo2.appendTo=-1;
        // schedule ad
        if (![framework scheduleVASTClip:vastAdInfo2 withManifest:manifest atTime:adLinearTime andGetClipId:&adIndex])
        {
            [self logFrameworkError];
        }
    }

Järgmises näites kujutatakse lisamiseks reklaami töötlemata lõikamine redigeerimine (PKT) abil

    //Example:1 How to use RCE.
    // specify manifest for ad content
    NSString *secondContent=@"http://wamsblureg001orig-hs.cloudapp.net/6651424c-a9d1-419b-895c-6993f0f48a26/The%20making%20of%20Microsoft%20Surface-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    
    // specify ad length
    mediaTime.currentPlaybackPosition = 0;
    mediaTime.clipBeginMediaTime = 0;
    mediaTime.clipEndMediaTime = 80;
    // append ad content
    if (![framework appendContentClip:[NSURL URLWithString:secondContent] withMediaTime:mediaTime andGetClipId:&clipId])
    {
        [self logFrameworkError];
    }

Järgmises näites on kujutatud ka ad pod ajastamine.

    //Example:5 Schedule an ad Pod.
    // Set start time for ad
    adLinearTime.startTime = 23;
    adLinearTime.duration = 0;
    
    // Specify URL to content
    NSString *adpodSt1=@"https://portalvhdsq3m25bf47d15c.blob.core.windows.net/asset-e47b43fd-05dc-4587-ac87-5916439ad07f/Windows%208_%20Cliffjumpers.mp4?st=2012-11-28T16%3A31%3A57Z&se=2014-11-28T16%3A31%3A57Z&sr=c&si=2a6dbb1e-f906-4187-a3d3-7e517192cbd0&sig=qrXYZBekqlbbYKqwovxzaVZNLv9cgyINgMazSCbdrfU%3D";
    // Create an AdInfo instance
    AdInfo *adpodInfo1 = [[[AdInfo alloc] init] autorelease];
    // set URI to ad content
    adpodInfo1.clipURL = [NSURL URLWithString:adpodSt1];
    // Set ad running time
    adpodInfo1.mediaTime = [[[MediaTime alloc] init] autorelease];
    adpodInfo1.mediaTime.clipBeginMediaTime = 0;
    adpodInfo1.mediaTime.clipEndMediaTime = 17;
    adpodInfo1.policy = @"policy for ad pod 1";
    // Set ad type
    adpodInfo1.type = AdType_Midroll;
    adpodInfo1.appendTo=-1;
    adIndex = 0;
    // Schedule ad
    if (![framework scheduleClip:adpodInfo1 atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

Järgmises näites on kujutatud ajastamine-sticky tagumine roll ad. Kui sõltumata sellest, mis tahes keda vaataja sooritab-sticky ad ainult esitatakse.
    
    //Example:6 Schedule a single non sticky mid roll Ad
    // specify URL to content
    NSString *oneTimeAd=@"http://wamsblureg001orig-hs.cloudapp.net/5389c0c5-340f-48d7-90bc-0aab664e5f02/Windows%208_%20You%20and%20Me%20Together-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    
    // create an AdInfo instance
    AdInfo *oneTimeInfo = [[[AdInfo alloc] init] autorelease];
    // set URL of ad
    oneTimeInfo.clipURL = [NSURL URLWithString:oneTimeAd];
    oneTimeInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    oneTimeInfo.mediaTime.clipBeginMediaTime = 0;
    oneTimeInfo.mediaTime.clipEndMediaTime = -1;
    oneTimeInfo.policy = @"policy for one-time ad";
    // set ad start time
    adLinearTime.startTime = 43;
    adLinearTime.duration = 0;
    // set ad type
    oneTimeInfo.type = AdType_Midroll;
    // non sticky ad
    oneTimeInfo.deleteAfterPlayed = YES;
    // schedule ad
    if (![framework scheduleClip:oneTimeInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

Järgmises näites on kujutatud ajastamine sticky keskel roll ad. Sticky ad kuvatakse iga kord, kui määratud punkti video ajaskaalalt on jõudnud.

    //Example:7 Schedule a single sticky mid roll Ad
    NSString *stickyAd=@"http://wamsblureg001orig-hs.cloudapp.net/2e4e7d1f-b72a-4994-a406-810c796fc4fc/The%20Surface%20Movement-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    // create AdInfo instance
    AdInfo *stickyAdInfo = [[[AdInfo alloc] init] autorelease];
    // set URI to ad
    stickyAdInfo.clipURL = [NSURL URLWithString:stickyAd];
    stickyAdInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    stickyAdInfo.mediaTime.clipBeginMediaTime = 0;
    stickyAdInfo.mediaTime.clipEndMediaTime = 15;
    stickyAdInfo.policy = @"policy for sticky mid-roll ad";
    // set ad start time
    adLinearTime.startTime = 64;
    adLinearTime.duration = 0;
    // set ad type
    stickyAdInfo.type = AdType_Midroll;
    stickyAdInfo.deleteAfterPlayed = NO;
    // schedule ad
    if (![framework scheduleClip:stickyAdInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }


Järgmises näites kujutatakse kavandamine pärast roll ad.

    //Example:8 Schedule Post Roll Ad
    NSString *postAdURLString=@"http://wamsblureg001orig-hs.cloudapp.net/aa152d7f-3c54-487b-ba07-a58e0e33280b/wp-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    // create AdInfo instance
    AdInfo *postAdInfo = [[[AdInfo alloc] init] autorelease];
    postAdInfo.clipURL = [NSURL URLWithString:postAdURLString];
    postAdInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    postAdInfo.mediaTime.clipBeginMediaTime = 0;
    // set ad length
    postAdInfo.mediaTime.clipEndMediaTime = 45;
    postAdInfo.policy = @"policy for post-roll ad";
    // set ad type
    postAdInfo.type = AdType_Postroll;
    adLinearTime.duration = 0;
    if (![framework scheduleClip:postAdInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

Järgmises näites kujutatakse eelse roll ad plaanimine.
    
    //Example:9 Schedule Pre Roll Ad
    NSString *adURLString = @"http://wamsblureg001orig-hs.cloudapp.net/2e4e7d1f-b72a-4994-a406-810c796fc4fc/The%20Surface%20Movement-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    AdInfo *adInfo = [[[AdInfo alloc] init] autorelease];
    adInfo.clipURL = [NSURL URLWithString:adURLString];
    adInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    adInfo.mediaTime.currentPlaybackPosition = 0;
    adInfo.mediaTime.clipBeginMediaTime = 40; //You could play a portion of an Ad. Yeh!
    adInfo.mediaTime.clipEndMediaTime = 59;
    adInfo.policy = @"policy for pre-roll ad";
    adInfo.appendTo = -1;
    adInfo.type = AdType_Preroll;
    adLinearTime.duration = 0;
    // schedule ad
    if (![framework scheduleClip:adInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

Järgmises näites kujutatakse keskel roll ülekatet ad plaanimine.
    
    // Example10: Schedule a Mid Roll overlay Ad
    NSString *adURLString = @"https://portalvhdsq3m25bf47d15c.blob.core.windows.net/asset-e47b43fd-05dc-4587-ac87-5916439ad07f/Windows%208_%20Cliffjumpers.mp4?st=2012-11-28T16%3A31%3A57Z&se=2014-11-28T16%3A31%3A57Z&sr=c&si=2a6dbb1e-f906-4187-a3d3-7e517192cbd0&sig=qrXYZBekqlbbYKqwovxzaVZNLv9cgyINgMazSCbdrfU%3D";
    //Create AdInfo instance
    AdInfo *adInfo = [[[AdInfo alloc] init] autorelease];
    adInfo.clipURL = [NSURL URLWithString:adURLString];
    adInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    adInfo.mediaTime.currentPlaybackPosition = 0;
    adInfo.mediaTime.clipBeginMediaTime = 0;
    // specify ad length
    adInfo.mediaTime.clipEndMediaTime = 20;
    adInfo.policy = @"policy for mid-roll overlay ad";
    adInfo.appendTo = -1;
    // specify ad type
    adInfo.type = AdType_Midroll;
    // specify ad start time & duration
    adLinearTime.startTime = 300;
    adLinearTime.duration = 20;
    // schedule ad            if (![framework scheduleClip:adInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }



##<a name="media-services-learning-paths"></a>Media Servicesi Õppeteed

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

 
##<a name="see-also"></a>Vt ka

[Videopleieri rakenduste arendajatele.](media-services-develop-video-players.md)
