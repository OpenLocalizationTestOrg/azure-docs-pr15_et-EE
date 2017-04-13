<properties
    pageTitle="Paljundada VMware virtuaalmasinates ja füüsilise serveri Azure'i Azure saidi taastamine (pärand) | Microsoft Azure'i" 
    description="Kirjeldab, kuidas ise kohapealse VMs ja Windows/Linux Azure'i füüsiliste serverite kasutamine Azure saidi taastamise pärand juurutuse klassikaline portaalis." 
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="raynew"/>

# <a name="replicate-vmware-virtual-machines-and-physical-servers-to-azure-with-azure-site-recovery-using-the-classic-portal-legacy"></a>Paljundada VMware virtuaalmasinates ja füüsilise serveri Azure'i Azure saidi taastamine klassikaline portaalis (pärand)

> [AZURE.SELECTOR]
- [Azure'i portaal](site-recovery-vmware-to-azure.md)
- [Klassikaline portaal](site-recovery-vmware-to-azure-classic.md)
- [Klassikaline portaali (pärand)](site-recovery-vmware-to-azure-classic-legacy.md)


Tere tulemast Azure'i saidi taastamine! Selles artiklis kirjeldatakse pärand juurutamise jaoks imitatsiooniga kohapealse VMware virtuaalmasinates või Windows/Linux füüsilise serveri Azure klassikaline portaalis Azure saidi taastamise abil.

## <a name="overview"></a>Ülevaade

Peavad organisatsioonid BCDR strateegia, mis määrab, kuidas rakendused, töökoormus ja andmete püsida töötab ja kättesaadav ajal plaanitud ja plaanimata tööseisakute ja taastamiseks tavaline tingimuste nii kiiresti kui võimalik. BCDR strateegia kavandamine peaks äriandmete turvalised ja saab hoida ja tagama töökoormus pidevalt katastroofi ilmnemisel.

Saidi taastamine on Azure'i teenus, mis aitab BCDR strateegia kavandamine, orkesteroinnin dispersioonanalüüs füüsilise kohapealsed serverid ja virtuaalmasinates pilveteenusesse (Azure) või teisene andmekeskusesse. Kui katkestuste esineda teie peamine asukoht, teil ei õnnestu üle teisene asukohta hoida rakendused ja töökoormus saadaval. Te ei tagasi oma peamine asukoht kui tagastab see toiminguid. Lisateavet leiate [mis on Azure saidi taastamise?](site-recovery-overview.md)


>[AZURE.WARNING] See artikkel sisaldab **juhiseid pärand**. Ärge kasutage seda uue juurutuste. Selle asemel, [järgige neid juhiseid](site-recovery-vmware-to-azure.md) juurutamiseks saidi taastamine Azure portaali või [Kasutage neid juhiseid](site-recovery-vmware-to-azure-classic.md) konfigureerimiseks täiustatud juurutamise klassikaline portaalis. Kui olete juba juurutanud, selles artiklis kirjeldatud meetodiga, soovitame migreerimine täiustatud juurutamise klassikaline portaalis.


## <a name="migrate-to-the-enhanced-deployment"></a>Täiustatud juurutuse siirduda

Selles jaotises on oluline, kui te olete juba juurutanud saidi taastamise abil selle artikli juhiseid ainult.

Peate olemasoleva juurutamise migreeritavad

1. Juurutage uus saidi taastamine oma kohapealse saidi komponendid.
2. Saate konfigureerida uue konfiguratsiooniserveri mandaat VMware VMs automaatne tuvastamine.
3. Vaadake VMware serverid uue konfiguratsiooni serveriga.
3. Loo uus konfiguratsiooniserveri kaitse uus rühm.


Enne alustamist:

- Soovitame häälestamine hoolduse migreerimise jaoks.
- **Migreerimine masinad** suvand on saadaval ainult siis, kui teil on loodud pärand juurutamisel kaitse rühmi.
- Kui olete migreerimise juhiseid võib kuluda 15 minutit või pikemaks mandaat, värskendamine, et leida ja värskendamine virtuaalmasinates, nii et saate need lisada kaitse rühma. Saate värskendada käsitsi ootel asemel. 

Migreerimine järgmiselt:

1. Lugege [rohkem kasutusele klassikaline portaalis](site-recovery-vmware-to-azure-classic.md#enhanced-deployment). Vaadake üle täiustatud [arhitektuur](site-recovery-vmware-to-azure-classic.md#scenario-architecture)ja [eeltingimused](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment).
2. Teil pole praegu imitatsiooniga masinad mobiilsus teenuse desinstallida. Teenuse uus versioon installitakse arvutites, kui lisate need uue rühma kaitse.
3. Laadige alla [vault registreerimise võti](site-recovery-vmware-to-azure-classic.md#step-4-download-a-vault-registration-key) ja konfiguratsiooniserveri, protsessi server ja juhtslaidi target serveri komponendi installimiseks [käivitage ühendatud häälestamise viisard](site-recovery-vmware-to-azure-classic.md#step-5-install-the-management-server) . Lugege lisateavet [võimsus kavandamise](site-recovery-vmware-to-azure-classic.md#capacity-planning)kohta.
4. [Häälestamine identimisteabe](site-recovery-vmware-to-azure-classic.md#step-6-set-up-credentials-for-the-vcenter-server) selle saidi taastamise abil saate VMware server automaatselt avastada VMware VMs juurde. Lisateavet [vajalikke õigusi](site-recovery-vmware-to-azure-classic.md#vmware-permissions-for-vcenter-access).
5. Lisage [Vcenteri serverid või vSphere hosts](site-recovery-vmware-to-azure-classic.md#step-7-add-vcenter-servers-and-esxi-hosts). Võib kuluda rohkem serverite kuvada saidi taastamine portaalis 15 minutit.
6. [Kaitse uue rühma](site-recovery-vmware-to-azure-classic.md#step-8-create-a-protection-group)loomine. Võib kuluda kuni 15 minutit portaali värskendamine, et selle virtuaalmasinates avastatakse ja kuvada. Kui te ei soovi oodata saate esile tõsta management serveri nime (ärge klõpsake seda) > **Värskenda**.
7. Klõpsake jaotise uus kaitse **Masinad migreerida**.

    ![Konto lisamine](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration1.png)

8. **Valige masinad** valige kaitse rühma, mida soovite migreerida, ja seadmed, mida soovite migreerida.

    ![Konto lisamine](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration2.png)

9. **Sihtrakenduse sätete konfigureerimine** Määrake, kas soovite kasutada sama sätted kõik seadmed ja valige protsess server ja Azure storage konto. Kui teil pole serveris eraldi protsessi see olla selle serveri konfiguratsiooniserveri IP-aadress.


    ![Konto lisamine](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration3.png)

    > [AZURE.NOTE] [Migreerimise salvestusruumi kontode](../resource-group-move-resources.md) üle ühe tellimuse ressursi rühma või tellimustes salvestusruumi kontod kasutatakse juurutamine saidi taastamine ei toetata.

10. **Määrake kontod**, valige konto, loodud protsessi serveri juurde pääseda arvuti push mobiilsus teenuse uue versiooni.

    ![Konto lisamine](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration4.png)

11. Saidi taastamine kuvatakse kopeeritud andmete migreerimine sisestatud Azure storage konto. Soovi korral saate kasutada pärand juurutuse kasutatud salvestusruumi konto.
12. Pärast lõpetab töö virtuaalmasinates automaatselt sünkroonida. Pärast sünkroonimise lõpulejõudmist saate kustutada selle virtuaalmasinates pärand kaitse rühmast.
13. Kui kõik masinad on migreeritud saate kustutada rühma pärand kaitse.
14. Pidage meeles, kui sünkroonimine on lõpule jõudnud masinad ja Azure võrgusätted Tõrkesiirde atribuutide määramiseks.
15. Kui teil on olemasolevaid taastamine lepingud, saate need **Migreerida taastamise kava** suvandiga täiustatud juurutamise migreerida. Ainult tehke seda pärast kõigi kaitstud masinad migreerimist. 

    ![Konto lisamine](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration5.png)

>[AZURE.NOTE] Kui olete lõpetanud migreerimise [täiustatud artiklis](site-recovery-vmware-to-azure-classic.md)jätkata. Ülejäänud pärand artiklit pole enam oluline ja te ei pea enam it ** kirjeldatud juhiseid järgida.




## <a name="what-do-i-need"></a>Mida pean?

See diagramm näitab juurutamise komponendid.

![Uue vault](./media/site-recovery-vmware-to-azure-classic-legacy/architecture.png)

Siin on teil vaja:

**Komponent** | **Juurutamine** | **Üksikasjad**
--- | --- | ---
**Konfiguratsiooniserveri** | Azure'i standard A3 virtuaalarvuti sama tellimuse nimega saidi taastamine. | Konfiguratsiooniserveri koordinaadid suhtlemine kaitstud masinad protsessi server ja Azure juhtslaidi target serverites. See häälestab kopeerimise ja Azure koordinaate taastamiseks Tõrkesiirde korral.
**Serveri** | Azure'i virtuaalarvuti – kas Windows server Windows Server 2012 R2 Galerii (Windowsi masinad kaitsmiseks) või Linuxi server põhjal OpenLogic CentOS 6.6 Galerii (kaitsmiseks Linux masinad).<br/><br/> Kolm sortimine suvandid on saadaval – Standard A4, Standard D14 ja Standard DS4.<br/><br/> Server on konfiguratsiooniserveri Azure samasse võrku ühendatud.<br/><br/> Saidi taastamine portaalis häälestamine | See saab ja säilitab teie kaitstud masinad abil loodud bloobimälu oma Azure storage konto manustatud VHDs kopeeritud andmed.<br/><br/> Valige Standard DS4 spetsiaalselt konfigureerimise kaitse töökoormus ühtsete suurt jõudlust ja madal latentsus Premium salvestusruumi konto abil.
**Protsessi server** | Operatsioonisüsteemi Windows Server 2012 R2 server kohapealse virtuaalse või füüsilise<br/><br/> Soovitame seda paigutatakse samasse võrku ja LAN lõigu on masinad, mida soovite kaitsta, kuid seda saab käitada erinevas võrgus kui kaitstud masinad on L3 võrgu nähtavuse selle.<br/><br/> Saate selle häälestada ja registreeruda konfiguratsiooniserveri saidi taastamine portaalis. | Kaitstud masinad dispersioonanalüüs andmeid saata kohapealses protsessi serveris. See on kettapõhiste vahemälu soovite vahemälu dispersioonanalüüs andmeid, mida saab. Tehtav arv toiminguid, et andmeid.<br/><br/> Andmete vahemällu, tihendamine ja krüptimise enne saatmist kellelegi serveri optimeerib.<br/><br/> Käsitlemisel mobiilsus teenuse tõuketeatised installi.<br/><br/> Tehtav VMware virtuaalmasinates automaatne tuvastamine.
**Kohapealse masinad** | Kohapealse VMware virtuaalmasinates või füüsilise serveri operatsioonisüsteemi Windows või Linux. | Ühe või mitme masinad rakendada dispersioonanalüüs sätete konfigureerimine. Teil võib nurjuda üle mõne üksiku seadme või sagedamini mitme masinad, et koguda kokku üheks taastamis. 
**Mobiilsus teenuse** | Iga virtuaalse masina või füüsiline server, mida soovite kaitsta installitud<br/><br/> Saate käsitsi installida või lükata ja installitakse automaatselt protsess server kui lubate dispersioonanalüüs mõne seadme jaoks. | Mobiilsus teenuse saadab protsess server ajal algsete dispersioonanalüüs (resync). Pärast arvuti pole kaitstud olekusse (kui resync on lõpule viidud) mobiilsus teenuse sisaldab kirjutab kettale-mälu ja saadab server protsess. Windowsi serverite Applicationconsistency saavutamiseni, kasutades rakendust
**Azure'i saidi taastamise hoidla** | Saate luua saidi taastamise hoidla Azure tellimusega ja registreerida serverid autoriloomingut. | Funktsiooni hoidla koordinaadid ja orchestrates andmete kopeerimine, Tõrkesiirde ja taastamine oma kohapealse saidi ja Azure vahel.
**Süsteemi dispersioonanalüüs** | **Interneti üle**– Communicates ja dubleerivate andmete kaitstud kohapealsed serverid Azure'i abil turvalist SSL/TLS kanal Interneti kaudu. See on vaikimisi.<br/><br/> **VPN-/ ExpressRoute**– Communicates ja dubleerivate andmete vahel kohapealsed serverid ja Azure VPN-ühenduse kaudu. Peate VPN saidilt või ExpressRoute seost oma kohapealse saidi ja Azure võrgu häälestamine.<br/><br/> Kuvatakse valige, kuidas soovite saidi taastamine juurutamisel korrata. Funktsiooni süsteem ei saa muuta, kui see on konfigureeritud olemasoleva masinad kopeerimine mõjutamata. | Määratud pole kumbagi valiku korral peab teil avamiseks mis tahes sissetuleva võrgu pordid kaitstud arvutites. Kõik võrguühenduse käivitatakse kohapealse saidi kaudu. 

## <a name="capacity-planning"></a>Võimsuse planeerimine

Peate arvesse võtta põhivaldkonnas:

- **Andmeallika keskkonnas**– The VMware taristu, allikas seadme sätted ja nõuetele.
- **Komponendi serverites**– protsess server, konfiguratsiooniserveri ja serveri 

### <a name="considerations-for-the-source-environment"></a>Kaalutlused allika keskkonnas

- **Ketas maksimummaht**– ketas, mis võivad olla seotud virtuaalse masina praeguse maksimaalne maht on 1 TB. Seega on ka allikas ketas, mida saate kopeeritud maksimaalne maht piiratud 1 TB.
- **Maksimummaht allikas**– ühe andmeallika masina maksimaalne maht on 31 TB (31 ketast) ja serveri jaoks ette valmistatud D14 eksemplariga. 
- **Mitmest allikast serveri kohta**– mitme andmeallika masinad saab kaitsta lisamine ühe serveri. Ühe andmeallika arvuti ei saa siiski kaitstud mitme juhtslaidi target serverites, kuna nagu ketast korrata, luuakse VHD, mis kajastab selle ketta Azure'i bloobimälu ja lisatud andmed ketas serveri.  
- **Maksimum iga päev muuta andmeallika kohta**– on kolm tegureid, mis tuleb käsitleda kui arutate soovitatavaid muutmine määr allikas. Jaoks vastavalt target kaalutlused peavad kahe IOPS target kettal iga toimingu allikas. See on vanu andmeid lugeda ja kirjutada uute andmete juhtub kettal Target (sihtkoht). 
    - **Iga päev muuta protsess server ei toeta**– allika arvuti ei saa jaotada mitme protsessi serveriga. Ühe protsessi server toetab kuni 1 TB iga päev muutmine määr. Seega on 1 TB maksimaalne igapäevane andmeid muuta Toetatud andmeallikate kohapeal. 
    - **Maksimaalse läbilaskevõime ei toeta target ketas**– maksimum piimapütt kohta allikas ketas ei saa rohkem kui 144 GB päevas (koos 8 K kirjutamine suurus). Vaata läbilaskevõime ja IOPs eesmärgi jaotises juhtslaidi target tabeli erinevate kirjutamine suurused. Kuna iga andmeallika silma genereeritud 2 IOPS kettal target tuleb see arv kaks jagada. Lugege [Azure skaleeritavus ja jõudluse sihtkohtade](../storage/storage-scalability-targets.md#scalability-targets-for-premium-storage-accounts) target premium salvestusruumi konto konfigureerimisel.
    - **Maksimaalse läbilaskevõime salvestusruumi konto ei toeta**– allika ei saa ühendada mitu salvestusruumi kontod. Antud, et salvestusruumi konto võtab kuni 20 000 taotlusi sekundis ja, et iga andmeallika silma genereeritud 2 IOPS serveri veebisaidil, soovitame teil üle allika IOPS arvu kuni 10 000. Lugege [Azure skaleeritavus ja jõudluse sihtkohtade](../storage/storage-scalability-targets.md#scalability-targets-for-premium-storage-accounts) allika premium salvestusruumi konto konfigureerimisel.

### <a name="considerations-for-component-servers"></a>Kaalutlused komponent serverite jaoks

Tabel 1 Kokkuvõte virtuaalse masina suurused konfiguratsiooni ja juhtslaidi target serverites.

**Komponent** | **Azure'i juurutatud eksemplarid** | **Südamikud** | **Mälu** | **Max ketast** | **Ketta suurus**
--- | --- | --- | --- | --- | ---
Konfiguratsiooniserveri | Standardse A3 | 4 | 7 GB | 8 | 1023 GB
Serveri | Standardse A4 | 8 | 14 GB | 16 | 1023 GB
 | Standardse D14 | 16 | 112 GB | 32 | 1023 GB
 | Standardse DS4 | 8 | 28 GB | 16 | 1023 GB

**Tabel 1**

#### <a name="process-server-considerations"></a>Protsess serveri, peaksite arvesse võtma

Üldiselt protsessi serveri suurus sõltub igapäevane muutmine üle kõik kaitstud töökoormus.


- Teil on vaja teha toiminguid nagu Tekstisisese tihendamise ja krüptimise piisavalt Arvuta.
- Protsessi server kasutab vastavalt vahemälu. Veenduge, et ruumi soovitatav vahemälu ja ketta läbilaskevõimet on võimalik lihtsustada andmete muudatused talletatud võrgu kitsaskoht või katkestuste korral. 
- Piisavalt ribalaiust tagada, et protsessi serverisse üles laadida andmed serveri pidev andmete kaitsmiseks. 

Tabel 2 annab ülevaate protsessi serveri juhistele.

**Andmete muutmine määr** | **CPU** | **Mälu** | **Vahemälu ketta suurus**| **Vahemälu ketta läbilaskevõimet** | **Läbilaskevõime sissepääsu/sealt**
--- | --- | --- | --- | --- | ---
< 300 GB | 4 vCPUs (2 sockets * 2 tuuma @ 2,5 GHz) | 4 GB | 600 GB | 7-10 MB sekundis | 30 Mbps/21 Mbps
300 kuni 600 GB | 8 vCPUs (2 sockets * 4 tuuma @ 2,5 GHz) | 6 GB | 600 GB | 11-15 MB sekundis | 60 Mbps/42 Mbps
Kuni 1 TB 600 GB | 12 vCPUs (2 sockets * 6 tuuma @ 2,5 GHz) | 8 GB | 600 GB | 16-20 MB sekundis | 100 Mbps/70 Mbps
> 1 TB | Mõne muu protsess serveri juurutamine | | | | 

**Tabel 2**

Kui: 

- Sissepääsu on allalaadimine läbilaskevõime (sisevõrgu protsessi lähte- ja serveri vahel).
- Sealt on üles läbilaskevõime (internet protsessi serveri ja serveri vahel). Sealt arvude eeldavad Keskmine 30% protsessi serveri tihendamise.
- Vahemälu jaoks on soovitatav eraldi kettal OS väikseima 128 GB vaba protsessi kõikides serverites.
- Vahemälu ketta läbilaskevõimet jaoks järgmised salvestusruumi kasutati võrdlemine: 8 SAS draivid 10 K p/min RAID 10 konfiguratsioon.

#### <a name="configuration-server-considerations"></a>Konfiguratsiooni serveri kaalutlused

Iga konfiguratsiooniserveri toetab kuni 100 allikas masinad 3-4 maht. Kui teie juurutusega on suurem soovitame teise konfiguratsiooniserveri juurutamist. Vaadake vaikeatribuudid virtuaalse masina konfiguratsiooniserveri tabel 1. 

#### <a name="master-target-server-and-storage-account-considerations"></a>Juhtslaidi target serveri ja salvestusruumi konto kaalutlused

Mäluruumi iga serveri jaoks sisaldab mõnda OS ketas, säilitamise helitugevust ja andmete ketast. Säilituspoliitika ketas säilitab ketta muudatused töölehe saidi taastamine portaalis määratletud säilituspoliitika akna kestel.  Vaadake virtuaalse masina atribuutide serveri tabel 1. Tabel 3 näitab, kuidas a4 ketast kasutatakse.

**Eksemplari** | **OS kettale** | **Säilitus.** | **Andmete ketast**
--- | --- | --- | ---
 | | **Säilitus.** | **Andmete ketast**
Standardse A4 | 1 ketta (1 * 1023 GB) | 1 ketta (1 * 1023 GB) | 15 ketast (15 * 1023 GB)
Standardse D14 |  1 ketta (1 * 1023 GB) | 1 ketta (1 * 1023 GB) | 31 ketast (15 * 1023 GB)
Standardse DS4 |  1 ketta (1 * 1023 GB) | 1 ketta (1 * 1023 GB) | 15 ketast (15 * 1023 GB)

**Tabel 3**

Serveri kavandamise sõltub:

- Azure'i salvestusruumi jõudlus ja piirangud
    - Maksimumarv tugevalt kasutatud ketast Standard taseme VM, on umbes 40 (500/20 000 IOPS kohta ketas) ühe salvestusruumi konto. Lugege kohta [kindlad sccounts skaleeritavus eesmärgid](../storage/storage-scalability-targets.md#scalability-targets-for-standard-storage-accounts) ning [premium salvestusruumi sccounts](../storage/storage-scalability-targets.md#scalability-targets-for-premium-storage-accounts).
-   Iga päev muuta 
-   Säilituspoliitika helitugevuse salvestusruumi.

Pange tähele järgmist.

- Ühe andmeallika ei saa ühendada mitu salvestusruumi kontod. See kehtib minge salvestusruumi kontod kaitse konfigureerimisel valitud andmed, kuhu. OS ja säilitamine ketast tavaliselt minge automaatselt juurutatud salvestusruumi konto.
- Nõutav säilitamise mälu maht sõltub igapäevane muutmine määr ja säilitamise päevade arv. Serveri kohta nõutavat säilitamise mälu = kokku piimapütt allikast päevas * säilitamise päevade arv. 
- Iga serveri on ainult üks säilituspoliitika helitugevust. Säilituspoliitika helitugevuse jagatakse ketast, mis on seotud serveri. Näiteks:
    - Kui puudub allika masina 5 ketast ja iga ketta genereeritud allika 120 IOPS (8K suurus), see tähendab, et 240 IOPS kettale (2 toimingute kohta allikas IO kettal target) kohta. 240 IOPS on Azure'is ketta IOPS limiit 500 kohta.
    - Säilitus-draivil muutub 120 * 5 = 600 IOPS ja see võib muutuda on pudelikaela. Selle stsenaariumi korral oleks hea strateegia veel ketast lisamine säilituspoliitika mahtu ja selle üle, kui RAID triip konfiguratsiooni üle. Kuna selle IOPS on jaotatud üle mitme ketta parandab jõudlust. Draivid lisatakse säilituspoliitika helitugevuse arv on järgmine:
        - Kokku IOPS allika keskkonnast / 500
        - Kokku piimapütt päevas allikas keskkonnast (tihendamata) / 287 GB. 287 GB on maksimaalne läbilaskevõime target kettal päevas ei toeta. Selle mõõdiku sõltuvad kirjutamine Muuda suurust koos teguri 8K, kuna sel juhul 8K on töövootüüpe eeldatakse, et kirjutamine suurus. Näiteks kui kirjutamine maht on 4K siis läbilaskevõime on 287/2. Ja kui kirjutamine maht on 16K siis läbilaskevõime 287 * 2.
- Arvu salvestusruumi kontode nõutav = kokku allika IOPs/10 000.


## <a name="before-you-start"></a>Enne alustamist

**Komponent** | **Nõuded** | **Üksikasjad**
--- | --- | --- 
**Azure'i konto** | Peate [Microsoft Azure'i](https://azure.microsoft.com/) kontosse. Saate alustada [tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/).
**Azure'i salvestusruum** | Peate konto Azure storage kopeeritud andmete talletamiseks<br/><br/> Kas kontot tuleks [Tavakasutajakonto geograafilise liigne salvestusruumi](../storage/storage-redundancy.md#geo-redundant-storage) või [Premium salvestusruumi konto](../storage/storage-premium-storage.md).<br/><br/> See peab piirkonna Azure saidi taastamise teenuste ja olema sama tellimusega seostatud. Me ei toeta salvestusruumi kontod ressursi rühma [uue Azure portaali](../storage/storage-create-storage-account.md) abil loodud liikuda.<br/><br/> Lisateavet vt [Sissejuhatus Microsoft Azure'i Tabelimäluga](../storage/storage-introduction.md)
**Azure virtuaalse võrgu** | Peate Azure virtuaalse võrgu, mis on aluseks juurutatakse konfiguratsiooniserveri ja serveri. See peaks olema sama tellimuse ja regiooni Azure'i saidi taastamise hoidla. Kui soovite andmed kopeerida ExpressRoute või VPN-ühenduse kaudu Azure virtuaalse võrgu peab olema ühendatud kohapealse võrgu üle mõne ExpressRoute ühenduse või VPN saidilt.
**Azure'i ressursse** | Veenduge, et teil on piisavalt Azure ressursse juurutamine kõik komponendid. Lugege rohkem teemast [Azure tellimuse piirangud](../azure-subscription-service-limits.md).
**Azure'i virtuaalmasinates** | Virtuaalmasinates, mida soovite kaitsta peavad vastama [Azure eeltingimused](site-recovery-best-practices.md).<br/><br/> **Ketas arv**– kuni 31 ketast, saate toetatud ühes kaitstud serveris<br/><br/> **Ketas suurused**– üksikute ketta maht ei tohiks üle 1023 GB<br/><br/> **Rühmitamist**– klaster serverid ei toetata<br/><br/> **Buutimine**– ühendatud Extensible püsivara Interface (UEFI) / Extensible püsivara Interface(EFI) buutimine ei toetata<br/><br/> **Maht**-BitLockeri krüptitud maht ei toetata<br/><br/> **Serverite nimed**– nimed peaks olema vahemikus 1 kuni 63 märki (tähti, numbreid ja sidekriipse). Nimi peab tähe või numbri alguse ja lõpu tähe või numbri. Pärast masina on kaitstud, saate muuta Azure nimi.
**Konfiguratsiooniserveri** | Tellimuse jaoks konfiguratsiooniserveri luuakse Standard A3 virtuaalse masina põhjal Azure saidi taastamine Windows Server 2012 R2 Galerii pilt. See on loodud uue pilveteenuses esimene eksemplar. Kui valite avaliku Interneti ühenduvus tüübiks konfiguratsiooniserveri luuakse pilveteenusesse reserveeritud avaliku IP-aadress.<br/><br/> Installitee peaks olema ainult inglise märke.
**Serveri** | Azure virtuaalse masina, standard A4, D14 või DS4.<br/><br/> Installitee peaks olema ainult inglise märke. Näiteks tuleb teed **/usr/local/ASR** juhtslaidi target serveris, kus töötab Linuxi jaoks.
**Protsessi server** | Saate juurutada protsessi serveri füüsilise või töötab Windows Server 2012 R2 uusimaid värskendusi. C: installimine /.<br/><br/> Soovitame teil koht serveri sama võrgu ja alamvõrgu on masinad, mida soovite kaitsta.<br/><br/> Installige VMware vSphere CLI 5.5.0 protsessi serveris. VMware vSphere CLI komponent nõutakse serveris protsessi avastamine virtuaalmasinates haldab vCenter server või töötab ka ESXi hosti virtuaalmasinates.<br/><br/> Installitee peaks olema ainult inglise märke.<br/><br/> ReFS failisüsteemi ei toetata.
**VMware** | VMware vCenter server oma VMware vSphere hypervisors haldamine. See peaks töötama vCenter versioon 5.1 või 5,5 uusimaid värskendusi.<br/><br/> Ühe või mitme vSphere hypervisors, mis sisaldab VMware virtuaalmasinates, mida soovite kaitsta. Selle hypervisor peaks töötama ESX/ESXi 5.1 või 5,5 versiooni uusimaid värskendusi.<br/><br/> VMware virtuaalmasinates peaks olema installitud ja töötab VMware tööriistad. 
**Windowsi seadmed** | Kaitstud füüsilise serveri või VMware virtuaalmasinates opsüsteemi Windows on mitmeid tingimusi.<br/><br/> A toetatud 64-bitine opsüsteem: **Windows Server 2012 R2**, **Windows Server 2012**, või **Windows Server 2008 R2 hoolduspaketiga veebisaidil vähemalt SP1**.<br/><br/> Hosti nimi, haakepunktide, seadme nimed, Windows system tee (nt: C:\Windows) peaks olema ainult inglise keeles.<br/><br/> Operatsioonisüsteem peaks olema installitud C:\ ketas.<br/><br/> Toetatakse ainult ketta. Dünaamiliste ketast pole toetatud.<br/><br/> Tulemüüri reeglid kaitstud masinad tuleks lubada neil jõuda konfiguratsiooni ja juhtslaidi target serverid Azure.p ><p>Teil on vaja administraatorikonto (peab olema kohaliku administraatori Windowsi arvutisse) push install mobiilsus teenuse Windows serverites. Kui on esitatud konto-domeeni konto peate keelata Remote kasutaja juurdepääsu reguleerimine kohalikus arvutis. See lisada väärtusega 1 jaotises HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System LocalAccountTokenFilterPolicy DWORD-registrikirje. Lisage registrikirje CLI avatud cmd või PowerShelli ja sisestage **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`**. [Lisateavet leiate teemast](https://msdn.microsoft.com/library/aa826699.aspx) umbes juurdepääsu reguleerimine.<br/><br/> Pärast Tõrkesiirde, kui soovite, et ühenduse loomine Windows Azure kaugtöölaua virtuaalmasinates veenduge, et kaugtöölaua on lubatud kohapealse kohapeal. Kui loote pole üle VPN, tuleks tulemüüri reeglid luba kaugtöölaua Interneti kaudu.
**Linux masinad** | Toetatud 64-bitine operatsioonisüsteem on: **Centos 6.4, 6.5, 6.6**; **Oracle'i Enterprise Linux 6.4 6.5 operatsioonisüsteemiga punane rolli ühilduvad tuum või purunematu Enterprise tuum väljaanne 3 (UEK3)**, **SUSE Linux Enterprise Server 11 SP3**.<br/><br/> Tulemüüri reeglid kaitstud arvutites tuleks lubada neil konfigureerimine ja Azure juhtslaidi target serverites.<br/><br/> muutsite failid kaitstud masinad peaks sisaldama kirjed, mis kohaliku hosti nimi vastendada kõik NICs seotud IP-aadressid <br/><br/> Kui soovite ühendada Azure virtuaalse masina, mis töötab Linux pärast Tõrkesiirde Secure Shell kliendi (ssh), tagada teenuse Secure Shell kaitstud arvutis on määratud käivitamine automaatselt süsteemi käivitamine ja, et lubada tulemüüri reeglid on ssh sellega ühendus.<br/><br/> Hosti nimi, haakepunktide, seadme nimed, ja Linux süsteemi teed ja faili nimi (nt/jne; /usr) tuleks ingliskeelsena ainult.<br/><br/> Kaitse saab lubada kohapealse masinad järgmised salvestusruumi:-<br>Failisüsteemi: EXT3, ETX4, ReiserFS, XFS<br>Silma tarkvara-seadme plaanuri (silma)<br>Helitugevuse haldur: LVM2<br>HP CCISS kontrolleril salvestusruumi füüsilise serverite ei toetata.
**Kolmanda osapoole** | Teatud juurutamise komponendid selle stsenaariumi sõltuvad kolmanda osapoole tarkvara õigesti. Täieliku loendi leiate [kolmanda osapoole tarkvara teatised ja teave](#third-party)


### <a name="network-connectivity"></a>Võrguühendus

Võrguühendus vahel oma kohapealse saidi ja kus taristu komponentide (konfiguratsiooniserveri, juhtslaidi target serverid) on juurutatud Azure virtuaalse võrgu konfigureerimisel on teil kaks võimalust. Peate otsustada, millist võrgu Ühenduvus suvandit enne juurutamist oma konfiguratsiooniserveri kasutada. Peate selle sätte juurutamise ajal valimiseks. Ei saa hiljem muuta.

**Internet:** Suhtlus ja dispersioonanalüüs kohapealsed serverid (protsessi server, kaitstud masinad) ja Azure infrastruktuuri komponent serverid (konfiguratsiooniserveri, serveri) vahel juhtub serverites konfiguratsiooni ja juhtslaidi target üle turvalist SSL/TLS-ühendus asutusesisesest avaliku lõpp-punktid. (Erandiks on protsess server ja TCP port 9080, mis on krüptimata serveri vahelise ühenduse. Ainult juhtelemendi seotud dispersioonanalüüs protokolli dispersioonanalüüs häälestamise vahetatakse selle ühenduse.)

![Juurutamise diagramm internet](./media/site-recovery-vmware-to-azure-classic-legacy/internet-deployment.png)

**VPN**: VPN-ühenduse kaudu kohapealse võrgu ja Azure virtuaalse võrgu konfiguratsiooniserveri ja juhtslaidi target serverid kasutatakse vahel juhtub suhtluse ja dispersioonanalüüs kohapealsed serverid (protsessi server, kaitstud masinad) ja Azure infrastruktuuri komponent serverid (konfiguratsiooniserveri, serveri) vahel. Veenduge, et kohapealse võrgu on Azure virtuaalse võrku ühendatud mõne ExpressRoute ühenduse või-saidilt VPN-ühendus.

![Juurutusskeem VPN](./media/site-recovery-vmware-to-azure-classic-legacy/vpn-deployment.png)


## <a name="step-1-create-a-vault"></a>Samm 1: Loo võlvkelder

1. Logige sisse [haldusportaali](https://portal.azure.com).


2. Laiendage **Data Services** > **Taastamise teenused** ja klõpsake nuppu **Saidi taastamise hoidla**.


3. Klõpsake nuppu **Loo uus** > **kiire loomine**.

4. Sisestage väljale **nimi**sõbralik nimi, mis tähistavad vault.

5. **Piirkond**, valige piirkonnas vault jaoks. Märkige ruut toetatud regioonide leiate [Azure'i saidi taastamise hinnad üksikasjad](https://azure.microsoft.com/pricing/details/site-recovery/) geograafilised saadavus

6. Klõpsake nuppu **Loo vault**.

    ![Uue vault](./media/site-recovery-vmware-to-azure-classic-legacy/quick-start-create-vault.png)

Kontrollige, et kinnitada, et vault on loodud. Vault kirjas **aktiivseks** **Taastamise teenused** avalehele.

## <a name="step-2-deploy-a-configuration-server"></a>Samm 2: Juurutamine serveri konfiguratsioon

### <a name="configure-server-settings"></a>Matemaatiline sümbol

1. Klõpsake lehe **Taastamine teenused** vault Kiirkäivituse lehe avamiseks. Lühijuhend saate avada ka ikooni abil.

    ![Lühijuhend ikoon](./media/site-recovery-vmware-to-azure-classic-legacy/quick-start-icon.png)

2. Valige rippmenüüst, **vahel on kohapealse saidi VMware/füüsilise serverid ja Azure**.
3. Klõpsake **Juurutada konfiguratsiooniserveri** **Ettevalmistamine Target(Azure) ressursse** .

    ![Konfiguratsiooni serveri juurutamine](./media/site-recovery-vmware-to-azure-classic-legacy/deploy-cs2.png)

4. Määrake **Uus konfiguratsiooni Server üksikasjad** :

    - Nimi konfiguratsiooniserveri ja identimisteavet ühenduse loomiseks.
    - Tippige võrgu Ühenduvus rippmenüüst Vali **Avaliku Interneti** - või **VPN**. Pange tähele, et te ei saa muuta seda sätet pärast selle rakendamist.
    - Valige Azure võrk, mille server tuleks asub. Kui kasutate VPN tegemine kindel Azure võrk on ühendatud kohapealse võrgu ootuspäraselt. 
    - Määrake sisemine IP-aadress ja alamvõrgu, mis määratakse serveriga. Pange tähele, et kõik alamvõrgu neli esimest IP-aadressid on mõeldud sisemise Azure kasutuse. Kasutage mõnda muud saadaval IP-aadress.
    
    ![Konfiguratsiooni serveri juurutamine](./media/site-recovery-vmware-to-azure-classic-legacy/cs-details.png)

5. Nupu **OK** standard A3 virtuaalse masina põhjal Azure saidi taastamine Windows Server 2012 R2 Galerii pilt luuakse tellimuse jaoks konfiguratsiooniserveri. See on loodud uue pilveteenuses esimene eksemplar. Kui valisite Interneti kaudu ühenduse loomist pilveteenusesse reserveeritud avaliku IP-aadress. **Klõpsake vahekaarti** edu saate jälgida.

    ![Edenemise jälgimine](./media/site-recovery-vmware-to-azure-classic-legacy/monitor-cs.png)

6.  Kui loote Interneti kaudu, pärast konfiguratsiooniserveri on juurutatud teate avaliku IP-aadressi määratud selle portaalis Azure'i **Virtuaalmasinates** lehel. Seejärel **lõpp-punktid** vahekaardil Märkus avaliku HTTPS port vastendatud privaatne pordi 443. Kui registreerute juhtslaidi target ja protsessi serverite konfiguratsiooniserveri seda teavet hiljem vaja. Konfiguratsiooniserveri juurutatakse nende lõpp-punktid:

    - HTTPS: Avaliku pordi kasutatakse koordineerimine suhtlemine komponent serverid ja Azure Interneti kaudu. Privaatne pordi 443 kasutatakse koordineerimiseks suhtlemine komponent serverid ja Azure VPN üle.
    - Kohandatud: Avaliku pordi kasutatakse failback tööriista suhtlemiseks Interneti kaudu. Privaatne pordi 9443 kasutatakse failback tööriista suhtlemiseks VPN üle.
    - PowerShelli: Privaatne pordi 5986
    - Kaugtöölaua: privaatne porti 3389
    
    ![VM lõpp-punktid](./media/site-recovery-vmware-to-azure-classic-legacy/vm-endpoints.png)

    >[AZURE.WARNING] Ärge kustutada või muuta mis tahes lõpp-punktid loodud konfiguratsiooni Serveri juurutamisel avalik või privaatne pordinumber.

Konfiguratsiooniserveri juurutatakse automaatselt loodud Azure pilveteenuses reserveeritud IP-aadressiga. Reserveeritud aadress on vaja tagada, et konfiguratsiooni serveri pilvepõhise teenuse IP-aadress jääb samaks üle taaskäivitamisega virtuaalmasinates, (sh konfiguratsiooniserveri), klõpsake pilveteenusesse. Reserveeritud avaliku IP-aadressi tuleb käsitsi vaba konfiguratsiooniserveri on kasutuselt või jäävad reserveeritud. On vaikimisi piirang 20 reserveeritud avaliku IP-aadresside tellimuse kohta. [Lisateavet leiate teemast](../virtual-network/virtual-networks-reserved-private-ip.md) reserveeritud IP-aadresside kohta. 

### <a name="register-the-configuration-server-in-the-vault"></a>Konfiguratsiooniserveri autoriloomingut register

1. **Kiirkäivituse** lehel nuppu **Sihtrakenduse ettevalmistamiseks ressursid** > **registreerimise võti alla laadida**. Võtme faili luuakse automaatselt. See kehtib 5 päeva pärast seda, kui see on loodud. Kopeerige see konfiguratsiooniserveri.
2. Valige **Virtuaalmasinates** konfiguratsiooniserveri virtuaalmasinates loendist. Avage vahekaart **armatuurlaud** ja klõpsake nuppu **Ühenda**. **Avatud** allalaaditud RDP-faili abil kaugtöölaua konfiguratsiooniserveri sisselogimiseks. Kui kasutate VPN, kasutada sisemise IP-aadress (määratud juurutamisel konfiguratsiooniserveri aadress) kaugtöölaua ühendus kohapealse saidi kaudu. Konfiguratsiooni Server Azure'i saidi taastamise häälestusviisardi käivitatakse automaatselt, kui logite sisse esimest korda.

    ![Registreerimine](./media/site-recovery-vmware-to-azure-classic-legacy/splash.png)

3. **Kolmanda osapoole tarkvara installimine** nuppu **nõustun** alla laadima ja installima MySQL-i.

    ![MySQL-i installimine](./media/site-recovery-vmware-to-azure-classic-legacy/sql-eula.png)

4. Looge **MySQL-i serveri üksikasjad** identimisteabe sisselogimiseks MySQL-i serveri eksemplar.

    ![MySQL-i identimisteave](./media/site-recovery-vmware-to-azure-classic-legacy/sql-password.png)

5. **Interneti-sätted** saate määrata, kuidas konfiguratsiooniserveri on Interneti-ühenduse. Pange tähele järgmist.

    - Kui soovite kasutada kohandatud puhverserveri peaks häälestamist enne installimist pakkuja.
    - **Järgmise** klõpsamisel käivitatakse test puhverserveri ühenduse kontrollimiseks.
    - Kui kasutate kohandatud puhverserveri või teie vaikimisi puhverserveri nõuab autentimist, peate sisestama puhverserveri üksikasjad, sh aadress, portide ja mandaadi.
    - Järgmisel URL peaks olema puhverserveri kaudu.
        - *. hypervrecoverymanager.windowsazure.com
        - *. accesscontrol.windows.net
        - *. backup.windowsazure.com
        - *. blob.core.windows.net
        - *. store.core.windows.net
    - Kui teil on IP address-põhiste tulemüüri reeglid tagama, et reegleid suhtluse lubamiseks konfiguratsiooniserveri [Azure'i andmekeskuse IP-vahemikke](https://msdn.microsoft.com/library/azure/dn175718.aspx) ja HTTPS (443) protokollis kirjeldatud IP-aadressid. Mida oleks valge nimekiri IP-vahemikke Azure piirkond, mida soovite kasutada, ja kas Lääne US.

    ![Puhverserveri registreerimine](./media/site-recovery-vmware-to-azure-classic-legacy/register-proxy.png)

6. Määrake **Pakkuja tõrge sõnumi lokaliseerimine sätted** mis keeles soovite kuvada tõrketeated.

    ![Tõrge sõnumi registreerimine](./media/site-recovery-vmware-to-azure-classic-legacy/register-locale.png)

7. **Azure'i saidi taastamise registreerimise** Sirvi ja valige kopeeritud serveri võtme fail.

    ![Võtme faili registreerimine](./media/site-recovery-vmware-to-azure-classic-legacy/register-vault.png)

8. Valige viisardi lõpuleviimine lehel suvanditest:

    - Valige **Konto haldamine dialoogiboksi Käivita** määramiseks dialoogiboksi kontod haldamine avatakse pärast viisardi juhiste täitmist.
    - Valige **Loo ikooni Cspsconfigtool jaoks** töölaua otsetee lisamiseks konfiguratsiooniserveri nii, et saate avada dialoogiboksi **Kontod haldamine** igal ajal ilma uuesti viisard.

    ![Täielik registreerimine](./media/site-recovery-vmware-to-azure-classic-legacy/register-final.png)

9. Klõpsake viisardi **lõpule** . Parool on loodud. Kopeerige see turvalises asukohas. Peate selle autentimiseks ja konfiguratsiooniserveri protsess ja juhtslaidi target serverid registreerida. Seda kasutatakse ka side konfigureerimine serveris kanali terviklust tagada. Te saate taastada parooli, kuid peate uuesti registreerida juhtslaidi target ja protsessi serverid kasutades uus parool.

    ![Parooli](./media/site-recovery-vmware-to-azure-classic-legacy/passphrase.png)

Pärast registreerimist kirjas konfiguratsiooniserveri autoriloomingut lehel **Konfiguratsiooni serverites** .

### <a name="set-up-and-manage-accounts"></a>Häälestamine ja haldamine kontod

Juurutamise käigus saidi taastamine taotleb mandaat järgmisi toiminguid:

- VMware konto nii, et thatSite taastamine saab automaatselt discovery VMs vCenter serverites või vSphere hosts. 
- Kui lisate masinad kaitse nii, et saidi taastamine saate installida mobiilsus teenuse neid.

Kui olete registreerunud konfiguratsiooniserveri saate avada **Haldamine kontode** dialoogiboks lisamist ja haldamist kontod, mis tuleks kasutada neid toiminguid. On paar võimalust:

- Avage otsetee valinud abil loodud jaoks klõpsake viimase Lehekülje häälestus dialoogiboksi konfiguratsiooniserveri (cspsconfigtool).
- Klõpsake dialoogiboksi konfiguratsiooni serveri häälestamise lõpule Ava.

1. **Kontode haldamine** nuppu **Lisa konto**. Saate muuta ja kustutada olemasolevaid kontosid.

    ![Konto haldamine](./media/site-recovery-vmware-to-azure-classic-legacy/manage-account.png)

2. Määrake **Konto üksikasjad** konto nimi, mida kasutada Azure ja mandaadi (kasutaja ja domeeni nimi). 

    ![Konto haldamine](./media/site-recovery-vmware-to-azure-classic-legacy/account-details.png)

### <a name="connect-to-the-configuration-server"></a>Konfiguratsiooni serveriga 

On kaks võimalust konfiguratsiooni serveriga ühenduse loomiseks.

- VPN saidilt või ExpressRoute ühenduse kaudu
- Interneti kaudu 

Pange tähele järgmist.

- Interneti-ühendus kasutab lõpp-punktid virtuaalse masina koos serveri avaliku virtuaalse IP-aadress.
- VPN-ühendus kasutab sisemise serveri IP-aadress koos lõpp-punkti privaatne pordid.
- On ühekordse otsust otsustada, kas ühenduse (juhtelement ja dispersioonanalüüs andmed) oma kohapealsed serverid erinevate komponent serverid (konfiguratsiooniserveri, serveri) Azure töötab VPN-ühendus või Interneti-ühendus. Ei saa seda sätet hiljem muuta. Kui te ei tee peate seda stsenaariumi Juurutage uuesti ja reprotect oma masinad.  


## <a name="step-3-deploy-the-master-target-server"></a>Samm 3: Juurutamine serveri

1. Klõpsake **Ettevalmistamine Target(Azure) ressursid** > **Deploy serveri**.
2. Määrake juhtslaidi target serveri üksikasjad ja mandaadi. Server juurutatakse konfiguratsiooniserveri Azure samasse võrku sisse. Kui klõpsate mõnda Azure virtuaalse masina lõpuleviimiseks luuakse Windowsi või Linuxi galerii abil.

    ![Target serveri sätted](./media/site-recovery-vmware-to-azure-classic-legacy/target-details.png)

Pange tähele, et kõik alamvõrgu neli esimest IP-aadressid on mõeldud sisemise Azure kasutuse. Saate määrata mis tahes muud saadaval IP-aadress.

>[AZURE.NOTE] Valige Standard DS4 konfigureerimisel kaitse töökoormus, mis nõuavad ühtsete kõrge I/O jõudlus ja madal latentsus, et majutada I/O intensiivse töökoormus [Premium salvestusruumi konto](../storage/storage-premium-storage.md)abil.


3. Windowsiga juhtlehed target serveri VM luuakse nende lõpp-punktid. Pange tähele, et avaliku lõpp-punktid on loodud ainult juhul, kui teie ühenduse Interneti kaudu.

    - Kohandatud: Avaliku pordi protsessi serveri kasutatava dispersioonanalüüs andmeid saata Interneti kaudu. Protsessi server kasutab privaatne pordi 9443 üle VPN-i serveri dispersioonanalüüs andmeid saata.
    - Custom1: Avaliku pordi kasutab protsessi server metaandmete saata Interneti kaudu. Protsessi server kasutab privaatne pordi 9080 metaandmete saatmiseks serveri VPN üle.
    - PowerShelli: Privaatne pordi 5986
    - Kaugtöölaua: privaatne porti 3389

4. Linux serveri VM luuakse nende lõpp-punktid. Pange tähele, et avaliku lõpp-punktid on loodud ainult siis, kui loote Interneti kaudu.

    - Kohandatud: Avaliku pordi protsessi serveri kasutatava dispersioonanalüüs andmeid saata Interneti kaudu. Protsessi server kasutab privaatne pordi 9443 üle VPN-i serveri dispersioonanalüüs andmeid saata.
    - Custom1: Avaliku pordi kasutatakse protsessi server metaandmete saata Interneti kaudu. Privaatne pordi 9080 kasutavad protsessi serveri üle VPN-i serveri metaandmete saatmiseks
    - SSH: Privaatne port 22

    >[AZURE.WARNING] Ärge kustutada või muuta avalik või privaatne pordinumber ühte loodud juurutamisel juhtslaidi target serveri lõpp-punktid.

5. Klõpsake **Virtuaalmasinates** oodata virtuaalse masina käivitamiseks.

    - Kui see on märkuse Windows server üles Remote'i töölaua üksikasjad.
    - Kui see on Linuxi server ja loote VPN Märkus üle virtuaalse masina sisemise IP-aadress. Kui loote Interneti kaudu Märkus avaliku IP-aadressi.

6.  Logige serveri installimine on lõpetatud ja registreerida konfiguratsiooniserveri. 
7.  Kui kasutate operatsioonisüsteemi Windows:

    1. Kaugtöölaua virtuaalse masina algatada. Esmakordsel sisselogimisel skripti käivituvad PowerShelli aknas. Ärge seda sulgege. Kui see lõpeb Host Agent Config tööriist avatakse automaatselt serveri registreerimiseks.
    2. Määrake **Host Agent Config** konfiguratsiooni serveri ja pordi 443 sisemise IP-aadress. Saate sisemise aadress ja privaatne pordi 443, isegi juhul, kui te ei loo ühendust VPN üle Kuna virtuaalse masina on lisatud konfiguratsiooniserveri Azure samasse võrku. Jätke **Kasutamine HTTPS** lubatud. Sisestage parool konfiguratsiooniserveri eespool märgitud. Klõpsake nuppu **OK** , et server registreerida. Võite ignoreerida NAT suvandid. Ta ei kasutata.
    3. Kui teie hinnanguline säilituspoliitika ketas nõue on rohkem kui 1 TB saate konfigureerida virtuaalse ketta ja [salvestusruum](http://blogs.technet.com/b/askpfeplat/archive/2013/10/21/storage-spaces-how-to-configure-storage-tiers-with-windows-server-2012-r2.aspx) abil säilituspoliitika maht (r)
    
    ![Windows Serveri](./media/site-recovery-vmware-to-azure-classic-legacy/target-register.png)

8. Kui teil on Linux:
    1. Veenduge, et installitud on uusim Linuxi integreerimine Services (Poolelioleva) installitud enne installimist serveri. Poolelioleva uusim versioon koos juhiseid installimise kohta leiate [siit](https://www.microsoft.com/download/details.aspx?id=46842). Taaskäivitage arvuti pärast selle Poolelioleva installida.
    2. Klõpsake **sihtrakenduse ettevalmistamiseks (Azure) ressursse** **alla laadida ja installida täiendavat tarkvara (ainult puhul Linux juhtslaidi Target Server)**. Kopeerige fail allalaaditud tõrva virtuaalse masina e sftp klient. Teise võimalusena saate juurutatud linux serveri sisse logida ja *wget http://go.microsoft.com/fwlink/?LinkID=529757&clcid=0x409* abil saate alla laadida soovitud fail.
    2. Logige sisse kliendi Secure Shell server. Kui olete üle VPN Azure võrku ühendatud kasutada sisemise IP-aadress. Muul juhul kasutatakse välise IP-aadress ja SSH avaliku lõpp-punkti.
    3. Failid ekstraktida gzipitud Installeri käivitades: **tar – xvzf Microsoft-ASR_UA_8.4.0.0_RHEL6-64***
    ![Linux serveri](./media/site-recovery-vmware-to-azure-classic-legacy/linux-tar.png)
    4. Veenduge, et kataloogi, millele ekstraktitud tar faili sisu.
    5. Käsu kohaliku faili kopeerimine konfiguratsiooni serveri parool * *kaja* `<passphrase>` * > passphrase.txt**
    6. Käivitage käsk "**sudo. / install -t nii - on majutada -R -d /usr/local/ASR MasterTarget -i* `<Configuration server internal IP address>` * -p 443 -s y - c https -P passphrase.txt**".

    ![Target serveri registreerimine](./media/site-recovery-vmware-to-azure-classic-legacy/linux-mt-install.png)

9. Oodake mõni minut (10 – 15) ja kontrollige lehe serveri loendis registreeritud **serverid**on > vahekaarti**Konfiguratsiooni serverid** **Server üksikasjad** . Kui teil on Linux ja see ei registreerinud käivitada selle hosti config tööriista uuesti /usr/local/ASR/Vx/bin/hostconfigcli. Peate käivitades chmod root juurdepääsu õiguste määramine.

    ![Veenduge, et serveri](./media/site-recovery-vmware-to-azure-classic-legacy/target-server-list.png)

>[AZURE.NOTE] Võib kuluda kuni 15 minutit, kui registreerimine on lõpule jõudnud, portaalis loetletud serveri jaoks. Värskenda kohe, **Konfiguratsiooni serverid** lehel nuppu **Värskenda** .

## <a name="step-4-deploy-the-on-premises-process-server"></a>Samm 4: Kohapealse protsessi serveri juurutamine

Enne alustamist soovitame konfigureerida staatiline IP-aadress protsessi serveris nii, et see on tagatud olema püsiv taaskäivitamisega üle.

1. Klõpsake Kiirkäivituse > **kohapealse installida protsessi serveri** > **alla laadida ja installida protsessi server**.

    ![Protsessi serverit installida](./media/site-recovery-vmware-to-azure-classic-legacy/ps-deploy.png)

2.  Kopeerige allalaaditud zip-fail installida protsessi server kavatsete server. Zip-fail sisaldab kahte installifailid:

    - Microsoft-ASR_CX_TP_8.4.0.0_Windows*
    - Microsoft-ASR_CX_8.4.0.0_Windows*

3. Pakkige lahti arhiivi ja kopeerige installifailid serveris asukohta.
4. Käivitage selle **Microsoft-ASR_CX_TP_8.4.0.0_Windows*** installi faili ja järgige juhiseid. See installib kolmanda osapoole komponendid juurutamise jaoks vajalik.
5. Käivitage **Microsoft-ASR_CX_8.4.0.0_Windows***.
6. Valige lehel **Serveri režiim** **Protsessi Server**.
7. **Keskkonna üksikasjade** lehel tehke järgmist.


    - Kui soovite kaitse VMware virtuaalse masinad nuppu **Jah**
    - Kui soovite ainult füüsilise serveri kaitse ja seega ei pea VMware vCLI protsessi serverisse installitud. Klõpsake nuppu **ei** ja jätkata.

8. VMware vCLI installimisel, võtke arvesse järgmist.

    - **Toetatakse ainult VMware vSphere CLI 5.5.0**. Protsessi server ei tööta muude versioonide või vSphere CLI värskendused.
    - Laadige alla vSphere CLI 5.5.0 kaudu [siin.](https://my.vmware.com/web/vmware/details?downloadGroup=VCLI550&productId=352)
    - Kui installisite vSphere CLI vahetult enne hakkasite protsessi serveri installimise ja häälestamise seda ei tuvasta, oodake, kuni viis minutit enne proovige uuesti häälestada. See tagab, et keskkonna muutujate vaja vSphere CLI avastamiseks on lähtestatud õigesti.

9.  Valige **NIC valiku protsessi serveri** võrguadapteri, et server protsess peaks kasutama.

    ![Valige adapterit](./media/site-recovery-vmware-to-azure-classic-legacy/ps-nic.png)

10. **Konfiguratsiooni Server üksikasjad**:

    - IP-aadresside ja portide, kui loote üle VPN määrata konfiguratsiooni serveri ja pordi 443 sisemise IP-aadress. Muul juhul Määrake avaliku virtuaalse IP-aadress ja vastendatud avaliku HTTP lõpp-punkti.
    - Tippige parool serveri konfiguratsioon.
    - Kui soovite keelata kontrollimise automaatse tõuketeatised kasutamisel teenuse installida, tühjendage **kinnitamine Mobility teenuse tarkvara allkirja** . Allkirja kontrollimine tuleb protsessi serverist Interneti-ühendus.
    - Klõpsake nuppu **edasi**.

    ![Konfiguratsiooniserveri registreerimine](./media/site-recovery-vmware-to-azure-classic-legacy/ps-cs.png)


11. Valige **Valige installi draivi** vahemälu ketas. Serveri protsess peab vahemälu ketas vähemalt 600 GB vaba ruumi. Klõpsake nuppu **Installi**. 

    ![Konfiguratsiooniserveri registreerimine](./media/site-recovery-vmware-to-azure-classic-legacy/ps-cache.png)

12. Pange tähele, et võib olla vaja uuesti installimise lõpuleviimiseks server. **Konfigureerimine**serveris > **Serveri üksikasjad** veenduge, et protsessi server kuvatakse ja on edukalt registreeritud autoriloomingut.

>[AZURE.NOTE]Võib kuluda kuni 15 minutit, kui registreerimine on lõpule jõudnud, protsessi serveri konfiguratsiooniserveri väljaprindil kuvatakse. Kohe värskendamiseks värskendamine konfiguratsiooniserveri klõpsates konfiguratsiooni server lehe allosas nuppu Värskenda
 
![Kinnitage protsessi server](./media/site-recovery-vmware-to-azure-classic-legacy/ps-register.png)

Kui keelate ei allkirja kontrollimise jaoks mobiilsus teenuse registreerimisel protsessi server, saate seda teha hiljem järgmiselt:

1. Logige administraatorina protsessi server ja C:\pushinstallsvc\pushinstaller.conf faili redigeerimiseks avada. Jaotises **[PushInstaller.transport]** selle rea lisamine: **SignatureVerificationChecks = "0"**. Salvestage ja sulgege fail.
2. Taaskäivitage InMage PushInstall.


## <a name="step-5-update-site-recovery-components"></a>Juhis 5: Update saidi taastamine komponendid

Saidi taastamise komponendid on värskendatud aeg-ajalt. Kui uued värskendused on saadaval installige need järgmises järjestuses:

1. Konfiguratsiooniserveri
2. Protsessi server
3. Serveri
4. Failback tööriista (vContinuum)

### <a name="obtain-and-install-the-updates"></a>Hankige ja installige värskendused


1. Saidi taastamine **armatuurlaua**kaudu saate hankida konfiguratsiooni, protsess ja juhtslaidi target serverid värskendused. Linux installi väljavõte failid gzipitud installer ja käivitage käsk "sudo. / install" värskenduse installimiseks.
2. [Laadige alla](http://go.microsoft.com/fwlink/?LinkID=533813) uusim Failback tool(vContinuum).
3. Kui teil on virtuaalmasinates või füüsilise serveri, mis on juba installitud mobiilsus teenuse, saate värskendused teenuse järgmiselt:

    - **Suvand 1**: Laadige alla värskendused:
        - [Windows Server (ainult 64-bitine)](http://download.microsoft.com/download/8/4/8/8487F25A-E7D9-4810-99E4-6C18DF13A6D3/Microsoft-ASR_UA_8.4.0.0_Windows_GA_28Jul2015_release.exe)
        - [CentOS 6.4,6.5,6.6 (ainult 64-bitine)](http://download.microsoft.com/download/7/E/D/7ED50614-1FE1-41F8-B4D2-25D73F623E9B/Microsoft-ASR_UA_8.4.0.0_RHEL6-64_GA_28Jul2015_release.tar.gz)
        - [Oracle'i Enterprise Linux 6.4,6.5 (ainult 64-bitine)](http://download.microsoft.com/download/5/2/6/526AFE4B-7280-4DC6-B10B-BA3FD18B8091/Microsoft-ASR_UA_8.4.0.0_OL6-64_GA_28Jul2015_release.tar.gz)
        - [SUSE Linux Enterprise Server SP3 (ainult 64-bitine)](http://download.microsoft.com/download/B/4/2/B4229162-C25C-4DB2-AD40-D0AE90F92305/Microsoft-ASR_UA_8.4.0.0_SLES11-SP3-64_GA_28Jul2015_release.tar.gz)
        - Pärast värskendamist protsessi serveri mobiilsus teenuse värskendatud versioon on saadaval C:\pushinstallsvc\repository kausta serveris protsess.
    - **Variant 2**: kui teil on mõnda arvutisse installitud Mobility teenuse vanema versiooniga, saate automaatselt üle minna mobiilsus teenuse seadmesse portaalis haldus.

        1. Veenduge, et protsessi server on värskendatud.
        2. Veenduge, et kaitstud arvuti vastab [eeltingimused](#install-the-mobility-service-automatically) automaatselt mobiilsus teenuse, lükates nii, et värskenduse toimib ootuspäraselt.
        2. Valige jaotises kaitse, esile tõsta kaitstud arvuti ja klõpsake nuppu **Värskenda mobiilsus teenuse**. See nupp on ainult saadaval, kui on mobiilsus teenuse uuemat versiooni. 

            ![Valige vCenter server](./media/site-recovery-vmware-to-azure-classic-legacy/update-mobility.png)

Valige kontod Määrake administraatori konto, mida soovite kasutada mobiilsus teenuse kaitstud serveris värskendada. Klõpsake nuppu OK ja oodake, kuni käivitatud töö lõpuleviimiseks.


## <a name="step-6-add-vcenter-servers-or-vsphere-hosts"></a>Samm 6: VCenter serverid või vSphere hosts lisamine

1. Klõpsake **serverid** > **Konfiguratsiooni serverid** > konfiguratsiooniserveri >**Lisa vCenter Server** vCenter server või vSphere host lisada.

    ![Valige vCenter server](./media/site-recovery-vmware-to-azure-classic-legacy/add-vcenter.png)

2. Määrake server või host üksikasju ja valige protsess server, mida kasutatakse avastada.

    - Kui vCenter server ei tööta pordi 443 vaikimisi määratud pordi number, millel vCenter server töötab.
    - Protsessi server peab olema sama võrgus vCenter server/vSphere host ja peaks olema VMware vSphere CLI 5.5.0 installitud.

    ![Vcenteri serveri sätted](./media/site-recovery-vmware-to-azure-classic-legacy/add-vcenter4.png)


3. Pärast on discovery lõppemist vCenter server jaotises konfiguratsiooni serveri üksikasjad.

    ![Vcenteri serveri sätted](./media/site-recovery-vmware-to-azure-classic-legacy/add-vcenter2.png)

4. Kui kasutate-administraatori konto lisamiseks server või host, veenduge, et konto on järgmised õigused.

    - vCenter kontod peaks olema andmekeskuse, andmesalve, kausta, Host, võrku, ressursside, salvestusruumi vaated, virtuaalse masina ja vSphere jaotatud vahetamise õigused lubatud.
    - vSphere host kontod peaks olema andmekeskuse, andmesalve, kausta, Host, võrku, ressursside, virtuaalse masina ja vSphere jaotatud vahetamise õigusi lubatud



## <a name="step-7-create-a-protection-group"></a>Juhis 7: Kaitse rühma loomine

1. Avage **Kaitstud üksuste** > **Kaitse rühma** > **kaitse rühma loomine**.

    ![Kaitse rühma loomine](./media/site-recovery-vmware-to-azure-classic-legacy/create-pg1.png)

2. Määrake lehel **Määrake rühma sätted** soovitud rühma nimi ja valige konfiguratsiooniserveri, mida soovite rühma luua.

    ![Rühma sätted](./media/site-recovery-vmware-to-azure-classic-legacy/create-pg2.png)

3. **Määrake Dispersioonanalüüs sätete** lehel kõik masinad jaotises kasutatud dispersioonanalüüs sätete konfigureerimine.

    ![Kaitse rühma dispersioonanalüüs](./media/site-recovery-vmware-to-azure-classic-legacy/create-pg3.png)

4. Sätted:
    - **Mitme VM järjepidevuse**: Kui lülitate selle loob ühiskasutuses rakenduse ühtsete taastamise punkte arvutites jaotises kaitse. See säte on kõige olulisemad, kui kõik jaotises kaitse masinad töötavad sama töökoormus. Kõik masinad on sama andmepunkti taastada. Saadaval ainult Windows serverite jaoks.
    - **RPO lävi**: teatiste luuakse kui järjestikusi andmeid kaitse dispersioonanalüüs RPO ületab konfigureeritud RPO lävi väärtus.
    - **Osutage taastamine säilitus**: säilituspoliitika aknas saate määrata. Kaitstud masinad taastada mis tahes punkti see aken.
    - **Rakenduse ühtsete hetktõmmise sagedus**: saate määrata, kui sageli luuakse taastamise punkte rakenduse ühtsete pilte sisaldavate.

Kaitse rühma saate jälgida, nagu need on loodud **Üksuste kaitstud** lehel.

## <a name="step-8-set-up-machines-you-want-to-protect"></a>Samm 8: Masinad, mida soovite kaitsta häälestamine

Peate mobiilsus teenuse installimine virtuaalmasinates ja füüsilise serveri, mida soovite kaitsta. Selleks on kaks võimalust:

- Automaatselt tõuketeatised ja teenuse serverist protsessi iga arvutisse installida.
- Teenuse käsitsi installida. 

### <a name="install-the-mobility-service-automatically"></a>Mobiilsus teenuse installitakse automaatselt

Kui lisate masinad kaitse rühma mobiilsus teenuse automaatselt lükata ja protsessi server iga arvutisse installitud. 

**Automaatselt tõuketeatised installige mobiilsus teenuse Windows serverites.** 

1. Installige uusimad värskendused protsessi server, nagu on kirjeldatud [Samm 5: uusimate värskenduste installimine](#step-5-install-latest-updates), ja veenduge, et protsessi server on saadaval. 
2. Veenduge, on võrguühendus allika arvuti ja protsess server ja allikas seadme pääseb protsessi serverist.  
3. Windowsi tulemüüri lubama **failide ja printeri ühiskasutus** ja **Windows Management seadmeid**konfigureerimine. Klõpsake jaotises Windowsi tulemüüri sätted, valige suvand "Luba rakenduse või läbi tulemüüri" ja valige rakendused, nagu on näidatud järgmisel pildil. Masinad, mis kuuluvad domeeni saate konfigureerida tulemüüri poliitika rühmapoliitika objekti.

    ![Tulemüüri sätted](./media/site-recovery-vmware-to-azure-classic-legacy/push-firewall.png)

4. Konto, mida kasutatakse installi tõuketeatised peab olema administraatorite rühma arvutisse, mida soovite kaitsta. Neid mandaate kasutatakse ainult tõuketeatised mobiilsus teenuse installimine ja te saatke neile masina lisamisel kaitse rühma.
5. Kui esitatud konto pole domeeni kontoga, peate keelata Remote kasutaja juurdepääsu reguleerimine kohalikus arvutis. See lisada väärtusega 1 jaotises HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System LocalAccountTokenFilterPolicy DWORD-registrikirje. Lisage registrikirje CLI avatud cmd või PowerShelli ja sisestage **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`**. 

**Automaatselt tõuketeatised installige mobiilsus teenuse Linux serverites.**

1. Installige uusimad värskendused protsessi server, nagu on kirjeldatud [Samm 5: uusimate värskenduste installimine](#step-5-install-latest-updates), ja veenduge, et protsessi server on saadaval.
2. Veenduge, on võrguühendus allika arvuti ja protsess server ja allikas seadme pääseb protsessi serverist.  
3. Veenduge, et konto on root kasutaja allikas Linux server.
4. Veenduge, et allikas Linux server muutsite fail sisaldab kirjeid, mis vastendada kõik NICs seotud IP-aadresside kohaliku hosti nimi.
5. Installige uusim openssh, openssh-server, openssl pakettide arvutisse, mida soovite kaitsta.
6. Veenduge, et SSH port 22 on lubatud ja töötab. 
7. Lubage SFTP alasüsteemi ja parooli autentimine sshd_config faili järgmiselt: 

    - a) sisse kui root.
    - (b) /etc/ssh/sshd_config fail, otsige üles rida, mis algab väärtusega **PasswordAuthentication**.
    - c) rea eemaldamine ja muutmine väärtuse "ei", "Jah".

        ![Linux mobility](./media/site-recovery-vmware-to-azure-classic-legacy/linux-push.png)

    - d) Real, mis algab väärtusega alasüsteemi otsimine ja eemaldamine rea.
    
        ![Linux tõuketeatised mobility](./media/site-recovery-vmware-to-azure-classic-legacy/linux-push2.png)    

8. Veenduge, et allikas seadme Linuxi variant on toetatud. 
 
### <a name="install-the-mobility-service-manually"></a>Mobiilsus teenuse käsitsi installimine

Tarkvarapakettide mobiilsus teenuse installimiseks kasutatud on C:\pushinstallsvc\repository serveris protsess. Protsessi serverisse sisse logida ja kopeerige vastav installipaketi allika masina vastavalt järgmises tabelis:-

| Andmeallika operatsioonisüsteem                               | Mobility pakett protsessi serveris                                                            |
|---------------------------------------------------    |------------------------------------------------------------------------------------------------------ |
| Windows Server (ainult 64-bitine)                          | `C:\pushinstallsvc\repository\Microsoft-ASR_UA_8.4.0.0_Windows_GA_28Jul2015_release.exe`         |
| CentOS 6.4, 6.5, 6.6 (ainult 64-bitine)                    | `C:\pushinstallsvc\repository\Microsoft-ASR_UA_8.4.0.0_RHEL6-64_GA_28Jul2015_release.tar.gz`     |
| SUSE Linux Enterprise Server 11 SP3 (ainult 64-bitine)     | `C:\pushinstallsvc\repository\Microsoft-ASR_UA_8.4.0.0_SLES11-SP3-64_GA_28Jul2015_release.tar.gz`|
| Oracle'i Enterprise Linux 6.4 6.5 (ainult 64-bitine)        | `C:\pushinstallsvc\repository\Microsoft-ASR_UA_8.4.0.0_OL6-64_GA_28Jul2015_release.tar.gz`       |


**Mobiilsus teenuse Windows serveris käsitsi installida**, tehke järgmist:

1. Kopeerige pakett **Microsoft-ASR_UA_8.4.0.0_Windows_GA_28Jul2015_release.exe** allika arvutisse ülaltoodud tabelis loetletud protsessi serveri kausta tee.
2. Mobility teenuse käivitatava käivitades allika arvutisse installida.
3. Järgige installiprogrammi juhiseid.
4. Valige **mobiilsus teenuse** roll ja klõpsake nuppu **edasi**.
    
    ![Mobiilsus teenuse installimine](./media/site-recovery-vmware-to-azure-classic-legacy/ms-install.png)

5. Jätke installikaust nimega vaikimisi Installitee ja klõpsake nuppu **Installi**.
6. Määrake **Host Agent** config IP-aadress ja HTTPS port configuration server.

    - Kui loote Interneti kaudu määrata pordi avaliku virtuaalse IP-aadress ja avaliku HTTPS-i lõpp-punkti.
    - Kui loote ühenduse VPN üle määrata sisemise IP-aadress ja pordi 443. Lahku **Kasutamine HTTPS** kontrollida.

    ![Mobiilsus teenuse installimine](./media/site-recovery-vmware-to-azure-classic-legacy/ms-install2.png)

7. Määrake server configuration parool ja klõpsake nuppu **OK** , et registreerida mobiilsus teenuse konfiguratsiooniserveri.

**Käsurea kaudu käivitamiseks tehke järgmist.**

1. Kopeerige parool on CX faili "C:\connection.passphrase" serveris ja käivitage see käsk. Käesolevas näites CX i 104.40.75.37 ja HTTPS port on 62519.

    `C:\Microsoft-ASR_UA_8.2.0.0_Windows_PREVIEW_20Mar2015_Release.exe" -ip 104.40.75.37 -port 62519 -mode UA /LOG="C:\stdout.txt" /DIR="C:\Program Files (x86)\Microsoft Azure Site Recovery" /VERYSILENT  /SUPPRESSMSGBOXES /norestart  -usesysvolumes  /CommunicationMode https /PassphrasePath "C:\connection.passphrase"`

**Mobiilsus teenuse Linux serveris käsitsi installida**.

1. Kopeerige ülaltoodud tabeli allikas seadme serverist protsessi põhjal vastav tõrva arhiivi.
2. Shell programmi avamiseks ja väljavõte ZIP tar Arhiiv kohalik tee, täites`tar -xvzf Microsoft-ASR_UA_8.2.0.0*`
3. Luua passphrase.txt faili kohalikku kausta, kuhu ekstraktitud tõrva arhiivi sisu, sisestades *`echo <passphrase> >passphrase.txt`* kest.
4. Installige mobiilsus teenuse sisestades *`sudo ./install -t both -a host -R Agent -d /usr/local/ASR -i <IP address> -p <port> -s y -c https -P passphrase.txt`*.
5. IP-aadress ja pordi määramiseks tehke järgmist.

    - Kui loote konfiguratsiooniserveri Interneti kaudu määrata konfiguratsiooni serveri virtuaalse avaliku IP-aadress ja avaliku HTTPS-i lõpp-punkti sisse `<IP address>` ja `<port>`.
    - Kui loote ühenduse VPN-ühenduse kaudu määrata sisemise IP-aadress ja 443.

**Käivitamiseks käsurea kaudu**:

1. Kopeerige parool on CX faili "passphrase.txt" serveris ja käivitage see käske. Käesolevas näites CX i 104.40.75.37 ja HTTPS port on 62519.

Tootmise server installimiseks tehke järgmist.

    ./install -t both -a host -R Agent -d /usr/local/ASR -i 104.40.75.37 -p 62519 -s y -c https -P passphrase.txt
 
Target (sihtkoht) serveris installimiseks tehke järgmist.


    ./install -t both -a host -R MasterTarget -d /usr/local/ASR -i 104.40.75.37 -p 62519 -s y -c https -P passphrase.txt

>[AZURE.NOTE] Kui lisate masinad kaitse rühma, mis on juba sobiv versioon mobiilsus teenuse seejärel tõuketeatised installimise vahele.


## <a name="step-9-enable-protection"></a>Samm 9: Luba kaitse

Kaitse lubamiseks lisate virtuaalmasinates ja füüsilise serveri kaitse rühma. Enne alustamist, Pange tähele järgmist.

- Virtuaalmasinates avastatakse iga 15 minuti ja võib kuluda kuni 15 minutit neid kuvada Azure'i saidi taastamine pärast on discovery.
- Keskkonna muutused virtuaalse masina (nt VMware tööriistad install) võivad ka kuni 15 minutit, et saidi taastamine värskendada.
- Saate viimast avastatud **Viimase KONTAKTI veebisaidil** välja vCenter server/ESXi hosti lehel **Konfiguratsiooni serverite** jaoks.
- Kui teil on juba loodud kaitse rühma ja vCenter Server või ESXi hosti lisamine pärast seda, kulub 15 minutit Azure saidi taastamise portaali värskendamiseks ja virtuaalmasinates loetletud dialoogiboksi **masinad kaitse rühma lisada** .
- Kui soovite kohe jätkamiseks kaitse rühma ootamata ajastatud Discovery masinad lisamise, tõstke esile konfiguratsiooniserveri (ärge klõpsake seda) ja klõpsake nuppu **Värskenda** .
- Virtuaalmasinates või füüsilise masinad kaitse rühma lisamisel protsessi server automaatselt sunnib ja installib mobiilsus teenuse allikas server, kui see pole juba installitud.
- Automaatse PTT süsteemi töötamiseks veenduge, et olete häälestanud oma kaitstud masinad, nagu on kirjeldatud eelmises etapis.

Lisage masinad järgmiselt:

1. Klõpsake **kaitstud üksuste** > **Kaitse rühma** > **masinad** > **masinad lisada**. Hea tava soovitame, et rühmade peaks kajastavad oma töökoormus nii, et lisate mõne kindla rakenduse samasse, kus käitatakse.
2. **Valige** virtuaalmasinates kui füüsilise serveri kaitsmisele **Lisamine füüsilise masinad** viisardi pakkuda IP-aadress ja sõbralik nimi. Valige operatsioonisüsteemi pere.

    ![V-Center serveri lisamine](./media/site-recovery-vmware-to-azure-classic-legacy/physical-protect.png)

3. **Valige** virtuaalmasinates kui VMware virtuaalmasinates kaitsmisele valige vCenter server, mis on hallata oma virtuaalmasinates (või EXSi host, mil nad töötab) ja valige masinad.

    ![V-Center serveri lisamine](./media/site-recovery-vmware-to-azure-classic-legacy/select-vms.png) 
4. Valige **Määrake Target ressursid** juhtslaidi target servers ja salvestusruumi kopeerimise ja valige, kas sätted tuleks kasutada kõigi töökoormus. Valige [Premium salvestusruumi konto](../storage/storage-premium-storage.md) konfigureerimisel kaitse töökoormus, mis nõuavad ühtsete kõrge IO jõudlus ja madal latentsus, et majutada IO intensiivse töökoormus. Kui soovite kasutada oma töökoormus ketast Premium salvestusruumi konto, peate kasutama juhtslaidi Target DS-sarja. Te ei saa kasutada Premium salvestusruumi ketast juhtslaidi Target DS sarja.

    >[AZURE.NOTE] Me ei toeta salvestusruumi kontod ressursi rühma [uue Azure portaali](../storage/storage-create-storage-account.md) abil loodud liikuda.

    ![vCenter server](./media/site-recovery-vmware-to-azure-classic-legacy/machine-resources.png)

5. Valige **Kontod määrata** selle konto kasutamiseks kaitstud masinad mobiilsus teenuse installimine. Automaatse installi mobiilsus teenuse jaoks on vajalik mandaat. Kui te ei saa valida mõne konto veenduge, et olete häälestanud ühte, nagu on kirjeldatud toiminguga 2. Pange tähele, et sellelt kontolt ei pääse juurde Azure. Windows Serveri konto peaks olema allikas server administraatoriõigused. Konto peab olema juursait Linux.

    ![Linux identimisteave](./media/site-recovery-vmware-to-azure-classic-legacy/mobility-account.png)

6. Klõpsake märkeruutu masinad kaitse rühma lisamise lõpetanud ja käivitada algse dispersioonanalüüs iga seadme jaoks. Saate jälgida olek lehel **tööde haldamine** .

    ![V-Center serveri lisamine](./media/site-recovery-vmware-to-azure-classic-legacy/pg-jobs2.png)

7. Lisaks saate jälgi olekut, klõpsates **Kaitstud üksused** > kaitse rühma nimi > **Virtuaalmasinates** . Pärast algse kopeerimine on lõpule jõudnud ja masinad on andmete sünkroonimine, kuvatakse need **kaitstud** olek.

    ![Virtuaalse masina tööde haldamine](./media/site-recovery-vmware-to-azure-classic-legacy/pg-jobs.png)


### <a name="set-protected-machine-properties"></a>Kaitstud seadme atribuutide seadmine

1. Pärast masina on **kaitstud** olek saate konfigureerida selle Tõrkesiirde atribuudid. Kaitse rühma üksikasjad valige arvuti ja avage vahekaart **konfigureerimine** .
2. Saate muuta selle nime, mille pööratakse masina Azure pärast Tõrkesiirde ja Azure virtuaalse masina suurus. Samuti võite märkida ruudu Azure võrk, millele seade on ühendatud pärast Tõrkesiirde.

    > [AZURE.NOTE] [Võrgu migreerimine](../resource-group-move-resources.md) üle ühe tellimuse ressursi rühma või tellimustes võrke kasutatakse juurutamine saidi taastamine ei toetata.

    ![Virtuaalne seadme atribuutide seadmine](./media/site-recovery-vmware-to-azure-classic-legacy/vm-props.png)

Pange tähele järgmist.

- Azure'i masina nimi peab vastama Azure nõuetele.
- Vaikimisi pole tiražeeritud virtuaalmasinates Azure Azure võrku ühendatud. Kui soovite kopeeritud virtuaalmasinates suhelda veenduge, et Azure'i samasse võrku seadmine.
- Kui olete suurust VMware virtuaalse masina või see on tõrkeolukorras kriitiline füüsilise serveris maht. Kui vajate suurust muuta, tehke järgmist.

    - a) suurus sätet muuta.
    - (b) **Virtuaalmasinates** vahekaardil, valige virtuaalse masina ja klõpsake nuppu **Eemalda**.
    - c) **eemaldada virtuaalse masina** valige suvand **Keela kaitse (Kasuta taastamise süvitsi ja helitugevuse suuruse muutmine)**. See suvand keelab kaitse, kuid säilitab taastamise punkte Azure.

        ![Virtuaalne seadme atribuutide seadmine](./media/site-recovery-vmware-to-azure-classic-legacy/remove-vm.png)

    - d) lubage virtuaalse masina kaitse uuesti. Kui te lubage uuesti kaitse andmeid muudetud suurusega maht ei viida Azure.

    

## <a name="step-10-run-a-failover"></a>Samm 10: Käivitada Tõrkesiirde

Praegu saab käivitada ainult planeerimata failovers kaitstud VMware virtuaalmasinates ja füüsilise serveri. Võtke arvesse järgmist.



- Enne, kui annate Tõrkesiirde, tagada konfiguratsiooni ja juhtslaidi target serverites töötab ja terve. Muul juhul Tõrkesiirde nurjub.
- Andmeallika masinad pole sulgeda planeerimata Tõrkesiirde osana. Andmete kopeerimine kaitstud serverite läbimiseks planeerimata Tõrkesiirde lõpetab. Peate masinad kustutada rühma kaitse ja lisage need uuesti alustamiseks kaitsmine masinad uuesti pärast planeerimata Tõrkesiirde on lõpule jõudnud.
- Kui soovite kasutada kaotamata mingeid andmeid, veenduge, et esmane saidi virtuaalmasinates on sisse lülitatud enne, kui annate selle Tõrkesiirde.

1. Lehel **Taastamine lepingud** ja lisage taastamis. Valige **Azure** mis Sihtvaluuta üksikasjad lepingu raames.

    ![Taastamise kava konfigureerimine](./media/site-recovery-vmware-to-azure-classic-legacy/rplan1.png)

2. **Valige virtuaalse masina** valige kaitse rühma ja seejärel valige jaotises lisamiseks taastamise kava masinad. [Lisateavet vt](site-recovery-create-recovery-plans.md) taastamise kohta.

    ![Lisage virtuaalmasinates](./media/site-recovery-vmware-to-azure-classic-legacy/rplan2.png)

3. Kui vaja saate kohandada leping loomiseks rühmad ja jada tellimuse millised masinad taastamise leping on nurjus üle. Saate lisada ka käsitsi toimingute ja skriptide puhul kuvatavaid juhiseid. [Azure'i automaatika tegevusraamatud](site-recovery-runbook-automation.md)abil saab lisada skripte kui Azure taastamisel.

4. Lehe **Taastamine lepingu** valimine leping ja klõpsake nuppu **Planeerimata Tõrkesiirde**.
5. Valige taastamise punkti nurjumise üle, et **Kinnitada Tõrkesiirde** kinnitage Tõrkesiirde suund (Azure).
6. Oodake Tõrkesiirde töö täita ja seejärel veenduge, et selle Tõrkesiirde töötas ootuspäraselt ja tiražeeritud virtuaalmasinates edukalt käivituda Azure.




## <a name="step-11-fail-back-failed-over-machines-from-azure"></a>Samm 11: Fail uuesti üle masinad Azure'i nurjus

[Lisateavet](site-recovery-failback-azure-to-vmware-classic-legacy.md) selle kohta, kuidas viia oma nurjunud üle, kus käitatakse Azure'i tagasi oma kohapealse keskkonna.


## <a name="manage-your-process-servers"></a>Teie protsessi serverite haldamine

Protsessi server saadab dispersioonanalüüs andmete serveri Azure ja leiab uue VMware virtuaalmasinates vCenter serverisse. Järgmistes olukordades võiksite muuta juurutamise protsessi server:

- Kui praegune protsess server läheb alla
- Kui teie ettevõtte jaoks aktsepteeritav tasemele tõusu taastamine punkti eesmärk (RPO).

Vajadusel saate liikuda kopeerimine mõnda või kõiki oma kohapealse VMware virtuaalmasinates ja füüsilise serveri ja muu protsess server. Näiteks:

- **Tõrge**– kui protsess server ei või pole saadaval saate teisaldada kaitstud kohapeal kopeerimine teise protsessi serverisse. Metaandmete allikas seadme ja koopia arvutisse teisaldatakse uus protsess server ja andmed on resynchronized. Uus protsess server automaatselt loob ühenduse Vcenteri serveri automaatse tuvastamise sooritamiseks. Saate jälgida protsessi serverid saidi taastamine armatuurlaual olek.
- **Laadi tasakaalustamiseks RPO reguleerimiseks**– täiustatud laadi tasakaalustamiseks saate valida muu protsess serveri saidi taastamise portaalis, ja nihutage dispersioonanalüüs ühe või mitme masinad selle käsitsi koormusetasakaalustuseks. Sel juhul metaandmete valitud lähte- ja koopia masinad teisaldatakse uus protsess server. Ühendus vCenter server säilib algne protsess server. 

### <a name="monitor-the-process-server"></a>Protsessi serveri jälgimine

Kui protsess server pole hoiatuse olek kuvatakse saidi taastamise armatuurlaual kriitiline olekus. Võite klõpsata olekut ning avage vahekaart sündmused ja seejärel klõpsake vahekaarti teatud tööd allapoole süvitsi. 

### <a name="modify-the-process-server-used-for-replication"></a>Muuta protsess server kasutatakse dispersioonanalüüs

1. Avage **Servers** > **Konfiguratsiooni Servers** > konfiguratsiooniserveri > **Serveri üksikasjad**.
2. Klõpsake **Protsessi serverid** > **Muuta protsessi serveri** kõrval, mida soovite muuta serveri.

    ![Protsessi Server 1 muutmine](./media/site-recovery-vmware-to-azure-classic-legacy/change-ps1.png)

3. **Muuda protsessi**serveris > **Target protsessi Server** valige uus server, mida soovite kasutada, ja seejärel valige virtuaalmasinates, mida soovite paljundada uue server. Lisateavet vaba ruumi ja mällu serveri nime kõrval ikooni teavet. Keskmine ruumi, mis on vaja paljundada iga valitud virtuaalse masina uue protsessi server kuvatakse aidata teil teha laadimine otsuseid.

    ![Protsessi Server 2 muutmine](./media/site-recovery-vmware-to-azure-classic-legacy/change-ps2.png)

4. Klõpsake märkeruutu alustamiseks imitatsiooniga uue protsessi serveriga. Pange tähele, et kõik virtuaalmasinates eemaldamisel protsessi server, mis oli see peaks enam kuva kriitilised hoiatuse armatuurlaual.


## <a name="third-party-software-notices-and-information"></a>Kolmanda osapoole tarkvara teatised ja teave

Ärge tõlkimine või Localize

Tarkvara ja püsivara töötab Microsofti toote või teenuse põhineb või sisaldab materjali projektid, mis on loetletud allpool "(ühiselt kolmanda osapoole kood).  Microsoft on kolmanda osapoole koodiga mitte algse autori.  Algne Autoriõigus ja litsents, millise Microsoft saanud selliste muude tootjate kood, määratakse toodud allpool.

A jaotise teave on seotud kolmandate osapoolte kood komponentide projektid, mis on loetletud allpool. Need litsentsid ja teavet edastatakse illustreerivad.  Kolmanda osapoole järgmine kood on on relicensed teile Microsoft jaotises Microsofti tarkvara litsentsitingimused Microsofti toote või teenuse.  

Teave jaotises b on seotud kolmandate osapoolte kood komponendid, mis tehakse kättesaadavaks teile Microsofti hulgilitsentsimise algse tingimustel.

Terve faili võib leida [Microsofti allalaadimiskeskusest](http://go.microsoft.com/fwlink/?LinkId=529428). Microsoft jätab kõik õigused puudub kirjalik kinnitus, kas, kaudselt estoppel või muul viisil.
