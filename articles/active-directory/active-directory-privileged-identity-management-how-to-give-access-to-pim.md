<properties
   pageTitle="Kuidas anda juurdepääsu PIM | Microsoft Azure'i"
   description="Saate teada, kuidas lisada laiendiga Azure Active Directory õigustega identiteedi haldus kasutajate rollid nii, et nad saavad hallata PIM."
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
   ms.date="09/22/2016"
   ms.author="kgremban"/>

# <a name="how-to-give-access-to-manage-azure-ad-privileged-identity-management"></a>Kuidas anda juurdepääsu haldamine Azure AD õigustega identiteetide haldus

Globaalne administraator, kes võimaldab Azure AD õigustega identiteetide haldus (PIM) ettevõtte automaatselt hankida rollimääranguid ja PIM juurdepääsu. Keegi saab kirjutamisõigused vaikimisi küll, sh muude globaalse administraatorid. Muud globaalne adminstrators, Turve administraatorid ja turvalisus lugejad on kirjutuskaitstud juurdepääs Azure AD PIM. Anda juurdepääs PIM, esimene kasutaja saab määrata teistele **õigustega rolli administraatori** roll. Selle ülesande peab sees PIM ise teha ja PowerShelli või muude portaalide kaudu ei saa muuta.

> [AZURE.NOTE] Azure'i AD PIM haldamine nõuab Azure'i MFA. Kuna Microsofti kontot ei saa registreerida Azure'i MFA jaoks, kes Microsofti kontoga sisse logib kasutaja ei pääse Azure AD PIM.

Veenduge, et on alati vähemalt kaks kasutajat õigustega roll administraatori roll, juhuks, kui üks kasutaja on lukustatud välja või oma konto kustutatakse.

## <a name="give-another-user-access-to-manage-pim"></a>Teise kasutaja juurdepääsu haldamiseks PIM

1. Valige **Azure AD õigustega identiteetide haldamisviis** rakenduse armatuurlaual [Azure portaali](https://portal.azure.com/) sisse logima.
2. Valige **õigustega rollide haldamine** > **administraatori õigustega rolli** > **lisamine**.

    ![Lisage õigustega rolli administraatorid - kuvatõmmis][1]

4. Hallatud kasutajate lisamine enne, samm 1 on juba lõpule viidud. Valige etappi 2, **Valige kasutajad** ja otsida kasutaja, mille soovite lisada.

    ![Valige kasutajad – kuvatõmmis][2]

6. Valige otsingu tulemuste seast soovitud kasutaja ja klõpsake nuppu **valmis**.
7. Klõpsake valiku salvestamiseks nuppu **OK** . Valitud kasutajale kuvatakse õigustega rolli administraatorite loendit.

    - Iga kord, kui määrate uude rolli kellelegi, nad on automaatselt seadistada tingimustele vastavad aktiveerimiseks roll. Kui soovite teha neid püsiv roll, klõpsake loendis kasutaja. Valige kasutaja teabe menüü **perm teha** .

8. Kasutaja lingi saatmiseks [Azure AD õigustega identiteetide haldamisviis töötamise alustamine](active-directory-privileged-identity-management-getting-started.md).


## <a name="remove-another-users-access-rights-for-managing-pim"></a>Teise kasutaja juurdepääsu õiguste haldamise PIM eemaldamine

Enne, kui eemaldate kellegi õigustega rolli administraatori roll, alati veenduge, et ikkagi määratud kaks kasutajat.

1. Klõpsake armatuurlaual PIM **õigustega rolli administraatori**roll.  Kuvatakse praegu rolli kasutajate loendit.
2. Klõpsake loendis kasutaja kasutaja.
3. Klõpsake **eemaldada**.  Teil on esitatud kinnitusteade.
4. Klõpsake nuppu **Jah** eemaldada kasutaja roll.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Järgmised sammud
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-give-access-to-pim/PIM_add_PRA.png
[2]: ./media/active-directory-privileged-identity-management-how-to-give-access-to-pim/PIM_select_users.png
