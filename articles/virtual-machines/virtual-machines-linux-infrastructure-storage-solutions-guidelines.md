<properties
    pageTitle="Salvestusruumi lahenduste juhised | Microsoft Azure'i"
    description="Teavet ja rakendamist suuniseid salvestusruumi lahenduste Azure taristu teenused kasutamise kohta."
    documentationCenter=""
    services="virtual-machines-linux"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="storage-infrastructure-guidelines"></a>Salvestusruumi taristu juhised

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)] 

See artikkel keskendub salvestusruumi vajadustele ja kujundamine optimaalse virtuaalse masina (VM) jõudluse saavutamiseks.


## <a name="implementation-guidelines-for-storage"></a>Salvestusruumi rakendamise juhised

Otsuseid:

- Kas vajate oma töökoormus Standard või Premium salvestusruumi kasutamiseks?
- Kas vajate kettale striping ketta 1023 GB mahukamate loomiseks?
- Kas vajate saavutamiseks oma töökoormus I/O optimaalse kettapuhastusriista striping?
- Millist komplekti salvestusruumi kontod on vaja majutada oma IT töökoormus või taristu?

Ülesanded:

- Vaadake üle I/O nõudmistele on juurutamine ja kavandamine sobiv arv ja tüüp salvestusruumi kontode rakendused.
- Saate luua salvestusruumi kontod abil oma nimetamistava määramine. Saate kasutada Azure CLI või portaali.


## <a name="storage"></a>Salvestusruumi

Azure'i salvestusruumi on oluline osa juurutamine ja virtuaalmasinates (VMs) ja rakenduste haldamine. Azure'i salvestusruumi osutab faili andmeid, struktureerimata andmed ja sõnumite talletamiseks ja see on ka toetavad VMs infrastruktuuri osa.

On kahte tüüpi salvestusruumi kontod toetavad VMs jaoks saadaval.

- Kindlad kontod annab teile juurdepääsu bloobimälu (kasutatakse salvestamiseks Azure VM ketast), tabelimälu, järjekorda salvestusruumi ja failisalvestusruumi.
- [Premium salvestusruumi](../storage/storage-premium-storage.md) kontod esitamisel suure jõudlusega, latentsusajaga ketta I/O intensiivse teenustest, nt MongoDB Sharded kobar tugi. Premium mälu toetab praegu ainult Azure VM ketast.

Azure'i loob VMs on operatsioonisüsteem ketas, ajutised kettale ja null või rohkem valikuline andmete ketast. Operatsioonisüsteemi ketta ja andmete ketast on Azure lehe plekid, ajutised ketas on talletatud kohalik sõlme, kus asub masina. Olge ettevaatlik kasutama ainult selle ajutine ketta püsiv andmete jaoks nagu VM võib viiakse vahel hosts hooldustööde sündmuse ajal rakenduste kujundamine. Kaoks ajutine kettale salvestatud andmed.

Aluseks oleva Azure Storage keskkonna tagada teie andmete eest kaitsta planeerimata hooldustööd või riistvara tõrgete on tagatud kestvus ja kõrge kättesaadavus. Azure Storage keskkonna kujundamisel, saate valida VM salvestusruumi korrata.

- Kohalik jooksul antud Azure'i andmekeskusega
- Azure'i andmekeskuste antud regioonis üle
- kogu Azure andmekeskuste erinevate piirkondade lõikes.

Lugege [rohkem kõrge-saadavus dispersioonanalüüs võimaluste kohta](../storage/storage-introduction.md#replication-for-durability-and-high-availability).

Operatsioonisüsteemi ketast ja andmete ketast on maksimaalne maht 1023 gigabaiti (GB). Mõne bloobimälu maksimaalne maht on 1024 GB ja mis peavad sisaldama metaandmeid (jalusesse) VHD faili (GB on 1024<sup>3</sup> baiti). Loogika helitugevuse Manager (LVM) abil saate selle piirmäära ületavad läbi viia andmete ketast esitada loogiline mahud 1023GB mahukamate oma VM.

On mõned piirangud skaleeritavus oma Azure Storage juurutuste kujundamisel - leiate [Microsoft Azure'i tellimus ja teenuste piirangud, kvootide ja piiranguid](azure-subscription-service-limits.md#storage-limits) rohkem üksikasju. Vt ka [Azure storage skaleeritavus ja jõudluse sihtkohtade](../storage/storage-scalability-targets.md).

Rakenduse Storage, saate salvestada objekti struktureerimata andmed näiteks dokumentide, piltide, varukoopiate, konfiguratsiooni andmed, logid jne. bloobimälu abil. Manustatud VM virtuaalse kettale kirjutamisel rakenduse asemel rakenduse saab kirjutada otse Azure'i bloobimälu. Bloobimälu ka pakub võimalust [kuum ja lahedad storage astme](../storage/storage-blob-storage-tiers.md) sõltuvalt teie vajadustele kättesaadavus ja maksumus piiranguid.


## <a name="striped-disks"></a>Musta Triibutatud ketast
Peale, mis võimaldab teil luua ketast suurem kui 1023 GB paljudel juhtudel striping kasutamise andmete ketast suurendab jõudlust, lubades mitme plekid tagasi salvestusruumi ühe maht. Striping, kus on nõutav kirjutada ja lugeda andmeid ühe loogilise ketta I/O tulu samal ajal.

Azure'i paneb piirangud andmete ketast arv ja läbilaskevõimet sõltuvalt VM suurus saadaval. Lisateavet leiate teemast [virtuaalmasinates suurused](virtual-machines-linux-sizes.md).

Kui kasutate ketta striping Azure'i andmed ketast, kaaluge järgmisi juhiseid.

- Andmete ketast tuleb alati maksimummaht (1023 GB).
- Manustada kuni andmete ketast, mis on lubatud maht VM.
- Kasutage LVM.
- Vältige Azure'i andmed kettale vahemällu salvestamise suvandid (vahemällu poliitika = puudub).

Lisateabe saamiseks vt [Konfigureerimise LVM Linux VM](virtual-machines-linux-configure-lvm.md).


## <a name="multiple-storage-accounts"></a>Mitme salvestusruumi kontod

Azure Storage keskkonna kujundamisel saate kasutada mitut salvestusruumi kontod VMs arv suureneb juurutamist. Seda moodust abil jaotada välja soovitud I/O kogu aluseks Azure Storage infrastruktuuri säilitada optimaalse jõudluse VMs ja rakendused. Kujundamisel juurutate rakendusi, kaaluge iga VM on I/O nõuetele ja need välja Saldo Azure Storage kontode vahel. Proovige vältida rühmitamise kõik kõrge I/O nõuavad VMs ainult ühe või kahe salvestusruumi kontod.

Lisateavet/o võimalusi eri Azure Storage võimaluste ja mõned soovitame üsnagi, leiate [Azure'i salvestusruumi skaleeritavus ja jõudluse sihtkohtade](../storage/storage-scalability-targets.md).


## <a name="next-steps"></a>Järgmised sammud

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)] 