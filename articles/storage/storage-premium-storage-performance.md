<properties
    pageTitle="Azure Premium mälu: Kujundada jõudluse | Microsoft Azure'i"
    description="Kujundage suure jõudlusega rakenduste abil Azure Premium mälu. Premium salvestusruumi pakub suure jõudlusega, latentsusajaga ketta I/O-nõudvaid töökoormus töötab Azure'i Virtuaalmasinates tugi."
    services="storage"
    documentationCenter="na"
    authors="aungoo-msft"
    manager="tadb"
    editor="tysonn" />

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="aungoo"/>

# <a name="azure-premium-storage-design-for-high-performance"></a>Azure Premium mälu: Suure jõudlusega kujundus

## <a name="overview"></a>Ülevaade  
Selles artiklis antakse juhiseid suure jõudlusega rakenduste abil Azure Premium mälu. Saate kasutada koos jõudluse põhitõed kehtivad rakenduse tehnoloogiaid selles dokumendis kuvatavad juhised. Et illustreerida suuniseid, on kasutatud SQL Server töötab Premium salvestusruumi näidisena selles dokumendis.

Kuigi me jõudluse stsenaariumid salvestusruumi kiht selle artikli, peate optimeerimine rakenduse kiht. Näiteks kui olete hosting SharePointi pargis Azure Premium Storage, saate selle artikli näited SQL serveri optimeerimine andmebaasiserveri. Lisaks optimeerida SharePointi serveripargi veebiserverisse ja Application server jõudluse saada.

See artikkel aitab vastus jälgimise levinud küsimustele rakenduse jõudluse Azure Premium Storage, optimeerimine

-   Kuidas mõõta oma rakenduse jõudlus?  
-   Miks te ei näe oodatud suurt jõudlust?  
-   Millised tegurid mõjutavad rakenduse jõudluse Premium säilitamise kohta?  
-   Kuidas teguritest mõjutada jõudlust rakenduse Premium säilitamise kohta?  
-   Kuidas saab optimeerida IOPS, läbilaskevõime ja latentsusaeg jaoks?  

Oleme pakkunud neid juhiseid spetsiaalselt Premium salvestusruumi Kuna töökoormus töötavate Premium mälu on väga tundliku jõudlust. Oleme pakkunud näited vajaduse korral. Mõned juhised saate rakendada ka kindlad ketast IaaS VMs töötavad rakendused.

Enne alustamist, kui teil on uus Premium salvestusruumi, lugege esmalt soovitud [Premium mälu: suure jõudlusega salvestusruumi Azure virtuaalse masina töökoormus](storage-premium-storage.md) artiklis ja [Azure Premium salvestusruumi skaleeritavus ja jõudluse sihtkohad](storage-scalability-targets.md#premium-storage-accounts).

## <a name="application-performance-indicators"></a>Rakenduse mõõdikud  
Me hinnata, kas rakenduse toimib ka või mitte kasutamine jõudluse näidikud, näiteks, kui kiiresti rakenduse töötleb kasutaja taotlus, kui palju andmeid rakenduse töötleb taotluse kohta, mitu taotleb on ka rakenduse töötlemine aeg, kui kaua kasutaja on ootama vastuse saamine pärast jagada teatud aja jooksul. Tehnilisi termineid jaoks need mõõdikud on IOPS, läbilaskevõime või läbilaskevõime ja latentsusaeg.

Selles jaotises me arutada levinud mõõdikud Premium salvestusruumi kontekstis. Järgmises jaotises kogumine rakenduse nõuded, saate teada, kuidas need mõõdikud rakenduse mõõta. Allpool olevat rakenduse jõudluse optimeerimine tutvustame teile nende mõõdikud ja soovitused optimeerida need mõjutavad tegurid.

## <a name="iops"></a>IOPS  
IOPS on arv, nõuab, et teie taotlus on saata salvestusruumi ketast ühe teise. Toimingu sisend saanud lugeda või kirjutada, järjestikku või juhuslik. OLTP rakendusi nagu veebisait veebikaubandus vaja töödelda mitme kasutaja samaaegseid taotlusi kohe. Kasutaja taotlused on lisa ja intensiivse andmebaasitoiminguid, mis rakendus peab töötlemine kiiresti värskendada. Seetõttu tuleb OLTP rakenduste väga kõrge IOPS. Selliste taotluste käsitlemiseks miljoneid väike- ja juhusliku IO taotlused. Kui teil on taotluse, peate rakenduse taristu optimeerimise jaoks IOPS kujundama. Hiljem jaotises *Rakenduse jõudluse optimeerimine*käsitleme üksikasjalikult tegureid, mida peate arvesse võtma kõrge IOPS saada.

Kui manustate premium salvestusruumi kettal oma kõrge skaala VM, Azure sätteid saate IOPS tagatud arv ühe ketta määratlus. Näiteks P30 kettal sätted 5000 IOPS. Iga kõrge skaala VM maht on ka teatud IOPS piirang, mis seda saate säilitada. Näiteks on standardne GS5 VM 80 000 IOPS piirata.

## <a name="throughput"></a>Läbilaskevõime  
Läbilaskevõime või läbilaskevõime on andmeid, mis rakenduse saadab salvestusruumi ketast määratud intervalli summa. Kui teie taotlus on toimingute sisend suurte IO ühiku suurused, on vaja kõrge läbilaskevõime. Andmete ladu rakenduste tavaliselt probleemi skannimine intensiivse, juurdepääs andmete suur osa korraga ja sagedamini hulgi toiminguid. Teisisõnu, nt rakenduste puhul suurema läbilaskevõimega. Kui teil on taotluse, peate kujundama oma infrastruktuuri läbilaskevõime optimeerimine. Järgmise jaotise käsitleme üksikasjalikult tegureid, mis teil tuleb selleks häälestada.

Kui manustate premium salvestusruumi kettal kõrge skaala VM, Azure sätete läbilaskevõime vastavalt selle ketta määratlus. Näiteks P30 kettal sätted 200 MB teise ketta läbilaskevõime kohta. Iga kõrge skaala VM maht on ka teatud läbilaskevõime piirang, mis seda saate säilitada. Näiteks Standard GS5 VM on maksimaalne läbilaskevõime 2000 MB sekundis.

On läbilaskevõime ja IOPS, nagu on näidatud allpool esitatud valemi abil.

![](media/storage-premium-storage-performance/image1.png)

Seetõttu on oluline optimaalse läbilaskevõime ja IOPS väärtused, mis rakenduse jaoks on vaja määrata. Kui proovite optimeerimine ühte, teine ka saab mõjutada. Hiljem jaotises *Rakenduse jõudluse optimeerimine*me arutada lähemalt optimeerimise IOPS ja läbilaskevõime kohta.

## <a name="latency"></a>Latentsus  
Latentsus on rakenduse ühe taotluse vastu võtta, saata salvestusruumi ketast ja saata vastuse kliendi kuluv aeg. See on rakenduse jõudluse Lisaks IOPS ja läbilaskevõime kriitilised mõõt. Premium salvestusruumi kettal latentsus on päringu jaoks teabe toomiseks ja edastama tagasi rakenduse kuluv aeg. Premium salvestusruumi pakub ühtsete madal latentsused. Kui lubate kirjutuskaitstud host vahemällu premium salvestusruumi kettale, saate palju alumise loetuks latentsus. Me arutada ketta vahemällu hiljem jaotise Täpsemalt *optimeerimine rakenduse*tööle.

Kui olete rakenduse kõrgema IOPS ja läbilaskevõime optimeerida, mõjutab rakenduse latentsus. Pärast häälestamist rakenduse jõudlus, alati hinnata latentsus ootamatud pika latentsusajaga käitumise vältimiseks.

## <a name="gather-application-performance-requirements"></a>Koguge rakenduse nõuded  
Esimene samm töötavate Azure Premium Storage suure jõudlusega rakenduste kujundamine on mõista rakenduse jõudluse nõuetele. Kui te koguda nõuded, saate optimeerida rakenduse saavutamiseks kõige optimaalse jõudluse tagamiseks.

Eelmises jaotises selgitatakse me levinud mõõdikud IOPS, läbilaskevõime ja latentsusaeg. Tuleb kindlaks teha, mida mõõdikud need olulised rakenduse soovitud kasutusvõimalused esitamisel. Näiteks kõrge IOPS vajalikest OLTP rakendustele töötlemine tehingud miljoneid teine. Kõrge läbilaskevõime on kriitiline andmebaas rakenduste teine suurte andmehulkade töötlemine. Väga väike latentsus on oluline reaalajas rakendusi nagu reaalajas video streaming veebilehed.

Järgmiseks mõõta oma eluea rakenduse maksimaalse jõudluse nõuetele. Kasutage valimi kontroll all algus. Kirje maksimaalse jõudluse nõuded ajal tavaline, tippväärtus ja off-tunni töökoormus perioodid. Nõuded kõigil töökoormus tuvastades saab määrata rakenduse üldise jõudluse nõue. Tavaline töökoormus pood veebisaidi olla näiteks tehingud, see pakub kõige päeva aastas. Veebisaidi tippväärtus töökoormus on see pakub pühade või allahindlus sündmused tehingud. Tipp töökoormus on tavaliselt kogenud piiratud aja jooksul, kuid saate nõuda rakenduse mastaapimiseks kaks või enam korda tavapärase toimimise. Siit saate teada 50 protsentiili, 90 protsentiili ja 99 protsentiili nõuetele. See aitab filtreerida mis tahes võõrväärtuste jõudluse nõuded ja teie püüete keskenduda optimeerimine jaoks õige väärtused.

**Rakenduse jõudluse nõuete kontroll-loend**

| **Nõuded** | **50 percentile** | **90 protsentiili** | **99 percentile** |
|---|---|---|---|
| Max. Tehinguid sekundis | | | |
| % Read toimingud            | | | |
| % Kirjutada toiminguid           | | | |
| % Juhusliku toimingud          | | | |
| % Järjestikku toimingud      | | | |
| IO taotluse suurus              | | | |
| Keskmine jõudlus           | | | |
| Max. Läbilaskevõime              | | | |
| Min. Latentsus                 | | | |
| Keskmise latentsuse              | | | |
| Max. CPU                     | | | |
| Keskmine CPU                  | | | |
| Max. Mälu                  | | | |
| Keskmine mälu               | | | |
| Järjekorda sügavus                  | | | |

>**Olulise teabe märge:**  
>Kaaluge skaleerimist need numbrid, mis põhineb rakenduse oodatud kasvu tulevikus. On hea mõte kavandamine growth ette valmistada, kuna see võib olla raskem taristu hiljem jõudluse parandamiseks.

Kui olete taotluse ja teisaldada Premium salvestusruumi, esmalt koostada kontroll-loendi kohal olemasoleva rakenduse. Seejärel rakenduse Premium Storage prototüüp ja kujundada rakenduse põhjal *Optimeerida rakenduse jõudluse* hiljem jaotises selle dokumendi kirjeldatud juhiseid. Järgmises jaotises kirjeldatakse tööriistade abil saate kogumine jõudluse mõõtmed.

Saate luua kontroll-loendi sarnaselt olemasoleva rakenduse kinnitustunnistuse. Võrdleva hindamise tööriistade abil saate simuleerida süsteemis ja prototüüp rakenduse jõudluse mõõtmiseks. Vt lisateavet [võrdleva hindamise](#benchmarking) . Tehes nii saate määrata, kas Premium salvestusruumi saate vastavad või ületavad teie taotluse jõudluse nõuded. Seejärel saate rakendada tootmise rakenduse samu juhiseid.

### <a name="counters-to-measure-application-performance-requirements"></a>Hinnale mõõta rakenduse nõuded  
Parim viis mõõta oma rakenduse nõuete on operatsioonisüsteem serveri jõudluse jälgimise tööriista kasutada. Saate kasutada Windowsi Perfmoni ja iostat Linuxi jaoks. Nende tööriistade jäädvustada vastav iga mõõt, mis on kirjeldatud ülal jaotises hinnale. Nende hinnale väärtused peavad jäädvustada, kui teie rakendus töötab selle tavaline, tippväärtus ja off-tunni töökoormus.

Protsessor, mälu ja iga loogilise ketta ja füüsilise ketta serveri jaoks saadaolevate Perfmoni hinnale. Premium salvestusruumi ketast kasutamisel koos VM füüsilise ketta hinnale on iga premium salvestusruumi ketta ja loogiline ketas hinnale on iga loodud premium salvestusruumi kettale maht. Peate jäädvustada oma rakenduse töökoormus majutavad ketast väärtused. Kui üks-ühele vastenduse vahel loogiline ja füüsiline ketast, saate viidata füüsilise ketta hinnale; muul juhul viitavad loogilise ketta hinnale. Linuxi iostat käsk loob CPU ja kettaruumi kasutamise aruanne. Ketas kasutamise aruanne statistikaga seadme või sektsiooni kohta. Kui teil on oma andmed ja log andmebaasiserveri eraldi kettale, koguda nii ketast selle andmeid. Allpool tabelis kirjeldatakse hinnale ketast, protsessor ja mälu.

| Counter | Kirjeldus | Perfmoni | Iostat |
|---|---|---|---|
| **IOPS või tehinguid sekundis** | I/O taotlusi sekundis kettale salvestusruumi antud arv. | Kui teil on järgmine sekundis <br> Ketas kirjutab sekundis | TPS <br> r/s <br> w/s |
| **Ketas loeb ja kirjutab** | % loeb ja kirjutada ketta toimingutest. | % Ketta lugemine aeg <br> % Vaba aeg | r/s <br> w/s |
| **Läbilaskevõime** | Andmeid lugeda või kirjutada ketta sekundis summa. | Ketas lugeda baiti sekundis <br> Kettale kirjutamine baiti sekundis | kB_read/s <br> kB_wrtn/s |
| **Latentsus** | Kogu aeg ketta IO taotluse lõpuleviimiseks. | Keskmine ketta sec/lugemine <br> Keskmine ketta sec/kirjutamine | ootavad <br> svctm |
| **IO suurus** | Suuruse sisend taotleb salvestusruumi ketast küsimusi. | Keskmine ketta baiti/lugemine <br> Keskmine ketta baiti/kirjutamine | avgrq-sz |
| **Järjekorda sügavus** | Arvu tasumata sisend taotleb ootel vormi lugeda või kirjutada kettale salvestusruumi. | Praeguse ketta järjekorra pikkus | avgqu-sz |
| **Max. Mälu** | Nõutav on rakendus tõrgeteta mälu | % Kinnitatud baiti kasutamine | Kasutage vmstat |
| **Max. CPU** | Summa CPU rakenduse tõrgeteta nõutav | % Protsessori aeg | % util |

Lisateave [iostat](http://linuxcommand.org/man_pages/iostat1.html) ja [Perfmoni](https://msdn.microsoft.com/library/aa645516.aspx).


## <a name="optimizing-application-performance"></a>Rakenduse jõudluse optimeerimine  
Peamised rakendus töötab Premium mälu jõudlust mõjutavad tegurid on laadi IO taotlused, VM suurus, ketta suurus, arv ketast, ketta vahemällu, Multithreading ja järjekorda sügavus. Nupud, mis on esitatud süsteemi abil saate määrata mõnda järgmistest teguritest. Enamik rakendusi võib anda teile muuta IO suurust ja järjekorda sügavus otse soovitud suvand. Näiteks kui kasutate SQL Server, ei saa valida IO suurus ja järjekorda sügavus. SQL serveri valib optimaalse IO suurus ja järjekorda sügavus väärtused jõudluse saada. On oluline mõista mõlemat tüüpi tegureid mõju jõudlusest rakenduse, et olete saab ette asjakohased ressursid jõudluse vajadustele.

Selles jaotises kogu viidata rakenduse nõuded kontroll-loendi loodud kindlaks teha, kui palju peate optimeerimine rakenduse jõudluse. Lähtudes, et teid kindlaks teha, millised tegurid selles jaotises on vaja häälestada. Rakenduse jõudluse iga teguri mõju osaleda võrdleva tööriistu kasutada oma rakenduste häälestamine. Vaadake jaotist [võrdleva hindamise](#Benchmarking) levinud võrdleva tööriistu kasutada Windows ja Linux VMs üksikasjalikud juhised käesoleva artikli lõpus.

### <a name="optimizing-iops-throughput-and-latency-at-a-glance"></a>Optimeerimine IOPS, läbilaskevõime ja latentsusaeg ülevaade  
Järgmises tabelis on kokkuvõte jõudlust mõjutavad tegurid ja toiminguid, kuidas optimeerida IOPS, läbilaskevõime ja latentsusaeg. Käesoleva kokkuvõtte jaotistes kirjeldatakse iga on palju sügavust.

| | **IOPS** | **Läbilaskevõime** | **Latentsus** |
|---|---|---|---|
| **Näide stsenaarium** | Ettevõtte OLTP rakendus, mis nõuab väga kõrge tehingute koguväärtus teise.                                                                                                                                 | Ettevõtte andmete lao rakenduse töötlemise suurte andmemahtude. | Lähedal reaalajas rakenduste kasutaja taotlusi, nt online mängu kohe vastuseid. |
| Jõudlust mõjutavad tegurid  | | | |
| **IO suurus** | Väiksem IO annab kõrgema IOPS.                                                                                                                                                                           | Et IO suuremaks annab suurema läbilaskevõimega. | |
| **VM suurus** | Kasutage VM suurus, mis pakub IOPS, mis on suurem kui teie rakendus nõue. Lugege teemat VM suurused ja siin IOPS piirangud. | Kasutage VM läbilaskevõime tähtaja, mis on suurem kui teie rakendus nõue. Vt VM suurused ja nende läbilaskevõime piirangute siin. | Kasutage VM suurus, et pakutakse mastaapimiseks piirangud, mis on suurem kui teie rakendus nõue. Lugege teemat VM suurused ja limiidid siin. |
| **Ketta suurus** | Kasutage ketta suurus, mis pakub IOPS, mis on suurem kui teie rakendus nõue. Lugege teemat ketta suurused ja siin IOPS piirangud. | Kasutada kettapuhastusriista suurus läbilaskevõime piirang, mis on suurem kui teie rakendus nõue. Lugege teemat ketta suurused ja siin läbilaskevõime piirangud. | Kasutage ketta suurus, et pakutakse mastaapimiseks piirangud, mis on suurem kui teie rakendus nõue. Vt ketta suurused ja nende piirangud siin. |
| **VM ja ketta skaala piirangud** | IOPS limiit valitud VM suurus peaks olema suurem kui kokku IOPS juhitud premium salvestusruumi ketast manustatud. | Läbilaskevõime limiit valitud VM suurus peaks olema suurem kui läbilaskevõime juhitud premium salvestusruumi ketast, millele on manustatud selle kogusumma. | Valitud VM suuruse skaala piirangud peab olema suurem kui manustatud premium salvestusruumi ketast kokku skaala piirid. |
| **Ketta vahemällu** | Luba premium salvestusruumi kettale lugemine raske toimingutega saada kõrgema lugemine IOPS kirjutuskaitstud vahemälu. | | Luba kirjutuskaitstud vahemälu premium salvestusruumi kettale valmis saada väga väike lugemine latentsused raske toimingutega. |
| **Ketas Striping** | Kasutage mitme ketast ja triip neid koos saada kombineeritud kõrgema IOPS ja läbilaskevõime määra. Pange tähele, et VM kombineeritud limiidist peaks olema suurem kui manustatud premium ketast kombineeritud piirid. | |
| **Triip suurus** | Juhuslik small IO muster OLTP rakendustes kuvada triip väiksem. Näiteks kasutada SQL serveri OLTP rakenduse triip 64KB suurust. | Järjestikune suurte IO muster andmebaas rakendustes kuvada triip suuremaks. Näiteks kasutada SQL serveri andmetega ladu rakenduse 256KB triip suurus. | |
| **Multithreading** | Kasutage multithreading push Premium salvestusruumi, mis viib suurema IOPS ja läbilaskevõime taotluste suurem arv. Näiteks SQL serveri väärtuseks kõrge MAXDOP väärtuseks rohkem protsessorid eraldatava SQL serveri. | |
| **Järjekorda sügavus** | Suuremate järjekorda sügavus annab kõrgema IOPS. | Suuremate järjekorda sügavus annab suurema läbilaskevõimega. | Väiksem järjekorda sügavus annab alumise latentsused. |

## <a name="nature-of-io-requests"></a>Laadi IO taotlused  
Taotluse IO on ühiku sisend toiming, mis rakenduse täita. Juhusliku või järjestikuse, IO taotluste laadi tuvastamine lugeda või kirjutada, väike või suur, aitavad teil kindlaks teha rakenduse jõudluse nõuetele. See on väga oluline mõista IO taotlusi, mida teha õigeid otsuseid oma rakenduse taristu kujundamisel laad.

IO maht on üks oluline tegureid. IO maht on loodud rakenduse sisend toiming taotluse suurust. IO suurus on oluline mõju jõudlust eriti IOPS ja läbilaskevõime kulu rakendus on võimalik saada. Järgmine valem näitab IOPS, seos IO suurus ja läbilaskevõime ja läbilaskevõime.  
    ![](media/storage-premium-storage-performance/image1.png)

Mõned rakendused võimaldavad teil muuta oma IO suurus, mõned rakendused mitte. Näiteks SQL serveri määratleb IO optimaalne suurus ise ja anda kasutajatele mis tahes nupud seda muuta. Teisalt, Oracle'i pakub nimetatakse parameetri [DB\_blokeerida\_suurus](https://docs.oracle.com/cd/B19306_01/server.102/b14211/iodesign.htm#i28815) kasutades, mida saate konfigureerida andmebaasi I/O taotluse suuruse.

Kui kasutate rakendus, mis ei luba teil IO suuruse muutmiseks, kasutage juhiseid selle artikli optimeerida KPI-d, mis on kõige olulisemad rakenduse. Näiteks

-   Rakenduse OLTP loob miljoneid väike- ja juhusliku IO taotlused. Need tüüpi IO taotlusi käsitlema peate kujundama oma rakenduse taristu kõrgema IOPS saada.  
-   Andmed, mis on rakenduse lao loob suur ja järjestikku IO taotlused. Need tüüpi IO taotlusi käsitlema peate kujundama oma rakenduse taristu suurema läbilaskevõime või läbilaskevõime.

Kui kasutate rakendus, mis võimaldab teil IO suurust muuta, kasutada selle rusikareegel IO suurus lisaks muude jõudluse juhistest

-   IO väiksem kõrgema IOPS saada. Näiteks 8 rakendus OLTP KB.  
-   IO suuremaks saada suurema läbilaskevõime/läbilaskevõimega. Näiteks 1024 KB andmete ladu rakenduse.

Siin on näide selle kohta, kuidas saate arvutada IOPS ja läbilaskevõime ja läbilaskevõime rakenduse. Kaaluge rakenduse abil P30 kettal. Võite saavutada IOPS ja läbilaskevõime ja läbilaskevõime P30 kettal maksimumväärtus on 5000 IOPS 200 MB sekundis vastavalt ja. Nüüd, kui teie rakendus nõuab maksimaalne IOPS P30 kettalt ja kasutate IO väiksem nagu 8 KB, on tulemuseks läbilaskevõime, on võimalik saada sekundis 40 MB. Kui teie rakendus nõuab maksimaalse läbilaskevõime ja läbilaskevõime P30 kettalt ja kasutate IO suuremaks nagu 1024 KB, tulemuseks IOPS on siiski väiksem, 200 IOPS. Seetõttu häälestada IO suurust nii, et see vastaks nii teie taotlus IOPS ja läbilaskevõime ja läbilaskevõime nõue. Järgmises tabelis on toodud erinevad IO suurused ja nende vastavate IOPS ja läbilaskevõime P30 ketta jaoks.

| **Rakenduse nõue** | **I/O suurus** | **IOPS** | **Läbilaskevõime ja läbilaskevõime** |
|-----------------------------|--------------|----------|--------------------------|
| Max IOPS                    | 8 KB         | 5000    | 40 MB sekundis         |
| Max jõudlus              | 1024 KB      | 200      | 200 MB sekundis        |
| Max Throughput + kõrge IOPS  | 64 KB        | 3200    | 200 MB sekundis        |
| Max IOPS + kõrge läbilaskevõime  | 32 KB        | 5000    | 160 MB sekundis        |

IOPS ja läbilaskevõime, mis on suurem kui ühe premium salvestusruumi kettal suurima väärtuse saamiseks kasutage mitme premium ketast triibuline koos. Näiteks triip kaks P30 plaati saada kombineeritud IOPS 10 000 IOPS või kombineeritud läbilaskevõime 400 MB sekundis. Järgmises jaotises kirjeldatud tuleb kasutada VM suurus, mis toetab kombineeritud kettale IOPS ja läbilaskevõime.

>**Märkus:**  
>Kas IOPS või teise suureneb ka läbilaskevõime suurendamisel veenduge, et teil tabas jõudlus või IOPS piirangud ketta või VM kui suurendamine ühe.

Rakenduse jõudluse IO maht mõju osaleda tööriistu saate kasutada võrdleva VM ja ketast. Mitme testi käivitatakse loomine ja muu IO suurus iga abil vaadata, millist mõju. Vaadake lisateavet käesoleva artikli lõpus jaotist [võrdleva hindamise](#Benchmarking) .

## <a name="high-scale-vm-sizes"></a>Suure ulatuse VM suurused  
Rakenduse kujundamise käivitamisel üks esimese asjad, mida teha on, valige VM majutada oma rakenduse. Salvestusruumi Premium kõrge skaala VM suurused, mida saab käitada rakenduste kõrgema Arvuta power ja kõrge kohalikule kettale I/O jõudlust. Need VMs kiiremini protsessorite, suurem mälumahuga-core suhe ja soovitud Solid-State Drive ketas ette kohalikule kettale. Kõrge skaala VMs toetavad Premium mälu on DS, DSv2 ja GS sarja VMs.

Kõrge skaala VMs on saadaval erinevas suuruses eri kohtade arvuga CPU südamikud, mälu, OS ja ketta ajutised suurus. Iga VM maht on ka andmete ketast, mille saate manustada VM arvu ülempiir. Seetõttu valitud VM suurus mõjutab palju töötlemine mälu, ja mälumaht on saadaval rakenduse. See mõjutab ka funktsiooni Arvuta ja salvestusruumi maksumus. Allpool on suurim VM suurus DS sarja, DSv2 sari ja GS sarja kirjeldused.

| VM suurus | Protsessorituuma | Mälu | VM ketta suurus | Max. andmete ketast | Vahemälu maht | IOPS | Läbilaskevõime vahemälu IO piirangud |
|---|---|---|---|---|---|---|---|
| Standard_DS14 | 16 | 112 GB | OS = 1023 GB <br> Kohaliku SSD = 224 GB | 32 | 576 GB | 50 000 IOPS <br> 512 MB sekundis | 4000 IOPS ja 33 MB sekundis |
| Standard_GS5 | 32 | 448 GB | OS = 1023 GB <br> Kohaliku SSD = 896 GB | 64 | 4224 GB | 80 000 IOPS <br> 2000 MB sekundis | 5000 IOPS ja 50 MB sekundis |

Kõik saadaolevad Azure VM suurused täieliku loendi vaatamiseks vaadake [Windows VM suurused](../virtual-machines/virtual-machines-windows-sizes.md) ja [Linux VM suurused](../virtual-machines/virtual-machines-linux-sizes.md). Valige VM suurus, mis saab täita ja mastaapimiseks oma soovitud rakendus jõudluse nõuetele. Lisaks sellele arvestama jälgimise vajavateks VM suurused valimisel.


*Skaala piirangud*  
IOPS ülempiirid VM ja ketta kohta on erinevad ja üksteisest sõltumatult. Veenduge, et rakendus sunnib IOPS VM kui ka premium ketast, mis on seotud piires. Muul juhul rakenduse jõudluse kogemus pidurdamise.

Näiteks Oletame, et rakendus nõue kuni 4000 IOPS. Selleks saate ettevalmistamise P30 kettal DS1 VM. Kuni 5000 IOPS pakkuda P30 ketas. Siiski DS1 VM on piiratud 3200 IOPS. Seetõttu rakenduse jõudlus olla kitsendada VM limiit veebisaidil 3200 IOPS ja on jõudluse langemist. Valige selle olukorra vältimiseks VM ja ketta suurus, mis on nii rakenduse nõuetele.

*Toimingu kulu*  
Paljudel juhtudel on võimalik, et oma üldine kulu abil Premium mälu on väiksem kui kindlad abil.

Näiteks Kaaluge rakenduse nõudva 16000 IOPS. Selle jõudluse saavutamiseks peate Standard\_D14 Azure IaaS VM, mida saab anda maksimaalne IOPS, kasutades 32 kindlad 1TB ketast 16000. Iga 1TB salvestusruumi standard ketta võite saavutada kuni 500 IOPS. Prognoositud kulud selle VM kuus on $1,570. Igakuine kulu on 32 kindlad ketast on $ 1 638. Igakuine hinnanguline kogukulu on $3,208.

Juhul, kui te majutatud sama rakenduse Premium säilitamise kohta, peate VM väiksem ja vähem premium salvestusruumi ketast, vähendades kogukulu. Standard\_DS13 VM saate rahuldamiseks 16000 IOPS neli P30 plaati abil. DS13 VM on maksimaalne IOPS, 25600 ja iga P30 kettal on maksimaalne IOPS 5000. Kokkuvõttes selle konfiguratsiooni võite saavutada 5000 x 4 = 20000 IOPS. Prognoositud kulud selle VM kuus on $1,003. Igakuine kulu on neli P30 premium salvestusruumi plaati on $544.34. Igakuine hinnanguline kogukulu on $1,544.

Järgmises tabelis on kokkuvõte kulu jaotus selle stsenaariumi Standard ja Premium salvestusruumi.

| | **Standard** | **Premium** |
|---|---|---|
| **Kuus VM kulu** | $1,570.58 (standard\_D14)   | $1,003.66 (standard\_DS13) |
| **Kuus ketast kulu** | $1,638.40 (32 x 1 TB ketast) | $544.34 (4 x P30 ketast) |
| **Üldised kulud kuus**  | $3,208.98 | $1,544.34 |

*Linux Distros*  

Azure Premium Storage, saate samal tasemel vms, kus töötab Windows ja Linux. Toetame palju maitsed Linux distros ja täieliku loendi leiate [siit](../virtual-machines/virtual-machines-linux-endorsed-distros.md). See on oluline märkida erinevate distros sobivad paremini erinevat tüüpi töökoormus. Kuvatakse sõltuvalt teie töökoormus töötab distributsiooni jõudluse erinevaid tasemeid. Testige Linux distros oma rakendusega ja valige üks, mis sobib kõige paremini.


Kui operatsioonisüsteemi Linux Premium salvestusruumi, kontrollige, kas uusimate värskenduste kohta nõutav draiverid kõrge jõudluse tagamiseks.

## <a name="premium-storage-disk-sizes"></a>Premium salvestusruumi ketta suuruse  
Azure Premium mälu pakub kolme ketta suuruses praegu. Iga ketta suurus on erinevate skaala limiit IOPS, läbilaskevõime ja salvestusruumi. Valige õige Premium salvestusruumi ketta suurus sõltuvalt rakenduse nõuded ja suure ulatuse VM suurus. Järgmises tabelis antakse ülevaade kolme ketast suurused ja nende võimalusi.

| **Ketta tüüp**       | **P10**           | **20**           | **P30**           |
|---------------------|-------------------|-------------------|-------------------|
| Ketta suurus           | 128 giB           | 512 giB           | 1024 giB (1 TB)   |
| IOPS ketta kohta.       | 500               | 2300              | 5000              |
| Ühe ketta | 100 MB sekundis | 150 MB sekundis | 200 MB sekundis |

Mitu ketast sõltub ketta suuruse valitud. Võite kasutada oma rakenduse nõude täitmiseks P30 kettal ühe või mitme P10 ketast. Konto kaalutlused allpool valiku tegemisel arvesse võtta.

*Skaala piirangud (IOPS ja läbilaskevõime)*  
Iga Premium ketta suurusega IOPS ja läbilaskevõime piirangud on erinevad ja sõltumatu VM skaala piirangud. Veenduge, et kokku IOPS ja läbilaskevõime kaudu soovitud ketast on valitud VM suuruses skaala piiridesse.

Näiteks kui rakenduse nõue kuni 250 MB/sec läbilaskevõime ja kasutate DS4 VM ühe P30 ketta. DS4 VM saate anda kuni 256 MB/sec läbilaskevõime. Ühe P30 ketta on siiski läbilaskevõime kuni 200 MB sekundis. Seega on piiratud rakendus, 200 MB sekundis tõttu ketta piirata. Selle piirmäära lahendada, säte rohkem kui üks andmete ketast, et VM.

>**Märkus:**  
>Loeb kätte vahemälu on kaasatud ketas IOPS ja läbilaskevõime, seega ei rakendata kettale piirangute. Vahemälu on selle eraldi IOPS ja läbilaskevõime limiidist VM.
>
>Näiteks algselt oma loeb ja kirjutab on 60MB sekundis 40MB sekundis vastavalt ja. Aja jooksul, vahemälu soojeneb ja pakub rohkem ja rohkem loeb vahemälu. Seejärel saate avada kõrgema kirjutamine läbilaskevõime kettalt.

*Arvu ketast.*  
Määratleda ketast, peate rakenduse nõuded hindamise abil. Iga VM maht on ka ketast, mille saate manustada VM arv on piiratud. Tavaliselt on kaks korda valdkond arv. Veenduge, et valite VM suuruse toetab ketast vaja arv.

Pidage meeles, et Premium salvestusruumi ketast on suurem jõudluse võimalusi võrreldes kindlad ketast. Seetõttu, kui migreerite rakenduse Azure IaaS VM abil Standard salvestusruumi juurde Premium salvestusruumile, tõenäoliselt peate vähem premium ketast saavutamiseks rakenduse jõudlus sama või uuem versioon.

## <a name="disk-caching"></a>Ketta vahemällu  
Kõrge skaala VMs, mida kasutada Azure Premium mälu on mitmekihilise vahemällu tehnoloogia BlobCache. BlobCache kasutab kombinatsiooni virtuaalse masina RAM- ja kohaliku SSD vahemällu. Vahemälu on saadaval Premium salvestusruumi püsivate ketast ja VM kohaliku ketast. Vaikimisi see vahemälu säte on lugemis-ja kirjutamisõigusega OS ketast ja kirjutuskaitstud jaoks andmete ketast hostitud Premium mälu. Ketta vahemällu Premium salvestusruumi kettale lubatud, saate kõrge skaala VMs saavutada väga kõrge jõudluse aluseks jõudluse ületavaid.

>[AZURE.WARNING] Mõne Azure ketta vahemälu sätte rebib ja uuesti manustab target ketas. Kui see on operatsioonisüsteem ketas, VM taaskäivitamist. Peatage kõik rakendused ja teenused, mida selle katkemist enne ketta vahemälu sätte muutmine võib mõjutada.

BlobCache tööpõhimõtete kohta lisateabe saamiseks vaadake sees [Azure Premium Storage](https://azure.microsoft.com/blog/azure-premium-storage-now-generally-available-2/) ajaveebipostituse.

On oluline, et võimaldada vahemälu ketast kogum. Kas tuleks lubada ketta vahemällu kettal premium või mitte sõltub töökoormus mustri ketta kuvatakse töötlemise. Järgmises tabelis antakse ülevaade vaikimisi OS- ja ketast vahemälusätted.

| **Ketta tüüp** | **Vahemälu vaikesäte** |
|---|---|
| OS kettale | ReadWrite |
| Andmete kettale | Ükski |

Järgnevalt on soovitatav ketta vahemälu sätete andmete ketast,

| **Ketta vahemällu säte** | **Soovituse kohta, millal seda sätet kasutavad** |
|---|---|
| Ükski | Konfigureerida host-vahemälu pole ainult kirjutamiseks ja kirjutage-raske ketast jaoks. |
| Kirjutuskaitstud | Kirjutuskaitstud ja lugemis-ja kirjutamisõigusega ketast jaoks konfigureerida kirjutuskaitstud host-vahemälu. |
| ReadWrite | Konfigureerida host-vahemälu ReadWrite ainult juhul, kui teie rakendus õigesti tagab vahemällu talletatud andmete kirjutamise püsivate ketast vastavalt vajadusele. |

*Kirjutuskaitstud*  
Konfigureerimisega kirjutuskaitstud Premium mälu andmete vahemällu talletamise ketast, saate saavutada madal lugemine latentsus ja rakenduse väga kõrge lugemine IOPS ja läbilaskevõime saamiseks. See on kahel põhjusel, tähtaja

1.  Loeb läbi vahemälust, mis on kohalik SSD ja VM mälu, on palju kiirem kui loeb andmete ketas, mis on Azure'i bloobimälu.  
2.  Premium mälu ei loe, loeb serveeritud vahemälu kettale IOPS poole ja läbilaskevõime. Seega teie taotlus on võimalik saada suurema kogusumma IOPS ja läbilaskevõime.

*ReadWrite*  
Vaikimisi on OS ketast ReadWrite vahemällu lubatud. Oleme lisanud toe ReadWrite ketast ka andmete vahemällu. Kui kasutate vahemällu ReadWrite, peab teil olema õige kirjutamise andmete vahemälust püsivate ketast. Näiteks SQL serveri käsitleb püsivate salvestusruumi ketast vahemällu talletatud andmete kirjutamise ise. Rakendus, mis ei oska püsib nõutavad andmed ReadWrite vahemälu kasutamine võib põhjustada andmete kaotsimineku korral VM.

Näiteks saate rakendada neid juhiseid SQL Server töötab Premium salvestusruumi, tehes järgmist

1.  Konfigureerimine "Kirjutuskaitstud" vahemälu premium salvestusruumi kettale majutusteenuse andmefailid.  
    lisamine.  Kiire loeb vahemälu alumise SQL serveri päringu aeg, kuna andmete lehed on toodud kiiremini vahemälust võrreldes otse ketast.  
    b.  Serveeritakse loeb vahemälust, tähendab, et täiendavad läbilaskevõime on saadaval premium andmete ketast. SQL serveri saate kasutada seda täiendavad läbilaskevõime suunas toomine andmete lehti ja muid toiminguid nagu varundus ja taaste, partii laadimise ja indeks luuakse.  
2.  Konfigureerimine "Pole" vahemälu premium salvestusruumi kettale majutusteenuse logifailid.  
    lisamine.  Logifailide on peamiselt kirjutamine-raske toimingud. Seetõttu nad pole kasu kirjutuskaitstud vahemälu.

## <a name="disk-striping"></a>Ketas Striping  
Kui kõrge skaalaga VM on lisatud mitu premium salvestusruumi püsivate ketast, saate liita nende IOPs, läbilaskevõime ja mälumaht on ketast koos triibuline.

Opsüsteemis Windows, saate salvestusruum triip ketast koos. Konfigureerige ühe veeru iga ketta pool. Muul juhul musta Triibutatud helitugevuse üldise jõudluse võib olla madalam, liikluse üle soovitud ketast ebaühtlane jaotus tõttu.

Tähtis: Kasutades Server Manager UI, saate määrata kuni 8 musta Triibutatud maht veergude arv. Rohkem kui 8 ketast manustamisel PowerShelli abil saate luua maht. PowerShelli abil saate veerge võrdne arv ketast. Näiteks kui on 16 ketast kogumi ühe triip Määrake *Uus* VirtualDiskcmdlet PowerShelli *NumberOfColumns* parameetri 16 veergu.


Linux, kasutage MDADM kasuliku triip ketast koos. Üksikasjalikud juhised striping kettale Linux viidata [Konfigureerimine tarkvara RAID Linux](../virtual-machines/virtual-machines-linux-configure-raid.md).


*Triip suurus*  
Ketas striping on oluline konfigureerimine on triip suurus. Riba suuruse või ploki suurus on väikseim andmekogumi, mille rakendus võite käsitleda musta Triibutatud maht. Saate konfigureerida triip suurus sõltub tüüp ja selle taotlus muster. Kui valite vale triip suurus, viia IO vastuolud, mis viib rakenduse jõudluse langemist.

Näiteks kui taotluse IO rakenduse loodud on suurem kui ketta triip suurus, salvestusruumi süsteemi kirjutab see üle triip ühiku piiride rohkem kui üks ketas. Kui see on selle andmetele juurdepääs aega, on tal taotlemine üle rohkem kui üks triip üksused taotluse lõpuleviimiseks. Kumulatiivse mõju selline käitumine võib kaasa tuua olulisi jõudluse vähenemine. Teisalt, kui IO taotluse maht on väiksem kui triip suurus ja kui see on juhuslik laadi, IO taotlusi võib lisada samale kettale põhjustab kitsaskoht ja lõpuks alandav IO jõudlus.


Olenevalt teie rakendus töötab töökoormus, sobiv triip suuruse valimine. Kasutage juhusliku small IO taotluste triip väiksem. Suure järjestikku IO jaoks taotlusi kasutavad triip suuremaks. Siit saate teada triip suurus soovitused rakenduse näitate Premium säilitamise kohta. SQL Server, konfigureerida triip 64KB OLTP töökoormus ja andmete tolliladustamise töökoormus 256KB suurust. Vaadake lisateavet [jõudluse head tavad SQL Server Azure'i VMs](../virtual-machines/virtual-machines-windows-sql-performance.md#disks-and-performance-considerations) .


>**Märkus:**  
>Saate triip koos kuni 32 premium salvestusruumi ketast DS rea VM ja 64 premium salvestusruumi ketast GS rea VM.

## <a name="multi-threading"></a>Lõimtöötlusele  
Azure'i on välja töötatud Premium salvestusruumi platvormi oluliselt paralleelselt. Seetõttu mitmetoimeline rakenduse saavutab palju suurema jõudluse Kui ühelõimelised rakendus. Mitmetoimeline rakenduse üle mitme lõime üles oma ülesandeid jagab ja suurendab tõhusust täitmise VM ja ketas ressursside maksimaalselt kasutades.

Näiteks kui teie rakendus töötab ühe core VM abil kaks Teemad, CPU saate vaheldumisi kaks Teemad tõhusust saavutamiseks. Samal ajal, kui üks teema ootavad kettal IO lõpuleviimiseks, saab vahetada CPU teiste teemat. Nii saate kaks Teemad täita rohkem kui ühte teemat. Kui VM on rohkem kui üks tuum, seda täpsemaks väheneb Käitusaeg Kuna iga core saate paralleelselt ülesannete täitmiseks.

Te ei saa muuta seda, kuidas rakenduse valmis rakendab ühe väliskeermestamiseks või lõimtöötlusele. Näiteks SQL serveri suudab mitme CPU ja mitme tuumaga. Kuid SQL serveri otsustab, millistel tingimustel see kogusummaks ühe või mitme Teemad töödelda päringu. See päringute sooritamine ja registrite abil lõimtöötlusele koostamine. Päring, mis hõlmab liitumisel suurte tabelite ja kasutajale tagasi andmete sortimine, SQL Server kasutab tõenäoliselt mitme teemad. Kasutaja ei saa siiski kontrollida, kas SQL serveri käivitab päringu abil lõime ühe või mitme teemad.

On konfiguratsioonisätted, mis saate muuta selle mõjutada lõimtöötlusele või paralleelselt rakenduse töötlemine. Näiteks SQL serveri korral on lubatud määral, paralleelsus konfigureerimine. See säte nimega MAXDOP, saate konfigureerida SQL serveri saate kasutada siis, kui paralleelselt protsessorite arvu ülempiir töötlemine. MAXDOP saate konfigureerida üksikute päringute või registri toimingute jaoks. See on kasulik, kui soovite saldo teie süsteemi jõudluse kriitilise tähtsusega ressursid.

Oletame näiteks, SQL serveri kasutamine rakenduse teostab suurele Päringukujunduse ja samal ajal toimingu indeks. Oletame, et soovisite registri toimingu olla rohkem kiire võrreldes suure päringu. Sellisel juhul saate seada MAXDOP väärtus olema suurem MAXDOP päringu registri toimingu. Sellisel viisil SQL Server on veel protsessorite, mis seda saate kasutada seda saate suunata suurele Päringukujunduse protsessorite arvu võrreldes registri toimimiseks arvu. Pidage meeles, et saate määrata SQL Server kasutab iga toimingu arvu. Saate määrata eesmärk on jätkuvalt jaoks protsessorite arvu ülempiir lõimtöötlusele.

Lugege lisateavet [Kraadi, paralleelsus](https://technet.microsoft.com/library/ms188611.aspx) SQL serveris. Siit saate teada selliseid sätteid, mis mõjutavad lõimtöötlusele rakenduse ja nende konfiguratsioone optimeerida jõudlust.

## <a name="queue-depth"></a>Järjekorda sügavus  
Järjekorda sügavus või järjekorra pikkus või järjekorra on süsteemi IO taotluste arv. Järjekorda sügavus väärtus määratleb mitu IO toimingute rakenduse saate joondada, mis salvestusruumi ketast on töötlemine. See mõjutab kõiki kolme rakenduse tootluse võtmenäitajate me kirjeldatud selles artiklis nt IOPS, läbilaskevõime ja latentsusaeg.

Järjekorda sügavus ja lõimtöötlusele on omavahel lähedalt seotud. Järjekorda sügavus väärtus näitab, kui palju lõimtöötlusele võib järgi saadud. Kui järjekorda sügavus on suur, rakenduse saab käivitada veel toiminguid üheaegselt, teisisõnu rohkem lõimtöötlusele. Kui järjekorda sügavus on väike, isegi juhul, kui rakendus on mitmetoimeline, ei ole piisavalt taotlusi samaaegne täitmine loendit.

Tavaliselt valmis rakenduste ei luba teil muuta järjekorda sügavus, kuna kui määramine valesti seda tuleb teha rohkem kahju kui kasu. Rakenduste seab järjekorda sügavus saada optimaalse jõudluse tagamiseks õige väärtus. Siiski on oluline mõista selle kontseptsiooni, nii et saate otsida oma rakendusega jõudlusprobleeme. Saate jälgida ka järjekorda sügavus efektid, käitades võrdleva tööriistad teie süsteemi.

Mõned rakendused pakuvad mõjutada järjekorda sügavus sätted. Näiteks MAXDOP (kuni taseme paralleelsus) sätet SQL serveri eelmises jaotises kirjeldatud. MAXDOP on võimalus mõjutada järjekorda sügavus ja lõimtöötlusele, kuigi see ei muuda otse SQL serveri järjekorda sügavus väärtus.

*Kõrge järjekorda sügavus*  
Kõrge järjekorda sügavus jooned üles kettal rohkem toiminguid. Ketas teab järgmise kutse tema järjekorras ette valmistada. Seetõttu ketas ette valmistada toimingute ajastamine ja töödelda ka optimaalse järjestuses. Kuna rakendus saadab rohkem taotlusi ketas, saab ketas protsessi veel paralleelselt iOS-i. Kokkuvõttes saab rakenduse kõrgema IOPS saavutamiseks. Kuna rakendus on veel taotluste, suureneb ka kogu läbilaskevõime taotluse.

Tavaliselt rakenduse võite saavutada maksimaalse läbilaskevõime 8 – 16 + tasumata IOs manustatud ketta kohta. Kui järjekorda sügavus on üks, rakendus on nõudnud piisavalt iOS-i süsteemis ja see töötleb vähem antud perioodi kohta. Teisisõnu, väiksem jõudlus.

Näiteks SQL serveri, MAXDOP väärtus "4" päringu seadmine teavitab SQL serveri, et see saate kasutada kuni nelja südamikud päringu. SQL serveri määravad, mis on parim järjekord sügavus väärtus ja päringu täitmise valdkond number.

*Optimaalse järjekorda sügavus*  
Väga kõrge järjekord sügavus väärtus on ka oma puudused. Kui järjekord sügavus väärtus on liiga suur, proovib rakenduse väga kõrge IOPS juhtida. Juhul, kui rakendus on piisavalt ettevalmistatud IOPS koos püsivate ketast, võib see negatiivselt mõjutada rakenduse latentsused. Järgmise valemi kuvatakse IOPS, latentsus ja järjekorda sügavus vaheline seos.  
    ![](media/storage-premium-storage-performance/image6.png)

Peaksite konfigureerima pole järjekorda sügavus mis tahes suure väärtusega, kuid optimaalse väärtuse, mis võib tuua piisavalt IOPS rakenduse latentsused mõjutamata. Näiteks kui rakenduse latentsus peab olema 1 millisekundilist, järjekorda sügavus 5000 IOPS saavutamiseks on, üks kord päevas = 5000 x 0,001 = 5.

*Järjekorda sügavus musta Triibutatud maht*  
Musta Triibutatud maht, pidama piisavalt järjekorda sügavus selliselt, et iga kettal on tippväärtus järjekorda sügavus ükshaaval. Näiteks kaaluge rakendus, mis sunnib järjekorda sügavus 2 ja 4 ketast on selle triip. Kahe IO taotlused lähevad kaks plaati ja allesjäänud kaks plaati saab jõude. Seetõttu konfigureerimine järjekorda sügavus nii, et kõik ketast võib olla hõivatud. Valem näitab, kuidas järjekorda sügavus musta Triibutatud maht kindlaks teha.  
    ![](media/storage-premium-storage-performance/image7.png)

## <a name="throttling"></a>Ahendamine  
Azure Premium mälu sätete määratud arv IOPS ja läbilaskevõime sõltuvalt VM suurused ja valite ketta suurused. Iga kord teie rakendus proovib drive IOPS või läbilaskevõime kohal need piirangud, mis VM või ketta võite hakkama, throttle Premium salvestusruumi, seda. See teatab oma rakenduse jõudluse langemist. See tähendab, et kõrgema latentsus, vähendage läbilaskevõime või IOPS langetada. Kui Premium salvestusruumi pole throttle, võib rakenduse nurjuda täielikult ületades oma ressursid on saavutamiseks. Jah, tõttu pidurdamise jõudlusprobleemide vältimiseks alati ettevalmistamise piisavalt ressursse rakenduse. Arvesse võtta, mida me arutada VM suurused ja ketta suuruses jaotiste ülaltoodud. Võrdlemine on parim viis aru saada, millised ressursid on vaja majutada oma rakenduse.

## <a name="benchmarking"></a>Võrdlemine  
Võrdlemine on simuleerida erinevate töökoormus oma taotlus ja mõõtmise rakenduse jõudlus jaoks iga töökoormus. Varem kirjeldatud juhised on kogunenud rakenduse jõudluse nõuetele. Käivitades võrdleva tööriistad majutusteenuse rakenduse VMs, saate määrata rakenduse võite saavutada Premium Storage soovitud jõudluse tasemed. Selles jaotises pakume teile näiteid võrdlemine Standard DS14 VM Azure Premium Storage ketast ette valmistatud.

Oleme kasutanud levinud võrdleva tööriistad Iometer ja FIO, Windowsi ja Linux vastavalt. Nende tööriistade kudema mitme lõime simuleerida tootmise nagu töökoormus ja süsteemi jõudluse mõõtmiseks. Tööriistade abil saate konfigureerida ka parameetrid nagu Blokeeri suurus ja järjekorda sügavus, mida te tavaliselt ei saa muuta rakenduse. See pakub rohkem paindlikkust juhtida maksimaalse jõudluse kõrge skaalal VM ette valmistatud premium ketast erinevat tüüpi rakenduse töökoormus. Lugege lisateavet iga võrdleva tööriista külastage [Iometer](http://www.iometer.org/) ja [FIO](http://freecode.com/projects/fio).

Järgige alltoodud näiteid, luua standardne DS14 VM ja manustamine 11 Premium salvestusruumi ketast VM. 11 ketast, konfigureerimine 10 ketast koos vahemällu nimega "Pole" ja need nimetatakse NoCacheWrites mahu triip. Konfigureerige host vahemällu nimega "Kirjutuskaitstud" ülejäänud kettal ja luua nimega Vahemälu lugemisi selle ketta maht. See häälestuse abil saab näha kuni lugemine ja kirjutamine jõudlust Standard DS14 VM. Üksikasjalikud juhised premium ketast DS14 VM loomise kohta, minge [loomine ja kasutamine Premium salvestusruumi konto virtuaalse masina andmete jaoks](storage-premium-storage.md#create-and-use-a-premium-storage-account-for-a-virtual-machine-data-disk).

*Soojendamine vahemälu*  
Kirjutuskaitstud host vahemällu kettale saab anda suurema IOPS, kui ketas. Saada selle maksimaalse loetuks jõudluse host vahemälu, esmalt peate soe üles selle ketta vahemälu. See tagab, et millist analüüsi tööriist, suunatakse Vahemälu lugemisi draivil lugemine iOS-i tegelikult tabab vahemälu ja pole ketast otse. Täiendavad IOPS ühe vahemälu vahemälu tabamust tulemi lubatud ketas.

>**Tähtis:**  
>Peate enne käivitamist võrdlemine, iga kord, kui VM taaskäivitatakse üles vahemälu soe.

#### <a name="iometer"></a>Iometer   
[Laadige alla tööriist Iometer](http://sourceforge.net/projects/iometer/files/iometer-stable/2006-07-27/iometer-2006.07.27.win32.i386-setup.exe/download) VM.

*Test faili*  
Iometer kasutab testi faili, mis on talletatud aluseks võrdleva katse käivitate. See juhib loeb ja kirjutab Proovifail mõõta ketas IOPS ja läbilaskevõime. Kui teil on antud üks, loob Iometer Proovifail. 200GB testi fail nimega iobw.tst draividel Vahemälu lugemisi ja NoCacheWrites loomine.

*Accessi tehnilised andmed*  
Tehnilised andmed, taotleda IO suurus % lugemis-ja kirjutamisõigusega, % juhusliku/järjestikku on konfigureeritud Iometer "Juurdepääs tehnilised andmed" vahekaarti kasutamine. Looge mõnda Accessi määratlus iga allpool kirjeldatud juhtudel. Loo Accessi tehnilised andmed ja "Salvesta" koos asjakohane nimi nagu – RandomWrites\_8K, RandomReads\_8K. Valige vastav täpsustus test stsenaarium käivitamisel.

Näide Accessi tehnilised andmed suurima kirjutamine IOPS stsenaariumi puhul on allpool  
    ![](media/storage-premium-storage-performance/image8.png)

*Suurim lubatud IOPS testi tehnilised andmed*  
Kasutada suurima IOPs näitamaks taotluse väiksem. Kasutage 8K taotluse suurus ja luua juhuslik kirjutab ja loeb.

| Accessi määratlus | Taotluse suurus | Juhusliku % | Lugege % |
|----------------------|--------------|----------|--------|
| RandomWrites\_8K     | 8K           | 100      | 0      |
| RandomReads\_8K      | 8K           | 100      | 100    |

*Maksimaalse läbilaskevõime testi tehnilised andmed*  
Kasutage maksimaalse läbilaskevõime näitamaks taotluse suuremaks. Kasutage 64K taotluse suurus ja luua juhuslik kirjutab ja loeb.

| Accessi määratlus | Taotluse suurus | Juhusliku % | Lugege % |
|----------------------|--------------|----------|--------|
| RandomWrites\_64K    | 64K          | 100      | 0      |
| RandomReads\_64K     | 64K          | 100      | 100    |

*Töötab Iometer Test*  
Tehke alltoodud vahemälu üles

1.  Looge kaks Accessi tehnilised andmed näidatud järgmises tabelis

  	| Nimi              | Taotluse suurus | Juhusliku % | Lugege % |
  	|-------------------|--------------|----------|--------|
  	| RandomWrites\_1MB | 1MB          | 100      | 0      |
  	| RandomReads\_1MB  | 1MB          | 100      | 100    |

2.  Käivitage Iometer katse lähtestamisel vahemälu ketas parameetritega järgmist. Kasutada kolme töötaja teemad target maht ja järjekorda sügavus 128. Test "Käitusaeg" kestuse määramine 2hrs vahekaardil "Testida Setup".

  	| Stsenaarium              | Target maht | Nimi              | Kestus |
  	|-----------------------|---------------|-------------------|----------|
  	| Vahemälu ketta lähtestamine | Vahemälu lugemisi    | RandomWrites\_1MB | 2hrs     |

3.  Käivitage Iometer test soojakud vahemälu ketas parameetritega järgmist. Kasutada kolme töötaja teemad target maht ja järjekorda sügavus 128. Test "Käitusaeg" kestuse määramine 2hrs vahekaardil "Testida Setup".

  	| Stsenaarium           | Target maht | Nimi             | Kestus |
  	|--------------------|---------------|------------------|----------|
  	| Soe vahemälu kettale häälestamine | Vahemälu lugemisi    | RandomReads\_1MB | 2hrs     |

Vahemälu kettale on soojenenud, jätkake testi stsenaariumid, mis on loetletud allpool. Iometer test, kasutage vähemalt kolme töötaja teemad **iga** target maht. Iga töötaja teemat, valige target maht, määrata järjekorda sügavus ja valige üks järgmistest salvestatud testi tehnilised andmed, nagu on näidatud järgmises tabelis, vastavate test stsenaarium käivitamiseks. Tabel näitab oodatavaid tulemusi ka IOPS ja läbilaskevõime, kui need testi. Kõik stsenaariumid, väike 8KB suurust IO ja kõrge järjekorda sügavus 128 kasutatakse.

| Test stsenaarium      | Target maht | Nimi              | Tulemus       |
|--------------------|---------------|-------------------|--------------|
| Max. Lugege IOPS     | Vahemälu lugemisi    | RandomWrites\_8K  | 50 000 IOPS  |
| Max. Kirjutage IOPS    | NoCacheWrites | RandomReads\_8K   | 64 000 IOPS  |
| Max. Ühendatud IOPS | Vahemälu lugemisi    | RandomWrites\_8K  | 100 000 IOPS |
|                    | NoCacheWrites | RandomReads\_8K   |              |
| Max. Lugege MB/sec   | Vahemälu lugemisi    | RandomWrites\_64K | 524 MB/sec   |
| Max. Kirjutage MB/sec  | NoCacheWrites | RandomReads\_64K  | 524 MB/sec   |
| Ühendatud MB/sec    | Vahemälu lugemisi    | RandomWrites\_64K | 1000 MB sekundis  |
|                    | NoCacheWrites | RandomReads\_64K  |              |

Allpool leiate selle Iometer kuvatõmmised testi tulemused kombineeritud IOPS ja läbilaskevõime stsenaariumid.

*Ühendatud loeb ja kirjutab maksimaalne IOPS*  
![](media/storage-premium-storage-performance/image9.png)

*Ühendatud loeb ja kirjutab maksimaalse läbilaskevõime*  
![](media/storage-premium-storage-performance/image10.png)

### <a name="fio"></a>FIO  
FIO on levinud vahend võrdlusalus salvestusruumi Linux VMs kohta. See on paindlikkuse erineva IO suurusega järjestikku valimiseks või juhusliku loeb ja kirjutab. Koeb töötaja teemad või protsesside määratud/v-toimingud teha. Saate määrata iga töötaja teemat peab tegema töö failide kasutamine/v-toimingud tüüp. Lõime üks töö fail stsenaarium, mis on näidatud allpool toodud näidetes kohta. Saate muuta neid töö faile võrrelda erinevate töökoormus töötavate Premium salvestusruumi kirjeldused. Näidetes kasutame töötab **Ubuntu**Standard DS 14 VM. Kasutage sama häälestamise kirjeldatud [võrdleva hindamise jaotise](#Benchmarking) algusse ja sooja vahemälu enne võrdleva testi.

Enne alustamiseks [FIO Laadige alla](https://github.com/axboe/fio) ja installige see arvuti virtuaalne.

Käivitage järgmine käsk Ubuntu,

        apt-get install fio

Kasutame nelja töötaja teemad juhtimiseks kirjutada toiminguid ja neli töötaja teemad juhtimise Read toimingud on kettale. Töötajate kirjutamine on olema liikluse mahu "nocache", mis on määratud "Pole" vahemälu 10 ketast. Lugege töötajate kuvatakse olema liikluse mahu "readcache", mis on 1 ketta vahemälu on seatud "Kirjutuskaitstud".

*Suurim lubatud kirjutamine IOPS*  
Luua töö faili saada kuni kirjutada IOPS järgmistele nõuetele. Pange nimi "fiowrite.ini".

```
[global]
size=30g
direct=1
iodepth=256
ioengine=libaio
bs=8k

[writer1]
rw=randwrite
directory=/mnt/nocache
[writer2]
rw=randwrite
directory=/mnt/nocache
[writer3]
rw=randwrite
directory=/mnt/nocache
[writer4]
rw=randwrite
directory=/mnt/nocache
```

Pange tähele jälgi olulisi asju, mis on kirjeldatud eelmiste jaotiste kujundus suuniste kooskõlas. Nende kirjeldused on oluline juhtida maksimaalne IOPS,  
-   256 sügavus kõrge järjekorda.  
-   Väikese ploki suurus 8 KB.  
-   Mitme lõime läbimiseks juhuslik kirjutab.

Käivitage järgmine käsk võrgukoosolekuga FIO test 30 sekundit,  

    sudo fio --runtime 30 fiowrite.ini

Ajal töötab katse, on võimalik vaadata arvu kirjutamine IOPS VM ja Premium ketast toetavad. Nagu on näidatud allpool valimi, on DS14 VM pakkuda oma suurima kirjutamine IOPS piirmäära 50 000 IOPS.  
    ![](media/storage-premium-storage-performance/image11.png)

*Suurim lubatud lugemine IOPS*  
Luua töö faili saada kuni lugemine IOPS järgmistele nõuetele. Pange nimi "fioread.ini".

```
[global]
size=30g
direct=1
iodepth=256
ioengine=libaio
bs=8k

[reader1]
rw=randread
directory=/mnt/readcache
[reader2]
rw=randread
directory=/mnt/readcache
[reader3]
rw=randread
directory=/mnt/readcache
[reader4]
rw=randread
directory=/mnt/readcache
```

Pange tähele jälgi olulisi asju, mis on kirjeldatud eelmiste jaotiste kujundus suuniste kooskõlas. Nende kirjeldused on oluline juhtida maksimaalne IOPS,

-   256 sügavus kõrge järjekorda.  
-   Väikese ploki suurus 8 KB.  
-   Mitme lõime läbimiseks juhuslik kirjutab.

Käivitage järgmine käsk võrgukoosolekuga FIO test 30 sekundit,

    sudo fio --runtime 30 fioread.ini

Kui test töötab, on võimalik loetud arv IOPS VM ja Premium ketast toetavad kuvamiseks. Nagu on näidatud allpool valimi, on DS14 VM pakkuda rohkem kui 64 000 lugemine IOPS. See on ketta ja jõudluse vahemälu kombinatsiooni.  
    ![](media/storage-premium-storage-performance/image12.png)

*Maksimum lugemine ja kirjutamine IOPS*  
Töö faili loomine abil järgmist tehnilised andmed saada maksimaalselt kombineeritud lugemine ja kirjutamine IOPS. Pange nimi "fioreadwrite.ini".

```
[global]
size=30g
direct=1
iodepth=128
ioengine=libaio
bs=4k

[reader1]
rw=randread
directory=/mnt/readcache
[reader2]
rw=randread
directory=/mnt/readcache
[reader3]
rw=randread
directory=/mnt/readcache
[reader4]
rw=randread
directory=/mnt/readcache

[writer1]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
[writer2]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
[writer3]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
[writer4]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
```

Pange tähele jälgi olulisi asju, mis on kirjeldatud eelmiste jaotiste kujundus suuniste kooskõlas. Nende kirjeldused on oluline juhtida maksimaalne IOPS,

-   Kõrge järjekorda sügavus 128.  
-   Väikese ploki suurus on 4KB.  
-   Mitme lõime läbimiseks juhusliku loeb ja kirjutab.

Käivitage järgmine käsk võrgukoosolekuga FIO test 30 sekundit,

    sudo fio --runtime 30 fioreadwrite.ini

Kui test töötab, saab näha arv kombineeritud lugemine ja kirjutamine IOPS VM ja Premium ketast toetavad. Nagu on näidatud allpool valimi, on üle 100 000 kombineeritud lugemine ja kirjutamine IOPS pakkuda DS14 VM. See on ketta ja jõudluse vahemälu kombinatsiooni.  
    ![](media/storage-premium-storage-performance/image13.png)

*Ühendatud maksimaalse läbilaskevõime*  
Saada maksimaalset kombineeritud lugemine ja kirjutamine läbilaskevõime, Blokeeri suuremaks ja suure järjekorda sügavus kasutamine mitme lõime läbimiseks loeb ja kirjutab. Saate blokeerida suurus 64 KB ja järjekorda sügavus 128.

## <a name="next-steps"></a>Järgmised sammud  

Lugege lisateavet Azure Premium Storage:

- [Premium mälu: Suure jõudlusega Azure virtuaalse masina töökoormus salvestusruum](storage-premium-storage.md)  

SQL Serveri kasutajad, lugege artikleid jõudluse heade tavade kohta SQL Server:

- [Jõudluse SQL Server Azure'i Virtuaalmasinates head tavad](../virtual-machines/virtual-machines-windows-sql-performance.md)
- [Azure Premium mälu pakub suurimat jõudlust SQL Server Azure'i VM](http://blogs.technet.com/b/dataplatforminsider/archive/2015/04/23/azure-premium-storage-provides-highest-performance-for-sql-server-in-azure-vm.aspx) 
