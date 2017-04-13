<properties 
    pageTitle="Luua veebirakenduse Azure'i rakenduse teenuses Azure SDK Java abil" 
    description="Saate teada, kuidas luua veebirakenduse teenuses Azure rakenduse Azure'i SDK programmiliselt Java abil." 
    tags="azure-classic-portal"
    services="app-service\web" 
    documentationCenter="Java" 
    authors="donntrenton" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="02/25/2016" 
    ms.author="v-donntr"/>


# <a name="create-a-web-app-in-azure-app-service-using-the-azure-sdk-for-java"></a>Luua veebirakenduse Azure'i rakenduse teenuses Azure SDK Java abil

<!-- Azure Active Directory workflow is not yet available on the Azure Portal -->

## <a name="overview"></a>Ülevaade

Juhendis näidatakse, kuidas luua ka Azure SDK Java rakendus, mis loob Web Appi [Azure'i rakendust Service][], siis selle rakenduse juurutamine. See koosneb kahest osast:

- 1 osa näitab, kuidas koostada Java rakendus, mis loob web appi.
- 2 osa näitab, kuidas luua lihtne JSP "Tere, maailm" rakendus ja seejärel kasutage FTP kliendi juurutama rakenduse teenuse kood.


## <a name="prerequisites"></a>Eeltingimused

### <a name="software-installations"></a>Tarkvara installid

Selles artiklis AzureWebDemo rakenduse koodi kirjutatud Azure'i Java SDK 0.7.0, mille saate installida, kasutades [Web platvormi Installer][] (WebPI) abil. Lisaks veenduge, et kasutada [Azure tööriistakomplekt Eclipse][]uusima versiooni. Pärast installimist SDK, käitades **Värskenda register** **Maven**hoidlate sõltuvused Eclipse projektis värskendada ja seejärel uuesti lisada iga paketi aknas **sõltuvused** uusim versioon. Saate kontrollida Office'i versiooni installitud tarkvara Eclipse, klõpsates **Spikker > installimisjuhised Alustusjuhendi**; peaks teil olema vähemalt järgmisi versioone:

- Microsoft Azure'i teekide Java 0.7.0.20150309 pakett
- Eclipse IDE 4.4.2.20150219 Java EE arendajatele


### <a name="create-and-configure-cloud-resources-in-azure"></a>Loomine ja konfigureerimine Azure pilveteenuse ressursid

Enne selle toiminguga alustamist peate on aktiivne Azure'i tellimus ja seadistada vaikimisi Active Directory (AD) Azure.


### <a name="create-an-active-directory-ad-in-azure"></a>Azure Active Directory (AD) loomine

Kui te pole juba Active Directory (AD) Azure tellimuse, logige [Azure klassikaline portaalis][] oma Microsofti kontoga sisse. Kui teil on mitu tellimust, klõpsake nuppu **tellimused** ja valige tellimus, mida soovite kasutada selle projekti vaikekataloogi. Seejärel klõpsake nuppu **Rakenda** selle tellimuse aktiveerimiseks.

1. **Active Directory** vasakpoolses menüüs valik. **Nuppu Uus > Directory > Loo kohandatud**.

2. **Lisada kataloogi**, valige **Loo uus kaust**.

3. Sisestage väljale **nimi**kausta nimi.

4. **Domeeni**, sisestage domeeni nimi. See on lihtne domeeninime, mis sisaldab vaikimisi kataloogi; See on vormi `<domain_name>.onmicrosoft.com`. Saate selle põhjal kausta nimi või muu domeeninimi, mida te oma nime. Hiljem saate lisada mõne muu domeeni nimi, mis on juba teie asutus kasutab.

5. **Riik või regioon**, valige oma lokaat.

AD kohta leiate lisateavet teemast [mis on Azure AD kataloog][]?


### <a name="create-a-management-certificate-for-azure"></a>Azure'i halduse serdi loomine

Azure'i SDK Java kasutab autentimiseks Azure tellimuste halduse serdid. Need on X.509 v3 sertide kasutamise autentida kliendi rakendus, mis kasutab teenuse juhtimise API nimel haldamine tellimuse ressursid tellimuse omanik.

Selle toimingu koodi kasutab iseallkirjastatud serdi Azure autentida. Selle toimingu jaoks peate serdi loomine ja laadige see eelnevalt [Azure klassikaline portaalis][] . See hõlmab järgmist:

- Luua oma kliendi sert tähistav PFX-faili ja kohalikult salvestada.
- Luua management sert (CER-fail) PFX-fail.
- CER-faili üleslaadimine Azure tellimuse.
- PFX-fail teisendada JKS, kuna Java kasutab seda vormingut autentida, kasutades serdid.
- Kirjutage rakenduse autentimise koodi, mis viitab JKS kohalik fail.

Kui olete selle protseduuri lõpetanud, CER serti ei asu Azure tellimuse ja JKS serdi hakkavad asuma kohalikul kõvakettal. Halduse serdid kohta leiate lisateavet teemast [luua ja üles laadida Azure serdi haldus][].


#### <a name="create-a-certificate"></a>Serdi loomine

Oma iseallkirjastatud serdi loomiseks avage käsk konsooli operatsioonisüsteem ja käivitage järgmised käsud.

> **Märkus:**  Arvuti selle käsu käivitamist peab olema JDK, mis on installitud. Tee on keytool sõltub ka, asukoht, kus saate installida selle JDK. Lisateabe saamiseks vt Java Online'i dokumendid [võti ja serdi Haldustööriista (keytool)][] .

Pfx-faili loomiseks tehke järgmist.

    <java-install-dir>/bin/keytool -genkey -alias <keystore-id>
     -keystore <cert-store-dir>/<cert-file-name>.pfx -storepass <password>
     -validity 3650 -keyalg RSA -keysize 2048 -storetype pkcs12
     -dname "CN=Self Signed Certificate 20141118170652"

CER-faili loomiseks tehke järgmist.

    <java-install-dir>/bin/keytool -export -alias <keystore-id>
     -storetype pkcs12 -keystore <cert-store-dir>/<cert-file-name>.pfx
     -storepass <password> -rfc -file <cert-store-dir>/<cert-file-name>.cer

kui:

- `<java-install-dir>`on Java installitud kausta tee.
- `<keystore-id>`võtmehoidjas kirje identifikaator (nt `AzureRemoteAccess`).
- `<cert-store-dir>`tee kataloogi, kus soovite talletada serdid on (nt `C:/Certificates`).
- `<cert-file-name>`serdi faili nimi (nt `AzureWebDemoCert`).
- `<password>`on parooliga, valite kaitsta serdi; peab olema vähemalt 6 tähemärki. Kuigi see pole soovitatav, saate sisestada pole parool.
- `<dname>`on X.500 Eraldusnimi seotud alias (pseudonüüm) ja seda kasutatakse iseallkirjastatud serdi väljaandja ja teema väljad.

Lisateavet leiate teemast [luua ja üles laadida Azure serdi haldus][].


#### <a name="upload-the-certificate"></a>Laadige üles sert

Iseallkirjastatud serdi üleslaadimine Azure, minge klassikaline portaalis lehel **sätted** ja seejärel klõpsake vahekaarti **Halduse serdid** . Klõpsake lehe allservas **üles laadida** ja liikuge loodud CER-faili asukoht.


#### <a name="convert-the-pfx-file-into-jks"></a>PFX-faili teisendada JKS

Rakenduses Windowsi Käsuviip (töötab administraatorina), CD-le kataloogi sisaldava serdid ja käivitage järgmine käsk, kus `<java-install-dir>` on kaust, kus te Java teie arvutisse installitud:

    <java-install-dir>/bin/keytool.exe -importkeystore
     -srckeystore <cert-store-dir>/<cert-file-name>.pfx
     -destkeystore <cert-store-dir>/<cert-file-name>.jks
     -srcstoretype pkcs12 -deststoretype JKS

1. Küsimise korral sisestage sihtkoha võtmehoidjas parooli; See on parool JKS faili.

2. Küsimise korral sisestage allika võtmehoidjas parooli; See on PFX-faili määratud parool.

Kaks parooli ei pea olema sama. Kuigi see pole soovitatav, saate sisestada pole parool.


## <a name="build-a-web-app-creation-application"></a>Veebirakenduse loomine rakenduse koostamine

### <a name="create-the-eclipse-workspace-and-maven-project"></a>Eclipse tööruumi ja Maven projekti loomine

Selles jaotises saate luua tööruumi ja Maven project web app loomine rakenduse nimega AzureWebDemo.

1. Looge uus Maven projekt. Klõpsake **Fail > uus > Maven projekti**. Valige **Uus Maven projekt**, **projekti loomine** ja **kasutamine vaikeasukoha tööruumi**.

2. **Uue Maven projekti**teisel lehel Määrake järgmine:

    - Rühma ID:`com.<username>.azure.webdemo`
    - Artefakt ID: AzureWebDemo
    - Versioon: 0.0.1-SNAPSHOT
    - Pakendamine: jar
    - Nimi: AzureWebDemo

    Klõpsake nuppu **valmis**.

3. Uue projekti pom.xml faili avada rakenduses Project Explorer. Valige vahekaart **sõltuvused** . See on uue projekti, pole paketid on loetletud veel.

4. Avage Maven hoidlate vaade. **Aknas nuppu > Kuva vaade > muu > Maven > Maven hoidlate** ja klõpsake nuppu **OK**. **Maven hoidlate** vaade kuvatakse IDE allosas.

5. Avage **Globaalne hoidlate**, paremklõpsake **keskses** hoidlas, ja valige **Taastada indeks**.

    ![][1]
    
    Selles etapis tuleb võib kuluda mitu minutit sõltuvalt oma ühenduse kiirust. Indeks luuakse, peaksite nägema Microsoft Azure'i pakettide Maven **keskses** hoidlas.

6. **Sõltuvused**, klõpsake nuppu **Lisa**. Sisestage väljale **Enter rühma ID..** `azure-management`. Valige pakettide base halduse ja rakenduse teenuse veebirakenduste haldus.

        com.microsoft.azure  azure-management
        com.microsoft.azure  azure-management-websites

    > **Märkus:** Kui värskendate sõltuvused pärast uus versioon versioon, peate lisama iga sõltuvused selles loendis uuesti.
    > Klõpsake nuppu **Lisa** ja valige iga sõltuvus, kuvatakse see uue versiooni numbriga **sõltuvused** loendis.

Klõpsake nuppu **OK**. Azure'i pakettide kuvatakse loendis **sõltuvused** .


### <a name="writing-java-code-to-create-a-web-app-by-calling-the-azure-sdk"></a>Java koodi luua veebirakenduse Azure SDK helistades

Järgmiseks kirjutage koodis kutsub API-de Azure'i SDK Java rakendust Service veebirakenduse loomine.

1. Looge Java klassi sisaldama põhikirje punkti kood. Rakenduses Project Explorer, paremklõpsake projekti sõlm ja valige **uus > klassi**.

2. **Uue Java klassi**nime ainekursuse `WebCreator` ja märkige ruut **avaliku staatilise kehtetu põhi** . Valikud kuvatakse järgmiselt:

    ![][2]

3. Klõpsake nuppu **valmis**. WebCreator.java fail kuvatakse Project Explorer.


### <a name="calling-the-azure-api-to-create-an-app-service-web-app"></a>Helistamiseks Azure API luua rakenduse teenuse Web App


#### <a name="add-necessary-imports"></a>Lisage vajalik import

WebCreator.java, lisage järgmised impordi; impordi sisestage WiFi-halduse teekides tarbimine Azure'i API-de jaoks.

    // General imports
    import java.net.URI;
    import java.util.ArrayList;
    
    // Imports for Exceptions
    import java.io.IOException;
    import java.net.URISyntaxException;
    import javax.xml.parsers.ParserConfigurationException;
    import com.microsoft.windowsazure.exception.ServiceException;
    import org.xml.sax.SAXException;
    
    // Imports for Azure App Service management configuration
    import com.microsoft.windowsazure.Configuration;
    import com.microsoft.windowsazure.management.configuration.ManagementConfiguration;
    
    // Service management imports for App Service Web Apps creation
    import com.microsoft.windowsazure.management.websites.*;
    import com.microsoft.windowsazure.management.websites.models.*;
    
    // Imports for authentication
    import com.microsoft.windowsazure.core.utils.KeyStoreType;


#### <a name="define-the-main-entry-point-class"></a>Põhikirje punkti klassi määratlemine

Kuna AzureWebDemo rakenduse eesmärk on luua rakenduse teenuse veebirakenduse, nime peamiseks klassi selle rakenduse `WebAppCreator`. See tund pakub põhikirje punkti kood, mis nõuab Azure'i teenuse juhtimise API veebirakenduse loomine.

Parameetri järgmisi web appi ja veebiruumi lisada. Peate oma Azure tellimuse ID ja serdi teavet.

    public class WebAppCreator {
    
        // Parameter definitions used for authentication.
        private static String uri = "https://management.core.windows.net/";
        private static String subscriptionId = "<subscription-id>";
        private static String keyStoreLocation = "<certificate-store-path>";
        private static String keyStorePassword = "<certificate-password>";
    
        // Define web app parameter values.
        private static String webAppName = "WebDemoWebApp";
        private static String domainName = ".azurewebsites.net";
        private static String webSpaceName = WebSpaceNames.WESTUSWEBSPACE;
        private static String appServicePlanName = "WebDemoAppServicePlan";

kui:

- `<subscription-id>`on Azure tellimuse ID, kus soovite luua ressurss.
- `<certificate-store-path>`on tee ja nimi JKS faili kohaliku serdi poe kataloogis. Näiteks `C:/Certificates/CertificateName.jks` Linuxi ja `C:\Certificates\CertificateName.jks` Windowsi jaoks.
- `<certificate-password>`on teie JKS serdi loomisel määratud parool.
- `webAppName`võib olla mis tahes teie nimi; See toiming kasutab nime `WebDemoWebApp`. Täielik domeeninimi on selle `webAppName` abil soovitud `domainName` lisatud, et sel juhul täielik domeen on `webdemowebapp.azurewebsites.net`.
- `domainName`peaks olema määratud nagu eespool näidatud.
- `webSpaceName`peaks olema üks [WebSpaceNames][] klassis määratletud väärtused.
- `appServicePlanName`peaks olema määratud nagu eespool näidatud.

> **Märkus:** Iga kord, kui seda rakendust käivitama, tuleb teil muuta väärtus `webAppName` ja `appServicePlanName` (või Azure'i portaalis veebirakenduse kustutamine) enne rakendus uuesti. Vastasel korral täitmise nurjub, kuna sama ressurss juba olemas Azure.


#### <a name="define-the-web-creation-method"></a>Määratleda web loomine

Järgmisena Määratlege meetodi veebirakenduse loomine. See meetod `createWebApp`, saate määrata parameetrid web appi ja selle veebiruumi. See loob ja konfigureerib rakenduse teenuse veebirakenduste haldamine klient, mis on määratletud [WebSiteManagementClient][] objekti. Kliendi haldus on veebirakenduste loomine. Pakub rahulik veebiteenuste rakendused, helistades Teenusehaldus API (toimingute, nagu loomine, värskendamine ja kustutamine) veebirakenduste haldamine.

    private static void createWebApp() throws Exception {

        // Specify configuration settings for the App Service management client.
        Configuration config = ManagementConfiguration.configure(
            new URI(uri),
            subscriptionId,
            keyStoreLocation,  // Path to the JKS file
            keyStorePassword,  // Password for the JKS file
            KeyStoreType.jks   // Flag that you are using a JKS keystore
        );

        // Create the App Service Web Apps management client to call Azure APIs
        // and pass it the App Service management configuration object.
        WebSiteManagementClient webAppManagementClient = WebSiteManagementService.create(config);

        // Create an App Service plan for the web app with the specified parameters.
        WebHostingPlanCreateParameters appServicePlanParams = new WebHostingPlanCreateParameters();
        appServicePlanParams.setName(appServicePlanName);
        appServicePlanParams.setSKU(SkuOptions.Free);
        webAppManagementClient.getWebHostingPlansOperations().create(webSpaceName, appServicePlanParams);

        // Set webspace parameters.
        WebSiteCreateParameters.WebSpaceDetails webSpaceDetails = new WebSiteCreateParameters.WebSpaceDetails();
        webSpaceDetails.setGeoRegion(GeoRegionNames.WESTUS);
        webSpaceDetails.setPlan(WebSpacePlanNames.VIRTUALDEDICATEDPLAN);
        webSpaceDetails.setName(webSpaceName);

        // Set web app parameters.
        // Note that the server farm name takes the Azure App Service plan name.
        WebSiteCreateParameters webAppCreateParameters = new WebSiteCreateParameters();
        webAppCreateParameters.setName(webAppName);
        webAppCreateParameters.setServerFarm(appServicePlanName);
        webAppCreateParameters.setWebSpace(webSpaceDetails);

        // Set usage metrics attributes.
        WebSiteGetUsageMetricsResponse.UsageMetric usageMetric = new WebSiteGetUsageMetricsResponse.UsageMetric();
        usageMetric.setSiteMode(WebSiteMode.Basic);
        usageMetric.setComputeMode(WebSiteComputeMode.Shared);

        // Define the web app object.
        ArrayList<String> fullWebAppName = new ArrayList<String>();
        fullWebAppName.add(webAppName + domainName);
        WebSite webApp = new WebSite();
        webApp.setHostNames(fullWebAppName);

        // Create the web app.
        WebSiteCreateResponse webAppCreateResponse = webAppManagementClient.getWebSitesOperations().create(webSpaceName, webAppCreateParameters);

        // Output the HTTP status code of the response; 200 indicates the request succeeded; 4xx indicates failure.
        System.out.println("----------");
        System.out.println("Web app created - HTTP response " + webAppCreateResponse.getStatusCode() + "\n");

        // Output the name of the web app that this application created.
        String shinyNewWebAppName = webAppCreateResponse.getWebSite().getName();
        System.out.println("----------\n");
        System.out.println("Name of web app created: " + shinyNewWebAppName + "\n");
        System.out.println("----------\n");
    }

Koodi väljund vastust, mis näitab teavitab õnnestumisest või nurjumisest HTTP olekut ja edukaks väljund loodud veebirakenduse nime.


#### <a name="define-the-main-method"></a>Määratleda main() meetod

Sisestage main() meetod kood, mis nõuab createWebApp() veebirakenduse loomine.

Lõpuks helistada `createWebApp` : `main`:

        public static void main(String[] args)
            throws IOException, URISyntaxException, ServiceException,
            ParserConfigurationException, SAXException, Exception {

            // Create web app
            createWebApp();

        }  // end of main()

    }  // end of WebAppCreator class


#### <a name="run-the-application-and-verify-web-app-creation"></a>Käivitage rakendus ja kontrollige web appi loomine

Veenduge, et teie rakendus töötab, klõpsake **käivitada > Käivita**. Kui rakendus töötab lõpule jõudnud, peaksite nägema järgmine väljund Eclipse konsooli:

    ----------
    Web app created - HTTP response 200
    
    ----------
    
    Name of web app created: WebDemoWebApp
    
    ----------

Azure'i klassikaline portaali sisse logida ja klõpsake nuppu **Web Apps**. Uue veebirakenduse peaks kuvatama veebirakenduste loendist mõne minuti jooksul.


## <a name="deploying-an-application-to-the-web-app"></a>Rakenduse Web Appi juurutamine

Kui olete käivitanud AzureWebDemo ja loonud uue veebirakenduse, klassikaline portaali sisse logida, klõpsake **Web Apps**ja valige **WebDemoWebApp** loendis **Web Apps** . Web appi armatuurlaua lehel nuppu **Sirvi** (või klõpsake URL-i, `webdemowebapp.azurewebsites.net`) liikumiseks selle. Kuvatakse kohatäite tühja lehe, kuna sisu on avaldatud web appi veel.

Järgmiseks on "Tere, maailm" rakenduse loomine ja selle juurutama web appi.


### <a name="create-a-jsp-hello-world-application"></a>JSP Tere, maailm rakenduse loomine

#### <a name="create-the-application"></a>Rakenduse loomine

Juurutamise taotluse veebis näitamaks järgmist näidatakse, kuidas luua lihtne "Tere, maailm" Java rakendus ja laadige see rakendus teenuse Web App rakenduse loodud.

1. Klõpsake **Fail > uus > dünaamilise projekti**. Nime `JSPHello`. Teil pole vaja muuta muid sätteid selles dialoogiboksis. Klõpsake nuppu **valmis**.

    ![][3]

2. Project Exploreri, laiendage **JSPHello** projekt, paremklõpsake **WebContent**ja seejärel käsku **uus > JSP faili**. Dialoogiboksis uue JSP faili nimi uue faili `index.jsp`. Klõpsake nuppu **edasi**.

3. **Valige JSP malli** dialoogiboksis Vali **Uus JSP fail (html)** ja klõpsake nuppu **valmis**.

4. Index.jsp, lisage järgmine kood on `<head>` ja `<body>` sildistamine jaotised:

        <head>
          ...
          java.util.Date date = new java.util.Date();
        </head>
    
        <body>
          Hello, the time is <%= date %> 
        </body>


#### <a name="run-the-hello-world-application-in-localhost"></a>Tere, maailm rakenduse käivitada localhost

Enne selle rakenduse käitamiseks peate mõne atribuutide konfigureerimine.

1. Paremklõpsake **JSPHello** projekti ja valige **Atribuudid**.

2. Dialoogiboksis **Atribuudid** : valige **Java koostada tee**, valige menüü **järjestust ja ekspordi** , märkige ruut **JRE süsteemi teek**ja klõpsake nuppu **üles** liikuda loendi algusse.

    ![][4]

3. Ka dialoogis **Atribuudid** : valige **Suunatud Runtimes** ja klõpsake nuppu **Uus**.

4. Dialoogiboksis **Uus serveri käitusaja keskkonnas** valida näiteks **Apache Tomcat v7.0** server ja klõpsake nuppu **edasi**. Dialoogiboksis **Tomcat serveri** määratud **nimi** `Apache Tomcat v7.0`, ja seadke **Tomcat installikaust** kataloogi Tomcat serveri, mida soovite kasutada versiooni installitud.

    ![][5]

    Klõpsake nuppu **valmis**.

5. Seejärel naasete lehele **Suunatud Runtimes** dialoogiboks **Atribuudid** . Valige **Apache Tomcat v7.0**ja seejärel klõpsake nuppu **OK**.

    ![][6]

6. Klõpsake menüü Eclipse **käivitada** **käivitada**. Valige dialoogiboksis **Nimega käivitada** **Server töötab**. Valige dialoogiboksis **käivitada serveri** **Tomcat v7.0 Server**.

    ![][7]

    Klõpsake nuppu **valmis**.

7. Kui rakendus töötab, näete **JSPHello** lehel kuvatakse localhost aknas Eclipse (`http://localhost:8080/JSPHello/`), kus on kuvatud järgmine teade:

    `Hello World, the time is Tue Mar 24 23:21:10 GMT 2015`


#### <a name="export-the-application-as-a-war"></a>Rakenduse eksportida vähe

Web projekti faile eksportida web arhiivifail (sõja) nii, et peate selle juurutama web appi. Järgmised web projekti faile asuda WebContent kaustas:

    META-INF
    WEB-INF
    index.jsp

1. Paremklõpsake WebContent kausta ja valige **ekspordi**.

2. Klõpsake dialoogiboksis **Eksportimiseks valige** **Web > sõda** faili ja seejärel klõpsake nuppu **edasi**.

3. Dialoogiboksis **sõda eksportimiseks** valige src kataloogi praeguse projekti ja kaasata sõda faili lõpus nime. Näiteks:

    `<project-path>/JSPHello/src/JSPHello.war`

Juurutamine sõda failide kohta leiate lisateavet teemast [Azure rakenduse teenuse veebirakenduste Java rakenduse lisamine](web-sites-java-add-app.md).


### <a name="deploying-the-hello-world-application-using-ftp"></a>Tere tulemast maailma rakenduse abil FTP juurutamine

Valige muu FTP klient avaldada rakendus. See toiming kirjeldab kaks võimalust: Kudu konsooli sisseehitatud Azure'i; ja FileZilla populaarsed tööriista abil on mugav, graafiline kasutajaliides.

> **Märkus:** Azure'i tööriistakomplekt Eclipse toetab salvestusruumi kontod ja pilveteenustega, kuid ei toeta praegu juurutamise veebirakenduste. Saate juurutada salvestusruumi kontod ja pilveteenustega abil mõnda Azure juurutamise projekti [loomise Tere maailma taotluse Azure Eclipse](http://msdn.microsoft.com/library/azure/hh690944.aspx)kirjeldatud, kuid mitte veebirakenduste. Kasutada muid viise, nt FTP või GitHub oma veebirakenduse faile edastada.

> **Märkus:** Me ei soovita FTP kaudu Windows Käsuviip (käsurea FTP.EXE kasuliku, mis on Windows). FTP kliendid, mis kasutavad active FTP, nt FTP.EXE, sageli ei tööta tulemüürid üle. Aktiivne FTP määrab sisemine LAN-põhiste aadress, millele tõenäoliselt ei serverist ühenduse.

Lisateavet rakenduse teenuse web Appi abil FTP juurutuse kohta leiate järgmistest teemadest:

- [Kasutades mõnda FTP kasuliku juurutamine](web-sites-deploy.md)


#### <a name="set-up-deployment-credentials"></a>Juurutamise identimisteabe häälestamine

Veenduge, et käivitate **AzureWebDemo** rakenduse web appi loomiseks. Te faile selles asukohas.

1. Klassikaline portaali sisse logida ja klõpsake nuppu **Web Apps**. Veenduge, et **WebDemoWebApp** kuvatakse loendis web Appsi ja veenduge, et see töötab. Klõpsake oma **armatuurlaua** lehe avamiseks **WebDemoWebApp** .

2. **Armatuurlaua** lehe **Kiirülevaate lühidalt**, klõpsake jaotises **häälestamine juurutamise mandaat** (kui teil on juba juurutamise mandaat, see loeb **lähtestamine juurutamise mandaat**).

    Juurutamise mandaat on seostatud Microsofti kontot. Peate määrama kasutajanime ja parooli, mille abil saate juurutada Git ja FTP abil. Saate neid mandaate juurutada kõigi Azure'i tellimused oma Microsofti kontoga seotud mis tahes web app. Dialoogiboksis Git ja FTP juurutamise mandaati ja kasutajanimi ja parool edaspidiseks kasutamiseks salvestada.


#### <a name="get-ftp-connection-information"></a>FTP ühenduse teavet

FTP abil saate juurutada rakenduse failid äsja loodud web appi, peate ühenduse teabe saamiseks. On kaks võimalust ühenduse teabe saamiseks. Üks võimalus on külastage **web appi armatuurlaualehe;** Teine võimalus on allalaadimiseks veebis rakenduse avaldamine profiili. Avalda profiil on teenuses Azure rakendus oma veebirakenduste teavet, nt FTP hosti nimi ja sisselogimisteave identimisteabe sisaldava XML-faili. Saate mis tahes web app kõik tellimused Azure'i konto, mitte ainult see kohtumine juurutada see kasutajanimi ja parool.

Web appi blade [Azure portaali][]saamiseks FTP ühenduse teave:

1. Klõpsake jaotises **Essentials**, otsimine ja kopeerige **FTP hostname (hostinimi)**. See on sarnane URI `ftp://waws-prod-bay-NNN.ftp.azurewebsites.windows.net`.

2. Jaotises **Essentialsi**, otsimine ja kopeerige **FTP/juurutamise kasutajanimi**. See on kujul *webappname\deployment-kasutajanimi*; näiteks `WebDemoWebApp\deployer77`.

Avalda profiili saamiseks FTP ühenduse teave:

1. Web appi labale nuppu **Too avaldada profiili**. See on kohalikule kettale .publishsettings faili alla laadida.

2. Avage fail .publishsettings XML-i redaktoris või tekstiredaktoris ja otsige üles soovitud `<publishProfile>` element, mis sisaldab `publishMethod="FTP"`. See peaks välja nägema umbes selline:

        <publishProfile
            profileName="WebDemoWebApp - FTP"
            publishMethod="FTP"
            publishUrl="ftp://waws-prod-bay-NNN.ftp.azurewebsites.windows.net/site/wwwroot"
            ftpPassiveMode="True"
            userName="WebDemoWebApp\$WebDemoWebApp"
            userPWD="<deployment-password>"
            ...
        </publishProfile>

3. Pange tähele, et web appi `publishProfile` sätted kaardi FileZilla saidi halduri sätted järgmiselt:

- `publishUrl`on sama mis **FTP hosti nimi**, **Host**seatud väärtuse.
- `publishMethod="FTP"`tähendab, et seada **protokoll** **FTP - File Transfer Protocol**ja **krüptimise** **lihtsat FTP**kasutamiseks.
- `userName`ja `userPWD` on tegelik kasutajanime ja parooli väärtused määratud juurutamise identimisteabe reset võtmed. `userName`sama, mis on **juurutamise / FTP kasutaja**. Need vastendamine **kasutaja** ja **parooli** FileZilla.
- `ftpPassiveMode="True"`tähendab, et FTP-saidi kasutab passiivne FTP saatmine; Valige vahekaardil **Sätted edastamine** **passiivne** .


#### <a name="configure-the-web-app-to-host-a-java-application"></a>Java teenuserakenduse hostimiseks Web Appi konfigureerimine

Rakenduse avaldamiseks peate mõne konfiguratsiooni sätteid muuta nii, et veebirakenduse majutada Java rakenduse.

1. Klassikaline portaalis minge web appi **armatuurlaua** leht ja klõpsake nuppu **Konfigureeri**. Lehe **konfigureerimine** määrake järgmised sätted.

2. **Java** versioon on vaikimisi **välja**; Valige Java versioon oma rakenduse eesmärgid; näiteks 1.7.0_51. Pärast seda, samuti veenduge, et **Web container** on määratud Tomcat serveri versioon.

3. **Vaikimisi dokumendid**, lisage index.jsp ja teisaldada selle loendi algusse. (Vaikimisi faili web apps on hostingstart.html.)

4. Klõpsake nuppu **Salvesta**.


#### <a name="publish-your-application-using-kudu"></a>Rakenduse abil Kudu avaldamine

Avaldamine rakenduse üheks võimaluseks on kasutada Kudu konsooli silumine Azure'i sisse ehitatud. Kudu teadaolevalt ühed ja rakenduse teenuse veebirakenduste ja Tomcat Server. Konsooli web appi juurde sirvimise URL järgmisel kujul:

`https://<webappname>.scm.azurewebsites.net/DebugConsole`

1. Selle toimingu jaoks Kudu konsooli asub järgmine URL; Liikuge sirvides selle kohta:

    `https://webdemowebapp.scm.azurewebsites.net/DebugConsole`

2. Valige ülemisel menüüs **konsooli silumine > CMD**.

3. Konsooli käsurea, liikuge `/site/wwwroot` (või klõpsake `site`, siis `wwwroot` directory vaate ülaosas, lehe):

    `cd /site/wwwroot`

4. Kui määrate **Java versiooni**, tuleks Tomcat server luua veebirakenduste kataloog. Konsooli käsurea, liikuge veebirakenduste kataloogi:

    `mkdir webapps`

    `cd webapps`

5. Lohistage JSPHello.war kaudu `<project-path>/JSPHello/src/` ja kukutage see Kudu directory vaatesse jaotises `/site/wwwroot/webapps`. Pole lohistage see "Lohistage siia üles laadida ja zip" ala, kuna Tomcat pakkige see lahti.

  ![][8]

Esimese JSPHello.war kuvatakse alal directory ise:

  ![][9]

Lühikese aja jooksul (ilmselt vähem kui 5 minuti) Tomcat Server on pakkige see lahti sõda faili lahti JSPHello kataloogi. Klõpsake juurkaust kas index.jsp on lahti pakitud ning kopeeritud seal. Sel juhul avage uuesti veebirakenduste kataloogi näha, kas lahti JSPHello kataloogi on juba loodud. Kui te ei näe neid üksusi, oodake ja korrake.

  ![][10]


#### <a name="publish-your-application-using-filezilla-optional"></a>Avaldamine rakenduse abil FileZilla (valikuline)

Mõnda muud tööriista, mille abil saate avaldada rakendus on FileZilla populaarsed kolmanda osapoole FTP klient mugav, graafiline kasutajaliides. Saate alla laadida ja installida FileZilla [http://filezilla-project.org/](http://filezilla-project.org/) kaudu, kui teil pole veel seda. Kliendi kasutamise kohta lisateabe saamiseks vt [FileZilla dokumentatsiooni](https://wiki.filezilla-project.org/Documentation) ja selle ajaveebikirje [FTP kliendid – osa 4: FileZilla](http://blogs.msdn.com/b/robert_mcmurray/archive/2008/12/17/ftp-clients-part-4-filezilla.aspx).

1. FileZilla, klõpsake **Fail > Saidihalduril**.
2. Klõpsake dialoogiboksis **Manageri saidi** **Uus sait**. **Valige kirje** palub teil nimi kuvatakse uus tühi FTP sait. Selle toimingu jaoks nimi `AzureWebDemo-FTP`.

    Klõpsake vahekaardi **üldist** , määrake järgmised sätted.
    - **Host:** Sisestage **FTP Host Name** , mille kopeerisite armatuurlaualt.
    - **Port:** (See väli tühjaks jätta, nagu see on passiivne üleandmine ja server määrab kasutada pordi.)
    - **Protocol (protokoll):** FTP File Transfer Protocol (protokoll)
    - **Krüptimise:** Tavaline FTP kasutamine
    - **Sisselogimise tüüp:** Tavaline
    - **Kasutaja:** Sisestage juurutamise / FTP kasutaja, mille te kopeerisite oma armatuurlaud. See on täielik FTP kasutajanimi, mis on vorm *webappname\username*.
    - **Parool:** Sisestage parool, kui seate juurutamise identimisteabe määratud.

    Valige vahekaardil **Sätted edastamine** **passiivne**.

3. Klõpsake nuppu **Loo ühendus**. Kui õnnestus, FileZilla's konsooli kuvatakse mõne `Status: Connected` sõnumi ja probleem on `LIST` käsk kataloogi sisu.

4. Valige paneelil **kohaliku** saidi allikas kataloog, kus asub JSPHello.war faili; tee on järgmine:

    `<project-path>/JSPHello/src/`

5. **Remote** saidi paani, valige kaust, kuhu. Juurutamist sõda faili selle `webapps` kataloogi alusel web appi juurkausta. Liikuge `/site/wwwroot`, paremklõpsake `wwwroot`, ja klõpsake nuppu **Loo kataloogi**. Kataloogi nime `webapps` ja sisestage selle kausta.

6. Kõne JSPHello.war, et `/site/wwwroot/webapps`. Valige JSPHello.war **kohaliku** faili loendis, paremklõpsake seda ja valige **üles laadida**. Peaksite nägema see kuvatakse `/site/wwwroot/webapps`.

7. Kui kopeerite JSPHello.war veebirakenduste kataloogi, Tomcat Server automaatselt lahti (pakkige lahti) failis sõda failid. Kuigi Tomcat Server algab lahtipakkimine peaaegu kohe, võib võtta kaua aega (võib-olla tundides) faile kuvada FTP klient.


#### <a name="run-the-hello-world-application-on-the-web-app"></a>Käivitage rakendus Tere, maailm Web App

1. Pärast sõda faili üles laadida ja kinnitatud Tomcat server on loodud mõne lahti `JSPHello` directory, liikuge sirvides `http://webdemowebapp.azurewebsites.net/JSPHello` rakenduse käivitamiseks.

    > **Märkus:** Kui klõpsate nuppu **Sirvi** klassikaline portaali kaudu, võidakse kuvada vaikimisi veebilehel räägivad "selle vastavalt Java veebirakenduse loomine õnnestus." Peate selleks, et vaadata rakenduse väljundi vaikimisi veebilehe asemel veebilehe värskendamine.

2. Kui rakendus töötab, peaksite nägema järgmine väljund Veebileht:

    `Hello World, the time is Tue Mar 24 23:21:10 GMT 2015`


#### <a name="clean-up-azure-resources"></a>Puhasta Azure ressursid

See toiming loob rakenduse teenuse web app. Kas arve ressursi seni, kuni see on olemas. Kui te ei plaani jätkata web appi kasutamine testimiseks või arengu, kaaluge peatamine või kustutage see. Veebirakenduse, mis on peatatud endiselt tekivad krediitkaardilt, kuid saate selle igal ajal taaskäivitada. Web appi kustutamine kustutab kõik andmed, saate selle alla laadinud.

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

  [1]: ./media/java-create-azure-website-using-java-sdk/eclipse-maven-repositories-rebuild-index.png
  [2]: ./media/java-create-azure-website-using-java-sdk/eclipse-new-java-class.png
  [3]: ./media/java-create-azure-website-using-java-sdk/eclipse-new-dynamic-web-project.png
  [4]: ./media/java-create-azure-website-using-java-sdk/eclipse-java-build-path.png
  [5]: ./media/java-create-azure-website-using-java-sdk/eclipse-targeted-runtimes-tomcat-server.png
  [6]: ./media/java-create-azure-website-using-java-sdk/eclipse-targeted-runtimes-properties-page.png
  [7]: ./media/java-create-azure-website-using-java-sdk/eclipse-run-on-server.png
  [8]: ./media/java-create-azure-website-using-java-sdk/kudu-console-drag-drop.png
  [9]: ./media/java-create-azure-website-using-java-sdk/kudu-console-jsphello-war-1.png
  [10]: ./media/java-create-azure-website-using-java-sdk/kudu-console-jsphello-war-2.png
 

[Azure'i rakendust Service]: http://go.microsoft.com/fwlink/?LinkId=529714
[Web platvormi Installer]: http://go.microsoft.com/fwlink/?LinkID=252838
[Azure'i tööriistakomplekt Eclipse]: https://msdn.microsoft.com/library/azure/hh690946.aspx
[Azure'i klassikaline portaal]: https://manage.windowsazure.com
[Mis on Azure AD kataloog]: http://technet.microsoft.com/library/jj573650.aspx
[Luua ja üles laadida serdi haldus Azure]: ../cloud-services/cloud-services-certs-create.md
[Võti ja serdi Haldustööriista (keytool)]: http://docs.oracle.com/javase/6/docs/technotes/tools/windows/keytool.html
[WebSiteManagementClient]: http://azure.github.io/azure-sdk-for-java/com/microsoft/azure/management/websites/WebSiteManagementClient.html
[WebSpaceNames]: http://dl.windowsazure.com/javadoc/com/microsoft/windowsazure/management/websites/models/WebSpaceNames.html
[Azure'i portaal]: https://portal.azure.com
