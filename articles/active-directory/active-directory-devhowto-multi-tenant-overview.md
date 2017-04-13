<properties
   pageTitle="Kuidas luua rakendus, mida saate sisse logida mis tahes Azure Active Directory kasutaja | Microsoft Azure'i"
   description="Samm-sammult juhised koostamise rakendus, mida saate sisselogimine Azure Active Directory rentniku, nimetatakse ka mitme rentniku rakenduse kasutaja."
   services="active-directory"
   documentationCenter=""
   authors="skwan"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/11/2016"
   ms.author="skwan;bryanla"/>

# <a name="how-to-sign-in-any-azure-active-directory-ad-user-using-the-multi-tenant-application-pattern"></a>Mis tahes Azure Active Directory (AD) kasutaja mustri mitme rentniku rakenduse abil sisselogimine
Kui te tarkvara paljude organisatsioonide teenuse rakendus, saate konfigureerida rakenduse kaudu mis tahes Azure AD rentniku sisselogimist kinnitamiseks.  Azure AD seda nimetatakse, muutes oma rakenduse mitme rentniku.  Mis tahes Azure AD rentniku kasutajate saab rakenduse pärast nõustute oma rakendusega kasutada oma kontole sisse logida.  

Kui teil on olemasolev rakendus, mis on oma konto süsteemi või muud tüüpi sisselogimine muude pilveteenuse pakkujad toetab, mis tahes rentniku lisamise Azure AD Logi sisse on sama lihtne kui rakenduse registreerimisel, Logi lisamise kaudu OAuth2, OpenID ühendamine või SAML-koodi ja paneb on Sisselogimine Microsofti nupp rakenduse. Lisateavet rakenduse sümboolika allpool nuppu.

[! [Logige sisse nupp] [AAD-sisselogimist]][AAD-App-Branding]


Selles artiklis eeldatakse, et olete juba tuttav koostamise Azure AD rentniku ühe taotluse.  Kui olete ei pea tagasi registriredaktori [arendaja juhend kodulehele] [ AAD-Dev-Guide] ja proovige ühte meie kiire käivitamise!

On neli lihtsate rakendusse Azure AD rentniku mitme rakenduse teisendada.

1.  Mitme rentniku olevat registreerimise rakenduse värskendamine
2.  Päringuid saata selle /common-koodi värskendamise lõpp-punkti 
3.  Mitme väljaandja väärtuste käsitlemise-koodi värskendamise
4.  Kasutaja ja administraatori nõusoleku mõista ja sobiv kood muudatused

Vaatame iga toimingu üksikasjalikult. Saate ka liikuda otse [nimekiri selle kohta, mitme rentniku näidised][AAD-Samples-MT].

## <a name="update-registration-to-be-multi-tenant"></a>Mitme rentniku olevat registreerimise
Vaikimisi on web app/API registreerimise Azure AD rentniku ühe.  Saate registreerimise mitme rentniku, leides parameetrit "Rakenduses on mitme rentniku" rakenduse registreerimise lehel konfiguratsioon [Azure klassikaline portaali] [ AZURE-classic-portal] ja seades "Jah".

Märkus: Enne saate teha mitme rentniku, Azure AD nõuab rakenduse ID URI taotluse globaalselt kordumatud. Rakenduse ID URI on üks viise rakendus on tuvastatud sõnumites Protocol (protokoll).  Ühe rentniku rakenduse, piisab rakenduse ID URI olema kordumatu selle rentniku jaoks.  Mitme rentniku rakendusega, tuleb globaalselt kordumatu nii leiate Azure'i AD Rakenduse kõik rentnikud.  Globaalne unikaalsuse on jõustatud nõudva rakenduse ID URI on hosti nimi, mis vastab kinnitatud Domeen Azure AD rentniku.  Näiteks kui teie rentniku nimi oli contoso.onmicrosoft.com siis sobiv rakenduse ID URI oleks `https://contoso.onmicrosoft.com/myapp`.  Kui teie rentnik oli kinnitatud domeen, `contoso.com`, siis oleks lubatud rakenduse ID URI `https://contoso.com/myapp`.  Rakenduse väärtuseks mitme rentniku nurjub, kui rakenduse ID URI ei järgige selles muster.

Omakliendi registreerimise on mitme rentniku vaikimisi.  Te ei pea vastu võtta native kliendi rakenduse registreerimise mitme rentnikuga.

## <a name="update-your-code-to-send-requests-to-common"></a>Päringuid saata /common-koodi värskendamise
Ühe rentniku rakenduses, saadetakse rentniku sisselogimine lõpp-punkti taotlusi sisselogimine.   Näiteks contoso.onmicrosoft.com lõpp-punkti oleks:

    https://login.microsoftonline.com/contoso.onmicrosoft.com

Saadetud a rentniku lõpp-punkti saavad kasutajad (või külalistele) sisse logida selle rentniku selle rakendusi.  Mitme rentniku rakendusega rakendus ei tea ette mis rentniku kasutaja pärineb nii, et te ei saa a rentniku lõpp-punkti päringuid saata.  Selle asemel osalejatele saadetakse kõik Azure AD rentnikud multiplexes lõpp-punkt:

    https://login.microsoftonline.com/common

Kui Azure AD saab taotluse kohta on /common lõpp-punkti, sinna sisse logib kasutaja ja seetõttu leiab mis pärineb kasutaja rentnik.  Funktsiooni/levinud lõpp-punkti töötab koos kõigi autentimise protokollid, mis ei toeta Azure AD: OpenID ühendus, OAuth 2.0, SAML 2.0 ja oli-Federation.

Sisselogimine ning seejärel taotluse sisaldab märgiks, mis tähistab kasutaja.  Luba väärtus väljaandja ütleb rakendus mis pärineb kasutaja rentnik.  Kui vastuse annab selle /common lõpp-punkti, vastab väljaandja luba väärtus kasutaja rentnik.  See on oluline märkida soovitud /common lõpp-punkti ei ole rentniku ja pole mõni väljaandja, on lihtsalt multiplekseri.  /Common kasutamisel rakenduse valideerimiseks sõned loogika tuleb arvesse võtta värskendada. 

Nagu varem mainitud, mitme rentniku ka taotlustele tuleks ühtsete sisselogimine kasutajate pärast Azure AD sümboolika juhised. Lisateavet rakenduse sümboolika allpool nuppu.

[! [Logige sisse nupp] [AAD-sisselogimist]][AAD-App-Branding]

Vaatame lähemalt selle /common kasutamine lõpp-punkti ja teie koodi rakendamine üksikasjalikumalt.

## <a name="update-your-code-to-handle-multiple-issuer-values"></a>Mitme väljaandja väärtuste käsitlemise-koodi värskendamise
Veebirakenduste ja web API-de vastu ja kinnitage sõnet Azure AD.  

> [AZURE.NOTE] Kuigi omakliendi rakenduste taotleda ja Azure AD sõned saadud, nad teha saatmiseks API-d, kus andmed on kinnitatud.  Algsete rakenduste valideerimiseks sõned ja peab neid käsitlema läbipaistmatu.

Vaatame lähemalt, kuidas rakendus kinnitatakse sõned veebisaidil saadud Azure AD.  Ühe rentniku rakenduse toimub üldjuhul lõpp-punkti väärtus, nt:

    https://login.microsoftonline.com/contoso.onmicrosoft.com

ja kasutada seda ehitada metaandmete URL (sel juhul OpenID ühendus), nt:

    https://login.microsoftonline.com/contoso.onmicrosoft.com/.well-known/openid-configuration

kahe kriitiline käivat teavet kasutatakse valideerimiseks sõned allalaadimiseks: rentniku kasutaja sisselogimine võtmed ja väljaandja väärtus.  Iga Azure AD rentniku on kordumatu väljaandja väärtus vorm.

    https://sts.windows.net/31537af4-6d77-4bb9-a681-d2394888ea26/

kui GUID väärtus on ohutu Nimeta versiooni rentniku rentniku ID-d.  Kui klõpsate jaoks linki metaandmete `contoso.onmicrosoft.com`, saate vaadata selle väljaandja väärtus dokumendis.

Kui ühe rentniku rakendus kinnitatakse märgiks, kontrollib allkirja luba vastu metaandmete dokumendi allkirjastamise võtmed ja saab kontrollida, kas luba väärtus väljaandja ühtib, mis leitud metaandmete dokument.

Kuna selle /common lõpp-punkti ei vasta rentniku ja pole uus isik, kui uurite väljaandja väärtus metaandmete/levinud on templated URL-i asemel on tegelik väärtus:

    https://sts.windows.net/{tenantid}/

Seetõttu ei saa mitme rentniku rakenduse valideerida sõned lihtsalt vastendades väljaandja väärtus metaandmete abil soovitud `issuer` luba väärtus.  Mitme rentniku rakenduse loogika otsustada, millised väljaandja väärtused on lubatud ja millised on ei vaja, põhineb rentniku ID osa väljaandja väärtus.  

Näiteks kui mitme rentniku rakendus võimaldab ainult teatud rentnike jaoks, kes on registreerunud teenuse Logi sisse, siis see peab kontrollima väärtus väljaandja või `tid` taotlemine veendumaks, et selle on tellijad loendis luba väärtust.  Kui mitme rentniku rakenduse ainult tegeleb üksikisikute ja ei otsuste mõnda Accessi põhjal rentnikud, siis see võite ignoreerida väljaandja väärtus täielikult eemaldada.

Mitme rentniku näidised leiate käesoleva artikli lõpus [Seotud sisu](#related-content) jaotise, väljaandja valideerimine on keelatud lubamiseks mis tahes Azure AD rentniku sisse logida.

Nüüd vaatame kasutajatele, kellel on sisselogimisega mitme rentniku rakenduste kasutusvõimalused.

## <a name="understanding-user-and-admin-consent"></a>Teave kasutaja ja administraatori nõusolekut
Kasutaja Azure AD rakendusse sisse logida, peab kasutaja rentnik esindatud rakendus.  See võimaldab organisatsiooni näiteks kordumatu poliitikaid rakendada, kui kasutajad saavad rentnikust rakenduse sisse logida.  Ühe rentniku rakenduse see registreerimine on lihtne; See on üks, mis juhtub rakenduse registreerimisel [Azure klassikaline portaali][AZURE-classic-portal].

Mitme rentniku rakenduse, algse registreerimise rakenduse elu Azure AD rentniku kasutatavaid arendaja.  Kui kasutaja on rentnikus sisse logib rakenduse esimest korda, Azure AD küsib nõusolekut taotleja rakenduse õigusi.  Kui ta nõus, siis kujutis rakendus nimega *teenuse põhisumma* luuakse kasutaja rentnik ja logige sisse saate jätkata. Mõne delegeerimine luuakse ka kataloogis, mis salvestab rakendus kasutaja nõusolekut. Vt [rakenduse objektid ja teenuse põhilise objektide] [ AAD-App-SP-Objects] rakenduse rakendus ja ServicePrincipal objekti, ja kuidas need omavahel seotud üksikasju.

![Et ühe taseme rakendus][Consent-Single-Tier] 

See nõusolekut kogemus mõjutab taotleja rakenduse õigusi.  Azure AD toetab kahte tüüpi õigused, vaid rakendus ja delegeeritud.

- Delegeeritud õiguse annab rakenduse võimalus toimida kehtib kasutaja alamhulga asjad, mida kasutaja saab teha.  Näiteks saate anda rakenduse delegeeritud õigus lugeda kehtib kasutaja kalender.
- Kuvatakse ainult rakenduse luba on antud otse rakenduse identiteeti.  Näiteks saate anda rakenduse ainult rakenduse õigus lugeda rentniku kasutajate loendit ja seda saab teha sõltumata sellest, kes on sisse logitud rakendusse.

Mõned õigused saate nõustunud end regulaarne kasutaja, aga vajavad rentniku administraatori nõusolekut. 

### <a name="admin-consent"></a>Administraator nõusolekut
Ainult rakenduse õigused alati vaja rentniku administraatori nõusolekut.  Kui teie rakendus taotleb kuvatakse ainult rakenduse õiguste ja tavaline kasutaja üritab rakendusse sisse logida, saavad rakenduse tõrketeade kasutaja ei saa nõustu.

Teatud delegeeritud õigusi on vaja ka rentniku administraatori nõusolekut.  Näiteks võimalus kirjutada tagasi Azure AD rentniku administraatori nõusolekuta kehtib kasutaja.  Kui tavaline kasutaja proovib rakendus, mis taotleb delegeeritud õigus, mis administraatori nõusolekuta sisse logida ainult rakenduse õigused, nt saavad rakenduse tõrge.  Kas on õigus nõuab administraator nõusolekut määratakse arendaja, mis on avaldatud ressursi ja võib leida ressursi dokumentatsioonist.  Kirjeldab saadaolevaid õigusi Azure AD Graph API ja Microsoft Graphi API teemade lingid on [Seotud sisu](#related-content) jaotises käesoleva artikli.

Kui teie rakendus kasutab õiguste haldus nõusolekut nõudvate, peate olema žest, nt nupu või lingi, kus admin saab algatada toimingu.  Rakenduse saadab see toiming on tavaline OAuth2/OpenID ühenduse luba taotluse, kuid mis sisaldab ka kutse on `prompt=admin_consent` päringu päringustringi parameetri.  Kui soovitud administraator on andnud ja teenuse põhilise luuakse kliendi rentnik, edaspidised taotlused sisselogimine pole vaja selle `prompt=admin_consent` parameeter.   Kuna administraator on otsustanud nõutud õigused on aktsepteeritav, küsitakse nõusolekut hetkest rentniku teisi kasutajaid.

Funktsiooni `prompt=admin_consent` parameeter saate kasutada ka rakendustes, mis taotleda õigused, mis ei nõua administraator nõusolekut, kuid soovite anda kogemus, kus on rentnikuadministraatori "registreerub" rakenduse üks kord ja teisi kasutajaid küsitakse nõusolekut sellega.

Kui rakenduse nõusolekuta administraator ja administraator logib rakenduse, kuid selle `prompt=admin_consent` parameeter ei saadeta, admin saab edukalt nõustu, kuid need kuvatakse ainult nõusolek oma kasutajakonto.  Tavaline kasutajatele endiselt ei saa sisse logida ja nõustu.  See on kasulik, kui soovite anda rentnikuadministraator võimalus enne teiste kasutajate juurdepääs rakenduse uurimine.

Rentnikuadministraator saab keelata tavaline kasutajatele lubada rakendused.  Kui see funktsioon on keelatud, on alati administraatori nõusolek nõutav, mis on loodud rentniku.  Kui soovite testida rakenduse regulaarne kasutaja nõusolekuta keelatud, leiate parameetrit konfiguratsiooni Azure AD rentniku konfigureerimine jaotises [Azure klassikaline portaali][AZURE-classic-portal].

> [AZURE.NOTE] Mõned rakendused soovite kus tavalise kasutajad saavad lubada algselt ja hiljem rakendus võib hõlmata administraator ja taotluse õiguste haldus nõusolekut nõudvate kogemus.  Seal on seda teha ühes rakenduses registreerimise Azure AD täna kuidagi.  On eelseisvad Azure AD v2 lõpp-punkti võimaldab rakenduste käitusajal taotluse õiguste asemel registreerimise ajal, mis võimaldab seda stsenaariumi.  Lisateavet leiate [Azure'i AD Rakenduse mudeli v2 arendaja juhend][AAD-V2-Dev-Guide].

### <a name="consent-and-multi-tier-applications"></a>Nõusolek ja mitmekihilise rakendused
Rakenduse võib-olla mitmetasandiline, igal oma registreerimise Azure AD.  Näiteks kohalikus rakenduses, mis nõuab web API või veebirakenduse mis helistab veebi-API.  Mõlemal juhul (rakenduste või veebirakenduse) klient taotleb õiguste helistamiseks ressursi (veebi-API).  Kliendi on edukalt nõustunud üheks kliendi rentniku jaoks peab kõik ressursid, millele see taotleb õigused juba olemas kliendi rentnik.  Kui see tingimus pole täidetud, Azure AD tagastavad vea, ressurss tuleb lisada esmalt.

See võib olla probleem, kui loogiline rakenduse koosneb kahe või enama rakenduse registreerimine, näiteks eraldi kliendi- ja ressursihalduse.  Kuidas teile ressursi üheks kliendi rentniku esimese?  Azure AD hõlmab sel juhul, võimaldades kliendi- ja ressursihalduse olema andnud ühe juhises, kus näeb kasutaja taotleb kliendi ja ressursside nõusoleku lehel õiguste kogusumma.  Selline käitumine lubamiseks ressursi rakenduse registreerimine peab sisaldama rakenduse kliendi ID, mis on `knownClientApplications` kontrollteave rakendus.  Näiteks:

    knownClientApplications": ["94da0930-763f-45c7-8d26-04d5938baab2"]

Selle atribuudi värskendamiseks kaudu ressursside [rakenduse manifesti][AAD-App-Manifest], ja ilmneb mitmekihilise omakliendi helistades veebi-API valimi [Seotud sisu](#related-content) jaotises käesoleva artikli lõpus. Alloleval joonisel antakse ülevaade nõusolekut mitmekihilise rakenduse:

![Et mitmekihilise teadaolevad kliendi rakendus][Consent-Multi-Tier-Known-Client] 

Sarnase juhtumi juhtub, kui rakenduse erinevatele tasemetele registreeritud eri rentnikke.  Näiteks kaaluda koostamise omakliendi rakendus, mis nõuab Office 365 Exchange Online'i API.  Arendada emakeelena rakendusele ja hiljem kohalikke rakenduse käivitada kliendi rentnik, peab olema Exchange Online'i teenuse põhilise.  Sel juhul on kliendil osta Exchange Online'i teenuse põhisumma luua oma rentniku kohta.  Puhul on loodud mõnes asutuses, kui Microsoft API, API vähegi pakkuda oma klientidele nõusolekut nende taotluse muutmine kliendi rentniku, näiteks veebilehele, mis juhib nõusolekut abil menetlustele, mis on selles artiklis kirjeldatud viisil.  Pärast teenuse põhilise on loodud rentniku, võite rakendus sõned API.

Alloleval joonisel antakse ülevaade nõusolekut mitmekihilise rakenduse erinevate rentnike jaoks registreeritud:

![Nõusolek mitmekihilise mitme osalejaga rakendusse][Consent-Multi-Tier-Multi-Party] 

### <a name="revoking-consent"></a>Nõusoleku tühistamine
Kasutajatele ja administraatoritele saate tühistada nõusoleku suvalisel ajal rakenduse:

- Kasutajate juurdepääsu üksikuid rakendusi tühistada, eemaldades need oma [Accessi paani rakenduste] [ AAD-Access-Panel] loend.
- Administraatorid tühistada juurdepääsu rakenduste eemaldamisega kasutades Azure AD haldamine jaotist [Azure klassikaline portaalis]Azure AD[AZURE-classic-portal].

Kui administraator nõustub rakenduse kõigi rentniku kasutajate, kasutajad ei saa tühistada juurdepääsu ükshaaval.  Ainult administraator, saate tühistada juurdepääsu ja ainult kogu rakenduse jaoks.

### <a name="consent-and-protocol-support"></a>Nõusolek ja protokolli tugi
Nõusolek on toetatud Azure AD OAuthi, OpenID Connect kaudu oli-liitmise ja SAML protokollid.  SAML ja oli-Federation Protokollid ei toeta selle `prompt=admin_consent` parameeter, nii, et administraator nõusolekut on võimalik OAuthi ja OpenID Connect kaudu ainult.

## <a name="multi-tenant-applications-and-caching-access-tokens"></a>Mitme rentniku rakenduste ja vahemällu Accessi sõned
Mitme rentniku rakendusi saate ka Accessi märkide API-d, mis on kaitstud Azure AD.  Levinud viga, kui Active Directory autentimise Raamatukogu (ADAL) kasutamise mitme rentniku rakendusega on esialgu taotleda märgiks kasutaja /common, kasutades saate vastuse ja taotlege seejärel edaspidised märgiks ka kasutades /common sama kasutaja jaoks.  Kuna pärineb vastuse Azure AD rentniku, mitte/levinud, ADAL vahemälu luba on sihtrentnikusse. Järgmise kõne /common saada juurdepääsu sümboolse kasutaja jätab kirje vahemälu ja kasutajalt uuesti sisse logida.  Puuduvad vahemälu vältimiseks veenduge, et edaspidised kõned juba kehtib kasutaja tehtud rentniku lõpp-punkti.

## <a name="related-content"></a>Seotud teemad

- [Mitme rentniku rakenduse näidised][AAD-Samples-MT]
- [Juhised rakenduste sümboolika][AAD-App-Branding]
- [Azure'i AD arendaja juhend][AAD-Dev-Guide]
- [Rakenduse objektid ja teenuse põhilise objektid][AAD-App-SP-Objects]
- [Rakenduste integreerimine Azure Active Directory][AAD-Integrating-Apps]
- [Nõusolek raames ülevaade][AAD-Consent-Overview]
- [Microsoft Graphi API õiguste otsinguulatuste][MSFT-Graph-AAD]
- [Azure'i AD Graph API õiguste otsinguulatuste][AAD-Graph-Perm-Scopes]

Kasutage Disqus kommentaarid allpool olevat jaotist tagasiside ja aidake meil viimistlemine ja kujundamine meie sisu.

<!--Reference style links IN USE -->
[AAD-Access-Panel]:  https://myapps.microsoft.com
[AAD-App-Branding]: ./active-directory-branding-guidelines.md
[AAD-App-Manifest]: ./active-directory-application-manifest.md
[AAD-App-SP-Objects]: ./active-directory-application-objects.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Consent-Overview]: ./active-directory-integrating-applications.md#overview-of-the-consent-framework
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Graph-Overview]: https://azure.microsoft.com/en-us/documentation/articles/active-directory-graph-api/
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AAD-Samples-MT]: https://azure.microsoft.com/documentation/samples/?service=active-directory&term=multitenant
[AAD-Why-To-Integrate]: ./active-directory-how-to-integrate.md
[AZURE-classic-portal]: https://manage.windowsazure.com
[MSFT-Graph-AAD]: https://graph.microsoft.io/en-us/docs/authorization/permission_scopes

<!--Image references-->
[AAD-Sign-In]: ./media/active-directory-devhowto-multi-tenant-overview/sign-in-with-microsoft-light.png
[Consent-Single-Tier]: ./media/active-directory-devhowto-multi-tenant-overview/consent-flow-single-tier.png
[Consent-Multi-Tier-Known-Client]: ./media/active-directory-devhowto-multi-tenant-overview/consent-flow-multi-tier-known-clients.png
[Consent-Multi-Tier-Multi-Party]: ./media/active-directory-devhowto-multi-tenant-overview/consent-flow-multi-tier-multi-party.png

<!--Reference style links -->
[AAD-App-Manifest]: ./active-directory-application-manifest.md
[AAD-App-SP-Objects]: ./active-directory-application-objects.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AAD-Graph-User-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity
[AAD-How-To-Integrate]: ./active-directory-how-to-integrate.md
[AAD-Security-Token-Claims]: ./active-directory-authentication-scenarios/#claims-in-azure-ad-security-tokens
[AAD-Tokens-Claims]: ./active-directory-token-and-claims.md
[AAD-V2-Dev-Guide]: ./active-directory-appmodel-v2-overview.md
[AZURE-classic-portal]: https://manage.windowsazure.com
[Duyshant-Role-Blog]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/
[JWT]: https://tools.ietf.org/html/draft-ietf-oauth-json-web-token-32
[O365-Perm-Ref]: https://msdn.microsoft.com/en-us/office/office365/howto/application-manifest
[OAuth2-Access-Token-Scopes]: https://tools.ietf.org/html/rfc6749#section-3.3
[OAuth2-AuthZ-Code-Grant-Flow]: https://msdn.microsoft.com/library/azure/dn645542.aspx
[OAuth2-AuthZ-Grant-Types]: https://tools.ietf.org/html/rfc6749#section-1.3 
[OAuth2-Client-Types]: https://tools.ietf.org/html/rfc6749#section-2.1
[OAuth2-Role-Def]: https://tools.ietf.org/html/rfc6749#page-6
[OpenIDConnect]: http://openid.net/specs/openid-connect-core-1_0.html
[OpenIDConnect-ID-Token]: http://openid.net/specs/openid-connect-core-1_0.html#IDToken














