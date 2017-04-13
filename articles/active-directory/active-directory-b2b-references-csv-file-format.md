<properties
   pageTitle="Azure Active Directory B2B koostöö eelvaade CSV-faili vorming | Microsoft Azure'i"
   description="Azure Active Directory B2B toetab oma rist – ettevõtte seoseid, võimaldades äripartneritega valikuliselt juurdepääsu oma ettevõtte rakendused"
   services="active-directory"
   documentationCenter=""
   authors="viv-liu"
   manager="cliffdi"
   editor=""
   tags=""/>

<tags
   ms.service="active-directory"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="identity"
   ms.date="05/09/2016"
   ms.author="viviali"/>

# <a name="azure-ad-b2b-collaboration-preview-csv-file-format"></a>Azure'i AD B2B koostöö eelvaade: CSV-faili vorming

Eelvaateversiooni Azure AD B2B koostöö jaoks on vaja määrata partneri kasutajateabe Azure AD portaali kaudu üles laadida CSV-faili. CSV-fail peaks sisaldama nõutav siltide allpool ja valikuliste väljade vastavalt vajadusele. Siltide esimeses reas õigekirja muutmata muuta CSV-vormingus näidisfaili (vt allpool).

>[AZURE.NOTE] Siltide (nt e-posti, DisplayName jne) esimesel real on vaja edukalt sõeluda CSV-faili. Õigekirja peab vastama allpool määratud väljad. Nende siltide tuvastada all neid sisu. Valikuline väljad, mis ei vaja, saate oma siltide eemaldada CSV-faili. Terve veeru saab tühjaks.

## <a name="required-fields-br"></a>Nõutavad väljad: <br/>
**E-posti:** Kutsutud kasutaja meiliaadressi. <br/>
**DisplayName:** Kuvatav nimi kutsutud kasutaja (tavaliselt ees- ja perekonnanime).<br/>


## <a name="optional-fields-br"></a>Valikuline väljad: <br/>

**InvitationText:** Kohandada kutse e-posti tekst pärast rakenduse sümboolika ja enne tagastushind link.<br/>
**InvitedToApplications:** AppIDs ettevõtte rakendustele kasutajatele määrata. AppIDs on tagastatava PowerShellis helistamise teel`Get-MsolServicePrincipal | fl DisplayName, AppPrincipalId`<br/>
**InvitedToGroups:** ObjectIDs rühmade kasutaja lisamiseks. ObjectIDs on tagastatava PowerShellis helistamise teel`Get-MsolGroup | fl DisplayName, ObjectId`<br/>
**InviteRedirectURL:** Pärast kutse aktsepteerimist kutsutud rakendust suunamiseks URL-i. See peaks olema ettevõttelt URL (nt [*contoso.my.salesforce.com*](http://contoso.my.salesforce.com/)). Valikuline väli pole määratud, suunatakse kutsutud kasutaja rakenduse Access paani kus nad saate liikuda teie valitud ettevõtte rakendused. Rakenduse Access juhtpaneeli URL on kujul `https://account.activedirectory.windowsazure.com/applications/default.aspx?tenantId=<TenantID>`.<br/>
**CcEmailAddress**: e-posti aadress kopeerimiseks saadetud kutse. Kui kasutatakse CcEmailAddress välja, seda ei saa kasutada e-posti kontrollida kasutaja või rentniku loomine.<br/>
**Keel:** Kutse e-posti ja tagastusväärtus kogemuse saamiseks "EN" (inglise keeles) kui määratlemata vaikimisi keel. Muud 10 toetatud keele koodid on:<br/>
1. de: Saksa<br/>
2. ES: Hispaania<br/>
3. FR: prantsuse keeles<br/>
4. See: Itaalia<br/>
5. ja: Jaapani keeles<br/>
6. Ko: Korea<br/>
7. PT-BR: Portugali (Brasiilia)<br/>
8. ru: vene<br/>
9. ZH-HANS: lihtsustatud Hiina<br/>
10. ZH-HANT: traditsiooniline Hiina<br/>

## <a name="sample-csv-file"></a>CSV-vormingus näidisfaili
Siin on näide CSV, saate muuta.

>[AZURE.NOTE] Kopeerige ja kleepige see Notepadi ja salvestada selle faililaiend "CSV". Redigeerige seda Excelis. Selle jagatud tabeli esimeses reas siltidega.

> Lisada rohkem valikuliste väljade Exceli määrates sildi ning ilma vaevata luua veeru selle all.

```
Email,DisplayName,InvitationText,InviteRedirectUrl,InvitedToApplications,InvitedToGroups,CcEmailAddress,Language
wharp@contoso.com,Walter Harp,Hi Walter! I hope you’ve been doing well.,,cd3ed3de-93ee-400b-8b19-b61ef44a0f29,,you@yourcompany.com,en
jsmith@contoso.com,Jeff Smith,Hi Jeff! I hope you’ve been doing well.,,cd3ed3de-93ee-400b-8b19-b61ef44a0f29,,you@yourcompany.com,en
bsmith@contoso.com,Ben Smith,Hi Ben! I hope you’ve been doing well.,,cd3ed3de-93ee-400b-8b19-b61ef44a0f29,,you@yourcompany.com,en

```

## <a name="related-articles"></a>Seotud artiklid
Sirvige meie teised artiklid Azure AD B2B koostöö

- [Mis on Azure AD B2B koostöö?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Kuidas see toimib](active-directory-b2b-how-it-works.md)
- [Üksikasjaliku selgituse](active-directory-b2b-detailed-walkthrough.md)
- [Väliste kasutajate Turbeloa vorming](active-directory-b2b-references-external-user-token-format.md)
- [Väliste kasutajate objekti atribuutide muutmine](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [Praegune eelvaade piirangud](active-directory-b2b-current-preview-limitations.md)
- [Artikli registri Rakendusehaldus Azure Active Directory 's](active-directory-apps-index.md)
