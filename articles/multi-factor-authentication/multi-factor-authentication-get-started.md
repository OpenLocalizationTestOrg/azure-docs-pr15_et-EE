<properties
    pageTitle="Azure'i MFA cloud vs server | Microsoft Azure'i"
    description="Valige soovitud mitmikautentimise secutiry lahenduse, mis on õige paludes mida ma proovite turvaline ja kus on minu kasutaja asub.  Valige pilves, MFA Server ja AD FS-i."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/14/2016"
    ms.author="kgremban"/>

#<a name="choose-the-azure-multi-factor-authentication-solution-for-you"></a>Valige lahenduse Azure'i Mitmikautentimise jaoks

Kuna on mitmeid maitsed Azure'i mitme teguriga autentimine (MFA), saame peab vastama mõnele küsimusele aru saada, milline versioon on õige kasutada.  Need küsimused on:

-   [Mida ma püüdes turvaline](#what-am-i-trying-to-secure)
-   [Kus asuvad kasutajad](#where-are-the-users-located)
- [Milliseid funktsioone vaja?](#what-featured-do-i-need)

Järgmised jaotised sisaldavad juhiseid iga nende vastused määramise kohta.

## <a name="what-am-i-trying-to-secure"></a>Mida ma püüdes turvaline?

Õige kaheastmelise kontrollimise lahenduse määratlemiseks esmalt me peab küsimusele, mida te proovite turvaline autentimine teise meetodi abil.  See on rakendus, mis on Azure?  Või Kaugpöördusteenuse süsteemi?  Määrates, mida me üritavad turvaline, saame vastata küsimus, kus Mitmikautentimise peab olema lubatud.  


Mis on proovite turvaline| Mitmikautentimise pilveteenuses|Mitmikautentimise Server
------------- | :-------------: | :-------------: |
Esimese Microsofti rakendused|● |● |
SaaS rakendused rakendus Galerii|● |● |
IIS-i rakenduste avaldatud Azure AD Rakenduse puhverserveri kaudu|● |● |
IIS-i rakenduste pole avaldatud Azure AD Rakenduse puhverserveri kaudu | |● |
Näiteks VPN, lugemisel Kaugpöördusteenuse| |● |



## <a name="where-are-the-users-located"></a>Kus asuvad kasutajad

Järgmiseks vaadates meie kasutajatele asukohaks aitab määratleda õige lahendus kasutada cloud või kohapealse MFA serveri kasutamine.



Kasutaja asukoht| Mitmikautentimise pilveteenuses|Mitmikautentimise Server
------------- | :-------------: | :-------------: |
Azure Active Directory|● | |
Azure AD ja kohapealsete AD federation AD FS-i abil|● |● |
Azure AD ja kohapealse AD DirSync Azure AD sünkroonimine, Azure'i AD-ühenduse - pole parooli sünkroonimise kasutamine|● |● |
Azure AD ja kohapealsete AD funktsiooniga DirSync Azure AD sünkroonimine, Azure'i AD-ühenduse - parooli sünkroonimise|● | |
Kohapealse Active Directory| |● |

## <a name="what-features-do-i-need"></a>Milliseid funktsioone vaja?

Järgmises tabelis on võrdlus funktsioonid, mis on saadaval koos Mitmikautentimise pilves ja mitmekordne autentimine serveriga.

 | Mitmikautentimise pilveteenuses | Mitmikautentimise Server
------------- | :-------------: | :-------------: |
Kui teine mobiilirakenduse teatis | ● | ● |
Kui teine mobiilirakenduse kinnituskood | ● | ●
Kui teine telefonikõne | ● | ●
Teine ühesuunalise SMS-i | ● | ●
Kahesuunalise SMS-i teine |  | ●
Kui teine riistvara sõned |  | ●
Rakenduse paroolid klientidele, mis ei toeta MFA | ● |  
Administraator juhtida autentimismeetodite | ● | ●
PIN-koodi režiim |  | ●
Pettuse teatis | ● | ●
MFA aruanded | ● | ●
Ühekordse Meediumierandi |  | ●
Kohandatud tervitust telefonikõnede | ● | ●
Telefonikõnede kohandatavad helistaja ID | ● | ●
Usaldusväärsete IP-d | ● | ●
Pidage meeles MFA usaldusväärsete seadmete jaoks  | ● |  
Tingimusvormingu juurdepääs | ● | ●
Vahemälu |  | ●

Nüüd, kui me kindel, kas kasutada cloud mitmikautentimise või MFA serveri asutusesisese, alustamiseks saame häälestamise ja kasutamise Azure'i Mitmikautentimise. **Valige ikoon, mis tähistab stsenaariumist!**

<center>




[![Cloud](./media/multi-factor-authentication-get-started/cloud2.png)](multi-factor-authentication-get-started-cloud.md)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[![Proofup](./media/multi-factor-authentication-get-started/server2.png)](multi-factor-authentication-get-started-server.md)  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</center>
