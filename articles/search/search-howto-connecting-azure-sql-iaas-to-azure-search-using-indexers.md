<properties 
    pageTitle="Konfigureerimine indekseerija Azure'i otsingu kaudu ühenduse SQL Server Azure'i virtual arvutis | Microsoft Azure'i | Indexers" 
    description="Krüptitud andmeühenduste lubamine ja konfigureerimine tulemüüri lubama ühendusi SQL Server Azure'i virtual arvutisse (VM) indekseerija Azure'i Otsing kaudu." 
    services="search" 
    documentationCenter="" 
    authors="jack4it" 
    manager="pablocas" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="09/26/2016" 
    ms.author="jackma"/>

# <a name="configure-a-connection-from-an-azure-search-indexer-to-sql-server-on-an-azure-vm"></a>Mõne Azure'i VM indekseerija Azure'i otsingu kaudu ühenduse SQL serveri konfigureerimine

Nagu [Ühenduse Azure'i SQL-andmebaasi Azure otsingu abil indexers](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md#frequently-asked-questions), indexers **SQL Server Azure'i VMs** (või **SQL Azure'i VMs** lühikese) loomine toetab Azure otsing, kuid on mõned turvalisusega seotud eeltingimused esimese hoida. 

**Ülesande kestus:** Umbes 30 minutit, eeldades, et teil on juba installitud serdi VM.

## <a name="enable-encrypted-connections"></a>Krüptitud andmeühenduste lubamine

Azure'i otsing nõuab krüptitud kanali kõik indekseerija taotlused avaliku Interneti-ühenduse kaudu. Selles jaotises on loetletud toimingud selle töö.

1. Kontrollige atribuutide serdi subjekti nimi on Azure VM täielik domeeninimi (FQDN) kinnitamiseks. Atribuutide vaatamiseks saate kasutada tööriista nagu CertUtils või lisandmooduli serdid. Saate FQDN VM blade Essentialsi jaotises väli **avaliku IP-aadress ja DNS-i nimi sildi** [Azure portaali](https://portal.azure.com/).

    - **Ressursihaldur** uuema malli abil loodud vms, vormindatakse FQDN `<your-VM-name>.<region>.cloudapp.azure.com`. 

    - Vanema vms loodud on **klassikaline** VM, vormindatakse FQDN `<your-cloud-service-name.cloudapp.net>`. 

2. SQL serveri kasutada registriredaktori (regedit) serdi konfigureerimine. 

    Kuigi SQL Serveri konfigureerimishaldur kasutatakse sageli selle ülesande jaoks, ei saa kasutada seda stsenaariumi. See ei leia imporditud serdi, kuna VM Azure FQDN ei vasta määratud (See tuvastab domeeni, kas kohalikus arvutis või võrgus domeeni, millele on liitunud) VM FQDN. Kui nimed ei ühti, regedit abil saate määrata serdi.

    - Regedit, otsige üles see registrivõti: `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\[MSSQL13.MSSQLSERVER]\MSSQLServer\SuperSocketNetLib\Certificate`.
     
    Funktsiooni `[MSSQL13.MSSQLSERVER]` osa sõltub versiooni ja eksemplari nimi. 

    - **Serdi** võti väärtuseks **sõrmejälje** SSL-serdi imporditud VM.

    On mitu võimalust saada sõrmejälje, mõned paremini kui teised. Kui kopeerite selle **serdid** lisandmooduli MMC, saate tõenäoliselt valige Märgi [tugi selles artiklis kirjeldatud](https://support.microsoft.com/kb/2023869/), mille tulemuseks on tõrge, kui proovite ühendust nähtamatu üles. Probleemi lahendamiseks on olemas mitu lahendusi. Lihtsaim on Tagasilüke üle ja seejärel tippige esimene märk sõrmejälje eemaldamiseks ees märgi regedit väljale väärtus. Teise võimalusena saate kopeerida soovitud sõrmejälje eri tööriista abil.

3. Teenuse konto õiguste andmine. 

    Veenduge, et SQL Serveri teenuse konto on antud vastav privaatvõti SSL-serdi õigus. Kui te unustada seda toimingut, SQL Server ei käivitu. Saate selle ülesande jaoks lisandmooduli **serdid** või **CertUtils** .

4. SQL Serveri teenuse taaskäivitamiseks.

## <a name="configure-sql-server-connectivity-in-the-vm"></a>SQL serveri ühenduvuse VM konfigureerimine

Kui olete häälestanud krüptitud ühendus nõutav Azure otsing, on konfigureerimise juhised sisemine SQL Server Azure'i VMs. Kui te pole seda veel teinud, on järgmiseks lõpuleviimiseks konfigureerimise abil ühe järgmistest artiklitest:

- **Ressursihaldur** VM teemast [ühenduse loomine abil SQL serveri virtuaalse masina Azure ressursihaldur abil](../virtual-machines/virtual-machines-windows-sql-connect.md). 

- On **klassikaline** VM, teemast [ühenduse loomine abil SQL serveri virtuaalse masina Azure'i klassikaline kohta](../virtual-machines/virtual-machines-windows-classic-sql-connect.md).

Vaadake jaotist "Interneti-ühenduse" artiklite.

## <a name="configure-the-network-security-group-nsg"></a>Võrgu turberühma (NSG) konfigureerimine

See ei ole ebatavalised konfigureerida NSG ja vastavate Azure lõpp-punkti või juurdepääsu kontrolli loend (ACL) oma Azure VM hõlbustamiseks muudele isikutele. Tõenäoliselt olete teinud seda varem lubama oma rakenduse loogika ühenduse loomiseks oma SQL Azure'i VM. See ei erine Azure'i otsingu ühendamiseks oma SQL Azure'i VM. 

Allpool olevad lingid pakuvad juhiseid NSG konfigureerimise VM juurutuste. Kasutage neid juhiseid, et ACL Azure'i otsingu lõpp vastavalt oma IP-aadress.

> [AZURE.NOTE] Tausta, lugege teemat [mis on võrgu turberühma?](../virtual-network/virtual-networks-nsg.md)

- **Ressursihaldur** VM teemast [NSGs ARM juurutuste loomise kohta](../virtual-network/virtual-networks-create-nsg-arm-pportal.md). 

- On **klassikaline** VM, vaadake, [Kuidas luua NSGs klassikaline juurutuste](../virtual-network/virtual-networks-create-nsg-classic-ps.md).

IP tegelemine võivad kujutada ohtu vähe probleeme, mis on lihtne ületada, kui teil on probleem ja võimalikud lahendused. Ülejäänud jaotistes soovitused töötlemise IP-aadresside ACL seotud probleemid.

#### <a name="restrict-access-to-the-search-service-ip-address"></a>Juurdepääsu piiramiseks otsingu teenuse IP-aadress

Soovitame selle juurdepääsu piiramiseks ACL, selle asemel, et teie SQL Azure'i VMs lahti taotlused ühendus oma otsinguteenuse IP-aadress. Saate hõlpsalt teada IP-aadressi pingida FQDN järgi (nt `<your-search-service-name>.search.windows.net`) oma otsingu teenuse.

#### <a name="managing-ip-address-fluctuations"></a>IP address kõikumise haldamine

Kui teie otsinguteenuse on ainult üks otsingu üksus (st ühe koopia ja üks sektsioon), IP-aadress võib muutuda ajal tavalised teenuse taaskäivitamist, lukustage mõne olemasoleva ACL oma otsinguteenuse IP-aadressiga.

Aitab vältida edaspidised ühenduvuse tõrge on kasutada rohkem kui ühe koopia ja ühe sektsiooni Azure'i otsing. Seda tehes suurendab, kuid see lahendab ka IP-aadressi probleem. Azure'i otsinguväljale IP-aadressid ei muuda, kui teil on rohkem kui üks otsingu ühiku.

Teine võimalus on lubab seda ühendust ei õnnestu ja seejärel konfigureerima ACL-ID, klõpsake soovitud NSG. Keskmiselt ootate muutmine iga paari nädala IP-aadressid. Klientidele, kes ei kontrollitud indekseerimine harv alusel, võib see lähenemine otstarbekas.

Kolmanda otstarbekas (kuid ei ole eriti turvaline) lähenemine on määratud IP-aadresside vahemiku Azure piirkonna, kus teie otsinguteenuse ette valmistatud. IP-aadresside vahemikud, kust eraldada avaliku IP-aadresside Azure ressursse loend on avaldatud [Azure'i andmekeskuse IP-vahemikke](https://www.microsoft.com/download/details.aspx?id=41653). 

#### <a name="include-the-azure-search-portal-ip-addresses"></a>Azure'i otsingu portaali IP-aadresside hulka

Kui kasutate Azure portaali loomine indekseerija, peab Azure'i otsingu portaali loogika juurdepääsu oma SQL Azure'i VM ka loomise ajal. Leiate Azure'i otsingu portaali IP-aadressid, pingida `stamp2.search.ext.azure.com`.

## <a name="next-steps"></a>Järgmised sammud

Konfiguratsiooni ära, nüüd saate määrata SQL Server Azure'i VM andmeallikana indekseerija Azure'i otsingu jaoks. Lisateabe saamiseks vaadake [Ühenduse Azure'i SQL-andmebaasi Azure otsimiseks indexers abil](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md) .
