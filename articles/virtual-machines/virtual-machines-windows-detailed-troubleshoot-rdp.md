<properties
    pageTitle="Üksikasjalik Remote'i töölaua tõrkeotsing | Microsoft Azure'i"
    description="Vaadake üle üksikasjalikud tõrkeotsingutoiminguid Remote'i töölaua tõrkeid, kus te ei saa Windows virtuaalmasinates Azure"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="top-support-issue,azure-service-management,azure-resource-manager"
    keywords="ei saa ühendust luua kaugtöölaua, et tõrkeotsing kaugtöölaua, kaugtöölaud ei saa ühendust luua, Remote'i töölaua tõrgete, Remote'i töölaua tõrkeotsing, Remote'i töölaua probleemid
"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="support-article"
    ms.date="09/27/2016"
    ms.author="iainfou"/>

# <a name="detailed-troubleshooting-steps-for-remote-desktop-connection-issues-to-windows-vms-in-azure"></a>Üksikasjalikke juhiseid Windows Azure'i VMs kaugtöölaua ühendus probleemide tõrkeotsing

Selles artiklis antakse diagnoosimine ja keerukate kaugtöölaua vigade jaoks Windowsi-põhiste Azure'i virtuaalmasinates tõrkeotsingujuhiseid.

> [AZURE.IMPORTANT] Kaugtöölaua rohkem levinud tõrgete kõrvaldamiseks veenduge enne jätkamist [põhilised tõrkeotsingu artikkel kaugtöölaua](virtual-machines-windows-troubleshoot-rdp-connection.md) .

Kaugtöölaua tõrketeade, mis sarnaneb, mis tahes kaetud [lihtsa kaugtöölaua tõrkeotsingu juhend](virtual-machines-windows-troubleshoot-rdp-connection.md)kindlaid tõrketeateid võivad ilmneda. Tehke kindlaks teha, miks kaugtöölaua (RDP) klient ei saa luua ühendust Azure VM RDP-teenusega.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Kui vajate rohkem abi, mis tahes hetkel selle artikli, võite pöörduda [Azure'i MSDN-i](https://azure.microsoft.com/support/forums/)ja virnas ületäitumise Foorumid Azure eksperdid. Teise võimalusena saate faili Azure'i tugiteenuse taotluse. [Azure'i tugiteabe veebisait](https://azure.microsoft.com/support/options/) ja klõpsake nuppu **Saada tugi**. Lisateavet selle kohta, kasutades Azure, lugege [Microsoft Azure'i tugi korduma kippuvad küsimused](https://azure.microsoft.com/support/faq/).


## <a name="components-of-a-remote-desktop-connection"></a>Kaugtöölaua ühendus komponendid

Järgmised komponendid on seotud RDP-ühendus:

![](./media/virtual-machines-windows-detailed-troubleshoot-rdp/tshootrdp_0.png)

Enne jätkamist võib aidata vaimselt läbi, mis on muutunud viimase eduka kaugtöölaua ühendus VM. Näiteks:

- VM või pilveteenuses, mis sisaldab (nimetatakse ka virtuaalse IP-aadressi [VIP](https://en.wikipedia.org/wiki/Virtual_IP_address)) VM avaliku IP-aadress on muutunud. RDP tõrge võimalik, et teie DNS-i kliendi vahemälu on veel *vana IP-aadressi* registreeritud DNS-i nimi. DNS-i kliendi vahemälu ja proovige uuesti VM ühendust luua. Või proovige ühendavad uue VIP.
- Muude tootjate rakenduste abil hallata oma kaugtöölaua ühenduse asemel Azure portaali loodud ühendus. Kontrollige, kas rakenduse konfigureerimise sisaldab õigete TCP pordi kaugtöölaua liikluse. See klassikaline virtuaalse masina [Azure portaali](https://portal.azure.com)pordi saate kontrollida, klõpsates soovitud VM sätted > lõpp-punktid.


## <a name="preliminary-steps"></a>Esialgsed sammud

Enne jätkamist üksikasjalik tõrkeotsing

- Värskendustoimingu oleku virtuaalse masina Azure klassikaline portaali või Azure portaali jaoks selge probleemidest.
- Järgige [kiirparandus levinud RDP tõrgete tõrkeotsingu juhend põhilised juhised](virtual-machines-windows-troubleshoot-rdp-connection.md#quick-troubleshooting-steps).


Proovige uuesti VM kaudu kaugtöölaua pärast järgmist.


## <a name="detailed-troubleshooting-steps"></a>Üksikasjalikud juhised tõrkeotsing

Kaugtöölaua klient ei saa jõuda kaugtöölaua teenuse Azure VM tõttu küsimused järgmistest allikatest:

- [Remote'i töölaua klientarvuti](#source-1-remote-desktop-client-computer)
- [Ettevõtte sisevõrgu serva seade](#source-2-organization-intranet-edge-device)
- [Pilvepõhise teenuse lõpp-punkti ja juurdepääsu juhtimine loend (ACL)](#source-3-cloud-service-endpoint-and-acl)
- [Võrgu turberühmad](#source-4-network-security-groups)
- [Windowsi-põhiste Azure'i VM](#source-5-windows-based-azure-vm)

## <a name="source-1-remote-desktop-client-computer"></a>Allikas 1: Remote'i töölaua klientarvuti

Veenduge, et teie arvuti saab teha teisele asutusesiseselt, Windowsi-põhises arvutis kaugtöölaua ühendusi.

![](./media/virtual-machines-windows-detailed-troubleshoot-rdp/tshootrdp_1.png)

Kui te ei saa, kontrollige järgmisi sätteid teie arvutis.

- Sätte kohaliku tulemüür blokeerib kaugtöölaua liikluse.
- Kohalik installitud puhverserveri klienttarkvara, mis takistab kaugtöölaua.
- Kohalik installitud võrgu jälgimise tarkvara, mis takistab kaugtöölaua.
- Muud tüüpi turbetarkvara, mis liikluse jälgimine või kindlat tüüpi liikluse, mis takistab kaugtöölaua lubada/keelata.

Sellisel juhul ajutiselt keelamine tarkvara ja proovige kohapealse arvutiga kaudu kaugtöölaua ühendus luua. Kui leiate tegelik põhjus sellisel viisil, töötada võrguadministraatori poole õige tarkvara sätted lubama kaugtöölaua ühendusi.

## <a name="source-2-organization-intranet-edge-device"></a>Allikas 2: Ettevõtte sisevõrgu serva seade

Veenduge, et arvuti Interneti-ühendus otse saab teha Azure virtuaalne arvuti kaugtöölaua ühendusi.

![](./media/virtual-machines-windows-detailed-troubleshoot-rdp/tshootrdp_2.png)

Kui teil pole arvutis, kus on ühendatud otse Interneti-ühendus, luua ja testige uue Azure virtuaalse masina ressursi rühma või pilvepõhises teenus. Lisateavet leiate teemast [loomine virtuaalse masina opsüsteemi Windows Azure](virtual-machines-windows-hero-tutorial.md). Saate kustutada virtuaalse masina ja ressursirühma või selle pilveteenusesse pärast test.

Kui loote kaugtöölaua ühendus otse Internetiga ühendatud arvuti, kontrollige oma ettevõtte sisevõrgu serva seadme jaoks.

- Mõne sisemise tulemüüri blokeerimise HTTPS Interneti-ühenduste.
- Kaugtöölaua ühenduste vältimine puhverserverit.
- Sissetungi tuvastamise või võrgu jälgimise tarkvara töötab teie serv võrku, mis takistab kaugtöölaua seadmetes.

Töötamine võrguadministraator valesti sätteid oma asutuse sisevõrgu serva seadme luba HTTPS-põhiste kaugtöölaua ühendus.

## <a name="source-3-cloud-service-endpoint-and-acl"></a>Allikas 3: Pilveteenuste teenuse lõpp-punkti ja ACL

Klassikaline juurutamise mudeli abil loodud vms, veenduge, et teise Azure VM, mis on sama pilveteenuses või virtuaalse võrgu saab teha kaugtöölaua oma Azure VM.

![](./media/virtual-machines-windows-detailed-troubleshoot-rdp/tshootrdp_3.png)

> [AZURE.NOTE] Ressursi Manager loodud virtuaalmasinates, jätkake [Allika 4: võrgu turberühmad](#source-4-network-security-groups).

Kui teil pole mõne muu virtuaalse masina sama pilveteenuses või virtuaalse võrgu, luua. Järgige [loomine virtuaalse masina opsüsteemi Windows Azure](virtual-machines-windows-hero-tutorial.md). Kui test on lõpule jõudnud, kustutada testi virtuaalse masina.

Kui saate luua ühenduse kaudu kaugtöölaua virtuaalse masina sama pilveteenuses või virtuaalse võrgu, kontrollige järgmisi sätteid.

- Lõpp-punkti konfiguratsiooni kaugtöölaua liikluse sihtkohas VM: privaatne TCP pordi lõpp-punkti peab vastama TCP pordi aluseks on esitatud VM kaugtöölaua teenuse listening (vaikimisi 3389).
- Kaugtöölaua liikluse lõpp-punkti sihtkohas VM ACL: ACL-ID abil saate määrata lubatud või keelatud liiklust IP-aadress selle andmeallika põhjal Interneti kaudu. Valesti ACL-ID, saate vältida kaugtöölaua liiklust lõpp-punkti. Märkige oma ACL tagada selle Sissetuleva liikluse oma avaliku IP-aadressid oma puhverserveri või muu serva server on lubatud. Lisateavet leiate teemast [mis on on võrgu juurdepääsu juhtimine loend (ACL)?](../virtual-network/virtual-networks-acl.md)

Kontrollige, kas lõpp-punkti on probleemi allikaks, eemaldage praeguse lõpp-punkti ja looge uus, valides juhusliku port vahemikus 49152 – 65535 välise pordinumber. Lisateabe saamiseks vaadake, [Kuidas häälestada virtuaalse masina lõpp-punktid](virtual-machines-windows-classic-setup-endpoints.md).

## <a name="source-4-network-security-groups"></a>Allikas 4: Võrgu turberühmad

Võrgu turberühmad lubada täpsema juhtimise lubatud sissetuleva ja väljamineva liikluse. Saate luua reeglid kestvad alamvõrku ja pilveteenused Azure virtuaalse võrgus. Kontrollige oma võrgu turberühma reeglid tagamaks, et kaugtöölaua liikluse Interneti kaudu on lubatud.

- Azure'i portaalis, valige oma VM
- Klõpsake nuppu **Kõik sätted** | **võrgu liidesed** ja valige oma võrgu liidese.
- Klõpsake nuppu **Kõik sätted** | **võrgu turberühma** ja valige oma võrgu turberühma.
- Klõpsake nuppu **Kõik sätted** | **turvalisuse sissetulevad reeglid** ja veenduge, et teil on jäetud TCP-pordi 3389 RDP reegel.
    - Kui teil pole reeglit, klõpsake nuppu **Lisa** reegli loomiseks. Sisestage **TCP** Protocol (protokoll) ja seejärel **3389** sihtkoha portide vahemiku.
    - Veenduge, et toiming on seatud **Luba** ja uue sissetuleva reegli salvestamiseks klõpsake nuppu OK.


Lisateavet leiate teemast [mis on võrgu turvalisus jaotises (NSG)?](../virtual-network/virtual-networks-nsg.md)

## <a name="source-5-windows-based-azure-vm"></a>Allikas 5: Azure'i VM Windowsi-põhiste

![](./media/virtual-machines-windows-detailed-troubleshoot-rdp/tshootrdp_5.png)

[Azure'i IaaS (Windows) diagnostika paketi](https://home.diagnostics.support.microsoft.com/SelfHelp?knowledgebaseArticleFilter=2976864) abil vaadata, kui selle põhjuseks Azure virtuaalse masina ise. Kui see diagnostika pakett ei saa **RDP Ühenduvus on Azure VM (nõutav on taaskäivitamine)** probleemi lahendamiseks, järgige [selles artiklis](virtual-machines-windows-reset-rdp.md)antud juhiseid. Selles artiklis lähtestab kaugtöölaua teenuse virtual arvutis:

- Luba "Kaugtöölaua" Windowsi tulemüüri vaikimisi reegel (TCP port 3389).
- Luba kaugtöölaua määrates HKLM\System\CurrentControlSet\Control\Terminal Server\fDenyTSConnections registri väärtus 0.

Proovige uuesti ühendus luua oma arvutist. Kui teil on ikka ei saa ühendust kaudu kaugtöölaua, kontrollige järgmisi võimalikke probleeme.

- Kaugtöölaua teenus ei tööta sihtkohas VM.
- TCP pordi 3389 on kaugtöölaua teenus pole kuulamise.
- Windowsi tulemüüri või muu kohaliku tulemüür on Väljaminev reegel, mis takistab kaugtöölaua liikluse.
- Kaugtöölaua takistab sissetungi tuvastamise või võrgu jälgimise töötavate Azure virtuaalse masina tarkvara.

Vms klassikaline juurutamise mudeli abil loodud, saate kasutada Azure PowerShelli Kaugseanss Azure virtuaalse masina abil. Esmalt peate installima sertifikaadi virtuaalse masina majutusteenuse pilveteenuses. Minge [Konfigureerida Secure PowerShelli Kaugpöördusteenuse Azure'i Virtuaalmasinates](http://gallery.technet.microsoft.com/scriptcenter/Configures-Secure-Remote-b137f2fe) ja **InstallWinRMCertAzureVM.ps1** skripti faili kohalikku arvutisse alla laadida.

Installige Azure'i PowerShelli edasi, kui te pole seda veel teinud. Vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md).

Järgmiseks avage käsuviip Azure PowerShelli ja praeguse kausta muutmiseks **InstallWinRMCertAzureVM.ps1** skripti faili asukoht. Mõne Azure PowerShelli skripti käivitamiseks peate määrama õige täitmise poliitika. Käivitage käsk **Get-ExecutionPolicy** oma praeguse taseme määratlemiseks. Sobiva taseme määramise kohta leiate teavet teemast [Set-ExecutionPolicy](https://technet.microsoft.com/library/hh849812.aspx).

Seejärel sisestage oma Azure tellimuse nime ja pilvepõhise teenuse nimi oma virtuaalse masina nimi (eemaldamine on < ja > märkide), ja seejärel käivitage järgmised käsud.

    $subscr="<Name of your Azure subscription>"
    $serviceName="<Name of the cloud service that contains the target virtual machine>"
    $vmName="<Name of the target virtual machine>"
    .\InstallWinRMCertAzureVM.ps1 -SubscriptionName $subscr -ServiceName $serviceName -Name $vmName

**Get-AzureSubscription** käsu Kuva atribuudi _SubscriptionName_ saad õige tellimuse nime. Saate teenus cloud virtuaalse masina nimi **Get-AzureVM** käsu Kuva veerust _teenuse nimi_ .

Märkige ruut, kui teil on uut serti. Avage soovitud lisandmooduli serdid praeguse kasutaja ja vaadata kausta **Trusted Root sertimine Authorities\Certificates** . Peaksite nägema oma pilveteenuses veerus välja nimi DNS-i sert (näide: cloudservice4testing.cloudapp.net).

Järgmiseks algatada Azure PowerShelli Kaugseanss nende käskude abil.

    $uri = Get-AzureWinRMUri -ServiceName $serviceName -Name $vmName
    $creds = Get-Credential
    Enter-PSSession -ConnectionUri $uri -Credential $creds

Kui olete sisestanud kehtiv administraatori identimisteave, peaksite nägema midagi sarnast Azure PowerShelli järgmine viip:

    [cloudservice4testing.cloudapp.net]: PS C:\Users\User1\Documents>

See viip esimene osa on teie cloud nimi, mis sisaldab target VM, mis ei pruugi olla "cloudservice4testing.cloudapp.net". Nüüd saate väljastada Azure PowerShelli käske selle pilveteenuses, et uurida mainitud ja õige konfiguratsiooni.

### <a name="to-manually-correct-the-remote-desktop-services-listening-tcp-port"></a>TCP port listening Kaugtöölauateenuseid käsitsi parandamiseks

Kui te ei saa, käivitage [Azure'i IaaS (Windows) diagnostika paketi](https://home.diagnostics.support.microsoft.com/SelfHelp?knowledgebaseArticleFilter=2976864) avaldamiseks **on Azure VM (nõutav on taaskäivitamine) RDP Ühenduvus** remote Azure PowerShelli seansi, käivitage see käsk.

    Get-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name "PortNumber"

Atribuudi PortNumber kuvatakse praeguse pordi number. Vajadusel muutke kaugtöölaua pordi number tagasi vaikeväärtus (3389) selle käsu abil.

    Set-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name "PortNumber" -Value 3389

Veenduge, et pordi on muudetud 3389 selle käsu abil.

    Get-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name "PortNumber"

Väljuge Azure PowerShelli Kaugseanss selle käsu abil.

    Exit-PSSession

Veenduge, et kaugtöölaua lõpp-punkti jaoks Azure VM ka kasutab TCP port 3398 selle sisemine Port. Taaskäivitage Azure VM ja proovige uuesti kaugtöölaua ühendus.


## <a name="additional-resources"></a>Lisaressursid

[Azure'i IaaS (Windows) diagnostika pakett](https://home.diagnostics.support.microsoft.com/SelfHelp?knowledgebaseArticleFilter=2976864)

[Kuidas lähtestada parooli või kaugtöölaua teenus Windows virtuaalmasinates](virtual-machines-windows-reset-rdp.md)

[Kuidas installida ja konfigureerida Azure PowerShell](../powershell-install-configure.md)

[Secure Shell (SSH) Ühenduste Linux-põhine Azure virtuaalse masina tõrkeotsing](virtual-machines-linux-troubleshoot-ssh-connection.md)

[Accessi rakenduse Azure virtuaalne arvutis töötab tõrkeotsing](virtual-machines-linux-troubleshoot-app-connection.md)
