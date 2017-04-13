<properties 
    pageTitle="Kuidas konfigureerida virtuaalse võrgu tugi Premium Azure'i Redis vahemälu | Microsoft Azure'i" 
    description="Saate teada, kuidas luua ja hallata oma Premium taseme Azure'i Redis vahemälu eksemplarid virtuaalse võrgu tugi" 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/15/2016" 
    ms.author="sdanie"/>

# <a name="how-to-configure-virtual-network-support-for-a-premium-azure-redis-cache"></a>Kuidas konfigureerida virtuaalse võrgu tugiteenuste Premium Azure'i Redis vahemälu
Azure'i Redis vahemälu on erinevate vahemälu pakkumised, mis pakuvad paindlikkust vahemälu maht ja funktsioone, sh uus Premium taseme valik.

Azure'i Redis vahemälu lisafunktsioonidele taseme kaasata rühmitamise püsimine ja virtuaalse võrgu (VNet) tugi. Mõne VNet on pilves. Kui soovitud VNet Azure'i Redis vahemälu eksemplar on konfigureeritud, pole avalikult adresseeritavad ja pääseb juurde ainult virtuaalmasinates ja selle VNet rakendustes. Selles artiklis kirjeldatakse, kuidas konfigureerida virtuaalse võrgu tugi premium Azure'i Redis vahemälu eksemplari.

>[AZURE.NOTE] Azure'i Redis vahemälu toetab nii klassikaline ja ARM VNets.

Muud lisafunktsioonidele vahemälu kohta leiate teavet teemast [Azure Redis vahemälu Premium taseme Sissejuhatus](cache-premium-tier-intro.md).

## <a name="why-vnet"></a>Miks VNet?
[Azure'i virtuaalse Network (VNet)](https://azure.microsoft.com/services/virtual-network/) kasutamine pakub tõhustatud turvalisus ja eraldamise Azure'i Redis vahemälu, kui ka alamvõrku, juurdepääsu piiramise poliitika ja muude funktsioonide täpsemaks juurdepääsu piiramiseks Azure'i Redis vahemälu.

## <a name="virtual-network-support"></a>Virtuaalne tugi
Virtuaalne (VNet) tugi on konfigureeritud enne **Uue Redis vahemälu** vahemälu loomise ajal. 

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-premium-create.md)]

Kui olete valinud hinnad taseme lisatasu, saate konfigureerida, valides VNet, mis on sama tellimuse ja vahemälu asukohaks Azure'i Redis vahemälu VNet integreerimine. Uue VNet kasutamiseks [luua virtuaalse võrgu (klassikaline) Azure portaali kaudu](../virtual-network/virtual-networks-create-vnet-classic-portal.md) või [virtuaalse võrgu Azure'i portaalis](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) juhiseid järgides esmalt looma ning naaske **Uue Redis vahemälu** tera loomiseks ja konfigureerimiseks premium vahemälu.

Jaoks uue vahemälu VNet konfigureerimiseks klõpsake **Virtuaalse võrgu** enne **Uue Redis vahemälu** ja valige soovitud VNet rippmenüü loendist.

![Virtuaalne võrk][redis-cache-vnet]

Valige soovitud alamvõrgu **alamvõrgu** ripploendist ja määrake soovitud **staatiline IP-aadress**. Kui kasutate klassikaline VNet **staatiline IP-aadress** väli on vabatahtlik ja kui pole määratud, üks on valitud valitud alamvõrgu.

>[AZURE.IMPORTANT] Azure'i Redis vahemälu on ARM VNet juurutamisel vahemälu peab olema sihtotstarbeline alamvõrku, mis sisaldab muud ressursid, välja arvatud Azure Redis vahemälu eksemplarid. Kui proovitakse juurutada Azure Redis vahemälu on ARM VNet on alamvõrku, mis sisaldab muud ressursid, juurutamise nurjub.

![Virtuaalse võrgu][redis-cache-vnet-ip]

>[AZURE.IMPORTANT] Neli esimest aadressid on alamvõrgu reserveeritud ja ei saa kasutada. Lisateavet leiate teemast [kas IP-aadresside sees need alamvõrku kasutamise piirangutest?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets)

Vahemälu on loodud, saate selle VNet konfiguratsiooni vaatamiseks klõpsata **Virtuaalse võrgu** keelest **sätted** .

![Virtuaalse võrgu][redis-cache-vnet-info]


Azure'i Redis vahemälu eksemplari ühenduse abil on VNet määrata vahemälu hostinimi ühendusstring, nagu on näidatud järgmises näites.

    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        return ConnectionMultiplexer.Connect("contoso5premium.redis.cache.windows.net,abortConnect=false,ssl=true,password=password");
    });
    
    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }

## <a name="azure-redis-cache-vnet-faq"></a>Azure'i Redis vahemälu VNet KKK

Järgmine loend sisaldab vastused korduma kippuvatele küsimustele Azure'i Redis vahemälu mastaapimist.

-   [Mis on vale konfiguratsioon levinumate probleemide Azure'i Redis vahemälu ja VNets?](#what-are-some-common-misconfiguration-issues-with-azure-redis-cache-and-vnets)
-   [Saate kasutada VNets standard- või lihtsa vahemälu?](#can-i-use-vnets-with-a-standard-or-basic-cache)
-   [Miks ei Redis vahemälu loomine nurjub mõned alamvõrku, kuid mitte teistele?](#why-does-creating-a-redis-cache-fail-in-some-subnets-but-not-others)
-   [Tehke kõik vahemälu funktsioonid töötavad, kui hosting vahemälu on VNET rakenduses?](#do-all-cache-features-work-when-hosting-a-cache-in-a-vnet)


## <a name="what-are-some-common-misconfiguration-issues-with-azure-redis-cache-and-vnets"></a>Mis on vale konfiguratsioon levinumate probleemide Azure'i Redis vahemälu ja VNets?

Kui Azure Redis vahemälu on majutatud on VNet, kasutatakse pordid järgmises tabelis. Kui järgmised pordid on blokeeritud, ei pruugi vahemälu õigesti töötada. Ühe või mitme need pordid blokeeritud on kõige levinum vale konfiguratsioon probleemi Azure Redis vahemälu on VNet kasutamisel.

| Port(s))     | Suund        | Transport Protocol (protokoll) | Otstarve                                                                           | Kaug-IP                           |
|-------------|------------------|--------------------|-----------------------------------------------------------------------------------|-------------------------------------|
| 80, 443     | Väljamineva meili         | TCP                | Redis sõltuvused Azure'i salvestusruumi/PKI (Internet)                                | *                                   |
| 53          | Väljamineva meili         | TCP/UDP            | DNS-i (Interneti-ja VNet) sõltuvuse redis                                         | *                                   |
| 6379, 6380  | Sissetulev          | TCP                | Kliendi side Redis, Azure'i Koormusetasakaalustuseks                               | VIRTUAL_NETWORK AZURE_LOADBALANCER |
| 8443        | Sissetulev/väljaminev liiklus | TCP                | Rakendamise üksikasjad Redis jaoks                                                   | VIRTUAL_NETWORK                     |
| 8500        | Sissetulev          | TCP/UDP            | Azure'i tasakaalustamiseks laadimine                                                              | AZURE_LOADBALANCER                  |
| 10221-10231 | Sissetulev/väljaminev liiklus | TCP                | Rakendamise üksikasjade Redis (võib piirata serveri lõpp-punkti VIRTUAL_NETWORK) | VIRTUAL_NETWORK AZURE_LOADBALANCER |
| 13000-13999 | Sissetulev          | TCP                | Kliendi side Redis kogumite, Azure'i Koormusetasakaalustuseks                      | VIRTUAL_NETWORK AZURE_LOADBALANCER |
| 15000-15999 | Sissetulev          | TCP                | Kogumite Redis Azure'i Koormusetasakaalustuseks kliendi suhtlus                      | VIRTUAL_NETWORK AZURE_LOADBALANCER |
| 16001       | Sissetulev          | TCP/UDP            | Azure'i tasakaalustamiseks laadimine                                                              | AZURE_LOADBALANCER                  |
| 20226       | Sissetuleva + väljaminev | TCP                | Rakendamise üksikasjad Redis kogumite jaoks                                          | VIRTUAL_NETWORK                     |


On võrgu Ühenduvus nõuded Azure'i Redis vahemälu, mis võivad pole algselt täidetud virtuaalse võrku. Azure'i Redis vahemälu nõuab kõigi järgmiste üksuste selleks, et toimida, kui seda kasutatakse virtuaalse võrgustikus.

-  Väljamineva võrguühendus worldwide Azure Storage lõpp-punktid. See hõlmab asub sama piirkonna Azure'i Redis vahemälu eksemplari, kui ka salvestusruumi lõpp-punktid asub **teiste** Azure'i piirkondade lõpp-punktid. Azure'i salvestusruumi lõpp-punktid lahendamiseks järgmist DNS-i administraatordomeenid: *table.core.windows.net*, *blob.core.windows.net*, *queue.core.windows.net*ja *file.core.windows.net*. 
-  Väljamineva võrguühendus *ocsp.msocsp.com*, *mscrl.microsoft.com* ja *crl.microsoft.com*. SSL-i funktsioonide tugi on vaja see Ühenduvus.
-  DNS-i konfigureerimine virtuaalse võrgu jaoks peab olema võimalik lahendamine kõigi lõpp-punktid ja varasemate punktides märgitud domeenid. Need DNS-i nõuded saate täidetud, tagades kehtiv DNS-i taristu on konfigureeritud ja virtuaalse võrgu säilitada.



### <a name="can-i-use-vnets-with-a-standard-or-basic-cache"></a>Saate kasutada VNets standard- või lihtsa vahemälu?

VNets saab kasutada ainult premium vahemälu.

### <a name="why-does-creating-a-redis-cache-fail-in-some-subnets-but-not-others"></a>Miks ei Redis vahemälu loomine nurjub mõned alamvõrku, kuid mitte teistele?

Kui juurutate Azure'i Redis vahemälu on ARM VNet, vahemälu peab olema sihtotstarbeline alamvõrku, ressursside tüüpi sisaldava. Kui proovitakse juurutada Azure Redis vahemälu on ARM VNet alamvõrku, mis sisaldab muud ressursid, juurutamise nurjub. Kustutage olemasolev ressursse sees alamvõrgu enne kui saate luua uue vahemälu Redis.

Kui teil on piisavalt IP-aadresside saadaval saate juurutada klassikaline VNet mitut tüüpi ressursid.

### <a name="do-all-cache-features-work-when-hosting-a-cache-in-a-vnet"></a>Tehke kõik vahemälu funktsioonid töötavad, kui hosting vahemälu on VNET rakenduses?

Kui vahemälu on osa on VNET, pääsete ainult kliendid on VNET vahemälu. Selle tulemusena vahemälu halduse järgmised funktsioonid ei tööta sel ajal.

-   Redis konsooli - konsool Redis kasutab majutatud VMs, mis ei kuulu teie VNET redis-cli.exe klient, seda ei saa ühendust luua, kuna vahemälu.


## <a name="use-expressroute-with-azure-redis-cache"></a>Azure'i Redis vahemälu ExpressRoute kasutamine

Klientide saate luua ühenduse mõne [Azure'i ExpressRoute](https://azure.microsoft.com/services/expressroute/) ringi oma virtuaalse võrgu taristule, seega ulatub oma kohapealse võrgu Azure. 

Vaikimisi vastloodud ExpressRoute ringi reklaamib vaikimisi marsruudi, mis võimaldab väljaminev Interneti-ühendus. Selle konfiguratsiooni puhul on klientrakendustes saab ühendada muude Azure lõpp-punktid, sh Azure Redis vahemälu.

Levinud kliendi konfiguratsiooni on siiski määratleda oma vaikimisi protsessi (0.0.0.0/0), mis jõustab hoopis liikumist kohapealse väljaminevate Interneti-liikluse. See liiklust alati piirid Ühenduvus Azure Redis vahemälu, kuna väljaminev liiklus on kas blokeeritud kohapeal või NAT sooviksite tundmatu kogum, mis pole enam töötamine erinevate Azure lõpp-punktid.

Lahendus on määratleda alamvõrgu, mis sisaldab Azure'i Redis vahemälu (vähemalt ühe) kasutaja määratletud marsruudib (UDRs). Mõne UDR määratleb alamvõrgu kohased protsessid, mis on au asemel vaikimisi marsruudi.

Võimaluse korral on soovitatav kasutada järgmist konfiguratsiooni:

- ExpressRoute konfiguratsiooni reklaamib 0.0.0.0/0 ja vaikimisi Jõusta tunnelite kogu väljaminev liiklus kohapealse.
- On rakendatud Azure'i Redis vahemälu alamvõrgu UDR määratleb 0.0.0.0/0 järgmise sõnumihüppe kohta, pärast tüüpi Internet (kaugemal allapoole selles artiklis on näide selle).

Kombineeritud mõju järgmist on, et alamvõrgu tase UDR ülimuslik ExpressRoute sunnitud tunneling, tagades seega Azure'i Redis vahemälust väljaminev Interneti-ühendus.

Kuigi ühenduse näiteks Azure'i Redis vahemälu asutusesisese rakenduses, valides ExpressRoute ei ole tüüpiline kasutus stsenaarium jõudluse põhjustel (jaoks parima jõudluse tagamiseks Azure'i Redis vahemälu kliendid peaks olema sama piirkonna Azure'i Redis vahemälu) Selle stsenaariumi väljaminev võrguteed ei saa läbida sisemise ettevõtte puhverserverite samuti ei saa Jõusta kohapealse tunneled. Nii muudab Azure'i Redis vahemälust efektiivse NAT aadressi väljaminevaid võrguliiklust. Azure'i Redis vahemälu NAT aadressi muutmine astme väljaminevaid võrguliiklust põhjustab ühenduvuse tõrkeid paljudele ülaltoodud lõpp-punktid. Selle tulemuseks on nurjunud Azure Redis vahemälu loomine katsed.

**Oluline:**  Marsruudib, mis on määratletud UDR **peab** olema seotud ülimuslik mis tahes marsruudib reklaamida ExpressRoute konfiguratsioon. Järgmine näide kasutab laialdane 0.0.0.0/0 aadress vahemiku ja sellisena saab potentsiaalselt kogemata tühistada marsruutimiseks reklaamide abil täpsemate aadresside vahemikud.

**Väga oluline:**  Azure'i Redis vahemälu pole toetatud ExpressRoute konfiguratsioone selle **valesti rist reklaamida marsruudib avaliku silmitsemine tee privaatne silmitsemine tee kaudu**. ExpressRoute konfiguratsiooni, mis on avalik silmitsemine konfigureeritud, saate marsruutimiseks reklaamide Microsoftilt suure hulga Microsoft Azure'i IP-aadresside vahemikud. Kui nende aadressid on valesti cross reklaamida privaatne silmitsemine tee, seetõttu, et on kogu väljaminev võrgu pakette Azure'i Redis vahemälu eksemplari alamvõrgu valesti Jõusta tunneled soovite kliendi kohapealse võrgu taristule. See võrgu vool piirid Azure'i Redis vahemälu. Selle probleemi lahendamiseks on peatada rist-reklaami marsruudib avaliku silmitsemine tee privaatne silmitsemine tee.

Kasutaja määratletud marsruudib kohta on saadaval sellest [Ülevaade](../virtual-network/virtual-networks-udr-overview.md). 

ExpressRoute kohta leiate lisateavet teemast [ExpressRoute tehniline ülevaade](../expressroute/expressroute-introduction.md)

## <a name="next-steps"></a>Järgmised sammud
Saate teada, kuidas kasutada rohkem vahemälu lisafunktsioonidele.

-   [Taseme Azure'i Redis vahemälu Premiumi tutvustus](cache-premium-tier-intro.md)





  
<!-- IMAGES -->

[redis-cache-vnet]: ./media/cache-how-to-premium-vnet/redis-cache-vnet.png

[redis-cache-vnet-ip]: ./media/cache-how-to-premium-vnet/redis-cache-vnet-ip.png

[redis-cache-vnet-info]: ./media/cache-how-to-premium-vnet/redis-cache-vnet-info.png

