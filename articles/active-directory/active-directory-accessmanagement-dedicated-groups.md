<properties
    pageTitle="Eesmärk on jätkuvalt rühmade Azure Active Directory | Microsoft Azure'i"
    description="Ülevaade sellest, kuidas sihtotstarbeline rühmade töö Azure Active Directory ja loomist."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""
    />

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="curtand"/>

# <a name="dedicated-groups-in-azure-active-directory"></a>Azure Active Directory sihtotstarbeline rühmade

Azure Active Directory (Azure AD), funktsiooni sihtotstarbeline rühmad automaatselt loob ja kuvab Azure AD eelmääratletud rühmade liikmelisuse. Sihtotstarbeline rühma liikmete ei saa lisada või eemaldada Azure klassikaline portaali, Windows PowerShelli cmdlet-käskude abil või programmiliselt.

>[AZURE.NOTE] Sihtotstarbeline rühmad nõua, et on Azure AD Premium litsents on määratud
>- administraator, kes haldab reegli rühmale
>- Kõik kasutajad, kes on valitud reegli rühma liige

**Lubamiseks sihtotstarbeline rühmad**

1. [Azure'i klassikaline portaalis](https://manage.windowsazure.com)valige **Active Directory**ja avage ettevõttekataloogile.

2. Valige vahekaart **rühmad** ja seejärel avage jaotis, mida soovite redigeerida.

3. Valige vahekaart **konfigureerimine** ja määrake **Lubamine spetsiaalne rühmade** väärtuseks **Jah**.

Kui parameetrit lubamine sihtotstarbeline rühmad on seatud **Jah**, saate lubada täpsemaks kataloogi loomiseks automaatselt kõigile kasutajatele spetsiaalne rühm, seades selle **lubamine "kõik" rühma** minna **Jah**. Saate seejärel redigeerida ka selle sihtotstarbeline rühma nime, tippides selle sisse selle **kuvatav nimi "Kõik kasutajad" rühma** välja.

Kõigi kasutajate rühma saab kasutada samu õigusi määrata kõigi kasutajate kataloogi. Näiteks saate anda kõigi kasutajate directory juurdepääsu SaaS rakendus, määrates selle rakenduse access sihtotstarbeline kõigi kasutajate rühma jaoks.

Spetsiaalne kõigi kasutajate rühma sisaldab kõigi kasutajate kataloogis, sh külalistele ja välised kasutajad. Kui teil on vaja rühma mis välistab väliskasutajad, siis võite saavutada selle rühma luues atribuut põhinev dünaamilise reegli näiteks järgmisi:

                (user.userPrincipalName -notContains "#EXT#@")

Rühma, mis ei hõlma kõik Külalised, kasutage reegli näiteks järgmisi:

                (user.userType -ne "Guest")

Dünaamilise rühma liikmeks *keerukamate* reeglite (reeglid, mis võib sisaldada mitut võrdlemine) loomise kohta leiate teemast [keerukamate reeglite loomiseks kasutamine atribuute](active-directory-accessmanagement-groups-with-advanced-rules.md).


Azure Active Directory esitada lisateavet järgmistest artiklitest.

* [Azure Active Directory levirühmadega ressursid juurdepääsu haldamine](active-directory-manage-groups.md)
* [Artikli registri Rakendusehaldus Azure Active Directory 's](active-directory-apps-index.md)
* [Mis on Azure Active Directory?](active-directory-whatis.md)
* [Teie kohapealse identiteetide integreerimine Azure Active Directory](active-directory-aadconnect.md)
