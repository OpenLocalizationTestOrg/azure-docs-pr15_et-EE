<properties
   pageTitle="Identiteedi haldamise rentnikuga rakenduste | Microsoft Azure'i"
   description="Head tavad autentimine, autoriseerimine ja identiteedi haldamine rentnikuga rakendustes."
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="roshar"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/02/2016"
   ms.author="mwasson"/>

# <a name="identity-management-for-multitenant-applications-in-microsoft-azure"></a>Microsoft Azure'i rentnikuga rakenduste identiteetide haldus

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Selle sarja kirjeldatakse heade tavade multitenancy, kasutades Azure AD autentimis- ja identiteedi haldamiseks.

Kui olete hoone rentnikuga rakenduse, haldab üks esimese probleeme kasutajate identiteete, kuna nüüd iga kasutaja kuulub rentniku jaoks. Näiteks:

- Kasutajate logige sisse oma ettevõtte mandaadi.
- Kasutajate peaks olema juurdepääs oma ettevõtte andmeid, kuid andmed, mis kuulub teine rentnikule.
- Ettevõtte saate rakenduse kasutajaks, ja seejärel määrata rakenduse rollide oma liikmetele.

Azure Active Directory (Azure AD) on hea funktsioonid, mis toetavad kõiki järgmisi olukordi.

Selle sarja artiklite juurde loodud lõpule jõudnud, [lõpuni rakendamist] [ tailspin] rentnikuga rakenduse. Artiklid kajastuvad, mida me õppinud protsessi koostamise rakendus. Alustamine rakenduse, lugege teemat [uuringute rakendus](https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/docs/running-the-app.md).

Siit leiate loendi selle sarja artikleid:

- [Identiteedi haldamise rentnikuga rakenduste tutvustus](guidance-multitenant-identity-intro.md)
- [Tailspin küsitluste rakenduse kohta](guidance-multitenant-identity-tailspin.md)
- [Autentimist kasutades Azure AD ja OpenID ühendamine](guidance-multitenant-identity-authenticate.md)
- [Taotluste-põhine identiteedid töötamine](guidance-multitenant-identity-claims.md)
- [Registreerumise ja rentniku kasutuselevõtt](guidance-multitenant-identity-signup.md)
- [Rakenduse rollid](guidance-multitenant-identity-app-roles.md)
- [Rollipõhine ja ressursside-põhiste autoriseerimine](guidance-multitenant-identity-authorize.md)
- [Turvaliseks taustväärtus veebi-API](guidance-multitenant-identity-web-api.md)
- [Vahemällu OAuth2 Accessi sõned](guidance-multitenant-identity-token-cache.md)
- [Kliendi AD FS rentnikuga rakenduste Azure ühendamine](guidance-multitenant-identity-adfs.md)
- [Accessi sõned toomine Azure AD kliendi kinnituse abil](guidance-multitenant-identity-client-assertion.md)
- [Kasutades klahvi Vault kaitsmiseks rakenduse saladusi](guidance-multitenant-identity-keyvault.md)

[tailspin]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps
