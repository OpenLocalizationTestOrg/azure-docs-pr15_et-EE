<properties 
    pageTitle="Sujuva voogesituse Windowsi poe rakenduse õpetuse | Microsoft Azure'i" 
    description="Saate teada, kuidas luua C# Windowsi poe rakendus taasesitus sujuv voo sisu juhtelemendiga XML-i MediaElement Azure Media Servicesi abil." 
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



#<a name="how-to-build-a-smooth-streaming-windows-store-application"></a>Kuidas luua sujuva voogesituse Windowsi poe rakendus

Sujuva voogesituse kliendi SDK jaoks Windows 8 võimaldab arendajatel luua Windowsi poe rakendused, mida saab esitada nõudmisel ja reaalajas sujuva voogesituse sisu. Lisaks lihtsa taasesitamise sujuva voogesituse sisu SDK pakub rikkaliku funktsioone, nagu Microsoft PlayReady kaitse, kvaliteetsema heli tagamiseks taseme piirang, Live DVR, voona üleminek, listening olekuvärskendusi (nt kvaliteedi saiditasemel muudatusi) ja tõrge jne. Toetatud funktsioonide kohta leiate lisateavet teemast [vabastage märkmed](http://www.iis.net/learn/media/smooth-streaming/smooth-streaming-client-sdk-for-windows-8-release-notes). Lisateabe saamiseks vt [Playeri Framework Windows 8](http://playerframework.codeplex.com/). 

Õppeteema sisaldab nelja lugu.

1. Lihtsa sujuva voogesituse Store rakenduse loomine
2. Lisage liugurriba määrata Media edenemine
3. Valige sujuva voogesituse voogu
4. Valige sujuva voogesituse rajad

##<a name="prerequisites"></a>Eeltingimused

- Windows 8 32-bitine või 64-bitine. Saate avada [Windows 8 Enterprise hindamine](http://msdn.microsoft.com/evalcenter/jj554510.aspx) MSDN-i kaudu.
- Visual Studio 2012 või Visual Studio Express 2012 (või uuem versioon). Saate prooviversiooni [siia](http://www.microsoft.com/visualstudio/11/downloads).
- [Microsoft sujuva voogesituse kliendi SDK Windows 8](http://visualstudiogallery.msdn.microsoft.com/04423d13-3b3e-4741-a01c-1ae29e84fea6?SRC=Homehttp://visualstudiogallery.msdn.microsoft.com/04423d13-3b3e-4741-a01c-1ae29e84fea6?SRC=Home).


Iga õppetüki lõplikus lahendus saab alla laadida MSDN-i arendaja koodinäiteid (kood galerii): 

- [Õppetüki 1](http://code.msdn.microsoft.com/Smooth-Streaming-Client-0bb1471f) - lihtsa Windows 8 sujuva voogesituse meediumipleierit, 
- [Tund 2](http://code.msdn.microsoft.com/A-simple-Windows-8-Smooth-ee98f63a) - lihtsa Windows 8 sujuva voogesituse Media Playeri liugurriba abil määrata, 
- [Õppetüki 3](http://code.msdn.microsoft.com/A-Windows-8-Smooth-883c3b44) – Windows 8 sujuva voogesituse Media Playeri voo valimist  
- [Õppetüki 4](http://code.msdn.microsoft.com/A-Windows-8-Smooth-aa9e4907) - Windows 8 tõrgeteta Streaming Media Playeri abil valimiseks.

##<a name="lesson-1-create-a-basic-smooth-streaming-store-application"></a>1 tund: Lihtsa sujuva voogesituse Store rakenduse loomine

Selle õppetüki loote Windowsi poe rakenduse koos MediaElement juhtelemendi sisu voo tõrgeteta esitada.  Töötava rakenduse näeb välja selline:

![Sujuv Streaming Windowsi poe rakenduse näide][PlayerApplication]
 
Windowsi poe rakenduse arendamise kohta leiate lisateavet teemast [töötada hea rakendused Windows 8](http://msdn.microsoft.com/windows/apps/br229512.aspx). Selle õppetüki sisaldab järgmist:

1.  Windowsi poe projekti loomine
2.  Kujundus kasutajaliides (XAML)
3.  Faili koodide muutmine
4.  Koostada ja testida rakendus

**Windowsi poe projekti loomise kohta**

1.  Käivitage Visual Studio 2012 või uuem versioon.
2.  Klõpsake menüü **fail** nuppu **Uus**ja seejärel klõpsake nuppu **projekti**.
3.  Valige dialoogiboksi Uus projekt tippige või valige järgmised väärtused:

Nimi|Väärtus
---|---
Malli rühma|Installitud/mallide/Visual C# / Windows Store
Mall|Tühi rakendus (XAML)
Nimi|SSPlayer
Asukoht|C:\SSTutorials
Lahenduse nimi|SSPlayer
Kataloogi lahenduse loomine|(valitud)

4.  Klõpsake nuppu **OK**.

**Sujuva voogesituse kliendi SDK viite lisamine**

1.  Paremklõpsake **SSPlayer**Solution Exploreris ja seejärel klõpsake nuppu **Lisa viide**.
2.  Tippige või valige järgmised väärtused:

Nimi|Väärtus
---|---
Viiterühm|Windowsi/laiendid
Viide|Valige Microsoft sujuva voogesituse kliendi SDK Windows 8 ja Microsoft Visual C++ Runtime pakett
    
3.  Klõpsake nuppu **OK**. 

Pärast lisamist viiteid, valige suunatud platvormi (x64 või x86), mis tahes CPU platvormi konfiguratsiooni lisamise viiteid ei toimi.  Solution Exploreris, näete, kollane hoiatus märgi nende lisatud viited.

**Kujundamiseks Playeri kasutajaliides**

1.  Solution Exploreris, topeltklõpsake nuppu **MainPage.xaml** avamiseks kujundusvaates.
2.  Otsige üles soovitud ** &lt;ruudustiku&gt; ** ja ** &lt;/Grid&gt; ** XAML faili sildid ja kleepige järgmine kood kaks siltide vahel:

        <Grid.RowDefinitions>
            <RowDefinition Height="20"/>    <!-- spacer -->
            <RowDefinition Height="50"/>    <!-- media controls -->
            <RowDefinition Height="100*"/>  <!-- media element -->
            <RowDefinition Height="80*"/>   <!-- media stream and track selection -->
            <RowDefinition Height="50"/>    <!-- status bar -->
        </Grid.RowDefinitions>
        
        <StackPanel Name="spMediaControl" Grid.Row="1" Orientation="Horizontal">
            <TextBlock x:Name="tbSource" Text="Source :  " FontSize="16" FontWeight="Bold" VerticalAlignment="Center" />
            <TextBox x:Name="txtMediaSource" Text="http://ecn.channel9.msdn.com/o9/content/smf/smoothcontent/elephantsdream/Elephants_Dream_1024-h264-st-aac.ism/manifest" FontSize="10" Width="700" Margin="0,4,0,10" />
            <Button x:Name="btnSetSource" Content="Set Source" Width="111" Height="43" Click="btnSetSource_Click"/>
            <Button x:Name="btnPlay" Content="Play" Width="111" Height="43" Click="btnPlay_Click"/>
            <Button x:Name="btnPause" Content="Pause"  Width="111" Height="43" Click="btnPause_Click"/>
            <Button x:Name="btnStop" Content="Stop"  Width="111" Height="43" Click="btnStop_Click"/>
            <CheckBox x:Name="chkAutoPlay" Content="Auto Play" Height="55" Width="Auto" IsChecked="{Binding AutoPlay, ElementName=mediaElement, Mode=TwoWay}"/>
            <CheckBox x:Name="chkMute" Content="Mute" Height="55" Width="67" IsChecked="{Binding IsMuted, ElementName=mediaElement, Mode=TwoWay}"/>
        </StackPanel>

        <StackPanel Name="spMediaElement" Grid.Row="2" Height="435" Width="1072"
                    HorizontalAlignment="Center" VerticalAlignment="Center">
            <MediaElement x:Name="mediaElement" Height="356" Width="924" MinHeight="225"
                          HorizontalAlignment="Center" VerticalAlignment="Center" 
                          AudioCategory="BackgroundCapableMedia" />
            <StackPanel Orientation="Horizontal">
                <Slider x:Name="sliderProgress" Width="924" Height="44"
                        HorizontalAlignment="Center" VerticalAlignment="Center"
                        PointerPressed="sliderProgress_PointerPressed"/>
                <Slider x:Name="sliderVolume" 
                        HorizontalAlignment="Right" VerticalAlignment="Center" Orientation="Vertical" 
                        Height="79" Width="148" Minimum="0" Maximum="1" StepFrequency="0.1" 
                        Value="{Binding Volume, ElementName=mediaElement, Mode=TwoWay}" 
                        ToolTipService.ToolTip="{Binding Value, RelativeSource={RelativeSource Mode=Self}}"/>
            </StackPanel>
        </StackPanel>

        <StackPanel Name="spStatus" Grid.Row="4" Orientation="Horizontal">
            <TextBlock x:Name="tbStatus" Text="Status :  " 
               FontSize="16" FontWeight="Bold" VerticalAlignment="Center" HorizontalAlignment="Center" />
            <TextBox x:Name="txtStatus" FontSize="10" Width="700" VerticalAlignment="Center"/>
        </StackPanel>

    Meediumi taasesituse kasutatakse MediaElement juhtelementi. Liuguri juhtelement nimega sliderProgress kasutatakse järgmise õppetüki meediumi edenemist kontrollida.

3.  Vajutage **Klahvikombinatsiooni CTRL + S** , kuhu soovite faili salvestada.

MediaElement juhtelementi ei toeta sujuva voogesituse sisu välja-ja-box. Sujuva voogesituse toe lubamiseks peate registreerima sujuva voogesituse bait-voo sündmuseohjuri failinime laiend ja MIME tüüp.  Registreerida, kasutate Windows.Media nimeruumi MediaExtensionManager.RegisterByteStremHandler meetodit.

XAML failis on mõni sündmuseohjur seotud juhtelemendid.  Peate määratlema nende sündmuseohjur.

**Faili koodide muutmiseks**

1.  Lahenduste Explorer, paremklõpsake **MainPage.xaml**ja seejärel klõpsake nuppu **Kuva kood**.
2.  Faili ülaosas lisada järgmised kasutamine lause:

        using Windows.Media;

3.  **Avaleht** klassi alguses järgmised andmed liikme lisamiseks tehke järgmist.

        private MediaExtensionManager extensions = new MediaExtensionManager();

4.  Lõpus **Avaleht** ehitaja, lisage järgmised kaks rida.

        extensions.RegisterByteStreamHandler("Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", ".ism", "text/xml");
        extensions.RegisterByteStreamHandler("Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", ".ism", "application/vnd.ms-sstr+xml");
        
5.  Lõpus **Avaleht** ainekursust, viimase järgmine kood:

        #region UI Button Click Events
        private void btnPlay_Click(object sender, RoutedEventArgs e)
        {
            mediaElement.Play();
            txtStatus.Text = "MediaElement is playing ...";
        }
        
        private void btnPause_Click(object sender, RoutedEventArgs e)
        {
            mediaElement.Pause();
            txtStatus.Text = "MediaElement is paused";
        }
        
        private void btnSetSource_Click(object sender, RoutedEventArgs e)
        {
            sliderProgress.Value = 0;
            mediaElement.Source = new Uri(txtMediaSource.Text);
        
            if (chkAutoPlay.IsChecked == true)
            {
                txtStatus.Text = "MediaElement is playing ...";
            }
            else
            {
                txtStatus.Text = "Click the Play button to play the media source.";
            }
        }
        
        private void btnStop_Click(object sender, RoutedEventArgs e)
        {
            mediaElement.Stop();
            txtStatus.Text = "MediaElement is stopped";
        }
        
        private void sliderProgress_PointerPressed(object sender, PointerRoutedEventArgs e)
        {
            txtStatus.Text = "Seek to position " + sliderProgress.Value;
            mediaElement.Position = new TimeSpan(0, 0, (int)(sliderProgress.Value));
        }
        #endregion

    Siin on määratletud sliderProgress_PointerPressed sündmuseohjuri.  On veel töid saada selle tööle, kus käsitletakse järgmise õppetüki õppetüki.
6.  Vajutage **Klahvikombinatsiooni CTRL + S** , kuhu soovite faili salvestada.

Lõpetanud faili koodide peab välja nägema selline:

![CodeView Visual Studio, sujuva voogesituse Windowsi poe rakenduses][CodeViewPic]

**Koostada ja testida rakendus**

1.  Klõpsake menüü **koostada** **Configuration Manager**.
2.  Saate muuta **aktiivse lahenduse platvormi** vastavaks oma arengu platvorm.
3.  Vajutage klahvi **F6** projekti koostada. 
4.  Vajutage klahvi **F5** käivitage rakendus.
5.  Rakenduse ülaservas, saate kasutada vaikimisi sujuva voogesituse URL või sisestada mõne muu. 
6.  Klõpsake **andmeallika määramine**. Kuna **Automaatne esitamine** on vaikimisi, esitada meediumi automaatselt.  Saate määrata meediumi **Esita**, **Peata** "ja" **Stop** nuppude abil.  Saate määrata meediumifailide maht vertikaalne liugurit.  Siiski horisontaalset liugurit meediumi edenemise kontrollimiseks pole täielikult veel juurutatud. 

Olete lesson1 lõpule jõudnud.  Selle õppetüki kasutage MediaElement juhtelemendi taasesitus sujuva voogesituse sisu.  Järgmise õppetüki lisamist liugur edenemise sujuva voogesituse sisu kontrollida.


##<a name="lesson-2-add-a-slider-bar-to-control-the-media-progress"></a>Tund 2: Lisada liugurriba määrata Media edenemine
1 tund, loodud Windowsi poe rakendus taasesitus sujuva voogesituse meediumisisu MediaElement XAML juhtelemendiga.  Tegemist on mõned elementaarne meediumi funktsioone nagu start, Peata ja paus.  Selle õppetüki on liugurit ribal juhtelemendi lisamine rakenduse.

Selles õpetuses kasutame on timer värskendamiseks positsiooni põhjal MediaElement juhtelemendi praegust paigutust.  Liugur algus ja lõpp aeg reaalajas sisu korral tuleb värskendada.  See võib paremini käsitleda kohandatava andmeallika värskendamine sündmus.

Meediumi allikad on objekte, mis meediumi andmete loomiseks.  Andmeallika lahendaja võtab URL-i või bait voo ja loob vastav meediumi allika sellele sisule.  Andmeallika lahendaja on standardne viis rakenduste loomine meedia allikatest. 

Selle õppetüki sisaldab järgmist:

1.  Sujuva voogesituse sündmuseohjuri registreerimine 
2.  Lisage kohandatava andmeallikate haldur taseme sündmuseohjur
3.  Sündmuseohjur taseme kohandatava andmeallika lisamine
4.  Lisage MediaElement sündmuseohjur
5.  Liugur seotud koodi lisamine
6.  Koostada ja testida rakendus

**Sujuva voogesituse bait-voo sündmuseohjuri registreerida ja edastama selle propertyset**

1.  Lahenduste Explorer, paremklõpsake **MainPage.xaml**ja seejärel klõpsake nuppu **Kuva kood**.
2.  Faili alguses, lisage järgmine lause abil:

        using Microsoft.Media.AdaptiveStreaming;

3.  Avaleht klassi alguses järgmised andmed liikmete lisamiseks tehke järgmist.

        private Windows.Foundation.Collections.PropertySet propertySet = new Windows.Foundation.Collections.PropertySet();             
        private IAdaptiveSourceManager adaptiveSourceManager;
    
4.  Sees **Avaleht** ehitaja, lisage järgmine kood pärast selle **see. Lähtestada Components();** rea- ja registreerimise koodi kirjutada eelmises õppetükis jooned:
    
        // Gets the default instance of AdaptiveSourceManager which manages Smooth 
        //Streaming media sources.
        adaptiveSourceManager = AdaptiveSourceManager.GetDefault();
        // Sets property key value to AdaptiveSourceManager default instance.
        // {A5CE1DE8-1D00-427B-ACEF-FB9A3C93DE2D}" must be hardcoded.
        propertySet["{A5CE1DE8-1D00-427B-ACEF-FB9A3C93DE2D}"] = adaptiveSourceManager;
    
5.  **Avaleht** ehitaja, sees muuta kahe RegisterByteStreamHandler meetodi lisamiseks soovitud edasi parameetrid:

        // Registers Smooth Streaming byte-stream handler for ".ism" extension and, 
        // "text/xml" and "application/vnd.ms-ss" mime-types and pass the propertyset. 
        // http://*.ism/manifest URI resources will be resolved by Byte-stream handler.
        extensions.RegisterByteStreamHandler(
            "Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", 
            ".ism", 
            "text/xml", 
            propertySet );
        extensions.RegisterByteStreamHandler(
            "Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", 
            ".ism", 
            "application/vnd.ms-sstr+xml", 
        propertySet);

6.  Vajutage **Klahvikombinatsiooni CTRL + S** , kuhu soovite faili salvestada.

**Kohandatavad andmeallikate haldur taseme sündmuseohjuri lisamine**

1.  Lahenduste Explorer, paremklõpsake **MainPage.xaml**ja seejärel klõpsake nuppu **Kuva kood**.
2.  Sees **Avaleht** ainekursust, järgmised andmed liikme lisamiseks tehke järgmist.

        private AdaptiveSource adaptiveSource = null;

3.  **Avaleht** klassi lõpus järgmist sündmuseohjuri lisamiseks tehke järgmist.
    
        #region Adaptive Source Manager Level Events
        private void mediaElement_AdaptiveSourceOpened(AdaptiveSource sender, AdaptiveSourceOpenedEventArgs args)
        {
            adaptiveSource = args.AdaptiveSource;
        }
        #endregion Adaptive Source Manager Level Events

4.  **Avaleht** ehitaja lõpus järgmine rida tellida sündmuse avamine kohandatava allika lisamiseks tehke järgmist.
    
    adaptiveSourceManager.AdaptiveSourceOpenedEvent += uue AdaptiveSourceOpenedEventHandler(mediaElement_AdaptiveSourceOpened);

5.  Vajutage **Klahvikombinatsiooni CTRL + S** , kuhu soovite faili salvestada.

**Taseme sündmuseohjur kohandatava andmeallika lisamine**

1.  Lahenduste Explorer, paremklõpsake **MainPage.xaml**ja seejärel klõpsake nuppu **Kuva kood**.
2.  Sees **Avaleht** ainekursust, järgmised andmed liikme lisamiseks tehke järgmist.
    
        private AdaptiveSourceStatusUpdatedEventArgs adaptiveSourceStatusUpdate; 
        private Manifest manifestObject;
    
3.  **Avaleht** klassi lõpus järgmist sündmuseohjur lisamiseks tehke järgmist.

        #region Adaptive Source Level Events
        private void mediaElement_ManifestReady(AdaptiveSource sender, ManifestReadyEventArgs args)
        {
            adaptiveSource = args.AdaptiveSource;
            manifestObject = args.AdaptiveSource.Manifest;
        }
        
        private void mediaElement_AdaptiveSourceStatusUpdated(AdaptiveSource sender, AdaptiveSourceStatusUpdatedEventArgs args)
        {
            adaptiveSourceStatusUpdate = args;
        }
        
        private void mediaElement_AdaptiveSourceFailed(AdaptiveSource sender, AdaptiveSourceFailedEventArgs args)
        {
            txtStatus.Text = "Error: " + args.HttpResponse;
        }
        #endregion Adaptive Source Level Events

4.  **MediaElement AdaptiveSourceOpened** meetod lõpus lisada tellida sündmuste järgmine kood:
    
        adaptiveSource.ManifestReadyEvent +=
                    mediaElement_ManifestReady;
        adaptiveSource.AdaptiveSourceStatusUpdatedEvent += 
            mediaElement_AdaptiveSourceStatusUpdated;
        adaptiveSource.AdaptiveSourceFailedEvent += 
            mediaElement_AdaptiveSourceFailed;
    
5.  Vajutage **Klahvikombinatsiooni CTRL + S** , kuhu soovite faili salvestada.

Sama sündmused on saadaval kohandatava allika sõim tasemel, mida saab kasutada käsitlemise levinud kõik meediumielemendid rakenduse funktsionaalsus. Iga AdaptiveSource sisaldab oma sündmused ja kõik AdaptiveSource sündmused on virnastatud jaotises AdaptiveSourceManager.

**Lisada meediumi elemendi sündmuseohjur**

1.  Lahenduste Explorer, paremklõpsake **MainPage.xaml**ja seejärel klõpsake nuppu **Kuva kood**.
2.  **Avaleht** klassi lõpus järgmist sündmuseohjur lisamiseks tehke järgmist.
    
        #region Media Element Event Handlers 
        private void MediaOpened(object sender, RoutedEventArgs e)
        {
            txtStatus.Text = "MediaElement opened";
        }
        
        private void MediaFailed(object sender, ExceptionRoutedEventArgs e)
        {
            txtStatus.Text= "MediaElement failed: " + e.ErrorMessage;
        }
        
        private void MediaEnded(object sender, RoutedEventArgs e)
        {
            txtStatus.Text ="MediaElement ended.";
        }
        #endregion Media Element Event Handlers

3.  **Avaleht** ehitaja lõpus lisamine allindeks sündmusi järgmine kood:
    
        mediaElement.MediaOpened += MediaOpened;
        mediaElement.MediaEnded += MediaEnded;
        mediaElement.MediaFailed += MediaFailed;

4.  Vajutage **Klahvikombinatsiooni CTRL + S** , kuhu soovite faili salvestada.

**Lisada liugurriba seotud kood**

1.  Lahenduste Explorer, paremklõpsake **MainPage.xaml**ja seejärel klõpsake nuppu **Kuva kood**.
2.  Faili alguses, lisage järgmine lause abil:
    
        using Windows.UI.Core;

3.  **Avaleht** ainekursust, kes kuuluvad järgmised andmed liikmete lisamiseks tehke järgmist.
    
        public static CoreDispatcher _dispatcher;
        private DispatcherTimer sliderPositionUpdateDispatcher;
    
4.  **Avaleht** ehitaja lõpus lisada järgmine kood:

        _dispatcher = Window.Current.Dispatcher;
        PointerEventHandler pointerpressedhandler = new PointerEventHandler(sliderProgress_PointerPressed);
        sliderProgress.AddHandler(Control.PointerPressedEvent, pointerpressedhandler, true);    
    
5.  Lõpus **Avaleht** ainekursust, lisage järgmine kood:
    
        #region sliderMediaPlayer
        private double SliderFrequency(TimeSpan timevalue)
        {
            long absvalue = 0;
            double stepfrequency = -1;
        
            if (manifestObject != null)
            {
                absvalue = manifestObject.Duration - (long)manifestObject.StartTime;
            }
            else
            {
                absvalue = mediaElement.NaturalDuration.TimeSpan.Ticks;
            }
        
            TimeSpan totalDVRDuration = new TimeSpan(absvalue);
        
            if (totalDVRDuration.TotalMinutes >= 10 && totalDVRDuration.TotalMinutes < 30)
            {
               stepfrequency = 10;
            }
            else if (totalDVRDuration.TotalMinutes >= 30 
                     && totalDVRDuration.TotalMinutes < 60)
            {
                stepfrequency = 30;
            }
            else if (totalDVRDuration.TotalHours >= 1)
            {
                stepfrequency = 60;
            }
        
            return stepfrequency;
        }
        
        void updateSliderPositionoNTicks(object sender, object e)
        {
            sliderProgress.Value = mediaElement.Position.TotalSeconds;
        }
        
        public void setupTimer()
        {
            sliderPositionUpdateDispatcher = new DispatcherTimer();
            sliderPositionUpdateDispatcher.Interval = new TimeSpan(0, 0, 0, 0, 300);
            startTimer();
        }

        public void startTimer()
        {
            sliderPositionUpdateDispatcher.Tick += updateSliderPositionoNTicks;
            sliderPositionUpdateDispatcher.Start();
        }
        
        // Slider start and end time must be updated in case of live content
        public async void setSliderStartTime(long startTime)
        {
            await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
                TimeSpan timespan = new TimeSpan(adaptiveSourceStatusUpdate.StartTime);
                double absvalue = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero);
                sliderProgress.Minimum = absvalue;
            });
        }
        
        // Slider start and end time must be updated in case of live content
        public async void setSliderEndTime(long startTime)
        {
            await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
                TimeSpan timespan = new TimeSpan(adaptiveSourceStatusUpdate.EndTime);
                double absvalue = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero);
                sliderProgress.Maximum = absvalue;
            });
        }
        #endregion sliderMediaPlayer

    **Märkus:** CoreDispatcher kasutatakse mitte UI lõime muutma UI lõime. Saatja, teema on olukord korral võite kasutada saatja järgi Kasutajaliidese-element ta kavas värskendada arendaja.  Näiteks:
    
        await sliderProgress.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => { TimeSpan 
          timespan = new TimeSpan(adaptiveSourceStatusUpdate.EndTime); 
        double absvalue  = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero); 
          sliderProgress.Maximum = absvalue; }); 
        

6.  Lõpus **mediaElement_AdaptiveSourceStatusUpdated** meetod, lisage järgmine kood:
    
        setSliderStartTime(args.StartTime);
        setSliderEndTime(args.EndTime);

7.  Lõpus **MediaOpened** meetod, lisage järgmine kood:
    
    sliderProgress.StepFrequency = SliderFrequency(mediaElement.NaturalDuration.TimeSpan); sliderProgress.Width = mediaElement.Width; setupTimer();

8.  Vajutage **Klahvikombinatsiooni CTRL + S** , kuhu soovite faili salvestada.

**Koostada ja testida rakendus**

1. Vajutage klahvi **F6** projekti koostada. 
2.  Vajutage klahvi **F5** käivitage rakendus.
3.  Rakenduse ülaservas, saate kasutada vaikimisi sujuva voogesituse URL või sisestada mõne muu. 
4.  Klõpsake **andmeallika määramine**. 
5.  Testige liugurit.

Tund 2 on lõpule viidud.  Selle õppetüki lisasite liugur rakendusse. 

##<a name="lesson-3-select-smooth-streaming-streams"></a>Tund 3: Valige sujuva voogesituse voogu
Sujuv Streaming suudab voogesituseks mitme keele heli lood, mis on valitav vaatajad poolt.  Selle õppetüki lubate vaatajatel valige voogu. Selle õppetüki sisaldab järgmist:

1. XAML-faili muutmine
2. Kood behand faili muutmine
3. Koostada ja testida rakendus


**XAML faili muutmiseks**

1. Paremklõpsake **MainPage.xaml**Solution Exploreris ja seejärel klõpsake nuppu **Kuvakujundaja**.
2. Otsige üles &lt;Grid.RowDefinitions&gt;, ja muutma selle RowDefinitions, et ta näeb:

        <Grid.RowDefinitions>            
            <RowDefinition Height="20"/>
            <RowDefinition Height="50"/>
            <RowDefinition Height="100"/>
            <RowDefinition Height="80"/>
            <RowDefinition Height="50"/>
        </Grid.RowDefinitions>

3. Sees on &lt;ruudustiku&gt;&lt;/Grid&gt; sildid, lisage määratleda loendiboksi juhtelemendi, nii et kasutajad saavad loendi kuvamiseks saadaval voole ja valige voogu järgmine kood:

        <Grid Name="gridStreamAndBitrateSelection" Grid.Row="3">
            <Grid.RowDefinitions>
                <RowDefinition Height="300"/>
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="250*"/>
                <ColumnDefinition Width="250*"/>
            </Grid.ColumnDefinitions>
            <StackPanel Name="spStreamSelection" Grid.Row="1" Grid.Column="0">
                <StackPanel Orientation="Horizontal">
                    <TextBlock Name="tbAvailableStreams" Text="Available Streams:" FontSize="16" VerticalAlignment="Center"></TextBlock>
                    <Button Name="btnChangeStreams" Content="Submit" Click="btnChangeStream_Click"/>
                </StackPanel>
                <ListBox x:Name="lbAvailableStreams" Height="200" Width="200" Background="Transparent" HorizontalAlignment="Left" 
                    ScrollViewer.VerticalScrollMode="Enabled" ScrollViewer.VerticalScrollBarVisibility="Visible">
                    <ListBox.ItemTemplate>
                        <DataTemplate>
                            <CheckBox Content="{Binding Name}" IsChecked="{Binding isChecked, Mode=TwoWay}" />
                        </DataTemplate>
                    </ListBox.ItemTemplate>
                </ListBox>
            </StackPanel>
        </Grid>

4. Vajutage muudatuste salvestamiseks **Klahvikombinatsiooni CTRL + S** .


**Faili koodide muutmiseks**

1. Lahenduste Explorer, paremklõpsake **MainPage.xaml**ja seejärel klõpsake nuppu **Kuva kood**.
2. Sees SSPlayer nimeruumi, lisada uue klassi: #region klassi voo
    
        public class Stream
        {
            private IManifestStream stream;
            public bool isCheckedValue;
            public string name;
    
            public string Name
            {
                get { return name; }
                set { name = value; }
            }
    
            public IManifestStream ManifestStream
            {
                get { return stream; }
                set { stream = value; }
            }
    
            public bool isChecked
            {
                get { return isCheckedValue; }
                set
                {
                    // mMke the video stream always checked.
                    if (stream.Type == MediaStreamType.Video)
                    {
                        isCheckedValue = true;
                    }
                    else
                    {
                        isCheckedValue = value;
                    }
                }
            }
    
            public Stream(IManifestStream streamIn)
            {
                stream = streamIn;
                name = stream.Name;
            }
        }
        #endregion class Stream

3. Avaleht klassi alguses järgmisi muutuv lisamiseks tehke järgmist.

        private List<Stream> availableStreams;
        private List<Stream> availableAudioStreams;
        private List<Stream> availableTextStreams;
        private List<Stream> availableVideoStreams;

4. Sees Avaleht ainekursust, lisage järgmised piirkond.

        #region stream selection
        ///<summary>
        ///Functionality to select streams from IManifestStream available streams
        /// </summary>
        
        // This function is called from the mediaElement_ManifestReady event handler 
        // to retrieve the streams and populate them to the local data members.
        public void getStreams(Manifest manifestObject)
        {
            availableStreams = new List<Stream>();
            availableVideoStreams = new List<Stream>();
            availableAudioStreams = new List<Stream>();
            availableTextStreams = new List<Stream>();
        
            try
            {
                for (int i = 0; i<manifestObject.AvailableStreams.Count; i++)
                {
                    Stream newStream = new Stream(manifestObject.AvailableStreams[i]);
                    newStream.isChecked = false;
        
                    //populate the stream lists based on the types
                    availableStreams.Add(newStream);
        
                    switch (newStream.ManifestStream.Type)
                    {
                        case MediaStreamType.Video:
                            availableVideoStreams.Add(newStream);
                            break;
                        case MediaStreamType.Audio:
                            availableAudioStreams.Add(newStream);
                            break;
                        case MediaStreamType.Text:
                            availableTextStreams.Add(newStream);
                            break;
                    }
        
                    // Select the default selected streams from the manifest.
                    for (int j = 0; j<manifestObject.SelectedStreams.Count; j++)
                    {
                        string selectedStreamName = manifestObject.SelectedStreams[j].Name;
                        if (selectedStreamName.Equals(newStream.Name))
                        {
                            newStream.isChecked = true;
                            break;
                        }
                    }
                }
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }
        }
        
        // This function set the list box ItemSource
        private async void refreshAvailableStreamsListBoxItemSource()
        {
            try
            {
                //update the stream check box list on the UI
                await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, ()
                    => { lbAvailableStreams.ItemsSource = availableStreams; });
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }
        }
        
        // This function creates a selected streams list
        private void createSelectedStreamsList(List<IManifestStream> selectedStreams)
        {
            bool isOneVideoSelected = false;
            bool isOneAudioSelected = false;
        
            // Only one video stream can be selected
            for (int j = 0; j<availableVideoStreams.Count; j++)
            {
                if (availableVideoStreams[j].isChecked && (!isOneVideoSelected))
                {
                    selectedStreams.Add(availableVideoStreams[j].ManifestStream);
                    isOneVideoSelected = true;
                }
            }
        
            // Select the frist video stream from the list if no video stream is selected
            if (!isOneVideoSelected)
            {
                availableVideoStreams[0].isChecked = true;
                selectedStreams.Add(availableVideoStreams[0].ManifestStream);
            }
        
            // Only one audio stream can be selected
            for (int j = 0; j<availableAudioStreams.Count; j++)
            {
                if (availableAudioStreams[j].isChecked && (!isOneAudioSelected))
                {
                    selectedStreams.Add(availableAudioStreams[j].ManifestStream);
                    isOneAudioSelected = true;
                    txtStatus.Text = "The audio stream is changed to " + availableAudioStreams[j].ManifestStream.Name;
                }
            }
        
            // Select the frist audio stream from the list if no audio steam is selected.
            if (!isOneAudioSelected)
            {
                availableAudioStreams[0].isChecked = true;
                selectedStreams.Add(availableAudioStreams[0].ManifestStream);
            }
        
            // Multiple text streams are supported.
            for (int j = 0; j < availableTextStreams.Count; j++)
            {
                if (availableTextStreams[j].isChecked)
                {
                    selectedStreams.Add(availableTextStreams[j].ManifestStream);
                }
            }
        }
        
        // Change streams on a smooth streaming presentation with multiple video streams.
        private async void changeStreams(List<IManifestStream> selectStreams)
        {
            try
            {
                IReadOnlyList<IStreamChangedResult> returnArgs =
                    await manifestObject.SelectStreamsAsync(selectStreams);
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }
        }
        #endregion stream selection

5. Leidke mediaElement_ManifestReady meetodit, lisab funktsioon lõpus järgmine kood:
    
        getStreams(manifestObject);
        refreshAvailableStreamsListBoxItemSource();

    Nii, et kui MediaElement manifesti on valmis, koodi saab saadaval voogu loendi ja kuvab väljal UI loendis.

6. Sees Avaleht ainekursust, leidke Kasutajaliidese nuppude klõpsake sündmuste piirkond ja seejärel lisage järgmine funktsioon määratlus:

        private void btnChangeStream_Click(object sender, RoutedEventArgs e)
        {
            List<IManifestStream> selectedStreams = new List<IManifestStream>();

            // Create a list of the selected streams
            createSelectedStreamsList(selectedStreams);

            // Change streams on the presentation
            changeStreams(selectedStreams);
        }

**Koostada ja testida rakendus**

1. Vajutage klahvi **F6** projekti koostada. 
2.  Vajutage klahvi **F5** käivitage rakendus.
3.  Rakenduse ülaservas, saate kasutada vaikimisi sujuva voogesituse URL või sisestada mõne muu. 
4.  Klõpsake **andmeallika määramine**. 
5.  Vaikimisi on audio_eng. Proovige audio_eng ja audio_es vaheldumisi aktiveerimine. Iga kord, valite uue voo, tuleb klõpsata nuppu Edasta.

Õppetüki 3 on lõpule viidud.  Selle õppetüki lisada saate valida voogu funktsioone.

##<a name="lesson-4-select-smooth-streaming-tracks"></a>Õppetüki 4: Valige sujuva voogesituse rajad
Sujuva voogesituse esitlus võib sisaldada mitut videofaile kodeeritud erinevate tasemete (bitine määr) ja lahendused. Selle õppetüki lubate kasutajatel valida jälitab. Selle õppetüki sisaldab järgmist:

1. XAML-faili muutmine
2. Kood behand faili muutmine
3. Koostada ja testida rakendus

**XAML faili muutmiseks**

1. Paremklõpsake **MainPage.xaml**Solution Exploreris ja seejärel klõpsake nuppu **Kuvakujundaja**.
2. Otsige soovitud &lt;ruudustiku&gt; nimi **gridStreamAndBitrateSelection**silt, lisa lõpus olevat silti järgmine kood:

        <StackPanel Name="spBitRateSelection" Grid.Row="1" Grid.Column="1">
         <StackPanel Orientation="Horizontal">
             <TextBlock Name="tbBitRate" Text="Available Bitrates:" FontSize="16" VerticalAlignment="Center"/>
             <Button Name="btnChangeTracks" Content="Submit" Click="btnChangeTrack_Click" />
         </StackPanel>
         <ListBox x:Name="lbAvailableVideoTracks" Height="200" Width="200" Background="Transparent" HorizontalAlignment="Left" 
                  ScrollViewer.VerticalScrollMode="Enabled" ScrollViewer.VerticalScrollBarVisibility="Visible">
             <ListBox.ItemTemplate>
                 <DataTemplate>
                     <CheckBox Content="{Binding Bitrate}" IsChecked="{Binding isChecked, Mode=TwoWay}"/>
                 </DataTemplate>
             </ListBox.ItemTemplate>
         </ListBox>
        </StackPanel>

3. Ta muudatuste salvestamiseks vajutage **Klahvikombinatsiooni CTRL + S**


**Faili koodide muutmiseks**

1. Lahenduste Explorer, paremklõpsake **MainPage.xaml**ja seejärel klõpsake nuppu **Kuva kood**.
2. Sees SSPlayer nimeruumi, uutel lisamiseks tehke järgmist.
    
        #region class Track
        public class Track
        {
            private IManifestTrack trackInfo;
            public string _bitrate;
            public bool isCheckedValue;
    
            public IManifestTrack TrackInfo
            {
                get { return trackInfo; }
                set { trackInfo = value; }
            }
    
            public string Bitrate
            {
                get { return _bitrate; }
                set { _bitrate = value; }
            }
    
            public bool isChecked
            {
                get { return isCheckedValue; }
                set
                {
                    isCheckedValue = value;
                }
            }
    
            public Track(IManifestTrack trackInfoIn)
            {
                trackInfo = trackInfoIn;
                _bitrate = trackInfoIn.Bitrate.ToString();
            }
            //public Track() { }
        }
        #endregion class Track

3. Avaleht klassi alguses järgmisi muutuv lisamiseks tehke järgmist.
    
        private List<Track> availableTracks;

4. Sees Avaleht ainekursust, lisage järgmised piirkond.
    
        #region track selection
        /// <summary>
        /// Functionality to select video streams
        /// </summary>

        /// This Function gets the tracks for the selected video stream
        public void getTracks(Manifest manifestObject)
        {
            availableTracks = new List<Track>();

            IManifestStream videoStream = getVideoStream();
            IReadOnlyList<IManifestTrack> availableTracksLocal = videoStream.AvailableTracks;
            IReadOnlyList<IManifestTrack> selectedTracksLocal = videoStream.SelectedTracks;

            try
            {
                for (int i = 0; i < availableTracksLocal.Count; i++)
                {
                    Track thisTrack = new Track(availableTracksLocal[i]);
                    thisTrack.isChecked = true;

                    for (int j = 0; j < selectedTracksLocal.Count; j++)
                    {
                        string selectedTrackName = selectedTracksLocal[j].Bitrate.ToString();
                        if (selectedTrackName.Equals(thisTrack.Bitrate))
                        {
                            thisTrack.isChecked = true;
                            break;
                        }
                    }
                    availableTracks.Add(thisTrack);
                }
            }
            catch (Exception e)
            {
                txtStatus.Text = e.Message;
            }
        }

        // This function gets the video stream that is playing
        private IManifestStream getVideoStream()
        {
            IManifestStream videoStream = null;
            for (int i = 0; i < manifestObject.SelectedStreams.Count; i++)
            {
                if (manifestObject.SelectedStreams[i].Type == MediaStreamType.Video)
                {
                    videoStream = manifestObject.SelectedStreams[i];
                    break;
                }
            }
            return videoStream;
        }

        // This function set the UI list box control ItemSource
        private async void refreshAvailableTracksListBoxItemSource()
        {
            try
            {
                // Update the track check box list on the UI 
                await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, ()
                    => { lbAvailableVideoTracks.ItemsSource = availableTracks; });
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }        
        }

        // This function creates a list of the selected tracks.
        private void createSelectedTracksList(List<IManifestTrack> selectedTracks)
        {
            // Create a list of selected tracks
            for (int j = 0; j < availableTracks.Count; j++)
            {
                if (availableTracks[j].isCheckedValue == true)
                {
                    selectedTracks.Add(availableTracks[j].TrackInfo);
                }
            }
        }

        // This function selects the tracks based on user selection 
        private void changeTracks(List<IManifestTrack> selectedTracks)
        {
            IManifestStream videoStream = getVideoStream();
            try
            {
                videoStream.SelectTracks(selectedTracks);
            }
            catch (Exception ex)
            {
                txtStatus.Text = ex.Message;
            }
        }
        #endregion track selection

5. Leidke mediaElement_ManifestReady meetodit, lisab funktsioon lõpus järgmine kood:

        getTracks(manifestObject);
        refreshAvailableTracksListBoxItemSource();

6. Sees Avaleht ainekursust, leidke Kasutajaliidese nuppude klõpsake sündmuste piirkond ja seejärel lisage järgmine funktsioon määratlus:

        private void btnChangeStream_Click(object sender, RoutedEventArgs e)
        {
            List<IManifestStream> selectedStreams = new List<IManifestStream>();

            // Create a list of the selected streams
            createSelectedStreamsList(selectedStreams);

            // Change streams on the presentation
            changeStreams(selectedStreams);
        }

**Koostada ja testida rakendus**

1. Vajutage klahvi **F6** projekti koostada. 
2.  Vajutage klahvi **F5** käivitage rakendus.
3.  Rakenduse ülaservas, saate kasutada vaikimisi sujuva voogesituse URL või sisestada mõne muu. 
4.  Klõpsake **andmeallika määramine**. 
5.  Vaikimisi on valitud kõik videovoo lood. Bitine intressimäära muutuste katsetused saate valida saadaval bitine alammäära, ja valige kõrgeim bitine määr. Pärast iga muudatuse peab klõpsake nuppu Edasta.  Saate vaadata videokvaliteet muudatused.

Õppetüki 4 on lõpule viidud.  Selle õppetüki lisada saate valida jälitab funktsionaalsus.


##<a name="media-services-learning-paths"></a>Media Servicesi Õppeteed

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="other-resources"></a>Muud ressursid:
- [Kuidas luua sujuva voogesituse Windows 8 JavaScripti rakenduse Täpsemad funktsioonid](http://blogs.iis.net/cenkd/archive/2012/08/10/how-to-build-a-smooth-streaming-windows-8-javascript-application-with-advanced-features.aspx)
- [Sujuva voogesituse tehniline ülevaade](http://www.iis.net/learn/media/on-demand-smooth-streaming/smooth-streaming-technical-overview)

[PlayerApplication]: ./media/media-services-build-smooth-streaming-apps/SSClientWin8-1.png
[CodeViewPic]: ./media/media-services-build-smooth-streaming-apps/SSClientWin8-2.png
 
