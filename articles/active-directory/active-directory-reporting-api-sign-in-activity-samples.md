<properties
    pageTitle="Azure Active Directory sisselogimise tegevuste aruande API näidised | Microsoft Azure'i"
    description="Kuidas alustada Azure Active Directory aruannete API"
    services="active-directory"
    documentationCenter=""
    authors="dhanyahk"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/25/2016"
    ms.author="dhanyahk;markvi"/>

# <a name="azure-active-directory-sign-in-activity-report-api-samples"></a>Azure Active Directory sisselogimise tegevuste aruande API näidised

Selles teemas on osa teemade kohta aruannete API Azure Active Directory kogum.  
Azure'i AD aruandlus annab teile API, mis võimaldab juurdepääsu sisse andmete koodi või seotud tööriistade abil.  
Selles teemas on teile proovi kood **sisselogimise tegevuste API**.

Järgmistest teemadest.

- [Auditilogide](active-directory-reporting-azure-portal.md#audit-logs) kontseptuaalne Lisateavet
- Lisateavet aruannete API [Azure Active Directory aruannete API töötamise alustamine](active-directory-reporting-api-getting-started.md) .

Küsimused, probleemid või tagasiside, pöörduge [AAD aruandlus aitab](mailto:aadreportinghelp@microsoft.com).


## <a name="prerequisites"></a>Eeltingimused
Enne kui saate selles teemas näidised, peate täitma [eeltingimused Azure AD, aruannete API juurdepääsu](active-directory-reporting-api-prerequisites.md).  


##<a name="powershell-script"></a>PowerShelli skripti

    # This script will require the Web Application and permissions setup in Azure Active Directory
    $ClientID       = "<clientId>"             # Should be a ~35 character string insert your info here
    $ClientSecret   = "<clientSecret>"         # Should be a ~44 character string insert your info here
    $loginURL       = "https://login.windows.net/"
    $tenantdomain   = "<tenantDomain>"
    $ daterange            # For example, contoso.onmicrosoft.com

    $7daysago = "{0:s}" -f (get-date).AddDays(-7) + "Z"
    # or, AddMinutes(-5)

    Write-Output $7daysago

    # Get an Oauth 2 access token based on client id, secret and tenant domain
    $body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}

    $oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

    if ($oauth.access_token -ne $null) {
    $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}

    $url = "https://graph.windows.net/$tenantdomain/activities/signinEvents?api-version=beta&`$filter=signinDateTime ge $7daysago"
    
    $i=0
    
    Do{
        Write-Output "Fetching data using Uri: $url"
        $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)
        Write-Output "Save the output to a file SigninActivities$i.json"
        Write-Output "---------------------------------------------"
        $myReport.Content | Out-File -FilePath SigninActivities$i.json -Force
        $url = ($myReport.Content | ConvertFrom-Json).'@odata.nextLink'
        $i = $i+1
    } while($url -ne $null)

    } else {
    
        Write-Host "ERROR: No Access Token"
    }




## <a name="executing-the-script"></a>Skripti täitmine
Kui skripti redigeerimise lõpetanud, käivitage see ja veenduge, et Audit oodatud andmeid logib aruande tagastatakse.

Skripti tagastab väljundi sisselogimise aruande JSON-vormingus. See loob ka mõne `SigninActivities.json` sama väljundi faili. Saate proovida muutes skripti muudest aruandeid ja kommentaari väljundvormingusse, et teil pole vaja välja andmete tagastamiseks.



## <a name="next-steps"></a>Järgmised sammud

- Kas soovite kohandada selles teemas näidised? Lugege teemat [Azure Active Directory sisselogimise tegevuste API viide](active-directory-reporting-api-sign-in-activity-reference.md). 

- Kui soovite näha täieliku ülevaate aruannete API Azure Active Directory abil, vt [Alustamine Azure Active Directory aruannete API](active-directory-reporting-api-getting-started.md).

- Kui soovite rohkem teada Azure Active Directory aruandlus, lugege teemat [Azure Active Directory aruandluse juhend](active-directory-reporting-guide.md).  