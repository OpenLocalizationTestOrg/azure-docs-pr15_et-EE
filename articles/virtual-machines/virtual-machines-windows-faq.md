<properties
    pageTitle="KKK Windowsi vms | Microsoft Azure'i"
    description="Alt leiate vastused mõnda levinud küsimustele Windows virtuaalmasinates loodud ressursihaldur mudel."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="cynthn"/>

# <a name="frequently-asked-question-about-windows-virtual-machines"></a>Korduma kippuvad küsimused Windows Virtuaalmasinates 


Selles artiklis käsitletakse mõnda levinud küsimustele Windows virtuaalmasinates Azure ressursihaldur juurutamise mudeli abil loodud. Selles teemas Linuxi versiooni, vt [korduma kippuvad küsimused Linux Virtuaalmasinates](virtual-machines-linux-faq.md)

## <a name="what-can-i-run-on-an-azure-vm"></a>Mida saab käitada on Azure VM?

Kõik tellijad saab käitada serveritarkvara Azure virtuaalne arvutis. Töötab serveritarkvara Microsoft Azure tugiteenuste poliitika kohta leiate teavet teemast [Microsofti serveritarkvara toetavad Azure'i Virtuaalmasinates](https://support.microsoft.com/kb/2721672)

Teatud versiooni Windows 7 ja Windows 8.1 on saadaval MSDN-i Azure kasu tellijad ja MSDN-i arendaja ja testida grupikindlustusleping tellijad, arendus ja testimine toimingute tegemiseks. Lisateavet leiate teemast [Windowsi kliendi piltide MSDN tellijad](http://azure.microsoft.com/blog/2014/05/29/windows-client-images-on-azure/)ja piirangud, sh juhendavad. 


## <a name="how-much-storage-can-i-use-with-a-virtual-machine"></a>Kui palju mäluruumi saab kasutada koos virtuaalse masina?

Iga andmete ketta võib olla kuni 1 TB. Andmete ketast, saate kasutada arvu sõltub virtuaalse masina suurus. Lisateavet leiate teemast [Virtuaalmasinates suurused](virtual-machines-windows-sizes.md).

Azure'i salvestusruumi konto pakub operatsioonisüsteemi ketta ja mis tahes andmete ketast. Iga ketta on salvestatud lehe bloobimälu .vhd fail. Hinnad üksikasjad, vt [Salvestusruumi hinnad üksikasjad](https://azure.microsoft.com/pricing/details/storage/).


## <a name="how-can-i-access-my-virtual-machine"></a>Kuidas pääseb minu virtuaalse masina?

Kaugtöölaua ühendus (RDP) kasutamine Windows VM remote ühendus luua. Juhised leiate teemast [ühenduse loomiseks ja kus töötab Windows Azure'i virtuaalarvuti sisse logida](virtual-machines-windows-connect-logon.md). Kuni kahe samaaegseid ühendused on toetatud, v.a juhul, kui server on konfigureeritud kaugtöölaua teenuste seansi korraldajaga.  


Kui teil on probleeme kaugtöölaua, lugege teemat [tõrkeotsing kaugtöölaua ühenduse abil Windowsi-põhiste Azure virtuaalse masina](virtual-machines-windows-troubleshoot-rdp-connection.md). 

Kui olete tuttav Hyper-V, võib otsite tööriista, mis on sarnane VMConnect. Azure'i ei paku sarnane tööriist, kuna konsooli virtuaalse masina juurdepääsu ei toetata.

## <a name="can-i-use-the-temporary-disk-the-d-drive-by-default-to-store-data"></a>Ajutiste ketas (D: ketas vaikimisi) saab kasutada andmete talletamiseks?

Ärge kasutage ajutist ketas andmete talletamiseks. See on ainult ajutine salvestusruum, nii, et soovite kaotada andmeid, mida ei saa taastada. Andmete kaotsimineku võib tekkida siis, kui mõni muu host viib virtuaalse masina. Suuruse muutmise virtuaalse masina, selle hosti või riistvara tõrke hosti värskendamise on mõned põhjused, mis on virtuaalse masina võib teisaldada.

Kui teil on rakendus, mida tuleb kasutada D: draivi nime, saate määrata tähti, nii, et ajutine ketas kasutab millegi muuga kui number D:. Juhised leiate teemast [muutmine draivi täht ajutine Windowsi jaoks](virtual-machines-windows-classic-change-drive-letter.md).

## <a name="how-can-i-change-the-drive-letter-of-the-temporary-disk"></a>Kuidas muuta draivi täht ajutine ketas?

Saate muuta selle lehe faili teisaldamine ja ümbermääramist tähti, kuid peate veenduge, et saate teha seda kindlas järjekorras. Juhised leiate teemast [muutmine draivi täht ajutine Windowsi jaoks](virtual-machines-windows-classic-change-drive-letter.md).

## <a name="can-i-add-an-existing-vm-to-an-availability-set"></a>Saate lisada mõne olemasoleva VM on kättesaadavus Sea?

Ei. Kui soovite oma VM on kättesaadavus komplekti osana, peate looma VM määratud jooksul. Praegu pole täiesti VM lisamiseks saadavus seadmine pärast selle loomist.

## <a name="can-i-upload-a-virtual-machine-to-azure"></a>Kas virtuaalse masina saate üles laadida Azure?

Jah. Juhised leiate [Azure'i Windows VM pildi üleslaadimine](virtual-machines-windows-upload-image.md)

## <a name="can-i-resize-the-os-disk"></a>Suurust saab muuta OS ketas?

Jah. Juhised leiate teemast [OS ketas, virtuaalse masina Azure'i ressursi jaotise laiendamiseks](virtual-machines-windows-expand-os-disk.md).

## <a name="can-i-copy-or-clone-an-existing-azure-vm"></a>Saate kopeerida või klooni mõne olemasoleva Azure VM?

Jah. Juhised leiate teemast [Windowsi virtuaalse masina ressursihaldur juurutamise mudeli koopia loomine](virtual-machines-windows-vhd-copy.md).

## <a name="why-am-i-not-seeing-canada-central-and-canada-east-regions-through-azure-resource-manager"></a>Miks ma ei näe Kanada Kesk- ja Kanada Ida piirkondade Azure'i ressursihaldur kaudu?

Kaks uut aladele Kanada Kesk- ja Kanada Ida ei ole automaatselt registreeritud virtuaalse masina loomiseks olemasoleva Azure'i tellimused. Selle registreerimise tehakse automaatselt virtuaalse masina juurutamisel muude alale, kasutades Azure ressursihaldur Azure portaali kaudu. Pärast virtuaalse masina juurutamist Azure regioon, uued piirkonnad peaks olema saadaval edaspidised virtuaalmasinates.

## <a name="does-azure-support-linux-vms"></a>Azure'i ei toeta Linux VMs?

Jah. Linux VM proovida kiireks loomiseks vaadake teemat [Linux VM Azure portaali loomine](virtual-machines-linux-quick-create-portal.md).

## <a name="can-i-add-a-nic-to-my-vm-after-its-created"></a>Saate lisada mõne NIC minu VM pärast selle loomist?

Ei. Lisada mõne NIC saab teha ainult loomise ajal.

## <a name="are-there-any-computer-name-requirements"></a>On ka arvuti nimi nõuded?

Jah. Arvuti nimi võib olla kuni 15 märki. Lisateavet [taristu nime juhised](virtual-machines-windows-infrastructure-naming-guidelines.md) ümber nimetamine oma ressursse.

## <a name="what-are-the-username-requirements-when-creating-a-vm"></a>Mis on kasutajanimi VM loomisel?

Kasutajanimede võib olla kuni 20 tähemärki pikk ning perioodi lõpus ei tohi ("."). 

Järgmised kasutajanimed ei ole lubatud:

<table>
    <tr>
        <td style="text-align:center">administraator </td><td style="text-align:center"> administraator </td><td style="text-align:center"> kasutaja </td><td style="text-align:center"> user1</td>
    </tr>
    <tr>
        <td style="text-align:center">test </td><td style="text-align:center"> user2 </td><td style="text-align:center"> test1 </td><td style="text-align:center"> user3</td>
    </tr>
    <tr>
        <td style="text-align:center">admin1 </td><td style="text-align:center"> 1 </td><td style="text-align:center"> 123 </td><td style="text-align:center"> lisamine</td>
    </tr>
    <tr>
        <td style="text-align:center">actuser  </td><td style="text-align:center"> adm </td><td style="text-align:center"> Admin2 </td><td style="text-align:center"> ASPNET</td>
    </tr>
    <tr>
        <td style="text-align:center">varundus </td><td style="text-align:center"> konsooli </td><td style="text-align:center"> David </td><td style="text-align:center"> külalisena</td>
    </tr>
    <tr>
        <td style="text-align:center">John </td><td style="text-align:center"> omanik </td><td style="text-align:center"> juur </td><td style="text-align:center"> Server</td>
    </tr>
    <tr>
        <td style="text-align:center">SQL-i </td><td style="text-align:center"> tugi </td><td style="text-align:center"> support_388945a0 </td><td style="text-align:center"> sys</td>
    </tr>
    <tr>
        <td style="text-align:center">test2 </td><td style="text-align:center"> testi3 </td><td style="text-align:center"> user4 </td><td style="text-align:center"> user5</td>
    </tr>
</table>

## <a name="what-are-the-password-requirements-when-creating-a-vm"></a>Mis on parooli loomisel VM?

Paroolide tuleb 8-123 märki ja vastavad 3, 4 ja keerukuse järgmistele nõuetele välja:

- Lower märgid
- Ülemine märgid
- On numbri
- On erimärgi (Regex vastavad [\W_])

Järgmised paroolid ei ole lubatud:

<table>
    <tr>
        <td style="text-align:center">abc@123</td><td style="text-align:center">P@$$w0rd</td><td style="text-align:center">P@ssw0rd</td><td style="text-align:center">P@ssword123</td><td style="text-align:center">PA$ $word</td>
    </tr>
    <tr>
        <td style="text-align:center">pass@word1</td><td style="text-align:center">Parooli!</td><td style="text-align:center">Parool1</td><td style="text-align:center">Password22</td><td style="text-align:center">ILOVEYOU!</td>
    </tr>
</table>
