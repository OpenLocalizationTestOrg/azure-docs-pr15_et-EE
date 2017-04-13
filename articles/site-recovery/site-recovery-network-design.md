<properties
    pageTitle="Võrgu taristule tõrkejärgseks kujundamiseks | Microsoft Azure'i"
    description="Selles artiklis käsitletakse võrgu kujundamine Azure saidi taastamine"
    services="site-recovery"
    documentationCenter=""
    authors="prateek9us"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="09/19/2016"
    ms.author="pratshar"/>

#  <a name="designing-your-network-infrastructure-for-disaster-recovery"></a>Võrgu taristule tõrkejärgseks kujundamine

Selles artiklis on suunatud IT-spetsialistid saavad eest architecting, rakendamise ja toetavad järjepidevuse ja katastroofi taastamine (BCDR) taristu ja kes soovivad Microsoft Azure saidi taastamine (ASR) toetavad ja täiustamiseks BCDR teenuseid kasutada. Käsitleme praktiline kaalutluste kohta System Center virtuaalse masina Manager serveri juurutamine, spetsialistid ja miinuste vs alamvõrgu Tõrkesiirde venitatud alamvõrku, ja kuidas struktureerimine avariitaastet Microsoft Azure virtuaalse saitidele.

## <a name="overview"></a>Ülevaade

[Azure'i saidi taastamine (ASR)](https://azure.microsoft.com/services/site-recovery/) on Microsoft Azure'i teenus, mis orchestrates kaitsmise ja taastamise oma äri järjepidevus disaster taastamine (BCDR) eesmärgil virtualiseeritud rakendused. See dokument on mõeldud juhend lugeja juhendab käivitusviisard kujundamiseks võrke, keskendudes architecting IP-aadresside vahemikud ja alamvõrku saidil katastroofi taastamine, kui imitatsiooniga abil saidi taastamine virtuaalmasinates (VMs).

Lisaks on see artikkel näitab, kuidas saidi taastamine võimaldab architecting ja rakendamine mitmes kohas virtuaalse andmekeskuse toetamiseks BCDR teenuste ajal testi või tõttu.

Maailmas, kus kõik eeldab, et 24/7 Ühenduvus, see on oluline kui kunagi hoida oma ja rakenduste ja käivitatud. Süsteemi järjepidevuse ja katastroofi taastamine (BCDR) on nurjunud komponendid taastada nii asutuses saate kiiresti jätkata toiminguid. Väljatöötamisel katastroofi taastamine tegeleda tõenäoline, laastavad sündmused on väga lihtsa. Selle põhjuseks on omased raske prognoosida tulevikus, eriti siis, kui ebatõenäolisi sündmusi ja esitada asjakohased meetmed ulatusliku katastroofe kaitse kõrge hind. 

Oluline BCDR kavandamise, taastamise aja eesmärk (RTO) ja taastamise punkti eesmärk (RPO) tuleb määratleda katastroofi taastamise kava osana. Kui katastroof kliendi andmekeskuse, kasutades Azure saidi taastamise, kliendid saavad kiiresti (väike RTO) tuua võrgus oma tiražeeritud virtuaalmasinates asub teisel andmekeskuse või Microsoft Azure'i minimaalne andmekao (väike RPO). 

Tõrkesiirde on võimalikuks ASR algselt kopeerib määratud virtuaalmasinates esmane andmekeskuse teisene andmekeskuse või Azure (olenevalt stsenaarium), mis värskendab perioodiliselt selle koopiad. Ajal taristu kavandamise, käsitleda võrgu kavandamise kitsaskoha, mis võivad takistada teie ettevõttest koosoleku RTO ja RPO eesmärgid.  

Administraatorid kavandamisel juurutamiseks katastroofi taastamise lahendus üks klahv küsimustele meelt on kuidas virtuaalse masina oleks kättesaadav pärast selle Tõrkesiirde on lõpule viidud. ASR võimaldab valida võrgu, millele soovite ühendada virtuaalse masina pärast Tõrkesiirde administraator. Kui esmane saidi haldab VMM server, siis selle saavutamiseks vastendades võrgu kaudu. Üksikasjalikumat teavet teemast [võrgu kaardistamine ettevalmistamine](site-recovery-network-mapping.md) .

Ajal võrgu taastamine saidi kujundamise, administraator on kaks võimalust.

- Kasutage muu IP-aadresside vahemiku võrgu taastamine saidi. Selle stsenaariumi virtuaalse masina pärast Tõrkesiirde saavad uue IP-aadress ja administraator on seda teha DNS-i värskendamine. Lugege lisateavet selle kohta, kuidas teha DNS-i värskendamine [siin](site-recovery-vmm-to-vmm.md#test-your-deployment) 
- Kasutage sama IP-aadresside vahemiku võrgu taastamine saidi. Teatud stsenaariumide administraatorid soovi säilitada IP-aadressid, et neil on esmane saidil, isegi pärast selle Tõrkesiirde. Tavaline stsenaarium oleks administraator värskendada marsruudib tähistamiseks IP-aadresside uude asukohta. Kuid seda stsenaariumi, kus venitatud VLAN juurutatakse esmast ja taastamise saitide vahel, säilitades selle virtuaalmasinates IP-aadresside muutub atraktiivseks. Säilitamise sama IP aadressid lihtsustab taastamine kasutades mis tahes võrk seotud postituse-Tõrkesiirde juhiseid.


Administraatorid kavandamisel juurutamiseks katastroofi taastamise lahendus üks klahv küsimusi meelt on kuidas rakendusi kättesaadav pärast selle Tõrkesiirde on lõpule viidud. Tänapäevased rakendused sõltuvad peaaegu alati võrgunduse mõningal määral nii füüsilise teisaldamine teise ühe saidilt teenuse võrgu protsess. On kaks peamist võimalust, mis selle probleemi lahendamiseks katastroofi taastamine lahendusi. Esimene lähenemine on fikseeritud IP-aadresside säilitada. Vaatamata teisaldamine teenuseid ja majutusteenuse serverid on erinevast, rakenduste võtta IP-aadressi konfiguratsiooni nendega uude asukohta. Teine lähenemine hõlmab täielikult muutmise ajal ülemineku taastatud saidi IP-aadress. Iga lähenemine on mitu rakendamist variatsioonid, mis on kokku võetud allpool.

Ajal võrgu taastamine saidi kujundamise, administraator on kaks võimalust.

## <a name="option-1-retain-ip-addresses"></a>Variant 1: Säilitamise IP-aadressid 

Seisukohast katastroofi taastamise protsessi kasutades fikseeritud IP aadressid näib olevat lihtsaim viis rakendada, kuid see on võimalikud probleemid, mis oleks vähemalt populaarne lähenemine arv. Azure'i saidi taastamine pakub võimalust säilitamise kõigi stsenaariumide IP-aadressid. Enne, kui üks otsustab säilitamise IP, tuleks vastav mõtte piiranguid esitab Tõrkesiirde võimalused. Vaatame tegureid, mis aitavad teil otsustada säilitamise IP-aadressid, või mitte. Seda saavutada kahel viisil, kasutades venitatud alamvõrgu või täielik alamvõrgu Tõrkesiirde tehes.

### <a name="stretched-subnet"></a>Venitatud alamvõrgu

Siin on alamvõrgu teha kättesaadavaks korraga nii esmast ja DR asukohad. Lihtsalt see tähendab, saate teisaldada server ja IP (Layer 3) konfiguratsioon teise saidi ja võrgu marsruutida liiklust uude asukohta automaatselt. See on triviaalne serveri perspektiivi tegelema, kuid see on mitmed probleemid:

- Kiht 2 (andmete link layer) seisukohalt vaja võrguseadmed, mida saate hallata venitatud VLAN, kuid see on muutunud väiksem probleem on nüüd saadaval. Teine ja keerulisem probleem on see, et, lükates VLAN võimaliku vea domeeni laiendatakse nii saite, tõrge ühtne põhiosas muutumas. See on tõenäoline, see on juhtunud saade storm alustamine, kuid ei saa isoleeritakse. Me ei leidnud kombineeritud arvamusi viimase probleemi ja on näha palju eduka rakendusi samuti "me kunagi rakendada selle tehnoloogia siin".
- Venitatud alamvõrgu ei ole võimalik, kui kasutate Microsoft Azure'i DR saidile.


### <a name="subnet-failover"></a>Alamvõrgu Tõrkesiirde

On võimalik rakendada alamvõrgu Tõrkesiirde venitatud alamvõrgu lahendus ulatub üle mitme saidi alamvõrgu ilma eespool kirjeldatud kasu saada. Siin kõik antud alamvõrgu oleks kohal saidi 1 või saidi 2, kuid mitte kunagi nii kohas korraga. IP-aadress ruumi korral Tõrkesiirde säilitamiseks on võimalik programmiliselt korraldada ruuteri infrastruktuuri soovitud alamvõrku teisele liikumine ühes kohas. Kaitstud Tõrkesiirde stsenaarium on alamvõrku liikuda koos seotud VMs. Peamine tagastamise seda moodust on kogu alamvõrku, mis võib olla OK liikumiseks peate tõrke korral, kuid see võib mõjutada Tõrkesiirde granulaarsus peaksite arvesse võtma. 

Uurime, kuidas on võimalik ise oma VMs taastamise asukohta samas vastasel üle kogu alamvõrgu väljamõeldud enterprise, nimega Contoso. Esmalt käsitleme kuidas Contoso suudab hallata oma alamvõrku ning samal ajal imitatsiooniga VMs kahe kohapealse asukohad, siis me arutada, kuidas alamvõrgu Tõrkesiirde töötab, kui [kasutatakse Azure katastroofi taastamine saidi](#failover-to-azure).

#### <a name="failover-to-a-secondary-on-premises-site"></a>Tõrkesiirde teisene kohapealse saidile

Vaadakem stsenaarium kui soovime säilitada IP iga VMs ja ei lõppenud täieliku alamvõrgu koos. Esmane saidil on rakenduste alamvõrgu 192.168.1.0/24 töötamine. Kui selle Tõrkesiirde juhtub, kõik virtuaalmasinates, mis on osa sellest alamvõrgu on üle ebaõnnestus taastamine saidi ja säilitada oma IP-aadressid. Marsruudib peavad olema õigesti asjaolu, et kuuluvad alamvõrgu 192.168.1.0/24 virtuaalmasinates liikunud nüüd taastamine saidi kajastamiseks muuta. 

Järgmisel joonisel marsruudib esmane saidi ja taastamine saidi, kolmanda saidi ja esmane saidi, ja kolmanda saidi taastamine saidi vahel peab olema õigesti muudetud. 

Järgmised pildid kuvatakse alamvõrku enne selle Tõrkesiirde. Alamvõrgu 192.168.0.1/24 enne selle Tõrkesiirde esmane saidil on aktiivne ja aktiveerub taastamine saidi pärast selle Tõrkesiirde 

![Enne Tõrkesiirde](./media/site-recovery-network-design/network-design2.png)

Enne Tõrkesiirde


Alloleval pildil on võrgu ja alamvõrku pärast Tõrkesiirde.
    
![Pärast Tõrkesiirde](./media/site-recovery-network-design/network-design3.png)

Pärast Tõrkesiirde

Teisene saidil on kohapealse ja VMM server abil hallata seda, siis teatud virtuaalse masina kaitse lubamisel ASR eraldab järgmised töövoo võrgu ressursse:

- ASR eraldab IP-aadress iga võrgu liidese virtual arvutisse staatilise IP address kogumi määratletud aktsepteerimist iga System Center VMM eksemplari.
- Kui administraator määratleb sama IP address rakenduskausta võrgu taastamine saidi IP address kogumi võrgu esmane saidil, kui ajal eraldamise koopia virtuaalse masina ASR IP-aadressi soovite eraldada sama IP-aadress see peamine virtuaalse masina nimega.  Reserveeritud IP VMM, kuid pole määratud Tõrkesiirde IP. Vahetult enne selle Tõrkesiirde on seatud Tõrkesiirde IP.

![Säilitamise IP-aadress](./media/site-recovery-network-design/network-design4.png)
    
Joonis 5

Joonis 5 kuvatakse Tõrkesiirde TCP/IP sätete koopia virtuaalse masina (Hyper-V konsool). Nende sätete oleks täidetud just enne virtuaalse masina käivitatakse pärast Tõrkesiirde

Kui sama IP pole saadaval, peaksite ASR eraldada kaustast määratletud IP address mõne muu saadaval IP-aadress. 

Pärast VM on lubatud kaitse abil saate kontrollida IP, mis on eraldatud virtuaalse masina jälgima skripti näide. Sama IP oleks seatud Tõrkesiirde IP ja määratud VM Tõrkesiirde ajal.

        $vm = Get-SCVirtualMachine -Name <VM_NAME>
        $na = $vm[0].VirtualNetworkAdapters>
        $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
        $ip.address  

>[AZURE.NOTE] Kus virtuaalmasinates kasutada DHCP stsenaariumi IP-aadresside haldamine on täielikult ASR ei kontrolli. Veenduge, et IP-aadresside taastamine saidi serveeritakse DHCP server võib olla sama vahemiku see peamine saidi peab administraator.

#### <a name="failover-to-azure"></a>Azure'i Tõrkesiirde

Azure'i saidi taastamine (ASR) võimaldab kasutada katastroofi taastamine saidi jaoks oma virtuaalmasinates Microsoft Azure'i. Sel juhul peate tegelema ühe rohkem piirang. 

Analüüsime stsenaarium, kus nimega Woodgrove panga väljamõeldud ettevõttel on kohapealse taristu majutusteenuse ärirakenduste nende rida ja nad on hosting oma mobiilirakenduste Azure. Woodgrove panga VMs Azure'i ja kohapealsete serverite vahelist ühenduvust on esitatud, on saidi saidi (S2S) virtuaalse privaatvõrgu (VPN). S2S VPN võimaldab Woodgrove panga virtuaalse võrgu Azure Woodgrove panga kohapealse võrgu laiendit pidada. See teatis lubamiseks S2S VPN Woodgrove panga serva ja Azure virtuaalse võrgu vahel. Nüüd soovib Woodgrove ASR abil saate ise oma töötab selle andmekeskuse Azure'i töökoormus. See suvand vastab Woodgrove, mis soovib ökonoomne DR suvandi ja suudab avaliku pilves keskkonnad andmete talletamiseks. Woodgrove peab tegelema rakenduste ja konfiguratsioone, mis sõltuvad raske koodiga IP-aadressid, seega need on pärast probleemse Azure IP-aadressid oma taotlusi säilitamise nõue.

Woodgrove otsustanud määrata IP-aadressid, IP-aadresside vahemiku (172.16.1.0/24, 172.16.2.0/24) oma ressursse, mis on Azure töötab.

Woodgrove saama ise oma virtuaalmasinates Azure säilitades IP-aadressid, peab muu Azure virtuaalse kaudu luua. Kohapealse võrgu laiendit peaks olema nii, et rakendusi saab Tõrkesiirde kohapealse saidi Azure sujuvalt. Azure'i saate lisada saidilt kui ka punkti saidi virtuaalse Privaatvõrgu ühendus loodud Azure virtuaalse võrgud. Saitide ühenduse loomisel saate Azure võrgu marsruutida liikluse kohapealse asukohta (Azure'i helistab see kohalik – võrgu) ainult siis, kui IP-aadresside vahemiku erineb kohapealse IP address vahemikus, kuna Azure'i ei toeta venitamine alamvõrku.  See tähendab, et kui teil on kohapealse alamvõrgu 192.168.1.0/24, ei saa lisada kohalik – võrgu 192.168.1.0/24 Azure võrku. See on oodata, kuna Azure'i ei tea alamvõrgu on pole aktiivne VMs ja et alamvõrgu on loodud DR eesmärgil. Saama õigesti marsruutimine alamvõrku võrgu ja kohalik – võrgu ei tohi olla vastuolus Azure väljaspool võrguliiklust. 

![Enne alamvõrgu Tõrkesiirde](./media/site-recovery-network-design/network-design7.png)

Enne Tõrkesiirde

Selleks, et Woodgrove täita oma ettevõtte nõuetele, läheb vaja rakendada järgmised töövood:

- Luua täiendavaid, andke meile selle kõne taastamine võrgu, kus luuakse virtuaalmasinates nurjus üle.
- Veenduge, et IP VM alles pärast Tõrkesiirde, valige jaotises VM atribuudid vahekaarti konfigureerimine Määrake sama IP, et VM on kohapealse ja seejärel klõpsake nuppu Salvesta. Kui kursor on nurjunud VM, määrata Azure saidi taastamise esitatud IP virtuaalse masina. 

![Atribuudid](./media/site-recovery-network-design/network-design8.png)

Pärast selle Tõrkesiirde käivitatakse ja selle virtuaalmasinates luuakse taastamine võrgu soovitud IP, ühenduvuse võrguga kindlaks, on [Vnet Vnet ühenduse](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)abil. Vajadusel saate seda toimingut kirjutasid.  Nagu me eelmises jaotises kirjeldatud kohta alamvõrgu Tõrkesiirde, isegi kui tegemist on Tõrkesiirde Azure, marsruudib oleks õigesti vastavalt selle 192.168.1.0/24 asunud nüüd Azure muuta. 

![Pärast alamvõrgu Tõrkesiirde](./media/site-recovery-network-design/network-design9.png)

Pärast Tõrkesiirde

Kui teil pole Azure'i võrgu, nagu on näidatud pildil. Pärast selle Tõrkesiirde saate luua saidilt vpn-ühendus oma "Esmane saidi" ja "Taastamine võrgustik" vahel.  


## <a name="option-2-changing-ip-addresses"></a>Variant 2: Muutmise IP-aadressid

Seda moodust ilmselt kõige rohkem levinud põhjal, mida me näinud. Selle vormiks muuta iga VM, mis on seotud selle Tõrkesiirde IP-aadress. Seda moodust kasutatav nõuab sissetulevate võrgu "õppida", et IPx olnud rakendus on nüüd IPy. Isegi kui IPx ja IPy on loogilised nimed, DNS-i kirjed on tavaliselt tühjendada kogu võrgus, või muudetud ja võrgu tabelite vahemällu talletatud kirjed on värskendatud või tühjendada, seetõttu on tööseisakute näha sõltuvalt sellest, kuidas DNS-i taristu on häälestus. Järgmiste probleemide korral saab leevendada madal TTL väärtuste puhul sisevõrgu rakenduste abil ja [Azure liikluse sõim ASR koos](http://azure.microsoft.com/blog/2015/03/03/reduce-rto-by-using-azure-traffic-manager-with-azure-site-recovery/) kasutamise rakendustest

### <a name="changing-the-ip-addresses---illustration"></a>IP-aadressid - joonisel muutmine

Vaatame seda stsenaariumi kui kavatsete kasutada eri IP-d esmast ja taastamise saidid. Järgmises näites oleme ka on kolmanda kohas, kus rakendused majutatud esmase või taastamine sait on avatud.

![Erinevate IP - enne Tõrkesiirde](./media/site-recovery-network-design/network-design10.png)

Joonis 11

Joonisel 11 on mõned rakendused majutatud alamvõrgu 192.168.1.0/24 alamvõrgu esmane saidi ja pärast Tõrkesiirde taastamine saidi alamvõrgu 172.16.1.0/24 tulevad konfigureerinud. VPN ühendused/võrgu marsruudib olla kõigi kolme saitide pääseks juurde üksteisest õigesti konfigureeritud.
 
Nagu joonis 12 näitab, pärast probleemse üks või mitu rakendust, taastatakse need alamvõrgu taastamine. Sel juhul me ei jää ühe Tõrkesiirde kogu alamvõrgu soovite samal ajal. VPN- või võrgu marsruudib konfigureerimiseks peavad muudatusi. Tõrkesiirde ja mõned DNS-i värskendused on veenduge, et rakenduste kättesaadavus. Kui DNS-i on konfigureeritud lubama dünaamiline värskendused siis soovitud virtuaalmasinates registreeruma ise, kasutades uus IP, kui nad alustada pärast Tõrkesiirde. 

![Erinevate IP - pärast Tõrkesiirde](./media/site-recovery-network-design/network-design11.png)

Joonis 12

Pärast vastasel üle virtuaalse masina koopia võib olla IP-aadress, mis pole sama, mis esmane virtuaalse masina IP-aadress. Virtuaalmasinates update DNS-i server, mida nad kasutavad pärast Alustuseks. DNS-i kirjed on tavaliselt tühjendada kogu võrgu või muudetud ja võrgu tabelite vahemällu talletatud kirjed on värskendatud või tühjendada, seega ei ole vähekasutatud esinema tööseisakute ajal toimuda nende muutuste. Selle probleemi saab leevendada:

- Sisevõrk rakenduste abil TTL väiksed väärtused.
- Azure'i liikluse sõim abil ASR internet põhinevaid rakendusi.
- Sees oma taastamise kava järgmise skripti abil värskendada DNS-i Server tagada õigeaegne värskendus (skript ei ole vaja kui dünaamiline DNS-i registreerimise on konfigureeritud)

        string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord


### <a name="changing-the-ip-addresses--dr-to-azure"></a>IP-aadresside – DR Azure muutmine

[Networking taristu häälestamise Microsoft Azure'i katastroofi taastamine saitide](http://azure.microsoft.com/blog/2014/09/04/networking-infrastructure-setup-for-microsoft-azure-as-a-disaster-recovery-site/) ajaveebipostituse selgitab, kuidas Setup nõutav Azure võrgu infrastruktuur kui säilitades IP-aadressid pole nõue. See algab kirjeldav rakendus ja otsige üles rakendus kohapealse häälestamine ja Azure ja seejärel millega lõpetatakse ja kuidas seda teha test Tõrkesiirde ja kavandatud Tõrkesiirde.



## <a name="next-steps"></a>Järgmised sammud

[Saate teada](site-recovery-network-mapping.md) , kuidas saidi taastamine kaartide lähte- ja võrgu kui VMM server kasutab esmane saidi haldamiseks. 
