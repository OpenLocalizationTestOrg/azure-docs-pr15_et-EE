<properties
   pageTitle="Arendus ja testimine keskkonnas | Microsoft Azure'i"
   description="Saate teada, kuidas Azure'i ressursihaldur mallide abil saate kiiresti ja pidevalt loomine ja kustutamine arendamise ja testida keskkonnas."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="01/22/2016"
   ms.author="tomfitz"/>

# <a name="development-and-test-environments-in-microsoft-azure"></a>Microsoft Azure Arendus ja testimine keskkonnas

Kohandatud rakendused on tavaliselt juurutatud mitme arendamise ja testimise keskkonnas enne tootmisse juurutamine. Keskkonnas loomisel kohapealne ressursside on kas hangitud või eraldatud iga keskkonna iga rakenduse jaoks. Keskkondade sisaldavad sageli mitme füüsilise või virtuaalmasinates teatud konfiguratsioone käsitsi juurutatud või keerukas automatiseerimise skriptide. Kasutuselevõttu sageli tundi ja tulemuseks vastuolulised kogemused konfiguratsioone üle keskkonnas.

## <a name="scenario"></a>Stsenaarium ##

Kui olete ette arendamise ja Microsoft Azure test keskkonnas, maksate ressursse, mida kasutada.  Selles artiklis selgitatakse, kuidas kiiresti ja süsteemne saate luua, säilitamiseks ja kustutada arengu ja testida keskkonnas Azure'i ressursihaldur salvestamine ja parameetri faile, nagu on näidatud allpool.

![Stsenaarium](./media/solution-dev-test-environments/scenario.png)

Eespool näidatud kolme arendamise ja testimise keskkonnas.  Iga on veebirakenduse ja SQL-i andmebaasi ressursse, mis on määratud malli faili.  Rakenduse ja andmebaasi iga keskkonnas nimed on erinevad ja on määratud parameetri kordumatu failid iga keskkond.  

Kui olete tuttav Azure'i ressursihaldur põhimõtet, on soovitatav lugeda [Azure'i ressursihaldur ülevaate](azure-resource-manager/resource-group-overview.md) artikli enne selle artikli lugemist.

Soovite esmalt läbida ilma lugemist viidatud artikleid kiiresti saada kogemusi Azure'i ressursihaldur mallide kasutamine loetletud selles artiklis toodud juhiseid. Pärast seda, kui olete olnud juhised kui, kuvatakse saama vastused küsimustele, mis tekkisid oma esimese enamiku aega kuni katsetamine täiendavaid juhiseid ja viidatud artiklite lugemine.

## <a name="plan-azure-resource-use"></a>Azure'i ressursi kasutamise kavandamine
Kui olete rakenduse kõrge taseme kujundus, saate määrata:

- Azure'i ressursside rakenduse kaasata. Võivad rakenduse koostamine ja juurutada Azure Web App Azure'i SQL-andmebaasi.  Võib luua rakenduse virtuaalmasinates PHP ja MySQL-i või IIS-i ja SQL serveri või muude komponentide abil. [Azure'i rakendust Service, pilveteenustega, ja Virtuaalmasinates võrdlus]( app-service-web/choose-web-site-cloud-service-vm.md) artikkel aitab teil otsustada, millist Azure ressursse, võiksite kasutada rakenduse.
- Millist teenuse taseme nõuetele nagu kättesaadavus, turvalisus ja skaala, mis vastavad teie rakendus.

## <a name="download-an-existing-template"></a>Olemasoleva malli allalaadimine
Mõni Azure ressursihaldur mall määratleb kõigi Azure ressursse, mis kasutab teie rakendus. Mitu malli juba olemas, et saate juurutada otse portaalis Azure, või alla laadida, muutmist ja salvestage oma rakenduse koodi Juhtelemendi allikas süsteemis.  Täitke juhised alla laadida olemasoleva malli.

1. Saate valida olemasoleva malli [Azure'i Kiirjuhend Mallid](https://github.com/Azure/azure-quickstart-templates/) GitHub hoidla. Klõpsake loendis kuvatakse kausta "[201-web-rakenduse-sql-andmebaasi](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-sql-database)". Kuna paljud kohandatud rakenduste hulka veebirakenduse ja SQL-andmebaasi, kasutatakse selle malli näidisena ülejäänud käesoleva artikli aidata teil mõista mallide kasutamise kohta. Selles artiklis selgitatakse täielikult kõik see mall loob ja konfigureerib väljapoole on, kuid kui kavatsete selle abil luua oma organisatsiooni tegeliku keskkonnas, peaksite täielikult mõista [sätte web Appi abil SQL-andmebaasi](app-service-web/app-service-web-arm-with-sql-database-provision.md) artiklist.
Märkus: See artikkel on [201-web-rakenduse-sql-andmebaasi](https://github.com/Azure/azure-quickstart-templates/tree/3f24f7b7e1e377538d1d548eaa6eab2851a21810/201-web-app-sql-database) malli detsember 2015 versiooni jaoks kirjutatud. Mis allolevate linkide käsk Mall ja parameetri failid on selle versiooni mall.
2. Klõpsake faili [azuredeploy.json](https://github.com/Azure/azure-quickstart-templates/tree/3f24f7b7e1e377538d1d548eaa6eab2851a21810/201-web-app-sql-database/azuredeploy.json) 201-web-rakenduse-sql-andmebaasi kausta sisu vaatamiseks. See on Azure ressursihaldur mallifail. 
3. Vaaterežiimis, klõpsake nuppu "[Raw](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/3f24f7b7e1e377538d1d548eaa6eab2851a21810/201-web-app-sql-database/azuredeploy.json)". 
4. Hiirega, valige selle faili sisu ja salvestada oma arvutisse fail nimega "TestApp1 Template.json." 
5. Malli sisu ja pange tähele järgmist:
 - **Ressursside** jaotis: selles jaotises määratleb selle malli abil loodud Azure ressursse tüüpi. Muu hulgas ressurss, see mall loob [Azure Web App](app-service-web/app-service-web-overview.md) ja [Azure'i SQL-andmebaasi](sql-database/sql-database-technical-overview.md) ressursid. Kui soovite käivitada ja hallata web ja SQL-i serverite virtuaalmasinates, "[iis-2vm sql-1vm](https://github.com/Azure/azure-quickstart-templates/tree/master/iis-2vm-sql-1vm)" või "[lamp-rakenduse](https://github.com/Azure/azure-quickstart-templates/tree/master/lamp-app)" malle saate kasutada, kuid selles artiklis antud juhiseid on [201-web-rakenduse-sql-andmebaasi](https://github.com/Azure/azure-quickstart-templates/tree/3f24f7b7e1e377538d1d548eaa6eab2851a21810/201-web-app-sql-database) malli põhjal.
 - **Parameetrite** jaotis: selles jaotises määratleb iga ressursi saab konfigureerida parameetrid. Mõned parameetrid määratud mall on "defaultValue" atribuudid, teised mitte. Azure'i ressursid malli juurutamisel peate sisestama väärtused kõigi parameetrite, millel on määratud malli defaultValue atribuudid.  Kui te ei esita väärtused parameetreid omadustega defaultValue, kasutatakse defaultValue parameetri malli jaoks määratud väärtust.

Malli määratleb Azure ressursside luuakse ja parameetrid iga ressursi saab konfigureerida. Kohta lisateavet mallide ja kuidas kujundada enda poolt [Azure'i ressursihaldur Mallid kujundamise head tavad](best-practices-resource-manager-design-templates.md) artikli lugemist.

## <a name="download-and-customize-an-existing-parameter-file"></a>Laadige alla ja parameetri olemasoleva faili kohandamine

Kui soovite selle *sama* Azure ressursse, mis on loodud iga keskkonnas, tõenäoliselt soovite ressursse, et olla *erinevad* iga keskkonna konfiguratsioon.  See on, kui parameeter failid on saadaval. Looge parameetri failid, mis sisaldavad kordumatuid väärtusi iga keskkonnas, tehes järgmist.   

1. 201-web-rakenduse-sql-andmebaasi kausta [azuredeploy.parameters.json](https://github.com/Azure/azure-quickstart-templates/tree/3f24f7b7e1e377538d1d548eaa6eab2851a21810/201-web-app-sql-database/azuredeploy.parameters.json) faili sisu kuvamine. See on parameetri faili salvestasite eelmises jaotises malli faili. 
2. Vaaterežiimis, klõpsake nuppu "[Raw](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/3f24f7b7e1e377538d1d548eaa6eab2851a21810/201-web-app-sql-database/azuredeploy.parameters.json)". 
3. Hiire abil valige selle faili sisu ja salvestage need kolm eraldi failid teie arvutis järgmised nimed:
 - TestApp1-parameetrite-Development.json
 - TestApp1-parameetrite-Test.json
 - TestApp1 parameetrite – eelnevalt Production.json

3. Teksti või JSON redaktori abil arengu keskkonna parameeter faili loodud sammus 3, asendades paremal **parameetrite** allpool toodud *väärtused* paremas servas parameetrite väärtused failis loetletud väärtused redigeerimine: 
 - **siteName**: *TestApp1DevApp*
 - **hostingPlanName**: *TestApp1DevPlan*
 - **siteLocation**: *Kesk-USA*
 - **Serveri nimi**: *testapp1devsrv*
 - **serverLocation**: *Kesk-USA*
 - **administratorLogin**: *testapp1Admin*
 - **administratorLoginPassword**: *asendage parooli*
 - **databaseName**: *testapp1devdb*

4. Teksti või JSON-redaktor redigeerimine keskkonna parameeter Proovifail loodud sammus 3, asendades selle paremas servas parameetrite väärtused *väärtused* failis loetletud väärtused loetletud **parameetrite** allpool paremal:
 - **siteName**: *TestApp1TestApp*
 - **hostingPlanName**: *TestApp1TestPlan*
 - **siteLocation**: *Kesk-USA*
 - **Serveri nimi**: *testapp1testsrv*
 - **serverLocation**: *Kesk-USA*
 - **administratorLogin**: *testapp1Admin*
 - **administratorLoginPassword**: *asendage parooli*
 - **databaseName**: *testapp1testdb*

5. Samm 3 loodud tootmiseelse parameetri faili teksti või JSON redaktori abil redigeerida.  Asendage kogu faili sisu mis on allpool:

        {
          "$schema" : "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
          "contentVersion" : "1.0.0.0",
          "parameters" : {
        "administratorLogin" : {
          "value" : "testApp1Admin"
        },
        "administratorLoginPassword" : {
          "value" : "replace with your password"
        },
        "databaseName" : {
          "value" : "testapp1preproddb"
        },
        "hostingPlanName" : {
          "value" : "TestApp1PreProdPlan"
        },
        "serverLocation" : {
          "value" : "Central US"
        },
        "serverName" : {
          "value" : "testapp1preprodsrv"
        },
        "siteLocation" : {
          "value" : "Central US"
        },
        "siteName" : {
          "value" : "TestApp1PreProdApp"
        },
        "sku" : {
          "value" : "Standard"
        },
            "requestedServiceObjectiveName" : {
              "value" : "S1"
        }
          }
        }

Ülaltoodud tootmiseelse parameetrid faili **sku** ja **requestedServiceObjectiveName** parameetrid olid *lisanud*, nad ei lisada Arendus ja testimine parameetrite failid. Selle põhjuseks on määratud malli, parameetrite vaikeväärtuste ja arendus ja testimine keskkonnas, kasutatakse vaikeväärtused, kuid eelse valmistamisel kasutatakse keskkonnas-vaikimisi järgmiste parameetrite väärtused.

Põhjus-vaikeväärtuste kasutatakse järgmiste parameetrite tootmiseelse keskkonnas on testida, mida te soovida, et tootmiskeskkonnast nii, et nad saavad katsetada parameetrite.  Need kõik parameetrid seotud Azure [Web Appi majutusteenuse lepingud](https://azure.microsoft.com/pricing/details/app-service/), või **SKU-ga** ja Azure [SQL-andmebaasi](https://azure.microsoft.com/pricing/details/sql-database/)või **requestedServiceObjectiveName** , mida kasutatakse rakendus.  Erinevate SKU-de jaoks ja teenusenimesid eesmärk on erinevad kulud ja funktsioonid ja toetavad erinevaid taseme mõõdikute.

Järgmises tabelis on loetletud järgmiste parameetrite määratud Mall ja väärtused, mida asemel vaikeväärtused tootmiseelse parameetrite failis kasutatakse vaikeväärtused.

| Parameetri | Vaikeväärtus | Faili parameetriväärtus |
|---|---|---|
| **SKU-ga** | Tasuta | Standard |
| **requestedServiceObjectiveName** | S0 | S1 |

## <a name="create-environments"></a>Looge keskkonnas
Kõik Azure ressursse tuleb luua ka [Azure ressursirühma](azure-resource-manager/resource-group-overview.md). Ressursi rühmad võimaldavad rühmitada Azure ressursse nii, et neid saab hallata ühiselt.  [Õigusi](./active-directory/role-based-access-built-in-roles.md) saab määrata ressursside rühmad nii, et teatud oma asutuses töötavaid inimesi saate loomine, muutmine, kustutamine või vaadata neid ja neid ressursse.  Teatiste ja arveldusteabe ressursirühma ressursside saab vaadata [Azure portaali](https://portal.azure.com). Ressursi rühmad on loodud Azure [piirkond](https://azure.microsoft.com/regions/).  Selles artiklis luuakse Kesk-USA piirkonna kõik ressursid. Tegelik keskkonnas loomise käivitamisel saate valida piirkond, mis vastab teie vajadustele kõige paremini. 

Looge ressursi rühmad iga keskkonna, kasutades mõnda järgmistest viisidest.  Kõik meetodid saavutada sama tulemuse.

###<a name="azure-command-line-interface-cli"></a>Azure'i käsurea liides (CLI)

Veenduge, et teil on selle CLI [installitud](xplat-cli-install.md) kas Windows, OS X või Linuxi arvutisse ja mida te olete [ühendatud](xplat-cli-connect.md) [konto Azure AD](./active-directory/active-directory-how-subscriptions-associated-directory.md) (nimetatakse ka töökoha või kooli kontoga) Azure tellimusele. Tippige käsureal CLI käsu alla, et luua ressursirühm arengu keskkonnas.

    azure group create "TestApp1-Development" "Central US"

Kui see ei õnnestu tagastab järgmine käsk:

    info:    Executing command group create
    + Getting resource group TestApp1-Development
    + Creating resource group TestApp1-Development
    info:    Created resource group TestApp1-Development
    data:    Id:                  /subscriptions/uuuuuuuu-vvvv-wwww-xxxx-yyyy-zzzzzzzzzzzz/resourceGroups/TestApp1-Development
    data:    Name:                TestApp1-Development
    data:    Location:            centralus
    data:    Provisioning State:  Succeeded
    data:    Tags: null
    data:
    info:    group create command OK

Ressursirühm testi keskkonna loomiseks tippige allpool käsk:

    azure group create "TestApp1-Test" "Central US"

Ressursirühm tootmiseelse keskkonna loomiseks tippige allpool käsk:

    azure group create "TestApp1-Pre-Production" "Central US"

###<a name="powershell"></a>PowerShelli

Veenduge, et teil on Azure PowerShelli 1.01 või suurem Windowsi arvutisse installitud ja ühendatud [konto Azure AD](./active-directory/active-directory-how-subscriptions-associated-directory.md) (nimetatakse ka töökoha või kooli kontoga) tellimuse, [Kuidas installida ja konfigureerida Azure PowerShelli](powershell-install-configure.md) artiklis kirjeldatud. PowerShelli käsureale, tippige käsu alla, et luua ressursirühm arengu keskkonnas.

    New-AzureRmResourceGroup -Name TestApp1-Development -Location "Central US"

Kui see ei õnnestu tagastab järgmine käsk:

    ResourceGroupName : TestApp1-Development
    Location          : centralus
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/uuuuuuuu-vvvv-wwww-xxxx-yyyy-zzzzzzzzzzzz/resourceGroups/TestApp1-Development

Ressursirühm testi keskkonna loomiseks tippige allpool käsk:

    New-AzureRmResourceGroup -Name TestApp1-Test -Location "Central US"

Ressursirühm tootmiseelse keskkonna loomiseks tippige allpool käsk:

    New-AzureRmResourceGroup -Name TestApp1-Pre-Production -Location "Central US"

###<a name="azure-portal"></a>Azure'i portaal

1. Logige sisse [Azure portaali](https://portal.azure.com) konto [Azure AD](./active-directory/active-directory-how-subscriptions-associated-directory.md) (nimetatakse ka tööl või koolis). Klõpsake nuppu Uus--> haldus--> ressursirühm ja sisestage väljale ressursi rühma nimi "TestApp1 arendamine", valige tellimuse ja valige "Keskse meie" väljale ressursi rühma asukoht nagu on näidatud järgmisel pildil.
   ![Portaal](./media/solution-dev-test-environments/rgcreate.png)
2. Ressursi rühma loomiseks nuppu Loo.
3. Klõpsake nuppu Sirvi, Keri ressursi rühmade loendi ja klõpsake ressursi rühmad, nagu allpool näidatud.
   ![Portaal](./media/solution-dev-test-environments/rgbrowse.png) 
4. Pärast klõpsates ressursi rühmad kuvatakse ressursi rühmade tera oma uue ressursirühma.
   ![Portaal](./media/solution-dev-test-environments/rgview.png)
5. TestApp1-testi loomine ja TestApp1 eel-tekitamiseks ressursi rühma samal viisil loodud ülaltoodud TestApp1-areng ressursirühma.

##<a name="deploy-resources-to-environments"></a>Ressursside juurutama keskkonnas

Juurutada Azure ressursid ressursside rühmad iga keskkonna mallifail lahendus ja parameetri failide abil iga keskkonna, kasutades ühte järgmistest viisidest.  Allpool on mõlema meetodi saavutada sama tulemuse.

###<a name="azure-command-line-interface-cli"></a>Azure'i käsurea liides (CLI)

Käsureal CLI tippige käsk allpool juurutada ressursse ressursirühma loodud arenduskeskkond, asendades [tee] eelmiste juhiste järgi salvestatud failide tee.

    azure group deployment create -g TestApp1-Development -n Deployment1 -f [path]TestApp1-Template.json -e [path]TestApp1-Parameters-Development.json 

Pärast kuvatakse teade "Ootab juurutuse lõpuleviimiseks" paar minutit, tulemuseks käsu järgmist, kui see ei õnnestu:

    info:    Executing command group deployment create
    + Initializing template configurations and parameters
    + Creating a deployment
    info:    Created template deployment "Deployment1"
    + Waiting for deployment to complete
    data:    DeploymentName     : Deployment1
    data:    ResourceGroupName  : TestApp1-Development
    data:    ProvisioningState  : Succeeded
    data:    Timestamp          : XXXX-XX-XXT20:20:23.5202316Z
    data:    Mode               : Incremental
    data:    Name                           Type          Value
    data:    -----------------------------  ------------  ----------------------------
    data:    siteName                       String        TestApp1DevApp
    data:    hostingPlanName                String        TestApp1DevPlan
    data:    siteLocation                   String        Central US
    data:    sku                            String        Free
    data:    workerSize                     String        0
    data:    serverName                     String        testapp1devsrv
    data:    serverLocation                 String        Central US
    data:    administratorLogin             String        testapp1Admin
    data:    administratorLoginPassword     SecureString  undefined
    data:    databaseName                   String        testapp1devdb
    data:    collation                      String        SQL_Latin1_General_CP1_CI_AS
    data:    edition                        String        Standard
    data:    maxSizeBytes                   String        1073741824
    data:    requestedServiceObjectiveName  String        S0
    info:    group deployment create command OKx

Kui käsk ei õnnestu, lahendada tõrketeated ja proovige uuesti.  Levinud probleemide kasutavad parameetrite väärtused, mis kuulu Azure ressursipiirangute nime panna. Muud tõrkeotsingu näpunäited leiate artiklist [tõrkeotsing ressursi rühma juurutuste Azure](./resource-manager-troubleshoot-deployments-cli.md) .

CLI käsureale, tippige käsk allpool juurutada ressursid ressursirühma loodud testimiskeskkonnas, asendades [tee] eelmiste juhiste järgi salvestatud failide tee.

    azure group deployment create -g TestApp1-Test -n Deployment1 -f [path]TestApp1-Template.json -e [path]TestApp1-Parameters-Test.json

Käsureal CLI tippige käsk allpool juurutada ressursse ressursirühma loodud tootmiseelse keskkonnas, asendades [tee] eelmiste juhiste järgi salvestatud failide tee.

    azure group deployment create -g TestApp1-Pre-Production -n Deployment1 -f [path]TestApp1-Template.json -e [path]TestApp1-Parameters-Pre-Production.json
  
###<a name="powershell"></a>PowerShelli

Azure'i PowerShelli (1.01 või uuem versioon) Käsuviip, tippige käsk allpool juurutada ressursid ressursirühma loodud arenduskeskkond, asendades [tee] eelmiste juhiste järgi salvestatud failide tee.

    New-AzureRmResourceGroupDeployment -ResourceGroupName TestApp1-Development -TemplateFile [path]TestApp1-Template.json -TemplateParameterFile [path]TestApp1-Parameters-Development.json -Name Deployment1 

Pärast kuvatakse vilkuv kursor paar minutit, tulemuseks käsu järgmist, kui see ei õnnestu:

    DeploymentName    : Deployment1
    ResourceGroupName : TestApp1-Development
    ProvisioningState : Succeeded
    Timestamp         : XX/XX/XXXX 2:44:48 PM
    Mode              : Incremental
    TemplateLink      : 
    Parameters        : 
                        Name             Type                       Value     
                        ===============  =========================  ==========
                        siteName         String                     TestApp1DevApp
                        hostingPlanName  String                     TestApp1DevPlan
                        siteLocation     String                     Central US
                        sku              String                     Free      
                        workerSize       String                     0         
                        serverName       String                     testapp1devsrv
                        serverLocation   String                     Central US
                        administratorLogin  String                     testapp1Admin
                        administratorLoginPassword  SecureString                         
                        databaseName     String                     testapp1devdb
                        collation        String                     SQL_Latin1_General_CP1_CI_AS
                        edition          String                     Standard  
                        maxSizeBytes     String                     1073741824
                        requestedServiceObjectiveName  String                     S0        
                        
    Outputs           :

  Kui käsk ei õnnestu, lahendada tõrketeated ja proovige uuesti.  Levinud probleemide kasutavad parameetrite väärtused, mis kuulu Azure ressursipiirangute nime panna. Muud tõrkeotsingu näpunäited leiate artiklist [tõrkeotsing ressursi rühma juurutuste Azure](./resource-manager-troubleshoot-deployments-powershell.md) .

  PowerShelli käsureale, tippige käsk allpool juurutada ressursid ressursirühma loodud testimiskeskkonnas, asendades [tee] eelmiste juhiste järgi salvestatud failide tee.

    New-AzureRmResourceGroupDeployment -ResourceGroupName TestApp1-Test -TemplateFile [path]TestApp1-Template.json -TemplateParameterFile [path]TestApp1-Parameters-Test.json -Name Deployment1

  PowerShelli käsureale, tippige käsk allpool juurutada ressursid ressursirühma loodud tootmiseelse keskkonnas, asendades [tee] eelmiste juhiste järgi salvestatud failide tee.

    New-AzureRmResourceGroupDeployment -ResourceGroupName TestApp1-Pre-Production -TemplateFile [path]TestApp1-Template.json -TemplateParameterFile [path]TestApp1-Parameters-Pre-Production.json -Name Deployment1

Mallide ja parameetri faile saab versiooniga ja säilitada oma rakenduse koodiga süsteemis Juhtelemendi allikas.  Võib salvestada ka ülaltoodud skript failid ja salvestage need oma koodiga käsud.

> [AZURE.NOTE] Otse [sätte Web Appi abil SQL-andmebaasi](https://azure.microsoft.com/documentation/templates/201-web-app-sql-database/) artiklist "Juurutada Azure" nupu abil saate juurutada Azure mall.  Abi võite leida see mallide kohta teavet, kuid teha nii redigeerida, versiooni ja salvestada oma malli ja parameetrite väärtused koodiga rakenduse, et selles artiklis ei käsitleta seda meetodit täiendavad üksikasjad.

## <a name="maintain-environments"></a>Säilitada keskkonnas
Kogu arengu, konfiguratsiooni Azure ressursside erinevates keskkondades võib ebajärjekindlalt muuta tahtlikult või kogemata.  See võib põhjustada rakenduste arendamise tsükli tõrkeotsingu tarbetu ja probleemide lahendamiseks.

1. Avamisel [Azure portaali](https://portal.azure.com)keskkondade muuta. 
2. Logige sisse selle sama kontoga, mida kasutasite ülaltoodud juhiseid. 
3. Nagu on näidatud järgmisel pildil, klõpsake nuppu Sirvi--> ressursi rühmad (võimalik, et peate Kerige allapoole suvandini vt ressursi rühmad).
   ![Portaal](./media/solution-dev-test-environments/rgbrowse.png)
4. Pärast klõpsates ressursi rühmad ülaltoodud joonisel, kuvatakse ressursi rühmade tera ja kolme ressursi rühmade eelmises juhises loodud, nagu on näidatud järgmisel pildil. Klõpsake TestApp1-areng ressursirühm ja teile kuvatakse blade, kus on loetletud malli TestApp1-areng ressursi rühma juurutuse te täitnud eelmises juhises loodud ressursside.  Kustutage TestApp1DevApp veebirakenduse ressursside, klõpsates TestApp1DevApp tera TestApp1-areng ressursi rühma ja seejärel käsku Kustuta TestApp1DevApp Web Appi tera.
   ![Portaal](./media/solution-dev-test-environments/portal2.png)
5. Valige "Jah", kui portaali palub teil selle kohta, kas olete kindel, et kustutada ressurss. TestApp1-areng ressursi rühma tera sulgeda ja uuesti avada selle nüüd kuvatakse see ilma veebirakenduse äsja kustutatud.  Ressursirühma sisu on nüüd muu, kui nad peaksid olema. Saate proovida täpsemaks mitme ressursse kustutamine mitu ressursi rühma või isegi muutes konfiguratsioonisätted mõned ressursid. Ressursi kustutamiseks ressursirühma Azure portaali asemel võite kasutada PowerShelli [Eemaldamine-AzureResource](https://msdn.microsoft.com/library/azure/dn757676.aspx) käsu või funktsiooni või sama ülesande CLI käsu "azure ressursi Kustuta".
6. Saada kõik ressursse ja konfiguratsiooni, mis peaks olema tagasi, et nad peaksid olema riigi ressursside rühmad, uuesti juurutamine keskkonnas sama käsud, mida kasutasite [Deploy ressursse, mis keskkonnas](#deploy-resources-to-environments) jaotise abil ressursi rühmadesse, kuid asendada "Deployment1" ja "Deployment2."
7.  Nagu on näidatud jaotise kokkuvõte TestApp1-areng tera pildil näidatud etappi 4, näete, et veebirakenduse kustutamist portaalis eelmises etapis olemas uuesti, nagu mis tahes muud ressursid olete valinud kustutamiseks. Kui olete muutnud konfiguratsiooni, mis tahes ressursside, samuti saate kontrollida, et need on seatud tagasi väärtused parameeter failid liiga. Üks eeliseid juurutamine oma keskkonnad Azure'i ressursihaldur Mallid on, saate hõlpsalt uuesti juurutada tagasi, et mingi igal ajal.
8. Kui klõpsate "Viimase juurutamine" järgmisel pildil all asuva teksti, kuvatakse teile tera, et näitab juurutamise ajalugu ressursirühma.  Kuna saate kasutada nime "Deployment1" esimese juurutus- ja "Deployment2" teise juurutus, siis on teil kaks kirjet.  Juurutamine klõpsamisel kuvatakse tera, mis näitab iga juurutamiseks tulemusi.
  ![Portaal](./media/solution-dev-test-environments/portal3.png)



## <a name="delete-environments"></a>Kustutage keskkonnas
Kui olete lõpetanud, kasutades keskkonnas, peaksite kustutama nii, et teil pole tekkida kulude enam kasutate Azure ressursid.  Kustutamise keskkonnas on veelgi lihtsam, kui need loomine.  Eelmiste juhiste järgi, üksikud Azure'i ressursi rühmad on loodud iga keskkonna ja seejärel ressursid on juurutatud ressursside rühmadesse. 

Kustutage keskkonnas, kasutades mõnda järgmistest viisidest.  Kõik meetodid saavutada sama tulemuse.

### <a name="azure-cli"></a>Azure'i CLI

CLI viip, sisestage järgmine:

    azure group delete "TestApp1-Development"

Küsimise korral sisestage y ja seejärel vajutage sisestage soovitud arenduskeskkond ja kõik selle ressursid eemaldamine. Mõne minuti pärast tagastab järgmine käsk:

    info:    group delete command OK

Tippige käsuviiba CLI kaudu, kustutada ülejäänud keskkonnas järgmine:

    azure group delete "TestApp1-Test"
    azure group delete "TestApp1-Pre-Production"
  
### <a name="powershell"></a>PowerShelli

Azure'i PowerShelli (1.01 või uuem versioon) Käsuviip, tippige käsu alla, et kustutada ressursirühma ja selle sisu.    

    Remove-AzureRmResourceGroup -Name TestApp1-Development

Kui olete kindel, et soovite eemaldada ressursirühma Küsimisel sisestage y, millele järgneb sisestusklahvi (enter).

Tippige kustutada ülejäänud keskkonnas järgmine:

    Remove-AzureRmResourceGroup -Name TestApp1-Test
    Remove-AzureRmResourceGroup -Name TestApp1-Pre-Production

### <a name="azure-portal"></a>Azure'i portaal

1. Azure'i portaalis, liikuge ressursirühma nagu tegite eelmisi juhiseid. 
2. Valige TestApp1-areng ressursirühm ja klõpsake nuppu Kustuta TestApp1-areng ressursi rühma tera. Kuvatakse uus laba. Sisestage ressursi rühma nimi ja klõpsake nuppu Kustuta.
![Portaal](./media/solution-dev-test-environments/rgdelete.png)
3. Kustutamine TestApp1-testi ja TestApp1 eel-tekitamiseks ressursi rühma kustutamist TestApp1-areng ressursirühm samal viisil.

Sõltumata kasutate, ressursside rühmad ja kõik need ressursid pole enam olemas ja teil tuleb enam tekkida arvelduse ressursside kulude.  

Funktsiooni Azure'i minimeerimiseks teil tekivad [Azure automatiseerimine](automation/automation-intro.md) abil saate ajastada rakenduste arendamise käigus ressursi kasutamise kulud töö mis:

- Peatage virtuaalmasinates lõpus iga päev ja taaskäivitage neid iga päeva alguses.
- Kustutada kogu keskkonnas iga päeva lõpus ja uuesti loomiseks iga päeva alguses.
 
Nüüd, kui olete kogenud, kui lihtne on luua, säilitamiseks ja kustutada arendamise ja testida keskkonnas, saate lisateavet kohta, mida te just ei täpsemalt katsetamiset eespool antud juhiseid ja viiteid käesoleva artikli lugemist.

## <a name="next-steps"></a>Järgmised sammud

- [Volitatud esindaja haldus juhtelemendi](./active-directory/role-based-access-control-configure.md) erinevate ressursside iga keskkonna määrate Microsoft Azure AD rühmadele või kasutajatele teatud rollid, mida hüpikmenüüsid alamhulk Azure ressursid toimingute sooritamiseks.
- [Siltide määramine](resource-group-using-tags.md) ressursi rühmad iga keskkonna ja/või üksikute ressursse. Võib lisada ka "Keskkond" sildi ressursi rühm ja määrake selle väärtuseks on teie keskkonnas nimed vastaksid. Siltide võib olla eriti kasulik, kui korraldamiseks ressursside arveldus-või haldamiseks peate.
- Jälgimine teatiste ja ressursside rühma ressursid [Azure portaali](https://portal.azure.com)arveldus.
