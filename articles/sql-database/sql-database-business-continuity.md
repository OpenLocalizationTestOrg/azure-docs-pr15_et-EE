<properties
   pageTitle="Pilvepõhine järjepidevuse – andmebaasi taastamine - SQL-andmebaasi | Microsoft Azure'i"
   description="Siit saate teada, kuidas Azure'i SQL-andmebaasi toetab cloud järjepidevuse ja andmebaasi taastamine ja aitab hoida olulise pilve rakendus."
   keywords="süsteemi järjepidevuse, cloud järjepidevuse, andmebaasi avariitaastet andmebaasi taastamine"
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/13/2016"
   ms.author="carlrab"/>

# <a name="overview-of-business-continuity-with-azure-sql-database"></a>Ettevõtte järjepidevus Azure'i SQL-andmebaasi ülevaade

See ülevaade kirjeldatakse võimalusi järjepidevuse ja avariitaastet leiate Azure'i SQL-andmebaasi. Pakub suvandid, soovitused ja õpetused taastamise häiriva sündmused, mis võivad põhjustada andmete kaotsimineku või oma andmebaasi ja rakenduse kättesaamatuks muutuda. Arutelu sisaldab, mida teha, kui kasutaja või rakenduse viga mõjutab andmetervikluse, Azure piirkond on ka katkestuste või rakenduse jaoks on vaja hooldustööd. 

## <a name="sql-database-features-that-you-can-use-to-provide-business-continuity"></a>SQL-andmebaasi funktsioonid, mida saate esitada järjepidevuse tagamine

SQL-andmebaasi pakub business järjepidevuse, sh automaatse varundamise funktsiooni ja valikuline andmebaasi tiražeerimine. Iga omadused erinevad hinnangulise aja (ERT) ja viimaste kannete andmekadu. Kui teil mõista nende suvandite, saate valida nende vahel - ja, enamikul neid kasutada koos erinevaid stsenaariumeid lähemalt. Kui teil tekib oma äriplaani järjepidevuse, vaja mõista suurim lubatud aja enne rakenduse täielikult taastab pärast häiriva sündmuse – see on teie taastamise aja eesmärk (RTO). Samuti peate aru suurt hulka tehtud andmete värskendamine (ajavahemik) rakenduse saab luba kaotada, kui taastamisel pärast häiriva sündmus - taastamine punkti eesmärk (RPO). 

Järgmises tabelis võrreldakse ERT ja RPO kolm kõige levinumad stsenaariumid.

| Võimalus |  Tavaline taseme | Standardse taseme  | Premium taseme |
|---|---|---|---|
| Punkti aja taastamine varukoopia põhjal | Mis tahes taastatava punkti 7 päeva jooksul   | Mis tahes taastatava punkti 35 päeva jooksul  | Mis tahes taastatava punkti 35 päeva jooksul |
Geo kopeeritud varukoopiate põhjal taastamine Geo | ERT < 12h, RPO < 1h   | ERT < 12h, RPO < 1h   | ERT < 12h, RPO < 1h |
|Aktiivse Geo-kopeerimine | ERT < 30.aastail RPO < 5s   | ERT < 30.aastail RPO < 5s | ERT < 30.aastail RPO < 5s |


### <a name="use-database-backups-to-recover-a-database"></a>Andmebaasi varundamine abil saate andmebaasi taastamine

SQL-andmebaasi teostab automaatselt täieliku andmebaasi varundamine iga nädal, erinevat kombinatsiooni andmebaasi kaitsta oma ettevõtte andmete kaotsimineku varukoopiate tunni ja tehingu varukoopiate iga viie minuti järel. Nende varukoopiate on talletatud kohalik liigsete salvestusruumi 35 päeva andmebaaside Standard- ja Premium teenuse astme ja andmebaaside põhiteenused astme seitse päeva - [teenus astme](sql-database-service-tiers.md) üksikasjalikumat teavet leiate teemast teenuse astme. Kui teie teenuse kiht säilitatakse ei vasta teie ettevõtte vajadustele, saate suurendada, [muutes teenuse kiht](sql-database-scale-up.md)säilitusperiood. Varukoopiate on ka kopeeritud [andmepunktipaaride andmekeskuse](../best-practices-availability-paired-regions.md) kaitseks andmete keskmist katkestuste - täielik ja erinevat andmebaasi leiate lisateavet [automaatse andmebaasi varundamine](sql-database-automated-backups.md) .

Saate neid automaatse andmebaasi varundamine andmebaasi taastamine häiriva ürituste oma andmekeskuse ja teise andmekeskuse. Kasuta automaatset andmebaasi varundamine, taastamine eeldatav aeg sõltub mitmest tegurist, sealhulgas andmebaaside taastamisel piirkonna samal ajal, andmebaasi mahtu, tehingulogi mahu ja võrgu läbilaskevõime koguarv. Enamikul juhtudel taastamise aeg on vähem kui 12 tundi. Kui teise alale taastamisel, võimaliku andmekao on piiratud 1 tund, geograafilise liigne talletamist kord tunnis erinevat andmebaasi varundamine. 

> [AZURE.IMPORTANT] Automaatse varundamise funktsiooni abil taastamiseks peab olema liikme SQL serveri kaasautori roll või tellimuse omanik – lugege teemat [RBAC: sisseehitatud rollid](../active-directory/role-based-access-built-in-roles.md). Saate taastada Azure portaali, PowerShelli või REST API-ga. Ei saa kasutada Transact-SQL-i.

Kasutada automatiseeritud varukoopiate business järjepidevus ja taastamise süsteem kui rakenduse:

- Ei loeta ülesanne kriitiline.
- Ei sisalda sidumine SLA, seetõttu on tulemuseks tööseisakute 24 tundi või rohkem kohustuse.
- On madal, andmete muutmine (madala tehingud tunnis) ja kaotanud kuni tund muutmine on on aktsepteeritav, andmete kaotsimineku. 
- Hind on tundlik. 

Kui teil on vaja kiirem taastamise, kasutage [Aktiivse Geo-kopeerimine](sql-database-geo-replication-overview.md) (arutada edasi). Kui peate saama üle 35 päeva jooksul andmete taastamine, kaaluge nende arhiivimist oma andmebaasi regulaarselt BACPAC faili (tihendatud faili, mis sisaldab teie andmebaasi skeemi ja seotud andmed) salvestatud Azure'i bloobimälu või mõne muu soovitud kohta. Ülekandega ühtse andmebaasi arhiivi loomise kohta leiate lisateavet teemast [andmebaasi luua](sql-database-copy.md) ja [eksportimine andmebaasi koopia](sql-database-export.md). 

### <a name="use-active-geo-replication-to-reduce-recovery-time-and-limit-data-loss-associated-with-a-recovery"></a>Aktiivse Geo-kopeerimine abil vähendada taastamise aeg ja piirata andmekadu seotud enam

Lisaks Skype'i ärirakenduse katkemise korral andmebaasi taastamine andmebaasi varundamine, saate [Aktiivse Geo-kopeerimine](sql-database-geo-replication-overview.md) on kuni nelja loetavad teisene andmebaaside piirkondades teie valitud andmebaasi konfigureerimine. Teisene andmebaasidele püsivad sünkroonitud esmane andmebaas on asünkroonne dispersioonanalüüs süsteemi abil. Seda funktsiooni kasutatakse business katkemise korral andmete keskmist katkestuste või rakenduse täiendamine ajal kaitsta. Aktiivse Geo-kopeerimine saate kasutada ka päringu töökindluse kirjutuskaitstud päringutega geograafiliselt hajutatud pakkumiseks.

Kui esmane andmebaas läheb ühenduseta ootamatult või teil on vaja seda ühenduseta hooldustegevused, saate kiiresti reklaamida teisese muutuvad (nimetatakse ka Tõrkesiirde) esmast ja konfigureerimiseks rakenduste äsja esiletõstetud esmase ühenduse. Kavandatud Tõrkesiirde, ei kaota võike andmed. Planeerimata Tõrkesiirde, mis võib olla mõni väike andmekao asünkroonne dispersioonanalüüs tõttu väga hiljutine tehingute puhul. Pärast Tõrkesiirde, saate hiljem failback – kas kava või kui andmekeskuse tuleb tagasi online. Kõigil juhtudel kasutajate kogemusi tööseisakute väike ja Ühenda. 

> [AZURE.IMPORTANT] Aktiivse Geo-kopeerimine kasutamiseks peate tellimuse omanik või SQL serveri administraatoriõigusi. Saate konfigureerida ja Azure portaali, PowerShelli või REST API kasutamise õigused tellimust või Transact-SQL-i abil Tõrkesiirde pääsuõiguste SQL serveri abil.

Aktiivse Geo-kopeerimine kui rakenduse vastab mõnele kriteeriumide kasutamine

- Äärmiselt oluline on ülesanne.
- On teenuse taseme leping (SLA), mis ei võimalda 24 tundi või rohkem tööseisakute.
- Tööseisakute tulemuseks kohustuse.
- Kas kõrge andmete muutmine on suur ja kaotamise tunni andmeid ei ole lubatud.
- Aktiivse geo-kopeerimine täiendavad maksumus on väiksem kui võimalik kohustuse ja seotud kadu business.

## <a name="recover-a-database-after-a-user-or-application-error"></a>Pärast kasutaja või rakendus viga andmebaasi taastamine

* Keegi pole täiuslik! Kasutaja võib kogemata kustutada mõned andmed, tahtmatult kukutage on oluline tabel või isegi kukutage kogu andmebaasist loobumine. Või rakendus võib kogemata üle kirjutada hea andmete vigaste andmete rakenduse vigu. 

Selle stsenaariumi korral neid oma taastesuvandeid.

### <a name="perform-a-point-in-time-restore"></a>Teha punkti õigel ajal taastamine

Automaatse varundamise funktsiooni abil saate taastada andmebaasi teadaolev hea punkti koopia ajal, tingimusel, et aeg on andmebaasi säilitamise aja jooksul. Kui andmebaas on taastatud, saate asendada algse andmebaasi taastatud andmebaasi või kopeerige vajalikud andmed taastatud andmed algse andmebaasi. Kui andmebaas kasutab aktiivse Geo-kopeerimine, soovitame taastatud koopia algse andmebaasi nõutavad andmed kopeerida. Kui asendate algse andmebaasi taastatud andmebaasiga, pead taaskonfigureerimine ja sünkroonige uuesti aktiivse Geo-kopeerimine (mis võib kuluda juba mõnda aega suure andmebaasi jaoks). 

Lisateavet ja üksikasjalikud juhised andmebaasi taastamine kohani, kellaaja Azure portaali kaudu või kasutades PowerShelli, vt [punkti õigel ajal taastada](sql-database-recovery-using-backups.md#point-in-time-restore). Te ei saa taastada Transact-SQL-i abil.

### <a name="restore-a-deleted-database"></a>Kustutatud andmebaasi taastamine

Kui andmebaas on kustutatud, kuid pole loogiline server kustutatud, saate taastada kustutatud andmebaas on kustutatud punkti. See taastab andmebaasi varukoopia sama loogilise SQL serveriga, millest on kustutatud. Saate taastada, kasutades algse nimega või sisestage uus nimi või taastatud andmebaasi.

Lisateavet ja üksikasjalikud juhised kustutatud andmebaasi taastamine Azure'i portaalis või PowerShelli, kaudu vaadata [Kustutatud andmebaasi taastamine](sql-database-recovery-using-backups.md#deleted-database-restore). Te ei saa taastada Transact-SQL-i abil.

> [AZURE.IMPORTANT] Kui loogiline server on kustutatud, ei saa taastada kustutatud andmebaasi. 

### <a name="import-from-a-database-archive"></a>Andmebaasi arhiivi importimine

Kui andmete kaotsimineku ilmnenud väljaspool automatiseeritud varufailide praeguse säilitusperiood ja teil on arhiivimine andmebaasi, saate [kasutada arhiivitud BACPAC faili importimine](sql-database-import.md) uue andmebaasi. Selles etapis saate asendada algse andmebaasi imporditud andmebaas või kopeerige vajalikud andmed imporditud andmete algse andmebaasi. 

## <a name="recover-a-database-to-another-region-from-an-azure-regional-data-center-outage"></a>Andmebaasi taastamine teise alale on Azure piirkondliku andmete keskmist katkestuste

<!-- Explain this scenario -->

Kuigi harva, on Azure andmekeskuse võib olla mõni katkestuste. Kui ilmneb mõni katkestuste, põhjustab business häirete, mis võib ainult viimati paar minutit või võib kesta tundi. 

- Üheks võimaluseks on tulla võrguühenduse taastamisel andmed keskmist katkestuste korral üle andmebaasi ootama. See toimib rakendusi, mis on ühenduseta andmebaasi endale. Näiteks projekti või tasuta prooviversioon ei pea pidevalt töötada. Kui ka katkestuste andmekeskuses, ei teate, kui kaua on katkestuste kestab, nii, et see suvand toimib ainult siis, kui te ei vaja veidi andmebaasi.
- Teine võimalus on kas Tõrkesiirde teise andmepiirkond kui kasutate aktiivse Geo-kopeerimine või taastada, kasutades geograafilise liigne andmebaasi varundamine (Geo taastamine). Tõrkesiirde võtab aega ainult mõne sekundi, varukoopiate põhjal taastamine võtab tundi.

Kui võtate toimingu, palju aega kulub teil taastada ja kui palju teil tekkida andmete keskmist katkestuste korral andmekao sõltub sellest, kuidas te otsustate kasutada äri järjepidevus funktsioonid rakenduse eespool kirjeldatud. Tõepoolest, võite kasutada andmebaasi varundamine ja aktiivse Geo kopeerimine sõltuvalt teie taotluse nõuded kombinatsiooni. Arutelu rakenduste kujundamine eraldiseisev andmebaasid ja nende äri järjepidevus funktsioonide kasutamise elastne kaustu, vt [kujundus cloud avariitaastet taotlus](sql-database-designing-cloud-solutions-for-disaster-recovery.md) ja [elastne rakenduskausta katastroofi taastamine strateegiad](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).

Järgmistes jaotistes ülevaate juhiseid taastada andmebaasi varundamine või aktiivse Geo-kopeerimine. Üksikasjalikud juhised, sealhulgas nõudeid, postituse taastamise juhiseid ja teavet selle kohta, kuidas mõni katkestuste jäljendamiseks katastroofi taastamine drill sooritamiseks, lugege teemat [on katkestuste SQL-i andmebaasi taastamine](sql-database-disaster-recovery.md).

### <a name="prepare-for-an-outage"></a>Mõne katkestuste ettevalmistamine

Sõltumata business järjepidevuse funktsiooni saate kasutada, peate tegema järgmist:

- Ära tunda ja valmistada target server, sh serveritaseme tulemüüri reeglid, sisselogimise ja põhi andmebaasi loenditaseme õigused.
- Määrata, kuidas suunata kliendid ja klientrakendustes uue serveriga
- Dokumendi muude sõltuvusi, nt sätted ja teatiste kontrollimine 
 
Kui te kavandamine ja ettevalmistamine õigesti, viies rakenduste online pärast Tõrkesiirde või enam võtab rohkem aega ja tõenäoliselt on vaja ka rõhku - halb kombinatsioon korraga tõrkeotsing.

### <a name="failover-to-a-geo-replicated-secondary-database"></a>Teisene geo tiražeeritud andmebaasi Tõrkesiirde 

Kui kasutate oma taastamine süsteemi, [jõustada geo kopeeritud teisese Tõrkesiirde](sql-database-disaster-recovery.md#failover-to-geo-replicated-secondary-database)aktiivse Geo-kopeerimine. Sekunditega, teisese on esiletõstetud saada uut esmast ja ongi valmis salvestamine uue tehingud ja vastata küsimusi - ainult mõne sekundi andmekao oli pole veel kopeeritud andmete jaoks. Automatiseerimise Tõrkesiirde kohta leiate teavet teemast [kujundus rakenduse puhul pilveteenuste avariitaastet](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

> [AZURE.NOTE] Kui andmekeskuse tuleb võrguühenduse taastamisel, saate failback, et algne esmane (või mitte).

### <a name="perform-a-geo-restore"></a>Tehke Geo-taastamine 

Kui kasutate automatiseeritud varukoopiate geograafilise liigne salvestusruumi kopeerimise abil oma taastamise süsteem, [algatada andmebaasi taastamise abil Geo-taastamine](sql-database-disaster-recovery.md#recover-using-geo-restore)nimega. Taastamine tavaliselt toimub 12 tunni jooksul - andmekao kuni üks tund, kui määrab viimase kord tunnis erinev varukoopia võetud ja tiražeeritud. Kuni taastamis lõpulejõudmist andmebaasi ei saa salvestada mis tahes tehingud või vastata küsimusi. 

> [AZURE.NOTE] Kui andmekeskuse tuleb võrguühenduse taastamisel, enne kui saate rakenduse üle minna taastatud andmebaasiga, saate lihtsalt taastamis tühistada.  

### <a name="perform-post-failover--recovery-tasks"></a>Tehke postitamine Tõrkesiirde / taastamise tööülesanded 

Pärast kummagi taastamine süsteemi taastamise, peate tegema järgmised täiendavad toimingud enne kasutajate ja rakendused on uuesti ja töötab.

- Redirect kliendid ja klientrakendustes uus server ja taastatud andmebaasiga
- Tagada vastav serveritaseme tulemüüri reeglid ühenduse (või kasutage [andmebaasi taseme tulemüürid](sql-database-firewall-configure.md#creating-database-level-firewall-rules)) kasutajate jaoks
- Tagada vastav sisselogimise ja põhi andmebaasi tasemel õiguste (või kasutage [sisaldas kasutajatele](https://msdn.microsoft.com/library/ff929188.aspx))
- Vajaduse korral Auditeerimispoliitika konfigureerimine
- Konfigureerida teatiste, vastavalt vajadusele

## <a name="upgrade-an-application-with-minimal-downtime"></a>Minimaalne ajakulu rakenduse täiendamine

Mõnikord on vaja rakenduse võrguühenduseta tõttu kavandatud hooldustööde, nt rakenduse täiendamine. [Haldamine rakenduse uuendused](sql-database-manage-application-rolling-upgrade.md) kirjeldatakse, kuidas lubada jooksva uuendamine oma pilveteenuse rakenduse minimeerida tööseisakute uuendamine ja taastamine tee juhul, kui midagi valesti läheb aktiivse Geo-kopeerimine abil. Selles artiklis käsitletakse kaks erinevat võimalust orkesteroinnin versiooniuuendus ja käsitletakse eelised ja kompromisse kummagi variandi.

## <a name="next-steps"></a>Järgmised sammud

Arutelu rakenduste kujundamine omaette andmebaasid ja elastne kaustu, vt [kujundus cloud avariitaastet taotlus](sql-database-designing-cloud-solutions-for-disaster-recovery.md) ja [elastne rakenduskausta katastroofi taastamine strateegiad](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).






