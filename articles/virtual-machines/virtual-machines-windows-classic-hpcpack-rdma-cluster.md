<properties
 pageTitle="MPI rakenduste käivitamiseks Windows RDMA kobar häälestamine | Microsoft Azure'i"
 description="Siit saate teada, kuidas luua suurus H16r, H16mr, A8 ja A9 VMs kasutada Azure RDMA MPI rakenduste käivitamiseks Windows HPC Pack kobar."
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="big-compute"
 ms.date="09/20/2016"
 ms.author="danlep"/>

# <a name="set-up-a-windows-rdma-cluster-with-hpc-pack-to-run-mpi-applications"></a>MPI rakenduste käivitamiseks Windows RDMA kobar HPC Pack häälestamine

Häälestada Windows RDMA kobar Azure [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) ja [H-sarja või Arvuta mahukat A-sarja eksemplari](virtual-machines-windows-a8-a9-a10-a11-specs.md) paralleelselt sõnumi läbides kasutajaliidese (MPI) rakendusi käivitada. Kui seadistate ka HPC Pack kobar sõlmed RDMA-ühenduse võimalusega, Windows Server, MPI rakenduste suhelda tõhus madal latentsus, kõrge läbilaskevõime võrgu Azure, mis põhineb serveri otsest mälu juurdepääs (RDMA) tehnoloogia üle.

Kui soovite käivitada MPI töökoormus Linux VMs Azure'i RDMA võrku, lugege teemat [Linux RDMA kobar käivitamiseks MPI rakenduste häälestamine](virtual-machines-linux-classic-rdma-cluster.md).


## <a name="hpc-pack-cluster-deployment-options"></a>HPC Pack kobar Juurutussuvandid
Microsoft HPC Pack on tööriist luua täiendavaid tasuta HPC kogumite kohapealse või Azure käivitada Windowsi või Linuxi HPC rakendusi. HPC pakett sisaldab käitusaja keskkonnas Microsoft rakendamiseks sõnumi läbides kasutajaliidese Windowsi (MS-MPI). Kasutamisel koos RDMA-ühenduse võimalusega eksemplarid, mis on toetatud Windows Server operatsioonisüsteemiga HPC Pack pakub tõhusa võimalust käivitada Windows MPI rakendusi, et juurdepääs Azure'i RDMA võrku. 

Selles artiklis tutvustatakse kahe stsenaariumid ja lingid häälestada Windowsi RDMA kobar Microsoft HPC Pack üksikasjalikke juhiseid. 

* Stsenaarium 1. Arvuta mahukat töötaja rolli aknad (PaaS) juurutamine

* Stsenaarium 2. Arvuta sõlmed Arvuta mahukat vms (IaaS) juurutamine

[H-sari ja Arvuta mahukat A-sarja VMs kohta](virtual-machines-windows-a8-a9-a10-a11-specs.md) leiate jaotisest üldine eeltingimused Arvuta mahukat eksemplari kasutada Windows.



## <a name="scenario-1-deploy-compute-intensive-worker-role-instances-paas"></a>Stsenaarium 1. Arvuta mahukat töötaja rolli aknad (PaaS) juurutamine

Olemasoleva HPC Pack kobar, lisada täiendavat Arvuta ressursse Azure töötaja rolli (Azure sõlmed) töötab pilveteenuses (PaaS). See funktsioon, mida nimetatakse ka "lõhkemise Azure" HPC Pack, toetab suurused töötaja rolli aknad. Kui lisate Azure sõlmed, lihtsalt määrata ühe RDMA-ühenduse võimalusega suuruses.

Järgnevalt on tingimusi, et RDMA-ühenduse võimalusega lõhkemisega Azure eksemplarid olemasoleva (tavaliselt kohapealse) kobar. Sarnaselt toimingute abil saate lisada HPC Pack pea sõlme, mis Azure VM juurutatud töötaja rolli aknad.

>[AZURE.NOTE] Juhendi lõhkemisega Azure HPC Pack, leiate [hübriid kobar HPC Pack häälestamine](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md). Pange tähele kaalutlused toimingut, mis kehtivad konkreetselt RDMA-ühenduse võimalusega Azure sõlmed.

![Azure'i lõhkeda][burst]

### <a name="steps"></a>Juhised

4. **Juurutamiseks ja konfigureerimiseks on HPC Pack 2012 R2 pea sõlme**

    [Microsofti allalaadimiskeskusest](https://www.microsoft.com/download/details.aspx?id=49922)alla laadida uusima installipaketi HPC Pack. Nõuded ja Azure lõhkemist juurutamise ettevalmistamiseks juhiseid teemast [HPC Pack alustusjuhend](https://technet.microsoft.com/library/jj884144.aspx) ja [purune Azure töötaja eksemplarides Microsoft HPC Pack](https://technet.microsoft.com/library/gg481749.aspx).

5. **Azure tellimuse halduse serdi konfigureerimine**

    Serdi turvamiseks pea sõlme ja Azure vahelise ühenduse konfigureerimine. Suvandid ja toimingute kohta leiate teemast [Azure halduse serdi HPC Pack konfigureerimiseks stsenaariumid](http://technet.microsoft.com/library/gg481759.aspx). Testi juurutuste HPC Pack installib vaikimisi Microsoft HPC Azure'i Management sert saate kiiresti üles laadida Azure tellimuse.

6. **Uue pilveteenuses ja salvestusruumi konto loomine**

    Azure'i klassikaline portaali abil saate luua pilveteenus ja salvestusruumi konto kasutuselevõtuks piirkonnas, kus RDMA-ühenduse võimalusega eksemplarid on saadaval.

7. **Azure sõlme malli loomine**

    Kasutage funktsiooni loomine viisardi sõlm malli HPC halduris. Juhised leiate teemast [Azure sõlme malli loomine](http://technet.microsoft.com/library/gg481758.aspx#BKMK_Templ) "Juhiseid soovite juurutada Azure sõlmed koos Microsoft HPC Pack".

    Algne testide, soovitame konfigureerimine käsitsi kättesaadavus poliitika mall.

8. **Lisada sõlmed klaster**

    Kasutage soovitud lisada sõlm viisardi HPC halduris. Lisateabe saamiseks vt [Windows HPC kobar Azure sõlmed lisada](http://technet.microsoft.com/library/gg481758.aspx#BKMK_Add).

    Kui suurus sõlmed määramisele valida ühe RDMA-ühenduse võimalusega eksemplari suuruses.
    
    >[AZURE.NOTE]Iga plahvatuse, et Azure juurutus, nii et Arvuta mahukat eksemplari, HPC Pack automaatselt kasutab vähemalt 2 RDMA-ühenduse võimalusega eksemplarid (nt A8) nimega puhverserveri sõlmed, lisaks teie määratud Azure töötaja rolli aknad. Puhverserveri sõlmed kasutada südamikud, mis on eraldatud tellimus ja tasuline koos Azure töötaja rolli aknad.

9. **Käivitage (sätted) sõlmed ja tuua võrgus tööde**

    Valige sõlmed ja kasutage toimingut **Käivita** HPC halduris. Kui ettevalmistamise on lõpule jõudnud, valige sõlmed ja **Tuua Online** toiminguga HPC halduris. Sõlmed on töö käivitamiseks valmis.

10. **Esitage klaster tööde haldamine**

    HPC Pack töö esitamise tööriistade abil saate käivitada kobar tööd. Vt [Microsoft HPC Pack: töö halduse](http://technet.microsoft.com/library/jj899585.aspx).

11. **Peatamine (deprovision) sõlmed**

    Kui olete valmis töö töötab, sõlmed võrguühenduseta ja kasutage toimingut **lõpetada** HPC halduris.





## <a name="scenario-2-deploy-compute-nodes-in-compute-intensive-vms-iaas"></a>Stsenaarium 2. Arvuta sõlmed Arvuta mahukat vms (IaaS) juurutamine

Selle stsenaariumi korral pea sõlme HPC Pack juurutamist ja kobar arvutada sõlmed VMs liitunud virtuaalse võrgu Azure Active Directory domeeni kohta. HPC Pack pakub mitmesuguseid [Juurutussuvandid Azure VM](virtual-machines-linux-hpcpack-cluster-options.md), sealhulgas automatiseeritud juurutamise skripte ja Azure Kiirjuhend mallid. Näiteks all tingimusi juhatavad teid [HPC Pack IaaS juurutamise skripti](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) abil automatiseerida kõige selle protsessi.

![Azure'i VM kobar][iaas]



### <a name="steps"></a>Juhised

1. **Luua kobar pea sõlme ja arvutada sõlm VMs klientarvutis HPC Pack IaaS juurutamise skripti käivitades**

    [Microsofti allalaadimiskeskusest](https://www.microsoft.com/download/details.aspx?id=49922)HPC Pack IaaS juurutamise skripti alla laadida.

    Klientarvuti ettevalmistamine skriptifail konfiguratsiooni loomine ja käivitage skript leiate jaotisest [Mõne HPC kobar HPC Pack IaaS juurutamise skripti](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md). 
    
    Juurutamine RDMA-ühenduse võimalusega Arvuta sõlmed, arvestage järgmisega täiendavad.
    
    * **Virtuaalne võrgu** - määrata uue virtuaalse võrgu piirkonnas on saadaval RDMA-ühenduse võimalusega eksemplari suurus, mida soovite kasutada.

    * **Operatsioonisüsteemi Windows Server** - toetavad RDMA Ühenduvus, Windows Server 2012 R2 või Windows Server 2012 operatsioonisüsteemi MSDN VMs jaoks määrata.

    * **Pilveteenustega** - soovitame juurutamine oma pea sõlme ühe pilveteenuses ja teie Arvuta sõlmed mõnes muus pilveteenuses.

    * **Pea sõlme suurus** – sel juhul kaaluge suurus vähemalt A4 (eest suur) pea sõlme jaoks.

    * **HpcVmDrivers laiend** - juurutamise skripti installib Azure VM Agent ja HpcVmDrivers laiend automaatselt suurus: A8 või arvutada sõlmed operatsioonisüsteemiga Windows Server A9 juurutamisel. HpcVmDrivers installib draiverid MSDN VMs nii, et nad saavad ühenduse RDMA võrku.

    * **Võrgukonfiguratsioon kobar** - juurutamise skripti automaatselt häälestab HPC Pack kobar topoloogia 5 (kõik sõlmed ettevõtte võrgus). See topoloogia on vaja kõik HPC Pack kobar juurutuste VM. Kobar võrgutopoloogia hiljem muuta.

2. **Arvuta sõlmed võrgus tuua tööde**

    Valige sõlmed ja **Tuua Online** toiminguga HPC halduris. Sõlmed on töö käivitamiseks valmis.

3. **Esitage klaster tööde haldamine**

    Selleks on kohapealse arvuti häälestada või ühenduse pea sõlme esitada töid. Lisateavet leiate teemast [Mõne HPC kobar Azure töö esitada](virtual-machines-windows-hpcpack-cluster-submit-jobs.md).

4. **Võrguühenduseta sõlmed "ja" stop (deallocate) nende**

    Kui olete valmis töö töötab, võtta sõlmed ühenduseta HPC halduris. Seejärel kasutage Azure Haldusriistad neid sulgeda.



## <a name="run-mpi-applications-on-the-cluster"></a>Käivitage MPI rakenduste klaster

### <a name="example-run-mpipingpong-on-an-hpc-pack-cluster"></a>Näide: Mpipingpong käitamist on HPC Pack kobar

HPC Pack kasutuselevõtu RDMA-ühenduse võimalusega eksemplarid kinnitamiseks klaster HPC Pack **mpipingpong** käsu käivitamine. **mpipingpong** saadab paketid andmepunktipaaride sõlmed korduvalt latentsus ja läbilaskevõime mõõtmed arvutamiseks ja statistika RDMA lubatud rakenduse võrgu vahel. Selles näites on tüüpilised mustri töö on MPI (sel juhul **mpipingpong**) kobar **mpiexec** käsu abil.

Selles näites eeldab lisasite Azure sõlmed "lõhkemist Azure" konfiguratsiooni ([stsenaarium 1](#scenario-1.-deploy-compute-intensive-worker-role-instances-(PaaS) in this article). Kui olete juurutanud HPC Pack klaster Azure VMs, peate käsu süntaks määrata eri sõlm ja määrata täiendavaid keskkonna muutujate RDMA võrgu network liikluse suunamiseks muutmine.


Klaster mpipingpong käivitamiseks tehke järgmist.


1. Pea sõlme või õigesti konfigureeritud kliendi arvutisse, avage käsuviip.

2. Latentsus vahel paari Azure lõhkemist kasutuselevõtu 4 sõlmed sõlmed prognoosimiseks, tippige järgmine käsk esitada töö, mida soovite käivitada mpipingpong small paketi suurus ja suure hulga iteratsiooni:

    ```
    job submit /nodegroup:azurenodes /numnodes:4 mpiexec -c 1 -affinity mpipingpong -p 1:100000 -op -s nul
    ```

    Käsk tagastab töö, mis on esitatud ID-d.

    Kui olete juurutanud HPC Pack klaster juurutatud Azure VMs, määrake sõlm rühm, mis sisaldab arvutada VMs juurutatud ühe pilveteenuses sõlm ja muuta käsk **mpiexec** järgmiselt:

    ```
    job submit /nodegroup:vmcomputenodes /numnodes:4 mpiexec -c 1 -affinity -env MSMPI_DISABLE_SOCK 1 -env MSMPI_PRECONNECT all -env MPICH_NETMASK 172.16.0.0/255.255.0.0 mpipingpong -p 1:100000 -op -s nul
    ```

3. Töö lõpetamisel vaatamiseks väljund (sel juhul, väljund töö ülesanne 1), tippige järgmine

    ```
    task view <JobID>.1
    ```

    Kui &lt; *JobID* &gt; on võimalik töö ID-d.

    Väljund hõlmab latentsuse tulemuste sarnaneb järgmisega.

    ![Ping pong latentsus][pingpong1]

4. Prognoosimiseks läbilaskevõime vahel paari Azure lõhkemist sõlmed, tippige järgmine käsk esitada töö, mida soovite käivitada **mpipingpong** suure paketi suurus ja väheste iteratsiooni:

    ```
    job submit /nodegroup:azurenodes /numnodes:4 mpiexec -c 1 -affinity mpipingpong -p 4000000:1000 -op -s nul
    ```

    Käsk tagastab töö, mis on esitatud ID-d.

    Muuta HPC Pack kobar juurutatud Azure VMs, klõpsake käsku nagu toimingus 2.

5. Töö lõpetamisel vaatamiseks väljund (sel juhul, väljund töö ülesanne 1), tippige järgmine:

    ```
    task view <JobID>.1
    ```

  Väljund sisaldab läbilaskevõime tulemid sarnaneb järgmisega.

  ![Ping pong läbilaskevõime][pingpong2]


### <a name="mpi-application-considerations"></a>MPI rakenduse peaksite arvesse võtma


Järgmised kaalutluste kohta HPC Pack Azure MPI rakenduste töötab. Mõned kehtivad ainult juurutuste Azure sõlmed (töötaja rolli aknad lisatud konfiguratsiooni "lõhkemist Azure").

* Töötaja rolli aknad pilveteenus on perioodiliselt reprovisioned ilma ette teatamata, Azure (näiteks hooldus või nurjub, näiteks juhul). Kui näiteks on reprovisioned, kui see töötab ka MPI töö, näiteks oma andmed lähevad kaotsi ja tagastab riik, kui see on esimene juurutatud, mis võib põhjustada MPI töö nurjumise. Rohkem sõlmi, et töö abil saate ühe MPI töökohta ja enam töötab, seda suurem on tõenäosus, et ühte linnanimede kuvatakse reprovisioned ajal töö töötab. See peaksite kaaluma ka siis, kui teil määrata ühe sõlm juurutuse failiserverisse.


* Azure MPI tööde käitamine, pole teil kasutada RDMA-ühenduse võimalusega eksemplarid. Saate kasutada mis tahes suurusega eksemplari, mis toetab HPC Pack. Siiski soovitatakse suhteliselt suuremahuliste MPI tööde, mis on tundlik latentsus ja mis ühendab sõlmed võrgu läbilaskevõime RDMA-ühenduse võimalusega eksemplarid. Kui kasutate muud suurused latentsus ja läbilaskevõime tundlik MPI töö käivitamiseks, on soovitatav väike töö, kus ühe tööülesande töötab vaid mõned sõlmed.

* Azure'i eksemplaridesse juurutatud rakendused on integreeritud hulgilitsentsimise ja sellega seotud. Võtke ühendust müüjaga trükikojas taotlemise hulgilitsentsimise või muude piirangute töötab pilveteenuses. Kõik müüjad pakuvad pensionitingimustega litsentsimine.


* Azure'i eksemplarid vaja veel häälestamise Accessi kohapealse sõlmed, osakud ja litsentsi. Näiteks Azure sõlmed serveriks kohapealse litsentsi juurdepääsu lubamiseks saate konfigureerida Azure-saidilt virtuaalse võrgus.


* Azure'i eksemplarid MPI rakenduste käitamiseks registreeruma iga MPI rakenduse Windowsi tulemüür linnanimede **hpcfwutil** käsu. See võimaldab MPI side toimub portide, mis on määratud dünaamiliselt tulemüüri.

    >[AZURE.NOTE] Purune Azure juurutuste abil, saate ka konfigureerida tulemüüri erand käsk kõik uue Azure sõlmed, mis lisatakse klaster automaatselt käivituma. Pärast käsu **hpcfwutil** ja veenduge, et teie rakendus töötab, lisage see käsk käivitus skripti oma Azure sõlmed. Lisateabe saamiseks leiate teemast [Azure sõlmed käivitus skript](https://technet.microsoft.com/library/jj899632.aspx).



* HPC Pack kasutab CCP_MPI_NETMASK kobar keskkonna muutuja, et määrata erinevaid aktsepteeritav aadressid MPI side. Alates HPC Pack 2012 R2, CCP_MPI_NETMASK kobar keskkonna muutuja mõjutab ainult MPI suhtlemine domeeni ühendatud kobar Arvuta sõlmed (kas kohapeal või Azure VMs). Muutuja ignoreeritakse sõlmed lisatud plahvatuse Azure konfigureerimine.


* Mitmes eksemplaris Azure, juurutatud erinevate pilveteenustega (nt väljaandes purune, et Azure'i juurutuste erinevate sõlme malle või Azure VM Arvuta sõlmed juurutatud mitme pilveteenustega) ei saa käivitada MPI tööde haldamine. Kui teil on mitu Azure sõlme juurutuste, mis algas erinevate sõlme malle, peate käivitama MPI töö ainult ühe komplekti Azure sõlmed.


* Kui lisate Azure sõlmed klaster ja viia need võrgus, proovib HPC töö toiminguajasti teenus kohe alustada tööd sõlmed. Kui ainult osa oma töökoormus saate käivitada Azure, veenduge, et värskendada või töö mallide määratleda, mis tüüpi töö saab käitada Azure'i loomine. Näiteks tagamaks, et koos töö malli ainult käitatakse Azure sõlmed, lisada töö malli atribuudi sõlm rühmad ja valige AzureNodes vajalik väärtusena. Kohandatud rühmade jaoks oma Azure sõlmed loomiseks kasutage lisa-HpcGroup HPC PowerShelli cmdlet-käsk.


## <a name="next-steps"></a>Järgmised sammud

* Alternatiivina HPC Pack abil välja Azure'i paketi teenuse hallatavate kaustu, Arvuta sõlmed Azure MPI rakenduste käivitamist. Lugege teemat [kasutamine mitme eksemplari tööülesannete sõnumi läbides kasutajaliidese (MPI) rakenduste Azure'i paketi käivitamiseks](../batch/batch-mpi.md).

* Kui soovite käivitada Linuxi MPI rakendusi, et juurdepääs Azure'i RDMA võrgu, lugege teemat [Linux RDMA kobar käivitamiseks MPI rakenduste häälestamine](virtual-machines-linux-classic-rdma-cluster.md).

<!--Image references-->
[burst]: ./media/virtual-machines-windows-classic-hpcpack-rdma-cluster/burst.png
[iaas]: ./media/virtual-machines-windows-classic-hpcpack-rdma-cluster/iaas.png
[pingpong1]: ./media/virtual-machines-windows-classic-hpcpack-rdma-cluster/pingpong1.png
[pingpong2]: ./media/virtual-machines-windows-classic-hpcpack-rdma-cluster/pingpong2.png