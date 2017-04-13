<properties
    pageTitle="KKK Linux vms | Microsoft Azure'i"
    description="Alt leiate vastused mõnda levinud küsimustele Linux virtuaalmasinates loodud ressursihaldur mudeliga."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="cynthn"/>

# <a name="frequently-asked-question-about-linux-virtual-machines"></a>Korduma kippuvad küsimused Linux Virtuaalmasinates

Selles artiklis käsitletakse Linux virtuaalmasinates Azure ressursihaldur juurutamise mudeli abil loodud levinud küsimustele. Selles teemas Windowsi versioon, lugege teemat [korduma kippuvad küsimused Windows Virtuaalmasinates](virtual-machines-windows-faq.md)

## <a name="what-can-i-run-on-an-azure-vm"></a>Mida saab käitada on Azure VM?

Kõik tellijad saab käitada serveritarkvara Azure virtuaalne arvutis. Lisateavet leiate teemast [Linuxi Azure-Endorsed üldiste müügijaotustega tutvumine](virtual-machines-linux-endorsed-distros.md)


## <a name="how-much-storage-can-i-use-with-a-virtual-machine"></a>Kui palju mäluruumi saab kasutada koos virtuaalse masina?

Iga andmete ketta võib olla kuni 1 TB. Andmete ketast, saate kasutada arvu sõltub virtuaalse masina suurus. Lisateavet leiate teemast [Virtuaalmasinates suurused](virtual-machines-linux-sizes.md).

Azure'i salvestusruumi konto pakub operatsioonisüsteemi ketta ja mis tahes andmete ketast. Iga ketta on salvestatud lehe bloobimälu .vhd fail. Hinnad üksikasjad, vt [Salvestusruumi hinnad üksikasjad](https://azure.microsoft.com/pricing/details/storage/).


## <a name="how-can-i-access-my-virtual-machine"></a>Kuidas pääseb minu virtuaalse masina?

Sisselogimiseks virtuaalse masina Secure Shell (SSH) kaugtöölaua ühendust luua. Vaadake juhiseid [Windowsi](virtual-machines-linux-ssh-from-windows.md) või [Linuxi ja Mac-arvuti kaudu](virtual-machines-linux-mac-create-ssh-keys.md)ühenduse loomise kohta. Vaikimisi lubab SSH kuni 10 samaaegseid ühendusi. Konfiguratsioonifail redigeerides saate suurendada seda numbrit.


Kui teil on probleeme, lugege teemat [tõrkeotsing Secure Shell (SSH) ühendused](virtual-machines-linux-troubleshoot-ssh-connection.md).


## <a name="can-i-use-the-temporary-disk-devsdb1-to-store-data"></a>Ajutiste ketas (/ arendaja/sdb1) saab kasutada andmete talletamiseks?

Ärge kasutage andmete talletamiseks ajutine ketas (/ arendaja/sdb1). See on olemas ainult ajutiseks säilitamiseks. Andmed, mida ei saa taastada kaotada.


## <a name="can-i-copy-or-clone-an-existing-azure-vm"></a>Saate kopeerida või klooni mõne olemasoleva Azure VM?

Jah. Juhiste saamiseks vaadake, [Kuidas luua Linux virtuaalse masina ressursihaldur juurutamise mudeli koopia](virtual-machines-linux-copy-vm.md).


## <a name="why-am-i-not-seeing-canada-central-and-canada-east-regions-through-azure-resource-manager"></a>Miks ma ei näe Kanada Kesk- ja Kanada Ida piirkondade Azure'i ressursihaldur kaudu?

Kaks uut aladele Kanada Kesk- ja Kanada Ida ei ole automaatselt registreeritud virtuaalse masina loomiseks olemasoleva Azure'i tellimused. Selle registreerimise tehakse automaatselt virtuaalse masina juurutamisel muude alale, kasutades Azure ressursihaldur Azure portaali kaudu. Pärast virtuaalse masina juurutamist Azure regioon, uued piirkonnad peaks olema saadaval edaspidised virtuaalmasinates.


## <a name="can-i-add-a-nic-to-my-vm-after-its-created"></a>Saate lisada mõne NIC minu VM pärast selle loomist?

Ei. Lisada mõne NIC saab teha ainult loomise ajal.


## <a name="are-there-any-computer-name-requirements"></a>On ka arvuti nimi nõuded?

Jah. Arvuti nimi võib olla kuni 64 märgi pikkused. Lisateavet [taristu nime juhised](virtual-machines-linux-infrastructure-naming-guidelines.md) ümber nimetamine oma ressursse.


## <a name="what-are-the-username-requirements-when-creating-a-vm"></a>Mis on kasutajanimi VM loomisel?

Kasutajanimed peavad olema 1 – 64 märki.

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

Paroolide tuleb 6 – 72 märki ja vastavad 3, 4 ja keerukuse järgmistele nõuetele välja:

- Lower märgid
- Ülemine märgid
- On numbri
- On erimärgi (Regex vastavad [\W_])

Järgmised paroolid ei ole lubatud:

<table>
    <tr>
        <td style="text-align:center">abc@123</td>
        <td style="text-align:center">P@$$w0rd</td>
        <td style="text-align:center">P@ssw0rd</td>
        <td style="text-align:center">P@ssword123</td>
        <td style="text-align:center">PA$ $word</td>
    </tr>
    <tr>
        <td style="text-align:center">pass@word1</td>
        <td style="text-align:center">Parooli!</td>
        <td style="text-align:center">Parool1</td>
        <td style="text-align:center">Password22</td>
        <td style="text-align:center">ILOVEYOU!</td>
    </tr>
</table>
