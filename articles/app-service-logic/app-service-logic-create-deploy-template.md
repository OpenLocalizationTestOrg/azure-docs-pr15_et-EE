<properties
   pageTitle="Loogika rakenduse juurutamine malli loomine | Microsoft Azure'i"
   description="Saate teada, kuidas loogika rakenduse juurutamine malli loomine ja kasutada seda Redaktsioonide"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/18/2016"
   ms.author="jehollan"/>

# <a name="create-a-logic-app-deployment-template"></a>Loogika rakenduse juurutamine malli loomine

Pärast loogika rakendus on loodud, võite luua ka Azure ressursihaldur mallina. Sellisel viisil saate hõlpsalt juurutada loogika rakenduse keskkonnas või ressursirühm, kus te võib-olla. Ressursihaldur mallide Sissejuhatus kindlasti artiklid [Azure'i ressursihaldur mallide koostamine](../resource-group-authoring-templates.md) ja [juurutamine ressursid Azure'i ressursihaldur mallide abil](../resource-group-template-deploy.md)kontrollida.

## <a name="logic-app-deployment-template"></a>Loogika juurutamise rakendusemall

Loogika rakendus on kolmest põhikomponendist.

* **Loogika rakenduse ressursi**. Selle ressursi sisaldab teavet hinnad leping, asukoht ja selle töövoo, näiteks.
* **Töövoo**. See on, mis on näha koodi vaade. See sisaldab juhiseid vool ja kuidas peab mootori käivitada määratlus. See on selle `definition` atribuudi loogika rakenduse ressursi.
* **Ühendused**. Need on eraldi ressursid, millest turvaliselt säilitada konnektor ühendused, nt ühendusstringi ja juurdepääsu sümboolse metaandmeid. Viidatava need loogika rakendust selle `parameters` jaotis loogika rakenduse ressursi.

Saate vaadata kõiki olemasolevaid loogika rakenduste neist tööriista nagu [Azure'i ressursi Exploreri](http://resources.azure.com)abil.

Loogika rakenduse kasutamiseks ressursi rühma juurutuste malli, peate ressursside määratlemine ja parameterize vastavalt vajadusele. Näiteks kui juurutamist arengu, testimine ja tootmiskeskkonda, tõenäoliselt peaksite kasutama erinevaid ühendusstringi SQL-andmebaasiga iga keskkonnas. Või, võiksite juurutada erinevate tellimuste või ressursside rühmad.  

## <a name="create-a-logic-app-deployment-template"></a>Loogika rakenduse juurutamine malli loomine

Lihtsaim viis on kehtiv loogika rakenduse juurutamine malli on kasutada [Visual Studio Tools for loogika rakendused](./app-service-logic-deploy-from-vs.md).  Visual Studio tööriistad luua kehtiv juurutamise Mall, mis saab kasutada mis tahes tellimuse või asukoha vahel.

Mõne muu tööriistad aitavad teil nagu loogika rakenduse juurutamine malli loomine. Te saate Autor käsitsi, st kasutamine juba arutada siin parameetrite loomiseks vastavalt vajadusele. Teine võimalus on [loogika rakenduse malli looja](https://github.com/jeffhollan/LogicAppTemplateCreator) PowerShelli mooduli kasutamise kohta. Avatud lähtekoodi moodul hindab esmalt loogika rakendus ja kõik ühendused, et see on kasutusel ning loob malli ressursid juurutamiseks vajalikud parameetritega. Näiteks kui teil on loogika rakendus, mis võtab vastu ka Azure'i teenus siini järjekorda sõnumi ja lisab andmete Azure'i SQL-andmebaasiga, tööriista säilitada kõik korraldamise loogika ja SQL-i ja teenuse siini ühendusstringi parameterize nii, et need saab panna juurutamise veebisaidil.

>[AZURE.NOTE] Ühendused peavad olema sama ressursirühm loogika rakendusena.

### <a name="install-the-logic-app-template-powershell-module"></a>Loogika rakenduse malli PowerShelli mooduli installimiseks

Lihtsaim viis mooduli installimiseks on [Galerii PowerShelli](https://www.powershellgallery.com/packages/LogicAppTemplate/0.1)kaudu käsuga `Install-Module -Name LogicAppTemplate`.  

Samuti saate installida PowerShelli mooduli käsitsi.

1. Laadige alla [loogika rakenduse malli looja](https://github.com/jeffhollan/LogicAppTemplateCreator/releases)uusimat versiooni.  
1. Väljavõte PowerShelli mooduli kaustas kausta (tavaliselt `%UserProfile%\Documents\WindowsPowerShell\Modules`).

Töötada mis tahes rentniku ja tellimuse Accessi mooduli Turbeloa, soovitame seda kasutada koos [ARMClient](https://github.com/projectkudu/ARMClient) käsurea tööriista.  Selle [ajaveebipostitus](http://blog.davidebbo.com/2015/01/azure-resource-manager-client.html) käsitletakse ARMClient üksikasjalikumalt.

### <a name="generate-a-logic-app-template-by-using-powershell"></a>Loogika rakenduse malli luua PowerShelli abil

Pärast PowerShelli on installitud, saate luua malli abil järgmine käsk:

`armclient token $SubscriptionId | Get-LogicAppTemplate -LogicApp MyApp -ResourceGroup MyRG -SubscriptionId $SubscriptionId -Verbose | Out-File C:\template.json`

`$SubscriptionId`on Azure tellimuse ID-ga. Selle rea esimene saab Accessi Turbeloa kaudu ARMClient, siis torude kaudu PowerShelli skripti ja seejärel loob malli JSON-failis.

## <a name="add-parameters-to-a-logic-app-template"></a>Loogika rakenduse malli parameetrite lisamine

Kui olete loonud malli loogika rakenduse, saate jätkata lisamiseks või muutmiseks peate võib-olla parameetrid. Näiteks kui definitsiooni sisaldab ressursi ID Azure funktsioon või pesastatud töövoogu, mille soovite ühele juurutuse juurutada, saate lisada veel ressursse malli ja parameterize ID vastavalt vajadusele. Sama kehtib viiteid kohandatud API-d või ärplema eeldate juurutamiseks iga ressursirühma lõpp-punktid.

## <a name="deploy-a-logic-app-template"></a>Loogika rakenduse malli juurutamine

Suvalist arvu tööriistad, sh PowerShelli, REST API-ga, Visual Studio Redaktsioonide ja Azure portaali malli juurutamise abil saate juurutada malli. Lugege artiklit kohta lisateabe saamiseks [Azure'i ressursihaldur mallide abil ressursse rakendades](../resource-group-template-deploy.md) . Soovitame luua [parameetri faili](../resource-group-template-deploy.md#parameter-file) , et salvestada parameetri väärtused.

### <a name="authorize-oauth-connections"></a>Lubada OAuthi ühendused

Pärast juurutuse loogika rakendus töötab end-to-end kehtiv parameetritega. Siiski OAuthi ühendused endiselt tuleb lubada juurdepääsu luba loomiseks. Saate seda teha, avades loogika rakenduse designer ja seejärel lubada ühendused. Või kui soovite automatiseerida, saate lubada iga OAuthi ühenduse skripti. On mõni näide skripti github [LogicAppConnectionAuth](https://github.com/logicappsio/LogicAppConnectionAuth) projekti all.

## <a name="visual-studio-release-management"></a>Redaktsioonide Visual Studio

Levinud stsenaariumi juurutamine ja haldamise keskkond on kasutada tööriista nagu Visual Studio Redaktsioonide loogika rakenduse juurutamine malli abil. Visual Studio Team Services sisaldab [Juurutada Azure ressursirühm](https://github.com/Microsoft/vsts-tasks/tree/master/Tasks/DeployAzureResourceGroup) ülesande, et saate lisada mis tahes koostamine või vabastage kohaletoimetamisel. Peab teil olema [teenuse põhisumma](https://blogs.msdn.microsoft.com/visualstudioalm/2015/10/04/automating-azure-resource-group-deployment-using-a-service-principal-in-visual-studio-online-buildrelease-management/) juurutamiseks autoriseerimine ja seejärel saate luua väljaanne määratlus.

1. Redaktsioonide, uue määratluse loomiseks valige **tühi** alustamiseks mõnda tühja määratlus.

    ![Looge uus, tühi määratlus][1]   

1. Valige mis tahes ressursid, peate selle. See on tõenäoliselt loodud käsitsi või Koosta käigus loogika rakenduse malli.
1. Lisage toimingu **Azure ressursi rühma juurutamine** .
1. [Teenuse põhisumma](https://blogs.msdn.microsoft.com/visualstudioalm/2015/10/04/automating-azure-resource-group-deployment-using-a-service-principal-in-visual-studio-online-buildrelease-management/)konfigureerimine ja viidata Mall ja Mall parameetrite failid.
1. Jätkake rajamise keskkonnas, automatiseeritud testi või kinnitajad vastavalt vajadusele väljaanne protsessi etappide.

<!-- Image References -->
[1]: ./media/app-service-logic-create-deploy-template/emptyReleaseDefinition.PNG
