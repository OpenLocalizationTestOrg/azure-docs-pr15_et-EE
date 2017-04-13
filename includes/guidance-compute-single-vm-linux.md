See artikkel kirjeldab tõestatud tavade töötab Linux virtuaalse masina (VM) Azure, tähelepanu skaleeritavus, kättesaadavus, hallatavust ja turvalisus. Azure'i toetab erinevate populaarsed Linuxi, sh CentOS, Debian, punane rolli Enterprise, Ubuntu ja FreeBSD töötab. Lisateavet leiate teemast [Azure ja Linux][azure-linux].

> [AZURE.NOTE] Azure'i on kaks erinevat juurutamise mudelit: [Ressursihaldur] [ resource-manager-overview] ja klassikaline. Selles artiklis kasutab ressursihaldur, mis soovitab Microsoft kasutada uut juurutuste.

Me ei soovita kasutamise ühe VM tootmise töökoormus, sest puudub üles-kellaaeg teenuse taseme leping (SLA) ühe vms Azure. SLA saamiseks peate juurutama [kättesaadavus]on mitu VMs[availability-set]. Lisateavet leiate teemast [töötab mitu VMs Azure][multi-vm]. 

## <a name="architecture-diagram"></a>Arhitektuur skeem

Ettevalmistamise VM Azure hõlmab rohkem liikuvad osad, kui lihtsalt VM. Arvuta, võrgunduse ja salvestusruumi elemente, mida tuleb arvestada on.

> Visio dokumendi, mis sisaldab arhitektuur diagramm on saadaval alla laadida [Microsoft allalaadimiskeskuse][visio-download]. See diagramm on "Arvuta - ühe VM" leht.

![[0]][0]

- **Ressursirühm.** [_Ressursirühm_] [ resource-manager-overview] on ümbris, mis on seotud ressursid. Hoidke ressursside selle VM ressursi rühma loomine.

- **VM**. Saate ettevalmistamise avaldatud piltide loetelu või virtuaalse kõvaketta (VHD) Faili üleslaadimine Azure'i bloobimälu VM.

- **OS ketas.** OS ketas on VHD, mis on talletatud [Azure storage][azure-storage]. See tähendab, et see ei lahene, isegi siis, kui majutusseadme läheb alla. OS ketas on `/dev/sda1`.

- **Ajutiste ketas.** VM luuakse ajutine ketas. Füüsilise draivi majutusseadme salvestatakse see ketas. On _Salvestatud Azure salvestusruumi ja võib minema taaskäivitamisega ja muid VM elutsükli sündmusi ajal_ . See ketas kasutada ainult ajutine andmed, nt lehe või Vaheta failid. Ajutiste ketas on `/dev/sdb1` ja on paigaldatud `/mnt/resource` või `/mnt`.

- **Andmete ketast.** [Andmete kettale] [ data-disk] on püsivate VHD, kasutatakse rakenduse andmeid. Andmete ketast on talletatud Azure salvestusruum, nt OS ketas.

- **Virtuaalse võrgu (VNet) ja alamvõrgu.** Iga VM Azure juurutatakse sisse (VNet), mis on jagatud alamvõrku.

- **Avaliku IP-aadress.** Avaliku IP-aadress on vaja suhelda VM&mdash;näiteks üle SSH.

- **Võrgu liidese (NIC)**. NIC lubab suhelda virtuaalse võrgu VM.

- **Võrgu turberühma (NSG)**. [NSG] [ nsg] kasutatakse lubada/keelata alamvõrgu võrguliiklust. Saate seostada ka NSG on üksikud NIC või mõne alamvõrgu. Kui saate seostada ka alamvõrku, NSG reeglite rakendamine kõik selle alamvõrgu VMs.
 
- **Diagnostika.** Diagnostikalogimise on oluline, haldamine ja tõrkeotsingu VM.

## <a name="recommendations"></a>Soovitused

Azure'i pakub palju erinevaid ressursse ja ressursside tüübid, nii, et see viide arhitektuur võib olla ette valmistatud mitmel erineval viisil. Oleme andnud mõni Azure ressursihaldur Mall viide arhitektuur soovituste järgneva installimiseks. Kui soovite luua oma viite arhitektuur peaksite nende soovituste täitmisel juhul, kui teil on konkreetse nõude, mis ei sobi soovitus. 

### <a name="vm-recommendations"></a>VM soovitused

Soovitame DS - ja GS-sarja, sest need seadme suurused toetavad [Premium salvestusruumi][premium-storage]. Valige üks nende masina suurust, juhul, kui teil on näiteks suure jõudlusega arvutuste eriotstarbelisi töökoormus. Lisateavet leiate teemast [virtuaalse masina suurused][virtual-machine-sizes]. Mõne olemasoleva töökoormus liikudes Azure alustada VM suurus, mis vastab teie kohapealne serverites. Seejärel mõõta oma tegelik töökoormus kohta CPU, mälu ja kettaruumi sisend/väljund toimingute sekundis (IOPS) ja vajadusel suurust muuta. Lisaks, kui teil on vaja mitu NICs, olema teadlik NIC limiit iga suurus.  

Kui olete ette VM ja muud ressursid, määrake asukoht. Üldiselt, valige oma sisemiste kasutajate või klientidele kõige lähemal asukoht. Siiski võib kõik VM suurused kõigi kohtade saadaval. Lisateavet leiate teemast [teenuste regiooniti][services-by-region]. Loendi antud kohas, läiked VM, käivitage Azure käsurea liides (CLI) järgmine käsk:

```
    azure vm sizes --location <location>
```

Avaldatud VM pildi valimise kohta leiate teemast [navigeerimine ja valige Azure virtuaalse masina pilte][select-vm-image].

### <a name="disk-and-storage-recommendations"></a>Ketas ja salvestusruumi soovitused

Parima ketta I/O jõudluse tagamiseks soovitame [Premium salvestusruumi][premium-storage], mis salvestab andmed välkdraivide (SSD). Maksumus põhineb ettevalmistatud kettal suurust. IOPS ja läbilaskevõime (ehk teisisõnu öeldes andmete edastamiseks määr) ka sõltuvad ketta suuruse, nii kui olete ette kettal, leiavad kõik kolm tegurit (maht, IOPS ja läbilaskevõime). 

Ühe salvestusruumi konto toetab 1 kuni 20 VMs.

Lisage üks või enam andmete ketast. Kui loote on VHD, on vormindamata. Logige sisse VM formaadis kettale. Andmete ketast nimega Näita `/dev/sdc`, `/dev/sdd`, jne. Saate käivitada `lsblk` loetleda Blokeeri seadmed, sh selle ketast. Andmete kettapuhastusriista kasutamiseks partition ja faili süsteemi loomine ja mount ketas. Näiteks:

```bat
    # Create a partition.
    sudo fdisk /dev/sdc     # Enter 'n' to partition, 'w' to write the change.     
    
    # Create a file system.
    sudo mkfs -t ext3 /dev/sdc1
    
    # Mount the drive.
    sudo mkdir /data1
    sudo mount /dev/sdc1 /data1
```

Kui teil on palju andmeid ketast, arvestage kokku I/O piirangud salvestusruumi konto. Lisateavet leiate teemast [Virtual arvuti kettale piirangute][vm-disk-limits].

Kui lisate andmed kettale, loogilise üksuse number (LUN) ID on määratud ketas. Soovi korral saate määrata LUN ID &mdash; näiteks, kui te olete asendamine ketas ja soovite säilitada sama LUN ID-d või kui teil on rakendus, mis otsib teatud LUN ID-ga. Pidage meeles, et LUN ID peab olema kordumatu iga ketta.

Kui soovite muuta I/O ajasti, optimeerida jõudluse kohta välkdraivide (SSD) (kasutatakse Premiumi salvestusruumi). Ühine soovitus on SSD NOOP ajasti kasutamiseks, kuid peaksite kasutama tööriist, näiteks [iostat] jälgida oma kindla töökoormus ketta I/O jõudlus.

- Parima jõudluse tagamiseks hoidke diagnostikalogid eraldi salvestusruumi konto loomine. Kohalik liigsete salvestusruumi (LRS) tavakasutajakonto piisab diagnostikalogid.


### <a name="network-recommendations"></a>Võrgu soovitused

Avaliku IP-aadressi võib olla dünaamiline või staatiline. Vaikimisi on dünaamiline.

- Reserveerida a [staatiline IP-aadress] [ static-ip] kui teil on vaja fikseeritud IP-aadress, mis ei muuda &mdash; näiteks, kui peate looma DNS-i A-kirje või vajate oma IP-aadressi olema whitelisted.

- Saate luua ka on täielik domeeninimi (FQDN) IP-aadressi. Seejärel saate registreerida [CNAME-kirje] [ cname-record] DNS-i, mis viitab FQDN. Lisateabe saamiseks lugege teemat [loomine täielikult kvalifitseeritud domeeninime Azure'i portaalis][fqdn].

Kõik NSGs sisaldavad [vaikereeglit][nsg-default-rules], sh kõigi sissetulevate Interneti-liikluse blokeerib reegel. Vaikimisi reegleid ei saa kustutada, kuid saate neid alistada muude reeglitega. Interneti-liikluse lubamiseks looge reeglid, mis võimaldavad sissetulev liiklus teatud pordid &mdash; näiteks port 80 http.  

SSH lubamiseks reegli lisamine NSG, mis võimaldab sissetulev liiklus TCP port 22.

## <a name="scalability-considerations"></a>Kaalutlused skaleeritavus.

Te saate mastaapimiseks VM üles või alla, [muutes VM suurus][vm-resize]. 

Mastaapimiseks horisontaalselt, pannakse taha laadi koormusetasakaalustusteenuse seadmine saadavus kahe või enama VMs. Lisateavet leiate teemast [töötab mitu VMs Azure][multi-vm].

## <a name="availability-considerations"></a>Kättesaadavus kaalutlused

Nagu ülal, ei ole SLA ühe VM. SLA saamiseks tuleb juurutada mitme VMs on kättesaadavus komplekt.

Teie VM võib mõjutada [kavandatud hooldustööd] [ planned-maintenance] või [planeerimata hooldustööd][manage-vm-availability]. Saate kasutada [VM taaskäivitage logid] [ reboot-logs] määratlemaks, kas VM uuesti põhjustas kavandatud hooldustööd.

VHDs on tagatud [Azure Storage][azure-storage], mis on kopeeritud kestvus ja kättesaadavuseks.

Kaitsta juhuslike andmete kaotsimineku ajal toiminguid (nt tõttu kasutaja tõrge), tuleks rakendada punkti õigel ajal varukoopiaid, kasutades [bloobimälu hetktõmmiseid] [ blob-snapshot] või mõnda muud tööriista.

## <a name="manageability-considerations"></a>Kaalutlused hallatavus

**Ressursi rühmad.** Tihedalt seotud ressursid, millest ühte [Ressursirühma]sama elutsükli ühiskasutus sellele[resource-manager-overview]. Ressursi rühmad võimaldavad juurutada ja jälgida ressursside rühmana kokku võtma arveldus ressursirühm kulusid. Kustutada saab ka kogumina, mis on väga kasulik test juurutuste ressursid. Pange ressursse tähendusega nimed. Mis on hõlpsam leida kindla ressursi ja oma rolli mõistmine. Leiate [Azure'i ressursid soovitatav Nimereeglid][naming conventions].

**SSH**. Enne, kui loote Linuxi VM, luua 2048-bitine RSA era-avalik võti paari. Kasutada avaliku võtme faili loomisel VM. Lisateabe saamiseks vaadake, [Kuidas kasutada SSH Linux ja Mac Azure][ssh-linux].

**VM diagnostika.** Luba jälgimine ja diagnostika, sh põhilisi mõõdikute, diagnostika taristu logid ja [buutimine diagnostika][boot-diagnostics]. Buutimine diagnostika abil saate diagnoosimine buutimine tõrge oma VM sattumisel-käivitatava oleku. Lisateabe saamiseks lugege teemat [lubamine diagnostika- ja][enable-monitoring].  

Järgmine käsk CLI võimaldab diagnostika.

```
    azure vm enable-diag <resource-group> <vm-name>
```

**Peatamine VM.** Azure'i vahet olekus "Tasuda" ja "Deallocated". Teilt kui VM olek on "lõpetanud". Te ei võeta kui VM deallocated.

Kasutage Deallocate VM CLI järgmine käsk:

```
    azure vm deallocate <resource-group> <vm-name>
```

- Azure'i portaalis nupp **Peata** deallocates ka VM. Siiski sulgemisel OS kaudu sisse loginud, VM peatatakse, kuid _mitte_ deallocated, seega saate endiselt tuleb tasuda.

**Kustutamine VM.** Kui kustutate VM, kuvatakse VHDs ei kustutata. See tähendab, et saate kustutada turvaline VM kaotamata andmeid. Siiski saate endiselt tuleb tasuda Storage. Funktsiooni VHD kustutamiseks kustutage fail [bloobimälu][blob-storage].

Vältida juhuslikku kustutamist, kasutage [Ressursi Lukusta] [ resource-lock] lukustamiseks kogu ressursi rühma või lukustamine üksikute ressursse, nt VM. 

## <a name="security-considerations"></a>Turvalisuse alused

[OSPatching] VM laiend abil automaatseks muuta OS värskendused. Installige see laiend, kui olete ette VM. Saate määrata, kuidas sageli installida plaastrid ja kas pärast lappimine taaskäivitage.

Kasutage [Rollipõhine juurdepääsu reguleerimine] [ rbac] (RBAC) juurdepääsu Azure ressursse, mis juurutamist. RBAC saate määrata oma DevOps meeskonna liikmed autoriseerimine rollid. Näiteks lugeja roll saate vaadata Azure ressursid, kuid ei luua, hallata või kustutage need. Azure'i ressursile tüübid on mõned rollid. Näiteks virtuaalse masina kaasautori roll saate taaskäivitage või deallocate VM, administraatori parooli lähtestamine, luua VM ja jne. Muud [Sisseehitatud RBAC-rollid] [ rbac-roles] mis võib olla kasulik see viide arhitektuur kaasata [DevTest Labs kasutaja] [ rbac-devtest] ja [Võrgu kaasautor][rbac-network]. 

Kasutaja saab määrata mitu rollid ja saate luua kohandatud rollid rohkem kohandatud õigused.

> [AZURE.NOTE] RBAC ei piira toiminguid, mida VM logitud kasutaja saab teha. Need õigused on määratud külaline OS konto tüüp.   

Kasutage [audit logid] [ audit-logs] ettevalmistamise toimingute ja muid sündmusi VM kuvamiseks.

- Kaaluge [Azure'i ketta krüptimise] [ disk-encryption] kui teil on vaja ketast OS- ja krüptimine. 

## <a name="solution-deployment"></a>Lahenduse juurutamine

[Juurutamise] [ github-folder] viite arhitektuur, mis näitab järgmistest headest tavadest on saadaval. See viide arhitektuur sisaldab virtuaalse võrgu (VNet), võrgu turberühma (NSG) ja ühe virtuaalse masina (VM).

On mitu võimalust, juurutada see viide arhitektuur. Lihtsaim viis on täitke järgmised juhised. 

1. Paremklõpsake nuppu ja valige kas "Ava link uus vahekaart" või "Ava link uues aknas".
[![Azure'i juurutamine](../articles/guidance/media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-single-vm%2Fazuredeploy.json)

2. Kui link on avatud Azure'i portaalis, peate sisestama väärtused mõned sätted: 
    - **Ressursirühm** nimi on juba määratletud parameeter faili, seega valige **Loo uus** ja sisestage `ra-single-vm-rg` tekstiväljale.
    - Valige piirkonnale **asukoht** rippmenüüst.
    - **Malli Root Uri** või **Parameetri Root Uri** tekstivälju ei saa redigeerida.
    - Valige rippmenüüst **Linuxi** **Os tüüp** .
    - Tingimused läbi ja seejärel märkige ruut **nõustun eespool tingimused** .
    - Klõpsake nuppu **osta** .

3. Oodake, kuni juurutuse lõpuleviimiseks.

4. Parameetri faile lisada raske koodiga administraatori kasutajanime ja parooli ja, on soovitatav kohe muuta nii. Klõpsake VM nimega `ra-single-vm0 `Azure'i portaalis. Seejärel klõpsake jaotises **tugi + tõrkeotsingu** **parooli lähtestamine** . Valige rippmenüüst **režiimi** **parooli lähtestamine** ja seejärel valige uus **kasutajanimi** ja **parool**. Nuppu **Värskenda** jää püsima uue kasutajanime ja parooli.

Täiendavaid võimalusi juurutamine see viide arhitektuur kohta leiate teavet teemast readme-faili [juhiseid-ühe-vm] [ github-folder] Github kausta.

## <a name="customize-the-deployment"></a>Juurutamise kohandamine

Juurutamise oma vajadustele vastavaks muutmiseks järgige [juhiseid-ühe-vm] [ github-folder] lehe.

## <a name="next-steps"></a>Järgmised sammud

Selleks, et [SLA jaoks Virtuaalmasinates] [ vm-sla] rakendada, peate juurutama kättesaadavus-Set kahe või enama eksemplari. Lisateavet leiate teemast [töötab mitu VMs Azure][multi-vm].

<!-- links -->

[audit-logs]: https://azure.microsoft.com/en-us/blog/analyze-azure-audit-logs-in-powerbi-more/
[availability-set]: ../articles/virtual-machines/virtual-machines-windows-create-availability-set.md
[azure-cli]: ../articles/virtual-machines-command-line-tools.md
[azure-linux]: ../articles/virtual-machines/virtual-machines-linux-azure-overview.md
[azure-storage]: ../articles/storage/storage-introduction.md
[blob-snapshot]: ../articles/storage/storage-blob-snapshots.md
[blob-storage]: ../articles/storage/storage-introduction.md
[boot-diagnostics]: https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/
[cname-record]: https://en.wikipedia.org/wiki/CNAME_record
[data-disk]: ../articles/virtual-machines/virtual-machines-linux-about-disks-vhds.md
[disk-encryption]: ../articles/security/azure-security-disk-encryption.md
[enable-monitoring]: ../articles/monitoring-and-diagnostics/insights-how-to-use-diagnostics.md
[fqdn]: ../articles/virtual-machines/virtual-machines-linux-portal-create-fqdn.md
[github-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-single-vm/
[iostat]: https://en.wikipedia.org/wiki/Iostat
[manage-vm-availability]: ../articles/virtual-machines/virtual-machines-linux-manage-availability.md
[multi-vm]: ../articles/guidance/guidance-compute-multi-vm.md
[naming conventions]: ../articles/guidance/guidance-naming-conventions.md
[nsg]: ../articles/virtual-network/virtual-networks-nsg.md
[nsg-default-rules]: ../articles/virtual-network/virtual-networks-nsg.md#default-rules
[OSPatching]: https://github.com/Azure/azure-linux-extensions/tree/master/OSPatching
[planned-maintenance]: ../articles/virtual-machines/virtual-machines-linux-planned-maintenance.md
[premium-storage]: ../articles/storage/storage-premium-storage.md
[rbac]: ../articles/active-directory/role-based-access-control-what-is.md
[rbac-roles]: ../articles/active-directory/role-based-access-built-in-roles.md
[rbac-devtest]: ../articles/active-directory/role-based-access-built-in-roles.md#devtest-lab-user
[rbac-network]: ../articles/active-directory/role-based-access-built-in-roles.md#network-contributor
[reboot-logs]: https://azure.microsoft.com/en-us/blog/viewing-vm-reboot-logs/
[Resize-VHD]: https://technet.microsoft.com/en-us/library/hh848535.aspx
[Resize virtual machines]: https://azure.microsoft.com/en-us/blog/resize-virtual-machines/
[resource-lock]: ../articles/resource-group-lock-resources.md
[resource-manager-overview]: ../articles/azure-resource-manager/resource-group-overview.md
[select-vm-image]: ../articles/virtual-machines/virtual-machines-linux-cli-ps-findimage.md
[services-by-region]: https://azure.microsoft.com/en-us/regions/#services
[ssh-linux]: ../articles/virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md
[static-ip]: ../articles/virtual-network/virtual-networks-reserved-public-ip.md
[storage-price]: https://azure.microsoft.com/pricing/details/storage/
[virtual-machine-sizes]: ../articles/virtual-machines/virtual-machines-linux-sizes.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vm-disk-limits]: ../articles/azure-subscription-service-limits.md#virtual-machine-disk-limits
[vm-resize]: ../articles/virtual-machines/virtual-machines-linux-change-vm-size.md
[vm-sla]: https://azure.microsoft.com/en-us/support/legal/sla/virtual-machines/v1_0/
[readme]: https://github.com/mspnp/reference-architectures/blob/master/guidance-compute-single-vm
[components]: #Solution-components
[blocks]: https://github.com/mspnp/template-building-blocks
[0]: ./media/guidance-blueprints/compute-single-vm.png "Ühe Linux VM arhitektuur Azure"

