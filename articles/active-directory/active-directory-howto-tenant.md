<properties
    pageTitle="Kuidas saada on Azure AD rentniku | Microsoft Azure'i"
    description="Kuidas saada on Azure Active Directory rentnik registreerimiseks ja rakenduste loomine."
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="terrylan"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/28/2015"
    ms.author="dastrock"/>

# <a name="how-to-get-an-azure-active-directory-tenant"></a>Kuidas saada rentnikku Azure Active Directory

Azure Active Directory (Azure AD), on [rentniku](https://msdn.microsoft.com/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant) organisatsiooni esindaja.  See on spetsiaalne eksemplari Azure AD teenus, mis ettevõtte saab omanik, kui see registreerub Microsofti pilveteenuses, nt Azure, Microsoft Intune'i või Office 365.  Iga Azure AD rentniku on erinevad ja muude Azure AD rentnike jaoks eraldi.  

Rentniku maja ettevõtte kasutajate ja teavet nende - paroolide, kasutajaprofiili andmete, õiguste ja jne.  See sisaldab ka rühmad, rakenduste ja muud teavet ettevõtte ja selle Turvalisus.

Azure AD kasutajatel teie rakendusse sisse logida, peate oma rentniku registreerima rakenduse.  Avaldamise Azure AD rentniku rakendus on **täiesti tasuta**.  Tegelikult enamik arendajate loob mitu rentnikke ja rakenduste katsetamiseks arengu, lavastus ja katsetamiseks.  Ettevõtted, mis registreeruda ja tarbimine rakenduse soovi korral saate osta litsentse, kui soovite täpsemalt kataloogi funktsioonide eeliseid.

Jah, kuidas lähete kohta, kuidas mõni Azure AD rentniku?  Protsess võib olla veidi erinevaid kui saate:

- [On olemasoleva Office 365 tellimus](#use-an-existing-office-365-subscription)
- [On seotud Account Microsoft Azure'i olemasolevale tellimusele](#use-an-msa-azure-subscription)
- [On seostatud organisatsioonikonto Azure olemasolevale tellimusele](#use-an-organizational-azure-subscription)
- [Ükski eespool on ja soovite alustada nullist](#start-from-scratch)

## <a name="use-an-existing-office-365-subscription"></a>Kasutage olemasolevasse Office 365 tellimusse
Kui teil on olemasoleva Office 365 tellimus, on teil juba on Azure AD rentniku! Saate oma O365 kontoga [Azure portaali](https://portal.azure.com) sisse logida ja Azure AD kasutamise alustamine.

## <a name="use-an-msa-azure-subscription"></a>Tellimuse MSA Azure kasutamine
Kui teil on varem registreerunud Azure tellimuse Microsoft Account, on juba rentniku!  Kui logite [Azure portaali](https://portal.azure.com), saate automaatselt logitakse oma rentniku vaikimisi. Olete selle rentniku kasutada vastavalt vajadusele -, kuid soovite ettevõtte administraatori konto luua.

Selleks tehke järgmist.  Teise võimalusena võite luua uue rentniku ja luua administraator selle rentniku pärast sarnast.

1.  Arve [Azure portaali](https://portal.azure.com) sisse logida
2.  Liikuge jaotises "Azure Active Directory" portaali (leitud vasakpoolse navigeerimisriba jaotises **Rohkem teenuseid**)
3.  Peate tuleks automaatselt sisse logitud kataloogiga"vaikimisi", kui mitte kataloogide saab vahetada, kui klõpsate paremas ülanurgas oma konto nime.
4.  **Kiirülevaate tööülesannete** jaotis, valige käsk **Lisa kasutaja**.
5.  Lisage kasutaja vorm pakuvad järgmisi üksikasju.

    - Nimi: (valige sobiv väärtus)
    - Kasutajanimi: (valige administraator selle kasutaja nimi)
    - Profiil: (eesnimi, viimase nimi, ametinimetus ja osakonna vastav väärtuste sisestamiseks)
    - Ülesanded: Globaalne administraator

6.  Kui kasutaja lisamine vormi täitnud ja vastu võtta ajutine parool uus administraatoriõigustega kasutaja, kindlasti salvestada see parool on vaja sisse logida selle uue kasutaja parooli muutmiseks. Lisaks saate saata otse kasutajale, kasutades alternatiivsete e-posti parool.
7.  **Loo** uus kasutaja loomiseks klõpsake nuppu.
8.  Ajutise parooli muutmiseks logige [https://login.microsoftonline.com](https://login.microsoftonline.com) koos selle uue kasutajakonto ja nõudmisel parooli muutmine.


## <a name="use-an-organizational-azure-subscription"></a>Ettevõtte Azure tellimuse kasutamine
Kui teil on varem registreerunud Azure tellimuse ettevõtte konto, on teil juba rentniku!  [Azure portaali](https://portal.azure.com), siis tuleb leida rentniku kui liigute "Veel teenused" ja "Azure Active Directory."  Olete selle rentniku kasutada vastavalt vajadusele. 


## <a name="start-from-scratch"></a>Alustamiseks algusest peale
Kui kõik ülaltoodud on teile plära, ärge muretsege.  Lihtsalt külastage [https://account.windowsazure.com/organization](https://account.windowsazure.com/organization) Azure uue organisatsiooni kasutajaks.  Kui olete selle protsessi lõpule jõudnud, on teil teie enda Azure AD rentniku teie valitud ajal domeeni nimi.  [Azure portaali](https://portal.azure.com), leiate oma rentniku vasakul puhasväärtuse "Azure Active Directory" liikumine

Osana protsessi Azure'i kasutajaks, tuleb teil esitada krediitkaardi üksikasju.  Saate jätkata confidence – te ei võeta avaldamise rakenduste Azure AD või luua uue rentnikke.
