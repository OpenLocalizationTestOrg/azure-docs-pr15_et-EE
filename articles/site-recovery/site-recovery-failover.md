<properties
    pageTitle="Saidi taastamine Tõrkesiirde | Microsoft Azure'i" 
    description="Azure'i saidi taastamine koordinaadid dispersioonanalüüs, Tõrkesiirde ja virtuaalmasinates ja füüsilise serveri. Teavet Tõrkesiirde Azure'i või sekundaarne andmekeskuse." 
    services="site-recovery" 
    documentationCenter="" 
    authors="rayne-wiselman" 
    manager="jwhit" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery" 
    ms.date="10/05/2016" 
    ms.author="raynew"/>

# <a name="failover-in-site-recovery"></a>Tõrkesiirde saidi taastamine

Azure'i saidi taastamise teenuse aitab teie ettevõtte järjepidevus ja avariijärgne taastamine (BCDR) strateegia orkesteroinnin dispersioonanalüüs, Tõrkesiirde ja virtuaalmasinates ja füüsilise serveri. Masinad saab paljundada Azure'i või sekundaarne asutusesisese andmekeskuse. Kiire ülevaate saamiseks lugege [mis on Azure saidi taastamise?](site-recovery-overview.md)

## <a name="overview"></a>Ülevaade

Sellest artiklist leiate teavet ja juhiseid taastamine (uuesti üle ning ei suuda) virtuaalmasinates ja füüsilise serveri, mis on kaitstud saidi taastamine. 

Postitada kommentaare või küsimusi allosas käesoleva artikli või [Azure taastamise teenused Foorum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="types-of-failover"></a>Tõrkesiirde tüübid

Pärast kaitse on lubatud virtuaalmasinates ja füüsilise serveri ja ta olete imitatsiooniga käivitada failovers vastavalt vajadusele. Saidi taastamine toetab tüüpi Tõrkesiirde arvu.

**Tõrkesiirde** | **Millal käivitamine** | **Üksikasjad** | **Protsess**
---|---|---|---
**Test Tõrkesiirde** | Kinnitage oma dispersioonanalüüs strateegia või teha katastroofi taastamine drill käivitamine | Andmete kaotsimineku ega tööseisakute.<br/><br/>Mingit mõju dispersioonanalüüs<br/><br/>Mingit mõju tootmiskeskkonnast | Käivitage on tõrkesiire<br/><br/>Määrake, kuidas testi masinad olema ühendatud võrke pärast Tõrkesiirde<br/><br/>**Klõpsake vahekaarti** edenemist jälgida. Test on loodud ja teisene asukoha käivitamine<br/><br/>Azure'i - ühenduse loomiseks arvuti Azure'i portaalis<br/><br/>Teisene saidi – seadme sama host ja pilvepõhise juurdepääsu<br/><br/>Täitke testimine ja automaatselt puhastamiseks test Tõrkesiirde sätted.
**Kavandatud Tõrkesiirde** | Käivita nõuetele vastavus<br/><br/>Kavandatud hooldustööde käivitamine<br/><br/>Kasutada andmeid hoida töökoormus töötab teadaolevad katkestuste jaoks – näiteks tõrke oodatud power või raske ilmateadet käivitamine<br/><br/>Käivitada failback pärast Tõrkesiirde: esmane, teisene abil | Andmete kaotsimineku pole<br/><br/>Tööseisakute on arvestatavaid aega kulub virtual arvuti esmase sulgeda ja avab teisene asukohale.<br/><br/>Virtuaalmasinates on mõju target masinad muutub allika masinad pärast Tõrkesiirde. | Käivitage on tõrkesiire<br/><br/>**Klõpsake vahekaarti** edenemist jälgida. Andmeallika masinad sulgema<br/><br/>Koopia masinad alustada teisene asukoht<br/><br/>Azure'i - ühenduse koopia arvutisse Azure'i portaalis<br/><br/>Teisene saidi – juurdepääs masina sama Host ja sama pilveteenuses<br/><br/>Kinnita on tõrkesiire
**Planeerimata Tõrkesiirde** | Seda tüüpi Tõrkesiirde reaktiivne viisil käivitada, kui esmane saidi muutub kättesaamatuks ootamatu juhtum, nt power katkestuste või viiruse rünnak <br/><br/> Saate käivitada planeerimata Tõrkesiirde saab teha ka siis, kui esmane sait pole saadaval. | Andmete kaotsimineku sõltuvad dispersioonanalüüs sagedus sätted<br/><br/>Andmeid saab vastavalt selle sünkrooniti viimast ajakohane | Käivitage on tõrkesiire<br/><br/>**Klõpsake vahekaarti** edenemist jälgida. Võite proovida virtuaalmasinates sulgeda ja nende sünkroonimine värskeimad andmed<br/><br/>Koopia masinad alustada teisene asukoht<br/><br/>Azure'i - ühenduse koopia arvutisse Azure'i portaalis<br/><br/>Saidi juurdepääsu masina sama Host ja sama pilveteenuses<br/><br/>Kinnita on tõrkesiire


Failovers, mis on toetatud tüüpi sõltuvad teie juurutamise stsenaariumi.

**Tõrkesiirde suund** | **Test Tõrkesiirde** | **Kavandatud Tõrkesiirde** | **Planeerimata Tõrkesiirde**
---|---|---|---
Esmane VMM saidi teisene VMM saidile | Toetatud | Toetatud | Toetatud 
VMM saidi esmane VMM saidile | Toetatud | Toetatud | Toetatud
Pilv pilve (ühe VMM server) |  Toetatud | Toetatud | Toetatud
VMM saidi Azure | Toetatud | Toetatud | Toetatud 
Azure'i VMM saidile | Pole toetatud | Toetatud | Pole toetatud 
Azure'i saidi Hyper-V | Toetatud | Toetatud | Toetatud
Azure Hyper-V saidile | Pole toetatud | Toetatud | Pole toetatud
Azure'i Vmware'i saidil | Toetatud (täiustatud stsenaarium)<br/><br/> Mittetoetatavad (pärand stsenaarium) |Pole toetatud | Toetatud
Füüsiline server Azure'i | Toetatud (täiustatud stsenaarium)<br/><br/> Mittetoetatavad (pärand stsenaarium) | Pole toetatud | Toetatud

## <a name="failover-and-failback"></a>Tõrkesiirde ja failback

Teil ei õnnestu üle teisene kohapealse saidi või Azure'i virtuaalmasinates sõltuvalt juurutamise. Arvuti, mis ei üle Azure'i luuakse on Azure virtuaalse masina. Teil võib nurjuda üle ühe virtuaalse masina või füüsilise serveri või taastamis. Taastamise sisaldab ühte või rohkem tellitud rühmad, mis sisaldavad kaitstud virtuaalmasinates või füüsilise serveri. Kasutatakse neid korraldab Tõrkesiirde vastavalt rühma ei õnnestu üle mitme masinad need asuvad. [Lisateavet vt](site-recovery-create-recovery-plans.md) taastamise kohta. 

Pärast Tõrkesiirde on lõpule jõudnud ja teie masinad on tööks teisene kohas, Pange tähele järgmist.

- Kui te ei ole üle Azure'i pärast Tõrkesiirde masinad pole kaitstud või imitatsiooniga põhi- või asukoht. Azure on virtuaalmasinates on talletatud geo kopeeritud salvestusruumi, mis pakub paindlikkust, kuid mitte kopeerimine.
- Kui te planeerimata Tõrkesiirde saidi, kui Tõrkesiirde masinad teisene asukohas pole kaitstud või imitatsiooniga.
- Kui tegite pärast Tõrkesiirde masinad teisene asukohas on kavandatud Tõrkesiirde saidi, on kaitstud.
 

### <a name="failback-from-secondary-site"></a>Saidi migreerite

Saidi migreerite lisamine kasutab sama protsessi Tõrkesiirde esmane, teisene. Pärast kinnitatud Tõrkesiirde: esmane, teisene andmekeskusse on lõpule jõudnud, saate algatada tagant dispersioonanalüüs esmane saidi vabanemise. Pööratud dispersioonanalüüs alustab dispersioonanalüüs saidi ja esmane, delta andmete sünkroonimine. Pööratud dispersioonanalüüs toob selle virtuaalmasinates kaitstud olekusse, kuid teisene andmekeskuse on endiselt aktiivne asukoht. Selleks, et esmane saidi annate kavandatud Tõrkesiirde, et esmane, teisene, millele järgneb teise tagant dispersioonanalüüs aktiivne asukohta.

### <a name="failback-from-azure"></a>Azure'i migreerite

Kui olete nurjus üle Azure'i virtuaalmasinates oma on kaitstud paindlikkust Azure'i virtuaalmasinates funktsioonid. Tehke algse esmane saidi aktiivne asukohta käivitage kavandatud Tõrkesiirde. Selle algsesse asukohta või alternatiivse asukoha võib nurjuda, kui algse saidi pole saadaval. Imitatsiooniga failback see peamine asukohta pärast uuesti alustamiseks annate ümberpööratud koopia.

### <a name="failover-considerations"></a>Tõrkesiirde kaalutlused

- **IP-aadressi pärast Tõrkesiirde**– poolt vaikimisi on nurjunud üle arvuti on kui allikas seadme eri IP-aadress. Kui soovite säilitada sama IP-aadressi vt: 
    - **Saidi**– kui te ei suuda üle saidi ja soovite säilitada mõne IP address [Lugege](http://blogs.technet.com/b/scvmm/archive/2014/04/04/retaining-ip-address-after-failover-using-hyper-v-recovery-manager.aspx) artiklit. Pange tähele, et saate säilitada avaliku IP-aadressi, kui Interneti-teenuse pakkuja toetab.
    - **Azure'i**– kui te ei suuda üle Azure saate määrata soovite määrata virtuaalse masina atribuudid vahekaardil **konfigureerimine** IP-aadress. Pärast Tõrkesiirde Azure ei saa alles jätta avaliku IP-aadressi. Saate säilitada RFC 1918 aadress tühikuid, mida kasutatakse sisemise aadressid.
- **Osalise Tõrkesiirde**– kui soovite kasutada saidi osa asemel anda kogu saidi pidage meeles järgmist: 
    - **Saidi**– kui teil ei õnnestu üle osa esmane saidi saidi ja soovite taas esmane saidiga ühenduse, abil-saidilt VPN-ühendus ühendamine nurjus rakenduste saidi taristu komponendid töötab esmane sait. Kui ka kogu alamvõrgu ei üle virtuaalse masina IP-aadressi saate säilitada. Kui teil ei õnnestu üle osalise alamvõrgu ei saa säilitada virtuaalse masina IP-aadress, kuna alamvõrku ei saa saitide vahel tükeldada.
    - **Azure'i**– kui teil ei õnnestu üle osalise saidi Azure ja soovite tagasi esmane saidiga ühenduse, saate VPN saidilt ühenduse üle rakenduse Azure taristu komponendid töötab esmane sait on nurjunud. Pange tähele, et kui säilib kogu alamvõrgu nurjub teie üle virtuaalse masina IP-aadress. Kui teil ei õnnestu üle osalise alamvõrgu ei saa säilitada virtuaalse masina IP-aadress, kuna alamvõrku ei saa saitide vahel tükeldada.
 
- **Täht**– kui soovite säilitada ketas kirja virtuaalmasinates pärast Tõrkesiirde saate seada virtuaalse masina SAN poliitika **kohta**. Virtuaalse masina ketast veebis automaatselt. [Lisateavet vt](https://technet.microsoft.com/library/gg252636.aspx).
- **Marsruutimiseks klientide päringutele**– saidi taastamise töötab Azure'i liikluse haldur marsruutimiseks kliendi taotlustele rakenduse pärast Tõrkesiirde.




## <a name="run-a-test-failover"></a>Käivitage test Tõrkesiirde

Test Tõrkesiirde käivitamisel palutakse teil valige testi koopia masinad võrgu sätteid. Teil on mitu võimalust.  

**Test Tõrkesiirde suvand** | **Kirjeldus** | **Tõrkesiirde kontrollimine** | **Üksikasjad**
---|---|---|---
**Azure'i ei õnnestu üle – ilma võrgu** | Ärge valige siht Azure võrgu | Tõrkesiirde kontrolle, mis on Azure virtuaalse masina käivitamise ootuspäraselt testimine | Kõik testi virtuaalmasinates taastamine leping lisatakse ühe pilveteenuses ja saate omavahel ühendada<br/><br/>Masinad ei ole pärast Tõrkesiirde Azure võrku ühendatud.<br/><br/>Kasutaja saab ühendada testi masinad avaliku IP-aadress
**Azure'i ei õnnestu üle – võrguga** | Valige siht Azure võrgu | Tõrkesiirde kontrollib võrku ühendatud testi masinad | Looge Azure võrk, mis on eraldatud Azure tootmise võrgu ja häälestamine taristu tiražeeritud virtual masina õigesti töötada.<br/><br/>Alamvõrgu testi virtuaalse masina põhineb alamvõrku, millel on nurjunud üle virtuaalse masina eeldatakse, et kui kavandatud või planeerimata Tõrkesiirde korral ühenduse.
**Üle nurjuda VMM saidi – ilma võrgu** | Ärge valige VM võrk | Tõrkesiirde kontrollib, kas testi masinad on loodud.<br/><br/>Testi virtuaalse masina luuakse sama nimega host, millel on olemas koopia virtuaalse masina Host. Ei lisata, kus asub koopia virtuaalse masina pilveteenusesse. | <p>Nurjunud seadme üle ei ühendatud võrgule.<br/><br/>Seade, saate VM võrku ühendatud pärast selle loomist
**Üle nurjuda VMM saidi – võrguga** | Valige olemasolevat VM võrku lisamine | Tõrkesiirde kontrollib, kas virtuaalmasinates on loodud | Testi virtuaalse masina luuakse sama nimega host, millel on olemas koopia virtuaalse masina Host. Ei lisata, kus asub koopia virtuaalse masina pilveteenusesse.<br/><br/>Looge VM võrk, mis on eraldatud tootmise võrgu kaudu<br/><br/>Kui kasutate VLAN põhineva võrgu soovitame loote eraldi loogilise võrgu (valmistamisel pole kasutatud) VMM selleks. See loogiline võrgu saab luua VM võrkude eesmärgil test Tõrkesiirde.<br/><br/>Loogilise võrgu peaks olema vähemalt üks võrguadapteri Hyper-V serverite hosting virtuaalmasinates seostatud.<br/><br/>VLAN loogilised võrgud, tuleks lisamist loogilise võrgu network saitide isoleeritakse.<br/><br/>Kui kasutate Windowsi võrgu Virtualization põhise loogilise võrgu, Azure saidi taastamise loob automaatselt eraldatud VM võrkudes.
**Üle nurjuda VMM saidi – võrgu loomine** | Ajutiste testi võrgu luuakse põhjal automaatselt teie määratud **Loogilise võrgu** ja selle seotud võrgu saitide säte | Tõrkesiirde kontrollib, kas virtuaalmasinates on loodud | Kasutage seda suvandit, kui taastamise kava kasutab rohkem kui üks VM võrk. Kui kasutate Windowsi võrgu Virtualization võrkude, selle suvandi saate luua automaatselt VM võrkude samade sätetega (alamvõrku ja IP-aadressi kaustu) koopia virtuaalse masina võrgus. Nende VM võrkude kustutatavate automaatselt, kui test Tõrkesiirde on lõpule jõudnud.</p><p>Testi virtuaalse masina luuakse sama nimega host, millel on olemas koopia virtuaalse masina Host. Ei lisata, kus asub koopia virtuaalse masina pilveteenusesse.

>[AZURE.NOTE] Edastatud virtuaalse masina test Tõrkesiirde ajal IP-aadress on sama, mis IP-aadress see saavad, kui tehes kavandatud või planeerimata Tõrkesiirde (eeldusel, et IP-aadress on test Tõrkesiirde võrgus saadaval. Kui sama IP-aadressi pole saadaval test Tõrkesiirde võrku siis virtuaalse masina saadetakse teise IP-aadressi test Tõrkesiirde võrgus saadaval.



### <a name="run-a-test-failover-from-on-premises-to-azure"></a>Käivitage test Tõrkesiirde asutusesisesest Azure

See toiming kirjeldab käivitamise test Tõrkesiirde taastamis jaoks. Teise võimalusena võite käivitada Tõrkesiirde ühe arvuti jaoks **Virtuaalmasinates** menüü.

1. Valige **Taastamise lepingud** > *recoveryplan_name*. Klõpsake **Tõrkesiirde** > **Test Tõrkesiirde**.
2. Määrake lehel **Kinnitage Test Tõrkesiirde** , kuidas koopia masinad ühendatakse Azure võrku pärast Tõrkesiirde.
3. Kui te ei suuda üle Azure ja andmete krüptimine on lubatud pilves, **Krüptovõtme** valige sert, mida kui märkisite andmete krüptimine pakkuja installimise ajal. 
4. **Klõpsake vahekaarti** Tõrkesiirde edenemist jälgida. Peaks olema näeksid testi koopia arvutisse Azure'i portaalis.
5. Pääsete koopia masinad Azure oma kohapealse saidi algatada RDP-ühendus virtuaalse masina. port 3389 tuleb lõpp-punkti jaoks virtuaalse masina avatud.
5. Kui olete lõpetanud, kui selle Tõrkesiirde jõuab **lõpuleviimine testimine** etapp, klõpsake nuppu **Katse** lõpuni.
5. **Märkmete** salvestamine ja salvestamine test Tõrkesiirde seostatud märkusi.
8. Klõpsake linki automaatselt puhastamiseks testi keskkonna **test Tõrkesiirde on lõpule viidud** . Kui see on lõpule jõudnud, kuvatakse test Tõrkesiirde C**omplete** olek.

> [AZURE.NOTE] Kui test Tõrkesiirde kestab kuni kaks nädalat täidetakse selle jõuga. Mis tahes elemente või luua automaatselt ajal test Tõrkesiirde virtuaalmasinates kustutatakse.
  

### <a name="run-a-test-failover-from-a-primary-on-premises-site-to-a-secondary-on-premises-site"></a>Käivitage test Tõrkesiirde saidilt esmane kohapealse teisene kohapealse saidi

Peate tegema mitmeid asju test Tõrkesiirde, sh selle domeenikontrolleri koopia tegemine ja testi DHCP ja DNS-i serverid paigutamine testimiskeskkonnas käivitamiseks. Saate teha seda mitmel viisil.

- Kui soovite mõne olemasoleva võrgu kaudu testi Tõrkesiirde käivitamiseks, ettevalmistamine Active Directory, DHCP ja DNS-i sellesse võrku.
- Kui soovite testi Tõrkesiirde suvandi abil saate luua VM automaatselt käivitada, lisada käsitsi samm enne rühma-1 kavatsete kasutada test Tõrkesiirde ja seejärel lisage infrastruktuur ressursid automaatselt loodud võrgu enne käivitamist test Tõrkesiirde taastamine lepingus.

#### <a name="things-to-note"></a>Asjad, mida

- Kui imitatsiooniga saidi, tüüp network, mida koopia arvutisse, ei pea vastavaks loogiline test Tõrkesiirde kasutatakse võrgu tüüpi, kuid ei pruugi mõned kombinatsioonid. Kui koopia kasutab DHCP ja eraldustaseme põhise VLAN, VM võrgu jaoks koopia ei pea staatilise IP-aadressi kogumi. Nii, et Windowsi võrgu Virtualization kasutamise katse Tõrkesiirde ei tööta, sest puudub aadress kaustu on saadaval. Lisaks test Tõrkesiirde ei tööta koopia võrk on ei eraldamise ja võrgu testimine Windowsi võrgu Virtualization. Selle põhjuseks ei eraldamise võrgus ei ole alamvõrku, luua Windowsi võrgu Virtualization võrgu nõutav.
- Nii, nagu koopia, mis on seotud virtuaalmasinates vastendatud VM võrgu pärast Tõrkesiirde sõltub VM võrgu konfigureerimist VMM konsooli:
    - **VM võrgu konfigureeritud ilma eraldi või VLAN eraldamise**– kui DHCP on määratletud VM võrgu, ühendatakse koopia virtuaalse masina VLAN ID sätted, mis on seotud loogika võrgu network saitide abil. Virtuaalse masina teile saadaval DHCP serveri IP-aadress. Te ei pea staatilise IP address kogumi target VM võrgu jaoks määratletud. Kui VM võrgu jaoks kasutatakse staatilise IP-aadressi kogumi koopia virtuaalse masina ühendatud VLAN ID sätted, mis on seotud loogika võrgu network saitide abil. Virtuaalse masina saate VM võrgu jaoks määratletud pool IP-aadress. Kui target VM võrgu staatilise IP-aadressi kogumi pole määratud, IP-aadress eraldatud nurjub. IP address rakenduskausta tuleks luua nii lähte- ja sihtsaitide kavatsete kasutada kaitsmise ja taastamise VMM serverites.
    - **Windows VM võrgu network virtualization**– kui VM võrk on konfigureeritud seda sätet staatilise kogumi määratleda target VM võrk, sõltumata sellest, kas andmeallika VM võrk on konfigureeritud kasutama DHCP või staatiline IP-aadresside pool. Kui määrate DHCP, target VMM server toimivad DHCP server ja IP-aadress kaustast, mis on määratletud ette target VM võrgu. Kui lähteserveriga jaoks määratletud kasutamine staatilise IP-aadressi kogumi, eraldada target VMM server IP-aadress pool. Mõlemal juhul IP address eraldatud nurjub, kui staatilise IP-aadressi kogumi pole määratletud.

#### <a name="run-test"></a>Käivitage test

See toiming kirjeldab käivitamise test Tõrkesiirde taastamis jaoks. Teise võimalusena võite käivitada Tõrkesiirde ühe virtuaalse masina või füüsilise serveri **Virtuaalmasinates** menüü.

1. Valige **Taastamise lepingud** > *recoveryplan_name*. Klõpsake **Tõrkesiirde** > **Test Tõrkesiirde**.
2. Määrake lehel **Testida Tõrkesiirde kinnitada,** kuidas virtuaalmasinates peaks olema ühendatud võrke test Tõrkesiirde pärast.
3. **Klõpsake vahekaarti** Tõrkesiirde edenemist jälgida. **Täielik testimine** etapp jõudmisel on tõrkesiire klõpsake **Täieliku testida** test Tõrkesiirde häälestuse lõpuleviimiseks.
4. Klõpsake nuppu **märkmed** salvestada ja salvestada märkusi, mis on seotud test Tõrkesiirde.
4. Kui see on lõpule jõudnud, et külastajad on virtuaalmasinates edukalt käivituda.
5. Pärast virtuaalmasinates edukalt käivituda, täitke test Tõrkesiirde puhastamiseks eraldatud keskkonnas. Kui valisite VM võrkude automaatseks loomiseks, Kettapuhastus kustutab kõik testi virtuaalmasinates ja võrgu testimine.

> [AZURE.NOTE] Kui test Tõrkesiirde kestab kuni kaks nädalat täidetakse selle jõuga. Mis tahes elemente või luua automaatselt ajal test Tõrkesiirde virtuaalmasinates kustutatakse.


#### <a name="prepare-dhcp"></a>DHCP ettevalmistamine

Kui soovitud virtuaalmasinates kaasatud test Tõrkesiirde kasutada DHCP, eraldatud võrgustikus, mis on loodud selleks, et test Tõrkesiirde luuakse test DHCP server.


### <a name="prepare-active-directory"></a>Ettevalmistamine Active Directory
Test Tõrkesiirde testimine rakenduse käivitamiseks peate koopia Active Directory tootmiskeskkonda testimiskeskkonnas. Läbida [testida Tõrkesiirde kaalutluste kohta active directory](site-recovery-active-directory.md#considerations-for-test-failover) lõik rohkem üksikasju. 


### <a name="prepare-dns"></a>DNS-i ettevalmistamine

Ettevalmistused DNS-i server test Tõrkesiirde järgmiselt:

- **DHCP**– kui virtuaalmasinates DHCP, katse DHCP server tuleks ajakohastada test DNS-i IP-aadress. Kui kasutate Windowsi võrgu Virtualization võrgu tüüp, toimib VMM server DHCP-server. Seetõttu peaks test Tõrkesiirde võrku värskendada DNS-i IP-aadress. Sel juhul on virtuaalmasinates register ise asjakohased DNS-i server.
- **Staatiline aadress**– kui virtuaalmasinates staatiline IP-aadress, test Tõrkesiirde võrku tuleks ajakohastada testi DNS-i serveri IP-aadress. Võib juhtuda värskendada DNS-i testi virtuaalmasinates IP-aadressiga. Selleks saate kasutada järgmist valimi skripti: 

        Param(
        [string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord



## <a name="run-a-planned-failover-primary-to-secondary"></a>Käivitage kavandatud Tõrkesiirde (esmane, teisene abil)

 See toiming kirjeldab käivitamise kavandatud Tõrkesiirde taastamis jaoks. Teise võimalusena võite käivitada Tõrkesiirde ühe virtuaalse masina jaoks **Virtuaalmasinates** menüü.

1. Enne alustamist veenduge, et soovite kasutada virtuaalmasinates lõpetanud algse dispersioonanalüüs.
2. Valige **Taastamise lepingud** > *recoveryplan_name*. Klõpsake **Tõrkesiirde** > **kavandatud Tõrkesiirde**. 
3. Valige lehel **Kinnitada plaanitud Tõrkesiirde **lähte- ja asukohad. Pange tähele Tõrkesiirde suunas.

    - Kui eelmine failovers töötas ootuspäraselt ja kõik virtuaalse masina serverid asuvad lähte- või sihtloendil asukohta, Tõrkesiirde suunas üksikasjad on ainult teavet. 
    - Kui virtuaalmasinates on aktiivne nii lähte- ja sihtsaitide asukohad, kuvatakse nupp **Muuda suunas** . Selle nupu abil saate muuta ja määrata suunas, milles on tõrkesiire peaks toimuma.

5. Kui te ei suuda üle Azure ja andmete krüptimine on lubatud pilves, **Krüptovõtme** valige sert, mida kui märkisite andmete krüptimine VMM Server pakkuja installimise ajal. 
6. Kui kavandatud Tõrkesiirde hakkab esimese asjana virtuaalmasinates tagada pole andmekao sulgeda. **Klõpsake vahekaarti** saate Tõrkesiirde edenemist jälgida. Kui ilmneb tõrge Tõrkesiirde, (kas virtuaalse masina või skripti, mis sisaldab taastamise kava) kavandatud Tõrkesiirde taastamise kava lõpetab. Saate algatada selle Tõrkesiirde uuesti.
8. Pärast koopia virtuaalmasinates luuakse nad on Kinnita ootele. Klõpsake nuppu **Kinnita** on tõrkesiire kinnitamiseks. 
9. Pärast kopeerimine on täielik virtuaalmasinates alustamise teisene asukohas. 

## <a name="run-an-unplanned-failover"></a>Planeerimata Tõrkesiirde käivitamine

See toiming kirjeldab käivitamise planeerimata Tõrkesiirde taastamis jaoks. Teise võimalusena võite käivitada Tõrkesiirde ühe virtuaalse masina või füüsilise serveri **Virtuaalmasinates** menüü.

1. Valige **Taastamise lepingud** > *recoveryplan_name*. Klõpsake **Tõrkesiirde** > **planeerimata Tõrkesiirde**. 
3. Valige lehel **Kinnitage planeerimata Tõrkesiirde **lähte- ja asukohad. Pange tähele Tõrkesiirde suunas.

    - Kui eelmine failovers töötas ootuspäraselt ja kõik virtuaalse masina serverid asuvad lähte- või sihtloendil asukohta, Tõrkesiirde suunas üksikasjad on ainult teavet. 
    - Kui virtuaalmasinates on aktiivne nii lähte- ja sihtsaitide asukohad, kuvatakse nupp **Muuda suunas** . Selle nupu abil saate muuta ja määrata suunas, milles on tõrkesiire peaks toimuma.

4. Kui te ei suuda üle Azure ja andmete krüptimine on lubatud pilves, **Krüptovõtme** valige sert, mida kui märkisite andmete krüptimine VMM Server pakkuja installimise ajal. 
5. Valige **virtuaalmasinates sulgeda ja nende sünkroonimine uusimaid andmeid** määramiseks saidi taastamine peaks proovite sulgeda kaitstud virtuaalmasinates ja sünkroonida, et andmed uusim versioon on nurjunud üle. Kui valite selle suvandi või proovi ei õnnestu saab selle Tõrkesiirde uusima saadaval taastamine punktist virtuaalse masina jaoks.
6. **Klõpsake vahekaarti** saate Tõrkesiirde edenemist jälgida. Pange tähele, et isegi juhul, kui planeerimata Tõrkesiirde esineb viga, taastamise kava töötab seni, kuni see on lõpule viidud.
7. Pärast Tõrkesiirde, on selle virtuaalmasinates **Kinnita ootel** olekus. Klõpsake nuppu **Kinnita** on tõrkesiire kinnitamiseks.
8. Kui häälestate dispersioonanalüüs kasutada mitut taastamise punkte, Muuda taastamine punkti saate valida taastamine punkti, mis ei ole hilisem kasutamine (Vaikimisi kasutatakse Viimane). Kui te endale täiendavad eemaldatakse taastamise punkte.
9. Kui kopeerimine on lõpule jõudnud soovitud virtuaalmasinates käivitub ja töötab teisene asukohas. Kuid nad ei ole kaitstud või imitatsiooniga. Kui esmane saidi on saadaval taas sama aluseks infrastruktuuri, valige **Pööratud ise** tagant kopeerimise alustamiseks. See tagab, et kõik andmed kopeeritakse tagasi esmane saidi ja virtuaalse masina on valmis Tõrkesiirde uuesti. Tühistada dispersioonanalüüs pärast planeerimata Tõrkesiirde andmesidekasutusele mõne pea kohal andmete edastamine. Ülemineku kasutab sama meetod, mis on konfigureeritud algse dispersioonanalüüs sätete pilve.

## <a name="failback-from-secondary-to-primary"></a>Sekundaarne esmane migreerite

 Pärast Tõrkesiirde esmane, teisene asukohta, tiražeeritud virtuaalmasinates pole kaitstud saidi taastamine ja teisene asukoht on nüüd abistas esmane. Järgige neid toiminguid tagasi algse esmane saidile nurjumise. See toiming kirjeldab käivitamise kavandatud Tõrkesiirde taastamis jaoks. Teise võimalusena võite käivitada Tõrkesiirde ühe virtuaalse masina jaoks **Virtuaalmasinates** menüü.

1. Valige **Taastamise lepingud** > *recoveryplan_name*. Klõpsake **Tõrkesiirde** > **kavandatud Tõrkesiirde**.
2. Valige lehel **Kinnitada plaanitud Tõrkesiirde **lähte- ja asukohad. Pange tähele Tõrkesiirde suunas. Kui Tõrkesiirde kaudu esmane töötanud nagu oodata ja kõik virtuaalmasinates on see kehtib ainult teabe teisene asukohta.
3. Kui te kasutate puudumisel tagasi: Azure'i valige **Andmete sünkroonimine**sätted:

    - **Saate sünkroonida enne Tõrkesiirde (ainult sünkroonib delta muudatuste)**– see suvand minimeeritakse tööseisakute virtuaalmasinates jaoks, nagu see sünkroonib ilma neid sulgemise. Teeb järgmist.
        - Etapp 1: Võtab hetktõmmise virtuaalse masina Azure ja kopeerib kohapealse Hyper-V Host. Seade ei lahene, Azure töötab.
        - Etapp 2: Sulgub Azure virtuaalse masina nii, et pole uue muudatused toimuvad seal. Muudatuste komplekti kantakse kohapealses serveris ja kohapealse virtuaalse masina käivitamist.
    

    - **Sünkroonimine andmed ainult Tõrkesiirde ajal (täielik alla)**– kasutage seda suvandit, kui te olete töötama Azure pikka aega. See suvand on kiirem, kuna me oodata, et enamik ketas on muutunud ja me ei taha kontrollsumma arvutuses aega. Tehtav ketta alla laadimata. See on kasulik, kui kohapeal virtuaalse masina on kustutatud.
    
    > [AZURE.NOTE] Soovitame kasutada seda suvandit siis, kui teil on kasutusel Azure'i aega (kuu või rohkem) või kohapeal VM on kustutatud. See suvand pole kontrollsumma arvutusi teha.
    
5. Kui te ei suuda üle Azure ja andmete krüptimine on lubatud pilves, **Krüptovõtme** valige sert, mida kui märkisite andmete krüptimine VMM Server pakkuja installimise ajal. 
5. Vaikimisi kasutatakse taastamine Viimane punkt, kuid **Muutmine taastamine punkti** saate määrata erinevate taastamise punkti. 
6. Klõpsake soovitud failback alustamiseks märke.  **Klõpsake vahekaarti** saate Tõrkesiirde edenemist jälgida. 
7. f valitud suvand sünkroonima andmeid Tõrkesiirde, enne kui algse andmete sünkroonimine on lõpule viidud ja olete valmis sulgeda virtuaalmasinates Azure, klõpsake **töö**  >  <planned failover job name> **Täieliku Tõrkesiirde**. See lõpetatakse Azure seadme üle virtuaalse masina kohapealse viimaste muudatuste kohta ja selle.
8. Nüüd saate logivad virtuaalse masina kinnitada, et see on saadaval ootuspäraselt. 
9. Virtuaalse masina on soovitud Kinnita ootele. Klõpsake nuppu **Kinnita** on tõrkesiire kinnitamiseks.
10. Nüüd klõpsake soovitud failback lõpuleviimiseks alustamiseks kaitsmine virtuaalse masina esmane saidi **Tühistada korrata** .



## <a name="failback-to-an-alternate-location"></a>Failback alternatiivse asukoha määramine

Kui juurutamist kaitse erinevad [Hyper-V sait ja Azure](site-recovery-hyper-v-site-to-azure.md) teil on võimalus, et Azure failback asendaja kohapealse asukoht. See on kasulik, kui teil on vaja luua uue kohapealse riistvara. Siin on, kuidas seda teha.

1. Kui häälestate uued riistvara installimine serveris Windows Server 2012 R2 ja Hyper-V roll.
2. Saate luua virtuaalse võrgu vahetamine sama nime, mida teil oli algse server.
3. Valige **Üksuste kaitstud** -> **Kaitse rühma**  ->  <ProtectionGroupName>  ->  <VirtualMachineName> soovite nurjuda uuesti ja valige **Plaanitud Tõrkesiirde**.
4. Valige **Plaanitud Tõrkesiirde kinnitada** **loomine kohapealse virtuaalse masina, kui see pole**. 
5. Valige uus Hyper-V hosti server kohta, kuhu soovite paigutada virtuaalse masina **Hosti nimi** .
6. Valige suvand **sünkroonimine andmed enne selle Tõrkesiirde**andmete sünkroonimine, soovitame teil. See vähendab tööseisakute virtuaalmasinates jaoks, nagu see sünkroonib ilma neid sulgemise. Teeb järgmist.

    - Etapp 1: Võtab hetktõmmise virtuaalse masina Azure ja kopeerib kohapealse Hyper-V Host. Seade ei lahene, Azure töötab.
    - Etapp 2: Sulgub Azure virtuaalse masina nii, et pole uue muudatused toimuvad seal. Muudatuste komplekti kantakse kohapealses serveris ja kohapealse virtuaalse masina käivitamist.
    
7. Klõpsake alustamiseks Tõrkesiirde (failback) märke.
8. Pärast algse sünkroonimise lõppemist ja olete valmis Azure virtuaalne arvuti sulgeda, klõpsake nuppu **töö** > <planned failover job> > **Täieliku Tõrkesiirde**. See Azure seadme sulgub, viib viimaste muudatuste kohta kohapealse virtuaalse masina ja selle.
9. Saab sisse logida kohapealse virtuaalse masina kontrollimaks, kas kõik toimib ootuspäraselt. Klõpsake nuppu **Kinnita** on tõrkesiire lõpuleviimiseks.
10. Klõpsake alustamiseks kaitsmine kohapealse virtuaalse masina **Tühistada korrata** .

    >[AZURE.NOTE] Kui tühistate failback töö juhises andmete sünkroonimise ajal, on kohapealse VM currupted olekus. See on andmete sünkroonimise jätkamine: eksemplaride Azure VM tabeliandmete kettad, et kohapeal andmete ketast ja kuni sünkroonimine on lõpule jõudnud, andmed ei pruugi olla ühtsete olekus ketas. Kui sees prem VM on käivitatud pärast andmete sünkroonimise jätkamine: tühistamisest, võib see ei käivitu. Uuesti käivitada, Tõrkesiirde andmete sünkroonimise jätkamine: lõpuleviimiseks.
 
