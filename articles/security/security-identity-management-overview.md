<properties
   pageTitle="Azure'i identiteedi haldamine turvalisuse ülevaade | Microsoft Azure'i"
   description=" Microsofti identiteedi ja juurdepääsu juhtimine lahenduste aidata seda kaitse Accessi rakenduste ja ressursside kogu ettevõtte andmekeskuse ja pilves, täiendavat taseme valideerimine, nt mitmikautentimise ja tingimusvormingu juurdepääsupoliitikaid lubamine. Selles artiklis antakse ülevaade tuum Azure turbefunktsioonid abi identiteetide haldus. "
   services="security"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/09/2016"
   ms.author="terrylan"/>

# <a name="azure-identity-management-security-overview"></a>Azure'i identiteedi haldamine turvalisuse ülevaade

Microsofti identiteedi ja juurdepääsu juhtimine lahenduste aidata seda kaitse Accessi rakenduste ja ressursside kogu ettevõtte andmekeskuse ja pilves, täiendavat taseme valideerimine, nt mitmikautentimise ja tingimusvormingu juurdepääsupoliitikaid lubamine. Jälgimine kahtlaste tegevuste kaudu täpsemate turbesätete aruandlus, auditeerimine ja teavitamine aitab leevendada väärtpaberi võimalikud probleemid. [Azure Active Directory Premium](../active-directory/active-directory-editions.md) pakub ühekordse sisselogimise tuhandetele pilveteenuse (SaaS) rakenduste ja juurdepääsu veebirakenduste kohapealse käivitamist.

Võimalus on järgmised eelised ja Azure Active Directory (AD).

- Saate luua ja hallata ühte identiteeti iga kasutaja jaoks kogu ettevõtte hübriid, hoides kasutajate, rühmade ja seadmed sünkroonis
- Ühekordse sisselogimise juurdepääsu tuhandeliste SaaS eelnevalt integreeritud rakendused, sealhulgas rakenduste
- Lubamine rakenduse access turvalisuse reeglid-põhise Mitmikautentimise jaoks asutusesiseselt rakendades ja Pilveteenusest rakenduste
- Sätte remote turvaline juurdepääs kohapealse veebirakenduste Azure AD Rakenduse puhverserveri kaudu

Käesoleva artikli eesmärk on ülevaate tuum Azure turbefunktsioonid abi identiteetide haldus. Pakume ka linke artiklitele, mis annab iga funktsiooni andmeid, nii et saate lisateavet.  

See artikkel keskendub järgmisi core Azure identiteedi haldamise võimalusi:

- Ühekordne sisselogimine
- Puhverserveri tühistada.
- Mitmikautentimise
- Turvalisus jälgimine, teatiste ja seadme õppe-põhiste aruanded
- Tarbija identiteedi ja juurdepääsu juhtimine
- Seadme registreerimine
- Õigustega identiteetide haldus
- Identiteedi kaitse
- Hübriidjuurutuse identiteetide haldus

## <a name="single-sign-on"></a>Ühekordne sisselogimine

Ühekordse sisselogimise (SSO) tähendab seda, et pääsevad juurde kõik rakendused ja ressursse, mida peate tegema business, logides sisse ainult üks kord, kasutades ühe kasutajakonto. Kui olete sisse loginud, pääsete kõik rakendused, peate ilma autentida (nt tippige parool) teist korda.

Paljud organisatsioonid usaldada tarkvara service (SaaS) rakendusi nagu Office 365, karp- ja Salesforce'i lõppkasutaja kontoritöö. Varem personali vajalik eraldi loomiseks ja värskendada iga SaaS rakenduse Kasutajakontod ja kasutajate oli iga SaaS rakenduse parooli meeles pidada.

Azure AD ulatub kohapealse Active Directory pilves, mis võimaldab kasutajatel mitte üksnes logige sisse oma domeeni ühendatud seadmete ja ettevõtte ressursid esmane ettevõtte konto abil, kuid samuti kõik web ja SaaS rakenduste vaja oma tööd.

Mitte ainult kasutajad ei pea haldamine kasutajanimed ja paroolid mitmele kriteeriumikogumile, rakenduse access võib olla automaatselt ettevalmistatud või tühistage ettevalmistatud põhjal ettevõtte rühmad ja tema oleku töötajana. Azure AD tutvustab turbe ja juurdepääsu juhtimine juhtelemendid, mis võimaldavad teil hallata kasutajate juurdepääsu üle SaaS rakenduste.

Lisateave:

- [Ühekordse sisselogimise ülevaade](https://azure.microsoft.com/documentation/videos/overview-of-single-sign-on/)
- [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](../active-directory/active-directory-appssoaccess-whatis.md)
- [Integreerimine Azure Active Directory ühekordse sisselogimise SaaS rakendused](../active-directory/active-directory-sso-integrate-saas-apps.md)

## <a name="reverse-proxy"></a>Puhverserveri tühistada.

Azure'i AD rakenduse puhverserver saate avaldada kohapealse rakendused, näiteks [SharePointi](https://support.office.com/article/What-is-SharePoint-97b915e6-651b-43b2-827d-fb25777f446f?ui=en-US&rs=en-US&ad=US) saitide, [Outlook Web Appi](https://technet.microsoft.com/library/jj657718.aspx)ja [IIS-i](http://www.iis.net/)-põhise rakendused teie privaatvõrgu sees ja pakub turvaline juurdepääs väljaspool võrgu kasutajad. Rakenduse puhverserveri pakub mitmesuguseid asutusesisese veebirakenduste SaaS rakendusi, mis toetab Azure AD tuhandete Kaugpöördusteenuse ja ühekordse sisselogimise (SSO). Töötajate sisselogimiseks oma rakenduste kaudu kodu oma seadmes ja selle pilvepõhist puhverserveri kaudu autentida.

Lisateave:

- [Azure AD Rakenduse puhverserveri lubamine](../active-directory/active-directory-application-proxy-enable.md)
- [Azure'i AD Rakenduse puhverserveri rakenduste avaldamine](../active-directory/active-directory-application-proxy-publish.md)
- [Ühekordse sisselogimise ja rakenduse puhverserver](../active-directory/active-directory-application-proxy-sso-using-kcd.md)
- [Tingimusvormingu Accessiga töötamine](../active-directory/active-directory-application-proxy-conditional-access.md)

## <a name="multi-factor-authentication"></a>Mitmikautentimise
Azure'i mitmikautentimise (MFA) on ehtsuse, mis on vaja kasutada rohkem kui üks kinnitusmeetod ja lisab kriitilised teine kiht turvalisus kasutaja sisselogimist ja tehingud. MFA aitab kaitsta juurdepääs andmetele ja rakenduste kasutaja nõudmisel lihtsa sisselogimise protsessi koosoleku ajal. See pakub tugeva autentimise kaudu mitmesuguseid võimalusi kinnitamine – telefonikõne, tekstsõnumi või mobiilirakenduses teatist või kinnitamine koodi ja 3. tootja OAuthi sõned.

Lisateave:

- [Mitmikautentimise](https://azure.microsoft.com/documentation/services/multi-factor-authentication/)
- [Mis on Azure Mitmikautentimise?](../multi-factor-authentication/multi-factor-authentication.md)
- [Azure'i Mitmikautentimise tööpõhimõte](../multi-factor-authentication/multi-factor-authentication-how-it-works.md)

## <a name="security-monitoring-alerts-and-machine-learning-based-reports"></a>Turvalisus jälgimine, teatiste ja seadme õppe-põhise aruanded

Turvalisus jälgimine ja teatiste ja seadme õppe-põhiste aruandeid, mis vastuolus Accessi mustrite tuvastamiseks aitab teil kaitsta oma äri. Saate Azure Active Directory access ja kasutusaruannete saada nähtavus terviklus ja ettevõtte kataloogi turvalisus. Selle teabe directory administraator saab paremini määratleda, kus võimalikest võimalik, et nad saavad piisavalt leping neid riske leevendada.

Azure'i klassikaline portaalis aruannete on liigitatud järgmiselt:

- Normaalne aruannetes – sündmused, mis leidsime olema Anomaalne sisselogimine. Meie eesmärk on teha teile teada selliste tegevuste ja võimaldab teil teha kindlaks, kas sündmuse on kahtlane.
- Integreeritud rakenduste – aruannete teadmisi, kuidas teie asutuses kasutatakse pilv rakendusi. Azure Active Directory pakub integreerimine tuhandete pilv rakendusi.
- Tõrketeated – tõrkeid, mis võivad ilmneda ettevalmistamise välistes rakendustes kontod.
- Kasutaja aruandeid – kuvada andmete mõne kindla kasutaja seade/märki.
- Logid – sisaldavad viimase 24 tunni jooksul, viimase 7 päeva jooksul või viimase 30 päeva jooksul, kui ka rühma tegevuse muutmine ja parooli lähtestamine ja registreerimise tegevuse kirje kõik auditeeritud sündmused.

Lisateave:

- [Saate vaadata oma aruandeid juurdepääs ja kasutamine](../active-directory/active-directory-view-access-usage-reports.md)
- [Alustamine: Azure Active Directory teatamine](../active-directory/active-directory-reporting-getting-started.md)
- [Azure Active Directory aruandluse juhend](../active-directory/active-directory-reporting-guide.md)

## <a name="consumer-identity-and-access-management"></a>Tarbija identiteedi ja juurdepääsu juhtimine

Azure Active Directory B2C on väga kättesaadav, üld-, identiteedi haldamine teenus tarbija suunatud rakenduste mis skaala sadu miljoneid identiteedid. See võib olla integreeritud üle mobile ja web platvormid. Tarbijatele saate logida kõik rakendused kaudu kohandatavad kogemusi social olemasolevaid abil või luua uue mandaadi.

Varem oleks rakenduste arendajatele, kes soovivad registreeruda kursustele ja logige sisse oma rakendustesse tarbijad kirjutanud oma kood. Ja need oleks kasutatud kohapealse andmebaasi või süsteemi talletada kasutajanimed ja paroolid. Azure Active Directory B2C pakub ettevõtte paremini integreerida rakenduste abil turvaline, standardid platvormi ja suure hulga laiendatav poliitikate tarbija identiteetide haldus.

Kui kasutate Azure Active Directory B2C, tarbijatele saavad registreeruda rakenduste abil oma olemasoleva suhtlusvõrgu kontod (Facebooki, Google, Amazon, LinkedIni) või luua uue mandaadi (meiliaadressi ja parooli või kasutajanime ja parooli).

Lisateave:

- [Mis on Azure Active Directory B2C?](https://azure.microsoft.com/services/active-directory-b2c/)
- [Azure Active Directory B2C eelvaade: logige häälestamine ja logige sisse oma rakendused tarbijad](../active-directory-b2c/active-directory-b2c-overview.md)
- [Azure Active Directory B2C eelvaade: Tüüpi rakendusi](../active-directory-b2c/active-directory-b2c-apps.md)

## <a name="device-registration"></a>Seadme registreerimine

Azure'i AD seadme registreerimine on device põhineva [juurdepääsu](../active-directory/active-directory-conditional-access-on-premises-setup.md) stsenaariumid. Kui seade on registreeritud, pakub Azure Active Directory seadme registreerimine identiteedi, mida kasutatakse autentimiseks seade, kui kasutaja seade. Autenditud seade ja atribuutide seade, siis saab rakendusi, mis on majutatud asutusesisese ja pilvepõhise juurdepääsu poliitikaid rakendada.

Kui koos mobiilsideseadme halduse (MDM) lahenduse, nt Intune, värskendatakse seadme atribuutide Azure Active Directory seadme kohta lisateabe. See võimaldab teil luua tingimusvormingu juurdepääsureeglid, mis kehtestavad Accessi seadmest, Turve ja nõuetele vastavus teie nõuetele.

Lisateave:

- [Alustamine Azure Active Directory seadme registreerimine](../active-directory/active-directory-conditional-access-device-registration-overview.md)
- [Kuidas häälestada kohapealse juurdepääsu abil Azure Active Directory seadme registreerimine](../active-directory/active-directory-conditional-access-on-premises-setup.md)
- [Automaatse seadme registreerimine Azure Active Directory Windows domeeni ühendatud seadmete](../active-directory/active-directory-conditional-access-automatic-device-registration.md)

## <a name="privileged-identity-management"></a>Õigustega identiteetide haldus
Azure Active Directory (AD) õigustega identiteetide haldus võimaldab teil hallata, määrata, ja jälgida oma õigustega identiteedid ja ressursside Azure AD kui ka muude Microsofti veebiteenuste nagu Office 365 või Microsoft Intune'i juurdepääsu.

Mõnikord kasutajad peavad õigustega toiminguid Azure'i või Office 365 ressursside või muud SaaS rakendused. See tähendab sageli organisatsioonidel on need püsivate õigustega juurdepääsu andmiseks Azure AD. See on pilve majutatud ressursid kasvab ohtu turvalisusele, sest ettevõtted ei saa piisavalt jälgida, mida kasutajad teevad tema administraatoriõigused. Lisaks, kui kasutajakonto õigustega juurdepääsu on rikutud, ühe rikkumise võivad mõjutada nende üldine cloud turvalisus. Azure'i AD õigustega identiteetide haldus aitab lahendada see risk.

Azure'i AD õigustega identiteetide haldus võimaldab.

- Vaadata, millised kasutajad on Azure AD administraatorid
- Nõudmisel, "samal ajal" Office 365 ja Intune, näiteks Microsoft Online Services administraatori juurdepääsu lubamine
- Administraatori ülesannete administraatori juurdepääs ajalugu ja muudatuste kohta aruannete hankimine
- Saada teatisi juurdepääsu õigustega rolli kohta

Lisateave:

- [Azure'i AD õigustega identiteetide haldus](../active-directory/active-directory-privileged-identity-management-configure.md)
- [Rollide Azure AD au identiteetide haldus](../active-directory/active-directory-privileged-identity-management-roles.md)
- [Azure'i AD õigustega identiteetide haldus: Kuidas lisada või eemaldada Kasutaja rolli](../active-directory/active-directory-privileged-identity-management-how-to-add-role-to-user.md)

## <a name="identity-protection"></a>Identiteedi kaitse
Azure'i AD identiteedi kaitse on turvalisus teenus, mis pakub risk sündmused ja võimalike nõrkade kohtade mõjutamata ettevõtte identiteedid konsolideeritud vaadet. Identiteedi kaitse mõjutab olemasoleva Azure Active Directory normaalne tuvastamise võimalused (saadaval Azure AD Anomaalne tegevuse aruannete kaudu) ja tutvustatakse uue risk sündmuste tüübid, mis tuvastab kõrvalekaldeid reaalajas.

Lisateave:

- [Azure Active Directory identiteedi kaitse](../active-directory/active-directory-identityprotection.md)
- [Kanali 9: Azure'i AD ja identiteedi Kuva: identiteedi kaitse eelvaade](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

## <a name="hybrid-identity-management"></a>Hübriidjuurutuse identiteetide haldus

Microsofti lähenemine identiteedi hõlmavad kohapealse ja pilveteenuse, ühe kasutaja identiteedi jaoks autentimise ja luba kõigile ressurssidele, olenemata asukohast loomine.

Lisateave:

- [Hübriid identiteedi lühiülevaade](http://download.microsoft.com/download/D/B/A/DBA9E313-B833-48EE-998A-240AA799A8AB/Hybrid_Identity_White_Paper.pdf)
- [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/)
- [Active Directory meeskonna ajaveeb](https://blogs.technet.microsoft.com/ad/)
