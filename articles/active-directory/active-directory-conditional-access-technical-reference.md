
<properties
    pageTitle="Azure Active Directory juurdepääsu tehniline teave | Microsoft Azure'i"
    description="Azure Active Directory kontrollib juurdepääsu kontrolli, saate valida, kui kasutaja autentimist ja enne Accessi rakendusse eritingimused. Kui nendest tingimustest on täidetud, autenditud kasutaja ja rakenduse juurdepääs lubatud."
    services="active-directory"
    documentationCenter=""
    authors="MarkusVi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
    ms.date="10/20/2016"
    ms.author="markvi"/>

# <a name="azure-active-directory-conditional-access-technical-reference"></a>Azure Active Directory juurdepääsu tehniline teave

## <a name="services-enabled-with-conditional-access"></a>Tingimusvormingu juurdepääsuga lubatud teenused
Tingimusvormingu juurdepääsureeglid on toetatud üle erinevate Azure AD rakendus. See loend sisaldab.

- Ühendatud rakenduste Azure AD Rakenduse galeriist
- Parooli SSO rakenduste Azure AD Rakenduse galeriist
- Rakenduste registreeritud Azure'i rakenduse puhverserver
- Business ja registreeritud Azure AD rentniku mitme rakenduste arendatud rida
- Visual Studio võrgus
- Azure'i kaugjuhtimise rakenduses
-   Dynamics CRM-iga
- Microsoft Office 365 Yammeri
- Microsoft Office 365 Exchange Online'is
- Microsoft Office 365 SharePoint Online'i (sh OneDrive for Business)


## <a name="enable-access-rules"></a>Luba juurdepääs reeglid

Iga reegli puhul saate lubada või keelata, klõpsake soovitud rakenduse alused kohta. Kui reeglid jagunevad **ON** siis need lubatud ja jõustatud kasutajate juurdepääs rakenduse jaoks. Kui nad on **väljas** ei kasutata ja kasutajate sisselogimist kogemus ei mõjuta.

## <a name="applying-rules-to-specific-users"></a>Reeglite rakendamine kindlatele kasutajatele
Reegleid saab rakendada teatud kasutajarühma seadmisega **Rakenduskoht**turberühma põhjal. **Rakenduskoht** saate määrata **Kõigile kasutajatele** või **rühmadele**. Kui seatud **Kõigile** kasutajatele reegleid rakendatakse kõigile kasutajatele juurdepääsu rakendus. **Rühmade** funktsioonis teatud turbe- ja levirühmade valitud reeglid jõustatakse ainult nende rühmade jaoks.

Reegli juurutamisel on levinud esmalt rakendada selle piiratud kasutajate, mis on piloteerimise rühma liikmete määramine. Kui täielik saab reeglit rakendada **Kõigile**kasutajatele. See põhjustab täitmisele kõigi kasutajate organisatsiooni reegel.

Valige rühmad võivad vabastada poliitika **peale** suvandi abil. Kõik need rühma liikmete vabastatakse isegi juhul, kui nad kuvada kaasatud rühma.

## <a name="at-work-networks"></a>Võrgu "tööl"


Juurdepääsu reeglid, mis kasutavad "tööl" võrk, sõltuvad usaldusväärsete IP-aadresside vahemikud, mis on konfigureeritud Azure AD, või kasutada "sees corpnet" nõude AD FS-i kaudu. Järgmised reeglid.

- Nõua mitmikautentimise pole tööl
- Blokeeri juurdepääs pole tööl

Suvandite situatsiooniplaani võrkude "tööl"

1. [Mitmikautentimise konfiguratsiooni lehele](../multi-factor-authentication/multi-factor-authentication-whats-next.md)konfigureerida usaldusväärse IP-aadresside vahemikud. Tingimusvormingu juurdepääsu poliitika kasutatakse iga autentimine taotluse ja Turbeloa emissiooni konfigureeritud vahemikud reegli. 
2. Konfigureerige kasutamine sees corpnet taotlemine, saab see suvand ühendatud kataloogide, AD FS-i abil. [Lisateavet sees coronet taotluste](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).
3. Konfigureerige avaliku IP-aadresside vahemikud. Klõpsake menüü konfigureerimine kataloogi, saate määrata avaliku IP-aadressid. Juurdepääsu kasutage järgmisi, nagu "tööl" IP-aadressid, see võimaldab täiendavad vahemikud konfigureerida üle 50 IP address, mis on MFA sätete lehel.



## <a name="rules-based-on-application-sensitivity"></a>Reeglid, mis põhineb rakenduse tundlikkuse

Reeglite on konfigureeritud taotluse lubamisel teenuste kõrge väärtus oleks tagatud mõjutamata juurdepääsu muude teenuste kohta. Tingimusvormingu juurdepääsureeglid saab konfigureerida rakenduse menüü **konfigureerimine** . 

Reeglid, mida praegu pakutakse:

- **Mitmikautentimise nõudmine**
 - Kõik kasutajad, mis rakendatakse see poliitika on vaja autentida kaudu mitmikautentimise vähemalt ühe korra.
 
- **Nõua mitmikautentimise pole tööl**
 - Kui see poliitika on rakendatud, kõik kasutajad on läbi mitmikautentimise vähemalt üks kord, kui nad juurdepääsu teenuse-töö mujalt vaja. Kui nad liiguvad töö eemal, peavad nad multifactor autentida teenuse kasutamisel.
 
- **Blokeeri juurdepääs pole tööl** 
 - Kui kasutajad liikuda töölt eemal, nad on blokeeritud kui neile on rakendatud poliitika "Blokeeri juurdepääs pole tööl".  Need uuesti lubatud juurdepääs kui töö asukoht.


## <a name="related-topics"></a>Seotud teemad

- [Office 365 ja muude rakenduste juurdepääsu ühendatud Azure Active Directory](active-directory-conditional-access.md)
- [Artikli registri Rakendusehaldus Azure Active Directory 's](active-directory-apps-index.md)
