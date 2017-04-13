<properties
   pageTitle="Häälestada oma arenduskeskkond | Microsoft Azure'i"
   description="Installige käitusaja, SDK ja tööriistad ja loomine kohaliku arengu kobar. Pärast selle setup, saab luua rakendusi valmis."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/26/2016"
   ms.author="ryanwi"/>

# <a name="prepare-your-development-environment"></a>Teie arenduskeskkond ettevalmistamine

> [AZURE.SELECTOR]
-[Windows](service-fabric-get-started.md)
- [Linux](service-fabric-get-started-linux.md)
- [OSX](service-fabric-get-started-mac.md)

 Luua ja kasutada [rakendusi Azure teenuse struktuuri] [ 1] arengu arvutisse installida käitusaja, SDK ja tööriistu. Samuti peate Windows PowerShelli skriptide kaasatud SDK käivitamise lubamiseks.

## <a name="prerequisites"></a>Eeltingimused
### <a name="supported-operating-system-versions"></a>Toetatud operatsioonisüsteemide versioonidega
Arengu on toetatud järgmised operatsioonisüsteemi versiooni:

- Windows 7
- Windows 8/Windows 8.1
- Windows Server 2012 R2
- Windows 10

>[AZURE.NOTE] Windows 7 sisaldab ainult Windows PowerShell 2.0 vaikimisi. Teenuse struktuuri PowerShelli cmdlet-käskude nõuab PowerShelli 3.0 või uuem versioon. Saate [alla laadida Windows PowerShelli 5.0] [ powershell5-download] Microsoft Download Center.

## <a name="install-the-runtime-sdk-and-tools"></a>Installige käitusaja, SDK ja tööriistad

Web platvormi Installer pakub kahte konfiguratsioone teenuse struktuuri arengu:

- [Installige see teenuse struktuuri käitusaja, SDK ja tööriistad Visual Studio 2015 (nõuab Visual Studio 2015 Update 2 või uuemad versioonid)][full-bundle-vs2015]
- [Teenuse struktuuri käitusaja ja SDK ainult (Visual Studio tööriistad) installimine][core-sdk]

## <a name="enable-powershell-script-execution"></a>PowerShelli skripti täitmise lubamine

Teenuse struktuuri kasutab Windows PowerShelli skriptide loomise kohaliku arengu kobar ja juurutamine Visual Studio rakendusi. Vaikimisi blokeerib Windows nende skriptide töökorras. Nende lubamiseks peate muutma oma PowerShelli täitmise poliitika. Avage PowerShelli administraatorina ja sisestage järgmine käsk:

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Force -Scope CurrentUser
```

## <a name="next-steps"></a>Järgmised sammud
Nüüd, kui te olete oma arenduskeskkond häälestamise lõpetanud, käivitage koostamise ja rakenduste.

- [Luua esimese teenuse struktuuri rakenduse Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md)
- [Saate teada, kuidas juurutada ja hallata rakendusi kohaliku klaster](service-fabric-get-started-with-a-local-cluster.md)
- [Lisateavet programmeerimise mudelite: usaldusväärsed teenused ja usaldusväärne osalejad](service-fabric-choose-framework.md)
- [Tutvuge teenuse struktuuri koodinäiteid github](https://aka.ms/servicefabricsamples)
- [Visualiseerimine klaster teenuse struktuuri Exploreri kaudu](service-fabric-visualizing-your-cluster.md)
- [Järgige saada laialdane tutvustus platvormi teenuse struktuuri õppeteema](https://azure.microsoft.com/documentation/learning-paths/service-fabric/)

[1]: http://azure.microsoft.com/en-us/campaigns/service-fabric/ "Teenuse struktuuri turunduskampaania leht"
[2]: http://go.microsoft.com/fwlink/?LinkId=517106 "VS RC"
[full-bundle-vs2015]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015 "VS 2015 WebPI link"
[full-bundle-dev15]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-Dev15 "Dev15 WebPI link"
[core-sdk]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK "Core SDK WebPI link"
[powershell5-download]:https://www.microsoft.com/en-us/download/details.aspx?id=50395
