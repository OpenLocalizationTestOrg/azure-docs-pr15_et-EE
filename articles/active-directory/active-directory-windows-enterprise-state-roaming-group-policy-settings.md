<properties
    pageTitle="Rühma poliitika ja MDM-i sätted | Microsoft Azure'i"
    description="Teave rühmapoliitika ja mobiilsideseadme halduse (MDM) sätteid, mis tuleks kasutada ettevõtte seadmetes. Need poliitikad on rakendatud kasutaja kogu seade."
    services="active-directory"
    keywords="mis on rühma poliitika ja MDM-i sätete Enterprise olekus rändlusteenuse, Enterprise olekus rändlusteenuse, windows cloud"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor="curtand"/>

<tags
    ms.service="active-directory"  
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="group-policy-and-mdm-settings"></a>Rühmapoliitika ja MDM-i sätted

Kasutada nende rühmapoliitika ja mobiilsideseadme halduse (MDM-i) sätted ainult ettevõtte seadmetes kuna need poliitikad rakendatakse kogu kasutaja seade. Isikliku sätete sünkroonimise keelamine MDM-i poliitika rakendamist kasutaja seade seadme kasutuse negatiivset mõju. Lisaks saavad kasutajakontodele seade mõjutatud poliitika.

Ettevõtted, kes soovite hallata rändlusteenuse isikliku (mittehallatava) seadmete jaoks saate kasutada Azure portaali lubamiseks või keelamiseks rändlusteenuse, mitte rühmapoliitika või MDM
Järgmises tabelis on kirjeldatud rühmapoliitika sätted saadaval.

## <a name="mdm-settings"></a>MDM-i sätted
MDM-i rühmapoliitika sätted rakenduvad nii Windows 10 ja Windows 10 Mobile.  Windows 10 Mobile tugi on olemas ainult Microsofti konto vastavalt rändlusteenuse kasutaja OneDrive'i konto kaudu.  Lugege lõiku "Seadmed ja lõpp-punktid" kohta, millised seadmed on toetatud vastavalt Azure AD sünkroonimine.

| Nimi                               | Kirjeldus                                                          |
|------------------------------------|----------------------------------------------------------------------|
| Microsofti kontoga ühenduse lubamine | Võimaldab kasutajatel autentida Microsofti konto seadme abil |
| Luba Sünkrooni minu sätted             | Võimaldab kasutajatel ringi liikuda Windowsi sätteid ja rakenduste andmeid; Selle poliitika keelamise keelata sünkroonimine kui ka varukoopiate mobiilsideseadmete jaoks                  |

## <a name="group-policy-settings"></a>Rühmapoliitika sätted
Rühmapoliitika sätted rakenduvad Windows 10 seadmetes, mis on ühendatud Active Directory domeenis. Tabel sisaldab ka pärand sätted mis näib sünkroonimissätete haldamine, kuid see ei tööta Enterprise olekus rändlusteenuse for Windows 10, mis märkis 'Kasuta' kirjeldus.

| Nimi                                | Kirjeldus |
|-------------------------------------|-------------|
| Kontod: Blokeeri Microsofti kontod  |See säte ei saa kasutajad uue Microsofti kontode lisamise selles arvutis|
| Sünkroonimine                         |Ei saa kasutajad ringi liikuda Windowsi sätted ja rakenduste andmed|
| Sünkroonimine isikupärastamine             |Keelatakse sünkroonimist jaotise kujundused|
| Sünkroonimine veebibrauseri sätted        |Internet Exploreri rühma sünkroonimine keelatakse|
| Sünkroonimine paroolid               |Keelatakse sünkroonimise paroolide rühma|
| Sünkroonige teiste Windowsi sätted  |Muud Windowsi sätted jaotises sünkroonimist keelatakse|
| Sünkroonimine Töölaua isikupärastamine |Ärge kasutage; ei mõjuta|
| Mahupõhiste ühenduste ei sünkroonita  |Keelatakse rändluse Mahupõhised ühendused, nt mobiilside 3 G|
| Sünkroonimine rakendused                    |Ärge kasutage; ei mõjuta|
|Sünkroonige rakenduse sätted             |Keelatakse rändlusteenuse rakenduse andmeid.|
|Sünkroonimine käivitamise sätted           |Ärge kasutage; ei mõjuta|


## <a name="related-topics"></a>Seotud teemad
- [Ettevõtte olekus rändlusteenuse ülevaade](active-directory-windows-enterprise-state-roaming-overview.md)
- [Lubage enterprise olekus rändlusteenuse Azure Active Directory 's](active-directory-windows-enterprise-state-roaming-enable.md)
- [Sätted ja andmete rändluse FAQ](active-directory-windows-enterprise-state-roaming-faqs.md)
- [Windows 10 rändluse sätted viited](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
