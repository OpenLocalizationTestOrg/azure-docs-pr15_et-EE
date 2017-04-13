<properties
   pageTitle="Kuidas nõuda mitmikautentimise | Microsoft Azure'i"
   description="Saate teada, kuidas nõuda mitmikautentimise (MFA) õigustega identiteedid laiendiga Azure Active Directory õigustega identiteedi haldus."
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

# <a name="how-to-require-mfa-in-azure-ad-privileged-identity-management"></a>Kuidas nõuda MFA Azure AD õigustega identiteedi haldamine

Soovitame, et vajate mitmikautentimise (MFA) kõigi oma administraatorid. See vähendab rünnaku tõttu rikutud parooli.

Saate nõuda, et kasutajad lõpetamiseks on MFA ülesanne sisselogimisel. Ajaveebipostituse [MFA jaoks Office 365 ja Azure MFA](https://blogs.technet.microsoft.com/ad/2014/02/11/mfa-for-office-365-and-mfa-for-azure/) võrdleb sisalduvatest Office ja Azure tellimused sisalduvad Microsoft Azure'i Mitmikautentimise pakkumise funktsioone.

Saate nõuda, teostada kasutajad on MFA ülesanne, kui nad on osa Azure AD PIM aktiveerida. Sellisel viisil, kui kasutaja ei on MFA ülesanne lõpule viia, kui ta on sisse logitud, pakutakse selleks PIM järgi.

## <a name="requiring-mfa-in-azure-ad-privileged-identity-management"></a>Nõudva MFA Azure AD õigustega identiteedi haldamine

Kui haldate identiteedid PIM õigustega rolli administraatorina, võidakse teile kuvada teatised, et soovitada MFA õigustega kontod. Klõpsake turbeteates PIM armatuurlaua ja uue tera avaneb nõudvate peaks MFA administraatorikontode loendit.  Saate nõuda MFA, valides mitme rollid ja seejärel klõpsake nuppu **parandamine** või saate üksikuid rollide kõrval kolmikpunkti nuppu ja seejärel klõpsake nuppu **lahendada** .

> [AZURE.IMPORTANT] Paremklõpsake nüüd Azure'i MFA töötab ainult töö või kooli kontode Microsofti kontod (tavaliselt isikliku kontoga sisse logima Microsofti teenustele nagu Skype, Xbox, Outlook.com jne kasutatava.). Seetõttu, Microsofti kontot ei saa olla õigus administraator, sest nad ei saa kasutada MFA aktiveerimine oma rollid. Kui need kasutajad on vaja haldamine töökoormus Microsofti konto kasutamise jätkamiseks, tõsta need püsivate administraatorid nüüd.

Lisaks saate muuta MFA nõue kindla rolliga klõpsates PIM armatuurlaua jaotises rollid. Klõpsake **sätete** rolli tera ja seejärel valige jaotises mitmikautentimise **lubamine** .

## <a name="how-azure-ad-pim-validates-mfa"></a>Kuidas Azure AD PIM kinnitatakse MFA

On kaks võimalust valideerimise MFA, kui kasutaja aktiveerib rollid.

Kõige lihtsam on kasutada Azure MFA kasutajatele, kellel on aktiveerimise õigustega roll. Selleks, kontrollige, et need kasutajad on litsentsitud vajadusel ja Azure MFA jaoks registreeritud olema. Lisateavet selle kohta, kuidas seda teha on [Azure Mitmikautentimise pilves töötamise alustamine](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md). See on soovitatav, kuid ei pea, et konfigureerida Azure AD jõustamiseks MFA nende kasutajate jaoks, kui neid sisselogimisel. See on MFA kontrolle teeb Azure AD PIM ise.

Võite, kui kasutajad autentida kohapealse olla teie eest MFA identiteedipakkuja. Kui olete konfigureerinud AD sisaldab Federation Services nõudma kiipkaardi autentimise enne juurdepääs Azure AD [turvamine cloud ressursid Azure'i Mitmikautentimise ja AD FS-i](../multi-factor-authentication/multi-factor-authentication-get-started-adfs-cloud.md) konfigureerimine AD FS-i juhised saata Azure AD nõuded. Kui kasutaja üritab aktiveerida rollid, aktsepteerib Azure AD PIM, et MFA on juba valideeritud kasutaja kui selle saab asjakohaste nõuete.


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Järgmised sammud
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]
