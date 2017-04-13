<properties
    pageTitle="Enne juurutamist Azure virnas POC | Microsoft Azure'i"
    description="Saate vaadata Azure'i virnas POC (teenuse administraator) keskkonna ja riistvara."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/12/2016"
    ms.author="erikje"/>

# <a name="azure-stack-deployment-prerequisites"></a>Azure'i virnas juurutamise eeltingimused

Enne juurutamist Azure virnas POC ([Tõendada, mõistet](azure-stack-poc.md)), veenduge, et teie arvuti vastab järgmistele nõuetele.
Funktsiooni POC Technical Preview 2 juurutamise nõuded on samad, mis on vaja Technical Preview 1. Seetõttu saab kasutada sama riistvara, mida kasutasite eelmises ühe-väljal Eelvaade.

## <a name="hardware"></a>Riistvara

| Komponent | Miinimumväärtus  | Soovitatav |
|---|---|---|
| Kettadraivide: operatsioonisüsteem | 1 OS ketast vähemalt 200 GB saadaval süsteemi sektsiooni (SSD või HDD) | 1 OS ketast vähemalt 200 GB saadaval süsteemi sektsiooni (SSD või HDD) |
| Kettadraivide: Azure'i virnas POC üldteavet | 4 ketast. Iga ketta pakub vähemalt 140 GB mahuga (SSD või HDD). Kõik saadaolevad ketast kasutatakse. | 4 ketast. Iga ketta pakub vähemalt 250 GB mahuga (SSD või HDD). Kõik saadaolevad ketast kasutatakse.|
| Arvutage: CPU | Kahe-turvasoklite: 12 tuuma (kogusumma)  | Kahe-turvasoklite: 16 tuuma (kogusumma) |
| Arvutage: mälu | 96 GB RAM-I  | 128 GB RAM-I |
| Arvutage: BIOS | Hyper-V lubatud (mis liist toetab)  | Hyper-V lubatud (mis liist toetab) |
| Võrgu: NIC | Windows Server 2012 R2 sertimine nõutavaid NIC; pole vaja erifunktsioone | Windows Server 2012 R2 sertimine nõutavaid NIC; pole vaja erifunktsioone |
| HW logo sertimine | [Windows Server 2012 R2 sertifitseeritud](http://windowsservercatalog.com/results.aspx?&chtext=&cstext=&csttext=&chbtext=&bCatID=1333&cpID=0&avc=79&ava=0&avq=0&OR=1&PGS=25&ready=0) |[Windows Server 2012 R2 sertifitseeritud](http://windowsservercatalog.com/results.aspx?&chtext=&cstext=&csttext=&chbtext=&bCatID=1333&cpID=0&avc=79&ava=0&avq=0&OR=1&PGS=25&ready=0)|

**Andmete kettale konfigureerimine:** Kõik andmed draivid peab olema sama tüüpi (kõik SAS või kõik SATA) ja mahtu. SAS kettadraivide kasutamisel on kettadraivide peab olema lisatud ühe tee (pole MPIO mitme tee tugi on esitatud) kaudu.

**HBA konfiguratsiooni suvandid**
 
- (Eelistatud) Lihtne HBA
- RAID HBA-adapterit tuleb konfigureerida "läbida" režiimis
- RAID HBA – ketast peaks olema konfigureeritud ketta, RAID 0

**Tippige toetatud siini ja meedia kombinatsioonid**

-   SATA HDD

-   SAS HDD

-   RAID HDD

-   RAID SSD (kui meediumi tüüp on määramata tundmatu\*)

-   SATA SSD + SATA HDD

-   SAS SSD + SAS HDD

\*Läbiv võimalus ilma RAID kontrollerid ei tuvasta meediumi tüüp. Sellise kontrollerid märkimine nii HDD ja SSD väärtuseks määramata. Sel juhul kasutatakse funktsiooni SSD püsiv hoidla asemel vahemällu seadmed. Seega saate juurutada Microsoft Azure'i virnas POC nende SSD.

**Näide HBAs**: LSI 9207-8i, LSI-9300-8i või LSI-9265-8i läbiv režiimis

Valimi OEM konfiguratsioone on saadaval.

## <a name="operating-system"></a>Operatsioonisüsteem

| | **Nõuded**  |
|---|---|
| **Opsüsteemi versioon** | Windows Server 2012 R2 või uuem versioon. Operatsioonisüsteemi versiooni ei ole kriitiline enne juurutamise käivitub, kui te saate käivitada hosti arvuti sisse VHD, mis sisaldub Azure'i virnas installi zip. OS ja kõik nõutavad paikade on juba integreeritud pilt. Ärge kasutage mis tahes klahvid esinemisvõimaluse Windows Server, kasutatakse selle POC aktiveerimiseks.|

## <a name="deployment-requirements-check-tool"></a>Juurutamise nõudeid märkige tööriist

Pärast opsüsteemi installimist saate kinnitada, et riistvara vastab kõik [Juurutamise kontroll Azure virnas Technical Preview 2](https://gallery.technet.microsoft.com/Deployment-Checker-for-50e0f51b) .



## <a name="microsoft-azure-active-directory-accounts"></a>Microsoft Azure Active Directory kontod

Microsoft Azure'i virnas POC juurutamise peab olema ühendatud Azure. Seetõttu tuleb koostada Microsoft Azure Active Directory kontot enne juurutamist PowerShelli skripti käivitamist. Selle konto muutub globaalne administraator Azure Active Directory rentniku jaoks. Seda kasutatakse ette ja volitatud esindaja rakenduste ja teenuste põhisumma kõikide suhelda Azure Active Directory ja pildi API Azure'i virnas teenuste jaoks. Seda kasutatakse omanik ja vaikimisi pakkuja tellimuse (mille saate hiljem muuta). Te saate sisse logida Azure'i virnas süsteemi haldusportaali selle kontoga.

1. Looge konto Azure AD, mis on vähemalt üks Azure Active Directory directory administraator. Kui teil juba on üks, saate kasutada mis. Muul juhul saate luua ükshaaval tasuta [http://azure.microsoft.com/en-us/pricing/free-trial/](http://azure.microsoft.com/pricing/free-trial/) (Hiina, külastage <http://go.microsoft.com/fwlink/?LinkID=717821> hoopis.)

    [Käivitage PowerShelli skripti juurutamise](azure-stack-run-powershell-script.md#run-the-powershell-deployment-script)juhist 6 identimisteabe kasutamiseks salvestada. Selle *teenuse administraatori* konto saate konfigureerida ja hallata ressursside pilved, Kasutajakontod, rentniku lepingud, kvootide ja hinnakirjad. Portaalis need saate luua veebisaidi pilved, virtuaalse masina privaatne pilved, luua ja kasutajale tellimuste haldamine.

2. Vähemalt üks [Loo](azure-stack-add-new-user-aad.md) konto nii, et saate Azure'i virnas POC rentniku nimega sisse logida.

  	| **Azure Active Directory kontot**  | **Toetatud?** |
  	|---|---| 
  	| Töökoha või kooli kontoga kehtiv avaliku Azure'i tellimus  | Jah |
  	| Microsofti Account kehtiv avaliku Azure'i tellimus  | Ei |
  	| Töökoha või kooli kontoga kehtiv Hiina Azure'i tellimus  | Jah |
  	| Töökoha või kooli kontoga kehtiv USA valitsuse Azure'i tellimus  | Jah |


## <a name="network"></a>Võrgu

### <a name="switch"></a>Üleminek

Üks saadaval port POC masina lülitit.  

Azure'i virnas POC arvuti toetab ühenduse vahetamise juurdepääsu pordi või magistraali port. Pole erifunktsioone on vaja lülitit. Kui kasutate magistraali pordi või kui teil on vaja konfigureerida VLAN ID, tuleb sisestada VLAN ID juurutamise parameetrina. Näited [juurutamise parameetrite loendit](azure-stack-run-powershell-script.md)saate vaadata.

### <a name="subnet"></a>Alamvõrgu

Järgmised alamvõrku POC arvuti ei saa ühendust:
- 192.168.200.0/24
- 192.168.100.0/27
- 192.168.101.0/26
- 192.168.102.0/24
- 192.168.103.0/25
- 192.168.104.0/25

Nende alamvõrku on mõeldud sisemise võrgu Microsoft Azure'i virnas POC keskkonnas.

### <a name="ipv4ipv6"></a>IPv4 ja IPv6

Toetatakse ainult IPv4. Ei saa luua IPv6 võrgu.

### <a name="dhcp"></a>DHCP

Veenduge, et puuduvad DHCP server network, mis ühendab NIC. Kui DHCP ei ole saadaval, saate mõne täiendavad staatiline IPv4 võrgu peale poolt host ette valmistada. Peate esitama IP-aadress ja lüüsi juurutamise parameetrina. Näited [juurutamise parameetrite loendit](azure-stack-run-powershell-script.md)saate vaadata.

### <a name="internet-access"></a>Interneti-ühendus

Azure'i virnas nõuab Interneti-ühendus, otseselt või läbipaistvaid puhverserveri. Azure'i virnas ei toeta konfiguratsiooni veebiteenuse puhverserveri lubamiseks Interneti-ühendus. Host IP- ja uue IP määratud MAS-BGPNAT01 (DHCP või staatiline IP) peab olema võimalus Interneti-ühendus. Klõpsake jaotises domeenid graph.windows.net ja login.windows.net kasutatakse pordid 80 ja 443.

### <a name="telemetry"></a>Telemeetria

Telemeetria andmevoo toetamiseks pordi 443 (HTTPS) tuleb avada teie võrku. Kliendi lõpp-punkti on https://vortex-win.data.microsoft.com.


## <a name="next-steps"></a>Järgmised sammud

[Laadige Azure'i virnas POC juurutamine](https://azure.microsoft.com/overview/azure-stack/try/?v=try)

[Azure'i virnas POC juurutamine](azure-stack-run-powershell-script.md)
