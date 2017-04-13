<properties 
   pageTitle="Azure'i salvestusruumi lisamiseks ühendatud teenuste kasutamisel Visual Studio | Microsoft Azure'i"
   description="Azure'i salvestusruumi lisamine rakenduse Visual Studio ühendatud teenuste lisamine dialoogiboksi kaudu"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="storage"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="adding-azure-storage-by-using-visual-studio-connected-services"></a>Visual Studio ühendatud teenuste kasutamisel Azure salvestusruumi lisamine

## <a name="overview"></a>Ülevaade

Visual Studio 2015, kus saate luua ühenduse mis tahes C# pilveteenuses, .NET kirjutamata mobiilsideteenus, ASP.net-i veebisaidi või teenuse, ASP.net-i 5 teenuse või Azure'i WebJob teenuse Azure Storage **Ühendatud teenuste lisamine** dialoogiboksi kaudu. Ühendatud teenuse funktsioonidele lisab kõik vajalikud viited ja ühenduse kood ja muudab oma konfiguratsiooni faile õigesti. Dialoogiboksi suunab teid ka dokumentatsioon, mis ütleb teile, mis järgmised toimingud on alustamiseks bloobimälu, järjekorrad ja tabeleid.

## <a name="supported-project-types"></a>Toetatud projekti tüübid

Ühendatud teenuste dialoogiboksi abil saate luua ühenduse Azure Storage järgmisi.

- ASP.net-i Web projektid

- ASP.net-i 5 projektid

- .Net-i pilvepõhise teenuse Web rolli ja töötaja rolli projektid

- .Net-i mobiilsideseadmete teenuste projektid

- Azure'i WebJob projektid


## <a name="connect-to-azure-storage-using-the-connected-services-dialog"></a>Ühenduse loomine ühendatud teenuste dialoogiboksis Azure'i Tabelimäluga

1. Veenduge, et olete Azure'i konto. Kui teil pole Azure'i konto, te saate registreeruda [tasuta prooviversioon](http://go.microsoft.com/fwlink/?LinkId=518146). Kui olete Azure'i konto, saate luua salvestusruumi kontod, mobiil teenuste loomine ja konfigureerimine Azure Active Directory.

1. Avage oma projekti Visual Studios, avada kontekstimenüü **Viited** sõlm Solution Exploreris ja valige **Ühendatud teenuse lisamine**.

    ![Ühendatud teenuse lisamine](./media/vs-azure-tools-connected-services-storage/IC796702.png)

1. Dialoogiboksis **Lisa ühendatud teenuse** valimine **Azure Storage**ja seejärel klõpsake nuppu **Konfigureeri** . Teil palutakse Azure'i sisse logida, kui te pole seda juba teinud.

    ![Dialoogiboks ühendatud teenuse - salvestusruumi lisamine](./media/vs-azure-tools-connected-services-storage/IC796703.png)

1. Dialoogiboksis **Azure Storage** valige salvestusruumi konto ja valige **Lisa**.

    Kui teil on vaja salvestusruumi uue konto loomine, minge järgmise juhise juurde. Muul juhul jätkake 6.

    ![Azure'i dialoogiaknas](./media/vs-azure-tools-connected-services-storage/IC796704.png)

1. Mäluruumi uue konto loomiseks tehke järgmist. 

    1. Azure Storage dialoogiboksi allservas nuppu **Loo uus konto salvestusruumi** .

    1. Täitke dialoogiboksi **Salvestusruumi konto loomine** ja seejärel klõpsake nuppu **Loo** .
    
        ![Azure'i dialoogiaknas](./media/vs-azure-tools-connected-services-storage/create-storage-account.png)

        Kui olete tagasi dialoogiboksis **Azure Storage** , kuvatakse loendis uue salvestusruumi.

    1. Valige loendist uus salvestusruumi ja valige **Lisa**.

1. Jaotises teenuse viited WebJob projekti sõlm kuvatakse ühendatud salvestusruumi teenus.

    ![Azure'i salvestusruumi rakenduses project web tööde haldamine](./media/vs-azure-tools-connected-services-storage/IC796705.png)

1. Vaadake üle kuvataval lehel alustamine ja teada saada, kuidas oma projekti muutmise. Iga kord, kui lisate ühendatud teenuse, kuvatakse leht alustamine teie brauseris. Saate läbi vaadata soovitatud järgmised toimingud ja koodi näited või viited, mis on lisatud projekti, ja kuidas muudeti kood ja konfiguratsiooni failide kuvamiseks on kadunud lehe kuvamiseks.

## <a name="how-your-project-is-modified"></a>Kuidas oma projekti on muudetud

Kui olete lõpetanud, kuvatakse dialoogiboks, Visual Studio lisab viited ja muudab teatud failid. Teatud muudatused sõltuvad projekti tüüp. 

 - ASP.net-i projektide, vt [mis on juhtunud – ASP.net-i projektide](http://go.microsoft.com/fwlink/p/?LinkId=513126). 
 - ASP.net-i 5 projektide teemast [kadunud – ASP.net-i 5 projektid](http://go.microsoft.com/fwlink/p/?LinkId=513124). 
 - Pilveteenuse teenuse projekte (web rollid ja töötaja rolli), leiate [kadunud – pilveteenuses projektid](http://go.microsoft.com/fwlink/p/?LinkId=516965). 
 - WebJob projektide teemast [kadunud - WebJob projektid](./storage/vs-storage-webjobs-what-happened.md).

## <a name="next-steps"></a>Järgmised sammud

1. Kasutades juhend alustamine koodinäiteid, tüübi talletusruumi, mille soovite luua, ja alustage koodi salvestusruumi kontole!

1. Küsimuste esitamine ja abi saamine
     - [MSDN-i Foorum: Azure'i salvestusruum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata)

     - [Azure'i salvestusruumi meeskonna ajaveeb](http://blogs.msdn.com/b/windowsazurestorage/)

     - [Azure.microsoft.com salvestusruum](https://azure.microsoft.com/services/storage/)

     - [Salvestusruumi dokumentatsiooni azure.microsoft.com](https://azure.microsoft.com/documentation/services/storage/)

