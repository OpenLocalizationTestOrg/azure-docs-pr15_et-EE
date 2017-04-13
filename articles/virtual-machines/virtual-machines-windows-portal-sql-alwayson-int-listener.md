<properties
   pageTitle="Looge kuulajale AlwaysOn availabilty rühma SQL Server Azure'i Virtuaalmasinates"
   description="Üksikasjalikud juhised on kuulajale AlwaysOn availabilty rühma loomine SQL Server Azure'i Virtuaalmasinates"
   services="virtual-machines"
   documentationCenter="na"
   authors="MikeRayMSFT"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="virtual-machines"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows-sql-server"
   ms.workload="infrastructure-services"
   ms.date="07/12/2016"
   ms.author="MikeRayMSFT"/>

# <a name="configure-an-internal-load-balancer-for-an-alwayson-availability-group-in-azure"></a>Azure AlwaysOn kättesaadavus rühm on sisemine laadi koormusetasakaalustusteenuse konfigureerimine

See teema selgitab, kuidas luua ka sisemise koormuse koormusetasakaalustusteenuse jaoks SQL serveri kättesaadavusrühma Azure'i virtuaalmasinates töötab ressursi halduri mudelis. Mõne kättesaadavusrühma nõuab laadi koormusetasakaalustusteenuse Azure'i virtuaalmasinates on SQL Serveri eksemplari. Laadi koormusetasakaalustusteenuse talletab kättesaadavus rühma kuulajale IP-aadress. Kui kättesaadavus rühma ulatub mutliple regioonid, peab iga piirkonna laadi koormusetasakaalustusteenuse.

Selle toimingu tegemiseks peate olema juurutatud Azure'i virtuaalmasinates ressursi halduri mudel SQL serveri AlwaysOn kättesaadavus rühma. Nii SQL serveri virtuaalmasinates peab kuuluma samu kättesaadavus. [Microsoft malli](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) abil saate automaatselt luua selle kättesaadavusrühma Azure ressursihaldur. See mall loob teie jaoks automaatselt sisemise koormuse koormusetasakaalustusteenuse. 

Soovi korral saate [ka kättesaadavusrühma käsitsi konfigureerimine](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

See teema nõuab, et ligipääsetavusele rühm on juba konfigureeritud.  

Seotud teemad on järgmised:

 - [Azure'i VM (GUI) Kättesaadavusrühmad konfigureerimine](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)   
 
 - [Azure'i ressursihaldur ja PowerShelli abil VNet-VNet ühenduse konfigureerimine](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

## <a name="steps"></a>Juhised

Selle dokumendi jalutuskäigul siis loomine ja Azure portaali laadi koormusetasakaalustusteenuse konfigureerimine. Kui see on lõpule jõudnud, tuleb konfigureerida kasutama IP-aadressi laadi koormusetasakaalustusteenuse AlwaysOn kättesaadavus rühma kuulajale kobar.

## <a name="create-and-configure-the-load-balancer-in-the-azure-portal"></a>Loomine ja konfigureerimine laadi koormusetasakaalustusteenuse Azure'i portaalis

Selles osas tööülesande tuleb teha järgmist Azure'i portaalis.

1. Laadi koormusetasakaalustusteenuse loomine ja konfigureerimine IP-aadress

1. Rakenduskausta kirjutamata konfigureerimine

1. Funktsiooni juures loomine 

1. Laadi tasakaalustamiseks reeglite seadmine

>[AZURE.NOTE] Kui SQL-i serverid on erinevate ressursside rühmad ja regioonid, teete kõiki neid toiminguid kaks korda, kui iga ressursi jaotises.

## <a name="1-create-the-load-balancer-and-configure-the-ip-address"></a>1. koormusetasakaalustusteenuse Laadi loomine ja konfigureerimine IP-aadress

Esimene samm on laadi koormusetasakaalustusteenuse loomiseks. Avage Azure'i portaalis ressursirühm, mis sisaldab SQL serveri virtuaalmasinates. Ressursirühm, klõpsake nuppu **Lisa**.

- Otsige **laadimine koormusetasakaalustusteenuse**. Valige otsingu tulemuste seast **Laadi koormusetasakaalustusteenuse**, mis on avaldatud **Microsoft**.

- **Laadi koormusetasakaalustusteenuse** enne, klõpsake nuppu **Loo**.

- **Loo laadimine koormusetasakaalustusteenuse**, seadistada soovitud laadi koormusetasakaalustusteenuse järgmiselt:

| Säte | Väärtus |
| ----- | ----- |
| **Nimi** | Laadi koormusetasakaalustusteenuse tähistav tekst nimi. Näiteks **sqlLB**. |
| **Skeemi** | **Sisemise** |
| **Virtuaalse võrgu** | Valige virtuaalse võrk, mis on SQL-i serverid.   |
| **Alamvõrgu**  | Valige alamvõrku, mis on SQL-i serverid. |
| **Tellimuse** | Kui teil on mitu tellimust, kuvatakse selle välja. Valige tellimus, mida soovite selle ressursi seostada. Tavaliselt on sama subcription kõik ressursid kättesaadavus rühma nimega.  |
| **Ressursirühm** | Valige ressursirühm, mis on SQL-i serverid. | 
| **Asukoht** | Valige soovitud Azure SQL-i serverid on asukohta. |

- Klõpsake nuppu **Loo**. 

Azure'i loob laadi koormusetasakaalustusteenuse kohal konfigureeritud. Laadi koormusetasakaalustusteenuse kuulub võrgu alamvõrgu ressursirühm ja asukoht. Pärast Azure on lõpule jõudnud, kontrollige Azure koormuse koormusetasakaalustusteenuse sätted. 

Nüüd konfigureerimine laadi koormusetasakaalustusteenuse IP-aadress.  

- Klõpsake Laadi koormusetasakaalustusteenuse **sätted** enne **IP-aadress**. **IP-aadress** tera näitab, et see on privaatne laadi koormusetasakaalustusteenuse SQL serveri sama virtuaalse võrgus. 

- Määrake järgmised sätted: 

| Säte | Väärtus |
| ----- | ----- |
| **Alamvõrgu** | Valige alamvõrku, mis on SQL-i serverid. |
| **Ülesanne** | **Staatiline** |
| **IP-aadress** | Tippige kasutamata virtuaalse IP-aadressi alamvõrgu kaudu.  |

- Sätted salvestada.

Laadi koormusetasakaalustusteenuse on nüüd IP-aadress. Kirje IP-aadress. Kasutage IP-aadress on kuulajale klaster loomisel. Selle artikli PowerShelli skripti, kasutage selle aadress on `$ILBIP` muutujana.



## <a name="2-configure-the-backend-pool"></a>2 kirjutamata rakenduskausta konfigureerimine

Järgmiseks on kirjutamata aadressi kausta loomiseks. Azure'i kõned kirjutamata aadress pool *taustväärtus pool*. Sel juhul taustväärtus pool on kaks SQL serveri oma kättesaadavus jaotises aadressid. 

- Klõpsake oma ressursirühm laadi koormusetasakaalustusteenuse, mis on loodud. 

- Klõpsake **sätete** **kirjutamata kaustu**.

- **Taustväärtus aadress kaustu**, klõpsake nuppu **Lisa** taustväärtus aadress kausta loomiseks. 

- **Lisa kirjutamata kausta** **nime**, tippige kirjutamata kausta nimi.

- Klõpsake jaotises **virtuaalmasinates** **+ Lisa virtuaalse masina**. 

- Jaotises **Valige virtuaalmasinates** nuppu, **Valige soovitud kättesaadavus** ja määrake soovitud ligipääsetavusele seadmine SQL serveri virtuaalmasinates privaatsusrühma.

- Kui olete valinud kättesaadavus määramine, klõpsake **Valige soovitud virtuaalmasinates**. Klõpsake nuppu SQL Serveri eksemplari kättesaadavus rühma majutada kaks virtuaalmasinates. Klõpsake nuppu **Vali**. 

- Klõpsake nuppu **OK** labad sulgemiseks **Valige virtuaalmasinates**ja **Lisa taustväärtus pool**. 

Azure'i värskendab kirjutamata aadress rakenduskausta sätteid. Nüüd on teie seatud kättesaadavus on kaks SQL-i serverid.

## <a name="3-create-a-probe"></a>3. mõne juures loomine

Järgmiseks on luua mõne juures. Funktsiooni juures määratleb, kuidas Azure'i kontrollida mis SQL-i serverite praegu kuulub rühma kuulajale kättesaadavus. Azure'i sondi teenuse põhjal portide, kui loote selle juures määratlevad IP-aadress.

- Enne laadi koormusetasakaalustusteenuse **sätted** nuppu **sondid**. 

- Enne **sondid** , klõpsake nuppu **Lisa**.

- Konfigureerida **Lisa juures** enne selle juures. Järgmised väärtused abil saate konfigureerida selle juures:

| Säte | Väärtus |
| ----- | ----- |
| **Nimi** | Funktsiooni juures tähistav tekst nimi. Näiteks **SQLAlwaysOnEndPointProbe**. |
| **Protocol (protokoll)** | **TCP** |
| **Port** | Võite kasutada mis tahes saadaval port. Näiteks *59999*.    |
| **Intervall**  | *5* | 
| **Vigane lävi**  | *2* | 

- Klõpsake nuppu **OK**. 

>[AZURE.NOTE] Veenduge, et teie määratud pordi on nii SQL-i serverite tulemüüris avatud. Nii vaja sissetulevast TCP pordi, mida kasutate. Lisateabe saamiseks vaadake [lisamine või redigeerimine tulemüüri reegel](http://technet.microsoft.com/library/cc753558.aspx) . 

Azure'i loob selle juures. Azure'i kasutatakse funktsiooni juures testimiseks mis SQL Server on kuulajale kättesaadavus rühma.

## <a name="4-set-the-load-balancing-rules"></a>4. Laadi tasakaalustamiseks reeglite seadmine

Määrake laadi tasakaalustamiseks reeglid. Koormust tasakaalustavad reegleid konfigureerida, kuidas laadi koormusetasakaalustusteenuse marsruudib liikluse SQL-i serverid. Selle laadi koormusetasakaalustusteenuse jaoks on lubada otsest serveri saatja, sest ainult üks kahest SQL-i serverite kunagi oma kättesaadavus rühma kuulajale ressursi korraga.

- Enne laadi koormusetasakaalustusteenuse **sätted** nuppu **Laadi tasakaalustavad reeglid**. 

- Enne **koormusetasakaalustuseks reeglid** nuppu **Lisa**.

- **Lisa laadimine tasakaalustavad reeglid** tera abil saate konfigureerida laadi tasakaalustamiseks reegel. Saate kasutada järgmisi sätteid: 

| Säte | Väärtus |
| ----- | ----- |
| **Nimi** | Laadi tasakaalustamiseks reeglid tähistav tekst nimi. Näiteks **SQLAlwaysOnEndPointListener**. |
| **Protocol (protokoll)** | **TCP** |
| **Port** | *1433*   |
| **Kirjutamata Port** | *1433*. Pange tähele, et see on keelatud, kuna see reegel kasutab **Ujuv IP (saatja otsest server)**.   |
| **Juures** | Kasutada seda laadi koormusetasakaalustusteenuse nime juures, mille lõite. |
| **Seansi püsivust**  | **Ükski** | 
| **Jõude ajalõpp (minutites)**  | *4* | 
| **Ujuv IP (saatja otsest server)**  | **Lubatud** | 

 >[AZURE.NOTE] Peate enne kõigi sätete kuvamiseks allapoole kerima.

- Klõpsake nuppu **OK**. 

- Azure'i konfigureerib laadi tasakaalustamiseks reegel. Nüüd-liikluse marsruutimiseks kuulajale kättesaadavus rühma sisaldav SQL Server on konfigureeritud laadi koormusetasakaalustusteenuse. 

Selles etapis ressursirühma on laadi koormusetasakaalustusteenuse, ühenduse nii SQL Server masinad. Laadi koormusetasakaalustusteenuse sisaldab IP-aadress ka SQL serveri AlwaysOn ligipääsetavusele rühma kuulajale, nii et kas kohapeal saate vastata kättesaadavus rühmad.

>[AZURE.NOTE] Kui teie SQL-i serverid on kahe omaette piirkondades, korrake samme teiste sama piirkonna. Iga piirkonna jaoks on vaja laadi koormusetasakaalustusteenuse. 

## <a name="configure-the-cluster-to-use-the-load-balancer-ip-address"></a>Laadi koormusetasakaalustusteenuse IP-aadressi kasutada kobar konfigureerimine 

Järgmiseks on kuulajale konfigureerimine klaster ja tuua kuulajale võrgus. Selleks tehke järgmist. 

1. Tõrkesiirdeklastri kuulajale ligipääsetavusele rühma loomine 

1. Tuua kuulajale võrgus

## <a name="1-create-the-availablity-group-listener-on-the-failover-cluster"></a>1. tõrkesiirdeklastri kuulajale ligipääsetavusele rühma loomine

Selles etapis tuleb teil käsitsi luua kättesaadavus rühma kuulajale tõrkesiirdeklastri halduri ja SQL Server Management Studio (SSMS).

- Kasutada RDP ühenduse loomiseks Azure virtuaalse masina, mis majutab esmane koopia. 

- Avatud tõrkesiirdeklastri halduri.

- Valige **võrgu** sõlm ja kobar võrgu nimi. Sellise nimega kasutatakse funktsiooni `$ClusterNetworkName` muutuv PowerShelli skripti.

- Laiendage kobar nimi ja klõpsake **rollid**.

- **Rollide** paanil Paremklõpsake kättesaadavus rühma nimi ja valige **Ressursi lisamine** > **Kliendi pääsupunkti**.

- Väljal **nimi** selle uue kuulajale nime loomine ja seejärel kaks korda nuppu **edasi** ja seejärel klõpsake nuppu **valmis**. Too kuulajale või online ressursi sel hetkel.

 >[AZURE.NOTE] Uue kuulajale nimi on kasutavate rakenduste kättesaadavus rühma SQL Serveri andmebaasiga ühenduse võrgu nimi.

- Klõpsake vahekaarti **ressursid** ja seejärel laiendage äsja loodud kliendi pääsupunkti. Paremklõpsake IP ressursi ja klõpsake käsku Atribuudid. Pange tähele nime IP-aadress. Seda nime kasutate selle `$IPResourceName` muutuv PowerShelli skripti.

- **IP** -aadress suvandit **Staatiline IP-aadressid** ja seadke staatiline IP-aadress sama aadress, kui seate Azure'i portaalis laadi koormusetasakaalustusteenuse IP-aadress. Luba NetBIOS selle aadressi ja klõpsake nuppu **OK**. Korrake seda toimingut iga IP ressurss, kui teie lahendus ulatub üle mitme Azure'i VNets. 

- Kobar sõlme, mis praegu majutab esmane koopia, avage mõni administraatoriõigustes PowerShell ISE ja kleepige uue skripti järgmised käsud.

        $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP Address resource name
        $ILBIP = “<X.X.X.X>” # the IP Address of the Internal Load Balancer (ILB). This is the static IP address for the load balancer you configured in the Azure portal.
    
        Import-Module FailoverClusters
    
        Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}

- Värskendage muutujate ja käivitage PowerShelli skripti IP-aadress ja pordi konfigureerimiseks uus kuulajale.

 >[AZURE.NOTE] Kui teie SQL-i serverid on eraldi regioonid, peate käivitama PowerShelli skripti kaks korda. Esimest korda kasutada kobar võrgu nimi, kobar IP ressursinimi ja esimese ressursirühm alla laadida koormusetasakaalustusteenuse IP-aadress. Teist korda kasutada kobar võrgu nimi, kobar IP ressursinimi ja alla laadida teise ressursirühm koormusetasakaalustusteenuse IP-aadress.

Klaster on nüüd kättesaadavus rühma kuulajale ressursi.

## <a name="2-bring-the-listener-online"></a>2 tuua kuulajale võrgus

Kättesaadavus rühma kuulajale ressursi konfigureeritud, saate tuua kuulajale võrgus nii, et luua rakendusi andmebaasidele, mis on kuulajale jaotises kättesaadavus.

- Liikuge tagasi abil tõrkesiirdeklastri halduri. Laiendage **rollid** ja seejärel tõsta kättesaadavus rühma. Vahekaardil **ressursid** paremklõpsake kuulajale nimi ja klõpsake käsku **Atribuudid**.

- Klõpsake vahekaarti **sõltuvused** . Kui loendis on loetletud mitu ressursid, veenduge, et IP-aadressid on või pole ja sõltuvused. Klõpsake nuppu **OK**.

- Paremklõpsake kuulajale nimi ja klõpsake nuppu **Too veebis**.


- Kui kuulajale on veebis, vahekaardil **ressursid** , paremklõpsake kättesaadavus rühma ja klõpsake käsku **Atribuudid**.

- Looge sõltuvus kuulajale nimi ressursi (mitte IP address ressursid nimi). Klõpsake nuppu **OK**.


- Käivitage SQL Server Management Studio ja ühendage peamine koopia.


- Liikuge **AlwaysOn kõrge-saadavus** | **kättesaadavus rühmad** | **kättesaadavus rühma kuulajatele**. 


- Nüüd näete kuulajale nimi, mille olete loonud tõrkesiirdeklastri halduris. Paremklõpsake kuulajale nimi ja klõpsake käsku **Atribuudid**.


- Klõpsake väljal **Port** määrata pordinumber kättesaadavus rühma kuulajale, kasutades funktsiooni $EndpointPort varem kasutatud (1433 oli vaikimisi), seejärel klõpsake nuppu **OK**.

Nüüd on teil SQL serveri kättesaadavusrühma Azure'i virtuaalmasinates ressursi halduri režiimis. 

## <a name="test-the-connection-to-the-listener"></a>Kuulajale ühenduse testimiseks

Ühenduse testimiseks tehke järgmist.

1. RDP SQL Server, mis on virtuaalse samasse võrku, kuid ei ole oma koopia. See võib olla muude SQL serveri klaster.

1. Ühenduse testimiseks kasutada **sqlcmd** kasuliku. Näiteks järgmine skript loob **sqlcmd** ühenduse kaudu Windowsi autentimist kuulajale peamine koopia:

        sqlcmd -S <listenerName> -E

Ühenduse sqlcmd automaatselt ühenduse SQL serveri eksemplar sellest, millist majutab esmane koopia. 

## <a name="guidelines-and-limitations"></a>Juhised ja piirangud

Võtke arvesse järgmisi juhiseid ligipääsetavusele rühma kuulajale Azure sisemine kasutamise kohta laadimine koormusetasakaalustusteenuse.

- Ainult üks sisemine ligipääsetavusele rühma kuulajale on toetatud pilveteenuses kohta, kuna kuulajale on konfigureeritud koormusetasakaalustusteenuse laadimine ja on ainult üks sisemise koormuse koormusetasakaalustusteenuse. Kuid on võimalik luua formaati välise kuulajatele. 

- Laadi koos sisemine koormusetasakaalustusteenuse pääsete kuulajale kaudu sama virtuaalse võrgustikus.
 
