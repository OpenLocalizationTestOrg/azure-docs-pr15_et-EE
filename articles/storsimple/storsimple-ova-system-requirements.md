<properties
   pageTitle="StorSimple virtuaalse massiiv süsteeminõuded"
   description="Lisateavet tarkvara ja võrgu nõuded oma StorSimple virtuaalse massiiv"
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/17/2016"
   ms.author="alkohli"/>

# <a name="storsimple-virtual-array-system-requirements"></a>StorSimple virtuaalse massiiv süsteeminõuded

## <a name="overview"></a>Ülevaade

Selles artiklis kirjeldatakse oma Microsoft Azure'i StorSimple virtuaalse massiiv (tuntud ka kui StorSimple kohapealse virtuaalse või StorSimple virtuaalse seade) ja juurdepääs massiiv salvestusruumi klientidele olulised süsteeminõuded. Soovitame teil üle vaadata teavet hoolikalt enne juurutada oma StorSimple süsteem ja seejärel viidata tagasi vastavalt vajadusele ajal kasutuselevõtu- ja järgmise.

Süsteeminõuded on järgmised.

-   **Tarkvaranõuded salvestusruumi kliendi** - kirjeldab virtualization Toetatud platvormid, veebibrauserite, iSCSI algataja, SMB Kliendid, minimaalne virtuaalse seadme nõuded ja nende opsüsteemide lisanõuded.

-   **Nõuded StorSimple seadme networking** - teave tuleb tulemüüri lubama iSCSI, cloud või halduse liikluse avatud pordid.

Selles artiklis avaldatud StorSimple süsteemi nõuded teave kehtib StorSimple virtuaalse massiivi ainult.

- 8000 sarja seadmed, avage [seadme StorSimple 8000 sarja süsteeminõuded](storsimple-system-requirements.md).
 
- 7000 sarja seadmed, avage [seadme StorSimple 5000-7000 sarja süsteeminõuded](http://onlinehelp.storsimple.com/1_StorSimple_System_Requirements).


## <a name="software-requirements"></a>Tarkvaranõuded

Tarkvaranõuded teenusekomplekti teave toetatud veebibrauserid, SMB versioonid, virtualization platvormid ja minimaalne virtuaalse seadme nõuded.

### <a name="supported-virtualization-platforms"></a>Virtualization Toetatud platvormid


| **Hypervisor** | **Versioon**                          |
|----------------|--------------------------------------|
| Hyper-V        | Windows Server 2008 R2 SP1 ja uuemad versioonid |
| VMware ESXi    | 5,5 ja uuemad versioonid                        |

### <a name="virtual-device-requirements"></a>Virtuaalne seadme nõuded

| **Komponent**                                | **Nõue**            |
|----------------------------------------------|----------------------------|
| Minimaalne arv virtuaalse protsessorite (südamikud) | 4                          |
| Minimaalne muutmälu (RAM)                         | 8 GB                       |
| Vaba ruumi<sup>1</sup>                       | Ketas OS - 80 GB <br></br>Andmete ketas - 8 TB 500 GB|
| Võrgu liidese/liideste minimaalne arv       | 1                          |
| Minimaalne Interneti läbilaskevõime<sup>2</sup>       | 5 Mbps                     |

<sup>1</sup> - õhuke ette valmistatud

<sup>2</sup> - võrgu nõuded võivad erineda sõltuvalt igapäevane andmete muutmine määr. Näiteks kui seade peab päeva jooksul kuni 10 GB või rohkem muudatused tagasi, siis päev varukoopia 5 Mbps kaudu võib kuluda kuni 4,25 tundi (kui andmed ei saa olla tihendatud või deduplicated).

### <a name="supported-web-browsers"></a>Toetatud veebibrauserid

| **Komponent**     | **Versioon** | **Täiendavad nõuded ja märkmed** |
|-------------------|-----------------|-----------------------------------|
| Microsoft Edge    | Uusim versioon  |                                   |
| Internet Exploreris | Uusim versioon  | Testitud Internet Explorer 11  |
| Google Chrome     | Uusim versioon  | Testitud Chrome'i 46             |

### <a name="supported-storage-clients"></a>Toetatud salvestusruumi kliendid 

Järgmised tarkvaranõuded on iSCSI algataja, et juurdepääs teie StorSimple virtuaalse massiiv (iSCSI server on konfigureeritud).

| **Toetatud opsüsteemid** | **Nõutav versioon** | **Täiendavad nõuded/märkmed** |
| --------------------------- | ---------------- | ------------- |
| Windows Server              | 2008R2 SP1, 2012 2012R2 |StorSimple saate luua õhukesed ettevalmistatud ja täielikult ettevalmistatud maht. See ei saa luua osaliselt ettevalmistatud draividel. StorSimple iSCSI maht on toetatud ainult: <ul><li>Lihtne mahud Windows talletab.</li><li>Windowsi NTFS vormingu maht.</li>|


Tarkvara järgmised nõuded on SMB klientidele, et juurdepääs teie StorSimple virtuaalse massiiv (konfigureeritud faili server).

| **SMB versioon** |
|-------------|
| SMB 2.x     |
| SMB 3.0     |
| SMB 3,02    |

> [AZURE.IMPORTANT] Kopeerige või salvestada faile, mis on kaitstud, Windowsi failisüsteemi krüptimine (EFS) StorSimple virtuaalse massiiv failiserverisse; Seetõttu konfiguratsiooni ei toetata. 

## <a name="networking-requirements"></a>Võrgunduse nõuded 

Järgmises tabelis on loetletud pordid, mida on vaja avada tulemüüri iSCSI, SMB, cloud või halduse liikluse lubamiseks. Selles tabelis *sisse* - või *sissetuleva meili* viitab, millest kliendi sissetulevad taotlused juurdepääs seadme suunas. *Välja* või *väljaminevaid* viitab StorSimple seadme saadab andmete väliselt, lisaks juurutamise suunas: näiteks Väljamineva meili Interneti-ühendus.

| **Pordi nr<sup>1</sup>** | **Sisse või välja** | **Pordi ulatus** | **Nõutav**              | **Märkmete**                                                                                                            |
|--------------------------|---------------|----------------|---------------------------|----------------------------------------------------------------------------------------------------------------------|
| TCP 80 (HTTP)            | Välja           | WAN            | Ei                        | Väljamineva pordi kasutatakse värskenduste toomiseks Interneti-ühendus. <br></br>Väljamineva veebiteenuse puhverserveri on kasutaja konfigureeritav. |
| TCP 443 (HTTPS)          | Välja           | WAN            | Jah                       | Väljamineva pordi kasutatakse andmete pilveteenuses. <br></br>Väljamineva veebiteenuse puhverserveri on kasutaja konfigureeritav. |
| UDP 53 (DNS)             | Välja           | WAN            | Mõnel juhul; Märkmete kuvamine | Selle pordi on nõutav ainult juhul, kui teil on Interneti-põhise DNS-serveri kasutamine. <br></br> **Märkus**: kui failiserverisse võtavad, soovitame kohaliku DNS-i serveri.|
| UDP 123 (NTP)            | Välja           | WAN            | Mõnel juhul; Märkmete kuvamine | Selle pordi on vaja ainult siis, kui kasutate Interneti-põhiste NTP-serverist.<br></br> **Märkus:** Failiserverisse võtavad, soovitame aja sünkroonimine oma Active Directory domeenikontrollerid.  |
| TCP 80 (HTTP)           | Rakenduses            | KOHTVÕRGU            | Jah                       | See on sissetulev port kohaliku UI StorSimple seadme jaoks kohaliku juhtimise. <br></br> **Märkus**: juurdepääs kohaliku UI http https automaatselt suunab.|
| TCP 443 (HTTPS)          | Rakenduses            | KOHTVÕRGU            | Jah                       | See on sissetulev port kohaliku UI StorSimple seadme jaoks kohaliku juhtimise.|
| TCP 3260 (iSCSI)         | Rakenduses            | KOHTVÕRGU            | Ei                        | Selle pordi kasutatakse pääsevad andmetele juurde iSCSI.|

<sup>1</sup> sissetuleva pordid pole vaja avada avaliku Interneti.

> [AZURE.IMPORTANT] Tagama, et tulemüüri muutmine või dekrüptida mis tahes SSL-i liikluse StorSimple seadme ja Azure vahel.


### <a name="url-patterns-for-firewall-rules"></a>URL-i mustrid tulemüüri reeglid 

Võrgu administraatorid saavad sageli konfigureerida täpsemad tulemüüri reeglite alusel filtreerimiseks sissetulev URL-i mustrite ja väljaminev liiklus. Oma virtuaalse massiivi ja StorSimple halduri teenuse sõltuvad muude Microsoft rakendusi nagu Azure'i teenus siini, Azure Active Directory juurdepääsu reguleerimine, salvestusruumi kontod ja Microsoft Update'i serverites. Nende rakenduste seostatud URL-i mustreid saab konfigureerida tulemüüri reeglid. See on oluline mõista, et need rakendused seostatud URL-i mustreid saate muuta. See omakorda nõuab võrguadministraatori poole, et jälgida ja värskendage oma StorSimple nimega ja vajadusel tulemüüri reeglid. 

Soovitame teil seada tulemüüri reeglite väljamineva liikluse, põhineb StorSimple fikseeritud IP-aadressi ohtralt enamikel juhtudel. Siiski saate seada täpsemad tulemüüri reeglid, mida on vaja luua turvalist keskkonnas allolevat teavet.

> [AZURE.NOTE] 
> 
> - Kasutatav seade (allikas) IP-d alati olema seatud pilve lubatud võrgu liidesed. 
> - Sihtkoha IP-d peaks olema seatud [Azure andmekeskuse IP-vahemikke](https://www.microsoft.com/en-us/download/confirmation.aspx?id=41653).


| URL-i mustri                                                      | Komponent/funktsioonid                                           |
|------------------------------------------------------------------|---------------------------------------------------------------|
| `https://*.storsimple.windowsazure.com/*`<br>`https://*.accesscontrol.windows.net/*`<br>`https://*.servicebus.windows.net/*`   | StorSimple halduri teenuse<br>Teenuse juurdepääsu juhtimine<br>Azure'i teenus siini|
|`http://*.backup.windowsazure.com`|Seadme registreerimine|
|`http://crl.microsoft.com/pki/*`<br>`http://www.microsoft.com/pki/*`|Serdi tühistamise |
| `https://*.core.windows.net/*`<br>`https://*.data.microsoft.com`<br>`http://*.msftncsi.com` | Azure'i salvestusruumi kontod ja jälgimine |
| `http://*.windowsupdate.microsoft.com`<br>`https://*.windowsupdate.microsoft.com`<br>`http://*.update.microsoft.com`<br> `https://*.update.microsoft.com`<br>`http://*.windowsupdate.com`<br>`http://download.microsoft.com`<br>`http://wustat.windows.com`<br>`http://ntservicepack.microsoft.com`| Microsoft Update serverid<br>                        |
| `http://*.deploy.akamaitechnologies.com`                         |Akamai CDN-ID |
| `https://*.partners.extranet.microsoft.com/*`                    | Tugiteenuste pakett                                                  |
| `http://*.data.microsoft.com `                   | Telemeetria teenuse Windowsis, lugege teemat [klientide kasutuskogemuse ja diagnostika telemeetria värskendus](https://support.microsoft.com/en-us/kb/3068708)                                                  |

## <a name="next-step"></a>Järgmise juhise juurde

-   [Portaali juurutamiseks oma StorSimple virtuaalse massiiv ettevalmistamine](storsimple-ova-deploy1-portal-prep.md)
