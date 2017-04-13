<properties
  pageTitle="Kasutage DevOps keskkonnas tõhusalt veebirakenduse jaoks"
  description="Saate teada, kuidas häälestada ja hallata mitut arengu keskkonnas rakenduse juurutamine abil"
  services="app-service\web"
  documentationCenter=""
  authors="sunbuild"
  manager="yochayk"
  editor=""/>

<tags
  ms.service="app-service"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="web"
  ms.date="10/24/2016"
  ms.author="sumuth"/>

# <a name="use-devops-environments-effectively-for-your-web-apps"></a>DevOps keskkonnas tõhusaks kasutamiseks oma veebirakendustes

Selles artiklis kirjeldatakse saate häälestada ja hallata web rakenduse kasutuselevõttu mitme versiooni, nt arengu, lavastus kV ja tootmise. Iga rakenduse versiooni saab lugeda arenduskeskkond teatud vaja oma juurutamise käigus. Näiteks saab teie meeskond arendajad rakenduse kvaliteedi testida enne vajutada muudatused tootmise kv keskkonnas.
Mitme arengu keskkonnas häälestamise võib olla keeruline ülesanne, kui soovite jälitada, hallata ressursse (Arvuta, veebirakenduse, andmebaasi, vahemälu jne) ja Juurutage kood keskkonnas.

## <a name="setting-up-a-non-production-environment-stagedevqa"></a>Kuidas häälestada mitte tekitamiseks keskkonnas (etapi, arendaja, KV)
Kui olete tootmise web appi ja töötab, on järgmiseks mitte tekitamiseks keskkonna loomiseks. Selleks, et kasutada juurutamine veenduge, et kasutate **standardseid** või **rakenduse Premiumi** leping režiimis. Juurutamine on tegelikult reaalajas veebirakenduste abil oma hostinimed. Web Appi sisu ja konfiguratsioon elementide saate vahetasin juurutamise pesast, sh tootmise pesa vahel. Rakenduse juurutamine pesa juurutamine on järgmised eelised:

1. Saate kinnitada web appi muudatuste juurutamise vahekausta pesa enne selle tootmise pesa vahetamine.
2. Teenuse juurutamisel web appi pesa esmalt ja vahetamine selle tootmisse tagab, et kõik eksemplarid pesa soojendada enne on vahetasin tootmisse. See kaob tööseisakute kui juurutate oma veebirakenduse. Liikluse ümbersuunamine on sujuvalt ja pole taotlused kõrvaldatakse Vaheta tegevuse tõttu. Kogu töövoo automatiseerida konfigureerimisega [Automaatne Vaheta](web-sites-staged-publishing.md#configure-auto-swap-for-your-web-app) kui eelse Vaheta valideerimine pole vaja.
3. Pärast soovitud Vaheta pesa varem etapiviisilise web app on nüüd eelmise tootmise web appi. Kui vahetasin tootmise pesa muudatused on pole nii, nagu soovite, saate teha sama vaheta kohe, et saada oma "Viimane tuntud hea web app" tagasi.

Juurutamise vahekausta pesa, vt teemast [lavastus keskkonnas web Appsi teenuses Azure rakenduse häälestamine](web-sites-staged-publishing.md). Iga keskkond, mis peaks sisaldama oma komplekti ressursid, näiteks kui teie web app kasutab andmebaasi tootmise nii lavastus web appi tuleks kasutada erinevaid andmebaase ja seejärel. Lisage lavastus keskkonna ressursse nagu andmebaasi, salvestusruumi või vahemälu kohta, kuidas te oma lavastus arenduskeskkond.

## <a name="examples-of-using-multiple-development-environments"></a>Mitme arengu keskkonnas kasutamise näited

Mis tahes projekti tuleks järgige source code juhtimise vähemalt kahes keskkonnas, arendus- ja keskkonnas, kuid kui sisuhaldus abil rakenduse raamistiku jne me võivad tekkida probleemid, kui rakendus ei toeta seda stsenaariumi välja kasti. See kehtib teatud populaarsed raamistiku allpool. Palju küsimusi tulevad meelde CMS/raamistiku näiteks töötamisel

1. Kuidas see välja murda viibite
2. Milliseid faile saab muuta ja tavaliselt mõjutada framework versiooni uuendused
3. Kuidas hallata kohta keskkonna konfiguratsioon
4. Kuidas hallata moodulid/lisandmoodulid versiooni uuendamiseks core raames versiooni uuendused

On palju võimalusi, kuidas häälestada oma projekti mitme keskkonnas ja alltoodud näiteid on üks ainult ühe sellise meetodi vastava rakendusega.

### <a name="wordpress"></a>WordPress
Selles jaotises saate teada, kuidas häälestada juurutamise töövoo teenindusaegade kasutamise WordPress. WordPress nagu enamik CMS lahendused ei toeta mitut arengu keskkonnas välja kasti töötamise. Rakenduse teenuse Web Apps on mõned funktsioonid, mis hõlbustavad talletada väljaspool teie kood.

Enne vahekausta pesa loomist häälestada rakenduse koodis toetab mitut keskkonnas. Mitme keskkonnas toetamiseks WordPress peate redigeerima `wp-config.php` teie kohaliku arengu web Appis lisada järgmine kood faili alguses. See võimaldab valida õige konfiguratsiooni põhjal valitud keskkonnas rakenduse.

```
// Support multiple environments
// set the config file based on current environment
if (strpos($_SERVER['HTTP_HOST'],'localhost') !== false) {
// local development
 $config_file = 'config/wp-config.local.php';
}
elseif ((strpos(getenv('WP_ENV'),'stage') !== false) || (strpos(getenv('WP_ENV'),'prod' )!== false ))
//single file for all azure development environments
 $config_file = 'config/wp-config.azure.php';
}
$path = dirname(__FILE__). '/';
if (file_exists($path. $config_file)) {
// include the config file if it exists, otherwise WP is going to fail
require_once $path. $config_file;
```

Looge kaust jaotises web appi juurkausta nimetatakse `config` ja lisage faili kahe failid: `wp-config.azure.php` ja `wp-config.local.php` tähistav azure- ja kohaliku keskkonna vastavalt.

Kopeerige järgmise `wp-config.local.php` :

```
<?php
// MySQL settings
/** The name of the database for WordPress */

define('DB_NAME', 'yourdatabasename');

/** MySQL database username */
define('DB_USER', 'yourdbuser');

/** MySQL database password */
define('DB_PASSWORD', 'yourpassword');

/** MySQL hostname */
define('DB_HOST', 'localhost');
/**
 * For developers: WordPress debugging mode.
 * * Change this to true to enable the display of notices during development.
 * It is strongly recommended that plugin and theme developers use WP_DEBUG
 * in their development environments.
 */
define('WP_DEBUG', true);

//Security key settings
define('AUTH_KEY', 'put your unique phrase here');
define('SECURE_AUTH_KEY','put your unique phrase here');
define('LOGGED_IN_KEY','put your unique phrase here');
define('NONCE_KEY', 'put your unique phrase here');
define('AUTH_SALT', 'put your unique phrase here');
define('SECURE_AUTH_SALT', 'put your unique phrase here');
define('LOGGED_IN_SALT', 'put your unique phrase here');
define('NONCE_SALT', 'put your unique phrase here');

/**
 * WordPress Database Table prefix.
 *
 * You can have multiple installations in one database if you give each a unique
 * prefix. Only numbers, letters, and underscores please!
 */
$table_prefix = 'wp_';
```

Turvalisus klahvid ülaltoodud säte aitab takistada oma veebirakenduse ründamise, seega kasutage kordumatuid väärtusi. Kui teil on vaja luua turvalisus võtmed ülalnimetatud stringi, saate minna automaatse generaator luua uue klahvid/väärtused, kasutades seda [link] (https://api.wordpress.org/secret-key/1.1/salt)

Kopeerige järgmine kood `wp-config.azure.php`:


``` <?php
    // MySQL settings
    /** The name of the database for WordPress */
    
    define('DB_NAME', getenv('DB_NAME'));
    
    /** MySQL database username */
    define('DB_USER', getenv('DB_USER'));
    
    /** MySQL database password */
    define('DB_PASSWORD', getenv('DB_PASSWORD'));
    
    /** MySQL hostname */
    define('DB_HOST', getenv('DB_HOST'));
    
    /**
    * For developers: WordPress debugging mode.
    *
    * Change this to true to enable the display of notices during development.
    * It is strongly recommended that plugin and theme developers use WP_DEBUG
    * in their development environments.
    * Turn on debug logging to investigate issues without displaying to end user. For WP_DEBUG_LOG to
    * do anything, WP_DEBUG must be enabled (true). WP_DEBUG_DISPLAY should be used in conjunction
    * with WP_DEBUG_LOG so that errors are not displayed on the page */
    
    */
    define('WP_DEBUG', getenv('WP_DEBUG'));
    define('WP_DEBUG_LOG', getenv('TURN_ON_DEBUG_LOG'));
    define('WP_DEBUG_DISPLAY',false);
    
    //Security key settings
    /** If you need to generate the string for security keys mentioned above, you can go the automatic generator to create new keys/values: https://api.wordpress.org/secret-key/1.1/salt **/
    define('AUTH_KEY',getenv('DB_AUTH_KEY'));
    define('SECURE_AUTH_KEY', getenv('DB_SECURE_AUTH_KEY'));
    define('LOGGED_IN_KEY', getenv('DB_LOGGED_IN_KEY'));
    define('NONCE_KEY', getenv('DB_NONCE_KEY'));
    define('AUTH_SALT', getenv('DB_AUTH_SALT'));
    define('SECURE_AUTH_SALT', getenv('DB_SECURE_AUTH_SALT'));
    define('LOGGED_IN_SALT',  getenv('DB_LOGGED_IN_SALT'));
    define('NONCE_SALT',  getenv('DB_NONCE_SALT'));
    
    /**
    * WordPress Database Table prefix.
    *
    * You can have multiple installations in one database if you give each a unique
    * prefix. Only numbers, letters, and underscores please!
    */
    $table_prefix = getenv('DB_PREFIX');
```

#### <a name="use-relative-paths"></a>Suhtelised teed kasutamine
Üks asi on konfigureerida WordPress rakenduse kasutamiseks suhtelised teed. WordPress talletatakse andmebaasi URL-i teave. See muudab teisaldamine ühest keskkonnast sisu teise raske, kui on vaja värskendada iga kord, kui teisaldate kohaliku etapp või etapi tootmise keskkondades andmebaasi. Kui soovite vähendada probleemid, mis võivad põhjustada juurutamisega andmebaasi iga kord, kui juurutate ühest keskkonnast teise kasutamine [suhteline Root lingid lisandmoodul](https://wordpress.org/plugins/root-relative-urls/) , mis saab installida WordPress administraatori armatuurlauale või alla laadida [siin](https://downloads.wordpress.org/plugin/root-relative-urls.zip)käsitsi.


Lisage järgmised kirjed, et teie `wp-config.php` faili enne selle `That's all, stop editing!` kommentaari:

```

  define('WP_HOME', 'http://'. filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
    define('WP_SITEURL', 'http://'. filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
    define('WP_CONTENT_URL', '/wp-content');
    define('DOMAIN_CURRENT_SITE', filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
```

Aktiveerimine lisandmooduli kaudu on `Plugins` menüü WordPress administraatori armatuurlauale. WordPress app permalink sätete salvestamine.

#### <a name="the-final-wp-configphp-file"></a>Lõplik `wp-config.php` fail
WordPress Core värskendusi ei mõjuta teie `wp-config.php`, `wp-config.azure.php` ja `wp-config.local.php` failid. Lõpuks see kuidas `wp-config.php` faili näeb välja umbes järgmine

```
<?php
/**
 * The base configurations of the WordPress.
 *
 * This file has the following configurations: MySQL settings, Table Prefix,
 * Secret Keys, and ABSPATH. You can find more information by visiting
 *
 * Codex page. You can get the MySQL settings from your web host.
 *
 * This file is used by the wp-config.php creation script during the
 * installation. You don't have to use the web web app, you can just copy this file
 * to "wp-config.php" and fill in the values.
 *
 * @package WordPress
 */

// Support multiple environments
// set the config file based on current environment
if (strpos($_SERVER['HTTP_HOST'],'localhost') !== false) { // local development
  $config_file = 'config/wp-config.local.php';
}
elseif ((strpos(getenv('WP_ENV'),'stage') !== false) ||(strpos(getenv('WP_ENV'),'prod' )!== false )){
  $config_file = 'config/wp-config.azure.php';
}


$path = dirname(__FILE__). '/';
if (file_exists($path. $config_file)) {
  // include the config file if it exists, otherwise WP is going to fail
  require_once $path. $config_file;
}

/** Database Charset to use in creating database tables. */
define('DB_CHARSET', 'utf8');

/** The Database Collate type. Don't change this if in doubt. */
define('DB_COLLATE', '');


/* That's all, stop editing! Happy blogging. */

define('WP_HOME', 'http://'. $_SERVER['HTTP_HOST']);
define('WP_SITEURL', 'http://'. $_SERVER['HTTP_HOST']);
define('WP_CONTENT_URL', '/wp-content');
define('DOMAIN_CURRENT_SITE', $_SERVER['HTTP_HOST']);

/** Absolute path to the WordPress directory. */
if ( !defined('ABSPATH') )
    define('ABSPATH', dirname(__FILE__). '/');

/** Sets up WordPress vars and included files. */
require_once(ABSPATH. 'wp-settings.php');
```

#### <a name="set-up-a-staging-environment"></a>Lavastus keskkonna häälestamine
Eeldades, et teil on juba WordPress web appi töötavate Azure Web [Azure'i halduse eelvaade portaali](http://portal.azure.com) sisse logida ja minge oma WordPress veebirakenduse. Rakenduste If ei saate luua ühe turuplatsilt. Lisateavet, [klõpsake siin](web-sites-php-web-site-gallery.md).
Klõpsake sätete -> juurutamise slots -> Lisa loomiseks juurutamine pesa etapi nime. Juurutamise pesa on teise veebirakenduse ühiskasutuse loodud kohal esmane veebirakenduse samadele ressurssidele.

![Etapi juurutamine pesa loomine](./media/app-service-web-staged-publishing-realworld-scenarios/1setupstage.png)

Lisage teine MySQL-andmebaasiga, Oletagem, et `wordpress-stage-db` oma ressursirühma `wordpressapp-group`.

 ![Ressursirühm MySQL-andmebaasiga lisamine](./media/app-service-web-staged-publishing-realworld-scenarios/2addmysql.png)

Ühendusstringi oma etapi juurutamise pesa äsja loodud andmebaasi osutamiseks värskendada `wordpress-stage-db`. Teate, et teie tootmise web appi `wordpressprodapp` ja lavastus web appi `wordpressprodapp-stage` peavad osutama erinevate andmebaasid.

#### <a name="configure-environment-specific-app-settings"></a>Keskkonna konkreetse rakenduse sätete konfigureerimine
Arendajad saate talletada võti väärtus stringi paari Azure seotud web appi, nimetatakse rakenduse sätete konfiguratsiooniteavet osana. Käitusajal rakenduse teenuse veebirakenduste automaatselt toob saate need väärtused ja muudab need koodi, mis töötab teie web Appis saadaval. Väärtpaberi perspektiivi, mis on kena pool kasu alates tundlikku teavet nagu andmebaasi ühendusstringi paroole kunagi kuvataks avatekstina faili nagu `wp-config.php`.

Seda toimingut, mis on määratletud on kasulik, kui sisaldab nii faili muudatused ja WordPress rakenduse andmebaasi muudatusi teha.
- WordPress versiooni täiendamine
- Või täiendamine lisandmoodulid uus lisamine või redigeerimine
- Või täiendamine kujundused uus lisamine või redigeerimine

Rakenduse sätete konfigureerimine:

- andmebaasi teave
- sisse- / väljalülitamine WordPress logimine
- WordPress turbesätted

![Rakenduse sätete Wordpress web app](./media/app-service-web-staged-publishing-realworld-scenarios/3configure.png)

Veenduge, et olete lisanud oma tootmise web app ja etapi pesa rakenduse järgmised sätted. Arvestage, et tootmise web app ja lavastus veebirakenduse eri andmebaasid.
Tühjendage kõik sätted parameetrid peale WP_ENV **Pesa säte** ruut. Omavahel vahetada oma veebirakenduse koos faili sisu ja andmebaasi konfigureerimine. Kui **Pesa säte** on **märgitud**, web app rakenduse sätete ja ühenduse stringi konfiguratsiooni ei liigu üle keskkonnas tehes Vaheta toiming ja seega andmebaasi muudatusi, kui see pole rikub teie tootmise web Appis.

Kohaliku arengu keskkonnas web appi juurutamiseks etapi web app ja andmebaasi WebMatrix või teie valitud, nt FTP, Git või PhpMyAdmin tööriista(de) abil.

![Web WordPress web appi dialoogiboks maatriksi avaldamine](./media/app-service-web-staged-publishing-realworld-scenarios/4wmpublish.png)

Liikuge ja testige oma lavastus veebirakenduse. Stsenaarium, kus veebirakenduse teema on värskendamist, arvestades siin on lavastus web app.

![Sirvige lavastus veebirakenduse enne teenindusaegade vahetamine](./media/app-service-web-staged-publishing-realworld-scenarios/5wpstage.png)


 Kui kõik sobib, klõpsake nuppu **Vaheta** teie sisu teisaldamiseks tootmiskeskkonda lavastus veebirakendus. Sel juhul saate vahetada web app ja andmebaasi keskkonnas üle iga **Vaheta** töötamise ajal.

![Vaheta muudatuste eelvaade WordPress](./media/app-service-web-staged-publishing-realworld-scenarios/6swaps1.png)

 > [AZURE.NOTE]
 >Kui teil on stsenaarium, kus teil tuleb ainult push faile (andmebaasi värskendused), siis **märkige ruut** kõik andmebaasi **Pesa säte** seotud *rakendus* ja *stringide ühendusesätete* web appi säte blade Azure eelvaade portaali sisse enne nende. See juhul DB_NAME, DB_HOST, DB_PASSWORD, DB_USER, vaikesäte ühenduse stringi peaks ei kuvata eelvaade muudatused tehes **vahetada**. Sel ajal, kui teil on lõpule jõudnud, on **Vaheta** toimingu WordPress web appi värskenduste failid **ainult**.

Enne mõne Vaheta, siin on veebirakenduse tootmise WordPress ![tootmise web appi enne teenindusaegade vahetamine](./media/app-service-web-staged-publishing-realworld-scenarios/7bfswap.png)

Pärast Vaheta toimingut teema on värskendatud teie tootmise web Appis.

![Tootmise veebirakenduse pärast teenindusaegade vahetamine](./media/app-service-web-staged-publishing-realworld-scenarios/8afswap.png)

Juhul kui teil on vaja **tagasi pöörata**, saate minge tootmise web appi sätete ja vahetada web appi ja andmebaasi toodetud vahekausta pesa nuppu **Vaheta** . Pidage meeles, et oluline on, et kui andmebaas on kaasas **Vaheta** toiming, mis tahes ajal, siis järgmine kord, kui juurutate uuesti oma lavastus veebirakenduse, andmebaasi juurutamiseks muutub praeguse andmebaasi, mis võiks olla eelmise tootmise andmebaasi või etapi andmebaasi lavastus veebirakenduse jaoks.

#### <a name="summary"></a>Kokkuvõte
Üldistada protsessi mis tahes rakenduse andmebaas

1. Kohaliku keskkonna rakenduste installimine
2. Keskkonna teatud konfiguratsiooni (kohaliku ja Azure Web App)
3. Teie keskkonnas rakenduse teenuse Web Apps – lavastus, tootmise häälestamine
4. Kui teil on juba töötab Azure tootmise rakendus, sünkroonimine kohaliku ja lavastus keskkonna tootmise sisu (failide/kood + andmebaasi).
5. Töötada rakenduse teie kohaliku keskkonna
6. Paigutage tootmise veebirakenduse lukustatud režiim ja hooldus ja sünkroonimine andmebaasi sisu toodetud lavastus ja arendaja keskkonnas
7. Võtta kasutusele lavastus keskkonna ja Test
8. Võtta kasutusele tootmiskeskkonnas
9. Korrake samme 4 kuni 6

### <a name="umbraco"></a>Umbraco
Selles jaotises saate teada, kuidas Umbraco CMS kasutab kohandatud moodul üle mitme DevOps keskkonnas juurutada. Selles näites annab teile eri lähenemisviisi hallata mitut arengu keskkonnas.

[Umbraco CMS](http://umbraco.com/) on üks popular.NET CMS lahendusi kasutavad paljud arendajad, mis pakub [Courier2](http://umbraco.com/products/more-add-ons/courier-2) mooduli juurutamiseks arengu lavastus tootmise keskkondades. Kohaliku arenduskeskkond Umbraco CMS web appi Visual Studio või WebMatrix abil saate hõlpsasti luua.

1. Looge Umbraco web app Visual Studio, [klõpsake siin](https://our.umbraco.org/documentation/Installation/install-umbraco-with-nuget).
2. Luua Umbraco web app WebMatrix, [klõpsake siin](http://umbraco.com/help-and-support/video-tutorials/getting-started/working-with-webmatrix).

Pidage meeles, et eemaldada soovitud `install` kausta, klõpsake jaotises rakenduse ja kunagi laadige see etapp või tootmise veebirakenduste. Selles õpetuses mõeldud ma kasutama WebMatrix

#### <a name="set-up-a-staging-environment"></a>Lavastus keskkonna häälestamine
- Looge juurutamine pesa nagu eespool kirjeldatud Umbraco CMS web appi, eeldades, et teil on juba Umbraco CMS web app ja töötab. Kui mitte, saate luua ühe turuplatsilt.

- Värskendage oma etapi juurutamise pesa osutamiseks äsja loodud andmebaasi **umbraco-etapi-db**ühendusstring. Oma tootmise web app (umbraositecms-1) ja lavastus web appi (umbracositecms 1-etapi) **peab** osutage eri andmebaaside.

![Värskendage ühendusstringi web Appi abil uue vaheandmebaasi jaoks.](./media/app-service-web-staged-publishing-realworld-scenarios/9umbconnstr.png)

- Klõpsake sätete **Saada avaldamine** juurutamise pesa **esitusala**. Laadige alla avalda sätete fail, mis kõik avaldada Azure web appi kohaliku arengu web Appist rakenduse Visual Studio või Web maatriksi vajaliku teabe.

 ![Saada avaldamine lavastus veebirakenduse seadmine](./media/app-service-web-staged-publishing-realworld-scenarios/10getpsetting.png)

- Avage oma kohaliku arengu veebirakenduse **WebMatrix** või **Visual Studio**. Selles õpetuses ma kasutan Web maatriksi ja tuleb kõigepealt lavastus veebirakenduse jaoks avalda sätted faili importimine

![Impordisätete avalda Umbraco Web maatriksi abil](./media/app-service-web-staged-publishing-realworld-scenarios/11import.png)

- Dialoogiboksis muutuste läbivaatamine ja Juurutage oma Azure veebirakenduse *umbracositecms 1-etapi*teie kohaliku web Appis. Kui juurutate faile otse oma lavastus veebirakenduse jätate kõik failid on `~/app_data/TEMP/` kausta need kuvatakse uuesti, kui etapi web appi esimest alustamine. Tuleks jätta ka selle `~/app_data/umbraco.config` ka seda, kui fail kuvatakse uuesti.

![Muutuste läbivaatamine avalda web maatriksi](./media/app-service-web-staged-publishing-realworld-scenarios/12umbpublish.png)

- Pärast edukalt lavastus veebirakenduse Umbraco kohaliku veebirakenduse avaldamine, liikuge sirvides oma lavastus veebirakenduse ja mõned analüüse, et välistada probleemidest.

#### <a name="set-up-courier2-deployment-module"></a>Courier2 juurutamise mooduli häälestamine
[Courier2](http://umbraco.com/products/more-add-ons/courier-2) mooduli vajutada sisu, laadilehte, arengu moodulid ja Lisateavet lihtsa paremklõpsake lavastus web Appist tootmise web appi veel probleemideta tasuta juurutuste ja vähendada katkestamine oma tootmise veebirakenduse juurutamisel värskendust.
Osta domeeni ka litsentsi Courier2 `*.azurewebsites.net` ja kohandatud domeeni (st http://abc.com) Kui olete ostnud litsentsi, viige allalaaditud litsents (. LIC fail) klõpsake soovitud `bin` kausta.

![Ripploendi litsentsi faili kaustas Prügikast](./media/app-service-web-staged-publishing-realworld-scenarios/13droplic.png)

Courier2 alla laadida [siin](https://our.umbraco.org/projects/umbraco-pro/umbraco-courier-2/). Teie etapi web appi sisse logida, teatavad, et http://umbracocms-site-stage.azurewebsites.net/umbraco ja klõpsake menüü **arendaja** ja valige **paketid**. Klõpsake kohaliku paketi **installimine**

![Umbraco pakett installer](./media/app-service-web-staged-publishing-realworld-scenarios/14umbpkg.png)

Kasutades installer courier2 paketi üleslaadimine

![Courier mooduli paketi üleslaadimine](./media/app-service-web-staged-publishing-realworld-scenarios/15umbloadpkg.png)

Saate konfigureerida värskendama oma veebirakenduse **Config** kaustas courier.config faili.

```xml
<!-- Repository connection settings -->
 <!-- For each site, a custom repository must be configured, so Courier knows how to connect and authenticate-->
 <repositories>
    <!-- If a custom Umbraco Membership provider is used, specify login & password + set the passwordEncoding to clear: -->
    <repository name="production web app" alias="stage" type="CourierWebserviceRepositoryProvider" visible="true">
      <url>http://umbracositecms-1.azurewebsites.net</url>
      <user>0</user>
      <!--<login>user@email.com</login> -->
      <!-- <password>user_password</password>-->
      <!-- <passwordEncoding>Clear</passwordEncoding>-->
      </repository>
 </repositories>
 ```

Klõpsake jaotises `<repositories>`, sisestage selle saidi URL-i ja kasutajale tooteteabe. Kui kasutate Umbraco liikmelisuse vaikepakkuja, lisage Administreerimine kasutaja ID-d <user> jaotis. Kui kasutate kohandatud Umbraco liikmestaatuse pakkuja, kasutage `<login>`,`<password>` Courier2 moodulisse teada, kuidas ühendada tootmiskohast. Täpsema teabe saamiseks lugege läbi Courier mooduli [dokumentatsiooni](http://umbraco.com/help-and-support/customer-area/courier-2-support-and-download/developer-documentation) .

Samamoodi oma tootmiskohast Courier mooduli installimine ja konfigureerimine selle punkti etapi web appi oma vastava courier.config faili siin esitatud kujul

```xml
 <!-- Repository connection settings -->
 <!-- For each site, a custom repository must be configured, so Courier knows how to connect and authenticate-->
 <repositories>
    <!-- If a custom Umbraco Membership provider is used, specify login & password + set the passwordEncoding to clear: -->
    <repository name="Stage web app" alias="stage" type="CourierWebserviceRepositoryProvider" visible="true">
      <url>http://umbracositecms-1-stage.azurewebsites.net</url>
      <user>0</user>
      </repository>
 </repositories>
```

Klõpsake menüü Courier2 Umbraco CMS web appi armatuurlaua ja valige asukohad. Peaksite nägema hoidla nime nimetatud `courier.config`. Tehke seda oma tootmise ja lavastus veebirakenduste.

![Vaate sihthoidlasse web app](./media/app-service-web-staged-publishing-realworld-scenarios/16courierloc.png)

Nüüd saab mõned tootmiskohast lavastus saidilt sisu juurutamine. Valige sisu ja valige olemasoleva lehe või uue lehe loomine. Ma valige olemasoleva lehe minu web Appist, kus lehe tiitel on muutunud **Töö alustamine – uue** ja nüüd klõpsake **Salvesta ja avalda**.

![Muuta lehe tiitel ja avaldamine](./media/app-service-web-staged-publishing-realworld-scenarios/17changepg.png)

Nüüd valige muudetud lehe ja *paremklõpsake* kõigi suvandite kuvamiseks. **Courier** juurutamise dialoogiboksi kuvamiseks klõpsake nuppu. Klõpsake **Deploy** algatada juurutamine

![Courier mooduli juurutamise dialoogiboks](./media/app-service-web-staged-publishing-realworld-scenarios/18dialog1.png)

Muudatused üle vaadata ja klõpsake käsku Jätka.

![Courier mooduli juurutamise dialoogiboksis muutuste läbivaatamine](./media/app-service-web-staged-publishing-realworld-scenarios/19dialog2.png)

Juurutamise log kuvatakse juurutamise õnnestus.

 ![Juurutamise logid Courier moodulist kuvamine](./media/app-service-web-staged-publishing-realworld-scenarios/20successdlg.png)

Liikuge sirvides oma tootmise veebirakenduse kuvamiseks, kui muudatusi, kajastuvad.

 ![Sirvige tootmise web app](./media/app-service-web-staged-publishing-realworld-scenarios/21umbpg.png)

Courier kasutamise kohta lisateabe saamiseks vaadake dokumentatsiooni.

#### <a name="how-to-upgrade-umbraco-cms-version"></a>Kuidas Umbraco CMS versiooni täiendamine

Courier ei aita juurutamine abil üleminekut Umbraco cm ühest versioonist teise. Umbraco CMS versiooni täiendamisel peate kontrollima kohta oma kohandatud moodulid või muude tootjate moodulid ja Umbraco Core teekide. Hea tava

1. ALATI varundada oma veebirakenduse ja andmebaasi enne täiendamist. Azure'i Web Appile, saate häälestada automaatse varundamise funktsiooni abil varukoopia veebisaidiga funktsioonide ja taastamine saidi kui vaja kasutamise taastada funktsiooni. Lisateavet leiate teemast [oma veebirakenduse varundamine](web-sites-backup.md) ja [taastamine oma veebirakenduse](web-sites-restore.md).

2. Märkige ruut, kui kasutate kolmanda osapoole paketid ühilduvad tõstate versioon. Paketi laadida lehe, vaadake üle projekti ühilduvus Umbraco CMS versiooniga.

Lisateavet selle kohta, kuidas uuendada oma veebirakenduse kohalikult, järgige juhiseid nimega mainitud [siin](https://our.umbraco.org/documentation/getting-started/set up/upgrading/general).

Kui versioon on täiendatud saidile kohaliku arengu muudatused avaldada lavastus web appi. Testige rakendust ja kui kõik sobib, kasutage **Vaheta** nuppu **Vaheta** lavastus tootmise web appi saidi. Kui nii toimingut **vahetada** , saate vaadata soovitud muudatused kuvatakse mõjutab oma veebirakenduse konfiguratsiooni. Selle toiminguga **vahetada** , saame vahetamine web Appsi ja andmebaasid. See tähendab, et pärast Vaheta tootmise web app on nüüd käsk umbraco-etapi-db andmebaasi ja lavastus web appi käsk umbraco-tootekataloogi-db andmebaasi.

![Vaheta eelvaade Umbraco CMS juurutamine](./media/app-service-web-staged-publishing-realworld-scenarios/22umbswap.png)

Ära nii web Appi kui ka andmebaasi vahetamine:
1. Annab teile võimalus tagasi pöörata oma veebirakenduse abil **vahetada** mõne muu varasema versiooni, kui on rakenduse probleemidest.
2. Versiooniuuenduse peate juurutada failid ja andmebaasi lavastus veebirakenduse tootmise web appi ja andmebaasi. Paljud asjad, mis võib valesti minna, failide ja andmebaasi juurutamisel on. Teenindusaegade **Vaheta** funktsiooni abil saame vähendada tööseisakute versiooniuuenduse ja vähendada tõrkeid, mis võivad tekkida juurutamine muudatused.
3. Võimaldab teil teha **A ja B testimine** [testimine valmistamisel](https://azure.microsoft.com/documentation/videos/introduction-to-azure-websites-testing-in-production-with-galin-iliev/) funktsiooni abil

Selles näites kirjeldatakse platvormi paindlikkust kus saate luua kohandatud moodulid sarnane Umbraco Courier mooduli haldamiseks juurutamise keskkonnas.

## <a name="references"></a>Viited
[Dünaamilised tarkvara arendamine Azure'i rakenduse teenusega](app-service-agile-software-development.md)

[Lavastus keskkonnas web Appsi teenuses Azure rakenduse häälestamine](web-sites-staged-publishing.md)

[Kuidas blokeerida web juurdepääsu mitte tekitamiseks juurutamine](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)
