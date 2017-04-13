<properties
    pageTitle="Azure'i paketi funktsioon ülevaade arendajatele | Microsoft Azure'i"
    description="Siit saate teada, funktsioonide paketi teenuse ja selle API arengu vaatevinklist."
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-compute"
    ms.date="09/29/2016"
    ms.author="marsma"/>

# <a name="batch-feature-overview-for-developers"></a>Paketi funktsioon ülevaade arendajatele

See ülevaade põhilisi komponente Azure'i paketi teenuse käsitleme esmane teenuse funktsioonid ja ressursid, millest paketi arendajate abil saate koostada suuremahuliste samal ajal arvutada lahendusi.

Kas olete arendamise jaotatud arvutuslik rakenduse või teenuse, mis annab otsest [REST API] [ batch_rest_api] kõned või te kasutate ühte [Paketi SDK-d](batch-technical-overview.md#batch-development-apis), saate kasutada mitmeid ressursse ja funktsioonid, mida selles artiklis käsitletakse.

> [AZURE.TIP] Paketi teenuse kõrgema taseme tutvustus leiate [Azure'i paketi põhitõed](batch-technical-overview.md).

## <a name="batch-service-workflow"></a>Paketi teenuse töövoo

Järgmine üksikasjalik töövoog on tüüpiline peaaegu kõik rakendused ja teenused, mis kasutavad paketi teenuse töötlemiseks paralleelselt töökoormus.

1. **Andmefailid** , mis on [Azure Storage] töötlemisviisi[ azure_storage] konto. Pakett sisaldab tugi juurdepääs Azure'i bloobimälu ja tööülesannete saate faile alla laadida neid [arvutada sõlmed](#compute-node) käitamisel tööülesanded.

2. Laadige **rakendus failide** tööülesannete töötavad. Need failid võivad olla kahendfaile või skripte ja nende sõltuvusi ja teostavad oma tööd tööülesanded. Tööülesannete saate salvestusruumi kontolt need failid alla või kasutage funktsiooni [rakenduse paketid](#application-packages) partii Rakendusehaldus ja juurutamise jaoks.

3. Looge [rakenduskausta](#pool) Arvuta sõlmed. Kui loote on, saate määrata Arvuta sõlmed pool, nende suurust ja operatsioonisüsteemi arv. Iga tööülesande tööpäevad käitamisel, see on määratud üks sõlmed olevate käivitada.

4. Looge [töö](#job). Töö haldab tööülesannete kogumi. Saate seostada iga töö teatud pool, kus see tööülesannetest käivituvad.

5. Projekti [tööülesannete](#task) lisada. Iga käivitub rakendust või töödelda seda allalaaditavate failide salvestusruumi kontolt andmefailide üleslaaditud skripti. Nagu iga tööülesande lõpule jõudnud, saate selle Azure'i salvestusruumi üles laadida selle väljundi.

6. Töö edenemist jälgida ja tuua tööülesande väljundi Azure Storage.

Järgmistes lõikudes käsitletakse neid ja muid ressursse paketi mis jaotatud arvutuslik stsenaariumist lubamine.

> [AZURE.NOTE] Vajate [paketi konto](batch-account-create-portal.md) paketi teenust kasutada. Peaaegu kõik lahendused kasutada ka, on [Azure Storage] [ azure_storage] konto jaoks faili salvestamine ja otsing. Partii praegu toetab ainult **üldist otstarbe** salvestusruumi konto tüüp, nagu on kirjeldatud [Loo konto salvestusruumi](../storage/storage-create-storage-account.md#create-a-storage-account) [kohta Azure salvestusruumi](../storage/storage-create-storage-account.md)kontodel juhis 5.

## <a name="batch-service-resources"></a>Paketi teenuste ressursid

Mõni järgmistest ressurssidest--kontod, arvutada sõlmed, kaustu, töö ja tööülesannete--on nõutud kõik lahendusi, mis paketi teenust kasutada. Teised, töö ajakava ja rakenduse paketid, nagu on kasulik, kuid ei ole kohustuslik, funktsioonid.

- [Konto](#account)
- [Arvutage sõlm](#compute-node)
- [Rakenduskausta](#pool)
- [Töö](#job)

  - [Töö ajakava](#scheduled-jobs)

- [Ülesanne](#task)

  - [Tööülesande käivitamine](#start-task)
  - [Tööülesanne haldur](#job-manager-task)
  - [Tööülesannete ettevalmistamine ja vabastamine](#job-preparation-and-release-tasks)
  - [Mitme eksemplari tööülesande (MPI)](#multi-instance-tasks)
  - [Ülesandesõltuvused](#task-dependencies)

- [Rakenduse paketid](#application-packages)

## <a name="account"></a>Konto

Paketi konto on kordumatult määratletud üksus paketi teenuses. Kõik töötlemine on paketi kontoga seostatud. Kui teete toiminguid paketi teenuse, peate konto nimi ja selle konto kiirklahvidele. Saate [luua konto Azure'i paketi Azure'i portaalis](batch-account-create-portal.md).

## <a name="compute-node"></a>Arvutage sõlm

Arvuta sõlme on Azure'i virtuaalarvuti (VM) mis eesmärk on jätkuvalt töötlemine rakenduse töökoormus osa. Sõlme suurus määrab CPU südamikud, mälumaht ja kohaliku faili süsteemi suurus, mis on eraldatud sõlm. Te saate luua Windowsi või Linuxi sõlmed Azure'i pilveteenustega või Virtuaalmasinates turuplatsi piltide abil. Teavet järgmisest jaotisest [rakenduskausta](#pool) Lisateavet nende suvandite kohta.

Sõlmed saab käitada mis tahes käivitatava või skripti, mis toetab sõlme operatsioonisüsteemi keskkonnas. See hõlmab \*.exe, \*cmd, \*.bat ja PowerShelli skriptide Windowsi--ja kahendfaile, shell ja Python skripte Linuxi jaoks.

Kõik arvutada sõlmed paketi ka:

- Standardse [kausta struktuuri](#files-and-directories) ja seotud [keskkonna muutujate](#environment-settings-for-tasks) viide ülesannete jaoks saadaolevaid.
- **Tulemüüri** sätted, mis on konfigureeritud juurdepääsu kontrollimiseks.
- [Kaugpöördusteenuse](#connecting-to-compute-nodes) Windows (Kaugtöölaua protokolli (RDP)) ja Linux (Secure Shell (SSH)) sõlmed.

## <a name="pool"></a>Rakenduskausta

On on sõlmi, mis töötab teie taotlus kogum. Kausta loomist käsitsi teie poolt, või automaatselt paketi teenuse kui määrate tööd teha. Saate luua ja hallata olev rakenduse vastab ressursi. On saab kasutada ainult paketi konto on loodud. Paketi konto võib olla rohkem kui ühe kausta.

Azure'i paketi kaustu luua core Azure Arvuta platvormi peal. Nad pakuvad suuremahuliste eraldatud, rakenduse installimine, andmete jaotuse, seisundi jälgimine ja paindlik kohandamine Arvuta sõlmed sees mõnda kausta ([skaleerimist](#scaling-compute-resources)) arvu.

Iga sõlm, mis lisatakse on määratud kordumatu nimi ja IP-aadress. Sõlm eemaldamisel on operatsioonisüsteem või failide tehtud muudatused lähevad kaotsi ja edaspidiseks kasutamiseks on välja nime ja IP-aadress. Sõlm lahkumisel on tööiga on üle.

Kui loote on, saate määrata järgmisi atribuute:

- Arvutage sõlm **operatsioonisüsteem** ja **versioon**

    Kui valite operatsioonisüsteemi sõlmed olevate on kaks võimalust: **Virtuaalse masina konfiguratsiooni** ja **Cloud Services konfigureerimine**.

    **Virtuaalne arvuti konfiguratsiooni** pakub Windows ja Linux piltide arvutada sõlmed [Azure'i Virtuaalmasinates turuplatsi][vm_marketplace].
    Kui loote kausta, mis sisaldab virtuaalse masina konfiguratsiooni sõlmed, määrake mitte ainult sõlmed, ka **virtuaalse masina pilt viide** ja paketi **sõlm agent SKU** olema installitud sõlmed suurust. Need kausta atribuutide kohta leiate lisateavet teemast [sätte Linux arvutada sõlmed Azure'i paketi kaustadesse](batch-linux-nodes.md).

    **Cloud Services konfiguratsiooni** pakub Windows arvutada sõlmed *ainult*. [Azure'i Külastajate OS väljalasete ja SDK ühilduvus maatriks](../cloud-services/cloud-services-guestos-update-matrix.md)on loetletud saadaolevate operatsioonisüsteemide jaoks Cloud Services konfiguratsiooni kaustu. Kui soovite luua kausta, mis sisaldab pilveteenustega sõlmed, peate määrama sõlme suurusest ja selle *OS pere*. Kui loote kaustu Windowsi arvutada sõlmed, kasutate kõige sagedamini pilveteenustega.

    - *OS pere* siinse, millised versioonid .NET installitakse OS.
    - Kui töötaja rolle: pilveteenuste, saate määrata, kas mõni *Opsüsteemi versioon* (töötaja rolli kohta leiate lisateavet jaotisest [Juhata mind kohta pilveteenustega](../cloud-services/cloud-services-choose-me.md#tell-me-about-cloud-services) [pilveteenustega ülevaade](../cloud-services/cloud-services-choose-me.md)).
    - Kui töötaja rollid, soovitame, et määrata `*` jaoks *Opsüsteemi versioon* nii, et sõlmed automaatselt uuendada ning poleks vaja sobi äsja tööd välja versioone. Peamine kasutus puhul valida kindla Opsüsteemi versioon on rakenduse ühilduvust, mis võimaldab testimine enne lubamisel versiooni värskendamiseks tagasiühilduvuse tagamiseks. Pärast valideerimist, saab värskendada pool *Opsüsteemi versioon* ja uus OS pilt võib olla installitud--töötava toiminguteks on katkenud ja liigutas selle paigast.

- **Sõlmed suurus**

    **Cloud Services konfiguratsiooni** Arvuta sõlm suurused on loetletud [suurused pilveteenustega](../cloud-services/cloud-services-sizes-specs.md). Paketi toetab pilveteenustega igas suuruses, välja arvatud `ExtraSmall`.

    **Virtuaalne arvuti konfiguratsiooni** Arvutage sõlm suurused on loetletud [Azure'i virtuaalmasinates suurused](../virtual-machines/virtual-machines-linux-sizes.md) (Linux) ja [suurused Azure'i virtuaalmasinates](../virtual-machines/virtual-machines-windows-sizes.md) (Windows). Paketi toetab Azure VM igas suuruses, välja arvatud `STANDARD_A0` ja premium salvestusruumi (`STANDARD_GS`, `STANDARD_DS`, ja `STANDARD_DSV2` sarja).

    Arvuta sõlm suurus valimisel arvesse võtta nõuetele rakendusi käivitate sõlmed ja omadused. Aspekte, näiteks kas rakendus on multithreaded ja kui palju see tarbib mälu aitab määratleda kõige sobiva ja kulutõhusamal sõlm suurus. Valige soovitud sõlm suurus eeldades ühe tööülesande käivitada sõlme ajal tavaliselt kasutatakse. Kuid see on võimalik on mitu ülesannet (ja seega mitu rakenduse eksemplari) [paralleelselt](batch-parallel-node-tasks.md) arvutada sõlmed töö käitamisel. Sel juhul on levinud valige sõlm suuremaks paralleelselt tööülesande täitmise suurenemine mahutamiseks. Lisateabe saamiseks vaadake [ülesande ajastamine poliitika](#task-scheduling-policy) .

    Kõik sõlmed pargis on sama suurus. Kui kavatsete käivitada rakendusi, mis erinevad süsteeminõuded ja/või laadimine taset, soovitame kasutada eraldi kaustu.

- **Sihtrühma sõlmed arv**

    See on Arvuta sõlmed kogumi juurutamiseks soovitud arv. Seda nimetatakse *target* Kuna mõnel juhul võib teie kausta ei jõua sõlmed soovitud arv. On võimalik ei jõua soovitud arvu sõlmed see jõuab [core kvoodi](batch-quota-limit.md#batch-account-quotas) paketi konto - või automaatne skaleerimist valemit, mille olete kogumi suurima arvu sõlmed (vt järgmist jaotist "Skaleerimist poliitika").

- **Skaleerimise poliitika**

    Lisaks staatilise arvu sõlmed määramisele saate selle asemel kirjutamine ja rakendada mõnda [Automaatne skaleerimist valem](#scaling-compute-resources) on. Paketi teenuse perioodiliselt hindab valem ja reguleerib sõlmed maksimaalselt pool pool, töö ja saate määrata ülesande parameetrite arvu.

- **Ülesande ajastamine poliitika**

    [Max ülesannete kohta sõlm](batch-parallel-node-tasks.md) konfigureerimist määratleb tööülesandeid, mida saab kasutada paralleelselt iga MSDN maksimaalselt pool arvu ülempiir.

    Vaikimisi konfiguratsioon on ühe tööülesande korraga töötab sõlme, et on stsenaariumid, kus on kasulik on rohkem kui ühe tööülesande täide sõlm korraga. [Näiteks sel juhul](batch-parallel-node-tasks.md#example-scenario) näha, kuidas saate kasutada mitut ülesannete kohta sõlm [samaaegseid sõlm tööülesannete](batch-parallel-node-tasks.md) artikli teemast.

    Saate määrata ka *täide tüüp* , mis määrab, kas paketi ulatub tööülesanded ühtlaselt üle kõik sõlmed pargis või keelepakettide enne tööülesande määramine teise sõlme iga sõlme tööülesanded maksimaalne arv.

- Arvuta sõlmed **side olek**

    Enamikul juhtudel ülesanded iseseisvalt ja suhtlevad pole vaja. Kuid on mõned rakendused, kus tööülesanded tuleb suhelda, nagu [MPI stsenaariumid](batch-mpi.md).

    Saate konfigureerida rakenduskausta, sõlmed sees it -**ülemist sõlmevahet side**vahelise suhtluse lubamiseks. Kui ülemist sõlmevahet side on lubatud, sõlmed Cloud Services konfiguratsiooni kaustadesse saate omavahel on suurem kui 1100 pordid ja virtuaalse masina konfiguratsiooni kaustu ei piira liikluse mis tahes porti.

    Pange tähele, et ülemist sõlmevahet side võimaldamiseks ka mõjutab paigutuse sõlmed sees kogumite võib piirata sõlmed pargis maksimumarv juurutamise piirangute tõttu. Kui teie taotlus ei vaja suhtlemine sõlmed, paketi teenuse saab eraldada potentsiaalselt suure hulga sõlmed pool kaudu paljude erinevate kogumite ja andmekeskuste lubamiseks suurendada paralleelne töötlemine power.

- Arvuta sõlmed **tööülesande käivitamine**

    Valikuline *käivitamine tööülesande* aktiveeritakse iga sõlme sõlme ühendab pool ja iga kord, kui sõlm on uuesti või reimaged. Alusta tööülesanne on eriti kasulik ülesannete, nt tööülesannete Arvuta sõlmed töötavad rakenduste installimine täitmise Arvuta sõlmed ettevalmistamine.

- **Rakenduse paketid**

    Saate määrata [rakenduse paketid](#application-packages) juurutada Arvuta sõlmed kogumi. Rakenduse paketid pakuvad lihtsustatud juurutamise ja tööülesannete töötavad rakenduse Versioonimine. Rakenduse paketid on määratud installitakse iga sõlme, mis ühendab selle pool ja iga kord, kui sõlm on rebooted või reimaged. Rakenduse paketid pole praegu toetatud Linux Arvuta sõlmed.

- **Võrgu konfigureerimise**

    Saate määrata, luuakse Azure [virtuaalse võrgu (VNet)](../virtual-network/virtual-networks-overview.md) , kus on pool arvutada sõlmed ID-d. Nõuded on VNet täpsustades jaoks oma pool leiate [Lisa konto lisamine pool] [ vnet] paketi REST API viite.

> [AZURE.IMPORTANT] Paketi kõik kontod on vaikimisi **piirmäära** , mis piirab **tuuma** (ja seega Arvuta sõlmed) arvu paketi konto. Leiate [kvootide](batch-quota-limit.md)ja limiidid Azure'i paketi teenuse vaikimisi kvootide ja juhised, kuidas [suurendada kvoodi](batch-quota-limit.md#increase-a-quota) (nt kontol paketi valdkond maksimum). Kui te ei leia endale küsida "Miks ei minu pool saavutamiseks rohkem kui X sõlmed?" selle piirmäära core võib selle põhjuseks olla.

## <a name="job"></a>Töö

Töö on tööülesannete kogumi. See õnnestub, kuidas toimub arvutus ülesannete Arvuta sõlmed pargis.

- Töö määrab **pool** , kus on töö käivitada. Saate luua uue kausta iga töö või ühe kausta paljude projektide jaoks kasutada. Saate luua kausta, iga töö, mis on seotud töö ajakava või kõik tööd, mis on seotud töö ajakava.

- Saate määrata, on valikuline **töö**. Kui tööd on esitatud koos kõrgem prioriteet, kui tööd, mis on parajasti käimas, lisatakse tööülesanded-prioriteet töö ees tööülesannete alumises tähtsusega tööde järjekorda. Tööülesannete alumises tähtsusega tööd, mis on juba on ei preempted.

- Töö **piiranguid** abil saate määrata oma töö jaoks teatud piirangud:

    Saate seada **wallclock ülempiir kellaaeg**nii, et kui töö kestab kauem kui määratud on maksimaalne wallclock, töö ja kõik selle ülesanded on lõpetatud.

    Paketi saate tuvastada ja proovige uuesti nurjunud tööülesanded. Saate määrata **ülesande korduskatsed maksimumarv** nimega piirangu, sh kas tööülesande on *alati* või *mitte kunagi* uuesti proovida. Tööülesande uuesti proovimine tähendab, et tööülesanne on liigutas selle paigast, uuesti käivitada.

- Klientrakenduse saate ülesanded lisada töö või saate määrata [tööülesandega haldur](#job-manager-task). Töö halduri tööülesande sisaldab teavet, mida on vaja luua ülesande töökohta, käivitatud ühele Arvuta sõlmed pool töö halduri ülesanne. Tööülesanne halduri käsitletakse konkreetselt, paketi--on järjekorda, kohe pärast töö loomist ja taaskäivitamist ei ole. Töö halduri ülesande jaoks on *vaja* tööd, mis on loodud [töögraafik](#scheduled-jobs) , sest see on ainus võimalus tööülesannete määratlemine enne töö on käivitatud.

- Vaikimisi töö jäävad aktiivse oleku siis, kui kõik tööülesanded töö on valmis. Seda käitumist saate muuta nii, et töö lõpetatakse automaatselt, kui kõik tööülesanded töö on valmis. Projekti **onAllTasksComplete** atribuudi ([OnAllTasksComplete] [ net_onalltaskscomplete] paketi .NET-is) abil *terminatejob* automaatselt töö lõpetada, kui kõik ülesannete lõplikus olekus.

    Pange tähele, et paketi teenuse leiab kõik ülesannete lõpule *pole* ülesannetega töö. Seetõttu see suvand on kõige sagedamini kasutatav [halduri tööülesanne](#job-manager-task). Kui soovite kasutada automaatvarunduse lõpetamise ilma töö halduri, tuleks algselt määratud uus töökoht **onAllTasksComplete** atribuudi *noaction*, siis määratud *terminatejob* ainult siis, kui olete lõpetanud, tööülesannete lisamine projekti.

### <a name="job-priority"></a>Töö Priority (prioriteet)

Saate määrata tööd, mis loote töölehel prioriteet. Paketi teenuse kasutab Priority (prioriteet) väärtust töö määratlemiseks järjestuse töö planeerimine jooksul konto (see on mitte saada segi [ajastatud töö](#scheduled-jobs)). Priority (prioriteet) väärtusi vahemikus-1000-1000-1000 on madalaim prioriteet ja 1000 on kõrgeim. [Töö atribuutide värskendamine] abil saate värskendada prioriteet töö[ rest_update_job] toiming (paketi REST) või muutes [CloudJob.Priority] [ net_cloudjob_priority] atribuudi (paketi .net-i).

Sama konto on kõrgem prioriteet töö ajastamise üle alumises tähtsusega töö järjestuse. Töö ühe konto-prioriteet väärtusega ei ole järjestuse ajastamise üle teise töö alumises tähtsusega väärtusega mõne muu kontoga sisse.

Töö ajastamise üle kaustu ei sõltu. Vahel eri kaustu, ei ole tagatud, et kõrgem prioriteet töö on esmalt planeeritud, kui selle seotud pool on puudu jõude sõlmed. Sisse sama töö prioriteet samal tasemel on võrdne võimalus on ajastatud.

### <a name="scheduled-jobs"></a>Ajastatud tööd

[Töö ajakava] [ rest_job_schedules] võimaldavad teil luua korduva tööde haldamine teenuses paketi. Töö ajakava määrab, millal tööde ja see sisaldab projektide käivitamise kirjeldused. Saate määrata ajakava--kaua ja kui graafik on tegelikult--ja sageduse ajavahemikus selle loodava töö kestus.

## <a name="task"></a>Ülesanne

Tööülesande on ühiku arvutus, mis on seotud tööd. See töötab sõlm. Tööülesanded määratakse sõlm täitmiseks või järjekorda kuni sõlm muutub tasuta. Lühidalt öeldes tööülesande töötab ühe või mitme programmi või skriptide MSDN tehtud töö, peate täitma.

Kui soovite luua tööülesande, saate määrata:

- **Käsurea** tööülesanne. See on käsurea, mis töötab teie rakenduse või skripti Arvuta sõlme.

    See on oluline Pange tähele, et käsurea töötama tegelikult shell. Seetõttu ei saa algupäraselt kuluda shell funktsioone nagu [Muutuja](#environment-settings-for-tasks) laiendamine (see hõlmab soovitud `PATH`). Ära selliseid funktsioone, peab kutsute shell käsureal – näiteks käivitades `cmd.exe` Windows sõlmed või `/bin/sh` Linux:

    `cmd /c MyTaskApplication.exe %MY_ENV_VAR%`

    `/bin/sh -c MyTaskApplication $MY_ENV_VAR`

    Kui tööülesannete peate käivitama rakenduse või skripti, mis pole selle sõlme `PATH` või viidata keskkonna muutujate, autonoomsest shell otseselt, klõpsake tööülesande käsurea.

- **Ressursi failid** , mis sisaldavad andmeid töödeldakse. Need failid kopeeritakse automaatselt sõlme bloobimälu **Üldine otstarve** Azure Storage konto kaudu enne tööülesande käsurea on täidetud. Lisateabe saamiseks lõigud [tööülesande käivitamine](#start-task) ja [failide ja kataloogide](#files-and-directories).

- **Keskkonna muutujad** , mis on nõutud rakenduse. Lisateavet leiate jaotisest [keskkonna sätteid ülesannete jaoks](#environment-settings-for-tasks) .

- **Piiranguid** millise tööülesande tuleks käivitada. Suurim lubatud aeg, et tööülesanne on lubatud käivitamiseks on mitu korda nurjunud tööülesande tuleb uuesti proovida, näiteks ja maksimaalne aega selle ülesande töötavat kausta failide säilitatakse.

- **Rakenduse paketid** juurutada Arvuta sõlme tööülesande plaanitakse käivitada. [Rakenduse paketid](#application-packages) pakuvad lihtsustatud juurutamise ja tööülesannete töötavad rakenduse Versioonimine. Rakenduse tööülesande tasemel paketid on eriti kasulik ühiskasutusse antud kausta keskkonnas, kus eri töid käitatakse ühe kausta ja kausta ei kustutata, kui tööd on lõpule viidud. Kui teie töö on vähem kui sõlmed tööülesannete kogumi, saate tööülesande rakenduse paketid minimeerida andmete edastamiseks Kuna rakenduse juurutatakse sõlmed, mis töötavad tööülesannete.

Lisaks määratlete ülesannete täitmiseks arvutus sõlme, teisiti järgmist on olemas paketi teenus võttis:

- [Tööülesande käivitamine](#start-task)
- [Tööülesanne haldur](#job-manager-task)
- [Tööülesannete ettevalmistamine ja vabastamine](#job-preparation-and-release-tasks)
- [Mitme eksemplari tööülesannete (MPI)](#multi-instance-tasks)
- [Ülesandesõltuvused](#task-dependencies)

### <a name="start-task"></a>Tööülesande käivitamine

**Tööülesande käivitamine** seostada koos on saate koostada oma sõlmed töökeskkonna. Näiteks saate näiteks tööülesannete töötavad rakenduste installimine ja käivitamine taustaprotsessid toiminguid teha. Alusta käivitub iga kord, kui sõlm käivitub, seni, kuni see pool – ka siis, kui sõlm on esimene lisada pool ja kui see on uuesti või reimaged.

Start tööülesande esmane kasu, on see, et võib sisaldada kogu teavet, mida on vaja konfigureerida Arvuta sõlme ja tööülesannete täitmiseks vajalike rakenduste installimine. Seetõttu sõlmed pargis arvu on sama lihtne kui soovite määrata uue sihtrakenduse sõlm count--paketi juba on teavet, mida on vaja konfigureerida uusi sõlmi ja nende ettevalmistamine vastuvõtmise tööülesanded.

Nagu mis tahes Azure'i paketi tööülesande, saate määrata **ressursside failide** loendi [Azure]Storage[azure_storage], lisaks **käsurea** käivituma. Paketi esmalt kopeeritakse Azure Storage ressurss faile sõlm ja käivitatakse käsurea. Rakenduskausta start tööülesande, failide loend sisaldab tavaliselt tööülesande taotlus ja sõltuvustega.

Siiski, selle võib hõlmata ka lähteandmed kõik toimingud, mis töötavad Arvuta sõlme kasutavad. Näiteks, start tööülesande käsurea sooritada on `robocopy` kopeerida rakenduse (mida määratud ressurss faile ja alla laaditud sõlm) start tööülesande [töö kataloog](#files-and-directories) [ühiskausta](#files-and-directories)ja käivitage MSI toimingu või `setup.exe`.

> [AZURE.IMPORTANT] Partii praegu toetab *ainult* **üldist otstarbe** salvestusruumi konto tüüp, nagu on kirjeldatud [Loo konto salvestusruumi](../storage/storage-create-storage-account.md#create-a-storage-account) [kohta Azure salvestusruumi](../storage/storage-create-storage-account.md)kontodel juhis 5. Tööülesannete pakett (sh standardsete, start tööülesandeid, töö ettevalmistustööd ja tööülesannete väljaanne), peate määrama ressursi faile, mis asuvad *ainult* **üldist otstarbe** salvestusruumi kontodel.

On tavaliselt soovitav ootama start tööülesande lõpuleviimiseks enne valmis olema määratud tööülesannete sõlm teenuse paketi, kuid saate selle konfigureerida.

Alusta tööülesande nurjumisel Arvuta sõlme seejärel sõlme olek värskendatakse vastavalt selle ja sõlme on määratud kõik tööülesanded. Alusta tööülesande võib nurjuda, kui seal on kopeerimine selle ressursi failide salvestusruumi, või kui sooritatud toimingu selle käsurea tagastab väljumise nullist erinev kood.

Kui lisate või värskendada start ülesande jaoks mõne *olemasoleva* kausta, peate selle Arvuta sõlmed start tööülesande rakendataks sõlmi taaskäivitama.

### <a name="job-manager-task"></a>Tööülesanne haldur

Tavaliselt kasutate **halduri tööülesanne** juhtelemendile ja/või töö täitmise – näiteks luua ja esitada töökohta tööülesannete jälgimine, kindlaks teha lisatoiminguid käivitamiseks ja kindlaks teha, kui töö on valmis. Tööülesande töö haldur pole piiratud nende tegevustega. See on valmis tööülesande, mida saate mis tahes töö vajalike toiminguid teha. Näiteks võib töö halduri tööülesande alla laadida faili, mis on määratud parameetrina, selle faili sisu analüüsimiseks ja esitage nende sisu põhjal lisatoiminguid.

Töö halduri tööülesande alustamist enne kõiki muid toiminguid. See pakub järgmisi funktsioone:

- See automaatselt esitatakse tööülesandena paketi teenus võttis töö loomisel.

- See on kavandatud käivitada enne projekti tööülesanded.

- Selle seotud sõlm on viimasena, eemaldatakse kaustast pool on on kärbitud.

- Selle saate seotud kõik tööülesanded töö lõpetamine.

- Töö halduri tööülesande antakse kõrgeim eelistus, kui see tuleb taaskäivitada. Kui mõni jõude sõlm pole saadaval, võib paketi teenuse muude töötava tööülesannete kogumi teha ruumi töö halduri toimingu käivitada lõpetada.

- Töö halduri tööülesande ühe töö ei ole muud tööd ülesannete prioriteet. Kogu töö, ainult töö taseme prioriteedid on kinni.

### <a name="job-preparation-and-release-tasks"></a>Tööülesannete ettevalmistamine ja vabastamine

Paketi pakub töö ettevalmistustööd eelse töö täitmise häälestus. Väljalaske tööülesanded on pärast töö või Kettapuhastus.

- **Tööülesanne ettevalmistamine**: töö ettevalmistamiseks käivitub kõik Arvuta sõlmed, mis on ajastatud tööülesanded enne mis tahes muud töö tööülesanded on täidetud. Saate kasutada ettevalmistamine tööülesanne andmete kopeerimine, mis on jagatud kõik tööülesanded, kuid on kordumatu töö, näiteks.
- **Tööülesanne väljaanne**: kui töö on lõpule viidud, töö väljaanne käivitub iga sõlme kogumi täidetud vähemalt üks tööülesanne. Abil saate töö väljaanne tööülesande tööülesanne ettevalmistamine kopeeritud andmed kustutada või tihendamine ja Diagnostikaandmete logisse näiteks üles laadida.

Nii töö ettevalmistamine ja väljaanne tööülesannete võimaldab teil määrata käsurea tööülesande kutsumisel käivitamiseks. Pakume funktsioone, nagu faili alla laadida, administraatoriõigustes täitmise, kohandatud keskkonna muutujate maksimaalne täitmise kestus, proovige count ja faili säilitamise aeg.

Ettevalmistamine ja väljaanne tööülesannete kohta leiate lisateavet teemast [tööülesannete ettevalmistamine ja lõpetamise Azure'i töölehe arvutada sõlmed käivitada](batch-job-prep-release.md).

### <a name="multi-instance-task"></a>Mitme eksemplari ülesanne

[Mitme eksemplari tööülesanne](batch-mpi.md) on tööülesande, mis on konfigureeritud käivitamiseks rohkem kui üks MSDN korraga. Mitme eksemplari ülesanded, saate lubada suure jõudlusega arvuti stsenaariumi, mis nõuavad rühma Arvuta sõlmed, mis on eraldatud koos töödelda ühe töökoormus (nt sõnumi läbides kasutajaliidese (MPI)).

Üksikasjalik arutelu MPI tööde käitamist partii teegi paketi .net-i abil, tutvuge [kasutamine mitme eksemplari tööülesannete sõnumi läbides kasutajaliidese (MPI) rakenduste Azure'i paketi käivitamiseks](batch-mpi.md).

### <a name="task-dependencies"></a>Ülesandesõltuvused

[Ülesandesõltuvused](batch-task-dependencies.md), nagu nimi viitab, abil saate määrata, et täitmise enne muude toimingute lõpetamist sõltub tööülesande. See funktsioon toetab olukordades, kus tarbib "allapoole suunatud" ülesanne "ülespoole" toimingu--või kui varustava toimingu sooritab mõned lähtestamine, mida on vaja järgmise etapi tööülesande väljund. Selle funktsiooni kasutamiseks tuleb esmalt lubada ülesandesõltuvused oma pakett-töö. Klõpsake iga tööülesande sõltub teise (või paljud teised), teie määratud tööülesandeid, mis sõltub selle ülesande.

Ülesandesõltuvused, kus saate konfigureerida stsenaariumid umbes selline:

* *taskB* sõltub *taskA* (*taskB* hakatakse täitmise kuni *taskA* on lõppenud).
* *taskC* sõltub nii *taskA* ja *taskB*.
* *taskD* sõltub erinevaid toiminguid, näiteks tööülesannete *1* kuni *10*, enne kui see käivitab.

Lugege teemat [Azure partii ülesandesõltuvused](batch-task-dependencies.md) ja [TaskDependencies] [ github_sample_taskdeps] [azure-paketi-näidised] proovi kood[ github_samples] GitHub hoidla põhjalikumat Lisateavet selle funktsiooni kohta.

## <a name="environment-settings-for-tasks"></a>Keskkonna sätteid toimingute tegemiseks.

Iga tööülesande sooritatud paketi teenus on juurdepääs keskkonna muutujate, mis seda määrab Arvuta sõlmed. See hõlmab keskkonna muutujate määratletud paketi teenuse ([teenuse määratletud][msdn_env_vars]) ja ülesannete jaoks saate määratleda kohandatud keskkonna muutujate. Rakenduste ja skriptide ülesannete täitmiseks juurdepääs nende keskkonna muutujate käitamise ajal.

Saate määrata kohandatud keskkonna muutujate tasemel ülesanne või töö, asustades atribuudi *keskkonna sätteid* järgmiste üksuste puhul. Näiteks vaadata [lisamine tööülesandele töö] [ rest_add_task] toiming (paketi REST API) või [CloudTask.EnvironmentSettings] [ net_cloudtask_env] ja [CloudJob.CommonEnvironmentSettings] [ net_job_env] paketi .NET atribuudid.

Oma kliendi rakenduse või teenuse, saate hankida on ülesande keskkonna muutujate nii teenuse määratletud ja kohandatud, kasutades [tööülesande kohta teabe saamiseks] [ rest_get_task_info] toiming (paketi REST) või kasutades [CloudTask.EnvironmentSettings] [ net_cloudtask_env] atribuudi (paketi .net-i). Arvuta sõlme käivitamisel protsessid pääseks juurde nende ja teiste keskkonna muutujate sõlme, näiteks tuttav `%VARIABLE_NAME%` (Windows) või `$VARIABLE_NAME` (Linux) süntaks.

Kõigi teenuste määratletud keskkonna muutujate täieliku loendi leiate [arvutada sõlm keskkonna muutujate][msdn_env_vars].

## <a name="files-and-directories"></a>Failide ja kataloogide

Iga tööülesande on on *töö kataloog* millise loob null või rohkem failid ja kataloogid. See töötamise kataloog saab kasutada programmi, mida käitatakse tööülesanne, töötleb andmeid ja väljundi tehtav töötlemise talletamiseks. Kõik failid ja kataloogide ülesande kuuluvad tööülesande kasutaja.

Paketi teenuse seab *juurkaust*osa sõlme failisüsteemis. Tööülesannete pääsete juurde selle juurkaust viitamine on `AZ_BATCH_NODE_ROOT_DIR` keskkonnas muutujana. Keskkonna muutujate kasutamise kohta leiate lisateavet teemast [tööülesannete keskkonna sätted](#environment-settings-for-tasks).

Juurkaustas sisaldab kataloogi järgmine struktuur:

![Arvutage sõlm register struktuur][1]

- **ühiskasutusega**: see kaust pakub lugemis-ja kirjutamisõigusega juurdepääsu *Kõik* toimingud, mis töötavad sõlm. Kõik toimingud, mida käitatakse sõlme saate loomine, lugemine, värskendamine ja kustutamine faile selles kaustas. Tööülesannete pääsete juurde selle kataloogi viitamine on `AZ_BATCH_NODE_SHARED_DIR` muutuja.

- **käivitus**: selle kataloogi kasutatakse start ülesande selle töötavat kausta. Kõik start ülesande sõlm laaditud failid on salvestatud siin. Start tööülesande saate loomine, lugemine, värskendamine ja jaotises selle kausta failide kustutamine. Tööülesannete pääsete juurde selle kataloogi viitamine on `AZ_BATCH_NODE_STARTUP_DIR` muutuja.

- **Tööülesanded**: kataloog luuakse iga tööülesande, mida käitatakse sõlme. Juurdepääsul viitamine on `AZ_BATCH_TASK_DIR` keskkonnas muutujana.

    Iga tööülesande Directory, paketi teenus loob töötavat kausta (`wd`) nime, kelle kordumatu tee on määratud soovitud `AZ_BATCH_TASK_WORKING_DIR` keskkonnas muutujana. See kaust võimaldab lugemis-ja kirjutamisõigusega tööülesanne. Tööülesande saate loomine, lugemine, värskendamine ja jaotises selle kausta failide kustutamine. Selle kataloogi alles *RetentionTime* piirang, mis on määratud tööülesande põhjal.

    `stdout.txt`ja `stderr.txt`: need failid kirjutada ülesanne kausta tööülesande täitmise ajal.

>[AZURE.IMPORTANT] Kui sõlm eemaldatakse kausta, eemaldatakse *Kõik* failid, mis on talletatud sõlme.

## <a name="application-packages"></a>Rakenduse paketid

Funktsiooni [rakenduse paketid](batch-application-packages.md) pakub lihtne haldus ja rakenduste Arvuta sõlmi oma kaustadesse. Saate üles laadida ja hallata mitut versiooni rakendusi käivitada, tööülesannete, sealhulgas nende kahendfaile ja faile. Seejärel saate automaatselt juurutada ühte või mitut need rakendused Arvuta sõlmed olevate.

Saate määrata rakenduse paketid rakenduskausta ja tööülesande tasemel. Kui määrate rakenduskausta rakenduse paketid, rakendus on juurutatud igal pool sõlm. Kui määrate tööülesande rakenduse paketid, rakendus on juurutatud ainult sõlmed, mis on ajastatud vähemalt üks just enne tööülesande käsurea käitatakse projekti tööülesanded.

Paketi tegeleb töötamine Azure Storage salvestada oma rakenduse paketid ja juurutada neid arvutada sõlmed, nii saab lihtsustada koodi ja haldus pea kohal üksikasju.

Lisateave rakenduse paketi funktsioon, tutvuge [rakenduse juurutamine Azure'i paketi rakenduse paketid](batch-application-packages.md).

>[AZURE.NOTE] Kui lisate mõne *olemasoleva* kausta rakenduskausta rakenduse paketid, peate selle Arvuta sõlmed jaoks rakenduse paketid sõlmi kasutusele võtta taaskäivitama.

## <a name="pool-and-compute-node-lifetime"></a>Rakenduskausta ja Arvuta sõlm eluiga

Kui loote oma Azure'i paketi lahenduse, peate kujundus otsust ja selle kohta, kuidas teha, kui luuakse ja kaua arvutada sõlmed sees neid kaustu kättesaadavad.

Käed ühes otsas, saate koostada andmebaas iga töö, mida te ja kustutada kausta kohe valmis ülesannete täitmise. See maksimeerib kasutamist, kuna sõlmed on eraldatud ainult siis, kui vaja ja sulgeda, kui need on jõude. See tähendab, et töö peate ootama sõlmed eraldatud, on oluline märkida tööülesannete ajastatud täitmise niipea, kui sõlmed on eraldi saadaval, eraldatud, Alusta tööülesanne on lõpule. Kas pakett *ootama, kuni kõik sõlmed sees on enne tööülesande määramine sõlmed on saadaval* . See tagab kõik saadaolevad sõlmed maksimaalne kasutamine.

Teises otsas käed, kui kohe alustada tööd on kõrgeim eelistus, saate koostada andmebaas ette valmistada ja selle sõlmed kättesaadavaks enne tööde haldamine. Selle stsenaariumi korral tööülesanded saate kohe alustada, kuid sõlmed võib istuda jõude ooteajal neid määratakse.

Kombineeritud lähenemine on tavaliselt kasutatakse töötlemise muutuv, kuid poolelioleva, laadi. Saate määrata olev mitme töö esitatakse, kuid saate mastaapimiseks arvu vastavalt töö sõlmed üles või alla laadida (vt järgmises jaotises [mastaapimine arvutada ressursid](#scaling-compute-resources) ). Saate teha seda tagantjärele, vastavalt praeguse laadimisel või aktiivselt, kui laadi saab prognoosida.

## <a name="scaling-compute-resources"></a>Arvuta ressursse mastaapimist

[Automaatset mastaapimist](batch-automatic-scaling.md), peate paketi teenuse dünaamiliselt kohandada vastavalt praeguse töökoormus ja ressursside kasutamist stsenaariumist Arvuta pargis Arvuta sõlmed. See võimaldab teil vähendada üldine töötab rakenduse abil ainult ressursse, peate, isikud ja te ei pea.

Lubate automaatse skaleerimist teguriga mõnda [automaatse skaleerimise valemi](batch-automatic-scaling.md#automatic-scaling-formulas) kirjutamist ja see valem on seostada. Paketi teenuse kasutab valemi määramiseks sihtrühma arv sõlmed pool järgmise skaleerimise intervalli (intervall, mille saate konfigureerida). On automaatne skaleerimise sätted saate määrata, kui selle loote, või lubada mastaapimine on hiljem. Saate värskendada ka skaleerimist lubatud rakenduskausta skaleerimise sätted.

Näiteks võib-olla töö nõuab teil esitada väga suure hulga ülesannete täitmiseks. Saate määrata skaleerimise valemi kogumi reguleerib sõlmed kogumi alusel praeguse arv ootel tööülesanded ja tööülesanded töö lõpetamise määr arv. Paketi teenuse perioodiliselt hindab valem ja suurust muuta rakenduskausta, võttes aluseks töökoormus (sõlmed palju ootel tööülesannete lisamine ja eemaldamine sõlmed ootel või esitatava toimingute tegemiseks) ja oma valemi sätted.

Skaleerimise valemis saate järgmised mõõdikute põhjal:

- **Aja mõõdikute** põhinevad statistika kogutud iga viie minuti määratud arvu tundi.

- **Ressursi mõõdikute** põhinevad CPU kasutus, läbilaskevõime kasutuse, mälukasutust ja sõlmed arv.

- **Tööülesande mõõdikute** põhinevad tööülesande oleku, nt *Active* (ootele), *töötab*või *lõppenud*.

Kui automaatset mastaapimist Arvuta sõlmed pargis arv väheneb, peate arvesse võtma reageerimine ülesandeid, mis töötavad vähendamine toimingu ajal. Selleks, et paketi pakub *sõlm deallocation suvand* valemitest sisaldavad. Näiteks saate määrata, et töötava tööülesanded on peatatud kohe, kohe lõpetada ja seejärel liigutas selle paigast teise sõlme täitmiseks või enne sõlme eemaldatakse pool lubatud.

Automaatselt skaleerimist rakenduse kohta leiate lisateavet teemast [automaatselt skaala arvutada sõlmed on Azure'i paketi kogumi](batch-automatic-scaling.md).

> [AZURE.TIP] Arvuta ressursi kasutamine maksimeerimiseks numbri saate määrata target sõlmed töö lõpus on null, kuid lubada toimingute lõpuleviimiseks.

## <a name="security-with-certificates"></a>Turvalisus ja serdid

Tavaliselt peate kasutama serdid krüptida või dekrüptida tundlikku teavet toimingute tegemiseks, nagu [Azure Storage konto]võti[azure_storage]. Seda toetab, saate installida sõlmed serdid. Krüptitud saladusi on möödas nupule tööülesanded kaudu käsurea parameetrid või manustatud üks tööülesanne ressursse ja dekrüptida neid saab kasutada installitud serdid.

[Serdi lisamine] kasutada[ rest_add_cert] toiming (paketi REST) või [CertificateOperations.CreateCertificate] [ net_create_cert] meetod (paketi .NET) paketi konto lisamiseks sert. Seejärel saate seostada serdi uude või olemasolevasse ühendada. Kui sert on seotud on, installib paketi teenuse sertifikaadi iga sõlme kogumi. Paketi teenuse installib sertide käivitumisel sõlme enne käivitamist toiminguteks (sh start tööülesande ja tööülesanne manager).

Kui lisate mõne *olemasoleva* kausta serdid, peate selle Arvuta sõlmed sertide peaks rakenduma sõlmed taaskäivitama.

## <a name="error-handling"></a>Tõrge töötlemine

Teil võib vajalikuks toime teie paketi lahendus sees nii tööülesannete kui ka rakenduse tõrked.

### <a name="task-failure-handling"></a>Tööülesande nurjumise töötlemine
Ülesande ebaõnnestumisi jagunevad järgmised Kategooriad:

- **Tõrked plaanimine**

    Tööülesande on määratud failide edastamine nurjumisel mingil põhjusel "ajastamise tõrge" on seatud tööülesanne.

    Plaanimine tõrked võivad tekkida, kui ülesande ressursi failid on teisaldatud, salvestusruumi konto pole enam saadaval või muu probleem ilmnes, et takistada failide sõlm eduka kopeerimine.

- **Rakenduse tõrked**

    Mis on määratud tööülesande käsurea protsess võib nurjuda ka. Protsessi käsitletakse kui väljumise nullist erinev kood on tagastatud käivitatud tööülesande, protsessi ei ole (vt järgmises jaotises *tööülesande väljumine koodid* ).

    Rakenduse tõrked, saate konfigureerida paketi automaatselt uuesti tööülesande ülespoole määratud arv kordi.

- **Piirangu tõrked**

    Saate seada piirang, mis määrab töö või tööülesande *maxWallClockTime*maksimaalne täitmise kestus. See võib olla kasulik lõpetamise "hangunud" tööülesanded.

    Kui maksimaalne aeg on ületatud, on märgitud tööülesande *lõpule viidud*, kuid välju kood on seatud `0xC000013A` ja *schedulingError* välja on märgitud `{ category:"ServerError", code="TaskEnded"}`.

### <a name="debugging-application-failures"></a>Rakenduse tõrked silumine

- `stderr`ja`stdout`

    Rakendus võib täitmisel, andes diagnostika väljundi, mille abil saate probleemide tõrkeotsing. Varasemast teemast [failide ja kataloogide](#files-and-directories)osutatud teenuse paketi kirjutab väljundit ja standardviga väljund `stdout.txt` ja `stderr.txt` failide kataloogis tööülesande Arvuta sõlme. Saate need failid alla laadida Azure portaali või üks pakett SDK-d. Näiteks saate alla laadida neid ja muid faile tõrkeotsinguks abil [ComputeNode.GetNodeFile] [ net_getfile_node] ja [CloudTask.GetNodeFile] [ net_getfile_task] paketi .NET teegis.

- **Tööülesande välju koodid**

    Nagu varem mainitud, tööülesande on märgitud nurjus paketi teenus võttis kui protsess, mis on sooritatud toiming tagastab nullist erinev välju koodi. Kui tööülesande täidab protsessi, kuvab paketi ülesande välju koodi atribuudi *tagasi protsessi kood*. See on oluline Pange tähele, et mõne ülesande välju kood on määratud **ei** paketi teenuse--seda määrab protsess ise või mil protsessi täidetud operatsioonisüsteem.

### <a name="accounting-for-task-failures-or-interruptions"></a>Raamatupidamine ülesande ebaõnnestumisi või segadusi

Tööülesannete aeg-ajalt võib nurjuda või katkestada. Ise ülesande rakendus võib nurjuda, sõlm, kus töötab tööülesande võib taaskäivitama või sõlme võib eemaldatakse pool suuruse muutmise toimingu käigus selle kausta deallocation poliitika on seatud väärtusele sõlmed kohe eemaldada, ootamata ülesannete lõpuleviimiseks. Igal juhul tööülesande saate olla automaatselt liigutas selle paigast, paketi teise sõlme täitmiseks.

Samuti on võimalik vahelduva probleemi põhjustada tööülesande hangub või võtab liiga kaua aega, et käivitada. Saate määrata ülesande maksimaalne täitmisaeg. Kui see on suurem, katkestab paketi tööülesande taotlus.

### <a name="connecting-to-compute-nodes"></a>Ühenduse arvutada sõlmed

Saate teha täiendavaid silumine ja tõrkeotsingu kaugühenduse teel Arvuta sõlme sisse logida. Saate alla laadida Windows sõlmed Kaugtöölaua protokolli (RDP) faili või hankida Secure Shell (SSH) ühenduseteavet Linuxi sõlmed Azure portaali. Te saate seda teha kasutades paketi API – näiteks [Paketi .NET] [ net_rdpfile] või [Paketi Python](batch-linux-nodes.md#connect-to-linux-nodes).

>[AZURE.IMPORTANT] Sõlm RDP või SSH loomiseks peate esmalt looma kasutaja sõlme. Selleks saate kasutada Azure portaali, [sõlme kasutajakonto lisamine] [ rest_create_user] paketi REST API abil helistamine [ComputeNode.CreateComputeNodeUser] [ net_create_user] meetod paketi .net-i või kõne [add_user] [ py_add_user] paketi Python mooduli meetod.

### <a name="troubleshooting-bad-compute-nodes"></a>Tõrkeotsingu "halb" arvutada sõlmed

Olukordades, kus mõned tööülesanded ei suuda, saate oma paketi kliendi rakenduse või teenuse uurida metaandmete nurjunud tööülesanded vigane sõlm tuvastamiseks. Igal pool sõlm on antud ainu-ID ja tööülesande töötab sõlme sisaldub tööülesande metaandmete. Kui olete kindlaks teinud probleemi sõlm, saate seda mitme toimingu:

- **Taaskäivitage sõlm** ([REST][rest_reboot] | [.NET][net_reboot])

    Taaskäivitada sõlme üles peidetud probleemid, näiteks ummikus või kukkus protsesside vahel tühjendada. Pange tähele, et kui teie pool kasutab start tööülesande või teie töö kasutab töö ettevalmistamine tööülesande, nad täidetakse sõlme taaskäivitamisel.

- **Reimage sõlm** ([REST][rest_reimage] | [.NET][net_reimage])

    See installib operatsioonisüsteemi sõlme. Koos taaskäivitus sõlm, käivitage tööülesanded ja töö ettevalmistustööd on uuesti, kui on antud reimaged sõlme.

- **Pool sõlme eemaldamine** ([REST][rest_remove] | [.NET][net_remove])

    Mõnikord on vaja pool sõlme täielikult eemaldada.

- **Ülesande ajastamine sõlme keelamine** ([REST][rest_offline] | [.NET][net_offline])

    See tõhus võtab sõlme "ühenduseta" nii, et pole veel tööülesanded määratakse selle, kuid lubab jääda sõlm töötava ja kogumi. See võimaldab teil teha edasise uurimise põhjuse ebaõnnestumisi ilma nurjunud tööülesande andmeid--ja ilma sõlme põhjustada täiendava tööülesande ebaõnnestumist. Näiteks saate keelata ülesande ajastamine on sõlm sündmuselogide uurida või teha muud tõrkeotsingu sõlme, siis [logige sisse kaugühenduse teel](#connecting-to-compute-nodes) . Kui olete lõpetanud oma juurdluste, saate siis tuua sõlm taastamisel, võimaldades ülesande ajastamine ([REST][rest_online] | [.NET][net_online]), ja tehke ühte toimingud, mida käsitletakse varasemas versioonis.

> [AZURE.IMPORTANT] Iga toimingu, mis on kirjeldatud selles jaotises--taaskäivitage, reimage, eemaldamine ja Keela ülesande ajastamine – teil on võimalik määrata, kuidas tööülesanded praegu töötavate sõlme käsitletakse kui toimingu tegemine. Näiteks kui keelate ülesande ajastamine sõlme abil paketi .net-i kliendi teek, saate määrata mõne [DisableComputeNodeSchedulingOption] [ net_offline_option] kellaajavälju väärtuseks määrata, kas soovite **lõpetada** töötab **Requeue** tööülesanded neile plaanimiseks muud sõlmed või lubada töötab ülesandeid täita (**TaskCompletion**) toimingu sooritamiseks.

## <a name="next-steps"></a>Järgmised sammud

- Tutvustavad valimi paketi rakenduse haaval [Alustamine Azure'i paketi teegi .net-i jaoks](batch-dotnet-get-started.md). On ka õpetuse, mis töötab on töökoormus Linux Arvuta sõlmed [Python versiooni](batch-python-tutorial.md) .

- Laadige alla ja koostada [Paketi Exploreri] [ github_batchexplorer] valimi projekti kasutamiseks, kui teil tekib teie paketi lahendusi. Paketi Exploreri abil saate teha järgmist ja järgmist.
  - Saate jälgida ja töödelda kaustu, töö ja ülesandeid teie paketi konto
  - Laadige alla `stdout.txt`, `stderr.txt`, ja muid faile sõlmed
  - Kasutajate loomiseks sõlmed ja remote login RDP-faile alla laadida

- Siit saate teada, kuidas [luua Linux Arvuta sõlmed](batch-linux-nodes.md).

- Külastage [Azure'i paketi Foorum] [ batch_forum] MSDN-is. Foorum on hea koht esitada küsimusi, kas just õppimise või on ekspert paketi abil.

[1]: ./media/batch-api-basics/node-folder-structure.png

[azure_storage]: https://azure.microsoft.com/services/storage/
[batch_forum]: https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurebatch
[cloud_service_sizes]: ../cloud-services/cloud-services-sizes-specs.md
[msmpi]: https://msdn.microsoft.com/library/bb524831.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_sample_taskdeps]:  https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/TaskDependencies
[github_batchexplorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[batch_net_api]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[msdn_env_vars]: https://msdn.microsoft.com/library/azure/mt743623.aspx
[net_cloudjob_jobmanagertask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobmanagertask.aspx
[net_cloudjob_priority]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.priority.aspx
[net_cloudpool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_cloudtask_env]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.environmentsettings.aspx
[net_create_cert]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificateoperations.createcertificate.aspx
[net_create_user]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.createcomputenodeuser.aspx
[net_getfile_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.getnodefile.aspx
[net_getfile_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.getnodefile.aspx
[net_job_env]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.commonenvironmentsettings.aspx
[net_multiinstancesettings]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.aspx
[net_onalltaskscomplete]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.onalltaskscomplete.aspx
[net_rdp]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.getrdpfile.aspx
[net_reboot]: https://msdn.microsoft.com/library/azure/mt631495.aspx
[net_reimage]: https://msdn.microsoft.com/library/azure/mt631496.aspx
[net_remove]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.removefrompoolasync.aspx
[net_offline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.disableschedulingasync.aspx
[net_online]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.enableschedulingasync.aspx
[net_offline_option]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.disablecomputenodeschedulingoption.aspx
[net_rdpfile]: https://msdn.microsoft.com/library/azure/Mt272127.aspx
[vnet]: https://msdn.microsoft.com/library/azure/dn820174.aspx#bk_netconf

[py_add_user]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.ComputeNodeOperations.add_user

[batch_rest_api]: https://msdn.microsoft.com/library/azure/Dn820158.aspx
[rest_add_job]: https://msdn.microsoft.com/library/azure/mt282178.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[rest_add_cert]: https://msdn.microsoft.com/library/azure/dn820169.aspx
[rest_add_task]: https://msdn.microsoft.com/library/azure/dn820105.aspx
[rest_create_user]: https://msdn.microsoft.com/library/azure/dn820137.aspx
[rest_get_task_info]: https://msdn.microsoft.com/library/azure/dn820133.aspx
[rest_job_schedules]: https://msdn.microsoft.com/library/azure/mt282179.aspx
[rest_multiinstance]: https://msdn.microsoft.com/library/azure/mt637905.aspx
[rest_multiinstancesettings]: https://msdn.microsoft.com/library/azure/dn820105.aspx#multiInstanceSettings
[rest_update_job]: https://msdn.microsoft.com/library/azure/dn820162.aspx
[rest_rdp]: https://msdn.microsoft.com/library/azure/dn820120.aspx
[rest_reboot]: https://msdn.microsoft.com/library/azure/dn820171.aspx
[rest_reimage]: https://msdn.microsoft.com/library/azure/dn820157.aspx
[rest_remove]: https://msdn.microsoft.com/library/azure/dn820194.aspx
[rest_offline]: https://msdn.microsoft.com/library/azure/mt637904.aspx
[rest_online]: https://msdn.microsoft.com/library/azure/mt637907.aspx

[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/
