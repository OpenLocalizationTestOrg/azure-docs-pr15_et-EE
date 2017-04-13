<properties
    pageTitle="Luua koopia eriotstarbelisi VM Azure | Microsoft Azure'i"
    description="Saate teada, kuidas luua koopia eriotstarbelisi Windows VM ressursihaldur juurutamise mudeli Azure, töötab."
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
    ms.date="10/20/2016"
    ms.author="cynthn"/>
    
    
    
# <a name="create-a-copy-of-a-specialized-windows-vm-running-in-azure"></a>Eriotstarbelisi VM Windows Azure töötab koopia loomine 

Selles artiklis kirjeldatakse, kuidas luua koopia on VHD eriotstarbelisi Windows VM, kus töötab Azure AZCopy tööriista abil. Seejärel saate luua uue VM on VHD koopia. 

- Kui soovite kopeerida generalized VM, vaadake, [Kuidas luua VM pildi olemasoleva generalized Azure'i VM](virtual-machines-windows-capture-image.md).

- Kui soovite üles laadida on VHD kohapealse VM, nagu üks Hyper-V, vt [üleslaadimiseks Windows VHD on kohapealse VM Azure](virtual-machines-windows-upload-image.md)abil loodud.


## <a name="before-you-begin"></a>Enne alustamist

Veenduge, et saate:

- Olla teavet **lähte-kui ka salvestusruumi kontod**. Allika VM, peate salvestusruumi konto ja container nimed. Tavaliselt saab container nime **vhds**. Samuti peate sihtkoha salvestusruumi konto. Kui teil pole veel üks, saate selle luua kas portaalis (**Rohkem teenuseid** > salvestusruumi Kontod > Lisa) või cmdlet-käsu [New-AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) abil. 

- On Azure [PowerShell 1.0](../powershell-install-configure.md) (või uuem versioon) installitud.

- Alla laadinud ja installinud [AzCopy tööriista](../storage/storage-use-azcopy.md). 


## <a name="deallocate-the-vm"></a>Deallocate VM

Deallocate VM, mis kopeerida VHD vabastada. 

- **Portaali**: klõpsake **virtuaalmasinates** > **myVM** > peatamine
- **PowerShelli**: `Stop-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM` deallocates nimega **myVM** sisse ressursi rühma **myResourceGroup**VM.

Azure'i portaalis VM **olek** muutub **peatamiseni** **peatamiseni (deallocated)**.


## <a name="get-the-storage-account-urls"></a>Salvestusruumi konto URL-ID hankimine

Peate URL-id lähte-kui ka salvestusruumi kontod. URL-ide välja näeb: `https://<storageaccount>.blob.core.windows.net/<containerName>/`. Kui teate juba salvestusruumi konto ja container nime, saate asendada lihtsalt sulgudesse luua oma URL-i teave. 

Saate Azure portaali või Azure PowerShelli saada URL:

- **Portaali**: klõpsake nuppu **rohkem teenuseid** > **salvestusruumi kontod**  >  <storage account> **plekid** ja VHD lähtefaili on tõenäoliselt **vhds** ümbrises. Klõpsake käsku **Atribuudid** ümbris ja kopeerida teksti pealkirjaga **URL-i**. Peate nii lähte-kui ka ümbriste URL-id. 

- **PowerShelli**: `Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"` saab teabe jaoks VM nimega **myVM** ressursi rühma **myResourceGroup**sisse. Otsige tulemuste saamiseks jaotises **salvestusruumi profiil** **Vhd Uri**. Esimene osa Uri on ümbris URL-i ja selle Viimane osa on VM OS VHD nimi.

## <a name="get-the-storage-access-keys"></a>Kiirklahvide salvestusruumi hankimine

Otsimine kiirklahvide lähte-kui ka salvestusruumi kontod. Kiirklahvide kohta leiate lisateavet teemast [Azure kohta salvestusruumi kontod](../storage/storage-create-storage-account.md).

- **Portaali**: klõpsake nuppu **rohkem teenuseid** > **salvestusruumi kontod**  >  <storage account> **Kõik sätted** > **kiirklahvide**. Kopeerige märgistatud **võti1**võti.
- **PowerShelli**: `Get-AzureRmStorageAccountKey -Name mystorageaccount -ResourceGroupName myResourceGroup` saab salvestusruumi võti salvestusruumi konto **mystorageaccount** ressursi rühma **myResourceGroup**. Kopeerige märgistatud **võti1**võti.


## <a name="copy-the-vhd"></a>Kopeerige soovitud VHD 

Saate kopeerida failide salvestusruumi konto abil AzCopy vahel. Sihtkoha ümbris, kui määratud ümbris pole olemas, see luuakse teie jaoks. 

AzCopy, avage käsuviip, kohalikust arvutist ja liikuge kausta, kuhu on installitud AzCopy. See on sarnane *C:\Program Files (x86) \Microsoft SDKs\Azure\AzCopy*. 

Kõik failid ümbris jooksul kopeerida, **kasutage/s** lüliti. See saab kopeerida OS VHD ja kõik andmed ketast, kui need on sama ümbrises. Selles näites kujutatakse kopeerige kõik failid container **mysourcecontainer** salvestusruumi konto **mysourcestorageaccount** container **mydestinationcontainer** **mydestinationstorageaccount** salvestusruumi kontole sisse. Asendage salvestusruumi kontod ja ümbriste nimesid oma. Asendage `<sourceStorageAccountKey1>` ja `<destinationStorageAccountKey1>` oma võtmed.

```
    AzCopy /Source:https://mysourcestorageaccount.blob.core.windows.net/mysourcecontainer `
        /Dest:https://mydestinationatorageaccount.blob.core.windows.net/mydestinationcontainer `
        /SourceKey:<sourceStorageAccountKey1> /DestKey:<destinationStorageAccountKey1> /S
```

Kui soovite teatud VHD kopeerige ümbris mitme failiga, saate määrata ka /Pattern abil faili nime. Selle näite puhul kopeeritakse ainult fail nimega **myFileName.vhd** .

```
    AzCopy /Source:https://mysourcestorageaccount.blob.core.windows.net/mysourcecontainer `
        /Dest:https://mydestinationatorageaccount.blob.core.windows.net/mydestinationcontainer `
        /SourceKey:<sourceStorageAccountKey1> /DestKey:<destinationStorageAccountKey1> `
        /Pattern:myFileName.vhd
```


Kui olete lõpetanud, kuvatakse teade, mis näeb välja umbes:

```
  Finished 2 of total 2 file(s).
  [2016/10/07 17:37:41] Transfer summary:
  -----------------
  Total files transferred: 2
  Transfer successfully:   2
  Transfer skipped:        0
  Transfer failed:         0
  Elapsed time:            00.00:13:07
```

## <a name="troubleshooting"></a>Tõrkeotsing

- Kui kasutate AZCopy, kui kuvatakse tõrketeade "Server ei saa autentida kutse. Veenduge, et väärtus autoriseerimine päise moodustavad õigesti, sh allkirja." ja kasutate Key 2 või sekundaarne salvestusruumi klahvi, proovige kasutada, esmane või 1st salvestusruumi.


## <a name="next-steps"></a>Järgmised sammud

- Saate luua uue VM [VHD VM on OS ketas koopia](virtual-machines-windows-create-vm-specialized.md)manustamine.












