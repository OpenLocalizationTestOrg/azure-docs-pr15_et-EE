<properties
    pageTitle="Azure'i ühe Logi välja SAML protokoll | Microsoft Azure'i"
    description="Selles artiklis kirjeldatakse ühe Sign-Out SAML protokoll Azure Active Directory 's"
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


# <a name="single-sign-out-saml-protocol"></a>Ühe väljunud SAML Protocol (protokoll)

Azure Active Directory (Azure AD) toetab SAML 2.0 web brauseri väljunud üks profiil. Ühe väljunud jaoks õige registreerima Azure AD metaandmete URL-i rakenduse registreerimisel. Azure AD saab metaandmete välju URL-i ja allkirjastamiseks võti pilveteenusesse. Azure AD kasutab allkirjastamiseks võti kinnitamaks, et sissetulevad LogoutRequest allkiri ja kasutab funktsiooni LogoutURL ümber suunata kasutajate pärast välja logitud.

Kui pilveteenusesse ei toeta metaandmete lõpp-punkti, kui rakendus on registreeritud, peab arendaja pöörduge Microsofti toe välju URL-i ja allkirjastamiseks võti.

Diagramm kuvab töövoo Azure AD ühe väljunud käigus.

![Ühekordse sisselogimise välja töövoo](media/active-directory-single-sign-out-protocol-reference/active-directory-saml-single-sign-out-workflow.png)

## <a name="logoutrequest"></a>LogoutRequest

Pilveteenuse teenus saadab on `LogoutRequest` Azure AD sõnum, mis näitab, et seansi lõpetada. Kuvatakse järgmised väljavõte valimi `LogoutRequest` element.

```
<samlp:LogoutRequest xmlns="urn:oasis:names:tc:SAML:2.0:metadata" ID="idaa6ebe6839094fe4abc4ebd5281ec780" Version="2.0" IssueInstant="2013-03-28T07:10:49.6004822Z" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.workaad.com</Issuer>
  <NameID xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
</samlp:LogoutRequest>
```

### <a name="logoutrequest"></a>LogoutRequest

Funktsiooni `LogoutRequest` saadetakse Azure AD elemendi jaoks on vaja järgmisi atribuute:

- `ID`: See tuvastab väljunud kutse. Väärtus `ID` peaks ei alga arvu. Tüüpilised on string kujutis GUID **id** lisada.

- `Version`: Määrake selle elemendi väärtuse **2.0**. See väärtus on nõutav.

- `IssueInstant`: See on mõne `DateTime` tekstistringi koordineerimine maailmaaega (UTC) väärtus ja [edasi vormingus ("s")](https://msdn.microsoft.com/library/az4se3k1.aspx). Azure AD eeldab väärtust, mille seda tüüpi, kuid ei jõusta.

- Funktsiooni `Consent`, `Destination`, `NotOnOrAfter` ja `Reason` atribuute ignoreeritakse, kui need sisalduvad on `LogoutRequest` element.

### <a name="issuer"></a>Väljaandja

Funktsiooni `Issuer` element on `LogoutRequest` peab täpselt vastama ühte **ServicePrincipalNames** pilveteenusesse Azure AD. Tavaliselt on see määratud **Rakenduse ID URI** rakenduse registreerimisel määratud.

### <a name="nameid"></a>NameID

Väärtus on `NameID` elemendi peab täpselt vastama selle `NameID` välja logitud kasutaja.
## <a name="logoutresponse"></a>LogoutResponse

Azure'i AD saadab on `LogoutResponse` vastuseks on `LogoutRequest` element. Kuvatakse järgmised väljavõte valimi `LogoutResponse`.

```
<samlp:LogoutResponse ID="_f0961a83-d071-4be5-a18c-9ae7b22987a4" Version="2.0" IssueInstant="2013-03-18T08:49:24.405Z" InResponseTo="iddce91f96e56747b5ace6d2e2aa9d4f8c" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://sts.windows.net/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success" />
  </samlp:Status>
</samlp:LogoutResponse>
```

### <a name="logoutresponse"></a>LogoutResponse

Azure'i AD komplektides on `ID`, `Version` ja `IssueInstant` väärtused on `LogoutResponse` element. Seda määrab ka selle `InResponseTo` elemendi väärtuse soovitud `ID` atribuut on `LogoutRequest` mis tiitrite vastuse.

### <a name="issuer"></a>Väljaandja

Azure AD määrab selle väärtuse `https://login.microsoftonline.com/<TenantIdGUID>/` kus <TenantIdGUID> on Azure AD rentniku rentniku ID-d.

Hindamaks, kui väärtus on `Issuer` element, kasutage **Rakenduse ID URI** väärtus rakenduse registreerimisel.

### <a name="status"></a>Olek

Azure'i AD kasutab funktsiooni `StatusCode` element on `Status` elemendi näitamaks, väljunud takistavaid. Kui väljunud loomise katse nurjus, kuvatakse `StatusCode` elemendi võib sisaldada ka kohandatud tõrketeated.
