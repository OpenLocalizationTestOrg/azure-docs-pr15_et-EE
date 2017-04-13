<properties
    pageTitle="v2.0 rakenduse registreerimine | Microsoft Azure'i"
    description="Kuidas registreeruda rakendus Microsoft sisselogimise lubamine ja Microsofti teenuste abil v2.0 lõpp-punkti juurdepääs"
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

# <a name="how-to-register-an-app-with-the-v20-endpoint"></a>Kuidas registreeruda rakenduse abil v2.0 lõpp-punkti

Rakendus, mida aktsepteerib nii MSA ja Azure AD koostamiseks Logi sisse, esmalt peate registreerida rakendus Microsoft.  Sel ajal, ei saa kasutada Azure AD peate olemasoleva rakendustel või MSA - peate luua täiesti uue dokumendi loomine.

> [AZURE.NOTE]
    Kõik Azure Active Directory stsenaariumid ja funktsioonid on toetatud v2.0 lõpp-punkt.  Kui kasutate v2.0 lõpp-punkti määramiseks lugege [v2.0 piirangud](active-directory-v2-limitations.md).

## <a name="visit-the-microsoft-app-registration-portal"></a>Külastage Microsofti rakenduse registreerimise portaal
Alustame avage esmalt - [https://apps.dev.microsoft.com/?deeplink=/appList](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).  See on uus rakendus registreerimise portaal kus saate hallata oma Microsofti rakendused.

Kas sisse logida isikliku või töö või kooli Microsofti kontot.  Kui teil pole kas uue konto kasutajaks. Minna, ei võta kaua - me ootama siin.

Teha? Siis peaks nüüd vaadata loendit Microsoft rakendusi, mis on tõenäoliselt tühi.  Vaatame seda muuta.

Klõpsake käsku **Lisa rakendus**ja sellele nime panna.  Portaali määrata rakenduse rakenduse globaalselt kordumatu Id, mida kasutate hiljem koodis.  Kui teie rakendus sisaldab serveripoolne komponent mis vajab Accessi sõned helistaja API-de (arvates: Office, Azure'i või oma veebi-API), peaksite loomiseks on **Rakenduse salajane** siin ka.
<!-- TODO: Link for app secrets -->

Järgmisena lisage platvormid, mis kasutab teie rakendus.

- Sisestage vastavalt veebirakendustes **URI ümber suunata** , kus saab saata sõnumeid Logi sisse.
- Mobiilirakendused, Kopeeri vaikimisi allapoole suunake uri teie jaoks automaatselt loodud.

Soovi korral saate kohandada oma sisselogimise lehe jaotises profiil ilme ja olemus.  Veenduge, et enne nuppu **Salvesta** .

> [AZURE.NOTE] Kui loote [https://apps.dev.microsoft.com/?deeplink=/appList](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)-rakendus, registreeritud rakenduse home rentniku konto, mida kasutate portaali sisse logida.  See tähendab, et te ei saa registreerida rakenduse oma Azure AD rentniku isikliku Microsofti kontoga.  Kui soovite konkreetselt kindla rentniku rakenduse registreerimine, logige sisse loodud algselt selle konto.

## <a name="build-a-quick-start-app"></a>Rakenduse loomiseks Lühijuhend
Nüüd, kui teil on Microsoft rakenduse, saate täita meie v2.0 Lühijuhend õpetused.  Siin on mõned soovitused.

[AZURE.INCLUDE [active-directory-v2-quickstart-table](../../includes/active-directory-v2-quickstart-table.md)]
