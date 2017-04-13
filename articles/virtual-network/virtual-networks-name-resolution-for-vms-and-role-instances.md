<properties 
   pageTitle="Eraldusvõime VMs ja rolli aknad"
   description="Nimi eraldusvõime stsenaariumid Azure'i IaaS hübriid lahendusi, erinevate pilveteenustega, Active Directory ja ise oma DNS-i serveri kasutamine "
   services="virtual-network"
   documentationCenter="na"
   authors="GarethBradshawMSFT"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="telmos" />

# <a name="name-resolution-for-vms-and-role-instances"></a>Nimelahendus VMs ja rolli aknad

Sõltuvalt sellest, kuidas kasutada Azure IaaS, PaaS ja hübriid lahendused majutada, peate lubama VMs ja rolli aknad, saate omavahel suhelda. Kuigi see teatis saab teha, kasutades IP-aadressid, on märksa lihtsam kasutada nimed, mida saab hõlpsalt meelde ja muuta. 

Kui rolli aknad ja Azure majutatud VMs vaja lahendamiseks domeeninimede sisemise IP-aadressid, saavad nad kasutada ühte kaks võimalust:

- [Azure'i antud nimelahendus](#azure-provided-name-resolution)

- [Nimelahendus ise oma DNS-i serveri kasutamine](#name-resolution-using-your-own-dns-server) (mis võib edastada päringute Azure'i antud DNS-serverid) 

Kasutate nimelahendus tüüp sõltub sellest, kuidas oma VMs ja rolli aknad tuleb omavahel suhelda.

**Järgmises tabelis on näidatud stsenaariumid ja lahendused nimi eraldusvõime:**

| **Stsenaarium** | **Lahendus** | **Järelliide** |
|--------------|--------------|----------|
| Nimelahendus rolli aknad või asub sama pilveteenuses või virtuaalse võrgu VMs vahel | [Azure'i antud nimelahendus](#azure-provided-name-resolution)| hostname (hostinimi) või FQDN |
| Nimelahendus rolli aknad või VMs, mis asuvad erinevates virtuaalse võrkudes vahel | Kliendi hallatav DNS-serverid ümbersuunamine päringute vnets lahendamise, Azure (DNS-i puhverserveri) vahel.  vt [nimelahendus ise oma DNS-i serveri kasutamine](#name-resolution-using-your-own-dns-server)| Ainult FQDN |
| Kohapealse arvuti ja teenuste nimed rolli aknad ja Azure VMs eraldusvõime | Kliendi hallatav DNS-serverid (nt kohapealne domeenikontrolleri, kohaliku kirjutuskaitstud domeenikontrolleri või sünkroonida, kasutades tsooni üle sekundaarne DNS).  Vt [nimelahendus ise oma DNS-i serveri kasutamine](#name-resolution-using-your-own-dns-server)|Ainult FQDN |
| Azure'i hostinimed kohapealne arvutitest eraldusvõime | Kliendi hallatav DNS-i puhverserveri klõpsake vastava vnet edasi päringud puhverserveri edastab päringud Azure resolutsiooni. Vt [nimelahendus ise oma DNS-i serveri kasutamine](#name-resolution-using-your-own-dns-server)| Ainult FQDN |
| Vastupidise DNS-i sisemise IP-d | [Nimelahendus ise oma DNS-i serveri kasutamine](#name-resolution-using-your-own-dns-server) | n/a |
| Nimelahendus VMs või rolli aknad, mis asuvad eri cloud Services, mitte virtuaalse võrgu vahel| Pole rakendatav. Väljaspool virtuaalse võrgu Ühenduvus VMs ja rolli erinevate pilveteenuste aknad ei toetata.| n/a |



## <a name="azure-provided-name-resolution"></a>Azure'i antud nimelahendus

Koos eraldusvõime avaliku DNS-i nimed, annab Azure'i sisemine nimelahendus VMs ja rolli eksemplarid, mis asuvad samas virtuaalse võrgus või pilveteenuses.  VMs/eksemplarid mõnes pilveteenuses ühiskasutusse sama DNS-i järelliite (nii piisab ainult hostname), kuid klassikaline virtuaalse võrgu eri pilveteenuses teenused on erinevate DNS-sufiksid nii FQDN on vaja nimede vahel eri pilveteenustega.  Virtuaalne võrkude ressursihaldur juurutamise mudeli, DNS-i järelliite on ühtsete üle virtuaalse võrgu (nii, et FQDN ei ole vajalik) ja DNS-i nimed saab määrata NICs nii VMs. Kuigi antud Azure'i nimelahendus ei nõua konfiguratsiooni, ei ole sobiv valik kõigi juurutamise stsenaariumid, nagu näha ülaltoodud tabelis.

> [AZURE.NOTE] Puhul veebi ja töötaja rollid, võite kasutada ka sisemise IP-aadressid rolli aknad, põhinedes määratud roll nimi ja eksemplari Azure'i teenuse haldamine REST API abil. Lisateabe saamiseks lugege teemat [Teenuste haldus REST API viide](https://msdn.microsoft.com/library/azure/ee460799.aspx).

### <a name="features-and-considerations"></a>Funktsioonid ja kaalutlused

**Funktsioonid.**

- Kasutuslihtsus: Azure'i antud nimelahendus kasutamiseks peab pole konfigureerimine.

- Azure'i antud nimi eraldusvõime teenus on väga kättesaadav, salvestamist saate luua ja hallata oma DNS-serverite kogumite vaja.

- Saab kasutada koos oma DNS-serverid kohapealne ja Azure hostinimed lahendamiseks.

- Nimelahendus on esitatud eksemplari rolli VMs sees ilma vajaduseta on FQDN sama pilveteenuses vahel.

- Nimelahendus on esitatud virtuaalse võrkudes, mis kasutavad ressursihaldur juurutamise mudeli ilma vajaduseta FQDN VMs vahel. Virtuaalne võrkude klassikaline juurutamise mudeli nõuda FQDN lahendavad nimesid erinevate cloud Services. 

- Saate kasutada oma juurutuste kirjeldavad hostinimed asemel töötamine automaatselt loodud nimed.

**Kaalutlused**

- Azure'i loodud DNS-i järelliite ei saa muuta.

- Te ei saa käsitsi registreerida oma kirjeid.

- VÕITU ja NetBIOS ei toetata. (Te ei näe oma Windows Exploreris VMs.)

- Hostinimed peab olema DNS-i ühilduv (need tuleb kasutada ainult 0 – 9 a – z ja "-", ja ei saa alustamine või lõpetamine koos on "-". Jaotisest RFC 3696 2.)

- DNS-i päringu liikluse on rakendus jaoks iga VM. See ei tohiks mõjutada Enamik rakendusi.  Taotluse pidurdamise ilmnemisel, kontrollige, kas kliendipoolne vahemälu on lubatud.  Leiate lisateavet teemast [Saada antud Azure'i nimelahendus kõige](#Getting-the-most-from-Azure-provided-name-resolution).

- Ainult esimese 180 pilveteenustega VMs registreeritakse iga virtuaalse võrgu klassikaline juurutamise mudel. See ei kehti virtuaalse võrkudega ressursihaldur juurutamise mudelid.


### <a name="getting-the-most-from-azure-provided-name-resolution"></a>Saada kõige antud Azure'i nimelahendus
**Kliendipoolne vahemälu:**

Mitte iga DNS-i päringu on vaja saata üle võrgu.  Kliendipoolne vahemälu aitab vähendada latentsus ja parandada paindlikkuse, et võrgu blips lahendamisega korduva DNS päringud kohaliku vahemälu.  DNS-i kirjed sisaldavad mõne aja Live (TTL), mis võimaldab säilitada nii kaua kirje mõjutamata kirje värskus, et kliendipoolne vahemälu sobib enamasti vahemälu.

Vaikimisi Windows DNS-i klient on valmis DNS-i vahemälu.  Mõned Linux distros ei sisalda vaikimisi vahemällu, on soovitatav, et ühte lisatakse iga Linux VM (pärast kontrollimist, et ei ole kohaliku vahemälu juba).

On mitmeid erinevaid DNS-i vahemälu paketid saadaval, nt dnsmasq, siin on kõige levinum distros dnsmasq installimine juhiseid:

- **Ubuntu (kasutab resolvconf)**:
    - install lihtsalt dnsmasq paketi ("sudo apt-get install dnsmasq").
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

> [AZURE.NOTE] 'Dnsmasq' pakett on ainult üks paljude DNS-i vahemälu Linuxi jaoks saadaval.  Enne selle kasutamist, kontrollige oma sobivust teie vajadustele ja vahemälu pole installitud.

**Kliendipoolne korduskatsed:**

DNS-i on peamiselt UDP-protokolli.  Nagu UDP-protokolli ei garanteeri kohaletoimetamise, käsitletakse uuesti loogika ise DNS-protokolli.  Iga DNS-i kliendi (operatsioonisüsteem) võib kaasa tuua eri proovi uuesti loogika sõltuvalt loojad eelistus:

 - Windowsi operatsioonisüsteemid uuesti pärast 1 teine ja seejärel uuesti pärast teise 2, 4 ja teise 4 sekundit. 
 - Vaikimisi Linux häälestamise korduskatsed sekundi möödumisel.  On soovitatav seda uuesti 5 intervallide 1 teise muuta.  

Praeguste sätete Linux VM "kass /etc/resolv.conf" ja "Valikud" Real, pilk nt kontrollimiseks tehke järgmist.

    options timeout:1 attempts:5

Resolv.conf fail on tavaliselt automaatselt loodud ja ei saa redigeerida.  Kindla rea 'Valikud' lisamise juhiseid erineda distributsiooni:

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
On mitmeid olukordi, kui teie nimi eraldusvõime vajadustele võib minna Lisaks võimalused Azure, näiteks kui abil Active Directory domeenid või kui vajate DNS-i resolvimise virtuaalse võrgu (vnets) vahel.  Need stsenaariumid katmiseks Azure pakub võimalust kasutada oma DNS-serverid.  

DNS-serverid virtuaalse võrgustikus saate edastada DNS-i päringute Azure Rekursiivsed selsüünid hostinimed selle virtuaalse võrgustikus lahendamiseks.  Näiteks on domeeni domeenikontrolleri d c töötab Azure oma domeenide DNS-i päringute vastata ja edastada kõik muud päringud Azure.  See võimaldab VMs kohapealne ressursid (näiteks Põhiliselt) kaudu ja Azure antud hostinimed (kaudu ekspediitor) kuvamiseks.  Juurdepääs Azure Rekursiivsed selsüünid on esitatud virtuaalse IP 168.63.129.16 kaudu.

DNS-i ümbersuunamine ka muu vnet DNS-i resolvimise lubab ja võimaldab oma kohapealne masinad lahendamiseks antud Azure'i hostinimed.  Mõne VM hostname lahendamiseks DNS-serveri VM peab asuma samas virtuaalse võrgus ja konfigureerida nii, et edasi hostname päringute Azure.  DNS-i järelliite on erinevad iga vnet, saate saata DNS-i päringute õige vnet lahendamise ümbersuunamine tingimusvormingu reegleid.  Järgmisel pildil on kujutatud kahe vnets ja kohapealne võrk, mis teevad selle meetodi kasutamise muu vnet DNS-i resolvimise.  Mõni näide DNS-i on saadaval [Azure Kiirjuhend kuukalendreid](https://azure.microsoft.com/documentation/templates/301-dns-forwarder/) ja [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/301-dns-forwarder).

![Muu vnet DNS-i](./media/virtual-networks-name-resolution-for-vms-and-role-instances/inter-vnet-dns.png)

Azure'i antud nimelahendus, sisemise DNS-i järelliite kasutamisel (*. internal.cloudapp.net) on saadaval iga VM DHCP abil.  See võimaldab hostname eraldusvõime hostname kirjed on internal.cloudapp.net tsooni.  Kui kasutate oma nime eraldusvõime lahendus, IDNS järelliite ei esitata VMs kuna see häirib muude DNS-i arhitektuur (nt domeeniga liitunud stsenaariumid).  Selle asemel pakume tööta kohatäite (reddog.microsoft.com).  

Vajadusel saate sisemise DNS-i järelliite määrata PowerShelli või API:

-  Virtuaalne võrkude ressursihaldur juurutamise mudelid, järelliide on saadaval [võrgu liidese kaart](https://msdn.microsoft.com/library/azure/mt163668.aspx) ressursside või [Get-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619434.aspx) cmdlet-käsu kaudu.    
-  Klassikaline juurutamise mudelid, sufiks on saadaval või [Saada juurutamise API](https://msdn.microsoft.com/library/azure/ee460804.aspx) kõne kaudu soovitud [Get-AzureVM-silumine](https://msdn.microsoft.com/library/azure/dn495236.aspx) cmdlet-käsk.


Kui ümbersuunamine päringute Azure ei sobi, peate oma DNS-i lahendus.  Teie DNS-i lahendus on vaja:

-  Sisestage asjakohane hostinime lahenduse, nt [DDNS](virtual-networks-name-resolution-ddns.md)kaudu.  Pange tähele, et kui soovite keelata DNS-i kirje roojase Azure DHCP liisingute on väga palju aega ja scavenging DDNS abil eemaldada DNS-i kirjete ennetähtaegselt. 
-  Sisestage vastav Rekursiivsed eraldusvõime eraldusvõime välise domeeninimede lubama.
-  Puuetega inimestele juurdepääsetavate (TCP ja UDP port 53) see kliendid ja pääse Interneti-ühendus.
-  Kinnitada Accessi Internetist välise agentide ohtude leevendada.

> [AZURE.NOTE] Parima jõudluse tagamiseks Azure VMs kasutamisel DNS-i serverid IPv6 olla keelatud ja määratakse [Eksemplari taseme avaliku IP](virtual-networks-instance-level-public-ip.md) -iga DNS-i serveris VM.  Kui otsustate kasutada Windows Server on teie DNS-i server, [selles artiklis](http://blogs.technet.com/b/networking/archive/2015/08/19/name-resolution-performance-of-a-recursive-windows-dns-server-2012-r2.aspx) on toodud täiendavad jõudluse analüüs ja optimeerimine.


### <a name="specifying-dns-servers"></a>DNS-serverid määramine

Kui kasutate oma DNS-serverid, pakub Azure võimalus määrata mitu DNS-serverid virtuaalse võrgu või kohta võrgu liidese (ressursihaldur) / cloud service (klassikaline).  DNS-serverid pilvepõhise teenuse/võrgu kasutajaliidese jaoks määratud saada järjestuse üle virtuaalse võrgu jaoks määratletud.

> [AZURE.NOTE] Võrgu ühenduse atribuudid, näiteks DNS-i serveri IP-d, ei saa redigeerida otse Windows VMs, nagu need võivad saada kustutada ajal teenuse heal kui virtuaalse võrgu adapterit saab asendada. 


Ressursihaldur juurutamise mudeli kasutamisel saab määrata DNS-serverid portaalis API/mallid ([vnet](https://msdn.microsoft.com/library/azure/mt163661.aspx), [nic](https://msdn.microsoft.com/library/azure/mt163668.aspx)) või PowerShelli ([vnet](https://msdn.microsoft.com/library/mt603657.aspx), [nic](https://msdn.microsoft.com/library/mt619370.aspx)).

Klassikaline juurutamise mudeli kasutamisel saate DNS-serverid virtuaalse võrgu jaoks määratud portaali või [ *Võrgu konfigureerimise* faili](https://msdn.microsoft.com/library/azure/jj157100).  Puhul pilveteenuste, DNS-serverid on määratud [ *Teenuse* konfiguratsioonifail](https://msdn.microsoft.com/library/azure/ee758710) kaudu või PowerShelli ([Uus-AzureVM](https://msdn.microsoft.com/library/azure/dn495254.aspx)).

> [AZURE.NOTE] Kui virtuaalse võrgu, virtuaalse masina, mis on juba juurutanud DNS-i sätete muutmiseks peate iga probleemse VM muudatuste jõustumiseks taaskäivitage.


## <a name="next-steps"></a>Järgmised sammud

Ressursihaldur juurutamise mudel:

- [Loomine ja värskendamine virtuaalse võrgus](https://msdn.microsoft.com/library/azure/mt163661.aspx)
- [Loomine ja värskendamine võrgu liidese kaart](https://msdn.microsoft.com/library/azure/mt163668.aspx)
- [Uue AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt603657.aspx)
- [Uue AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619370.aspx)

 
Klassikaline juurutamise mudel:

- [Azure'i teenus konfiguratsioon skeemi](https://msdn.microsoft.com/library/azure/ee758710)
- [Virtual Network Configuration skeem](https://msdn.microsoft.com/library/azure/jj157100)
- [Võrgu konfigureerimise faili virtuaalse võrgu konfigureerimine](virtual-networks-using-network-configuration-file.md) 

