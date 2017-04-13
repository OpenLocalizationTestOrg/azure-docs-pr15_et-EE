<properties
    pageTitle="Ketast ja Linux vms VHDs kohta | Microsoft Azure'i"
    description="Lisateavet põhitõdesid ketast ja VHDs Linux virtuaalmasinates Azure jaoks."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/16/2016"
    ms.author="cynthn"/>

# <a name="about-disks-and-vhds-for-azure-virtual-machines"></a>Ketast ja VHDs jaoks Azure'i virtuaalmasinates kohta

Nii nagu mis tahes arvuti Azure'i virtuaalmasinates kasutada ketast kohana operatsioonisüsteemi, rakendused ja andmete talletamiseks. Kõik Azure'i virtuaalmasinates on vähemalt kaks plaati – Linux operatsioonisüsteemi kettal (puhul Linux VM) ja ajutine ketas. Operatsioonisüsteemi ketas on loodud pilt ja nii operatsioonisüsteemi ketas kui pilt on talletatud Azure storage konto tegelikult virtuaalse kõvaketta (VHDs). Virtuaalmasinates võib olla üks või enam andmete ketast, mis salvestatakse ka VHDs. See artikkel kehtib ka [Windowsi virtuaalmasinates](virtual-machines-windows-about-disks-vhds.md)jaoks saadaval.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

> [AZURE.NOTE] Kui teil on mõne hetke aega, aidake meil täiustada Azure Linux VM dokumentatsiooni võttes selle [kiire küsitluse](https://aka.ms/linuxdocsurvey) oma kogemusi. Iga vastus aitab aitavad teil oma töö valmis.

## <a name="operating-system-disk"></a>Operatsioonisüsteemi ketta

Iga virtuaalse masina on üks manustatud operatsioonisüsteemi ketas. See on registreeritud SATA ketas ja märgistatud vaikimisi c-ketas. See ketas on maksimaalne maht on 1023 gigabaiti (GB). 

##<a name="temporary-disk"></a>Ajutist

Teie jaoks automaatselt loodud ajutine ketas. Klõpsake Linux virtuaalmasinates, ketas on tavaliselt SDB ja on vormindatud ja paigaldatud /mnt/resource Azure Linux agent.

Ajutiste ketta muutub, virtuaalse masina suurusest. Lisateavet leiate teemast [suurused Linuxi](virtual-machines-linux-sizes.md).

>[AZURE.WARNING] Andmeid ei ajutine kettale salvestada. See pakub ajutist rakenduste ja protsesside ja on mõeldud ainult andmed nt lehe või Vaheta failid salvestada. 

Lisateavet selle kohta, kuidas Azure'i kasutab ajutist ketas, lugege teemat [mõistmine ajutine kettale klõpsake Microsoft Azure'i Virtuaalmasinates](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)

## <a name="data-disk"></a>Andmete kettale

Andmete ketta on VHD, on seotud virtuaalse masina salvestada rakenduse andmete või muid andmeid, mida soovite säilitada. Andmete ketast on registreeritud SCSI draivid ja on sildistatud tähega, mille valimisel.  Iga andmete kettal on maksimaalne maht on 1023 GB. Suuruse virtuaalse masina määratleb, kui palju andmeid ketast, saate lisada selle ja tüübi talletusruumi abil saate soovitud ketast majutada.

>[AZURE.NOTE] Virtuaalmasinates võimalusi kohta leiate lisateavet teemast [Linuxi suurused](virtual-machines-linux-sizes.md).

Azure'i loob mõne operatsioonisüsteemi ketta virtuaalse masina loomisel pilt. Kui kasutate pilt, mis sisaldab andmeid ketast, Azure'i ka loob andmete ketast loob virtuaalse masina. Vastasel korral saate lisada andmete ketast, kui olete loonud virtuaalse masina.

Saate lisada andmed ketast virtuaalse masina igal ajal **manustamise** virtuaalse masina kettale. Saate kasutada VHD, mille olete üles laaditud või kopeeritud salvestusruumi konto või mõni varem Azure'i loob teie jaoks. Seotud andmete kettal seostab VHD faili salvestusruumi kontolt virtuaalse masina, pannes selle VHD "rendilepingu" nii, et seda ei saa kustutada salvestusruumi samas see on lisatud.

## <a name="about-vhds"></a>VHDs kohta

VHDs, kasutatakse Azure on nimega lehe plekid standard- või premium salvestusruumi konto Azure .vhd failid. Lehe plekid kohta leiate üksikasjalikumat teavet teemast [mõistmine Blokeeri plekid ja lehe plekid](https://msdn.microsoft.com/library/ee691964.aspx). Premium salvestusruumi kohta leiate üksikasjalikumat teavet leiate teemast [Premium mälu: suure jõudlusega salvestusruumi Azure virtuaalse masina töökoormus](../storage/storage-premium-storage.md).

Azure'i toetab kõvaketta VHD vorming. Fikseeritud vorming näeb loogiline ketas välja lineaarses faili, nii, et selle ketta nihke X talletuskohaks oleval veebisaidil bloobimälu nihke X. Small jaluse lõpus olevat bloobimälu kirjeldatakse on VHD atribuute. Sageli kindlas vormingus raiskab ruumi, kuna enamik ketast on suur kasutamata vahemike neid. Siiski Azure'i salvestab .vhd faile vähe vormingut, nii, et saate samal ajal fikseeritud ja dünaamiline ketast eelised. Lisateavet leiate teemast [virtuaalse kõvaketta töötamise alustamine](https://technet.microsoft.com/library/dd979539.aspx).

Kõik .vhd failid Azure, mida soovite allikana abil saate luua ketast või pildid on kirjutuskaitstud. Kui loote kettale või pilt, teeb Azure'i .vhd failide koopiad. Koopiad võib olla kirjutuskaitstud või lugemis-ja-kirjutamine, olenevalt sellest, kuidas kasutada funktsiooni VHD.

Virtuaalse masina loomisel pilt loob Azure'i kettal virtual masina, mis on andmeallika .vhd faili koopia. Kaitsta juhuslikku kustutamist, asetab Azure'i rendilepingu allikas .vhd faili, mis saab luua pildi, on operatsioonisüsteem kettale või andmed kettale.

Enne andmeallika .vhd faili kustutamiseks peate eemaldada rendilepingu kustutades kettale või pilt. Kustutada .vhd faili, mis kasutab virtuaalse masina on operatsioonisüsteem ketas, võite kustutada virtuaalse masina, operatsioonisüsteemi ketas, ja .vhd lähtefail korraga kustutamine virtuaalse masina ja kustutada kõik seotud ketast. Siiski kustutamine .vhd faili, mis on allika andmete jaoks nõuab mitu toimingut kogumi tellige--eemaldada arvutist virtuaalne ketas, kustutada ketta ja seejärel kustutage fail .vhd.

>[AZURE.WARNING] Kui salvestusruumi allika .vhd faili kustutada või konto salvestusruumi kustutamine, Microsoft ei saa taastada andmed teie eest.


## <a name="troubleshooting"></a>Tõrkeotsing
[AZURE.INCLUDE [virtual-machines-linux-lunzero](../../includes/virtual-machines-linux-lunzero.md)]

## <a name="next-steps"></a>Järgmised sammud

-  [Manusta kettal](virtual-machines-linux-add-disk.md) oma VM jaoks täiendava salvestusruumi lisamiseks.
-  [Konfigureerimine tarkvara RAID](virtual-machines-linux-configure-raid.md) koondamise.
-  [Jäädvustada Linux virtuaalse masina](virtual-machines-linux-classic-capture-image.md) nii, et saate kiiresti juurutada täiendavaid VMs.


