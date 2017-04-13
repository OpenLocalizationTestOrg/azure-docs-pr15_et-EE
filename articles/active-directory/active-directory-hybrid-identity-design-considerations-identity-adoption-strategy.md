<properties
    pageTitle="Azure Active Directory hübriid identiteedi kujundamine - määratlemine hübriid identiteedi kasutuselevõtustrateegia arendamine | Microsoft Azure'i"
    description="Azure Active Directory kontrollib juurdepääsu kontrolli, saate valida, kui kasutaja autentimist ja enne Accessi rakendusse eritingimused. Kui nendest tingimustest on täidetud, autenditud kasutaja ja rakenduse juurdepääs lubatud."
    documentationCenter=""
    services="active-directory"
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
    ms.date="08/08/2016"
    ms.author="billmath"/>


# <a name="define-a-hybrid-identity-adoption-strategy"></a>Hübriid identiteedi kasutuselevõtu strateegia määratlemine

Selles ülesandes määratlete hübriid identiteedi kasutuselevõtustrateegia arendamine teie hübriid identiteedi lahendus on äri nõuetele, mis võeti:

- [Ärivajaduste määratlemine](active-directory-hybrid-identity-design-considerations-business-needs.md)
- [Directory sünkroonimise nõuete määratlemine](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)
- [Mitmikautentimise nõuete määratlemine](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)

## <a name="define-business-needs-strategy"></a>Ettevõtte vajadustele strateegia määratlemine
Esimese tööülesande aadressid määratlemine ettevõtted ettevõtte vajab.  See võib olla väga lai ja ulatus libiseda võib ilmneda juhul, kui te ei ole ettevaatlik.  Alguses Hoidke sõnum lihtne, kuid pidage meeles, kujundus, et majutada ja hõlbustada muutmine tulevikus kavandada.  Sõltumata sellest, kas see on lihtne kujundus või äärmiselt keerukas sortimisloendeid, Azure Active Directory on Microsofti Identity platvorm, mis toetab Office 365, teenuse Microsoft Online Services ja pilveteenuse rakendusi.

## <a name="define-an-integration-strategy"></a>Integreerimise strateegia määratlemine
Microsoft on kolm peamist integreerimine stsenaariumid, mis on pilv identiteedid, sünkroonitud identiteedid ja ühendatud identiteedid.  Plaanite peaks vastuvõtmise ühte need integreerimise strateegiad.  Saate valida, võib olla muutuv ja otsuseid, valides ühe võivad sisaldada, millist tüüpi soovite sisestada, kas teil on mõne olemasoleva infrastruktuuri juba kohapealne ja mis on kõige kulu tõhusam kasutuskogemuse strateegia.  
 
![](./media/hybrid-id-design-considerations/integration-scenarios.png)

Stsenaariumid, mis on määratletud ülaltoodud joonisel on:

- **Pilveteenuse identiteedid**: need identiteedid, mis on olemas üksnes pilveteenuses.  Azure AD, puhul soovite elavad spetsiaalselt Azure AD kataloogi.
- **Sünkroonitud**: need identiteedid, mis on olemas asutusesisese ja pilve.  Azure'i AD-ühenduse kasutamisel need kasutajad on loodud või olemasoleva Azure AD kontode liitunud.  Kasutaja parooli räsi on sünkroonitud kohapealse keskkonna nn parooli räsi pilveteenusesse.  Kui abil sünkroonitud on üks hoiatus, et kui kasutaja on keelatud asutusesiseses keskkonnas, võib kuluda kuni 3 tundi selle konto oleku kuvamiseks Azure AD.  Selle põhjuseks sünkroonimise ajavahemik.
- **Mikroneesia**: need identiteedid olemas asutusesiseselt ja pilves.  Azure'i AD-ühenduse kasutamisel need kasutajad on loodud või olemasoleva Azure AD kontode liitunud.  
 
>[AZURE.NOTE]
Lisateavet sünkroonimise kohta lugege suvandid [integreerimine Azure Active Directory oma kohapealse identiteedid](active-directory-aadconnect.md).

Järgmine tabel aitab kindlaks teha, eelised ja puudused iga järgmised strateegiad:

| Strateegia         | Eelised                                                                                                                                                                                                                                                  | Puudused                                                                                                                                                                                                                                                                                                                                                  |
|------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Pilveteenuse identiteedid** | Hõlpsam hallata small ettevõtte jaoks. <br> Installimiseks klõpsake tööruumide ei täiendavat riistvara vaja midagi<br>Kui kasutaja lahkub ettevõtte hõlpsalt keelatud.                                                                                                   | Kasutajate peate sisse logima töökoormus pilveteenuses kasutamisel <br> Paroolide võib või ei pruugi olla sama pilveteenuste ja kohapealsete identiteetide jaoks                                                                                                                                                                                                                     |
| **Sünkroonitud**     | Kohapealse parool on asutusesiseselt autentimiseks ja cloud kataloogide <br>Lihtsam hallata väike, Keskmine või suur ettevõtete jaoks <br>Kasutajad saavad on ühekordse sisselogimise (SSO) mõned ressursid <br> Microsoft eelistatud meetod sünkroonimiseks <br> Haldamise hõlbustamiseks | Mõned kliendid ei soovi oma kataloogide sünkroonimiseks tähtaja konkreetse ettevõtte politsei pilveteenuses                                                                                                                                                                                                                                                  |
| **Ühendatud**        | Kasutajad saavad on ühekordse sisselogimise (SSO) <br>Kui kasutaja on lõpetatud või lehed, konto, saate kohe keelatud ja juurdepääsu tühistada,<br> Toetab täpsemaid stsenaariumid, mida ei saa, täita sünkroonitud                                           | Lisateavet juhiseid setup ja konfigureerimine <br> Suurema hooldustööd <br> Võib vaja täiendavat riistvara STS taristu <br> Täiendavate riistvara installida oleva liiduserveri võib olla nõutav. Täiendavat tarkvara on nõutav AD FS-i kasutamisel <br> Nõua SSO olulisel häälestamine <br> Kriitiline punkt tõrge, kui Domeeniliit serveris ei tööta, kasutajad ei saa autentida |

### <a name="client-experience"></a>Klientide programmikasutuskogemuse
Strateegia, mida kasutate ei näe kasutaja sisselogimine.  Järgmistes tabelites teile teavet selle kohta, mida kasutajad peaksid olema nende sisselogimine.  Ei toeta kõiki ühendatud identiteedipakkujad SSO kõigi stsenaariumide.

**Doman liitunud ja privaatne võrgu rakenduste**:
 

|                              | Sünkroonitud identiteet      | Ühendatud identiteet                                           |
|------------------------------|----------------------------|--------------------------------------------------------------|
| Veebibrauserid                 | Vormipõhine autentimine | ühekordse sisselogimise, mõnikord esitama organisatsiooni ID-d |
| Outlooki                      | Küsi mandaate     | Küsi mandaate                                       |
| Skype'i ärirakenduse (Lync)    | Küsi mandaate     | ühekordse sisselogimise kohta Lync, Exchange identimisteabe küsitakse   |
| SkyDrive Pro                 | Küsi mandaate     | ühekordse sisselogimise                                               |
| Office'i tellimuse ProPlus | Küsi mandaate     | ühekordse sisselogimise                                               |

**Välise või mitteusaldusväärsel allikad**:

|                              | Sünkroonitud identiteet      | Ühendatud identiteet                                           |
|------------------------------|----------------------------|--------------------------------------------------------------|
| Veebibrauserid                 | Vormipõhine autentimine |  Vormipõhine autentimine |
| Skype'i ärirakenduse (Lync), Skydrive Pro, Office'i tellimus Outlookis| Küsi mandaate     | Küsi mandaate                                       |
| Exchange ActiveSynci    | Küsi mandaate     | ühekordse sisselogimise kohta Lync, Exchange identimisteabe küsitakse   |
| Mobiilirakenduste                 | Küsi mandaate     | Küsi mandaate                                               |

Kui olete otsustanud tööülesandest 1, et teil on 3 osapoole IdP või ei kavatse kasutavad ühte federation Azure AD., peate olema teadlik järgmisi toetatud funktsioone:
- Mis tahes SAML 2.0 teenusepakkuja, mis vastab SP-Lite profiili toetab Azure AD autentimise ja seostuvad rakendused
- Toetab passiivset autentimist, mis hõlbustab auth OWA, SPO jne.
- Exchange Online'i kliendid saavad toetatud kaudu soovitud SAML 2.0 täiustatud kliendi profiili (ECP)

Teil peab olema teadlik millised funktsioonid ei ole saadaval:

- Toetuseta oli-usalda/Federation, rikub kõik aktiivsed kliendid
 - See tähendab, et pole Lynci klient, OneDrive kliendi Office'i tellimus, Office Mobile enne Office 2016
- Üleminek Office'i passiivset autentimist võimaldab neid toetada puhas SAML 2.0 sisepagulaste reintegreerimine, kuid tugiteenuste ikkagi kliendi-kliendi alusel


>[AZURE.NOTE]
Viimati värskendatud loendi lugege artiklis http://aka.ms/ssoproviders.

## <a name="define-synchronization-strategy"></a>Sünkroonimise strateegia määratlemine
Selles ülesandes Määratlege tööriistu, mida kasutatakse sünkroonida ettevõtte kohapealses andmete pilve ja mida peaksite kasutama topoloogia.  Kuna enamikule asutustest Active Directory kasutamiseks klõpsake ülaltoodud küsimustele Azure'i AD-ühenduse kaudu teavet üksikasjalikumalt.  Keskkonnas, mis on Active Directory, on see strateegia abil FIM 2010 R2 või MIM 2016 kasutamise kohta leiate.  Kuid väljaannetes on Azure'i AD-ühenduse toetab LDAP kataloogid, nii olenevalt teie ajaskaala, see teave on võimalik, et aidata.

###<a name="synchronization-tools"></a>Sünkroonimise tööriistad
Aasta jooksul mitu sünkroonimise tööriistad on olemas ja kasutada erinevaid võimalusi.  Azure'i AD-ühenduse praegu avage tööriist valik kõigi toetatud stsenaariumid.  AAD Sync ja DirSync ümber ka ikka on nüüd olla isegi teie keskkonnas olemas. 

>[AZURE.NOTE]
Toetatud võimaluste viimase teavet iga tööriista, lugege artikli [kataloogi integreerimise võrdlus](active-directory-hybrid-identity-design-considerations-tools-comparison.md) .  

### <a name="supported-topologies"></a>Toetatud topoloogiatest
Sünkroonimise strateegia määratlemisel topoloogia, mida kasutatakse määratakse. Sõltuvalt teabe määratletud toiming 2 saate määrata, millised topoloogia on õige kasutada. Ühe mets, ühe Azure AD topoloogia on kõige levinum ja koosneb ühe Active Directory kogumis ja Azure AD ühekordsest.  Seda saab kasutada enamikul juhtudel ja on oodatud topoloogia Azure'i AD-ühenduse Express installi kasutamisel, nagu on näidatud alloleval joonisel.
 
![](./media/hybrid-id-design-considerations/single-forest.png)Ühe mets stsenaarium see on väga levinud suur- ja isegi organisatsioonidel on mitu kogumit, nagu on näidatud joonisel 5.

>[AZURE.NOTE]
Lisateavet erinevatest asutusesisestest ja Azure AD topoloogiatest koos Azure'i AD-ühenduse sünkroonimine lugege artiklit [topoloogiatest jaoks Azure'i AD-ühenduse](active-directory-aadconnect-topologies.md).


![](./media/hybrid-id-design-considerations/multi-forest.png) 

Mitme mets stsenaarium

Kui sellega juhul seejärel mitme-forest-ühe Azure AD topoloogia tuleb arvestada kui täidetud on järgmised üksused:

- Kasutajatel on ainult 1 identiteedi üle kõik metsad – kordumatult isikupärase kasutajad allpool olevat jaotist kirjeldab seda üksikasjalikumalt.
- Kus asub tema identiteedi mets autendib kasutaja
- UPN-i ja ankur allikas (püsiv id) on pärit selle mets
- Kõik metsad on kättesaadav Azure'i AD-ühenduse – see tähendab, et see ei pea olema domeeni liitunud ja paigutada on DMZ kui see hõlbustab see.
- Kasutajatel on ainult üks postkast
- Kasutaja postkasti majutav mets on parim andmete kvaliteedi atribuudid, mis on nähtav, klõpsake selle Exchange'i globaalse aadressiloendi (GAL)
- Kui kasutaja on ühtegi postkasti, võib mõni mets, aidata need väärtused
- Kui teil on lingitud postkasti, siis on ka mõne muu kontoga sisselogimiseks kasutatud erinevate mets.

>[AZURE.NOTE]
Objektid, mis on olemas asutusesiseselt ja pilves on "" kaudu ühendatud kordumatut tunnust. Kataloogisünkroonimise kontekstis selle kordumatut tunnust nimetatakse selle SourceAnchor. Kontekstis, ühekordse sisselogimise, seda nimetatakse funktsiooni ImmutableId. Veel kaalutlused kasutamise sourceAnchor [kujundus kontseptsioonide Azure'i AD-ühenduse](active-directory-aadconnect-design-concepts.md#sourceanchor) .

Kui eeltoodud pole täidetud ja teil on rohkem kui ühe aktiivse konto või rohkem kui üks postkast, valige üks ja ignoreerida teise Azure'i AD-ühenduse.  Kui teil on seotud postkastid, kuid pole muu konto, nende kontodega ei ekspordita Azure AD ja kasutaja ei ole mis tahes rühma liige.  See on erinev kuidas see oli varem tööriistaga DirSync ja tahtlikult paremini toetatakse järgmisi olukordi mitme mets. Mitme mets stsenaarium on näidatud alloleval joonisel.
 
![](./media/hybrid-id-design-considerations/multiforest-multipleAzureAD.png) 
 
**Mitme forest mitme Azure AD stsenaarium**

On soovitatav on vaid ühe kataloog Azure AD ettevõtte jaoks, kuid seda toetatakse seda 1:1 seose hoitakse Azure'i AD-ühenduse sünkroonimine serveriga ja on Azure AD kataloogi vahel.  Iga eksemplari Azure AD, peate installi Azure'i AD-ühenduse.  Lisaks Azure AD, kujundus on eraldatud ja ühe eksemplari Azure AD kasutajad ei saa kasutajad mõnes muus eksemplaris kuvamiseks.

On võimalik ja toetatud ühenduse mitme Azure AD kataloogide üks kohapealse Active Directory eksemplar, nagu on näidatud alloleval joonisel:

![](./media/hybrid-id-design-considerations/single-forest-flitering.png) 
 

**Ühe – mets filtreerimise stsenaarium**

Selleks järgmist peab olema täidetud.

- Azure'i AD-ühendus sünkroonimine serverid peab olema konfigureeritud nii, et nad on üksteist välistavate objektide kogumi filtreerimiseks.  Seda teha, näiteks iga serveri kindla domeeni või OU ulatuse määramine.
- DNS-i domeeni saab registreerida ainult ühe Azure AD directory nii UPN-ID asutusesiseses kasutajate AD tuleb kasutada eraldi nimeruumid
- Ühe eksemplari Azure AD kasutajate saab ainult kasutajad saavad eksemplarist kuvamiseks.  Ta ei saa kasutajad muudel juhtudel kuvamiseks
- Ainult üks Azure AD kataloogid saate lubada koos asutusesisese Exchange'i hübriidjuurutuse AD
- Vastastikuste ainuõigused kehtib ka allahindluse.  Seetõttu ei toeta seda topoloogia kuna need endale ühe asutusesisese konfiguratsiooni allahindluse funktsioone.  See hõlmab:
 - Allahindluse rühmitamiseks vaikeväärtus konfiguratsioon
 - Seadme allahindluse


Arvestage, et järgmist ei toetata ja ei saa valida mõne rakendada:

- Ei toetata on mitme Azure'i AD-ühenduse ühenduse ka siis, kui need on konfigureeritud sünkroonima üksteist välistavate sama Azure AD kataloogi sünkroonimine serverid objekti seadmine
- See ei toeta sama kasutajal mitu Azure AD kataloogide sünkroonimiseks. 
- See on ka toetuseta konfiguratsiooni muuta ühe Azure AD kuvatav teise Azure AD kataloogi kontaktid kasutajad teha. 
- Samuti on toetamata Azure'i AD-ühenduse sünkroonimine mitme Azure AD kataloogide ühenduse muutmine.
- Azure'i AD kataloogide on eraldatud kujundus. See ei toetata andmeid lugeda mõne muu Azure AD kataloogist püüdes koostada levinud ja ühendatud GAL vahel kataloogid sünkroonimine Azure'i AD-ühenduse konfiguratsiooni muuta. See on ka toetuseta kasutajatele nimega kontaktide eksportimine teise kohapealse AD sünkroonimine Azure'i AD-ühenduse kaudu.


>[AZURE.NOTE]
Kui teie asutuses piirab arvutites võrgu kaudu Interneti-ühenduse loomiseks, selles artiklis on loetletud lõpp-punktid (FQDN-i IPv4 ja IPv6 aadresside vahemikud), et peaksite kaasama oma väljamineva luba loendite ja Internet Exploreri usaldusväärsete saitide tsooni kliendi arvutite tagada edukalt abil saate oma arvutisse Office 365. Lisateabe saamiseks lugege [Office 365 URL-id ja IP-aadresside vahemikud](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2?ui=en-US&rs=en-US&ad=US).

## <a name="define-multi-factor-authentication-strategy"></a>Mitmikautentimise strateegia määratlemine
Selles ülesandes Määratlege mitmikautentimise strateegia kasutada.  Azure'i Mitmikautentimise on kaks erinevat versiooni.  Üks pilvepõhist ja teine on kohapealse vastavalt Azure'i MFA serveri kasutamine.  Hindamise ei kohale, mida saate määratleda, milline lahendus on õige jaoks strateegia kavandamine.  Järgmises tabelis abil saate määratleda, milline kujundus valik parim täita teie ettevõtte turvalisuse nõue:

Mitme teguri kujundus suvandid:

| Varade tagamiseks                                               | MFA pilveteenuses | Kohapealne MFA |
|---------------------------------------------------------------|------------------|----------------|
| Microsofti rakendused                                                | Jah              | Jah            |
| SaaS rakendused rakendus Galerii                                  | Jah              | Jah            |
| IIS-i rakenduste avaldatud Azure AD Rakenduse puhverserveri kaudu         | Jah              | Jah            |
| IIS-i rakenduste pole avaldatud Azure AD Rakenduse puhverserveri kaudu | Ei               | Jah            |
| Kui VPN, lugemisel Kaugpöördusteenuse                                     | Ei               | Jah            |

Isegi juhul, kui teil võib olla arveldatud lahenduse jaoks strateegia kavandamine, peate endiselt väärtustamise ülevalt kasutamine kasutajate asukohaks.  See võib põhjustada lahenduse muutmiseks.  Järgmises tabelis abil saate kindlaks teha, see:

| Kasutaja asukoht                                                       | Suvandi eelistatud kujundus                 |
|---------------------------------------------------------------------|-----------------------------------------|
| Azure Active Directory                                              | Mitme-FactorAuthentication pilveteenuses |
| Azure AD ja kohapealsete AD federation AD FS-i abil             | Mõlemad                                    |
| Azure AD ja kohapealsete AD, kasutades Azure AD-ühenduse pole parooli sünkroonimise | Mõlemad                                    |
| Azure AD ja kohapealsete parooli sünkroonimise Azure'i AD-ühenduse kasutamine  | Mõlemad                                    |
| Kohapealse AD                                                      | Mitmikautentimise Server      |

>[AZURE.NOTE]
Te peate tagama, et valitud mitmikautentimise kujundus suvandist toetab funktsioone kujunduse jaoks vajalike.  Lisateabe saamiseks lugege [valimine mitme teguri turvalisus lahendus teie jaoks](../multi-factor-authentication-get-started.md#what-am-i-trying-to-secure).

## <a name="multi-factor-auth-provider"></a>Mitme teguri Auth pakkuja
Mitmikautentimise on vaikimisi globaalne administraatoritele, kes on Azure Active Directory rentniku jaoks saadaval. Siiski kui soovite hõlmavad mitmikautentimise kõik kasutajad ja/või soovite oma globaalse administraatorid saama kasutada funktsioone nagu haldusportaali, kohandatud Tervitused ja aruannete võtta, siis tuleb osta ja konfigureerida mitme teguri autentimisteenuse.

>[AZURE.NOTE]
Te peate tagama, et valitud mitmikautentimise kujundus suvandist toetab funktsioone kujunduse jaoks vajalike. 

##<a name="next-steps"></a>Järgmised sammud
[Andmekaitse nõudeid määratlemine](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md)

## <a name="see-also"></a>Vt ka
[Kujundus kaalutlused ülevaade](active-directory-hybrid-identity-design-considerations-overview.md)
