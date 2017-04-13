<properties
   pageTitle="Azure'i teenuse PowerShelliga põhisumma loomine | Microsoft Azure'i"
   description="Selles artiklis kirjeldatakse Azure PowerShelli abil saate luua Active Directory rakendus ja teenuse põhilise, ja anda selle juurdepääsu abil Rollipõhine juurdepääsu reguleerimine. See näitab, kuidas rakenduse parooli või serdiga autentida."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="09/12/2016"
   ms.author="tomfitz"/>

# <a name="use-azure-powershell-to-create-a-service-principal-to-access-resources"></a>Azure'i PowerShelli abil saate luua teenuse põhilise juurdepääs ressurssidele

> [AZURE.SELECTOR]
- [PowerShelli](resource-group-authenticate-service-principal.md)
- [Azure'i CLI](resource-group-authenticate-service-principal-cli.md)
- [Portaal](resource-group-create-service-principal-portal.md)

Kui teil on rakenduse või skripti, mida vajab ressursse, siis tõenäoliselt soovite käivitada oma mandaadi alusel importimistoimingut. Teil võib olla teistsugused õigused, mida soovite rakenduse ja te ei soovi rakenduse edasi kasutada oma kasutajanimi ja parool, kui teie kohustused muuta. Selle asemel saate luua rakenduse, mis sisaldab autentimiseks mandaadi ja rollimääranguid identiteedi. Iga kord, kui rakendus töötab, see autendib ise neid mandaate. Selles teemas näidatakse, kuidas kõike, mida vajate oma mandaadi ja identiteedi käivituma rakenduse häälestamine [Azure PowerShelli](powershell-install-configure.md) abil.

PowerShelli abil, on teil kaks võimalust autentimiseks AD Rakenduse:

 - parooli
 - sert

Selles teemas kirjeldatakse, kuidas kasutada mõlema variandi PowerShellis. Kui kavatsete jätta programmide (nt Python, Ruby või Node.js) Azure'i sisse logida, võib Paroolautentimine oma parim valik. Otsustada, kas kasutada parooli või serti jaotisest [valimi rakenduste](#sample-applications) näiteid erinevate raamistikku autentimist.

## <a name="active-directory-concepts"></a>Active Directory mõisted

Selles artiklis saate luua kaks objekti – Active Directory (AD) rakenduste ja teenuste põhisumma. AD rakendus on globaalne kujutis rakenduse. See sisaldab identimisteabe (rakenduse id ja parool või sert). Peamine teenus on kohalik esindus rakenduse Active Directory. See sisaldab rolli määramine. Selles teemas käsitletakse ühe – rentniku rakenduse, kui rakendus on mõeldud ainult üks ettevõttes käivitada. Tavaliselt ühe – rentniku rakenduste jaoks kasutatavasse ärivaldkonna rakendusi, mis töötavad teie ettevõttes. Ühe-rentniku rakenduses, on üks AD Rakenduse ja teenuse ühe põhilise.

Võib tekkida – miks on vaja nii objektid? Seda moodust mõttekas rohkem kui mõelda mitme rentniku rakendused. Tavaliselt kasutate mitme rentniku rakenduste tarkvara-kui-a-service (SaaS) rakenduste, kus teie rakendus töötab paljudes erinevates tellimustes. Mitme rentniku rakenduste, on üks AD rakendus ja mitme teenuse põhisumma (üks iga Active Directory, mis annab rakendus). Mitme rentniku rakenduse vaadake [arendaja juhend autoriseerimine Azure'i ressursihaldur API-ga](resource-manager-api-authentication.md).

## <a name="required-permissions"></a>Vajalikke õigusi

Selles teemas lõpuleviimiseks peab teil olema nii teie Azure Active Directory ja Azure tellimuse piisavaid õigusi. Täpsemalt, peate olema võimalus luua rakenduse Active Directory ja teenuse põhilise rollile määrata. 

Teie Active Directory konto peab olema administraator (nt **Üldadministraatori** või **Kasutaja haldus**). Kui teie konto on määratud **kasutajale** , peate olema administraator tõsta teie õigused.

Teie tellimus on teie konto peab olema `Microsoft.Authorization/*/Write` antakse rolli [omanik](./active-directory/role-based-access-built-in-roles.md#owner) või administraatorirolli ja [kasutajale](./active-directory/role-based-access-built-in-roles.md#user-access-administrator) kaudu juurdepääsu. Kui teie konto on määratud **kaasautori** roll, saate tõrketeate, kui proovite teenuse põhilise rollile määrata. Uuesti oma tellimuse administraator peab teile andma piisavalt juurdepääsu.

Nüüd jätkake jaotise kas [parooli](#create-service-principal-with-password) või [serdi](#create-service-principal-with-certificate) autentimine.

## <a name="create-service-principal-with-password"></a>Teenuse parooliga põhisumma loomine

Selle jaotise juhiste abil:

- parooliga AD Rakenduse loomine
- teenuse põhilise loomine
- teenuse põhilise lugeja rolli määramine

Nende juhiste täitmiseks kiiresti, vt järgmised kolm cmdlet-käsud. 

     $app = New-AzureRmADApplication -DisplayName "{app-name}" -HomePage "https://{your-domain}/{app-name}" -IdentifierUris "https://{your-domain}/{app-name}" -Password "{your-password}"
     New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId
     New-AzureRmRoleAssignment -RoleDefinitionName Reader -ServicePrincipalName $app.ApplicationId.Guid

Vaatame järgmist lähemalt veendumaks, et teil mõista protsessi.

1. Logige sisse oma konto.

        Add-AzureRmAccount

1. Loo uus rakendus Active Directory, andes kuvatav nimi, URI, mis kirjeldab rakenduse, rakenduse ja oma rakenduse identiteedi parooli tuvastavat URI-d.

        $app = New-AzureRmADApplication -DisplayName "exampleapp" -HomePage "https://www.contoso.org/exampleapp" -IdentifierUris "https://www.contoso.org/exampleapp" -Password "<Your_Password>"

     Ühe-rentniku rakenduste, kuvatakse URI-d on kinnitatud.
     
     Kui teie kontol puuduvad [vajalikud õigused](#required-permissions) Active Directory, kuvatakse tõrketeade, mis näitab "Authentication_Unauthorized" või "kontekstis tellimust ei leitud".

1. Uurige uue rakenduse objekti. 

        $app
        
     Pange tähele eriti **ApplicationId** atribuut, mida on vaja teenuse põhisumma rollimääranguid, loomise ja hankimisega juurdepääsu luba.

        DisplayName             : exampleapp
        ObjectId                : c95e67a3-403c-40ac-9377-115fa48f8f39
        IdentifierUris          : {https://www.contoso.org/example}
        HomePage                : https://www.contoso.org
        Type                    : Application
        ApplicationId           : 8bc80782-a916-47c8-a47e-4d76ed755275
        AvailableToOtherTenants : False
        AppPermissions          : 
        ReplyUrls               : {}

2. Teenuse põhilise rakenduse läbimise rakenduste ID Active Directory rakenduse loomiseks.

        New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId

3. Teenuse põhisumma õiguste tellimuse andmine. Selles näites saate lisada teenuse põhilise **lugeja** rolli, mis annab õiguse lugeda kõik ressursid tellimuse. Muud rollid, lugege teemat [RBAC: sisseehitatud rollid](./active-directory/role-based-access-built-in-roles.md). **Andke** parameetri ette **ApplicationId** , mida kasutasite rakenduse loomisel. 

        New-AzureRmRoleAssignment -RoleDefinitionName Reader -ServicePrincipalName $app.ApplicationId.Guid

    Kui teie konto pole piisavaid õigusi määrata rolli, kuvatakse tõrketeade. Teade teie konto **on luba Microsoft.Authorization/roleAssignments/write' üle ulatus "/ tellimuste / {guid}" toimingu sooritamiseks**. 

See on õige! Oma AD rakenduste ja teenuste põhisumma häälestada. Järgmises jaotises kirjeldatakse, kuidas mandaati PowerShelli kaudu sisse logida. Kui soovite kasutada mandaadi kood rakenduse, saate liikuda [valimi rakendused](#sample-applications). 

### <a name="provide-credentials-through-powershell"></a>Mandaat PowerShelli kaudu

Nüüd, peate sisse logima rakenduse toimingute tegemiseks.

1. Käsu **Get-mandaadi** mandaat sisaldava **PSCredential** objekti loomine. Peate **ApplicationId** enne selle käsu seega veenduge, et teil on see saadaval kleepimiseks töötab.

        $creds = Get-Credential

2. Teil palutakse teil sisestada mandaat. Kasutajanime, kasutage **ApplicationId** , mida kasutasite rakenduse loomisel. Kasutage parooli, saate määratud konto loomisel.

     ![Sisestage kasutajanimi ja parool](./media/resource-group-authenticate-service-principal/arm-get-credential.png)

2. Iga kord, kui logite sisse teenuse põhi, peate kataloogi rentniku ID-d pakuvad AD Rakenduse. Rentniku on Active Directory eksemplar. Kui teil on ainult üks tellimus, saate kasutada:

        $tenant = (Get-AzureRmSubscription).TenantId
    
     Kui teil on mitu tellimust, määrake see tellimus, kus asub teie Active Directory. Lisateabe saamiseks lugege teemat [Kuidas Azure'i tellimused on seostatud Azure Active Directory](./active-directory/active-directory-how-subscriptions-associated-directory.md).

        $tenant = (Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId

4. Logige sisse nagu teenuse peamine, määrates, et see konto on teenuse põhilise ja mandaadi objekti. 

        Add-AzureRmAccount -Credential $creds -ServicePrincipal -TenantId $tenant
        
     Nüüd on kinnitatud, nagu põhisumma teenuse Active Directory rakendus, mida olete loonud.

### <a name="save-access-token-to-simplify-log-in"></a>Salvesta juurdepääsu luba lihtsustamiseks sisse logida

Vältimaks mandaatide selle teenuse põhisumma iga kord, kui seda on vaja sisse logida, saate salvestada juurdepääsu luba.

1. Praeguse juurdepääsu luba kasutada hiljem seansi, salvestage profiili.

        Save-AzureRmProfile -Path c:\Users\exampleuser\profile\exampleSP.json
        
     Avage eemaldatav profiil ja kontrollida selle sisu. Pange tähele, et see sisaldab juurdepääsu sümboolse. 
        
2. Selle asemel, et käsitsi uuesti sisse logida, laadige lihtsalt profiili.

        Select-AzureRmProfile -Path c:\Users\exampleuser\profile\exampleSP.json
        
> [AZURE.NOTE] Juurdepääsu luba on aegunud, nii, et salvestatud profiili abil toimib ainult seni, kuni luba on lubatud.
        
## <a name="create-service-principal-with-certificate"></a>Teenuse serdiga põhisumma loomine

Selle jaotise juhiste abil:

- iseallkirjastatud serdi loomine
- sertifikaadiga AD Rakenduse loomine
- teenuse põhilise loomine
- teenuse põhilise lugeja rolli määramine

Kiiresti teha järgmist Azure PowerShelli 2.0 Windows 10 ja Windows Server 2016 tehniline eelvaade, vt järgmised cmdlet-käsud. 

    $cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=exampleapp" -KeySpec KeyExchange
    $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())
    $app = New-AzureRmADApplication -DisplayName "exampleapp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.contoso.org/example" -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore
    New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId
    New-AzureRmRoleAssignment -RoleDefinitionName Reader -ServicePrincipalName $app.ApplicationId.Guid

Vaatame järgmist lähemalt veendumaks, et teil mõista protsessi. Selles artiklis kirjeldatakse ka kuidas ülesandeid täita Azure PowerShelli või operatsioonisüsteemi varasemate versioonide kasutamisel.

### <a name="create-the-self-signed-certificate"></a>Iseallkirjastatud serdi loomine

PowerShelli saadaval Windows 10 ja Windows Server 2016 tehniline eelvaade versioonil on värskendatud **New-SelfSignedCertificate** cmdlet iseallkirjastatud serdi loomiseks. Varasem operatsioonisüsteem on cmdlet-käsu New-SelfSignedCertificate, kuid see ei paku selles teemas vaja parameetrid. Selle asemel, tuleb teil importida mooduli serdi loomiseks. Selles teemas kirjeldatakse mõlema lähenemisel loomiseks peate operatsioonisüsteemi põhjal sert. 

- Kui teil on **Windows 10 ja Windows Server 2016 tehniline eelvaade**, käivitage järgmine käsk iseallkirjastatud serdi loomine. 

        $cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=exampleapp" -KeySpec KeyExchange
       
- Kui teil **ei ole Windows 10 ja Windows Server 2016 tehniline eelvaade**, peate alla laadida Microsoft Script Center [iseallkirjastatud serdi generaator](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6/) . Selle sisu ekstraktida ja vajate cmdlet importida.
     
        # Only run if you could not use New-SelfSignedCertificate
        Import-Module -Name c:\ExtractedModule\New-SelfSignedCertificateEx.ps1
    
     Seejärel Loo sert.
    
        $cert = New-SelfSignedCertificateEx -Subject "CN=exampleapp" -KeySpec "Exchange" -FriendlyName "exampleapp"

Teil on oma serti ja saate alustada oma AD Rakenduse loomine.

### <a name="create-the-active-directory-app-and-service-principal"></a>Active Directory rakenduste ja teenuste põhisumma loomine

1. Serdi võtmeväärtuse tuua.

        $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())

2. Azure'i kontosse sisse logida.

        Add-AzureRmAccount

3. Loo uus rakendus Active Directory, andes kuvatav nimi, URI, mis kirjeldab rakenduse, rakenduse ja oma rakenduse identiteedi parooli tuvastavat URI-d.

     Kui teil on Azure PowerShell 2.0 (August 2016 või uuem versioon), kasutage järgmine cmdlet-käsk:

        $app = New-AzureRmADApplication -DisplayName "exampleapp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.contoso.org/example" -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore      
    
    Kui teil on Azure PowerShelli 1.0, kasutage järgmist cmdletti:
    
        $app = New-AzureRmADApplication -DisplayName "exampleapp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.contoso.org/example" -KeyValue $keyValue -KeyType AsymmetricX509Cert  -EndDate $cert.NotAfter -StartDate $cert.NotBefore      
    
    Ühe-rentniku rakenduste, kuvatakse URI-d on kinnitatud.
    
    Kui teie kontol puuduvad [vajalikud õigused](#required-permissions) Active Directory, kuvatakse tõrketeade, mis näitab "Authentication_Unauthorized" või "kontekstis tellimust ei leitud".
        
    Uurige uue rakenduse objekti. 

        $app

    Pange tähele **ApplicationId** atribuut, mida on vaja teenuse põhisumma rollimääranguid, loomise ja Accessi sõned hankimine.

        DisplayName             : exampleapp
        ObjectId                : c95e67a3-403c-40ac-9377-115fa48f8f39
        IdentifierUris          : {https://www.contoso.org/example}
        HomePage                : https://www.contoso.org
        Type                    : Application
        ApplicationId           : 8bc80782-a916-47c8-a47e-4d76ed755275
        AvailableToOtherTenants : False
        AppPermissions          : 
        ReplyUrls               : {}


5. Teenuse põhilise rakenduse läbimise rakenduste ID Active Directory rakenduse loomiseks.

        New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId

6. Teenuse põhisumma õiguste tellimuse andmine. Selles näites saate lisada teenuse põhilise **lugeja** rolli, mis annab õiguse lugeda kõik ressursid tellimuse. Muud rollid, lugege teemat [RBAC: sisseehitatud rollid](./active-directory/role-based-access-built-in-roles.md). **Andke** parameetri ette **ApplicationId** , mida kasutasite rakenduse loomisel.

        New-AzureRmRoleAssignment -RoleDefinitionName Reader -ServicePrincipalName $app.ApplicationId.Guid

    Kui teie konto pole piisavaid õigusi määrata rolli, kuvatakse tõrketeade. Teade teie konto **on luba Microsoft.Authorization/roleAssignments/write' üle ulatus "/ tellimuste / {guid}" toimingu sooritamiseks**.

See on õige! Oma AD rakenduste ja teenuste põhisumma häälestada. Järgmises jaotises kirjeldatakse, kuidas serdi PowerShelli kaudu sisse logima.

### <a name="provide-certificate-through-automated-powershell-script"></a>Sisestage serdi automatiseeritud PowerShelli skripti kaudu

Iga kord, kui logite sisse teenuse põhi, peate kataloogi rentniku ID-d pakuvad AD Rakenduse. Rentniku on Active Directory eksemplar. Kui teil on ainult üks tellimus, saate kasutada:

    $tenant = (Get-AzureRmSubscription).TenantId
    
Kui teil on mitu tellimust, määrake see tellimus, kus asub teie Active Directory. Lisateavet leiate teemast [Administreerimine Azure AD kataloogi](./active-directory/active-directory-administer.md).

    $tenant = (Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId

Autentida oma skripti, määrake konto on teenus põhisumma ja serdi sõrmejälje rakenduse id ja rentniku ID-d. Skripti automatiseerimiseks saate talletada need väärtused nimega keskkonna muutujate ja täitmise ajal taastada, või saate lisada need oma skripti.

    Add-AzureRmAccount -ServicePrincipal -CertificateThumbprint $cert.Thumbprint -ApplicationId $app.ApplicationId -TenantId $tenant

Nüüd on kinnitatud, nagu põhisumma teenuse Active Directory rakendus, mida olete loonud.

## <a name="sample-applications"></a>Lihtsad rakendused

Kuidas sisse logida teenuse peamise valimi järgmiste rakenduste kuvamine

**.NET-I**

- [Juurutamine on SSH lubatud VM malli .net-i abil](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-template-deployment/)
- [Azure'i ressursse ja .net-i ressursikeskuse rühmade haldamine](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-resources-and-groups/)

**Java**

- [Java kasutamise alustamine – juurutada Azure ressursihaldur malli abil - ressursse](https://azure.microsoft.com/documentation/samples/resources-java-deploy-using-arm-template/)
- [Java kasutamise alustamine – haldamine ressursirühm - ressursid](https://azure.microsoft.com/documentation/samples/resources-java-manage-resource-group//)

**Python**

- [Juurutamine on SSH lubatud VM Python malli abil](https://azure.microsoft.com/documentation/samples/resource-manager-python-template-deployment/)
- [Azure'i ressursside ja ressursside rühmade Python haldamine](https://azure.microsoft.com/documentation/samples/resource-manager-python-resources-and-groups/)

**Node.js**

- [Juurutamine on SSH lubatud VM Node.js malli abil](https://azure.microsoft.com/documentation/samples/resource-manager-node-template-deployment/)
- [Azure'i ressursid ja ressursside Node.js rühmade haldamine](https://azure.microsoft.com/documentation/samples/resource-manager-node-resources-and-groups/)

**Ruby**

- [Juurutamine on SSH lubatud VM Ruby malli abil](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-template-deployment/)
- [Azure'i ressursside ja ressursside Ruby rühmade haldamine](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)

## <a name="next-steps"></a>Järgmised sammud
  
- Üksikasjalikud juhised rakenduse integreerimine Azure ressursside haldamise kohta leiate teemast [arendaja juhend autoriseerimine Azure'i ressursihaldur API-ga](resource-manager-api-authentication.md).
- Selgituse rakenduste ja teenuste põhisumma, leiate [rakenduse objektid ja teenuse põhilise objektid](./active-directory/active-directory-application-objects.md). 
- Active Directory autentimise kohta leiate lisateavet teemast [Azure AD autentimise stsenaariumid](./active-directory/active-directory-authentication-scenarios.md).



