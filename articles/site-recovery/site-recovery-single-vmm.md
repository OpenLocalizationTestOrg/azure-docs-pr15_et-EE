
<properties
    pageTitle="Azure'i saidi taastamine: Dubleeriva Hyper-V virtuaalmasinates ühes VMM serveris | Microsoft Azure'i"
    description="Selles artiklis kirjeldatakse, kuidas kopeerida Hyper-V virtuaalmasinates, kui teil on ainult üks VMM server."
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
    ms.workload="backup-recovery"
    ms.date="08/24/2016"
    ms.author="raynew"/>

#  <a name="replicate-hyper-v-virtual-machines-on-a-single-vmm-server"></a>Ise Hyper-V virtuaalmasinates ühes VMM serveris

Sellest artiklist saate teada, kuidas ise Hyper-V virtuaalmasinates asub VMM pilveteenusesse Hyper-V hosti server, kui teil on ainult üks VMM server juurutamise lugeda.

Azure'i on kaks eri [juurutamise mudelite](../resource-manager-deployment-model.md) loomise ja ressursside töötamine: Azure'i ressursihaldur ja klassikaline. Azure'i on kaks portaalide – Azure klassikaline portaali, mis toetab klassikaline juurutamise mudeli ja Azure portaali nii juurutamise mudelid tugi. Käesolevast artiklist leiate Azure'i portaalis dispersioonanalüüs häälestamise juhised.


Kui teil on küsimusi pärast selle artikli lugemist, postitage need Disqus kommentaaride allosas käesoleva artikli või [Azure taastamise teenused Foorum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="overview"></a>Ülevaade

Kui soovite paljundada Hyper-V VMs asub Hyper-V hosts VMM ja teil on ainult üks VMM server, saate [ise Azure](site-recovery-vmm-to-azure.md), või parkimise ühe VMM serveri vahel.

Soovitame, et teil korrata Azure, kuna Tõrkesiirde taastamine pole ja sujuvalt imitatsiooniga vahel pilved ja arvu juhised on vaja. Kui soovite korrata, kasutades ainult VMM server, saate teha järgmist:

- Kopeerida ühe autonoomse VMM server
- Kopeerida ühe VMM serveris juurutatud venitatud Windows kobar


## <a name="replicate-across-sites-with-a-single-standalone-vmm-server"></a>Saitidel kopeerida ühe autonoomse VMM server

![Autonoomse virtuaalserveri VMM](./media/site-recovery-single-vmm/single-vmm-standalone.png)

Selle stsenaariumi juurutamine ühe VMM server nimega virtuaalse masina esmane saidi ja korrata seda VM saidi saidi taastamine ja Hyper-V koopia abil.

1. Saate häälestada VMM Hyper-V VM. Soovitame installida SQL serveri eksemplar, mis kasutavad sama VM VMM aega kokku hoida. Kui soovite kasutada serveri eksemplar SQL serveri ja mõne katkestuste ilmneb, peate selle eksemplari taastada, enne kui saate taastada VMM.
2. Veenduge, et VMM server on vähemalt kahe pilve konfigureeritud. Ühe pilve sisaldab VMs soovite kopeerida, ja muud pilveteenuses toimib teisene asukoht. Pilveteenuse, mis sisaldab VMs, mida soovite kaitsta peaks olema:

    - Ühe või mitme VMM hosti rühmad sisaldav ühe või mitme Hyper-V hosti serverites iga host rühma.
    - Vähemalt üks Hyper-V virtuaalse masina iga Hyper-V hosti server.

3. Luua taastamise teenused vault, luua ja allalaadimine vault registreerimise võti ja registreerida VMM server autoriloomingut. Registreerimise käigus saate installida Azure saidi taastamise pakkuja VMM server.
4. Ühe või mitme parkimise VMM VM häälestamine ja lisage Hyper-V hosts, et need pilved.
3. Pilved kaitse sätete konfigureerimine. Saate määrata ühe VMM serveri nimi lähte- ja asukohad. Võrgu kaardistamine konfigureerimiseks vastendate VM võrgu jaoks VMs kaitstava VM võrgu dispersioonanalüüs Cloud pilve.
4. Lubada algse kopeerimine vms kaitstava võrgu kaudu, kuna mõlemad pilved asuvad samas serveris.
4. Konsooli Hyper-V halduri lubamine Hyper-V koopia Hyper-V Host, mis sisaldab VMM VM ja lubada kopeerimine VM. Veenduge, et lisate ei VMM VM mis tahes pilved, mis on kaitstud saidi taastamine. See tagab, et Hyper-V koopia sätted ei kaalu saidi taastamine.
5. Kui soovite luua taastamise, saate määrata sama VMM server lähte- ja sihtsaitide jaoks.

Järgige [selles artiklis](site-recovery-vmm-to-vmm.md) loomine võlvkelder, registreerida server ja kaitse häälestamine.

### <a name="what-to-do-in-an-outage"></a>Mida teha ka katkestuste

Kui täielik katkestuste ilmneb ja teil on vaja toimida saidi, tehke järgmist.

1.  Saidi konsooli Hyper-V halduri käivitamine planeerimata Tõrkesiirde kasutada VMM VM alates esmane, teisene saidile.
2.  Veenduge, et VMM VM tööks saidi teise.
3.  Taastamise teenused võlvkelder, käivitage planeerimata Tõrkesiirde nurjumise üle töökoormus VMs esmane, teisene pilved abil. Planeerimata Tõrkesiirde, VMs, on tõrkesiire Kinnita või valimiseks erinevate taastamise punkti vastavalt vajadusele.
4.  Pärast planeerimata Tõrkesiirde on lõpule jõudnud, kasutajad pääsevad töökoormus ressursside saidi teise.

Kui esmane saidi on tavalisel viisil uuesti, tehke järgmist.

1.  Konsooli Hyper-V halduri lubada tagant kopeerimine VMM VM, alustamiseks imitatsiooniga see sekundaarne esmane kaudu.
2.  Konsooli Hyper-V halduri käivitamine kavandatud Tõrkesiirde nurjumise tagasi VMM VM esmane saidile. Kinnitage see lõpuleviimiseks Tõrkesiirde. Seejärel lubada tagant kopeerimine imitatsiooniga soovitud VMM esmane, teisene käivitamiseks.
3.  Taastamise teenused võlvkelder, lubada tagant kopeerimine töökoormus VMs alustamiseks imitatsiooniga need kaudu sekundaarne esmane jaoks.
4.  Taastamise teenused võlvkelder, käivitage kavandatud Tõrkesiirde nurjumise tagasi töökoormus VMs esmane saidile. Kinnitage see lõpuleviimiseks Tõrkesiirde. Seejärel lubada tagant kopeerimine imitatsiooniga töökoormus VMs esmane, teisene käivitamiseks.



## <a name="replicate-across-sites-with-a-single-vmm-server-in-a-stretched-cluster"></a>Saitidel kopeerida ühe VMM server venitatud kobar

![Rühmitatud VMM virtuaalserveri](./media/site-recovery-single-vmm/single-vmm-cluster.png)

Asemel võtavad autonoomse VMM server VM, mis tiražeerib saidi nimega, saate teha VMM väga kättesaadav, juurutamine selle nimega Windows tõrkesiirdeklastrite VM. See pakub töökoormus paindlikkuse ja kaitse riistvara tõrke. Saidi taastamine juurutamiseks VMM VM juurutatud venitus kobar geograafiliselt eraldi saitidel. Soovitud toiming

1. VMM installimine Windows tõrkesiirdeklastrite virtuaalse masina ja valige suvandi Käivita server tugevalt saadaval installimise käigus.
2. SQL serveri eksemplar, mida kasutatakse VMM peaks kopeeritud SQL serveri Kättesaadavusrühmad koos, et on saidi teise andmebaasi koopia.
3. Järgige [selles artiklis](site-recovery-vmm-to-vmm.md) loomine võlvkelder, registreerida server ja kaitse häälestamine. Peate registreeruma iga VMM server klaster võlvkelder taastamise teenused. Selle tegemiseks aktiivse sõlme pakkuja installimine ja registreerimine VMM server. Seejärel installige pakkuja sõlmi.

### <a name="what-to-do-in-an-outage"></a>Mida teha ka katkestuste

Mõne katkestuste ilmnemisel VMM server ja selle vastav SQL Serveri andmebaas on üle nurjus ja juurde teisene saidilt.


## <a name="next-steps"></a>Järgmised sammud

[Lisateavet leiate teemast](site-recovery-vmm-to-vmm.md) üksikasjalik saidi taastamine juurutamise VMM VMM kopeerimise kohta.
