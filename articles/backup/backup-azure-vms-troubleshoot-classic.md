<properties
    pageTitle="Azure virtuaalse masina varundamise tõrkeotsing | Microsoft Azure'i"
    description="Varundus ja taaste, Azure'i virtuaalmasinates tõrkeotsing"
    services="backup"
    documentationCenter=""
    authors="trinadhk"
    manager="shreeshd"
    editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/26/2016"
    ms.author="trinadhk;jimpark;"/>


# <a name="troubleshoot-azure-virtual-machine-backup"></a>Azure virtuaalse masina varundamise tõrkeotsing

> [AZURE.SELECTOR]
- [Taastamise teenuste hoidla](backup-azure-vms-troubleshoot.md)
- [Varukoopiate hoidla](backup-azure-vms-troubleshoot-classic.md)

Saate tõrkeotsing ilmnes järgmises tabelis loetletud Azure'i varundamise teabega.

## <a name="discovery"></a>Tuvastamine

| Varundamise | Tõrke üksikasjade | Lahendus |
| -------- | -------- | -------|
| Tuvastamine | Uued üksused – Microsoft Azure varukoopia ilmnes ja sisemine tõrge leidmine nurjus. Oodake mõni minut ja proovige seejärel uuesti. | 15 minuti pärast uuesti on discovery protsess.
| Tuvastamine | Uued üksused – leidmine nurjus teise Discovery toiming on juba on pooleli. Oodake, kuni on Discovery praeguse toimingu lõpuleviimist. | Ükski |

## <a name="register"></a>Registreeru
| Varundamise | Tõrke üksikasjade | Lahendus |
| -------- | -------- | -------|
| Registreeru | Andmete ketast manustatud virtuaalse masina arv ületab toetatud arvu piirangu – palun eemaldada mõne andmete ketta selle virtual arvuti ja proovige uuesti. Azure'i varukoopiad toetab kuni 16 andmete ketast manustatud Azure'i virtuaalarvuti varundamine | Ükski |
| Registreeru | Microsoft Azure varukoopia ilmnes sisemine tõrge - oodake paar minutit ja proovige siis toimingut korrata. Kui probleem ei lahene, pöörduge Microsoft Support. | Saate selle tõrke tõttu ühte VM järgmist konfiguratsiooni ei toetata Premium LRS. <br> Premium mälu VMs saab varundada taastamise teenuste hoidla abil. [Lisateave](backup-introduction-to-azure-backup.md/#back-up-and-restore-premium-storage-vms) |
| Registreeru | Installige Agent toiming ajalõpp registreerimine nurjus | Kontrollige, kas virtuaalse masina OS versioon on toetatud. |
| Registreeru | Käsu täitmine nurjus - teise toiming on pooleli selle üksusega. Oodake, kuni eelmine on lõpetatud | Ükski |
| Registreeru | On talletatud Premium salvestusruumi virtuaalse kõvaketta virtuaalmasinates ei toetata varundamine | Ükski |
| Registreeru | Virtuaalse masina agent ei esine virtual arvutisse – installige nõutav eelse nõutavad VM agent ja uuesti töö. | [Lisateavet vt](#vm-agent) VM agenti installi ja kuidas VM agendi installimise kohta. |

## <a name="backup"></a>Varundus

| Varundamise | Tõrke üksikasjade | Lahendus |
| -------- | -------- | -------|
| Varundus | Võiks suhelda VM agent hetktõmmise olek. Hetktõmmis VM sub tööülesande aegus. -Palun vaadake tõrkeotsingu juhend probleemi lahendamiseks. | See tõrge Expression.Error, kui on probleem VM Agent või võrgu juurdepääs Azure'i infrastruktuuri on blokeeritud mingil viisil. Lisateavet [silumine üles VM hetktõmmise probleemid](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md). <br> Kui VM agent põhjustab pole probleemidest, taaskäivitage VM. Aeg-ajalt vale VM olek, võivad põhjustada probleeme ja taaskäivitada VM lähtestatakse selle "halb riik" |
| Varundus | Varundus nurjus sisemine tõrge – proovige toimingut paar minutit. Kui probleem ei lahene, pöörduge Microsoft Support | Kontrollige, kas on ajutine probleem juurdepääsu VM mälu. Kontrollige [Azure'i olek](https://azure.microsoft.com/en-us/status/) kas on mis tahes seotud võrgu/Arvuta/salvestusruumi piirkonna käimas probleemi. Palun proovi uuesti varukoopia postituse probleemi leevendada. |
| Varundus | Toimingut ei teha, kui VM pole enam olemas. | Varundus ei saa täita, nagu on konfigureeritud lubama varundamise VM on kustutatud. Lõpeta edasise varukoopiate minnes kaitstud vaates üksused, kaitstud üksuse valimine ja klõpsake nuppu Peata kaitse. Saate säilitada andmete säilitamise varundamise andmete suvandi valimisel. Saate hiljem jätkata kaitse jaoks, klõpsates selle virtuaalse masina sisse konfigureerimine kaitse registreeritud üksuste kuvamine|
| Varundus | Ei saanud installida Azure taastamise teenused on valitud üksuse - VM Agent laiendamine eelduseks Azure taastamise teenuste laiendamine. Installige Azure'i VM agent ja uuesti töö registreerimine | <ol> <li>Märkige ruut, kui VM agent on installitud õigesti. <li>Veenduge, et lipu VM config on õigesti seatud.</ol> [Lisateavet vt](#validating-vm-agent-installation) VM agenti installi ja kuidas VM agendi installimise kohta. |
| Varundus | Käsu täitmine nurjus - teise toiming on parajasti käimas selle üksusega. Oodake, kuni eelmine toiming on lõpule viidud ja proovige uuesti | Mõne olemasoleva varukoopia või VM taastamine töö töötab ja uus töökoht ei saa käivitada olemasoleva töö töötamise ajal. |
| Varundus | Laiend installimine nurjus tõrke "COM + ei saa rääkida Microsoft Distributed tehingkoordinaatori | See tähendab tavaliselt, et COM + teenus ei tööta. Pöörduge Microsofti tugiteenuste poole, millega seda küsimust. |
| Varundus | Hetktõmmise toiming nurjus VSS toiming tõrke "seda draiv on lukustatud BitLockeri krüptimise. Peate esmalt avama selle ketas juhtpaneeli kaudu. | BitLockeri välja lülitada kogu draivid VM ja jälgida, kui VSS probleemi lahendamiseks |
| Varundus | On talletatud Premium salvestusruumi virtuaalse kõvaketta virtuaalmasinates ei toetata varundamine | Ükski |
| Varundus | Azure virtuaalse masina ei leitud. | See juhtub siis, kui esmane VM on kustutatud, kuid varukoopia poliitika jätkuvalt otsida VM teha varukoopia. Selle tõrke lahendamiseks tehke järgmist. <ol><li>Looge virtuaalse masina sama nime ja sama ressursirühma nimi [pilvepõhise teenuse nimi], <br>(OR) <li> Keela see VM kaitse nii, et edaspidised varukoopiate ei saada käivitatud. </ol> |
| Varundus | Virtuaalse masina agent pole virtuaalse masina – installige nõutav eelsete nõutavad VM agent ja uuesti töö. | [Lisateavet vt](#vm-agent) VM agenti installi ja kuidas VM agendi installimise kohta. |

## <a name="jobs"></a>Tööde haldamine
| Toiming | Tõrke üksikasjade | Lahendus |
| -------- | -------- | -------|
| Katkestamine | Loobumise ei toetata selle töö tüüp – oodake, kuni töö lõpulejõudmist. | Ükski |
| Katkestamine | Töö pole cancelable olekus – oodake, kuni töö lõpulejõudmist. <br>VÕI<br> Valitud töö pole cancelable olekus – oodake töö lõpuleviimiseks.| Tõenäoliselt töö peaaegu lõpetamist; Oodake, kuni töö lõpulejõudmist |
| Katkestamine | Töö ei saa tühistada, kuna see ei ole pooleli - tühistamine on toetatud ainult töökohta, mis on pooleli. Palun proovi tühistamine kohta on tekstivormingus töö. | See juhtub tõttu ajutine olek. Oodake, kuni minut ja proovige uuesti tühistamine |
| Katkestamine | Töö - katkestada nurjus Palun oodake, kuni tööd lõpule. | Ükski |


## <a name="restore"></a>Taastamine
| Toiming | Tõrke üksikasjade | Lahendus |
| -------- | -------- | -------|
| Taastamine | Pilveteenuse sisemine tõrge nurjus taastamine | <ol><li>DNS-i sätted on konfigureeritud pilveteenuses, millele soovite taastada. Saate kontrollida <br>$deployment = get-AzureDeployment - teenuse nimi "teenuse nimi"-"Loomine" Get-AzureDns - DnsSettings $deployment pesa. DnsSettings<br>Kui on konfigureeritud aadress, tähendab see, et DNS-i sätted on konfigureeritud.<br> <li>Pilveteenuses, kuhu soovite taastada on konfigureeritud ReservedIP ja olemasoleva VMs pilveteenuses on peatatud olekus.<br>Saate kontrollida pilveteenus on reserveeritud IP jälgima PowerShelli cmdlet-käskude abil.<br>$deployment = get-AzureDeployment - teenuse nimi "teenuse nimi"-"Loomine" $dep pesa. ReservedIPName <br><li>Soovite taastada virtuaalse masina koos järgmised teisiti võrgu sama pilveteenusesse. <br>-Virtuaalmasinates jaotises Laadi koormusetasakaalustusteenuse konfigureerimine (sise- ja väline)<br>-Virtuaalmasinates koos mitme reserveeritud IP-d<br>-Virtuaalmasinates mitu NICs abil<br>Valige uus pilveteenuses UI või vaadake [taastamine kaalutlused](./backup-azure-restore-vms.md/#restoring-vms-with-special-network-configurations) vms koos teisiti võrgu konfiguratsioone</ol> |
| Taastamine | Valitud DNS-i nimi on juba kasutusel – Määrake erinevate DNS-i nimi ja proovige uuesti. | DNS-i nimi siin viitab pilvepõhise teenuse nimi (tavaliselt lõpeb. cloudapp.net). See peab olema kordumatu. Kui tõrge ilmneb, peate valima mõne muu VM nimega taastamisel. <br><br> See tõrketeade kuvatakse ainult Azure portaali kasutajatele. PowerShelli kaudu taastetoimingu õnnestub, sest see ainult taastab soovitud ketast ja ei looda VM. Viga kuvatakse ees loomisel VM konkreetselt teie pärast ketta taastamine toiming. |
| Taastamine | Määratud virtuaalse võrgukonfiguratsioon pole õige – Määrake erinevate virtuaalse võrgukonfiguratsioon ja proovige uuesti. | Ükski |
| Taastamine | Määratud pilveteenuses kasutab reserveeritud IP, mis ei vasta virtuaalse masina taastatakse konfiguratsioon – Määrake erinevate pilveteenus, mis ei kasuta reserveeritud IP, või valida mõne muu taastamine punkti taastamine. | Ükski |
| Taastamine | Pilveteenuses jõudnud piirang Sisestuskeel lõpp-punkti - toimingut, määrates erinevate pilveteenuses või olemasoleva lõpp abil. | Ükski |
| Taastamine | Varukoopia hoidla ja target salvestusruumi konto on kaks eri regioonide – veenduge, et salvestusruumi konto määratud taastetoimingu on Azure sama piirkonna varukoopiate hoidla. | Ükski |
| Taastamine | Salvestusruumi konto määratud taastetoimingu ei ole toetatud - ainult Standard Basic salvestusruumi kontod kohalikult liigsete või geo liigsete dispersioonanalüüs sätted toetatud. Valige toetatud salvestusruumi konto | Ükski |
| Taastamine | Salvestusruumi konto taastamine toimingu jaoks määratud tüüp pole võrgus – veenduge, et määratud taastetoimingu salvestusruumi konto on veebis | See võib juhtuda Azure Storage või mõne katkestuste tõttu siirdamiseks tõrke tõttu. Valige salvestusruumi teisele kontole. |
| Taastamine | Ressursirühm kvoodi jõudnud – palun Azure portaali mõne ressursi rühma kustutada või Azure tugiteenuste suurendamiseks seotud piirangud. | Ükski |
| Taastamine | Valitud alamvõrgu pole – valige alamvõrku, mis on olemas | Ükski |


## <a name="policy"></a>Poliitika
| Toiming | Tõrke üksikasjade | Lahendus |
| -------- | -------- | -------|
| Poliitika loomine | Ei saanud luua poliitika - vähendage säilituspoliitika Valikud jätkamiseks poliitika konfigureerimine. | Ükski |


## <a name="vm-agent"></a>VM Agent

### <a name="setting-up-the-vm-agent"></a>Kuidas häälestada VM Agent
Tavaliselt VM Agent on juba olemas VMs, mis on loodud Azure galeriist. Siiski ei oleks virtuaalmasinates, mis migreeritakse kohapealse andmekeskuste installitud VM Agent. Sellise vms VM Agent peab olema installitud otseselt. Lugege lisateavet [VM agent mõne olemasoleva VM installimise](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx)kohta.

Windowsi vms:

- Laadige alla ja installige [agent MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Peate installimise lõpuleviimiseks administraatoriõigused.
- [VM atribuudi värskendamiseks](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) , näitamaks, et ta on installitud.

Linux vms:

- Installige uusim [Linux agent](https://github.com/Azure/WALinuxAgent) github.
- [VM atribuudi värskendamiseks](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) , näitamaks, et ta on installitud.


### <a name="updating-the-vm-agent"></a>VM Agent värskendamine
Windowsi vms:

- Värskendamise VM Agent on sama lihtne nagu [VM Agent kahendfaile](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409)taasinstallimine. Siiski peate tagada ilma varukoopia toiming töötab VM Agent värskendamise ajal.

Linux vms:

- Järgige [Värskendamine Linux VM Agent](../virtual-machines/virtual-machines-linux-update-agent.md).


### <a name="validating-vm-agent-installation"></a>VM agenti installi kontrollimine
Kuidas otsida Windows VMs VM Agent versioon:

1. Azure virtuaalse masina sisse logida ja liikuge kausta, *C:\WindowsAzure\Packages*. Tuleb leida WaAppAgent.exe faili esitus.
2. Paremklõpsake faili, valige **Atribuudid**ja seejärel valige vahekaart **üksikasjad** . Toote versioon välja peaks olema 2.6.1198.718 või uuem versioon





