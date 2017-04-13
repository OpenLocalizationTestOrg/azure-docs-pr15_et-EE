<properties
    pageTitle="Konfigureerimine alati kättesaadavus rühma Azure VM automaatselt - ressursihaldur"
    description="Loo alati kättesaadavus rühma Azure'i virtuaalmasinates Azure'i ressursihaldur režiimis. Selle õpetuse peamiselt kasutab kasutajaliidese automaatselt luua kogu lahendus."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-resource-manager" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/20/2016"
    ms.author="MikeRayMSFT" />

# <a name="configure-always-on-availability-group-in-azure-vm-automatically---resource-manager"></a>Konfigureerimine alati kättesaadavus rühma Azure VM automaatselt - ressursihaldur

> [AZURE.SELECTOR]
- [Ressursihaldur: Mall](virtual-machines-windows-portal-sql-alwayson-availability-groups.md)
- [Ressursihaldur: käsitsi](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)
- [Klassikaline: kasutajaliides](virtual-machines-windows-classic-portal-sql-alwayson-availability-groups.md)
- [Klassikaline: PowerShelli](virtual-machines-windows-classic-ps-sql-alwayson-availability-groups.md)

<br/>

Õppeteema – lõpuni näidatakse, kuidas luua SQL serveri kättesaadavus rühma ressursihaldur Azure'i virtuaalmasinates. Õpetuse kasutab Azure labad konfigureerida malli. Kas vaikesätete uurimiseks, tippige nõutav sätted ja värskendada labad portaalis, nagu te tutvustavad selles õpetuses.

Õpetuse lõpus teie SQL serveri kättesaadavus rühma lahendus Azure koosneb järgmisi elemente:

- Mitu alamvõrku, sh on ees- ja tagaandmebaas alamvõrgu sisaldavad virtuaalse võrgus

- Kahe domeenikontrolleri Active Directory (AD) domeeniga

- Kahe SQL serveri VMs tagaandmebaas alamvõrgu juurutatud ja ühendatud AD domeeni

- 3-sõlm WSFC kobar mudeliga sõlm enamik kvoorumi

- Kättesaadavus rühma kaks sünkroonse Kinnita koopiad andmebaasi kättesaadavus

Alloleval joonisel on graafiline kujutis lahendus.

![AG Azure test Lab arhitektuur](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/0-EndstateSample.png)

Kõigi ressursside kohta see lahendus kuuluvad ühte ressursirühma.

Selle õpetuse eeldab järgmist:

- Teil on juba Azure'i konto. Kui teil pole ühte [prooviversiooni konto](http://azure.microsoft.com/pricing/free-trial/).

- Teate juba ettevalmistamise SQL serveri VM abil GUI virtuaalse masina galeriist. Lisateabe saamiseks lugege teemat [Azure SQL serveri virtuaalse masina ettevalmistamine](virtual-machines-windows-portal-sql-server-provision.md)

- Teil on juba ühtlase mõistmise kättesaadavus rühmad. Lisateavet leiate teemast [alati kättesaadavus rühmade (SQL Server)](http://msdn.microsoft.com/library/hh510230.aspx).

>[AZURE.NOTE] Kui olete huvitatud kättesaadavus rühmade kasutamine koos SharePointi, lugege teemat [konfigureerimine SQL Server 2012 alati On kättesaadavus rühmad SharePoint 2013](http://technet.microsoft.com/library/jj715261.aspx).

Selles õppetükis saate Azure'i portaalis.

- Valige soovitud portaalist alati Mall

- Malli sätete ülevaatamine ja mõned keskkonna konfiguratsioon sätete värskendamine

- Azure'i jälgimine loob kogu keskkonnas

- Ühendage üks hulka kuuluvad soovitud domeen ja seejärel üks SQL-i serverid

[AZURE.INCLUDE [availability-group-template](../../includes/virtual-machines-windows-portal-sql-alwayson-ag-template.md)]


## <a name="provision-the-cluster-from-the-gallery"></a>Sätte kobar galeriist

Azure'i pakub kogu lahenduse galeriis pilt. Malli leidmiseks:

1.  Logige sisse Azure portaali konto kaudu.
1.  Azure'i portaali klõpsamisel **+ Uus.** Portaali avatakse uus laba.
1.  Uue blade otsida **AlwaysOn**.
![AlwaysOn malli otsimine](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/16-findalwayson.png)
1.  Leidke Otsingu tulemused **SQL serveri AlwaysOn kobar**.
![AlwaysOn Mall](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/17-alwaysontemplate.png)
1.  Valige **Valige juurutamise mudeli** **Ressursihaldur**.

### <a name="basics"></a>Põhitõed

Klõpsake **põhialused** ja konfigureerida järgmist:

- **Administraatori nime** on nii SQL Serveri eksemplari domeeni administraatoriõigused ja SQL serveri süsteemiadministraator fikseeritud serveri rolli liige kasutajakonto. Selle õpetuse kasutamise **DomainAdmin**.

- **Parool** on domeeni administraatori konto parool. Keeruka parooli kasutada. Kinnitage parool.

- **Tellimust** on tellimus, mis Azure'i arve kõigi ressursside kättesaadavus rühma juurutatud käivitatud. Muud tellimust saate määrata, kui teie konto on mitu tellimust.

- **Ressursirühm** on kuuluvad kõik Azure ressursid loodud selles õpetuses rühma nime. Selle õpetuse kasutada **SQL-HA-RG**. Lisateavet leiate teemast (Azure'i ressursihaldur ülevaade) [ressursi-rühma-overview.md / # ressursi-rühmad].

- Azure'i piirkond, kus luuakse selles õpetuses mõeldud ressursid on **koht** . Valige majutada infrastruktuuri Azure piirkond.

Allpool on **põhitõed** tera on näha:

![Põhitõed](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/1-basics.png)

- Klõpsake nuppu **OK**.

### <a name="domain-and-network-settings"></a>Domeeni ja võrgu sätted

See Azure Galerii mall loob uue domeeni uus domeenikontrollerid. See loob ka uue võrgu ja kahe alamvõrku. Malli loomine olemasoleva domeeni või virtuaalse võrgu serverid ei võimalda. Järgmiseks on domeeni ja võrgu sätete konfigureerimine.

**Domeeni ja võrgu** sätete tera läbi vaatama algne domeen ja võrgu sätted:

- **Mets juurkausta domeeninimi** on domeeninimi, mida kasutatakse klaster majutavad AD domeeni. Juhend on kasutada **contoso.com**.

- **Virtuaalne võrgu nimi** on Azure virtuaalse võrgu võrgu nimi. Selle õpetuse kasutamise **autohaVNET**.

- **Selle domeenikontrolleri alamvõrgu nimi** on nimi, virtuaalse võrgu, mis hostib domeenikontrolleri osa. Selles õpetuses mõeldud kasutada **alamvõrgu-1**. Selle alamvõrgu kasutatakse aadress eesliide **10.0.0.0/24**.

- **SQL serveri alamvõrgu nimi** on nimi, et hosts SQL-i serverid ja faili jagada tunnistaja virtuaalse võrgu osa. Selle õpetuse kasutamise **alamvõrgu-2**. Selle alamvõrgu kasutatakse aadress eesliide **10.0.1.0/26**.

Lisateavet [leiate Azure'i virtuaalse võrgu ülevaade](../virtual-network/virtual-networks-overview.md)virtuaalse võrkude kohta.  

**Domeeni ja võrgu sätted** peaks välja nägema umbes järgmine:

![Domeeni ja võrgu sätted](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/2-domain.png)

Vajaduse korral saate muuta need väärtused. Selle õpetuse kasutame eelseadistatud väärtused.

- Kontrollige sätteid ja klõpsake nuppu **OK**.

###<a name="availability-group-settings"></a>kättesaadavus rühma sätted

**Kättesaadavus Rühmasätete** vaadata kättesaadavus rühma ja kuulajale eelseadistatud väärtused.

- **Ligipääsetavusele rühma nimi** on rühmitatud ressursi kättesaadavus rühma nimi. Selle õpetuse kasutamise **Contoso-ag**.

- klaster ja sisemise koormuse koormusetasakaalustusteenuse kasutatakse **kättesaadavus rühma kuulajale nime** . Kliendid Microsoft SQL serveri abil saate selle nime ühenduse andmebaasi vastav koopiat. Selle õpetuse kasutamise **Contoso-kuulajale**.

-  **kättesaadavus rühma kuulajale port** määrab TCP pordi SQL serveri kuulajale kasutama hakkab. Kasutage selles õpetuses vaikeport **1433**.

Vajaduse korral saate muuta need väärtused. Selles õpetuses kasutage eelmääratud väärtused.  

![kättesaadavus rühma sätted](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/3-availabilitygroup.png)

- Klõpsake nuppu **OK**.

###<a name="vm-size-storage-settings"></a>Talletusmahu VM suurus

**VM suurus** , talletusmahu SQL serveri virtuaalse masina suuruse valimine ja vaadake üle ka muud sätted.

- **SQL serveri virtuaalse masina** on nii SQL Server Azure'i virtuaalse masina suurus. Valige oma töökoormus virtuaalse masina suurus. Kui olete hoone selle õpetuse keskkonna kasutada **DS2**. Tootmiseks töökoormus toetavad töökoormus virtuaalse masina suuruse valimine. Nõuab palju tootmise töökoormus **DS4** või suurem. Malli ehitada kaks virtuaalmasinates selle suurus ja SQL Serverit installida igas seadmes. Lisateavet leiate teemast [virtuaalmasinates suurused](virtual-machines-linux-sizes.md).

>[AZURE.NOTE]Azure'i installitakse SQL serveri väljaande Enterprise. Maksumus sõltub väljaanne ja virtuaalse masina suurus. Täpsemat teavet praeguse hindade kohta leiate teemast [virtuaalmasinates hinnad](http://azure.microsoft.com/pricing/details/virtual-machines/#Sql).

- **Domeeni domeenikontrolleri virtuaalse masina** on virtuaalse masina suurus domeenikontrollerite. Selle õpetuse kasutamise **D2**.

- **Faili ühiskasutus tunnistaja virtuaalse masina** on virtuaalse masina suurus jaoks failide ühiskasutuse haldur. Selle õpetuse kasutamise **A1**.

- **SQL-i salvestusruumi konto** on SQL serveri andmetega ja operatsioonisüsteemi ketast salvestusruumi konto nimi. Selle õpetuse kasutamise **alwaysonsql01**.

- **Näiteks Põhiliselt salvestusruumi konto** on domeenikontrollerid salvestusruumi konto nimi. Selle õpetuse kasutamise **alwaysondc01**.

- **SQL serveri andmetega kettale suurus** TB on SQL serveri andmetega ketast TB suurust. Määrake arv vahemikus 1 – 4. See on andmete ketta, mis lisatakse iga SQL Server. Selle õpetuse kasutamise **1**.

- **Mäluruumi optimeerimine** määrab teatud salvestusruumi konfiguratsioonisätted SQL serveri virtuaalmasinates töökoormus tüüp. Kõik SQL-i serverid sel premium mäluruumi kasutamine Azure hosti vahemälu kirjutuskaitstud. Lisaks saab optimeerida töökoormus SQL Serveri sätted, valides ühe järgmised kolm sätted:

    - **Üldine töökoormus** määrab ilma kindla konfiguratsioonisätted

    - **Tehinguotsust töötlemine** määrab jälgi lipu 1117 ja 1118

    - **Võrgupõhised andmebaasid** määrab jälgi lipu 1117 ja 610

Selle õpetuse kasutamise **üldine töökoormus**.

![VM suurus salvestusruumi sätted](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/4-vm.png)

- Kontrollige sätteid ja klõpsake nuppu **OK**.

####<a name="a-note-about-storage"></a>Märkus salvestusruum

Täiendavad Optimeerimised sõltuvad SQL serveri andmetega ketast suurust. Iga Teratavu andmed kettale, lisab Azure'i täiendavad 1 TB premium salvestusruumi (SSD). Server nõuab 2 TB või rohkem, loob malli lisamine talletusmahtu iga SQL serveris. On talletusmahtu on salvestusruumi Virtualization vorm, kus mitmele plaadile on konfigureeritud anda suurema võimsus, paindlikkust ning jõudlust.  Malli seejärel loob salvestusruumi soovitud talletusmahtu ja esitab seda operatsioonisüsteemi ühe andmetena. Mall määrab selle ketta andmete ketas SQL serveri korral. Malli lugusid selle talletusmahtu SQL serveri abil järgmised sätted:

- Triip maht on virtuaalse ketta interleave sätet. Selgituseks töökoormus see on seatud 64 KB. Andmete hoidmise töökoormus on 256 KB.

- Paindlikkust on lihtne (paindlikkust ei).

>[AZURE.NOTE] Azure premium mälu on kohalik liigsete ja hoiab andmeid kolm eksemplari ühtse piirkonna, nii, et juures olevat talletusmahtu täiendavad paindlikkust ei ole vaja.

- Salvestusruumi kogumi ketast arv võrdub veergude arv.

Lisateavet mäluruumi leiate ruumi ja salvestusruumi.

- [Salvestusruumi tühikuid ülevaade](http://technet.microsoft.com/library/hh831739.aspx).

- [Windows Serveri varukoopia ja salvestusruumi kaustu](http://technet.microsoft.com/library/dn390929.aspx)

SQL Server configuration heade tavade kohta leiate lisateavet teemast [jõudluse SQL Server Azure'i virtuaalmasinates head tavad](virtual-machines-windows-sql-performance.md)


###<a name="sql-server-settings"></a>SQL Serveri sätted

**SQL serveri sätete** kohta vaadake üle ja muutke SQL serveri VM nime eesliide, SQL serveri versioon, SQL Serveri teenuse konto ja parool ja SQL-i automaatse lappimine hooldus ajakava.

- **SQL serveri nime eesliide** saab luua iga SQL serveri nimi. Selle õpetuse kasutamise **Contoso-ag**. SQL serveri nimesid saab *Contoso-ag-0* ja *Contoso-ag-1*.

- **SQL serveri versioon** on SQL serveri versioon. Selle õpetuse jaoks kasutada **SQL serveri 2014**. Soovi korral saate ka **SQL Server 2012** või **SQL Server 2016**.

- **SQL Serveri teenuse konto kasutajanimi** on SQL Serveri teenuse konto domeeninime. Selle õpetuse kasutamise **sqlservice**.

- **Parool** on SQL Serveri teenuse konto parooli.  Keeruka parooli kasutada. Kinnitage parool.

- **SQL-i automaatne lappimine hooldus ajakava** tuvastab, et Azure'i kuvatakse automaatselt paik SQL-i serverid nädalapäeva. Tippige selle õpetuse **pühapäev**.

- **SQL-i automaatne lappimine hooldustööd käivitamine tund** on kellaaeg Azure regiooni jaoks, kui automaatne paikamine hakkab.

>[AZURE.NOTE]Iga VM lappimine akna jagatakse ühe tunni võrra. Ainult ühe virtuaalse masina on paigatud korraga teenuse katkemise vältimiseks.

![SQL Serveri sätted](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/5-sql.png)

Kontrollige sätteid ja klõpsake nuppu **OK**.

###<a name="summary"></a>Kokkuvõte

Kokkuvõte lehel Azure'i kontrollib sätted. Samuti saate alla laadida mall. Vaadake üle kokkuvõttele. Klõpsake nuppu **OK**.

###<a name="buy"></a>Ostmine

See lõplik blade sisaldab **kasutustingimused**ja **Privaatsus**. Seda teavet vaadata. Kui olete Azure'i virtuaalmasinates selle ja kõik muud nõutavad ressursid kättesaadavus rühma loomise alustamiseks valmis, klõpsake nuppu **Loo**.

Azure'i portaalis loob ressursirühma ja kõik ressursid.

##<a name="monitor-deployment"></a>Kuvari juurutamine

Azure'i portaalis juurutamise edenemist jälgida. Juurutamise ikooni kinnitatakse automaatselt Azure portaali armatuurlaud.

![Azure'i armatuurlaud](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/11-deploydashboard.png)

##<a name="connect-to-sql-server"></a>SQL serveriga

SQL serveri uued eksemplarid töötavad virtuaalmasinates, millel on Interneti-ühenduste. Domeenikontrollerid on siiski Interneti vastastikuste ühendus. Selleks, et luua ühenduse SQL-i serverid kaugtöölaua, esimese RDP ühele hulka kuuluvad selle domeeni. Selle domeenikontrolleri avage teise RDP SQL serveriga.

Esmane domeenikontrolleri RDP, toimige järgmiselt.

1.  Azure'i portaali armatuurlaualt väga, et juurutamise õnnestus.

1.  Klõpsake kategooriat **ressursid**.

1.  Klõpsake **ressursid** labale **ad-primaarne – näiteks põhiliselt** on virtuaalse masina esmase domeenikontrolleri arvuti nimi.

1.  Enne **ad-primaarne – näiteks põhiliselt** jaoks klõpsake nuppu **Ühenda**. Brauseri küsib, kas soovite avada või salvestada kaugühenduse objekti. Klõpsake nuppu **Ava**.
![Ühenduse näiteks Põhiliselt](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/13-ad-primary-dc-connect.png)
1.  **Kaugtöölaua ühendus** teid hoiatada, publisher selle kaugtöölaua ei saa tuvastada. Klõpsake nuppu **Loo ühendus**.

1.  Windowsi Turve, mis palub teil sisestada mandaat ühenduse esmase domeenikontrolleri IP-aadress. Klõpsake nuppu **Kasuta mõne muu kontoga**. Sisestage väljale **kasutajanimi** **contoso\DomainAdmin**. See on teie valitud administraatori nime jaoks konto. Kui olete konfigureerinud Mall valitud keeruka parooli kasutada.

1.  **Kaugtöölaua** teid hoiatada, et kaugarvuti ei saa autentida oma turbeserti probleemid. See näitab teile turvalisus serdi nimi. Kui olete täitnud õpetuse nimi on **ad-primaarne-dc.contoso.com**. Klõpsake nuppu **Jah**.

Teil on nüüd ühendatud esmase domeenikontrolleri. RDP SQL Server, toimige järgmiselt.

1.  Selle domeenikontrolleri, avage **Kaugtöölaua ühendus**.

1.  Tippige **arvutisse**, SQL serveri nimi. Tippige selle õpetuse **sqlserver-0**.

1.  Kasutage sama konto ja parool, mida kasutasite RDP domeenikontrolleri.

Teil on nüüd seotud RDP SQL serveriga. Saate avada SQL Server management Studios, ühenduse loomine SQL serveri eksemplar vaikimisi ja veenduge, et jaotises availabilty on konfigureeritud.


