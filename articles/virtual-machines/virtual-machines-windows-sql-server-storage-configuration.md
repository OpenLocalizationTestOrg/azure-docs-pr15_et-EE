<properties
    pageTitle="Salvestusruumi konfiguratsiooni SQL serveri vms | Microsoft Azure'i"
    description="Selles teemas kirjeldatakse, kuidas Azure'i konfigureerib salvestusruumi SQL serveri vms ajal ettevalmistusmudel (ressursihaldur juurutamise). Samuti selgitatakse, kuidas seadistada salvestusruumi oma olemasoleva SQL serveri VMs."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="ninarn"
    manager="jhubbard"    
    tags="azure-resource-manager"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/04/2016"
    ms.author="ninarn" />

# <a name="storage-configuration-for-sql-server-vms"></a>SQL serveri vms salvestusruumi konfigureerimine

Kui konfigureerite Azure SQL serveri virtuaalse masina pilt, aitab automatiseerimiseks mäluruumi konfiguratsioonist portaali. See hõlmab manustamise salvestusruumi VM, muuta selle salvestusruumi kättesaadavaks SQL serveri ja seadistamine optimeerida oma konkreetsete tulemuslikkuse nõuetele.

See teema selgitab, kuidas Azure'i konfigureerib mäluruumi teie SQL serveri vms ettevalmistamise nii jooksul olemasoleva vms. Selle konfiguratsiooni põhjal [jõudluse head tavad](virtual-machines-windows-sql-performance.md) töötab SQL Server Azure'i vms.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]klassikaline juurutamise mudel.

## <a name="prerequisites"></a>Eeltingimused
Saate kasutada automatiseeritud salvestamise konfiguratsiooni sätteid, virtuaalse arvuti nõuab järgmised omadused:

- [SQL serveri Galerii](virtual-machines-windows-sql-server-iaas-overview.md#option-1-deploy-a-sql-vm-per-minute-licensing)on ette valmistatud.
- Kasutab [ressursihaldur juurutamise mudel](../resource-manager-deployment-model.md).
- Kasutab [Premium salvestusruumi](../storage/storage-premium-storage.md).

## <a name="new-vms"></a>Uue VMs
Järgmistes jaotistes kirjeldatakse, kuidas konfigureerida uue SQL serveri virtuaalmasinates salvestusruum.

### <a name="azure-portal"></a>Azure'i portaal
Kui ettevalmistamise Azure VM, mis on SQL serveri galerii abil, saate oma uut VM talletamist automaatne konfigureerimine. Määrake mälumaht, tulemuslikkuse ja töökoormus tüüp. Järgmine pilt kuvatakse salvestusruumi konfiguratsiooni saelehe ajal SQL-i VM ettevalmistamise.

![SQL serveri VM salvestusruumi konfiguratsiooni ettevalmistamise ajal](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration-provisioning.png)

Vastavalt teie valikud, sooritab Azure'i VM loomise järel salvestusruumi konfiguratsiooni järgmist.

- Loob ja manustab premium salvestusruumi andmete ketast virtuaalse masina.
- Konfigureerib juurdepääs SQL serveri andmetega ketast.
- Konfigureerib ketast andmed üheks talletusmahtu, mis on määratud maht ja jõudlus (IOPS ja läbilaskevõime) vajaduste järgi.
- Funktsiooni talletusmahtu seostab uus draiv virtual arvutisse.
- Optimeerib selle uus draiv vastavalt teie määratud töökoormus tüüp (võrgupõhised andmebaasid tehinguotsust töötlemine või üldine).

Leiate täpsemat teavet, kuidas Azure'i konfigureerib talletusmahu [salvestusruumi konfigureerimise jaotisest](#storage-configuration). Teemast Azure portaali loomine SQL serveri VM täielik lühiülevaade [ettevalmistamise õpetuse](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="resource-manage-templates"></a>Ressursside haldamine Mallid
Kui kasutate ressursihaldur järgmised Mallid, kaks premium andmete plaati on lisatud vaikimisi pole salvestusruumi rakenduskausta konfiguratsioon. Siiski saate kohandada need Mallid premium andmete ketast, mis on lisatud virtuaalse masina numbri muutmiseks.

- [Automaatse varundamise VM loomine](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-autobackup)
- [Luua VM automatiseeritud lappimine](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-autopatching)
- [Looge VM AKV integratsiooniga](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-keyvault)

## <a name="existing-vms"></a>Olemasoleva VMs
Olemasoleva SQL serveri vms, saate muuta salvestusruumi sätete Azure'i portaalis. Valige oma VM, avage sätted ja valige SQL Server Configuration. SQL Server Configuration tera kuvatakse praeguse salvestusruumi kasutuse oma VM. Sellel diagrammil kuvatakse kõik draivid, mis on olemas teie VM. Iga draivi salvestusruumi kuvab neli jaotised:

- SQL-i andmete
- SQL-i Logi
- Muu (mitte-SQL-i salvestusruumi)
- Saadaval

![Olemasoleva SQL serveri VM salvestusruumi konfigureerimine](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration-existing.png)

Konfigureerida salvestusruumi lisada uus draiv või mõne olemasoleva draivi laiendada, klõpsake linki Redigeeri diagrammi kohal.

Konfiguratsiooni suvandid, mida näete, sõltub sellest, kas olete kasutanud enne seda funktsiooni. Esmakordsel kasutamisel saate määrata oma salvestusruumi nõuded uus draiv. Kui olete varem kasutanud funktsiooni ketta loomiseks, saate laiendada, et juhtida salvestusruumi.

### <a name="use-for-the-first-time"></a>Kasuta esimest korda
Kui see on selle funktsiooni kasutamine esimest korda, saate määrata uus draiv talletusruumi maht ja jõudlus piirangud. See kogemus on sarnane, mida soovite näha veebisaidil ettevalmistamise aeg. Peamine erinevus on see, et teil pole lubatud töökoormus määramiseks. See piirang hoiab katkestades kõiki olemasolevaid SQL serveri konfiguratsioone virtual arvutisse.

![SQL serveri talletusruumi liugureid konfigureerimine](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-usage-sliders.png)

Azure'i loob uue ketta vastavalt teie määratud tingimustele. Selle stsenaariumi korral sooritab Azure'i salvestusruumi konfiguratsiooni järgmist.

- Loob ja manustab premium salvestusruumi andmete ketast virtuaalse masina.
- Konfigureerib juurdepääs SQL serveri andmetega ketast.
- Konfigureerib ketast andmed üheks talletusmahtu, mis on määratud maht ja jõudlus (IOPS ja läbilaskevõime) vajaduste järgi.
- Funktsiooni talletusmahtu seostab uus draiv virtual arvutisse.

Leiate täpsemat teavet, kuidas Azure'i konfigureerib talletusmahu [salvestusruumi konfigureerimise jaotisest](#storage-configuration).

### <a name="add-a-new-drive"></a>Lisage uus draiv
Kui teil on juba loodud salvestusruumi oma SQL serveri VM, laiendamine salvestusruumi avab kaks uut võimalust. Esimene variant on lisada uus draiv, kus saate parandada jõudlust oma VM.

![SQL-i VM uus draiv lisamine](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration-add-new-drive.png)

Siiski pärast lisamist ketas, peate tegema mõned jõudluse suurendamine saavutamiseks lisatasu käsitsi konfigureerimine.

### <a name="extend-the-drive"></a>Ketas laiendamine
Teine võimalus laiendamise salvestusruumi on olemasoleva draivi laiendada. See suvand suurendab saadaolevat draivi, kuid see suurendada jõudlust. Koos salvestusruumi kaustu, ei saa muuta veergude arv soovitud talletusmahtu loomise järel. Veergude arv määratleb paralleelselt kirjutab, mille saate triibuline üle andmete ketast arvu. Seetõttu ei saa mis tahes lisatud andmed ketast suurendada jõudlust. Nad suudavad ainult andmed kirjutatakse rohkem ruumi. Seda piirangut on ka tähendab, et, kui ulatub ketas, määratleb veergude arv väikseima arvu andmete ketast, saate lisada. Kui loote mõnda talletusmahtu neli andmete ketast, veergude arv on ka neli. Igal ajal saate laiendada talletamist, peate lisama vähemalt neli andmete ketast.

![SQL-i VM kettal laiendamine](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-extend-a-drive.png)

## <a name="storage-configuration"></a>Salvestusruumi konfigureerimine
Selles jaotises antakse viide Azure'i sooritab automaatselt SQL-i VM ettevalmistamise või konfiguratsiooni Azure'i portaalis ajal salvestusruumi konfiguratsiooni muudatusi.

- Kui olete valinud vähem kui kaks TBs salvestusruumi oma VM, ei loo Azure on talletusmahtu.
- Kui olete valinud vähemalt kaks TBs salvestusruumi oma VM, konfigureerib Azure on talletusmahtu. Järgmises jaotises selle teema sisaldab salvestusruumi rakenduskausta konfiguratsiooni üksikasju.
- Automaatse salvestusruumi konfiguratsiooni alati kasutab [premium salvestusruumi](../storage/storage-premium-storage.md) P30 andmete ketast. Seega on 1:1 vastendamine teie valitud arv TB ja andmete ketast, mis on seotud teie VM arvu vahel.

Hindade teavet, [salvestusruumi hinnad](https://azure.microsoft.com/pricing/details/storage) lehelt leiate menüü **Kettaruumi** .

### <a name="creation-of-the-storage-pool"></a>Funktsiooni talletusmahtu loomine
Azure'i kasutab luua selle talletusmahtu SQL serveri VMs järgmisi sätteid.

| Säte | Väärtus |
|-----|-----|
| Triip suurus  | 256 KB (võrgupõhised andmebaasid); 64 KB (selgituseks) |
| Ketas suurused | 1 TB |
| Vahemälu | Lugemine |
| Jaotuse maht | 64 KB NTFS jaotuse ühiku maht |
| Kiirsõnumite faili lähtestamine | Lubatud |
| Lukusta lehtede mälu | Lubatud |
| Taastamine | Lihtne taaste (pole paindlikkust) |
| Veergude arvu. | Andmete ketast<sup>1</sup> arv |
| TempDB asukoht | Talletatud andmed ketast<sup>2</sup> |

<sup>1</sup> on talletusmahtu on loodud, ei saa muuta talletusmahtu soovitud veergude arv.

<sup>2</sup> see säte kehtib ainult esimese draivi salvestusruumi konfigureerimise funktsiooni abil loote.

## <a name="workload-optimization-settings"></a>Töökoormus optimeerimise sätted.
Järgmises tabelis kirjeldatakse kolme töökoormus tüüp suvandid saadaval ja nende vastavate optimeerimine:

| Töökoormus tüüp | Kirjeldus | Optimeerimine |
|-----|-----|-----|
| **Üldine** | Vaikesäte, mis toetab enamik töökoormus | Ükski |
| **Selgituseks töötlemine** | Optimeerib traditsiooniline andmebaasi OLTP töökoormus salvestusruum | Jälgi lipu 1117<br/>Jälgi lipu 1118 |
| **Andmete hoidmise** | Optimeerib analüütiline ja aruannete töökoormus salvestusruum | Jälgi lipu 610<br/>Jälgi lipu 1117 |

>[AZURE.NOTE] Saate määrata ainult töökoormus tüüp, kui olete ette SQL-i virtuaalse masina valides salvestusruumi konfiguratsioon samm.

## <a name="next-steps"></a>Järgmised sammud
Muud teemad, mis töötab SQL Server Azure'i VMs seotud, vt [SQL Server Azure'i Virtuaalmasinates](virtual-machines-windows-sql-server-iaas-overview.md).
