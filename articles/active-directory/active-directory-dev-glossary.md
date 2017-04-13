<properties
   pageTitle="Azure Active Directory arendaja sõnastik | Microsoft Azure'i"
   description="Levinud Azure Active Directory arendaja põhimõtet ja funktsioonide loend."
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
   ms.date="08/31/2016"
   ms.author="bryanla"/>

# <a name="azure-active-directory-developer-glossary"></a>Azure Active Directory arendaja sõnastik
See artikkel sisaldab määratlusi mõne Azure Active Directory (AD) arendaja keskendudes, mis on kasulik, kui Õppekeskuse Azure AD jaoks rakenduste arendamise kohta.

## <a name="access-token"></a>juurdepääsu luba
Teatud tüüpi [Turbeloa](#security-token) välja ettevõtte [autoriseerimine server](#authorization-server)ja [klientrakendusega](#client-application) [kaitstud ressursi serveri](#resource-server)juurdepääsuks kasutada. Tavaliselt on [JSON Web Turbeloa (JWT)]kujul[JWT], luba hõlmab kliendi [ressursi omanik](#resource-owner)nõutud taseme juurdepääsu luba. Luba sisaldab kõiki rakendatav [taotluste](#claim) teemal, mis võimaldab kasutada seda identimisteavet vormile antud ressursi kasutamisel kliendi kohta. See kaob ka vajadus ressursi omanik esitamist kliendi mandaat.

Accessi sõned nimetatakse vahel "Kasutaja + App" või "Ainult App", olenevalt identimisteabe esitada. Näiteks kui kliendi rakendus kasutab funktsiooni:

- ["Luba kood" loa andmine](#authorization-grant), lõppkasutaja autendib esmalt ressursi omanik, delegeerivad kliendi ressursile juurdepääsu luba. Kliendi autendib hiljem saamiseks juurdepääsu luba. Luba saate mõnikord nimetatakse täpsemalt märgiks "Kasutaja + App" see väljendab nii kasutajale lubatud klientrakenduse ja rakendus.
- ["Kliendi mandaati" loa andmine](#authorization-grant), kliendi pakub ainus autentimisel toimimist ilma ressursi omanik autentimine/luba nii luba saate mõnikord nimetatakse ka "Ainult rakenduse" luba.

Teemast [Azure AD Turbeloa juhend] [ AAD-Tokens-Claims] rohkem üksikasju.

## <a name="application-manifest"></a>rakenduse manifesti  
Funktsioon [Azure klassikaline portaalis]esitatud[AZURE-classic-portal], mis toodab JSON kujutis rakenduse identiteedi konfiguratsioon, kasutatakse süsteem seotud [rakenduse] värskendamine[ AAD-Graph-App-Entity] ja [ServicePrincipal] [ AAD-Graph-Sp-Entity] üksused. Teemast [Azure Active Directory Rakendusmanifest mõistmine] [ AAD-App-Manifest] rohkem üksikasju.

## <a name="application-object"></a>rakenduse objekt  
Kui teil register või uuendada rakenduse [Azure klassikaline portaali][AZURE-classic-portal], portaali loob/värskenduste objekti rakenduse nii vastava [põhisumma objekti](#service-principal-object) selle rentniku jaoks. Rakenduse objekti *määratleb* kasutaja identiteedi konfiguratsiooni globaalselt (rentnikud kõik kui see on juurdepääs), esitada malli, kust selle vastava teenuse põhisumma objektide on *tuletatud* kasutamiseks kohalikult käitusaja (teatud sihtrentnikus).

Lugege teemat [rakenduste ja teenuste põhisumma objektide] [ AAD-App-SP-Objects] lisateabe saamiseks.

## <a name="application-registration"></a>rakenduse registreerimine  
Selleks, et integreerida ja volitatud esindaja Azure AD identiteedi ja juurdepääsu juhtimine funktsioone, peab registreerunud on Azure AD [rentniku](#tenant). Azure AD Rakenduse registreerimisel esitate konfigureerimisel identiteedi rakenduse, võimaldades integreerimine Azure AD ja kasutada funktsioone, näiteks:

- Robustne haldus, ühekordse sisselogimise kasutamine Azure AD identiteetide haldus ja [OpenID ühendamine] [ OpenIDConnect] Protocol (protokoll) rakendamine
- Vahendatud juurdepääsu [kaitstud ressursid](#resource-server) [klientrakendustes](#client-application), kaudu Azure AD OAuth 2.0 [autoriseerimine serveri](#authorization-server) juurutamine
- [Nõusolek framework](#consent) kliendi kaitstud ressursse, mis põhineb ressursi omaniku luba juurdepääsu haldamine.

Teemast [Azure Active Directory integreerimine rakenduste] [ AAD-Integrating-Apps] rohkem üksikasju.

## <a name="authentication"></a>autentimine
Tegu raske isiku õigustatud mandaat, luues loomine turvalisus põhisumma kasutatakse identiteedi ja juurdepääsu juhtimine. Ajal on [OAuth2 loa andmine](#authorization-grant) näiteks pool autentimine on täitmine [ressursi omanik](#resource-owner) või [klientrakendusega](#client-application), sõltuvalt kasutatud andmise roll.

## <a name="authorization"></a>luba
Act anda autenditud turvalisus põhisumma luba midagi teha. Azure AD programmeerimise mudelis on kaks esmane kasutamise juhtudel.

- Mõne [OAuth2 loa andmine](#authorization-grant) meilivoo ajal: kui [ressursi omanik](#resource-owner) annab autoriseerimine [klientrakendusega](#client-application), võimaldades kliendi juurdepääsu ressursi omanik ressursid.
- Ressursi juurdepääsu kliendi poolt ajal: rakendatud [ressursside server](#resource-server), kasutades [taotlemine](#claim) väärtuste esitamine [juurdepääsu luba](#access-token) põhineb neile juurdepääsu juhtimine otsuste tegemiseks.

## <a name="authorization-code"></a>luba kood
"Turbeloa" lühike elas esitatud [klientrakendusega](#client-application) [autoriseerimine lõpp-punkti](#authorization-endpoint)osana "autoriseerimine kood" voogu, üks neljast OAuth2 [autoriseerimine annab](#authorization-grant). Koodi tagastatakse klientrakendusega vastuseks autentimise [ressursi omanik](#resource-owner), mis näitab, ressursside omanik delegeeritud Luba juurdepääs nõutud ressursid. Voo osana kood on hiljem lunastada mõne [juurdepääsu luba](#access-token).

## <a name="authorization-endpoint"></a>luba lõpp-punkti
Lõpp-punktid [autoriseerimine server](#authorization-server), mida suhelda [ressursi omanik](#resource-owner) pakkumiseks ajal OAuth2 luba [loa andmine](#authorization-grant) on rakendatud üks anda kulgemist. Sõltuvalt loa andmine meilivoo kasutatud esitatud andmist võivad erineda, sealhulgas [kood](#authorization-code) - või [Turbeloa](#security-token).

Vt OAuth2 määratlus [loa andmine tüüpi] [ OAuth2-AuthZ-Grant-Types] ja [Luba lõpp-punkti] [ OAuth2-AuthZ-Endpoint] jaotiste ja [OpenIDConnect määratlus] [ OpenIDConnect-AuthZ-Endpoint] lähemalt.

## <a name="authorization-grant"></a>loa andmine
[Ressursi omaniku](#resource-owner) [Luba](#authorization) juurdepääsu selle kaitstud ressursse, mis on antud [klientrakendusega](#client-application)tähistav mandaati. Kliendi rakendus saate kasutada ühte [neli anda tüüpi OAuth2 autoriseerimine raames määratletud] [ OAuth2-AuthZ-Grant-Types] saada grant, olenevalt kliendi tüüp/nõuded: "luba kood grant", "kliendi mandaadi andmiseks", "peidetud grant" ja "ressursi omanik parooli mandaadi anda". Mandaat, tagastatakse kliendile on kas mõni [juurdepääsu luba](#access-token), või [Luba kood](#authorization-code) (vahetatud hiljem juurdepääsu sümboolse), sõltuvalt kasutatud loa andmine.

## <a name="authorization-server"></a>autoriseerimine server
Määratletud [OAuth2 autoriseerimine Framework][OAuth2-Role-Def], Accessi välja server märkide [Kliendi](#client-application) pärast edukalt autentimisel [ressursi omanik](#resource-owner) ja selle loa saamiseks. [Klientrakenduse](#client-application) suhtleb autoriseerimine server käitusajal kaudu [autoriseerimine](#authorization-endpoint) ja [Turbeloa](#token-endpoint) lõpp-punktid vastavalt selle OAuth2 määratletud [autoriseerimine annab](#authorization-grant).

Azure AD rakenduste integreerimise, puhul Azure AD rakendab autoriseerimine serveri roll Azure AD rakenduste ja Microsofti teenus API-d, näiteks [Microsoft Graphi API -de][Microsoft-Graph].

## <a name="claim"></a>taotlemine
[Turbeloa](#security-token) sisaldab nõuete esitada kinnitused ühe üksuse (nt [klientrakendusega](#client-application) või [ressursside omanik](#resource-owner)) üle teisele isikule (nt [ressursside server](#resource-server)). Nõuded on nime ja väärtuse paarideks, mis edastavad fakte Turbeloa teema (nt turvalisus põhisumma, mis on autenditud [autoriseerimine server](#authorization-server)). Esita on antud märgiks nõuded on sõltub mitut muutujat, sealhulgas luba, teema, rakenduse konfigureerimise jne kasutatakse mandaati tüüp.

Teemast [Azure AD Turbeloa juhend] [ AAD-Tokens-Claims] rohkem üksikasju.

## <a name="client-application"></a>klientrakendus  
Määratletud [OAuth2 autoriseerimine Framework][OAuth2-Role-Def], rakendus, mis teeb kaitstud ressursi taotlusi nimel [ressursi omanik](#resource-owner). Termini "klient" ei tähenda kindla riistvara rakendamist omadusi (näiteks kas rakenduse aktiveeritakse server, laua-või muud seadmed).  

Kliendi rakendus taotleb ressursi omanik osalemiseks on [OAuth2 loa andmine](#authorization-grant) meilivoo [autoriseerimine](#authorization) ja juurdepääs API-de/andmetele ressursi omaniku nimel. OAuth2 autoriseerimine raamistiku [määratleb kahte tüüpi kliendid][OAuth2-Client-Types], "konfidentsiaalne" ja "avalik", mis põhineb kliendi võimalus hoidma oma mandaadi. Rakendusi saate rakendada [web klient (konfidentsiaalne)](#web-client) , mis töötab veebiserverisse installitud seadme või [kasutaja agent vastavalt kliendi (avalik)](#user-agent-based-client) , mis töötab seadme brauseris [omakliendi (avalik)](#native-client) .

## <a name="consent"></a>nõusolek
[Ressursi omanik](#resource-owner) on [klientrakendusega](#client-application)teatud [õigused](#permissions) kaitstud ressursid, ressursside omaniku nimel annab protsess. Olenevalt kliendi poolt küsitud õigused, palutakse administraator või kasutaja nõusolekut vastavalt oma asutuse/isik andmetele juurdepääsu lubamiseks. Pange tähele, [mitme rentniku](#multi-tenant-application) stsenaariumi rakenduse [teenuse põhisumma](#service-principal-object) salvestatakse ka, kus kasutaja rentnik.

## <a name="id-token"></a>ID luba
Mõne [OpenID Connect] [ OpenIDConnect-ID-Token] [Turbeloa](#security-token) esitatud [autoriseerimine server](#authorization-server) [autoriseerimine lõpp-punkti](#authorization-endpoint), mis sisaldab [taotluste](#claim) autentimise lõppkasutaja [ressursside omanik](#resource-owner). Juurdepääsu sümboolse, nt ID sõned digitaalselt allkirjastatud [JSON Web Turbeloa (JWT)]ka arvuna[JWT]. Erinevalt juurdepääsu sümboolse kuigi ID luba taotluste seotud ressursside access ei kasutata ja konkreetselt juurdepääsu kontroll.

Teemast [Azure AD Turbeloa juhend] [ AAD-Tokens-Claims] rohkem üksikasju.

## <a name="multi-tenant-application"></a>mitme rentniku rakendus
[Kliendi rakendus](#client-application) , mis võimaldab klassi logige sisse ja [nõusoleku](#consent) kasutajate poolt ette valmistatud mis tahes Azure AD [rentniku](#tenant), sh rentnikud, kus on kliendi kasutajaks. Seevastu rakenduse ühe rentniku, mis on registreeritud ainult võimaldaks sisselogimist ette valmistatud samal rentnikukontol üks kasutaja kontode kus rakendus on registreeritud. [Omakliendi](#native-client) rakendused on mitme rentniku vaikimisi [web](#web-client) klientrakendustes on võimalus valida ühe ja mitme rentnikuga.

Vaadake, [kuidas mõni Azure AD kasutaja mustri mitme rentniku rakenduse abil sisse logida] [ AAD-Multi-Tenant-Overview] rohkem üksikasju.

## <a name="native-client"></a>omakliendi
Teatud tüüpi [klientrakendusega](#client-application) algupäraselt seadmesse installitud. Kuna kõik kood käivitatakse seadmes, leitakse "avalik" kliendi tõttu ta ei ole identimisteabe privaatselt/konfidentsiaalsus talletamiseks. Vt [OAuth2 KLIENDITÜÜPIDE ja profiilid] [ OAuth2-Client-Types] rohkem üksikasju.

## <a name="permissions"></a>õigused
[Klientrakenduse](#client-application) saab juurdepääsu [Ressursi server](#resource-server) , deklareerimise luba kutsed. Saadaval on kahte tüüpi:

- Jaotises [ulatus vastavalt](#scopes) juurdepääsu taotlemise "Delegeeritud" õigused delegeeritud loata sisse logitud [ressursi omanik](#resource-owner), kliendi [juurdepääsu luba](#access-token) ["scp" taotluste](#claim) ressursile käitusajal esitatud.
- Jaotises kliendi rakendus identimisteabe/identiteedi [Rollipõhine](#roles) juurdepääsu taotlemise, "Rakendus" õigused on esitatud ressursile käitusajal ["rollid" nõuded](#claim) kliendi juurdepääsu luba.

Need on ka Surface'i käigus [nõusoleku](#consent) anda administraatori või ressursside omaniku võimalus anda/keelata kliendi juurdepääsu oma rentniku ressurssidele.

Luba kutsed on konfigureeritud "Rakendused" ja "Konfigureerimine" menüüd [classic Azure portaali][AZURE-classic-portal], klõpsake jaotises "muudes rakendustes", valides soovitud "delegeeritud õigused" ja "Rakenduse õigused" (nõutud liikmelisus sisse üldadministraatori rolliga). Kuna [avaliku kliendi](#client-application) ei saa hallata mandaat, seda ainult taotleda delegeeritud õigused ajal [konfidentsiaalse kliendi](#client-application) on võimalus taotluse nii delegeeritud ja rakenduse õigused. Kliendi [rakenduse objekti](#application-object) talletab deklareeritud õiguste [requiredResourceAccess atribuudi][AAD-Graph-App-Entity].

## <a name="resource-owner"></a>ressursi omanik
Määratletud [OAuth2 autoriseerimine Framework][OAuth2-Role-Def], üksuse kaitstud ressursi juurdepääsu andmine. Kui ressursi omanik on isik, nimetatakse lõppkasutaja. Näiteks kui [klientrakendusega](#client-application) soovib kasutaja postkasti [Microsoft Graphi API]kaudu juurdepääsu[Microsoft-Graph], see nõuab ressursi omaniku loata postkasti.

## <a name="resource-server"></a>ressursi server
Määratletud [OAuth2 autoriseerimine Framework][OAuth2-Role-Def], server, et hosts kaitstud ressursid vastu võtmist ja koosolekukutsetele kaitstud ressursi taotluste esitamine on [Luba juurdepääs](#access-token) [klientrakendustes](#client-application) . Tuntud ka kui kaitstud ressursi server, või ressursside rakendus.

Ressursi serveri seab API-d ja jõustab juurdepääsu oma kaitstud ressursside [otsinguulatuste](#scopes) ja [rollid](#roles), kasutades OAuthi 2.0 autoriseerimine raames. Näiteks Azure AD Graph API, mis pakub Azure AD rentniku andmetele juurdepääsu ja Office 365 API, mis pakuvad juurdepääsu andmetele, nt meil ja kalender. Mõlemad on kättesaadavad [Microsoft Graphi API][Microsoft-Graph].  

Nii nagu klientrakendusega, ressursside rakenduse identiteedi konfiguratsioon foorumite kaudu [registreerimise](#application-registration) Azure AD rentniku, rakenduste ja teenuste peamine eesmärk. Mõned Microsofti lisatud API Azure AD Graph API, nagu on registreerinud teenuse põhisumma kättesaadavaks kõigi rentnikud ettevalmistamise ajal.

## <a name="roles"></a>rollid
[Otsinguulatuste](#scopes), nt rollid võimalda [ressursside serveri](#resource-server) reguleerida juurdepääsu oma kaitstud ressursse. On kahte tüüpi: "kasutajaroll" rakendab Rollipõhine juurdepääsu reguleerimine kasutajate ja rühmade ajal "rakenduste" roll rakendab sama [klientrakendustes](#client-application) juurdepääsu nõudvate, ressursile juurdepääsu nõudvate.

Rollid on stringid ressursi määratletud (näiteks "kulude kinnitaja", "Kirjutuskaitstud", "Directory.ReadWrite.All") hallatavate [Azure klassikaline portaali] [ AZURE-classic-portal] kaudu ressursi [rakenduse näidata](#application-manifest), ja talletatud ressursi [appRoles atribuudi][AAD-Graph-Sp-Entity]. Azure'i klassikaline portaali kasutatakse ka kasutajate määramine "kasutaja" rollid ja konfigureerida kliendi [teenuserakenduse õiguste](#permissions) rolli "rakendus".

Rakenduse rollid, mis on esitatud Azure AD Graph API üksikasjalik arutelu, leiate teemast [Graph API õiguste otsinguulatuste][AAD-Graph-Perm-Scopes]. Samm-sammult rakendamist näiteks vt [rollipõhise juurdepääsu reguleerimine cloud rakendustes abil Azure AD][Duyshant-Role-Blog].

## <a name="scopes"></a>otsinguulatuste
[Rollid](#roles), nt otsinguulatuste võimalda [ressursside serveri](#resource-server) reguleerida juurdepääsu oma kaitstud ressursse. Otsinguulatuste kasutatakse rakendamiseks [ulatus vastavalt] [ OAuth2-Access-Token-Scopes] juurdepääsu kontroll [klientrakendusega](#client-application) , mis on antud delegeeritud juurdepääsu ressursile omanik.

Otsinguulatuste on ressurss määratletud stringid (nt "Mail.Read", "Directory.ReadWrite.All") hallatavate [Azure klassikaline portaali] [ AZURE-classic-portal] kaudu ressursi [rakenduse näidata](#application-manifest), ja talletatud ressursi [oauth2Permissions atribuudi][AAD-Graph-Sp-Entity]. Azure'i klassikaline portaali kasutatakse ka kliendi rakenduse [delegeeritud õiguste](#permissions) juurde pääseda, mille ulatus konfigureerimiseks.

Parimaid tavasid nimereeglistik, on kasutada "resource.operation.constraint" vormingut. Otsinguulatuste, mis on esitatud Azure AD Graph API üksikasjalik arutelu, leiate teemast [Graph API õiguste otsinguulatuste][AAD-Graph-Perm-Scopes]. Otsinguulatuste esitatud Office 365 teenustega, leiate teemast [Office 365 API õiguste viide][O365-Perm-Ref].

## <a name="security-token"></a>Turbeloa
Allkirjastatud dokument, mis sisaldab taotluste, nagu OAuth2 luba või SAML 2.0 kinnitus. Mõne OAuth2 [loa andmine](#authorization-grant), [juurdepääsu luba](#access-token) (OAuth2) ja ka [ID Turbeloa](OpenID Connect) on turvalisus sõned tüübid, mis on rakendada mõne [JSON Web Turbeloa (JWT)][JWT].

## <a name="service-principal-object"></a>teenuse peamine eesmärk
Kui teil register või uuendada rakenduse [Azure klassikaline portaali][AZURE-classic-portal], portaali loob/värskenduste [rakenduse objekti](#application-object) nii vastava teenuse peamine eesmärk selle rentniku jaoks. Rakenduse objekti *määratleb* rakenduse identiteedi konfiguratsioon globaalselt (rentnikud kõik kui seotud rakendus on antud juurdepääs), ja on Mall, mille kohta oma vastava teenuse põhisumma objektide on *tuletatud* kasutamiseks kohalikult käitusaja (teatud sihtrentnikus).

Lugege teemat [rakenduste ja teenuste põhisumma objektide] [ AAD-App-SP-Objects] lisateabe saamiseks.

## <a name="sign-in"></a>sisselogimine
Protsessi [klientrakendusega](#client-application) alustamist lõppkasutaja autentimise ja hõivamine seotud olekus [Turbeloa](#security-token) hankimine ja selle rakenduse seansi ulatuse määramine. Märkige võivad hõlmata esemeid, nt kasutajaprofiili teabe ja teavet saadud Turbeloa taotluste.

Rakenduse funktsiooni kasutatakse tavaliselt rakendada ühekordse sisselogimise (SSO). Selle võib ka eelnema "registreerumise" funktsiooni nimega sisendpunkti kasutajal juurde pääseda rakenduse (pärast esimest sisselogimist). Registreerumine funktsiooni kasutatakse kogumine ja püsivad täiendavad olekus teatud kasutaja ja võib olla vaja [kasutaja nõusolek](#consent).

## <a name="sign-out"></a>väljunud
Protsessi un autentiva lõppkasutaja saidimääratlusest kasutaja riigi seostatud [klientrakendusega](#client-application) seansi [sisselogimisel](#sign-in)

## <a name="tenant"></a>rentniku
Azure'i AD-kaustas näiteks nimetatakse ka Azure AD rentniku. Pakub mitmesuguseid funktsioone, sh:

- integreeritud rakenduste registri teenus
- kasutajakontode ja registreeritud taotluste autentimise
- ÜLEJÄÄNUD lõpp-punktid nõutav erinevate protokollide, sh OAuth2 ja SAML, sh [autoriseerimine lõpp-punkti](#authorization-endpoint), [Turbeloa lõpp-punkti](#token-endpoint) ja "Üldine" lõpp-punkti [mitme rentniku](#multi-tenant-application)rakendused.

Rentniku on seotud Azure AD ka või Office 365 tellimuse tellimuse, identiteedi ja juurdepääsu juhtimine funktsioonide pakkumise tellimuse ettevalmistamise ajal. Vaadake, [Kuidas hankida on Azure Active Directory rentnik] [ AAD-How-To-Tenant] üksikasjalikku erinevatel viisidel pääseda rentniku jaoks. Vaadake, [Kuidas Azure'i tellimused on seostatud Azure Active Directory] [ AAD-How-Subscriptions-Assoc] tellimuste seos on Azure AD rentniku kohta.

## <a name="token-endpoint"></a>Turbeloa lõpp-punkti
Üks rakendatud [autoriseerimine server](#authorization-server) toetamiseks OAuth2 [autoriseerimine annab](#authorization-grant)lõpp-punktid. Sõltuvalt anda, seda saab kasutada hankida [juurdepääsu luba](#access-token) (ja sellega seotud "Värskenda" luba) [Kliendi](#client-application)või [ID luba](#ID-token) kasutamisel koos [OpenID ühendamine] [ OpenIDConnect] Protocol (protokoll).

## <a name="user-agent-based-client"></a>Kasutaja agent vastavalt kliendi
Teatud tüüpi [klientrakendus](#client-application) , mis allalaadimist veebiserverisse koodi ja käivitab sees kasutajaagendi (näiteks veebibrauser), nt ühe lehe rakenduse (SPA). Kuna kõik kood käivitatakse seadmes, leitakse "avalik" kliendi tõttu ta ei ole identimisteabe privaatselt/konfidentsiaalsus talletamiseks. Vt [OAuth2 KLIENDITÜÜPIDE ja profiilid] [ OAuth2-Client-Types] rohkem üksikasju.

## <a name="user-principal"></a>kasutaja põhimakse.
Sarnaselt teenuse peamine eesmärk on kasutatud tähistada rakenduse eksemplari, kasutaja peamine eesmärk on muud tüüpi põhisumma; Turve, mis tähistab kasutaja. Azure'i AD Graphi [Kasutaja olemi] [ AAD-Graph-User-Entity] määratleb skeemi kasutaja objekti, sh kasutaja seotud atribuudid, nagu ees- ja perekonnanimi, kasutajanimi, directory rolli liikmestaatust jne. See pakub kasutaja identiteet konfiguratsiooni Azure AD kasutaja subjekt käitusajal luua. Kasutaja põhisumma kasutatakse tähistada autenditud kasutaja jaoks Ühekordne sisselogimine, salvestamine [nõusoleku](#consent) delegeerimine, juurdepääsu juhtimine otsuseid jne.

## <a name="web-client"></a>Web klient
Teatud tüüpi [klientrakendusega](#client-application) mis käivitab kõik kood veebiserverisse, ja saavad toimida "konfidentsiaalne" kliendi, salvestades selle identimisteabe turvaliselt serveris. Vt [OAuth2 KLIENDITÜÜPIDE ja profiilid] [ OAuth2-Client-Types] rohkem üksikasju.

## <a name="next-steps"></a>Järgmised sammud
[Azure'i AD arendaja juhend] [ AAD-Dev-Guide] on kõik Azure AD arengu kasutada portaali seotud teemade, sh [rakenduste integreerimise] ülevaade[ AAD-How-To-Integrate] ja [Azure AD autentimis- ja toetatud autentimine stsenaariumid]põhitõdesid[AAD-Auth-Scenarios].

Kasutage järgmist Disqus kommentaarides tagasiside ja aidake meil viimistlemine ja kujundamine meie sisu.

<!--Image references-->

<!--Reference style links -->
[AAD-App-Manifest]: ./active-directory-application-manifest.md
[AAD-App-SP-Objects]: ./active-directory-application-objects.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AAD-Graph-User-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity
[AAD-How-Subscriptions-Assoc]: ./active-directory-how-subscriptions-associated-directory.md
[AAD-How-To-Integrate]: ./active-directory-how-to-integrate.md
[AAD-How-To-Tenant]: active-directory-howto-tenant.md
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AAD-Multi-Tenant-Overview]: active-directory-devhowto-multi-tenant-overview.md
[AAD-Security-Token-Claims]: ./active-directory-authentication-scenarios/#claims-in-azure-ad-security-tokens
[AAD-Tokens-Claims]: ./active-directory-token-and-claims.md
[AZURE-classic-portal]: https://manage.windowsazure.com
[Duyshant-Role-Blog]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/
[JWT]: https://tools.ietf.org/html/draft-ietf-oauth-json-web-token-32
[Microsoft-Graph]: https://graph.microsoft.io
[O365-Perm-Ref]: https://msdn.microsoft.com/en-us/office/office365/howto/application-manifest
[OAuth2-Access-Token-Scopes]: https://tools.ietf.org/html/rfc6749#section-3.3
[OAuth2-AuthZ-Endpoint]: https://tools.ietf.org/html/rfc6749#section-3.1
[OAuth2-AuthZ-Grant-Types]: https://tools.ietf.org/html/rfc6749#section-1.3
[OAuth2-Client-Types]: https://tools.ietf.org/html/rfc6749#section-2.1
[OAuth2-Role-Def]: https://tools.ietf.org/html/rfc6749#page-6
[OpenIDConnect]: http://openid.net/specs/openid-connect-core-1_0.html
[OpenIDConnect-AuthZ-Endpoint]: http://openid.net/specs/openid-connect-core-1_0.html#AuthorizationEndpoint
[OpenIDConnect-ID-Token]: http://openid.net/specs/openid-connect-core-1_0.html#IDToken
