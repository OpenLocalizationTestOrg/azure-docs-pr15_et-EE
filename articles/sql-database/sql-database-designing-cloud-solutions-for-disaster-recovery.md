<properties
   pageTitle="Pilvepõhine katastroofi taastamine lahendused - SQL-i andmebaasi aktiivse Geo kopeerimine | Microsoft Azure'i"
   description="Saate teada, kuidas kujundada cloud katastroofi taastamine lahendused äri järjepidevuse planeerimine geo-kopeerimise abil rakenduse andmete varundamine koos Azure'i SQL-andmebaasi."
   keywords="pilvepõhine avariitaastet, katastroofi taastamine lahendused, rakenduse andmete varundamine geo-dispersioonanalüüs äri järjepidevuse planeerimine"
   services="sql-database"
   documentationCenter=""
   authors="anosov1960"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management"
   ms.date="07/20/2016"
   ms.author="sashan"/>

# <a name="design-an-application-for-cloud-disaster-recovery-using-active-geo-replication-in-sql-database"></a>Kujundage taotlus cloud avariitaastet kasutamine SQL-andmebaasi aktiivse Geo-kopeerimine


> [AZURE.NOTE] [Aktiivse Geo-kopeerimine](sql-database-geo-replication-overview.md) on nüüd saadaval kogu astme kõik andmebaasid.



Saate teada, kuidas kasutada SQL-andmebaasis [Aktiivse Geo-kopeerimine](sql-database-geo-replication-overview.md) kujundus andmebaasirakendusi olles piirkondliku tõrkeid ja katastroofiline katkestuste. Äri järjepidevuse planeerimine, kaaluge juurutamise Otsingurakenduse topoloogia on teenuseleping olete suunatud liikluse latentsus ja kulud. Selles artiklis vaatame levinud rakenduse mustrid, ja eelised ja kompromisse iga suvandi foorumis. Kohta aktiivse Geo-kopeerimine koos elastne kaustu leiate artiklist [elastne rakenduskausta katastroofi taastamine strateegiad](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).

## <a name="design-pattern-1-active-passive-deployment-for-cloud-disaster-recovery-with-a-co-located-database"></a>Kujundus mustri 1: pilveteenuste avariitaastet asuvad andmebaasi aktiivne passiivne juurutamine

See suvand on kõige paremini rakenduste järgmised omadused:

+ Aktiivse eksemplari ühes Azure piirkonnas
+ Tugev sõltuvus registri (RW) juurdepääs andmetele
+ Rist-piirkond ühenduvuse rakenduse loogika ja andmebaasi vaheline ei ole lubatud tõttu latentsus ja liikluse kulu    

Sel juhul rakenduse juurutamine topoloogia on optimeeritud töötlemise piirkondliku katastroofi, kui kõik komponendid mõjutab ja Tõrkesiirde, kui üksus on vaja. Geograafilised koondamise rakenduse loogika ja andmebaas on teises regioonis kopeeritud, kuid neid ei kasutata rakenduse töökoormus tavaline tingimustel. Teisene piirkonna rakendus peaks olema konfigureeritud SQL-i ühendusstringi teisene andmebaasi kasutama. Liikluse haldur on häälestatud [Tõrkesiirde marsruutimise meetodit](../traffic-manager/traffic-manager-configure-failover-routing-method.md)kasutada.  

> [AZURE.NOTE] [Azure'i liikluse haldur](../traffic-manager/traffic-manager-overview.md) kasutatakse kogu käesoleva artikli eesmärgil. Saate kasutada koormust tasakaalustavad lahenduse, mis toetab Tõrkesiirde marsruutimise meetod.    

Lisaks rakenduse eksemplari, võiksite kaaluda juurutamine väike [töötaja rolli rakendus](cloud-services-choose-me.md#tellmecs) , mis jälgib esmane andmebaasi väljastanud perioodiliste T-SQL-i kirjutuskaitstud (RO) käsud. Saate automaatselt käivitada Tõrkesiirde, luua teatise sisse oma rakendus admin console või mõlemat. Tagada, et jälgimine on mõjutatud piirkonna kogu katkestuste, peaksite juurutada jälgimisega seotud teenuserakenduse eksemplaride iga piirkonna ja iga eksemplari esmane piirkonna esmane andmebaas ja teise andmebaasi kohaliku piirkonna ühendatud. 

> [AZURE.NOTE] Nii rakenduste jälgimine tuleks active ja probe nii põhi- ja andmebaasid. Viimase saab tuvastamiseks tõrke teisene piirkond ja Teavita, kui rakendus pole kaitstud.     

Järgmisel joonisel on katkestuste enne selle konfiguratsiooni.

![SQL-i andmebaasi Geo-tiražeerimine konfigureerimine. Pilveteenuse katastroofiabi.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern1-1.png)

Pärast mõne elektrikatkestus esmane piirkonna, jälgimisega seotud rakenduse tuvastab, et esmane andmebaas pole kättesaadav ja registreerib teatise. Olenevalt teie rakenduse SLA, saate otsustada, mitu järjestikust jälgimisega seotud sondid ei suuda enne andmebaasi katkestuste leitakse. Rakenduse lõpp-punkt ja andmebaasi koordineeritud Tõrkesiirde saavutamiseks peaks olema jälgimisega seotud rakenduse, tehke järgmist:

1. Selleks, et käivitada lõpp-punkti Tõrkesiirde [värskendada oleku esmane lõpp-punkt](https://msdn.microsoft.com/library/hh758250.aspx) .
2. Helistage teise andmebaasi [andmebaasi Tõrkesiirde](sql-database-geo-replication-portal.md)algatada.

Pärast Tõrkesiirde, töötleb kasutaja taotlused teisene piirkonna rakenduse, kuid jääb koostööd asub andmebaas, sest esmane andmebaas on nüüd teisene piirkonna. Selle stsenaariumi illustreerib järgmine skeem. Kõik skeemil, pidevjooned aktiivsed ühendused, tähistavad punktiirjooned peatatud ühendused ja stop märke tähistama toimingu päästikute.


![Geo-Dispersioonanalüüs: Tõrkesiirde teise andmebaasi. Rakenduse andmete varundamine.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern1-2.png)

Kui ka katkestuste juhtub teisene piirkonna, dispersioonanalüüs esmast ja teise andmebaasi vaheline seos on peatatud ja jälgimisega seotud rakenduse registreerib teatise, et esmane andmebaas on avatud. Rakenduse jõudluse pole mõjutab, kuid rakendus töötab exposed ja seega suurem risk juhul mõlemad piirkonnad nurjuda järjest.

> [AZURE.NOTE] Soovitame ainult ühe DR piirkonnas juurutamise konfiguratsioone. Seda sellepärast, et enamik Azure kaugemad on mõlema piirkonna. Neid konfiguratsioone ei kaitse rakenduse mõlema piirkonna katastroofiline tõrke. Sellise tõrke tõenäoline sündmuse saab taastada, andmebaaside kasutamine [geo taastetoimingu](sql-database-disaster-recovery.md#recovery-using-geo-restore)kolmanda piirkonnas.

Kui soovitud katkestuste leevendada, sünkroonitakse teise andmebaasi automaatselt esmane. Sünkroonimisel esmase jõudlust võivad pisut mõjutada sõltuvalt hulka andmeid, mis tuleb sünkroonida. Järgmine diagramm näitab mõne katkestuste teisene piirkonna.

![Teisene andmebaasi sünkroonitud primaarne. Pilveteenuse katastroofiabi.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern1-3.png)


See kujundus muster võtme **eelised** on:

+ SQL-i ühendusstring on seatud ajal rakenduse juurutamine iga piirkonna ja ei muutu pärast Tõrkesiirde.
+ Rakenduse jõudluse pole mõjutab Tõrkesiirde soovitud rakendus ja andmebaas on alati asuvad.

Peamised **Miinuseks** on üleliigsed rakenduse eksemplari teisene piirkonna kasutatakse ainult katastroofiabi.

## <a name="design-pattern-2-active-active-deployment-for-application-load-balancing"></a>Kujundus kujundus 2: rakenduse koormusetasakaalustuseks Active active juurutamine
Pilveteenuse katastroofi taastamise suvand sobib kõige paremini rakenduste järgmised omadused:

+ Kõrge-suhte andmebaasi loeb kirjutab
+ Andmebaasi kirjutamine latentsus ei mõjuta lõppkasutaja kogemus  
+ Kirjutuskaitstud loogika saab eraldada lugemis-ja kirjutamisõigusega loogika eri ühendusstringi abil
+ Kirjutuskaitstud loogika ei sõltu andmed on täielikult sünkroonitud uusimad värskendused  

Kui teie rakendused on need omadused, laadi tasakaalustamiseks lõppkasutaja ühendused üle mitu rakenduse eksemplari eri piirkondade saate parandada jõudlust ja lõppkasutaja. Rakendada koormust tasakaalustavad iga piirkonna peaks olema aktiivne rakenduse eksemplari esmane piirkonna esmane andmebaasiga ühendatud loogika lugemis-ja kirjutamisõigusega (r). Kirjutuskaitstud (RO) loogika peaks olema ühendatud teise andmebaasi sama piirkonna rakenduse. [Lõpp-punkti jälgimine](../traffic-manager/traffic-manager-monitoring.md) lubatud iga rakenduse eksemplari [round-jaan marsruutimist](../traffic-manager/traffic-manager-configure-round-robin-routing-method.md) ja [jõudluse marsruutimist](../traffic-manager/traffic-manager-configure-performance-routing-method.md) kasutamiseks tuleks luua liikluse haldur.

Nagu mustri #1, võiksite kaaluda sarnase jälgimisega seotud rakenduse juurutamine. Kuid erinevalt mustri #1 jälgimise rakendus ei vastuta käivitamise Tõrkesiirde lõpp-punkti jaoks.

> [AZURE.NOTE] Kuigi see mudel kasutab rohkem kui üks teise andmebaasi, kasutataks ainult üks soovitud sekundaaride Tõrkesiirde märgitud varem põhjustel. Kuna see muster nõuab teisese kirjutuskaitstud juurdepääsu, on vaja aktiivse Geo-kopeerimine.

Liikluse haldur peaks olema konfigureeritud jõudluse marsruutimine otse kasutaja ühendused rakenduse eksemplari lähemal kasutaja geograafiline asukoht. Järgmine diagramm näitab selle konfiguratsiooni enne mõne katkestuste.
![Pole katkestuste: lähima rakenduse marsruutimiseks jõudlust. Geo-Dispersioonanalüüs.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern2-1.png)

Andmebaasi katkestuste tuvastamisel esmane piirkonna annate Tõrkesiirde esmane andmebaasi teisene piirkonna esmase andmebaasi asukoha muutmine. Liikluse haldur automaatselt välistab ühenduseta lõpp-punkt marsruutimise tabelist, kuid ei lahene, lõppkasutaja liikluse marsruutimiseks ülejäänud Online'i eksemplari. Kuna esmane andmebaas on nüüd teises regioonis, online läbivalt kogu tuleb muuta oma lugemis-ja kirjutamisõigusega SQL-i ühendusstringi uue esmase ühenduse. On oluline, et selle muudatuse enne andmebaasi Tõrkesiirde. Kirjutuskaitstud SQL-i ühendusstringi jäävad samaks nagu alati sama piirkonna-andmebaasiga, valige käsk. Tõrkesiirde toimingud on.  

1. Muutmiseks osutage käsule Uus esmane lugemis-ja kirjutamisõigusega SQL-i ühendusstringi.
2. Selleks, et [algatada andmebaasi Tõrkesiirde](sql-database-geo-replication-portal.md)helistage määratud teise andmebaasi.

Järgmine diagramm näitab uue konfiguratsiooni pärast selle Tõrkesiirde.
![Pärast Tõrkesiirde konfigureerimine. Pilveteenuse katastroofiabi.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern2-2.png)

Korral on elektrikatkestus ühes teisene regioonid, liikluse haldur eemaldab automaatselt ühenduseta lõpp-punkt selle piirkonna marsruutimise tabelist. Kanali kopeerimine teise andmebaasi selle piirkonna on peatatud. Kuna ülejäänud piirkondade saamiseks täiendavad kasutaja liikluse sel on rakenduse jõudlust mõjutada selle katkestuste ajal. Kui soovitud katkestuste leevendada, sünkroonitakse teise andmebaasi kannatavas piirkonna kohe esmane. Sünkroonimisel esmase jõudlust võivad pisut mõjutada sõltuvalt hulka andmeid, mis tuleb sünkroonida. Järgmine diagramm näitab mõne katkestuste ühes teisene piirkondadest.

![Teisene piirkonnas katkestuste. Pilvepõhine avariitaastet - Geo-Dispersioonanalüüs.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern2-3.png)

Võtme **ära** see kujundus muster on, et te saate mastaapimiseks rakenduse töökoormus üle mitme sekundaaride lõppkasutaja optimaalse jõudluse saavutamiseks. See suvand **kompromissidega** on:

+ Lugemis-ja kirjutamisõigusega seoseid teenuserakenduse eksemplaride ja andmebaas on erineva latentsus ja maksumus
+ Rakenduse jõudlus mõjutab selle katkestuste ajal
+ Rakenduse eksemplari on vaja dünaamiliselt muuta pärast andmebaasi Tõrkesiirde SQL-i ühendusstring.  

> [AZURE.NOTE] Sarnast lähenemisviisi saab et eriotstarbelisi teenustest, nt aruandlusteenuste töö, ärianalüüsitööriistade või varukoopiaid. Tavaliselt nende töökoormus tarbimine märgatavat andmebaasi ressursid seetõttu on soovitatav teil määrata ühe teisene andmebaaside neid jõudluse taseme sobitada eeldatava töökoormus.

## <a name="design-pattern-3-active-passive-deployment-for-data-preservation"></a>Kujundus kujundus 3: aktiivne passiivne juurutamine andmete säilitamine  
See suvand on kõige paremini rakenduste järgmised omadused:

+ Mis tahes andmete kaotsimineku on kõrge risk. Andmebaasi Tõrkesiirde kasutada viimasena ainult siis, kui selle katkestuste on püsiv.
+ Rakenduse saate juhtida "kirjutuskaitstud režiimis" aja jooksul.

Rakenduse aktiveerib selle muster, kirjutuskaitstud režiimis, kui ühendatud teise andmebaasi. Rakenduse loogika esmane piirkonnas on koostööd asub esmane andmebaas ja pakub lugemiseks-kirjutamiseks režiimis (r). Rakenduse loogika teisene piirkonnas on koostööd asub teise andmebaasi ja kirjutuskaitstud režiimis (RO) kasutamiseks valmis.  [Lõpp-punkti jälgimine](../traffic-manager/traffic-manager-monitoring.md) lubatud nii teenuserakenduse eksemplaride [Tõrkesiirde marsruutimine](../traffic-manager/traffic-manager-configure-failover-routing-method.md) kasutamiseks tuleks luua liikluse haldur.

Järgmine diagramm näitab selle konfiguratsiooni enne mõne katkestuste.
![Aktiivne passiivne juurutus ette Tõrkesiirde. Pilveteenuse katastroofiabi.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern3-1.png)

Kui liikluse haldur tuvastab ühenduvuse tõrke esmane alale, aktiveerib selle kasutaja liikluse automaatselt rakenduse eksemplari teisene piirkonna. Selle mustriga on oluline see, mida **ei ole** algatada andmebaasi Tõrkesiirde pärast selle katkestuste tuvastamist. Teisene piirkonna rakendus on aktiveeritud ja töötab kasutamine teise andmebaasi kirjutuskaitstud režiimis. See on kujutatud järgmisel joonisel järgi.

![Katkestuste: Rakenduse kirjutuskaitstud režiimis. Pilveteenuse katastroofiabi.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern3-2.png)

Kui katkestuste esmane piirkonna leevendada, liikluse haldur tuvastab esmane piirkonna ühenduvuse taastamine ja aktiveerib kasutaja liikluse tagasi rakenduse eksemplari esmane piirkonna. Selle rakenduse eksemplari uuesti ja töötab lugemis-ja kirjutamisõigusega režiimis kasutaks primaarvõtme andmebaasi.

> [AZURE.NOTE] Kuna see muster nõuab teisese kirjutuskaitstud juurdepääsu on vaja aktiivse Geo-kopeerimine.

Korral on katkestuste teisene piirkonna, liikluse haldur märgib rakendus lõpp-punkt esmane piirkonna nagu halvenenud ja dispersioonanalüüs kanali on peatatud. Siiski ei mõjuta see katkestuste rakenduse jõudlust on katkestuste ajal. Kui soovitud katkestuste leevendada, sünkroonitakse teise andmebaasi kohe esmane. Sünkroonimisel esmase jõudlust võivad pisut mõjutada sõltuvalt hulka andmeid, mis tuleb sünkroonida.

![Katkestuste: Teise andmebaasi. Pilveteenuse katastroofiabi.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern3-3.png)

See kujundus muster on mitmeid **eelised**:

+ Andmete kaotsimineku oleks ajutiste katkestuste ajal.
+ See ei nõua, mida saate kasutada jälgimisega seotud rakenduse nagu taastamis käivitab liikluse haldur.
+ Tööseisakute sõltub ainult kui kiiresti liikluse haldur tuvastab ühenduvuse tõrge, mis on konfigureeritav.

**Kompromissidega** on:

+ Rakendus peab olema tegutsemiseks kirjutuskaitstud režiimis.
+ See on vajalik selle dünaamiliselt vahetada, kui see on kirjutuskaitstud andmebaasiga ühendatud.
+ Jätkamine lugemis-ja kirjutamisõigusega toimingute jaoks on vaja taastamise esmane piirkonna, mis tähendab, et täielik andmepääsu võib olla keelatud tundi või isegi päeva.

> [AZURE.NOTE] Korral püsivate teenuse elektrikatkestus piirkonna, tuleb käsitsi aktiveerida andmebaasi Tõrkesiirde ja nõustuge andmete kaotsimineku. Rakendus on otstarbekas teisene piirkonna lugemis-kirjutuspääs andmebaasi.

## <a name="business-continuity-planning-choose-an-application-design-for-cloud-disaster-recovery"></a>Business järjepidevuse kavandamine: valida puhul pilveteenuste avariitaastet rakenduse kujundus

Teatud cloud katastroofi taastamise strateegia kavandamine saate kombineerida või nende kujundus mustrite vajadustele kõige paremini teie taotlus laiendamine.  Nagu varem mainitud, põhineb valitav strateegia soovite pakkuda oma klientidele ja rakenduse juurutamine topoloogia SLA. Teie otsus juhendit järgmises tabelis võrreldakse Valikud põhjal hinnangulise andmete kaotsimineku või osutage eesmärk (RPO) ja eeldatav aeg (ERT).

| Mustri | RPO | ERT
| :--- |:--- | :---
| Aktiivne passiivne avariitaastet asuvad andmebaasi juurdepääsu juurutamine | Lugemis-ja kirjutamisõigused < 5 sec | Tõrge tuvastamise aeg + Tõrkesiirde API kõne rakenduste kinnitamine testimine
| Rakenduse koormusetasakaalustuseks aktiivne aktiivne juurutamine | Lugemis-ja kirjutamisõigused < 5 sec | Tõrge tuvastamise aeg + SQL-i ühendusstringi muutmine + Tõrkesiirde API kõne rakenduste kinnitamine testimine
| Andmete säilitamine aktiivne passiivne juurutamine | Lugemisõiguse < 5 sec lugemis-ja kirjutamisõigused = null | Lugemisõiguse = ühenduvuse tõrge tuvastamise aeg + rakenduste kinnitamine test <br>Lugemis-ja kirjutamisõigused = leevendada on katkestuste aeg

## <a name="next-steps"></a>Järgmised sammud

- Azure'i SQL-andmebaasi automaatse varundamise funktsiooni kohta lisateabe saamiseks vt [SQL-andmebaasi automaatse varundamise funktsiooni](sql-database-automated-backups.md)
- Äri järjepidevuse ülevaade ja stsenaariumid, vaadake teemat [äri järjepidevuse ülevaade](sql-database-business-continuity.md)
- Taastamine automaatse varundamise funktsiooni kasutamise kohta leiate teemast [andmebaasi teenuse algatatud varukoopiate põhjal taastamine](sql-database-recovery-using-backups.md)
- Kiirem taaste suvandite kohta leiate teemast [Aktiivne-Geo-Dispersioonanalüüs](sql-database-geo-replication-overview.md)  
- Arhiivimine automaatse varundamise funktsiooni kasutamise kohta lisateabe saamiseks lugege artiklit [andmebaasi kopeerimine](sql-database-copy.md)
- Kohta aktiivse Geo-kopeerimine koos elastne kaustu leiate artiklist [elastne rakenduskausta katastroofi taastamine strateegiad](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
