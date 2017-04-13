<properties
pageTitle="Azure Active Directory rakenduste ja teenuste põhisumma objektide | Microsoft Azure'i"
description="Arutelu rakenduste ja teenuste põhisumma objektide Azure Active Directory seos"
documentationCenter="dev-center-name"
authors="bryanla"
manager="mbaldwin"
services="active-directory"
editor=""/>

<tags
ms.service="active-directory"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="identity"
ms.date="08/10/2016"
ms.author="bryanla;mbaldwin"/>

# <a name="application-and-service-principal-objects-in-azure-active-directory"></a>Rakenduste ja teenuste põhisumma objektide Azure Active Directory
Kui loete kohta on Azure Active Directory (AD) "rakendus", ei ole alati selge täpselt mis suunatakse autori. Selles artiklis eesmärk on muuta selgemaks, määratledes kontseptuaalne ja konkreetsete aspektide Azure AD Rakenduse integreerimine, näiteks registreerimist ja nõusolekut [mitme rentniku rakenduse](active-directory-dev-glossary.md#multi-tenant-application)jaoks.

## <a name="overview"></a>Ülevaade
Rakenduse Azure AD on laiem kui lihtsalt tükk tarkvara. See on kontseptuaalne termin, viidates mitte ainult rakenduse tarkvara, kuid registreerimist (alias: identiteedi konfigureerimine) Azure AD, mis võimaldab seda autentimise ja luba "Vestlused" käitusajal osalemiseks. Määratluse saate rakenduse funktsioon [Kliendi](active-directory-dev-glossary.md#client-application) rolli (tarbimine ressursi), [ressursside serveri](active-directory-dev-glossary.md#resource-server) rolli (paljastamine API klientidele) või ka mõlemad. Vestluse protokoll on määratletud mõne [OAuthi 2.0 loa andmine meilivoo](active-directory-dev-glossary.md#authorization-grant), on eesmärki võimaldada kliendi/ressursile juurdepääsu/ressursi andmete kaitsmiseks vastavalt. Nüüd esmalt tutvustatakse põhjalikult taseme ja vaadake, kuidas Azure AD Rakenduse mudel rakenduse ettevõttesiseselt tähistab. 

## <a name="application-registration"></a>Rakenduse registreerimine
Rakenduse registreerimisel [Azure klassikaline portaali][AZURE-Classic-Portal], kaks objektid luuakse teie Azure AD rentniku: objekti rakenduste ja teenuste peamine eesmärk.

#### <a name="application-object"></a>Rakenduse objekt
Rakenduse Azure AD on *määratletud* selle üks ja ainult rakenduse objekti, mis asub Azure AD rentniku, kus on registreeritud rakenduse, nimetatakse rakenduse "kodu" rentniku. Objekti application identiteedi seotud teave, rakenduse ja on Mall, mille kohta oma vastava teenuse põhisumma objektide on *tuletatud* käitusajal kasutamiseks. 

Kui arvate, rakenduse *globaalne* kujutis rakenduse (kõik rentnikud kasutamiseks) ja teenuse põhilise nimega *kohalik* esindus (kasutamiseks teatud rentniku jaoks). Azure'i AD Graphi [rakenduse üksuse] [ AAD-Graph-App-Entity] määratleb objekti rakenduse skeemi. Objekti rakendus on seega 1:1 suhe tarkvara rakendus ja 1:*n* suhe oma vastavate *n* teenuse põhisumma objektide.

#### <a name="service-principal-object"></a>Teenuse peamine eesmärk
Teenuse peamine eesmärk määratleb poliitika ja õiguste rakenduse, luues turvalisus subjekt tähistada rakenduse kasutamisel käitusajal ressursid. Azure'i AD Graphi [ServicePrincipal üksuse] [ AAD-Graph-Sp-Entity] määratleb skeemi teenuse peamine eesmärk. 

Teenuse peamine eesmärk on nõutav iga rentniku, mille eksemplari selle rakenduse kasutamine olema esindatud lubamine turvaline juurdepääs kuuluv Kasutajakontod selle ressursid. Ühe-rentniku rakendus on ainult üks teenuse põhilise (oma kodune rentnik). Mitme rentniku [veebirakenduse](active-directory-dev-glossary.md#web-client) on ka teenuse põhilise iga rentnik, kus administraator või kinnitust, et rentnikust nõusolekut, võimaldades juurdepääsu oma ressursse. Pärast nõusoleku teenuse peamine eesmärk on tutvuda tulevaste luba kutsed. 

> [AZURE.NOTE] Rakenduse objekti, mis tahes muudatused kajastuvad ka teenuse peamine eesmärk rakenduse Avaleht rentniku ainult (rentnik, kui see on registreeritud). Mitme rentniku rakendusi, rakenduse objekti ei kajastu mis tahes tarbija rentnikud teenuse põhisumma objektid, kuni tarbija rentniku eemaldab juurdepääsu ja annab uuesti.

## <a name="example"></a>Näide
Järgmine diagramm näitab rakenduse rakenduse objekt ja vastav seos teenuse põhisumma objektide valimi mitme rentniku rakenduse nimega **HR rakenduse**kontekstis. Selle stsenaariumi on kolm Azure AD rentnikke. 

- **Adatum** – rentniku kasutatavaid ettevõte, mis on välja töötatud **HR rakendus**
- **Contoso** - rentniku kasutatavaid Contoso ettevõte, mis on tarbija **HR rakendus**
- **Fabrikam** – rentniku kasutavad ettevõtte Fabrikam, mis ka tarbib **HR rakendus**

![Objekti rakenduste ja teenuste peamine eesmärk seos](./media/active-directory-application-objects/application-objects-relationship.png)

Eelmise skeemi samm 1 on rakenduse Avaleht rentniku rakenduste ja teenuste põhisumma objektide loomise protsess.

Kui teostada Contoso ja Fabrikam administraatorid nõusolekut, on teenuse peamine eesmärk samm 2, oma ettevõtte Azure AD rentniku loodud ja määratud õigused, et administraator on andnud. Samuti võtke arvesse, et HR rakendus võib konfigureeritud/koostanud lubamiseks nõusolekut kasutajatele mõeldud kasutamiseks.

Samm 3 HR taotluse (Contoso ja Fabrikam) iga tarbija rentnikud on oma teenuse peamine eesmärk. Iga tähistab kasutamisel käitusajal, reguleerib õiguste rakenduse eksemplari on nõustunud vastav administraator.

## <a name="next-steps"></a>Järgmised sammud
Rakenduse rakenduse objekti pääseb Azure AD Graph API, nagu esindab oma OData [rakenduse üksus][AAD-Graph-App-Entity]

Rakenduse põhisumma objekti pääseb Azure AD Graph API, nagu esindab oma OData [ServicePrincipal üksus][AAD-Graph-Sp-Entity]



<!--Image references-->

<!--Reference style links -->
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AZURE-Classic-Portal]: https://manage.windowsazure.com