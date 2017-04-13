<properties
   pageTitle="Rakenduse sõltuvus kuvari (ADM) Operations Management Suite (OMS) | Microsoft Azure'i"
   description="Sõltuvus kuvari (ADM) on toimingud halduse komplekti (OMS) lahenduse, mis automaatselt leiab operatsioonisüsteemides Windows ja Linux komponendid ja teenuste suhtlemine kaardid.  Selles artiklis on toodud üksikasjad juurutamine ADM keskkond ja kasutada erinevaid stsenaariumeid lähemalt."
   services="operations-management-suite"
   documentationCenter=""
   authors="daseidma"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/28/2016"
   ms.author="daseidma;bwren" />

# <a name="using-application-dependency-monitor-solution-in-operations-management-suite-oms"></a>Rakenduse sõltuvus kuvari lahenduse toimingud halduse komplekti (OMS) abil
![Teatiseikoon haldus](media/operations-management-suite-application-dependency-monitor/icon.png) Sõltuvus kuvari (ADM) automaatselt leiab operatsioonisüsteemides Windows ja Linux komponendid ja kaardid suhtlemine teenused. See võimaldab teil vaadata oma servereid, nagu te arvate neist – ühendatud võrkudega, et pakkuda kriitiliste teenuste.  Rakenduse sõltuvus kuvari näitab seoseid serverites protsessid, ja pordid üle kõik TCP ühendatud arhitektuur pole konfigureerimine vajalik erinevalt agent installi.

Selles artiklis kirjeldatakse rakenduse sõltuvus kuvari kasutamise üksikasjad.  ADM ja kasutuselevõtt agentide konfigureerimise kohta leiate teemast [rakenduse konfigureerimise kuvari sõltuvus lahenduse toimingud halduse komplekti (OMS)](operations-management-suite-application-dependency-monitor-configure.md)

>[AZURE.NOTE]Rakenduse sõltuvus kuvari praegu privaatne eelvaade.  Saate taotleda juurdepääsu ADM privaatne eelvaade [https://aka.ms/getadm](https://aka.ms/getadm)juures.
>
>Privaatne eelvaate, on kõik OMS kontod ADM. piiramatu juurdepääs  ADM sõlmed on tasuta, kuid Log Analytics andmete AdmComputer_CL ja AdmProcess_CL tüüpi on Mahupõhised nagu mis tahes muu lahendus.
>
>Pärast ADM sisestab avaliku eelvaade, oleks saadaval vaid tasulisi klientidele ülevaate ja analüüsi OMS hinnad kavas.  Tasuta taseme kontod piirduvad 5 ADM sõlmed.  Kui osalevad privaatne eelvaade ja on registreeritud OMS hinnad kavas ADM viimisel avaliku eelvaate, keelatakse ADM sel ajal. 


## <a name="use-cases-make-your-it-processes-dependency-aware"></a>Kasuta juhtudel: Tee oma töötleb arvestada sõltuvus

### <a name="discovery"></a>Tuvastamine
ADM koostab levinud viide kaardil sõltuvused automaatselt teie serverid, protsesside ja 3 tootja teenused.  See leiab ja kõik TCP sõltuvusi, tuvastamise ebameeldivusi ühendused remote kaartide 3 tootja süsteemid sõltuvad sellest, mida ja sõltuvused traditsiooniline tume alasid nagu DNS-i ja AD võrgu.  ADM leiab nurjus võrguühendust, mida teie hallatavate süsteemide üritavad teha, aidates teil võimalik serveri vale konfiguratsioon, teenuse katkestuste ja Võrguprobleemide tuvastamine.

### <a name="incident-management"></a>Sündmuse juhtimine
ADM aitab kõrvaldada ebaselgust probleemi eraldustaseme näitab teile, kuidas süsteemid on ühendatud ja üksteisest mõjutamata.  Lisaks nurjunud ühendused seotud klientide teavet aitab tuvastada valesti koormus soolise, üllatav või liigse koormus kriitiliste teenuste, ja petturitest kliendid, nagu arendaja masinad tootmissüsteemide rääkida.  Integreeritud töövood, mille OMS muutmine jälgimine võimaldab teil vaadata, kas muutmine sündmuse tagaandmebaas arvutisse või teenuse selgitatakse põhjuseks juhtum.

### <a name="migration-assurance"></a>Migreerimise tagamine
ADM võimaldab teil tõhusalt kavandamine, kiirendada ja kinnitage Azure migratsioon, tagada, et midagi on jäänud ja on pole ebameeldivusi katkestuste.  Saate kõik omavahel seotud süsteemid, mis koos migreerida, hinnata süsteemi konfiguratsioon ja võimsus ja tehke kindlaks, kas töötab süsteem on endiselt serveeritakse kasutajate või eemaldama asemel migreerimise jaoks on vaja teada.  Pärast teisaldamist on lõpule jõudnud, saate kliendi laadimine ja identiteedi kinnitamiseks ühenduse loomise katse süsteemid ja klientidega.  Kui alamvõrgu plaanimine ja tulemüüri määratlused on probleeme, on nurjunud ühendused ADM kaartidel saate osutage süsteemid, mida on vaja ühenduvust.

### <a name="business-continuity"></a>Süsteemi järjepidevuse tagamine
Kui kasutate Azure saidi taastamine ja vajate abi määratlemine taastamine jada rakenduse keskkonnas, ADM automaatselt saate kuvada kuidas süsteemide toetuvad üksteise tagamaks, et teie taastamise kava on usaldusväärne.  Valida kriitiliste server ja vaatamiseks oma klientidele, saate tuvastada ees süsteemide, mis peaks taastatud alles pärast seda kriitiliste server on taastatud ja saadaval.  Vastupidi, vaadates kriitilised serveri tagaandmebaas sõltuvused, saate tuvastada need süsteemid, mis tuleb enne taastamist oma fookuse süsteemi taastada.

### <a name="patch-management"></a>Paikade haldus
ADM suurendab OMS Update hindamise kasutamisel näitavad, mis muud meeskondi ja serverid sõltuvad teenust, nii saate teatada neid eelnevalt enne teie lappimine.  ADM suurendab paikade haldus OMS näitab teile, kas teie teenused on õigesti ühendatud ka pärast seda, kui nad on paigatud ja uuesti. 


## <a name="mapping-overview"></a>Vastenduse ülevaade
ADM agentide Koguge teavet kõik TCP ühendatud protsessid serveris, kus neil on arvutisse installitud, samuti üksikasjade iga protsessi sissetulevad ja väljaminevad ühendused.  Seadme loendi vasakus servas ADM lahenduse kasutamisel saab valida masinad ADM agentide visualiseerida nende sõltuvusi valitud ajavahemiku jooksul.  Seadme sõltuvus kaartide liikumine teatud arvutisse ja Kuva otsese TCP kliendid või selle arvuti serverid masinad.

![ADM ülevaade](media/operations-management-suite-application-dependency-monitor/adm-overview.png)

Masinad saab laiendada kaardil kuvamiseks töötavad protsessid aktiivne võrguühendus koos valitud ajavahemiku jooksul.  Kui kaugarvuti, ADM agent on laiendatud protsessi üksikasjade kuvamiseks, kuvatakse üksnes need protsessid suhtlemine fookuse kohapeal.  Arvu agentless ees masinad ühendamine kohale liikumine on märgitud loomisel protsesside vasakus servas.  Kui fookus masina on tegemist ühenduse tagaandmebaas masina agent, ilma tagaandmebaas on esindatud sõlm kaart, mis näitab selle IPv4 aadressi, ja sõlme saate laiendada, et kuvada üksikute pordid ja teenuseid, mis fookuse masina suhelda.

Vaikimisi ADM kaartide kuvamine objektisõltuvusteave viimase 10 minuti.  Kellaaja juhtelementide abil vasakus ülanurgas, kaardid saate kasutataks jaoks ajaloolise aja vahemikud, kuni kogu üks tund, kuidas sõltuvuste vanal varem, nt ajal juhtum või muudatuse ilmnemisele kuvamiseks.    ADM andmed on salvestatud 30 päeva tasulisele tööruumides ja tasuta tööruumides 7 päevaks.

![Seadme kaart koos valitud seadme atribuudid](media/operations-management-suite-application-dependency-monitor/machine-map.png)

## <a name="failed-connections"></a>Nurjunud ühendused
Nurjunud ühendused on esitatud ADM kaartide ja protsesside, punane kriipsjoontega kuvatakse, kui kliendi süsteemi ei suuda protsessi või pordi saavutamiseks.  Nurjunud ühendused on teatatud mis tahes süsteemi juurutatud ADM agent, kui süsteemi on nurjunud ühenduse loomise katse.  ADM meetmed see jälgides TCP sockets, et ei suuda ühendust luua.  See võib olla tingitud tulemüüri, olekuteenus kliendi või serveri või remote teenus on saadaval. 

![Nurjunud ühendused](media/operations-management-suite-application-dependency-monitor/failed-connections.png)

Mõistmist tõrkeotsingu migreerimise valideerimine, Turve analüüsi ja üldist arhitektuuri mõistmine aitab nurjunud ühendused.  Mõnikord ei ühendused on ohutu, kuid tihti otse, et probleem, nt Tõrkesiirde keskkond, mis on muutumas äkki kättesaamatu, valige käsk.. või kahe rakenduse astme ei saa rääkida pärast pilve migreerimist.  Pildil, IIS-i ja WebSphere nii töötab, kuid neid ei saa ühendust luua. 

## <a name="computer-and-process-properties"></a>Arvuti ja protsessi atribuudid
Vastenduse ADM navigeerimisel saate valida masinad ja protsesside saada nende atribuutide kohta täiendavat konteksti.  Masinad teavet DNS-i nimi, IPv4 aadressid, CPU ja mälu, VM tüübi, operatsioonisüsteemi versiooni, viimase taaskäivitage aja ja sealsete OMS ja ADM ID-de kohta.

Protsessi üksikasjad on kogutud operatsioonisüsteemi metaandmeid töötavate protsesside, sh protsessi nimi, protsessi kirjeldus, kasutajanimi ja Domeen (Windowsis), ettevõtte nimi, toote nimi, toote versioon, töötavat kausta, käsurea ja protsessi algusaeg.

![Protsessi atribuudid](media/operations-management-suite-application-dependency-monitor/process-properties.png)

Protsessi Kokkuvõte paani annab Lisateavet selle protsessi Ühenduvus, sh selle seotud pordid, sissetulevad ja väljaminevad ühendused ja ühendused nurjus. 

![Protsessi Kokkuvõte](media/operations-management-suite-application-dependency-monitor/process-summary.png)

## <a name="oms-change-tracking-integration"></a>OMS muutuste jälituse integreerimine
ADM's muutuste jälitamise integreerimine on automaatne, kui mõlema lahenduse on lubatud ja konfigureeritud oma OMS tööruumis.

Seadme Kokkuvõte paani näitab, kas muutuste jälitamise sündmust valitud arvutis valitud ajavahemiku jooksul.

![Seadme Kokkuvõte paan](media/operations-management-suite-application-dependency-monitor/machine-summary.png)

Seadme muutmine jälgimise paneel näitab loendi Kõik muudatused, viimase esimese, koos lingiga üldiseks Log otsida täiendavaid üksikasju.
![Seadme muutuste jälituse paan](media/operations-management-suite-application-dependency-monitor/machine-change-tracking.png)

Järgmine on süvitsiminek konfiguratsiooni muuta sündmuse pärast **Log Kasutusanalüüsi kuvamiseks**.
![Sündmuse konfigureerimine](media/operations-management-suite-application-dependency-monitor/configuration-change-event.png)


## <a name="log-analytics-records"></a>Kasutusanalüüsi kirjed
ADM kasutaja arvuti ja protsessi varude andmed on saadaval Log Analytics [Otsing](../log-analytics/log-analytics-log-searches.md) .  See saab rakendada stsenaariumid, sh migreerimise kavandamine, läbilaskevõime analüüs, discovery ja erakorralised jõudluse tõrkeotsingu. 

Ühe kirje on loodud tunnis iga kordumatu arvutis ja lisaks kirjete protsessi loodud kui, et protsessi või arvuti alustab või klõpsake-peatatud ADM. abil  Järgmistes tabelites on need kirjed atribuutide. 

On ettevõttesiseselt loodud atribuutide abil saate tuvastada kordumatu protsesside ja arvutites.

- PersistentKey_s on kordumatult määratletud protsessi konfiguratsiooni, nt käsurea ja kasutaja ID-d.  See on kordumatu antud arvutis, kuid võib arvutite korrata.
- ProcessId_s ja ComputerId_s on globaalselt kordumatu ADM mudel.



### <a name="admcomputercl-records"></a>AdmComputer_CL kirjed
Kirjed, mis tüüpi **AdmComputer_CL** varude andmed on ADM agentide serverid.  Järgmises tabelis on need kirjed atribuudid.  

| Atribuut | Kirjeldus |
|:--|:--|
| Tüüp | *AdmComputer_CL* |
| SourceSystem | *OpsManager* |
| ComputerName_s | Windowsi või Linuxi arvuti nimi |
| CPUSpeed_d | Kiirus MHz Protsessor |
| DnsNames_s | Kõik selle arvuti DNS-i nimede loend |
| IPv4s_s | Loendi kõigi IPv4 aadressid Kasuta selles arvutis |
| IPv6s_s | Kasuta selles arvutis kõigi IPv6-aadresside loend.  (ADM tuvastab IPv6 aadressid, kuid avastamine IPv6 sõltuvused.) |
| Is64Bit_b | tõene või väär OS tüüp |
| MachineId_s | Sisemise GUID kordumatu üle mõne OMS tööruumi  |
| OperatingSystemFamily_s | Windowsi või Linuxi |
| OperatingSystemVersion_s | Pikk OS versioon string |
| TimeGenerated | Kuupäev ja kellaaeg, mis on loodud kirje. |
| TotalCPUs_d | CPU valdkond arv |
| TotalPhysicalMemory_d | Mälumaht MB |
| VirtualMachine_b | tõene või väär vastavalt sellele, kas OS on külalisena VM |
| VirtualMachineID_g | Hyper-V VM ID |
| VirtualMachineName_g | Hyper-V VM nime |
| VirtualMachineType_s | HyperV, Vmware, Xen, Kvm, Ldom, Lpar, Virtualpc |


### <a name="admprocesscl-type-records"></a>Kirjete AdmProcess_CL tüüp 
Kirjed, mis tüüpi **AdmProcess_CL** on varude andmeid TCP ühendatud protsesside ADM agentide serverites.  Järgmises tabelis on need kirjed atribuudid.

| Atribuut | Kirjeldus |
|:--|:--|
| Tüüp | *AdmProcess_CL* |
| SourceSystem | *OpsManager* |
| CommandLine_s | Täielik käsurea protsess |
| CompanyName_s | Ettevõtte nimi (alates Windows PE või Linuxi pööret MINUTIS) |
| Description_s | Pikk protsess kirjeldus (alates Windows PE või Linuxi pööret MINUTIS) |
| FileVersion_s | Täitmisfail versiooni (Windows PE, ainult Windows): |
| FirstPid_d | OS protsessi ID |
| InternalName_s | Käivitatava faili sisemine nimi (alates Windows PE, ainult Windows) |
| MachineId_s | Sisemise GUID kordumatu üle mõne OMS tööruumi  |
| Name_s | Protsessi käivitatava nimi |
| Path_s | Protsessi käivitatava süsteemi failitee |
| PersistentKey_s | Sisemise GUID kordumatu sellest arvutist |
| PoolId_d | Sisemine ID liitmise põhineb sarnased käsk read. |
| ProcessId_s | Sisemise GUID kordumatu üle mõne OMS tööruumi  |
| ProductName_s | Toote nimi tekstistringi (Windows PE või Linuxi pööret MINUTIS) |
| ProductVersion_s | Toote versioon tekstistringi (Windows PE või Linuxi pööret MINUTIS) |
| StartTime_t | Protsessi alguskellaaeg kohaliku arvuti kella. |
| TimeGenerated | Kuupäev ja kellaaeg, mis on loodud kirje. |
| UserDomain_s | Domeeni protsessi omaniku (ainult Windows) |
| UserName_s | Nime protsessi omanik (ainult Windows) |
| WorkingDirectory_s | Protsessi töötavat kausta |


## <a name="sample-log-searches"></a>Valimi log otsingud

### <a name="list-the-physical-memory-capacity-of-all-managed-computers"></a>Loetle kõik hallatavad arvutid füüsilise mälumahu. 
Tüüp = AdmComputer_CL | Valige TotalPhysicalMemory_d ComputerName_s | Dedup ComputerName_s

![ADM päringu näide](media/operations-management-suite-application-dependency-monitor/adm-example-01.png)

### <a name="list-computer-name-dns-ip-and-os-version"></a>Arvuti nimi, DNS-i, IP ja OS versioon.
Tüüp = AdmComputer_CL | Valige ComputerName_s OperatingSystemVersion_s, DnsNames_s, IPv4s_s | Dedup ComputerName_s

![ADM päringu näide](media/operations-management-suite-application-dependency-monitor/adm-example-02.png)

### <a name="find-all-processes-with-sql-in-the-command-line"></a>Otsige üles kõik protsessid "SQL" käsureal
Tüüp = AdmProcess_CL CommandLine_s = \*SQL-i\* | Dedup ProcessId_s

![ADM päringu näide](media/operations-management-suite-application-dependency-monitor/adm-example-03.png)

### <a name="after-viewing-event-data-for-given-process-use-its-machine-id-to-retrieve-the-computers-name"></a>Pärast sündmuse andmete vaatamiseks antud töötlemine, selle seadme ID abil saate tuua arvuti nimi
Tüüp = "m!m-9bb187fa-e522-5f73-66d2-211164dc4e2b" AdmComputer_CL | Erinevate ComputerName_s

![ADM päringu näide](media/operations-management-suite-application-dependency-monitor/adm-example-04.png)

### <a name="list-all-computers-running-sql"></a>Loetle kõik arvutid SQL-i
Tüüp = AdmComputer_CL MachineId_s tolli {tüüp = AdmProcess_CL \*SQL-i\* | Erinevate MachineId_s} | Erinevate ComputerName_s

![ADM päringu näide](media/operations-management-suite-application-dependency-monitor/adm-example-05.png)

### <a name="list-of-all-unique-product-versions-of-curl-in-my-datacenter"></a>Loend kõik curl minu andmekeskuses kordumatu toote versioon
Tüüp = AdmProcess_CL Name_s = curl | Erinevate ProductVersion_s

![ADM päringu näide](media/operations-management-suite-application-dependency-monitor/adm-example-06.png)

### <a name="create-a-computer-group-of-all-computers-running-centos"></a>Kõik arvutid CentOS arvuti rühma loomine

![ADM päringu näide](media/operations-management-suite-application-dependency-monitor/adm-example-07.png)



## <a name="diagnostic-and-usage-data"></a>Diagnostika-ja kasutamine
Microsoft kogub automaatselt kasutus- ja jõudluse andmeid rakenduse sõltuvus kuvari teenuse kasutamise kaudu. Microsoft kasutab neid andmeid sisestada ja parandada kvaliteeti, turvalisus ja terviklus rakenduse sõltuvus kuvari teenuse. Andmete sisaldab teavet teie tarkvara operatsioonisüsteemi ja versioon nagu konfiguratsiooni ja sisaldab ka IP-aadress, DNS-i nimi ja töökoha nimi probleemide tõrkeotsinguks ja lahendamiseks täpne ja tõhus võimaluste pakkumiseks. Me ei kogu nimesid, aadresse või muud kontaktteavet.

Andmete kogumise ja kasutamise kohta leiate lisateavet teemast [Microsoft Online Services privaatsusavaldus](hhttps://go.microsoft.com/fwlink/?LinkId=512132).



## <a name="next-steps"></a>Järgmised sammud
- Lisateavet [logige otsingud](../log-analytics/log-analytics-log-searches.md] in Log Analytics to retrieve data collected by Application Dependency Monitor.)
