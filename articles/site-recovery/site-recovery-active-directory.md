<properties
    pageTitle="Kaitse Active Directory ja DNS-i Azure'i saidi taastamine | Microsoft Azure'i"
    description="Selles artiklis kirjeldatakse, kuidas rakendada katastroofi taastamise lahendus Active Directory abil Azure saidi taastamise jaoks."
    services="site-recovery"
    documentationCenter=""
    authors="prateek9us"
    manager="abhiag"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="08/31/2016"
    ms.author="pratshar"/>

# <a name="protect-active-directory-and-dns-with-azure-site-recovery"></a>Kaitse Active Directory ja DNS-i Azure'i saidi taastamine

Ettevõtte rakendusi nagu Dynamics AXI SharePointi ja SAP-i sõltuvad Active Directory ja DNS-i taristu töötaks õigesti. Katastroofi taastamise lahendus rakenduste loomisel on oluline meeles pidada, et peate kaitsta ja muude rakenduste komponendid, selleks, et tagada, et asju õigesti katastroofi ilmnemisel enne Active Directory ja DNS-i taastada.

Saidi taastamine on Azure'i teenus, mis pakub avariitaastet orkesteroinnin dispersioonanalüüs, Tõrkesiirde ja virtuaalmasinates taastamine. Saidi taastamine toetab arvu dispersioonanalüüs stsenaariumid süsteemne kaitsmiseks ja sujuvalt Tõrkesiirde virtuaalmasinates ja privaatne, avalik või hoster pilved rakendusi.

Saidi taastamise abil saate luua automatiseeritud katastroof taastamise kava Active Directory jaoks. Häireid ilmnemisel saate algatada Tõrkesiirde sekunditega igal pool ja tööks Active Directory paar minutit. Kui olete juurutatud Active Directory mitme rakendusi nagu SharePointi ja SAP-i esmane saidi ja soovite ei õnnestu üle täielik saidi, võib nurjuda üle Active Directory abil esmalt saidi taastamine ja seejärel ei õnnestu üle rakendusele vastav taastamise abil teisi rakendusi.

Selles artiklis selgitatakse, kuidas luua katastroofi taastamise lahendus Active Directory, kuidas teha plaanitud, ootamatute ja testi failovers ühe klõpsuga taastamis toetatud konfiguratsioone ja eeltingimused abil.  Saate tuleks Active Directory ja Azure saidi taastamise tuttav enne alustamist.

On kaks soovitatav võimalusi keskkonna keerukusest.

### <a name="option-1"></a>Suvand 1

Kui teil on väheste rakenduste ja ühe domeenikontrolleri ja soovite ei õnnestu üle terve saidi, siis soovitame kasutada saidi taastamine (kas te ei suuda üle Azure'i või saidi) saidi domeenikontrolleri korrata. Tiražeeritud virtual samasse arvutisse saab kasutada test Tõrkesiirde.

### <a name="option-2"></a>Variant 2

Kui teil on suur hulk rakendusi ja keskkonnas on rohkem kui üks domeenikontrolleri või kui kavatsete kasutada paari rakenduste korraga, soovitame Lisaks imitatsiooniga domeeni domeenikontrolleri virtuaalse masina saidi taastamine saate häälestada ka mõne täiendavad domeenikontrolleri sihtsaidi (Azure'i või sekundaarne asutusesisese andmekeskuse).

>[AZURE.NOTE] Isegi juhul, kui olete rakendamisel variant 2, tehes test Tõrkesiirde endiselt peate ise selle domeenikontrolleri saidi taastamise abil. Lugege [testida Tõrkesiirde kaalutlused](#considerations-for-test-failover) lisateavet.


Järgmistes jaotistes kirjeldatakse kaitse domeenikontrolleri saidi taastamise lubamine ja kuidas luua Azure domeenikontrolleri.


## <a name="prerequisites"></a>Eeltingimused

- Kohapealse juurutuse Active Directory ja DNS-i serveri.
- Azure'i saidi taastamise teenused vault Microsoft Azure'i tellimus.
- Kui olete imitatsiooniga Azure'i klõpskäivitusversiooni Azure virtuaalse masina valmisoleku hindamise tööriista VMs, et tagada, et need on Azure VMs ja Azure'i saidi taastamise teenused.


## <a name="enable-protection-using-site-recovery"></a>Kaitse abil saidi taastamise lubamine


### <a name="protect-the-virtual-machine"></a>Virtuaalne arvuti kaitsmine

Kaitse domeeni domeenikontrolleri ja DNS-i virtuaalse masina saidi taastamise lubamine. Virtuaalne seadme tüübist (Hyper-V või VMware) saidi taastamine sätete konfigureerimine. Soovitame krahh ühtsete dispersioonanalüüs sagedus 15 minutit.

###<a name="configure-virtual-machine-network-settings"></a>Virtuaalse masina võrgu sätete konfigureerimine

Domeeni domeenikontrolleri ja DNS-i virtuaalse masina, et VM manustatakse pärast Tõrkesiirde õige võrgu konfigureerimine saidi taastamine võrgu sätteid. Näiteks, kui te olete imitatsiooniga Hyper-V VMs Azure'i saate valida VM VMM pilveteenuses või konfigureerida võrgu sätteid nagu allpool näidatud jaotises kaitse

![VM võrgu sätteid](./media/site-recovery-active-directory/VM-Network-Settings.png)

## <a name="protect-active-directory-with-active-directory-replication"></a>Active Directory kopeerimise abil Active Directory kaitsmine

### <a name="site-to-site-protection"></a>Saitide kaitse

Domeenikontrolleri saidi loomine ja määrake sama domeen, mida kasutatakse esmane saidil, kui te edendada domeenikontrolleri roll serveri nimi. Saate **Active Directory saidid ja teenused** lisandmooduli sätete konfigureerimiseks saidi lingi objekt, millele on lisatud saitide. Lingi saidi sätete konfigureerimisega saate määrata, kui kopeerimine toimub kahe või enama saitidel, ja kui sageli. Üksikasjalikumat teavet teemast [Plaanimine Dispersioonanalüüs vahel saidid](https://technet.microsoft.com/library/cc731862.aspx) .

###<a name="site-to-azure-protection"></a>Saidi Azure kaitse

Järgige [domeenikontrolleri Azure virtuaalse võrgu](../active-directory/active-directory-install-replica-active-directory-domain-controller.md)loomiseks. Kui teil reklaamida serveri domeenikontrolleri roll määrata sama domeeni nime, mida kasutatakse esmane saidil.

Seejärel [konfigureerida virtuaalse võrgu jaoks DNS-i server](../active-directory/active-directory-install-replica-active-directory-domain-controller.md#reconfigure-dns-server-for-the-virtual-network), kasutada DNS-i server Azure'i.

![Azure'i võrgu](./media/site-recovery-active-directory/azure-network.png)

## <a name="test-failover-considerations"></a>Test Tõrkesiirde kaalutlused

Test Tõrkesiirde võrgus, mis on eraldatud katsevõrgust nii, et on mingit mõju tootmise töökoormus.

Enamik rakenduste on vaja ka domeenikontrolleri ja DNS-i server, funktsioon nii, et enne rakenduse nurjus üle, domeenikontrolleri tuleb luua kasutatakse test Tõrkesiirde isoleeritakse võrgus. Lihtsaim viis selleks on lubamiseks domeeni domeenikontrolleri ja DNS-i virtual arvutisse saidi taastamine ja käivitage test Tõrkesiirde selle virtuaalse masina, enne test Tõrkesiirde taastamise kava rakenduse käivitamist. Siin on, kuidas seda teha.

1. Kaitse domeeni domeenikontrolleri ja DNS-i virtuaalse masina saidi taastamise lubamine.
2. Luua isoleeritakse. Mis tahes vaikimisi luuakse Azure virtuaalse võrk on eraldatud muude võrkude väliskontaktidega. Soovitame, et selle võrgu IP-aadresside vahemiku on sama, mis tootmise võrgu. Ei luba selle võrgu-saidilt Ühenduvus.
3. Sisestage DNS-i IP-aadress, võrku, mis on loodud, kui eeldate, et saada DNS-i virtuaalse masina IP-aadress. Kui te olete imitatsiooniga Azure'i, seejärel sisestage IP-aadress VM, mida kasutatakse Tõrkesiirde **Target IP** säte VM atribuudid. Kui olete imitatsiooniga teise kohapealse saidi ja kasutate DHCP Jälgi juhiseid [DNS-i ja DHCP test Tõrkesiirde](site-recovery-failover.md#prepare-dhcp) häälestamine

>[AZURE.NOTE] Eraldatud virtuaalse masina test Tõrkesiirde ajal IP-aadress on sama, mis IP-aadress see saaksin kavandatud või planeerimata Tõrkesiirde ajal, kui IP-aadress on test Tõrkesiirde võrgus saadaval. Kui see pole, saab virtuaalse masina on test Tõrkesiirde võrgus saadaval eri IP-aadress.

4. Domeeni domeenikontrolleri virtual arvutisse käivitage test Tõrkesiirde see isoleeritakse võrku. Kasutage uusima saadaval rakenduse ühtsete taastamine punkti domeeni domeenikontrolleri virtuaalse masina test Tõrkesiirde. 
5. Käivitage test Tõrkesiirde rakenduse taastamise lepingu raames.
6. Kui testimine on lõpule jõudnud, märkida testi Tõrkesiirde töö domeeni domeenikontrolleri virtuaalse masina ja taastamise kava "Lõpuleviimine" **klõpsake vahekaarti saidi taastamine portaalis** .

### <a name="dns-and-domain-controller-on-different-machines"></a>DNS-i ja domeeni domeenikontrolleri eri arvutites

Kui peate luua DNS-i VM test Tõrkesiirde domeenikontrolleri sama virtual arvutis pole DNS-i. Kui need on sama VM, võite selle jaotise vahele jätta.

Saate kasutada värske DNS-i server ja looge kõik nõutavad tsoonid. Näiteks kui teie Active Directory domeeni contoso.com, saate luua DNS-i tsooni koos nimi contoso.com. Active Directory vastavad kirjed tuleb värskendada DNS-i, järgmiselt:

1. Tagada neid sätteid enne ilmub virtuaalse masina taastamise kavandamine:

    - Mets juurkausta nimi peab olema nime tsooni.
    - Tsooni peab olema faili varundada.
    - Tsooni peab olema lubatud turvaline ja ebaturvalise värskendusi.
    - Domeeni domeenikontrolleri virtuaalse masina lahendaja peaks osutage DNS-i virtuaalse masina IP-aadress.

2. Domeeni domeenikontrolleri virtual arvutisse käivitage järgmine käsk:

    `nltest /dsregdns`

3. Tsooni lisada DNS-i server, luba värskendused ebaturvalise ja seda DNS-i kirje lisamine.

        dnscmd /zoneadd contoso.com  /Primary
        dnscmd /recordadd contoso.com  contoso.com. SOA %computername%.contoso.com. hostmaster. 1 15 10 1 1
        dnscmd /recordadd contoso.com %computername%  A <IP_OF_DNS_VM>
        dnscmd /config contoso.com /allowupdate 1


## <a name="next-steps"></a>Järgmised sammud

Lugege [mis töökoormus saate kaitsta?](../site-recovery/site-recovery-workload.md) enterprise töökoormus Azure saidi taastamine kaitsmise kohta lisateabe saamiseks.
