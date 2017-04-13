<properties 
    pageTitle="Elementaarse Live kodeerija saata ühe bitrate reaalajas voo konfigureerimine | Microsoft Azure'i" 
    description="Selles teemas kirjeldatakse, kuidas konfigureerida elementaarse Live kodeerija AMS kanalid, mis on lubatud reaalajas kodeering ühe bitrate voo saatmiseks." 
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
    ms.date="10/12/2016"
    ms.author="cenkdin;anilmur;juliako"/>

#<a name="use-the-elemental-live-encoder-to-send-a-single-bitrate-live-stream"></a>Elementaarse Live kodeerija abil saate saata ühe bitrate reaalajas voo

> [AZURE.SELECTOR]
- [Reaalajas oksiidvormis](media-services-configure-elemental-live-encoder.md)
- [Tricaster](media-services-configure-tricaster-live-encoder.md)
- [Wirecast](media-services-configure-wirecast-live-encoder.md)
- [FMLE](media-services-configure-fmle-live-encoder.md)

Selles teemas kirjeldatakse, kuidas konfigureerida saata ühe bitrate voo AMS kanalid, mis on lubatud reaalajas kodeeringus [Elementaarse Live](http://www.elementaltechnologies.com/products/elemental-live) kodeerija.  Lisateavet leiate teemast [kanalid, mis on lubatud teha Live kodeeringus Azure Media Servicesi koos töötamine](media-services-manage-live-encoder-enabled-channels.md).

Selle õpetuse näitab, kuidas hallata Azure Media Services Explorer (AMSE) tööriista Azure Media Services (AMS). See tööriist töötab ainult Windows Arvutis. Kui kasutate Maci või Linuxi, Azure portaali abil saate luua [kanalid](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) ja [programmid](media-services-portal-creating-live-encoder-enabled-channel.md#create-and-manage-a-program).

##<a name="prerequisites"></a>Eeltingimused

- Peab olema tundma elementaarse Live liidese abil luua reaalajas sündmused.
- [Looge konto Azure Media Servicesi](media-services-portal-create-account.md)
- Tagada Streaming Endpoint, töötamise vähemalt üks streaming ühiku eraldatud. Lisateavet leiate teemast [Haldamine Streaming lõpp-punktid Media Services konto](media-services-portal-manage-streaming-endpoints.md).
- Installige tööriist [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) uusim versioon.
- Käivitage tööriist ja AMS kontoga ühenduse loomiseks.

##<a name="tips"></a>Näpunäited

- Võimaluse korral kasutada hardwired Interneti-ühendus.
- Hea rusikareegel läbilaskevõime nõuete määramisel on topeltklõpsake streaming bitikiirus. Kui see pole kohustuslik tingimus, aitab see võrgu ülekoormuse leevendada.
- Kui tarkvaraga vastavalt kodeerimisseadmetest, sulgege välja kõik mittevajalikud programmid.

## <a name="elemental-live-with-rtp-ingest"></a>Elementaarse Live keskpaika neelata

Selles jaotises kirjeldatakse elementaarse Live kodeerija, mis saadab ühe bitrate reaalajas voo üle RTP konfigureerimine.  Lisateavet leiate teemast [MPEG-TS voo RTP üle](media-services-manage-live-encoder-enabled-channels.md#channel).

### <a name="create-a-channel"></a>Kanali loomine

1.  AMSE tööriista, liikuge **Live** vahekaarti ja klõpsake alal kanali. Valige **Loo kanal...** menüüst.

![Elementaarse](./media/media-services-elemental-live-encoder/media-services-elemental1.png)

2. Määrake kanali nimi, kirjeldus väli on vabatahtlik. Kanali sätete, valige **Standard** suvandi Live kodeerimine RTP **(MPEG-TS)**seadmine sisendit protokolli. Kõik muud sätted, nagu on jätta.


Veenduge, et **alustada uue kanali nüüd** oleks märgitud.

3. Klõpsake **kanalit luua**.
![Elementaarse](./media/media-services-elemental-live-encoder/media-services-elemental12.png)

>[AZURE.NOTE] Kanali võib kuluda kuni 20 minutit, et alustada.

Ajal käivitub kanali saate [konfigureerida kodeerija](media-services-configure-elemental-live-encoder.md#configure_elemental_rtp).

>[AZURE.IMPORTANT] Pange tähele, et arveldamine algab kohe, kui kanali on valmis tõrkeolukorras. Lisateavet leiate teemast [kanali olekus](media-services-manage-live-encoder-enabled-channels.md#states).

###<a id=configure_elemental_rtp></a>Elementaarse Live kodeerija konfigureerimine 

Selles õpetuses kasutatakse järgmist väljastussätted. Ülejäänud selles jaotises kirjeldatakse konfigureerimistoimingute üksikasjalikumalt. 

**Video**:
 
- Kodek: H.264. 
- Profiil: Kõrge (tase 4.0) 
- Bitikiirus: 5000 kbps 
- Keyframe: kaks sekundit all (60 sekundi järel) 
- Raami määr: 30
 
**Heli**:

- Kodek: AAC (LC) 
- Bitikiirus: 192 kbps 
- Proovi Rate: 44,1 kHz


####<a name="configuration-steps"></a>Konfigureerimistoimingute

1. Liikuge **Elementaarse Live** web kasutajaliidese ja **UDP/TS** streaming kodeerija häälestamine. 

2. Kui luuakse uus sündmus, liikuge kerides allapoole jaotiseni väljundi rühmad ja **UDP/TS** väljundi rühma lisada. 

3. Luua uue väljundi, valides **Uus voo** ja seejärel klõpsata käsku **Lisa väljund**.  
    
    ![Elementaarse](./media/media-services-elemental-live-encoder/media-services-elemental13.png)
    
    >[AZURE.NOTE] Soovitatav on elementaarse sündmuse Code, mis on seatud "Süsteemikell" aidata kodeerija voo tõrke korral taastada.

4. Nüüd, kui väljund on loodud, klõpsake nuppu **Lisa voo**. Nüüd saab väljundi sätteid konfigureerida. 
5. Liikuge kerides allapoole jaotiseni "Voo 1" äsja loodud, klõpsake vahekaarti **Video** vasakul ja **Täpsemalt** jaotise sätted laiendamiseks. 

    ![Elementaarse](./media/media-services-elemental-live-encoder/media-services-elemental4.png)

    Kuigi elementaarse Live kohandamiseks on saadaval mitmesuguseid, järgmised sätted on soovitatav voogesitus AMS töötamise alustamine. 
    
    - Lahendus: 1280 x 720 
    - Sekundis: 30 
    - GOP suurus: 60 paneelid 
    - Interlace režiimi: järk-järgult 
    - Bitikiirus: 5000000 bit/s (see saab reguleerida võrgu piirangud alusel) 
    

    ![Elementaarse](./media/media-services-elemental-live-encoder/media-services-elemental5.png)

6. Kanali Sisestuskeel URL-i hankimine
    
    Liikuge tagasi AMSE tööriista ja kanali lõpetatusoleku vaadata. Kui olek on muutunud **alustades** **töötab**, saate sisestatud URL-i.
      
    Kui kanali töötab, paremklõpsake kanali nimi, liikuge allapoole hover üle **Kopeeri sisestatud URL lõikelauale** ja seejärel valige **Esmane sisestatud URL-i**.  
    
    ![Elementaarse](./media/media-services-elemental-live-encoder/media-services-elemental6.png)
    
1. Kleepige see teave oksiidvormis **Esmane** sihtvälja. Kõik muud sätted saate jäävad vaikimisi.
    
    ![Elementaarse](./media/media-services-elemental-live-encoder/media-services-elemental14.png)

    Jaoks täiendav koondamise, korrake neid juhiseid teisene sisestatud URL-iga, luues eraldi "Väljundi" menüü UDP/TS streaming.
    
7. Klõpsake **loomine** (kui see on loodud uue sündmuse) või **värskendus** (kui olemasolev sündmuse redigeerimine) ja siis jätkake kodeerija käivitamiseks. 

>[AZURE.IMPORTANT]Enne käsu **käivitamine** elementaarse Live web interface, te **peate** veenduge, et kanali on valmis. 
>Lisaks veenduge, et mitte lahkuda kanali ilma sündmuse rohkem kui 15 minutit > valmis olekus.

Pärast voo peab töötama 30 sekundit, liikuge tagasi AMSE tööriista ja testige taasesitus.  

###<a name="test-playback"></a>Testi taasesitus
  
1. Liikuge AMSE tööriista ja paremklõpsake kanalit testitakse. Klõpsake menüü **taasesitus eelvaate** kursorit ja valige **Azure Media Player**.  

    ![Elementaarse](./media/media-services-elemental-live-encoder/media-services-elemental8.png)

Kui voo kuvatakse Playeris, siis kodeerija on õigesti konfigureeritud AMS ühendamiseks. 

Kui tõrke on saanud, tuleb kanali lähtestada ja kodeerija sätteid kohandada. Palun vaadake [tõrkeotsingu](media-services-troubleshooting-live-streaming.md) juhiseid teemas.   

###<a name="create-a-program"></a>Rakenduse loomine

1. Kui kanali taasesitus on kinnitatud, looge programmi. Tööriista AMSE vahekaardil **Live** programmi alal paremklõps ja valige **Loo uus programmi**.  

    ![Elementaarse](./media/media-services-elemental-live-encoder/media-services-elemental9.png)

2. Programmi nime ja vajadusel reguleerida **Arhiivi akna pikkus** (mis on vaikimisi 4 tundi). Saate määrata Salvestuskoht või jätke vaikimisi.  
3. Märkige ruut **Käivita programm kohe** .
4. Klõpsake nuppu **Loo programmi**.  
  
    Märkus: Programmi loomine võtab vähem aega, kui kanali loomine.    
 
5. Kui programm töötab, kinnitage taasesitus paremklõpsake klõpsates programm ja liikumine **taasesitus soovitud programmid** ja seejärel käsku **Azure Media Player**.  
6. Kui kinnitatud, Paremklõpsake programmi uuesti ja valige **väljundi URL lõikelauale kopeerida** (või selle teabe toomiseks **programmiteabe ja sätete** suvand menüüst). 

Voog on nüüd valmis mängija manustatud või jaotatud publikule reaalajas vaatamiseks.  

## <a name="troubleshooting"></a>Tõrkeotsing

Palun vaadake [tõrkeotsingu](media-services-troubleshooting-live-streaming.md) juhiseid teemas. 


##<a name="media-services-learning-paths"></a>Media Servicesi Õppeteed

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
