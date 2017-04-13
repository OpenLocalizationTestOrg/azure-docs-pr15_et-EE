<properties
    pageTitle="Tingimusvormingu juurdepääsupoliitikaid seadme Office 365 teenuste jaoks | Microsoft Azure'i"
    description="Kuidas seadme vastavalt tingimuste üksikasjad reguleerida juurdepääsu teenusekomplektile Office 365 teenuste. Teabetöötajad (IWs), millele soovite juurdepääsu Office 365 teenuste, nt Exchange ja SharePoint Online'i tööl või koolis oma isikliku seadmest, IT-administraator soovib secure.IT administraatoritel on juurdepääs sätte tingimusvormingu juurdepääsu seadme poliitika secure ettevõtte ressursid, samal ajal võimaldab IWs nõuetele vastavuse seadmes juurdepääsu teenustele."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>
# <a name="conditional-access-device-policies-for-office-365-services"></a>Tingimusvormingu juurdepääsupoliitikaid seadme Office 365 teenuste jaoks

Termini, "tingimuslik juurdepääs" on palju tingimusi, nt mitme teguri autenditud kasutaja, autenditud seostatud seade, ühilduv seadme jne. Selles teemas peamiselt keskendutakse Office 365 teenuste juurdepääsu piiramiseks seade põhinevad tingimused. Kui Teabetöötajad (IWs) soovite juurdepääsu Office 365 teenuste nagu Exchange ja SharePoint Online'i tööl või koolis oma isikliku seadmeid, IT-administraator soovib turvaline juurdepääs. IT-administraatorid saavad ettevalmistamise tingimusvormingu juurdepääsupoliitikaid seadme ettevõtte ressursid, samal ajal lubamisel IWs nõuetele vastavuse seadmes juurdepääsu tagamiseks. Teenusekomplekti Office 365 juurdepääsu poliitikaid saab konfigureerida Microsoft Intune'i juurdepääsu portaalist.

Azure Active Directory jõustab turvaline juurdepääs Office 365 teenustele juurdepääsu poliitika. Rentniku administraator, saate luua juurdepääsu poliitika, blokeerib kasutaja juurdepääsu teenuse O365 mittevastavate seadmes. Kasutaja peab vastama ettevõtte seadme poliitika enne Accessi teenusega. Vaheldumisi, saate luua admin poliitika, mis nõuab kasutajate registreerimine just oma seadmetest juurde pääseda teenuse O365. Poliitikate võib rakendatakse kõigile kasutajatele ettevõttes, või ainult mõne sihtrühmade ja täiustatud aja jooksul täiendavate sihtrühmade kaasata.

Nõutav seadme poliitikate jõustamine on kasutajatele Azure Active Directory seadme registreerimine teenusega oma seadmed registreerida. Saate valida, kas lubada mitmikautentimise (MFA), registreerimise seadmed Azure Active Directory seadme registreerimine teenusega. MFA on soovitatav Azure Active Directory seadme registreerimine teenus. MFA lubamisel puutuvad teine tegur autentimise registreerimisel tema seadmetes Azure Active Directory seadme registreerimine teenuse kasutajad.

##<a name="how-does-conditional-access-policy-work"></a>Juurdepääsu poliitika tööpõhimõtted

Kui mõne kasutaja taotleb juurdepääsu O365 teenus toetatud seadme platvormi kaudu, Azure Active Directory autendib kasutaja ja seadme, kust kasutaja käivitab taotluse; ja toetused juurdepääsu teenuse ainult siis, kui kasutaja vastab poliitika määramine teenuse kohta. Kasutajad, kes on registreerunud seadmes on esitatud parandavat juhiseid kohta, kuidas registreerida ja saada nõuetele vastavuse ettevõtte O365 teenustele juurdepääsuks. IOS-i ja Androidi seadmete kasutajad peavad oma seadet ettevõtteportaali rakenduse abil. Kui kasutaja enrolls oma seade, seade on registreeritud Azure Active Directory ja registreerunud seadmehalduse ja nõuetele vastavus. Klientide peate kasutama Azure Active Directory seadme registreerimine teenuse Microsoft Intune'i koos mobiilsideseadmete haldus Office 365 teenuse lubamiseks. Seadme registreerimine on kasutajatel juurdepääs Office 365 teenuste eelduseks, kui seadme poliitikate jõustamine.

Kui kasutaja enrolls oma seadme edukalt, muutub usaldusväärsete. Azure Active Directory pakub ühekordse sisselogimise juurdepääsu ettevõtte rakendustele ja jõustab juurdepääsu poliitika juurdepääsu teenuse mitte ainult esimese kuupäeva ja kellaaja kasutaja taotleb juurdepääsu, kuid iga kord, kui kasutaja taotleb pikendamiseks juurdepääsu andmiseks. Kasutaja keelatakse juurdepääs teenustele muudetakse sisselogimismandaati, seade on kadunud/varastatud või poliitika ei ole täidetud taotluse uuendamise ajal.

## <a name="deployment-considerations"></a>Juurutamise asjaoluga:
Peate kasutama Azure Active Directory seadme registreerimine teenuse seadme registreerimiseks.

Kui kasutajad on kohapealne autentida, Active Directory Federation Services (AD FS) (1,0 ja ülaltoodud) on nõutav. Mitmikautentimise jaoks töökoha liitumine ei õnnestu, kui identiteedipakkuja ei mitmikautentimise. Näiteks AD FS 2.0 pole mitme teguri autentimise-ühenduse võimalusega. Rentnikuadministraatori tuleb tagada, et asutusesisene AD FS-i on MFA toega ja kehtiv MFA meetod on lubatud, enne MFA teenuses Azure Active Directory seadme registreerimine. Näiteks Windows Server 2012 R2 AD FS-i on MFA võimalused. Enne MFA teenuses Azure Active Directory seadme registreerimine, tuleb lubada AD FS server sisestusmeetod täiendavad kehtiv autentimine (MFA). Toetatud MFA meetodit AD FS-i kohta leiate lisateavet teemast täiendavad autentimismeetodite konfigureerimine AD FS-i.

## <a name="frequently-asked-questions-faq"></a>Korduma kippuvad küsimused (KKK)

Q: mis on välistamist vaikepoliitika seade pole toetatud platvormid?

V: praegu juurdepääsu poliitikate jõustamine valikuliselt iOS-i ja Androidi seadmete kasutajad. Muude platvormide jaoks loodud seadme rakenduste on vaikimisi ei mõjuta juurdepääsu poliitika iOS-i ja Androidi seadmete jaoks. Rentnikuadministraatori võite siiski alistamiseks globaalne poliitika pole toetatud platvormid kasutajad juurdepääsu keelamiseks.
See on teejuht laiendada juurdepääsu poliitika muude platvormide jaoks loodud, sh Windowsi kasutajad.

Q: kui laiendatakse juurdepääsu poliitika Office 365 teenustega veebipõhine rakenduste (nt OWA, Brauseripõhine SharePointi).

V: praegu tingimusvormingu juurdepääsu Office 365 teenused on piiratud rikkaliku rakenduste seadmes. See on juhised juurdepääsu poliitika kasutajad, kes kasutavad teenuseid brauseritest laiendada.
