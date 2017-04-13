<properties 
   pageTitle="DNS-i Nimelahenduse suvandite Linux vms Azure"
   description="Nime eraldusvõime stsenaariumid Linux VMs Azure'i IaaS, sh esitatud DNS-teenused, välise DNS-i ja tuua teie ise oma DNS-i hübriidserveri."
   services="virtual-machines"
   documentationCenter="na"
   authors="RicksterCDN"
   manager="timlt"
   editor="tysonn" />
<tags 
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/19/2016"
   ms.author="rclaus" />

# <a name="dns-name-resolution-options-for-linux-vms-in-azure"></a>DNS-i Nimelahenduse suvandite Linux vms Azure

Azure'i pakub DNS-i nimelahendus vaikimisi kõik vms sisalduvad ühe virtuaalse võrgu. Teil on võimalik rakendada oma DNS-i nimi eraldusvõime lahendust konfigureerimisega ise oma DNS-i teenuste kohta oma Azure majutatud VMs. Järgmistel juhtudel peaks aitavad teil valida, millise paremini sobib teie olukorrale.

- [Azure'i antud nimelahendus](#azure-provided-name-resolution)

- [Nimelahendus ise oma DNS-i serveri kasutamine](#name-resolution-using-your-own-dns-server) 

Kasutate nimelahendus tüüp sõltub sellest, kuidas oma VMs ja rolli aknad tuleb omavahel suhelda.

**Järgmises tabelis on näidatud stsenaariumid ja lahendused nimi eraldusvõime:**

| **Stsenaarium** | **Lahendus** | **Järelliide** |
|--------------|--------------|----------|
| Nimelahendus rolli aknad või VMs, mis asuvad samas virtuaalse võrgu vahel | [Azure'i antud nimelahendus](#azure-provided-name-resolution)| hostname (hostinimi) või FQDN |
| Nimelahendus rolli aknad või VMs, mis asuvad erinevates virtuaalse võrkudes vahel | Kliendi hallatav DNS-serverid ümbersuunamine päringute vnets lahendamise, Azure (DNS-i puhverserveri) vahel.  vt [nimelahendus ise oma DNS-i serveri kasutamine](#name-resolution-using-your-own-dns-server)| Ainult FQDN |
| Kohapealse arvuti ja teenuste nimed rolli aknad ja Azure VMs eraldusvõime | Kliendi hallatav DNS-serverid (nt kohapealne domeenikontrolleri, kohaliku kirjutuskaitstud domeenikontrolleri või sünkroonida, kasutades tsooni üle teisene DNS).  Vt [nimelahendus ise oma DNS-i serveri kasutamine](#name-resolution-using-your-own-dns-server)|Ainult FQDN |
| Azure'i hostinimed kohapealne arvutitest eraldusvõime | Kliendi hallatav DNS-i puhverserveri klõpsake vastava vnet edasi päringud puhverserveri edastab päringud Azure resolutsiooni. Vt [nimelahendus ise oma DNS-i serveri kasutamine](#name-resolution-using-your-own-dns-server)| Ainult FQDN |
| Vastupidise DNS-i sisemise IP-d | [Nimelahendus ise oma DNS-i serveri kasutamine](#name-resolution-using-your-own-dns-server) | n/a |

## <a name="azure-provided-name-resolution"></a>Azure'i antud nimelahendus

Koos eraldusvõime avaliku DNS-i nimed, annab Azure'i sisemine nimelahendus VMs ja rolli eksemplarid, mis asuvad samas virtuaalse võrgustikus.  ARM-põhised virtuaalne võrkude DNS-i järelliite on ühtsete üle virtuaalse võrgu (nii, et FQDN ei ole vajalik) ja DNS-i nimed saab määrata NICs nii VMs. Kuigi antud Azure'i nimelahendus ei nõua konfiguratsiooni, ei ole sobiv valik kõigi juurutamise stsenaariumid, nagu eelmises tabelis.

### <a name="features-and-considerations"></a>Funktsioonid ja kaalutlused

**Funktsioonid.**

- Kasutuslihtsus: Azure'i antud nimelahendus kasutamiseks on vajalik pole konfigureerimine.

- Azure'i antud nimi eraldusvõime teenus on väga kättesaadav, salvestamist saate luua ja hallata oma DNS-serverite kogumite vaja.

- Saab kasutada koos oma DNS-serverid kohapealne ja Azure hostinimed lahendamiseks.

- Nimelahendus on esitatud virtuaalse võrgu ilma vajaduseta FQDN VMs vahel. 

- Saate kasutada oma juurutuste kirjeldavad hostinimed asemel töötamine automaatselt loodud nimed.

**Kaalutlused**

- Azure'i loodud DNS-i järelliite ei saa muuta.

- Te ei saa käsitsi registreerida oma kirjeid.

- VÕITU ja NetBIOS ei toetata.

- Hostinimed peab olema DNS-i ühilduv (need tuleb kasutada ainult 0 – 9 a – z ja "-", ja ei saa alustamine või lõpetamine koos on "-". Jaotisest RFC 3696 2.)

- DNS-i päringu liikluse on rakendus jaoks iga VM. See ei tohiks mõjutada Enamik rakendusi.  Taotluse pidurdamise ilmnemisel, kontrollige, kas kliendipoolne vahemälu on lubatud.  Lisateabe saamiseks lugege teemat [Saada antud Azure'i nimelahendus kõige](#Getting-the-most-from-Azure-provided-name-resolution).


### <a name="getting-the-most-from-azure-provided-name-resolution"></a>Saada kõige antud Azure'i nimelahendus
**Kliendipoolne vahemälu:**

Võrgu kaudu saadetakse mitte iga DNS-i päringu.  Kliendipoolne vahemälu aitab vähendada latentsus ja parandada paindlikkuse, et võrgu blips lahendamisega korduva DNS päringud kohaliku vahemälu.  DNS-i kirjed sisaldavad mõne aja Live (TTL), mis võimaldab säilitada nii kaua kirje kirje värskus mõjutamata vahemälu.  Seetõttu on sobiv enamasti kliendipoolne vahemälu.

Mõned Linux distros ei sisalda vaikimisi vahemällu.  Soovitatav on teil seda iga Linux VM (pärast kontrollimist, et ei ole kohaliku vahemälu juba) lisada.

On mitmeid erinevaid DNS-i vahemälu pakette saadaval, näiteks dnsmasq, siin on kõige levinum distros dnsmasq installimine juhiseid:

- **Ubuntu (kasutab resolvconf)**:
    - Installige dnsmasq pakett ("sudo apt-get install dnsmasq").
- **SUSE (kasutab netconf)**:
    - Installige dnsmasq pakett ("sudo zypper installi dnsmasq") 
    - dnsmasq teenuse ("systemctl luba dnsmasq.service") 
    - Käivitage dnsmasq teenus ("systemctl start dnsmasq.service") 
    - Redigeeri "/ jne/sysconfig võrgu config" ja muuta NETCONFIG_DNS_FORWARDER = "" "dnsmasq" abil
    - kohalikud DNS-i Resolveri nimega resolv.conf ("netconfig update") määramiseks vahemälu värskendamine
- **OpenLogic (kasutab NetworkManager)**:
    - Installige dnsmasq pakett ("sudo yum installi dnsmasq")
    - dnsmasq teenuse ("systemctl luba dnsmasq.service")
    - Käivitage dnsmasq teenus ("systemctl start dnsmasq.service")
    - Lisage "prepend domeeni-nimeserverite 127.0.0.1;" kuni "/etc/dhclient-eth0.conf"
    - taaskäivitage kohalikud DNS-i Resolveri vahemälu seada võrguteenuse ("teenuse võrgu uuesti")

> [AZURE.NOTE]: 'Dnsmasq' pakett on ainult üks paljude DNS-i vahemälu Linuxi jaoks saadaval.  Enne selle kasutamist, märkige ruut selle sobivus teie vajadustele ja vahemälu pole installitud.

**Kliendipoolne korduskatsed:**

DNS-i on peamiselt UDP-protokolli.  Nagu UDP-protokolli ei garanteeri kohaletoimetamise, käsitletakse uuesti loogika ise DNS-protokolli.  Iga DNS-i kliendi (operatsioonisüsteem) võib kaasa tuua eri proovi uuesti loogika sõltuvalt loojad eelistus:

 - Windowsi operatsioonisüsteemid uuesti pärast ühe teise ja seejärel uuesti pärast teise kaks, neli ja teise neli sekundit. 
 - Vaikimisi Linux häälestamise korduskatsed 5 sekundi pärast.  Peaksite muutma selle uuesti viis korda ühe sekundi järel.  

Praeguste sätete Linux VM "kass /etc/resolv.conf" ja "Valikud" Real, pilk näiteks kontrollimiseks tehke järgmist.

    options timeout:1 attempts:5

Resolv.conf fail on automaatselt loodud ja ei saa redigeerida.  Kindla rea 'Valikud' lisamise juhiseid erineda distributsiooni:

- **Ubuntu** (kasutatakse resolvconf):
    - suvandite Joone lisamine "/ etc/resolveconf/resolv.conf.d/head" 
    - Käivitage 'resolvconf -u' värskendamine
- **SUSE** (kasutatakse netconf):
    - timeout:1 katsete: 5 lisamine soovitud NETCONFIG_DNS_RESOLVER_OPTIONS = "" parameeter tekstivormingus "/ jne/sysconfig võrgu config" 
    - Käivitage 'netconfig update' värskendamine
- **OpenLogic** (kasutatakse NetworkManager):
    - "kaja"suvandid timeout:1 katsete: 5"" lisamine "/ etc/NetworkManager/dispatcher.d/11-dhclient" 
    - Käivita 'teenuse võrgu uuesti' värskendamine

## <a name="name-resolution-using-your-own-dns-server"></a>Nimelahendus ise oma DNS-i serveri kasutamine
On mitmeid olukordi, kus teie nimi eraldusvõime vajadustele võib kaugemale funktsioonide tingimusel, Azure, näiteks kui teil on vaja DNS-i resolvimise virtuaalse võrgu (vnets) vahel.  Selle stsenaariumi katmiseks Azure pakub võimalust kasutada oma DNS-serverid.  

DNS-serverid virtuaalse võrgustikus saate edastada DNS-i päringute Azure Rekursiivsed selsüünid hostinimed selle virtuaalse võrgustikus lahendamiseks.  Näiteks saate DNS-i serveris, kus töötab Azure DNS-i päringute oma DNS-i tsooni failide vastata ja edastada kõik muud päringud Azure.  See võimaldab VMs nii kirjete tsoonifailide ja Azure antud hostinimed (kaudu ekspediitor) kuvamiseks.  Juurdepääs Azure Rekursiivsed selsüünid on esitatud virtuaalse IP 168.63.129.16 kaudu.

DNS-i ümbersuunamine ka muu vnet DNS-i resolvimise lubab ja võimaldab oma kohapealne masinad lahendamiseks antud Azure'i hostinimed.  Probleemi lahendamiseks on VM hostname DNS-serveri VM peab asuma samas virtuaalse võrgus ja konfigureerida nii, et edasi hostname päringute Azure.  DNS-i järelliite on erinevad iga vnet, saate saata DNS-i päringute õige vnet lahendamise ümbersuunamine tingimusvormingu reegleid.  Järgmisel pildil on kujutatud kahe vnets ja kohapealne võrk, mis teevad selle meetodi kasutamise muu vnet DNS-i resolvimise:

![Muu vnet DNS-i](./media/virtual-machines-linux-azure-dns/inter-vnet-dns.png)

Azure'i antud nimelahendus kasutamisel sisemise DNS-i järelliite pakutakse iga VM DHCP abil.  Oma nime eraldusvõime lahenduse kasutamisel see ei esitata VMs kuna see häirib muude DNS-i arhitektuur.  Viidata masinad FQDN, või oma VMs järelliide konfigureerimiseks järelliide kindlaks, PowerShelli või API:

-  Azure'i ressursihalduse hallatavate vnets, järelliide on kättesaadav ressursile [võrgu liidese kaart](https://msdn.microsoft.com/library/azure/mt163668.aspx) või käivitate käsu `azure network public-ip show <resource group> <pip name>` oma avaliku IP, sh FQDN nic. üksikasjade kuvamiseks    


Kui ümbersuunamine päringute Azure ei sobi, peate oma DNS-i lahendus.  Peab teie DNS-i lahendus.

-  Sisestage asjakohane hostinime lahenduse, näiteks [DDNS](../virtual-network/virtual-networks-name-resolution-ddns.md)kaudu.  Pange tähele, et kui soovite keelata DNS-i kirje roojase Azure DHCP liisingute on väga palju aega ja scavenging DDNS abil eemaldada DNS-i kirjete ennetähtaegselt. 
-  Sisestage vastav Rekursiivsed eraldusvõime eraldusvõime välise domeeninimede lubama.
-  Puuetega inimestele juurdepääsetavate (TCP ja UDP port 53) see kliendid ja pääse Interneti-ühendus.
-  Kinnitada Accessi Internetist välise agentide ohtude leevendada.

> [AZURE.NOTE] Parima jõudluse tagamiseks Azure VMs kasutamisel DNS-i serverid IPv6 olla keelatud ja määratakse [Eksemplari taseme avaliku IP](../virtual-network/virtual-networks-instance-level-public-ip.md) -iga DNS-i serveris VM.  

