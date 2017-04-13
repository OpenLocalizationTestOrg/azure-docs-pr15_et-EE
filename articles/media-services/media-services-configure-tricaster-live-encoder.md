<properties 
    pageTitle="Konfigureerimine NewTek TriCaster kodeerija saata ühe bitrate reaalajas voo | Microsoft Azure'i" 
    description="Selles teemas kirjeldatakse, kuidas konfigureerida Tricaster reaalajas kodeerija saata ühe bitrate voo AMS kanalid, mis on lubatud reaalajas kodeeringus." 
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
    ms.author="juliako;cenkd;anilmur"/>

#<a name="use-the-newtek-tricaster-encoder-to-send-a-single-bitrate-live-stream"></a>Ühe bitrate reaalajas voo saatmine NewTek TriCaster kodeerija abil

> [AZURE.SELECTOR]
- [Tricaster](media-services-configure-tricaster-live-encoder.md)
- [Reaalajas oksiidvormis](media-services-configure-elemental-live-encoder.md)
- [Wirecast](media-services-configure-wirecast-live-encoder.md)
- [FMLE](media-services-configure-fmle-live-encoder.md)

Selles teemas kirjeldatakse, kuidas konfigureerida [NewTek TriCaster](http://newtek.com/products/tricaster-40.html) reaalajas kodeerija saata ühe bitrate voo AMS kanalite reaalajas kodeerimine jaoks lubatud. Lisateavet leiate teemast [kanalid, mis on lubatud teha Live kodeeringus Azure Media Servicesi koos töötamine](media-services-manage-live-encoder-enabled-channels.md).

Selle õpetuse näitab, kuidas hallata Azure Media Services Explorer (AMSE) tööriista Azure Media Services (AMS). See tööriist töötab ainult Windows Arvutis. Kui kasutate Maci või Linuxi, Azure portaali abil saate luua [kanalid](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) ja [programmid](media-services-portal-creating-live-encoder-enabled-channel.md#create-and-manage-a-program).

>[AZURE.NOTE]Kanali AMS kanalid, mis on lubatud reaalajas kodeerimine kaasa saatmiseks Tricaster kasutamisel ei saa heli tõrgete oma reaalajas juhul kui kasutate teatud funktsioone Tricaster kiire lõikamist kanalite vahel või tahvlid ja sealt vahetamine. AMS meeskonna järgmiste probleemide korral, millega seni, see on ei soovita nende funktsioonide kasutamiseks.


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

![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster1.png)

2. Määrake kanali nimi, kirjeldus väli on vabatahtlik. Kanali sätete, valige **Standard** suvandi Live kodeerimine Sisestusvahemik määratud **RTMP**protokolli. Kõik muud sätted, nagu on jätta.


Veenduge, et **alustada uue kanali nüüd** oleks märgitud.

3. Klõpsake **kanalit luua**.
![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster2.png)

>[AZURE.NOTE] Kanali võib kuluda kuni 20 minutit, et alustada.


Ajal käivitub kanali saate [konfigureerida kodeerija](media-services-configure-tricaster-live-encoder.md#configure_tricaster_rtmp).

>[AZURE.IMPORTANT] Pange tähele, et arveldamine algab kohe, kui kanali on valmis tõrkeolukorras. Lisateavet leiate teemast [kanali olekus](media-services-manage-live-encoder-enabled-channels.md#states).

##<a id=configure_tricaster_rtmp></a>NewTek TriCaster kodeerija konfigureerimine

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

1. Sõltuvalt sellest, millist video sisendandmete allikas kasutatakse **NewTek TriCaster** uue projekti loomine 
2. Üks kord selle projekti nupp **voo** otsimine ja klõpsake hammasratta ikooni kõrval voo konfiguratsiooni menüü avamiseks.

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster3.png)
3. Kui menüü on avatud, klõpsake nuppu **Uus** ühenduse pealkirja all. Kui küsitakse ühenduse tüüp, valige **Adobe Flash**.

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster4.png)

4. Klõpsake nuppu **OK**.

5. Nüüd saab importida FMLE profiil on rippmenüü noolt klõpsates jaotise **Streaming profiil** ja **sirvida**liikumine.

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster5.png)

6. Liikuge, kuhu on salvestatud konfigureeritud FMLE profiili.
7. Valige see ja klõpsake nuppu **OK**.

    Kui profiili on üles laaditud, jätkake järgmise juhisega.

6. Saada kanali Sisestuskeel URL-i, et määrata Tricaster **RTMP lõpp-punkti**.
    
    Liikuge tagasi AMSE tööriista ja kanali lõpetatusoleku vaadata. Kui olek on muutunud **alustades** **töötab**, saate sisestatud URL-i.
      
    Kui kanali töötab, paremklõpsake kanali nimi, liikuge allapoole hover üle **Kopeeri sisestatud URL lõikelauale** ja seejärel valige **Esmane sisestatud URL-i**.  
    
    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster6.png)

7. Kleepige see teave Projecti Tricaster jaotises **Serveri Flash** välja **asukoht** . Määrata ka voo **Voo ID** välja nimi. 

    Kui voo on lisatud FMLE profiilile, seda saab ka importida see jaotis, klõpsates **Impordisätete**, liikumine salvestatud FMLE profiil ja klõpsake nuppu **OK**. Flash Server väljad peaks asustada FMLE teave.

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster7.png)

9. Kui olete lõpetanud, klõpsake nuppu **OK** ekraani allosas. Kui heli ja video sisendeid on Tricaster sisse, alustage AMS voogesitus, klõpsates nuppu **voo** .

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster11.png)

>[AZURE.IMPORTANT]Enne, kui klõpsate **voo**, mida **peate** veenduge, et kanali on valmis. 
>Lisaks veenduge, et mitte lahkuda kanali valmis olekus ilma on rohkem kui 15 minutit > kanali Sisestuskeel osa. 

##<a name="test-playback"></a>Testi taasesitus
  
1. Liikuge AMSE tööriista ja paremklõpsake kanalit testitakse. Klõpsake menüü **taasesitus eelvaate** kursorit ja valige **Azure Media Player**.  

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster8.png)

Kui voo kuvatakse Playeris, siis kodeerija on õigesti konfigureeritud AMS ühendamiseks. 

Kui tõrke on saanud, tuleb kanali lähtestada ja kodeerija sätteid kohandada. Palun vaadake [tõrkeotsingu](media-services-troubleshooting-live-streaming.md) juhiseid teemas.  

##<a name="create-a-program"></a>Rakenduse loomine

1. Kui kanali taasesitus on kinnitatud, looge programmi. Tööriista AMSE vahekaardil **Live** programmi alal paremklõps ja valige **Loo uus programmi**.  

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster9.png)

2. Programmi nime ja vajadusel reguleerida **Arhiivi akna pikkus** (mis on vaikimisi 4 tundi). Saate määrata Salvestuskoht või jätke vaikimisi.  
3. Märkige ruut **Käivita programm kohe** .
4. Klõpsake nuppu **Loo programmi**.  
  
    Märkus: Programmi loomine võtab vähem aega, kui kanali loomine.    
 
5. Kui programm töötab, kinnitage taasesitus paremklõpsake klõpsates programm ja liikumine **taasesitus soovitud programmid** ja seejärel käsku **Azure Media Player**.  
6. Kui kinnitatud, Paremklõpsake programmi uuesti ja valige **väljundi URL lõikelauale kopeerida** (või selle teabe toomiseks **programmiteabe ja sätete** suvand menüüst). 

Voog on nüüd valmis mängija manustatud või jaotatud publikule reaalajas vaatamiseks.  


## <a name="troubleshooting"></a>Tõrkeotsing

Palun vaadake [tõrkeotsingu](media-services-troubleshooting-live-streaming.md) juhiseid teemas. 


##<a name="next-step"></a>Järgmise juhise juurde

Vaadake üle Media Servicesi õppeteemad.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
