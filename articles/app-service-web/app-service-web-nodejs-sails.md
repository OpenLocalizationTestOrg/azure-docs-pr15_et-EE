<properties
    pageTitle="Azure'i rakendust Service juurutama Sails.js web app"
    description="Saate teada, kuidas Node.js rakenduse Azure'i rakendust Service juurutamine. Selle õpetuse näitab, kuidas kasutada Sails.js web app."
    services="app-service\web"
    documentationCenter="nodejs"
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="09/23/2016"
    ms.author="cephalin"/>

# <a name="deploy-a-sailsjs-web-app-to-azure-app-service"></a>Azure'i rakendust Service juurutama Sails.js web app

Selle õpetuse näidatakse, kuidas juurutada Sails.js rakenduse Azure'i rakendust Service. Protsessi, saate noppima mõned üldine teadmisi, kuidas konfigureerida rakenduse Node.js rakenduse teenuse käivitamiseks. 

Peaks olema Sails.js tundma. Selles õpetuses ei peaks teid aidata töötab Sail.js üldiselt seotud probleemid.


## <a name="prerequisites"></a>Eeltingimused

- [Node.js](https://nodejs.org/)
- [Sails.js](http://sailsjs.org/get-started)
- [Git](http://www.git-scm.com/downloads)
- [Azure'i CLI](../xplat-cli-install.md)
- Microsoft Azure'i konto. Kui teil pole kontot, saate [tasuta prooviversiooni kasutajaks](/pricing/free-trial/?WT.mc_id=A261C142F) või [aktiveerida oma Visual Studio abonendi eelised](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

>[AZURE.NOTE] Vaadake Azure'i rakendust Service teha enne Azure'i konto kasutajaks, minge [Proovige rakenduse teenuse](http://go.microsoft.com/fwlink/?LinkId=523751). Olemas, saate kohe luua lühiajaline starter rakenduse rakendust Service – pole vaja krediitkaarti, kohustusi.

## <a name="step-1-create-a-sailsjs-app-locally"></a>Samm 1: Sails.js rakenduse kohalik loomine

Esmalt kiiresti luua vaikimisi Sails.js rakendus oma arenduskeskkond järgmiste juhiste järgi:

1. Avage oma valik käsurea terminal ja `CD` töötamise kataloogi.

2. Sails.js rakenduse loomine ja käivitage see.

        sails new <appname>
        cd <appname>
        sails lift

    Veenduge, et saate liikuda vaikimisi avalehe http://localhost:1377 juures.

## <a name="step-2-create-the-azure-app-resource"></a>Samm 2: Looge Azure rakenduse ressurss

Järgmiseks luua Azure'i rakendust Service ressursi. Te ei kavatse Juurutage Sails.js rakendus selle hiljem.

1. Logige sisse Azure, näiteks nii:
1. Sama terminalis ASM režiimi muutmine ja Azure sisse logida.

        azure config mode asm
        azure login

    Järgige küsimuse jätkamiseks login Microsofti kontoga, mis on Azure tellimuse brauseris.

2. Veenduge, et olete endiselt juurkaust Sails.js projekti. Looge rakenduse rakendust Service ressursi Azure kordumatu rakenduse nimega järgmise käsu abil. Oma veebirakenduse URL on http://&lt;rakendusenimi >. azurewebsites.net.

        azure site create --git <appname>

    Järgige viip valige Azure regioon, kus juurutada. Kui te olete kunagi häälestanud Git/FTP juurutamise identimisteabe Azure tellimuse, ka palutakse neid luua.

    Kui rakenduse rakendust Service ressursi on loodud:

    - Sails.js rakendus on Git lähtestatud
    - Kohaliku Git lähtestatud hoidla on ühendatud uus rakendus teenuse rakendus nimega remote, Git tabavalt nimeks "azure" ja
    - Ja iisnode.yml fail on loodud oma juurkaust. Saate konfigureerida [iisnode](https://github.com/tjanczuk/iisnode), mis kasutab rakenduse Service Node.js rakenduste käivitamiseks seda faili.

## <a name="step-3-configure-and-deploy-your-sailsjs-app"></a>Samm 3: Konfigureerimine ja Juurutage Sails.js rakendus

 Sails.js rakenduse App teenuses töötamise koosneb kolm põhitoimingut.

 - Teie minirakendus käivitada rakenduse teenuse konfigureerimine
 - Selle juurutama rakenduse teenus
 - Juurutamise seotud probleemide tõrkeotsinguks lugege stderr ja stdout logid

Tehke järgmist.

1. Avage uus iisnode.yml fail oma juurkaust ja lisada järgmised kaks rida.

        loggingEnabled: true
        logDirectory: iisnode

    Logimine on lubatud nüüd iisnode. Lisateavet selle kohta, kuidas see toimib, lugege teemat  [stdout ja stderr logid iisnode kaudu](app-service-web-nodejs-get-started.md#iisnodelog).

2. Avage config/env/production.js konfigureerida tootmiskeskkonnast ja määrake `port` ja `hookTimeout`:

        module.exports = {

            // Use process.env.port to handle web requests to the default HTTP port
            port: process.env.port,
            // Increase hooks timout to 30 seconds
            // This avoids the Sails.js error documented at https://github.com/balderdashy/sails/issues/2691
            hookTimeout: 30000,

            ...
        };

    Nende sätete konfigureerimine dokumentatsioonist leiate  [Sails.js dokumentatsiooni](http://sailsjs.org/documentation/reference/configuration/sails-config).

    Järgmiseks peate veenduge, et [Grunt](https://www.npmjs.com/package/grunt) ühildub Azure võrgu kaudu. Grunt versioonid, mis on väiksem kui 1.0.0 kasutab aegunud [glob](https://www.npmjs.com/package/glob) pakett (väiksem kui 5.0.14), mis ei toeta võrgu kaudu. 

3. Package.json avada ja muuta selle `grunt` versiooni `1.0.0` ja eemaldage kõik `grunt-*` paketid. Teie `dependencies` atribuut peaks välja nägema umbes järgmine:

        "dependencies": {
            "ejs": "<leave-as-is>",
            "grunt": "1.0.0",
            "include-all": "<leave-as-is>",
            "rc": "<leave-as-is>",
            "sails": "<leave-as-is>",
            "sails-disk": "<leave-as-is>",
            "sails-sqlserver": "<leave-as-is>"
        },

3. Package.json, lisage järgmine `engines` atribuudi seadmiseks ühele, mille soovime Node.js versioon.

        "engines": {
            "node": "6.6.0"
        },

6. Muudatuste salvestamiseks ja veenduge, et teie rakendus töötab endiselt kohalikult muudatuste kontrollimiseks. Selleks, et kustutada selle `node_modules` kausta ja seejärel käivitage:

        npm install
        sails lift

4. Nüüd Juurutage rakendus Azure git abil:

        git add .
        git commit -m "<your commit message>"
        git push azure master

5. Lõpuks lihtsalt käivitada reaalajas Azure rakenduse brauseris:

        azure site browse

    Nüüd peaks nähtaval olema sama Sails.js avalehele.
    
    ![](./media/app-service-web-nodejs-sails/sails-in-azure.png)

## <a name="troubleshoot-your-deployment"></a>Juurutamise tõrkeotsing

Sails.js rakenduse nurjumisel mingil põhjusel rakenduse teenuses leida tõrkeotsinguks selle stderr logid.
Lisateabe saamiseks lugege teemat [stdout ja stderr logid iisnode kaudu](app-service-web-nodejs-sails.md#iisnodelog).
Kui see käivitatud, stdout log peaks näitab teile tuttavad sõnum:

                .-..-.

    Sails              <|    .-..-.
    v0.12.4             |\
                        /|.\
                        / || \
                    ,'  |'  \
                    .-'.-==|/_--'
                    `--'-------' 
    __---___--___---___--___---___--___
    ____---___--___---___--___---___--___-__

    Server lifted in `D:\home\site\wwwroot`
    To see your app, visit http://localhost:\\.\pipe\c775303c-0ebc-4854-8ddd-2e280aabccac
    To shut down Sails, press <CTRL> + C at any time.

Saate määrata granulaarsus stdout logid [config/log.js](http://sailsjs.org/#!/documentation/concepts/Logging) faili. 

## <a name="connect-to-a-database-in-azure"></a>Azure andmebaasiga ühenduse loomiseks

Azure'i andmebaasi ühenduse luua andmebaasi oma valik Azure, näiteks Azure'i SQL-andmebaasi, MySQL-i, MongoDB, Azure (Redis) vahemälu jne ja vastavate [andmesalve adapterit](https://github.com/balderdashy/sails#compatibility) ühenduse loomiseks kasutada. Selle jaotise juhised näitavad, kuidas Azure MySQL-i andmebaasiga ühenduse loomiseks.

1. Järgige selle kuueosalisest [siin](../store-php-create-mysql-database.md) Azure MySQL-i andmebaasi loomiseks.

2. Installige kaudu oma käsurea terminalis MySQL-i adapterit.

        npm install sails-mysql --save

3. Avage config/connections.js ja järgmised ühenduse objekti lisamine loendisse. 

        mySql: {
            adapter: 'sails-mysql',
            user: process.env.dbuser,
            password: process.env.dbpassword,
            host: process.env.dbhost, 
            database: process.env.dbname,
            options: {
                encrypt: true
            }
        },

4. Keskkonna muutujate (`process.env.*`), peate määrama selle rakenduse teenus. Selle tegemiseks käivitage järgmised käsud oma terminal kaudu. Kõik ühendus vajalik teave on Azure portaali (vt [ühenduse loomine MySQL-andmebaasiga](../store-php-create-mysql-database.md#connect)).

        azure site appsetting add dbuser="<database user>"
        azure site appsetting add dbpassword="<database password>"
        azure site appsetting add dbhost="<database hostname>"
        azure site appsetting add dbname="<database name>"
        
    Lihtsate sätete Azure rakenduse sätetes hoiab tundliku loomuga andmete allikas kontrolli (Git). Järgmiseks tuleb konfigureerida oma arenduskeskkond kasutada sama ühenduseteavet.

4. Avage config/local.js ja lisada järgmised ühendused objekti:

        connections: {
            mySql: {
                user: "<database user>",
                password: "<database password>",
                host: "<database hostname>", 
                database: "<database name>",
            },
        },
    
    Selle konfiguratsiooni alistab config/connections.js faili kohaliku keskkonna sätteid. See fail on välistatud vaikimisi .gitignore projektis, nii, et see ei salvestata git. Nüüd olete nii Azure web Appi kui ka teie kohaliku arenduskeskkond MySQL-andmebaasiga ühendust luua.

4. Avage config/env/production.js tootmiskeskkonnast konfigureerimiseks ja lisage järgmine `models` objekti:

        models: {
            connection: 'mySql',
            migrate: 'safe'
        },

4. Avage oma arenduskeskkond konfigureerimiseks config/env/development.js ja lisage järgmine `models` objekti:

        models: {
            connection: 'mySql',
            migrate: 'alter'
        },

    `migrate: 'alter'`võimaldab teil andmebaasi migreerimine funktsioonide abil saate luua ja värskendada oma MySQL andmebaasitabelites hõlpsalt. Siiski `migrate: 'safe'` kasutatakse keskkonna Azure (tootmine), kuna Sails.js ei luba teil kasutada `migrate: 'alter'` tootmiskeskkonnas (vt  [Sails.js dokumendid](http://sailsjs.org/documentation/concepts/models-and-orm/model-settings)).

4. Kaugusel [luua](http://sailsjs.org/documentation/reference/command-line-interface/sails-generate) Sails.js [näidis API](http://sailsjs.org/documentation/concepts/blueprints) nagu tavaliselt, siis läheks `sails lift` Sails.js andmebaasi migreerimine andmebaasi loomiseks. Näiteks:

         sails generate api mywidget
         sails lift

    Funktsiooni `mywidget` mudelit, mis on loodud see käsk on tühi, kuid saate kasutame kuvamiseks, et meil andmebaasipöördus.
    Kui käivitate `sails lift`, loob puuduvad tabelid mudelite teie rakendus kasutab.

6. Juurdepääs näidis API äsja loodud brauseris. Näiteks:

        http://localhost:1337/mywidget/create
    
    API peaks naasta loodud kirje te brauseriaknas, mis tähendab, et teie andmebaas on loodud.

        {"id":1,"createdAt":"2016-09-23T13:32:00.000Z","updatedAt":"2016-09-23T13:32:00.000Z"}

5. Nüüd, vajutage muudatuste Azure ja sirvige rakenduse veendumaks, et see töötab.

        git add .
        git commit -m "<your commit message>"
        git push azure master
        azure site browse

6. Näidis API oma Azure veebirakenduse juurde. Näiteks:

        http://<appname>.azurewebsites.net/mywidget/create

    Kui API tagastab uue kirjele, räägib oma Azure veebirakenduse MySQL-andmebaasiga.

## <a name="more-resources"></a>Veel ressursse

- [Azure'i rakendust Service Node.js veebirakendustega alustamine](app-service-web-nodejs-get-started.md)
- [Azure'i rakendustega Node.js moodulid abil](../nodejs-use-node-modules-azure-apps.md)
