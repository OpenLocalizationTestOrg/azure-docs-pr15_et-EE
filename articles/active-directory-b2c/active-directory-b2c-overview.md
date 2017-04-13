<properties
    pageTitle="Azure Active Directory B2C: Ülevaade | Microsoft Azure'i"
    description="Azure Active Directory B2C tarbija suunatud rakenduste arendamise"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-sign-up-and-sign-in-consumers-in-your-applications"></a>Azure Active Directory B2C: Registreeruda kursustele ja logige sisse oma rakendused tarbijad

Azure Active Directory B2C on täielik pilve identiteedi haldamise lahendus tarbija suunatud veebi-ja mobiilirakendused. See on väga kättesaadav globaalne teenus, mis sadu miljoneid tarbija identiteedid skaala. Azure Active Directory B2C ehitatud on turvalise äriklassi platvormi, hoiab rakenduste, teie ettevõte ja kaitstud tarbijatele.

Varem oleks rakenduste arendajatele, kes soovivad registreeruda kursustele ja logige sisse oma rakendustesse tarbijad kirjutanud oma kood. Ja need oleks kasutatud kohapealse andmebaasi või süsteemi talletada kasutajanimed ja paroolid. Azure Active Directory B2C pakub arendajatele paremini integreerida nende rakenduste abil turvaline, standardid platvormi ja suurel hulgal erinevaid laiendatav poliitikate tarbija identiteetide haldus. Kui kasutate Azure Active Directory B2C, tarbijatele saavad registreeruda rakenduste abil oma olemasoleva suhtlusvõrgu kontod (Facebooki, Google, Amazon, LinkedIni) või luua uue mandaadi (meiliaadressi ja parooli või kasutajanime ja parooli); Me kutsume viimase "kohalikud kontod."

## <a name="get-started"></a>Alustamine

Koostamiseks rakendus, mida aktsepteerib tarbija välja logida ja logige sisse, kuvatakse rakenduse esimese registreeruda ja Azure Active Directory B2C rentniku. Saada oma rentniku abil [loomine mõne Azure AD B2C rentniku](active-directory-b2c-get-started.md)kirjeldatud juhised.

Rakenduse Azure Active Directory B2C teenuse vastu, saate kirjutada protokoll sõnumite saatmiseks otse [OAuth 2.0](active-directory-b2c-reference-protocols.md#oauth2-authorization-code-flow) või [Avatud ID ühenduse](active-directory-b2c-reference-protocols.md#openid-connect-sign-in-flow), valides või meie teekide abil ei tööta teie jaoks. Valige oma lemmik platvormi järgmises tabelis ja saategi alustada.

[AZURE.INCLUDE [active-directory-b2c-quickstart-table](../../includes/active-directory-b2c-quickstart-table.md)]

## <a name="whats-new"></a>Mis on uut

Siit tagasi sageli õppida Azure Active Directory B2C tulevaste muudatuste kohta. Meil kuvatakse ka piiksatus värskendusi, kasutades @AzureAD.

- Lugege meie [laiendatav raamistik](active-directory-b2c-reference-policies.md) ning tüüpi poliitikad, mida saate luua ja kasutada rakenduste kohta.
- Meie [teenuse ajaveebi](https://blogs.msdn.microsoft.com/azureadb2c/) teatised vaheversioonid teenuste probleemid, värskenduste, olek ja kergendamise järjehoidja. Jätkake [Azure oleku armatuurlaua](https://azure.microsoft.com/status/) ka jälgimiseks.
- Praeguse [teenusepiirangud, piiranguid ja piiranguid](active-directory-b2c-limitations.md).
- Lõpuks [proovi kood](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect-aspnetcore-b2c) Azure AD B2C ja Core ASP.net-i abil.

## <a name="how-to-articles"></a>Juhendid

Saate teada, kuidas kasutada Azure Active Directory B2C funktsioonid:

- [Facebooki](active-directory-b2c-setup-fb-app.md), [Google +](active-directory-b2c-setup-goog-app.md), [Microsofti konto](active-directory-b2c-setup-msa-app.md), [Amazon](active-directory-b2c-setup-amzn-app.md)ja [LinkedIni](active-directory-b2c-setup-li-app.md) kontode kasutamiseks konfigureerida oma kliendile suunatud rakendused.
- [Kasutage kohandatud atribuute tarbijatele kohta teavet koguda](active-directory-b2c-reference-custom-attr.md).
- [Luba Azure'i Mitmikautentimise oma kliendile suunatud rakendused](active-directory-b2c-reference-mfa.md).
- [Iseteeninduslik parooli lähtestamine tarbijatele häälestamine](active-directory-b2c-reference-sspr.md).
- [Ilme ja logige sisse logida ja muude tarbija suunatud lehtede kohandamine](active-directory-b2c-reference-ui-customization.md) , mis on Azure Active Directory B2C kätte.
- Oma Azure Active Directory B2C rentnik [kasutamine Azure Active Directory Graph API programmiliselt loomine, lugemine, värskendamine, ja kustutada tarbijad](active-directory-b2c-devquickstarts-graph-dotnet.md) .

## <a name="next-steps"></a>Järgmised sammud

Teenuse süvitsi uurida on kasu järgmisi linke:

- Teemast [Azure Active Directory B2C hinnateavet](https://azure.microsoft.com/pricing/details/active-directory-b2c/).
- Hankida abi virnas ületäitumise [Azure'i – active directory](http://stackoverflow.com/questions/tagged/azure-active-directory) abil või [adal](http://stackoverflow.com/questions/tagged/adal) sildid.
- Andke meile oma mõtete [Kasutaja hääle](https://feedback.azure.com/forums/169401-azure-active-directory/)abil – Soovime kuulda neid! Kasutage teemas on fraas "AzureADB2C:" nii, et me ei leia seda postituse pealkirja.
- Lugege läbi [viite Azure AD B2C Protocol (protokoll)](active-directory-b2c-reference-protocols.md).
- Vaadake üle [Azure AD B2C Turbeloa viide](active-directory-b2c-reference-tokens.md).
- Lugege [Azure Active Directory B2C korduma kippuvad küsimused](active-directory-b2c-faqs.md).
- [Faili tugi Azure Active Directory B2C taotlused](active-directory-b2c-support.md).

## <a name="get-security-updates-for-our-products"></a>Meie toodete värskendusi turvalisus

Soovitame teil saada teatisi, kui turvalisus ilmnevad külastades [selle lehe](https://technet.microsoft.com/security/dd252948) ja turbeteatised nõu tellimise.
