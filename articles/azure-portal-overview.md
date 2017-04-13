<properties
    pageTitle="Microsoft Azure'i portaali ülevaade"
    description="Siit saate teada, kuidas kasutada Microsoft Azure'i portaal."
    services=""
    documentationCenter=""
    authors="davidwrede"
    manager="dwrede"
    editor="jimbe"/>

<tags
    ms.service="na"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="12/16/2015"
    ms.author="dwrede"/>

# <a name="microsoft-azure-portal-overview"></a>Microsoft Azure'i portaali ülevaade

Microsoft Azure'i portaal on keskne koht, kus saate ettevalmistamine ja hallata oma Azure ressursse.  Selles õpetuses on kurssi portaali ja näitab teile, kuidas kasutada osa järgmisi olulisi võimalusi.
- **Täielik turuplats** , mille abil saate sirvida tuhandeliste üksuste Microsofti ja muude müüjad, mis võib olla ostnud ja/või ette valmistatud.
- On **ühendatud ja scalable Sirvi kasutusvõimalusi:** mis on lihtne leida ressursid saate hoolikalt kohta ning sooritada erinevaid haldamise toiminguid.
- **Ühtse halduse lehekülgede** (või labad) mis abil saate hallata Azure kaudu iga kord ühtemoodi, asetades sätted, toimingud, arvelduse teavet, seisundi jälgimine ja kasutamine andmete ja palju erinevaid rohkem.
- **Isiklik kogemus** , mis võimaldab teil luua kohandatud avakuva, kus kuvatakse teave, mida soovite vaadata alati, kui logite.  Saate kohandada mõnda haldus labad, mis sisaldavad paanid.

 ![Azure'i portaali Kasutajaliidese suund][UIOrientation]

## <a name="before-you-get-started"></a>Enne alustamist

Peate läbi selles õpetuses kehtiv Azure'i tellimus.  Kui teil pole ühte, siis [tasuta prooviversiooni kasutajaks](https://azure.microsoft.com/pricing/free-trial/) juba täna.  Kui teil on tellimus, pääsete portaali aadressil [https://portal.azure.com].

## <a name="how-to-create-a-resource"></a>Kuidas luua ressurss

Azure'i on turg tuhandete üksused, mida saate luua ühest kohast.  Oletame, et soovite luua uue Windows Server 2012 VM.  Funktsiooni + uus jaoturi on teie sisenemise esiletõstetud kategooriad kuvatakse Marketplace'ist eelkoostatud kogum.  Iga kategooriaga on võrdlemisi väike hulk esiletõstetud üksusi koos linki täielik turuplatsi, kus kuvatakse kõigis kategooriates ja otsing. Selle uue Windows Server 2012 VM loomiseks tehke järgmist.  

1.  Windows Server 2012 on esiletõstetud, seega saate valida Arvuta kategooria.  
2.  Täitke vormi mõne lihtsa sisendina.
3.  Klõpsake 'Loo ja teie VM algab kohe ette valmistada.

Teatiste jaoturi annab teile märku, kui teie ressurss on loodud ja avatakse halduse blade (saate alati sirvides ressursid hiljem).

![Portaali kategooriad][PortalCategories]


## <a name="how-to-find-your-resources"></a>Kuidas leida oma ressursid

Saate alati PIN-koodi sageli ressursid oma startboard, kuid peate midagi, mis ei pääse juurde sageli sirvimiseks.  Sirvi jaoturi allpool on teie võimalus kõigile oma ressursse.  Saate filtreerida tellimus, veergude valimine või suurust, ja liikuge halduse labad üksikute üksuste klõpsates.

![Liikuge jaoturi][BrowseHub]

## <a name="how-to-manage-and-delegate-access-to-a-resource"></a>Kuidas hallata ja volitatud esindaja juurdepääsu ressursile

See blade saate luua ühenduse virtuaalse masina kaugtöölaua, tootluse mõõdikute jälgimine, selle VM rollipõhise juurdepääsu (RBAC) kaudu juurdepääsu, VM konfigureerimine ja muud olulised haldamisega seotud toiminguid teha.  Äärmiselt oluline tasandil haldamine on delegeerivad Accessi rolli alusel.  Klõpsake [siin](./active-directory/role-based-access-control-configure.md) Lisateavet selle kohta. Delegaadi juurdepääsu ressursile, tehke järgmist.

1.  Liikuge sirvides oma ressursi.
2.  Klõpsake nuppu "kõik sätted' Essentialsi jaotises.
3.  Klõpsake loendis sätted 'Kasutajad'.
4.  Käsk ribal nuppu "Lisa".
5.  Valige kasutaja ja rollid.

![Ressursi haldamine][ManageResource]

## <a name="how-to-customize-a-resource-blade"></a>Kuidas kohandada ressursi blade

Azure'i preconfigures lőiketerad oma ressursse, kuid need terad paanide on teie enda juhtelemendile.  Saate hõlpsalt avada üheks kohandamine režiimi lisamine, eemaldamine, suuruse või ümber korraldada paanid. Tera kohandamiseks tehke järgmist.

1.  Liikuge sirvides oma ressursi.
2.  Klõpsake soovitud "..." tera ülaservas, mida soovite kohandada.
3.  Klõpsake osade lisamine.
4.  Käivitage pukseerimine osad.  

![Labad kohandamine][CustomizeBlades]

## <a name="how-to-get-help"></a>Abi hankimine

Kui teil on probleem, oleme siin teie jaoks.  Portaali on punkt teil õiget lehe spikker ja tugi.  Olenevalt teie [toetavad leping](https://azure.microsoft.com/support/plans/), saate luua tugipileteid otse portaalis.  Pärast tugi Piletite loomist saate hallata elutsükli jooksul portaali kaudu soovitud Piletite. Saate avada abi ja lehe tugiteenuste navigeerides Sirvi -> abi + tugi.  

![Spikker ja tugiteenused][HelpSupport]

## <a name="summary"></a>Kokkuvõte

Vaatame selles õpetuses õpitu:
- Saate teada, kuidas registreeruda, saada tellimus ja liikuge sirvides portaali
- Saate sai saamine portaalis Kasutajaliidese ja õppinud, kuidas luua ja sirvige ressursid
- Soovite õpitut ressursi loomise ja liikuge ressursid
- Soovite õpitut struktuuri või halduse labad ja kuidas saate hallata pidevalt erinevat tüüpi ressursside kohta
- Soovite õpitut, kuidas kohandada portaali, mis toob teavet te hoolikalt kohta ees- ja keskel
- Soovite õpitut kohta juurdepääsu rollipõhise juurdepääsu (RBAC) abil
- Soovite õpitut kohta abi ja toe saamiseks

Microsoft Azure'i portaal lihtsustab põhjalikult koostamise ja pilveteenuses rakenduste haldamine.  Heitke pilk [halduse ajaveebi](https://azure.microsoft.com/blog/topics/management/) ajakohastamine nagu me pidevalt [kuulata tagasiside](https://feedback.azure.com/forums/223579-azure-preview-portal/) ja täiustada.  [ScottGu's ajaveebi](http://weblogs.asp.net/scottgu) on teine suurepärane koht kõigi Azure värskenduste otsimiseks.

[UIOrientation]: ./media/azure-portal-how-to-use/azure_portal_1.png
[PortalCategories]: ./media/azure-portal-how-to-use/azure_portal_2.png
[BrowseHub]: ./media/azure-portal-how-to-use/azure_portal_3.png
[ManageResource]: ./media/azure-portal-how-to-use/azure_portal_4.png
[CustomizeBlades]: ./media/azure-portal-how-to-use/azure_portal_5.png
[HelpSupport]: ./media/azure-portal-how-to-use/azure_portal_6.png
