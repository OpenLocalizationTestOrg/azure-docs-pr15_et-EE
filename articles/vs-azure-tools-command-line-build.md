<properties
   pageTitle="Käsurea Koosta Azure | Microsoft Azure'i"
   description="Azure käsurea koostamine"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="command-line-build-for-azure"></a>Azure käsurea koostamine

## <a name="overview"></a>Ülevaade

Saate luua paketi Azure juurutamiseks MSBuild käivitades käsureale. Saate konfigureerida ja määratleda järgud silumine, lavastus ja tootmiseks, lisaks automatiseerimine mõned Koosta protsess.


## <a name="microsoft-build-engine-msbuild"></a>Microsoft Koosta otsimootori (MSBuild)

Microsoft koostada Engine (MSBuild) abil saate koostada koostamine toodete lab keskkonnas, kus pole installitud Visual Studio. Projekti faile, mis on laiendatav ja täielikult toetatud Microsoft kasutab MSBuild on XML-vorming. Selles vormingus, saab kirjeldada kirjed peavad olema ehitatud ühe või mitme platvormid ja konfiguratsioone.

Samuti saate käivitada MSBuild käsuviibale ja selles teemas kirjeldatakse seda lähenemisviisi. Käsk Atribuudid seadmisega saate koostada teatud konfiguratsioone projekti. Samuti saate määratleda sihtkohtade, mis MSBuild käsu ehitada. Käsurea parameetrid ja MSBuild kohta leiate lisateavet teemast [MSBuild käsurea viide](https://msdn.microsoft.com/library/ms164311.aspx).

## <a name="installation"></a>Installimine

Nagu Järgmistes juhistes kirjeldatakse, peate installima tarkvara ja tööriistad serveris koostamine enne MSBuild abil saate luua ka Azure'i paketi:

1. Installige .NET Framework 4 või uuem versioon, mis sisaldab MSBuild.

1. Installige [Azure'i loome tööriistad](http://go.microsoft.com/fwlink/?LinkId=394615) (vt MicrosoftAzureAuthoringTools-x64.msi või MicrosoftAzureAuthoringTools-x86.msi.

1. Installige [Azure'i teekide .net-i](http://go.microsoft.com/fwlink/?LinkId=394616) (vt MicrosoftAzureLibsForNet-x64.msi või MicrosoftAzureLibs-x86.msi.

1. Kopeerige fail Microsoft.WebApplication.targets Visual Studio installimine mõnda muusse arvutisse.

    Fail asub kataloogis C:\Program Files (x86) \MSBuild\Microsoft\Visual Studio\v12.0\WebApplications (v11.0 Visual Studio 2012) ja peaks kopeerimiseks serveris Koosta samas kaustas.

1. Installige [Azure'i Tools for Visual Studio](http://go.microsoft.com/fwlink/?LinkId=394616).

    Otsige WindowsAzureTools.vs120.exe Visual Studio 2013 projektide koostamiseks.

## <a name="msbuild-parameters"></a>MSBuild parameetrid

Lihtsaim viis paketi loomiseks on MSBuild koos käivitamiseks on `/t:Publish` suvand. Vaikimisi see käsk loob kausta seoses juurkausta projekti, nt ProjectDir\bin\Configuration\app.publish\. kui koostate mõnda Azure'i projekti, saate luua kaks faili, ise paketifaili ja kaasnevaid konfiguratsioonifail:

- Project.cspkg

- ServiceConfiguration.TargetProfile.cscfg

Vaikimisi iga Azure'i projekti sisaldab ühte teenuse konfiguratsioonifail kohaliku (silumine) koostab ja teine pilveteenuse (lavastus või tootmise) järgud, kuid te saate lisada või eemaldada teenuse konfigureerimine failide vastavalt vajadusele. Kui koostate paketi Visual Studio sees, palutakse teil mis teenuse konfigureerimine faili kaasa pakendi kõrval. Kui koostate paketi MSBuild abil, on vaikimisi kaasatud kohaliku teenuse konfiguratsioonifail. Muu teenuse konfiguratsioonifail kaasata, määrake soovitud `TargetProfile` atribuudi MSBuild käsk (`MSBuild /t:Publish /p:TargetProfile=ProfileName`).

Kui soovite kasutada mõne alternatiivse directory salvestatud paketi ja failid, Määrake tee, kasutades funktsiooni `/p:PublishDir=Directory\` suvand, sh lõpunullid kurakriipsu eraldaja.

## <a name="deployment"></a>Juurutamine

Pärast paketi on loodud, peate selle juurutama Azure. Õpetuse, mis näitab selle protsessi leiate Azure'i veebisaidi. Lisateavet selle protsessi automatiseerida leiate teemast [Azure pilveteenused pidev kohaletoimetamist](./cloud-services/cloud-services-dotnet-continuous-delivery.md).
