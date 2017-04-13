<properties
   pageTitle="Azure Active Directory B2B koostöö | Microsoft Azure'i"
   description="Azure Active Directory B2B koostöö võimaldab äripartneritega juurde oma ettevõtte rakendused, tähistab ühe Azure AD kasutajatele iga konto"
   services="active-directory"
   documentationCenter=""
   authors="curtand"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/23/2016"
   ms.author="curtand"/>

# <a name="azure-active-directory-b2b-collaboration"></a>Azure Active Directory B2B koostöö

Azure Active Directory (Azure AD) B2B koostöö võimaldab juurdepääsu lubamine oma ettevõtte rakendusi partneri hallatavad identiteedid kaudu. Saate luua rist – ettevõtte seosed, kutsudes ja lubada kasutajate partneri ettevõtetele juurdepääsu oma ressursse. Keerukuse on vähendatud, kuna iga ettevõtte federates üks kord Azure Active Directory ja iga kasutajat tähistab ühte Azure AD konto. Oma äripartnerite haldamine oma kontode Azure AD, kuna access tühistatakse, kui partneri kasutajad saavad ettevõtetest lõpetatakse ja ootamatuid juurdepääsu kuuluvus sisemise kataloogide kaudu ei saa suurendada turvalisus. Business partnerid, kellel ei ole juba Azure AD, on B2B koostöö sujuv registreerumise kogemus anda oma äripartnerite Azure AD kontod.

-   Oma äripartnerite kasutada oma sisselogimise identimisteave, mis vabastab teid mõne välise partneri directory haldamine ja Accessi eemaldamiseks, kui kasutajad lahkuvad partneri organisatsiooni.

-   Accessi rakenduste sõltumatult business partneri konto elutsükli haldamine See tähendab, näiteks, et saate tühistada juurdepääsu ilma küsige midagi tegema oma ettevõtte partneri IT-osakonna poole.

## <a name="capabilities"></a>Võimalused

B2B koostöö lihtsustab haldust ja parandab partnerite juurdepääsuga ettevõtte ressursse, sh SaaS rakendused, nagu Office 365, Salesforce, Azure'i teenused ja iga mobile cloud ja kohapealse nõuded meeles rakendus turvalisus. B2B koostöö lubab partnerite hallata oma kontode ja suurettevõtete saate rakendada turbepoliitikate partnerite juurdepääsuga.

Koostöö on lihtne konfigureerida Azure Active Directory B2B lihtsustatud registreerumise partnerite igas suuruses isegi juhul, kui neil pole oma Azure Active Directory kaudu e-posti kontrollida käigus. Samuti on lihtne säilitada välise kaustu või partneri federation konfiguratsioone kohta.

## <a name="b2b-collaboration-process"></a>B2B koostöö protsess

1. Azure'i AD B2B koostööfunktsioon võimaldab ettevõtte administraatori kutsuda ja lubada rohkem kui 2000 joonte komaga eraldatud väärtuste (CSV) faili üleslaadimise B2B koostöö portaali Väliskasutajate kogum.

  ![CSV-faili üleslaadimine dialoogiboks](./media/active-directory-b2b-collaboration-overview/upload-csv.png)

2. Portaali saavad saata meilikutsed ka need väliskasutajad.

3. Kutsutud kasutaja on kas töö konto Microsoft (hallatav Azure AD) sisse logida, või pöörduge töö uue konto Azure AD.

4. Kui olete sisse loginud, suunatakse kasutaja rakendus, mis on nendega ühiskasutuses.

Kutsete tarbija e-posti aadressid (nt Gmail või [*teenuste comcast.net*](http://comcast.net/)) pole praegu toetatud.

Lisateavet B2B koostöö toimimise kohta, vaadake [seda videot](http://aka.ms/aadshowb2b).

## <a name="next-steps"></a>Järgmised sammud
Sirvige meie teised artiklid Azure AD B2B koostöö.

- [Mis on Azure AD B2B koostöö?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Kuidas see toimib](active-directory-b2b-how-it-works.md)
- [Üksikasjaliku selgituse](active-directory-b2b-detailed-walkthrough.md)
- [CSV-faili vorming viide](active-directory-b2b-references-csv-file-format.md)
- [Väliste kasutajate Turbeloa vorming](active-directory-b2b-references-external-user-token-format.md)
- [Väliste kasutajate objekti atribuutide muutmine](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [Praegune eelvaade piirangud](active-directory-b2b-current-preview-limitations.md)
- [Artikli registri Rakendusehaldus Azure Active Directory 's](active-directory-apps-index.md)
