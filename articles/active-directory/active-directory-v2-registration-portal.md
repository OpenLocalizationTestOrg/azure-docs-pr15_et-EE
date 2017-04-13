<properties
    pageTitle="Rakenduse registreerimine portaali spikriteemad | Microsoft Azure'i"
    description="Kirjeldus Microsofti rakenduse registreerimise portaalis erinevaid funktsioone."
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>

# <a name="app-registration-reference"></a>Viite rakendus registreerimine
Selles dokumendis on toodud konteksti ja kirjeldused Microsoft rakenduse registreerimise portaali [https://apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)erinevaid funktsioone.

## <a name="my-applications"></a>Minu rakendused
See loend sisaldab kõiki rakenduste Azure AD v2.0 lõpp-punkti jaoks registreeritud.  Need rakendused on võimalus nii isiklikud kontod Microsofti konto ja töö/kool kontod Azure Active Directory kasutajad sisse logida.  Azure AD v2.0 lõpp-punkti kohta leiate lisateavet teemast meie [v2.0 ülevaade](active-directory-appmodel-v2-overview.md).  Need rakendused saate kasutada ka Microsofti konto autentimine lõpp-punkti, integreerida `https://login.live.com`.

## <a name="live-sdk-applications"></a>Reaalajas SDK rakendused
See loend sisaldab kõiki rakenduste kasutamiseks üksnes Microsofti kontoga registreeritud.  Need pole lubatud Azure Active Directory üldse kasutamiseks.  See on, kust leiate kõik rakendused, et varem registreeritud koos MSA arendaja portaali aadressil `https://account.live.com/developers/applications`.  Kõik funktsioonid, mida tegite eelnevalt veebisaidil `https://account.live.com/developers/applications` toimub nüüd selle uue portaali `https://apps.dev.microsoft.com`.  Kui teil on veel küsimusi Microsofti konto rakenduste kohta, võtke meiega ühendust.

## <a name="application-secrets"></a>Rakenduse saladusi
Rakenduse saladusi on rakenduse usaldusväärne [Kliendi autentimine](http://tools.ietf.org/html/rfc6749#section-2.3) Azure AD üksust.  OAuthi ja OpenID ühendus, kuvatakse rakenduse saladusi sagedamini nimetatakse ka `client_secret`.  V2.0 protokollis, mis tahes rakendus, mis saab Turbeloa, web adresseeritavad asukohas (abil soovitud `https` värviskeemi) tuleb kasutada ka rakenduse salajane enda Azure AD, et Turbeloa tagasivõtmisel tuvastamiseks.  Lisaks mis tahes omakliendi seda saab märkide kohta seade on keelatud kasutada ka rakenduse salajane kliendi autentida tõkestamiseks talletamist saladusi ebaturvalise keskkonnas.

Konkreetse rakenduse võib sisaldada kaks lubatud rakendus saladused mis tahes ajal.  Kahe saladusi säilitades teil ablilty perioodilise võtme edastamiseks täitmiseks üle rakenduse kogu keskkonnas.  Kui on viiakse kogu oma rakenduse uus salajane, võite kustutage vana salajane ja uus säte.

Sel ajal, on lubatud ainult kahte tüüpi rakenduse saladusi rakenduse registreerimise portaalis.  Valige **Loo uus parool** luua ja salvestada ühiskasutusega salajane poes vastavaid andmeid, mille abil saate oma rakenduse.  **Luua uue võti paari** valimine loob uue avaliku/era võti paari saab alla laadida ja kasutada kliendi autentimiseks Azure AD.

## <a name="profile"></a>Profiil
Profiili jaotises rakenduse registreerimise portaali saab kohandada rakenduse lehel sisselogimine.  Praegu saate muuta lehe rakenduse logo, teenuse URL-i ja privaatsusavaldus sisselogimine.  Logo peab olema läbipaistva 48 x 48 või 50 × 50 pikslit pildi GIF-, jpg- või PNG-failis, mis on 15 KB või väiksem.  Proovige väärtuste muutmine ja vaatamine tulemuseks logige lehel!

## <a name="live-sdk-support"></a>Reaalajas SDK tugi
Kui lubate "Live SDK tugi", mis tahes rakenduse saladusi loomist kuvatakse arvesse nii Azure AD ette valmistatud ja Microsofti Account andmete poed.  See võimaldab rakenduse integreerida Microsoft Account teenuse (login.live.com).  Kui soovite luua rakenduse Microsoft Account abil otse (erinevalt kasutades Azure AD v2.0 lõpp-punkti), veenduge, et Live SDK tugi on lubatud.

Keelamine Live SDK tugi tagab rakenduse salajane ainult kirjutatakse andmetesse Azure AD talletada.  Azure AD andmed poe sisaldab äriklassi õigus, mis võimaldavad täita teatud standardeid, näiteks FISMA nõuetele vastavus.  Kui lubate Live SDK tugi, ei pruugi rakenduse saavutada mõnede standardeid.

Kui kavatsete ainult kunagi kasutada Azure AD v2.0 lõpp-punkti, saate keelata turvaline Live SDK tugi.

