<properties
    pageTitle="Azure'i ühekordse sisselogimise SAML protokoll | Microsoft Azure'i"
    description="Selles artiklis kirjeldatakse ühekordse sisselogimise kohta SAML protokoll Azure Active Directory 's"
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

# <a name="single-sign-on-saml-protocol"></a>Ühekordse sisselogimise SAML Protocol (protokoll)

Selles artiklis antakse ülevaade SAML 2.0 autentimist taotlusi ja Azure Active Directory (Azure AD) toetab jaoks ühekordse sisselogimise vastused.

Protokoll alloleval joonisel kirjeldatakse ühekordse sisselogimise jada. Pilveteenuses (teenusepakkuja) kasutab mõnda HTTP Redirect sidumine edasi ka `AuthnRequest` (autentimine taotluse) elemendi Azure AD (identiteedipakkuja). Azure AD seejärel kasutab HTTP postituse, mis sidumine postituse lisamine `Response` element on pilveteenusesse.

![Ühekordse sisselogimise töövoo](media/active-directory-single-sign-on-protocol-reference/active-directory-saml-single-sign-on-workflow.png)

## <a name="authnrequest"></a>AuthnRequest

Taotleda kasutaja autentimise, cloud services saada mõne `AuthnRequest` elemendi Azure AD. Valimi SAML 2.0 `AuthnRequest` võib välja näha selline:

```
<samlp:AuthnRequest
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="id6c1c178c166d486687be4aaf5e482730"
Version="2.0" IssueInstant="2013-03-18T03:28:54.1839884Z"
xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.contoso.com</Issuer>
</samlp:AuthnRequest>
```


| Parameetri | | Kirjeldus |
| ----------------------- | ------------------------------- | --------------- |
| ID | nõutav | Azure AD kasutab atribuudi asustamiseks on `InResponseTo` atribuut tagastatud vastus. ID peab algama arv, seega ühise strateegia prepend stringi nagu "id" stringi kujutise GUID. Näiteks `id6c1c178c166d486687be4aaf5e482730` on kehtiv ID. |
| Versioon | nõutav | See peaks olema **2.0**.|
| IssueInstant | nõutav | See on kuupäev ja kellaaeg stringi UTC väärtus ja [edasi vormingus ("s")](https://msdn.microsoft.com/library/az4se3k1.aspx). Azure AD eeldab, et seda tüüpi väärtus DateTime, kuid hinnata või kasutage väärtust. |
| AssertionConsumerServiceUrl | valikuline. | Kui see on esitatud see peab vastama selle `RedirectUri` , pilveteenus Azure AD. |
| ForceAuthn | valikuline. | Kui, see peaks olema false. Mis tahes muu väärtus põhjustab vea.|
| Seda IsPassive | valikuline. | Kui, see peaks olema false. Mis tahes muu väärtus põhjustab vea. |  

Kõik muud `AuthnRequest` atribuute, nagu näiteks nõusolekut, sihtkoha, AssertionConsumerServiceIndex, AttributeConsumerServiceIndex ja ProviderName on **ignoreeritud**.

Azure AD ka ignoreerib selle `Conditions` element `AuthnRequest`.

### <a name="issuer"></a>Väljaandja

Funktsiooni `Issuer` element on `AuthnRequest` peab täpselt vastama ühte **ServicePrincipalNames** pilveteenusesse Azure AD. Tavaliselt on see määratud **Rakenduse ID URI** rakenduse registreerimisel määratud.

Valimi SAML väljavõte sisaldavad soovitud `Issuer` element, mis näeb välja umbes järgmine:

```
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.contoso.com</Issuer>
```

### <a name="nameidpolicy"></a>NameIDPolicy

– See element taotleb kindla nime ID vorming vastuse ja on valikuline `AuthnRequest` elemente, mis on saadetud Azure AD.

Valimi `NameIdPolicy` element, mis näeb välja umbes järgmine:

```
<NameIDPolicy Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent"/>
```

Kui `NameIDPolicy` esitatakse, saate lisada oma valikuline `Format` atribuut. Funktsiooni `Format` atribuut võib olla ainult üks järgmistest väärtustest; mis tahes muu väärtus annab tulemuseks vea.

-  `urn:oasis:names:tc:SAML:2.0:nameid-format:persistent`: Azure Active Directory probleemid NameID taotluste paariviisiline identifikaator nimega.
- `urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress`: Azure Active Directory probleemid NameID taotluste vormingus e-posti aadress.
- `urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified`: Väärtus seda lubab Azure Active Directory taotluste vormingu valimine. Azure Active Directory probleemid on NameID nimega paariviisiline identifikaator.

Ärge lisage soovitud `SPNameQualifer` atribuut. Azure AD ignoreerib selle `AllowCreate` atribuut.

### <a name="requestauthncontext"></a>RequestAuthnContext

Funktsiooni `RequestedAuthnContext` element määrab soovitud autentimismeetodite. See on valikuline `AuthnRequest` elemente, mis on saadetud Azure AD. Azure AD toetab ainult üks `AuthnContextClassRef` väärtus: `urn:oasis:names:tc:SAML:2.0:ac:classes:Password`.

### <a name="scoping"></a>Ulatuse määramine

Funktsiooni `Scoping` element, mis sisaldab loendit identiteedipakkujad, on valikuline `AuthnRequest` elemente, mis on saadetud Azure AD.

Kui see on esitatud ei sisalda soovitud `ProxyCount` atribuut, `IDPListOption` või `RequesterID` element, nagu nad ei toetata.

### <a name="signature"></a>Allkiri

Ei sisalda mõnda `Signature` element `AuthnRequest` elemente, Azure AD ei toeta allkirjastatud taotluste autentimise.

### <a name="subject"></a>Teema

Azure AD ignoreerib selle `Subject` element `AuthnRequest` elemente.

## <a name="response"></a>Vastus

Kui on nõutud sisselogimise lõpule jõudnud edukalt, Azure AD postituste vastuseks pilveteenusesse. Eduka sisselogimise katsel vastuseks valimi näeb välja umbes järgmine:

```
<samlp:Response ID="_a4958bfd-e107-4e67-b06d-0d85ade2e76a" Version="2.0" IssueInstant="2013-03-18T07:38:15.144Z" Destination="https://contoso.com/identity/inboundsso.aspx" InResponseTo="id758d0ef385634593a77bdf7e632984b6" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
    ...
  </ds:Signature>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success" />
  </samlp:Status>
  <Assertion ID="_bf9c623d-cc20-407a-9a59-c2d0aee84d12" IssueInstant="2013-03-18T07:38:15.144Z" Version="2.0" xmlns="urn:oasis:names:tc:SAML:2.0:assertion">
    <Issuer>https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
    <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
      ...
    </ds:Signature>
    <Subject>
      <NameID>Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
      <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
        <SubjectConfirmationData InResponseTo="id758d0ef385634593a77bdf7e632984b6" NotOnOrAfter="2013-03-18T07:43:15.144Z" Recipient="https://contoso.com/identity/inboundsso.aspx" />
      </SubjectConfirmation>
    </Subject>
    <Conditions NotBefore="2013-03-18T07:38:15.128Z" NotOnOrAfter="2013-03-18T08:48:15.128Z">
      <AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
      </AudienceRestriction>
    </Conditions>
    <AttributeStatement>
      <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">
        <AttributeValue>testuser@contoso.com</AttributeValue>
      </Attribute>
      <Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">
        <AttributeValue>3F2504E0-4F89-11D3-9A0C-0305E82C3301</AttributeValue>
      </Attribute>
      ...
    </AttributeStatement>
    <AuthnStatement AuthnInstant="2013-03-18T07:33:56.000Z" SessionIndex="_bf9c623d-cc20-407a-9a59-c2d0aee84d12">
      <AuthnContext>
        <AuthnContextClassRef> urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
      </AuthnContext>
    </AuthnStatement>
  </Assertion>
</samlp:Response>
```

### <a name="response"></a>Vastus

Funktsiooni `Response` element sisaldab luba taotluse. Azure'i AD komplektid on `ID`, `Version` ja `IssueInstant` väärtused on `Response` element. Seda määrab ka järgmisi atribuute:

- `Destination`: Kui sisselogimise lõpule jõudnud edukalt, see on seatud soovitud `RedirectUri` teenusepakkuja (pilveteenuses).
- `InResponseTo`: See on seatud soovitud `ID` atribuut on `AuthnRequest` element, mis algatatud vastus.

### <a name="issuer"></a>Väljaandja

Azure'i AD komplekti soovitud `Issuer` elemendi `https://login.microsoftonline.com/<TenantIDGUID>/` kus <TenantIDGUID> on Azure AD rentniku rentniku ID-d.

Näiteks valimi vastuse väljaandja element võib välja näha selline:

```
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
```

### <a name="status"></a>Olek

Funktsiooni `Status` elemendi loob takistavaid sisselogimise. See sisaldab selle `StatusCode` element, mis sisaldab koodi või pesastatud koodid, mis tähistavad taotluse olekut kogum. See hõlmab ka selle `StatusMessage` element, mis sisaldab kohandatud tõrketeated, mis on loodud käigus sisselogimist.

<!-- TODO: Add a authentication protocol error reference -->

Järgmises SAML vastuseks õnnestu sisselogimise katse.

```
<samlp:Response ID="_f0961a83-d071-4be5-a18c-9ae7b22987a4" Version="2.0" IssueInstant="2013-03-18T08:49:24.405Z" InResponseTo="iddce91f96e56747b5ace6d2e2aa9d4f8c" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://sts.windows.net/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Requester">
      <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:RequestUnsupported" />
    </samlp:StatusCode>
    <samlp:StatusMessage>AADSTS75006: An error occurred while processing a SAML2 Authentication request. AADSTS90011: The SAML authentication request property 'NameIdentifierPolicy/SPNameQualifier' is not supported.
Trace ID: 66febed4-e737-49ff-ac23-464ba090d57c
Timestamp: 2013-03-18 08:49:24Z</samlp:StatusMessage>
  </samlp:Status>
```

### <a name="assertion"></a>Argument

Lisaks on `ID`, `IssueInstant` ja `Version`, Azure AD seab järgmisi elemente selle `Assertion` elemendi tagasiside.

#### <a name="issuer"></a>Väljaandja

See on seatud `https://sts.windows.net/<TenantIDGUID>/`kus <TenantIDGUID> on Azure AD rentniku rentniku ID-d.

```
<Issuer>https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
```

#### <a name="signature"></a>Allkiri

Azure AD märke väide on eduka sisselogimise vastuseks. Funktsiooni `Signature` element on digitaalallkiri autentida allika terviklust väide pilveteenusesse kasutavad.

See digitaalallkiri genereerimiseks kasutab Azure AD allkirjastamiseks Sisestage soovitud `IDPSSODescriptor` selle metaandmete dokumendi osa.

```
<ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
      digital_signature_here
    </ds:Signature>
```

#### <a name="subject"></a>Teema

Seda määrab laenu põhisumma, mis on teema on aruannete kinnitus. See sisaldab mõnda `NameID` element, mis tähistab autenditud kasutaja. Funktsiooni `NameID` väärtus on suunatud identifikaator, mis on suunatud ainult teenusepakkuja, mis on publiku luba. See on püsiv – seda saab tühistada, kuid mitte kunagi ümber. Samuti on läbipaistmatu, ei saa kasutada identifikaatori atribuut päringuid ja anna kasutaja kohta.

Funktsiooni `Method` atribuut on `SubjectConfirmation` element on alati seatud `urn:oasis:names:tc:SAML:2.0:cm:bearer`.

```
<Subject>
      <NameID>Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
      <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
        <SubjectConfirmationData InResponseTo="id758d0ef385634593a77bdf7e632984b6" NotOnOrAfter="2013-03-18T07:43:15.144Z" Recipient="https://contoso.com/identity/inboundsso.aspx" />
      </SubjectConfirmation>
</Subject>
```

#### <a name="conditions"></a>Tingimused

– See element saate määrata tingimused, mis määratlevad SAML kinnitused aktsepteeritav kasutamine.

```
<Conditions NotBefore="2013-03-18T07:38:15.128Z" NotOnOrAfter="2013-03-18T08:48:15.128Z">
      <AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
      </AudienceRestriction>
</Conditions>
```

Funktsiooni `NotBefore` ja `NotOnOrAfter` atribuutide määramine intervall, mille jooksul väide on kehtiv.

- Väärtus on `NotBefore` atribuut on veidi (vähem kui teine) väärtus hiljem `IssueInstant` atribuut on `Assertion` element. Azure AD arvele mis tahes aja erinevus ise ja pilveteenuses (teenuse pakkuja) ja ei saa lisada mis tahes puhvri seekord.
- Väärtus on `NotOnOrAfter` atribuut on 70 minutit hiljem, kui väärtus on `NotBefore` atribuut.

#### <a name="audience"></a>Sihtrühma

See sisaldab URI, mis tuvastab publikule ettenähtud. Azure AD määrab – see element väärtus väärtus `Issuer` element on `AuthnRequest` mis algatatud Logi sisse. Hindamaks, kui selle `Audience` väärtus, kasutage väärtus on `App ID URI` määratud rakenduse registreerimisel.

```
<AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
</AudienceRestriction>
```

Nagu on `Issuer` väärtus on `Audience` väärtus peab täpselt vastama üks teenuse põhilist nime, mis tähistab pilveteenusesse Azure AD. Kui väärtus on `Issuer` element pole URI väärtus, on `Audience` vastus väärtus on soovitud `Issuer` väärtuse eesliide `spn:`.

#### <a name="attributestatement"></a>AttributeStatement

See sisaldab taotluste teema või kasutaja kohta. Järgmised väljavõte sisaldab valimi `AttributeStatement` element. Kolm punkti näitab, et elemendi võib sisaldada mitut atribuudid ja atribuudiväärtused.

```
<AttributeStatement>
      <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">
        <AttributeValue>testuser@contoso.com</AttributeValue>
      </Attribute>
      <Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">
        <AttributeValue>3F2504E0-4F89-11D3-9A0C-0305E82C3301</AttributeValue>
      </Attribute>
      ...
</AttributeStatement>
```     

- **Nime taotluste** : väärtus on `Name` atribuut (`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`) on autenditud kasutaja kasutaja nime nagu `testuser@managedtenant.com`.
- **ObjectIdentifier taotluste** : väärtus on `ObjectIdentifier` atribuut (`http://schemas.microsoft.com/identity/claims/objectidentifier`) on soovitud `ObjectId` directory objekti, mis tähistab autenditud kasutaja Azure AD. `ObjectId`ei püsiv, globaalselt kordumatu ja uuesti kasutada autenditud kasutaja turvaliste ainuidentifikaator.

#### <a name="authnstatement"></a>AuthnStatement

– See element väidab, et argument teema kinnitamist teatud viisil teatud ajal.

- Funktsiooni `AuthnInstant` atribuut määrab aeg, mille autenditud kasutaja Azure AD.
- Funktsiooni `AuthnContext` element määrab kasutatakse kasutaja autentimise kontekstis.

```
<AuthnStatement AuthnInstant="2013-03-18T07:33:56.000Z" SessionIndex="_bf9c623d-cc20-407a-9a59-c2d0aee84d12">
      <AuthnContext>
        <AuthnContextClassRef> urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
      </AuthnContext>
</AuthnStatement>
```
