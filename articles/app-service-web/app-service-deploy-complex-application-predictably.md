<properties
    pageTitle="Ettevalmistamine ja microservices ootuspäraselt sisse Azure juurutamine"
    description="Saate teada, kuidas juurutamine rakendust, mis koosneb microservices Azure'i rakendust Service ühtse ja hõlpsalt prognoosida viisil JSON ressursi Mallid ja PowerShelli skripti abil."
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor="jimbe"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="01/06/2016"
    ms.author="cephalin"/>


# <a name="provision-and-deploy-microservices-predictably-in-azure"></a>Ettevalmistamine ja microservices ootuspäraselt sisse Azure juurutamine #

Selle õpetuse näitab, kuidas ette valmistada ja juurutamine rakendust, mis koosneb [microservices](https://en.wikipedia.org/wiki/Microservices) [Azure'i rakendust Service](/services/app-service/) ühtse ja hõlpsalt prognoosida viisil JSON ressursi Mallid ja PowerShelli skripti abil. 

Kui ettevalmistamise ja juurutamine suure ulatusega rakendusi, mis koosneb väga sidumata microservices, korratavuse ja prognoositavuse on oluline edu. [Azure'i rakendust Service](/services/app-service/) võimaldab teil luua microservices, mis sisaldavad veebirakenduste, mobiilirakendused, API rakendused ja loogika rakenduste. [Azure'i ressursihaldur](../azure-resource-manager/resource-group-overview.md) võimaldab teil haldamine üksus, näiteks andmebaasi ressursi sõltuvused koos kõigi microservices ja andmeallika juhtelemendi sätted. Nüüd saate juurutada taotluse JSON Mallid ja lihtsat PowerShelli skripti abil. 

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="what-you-will-do"></a>Mida te teete ##

Õpetuses, juurutamist rakendus, mis sisaldab:

-   Kaks web apps (st kaks microservices)
-   Mõne taustväärtus SQL-andmebaas
-   Rakenduse sätted, ühendusstringi ja allikas
-   Rakenduse ülevaated, teatiste, autoscaling sätted

## <a name="tools-you-will-use"></a>Kasutage tööriistad ##

Selles õpetuses, kasutage järgmisi tööriistu. Kuna see ei ole täielik arutelu tööriistad, ma jääda lõpuni stsenaarium ja just teile lühike sissejuhatav iga, ja kus võite leida rohkem teavet. 

### <a name="azure-resource-manager-templates-json"></a>Azure'i ressursihaldur mallid (JSON) ###
 
Iga kord, kui loote web appi Azure'i rakendust Service, näiteks Azure'i ressursihaldur kasutab JSON malli loomiseks kogu ressursirühm komponent ressurssidega. Keerukate malli [Azure'i turuplatsi](/marketplace) nt [Scalable WordPress](/marketplace/partners/wordpress/scalablewordpress/) rakendus võib sisaldada MySQL-andmebaasiga, salvestusruumi kontod, teenusleping rakenduse ise, veebirakenduse teatiste reegleid, rakenduse sätete, autoscale sätted ja rohkem ja kõik need Mallid on saadaval teile PowerShelli kaudu. Lisateavet selle kohta, kuidas alla laadida ja kasutada neid malle, leiate [Azure'i ressursihaldur Azure PowerShelli kasutamine](../powershell-azure-resource-manager.md).

Azure'i ressursihaldur mallide kohta leiate lisateavet teemast [Azure ressursihaldur mallide koostamine](../resource-group-authoring-templates.md)

### <a name="azure-sdk-26-for-visual-studio"></a>Azure'i SDK 2.6 Visual Studio ###

Uusim SDK sisaldab ressursihaldur malli tugi JSON redaktoris täiustused. Saate selle abil saate kiiresti ressursi rühma malli loomine algusest peale ise luua või avada olemasoleva JSON malli (nt allalaaditud Galerii malli) muutmiseks, asustades parameetrite fail ja isegi ressursirühm otse Azure'i ressursirühm lahenduse juurutamine.

Lisateavet leiate teemast [Azure SDK 2.6 Visual Studio](/blog/2015/04/29/announcing-the-azure-sdk-2-6-for-net/).

### <a name="azure-powershell-080-or-later"></a>Azure'i PowerShelli 0.8.0 või uuem versioon ###

Azure'i PowerShelli installimine alates versioon 0.8.0, sisaldab Azure'i ressursihaldur mooduli Lisaks Azure mooduli. Selle uue mooduli võimaldab skript kasutuselevõtu ressursi rühmad.

Lisateavet leiate teemast [Azure ressursihaldur Azure PowerShelli kasutamine](../powershell-azure-resource-manager.md)

### <a name="azure-resource-explorer"></a>Azure'i Resource Explorer ###

See [tööriist eelvaade](https://resources.azure.com) võimaldab teil uurida ressursi rühmade tellimuse ja üksikute ressursside JSON määratlusi. Tööriista, saate redigeerida ressursi JSON määratlused, kustutada ressursid on kogu hierarhia ja luua uusi ressursse.  Teave käepärast see tööriist on väga kasulik koostamine malli, sest see näitab, milliseid atribuute, peate määrama teatud tüüpi ressursi, õigete väärtuste jne. Saate isegi luua oma ressursirühm [Azure portaali](https://portal.azure.com/)ja seejärel kontrolli selle JSON määratlused Exploreri tööriista aitavad templatize ressursirühma.

### <a name="deploy-to-azure-button"></a>Azure'i nupule juurutamine ###

Kui kasutate andmeallika juhtelemendi GitHub, saate mõne [Deploy Azure nupp](/blog/2014/11/13/deploy-to-azure-button-for-azure-websites-2/) pannakse oma seletusfail. MD, mis võimaldab võtmed juurutamise Kasutajaliidese Azure. Ajal mis tahes lihtsa web app saate teha seda, saate seda lubada juurutamine on kogu ressursirühm paigutades failina azuredeploy.json hoidla root laiendamine. Azure'i nupule Deploy kasutavad selle JSON-faili, mis sisaldab ressursi rühma Mall, ressursirühma loomiseks. Näiteks vt [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) näidis, mille abil saate selles õpetuses.

## <a name="get-the-sample-resource-group-template"></a>Ressursi rühma näidismall hankimine ##

Nüüd vaatame õige selle.

1.  Liikuge [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) rakenduse teenuse valimi.

2.   Klõpsake readme.md, **Deploy Azure**.
 
3.  Teil on tehtud [azure juurutamine](https://deploy.azure.com) saidile ja palutakse juurutamise sisendparameetrid. Pange tähele, et enamik väljad täidetakse hoidla nimi ja mõned juhusliku stringid teie eest. Saate muuta kõiki välju, kui tahate, kuid peate sisestama ainult asjad on SQL serveri haldus kasutajanime ja parooli, siis klõpsake nuppu **edasi**.
 
    ![](./media/app-service-deploy-complex-application-predictably/gettemplate-1-deploybuttonui.png)

4.  Seejärel klõpsake **Deploy** juurutamise alustamiseks. Kui protsess töötab valmimiseni, klõpsake http://todoapp*XXXX*. azurewebsites.net linki Sirvi juurutatud rakendusega. 

    ![](./media/app-service-deploy-complex-application-predictably/gettemplate-2-deployprogress.png)

    UI oleks natuke aeglane esmalt sirvides selle Kuna rakendused on hakanud just, kuid veenda ennast, et see on täielikult toimiv rakendus.

5.  Uuesti sisse Deploy lehel linki **Halda** leiate Azure'i portaalis uue rakenduse.

6.  **Essentialsi** rippmenüüst linki ressursside rühma. Pange tähele, et veebirakenduse on juba ühendatud GitHub hoidla **Välise projekti**all. 

    ![](./media/app-service-deploy-complex-application-predictably/gettemplate-3-portalresourcegroup.png)
 
7.  Ressursi rühma tera, Pange tähele, et juba kaks veebirakenduste ja ühe SQL-andmebaasi ressursirühma.

    ![](./media/app-service-deploy-complex-application-predictably/gettemplate-4-portalresourcegroupclicked.png)
 
Kõik, mis teile kuvatud lühike paar minutit on täiesti juurutatud kahe-microservice rakenduse, kõik komponendid, sõltuvused, sätted, andmebaasi ja pidev avaldamine, häälestada, on automatiseeritud korraldamise Azure'i ressursihaldur. Kõik tehtud kahest asjaolust:

-   Azure'i nupule Deploy
-   azuredeploy.JSON repo root

Saate seda sama rakendust juurutada kümneid, sadu või tuhandeliste korda ja on sama täpne konfiguratsioon iga kord. Korratavus ja muudab seda moodust võimaldab teil lihtne ja turvaline suure ulatusega rakenduste juurutamine.

## <a name="examine-or-edit-azuredeployjson"></a>Uurida (või redigeerida) AZUREDEPLOY. JSON ##

Nüüd vaatame, kuidas häälestati GitHub hoidla. Saate JSON redaktori kasutamine Azure .NET SDK, seega kui te pole seda juba installitud [Azure'i .NET SDK 2.6](/downloads/), tehke seda kohe.

1.  Klooni [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) hoidla oma lemmik git tööriista abil. Pildil, teen see meeskonnatöö Exploreris Visual Studio 2013.

    ![](./media/app-service-deploy-complex-application-predictably/examinejson-1-vsclone.png)

2.  Avage hoidla juures Visual Studios azuredeploy.json. Kui te ei näe paani JSON liigendus, on vaja installida Azure .NET SDK.

    ![](./media/app-service-deploy-complex-application-predictably/examinejson-2-vsjsoneditor.png)

Ma ei kavatse kirjeldada iga detail JSON-vormingus, kuid [Veel ressursse](#resources) jaotises on lingid ressursi rühma malli keele jaoks. Siin lihtsalt ma kuvamiseks, huvitav funktsioone, mis aitavad teil alustada tööd teha rakenduse juurutamiseks oma kohandatud mall.

### <a name="parameters"></a>Parameetrid ###

Heitke pilk parameetrite jaotise kuvamiseks, et enamik järgmiste parameetrite on nupp **Deploy Azure** palub teil sisestada. Saidi taha **Deploy Azure** nupp kuvab UI azuredeploy.json määratletud parameetrite abil sisendi. Neid parameetreid kasutatakse kogu ressursi määratlused, nt ressursside nimed, kinnisvara jne.

### <a name="resources"></a>Ressursid ###

Ressursside sõlme näete 4 ülataseme ressursid määratletud, sh SQL serveri eksemplar, kuvatakse rakenduse teenusleping ja kahe veebirakenduste. 

#### <a name="app-service-plan"></a>Rakenduse teenusleping ####

Alustame selle JSON lihtsa ülataseme ressurssi. JSON liigendus, klõpsake rakenduse teenusleping nimega **[hostingPlanName]** esiletõstmiseks vastav JSON-kood. 

![](./media/app-service-deploy-complex-application-predictably/examinejson-3-appserviceplan.png)

Pange tähele, et selle `type` element määrab stringi rakenduse teenuse lepingule (nimetati serveripargi kaua, kaua aega tagasi), ja muud elemendid ja atribuudid on täidetud parameetrid määratletud JSON-faili abil selle ressursi pole pesastatud ressursse.

>[AZURE.NOTE] Pange tähele, et väärtus `apiVersion` ütleb Azure, millist versiooni REST API JSON ressursside määratlemine koos kasutada, ja võib mõjutada ressursi peaks vormindamise sees on `{}`. 

#### <a name="sql-server"></a>SQL serveri ####

Klõpsake SQL serveri ressursi nimega **SQLServer** JSON liigenduse.

![](./media/app-service-deploy-complex-application-predictably/examinejson-4-sqlserver.png)
 
Võtke arvesse järgmist esiletõstetud JSON koodi kohta.

-   Parameetrite kasutamine tagab loodud ressursid on nimega ja konfigureeritud nii, et need üksteisele vastavad.
-   Ressursi SQLServer on kaks pesastatud ressursid, iga on erinev väärtus `type`.
-   Pesastatud ressursse sees `“resources”: […]`, kus on määratletud andmebaas ja tulemüüri reegleid, on mõne `dependsOn` element, mis määrab ülataseme SQLServer Ressursi ressursi ID-d. See ütleb Azure'i ressursihaldur, "enne, kui loote selle ressursi, mis on muu ressursiga peab juba olemas; ja kui selle muu ressursiga on määratletud malli, siis looge selle esmalt".

    >[AZURE.NOTE] Üksikasjalikku teavet selle kohta, kuidas kasutada funktsiooni `resourceId()` funktsioon leiate [Azure'i ressursihaldur malli funktsioonid](../resource-group-template-functions.md).

-   Mõju on `dependsOn` element on Azure ressursihaldur saate teada, millised ressursid saab luua paralleelselt ja ressursside tuleb luua sirgjoontega. 

#### <a name="web-app"></a>Web app ####

Nüüd vaatame teisaldamiseks tegelik veebirakenduste ise, mis on keerulisem. Klõpsake [variables('apiSiteName')] veebirakenduse JSON liigenduse esiletõstmiseks selle JSON-kood. Märkate, et asjad palju huvitavamaks. Selleks tuleb rääkida funktsioonid ükshaaval:

##### <a name="root-resource"></a>Root ressurss #####

Veebirakenduse sõltub kaks erinevat allikat. See tähendab, et Azure'i ressursihaldur loob veebirakenduse alles pärast seda, kui luuakse nii rakenduse teenusleping ja SQL serveri eksemplar.

![](./media/app-service-deploy-complex-application-predictably/examinejson-5-webapproot.png)

##### <a name="app-settings"></a>Rakenduse sätted #####

Rakenduse sätete on määratletud ka pesastatud ressursi.

![](./media/app-service-deploy-complex-application-predictably/examinejson-6-webappsettings.png)

Klõpsake soovitud `properties` elemendi jaoks `config/appsettings`, teil on kaks rakenduse sätete vormingus `“<name>” : “<value>”`.

-   `PROJECT`on [KUDU säte](https://github.com/projectkudu/kudu/wiki/Customizing-deployments) , mis ütleb Azure juurutamise mis projekti kasutamine mitme projekti Visual Studio lahendus. Näitab teile, kuidas hiljem andmeallika juhtelement on konfigureeritud, kuid kuna ToDoApp kood on mitme projekti Visual Studio lahenduse, tuleb see säte.
-   `clientUrl`on lihtsalt sätte rakenduse koodi rakendus kasutab.

##### <a name="connection-strings"></a>Ühendusstringi #####

Ühenduse stringid on määratletud ka pesastatud ressursi.

![](./media/app-service-deploy-complex-application-predictably/examinejson-7-webappconnstr.png)

Klõpsake soovitud `properties` elemendi jaoks `config/connectionstrings`, iga ühendusstringi on ka määratletud nimi: väärtus paari, kindla vormingu `“<name>” : {“value”: “…”, “type”: “…”}`. Jaoks soovitud `type` element, võimalikud väärtused on `MySql`, `SQLServer`, `SQLAzure`, ja `Custom`.

>[AZURE.TIP] Käivitage järgmine käsk ühenduse stringi tüüpi lõpliku loendi leiate Azure'i PowerShelli: \[Enum]::GetNames("Microsoft.WindowsAzure.Commands.Utilities.Websites.Services.WebEntities.DatabaseType")
    
##### <a name="source-control"></a>Andmeallika juhtelement #####

Andmeallika juhtelement sätted on määratletud ka pesastatud ressursi. Azure'i ressursihaldur kasutab selle ressursi pidev avaldamise konfigureerimiseks (vt hoiatus `IsManualIntegration` hiljem) ja ka võrgukoosolekuga juurutamise rakenduse koodi JSON-faili töötlemise ajal automaatselt.

![](./media/app-service-deploy-complex-application-predictably/examinejson-8-webappsourcecontrol.png)

`RepoUrl`ja `branch` peaksid olema päris selge ja peaks käsk Git hoidla ja avaldamiseks haru nime. Klõpsake uuesti need on määratletud sisendparameetrid. 

Märkus selle `dependsOn` element, mis lisaks web appi ressurss, `sourcecontrols/web` sõltub ka `config/appsettings` ja `config/connectionstrings`. See on, kuna pärast `sourcecontrols/web` on konfigureeritud, Azure juurutamise protsessi proovib automaatselt juurutada, koostamine ja alustage rakenduse koodi. Seetõttu selle sõltuvus lisamise aitab teil veenduge, et rakendus on nõutav rakenduse sätete ja ühendusstringi, enne rakenduse koodi käivitada. 

>[AZURE.NOTE] Pange tähele, et `IsManualIntegration` on seatud `true`. Atribuut on vaja selles õpetuses, kuna te tegelikult oma GitHub hoidla ja seetõttu ei saa tegelikult anda õiguse Azure konfigureerida pidev avaldamise kaudu [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) (st Lükake automaatse hoidla Värskenduste Azure). Kasutage vaikeväärtust, milleks `false` jaoks määratud hoidla ainult juhul, kui olete konfigureerinud omaniku GitHub identimisteabe [Azure portaali](https://portal.azure.com/) enne. Teisisõnu, kui olete loonud andmeallika juhtelemendi GitHub või BitBucket mis tahes rakenduse [Azure portaali](https://portal.azure.com/) varem, kasutaja mandaadiga, siis Azure on meeles pidada identimisteabe ja kasutada neid iga kord, kui tulevikus mis tahes rakendust GitHub või BitBucket juurutamist. Juhul, kui te pole seda veel teinud, juurutamise JSON malli nurjub, kui Azure'i ressursihaldur püüab web appi andmeallika juhtelemendi sätete konfigureerimiseks, kuna see ei saa sisse logida GitHub või BitBucket hoidla omanik mandaati.

## <a name="compare-the-json-template-with-deployed-resource-group"></a>JSON malli abil juurutatud ressursirühm võrdlus ##

Siin saate läbida kõik web appi labad [Azure portaali](https://portal.azure.com/), kuid on mõnda muud tööriista, mis on kasulik, kui nagu pole veel. Avage [Azure'i Resource Explorer](https://resources.azure.com) eelvaade tööriista, mis pakub JSON kujutis ressursi rühmad tellimuste, nagu need on olemas tegelikult Azure taustväärtus. Samuti näete, kuidas ressursside rühma JSON hierarhia Azure vastab hierarhia loomiseks kasutatud malli failis.

Näiteks kui minge [Azure'i ressursi Exploreri](https://resources.azure.com) tööriista ja Exploreris sõlmi laiendada, näen ressursirühma ja nende vastavate ressursside tüübid kogunud ülataseme ressursse.

![](./media/app-service-deploy-complex-application-predictably/ARM-1-treeview.png)

Kui teil minna süvitsi web appi, peaks olema näeksid web appi konfiguratsiooni üksikasjad sarnaselt on allpool pildil:

![](./media/app-service-deploy-complex-application-predictably/ARM-2-jsonview.png)

Uuesti pesastatud ressursside peaks olema väga sarnased JSON mallifail hierarhia ja peaksite nägema rakenduse sätete, ühendusstringi, õigesti kajastuksid paani JSON jne. Siinsed sätted võib see viidata JSON-failiga ja aitavad teil oma JSON mallifail.

## <a name="deploy-the-resource-group-template-yourself"></a>Malli ressursi rühma ise ##

**Deploy Azure** nupp sobib hästi, kuid see võimaldab teil juurutada ressursi rühma malli azuredeploy.json ainult juhul, kui teil on juba lükata azuredeploy.json github. Azure'i .NET SDK pakub tööriistu, mida saate kasutada mis tahes JSON mallifail otse oma kohalikus arvutis. Selleks tehke järgmist:

1.  Visual Studio, klõpsake **faili** > **Uus** > **projekti**.

2.  Klõpsake **Visual C#** > **Cloud** > **Azure ressursirühm**ja klõpsake siis nuppu **OK**.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-1-vsproject.png)

3.  **Valige Azure malli**, valige **Tühi Mall** ja klõpsake nuppu **OK**.

4.  Lohistage azuredeploy.json uue projekti **malli** kausta.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-2-copyjson.png)

5.  Avage Solution Exploreris kopeeritud azuredeploy.json.

6.  Lihtsalt huvides tutvustamise, lisamine teatud standardse rakenduse ülevaate ressursid meie JSON-faili, klõpsates nuppu **Lisa ressursi**. Kui olete just huvitatud juurutamine JSON-faili, jätkake deploy juhiseid.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-3-newresource.png)

7.  Valige **Rakenduse ülevaated veebirakenduste**, siis veenduge, et olemasoleva rakenduse teenuse kavandamine ja web app on valitud, ja seejärel klõpsake nuppu **Lisa**.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-4-newappinsight.png)

    Teile kuvatakse nüüd võimalik näha mitmeid uusi ressursse, mis sõltuvalt ressursi ja mida see tähendab, sõltuvuste rakenduse teenusleping või veebirakenduse. Need ressursid on lubatud oma olemasoleva määratluse ja te ei kavatse seda muuta.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-5-appinsightresources.png)
 
8.  Klõpsake JSON liigendus, **appInsights AutoScale** esiletõstmiseks selle JSON-kood. See on teie rakendus teenusleping skaleerimise sätet.

9.  Esiletõstetud JSON-koodi, otsige üles soovitud `location` ja `enabled` atribuudid ja määrata need, nagu allpool näidatud.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-6-autoscalesettings.png)

10. JSON liigendus, klõpsake selle JSON kood esiletõstmiseks **CPUHigh appInsights** . See on teatise.

11. Otsige üles soovitud `location` ja `isEnabled` atribuudid ja määrata need, nagu allpool näidatud. Tehke sama muud kolme teatised (lilla pirnid).

    ![](./media/app-service-deploy-complex-application-predictably/deploy-7-alerts.png)

12. Nüüd oletegi valmis kasutama. Paremklõpsake projekti ja valige **Deploy** > **Uue juurutamise**.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-8-newdeployment.png)

13. Kui te pole seda juba teinud, logige sisse oma Azure'i kontosse.

14. Ressursi olemasolevasse rühma valige tellimuse või looge uus, valige **azuredeploy.json**ja seejärel nuppu **Redigeeri parameetrid**.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-9-deployconfig.png)

    Nüüd küll kõik parameetrid määratletud kena tabeli malli faili redigeerida. Parameetrite, mis määratlevad vaikesätted juba vaikeväärtused ja parameetrid, mis määratlevad lubatud väärtuste loend kuvatakse nimega rippmenüüst.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-10-parametereditor.png)

15. Täitke tühjad parameetrid ja kasutada [GitHub repo aadress ToDoApp](https://github.com/azure-appservice-samples/ToDoApp.git) **repoUrl**. Klõpsake nuppu **Salvesta**.
 
    ![](./media/app-service-deploy-complex-application-predictably/deploy-11-parametereditorfilled.png)

    >[AZURE.NOTE] Autoscaling on funktsioon **Standard** astme või uuem versioon ja lepingu taseme teatiste on pakutakse **tavaline** astme või uuemas versioonis, peate seatud **Standard** või **Premium** **sku** parameetri selleks, et näha kõiki oma uue rakenduse teadmiste ressursid lihtsustatud üles.
    
16. Klõpsake nuppu **juurutamine**. Kui valisite **paroole salvestada**, salvestatakse parool selle parameetri faili **lihttekstina**. Muul juhul küsitakse andmebaasi parool sisestada käigus juurutamist.

See on õige! Nüüd peate Avage uus teatiste kuvamiseks [Azure portaali](https://portal.azure.com/) ja [Azure ressursi Exploreri](https://resources.azure.com) tööriista ja autoscale sätted lisatakse teie JSON juurutatud rakendus.

Selle jaotise juhiseid oma peamiselt teha järgmist:

1.  Valmis mallifail
2.  Loodud parameetri faili minna mallifail
3.  Juurutatud parameetri faili mallifail

Viimane toiming on lihtne teha PowerShelli cmdlet-käsu abil. Mida Visual Studio ei rakenduse juurutamisel vaatamiseks avage Scripts\Deploy-AzureResourceGroup.ps1. On palju koodi olemas, kuid ma just kõik asjakohased kood juurutamiseks mallifail parameetri faili abil esile tõsta.

![](./media/app-service-deploy-complex-application-predictably/deploy-12-powershellsnippet.png)

Viimase cmdlet `New-AzureResourceGroup`, mis tegelikult sooritab toimingu. Kõik see tuleb teile, tänu instrumentaarium, on üsna lihtne juurutada cloud rakendust ootuspäraselt. Iga kord, kui käivitate cmdlet sama malli sama parameetri failiga, kavatsete sama tulemuse.

## <a name="summary"></a>Kokkuvõte ##

DevOps, korratavuse ja prognoositavuse on mis tahes suure ulatusega rakendus, mis koosneb microservices juurutamise võtmed. Selles õpetuses olete juurutanud kahe-microservice rakenduse Azure ressurss rühmana Azure'i ressursihaldur malli abil. Loodetavasti, see on andnud teile teadmisi, mida vajate selleks, et alustada rakenduse Azure teisendamisel malli ja ettevalmistamine ja juurutada ootuspäraselt. 

## <a name="next-steps"></a>Järgmised sammud ##

Vaadake, kuidas [rakendada dünaamilised meetodite ja pidevalt avaldamine microservices rakenduse lihtsalt](app-service-agile-software-development.md) ja Täpsem meetodite abil nagu [flighting juurutamise](app-service-web-test-in-production-controlled-test-flight.md) hõlpsalt.

<a name="resources"></a>
## <a name="more-resources"></a>Veel ressursse ##

-   [Azure'i ressursihaldur malli keel](../resource-group-authoring-templates.md)
-   [Azure'i ressursihaldur mallide koostamine](../resource-group-authoring-templates.md)
-   [Azure'i ressursihaldur malli funktsioonid](../resource-group-template-functions.md)
-   [Azure'i ressursihaldur malliga rakenduse juurutamine](../resource-group-template-deploy.md)
-   [Azure'i ressursihaldur Azure PowerShelli abil](../powershell-azure-resource-manager.md)
-   [Ressursirühm juurutuste Azure tõrkeotsing](../resource-manager-troubleshoot-deployments-portal.md)




 
