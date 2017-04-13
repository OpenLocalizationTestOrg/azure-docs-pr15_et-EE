<properties
    pageTitle="Azure AD .NET protokolli ülevaade | Microsoft Azure'i"
    description="Selles artiklis kirjeldatakse, kuidas kasutada HTTP sõnumeid ei luba juurdepääsu veebirakenduste ja web API-de oma rentniku Azure Active Directory ja OAuth 2.0 abil."
    services="active-directory"
    documentationCenter=".net"
    authors="priyamohanram"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="priyamo"/>


# <a name="authorize-access-to-web-applications-using-oauth-20-and-azure-active-directory"></a>Luba juurdepääs veebirakenduste OAuth 2.0 ja Azure Active Directory abil

Azure Active Directory (Azure AD) kasutab OAuth 2.0 selleks, et saaksite lubada juurdepääsu veebirakenduste ja oma Azure AD rentniku web API-d. Sellest juhendist on keele sõltumatu ja kirjeldatakse, kuidas sõnumite saatmiseks ja vastuvõtmiseks HTTP ühte meie avatud lähtekoodi teekide kasutamata.

OAuth 2.0 autoriseerimine kood voogu on kirjeldatud jaotises [4.1 OAuth 2.0 kirjelduse](https://tools.ietf.org/html/rfc6749#section-4.1) . Seda kasutatakse sooritamiseks autentimise ja luba enamikus rakenduse tüübid, sh veebirakenduste ja algupäraselt installitud rakendused.

[AZURE.INCLUDE [active-directory-protocols-getting-started](../../includes/active-directory-protocols-getting-started.md)]


## <a name="oauth-20-authorization-flow"></a>OAuth 2.0 autoriseerimine meilivoo

Kõrge, kogu autoriseerimine voogu rakenduse veidi on järgmine:

![OAuthi Auth kood meilivoo](media/active-directory-protocols-oauth-code/active-directory-oauth-code-flow-native-app.png)


## <a name="request-an-authorization-code"></a>Kutse mõne kood

Luba kood voogu algab suunavad kasutajal kliendi soovitud `/authorize` lõpp-punkti. Selle taotluse näitab kliendi tuleb hankida kasutaja õigusi. Saate lõpp-punktid OAuth 2.0 rakenduse lehel Azure'i klassikaline portaalis all sahtlis nupp **Kuva lõpp-punktid** .

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&resource=https%3A%2F%2Fservice.contoso.com%2F
&state=12345
```

| Parameetri | | Kirjeldus |
| ----------------------- | ------------------------------- | --------------- |
| rentniku | nõutav | Funktsiooni `{tenant}` väärtus taotluse tee saab kontrollida, kes saavad logida rakendusse.  Lubatud väärtused on rentniku identifikaatorite, nt `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` või `contoso.onmicrosoft.com` või `common` rentniku sõltumatul märkide |
| client_id | nõutav | Rakenduse Id määratud rakenduse registreerimisel Azure AD. See leiate Azure'i haldusportaal. Klõpsake **Active Directory**, klõpsake kausta, klõpsake rakendus ja seejärel klõpsake **konfigureerimine** |
| response_type | nõutav | Peab sisaldama `code` autoriseerimine kood meilivoo jaoks. |
| redirect_uri | soovitatav | Rakenduse, kus autentimine vastuseid saate saadetud ja vastuvõetud rakenduse redirect_uri.  See peab täpselt vastama ühte redirect_uris, mis on registreeritud portaali, kuid peab olema URL-ina kodeeritav.  Kohalikke ja Mobile'i rakendused, kasutage vaikeväärtust, milleks on `urn:ietf:wg:oauth:2.0:oob`. |
| response_mode | soovitatav | Saate määrata meetod, mis tuleks kasutada rakenduse tulemuseks Turbeloa tagasi saatmiseks.  Võib olla `query` või `form_post`.  |
| olek | soovitatav | Väärtus, mis sisaldab taotluse, mis tagastatakse loa vastuse. Juhuslik kordumatu väärtus on tavaliselt kasutatakse [vältimine saitidevaheline taotluse võltsimist eest](http://tools.ietf.org/html/rfc6749#section-10.12).  Riigi kasutatakse ka kodeerida teavet rakenduse kasutaja olekus enne autentimise taotluse ilmnes, näiteks lehe või vaate nad. |
| ressurss | valikuline. | Rakenduse ID URI veebi-API (turvalise ressurss). Leiate Azure'i haldusportaal rakenduse ID URI, veebi-API, klõpsake **Active Directory**, klõpsake kataloogi, valige rakendus ja klõpsake nuppu **Konfigureeri**. |
| küsimus | valikuline. |  Näitab tüüpi kasutaja suhtlus, mida on vaja.<p> Sobivad väärtused on: <p> *login*: palutakse kasutajal uuesti autentida. <p> *nõusolek*: kasutaja nõusolekuta on antud, kuid tuleb värskendada. Kasutajal palutakse nõustu. <p> *admin_consent*: palutakse administraator nõusoleku kõigi oma asutuse kasutajate eest |
| login_hint | valikuline. | Saab kasutada eelnevalt täitmiseks välja kasutajanimi/e-posti aadress sisselogimislehe kasutaja, kui te ei tea oma kasutajanime ette valmistada.  Kasutatakse sageli rakenduste see parameeter on juba ekstraktimist kasutajanimi on eelmise sisselogimise abil uuesti autentimisel on `preferred_username` taotlemine. |
| domain_hint | valikuline. | Pakub Vihje rentniku või domeen, mida kasutaja peaks sisse logimiseks kasutage kohta. Funktsiooni domain_hint väärtus rentniku registreeritud domeeni. Kui Rentnik on ühendatud mõne kohapealse kataloogi, suunab AAD määratud rentniku liiduserveri. |

> [AZURE.NOTE] Kui kasutaja on ettevõttega, asutuse administraator saab nõusoleku kasutaja nimel keeldu või lubada kasutajal nõusolekut. Kasutaja on võimalus lubada ainult siis, kui administraator selle lubab.

Sel hetkel, sisestage oma kasutajanimi ja parool ning nõustute õigused, nagu on näidatud on kasutajal palutakse selle `scope` päringu parameeter. Kui kasutaja autendib ja annab nõusoleku, Azure AD saadab vastuseks rakenduse juures olevat `redirect_uri` teie taotlus aadressi.

### <a name="successful-response"></a>Eduka vastus

Eduka vastuse võib välja näha selline:

```
GET  HTTP/1.1 302 Found
Location: http://localhost/myapp/?code= AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrqqf_ZT_p5uEAEJJ_nZ3UmphWygRNy2C3jJ239gV_DBnZ2syeg95Ki-374WHUP-i3yIhv5i-7KU2CEoPXwURQp6IVYMw-DjAOzn7C3JCu5wpngXmbZKtJdWmiBzHpcO2aICJPu1KvJrDLDP20chJBXzVYJtkfjviLNNW7l7Y3ydcHDsBRKZc3GuMQanmcghXPyoDg41g8XbwPudVh7uCmUponBQpIhbuffFP_tbV8SNzsPoFz9CLpBCZagJVXeqWoYMPe2dSsPiLO9Alf_YIe5zpi-zY4C3aLw5g9at35eZTfNd0gBRpR5ojkMIcZZ6IgAA&session_state=7B29111D-C220-4263-99AB-6F6E135D75EF&state=D79E5777-702E-4260-9A62-37F75FF22CCE
```

| Parameetri | Kirjeldus |
| -----------------------| --------------- |
| admin_consent | Väärtus on True, kui administraator on andnud nõusoleku taotluse küsimus.|
| kood | Luba kood soovitud rakendus. Rakenduse saate kasutada taotleda juurdepääsu ressursile target sümboolse luba kood. |
| session_state | Kordumatu väärtus, mille abil tuvastatakse kasutaja seansi. See väärtus on GUID, kuid tuleks käsitleda läbipaistmatu väärtus, mis on möödas ilma. |
| olek | Kui parameeter olek on kaasatud kutse, kuvatakse sama väärtuse vastus. See on hea tava rakenduse kinnitamiseks koosolekukutsete ja kutsele vastamise olekus väärtused on täpselt ühesugused enne vastuse abil. See aitab tuvastada [saitidevaheline taotlemine võltsimist (CSRF) eest](https://tools.ietf.org/html/rfc6749#section-10.12) kliendi vastu.  

### <a name="error-response"></a>Tõrge vastus

Tõrge vastuste võib ka saata selle `redirect_uri` nii, et taotluse võite hakkama neid õigesti.

```
GET http://localhost:12345/?
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Parameetri | Kirjeldus |
|-----------|-------------|
| tõrge | Määratud punkti 5.2 [OAuthi 2.0 autoriseerimine raamistiku](http://tools.ietf.org/html/rfc6749)koodi veaväärtuse. Järgmine tabel kirjeldab Azure AD tagastav tõrkekoodid. |
| error_description | Üksikasjalikuma kirjelduse viga. See sõnum ei peaks olema lõppkasutaja sõbralik. |
| olek | Oleku väärtus on juhuslik taaskasutada väärtus, mis on saadetud kutse ja tagastatud vastuseks vältida saitidevaheline taotluse võltsimist (CSRF) eest. |

#### <a name="error-codes-for-authorization-endpoint-errors"></a>Tõrkekoodid autoriseerimine lõpp-punkti tõrkeid

Järgmises tabelis kirjeldatakse saate tagasi erinevate tõrkekoodid on `error` parameetri tõrge tagasiside.

| Tõrkekood | Kirjeldus | Kliendi toiming |
|------------|-------------|---------------|
| invalid_request | Protocol (protokoll) viga, nt puuduvad parameeter. | Parandamine ja kutse uuesti. See on areng tõrge püütakse tavaliselt algse testimise käigus.|
| unauthorized_client | Klientrakenduse ei ole lubatud taotleda luba näeb välja umbes järgmine. | See juhtub tavaliselt kui klientrakendus pole registreeritud Azure AD või kasutaja Azure AD rentniku ei lisata. Rakenduse saate kiiresti juhiseid rakenduse installimine ja selle lisamist Azure AD kasutaja. |
| ACCESS_DENIED | Ressursi omanik nõusolekut keelatud | Klientrakenduse saab seda ei saa jätkata, v.a juhul, kui kasutaja nõustub kasutaja teatada. |
| unsupported_response_type | Luba server ei toeta taotluse vastuse tüüp. | Parandamine ja kutse uuesti. See on areng tõrge püütakse tavaliselt algse testimise käigus.|
|server_error | Server ilmnes ootamatu tõrge. | Kutse uuesti. Nende vigade võib põhjustada ajutist tingimusi. Klientrakenduse võib selgitada kasutaja, et oma vastuse viibib tähtaja ajutine tõrge. |
| temporarily_unavailable | Server pole ajutiselt hõivatud päringu töötlemiseks. | Kutse uuesti. Klientrakenduse võib selgitada kasutaja, et oma vastuse viibib tähtaja ajutine seisund. |
| invalid_resource |Sihtrakenduse ressursi ei sobi, kuna seda pole, Azure AD ei leia või see on õigesti konfigureeritud.| See näitab, et ressurss, kui see on olemas, ei ole konfigureeritud rentniku. Rakenduse saate kiiresti juhiseid rakenduse installimine ja selle lisamist Azure AD kasutaja. |

## <a name="use-the-authorization-code-to-request-an-access-token"></a>Taotleda juurdepääsu sümboolse luba kood abil

Omandatud näeb välja umbes järgmine autoriseerimine ja on antud õigus kasutaja, saab lunastada kood soovitud ressursile juurdepääsu sümboolse postituse taotluse saatmisega on `/token` lõpp-punkt:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded
grant_type=authorization_code
&client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrqqf_ZT_p5uEAEJJ_nZ3UmphWygRNy2C3jJ239gV_DBnZ2syeg95Ki-374WHUP-i3yIhv5i-7KU2CEoPXwURQp6IVYMw-DjAOzn7C3JCu5wpngXmbZKtJdWmiBzHpcO2aICJPu1KvJrDLDP20chJBXzVYJtkfjviLNNW7l7Y3ydcHDsBRKZc3GuMQanmcghXPyoDg41g8XbwPudVh7uCmUponBQpIhbuffFP_tbV8SNzsPoFz9CLpBCZagJVXeqWoYMPe2dSsPiLO9Alf_YIe5zpi-zY4C3aLw5g9at35eZTfNd0gBRpR5ojkMIcZZ6IgAA
&redirect_uri=https%3A%2F%2Flocalhost%2Fmyapp%2F
&resource=https%3A%2F%2Fservice.contoso.com%2F
&client_secret=p@ssw0rd

//NOTE: client_secret only required for web apps
```

| Parameetri | | Kirjeldus |
| ----------------------- | ------------------------------- | --------------------- |
| rentniku | nõutav |  Funktsiooni `{tenant}` väärtus taotluse tee saab kontrollida, kes saavad logida rakendusse.  Lubatud väärtused on rentniku identifikaatorite, nt `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` või `contoso.onmicrosoft.com` või `common` rentniku sõltumatul märkide |
| client_id | nõutav | Rakenduse Id määratud rakenduse registreerimisel Azure AD. See leiate Azure'i klassikaline portaalis. Klõpsake **Active Directory**, klõpsake kausta, klõpsake rakendus ja seejärel klõpsake **konfigureerimine** |
| grant_type | nõutav | Peab olema `authorization_code` autoriseerimine kood meilivoo jaoks. |
| kood | nõutav | Funktsiooni `authorization_code` mis eelmises jaotises ostsite   |
| redirect_uri | nõutav | Sama `redirect_uri` väärtus, mida kasutati omandada soovitud `authorization_code`. |
| client_secret | nõutav web apps | Rakenduse salajane rakenduse registreerimise portaali loodud rakenduse.  See peaks ei tohi kasutada rakenduste, kuna client_secrets ei saa usaldusväärselt salvestatud seadmetes.  See on nõutav veebirakenduste ja veebi API-d, mis on võimalus talletada soovitud `client_secret` turvaliselt serveripoolne. |
| ressurss | Kui määratud luba kood taotluse veel valikuline nõutav | Rakenduse ID URI veebi-API (turvalise ressurss).
Leiate Azure'i haldusportaal rakenduse ID URI, klõpsake **Active Directory**, klõpsake kausta, valige rakendus ja klõpsake nuppu **Konfigureeri**.

### <a name="successful-response"></a>Eduka vastus

Azure AD annab juurdepääsu sümboolse eduka vastuse korral. Võrgu kõned klientrakenduse ja nende seotud latentsus minimeerimiseks peaks klientrakenduse vahemälu Accessi sõned Turbeloa OAuth 2.0 vastuse määratud eluiga. Sümboolne eluiga määramiseks kasutada ühte selle `expires_in` või `expires_on` parameetrite väärtused.

Kui web API ressursi tagastab mõne `invalid_token` tõrkekood, see võib viidata ressurss on määratud, et luba on aegunud. Kui kliendi- ja ressursihalduse kella ajad on erinevad (tuntud "skew aeg"), kaaluda ressursi luba, et olla aegunud enne kliendi vahemälu tühjendatakse luba. Sel juhul tühjendage Luba vahemälu, isegi juhul, kui see on arvutatud tööiga.

Eduka vastuse võib välja näha selline:

```
{
  "access_token": " eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ",
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1388444763",
  "resource": "https://service.contoso.com/",
  "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4rTfgV29ghDOHRc2B-C_hHeJaJICqjZ3mY2b_YNqmf9SoAylD1PycGCB90xzZeEDg6oBzOIPfYsbDWNf621pKo2Q3GGTHYlmNfwoc-OlrxK69hkha2CF12azM_NYhgO668yfcUl4VBbiSHZyd1NVZG5QTIOcbObu3qnLutbpadZGAxqjIbMkQ2bQS09fTrjMBtDE3D6kSMIodpCecoANon9b0LATkpitimVCrl-NyfN3oyG4ZCWu18M9-vEou4Sq-1oMDzExgAf61noxzkNiaTecM-Ve5cq6wHqYQjfV9DOz4lbceuYCAA",
  "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
"id_token": " eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83ZmU4MTQ0Ny1kYTU3LTQzODUtYmVjYi02ZGU1N2YyMTQ3N2UvIiwiaWF0IjoxMzg4NDQwODYzLCJuYmYiOjEzODg0NDA4NjMsImV4cCI6MTM4ODQ0NDc2MywidmVyIjoiMS4wIiwidGlkIjoiN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlIiwib2lkIjoiNjgzODlhZTItNjJmYS00YjE4LTkxZmUtNTNkZDEwOWQ3NGY1IiwidXBuIjoiZnJhbmttQGNvbnRvc28uY29tIiwidW5pcXVlX25hbWUiOiJmcmFua21AY29udG9zby5jb20iLCJzdWIiOiJKV3ZZZENXUGhobHBTMVpzZjd5WVV4U2hVd3RVbTV5elBtd18talgzZkhZIiwiZmFtaWx5X25hbWUiOiJNaWxsZXIiLCJnaXZlbl9uYW1lIjoiRnJhbmsifQ.”
}

```

| Parameetri | Kirjeldus |
| ----------------------- | ------------------------------- |
| access_token | Nõutud juurdepääsu luba. Rakenduse saate kasutada seda luba turvalise ressursile, nt veebi-API autentida. |
| token_type | Näitab, et loa tüüp väärtus. Ainult tüüp, mis toetab Azure AD on esitaja. Esitaja sõned kohta leiate lisateavet teemast [OAuth2.0 autoriseerimine raames: esitaja Turbeloa kasutus (RFC 6750)](http://www.rfc-editor.org/rfc/rfc6750.txt)  |
| expires_in | Kui kaua kehtib juurdepääsu luba (sekundites). |
| expires_on | Kellaaeg, millal juurdepääsu luba on aegunud. Kuupäev esitatakse arvuna sekundite arv 1970-01-01T0:0:0Z UTC kuni järgneb. Seda väärtust kasutatakse vahemällu talletatud sõned kestuse määramiseks. |
| ressurss | Rakenduse ID URI veebi-API (turvalise ressurss).|
| ulatus | Isikustamine õiguste klientrakendust. Vaikimisi õigused on `user_impersonation`. Turvalise ressursi omanik saab registreerida väärtused täiendav Azure AD.|
| refresh_token |  OAuth 2.0 värskendamise luba. Rakenduse abil saate selle luba hankida täiendavat juurdepääsu sõned pärast praeguse juurdepääsu luba on aegunud.  Värskenda sõned on vanade ja seda saab kasutada säilitamise ressurssidele laiendatud aja jooksul. |
| id_token | Ka allkirjastamata JSON Web Turbeloa (JWT). Rakenduse saab base64Url dekodeerida selle märgiks sisse logitud kasutaja teavet taotleda lõigud. Rakenduse saate vahemälu väärtused ja neid kuvada, kuid see ärge neid autoriseerimine või turvalisus piirmäärad. |

### <a name="jwt-token-claims"></a>JWT Turbeloa nõuded
JWT luba väärtus on `id_token` parameeter saate rakendusse järgmised nõuded:

```
{
 "typ": "JWT",
 "alg": "none"
}.
{
 "aud": "2d4d11a2-f814-46a7-890a-274a72a7309e",
 "iss": "https://sts.windows.net/7fe81447-da57-4385-becb-6de57f21477e/",
 "iat": 1388440863,
 "nbf": 1388440863,
 "exp": 1388444763,
 "ver": "1.0",
 "tid": "7fe81447-da57-4385-becb-6de57f21477e",
 "oid": "68389ae2-62fa-4b18-91fe-53dd109d74f5",
 "upn": "frank@contoso.com",
 "unique_name": "frank@contoso.com",
 "sub": "JWvYdCWPhhlpS1Zsf7yYUxShUwtUm5yzPmw_-jX3fHY",
 "family_name": "Miller",
 "given_name": "Frank"
}.
```

Funktsiooni `id_token` parameeter sisaldab järgmist tüüpi taotluste. JSON web sõned kohta leiate lisateavet teemast [JWT IETF mustand määratlus](http://go.microsoft.com/fwlink/?LinkId=392344). Turbeloa tüübid ja nõuete kohta lisateabe saamiseks lugege [toetatud Turbeloa ja taotluste tüübid](active-directory-token-and-claims.md)

| Taotluse tüüp | Kirjeldus |
|------------|-------------|
| AUD | Luba mis tahes suurusega publikule. Kui kliendi rakendus on antud luba, et publik on selle `client_id` kliendi.
| exp | Aegumise kellaaeg. Kellaaeg, millal luba on aegunud. Luba kehtib, praegune kuupäev/kellaaeg peab olema väiksem kui või võrdne soovitud `exp` väärtus. Aeg on esitatud arvuna sekundite arv 1 Jaanuar 1970 (1970-01-01T0:0:0Z) on antud luba kuni UTC. |
| family_name | Kasutaja viimase nimi või perekonnanimi. Rakenduse saate kuvada selle väärtuse. |
| given_name | Kasutaja eesnimi. Rakenduse saate kuvada selle väärtuse. |
| IAT | Välja ajal. Kui soovitud JWT on välja antud aeg. Aeg on esitatud arvuna sekundite arv 1 Jaanuar 1970 (1970-01-01T0:0:0Z) on antud luba kuni UTC. |
| ISS | Tuvastab loa väljaandja |
| nbf | Ei enne aega. Kui luba kehtivuse aeg. Luba kehtib, praegune kuupäev/kellaaeg peab olema suurem või võrdne väärtusega Nbf. Aeg on esitatud arvuna sekundite arv 1 Jaanuar 1970 (1970-01-01T0:0:0Z) on antud luba kuni UTC. |
| objekti ID | Kasutaja objekti Azure AD objekti identifikaator (ID). |
| sub | Turbeloa teema identifikaator. See on püsiv ja püsiv identifikaator luba kirjeldav kasutaja jaoks. Kasutage loogika vahemällu selle väärtuse. |
| tid | Luba väljastanud Azure AD rentniku rentniku identifikaator (ID). |
| unique_name | Kasutaja saab kuvada selle jaoks kordumatut tunnust. See on tavaliselt kasutaja turvasubjektinimi (UPN). |
| UPN-i | Kasutaja turvasubjektinimi kasutaja. |
| ver | Versioon. JWT luba, tavaliselt 1.0 versiooni. |

### <a name="error-response"></a>Tõrge vastus

Turbeloa emissiooni lõpp-punkti tõrgete on HTTP tõrkekoodid, kuna Klient kutsub Turbeloa emissiooni lõpp-punkti otse. Lisaks HTTP olekukoodi, Azure AD Turbeloa emissiooni lõpp-punkti tagastab JSON dokumendi objektid, mis kirjeldavad viga.

Vastuse valimi tõrge võib välja näha selline:

```
{
  "error": "invalid_grant",
  "error_description": "AADSTS70002: Error validating credentials. AADSTS70008: The provided authorization code or refresh token is expired. Send a new interactive authorization request for this user and resource.\r\nTrace ID: 3939d04c-d7ba-42bf-9cb7-1e5854cdce9e\r\nCorrelation ID: a8125194-2dc8-4078-90ba-7b6592a7f231\r\nTimestamp: 2016-04-11 18:00:12Z",
  "error_codes": [
    70002,
    70008
  ],
  "timestamp": "2016-04-11 18:00:12Z",
  "trace_id": "3939d04c-d7ba-42bf-9cb7-1e5854cdce9e",
  "correlation_id": "a8125194-2dc8-4078-90ba-7b6592a7f231"
}
```
| Parameetri | Kirjeldus |
| ----------------------- | ------------------------------- |
| tõrge | Kuvatakse tõrge kood string, mis saab liigitada tüüpi ilmnevad tõrked ja seda saab kasutada reageerida tõrked. |
| error_description | Teatud tõrketeade, mis aitab tuvastada autentimise tõrke põhjuseks arendaja.  |
| error_codes | STS kindlate tõrkekoodide, mis aitavad diagnostika loend. |
| ajatempli | Aeg, mille viga. |
| trace_id | Taotlus, mis aitavad diagnostika kordumatut tunnust.  |
| correlation_id | Koosolekukutse, mis aitavad diagnostika üle komponendid ainuidentifikaator.|

#### <a name="http-status-codes"></a>HTTP olekukoodi

Järgmises tabelis on loetletud HTTP olek koodid, mis tagastab Turbeloa emissiooni lõpp-punkti. Mõnel juhul tõrkekoodi piisab vastuse kirjeldamiseks, kuid korral tõrgete, peate sõeluda kaasnevaid JSON dokumendi ja selle tõrkekoodi uurida.

| HTTP kood | Kirjeldus |
|-----------|-------------|
| 400       | Vaikimisi HTTP kood. Kasutatakse enamasti ja on tavaliselt moonutatud taotluse tõttu. Parandamine ja kutse uuesti. |
| 401       | Autentimine nurjus. Näiteks puudub taotluse client_secret parameeter.|
| 403       | Autoriseerimine nurjus. Näiteks pole kasutaja ressursile juurdepääsu õigus. |
| 500       | Teenust on ilmnenud sisemine tõrge. Kutse uuesti. |

#### <a name="error-codes-for-token-endpoint-errors"></a>Tõrkekoodid Turbeloa lõpp-punkti tõrkeid

| Tõrkekood | Kirjeldus | Kliendi toiming |
|------------|-------------|---------------|
| invalid_request | Protocol (protokoll) viga, nt puuduvad parameeter. | Parandamine ja edastage taotlus |
| invalid_grant | Luba kood ei sobi või on aegunud. | Proovige uue taotluse selle `/authorize` lõpp-punkti |
| unauthorized_client | Autenditud kliendi pole õigust kasutatav loa andmine tüüp. | See juhtub tavaliselt kui klientrakendus pole registreeritud Azure AD või kasutaja Azure AD rentniku ei lisata. Rakenduse saate kiiresti juhiseid rakenduse installimine ja selle lisamist Azure AD kasutaja. |
| invalid_client | Kliendi autentimine nurjus. | Kliendi identimisteave ei sobi. Lahendamiseks värskendab teenuserakenduse administraatori mandaadi. |
| unsupported_grant_type | Luba server ei toeta loa andmine tüüp. | Muuta taotluse anda tüüpi. Seda tüüpi tõrge peaks toimuma ainult arendamise käigus ning leitud esimene katsetamine. |
| invalid_resource | Sihtrakenduse ressursi ei sobi, kuna seda pole, Azure AD ei leia või see on õigesti konfigureeritud. | See näitab, et ressurss, kui see on olemas, ei ole konfigureeritud rentniku. Rakenduse saate kiiresti juhiseid rakenduse installimine ja selle lisamist Azure AD kasutaja. |
| interaction_required | Kutse nõuab kasutaja suhtlus. Näiteks on samm täiendavate autentimine on nõutav. | Proovige uuesti taotluse sama ressursiga. |
| temporarily_unavailable | Server pole ajutiselt hõivatud päringu töötlemiseks. | Kutse uuesti. Klientrakenduse võib selgitada kasutaja, et oma vastuse viibib tähtaja ajutine seisund.|

## <a name="use-the-access-token-to-access-the-resource"></a>Kasutage juurdepääsu ressursile juurdepääsu luba

Nüüd, kui olete edukalt ostsite mõne `access_token`, saate kasutada luba Web API-de taotlusi, sh see on `Authorization` päis. [RFC 6750](http://www.rfc-editor.org/rfc/rfc6750.txt) spetsifikatsioon selgitab, kuidas kasutada esitaja sõned HTTP päringuid juurdepääsu kaitstud ressurssidele.

### <a name="sample-request"></a>Valimi taotlus

```
GET /data HTTP/1.1
Host: service.contoso.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ
```

### <a name="error-response"></a>Tõrge vastus

Turvalise ressursid, millest RFC 6750 probleemi HTTP olekukoodi rakendada. Kui taotlus ei sisalda autentimiseks mandaadi või luba puudub, sisaldab vastus mõne `WWW-Authenticate` päis. Kui taotlus nurjub, ressursside server saadab selle mõne HTTP olekukoodi ja tõrkekood.

Järgmine on näide õnnestu vastuse, kui kliendi tellimus ei sisalda esitaja luba:

```
HTTP/1.1 401 Unauthorized
WWW-Authenticate: Bearer authorization_uri="https://login.window.net/contoso.com/oauth2/authorize",  error="invalid_token",  error_description="The access token is missing.",
```

#### <a name="error-parameters"></a>Tõrge parameetrid

| Parameetri | Kirjeldus |
|-----------|-------------|
| authorization_uri | URI (füüsilise lõpp-punkti) autoriseerimine server. Seda väärtust kasutatakse ka otsingu klahvi saada lisateavet server on discovery lõpp-punkti. <p><p> Kliendi peate kontrollima, kas autoriseerimine server on usaldusväärne. Kui ressurss on kaitstud Azure AD, piisab kinnitamaks, et URL-i algab https://login.windows.net või mõne muu hostname, mis toetab Azure AD. Rentniku kohased ressursi alati peaks tagastama rentniku kohased autoriseerimine URI. |
| tõrge | Määratud punkti 5.2 [OAuthi 2.0 autoriseerimine raamistiku](http://tools.ietf.org/html/rfc6749)koodi veaväärtuse.|
| error_description | Üksikasjalikuma kirjelduse viga. See sõnum ei peaks olema lõppkasutaja sõbralik.|
| resource_id | Tagastab Ressursi ainuidentifikaator. Klientrakenduse saate kasutada seda identifikaator väärtus on `resource` parameeter, kui seda taotleb märgiks ressursi. <p><p> On väga oluline klientrakenduse kinnitamaks, et see väärtus, muidu pahatahtlik teenuse võib olla võimalus esile kutsuda rünnaku **tõus õigusi.** <p><p> Soovitatav strateegia vältimine rünnak on veendumaks, et selle `resource_id` vastab alus Web API URL-i, mis on. Näiteks kui https://service.contoso.com/data pääseb soovitud `resource_id` võib olla htttps://service.contoso.com/. Klientrakenduse peab kõnest on `resource_id` mis ei alga base URL-i, kui on usaldusväärne alternatiivne viis id kinnitamiseks. |

#### <a name="bearer-scheme-error-codes"></a>Esitaja värviskeemi kuvatavad tõrkekoodid

RFC 6750 spetsifikatsioon määratleb järgmistest tõrketeadetest WWW autentida päise- ja esitaja värviskeemi kasutamine vastus kasutavate ressursid.

| HTTP olekukoodi | Tõrkekood | Kirjeldus | Kliendi toiming |
|------------------|------------|-------------|---------------|
| 400 | invalid_request | Taotluse pole regulaarne. Näiteks see võib olla puudu parameetri või sama parameetriga kaks korda. | Vea parandamiseks kutse uuesti. Seda tüüpi tõrge peaks toimuma ainult arendamise käigus ning leitud esimene katsetamine. |
| 401 | invalid_token   | Luba juurdepääs on puudu, sobimatud või on tühistatud. Error_description parameeter on toodud täiendavad üksikasjad. |  Uue loa taotleda luba serverist. Uue loa nurjumisel ilmnes ootamatu tõrge. Saata tõrketeade juhusliku viivitused pärast kasutaja ja proovige uuesti. |
| 403 | insufficient_scope | Luba juurdepääs ei sisalda isikustamine ressursile juurdepääsemiseks vajalikud õigused. | Luba uus koosolekukutse saatmiseks autoriseerimine lõpp-punkti. Kui vastus sisaldab ulatus parameeter, kasutage ressursile taotluse ulatus väärtus. |
| 403 | insufficient_access | Luba teema ei saa ressursile juurdepääsemiseks vajalikud õigused. | Küsi kasutaja kasutada mõne muu kontoga või paluge määratud ressursi õigused. |

## <a name="refreshing-the-access-tokens"></a>Accessi sõned värskendamine

Accessi märgid on lühiajaline ja tuleb värskendada pärast need aeguvad edaspidi juurdepääsu ressursid. Saate värskendada selle `access_token` esitades teise `POST` taotlus on `/token` lõpp-punkti, kuid ajal esitada soovitud `refresh_token` asemel on `code`.

Värskenda sõned ei saa määratud eluajal. Tavaliselt on suhteliselt pikka eluajal sõned värskendamise kohta. Siiski mõnel juhul värskendamise sõned aegumise, on tühistatud või puudub soovitud toimingu piisavaid õigusi. Rakenduse tuleb oodata ja vigade tagastatud Turbeloa emissiooni lõpp-punkti õigesti.

Kui teile saadetakse vastuse Värskenda Turbeloa viga, Hülga praeguse värskendamise luba ja taotleda uut luba kood või Luba juurdepääs. Kui abil värskendamise sümboolne rakenduses autoriseerimine koodi anda voogu, kui saate vastus koos selle `interaction_required` või `invalid_grant` tõrkekoodid, Hülga värskendamise luba ja taotleda uut luba kood.

Valimi taotlus **rentniku kohased** lõpp-punkti (saate kasutada ka **levinud** lõpp-punkti) saada uut Accessi Turbeloa kasutades värskendamise märgiks näeb välja umbes järgmine:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&refresh_token=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq...
&grant_type=refresh_token
&resource=https%3A%2F%2Fservice.contoso.com%2F
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for web apps
```
| Parameetri | Kirjeldus |
|-----------|-------------|
| access_token | Uue juurdepääsu luba, mis on nõutav.|
| expires_in   | Luba järelejäänud elu sekundites. Tüüpilised väärtus 3600 (1 tund). |
| expires_on   | Kuupäev ja kellaaeg, mil luba on aegunud. Kuupäev esitatakse arvuna sekundite arv 1970-01-01T0:0:0Z UTC kuni järgneb. |
| refresh_token | Uue OAuth 2.0 refresh_token, taotleda uut Accessi sõned aegumisel selle vastus üks kasutatud. |
| ressurss     | Tuvastab turvalise ressurss, mida saab juurdepääsu luba kasutatud juurdepääs. |
| ulatus        | Isikustamine õiguste omakliendi rakendus. **User_impersonation**on vaikimisi luba. Sihtrakenduse ressursi omanik saab registreerida asendusväärtused Azure AD. |
| token_type   | Turbeloa tüüp. Ainus toetatud väärtus on **esitaja**. |

### <a name="successful-response"></a>Eduka vastus

Eduka loa vastuse näeb:

```
{
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1460404526",
  "resource": "https://service.contoso.com/",
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ",
  "refresh_token": "AwABAAAAv YNqmf9SoAylD1PycGCB90xzZeEDg6oBzOIPfYsbDWNf621pKo2Q3GGTHYlmNfwoc-OlrxK69hkha2CF12azM_NYhgO668yfcUl4VBbiSHZyd1NVZG5QTIOcbObu3qnLutbpadZGAxqjIbMkQ2bQS09fTrjMBtDE3D6kSMIodpCecoANon9b0LATkpitimVCrl PM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4rTfgV29ghDOHRc2B-C_hHeJaJICqjZ3mY2b_YNqmf9SoAylD1PycGCB90xzZeEDg6oBzOIPfYsbDWNf621pKo2Q3GGTHYlmNfwoc-OlrxK69hkha2CF12azM_NYhgO668yfmVCrl-NyfN3oyG4ZCWu18M9-vEou4Sq-1oMDzExgAf61noxzkNiaTecM-Ve5cq6wHqYQjfV9DOz4lbceuYCAA"
}
```

### <a name="error-response"></a>Tõrge vastus

Vastuse valimi tõrge võib välja näha selline:

```
{
  "error": "invalid_resource",
  "error_description": "AADSTS50001: The application named https://foo.microsoft.com/mail.read was not found in the tenant named 295e01fc-0c56-4ac3-ac57-5d0ed568f872.  This can happen if the application has not been installed by the administrator of the tenant or consented to by any user in the tenant.  You might have sent your authentication request to the wrong tenant.\r\nTrace ID: ef1f89f6-a14f-49de-9868-61bd4072f0a9\r\nCorrelation ID: b6908274-2c58-4e91-aea9-1f6b9c99347c\r\nTimestamp: 2016-04-11 18:59:01Z",
  "error_codes": [
    50001
  ],
  "timestamp": "2016-04-11 18:59:01Z",
  "trace_id": "ef1f89f6-a14f-49de-9868-61bd4072f0a9",
  "correlation_id": "b6908274-2c58-4e91-aea9-1f6b9c99347c"
}
```

| Parameetri | Kirjeldus |
| ----------------------- | ------------------------------- |
| tõrge | Kuvatakse tõrge kood string, mis saab liigitada tüüpi ilmnevad tõrked ja seda saab kasutada reageerida tõrked. |
| error_description | Teatud tõrketeade, mis aitab tuvastada autentimise tõrke põhjuseks arendaja.  |
| error_codes | STS kindlate tõrkekoodide, mis aitavad diagnostika loend. |
| ajatempli | Aeg, mille viga. |
| trace_id | Taotlus, mis aitavad diagnostika kordumatut tunnust.  |
| correlation_id | Koosolekukutse, mis aitavad diagnostika üle komponendid ainuidentifikaator.|

Kuvatavad tõrkekoodid ja Soovitatavad kliendi toimingu kirjeldust, leiate [tõrkekoodid Turbeloa lõpp-punkti tõrkeid](#error-codes-for-token-endpoint-errors).
