<properties
   pageTitle="SQL Azure'i andmebaasi Azure juhtumianalüüsi - Snelstart | Microsoft Azure'i"
   description="Teada, kuidas SnelStart kasutab SQL-andmebaasi laiendatud kiiresti oma business teenuste 1000 uue Azure SQL-i andmebaasid kuus määr"
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
   ms.date="09/08/2016"
   ms.author="carlrab"/>

# <a name="with-azure-snelstart-has-rapidly-expanded-its-business-services-at-a-rate-of-1000-new-azure-sql-databases-per-month"></a>Azure, laiendatud SnelStart kiiresti oma business teenuste 1000 uue Azure SQL-i andmebaasid kuus määr

![SnelStartLogo](./media/sql-database-implementation-snelstart/snelstartlogo.png)

SnelStart teeb Hollandi populaarsed finants - ja business-juhtimine väikestele ja keskmise suurusega ettevõtetele (SMBs). 55 000 klientide juurde pääseb personal 110 töötajat, sh ka IT töötajate 35. Teisaldate töölaua tarkvara tarkvara-kui-a-teenus, mis (SaaS) pakub tugineb Azure, SnelStart tehtud kõige sisseehitatud teenuste automatiseerimine halduse kasutamine tuttavas keskkonnas C# ja optimeerimine jõudlus ja skaleeritavus määratud pole kumbagi üle - või jaotises-ettevalmistamise ettevõtted abil elastne andmebaasi kaustu. Azure'i kasutamine tagab SnelStart paindlikkuse jooksva kliendid kohapealse ja pilveteenuse vahel.

> [AZURE.VIDEO azure-sql-database-case-study-snelstart]

##<a name="why-snelstart-extended-services-from-the-desktop-to-the-cloud"></a>Miks SnelStart laiendatud teenuste töölaualt pilveteenusesse

> "Töötamise Azure'i tähendab saame esitamisel tarkvara kiirem, kiiresti reageerida klientide nõudmistele ja mastaapimiseks lahendusi, kui nõuab suurendada."

> – Henry olnud, tarkvara arhitekt

SnelStart parandusfunktsiooni eduka tarkvara business aastat, traditsioonilise arengu mudeli kasutamine: kood, paketti, saata ja korrake. Aja jooksul muutuste kiirust kasvas kiiremini ja kiiremini. Määrus muudetud sageli ja klientide vajalik hõlbustamine töötlemine rahandus kirjed ja teha koostööd audiitorite ja government neid muudatusi kursis.

"Saatmine tarkvara klientidele oli kulukas ja piiravad," vastavalt Henry antud tarkvara arhitekt. "Tootmise, pakendamise ja kulud piiratud, kui sageli võib anname tarkvara. Meil oli paketi värskendused perioodiliste lähetustele, kuid et tehtud suur muutmisega klientide vajaduste rahuldamiseks. Me võib kunagi olla kindel, et kliendid versioonile toote uusim versioon. Seetõttu meil oli toetab mitut versiooni, muudab tugiteenuste töö väga raske hästi."

Antud lisab, "tahame võimalus programmi ja väljaanne värskendused kiirendatud tempos, et saaksime uuendamise kiiremini ja luua uusi teenuseid klientidele. Me ka kalendriüksusi nii mitme protsessi automatiseerimiseks klientide äri-halduse vajadustele lihtsustamiseks."

Lahendus oli SnelStart, laiendada oma teenuste saades pilvepõhist SaaS pakkuja. Azure'i SQL-andmebaasi platvormi aitas SnelStart sattusid ilma, et tekiks põhi IT-kulusid, millele oleks tulnud lahenduse taristu-kui-a-service (IaaS).

Pilveteenuse mudeli võimaldab SnelStart määrata vead ja esitage uued funktsioonid kiirelt, ilma kliendid, kellel on vaja alla laadida ja uuendada tarkvara. Vastavalt antud "kaudu Azure'i pilveteenustega võimaldab meil kiiresti töötlemiseks kolmandate osapoolte nõuete muutmine. Selle asemel, et saata tuhandeliste klientide uus versioon, saate kohandame meie uut vorminguid kolmandatele isikutele töölauarakenduse kaudu saadetud teabe."

##<a name="a-modern-company-with-traditional-roots"></a>Tänapäevane ettevõtte traditsiooniline juured

SnelStart on tänapäevane, dünaamilised, tehnoloogia äri alandlik juured ulatuvad 1964, kui asutajad alustamine ettevõtte muusikariistade osade tootja. Ning SnelStart tarkvara ettevõtetele alustamine tõesti peksmise 1980 ajal leviku arvuti. Ettevõtte vaja parem alternatiiv raamatupidamine tarkvara ajal saadaval nii, et kulus küsimustes enda kätes. Ettevõtte loodud 1982, fondi, mida soovite hakata SnelStart raamatupidamine tarkvara. Algusest, on lihtne ja kiiruse imetlenud tarkvara – nii palju, et ettevõtte lõpuks muutunud liikumine ja välja mõeldud ise tarkvara ettevõttesse.

1995 SnelStart välja oma esimese raamatupidamine rakendus Windows. Rakenduse Microsoft Visual Basic 1.0 ja Microsoft Accessi andmebaaside, tugineb aidanud kasvada kliendi base üle 5000 kasutajat.

Täna, on SnelStart joon-, äri (LOB) ja äri-halduse rakenduse suunatud Hollandi SMBs ja omas isikust ettevõtjaid põhi pakkuja. Carlo Kuip, nagu see arhitekt, küsib "Meie eesmärk on anda 100 protsenti automatiseerimise äri-halduse teenuste klientidele."

##<a name="optimizing-performance-and-cost-with-elastic-pools"></a>Jõudlus ja koos elastne kaustu optimeerimine

SnelStart oli suuremahuliste varajase kasutuselevõtja, elastne andmebaasi kaustu. Elastne kaustu aidata ettevõtte kulusid piirata ja nõuete tõhusamalt hallata. Vastavalt antud "elastne andmebaasi kaustu abil me saame optimeerida jõudlust ilma üle ettevalmistamise meie klientide vajaduste rahuldamiseks põhjal. Kui meil oleks ettevalmistamise tippväärtus laadi alusel, on üsna kulukas. Selle asemel suvandi vahel mitme, ressursside jagamiseks madal, kasutamine andmebaaside saame luua lahenduse, mis toimib hästi ja on tõhus. "

##<a name="azure-sql-databases-help-containerize-data-for-isolation-and-security"></a>SQL Azure'i andmebaasid aitab containerize andmeid eraldi ja turve 

SQL Azure'i andmebaas võimaldab SnelStart liikumiseks on lihtne ja läbipaistev klientide kohapealse äri-halduse Azure'i andmed. Azure'i SQL-andmebaasi on mugav ümbris, mis pakub eraldamise, piiri autentimine, autoriseerimine ja lihtne varundus ja taaste võimalusi. Andmebaaside ette sobivad hästi kontseptuaalne mudel äri haldus. Carlo Kuip, vastavalt selle arhitekt, "selle container piiri üksusi sisaldavad tundlikku andmeid, mida on oluline äri ja nende üksuste salvestamiseks isoleeritakse andmebaasi hoiab neid hästi kaitstud. Saame autoriseerimine tasemel andmebaasi haldamine ja isegi automatiseerida haldus ja skaala-out nende andmebaaside nõudmata andmebaasi administraatorid (DBAs) töötajatega."

SQL Azure'i andmebaas ka roll SnelStart turvalisus ja halduse lugu aidates ettevõtte kogumine telemeetria andmed, nt sissetungi tuvastamise, kasutaja tegevuse logimine ja Ühenduvus.

##<a name="azure-removes-overhead-so-that-developers-can-spend-more-time-delivering-value"></a>Azure'i eemaldab kohal, et arendajad saate rohkem aega pakkuda väärtus 

Azure'i platvormi mudeli eemaldatud taristu pea kohal ja lubatud SnelStart juurutuste C# haldamine teekide abil automatiseerida. Nagu Kuip, "Meil oli võimalik kasvada meie praeguse tegevuse väga väike töötajatega, samal ajal suurendades skaleeritavus, kiiruse ja katastroof taastesuvandeid klientidele. Üleminek teenuste arengu luua uusi teenuseid ja funktsioone, mitte ainult värskendamine koos uute määruste kursis kood või maksuvabastuse koodid ressursse." Lisab ta ", automatiseerimine haldus ja abil SaaS, pakub meil on võimalik pakkuda rohkem väärtust klientidele ilma teha suurte investeeringute töötajate." Näiteks Azure ja elastne andmebaasi kaustu abil SnelStart suutis lisada mitmesuguseid uusi funktsioone, sealhulgas senisest käepärasemad – kliendiandmete integreerimine pankadevahelistes, arveldus uusi teenuseid, väikeettevõtete tausta kontrollib ja e-posti teenuste.

> "Vaid mõned kuuga 2016, saame laiendatud meie Azure'i SQL-andmebaasi juurutuste kohta 5500 rohkem kui 12 000 ja meil on praegu lisada ligikaudu 1000 andmebaaside kohta kuus."

> – Henry olnud, tarkvara arhitekt

Andmebaasi haldamine on automatiseeritud põhjalikumaks elastne töö funktsiooni abil. Nagu Kuip, "me väga tänulikud automaatse tuvastamise andmebaaside SQL DB vahemikul [server]." See võimaldab SnelStart käivitada haldustoimingute üle nende dünaamiliselt kasvab klientide base ilma täiendavate pea kohal.

SnelStart areneb ka API, mis toimib mõnda ta Kliendiandmete ja muu tarkvara partnerite sisseehitatud rakenduste vahel. Nagu Kuip, "see API võimaldab muude tarnijatele funktsioone lisada meie tarkvara, nagu andmesisestuse arvete ja muude dokumentidega." Kliendid ei ole enam arveid käsitsi tippida oma väikeettevõtete raamatupidamine tarkvara; SnelStart tarkvara Exchange'i vajaliku teabe otse. See võimaldab liituda oma äri haldustoimingute teave, mis on väljunud digitaalse teisendus valdkonnas ökosüsteemi kliendid.  

##<a name="how-azure-services-enable-saas-for-snelstart"></a>Kuidas Azure'i teenuste SnelStart SaaS lubamine

Azure'i abil SnelStart võib olla oma klientidele ja nende audiitorite rohkem sujuvalt pilveteenuses. Näiteks nii kliendid ja audiitorite jagada teavet otse juurdepääs SnelStart's kliendi API, majutatud Azure. Kuip kinnitab: "need korduvkasutatava teenused on kättesaadavad meie klientide suunatud veebirakenduste ja nad pakuvad levinud taristu ja funktsioonid soovite lubada juurdepääsu business Administreerimine klientide ja muu tarkvara, mida meie klientide. Näiteks toote konfiguratsiooni võimalusi pakkuda, tulemüüri reeglite haldamine ja haldamise pikaajalisi protsessid, nagu varukoopiate."

> Meie eesmärk on pakkuda äri-halduse teenuste klientidele 100 protsenti automatiseerimine." 

> – Carlo Kuip IT arhitekt

Lisaks SnelStart veebiteenused nii kliendid ja audiitorite Azure'i SQL-andmebaasi elastne kaustadesse andmeid hõlpsalt juurde. Selle SaaS mudeli koos andmebaasi elastsuse ja Azure ressursihaldur, pakub SnelStart skaleeritavus funktsioone, mis täiendab iga Azure juurutamise. Rakendamist on täielikult automatiseeritud C# haldamine teekide abil.

![SnelStart arhitektuur](./media/sql-database-implementation-snelstart/figure1.png)

Joonis 1. Alates juunist 2016 SnelStart on rohkem kui 11 000 andmebaasid ja rohkem kui 50 elastne andmebaasi kaustu
 
##<a name="simplicity-from-the-cloud"></a>Lihtne pilveteenuses

Kuna liiguvad Azure pilvepõhist lahenduse, on SnelStart toetama kiire kliendi growth uuendusliku funktsioone ja teenuseid, pakkudes. Vastavalt antud "Azure, saame pakkuda lähedal pidev uuendused klientidele, ilma meie toimingute töötajad laiendamine. Ja kõik muud suur-Azure funktsioonid saame – näiteks skaleeritavus ja Avariijärgne taaste – sisse kombineeritud. "

> "Kui meil oli tegelikult üle Redmond... Sain kõne arendaja uuesti sisse Holland, helistades mulle mõne kindla probleemi kohta. Meil on võimalik saada Microsoft esitamisel muudatuse oma tootmiskeskkonnas 48 tundi meie probleemi lahendada."

> – Henry olnud, tarkvara arhitekt

SnelStart hindab ka need olen töötatud Microsoft Azure'i SQL-i DB meeskond tugev partnerlus. Nagu Kuip märgib, "meil arutelude funktsioonide ja tehnoloogia, mis on midagi kasutamise kohta hinnatud mõlemale poolele."
SnelStart kohe eesmärk on aina kasvavad alus selle rahul klient. Kuna olekud "Ilma tehniliste ja ressursside piirangud, mida me oli mõne ISV, on mingeid piiranguid, kui palju me ei saa kasvada."


## <a name="more-information"></a>Lisateave

- Azure'i elastne andmebaasi kaustu kohta leiate lisateavet teemast [elastne andmebaasi kaustu](sql-database-elastic-pool.md).

- Web rollid ja töötaja rolli kohta leiate lisateavet teemast [töötaja rollid](../fundamentals-introduction-to-azure.md#compute). 

- SQL Azure'i andmebaas kohta leiate lisateavet teemast [SQL-andmebaas](https://azure.microsoft.com/documentation/services/sql-data-warehouse/)

- SnelStart kohta leiate lisateavet teemast [SnelStart](http://www.snelstart.nl).


