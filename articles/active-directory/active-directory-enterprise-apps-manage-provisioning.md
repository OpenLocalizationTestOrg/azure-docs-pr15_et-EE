<properties
    pageTitle="Kasutaja halduse enterprise rakenduste Azure Active Directory eelvaates ettevalmistamise | Microsoft Azure'i"
    description="Siit saate teada, kuidas hallata kasutajate konto ettevalmistamise enterprise rakenduste Azure Active Directory preview abil"
    services="active-directory"
    documentationCenter=""
    authors="asmalser"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/12/2016"
    ms.author="asmalser"/>

#<a name="preview-managing-user-account-provisioning-for-enterprise-apps-in-the-new-azure-portal"></a>Eelvaade: Haldamine kasutajakonto ettevalmistamise enterprise rakenduste uue Azure'i portaalis

Selles artiklis kirjeldatakse, kuidas kasutada [Azure portaali](https://portal.azure.com) haldamiseks ettevalmistamise ja tühistage ettevalmistamise rakendusi, mis toetavad tegemist, eriti need, mis on lisatud [Azure Active Directory rakenduse Galerii](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery)"esiletõstetud" kategooriast automaatse kasutajakonto. Selle uue Azure portaali teenuse haldamine on praegu avaliku eelvaate ja selles artiklis kirjeldatakse uusi funktsioone, samuti ajutiste piirangutega mis on kohast ajal eelvaade. [Mis on eelvaateversioonis?](active-directory-preview-explainer.md)

Automaatse kasutaja konto ettevalmistamise ja kuidas see toimib kohta leiate lisateavet teemast [automatiseerida kasutaja ettevalmistamise ja Azure Active Directory SaaS rakenduste Deprovisioning](active-directory-saas-app-provisioning.md).

##<a name="finding-your-apps-in-the-new-portal"></a>Uue portaali rakenduste leidmine

Mai 2016 seisuga kõik rakendused, mis on konfigureeritud ühe sisselogimise Directorys directory administraator [Azure Active Directory rakenduse Galerii](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery) [Azure klassikaline portaalis](https://manage.windowsazure.com)sees, saab nüüd vaadata ja hallatavate uue Azure'i portaalis.

**Ettevõtte rakenduste** osas uue Azure portaali, millele pääseb vasakpoolsel navigeerimisalal menüüs **Rohkem teenuseid** leiate need rakendused. Ettevõtte rakendustel on kasutusele võetud ja kasutavad kasutajad oma ettevõttes.

![Ettevõtte rakenduste blade][0]

Klõpsake vasakul linki **Kõik rakendused** kuvatakse loendi Kõik rakendused, mis on konfigureeritud, sh rakendusi, mis oli lisatud galeriist. Valige rakendus laadib ressursi tera rakenduse, kus aruandeid saab vaadata rakenduse ja erinevaid sätteid saab hallata.

Kasutajakonto sätete ettevalmistamise saab hallata, valides **Provisioning** vasakul.

![Rakenduse ressursi blade][1]


##<a name="provisioning-modes"></a>Ettevalmistamise režiimi

**Provisioning** tera algab **režiimi** menüü, mis näitab, milliseid ettevalmistamise režiimid on toetatud rakenduse enterprise, ja võimaldab tal olema konfigureeritud. Saadaolevad suvandid on järgmised.

* **Automaatne** – see suvand kuvatakse, kui Azure AD toetab automaatse API-põhiste ettevalmistamise ja/või tühistage ettevalmistamise kasutajakontosid, et see rakendus. Selle režiimi valimine kuvab liides, mis juhendab administraatorid konfigureerida Azure AD ühenduse rakenduse kasutaja halduse API, konto vastendused ja töövood, mis määratlevad, kuidas kasutaja kontot tuleks andmevoo vahel Azure AD loomise kaudu ja rakendus ja Azure AD, teenuse ettevalmistamise haldamine.

* **Käsitsi** – see suvand kuvatakse, kui Azure AD ei toeta automaatse ettevalmistamise kasutajakontosid, et see rakendus. See suvand tähendab, et kasutaja Kontokirjetega talletatud rakenduse tuleb hallata abil välise protsess, mis põhineb kasutaja haldus ja ettevalmistamise võimaluste esitatud taotluse (mis võib sisaldada SAML just ettevalmistamise).


##<a name="configuring-automatic-user-account-provisioning"></a>Automaatse kasutaja konto ettevalmistamise konfigureerimine

Suvand **Automaatne** kuvatakse kuva, mis on jagatud nelja jaotised.

###<a name="admin-credentials"></a>Administraatori identimisteave
See on, kus identimisteabe vaja Azure AD ühenduse rakenduse kasutaja halduse API on sisestatud. Nõutav sisend erineb sõltuvalt rakenduse. Mandaadi tüübid ja teatud rakenduste nõuete kohta leiate teemast [konfiguratsiooni õpetuse selle kindla rakenduse](active-directory-saas-app-provisioning.md#list-of-apps-that-support-automated-user-provisioning).

Klõpsake nuppu **Testi ühendust** võimaldab teil testida identimisteabe lastes Azure AD püüdke ühenduse rakenduse kasutaja ettevalmistamise rakenduse esitatud mandaadi abil.

###<a name="mappings"></a>Vastendused
See on, kus administraatorid saavad vaadata ja redigeerida mis kasutaja atribuutide liikumist vahel Azure AD ja sihtrakendust, kui kasutaja kontod on ette valmistatud või mida on värskendatud.

On eelkonfigureeritud Azure AD kasutaja objektid ja iga SaaS rakenduse kasutaja objektide vaheliste vastenduste kogum. Mõni muud tüüpi objektid, nt rühmade või Kontaktide haldamine Valige mõni need vastendused tabel näitab vastenduse redaktori paremale, kus nad saavad vaadata ja kohandada.

![Rakenduse ressursi blade][2]

Toetatud kohandused ajal esimene eelvaade on järgmised.

* Lubamine ja keelamine vastendused teatud objektid, nt Azure AD kasutaja objekti SaaS rakenduse objekt.

* Redigeerimine, millised atribuudid flow Azure AD kasutaja objekti rakenduse kasutaja objektile. Atribuut vastendamise kohta leiate lisateavet teemast [mõistmine atribuudi vastendamine tüübid](active-directory-saas-customizing-attribute-mappings.md#understanding-attribute-mapping-types).

* Filtreerida, mis tuleks sooritada Azure AD sihtrakendust, mis on Azure portaali uus funktsioon ettevalmistamise toiminguid. Selle asemel Azure AD täielikult-sünkroonimine objektid, saate piirata soovitud toimingud. Ainult, valides **Update**, Azure AD ainult värskenduste olemasolevale kasutajale rakenduses kontod ja luua uusi. **Loo**ainult valides Azure ainult loob uue Kasutajakontod, kuid ei värskenda olemasolevate. See funktsioon võimaldab administraatorid luua erinevate vastendused konto loomine ja värskendamine töövood. Täielik võimalus luua mitme vastendused rakenduse kohta on planeeritud hiljem perioodil eelvaade.

###<a name="settings"></a>Sätted
Selles jaotises saavad administraatorid käivitamine ja peatamine Azure AD, ettevalmistamise teenuse valitud rakenduse kui ka soovi ettevalmistamise vahemälu ja uuesti teenuse.

Kui ettevalmistamise on lubatud rakenduse esimest korda, lülitage teenuse **Olek ettevalmistamise** **kohta,**et muuta. See põhjustab teenuse ettevalmistamise Azure AD teha algse sünkroonimise, kus loeb määratud jaotises **kasutajad ja rühmad** kasutajate päringute sihtrakenduse neid ja seejärel teha toiminguid ettevalmistamise Azure AD **vastendused** jaotise määratletud. Selle käigus ettevalmistamise teenuse talletab vahemällu talletatud andmetega, mis see on haldamine, kasutajakontode kohta nii, et-hallatavate kontode sees sihtrakenduste, mis olid kunagi ulatuse määramine ei mõjuta tühistage ettevalmistamise toimingud. Pärast algse sünkroonimise, ettevalmistamise teenuse sünkroonib automaatselt kasutaja ja rühma objekte klõpsake iga 10 minuti järel.

Teenuse ettevalmistamise lihtsalt muutmise **Ettevalmistamise oleku** **väljalülitamine** peatab. Selles olekus Azure'i ei luua, värskendada või eemaldada kasutaja või rühma objekte rakenduses. Asendisse Sees oleku muutmine põhjustab teenuse valimiseks, kus see pooleli jäi.

**Tühjendage praeguses olekus ja taaskäivitage sünkroonimise** ruut valimiseks ja salvestamiseks lakkab ettevalmistamise teenuse puistab vahemällu talletatud andmetega, mis kontode Azure AD kohta on haldamine, taaskäivitamist teenuseid ja sooritab algse sünkroonimise uuesti. See suvand võimaldab administraatorid alustamiseks juurutamise ebausaldusväärsete uuesti.

###<a name="synchronization-details"></a>Sünkroonimise üksikasjad
Selles jaotises antakse lisaks ettevalmistamise teenuse, sh ettevalmistamise teenuse parandusfunktsiooni rakendus ja kui palju kasutaja ja rühma objekte juhitakse aegade esimese ja viimase toimingu üksikasjad.

Lingid on esitatud **Provisioning tegevuse aruanne**, mis pakub Logi kõik kasutajad ja rühmad loodud, värskendatud ja eemaldatud vahel Azure AD ja sihtkohas rakendust ja **Provisioning tõrkearuanne** , mis pakub veel üksikasjalikud veateateid kasutaja ja rühma objekte, mis ei loe, loodud, värskendatakse või eemaldada. 

[0]: ./media/active-directory-enterprise-apps-manage-provisioning/enterprise-apps-blade.PNG
[1]: ./media/active-directory-enterprise-apps-manage-provisioning/enterprise-apps-provisioning.PNG
[2]: ./media/active-directory-enterprise-apps-manage-provisioning/enterprise-apps-provisioning-mapping.PNG
