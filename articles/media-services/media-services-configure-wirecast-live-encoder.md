<properties 
    pageTitle="Konfigureerimine Telestreami Wirecast kodeerija saata ühe bitrate reaalajas voo | Microsoft Azure'i" 
    description="Selles teemas kirjeldatakse, kuidas konfigureerida Wirecast reaalajas kodeerija saata ühe bitrate voo AMS kanalite reaalajas kodeerimine jaoks lubatud. " 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ne" 
    ms.topic="article" 
    ms.date="10/12/2016"
    ms.author="juliako;cenkdin;anilmur"/>

#<a name="use-the-wirecast-encoder-to-send-a-single-bitrate-live-stream"></a>Ühe bitrate reaalajas voo saatmine Wirecast kodeerija abil

> [AZURE.SELECTOR]
- [Wirecast](media-services-configure-wirecast-live-encoder.md)
- [Reaalajas oksiidvormis](media-services-configure-elemental-live-encoder.md)
- [Tricaster](media-services-configure-tricaster-live-encoder.md)
- [FMLE](media-services-configure-fmle-live-encoder.md)

Selles teemas kirjeldatakse, kuidas konfigureerida [Telestreami Wirecast](http://www.telestream.net/wirecast/overview.htm) reaalajas kodeerija saata ühe bitrate voo AMS kanalite reaalajas kodeerimine jaoks lubatud.  Lisateavet leiate teemast [kanalid, mis on lubatud teha Live kodeeringus Azure Media Servicesi koos töötamine](media-services-manage-live-encoder-enabled-channels.md).

Selle õpetuse näitab, kuidas hallata Azure Media Services Explorer (AMSE) tööriista Azure Media Services (AMS). See tööriist töötab ainult Windows Arvutis. Kui kasutate Maci või Linuxi, Azure portaali abil saate luua [kanalid](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) ja [programmid](media-services-portal-creating-live-encoder-enabled-channel.md#create-and-manage-a-program).


##<a name="prerequisites"></a>Eeltingimused

- [Looge konto Azure Media Servicesi](media-services-portal-create-account.md)
- Tagada Streaming Endpoint, töötamise vähemalt üks streaming ühiku eraldatud. Lisateavet leiate teemast [Haldamine Streaming lõpp-punktid Media Services konto](media-services-portal-manage-streaming-endpoints.md)
- Installige tööriist [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) uusim versioon.
- Käivitage tööriist ja AMS kontoga ühenduse loomiseks.

##<a name="tips"></a>Näpunäited

- Võimaluse korral kasutada hardwired Interneti-ühendus.
- Hea rusikareegel läbilaskevõime nõuete määramisel on topeltklõpsake streaming bitikiirus. Kui see pole kohustuslik tingimus, aitab see võrgu ülekoormuse leevendada.
- Kui tarkvaraga vastavalt kodeerimisseadmetest, sulgege välja kõik mittevajalikud programmid.


## <a name="create-a-channel"></a>Kanali loomine

1.  AMSE tööriista, liikuge **Live** vahekaarti ja klõpsake alal kanali. Valige **Loo kanal...** menüüst.

![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast1.png)

2. Määrake kanali nimi, kirjeldus väli on vabatahtlik. Kanali sätete, valige **Standard** suvandi Live kodeerimine Sisestusvahemik määratud **RTMP**protokolli. Kõik muud sätted, nagu on jätta.


Veenduge, et **alustada uue kanali nüüd** oleks märgitud.

3. Klõpsake **kanalit luua**.
![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast2.png)

>[AZURE.NOTE] Kanali võib kuluda kuni 20 minutit, et alustada.

Ajal käivitub kanali saate [konfigureerida kodeerija](media-services-configure-wirecast-live-encoder.md#configure_wirecast_rtmp).

>[AZURE.IMPORTANT] Pange tähele, et arveldamine algab kohe, kui kanali on valmis tõrkeolukorras. Lisateavet leiate teemast [kanali olekus](media-services-manage-live-encoder-enabled-channels.md#states).

##<a id=configure_wirecast_rtmp></a>Telestreami Wirecast kodeerija konfigureerimine

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


###<a name="configuration-steps"></a>Konfigureerimistoimingute

1. Avage Telestreami Wirecast rakendus arvuti on kasutatud ja RTMP streaming häälestamine.
2. Liikumine vahekaardile **väljundi** ja valides **Väljastussätted...**konfigureerimine väljund.
    
    Veenduge, et **Väljundi sihtkoht** on seatud **RTMP Server**.
3. Klõpsake nuppu **OK**.
4. Määrake lehel sätted **sihtvälja **Azure Media Servicesi**olevat** .
 
    Profiili kodeering on eelnevalt valitud **Azure'i**h.264 720 p 16:9 (1280 x 720). Nende sätete kohandamiseks valige rippmenüüst paremal hammasrattaikooni allapoole ja valige **Uus eelmääratud**.

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast3.png)

5. Konfigureerige kodeerija valmiskombinatsioonid.

    Nime valmissäte ja otsida Soovitatavad sätted järgmist.

    **Video**
    
    - Kodeerija: MainConcept H.264.
    - Paneelid sekundis: 30
    - Keskmine natuke määr: 5000 kbits/sec (saate kohandada võrgu piirangud alusel)
    - Profiil: põhi
    - Võtme paneeli iga: 60 paneelid

    **Heli**

    - Target bit rate: 192 kbits/sec
    - Proovi Rate: 44,100 kHz
     
    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast4.png)

6. Vajutage **salvestada**.

    Väli kodeerimis on nüüd äsja loodud profiili valida. 

    Veenduge, et valitud on uus profiil.

7. Saada kanali Sisestuskeel URL-i, et määrata Wirecast **RTMP lõpp-punkti**.
    
    Liikuge tagasi AMSE tööriista ja kanali lõpetatusoleku vaadata. Kui olek on muutunud **alustades** **töötab**, saate sisestatud URL-i.
      
    Kui kanali töötab, paremklõpsake kanali nimi, liikuge allapoole hover üle **Kopeeri sisestatud URL lõikelauale** ja seejärel valige **Esmane sisestatud URL-i**.  
    
    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast6.png)

8. Wirecast **Väljastussätted** aken, kleepige see teave väljale **aadress** väljundi jaotise ja voo nime määrata. 


    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast5.png)

9. Klõpsake nuppu **OK**.

10. Kinnitage **Wirecast** põhikuval Sisestuskeel allikad video ja heli on valmis ja seejärel vajutage **voo** klõpsake vasakus ülanurgas.

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast7.png)

>[AZURE.IMPORTANT]Enne, kui klõpsate **voo**, mida **peate** veenduge, et kanali on valmis. 
>Lisaks veenduge, et mitte lahkuda kanali valmis olekus ilma on rohkem kui 15 minutit > kanali Sisestuskeel osa.

##<a name="test-playback"></a>Testi taasesitus
  
1. Liikuge AMSE tööriista ja paremklõpsake kanalit testitakse. Klõpsake menüü **taasesitus eelvaate** kursorit ja valige **Azure Media Player**.  

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast8.png)

Kui voo kuvatakse Playeris, siis kodeerija on õigesti konfigureeritud AMS ühendamiseks. 

Kui tõrke on saanud, tuleb kanali lähtestada ja kodeerija sätteid kohandada. Palun vaadake [tõrkeotsingu](media-services-troubleshooting-live-streaming.md) juhiseid teemas.  

##<a name="create-a-program"></a>Rakenduse loomine

1. Kui kanali taasesitus on kinnitatud, looge programmi. Tööriista AMSE vahekaardil **Live** programmi alal paremklõps ja valige **Loo uus programmi**.  

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast9.png)

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
