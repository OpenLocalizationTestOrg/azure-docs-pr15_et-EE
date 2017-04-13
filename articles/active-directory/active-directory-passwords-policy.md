<properties
    pageTitle="Parool poliitika ja Azure Active Directory piiranguid | Microsoft Azure'i"
    description="Kirjeldatakse rakendamine paroolide Azure Active Directory, sh lubatud märkide, pikkus ja aegumise poliitika"
  services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="curtand"/>


# <a name="password-policies-and-restrictions-in-azure-active-directory"></a>Parooli poliitika ja Azure Active Directory piirangud

Selles artiklis kirjeldatakse parool poliitika ja seostatud Kasutajakontod, talletatakse teie Azure AD keerukuse nõuetele.

> [AZURE.IMPORTANT] **Kas olete siin, sest teil on sisselogimisega probleeme?** Kui, siis [siin on, kuidas saate muuta ja enda parool lähtestada](active-directory-passwords-update-your-own-password.md).

## <a name="userprincipalname-policies-that-apply-to-all-user-accounts"></a>UserPrincipalName poliitikad, mis rakenduvad kõik kasutajakontod

Igale kasutajakontole, mis tuleb Azure AD autentimise süsteemi sisselogimiseks peab olema kordumatu kasutaja turvasubjektinimi (UPN) atribuudi väärtus kontoga seotud. Järgmine tabel kirjeldab soovitud poliitikat, mis kehtivad nii kohapealse Active Directory hangitud Kasutajakontod (sünkroonitud pilveteenusesse) ja vaid kasutajakontodele.

|   Atribuut           |     UserPrincipalName nõuded  |
|   ----------------------- |   ----------------------- |
|  Lubatud märgid    |  <ul> <li>A – Z</li> <li>-z </li><li>0 – 9</li> <li> . - \_ ! \# ^ \~</li></ul> |
|  Märgid, mis pole lubatud  | <ul> <li>Mis tahes '@' märk, mis on eraldamise domeeni kasutajanimi.</li> <li>Perioodi märk ei tohi sisaldada "." vahetult enne selle '@' sümbol</li></ul> |
| Pikkus piiranguid  |       <ul> <li>Kogupikkus ei tohi ületada 113 märki</li><li>64 märgist enne selle ‘@’ sümbol</li><li>pärast 48 märgid kuvatakse ‘@’ sümbol</li></ul>

## <a name="password-policies-that-apply-only-to-cloud-user-accounts"></a>Parool poliitika, mis kehtivad ainult cloud Kasutajakontod

Järgmises tabelis kirjeldatakse saadaval parooli rühmapoliitika sätted, mida saab rakendada Kasutajakontod, mis on loodud ja hallatava Azure AD.

|  Atribuut       |    Nõuded          |
|   ----------------------- |   ----------------------- |
|  Lubatud märgid   |   <ul><li>A – Z</li><li>-z </li><li>0 – 9</li> <li>@# $ % ^ & * - _ ! + [{} = & #124; \ : ‘ , . ? / ` ~ “ ( ) ;</li></ul> |
|  Märgid, mis pole lubatud   |       <ul><li>Unicode'i märki</li><li>Tühikuid</li><li> **Keerukate paroolide ainult**: punkti märk ei tohi sisaldada "." vahetult enne selle '@' sümbol</li></ul> |
|   Parooli piirangud | <ul><li>8 märkide minimaalne ja 16 märki</li><li>**Keerukate paroolide ainult**: nõuab 3 välja 4 järgmistest:<ul><li>Väiketähed</li><li>Suurtähed</li><li>Arvud (0 – 9)</li><li>Sümboleid (vt parool piirangud eespool)</li></ul></li></ul> |
| Parooli aegumise kestus      | <ul><li>Vaikimisi väärtus: **90** päeva </li><li>Väärtus on konfigureeritav, kasutades cmdlet Set-MsolPasswordPolicy: on Azure Active Directory moodul Windows PowerShelli jaoks.</li></ul> |
| Parooli aegumise teatise |  <ul><li>Vaikimisi väärtus: **14** päeva (enne parooli aegumise teatise saamist)</li><li>Väärtus on konfigureeritav cmdlet-käsu Set-MsolPasswordPolicy abil.</li></ul> |
| Parooli aegumise |  <ul><li>Vaikimisi väärtus: **false** päeva (näitab, et parooli aegumise on lubatud) </li><li>Kasutajakontode cmdlet-käsu Set-MsolUser abil saate konfigureerida väärtus. </li></ul> |
|  Parooli ajalugu  | Viimase parooli ei saa uuesti kasutada. |
|  Parooli ajaloo kestus | Terve igavik |
|  Konto blokeeringu | Pärast 10 õnnestu sisselogimise katset (vale parool), on kasutaja lukustatud minut. Täpsemaks vale katsete on lock välja kasutaja suurendamiseks kestus. |


## <a name="next-steps"></a>Järgmised sammud

* **Kas olete siin, sest teil on sisselogimisega probleeme?** Kui, siis [siin on, kuidas saate muuta ja enda parool lähtestada](active-directory-passwords-update-your-own-password.md).
* [Igal pool oma paroolide haldamine](active-directory-passwords.md)
* [Kuidas töötab parooli haldus](active-directory-passwords-how-it-works.md)
* [Parooli Mangement töötamise alustamine](active-directory-passwords-getting-started.md)
* [Parooli halduse kohandamine](active-directory-passwords-customize.md)
* [Parooli haldamise head tavad](active-directory-passwords-best-practices.md)
* [Kuidas saada funktsionaalseid arusaamiseks parooli haldus aruanded](active-directory-passwords-get-insights.md)
* [Parooli halduse KKK](active-directory-passwords-faq.md)
* [Tõrkeotsing parooli haldus](active-directory-passwords-troubleshoot.md)
* [Lisateave](active-directory-passwords-learn-more.md)
