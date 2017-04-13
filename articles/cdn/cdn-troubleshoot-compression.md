<properties
    pageTitle="Faili tihendamine Azure'i CDN tõrkeotsing | Microsoft Azure'i"
    description="Azure'i CDN faili tihendamine seotud probleemide tõrkeotsing."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="casoper"/>
    
# <a name="troubleshooting-cdn-file-compression"></a>Tõrkeotsingu CDN faili tihendamine

See artikkel aitab teil [CDN faili tihendamine](cdn-improve-performance.md)seotud probleemide tõrkeotsing.

Kui vajate rohkem abi, mis tahes hetkel selle artikli, võite pöörduda [Azure'i MSDN-i](https://azure.microsoft.com/support/forums/)ja virnas ületäitumise Foorumid Azure eksperdid. Teise võimalusena saate faili Azure'i tugiteenuse taotluse. [Azure'i tugiteabe veebisait](https://azure.microsoft.com/support/options/) ja klõpsake nuppu **Saada tugi**.

## <a name="symptom"></a>Sümptom

Tihendamise oma lõpp-punkti jaoks on lubatud, kuid failide tagastatakse tihendamata.

>[AZURE.TIP] Kontrollimaks, kas failide tagastatakse tihendatud, peate kasutama tööriista nagu [viiuldaja](http://www.telerik.com/fiddler) või brauseri [Arendaja tööriistad](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/).  Märkige HTTP vastus päised tagasi oma vahemällu talletatud CDN sisu.  Kui on nimega päise `Content-Encoding` **gzip**, **bzip2**või **deflate**väärtusega, on tihendatud sisu.
>
>![Päise sisu kodeerimine](./media/cdn-troubleshoot-compression/cdn-content-header.png)

## <a name="cause"></a>Põhjus

On mitu võimalikku põhjust, sh:

- Nõutud sisu ei ole tihendamine.
- Tihendamise nõutud faili tüüp pole lubatud.
- HTTP-päring ei sisalda päise paluda kehtiv tihendamise tüüp.

## <a name="troubleshooting-steps"></a>Tõrkeotsingu juhiste

> [AZURE.TIP] Koos võtavad uue lõpp-punktid, CDN konfiguratsioonimuudatuste võrgu kaudu levitada veidi aega võtta.  Muudatused rakendatakse tavaliselt 90 minuti jooksul.  Kui olete häälestanud tihendamise oma CDN lõpp-punkti esimest korda, kaaluge ootamine 1-2 tundi olla kindel, et sätted on olevatesse hüppab tihendamist. 

### <a name="verify-the-request"></a>Taotluse kinnitamine

Esmalt teeme kiirülevaate meelerahu sisse nõudmisel.  Saate oma brauseri [Arendaja tööriistad](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/) kuvamiseks on taotlusi.

- Veenduge, et teie lõpp-punkti URL-i, saadetakse kutse `<endpointname>.azureedge.net`, mitte oma origin ja.
- Veenduge, et taotluse sisaldab mõnda **Aktsepteeri kodeerimine** päis ja selle päise väärtus **gzip**, **deflate**või **bzip2**.

> [AZURE.NOTE] **Azure'i CDN kaudu Akamai** profiilid toetavad ainult **gzip** kodeerimine.

![CDN taotluse päised](./media/cdn-troubleshoot-compression/cdn-request-headers.png)

### <a name="verify-compression-settings-standard-cdn-profile"></a>Veenduge, et tihendussätted (Standard CDN profiil)

> [AZURE.NOTE] Selles etapis tuleb kehtib ainult siis, kui profiili CDN on **Azure CDN standardseid Verizon** või **Azure CDN Standard kaudu Akamai** profiil. 

Liikuge oma lõpp-punkti [Azure'i portaal](https://portal.azure.com) , ja klõpsake nuppu **Konfigureeri** .

- Veenduge, et tihendamise on lubatud.
- Veenduge, et MIME tüüp soovitud sisu saamiseks tuleb tihendatud sisaldub tihendatud arvuvormingute loendi.

![CDN tihendussätted](./media/cdn-troubleshoot-compression/cdn-compression-settings.png)

### <a name="verify-compression-settings-premium-cdn-profile"></a>Veenduge, et tihendussätted (Premium CDN profiil)

> [AZURE.NOTE] Selles etapis tuleb kehtib ainult siis, kui profiili CDN on **Azure CDN Premium Verizon** profiil.

Liikuge oma lõpp-punkti [Azure portaali](https://portal.azure.com) ja klõpsake nuppu **haldamine** .  Avatakse täiendavatele portaali.  Kursoriga **HTTP suur** vahekaarti ja seejärel kursoriga **Vahemälusätete** hüpik.  Klõpsake **tihendamise**. 

- Veenduge, et tihendamise on lubatud.
- Veenduge, et **Failitüüpide** loend sisaldab komaeraldusega loendi (ilma tühikuteta) MIME tüübid.
- Veenduge, et MIME tüüp soovitud sisu saamiseks tuleb tihendatud sisaldub tihendatud arvuvormingute loendi.

![CDN premium tihendussätted](./media/cdn-troubleshoot-compression/cdn-compression-settings-premium.png)

### <a name="verify-the-content-is-cached"></a>Veenduge, et sisu on vahemällu

> [AZURE.NOTE] Selles etapis tuleb kehtib ainult siis, kui profiili CDN on **Azure CDN Verizon** profiil (Standard- või Premium).

Brauseri arendustööriistu, märkige ruut vastus päised tagamaks, et fail on vahemällu ala, kui seda taotletakse.

- Märkige ruut **Server** vastuse päis.  Päise peaks olema vorming **platvormi (POP/serveri ID)**, nagu näha Järgnevas näites.
- Märkige ruut **X-vahemälu** vastuse päis.  Päise lugege **tulemus**.  

![CDN vastus päised](./media/cdn-troubleshoot-compression/cdn-response-headers.png)

### <a name="verify-the-file-meets-the-size-requirements"></a>Veenduge, et faili suurus vastab

> [AZURE.NOTE] Selles etapis tuleb kehtib ainult siis, kui profiili CDN on **Azure CDN Verizon** profiil (Standard- või Premium).

Saaks tihendamise, faili peab vastama järgmistele nõuetele suurus:

- Suurem kui 128 baiti.
- Väiksem kui 1 MB.

### <a name="check-the-request-at-the-origin-server-for-a-via-header"></a>Märkige ruut origin server **kaudu** päise taotlus

Päise **kaudu** HTTP näitab veebiserverisse, et kutse on on vastu võetud puhverserverit.  Microsoft IIS web serverite vaikimisi tihendamine vastuseid, kui taotlus sisaldab päise **kaudu** .  Selline käitumine alistada, tehke järgmist.

- **IIS 6**: [HcNoCompressionForProxies seatud "Väär" = IIS-i metaandmebaasi atribuudid](https://msdn.microsoft.com/library/ms525390.aspx)
- **IIS 7 ja kuni**: [nii **noCompressionForHttp10** ja **noCompressionForProxies** väär (FALSE) serveri konfiguratsiooni seadmine](http://www.iis.net/configreference/system.webserver/httpcompression)

