<Properties
    pageTitle="Azure Active Directory juurdepääsu | Microsoft Azure'i"  
    description="Azure Active Directory tingimusvormingu juurdepääsu reguleerimine abil otsida eritingimuste, kui juurdepääsu taotluste autentimine."  
    services="active-directory"
    keywords="tingimusvormingu juurdepääsu rakendused, juurdepääsu Azure AD, ettevõtte ressursid tingimusvormingu juurdepääsupoliitikaid turvaline juurdepääs"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/21/2016"
    ms.author="markvi"/>


# <a name="conditional-access-in-azure-active-directory"></a>Juurdepääsu Azure Active Directory 's   

Juhtelemendi tingimusvormingu Accessis Azure Active Directory (Azure AD) võimaluste pakuvad turvalist ressursid kohapealse ja pilveteenuse lihtsa võimalusi. Tingimusvormingu juurdepääsupoliitikaid nagu mitmikautentimise aitab kaitsta riski varastatud ja välja petetud mandaat. Muude tingimusvormingu juurdepääsupoliitikaid aitab hoida oma ettevõtte andmete ohutu. Näiteks Lisaks nõudva mandaat, peate poliitika selle ainult seadmete õpib mobiilsideseadme halduse süsteemi samamoodi nagu Microsoft Intune'i pääsete juurde oma ettevõtte tundliku teenused.


## <a name="prerequisites"></a>Eeltingimused

Azure'i AD juurdepääsu funktsioon [Azure Active Directory Premium](http://www.microsoft.com/identity). Igale kasutajale, kes kasutab rakendust, mis on rakendatud tingimusvormingu juurdepääsupoliitikaid peab olema on Azure AD Premium litsents. Saate lisateavet litsentsinõuded [litsentsimata kasutajate](https://aka.ms/utc5ix)aruandes.


## <a name="how-is-conditional-access-control-enforced"></a>Kuidas jõustatakse tingimusvormingu juurdepääsu reguleerimine?  

Tingimusvormingu juurdepääsu reguleerimine kohas, kus Azure AD kontrollib jaoks eritingimused, mida kasutaja juurde pääseda rakenduse jaoks. Pärast Accessi nõuded on täidetud, kasutaja autenditakse ja pääsete juurde rakenduse.  

![Juurdepääsu ülevaade](./media/active-directory-conditional-access/conditionalaccess-overview.png)

## <a name="conditions"></a>Tingimused

Need, mis võite kaasata juurdepääsu poliitika:

- **Liikmestaatuse**. Kasutaja juurdepääsu rühma liikmelisusel põhinevad juhtida.

- **Asukoht**. Kasutage päästik mitmikautentimise kasutajal asukoha ja Blokeeri juhtelementide kasutamine, kui kasutaja pole usaldusväärses võrgus.

- **Seadme platvormi**. Kasutada seadme platvorm, nagu iOS-i, Android, Windows Mobile või Windows, kui poliitika rakendamist.

- **Seadme lubatud**. Seadme olek, kas lubada või keelata, on kinnitatud seadme poliitika hindamise käigus. Kui keelate kadunud või varastatud seadme kataloogis, seda ei saa enam täita nõuete poliitika.

- **Sisselogimine ja kasutajale risk**. Saate juurdepääsu riski poliitikaid [Azure AD identiteedi kaitse](active-directory-identityprotection.md) . Tingimusvormingu juurdepääsu riski poliitika aidata oma ettevõtte advance kaitse risk sündmuste ja ebatavalised tegevuse alusel.


## <a name="controls"></a>Juhtelemendid

Need on juhtelemendid, mille abil saate jõustada juurdepääsu poliitika.

- **Mitmikautentimise**. Saate nõuda tugeva autentimise mitmikautentimise kaudu. Azure'i Mitmikautentimise või kohapealse mitmikautentimise pakkuja, koos Active Directory Federation Services (AD FS) abil saate kasutada mitmikautentimise. Mitmikautentimise aitab kaitsta volitamata kasutaja, kellel on juurdepääs lubatud kasutaja mandaat omandanud juurdepääsu ressursse.

- **Blokeeri**. Saate rakendada nagu kasutaja juurdepääsu kasutaja asukohta. Näiteks saate blokeerida Accessi, kui kasutaja pole usaldusväärses võrgus.

- **Ühilduvad seadmed**. Saate seada tingimusvormingu juurdepääsupoliitikaid seadme tasemel. Poliitika võib seadistada nii, et ainult on domeeni ühendatud arvutites või mobiilsideseadmete jaoks, mis on registreeritud mobiilsideseadme halduse rakenduses, pääsete juurde oma ettevõtte ressursse. Näiteks saate kontrollida seadme Intune abil ja seejärel aruande selle jõustamiseks Azure AD kui kasutaja proovib juurde pääseda rakenduse. Rakenduste ja andmete kaitsmiseks Intune kasutamise kohta üksikasjalikud juhised leiate teemast [kaitse rakendused ja andmed koos Microsoft Intune'i](https://docs.microsoft.com/intune/deploy-use/protect-apps-and-data-with-microsoft-intune). Samuti saate kasutada Intune'i jõustamiseks andmekaitse kadunud või varastatud seadmete jaoks. Lisateabe saamiseks vt [täieliku või valikulise kustutamise, kasutades Microsoft Intune andmete kaitsmiseks](https://docs.microsoft.com/intune/deploy-use/use-remote-wipe-to-help-protect-data-using-microsoft-intune).

## <a name="applications"></a>Rakenduste

Saate jõustada juurdepääsu poliitika tasemel. Saate seada kohapealse ja pilveteenuse juurdepääsutasemete rakenduste ja teenuste jaoks. Poliitika on rakendatud otse veebisaidile või teenuse. Brauseri ja rakendusi, et juurdepääs teenuse poliitika jõustatakse.


## <a name="device-based-conditional-access"></a>Device põhineva juurdepääsu

Saate piirata juurdepääsu rakendusi seadmetes, mis on registreeritud Azure AD ja mis on teatud kindlatele tingimustele. Device põhineva juurdepääsu kaitseb ettevõtte ressursid ressursside kaudu juurdepääsu kasutajatele:

- Tundmatute või mittehallatavasse seadmed.
- Seadmed, mis ei vasta selle turbepoliitikate ettevõtte häälestamine.

Saate määrata vastavalt nende nõuete poliitika.

- **Domeeni ühendatud seadmete**. Seadnud poliitika juurdepääsu piiramiseks seadmetes, mis on liitunud kohapealse Active Directory domeeni ja mis on ka registreeritud Azure AD. See poliitika kehtib Windows lauaarvutite, sülearvutid ja ettevõtte tahvelarvutites.
Automaatse registreerimise Azure AD domeeni ühendatud seadmete häälestamise kohta leiate lisateavet teemast [automaatse registreerimise Windows Azure Active Directory domeeni ühendatud seadmete häälestamine](active-directory-conditional-access-automatic-device-registration-setup.md).

- **Ühilduvad seadmed**. Seadnud poliitika juurdepääsu piiramiseks seadmed, mis on tähistatud **nõuetele vastavuse** kataloogis halduse süsteem. See poliitika tagab, et ainult seadmete turbepoliitikate jõustamine faili krüptimise seadmes, nt vastavad on lubatud juurdepääs. Selle poliitika abil saate piirata juurdepääsu järgmised seadmed:

    - **Windowsi domeeni ühendatud seadmete**. Hallatud, System Center Configuration Manager (praegune kontoris) hübriidkonfiguratsiooni juurutatud.
    - **Windows 10 Mobile töö- või isikliku seadmed**. Hallatud Intune või muude tootjate toetatud mobiilsideseadme halduse süsteem.
    - **iOS-i ja Androidi seadmete**. Hallatud Intune.


Kasutajad, kes rakendustele, mis on kaitstud seadme vastavalt, poliitika asutust peavad juurde rakenduse seade, mis vastab sellele seadnud. Juurdepääs keelatud kui proovitakse seade, mis ei vasta nõudmistele.

Azure AD device põhineva sertimine asutuse poliitika konfigureerimise kohta leiate teavet teemast [Azure Active Directory ühendatud rakenduste device põhineva juurdepääsu poliitika määramine](active-directory-conditional-access-policy-connected-applications.md).

## <a name="resources"></a>Ressursid

Lugege järgmist ressursi kategooriad ja artiklid Lisateavet säte juurdepääsu oma ettevõtte jaoks.


### <a name="multi-factor-authentication-and-location-policies"></a>Mitme teguri autentimis- ja asukoha poliitika

- [Alustamine: Azure'i AD-ühendus rakenduste alusel rühmitamine, asukoha ja mitmikautentimise poliitikate juurdepääsu](active-directory-conditional-access-azuread-connected-apps.md)
- [Toetatavad rakendused](active-directory-conditional-access-supported-apps.md)


### <a name="device-based-conditional-access"></a>Device põhineva juurdepääsu

- [Azure Active Directory ühendatud rakenduste jaoks juurdepääsu reguleerimine device põhineva juurdepääsu poliitika määramine](active-directory-conditional-access-policy-connected-applications.md)
- [Automaatse registreerimise Windowsi domeeni ühendatud seadmete Azure Active Directory häälestamine](active-directory-conditional-access-automatic-device-registration-setup.md)
- [Azure Active Directory access probleemide tõrkeotsing](active-directory-conditional-access-device-remediation.md)
- [Kaitske andmeid kadunud või varastatud seadmete Microsoft Intune'i abil](https://docs.microsoft.com/intune/deploy-use/use-remote-wipe-to-help-protect-data-using-microsoft-intune)


### <a name="protect-resources-based-on-sign-in-risk"></a>Kaitse põhjal sisselogimise risk ressursid

-   [Azure'i AD identiteedi kaitse](active-directory-identityprotection.md)

### <a name="next-steps"></a>Järgmised sammud

- [KKK-d juurdepääsu](active-directory-conditional-faqs.md)
- [Tehniline teave](active-directory-conditional-access-technical-reference.md)
