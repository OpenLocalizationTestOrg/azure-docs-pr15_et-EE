<properties
   pageTitle="Mustrite rentnikuga SaaS rakenduste ja Azure'i SQL-andmebaasi kujunduse | Microsoft Azure'i"
   description="Selles artiklis käsitletakse nõuded ja levinud andmete arhitektuur mustrid rentnikuga andmebaasi rakenduste töötamine cloud keskkonnas tuleb arvesse võtta ja erinevate kompromissidega, mis on seotud neid mustreid. See ka selgitatakse, kuidas Azure'i SQL-andmebaasi selle elastne kaustu ja elastne tööriistad, nendele nõuetele no kompromiss mood aadress."
   keywords=""
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
   ms.workload="sqldb-design"
   ms.date="08/24/2016"
   ms.author="carlrab"/>

# <a name="design-patterns-for-multitenant-saas-applications-and-azure-sql-database"></a>Kujundage mustrite rentnikuga SaaS rakenduste ja Azure SQL-andmebaas

Selles artiklis saate teadmisi nõuded ja ühiseid andmeid arhitektuur mustrite rentnikuga tarkvara nimega service (SaaS) andmebaasirakendusi käivitada cloud keskkonnas. Selgitatakse ka, tuleb arvestada tegurite ja muu kujundus mustrite kompromisse. Elastne kaustu ja Azure SQL-andmebaasis elastne tööriistad aitavad teil ilma muid eesmärke kompromisse teie konkreetsetele nõuetele.

Arendajate mõnikord teha valikuid, mis töötavad vastu nende pikaajalise huvides rendiõigus mudelid andmete astme rentnikuga rakenduste koostamisel. Esialgu vähemalt arendaja võib tajuvad hõlbustussätete ja alumise pilve tasu nimega olulisem rentniku eraldamise või rakenduse skaleeritavus. See valik võib põhjustada klientide rahulolu probleemid ja kulukas kursuse paranduse hiljem.

Rentnikuga rakendus on rakendus, mis on majutatud cloud keskkonnas ja teenuste sadu või rentnike jaoks, kes ühiskasutusse anda ja näha kohe üksteise andmeid tuhandete samu, mis pakub. Näide on SaaS rakendus, mis osutab rentnikega pilve majutatud keskkonnas.

## <a name="multitenant-applications"></a>Rentnikuga rakendused

Rentnikuga rakendustes andmed ja töökoormus võib olla hõlpsasti liigendatud. Te saate partition andmed ja töökoormus, näiteks mööda rentniku piirmäärad, kuna enamik taotlusi tekkida rentniku piirides. See atribuut on andmed ja töökoormus ja see soodustab rakenduse mustrid selles artiklis kirjeldatud.

Arendajad kasutada seda tüüpi rakendus kogu pilvepõhist rakenduste, sh:

- Partneri andmebaasirakendusi, mis on üleviidavad pilveteenusesse SaaS rakendused
- SaaS rakenduste puhul pilveteenuste maa loonud
- Otsest kliendi-ühendusega rakenduste
- Töötaja suunatud ettevõtte rakendused

SaaS rakendusi, mis on loodud pilve ja juured partneri andmebaasirakendusi tavaliselt on rakenduste rentnikuga. Need SaaS rakendused oma teenust eriotstarbelisi rakendus oma rentnikke. Rentnikud pääsete juurde rakenduse teenus ja on täielik omandiõiguse seotud andmed rakenduse osana. Kuid SaaS eelised ära, peab rentnikud tagastada mõned üle oma andmed. Usaldusväärne SaaS teenusepakkuja hoida oma andmeid turvalised ja eraldatud muude rentnike jaoks andmete põhjal. Selline rentnikuga SaaS rakendus on MYOB, SnelStart ja Salesforce.com. Kõik need taotlused saate liigendatud mööda rentniku piirmäärad ja rakenduse kujunduse mustrite me selles artiklis käsitletakse tugi.

Rakenduste, mis pakuvad otsest teenuse kliendid või töötajad ettevõttes (nimetatakse sageli kasutajad, mitte rentnikud) on teise kategooria rentnikuga rakenduse dokumendialase. Klientide tellida ja omanik teenusepakkuja kogub ja talletab andmeid. Teenuse pakkujad on vähem ranged nõuded hoida oma klientide andmeid, mis on eraldatud üksteisest Lisaks valitsuse volitatud privaatsust käsitlevate õigusaktidega. Selline kliendi-ühendusega rentnikuga rakendus on meediumi sisu pakkujad nagu Netflix, Spotify ja Xbox LIVE. Muude rakenduste kergesti partitionable on näiteks kliendi-ühendusega, Interneti-skaala rakendused või Internet, asjade rakenduste mis iga kliendi või seade võib olla sektsiooni. Sektsiooni piirmäärad saate eraldi kasutajatele ja seadmetele.

Kõik rakendused partition hõlpsasti mööda ühe atribuudi, nt rentniku, klient, kasutaja või seadme. Keerukate enterprise ressursi planeerimine (ERP) rakenduse, on näiteks toodete, tellimused ja kliendid. See on tavaliselt omavahel tugevalt seotud tabelite tuhandete keerukate skeem.

Puudub ühe sektsiooni strateegia saate rakendada kõigi tabelite ja töötavad rakenduse töökoormus. See artikkel keskendub rentnikuga rakendused on lihtne partitionable andmed ja töökoormus.

## <a name="multitenant-application-design-trade-offs"></a>Kompromisse rentnikuga rakenduse kujundus

Mustri kujundus, mis tavaliselt valib rentnikuga rakenduse arendaja on võttes arvesse järgmisi tegureid:

-   **Rentniku eraldamise**. Arendaja tuleb tagada, et pole rentniku on soovimatute muude rentnike jaoks andmetele juurdepääsu. Eraldamise nõue laieneb muid atribuute, nt pakkudes kaitse maksta, on võimalik, luuakse rentnikukonto andmete taastamine ja rakendamise rentniku kohased kohandused.
-   **Pilveteenuse Ressursi maksumus**. Rakenduse SaaS peab olema maksumus konkurentsianalüüsi. Rentnikuga rakenduse arendaja valida optimeerimine soodushinnaga cloud ressursse, näiteks salvestusruumi kasutamise ja kulud arvutada.
-   **DevOps hõlpsamaks**. Rentnikuga rakenduse arendaja tuleb lisada eraldi kaitse, säilitamiseks ja oma andmebaasi skeemi ja seisundi jälgimine ja rentniku probleemide tõrkeotsing. Rakenduste arendamise ja keerukuse vaste otse suurema kulu ja väljakutsed rentniku rahulolu.
-   **Skaleeritavus**. Võimalus artistide lisada rohkem rentnikke ja võimet jaoks, kellel on vaja see on oluline eduka SaaS toiming.

Iga teguritest on kompromisse võrreldes teise. Kõige väiksemad maksumuse pilveteenuses, ei pruugi pakkuda kõige mugavam arengu võimalusi pakub. Tuleb teha valikuid kursis püsida, nende võimaluste ja nende kompromisse kujundamise käigus arendaja.

Populaarsed arengu mustri on mitme rentniku üks või mitu andmebaasi pakkida. Seda moodust on väiksem maksumus Kuna maksate mõned andmebaasid ja suhteline lihtsad teatud andmebaaside töötamine. Kuid aja jooksul SaaS rentnikuga rakenduse arendaja aru, et see valik on olulisi varjuküljed rentniku eraldamise ja skaleeritavus. Kui rentniku eraldamise olulised, täiendavad peegeldav on vajalik ühine rentniku andmete kaitsmiseks volitamata juurdepääsu või maksta. See täiendavad peegeldav võib oluliselt suurendada arengu püüete ja eraldamise hoolduskulusid. Samamoodi kui lisamise rentnike jaoks on vajalik, selle kujunduse muster nõuab tavaliselt teadmisi levitada rentniku andmed üle andmebaaside õigesti mastaapimiseks andmete taseme rakenduse.  

Rentniku eraldamise sageli on oluline SaaS rentnikuga rakendustes, mis sobi ettevõtted ja organisatsioonid. Arendaja võib kiusata kaasnevaid lihtne ja maksumus üle rentniku eraldamise ja skaleeritavus. See kompromiss võib osutuda keerukad ja kulukad teenuse kasvab ja rentniku eraldamise muutuvad veel olulist ja hallatavate rakenduste kihis. Siiski võib rentniku eraldamise rentnikuga rakendustes otsese, tarbija suunatud osutamiseks klientidele, madalamat prioriteeti, kui cloud ressursside kulude optimeerimine.

## <a name="multitenant-data-models"></a>Rentnikuga andmemudelid

Kujundus tavad rentniku andmete suunamise järgige kolm erinevat mudelit, joonisel 1.


  ![Rentnikuga rakenduse andmemudelite](./media/sql-database-design-patterns-multi-tenancy-saas-applications/sql-database-multi-tenant-data-models.png)
 joonisel 1: kujundus tavad rentnikuga andmemudelite jaoks.

-   **Andmebaasi rentniku kohta**. Iga Rentnik on oma andmebaasi. Andmete kõigi rentniku piirdub rentniku andmebaasi ja muud rentnikud ja nende andmete eraldatud.
-   **Andmebaasi sharded ühiskasutuses**. Mitme rentniku jagada üks mitme andmebaasid. Erinevate kogumi rentnikud on määratud iga andmebaasi nt räsi, vahemiku või loendi eraldamine eraldamine strateegia abil. Andmete jaotuse strateegia sageli nimetatakse sharding.
-   **Ühiskasutusse antud andmebaasi-ühekordne**. Ühes, mõnikord suur, andmebaas sisaldab andmeid kõik rentnikud, mis on disambiguated rentniku ID veeru jaoks.

> [AZURE.NOTE] Rakendus arendaja valida muu andmebaasi skeemid erinevate rentnikega paigutamine ja disambiguate erinevate rentnikega skeemi nimi abil. Me ei soovita seda moodust, kuna see nõuab tavaliselt dünaamiline SQL-i kasutuse ja ei saa see olla tõhus leping vahemällu. Käesoleva artikli ülejäänud me keskenduda ühiskasutusse antud tabeli lähenemine selle rentnikuga rakenduse kategooria.

## <a name="popular-multitenant-data-models"></a>Populaarsed rentnikuga andmemudelid

See on oluline hinnata erinevat tüüpi rentnikuga andmemudelite osas on rakenduse kujundus kompromisse me juba olete kindlaks teinud. Teguritest aidata vabadusastmeid on kolm kõige levinum rentnikuga andmemudelite eespool kirjeldatud ja oma andmebaasi kasutamine, nagu on näidatud joonisel 2.

-   **Eraldi**. Millises rentnikud vahelise eraldustaseme võib olla palju rentniku eraldustaseme andmemudeli saavutab mõõt.
-   **Pilveteenuse Ressursi maksumus**. Ressursside jagamine rentnikud summa saab optimeerida cloud Ressursi maksumus. Ressursi defineeritakse Arvuta ja salvestusruumi maksumus.
-   **DevOps maksumus**. Lihtne rakenduste arendamise, juurutamise ja hallatavust vähendab SaaS toiming kogukulu.  

Joonis 2, Y-telg näitab rentniku eraldustaseme. X-telje näitab ressursside ühiskasutus. Hall, diagonaalse noole keskel näitab DevOps kulud, aidata suurendamiseks või vähendamiseks suunda.

![Populaarsed rentnikuga rakenduse kujundus mustrid](./media/sql-database-design-patterns-multi-tenancy-saas-applications/sql-database-popular-application-patterns.png) joonis 2: populaarsed rentnikuga andmemudelid

Alumises paremas nurgas joonisel 2 näitab rakendus mustri, mis kasutab potentsiaalselt suure ühe ühisandmebaasiga ja ühiskasutusse antud tabeli (või eraldi skeemi) lähenemine. See on hea ressursside ühiskasutus, kuna kõik rentnikud kasutada sama andmebaasi ressursse (CPU, mälu, sisend) ühe andmebaasi. Kuid rentniku eraldamise on piiratud. Peate kaitsta rentnikud üksteisest kihis rakenduse täiendavaid samme. Need täiendavad juhised võib oluliselt suurendada DevOps kulu arendamise ja rakenduse haldamine. Riistvara, mis hostib andmebaasi skaala on piiratud skaleeritavus.

Vasakus moodustavad joonisel 2 illustreerib mitme rentniku sharded üle mitme andmebaasi (tavaliselt erinevaid riistvara mastaapimiseks ühikud). Iga andmebaasi majutab alamhulga rentnikud, milles käsitletakse muude mustrite skaleeritavus huvi. Kui suuremat nõutakse veel rentnike jaoks, saate hõlpsasti paigutada uute andmebaaside eraldatud uute riistvara skaala üksuste on rentnike jaoks. Ressursside ühiskasutus summa on vähendatud. Ainult rentnikud pandud sama skaala üksuste ühiskasutus ressursid. Seda moodust pakub vähe parandamise rentniku eraldamise, kuna paljud rentnikud on endiselt paikneb ilma automaatselt kaitstud üksteise toimingud. Rakenduse keerukuse endiselt kõrge.

Kolmas lähenemine on ülemise vasakpoolse moodustavad joonisel 2. See paigutab iga rentniku andmeid oma andmebaasi. Seda moodust on hea rentniku-eraldamine atribuudid, kuid piirab ressursside ühiskasutus kui iga andmebaas on spetsiaalne ressurssidest. See lähenemine on hea, kui kõik rentnikud on prognoositavad töökoormus. Kui rentniku töökoormus prognoosida, ei saa pakkuja optimeerida ressursside ühiskasutus. Ettearvamatust on tavaline SaaS rakendused. Pakkuja peab kas üle sätte vastavad nõudmistele või madalam ressursid. Kas toimingu tulemuste suuremad kulud või madalam rentniku rahulolu. Suurema astet ressursside jagamise rentnikud muutub soovitav lahendus keerulisemaks teha. Andmebaaside arvu suureneb ka DevOps maksumus juurutada ja hallata rakenduse. Hoolimata nende probleemide pakub seda meetodit rentnike jaoks parim ja lihtsaim eraldamise.

Teguritest mõjutada ka klient valib kujundus muster:

-   **Rentniku andmete omandiõiguse**. Rakendus, mille rentnikud jääma oma andmete eelistab mustri ühe andmebaasi rentniku kohta.
-   **Skaala**. Tuhandeliste või miljoneid rentnikud sadu rakenduse eelistab andmebaasi ühiskasutuse võimalused, nt sharding. Siiski saate eraldamise nõuete tekitada probleeme.
-   **Väärtus ja business mudeli**. Kui rakenduse kohta – rentniku tulu kui väike (vähem kui dollar), muutuvad vähem kriitilised eraldamise nõuded ja ühiskasutatav andmebaas on mõistlik. Kui tulu rentniku kohta on paar dollarit või rohkem andmebaasi – rentniku mudeli enam võimalik. See võib aidata vähendada areng.

Antud joonisel 2 kujundus kompromisse, optimaalne rentnikuga mudelit, mis on vaja lisada hea rentniku eraldamise atribuudid ühiskasutuse rentnikud optimaalse ressursiga. See mudel sobib kirjeldatud ülemises paremas nurgas joonise 2 kategooria.

## <a name="multitenancy-support-in-azure-sql-database"></a>Azure'i SQL-andmebaasi multitenancy tugi

Azure'i SQL-andmebaasi toetab kõiki rentnikuga rakenduse mustrid liigendatud joonisel 2. Elastne kaustu, samuti toetab rakenduse mustri, mis ühendab hea ressursside ühiskasutus ja eraldamise eelised andmebaasi-kohta – rentniku lähenemine (vt ülemises paremas nurgas joonisel 3). Elastne Andmebaasiriistad ja võimalusi SQL-andmebaasis aitab vähendada seotud rakendus, mis on palju andmebaasid (näidatud varjustatud alale joonisel 3). Nende tööriistade aitab teil luua ja hallata rakendusi, mis kasutavad ühte mitme andmebaasi mustrite.

![Azure'i SQL-andmebaasi mustrite](./media/sql-database-design-patterns-multi-tenancy-saas-applications/sql-database-patterns-sqldb.png) joonis 3: rentnikuga rakenduse mustrid Azure SQL-andmebaasis

## <a name="database-per-tenant-model-with-elastic-pools-and-tools"></a>Andmebaasi – rentniku mudeli elastne kaustu ja tööriistad

Elastne kaustu SQL-andmebaasis kombineerida rentniku eraldamise ressursside jagamise rentniku andmebaaside parema tugiteenuste lähenemine andmebaasi rentniku kohta. SQL-andmebaasi on andmete taseme lahendus SaaS pakkujad, kes luua rentnikuga rakendusi. Ressursside ühiskasutus rentnikud koormust nihkub rakenduse kiht andmebaasi teenuse kihti. Elastne tööde haldamine, elastne päringu, elastne tehingud ja elastne andmebaasi kliendi teek on lihtsustatud keerukus tasandil üle andmebaaside päringute ja haldamine.

| Rakenduse nõuded | SQL-i andmebaasi võimalused |
| ------------------------ | ------------------------- |
| Rentniku eraldamise ja ressursside ühiskasutus | [Elastne kaustu](sql-database-elastic-pool.md): SQL-andmebaasi ressursid kogumi eraldada ja ressursside ühiskasutamiseks andmebaase. Ka üksikute andmebaasid saate joonistada piisavalt ressursse kaustast vastavalt vajadusele majutada võimsus nõudmisel diagrammi rentniku töökoormus muudatuste tõttu. Elastne pool ise saate mastaabitud üles või alla vastavalt vajadusele. Elastne kaustu pakuvad ka lihtne hallatavust ja jälgimine ja tõrkeotsingu rakenduskausta tasemel. |
| Lihtne DevOps üle andmebaasid | [Elastne kaustu](sql-database-elastic-pool.md): nagu varem.|
||[Elastne päringu](sql-database-elastic-query-horizontal-partitioning.md): päringu üle andmebaaside aruandluse jaoks või rist – rentniku analüüsi.|
||[Elastne tööde haldamine](sql-database-elastic-jobs-overview.md): paketi ja Juurutage usaldusväärselt mitme andmebaasi andmebaasi hooldustööd või andmebaasi skeemi muudatusi.|
||[Elastne tehingud](sql-database-elastic-transactions-overview.md): protsess muutub andmebaasidest atomic ja isoleeritakse. Elastne pole vaja kui rakenduste vaja "kõik või mitte midagi" garantiid üle mitme andmebaasi toimingud. |
||[Elastne andmebaasi kliendi teek](sql-database-elastic-database-client-library.md): andmete jaotuse hallata ja Vastendage rentnikud andmebaasid. |

## <a name="shared-models"></a>Ühiskasutusega mudelite

Mis on kirjeldatud varasemas versioonis, SaaS enamik teenusepakkujaid, jagatud mudel lähenemine võivad kujutada probleemid rentniku eraldamise probleemid ja keerukus rakenduste arendamise ja hooldus. Siiski rentnikuga rakendusi, mis annavad teenuse otse tarbijatele, rentniku eraldamise nõuded ei pruugi kõrge tähtsusega nimega minimeerimine maksumus. Neid võib pakkida rentnikud kõrge tihedusega kulude vähendamiseks üks või mitu andmebaasid. Andmebaasi ühe või mitme sharded andmebaaside abil ühiskasutusse antud andmebaasi mudelite võib põhjustada täiendavaid tõhusust ressursside ühiskasutus ja üldine maksumus. SQL Azure'i andmebaas pakub mõned funktsioonid, mille abil kliendid koostada eraldamise täiustatud turvalisus ja haldamise alal andmete astme.

| Rakenduse nõuded | SQL-i andmebaasi võimalused |
| ------------------------ | ------------------------- |
| Eraldamise turbefunktsioonid | [Rea taseme turvalisus](https://msdn.microsoft.com/library/dn765131.aspx) |
|| [Andmebaasi skeemi](https://msdn.microsoft.com/library/dd207005.aspx) |
| Lihtne DevOps üle andmebaasid | [Elastne päring](sql-database-elastic-query-horizontal-partitioning.md) |
|| [Elastne tööde haldamine](sql-database-elastic-jobs-overview.md) |
|| [Elastne tehinguid](sql-database-elastic-transactions-overview.md) |
|| [Elastne andmebaasi kliendi teek](sql-database-elastic-database-client-library.md) |
|| [Elastne andmebaasi tükeldamine ja ühendamine](sql-database-elastic-scale-overview-split-and-merge.md) |

## <a name="summary"></a>Kokkuvõte

Rentniku eraldamise nõuded on oluline, et enamik SaaS rentnikuga rakendused. Tugevalt kuvasuhtele andmebaasi – rentniku lähenemine on parim valik eraldamise esitada. Kaks lähenemisviisi jaoks on vaja investeeringud keerukate rakenduse kihid, mis nõuavad kvalifitseeritud arengu töötajate eraldustaseme, mis suurendab oluliselt ja esitada. Kui eraldamise nõuded neid ei arvestata varakult väljatöötamisel teenus, nende modifitseerimine võib olla isegi kulukam kaks esimest mudelid. Peamised puudused seotud mudeliga andmebaasi rentniku kohta on seotud suurema cloud ressursside kulude tõttu vähendatud ühiskasutus ja säilitamine palju andmebaaside haldamine. SaaS rakenduste arendajatele sageli vaeva, kui nad teevad neid kompromisse.

Kuigi kompromisse võib olla peamised takistused enamik cloud andmebaasi teenusepakkujatele, Azure'i SQL-andmebaasi kõrvaldab takistused elastne rakenduskausta ja elastne andmebaasi võimalusi. SaaS arendajate saate ühendab eraldamise omadused andmebaasi – rentniku mudeli ja optimeerida ressursside ühiskasutus ja palju andmebaaside hallatavust täiustused elastne kaustu ja seotud tööriistade abil.

Rentnikuga rakenduse pakkujaid, kellel on rentniku eraldamise nõuded ja kes saab pakkida rentnikud andmebaasi kõrge tihedusega võib leida ühiskasutusega andmete mudelid tulemi täiendavad tõhusust ressursside ühiskasutus ja üldine kuidagi vähendada. Azure'i SQL-andmebaasi elastne Andmebaasiriistad, sharding teekide ja turbefunktsioonid aidata SaaS pakkujad koostamine ja rentnikuga rakenduste haldamine.

## <a name="next-steps"></a>Järgmised sammud

[Alustamine elastne Andmebaasiriistad](sql-database-elastic-scale-get-started.md) valimi rakendusega, mis näitab kliendi teek.

[Elastne rakenduskausta kohandatud armatuurlaud SaaS](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools-custom-dashboard) valimi rakendusega, mis kasutab elastne kaustu kuluefektiivseid, scalable andmebaasi lahenduse loomine.

Azure'i SQL-andmebaasi migreerimine [olemasolevaid andmebaase mastaapimiseks välja](sql-database-elastic-convert-to-use-elastic-tools.md)tööriistu saate kasutada.

Saate vaadata meie õpetuse [elastne kohapeal on loomise](sql-database-elastic-pool-create-portal.md)kohta.  

Siit saate teada, kuidas [jälgimine ja haldamine kohapeal on elastne](sql-database-elastic-pool-manage-portal.md).

## <a name="additional-resources"></a>Lisaressursid

- [Mis on Azure elastne kohapeal on?](sql-database-elastic-pool.md)
- [Skaala läbi Azure SQL-i andmebaasiga](sql-database-elastic-scale-introduction.md)
- [Rentnikuga rakenduste elastne Andmebaasiriistad ja rea taseme turvalisus](sql-database-elastic-tools-multi-tenant-row-level-security.md)
- [Rentnikuga rakendustes Azure Active Directory ja OpenID Connect abil autentimine](../guidance/guidance-multitenant-identity-authenticate.md)
- [Tailspin uuringute rakendus](../guidance/guidance-multitenant-identity-tailspin.md)
- [Lahendus kiire käivitamise](sql-database-solution-quick-starts.md)

## <a name="questions-and-feature-requests"></a>Küsimused ja soovitada funktsioone

Küsimused, otsige üles kontaktteabe [SQL-andmebaasi Foorum](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted). [SQL-andmebaasi tagasiside Foorum](https://feedback.azure.com/forums/217321-sql-database/)funktsiooni taotluse lisada.
