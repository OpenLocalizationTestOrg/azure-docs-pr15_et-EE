<properties
    pageTitle="SQL serveri andmebaasi migreerida SQL serveri VM | Microsoft Azure"
    description="Teave migreerimise asutusesisese kasutaja andmebaasi SQL serveri Azure'i virtuaalarvuti."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="sabotta"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="carlasab"/>


# <a name="migrate-a-sql-server-database-to-sql-server-in-an-azure-vm"></a>Azure VM SQL serveri SQL serveri andmebaasi migreerida

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]Ressursihaldur mudel.


On mitmeid meetodeid rändavad kohapeal SQL serveri kasutaja andmebaasi Azure VM SQL serveriga. Käesoleva artikli, lühidalt arutada erinevaid meetodeid, soovitan mõistlik erinevaid stsenaariume ja kaasata [juhendaja](#azure-vm-deployment-wizard-tutorial) juhendab teid **Deploy Microsoft Azure VM SQL serveri andmebaasi** viisardi abil. 

Kirjeldatud [juhendaja](#azure-vm-deployment-wizard-tutorial) **Deploy Microsoft Azure VM SQL serveri andmebaasi** viisardi kasutamise meetod on ainult klassikaline juurutamise mudeli. 

## <a name="what-are-the-primary-migration-methods"></a>Millised on esmased migreerimise meetodid?

Esmane migreerimise meetodid on:

- Kasutage SQL serveri andmebaasi viisard Microsoft Azure VM Deploy
- Kohapeal varukoopia abil compression ja käsitsi kopeerida varufaili Azure'i virtuaalarvuti
- Varundada URL URL ja Azure virtuaalne masin taastada
- Lahti ja seejärel kopeerige andmete ja logifailid eksperdirühma Azure'i bloobimälu ja seejärel lisada Azure VM SQL serveri URL:
- Teisendada füüsilist masinat kohapeal Hyper-V VHD, Azure bloobimälu laadida ja seejärel juurutada nagu uus VM kasutades üles laadida VHD
- Laeva kõvakettale Windows Import/eksport teenuse kasutamise
- Kui teil on asutusesisene AlwaysOn juurutamine, [Lisada Azure koopia viisardi](virtual-machines-windows-classic-sql-onprem-availability.md) abil saate luua koopiat Azure ja seejärel Tõrkesiirde, Azure Andmebaasieksemplari kasutajate viidates
- Abil konfigureerida Azure SQL serveri eksemplaris tellijana ja seejärel Keela replikatsiooni, osutades kasutajatele Azure'i Andmebaasieksemplari SQL serveri [selgituseks replikatsiooni](https://msdn.microsoft.com/library/ms151176.aspx)



## <a name="choosing-your-migration-method"></a>Valides oma migreerimise meetod

Optimaalse andmete edastamiskiirus sisserände tihendatud varukoopiafaili Azure VM andmebaasi failid on üldjuhul mõistlik. See on meetod, mis kasutab [Deploy SQL serveri andmebaasi viisard Microsoft Azure VM](#azure-vm-deployment-wizard-tutorial) . See viisard on soovitatav meetod rändavad asutusesisese kasutaja andmebaasi jooksvate SQL Server 2005 või SQL Server 2014 rohkem või rohkem kui tihendatud andmebaasi varukoopia on alla 1 TB.

Sentimeetritki ajal andmebaasi kasutama AlwaysOn võimalus või tiražeerimise valiku.

Kui see ei ole võimalik kasutada eespool nimetatud meetoditele, rändavad käsitsi andmebaasi. Seda meetodit kasutades saad üldiselt alustada järgneb koopia andmebaasi varukoopia ning andmebaasi varundada Azure'i sisse ja seejärel sooritada andmebaasi taastamiseks. Saate ka kopeerida failid ise Azure ja siis lisab. Seal mitmeid meetodeid, mille abil saab täita käsitsi protsessi sisse Azure VM andmebaasi migreerimine.

> [AZURE.NOTE] Kui täiendate SQL Server 2014 või SQL Server 2016 vanemate versioonide SQL serveri, siis peaks kaaluma, kas on vaja muuta. Me soovitame, et teil lahendada kõik sõltuvused funktsioone ei toeta SQL serveri uus versioon rände projekti. Toetatud väljaanded ja stsenaariumide kohta lisateabe saamiseks vaadake [versiooniks SQL Server](https://msdn.microsoft.com/library/bb677622.aspx).

Järgmises tabelis on ära toodud kõik esmane migreerimise meetodid ja arutab, kui iga meetodi kasutamine on kõige sobivam.

| Meetod  | Allikas versioon  |  Sihtkoha andmebaasi versioon | Allika andmebaasi varukoopia suuruse piirang  | Märkmed  |
|---|---|---|---|---|
| [Kasutage SQL serveri andmebaasi viisard Microsoft Azure VM Deploy](#azure-vm-deployment-wizard-tutorial) | SQL Server 2005 või suurem | SQL serveri 2014 või suurem | < 1 TB  | Kiireim ja Lihtsaim meetod, kasutamise võimaluse rännata uue või olemasoleva SQL serveri eksemplarile Azure'i virtuaalarvuti | 
| [Kasutamise ning lisada Azure koopia viisard](virtual-machines-windows-classic-sql-onprem-availability.md) | SQL Server 2012 või suurem | SQL Server 2012 või suurem | [Azure'i VM salvestuslimiidi](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | Vähendab seisakuid, kui teil on AlwaysOn asutusesisese juurutamise |
| [Kasutage SQL serveri selgituseks replikatsiooni](https://msdn.microsoft.com/library/ms151176.aspx) | SQL Server 2005 või suurem | SQL Server 2005 või suurem | [Azure'i VM salvestuslimiidi](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | Kasutada, kui teil on vaja vähendada seisakuid ja on AlwaysOn asutusesisese juurutamise |
| [Kohapeal varukoopia abil compression ja käsitsi kopeerida varufaili Azure'i virtuaalarvuti](#backup-to-file-and-copy-to-vm-and-restore) | SQL Server 2005 või suurem | SQL Server 2005 või suurem | [Azure'i VM salvestuslimiidi](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | Kasutada ainult siis, kui ei saa kasutada viisardit, näiteks kui sihtkoha andmebaasi versiooni on väiksem kui SQL Server 2012 SP1 CU2 või andmebaasi varukoopia maht on suurem kui 1 TB (12.8 TB SQL Server 2016) |
| [Varundada URL URL ja Azure virtuaalne masin taastada](#backup-to-url-and-restore) | SQL Server 2012 SP1 CU2 või suurem | SQL Server 2012 SP1 CU2 või suurem | < 12.8 TB SQL Server 2016, muidu < 1 TB | Üldiselt kasutades [backup URL](https://msdn.microsoft.com/library/dn435916.aspx) on samaväärse jõudluse viisardiga ja ei ole päris nii lihtne |
| [Lahti ja seejärel kopeerige andmete ja logifailid eksperdirühma Azure'i bloobimälu ja seejärel lisada SQL Server Azure'i virtuaalarvuti URL](#detach-and-copy-to-url-and-attach-from-url) | SQL Server 2005 või suurem | SQL serveri 2014 või suurem | [Azure'i VM salvestuslimiidi](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | Kasutage seda meetodit, kui soovite [salvestada failide Azure'i bloobimälu talletusteenuse](https://msdn.microsoft.com/library/dn385720.aspx) ja lisab selle SQL Server töötab Azure VM, eriti väga suurte andmebaasidega |
| [Kohapeal masin teisendada Hyper-V VHDs, Azure bloobimälu laadida ja seejärel juurutada uus virtuaalne masin, kasutades üles laaditud VHD](#convert-to-vm-and-upload-to-url-and-deploy-as-new-vm) | SQL Server 2005 või suurem | SQL Server 2005 või suurem | [Azure'i VM salvestuslimiidi](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | [Tuues oma SQL serveri litsentsi](../sql-database/sql-database-paas-vs-sql-server-iaas.md), kui rändavad andmebaas, mis teil töötab SQL serveri või vanem versioon kui rändavad süsteemi ja kasutaja andmebaasid koos osana rände sõltub teiste kasutaja andmebaasid ja/või süsteemi andmebaasid andmebaasi kasutamiseks. |
| [Laeva kõvakettale Windows Import/eksport teenuse kasutamise](#ship-hard-drive) | SQL Server 2005 või suurem | SQL Server 2005 või suurem | [Azure'i VM salvestuslimiidi](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | [Windows Import/eksport teenuseid](../storage/storage-import-export-service.md) kasutada, kui käsitsi koopia on liiga aeglane, näiteks väga suurte andmebaasidega |

## <a name="azure-vm-deployment-wizard-tutorial"></a>Azure'i VM juurutamine viisard juhendaja

**Deploy Microsoft Azure VM SQL serveri andmebaasi** viisardi abil Microsoft SQL Server Management Studio migreerida SQL Server 2005, SQL Server 2008, SQL Server 2008 R2, SQL Server 2012, SQL Server 2014 või SQL Server 2016 asutusesisese kasutaja andmebaasi (kuni 1 TB) SQL Server 2014 või SQL Server 2016 Azure'i virtuaalarvuti. Selle viisardi abil saate migreerida olemasolevate Azure'i virtuaalarvuti või Azure VM SQL serveri loodud kasutaja andmebaasi viisardi siirdeprotsessi käigus. Kui migreerite SQL serveri versiooni andmebaasi, andmebaasi käigus automaatselt uuendada.

Meetod on ainult klassikaline juurutamise mudeli. 

### <a name="get-latest-version-of-the-deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard"></a>Too uusim versioon Deploy SQL serveri andmebaasi viisard Microsoft Azure VM

Uusima versiooni Microsoft SQL Server Management Studio SQL serveri abil saate tagada, et teil on uusim versioon **Deploy Microsoft Azure VM SQL serveri andmebaasi** viisard. See viisard uusim versioon sisaldab classic Azure portaali uusimad värskendused ja toetab uusim Azure VM Piltide galerii (viisard vanemad versioonid ei pruugi). Et saada uusima versiooni Microsoft SQL Server Management Studio SQL Server, [alla laadida](http://go.microsoft.com/fwlink/?LinkId=616025) ja installida kliendi arvutis andmebaasi, mida soovite rändavad ja Interneti-ühenduse kaudu.

### <a name="configure-the-existing-azure-virtual-machine-and-sql-server-instance-if-applicable"></a>Olemasoleva Azure'i virtuaalarvuti ja SQL Serveri eksemplari konfigureerimine (vajadusel)

Kui migreerite olemasolevate Azure VM, nõutakse konfiguratsioon järgmist:

- Konfigureerida Azure VM ja SQL Serveri eksemplari lubama ühenduse teise arvuti ühendamiseks [SQL serveri VM eksemplari teises arvutis SSMS](virtual-machines-windows-sql-connect.md)juhiseid järgides. Kui migreerite viisardiga toetatakse ainult SQL Server 2014 ja SQL Server 2016 pilte galeriis.
- Konfigureerida Microsoft Azure lüüsi 11435 avalikes Port avatud SQL serveri pilve Adapter teenuse lõpp-punkt. Selle pordi luuakse Microsoft Azure VM SQL Server 2014 või SQL Server 2016 kohustuste osana. Pilve Adapter loob ka Windowsi tulemüüri reegel, et lubada TCP oma sissetulevate ühenduste vaikimisi port 11435. Selle näitaja võimaldab kasutada pilve adapter teenuse asutusesisese eksemplar backup failide kopeerimine Azure VM viisardi. Lisateabe saamiseks vt [Pilve Adapter SQL Server](https://msdn.microsoft.com/library/dn169301.aspx).

    ![Luua pilve Adapter lõpp-punkti](./media/virtual-machines-windows-migrate-sql/cloud-adapter-endpoint.png)

### <a name="run-the-use-the-deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard"></a>Käivitage kasutamise Deploy SQL serveri andmebaasi viisard Microsoft Azure VM

1. Avage Microsoft SQL Server Management Studio Microsoft SQL Server 2016 ja ühendada mis sisaldab kasutaja andmebaas, mis teil ei rändavad Azure VM SQL serveri eksemplaris.
2. Paremklõpsake Andmebaasi migreerimist, toimingud ja seejärel klõpsake Deploy Microsoft Azure VM.

    ![Viisardi käivitamine](./media/virtual-machines-windows-migrate-sql/start-wizard.png)

3. Tutvustus lehel nuppu edasi.
4. Lehel allikas sätted ühendada sisaldav andmebaas, mida te kavatsete rännata Azure VM SQL serveri eksemplaris.
5. Määrama varukoopiafailide ajutises asukohas. Kui ühendate serveritega, määrake võrgudraivi.

    ![Andmeallikate sätted](./media/virtual-machines-windows-migrate-sql/source-settings.png)

6. Klõpsake nuppu edasi.
7. Klõpsake lehel Microsoft Azure sisselogimine Logi sisse ja Azure'i kontole sisse logima.
8. Valige tellimus, mida soovite kasutada ja klõpsake nuppu edasi.

    ![Azure sisselogimine](./media/virtual-machines-windows-migrate-sql/azure-signin.png)

9. Juurutamise sätete lehe, saate määrata uue või olemasoleva pilve teenuse nimi ja nimi virtuaalne masin:

 - Määrake uus pilve teenuse nimi ja virtuaalarvuti nimi kasutades SQL Server 2014 või SQL Server 2016 Galerii pilt uue Azure'i virtuaalarvuti luua uus pilveteenus.

     - Kui määrate pilveteenus uus nimi, määrake storage konto, mida kasutate.

     - Kui määrate olemasoleva pilve teenuse nimi, välja otsitud storage konto ja teie eest juba sisestanud.

 - Määrake olemasoleva pilveteenus nimi ja uus virtuaalarvuti nimi uue Azure'i virtuaalarvuti luua olemasoleva pilve teenuse. Määrake ainult Galeriipildi SQL Server 2014 või SQL Server 2016.
 - Määrake olemasoleva pilve teenuse nimi ja virtuaalarvuti nimi olemasoleva Azure'i virtuaalarvuti. See peab kasutavate SQL Server 2014 või SQL Server 2016 Galeriipildi pilt.

        ![Deployment Settings](./media/virtual-machines-windows-migrate-sql/deployment-settings.png)

10. Klõpsake sätted
  - Kui olete valinud olemasoleva pilve teenuse nimi ja nimi virtuaalne masin, küsitakse teilt kasutajanime ja parooli.

        ![Azure seadme määrangute](./media/virtual-machines-windows-migrate-sql/azure-machine-settings.png)

    - Kui olete valinud uue virtuaalarvuti nime, küsitakse teilt, kas soovite valige pildi Galerii pilte ja järgmist teavet:
      - Piltide – valige ainult SQL Server 2014 või SQL Server 2016
        - Kasutajanimi
        - Uus parool
        - Kinnitage parool
        - Asukoht
        - Suurus.
    - Lisaks klõpsata vastu eneseregulatsiooni genereeritud tunnistuse jaoks see uus Microsoft Azure virtuaalne masin ja seejärel klõpsake nuppu OK.

    ![Azure'i uue masina sätteid](./media/virtual-machines-windows-migrate-sql/azure-new-machine-settings.png)

11. Määrata sihtkoha andmebaasi nimi, kui see erineb allika andmebaasi nimi. Kui sihtandmebaas on olemas, süsteemi näidatakse automaatselt juurdekasvu andmebaasi nime asemel kirjuta üle olemasolev andmebaas.
12. Klõpsake nuppu edasi ja seejärel nuppu valmis.

    ![Tulemused](./media/virtual-machines-windows-migrate-sql/results.png)

13. Kui viisard on lõpule jõudnud, ühendust virtuaalarvuti ja veenduge, et andmebaasi on siiratud.
14. Kui olete loonud uue virtuaalarvuti, konfigureerida Azure virtuaalarvuti ja SQL Serveri eksemplari [teises arvutis SSMS SQL serveri VM astme](virtual-machines-windows-sql-connect.md)ühenduse loomiseks juhiseid järgides.

## <a name="backup-to-file-and-copy-to-vm-and-restore"></a>Backup faili ja Kopeeri VM ja taastamine

Kasutage seda meetodit siis, kui SQL serveri andmebaasi viisard Microsoft Azure VM Deploy kas versiooni SQL serveri SQL Server 2014 enne migreerimist või teie varufail on suurem kui 1 TB. Kui teie varufail on suurem kui 1 TB, peab see triip, sest VM ketta maksimaalne suurus on 1 TB. Kasutaja andmebaasi käsitsi selle meetodi siirdamiseks kasutada alltoodud peamistel etappidel:

1.  Täita täieliku andmebaasi varukoopia asukohta kohapeal.
2.  Luua või upload virtuaalarvuti soovitud SQL serveri versiooni.
3.  Saate häälestada ühenduse oma vajaduste järgi. Vt [ühendamine SQL serveri virtuaalarvuti Azure (ressursihaldur)](virtual-machines-windows-sql-connect.md).
4.  Backup failid kopeerida oma VM kaugtöölaua, Windows Explorer või käsuviiba käsku Kopeeri.

## <a name="backup-to-url-and-restore"></a>URL ja Taasta varukoopia

Kasutage [URL backup](https://msdn.microsoft.com/library/dn435916.aspx) meetodi kui Deploy SQL serveri andmebaasi Microsoft Azure VM viisardit ei saa kasutada, kuna faili varundusfaili on suurem kui 1 TB ja migreerimist: ja SQL Server 2016. Andmebaasid, mis on väiksem kui 1 TB või versiooni SQL serveri SQL Server 2016 enne, soovitatakse kasutada viisardi. SQL Server 2016, triibuline varunduskomplekti ei toetatud, on soovitatav tulemused ja kohustatud ületada mahupiirangud kohta Kämp. Väga suuri andmebaase, soovitatakse [Windows Import/eksport teenuse](../storage/storage-import-export-service.md) kasutamisega.

## <a name="detach-and-copy-to-url-and-attach-from-url"></a>Lahti ja Kopeeri URL ja lisada URL:

Kasutage seda meetodit, kui soovite [salvestada failide Azure'i bloobimälu talletusteenuse](https://msdn.microsoft.com/library/dn385720.aspx) ja lisab selle SQL Server töötab Azure VM, eriti väga suurte andmebaasidega. Kasutaja andmebaasi käsitsi selle meetodi siirdamiseks kasutada alltoodud peamistel etappidel:

1.  Eemaldage Andmebaasieksemplari asutusesisese andmebaasi faile.
2.  [AZCopy käsurea utiliidi](../storage/storage-use-azcopy.md)kasutamine Azure bloobimälu eraldiseisva andmebaasi faile kopeerida.
3.  Manusena lisada Azure URL Andmebaasifailid Azure VM SQL Serveri eksemplariga.

## <a name="convert-to-vm-and-upload-to-url-and-deploy-as-new-vm"></a>Teisendada VM ja upload URL ja juurutada nagu uus VM

Selle meetodi rännata kõik süsteemi ja kasutaja andmebaasid SQL Serveri eksemplari kohapeal Azure'i virtuaalarvuti. Kogu SQL Serveri eksemplari käsitsi selle meetodi siirdamiseks kasutada alltoodud peamistel etappidel:

1.  Teisendada füüsilise või virtuaalse masinaid Hyper-V VHDs [Microsoft Virtual Machine Converter](http://technet.microsoft.com/library/dn873998.aspx)abil.
2.  VHD failide üleslaadimiseks Azure Storage [Lisa-AzureVHD cmdlet-käsu](https://msdn.microsoft.com/library/windowsazure/dn495173.aspx)abil.
3.  Üles laaditud VHD juurutada uus virtuaalne masin.

> [AZURE.NOTE] Rännata kogu taotluse, võite kasutada [Azure saidi taastamise](../site-recovery/site-recovery-overview.md)].

## <a name="ship-hard-drive"></a>Laeva kõvaketas

Kasutage [Windows impordi/ekspordi meetod](../storage/storage-import-export-service.md) faili andmete ülekandmiseks Azure bloobimälu, kus võrgu kaudu üleslaadimine on liiga kulukas või ei ole teostatav. Selle teenuse abil saata ühe või mitme Azure'i andmekeskuse, kus teie andmed laaditakse storage konto teabe sisaldavate kõvakettad.

## <a name="next-steps"></a>Järgmised sammud

Töötab SQL serveri Azure'i virtuaalarvutite kohta lisateabe saamiseks vaadake [SQL serveri Azure'i virtuaalarvutite ülevaade](virtual-machines-windows-sql-server-iaas-overview.md).

Juhised loomine Azure SQL serveri virtuaalarvuti hõivatud kujutist, vt CSS SQL Server insenerid blogi [Tips & Tricks "kloonimine" Azure SQL virtuaalseid masinaid salvestamisel](https://blogs.msdn.microsoft.com/psssql/2016/07/06/tips-tricks-on-cloning-azure-sql-virtual-machines-from-captured-images/) .
