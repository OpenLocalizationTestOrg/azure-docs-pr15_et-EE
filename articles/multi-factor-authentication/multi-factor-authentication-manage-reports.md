<properties
    pageTitle="Azure'i Mitmikautentimise aruanded"
    description="See kirjeldatakse, kuidas kasutada funktsiooni Azure'i Mitmikautentimise - aruanded."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="reports-in-azure-multi-factor-authentication"></a>Azure'i Mitmikautentimise aruanded

Azure'i Mitmikautentimise pakub mitu aruannet, mida saab kasutada nii teie kui teie asutuses. Aruannete pääseb mitme teguriga autentimine haldusportaali, mis eeldab, et teil oleks Azure'i MFA pakkuja või Azure MFA, Azure AD Premium või ettevõtte mobiilsus litsents. Järgmises loendis saadaolevate aruannete.

Saate kasutada aruannete Azure haldusportaali kaudu.

Nimi| Kirjeldus
:------------- | :------------- |
Kasutus | Kasutusaruanded kuvatava teabe üldine kasutus, kasutaja kokkuvõte ja kasutajale üksikasjad.
Serveri olek|See aruanne kuvab mitme teguriga autentimine serverite kontoga seotud.
Blokeeritud kasutaja ajalugu|Aruannete taotluste blokeerimine või blokeerimise tühistamine kasutajate ajaloo kuvamine
Mööda kasutaja ajalugu|Näitab ajalugu ja taotluste mööduda Mitmikautentimise kasutaja telefoninumber.
Pettuse teatis|Näitab ajaloo jooksul määratud kuupäevavahemikus pettuse teatisi.
Ootele|Loendite aruannete ootele töötlemiseks ja tema oleku. Kui aruanne on lõpule jõudnud, on toodud link alla laadida või aruande vaatamiseks.

## <a name="to-view-reports"></a>Aruannete kuvamiseks

1.  Logige sisse http://azure.microsoft.com
2.  Valige vasakul Active Directory.
3.  Valige üks järgmistest suvanditest:
    - **Suvand 1**: mitme teguri Auth pakkujate vahekaarti. Valige pakkuja MFA ja allosas nuppu haldamine.
    - **Variant 2**: valige kataloogi ja klõpsake vahekaarti konfigureerimine. Valige jaotises mitmikautentimise haldamine haldusteenuste sätete. MFA Teenusesätted lehe allosas nuppu portaali link Ava.
4.  Azure'i mitmekordne autentimine haldusportaal, kuvatakse vaate aruande jaotise vasakpoolsel navigeerimisribal. Siit saate valida eespool kirjeldatud aruanded.

<center>![Pilveteenuse](./media/multi-factor-authentication-manage-reports/report.png)</center>


**Lisaressursid**

* [Kasutajate jaoks](./end-user/multi-factor-authentication-end-user.md)
* [Azure'i Mitmikautentimise MSDN-is](https://msdn.microsoft.com/library/azure/dn249471.aspx)
