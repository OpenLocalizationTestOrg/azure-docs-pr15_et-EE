<properties 
    pageTitle="Kuidas seadistada arvuti Media Services arengu .net-i abil" 
    description="Lisateavet Media Servicesi .NET Media Services SDK kasutamise eeltingimused. Samuti saate teada, kuidas luua rakenduse Visual Studio." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="10/24/2016"  
    ms.author="juliako"/>

#<a name="media-services-development-with-net"></a>Media Servicesi arengu .net-i abil

[AZURE.INCLUDE [media-services-selector-setup](../../includes/media-services-selector-setup.md)]

Selles teemas kirjeldatakse, kuidas alustada Media Servicesi rakenduste .net-i abil.

**Azure Media Services.NET SDK** teegi võimaldab programmi vastu Media Servicesi .net-i abil. Oleks veelgi lihtsam töötada koos .net-i, on esitatud **Azure Media Services .NET SDK laiendid** teek. See teek sisaldab laiend meetodite ja helper funktsioone, mis lihtsustab oma .net-i kood. Nii teegid on saadaval **Nugeti** ja **GitHub**kaudu.


##<a name="prerequisites"></a>Eeltingimused

-   Media Servicesi konto uude või olemasolevasse Azure'i tellimus. Teemast [Media Services konto loomise kohta](media-services-portal-create-account.md).
-   Operatsioonisüsteemid: Windows 10, Windows 7, Windows 2008 R2 või Windows 8.
-   .NET framework 4.5.
-    Visual Studio 2015, Visual Studio 2013, Visual Studio 2012 või Visual Studio 2010 SP1 (Professional, Premium, Ultimate või Express).


##<a name="create-and-configure-a-visual-studio-project"></a>Loomine ja konfigureerimine Visual Studio projekti

Selles jaotises kirjeldatakse, kuidas projekti loomine Visual Studio ja häälestama Media Servicesi arengu.  Kui projekt on C# Windowsi konsooli rakendus, kuid siin näidatud sama häälestamise juhised kehtivad muud tüüpi projekti, saate luua Media Servicesi rakenduste (nt Windowsi vormide rakendus või rakenduse ASP.net-i Web).

Selles jaotises kirjeldatakse **Nugeti** lisada Media Services .NET SDK ja muude sõltuvad teekide kasutamise kohta.

Teise võimalusena saate uusima Media Services .NET SDK bittide toomine GitHub ([github.com/Azure/azure-sdk-for-media-services](https://github.com/Azure/azure-sdk-for-media-services) ja [github.com/Azure/azure-sdk-for-media-services-extensions](https://github.com/Azure/azure-sdk-for-media-services-extensions)), lahendus luua ja lisada kliendi projekti viiteid. Pange tähele, et kõik vajalikud sõltuvused saada allalaaditud ekstraktimist automaatselt.

1. Visual Studio 2010 SP1 või uuem VS luua uus konsool rakendus C#. Sisestage **nimi**, **asukoha**ja **lahenduse nimi**ja seejärel klõpsake nuppu OK.

2. Koostada lahendus.

2. Kasutage **Nugeti** installida ja **Azure Media Services .NET SDK laiendid**. Selle paketi installimisel installib **Media Services.NET SDK** ja lisab kõik nõutavad sõltuvused.

    Veenduge, et teil on installitud Nugeti uusim versioon. Veel teavet ja installimise juhiste saamiseks vaadake teemat [Nugeti](http://nuget.codeplex.com/).

2. Solution Exploreris paremklõpsake projekti nime ja valige Halda Nugeti pakettide...

    Kuvatakse dialoogiboks haldamine NuGet-paketid.

3. Online Galerii, otsige Azure'i MediaServices laiendid, valige Azure Media Services .NET SDK laiendid ja seejärel klõpsake nuppu Installi.

    Projekti on muudetud ja viited Media Services .NET SDK laiendid, Media Services .NET SDK ja teiste sõltuvad komplektide lisatakse.

4. Edendada cleaner arenduskeskkond, kaaluge Nugeti pakett taastamine. Lisateavet leiate teemast [Nugeti pakett taastamine "](http://docs.nuget.org/consume/package-restore).

3. Lisage **System.Configuration** komplekti viide. Selle komplekti sisaldab selle System.Configuration. **ConfigurationManager** klassi, kasutatakse juurdepääsuks failid (nt App.config).

    Viited Viited haldamine dialoogiboksis lisamiseks paremklõpsake Solution Exploreris projekti nime. Seejärel valige Lisa ja viited.

    Kuvatakse dialoogiboks viited haldamine.

4. .NET Frameworki komplekte, klõpsake jaotises otsimine ja valige System.Configuration komplekti ja klõpsake nuppu OK.
5. Avage fail App.config (lisamine projekti faili, kui see on lisatud, vaikimisi) ja mõne *appSettings* jaotise faili lisamine.     
Määrake oma Azure Media Servicesi konto nimi ja konto võti, väärtused, nagu on näidatud järgmises näites.

    Nime ja klahvi asuvate väärtuste otsimine, Azure'i portaalis ja valige oma konto. Sätete aken paremal. Valige aknas sätted võtmed. Iga tekstivälja kõrval olevat ikooni klõpsamisel kopeerib väärtuse süsteemi lõikelauale.


        <configuration>
        ...
            <appSettings>
              <add key="MediaServicesAccountName" value="Media-Services-Account-Name" />
              <add key="MediaServicesAccountKey" value="Media-Services-Account-Key" />
            </appSettings>

        </configuration>

6. Olemasoleva **abil** teksti alguses Program.cs faili kirjutada järgmine kood.

        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Text;
        using System.Threading.Tasks;
        using System.Configuration;
        using System.Threading;
        using System.IO;
        using Microsoft.WindowsAzure.MediaServices.Client;

Sel hetkel, olete valmis arendamise Media Services rakenduse käivitamiseks.    


##<a name="media-services-learning-paths"></a>Media Servicesi Õppeteed

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
