<properties
    pageTitle="Luua mõne VM kättesaadavus | Microsoft Azure'i"
    description="Saate teada, kuidas luua saadavus määramine teie virtuaalmasinates Azure portaali või ressursihaldur juurutamise näidise PowerShelli abil."
    keywords="kättesaadavus määramine"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>
<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="cynthn"/>


# <a name="create-an-availability-set"></a>Luua mõne kättesaadavus 

Kui abil portaali, kui soovite oma VM saadavus osa, peate looma esmalt seadmine kättesaadavus.

Loomine ja kättesaadavus komplekti kasutamise kohta leiate lisateavet teemast [virtuaalmasinates olemasolu](virtual-machines-windows-manage-availability.md).


## <a name="use-the-portal-to-create-an-availability-set-before-creating-your-vms"></a>Portaali abil saate luua saadavus seadmine enne oma VMs loomine

1. Jaoturi menüü, klõpsake nuppu **Sirvi** ja valige **kättesaadavus määrab**.

2. **Kättesaadavus määrab tera**klõpsake nuppu **Lisa**.

    ![Määrake kuvatõmmis, millel on kujutatud uus kättesaadavus loomiseks nuppu Lisa.](./media/virtual-machines-windows-create-availability-set/add-availability-set.png)

3. Tehke enne **loomine kättesaadavus** teie seatud lisateavet.

    ![Kuvatõmmis, kus kuvatakse teave peate sisestama kättesaadavus hulga loomiseks.](./media/virtual-machines-windows-create-availability-set/create-availability-set.png)

    - **Nimi** – nimi peab olema 1-80 numbrid, tähed, perioodid, allakriipsutatud märkideks ja kriipsud. Esimene märk peab olema tähe või numbri. Viimane märk peab olema kirjas, arvu või allkriips.
    - **Viga domeenide** - viga domeenide määratlemine virtuaalmasinates levinud power lähte- ja võrgu vahetamise jagavad rühma. Vaikimisi VMs eraldatakse kuni kolm viga domeenides ja saab muuta vahemikus 1 – 3.
    - **Domeenide värskendamine** - domeenid on määratud vaikimisi ja see viis update saate seada vahemikus 1 kuni 20. Värskenduse domeenide näitavad virtuaalmasinates ja aluseks füüsilise riistvara, mis saab korraga taaskäivitama. Näiteks kui me määrata viis värskendamiseks domeenid, kui vähemalt viis virtuaalmasinates on konfigureeritud ühele kättesaadavus määramine, kuuenda virtuaalse masina paigutatakse sisse sama Värskenda Domeen esimese virtuaalse masina sama UD virtual teise arvutisse, nagu seitsmes ja nii edasi. Selle taaskäivitamisega järjestuse ei pruugi järjestikku, kuid korraga kuvatakse ainult üks update domain taaskäivitama.
    - **Tellimuse** – valige kasutada, kui teil on mitu tellimust.
    - **Ressursirühm** - ressursside olemasolevasse rühma valige klõpsake noolt ja valige rippmenüüst kaudu ressursirühma alla. Samuti saate luua uue ressursirühma nimi otsinguväljale. Nimi võib sisaldada järgmisi märke: tähtede, numbrite, perioodid, kriipsjooned, allakriipsutatud märkideks ja avamine või paremsulg. Nime ei saa perioodi lõpus. Kõik VMs kättesaadavus rühma vaja luua sama nimega kättesaadavus määramine ressursirühma.
    - **Asukoht** – valimine ripploendist soovitud asukohta.

4. Kui olete valmis teabe, klõpsake nuppu **Loo**. Kui rühma kättesaadavus on loodud, näete seda loendis pärast värskendamist portaali.

## <a name="use-the-portal-to-create-a-virtual-machine-and-an-availability-set-at-the-same-time"></a>Portaali abil saate luua virtuaalse masina ja -saadavus korraga seadmine

Kui loote uue VM portaalis, saate luua uue kättesaadavus, seadmine VM ajal loote esimese VM määramine.

![Kuvatõmmis, millel on kujutatud uus kättesaadavus, seada, kui loote VM loomise protsess.](./media/virtual-machines-windows-create-availability-set/new-vm-avail-set.png)


## <a name="add-a-new-vm-to-an-existing-availability-set"></a>Uue VM lisamiseks mõne olemasoleva kättesaadavus määramine

Jaoks iga täiendava VM, mille loote mis kuuluvad määramine, veenduge, et luua sama **ressursirühm** ja seejärel valige olemasolev kättesaadavus seadmine sammus 3. 

![Kuvatõmmis, mis näitab, kuidas kasutada oma VM seadmine olemasoleva saadavus valimiseks.](./media/virtual-machines-windows-create-availability-set/add-vm-to-set.png)



## <a name="use-powershell-to-create-an-availability-set"></a>PowerShelli kasutamine saadavus loomiseks seadmine

Selles näites loob **Lääne USA** asukoha määramine **RMResGroup** Ressursirühma saadavus. See on vaja teha enne loomist esimese VM, mis on määramine.

    New-AzureRmAvailabilitySet -ResourceGroupName "RMResGroup" -Name "AvailabilitySet03" -Location "West US"
    
Lisateavet leiate teemast [New-AzureRmAvailabilitySet](https://msdn.microsoft.com/library/mt619453.aspx).


## <a name="troubleshooting"></a>Tõrkeotsing

- Kui loote VM, kui soovite kättesaadavus määramine pole portaalis ripploendist võib loomist erinevate ressursside rühma. Kui te ei tea ressursirühma jaoks Kättesaadavusoleku määramine, minge menüüsse jaoturi ja klõpsake nuppu Sirvi > kättesaadavus loendi kättesaadavus komplekti ja mis määrab ressursi rühmad nad kuuluvad.


## <a name="next-steps"></a>Järgmised sammud

Täiendava salvestusruumi lisamine oma VM, lisades [andmed kettale](virtual-machines-windows-attach-disk-portal.md).
