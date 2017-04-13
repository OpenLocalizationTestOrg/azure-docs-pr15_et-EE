<properties 
    pageTitle="Kasutage veebisaidi Reportvieweri | Microsoft Azure'i"
    description="Selles teemas kirjeldatakse, kuidas Microsoft Azure'i veebisaidi ja Visual Studio Reportvieweri juhtelement, mis kuvab salvestatud kohta mõne Microsoft Azure virtuaalse masina aruande koostamine."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="guyinacube"
    manager="erikre"
    editor="monicar" 
    tags="azure-service-management" />
<tags 
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/04/2016"
    ms.author="asaxton" />

# <a name="use-reportviewer-in-a-web-site-hosted-in-azure"></a>Reportvieweri Azure majutatud veebisaidi kasutamine

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Microsoft Azure'i veebisaidi ja Visual Studio Reportvieweri juhtelement, mis kuvab aruande, mis on talletatud kohta mõne Microsoft Azure virtuaalse masina saate koostada. Reportvieweri kontroll on veebirakenduse koostate ASP.net-i Web rakenduse malli abil.

>[AZURE.IMPORTANT] ASP.net-i MVC veebirakenduse Mallid ei toeta Reportvieweri kontroll.

Lisada oma Microsoft Azure'i veebisaidi Reportvieweri, peate täitma järgmisi toiminguid.

- **Lisamine** Komplektide juurutamise pakett

- **Konfigureerimine** Autentimise ja luba

- ASP.net-i veebirakenduse Azure **avaldamine**

## <a name="prerequisites"></a>Eeltingimused

Vaadake üle [SQL serveri ärianalüüs Azure'i Virtuaalmasinates](virtual-machines-windows-classic-ps-sql-bi.md)"üldine soovitus ja põhitõed" jaotisele.

>[AZURE.NOTE] Reportvieweri juhtelementide saadetakse koos Visual Studio Standard Edition või kohale. Web arendaja Express Editioni 1Kui peate installima [MICROSOFT REPORT VIEWER 2012 RUNTIME](https://www.microsoft.com/download/details.aspx?id=35747) Reportvieweri käitusaja funktsioonide kasutamiseks.
>
>Microsoft Azure Reportvieweri konfigureeritud kohaliku töötlemise režiimis ei toetata.

Vaadake üle raamatus [aruandlusteenuste aruande viewer juhtelement ja Microsoft Azure virtuaalse masina vastavalt aruande serverites](http://download.microsoft.com/download/2/2/0/220DE2F1-8AB3-474D-8F8B-C998F7C56B5D/Reporting%20Services%20report%20viewer%20control%20and%20Azure%20VM%20based%20report%20servers.docx).

## <a name="adding-assemblies-to-the-deployment-package"></a>Juurutamise pakett assemblereid lisamine

Kui majutate oma ASP.net-i rakenduse kohapealne, Reportvieweri assemblereid installitakse tavaliselt otse assemblervahemälust (GAC) IIS-i serveri rakenduses Visual Studio installimise ajal ja pääseb otse rakenduse. Juhul, kui majutate ASP.net-i rakenduse pilves, Microsoft Azure'i ei luba midagi olema installitud üheks GAC, seega tuleb teil teha kindel Reportvieweri assemblereid on saadaval kohalikult rakenduse. Saate lisage need viited projekti ja need kopeerida kohalikult konfigureerida.

Remote töötlemine režiimis, Reportvieweri kontroll kasutab järgmist assemblereid:

- **Microsoft.ReportViewer.WebForms.dll**: sisaldab Reportvieweri koodi, mille peate kasutama Reportvieweri lehel kuvatud. Kui te kukutage Reportvieweri kontroll leheküljele ASP.net-i projektis lisatakse viide selle komplekti projekti.

- **Microsoft.ReportViewer.Common.dll**: sisaldab tunnid, Reportvieweri kontroll käivitamisel kasutada. See pole lisatakse automaatselt projekti.

### <a name="to-add-a-reference-to-microsoftreportviewercommon"></a>Viide Microsoft.ReportViewer.Common lisamine

- Paremklõpsake oma projekti **Viited** sõlm ja valige **Lisada viide**, valige soovitud komplekti .NET menüüs ja siis nuppu **OK**.

### <a name="to-make-the-assemblies-locally-accessible-by-your-aspnet-application"></a>Kasutamise hõlbustamiseks koosolekutel kohalikult ASP.net-i rakenduse järgi

1. **Viited** kausta, klõpsake Microsoft.ReportViewer.Common komplekti nii, et atribuudid kuvatakse paanil atribuudid.

1. Valige paanil atribuudid seatud **Kohaliku eksemplari** True.

1. Korrake juhiseid 1 ja 2 Microsoft.ReportViewer.WebForms.

### <a name="to-get-reportviewer-language-pack"></a>Saada Reportvieweri keelepakett

1. Installige Microsoft Report Viewer 2012 Runtime redistributable sobiva paketi [Microsofti](http://go.microsoft.com/fwlink/?LinkId=317386)allalaadimiskeskusest.

1. Valige ripploendist soovitud keel ja lehe ümbersuunamist vastavate allalaadimislehele keskele.

1. Klõpsake nuppu **Laadi alla** ReportViewerLP.exe allalaadimise käivitamiseks.

1. Pärast ReportViewerLP.exe allalaadimiseks klõpsake nuppu **Käivita** kohe, või klõpsake salvestamiseks oma arvutisse **salvestada** . Kui klõpsate nuppu **Salvesta**, pidage meeles, kausta, kui salvestate faili nime.

1. Otsige kaust, kuhu te faili salvestasite. Paremklõpsake ReportViewerLP.exe, klõpsake käsku **Käivita administraatorina**ja seejärel klõpsake nuppu **Jah**.

1. Pärast töötab ReportViewerLP.exe, kuvatakse selle c:\windows\assembly on ressurss **Microsoft.ReportViewer.Webforms.Resources** ja **Microsoft.ReportViewer.Common.Resources**failid.

### <a name="to-configure-for-localized-reportviewer-control"></a>Konfigureerida lokaliseeritud Reportvieweri kontroll

1. Alla ja installige Microsoft Report Viewer 2012 Runtime redistributable paketi määratud eeltoodud juhiste järgi.

1. Looge <language> projekti ja Kopeeri kausta seotud ressursside komplekti faile ei. Ressursi komplekti failid kopeerida: **Microsoft.ReportViewer.Webforms.Resources.dll** ja **Microsoft.ReportViewer.Common.Resources.dll**. Valige ressursi komplekti failid ja määrake paanil atribuudid **Kopeeri asukohta väljundkausta** ,**kopeerige alati"**.

1. Web project kultuur UICulture määramiseks. Kuidas määrata mõne ASP.net-i veebileht kultuuri ja UI kultuuri kohta leiate lisateavet teemast [kohta: Kultuur ja UI kultuur seadmine ASP.net-i veebileht globaliseerumiseni](http://go.microsoft.com/fwlink/?LinkId=237461).

## <a name="configuring-authentication-and-authorization"></a>Autentimise ja luba konfigureerimine

Funktsiooni Reportvieweri peab kasutama õige mandaat serveri autentimiseks ja mandaadi on lubatud serveri juurdepääsu aruanded, mida soovite. Autentimise kohta leiate teavet teemast raamatus [aruandlusteenuste aruande viewer juhtelement ja Microsoft Azure virtuaalse masina vastavalt aruande serverites](https://msdn.microsoft.com/library/azure/dn753698.aspx).

## <a name="publish-the-aspnet-web-application-to-azure"></a>ASP.net-i veebirakenduse Azure avaldamine

Juhised leiate Azure'i ASP.net-i veebirakenduse avaldamislehtedel näha [kohta: migreerimine ja avaldada veebirakenduse Azure Visual Studio](../vs-azure-tools-migrate-publish-web-app-to-cloud-service.md) ja [veebirakenduste ja ASP.net-i kasutamise alustamine](../app-service-web/web-sites-dotnet-get-started.md).

>[AZURE.IMPORTANT] Kui lisada Azure juurutamise projekti või lisada Azure pilveteenuse teenuse Project käsu kiirmenüü Solution Exploreris ei kuvata, peate muuta projekti sihtrühma raamistik .NET Framework 4.
>
>Kaks käsud pakuvad põhiosas sama funktsiooni. Ühe või muude käsk kuvatakse kiirmenüü sõltuvalt sellest, millist versiooni Microsoft Azure'i SDK on installitud.

## <a name="resources"></a>Ressursid

[Microsoft aruanded](http://go.microsoft.com/fwlink/?LinkId=205399)

[SQL Server Azure'i Virtuaalmasinates ärianalüüs](virtual-machines-windows-classic-ps-sql-bi.md)

[PowerShelli kasutamine Omarežiimis Report Server Azure'i VM loomiseks](virtual-machines-windows-classic-ps-sql-report.md)

[Aruandluse teenuste aruande viewer juhtelement ja Microsoft Azure virtuaalse masina vastavalt aruande serverid](http://download.microsoft.com/download/2/2/0/220DE2F1-8AB3-474D-8F8B-C998F7C56B5D/Reporting%20Services%20report%20viewer%20control%20and%20Azure%20VM%20based%20report%20servers.docx)
