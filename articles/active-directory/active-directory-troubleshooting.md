<properties
   pageTitle="Tõrkeotsing: 'Active Directory' üksus on puudu või pole saadaval | Microsoft Azure'i "
   description="Mida teha, kui haldusportaali Azure Active Directory menüükäsk ei kuvata."
   services="active-directory"
   documentationCenter="na"
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin"/>

# <a name="troubleshooting-active-directory-item-is-missing-or-not-available"></a>Tõrkeotsing: 'Active Directory' üksus on puudu või pole saadaval

Paljude juhiseid Azure Active Directory funktsioone ja teenuseid, alustage "Minge Azure'i haldusportaal ja klõpsake käsku **Active Directory**." Mida teha, kui Active Directory laiendi või menüü üksust ei kuvata, või kui see on märgitud, **Pole saadaval**, kuid? Selles teemas on loodud. See kirjeldab mis **Active Directory** ei kuvata või pole saadaval ja selgitatakse, kuidas jätkata.

## <a name="active-directory-is-missing"></a>Active Directory on puudu

Tavaliselt **Active Directory** üksuse kuvatakse vasakpoolses navigeerimismenüüs. Azure Active Directory toimingute juhiseid Oletame, et see on teie arvates.

![Kuvatõmmis: Azure Active Directory](./media/active-directory-troubleshooting/typical-view.png)

Active Directory üksuse kuvatakse vasakpoolses navigeerimismenüüs, kui mõni järgmistest tingimustest on täidetud. Muul juhul üksust ei kuvata.

* Praeguse kasutaja loginud Microsofti kontoga (varem Windows Live ID).

    VÕI

* Azure'i rentniku on kataloog ja praeguse konto on directory administraator.

    VÕI

* Azure'i rentniku on vähemalt üks Azure AD juurdepääsu reguleerimine (ACS) nimeruumi. Lisateavet leiate teemast [Juurdepääsu juhtimine Namespace](https://msdn.microsoft.com/library/azure/gg185908.aspx).

    VÕI

* Azure'i rentniku on vähemalt üks Azure'i Mitmikautentimise pakkuja. Lisateabe saamiseks vt [Haldamise Azure'i mitmekordne autentimisteenuse pakkujate](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).

Mõne juurdepääsu reguleerimine nimeruumi või Mitmikautentimise pakkuja loomiseks valige **+ Uus** > **Rakenduse teenuste** > **Active Directory**.

Leida administraatoriõigusi kataloogiga, on kontole administraatorirolli määrata administraator. Lisateavet leiate teemast [administraatorirollide](active-directory-assign-admin-roles.md).

## <a name="active-directory-is-not-available"></a>Active Directory pole saadaval

Kui klõpsate nuppu **+ Uus** > **Rakenduse teenused**, **Active Directory** üksus kuvatakse. Täpsemalt Active Directory üksuse kuvatakse, kui mõni Active Directory funktsioone, näiteks Directory, juurdepääsu reguleerimine või mitme teguri Auth pakkuja juures on praeguse kasutajale saadaval.

Kuid lehe laadimisel üksus on tuhmid ja on märgitud **Pole saadaval**. See on ajutine olek. Kui te oodake paar minutit, muutub kättesaadavaks üksus. Viivitus on pikaajaline, veebilehe sageli värskendamine probleemi lahendab.

![Kuvatõmmis: Active Directory pole saadaval](./media/active-directory-troubleshooting/not-available.png)
