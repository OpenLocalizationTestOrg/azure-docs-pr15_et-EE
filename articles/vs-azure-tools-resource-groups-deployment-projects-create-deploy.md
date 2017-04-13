<properties
   pageTitle="Azure'i ressursi rühma Visual Studio projektid | Microsoft Azure'i"
   description="Azure'i ressursi rühma projekti loomine ja juurutamine ressursside Azure'i Visual Studio abil."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn" />
<tags
   ms.service="azure-resource-manager"
   ms.devlang="multiple"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/20/2016"
   ms.author="tomfitz" />

# <a name="creating-and-deploying-azure-resource-groups-through-visual-studio"></a>Loomine ja juurutamine Azure ressursi rühma kaudu Visual Studio

Visual Studio ja [Azure SDK](https://azure.microsoft.com/downloads/), saate luua juurutamine taristu- ja koodi Azure'i projekt. Näiteks saate määratleda veebi, veebisaidi ja andmebaasi oma rakenduse ja selle taristu koos koodi juurutamine. Või saate määratleda virtuaalse masina, virtuaalse võrgu ja salvestusruumi konto ja selle taristu koos skripti, mis on täide virtuaalse masina juurutamine. **Azure'i ressursirühm** juurutamise projekti võimaldab teil kasutada ühe korratavad kasutusel kõik vajalikud ressursid. Juurutamine ja oma ressursse haldamise kohta leiate lisateavet teemast [Azure ressursihaldur ülevaade](azure-resource-manager/resource-group-overview.md).

Azure'i ressursirühm projektide sisaldavad Azure'i ressursihaldur JSON Mallid, mis määratlevad ressursse, mis juurutamist Azure. Ressursihaldur malli elementide kohta leiate teemast [Azure ressursihaldur loome Mallid](resource-group-authoring-templates.md). Visual Studio abil saab muuta neid malle ja annab teie käsutusse tööriistad, mida mallide töötamise lihtsustamiseks.

Selles teemas juurutamist web app ja SQL-andmebaasi. Juhised kehtivad peaaegu sama mis tahes tüüpi ressursi. Saate nimega hõlpsalt juurutada virtuaalse masina ja selle seotud ressursid. Visual Studio pakub palju erinevaid starter malle juurutamine levinud stsenaariumi.

Selles artiklis kirjeldatakse Visual Studio 2015 Update 2 ja Microsoft Azure'i SDK 2.9 .net-i jaoks. Kui Visual Studio 2013 kasutamisel Azure'i SDK 2.9 kasutusvõimalusi on suures osas sama. Saate kasutada versioone Azure'i SDK 2,6 või uuemat versiooni; kasutajaliidese kasutusvõimalusi võib olla erinevas kasutajaliidese see artikkel. Soovitame tungivalt enne alustamist juhiseid installida [Azure SDK](https://azure.microsoft.com/downloads/) uusim versioon. 

## <a name="create-azure-resource-group-project"></a>Projekti loomine Azure ressursirühm

Selle protseduuri käigus pakutakse loote mõnda Azure'i ressursirühm projekti **Web Appi + SQL-i** malli.

1. Visual Studio, valige **fail**, **Uue projekti**, valige **C#** või **Visual Basic**. Valige **pilve**ja seejärel valige **Azure ressursirühm** projekti.

    ![Pilveteenuse juurutamise projekti](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-project.png)

1. Valige mall, millele soovite juurutada Azure ressursihaldur. Teade põhinevad palju erinevaid võimalusi tüüpi projekti soovite juurutamine. Selles teemas **veebirakenduse + SQL-i** malli valimine.

    ![Malli valimine](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-project.png)

    Saate valida malli on lihtsalt alguspunkti; Saate lisada ja eemaldada ressursse, mis vastavad teie stsenaariumi.

    >[AZURE.NOTE] Visual Studio toob saadaolevad Mallid veebisaidil loendit. Loendi võivad muutuda.

    Visual Studio loob ressursi rühma juurutamise project web appi ja SQL-andmebaasi.

1. Et näha, mida olete loonud, sõlmed juurutamise projekti laiendamine

    ![Kuva sõlmed](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-items.png)

    Kuna valisime Web Appi + SQL-i malli selle näite puhul, kuvatakse järgmised failid. 

  	|Faili nimi|Kirjeldus|
  	|---|---|
  	|AzureResourceGroup.ps1 juurutamine|PowerShelli skripti, mis käivitab Azure'i ressursihaldur juurutamiseks PowerShelli käske.<br />**Märkus** Visual Studio kasutab seda PowerShelli skripti malli juurutamine. See skript muudatused mõjutavad juurutamine Visual Studios, et olla ettevaatlik.|
  	|WebSiteSQLDatabase.json|Ressursihaldur Mall, mis määratleb infrastruktuuri soovite võtta kasutusele Azure ja juurutamise käigus saate sisestada parameetrid. Samuti määratletakse sõltuvuste ressursid, et ressursihaldur kasutab vahendid õiges järjestuses.|
  	|WebSiteSQLDatabase.parameters.json|Parameetrite fail, mis sisaldab väärtusi, mis on nõutud mall. Saate edastada parameetrite väärtused iga juurutamise kohandada.|

    Kõigi ressursside rühmaprojekte juurutuse sisaldavad neid põhilisi faile. Muud projektide võib sisaldada täiendavaid faile toetama muud funktsioonid.

## <a name="customize-the-resource-manager-template"></a>Ressursihaldur malli saab kohandada

Saate kohandada juurutamise projekti muutes JSON Mallid, mis kirjeldavad ressursid, mida soovite kasutada. JSON tähistab JavaScripti Objektiesituse ja on sarjadesse jaotatud andmete vorming, mida on lihtne töötada. JSON-faile kasutada skeem, mis viitavad saate iga faili ülaosas. Kui soovite mõista skeemi, saate alla laadida ja analüüsida. Skeem määratleb, milliseid elemendid on lubatud, tüübid ja väljade vormingud, võimalikud väärtused loendatud väärtuste ja jne. Ressursihaldur malli elementide kohta leiate teemast [Azure ressursihaldur loome Mallid](resource-group-authoring-templates.md).

Klõpsake malli avamine **WebSiteSQLDatabase.json**

Visual Studio redaktori annab teie käsutusse tööriistad teid aidata ressursihaldur malli redigeerimine. **JSON liigendus** akna hõlbustab määratletud malli kuvamiseks.

![Kuva JSON liigendus](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-json-outline.png)

Mis tahes elemente liigenduse suunab teid osa Mall ja tõstetakse esile vastavate JSON.

![Liikuge JSON](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/navigate-json.png)

Saate lisada ressursi, JSON liigendus akna ülaosas nuppu **Ressursi lisamine** valides või paremklõpsates **ressursid** ja valides **Lisada uue ressursi**.

![ressursi lisamine](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-resource.png)

Selles õpetuses valige **Salvestusruumi konto** ja sellele nime panna. Sisestage nimi on rohkem kui 11 märki, mis sisaldab ainult numbreid ja tähti väiketähed.

![salvestusruumi lisamine](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-storage.png)

Teade, et mitte ainult oli lisatud ressurss, vaid ka parameetri konto tüübi talletusruumi ja muutuja jaoks salvestusruumi konto nimi.

![Kuva kontuur](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-new-items.png)

**StorageType** parameeter on eelmääratletud lubatud tüübid ja vaikimisi tüüp. Saate neid redigeerida, milline olukord teie või jätke need väärtused. Kui te ei soovi, et kõik juurutamine **Premium_LRS** salvestusruumi konto kaudu selle malli, lubatud tüüpi selle eemaldamine. 

    "storageType": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_ZRS",
        "Standard_GRS",
        "Standard_RAGRS"
      ]
    }

Visual Studios annab IntelliSense'i, et aidata teil mõista, millised atribuudid on saadaval, kui malli redigeerimine. Näiteks oma rakenduse teenusleping atribuutide redigeerimiseks liikuge **HostingPlan** ressurss ja lisage väärtus **Atribuudid**. Pange tähele selle IntelliSense'i kuvatakse saadaval väärtused ja kirjeldatakse selle väärtuse.

![Kuva IntelliSense'i](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-intellisense.png)

Saate seada **numberOfWorkers** 1.

    "properties": {
      "name": "[parameters('hostingPlanName')]",
      "numberOfWorkers": 1
    }

## <a name="deploy-the-resource-group-project-to-azure"></a>Azure'i ressursirühm projekti juurutamine

Nüüd olete valmis oma projekti juurutamine. Azure'i ressursi rühmaprojekti juurutamisel juurutamist Azure ressursside rühma. Ressursirühma on loogiline rühmitamine ressursse, mis levinud elutsükli ühiskasutusse anda.

1. Valige juurutamise projekti sõlm kiirmenüü **Deploy** > **Uue juurutamise**.

    ![Juurutada, uue juurutamise menüükäsk](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/deploy.png)

    Kuvatakse dialoogiboks **Deploy ressursirühma** .

    ![Ressursirühm dialoogiboksis juurutamine](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployment.png)

1. **Ressursirühm** ripploend väljal valige ressursi olemasolevasse rühma või luua uue. Ressursi rühma loomiseks avada boks **Ressursirühm** rippmenüü ja valige **Loo uus**.

    ![Ressursirühm dialoogiboksis juurutamine](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-new-group.png)

    Kuvatakse dialoogiboks **Loo ressursirühma** . Pange oma rühma nimi ja asukoht ja klõpsake nuppu **Loo** .

    ![Ressursirühm dialoogiboks loomine](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-resource-group.png)
   
1. **Parameetrite redigeerimiseks** klõpsake nuppu Redigeeri juurutamise parameetrid.

    ![Parameetrite redigeerimisnupp](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/edit-parameters.png)

1. Sisestage tühja parameetrite väärtused ja klõpsake nuppu **Salvesta** . Tühja parameetrid on **hostingPlanName**, **administratorLogin**, **administratorLoginPassword**ja **databaseName**.

    **hostingPlanName** saate määrata [rakenduse teenusleping](./app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) loomiseks nimi. 
    
    **administratorLogin** määrab kasutaja jaoks SQL serveri administraator. Ärge kasutage levinud administraator nimed nagu **sa** või **administraator**. 
    
    **AdministratorLoginPassword** määrab SQL serveri administraatori parooli. **Paroolide lihttekstina parameetrid faili salvestamine** suvand pole turvaline; Seetõttu, valige see suvand. Kuna parooli ei salvestata lihttekstina, peate sisestama parooli uuesti käigus juurutamist. 
    
    **databaseName** määrab luua andmebaasi nimi. 

    ![Klõpsake dialoogiboksi parameetrid redigeerimine](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/provide-parameters.png)
    
1. Azure'i projekti juurutada nuppu **Deploy** . PowerShelli konsooli avatakse väljaspool Visual Studio eksemplari. Sisestage SQL serveri administraatori parooli küsimise PowerShelli konsooli. **Teie PowerShelli konsooli taha muudele üksustele peita või tegumiribal minimeeritud.** Vaadake, kas see konsool ja valige see esitada parool.

    >[AZURE.NOTE] Visual Studio palutakse teil installida Azure PowerShelli cmdlet-käsud. Teil on vaja Azure PowerShelli cmdlet-käsud on edukalt kasutada ressursside rühmad. Kui palutakse, installige need.
    
1. Juurutamise võib kuluda mõni minut. **Väljundi** Windowsis, näete juurutamise olekut. Kui juurutamise on lõpule jõudnud, näitab viimase sõnumi eduka juurutamise koos midagi sarnast:

        ... 
        18:00:58 - Successfully deployed template 'c:\users\user\documents\visual studio 2015\projects\azureresourcegroup1\azureresourcegroup1\templates\websitesqldatabase.json' to resource group 'DemoSiteGroup'.


1. Brauseris, avage [Azure portaali](https://portal.azure.com/) ja logige oma kontosse sisse. Ressursirühma vaatamiseks valige **Ressursi rühmad** ja te juurutatud ressursirühma.

    ![Valige rühm](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-group.png)

1. Kuvatakse kõik juurutatud ressursid. Pange tähele, et salvestusruumi konto nimi ei ole just teie määratud lisamisel selle ressursi. Salvestusruumi konto peab olema kordumatu. Malli lisab automaatselt märgistring, on nimi, mille saate sellele kordumatu nimi. 

    ![ressursside kuvamine](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployed-resources.png)

1. Kui soovite muuta ümberkorraldamine projekti, valige olemasolev ressursirühm Azure ressursi rühmaprojekti kiirmenüü. Kiirmenüü, valige **Deploy**ja valige saate juurutada ressursirühma.

    ![Azure'i ressursirühm juurutatud](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/redeploy.png)

## <a name="deploy-code-with-your-infrastructure"></a>Kood oma taristu juurutamine

Selles etapis infrastruktuuri juurutanud oma rakenduse, kuid ei ole juurutatud projekti tegelik koodi. Selles teemas kirjeldatakse web appi ja SQL-andmebaasitabelid juurutamise käigus juurutamist. Kui juurutate virtuaalse masina asemel web appi, mida soovite teatud koodi käivitada arvutis käigus juurutamist. Juurutamine kood web appi või virtuaalse masina häälestamise protsess on peaaegu sama.

1. Projekti lisada oma Visual Studio lahendus. Lahendus paremklõpsake ja valige käsk **Lisa** > **Uue projekti**.

    ![Saate lisada projekti](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-project.png)

1. **ASP.net-i veebirakenduse**lisada. 

    ![veebirakenduse lisamine](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-app.png)
    
1. Valige **MVC** ja tühjendage väli **Host pilves** , kuna ressursi rühmaprojekti teeb selle ülesande.

    ![Valige MVC](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-mvc.png)
    
1. Pärast Visual Studio loob oma veebirakenduse, näete nii projektide lahendus.

    ![projektide kuvamiseks](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-projects.png)

1. Nüüd, peate veenduge, et ressursi rühma projekti on uue projekti teadlikud. Minge tagasi ressursside rühmaprojekti (AzureResourceGroup1). Paremklõpsake **Viited** ja valige **Lisada viide**.

    ![viite lisamine](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-new-reference.png)

1. Valige loodud web appi projekt.

    ![viite lisamine](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-reference.png)
    
    Viite lisamisega link web appi project ressursside rühmaprojekti ja automaatselt kolm klahv atribuutide seadmine. Näete atribuutidest viiteks aken **Atribuudid** .

      ![teemast juhend](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/see-reference.png)
    
    Atribuudid on:

    - **Täiendavad atribuudid** sisaldab web juurutamise pakett lavastus asukohta, mis on Azure mäluruumi lükata. Pange tähele, et kausta (ExampleApp) ja faili (package.zip). Saate annab need väärtused parameetrid, kui rakenduse juurutamine. 
    - **Kaasa faili tee** sisaldab tee, kus luuakse paketi. **Kaasa sihtkohtade** sisaldab juurutamise aktiveeritakse käsk. 
    - Vaikeväärtust, milleks on **koostamine; Paketi** võimaldab juurutamise koostamine ja luua web juurutamise pakett (package.zip).  
    
    Teil pole vaja avalda profiili nagu juurutamise saab atribuutide paketi loomiseks vajalikku teavet.
      
1. Ressursi lisamine mall.

    ![ressursi lisamine](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-resource-2.png)

1. Seekord märkige **Web Web Apps juurutamine**. 

    ![Lisage web juurutamine](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-web-deploy.png)
    
1. Juurutage uuesti oma ressursi rühma projekti ressursirühma. See aeg on mõned uue parameetrid. Kuna Visual Studio põhjal automaatselt genereerida neid väärtusi sisestama väärtused **_artifactsLocation** või **_artifactsLocationSasToken** pole vaja. Aga teil on määratud kausta ja faili nimi tee, mis sisaldab juurutamise pakett (näidatakse **ExampleAppPackageFolder** ja **ExampleAppPackageFileName** järgmisel pildil). Sisestage väärtused, mis teile kuvati eelnevalt kirjeldatud viide atribuudid (**ExampleApp** ja **package.zip**).

    ![Lisage web juurutamine](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/set-new-parameters.png)
    
    **Artefakt salvestusruumi konto**, valige selle ressursi rühmaga juurutatud.
    
1. Kui juurutamise on lõpule jõudnud, valige oma veebirakenduse portaalis. Valige sirvimiseks saidi URL.

    ![saidi vaatamine](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/browse-site.png)

1. Pöörake tähelepanu sellele, et olete edukalt juurutanud Vaikimisi ASP.net-i rakenduse.

    ![Kuva juurutatud rakendus](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployed-app.png)

## <a name="next-steps"></a>Järgmised sammud

- Oma ressursse portaali kaudu haldamise kohta leiate teemast [Azure portaalis hallata oma Azure ressursse](./azure-portal/resource-group-portal.md).
- Mallide kohta leiate lisateavet teemast [Azure ressursihaldur loome Mallid](resource-group-authoring-templates.md).
