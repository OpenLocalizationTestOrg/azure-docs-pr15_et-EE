
<properties
   pageTitle="Autentimise stsenaariumid Azure AD | Microsoft Azure'i"
   description="Viis kõige levinum autentimise stsenaariumi jaoks Azure Active Directory (AAD) ülevaade"
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin"/>

# <a name="authentication-scenarios-for-azure-ad"></a>Azure AD autentimise stsenaariumid

Azure Active Directory (Azure AD) lihtsustab autentimise arendajatele esitada identiteedi teenus, mis industry-standard protokolle, näiteks OAuth 2.0 ja OpenID ühenduse loomine toetab, samuti avatud allikana teekide erinevad platvormid, mis aitavad teil alustada kodeerimise kiiresti. Selle dokumendi aitavad teil mõista erinevate stsenaariumid Azure AD toetab ja näitab teile, kuidas alustada. See on jagatud järgnevalt:

- [Põhiteave Azure AD autentimine](#basics-of-authentication-in-azure-ad)

- [Nõuete Azure AD turvalisus sõned](#claims-in-azure-ad-security-tokens)

- [Rakenduse registreerimisel Azure AD põhialused](#basics-of-registering-an-application-in-azure-ad)

- [Rakenduse tüübid ja stsenaariumid](#application-types-and-scenarios)

  - [Veebirakenduse veebibrauser](#web-browser-to-web-application)

  - [Ühelt leheküljelt rakenduse (SPA)](#single-page-application-spa)

  - [Algne rakendus veebi-API](#native-application-to-web-api)

  - [Veebirakenduse veebi-API](#web-application-to-web-api)

  - [Daemon või serverirakenduse veebi-API](#daemon-or-server-application-to-web-api)



## <a name="basics-of-authentication-in-azure-ad"></a>Põhiteave Azure AD autentimine

Kui olete varem selliseid põhimõtted autentimise Azure AD, lugege seda jaotist. Muul juhul võite vahele jätta allapoole [taotluse tüübid ja stsenaariumid](#application-types-and-scenarios).

Vaatame kõige olulisemaid stsenaarium, kus on nõutud: veebirakenduse autentimiseks peab kasutaja veebibrauseris. Selle stsenaariumi on kirjeldatud Täpsemalt jaotises [Veebirakenduse veebibrauser](#web-browser-to-web-application) , kuid see on kasulik alguspunkti, mis illustreerivad Azure AD võimaluste ja ettekujutus, kuidas seda stsenaariumi töötab. Vaatame seda järgmisel joonisel.

![Sisselogimise veebirakenduse ülevaade](./media/active-directory-authentication-scenarios/basics_of_auth_in_aad.png)

Diagrammi kohal meeles pidada, siin on peate teadma selle erinevad osad:

- Azure AD on identiteedipakkuja kasutajatele ja rakendused, mis on olemas ettevõtte nimistust identiteedi ja lõpuks väljasta turvalisus lube kinnitamisel eduka nende kasutajate ja rakenduste eest.


- Rakendus, mis soovib tellida autentimise Azure AD peab olema registreeritud Azure AD, mis registreerib ja see tuvastab kordumatult rakenduse kataloogis.


- Arendajad saate avatud allika Azure AD autentimise teekide abil lihtsustamiseks autentimise teostavaid protokolli üksikasjad teile. Lisateavet leiate [Azure'i Active Directory autentimise teekide](active-directory-authentication-libraries.md) .


•, Kui kasutaja on autenditud, rakenduse peate kontrollima kasutaja Turbeloa tagamaks, et autentimine õnnestus ettenähtud isikutele. Arendajad saate kasutada pakutavaid autentimise teekide toime mis tahes luba: Azure'i AD, sh JSON Web sõned (JWT) või SAML 2.0 valideerimine. Kui soovite teha valideerimine käsitsi, lugege teemat [JWT Turbeloa sündmuseohjuri](https://msdn.microsoft.com/library/dn205065.aspx) dokumentatsiooni kohta.


> [AZURE.IMPORTANT] Azure AD kasutab avaliku võtme krüptograafia sõned sisse ja veenduge, et need on lubatud. Saate vaadata [Olulist teavet kohta sisselogimise klahvi edastamiseks Azure AD](active-directory-signing-key-rollover.md) kohta lisateabe saamiseks vajalikud loogika peab teil olema rakenduse tagamaks, et see on alati uuendatud uusim võtmed.


• Taotlusi ja koosolekutaotluste autentimise protsessi kulgemist määrab autentimise protokoll, mida kasutati, nt OAuth 2.0 OpenID Connect oli-liitmist või SAML 2.0. Need protokollid käsitletakse teemat [Azure Active Directory autentimine Protokollid](active-directory-authentication-protocols.md) ja allpool lähemalt.

> [AZURE.NOTE] Azure AD toetab OAuth 2.0 ja OpenID Connect standardid, mis võimaldavad kasutada esitaja sõned, sh esitaja sõned JWTs arvuna. Esitaja märgiks on kerge Turbeloa, mis annab "esitaja" juurdepääs kaitstud ressursiga. Selles mõttes on "esitaja" igal pool, mida saab esitada luba. Kuigi peole tuleb esmalt autentimiseks Azure AD vastuvõtmiseks esitaja luba, kui ei võeta nõutud toimingute tagamiseks on luba edastamine, saab see kinni ja kasutada ootamatuid poole. Mõned turvalisus sõned on sisseehitatud süsteem volitamata isikutele takistades abil neid, esitaja sõned ei ole see süsteem ja turvalise kanali, nt transpordikihi Turve (HTTPS) vedamine peab. Kui esitaja luba edastatakse selge, on mees-keskmisel rünnak saab pahatahtlik isik hankida luba ja kasutage seda volitamata juurdepääsu kaitstud ressursi. Turvalisus põhimõtted rakendada, kui salvestamise või esitaja sõned hilisemaks kasutamiseks vahemällu. Alati veenduge, et teie rakendus edastab ja salvestab esitaja sõned turvaline viisil. Vaadake veel turvakaalutlused esitaja sõned, [RFC 6750 jaotise 5](http://tools.ietf.org/html/rfc6750).


Nüüd, kui teil on ülevaate leiate altpoolt, lugege allpool mõista, kuidas ettevalmistamise töötab Azure AD ja Levinumad stsenaariumid Azure AD toetab.


## <a name="claims-in-azure-ad-security-tokens"></a>Nõuete Azure AD turvalisus sõned

Turvalisus sõned välja Azure AD sisaldavad taotluste või argumentide suhtes, mis on autenditud kohta käiva teabe. Nende nõuete saate kasutada rakenduse eri toimingute jaoks. Näiteks saate neid kasutada valideerimiseks luba, selle teema directory rentniku tuvastamine, kasutaja teabe kuvamiseks, määrata selle teema autoriseerimine ja muudeks tegevusteks. Mis tahes antud Turbeloa kohal nõuded on sõltub luba autentida kasutaja ja rakenduse konfigureerimise kasutatavat tüüpi tüüpi. Järgmises tabelis on esitatud erinevat tüüpi kiiratava Azure AD taotluste lühikirjeldus. Lisateabe saamiseks vaadake [toetatud Turbeloa ja taotluste tüübid](active-directory-token-and-claims.md).


| Nõue | Kirjeldus |
|-------|-------------|
| Rakenduse ID | Tuvastab rakendus, mis kasutab luba.
| Sihtrühma | Tuvastab adressaatide ressursi luba on mõeldud. |
| Rakenduse autentimise kontekstis klassi viide | Näitab, kuidas kliendi oli autenditud (avaliku klient vs konfidentsiaalse klient). |
| Kiirsõnumite autentimine | Kirjete kuupäev ja kellaaeg, millal ilmnes autentimine. |
| Autentimismeetodi määramine | Näitab, kuidas luba teema kinnitamist (parooli, sert). |
| Eesnimi | Azure AD pakub komplekti kasutaja eesnimi. |
| Rühmad | Sisaldab objekti ID-d Azure AD rühmade kasutaja on liige. |
| Identiteedipakkuja | Kirjete identiteedipakkuja juures, mille teema luba autenditud. |
| Välja | Kirjete aeg, kus on antud luba, kasutatakse sageli Turbeloa värskus. |
| Väljaandja | Tuvastab STS, mille kiiratav luba kui ka Azure AD rentniku. |
| Perekonnanimi | Azure AD pakub komplekti kasutaja perekonnanimi. |
| Nimi | Inimeste loetav väärtus, mis tuvastab luba teema pakub. |
| Objekti Id | Sisaldab teema püsiv, kordumatu identifikaatori Azure AD. |
| Rollid | Sisaldab Azure AD Rakenduse rollid, mida kasutaja on antud sõbralik nimesid. |
| Ulatus | Näitab õiguste klientrakendust. |
| Teema | Näitab kohta, mis kinnitab luba teabe põhisumma. |
| Rentniku Id | Sisaldab kataloogi rentniku luba väljastanud püsiv, kordumatu identifikaatori. |
| Sümboolne eluiga | Määratleb ajavahemiku, mille jooksul on kehtiv märgiks. |
| Kasutaja turvasubjektinimi | Sisaldab kasutaja nime teema. |
| Versioon | Sisaldab luba versiooninumber. |


## <a name="basics-of-registering-an-application-in-azure-ad"></a>Rakenduse registreerimisel Azure AD põhialused

Mis tahes rakendus, mis tellib autentimise Azure AD registreeritud peab kataloogi. Selles etapis tuleb hõlmab Azure AD räägib rakenduse, sh URL-i, kus see asub, URL-i saada vastuseid pärast autentimist URI tuvastamiseks rakenduse ja palju muud. See teave on nõutud paar peamist põhjust.

- Azure AD peab suhelda rakenduse sisselogimise või vahetamine sõned käsitlemisel koordinaadid. Need on järgmised:

  - Rakenduse ID URI: Identifikaator rakenduse. See väärtus saadetakse Azure AD autentimisel näitamaks, milline rakendus helistaja soovib märgiks jaoks. Lisaks sisaldub seda väärtust luba, et rakendus ei tea, eesmärgiks.


  - Vasta URL-i ja ümber suunata URI: veebi-API või veebirakenduse puhul on vastus URL-i asukoht Azure AD saadab autentimise vastust, sh märgiks, kui autentimine õnnestus. Puhul kohalikus rakenduses, suunake URI on millele suunab Azure AD kasutajaagendi mõne OAuth 2.0 taotluse kordumatu.


  - Kliendi ID: ID rakendus, mis on loodud Azure AD, kui rakendus on registreeritud. Luba kood või luba taotlemisel saadetakse Azure AD autentimisel kliendi ID ja võti.


  - Võti: Key, mis saadetakse koos kliendi ID kui autentimisel Azure AD helistamiseks veebi-API.


- Azure AD peab veenduge, et rakendus on juurdepääs teie asutuse muude rakenduste kataloogi andmete ja muudeks tegevusteks vajalikke õigusi

Ettevalmistamise muutub selgemaks, kui te aru, et on kaht liiki saate välja töötada ja Azure AD integreeritud rakenduste:

- Ühe rentniku rakenduse: ühe rentniku rakendus on mõeldud kasutamiseks ühe ettevõtte. Need on tavaliselt joon, business (LoB) rakenduste enterprise arendaja on kirjutanud. Ühe rentniku rakendus peab ainult ühes kataloogis kasutajad pääsevad juurde ja seetõttu tuleb ainult ühes kataloogis ette valmistada. Need rakendused on tavaliselt registreeritud ettevõte arendaja.


- Mitme rentniku rakenduse: mitme rentniku rakendus on mõeldud kasutamiseks paljude asutuste, mitte ainult ühe ettevõtte. Need on tavaliselt tarkvara-kui-a-service (SaaS) rakenduste kirjutanud sõltumatu tarkvaratarnija (ISV). Mitme rentniku rakenduste tuleb iga kataloogis, kui on kasutada, mis nõuab administraatori nõusolekut registreerida neid ette valmistada. Nõusolek käivitub kui rakendus on registreeritud kataloogis ja on antud juurdepääs Graph API või võib-olla muud veebi-API. Kui kasutaja või muu ettevõtte administraator registreerub rakenduse kasutamiseks, esitatakse need koos dialoogiboksi, mis kuvatakse rakenduse jaoks on vaja õigused. Administraatori saate siis nõustu, mis annab juurdepääsu rakenduse märgitud andmed ja lõpuks registreerib rakendus oma kataloogi. Lisateabe saamiseks vt [Ülevaade nõusoleku raames](active-directory-integrating-applications.md#overview-of-the-consent-framework).

Mitme rentniku rakenduse asemel ühe rentniku rakenduste arendamise tekivad mõned täiendavad kaalutlused. Näiteks, kui teil on rakenduse kasutajatele kättesaadavaks tegemine mitme kataloogidesse, peate süsteem mis rentniku määratlemiseks need asuvad. Ainult ühe rentniku rakenduse peab kataloogi kasutaja, vaadake ajal mitme rentniku nõuab tuvastamiseks mõne kindla kasutaja kõik kataloogid Azure AD. Selle ülesande Azure AD pakub levinud autentimise lõpp-punkti kui mis tahes mitme rentniku rakenduses saate määrata sisselogimist taotlusi, mitte rentniku kohased endpoint. Selle lõpp-punkti on https://login.microsoftonline.com/common kõik kataloogid Azure AD rentniku kohased endpoint osutuda https://login.microsoftonline.com/contoso.onmicrosoft.com. Levinud lõpp-punkti on eriti oluline silmas pidada rakenduse väljatöötamisel, sest teil on vaja vajalikud loogika käsitlema mitme rentniku ajal sisselogimist, väljunud, ja Turbeloa valideerimine.

Kui teil on praegu välja ühe rentniku rakenduse, kuid soovite paljude ettevõtete jaoks kättesaadavaks teha, saate hõlpsasti muudate rakendus ja Azure AD konfiguratsioon oleks mitme rentniku toega. Lisaks Azure AD kasutab sama allkirjastamiseks võti kõigi märkide kõik kataloogid, kas esitate rentniku ühe või mitme rentniku rakenduse autentimist.

Iga stsenaarium, mis on loetletud selles dokumendis sisaldab sub lõik, mis oma ettevalmistamise nõuetele. Lisateavet leiate põhjalikumat teavet rakenduse Azure AD ja ühe või mitme rentniku rakenduste vahelised erinevused ettevalmistamise [Azure Active Directory rakenduste integreerimine](active-directory-integrating-applications.md) . Edasi lugedes mõista rakenduse tavastsenaariumid Azure AD.

## <a name="application-types-and-scenarios"></a>Rakenduse tüübid ja stsenaariumid

Iga dokumendis kirjeldatud juhtudel saate välja töötada kasutamine erinevates keeltes ja platvormid. Need kõik toetavad täieliku koodinäiteid, mis on saadaval, [koodinäiteid juhend](active-directory-code-samples.md)või otse vastavate [Github valimi hoidlate](https://github.com/Azure-Samples?utf8=%E2%9C%93&query=active-directory). Lisaks, kui teie rakendus peab kindlat või lõigust on lõpuni stsenaarium, enamasti funktsioone saab lisada sõltumatult. Näiteks, kui teil on kohalikus rakenduses, mis nõuab veebi-API, saate hõlpsasti lisada veebirakendus, mis nõuab ka veebi-API. Järgmine diagramm näitab, et need stsenaariumid ja taotluse tüübid, ja kuidas saab lisada eri osade:

![Rakenduse tüübid ja stsenaariumid](./media/active-directory-authentication-scenarios/application_types_and_scenarios.png)

Need on viis esmane rakenduse stsenaariumid Azure AD ei toeta.

- [Veebirakenduse veebibrauser](#web-browser-to-web-application): veebirakendus, mis on kaitstud Azure AD sisselogimiseks peab kasutaja.

- [Ühe lehe rakenduse (SPA)](#single-page-application-spa): peab kasutaja ühelt leheküljelt rakendus, mis on kaitstud Azure AD sisse logida.

- [Algne rakendus veebi-API](#native-application-to-web-api): kohalikus rakenduses, mis töötab telefoni, tahvelarvuti või arvuti peab kasutaja saada ressursid veebist API, mis on kaitstud Azure AD autentimiseks.

- [Veebirakenduse Web API](#web-application-to-web-api): veebirakenduse vajab ressursid veebist API tagatud Azure AD.

- [Daemon või serverirakenduse Web API](#daemon-or-server-application-to-web-api): daemon rakenduse või serveri rakendus pole kasutajaliides web vajab ressursid veebist API tagatud Azure AD.

### <a name="web-browser-to-web-application"></a>Veebirakenduse veebibrauser

Selles jaotises kirjeldatakse rakendus, mis autendib kasutaja veebirakenduses veebibrauseris. Selle stsenaariumi korral veebirakenduse suunab kasutaja brauseris sisse Azure AD logida. Azure AD tagastab sisselogimise vastuse kaudu kasutaja brauserit, mis sisaldab taotlusi turvalisuse luba kasutaja kohta. See stsenaarium toetab sisselogimise oli-Federation, SAML 2.0 ja OpenID Connect Protokollid abil.


#### <a name="diagram"></a>Skeem
![Autentimise meilivoo brauseri veebirakenduse](./media/active-directory-authentication-scenarios/web_browser_to_web_api.png)


#### <a name="description-of-protocol-flow"></a>Protokolli voo kirjeldus


1. Kui kasutaja külastamine rakendus ja tuleb sisse logida, nad suunatakse kaudu sisselogimine taotlus autentimise lõpp-punkti Azure AD.


2. Kasutaja sisse logib lehel sisselogimine.


3. Kui autentimine õnnestus, Azure AD loob autentimine on luba ja tagastab sisselogimise vastuse rakenduse vasta URL Azure'i haldusportaal konfigureeritud. Tootmise rakenduse, tuleks see vastus URL-i HTTPS. Tagastatud luba sisaldab nõuete kohta kasutaja ja Azure AD Rakenduse luba kinnitamiseks vajalike.


4. Rakendus kinnitatakse luba abil allkirjastamiseks avalik võti ja väljaandja teavet, mis on saadaval veebisaidil federation metaandmete dokumendi Azure AD. Kui rakendus kinnitatakse luba, Azure AD käivitab uue seansi kasutaja. Selle seansi võimaldab kasutajal rakenduse avamiseks enne selle lõppemist.


#### <a name="code-samples"></a>Koodinäiteid


Lugege teemat proovi kood veebirakenduse stsenaariumid veebibrauseris. Ja kontrollige uuesti sageli--lisame uue näidised kogu aeg. [Veebirakenduse veebibrauser](active-directory-code-samples.md#web-browser-to-web-application).


#### <a name="registering"></a>Registreerimine


- Ühe rentnik: Kui olete just teie ettevõtte jaoks rakenduse hoone, see peab olema registreeritud ettevõtte nimistust Azure haldusportaali abil.


- Mitme rentniku: Kui on rakendus, mida saate kasutada väljaspool teie asutust kasutajate loomine, see tuleb registreerida ettevõtte kataloogi, kuid iga ettevõtte kataloogi, mis on rakenduse abil ka registrisse. Rakenduse oma kataloogi kättesaadavaks muuta, saate lisada oma klientidele, mis võimaldab nõusolekut rakenduse registreerumise protsessi. Kui nad kasutajaks rakenduse, esitatakse need dialoogiboks, mis näitab rakendus nõuab õigusi, ja seejärel suvandit nõustu. Sõltuvalt vajalikke õigusi muu ettevõtte administraator võib vaja nõusolekut. Administraatori nõustub, registreeritakse rakendus oma kataloogi. Lisateabe saamiseks lugege teemat [Azure Active Directory rakenduste integreerimine](active-directory-integrating-applications.md).


#### <a name="token-expiration"></a>Turbeloa aegumise

Kasutaja seansi lõppemist kestuse välja Azure AD luba aegumisel. Rakenduse lühendamiseks selle aja jooksul, kui soovitud, nt väljalogimine kasutajate tegevusetuse perioodi põhjal. Seansi aegumisel palutakse kasutajal uuesti sisse logida.





### <a name="single-page-application-spa"></a>Ühelt leheküljelt rakenduse (SPA)

Selles jaotises kirjeldatakse autentimise ühe lehe rakendus, mis kasutab Azure AD ja OAuth 2.0 peidetud loa andmine tagada selle web API tagasi lõpetamine. Ühe lehe rakendused on tavaliselt struktureeritud JavaScripti esitluse kiht (esiosa), mis töötab brauseri ja veebi-API tagasi lõpuks, mis töötab serveris ja rakendab rakenduse äriloogika. Peidetud autoriseerimine rohkem teada anda, ja aitab teil otsustada, kas see on õige rakendus stsenaariumist vt [mõistmine Azure Active Directory kulgemist OAuth2 peidetud anda](active-directory-dev-understanding-oauth2-implicit-grant.md).

Sel juhul, kui kasutaja märke, JavaScript ees lõppkasutajad [Active Directory Authentication Library JavaScript (ADAL. JS)](https://github.com/AzureAD/azure-activedirectory-library-for-js/tree/dev) ja peidetud loa andmine saada mõne ID luba (id_token) Azure AD. Luba vahemällu ja kliendi manustab taotluse esitaja luba nimega tegemisel uuesti Lõpeta, mis on kinnitatud, kasutades OWIN vahevara oma veebi-API kõned. 
#### <a name="diagram"></a>Skeem

![Ühe lehe rakenduse skeem](./media/active-directory-authentication-scenarios/single_page_app.png)

#### <a name="description-of-protocol-flow"></a>Protokolli voo kirjeldus

1. Kasutaja liigub veebirakenduse.


2. Rakendus annab brauseri JavaScripti esiosa (esitluse layer).


3. Kasutaja alustab näiteks sisselogimine, klõpsates linki märk. Brauseri saadab saamiseks Azure AD autoriseerimine lõpp-punkti ID on luba taotleda. Taotlus hõlmab kliendi ID ja vasta URL-i päringu parameetrid.


4. Azure AD kinnitatakse vastus URL-i registreeritud vasta URL-i Azure'i haldusportaal konfigureeritud suhtes.


5. Kasutaja sisse logib lehel sisselogimine.


6. Kui autentimine õnnestus, Azure AD loob ID on luba ja tagastab selle URL-i fragment (#) rakenduse vastus URL-i. Tootmise rakenduse, tuleks see vastus URL-i HTTPS. Tagastatud luba sisaldab nõuete kohta kasutaja ja Azure AD Rakenduse luba kinnitamiseks vajalike.


7. JavaScripti kliendi koodi, mis töötab brauseris ekstraktib luba vastuse kasutamine turvaliseks API tagasi lõpetamine rakenduse web kõned.


8. Brauseri kõned rakenduse web API tagasi lõpus juurdepääsu luba päises autoriseerimine.

#### <a name="code-samples"></a>Koodinäiteid


Lugege teemat koodinäiteid ühe lehe rakenduse (SPA) stsenaariumid. Veenduge, et sageli--lisame uue näidised kogu aeg. [Ühelt leheküljelt rakenduse (SPA)](active-directory-code-samples.md#single-page-application-spa).


#### <a name="registering"></a>Registreerimine


- Ühe rentnik: Kui olete just teie ettevõtte jaoks rakenduse hoone, see peab olema registreeritud ettevõtte nimistust Azure haldusportaali abil.


- Mitme rentniku: Kui on rakendus, mida saate kasutada väljaspool teie asutust kasutajate loomine, see tuleb registreerida ettevõtte kataloogi, kuid iga ettevõtte kataloogi, mis on rakenduse abil ka registrisse. Rakenduse oma kataloogi kättesaadavaks muuta, saate lisada oma klientidele, mis võimaldab nõusolekut rakenduse registreerumise protsessi. Kui nad kasutajaks rakenduse, esitatakse need dialoogiboks, mis näitab rakendus nõuab õigusi, ja seejärel suvandit nõustu. Sõltuvalt vajalikke õigusi muu ettevõtte administraator võib vaja nõusolekut. Administraatori nõustub, registreeritakse rakendus oma kataloogi. Lisateabe saamiseks lugege teemat [Azure Active Directory rakenduste integreerimine](active-directory-integrating-applications.md).

Pärast registreerumist rakendus, tuleb see konfigureerida kasutama OAuthi 2.0 peidetud anda protokolli. Vaikimisi on keelatud protokolli rakendusi. Rakenduse OAuth2 peidetud anda protokolli lubamiseks selle rakenduse manifesti alla laadida Azure'i haldusportaal, "oauth2AllowImplicitFlow" väärtuseks true ja laadige manifest tagasi portaali. Üksikasjalikud juhised leiate teemast [Lubamine OAuthi 2.0 peidetud anda ühe lehe rakendusi](active-directory-integrating-applications.md).


#### <a name="token-expiration"></a>Turbeloa aegumise

Kui ADAL.js abil saate hallata Azure AD autentimine, kasu mitmesuguseid funktsioone, mis hõlbustavad värskendamine on aegunud luba samuti saada sõned täiendavad veebi API ressursid, mida nimetatakse rakendus. Kui kasutaja on edukalt autendib Azure AD, seatakse tagatud küpsis seansi kasutaja brauseri ja Azure AD vahel. See on oluline märkida seansi on olemas kasutaja ja Azure AD vahel pole kasutaja ja serveris veebirakenduse vahel. Märgiks aegumisel ADAL.js kasutab selle seansi vaikselt uus luba. See, et see peidetud IFRAME'i abil saata ja vastu võtta taotluse OAuthi peidetud anda protokolli abil. ADAL.js saate kasutada ka see sama süsteem vaikselt saada juurdepääsu sõned Azure AD muud Web API ressursse, mis rakenduse kõned seni, kuni need ressursid toetavad rist-päritolu ressursside jagamise (CORS) on juba registreeritud kasutaja kataloogi ja mõni nõutav nõusolekut anti kasutaja sisselogimisel.


### <a name="native-application-to-web-api"></a>Algne rakendus veebi-API


Selles jaotises kirjeldatakse kohalikus rakenduses, mis nõuab web API kasutaja nimel. See stsenaarium põhineb OAuth 2.0 autoriseerimine koodi anda tüüp avaliku kliendiga, nagu on kirjeldatud jaotises 4.1 [OAuth 2.0 määratlus](http://tools.ietf.org/html/rfc6749). Rakendus hangib juurdepääsu sümboolse kasutaja OAuth 2.0 protokolli abil. Selle juurdepääsu luba saadetakse siis taotluse veebis API, mis lubab kasutaja ja tagastab soovitud ressurss.

#### <a name="diagram"></a>Skeem

![Algne rakendus Web API skeem](./media/active-directory-authentication-scenarios/native_app_to_web_api.png)

#### <a name="authentication-flow-for-native-application-to-api"></a>Algne rakendus API liikumise autentimine

#### <a name="description-of-protocol-flow"></a>Protokolli voo kirjeldus


Kui kasutate AD autentimise teekide, käsitletakse enamik allpool kirjeldatud protokolli üksikasjad teile, nt brauseri hüpikakna, Turbeloa vahemällu talletamine ja Värskenda sõned käsitlemine.

1. Brauseris hüpikakende rakendus esitab taotluse luba lõpp-punkti Azure AD. See taotlus sisaldab kliendi ID ja redirect URI rakendus, nagu on näidatud portaalis haldus ja veebi-API taotluse ID URI. Kui kasutaja pole juba sisse logitud, siis palutakse uuesti sisse logida


2. Azure AD autendib kasutaja. Kui see on mitme rentniku rakendus ja on vaja rakendust nõusolekut, peavad kasutaja nõusolekut, kui nad pole seda juba teinud. Pärast loa andmist ja kinnitamisel eduka Azure AD probleemid vastuse autoriseerimine naasmiseks kliendi rakendus redirect URI.


3. Kui Azure AD probleemid vastuse autoriseerimine naasmiseks redirect URI, klientrakendust lõpetab brauseri ja luba kood ekstraktib vastus. Luba koodiga selle klientrakenduse saadab taotluse Azure AD Turbeloa lõpp, mis sisaldab luba kood, andmeid klientrakendust (kliendi ID ja redirect URI) ja soovitud ressursi (ID URI taotlus veebi-API).


4. Luba kood ja teavet kliendi ja veebi-API kinnitanud Azure AD. Eduka kinnitamisel Azure AD tagastab kahe sõned: JWT juurdepääsu luba ja märgiks JWT värskendamine. Lisaks Azure AD annab põhiteavet kasutajat, nt kuvatav nimi ja rentniku ID.


5. Https, kasutab klientrakenduse lisamiseks autoriseerimine päises taotluse JWT stringiga "Esitaja" nimetuse veebi-API tagastatud JWT juurdepääsu luba. Veebi-API seejärel kinnitatakse JWT luba, ja kui kinnitamine õnnestus, tagastab soovitud ressurss.


6. Juurdepääsu luba aegumisel klientrakenduse kuvatakse tõrge, mis näitab, et kasutaja peab kinnitamiseks uuesti. Kui rakendus on lubatud värskendamise luba, saate seda kasutada omandada uue juurdepääsu luba küsimata kasutajal uuesti sisse logida. Kui värskendamine luba on aegunud, peate rakenduse interaktiivseks autentida taas.


> [AZURE.NOTE] Värskenda luba väljastatud Azure AD saab juurdepääsu mitme ressurssidele. Näiteks, kui teil on klientrakendus, millel on kaks web API kõne õigus, saab värskendamise luba saada juurdepääsu Turbeloa muude veebi API ka.


#### <a name="code-samples"></a>Koodinäiteid


Vaadake Veebiteenuste stsenaariumid algne rakendus proovi kood. Ja kontrollige uuesti sageli--lisame uue näidised kogu aeg. [Algne rakendus veebi-API](active-directory-code-samples.md#native-application-to-web-api).


#### <a name="registering"></a>Registreerimine


- Ühe rentnik: Nii emakeelena ja veebi API peab registreeritud samas kaustas Azure AD. Veebi-API saab konfigureerida esitamist kogumi õigused, mida kasutatakse kohalikus rakenduses oma ressursse juurdepääsu piirata. Seejärel klientrakendusega klõpsab soovitud õigused Azure'i haldusportaal rippmenüü "Õigused soovite muud rakendused".


- Mitme rentniku: Esmalt rakendus ainult kunagi varem registreeritud arendaja või avaldaja kataloogist. Teiseks rakendus on konfigureeritud, näitamaks, et see nõuab funktsionaalne õigused. See vajalike õiguste loend on dialoogiboksis kui kasutaja või administraator sihtkoha kaustas nõusoleku rakendus, mis teeb selle kättesaadavaks oma organisatsiooni. Mõned rakenduste puhul saate asutuses iga kasutaja nõusolek lihtsalt kasutajatasandi õigused. Muude rakenduste jaoks on vaja administraatori tasemel õigused, mille kasutaja ettevõte ei nõustu. Directory administraator saab anda nõusoleku rakendusi, mis nõuavad selle õiguste tase. Administraatori nõustub, registreeritakse web API oma kataloogi. Lisateabe saamiseks lugege teemat [Azure Active Directory rakenduste integreerimine](active-directory-integrating-applications.md).


#### <a name="token-expiration"></a>Turbeloa aegumise


Kui rakendus kasutab selle kood saada JWT Accessi Turbeloa, saab ka märgiks JWT värskendamine. Luba juurdepääs aegumisel saab värskendamise luba uuesti autentida seda uuesti sisse logida. Selle värskenduse märgiks kasutatakse autentida, mille tulemuseks on uue juurdepääsu luba ja Värskenda luba.





### <a name="web-application-to-web-api"></a>Veebirakenduse veebi-API


Selles jaotises kirjeldatakse veebirakenduse, mis tuleb saada ressursid veebi-API. Selle stsenaariumi korral on kaks identiteedi autentimiseks ja kõne veebi-API veebirakenduse kasutavate: rakenduse identiteedi või delegeeritud kasutaja identiteet.

*Rakenduse identiteedi:* Sel juhul kasutab OAuth 2.0 kliendi identimisteabe anda taotluse autentimiseks ja juurdepääs veebi-API. Rakenduse identiteedi veebi API ainult tuvastab, et veebirakenduse helistab, kasutades näiteks web API ei saa kasutaja teavet. Kui rakendus saab kasutaja kohta teavet, saadetakse see rakendus protokolli ja ei ole allkirjastanud Azure AD. Veebi-API loodab, et veebirakenduse autenditud kasutaja. Seetõttu nimetatakse seda mudelit usaldusväärse alamsüsteemi.

*Delegeeritud kasutaja identiteet:* Selle stsenaariumi suunamist kahel viisil: OpenID ühendamine ja OAuth 2.0 autoriseerimine koodi anda konfidentsiaalset kliendiga. Veebirakenduse hangib juurdepääsu sümboolse osutub veebi-API, mis on antud edukalt autenditud kasutaja veebirakendusse ja veebirakenduse on võimalik saada delegeeritud kasutaja identiteet helistamiseks veebi-API kasutaja jaoks. Selle juurdepääsu luba saadetakse taotluse veebis API, mis lubab kasutaja ja tagastab soovitud ressurss.

#### <a name="diagram"></a>Skeem

![Veebirakenduse Veebiteenuste skeem](./media/active-directory-authentication-scenarios/web_app_to_web_api.png)



#### <a name="description-of-protocol-flow"></a>Protokolli voo kirjeldus

Nii rakenduse identiteedi ja delegeeritud kasutaja identiteet tüüpi arutatakse allpool voogu. Võtme vahe neid on, et delegeeritud kasutaja identiteet peate esmalt hankida luba näeb välja umbes järgmine enne kasutaja sisselogimine ja sellega pääseda veebi-API.

##### <a name="application-identity-with-oauth-20-client-credentials-grant"></a>Rakenduse identiteedi OAuthi 2.0 kliendiga identimisteabe andmine

1. Kasutaja on sisse logitud Azure AD veebirakenduse (vt teemat [Veebirakenduse veebibrauser](#web-browser-to-web-application) ülaltoodud).


2. Veebirakenduse peab omandada juurdepääsu sümboolse, nii et see saab autentida veebi-API ja tuua soovitud ressurss. See muudab taotluse Azure AD Turbeloa lõpp-punkti, pakkudes mandaat, kliendi ID ja API veebirakenduse ID URI.


3. Azure AD autendib rakendus ja tagastab JWT juurdepääsu luba kasutatava veebi-API kõne.


4. Https, kasutab veebirakenduse JWT stringiga "Esitaja" nimetuse taotluse päises autoriseerimine lisamiseks veebi-API tagastatud JWT juurdepääsu luba. Veebi-API seejärel kinnitatakse JWT luba, ja kui kinnitamine õnnestus, tagastab soovitud ressurss.

##### <a name="delegated-user-identity-with-openid-connect"></a>Delegeeritud kasutaja identiteedi OpenID ühenduse loomine

1. Kasutaja on sisse logitud veebirakenduse Azure AD abil (vt [Veebibrauser veebirakenduse](#web-browser-to-web-application) ülaltoodud). Kui veebirakenduse veebi-API nimel helistamiseks võimaldab kasutajal veebirakenduse veel ei ole nõus, peate selle kasutaja nõusolekut. Rakendus kuvatakse see nõuab õiguste ja kui mõni nendest on administraatori tasemel õiguste, kataloogis tavaline kasutaja ei saa nõusolekut. Nõusolekut selle protsessi kehtib ainult rakenduste mitme rentniku, mitte ühe rentniku rakendused, kui rakendus on juba vajalikke õigusi. Kui kasutaja on sisse logitud, veebirakenduse saanud mõne ID luba teavet kasutaja, samuti autoriseerimine näeb välja umbes järgmine.


2. Luba koodi välja Azure AD kasutamisel veebirakenduse saadab taotluse Azure AD Turbeloa lõpp, mis sisaldab luba kood, andmeid klientrakendust (kliendi ID ja redirect URI) ja soovitud ressursi (ID URI taotlus veebi-API).


3. Luba kood ja teavet veebirakenduse ja veebi-API kinnitanud Azure AD. Eduka kinnitamisel Azure AD tagastab kahe sõned: JWT juurdepääsu luba ja märgiks JWT värskendamine.


4. Https, kasutab veebirakenduse tagastatud JWT juurdepääsu luba, et lisada JWT string "Esitaja" nimetuse taotluse päises autoriseerimine veebi-API. Veebi-API seejärel kinnitatakse JWT luba, ja kui kinnitamine õnnestus, tagastab soovitud ressurss.

##### <a name="delegated-user-identity-with-oauth-20-authorization-code-grant"></a>Delegeeritud kasutaja identiteedi OAuthi 2.0 autoriseerimine koodi andmine

1. Kasutaja on juba sisse logitud veebirakenduses, mille autentimine on Azure AD sõltumatu.


2. Veebirakenduse jaoks on vaja mõne kood hankida juurdepääsu sümboolse, nii et see probleemid taotluse brauseri kaudu Azure AD autoriseerimine lõpp-punkti, pakkudes kliendi ID ja suunavad URI veebirakenduse pärast eduka autentimist. Kasutaja sisse logib Azure AD.


3. Kui veebirakenduse veebi-API nimel helistamiseks võimaldab kasutajal veebirakenduse veel ei ole nõus, peate selle kasutaja nõusolekut. Rakendus kuvatakse see nõuab õiguste ja kui mõni nendest on administraatori tasemel õiguste, kataloogis tavaline kasutaja ei saa nõusolekut. Nõusolekut selle protsessi kehtib ainult rakenduste mitme rentniku, mitte ühe rentniku rakendused, kui rakendus on juba vajalikke õigusi.


4. Pärast seda, kui kasutaja on andnud, saab veebirakenduse autoriseerimine koodis on vaja juurdepääsu sümboolse hankimine.


5. Luba koodi välja Azure AD kasutamisel veebirakenduse saadab taotluse Azure AD Turbeloa lõpp, mis sisaldab luba kood, andmeid klientrakendust (kliendi ID ja redirect URI) ja soovitud ressursi (ID URI taotlus veebi-API).


6. Luba kood ja teavet veebirakenduse ja veebi-API kinnitanud Azure AD. Eduka kinnitamisel Azure AD tagastab kahe sõned: JWT juurdepääsu luba ja märgiks JWT värskendamine.


7. Https, kasutab veebirakenduse tagastatud JWT juurdepääsu luba, et lisada JWT string "Esitaja" nimetuse taotluse päises autoriseerimine veebi-API. Veebi-API seejärel kinnitatakse JWT luba, ja kui kinnitamine õnnestus, tagastab soovitud ressurss.

#### <a name="code-samples"></a>Koodinäiteid

Vaadake Veebiteenuste stsenaariumid veebirakenduse proovi kood. Ja kontrollige uuesti sageli--lisame uue näidised kogu aeg. Web [rakenduse veebi-API](active-directory-code-samples.md#web-application-to-web-api).


#### <a name="registering"></a>Registreerimine

- Ühe rentnik: Nii rakenduse identiteedi ja delegeeritud kasutaja identiteet juhtudel, veebirakenduse ja veebi-API peab registreeritakse samas kaustas Azure AD. Veebi-API saab konfigureerida esitamist kogumi õigused, mida kasutatakse veebirakenduse ressursside juurdepääsu piirata. Kui kasutusel on delegeeritud kasutaja identiteet tüüp, veebirakenduse tuleb valida soovitud õigused Azure'i haldusportaal rippmenüü "Õigused soovite muud rakendused". See toiming pole nõutav, kui kasutusel on rakenduse identiteedi tüüp.


- Mitme rentniku: Esmalt veebirakenduse on konfigureeritud näitamaks, et see nõuab funktsionaalne õigused. See vajalike õiguste loend on dialoogiboksis kui kasutaja või administraator sihtkoha kaustas nõusoleku rakendus, mis teeb selle kättesaadavaks oma organisatsiooni. Mõned rakenduste puhul saate asutuses iga kasutaja nõusolek lihtsalt kasutajatasandi õigused. Muude rakenduste jaoks on vaja administraatori tasemel õigused, mille kasutaja ettevõte ei nõustu. Directory administraator saab anda nõusoleku rakendusi, mis nõuavad selle õiguste tase. Kui administraatori nõustub, veebirakenduse ja veebi-API on mõlemad registreeritud oma kataloogi.

#### <a name="token-expiration"></a>Turbeloa aegumise

Kui veebirakenduse kasutab selle kood saada JWT Accessi Turbeloa, saab ka märgiks JWT värskendamine. Luba juurdepääs aegumisel saab värskendamise luba uuesti autentida seda uuesti sisse logida. Selle värskenduse märgiks kasutatakse autentida, mille tulemuseks on uue juurdepääsu luba ja Värskenda luba.


### <a name="daemon-or-server-application-to-web-api"></a>Daemon või serverirakenduse veebi-API


Selles jaotises kirjeldatakse daemon või serveri rakendus, mis tuleb saada ressursid veebi-API. On kaks sub stsenaariumi, mis rakenduvad selles jaotises: daemon, mis tuleb kõne on veebi-API, tugineb OAuth 2.0 identimisteabe anda klienditüübi; ja serverirakenduse (nt web API), mis tuleb kõne veebi-API, ehitatud OAuthi 2.0 On-Behalf-Of mustand määratlus.

Stsenaariumi kui daemon nõuab helistamiseks veebi-API, on oluline mõista paari asja. Esmalt kasutaja suhtluse ei ole võimalik daemon rakendusega, mille jaoks on vaja on oma identiteedi. Näide daemon rakendus on pakett-töö või teenuse operatsioonisüsteemi taustal. Seda tüüpi rakendus taotleb juurdepääsu sümboolse, kasutades oma rakenduse identiteedi ja esitamine oma kliendi ID, mandaadi (parooli või sert) ja rakendus ID URI Azure AD. Pärast eduka autentimisel daemon saab juurdepääsu sümboolse Azure AD, mida kasutatakse helistamiseks veebi-API.

Stsenaariumi, kui server nõuab helistamiseks veebi-API, on kasulik kasutamise näide. Oletage, et kohalikke rakendus on autenditud kasutaja ja selle kohalikus rakenduses peab olema veebi-API kõne. Azure AD probleemid JWT juurdepääsu luba helistamiseks veebi-API. Kui veebi-API peab kõne teisele järgneval veebi-API, selle voo nimel-kohta abil volitatud kasutaja identiteedi ja teise taseme veebi-API autentida.

#### <a name="diagram"></a>Skeem

![Daemon või serverirakenduse Veebiteenuste diagrammile](./media/active-directory-authentication-scenarios/daemon_server_app_to_web_api.png)

#### <a name="description-of-protocol-flow"></a>Protokolli voo kirjeldus

##### <a name="application-identity-with-oauth-20-client-credentials-grant"></a>Rakenduse identiteedi OAuthi 2.0 kliendiga identimisteabe andmine

1. Kõigepealt peab serveri rakendus Azure AD iseendana, mis tahes inimeste vahel, nt interaktiivse sisselogimise dialoogiboksi ilma autentimiseks. See muudab taotluse Azure AD Turbeloa lõpp-punkti, pakkudes identimisteavet, kliendi ID ja rakenduste ID URI.


2. Azure AD autendib rakendus ja tagastab JWT juurdepääsu luba kasutatava veebi-API kõne.


3. Https, kasutab veebirakenduse tagastatud JWT juurdepääsu luba, et lisada JWT string "Esitaja" nimetuse taotluse päises autoriseerimine veebi-API. Veebi-API seejärel kinnitatakse JWT luba, ja kui kinnitamine õnnestus, tagastab soovitud ressurss.


##### <a name="delegated-user-identity-with-oauth-20-on-behalf-of-draft-specification"></a>Delegeeritud kasutaja identiteet OAuthi 2.0-nimel-kohta mustand spetsifikatsioonile

Allpool voogu eeldatakse, et klõpsake mõnda muusse rakendusse (nt mõne kohalikus rakenduses) on autenditud kasutaja, ja nende kasutaja identiteet on kasutatud omandada juurdepääsu sümboolse esimese taseme veebi API.

1. Rakendus saadab esimese taseme veebi-API juurdepääsu luba.


2. Esimese taseme veebi-API saadab taotluse Azure AD Turbeloa lõpp-punkti esitada oma kliendi ID ja mandaat, samuti kasutaja juurdepääsu luba. Lisaks taotluse saadetakse koos mõne on_behalf_of parameeter, mis näitab, et veebirakenduse API taotleb uue sõned järgneval web API algse kasutaja nimel helistamiseks.


3. Azure AD kontrollib, kas esimese taseme veebi-API on teise taseme veebi-API juurdepääsu ja valideerib taotluse esitus JWT juurdepääsu luba, ja on JWT värskendamine esimese taseme veebi-API luba.


4. Https, esimese taseme veebi-API siis helistab teise taseme veebi-API mõnel autoriseerimine päises taotluse Turbeloa string. Esimese taseme veebi-API edasi kõne teise taseme veebi-API, kui juurdepääsu luba ja Värskenda märkide on lubatud.

#### <a name="code-samples"></a>Koodinäiteid

Vaadake proovi kood Daemon või serverirakenduse Veebiteenuste stsenaariumid. Ja kontrollige uuesti sageli--lisame uue näidised kogu aeg. [Server või Daemon rakenduse veebi-API](active-directory-code-samples.md#server-or-daemon-application-to-web-api)

#### <a name="registering"></a>Registreerimine

- Ühe rentnik: Nii rakenduse identiteedi ja delegeeritud kasutaja identiteet juhtudel, daemon või serveri rakendus peab registreerida samas kaustas Azure AD. Veebi-API saab konfigureerida esitamist kogumi õigused kasutatakse piiramiseks daemon või serveri juurdepääsu oma ressursse. Kui kasutusel on delegeeritud kasutaja identiteet tüüp, peab serveri rakendus valige soovitud õigused Azure'i haldusportaal rippmenüü "Õigused soovite muud rakendused". See toiming pole nõutav, kui kasutusel on rakenduse identiteedi tüüp.


- Mitme rentniku: Esmalt daemon või serveri rakendus on konfigureeritud näitamaks, et see nõuab funktsionaalne õigused. See vajalike õiguste loend on dialoogiboksis kui kasutaja või administraator sihtkoha kaustas nõusoleku rakendus, mis teeb selle kättesaadavaks oma organisatsiooni. Mõned rakenduste puhul saate asutuses iga kasutaja nõusolek lihtsalt kasutajatasandi õigused. Muude rakenduste jaoks on vaja administraatori tasemel õigused, mille kasutaja ettevõte ei nõustu. Directory administraator saab anda nõusoleku rakendusi, mis nõuavad selle õiguste tase. Kui administraatori nõustub, nii veebi-API-de registreeritud oma kataloogi.

#### <a name="token-expiration"></a>Turbeloa aegumise

Kui esimene rakendus kasutab selle kood saada JWT Accessi Turbeloa, saab ka märgiks JWT värskendamine. Luba juurdepääs aegumisel saab värskendamise luba uuesti autentida küsimata mandaati. Selle värskenduse märgiks kasutatakse autentida, mille tulemuseks on uue juurdepääsu luba ja Värskenda luba.

## <a name="see-also"></a>Vt ka

[Azure Active Directory arendaja juhend](active-directory-developers-guide.md)

[Azure Active Directory koodinäiteid](active-directory-code-samples.md)

[Oluline teave sisselogimise võtme edastamiseks Azure AD kohta](active-directory-signing-key-rollover.md)

[Azure AD OAuthi 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx)
