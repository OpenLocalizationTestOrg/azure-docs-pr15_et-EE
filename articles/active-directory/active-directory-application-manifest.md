<properties
   pageTitle="Azure Active Directory Rakendusmanifest mõistmine | Microsoft Azure'i"
   description="Üksikasjaliku ulatuse Azure Active Directory Rakendusmanifest, mis tähistab rakenduse identiteedi konfiguratsiooni Azure AD rentniku ja lihtsustamiseks OAuthi luba, nõusolekut kogemusi ja muud kasutatakse."
   services="active-directory"
   documentationCenter=""
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/11/2016"
   ms.author="dkershaw;bryanla"/>

# <a name="understanding-the-azure-active-directory-application-manifest"></a>Azure Active Directory Rakendusmanifest mõistmine

Azure AD rentniku, mis esitada püsivate identiteedi konfiguratsiooni rakenduse tuleb registreerida ja Azure Active Directory (AD) integreeritud rakendused. Selle konfiguratsiooni nõu käitusajal lubada stsenaariumid, mis võimaldavad tellida ja maakler autentimine/autoriseerimine Azure AD. Azure AD Rakenduse mudel kohta leiate lisateavet teemast [lisamine, värskendamine, ja rakenduse eemaldamise] [ ADD-UPD-RMV-APP] artikkel.

## <a name="updating-an-applications-identity-configuration"></a>Rakenduse identiteedi konfiguratsiooni värskendamine

See on saadaval värskendamine atribuutide rakenduse identiteedi konfiguratsiooni, mis on erinevad võimaluste ja kraadid raskusi, sealhulgas järgmisi tegelikult mitu võimalust:

- Funktsiooni ** [Azure klassikaline portaali] [ AZURE-CLASSIC-PORTAL] kasutajaliides Web** võimaldab teil rakenduse kõige levinum atribuutide värskendamine. See on kiireim ja vähemalt tõrge, kuna võivad tekkida kirjavead viis oma rakenduse atribuutide värskendamine, kuid ei anna teile täielik juurdepääs kõik atribuudid, näiteks järgmised kaks võimalust.
- Täpsemate stsenaariumi puhul, kus peate värskendama atribuudid, mis ei ole kokku Azure klassikaline portaalis, saate muuta **rakenduse näidata**. See on selles artiklis keskendutakse ja käsitletakse üksikasjalikumalt alates järgmisest jaotisest.
- Samuti on võimalik, et **kirjutamine rakendus kasutab [Graph API] [ GRAPH-API] ** värskendada oma rakendus, mis nõuab kõige vaeva. See võib olla atraktiivseks, kuigi, kui teil on kirjutamise tarkvara või rakenduse atribuutide regulaarselt automatiseeritud viisil värskendama.

## <a name="using-the-application-manifest-to-update-an-applications-identity-configuration"></a>Rakenduse manifesti abil värskendada rakenduse identiteedi konfigureerimine
[Azure'i klassikaline portaali]kaudu[AZURE-CLASSIC-PORTAL], saate hallata oma rakenduse identiteedi konfiguratsioon, allalaadimine ja üleslaadimine on JSON faili esituse, mida nimetatakse ka Rakendusmanifest. Tegelik faili talletatakse kataloogis. Rakenduse manifesti on üksnes HTTP GET toimingu Azure AD Graphi API rakendus üksus ja üleslaadimine on rakenduse üksuse HTTP paik toimingu.

Selle tulemusena vorming ja atribuudid Rakendusmanifest mõistmaks on vaja viidata Graph API [rakenduse üksuse] [ APPLICATION-ENTITY] dokumentatsiooni. Värskendused, mida saab teha kuigi rakenduse näidata üles näited.

- Esitatud oma veebi-API **Deklareerimislauset õiguse otsinguulatuste (oauth2Permissions)** . Vaadake teemat "Asetades API-de abil muu veebirakendused" [Integreerimine Azure Active Directory rakenduste] [ INTEGRATING-APPLICATIONS-AAD] teavet kasutaja isikustamine rakendamise abil soovitud oauth2Permissions delegeeritud õiguste ulatuse. Nagu eelnevalt mainitud, on rakenduse üksuse atribuutide on avaldatud Graph API [üksus ja keerukas tüüp] [ APPLICATION-ENTITY] artiklit, sh oauth2Permissions atribuut, mis on tippige [OAuth2Permission]kogum[APPLICATION-ENTITY-OAUTH2-PERMISSION].
- **Rakenduse esitatud Deklareerimislauset rakenduse rollide (appRoles)**. Rakenduse ettevõtte appRoles atribuut on tippige [AppRole]kogumi[APPLICATION-ENTITY-APP-ROLE]. Vaata [rollipõhise juurdepääsu reguleerimine cloud rakendustes abil Azure AD] [ RBAC-CLOUD-APPS-AZUREAD] artikkel näide rakendamise kohta.
- **Deklareerimislauset teadaolevad kliendi rakenduste (knownClientApplications)**, mis võimaldavad teil loogiliselt siduda määratud kliendi rakendustes ressursi/veebi API nõusolek.
- **Taotleda rühma liikmestaatusi taotlemine väljaandmisel Azure AD** kehtib kasutaja (groupMembershipClaims).  See võib olla konfigureeritud väljaandmisel nõuete kohta kasutaja directory rollid liikmelisused. [Luba Cloud rakendustes AD rühmad, mille abil] vt[ AAD-GROUPS-FOR-AUTHORIZATION] artikkel näide rakendamise kohta.
- **Luba rakenduse toetamiseks OAuthi 2.0 peidetud anda** puhul (oauth2AllowImplicitFlow). Seda tüüpi anda meilivoo kasutatakse manustatud JavaScripti veebilehtede ja ühe lehe rakenduste (SPA). Peidetud loa andmine kohta leiate lisateavet teemast [Azure Active Directory kulgemist mõistmine peidetud OAuth2 anda][IMPLICIT-GRANT].
- **Luba kasutamine X509 sertifikaatide salajane võti** (keyCredentials). Lugege teemat [Office 365 teenuseid ja daemon rakenduste koostamine] [ O365-SERVICE-DAEMON-APPS] ja [arendaja juhend auth Azure'i ressursihaldur API-ga] [ DEV-GUIDE-TO-AUTH-WITH-ARM] rakendamise näited leiate artiklitest.
- **Lisa uus rakendus ID URI** rakenduse (identifierURIs[]). Rakenduse ID URI-d kasutatakse kordumatult rakenduse oma Azure AD rentniku (või üle mitme Azure AD rentniku mitme rentniku stsenaariumid, kui kvalifitseeritud kinnitatud kohandatud domeeninime kaudu). Neid kasutatakse taotlemisel ressursi rakenduse või juurdepääsu sümboolse ressursi rakenduse hankimine õigused. – See element värskendamisel tehakse vastava teenuse põhilise servicePrincipalNames [] saidikogumi, mis asub rakenduse Avaleht rentniku sama värskendust.

Rakenduse manifesti pakub hea viis rakenduse registreerimise oleku jälgimine. Kuna see on saadaval JSON-vormingus, saate faili esituse märgitud rakenduse lähtekoodi koos teie andmeallika juhtelementi.

## <a name="step-by-step-example"></a>Samm-samm näide
Nüüd saate värskendada oma rakenduse identiteedi konfiguratsioon kaudu Rakendusmanifest nõutav sammult. Toome esile ühte eelmistes näidetes, näitab, kuidas deklareerida uue õiguste ulatuse ressursside rakendus:

1. Liikuge [Azure klassikaline portaali] [ AZURE-CLASSIC-PORTAL] ja logige sisse kontoga, mis sisaldab teenuse administraator või koostöö administraator õigusi.

2. Kui olete autenditud, liikuge kerides allapoole ja valige Azure "Active Directory" pikendamise vasakpoolsel navigeerimisribal paneel (1), siis klõpsake Azure AD rentniku, kus teie rakendus on registreeritud (2).

    ![Valige Azure AD rentniku][SELECT-AZURE-AD-TENANT]

3. Kui lehe kataloogi, klõpsake registreeritud rentniku rakenduste loendi kuvamiseks lehe ülaosas "Rakendused" (1). Otsige soovitud rakendus loendi värskendamiseks ja klõpsake seda (2).

    ![Valige Azure AD rentniku][SELECT-AZURE-AD-APP]

4. Nüüd, kui olete valinud rakenduse põhilehe, Pange tähele (1) lehe lõppu "Halda näidata" funktsiooni. Kui seda linki, palutakse alla laadida või JSON manifest faili üles laadida. Klõpsake "Alla manifesti" (2). See kohe järgneb koos allalaadimine kinnituse dialoogiboks, mis palub teil kinnitada, klõpsates "Allalaadimine näidata" (3), siis kas avada või salvestada faili kohalik (4).

    ![Manifest haldamine, allalaadimine suvand][MANAGE-MANIFEST-DOWNLOAD]

    ![Laadige alla manifest][DOWNLOAD-MANIFEST]

5. Selles näites me faili salvestanud, mis võimaldab meil toimetaja avada, muuta selle JSON ja salvestage uuesti. Siin on JSON struktuuri näeb sees Visual Studio JSON-redaktor. See järgib skeemiga [rakenduse üksusele] märkme[ APPLICATION-ENTITY] nagu varem mainitud:

    ![Manifest JSON värskendamine][UPDATE-MANIFEST]

    Näiteks eeldades, et soovime rakendada/jätke uue õigusetaseme nimega "Employees.Read.All" meie ressursside rakendus (API), lihtsalt lisate uue/teise elemendi oauth2Permissions kogumi, st:

        "oauth2Permissions": [
        {
        "adminConsentDescription": "Allow the application to access MyWebApplication on behalf of the signed-in user.",
        "adminConsentDisplayName": "Access MyWebApplication",
        "id": "aade5b35-ea3e-481c-b38d-cba4c78682a0",
        "isEnabled": true,
        "type": "User",
        "userConsentDescription": "Allow the application to access MyWebApplication on your behalf.",
        "userConsentDisplayName": "Access MyWebApplication",
        "value": "user_impersonation"
        },
        {
        "adminConsentDescription": "Allow the application to have read-only access to all Employee data.",
        "adminConsentDisplayName": "Read-only access to Employee records",
        "id": "2b351394-d7a7-4a84-841e-08a6a17e4cb8",
        "isEnabled": true,
        "type": "User",
        "userConsentDescription": "Allow the application to have read-only access to your Employee data.",
        "userConsentDisplayName": "Read-only access to your Employee records",
        "value": "Employees.Read.All"
        }
        ],

    Kirje peab olema kordumatu ja seega peab saate luua uue globaalselt kordumatu ID (GUID) jaoks soovitud `"id"` atribuut. Sel juhul, kuna oleme määratud `"type": "User"`, saab see õigus olema nõus ühegi konto autenditud Azure AD rentniku ressursi/API rakendus on registreeritud. Seda määrab kliendi rakenduse juurdepääsuõigusi selle konto nimel. Kirjelduse ja kuvatav nimi stringid kasutatakse ajal nõusolekut ja Kuva Azure klassikaline portaalis.

6. Kui olete lõpetanud, värskendamine manifest, Azure klassikaline portaalis Azure AD Rakenduse lehele naasmiseks ja klõpsake "Halda näidata" funktsiooni uuesti (1). Kuid seekord, valige suvand "Üles näidata" (2). Sarnaselt allalaadimine, mida ootab uuesti teise dialoogiboksis küsimine JSON-faili asukoht. Klõpsake "Sirvi faili..." (3), siis dialoogiboksis "Valige faili laadi kasutamiseks üles" abil saate valida JSON faili (4) ja vajutage "Ava". Kui dialoogiboksi kaob, valige "OK" märge (5) ja laaditakse teie manifesti.  

    ![Hallata manifest, üles suvand][MANAGE-MANIFEST-UPLOAD]

    ![Laadige manifest JSON][UPLOAD-MANIFEST]

    ![Laadige manifest JSON - kinnituse][UPLOAD-MANIFEST-CONFIRM]

Nüüd, kui manifest on salvestatud, saate anda registreeritud kliendi rakenduse juurdepääsu on uut õigusetaset lisasime kohal. Seekord saate kasutada Azure klassikaline portaali Web UI redigeerimise kliendi Rakendusmanifest asemel.  

1. Esmalt avage leht "Konfigureerimine" klientrakendusega, millele soovite lisada uue API juurdepääsu, ja klõpsake nuppu "Lisa rakenduse".
2. Seejärel saate esitatakse koos rentniku registreeritud ressursi rakenduste (API) loend. Klõpsake plussmärki / + ressurss rakenduse nimi ja valige selle kõrval sümbol.  
3. Klõpsake paremas allnurgas märke.
4. Oma kliendi konfiguratsiooni lehe jaotises "Lisada rakendus" naastes näete uue ressursi rakenduste loendis. Kui viite hiirekursori "Delegeeritud õigused" jaotis rea paremal, kuvatakse ripploendis kuvada. Klõpsake loendis ja valige uue õigusetaseme lisamiseks kliendi nõutud loendi õigused. Märkus: salvestatakse see uue õigusetaseme kliendi rakendus identiteedi konfigureerimine saidikogumi atribuudi "requiredResourceAccess".

![Muude rakenduste õigused][PERMS-TO-OTHER-APPS]

![Muude rakenduste õigused][PERMS-SELECT-APP]

![Muude rakenduste õigused][PERMS-SELECT-PERMS]

See on õige. Rakenduste käivituvad kohe tema identiteedi uue konfiguratsiooni abil.

## <a name="next-steps"></a>Järgmised sammud
- Rakenduse rakenduste ja teenuste põhisumma objektide seoste kohta leiate lisateavet teemast [rakenduste ja teenuste põhisumma objektide Azure AD][AAD-APP-OBJECTS].
- Teemast [Azure AD arendaja sõnastik] [ AAD-DEVELOPER-GLOSSARY] osa Azure Active Directory (AD) arendaja keskendudes määratlusi.

Kasutage DISQUS kommentaarid allpool olevat jaotist tagasiside ja aidake meil viimistlemine ja kujundamine meie sisu.

<!--Image references-->
[DOWNLOAD-MANIFEST]: ./media/active-directory-application-manifest/download-manifest.png
[MANAGE-MANIFEST-DOWNLOAD]: ./media/active-directory-application-manifest/manage-manifest-download.png
[MANAGE-MANIFEST-UPLOAD]: ./media/active-directory-application-manifest/manage-manifest-upload.png
[PERMS-SELECT-APP]: ./media/active-directory-application-manifest/portal-perms-select-app.png
[PERMS-SELECT-PERMS]: ./media/active-directory-application-manifest/portal-perms-select-perms.png
[PERMS-TO-OTHER-APPS]: ./media/active-directory-application-manifest/portal-perms-to-other-apps.png
[SELECT-AZURE-AD-APP]: ./media/active-directory-application-manifest/select-azure-ad-application.png
[SELECT-AZURE-AD-TENANT]: ./media/active-directory-application-manifest/select-azure-ad-tenant.png
[UPDATE-MANIFEST]: ./media/active-directory-application-manifest/update-manifest.png
[UPLOAD-MANIFEST]: ./media/active-directory-application-manifest/upload-manifest.png
[UPLOAD-MANIFEST-CONFIRM]: ./media/active-directory-application-manifest/upload-manifest-confirm.png

<!--article references -->
[AAD-APP-OBJECTS]: active-directory-application-objects.md
[AAD-DEVELOPER-GLOSSARY]: active-directory-dev-glossary.md
[AAD-GROUPS-FOR-AUTHORIZATION]: http://www.dushyantgill.com/blog/2014/12/10/authorization-cloud-applications-using-ad-groups/
[ADD-UPD-RMV-APP]: active-directory-integrating-applications.md
[APPLICATION-ENTITY]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[APPLICATION-ENTITY-APP-ROLE]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#approle-type
[APPLICATION-ENTITY-OAUTH2-PERMISSION]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permission-type
[AZURE-CLASSIC-PORTAL]: https://manage.windowsazure.com
[DEV-GUIDE-TO-AUTH-WITH-ARM]: http://www.dushyantgill.com/blog/2015/05/23/developers-guide-to-auth-with-azure-resource-manager-api/
[GRAPH-API]: active-directory-graph-api.md
[IMPLICIT-GRANT]: active-directory-dev-understanding-oauth2-implicit-grant.md
[INTEGRATING-APPLICATIONS-AAD]: https://azure.microsoft.com/documentation/articles/active-directory-integrating-applications/
[O365-PERM-DETAILS]: https://msdn.microsoft.com/office/office365/HowTo/application-manifest
[O365-SERVICE-DAEMON-APPS]: https://msdn.microsoft.com/office/office365/howto/building-service-apps-in-office-365
[RBAC-CLOUD-APPS-AZUREAD]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/

