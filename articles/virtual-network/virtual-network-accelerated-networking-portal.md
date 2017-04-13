<properties 
   pageTitle="Kiirendada networking virtuaalse masina - portaali jaoks | Microsoft Azure'i"
   description="Saate teada, kuidas konfigureerida kiirendada Networking Azure virtuaalse masina, mis Azure'i portaalis."
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/26/2016"
   ms.author="jdial" />

# <a name="accelerated-networking-for-a-virtual-machine"></a>Kiirendatud võrgunduse virtuaalse masina jaoks

> [AZURE.SELECTOR]
- [Azure'i portaal](virtual-network-accelerated-networking-portal.md)
- [PowerShelli](virtual-network-accelerated-networking-powershell.md)

Kiirendatud võrgunduse võimaldab ühe juurkausta i/o Virtualization (SR-IOV) virtuaalse masina (VM), parandades oma võrgu jõudlust. See suure jõudlusega tee, mille liiklus möödub host Andmeteed, vähendada latentsus, närvitsema ja Protsessori kasutuse jaoks kõige rangete võrgu töökoormus toetatud VM tüüpide kaudu. Selles artiklis selgitatakse, kuidas kasutada Azure portaali konfigureerimiseks kiirendada Networking Azure'i ressursihaldur juurutamise mudel. Kiirendada võrgutoe Azure PowerShelli abil saate luua ka VM. Saate teada, kuidas, klõpsake välja PowerShelli selle artikli alguses.

Järgmisel pildil on kujutatud kahe virtuaalmasinates (VM) ja ilma kiirendada Networking suhtlemine:

![Võrdlus](./media/virtual-network-accelerated-networking-portal/image1.png)

Ilma kiirendada võrgu kõigi võrgu liikluse ja sealt VM tuleb läbida selle hosti ja virtual switch. Virtuaalne lüliti pakub kõik poliitika jõustamine, nt võrgu turberühmad, pääsuloendid, eraldamise ja muude teenuste võrgu virtualiseeritud võrguliiklust. Lisateabe saamiseks lugege artiklit [Hyper-V võrgu Virtualization ja virtuaalse vahetamine](https://technet.microsoft.com/library/jj945275.aspx) .

Kiirendada võrgutoe võrguliiklust saabub võrgu kaart (NIC) ja seejärel edastatakse VM. Kõigi võrgu poliitika, mis virtual switch kehtib ilma kiirendada Networking on üle viidud ja riistvara rakendatud. Riistvara poliitika rakendamist võimaldab NIC VM, säilitades kõik selle hosti rakendatakse see poliitika selle hosti ja virtuaalse parameetrit mööda otse edasi võrguliikluse abil.

Kiirendada Networking eelised rakenduvad ainult VM, mis see on sisse lülitatud. Parimate tulemuste saamiseks sobib vähemalt kaks VMs ühendatud sama VNet selle funktsiooni. Üle VNets või ühendusjooni kohapealse esitamisel on see funktsioon on minimaalne mõju üldise latentsuse.

[AZURE.INCLUDE [virtual-network-preview](../../includes/virtual-network-preview.md)]

##<a name="benefits"></a>Eelised

- **Lower latentsus / kõrgema paketid kohta teine (pps):** Funktsiooni Andmeteed virtual switch eemaldamine eemaldab aega kulub paketid host poliitika töötlemiseks ja paketid, mida saab töödelda sees VM arv suureneb.
- **Vähendatud jitter:** Virtuaalne lüliti töötlemine sõltub poliitika, mis tuleb haldurilt summa ja CPU, mis teeb töötlemise töökoormus. Selle hajuvuse mahalaadimine poliitika jõustamine riistvara abil pakkuda paketid otse VM, eemaldamise hosti VM suhtlus ja tarkvara katkestuste ja kontekstis parameetrid eemaldatakse.
- **Vähenemine Protsessori kasutuse:** Mööda virtuaalse sisse selle hosti viib vähem CPU kasutuse töötlemiseks võrguliiklust.

## <a name="limitations"></a>Piirangud

Selle funktsiooni kasutamisel olemas järgmised piirangud:
 
- **Network kasutajaliidese loomine:** Kiirendatud võrgunduse saab lubada ainult uue võrgu liidese.  See ei saa lubada olemasoleva võrgu liidesel.
- **VM loomine:** Võrgu liidese kiirendatud võrguühendusega saate manustada ainult VM VM loomisel. Võrgu liidese ei saa lisada mõne olemasoleva VM.
- **Regioonid:** Ainult Lääne keskse meile ja Lääne Euroopa Azure piirkondades saadaval. Piirkondade määramine laiendab tulevikus.
- **Toetatud operatsioonisüsteem:** Microsoft Windows Server 2012 R2 ja Windows Server 2016 tehnilise eelvaate 5. Linux ja Windows Server 2012 tugi lisatakse peagi.
- **VM suurus:** Standard_D15_v2 ja Standard_DS15_v2 on ainus toetatud VM eksemplari suurused. Lisateavet leiate artiklist [Windows VM suurused](../virtual-machines/virtual-machines-windows-sizes.md) . Toetatud VM eksemplari suuruses määramine laiendab tulevikus.

Need piirangud muudatustest avaldatakse [värskenduste Azure virtuaalse Networking](https://azure.microsoft.com/updates/accelerated-networking-in-preview) lehe kaudu.

## <a name="create-a-windows-vm-with-accelerated-networking"></a>Windowsiga VM kiirendatud võrguühendusega loomine

1. Registrisse Preview [Kiirendada Networking tellimused](mailto:axnpreview@microsoft.com?subject=Request%20to%20enable%20subscription%20%3csubscription%20id%3e) oma tellimuse ID ja mõeldud meilisõnumi saatmine. Täitke ülejäänud juhised kuni, kui olete saanud e-posti sõnumiga, kas te olete aktsepteerinud eelvaate sisse.
2. Azure'i portaali aadressil http://portal.azure.com Logi sisse.
3. Luua VM juhiseid [loomine Windowsi VM](../virtual-machines/virtual-machines-windows-hero-tutorial.md) artiklist valida järgmised suvandid:
    - Valige operatsioonisüsteem on loetletud käesoleva artikli jaotises piirangud.
    - Valige asukoht (piirkond) loetletud käesoleva artikli jaotises piirangud.
    - Valige loendis selle artikli jaotises piirangud VM suurus. Kui üks toetatud suuruses pole loendis, klõpsake nuppu **Kuva kõik** , **Valige suurus** tera suurus laiendatud loendist valimiseks.
    - Tera **sätted** , klõpsake *lubatud* **kiirendatud võrgunduse**, nagu on näidatud järgmisel pildil.

        ![Sätted](./media/virtual-network-accelerated-networking-portal/image3.png)

    >[AZURE.NOTE] Kiirendada Networking suvand on ainult siis, kui olete:
    >
    >- Eelvaate sisse võetud
    >- Valitud toetatud operatsioonisüsteemide, asukoht ja VM suurused käesoleva artikli jaotises piirangud.

5. Kui VM on loodud, laadige alla [kiirendada Networking draiveri](https://gallery.technet.microsoft.com/Azure-Accelerated-471b5d84), ühenduse ja VM ja Käivita draiveri installiprogrammi VM sees.
6. Paremklõpsake Windowsi nuppu ja klõpsake nuppu **Seadmehaldur**. Veenduge, et **Mellanox ConnectX-3 virtuaalse funktsioon Ethernet adapterit** kuvatakse jaotises **võrku** suvand, kui laiendatud, nagu on näidatud järgmisel pildil.

    ![Seadmehaldur](./media/virtual-network-accelerated-networking-portal/image2.png)