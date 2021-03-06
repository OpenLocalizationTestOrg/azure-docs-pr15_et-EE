<properties
    pageTitle="Juurutamine virtuaalse masina skaala seadmine Visual Studio abil | Microsoft Azure'i"
    description="Juurutamine virtuaalse masina skaala komplektid Visual Studio ja ressursihaldur malli abil"
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gbowerman"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/13/2016"
    ms.author="guybo"/>

# <a name="deploy-virtual-machine-scale-set-using-visual-studio"></a>Juurutamine virtuaalse masina skaala seadmine Visual Studio abil

Selles artiklis kirjeldatakse juurutamise on Azure virtuaalse masina skaala seadmine Visual Studio ressursi rühma juurutamise abil.


[Azure virtuaalse masina skaala](https://azure.microsoft.com/blog/azure-vm-scale-sets-public-preview/) on Azure arvutada ressursi juurutada ja hallata kogumi sarnaseid virtuaalmasinates hõlpsalt integreeritud suvandeid mastaabi automaatselt ja laadimine tasakaalustamiseks. Saate ette valmistada ja juurutada VM skaala komplektid [Azure'i ressursi Manager (ARM) mallide](https://github.com/Azure/azure-quickstart-templates)kasutamine. ARM malle saab kasutada Azure CLI PowerShelli, REST kaudu kui ka otse Visual Studio. Visual Studios annab näide malle, mida saab kasutada projekti Azure'i ressursi rühma juurutamise käigus.

Azure'i ressursirühm juurutuste on viis rühmitada ja avaldada Azure seotud ressursid kogumi ühe juurutamise käigus. Saate lisateavet nende kohta siin: [loomine ja juurutamine Azure ressursi rühma Visual Studio kaudu](../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).

## <a name="pre-requisites"></a>Eeltingimused

Juurutamine VM skaala komplektid Visual Studio alustamiseks vajate järgmist:

- Visual Studio 2013 või 2015
- Azure'i SDK 2.7, 2,8 või 2.9

Märkus: Järgmised juhised eeldavad, et kasutate Visual Studio 2015 [Azure'i SDK 2,8](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/).

## <a name="creating-a-project"></a>Projekti loomine

1. Visual Studio 2015 uue projekti loomise, valides **faili | Uus | Projekti**.

    ![Uus fail][file_new]

2. Klõpsake jaotises **Visual C# | Pilveteenuse**, valige **Azure ressursihaldur** on ARM malli kasutamise kohta projekti loomise kohta.

    ![Projekti loomine][create_project]

3.  Valige loendis malle, Linux või Windowsi virtuaalse masina skaala seadmine malli.

    ![Valige mall][select_Template]

4. Pärast projekti loomist kuvatakse PowerShelli juurutamise skriptide, mõni Azure ressursihaldur Mall ja parameetri faili jaoks virtuaalse masina skaala määramine.

    ![Solution Exploreris][solution_explorer]

## <a name="customize-your-project"></a>Projekti kohandamine

Nüüd saate redigeerida malli seda kohandada rakenduse tarbeks, nt VM laiend atribuutide lisamine või redigeerimine laadi tasakaalustamiseks reeglid. Vaikimisi on konfigureeritud VM skaala seadmine Mallid juurutamine AzureDiagnostics laiend, mis lihtsustab autoscale reeglite lisamine. See avaneb ka laadi koormusetasakaalustusteenuse avaliku IP-aadress, konfigureeritud sissetulevad reeglid NAT, mis võimaldab ühendamist VM juhtudel SSH (Linux) või RDP (Windows) – esiosa port vahemikus algab 50000, mis tähendab, et Linux, juhul kui teie SSH pordi 50000 avaliku IP-aadress (või domeeni nimi) saate suunatakse port 22 skaala Set esimese VM. Ühenduse loomine pordi 50001 suunatakse teise VM port 22 jne.

 Hea võimalus oma Visual Studio mallide redigeerimine on JSON liigenduse abil korraldada parameetrid, muutujate ja ressursse. Koos skeemi mõistmiseks saate Visual Studio meelde tõrgete lahendamine malli enne juurutamist.

![JSON Explorer][json_explorer]

## <a name="deploy-the-project"></a>Projekti juurutamine

6. Malli ARM Azure VM skaala seadmine ressursside loomiseks. Paremklõpsake valikut projekti sõlm ja valige **Deploy | Uue juurutamise**.

    ![Malli juurutamine][5deploy_Template]

7. Valige oma tellimuse dialoogis "Juurutamine ressursi rühma".

    ![Malli juurutamine][6deploy_Template]

8. Siit saate luua ka Azure uue ressursirühma juurutada, et teie mall.

    ![Uue ressursirühma][new_resource]

9. Edasi klõpsake **Parameetrite redigeerimine** nuppu parameetrid, kus antakse edasi malli teatud väärtused, nt kasutajanimi sisestama ja OS parool on nõutav juurutamise loomiseks. Kui teil pole installitud Visual Studio PowerShelli tööriistad, on soovitatav kontrollida "Salvesta paroolid" vältimiseks peidetud PowerShelli käsurea viip, või kasutada [keyvault toetavad](https://azure.microsoft.com/blog/keyvault-support-for-arm-templates/).

    ![Parameetrite redigeerimine][edit_parameters]

10. Nüüd klõpsake **Deploy**. Juurutamise pooleli jäänud **väljundi** aknas. Pange tähele, et selle toimingu teostab **Deploy-AzureResourceGroup.ps1** skripti.

    ![Väljundi aknas][output_window]

## <a name="exploring-your-vm-scale-set"></a>Uurimine VM skaala määramine

Kui juurutamise lõpule jõudnud, saate vaadata uue VM skaala seadmine Visual Studio **Cloud Explorer** (loendi värskendamine). Pilveteenuse Exploreri saate hallata Azure ressursse Visual Studio rakenduste arendamise ajal. Saate ka vaadata VM skaala määramine [Azure portaali](https://portal.azure.com) ja [Azure ressursi Exploreris](https://resources.azure.com/).

![Cloud Explorer][cloud_explorer]

 Portaalis on parim viis visuaalselt haldamine oma veebibrauseriga, Azure'i infrastruktuuri ajal Azure'i ressursi Exploreri abil on lihtne Explorer ja silumine Azure ressursse, andes aknas "eksemplari vaatesse" ja nähtaval ka vaatate ressursside PowerShelli käske. Kuigi VM skaala komplektid on eelvaade, ressursside Explorer kuvab enamik üksikasjad VM skaala komplektide.

## <a name="next-steps"></a>Järgmised sammud

Kui edukalt juurutamist VM skaala komplektid Visual Studio kaudu saate kohandada vastavalt oma rakenduse nõudmistele projekti. Näiteks häälestamise autoscale lisamine on ülevaateid ressurss, taristu lisamine oma mall, nt autonoomse VMs või rakendustes, mis kasutavad kohandatud skript laiend juurutamine. Hea allikas näide Mallid leiate [Azure'i Kiirjuhend Mallid](https://github.com/Azure/azure-quickstart-templates) GitHub hoidla (Otsi "vmss").

[file_new]: ./media/virtual-machine-scale-sets-vs-create/1-FileNew.png
[create_project]: ./media/virtual-machine-scale-sets-vs-create/2-CreateProject.png
[select_Template]: ./media/virtual-machine-scale-sets-vs-create/3b-SelectTemplateLin.png
[solution_explorer]: ./media/virtual-machine-scale-sets-vs-create/4-SolutionExplorer.png
[json_explorer]: ./media/virtual-machine-scale-sets-vs-create/10-JsonExplorer.png
[5deploy_Template]: ./media/virtual-machine-scale-sets-vs-create/5-DeployTemplate.png
[6deploy_Template]: ./media/virtual-machine-scale-sets-vs-create/6-DeployTemplate.png
[new_resource]: ./media/virtual-machine-scale-sets-vs-create/7-NewResourceGroup.png
[edit_parameters]: ./media/virtual-machine-scale-sets-vs-create/8-EditParameter.png
[output_window]: ./media/virtual-machine-scale-sets-vs-create/9-Output.png
[cloud_explorer]: ./media/virtual-machine-scale-sets-vs-create/12-CloudExplorer.png
