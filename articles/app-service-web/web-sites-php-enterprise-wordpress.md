<properties
    pageTitle="Azure'i rakenduse teenuse äriklassi WordPress | Microsoft Azure'i"
    description="Saate teada, kuidas majutada äriklassi WordPress saidil Azure'i rakendust Service"
    services="app-service\web"
    documentationCenter=""
    authors="sunbuild"
    manager="yochayk"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.devlang="php"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="web"
    ms.date="10/24/2016"
    ms.author="sumuth"/>

# <a name="enterprise-class-wordpress-on-azure-app-service"></a>Azure'i rakenduse teenuse äriklassi WordPress

Azure'i rakenduse teenus pakub scalable turvaline ning lihtne kasutada ülesanne kriitilised suuremahuline [WordPress] jaoks[ wordpress] saidid. Microsoft ise töötab äriklassi saitidel nagu [Office] [ officeblog] ja [Bingi] [ bingblog] ajaveebide. See dokument näitab, kuidas saate kasutada Azure rakenduse teenuse veebirakenduste loomine ja säilitamine soovitud äriklassi, pilvepõhist WordPress saidi, et külastajad suurel hulgal hakkama.

## <a name="architecture-and-planning"></a>Arhitektuur ja kavandamine

Ainult kahe nõuded on installimise põhilised WordPress.

* **MySQL-andmebaasiga** - [ClearDB Azure'i turuplatsil]kaudu saadaolevad[cdbnstore], või oma MySQL-i installimise kohta Azure'i Virtuaalmasinates kas [Windows] abil saate hallata[ mysqlwindows] või [Linuxi][mysqllinux].

  > [AZURE.NOTE] ClearDB pakub mitu MySQL-i konfiguratsioone erinevatele omadustega iga konfiguratsiooni. Leiate [Azure'i poe] [ cdbnstore] teavet Azure poe kaudu või otse [ClearDB](http://www.cleardb.com/pricing.view)kodulehel pakkumisi.

* **PHP 5.2.4 või suurem** -Azure'i rakendust Service praegu anda [PHP versioonid 5.4, 5,5, ja 5.6][phpwebsite].

    > [AZURE.NOTE] Soovitatav alati PHP, et teil oleks uusim turvalisus parandused uusima versiooni.

### <a name="basic-deployment"></a>Tavaline juurutamine

Ainult nõuetele saanud luua lihtsa lahendust Azure piirkond.

![ühe Azure piirkonnas majutatud leiate Azure'i web appi ning MySQL-andmebaasiga][basic-diagram]

Kuigi see võimaldab teil skaala-out rakenduse Web Appsi mitu eksemplari saidi loomise teel, kõik majutatakse andmekeskuste kindla geograafilise piirkonna sees. Külastajaid väljaspool seda võidakse kuvada aeglane vastus times saidi kasutamisel ja andmekeskuste selle piirkonna minna, seega ei rakenduse.


### <a name="multi-region-deployment"></a>Mitme piirkonna juurutamine

Azure'i [Liikluse haldur][trafficmanager], on võimalik mastaapimiseks WordPress saidi külastajad ainult üks URL, tagades mitme geograafiliste piirkondade lõikes. Külastajaid tulevad kaudu liikluse haldur ja seejärel marsruuditakse piirkonnas koormust tasakaalustavad konfiguratsiooni põhjal.

![Azure'i veebirakendus majutatud mitme piirkondades CDBR kõrge-saadavus ruuteri marsruutimiseks MySQL-i piirkondade lõikes][multi-region-diagram]

Iga piirkonna WordPress saidi oleks mastaabitud endiselt üle mitme veebirakenduste aknad, kuid see skaleerimist on piirkond teatud; veebiaadresside piirkondade saate mastaabitud väikese liikluse neist erineda.

Kopeerimine ja marsruutimise mitme MySQL-i andmebaaside saab teha abil ClearDB's [CDBR kõrge kättesaadavus ruuteri] [ cleardbscale] (kuvatakse vasakul) või [MySQL-i kobar CGE][cge].

### <a name="multi-region-deployment-with-media-storage-and-caching"></a>Mitme piirkonna juurutus, nii et media salvestus- ja vahemällu talletamine
Kui saidi võtab üles või host meediumifaile, kasutage Azure'i bloobimälu. Kui teil on vaja vahemällu, kaaluge [Redis vahemälu][rediscache], [Memcache pilves](http://azure.microsoft.com/gallery/store/garantiadata/memcached/), [MemCachier](http://azure.microsoft.com/gallery/store/memcachier/memcachier/)või mõnda muud vahemällu pakkumiste [Azure'i poe](http://azure.microsoft.com/gallery/store/).

![Azure'i veebirakendus majutatud mitme piirkondades CDBR kõrge-saadavus ruuteri MySQL-i vahemälu hallatavate, bloobimälu ja CDN-ID][performance-diagram]

Bloobimälu on jaotatud geo piirkondade vaikimisi nii, et te ei pea muretsema imitatsiooniga failid üle kõigi saitide kohta. Saate lubada Azure [Sisu jaotuse Network (CDN)] [ cdn] jaoks bloobimälu, mis jagab failide lõpetamiseks külastajad sõlmed lähemale.

### <a name="planning"></a>Plaanimine

#### <a name="additional-requirements"></a>Lisanõuded

Selle tegemiseks... | Kasutage seda …
------------------------|-----------
**Üles- või suuri faile talletada.** | [WordPress lisandmooduli kasutamise bloobimälu][storageplugin]
**Meilisõnumi saatmine** | [SendGrid] [ storesendgrid] ja [WordPress lisandmooduli kasutamise SendGrid][sendgridplugin]
**Kohandatud domeeni nimed** | [Kohandatud domeeninime Azure'i rakendust Service konfigureerimine][customdomain]
**HTTPS** | [Veebirakenduse teenuses Azure rakenduse HTTPS-i lubamine][httpscustomdomain]
**Tootmiseelse valideerimine** | [Lavastus keskkonnas web Appsi teenuses Azure rakenduse häälestamine][staging] <p>Üleminek web appi kaudu lavastus tootmisele ka viib WordPress konfigureerimine. Veenduge, et kõik sätted on värskendatud tootmise rakenduse nõuded enne etapiviisilise rakenduse vahetamist tootmisse.</p>
**Jälgimine ja tõrkeotsing** | [Diagnostika logimise veebirakendustes teenuses Azure rakenduse] [ log] ja [Teenuses Azure rakenduse Web rakenduste jälgimine][monitor]
**Oma saidi juurutamine** | [Veebirakenduse teenuses Azure rakenduse juurutamine][deploy]

#### <a name="availability-and-disaster-recovery"></a>Kättesaadavus ja Avariijärgne taaste

Selle tegemiseks... | Kasutage seda …
------------------------|-----------
**Laadi saldo saidid** või **geo-saitide levitamine** | [Liikluse marsruutimiseks Azure'i liikluse haldur][trafficmanager]
**Varundamine ja taastamine** | [Varundamine web appi teenuses Azure rakenduse] [ backup] ja [veebirakenduse teenuses Azure rakenduse taastamine][restore]

#### <a name="performance"></a>Jõudluse

Jõudluse pilveteenuses on saavutada peamiselt vahemällu talletamine ja skaala välja; Siiski tuleks kaaluda mälu, läbilaskevõime ja muud atribuudid majutusteenuse veebirakenduste.

Selle tegemiseks... | Kasutage seda …
------------------------|-----------
**Rakenduse teenuse eksemplari võimaluste mõistmine** | [Hinnad üksikasjad, sh rakenduse teenuse astme võimaluste][websitepricing]
**Vahemälu ressursid** | [Redis vahemälu][rediscache], [Memcache pilves](/gallery/store/garantiadata/memcached/), [MemCachier](/gallery/store/memcachier/memcachier/)või mõnda muud vahemällu pakkumiste [Azure'i poe](/gallery/store/)
**Rakenduse skaala** | [Teenuses Azure rakenduse web appi mastaapimiseks] [ websitescale] ning [ClearDB kõrge kättesaadavus marsruutimise][cleardbscale]. Kui valite majutada ja hallata oma MySQL-i installi, võiksite kaaluda [MySQL-i kobar CGE] [ cge] skaala-out jaoks

#### <a name="migration"></a>Migreerimise

On kaks võimalust, Azure'i rakendust Service olemasoleva WordPress saidi migreerimine.

* ** [WordPress eksportimine] [ export] ** – see eksport ajaveebi saab importida uue WordPress saidile teenuses Azure rakenduse [WordPress importija lisandmooduli]abil sisu[import].

    > [AZURE.NOTE] Ajal selle protsessi võimaldab teil oma sisu migreerida, see migreerida kõik lisandmoodulid, teemade või muude kohandused. Need peab olema installitud uuesti käsitsi.

* **Käsitsi migreerimise** - [varundada oma saidi] [ wordpressbackup] ja [andmebaasi][wordpressdbbackup], siis käsitsi taastamiseks web appi teenuses Azure rakendus ja seotud MySQL-i andmebaasi migreerimine väga kohandatud saidid ja vältida tüdimus käsitsi installimise lisandmoodulid, kujunduste ja muud kohandused.

## <a name="step-by-step-instructions"></a>Üksikasjalikud juhised

### <a name="create-a-wordpress-site"></a>WordPress saidi loomine

1. Kasutage [Azure'i turuplatsi] [ cdbnstore] loomine MySQL-i andmebaasi suuruse, mille te määrasite jaotises [arhitektuur ja kavandamine](#planning) , et majutate oma saidi piirkonna.

2. Juhised leiate [Azure'i rakendust Service WordPress veebirakenduse loomine] [ createwordpress] WordPress web app loomiseks. Veebirakenduse loomisel **kasutada olemasoleva andmebaasi MySQL-i** ja valige andmebaasi juhises loodud 1.

Kui migreerite WordPress olemasoleval saidil, lugege teemat [Azure olemasoleva WordPress saidi migreerimine](#Migrate-an-existing-WordPress-site-to-Azure) pärast uue veebirakenduse loomine.

### <a name="migrate-an-existing-wordpress-site-to-azure"></a>Azure'i olemasoleva WordPress saidi migreerimine

Nagu osa [arhitektuur ja kavandamise](#planning) , on kaks võimalust WordPress saidi migreerimine.

* **eksportimine ja importimine** - saitide ilma suurel määral kohandamist või kui soovite lihtsalt teisaldada sisu.

* **varundus ja taaste** - saitide palju kohandamine, kuhu soovite teisaldada kõik.

Üks järgmistest jaotistest abil saate migreerida saidile.

#### <a name="the-export-and-import-method"></a>Impordi ja ekspordi meetod

1. Kasutage [WordPress eksportimine] [ export] eksportida oma olemasoleval saidil.

2. Luua veebirakenduse [WordPress saidi loomine](#Create-a-new-WordPress-site) jaotise juhiseid abil.

3. Logige sisse oma WordPress saidi veebirakenduste ja klõpsake nuppu **lisandmoodulid** -> **Lisa uus**. Otsida ja installida, **WordPress importija** lisandmoodul.

4. Pärast importija lisandmoodul on installitud, klõpsake nuppu **Tööriistad** -> **impordi**ja seejärel valige **WordPress** WordPress importija lisandmooduli kasutama.

5. Klõpsake lehel **Impordi WordPress** **Faili valimine**. Otsige sirvides üles eksporditud olemasoleva WordPress saidi WXR fail ja valige **faili üles laadida ja importida**.

6. Klõpsake **esitada**. Teil palutakse importimine õnnestus.

8. Kui olete täitnud kõiki neid samme, taaskäivitage oma web appi blade saidi [Azure portaali][mgmtportal].

Pärast importimist saidi, peate järgmiste toimingute lubamiseks ei sisalda impordifaili sätted.

Kui kasutasite seda … | Toiming
------------------ | ----------
**Permalinks** | Uue saidi WordPress armatuurlaud, valige **sätted** -> **Permalinks** ning seejärel update Permalinks struktuur
**pildi/lingid** | Kasutada linkide värskendamiseks uude asukohta [Velvet Blues värskendada URL-ide lisandmooduli][velvet], otsing ja asendus tööriista või käsitsi andmebaasis
**Kujundused** | Minge **ilme** -> **teema** ja update saidi kujunduse vastavalt vajadusele
**Menüüde** | Kui kujunduse toetab menüüde, lingid avalehe võib-olla vanad manustatud neid sub kataloogi. Minge **ilme** -> **menüüde** ja nende värskendamine

#### <a name="the-backup-and-restore-method"></a>Varundus ja taaste meetod

1. Varundage oma olemasoleva WordPress saidi [WordPress varukoopiate]teabe abil[wordpressbackup].

2. Teabe abil [oma andmebaasi varundada]olemasoleva andmebaasi varundamine[wordpressdbbackup].

3. Andmebaasi loomine ja taastamine varukoopia.

    1. Osta uue andmebaasi [Azure'i turuplatsi][cdbnstore], või häälestamine [Windows] MySQL-andmebaasiga[ mysqlwindows] või [Linuxi] [ mysqllinux] VM.

    2. MySQL-i kliendi nagu [MySQL-i töölaua][workbench], uue andmebaasiga ühenduse loomiseks ja WordPress andmebaasi importida.

    3. Värskendage andmebaasist uue Azure'i rakendust Service domeeni domeeni kirjete muutmine. Näiteks mywordpress.azurewebsites.net. Kasutage [otsimiseks ja asendamiseks WordPress andmebaaside skripti] [ searchandreplace] turvaline muuta kõik eksemplarid.

4. Web appi Azure portaali loomine ja avaldamine WordPress varukoopia.

    1. Luua veebirakenduse [Azure portaali] [ mgmtportal] abil **Uus**andmebaas -> **Web + Mobile** -> **Azure'i turuplatsi** -> **Veebirakenduste** -> **veebirakenduse + SQL-i** (või **veebirakenduse + MySQL-i**) -> **Loo**. Luua tühja web app kõiki nõutud sätteid konfigureerida.

    2. Otsige üles fail **wp-config.php** ja avage see toimetaja WordPress varukoopia. Asendage järgmised kirjed uue MySQL-i andmebaasi teave.

        * **DB_NAME** - andmebaasi kasutajanimi

        * **DB_USER** - kasutaja nimi, mida kasutatakse andmebaasile juurde pääseda

        * **DB_PASSWORD** - kasutaja parooli

        Pärast muutuvad need kirjed, salvestage ja sulgege fail **wp-config.php** .

    3. Kasutage [Deploy web appi teenuses Azure rakenduse] [ deploy] teavet, mis võimaldab juurutamise meetodit, mida soovite kasutada, ja seejärel juurutada varukoopia WordPress Azure'i rakendust Service veebirakendusse.

5. Kui kasutusele võetud WordPress veebilehte, siis peaks olema juurdepääs uus sait (nagu rakenduse teenuse web app) kasutades soovitud *. azurewebsite.net saidi URL.

### <a name="configure-your-site"></a>Oma saidi konfigureerimine

Pärast saidi WordPress on loodud või migreeritud, kasutage järgmist teavet jõudluse parandamiseks lubamiseks või muid funktsioone.

Selle tegemiseks... | Kasutage seda …
------------- | -----------
**Rakenduse teenuse leping režiimi, suurus ja luba skaleerimist seadmine** | [Mastaapimiseks teenuses Azure rakenduse web app][websitescale]
**Andmebaasi püsiv andmeühenduste lubamine** | Vaikimisi ei kasuta WordPress püsivate andmebaasi ühendused, mis võivad põhjustada ühenduse loomiseks muutuvad rakendus pärast mitu ühendust andmebaasi. Püsivate ühendused, installige (püsiv ühendused adapterit lisandmoodul) [https://wordpress.org/plugins/persistent-database-connection-updater/installation/]. 
**Jõudluse parandamiseks** | <ul><li><p><a href="http://ppe.blogs.msdn.com/b/windowsazure/archive/2013/11/18/disabling-arr-s-instance-affinity-in-windows-azure-web-sites.aspx">Keela ARR küpsise</a> – saate parandada jõudlust käivitamisel WordPress veebirakenduste mitmes eksemplaris</p></li><li><p>Luba vahemällu. <a href="http://msdn.microsoft.com/library/azure/dn690470.aspx">Redis vahemälu</a> <a href="https://wordpress.org/plugins/redis-object-cache/">Objekti vahemälu WordPress lisandmooduli Redis</a>või kasutage ühte muud vahemällu pakkumiste <a href="/gallery/store/">Azure'i poe</a> kaudu saab kasutada (eelvaade)</p></li><li><p><a href="http://ruslany.net/2010/03/make-wordpress-faster-on-iis-with-wincache-1-1/">Kuidas teha WordPress kiiremini Wincache</a> - Wincache on vaikimisi Web Apps</p></li><li><p><a href="../web-sites-scale/">Veebirakenduse teenuses Azure rakenduse ulatuse</a> ja <a href="http://www.cleardb.com/developers/cdbr/introduction">ClearDB kõrge kättesaadavus marsruutimine</a> või <a href="http://www.mysql.com/products/cluster/">MySQL-i kobar CGE</a> kasutamine</p></li></ul>
**Kasutage plekid Storage** | <ol><li><p><a href="../storage-create-storage-account/">Azure Storage konto loomine</a></p></li><li><p>Siit saate teada, kuidas kasutada <a href="../cdn-how-to-use/">Sisu jaotuse Network (CDN)</a> geo-levitamine plekid talletatavad andmed.</p></li><li><p>Installima ja konfigureerima <a href="https://wordpress.org/plugins/windows-azure-storage/">Azure Storage WordPress lisandmooduli</a>.</p><p>Üksikasjalik häälestamise ja selle lisandmooduli konfiguratsiooniteavet, vt <a href="http://plugins.svn.wordpress.org/windows-azure-storage/trunk/UserGuide.docx">kasutusjuhendist</a>.</p> </li></ol>
**E-posti lubamine** | Luba <a href="https://azure.microsoft.com/en-us/marketplace/partners/sendgrid/sendgrid-azure/">SendGrid</a> Azure poe kaudu. Installige WordPress <a href="http://wordpress.org/plugins/sendgrid-email-delivery-simplified">SendGrid lisandmoodul</a> .
**Kohandatud domeeninime konfigureerimine** | [Kohandatud domeeninime Azure'i rakendust Service konfigureerimine][customdomain]
**Kohandatud domeeninime HTTPS-i lubamine** | [Veebirakenduse teenuses Azure rakenduse HTTPS-i lubamine][httpscustomdomain]
**Laadi saldo või geo – saidile levitamine** | [Marsruutida liikluse Azure'i liikluse haldur][trafficmanager]. Kui kasutate kohandatud domeeni, lugege teemat [konfigureerimine kohandatud domeeninime teenuses Azure rakenduse] [ customdomain] liikluse haldur kohandatud domeeninimede kasutamise kohta teavet
**Automaatse varundamise funktsiooni lubamine** | [Azure'i rakendust Service veebirakenduse varundamine][backup]
**Diagnostikalogimise lubamiseks** | [Veebirakenduste teenuses Azure rakenduse diagnostika logimise lubamine][log]

## <a name="next-steps"></a>Järgmised sammud

* [WordPress optimeerimine](http://codex.wordpress.org/WordPress_Optimization)

* [WordPress teisendamine mitmes kohas Azure rakenduse teenus](web-sites-php-convert-wordpress-multisite.md)

* [ClearDB täiendamine viisardi Azure](http://www.cleardb.com/store/azure/upgrade)

* [Teenuses Azure rakendus oma veebirakenduse alamkausta majutusteenuse WordPress](http://blogs.msdn.com/b/webapps/archive/2013/02/13/hosting-wordpress-in-a-subfolder-of-your-windows-azure-web-site.aspx)

* [Juhised: Azure'i abil WordPress saidi loomine](http://blogs.technet.com/b/blainbar/archive/2013/08/07/article-create-a-wordpress-site-using-windows-azure-read-on.aspx)

* [Olemasoleva WordPress ajaveebi Azure hosti](http://blogs.msdn.com/b/msgulfcommunity/archive/2013/08/26/migrating-a-self-hosted-wordpress-blog-to-windows-azure.aspx)

* [WordPress päris permalinks lubamine](http://www.iis.net/learn/extensions/url-rewrite-module/enabling-pretty-permalinks-in-wordpress)

* [Kuidas migreerida ja ajaveebi WordPress teenuses Azure rakenduse käivitamine](http://www.kefalidis.me/2012/06/how-to-migrate-and-run-your-wordpress-blog-on-windows-azure-websites/)

* [Kuidas Käivita WordPress Azure'i rakendust Service tasuta](http://architects.dzone.com/articles/how-run-wordpress-azure)

* [WordPress Azure kaks minutit või vähem](http://www.sitepoint.com/wordpress-windows-azure-2-minutes-less/)

* [Teisaldada WordPress ajaveebi Azure - 1 osa: Azure'i WordPress ajaveebi loomine](http://www.davebost.com/2013/07/10/moving-a-wordpress-blog-to-windows-azure-part-1)

* [Teisaldada WordPress ajaveebi Azure - osa 2: teie sisu edastamine](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-transferring-your-content)

* [Teisaldada WordPress ajaveebi Azure - osa 3: kohandatud domeeni häälestamiseks](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-part-3-setting-up-your-custom-domain)

* [Teisaldada WordPress ajaveebi Azure - osa 4: päris permalinks ja URL-i ümberkirjutamine reeglid](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-part-4-pretty-permalinks-and-url-rewrite-rules)

* [Teisaldada WordPress ajaveebi Azure - osa 5: üleminek rakenduselt alamkausta juurkausta](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-part-5-moving-from-a-subfolder-to-the-root)

* [Kuidas WordPress web appi Azure'i konto häälestamine](http://www.itexperience.net/2014/01/20/how-to-set-up-a-wordpress-website-in-your-windows-azure-account/)

* [Propping kuni Azure WordPress](http://www.johnpapa.net/wordpress-on-azure/)

* [Näpunäiteid WordPress Azure](http://www.johnpapa.net/azurecleardbmysql/)

>[AZURE.NOTE] Kui soovite alustada Azure'i rakendust Service enne Azure'i konto kasutajaks, minge [Proovige rakenduse teenus](http://go.microsoft.com/fwlink/?LinkId=523751), kus saate kohe luua lühiajaline starter web app rakenduse teenus. Nõutav; krediitkaardid kohustusi.

## <a name="whats-changed"></a>Mis on muutunud
* Muuda juhend veebisaitide rakenduse teenusega leiate: [Azure'i rakendust Service ja selle mõju olemasoleva Azure'i teenused](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- URL List -->

[performance-diagram]: ./media/web-sites-php-enterprise-wordpress/performance-diagram.png
[basic-diagram]: ./media/web-sites-php-enterprise-wordpress/basic-diagram.png
[multi-region-diagram]: ./media/web-sites-php-enterprise-wordpress/multi-region-diagram.png
[wordpress]: http://www.microsoft.com/web/wordpress
[officeblog]: http://blogs.office.com/
[bingblog]: http://blogs.bing.com/
[cdbnstore]: http://www.cleardb.com/store/azure
[storageplugin]: https://wordpress.org/plugins/windows-azure-storage/
[sendgridplugin]: http://wordpress.org/plugins/sendgrid-email-delivery-simplified/
[phpwebsite]: web-sites-php-configure.md
[customdomain]: web-sites-custom-domain-name.md
[trafficmanager]: ../traffic-manager/traffic-manager-overview.md
[backup]: web-sites-backup.md
[restore]: web-sites-restore.md
[rediscache]: https://azure.microsoft.com/documentation/services/redis-cache/
[managedcache]: http://msdn.microsoft.com/library/azure/dn386122.aspx
[websitescale]: web-sites-scale.md
[managedcachescale]: http://msdn.microsoft.com/library/azure/dn386113.aspx
[cleardbscale]: http://www.cleardb.com/developers/cdbr/introduction
[staging]: web-sites-staged-publishing.md
[monitor]: web-sites-monitor.md
[log]: web-sites-enable-diagnostic-log.md
[httpscustomdomain]: web-sites-configure-ssl-certificate.md
[mysqlwindows]: ../virtual-machines/virtual-machines-windows-classic-mysql-2008r2.md
[mysqllinux]: ../virtual-machines/virtual-machines-linux-classic-mysql-on-opensuse.md
[cge]: http://www.mysql.com/products/cluster/
[websitepricing]: /pricing/details/app-service/
[export]: http://en.support.wordpress.com/export/
[import]: http://wordpress.org/plugins/wordpress-importer/
[wordpressbackup]: http://wordpress.org/plugins/wordpress-importer/
[wordpressdbbackup]: http://codex.wordpress.org/Backing_Up_Your_Database
[createwordpress]: web-sites-php-web-site-gallery.md
[velvet]: https://wordpress.org/plugins/velvet-blues-update-urls/
[mgmtportal]: https://portal.azure.com/
[wordpressbackup]: http://codex.wordpress.org/WordPress_Backups
[wordpressdbbackup]: http://codex.wordpress.org/Backing_Up_Your_Database
[workbench]: http://www.mysql.com/products/workbench/
[searchandreplace]: http://interconnectit.com/124/search-and-replace-for-wordpress-databases/
[deploy]: web-sites-deploy.md
[posh]: ../powershell-install-configure.md
[Azure CLI]: ../xplat-cli-install.md
[storesendgrid]: https://azure.microsoft.com/marketplace/partners/sendgrid/sendgrid-azure/
[cdn]: ../cdn/cdn-overview.md
