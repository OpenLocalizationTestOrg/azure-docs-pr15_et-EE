<properties 
    pageTitle="Kuidas teha reaalajas streaming Azure Media Servicesi abil saate luua mitme bitikiirusega voogu Azure portaali | Microsoft Azure'i" 
    description="Selles õppeteemas tutvustatakse juhiseid, mis saab ühe bitikiirusega reaalajas voo ja kodeeritakse see mitme bitikiirusega voo Azure'i portaalis kanali loomine." 
    services="media-services" 
    documentationCenter="" 
    authors="anilmur" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article"
    ms.date="10/24/2016"
    ms.author="juliako"/>


#<a name="how-to-perform-live-streaming-using-azure-media-services-to-create-multi-bitrate-streams-with-the-azure-portal"></a>Kuidas teha reaalajas streaming loomine mitme bitikiirusega voogu Azure'i portaalis Azure Media Servicesi kaudu

> [AZURE.SELECTOR]
- [Portaal](media-services-portal-creating-live-encoder-enabled-channel.md)
- [.NET-I](media-services-dotnet-creating-live-encoder-enabled-channel.md)
- [REST API-GA](https://msdn.microsoft.com/library/azure/dn783458.aspx)

Selles õpetuses juhendab teid **kanali** , mida saab ühe bitikiirusega reaalajas voo ja kodeeritakse see mitme bitikiirusega voo loomise juhiseid.

>[AZURE.NOTE]Kontseptuaalne seotud kanalid, mis on lubatud reaalajas kodeerimine, Lisateavet [Live streaming loomine mitme bitikiirusega voogu Azure Media Servicesi kaudu](media-services-manage-live-encoder-enabled-channels.md).

##<a name="common-live-streaming-scenario"></a>Reaalajas Streaming stsenaarium

Järgmised üldised juhised seotud levinud reaalajas streaming rakenduse loomiseks.

>[AZURE.NOTE] Praegu on soovitatav max live sündmus kestab 8 tundi. Kui peate käivitama kanali jaoks pikemaks, võtke ühendust amslived veebisaidil Microsoft.com.

1. Videokaamera arvutiga. Käivitage ja konfigureerige kohapealse reaalajas kodeerija, mida saate väljund ühe bitrate voo ühel järgmistest protokollid: RTMP, sujuva voogesituse või RTP (MPEG-TS). Lisateabe saamiseks vt [Azure Media Services RTMP tugi ja Live kodeerimisseadmetest](http://go.microsoft.com/fwlink/?LinkId=532824).
    
    Selles etapis tuleb võivad täita ka pärast oma kanali loomiseks.

1. Loomine ja käivitamine kanali. 

1. Too kanali neelata URL-i. 

    Neelata URL-i kasutatakse reaalajas kodeerija voo saatmiseks kanal.
1. Saate tuua eelvaade kanali URL. 

    Seda URL-i abil veenduda, et teie kanal on õigesti saab reaalajas voo.

3. Looge sündmuse/programmi, (mis loob ka vara). 
1. Avaldamine sündmusega (loob on seotud vara nõudmisel locator).  

    Veenduge, et on vähemalt üks streaming reserveeritud üksus, millest soovite voogesituseks streaming lõpp-punkti.
1. Alustada juhul, kui olete valmis alustama streaming ja arhiivimine.
2. Soovi korral saate reaalajas kodeerija märku, reklaami käivitamiseks. Reklaami lisatakse väljundi voo.
1. Iga kord, kui soovite peatada streaming ja arhiivimine sündmus sündmus lõpetada.
1. Sündmuse kustutamine (ja kustutada vara).   

##<a name="in-this-tutorial"></a>Selles õpetuses

Selles õpetuses kasutatakse Azure portaali tegemise järgmisi toiminguid: 

2.  Konfigureerige streaming lõpp-punktid.
3.  Mis on lubatud teha reaalajas kodeerimine kanali loomine.
1.  Saada neelata URL, et edastada seda live kodeerija. Reaalajas kodeerija kasutab seda URL-i neelata voo kanali sisse. .
1.  Sündmuse/programmis (ja vara) loomine
1.  Vara avaldamine ja saada streaming URL-id  
1.  Sisu esitamine 
2.  Puhastamine

##<a name="prerequisites"></a>Eeltingimused

Õpetuse lõpule viimiseks on nõutav.

- Selle õpetuse tegemiseks peate Azure'i konto. Kui teil pole kontot, saate luua tasuta prooviversiooni konto vaid paar minutit. Lisateavet leiate teemast [Azure tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/).
- Media Servicesi konto. Media Servicesi konto loomiseks vaadake teemat [Loo konto](media-services-portal-create-account.md).
- Kodeerija, mille saate saata ühe bitrate ja veebikaamera live voo.

##<a name="configure-streaming-endpoints"></a>Streaming lõpp-punktid konfigureerimine 

Media Servicesi pakub dünaamiline pakendit, mis võimaldab teil esitada oma mitme bitikiirusega MP4s streaming failivormingud: MPEG MÕTTEKRIIPSU, HLS, sujuva voogesituse või HDS, pole vaja uuesti sisse neid vorminguid streaming paketti. Dünaamiliste pakendit peate ainult talletada ja ühe salvestusruumi-vormingus failide eest maksta ja Media Servicesi ehitada ja teeni sobiv vastus taotluste klientrakenduses.

Dünaamiliste pakendit ära, peate vähemalt üks streaming ühiku saamiseks streaming lõpp-punkti, millest plaanite kohaletoimetamise sisu.  

Luua ja muuta streaming reserveeritud üksuste arvu, tehke järgmist.

1. Valige konto AMS [Azure portaali](https://portal.azure.com/) sisse logida.
1. Klõpsake aknas **sätted** **Streaming lõpp-punktid**. 

2. Klõpsake nuppu lõpp-punkti streaming vaikimisi. 

    Kuvatakse aken **Vaikimisi STREAMING lõpp-punkti üksikasjad** .

3. Streaming ühikute määramiseks libistage liugurit **Streaming üksused** .

    ![Streaming ühikud](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-streaming-units.png)

4. Klõpsake muudatuste salvestamiseks nuppu **Salvesta** .

    >[AZURE.NOTE]Mis tahes uute üksuste eraldatud kuluda kuni 20 minutit.

##<a name="create-a-channel"></a>KANALI loomine

1. [Azure'i portaalis](https://portal.azure.com/)valige Media Services ja seejärel klõpsake oma Media Servicesi konto nimi.
2. Valige **voogedastuse**.
3. Valige **kohandatud loomine**. See suvand annab teile kanali, mis on lubatud reaalajas kodeerimine loomine.

    ![Kanali loomine](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-channel.png)
    
4. Klõpsake **sätted**.
    
    1.  Valige **Live kodeerimine** kanali tüüp. Seda tüüpi saate määrata, kas soovite luua kanal, mis on lubatud reaalajas kodeerimine. See tähendab, et sissetulevad ühe bitrate voo on saadetud kanalit ja kodeeritakse mitme bitikiirusega voo, kasutades määratud reaalajas kodeerija sätteid. Lisateabe saamiseks lugege teemat [Live streaming loomine mitme bitikiirusega voogu Azure Media Servicesi kaudu](media-services-manage-live-encoder-enabled-channels.md). Klõpsake nuppu OK.
    2. Määrake soovitud kanali nimi.
    3. Ekraani allservas nuppu OK.
    
5. Valige vahekaart **neelata** .

    1. Sellel lehel saate valida streaming protokolli. **Reaalajas kodeerimine** kanali Type sobivat protokolli suvandid on järgmised.
        
        - Ühe bitrate killustunud MP4 (sujuva voogesituse)
        - Ühe bitrate RTMP
        - RTP (MPEG-TS): MPEG-2 Transport voo RTP üle.
        
        Leiate üksikasjaliku selgituse iga protokolli, [Live streaming loomine mitme bitikiirusega voogu Azure Media Servicesi kaudu](media-services-manage-live-encoder-enabled-channels.md).
    
        Te ei saa muuta Protocol (protokoll) suvand ajal kanali või seotud sündmused/programmid töötavad. Kui vajate erinevaid protokolle, peaksite looma eraldi kanalid iga streaming protokolli.  

    2. Saate rakendada IP piirang neelata kohta. 
    
        Saate määratleda neelata video selle kanali lubatud IP-aadressid. Lubatud IP-aadresside saab määrata ühe IP-aadress (nt 10.0.0.1), IP-vahemiku, IP-aadress ja CIDR alamvõrgu mask (nt 10.0.0.1/22) abil või IP-vahemiku, kasutades IP-aadress ja punktiirjoonega kümnendkoha alamvõrgu maski (nt "10.0.0.1(255.255.252.0)').

        Kui IP-aadressid on määratud ning puudub reegli määratlus siis IP-aadress on lubatud. IP-aadressi lubamiseks reegli loomine ja määramine 0.0.0.0/0.

6. Rakendage menüü **Eelvaade** IP piirangut eelvaate.
7. Määrake vahekaardil **kodeerimis** kodeering valmissäte. 

    Praegu ainult süsteemi valige valmissäte on **vaikimisi 720 p**. Kui soovite määrata kohandatud valmissäte, avage Microsofti tugiteenuste Piletite. Sisestage nimi, mis loodi teile valmiskombinatsiooni. 

>[AZURE.NOTE] Praegu kanali algus võib kuluda kuni 30 minutit. Kanali lähtestamine võib kuluda kuni 5 minutit.

Kui olete loonud kanalit, saate kanali klõpsake ja valige **sätted** , kus saate vaadata oma kanalite konfiguratsioone. 

Lisateabe saamiseks lugege teemat [Live streaming loomine mitme bitikiirusega voogu Azure Media Servicesi kaudu](media-services-manage-live-encoder-enabled-channels.md).


##<a name="get-ingest-urls"></a>Hangi neelata URL-id

Kui kanali on loodud, saate URL-id, et reaalajas kodeerija varustavate neelata. Kodeerija kasutab idest reaalajas voo sisestamiseks.

![ingesturls](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-ingest-urls.png)


##<a name="create-and-manage-events"></a>Sündmuste loomine ja haldamine

###<a name="overview"></a>Ülevaade

Kanali on seotud sündmused/programmi, mis võimaldavad teil määrata avaldamise ja lõikude reaalajas voo salvestusruumi. Kanalite haldamine sündmuste/programmid. Kanali ja programmi seos on väga sarnane traditsioonilise meedia, kus kanali on pidev vool sisu ja programmi on rakendatud mõni selle kanali Ajastatud sündmus.

Saate määrata tunde, mida soovite säilitada salvestatud sisu sündmuse **Arhiivi akna** pikkuse. Seda väärtust saab määrata vähemalt 5 minutit kuni 25 tundi. Arhiivi akna pikkus näitab ka suurt hulka aja kliendid saavad taotleda ajas tagasi reaalajas praegust paigutust. Sündmuste käitamise üle määratud aja jooksul, kuid sisu, mis langeb pikkus akna taga pidevalt hüljatakse. See selle atribuudi väärtust määratleb ka kaua kliendi manifestid võivad kasvada.

Iga sündmuse on seostatud vara. Sündmuse avaldada, peate looma ka nõudmisel locator seotud vara. Millel see locator võimaldab teil koostada streaming URL, mida saate sisestada oma klientidele.

Kanali toetab kuni kolm samaaegselt töötava sündmused nii, et saate luua mitu sama sissetuleva voo arhiivi. See võimaldab teil avaldada ja arhiivi erinevate osade sündmuse vastavalt vajadusele. Näiteks oma ettevõtte nõue on arhiivida sündmuse 6 tunni, kuid ainult viimase 10 minuti levisaadet edastada. Selle saavutamiseks, peate looma kaks samaaegselt töötab sündmus. Ühe sündmuse on seatud arhiivida ürituse 6 tunni, kuid programmi avaldamata. Muu sündmuse on seatud arhiivida 10 minutit ja see programm on avaldatud.

Ei peaks korduvalt kasutada olemasoleva programmide uute sündmuste jaoks. Selle asemel, luua ja iga uue programmi käivitamine.

Sündmuse/programmi käivitamine, kui olete valmis alustama streaming ja arhiivimine. Iga kord, kui soovite peatada streaming ja arhiivimine sündmus sündmus lõpetada. 

Arhiivitud sisu kustutamiseks peatada ja kustutada sündmus ja seejärel kustutage seotud vara. Vara ei saa kustutada, kui seda kasutatakse sündmust; sündmuse tuleb esmalt kustutada. 

Isegi juhul, kui saate peatada ja kustutada sündmus, kasutajad saaks voona arhiivitud sisu nõudmisel, videona jaoks, kui te ei kustutaks vara.

Kui soovite säilitada arhiivitud sisu, kuid see ei oleks saadaval streaming, kustutada streaming locator.

###<a name="createstartstop-events"></a>Sündmuste loomine ja käivitamine/Peata

Kui teil on kanali voolav voo alustamist streaming sündmus on varade, programmide ja Streaming Locator loomisega. Selle voo arhiivimine ja vaatajatele Streaming lõpp-punkti kaudu kättesaadavaks teha. 

On kaks võimalust sündmuse käivitamiseks. 

1. Vajutage lehelt **kanali** **Live sündmus** uue sündmuse lisamiseks.

    Määramiseks tehke järgmist: sündmuse nimi, vara nimi, arhiivi akna ja krüptimise suvand.
    
    ![createprogram](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-program.png)
    
    Kui jätate **avaldada reaalajas sündmuse nüüd** märgitud, sündmuse AVALDA URL-ID hankimine luuakse.
    
    Võite vajutada **alustada**, iga kord, kui olete valmis voona sündmus.

    Kui hakkate sündmus, võite vajutada **jälgimise** alustamiseks sisu.

2. Teise võimalusena saate kasutada otsetee ja vajutage nuppu **Go Live** **kanali** lehele. See loob vaikimisi varade, programmide ja Streaming Locator.

    Sündmuse nimi on **vaikimisi** ja arhiivi akna väärtuseks 1 tund.

Saate vaadata avaldatud sündmuse lehelt **Live sündmus** . 

Kui klõpsate **Välja Air**, see katkestab kõik reaalajas sündmused. 


##<a name="watch-the-event"></a>Vaata sündmuse

Sündmust jälgida, klõpsake nuppu **Vaata** Azure'i portaalis või kopeerige streaming URL ja kasutada player oma valik. 
 
![Loodud](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-play-event.png)

Reaalajas sündmuse teisendab automaatselt sündmused nõudmisel sisu, kui on peatatud.

##<a name="clean-up"></a>Puhastamiseks

Kui streaming sündmuste lõpetanud ja soovite puhastada ressursse, mis on varem ette valmistatud, tehke järgmist.

- Peatage lükkamine voo kodeerija kaudu.
- Kanali lõpetada. Pärast kanali on peatatud, ei kanna tasusid. Kui teil on vaja seda uuesti käivitada, siis on sama URL-i neelata, seega ei pea te oma kodeerija konfigureerimiseks.
- Teie Streaming lõpp-punkti, saate peatada juhul, kui soovite esitada oma reaalajas sündmuse voogu nõudmisel arhiivi jätkata. Kui kanali on peatatud olekus, seda kannavad tasusid.
  
##<a name="view-archived-content"></a>Arhiivitud sisu vaatamine

Isegi juhul, kui saate peatada ja kustutada sündmus, kasutajad saaks voona arhiivitud sisu nõudmisel, videona jaoks, kui te ei kustutaks vara. Vara ei saa kustutada, kui seda kasutatakse sündmuse; sündmuse tuleb esmalt kustutada. 

Hallata oma varasid, valige **seade** ja klõpsake nuppu **varad**.

![Varad](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-assets.png)

##<a name="considerations"></a>Kaalutlused

- Praegu on soovitatav max live sündmus kestab 8 tundi. Kui peate käivitama kanali jaoks pikemaks, võtke ühendust amslived veebisaidil Microsoft.com.
- Veenduge, et on vähemalt üks streaming reserveeritud üksus, millest soovite voogesituseks streaming lõpp-punkti.


##<a name="next-step"></a>Järgmise juhise juurde

Vaadake üle Media Servicesi õppeteemad.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

 
