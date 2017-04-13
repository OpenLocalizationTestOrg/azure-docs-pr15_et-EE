<properties
    pageTitle="SQL serveri virtuaalse masina ettevalmistamine | Microsoft Azure'i"
    description="Loomine ja luua ühenduse SQL serveri virtuaalse masina Azure'i portaalis. Selle õpetuse kasutab ressursihaldur režiimi."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    editor=""
    manager="jhubbard"
    tags="azure-resource-manager" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/21/2016"
    ms.author="jroth" />

# <a name="provision-a-sql-server-virtual-machine-in-the-azure-portal"></a>SQL Server Azure'i portaal virtuaalse masina ettevalmistamine

> [AZURE.SELECTOR]
- [Portaal](virtual-machines-windows-portal-sql-server-provision.md)
- [PowerShelli](virtual-machines-windows-ps-sql-create.md)

Õppeteema – lõpuni näidatakse, kuidas kasutada Azure portaali ettevalmistamise SQL Server töötab.

Azure virtuaalse masina (VM) Galerii sisaldab mitu pilti, mis sisaldavad Microsoft SQL Server. Vaid mõne hiireklõpsuga saate valida üks SQL-i VM piltide galeriist ja selle ette teie Azure keskkonnas.

Selles õppetükis saate küll:

- [Valige SQL-i VM Pilt galeriist](#select-a-sql-vm-image-from-the-gallery)
- [Konfigureerige ja VM loomine](#configure-the-vm)
- [Kaugtöölaua VM avamine](#open-the-vm-with-remote-desktop)
- [SQL serveriga kaugühenduse teel](#connect-to-sql-server-remotely)

## <a name="select-a-sql-vm-image-from-the-gallery"></a>Valige SQL-i VM Pilt galeriist

1. Logige sisse [Azure portaali](https://portal.azure.com) konto kaudu.

    >[AZURE.NOTE] Kui teil pole Azure'i konto, külastage [Azure'i tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/).

1. Azure'i portaalis, klõpsake nuppu **Uus**. Portaali avatakse **Uus** tera. SQL serveri VM ressursid on **Virtuaalmasinates** jaotises kuvatakse Marketplace'ist.

1. Klõpsake **Uus** labale **Virtuaalmasinates**.

1. Kõik saadaolevad piltide kuvamiseks klõpsake **Virtuaalmasinates** enne **näha kõiki** .

    ![Azure'i Virtuaalmasinates Blade](./media/virtual-machines-windows-portal-sql-server-provision/azure-compute-blade.png)

1. Klõpsake jaotises **andmebaasi serverid**, **SQL Server**. Peate **andmebaasi serverid**leidmiseks allapoole kerima. Vaadake üle SQL serveri saadaolevaid malle.

    ![Virtuaalne seadme Galerii SQL-i pildid](./media/virtual-machines-windows-portal-sql-server-provision/virtual-machine-gallery-sql-server.png)

1. Iga malli tuvastab SQL serveri versioon ja operatsioonisüsteem. Valige loendist üks neid pilte. Vaadake läbi üksikasjad tera, mida kirjeldatakse, virtuaalse masina pilt.

    >[AZURE.NOTE] SQL-i VM pilte lisada hulgilitsentsimise kulud SQL serveri minutis hinnad loote VM. On veel üks võimalus tuua – teie-omanik-litsents (BYOL) ja maksma ainult VM. Pildi nimed on eesliide {BYOL}. Selle suvandi kohta lisateabe saamiseks vt [SQL Server Azure'i Virtuaalmasinates alustamine](virtual-machines-windows-sql-server-iaas-overview.md).

1. Klõpsake jaotises **Valige juurutamise mudeli**, veenduge, et **Ressursihaldur** on valitud. Ressursihaldur on soovitatav juurutamise mudeli uue virtuaalmasinates. Klõpsake nuppu **Loo**.

    ![Luua VM SQL-i ressursihaldur abil](./media/virtual-machines-windows-portal-sql-server-provision/azure-compute-sql-deployment-model.png)

## <a name="configure-the-vm"></a>VM konfigureerimine
On viis labad konfigureerida SQL serveri virtuaalse masina.

| Toiming               | Kirjeldus                          |
|---------------------|-------------------------------|
| **Põhitõed**              | [Tavaline sätete konfigureerimine](#1-configure-basic-settings)      |
| **Suurus**                | [Virtuaalse masina suuruse valimine](#2-choose-virtual-machine-size)   |
| **Sätted**            | [Valikuline funktsioonide konfigureerimine](#3-configure-optional-features)   |
| **SQL Serveri sätted** | [SQL serveri sätete konfigureerimine](#4-configure-sql-server-settings) |
| **Kokkuvõte**             | [Kokkuvõte](#5-review-the-summary)            |

## <a name="1-configure-basic-settings"></a>1. lihtne sätete konfigureerimine
Enne **põhitõed** , sisestage järgmine teave:

* Sisestage kordumatu virtuaalse masina **nimi**.
* Määrake **kasutajanimi** kohaliku administraatorikonto VM. SQL serveri **süsteemiadministraator** fikseeritud serveri rolli lisatakse ka selle kontoga.
* Sisestage keerukas **parool**.
* Kui teil on mitu tellimust, veenduge, et tellimus on õiged uue VM.
* Tippige väljale **Ressursirühma** nimi uue ressursirühma. Teise võimalusena kasutada mõne olemasoleva ressurss klõpsake rühma, **Valige olemasolev**. Ressursirühma on seotud ressursid Azure (virtuaalmasinates, salvestusruumi kontod, virtuaalse võrgu jne).

    >[AZURE.NOTE] Abil uue ressursirühma on kasulik, kui te olete testimine või Azure SQL serveri juurutuste tundma õppida. Kui olete lõpetanud oma test, kustutada ressursirühm automaatseks kustutamiseks VM ja kõik selle ressursirühma seotud ressursid. Ressursi rühmade kohta leiate lisateavet teemast [Azure ressursihaldur ülevaade](../azure-resource-manager/resource-group-overview.md).

* Valige **asukoht** selle juurutamiseks.
* Klõpsake sätete salvestamiseks nuppu **OK** .

    ![SQL-i põhitõed Blade](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-basic.png)

## <a name="2-choose-virtual-machine-size"></a>2 virtuaalse masina suuruse valimine
Valige **suurus** etappi, **Valige suurus** tera virtuaalse masina suurus. Tera kuvab kõigepealt soovitatav arvuti suurused valitud malli põhjal. Tagastab hinnangulise ka kuutasuga VM käivitamiseks.

![SQL-i VM suvandid](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-vm-choose-a-size.png)

Tootmise töökoormus, soovitatav on valida suvand virtuaalse masina suurus, mis toetab [Premium mälu](../storage/storage-premium-storage.md). Kui te ei pea seda tasemel, kasutage nupp **Kuva kõik** , mis kuvatakse kõigi arvuti suvandid. Näiteks võite kasutada seadme väiksem arendamiseks või testimiskeskkonnas.

>[AZURE.NOTE] Lisateavet virtuaalse masina suuruses kuvamiseks [virtuaalmasinates suurused](virtual-machines-windows-sizes.md). Kaalutluste kohta SQL serveri VM suurused, vt [jõudluse head tavad SQL Server Azure'i Virtuaalmasinates](virtual-machines-windows-sql-performance.md).

Oma seadme suuruse valimine ja seejärel klõpsake nuppu **Vali**.

## <a name="3-configure-optional-features"></a>3. valikuline funktsioonide konfigureerimine
Enne **sätted** , konfigureerida Azure salvestusruumi, networking ja virtuaalse masina jälgimine.

- Määrake jaotises **salvestusruumi**, on **Tippige ketta** Standard- või Premium ketas. Premium mälu on soovitatav tootmise töökoormus.

>[AZURE.NOTE] Kui valite seadme suurus, mis ei toeta Premium salvestusruumi Premium ketas, oma seadme suurus muutub automaatselt.  

- **Salvestusruumi konto**, saate aktsepteerida automaatselt ettevalmistatud salvestusruumi konto nimi. Võite klõpsata ka **salvestusruumi konto** nuppu konto ja konfigureerimiseks salvestusruumi konto tüüp. Vaikimisi Azure'i loob uue konto salvestusruumi kohalikult liigsete salvestusruumi. Salvestusruumi suvandite kohta leiate lisateavet teemast [Azure Storage kopeerimine](../storage/storage-redundancy.md).

- Jaotises **võrku**nõustute automaatselt täidetud väärtused. Võite klõpsata ka iga funktsiooni **Virtual võrgu**, **alamvõrku**, **avaliku IP-aadress**ja **Võrgu turberühma**käsitsi konfigureerimiseks. Selleks et selles õpetuses, võtke vaikeväärtusi.

- Azure'i võimaldab **jälgimis** vaikimisi määratud VM salvestusruumi sama kontoga. Saate muuta järgmisi sätteid siin.

- Määrake jaotises **kättesaadavus**, on kättesaadavus määramine. Selleks et selles õpetuses, saate valida **pole**. Kui plaanite SQL-i Kättesaadavusrühmad häälestada, konfigureerida vältimiseks taasloomine virtuaalse masina kättesaadavus.  Lisateavet leiate teemast [Virtuaalmasinates kättesaadavus](virtual-machines-windows-manage-availability.md).

Kui olete valmis konfigureerida järgmisi sätteid, klõpsake nuppu **OK**.

## <a name="4-configure-sql-server-settings"></a>4. SQL serveri sätete konfigureerimine
Enne **SQL Serveri sätted** , konfigureerida teatud sätted ja SQL serveri Optimeerimised. Saate konfigureerida SQL Serveri sätted on järgmised.

| Säte               |
|---------------------|
| [Ühenduvus](#connectivity)              |
| [Autentimine](#authentication)                |
| [Salvestusruumi konfigureerimine](#storage-configuration)            |
| [Automaatse lappimine](#automated-patching) |
| [Automaatse varundamise](#automated-backup)             |
| [Azure'i võtme Vault integreerimine](#azure-key-vault-integration)             |
| [R-teenused](#r-services) |

### <a name="connectivity"></a>Ühenduvus
Määrake jaotises **SQL-i teenus**, soovite selle VM SQL serveri eksemplar juurdepääsu tüüp. Selle õpetuse selleks, valige **avalik (internet)** lubama ühendusi SQL serveri masinad või teenuste Interneti kaudu. See suvand on valitud, Azure'i konfigureerib automaatselt tulemüüri ja võrgu turberühma Port 1433 liikluse lubamiseks.  

![SQL-i ühenduvuse suvandid](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-connectivity-alt.png)

SQL serveri Interneti kaudu ühenduse ka tuleb lubada SQL serveri autentimine, mida on kirjeldatud järgmises jaotises.

>[AZURE.NOTE] On võimalik lisada rohkem piiranguid võrgu side oma SQL serveri VM. Selleks redigeerimine jaotises võrku turvalisus VM loomise järel. Lisateavet leiate teemast [mis on võrgu turvalisus jaotises (NSG)?](../virtual-network/virtual-networks-nsg.md)

Kui eelistate ei luba ühendused andmebaasimootor Interneti kaudu, valige üks järgmistest suvanditest.

- **Kohaliku (sees ainult VM)** lubama ühendusi ainult SQL serveri VM sees.
- **Privaatne (virtuaalne võrgustikus)** lubama ühendusi SQL serveri masinad või teenuste virtuaalse samasse võrku.

>[AZURE.NOTE] Automaatselt ei võimalda virtuaalse masina pildi SQL Server Express Editioni kohta. See kehtib ka ja avalikes ühenduvuse suvandid. Express väljaanne, peate kasutama SQL Serveri konfigureerimishaldur [käsitsi](#configure-sql-server-to-listen-on-the-tcp-protocol) lubamise kohta VM loomise järel.

Üldjuhul turvalisuse täiustamine, valides kõige piiravad Ühenduvus, mis võimaldab teie stsenaariumi. Kuid kõigi suvandite turvatavate kaudu võrgu turberühma reeglid ja SQL-i/Windowsi autentimist.

**Port** vaikimisi 1433. Saate määrata pordinumber.
Lisateabe saamiseks vt [ühendus, et SQL serveri virtuaalse masina (ressursihaldur) | Microsoft Azure'i](virtual-machines-windows-sql-connect.md).

### <a name="authentication"></a>Autentimine
Kui vajate SQL serveri autentimine, klõpsake jaotises **SQL-i autentimise** **lubamine** .

![SQL serveri autentimine](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-authentication.png)

>[AZURE.NOTE] Kui plaanite juurde pääseda SQL serveri Interneti (st avaliku ühenduvuse valik), tuleb lubada SQL-i autentimise siin. Avaliku juurdepääs SQL Server nõuab autentimist SQL-i kasutamine.

Kui lubate SQL serveri autentimine, määrake **kasutajanimi** ja **parool**. Selle kasutaja nimi on konfigureeritud SQL serveri autentimine kasutajanime ja **süsteemiadministraator** fikseeritud rolli liige. Vt [valimine soovitud autentimisrežiim](http://msdn.microsoft.com/library/ms144284.aspx) autentimise režiimide kohta lisateavet.

Kui lubate SQL serveri autentimine, siis saate kasutada kohaliku administraatorikonto VM ühenduse SQL serveri eksemplar.

### <a name="storage-configuration"></a>Salvestusruumi konfigureerimine
Klõpsake **salvestusruumi konfiguratsiooni** määramiseks salvestusruumi nõuetele.

![SQL-i salvestusruumi konfigureerimine](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-storage.png)

>[AZURE.NOTE] Kui valite kindlad, pole see suvand saadaval. Automaatse salvestusruumi optimeerimine on saadaval ainult Premium mälu.

Saate määrata nõuded sisend toimingute läbilaskevõime MB/s ja kokku mälumaht sekundis (IOPs). Konfigureerida järgmisi väärtuseid libiseva skaalasid. Portaali arvutab arvu ketast nende nõuete alusel.

Vaikimisi mahub Azure'i talletamist 5000 IOPs, 200 MB ja 1 TB salvestusruumi. Saate muuta nende salvestusruumi sätete põhjal töökoormus. Valige jaotises **jaoks optimeeritud salvestusruumi**, üks järgmistest suvanditest:

- **Üldine** on vaikesäte ja toetab enamik töökoormus.
- **Tehinguotsust** töötlemine optimeerib salvestusruumi traditsiooniline andmebaasi OLTP töökoormus.
- **Võrgupõhised andmebaasid** optimeerib salvestusruumi analüütiline ja aruannete töökoormus.

>[AZURE.NOTE] Ülemmäära liugurid erineda sõltuvalt teie valitud virtuaalse masina suurus.

### <a name="automated-patching"></a>Automaatse lappimine
**Automaatse lappimine** on vaikimisi. Automaatne lappimine võimaldab Azure'i automaatselt paik SQL serveri ja operatsioonisüsteemi. Määrake päeva, nädala-, kellaaja- ja hoolduse kestus. Azure'i teostab lappimine selles aknas hooldustööd. Hoolduse akna ajakava kasutab VM lokaadi korda. Kui te ei soovi Azure'i automaatselt paik SQL serveri ja operatsioonisüsteemi, klõpsake nuppu **Keela**.  

![SQL-i automaatse lappimine](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-patching.png)

Lisateavet leiate teemast [Automaatse lappimine SQL Server Azure'i Virtuaalmasinates](virtual-machines-windows-sql-automated-patching.md).

### <a name="automated-backup"></a>Automaatse varundamise
Luba automaatne andmebaasi varundamine kõigile andmebaasidele jaotises **automaatse varundamise**. Vaikimisi on keelatud automaatse varundamise.

Kui lubate SQL-i automaatse varundamise, saate konfigureerida järgmist.

- Säilitusperiood (päeva) varufailide
- Salvestusruumi konto kasutamiseks varukoopiate
- Krüptimise suvand ja parooli varufailide

Krüptida varukoopia, klõpsake nuppu **Luba**. Määrake **parool**. Azure'i loob varukoopiaid krüptimiseks sert ja kasutab seda serti kaitsmiseks määratud parool.

![SQL-i automaatse varundamise](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-autobackup.png)

 Lisateavet leiate teemast [Automaatse varundamise SQL Server Azure'i Virtuaalmasinates](virtual-machines-windows-sql-automated-backup.md).

### <a name="azure-key-vault-integration"></a>Azure'i klahvi Vault integreerimine
Azure'i saladused talletatakse krüptimiseks klõpsake **integreerimine Azure võtme vault** ja klõpsake nuppu **Luba**.

![SQL Azure'i võtme Vault integreerimine](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-akv.png)

Järgmises tabelis on loetletud nõutav Azure klahvi Vault integreerimise konfigureerimiseks parameetrid.

|PARAMEETRI|KIRJELDUS|NÄIDE|
|----------|----------|-------|
|**Võtme Vault URL-i** |Võtme vault asukoht.|https://contosokeyvault.Vault.Azure.net/ |
|**Turvasubjektinimi** |Azure Active Directory teenuse põhilist nime. See nimi on ka edaspidi kliendi ID.  |fde2b411 - 33d 5-4e11-af04eb07b669ccf2|
| **Peamine salajane**|Azure Active Directory teenuse põhisumma salajane. Selle salajase ka edaspidi kliendi salajane. | 9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM =|
|**Mandaadi nimi**|**Mandaadi nimi**: AKV integreerimine loob jooksul SQL Server, võimaldades juurdepääsu võtme vault VM mandaati. Valige selle mandaadi nimi.| mycred1|

Lisateabe saamiseks vt [Konfigureerimine Azure'i klahvi Vault integreerimine Azure VMs SQL Server](virtual-machines-windows-ps-sql-keyvault.md).

Kui olete lõpetanud, SQL serveri sätete konfigureerimine, klõpsake nuppu **OK**.

### <a name="r-services"></a>R-teenused
SQL Server 2016 ettevõtte väljaanne, on teil võimalus lubada [SQL serveri R teenused](https://msdn.microsoft.com/library/mt604845.aspx). See võimaldab teil kasutada täiustatud analüüsi SQL Server 2016. Enne **SQL Serveri sätted** nuppu **Luba** .

![SQL serveri R Services lubamine](./media/virtual-machines-windows-portal-sql-server-provision/azure-vm-sql-server-r-services.png)

>[AZURE.NOTE] SQL serveri pilte, mis pole 2016 Enterprise'i, suvandi lubamiseks R teenused on keelatud.

## <a name="5-review-the-summary"></a>5. Ülevaade Kokkuvõte
**Kokkuvõte** enne kokkuvõte ja klõpsake nuppu **OK** , et luua SQL serveri, ressursirühm ja selle VM jaoks määratud ressursid.

Saate jälgida juurutamise azure portaalist. Nupp **teatised** ekraani ülaosas kuvatakse juurutamise tavaline olek.

>[AZURE.NOTE] Teile aimu juurutamise korda, I juurutatud SQL-i VM Ida-USA piirkonnas vaikesätetega. Selle testi juurutamise võttis 26 minutit lõpuleviimiseks. Kuid võivad ilmneda kiiremini või aeglasemalt juurutamise aja vastavalt teie regioonis ja valitud sätted.

## <a name="open-the-vm-with-remote-desktop"></a>Kaugtöölaua VM avamine

Virtuaalse masina kaugtöölaua ühenduse loomiseks tehke järgmist:

1. Pärast Azure VM on loodud, kuvatakse ikooni VM Azure armatuurlauale. Leiate selle sirvides oma olemasoleva virtuaalmasinates. Klõpsake SQL-i virtual seadme. **Virtuaalse masina** blade kuvatakse teie virtuaalse masina üksikasjad.
1. **Virtuaalse masina** tera ülaosas käsku **Ühenda**.
1. Brauseri allalaadimine VM RDP-faili. Avage RDP-faili.
    ![Kaugtöölaua SQL-i VM](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-vm-remote-desktop.png)
1. Kaugtöölaua kliendi teavitab Publisheri selle kaugtöölaua ei saa tuvastada. Klõpsake nuppu **Loo ühendus** jätkata.
1. Klõpsake dialoogiboksis **Windowsi Turve** **kasutada mõne muu kontoga**.
1. **Kasutajanimi** Type ** \<kasutajanimi >**, kus <user name> on määratud, kui olete konfigureerinud VM kasutajanimi. Teil on mõni algse kurakriipsu enne nime lisamiseks.
1. Tippige **parool** selle VM jaoks eelnevalt konfigureeritud, ja seejärel klõpsake nuppu **OK** , et ühendada.
1. Kui teine **Kaugtöölaua ühendus** dialoogiboksi küsib, kas ühenduse, klõpsake nuppu **Jah**.

Kui loote ühenduse SQL serveri virtuaalse masina, saate käivitada SQL Server Management Studio ja ühendada Windowsi autentimist kasutades kohaliku administraatori identimisteave. Kui märkisite SQL serveri autentimine, saate ka kasutajaga SQL-i autentimist kasutades SQL-i kasutajanime ja parooli, mis on konfigureeritud ettevalmistamise ajal.

Seadme juurdepääsu võimaldab otse muutmine kohapeal ja SQL Serveri sätted oma vajaduste järgi. Näiteks võite tulemüüri sätete konfigureerimine või SQL Server configuration sätete muutmine.

## <a name="connect-to-sql-server-remotely"></a>SQL serveriga kaugühenduse teel

Selles õpetuses valisime **avaliku** juurdepääsu virtuaalse masina ja **SQL serveri autentimine**. Nende sätete automaatselt virtuaalse masina lubama SQL serveri ühendusi, mis tahes klient Interneti (eeldades, et neil on õige SQL-i Logi sisse).

>[AZURE.NOTE] Kui te ei valinud avaliku ettevalmistamise ajal, siis lisatoimingud juurdepääsemiseks vajalikud SQL serveri eksemplar Interneti kaudu. Lisateavet leiate teemast [ühenduse loomine abil SQL serveri virtuaalse masina](virtual-machines-windows-sql-connect.md).

Järgmistes jaotistes näitab, kuidas ühenduse SQL serveri eksemplar oma VM mõnes teises arvutis Interneti kaudu.

> [AZURE.INCLUDE [Connect to SQL Server in a VM Resource Manager](../../includes/virtual-machines-sql-server-connection-steps-resource-manager.md)]

## <a name="next-steps"></a>Järgmised sammud
Muud Azure SQL serveri kasutamise kohta leiate artiklist [SQL Server Azure'i Virtuaalmasinates](virtual-machines-windows-sql-server-iaas-overview.md) ja [Korduma kippuvad küsimused](virtual-machines-windows-sql-server-iaas-faq.md).

SQL Server Azure'i Virtuaalmasinates video ülevaate saamiseks vaadake [Azure VM on parim platvorm, SQL Server 2016](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/Azure-VM-is-the-best-platform-for-SQL-Server-2016).

SQL Server Azure'i virtuaalmasinates [Uuri Õppeteema](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) .
