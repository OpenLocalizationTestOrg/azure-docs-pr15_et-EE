<properties 
    pageTitle="Võtta kasutusele Azure'i oma esimese veebirakenduse viis minutit | Microsoft Azure'i" 
    description="Siit saate teada, kui lihtne on veebirakenduste Käivita rakendus teenuse valimi rakenduse juurutamine. Käivitage arengu kiiresti teha ja tulemuste vaatamiseks kohe." 
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""
/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/13/2016" 
    ms.author="cephalin"
/>
    
# <a name="deploy-your-first-web-app-to-azure-in-five-minutes"></a>Võtta kasutusele Azure'i oma esimese veebirakenduse viis minutit

Selle õpetuse abil saate juurutada oma esimese veebirakenduse [Azure'i rakendust Service](../app-service/app-service-value-prop-what-is.md).
Rakenduse teenuse abil saate luua veebirakenduste, [mobiilirakenduse tagasi lõpetatakse](/documentation/learning-paths/appservice-mobileapps/)ja [API rakendused](../app-service-api/app-service-api-apps-why-best-platform.md).

Siis: 

- Azure'i rakendust Service web app luua.
- Proovi kood juurutamine (valige ASP.net-i, PHP, Node.js, Java või Python vahel).
- Vaadake oma koodi, mis töötab reaalajas valmistamisel.
- Värskendage oma veebirakenduse samamoodi [tõuketeatised Git paneb](https://git-scm.com/docs/git-push)samal viisil.

>[AZURE.INCLUDE [app-service-linux](../../includes/app-service-linux.md)] 

## <a name="prerequisites"></a>Eeltingimused

- [Git](http://www.git-scm.com/downloads).
- [Azure'i CLI](../xplat-cli-install.md).
- Microsoft Azure'i konto. Kui teil pole kontot, saate [tasuta prooviversiooni kasutajaks](/pricing/free-trial/?WT.mc_id=A261C142F) või [aktiveerida oma Visual Studio abonendi eelised](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

>[AZURE.NOTE] Võite [Proovida rakenduse teenuse](http://go.microsoft.com/fwlink/?LinkId=523751) ilma Azure'i konto. Starteri rakenduse loomine ja esitamine seda kuni tund--krediitkaarti nõutav, kohustusi.

## <a name="deploy-a-web-app"></a>Web appi juurutamine

Vaatame juurutada web appi Azure'i rakendust Service.

1. Avage uus Käsuviip Windows PowerShelli aknas Linuxi shelli või OS X terminal. Käivitage `git --version` ja `azure --version` kinnitamaks, et teie arvutisse on installitud Git ja Azure CLI.

    ![Installi CLI tööriistade esimese veebirakenduse jaoks testimine Azure](./media/app-service-web-get-started/1-test-tools.png)

    Kui teil pole installitud tööriistu, lugege teemat [eeltingimused](#Prerequisites) linke.

3. Logige sisse Azure'i umbes järgmine:

        azure login

    Järgige abi sõnumi login jätkata.

    ![Azure'i luua oma esimese web appi sisse logida](./media/app-service-web-get-started/3-azure-login.png)

4. Azure'i CLI muutuda ASM režiimi ja määrake juurutamise kasutaja rakenduse teenuse jaoks. Kas juurutamist mandaadiga hiljem kood.

        azure config mode asm
        azure site deployment user set --username <username> --pass <password>

1. Töötamine kausta (`CD`) ja klooni valimi rakenduse umbes järgmine:

        git clone <github_sample_url>

    ![Klooni rakenduse proovi kood Azure esimese veebirakenduse jaoks](./media/app-service-web-get-started/2-clone-sample.png)

    Jaoks * &lt;github_sample_url >*, kasutage ühte järgmistest URL, sõltuvalt sellest, mis teile meeldib raames:

    - HTML -i + CSS-i + JS: [https://github.com/Azure-Samples/app-service-web-html-get-started.git](https://github.com/Azure-Samples/app-service-web-html-get-started.git)
    - ASP.net-i: [https://github.com/Azure-Samples/app-service-web-dotnet-get-started.git](https://github.com/Azure-Samples/app-service-web-dotnet-get-started.git)
    - PHP (CodeIgniter): [https://github.com/Azure-Samples/app-service-web-php-get-started.git](https://github.com/Azure-Samples/app-service-web-php-get-started.git)
    - Node.js (Express): [https://github.com/Azure-Samples/app-service-web-nodejs-get-started.git](https://github.com/Azure-Samples/app-service-web-nodejs-get-started.git)
    - Java: [https://github.com/Azure-Samples/app-service-web-java-get-started.git](https://github.com/Azure-Samples/app-service-web-java-get-started.git)
    - Python (Django): [https://github.com/Azure-Samples/app-service-web-python-get-started.git](https://github.com/Azure-Samples/app-service-web-python-get-started.git)

2. Saate muuta rakenduse valimi hoidla. Näiteks:

        cd app-service-web-html-get-started

4. Luua rakenduse rakendust Service ressursi Azure kordumatu rakenduse nimi ja juurutamise kasutaja konfigureeritud varasemas versioonis. Kui teilt küsitakse, määrake soovitud piirkond arv.

        azure site create <app_name> --git --gitusername <username>

    ![Azure'i Azure ressursi esimese veebirakenduse jaoks loomine](./media/app-service-web-get-started/4-create-site.png)

    Rakenduse luuakse Azure'i kohe. Lisaks praeguse kataloogi on Git lähtestatud ja ühendatud uus rakendus teenuse rakendus Git remote.
    Saate sirvida rakenduse URL-i (http://&lt;app_name >. azurewebsites.net) ilus vaikelehe HTML-i näha, kuid vaatame tegelikult saada oma koodi nüüd.

4. Nagu vajutada mis tahes koodi Git juurutada Azure rakenduse oma proovi kood. Vastava viiba kuvamisel kasutada varem konfigureeritud parool.

        git push azure master

    ![Tõuketeatised koodi esimese veebirakendusse Azure](./media/app-service-web-get-started/5-push-code.png)

    Kui kasutasite keele raamistiku, kuvatakse väljund. `git push`paneb koodi Azure, vaid ka käivitab juurutamise engine juurutamise tööülesanded. Kui teil on mõni package.json (Node.js) või requirements.txt (Python) faile oma projekti (hoidla) juursait või kui teil on packages.config faili ASP.net-i projekti, taastab juurutamise skripti nõutav paketid. Samuti saate automaatselt töödelda composer.json faile PHP rakenduse [helilooja laiendamiseks](web-sites-php-mysql-deploy-use-git.md#composer) .

Palju õnne, mida on juurutatud rakenduse Azure'i rakendust Service.

## <a name="see-your-app-running-live"></a>Vaadake oma rakendus töötab reaalajas

Teie rakendus töötab reaalajas Azure kuvamiseks käivitage see käsk oma hoidlas, mis tahes kataloogist:

    azure site browse

## <a name="make-updates-to-your-app"></a>Tehke oma rakenduse värskendused

Nüüd saate Git push saidil update teha oma projekti (hoidla) juursait igal ajal. Seda Samamoodi nagu kui juurutatud oma koodi esimest korda. Näiteks iga kord, kui soovite uue muutmine, et te olete testitud kohalikult push, lihtsalt käivitage järgmised käsud oma projekti (hoidla) juursait:

    git add .
    git commit -m "<your_message>"
    git push azure master

## <a name="next-steps"></a>Järgmised sammud

Otsige oma keele framework eelistatud arendamise ja juurutamise juhised.

> [AZURE.SELECTOR]
- [.NET-I](web-sites-dotnet-get-started.md)
- [PHP](app-service-web-php-get-started.md)
- [Node.js](app-service-web-nodejs-get-started.md)
- [Python](web-sites-python-ptvs-django-mysql.md)
- [Java](web-sites-java-get-started.md)

Või oma esimese veebirakenduse lisavõimalused. Näiteks:

- [Muud võimalused oma koodi Azure juurutamine](../app-service-web/web-sites-deploy.md)proovida. Näiteks ühest oma GitHub hoidlate juurutamiseks lihtsalt valige **GitHub** asemel **Kohalik Git hoidla** **kasutamise võimalusi**.
- Azure'i rakenduse viimine uuele tasemele. Oma kasutajate autentimiseks. Selle aluseks nõudmisel skaala. Mõned jõudluse teatiste häälestamine. Kõik vaid mõne hiireklõpsuga. Lugege teemat [esimese veebirakendusse lisa funktsioonid](app-service-web-get-started-2.md).

