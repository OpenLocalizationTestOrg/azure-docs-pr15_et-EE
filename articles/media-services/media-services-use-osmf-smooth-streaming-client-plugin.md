<properties 
    pageTitle="Sujuva voogesituse lisandmooduli avatud allika meediumi Framework" 
    description="Saate teada, kuidas kasutada Azure Media teenuste sujuva voogesituse lisandmooduli Adobe avatud allika Media raames." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"
    ms.author="juliako"/>


# <a name="how-to-use-the-microsoft-smooth-streaming-plugin-for-the-adobe-open-source-media-framework"></a>Kuidas kasutada Microsoft tõrgeteta Streaming lisandmooduli Adobe avatud allika meediumi raames

##<a name="overview"></a>Ülevaade


Avatud allika meediumi Framework 2.0 (SS jaoks OSMF) Microsoft sujuva voogesituse lisandmooduli laiendab OSMF vaikimisi võimaluste ja lisab Microsoft sujuva voogesituse sisu taasesitus uute ja olemasolevate OSMF mängijad. Selle lisandmooduli lisab sujuva voogesituse taasesituse funktsioonid Strobe meediumi taasesituse (SMP).

SS OSMF sisaldab lisandmooduli kaks versiooni:

- Staatilise sujuva voogesituse lisandmooduli OSMF (.swc)

- Dünaamiliste sujuva voogesituse lisandmooduli OSMF (.swf)

Selle dokumendi eeldab, et lugeja on üldine tundma OSMF ja OSMF lisandmoodulid. OSMF kohta lisateabe saamiseks lugege dokumentatsiooni [ametlik OSMF leht](http://osmf.org/).

###<a name="smooth-streaming-plugin-for-osmf-20"></a>Sujuv Streaming lisandmooduli OSMF 2.0

Selle lisandmooduli toetab laadimise ja taasesitus nõudmisel sujuva voogesituse sisu järgmisi funktsioone.

- Nõudmisel sujuva voogesituse Taasesita (Esita, Peata, otsida, Peata)
- Reaalajas sujuva voogesituse taasesitus (Esita)
- Reaalajas DVR funktsioonid (paus, otsida, DVR Taasesita, minge Live)
- Video-ja helikodekite - H.264 tugi
- Heli kodekite - AAC tugi
- Mitme heli keele vahetamine koos OSMF sisseehitatud API-d
- Max taasesituse kvaliteedi valiku OSMF sisseehitatud API-d
- Külghaagisega suletud pealdiste OSMF pealdised lisandmoodul
- Adobe&reg; Flash&reg; mängija 11.4 või uuem versioon.
- See versioon toetab ainult OSMF 2.0.

## <a name="supported-features-and-known-issues"></a>Toetatud funktsioonid ja teadaolevad probleemid

Toetatud funktsioonid, Mittetoetatavaid funktsioone ja teadaolevate probleemide täieliku loendi leiate [selles dokumendis](http://download.microsoft.com/download/3/1/B/31B63D97-574E-4A8D-BF8D-170744181724/Smooth_Streaming_Plugin_for_OSMF.pdf).


## <a name="loading-the-plugin"></a>Selle lisandmooduli laadimine
OSMF lisandmoodulid laaditakse staatiliselt (ajal kompileerida) või dünaamiliselt (käitusajal). Sujuva voogesituse OSMF alla laadida lisandmooduli sisaldab nii dünaamiline ja staatiline versioonid.

- Staatilise laadimise: laadimine staatiliselt, staatilise teegifail (SWC) on nõutav. Staatilise lisandmoodulid lisatakse kompileerida ajal projektide ja Ühenda sees lõplik väljundfail viitena.

- Dünaamiliste laadimise: dünaamiliselt laadimiseks on vaja precompiled (SWF) faili. Dünaamiliste lisandmoodulid on laaditud pikkus ja projekti väljundisse kaasatud. (Kompileeritud väljund) Dünaamiliste lisandmoodulid saab laadida Protokollid HTTP ja faili abil.

Staatiline ja dünaamiline laadimise kohta leiate lisateavet teemast ametlik [OSMF lisandmooduli lehe](http://osmf.org/dev/osmf/OtherPDFs/osmf_plugin_dev_guide.pdf).

###<a name="ss-for-osmf-static-loading"></a>SS OSMF staatilise laadimise
Allpool koodilõigu näitab, kuidas staatiliselt jaoks OSMF SS lisandmooduli laadimine ja OSMF MediaFactory klassi abil lihtsa videot. Enne sh OSMF koodi SS, veenduge sisaldab projekti viite "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swc" staatilise lisandmoodul.

```
package 
{
    
    import com.microsoft.azure.media.AdaptiveStreamingPluginInfo;
    
    import flash.display.*;
    import org.osmf.media.*;
    import org.osmf.containers.MediaContainer;
    import org.osmf.events.MediaErrorEvent;
    import org.osmf.events.MediaFactoryEvent;
    import org.osmf.events.MediaPlayerStateChangeEvent;
    import org.osmf.layout.*;
    
    
    
    [SWF(width="1024", height="768", backgroundColor='#405050', frameRate="25")]
    public class TestPlayer extends Sprite
    {        
        public var _container:MediaContainer;
        public var _mediaFactory:DefaultMediaFactory;
        private var _mediaPlayerSprite:MediaPlayerSprite;
        

        public function TestPlayer( )
        {
            stage.quality = StageQuality.HIGH;

            initMediaPlayer();

        }
    
        private function initMediaPlayer():void
        {
        
            // Create the container (sprite) for managing display and layout
            _mediaPlayerSprite = new MediaPlayerSprite();    
            _mediaPlayerSprite.addEventListener(MediaErrorEvent.MEDIA_ERROR, onPlayerFailed);
            _mediaPlayerSprite.addEventListener(MediaPlayerStateChangeEvent.MEDIA_PLAYER_STATE_CHANGE, onPlayerStateChange);
            _mediaPlayerSprite.scaleMode = ScaleMode.NONE;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;
            //Adds the container to the stage
            addChild(_mediaPlayerSprite);
            
            // Create a mediafactory instance
            _mediaFactory = new DefaultMediaFactory();
            
            // Add the listeners for PLUGIN_LOADING
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD,onPluginLoaded);
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD_ERROR, onPluginLoadFailed );
            
            // Load the plugin class 
            loadAdaptiveStreamingPlugin( );  
            
        }
        
        private function loadAdaptiveStreamingPlugin( ):void
        {
            var pluginResource:MediaResourceBase;
            
            pluginResource = new PluginInfoResource(new AdaptiveStreamingPluginInfo( )); 
            _mediaFactory.loadPlugin( pluginResource ); 
        }
        
        private function onPluginLoaded( event:MediaFactoryEvent ):void
        {
            // The plugin is loaded successfully.
            // Your web server needs to host a valid crossdomain.xml file to allow plugin to download Smooth Streaming files.
        loadMediaSource("http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest")
        
        }
        
        private function onPluginLoadFailed( event:MediaFactoryEvent ):void
        {
            // The plugin is failed to load ...
        }
        
        
        private function onPlayerStateChange(event:MediaPlayerStateChangeEvent) : void
        {
            var state:String;
            
            state =  event.state;
            
            switch (state)
            {
                case MediaPlayerState.LOADING: 
                    
                    // A new source is started to load.
                    
                    break;
                
                case  MediaPlayerState.READY :   
                    // Add code to deal with Player Ready when it is hit the first load after a source is loaded. 
                    
                    break;
                
                case MediaPlayerState.BUFFERING :
                    
                    break;
                
                case  MediaPlayerState.PAUSED :
                    break;      
                // other states ...          
            }
        }
        
        private function onPlayerFailed(event:MediaErrorEvent) : void
        {
            // Media Player is failed .           
        }
        
        private function loadMediaSource(sourceURL : String):void 
        {
            // Take an URL of SmoothStreamingSource's manifest and add it to the page.
            
            var resource:URLResource= new URLResource( sourceURL );
            
            var element:MediaElement = _mediaFactory.createMediaElement( resource );
            _mediaPlayerSprite.scaleMode = ScaleMode.LETTERBOX;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;
            
            // Add the media element
            _mediaPlayerSprite.media = element;
        }     
        
    }
}
```


###<a name="ss-for-osmf-dynamic-loading"></a>SS OSMF dünaamiline laadimise

Allpool koodilõigu näitab, kuidas dünaamiliselt jaoks OSMF SS lisandmooduli laadimine ja esitada põhiline video OSMF MediaFactory klassi abil. Enne sh OSMF koodi SS, kopeerimine "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf" dünaamiline lisandmooduli projekti kausta, kui soovite laadida faili protokolli abil või kopeerige jaotises veebiserverisse HTTP koormus. Ei ole vaja kaasata projekti viited "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swc".

 
paketi {}
    
    import flash.display.*;
    import org.osmf.media.*;
    import org.osmf.containers.MediaContainer;
    import org.osmf.events.MediaErrorEvent;
    import org.osmf.events.MediaFactoryEvent;
    import org.osmf.events.MediaPlayerStateChangeEvent;
    import org.osmf.layout.*;
    import flash.events.Event;
    import flash.system.Capabilities;

    
    //Sets the size of the SWF
    
    [SWF(width="1024", height="768", backgroundColor='#405050', frameRate="25")]
    public class TestPlayer extends Sprite
    {        
        public var _container:MediaContainer;
        public var _mediaFactory:DefaultMediaFactory;
        private var _mediaPlayerSprite:MediaPlayerSprite;
        
        
        public function TestPlayer( )
        {
            stage.quality = StageQuality.HIGH;
            initMediaPlayer();
        }
        
        private function initMediaPlayer():void
        {
            
            // Create the container (sprite) for managing display and layout
            _mediaPlayerSprite = new MediaPlayerSprite();    
            _mediaPlayerSprite.addEventListener(MediaErrorEvent.MEDIA_ERROR, onPlayerFailed);
            _mediaPlayerSprite.addEventListener(MediaPlayerStateChangeEvent.MEDIA_PLAYER_STATE_CHANGE, onPlayerStateChange);

            //Adds the container to the stage
            addChild(_mediaPlayerSprite);
            
            // Create a mediafactory instance
            _mediaFactory = new DefaultMediaFactory();
            
            // Add the listeners for PLUGIN_LOADING
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD,onPluginLoaded);
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD_ERROR, onPluginLoadFailed );
            
            // Load the plugin class 
            loadAdaptiveStreamingPlugin( );  
            
        }
        
        private function loadAdaptiveStreamingPlugin( ):void
        {
            var pluginResource:MediaResourceBase;
            var adaptiveStreamingPluginUrl:String;

            // Your dynamic plugin web server needs to host a valid crossdomain.xml file to allow loading plugins.

            adaptiveStreamingPluginUrl = "http://yourdomain/MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf";
            pluginResource = new URLResource(adaptiveStreamingPluginUrl);
            _mediaFactory.loadPlugin( pluginResource ); 

        }
        
        private function onPluginLoaded( event:MediaFactoryEvent ):void
        {
            // The plugin is loaded successfully.

            // Your web server needs to host a valid crossdomain.xml file to allow plugin to download Smooth Streaming files.

    loadMediaSource("http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest")
        }
        
        private function onPluginLoadFailed( event:MediaFactoryEvent ):void
        {
            // The plugin is failed to load ...
        }
        
        
        private function onPlayerStateChange(event:MediaPlayerStateChangeEvent) : void
        {
            var state:String;
            
            state =  event.state;
            
            switch (state)
            {
                case MediaPlayerState.LOADING: 
                    
                    // A new source is started to load.
                    
                    break;
                
                case  MediaPlayerState.READY :   
                    // Add code to deal with Player Ready when it is hit the first load after a source is loaded. 
                    
                    break;
                
                case MediaPlayerState.BUFFERING :
                    
                    break;
                
                case  MediaPlayerState.PAUSED :
                    break;      
                // other states ...          
            }
        }
        
        private function onPlayerFailed(event:MediaErrorEvent) : void
        {
            // Media Player is failed .           
        }
        
        private function loadMediaSource(sourceURL : String):void 
        {
            // Take an URL of SmoothStreamingSource's manifest and add it to the page.
            
            var resource:URLResource= new URLResource( sourceURL );
            
            var element:MediaElement = _mediaFactory.createMediaElement( resource );
            _mediaPlayerSprite.scaleMode = ScaleMode.LETTERBOX;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;
            // Add the media element
            _mediaPlayerSprite.media = element;
        }     
        
    }
}

##<a name="strobe-media--playback-with-the-ss-odmf-dynamic-plugin"></a>Strobe meediumi taasesituse SS ODMF dünaamiline lisandmooduli abil

Sujuva voogesituse OSMF dünaamiline lisandmooduli ühildub [Strobe meediumi taasesituse Kulupõhisusele](http://osmf.org/strobe_mediaplayback.html). Saate lisada sujuva voogesituse sisu taasesitus SMP SS OSMF lisandmooduli. Selleks kopeerige "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf" jaotises veebiserverisse HTTP koormus täitke järgmised juhised.

1.  Liikuge [lehe Strobe meediumi taasesituse häälestamine](http://osmf.org/dev/2.0gm/setup.html). 
2.  Määrake src sujuva voogesituse andmeallikaga (nt http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest) 
3.  Soovitud konfiguratsioon muudatused ja klõpsake käsku Eelvaade ja värskendus.
 
    **Märkus** Oma sisu veebiserver peab kehtiv crossdomain.xml. 
4.  Kopeerige ja kleepige kood lihtsa HTML-i lehele abil oma lemmik tekstiredaktoris, nagu järgmises näites:



        <html>
        <body>
        <object width="920" height="640"> 
        <param name="movie" value="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf"></param>
        <param name="flashvars" value="src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest &autoPlay=true"></param>
        <param name="allowFullScreen" value="true"></param>
        <param name="allowscriptaccess" value="always"></param>
        <param name="wmode" value="direct"></param>
        <embed src="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf" 
            type="application/x-shockwave-flash" 
            allowscriptaccess="always" 
            allowfullscreen="true" 
            wmode="direct" 
            width="920" 
            height="640" 
            flashvars=" src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest&autoPlay=true">
        </embed>
        </object>
        </body>
        </html>



5. Sujuva voogesituse OSMF lisandmooduli manustamiskood ja salvestada.

        <html>
        <object width="920" height="640"> 
        <param name="movie" value="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf"></param>
        <param name="flashvars" value="src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest&autoPlay=true&plugin_AdaptiveStreamingPlugin=http://yourdomain/MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf&AdaptiveStreamingPlugin_retryLive=true&AdaptiveStreamingPlugin_retryInterval=10"></param>
        <param name="allowFullScreen" value="true"></param>
        <param name="allowscriptaccess" value="always"></param>
        <param name="wmode" value="direct"></param>
        <embed src="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf" 
            type="application/x-shockwave-flash" 
            allowscriptaccess="always" 
            allowfullscreen="true" 
            wmode="direct" 
            width="920" 
            height="640" 
            flashvars="src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest&autoPlay=true&plugin_AdaptiveStreamingPlugin=http://yourdomain/MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf&AdaptiveStreamingPlugin_retryLive=true&AdaptiveStreamingPlugin_retryInterval=10">
        </embed>
        </object>
        </html>


6.  Teie HTML-i lehe salvestamine ja avaldamine veebiserverisse. Liikuge sirvides avaldatud veebilehele, kasutades oma lemmik Flash&reg; mängija lubatud Interneti-brauser (Internet Explorer, Chrome, Firefox, jne).
7.  Nautige sujuva voogesituse sisu sees Adobe&reg; Flash&reg; mängija.

Üldine OSMF arendamise kohta leiate lisateavet teemast ametlik [OSMF arengu lehe](http://osmf.org/resources.html).

##<a name="media-services-learning-paths"></a>Media Servicesi Õppeteed

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



##<a name="see-also"></a>Vt ka

[Microsoft kohandatava Streaming lisandmooduli OSMF värskendamine](https://azure.microsoft.com/blog/2014/10/27/microsoft-adaptive-streaming-plugin-for-osmf-update/) 
