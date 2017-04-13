 <properties
   pageTitle="Azure'i AD luba viide | Microsoft Azure'i"
   description="Juhendi mõistmine ja hindamise nõuded SAML 2.0 ja JSON Web sõned (JWT) märkide väljaandja järgi Azure Active Directory (AAD)"
   documentationCenter="na"
   authors="bryanla"
   services="active-directory"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/06/2016"
   ms.author="mbaldwin"/>

# <a name="azure-ad-token-reference"></a>Azure'i AD Turbeloa viide

Azure Active Directory (Azure AD) eraldab mitut tüüpi turvalisus märkide töötlemine iga autentimist kulgemist. Selles dokumendis kirjeldatakse vorming, Turve omadusi ja igat tüüpi luba sisu.

## <a name="types-of-tokens"></a>Märkide tüübid

Azure AD toetab [OAuth 2.0 autoriseerimine Protocol (protokoll)](active-directory-protocols-oauth-code.md), mis kasutab nii access_tokens ja refresh_tokens.  Samuti toetab autentimis- ja Logi sisse [OpenID ühendus](active-directory-protocols-openid-connect-code.md), mis tutvustab tüüpi luba, et id_token kaudu.  Iga nende tähiste on esitatud märgiks"esitaja".

Esitaja märgiks on kerge Turbeloa, mis annab "esitaja" juurdepääs kaitstud ressursiga. Selles mõttes on "esitaja" igal pool, mida saab esitada luba. Kuigi autentimine Azure AD nõutakse esitaja luba vastu võtta, tuleb tagada, et vältida faili tahtmatut isiku kolmas luba. Kuna esitaja sõned ei ole sisseehitatud süsteem takistada volitamata isikutel, kasutades neid, nende vedamine peab toimuma turvaline kanali, nt transpordikihi Turve (HTTPS). Kui esitaja luba edastatakse selge, on mees-keskmisel rünnak saab hankida luba ja volitamata juurdepääsu kaitstud ressursi. Turvalisus põhimõtted rakendada, kui salvestamise või esitaja sõned hilisemaks kasutamiseks vahemällu. Alati veenduge, et teie rakendus edastab ja salvestab esitaja sõned turvaline viisil. Vaadake veel turvakaalutlused esitaja sõned, [RFC 6750 jaotise 5](http://tools.ietf.org/html/rfc6750).

Paljud märgid, mis on antud Azure AD rakendatakse JSON Web sõned või JWTs.  Mõne JWT on tihendatud, URL-i ohutu teabe üle kahe poole vahel.  JWTs sisalduvat teavet nimetatakse "nõuded" või kinnitused teabe esitaja ja luba teema kohta.  JWTs nõuded on JSON objektide kodeeritud ja seeriasertide edastamiseks.  Kuna JWTs, Azure AD välja logitud, kuid pole krüptitud, saate hõlpsasti kontrolli JWT on sisu silumine eesmärgil.  See on saadaval nii, nagu [jwt.calebb.net](http://jwt.calebb.net)mitmesuguseid tööriistu. Lisateavet JWTs, võib viidata [JWT määratlus](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html).

## <a name="idtokens"></a>Id_tokens

Id_tokens on sisselogimise Turbeloa rakenduse saab autentimist kasutades [OpenID Connect](active-directory-protocols-openid-connect-code.md)täites vormile.  Need on esindatud [JWTs](#types-of-tokens)ja sisaldavad taotluste, mille abil saate oma rakenduse kasutaja rakendusse.  Nagu näed – need on levinud konto teabe kuvamine või juurdepääsu juhtimine otsuste rakenduse saate kasutada ka id_token nõuded.

Id_tokens sisse loginud, kuid seekord krüptitud.  Kui minirakenduse saab ka id_token, tuleb [allkirja valideerimiseks](#validating-tokens) tõendada selle luba autentsust ja mõned nõuded luba tõendada selle kehtivust kinnitada.  Kinnitatud rakenduse nõuded erinevad sõltuvalt stsenaarium nõuetele, kuid on mõned [levinud taotluste valideerimised](#validating-tokens) , mis rakenduse peate tegema iga stsenaarium.

Leiate järgmisest jaotisest teavet id_tokens taotluste kui ka valimi id_token.  Pange tähele, et id_tokens nõuded ei tagastata kindlas järjekorras.  Lisaks uute taotluste võimalik viia id_tokens igal ajal - rakenduse peaks leheküljepiiri, nagu uute taotluste tuuakse.  Järgmine loend sisaldab taotluste, mis rakenduse saab usaldusväärselt tõlgendamine kirjutamise ajal.  Vajaduse korral leiate veel rohkem üksikasju [OpenID Connect määratlus](http://openid.net/specs/openid-connect-core-1_0.html).

#### <a name="sample-idtoken"></a>Valimi id_token

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83ZmU4MTQ0Ny1kYTU3LTQzODUtYmVjYi02ZGU1N2YyMTQ3N2UvIiwiaWF0IjoxMzg4NDQwODYzLCJuYmYiOjEzODg0NDA4NjMsImV4cCI6MTM4ODQ0NDc2MywidmVyIjoiMS4wIiwidGlkIjoiN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlIiwib2lkIjoiNjgzODlhZTItNjJmYS00YjE4LTkxZmUtNTNkZDEwOWQ3NGY1IiwidXBuIjoiZnJhbmttQGNvbnRvc28uY29tIiwidW5pcXVlX25hbWUiOiJmcmFua21AY29udG9zby5jb20iLCJzdWIiOiJKV3ZZZENXUGhobHBTMVpzZjd5WVV4U2hVd3RVbTV5elBtd18talgzZkhZIiwiZmFtaWx5X25hbWUiOiJNaWxsZXIiLCJnaXZlbl9uYW1lIjoiRnJhbmsifQ.
```

> [AZURE.TIP] Harjutamine, proovige kontrollimise nõuded valimi id_token, kleepides [calebb.net](http://jwt.calebb.net).

#### <a name="claims-in-idtokens"></a>Nõuete id_tokens

| JWT taotluste | Nimi | Kirjeldus |
|-----------|------|-------------|
| `appid`| Rakenduse ID | Tuvastab rakendus, mis kasutab luba juurdepääsuks ressursi. Rakenduse saate toimivad iseendana või kasutaja nimel. Rakenduse ID tavaliselt tähistab objekti rakenduse, kuid selle võib ka Azure AD tähistada teenuse peamine eesmärk. <br><br> **Näide JWT väärtus**: <br> `"appid":"15CB020F-3984-482A-864D-1D92265E8268"` |
| `aud`| Sihtrühma | Luba adressaadini. Rakendus, mida saab luba peab veenduge, et publik väärtus oleks õige ja Hülga kõik märgid, mis on mõeldud muu publik. <br><br> **Näide SAML väärtus**: <br> `<AudienceRestriction>`<br>`<Audience>`<br>`https://contoso.com`<br>`</Audience>`<br>`</AudienceRestriction>` <br><br> **Näide JWT väärtus**: <br> `"aud":"https://contoso.com"` |
| `appidacr`| Rakenduse autentimise kontekstis klassi viide | Näitab, kuidas kliendi kinnitamist. Avaliku klient, väärtus 0. Kliendi ID ja kliendi salajane kasutamisel on väärtus 1. <br><br> **Näide JWT väärtus**: <br> `"appidacr": "0"`|
| `acr`| Autentimise kontekstis klassi viide | Näitab, kuidas teema on autenditud rakenduse autentimise kontekstis klassi viite taotluste kliendi asemel. Väärtus "0" näitab lõppkasutaja autentimise ei vasta nõuetele ISO/IEC 29115. <br><br> **Näide JWT väärtus**: <br> `"acr": "0"`|
| | Kiirsõnumite autentimine | Kirjete kuupäev ja kellaaeg, millal ilmnes autentimist. <br><br> **Näide SAML väärtus**: <br> `<AuthnStatement AuthnInstant="2011-12-29T05:35:22.000Z">` |
| `amr`| Autentimismeetodi määramine | Tuvastab, kuidas luba teema kinnitamist. <br><br> **Näide SAML väärtus**: <br> `<AuthnContextClassRef>`<br>`http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod/password`<br>`</AuthnContextClassRef>` <br><br> **Näide JWT väärtus**:`“amr”: ["pwd"]` |
| `given_name`| Eesnimi | Sisaldab esimest või "antud" selle kasutaja nimi, nagu Azure AD kasutaja objekti. <br><br> **Näide SAML väärtus**: <br> `<Attribute Name=”http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname”>`<br>`<AttributeValue>Frank<AttributeValue>` <br><br> **Näide JWT väärtus**: <br> `"given_name": "Frank"` |
| `groups`| Rühmad | Pakub objekti ID-d, mis tähistavad selle teema rühmaliikmeid. Need väärtused on kordumatu (vt objekti ID) ja turvaline kasutada juurdepääsu, nt jõustamine loa ressurssi juurdepääsu haldamine. Rühmade taotluste kaasatud rühmad on konfigureeritud rakenduse kohta alusel Rakendusmanifest atribuudi "groupMembershipClaims" kaudu. Kõigi rühmade ei sisalda väärtus on null, "SecurityGroup" väärtus sisaldab ainult Active Directory turberühma rühmakuuluvus ja väärtuse "Kõik" Kas kaasata turberühmad ja Office 365 leviloend. <br><br> **Näide SAML väärtus**: <br> `<Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/groups">`<br>`<AttributeValue>07dd8a60-bf6d-4e17-8844-230b77145381</AttributeValue>` <br><br> **Näide JWT väärtus**: <br> `“groups”: ["0e129f5b-6b0a-4944-982d-f776045632af", … ]` |
| `idp` | Identiteedipakkuja | Kirjete identiteedipakkuja juures, mille teema luba autenditud. See väärtus on identne väljaandja nõude väärtus juhul, kui kasutaja on rentnikus, kui väljaandja. <br><br> **Näide SAML väärtus**: <br> `<Attribute Name=” http://schemas.microsoft.com/identity/claims/identityprovider”>`<br>`<AttributeValue>https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/<AttributeValue>` <br><br> **Näide JWT väärtus**: <br> `"idp":”https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/”` |
| `iat` | IssuedAt | Talletab korda, mis on antud luba. Seda kasutatakse sageli mõõta Turbeloa värskus. <br><br> **Näide SAML väärtus**: <br> `<Assertion ID="_d5ec7a9b-8d8f-4b44-8c94-9812612142be" IssueInstant="2014-01-06T20:20:23.085Z" Version="2.0" xmlns="urn:oasis:names:tc:SAML:2.0:assertion">` <br><br> **Näide JWT väärtus**: <br> `"iat": 1390234181` |
| `iss` | Väljaandja | Tuvastab turvalisus Turbeloa teenuse (STS) sisuosast ja tagastab luba. Märgid, mis tagastab Azure AD, on väljaandja sts.windows.net. GUID väljaandja taotluste väärtus on kataloogi Azure AD rentniku ID-d. Rentniku ID on kataloogi püsiv ja usaldusväärne identifikaatori. <br><br> **Näide SAML väärtus**: <br> `<Issuer>https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/</Issuer>` <br><br> **Näide JWT väärtus**: <br>  `"iss":”https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/”` |
| `family_name` | Perekonnanimi | Pakub viimase nimi, perekonnanimi või perekonnanimi kasutaja määratletud Azure AD kasutaja objekti. <br><br> **Näide SAML väärtus**: <br> `<Attribute Name=” http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname”>`<br>`<AttributeValue>Miller<AttributeValue>` <br><br> **Näide JWT väärtus**: <br> `"family_name": "Miller"` |
| `unique_name`| Nimi | Inimeste loetav väärtus, mis tuvastab luba teema pakub. See väärtus on tagatud rentniku raames olema kordumatu ja on mõeldud kasutamiseks ainult kuvatava taustvärvi. <br><br> **Näide SAML väärtus**: <br> `<Attribute Name=”http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name”>`<br>`<AttributeValue>frankm@contoso.com<AttributeValue>` <br><br> **Näide JWT väärtus**: <br> `"unique_name": "frankm@contoso.com"` |
| `oid` | Objekti ID | Sisaldab Azure AD objekti ainuidentifikaator. See väärtus on püsiv ja seda ei saa ümber või uuesti kasutada. Objekti ID abil saate tuvastada päringute Azure AD objekti. <br><br> **Näide SAML väärtus**: <br> `<Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">`<br>`<AttributeValue>528b2ac2-aa9c-45e1-88d4-959b53bc7dd0<AttributeValue>` <br><br> **Näide JWT väärtus**: <br> `"oid":"528b2ac2-aa9c-45e1-88d4-959b53bc7dd0"` |
| `roles` | Rollid | Tähistab kõik rakenduse rollid, mille teema on antud otse ja kaudselt kaudu rühmakuuluvus ja saate kasutada jõustamiseks Rollipõhine juurdepääsu reguleerimine. Rakenduse rollide on määratletud alusel rakenduste kohta, kuni soovitud `appRoles` Rakendusmanifest atribuut. Funktsiooni `value` iga rakenduse rolli atribuut on väärtus, mis kuvatakse taotluste rollid. <br><br> **Näide SAML väärtus**: <br> `<Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/role">`<br>`<AttributeValue>Admin</AttributeValue>` <br><br> **Näide JWT väärtus**: <br> `“roles”: ["Admin", … ]` |
| `scp` | Ulatus | Näitab isikustamine õiguste klientrakendust. Vaikimisi õigused on `user_impersonation`. Turvalise ressursi omanik saab registreerida väärtused täiendav Azure AD. <br><br> **Näide JWT väärtus**: <br> `"scp": "user_impersonation"`|
| `sub` |Teema| Laenu põhisumma kohta, mis kinnitab luba teave, näiteks rakenduse kasutaja ID. See väärtus ei püsiv ja ei saa ümber või uuesti kasutada, nii et kasutamist autoriseerimine kontrolle turvaline teha. Kuna teema on alati olemas sõned Azure AD probleeme, soovitame selle väärtuse kasutamine üldine otstarve autoriseerimine süsteemi. <br> `SubjectConfirmation`pole nõue. See kirjeldab, kuidas luba teema on kinnitatud. `Bearer`näitab, et teema kinnitab luba nende olemasolu. <br><br> **Näide SAML väärtus**: <br> `<Subject>`<br>`<NameID>S40rgb3XjhFTv6EQTETkEzcgVmToHKRkZUIsJlmLdVc</NameID>`<br>`<SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer" />`<br>`</Subject>` <br><br> **Näide JWT väärtus**: <br> `"sub":"92d0312b-26b9-4887-a338-7b00fb3c5eab"`|
| `tid` | Rentniku ID | Püsiv, ühekordselt kood, mis tuvastab directory rentniku luba väljastanud. Saate selle väärtuse juurdepääsu mitme rentniku rakenduses rentniku kohased directory ressurssidele. Näiteks saate selle väärtuse tuvastamiseks kõne Graph API rentniku. <br><br> **Näide SAML väärtus**: <br> `<Attribute Name=”http://schemas.microsoft.com/identity/claims/tenantid”>`<br>`<AttributeValue>cbb1a5ac-f33b-45fa-9bf5-f37db0fed422<AttributeValue>` <br><br> **Näide JWT väärtus**: <br> `"tid":"cbb1a5ac-f33b-45fa-9bf5-f37db0fed422"`|
| `nbf`, `exp`|Sümboolne eluiga | Määratleb ajavahemiku, mille jooksul on kehtiv märgiks. Teenus, mis kinnitab luba peaks veenduge, et praeguse kuupäeva on sümboolne eluiga, veel selle tagasi lükata luba sees. Teenuse võib kuni viis minutit Lisaks sümboolne eluiga vahemikus luba vahel Azure AD vastavalt kellaaja ("aja viltune") konto ja teenus. <br><br> **Näide SAML väärtus**: <br> `<Conditions`<br>`NotBefore="2013-03-18T21:32:51.261Z"`<br>`NotOnOrAfter="2013-03-18T22:32:51.261Z"`<br>`>` <br><br> **Näide JWT väärtus**: <br> `"nbf":1363289634, "exp":1363293234` |
| `upn`| Kasutaja turvasubjektinimi | Talletab kasutajanime ja kasutajale põhimakse.<br><br> **Näide JWT väärtus**: <br> `"upn": frankm@contoso.com`|
| `ver`| Versioon | Salvestab luba versiooninumber. <br><br> **Näide JWT väärtus**: <br> `"ver": "1.0"`|

## <a name="access-tokens"></a>Accessi sõned

Accessi sõned on ainult sel hetkel tarbitavad Microsoft Services.  Saate teha mõnda valideerimine või Accessi sõned kontrolli mis tahes hetkel toetatud stsenaariumid rakenduste ei vaja.  Te saate käsitleda Accessi sõned läbipaistmatu - nad on lihtsalt stringid, mis rakenduse Microsoft lähevad sisse HTTP päringuid.

Kui te taotleda juurdepääsu sümboolse, tagastab Azure AD ka teatud metaandmeid juurdepääsu luba oma rakenduse tarbeks.  See teave sisaldab aegumise korral luba juurdepääs ja otsinguulatuste, mille jaoks on lubatud.  See võimaldab teha nutikad vahemällu Accessi sõned ilma sõeluda avatud rakenduse juurdepääsu luba ise.

## <a name="refresh-tokens"></a>Märkide värskendamine

Värskenda sõned on rakenduse abil saate hankida uue Accessi märkide mõne OAuth 2.0 meilivoo turvalisus märkide.  See võimaldab rakenduse suhtlust kasutaja nõudmata pikaajalise juurdepääsu ressursid kasutaja nimel saavutamiseks.

Värskenda sõned on mitme ressurss, mis tähendab, et nad võib olla üks ressurss Turbeloa taotluse vastu, kuid hoopis ressursile juurdepääsu sõned lunastada. Mitme ressursside määramiseks määrake soovitud `resource` parameetri taotluse suunatud ressursi.

Värskenda sõned on läbipaistmatu app. Need on vanade, kuid rakenduse tuleks alati kirjutada oodata märgiks värskendamise kestab aega.  Värskenda sõned võib olla mis tahes ajahetkel jaoks mitmel põhjusel kehtetuks.  Ainus viis oma rakenduse teadma, kui värskendamine luba kehtib on proovib lunastada seda tehes Turbeloa taotluse Azure AD Turbeloa lõpp-punkti.

Kui te lunastada värskendamise luba jaoks uus juurdepääsu luba, saate uue värskendamise loa loa vastuse.  Peaksite salvestama äsja väljastatud värskendamise luba, asendaks kasutasite kutse.  See tagab, et teie värskendamise sõned kehtivad nii kaua.

## <a name="validating-tokens"></a>Märkide kontrollimine

Sel hetkel oma kliendi rakendus peaks tuleb teha ainult Turbeloa valideerimine valideerib id_tokens.  Mõne id_token valideerimiseks peaks rakenduse kinnitage nii on id_token allkiri ja selle id_token nõuded.

Pakume teekide ja koodinäiteid, mis näitavad reageerimine hõlpsalt Turbeloa valideerimine, kui soovite mõista aluseks protsessi.  On ka mitu kolmanda osapoole avatud allikas teekide JWT valideerimine, vähemalt üks võimalus peaaegu iga platvormi ja keele jaoks saadaval. Azure AD-autentimine, teekide ja koodinäiteid kohta leiate lisateavet teemast [Azure AD autentimise teekide](active-directory-authentication-libraries.md).

#### <a name="validating-the-signature"></a>Allkirja kontrollimine

Mõne JWT sisaldab kolme segmente, mis on eraldatud soovitud `.` märgi.  Esimese tuntakse **päise** **sisu**teine ja kolmas **allkirja**.  Lõigu allkirja saab kasutada nii, et see saab usaldada, rakenduse soovitud id_token ehtsuse kinnitamiseks.

Id_Tokens logitud valdkonna asümmeetriline tavalise krüptimise algoritmide kohta, nt RSA 256. Funktsiooni id_token päis sisaldab teavet võti ja krüptimise meetod logida luba:

```
{
  "typ": "JWT",
  "alg": "RS256",
  "x5t": "kriMPdmBvx68skT8-mPAB3BseeA"
}
```

Funktsiooni `alg` taotluste näitab algoritmi kasutatud logida Turbeloa, samal ajal soovitud `x5t` taotluste näitab teatud avalik võti, mida kasutati logida luba.

Azure AD võib antud hetkel aega, logige sisse mõne id_token era-avaliku võtme paari kogumi ühe. Azure AD pööratakse võimalik kogum klahvireas perioodiliselt nii, et need muutub automaatselt käsitlema tuleb kirjutada oma rakenduse.  Mõistlik sagedus Azure AD avaliku võtmed värskendusi on kord 24 tunni jooksul.

Saate hankida allkirjastamiseks võtme andmed, kasutades OpenID Connect metaandmete dokument asub allkirja valideerimiseks vajalikud:

```
https://login.microsoftonline.com/common/.well-known/openid-configuration
```

> [AZURE.TIP] Proovige seda URL-i brauseri!

See metaandmete dokument on JSON objekti, mis sisaldavad mitu kasulikku tekstilõigule teave, näiteks asukoha läbimiseks OpenID Connect autentimine nõutav erinevad lõpp-punktid.  

See hõlmab ka on `jwks_uri`, mis annab avaliku abil logige sõned kogumi asukoht.  JSON dokument asub selle `jwks_uri` sisaldab kõiki avaliku olulist teavet kasutab seda teatud ajahetkel.  Rakenduse saate kasutada funktsiooni `kid` taotlemine JWT päises valimiseks, mis avalik võti selles dokumendis on kasutatud logida kindla luba.  Seda saate teha allkirja valideerimine õige avalik võti ja näidatud algoritmi abil.

Allkirja valideerimine läbimiseks on selle dokumendi väljapoole – mitme avatud allika teekide on saadaval, mis aitavad teil teha vajaduse korral.

#### <a name="validating-the-claims"></a>Nõuded kontrollimine

Kui minirakenduse saab ka id_token korral kasutajate sisselogimist, seda tegema ka mõned kontrollid nõuete soovitud id_token.  Need sisaldavad, kuid ei ole piiratud:

  - **Sihtrühma** nõude - kinnitamiseks pidi soovitud id_token rakenduse pöörata.
  - **Pole enne** ja **Aegumise aja** nõuete - kinnitamaks, et selle id_token on aegunud, ei.
  - **Väljaandja** nõude - kinnitamaks, et luba tõepoolest väljastanud app Azure AD.
  - Funktsiooni **nonss** - loa kordus rünnak leevendada.
  - ja muud...

Vaadake taotluste valideerimised rakenduse tuleks teha täieliku loendi leiate [OpenID Connect määratlus](http://openid.net/specs/openid-connect-core-1_0.html#IDTokenValidation).

Oodatud väärtusega need andmed on kaasatud [id_token jaotis](#id-tokens) eelmises jaotises.

## <a name="sample-tokens"></a>Valimi sõned

Selles jaotises kuvatakse SAML ja JWT märgid, mis tagastab Azure AD näited. Need näited võimaldavad teil näha taotluste kontekstis.
SAML luba

See on tüüpilised SAML luba valimil.

    <?xml version="1.0" encoding="UTF-8"?>
    <t:RequestSecurityTokenResponse xmlns:t="http://schemas.xmlsoap.org/ws/2005/02/trust">
      <t:Lifetime>
        <wsu:Created xmlns:wsu="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd">2014-12-24T05:15:47.060Z</wsu:Created>
        <wsu:Expires xmlns:wsu="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd">2014-12-24T06:15:47.060Z</wsu:Expires>
      </t:Lifetime>
      <wsp:AppliesTo xmlns:wsp="http://schemas.xmlsoap.org/ws/2004/09/policy">
        <EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
          <Address>https://contoso.onmicrosoft.com/MyWebApp</Address>
        </EndpointReference>
      </wsp:AppliesTo>
      <t:RequestedSecurityToken>
        <Assertion xmlns="urn:oasis:names:tc:SAML:2.0:assertion" ID="_3ef08993-846b-41de-99df-b7f3ff77671b" IssueInstant="2014-12-24T05:20:47.060Z" Version="2.0">
          <Issuer>https://sts.windows.net/b9411234-09af-49c2-b0c3-653adc1f376e/</Issuer>
          <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
            <ds:SignedInfo>
              <ds:CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
              <ds:SignatureMethod Algorithm="http://www.w3.org/2001/04/xmldsig-more#rsa-sha256" />
              <ds:Reference URI="#_3ef08993-846b-41de-99df-b7f3ff77671b">
                <ds:Transforms>
                  <ds:Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature" />
                  <ds:Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
                </ds:Transforms>
                <ds:DigestMethod Algorithm="http://www.w3.org/2001/04/xmlenc#sha256" />
                <ds:DigestValue>cV1J580U1pD24hEyGuAxrbtgROVyghCqI32UkER/nDY=</ds:DigestValue>
              </ds:Reference>
            </ds:SignedInfo>
            <ds:SignatureValue>j+zPf6mti8Rq4Kyw2NU2nnu0pbJU1z5bR/zDaKaO7FCTdmjUzAvIVfF8pspVR6CbzcYM3HOAmLhuWmBkAAk6qQUBmKsw+XlmF/pB/ivJFdgZSLrtlBs1P/WBV3t04x6fRW4FcIDzh8KhctzJZfS5wGCfYw95er7WJxJi0nU41d7j5HRDidBoXgP755jQu2ZER7wOYZr6ff+ha+/Aj3UMw+8ZtC+WCJC3yyENHDAnp2RfgdElJal68enn668fk8pBDjKDGzbNBO6qBgFPaBT65YvE/tkEmrUxdWkmUKv3y7JWzUYNMD9oUlut93UTyTAIGOs5fvP9ZfK2vNeMVJW7Xg==</ds:SignatureValue>
            <KeyInfo xmlns="http://www.w3.org/2000/09/xmldsig#">
              <X509Data>
                <X509Certificate>MIIDPjCCAabcAwIBAgIQsRiM0jheFZhKk49YD0SK1TAJBgUrDgMCHQUAMC0xKzApBgNVBAMTImFjY291bnRzLmFjY2Vzc2NvbnRyb2wud2luZG93cy5uZXQwHhcNMTQwMTAxMDcwMDAwWhcNMTYwMTAxMDcwMDAwWjAtMSswKQYDVQQDEyJhY2NvdW50cy5hY2Nlc3Njb250cm9sLndpbmRvd3MubmV0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAkSCWg6q9iYxvJE2NIhSyOiKvqoWCO2GFipgH0sTSAs5FalHQosk9ZNTztX0ywS/AHsBeQPqYygfYVJL6/EgzVuwRk5txr9e3n1uml94fLyq/AXbwo9yAduf4dCHTP8CWR1dnDR+Qnz/4PYlWVEuuHHONOw/blbfdMjhY+C/BYM2E3pRxbohBb3x//CfueV7ddz2LYiH3wjz0QS/7kjPiNCsXcNyKQEOTkbHFi3mu0u13SQwNddhcynd/GTgWN8A+6SN1r4hzpjFKFLbZnBt77ACSiYx+IHK4Mp+NaVEi5wQtSsjQtI++XsokxRDqYLwus1I1SihgbV/STTg5enufuwIDAQABo2IwYDBeBgNVHQEEVzBVgBDLebM6bK3BjWGqIBrBNFeNoS8wLTErMCkGA1UEAxMiYWNjb3VudHMuYWNjZXNzY29udHJvbC53aW5kb3dzLm5ldIIQsRiM0jheFZhKk49YD0SK1TAJBgUrDgMCHQUAA4IBAQCJ4JApryF77EKC4zF5bUaBLQHQ1PNtA1uMDbdNVGKCmSp8M65b8h0NwlIjGGGy/unK8P6jWFdm5IlZ0YPTOgzcRZguXDPj7ajyvlVEQ2K2ICvTYiRQqrOhEhZMSSZsTKXFVwNfW6ADDkN3bvVOVbtpty+nBY5UqnI7xbcoHLZ4wYD251uj5+lo13YLnsVrmQ16NCBYq2nQFNPuNJw6t3XUbwBHXpF46aLT1/eGf/7Xx6iy8yPJX4DyrpFTutDz882RWofGEO5t4Cw+zZg70dJ/hH/ODYRMorfXEW+8uKmXMKmX2wyxMKvfiPbTy5LmAU8Jvjs2tLg4rOBcXWLAIarZ</X509Certificate>
              </X509Data>
            </KeyInfo>
          </ds:Signature>
          <Subject>
            <NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent">m_H3naDei2LNxUmEcWd0BZlNi_jVET1pMLR6iQSuYmo</NameID>
            <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer" />
          </Subject>
          <Conditions NotBefore="2014-12-24T05:15:47.060Z" NotOnOrAfter="2014-12-24T06:15:47.060Z">
            <AudienceRestriction>
              <Audience>https://contoso.onmicrosoft.com/MyWebApp</Audience>
            </AudienceRestriction>
          </Conditions>
          <AttributeStatement>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">
              <AttributeValue>a1addde8-e4f9-4571-ad93-3059e3750d23</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/tenantid">
              <AttributeValue>b9411234-09af-49c2-b0c3-653adc1f376e</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">
              <AttributeValue>sample.admin@contoso.onmicrosoft.com</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname">
              <AttributeValue>Admin</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname">
              <AttributeValue>Sample</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/groups">
              <AttributeValue>5581e43f-6096-41d4-8ffa-04e560bab39d</AttributeValue>
              <AttributeValue>07dd8a89-bf6d-4e81-8844-230b77145381</AttributeValue>
              <AttributeValue>0e129f4g-6b0a-4944-982d-f776000632af</AttributeValue>
              <AttributeValue>3ee07328-52ef-4739-a89b-109708c22fb5</AttributeValue>
              <AttributeValue>329k14b3-1851-4b94-947f-9a4dacb595f4</AttributeValue>
              <AttributeValue>6e32c650-9b0a-4491-b429-6c60d2ca9a42</AttributeValue>
              <AttributeValue>f3a169a7-9a58-4e8f-9d47-b70029v07424</AttributeValue>
              <AttributeValue>8e2c86b2-b1ad-476d-9574-544d155aa6ff</AttributeValue>
              <AttributeValue>1bf80264-ff24-4866-b22c-6212e5b9a847</AttributeValue>
              <AttributeValue>4075f9c3-072d-4c32-b542-03e6bc678f3e</AttributeValue>
              <AttributeValue>76f80527-f2cd-46f4-8c52-8jvd8bc749b1</AttributeValue>
              <AttributeValue>0ba31460-44d0-42b5-b90c-47b3fcc48e35</AttributeValue>
              <AttributeValue>edd41703-8652-4948-94a7-2d917bba7667</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/identityprovider">
              <AttributeValue>https://sts.windows.net/b9411234-09af-49c2-b0c3-653adc1f376e/</AttributeValue>
            </Attribute>
          </AttributeStatement>
          <AuthnStatement AuthnInstant="2014-12-23T18:51:11.000Z">
            <AuthnContext>
              <AuthnContextClassRef>urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
            </AuthnContext>
          </AuthnStatement>
        </Assertion>
      </t:RequestedSecurityToken>
      <t:RequestedAttachedReference>
        <SecurityTokenReference xmlns="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd" xmlns:d3p1="http://docs.oasis-open.org/wss/oasis-wss-wssecurity-secext-1.1.xsd" d3p1:TokenType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0">
          <KeyIdentifier ValueType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLID">_3ef08993-846b-41de-99df-b7f3ff77671b</KeyIdentifier>
        </SecurityTokenReference>
      </t:RequestedAttachedReference>
      <t:RequestedUnattachedReference>
        <SecurityTokenReference xmlns="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd" xmlns:d3p1="http://docs.oasis-open.org/wss/oasis-wss-wssecurity-secext-1.1.xsd" d3p1:TokenType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0">
          <KeyIdentifier ValueType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLID">_3ef08993-846b-41de-99df-b7f3ff77671b</KeyIdentifier>
        </SecurityTokenReference>
      </t:RequestedUnattachedReference>
      <t:TokenType>http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0</t:TokenType>
      <t:RequestType>http://schemas.xmlsoap.org/ws/2005/02/trust/Issue</t:RequestType>
      <t:KeyType>http://schemas.xmlsoap.org/ws/2005/05/identity/NoProofKey</t:KeyType>
    </t:RequestSecurityTokenResponse>

### <a name="jwt-token---user-impersonation"></a>JWT luba – kasutaja isikustamine

See on tüüpilised JSON web märgiks (JWT) kasutatakse autoriseerimine koodi anda meilivoo valimil.
Lisaks taotluste, sisaldab luba versiooninumber **ver** ja **appidacr**, autentimine kontekstis klassi viide, mis näitab, kuidas kliendi kinnitamist. Avaliku klient, väärtus 0. Kui kliendi ID või kliendi salajane kasutati, väärtus 1.

    {
     typ: "JWT",
     alg: "RS256",
     x5t: "kriMPdmBvx68skT8-mPAB3BseeA"
    }.
    {
     aud: "https://contoso.onmicrosoft.com/scratchservice",
     iss: "https://sts.windows.net/b9411234-09af-49c2-b0c3-653adc1f376e/",
     iat: 1416968588,
     nbf: 1416968588,
     exp: 1416972488,
     ver: "1.0",
     tid: "b9411234-09af-49c2-b0c3-653adc1f376e",
     amr: [
      "pwd"
     ],
     roles: [
      "Admin"
     ],
     oid: "6526e123-0ff9-4fec-ae64-a8d5a77cf287",
     upn: "sample.user@contoso.onmicrosoft.com",
     unique_name: "sample.user@contoso.onmicrosoft.com",
     sub: "yf8C5e_VRkR1egGxJSDt5_olDFay6L5ilBA81hZhQEI",
     family_name: "User",
     given_name: "Sample",
     groups: [
      "0e129f6b-6b0a-4944-982d-f776000632af",
      "323b13b3-1851-4b94-947f-9a4dacb595f4",
      "6e32c250-9b0a-4491-b429-6c60d2ca9a42",
      "f3a161a7-9a58-4e8f-9d47-b70022a07424",
      "8d4c81b2-b1ad-476d-9574-544d155aa6ff",
      "1bf80164-ff24-4866-b19c-6212e5b9a847",
      "76f80127-f2cd-46f4-8c52-8edd8bc749b1",
      "0ba27160-44d0-42b5-b90c-47b3fcc48e35"
     ],
     appid: "b075ddef-0efa-123b-997b-de1337c29185",
     appidacr: "1",
     scp: "user_impersonation",
     acr: "1"
    }.

## <a name="related-content"></a>Seotud teemad
- Sümboolne eluiga poliitika Azure AD Graph API kaudu haldamise kohta saate lisateavet teemast Azure AD Graphi [toimingutest](https://msdn.microsoft.com/library/azure/ad/graph/api/policy-operations) ja [poliitika üksus](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#policy-entity).
- Vt lisateavet haldamise poliitikate kaudu PowerShelli cmdlet-käskude, sh näidised, näidiseid ja [Konfigureeritav Turbeloa eluajal Azure AD](active-directory-configurable-token-lifetimes.md). 
