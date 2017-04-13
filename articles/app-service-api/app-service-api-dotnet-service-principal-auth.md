<properties
    pageTitle="Peamine autentimise API rakendused teenuses Azure rakendus | Microsoft Azure'i"
    description="Saate teada, kuidas kaitsta API rakenduse-teenusest stsenaariumid teenuses Azure rakendus."
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

# <a name="service-principal-authentication-for-api-apps-in-azure-app-service"></a>Peamine autentimise API rakendused teenuses Azure rakendus

## <a name="overview"></a>Ülevaade

Selles artiklis selgitatakse, kuidas kasutada rakenduse autentimise *sisemise* juurdepääsuks API rakendused. On sisemine stsenaarium on, kus teil on API app, mida soovite tarbitavad vaid oma rakenduse koodi. Nimega API rakenduse kaitsmiseks kasutada Azure AD on soovitatav viis rakenduse teenuse sel juhul rakendada. Kutsuge kaitstud API rakenduse esitaja luba, mille saate Azure AD esitada rakenduse identiteedi (põhisumma teenus) mandaadi abil. Võimalusi kasutades Azure AD, [Azure'i rakendust Service autentimise ülevaade](../app-service/app-service-authentication-overview.md#service-to-service-authentication)jaotisest **teenusest autentimist** .

Selles artiklis õpite:

* Kuidas kasutada Azure Active Directory (Azure AD) autentimata Accessi rakendus API kaitsta.
* Kuidas kasutada Azure AD teenuse põhisumma (rakenduse identiteet) identimisteavet kasutades kaitstud API rakenduse API rakenduse, veebirakenduse, või mobiilirakenduse kaudu. Loogika rakenduse kaudu peab olema kohta leiate artiklist [kaudu oma kohandatud API majutatud rakenduse teenuse loogika rakendustega](../app-service-logic/app-service-logic-custom-hosted-api.md).
* Kuidas tagada, et kaitstud API rakendus ei saa helistada, brauseri kaudu sisse loginud kasutajad.
* Kuidas veenduda, et kaitstud API rakenduse saab kutsuda ainult teatud Azure AD teenuse põhimakse.

See artikkel on kaheosaline:

* [Teenuses Azure rakenduse põhisumma autentimise konfigureerimine](#authconfig) jaotis selgitab üldiselt mõne API rakenduse autentimise konfigureerimine ja kuidas kaitstud API rakenduse kasutamine. See jaotis kehtib võrdselt kõigi raamistiku rakenduse teenus, sh .net-i, Node.js ja Java ei toeta.

* Alustades [jätkuva töötamise alustamine .NET õpetused](#tutorialstart) jaotis, õpetuse juhatab teid läbi mõne "sisemine juurdepääsu" stsenaarium .NET valimi rakenduse töötavad rakenduse teenuse konfigureerimine. 

## <a id="authconfig"></a>Azure'i rakendust Service põhisumma autentimise konfigureerimine

Sellest jaotisest leiate üldised juhised, mis tahes API rakenduste. Tehke loendis .NET valimi rakenduse teatud juhised, minge [jätkuva .NET API rakenduste kuueosalisest sarjast](#tutorialstart).

1. [Azure'i portaalis](https://portal.azure.com/), liikuge API rakendus, mida soovite kaitsta, ja seejärel otsige üles jaotis **funktsioonid** ja klõpsake nuppu **sätted** tera **autentimine / luba**.

    ![Autentimist/autoriseerimine Azure'i portaalis](./media/app-service-api-dotnet-user-principal-auth/features.png)

3. Klõpsake soovitud **autentimine / autoriseerimine** tera, **klõpsake**nuppu.

4. Valige ripploendist **toiming taotluse pole autenditud** **Azure Active Directory sisse logida** .

5. Valige jaotises **Autentimisteenuse pakkujate** **Azure Active Directory**.

    ![Autentimist/autoriseerimine blade Azure'i portaalis](./media/app-service-api-dotnet-user-principal-auth/authblade.png)

6. **Azure Active Directory sätted** tera luua uus konfigureerimine Azure AD Rakenduse või kasutage olemasolevat Azure AD rakendust kui teil juba on üks, mida soovite kasutada.

    Sisemise stsenaariumid hõlmavad tavaliselt helistamiseks API rakenduse API rakendus. Saate kasutada eraldi Azure AD rakenduste iga API rakenduse või ainult ühe Azure AD Rakenduse.

    Üksikasjalikud juhised selle tera, vaadake, [Kuidas konfigureerida rakenduse teenuserakenduse kasutamiseks Azure Active Directory Logi sisse](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).

7. Kui olete lõpetanud pakkuja autentimise konfigureerimine tera, klõpsake nuppu **OK**.

7. Klõpsake soovitud **autentimine / autoriseerimine** tera, klõpsake nuppu **Salvesta**.

    ![Klõpsake nuppu Salvesta](./media/app-service-api-dotnet-service-principal-auth/authsave.png)

Kui see on valmis, rakenduse teenus ainult võimaldab taotlusi helistajate rakenduses on konfigureeritud Azure AD rentniku. Autentimist või luba koodi pole vajalik kaitstud API rakendus. Luba esitaja edastatakse API rakenduse koos sageli kasutatud taotluste HTTP päised ja loete seda teavet kinnitada, et taotlused on teatud helistaja, nt teenuse põhilise-koodi.

See autentimise funktsioon töötab samal viisil teenuse rakendus toetab, sh .net-i, Node.js ja Java kõigi keelte jaoks. 

#### <a name="how-to-consume-the-protected-api-app"></a>Kuidas kaitstud API rakenduse kasutamine

Funktsiooni helistaja peab andma on Azure AD esitaja luba API kõned. Teenuse põhisumma mandaadiga esitaja märgiks saamiseks kasutab funktsiooni helistaja Active Directory Authentication Library (ADAL [.net-i](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory), [Node.js](https://github.com/AzureAD/azure-activedirectory-library-for-nodejs)või [Java](https://github.com/AzureAD/azure-activedirectory-library-for-java)). Märgiks saamiseks kood, mis nõuab ADAL pakub ADAL järgmine teave:

* Teie Azure AD rentniku nimi.
* Kliendi ID ja Azure AD Rakenduse seotud helistaja kliendi salajane (rakenduse võti).
* Kliendi ID kaitstud API rakendusega seotud Azure AD Rakenduse. (Kui kasutatakse ainult ühe Azure AD Rakenduse, see on sama kliendi ID funktsiooni helistaja üks.)

Need väärtused on saadaval [Azure klassikaline portaalis](https://manage.windowsazure.com/)Azure AD lehtedel.

Kui luba omandanud helistaja sisaldab see HTTP päringuid päises autoriseerimine.  Teenuse rakendus kinnitatakse luba ja võimaldab taotlusi kaitstud API rakenduse saavutamiseks.

#### <a name="how-to-protect-the-api-app-from-access-by-users-in-the-same-tenant"></a>Kuidas kaitsta API rakenduse sama rentniku kasutajate juurdepääs

Kaitstud API rakenduse kehtivad esitaja sõned sama rentniku kasutajate jaoks.  Kui soovite tagada, et teenuse põhilise saate helistada kaitstud API rakenduse, lisada koodi kaitstud API rakenduse valideerimiseks märgiks järgmised nõuded:

* `appid`peaks olema Azure AD rakendus, mis on seotud helistaja kliendi ID-d. 
* `oid`(`objectidentifier`) peaks olema helistaja ID teenus põhisumma. 

Rakenduse teenus pakub soovitud `objectidentifier` taotlemine päise X-MS-kliendi-põhisumma-ID.

### <a name="how-to-protect-the-api-app-from-browser-access"></a>Veebibrauseri juurdepääs API rakenduse kaitsmine

Kui ei kinnitamist taotluste kaitstud API rakenduse koodi ja kasutate eraldi Azure AD Rakenduse kaitstud API rakenduse, veenduge, et Azure AD Rakenduse vastus URL-i on pole sama, mis API rakenduse base URL-i. Kui vastus URL-i viitab otse kaitstud API rakendus, võib kasutaja sama Azure AD rentniku sirvige API rakenduse, logige sisse ja edukalt kõne API.

## <a id="tutorialstart"></a>.Net-i API rakenduste kuueosalisest sarjast jätkuva

Kui teil on pärast Node.js või Java kuueosalisest sarjast API rakendused, minge jaotise [järgmised toimingud](#next-steps) . 

Käesoleva artikli ülejäänud endiselt .NET API rakenduste kuueosalisest sarjast ja eeldab, et [Kasutaja autentimise õpetuse](app-service-api-dotnet-user-principal-auth.md) lõpetanud ja on valimi rakendus töötab Azure kasutaja autentimine on lubatud.

## <a name="set-up-authentication-in-azure"></a>Autentimise Azure häälestamine

Selles jaotises saate konfigureerida rakendus teenuse nii, et see lubab andmete taseme API rakenduse saavutamiseks ainult HTTP-päringud on need, mis on lubatud Azure AD esitaja sõned. 

Saate konfigureerida järgmises jaotises keskmisel taseme API rakenduse saata rakenduse mandaat Azure AD, naasta märgiks esitaja ja andmete taseme API rakenduse esitaja luba saata. Selle protsessi on kujutatud skeemi.

![Teenuse autentimise skeem](./media/app-service-api-dotnet-service-principal-auth/appdiagram.png)

Kui teil tekib probleeme kuueosalisest juhiseid järgides, leiate jaotisest [tõrkeotsingu](#troubleshooting) lõpus õpetuse. 

1. [Azure portaali](https://portal.azure.com/)liikuge **sätted** tera API rakenduse loodud ToDoListDataAPI (andmete taseme) API rakenduse ja seejärel klõpsake nuppu **sätted**.

2. Leidke jaotis **funktsioonid** labale **sätted** ja klõpsake **autentimine / luba**.

    ![Autentimist/autoriseerimine Azure'i portaalis](./media/app-service-api-dotnet-user-principal-auth/features.png)

3. Klõpsake soovitud **autentimine / autoriseerimine** tera, **klõpsake**nuppu.

4. Valige ripploendist **toiming taotluse pole autenditud** **Azure Active Directory sisse logida**.

    See on säte, mis põhjustab rakenduse teenuse tagamaks, et ainult autenditud taotlusi REACHi API rakendus. Taotlusi, mis on kehtiv esitaja sõned rakenduse teenuse edastab sõned mööda API rakendus ja kuvab HTTP päised levinumaid nõuete isikuandmeid, mida soovite oma koodi kergemini kättesaadavaks teha.

5. Klõpsake jaotises **Autentimisteenuse pakkujate** **Azure Active Directory**.

    ![Autentimist/autoriseerimine blade Azure'i portaalis](./media/app-service-api-dotnet-user-principal-auth/authblade.png)

6. Labale **Azure Active Directory sätted** nuppu **Express**.

    **Kiire** valik Azure automaatselt luua AAD rakenduse teie Azure AD [rentniku](https://msdn.microsoft.com/en-us/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant). 

    Teil pole loomiseks rentniku, kuna iga Azure'i konto automaatselt on olemas.

7. Klõpsake jaotises **juhtimist**, **Looge uus AD rakendus** , kui see pole juba valitud.

    Portaali ühendatud **Rakenduse loomine** väljale vaikeväärtus abil. Vaikimisi on Azure AD Rakenduse nimega sama API rakendus. Soovi korral saate sisestada mõni muu nimi.
    
    ![Azure'i AD-sätted](./media/app-service-api-dotnet-service-principal-auth/aadsettings.png)

    **Märkus**: teise võimalusena võite kasutada ühe Azure AD taotlus helistaja API rakenduse nii kaitstud API rakendus. Kui valisite selle asemel, te ei pea suvand **Loo uus AD rakendus** siin kuna juba loodud rakenduse Azure AD kasutaja autentimise õpetuses. Selles õpetuses jaoks peate kasutama Azure AD rakenduste helistaja API rakendus ja kaitstud API rakenduse jaoks eraldi.

8. Märkige soovitud väärtus, mis on **Rakenduse loomine** väljale; Saate vaadata selle AAD rakenduse Azure klassikaline portaalis hiljem üles.

7. Klõpsake nuppu **OK**.

10. Klõpsake soovitud **autentimine / autoriseerimine** tera, klõpsake nuppu **Salvesta**.

    ![Klõpsake nuppu Salvesta](./media/app-service-api-dotnet-service-principal-auth/saveauth.png)

    Rakenduse teenus loob rakenduse Azure Active Directory **sisselogimise URL-i** ja **Vastus URL-i** automaatselt määratud URL-i API rakenduse. Viimane väärtus võimaldab kasutajatel teie AAD rentnikus sisse logida ja juurdepääs API rakendus.

### <a name="verify-that-the-api-app-is-protected"></a>Veenduge, et API rakendus on kaitstud

1. Minge brauseris URL-i API rakenduse: Azure'i portaalis labale **API rakenduse** klõpsake lingi all **URL-i**. 

    Teid suunatakse ümber login kuvale Kuna autentimata taotlused saavutamiseks API rakendus pole lubatud. 

    Brauseri ärplema UI, kui brauseri võib juba loginud sisse – sel juhul InPrivate või inkognito akna avamine ja minge ärplema UI URL.

18. Logige sisse oma AAD rentniku kasutaja mandaat.

    Kui olete sisse logitud, kuvatakse "edukalt loodud" leht brauseris.

## <a name="configure-the-todolistapi-project-to-acquire-and-send-the-azure-ad-token"></a>ToDoListAPI projekti hankimine ja Azure AD luba saata konfigureerimine

Selles jaotises saate teha järgmisi toiminguid:

* Teise eesnime, teise API rakendus, mis kasutab Azure AD Rakenduse mandaati, et hankida märgiks ja saatke see HTTP-päringud andmete taseme API rakendusse lisada koodi.
* Teil on vaja identimisteabe toomine Azure AD.
* Azure'i rakendust Service käitusaja keskkonna sätteid keskmisel astme API rakenduse identimisteabe sisestamine. 

### <a name="configure-the-todolistapi-project-to-acquire-and-send-the-azure-ad-token"></a>ToDoListAPI projekti hankimine ja Azure AD luba saata konfigureerimine

Visual Studio Projectis ToDoListAPI teha järgmisi muudatusi.

1. Kommenteerige välja failis *ServicePrincipal.cs* koodi osa.

    See on koodi, mis kasutab ADAL .net-i hankimine Azure AD esitaja luba.  Kasutab mitu konfiguratsiooni väärtused, mida saate seada Azure käitusaja keskkonnas hiljem. Siin on kood: 

        public static class ServicePrincipal
        {
            static string authority = ConfigurationManager.AppSettings["ida:Authority"];
            static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
            static string clientSecret = ConfigurationManager.AppSettings["ida:ClientSecret"];
            static string resource = ConfigurationManager.AppSettings["ida:Resource"];
        
            public static AuthenticationResult GetS2SAccessTokenForProdMSA()
            {
                return GetS2SAccessToken(authority, resource, clientId, clientSecret);
            }
        
            static AuthenticationResult GetS2SAccessToken(string authority, string resource, string clientId, string clientSecret)
            {
                var clientCredential = new ClientCredential(clientId, clientSecret);
                AuthenticationContext context = new AuthenticationContext(authority, false);
                AuthenticationResult authenticationResult = context.AcquireToken(
                    resource,
                    clientCredential);
                return authenticationResult;
            }
        }

    **Märkus:** Järgmine kood nõuab selle ADAL .NET Nugeti pakett (Microsoft.IdentityModel.Clients.ActiveDirectory), mis on juba installitud projekti. Kui teil on selle projekti loomine algusest peale, mida oleks paketi installimist. See pakett API rakenduse uue projekti malli automaatselt pole installitud.

2. Kommenteerige *Kontrollerid/ToDoListController*, klõpsake välja koodi selle `NewDataAPIClient` meetod, mis lisab luba HTTP taotleb päises autoriseerimine.

        client.HttpClient.DefaultRequestHeaders.Authorization =
            new AuthenticationHeaderValue("Bearer", ServicePrincipal.GetS2SAccessTokenForProdMSA().AccessToken);

3. ToDoListAPI projekti juurutamine. (Projekt paremklõpsake ja klõpsake valikut **Avalda > Avalda**.)

    Visual Studio kasutab projekti ja avatakse brauseris web appi base URL. See näitab 403 viga lehe, mis on tavaline katse minge veebi-API base URL brauseri kaudu.

4. Sulgege brauser.

### <a name="get-azure-ad-configuration-values"></a>Saada Azure AD väärtuste määramine

11. [Azure'i klassikaline portaali](https://manage.windowsazure.com/), minge **Azure Active Directory**.

12. Klõpsake vahekaardil **Directory** teie AAD rentniku.

14. Klõpsake **rakendused > Minu ettevõte kuulub rakenduste**, ja seejärel klõpsake märkeruutu.

15. Rakenduste loendis klõpsake seda, mida teie jaoks loodud Azure, kui märkisite autentimine ToDoListDataAPI (andmete taseme) API rakenduse nimi.

16. Klõpsake vahekaarti **konfigureerimine** .

5. Kopeerige **Kliendiid** väärtus ja salvestage see kuskil saate selle hiljem. 

8. Azure'i klassikaline portaalis minge tagasi **minu ettevõtte kuulub rakenduste**loendit ja nuppu AAD rakendus, teise eesnime esimese taseme ToDoListAPI API rakenduse (üks eelmises õppeteemas loodud, ei loodud selles õpetuses üks) loodud.

16. Klõpsake vahekaarti **konfigureerimine** .

5. Kopeerige **Kliendiid** väärtus ja salvestage see kuskil saate selle hiljem.

6. Valige jaotises **klahvid**, **1 aasta** ripploendist **Valige kestus** .

6. Klõpsake nuppu **Salvesta**.

    ![Luua rakenduse võti](./media/app-service-api-dotnet-service-principal-auth/genkey.png)

7. Kopeerige võtmeväärtuse ja salvestage see kuskil saate selle hiljem.

    ![Kopeerimine uue rakenduse tootenumbri](./media/app-service-api-dotnet-service-principal-auth/genkeycopy.png)

### <a name="configure-azure-ad-settings-in-the-middle-tier-api-apps-runtime-environment"></a>Teise eesnime esimese taseme API rakenduse käitusaja keskkonnas Azure AD sätete konfigureerimine

1. [Azure portaali](https://portal.azure.com/)ja seejärel liikuge **API rakenduse** tera API rakenduse, mis hostib TodoListAPI (keskmisel taseme) projekt.

2. Klõpsake **Sätted > rakenduse sätted**.

3. Lisage jaotises **rakenduse sätted** järgmised võtmed ja väärtused.

  	| **Klahv** | Ida: asutus |
  	|---|---|
  	| **Väärtus** | https://login.microsoftonline.com/ {oma Azure AD rentniku nimi} |
  	| **Näide** | https://login.microsoftonline.com/contoso.onmicrosoft.com |

  	| **Klahv** | Ida: ClientId |
  	|---|---|
  	| **Väärtus** | Kliendi ID helistaja rakendus (keskmisel taseme - ToDoListAPI) |
  	| **Näide** | 960adec2-b74a-484a-960adec2-b74a-484a |

  	| **Klahv** | Ida: ClientSecret |
  	|---|---|
  	| **Väärtus** | Rakenduse klahvi helistaja taotluse (keskmisel taseme - ToDoListAPI) |
  	| **Näide** | e65e8fc9-5f6b-48e8-e65e8fc9-5f6b-48e8 |

  	| **Klahv** | Ida: ressurss |
  	|---|---|
  	| **Väärtus** | Kliendi ID nimega rakendus (andmete kiht - ToDoListDataAPI) |
  	| **Näide** | e65e8fc9-5f6b-48e8-e65e8fc9-5f6b-48e8 |

    **Märkus**: jaoks `ida:Resource`, veenduge, et kasutate nimega rakenduse **Kliendi ID** "ja" pole selle **Rakenduse ID URI**.

    `ida:ClientId`ja `ida:Resource` erinevad väärtused selles õpetuses, sest kasutate eraldi Azure AD applicaations teise eesnime taseme ja andmete taseme. Kui kasutate ühte Azure AD Rakenduse helistaja API rakendus ja kaitstud API rakenduse, tuleks kasutada sama väärtuse nii `ida:ClientId` ja `ida:Resource`.

    Kood kasutab ConfigurationManager saada need väärtused, nii et nad võivad salvestatud projekti fail või Azure käitusaja keskkonnas. ASP.net-i rakenduse töötamise Azure'i rakendust Service keskkonna sätted alistada automaatselt Web.config sätted. Keskkonna sätteid on üldiselt [turvalisem viis võrreldes faili Web.config tundliku teabe talletamiseks](http://www.asp.net/identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure).

6. Klõpsake nuppu **Salvesta**.

    ![Klõpsake nuppu Salvesta](./media/app-service-api-dotnet-service-principal-auth/appsettings.png)

### <a name="test-the-application"></a>Rakenduse testimine

1. Minge brauseris HTTPS URL-i veebirakenduse AngularJS esiosa.

2. Klõpsake vahekaarti **Loendite** ja logige sisse oma Azure AD rentniku kasutaja mandaat. 

4. Lisage ülesandeloendi üksuste kinnitamaks, et rakendus töötab.

    ![Lehe Ülesandeloend](./media/app-service-api-dotnet-service-principal-auth/mvchome.png)

    Kui rakendus ei tööta oodatud viisil, kontrollige, kas kõik teie sisestatud Azure'i portaalis sätted. Kui kõik sätted olevat õige, vaadake [tõrkeotsingu](#troubleshooting) jaotise hiljem selles õpetuses.

## <a name="protect-the-api-app-from-browser-access"></a>Veebibrauseri juurdepääs API rakenduse kaitsmine

Selles õppeteemas loodud eraldi Azure AD Rakenduse ToDoListDataAPI (andmete taseme) API rakenduse. Nagu näete, kui rakenduse teenus loob rakenduse AAD, konfigureerib AAD rakenduse viisil, mis võimaldab kasutajatel minge brauseris API rakenduse URL-i ja logige sisse. Mida tähendab, et on võimalik, et teie Azure AD rentniku, mitte ainult teenuse põhisumma; API juurdepääsu lõppkasutaja. 

Kui soovite vältida veebibrauseri juurdepääs ilma koodi kirjutamata kaitstud API rakenduses, saate muuta **Vastus URL-i** AAD rakenduse, et see erineb API rakenduse base URL-i. 

### <a name="disable-browser-access"></a>Veebibrauseri juurdepääs keelamine

1. Klassikaline portaali **konfigureerimine** vahekaardil AAD rakendus, mis loodi jaoks soovitud TodoListService muuta nii, et see on kehtiv URL, kuid API rakenduse URL-i **Vastus URL-i** välja väärtus.
 
2. Klõpsake nuppu **Salvesta**.

### <a name="verify-browser-access-no-longer-works"></a>Veebibrauseri juurdepääs ei tööta enam kinnitamine

Varasemas versioonis saate kontrollida, et saate minna API rakenduse URL brauseri kaudu Facebooki üksikute kasutaja mandaat. Selles jaotises saate veenduge, et see pole enam võimalik. 

1. Uues brauseriaknas, avage URL-i API rakendus uuesti.

2. Logige sisse, kui teil palutakse seda teha.

3. Sisselogimine õnnestub, kuid viib tõrge lehel.

    Olete konfigureerinud AAD rakenduse, et AAD rentniku kasutajate ei saa sisse logida ja juurdepääs API brauseri kaudu. Pääsete endiselt API rakenduse teenuse peamised märgiks, mis saate kontrollida web appi URL-i ja lisada rohkem ülesandeloendi üksuste abil.

## <a name="restrict-access-to-a-particular-service-principal"></a>Kindla teenuse põhilise juurdepääsu piiramiseks  

Kohe, mis tahes helistaja, mida saab hankida märgiks kasutaja või teenuse põhilise oma Azure AD rentniku saate helistada TodoListDataAPI (andmete taseme) API rakendus. Võite veenduge, et andmete taseme API rakenduse aktsepteerib ainult kõned rakendusest TodoListAPI (keskmisel taseme) API ja ainult kindla teenuse subjekt. 

Saate lisada need piirangud, lisades koodi valideerimiseks on `appid` ja `objectidentifier` nõuded sissetulevaid kõnesid.

Selles õppetükis saate kasutusele koodis kinnitatakse rakenduse ID-d ja teenuse põhisumma ID otse oma kontrolleril toimingud.  Alternatiivid on kasutada kohandatud `Authorize` atribuut või teha kinnitamine teie käivitamisel sequences (nt OWIN vahetarkvara). Näiteks viimase, leiate [selle rakenduse valimi](https://github.com/mohitsriv/EasyAuthMultiTierSample/blob/master/MyDashDataAPI/Startup.cs). 

Projekti TodoListDataAPI järgmisi muudatusi teha.

2. Avage fail *Controllers/TodoListController.cs* .

3. Kommenteerige välja read, mis seadmine `trustedCallerClientId` ja `trustedCallerServicePrincipalId`.

        private static string trustedCallerClientId = ConfigurationManager.AppSettings["todo:TrustedCallerClientId"];
        private static string trustedCallerServicePrincipalId = ConfigurationManager.AppSettings["todo:TrustedCallerServicePrincipalId"];

4. Kommenteerige välja koodi CheckCallerId meetod. See meetod nimetatakse iga toimingu meetod selle domeenikontrolleri alguses. 

        private static void CheckCallerId()
        {
            string currentCallerClientId = ClaimsPrincipal.Current.FindFirst("appid").Value;
            string currentCallerServicePrincipalId = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
            if (currentCallerClientId != trustedCallerClientId || currentCallerServicePrincipalId != trustedCallerServicePrincipalId)
            {
                throw new HttpResponseException(new HttpResponseMessage { StatusCode = HttpStatusCode.Unauthorized, ReasonPhrase = "The appID or service principal ID is not the expected value." });
            }
        }

5. Juurutage uuesti ToDoListDataAPI projekti Azure'i rakendust Service.

6. Brauseris, minge AngularJS esiosa web appi HTTPS-i URL ja avalehel menüüd **Loendite** .

    Rakendus ei tööta, kuna ei suuda kõnede tagasi lõppu. Uus kood kontrollib tegelik appid ja objectidentifier, kuid see pole veel õigete väärtuste kontrollida neid vastu. Arendaja tööriistad konsool brauseris aruannete, et server on tagasi HTTP 401 tõrge.

    ![Tööriistade arendaja tõrke Console](./media/app-service-api-dotnet-service-principal-auth/webapperror.png)

    Järgmistes juhistes Konfigureerige oodatud väärtusega.

8. Azure'i AD PowerShelli kaudu too väärtus põhisumma teenuse Azure AD TodoListWebApp projekti jaoks loodud rakenduse.

    lisamine. Juhised selle kohta, kuidas installida Azure PowerShelli ja luua ühenduse oma tellimuse, leiate [Azure'i ressursihaldur Azure PowerShelli kasutamine](../powershell-azure-resource-manager.md).

    b. Teenuse põhisumma loendi saamiseks käivitada soovitud `Login-AzureRmAccount` käsk ja seejärel soovitud `Get-AzureRmADServicePrincipal` käsk.

    c. Teenuse põhilise TodoListAPI taotluse ning ObjectId väärtuse otsimine ja salvestage see asukohta saate kopeerida hiljem.

7. Azure'i portaalis, liikuge API rakenduse tera API rakenduse juurutatud ToDoListDataAPI projekti.

9. Klõpsake **Sätted > rakenduse sätted**.

3. Lisage jaotises **rakenduse sätted** järgmised võtmed ja väärtused.

  	| **Klahv** | Todo:TrustedCallerServicePrincipalId |
  	|---|---|
  	| **Väärtus** | Teenuse põhisumma id helistaja rakendus |
  	| **Näide** | 4f4a94a4-6f0d-4072-4f4a94a4-6f0d-4072 |

  	| **Klahv** | Todo:TrustedCallerClientId |
  	|---|---|
  	| **Väärtus** | Kliendi ID helistaja rakenduse - TodoListAPI Azure AD rakendusest kopeeritud |
  	| **Näide** | 960adec2-b74a-484a-960adec2-b74a-484a |

6. Klõpsake nuppu **Salvesta**.

    ![Klõpsake nuppu Salvesta](./media/app-service-api-dotnet-service-principal-auth/trustedcaller.png)

6. Brauseris naaske web appi URL-i ja avalehel menüüd **Loendite** .

    Seekord rakendus töötab ootuspäraselt, sest rakendus usaldusväärsete helistaja ID ja teenuse põhisumma on oodatud väärtusega.

    ![Lehe Ülesandeloend](./media/app-service-api-dotnet-service-principal-auth/mvchome.png)

## <a name="building-the-projects-from-scratch"></a>Koostamise projektide algusest peale

Kaks Web API projekte on loodud **Azure'i API rakenduse** project malli abil ning asendades vaikimisi väärtused kontrolleril ToDoList kontrolleril. Azure AD teenuse põhisumma sõned ToDoListAPI projekti ostmiseks on installitud [Active Directory autentimise Raamatukogu (ADAL) .net-i](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) Nugeti pakett.
 
Leiate teavet selle kohta, kuidas luua rakenduse AngularJS ühe lehe Veebiteenuste tagasi end nagu ToDoListAngular, [käed kohta Lab: koostada ühe lehe rakenduse (SPA) ASP.net-i veebi-API ja Angular.js](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs). Azure AD autentimise koodi lisamise kohta leiate artiklist [Turvaliseks AngularJS ühe lehe rakenduste Azure AD](../active-directory/active-directory-devquickstarts-angular.md).

## <a name="troubleshooting"></a>Tõrkeotsing

[AZURE.INCLUDE [troubleshooting](../../includes/app-service-api-auth-troubleshooting.md)]

* Veenduge, et Ärge ajage ToDoListAPI (keskmisel taseme) ja ToDoListDataAPI (andmete taseme). Näiteks selles õpetuses lisate autentimise andmete taseme API rakenduse, **kuid rakenduse võti peab pärinema keskmisel taseme API rakenduse jaoks loodud rakenduse Azure AD**.

## <a name="next-steps"></a>Järgmised sammud

See on viimase õpetuse API rakenduste sarja. 

Teemast Azure Active Directory kohta leiate lisateavet järgmistest teemadest.

* [Azure'i AD arendajate juhend](http://aka.ms/aaddev)
* [Azure'i AD stsenaariumid](http://aka.ms/aadscenarios)
* [Azure'i AD näidised](http://aka.ms/aadsamples)

    [Web Appis WebAPI-OAuth2-AppIdentity-DotNet](http://github.com/AzureADSamples/WebApp-WebAPI-OAuth2-AppIdentity-DotNet) valim on sarnane, mis on näidatud selles õpetuses, kuid ilma rakendust Service autentimist kasutades.

Lisateavet muul viisil juurutamine Visual Studios projektide API rakendused Visual Studio abil või [automatiseerimine juurutamine](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery) [andmeallika juhtelemendi süsteemi](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control)kohta leiate teemast [juurutamise Azure'i rakendust Service rakendus](../app-service-web/web-sites-deploy.md).
