<properties
    pageTitle="Üksikasjalik SSH tõrkeotsing on Azure VM | Microsoft Azure'i"
    description="Üksikasjalikumad SSH juhised on Azure virtuaalse masina probleemide tõrkeotsing"
    keywords="SSH ühendus keeldus, ssh viga, Azure'i ssh, SSH ühendus katkes"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="top-support-issue,azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="support-article"
    ms.date="09/01/2016"
    ms.author="iainfou"/>

# <a name="detailed-ssh-troubleshooting-steps"></a>SSH tõrkeotsingujuhiseid

On palju võimalikku põhjust, mis ei pruugi SSH kliendi SSH teenuse VM saavutamiseks. Kui järgisite rohkem [Üldine SSH tõrkeotsingujuhiste juurde](virtual-machines-linux-troubleshoot-ssh-connection.md), peate ühenduse probleemi tõrkeotsingu sooritamiseks. Selles artiklis juhendab teid määratlemiseks, kus SSH ühendus ei suuda ja kuidas selle probleemi lahendamiseks tõrkeotsingujuhiseid.

## <a name="take-preliminary-steps"></a>Tehke toimingud esialgne

Järgmisel joonisel on esitatud komponendid, mis on seotud.

![Skeem, mis kuvatakse SSH teenuse komponendid](./media/virtual-machines-linux-detailed-troubleshoot-ssh-connection/ssh-tshoot1.png)

Järgmiste juhiste abil saate eristada allikas, kuvatakse tõrge ja aru saada probleemidele lahendusi.

Esmalt oleku VM portaalis.

[Azure'i portaalis](https://portal.azure.com):

1. Klassikaline juurutamise mudeli abil loodud vms, valige **Sirvi** > **virtuaalmasinates (klassikaline)** > *VM nime*.

    -VÕI-

    Ressursihaldur mudeli abil loodud vms, valige **Sirvi** > **virtuaalmasinates** > *VM nime*.

    **Käivitatud**peaks VM paani oleku kuvamine. Liikuge kerides jaotiseni Kuva tehtud tegevuse Arvuta, salvestusruumi ja võrgu ressursse.

2. Valige **seaded** uurida lõpp-punktid, IP-aadressid ja muud sätted.

    Lõpp-punktid VMs, mis on loodud, kasutades ressursihaldur tuvastamiseks Kontrollige [võrgu turberühma](../virtual-network/virtual-networks-nsg.md) määratleda. Samuti kontrollida reegleid on rakendatud võrgu turberühma ja olete viidatud alamvõrgu.

[Azure'i klassikaline portaali](https://manage.windowsazure.com)vms klassikaline juurutamise mudeli abil loodud:

1. Valige **virtuaalmasinates** > *VM nime*.
2. Valige soovitud VM **armatuurlaua** selle oleku kontrollimine.
3. Valige **kuvari** näha tehtud tegevuse Arvuta, salvestusruumi ja võrgu ressursse.
4. Valige **lõpp-punktid** , et tagada, et SSH liikluse lõpp.

Võrguühenduse kontrollimiseks kontrollige konfigureeritud lõpp-punktid ja kui jõuate VM teise protokolli, nt HTTP- või muu teenuse kaudu.

Pärast neid juhiseid, proovige SSH ühenduse uuesti.


## <a name="find-the-source-of-the-issue"></a>Probleemi allika otsimine

SSH kliendi arvutisse võib ei jõua SSH teenuse Azure VM probleemid või misconfigurations järgmist:

- [SSH klientarvuti](#source-1-ssh-client-computer)
- [Ettevõtte serva seade](#source-2-organization-edge-device)
- [Pilvepõhise teenuse lõpp-punkti ja juurdepääsu juhtimine loend (ACL)](#source-3-cloud-service-endpoint-and-acl)
- [Võrgu turberühmad](#source-4-network-security-groups)
- [Azure'i VM Linux-põhine](#source-5-linux-based-azure-virtual-machine)

## <a name="source-1-ssh-client-computer"></a>Allikas 1: SSH klientarvuti

Allikana tõrke kõrvaldamiseks arvuti, veenduge, et seda saab teha SSH ühenduste teisele asutusesiseselt, Linuxi põhises arvutis.

![Skeem, kus tõstetakse esile SSH kliendi arvuti komponendid](./media/virtual-machines-linux-detailed-troubleshoot-ssh-connection/ssh-tshoot2.png)

Kui nurjub, kontrollige oma arvutis järgmist.

- Sätte kohaliku tulemüür blokeerib sissetulevaid või väljaminevaid SSH liikluse (TCP 22)
- Kohalik installitud puhverserveri klienttarkvara, mis takistab SSH ühendused
- Kohalik installitud võrgu jälgimise tarkvara, mis takistab SSH ühendused
- Muud tüüpi turbetarkvara, mis liikluse jälgimine või lubada/keelata kindlat tüüpi liikluse

Kui tingimustel rakendada, ajutiselt keelata tarkvara ja proovige SSH ühendus arvutiga kohapealse välja selgitada põhjus ühendus on blokeeritud teie arvutis. Seejärel saate töötada õige tarkvara sätted lubama ühendusi SSH võrguadministraatori poole.

Kui kasutate serdi autentimine, veenduge, et on need õigused .ssh kausta Avaleht kataloogis:

- Chmod 700 ~/.ssh
- Chmod 644 ~/.ssh/\*Pub
- Chmod 600 ~/.ssh/id_rsa (või muid faile, mis on talletatud neid oma privaatvõtmete)
- Chmod 644 ~/.ssh/known_hosts (sisaldab hosts, millega olete ühenduses SSH)

## <a name="source-2-organization-edge-device"></a>Allikas 2: Ettevõtte serva seade

Allikana tõrke kõrvaldamiseks seadme ettevõtte serv, veenduge, et arvuti, mis on ühendatud otse Interneti saate teha oma Azure VM SSH ühendused. Kui avate VM üle VPN saidilt või Azure'i ExpressRoute-ühenduse, minge [Allika 4: Network turberühmad](#nsg).

![Skeem, kus tõstetakse esile ettevõtte serva seade](./media/virtual-machines-linux-detailed-troubleshoot-ssh-connection/ssh-tshoot3.png)

Kui teil pole arvutis, kus on ühendatud otse Interneti-ühendus, luua uue Azure VM oma ressursirühm või teenus cloud ja seda kasutada. Lisateavet leiate teemast [loomine virtuaalse masina töötab Linux Azure](virtual-machines-linux-quick-create-cli.md). Ressursirühm või VM ja pilvepõhise teenuse kustutada, kui olete lõpetanud oma testimine.

Kui loote SSH ühendus Internetiga ühendatud otse arvutiga, kontrollige oma ettevõtte serva seadme jaoks.

- Sisemise tulemüüri, blokeerib SSH liiklus Interneti-ühendus
- Puhverserverit, mis takistab SSH ühendused
- Sissetungi tuvastamise või võrgu jälgimise tarkvara töötab teie serv võrku, mis takistab SSH ühendused seadmetes

Töötamine võrguadministraator valesti sätteid oma asutuse serva seadmetes Internetiga SSH liikluse lubamiseks.

## <a name="source-3-cloud-service-endpoint-and-acl"></a>Allikas 3: Pilveteenuste teenuse lõpp-punkti ja ACL

> [AZURE.NOTE] Selle andmeallika kehtib ainult klassikaline juurutamise mudeli abil loodud VMs. Ressursihaldur abil loodud vms, jätkake [andmeallika 4: Network turberühmad](#nsg).

Kõrvaldamiseks pilvepõhise teenuse lõpp-punkti ja ACL allikana tõrke, veenduge, et teise Azure VM virtuaalse samasse võrku saate teha oma VM SSH ühendused.

![Skeem, mis toob esile pilvepõhise teenuse lõpp-punkti ja ACL](./media/virtual-machines-linux-detailed-troubleshoot-ssh-connection/ssh-tshoot4.png)

Kui teil pole teise VM virtuaalse samasse võrku, saate hõlpsalt luua uus. Lisateavet leiate teemast [loomine Linux VM Azure CLI abil](virtual-machines-linux-quick-create-cli.md). Kui olete valmis oma testimine eest VM kustutamine

Kui loote SSH ühendus koos VM virtuaalse samasse võrku, kontrollige järgmist.

- **Lõpp-punkti konfiguratsiooni SSH liikluse sihtkohas VM.** Privaatne TCP pordi lõpp-punkti peaksid olema samad TCP pordi aluseks on listening SSH teenuse VM. (Vaikeport on 22). Ressursihaldur juurutamise mudeli abil loodud VMs kinnitamine SSH TCP pordi number Azure'i portaalis, valides **sirvida** > **virtuaalmasinates (v2)** > *VM nime* > **sätted** > **lõpp-punktid**.

- **ACL SSH liikluse lõpp-punkti virtual sihtarvutis.** Mõne ACL võimaldab teil määrata lubatud või keelatud liiklust Internet, IP-aadress selle andmeallika põhjal. Valesti ACL-ID, saate vältida SSH liiklust lõpp-punkti. Märkige oma ACL tagada selle Sissetuleva liikluse oma puhverserveri avaliku IP-aadresside või muu serva server on lubatud. Lisateavet leiate teemast [loendite juhtelemendi võrgupääs (ACL)](../virtual-network/virtual-networks-acl.md).

Lõpp-punkti kõrvaldamiseks allikana probleemi eemaldamine praeguse lõpp-punkti, uue lõpp-punkti loomine ja määrake SSH nimi (TCP port 22 avalike ja privaatvõ pordi number). Lisateabe saamiseks lugege teemat [häälestamine virtual arvutisse Azure lõpp-punktid](virtual-machines-windows-classic-setup-endpoints.md).

<a id="nsg"></a>
## <a name="source-4-network-security-groups"></a>Allikas 4: Võrgu turberühmad

Võrgu turberühmad võimaldavad teil on lubatud sissetuleva ja väljamineva liikluse täpsema juhtimise. Saate luua reeglid, mis hõlmavad alamvõrku ja pilveteenused Azure virtuaalse võrgus. Kontrollige oma võrgu turvalisuse rühma reeglid tagamaks, et SSH liiklus ja Interneti kaudu on lubatud.
Lisateabe saamiseks lugege [võrgu turberühmad](../virtual-network/virtual-networks-nsg.md).

## <a name="source-5-linux-based-azure-virtual-machine"></a>Allikas 5: Linuxi-põhise Azure arvutiga

Võimalikud probleemid viimase allikas on Azure virtuaalse masina ise.

![Skeem, kus tõstetakse esile Linuxi-põhise Azure arvutiga](./media/virtual-machines-linux-detailed-troubleshoot-ssh-connection/ssh-tshoot5.png)

Kui te pole seda veel teinud, järgige selle juhiseid [SSH Linux-põhine virtuaalmasinates või parool lähtestada](virtual-machines-linux-classic-reset-access.md).

Proovige oma arvutist uuesti ühendust luua. Kui see nurjub, on mõned võimalikud probleemid:

- Virtuaalne sihtarvutis SSH teenus ei tööta.
- TCP pordi 22 on SSH teenus pole kuulamise. Selle testimiseks kohalikus arvutis Telneti kliendi installimine ja käivitamine "Telneti *cloudServiceName*. cloudapp.net 22". See määratleb, kui virtuaalse masina võimaldab sissetulevate ja väljaminevate side SSH lõpp-punkti.
- Kohaliku tulemüüri virtual sihtarvutis on reeglid, mis ei lase sissetulevaid või väljaminevaid SSH liikluse.
- Sissetungi tuvastamise või võrgu jälgimise tarkvara, mis töötab Azure virtuaalse masina takistab SSH ühendused.


## <a name="additional-resources"></a>Lisaressursid
Rakenduse access tõrkeotsingu kohta leiate lisateavet teemast [tõrkeotsing juurdepääsu rakenduse Azure virtuaalne arvutis töötab](virtual-machines-linux-troubleshoot-app-connection.md)