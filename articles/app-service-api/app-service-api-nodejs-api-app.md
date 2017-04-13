<properties
    pageTitle="Azure'i rakendust Service Node.js API rakenduse | Microsoft Azure'i"
    description="Saate teada, kuidas luua Node.js rahulik API ja selle juurutama Azure'i rakendust Service rakenduse API."
    services="app-service\api"
    documentationCenter="node"
    authors="bradygaster"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-api"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="node"
    ms.topic="get-started-article"
    ms.date="05/26/2016"
    ms.author="rachelap"/>

# <a name="build-a-nodejs-restful-api-and-deploy-it-to-an-api-app-in-azure"></a>Node.js rahulik API luua ja juurutada Azure rakendus API

[AZURE.INCLUDE [app-service-api-get-started-selector](../../includes/app-service-api-get-started-selector.md)]

Selle õpetuse näitab, kuidas luua lihtne [Node.js](http://nodejs.org) API ja kasutusele võtta [API rakenduse](app-service-api-apps-why-best-platform.md) teenuses [Azure rakenduse](../app-service/app-service-value-prop-what-is.md) abil [Git](http://git-scm.com). Saate kasutada mis tahes operatsioonisüsteem, mida saab käitada Node.js ja saate teha kõiki töid, nt cmd.exe käsurea tööriistade kasutamine või bash.

## <a name="prerequisites"></a>Eeltingimused

1. Microsoft Azure'i konto ([avage tasuta konto](https://azure.microsoft.com/pricing/free-trial/))
1. [Node.js](http://nodejs.org) installitud (selle valimi eeldab, et teil on Node.js 4.2.2 versioon)
2. [Git](https://git-scm.com/) installitud
1. [GitHub](https://github.com/) konto

Teenuse rakendus toetab mitmel viisil juurutada oma kood API rakendus, selle õpetuse kuvatakse Git viis ja eeldab, et teil on lihtne teadmisi, kuidas töötada Git. Teiste meetodite juurutamise kohta leiate teavet teemast [Deploy oma Azure'i rakendust Service rakendus](../app-service-web/web-sites-deploy.md).

## <a name="get-the-sample-code"></a>Proovi kood hankimine

1. Avage mõne käsurea liides Node.js ja Git käsud töötavad.

1. Liikuge kausta, mille abil saate kohaliku Git hoidla ja klooni [GitHub hoidla, mis sisaldab proovi kood](https://github.com/Azure-Samples/app-service-api-node-contact-list).

        git clone https://github.com/Azure-Samples/app-service-api-node-contact-list.git

    Valimi API pakub kahte lõpp-punktid: Get-päringu abil `/contacts` tagastab loendi nimesid ja meiliaadresse JSON-vormingus ajal `/contacts/{id}` tagastab ainult valitud kontakti.

## <a name="scaffold-auto-generate-nodejs-code-based-on-swagger-metadata"></a>Tellingud (Loo) Node.js koodi põhjal ärplema metaandmed

[Ärplema](http://swagger.io/) on metaandmete, mis kirjeldab rahulik API failivorming. Azure'i rakenduse teenus on [ärplema metaandmete tugi](app-service-api-metadata.md). Selles jaotises õpetuse mudelid API arengu töövoo, kus saate luua ärplema metaandmete esmalt ja kasutage seda tellingud (Loo) API serveri kood. 

>[AZURE.NOTE] Selles jaotises saate vahele jätta, kui te ei soovi saate teada, kuidas tellingud Node.js kood ärplema metaandmete failist. Kui soovite lihtsalt juurutada proovi kood API uue rakenduse, minge jaotisse [Azure API rakenduse loomine](#createapiapp) .

### <a name="install-and-execute-swaggerize"></a>Installimine ja käivitada Swaggerize

1. Järgmised käsud installida **yo** ja **generaator swaggerize** NPM moodulid globaalselt käivitada.

        npm install -g yo
        npm install -g generator-swaggerize

    Swaggerize on tööriist, mida on kirjeldatud ärplema metaandmete faili API serveri kood tekitab. Ärplema fail, mida saate kasutada nimega *api.json* ja asub kaustas *alustada* saate kloonitud hoidla.

2. Liikuge kausta, *käivitamine* , ja seejärel käivitada soovitud `yo swaggerize` käsk. Swaggerize saavad küsimusi esitada.  , **Mida selle projekti helistada**, sisestage "ContactList" **tee ärplema dokumendi**, sisestage "api.json" ja **Express tulemusega, või Restify**, sisestage "kiire".

        yo swaggerize

    ![Käsurea swaggerize](media/app-service-api-nodejs-api-app/swaggerize-command-line.png)
    
    **Märkus**: selles juhises tõrke ilmnemisel Järgmiseks selgitab, kuidas seda parandada.

    Swaggerize loob mõnda kausta, toesed käitlejate ja failid ja loob **package.json** faili. Kiire vaade engine kasutatakse ärplema abi lehe loomiseks.  

3. Kui soovitud `swaggerize` käsk nurjub "ootamatu luba" või "lubamatud paojada" tõrge, tõrke põhjuse abil loodud *package.json* faili redigeerimise korrigeerida. Klõpsake soovitud `regenerate` jaotises joone `scripts`, tagasi kaldkriips, mis eelneb *api.json* kaldkriips, et muuta nii, et rida, mis näeb välja nagu järgmine näide:

        "regenerate": "yo swaggerize --only=handlers,models,tests --framework express --apiPath config/api.json"

1. Liikuge kausta, mis sisaldab scaffolded koodi (sel juhul */start/ContactList* alamkausta).

1. Käivitage `npm install`.
    
        npm install
        
2. Installige **jsonpath** NPM mooduli. 

        npm install --save jsonpath
        
    ![Jsonpath installimine](media/app-service-api-nodejs-api-app/jsonpath-install.png)

1. Installige **swaggerize ui** NPM mooduli. 

        npm install --save swaggerize-ui
        
    ![Swaggerize Ui installimine](media/app-service-api-nodejs-api-app/swaggerize-ui-install.png)

### <a name="customize-the-scaffolded-code"></a>Scaffolded koodi kohandamine

1. Kopeerige **teegi** kausta kaustast **käivitamine** on scaffolder loodud **ContactList** kausta. 

1. Asendage kood **handlers/contacts.js** faili järgmine kood. 

    Järgmine kood kasutab JSON **lib/contacts.json** faili, mis **lib/contactRepository.js**talletatud andmed. Uue contacts.js kood vastab HTTP päringutele saada kõik kontaktid ja tagastada need JSON last nimega. 

        'use strict';
        
        var repository = require('../lib/contactRepository');
        
        module.exports = {
            get: function contacts_get(req, res) {
                res.json(repository.all())
            }
        };

1. Asendage kood **handlers/contacts/{id}.js** faili fofllowing kood. 

        'use strict';

        var repository = require('../../lib/contactRepository');
        
        module.exports = {
            get: function contacts_get(req, res) {
                res.json(repository.get(req.params['id']));
            }    
        };

1. Asendage kood **server.js** järgmine kood. 

    Server.js failis tehtud muudatused nimetatakse kommentaaride abil, et näeksite on tehtud muudatused. 

        'use strict';

        var port = process.env.PORT || 8000; // first change

        var http = require('http');
        var express = require('express');
        var bodyParser = require('body-parser');
        var swaggerize = require('swaggerize-express');
        var swaggerUi = require('swaggerize-ui'); // second change
        var path = require('path');

        var app = express();

        var server = http.createServer(app);

        app.use(bodyParser.json());

        app.use(swaggerize({
            api: path.resolve('./config/api.json'), // third change
            handlers: path.resolve('./handlers'),
            docspath: '/swagger' // fourth change
        }));

        // change four
        app.use('/docs', swaggerUi({
          docs: '/swagger'  
        }));

        server.listen(port, function () { // fifth and final change
        });

### <a name="test-with-the-api-running-locally"></a>Kohalik töötab API testimine

1. Aktiveerida serveri käivitatava Node.js käsurea kaudu. 

        node server.js

1. **Http://localhost:8000/kontaktide**sirvimisel kuvatakse JSON väljund kontaktiloendis soovitud kontakti (või teil palutakse alla laadida, sõltuvalt teie brauser). 

    ![Kõigi kontaktide Api kõne](media/app-service-api-nodejs-api-app/all-contacts-api-call.png)

1. **Http://localhost:8000/kontaktid/2**sirvimisel kuvatakse kontakti tähistab selle id väärtuse.

    ![Kindla kontakti Api kõne](media/app-service-api-nodejs-api-app/specific-contact-api-call.png)

1. Ärplema JSON andmete **serveeritakse/ärplema** lõpp-punkti kaudu:

    ![Kontaktide ärplema Json](media/app-service-api-nodejs-api-app/contacts-swagger-json.png)

1. Ärplema UI serveeritakse **/docs** lõpp-punkti kaudu. Kasutajaliidese ärplema saate rikkaliku HTML-i klient funktsioonid katsetada oma API.

    ![Ärplema kasutajaliides](media/app-service-api-nodejs-api-app/swagger-ui.png)

## <a id="createapiapp"></a>Uue API rakenduse loomine

Selles jaotises saate Azure portaali luua uue API rakenduse Azure. See rakendus API tähistab Arvuta ressursse, mis annab Azure'i oma koodi käivitada. Osades kuvatakse uut API rakendust juurutada oma kood.

1. Liikuge sirvides [Azure portaali](https://portal.azure.com/). 

1. Klõpsake **uus > Web + Mobile > API rakenduse**. 

    ![Uue API rakenduse portaalis](media/app-service-api-nodejs-api-app/new-api-app-portal.png)

4. Sisestage soovitud **rakenduse nime** , mis on kordumatu *azurewebsites.net* domeeni, näiteks NodejsAPIApp pluss arvu, et see oleks kordumatu. 

    Näiteks kui nimi on `NodejsAPIApp`, URL-i saab `nodejsapiapp.azurewebsites.net`.

    Kui sisestate nime, mille keegi teine on juba kasutanud, kuvatakse punane hüüumärk paremale.

6. **Ressursirühm** ripploendis nuppu **Uus**ja **uue ressursirühma nimi** sisestage "NodejsAPIAppGroup" või mõne muu nime kui eelistate. 

    [Ressursirühm](../azure-resource-manager/resource-group-overview.md) on Azure ressursse, nt API rakendused, andmebaasid ja VMs kogum. Selles õpetuses on parim, kuna mis hõlbustab kustutada sammhaaval Azure ressursse, juhend on loodud uue ressursirühma loomiseks.

4. Klõpsake **Rakenduse leping/asukoht**ja seejärel klõpsake nuppu **Loo uus**.

    ![Rakenduse teenusleping loomine](./media/app-service-api-nodejs-api-app/newappserviceplan.png)

    Järgmistes juhistes, loote uue ressursirühma rakenduse teenusleping. Mõni rakendus teenusleping määrab Arvuta ressursse, mis töötab teie API rakenduse. Näiteks kui valite tasuta taseme, API rakenduse töötab ühiskasutusega VMs ajal mõned makstud astme töötab see sihtotstarbeline VMs. Rakenduse teenuse lepingute kohta leiate teemast [rakenduse teenuse lepingute ülevaade](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).

5. **Rakenduse teenuse leping** tera, sisestage "NodejsAPIAppPlan" või mõne muu nime kui eelistate.

5. Valige ripploendist **asukoht** asukoht, mis on teile.

    See säte määrab, mis töötavad rakenduse Azure andmekeskuse. Selles õppetükis saate valida mis tahes piirkond ja seda ei tee märgatav erinevus. Kuid tootmise rakenduse, võite soovida oma serveri olla võimalikult lähedal klientidele, mis on juurdepääs seda, et vähendada [latentsus](http://www.bing.com/search?q=web%20latency%20introduction&qs=n&form=QBRE&pq=web%20latency%20introduction&sc=1-24&sp=-1&sk=&cvid=eefff99dfc864d25a75a83740f1e0090).

5. Klõpsake **hinnakirjad taseme > Kuva kõik > F1 tasuta**.

    Selles õpetuses annab tasuta hinnakirjad taseme piisavalt jõudlust.

    ![Valige kiht hinnad](./media/app-service-api-nodejs-api-app/selectfreetier.png)

6. **Rakenduse teenuse leping** tera, klõpsake nuppu **OK**.

7. **API rakenduse** tera, klõpsake nuppu **Loo**.

## <a name="set-up-your-new-api-app-for-git-deployment"></a>Uus rakendus API Git juurutamiseks häälestamine

Vajutamine võtab Git hoidlasse teenuses Azure rakendus kuvatakse teie koodi juurutamiseks API rakenduse. Selle õpetuse jaotises loote mandaadid ja Git hoidla Azure, mida kasutate juurutamiseks.  

1. Kui teie API rakendus on loodud, klõpsake nuppu **rakenduse teenused > {API rakenduse}** videoportaali avalehel. 

    Portaali kuvab labad **API rakenduse** ja **sätted** .

    ![API ettevõtteportaali rakendus ja blade sätted](media/app-service-api-nodejs-api-app/portalapiappblade.png)

1. Liikuge kerides jaotiseni **avaldamise** labale **sätted** ja klõpsake **juurutamise identimisteabe**.
 
3. **Juurutamise identimisteabe määramine** tera, sisestage kasutajanimi ja parool ja klõpsake siis nuppu **Salvesta**.

    Saate kasutada neid mandaate avaldada oma Node.js kood API rakenduse jaoks. 

    ![Juurutamise identimisteave](media/app-service-api-nodejs-api-app/deployment-credentials.png)

1. Tera **sätted** , klõpsake **juurutamise allika > andmeallika valimine > kohaliku Git hoidla**, seejärel klõpsake nuppu **OK**.

    ![Git Repo loomine](media/app-service-api-nodejs-api-app/create-git-repo.png)

1. Kui teie Git hoidla on loodud blade muudatusi teie active juurutuste kuvamiseks. Kuna hoidla on uus, on loendis pole aktiivne juurutuste. 

    ![Pole aktiivne juurutuste](media/app-service-api-nodejs-api-app/no-active-deployments.png)

1. Kopeerige URL Git hoidla. Selleks liikumiseks tera API uus rakendus ja vaadake tera **Essentialsi** osas. Pange tähele jaotise **Essentialsi** **Git klooni URL-i** . Kui viite kursori seda URL-i, kuvatakse ikoon paremale, mis kuvatakse kopeerige URL lõikelauale. Klõpsake selle ikooni kopeerige URL.

    ![Portaalist Git URL-i hankimine](media/app-service-api-nodejs-api-app/get-the-git-url-from-the-portal.png)

    **Märkus**: peate Git klooni URL-i järgmises jaotises seega veenduge salvestamine see kuskil praegu.

Nüüd, kui teil on rakenduse API Git hoidla varundamise, saate juurutada kood API rakenduse hoidlasse push kood. 

## <a name="deploy-your-api-code-to-azure"></a>Azure'i oma API kood juurutamine

Selles jaotises saate luua kohaliku Git hoidla, mis sisaldab teie serveri kood API ja seejärel vajutada oma koodi selle hoidla Azure varem loodud hoidlasse.

1. Kopeerige soovitud `ContactList` kausta asukohta, mille abil saate uue kohaliku Git hoidla. Kui te õpetuse esimene osa, kopeerige `ContactList` kaudu soovitud `start` kausta; muul juhul kopeerida `ContactList` kaudu soovitud `end` kausta.

1. Käsurea tööriist, liikuge uue kausta ja seejärel käivitage järgmine käsk luua uue kohaliku Git hoidla. 

        git init

     ![Uue kohaliku Git Repo](media/app-service-api-nodejs-api-app/new-local-git-repo.png)

1. Käivitage järgmine käsk remote jaoks API rakenduse hoidla Git lisada. 

        git remote add azure YOUR_GIT_CLONE_URL_HERE

    **Märkus**: string "YOUR_GIT_CLONE_URL_HERE" asendamine oma Git klooni URL, mis varem kopeeritud. 

1. Järgmised käsud loomiseks kinnitamine, mis sisaldab kõiki teie koodi käivitada. 

        git add .
        git commit -m "initial revision"

    ![Git Kinnita väljund](media/app-service-api-nodejs-api-app/git-commit-output.png)

1. Käsu push Azure oma kood. Kui teil palutakse parooli, sisestage üks Azure'i portaalis varasemas versioonis loodud.

        git push azure master

    See käivitab API rakenduse juurutamine.  

1. Brauseris liikuda uuesti oma API rakenduse **juurutuste** tera ja näete, et juurutamise esineb. 

    ![Toimu juurutamine](media/app-service-api-nodejs-api-app/deployment-happening.png)

    Samaaegselt peegeldab käsurea liides juurutamise oleku samal ajal, kui see juhtub. 

    ![Sõlm Js juurutamise juhtub](media/app-service-api-nodejs-api-app/node-js-deployment-happening.png)

    Kui juurutamise on lõppenud, kajastab **juurutuste** tera juurutamise API rakenduse muudatuste kood. 

## <a name="test-with-the-api-running-in-azure"></a>Testige API-ga Azure'i töötab?
 
3. Kopeerige **URL-i** oma API rakenduse blade **Essentialsi** osas. 

    ![Juurutamise lõpetatud](media/app-service-api-nodejs-api-app/deployment-completed.png)

1. REST API kliendi postiljon või viiuldaja (või teie veebibrauser), nt pakuvad URL-i kontaktide API kõne, mis on selle `/contacts` API rakenduse lõpp-punkti. URL-i saab`https://{your API app name}.azurewebsites.net/contacts`

    GET-päringu anda selle lõpp-punkti, saate oma API rakenduse JSON väljund.

    ![Postiljon on pihta Api](media/app-service-api-nodejs-api-app/postman-hitting-api.png)

2. Minge veebibrauseris, on `/docs` proovida ärplema UI, nagu see töötab Azure'i lõpp-punkti.

Nüüd, kui teil on pidev kohaletoimetamise võrguühendusega, saate muuta koodi ja juurutamist Azure lihtsalt vajutades võtab oma Azure'i Git hoidla.

## <a name="next-steps"></a>Järgmised sammud

Sel hetkel on edukalt loodud API rakenduse ja juurutatud Node.js API koodi see. Järgmise õpetus näitab [tarbimine API rakenduste JavaScripti klientidega, kasutades CORS](app-service-api-cors-consume-javascript.md)tehke järgmist.
