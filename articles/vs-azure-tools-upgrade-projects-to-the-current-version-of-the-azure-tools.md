<properties
   pageTitle="Projektide Azure'i tööriistad praeguse versiooni täiendamise | Microsoft Azure'i"
   description="Saate teada, kuidas võtta kasutusele mõne Azure'i projekti Visual Studio praegune versioon Azure tööriistad"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="how-to-upgrade-projects-to-the-current-version-of-the-azure-tools-for-visual-studio"></a>Kuidas uuendada projektide praeguse versiooni Azure'i tööriistad Visual Studio

## <a name="overview"></a>Ülevaade

Pärast installimist praeguse versiooni Azure'i tööriistad (või eelmise versiooni, mis on uuem kui 1,6), mis tahes Azure'i tööriistade abil loodud projekte vabastage enne 1,6 (November 2011) uuendatakse automaatselt, kui avate need. Kui 1,6 (November 2011) versiooni nende tööriistade abil loodud projekte ja teil on endiselt see väljaanne installitud, saate avada projektid vanemas versioonis ja hiljem otsustada, kas neid uuendada.

## <a name="how-your-project-changes-when-you-upgrade-it"></a>Kuidas oma projekti muutub versiooniuuenduse installimisel

Kui projekti uuendatakse automaatselt või saate määrata, kas soovite seda uuendada, projekti on muudetud teatud komplektide praeguse versiooni töötamiseks ja osa atribuute on muudetud ka nagu selles jaotises kirjeldatakse. Kui projekti jaoks on vaja muud muudatused ühilduma tööriistade uuema versiooni, tuleb teil teha neid muudatusi käsitsi.

- Fail web rollid ja töötaja rollide app.config faili on värskendatud viitamiseks Microsoft.WindowsAzure.Diagnostics.DiagnosticMonitoirTraceListener.dll uuema versiooni.

- Uus versioon on täiendatud Microsoft.WindowsAzure.StorageClient.dll, Microsoft.WindowsAzure.Diagnostics.dll ja Microsoft.WindowsAzure.ServiceRuntime.dll assemblereid.

- Eraldi faili laiend .azurePubXml, klõpsake **Avalda** alamkausta teisaldatakse avalda profiilid, mida talletatakse Azure'i projekti faili (.ccproj).

- Osa atribuute avalda profiilis värskendatakse uue ja muudetud funktsioonide tugi. Kuna juurutatud pilveteenuses saate värskendada korraga või artistide asendatakse **AllowUpgrade** **DeploymentReplacementMethod** .

- Atribuut **UseIISExpressByDefault** lisatakse ja väärtuseks false, et veebiserver, mida kasutatakse silumine ei muutu automaatselt: Internet Information Services (IIS) IIS-i kiire. IIS-i kiire on vaikimisi veebiserver tööriistad uuemate versioonidega loodud projekte.

- Kui Azure vahemällu majutab ühe või mitme projekti rollid, muudetakse osa atribuute teenuse konfigureerimine (.cscfg fail) ja teenuse määratlus (.csdef-faili) projekti uuendamisel. Kui projekt kasutab Azure vahemällu Nugeti pakett, projekti versioonile paketi uusima versiooni. Peaksite avage fail ja kontrollige, et kliendi konfiguratsiooni hoitakse õigesti versiooniuuenduse installimise käigus. Kui lisasite Azure'i vahemällu kliendi assemblereid viited Nugeti pakett kasutamata, ei värskendata neid assemblereid; peate värskendama käsitsi need viited uued versioonid.

>[AZURE.IMPORTANT] F # projektid, ei peab käsitsi värskendada Azure assemblereid viiteid nii, et nad viitavad nende komplektide uuemad versioonid.

### <a name="how-to-upgrade-an-azure-project-to-the-current-release"></a>Kuidas uuendada mõnda Azure'i projekti praegune versioon

1. Installige Azure'i tööriistad praeguse versiooni installimise Visual Studio, mida soovite kasutada täiendatud projekti ja seejärel avage projekt, mida soovite täiendada. Kui projekt on loodud Azure'i tööriistad vabastage see 1,6 enne (November 2011), projekti automaatselt praeguse versiooni täiendamist. Kui projekt on loodud November 2011 vabastada ja selle versiooni installimist endiselt, projekti avatakse see väljaanne.

1. Solution Exploreris projekti sõlm kiirmenüü avamine, valige **Atribuudid**ja seejärel valige kuvatavas dialoogiboksis vahekaarti **rakendus** .

    **Vahekaardi** kuvab tööriistad versioon, mis on seotud projekt. Kui kuvatakse praegune versioon Azure tööriistad, projekt on juba täiendatud. Kui olete installinud kui, mis kuvatakse vahekaardil tööriistade uuemat versiooni, kuvatakse ka **täiendamine** nupp.

1. Klõpsake nuppu **versiooniuuenduse** uuendada projekti tööriistade praeguse versiooni.

1. Projekti koostamine ja seejärel aadress API muudatused põhjustavad vigu. Lisateavet selle kohta, kuidas muuta oma koodi uus versioon, teatud API dokumentatsioonist.
