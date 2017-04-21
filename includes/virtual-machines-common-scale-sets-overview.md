

Rakendusi mis tuleb mastaapimiseks Arvuta ressursid välja ja, skaala toimingud on peidetult tasakaalustatud viga ja Värskenda domeenides. VM skaala tutvustus komplekti leiate [Azure'i ajaveebi teadaanne](https://azure.microsoft.com/blog/azure-vm-scale-sets-public-preview/)tehtud.

Heitke pilk neid videoid, VM skaala komplekti kohta lisateavet:

 - [Mark Russinovich räägib Azure skaala komplektid](https://channel9.msdn.com/Blogs/Regular-IT-Guy/Mark-Russinovich-Talks-Azure-Scale-Sets/)  

 - [Virtuaalse masina skaala määrab mees Bowerman abil](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-191-Virtual-Machine-Scale-Sets-with-Guy-Bowerman)

## <a name="creating-and-managing-vm-scale-sets"></a>Määrab loomise ja haldamise VM skaala

VM skaala komplekti saate määratleda ja juurutada JSON mallide ja [REST API-de](https://msdn.microsoft.com/library/mt589023.aspx) nagu üksikute Azure ressursihaldur VMs abil. Seetõttu saab kasutada mis tahes standard Azure'i ressursihaldur juurutamise võimalust. Mallide kohta leiate lisateavet teemast [Azure ressursihaldur loome Mallid](../articles/resource-group-authoring-templates.md).

Näide mallide VM skaala komplektide leiate Azure'i Kiirjuhend Mallid GitHub hoidla siit:

[https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates) – vaadake _vmss_ pealkiri malle.

Need Mallid lehtedel üksikasjad kuvatakse nupp, mis ühendab portaali juurutamine funktsioon. Juurutada VM skaala määramine, klõpsake nuppu ja seejärel täitke portaalis vajalikud parameetrid. Kui te pole kindel, kas ressursi toetab üla- või kombineeritud juhul oleks turvalisem Kasuta alati väiketähtedes parameetrite väärtused. Olemas on ka mugav video lahkamine VM skaala määramine malli siin:

[VM skaala määramine malli lahkamine](https://channel9.msdn.com/Blogs/Windows-Azure/VM-Scale-Set-Template-Dissection/player)

## <a name="scaling-a-vm-scale-set-out-and-in"></a>Skaleerimist VM skaala seadmine ja

Suurendamiseks või vähendamiseks virtuaalmasinates VM skaala komplekti arvu, lihtsalt muuta atribuudi _võimsus_ ja Juurutage uuesti mall. See lihtne lihtne kirjutada oma kohandatud skaleerimise kiht, kui soovite määrata kohandatud mastaap sündmused, mis ei toeta Azure autoscale.

Kui on ümbersuunamine malli võimsus muuta, võite määratleda palju väiksem malli, mis sisaldab ainult kasutatava SKU ja värskendatud võimsus. Siin on esitatud näide selle: [https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-scale-in-or-out/azuredeploy.json](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-linux-nat/azuredeploy.json).

Sammult skaala komplekt, mis on automaatselt mastaabitud loodud, lugege teemat [Automaatselt skaala masinad virtuaalse masina skaala määramine](../articles/virtual-machines/virtual-machines-windows-vmss-powershell-creating.md)

## <a name="monitoring-your-vm-scale-set"></a>Jälgida oma VM skaala määramine

Praegu on soovitatav [Azure'i ressursi Exploreri](https://resources.azure.com) abil saate vaadata VM skaala komplektid. VM skaala terminikomplektid on Microsoft.Compute, klõpsake jaotises ressursi näeksite selle saidi neid laiendab järgmisi linke:

    subscriptions -> your subscription -> resourceGroups -> providers -> Microsoft.Compute -> virtualMachineScaleSets -> your VM scale set -> etc.

## <a name="vm-scale-set-scenarios"></a>VM skaala seadmine stsenaariumid

Selles jaotises on loetletud mõned tüüpilised VM skaala määramine stsenaariumid. Mõned kõrgema taseme Azure teenuseid (nt paketi, teenuse struktuuri, Azure'i teenus) kasutab järgmisi olukordi.

 - **RDP / SSH VM skaala set eksemplari** – A VM skaala määramine on loodud sees on VNET ja üksikute VMs eraldamata avaliku IP-aadressid. See on hea, kuna te ei soovi üldiselt pea jaotamise eraldi IP-aadresside kõik teie Arvuta ruudustikus kodakondsuseta ressursside kulude ja haldus kohal ja saate hõlpsalt luua ühenduse nende vms muudest ressurssidest oma VNET, sealhulgas need, mis on nagu koormus soolise või autonoomse virtuaalmasinates avaliku IP-aadressid.

 - **Ühenduse loomine VMs NAT reeglite kasutamine** – saate luua avaliku IP-aadressi, määrata laadi koormusetasakaalustusteenuse ja määratlege sissetuleva NAT reeglid, mis porti IP-aadressi vastendamine porti VM VM ulatuse määramine. Nt

    Avaliku IP -> Port 50000 -> vmss\_0 -> Port 22 avaliku IP - > Port 50001 -> vmss\_1 -> Port 22 avaliku IP - > Port 50002 -> vmss\_2 -> Port 22

    Siin on näide VM skaala komplekti, mis kasutab NAT reeglite lubamiseks SSH iga VM skaalal määrata ühe avaliku IP-ühendus luua: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat)

    Siin on näide seda sama RDP ja aknad: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat)

 - **Ühenduse loomine abil "jumpbox" VMs** - kui loote VM skaala määramine ja eraldi VM sama VNET, autonoomne VM ja VM skaala määramine VMs saab ühendada ühe teise abil oma ettevõtte IP-aadressid VNET/alamvõrgu määratletud. Kui loote avaliku IP-aadress ja määrata autonoomse VM saate RDP või SSH autonoomse VM ja ühendage see arvutist oma VM skaala määramine eksemplarid. Võite märgata sel hetkel, et VM skaala lihtsate on potentsiaalselt turvalisemaks lihtsa autonoomse VM avaliku IP-aadress oma vaikimisi konfiguratsiooni.

    Seda moodust, näiteks see mall loob lihtsa Mesos kobar koosneb autonoomse juhtslaidi VM, kes haldab VM skaala seadistatud vastavalt kobar vms: [https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json](https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json)

 - **Round jaan koormusetasakaalustuseks VM skaala seadmine eksemplarid** - kui soovite pakkuda töö arvutikobaraga vms "round jaan" lähenemisviisi, saate konfigureerida on Azure laadi koormusetasakaalustusteenuse koormust tasakaalustavad reeglite vastavalt sellele. Samuti saate määratleda, kinnitamaks, et teie rakendus töötab pingida sondid pordid määratud protokolli intervall ja taotluse teega.

    Siin on näide, mis loob VM skaala kogumi VMs töötab IIS-i veebiserverisse ja kasutab laadi koormusetasakaalustusteenuse laadi, mida iga VM saab tasakaalustamiseks. Samuti kasutab HTTP-protokolli ping teatud URL-i iga VM: [https://github.com/gbowerman/azure-myriad/blob/master/vmss-win-iis-vnet-storage-lb.json](https://github.com/gbowerman/azure-myriad/blob/master/vmss-win-iis-vnet-storage-lb.json) - pilk Microsoft.Network/loadBalancers ressursi tüüp ja networkProfile ja klõpsake soovitud virtualMachineScaleSet extensionProfile.

 - **Juurutamine VM skaala seadmine arvutikobaraga PaaS kobar Manager** - VM skaala komplektid on mõnikord kirjeldatakse nii edasi-genereerimine töötaja roll. Sobiv kirjeldus, kuid see töötab ka skaala ohtu seadmine funktsioonide PaaS v1 töötaja rolli funktsioone. Mõttes VM skaala komplektid pakkuda on true "töötaja roll" või töötaja ressurss, et nad pakuvad generalized Arvuta ressurssi, mis on platvormi/käitusaja sõltumatult, kohandatavad ja Azure ressursihaldur IaaS on kaasatud.

    PaaS v1 töötaja roll, samal ajal piiratud seoses platform/runtime tugi (ainult Windowsi platvormi pildid) hõlmab ka teenuseid, näiteks VIP Vaheta, konfigureeritav täiendamise sätted, käitusaja/rakenduse juurutamine teatud sätted, mis ei ole kas _veel_ saadaval VM mastaapimiseks komplektid või muude kõrgema taseme PaaS teenused nagu teenuse struktuuri toimetatakse. Seda arvestage, et saate vaadata VM skaala komplekti infrastruktuur, mida toetab PaaS. St PaaS lahenduste nagu teenuse struktuuri või kobar haldurid nagu Mesos saate koostada peal VM mastaapimiseks scalable Arvuta kiht nimega komplektid.

    Siin on näide Mesos kobar, mis kasutab VM skaala määrata sama VNET eraldi VM. Autonoomse VM on Mesos juhtslaidi ja VM skaala määramine tähistab kogumi ori sõlmed: [https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json](https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json). Tulevaste versioonide [Azure'i teenus](https://azure.microsoft.com/blog/azure-container-service-now-and-the-future/) on juurutada rohkem kompleksarvu/kõva versioonid seda stsenaariumi, mis põhineb VM skaala komplektid.

## <a name="vm-scale-set-performance-and-scale-guidance"></a>VM skaala määramine jõudlus ja skaala juhised

- Avaliku eelvaate ajal ei Loo rohkem kui 500 VMs mitme VM skaala komplekti korraga.
- Lepingule kuni 40 vms salvestusruumi konto kohta.
- Salvestusruumi kontonimed võimalikult palju esitähed hajutamine.  Näide VMSS Mallid [Azure'i Kiirjuhend Mallid](https://github.com/Azure/azure-quickstart-templates/) näiteid, kuidas seda teha.
- Kui kasutate kohandatud VMs, kavandamine rohkem kui 40 VMs ühe salvestusruumi konto VM skaala komplektis.  Peate eelnevalt kopeeritud salvestusruumi kontole enne alustamist VM skaala määramine juurutamise pilt. Kohta leiate lisateavet teemast.
- Lepingule kuni 2048 vms VNET kohta.  Selle piirmäära suurendatakse tulevikus.
- Saate luua VMs arv on piiratud meiliteavitus Arvuta core mis tahes ala. Võimalik, et peate võtke ühendust klienditoega suurendamiseks oma Arvuta kvoodilimiit suurendada isegi juhul, kui teil on kõrge piirata valdkond pilveteenustega või IaaS v1 täna. Päringu meiliteavitus käivitada Azure'i CLI järgmine käsk: _Azure'i vm loendi-kasutus_- ja PowerShelli järgmine käsk: _Get-AzureRmVMUsage_ (kui _Get-AzureVMUsage_PowerShelli versiooni 1.0 all kasutada).

## <a name="vm-scale-set-frequently-asked-questions"></a>VM skaala seadmine korduma kippuvad küsimused

**K.** Kui palju VMs teil on kogumi VM skaala?

**V-SSE.** Kui kasutate platvormi pilte, mida saab üle mitme salvestusruumi kontod levitada 100. Kui kasutate kohandatud pilte, kuni 40, kuna kohandatud pildid on piiratud ühe salvestusruumi konto eelvaate.

**K** mis ressursi piirangud on olemas VM skaala komplektide?

**V-SSE.** Teil on piiratud rohkem kui 500 VMs loomine mitme skaala komplektid müüginäitajaid piirkonna kohta perioodil eelvaade. Olemasoleva [Azure tellimuse teenuste piirangud /](../articles/azure-subscription-service-limits.md) rakendada.

**K.** Andmete ketast toetatud jäävad VM skaala komplektid?

**V-SSE.** Pole esialgse versioonis. Teie valikud andmete talletamiseks on:

- Azure'i failid (ühiskasutuses SMB draivid)

- OS ketas

- Temp ketas (kohalik, mitte toetavad Azure storage)

- Välisandmete teenuse (nt remote DB, Azure tabelid, Azure plekid)

**K.** Azure'i piirkonnad toetavad VM skaala komplektide?

**V-SSE.** Mis tahes piirkond, mis toetab Azure ressursihaldur toetab VM skaala komplektid.

**K.** Kuidas saate luua VM skaala komplekti, kasutades kohandatud pilt?

**V-SSE.** Jätke vhdContainers atribuut tühjaks, näiteks:

    "storageProfile": {
        "osDisk": {
            "name": "vmssosdisk",
            "caching": "ReadOnly",
            "createOption": "FromImage",
            "image": {
                "uri": [https://mycustomimage.blob.core.windows.net/system/Microsoft.Compute/Images/mytemplates/template-osDisk.vhd](https://mycustomimage.blob.core.windows.net/system/Microsoft.Compute/Images/mytemplates/template-osDisk.vhd)
            },
            "osType": "Windows"
        }
    },


**K.** Kui vähendada minu VM skaala seatud võimsus 20 15, mis eemaldatakse VMs?

**V-SSE.** Virtuaalmasinates eemaldatakse maksimeerimine kättesaadavus kogu täiendamine domeenide ja viga domeenide ühtlaselt määratud skaala.

**K.** Kuidas see kui 15 võimet 18 seejärel tõsta?

**V-SSE.** Kui tõstate 18 VMs indeks 15, 16, luuakse 17. Mõlemal juhul on VMs tasakaalustatud FDs ja UDs.

**K.** VM skaala määramine mitmele laiendid kasutamisel saate ma jõustada ka täitmise järjestus?

**V-SSE.** Mitte otse, kuid pikendamise customScript skripti võib oodata teise laiend lõpuleviimiseks (nt jälgides laiendamine log - vt [https://github.com/Azure/azure-quickstart-templates/blob/master/201-vmss-lapstack-autoscale/install\_lap.sh](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vmss-lapstack-autoscale/install_lap.sh)).

**K.** Tehke VM skaala komplektid Azure-saadavus komplekti töötada?

**V-SSE.** Jah. VM skaala on peidetud saadavus 3 FDs ja 5 UDs. Te ei pea midagi all virtualMachineProfile konfigureerimine. Tulevikus versioonidega, VM skaala komplektid on tõenäoliselt üle mitme rentniku, kuid nüüd skaala on ühe-saadavus kogum.
