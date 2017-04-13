<properties
   pageTitle="Andmete migreerimine SQL-i andmebaas | Microsoft Azure'i"
   description="Näpunäiteid andmete migreerimist SQL Azure'i andmebaas arendamise lahendusi."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/25/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="migrate-your-data"></a>Andmete migreerimine
Andmeid saab eri allikatest pärit teisaldada SQL-i andmebaas erinevaid tööriistu.  ADF koopia, SSIS ja bcp kõik saab selle eesmärgi saavutamiseks. Siiski andmete suureneb summana peaksite mõtlema, milliseid kõrvaldamine andmete migreerimisprotsessi üheks juhiseid. See pakub teile võimaluse optimeerida iga toimingu jõudluse ja paindlikkuse tagamiseks sujuv andmete migreerimise jaoks.

Selles artiklis käsitletakse esmalt lihtsa migreerimise stsenaariume ADF koopia, SSIS ja bcp. See siis ilme, kuidas saab optimeerida migreerimise pisut pöörama.

## <a name="azure-data-factory-adf-copy"></a>Azure'i andmed Factory (ADF) kopeerimine
[Azure'i andmed Factory][] [ADF Kopeeri][] kuulub. ADF Kopeeri abil saate oma andmete eksportimine tasapinnalise failide elavate kohaliku salvestusruumi, remote tasapinnalise failide Azure'i bloobimälu või otse SQL-i andmebaas.

Kui andmete käivitab tasapinnalise faile, siis peate esmalt edastada salvestusruumi Azure'i bloobimälu enne alustamist koormus see SQL-i andmebaas. Kui andmed on üle Azure'i bloobimälu saate [ADF Kopeeri][] push andmed SQL-i andmebaas uuesti kasutada.

PolyBase pakub suure jõudlusega suvand laadimise andmed. Siiski, mis tähendab, et ühe asemel kaks tööriista abil. Kui vajate parima jõudluse, siis kasutage PolyBase. Kui soovite ühe tööriista kogemus (ja andmed pole suuri) on ADF vastust oma küsimusele.

> [AZURE.NOTE] PolyBase nõuab andmefailide olema UTF-8. See on ADF Kopeeri vaikimisi kodeering, seega ei ole muuta. See on lihtsalt meeldetuletus pole ADF Kopeeri vaikekäitumise muutmiseks.

Mõned suured [ADF näidised][]pea üle järgmistest artiklitest.

## <a name="integration-services"></a>Integration Services ##
Integration Services (SSIS) on võimas ja paindlik ekstraktida transformatsioon ja laadi (ETL) tööriista, mis toetab keerukate töövood, teisendamiseks ja mitu andmete laadimise suvandid. SSIS-i abil lihtsalt andmete edastamiseks Azure või laiema migreerimise osana.

> [AZURE.NOTE] SSIS-i saate eksportida ilma bait tellimuse märgi faili UTF-8. Konfigureerida see tuleb esmalt kasutada tuletatud veeru komponent andmevoo kasutada 65001 UTF-8 koodileht märgi andmete teisendamiseks. Kui veerud on ümber, kirjutage andmed lamefaili sihtkoha adapterit tagada, et 65001 on valitud ka koodileht faili.

SSIS-i loob ühenduse SQL-i andmebaas, nagu see oleks ühenduse loomine SQL serveri juurutamine. Ühenduste on vaja kasutada ka ADO.net-i haldur. Mida peaks ka Olge ettevaatlik, et konfigureerida "Kasuta hulgi lisa kui need on saadaval" säte läbilaskevõime maksimeerimiseks. Lugege lisateavet selle atribuudi [ADO.NET-adapterit, sihtkoha][] artiklis

> [AZURE.NOTE] Ühenduse loomine SQL Azure'i andmebaas, kasutades OLEDB ei toetata.

Lisaks on alati võimalus, et paketi võib nurjuda tähtaja probleeme pidurdamise või võrku. Kujunduse paketid nii, et nad saavad jätkata kohas tõrge, ilma redoing töö lõpetatud enne tõrke.

Lugege lisateavet [SSIS dokumentatsiooni][].

## <a name="bcp"></a>BCP
BCP on käsurea kasuliku, mis on mõeldud lamefaili andmed impordi ja ekspordi. Mõned teisendus võib toimuda andmete eksportimise käigus. Teha lihtsa teisendused päringu abil saate valida ja muuta andmeid. Kui eksporditud, tasapinnalise faile saab seejärel laadida otse sihtkohas andmebaasi SQL-i andmebaas.

> [AZURE.NOTE] Sageli on mõistlik kasutada andmete eksportimine vaates allika süsteemi ajal muutusi kapseldada. See tagab loogika säilib ja protsess on korratavad.

BCP eelised on järgmised.

- Lihtne. BCP käsud on lihtne luua ja käivitada
- Laadi uuesti startable protsess. Laadi võib olla üks kord eksporditud täidetud mis tahes arv kordi.

Bcp piirangud on järgmised.

- BCP töötab ainult tabelina tasapinnalise failid. See ei tööta, nt xml või JSON failidega
- BCP ei toeta eksportimise UTF-8. See võib takistada PolyBase bcp eksporditud andmete kasutamine
- Andmete teisendus võimalused on piiratud ainult ekspordi etapi ja on lihtne laadi
- BCP ei ole kohandatud olla robustne, kui andmete laadimine Interneti kaudu. Võrgu ebastabiilsus võib põhjustada koormus viga.
- BCP tugineb viibides sihtandmebaas enne laadi skeem

Lisateabe saamiseks vt [kasutamine bcp andmeid SQL-i andmebaas laadimiseks][].

## <a name="optimizing-data-migration"></a>Andmete migreerimise optimeerimine
SQLDW andmete migreerimisprotsessi saate tõhusalt jagada eraldi kolm toimingut:

1. Allika andmete eksportimine
2. Andmete kandmine Azure
3. SQLDW sihtandmebaas laadimine

Iga etapi saab optimeerida eraldi luua töökindlaid, uuesti startable ja olles migreerimisprotsessi, mis maksimeerib jõudluse iga toimingu juures.

## <a name="optimizing-data-load"></a>Optimeerimise andmete laadimine
Vaadates neid vastupidises järjekorras hetkeks; kiireim viis andmete laadimine on PolyBase kaudu. Laadi PolyBase protsessi optimeerimine kohti eeltingimused eelmist toimingut nii, et see kõige paremini mõista seda algusest peale. Need on:

1. Andmefailid kodeerimine
2. Andmefailide vorming
3. Andmefailide asukohta

### <a name="encoding"></a>Kodeerimine
PolyBase jaoks on vaja olema UTF-8 kodeeringuga andmefailid. See tähendab, et andmete eksportimisel see peab vastama nõue. Kui teie andmed sisaldavad ainult lihtsa ASCII märkide (mitte laiendatud ASCII) siis nende kaarti otse UTF-8 standard ja te ei pea muretsema liiga palju kodeering. Juhul, kui teie andmed sisaldavad erimärke nagu täpitähtedega, tähed või sümbolid või andmete toetab Ladina keeles siis on tagada ekspordi failide õigesti UTF-8 kodeeringuga.

> [AZURE.NOTE] BCP ei toeta andmete eksportimise UTF-8. Seega on teie parim valik kasutada andmete eksportimine Integration Services või ADF Kopeeri. See on vaja rõhutada, et andmefaili ei pea selle UTF-8 bait märgi (komplekti).

Kõik failid kodeeritud UTF-16 tuleb uuesti kirjutada ***eelneva*** andmete edastamiseks.

### <a name="format-of-data-files"></a>Andmefailide vorming
PolyBase mandaatide fikseeritud rea terminator \n või newline. Failide andmed peavad vastama standardit. Tekstistring või veerg terminaatorid piiranguid pole.

Kas peate määratlema iga veeru faili oma välise tabeli PolyBase osana. Veenduge, et kõik eksporditud veerud vajalike ja nõuetele vastavad tüübid.

Palun vaadake tagasi soovitud [migreerimine oma skeemi] artiklis teavet toetatud andmetüüpide kohta.

### <a name="location-of-data-files"></a>Andmefailide asukohta
SQL-i andmebaas kasutab PolyBase alla laadida ainult Azure'i bloobimälu andmeid. Järelikult andmed tuleb esmalt edastamist bloobimälu sisse.

## <a name="optimizing-data-transfer"></a>Optimeerimise andmete edastamine
Üks andmete migreerimise parema osad on andmete kandmine Azure. Võrgu läbilaskevõime võib olla probleemi vaid ka võrgu töökindluse võivad oluliselt takistada edenemist. Azure'i andmete migreerimine on vaikimisi Interneti kaudu edastamine vigu tõenäoliselt üksikisiku. Nende vigade võib siiski nõuda andmetega täielikult või osaliselt uuesti saata.

Õnneks on teil mitu võimalust paindlikkust seda toimingut ja kiiruse parandamiseks:

### <a name="expressroute"></a>[ExpressRoute][]
Võite kaaluda [ExpressRoute][] kiirendamiseks edastust. [ExpressRoute][] annab teile loodud privaatne ühenduse Azure'i nii, et ühendust ei lähe avaliku Interneti kaudu. See ei ole kohustuslik. Siiski, seda parandada läbilaskevõime kui lükkamine andmete Azure asutusesisese kaudu või kaasautorluse kohta.

[ExpressRoute][] kasutamise eelised on:

1. Suurema töökindluse
2. Võrgu kiiremini
3. Alumise võrgu latentsus
4. võrgu suurem turvalisus

[ExpressRoute][] on kasulik arvu stsenaariumid; mitte ainult migreerimise.

Huvi? Lisateabe saamiseks ja hinnakirjad leiate veebisaidilt [ExpressRoute dokumentatsiooni][].

### <a name="azure-import-and-export-service"></a>Azure'i impordi ja ekspordi teenus
Azure'i importimine ja eksportimine teenus on mõeldud suurele (GB ++) suuri (TB ++) andmete edastamine Azure'i sisse andmete edastamine protsess. See hõlmab andmete ketast kirjutamist ja need on Azure andmekeskuse saatmise. Seejärel laaditakse üheks Azure salvestusruumi plekid ketta sisu teie nimel.

Üksikasjalik vaade impordi eksportimise käigus on järgmine:

1. Azure'i bloobimälu nõus kui andmed konfigureerimine
2. Andmete eksportimine kohalik salvestusruum
2. Kopeerige andmed, et 3.5 tolli SATA III ja II kõvaketta draivid [Azure impordi/ekspordi tööriista abil]
3. Impordi töö, mis abil Azure impordi ja ekspordi teenust töölehe failid toodetud [Azure impordi/ekspordi tööriista] loomine
4. Saata selle ketast määratud Azure andmekeskuse
5. Azure'i bloobimälu oma container kantakse andmete
6. Andmete laadimine SQLDW PolyBase abil

### <a name="azcopy-utility"></a>[AZCopy][] kasuliku
[AZCopy][] kasuliku on suurepärane vahend Azure salvestusruumi plekid üle andmete toomiseks. See on mõeldud väike (MB ++) abil väga suurte andmeedastuse (GB ++). [AZCopy] on ka loodud hea olles läbilaskevõime kui edastate andmeid Azure ja nii on hea valik andmete edastamine juhise jaoks. Kui edastatud laadite andmed abil PolyBase SQL-i andmebaas. SSIS-pakettide "Käivitada protsess" toimingu abil saate lisada ka AZCopy.

AZCopy kasutamiseks peate esmalt alla laadima ja installima. Lisaks on [tootmise versioon][] ja [eelvaateversiooni][] .

Failisüsteemi kaudu faili üles laadimiseks peate käsu nagu allpool:

```
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:abc.txt
```

Üksikasjalik protsess Kokkuvõte võiks olla.

1. Salvestusruumi Azure'i bloobimälu paaniümbrist kui andmed konfigureerimine
2. Andmete eksportimine kohalikku
3. AZCopy andmete ümbrises Azure'i bloobimälu
4. SQL-i andmebaas PolyBase abil andmete laadimine

Täielik dokumentatsiooni: [AZCopy][].

## <a name="optimizing-data-export"></a>Optimeerimise andmete eksportimine
Lisaks tagab ekspordi PolyBase on sätestatud nõuetele võite ka otsida optimeerida protsessi edasiseks parandamiseks andmete eksportimine.

> [AZURE.NOTE] Nagu PolyBase nõuab andmed olema UTF-8 vormingus tõenäoliselt kasutate bcp andmete eksportimine sooritamiseks. BCP ei toeta UTF-8 kirjutamine andmefailid. SSIS-i või ADF Kopeeri on palju paremini toime läbimiseks seda tüüpi andmete eksport.

### <a name="data-compression"></a>Andmete tihendamine
PolyBase saab lugeda gzip tihendatud andmeid. Kui teil on võimalik tihendada andmete gzip failide seejärel minimeerida andmeid on üle võrgu summa.

### <a name="multiple-files"></a>Mitme faili
Suurte tabelite mitu failidesse osadeks mitte ainult aitab ekspordi kiiruse parandamiseks, see aitab ka edastamine re-startability ja üldine hallatavust andmed üks kord selle Azure Bloobivahemälu salvestusruumi. Üks PolyBase palju kena funktsioonid on, et see lugeda kõik failid kausta sees ja kohelge seda kui: ühe tabeli. Seetõttu on hea mõte eristamiseks failid iga tabeli jaoks eraldi kausta.

PolyBase toetab ka funktsiooni nimetatakse "Rekursiivsed kausta läbikäiku". Selle funktsiooni abil tugevdada organisatsiooni eksporditud andmete parandamiseks oma andmete haldamiseks.

PolyBase andmete laadimine kohta leiate lisateavet teemast [Kasutamine PolyBase andmeid SQL-i andmebaas laadimiseks][].


## <a name="next-steps"></a>Järgmised sammud
Migreerimise kohta leiate lisateavet teemast [migreerimine teie lahendus SQL-i andmebaas][].
Veel arengu näpunäiteid teemast [arengu ülevaade][].

<!--Image references-->

<!--Article references-->
[AZCopy]: ../storage/storage-use-azcopy.md
[ADF kopeerimine]: ../data-factory/data-factory-data-movement-activities.md 
[ADF näidised]: ../data-factory/data-factory-samples.md
[ADF Copy examples]: ../data-factory/data-factory-copy-activity-tutorial-using-visual-studio.md
[arengu ülevaade]: sql-data-warehouse-overview-develop.md
[Teie lahendus migreerimine SQL-andmebaas]: sql-data-warehouse-overview-migrate.md
[SQL Data Warehouse development overview]: sql-data-warehouse-overview-develop.md
[Kasutage bcp andmete laadimiseks SQL-andmebaas]: sql-data-warehouse-load-with-bcp.md
[Kasutage PolyBase andmete laadimiseks SQL-andmebaas]: sql-data-warehouse-get-started-load-with-polybase.md


<!--MSDN references-->

<!--Other Web references-->
[Azure'i andmed Factory]: http://azure.microsoft.com/services/data-factory/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[ExpressRoute dokumentatsioon]: http://azure.microsoft.com/documentation/services/expressroute/

[tootmise versioon]: http://aka.ms/downloadazcopy/
[eelvaateversiooni]: http://aka.ms/downloadazcopypr/
[ADO.NET-adapterit, sihtkoht]: https://msdn.microsoft.com/library/bb934041.aspx
[SSIS-i dokumendid]: https://msdn.microsoft.com/library/ms141026.aspx
