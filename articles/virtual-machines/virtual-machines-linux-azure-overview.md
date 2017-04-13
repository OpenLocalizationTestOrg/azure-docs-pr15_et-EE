 <properties
   pageTitle="Azure'i ja Linux | Microsoft Azure'i"
   description="Kirjeldatakse Linuxi Azure'i arvutada, salvestusruumi ja võrk teenustega."
   services="virtual-machines-linux"
   documentationCenter="virtual-machines-linux"
   authors="vlivech"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="09/14/2016"
   ms.author="v-livech"/>

# <a name="azure-and-linux"></a>Azure'i ja Linux

Microsoft Azure'i on integreeritud avaliku kasvab kogum pilveteenused, sealhulgas analytics Virtuaalmasinates, andmebaaside, mobiil, networking, salvestusruumi, ja veebi – käepärane hosting oma lahendusi.  Microsoft Azure'i pakub scalable arvutite platvorm, mis võimaldab teil ainult maksma, mida te kasutate, kui soovite seda - ilma investeerida kohapealse riistvara.  Azure'i on valmis, kui olete oma lahenduste skaalal ja välja mis tahes skaala vaja teenuse klientide vajaduste.

Kui olete tuttav Amazoni erinevaid funktsioone AWS, saate uurida Azure vs AWS [määratlus vastenduse dokumendi](https://azure.microsoft.com/campaigns/azure-vs-aws/mapping/).


## <a name="regions"></a>Piirkondade
Microsoft Azure'i ressursid on jaotatud kogu maailmas mitme piirkondades.  "Piirkond" tähistab mitme andmekeskuste ühe geograafilise ala.  1 Jaanuar 2016 seisuga see rühm sisaldab: 8-Ameerikas, Euroopa 6 Aasia Vaikse ookeani Mandri-Hiinas 2 ja 3 India 2.  Kui soovite täieliku loendi kõigi Azure piirkondade, me säilitada loendi olemasolevate ja hiljuti teada regioonid.

- [Azure'i piirkondade](https://azure.microsoft.com/regions/)

## <a name="availability"></a>Kättesaadavus
Selleks, et saada meie 99,95 VM teenuselepingu juurutamise, juurutamiseks kaks või rohkem VMs töötab teie töökoormus sees saadavus seadmine. See tagab oma VMs on jaotatud viga mitu domeenides meie andmekeskuste samuti peale hosts erinevate hooldustööd Windowsiga juurutatud. Täielik [Azure'i SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_0/) selgitatakse Azure'i tagatud kättesaadavus kogu.

## <a name="azure-virtual-machines--instances"></a>Azure'i Virtuaalmasinates & eksemplari
Microsoft Azure'i toetab töötab arvu populaarsed Linuxi on esitanud ja haldab partnerite arv.  Siit leiate nagu punane rolli Enterprise, CentOS, Debian, Ubuntu, CoreOS, RancherOS, FreeBSD ja muud Azure'i turuplatsil jaotuse. Töötame aktiivselt erinevate Linuxi ühenduste lisada rohkem maitsed [Azure'i kinnitatud Linux Distros](virtual-machines-linux-endorsed-distros.md) loend.

Kui teie eelistatud Linuxi distributsiooni valik pole praegu galeriis, saate "tuua oma Linux", [luua ja üles laadida Linux VHD Azure](virtual-machines-linux-create-upload-generic.md)VM.

Azure'i virtuaalmasinates abil saate juurutada mitmesuguseid arvutuste lahenduste dünaamilised viisil. Saate juurutada praktiliselt kõigist töökoormus ja mis tahes keeleks, peaaegu iga operatsioonisüsteem - Windows, Linux, või luua kohandatud loomise ühte meie kasvab partnerite loendi kaudu. Ikka ei kuvata, mida otsite?  Ärge muretsege – saate ka tuua oma pilte asutusesisesest.

## <a name="vm-sizes"></a>VM suurused
Kui juurutate Azure VM, te ei kavatse valige VM suurus ühte meie suuruses sari, mis sobib teie töökoormus. Suuruse mõjutab ka töötlemise virtuaalse masina power, mälu ja salvestusruumi võimsus. Teil on arve põhjal aja VM töötab ja tarbimine selle eraldatud. [Suurused Virtuaalmasinates](virtual-machines-linux-sizes.md)täieliku loendi.

Siit leiate juhised, valimise VM suurus ühest meie sarja (A, D, DS, G ja GS).

* A-sarja VMs on meie hinnaga algtaseme VMs hele töökoormus ja arendaja katse stsenaariumid väärtus. Need on saadaval kõikides piirkondades ja saate ühendada ja kasutada standard ressursse, mis on saadaval virtuaalmasinates.
* A-sarja suurused (A8 - A11) on teisiti Arvuta intensiivse konfiguratsioone suure jõudlusega arvuti kobar rakenduste sobib.
* D-sarja VMs on mõeldud rakendusi, mis nõuavad kõrgema Arvuta power ja ajutine jõudluse parandamine. D-sarja VMs välkdraiv (SSD) kiiremini protsessorite ja suurem mälumahuga-core suhe ette ajutine ketas.
* Dv2 sarja, on meie D-sarja uusim versioon, võimsam CPU funktsioonid. Dv2-sarja CPU on umbes 35% kiiremini D-sarja CPU. See põhineb uusima genereerimine 2,4 GHz Inteli Xeon® E5-2673 v3 protsessor (Haskell), ja Inteli Turbo Boost Technology 2.0 saate üle minna kuni 3,2 GHz. Dv2-sarja on sama mälu ja kettaruumi konfiguratsioone sarja-D.
* G-seeria VMs pakuvad enamik mälu ja käivitage hosts Inteli Xeon E5 V3 pere protsessorid.

Märkus: DS-sari ja GS-sarja VMs on juurdepääs Premium mälu - meie SSD tagatud suure jõudlusega, latentsusajaga salvestusruumi I/O intensiivse töökoormus. Premium mälu on teatud piirkondades saadaval. Lisateavet leiate teemast:

- [Premium mälu: Suure jõudlusega Azure virtuaalse masina töökoormus salvestusruum](../storage/storage-premium-storage.md)

## <a name="automation"></a>Automatiseerimine
Proper DevOps kultuur saavutamiseks tuleb kõik taristu kood.  Kui kõik infrastruktuuri elu koodi saab hõlpsasti uuesti (Phoenix serverid).  Azure'i töötab kõik põhi automaatika, nt Ansible, Chef, SaltStack ja nuku instrumentaarium.  Azure'i on ka oma tööriistasid automatiseerimine.

- [Azure'i Mallid](virtual-machines-linux-create-ssh-secured-vm-from-template.md)

- [Azure'i VMAccess](virtual-machines-linux-using-vmaccess-extension.md)

Azure'i on üle enamik Linux Distros, mis toetavad seda jooksvalt välja tugi [pilveteenuses – käivitamise](http://cloud-init.io/) .  Praegu Canonical's Ubuntu VMs on juurutatud koos pilveteenuses – käivitamise vaikimisi sisse lülitatud.  RedHats RHEL, CentOS ja Fedora toetavad pilveteenuses – käivitamise, aga Azure pildid haldab RedHat ei ole pilveteenuses – käivitamise installitud.  Pilveteenuse – käivitamise RedHat pere OS kasutamiseks peate looma kohandatud pildi pilveteenuses – käivitamise installitud.

- [Azure Linux VMs pilveteenuses – käivitamise kasutamine](virtual-machines-linux-using-cloud-init.md)

## <a name="quotas"></a>Kvoote
Iga Azure'i tellimus on vaikimisi limiitide kohta, mis võivad mõjutada suure hulga VMs oma projekti juurutamise. Praeguse tellimuse kohta alusel on 20 VMs müüginäitajaid piirkonna kohta.  Limiitide saab tõsta, esitades tugi Piletite, paluda limiit kasvavad.  Täpsemat teavet limiitide kohta:

- [Azure'i tellitavas teenuses failimahupiirangute](../azure-subscription-service-limits.md)


## <a name="partners"></a>Partnerite

Pildid, mis on saadaval on värskendatud ja mõni Azure käitusaja jaoks optimeeritud partneritega tihedat Microsoft.  Kontrollige partnerite kohta lisateabe saamiseks nende turuplatsi lehtede allpool.

- [Linuxi Azure'i kinnitatud üldiste müügijaotustega tutvumine](virtual-machines-linux-endorsed-distros.md)

RedHat - [Azure'i turuplats - RedHat Enterprise Linux 7.2](https://azure.microsoft.com/marketplace/partners/redhat/redhatenterpriselinux72/)

Kanoonilise - [Azure'i turuplats - Ubuntu Server 16.04 LTS](https://azure.microsoft.com/marketplace/partners/canonical/ubuntuserver1604lts/)

Debian - [Azure'i turuplats - Debian 8 "Jessie"](https://azure.microsoft.com/marketplace/partners/credativ/debian8/)

FreeBSD - [Azure'i turuplats - FreeBSD 10.3](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/)

CoreOS - [Azure'i turuplats - CoreOS (stabiilne)](https://azure.microsoft.com/marketplace/partners/coreos/coreosstable/)

RancherOS - [Azure'i turuplats - RancherOS](https://azure.microsoft.com/marketplace/partners/rancher/rancheros/)

Bitnami - [Bitnami teegi Azure](https://azure.bitnami.com/)

Mesosfäärini - [Azure'i turuplats – näiteks Põhiliselt mesosfäärini OS Azure](https://azure.microsoft.com/marketplace/partners/mesosphere/dcosdcos/)

Keskmise suurusega - [Azure'i turuplats - ja keskmise suurusega sülem Azure'i teenus](https://azure.microsoft.com/marketplace/partners/microsoft/acsswarms/)

Jenkins - [Azure'i turuplats - CloudBees Jenkins platvorm](https://azure.microsoft.com/marketplace/partners/cloudbees/jenkins-platformjenkins-platform/)


## <a name="getting-setup-on-azure"></a>Saada Setup Azure
Azure'i kasutamise alustamiseks peate Azure'i konto, Azure'i CLI installitud ja paari SSH avalike ja privaatvõ võtmed.

## <a name="sign-up-for-an-account"></a>Konto kasutajaks registreerumine
Azure'i pilveteenuste kasutamise esimene samm on Azure'i konto jaoks registreeruda.  Avage [Azure'i konto](https://azure.microsoft.com/pricing/free-trial/) registreerumislehel alustada.

## <a name="install-the-cli"></a>Installige CLI
Azure'i kontosse abil saate kohe kasutamise alustamisel Azure portaali, mis on veebipõhine admin paneeli.  Azure'i pilveteenuses käsurea kaudu haldamise kohta leiate installimist on `azure-cli`.  Installige [Azure'i CLI ](../xplat-cli-install.md)oma Maci või Linuxi töökoha.

## <a name="create-an-ssh-key-pair"></a>Mõne SSH paari loomine
Nüüd on teil konto Azure, Azure veebiportaali ja Azure CLI.  Järgmiseks on luua SSH võti paari, mida kasutatakse SSH into Linux parooli kasutamata.  [Luua SSH klahvireas Linuxi ja Mac](virtual-machines-linux-mac-create-ssh-keys.md) lubada paroolita sisselogimise ja parema turvalisuse eesmärgil.

## <a name="getting-started-with-linux-on-microsoft-azure"></a>Microsoft Azure'i Linuxi töötamise alustamine
Azure'i konto häälestust, Azure'i CLI installitud ja SSH klahvid loodud nüüd olete valmis alustamiseks koostamise infrastruktuur Azure'i pilveteenuses.  Esimene ülesanne on luua VMs paar.

## <a name="create-a-vm-using-the-cli"></a>Luua VM CLI abil
Loomine abil CLI Linux VM on kiiresti juurutada VM terminal lahkumata töötate.  Kõik, mida saate määrata veebiportaali on saadaval käsurea lipu või vahetamine.  

- [Linux VM abil CLI loomine](virtual-machines-linux-quick-create-cli.md)

## <a name="create-a-vm-in-the-portal"></a>Luua VM portaalis
Azure'i veebiportaali loomise Linux VM on võimalus hõlpsasti käsk ja klõpsake käsku Saada juurutamine erinevate suvandite kaudu.  Käsurea lipud või asemel parameetrid, teil on võimalik kena veebivaade erinevate suvandite ja sätete kuvamine.  Kõik käsurea liides saadaval on ka portaalis.

- [Linux VM portaali loomine](virtual-machines-linux-quick-create-portal.md)

## <a name="login-using-ssh-without-a-password"></a>Logige SSH paroolita
VM töötab nüüd Azure ja olete valmis Logi sisse.  Paroolide kasutamise sisse logida SSH kaudu on ebaturvalise ja aeganõudev.  SSH klahvide abil on kõige turvaliselt ja kiireim viis Logi sisse.  Kui loote te Linux VM kaudu portaali või CLI, on teil kaks autentimise valikut.  Kui valite parooli SSH, konfigureerib Azure'i VM kaudu paroolid sisselogimise lubamiseks.  Kui otsustasite kasutada ka SSH avalik võti, Azure'i konfigureerib VM lubama ainult sisselogimise kaudu SSH võtmed ja keelab sisselogimise parool. Secure oma Linux VM, võimaldades ainult SSH võtme sisselogimise, kasutage SSH avaliku võtme suvandit portaali või CLI VM loomise ajal.

- [Keela SSH paroolid oma Linux VM SSHD konfigureerimise abil](virtual-machines-linux-mac-disable-ssh-password-usage.md)

## <a name="related-azure-components"></a>Seotud Azure komponendid

## <a name="storage"></a>Salvestusruumi

- [Microsoft Azure'i salvestusruumi tutvustus](../storage/storage-introduction.md)

- [Linux VM azure-cli abil ketta lisamine](virtual-machines-linux-add-disk.md)

- [Kuidas manustada Linux VM Azure portaalis kettal andmed](virtual-machines-linux-attach-disk-portal.md)

## <a name="networking"></a>Võrgunduse

- [Virtuaalse võrgu ülevaade](../virtual-network/virtual-networks-overview.md)

- [Azure'i IP-aadressid](../virtual-network/virtual-network-ip-addresses-overview-arm.md)

- [Avades pordid Linux VM Azure](virtual-machines-linux-nsg-quickstart.md)

- [Täielikult kvalifitseeritud domeeninime Azure portaali loomine](virtual-machines-linux-portal-create-fqdn.md)


## <a name="containers"></a>Ümbriste

- [Virtuaalmasinates ja Azure ümbriste](virtual-machines-linux-containers.md)

- [Azure'i teenus tutvustus](../container-service/container-service-intro.md)

- [Azure'i teenus on kobar juurutamine](../container-service/container-service-deployment.md)

## <a name="next-steps"></a>Järgmised sammud

Nüüd saate ülevaate Linux Azure.  Järgmise sammuna sukelduda ja luua mõni VMs!

- [Linux VM Azure portaali loomine](virtual-machines-linux-quick-create-portal.md)

- [Luua Linux VM Azure CLI abil](virtual-machines-linux-quick-create-cli.md)
