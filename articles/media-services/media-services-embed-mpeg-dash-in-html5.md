<properties 
    pageTitle="MPEG-kriips kohandatava Streaming Video manustamine in HTML5 taotluse DASH.js | Microsoft Azure'i" 
    description="See teema näitab, kuidas MPEG-KRIIPSJOONE kohandatava Streaming Video manustamine in HTML5 taotluse DASH.js." 
    authors="Juliako" 
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
    ms.date="09/26/2016" 
    ms.author="juliako"/>


#<a name="embedding-a-mpeg-dash-adaptive-streaming-video-in-an-html5-application-with-dashjs"></a>In HTML5 taotluse DASH.js MPEG-kriips kohandatava Streaming Video manustamine

##<a name="overview"></a>Ülevaade

MPEG-kriips on ISO standard kohandatava streaming videosisu, mis pakub neile, kes soovivad pakkuda kõrge kvaliteediga, kohandatava video voogesitus väljundi märkimisväärsed eelised. MPEG mõttekriipsuga automaatselt langeb videovoo määratluse lower kui võrguühendus muutub ülekoormatud. See vähendab nägemine "peatatud" video ajal mängija allalaaditavate failide esitamiseks (ehk puhverdamine) järgmise sekundi vaataja tõenäosuse. Kuna võrgu ülekoormuse vähendab, videopleieri omakorda naaske kõrgema kvaliteediga voo. Selle võimaluse kohandada läbilaskevõime, mis on nõutav, on tulemuseks ka kiirem algusaeg video. Mida tähendab, et esimese paari sekundi saab esitada kiire alla laadida alumise kvaliteedi lõigu ja samm kuni kõrgema kvaliteediga üks kord piisavalt sisu puhverdatud siis.

Dash.js on avatud allika MPEG-KRIIPSJOONE video mängija kirjutatud JavaScript. Selle eesmärk on esitada robustne, mitu platvormi mängija, mis võib vabalt kasutada rakendusi, mis nõuavad videoesitus. Pakub MPEG-kriips videoesitust brauserit, mis toetab täna on W3C meediumi allika laiendid (MSE), Chrome'i, Microsoft Edge ja IE11 (muudes brauserites on näidatud kavatsust toetada MSE). DASH.js kohta leiate lisateavet teemast js GitHub dash.js hoidla.


##<a name="creating-a-browser-based-streaming-video-player"></a>Brauseripõhine streaming videopleieri loomine

Luua lihtsa veebileht, mis kuvatakse koos kollaste videopleieri juhtelementide selline Esita, Peata, tagasikerimine jne, peate:

1. HTML-lehele loomine
1. Video sildi lisamine
1. Dash.js mängija lisamine
1. Playeri käivitamine
1. CSS-i laadi lisamine
1. Brauserit, mis rakendab MSE tulemuste kuvamine

Lähtestamisel mängija saab valmis vaid üksikud rida JavaScripti koodi. Dash.js kasutamisel väga on mis lihtne MPEG-KRIIPSJOONE video manustamine oma veebipõhine rakendused.

##<a name="creating-the-html-page"></a>HTML-i lehe loomine

Kõigepealt tuleb luua standard HTML-i lehte, mis sisaldab **video** elemendi, Salvesta see fail nimega basicPlayer.html, nagu on näidatud järgmises näites:

    <!DOCTYPE html>
    <html>
      <head><title>Adaptive Streaming in HTML5</title></head>
      <body>
        <h1>Adaptive Streaming with HTML5</h1>
        <video id="videoplayer" controls></video>
      </body>
    </html>

##<a name="adding-the-dashjs-player"></a>DASH.js mängija lisamine

Dash.js viide rakendamine rakenduse lisamiseks peate ostke 1,0 release dash.js projekti dash.all.js fail. See peaks olema salvestatud JavaScripti kausta rakenduse. See on mugavuse fail, mis tõmbab kokku kõik vajalikud dash.js kood ühte faili. Kui teil on, vaadake dash.js hoidla ümber, te leida üksikuid faile, koodi ja palju muud, kuid kui kõik, mida soovite teha on kasutada dash.js, siis dash.all.js fail on teil vaja järgmist.

Dash.js mängija lisamiseks rakenduste skripti sildi lisamine basicPlayer.html pea osa:

    <!-- DASH-AVC/265 reference implementation -->
    < script src="js/dash.all.js"></script>


Järgmisena looge funktsiooni lähtestamine mängija lehe laadimise ajal. Pärast rida, mis teil laadida dash.all.js järgmist skripti lisamiseks tehke järgmist.

    <script>
    // setup the video element and attach it to the Dash player
    function setupVideo() {
      var url = "http://wams.edgesuite.net/media/MPTExpressionData02/BigBuckBunny_1080p24_IYUV_2ch.ism/manifest(format=mpd-time-csf)";
      var context = new Dash.di.DashContext();
      var player = new MediaPlayer(context);
                      player.startup();
                      player.attachView(document.querySelector("#videoplayer"));
                      player.attachSource(url);
    }
    </script>

See funktsioon loob esmalt soovitud DashContext. Seda kasutatakse konfigureerida rakendus teatud käitusaja keskkonnas. Tehniline seisukohast see määratleb tunnid, et sõltuvus süsti raames peaks kasutama, kui rakenduse loomine. Enamikul juhtudel saate kasutada Dash.di.DashContext.

Järgmiseks väärtustada esmane ainekursuse dash.js raamistiku Media Player. See tund sisaldab core vaja võimalust esitada ja viige, haldab video elemendi seos ja ka haldab tõlgendamise Media esitluse kirjeldus (MPD) faili, mis kirjeldab video esitada.

Funktsiooni startup() Media Player klassi nimetatakse tagamaks, et mängija on video esitamiseks valmis. Muu hulgas see funktsioon tagab, et kõik vajalikud tunnid (konteksti poolt määratletud) on laaditud. Kui mängija on valmis, saate selle funktsiooniga attachView() manustada video element. See võimaldab soovitud Media Player annavad videovoo elemendi ja ka esitust vastavalt vajadusele.

Liigu MPD faili URL on Media Player nii, et see teab video esitamiseks on tõenäoline. Äsja loodud setupVideo() funktsiooni on vaja käivitada, kui leht on täielikult laaditud. Tehke seda allalaadimine sündmus keha elemendi abil. Muuda oma <body> elemendi:

    <body onload="setupVideo()">

Lõpetuseks, määrata CSS-i abil video elemendi suurust. Kohandatavad streaming keskkonnas, see on eriti oluline, kuna esitatava video suurust võib muutuda taasesitus kohandub võrgu tingimuste muutmine. Selle lihtsa demo lihtsalt jõustada video elemendi on saadaval brauseriakna 80%, lisades järgmised CSS-i pea lehe osa.
    
    <style>
    video {
      width: 80%;
      height: 80%;
    }
    </style>

##<a name="playing-a-video"></a>Video esitamine

Video esitamine osutage brauseri basicPlayback.html faili ja klõpsake käsku Esita kuvatakse videopleieri.


##<a name="media-services-learning-paths"></a>Media Servicesi Õppeteed

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Vt ka

[Videopleieri rakenduste arendajatele.](media-services-develop-video-players.md)

[GitHub dash.js hoidla](https://github.com/Dash-Industry-Forum/dash.js) 
