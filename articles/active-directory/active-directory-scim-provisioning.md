<properties
    pageTitle="Luba automaatne ettevalmistamise kasutajatele ja rühmadele Azure Active Directory rakendustele SCIM abil | Microsoft Azure'i"
    description="Azure Active Directory saab automaatselt ettevalmistamine kasutajate ja rühmade rakenduse või identiteedi Store, mis on esiküljega, veebiteenuse kasutajaliides, mis on määratletud SCIM protokolli määratlus"
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/09/2016"
    ms.author="asmalser-msft"/>

#<a name="using-scim-to-enable-automatic-provisioning-of-users-and-groups-from-azure-active-directory-to-applications"></a>Luba automaatne ettevalmistamise kasutajatele ja rühmadele Azure Active Directory rakendustele SCIM abil

##<a name="overview"></a>Ülevaade

Azure Active Directory saab automaatselt ette kasutajad ja rühmad rakenduse või identiteedi Store, mis on esiküljega, veebiteenuse liidese [SCIM 2.0 protokolli määratlus](https://tools.ietf.org/html/draft-ietf-scim-api-19). Azure Active Directory, saate luua, muuta ja kustutada määratud kasutajad ja rühmad veebiteenusele, mille saate teisendada need taotlused toimingud pärast sihtrakenduse identiteedi poe päringuid. 

![][1]
*Joonis: Ettevalmistamise Azure Active Directory säilitada identiteedi veebiteenuse kaudu*

Seda funktsiooni saab kasutada koos "[tuua oma rakenduse](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx)" võimalusega Azure AD lubada ühekordse sisselogimise ja automaatse kasutaja ettevalmistamise rakendusi, mis pakuvad või on esiküljega SCIM veebiteenuse abil.

On kaks kasutamine karpide SCIM Azure Active Directory.

* **Kasutajad ja rühmad, mis toetavad SCIM rakendustele ettevalmistamise** - rakendusi, mis toetavad SCIM 2.0 ja kasutage autentimiseks OAuthi esitaja sõned töötab Azure AD välja.

* **Luua oma ettevalmistamise lahenduse, mis toetavad muude API-põhiste ettevalmistamise rakenduste** --SCIM rakendused, saate luua tõlkimiseks Azure AD SCIM lõpp-punkti ja mis tahes SCIM lõpp-punkti API rakendus toetab kasutaja ettevalmistamine.  Abi SCIM endpoint arengu, pakume CLI teekide koodinäiteid, mis näitab, kuidas SCIM endpoint ja tõlkida SCIM sõnumid koos.  

##<a name="provisioning-users-and-groups-to-applications-that-support-scim"></a>Kasutajate ja rühmade rakendustele, mis toetavad SCIM ettevalmistamine

Azure Active Directory saab konfigureerida automaatselt määratud kasutajate ja rühmade rakendustele eeldab, et [süsteemi domeenide identiteetide haldus 2 (SCIM)](https://tools.ietf.org/html/draft-ietf-scim-api-19) Web teenuse ja nõustuge OAuthi esitaja sõned autentimiseks. Spetsifikatsioonile SCIM 2.0 rakenduste peab vastama järgmistele nõuetele:

* Kasutajate ja/või rühmade punkti 3.3 SCIM protokolli loomine toetab.  

* Kasutajate ja/või rühmade paik taotlusi punkti 3.5.2 SCIM protokolli muutmine toetab.  

* Toetab toomine teadaolevad ressursi punkti 3.4.1 SCIM protokolli.  

*  Päringute kasutajate ja/või rühmade punkti 3.4.2 SCIM protokolli tugi.  Vaikimisi kasutajad on esitama päringu poolt externalId ja rühmad on esitama päringu poolt displayName.  

* Päringute kasutaja ID-d ja punkti 3.4.2 SCIM protokolli haldur toetab.  

* Päringute rühma ID-d ja liikme punkti 3.4.2 SCIM protokolli tugi.  

* Aktsepteerib OAuthi esitaja sõned loa punkti 2.1 SCIM protokolli.

Kontrollige oma rakenduse pakkuja või laused nendele nõuetele vastavust rakenduse pakkuja dokumentatsioonist.
 
###<a name="getting-started"></a>Alustamine

Azure Active Directory Azure AD rakenduse Galerii "kohandatud" rakenduse funktsiooni abil saab ühendada rakendusi, mis toetavad eespool kirjeldatud SCIM profiili. Kui ühendus on loodud, käivitatakse Azure AD sünkroonimise protsessi iga 5 minuti järel, kui selle rakenduse SCIM lõpp-punkti jaoks määratud kasutajad ja rühmad, päringuid ja loob või muudab neid vastavalt ülesande üksikasjad.

**Ühenduse rakendus, mis toetab SCIM:**

1.  Veebibrauseris, käivitage Azure'i haldusportaal https://manage.windowsazure.com juures.
2.  Liikuge sirvides **Active Directory > Directory > [teie Directory] > Applications**, ja valige **Lisa > rakenduse lisamine galeriist**.
3.  Valige vahekaardi **kohandatud** vasakul, sisestage oma rakenduse nimi ja klõpsake ikooni märge objekti rakenduse loomiseks.

![][2]

4.  Valige teine nuppu **Konfigureeri konto ettevalmistamise** tulemuseks ekraani.
5.  Sisestage väljale **Ettevalmistamise lõpp-punkti URL-i** lõpp-punkti rakenduse SCIM URL-i.
6.  Kui SCIM lõpp-punkti jaoks on vaja mõnda OAuthi esitaja luba kaudu on väljaandja peale Azure AD, kopeerige nõutav OAuthi esitaja luba **Turbeloa autentimine (valikuline)** välja. On see väli tühjaks, siis Azure AD sisaldab OAuthi esitaja luba Azure AD iga taotlusega välja. Rakendused, mis kasutavad Azure AD nagu idenity osutaja valideerib selle Azure'i AD-välja luba.
7.  Klõpsake nuppu **Järgmine**ja klõpsake nuppu **Testi alustage** on Azure Active Directory SCIM lõpp-punkti loomiseks. Kui katsed ei õnnestu, kuvatakse diagnostikateave.  
8.  Kui katsete ühenduse rakenduse õnnestub, seejärel klõpsake **Järgmine** ülejäänud ekraanid ja **lõpuleviimine** dialoogiboksi sulgemiseks klõpsake nuppu.
9.  Valige tulemuseks ekraani kolmanda nuppu **Määra kontod** . Tulemuseks jaotises kasutajad ja rühmad, määrata kasutajate või rühmade soovite rakendusse ettevalmistamine.
10. Kui kasutajad ja rühmad on määratud, klõpsake ekraani ülaosas vahekaarti **konfigureerimine** .
11. Jaotises **Konto ettevalmistamise**, veenduge, et olek on sees. 
12. **Tööriistad**, klõpsake **uuesti konto ettevalmistamise** ebausaldusväärsete käivitada.

Pange tähele, et 5 – 10 minuti võib ajavahemik enne ebausaldusväärsete hakkab SCIM lõpp-punkti päringuid saata.  Ühenduse katsete kokkuvõte on toodud rakenduse vahekaart armatuurlaud ja ettevalmistamise tegevuse aruande nii ettevalmistamise vigu saab alla laadida selle kataloogi vahekaart aruanded.

##<a name="building-your-own-provisioning-solution-for-any-application"></a>Hoone oma ettevalmistamise lahenduse, mis tahes rakenduse

Luues SCIM veebiteenus, mis ühendab Azure Active Directory, saate lubada ühekordse sisselogimise ja automaatse kasutaja ettevalmistamise praktiliselt kõigist rakendus, mis pakub ülejäänud või SOAP kasutaja ettevalmistamise API.

Kuidas see toimib järgmiselt

1.  Azure AD pakub levinud keele taristu teegi nimega [Microsoft.SystemForCrossDomainIdentityManagement](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/). Arendajad ja süsteemiintegraatorid abil saate selle teegi luua ja kasutusele võtta mõne SCIM veebipõhine teenuse lõpp-punkti võimeline Azure AD ühenduse mis tahes rakenduses identiteedi poe.
2.  Vastendused rakendatakse veebiteenuse standardne kasutaja skeemi vastendamiseks kasutaja skeemi ja Protocol (protokoll) nõuab.
3.  URL-i lõpp-punkti on registreeritud Azure AD kohandatud rakenduse galeriis rakenduse osana.
4.  Selle rakenduse Azure AD on määratud kasutajad ja rühmad. Pärast ülesande need pannakse järjekorda sihtrakenduse sünkroonimist. Töötlemise kuhjuda sünkroonimine käivitatakse iga 5 minuti järel.

###<a name="code-samples"></a>Koodinäiteid

Selle protsessi hõlbustamiseks on kogumi [proovi kood](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master) , mis on SCIM web teenuse lõpp-punkti loomine ja näidata automaatse ettevalmistamise. Ühe näidise on pakkuja, mis säilitab faili reaga komaga eraldatud väärtuste kasutajad ja rühmad.  Teine on, mis toimib Amazon Web Services identiteedi ja juurdepääsu juhtimine teenuse pakkuja.  

**Eeltingimused**

* Visual Studio 2013 või uuem versioon
* [Azure'i SDK .net-i jaoks](https://azure.microsoft.com/downloads/)
* Windowsi arvuti ASP.net-i framework 4.5 toetava kasutada SCIM lõpp-punkti. See arvuti peab olema puuetega inimestele juurdepääsetavate pilveteenuses
* [Azure'i tellimuse Azure AD Premium prooviversiooni või litsentsitud versiooni abil](https://azure.microsoft.com/services/active-directory/)
* Amazon AWS valimi nõuab [AWS tööriistakomplekt Visual Studio](http://docs.aws.amazon.com/AWSToolkitVS/latest/UserGuide/tkv_setup.html)teeke. Vaadake README faili kaasas valimi täiendavad üksikasjad

###<a name="getting-started"></a>Alustamine

Lihtsaim viis rakendada mõne SCIM lõpp-punkti, mida saate aktsepteerige ettevalmistamise Azure AD on luua ja juurutada mis väljundid ettevalmistatud kasutajatele komaeraldusega (CSV) faili proovi kood.

**Valimi SCIM lõpp-punkti loomiseks tehke järgmist.**

1.  Proovi kood [https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master) paketi allalaadimine
2.  Pakkige pakett ja paigutada selle oma Windowsi arvuti asukohast, nt C:\AzureAD-BYOA-Provisioning-Samples\.
3.  Selles kaustas, käivitage FileProvisioningAgent lahenduse Visual Studios.
4.  Valige **Tööriistad > Raamatukogu Package Manager > Package Manager konsooli**, ja käivitada käske all FileProvisioningAgent projekti lahenduse viiteid lahendamiseks:

    Installi pakett Microsoft.SystemForCrossDomainIdentityManagement Installi pakett Microsoft.IdentityModel.Clients.ActiveDirectory Installi pakett Microsoft.Owin.Diagnostics Installi pakett Microsoft.Owin.Host.SystemWeb

5.  Koostada FileProvisioningAgent projekt.
6.  Käivitage käsuviip rakendus Windows (administraatorina) ja **CD-le** käsu abil saate muuta kataloogi **\AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug** kausta.
7.  Käivitage käsk all, asendades < ip-aadressi > Windowsi arvuti IP- või domeeni nime.

    FileAgnt.exe http://<ip-address>:9000 TargetFile.csv

8.  Jaotises Windows **Windowsi sätted > Võrk ja Internet sätted**, valige soovitud **Windowsi tulemüüri > Täpsemad sätted**, ja luua mõni **Sissetulev reegel** , mis lubab sissetulevate juurdepääsu port 9000.
9.  Kui Windowsi seade on ruuteri taga, ruuteri tuleb konfigureerida nii, et teha võrgu juurdepääsu tõlge port 9000, mis Internetis on avatud ja pordi 9000 Windowsi arvutisse vahel. See on nõutav Azure AD saama pilveteenuses selle lõpp-punkti juurdepääs.


**Valimi SCIM lõpp-punkti Azure AD registreerimine**

1.  Veebibrauseris, käivitage Azure'i haldusportaal https://manage.windowsazure.com juures.
2.  Liikuge sirvides **Active Directory > Directory > [teie Directory] > Applications**, ja valige **Lisa > rakenduse lisamine galeriist**.
3.  Valige menüü **kohandatud** vasakul, sisestage nimi, näiteks "SCIM testi App" ja klõpsake ikooni märge objekti rakenduse loomiseks. Pange tähele, et objekt rakenduse loodud on kavatse tähistada sihtrakendusele oleks ettevalmistamine ja ühekordse sisselogimise jaoks ja mitte ainult SCIM lõpp-punkti.

![][2]

4.  Valige teine nuppu **Konfigureeri konto ettevalmistamise** tulemuseks ekraani.
5.  Sisestage dialoogiboksis Interneti-esitatud URL-i ja port oma SCIM lõpp-punkti. See on midagi nagu http://testmachine.contoso.com:9000 või http://<ip-address>:9000/, kus on < ip-aadressi > Interneti-ühendus esitatud IP address.  
6.  Klõpsake nuppu **Järgmine**ja klõpsake nuppu **Testi alustage** on Azure Active Directory SCIM lõpp-punkti loomiseks. Kui katsed ei õnnestu, kuvatakse diagnostikateave.  
7.  Kui katsete ühenduse loomiseks oma veebiteenuse õnnestub, seejärel klõpsake nuppu **Järgmine** ülejäänud ekraanidel ja **täielik** dialoogiboksi sulgemiseks klõpsake nuppu.
8.  Valige tulemuseks ekraani kolmanda nuppu **Määra kontod** . Tulemuseks jaotises kasutajad ja rühmad, määrata kasutajate või rühmade soovite rakendusse ettevalmistamine.
9.  Kui kasutajad ja rühmad on määratud, klõpsake ekraani ülaosas vahekaarti **konfigureerimine** .
10. Jaotises **Konto ettevalmistamise**, veenduge, et olek on sees. 
11. **Tööriistad**, klõpsake **uuesti konto ettevalmistamise** ebausaldusväärsete käivitada.

Pange tähele, et 5 – 10 minuti võib ajavahemik enne ebausaldusväärsete hakkab SCIM lõpp-punkti päringuid saata.  Ühenduse katsete kokkuvõte on toodud rakenduse vahekaart armatuurlaud ja ettevalmistamise tegevuse aruande nii ettevalmistamise vigu saab alla laadida selle kataloogi vahekaart aruanded.

Viimane toiming valimi kontrollimisel on TargetFile.csv faili avamiseks \AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug kausta teie Windowsi arvutisse. Kui ebausaldusväärsete käitamiseks see fail kuvatakse kõik üksikasjad määratud ja ette valmistatud kasutajad ja rühmad.

###<a name="development-libraries"></a>Arengu teegid

Arendada oma veebiteenus, mis vastab SCIM määratlus, end esmalt järgmist teekide Microsofti kiirendada arendamise protsessi: 

**1:**  Levinud keele taristu teekide pakutakse vastavalt selle taristu, nt C# keelte kasutamiseks.  Üks neisse teekidesse [Microsoft.SystemForCrossDomainIdentityManagement.Service](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/), kinnitab liides, Microsoft.SystemForCrossDomainIdentityManagement.IProvider, näidatud alloleval joonisel.  Teekide abil arendaja rakendada mis ühilduvad klassi, mis võib viidata, üldiselt pakkuja.  Teekide lubamine arendaja hõlpsalt juurutada veebiteenus, mis vastab SCIM määratlus, kas majutatud Internet Information Services või mis tahes käivitatava levinud keele taristu komplekti.  Taotlusi veebiteenusele tõlgitakse teenusepakkuja meetodid, mida soovite programmeeritud arendaja sõitmiseks mõned identiteedi poe kõned.    

![][3]

**2:** [Kiire marsruutimiseks käitleja](http://expressjs.com/guide/routing.html) on saadaval sõelumine node.js taotluse objektide kõned (määratletud SCIM specification), mis tähistab tehtud on node.js veebiteenus.     

###<a name="building-a-custom-scim-endpoint"></a>Kohandatud SCIM Endpoint ehitamine

Eespool kirjeldatud teekide abil arendajate neisse teekidesse abil saate majutada oma teenuseid, mis tahes käivitatava levinud keele taristu komplekti või Internet Information Services.  Siin on proovi kood hosting teenuse käivitatava rühmituse aadressil http://localhost:9000 sees. 

    private static void Main(string[] arguments)
    {
    // Microsoft.SystemForCrossDomainIdentityManagement.IMonitor, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IProvider and 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service are all defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service.dll.  
    
    Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor = 
      new DevelopersMonitor();
    Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider = 
      new DevelopersProvider(arguments[1]);
    Microsoft.SystemForCrossDomainIdentityManagement.Service webService = null;
    try
    {
        webService = new WebService(monitor, provider);
        webService.Start("http://localhost:9000");

        Console.ReadKey(true);
    }
    finally
    {
        if (webService != null)
        {
            webService.Dispose();
            webService = null;
        }
    }
    }

    public class WebService : Microsoft.SystemForCrossDomainIdentityManagement.Service
    {
    private Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor;
    private Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider;

    public WebService(
      Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitoringBehavior, 
      Microsoft.SystemForCrossDomainIdentityManagement.IProvider providerBehavior)
    {
        this.monitor = monitoringBehavior;
        this.provider = providerBehavior;
    }

    public override IMonitor MonitoringBehavior
    {
        get
        {
            return this.monitor;
        }

        set
        {
            this.monitor = value;
        }
    }

    public override IProvider ProviderBehavior
    {
        get
        {
            return this.provider;
        }

        set
        {
            this.provider = value;
        }
    }
    }

See on oluline märkida, et see teenus peab olema ka HTTP aadress ja serveri autentimissert mis juurkausta sertimiskeskus on ühte järgmistest: 

* CNNIC
* Comodo
* CyberTrust
* DigiCert
* GeoTrust
* GlobalSign
* Go Daddy
* VeriSign
* WoSign

Serveri autentimissert saate seotud pordi Windows Server abil võrgu shell kasuliku, näiteks nii: 

    netsh http add sslcert ipport=0.0.0.0:443 certhash=0000000000003ed9cd0c315bbb6dc1c08da5e6 appid={00112233-4455-6677-8899-AABBCCDDEEFF}  
 
Siin on esitatud serdi räsi argumendi väärtus sertifikaadi sõrmejälje ajal esitatud appid argumendi väärtus on suvaline globaalselt kordumatu identifikaatori.  

Teenusekomplekti Internet Information Services teenust majutada, luua arendaja levinud keele taristu kood teegi komplekti nimega käivitus Vaikenimeruum komplekti klassi.  Siin on näide sellise klassi: 

    public class Startup
    {
    // Microsoft.SystemForCrossDomainIdentityManagement.IWebApplicationStarter, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IMonitor and  
    // Microsoft.SystemForCrossDomainIdentityManagement.Service are all defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service.dll.  

    Microsoft.SystemForCrossDomainIdentityManagement.IWebApplicationStarter starter;

    public Startup()
    {
        Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor = 
          new DevelopersMonitor();
        Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider = 
          new DevelopersProvider();
        this.starter = 
          new Microsoft.SystemForCrossDomainIdentityManagement.WebApplicationStarter(
            provider, 
            monitor);
    }

    public void Configuration(
      Owin.IAppBuilder builder) // Defined in in Owin.dll.  
    {
        this.starter.ConfigureApplication(builder);
    }
    }

###<a name="handling-endpoint-authentication"></a>Käsitsemise lõpp-punkti autentimine

Azure Active Directory päringutele kaasata ka OAuth 2.0 esitaja luba.   Mis tahes teenuse taotluse vastu peaks autentida väljaandja Azure Active Directory on oodatud Azure Active Directory rentniku juurdepääsuks Azure Active Directory Graphi veebiteenuse nimel.  Luba, väljaandja on tähistatud iss taotluste, nt "iss": "https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/".  Selles näites baasaadressi väärtuse taotluste https://sts.windows.net, tuvastab Azure Active Directory väljaandja, lõigu suhteline aadress cbb1a5ac-f33b-45fa-9bf5-f37db0fed422, on Azure Active Directory rentniku eest, mis on antud luba kordumatut tunnust.  Kui on antud luba juurdepääsuks Azure Active Directory Graphi veebiteenuse, siis identifikaator teenuse 00000002-0000-0000-c000-000000000000, peaks olema selle luba aud nõude väärtus.  

Arendajate esitatud hoone SCIM teenus Microsoft Common Language infrastruktuur teekide abil saate autentida päringutele Azure Active Directory abil Microsoft.Owin.Security.ActiveDirectory paketi järgmiste juhiste järgi: 

**1:**  Pakkuja, rakendada Microsoft.SystemForCrossDomainIdentityManagement.IProvider.StartupBehavior atribuut on selle meetodi nimetatakse iga kord, kui teenus on käivitatud tagastada: 

    public override Action\<Owin.IAppBuilder, System.Web.Http.HttpConfiguration.HttpConfiguration\> StartupBehavior
    {
      get
      {
        return this.OnServiceStartup;
      }
    }

    private void OnServiceStartup(
      Owin.IAppBuilder applicationBuilder,  // Defined in Owin.dll.  
      System.Web.Http.HttpConfiguration configuration)  // Defined in System.Web.Http.dll.  
    {
    }

**2:**  See meetod on taotluse teenuse lõpp-punktid autenditud kandvate märgiks nimel määratud rentniku juurdepääsuks Azure Active Directory Graphi veebiteenuse Azure Active Directory välja nagu mis tahes järgmine kood lisamiseks tehke järgmist. 

    private void OnServiceStartup(
      Owin.IAppBuilder applicationBuilder IAppBuilder applicationBuilder, 
      System.Web.Http.HttpConfiguration HttpConfiguration configuration)
    {
      // IFilter is defined in System.Web.Http.dll.  
      System.Web.Http.Filters.IFilter authorizationFilter = 
        new System.Web.Http.AuthorizeAttribute(); // Defined in System.Web.Http.dll.configuration.Filters.Add(authorizationFilter);

      // SystemIdentityModel.Tokens.TokenValidationParameters is defined in    
      // System.IdentityModel.Token.Jwt.dll.
      SystemIdentityModel.Tokens.TokenValidationParameters tokenValidationParameters =     
        new TokenValidationParameters()
        {
          ValidAudience = "00000002-0000-0000-c000-000000000000"
        };

      // WindowsAzureActiveDirectoryBearerAuthenticationOptions is defined in 
      // Microsoft.Owin.Security.ActiveDirectory.dll
      Microsoft.Owin.Security.ActiveDirectory.
      WindowsAzureActiveDirectoryBearerAuthenticationOptions authenticationOptions =
        new WindowsAzureActiveDirectoryBearerAuthenticationOptions()    {
        TokenValidationParameters = tokenValidationParameters,
        Tenant = "03F9FCBC-EA7B-46C2-8466-F81917F3C15E" // Substitute the appropriate tenant’s 
                                                      // identifier for this one.  
      };

      applicationBuilder.UseWindowsAzureActiveDirectoryBearerAuthentication(authenticationOptions);
    }

##<a name="user-and-group-schema"></a>Kasutaja ja rühma skeem

Azure Active Directory saate kahte tüüpi ressursse SCIM veebiteenuste säte.  Need ressursid on kasutajad ja rühmad.  

Kasutaja ressursid on tähistatud skeemi identifikaator, kaasatakse see protokolli spetsifikatsioon urn: ietf:params:scim:schemas:extension:enterprise:2.0:User: http://tools.ietf.org/html/draft-ietf-scim-core-schema.  Allpool on esitatud tabelis 1, vaikimisi Azure Active Directory kasutajate atribuute urn: ietf:params:scim:schemas:extension:enterprise:2.0:User ressursside atribuutide vastendust.  

Jaotises ressursid on tähistatud skeemi identifikaator, http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group.  Tabel 2, all, kuvatakse vaikimisi rühmades atribuute Azure Active Directory http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group ressursside atribuutide vastendust.  

###<a name="table-1-default-user-attribute-mapping"></a>Tabel 1: Vaikimisi kasutaja atribuudi vastendamine

| Azure Active Directory kasutaja | urn: ietf:params:scim:schemas:extension:enterprise:2.0:User |
| ------------- | ------------- |
| IsSoftDeleted | aktiivne |
| displayName | displayName |
| Faksi-TelephoneNumber | phoneNumbers [tüüp eq "Faks"] .value |
| givenName | name.givenName |
| Ametinimetus | pealkiri |
| e-posti | e-kirju [tüüp eq "töö"] .value |
| mailNickname | externalId |
| haldur | haldur |
| mobiilse | phoneNumbers [tüüp eq "mobile"] .value |
| ObjectId väärtuse | ID |
| Sihtnumber | aadresside [tüüp eq "töö"] .postalCode |
| puhverserveri aadressid | e-kirju [Tippige eq "muu"]. Väärtus |
| füüsilise-kohaletoimetamise-OfficeName | aadresside [Tippige eq "muu"]. Vormindatud |
| streetAddress | aadresside [tüüp eq "töö"] .streetAddress |
| perekonnanimi | name.familyName |
| telefoni Number | phoneNumbers [tüüp eq "töö"] .value |
| kasutaja-PrincipalName | Kasutajanimi |


###<a name="table-2-default-group-attribute-mapping"></a>Tabel 2: Vaikimisi rühma atribuudi vastendamine

| Azure Active Directory rühma | http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group |
| ------------- | ------------- |
| displayName | externalId |
| e-posti | e-kirju [tüüp eq "töö"] .value |
| mailNickname | displayName |
| liikmed | liikmed |
| ObjectId väärtuse | ID |
| proxyAddresses | e-kirju [Tippige eq "muu"]. Väärtus |


##<a name="user-provisioning-and-de-provisioning"></a>Kasutajate ettevalmistamine ja tühistage ettevalmistamine

Kuvatakse allpool sõnumeid, et Azure Active Directory saadab SCIM teenuse haldamiseks identiteedi teise kasutaja elutsükli talletada.  Ka diagramm näitab, kuidas kasutada Microsofti teenuste jaoks antud levinud keele taristu teekide SCIM teenuse tõlkida taotluste pakkuja meetodid kõned.  

![][4]
*Joonis: Kasutaja ettevalmistamise ja tühistage ettevalmistamise järjestust*

**1:**  Azure Active Directory päringu teenuse kasutaja koos sobitamine mailNickname atribuudi väärtus kasutaja Azure Active Directory externalId atribuudi väärtust.  Päring on väljendatud hinnaks dollarites Hypertext Transfer Protocol taotluse, nagu see, kus jyoung on valimi mailNickname Azure Active Directory kasutaja: 

    GET https://.../scim/Users?filter=externalId eq jyoung HTTP/1.1
    Authorization: Bearer ...

Kui teenus Microsoft rakendamiseks SCIM teenuste ette nähtud levinud keele taristu teekide abil, siis taotluse tõlgitakse päringu meetod on teenusepakkuja kõne.  Siin on selle meetodi allkirja. 

    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Protocol.  

    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource[]> Query(
      Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters parameters, 
      string correlationIdentifier);

Siin Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters kasutajaliidese määratlus. 

    public interface IQueryParameters: 
      Microsoft.SystemForCrossDomainIdentityManagement.IRetrievalParameters
    {
        System.Collections.Generic.IReadOnlyCollection <Microsoft.SystemForCrossDomainIdentityManagement.IFilter> AlternateFilters 
        { get; }
    }

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IRetrievalParameters
    {
      system.Collections.Generic.IReadOnlyCollection<string> ExcludedAttributePaths 
      { get; }
      System.Collections.Generic.IReadOnlyCollection<string> RequestedAttributePaths 
      { get; }
      string SchemaIdentifier 
      { get; }
    }

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IFilter
    {
        Microsoft.SystemForCrossDomainIdentityManagement.IFilter AdditionalFilter 
          { get; set; }
        string AttributePath 
          { get; } 
        Microsoft.SystemForCrossDomainIdentityManagement.ComparisonOperator FilterOperator 
          { get; }
        string ComparisonValue 
          { get; }
    }
    
    public enum Microsoft.SystemForCrossDomainIdentityManagement.ComparisonOperator
    {
        Equals
    }

Päringu kasutaja antud väärtusega externalId atribuudi eeltoodud proovi puhul päringu meetodil on möödas argumentide väärtuste on järgmine: 

* parameetrite. AlternateFilters.Count: 1
* parameetrite. AlternateFilters.ElementAt(0). AttributePath: "externalId"
* parameetrite. AlternateFilters.ElementAt(0). ComparisonOperator: ComparisonOperator.Equals
* parameetrite. AlternateFilter.ElementAt(0). ComparisonValue: "jyoung"
* correlationIdentifier: System.Net.Http.HttpRequestMessage.GetOwinEnvironment["owin. RequestId"] 

**2:**  Kui vastus päringusse, et kasutaja externalId atribuudi väärtus sobitamine mailNickname atribuudi väärtus kasutaja Azure Active Directory teenus ei tagasta kõik kasutajad, siis Azure Active Directory taotleb kas teenuse ettevalmistamise vastab mõnele Azure Active Directory kasutaja.  Siin on näide taotlus: 

    POST https://.../scim/Users HTTP/1.1
    Authorization: Bearer ...
    Content-type: application/json
    {
      "schemas":
      [
        "urn:ietf:params:scim:schemas:core:2.0:User",
        "urn:ietf:params:scim:schemas:extension:enterprise:2.0User"],
      "externalId":"jyoung",
      "userName":"jyoung",
      "active":true,
      "addresses":null,
      "displayName":"Joy Young",
      "emails": [
        {
          "type":"work",
          "value":"jyoung@Contoso.com",
          "primary":true}],
      "meta": {
        "resourceType":"User"},
       "name":{
        "familyName":"Young",
        "givenName":"Joy"},
      "phoneNumbers":null,
      "preferredLanguage":null,
      "title":null,
      "department":null,
      "manager":null}

Microsofti teenuste SCIM rakendamiseks antud levinud keele taristu teekide tõlkida taotluse loomine meetod on teenusepakkuja kõne.  Loo meetodil on allkirja. 

    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  

    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource> Create(
      Microsoft.SystemForCrossDomainIdentityManagement.Resource resource, 
      string correlationIdentifier);

Ettevalmistamise kasutaja taotluse korral saab ressursi argumendi väärtus on Microsoft.SystemForCrossDomainIdentityManagement eksemplari. Core2EnterpriseUser klassi Microsoft.SystemForCrossDomainIdentityManagement.Schemas teegis.  Kui kasutaja ettevalmistamise taotluse õnnestub, siis meetodi rakendamist eeldatakse, et tagastada eksemplari on soovitud Microsoft.SystemForCrossDomainIdentityManagement. Klassi Core2EnterpriseUser atribuudi identifikaator ainuidentifikaator äsja ette valmistatud kasutaja määratud väärtusega.  

**3:**  Kasutaja identiteedi poe, mis on SCIM poolt esineb värskendamiseks jätkub Azure Active Directory paluda kasutaja praeguses olekus teenuse taotluse umbes selline: 

    GET ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...

Microsofti teenuste SCIM rakendamiseks antud levinud keele taristu teekide abil loodud teenusega tõlgitakse taotluse too meetod on teenusepakkuja kõne.  Siin on allkirja too meetod. 

    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource and 
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters 
    // are defined in Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  
    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource> 
       Retrieve(
         Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters 
           parameters, 
           string correlationIdentifier);
    
    public interface 
      Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters:   
        IRetrievalParameters
        {
          Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier 
            ResourceIdentifier 
              { get; }
    }
    public interface Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier
    {
        string Identifier 
          { get; set; }
        string Microsoft.SystemForCrossDomainIdentityManagement.SchemaIdentifier 
          { get; set; }
    }

Taotlus tuua kasutaja praeguses olekus eeltoodud näites puhul parameetrite argumendi väärtus sisestatud objekti atribuutide väärtused on järgmised: 

* Identifikaator: "54D382A4-2050-4C03-94D1-E769F1D15682"
* SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"

**4:**  Kui viide atribuut on värskendatud, siis Azure Active Directory päringu teenuse määratlemaks, kas praegune väärtus viide atribuut identiteedi poes esiküljega teenus juba vastab Azure Active Directory selle atribuudi väärtust.  Kasutajad, ainult atribuut, mis on sellisel viisil päringuid praegune väärtus on halduri atribuuti.  Siin on näide taotluse kindlaks teha, kas teatud kasutaja objekti halduri atribuut on praegu teatud väärtus: 

    GET ~/scim/Users?filter=id eq 54D382A4-2050-4C03-94D1-E769F1D15682 and manager eq 2819c223-7f76-453a-919d-413861904646&attributes=id HTTP/1.1
    Authorization: Bearer ...

Väärtuse atribuute Päringuparameetri, id, näitab, et kui kasutaja objekt on olemas, mis vastab avaldis, mis on esitatud Päringuparameetri filter väärtus ja seejärel teenuse eeldatakse, et vastata urn: ietf:params:scim:schemas:core:2.0:User või urn: ietf:params:scim:schemas:extension:enterprise:2.0:User ressurssi, sh ainult selle ressursi id atribuudi väärtust.  Muidugi id atribuudi väärtus on teada imperatiivsus päringu – see on kaasatud filter päringu parameeter; See ei küsiks eesmärk on tegelikult nõuda minimaalsete kujutis ressursi vasta filtriavaldis tähistamise kas on olemas sellist objekti.   

Kui teenus on Microsofti SCIM teenuste rakendamiseks antud levinud keele taristu teekide abil loodud, siis taotluse tõlgitakse päringu meetod on teenusepakkuja kõne.  Parameetrite argumendi väärtus sisestatud objekti atribuutide väärtus on järgmine: 

* parameetrite. AlternateFilters.Count: 2
* parameetrite. AlternateFilters.ElementAt(x). AttributePath: "id"
* parameetrite. AlternateFilters.ElementAt(x). ComparisonOperator: ComparisonOperator.Equals
* parameetrite. AlternateFilter.ElementAt(x). ComparisonValue: "54D382A4-2050-4C03-94D1-E769F1D15682"
* parameetrite. AlternateFilters.ElementAt(y). AttributePath: "manager"
* parameetrite. AlternateFilters.ElementAt(y). ComparisonOperator: ComparisonOperator.Equals
* parameetrite. AlternateFilter.ElementAt(y). ComparisonValue: "2819c223-7f76-453a-919d-413861904646"
* parameetrite. RequestedAttributePaths.ElementAt(0): "id"
* parameetrite. SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"

Siin on väärtus indeksi x võib olla 0 ja index y väärtus võib olla 1, või väärtuse x võib olla 1 ning argumendi y väärtus võib olla 0, olenevalt filtri Päringuparameetri avaldiste järjestuse.   

**5:**  Siin on näide taotluse Azure Active Directory SCIM teenusesse värskendamiseks kasutaja: 

    PATCH ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
    Content-type: application/json
    {
      "schemas": 
      [
        "urn:ietf:params:scim:api:messages:2.0:PatchOp"],
      "Operations":
      [
        {
          "op":"Add",
          "path":"manager",
          "value":
            [
              {
                "$ref":"http://.../scim/Users/2819c223-7f76-453a-919d-413861904646",
                "value":"2819c223-7f76-453a-919d-413861904646"}]}]}

Microsoft Common Language infrastruktuur teekide SCIM teenuste rakendamiseks tõlkida taotluse värskendus meetod on teenusepakkuja kõne.  Siin on selle meetodi allkirja. 

    // System.Threading.Tasks.Tasks and 
    // System.Collections.Generic.IReadOnlyCollection<T>
    // are defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IPatch, 
    // Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier, 
    // Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation, 
    // Microsoft.SystemForCrossDomainIdentityManagement.OperationName, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IPath and 
    // Microsoft.SystemForCrossDomainIdentityManagement.OperationValue 
    // are all defined in Microsoft.SystemForCrossDomainIdentityManagement.Protocol. 

    System.Threading.Tasks.Task Update(
      Microsoft.SystemForCrossDomainIdentityManagement.IPatch patch, 
      string correlationIdentifier);

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IPatch
    {
    Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase 
      PatchRequest 
        { get; set; }
    Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier 
      ResourceIdentifier 
        { get; set; }        
    }

    public class PatchRequest2: 
      Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase
    {
    public System.Collections.Generic.IReadOnlyCollection
      <Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation> 
        Operations
        { get;}

    public void AddOperation(
      Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation operation);
    }

    public class PatchOperation
    {
    public Microsoft.SystemForCrossDomainIdentityManagement.OperationName 
      Name
      { get; set; }
    
    public Microsoft.SystemForCrossDomainIdentityManagement.IPath 
      Path
      { get; set; }

    public System.Collections.Generic.IReadOnlyCollection
      <Microsoft.SystemForCrossDomainIdentityManagement.OperationValue> Value
      { get; }

    public void AddValue(
      Microsoft.SystemForCrossDomainIdentityManagement.OperationValue value);
    }

    public enum OperationName
    {
      Add,
      Remove,
      Replace
    }

    public interface IPath
    {
      string AttributePath { get; }
      System.Collections.Generic.IReadOnlyCollection<IFilter> SubAttributes { get; }
      Microsoft.SystemForCrossDomainIdentityManagement.IPath ValuePath { get; }
    }

    public class OperationValue
    {
      public string Reference
      { get; set; }
      
      public string Value
      { get; set; }
    }



Taotluse ajakohastada kasutaja eeltoodud näites puhul sisestatud objekti paik argumendi väärtus on need atribuudi väärtused: 

* ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"
* ResourceIdentifier.SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"
* (Kui PatchRequest2 PatchRequest). Operations.Count: 1
* (Kui PatchRequest2 PatchRequest). Operations.ElementAt(0). OperationName: OperationName.Add
* (Kui PatchRequest2 PatchRequest). Operations.ElementAt(0). Path.AttributePath: "manager"
* (Kui PatchRequest2 PatchRequest). Operations.ElementAt(0). Value.Count: 1
* (Kui PatchRequest2 PatchRequest). Operations.ElementAt(0). Value.ElementAt(0). Viide: http://.../scim/Users/2819c223-7f76-453a-919d-413861904646
* (Kui PatchRequest2 PatchRequest). Operations.ElementAt(0). Value.ElementAt(0). Väärtus: 2819c223-7f76-453a-919d-413861904646

**6:**  Asutusest kasutaja poolt teenuse SCIM identiteedi poest, saadab Azure Active Directory päringu umbes selline: 

    DELETE ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
    
Kui teenus on Microsofti SCIM teenuste rakendamiseks antud levinud keele taristu teekide abil loodud, siis taotluse tõlgitakse Kustuta meetod on teenusepakkuja kõne.   See meetod on allkirja. 

    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier, 
    // is defined in Microsoft.SystemForCrossDomainIdentityManagement.Protocol. 
    System.Threading.Tasks.Task Delete(
      Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier  
        resourceIdentifier, 
      string correlationIdentifier);
 
Objekti, mis on esitatud resourceIdentifier argumendi väärtus on need atribuudi väärtused puhul taotluse asutusest kasutaja eeltoodud näites: 

* ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"
* ResourceIdentifier.SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"

##<a name="group-provisioning-and-de-provisioning"></a>Rühma ettevalmistamise ja tühistage ettevalmistamine

Kuvatakse allpool sõnumeid, et Azure Active Directory saadab SCIM teenuse haldamiseks rühmast teise identiteedi elutsükli talletada.  Need sõnumid erinevad sõnumid, mis on seotud kasutajad on kolm võimalust: 

* Rühma ressursi skeemiga on tuvastatud http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group.  
* Taotlusi tuua rühmad on ette nähtud, et liikmed atribuut on mis tahes ressursi taotluse vastuseks esitatud välja jätta.  
* Taotluste kindlaks teha, kas teatud väärtus on viide atribuudile saab taotlusi liikmete atribuudi kohta.  

![][5]
*Joonis: Jaotis ettevalmistamise ja tühistage ettevalmistamise järjestust*

##<a name="related-articles"></a>Seotud artiklid

- [Artikli registri Rakendusehaldus Azure Active Directory 's](active-directory-apps-index.md)
- [Kasutaja ettevalmistamise/Deprovisioning SaaS rakenduste automatiseerimine](active-directory-saas-app-provisioning.md)
- [Kasutajate ettevalmistamine atribuudi vastendused kohandamine](active-directory-saas-customizing-attribute-mappings.md)
- [Avaldiste atribuudi vastendused kirjutamine](active-directory-saas-writing-expressions-for-attribute-mappings.md)
- [Ulatuse määramine filtrid kasutajate ettevalmistamine](active-directory-saas-scoping-filters.md)
- [Konto ettevalmistamise teatised](active-directory-saas-account-provisioning-notifications.md)
- [Õpetused kuidas integreerida SaaS rakenduste loend](active-directory-saas-tutorial-list.md)


    
<!--Image references-->
[1]: ./media/active-directory-scim-provisioning/scim-figure-1.PNG
[2]: ./media/active-directory-scim-provisioning/scim-figure-2.PNG
[3]: ./media/active-directory-scim-provisioning/scim-figure-3.PNG
[4]: ./media/active-directory-scim-provisioning/scim-figure-4.PNG
[5]: ./media/active-directory-scim-provisioning/scim-figure-5.PNG
