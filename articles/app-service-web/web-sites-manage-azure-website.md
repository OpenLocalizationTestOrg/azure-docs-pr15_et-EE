<properties 
    pageTitle="Veebirakenduse Azure'i rakendust Service haldamine" 
    description="Haldamise Azure'i rakendust Service veebirakenduse ressursside lingid." 
    services="app-service\web" 
    documentationCenter="" 
    authors="erikre" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/24/2016" 
    ms.author="rachelap"/>

# <a name="manage-a-web-app-in-azure-app-service"></a>Veebirakenduse Azure'i rakendust Service haldamine

See teema hõlmab lingid seostuvatele ressurssidele haldamise veebirakenduse teenuses [Azure rakendus](http://go.microsoft.com/fwlink/?LinkId=529714). Haldus hõlmab kõiki toiminguid, et hoida oma veebirakenduse sujuvalt. 

Web appi kestuse jooksul saate teha eri haldamise toiminguid, kui teisaldate esialgse kasutuselevõtu tavapärase ja hooldus värskendused.

Azure'i portaalis saab teha palju web appi haldamisega seotud toiminguid.

## <a name="before-you-deploy-your-web-app-to-production"></a>Enne juurutamist oma veebirakenduse tootmisele

### <a name="choose-a-tier"></a>Valige soovitud taseme

Azure'i rakenduse teenus on saadaval viis astme: vaba, ühiskasutuses, Basic, Standard, ja. Funktsioonid ja iga taseme jaoks hinnad kohta leiate teavet teemast [hinnad üksikasjad](/pricing/details/app-service/). 

- [Rakenduse teenuse lepingute](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) abil saate rühmitada sama jaotises mitme veebirakenduste.
- Saate alati [vahetada astme](web-sites-scale.md) pärast loomist oma veebirakenduse.

### <a name="configuration"></a>Konfigureerimine

[Azure portaali](https://portal.azure.com/) abil saate määrata konfiguratsiooni erinevaid võimalusi. Lisateavet leiate teemast [konfigureerimine web apps kasutamine Azure'i rakendust Service](web-sites-configure.md). Siin on kiire kontroll-loend.

- Valige **käitusaja versioonide** .NET, PHP, Java või Python, vajaduse korral.
- Luba **WebSockets** kui oma veebirakenduse kasutab WebSocket Protocol (protokoll). (See sisaldab rakendused, mis kasutavad [ASP.net-i SignalR](http://www.asp.net/signalr) või [socket.io](web-sites-nodejs-chat-app-socketio.md).)
- Kas kasutate pidev tõstab? Kui jah, luba **Alati**.
- Määrake **vaikimisi dokumendi**, nt index.html.

Lisaks seadistada neid sätteid soovite konfigureerida järgmist:

- **Turvasoklite kiht (SSL)** krüptimist. SSL-i kohandatud domeeninime kasutamiseks tuleb saada SSL-sert ja seda kasutada oma veebirakenduse konfigureerimine. Leiate [Azure'i rakendust Service web app HTTPS lubamine](web-sites-configure-ssl-certificate.md).
- **Kohandatud domeeni nimi.** Oma veebirakenduse on automaatselt alamdomeeni jaotises azurewebsites.net. Saate seostada kohandatud domeeninime, nt contoso.com. Vaadake [konfigureerimine teenuses Azure rakenduse kohandatud domeeni nimi](web-sites-custom-domain-name.md).

Keelekohase konfiguratsioon:

- **PHP**: [Azure'i rakendust Service veebirakendustes PHP konfigureerimine](web-sites-php-configure.md).
- **Python**: [Python veebirakendustega Azure'i rakendust Service konfigureerimine](web-sites-python-configure.md)


## <a name="while-your-web-app-is-running"></a>Oma veebirakenduse töötamise ajal

Oma veebirakenduse töötamise, mida soovite veenduge, et see on saadaval ja, et see skaala kasutaja liikluse täita. Peate võib-olla ka tõrkeotsing.

### <a name="monitoring"></a>Jälgimine

- Azure'i portaali kaudu saate [lisada jõudluse mõõdikute](web-sites-monitor.md) nagu CPU hõivatus ja kliendi taotluste arv.
- [Skaala oma veebirakenduse](web-sites-scale.md) liikluse vastuseks. Sõltuvalt teie taseme saate muudate VM juhtudel suurus ja/või VMs arv. Klõpsake Standard- ja Premium astme, te saate häälestada ka autoscaling, nii, et oma veebirakenduse skaala automaatselt kindla intervalliga või vastuseks laadimine.  
 
### <a name="backups"></a>Varukoopiate

- Määrake oma veebirakenduse [automaatvarunduse](web-sites-backup.md) . Lugege lisateavet [selle](https://azure.microsoft.com/documentation/videos/azure-websites-automatic-and-easy-backup/)video varukoopiad.
- Siit saate teada, [andmebaasi taastamine](../sql-database/sql-database-business-continuity.md) Azure SQL-andmebaasis võimaluste kohta.

### <a name="troubleshooting"></a>Tõrkeotsing

- Kui midagi läheb valesti, saate [tõrkeotsing Visual Studios](web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug), kasutades diagnostikalogid ja reaalajas silumine pilveteenuses. 
- Visual Studio väljaspool on mitmel viisil koguda diagnostikalogid. Vt [diagnostika logimine veebirakenduste teenuses Azure rakenduse lubamine](web-sites-enable-diagnostic-log.md).
- Node.js rakenduste, vaadake, [Kuidas silumine Node.js veebirakenduse teenuses Azure rakendus](web-sites-nodejs-debug.md).

### <a name="restoring-data"></a>Andmete taastamine

- [Taastamine](web-sites-restore.md) web appi, mis oli varem varundada.


## <a name="when-you-update-your-web-app"></a>Kui olete oma veebirakenduse

Kui teil on lubatud automaatvarunduse, saate luua [käsitsi varundamise](web-sites-backup.md).

Kaaluge [etapiviisilise juurutamise](web-sites-staged-publishing.md). Selle suvandiga saate avaldada värskenduste lavastus juurutus, mis töötab kõrvuti tootmise juurutamise abil. 

Kui kasutate Visual Studio meeskonnatöö teenuseid, saate häälestada pidev juurutamine andmeallika juhtelemendi:

- [Team Foundation versiooni kontrollimine (TFVC) abil](../cloud-services/cloud-services-continuous-delivery-use-vso.md) 
- [Git abil](../cloud-services/cloud-services-continuous-delivery-use-vso-git.md)
 
<!-- Anchors. -->

[Before you deploy your site to production]: #before-you-deploy-your-site-to-production
[While your website is running]: #while-your-website-is-running
[When you update your website]: #when-you-update-your-website

  
