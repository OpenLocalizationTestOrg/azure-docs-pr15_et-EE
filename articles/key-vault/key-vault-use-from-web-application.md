<properties
    pageTitle="Azure'i võtme Vault veebirakenduse kaudu kasutada | Microsoft Azure'i"
    description="Selle õpetuse abil saate teada, kuidas kasutada Azure klahvi Vault veebirakenduse kaudu."
    services="key-vault"
    documentationCenter=""
    authors="adhurwit"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/05/2016"
    ms.author="adhurwit"/>

# <a name="use-azure-key-vault-from-a-web-application"></a>Kasutage Azure võtme Vault veebirakenduse kaudu #

## <a name="introduction"></a>Sissejuhatus  
Selle õpetuse abil saate teada, kuidas kasutada Azure klahvi Vault veebirakenduse Azure. See juhatab teid läbi protsessi juurdepääs salajase Azure'i klahvi hoidlast nii, et seda saab kasutada oma veebirakenduse.

**Hinnanguline aega:** 15 minutit


Azure'i klahvi Vault kohta ülevaate leiate teemast [mis on Azure klahvi Vault?](key-vault-whatis.md)

## <a name="prerequisites"></a>Eeltingimused

Selle õpetuse lõpuleviimiseks peab olema järgmised:

- Mis on Azure klahvi Vault salajase URI
- Kliendi ID ja kliendi salajane veebirakenduse jaoks registreeritud Azure Active Directory, kellel on juurdepääs teie võti Vault
- Veebirakenduse. Näitame juurutatud Azure Web App rakenduse ASP.net-i MVC juhised.

> [AZURE.NOTE]  On oluline, et olete täitnud toodud juhised [Alustamine Azure'i klahvi Vault](key-vault-get-started.md) selles õpetuses mõeldud nii, et teil on salajane ja kliendi ID ja kliendi salajane URI veebirakenduse.

Veebirakenduse, mida saab juurdepääsu võti Vault on üks, mis on registreeritud Azure Active Directory ja on antud juurdepääs teie võti Vault. Kui see ei ole, minge tagasi Register õpetusega alustamine rakenduses ja korrake juhiseid loetletud.

Selles õpetuses on mõeldud veebi arendajad, et põhialuste Azure veebirakenduste loomisega. Azure'i Web Apps kohta leiate lisateavet teemast [veebirakenduste ülevaade](../app-service-web/app-service-web-overview.md).



## <a id="packages"></a>Lisage Nugeti paketid ##
On kaks pakette, mis oma veebirakenduse peab olema installitud.

- Active Directory Authentication Library - sisaldab suheldes Azure Active Directory ja kasutajale identiteedi haldamine
- Azure'i klahvi Vault teek - sisaldab suheldes Azure'i klahvi Vault meetodid


Nii need paketid installimist Package Manager konsooli käsk Installi pakett abil.

    // this is currently the latest stable version of ADAL
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.16.204221202

    Install-Package Microsoft.Azure.KeyVault


## <a id="webconfig"></a>Web.Config muutmine ##
On kolme rakenduse sätted, mida tuleb lisada fail järgmiselt.

    <!-- ClientId and ClientSecret refer to the web application registration with Azure Active Directory -->
    <add key="ClientId" value="clientid" />
    <add key="ClientSecret" value="clientsecret" />

    <!-- SecretUri is the URI for the secret in Azure Key Vault -->
    <add key="SecretUri" value="secreturi" />


Kui te ei kavatse majutada kui Azure Web App rakenduse, siis tuleks lisada tegelike väärtuste ClientId, kliendi salajane ja salajane URI web.config. Muul juhul jätta need fiktiivne väärtused, kuna lisame tegelike väärtuste Azure'i portaalis täiendavad taseme turvalisus.


## <a id="gettoken"></a>Meetod, et saada juurdepääsu Turbeloa lisamine ##
API võti Vault kasutamiseks peab olema juurdepääsu sümboolse. Klahv Vault kliendi käsitleb kõnesid API võti Vault, kuid peate esitama selle funktsiooniga, mis toob juurdepääsu luba.  

Järgmine on saada juurdepääsu Turbeloa Azure Active Directory kood. Järgmine kood minna oma rakenduse suvalist kohta. Meeldib Utils või EncryptionHelper klassi lisada.  

    //add these using statements
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System.Threading.Tasks;
    using System.Web.Configuration;

    //this is an optional property to hold the secret after it is retrieved
    public static string EncryptSecret { get; set; }

    //the method that will be provided to the KeyVaultClient
    public static async Task<string> GetToken(string authority, string resource, string scope)
    {
        var authContext = new AuthenticationContext(authority);
        ClientCredential clientCred = new ClientCredential(WebConfigurationManager.AppSettings["ClientId"],
                    WebConfigurationManager.AppSettings["ClientSecret"]);
        AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

        if (result == null)
            throw new InvalidOperationException("Failed to obtain the JWT token");

        return result.AccessToken;
    }

> [AZURE.NOTE] 
> Kliendi ID ja kliendi salajane abil on on kõige lihtsam viis rakenduse Azure AD autentida. Ja kasutamine veebirakenduses võimaldab eraldamine ülesannete ja teie olulisi juhtimise üle rohkem kontrolli. Kuid see toetuvad kasutusele kliendi salajane konfiguratsioonisätted, mis mõne jaoks võib olla nii ohtlik nagu lihtsate salajane, mida soovite kaitsta sätete konfigureerimine. Arutelu Kliendiid ja serdi kasutamise asemel kliendi ID ja kliendi salajane autentida Azure AD Rakenduse allpool.



## <a id="appstart"></a>Saate tuua salajane klõpsake rakenduse käivitamine ##
Nüüd tuleb helistada Vault API võti ja tuua salajane kood. Kui seda nimetatakse enne, kui peate kasutama seda saaks suvalist järgmine kood. Mul on pandud järgmine kood on Global.asax sündmuse rakendust käivitada, et see käivitatakse üks kord start ja teeb salajane rakenduse jaoks saadaval.

    //add these using statements
    using Microsoft.Azure.KeyVault;
    using System.Web.Configuration;

    // I put my GetToken method in a Utils class. Change for wherever you placed your method.
    var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));

    var sec = kv.GetSecretAsync(WebConfigurationManager.AppSettings["SecretUri"]).Result.Value;

    //I put a variable in a Utils class to hold the secret for general  application use.
    Utils.EncryptSecret = sec;



## <a id="portalsettings"></a>Rakenduse sätete lisamine Azure'i portaalis (valikuline) ##
Kui teil on Azure Web App saate lisada nüüd tegelike väärtuste jaoks AppSettings Azure'i portaalis. Seda tehes tegelike väärtuste ei ole Web.config, kuid kui teil on eraldi juurdepääsu juhtimine võimaluste portaali kaudu kaitstud. Need väärtused asendab teie web.config sisestatud väärtused. Veenduge, et nimed on samad.

![Rakenduse sätted kuvatakse Azure'i portaalis][1]


## <a name="authenticate-with-a-certificate-instead-of-a-client-secret"></a>Kliendi salajane asemel sertifikaadiga autentida
Teine võimalus autentida Azure AD rakendus on kliendi ID ja kliendi salajane asemel kliendi ID ja serdi abil. Järgnevalt on toiminguid, mida tuleb kasutada serti Azure Web Appis.

1. Saada või serdi loomine
2. Serdi seostamine rakenduse Azure AD
3. Serdi kasutama oma veebirakenduse koodi lisamine
4. Serdi lisamine oma veebirakenduse


**Saada või serdi loomine** Meie eesmärkide puhul teeme testserdi. Siin on paar käskude, mille abil saate arendaja Käsuviip serdi loomine. Muuta kataloogi, kuhu soovite cert faile luua.

    makecert -sv mykey.pvk -n "cn=KVWebApp" KVWebApp.cer -b 07/31/2015 -e 07/31/2016 -r
    pvk2pfx -pvk mykey.pvk -spc KVWebApp.cer -pfx KVWebApp.pfx -po test123

Märkige üles asuva lõppkuupäeva ja selle pfx parool (selles näites: 07/31/2016 ja test123). Te peate neid allpool.

Testserdi loomise kohta leiate lisateavet teemast [kohta: luua teie enda testimiseks serdi](https://msdn.microsoft.com/library/ff699202.aspx)


**Serdi rakendusega Azure AD seostada.** Nüüd, kui teil on sert, peate seostada rakenduse Azure AD. Kuid Azure'i haldusportaal ei toeta seda kohe. Selle asemel tuleb PowerShelli kasutamine. Järgnevalt on käske, mida soovite käivitada.

    $x509 = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2

    PS C:\> $x509.Import("C:\data\KVWebApp.cer")

    PS C:\> $credValue = [System.Convert]::ToBase64String($x509.GetRawCertData())

    PS C:\> $now = [System.DateTime]::Now

    # this is where the end date from the cert above is used
    PS C:\> $yearfromnow = [System.DateTime]::Parse("2016-07-31")

    PS C:\> $adapp = New-AzureRmADApplication -DisplayName "KVWebApp" -HomePage "http://kvwebapp" -IdentifierUris "http://kvwebapp" -KeyValue $credValue -KeyType "AsymmetricX509Cert" -KeyUsage "Verify" -StartDate $now -EndDate $yearfromnow

    PS C:\> $sp = New-AzureRmADServicePrincipal -ApplicationId $adapp.ApplicationId

    PS C:\> Set-AzureRmKeyVaultAccessPolicy -VaultName 'contosokv' -ServicePrincipalName $sp.ServicePrincipalName -PermissionsToSecrets all -ResourceGroupName 'contosorg'

    # get the thumbprint to use in your app settings
    PS C:\>$x509.Thumbprint

Kui käivitate need käsud, saate vaadata rakenduse Azure AD. Kui te ei näe rakenduse esmalt otsing "Minu ettevõte kuulub rakendused" asemel "Rakenduste minu ettevõte kasutab".

Azure'i AD Rakenduse objektid ja ServicePrincipal objektide kohta leiate lisateavet teemast [rakenduse objektid ja teenuse põhilise objektid](../active-directory/active-directory-application-objects.md)



**Serdi kasutama oma veebirakenduse koodi lisamine** Nüüd lisame kood oma veebirakenduse cert vaadata ja kasutada seda autentimist.

Esmalt on kood cert juurdepääsu.

    public static class CertificateHelper
    {
        public static X509Certificate2 FindCertificateByThumbprint(string findValue)
        {
            X509Store store = new X509Store(StoreName.My, StoreLocation.CurrentUser);
            try
            {
                store.Open(OpenFlags.ReadOnly);
                X509Certificate2Collection col = store.Certificates.Find(X509FindType.FindByThumbprint,
                    findValue, false); // Don't validate certs, since the test root isn't installed.
                if (col == null || col.Count == 0)
                    return null;
                return col[0];
            }
            finally
            {
                store.Close();
            }
        }
    }


Pöörake tähelepanu sellele, et StoreLocation CurrentUser asemel LocalMachine. Ja, et me varustavad "false" Otsi meetod, kuna me kasutame test cert.


On järgmine kood, mis kasutab funktsiooni CertificateHelper ja loob ClientAssertionCertificate, mida on vaja autentimist.

    public static ClientAssertionCertificate AssertionCert { get; set; }

    public static void GetCert()
    {
        var clientAssertionCertPfx = CertificateHelper.FindCertificateByThumbprint(WebConfigurationManager.AppSettings["thumbprint"]);
        AssertionCert = new ClientAssertionCertificate(WebConfigurationManager.AppSettings["clientid"], clientAssertionCertPfx);
    }


Siin on uue koodi saada juurdepääsu luba. See asendab eeltoodud meetodi GetToken. On andnud selle mugavuse mõni muu nimi.

    public static async Task<string> GetAccessToken(string authority, string resource, string scope)
    {
        var context = new AuthenticationContext(authority, TokenCache.DefaultShared);
        var result = await context.AcquireTokenAsync(resource, AssertionCert);
        return result.AccessToken;
    }

Mul on pandud kõik järgmine kood oma veebirakenduse projekti Utils klassi hõlpsamaks kasutamiseks.

Viimase koodi muutmine on Application_Start meetod. Kõigepealt on vaja GetCert() meetod on ClientAssertionCertificate laadimiseks. Ja tagasihelistamise meetodit, kui loote uue KeyVaultClient pakume muutke. Pange tähele, et see asendab koodi, mis oli meil kohal.

    Utils.GetCert();
    var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetAccessToken));


**Azure'i portaali kaudu oma veebirakenduse serdi lisamine** Serdi lisamine oma veebirakenduse on lihtne kontrollimiseks. Esmalt minge Azure'i portaal ja liikuge oma veebirakenduse. Veebirakenduse jaoks enne sätted, klõpsake kirje "kohandatud domeenid ja SSL-i". Avaneb enne saab üles laadida ülaltoodud KVWebApp.pfx loodud sert, veenduge, et te ei mäleta selle pfx parool.

![Serdi lisamine Web Appi Azure'i portaalis][2]


Viimane asi, mida peate tegema on mõne sätte rakenduse lisamiseks oma veebirakenduse, mis sisaldab nime veebisaidi\_laadi\_SERDID ja väärtus *. See tagab, et kõik serdid on laaditud. Kui soovite laadida serdid, mida olete alla laadinud, saate sisestada oma thumbprints komaga eraldatud loend.

Serdi lisamine Web Appi kohta leiate lisateavet teemast [Azure veebisaitide rakendustes serdid abil](https://azure.microsoft.com/blog/2014/10/27/using-certificates-in-azure-websites-applications/)


**Klahv Vault nimega salajase serdi lisamine** Asemel üles oma serdi veebiversiooni teenust otse, saate salvestada selle klahvi võlvkelder nimega salajase ja juurutada seda seal. See on kaheosaline toiming, mis on esitatud Järgmine ajaveebipostitus [Juurutamine Azure Web Appi serdi kaudu klahvi Vault](https://blogs.msdn.microsoft.com/appserviceteam/2016/05/24/deploying-azure-web-app-certificate-through-key-vault/)



## <a id="next"></a>Järgmised sammud ##


Viited kavandamiseks kohta leiate teemast [Azure klahvi Vault C# kliendi API juhend](https://msdn.microsoft.com/library/azure/dn903628.aspx).


<!--Image references-->
[1]: ./media/key-vault-use-from-web-application/PortalAppSettings.png
[2]: ./media/key-vault-use-from-web-application/PortalAddCertificate.png
