<properties
    pageTitle="Azure AD Federation metaandmete | Microsoft Azure'i"
    description="Selles artiklis kirjeldatakse federation metaandmete dokument, mille Azure Active Directory avaldab Azure Active Directory sõned aktsepteerib teenuste jaoks."
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


# <a name="federation-metadata"></a>Federation metaandmed

Azure Active Directory (Azure AD) avaldab dokumendi federation metaandmete teenused, mis on konfigureeritud aktsepteerima Azure AD probleemid märkide turvalisus. Federation metaandmete dokumendi vorming on kirjeldatud [Web Services Federation keele (oli ühinemine) versioon 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html), mis ulatub [metaandmete v2.0 OASIS turvalisuse väide Markup Language (SAML)](http://docs.oasis-open.org/security/saml/v2.0/saml-metadata-2.0-os.pdf).

## <a name="tenant-specific-and-tenant-independent-metadata-endpoints"></a>Rentniku kohased ja rentniku sõltumatul metaandmete lõpp-punktid

Azure AD avaldab rentniku kohased ja rentniku sõltumatul lõpp-punktid.

Rentniku kohased lõpp-punktid on mõeldud eelkõige rentniku jaoks. Rentniku kohased federation metaandmete sisaldab teavet rentniku, sh rentniku väljaandja ja lõpp-punkti teave. Rakendusi, mis on ühe rentniku juurdepääsu piiramiseks kasutada rentniku kohased lõpp-punktid.

Rentniku sõltumatul lõpp-punktid teavet, mis on levinud kõik Azure AD rentnikega. See teave kehtib rentnikud, mis on majutatud veebisaidil *login.microsoftonline.com* ja rentnikud ühiselt. Rentniku sõltumatul lõpp-punktid on soovitatav mitme rentniku rakenduste, kuna seostatud mis tahes kindla rentniku.

## <a name="federation-metadata-endpoints"></a>Lõpp-punktid Federation metaandmed

Azure AD avaldab federation metaandmete veebisaidil `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`.

**Rentniku kohased lõpp-punktid**puhul on `TenantDomainName` võib olla üks järgmistest:

- Registreeritud domeeninimi on Azure AD rentniku, näiteks: `contoso.onmicrosoft.com`.
- Funktsiooni püsiv rentniku ID domeeninime, näiteks `72f988bf-86f1-41af-91ab-2d7cd011db45`.

**Rentniku sõltumatul lõpp-punktid**puhul on `TenantDomainName` on `common`. Selles dokumendis on loetletud ainult Federation metaandmete elemente, mis on levinud kõik Azure AD rentnikega, mis on majutatud veebisaidil login.microsoftonline.com.

Võib olla näiteks rentniku kohased endpoint `https:// login.microsoftonline.com/contoso.onmicrosoft.com/FederationMetadata/2007-06/FederationMetadata.xml`. Rentniku sõltumatul lõpp-punkti on [https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml](https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml). Saate vaadata federation metaandmete dokument, tippides selle URL-i brauseris.

## <a name="contents-of-federation-metadata"></a>Sisu metaandmete federation

Järgmises jaotises antakse teenused, mis kasutavad märgid, mis on antud Azure AD vajalik teave.

### <a name="entity-id"></a>Üksuse ID

Funktsiooni `EntityDescriptor` element, mis sisaldab ka `EntityID` atribuut. Väärtus on `EntityID` atribuut tähistab väljaandja, st turvalisus Turbeloa teenuse (STS) väljastatud luba. See on oluline valideerimiseks väljaandja, kui saate märgiks.

Kuvatakse järgmised metaandmete valimi rentniku kohased `EntityDescriptor` element on `EntityID` element.

```
<EntityDescriptor
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="_b827a749-cfcb-46b3-ab8b-9f6d14a1294b"
entityID="https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db45/">
```
Saate asendada selle rentniku ID rentniku sõltumatul lõpp-punkti loomiseks kindla rentniku oma rentniku ID-ga `EntityID` väärtus. Tulemiks oleva väärtuse on sama, mis loa väljaandja. Strateegia rakendusel mitme rentniku, valideerimiseks väljaandja antud rentniku jaoks.

Kuvatakse järgmised metaandmete valimi rentniku sõltumatul `EntityID` element. Pange tähele, et selle `{tenant}` on, mitte kohatäite.

```
<EntityDescriptor
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="="_0e5bd9d0-49ef-4258-bc15-21ce143b61bd"
entityID="https://sts.windows.net/{tenant}/">
```

### <a name="token-signing-certificates"></a>Turbeloa allkirjastamiseks serdid

Kui teenus saab luba, mis on välja andnud Azure AD rentniku, tuleb luba allkirja kontrollida allkirjastamiseks võti, mis on avaldatud dokumendis federation metaandmete. Metaandmete federation sisaldab avalik osa on rentnike jaoks kasutada Turbeloa allkirjastamiseks serdid. Serdi töötlemata baiti kuvatakse soovitud `KeyDescriptor` element. Allkirjasert luba on ainult siis, kui allkirjastamiseks sobiv väärtus on `use` atribuut on `signing`.

Avaldatud Azure AD federation metaandmete dokumendi võib olla mitu allkirjastamiseks klahvi, näiteks kui Azure AD valmistab Allkirjaserdi värskendamiseks. Kui Domeeniliit metaandmete dokument sisaldab rohkem kui üks sert, toetama teenus, mis on kinnitamise märkide dokumendis kõik serdid.

Kuvatakse järgmised metaandmete valimi `KeyDescriptor` elemendi allkirjastamiseks võti.

```
<KeyDescriptor use="signing">
<KeyInfo xmlns="http://www.w3.org/2000/09/xmldsig#">
<X509Data>
<X509Certificate>
MIIDPjCCAiqgAwIBAgIQVWmXY/+9RqFA/OG9kFulHDAJBgUrDgMCHQUAMC0xKzApBgNVBAMTImFjY291bnRzLmFjY2Vzc2NvbnRyb2wud2luZG93cy5uZXQwHhcNMTIwNjA3MDcwMDAwWhcNMTQwNjA3MDcwMDAwWjAtMSswKQYDVQQDEyJhY2NvdW50cy5hY2Nlc3Njb250cm9sLndpbmRvd3MubmV0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEArCz8Sn3GGXmikH2MdTeGY1D711EORX/lVXpr+ecGgqfUWF8MPB07XkYuJ54DAuYT318+2XrzMjOtqkT94VkXmxv6dFGhG8YZ8vNMPd4tdj9c0lpvWQdqXtL1TlFRpD/P6UMEigfN0c9oWDg9U7Ilymgei0UXtf1gtcQbc5sSQU0S4vr9YJp2gLFIGK11Iqg4XSGdcI0QWLLkkC6cBukhVnd6BCYbLjTYy3fNs4DzNdemJlxGl8sLexFytBF6YApvSdus3nFXaMCtBGx16HzkK9ne3lobAwL2o79bP4imEGqg+ibvyNmbrwFGnQrBc1jTF9LyQX9q+louxVfHs6ZiVwIDAQABo2IwYDBeBgNVHQEEVzBVgBCxDDsLd8xkfOLKm4Q/SzjtoS8wLTErMCkGA1UEAxMiYWNjb3VudHMuYWNjZXNzY29udHJvbC53aW5kb3dzLm5ldIIQVWmXY/+9RqFA/OG9kFulHDAJBgUrDgMCHQUAA4IBAQAkJtxxm/ErgySlNk69+1odTMP8Oy6L0H17z7XGG3w4TqvTUSWaxD4hSFJ0e7mHLQLQD7oV/erACXwSZn2pMoZ89MBDjOMQA+e6QzGB7jmSzPTNmQgMLA8fWCfqPrz6zgH+1F1gNp8hJY57kfeVPBiyjuBmlTEBsBlzolY9dd/55qqfQk6cgSeCbHCy/RU/iep0+UsRMlSgPNNmqhj5gmN2AFVCN96zF694LwuPae5CeR2ZcVknexOWHYjFM0MgUSw0ubnGl0h9AJgGyhvNGcjQqu9vd1xkupFgaN+f7P3p3EVN5csBg5H94jEcQZT7EKeTiZ6bTrpDAnrr8tDCy8ng
</X509Certificate>
</X509Data>
</KeyInfo>
</KeyDescriptor>
  ```

Funktsiooni `KeyDescriptor` element kuvatakse kahes kohas federation metaandmete dokumendis; SAML kohased jaotises ja jaotises oli Federation-kohased. Nii teemasid serdid on samad.

Jaotises oli Federation-kohased oli-Federation metaandmete lugeja lugeda kaudu serdid on `RoleDescriptor` element on `SecurityTokenServiceType` tüüp.

Kuvatakse järgmised metaandmete valimi `RoleDescriptor` element.

```
<RoleDescriptor xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:fed="http://docs.oasis-open.org/wsfed/federation/200706" xsi:type="fed:SecurityTokenServiceType"protocolSupportEnumeration="http://docs.oasis-open.org/wsfed/federation/200706">
```

Jaotises SAML kohased oli-Federation metaandmete lugeja lugeda kaudu serdid on `IDPSSODescriptor` element.

Kuvatakse järgmised metaandmete valimi `IDPSSODescriptor` element.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
```
Puuduvad erinevused rentniku kohased ja rentniku sõltumatul serdid vormingus.

### <a name="ws-federation-endpoint-url"></a>URL-i oli-Federation lõpp-punkti

Metaandmete federation sisaldab URL-i, mis on Azure AD kasutusalad ühe sisselogimine ja ühe väljunud oli-Federation protokolli. Kuvatakse selle lõpp-punkti on `PassiveRequestorEndpoint` element.

Kuvatakse järgmised metaandmete valimi `PassiveRequestorEndpoint` element on rentniku kohased lõpp-punkti.

```
<fed:PassiveRequestorEndpoint>
<EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
<Address>
https://login.microsoftonline.com/72f988bf-86f1-41af-91ab-2d7cd011db45/wsfed
</Address>
</EndpointReference>
</fed:PassiveRequestorEndpoint>
```
Rentniku sõltumatul lõpp-punkti oli-Federation URL-i kuvatakse oli-Federation lõpp-punkti, nagu on näidatud järgmises näites.

```
<fed:PassiveRequestorEndpoint>
<EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
<Address>
https://login.microsoftonline.com/common/wsfed
</Address>
</EndpointReference>
</fed:PassiveRequestorEndpoint>
```

### <a name="saml-protocol-endpoint-url"></a>SAML protokolli lõpp-punkti URL-i

Metaandmete federation sisaldab URL, mis kasutab Azure AD ühekordse sisselogimise ja ühe väljunud SAML 2.0 protokolli. Nende lõpp-punktid kuvatakse soovitud `IDPSSODescriptor` element.

Sisselogimine ja väljunud URL-id, kuvatakse selle `SingleSignOnService` ja `SingleLogoutService` elemente.

Kuvatakse järgmised metaandmete valimi `PassiveResistorEndpoint` rentniku kohased lõpp.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
…
    <SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/contoso.onmicrosoft.com/saml2" />
    <SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/contoso.onmicrosoft.com /saml2" />
  </IDPSSODescriptor>
```

Sarnaselt lõpp-punktide levinud SAML 2.0 protokolli lõpp-punktid on avaldatud rentniku sõltumatul federation metaandmeid, nagu on näidatud järgmises näites.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
…
    <SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/common/saml2" />
    <SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/common/saml2" />
  </IDPSSODescriptor>
```
