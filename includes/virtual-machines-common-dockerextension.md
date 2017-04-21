

[Keskmise suurusega](https://www.docker.com/) on üks kõige populaarsemate virtualization lähenemisel, mis kasutab [Linux ümbriste](http://en.wikipedia.org/wiki/LXC) asemel virtuaalmasinates viis rakenduse andmete eraldamiseks ja arvuti ühiskasutusega ressursid. [Azure keskmise suurusega VM laiend](https://github.com/Azure/azure-docker-extension/blob/master/README.md) [Azure Linux Agent](../articles/virtual-machines/virtual-machines-linux-agent-user-guide.md) abil saate luua keskmise suurusega VM, mis hostib suvalist arvu ümbriste Azure rakendusi.

Selles teemas kirjeldatakse:

+ [Keskmise suurusega ja Linux ümbriste]
+ [Kuidas kasutada Azure keskmise suurusega VM laiend]
+ [Windows ja Linux virtuaalse masina laiendid]

Keskmise suurusega lubatud VMs kohe loomiseks vaadake teemat:

+ [Kuidas kasutada keskmise suurusega VM laiend: Azure'i käsurea liides (Azure'i CLI)]
+ [Kuidas kasutada Azure klassikaline portaali keskmise suurusega VM laiend]
+ [Kuidas alustada kiiresti keskmise suurusega Azure'i turuplatsilt]

Pikendamise ja kuidas see toimib kohta leiate lisateavet teemast [Keskmise suurusega laiend kasutusjuhendis](https://github.com/Azure/azure-docker-extension/blob/master/README.md).

## <a name="docker-and-linux-containers"></a>Keskmise suurusega ja Linux ümbriste
[Keskmise suurusega](https://www.docker.com/) on üks kõige populaarsemate virtualization lähenemisel, mis kasutab [Linux ümbriste](http://en.wikipedia.org/wiki/LXC) asemel virtuaalmasinates viis andmete eraldamiseks ja arvuti ühiskasutusega ressursid ja pakub muid teenuseid, mis võimaldavad teil koostada või rakenduste kiiresti koguda ja neid jagada teiste keskmise suurusega ümbriste vahel.

Keskmise suurusega ja Linux ümbriste pole [Hypervisors](http://en.wikipedia.org/wiki/Hypervisor) nagu Windows Hyper-V ja [KVM](http://www.linux-kvm.org/page/Main_Page) Linux (on palju teisi näiteid). Hypervisors Virtualisoinnilla aluseks operatsioonisüsteemi täieliku opsüsteemides (ehk *virtuaalmasinates*) käivitamiseks selle hypervisor sees, nagu oleks tegemist rakenduse lubamiseks.

Keskmise suurusega ja muude *container* lähenemisel kahanenud põhjalikult nii tarbitud käivitamise aja ja andmetöötlus ja kohal nõutav, kasutades protsessi ja faili süsteemi eraldamise funktsioone Linuxi tuum esitamist ainult tuum funktsioonid teisiti eraldatud ümbrises.

Järgmises tabelis kirjeldatakse väga kõrge funktsioonide erinevused mis on olemas hypervisors ja Linux ümbriste tüüpi. Pange tähele, et mõned funktsioonid võib-olla rohkem või vähem soovitav sõltuvalt rakenduse oma vajadustele.

|   Funktsioon      | Hypervisors | Ümbriste  |
| :------------- |-------------| ----------- |
| Protsessi eraldamise | Rohkem või vähem lõpuleviimine | Kui root container host ohtu |
| Nõutav vaba mälumahuga | Täieliku OS plus rakendused | Ainult rakenduse nõuded |
| Alusta kuluv aeg | Oluliselt pikem: Buutimine OS pluss rakenduse laadimine | Oluliselt lühem: ainult rakendused on vaja käivitamine, sest tuum töötab juba  |
| Container automatiseerimine | Erinev sõltuvalt OS ja rakendused | [Keskmise suurusega pildi Galerii](https://registry.hub.docker.com/); teised

Üksikasjalik arutelu ümbriste ja nende eeliseid, leiate artiklist [Keskmise suurusega kõrge taseme tahvel](http://channel9.msdn.com/Blogs/Regular-IT-Guy/Docker-High-Level-Whiteboard).

Mis on keskmise suurusega ja kuidas see toimib tõesti kohta leiate lisateavet teemast [mis on keskmise suurusega?](https://www.docker.com/whatisdocker/)

#### <a name="docker-and-linux-container-security-best-practices"></a>Keskmise suurusega ja Linux Container Turve head tavad

Kuna ümbriste juurdepääsu hosti arvuti tuum, ühiskasutusse anda, kui pahatahtlik kood on võimalik saada root võib olla mitte ainult selle arvuti, vaid ka muude ümbriste pääsete. Tagamine teie süsteemi container tungivalt kui vaikimisi konfiguratsiooni, [keskmise suurusega soovitab](https://docs.docker.com/articles/security/) lisamine rühmapoliitika või [Rollipõhine Turve](http://en.wikipedia.org/wiki/Role-based_access_control) , nt [SELinux](http://selinuxproject.org/page/Main_Page) või [AppArmor](http://wiki.apparmor.net/index.php/Main_Page), näiteks samuti vähendada võimalikult palju tuum funktsioonid, mis on ümbriste. Lisaks on palju muid dokumente Internetis, mis kirjeldavad lähenemisel turvalisus abil ümbriste nagu keskmise suurusega.

## <a name="how-to-use-the-docker-vm-extension-with-azure"></a>Kuidas kasutada Azure keskmise suurusega VM laiend

Keskmise suurusega VM laiend on osa, mis on installitud, mis ise installib engine keskmise suurusega ja haldab remote suhtlemine VM loodud VM eksemplari. On kaks võimalust installida VM laiend: teie VM haldusportaali abil saate luua või saate luua: Azure'i käsurea liides (Azure'i CLI).

Portaali abil saate lisada mis tahes ühilduvad Linux VM keskmise suurusega VM laiend (praegu ainult pilt, mis toetab seda on Ubuntu 14.04 LTS pilt uuem kui juuli). Azure'i CLI käsureal, siiski saate installida keskmise suurusega VM laiend ja luua ja üles laadida sertide keskmise suurusega teatis kõik samal ajal kui loote VM eksemplari.

Keskmise suurusega lubatud VMs kohe loomiseks vaadake teemat:

+ [Kuidas kasutada keskmise suurusega VM laiend: Azure'i käsurea liides (Azure'i CLI)]
+ [Kuidas kasutada Azure klassikaline portaali keskmise suurusega VM laiend]

## <a name="virtual-machine-extensions-for-linux-and-windows"></a>Windows ja Linux virtuaalse masina laiendid
[Keskmise suurusega VM laiend Azure](https://github.com/Azure/azure-docker-extension/blob/master/README.md) on vaid üks mitmele VM laiendid, mis pakuvad teisiti käitumine ja Lisaks on väljatöötamisel. Näiteks mitmed [Linuxi VM Agent laiend](../articles/virtual-machines/virtual-machines-linux-agent-user-guide.md) funktsioonid võimaldavad teil muuta ja hallata virtuaalse masina, sh turbefunktsioonid, tuum ja võrgunduse ja jne. VMAccess laiend saate näiteks SSH võti ja administraatori parooli lähtestamine.

Täieliku loendi leiate [Azure'i VM laiendid](../articles/virtual-machines/virtual-machines-windows-extensions-features.md).

<!--Anchors-->
[Kuidas kasutada keskmise suurusega VM laiend: Azure'i käsurea liides (Azure'i CLI)]: http://azure.microsoft.com/documentation/articles/virtual-machines-docker-with-xplat-cli/
[Kuidas kasutada Azure klassikaline portaali keskmise suurusega VM laiend]: http://azure.microsoft.com/documentation/articles/virtual-machines-docker-with-portal/
[Kuidas alustada kiiresti keskmise suurusega Azure'i turuplatsilt]: http://azure.microsoft.com/documentation/articles/virtual-machines-docker-ubuntu-quickstart/
[Keskmise suurusega ja Linux ümbriste]: #Docker-and-Linux-Containers
[Kuidas kasutada Azure keskmise suurusega VM laiend]: #How-to-use-the-Docker-VM-Extension-with-Azure
[Windows ja Linux virtuaalse masina laiendid]: #Virtual-Machine-Extensions-For-Linux-and-Windows
