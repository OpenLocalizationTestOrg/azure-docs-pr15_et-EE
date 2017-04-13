<properties
   pageTitle="Kuidas lisada või eemaldada Kasutaja rolli | Microsoft Azure'i"
   description="Saate teada, kuidas lisada rollid õigustega identiteedid rakendusega Azure Active Directory õigustega identiteedi haldus."
   services="active-directory"
   documentationCenter=""
   authors="kgremban"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/24/2016"
   ms.author="kgremban"/>

# <a name="azure-ad-privileged-identity-management-how-to-add-or-remove-a-user-role"></a>Azure'i AD õigustega identiteetide haldus: Kuidas lisada või eemaldada Kasutaja rolli

Üldadministraator (või ettevõtte administraatori) koos Azure Active Directory (AD), värskendamiseks, millistel kasutajatel on **jäädavalt** Azure AD rollidele määratud. Seda saab teha näiteks PowerShelli cmdlet-käskude `Add-MsolRoleMember` ja `Remove-MsolRoleMember`. Või nad saavad kasutada Azure klassikaline portaali, nagu on kirjeldatud [Azure Active Directory administraatorirollide määramine](active-directory-assign-admin-roles.md).

Azure'i AD õigustega identiteedi haldamise rakendus võimaldab õigustega rolli administraatorid püsivate rollimääranguid, samuti teha. Lisaks saate teha kasutajate **õigus** administraatorirollide õigustega rolli administraatorid. Õigus administraator saate aktiveerida roll, kui need on vaja seda ja siis nende õiguste lõpeb, kui need on valmis.

## <a name="manage-roles-with-pim-in-the-azure-portal"></a>Azure'i portaalis PIM rollide haldamine

Teie asutuse saate määrata kasutajatele erinevate administraatorirollid Azure AD, Office 365 ja muude Microsofti teenuste ja rakendused.  Täpsemat teavet saadaolevate rollid kohta leiate [Azure'i AD PIM rollid](active-directory-privileged-identity-management-roles.md).

Lisada või eemaldada kasutaja identiteedi haldusega õigustega roll, avab PIM armatuurlaud. Klõpsake nuppu **Kasutajad administraatorirollide** või rollid tabelist valida kindla rolliga (nt üldadministraator).

> [AZURE.NOTE] Kui PIM pole veel Azure'i portaalis lubanud, avage [Alustamine Azure AD PIM](active-directory-privileged-identity-management-getting-started.md) üksikasju.

Kui soovite teise kasutaja juurdepääsu andmine PIM ise, rollid, mis nõuab PIM kasutajal on on kirjeldatud, põhjalikumaks [Kuidas PIM juurdepääsu anda](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).

## <a name="add-a-user-to-a-role"></a>Kasutaja lisamine rolli

1. [Azure'i portaalis](https://portal.azure.com/)valige **Azure AD õigustega identiteetide haldamisviis** paan armatuurlaual.
2. Valige **õigustega rollide haldamine**.
3. Valige roll, mida soovite hallata **rolli kokkuvõtva** tabeli.
4. Roll tera, valige **Lisa**.
5. Klõpsake nuppu **Valige kasutajad** ja **Valige kasutajad** enne kasutaja otsing.  
6. Valige otsingu tulemuste loendist kasutaja ja klõpsake nuppu **valmis**.
4. Klõpsake valiku salvestamiseks nuppu **OK** . Valitud kasutajale kuvatakse loendis saamise roll.

> [AZURE.NOTE]
>Uute kasutajate lisamine rolli laiene ainult vaikimisi roll. Kui soovite teha roll püsiv, klõpsake loendis kasutaja. Kasutaja teave kuvatakse uus laba. Valige kasutaja teabe menüü **perm teha** .  
>Kui kasutaja ei saa registreerida Azure'i mitme teguriga autentimine (MFA) või kasutab Microsofti kontoga (tavaliselt @outlook.com), peate tegema need püsiv kõigi nende rollid. Registreeruge MFA aktiveerimisel palutakse õigus administraatoritele.

Nüüd, kui kasutaja on õigus saada rollid, andke sellest talle teada, et need saate selle aktiveerida, [Kuidas aktiveerida või desaktiveerida rolli](active-directory-privileged-identity-management-how-to-activate-role.md)juhiste järgi.

## <a name="remove-a-user-from-a-role"></a>Kasutaja eemaldamine rollid

Saate kasutajate eemaldamine õigus rollimääranguid, kuid veenduge, et on alati vähemalt üks kasutaja, kes on püsivate üldadministraator.

Mõne kindla kasutaja rollid eemaldamiseks tehke järgmist

1. Liikuge loendis rollirühma rolli valides rolli Azure AD PIM armatuurlaua või **kasutajate administraatorirollide** klõpsake nuppu.
2. Klõpsake loendis kasutaja kasutaja.
3. Klõpsake käsku **Eemalda**. Sõnumi palub teil kinnitada.
4. Klõpsake nuppu **Jah** eemaldada kasutaja roll.

Kui te pole kindel, millistel kasutajatel on vaja veel nende rollimääranguid, siis saate [alustada rolli ülevaate access](active-directory-privileged-identity-management-how-to-start-security-review.md).


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Järgmised sammud
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]
