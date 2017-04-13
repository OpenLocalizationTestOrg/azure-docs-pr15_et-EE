<properties 
    pageTitle="Võtta kasutusele Azure'i esimese PHP veebirakenduse viis minutit | Microsoft Azure'i" 
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
    
# <a name="deploy-your-first-php-web-app-to-azure-in-five-minutes"></a>Võtta kasutusele Azure'i esimese PHP veebirakenduse viis minutit

Selle õpetuse abil saate juurutada esimese PHP veebirakenduse [Azure'i rakendust Service](../app-service/app-service-value-prop-what-is.md).
Rakenduse teenuse abil saate luua veebirakenduste, [mobiilirakenduse tagasi lõpetatakse](/documentation/learning-paths/appservice-mobileapps/)ja [API rakendused](../app-service-api/app-service-api-apps-why-best-platform.md).

Siis: 

- Azure'i rakendust Service web app luua.
- Juurutada PHP kood.
- Vaadake oma koodi, mis töötab reaalajas valmistamisel.
- Värskendage oma veebirakenduse samamoodi [tõuketeatised Git paneb](https://git-scm.com/docs/git-push)samal viisil.

## <a name="prerequisites"></a>Eeltingimused

- [Git](http://www.git-scm.com/downloads).
- [Azure'i CLI](../xplat-cli-install.md).
- Microsoft Azure'i konto. Kui teil pole kontot, saate [tasuta prooviversiooni kasutajaks](/pricing/free-trial/?WT.mc_id=A261C142F) või [aktiveerida oma Visual Studio abonendi eelised](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

>[AZURE.NOTE] Võite [Proovida rakenduse teenuse](http://go.microsoft.com/fwlink/?LinkId=523751) ilma Azure'i konto. Starteri rakenduse loomine ja esitamine seda kuni tund--krediitkaarti nõutav, kohustusi.

## <a name="deploy-a-php-web-app"></a>Kasutada PHP web app

1. Avage uus Windowsi Käsuviip PowerShelli aknas Linuxi shelli või OS X terminal. Käivitage `git --version` ja `azure --version` kinnitamaks, et teie arvutisse on installitud Git ja Azure CLI.

    ![Testi installi CLI vahendite Azure esimese veebirakenduse jaoks](./media/app-service-web-get-started/1-test-tools.png)

    Kui teil pole installitud tööriistu, lugege teemat [eeltingimused](#Prerequisites) linke.

3. Logige sisse Azure'i umbes järgmine:

        azure login

    Järgige abi sõnumi login jätkata.

    ![Azure'i luua oma esimese web appi sisse logida](./media/app-service-web-get-started/3-azure-login.png)

4. Azure'i CLI muutuda ASM režiimi ja määrake juurutamise kasutaja rakenduse teenuse jaoks. Kas juurutamist mandaadiga hiljem kood.

        azure config mode asm
        azure site deployment user set --username <username> --pass <password>

1. Töötamine kausta (`CD`) ja klooni valimi rakenduse umbes järgmine:

        git clone https://github.com/Azure-Samples/app-service-web-php-get-started.git

2. Saate muuta rakenduse valimi hoidla. Näiteks:

        cd app-service-web-php-get-started

4. Luua rakenduse rakendust Service ressursi Azure kordumatu rakenduse nimi ja juurutamise kasutaja konfigureeritud varasemas versioonis. Kui teilt küsitakse, määrake soovitud piirkond arv.

        azure site create <app_name> --git --gitusername <username>

    ![Azure'i Azure ressursi esimese veebirakenduse jaoks loomine](./media/app-service-web-get-started-languages/php-site-create.png)

    Rakenduse luuakse Azure'i kohe. Lisaks praeguse kataloogi on Git lähtestatud ja ühendatud uus rakendus teenuse rakendus Git remote.
    Saate sirvida rakenduse URL-i (http://&lt;app_name >. azurewebsites.net) ilus vaikelehe HTML-i näha, kuid vaatame tegelikult saada oma koodi nüüd.

4. Nagu vajutada mis tahes koodi Git juurutada Azure rakenduse oma proovi kood. Vastava viiba kuvamisel kasutada varem konfigureeritud parool.

        git push azure master

    ![Tõuketeatised koodi esimese veebirakendusse Azure](./media/app-service-web-get-started-languages/php-git-push.png)

    `git push`paneb koodi Azure, vaid ka käivitab juurutamise engine juurutamise tööülesanded. Samuti saate automaatselt töödelda composer.json faile PHP rakenduse  [helilooja laiendamiseks](web-sites-php-mysql-deploy-use-git.md#composer) .

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

[Loo konfigureerimiseks ja juurutamine Laravel veebirakenduse Azure](app-service-web-php-get-started.md). Järgides selles õppetükis saate teada, oskused peate käivitama mis tahes PHP web appi Azure, näiteks:

- Loomine ja konfigureerimine rakenduste Azure PowerShelli/Bash.
- Seadmine PHP versioon.
- Kasutage start faili, mis pole rakenduses juurkaust.
- Luba helilooja automatiseerimine.
- Accessi keskkonna kohased muutujate.
- Levinud probleemide tõrkeotsingu.

Või oma esimese veebirakenduse lisavõimalused. Näiteks:

- [Muud võimalused oma koodi Azure juurutamine](../app-service-web/web-sites-deploy.md)proovida. Näiteks ühest oma GitHub hoidlate juurutamiseks lihtsalt valige **GitHub** asemel **Kohalik Git hoidla** **kasutamise võimalusi**.
- Azure'i rakenduse viimine uuele tasemele. Oma kasutajate autentimiseks. Selle aluseks nõudmisel skaala. Mõned jõudluse teatiste häälestamine. Kõik vaid mõne hiireklõpsuga. Lugege teemat [esimese veebirakendusse lisa funktsioonid](app-service-web-get-started-2.md).

