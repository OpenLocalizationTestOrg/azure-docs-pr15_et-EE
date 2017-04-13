<properties
    pageTitle="Azure'i rakendust Service web appi Täpsemad config ja laiendid"
    description="Kasutage XML dokumendi Transformation(XDT) deklaratsiooni ApplicationHost.config faili Azure'i rakendust Service web Appis muuta ja lisada privaatne laiendid kohandatud Administreerimine toimingute lubamiseks."
    authors="cephalin"
    writer="cephalin"
    editor="mollybos"
    manager="wpickett"
    services="app-service"
    documentationCenter=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/25/2016"
    ms.author="cephalin"/>

# <a name="azure-app-service-web-app-advanced-config-and-extensions"></a>Azure'i rakendust Service web appi Täpsemad config ja laiendid

[XML-i dokumendi teisendamiseks](http://msdn.microsoft.com/library/dd465326.aspx) (XDT) deklaratsiooni abil saate muuta faili [ApplicationHost.config](http://www.iis.net/learn/get-started/planning-your-iis-architecture/introduction-to-applicationhostconfig) teenuses Azure rakenduse web Appis. XDT deklaratsiooni abil saate lisada privaatne laiendid kohandatud web appi Administreerimine toimingute lubamiseks. See artikkel sisaldab valimi PHP halduri web appi laiend, mis võimaldab hallata PHP sätted web kasutajaliidese kaudu.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

##<a id="transform"></a>Täpsem konfiguratsiooni ApplicationHost.config kaudu
Platvormi rakenduse teenus pakub paindlikkust ja juhtelemendi web appi konfigureerimine. Kuigi standard IIS-i ApplicationHost.config konfiguratsioonifail ei ole saadaval rakenduse teenuse otse redigeerimiseks, toetab platvormi deklaratiivseid ApplicationHost.config transformatsioon mudel, mis põhineb XML-i dokumendi teisendamiseks (XDT).

Selle teisenduse funktsioonide laiemaks, saate luua rakenduse ApplicationHost.xdt fail XDT sisu ja saidi juursait (d:\home\site) [Kudu konsooli](https://github.com/projectkudu/kudu/wiki/Kudu-console)all. Võimalik, et peate veebirakenduse muudatuste jõustumiseks taaskäivitage.

Järgmises näites applicationHost.xdt näitab, kuidas lisada uue kohandatud keskkonna muutuja web appi, mis kasutab PHP 5.4.

    <?xml version="1.0"?>
    <configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
        <system.webServer>
                <fastCgi>
                    <application>
                        <environmentVariables>
                                <environmentVariable name="CONFIGTEST" value="TEST" xdt:Transform="Insert" xdt:Locator="XPath(/configuration/system.webServer/fastCgi/application[contains(@fullPath,'5.4')]/environmentVariables)" />
                        </environmentVariables>
                    </application>
                </fastCgi>
        </system.webServer>
    </configuration>


Logifaili teisenduse oleku ja üksikasjad on saadaval jaotises LogFiles\Transform juurest FTP.

Täiendavad näidised, vt [https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples](https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples).

**Märkus**<br />
Elementide loendist moodulid jaotises `system.webServer` ei saa eemaldada või ümber rühmitada, kuid loendisse lisatud, on võimalik.


##<a id="extend"></a>Oma veebirakenduse laiendamine

###<a id="overview"></a>Privaatne web appi laiendid ülevaade

Teenuse rakendus toetab web appi laiendid laiendatavuse punktina haldustoimingute kohta. Mõned rakenduse teenuse platvormi funktsioonid on tegelikult rakendada eelinstallitud laiendid. Kuigi eelinstallitud platvormi laiendid ei saa muuta, saate luua ja privaatne laiendid konfigureerimine oma veebirakenduse. See funktsioon tugineb ka XDT deklaratsiooni. Võtme juhiseid privaatne web appi laiend loomise kohta on järgmised:

1. Web Appi laiendamine **sisu**: kõik rakenduse teenus ei toeta veebirakenduse loomine
2. Web Appi laiend **deklareerimise**: ApplicationHost.xdt faili loomine
3. Web Appi laiend **juurutamise**: SiteExtensions kaustas sisu paigutamine`root`

Sisemised lingid web appi peaks osutage suhtes rakenduse tee ApplicationHost.xdt failis määratud tee. Mis tahes muuta ApplicationHost.xdt faili jaoks on vaja web appi prügikast.

**Märkus**: Lisateavet võtme elementi on saadaval veebisaidil [https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions](https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions).

Üksikasjalikud näiteks kaasatakse illustreerimiseks loomise ja privaatne web appi pikendamise lubamine juhiseid. Lähtekoodi PHP halduri näiteks järgneva saate alla laadida [https://github.com/projectkudu/PHPManager](https://github.com/projectkudu/PHPManager).

###<a id="SiteSample"></a>Web Appi laiend näide: PHP haldur

PHP Manager on web appi laiend, mis võimaldab web appi administraatorid hõlpsasti vaadata ja konfigureerida oma PHP web kasutajaliidese kasutamise asemel, et muuta PHP ini faile otse. Levinud php failid lisada php.ini fail asub jaotises Program Files ja. user.ini fail asub oma veebirakenduse juurkausta. Php.ini faili ei saa otse redigeerida rakenduse teenuse platvormi ja PHP halduri laiendamine kasutab soovitud. user.ini faili sätete muudatused rakendada.

####<a id="PHPwebapp"></a>Veebirakenduse PHP haldur

PHP halduri juurutamise avalehel on järgmine:

![TransformSitePHPUI][TransformSitePHPUI]

Nagu näete, on web appi laiend samamoodi nagu tavalise veebirakenduse, kuid täiendava ApplicationHost.xdt faili (täpsemat teavet ApplicationHost.xdt fail on saadaval käesoleva artikli järgmises jaotises) veebirakenduse juurkausta paigutada.

PHP halduri laiend on loodud Visual Studio ASP.NET MVC 4 veebirakenduse malli abil. Järgmised vaade Solution Exploreris kuvatakse PHP halduri laiend struktuuri.

![TransformSiteSolEx][TransformSiteSolEx]

Vajalik faili I/O ainult teisiti loogika on näitamaks, kus asub wwwroot kataloogi web app. Nagu koodi järgmises näites on kujutatud, keskkonnas muutuv "kodu" näitab web appi juurkausta tee ja wwwroot tee saate ehitatud mõnel "site\wwwroot".

    /// <summary>
    /// Gives the location of the .user.ini file, even if one doesn't exist yet
    /// </summary>
    private static string GetUserSettingsFilePath()
    {
            var rootPath = Environment.GetEnvironmentVariable("HOME"); // For use on Azure Websites
            if (rootPath == null)
            {
                rootPath = System.IO.Path.GetTempPath(); // For testing purposes
            };
            var userSettingsFile = Path.Combine(rootPath, @"site\wwwroot\.user.ini");
            return userSettingsFile;
    }


Kui teil on selle kausta tee, saate lugeda ja kirjutada faile tavaline faili/v-toimingud.

Ühe punkti hoiatused web appi laiendiga osas käsitsemise kohta sisemised lingid.  Kui teil on absoluutne teed anda sisemise linkide oma veebirakenduse HTML-vormingus failide linke, tuleb tagada, need lingid on täiendatud oma root laiend nimega. See on vajalik, kuna teie pikendamise juurkausta on nüüd "/`[your-extension-name]`/" selle asemel, et peate lihtsalt "/", et igal sisemine lingid vastavalt ajakohastada. Oletagem näiteks, et teie kood sisaldab järgmist linki:

`"<a href="/Home/Settings">PHP Settings</a>"`

Kui link moodustab osa web appi laiend, tuleb lingi järgmisel kujul:

`"<a href="/[your-site-name]/Home/Settings">Settings</a>"`

Saate selle nõude sellega kas suhtelised teed oma veebirakenduse või ASP.net-i puhul, kasutades funktsiooni `@Html.ActionLink` meetod, mis loob teie jaoks asjakohased lingid.

####<a id="XDT"></a>ApplicationHost.xdt fail

Web appi laiendiga kood läheb alla %HOME%\SiteExtensions\[teie laiend nimi]. Me kutsume see laiend juurkausta.  

Registreerimiseks web appi laiendiga faili applicationHost.config, peate faili nimega ApplicationHost.xdt laiend juurkausta paigutada. ApplicationHost.xdt faili sisu peaks olema järgmine:

    <?xml version="1.0"?>
    <configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
        <system.applicationHost>
                <sites>
                    <site name="%XDT_SCMSITENAME%" xdt:Locator="Match(name)">
                        <!-- NOTE: Add your extension name in the application paths below -->
                        <application path="/[your-extension-name]" xdt:Locator="Match(path)" xdt:Transform="Remove" />
                        <application path="/[your-extension-name]" applicationPool="%XDT_APPPOOLNAME%" xdt:Transform="Insert">
                            <virtualDirectory path="/" physicalPath="%XDT_EXTENSIONPATH%" />
                        </application>
                    </site>
                </sites>
        </system.applicationHost>
    </configuration>

Valite oma laiend nimi nimi peaks olema sama nimi nagu teie laiend juurkausta.

See on lisamise uue rakenduse tee on `system.applicationHost` SCM saidi saitide loendist. SCM sait on saidi Administreerimine end punkti juurdepääsetav mandaati. See on URL-i `https://[your-site-name].scm.azurewebsites.net`.  

    <system.applicationHost>
        ...
        <site name="~1[your-website]" id="1716402716">
                <bindings>
                    <binding protocol="http" bindingInformation="*:80: [your-website].scm.azurewebsites.net" />
                    <binding protocol="https" bindingInformation="*:443: [your-website].scm.azurewebsites.net" />
                </bindings>
                <traceFailedRequestsLogging enabled="false" directory="C:\DWASFiles\Sites\[your-website]\VirtualDirectory0\LogFiles" />
                <detailedErrorLogging enabled="false" directory="C:\DWASFiles\Sites\[your-website]\VirtualDirectory0\LogFiles\DetailedErrors" />
                <logFile logSiteId="false" />
                <application path="/" applicationPool="[your-website]">
                    <virtualDirectory path="/" physicalPath="D:\Program Files (x86)\SiteExtensions\Kudu\1.24.20926.5" />
                </application>
                <!-- Note the custom changes that go here -->
                <application path="/[your-extension-name]" applicationPool="[your-website]">
                    <virtualDirectory path="/" physicalPath="C:\DWASFiles\Sites\[your-website]\VirtualDirectory0\SiteExtensions\[your-extension-name]" />
                </application>
            </site>
    </sites>
      ...
    </system.applicationHost>

###<a id="deploy"></a>Web Appi laiend juurutamine

Web appi laiendiga installimiseks saate FTP kõik failid, kui soovite oma veebirakenduse soovitud `\SiteExtensions\[your-extension-name]` kaust, kuhu soovite installida laiendamine veebirakendusse.  Kindlasti ka selle asukoha ApplicationHost.xdt faili kopeerimine. Taaskäivitage oma veebirakenduse pikendamise.

Tuleks vaadata web appi laiendiga veebisaidil:

`https://[your-site-name].scm.azurewebsites.net/[your-extension-name]`

Pange tähele, et URL näeb välja nagu URL-i oma veebirakenduse jaoks, et see kasutab HTTPS-i ja sisaldab ".scm".

On võimalik keelata kõik privaatseks (mitte eelinstallitud) laiendid oma veebirakenduse arendamise ja juurdluse ajal, lisades mõne rakenduse sätete võti `WEBSITE_PRIVATE_EXTENSIONS` ja väärtus `0`.

>[AZURE.NOTE] Kui soovite alustada Azure'i rakendust Service enne Azure'i konto kasutajaks, minge [Proovige rakenduse teenus](http://go.microsoft.com/fwlink/?LinkId=523751), kus saate kohe luua lühiajaline starter web app rakenduse teenus. Nõutav; krediitkaardid kohustusi.

## <a name="whats-changed"></a>Mis on muutunud
* Muuda juhend veebisaitide rakenduse teenusega leiate: [Azure'i rakendust Service ja selle mõju olemasoleva Azure'i teenused](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- IMAGES -->
[TransformSitePHPUI]: ./media/web-sites-transform-extend/TransformSitePHPUI.png
[TransformSiteSolEx]: ./media/web-sites-transform-extend/TransformSiteSolEx.png
 
