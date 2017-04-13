<properties 
    pageTitle="Ühekordse sisselogimise rakendustele, mis pole galeriis Azure Active Directory rakenduse konfigureerimise | Microsoft Azure'i" 
    description="Siit saate teada, kuidas ühendada iseteenindusliku Azure Active Directory abil SAML ja parooli põhinev SSO rakendused" 
    services="active-directory" 
    authors="asmalser-msft"  
    documentationCenter="na" manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="02/09/2016" 
    ms.author="asmalser" />

#<a name="configuring-single-sign-on-to-applications-that-are-not-in-the-azure-active-directory-application-gallery"></a>Ühekordse sisselogimise rakendustele, mis pole galeriis Azure Active Directory rakenduste konfigureerimine

See artikkel on funktsioon, mis lubab administraatorid konfigureerida ühekordse sisselogimise rakendustele ei kohal olevat Azure Active Directory rakenduse Galerii *koodi kirjutamata*. See funktsioon ilmus tehnilise eelvaate 18 November 2015 ja [Azure Active Directory Premium](active-directory-editions.md)on kaasatud. Kui otsite hoopis arendaja juhised selle kohta, kuidas kohandatud rakenduste integreerimine Azure AD koodi kaudu, lugege teemat [Autentimise stsenaariumid Azure AD](active-directory-authentication-scenarios.md).

Azure Active Directory rakenduse Galerii sisaldab rakendusi, mis toetab kujul ühekordse sisselogimise Azure Active Directory, nagu on kirjeldatud [selles artiklis](active-directory-appssoaccess-whatis.md). (Kui IT-spetsialist või süsteemi integraatori ettevõtte) leidnud soovitud rakendus, mida soovite ühendada, saate alustada, järgige üksikasjalikke juhiseid esitatud Azure'i haldusportaal ühekordse sisselogimise lubamiseks.

Kliendid, kellel on [Azure Active Directory Premium](active-directory-editions.md) litsentside kuvada ka nende lisavõimaluste.

* Mis tahes rakendus, mis toetab SAML 2.0 identiteedipakkujad (SP algatatud või IdP algatatud) integratsiooni iseteenindusliku
* Mis tahes veebirakenduse, mis sisaldab mõnda HTML-i-põhine sisselogimislehel [parooli põhinev](active-directory-appssoaccess-whatis.md#password-based-single-sign-on) SSO kasutamist iseteenindusliku integreerimine
* Iseteeninduslik ühenduse rakenduste kasutatavate SCIM protokoll kasutaja ettevalmistamise ([siin kirjeldatud](active-directory-scim-provisioning.md))
* Võimalus lisada mis tahes rakenduse [Office 365 rakendusekäiviti](https://blogs.office.com/2014/10/16/organize-office-365-new-app-launcher-2/) või [Azure AD kasutada paneeli](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users) lingid

See võib sisaldada ainult mitte SaaS rakendusi, mis kasutada, kuid ei ole veel sisse-peatatud Azure AD rakenduste galeriisse, kuid kolmanda osapoole veebirakendusi, et teie asutuses on juurutatud serverid saate juhtida, cloud või kohapealse.

Neid võimalusi, nimetatakse ka *Rakenduse integreerimise Mallid*, ette standardid alusel ühenduse punktide rakendusi, mis toetavad SAML, SCIM või vormipõhist autentimist ja lisada paindlik suvandite ja sätete paljudele rakenduste ühilduvust. 

##<a name="adding-an-unlisted-application"></a>Lisage kontaktiloendist rakendus

Rakenduse abil kuvatakse rakenduse integreerimise malli ühenduse Azure Active Directory administraator konto kaudu Azure'i haldusportaal logige sisse ja liikuge sirvides soovitud **Active Directory > [Directory] > Applications** jaotises nuppu **Lisa**ja seejärel käsku **Lisa rakendus galeriist**. 

![][1]

Rakenduse Galerii, saate lisada kontaktiloendist rakendus, kasutades **kohandatud** kategooria vasakul, või valige **kontaktiloendist rakenduse lisamine** link, mis kuvatakse otsingutulemite, kui teie soovitud rakendus ei leitud. Kui olete sisestanud oma rakenduse nimi, saate konfigureerida ühekordse sisselogimise suvandid ja käitumist. 

**Kiire Vihje**: hea tava, kasutada otsingu abil kontrollida, kui rakendus on juba olemas rakenduse Galerii. Kui rakendus on leitud ja selle kirjeldus mainimised "ilma ühekordse sisselogimise", siis rakenduse juba ühendatud ühekordse sisselogimise jaoks toetatud. 

![][2]

Lisage rakendus sellisel viisil tagab on üsna sarnased saadaval eelnevalt integreeritud rakenduste juurde. Alustamiseks valige **Konfigureerimine ühekordse sisselogimise**. Järgmisel kuval esitatakse järgmised kolm võimalust konfigureerida ühekordse sisselogimise kohta, mida on kirjeldatud järgmistes lõikudes.

![][3]


##<a name="azure-ad-single-sign-on"></a>Azure'i AD ühekordse sisselogimise

Valige see suvand rakenduse SAML-põhise autentimise konfigureerimine. Selleks, et rakenduse tugi SAML 2.0 ja teil peaks koguma teavet rakenduse enne jätkamist SAML võimaluste kasutamise kohta. Pärast valiku **järgmisele**, palutakse teil sisestada kolme eri URL-id vastava rakenduse SAML lõpp-punktid. 

![][4]
 
Need on:

* **Logi sisse URL-i (SP algatatud ainult)** – kui kasutaja läheb selle rakenduse sisse logima. Kui rakendus on konfigureeritud täita teenuse pakkuja algatatud ühekordse sisselogimise kohta, siis kui kasutaja viib seda URL-i, tehke teenusepakkuja vajalikud ümbersuunamise Azure AD autentimiseks ja kasutajale sisse logida. Kui väli on täidetud, siis Azure AD kasutab seda URL-i käivitage rakendus Office 365 ja Azure AD Accessi juhtpaneeli kaudu. Kui väli on ommited, siis selle asemel täita identiteedipakkuja Azure AD-algatatud Logi sisse, kui käivitatakse rakenduse Office 365, Azure AD Accessi paneeli või Azure AD ühe sisselogimise URL-i (copiable vahekaart armatuurlaud).

* **Väljaandja URL** - URL-i selgitama kordumatult mis ühekordse sisselogimise taotluse väljaandja konfigureeritakse. See on väärtus, mis Azure AD saadab tagasi parameetrina **sihtrühma** kohaldamine SAML luba ja peaks selle kinnitamiseks. **Üksuse ID** , mis tahes SAML metaandmete rakenduse kuvatakse ka selle väärtuse. Kontrollige rakenduse SAML dokumentatsiooni kohta, mis on üksuse ID või sihtrühma väärtus on. Allpool on näide sellest, kuidas sihtrühma URL-i kuvatakse SAML luba, tagastatakse rakenduse:

```
    <Subject>
    <NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:unspecificed">chad.smith@example.com</NameID>
        <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer" />
      </Subject>
      <Conditions NotBefore="2014-12-19T01:03:14.278Z" NotOnOrAfter="2014-12-19T02:03:14.278Z">
        <AudienceRestriction>
          <Audience>https://tenant.example.com</Audience>
        </AudienceRestriction>
      </Conditions>
```

* **Vasta URL** - vastus URL-i on kui rakenduse ootab SAML luba. See on ka edaspidi **kinnituse tarbija teenus (ACS) URL-i**. Otsige rakenduse SAML dokumentidest käsitleva SAML Turbeloa või oma vastuse URL-i ACS URL-i üksikasjad.
 Kui need on sisestanud, klõpsake nuppu **edasi** jätkamiseks järgmisel kuval. Kuva teave, mida on vaja konfigureerida rakendus küljel lubamiseks aktsepteerimiseks SAML luba: Azure'i AD. 

![][5]

Millised väärtused on nõutav erineda sõltuvalt rakendusest, seega kontrollige rakenduse SAML dokumentatsioonist. Funktsiooni **Sisselogimise** ja **väljunud** teenuse URL-i nii lahendamiseks sama lõpp-punkti, mis on teie eksemplari Azure AD SAML taotluse käsitlemine lõpp-punkti. Väljaandja URL on väärtus, mis kuvatakse kujul "Väljaandja" sees SAML luba rakendusse välja. 

Pärast seda, kui teie taotlus on konfigureeritud, klõpsake nuppu **edasi** ja seejärel dialoogiboksi sulgemiseks **lõpuleviimine** . 

##<a name="assigning-users-and-groups-to-your-saml-application"></a>SAML rakenduse kasutajate ja rühmade määramine 

Kui teie taotlus on konfigureeritud kasutamiseks Azure AD SAML-põhiste identiteedipakkuja, siis on peaaegu valmis, et testida. Turvalisus kontrollimise, ei anna Azure AD märgiks võimaldab rakenduse sisse logida, kui neile on antud juurdepääs Azure AD abil. Kasutajate võib anda juurdepääsu otse või rühma, et nad on liige. 

Rakenduse kasutaja või rühma määramiseks nuppu **Määra kasutajad** . Valige kasutaja või rühm, mida soovite määrata ja seejärel klõpsake nuppu **Määra** . 

![][6]

Kasutaja määramine lubab Azure AD väljaandmisel märgiks kasutaja, samuti põhjustab selle rakenduse kasutaja juurdepääs paanil kuvatakse paani. Mõne rakenduse paani kuvatakse ka Teenusekomplektis Office 365 rakenduse käiviti, kui kasutaja on teenusekomplekti Office 365. 

Saate üles laadida rakenduse **Logo laadige** nupu abil **konfigureerimine** menüü rakenduse paan logo. 

###<a name="customizing-the-claims-issued-in-the-saml-token"></a>SAML luba välja antud nõuete kohandamine 

Kui kasutaja autendib rakendusse, annab Azure AD SAML luba rakendus, mis sisaldab teavet (või taotluste) kasutaja, mis tuvastab kordumatult nende kohta. See sisaldab vaikimisi kasutaja kasutajanimi, meiliaadress, eesnimi ja perekonnanimi. 

Saate vaadata või redigeerida SAML luba **Atribuudid** vahekaardil rakendusse saadetud nõuded. 

![][7]

On mitu võimalikku põhjust, miks peaksite nõuete välja SAML luba redigeerimine: •pildi rakendus on kirjutatud nõua teistsuguseid taotluste URI-d või taotluste väärtusi nii, et selle NameIdentifier nõuab • teie rakenduse kasutusele võetud pidada millegi muuga kui number talletatud Azure Active Directory kasutajanimi (AKA kasutaja turvasubjektinimi). 

Lisateavet selle kohta, kuidas lisada ja redigeerida taotluste need stsenaariumid, vaadake käesoleva [artikli nõuete kohandamise kohta](active-directory-saml-claims-customization.md). 

###<a name="testing-the-saml-application"></a>SAML testimisel 

Kui SAML URL-id ja serdi pole konfigureeritud Azure AD ja rakenduse, kasutajatele või rühmadele on määratud Azure, rakenduse ja nõuete läbi ning vajaduse korral redigeerida, siis kasutaja on valmis logige sisse rakendusse. 

Testimiseks lihtsalt Logi-üheks Azure AD juurdepääs paneeli https://myapps.microsoft.com määratud rakenduse kasutaja kontot ja seejärel klõpsake paani, mis on ühekordse sisselogimise protsessi võrgukoosolekuga. Vaheldumisi, saate sirvida otse sisselogimise URL-i rakendus ja logige sisse seal. 

Silumine näpunäiteid, leiate selle [artikli kohta, kuidas silumine SAML-põhiste ühekordse sisselogimise rakendustele](active-directory-saml-debugging.md) 

##<a name="password-single-sign-on"></a>Parooli Ühekordne sisselogimine

Valige see suvand konfigureerida [parooli põhinev ühekordse sisselogimise](active-directory-appssoaccess-whatis.md) veebirakenduse, mis sisaldab HTML-sisselogimise leht. Parooli põhinev SSO, nimetatakse ka parooli Holvisto, võimaldab kasutajate juurdepääsu ja veebirakenduste, mis ei toeta identiteedi ühendamine paroolide haldamine. See on abiks stsenaariumid, kus mitu kasutajat tuleb jagada ühe konto, näiteks ettevõtte sotsiaalmeedia rakenduse kontod. 

Pärast valiku **järgmisele**, palutakse teil sisestada rakenduse veebipõhine sisselogimise lehe URL. Pange tähele, et see peab olema leht, mis sisaldab kasutajanime ja parooli väljad. Kui sisestanud, Azure AD hakkab protsessi sõeluda sisestusmeetodi kasutajanime ja parooliga, sisestage soovitud sisselogimislehel. Kui toiming ei õnnestu, siis see juhatab teid läbi mõne alternatiivse protsessi installimisel brauseri laiend (nõuab Internet Exploreri, Chrome'i või Firefoxi), mis võimaldab teil käsitsi jäädvustada väljad.

Kui sisselogimise leht on hõivatud, võib määratud kasutajad ja rühmad ja mandaati poliitikad, saate määrata samamoodi nagu tavalise [parooli SSO rakendused](active-directory-appssoaccess-whatis.md).

Märkus: Saate üles laadida rakenduse **Logo laadige** nupu abil **konfigureerimine** menüü rakenduse paan logo. 

##<a name="existing-single-sign-on"></a>Olemasoleva ühekordse sisselogimise

Valige see suvand lingi lisamiseks rakenduse ettevõtte Azure AD kasutada paneeli või Office 365 portaali. Saate seda linkide lisamiseks kohandatud veebirakenduste, mida praegu kasutada Azure Active Directory Federation Services (või muude federation teenus) asemel Azure AD autentimiseks. Või saate lisada sügav lingid teatud SharePointi lehtede või muude teie kasutaja juurdepääs paneelid kuvada ainult soovitud veebilehtede. 

Pärast valiku **järgmisele**, palutakse teil sisestada URL-i taotluse linkida. Täidetud kasutajad ja rühmad rakendusele põhjustab kuvatakse rakenduse [Office 365 rakendusekäiviti](https://blogs.office.com/2014/10/16/organize-office-365-new-app-launcher-2/) või [Azure AD kasutada paneeli](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users) nende kasutajate jaoks määratud.

Märkus: Saate üles laadida rakenduse **Logo laadige** nupu abil **konfigureerimine** menüü rakenduse paan logo.

## <a name="related-articles"></a>Seotud artiklid

- [Artikli Index Rakendusehaldus Azure Active Directory 's](active-directory-apps-index.md)
- [Kuidas kohandada taotluste välja SAML luba eelnevalt integreeritud rakendused](active-directory-saml-claims-customization.md)
- [SAML-põhiste ühekordse sisselogimise tõrkeotsing](active-directory-saml-debugging.md)

<!--Image references-->
[1]: ./media/active-directory-saas-custom-apps/customapp1.png
[2]: ./media/active-directory-saas-custom-apps/customapp2.png
[3]: ./media/active-directory-saas-custom-apps/customapp3.png
[4]: ./media/active-directory-saas-custom-apps/customapp4.png
[5]: ./media/active-directory-saas-custom-apps/customapp5.png
[6]: ./media/active-directory-saas-custom-apps/customapp6.png
[7]: ./media/active-directory-saas-custom-apps/customapp7.png
