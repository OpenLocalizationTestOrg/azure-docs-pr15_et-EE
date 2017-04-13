<properties 
   pageTitle="Visual Studio ühendatud teenuste kasutamisel Mobile teenuste lisamine | Microsoft Azure'i"
   description="Klõpsake dialoogiboksis Lisa Mobile teenuste Visual Studio lisamine ühendatud teenuste abil"
   services="visual-studio-online"
   documentationCenter="na"
   authors="mlhoop"
   manager="douge"
   editor="" />
<tags 
   ms.service="visual-studio-online"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="mobile"
   ms.date="12/16/2015"
   ms.author="mlearned" />

# <a name="adding-mobile-services-by-using-visual-studio-connected-services"></a>Visual Studio ühendatud teenuste kasutamisel Mobile teenuste lisamine

Visual Studio 2015, kus saate luua ühenduse Azure Mobile teenused dialoogis **Ühendatud teenuse lisamine** . Saate luua ühenduse mõne kliendi rakenduse C#, mis tahes JavaScripti rakenduse või mitu platvormi Cordova rakendus. Pärast ühenduse loomist saate luua ja pääsevad andmetele juurde, luua kohandatud API-d ja ajastatud tööd või lisada tõuketeavituse tugi.  Ühendatud teenuste toiming lisab kõik vastavad viited ja ühenduse kood. Te saate ka ära tugi autentimise erinevaid populaarsed identiteedi süsteemide nagu Azure AD, Facebooki, Twitteri ja Microsofti Accounts.

## <a name="supported-project-types"></a>Toetatud projekti tüübid

>[AZURE.NOTE] Visual Studio 2015 Azure Mobile teenuste lisamine Windows universaalne (Windows 10) projektid ühendatud teenuste lisamine dialoogiboksi kaudu ei toetata. Saate lisada Azure Mobile teenused, installides Nugeti Package Manager abil saate projekti jaoks sobiv paketid.

Ühendatud teenuste dialoogiboksi abil saate luua ühenduse järgmist tüüpi projekti Azure Mobile teenused.

- .Net-i Windows 8.1 Store, telefonis ja universaalne App projektid

- JavaScripti Windows 8.1 poe, telefonis ja universaalne App projektid

- Visual Studio Tools for Apache Cordova abil loodud projektid


## <a name="connect-to-azure-mobile-services-using-the-add-connected-services-dialog"></a>Ühenduse loomine Azure Mobile teenused dialoogis ühendatud teenuste lisamine

1. Veenduge, et olete Azure'i konto. Kui teil pole Azure'i konto, te saate registreeruda [tasuta prooviversioon](http://go.microsoft.com/fwlink/?LinkId=518146).

1. Avage dialoogiboks **Ühendatud teenuste lisamine** .
 - .Net-i rakendused, avage oma projekti Visual Studios, Solution Exploreris **Viited** sõlm kontekstimenüü avamiseks ja seejärel valige **Ühendatud teenuse lisamine**
 
        ![Connecting to Azure Mobile Service](./media/vs-azure-tools-connected-services-add-mobile-services/IC797635.png)

 - Apache Cordova rakenduse projektide, avage oma projekti Visual Studios, avada kontekstimenüü projekti sõlm Solution Exploreris ja valige **Ühendatud teenuse lisamine**.

1. Dialoogiboksis **Lisa ühendatud teenuse** valimine **Azure Mobile teenused**ja seejärel klõpsake nuppu **Konfigureeri** . Teil palutakse Azure'i sisse logida, kui te pole seda juba teinud.

    ![Azure'i mobiilsideseadmete teenuse lisamine](./media/vs-azure-tools-connected-services-add-mobile-services/IC797636.png)

1. Valige dialoogiboksis **Azure Mobile teenused** olemasoleva Mobiiliversiooni teenuse, kui on olemas. Kui teil on vaja luua uue Azure mobiilsideteenuse, tehke seda teha järgmist. Muul juhul jätkake järgmise juhisega.

    Uue mobiilsideteenuse konto loomiseks tehke järgmist.
    1. Valige dialoogiboksi allosas linki **Teenuse loomine **.
        ![Lisage uus ühendatud mobiilsideteenus](./media/vs-azure-tools-connected-services-add-mobile-services/IC797637.png)




    2. Klõpsake dialoogiboksis **Loomine mobiiliteenuse** saate valida JavaScripti kirjutamata mobiilsideteenuse või .NET taustväärtus mobiilsideteenuse **käitusaja** ripploendist. 
  
        ![Loomise mobiilsideteenuse ülevaade](./media/vs-azure-tools-connected-services-add-mobile-services/IC797638.png)

        JavaScripti kirjutamata teenus on lihtne ja võimas. Kui loote JavaScripti kirjutamata mobiilsideteenus, serveripoolse JavaScripti koodi talletatakse pilves, kuid Server Explorer või Azure haldusportaali abil saate redigeerida serveri skriptide. 

        .Net-i kirjutamata mobiilsideteenus annab teile täielik õigus ja paindlikkuse Web API ja üksuse raames. Kui loote .NET taustväärtus mobiilsideteenuse, projekti on teie jaoks loodud ja lisatakse teie lahendus. 

    1. Valige **piirkond** , kuhu soovite mobiilsideteenuse ülevaade ja sisestage serveri kasutajanimi ja parool.
 
    1. Kui olete nõutava teabe sisestanud, valige soovitud mobiilsideteenuse loomiseks nuppu **Loo** .
    2. Uue mobiilsideteenuse peaks kuvatama dialoogiboksi **Azure Mobile teenused** loendis teenus. Valige uus mobiilsideteenuse loendist ja seejärel valige nupp **Lisa** teenuse lisamine projekti.
    

1. Vaadake üle saamisega alustamine leht, mis kuvatakse ja teada saada, kuidas oma projekti muutmise. Iga kord, kui lisate ühendatud teenuse, kuvatakse leht alustamine brauseris. Saate läbi vaadata soovitatud järgmised toimingud ja koodi näited või viited, mis on lisatud projekti, ja kuidas muudeti kood ja konfiguratsiooni failide kuvamiseks on kadunud lehe kuvamiseks.

1. Kasutades koodi näidised juhendina, hakake kood juurdepääsu oma mobiilsideteenuse!

## <a name="how-your-project-is-modified"></a>Kuidas oma projekti on muudetud

Kuidas muudab Visual Studio projekti sõltub projekti tüüp. C# kliendi rakenduste näha, [mis juhtus – C# projektid](http://go.microsoft.com/fwlink/p/?LinkId=513119). JavaScripti kliendi rakenduste näha, [mis juhtus – JavaScripti projektid](http://go.microsoft.com/fwlink/p/?LinkId=513120). Rakenduste Cordova näha, [mis juhtus – Cordova projektid](http://go.microsoft.com/fwlink/p/?LinkId=513116).


##<a name="next-steps"></a>Järgmised sammud

Küsimuste esitamine ja abi. 

 - [MSDN-i Foorum: Azure'i mobiilsideseadmete teenused](https://social.msdn.microsoft.com/forums/azure/home?forum=azuremobile)

 - [Azure'i mobiilsideseadmete teenuste veebisaidil Microsoft Azure'i meeskonna ajaveeb](https://azure.microsoft.com/blog/topics/mobile/)

 - [Azure'i Mobile teenuste azure.microsoft.com](https://azure.microsoft.com/services/mobile-services/)

 - [Azure'i Mobile teenuste dokumentatsiooni azure.microsoft.com](https://azure.microsoft.com/documentation/services/mobile-services/)



