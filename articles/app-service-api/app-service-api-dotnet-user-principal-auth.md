<properties
    pageTitle="Kasutaja autentimisega API rakendused teenuses Azure rakenduse | Microsoft Azure'i"
    description="Saate teada, kuidas kaitsta API rakenduse teenuses Azure rakendus, mis võimaldab juurdepääsu ainult autenditud kasutajad."
    services="app-service\api"
    documentationCenter=".net"
    authors="tdykstra"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-api"
    ms.workload="na"
    ms.tgt_pltfrm="dotnet"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/30/2016"
    ms.author="rachelap"/>

# <a name="user-authentication-for-api-apps-in-azure-app-service"></a>Kasutaja autentimisega API rakendused teenuses Azure rakendus

## <a name="overview"></a>Ülevaade

Selles artiklis näitab, kuidas kaitsta Azure'i API rakenduse nii, et ainult autenditud kasutajad saavad helistada seda. Artikkel eeldab, et olete läbi lugenud [Azure'i rakendust Service autentimise ülevaade](../app-service/app-service-authentication-overview.md).

Saate teada:

* Kuidas seadistada autentimisteenuse pakkuja, üksikasjadega Azure Active Directory (Azure AD) jaoks.
* Kuidas kasutada kaitstud API rakenduse abil [Active Directory autentimise Raamatukogu (ADAL) JavaScript](https://github.com/AzureAD/azure-activedirectory-library-for-js).

See artikkel on kaheosaline:

* [Teenuses Azure rakenduse kasutaja autentimise konfigureerimine](#authconfig) jaotises selgitatakse üldiselt mis tahes API rakenduse kasutaja autentimise konfigureerimine ja sama kehtib ka rakenduse teenus, sh .net-i, Node.js ja Java ei toeta kõik raamistiku.

* Alustades [jätkuva .NET API rakenduste õpetused](#tutorialstart) jaotise artiklis juhendid teid läbi mõne valimi rakenduse konfigureerimine on .net-i abil tagasi end ja mõne AngularJS ette lõpetada, et ta kasutab Azure Active Directory kasutaja autentimise. 

## <a id="authconfig"></a>Azure'i rakendust Service kasutaja autentimise konfigureerimine

Sellest jaotisest leiate üldised juhised, mis tahes API rakenduste. Tehke loendis .NET valimi rakenduse teatud juhised, minge [jätkuva õpetused .NET töötamise alustamine](#tutorialstart).

1. [Azure'i portaalis](https://portal.azure.com/), liikuge **sätted** tera API rakendus, mida soovite kaitsta, otsige üles jaotis **funktsioonid** ja klõpsake **autentimine / luba**.

    ![Azure'i portaali autentimine/autoriseerimine](./media/app-service-api-dotnet-user-principal-auth/features.png)

3. Klõpsake soovitud **autentimine / autoriseerimine** tera, **klõpsake**nuppu.

4. Valige üks suvanditest ripploendist **toiming taotluse pole autenditud** .

    * Kui soovite, et ainult autenditud kõned saavutamiseks API rakenduse, valige üks suvanditest **... sisse logida** . See suvand võimaldab kaitsta API rakenduse ilma, et see töötab koodi kirjutamata.

    * Kui soovite kõik kõned saavutamiseks API rakenduse, valige **luba taotluse (midagi)**. Selle suvandi abil saate suunata autentimata helistajad valiku autentimisteenuse pakkujate. See suvand on teil autoriseerimine käsitlema koodi kirjutamine.

    Lisateavet leiate teemast [autentimise ja luba API rakendused teenuses Azure rakenduse](app-service-api-authentication.md#multiple-protection-options).

5. Valige üks või mitu **Autentimisteenuse pakkujate**.

    Pildil Valikud, mis nõuavad kõik helistajad autentida Azure AD.
 
    ![Azure'i portaali autentimine/autoriseerimine blade](./media/app-service-api-dotnet-user-principal-auth/authblade.png)

    Autentimisteenuse pakkuja valimisel kuvatakse portaali konfiguratsiooni blade selle pakkuja. 

    Üksikasjalikud juhised, mis selgitavad, kuidas konfigureerida autentimise pakkuja konfiguratsiooni labad, vaadake, [Kuidas konfigureerida rakenduse teenuserakenduse kasutamiseks Azure Active Directory Logi sisse](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md). (Link viib artikli kohta Azure AD, kuid ise artikkel sisaldab vahekaardid, mis lingitakse artikleid autentimisteenuse pakkujate.)

7. Kui olete lõpetanud pakkuja autentimise konfigureerimine tera, klõpsake nuppu **OK**.

7. Klõpsake soovitud **autentimine / autoriseerimine** tera, klõpsake nuppu **Salvesta**.

Kui see on valmis, rakenduse teenuse autendib kõik API kõned enne jõudmist API rakendus. Autentimise toimima sama teenuse rakendus toetab, sh .net-i, Node.js ja Java kõigi keelte jaoks. 

Helistamiseks autenditud API funktsiooni helistaja sisaldab autentimisteenuse pakkuja OAuth 2.0 esitaja luba luba päises HTTP päringuid. Luba saab omandada autentimisteenuse pakkuja SDK abil.

## <a id="tutorialstart"></a>Jätkuva .NET API rakenduste õppetükid

Kui teil on pärast Node.js või Java õpetused API rakendused, jätkake järgmise artikkel, [põhisumma autentimise API rakenduste](app-service-api-dotnet-service-principal-auth.md). 

Kui jälgivad .NET kuueosalisest sarjast API rakendused ja valimi rakenduse juba juurutanud suunatud [esimese](app-service-api-dotnet-get-started.md) ja [teise](app-service-api-cors-consume-javascript.md) õpetused, minge jaotise [autentimise rakendust Service ja Azure AD häälestamine](#azureauth) .

Kui soovite järgige selles õpetuses ilma esimese ja teise õpetused, tehke järgmist, mis selgitab, kuidas alustada rakenduse valimi automaatse protsessi abil.

>[AZURE.NOTE] Järgmised toimingud saaksite sama alguspunkti kui tegite kaks esimest õpetused, ühe erandiga: Visual Studio juba ei tea, milline web appi või API rakendus, mida iga projekti saab juurutatud. Mida tähendab, et ei pea te selle õpetuse täpsed juhised, mis selgitavad, kuidas võtta kasutusele õigete eesmärkidega. Kui te pole ise te ei tea, kuidas seda teha juurutamise juhiseid oma, on parem jälgimiseks kuueosalisest sarjast [esimese õpetuse](app-service-api-dotnet-get-started.md) kui selle automatiseeritud juurutamise protsessi alustada.

1. Veenduge, et teil on kõiki loetletud [esimese õpetuse](app-service-api-dotnet-get-started.md)eeltingimused. Lisaks loetletud eeltingimused, olümpiaandmed autentimine Oletame, et olete töötanud rakenduse teenuse veebirakenduste ja API rakendustega Visual Studio ja Azure portaali.

2. [Loendite hoidla seletusfail näidisfaili](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/readme.md) juurutamine API rakendused ja veebirakenduse nuppu **Deploy Azure** . Kirjutage Azure ressursirühm, mida saab luua, kuna see hilisemaks otsimiseks web app ja API rakenduse nimed.
 
3. Laadige alla või klooni [loendite valimi hoidla](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list) koodi, mida saate töötada kohalikult Visual Studios.

## <a id="azureauth"></a>Häälestada rakendus ja Azure AD autentimine

Nüüd on teil kasutajad autentida nõudmata Azure'i rakendust Service soovitud rakendus. Selle jaotise lisamine autentimine, tehes järgmist:

* Nõua Azure Active Directory (Azure AD) autentimine helistamine keskmisel taseme API rakenduse App teenuse konfigureerimine.
* Luua rakenduse Azure AD.
* Konfigureerige Azure AD Rakenduse saatmiseks pärast sisselogimist esitaja luba AngularJS esiosa. 

Kui teil tekib probleeme kuueosalisest juhiseid järgides, leiate jaotisest [tõrkeotsingu](#troubleshooting) lõpus õpetuse. 
 
### <a name="configure-authentication-for-the-middle-tier-api-app"></a>Teise eesnime taseme API rakenduse autentimise konfigureerimine

1. [Azure'i portaalis](https://portal.azure.com/), liikuge **sätted** tera API rakenduse loodud ToDoListAPI projekti, otsige üles jaotis **funktsioonid** ja klõpsake **autentimine / luba**.

    ![Azure'i portaali autentimine/autoriseerimine](./media/app-service-api-dotnet-user-principal-auth/features.png)

3. Klõpsake soovitud **autentimine / autoriseerimine** tera, **klõpsake**nuppu.

4. Valige ripploendist **toiming taotluse pole autenditud** **Azure Active Directory sisse logida**.

    See suvand tagab, et mitte unauathenticated taotlusi jõuab API rakendus. 

5. Klõpsake jaotises **Autentimisteenuse pakkujate** **Azure Active Directory**.

    ![Azure'i portaali autentimine/autoriseerimine blade](./media/app-service-api-dotnet-user-principal-auth/authblade.png)

6. Tera **Azure Active Directory sätted** , klõpsake nuppu **Express**

    ![Azure'i portaali autentimine/autoriseerimine kiire võimalus](./media/app-service-api-dotnet-user-principal-auth/aadsettings.png)

    **Kiire** võimalus, rakenduse teenuse automaatselt luua Azure AD Rakenduse teie Azure AD [rentniku](https://msdn.microsoft.com/en-us/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant). 

    Teil pole loomiseks rentniku, kuna iga Azure'i konto automaatselt on olemas.

7. Klõpsake jaotises **juhtimist**, kui see pole juba valitud, klõpsake nuppu **Loo uus AD rakendus** ja märkate väärtust, mis on **Rakenduse loomine** tekstiväljale; Saate vaadata selle AAD rakenduse Azure klassikaline portaalis hiljem üles.

    ![Azure'i portaalis Azure AD sätted](./media/app-service-api-dotnet-user-principal-auth/aadsettings2.png)

    Azure'i loob automaatselt rakenduse Azure AD oma Azure AD rentniku näidatud nimega. Vaikimisi on Azure AD Rakenduse nimega sama API rakendus. Soovi korral saate sisestada mõni muu nimi.
 
7. Klõpsake nuppu **OK**.

7. Klõpsake soovitud **autentimine / autoriseerimine** tera, klõpsake nuppu **Salvesta**.

    ![Klõpsake nuppu Salvesta](./media/app-service-api-dotnet-user-principal-auth/authsave.png)

Nüüd saate ainult teie Azure AD rentniku kasutajate helistada API rakendus.

### <a name="optional-test-the-api-app"></a>Valikuline: API rakenduse testimine

1. Minge brauseris URL-i API rakenduse: Azure'i portaalis labale **API rakenduse** klõpsake lingi all **URL-i**.  

    Teid suunatakse ümber login kuvale Kuna autentimata taotlused saavutamiseks API rakendus pole lubatud.

    Kui teie brauser läheb "edukalt loodud" leht, brauseri võib juba loginud sisse – sel juhul InPrivate või inkognito akna avamine ja minge API rakenduse URL.

2. Logige sisse oma Azure AD rentniku kasutaja mandaadi abil.

    Kui olete sisse logitud, kuvatakse "edukalt loodud" leht brauseris.

9. Sulgege brauser.

### <a name="configure-the-azure-ad-application"></a>Azure AD rakenduse konfigureerimine

Azure'i AD-autentimine konfigureerimisel loodud rakendust Service rakenduse Azure AD. Vaikimisi uue Azure AD rakendus on konfigureeritud esitaja luba anda API rakenduse URL-i. Selles jaotises saate konfigureerida Azure AD taotluse esitaja luba AngularJS esiosa juurde asemel rakendusse keskmisel taseme API. AngularJS esiosa saadab luba API rakendus, kui see nõuab API rakendus.

>[AZURE.NOTE] Hoida protsessi võimalikult lihtne, see õpetus kasutab ühe Azure AD nii esiosa ja keskel esimese API rakenduse. Teine võimalus on kaks Azure AD rakendust kasutada. Sel juhul saate oleks luba ees end Azure AD Rakenduse keskmisel taseme Azure AD Rakenduse helistada. See mitme rakenduse lähenemine annaks täpsema juhtida iga taseme õigusi.

11. [Azure'i klassikaline portaali](https://manage.windowsazure.com/), minge **Azure Active Directory**.

    Teil on kasutada klassikaline portaali, kuna teatud Azure Active Directory sätted, mida teil on vaja juurdepääsu ei ole veel praeguse Azure'i portaalis.

12. Klõpsake vahekaardil **Directory** teie AAD rentniku.

    ![Azure AD klassikaline portaalis](./media/app-service-api-dotnet-user-principal-auth/selecttenant.png)

14. Klõpsake **rakendused > Minu ettevõte kuulub rakenduste**, ja seejärel klõpsake märkeruutu.

    Teil võib ka värskendage lehte uue rakenduse kuvamiseks.

15. Rakenduste loendis klõpsake seda, mida teie jaoks loodud Azure, kui märkisite autentimise oma API rakenduse nimi.

    ![Azure'i AD Rakendused vahekaardil](./media/app-service-api-dotnet-user-principal-auth/appstab.png)

16. Klõpsake nuppu **Konfigureeri**.

    ![Azure'i AD konfigureerimine menüü](./media/app-service-api-dotnet-user-principal-auth/configure.png)

17. **Sisselogimise URL-i** määramine teie AngularJS web Appis, pole lõpunullid kaldkriips URL-i.

    Näide: https://todolistangular.azurewebsites.net

    ![Sisselogimise URL-i](./media/app-service-api-dotnet-user-principal-auth/signonurlazure.png)

17. **Vastus URL-i** määramine teie web Appis, pole lõpunullid kaldkriips URL-i.

    Näide: https://todolistsangular.azurewebsites.net

16. Klõpsake nuppu **Salvesta**.

    ![Vastus URL-i](./media/app-service-api-dotnet-user-principal-auth/replyurlazure.png)

15. Klõpsake lehe allosas nuppu **haldamine manifesti > allalaadimine manifesti**.

    ![Manifesti allalaadimine](./media/app-service-api-dotnet-user-principal-auth/downloadmanifest.png)

17. Faili asukohta, kus saate redigeerida selle alla laadida.

16. Allalaaditud Avaldamisfail, otsige soovitud `oauth2AllowImplicitFlow` atribuut. Selle atribuudi väärtust muuta `false` et `true`, ja seejärel salvestage fail.

    See säte on vaja Accessi rakendusest JavaScripti ühe lehe. See võimaldab Oauth 2.0 esitaja luba puhul tagastatakse URL-i fragment.

16. Klõpsake **haldamine manifesti > Laadi manifesti**, ja eelmises etapis värskendatud faili üles laadida.

    ![Manifesti üleslaadimine](./media/app-service-api-dotnet-user-principal-auth/uploadmanifest.png)

17. Kopeerige **Kliendiid** väärtus ja salvestage see kuskil saate selle hiljem.

## <a name="configure-the-todolistangular-project-to-use-authentication"></a>ToDoListAngular projekti kasutada autentimise konfigureerimine

Selles jaotises saate muuta AngularJS ots nii, et see kasutaks Active Directory autentimise Raamatukogu (ADAL) jaoks JS omandada esitaja luba sisse logitud kasutajale: Azure'i AD. Kood sisaldab luba HTTP-päringud saadetakse teise eesnime taseme, nagu on näidatud järgmisel joonisel. 

![Kasutaja autentimise skeem](./media/app-service-api-dotnet-user-principal-auth/appdiagram.png)

Projekti ToDoListAngular failid järgmisi muudatusi teha.

1. Avage fail *index.html* .

2. Kommenteerige välja read, mis viitavad Active Directory autentimise Raamatukogu (ADAL) JS skripte.

        <script src="app/scripts/adal.js"></script>
        <script src="app/scripts/adal-angular.js"></script>

1. Avage fail *app/scripts/app.js* .

2. Välja kood märgitud "ilma autentimine" tekstiploki kommentaar ja kommenteerige välja märgitud autentimiseks"ja" kood plokk.

    Selle muudatuse viitab ADAL JS autentimisteenuse ja pakub konfiguratsiooni väärtusi. Järgmistes juhistes seate oma API rakenduse ja Azure AD rakenduse konfigureerimise väärtused.

8. Koodi, mis määrab selle `endpoints` muutuja, määrama API URL-i URL-i API rakenduse ToDoListAPI projekti loodud ja määratud Azure AD rakenduse ID Azure klassikaline portaalist kopeeritud Kliendiid.

    Kood on nüüd sarnane järgmises näites.

        var endpoints = {
            "https://todolistapi0121.azurewebsites.net/": "1cf55bc9-9ed8-4df31cf55bc9-9ed8-4df3"
        };

9. Klõpsake kõne edastamiseks `adalProvider.init`, seadmine `tenant` teie rentniku nimi ja `clientId` sama väärtusega, mida kasutasite eelmises etapis.

    Kood on nüüd sarnane järgmises näites.

        adalProvider.init(
            {
                instance: 'https://login.microsoftonline.com/', 
                tenant: 'contoso.onmicrosoft.com',
                clientId: '1cf55bc9-9ed8-4df31cf55bc9-9ed8-4df3',
                extraQueryParameter: 'nux=1',
                endpoints: endpoints
            },
            $httpProvider
            );

    Neid muutusi `app.js` määrata helistaja koodi ja nimega API on sama Azure AD Rakenduse.

1. Avage fail *app/scripts/homeCtrl.js* .

2. Välja kood märgitud "ilma autentimine" tekstiploki kommentaar ja kommenteerige välja märgitud autentimiseks"ja" kood plokk.

1. Avage fail *app/scripts/indexCtrl.js* .

2. Välja kood märgitud "ilma autentimine" tekstiploki kommentaar ja kommenteerige välja märgitud autentimiseks"ja" kood plokk.

### <a name="deploy-the-todolistangular-project-to-azure"></a>Azure'i ToDoListAngular projekti juurutamine

8. Paremklõpsake ToDoListAngular projekti **Solution Exploreris**, ja seejärel klõpsake nuppu **Avalda**.

9. Klõpsake nuppu **Avalda**.

    Visual Studio kasutab projekti ja avatakse brauseris web appi base URL. See näitab 403 viga lehe, mis on tavaline katse minge veebi-API base URL brauseri kaudu.

    Teil on endiselt keskmisel taseme API rakenduse muudatuste tegemine, enne kui saate rakenduse testida.

10. Sulgege brauser.

## <a name="configure-the-todolistapi-project-to-use-authentication"></a>ToDoListAPI projekti kasutada autentimise konfigureerimine

Praegu saadab ToDoListAPI projekti "*" nimega soovitud `owner` ToDoListDataAPI väärtuse. Selles jaotises saate muuta koodi saata sisselogitud kasutaja ID-d.

Projectis ToDoListAPI teha järgmisi muudatusi.

1. Avage fail *Controllers/ToDoListController.cs* ja kommenteerige välja rida iga toimingu meetod, mis määrab `owner` Azure AD `NameIdentifier` taotlemine väärtus. Näiteks:

        owner = ((ClaimsIdentity)User.Identity).FindFirst(ClaimTypes.NameIdentifier).Value;

    **Tähtis**: ei kommentaari kood on `ToDoListDataAPI` meetod; saab teha mis hiljem juhend on teenuse põhisumma autentimist.

### <a name="deploy-the-todolistapi-project-to-azure"></a>Azure'i ToDoListAPI projekti juurutamine

8. Paremklõpsake ToDoListAPI projekti **Solution Exploreris**, ja seejärel klõpsake nuppu **Avalda**.

9. Klõpsake nuppu **Avalda**.

    Visual Studio kasutab projekti ja avatakse brauseris API rakenduse base URL.

10. Sulgege brauser.

### <a name="test-the-application"></a>Rakenduse testimine

9. Avage URL-i veebirakenduse, **HTTPS, mitte HTTP kaudu**.

8. Klõpsake vahekaarti **Loendite** .

    Teil palutakse sisse logida.

9. Logige sisse oma AAD rentniku kasutaja mandaat.

10. Kuvatakse leht **Loendite** .

    ![Lehe Ülesandeloend](./media/app-service-api-dotnet-user-principal-auth/webappindex.png)

    Ei ole ülesande üksused kuvatakse, kuna seni nad kõik olnud omanik "*". Nüüd taotleb teise eesnime esimese taseme üksuste sisse logitud kasutajale ja pole veel loodud.

11. Lisage uus ülesandeloendi üksuste kinnitamaks, et rakendus töötab.

12. Mõnes muus brauseriaknas, avage rakenduse ToDoListDataAPI API ärplema UI URL ja klõpsake nuppu **ToDoList > saada**. Sisestage tärn jaoks soovitud `owner` parameeter ja klõpsake seejärel nuppu **proovida**.

    Vastus näitab, et uute ülesandeloendi üksuste tegelik Azure AD kasutaja ID atribuudi omanik.

    ![Omaniku ID JSON vastus](./media/app-service-api-dotnet-user-principal-auth/todolistapiauth.png)


## <a name="building-the-projects-from-scratch"></a>Koostamise projektide algusest peale

Kaks Web API projekte on loodud **Azure'i API rakenduse** project malli abil ning asendades vaikimisi väärtused kontrolleril ToDoList kontrolleril. 

Leiate teavet selle kohta, kuidas luua rakenduse AngularJS ühe lehe Web API 2 tagasi lõpuks, [käed kohta Lab: koostada ühe lehe rakenduse (SPA) ASP.net-i veebi-API ja Angular.js](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs). Teavet selle kohta, kuidas lisada Azure AD autentimise koodi, leiate järgmistest teemadest.

* [Turvaliseks AngularJS ühe lehe rakenduste Azure AD](../active-directory/active-directory-devquickstarts-angular.md).
* [ADAL JS v1 tutvustus](http://www.cloudidentity.com/blog/2015/02/19/introducing-adal-js-v1/)

## <a name="troubleshooting"></a>Tõrkeotsing

[AZURE.INCLUDE [troubleshooting](../../includes/app-service-api-auth-troubleshooting.md)]

* Veenduge, et Ärge ajage ToDoListAPI (keskmisel taseme) ja ToDoListDataAPI (andmete taseme). Näiteks, veenduge, et autentimise lisatakse teise eesnime esimese taseme API rakendus, mitte andmete taseme. 
* Veenduge, et et AngularJS lähtekoodi viitab teise eesnime esimese taseme API rakenduse URL (ToDoListAPI, mitte ToDoListDataAPI) ja õige Azure AD kliendi ID-ga. 

## <a name="next-steps"></a>Järgmised sammud

Selles õpetuses soovite õpitut rakenduse autentimise API rakenduse kasutamise kohta ja kuidas helistada API rakenduse abil ADAL JS teek. Järgmise õppeteema saate teada, kuidas [rakenduse API - teenusest stsenaariumid turvaline juurdepääs](app-service-api-dotnet-service-principal-auth.md).

