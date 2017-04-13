<properties 
   pageTitle="Azure Active Directory lisamise ühendatud teenuste kasutamisel Visual Studio | Microsoft Azure'i"
   description="Lisa Azure Active Directory Visual Studio ühendatud teenuste lisamine dialoogiboksi kaudu"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="active-directory"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="adding-an-azure-active-directory-by-using-connected-services-in-visual-studio"></a>Azure Active Directory abil ühendatud teenuste Visual Studio lisamine 

##<a name="overview"></a>Ülevaade
Azure Active Directory (Azure AD) abil saate ASP.net-i MVC veebirakenduste või AD autentimise Veebiteenuste teenused toetavad ühekordse sisselogimise (SSO). Azure'i AD-autentimine, saate kasutajate Azure AD kaudu oma kontoga ühenduse loomiseks oma veebirakendusi. Kui asetades API veebirakenduse kaudu, näiteks Azure AD autentimise Web API-ga eelised täiustatud turvalisus. Azure AD, kus on eraldi autentimise süsteemi oma konto ja kasutajale haldamise haldamiseks.

## <a name="supported-project-types"></a>Toetatud projekti tüübid

Saate projekti järgmiste Azure AD ühenduse ühendatud teenuste dialoogiboksi.

- ASP.net-i MVC projektid

- ASP.net-i veebi-API projektid


### <a name="connect-to-azure-ad-using-the-connected-services-dialog"></a>Azure AD ühendatud teenuste dialoogiboksis ühenduse loomine

1. Veenduge, et olete Azure'i konto. Kui teil pole Azure'i konto, te saate registreeruda [tasuta prooviversioon](http://go.microsoft.com/fwlink/?LinkId=518146).

1. Visual Studios, projekti sõlm **Viited** kiirmenüü avamine ja valige **Lisa ühendatud teenuseid**.
1. Valige **Azure AD autentimist** ja klõpsake nuppu **Konfigureeri**.

    ![Valige lisada Azure AD autentimine](./media/vs-azure-tools-connected-services-add-active-directory/connected-services-add-active-directory.png)

1. Esilehekülje **konfigureerimine Windows Azure AD autentimist**, märkige ruut **konfigureerimine ühekordse sisselogimise kasutamine Azure AD**.

    Kui projekti on konfigureeritud teise autentimise konfigureerimine, viisardi hoiatab teid, et jätkuva keelata eelmise konfiguratsiooni.

    ![Viisardi Azure AD konfigureerimine](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-1.png)

1.  Teisel lehel Valige Domeen ripploendist **domeeni** kaudu. Domeenide loend sisaldab kõiki domeene puuetega inimestele juurdepääsetavate dialoogiboksis konto sätted loetletud kontod. Teise võimalusena saate sisestada domeeninime, kui te ei leia see, mida otsite, nt mydomain.onmicrosoft.com. Saate valida suvandi abil saate luua uue Azure AD Rakenduse või seadistada olemasoleva Azure AD Rakenduse kaudu. 

    ![Viisardi Azure AD konfigureerimine](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-2.png)


1. Viisardi lehel kolmanda veenduge, et **kataloogi andmeid lugeda** märgitud. Täitke viisardi **Kliendi salajane**. 

    ![Viisardi Azure AD konfigureerimine](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-3.png)

1. Klõpsake nuppu **valmis** . Dialoogiboks lisab vajalikud konfigureerimiskoodi ja viited projekti Azure AD autentimise lubamiseks. Saate vaadata AD domeeni [Azure portaali](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Vaadake üle järgmised toimingud ideid brauseris kuvataval lehel alustamine ja kadunud leht, et näha, kuidas projekti muutmise. Kui soovite kontrollida, kas kõik töötas, avage üks muudetud failid ja veenduge, et sätted, mis on juhtunud mainitud on seal. ASP.net-i MVC projekti peamised web.config on näiteks neid sätteid, mis on lisatud:

        <appSettings> 
            <add key="ida:ClientId" value="ClientId from the new Azure AD App" />
            <add key="ida:AADInstance" value="https://login.windows.net/" />
            <add key="ida:Domain" value="Your selected domain" />
            <add key="ida:TenantId" value="The Id of your selected Azure AD Tenant" />
            <add key="ida:PostLogoutRedirectUri" value="The default redirect URI from the project" />
        </appSettings>

## <a name="how-your-project-is-modified"></a>Kuidas oma projekti on muudetud

Viisardi käivitamisel Visual Studio lisab Azure AD ja seotud viited projekti. Failid ja projekti kood failide lisamine tugi Azure AD ka muudetud. Teatud muudatused, mis toodab Visual Studio sõltuvad projekti tüüp. Üksikasjalikku teavet selle kohta, kuidas ASP.net-i MVC projektide on muudetud, lugege teemat [Mis juhtus – MVC projektid](http://go.microsoft.com/fwlink/p/?LinkID=513809). Veebi-API projektide teemast [kadunud – veebi-API projektid](http://go.microsoft.com/fwlink/p/?LinkId=513810).

##<a name="next-steps"></a>Järgmised sammud

Esitada küsimusi ja saada abi.

 - [MSDN-i Foorum: Azure'i AD](https://social.msdn.microsoft.com/forums/azure/home?forum=WindowsAzureAD)

 - [Azure'i AD-dokumendid](https://azure.microsoft.com/documentation/services/active-directory/)

 - [Ajaveebipostitus: Azure'i AD Sissejuhatus](http://blogs.msdn.com/b/brunoterkaly/archive/2014/03/03/introduction-to-windows-azure-active-directory.aspx)

