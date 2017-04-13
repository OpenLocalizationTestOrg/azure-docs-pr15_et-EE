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

## <a name="backup"></a>Varundus

| Varundamise | Tõrke üksikasjade | Lahendus |
| -------- | -------- | -------|
|Varundus|    Toimingut ei teha, kui VM pole enam olemas. -Peatada virtuaalse masina kaitsmine ilma varukoopia andmete kustutamist. Rohkem üksikasju http://go.microsoft.com/fwlink/?LinkId=808124   |See juhtub siis, kui esmane VM on kustutatud, kuid varukoopia poliitika jätkuvalt otsida VM teha varukoopia. Selle tõrke lahendamiseks tehke järgmist. <ol><li> Looge virtuaalse masina sama nime ja sama ressursirühma nimi [pilvepõhise teenuse nimi],<br>(OR)</li><li> Peatage kaitsmise virtuaalse masina koos või ilma varukoopia andmete kustutamist. [Lisateavet](http://go.microsoft.com/fwlink/?LinkId=808124)</li></ol>|
|Varundus|Võiks suhelda VM agent hetktõmmise olek. -Tagada, et VM on Interneti-ühendus. Ka värskendada VM agent nimetatud veebisaidil http://go.microsoft.com/fwlink/?LinkId=800034 tõrkeotsingu juhend |See tõrge Expression.Error, kui on probleem VM Agent või võrgu juurdepääs Azure'i infrastruktuuri on blokeeritud mingil viisil. Lugege lisateavet silumine üles VM hetktõmmise probleemid.<br> Kui VM agent põhjustab pole probleemidest, taaskäivitage VM. Aeg-ajalt vale VM olek, võivad põhjustada probleeme ja taaskäivitada VM lähtestatakse selle "halb riik"|
|Varundus|    Taastamine teenuste laiendi toiming nurjus. – Veenduge, et uusim virtuaalse masina agent on olemas virtuaalse masina ja töötab teenus. Proovige varundamise ja kui see ei õnnestu, pöörduge Microsofti klienditoe poole.|   See tõrge Expression.error kui VM agent on aegunud. Vaadake "Värskendamine VM Agent" jaotist allpool VM agent värskendada.|
|Varundus |Virtuaalse masina pole olemas. -Palun veenduge, et virtuaalse masina on olemas, või valige muu virtuaalse masina. | See juhtub siis, kui esmane VM on kustutatud, kuid varukoopia poliitika jätkuvalt otsida VM teha varukoopia. Selle tõrke lahendamiseks tehke järgmist. <ol><li> Looge virtuaalse masina sama nime ja sama ressursirühma nimi [pilvepõhise teenuse nimi],<br>(OR)<br></li><li>Peatage virtuaalse masina kaitsmine ilma varukoopia andmete kustutamist. [Lisateavet](http://go.microsoft.com/fwlink/?LinkId=808124)</li></ol>|
|Varundus |Käsu täitmine nurjus. -Teine toiming on parajasti käimas selle üksuse. Oodake, kuni eelmine toiming on lõpule viidud ja proovige uuesti |Mõne olemasoleva varukoopia või VM taastamine töö töötab ja uus töökoht ei saa käivitada olemasoleva töö töötamise ajal.|
| Varundus | Kopeerimise VHDs varukoopia hoidlast aegus – proovige toimingut paar minutit. Kui probleem ei lahene, pöörduge Microsoft Support. | See juhtub siis, kui seal on liiga palju andmeid kopeerida. Kontrollige, kui teil on vähem kui 16 andmete ketast. |
| Varundus | Varundus nurjus sisemine tõrge – proovige toimingut paar minutit. Kui probleem ei lahene, pöörduge Microsoft Support | Saate selle tõrketeate 2 põhjusel: <ol><li> Juurdepääs VM salvestusruumi on ajutine probleem. Kontrollige [Azure'i olek](https://azure.microsoft.com/en-us/status/) kas on mis tahes seotud võrgu/Arvuta/salvestusruumi piirkonna käimas probleemi. Palun proovi uuesti varukoopia postituse probleemi leevendada. <li>Algne VM on kustutatud, ja seetõttu ei saa võtta varukoopia. Kustutatud VM varukoopia andmete säilitamine, kuid varukoopia tõrkeid, võta VM ja valige suvand andmeid hoida. See toiming peatab varunduse ajakava ja ka korduva tõrketeated. |
| Varundus | Ei saanud installida Azure taastamise teenused on valitud üksuse - VM Agent laiendamine eelduseks Azure taastamise teenuste laiendamine. Installige Azure'i VM agent ja uuesti töö registreerimine | <ol> <li>Märkige ruut, kui VM agent on installitud õigesti. <li>Veenduge, et lipu VM config on õigesti seatud.</ol> [Lisateavet vt](#validating-vm-agent-installation) VM agenti installi ja kuidas VM agendi installimise kohta. |
| Varundus | Laiendi installimine nurjus tõrke "COM + ei saa rääkida Microsoft Distributed tehingkoordinaatori | See tähendab tavaliselt, et COM + teenus ei tööta. Pöörduge Microsofti tugiteenuste poole, millega seda küsimust. |
| Varundus | Hetktõmmise toiming nurjus VSS toiming tõrke "seda draiv on lukustatud BitLockeri krüptimise. Peate esmalt avama selle ketas juhtpaneeli kaudu. | BitLockeri välja lülitada kogu draivid VM ja jälgida, kui VSS probleemi lahendamiseks |
| Varundus | On talletatud Premium salvestusruumi virtuaalse kõvaketta virtuaalmasinates ei toetata varundamine | Ükski |
| Varundus | Azure virtuaalse masina ei leitud. | See juhtub siis, kui esmane VM on kustutatud, kuid varukoopia poliitika jätkuvalt otsida VM teha varukoopia. Selle tõrke lahendamiseks tehke järgmist. <ol><li>Looge virtuaalse masina sama nime ja sama ressursirühma nimi [pilvepõhise teenuse nimi], <br>(OR) <li> Keela see VM kaitse nii, et varundamise ei saa luua </ol> |
| Varundus | Virtuaalse masina agent ei esine virtual arvutisse – nõutav eelsete nõutavad VM agent installige ja uuesti töö. | [Lisateavet vt](#vm-agent) VM agenti installi ja kuidas VM agendi installimise kohta. |

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
| Taastamine | Pilveteenuse sisemine tõrge nurjus taastamine | <ol><li>DNS-i sätted on konfigureeritud pilveteenuses, millele soovite taastada. Saate kontrollida <br>$deployment = get-AzureDeployment - teenuse nimi "teenuse nimi"-"Loomine" Get-AzureDns - DnsSettings $deployment pesa. DnsSettings<br>Kui on konfigureeritud aadress, tähendab see, et DNS-i sätted on konfigureeritud.<br> <li>Pilveteenuses, kuhu soovite taastada on konfigureeritud ReservedIP ja olemasoleva VMs pilveteenuses on peatatud olekus.<br>Saate kontrollida pilveteenus on reserveeritud IP jälgima PowerShelli cmdlet-käskude abil.<br>$deployment = get-AzureDeployment - teenuse nimi "teenuse nimi"-"Loomine" $dep pesa. ReservedIPName <br><li>Soovite taastada virtuaalse masina koos järgmised teisiti võrgu sama pilveteenusesse. <br>-Virtuaalmasinates jaotises Laadi koormusetasakaalustusteenuse konfigureerimine (sise- ja väline)<br>-Virtuaalmasinates koos mitme reserveeritud IP-d<br>-Virtuaalmasinates mitu NICs abil<br>Valige uus pilveteenuses UI või vaadake [taastamine kaalutlused](./backup-azure-arm-restore-vms.md/#restoring-vms-with-special-network-configurations) vms koos teisiti võrgu konfiguratsioone</ol> |
| Taastamine | Valitud DNS-i nimi on juba kasutusel – Määrake erinevate DNS-i nimi ja proovige uuesti. | DNS-i nimi siin viitab pilvepõhise teenuse nimi (tavaliselt lõpeb. cloudapp.net). See peab olema kordumatu. Kui tõrge ilmneb, peate valima mõne muu VM nimega taastamisel. <br><br> Pange tähele, et see viga kuvatakse ainult Azure portaali kasutajatele. PowerShelli kaudu taastetoimingu õnnestub, kuna see ainult taastab soovitud ketast ja ei looda VM. Viga kuvatakse ees loomisel VM konkreetselt teie pärast ketta taastamine toiming. |
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

## <a name="troubleshoot-vm-snapshot-issues"></a>VM hetktõmmise probleemide tõrkeotsing
Välja hetktõmmise käsk aluseks salvestusruumi tugineb VM varukoopia. Ei, millel salvestusruumi või viivitus hetktõmmise ülesande täitmine võib nurjuda varukoopia. Hetktõmmis tööülesande tõrke võib põhjustada.

1. Salvestusruumi juurdepääsu blokeeritud NSG<br>
   Lisateavet selle kohta, kuidas [võrgule juurdepääsu lubamine](backup-azure-vms-prepare.md#2-network-connectivity) , kasutades kas IP valge nimekirja salvestusruumi või puhverserveri kaudu.
2.  Sql serveri varundamise konfigureeritud VMs põhjustada hetktõmmise ülesande viivituse <br>
    Vaikimisi VM varundada probleemid VSS täielik varukoopia Windowsi VMs. Klõpsake VMs, mis töötab SQL-i serverite ja Sql serveri varundus on konfigureeritud, see võib põhjustada viivituse hetktõmmise täitmise. Seadke registrivõtme jälgima, kui teil on probleeme varukoopia tõrkeid hetktõmmise probleemide tõttu.

    ```
    [HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT]
    "USEVSSCOPYBACKUP"="TRUE"
    ```
3.  VM olek esitatud valesti, kuna VM on RDP sulgumist.  <br>
    Kui teil on virtual arvuti RDP sulgeda, kontrollige uuesti sisse portaali VM olek kuvatakse õigesti. Kui ei, siis sulgege VM portaalis VM armatuurlaua "Sulgumist" suvandi abil.
4.  Kui rohkem kui neli VM osa sama pilvepõhine teenus, konfigureerida etapil varunduse ajal seega pole rohkem kui neli VM varukoopiaid käivitatud korraga mitu varukoopia poliitika. Proovige levinud varukoopia algusaegade tunnis lahku vahel. 
5.  VM töötab veebisaidil kõrge CPU/mälu.<br>
    Kui suur CPU töötab virtuaalse masina usage(>90%) või mälu, hetktõmmise tööülesanne on ootele viivitada ning lõpuks kuvatakse saab aegunud. Proovige sellisel nõudmisel varukoopia.

<br>

## <a name="networking"></a>Võrgunduse
Nagu kõik laiendid, peate varundamise laiendi avaliku Interneti töötamiseks. Avaliku Interneti-ühendus ei ole saab näidata mitmel viisil:

- Laiendi install võib nurjuda
- Varukoopia toiminguid (nt ketta hetktõmmise) võib nurjuda
- Varukoopia toimingu oleku kuvamine võib nurjuda

Avaliku Interneti-aadresside lahendamise vaja on ära toodud [siin](http://blogs.msdn.com/b/mast/archive/2014/06/18/azure-vm-provisioning-stuck-on-quot-installing-extensions-on-virtual-machine-quot.aspx). Peate selle VNET DNS-i konfiguratsioone ja veenduge, et Azure'i URI-d saab lahendada.

Kui nime eraldusvõime on tehtud õigesti, Azure'i IP-d juurdepääsu peab olema. Azure'i taristu avamiseks tehke mõnda järgmistest toimingutest.

1. Valge Azure andmekeskuse IP-vahemikke.
    - [Azure'i andmekeskuse IP-d](https://www.microsoft.com/download/details.aspx?id=41653) olema whitelisted soovitud loendi saamiseks.
    - Blokeeringu IP-d, [I New-NetRoute](https://technet.microsoft.com/library/hh826148.aspx) cmdlet-käsu abil. Käivitada sees Azure VM, administraatoriõigustes PowerShelli aknas (Käivita administraatorina).
    - Lisada reeglid on NSG (kui teil on üks koht) on IP-d juurdepääsu lubamiseks.
2. Saate luua HTTP-liikluse meilivoo jaoks tee
    - Kui teil on mõni võrgu piirang kohas (võrgu turberühma, näiteks) juurutamine abil saate marsruutida liiklust HTTP-puhverserver. HTTP-puhverserver serveri juurutamine juhised leiate [siit](backup-azure-vms-prepare.md#2-network-connectivity).
    - Lisada reeglid on NSG (kui teil on üks koht) juurdepääsetav Interneti kaudu HTTP-puhverserver.

>[AZURE.NOTE] DHCP peab olema lubatud sees Külastajate IaaS VM varundamiseks töötamiseks.  Kui teil on vaja privaatne staatiline IP, peaksite konfigureerima selle platvormi kaudu. DHCP suvandi sees VM olema lubatud vasakule.
Saate vaadata [sisemise privaatne staatiline seadmise](../virtual-network/virtual-networks-reserved-private-ip.md)kohta lisateavet.
