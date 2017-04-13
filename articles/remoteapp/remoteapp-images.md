<properties
    pageTitle="Mis on Azure RemoteApp malli pilti? | Microsoft Azure'i"
    description="Teavet mallide piltide kaasas Azure RemoteApp."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="elizapo" />

# <a name="what-is-in-the-azure-remoteapp-template-images"></a>Mis on Azure RemoteApp malli pilti?

> [AZURE.IMPORTANT]
> Azure RemoteApp katkeb. Lugege lisateavet [teadaanne](https://go.microsoft.com/fwlink/?linkid=821148) .

Tellimuse Azure RemoteApp sisaldab kolm malli pilti.


- Windows Server 2012
- Microsoft Office 365 ProPlusi (Office 365 tellimuse)
- Microsoft Office 2013 Professional Plusi (ainult prooviversioon)

> [AZURE.IMPORTANT]Tellimuse Azure RemoteApp annab teile juurdepääsu pilte, välja arvatud Office 365 ProPlus, mis nõuab eraldi tellimus, ja Office 2013, mida ei saa kasutada valmistamisel tarkvara. See tähendab, et saate jagada programmi või rakenduste mallide piltide kasutajatega. Näiteks kui loote saidikogumi, mis kasutab Windows Server 2012 R2 pilt, saate avaldada System Center Endpoint Protection kasutajatel RemoteAppi kaudu.
>
> Vaadake lisateavet [RemoteAppi hulgilitsentsimise üksikasjad](remoteapp-licensing.md) . Ja [Azure RemoteApp Office'i abil](remoteapp-o365.md) Office'i litsentsid teave.

Lugege edasi, mida iga pilt sisaldab täpsemat.

## <a name="windows-server-2012-r2--the-vanilla-image"></a>Windows Server 2012 R2 ("vanilje pilt")
Sellel pildil on operatsioonisüsteemi Microsoft Windows Server 2012 R2 andmekeskuse ja on järgmised rollid ja Azure RemoteApp mallide piltide nõuete täitmiseks funktsioone:


- .NET framework 4.5, 3.5.1, 3.5
- Töölauafunktsioonid
- Tint ja käsitsikirja teenused
- Media Foundation
- Seansil Host
- Windows PowerShelli 4.0
- Windows PowerShell ISE
- WoW64 tugi

Sellel pildil on ka järgmisi rakendusi.

- Adobe Flash Playeri
- Microsoft Silverlight
- Microsoft System Center 2012 Endpoint Protection
- Microsoft Windows Media Playeri


## <a name="microsoft-office-365-proplus-subscription-required"></a>Microsoft Office 365 ProPlusi (nõuab tellimuse)
Office 365 on kõige soovitud rakendus, et lõime "kohandatud" pilt, saate töötada.

Sellel pildil on vanilli pilt laiend ja Microsoft Office 365 ProPlusi kirjeldatud Windows Server 2012 R2 pildi peale installitud järgmised komponendid on:


- Juurdepääs
- Exceli
- Lynci
- OneNote'i
- OneDrive for Business (lisa sünkroonimine agent ei toetata Azure RemoteApp jaoks)
- Outlooki
- PowerPointi
- Wordi
- Microsoft Office'i õigekeelsusriistad

Pilt sisaldab ka Visio Pro ja Project Pro.

Ja selle järgmisi rakendusi.

- SQL-i omakliendi
- ODBC-draiver
- SQL Server Data Miningu klient
- MasterDataServices klient
- Microsoft Publisher
- PowerQuery
- Kujundifaili


Office 365 ProPlusi rakenduste täisfunktsionaalsuse on saadaval ainult kasutajatele, kellel on Office 365 ProPlus lepingut. Üksikasjalikumat teavet Office 365 tellimislepingute teemast [Office 365 teenuse lepingud](http://technet.microsoft.com/library/office-365-plan-options.aspx). Kas teil on veel küsimusi? Vaadake [Office 365 + RemoteApp](remoteapp-o365.md) teavet. Lugege lisaks ka uus artikkel, [Kuidas kasutada Office 365 tellimuse Azure RemoteApp](remoteapp-officesubscription.md).

Pange tähele, et teil on vaja Office 365 ProPlus, Visio Pro ja Project Pro eraldi litsentsi - nad on oma litsentsi.

## <a name="microsoft-office-2013-professional-plus-trial-only"></a>Microsoft Office 2013 Professional Plusi (ainult prooviversioon)
Tasuta prooviversiooni perioodi jooksul saate testida teenuse Office 2013 pildiga.

Sellel pildil on vanilli pilt laiendamise ja Microsoft Office 2013 Professional Plusi kirjeldatud Windows Server 2012 R2 pildi peale installitud järgmised komponendid on:


- Juurdepääs
- Exceli
- Lynci
- OneNote'i
- OneDrive for Business (lisa sünkroonimine agent ei toetata Azure RemoteApp jaoks)
- Outlooki
- PowerPointi
- Projekti
- Visio
- Wordi
- Microsoft Office'i õigekeelsusriistad

> [AZURE.IMPORTANT]**Juriidiline teave:** Sellel pildil ei sisalda Microsoft Office'i litsents ja *ei saa kasutada tekitamiseks*. Office 2013 Professional Plusi pilt on mõeldud ainult prooviversiooni kasutamiseks. Kui soovite kasutada Office'i rakenduste Azure RemoteApp tootmiseks, peate kasutama teenusekomplekti Office 365 ProPlus pilt. Hulgilitsentsimise Office'i kohta leiate lisateavet teemast [Azure RemoteAppi abil Office 365](remoteapp-o365.md)
