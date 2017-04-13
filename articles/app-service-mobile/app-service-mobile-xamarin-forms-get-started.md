<properties
    pageTitle="Alustamine mobiilirakenduste Xamarin.Forms abil"
    description="Järgige selle õpetuse Xamarin.Forms arengu Azure'i Mobile'i rakenduste kasutamise alustamine"
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

#<a name="create-a-xamarinforms-app"></a>Xamarin.Forms rakenduse loomine

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

##<a name="overview"></a>Ülevaade

Selle õpetuse näidatakse, kuidas lisada kirjutamata pilvepõhist teenust Xamarin.Forms mobiilirakenduse abil on Azure Mobile'i rakendus taustväärtus. Loote uue mobiilirakenduse kirjutamata nii lihtsa _Todo loendi_ Xamarin.Forms rakendus, mis talletab rakenduse andmete Azure.

Selle õpetuse lõpuleviimine on nõutav kõik muud mobiilirakenduste õpetused Xamarin.Forms.

##<a name="prerequisites"></a>Eeltingimused

Selle õpetuse tegemiseks vajate järgmist:

* Aktiivne Azure'i konto. Kui teil pole kontot, saate mõne Azure'i prooviversiooni kasutajaks ja saada kuni 10 tasuta Mobile'i rakendused, mis saab edasi kasutada, isegi pärast prooviperioodi lõppu. Lisateavet leiate teemast [Azure tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/).

* Visual Studio abil Xamarin. Vaadake [installiprogramm ja installige Visual Studio ja Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) olevaid juhiseid. 

* Mac-arvutisse või uuema versiooniga Xcode v7.0 ja Xamarin Studio ühenduse installitud. Vt [häälestamine ja Visual Studio ja Xamarin installimine](https://msdn.microsoft.com/library/mt613162.aspx) ja [häälestamine, installimist ja kontrolle Maci kasutajad](https://msdn.microsoft.com/library/mt488770.aspx) (MSDN).
 
>[AZURE.NOTE] Kui soovite alustada Azure'i rakendust Service enne Azure'i konto kasutajaks, minge [Proovige rakenduse teenus](https://tryappservice.azure.com/?appServiceName=mobile), kus saate kohe luua lühiajaline starter Mobile'i rakendus App teenuses. Nõutav; krediitkaardid kohustusi.

## <a name="create-a-new-azure-mobile-app-backend"></a>Looge uus Azure'i Mobile'i rakendus taustväärtus

Järgmiste juhiste abil saate luua uue mobiilirakenduse taustväärtus.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]


Nüüd on ette valmistatud Azure'i Mobile'i rakendus taustväärtus, mida saate kasutada rakenduste mobiilikliendi. Järgmiseks laadite serveri projekti jaoks lihtne "todo loend" kirjutamata ja Azure avaldada.

## <a name="configure-the-server-project"></a>Project serveri konfigureerimine

Järgige alltoodud konfigureerimiseks kasutada Node.js või .NET taustväärtus serveri projekti.

[AZURE.INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

##<a name="download-and-run-the-xamarinforms-solution"></a>Laadige alla ja käivitage Xamarin.Forms lahendus

Siin on paar võimalust. Saate lahendus Mac-arvutisse alla laadida ja avage see Xamarin Studio või saate laadige lahendus Windowsi arvutisse ja avage see Visual Studio abil võrku ühendatud Mac-arvuti rakendus iOS-i jaoks. Kui vajate rohkem üksikasjalikud juhised Xamarin setup stsenaariumid, vaadake [installiprogramm ja installige Visual Studio ja Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) .

Vaatame jätkake:

 1. Mac-arvutis või Windowsi arvutisse, avage [Azure portaali] brauseriaknas.
 2. Oma Mobile rakenduse enne sätted, klõpsake nuppu **Alustada** (mobiil) > **Xamarin.Forms**. Jaotises Samm 3, klõpsake nuppu **Loo uus rakendus** , kui see pole juba valitud.  Järgmiseks klõpsake nuppu **Laadi alla** .

    See allalaadimist projekti, mis sisaldab kliendi rakendus, mis on ühendatud teie mobiilirakenduse kaudu. Salvestage fail tihendatud projekti kohalikus arvutis ja kirjutage, kuhu see salvestada.

 3. Projekti allalaaditud ekstraktida ja avage see Xamarin Studio või Visual Studio.

    ![][9]

    ![][8]


##<a name="optional-run-the-ios-project"></a>(Valikuline) IOS-i projekti käivitamine

See jaotis on mõeldud Xamarin iOS-i projekti iOS-seadmete jaoks. Selles jaotises saate vahele jätta, kui te ei tööta iOS-seadmete.

####<a name="in-xamarin-studio"></a>Xamarin Studio

1. Paremklõpsake iOS-i projekti, ning seejärel käsku **Käivitus-projekt nimega**.
2. Klõpsake menüü **käivitada** **Alustada silumine** projekti koostamine ja käivitage rakendus iPhone emulaator.

####<a name="in-visual-studio"></a>Visual Studio
1. Paremklõpsake project iOS-i ja **käivitus projekti**saate määrata.
2. Klõpsake menüü **koostada** **Configuration Manager**.
3. Valige dialoogiboksis **Configuration Manager** iOS-i projekti **koostamine** ja **Deploy** märkeruudud.
4. Projekti koostamine ja käivitage rakendus iPhone emulaator klahvi **F5** .

    >[AZURE.NOTE] Kui teil on probleeme koostamise, käivitage Nugeti paketi juhataja ja värskendamine, et uusim versioon on Xamarin toetavad paketid. Mõnikord võib Kiirjuhend projektide rakenduses värskendamist uusim maha.    

Rakenduse tippige tähendusrikas teksti, nt _Xamarin saate teada,_ ja klõpsake seejärel soovitud **+** nupp.

![][10]

See saadab POST taotlus uue mobiilirakenduse kirjutamata majutatud Azure. Taotluse andmed lisatakse tabelisse TodoItem. Üksused, mis on talletatud tabelis on tagastatud mobiilirakenduse kirjutamata ja andmed kuvatakse loendis.

>[AZURE.NOTE]
> Leiate mis kasutab teie mobiilirakenduse kirjutamata kaasaskantav klassi Raamatukogu projekti lahenduse failis TodoItemManager.cs C# koodi.

##<a name="optional-run-the-android-project"></a>(Valikuline) Androidi projekti käivitamine

See jaotis on mõeldud Xamarin droid projekti opsüsteemi Android. Selles jaotises saate vahele jätta, kui te ei tööta Androidi seadmete.

####<a name="in-xamarin-studio"></a>Xamarin Studio

1. Paremklõpsake Androidi projekti ja seejärel käsku **Käivitus-projekt nimega**.
2. Klõpsake menüü **käivitada** **Alustada silumine** projekti koostamine ja alustage rakenduse Android emulaator.

####<a name="in-visual-studio"></a>Visual Studio
1. Paremklõpsake Android (Droid) projekti ja **käivitus projekti**saate määrata.
4. Klõpsake menüü **koostada** **Configuration Manager**.
5. Valige dialoogiboksis **Configuration Manager** Androidi projekti **koostamine** ja **Deploy** märkeruudud.
6. Projekti koostamine ja alustage rakenduse Android emulaator klahvi **F5** .

    >[AZURE.NOTE] Kui teil on probleeme koostamise, käivitage Nugeti paketi juhataja ja värskendamine, et uusim versioon on Xamarin toetavad paketid. Mõnikord võib Kiirjuhend projektide rakenduses värskendamist uusim maha.    


Rakenduse tippige tähendusrikas teksti, nt _Xamarin saate teada,_ ja klõpsake seejärel soovitud **+** nupp.

![][11]

See saadab POST taotlus uue mobiilirakenduse kirjutamata majutatud Azure. Taotluse andmed lisatakse tabelisse TodoItem. Üksused, mis on talletatud tabelis on tagastatud mobiilirakenduse kirjutamata ja andmed kuvatakse loendis.

> [AZURE.NOTE]
> Leiate mis kasutab teie mobiilirakenduse kirjutamata kaasaskantav klassi Raamatukogu projekti lahenduse failis TodoItemManager.cs C# koodi.


##<a name="optional-run-the-windows-project"></a>(Valikuline) Käivitage Windows projekt


See jaotis on mõeldud Xamarin WinApp projekti Windowsi seadmete jaoks. Selles jaotises saate vahele jätta, kui te ei tööta koos Windowsi jaoks.


####<a name="in-visual-studio"></a>Visual Studio
1. Paremklõpsake mis tahes Windowsi projektide ja **käivitus projekti**saate määrata.
4. Klõpsake menüü **koostada** **Configuration Manager**.
5. Valige dialoogiboksis **Configuration Manager** Windows projekti, mille valisite **koostamine** ja **Deploy** märkeruudud.
6. Projekti koostamine ja käivitage rakendus Windows emulaator klahvi **F5** .

    >[AZURE.NOTE] Kui teil on probleeme koostamise, käivitage Nugeti paketi juhataja ja värskendamine, et uusim versioon on Xamarin toetavad paketid. Mõnikord võib Kiirjuhend projektide rakenduses värskendamist uusim maha.    


Rakenduse tippige tähendusrikas teksti, nt _Xamarin saate teada,_ ja klõpsake seejärel soovitud **+** nupp.

See saadab POST taotlus uue mobiilirakenduse kirjutamata majutatud Azure. Taotluse andmed lisatakse tabelisse TodoItem. Üksused, mis on talletatud tabelis on tagastatud mobiilirakenduse kirjutamata ja andmed kuvatakse loendis.

![][12]

> [AZURE.NOTE]
> Leiate mis kasutab teie mobiilirakenduse kirjutamata kaasaskantav klassi Raamatukogu projekti oma lahendus failis TodoItemManager.cs C# koodi.

##<a name="next-steps"></a>Järgmised sammud

* [Rakenduse lisamiseks autentimine](app-service-mobile-xamarin-forms-get-started-users.md)  
Saate teada, kuidas kasutajad oma rakenduse pakkujaga identiteedi kinnitamiseks.

* [Tõuketeatiste lisamiseks rakenduse](app-service-mobile-xamarin-forms-get-started-push.md)  
Saate teada, kuidas lisada push teatised rakenduse tugi ja konfigureerida oma mobiilirakenduse kirjutamata saatmiseks tõuketeatisi Azure'i teatis jaoturi abil.

* [Rakenduse ühenduseta sünkroonimise lubamine](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  Saate teada, kuidas lisada ühenduseta režiimi tugi rakenduse abil ka Mobile'i rakendus taustväärtus. Ühenduseta sünkroonimine võimaldab lõppkasutajad suhelda mobiilirakenduses&mdash;vaatamine, lisamine või muutmine andmete&mdash;isegi siis, kui seal on pole võrguühendust.

* [Kuidas kasutada hallatavate kliendi Azure'i mobiilirakenduste kohta](app-service-mobile-dotnet-how-to-use-client-library.md)  
Saate teada, kuidas töötada hallatavate kliendi SDK Xamarin rakenduse. 


<!-- Anchors. -->
[Getting started with mobile app backends]:#getting-started
[Create a new mobile app backend]:#create-new-service
[Next Steps]:#next-steps


<!-- Images. -->
[6]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart.png
[8]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart-vs.png
[9]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart-xs.png
[10]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-ios.png
[11]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-android.png
[12]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-windows.png


<!-- URLs. -->
[Visual Studio Professional 2013]: https://go.microsoft.com/fwLink/p/?LinkID=257546
[Mobile app SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[Azure'i portaal]: https://portal.azure.com/

