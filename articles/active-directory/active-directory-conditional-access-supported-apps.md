<properties
    pageTitle="Azure Active Directory access tingimusvormingu reegleid kasutavad rakendused | Microsoft Azure'i"
    description="Tingimusvormingu juurdepääsu kontroll, kontrollib Azure Active Directory eritingimuste, kui ta kontrollib kasutaja ja rakenduse juurdepääsu lubamiseks."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="10/26/2016"
    ms.author="markvi"/>

# <a name="applications-that-use-conditional-access-rules-in-azure-active-directory"></a>Rakendustes, mis kasutavad tingimusvormingu juurdepääsureeglid Azure Active Directory 's

Tingimusvormingu juurdepääsureeglid toetatud Azure Active Directory (Azure AD) – ühendatud rakendused, eelnevalt integreeritud ühendatud tarkvara service (SaaS) rakenduste, rakendusi, mis kasutavad parooli Ühekordne sisselogimine (SSO), ärivaldkonna rakenduste ja rakendusi, mis kasutavad Azure AD Rakenduse puhverserveri nimega. Üksikasjalik loend, mille abil saate tingimusvormingu Accessi rakenduste, vt [teenuste lubatud tingimusvormingu juurdepääsu](active-directory-conditional-access-technical-reference.md#Services-enabled-with-conditional-access). Juurdepääsu töötab nii mobiil- ja lauaarvutite rakendusi, mis kasutavad kaasaegne autentimine. Selles artiklis käsitleme kuidas tingimusvormingu Accessi töötab mobiil- ja lauaarvutite rakendustes.

Kaasaegne autentimine kasutavates rakendustes saate kasutada Azure AD sisselogimise lehed. Sisselogimise leht, küsitakse kasutaja mitmikautentimise jaoks. Teade kuvatakse, kui kasutaja juurdepääs on blokeeritud. Kaasaegne autentimine on vaja seadme autentimiseks Azure AD, nii, et seade põhinev tingimusvormingu juurdepääsupoliitikaid hinnatakse.

See on oluline teada, millised rakendused saate kasutada Accessi tingimusvormingu reeglid ja toimingud, mida peate tegema turvamiseks muude rakenduste kirje punktide.

## <a name="applications-that-use-modern-authentication"></a>Rakendustes, mis kasutavad kaasaegne autentimine

> [AZURE.NOTE] Kui teil on juurdepääsu poliitika Azure AD, mis on samaväärne Teenusekomplektis Office 365, nii juurdepääsu poliitikate konfigureerimine. See kehtib, näiteks juurdepääsu poliitika Exchange Online'i ja SharePoint Online'i jaoks.

Järgmiste rakenduste tugi Office 365 ja muude Azure'i AD-ühendus teenuserakenduste juurdepääsu:

| Target teenus  | Platvorm  | Rakenduse                                                  |
|--------------|-----------------|----------------------------------------------------------------|
|Office 365 Exchange Online'is | Windows 10|E-posti ja kalendri/inimesed rakenduses Outlook 2016, Outlook 2013 (tänapäevane autentimisega), Skype'i ärirakenduse (tänapäevane autentimisega)|
|Office 365 Exchange Online'is| Windows 8.1, Windows 7 |Outlook 2016, Outlook 2013 (tänapäevane autentimisega), Skype'i ärirakenduse (tänapäevane autentimisega)|
|Office 365 Exchange Online'is|iOS ja Android|  Outlooki mobiilirakenduse kaudu|
|Office 365 Exchange Online'is|Mac OS X| Outlook 2016 mitmikautentimise ja asukoha ainult; tugiteenuste poliitika seadme vastavalt kavandatud tulevikus Skype'i ärirakenduse tugiteenuste ka tulevikus|
|Office 365 SharePoint Online|Windows 10| Office 2016 rakendustes, ühes kohas Office'i rakenduste Office 2013 (koos kaasaegne autentimine), OneDrive for Businessi rakenduse (järgmise genereerimine Sünkroonimiskliendi või NGSC) toetavad kavandatud tulevikus ka tulevikus SharePointi rakenduse tugi ka tulevikus Office rühmade tugi|
|Office 365 SharePoint Online|Windows 8.1, Windows 7|Office 2016 rakendusi Office 2013 (tänapäevane autentimisega), OneDrive for Businessi (Groove'i sünkroonimisrakenduse)|
|Office 365 SharePoint Online|iOS ja Android|  Office Mobile'i rakendused |
|Office 365 SharePoint Online|Mac OS X| Mitmikautentimise ja ainult; asukoha Office 2016 rakendustes seadme põhineva poliitika tugi ka tulevikus|
|Office 365 Yammeri|Windows 10, iOS ja Android | Office'i Yammeri rakenduses|
|Dynamics CRM-iga|Windows 10, Windows 8.1, Windows 7, iOS ja Android | Dynamics CRM-i rakendus|
|PowerBI teenus|Windows 10, Windows 8.1, Windows 7, iOS ja Android | PowerBI rakendus|
|Azure'i kaugjuhtimise rakenduses teenus|Windows 10, Windows 8.1, Windows 7, iOS-i, Androidi ja Mac OS X |Azure'i kaugjuhtimise rakenduses|
|Mis tahes minu rakendused rakendus teenus|Androidi ja iOS-i|Mis tahes minu rakendused rakendus teenus |


## <a name="applications-that-do-not-use-modern-authentication"></a>Rakendustes, mis kasutavad kaasaegne autentimine

Praegu peab teiste meetodite abil blokeerida juurdepääsu rakendused, mis ei kasuta kaasaegne autentimine. Juurdepääsureeglid apps, mis ei kasuta kaasaegne autentimine on jõustatud pole juurdepääsu. See on peamiselt tasu Exchange ja SharePoint juurdepääsu. Enamik varasemate versioonide rakendusi kasutada vanemat Accessi kontrolli protokolle.

### <a name="control-access-in-office-365-sharepoint-online"></a>Juurdepääsu haldamine Teenusekomplektis Office 365 SharePoint online'is
Cmdlet Set-SPOTenant abil saate keelata pärand Protokollid SharePointi juurdepääsu. Selle cmdlet-käsu abil saate takistada Office klientides-modernne autentimise Protokollid juurdepääsu SharePoint Online'i ressursid.

**Näide käsk**:    `Set-SPOTenant -LegacyAuthProtocolsEnabled $false`

### <a name="control-access-in-office-365-exchange-online"></a>Office 365 Exchange Online'i juurdepääsu haldamine

Exchange'i pakub kahte liiki protokollid. Vaadake üle järgmised suvandid ja seejärel valige poliitika, mis sobib teie asutuses.

-   **Exchange ActiveSynci**. Vaikimisi on jõustatud tingimusvormingu juurdepääsupoliitikaid mitmikautentimise ja asukoha jaoks Exchange ActiveSynci. Peate kaitsta nende teenuste konfigureerimisega Exchange ActiveSynci poliitika otse või blokeerides Exchange ActiveSynci Active Directory Federation Services (AD FS) reeglite abil.
-   **Pärand Protokollid**. Pärand Protokollid AD FS-i abil saate blokeerida. See blokeerib access vanemate Office'i kliendid, näiteks Office 2013 kaasaegne autentimine on lubatud, ilma et ja Office'i varasemad versioonid.


### <a name="use-ad-fs-to-block-legacy-protocol"></a>AD FS-i abil saate blokeerida pärand Protocol (protokoll)

Saate näiteks järgmised reeglid pärand protokolli juurdepääsu AD FS-i tasemel. Valige kaks levinud konfiguratsioone.

#### <a name="option-1-allow-exchange-activesync-and-allow-legacy-apps-but-only-on-the-intranet"></a>Variant 1: Luba Exchange ActiveSynci ja luba pärand rakendused, kuid ainult sisevõrgus

Rakendades järgmised kolm reeglid tuginedes poole usaldus Microsoft Office 365 identiteedi platvormi, Exchange ActiveSynci liikluse, ja brauseri ja kaasaegne autentimine liikluse AD FS-i, on juurdepääs. Pärand rakendused on blokeeritud soovitud Suhtevõrgu.

##### <a name="rule-1"></a>Reegli 1

    @RuleName = “Allow all intranet traffic”
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "true"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

##### <a name="rule-2"></a>Reegli 2

    @RuleName = “Allow Exchange ActiveSync ”
    c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value == "Microsoft.Exchange.ActiveSync"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

##### <a name="rule-3"></a>Reegli 3

    @RuleName = “Allow extranet browser and browser dialog traffic”
    c1:[Type == " http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

#### <a name="option-2-allow-exchange-activesync-and-block-legacy-apps"></a>Variant 2: Luba Exchange ActiveSynci ja blokeerida pärand rakendused

Rakendades järgmised kolm reeglid tuginedes poole usaldus Microsoft Office 365 identiteedi platvormi, Exchange ActiveSynci liikluse, ja brauseri ja kaasaegne autentimine liikluse AD FS-i, on juurdepääs. Pärand rakendused on blokeeritud mis tahes asukohas.

##### <a name="rule-1"></a>Reegli 1

    @RuleName = “Allow all intranet traffic only for browser and modern authentication clients”
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "true"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");


##### <a name="rule-2"></a>Reegli 2

    @RuleName = “Allow Exchange ActiveSync”
    c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value == "Microsoft.Exchange.ActiveSync"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");


##### <a name="rule-3"></a>Reegli 3

    @RuleName = “Allow extranet browser and browser dialog traffic”
    c1:[Type == " http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");
