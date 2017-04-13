<properties
    pageTitle="Azure'i DevTest Labs KKK | Microsoft Azure'i"
    description="Azure'i DevTest Labs levinud küsimustele vastuste otsimine"
    services="devtest-lab,virtual-machines"
    documentationCenter="na"
    authors="tomarcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="devtest-lab"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="tarcher"/>

# <a name="azure-devtest-labs-faq"></a>Azure'i DevTest Labs KKK

Sellest artiklist leiate Azure'i DevTest Labs kõige levinud küsimustele vastused.

[AZURE.INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="general"></a>Üldine
- [Mida teha, kui mu küsimusele pole siin vastata?](#what-if-my-question-isnt-answered-here)
- [Miks kasutada Azure DevTest Labs?](#why-should-i-use-azure-devtest-labs) 
- [Mida tähendab "hõlpsalt, Iseteeninduslik"?](#what-does-quotworry-free-self-servicequot-mean)
- [Kuidas kasutada Azure DevTest Labs?](#how-can-i-use-azure-devtest-labs) 
- [Kuidas maksmine Azure'i DevTest Labs jaoks?](#how-am-i-billed-for-azure-devtest-labs) 
 
## <a name="security"></a>Turvalisus 
- [Mis on Azure DevTest Labsissa eri turvalisuse tasemete?](#what-are-the-different-security-levels-in-azure-devtest-labs) 
- [Kuidas luua roll, et kasutajad saaksid teatud toimingu?](#how-do-i-create-a-role-to-allow-users-to-perform-a-specific-task) 
 
## <a name="cicd-integration--automation"></a>CI/CD integreerimine ja automatiseerimine 
 
- [Kas Azure'i DevTest Labs integreerida minu CI/CD asukohti?](#does-azure-devtest-labs-integrate-with-my-cicd-toolchain) 
 
## <a name="virtual-machines"></a>Virtuaalmasinates 
 
- [Miks ei kuvata teatud Azure'i Virtuaalmasinates tera, mis näen sees Azure'i DevTest Labs VMs?](#why-cant-i-see-certain-vms-in-the-azure-virtual-machines-blade-that-i-see-within-azure-devtest-labs) 
- [Mis on kohandatud pilte ja valemite vaheline erinevus?](#what-is-the-difference-between-custom-images-and-formulas) 
- [Kuidas luua mitme VMs sama malli korraga?](#how-do-i-create-multiple-vms-from-the-same-template-at-once) 
- [Kuidas viimine minu Azure'i DevTest Labs lab minu olemasoleva Azure VMs?](#how-do-i-move-my-existing-azure-vms-into-my-azure-devtest-labs-lab) 
- [Saate lisada mitu ketast minu VMs?](#can-i-attach-multiple-disks-to-my-vms) 
- [Kuidas luua kohandatud pilte VHD failide üleslaadimise protsessi automaatseks muuta?](#how-do-i-automate-the-process-of-uploading-vhd-files-to-create-custom-images) 
- [Kuidas saab kustutamisel kõik VMs minu lab automaatseks muuta?](#how-can-i-automate-the-process-of-deleting-all-the-vms-in-my-lab)
 
## <a name="artifacts"></a>Esemeid 
 
- [Mis on esemeid?](#what-are-artifacts) 
 
## <a name="lab-configuration"></a>Lab konfigureerimine 
 
- [Kuidas koostada lab mõni Azure ressursihaldur Mall?](#how-do-i-create-a-lab-from-an-azure-resource-manager-template) 
- [Miks mu VMs erinevate ressursside rühmade suvalise nimedega loodud? Saate ümber nimetada või muuta nende ressursside rühmad?](#why-are-my-vms-created-in-different-resource-groups-with-arbitrary-names-can-i-rename-or-modify-these-resource-groups) 
- [Kui palju labs saate luua ühe tellimuse jaotises?](#how-many-labs-can-i-create-under-the-same-subscription)
- [Kui palju VMs saab luua lab kohta?](#how-many-vms-can-i-create-per-lab)
- [Kuidas anda otsene minu Lab?](#how-do-i-share-a-direct-link-to-my-lab)
- [Mis on Microsofti konto?](#what-is-a-microsoft-account)
 
## <a name="troubleshooting"></a>Tõrkeotsing 
 
- [Minu artefakt nurjus VM loomise ajal. Kuidas see tõrkeotsing?](#my-artifact-failed-during-vm-creation-how-do-i-troubleshoot-it) 
- [Miks pole minu olemasoleva virtuaalse võrgu salvestamine õigesti?](#why-isnt-my-existing-virtual-network-saving-properly)  

### <a name="what-if-my-question-isnt-answered-here"></a>Mida teha, kui mu küsimusele pole siin vastata?
Kui küsimus pole siin loetletud, andke meile teada ja aitame teil leia vastust.

- Postitage küsimus [Disqus teemat](#comments) korduma Kippuvate lõpus ja meeskonnatöö Azure'i vahemälu ja muude kogukonnaliikmete kohta käesoleva artikli suhelda.
- Laiemalt saavutamiseks Postitage küsimus [Azure'i DevTest Labs MSDN-i Foorum](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureDevTestLabs)ja Azure DevTest Labs meeskonnatöö ja teiste liikmetega ühenduse.
- Veendumaks, et funktsioon taotlemiseks, esitada oma taotlusi ja ideede [Azure'i DevTest Labs kasutaja kõneposti](https://feedback.azure.com/forums/320373-azure-devtest-labs).

### <a name="why-should-i-use-azure-devtest-labs"></a>Miks kasutada Azure DevTest Labs? 
Azure'i DevTest Labs saate salvestada oma meeskonna aega ja raha. Arendajad saate luua oma keskkonnas, kasutades mitut eri alustel ja esemeid abil kiiresti juurutada ja konfigureerida rakendused. Kohandatud pilte ja valemite abil, virtuaalmasinates saab salvestada mallina ja hõlpsalt korrata. Lisaks labs pakuvad mitu konfigureeritav poliitikad, mis võimaldavad lab administraatorite vähendada jäätmeid ja hallata meeskonna keskkonnas. Need poliitikad kaasata automaatne sulgumist, maksumus lävi, kuni VMs VM Max suurused ja kasutaja kohta. Azure'i DevTest Labs põhjalikumalt selgitus [Ülevaade](devtest-lab-overview.md) lugeda või kontrollida [tutvustav video](/documentation/videos/videos/what-is-azure-devtest-labs). 

### <a name="what-does-worry-free-self-service-mean"></a>Mida tähendab "hõlpsalt, Iseteeninduslik"?
"Hõlpsalt, iseteenindusliku" tähendab, et arendajad ja katsetajate luua oma keskkonnas vastavalt vajadusele ja administraatoritel on see, et Azure'i DevTest Labs aitab minimeerida turvalisus jäätmete ja määrata kulude. Administraatorid saavad määrata, millised VM suurused on lubatud, VMs, maksimaalne arv ja kui VMs alustamine ja sulgeda. Azure'i DevTest Labs ka on lihtne jälgida kulud ja teada, kuidas lab ressursse kasutataks püsida teatiste seadmine. 

### <a name="how-can-i-use-azure-devtest-labs"></a>Kuidas kasutada Azure DevTest Labs? 
Azure'i DevTest Labs on kasulik niipea, kui vajate arendaja või testimiseks keskkonnas, ja soovite need kiiresti paljundada ja/või neid koos kokkuhoid poliitikate haldamine. 

Siin on mõned stsenaariumi, mis kliendid kasutavad Azure'i DevTest Labs jaoks. 

- Haldamine ühes kohas arendaja ja testige keskkonnas, kasutades poliitikat, et vähendada maksumuse ja kohandatud piltide jagamiseks koostab meeskond üle.
- Arendamise kasutav kohandatud piltide salvestamiseks kettale olekus kogu arengu etapid.
- Maksumus seoses edenemise jälgimine. 
- Hulgipostituste test keskkondades kvaliteedi assurance testimiseks loomine.
- Paljundada erinevaid rakendus ja hõlpsalt konfigureerimiseks esemeid ja valemite abil. 
- Levitamise VMs jaoks hackathons (arendaja või testi teha) ja seejärel hõlpsalt tühistage ettevalmistamise need sündmuse lõppemisel. 

### <a name="how-am-i-billed-for-azure-devtest-labs"></a>Kuidas maksmine Azure'i DevTest Labs jaoks? 
Azure'i DevTest Labs on tasuta teenus, tähendab, et labs loomine ja konfigureerimine poliitikad, malle ja esemeid on tasuta. Maksate ainult kasutada oma labs, nt virtuaalmasinates, salvestusruumi kontod ja virtuaalne võrkude Azure ressursse. Lab ressursside kulude kohta lisateabe saamiseks lugege [Azure'i DevTest Labs hinnad](https://azure.microsoft.com/pricing/details/devtest-lab/). 

### <a name="what-are-the-different-security-levels-in-azure-devtest-labs"></a>Mis on Azure DevTest Labsissa eri turvalisuse tasemete?  
Turvaline juurdepääs määratakse [Azure Role-Based juurdepääsu juhtimine (RBAC)](../active-directory/role-based-access-built-in-roles.md). Mõistmaks, kuidas access töötab, aitab see luba, rollid ja mille ulatus RBAC määratletud erinevuste mõistmine.

- **Luba** - luba on määratletud juurdepääsu teatud toimingu. Näiteks võib luba kõik Virtual lugemisõigus masinad. 
- **Roll** - rolli on kogumi õigused, mida saab rühmitatud ja kasutajale määratud. Näiteks "tellimuse omanik" on juurdepääs kõigi ressursside tellimus. 
- **Ulatus** – mille ulatus on Azure ressursi järjestuses. Näiteks, mille ulatus võib olla ressursirühma või ühe lab või kogu tellimus. 
 
Azure'i DevTest Labs piires on kahte tüüpi rollid kasutaja õiguste määramiseks: lab omanikku ja kasutajale lab.

- **Lab omaniku** - lab omanik on mis tahes vahendeid lab juurdepääs. Seetõttu need saate muuta poliitika, lugeda ja kirjutada mis tahes VMs, virtuaalse võrgu muutmine jne. 
- **Lab kasutaja** - lab kasutaja saab vaadata kõik lab ressursse, näiteks VMs, poliitika ja virtuaalse võrke, kuid need ei saa muuta poliitikate või mis tahes teiste kasutajate loodud VMs. Samuti on võimalik luua kohandatud rollid Azure'i DevTest Labsissa ja saate teada, kuidas teha artikli, [teatud lab poliitikate kasutusõiguste andmine](devtest-lab-grant-user-permissions-to-specific-lab-policies.md). 
 
Kuna otsinguulatuste hierarhilise, kui kasutaja on õigused teatud ulatusega, antakse need automaatselt need õigused iga hõlmas madalama taseme ulatuses. Näiteks, kui kasutaja on määratud roll tellimuse omanik, siis need on juurdepääs kõikidele ressurssidele tellimus. Järgmiste ressursside kaasata kõik virtuaalmasinates, kõik virtuaalse võrgu ja kõik labs. Seega tellimuse omanik pärib automaatselt lab omanik roll. Vastand pole täidetud. Lab omanikul juurdepääsu lab, mis on väiksem ulatus, kui tellimus. Seetõttu ei saa virtuaalmasinates või virtuaalse võrgu või mis tahes ressursse, mis on väljaspool lab lab omanik. 

### <a name="how-do-i-create-a-role-to-allow-users-to-perform-a-specific-task"></a>Kuidas luua roll, et kasutajad saaksid teatud toimingu?
Põhjaliku artikli kohta, kuidas luua kohandatud rollide ja õiguste määramine rolli leiate siit. Siin on näide skripti, mis loob roll "DevTest Labs Täpsemad kasutaja", kellel on õigus käivitamine ja peatamine kõik VMs lab:
 
    $policyRoleDef = Get-AzureRmRoleDefinition "DevTest Labs User" 
    $policyRoleDef.Actions.Remove('Microsoft.DevTestLab/Environments/*') 
    $policyRoleDef.Id = $null 
    $policyRoleDef.Name = "DevTest Labs Advance User" 
    $policyRoleDef.IsCustom = $true 
    $policyRoleDef.AssignableScopes.Clear() 
    $policyRoleDef.AssignableScopes.Add("subscriptions/<subscription Id>") 
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/virtualMachines/Start/action") 
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/virtualMachines/Stop/action") 
    $policyRoleDef = New-AzureRmRoleDefinition -Role $policyRoleDef  
 
### <a name="does-azure-devtest-labs-integrate-with-my-cicd-toolchain"></a>Kas Azure'i DevTest Labs integreerida minu CI/CD asukohti? 
Kui kasutate VSTS, on [Azure DevTest Labs tööülesannete laiend](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) , mis võimaldab teil oma väljaanne müügivõimaluste Azure'i DevTest Labsissa automatiseerimiseks. Mõned kasutab seda järgmised.

- Loomine ja juurutamine VM automaatselt ja seadistamine uusima koostamine Azure'i faili koopia või PowerShelli VSTS ülesannete abil. 
- Automaatselt hõivamine VM olek pärast testimine paljundada sama VM täiendavaks uurimiseks viga. 
- Kui see pole enam vaja, kustutamine VM väljaanne müügivõimaluste lõpus. 

Järgmist postitused pakuvad juhiseid ja VSTS laiend kasutamise kohta leiate:
 
- [Azure'i DevTest Labs – VSTS laiend](https://blogs.msdn.microsoft.com/devtestlab/2016/06/15/azure-devtest-labs-vsts-extension/) 
- [Uue VM sisse mõne olemasoleva AzureDevTestLab kaudu VSTS juurutamine](http://www.visualstudiogeeks.com/blog/DevOps/Deploy-New-VM-To-Existing-AzureDevTestLab-From-VSTS) 
- [Pidev juurutuste, et AzureDevTestLabs VSTS Redaktsioonide abil](http://www.visualstudiogeeks.com/blog/DevOps/Use-VSTS-ReleaseManagement-to-Deploy-and-Test-in-AzureDevTestLabs) 
 
Jaoks muud CI/CD toolchains, eelnevalt mainitud stsenaariumid, mida on võimalik VSTS ülesannete laiendamise sarnaselt saavutada juurutamine [Azure'i ressursihaldur mallide](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates) abil [Azure PowerShelli cmdlet-käskude](../resource-group-template-deploy.md) ja [.NET SDK-d](https://www.nuget.org/packages/Microsoft.Azure.Management.DevTestLabs/). [REST API-de jaoks DevTest Labs](http://aka.ms/dtlrestapis) abil saate oma asukohti integreerida.  

### <a name="why-cant-i-see-certain-vms-in-the-azure-virtual-machines-blade-that-i-see-within-azure-devtest-labs"></a>Miks ei kuvata teatud Azure'i Virtuaalmasinates tera, mis näen sees Azure'i DevTest Labs VMs?
Azure'i DevTest Labs loomisel VM õigus on antud juurdepääs selle VM. Teil on võimalik vaadata nii labs tera ja **Virtuaalmasinates** tera. Kasutajad DevTest Labs rolli näevad kõik virtuaalmasinates loodud lab lab **Kõik Virtuaalmasinates** tera kaudu. Siiski ei anta DevTest Labs roll kasutajatele automaatselt lugemisõigus VM ressursse, mis teised on loonud. Seetõttu ei kuvata need **Virtuaalmasinates** tera. 

### <a name="what-is-the-difference-between-custom-images-and-formulas"></a>Mis on kohandatud pilte ja valemite vaheline erinevus? 
Kohandatud pilt on valem pildiks, mida saate konfigureerida täiendavaid sätteid, mida saate salvestada ja paljundada on VHD (virtuaalse kõvaketta). Kohandatud pilt võib olla soovitatav, kui soovite kiiresti luua mitu keskkonnas sama lihtne, püsiv pilt. Valem võib olla paremini, kui soovite paljundada konfiguratsiooni oma VM uusima bittide, virtuaalse võrgu/alamvõrgu või kindlat suurust. Põhjalikum selgitus, leiate artiklist, [võrdlemine kohandatud pilte ja valemite DevTest Labsissa](devtest-lab-comparing-vm-base-image-types.md). 
 
### <a name="how-do-i-create-multiple-vms-from-the-same-template-at-once"></a>Kuidas luua mitme VMs sama malli korraga? 
Saate [VSTS tööülesannete laiendi](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) või [luua mõne Azure'i ressursihaldur malli](devtest-lab-add-vm-with-artifacts.md#save-arm-template) loomisel VM ja [Azure ressursihaldur malli Windows PowerShelli kaudu](../resource-group-template-deploy.md). 
 
### <a name="how-do-i-move-my-existing-azure-vms-into-my-azure-devtest-labs-lab"></a>Kuidas viimine minu Azure'i DevTest Labs lab minu olemasoleva Azure VMs? 
Meil on kavandamisel liikuda otse VMs Azure'i DevTest Labs lahenduse, kuid praegu saate kopeerida oma olemasoleva VMs Azure'i DevTest Labs järgmiselt: 

1. Kopeerige fail VHD oma olemasoleva VM [Windows PowerShelli skripti](https://github.com/Azure/azure-devtestlab/blob/master/Scripts/CopyVHDFromVMToLab.ps1) abil 
1. [Luua kohandatud pilt](devtest-lab-create-template.md) oma Azure'i DevTest Labs lab sees. 
1. Luua kohandatud pildilt lab VM 
 
### <a name="can-i-attach-multiple-disks-to-my-vms"></a>Saate lisada mitu ketast minu VMs? 
Manustamise mitu ketast VMs on toetatud.  
 
### <a name="how-do-i-automate-the-process-of-uploading-vhd-files-to-create-custom-images"></a>Kuidas luua kohandatud pilte VHD failide üleslaadimise protsessi automaatseks muuta? 
On kaks võimalust.

- [Azure'i AzCopy](../storage/storage-use-azcopy.md#blob-upload) saab kopeerida või VHD failide üleslaadimine seotud lab salvestusruumi konto.
- [Microsoft Azure'i salvestusruumi Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) on autonoomse rakendus, mis töötab Windows, OSX ja Linux.   
 
Sihtkoha salvestusruumi kontoga seostatud teie lab leidmiseks toimige järgmiselt.

1. [Azure'i portaali](http://go.microsoft.com/fwlink/p/?LinkID=525040)sisse logida. 
1. Valige vasakul paanil **Ressursi rühmad** . 
1. Leidke ja valige oma lab seostatud ressursirühma. 
1. Enne **Ülevaade** , valige üks salvestusruumi kontod. 
1. Valige **plekid**.
1. Otsige loendist üles. Kui pole, pöörduge tagasi sammu #4 ja proovige salvestusruumi mõne muu kontoga.
1. Kasutage oma AzCopy käsk sihtkohana **URL-i** .


### <a name="how-can-i-automate-the-process-of-deleting-all-the-vms-in-my-lab"></a>Kuidas saab kustutamisel kõik VMs minu lab automaatseks muuta?

VMs kustutada oma lab Azure portaali Lisaks saate kustutada kõik VMs teie lab PowerShelli skripti abil. Järgmises näites lihtsalt muutke parameetrite väärtused **väärtusi muuta** kommentaari all. Saate tuua selle `subscriptionId`, `labResourceGroup`, ja `labName` Azure'i portaalis keelest lab väärtused. 


    # Delete all the VMs in a lab
    
    # Values to change
    $subscriptionId = "<Enter Azure subscription ID here>"
    $labResourceGroup = "<Enter lab's resource group here>"
    $labName = "<Enter lab name here>"

    # Login to your Azure account
    Login-AzureRmAccount
    
    # Select the Azure subscription that contains the lab. This step is optional
    # if you have only one subscription.
    Select-AzureRmSubscription -SubscriptionId $subscriptionId
    
    # Get the lab that contains the VMs to delete.
    $lab = Get-AzureRmResource -ResourceId ('subscriptions/' + $subscriptionId + '/resourceGroups/' + $labResourceGroup + '/providers/Microsoft.DevTestLab/labs/' + $labName)
    
    # Get the VMs from that lab.
    $labVMs = Get-AzureRmResource | Where-Object { 
              $_.ResourceType -eq 'microsoft.devtestlab/labs/virtualmachines' -and
              $_.ResourceName -like "$($lab.ResourceName)/*"}
    
    # Delete the VMs.
    foreach($labVM in $labVMs)
    {
        Remove-AzureRmResource -ResourceId $labVM.ResourceId -Force
    }




### <a name="what-are-artifacts"></a>Mis on esemeid? 
Esemeid on kohandatav elemente, mida saab kasutada oma uusima bittide või teie Arendaja tööriistad peale VM juurutamine. Need on lisatud teie VM vaid paari klõpsuga loomise ajal ja kui VM on ette valmistatud, esemeid juurutada ja konfigureerida oma VM. Meie [avaliku Github hoidla](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts)on erinevaid olemasoleva esemeid, kuid saate teha ka lihtsalt [oma esemeid Autor](devtest-lab-artifact-author.md). 

### <a name="how-do-i-create-a-lab-from-an-azure-resource-manager-template"></a>Kuidas koostada lab mõni Azure ressursihaldur Mall? 
Meil on ette [Github hoidla lab Azure'i ressursihaldur malle](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates) , mida saate juurutada nimega-on või muuta, et luua oma labs kohandatud malle. Iga need Mallid on link, et saate klõpsata juurutamiseks lab nimega-all oma Azure tellimuse või saate kohandada malli ja [juurutamine PowerShelli või Azure CLI](../resource-group-template-deploy.md).
 
### <a name="why-are-my-vms-created-in-different-resource-groups-with-arbitrary-names-can-i-rename-or-modify-these-resource-groups"></a>Miks mu VMs erinevate ressursside rühmade suvalise nimedega loodud? Saate ümber nimetada või muuta nende ressursside rühmad? 
Ressursi rühmad on sellisel viisil loodud selleks, et Azure'i DevTest Labs kasutajaõiguste ja virtuaalmasinates juurdepääsu haldamiseks. Ajal teise ressursirühm VM saate teisaldada oma soovitud nime, tehes nii pole soovitatav. Me tegeleme rohkem paindlikkust seda kasutusvõimalusi parandada.   
 
### <a name="how-many-labs-can-i-create-under-the-same-subscription"></a>Kui palju labs saate luua ühe tellimuse jaotises? 
Ei ole teatud piiratud arv tellimuse kohta loodud labs. Siiski kasutada ressursid on piiratud tellimuse kohta. Saate lugeda [piirangud ja kvootide pandud Azure'i tellimused](../azure-subscription-service-limits.md) ja [kuidas need piirangud suurendada](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests). 
 
### <a name="how-many-vms-can-i-create-per-lab"></a>Kui palju VMs saab luua lab kohta? 
Ei ole teatud piiratud VMs kohta lab loodud arvu. Siiski praegu lab toetab ainult 40 VMs, mis töötab samal ajal standard laos ja 25 VMs töötab samaaegselt premium mälu. Me tegeleme praegu suurendab need piirangud. 

### <a name="how-do-i-share-a-direct-link-to-my-lab"></a>Kuidas anda otsene minu Lab?

Otselink lab kasutajate ühiskasutusse andmiseks saate teha järgmist.

1. Liikuge sirvides lab Azure'i portaalis.
2. Kopeerige lab URL brauseri kaudu ja selle ühiskasutusse lab kasutajad. 

>[AZURE.NOTE] Kui teie lab kasutajad on välised kasutajad [MSA konto](#what-is-a-microsoft-account) ja nad ei kuulu teie ettevõtte Active directory, nad saate tõrketeate navigeerimisel esitatud link. Kui need kuvatakse tõrketeade, paluge tal klõpsake Azure portaali paremas ülanurgas oma nime ja valige kataloog, kus lab olemas **Directory** jaotise menüü.

### <a name="what-is-a-microsoft-account"></a>Mis on Microsofti konto?

Microsofti konto on peaaegu kõike teha Microsoft seadmed ja teenused, mida te kasutate. See on meiliaadress ja parool, mida kasutate Skype'i, Outlook.com, OneDrive, Windows Phone ja Xbox LIVE – sisse logida ja see tähendab, et failide, fotode, kontaktide ja sätted saate järgige saate mis tahes seadmes. 

>[AZURE.NOTE] Microsofti konto varem nimetati "Windows Live'i ID".
 
### <a name="my-artifact-failed-during-vm-creation-how-do-i-troubleshoot-it"></a>Minu artefakt nurjus VM loomise ajal. Kuidas see tõrkeotsing? 
Vaadake ajaveebipostituse [tõrkeotsing puudumisel esemeid AzureDevTestLabs](http://www.visualstudiogeeks.com/blog/DevOps/How-to-troubleshoot-failing-artifacts-in-AzureDevTestLabs) - kirjutanud ühte meie MVP – saate teada, kuidas hankida logid oma nurjunud artefakt kohta. 
 
### <a name="why-isnt-my-existing-virtual-network-saving-properly"></a>Miks pole minu olemasoleva virtuaalse võrgu salvestamine õigesti?  
Üks võimalus on, et teie virtuaalne võrgu nimi sisaldab perioodi. Kui jah, proovige perioodide eemaldamine või asendamine sidekriipse ja proovige seejärel uuesti virtuaalse võrgu salvestada.
