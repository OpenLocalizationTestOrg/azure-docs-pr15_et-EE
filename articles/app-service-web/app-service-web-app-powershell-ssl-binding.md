<properties
    pageTitle="SSL-sertide sidumine PowerShelli abil"
    description="Saate teada, kuidas SSL-sertide sidumiseks oma veebirakenduse PowerShelli kaudu."
    services="app-service\web"
    documentationCenter=""
    authors="ahmedelnably"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="01/13/2016"
    ms.author="ahmedelnably"/>

# <a name="azure-app-service-ssl-certificate-binding-using-powershell"></a>Azure'i rakenduse teenuse SSL-i serdi sidumine PowerShelli abil #

Väljaandes Microsoft Azure'i PowerShelli versiooni 1.1.0 on lisatud uued cmdlet, mis annaks kasutaja võimalus olemasolevad või uued SSL-sertide sidumiseks mõne olemasoleva Web App.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

Lugege teemat Azure ressursihaldur vastavalt Azure PowerShelli cmdlet-käskude oma veebirakenduste haldamine märkige [Azure'i ressursihaldur vastavalt PowerShelli käske Azure Web App](app-service-web-app-azure-resource-manager-powershell.md)

## <a name="uploading-and-binding-a-new-ssl-certificate"></a>Üles- ja Köitmine uue SSL-sert ##

Stsenaarium: Kasutaja soovib SSL-serdi sidumiseks oma web apps.

Ressursi rühma nime, mis sisaldab veebirakenduse, web rakenduse nime, serdi pfx faili tee kasutaja arvutisse, parooli serti ja kohandatud hostname teab, saame kasutada PowerShelli käsk selle SSL-i sidumine loomiseks:

    New-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -CertificateFilePath PathToPfxFile -CertificatePassword PlainTextPwd -Name www.contoso.com

Pange tähele, et enne lisada SSL-i sidumine web appi, peab olema juba konfigureeritud hostinimi (kohandatud Domeen). Kui serveri nimi pole konfigureeritud, siis kuvatakse järgmine tõrketeade "hostname" pole New-AzureRmWebAppSSLBinding käitamise ajal. Hostinimi saate lisada portaali või Azure PowerShelli kaudu. Järgmine PowerShelli koodilõigu saab konfigureerida hostname enne käivitamist New-AzureRmWebAppSSLBinding.   
  
    $webApp = Get-AzureRmWebApp -Name mytestapp -ResourceGroupName myresourcegroup  
    $hostNames = $webApp.HostNames  
    $HostNames.Add("www.contoso.com")  
    Set-AzureRmWebApp -Name mytestapp -ResourceGroupName myresourcegroup -HostNames $HostNames   
  
On oluline, et aru saada, et cmdlet Set-AzureRmWebApp kirjutab hostinimed web app. Seega on ülaltoodud PowerShelli koodilõigu lisamine hostinimed web appi olemasolevat loendit.  

## <a name="uploading-and-binding-an-existing-ssl-certificate"></a>Üles- ja Köitmine olemasoleva SSL-sert ##

Stsenaarium: Kasutaja soovib varem üleslaaditud SSL-serdi sidumiseks oma web apps.

Saame loendit serdid juba üleslaaditud mõne kindla ressursirühma järgmise käsu abil

    Get-AzureRmWebAppCertificate -ResourceGroupName myresourcegroup

Tähele, et serdid on teatud asukohta ja ressursirühm kohaliku, kasutaja on vaja uuesti laadige üles sert, kui mõnes muus asukohas on konfigureeritud web appi ja ressursside rühma muud, mida on vaja serdi 

Ressursi rühma nime sisaldav veebirakenduse, web rakenduse nime, serdi sõrmejälje ja kohandatud hostname teab, saame kasutada PowerShelli käsk selle SSL-i sidumine loomiseks:

    New-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Thumbprint <certificate thumbprint> -Name www.contoso.com

## <a name="deleting-an-existing-ssl-binding"></a>SSL-i olemasolevaid siduv kustutamine  ##

Stsenaarium: Kasutaja soovib olemasolevaid SSL-i siduv kustutada.

Ressursi rühma nime, mis sisaldab veebirakenduse, web rakenduse nime ja kohandatud hostname teab, saame kasutada PowerShelli käsk eemaldada, et SSL-i sidumine:

    Remove-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Name www.contoso.com

Pange tähele, et kui eemaldatud SSL-i sidumine oli selle serdi abil, et asukohta, vaikimisi serdi kustutatakse, kui kasutaja soovi säilitada ta saate säilitada serdi DeleteCertificate suvandi serdi viimase sidumine

    Remove-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Name www.contoso.com -DeleteCertificate $false

### <a name="references"></a>Viited ###
- [Azure'i ressursihaldur põhjal PowerShelli käske Azure Web App](app-service-web-app-azure-resource-manager-powershell.md)
- [Sissejuhatus rakenduse teenuse keskkonnas](app-service-app-service-environment-intro.md)
- [Azure'i ressursihaldur Azure PowerShelli abil](../powershell-azure-resource-manager.md)
