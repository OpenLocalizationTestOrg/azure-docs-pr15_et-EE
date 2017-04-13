<properties
    pageTitle="Loomine ja Azure MySQL-andmebaasiga ühenduse loomine"
    description="Saate teada, kuidas kasutada Azure portaali loomine MySQL-andmebaasiga ja looge siis PHP web appi Azure kaudu."
    documentationCenter="php"
    services="app-service\web"
    authors="cephalin"
    manager="wpickett"
    editor=""
    tags="mysql"/>

<tags
    ms.service="multiple"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm;cephalin"/>

# <a name="create-and-connect-to-a-mysql-database-in-azure"></a>Loomine ja Azure MySQL-andmebaasiga ühenduse loomine

Selle õpetuse näitab, kuidas MySQL-i andmebaasi loomine [Azure portaali](https://portal.azure.com) (pakkuja on [ClearDB](http://www.cleardb.com/)) ja kuidas ühendada PHP web Appist teenuses [Azure rakendus](./app-service/app-service-value-prop-what-is.md)töötab. 

> [AZURE.NOTE] Saate luua ka MySQL-i andmebaasi [turuplatsi rakenduse malli](./app-service-web/app-service-web-create-web-app-from-marketplace.md)osana.

## <a name="create-a-mysql-database-in-azure-portal"></a>Azure'i portaalis MySQL-i andmebaasi loomine

Azure'i portaalis MySQL-i andmebaasi loomiseks tehke järgmist.

1. [Azure'i portaali](https://portal.azure.com)sisse logida.

2. Klõpsake vasakpoolses menüüs **Uus** > **andmete + salvestusruumi** > **MySQL-andmebaasiga**.

    ![Azure - start MySQL-i andmebaasi loomine](./media/store-php-create-mysql-database/create-db-1-start.png)

2. MySQL-andmebaasiga [blade](azure-portal-overview.md)konfigureerimine uue MySQL-i andmebaasi järgmiselt (*blade*: portaal, mis avab horisontaalselt):

    - **Andmebaasi nimi**: tippige üheselt tuvastada nimi
    - **Tellimus**: valige tellimus, kasutamine
    - **Andmebaasi tüüp**: valige **ühiskasutuses** jaoks madala hinnaga või tasuta astme või **pühendunud** saada sihtotstarbeline ressursse. 
    - **Ressursirühm**: MySQL-andmebaasiga lisamiseks olemasolevasse [ressursirühm](../azure-resource-manager/resource-group-overview.md) või pane see uus. Ressursside sama rühma saate hõlpsasti hallatavate koos.
    - **Asukoht**: valige asukoht, kuhu teie lähedal. Olemasoleva ressursi rühma lisamisel on lukustatud ressursi rühma asukohta.
    - **Hinnad taseme**: klõpsake **Taseme hinnad**ja seejärel hinnakirjad suvand (**elavhõbeda** taseme on tasuta), ja seejärel klõpsake nuppu **Vali**. 
    - **Juriidilised tingimused**: klõpsake **Õiguslikult**ostu üksikasjad üle ja klõpsake nuppu **osta**.
    - **PIN-koodi armatuurlaud**: valige, kui soovite kasutada seda otse armatuurlaud. See on eriti kasulik, kui te pole veel portaali navigeerimise tuttav.
    
    Järgmine pilt on lihtsalt näide sellest, kuidas seadistada MySQL-andmebaasiga.  
    ![MySQL-i andmebaasi loomine Azure - konfigureerimine](./media/store-php-create-mysql-database/create-db-2-configure.png)

3. Kui olete lõpetanud konfigureerida, klõpsake nuppu **Loo**.

    ![Azure MySQL-i andmebaasi loomine – saate luua](./media/store-php-create-mysql-database/create-db-3-create.png)

    Kuvatakse hüpikteatise ettevõttevälistel, teate, et juurutamise käivitas.

    ![Poolelioleva Azure - MySQL-i andmebaasi loomine](./media/store-php-create-mysql-database/create-db-4-started-status.png)

    Saate teise hüpikakende kui juurutamise õnnestus. Portaali Lisaks avamiseks oma MySQL-i andmebaasi blade automaatselt.

<a name="connect"></a>
## <a name="connect-to-your-mysql-database"></a>Ühenduse loomine MySQL-andmebaasiga

Uue MySQL-i andmebaasi ühendus teabe saamiseks klõpsake lihtsalt **Atribuudid** web appi tera.
    
![Azure - MySQL-i andmebaasi blade MySQL-i andmebaasi loomine](./media/store-php-create-mysql-database/create-db-5-finished-db-blade.png)

Nüüd saate selle ühenduse teavet mis tahes web Appis. Näide, mis näitab, kuidas kasutada ühendusteabe lihtsa PHP rakendusest on saadaval [siin](https://github.com/WindowsAzure/azure-sdk-for-php-samples/tree/master/tasklist-mysql).

## <a name="connect-a-laravel-web-app-from-the-php-get-started-tutorial"></a>Ühenduse loomine Laravel web app (alates PHP toomine alustamise õpetus)

Oletame, et te just lõpetanud õpetus [loomine, konfigureerimiseks ja juurutada PHP web appi Azure](./app-service-web/app-service-web-php-get-started.md) ja on [Laravel](https://www.laravel.com/) web appi Azure töötab. Saate hõlpsalt lisada andmebaasi võimaluste Laravel rakenduse. Järgige alltoodud juhiseid:

>[AZURE.NOTE] Järgmiste juhiste korral eeldatakse lõpetamist õpetus [loomine, konfigureerimiseks ja PHP web appi Azure juurutamine](./app-service-web/app-service-web-php-get-started.md).

1. Konfigureerida rakendus Laravel andmebaasi MySQL-i osutamiseks teie kohaliku arenduskeskkond. Selle tegemiseks Avage `.env` Laravel rakenduste root directory ja konfigureerida MySQL-i andmebaasi suvandid.

        DB_CONNECTION=mysql
        DB_HOST=<HOSTNAME_from_properties_blade>
        DB_PORT=<PORT_from_properties_blade>
        DB_DATABASE=<see_note_below>
        DB_USERNAME=<USERNAME_from_properties_blade>
        DB_PASSWORD=<PASSWORD_from_properties_blade>

    >[AZURE.NOTE] **Atribuutide** labale MySQL-i andmebaasi nimi võib või ei pruugi olla selline, nagu esitatud väljale **Andmebaasi nimi** . See on parem kontrollida andmebaasi parameetri väljal **ÜHENDUSSTRING** . 
    >
    >![Poolelioleva Azure - MySQL-i andmebaasi loomine](./media/store-php-create-mysql-database/connect-db-1-database-name.png)

2. Kiireim viis veenduge, et teil on juurdepääs MySQL-i nüüd on kasutada [Laravel on vaikimisi autentimise tellingud](https://laravel.com/docs/5.2/authentication#authentication-quickstart). Käivitage käsurea terminalis Laravel rakenduse juurkaust järgmised käsud:

         php artisan migrate
         php artisan make:auth

    Esimese käsu loob tabelite põhjal eelmääratletud migratsioon rakenduses Azure on `database/migrations` kataloogi ja teine käsk mikrokapslid peamiste vaadete ja marsruudib kasutajaks registreerumine ja autentimise jaoks.

3. Käivitage arengu server kohe.

        php artisan serve

4. Brauseris, liikuge http://localhost:8000 ja Registreeri uus kasutaja, nagu on näidatud:

    ![Ühenduse loomine MySQL-andmebaasiga Azure – kasutaja registreerimine](./media/store-php-create-mysql-database/connect-db-2-development-server.png)

    Järgige UI küsimuse täieliku registreerimise. Kui registreerimine on lõpule jõudnud, logitakse.
    
    ![Ühenduse loomine MySQL-andmebaasiga Azure – kasutaja registreerimine](./media/store-php-create-mysql-database/connect-db-3-registered-user.png)

    Teil on nüüd tekkinud rakenduse Azure MySQL-i andmebaasist.

5. Nüüd, tuleb lihtsalt ise oma `.env` sätteid nii, et teie Azure web Appis. Käivitage järgmised Azure'i CLI käsud:

        azure site appsetting add DB_CONNECTION=mysql
        azure site appsetting add DB_HOST=<HOSTNAME_from_properties_blade>
        azure site appsetting add DB_PORT=<PORT_from_properties_blade>
        azure site appsetting add DB_DATABASE=<Database_param_from_CONNECTION_INFO_from_properties_blade>
        azure site appsetting add DB_USERNAME=<USERNAME_from_properties_blade>
        azure site appsetting add DB_PASSWORD=<PASSWORD_from_properties_blade>

    Siit saate teada, kuidas see toimib [Azure web appi](./app-service-web/app-service-web-php-get-started.md#configure)konfigureerimine.

6. Seejärel Kinnita ja push Azure kohaliku ajal varem tehtud muudatused `php artisan make:auth`.

        git add .
        git commit -m "scaffold auth views and routes"
        git push azure master

7. Liikuge sirvides Azure web appi.

        azure site browse

8. Logige sisse varem loodud kasutaja mandaadi abil.

    ![Ühenduse loomine MySQL-andmebaasiga Azure - sirvige Azure web app](./media/store-php-create-mysql-database/connect-db-4-browse-azure-webapp.png)

    Pärast seda, kui logite, peaksite nägema sõbralik pärast sisselogimine.
    
    ![Ühenduse loomine MySQL-andmebaasiga Azure - sisse logitud](./media/store-php-create-mysql-database/connect-db-5-logged-in.png)

    Palju õnne, teie PHP web Appis Azure on nüüd juurdepääsu andmeid MySQL-andmebaasiga. 

## <a name="next-steps"></a>Järgmised sammud

Lisateavet leiate teemast [PHP Arenduskeskus](/develop/php/).
