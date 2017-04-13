<properties
   pageTitle="Turbeviisard Azure AD õigustega identiteetide haldus"
   description="Esimest korda, kui kasutate Azure Active Directory õigustega identiteedi haldus laiend, ilmub turvalisus viisardi abil. Selles artiklis kirjeldatakse juhiseid viisardi abil."
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
   ms.date="07/01/2016"
   ms.author="kgremban"/>

# <a name="the-azure-ad-privileged-identity-management-security-wizard"></a>Turbeviisard Azure AD õigustega identiteetide haldus

Kui olete käivitamiseks Azure'i õigustega identiteedi haldamine (PIM) oma ettevõtte jaoks esimene isik, esitatakse viisardi abil. Viisard aitab teil mõista seotud ohtude õigustega identiteedid ja kuidas neid riske PIM abil. Te ei pea muuta olemasolevaid rollimääranguid viisardis soovi seda hiljem.

## <a name="what-to-expect"></a>Mida oodata

Enne ettevõtte abil PIM, kõik rollimäärangud on püsiv: kasutajad on alati sooritaja roll isegi juhul, kui nad ei pruugi praegu oma õigusi.  Viisardi esimesel etapil näete kõrge suurte õigustega rollide loend ja nende rollid praegu on mitu kasutajat. Saate minna soovite saada lisateavet kasutajate kui ühe kindla rolliga või neist on võõras.

Teise etapi viisard pakub võimalus muuta administraatori rollimääranguid.  

> [AZURE.WARNING]On oluline, et teil on vähemalt üks globaalne administraator ja rohkem kui üks õigustega roll administraator koos organisatsioonikonto (mitte Microsofti konto). Kui seal on ainult üks õigustega roll administraator, ei saa organisatsiooni haldamine PIM, kui selle konto kustutatakse.
> Samuti pidage rollimääranguid püsivalt, kui kasutaja on Microsofti konto (konto, mida nad kasutavad Microsofti teenustele nagu Skype ja Outlook.com-i sisse logida). Kui plaanite nõudma MFA aktiveerimise selle rolli jaoks, et kasutaja lukustatud.


Pärast muudatuste tegemist, viisardi enam kuvatakse. Järgmine kord, kui teie või mõne muu õigustega rolli määratud kasutada PIM, kuvatakse PIM armatuurlaud.  

- Kui soovite lisamiseks või kasutajate rollid eemaldamine või muutmine ülesanded püsivate õigus, et lisateavet, kuidas [lisada või eemaldada kasutaja roll](active-directory-privileged-identity-management-how-to-add-role-to-user.md).
- Kui soovite anda kasutajate juurdepääsu haldamine PIM, lugege lisateavet kuidas [hallata PIM juurdepääsu anda](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).



## <a name="next-steps"></a>Järgmised sammud
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]
