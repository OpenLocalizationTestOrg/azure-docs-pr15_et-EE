<properties 
    pageTitle="WordPress teisendamine mitmes kohas Azure rakenduse teenus" 
    description="Siit saate teada, kuidas võtta olemasoleva WordPress web appi kaudu Azure'i galeriis loodud ja teisendage see WordPress mitmes kohas" 
    services="app-service\web" 
    documentationCenter="php" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="PHP" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>



# <a name="convert-wordpress-to-multisite-in-azure-app-service"></a>WordPress teisendamine mitmes kohas Azure rakenduse teenus

## <a name="overview"></a>Ülevaade

*Järgi [Ben Lobaugh][ben-lobaugh], [Microsoft Ava tehnoloogiad Inc kaubamärk.][ms-open-tech]*

Selles õppetükis saate teada, kuidas võtta olemasoleva WordPress web appi kaudu Azure'i galeriis loodud ja muuta selle WordPress mitmes kohas installi. Lisaks saate teada, kuidas määrata kohandatud domeeni iga alamsaitide sees oma installi.

Eeldatakse, et teil on praeguse installi WordPress. Kui te ei saa, järgige juhised loomine [WordPress veebisaidi Azure galeriist][website-from-gallery].

Teisendamine mõne olemasoleva WordPress saidi installi, et mitmes kohas on tavaliselt üsna lihtne ja esimesed sammud siin pärit otse [Loomine A võrgu] [ wordpress-codex-create-a-network] lehe [WordPress Codex](http://codex.wordpress.org).

Alustagem.

## <a name="allow-multisite"></a>Luba mitmes kohas

Peate esmalt lubama mitmes kohas kaudu soovitud `wp-config.php` faili selle **WP\_luba\_mitmes kohas** pidev. On kaks võimalust web appi failide redigeerimiseks: esimene on läbi FTP ja teine Git kaudu. Kui olete varem selliseid häälestamine järgmised võimalused, palun vaadake järgmist õpetused:

* [Veebisaidi PHP MySQL-i ja FTP][website-w-mysql-and-ftp-ftp-setup]

* [Veebisaidi PHP MySQL-i ja Git][website-w-mysql-and-git-git-setup]

Avatud on `wp-config.php` faili toimetaja enda valitud Meilikausta ja lisada ülaltoodud järgmist funktsiooni `/* That's all, stop editing! Happy blogging. */` joon.

    /* Multisite */

    define( 'WP_ALLOW_MULTISITE', true );

Kindlasti salvestage fail ja laadige see server!

## <a name="network-setup"></a>Võrgu häälestamine

Logige veebi alale *wp-admin* rakendus ja te peaksite nägema uue üksuse nimega **Seadistamine**menüü **Tööriistad** . Klõpsake **Seadistamine** ja sisestage oma võrgu üksikasjad.

![Võrgu häälestamine aknas][wordpress-network-setup]

Selle õpetuse kasutab *sub kataloogide* saidi skeemi, kuna see peaks alati töötama ja me seadistada kohandatud domeenide iga alamsaidi hiljem sisse õpetuse. Siiski peaks olema võimalik häälestamise alamdomeeni installida, kui vastendate kaudu [Azure portaali](https://portal.azure.com) ja häälestamise metamärkide DNS-i domeeni õigesti.

Alamdomeeni vs kohta lisateabe saamiseks vt sub-directory seadistuse [mitmes kohas võrgu tüüpi] [ wordpress-codex-types-of-networks] artiklis WordPress Codex.

## <a name="enable-the-network"></a>Võrgu

Võrgu on nüüd konfigureeritud andmebaasi, kuid on ülejäänud sammhaaval võrgu funktsiooni lubamiseks. Viimistlemine soovitud `wp-config.php` sätted ja veenduge, et `web.config` õigesti marsruudib iga ala.


Pärast nupu **installida** lehe *Võrgu häälestamine* , proovib WordPress värskendamiseks on `wp-config.php` ja `web.config` failid. Siiski peaksite alati failide värskendusi ei õnnestunud tagamiseks. Kui ei, Kuva esitada teile vajalikud värskendused. Redigeerida ja salvestada faile.


Pärast tegemist värskendused, peate logige ja logige tagasi wp-administraatori armatuurlauale.

Nüüd peaks olema täiendavaid menüü admin riba siltidega **Isiklikke saite**. See menüü võimaldab teil määrata uue võrgu kaudu **Võrgu administraatori** armatuurlauale.

## <a name="adding-custom-domains"></a>Kohandatud domeenide lisamine

[WordPress MU domeeni vastendamise] [ wordpress-plugin-wordpress-mu-domain-mapping] on Tuul lisada kohandatud domeenide mis tahes saidile teie võrku. Et töötamist lisandmoodul, peate tegema mõned täiendavad setup portaali ja ka oma domeeniregistraatori juures.

## <a name="enable-domain-mapping-to-the-web-app"></a>Domeeni vastenduse web appi lubamine

**Tasuta** [Rakenduse teenuse](http://go.microsoft.com/fwlink/?LinkId=529714) leping režiimi ei toeta kohandatud domeenide lisamine Web Apps. Peate **ühiskasutuses** või **Standard** lülitumine. Soovitud toiming

* Azure portaali sisse logida ja otsige üles oma veebirakenduse. 
* Klõpsake menüü **skaala kuni** **sätted**.
* Valige jaotises **üldist**nuppu *ÜHISKASUTUSES* või *Standardne*
* Klõpsake nuppu **Salvesta**

Võidakse kuvada teade, mis on palub teil kinnitada muutmine ja kinnitage oma veebirakenduse tekkida nüüd kulu, olenevalt sellest, kas kasutus- ja konfiguratsiooni, saate määrata.

Kulub mitu sekundit protsessi uued sätted, nii et nüüd on hea aeg domeeni häälestamise alustamiseks.

## <a name="verify-your-domain"></a>Domeeni kinnitamine

Enne Azure'i veebirakenduste võimaldab teil saidi domeeni vastendada, peate esmalt veenduge, et teil on luba domeeni vastendamiseks. Selle tegemiseks peate lisama oma DNS-i kirje uus CNAME-kirje.

* Logige sisse oma domeeni DNS-i haldur
* Looge uus CNAME *awverify*
* Osutage *awverify* *awverify. YOUR_DOMAIN.azurewebsites.net*

See võib võtta aega DNS-i muudatuste täielikuks minna, nii et kui järgmised toimingud ei tööta kohe, avage kohvi, siis naaske ja proovige uuesti.

## <a name="add-the-domain-to-the-web-app"></a>Veebirakenduse domeeni lisamine

Naaske oma veebirakenduse Azure portaali kaudu, klõpsake nuppu **sätted**ja seejärel klõpsake nuppu **kohandatud domeenid ja SSL-i**.

Kui *SSL-i sätted* on kuvatud, kuvatakse väljad, kuhu sisestate domeenid, mida soovite oma veebirakenduse määramine. Kui domeeni siin loetletud, ei oleks saadaval vastenduse sees WordPress, olenemata sellest, kuidas DNS-i domeen on häälestatud.

![Dialoogiboksi kohandatud domeenide haldamine][wordpress-manage-domains]

Pärast domeeni tekstiväljale, kontrollib Azure'i varem loodud CNAME-kirje. Kui DNS-i on täielikult levitatud, näitab punane indikaator. Kui see õnnestus, kuvatakse roheline märge. 

Pange tähele loetletud dialoogiboksi allservas IP-aadress. Peate selle domeeni A-kirje häälestamine.

## <a name="setup-the-domain-a-record"></a>Kirje domeeni häälestamine

Kui muud juhised edukalt, võib nüüd määrata domeeni oma Azure web appi kaudu A DNS-i kirje. 

On oluline märkida Azure veebirakenduste vastu võtta CNAME- ja kirjete, aga teil *tuleb* kasutada lubamiseks õige Domeen vastenduse A-kirje. Ka CNAME ei saa edastatakse teise CNAME-i, mis on, mis Azure'i loodud teile YOUR_DOMAIN.azurewebsites.net.

Eelmises juhises IP-aadressiga, naaske oma DNS-i halduri ja A-kirje, et IP osutamiseks häälestamine.


## <a name="install-and-setup-the-plugin"></a>Installimine ja häälestus on lisandmoodul

Praegu ei ole WordPress mitmes kohas sisseehitatud kohandatud domeenide meetod. Siiski on lisandmoodul, mis on nimega [WordPress MU domeeni vastendamise] [ wordpress-plugin-wordpress-mu-domain-mapping] funktsioonid lisatakse teie eest. Võrgu administraator osa saidile sisse logida ja installida **WordPress MU domeeni vastendamise** lisandmoodul.

Pärast installimise ja aktiveerimise lisandmoodul, külastage **sätted** > **Domeeni vastendamise** konfigureerida selle lisandmooduli. Esimest tekstivälja, sisestada *Serveri IP-aadress*, domeeni A-kirje häälestamine kasutasite IP-aadress. Seadke muud *Domeeni suvandid* soovite (vaikeväärtused on sageli peene) ja klõpsake nuppu **Salvesta**.

## <a name="map-the-domain"></a>Kaardi domeeni

Külastage **armatuurlaua** soovite vastendada domeeni saidi jaoks. Klõpsake käsku **Tööriistad** > **Domeeni vastendamise** ja tippige uus domeen tekstiväli ja klõpsake nuppu **Lisa**.

Vaikimisi kuvatakse uus domeen ümber tagastanud saidi domeeni. Kui soovite kogu-liikluse suunamiseks uus domeen on, märkige ruut *selle ajaveebi jaoks põhidomeen* enne salvestamist. Saate lisada piiramatu arvu domeenide saidile, kuid ainult ühe võib olla esmane.

## <a name="do-it-again"></a>Tehke seda uuesti

Azure'i veebirakenduste võimaldavad lisada piiramatu arvu domeenide web appi. Mõne muu domeeni lisamiseks peate käivitada iga domeeni **domeeni kinnitamine** ja **kirje domeeni häälestamise** jaotised.  

>[AZURE.NOTE] Kui soovite alustada Azure'i rakendust Service enne Azure'i konto kasutajaks, minge [Proovige rakenduse teenus](http://go.microsoft.com/fwlink/?LinkId=523751), kus saate kohe luua lühiajaline starter web app rakenduse teenus. Nõutav; krediitkaardid kohustusi.

## <a name="whats-changed"></a>Mis on muutunud
* Muuda juhend veebisaitide rakenduse teenusega leiate: [Azure'i rakendust Service ja selle mõju olemasoleva Azure'i teenused](http://go.microsoft.com/fwlink/?LinkId=529714)

[ben-lobaugh]: http://ben.lobaugh.net
[ms-open-tech]: http://msopentech.com
[website-from-gallery]: https://www.windowsazure.com/develop/php/tutorials/website-from-gallery/
[wordpress-codex-create-a-network]: http://codex.wordpress.org/Create_A_Network
[website-w-mysql-and-ftp-ftp-setup]: https://www.windowsazure.com/develop/php/tutorials/website-w-mysql-and-ftp/#header-0
[website-w-mysql-and-git-git-setup]: https://www.windowsazure.com/develop/php/tutorials/website-w-mysql-and-git/#header-1
[wordpress-network-setup]: ./media/web-sites-php-convert-wordpress-multisite/wordpress-network-setup.png
[wordpress-codex-types-of-networks]: http://codex.wordpress.org/Before_You_Create_A_Network#Types_of_multisite_network
[wordpress-plugin-wordpress-mu-domain-mapping]: http://wordpress.org/extend/plugins/wordpress-mu-domain-mapping/

[wordpress-manage-domains]: ./media/web-sites-php-convert-wordpress-multisite/wordpress-manage-domains.png

 
