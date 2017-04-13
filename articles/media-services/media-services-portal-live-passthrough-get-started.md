<properties 
    pageTitle="Kuidas teha reaalajas voogesitus kohapealne kodeerimisseadmetest Azure'i portaalis | Microsoft Azure'i" 
    description="Selles õppeteemas tutvustatakse juhiseid, mis on konfigureeritud lubama läbiv kohaletoimetamise kanali loomine." 
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
    ms.topic="get-started-article"
    ms.date="10/24/2016" 
    ms.author="juliako"/>


#<a name="how-to-perform-live-streaming-with-on-premise-encoders-using-the-azure-portal"></a>Kuidas teha reaalajas voogesitus kohapealne kodeerimisseadmetest Azure'i portaalis

> [AZURE.SELECTOR]
- [Portaal]( media-services-portal-live-passthrough-get-started.md)
- [.NET-I]( media-services-dotnet-live-encode-with-onpremises-encoders.md)
- [ÜLEJÄÄNUD]( https://msdn.microsoft.com/library/azure/dn783458.aspx)

Selles õpetuses juhendab teid Azure portaali loomine **kanal** , mis on konfigureeritud lubama läbiv kohaletoimetamise abil juhiseid. 

##<a name="prerequisites"></a>Eeltingimused

Õpetuse lõpule viimiseks on nõutav:

- Azure'i konto. Lisateavet leiate teemast [Azure tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/). 
- Media Servicesi konto. Media Servicesi konto loomiseks teemast [Media Services konto loomine](media-services-portal-create-account.md).
- Veebikaamera. Näiteks [Telestreami Wirecast kodeerija](http://www.telestream.net/wirecast/overview.htm).

Soovitatav on väga vaadake järgmisi artikleid:

- [Azure Media Servicesi RTMP toetavad ja Live kodeerimisseadmetest](https://azure.microsoft.com/blog/2014/09/18/azure-media-services-rtmp-support-and-live-encoders/)
- [Ülevaade Live aurutatud Azure Media Servicesi kaudu](media-services-manage-channels-overview.md)
- [Kohapealne kodeerimisseadmetest mitme bitikiirusega voogu loodud streaming Live](media-services-live-streaming-with-onprem-encoders.md)


##<a id="scenario"></a>Reaalajas streaming stsenaarium

Järgmised sammud kirjeldavad seotud levinud reaalajas streaming rakendusi, mis kasutavad kanalid, mis on konfigureeritud läbiv kohaletoimetamiseks loomine. Selle õpetuse näitab, kuidas luua ja hallata läbiv kanali ja reaalajas sündmused.

1. Videokaamera arvutiga. Käivitage ja konfigureerige kohapealse reaalajas kodeerija, mis väljundid mitme bitikiirusega RTMP või killustatud MP4 voo. Lisateavet leiate teemadest [Azure Media Services RTMP tugi ja Live kodeerimisseadmetest](http://go.microsoft.com/fwlink/?LinkId=532824).
    
    Selles etapis tuleb võivad täita ka pärast oma kanali loomiseks.

1. Loomine ja käivitamine läbiv kanal.
1. Too kanali neelata URL-i. 

    Neelata URL-i kasutatakse reaalajas kodeerija voo saatmiseks kanal.
1. Saate tuua eelvaates kanali URL. 

    Seda URL-i abil veenduda, et teie kanal on õigesti saab reaalajas voo.

3. Looge reaalajas sündmuse/programmi. 

    Kui kasutate Azure portaali, reaalajas sündmuse loomise luuakse vara. 
      
    >[AZURE.NOTE]Veenduge, et on vähemalt üks streaming reserveeritud üksus, millest soovite voogesituseks streaming lõpp-punkti.
1. Kui olete valmis alustama streaming ja arhiivimine sündmuse/programmi käivitamine
2. Soovi korral saate reaalajas kodeerija märku, reklaami käivitamiseks. Reklaami lisatakse väljundi voo.
1. Peatage sündmuse/programmi iga kord, kui soovite peatada streaming ja arhiivimine sündmus.
1. Kustutage sündmus ja programmide (ja kustutada vara).     

>[AZURE.IMPORTANT] Palun vaadake üle, [kohapealne kodeerimisseadmetest, et luua mitme bitikiirusega voogu reaalajas voogesitus](media-services-live-streaming-with-onprem-encoders.md) põhimõtet ja reaalajas voogesitus kohapealne kodeerimisseadmetest ja läbiv kanalite seotud kaalutluste kohta.

##<a name="to-view-notifications-and-errors"></a>Tõrgete ja teadete vaatamine

Kui soovite vaadata teatised ja Azure portaali loodud tõrgete, klõpsake teavitusikooni.

![Teatised](./media/media-services-portal-passthrough-get-started/media-services-notifications.png)

##<a name="configure-streaming-endpoints"></a>Streaming lõpp-punktid konfigureerimine 

Media Servicesi pakub dünaamiline pakendit, mis võimaldab teil esitada oma mitme bitikiirusega MP4s streaming failivormingud: MPEG MÕTTEKRIIPSU, HLS, sujuva voogesituse või HDS, ilma neid vorminguid streaming sisse pakkige. Dünaamiliste pakendit peate ainult talletada ja ühe salvestusruumi-vormingus failide eest maksta ja Media Servicesi koostab ja pakutakse sobiv vastus taotluste klientrakenduses.

Dünaamiliste pakendit ära, peate vähemalt üks streaming ühiku saamiseks streaming lõpp-punkti, millest plaanite kohaletoimetamise sisu.  

Luua ja muuta streaming reserveeritud üksuste arvu, tehke järgmist.

1. Logige sisse veebisaidil [Azure portaali](https://portal.azure.com/).
1. Klõpsake aknas **sätted** **Streaming lõpp-punktid**. 

2. Klõpsake nuppu lõpp-punkti streaming vaikimisi. 

    Kuvatakse aken **Vaikimisi STREAMING lõpp-punkti üksikasjad** .

3. Streaming ühikute määramiseks libistage liugurit **Streaming üksused** .

    ![Streaming ühikud](./media/media-services-portal-passthrough-get-started/media-services-streaming-units.png)

4. Klõpsake muudatuste salvestamiseks nuppu **Salvesta** .

    >[AZURE.NOTE]Mis tahes uute üksuste eraldatud kuluda kuni 20 minutit.
    
##<a name="create-and-start-pass-through-channels-and-events"></a>Loomine ja käivitamine läbiv kanalid ja sündmused

Kanali on seotud sündmused/programmi, mis võimaldavad teil määrata avaldamise ja lõikude reaalajas voo salvestusruumi. Kanalite haldamine sündmused. 
    
Saate määrata tundide soovite säilitada **Arhiivi akna** pikkuse programmi salvestatud sisu. Seda väärtust saab määrata vähemalt 5 minutit kuni 25 tundi. Arhiivi akna pikkus näitab ka suurt hulka aja kliendid saavad taotleda ajas tagasi reaalajas praegust paigutust. Sündmuste käitamise üle määratud aja jooksul, kuid sisu, mis langeb pikkus akna taga pidevalt hüljatakse. See selle atribuudi väärtust määratleb ka kaua kliendi manifestid võivad kasvada.

Iga sündmuse on seostatud vara. Avaldada sündmus, peate looma ka nõudmisel locator seotud vara. Millel see locator võimaldab teil koostada streaming URL, mida saate sisestada oma klientidele.

Kanali toetab kuni kolm samaaegselt töötava sündmused nii, et saate luua mitu sama sissetuleva voo arhiivi. See võimaldab teil avaldada ja arhiivi erinevate osade sündmuse vastavalt vajadusele. Näiteks oma ettevõtte nõue on arhiivida 6 tunni programmi, kuid ainult viimase 10 minuti levisaadet edastada. Selle saavutamiseks, peate looma kaks samaaegselt töötavad programmid. Ühes programmis on seatud arhiivida sündmuse 6 tunni, kuid programm ei ole avaldatud. Muu programmi on seatud arhiivida 10 minutit ja see programm on avaldatud.

Mida peaks korduvalt kasutada reaalajas olemasolevate sündmuste. Selle asemel loomine ja käivitamine iga sündmuse uus sündmus.

Alustada juhul, kui olete valmis alustama streaming ja arhiivimine. Iga kord, kui soovite peatada streaming ja arhiivimine sündmus, sulgege see programm. 

Arhiivitud sisu kustutamiseks peatada ja kustutada sündmus ja seejärel kustutage seotud vara. Vara ei saa kustutada, kui seda kasutatakse sündmuse; sündmuse tuleb esmalt kustutada. 

Isegi juhul, kui saate peatada ja kustutada sündmus, kasutajad saaks voona arhiivitud sisu nõudmisel, videona jaoks, kui te ei kustutaks vara.

Kui soovite säilitada arhiivitud sisu, kuid see ei oleks saadaval streaming, kustutada streaming locator.

###<a name="to-use-the-portal-to-create-a-channel"></a>Kanali loomiseks kasutada portaali 

Selles jaotises kirjeldatakse lepingute abil **Kiiresti luua** suvand läbiv kanali loomine.

Läbiv kanalite kohta leiate lisateavet teemast [Live streaming kohapealne kodeerimisseadmetest, et luua mitme bitikiirusega voogu](media-services-live-streaming-with-onprem-encoders.md).

1. Valige [Azure portaali](https://portal.azure.com/)konto Azure Media Servicesi.
2. Klõpsake aknas **sätted** **Live streaming**. 

    ![Alustamine](./media/media-services-portal-passthrough-get-started/media-services-getting-started.png)
    
    Kuvatakse aken **Live streaming** .

3. Klõpsake nuppu **Kiiresti luua** läbiv kanali loomine ja selle RTMP neelata Protocol (protokoll).

    **Looge uus kanal** aken.
4. Tippige uue kanali nimi ja klõpsake nuppu **Loo**. 

    See loob läbiv kanali abil soovitud RTMP neelata Protocol (protokoll).

##<a name="create-events"></a>Sündmuste loomine

1. Valige kanali, millele soovite sündmuse lisamine.
2. Klõpsake nuppu **Live sündmus** .

![Sündmuse](./media/media-services-portal-passthrough-get-started/media-services-create-events.png)


##<a name="get-ingest-urls"></a>Hangi neelata URL-id

Kui kanali on loodud, saate URL-id, et reaalajas kodeerija varustavate neelata. Kodeerija kasutab idest reaalajas voo sisestamiseks.

![Loodud](./media/media-services-portal-passthrough-get-started/media-services-channel-created.png)

##<a name="watch-the-event"></a>Vaata sündmuse

Sündmust jälgida, klõpsake nuppu **Vaata** Azure'i portaalis või kopeerige streaming URL ja kasutada player oma valik. 
 
![Loodud](./media/media-services-portal-passthrough-get-started/media-services-default-event.png)

Reaalajas sündmuse automaatselt saada ümber tellitav sisu, kui on peatatud.

##<a name="clean-up"></a>Puhastamiseks

Läbiv kanalite kohta leiate lisateavet teemast [Live streaming kohapealne kodeerimisseadmetest, et luua mitme bitikiirusega voogu](media-services-live-streaming-with-onprem-encoders.md).

- Kanali saate peatada ainult siis, kui kõik sündmused programmid kanali on peatatud.  Pärast kanali on peatatud, ei kanna tasusid. Kui teil on vaja seda uuesti käivitada, siis on sama URL-i neelata, seega ei pea te oma kodeerija konfigureerimiseks.
- Kanali saab kustutada ainult siis, kui kõik reaalajas sündmused kanali on kustutatud.

##<a name="view-archived-content"></a>Arhiivitud sisu vaatamine

Isegi juhul, kui saate peatada ja kustutada sündmus, kasutajad saaks voona arhiivitud sisu nõudmisel, videona jaoks, kui te ei kustutaks vara. Vara ei saa kustutada, kui seda kasutatakse sündmuse; sündmuse tuleb esmalt kustutada. 

Hallata oma varasid, valige **seade** ja klõpsake nuppu **varad**.

![Varad](./media/media-services-portal-passthrough-get-started/media-services-assets.png)

##<a name="next-step"></a>Järgmise juhise juurde

Vaadake üle Media Servicesi õppeteemad.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
