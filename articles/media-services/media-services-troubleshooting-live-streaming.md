<properties 
    pageTitle="Reaalajas streaming tõrkeotsingu juhend | Microsoft Azure'i" 
    description="See teema annab soovitusi reaalajas streaming probleemide tõrkeotsing." 
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
    ms.date="10/12/2016"  
    ms.author="juliako"/>

#<a name="troubleshooting-guide-for-live-streaming"></a>Reaalajas streaming tõrkeotsingujuhendi

See teema annab mõned reaalajas streaming probleemide tõrkeotsing soovitusi.

## <a name="issues-related-to-on-premises-encoders"></a>Kohapealse kodeerimisseadmetest seotud probleemid 

See jaotis annab soovitusi kohapealse kodeerimisseadmetest AMS kanalid, mis on lubatud reaalajas kodeering ühe bitrate voo saatmiseks konfigureeritud seotud probleemide tõrkeotsing.

###<a name="problem-would-like-to-see-logs"></a>Probleem: Soovite näha logid 

- **Probleem**: ei saa Otsi kodeerija logib, mis võib aidata silumine probleemid.
    
    - **Telestreami Wirecast**: leiate tavaliselt C:\Users logid\{username} \AppData\Roaming\Wirecast\ 
    - **Elementaarse Live**: saate otsida on lingid logid portaalis haldus. Klõpsake **statistika**ja seejärel **logid**. **Logifailide** lehel kuvatakse loendi logid LiveEvent üksuste; Valige oma praeguse seansi sobitamine. 
    - **Flash Media Live Encoderi**: leiate **Log Directory...** navigeerides **Kodeerimine Log** menüü.
    
###<a name="problem-there-is-no-option-for-outputting-a-progressive-stream"></a>Probleem: Puudub võimalus jaoks järk-järgult voo kirjutamine

- **Probleem**: kasutatava kodeerija ei automaatselt deinterlace. 

    **Tõrkeotsingu samme**: Otsige kodeerija kasutajaliideses ülerealaotusega suvandeid. Kui de-põimimine on lubatud, kontrollige uuesti järk-järgult väljastussätted. 
 
###<a name="problem-tried-several-encoder-output-settings-and-still-unable-to-connect"></a>Probleem: Proovinud mitu kodeerija väljastussätted ja ikka ei saa ühendust. 

- **Probleem**: Azure'i kodeering kanal pole õigesti lähtestada. 

    **Tõrkeotsingu samme**: Veenduge, et kodeerija pole enam lükkamine AMS, lõpetamine ja kanali lähtestada. Pärast uuesti käivitada, proovige oma kodeerija ühendavad uued sätted. Kui see endiselt ei paranda probleem, proovige luua täiesti uue kanali, mõnikord kanalite saate rikutud pärast mitut nurjus katsete.  

- **Probleem**: The GOP suuruse või klahv paneeli sätted ei ole optimaalse. 

    **Tõrkeotsingu samme**: soovitatav GOP suuruse või keyframe intervall on kaks sekundit all. Mõned kodeerimisseadmetest arvutada selle sätte arv raamid, kuigi teised kasutada sekundit. Näide: kui 30 sekundis kirjutamine, GOP suurus oleks 60 raamid, mis on võrdne kaks sekundit all.  
     
- **Probleem**: suletud pordid blokeerivad voo. 

    **Tõrkeotsingu samme**: kui voogesituse kaudu RTMP, kontrollige, kas/tulemüüri või puhverserveri sätteid nii, et kinnitada, et Väljaminev pordid 1935 ja 1936 on avatud. Kui kasutate RTP streaming, veenduge, et väljamineva pordi 2010 on avatud. 


###<a name="problem-when-configuring-the-encoder-to-stream-with-the-rtp-protocol-there-is-no-place-to-enter-a-host-name"></a>Probleem: Kodeerija voog konfigureerimisel RTP protokolli puudub koht sisestada hosti nimi. 

- **Probleem**: mitme RTP kodeerimisseadmetest ei võimalda hostinimed ja IP-aadress on vaja omandada.  

    **Tõrkeotsingu samme**: IP-aadressi leidmiseks avage käsuviip, mis tahes arvutis. Selleks Windows avage Käivita käivitit (WIN + R) ja tippige "cmd" avamiseks.  

    Kui käsuviiba on avatud, tippige "Ping [AMS hosti nimi]". 

    Hosti nimi saab tuletada jättes pordi number: Azure'i neelata URL-i, nagu järgmises näites: 

    RTP://test2-amstest009.RTP.Channel.mediaservices.Windows.net:2010 / 

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle10.png)

###<a name="problem-unable-to-playback-the-published-stream"></a>Probleem: Ei saa taasesitus avaldatud voo.
 
- **Probleem**: ei ole Streaming lõpp-punkti töötab või on streaming ühikuid (skaala ühikud) eraldatud. 

    **Tõrkeotsingu samme**: liikuge "Streaming lõpp-punkti" menüü AMSE tööriista ja kinnitage on Streaming Endpoint, töötamise streaming ühe üksuse. 
    


>[AZURE.NOTE] Pärast järgmisi tõrkeotsingu samme te endiselt ei saa edukalt voona, küsi tugi Azure portaalis.

##<a name="media-services-learning-paths"></a>Media Servicesi Õppeteed

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
