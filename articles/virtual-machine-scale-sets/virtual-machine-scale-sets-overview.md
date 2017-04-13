<properties
    pageTitle="Virtuaalse masina skaala määrab ülevaade | Microsoft Azure'i"
    description="Lisateavet virtuaalse masina skaala komplektid"
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
    ms.topic="get-started-article"
    ms.date="09/13/2016"
    ms.author="guybo"/>

# <a name="virtual-machine-scale-sets-overview"></a>Virtuaalse masina skaala komplekti ülevaade

Virtuaalse masina skaala terminikomplektid on Azure arvutada ressursi abil saate juurutada ja hallata identse VMs kogumist. Kõik VMs konfigureeritud sama VM skaala komplektid toetamiseks true autoscale – pole eelnevalt ettevalmistamise vms on vaja ning sellisena on hõlpsam koostada suuremahuliste teenuste suunamise suur Arvuta, big data ja konteinerite töökoormus.

Rakendusi mis tuleb mastaapimiseks Arvuta ressursid välja ja, skaala toimingud on peidetult tasakaalustatud viga ja Värskenda domeenides. Sissejuhatus VM skaala komplektid leiate [Azure'i ajaveebi teadaanne](https://azure.microsoft.com/blog/azure-virtual-machine-scale-sets-ga/).

Heitke pilk neid videoid, VM skaala komplekti kohta lisateavet:

 - [Mark Russinovich räägib Azure skaala komplektid](https://channel9.msdn.com/Blogs/Regular-IT-Guy/Mark-Russinovich-Talks-Azure-Scale-Sets/)  

 - [Virtuaalse masina skaala määrab mees Bowerman abil](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-191-Virtual-Machine-Scale-Sets-with-Guy-Bowerman)

## <a name="creating-and-managing-vm-scale-sets"></a>Määrab loomise ja haldamise VM skaala

Saate luua VM skaala määramine [Azure portaali](https://portal.azure.com) , valige _Uus_ ning tippides "skaala" otsinguriba. "Virtuaalse masina skaala määramine" näete tulemusi. Seal saate täitke nõutavad väljad kohandada ja kasutada oma skaala määramine. 

VM skaala komplekti saate määratleda ja juurutada JSON Mallid ja [REST API-de](https://msdn.microsoft.com/library/mt589023.aspx) nagu üksikute Azure ressursihaldur VMs abil. Seetõttu saab kasutada mis tahes standard Azure'i ressursihaldur juurutamise võimalust. Mallide kohta leiate lisateavet teemast [Azure ressursihaldur loome Mallid](../resource-group-authoring-templates.md).

Näide mallide VM skaala komplektide leiate Azure'i Kiirjuhend Mallid GitHub hoidla [siin.](https://github.com/Azure/azure-quickstart-templates) (otsige malle _vmss_ pealkiri)

Need Mallid lehtedel üksikasjad kuvatakse nupp, mis ühendab portaali juurutamine funktsioon. Juurutada VM skaala määramine, klõpsake nuppu ja seejärel täitke portaalis vajalikud parameetrid. Kui te pole kindel, kas ressursi toetab üla- või kombineeritud juhul oleks turvalisem Kasuta alati väiketähtedes parameetrite väärtused. Olemas on ka mugav video lahkamine VM skaala määramine malli siin:

[VM skaala määramine malli lahkamine](https://channel9.msdn.com/Blogs/Windows-Azure/VM-Scale-Set-Template-Dissection/player)

## <a name="scaling-a-vm-scale-set-out-and-in"></a>Skaleerimist VM skaala seadmine ja

Suurendamiseks või vähendamiseks virtuaalmasinates VM skaala komplekti arvu, lihtsalt muuta atribuudi _võimsus_ ja Juurutage uuesti mall. See lihtne lihtne kirjutada oma kohandatud skaleerimise kiht, kui soovite määrata kohandatud mastaap sündmused, mis ei toeta Azure autoscale.

Kui on ümbersuunamine malli võimsus muuta, võite määratleda palju väiksema malli, mis sisaldab ainult kasutatava SKU ja värskendatud võimsus. Järgmine näide on kuvatud [siin.](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing)

Sammult skaala komplekt, mis on automaatselt mastaabitud loodud, lugege teemat [Automaatselt skaala masinad virtuaalse masina skaala määramine](virtual-machine-scale-sets-windows-autoscale.md)

## <a name="monitoring-your-vm-scale-set"></a>Jälgida oma VM skaala määramine

Nii [Azure portaali](https://portal.azure.com) loendite skaala komplektid ja kuvab lihtsa atribuudid, kui ka kirjet VMs määramine. Täpsemat teavet saate [Azure'i Resource Explorer](https://resources.azure.com) VM skaala komplekti kuvamiseks. VM skaala terminikomplektid on Microsoft.Compute, klõpsake jaotises ressursi näeksite selle saidi neid laiendab järgmisi linke:

    subscriptions -> your subscription -> resourceGroups -> providers -> Microsoft.Compute -> virtualMachineScaleSets -> your VM scale set -> etc.

## <a name="vm-scale-set-scenarios"></a>VM skaala seadmine stsenaariumid

Selles jaotises on loetletud mõned tüüpilised VM skaala määramine stsenaariumid. Mõned kõrgema taseme Azure teenuseid (nt paketi, teenuse struktuuri, Azure'i teenus) kasutab järgmisi olukordi.

 - **RDP / SSH VM skaala set eksemplari** – A VM skaala määramine on loodud sees on VNET ja üksikute VMs skaala määramine eraldamata avaliku IP-aadressid. See on hea, kuna te ei soovi üldiselt pea jaotamise eraldi avaliku IP-aadressid kõik teie Arvuta ruudustikus kodakondsuseta ressursside kulude ja haldus kohal ja saate hõlpsalt luua ühenduse nende vms muudest ressurssidest oma VNET, sealhulgas need, mis on nagu koormus soolise või autonoomse virtuaalmasinates avaliku IP-aadressid.

 - **Ühenduse loomine VMs NAT reeglite kasutamine** – saate luua avaliku IP-aadress, määramine laadi koormusetasakaalustusteenuse ja määratlege sissetuleva meili NAT reeglid, mis porti IP-aadressi vastendamine porti VM VM skaala määramine. Näiteks:
 
    Allikas | Lähteport | Sihtkoht | Sihtport
    --- | --- | --- | ---
    Avaliku IP | Pordi 50000 | vmss\_0 | Pordi 22
    Avaliku IP | Pordi 50001 | vmss\_1 | Pordi 22
    Avaliku IP | Pordi 50002 | vmss\_2 | Pordi 22

    Siin on näide VM skaala komplekti, mis kasutab NAT reeglite lubamiseks SSH iga VM skaalal määrata ühe avaliku IP-ühendus luua: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat)

    Siin on näide seda sama RDP ja aknad: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat)

 - **Ühenduse loomine abil "jumpbox" VMs** - kui loote VM skaala määramine ja eraldi VM sama VNET, autonoomne VM ja VM skaala määramine VMs saab ühendada ühe teise abil oma sisemine IP-aadressid VNET/alamvõrgu määratletud. Kui loote avaliku IP-aadress ja määrata autonoomse VM saate RDP või SSH autonoomse VM ja ühendage see arvutist oma VM skaala määramine eksemplarid. Võite märgata sel hetkel, et VM skaala lihtsate on potentsiaalselt turvalisemaks lihtsa autonoomse VM avaliku IP-aadress oma vaikimisi konfiguratsiooni.

    [Seda moodust, näiteks see mall loob lihtsa Mesos kobar koosneb eraldi juhtslaidi VM, kes haldab VM skaala seadistatud vastavalt kobar vms.](https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json)

 - **Koormusetasakaalustuseks VM skaala eksemplari määramine** – kui soovite pakkuda töö arvutikobaraga vms "round jaan" lähenemisviisi, saate konfigureerida on Azure laadi koormusetasakaalustusteenuse koormust tasakaalustavad reeglid vastavalt sellele. Saate määratleda sondid kinnitamaks, et teie rakendus töötab pingida pordid määratud protokolli intervall ja taotluse teega. Azure'i [Rakenduse lüüsi](https://azure.microsoft.com/services/application-gateway/) toetab samuti skaala komplektid koos keerukamaid laadi stsenaariumid tasakaalustamiseks.

    [Siin on näide, mis loob VM skaala kogumi VMs töötab IIS-i veebiserverisse ja kasutab laadi koormusetasakaalustusteenuse laadi, mida iga VM saab tasakaalustamiseks. Samuti kasutab ping teatud URL-i iga VM HTTP-protokolli.](https://github.com/gbowerman/azure-myriad/blob/master/vmss-win-iis-vnet-storage-lb.json) (vaadata Microsoft.Network/loadBalancers ressursi tüüp ja networkProfile ja klõpsake soovitud virtualMachineScaleSet extensionProfile)

 - **Juurutamine VM skaala seadmine arvutikobaraga PaaS kobar Manager** - VM skaala komplektid on mõnikord kirjeldatakse nii edasi-genereerimine töötaja roll. Sobiv kirjeldus, kuid see töötab ka skaala ohtu seadmine funktsioonide PaaS v1 töötaja rolli funktsioone. Mõttes VM skaala komplektid pakkuda on true "töötaja roll" või töötaja ressurss, et nad pakuvad generalized Arvuta ressurssi, mis on platvormi/käitusaja sõltumatult, kohandatavad ja Azure ressursihaldur IaaS on kaasatud.

    PaaS v1 töötaja roll, samal ajal piiratud seoses platform/runtime tugi (ainult Windowsi platvormi pildid) hõlmab ka teenuseid, näiteks VIP Vaheta, konfigureeritav täiendamise sätted, käitusaja/rakenduse juurutamine teatud sätted, mis ei ole kas _veel_ saadaval VM mastaapimiseks komplektid või muude kõrgema taseme PaaS teenused nagu teenuse struktuuri toimetatakse. Seda arvestage, et saate vaadata VM skaala komplekti infrastruktuur, mida toetab PaaS. St PaaS lahenduste nagu teenuse struktuuri või kobar haldurid nagu Mesos saate koostada peal VM mastaapimiseks scalable Arvuta kiht nimega komplektid.

    [Seda moodust, näiteks see mall loob lihtsa Mesos kobar koosneb eraldi juhtslaidi VM, kes haldab VM skaala seadistatud vastavalt kobar vms.](https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json) Tulevaste versioonide [Azure'i teenus](https://azure.microsoft.com/blog/azure-container-service-now-and-the-future/) on juurutada rohkem kompleksarvu/kõva versioonid seda stsenaariumi, mis põhineb VM skaala komplektid.

## <a name="vm-scale-set-performance-and-scale-guidance"></a>VM skaala määramine jõudlus ja skaala juhised

- Luua rohkem kui 500 VMs mitme VM skaala kogumitena korraga.
- Salvestusruumi konto kohta rohkem kui 20 VMs kavandamine (kui atribuudi _unarusse_ "false" seadmiseks, mille puhul te ei lähe kuni 40).
- Salvestusruumi kontonimed võimalikult palju esitähed hajutamine.  Näide VMSS Mallid [Azure'i Kiirjuhend Mallid](https://github.com/Azure/azure-quickstart-templates/) näiteid, kuidas seda teha.
- Kui kasutate kohandatud VMs, kavandamine rohkem kui 40 VMs ühe salvestusruumi konto VM skaala komplektis.  Peate eelnevalt kopeeritud salvestusruumi kontole enne alustamist VM skaala määramine juurutamise pilt. Kohta leiate lisateavet teemast.
- Lepingule kuni 4096 vms VNET kohta.
- Saate luua VMs arv on piiratud core kvoodi võtate kasutusele piirkonnas. Võimalik, et peate võtke ühendust klienditoega suurendamiseks oma Arvuta kvoodilimiit suurendada isegi juhul, kui teil on kõrge piirata valdkond pilveteenustega või IaaS v1 täna. Päringu meiliteavitus käivitada Azure'i CLI järgmine käsk: `azure vm list-usage`, ja PowerShelli järgmine käsk: `Get-AzureRmVMUsage` (kui PowerShelli versiooni 1.0 kasutamine all `Get-AzureVMUsage`).

## <a name="vm-scale-set-frequently-asked-questions"></a>VM skaala seadmine korduma kippuvad küsimused

**K.** Kui palju VMs teil on kogumi VM skaala?

**V-SSE.** Kui kasutate platvormi pilte, mida saab üle mitme salvestusruumi kontod levitada 100. Kui kasutate kohandatud pilte, kuni 40 (kui _unarusse_ atribuut on seatud "väär" 20 vaikimisi), kuna kohandatud pildid on praegu vaid ühe salvestusruumi konto.

**K** mis ressursi piirangud on olemas VM skaala komplektide?

**V-SSE.** Teil on rohkem kui 500 VMs loomine mitme skaala komplekti piirkonna kohta 10 minuti jooksul. Olemasoleva [Azure tellimuse teenuste piirangud /](../azure-subscription-service-limits.md) rakendada.

**K.** Andmete ketast toetatud jäävad VM skaala komplektid?

**V-SSE.** Pole esialgse versioonis. Teie valikud andmete talletamiseks on:

- Azure'i failid (ühiskasutuses SMB draivid)

- OS ketas

- Temp ketas (kohalik, mitte toetavad Azure storage)

- Azure'i andmed teenuse (nt Azure tabelid, Azure plekid)

- Välisandmete teenuse (nt remote DB)

**K.** Azure'i piirkonnad toetavad VM skaala komplektid?

**V-SSE.** Mis tahes piirkond, mis toetab Azure ressursihaldur toetab VM skaala komplektid.

**K.** Kuidas luua VM skaala, määrata kohandatud pildi kasutamine

**V-SSE.** Jätke vhdContainers atribuut tühjaks, näiteks:

    "storageProfile": {
        "osDisk": {
            "name": "vmssosdisk",
            "caching": "ReadOnly",
            "createOption": "FromImage",
            "image": {
                "uri": "https://mycustomimage.blob.core.windows.net/system/Microsoft.Compute/Images/mytemplates/template-osDisk.vhd"
            },
            "osType": "Windows"
        }
    },


**K.** Kui vähendada minu VM skaala seatud võimsus 20 15, mis eemaldatakse VMs?

**V-SSE.** Virtuaalmasinates eemaldatakse maksimeerimine kättesaadavus kogu täiendamine domeenide ja viga domeenide ühtlaselt määratud skaala. Esmalt eemaldatakse VMs kõrgeim id-ga.

**K.** Kuidas see kui 15 võimet 18 seejärel tõsta?

**V-SSE.** Kui võimet 18 suurendamiseks klõpsake 3 uue VMs luuakse. Iga kord, kui VM eksemplari id suurendatakse eelmise kõrgeimast väärtusest (nt 20, 21, 22) kaudu. VMs on tasakaalustatud FDs ja UDs.

**K.** VM skaala määramine mitmele laiendid kasutamisel saate ma jõustada ka täitmise järjestus?

**V-SSE.** Mitte otse, kuid pikendamise customScript skripti võiks oodata teise laiend ([nt jälgides laiend Logi](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vmss-lapstack-autoscale/install_lap.sh)) lõpuleviimiseks. Laiend järjestuse täiendavad juhised leiate sellest ajaveebipostitusest: [Azure'i VM skaala kogumitena laiend järjestuse](https://msftstack.wordpress.com/2016/05/12/extension-sequencing-in-azure-vm-scale-sets/).

**K.** Tehke VM skaala komplektid Azure-saadavus komplekti töötada?

**V-SSE.** Jah. VM skaala on peidetud saadavus 5 FDs ja 5 UDs. Te ei pea midagi all virtualMachineProfile konfigureerimine. Tulevikus versioonidega, VM skaala komplektid on tõenäoliselt üle mitme rentniku, kuid nüüd skaala on ühe-saadavus kogum.
