<properties
   pageTitle="Arvuta power (ülevaade) SQL Azure'i andmebaas haldamine | Microsoft Azure'i"
   description="Jõudluse skaala läbi funktsioonidele SQL Azure'i andmebaas. Välja, reguleerides DWUs mastaapimiseks või viige ja jätkake Arvuta ressursside kulude salvestamiseks."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/03/2016"
   ms.author="barbkess;sonyama"/>

# <a name="manage-compute-power-in-azure-sql-data-warehouse-overview"></a>Arvuta power (ülevaade) SQL Azure'i andmebaas haldamine

> [AZURE.SELECTOR]
- [Ülevaade](sql-data-warehouse-manage-compute-overview.md)
- [Portaal](sql-data-warehouse-manage-compute-portal.md)
- [PowerShelli](sql-data-warehouse-manage-compute-powershell.md)
- [ÜLEJÄÄNUD](sql-data-warehouse-manage-compute-rest-api.md)
- [TSQL](sql-data-warehouse-manage-compute-tsql.md)

SQL-i andmebaas arhitektuur eraldab salvestusruumi ja Arvuta, mis võimaldab iga mastaapimiseks sõltumatult. Selle tulemusena skaala välja jõudlusprobleeme salvestamise kulud ainult makstes jõudluse, kui neid vajate. 

See ülevaade kirjeldatakse järgmisi jõudluse skaala-out võimalusi SQL-i andmebaas ja annab soovitused, kuidas ja millal neid kasutada. 

- Skaala arvutada power, reguleerides [andmete ladu üksused (DWUs)][]
- Peatamine või jätkamine Arvuta ressursid

<a name="scale-performance-bk"></a>

## <a name="scale-performance"></a>Skaala jõudlus

Klõpsake SQL-i andmebaas, saate kiiresti skaala välja või tagasi suureneb või väheneb Arvuta ressursse CPU, mälu ja I/O läbilaskevõime jõudlus. Jõudluse mastaapida, teha pole vaja reguleerida arvu [andmete ladu (DWUs) üksused][] , mis eraldab andmebaasi SQL-i andmebaas. SQL-i andmebaas kiiresti muudatuse ja tegeleb kõikide aluseks muudatuste või riistvara.

Läinud on päevad, kus teil tuleb uurimine tüübi protsessorite mälumahu või tüübi talletusruumi peate teie andmebaas on hea jõudlus. Teie andmebaas paigutades pilveteenuses enam peate madala riistvara küsimustega tegeleda. Selle asemel SQL-i andmebaas palub teil seda küsimust: kui kiiresti soovite andmete analüüsimine? 

### <a name="how-do-i-scale-performance"></a>Kuidas suurust jõudlus?

Elastically suurendamiseks või vähendamiseks oma Arvuta power, lihtsalt muuta oma andmebaasi [andmete ladu ühikud (DWUs)][] sätet. Jõudluse suurendamine lineaarses lisamisel veel DWU.  Suurema DWU tasemel, peate lisama rohkem kui 100 DWUs teade jõudluse märkimisväärselt täiustada. Selleks et saaksite valida mõtestatud viib DWUs, pakume DWU taset, mis annab parima tulemuse.
 
DWUs kohandamiseks saate kõik need üksikud meetodid.

- [Skaala arvutada power Azure'i portaal][]
- [Skaala arvutada power PowerShelli abil][]
- [Skaala arvutada power REST API-de abil][]
- [Skaala arvutada power TSQL abil][]

### <a name="how-many-dwus-should-i-use"></a>Kui palju DWUs kasutada?
 
SQL-i andmebaas jõudluse skaala lineaarses ja muuta ühe Arvuta skaala teise (st 100 DWUs kuni 2000 DWUs) juhtub sekundit. See võimaldab paindlikult eksperimenteerida eri DWU sätteid, kuni olete kindlaks teinud teie stsenaariumi kõige sobivam.

Et aru saada, mis on teie optimaalne DWU väärtus, proovige skaleerimist üles ja töötab mõni päringute pärast andmete laadimine. Kuna skaleerimist on kiire, võite proovida jõudluse tund või vähem erinevate tasemete arvu. Pidage meeles, et SQL-i andmebaas on mõeldud suurte andmehulkade töötlemine ja näha oma true võimalused mastaapimist, eriti veebisaidil suuremat skaalasid pakume, peaksite kasutama suure andmehulga, mis meetodid või ületab 1 TB.

Soovitused oma töökoormus parim DWU leidmiseks:

1. Alata andmebaas arendatakse, valides DWUs väike arv.  Hea alguspunkti on DW400 või DW200.
2. Oma rakenduse jõudluse, järgides arvu DWUs valitud märkate jõudluse jälgimist.
3. Kindlaks teha, kui palju kiiremini või aeglasemalt jõudluse peaks olema teile jõuavad optimaalse jõudluse teie vajadustele, eeldades lineaarset skaala.
4. Suurendage või vähendage DWUs arvu proportsionaalselt, kuidas palju kiiremini või aeglasemalt, mille soovite oma töökoormus sooritamiseks. Teenus on kiiresti ja reguleerimine Arvuta ressursid uue DWU nõuetele.
5. Jätkake kohanduste seni, kuni jõuate optimaalse jõudluse tase teie ettevõtte vajadustele.

### <a name="when-should-i-scale-dwus"></a>Millal tuleks DWUs suurust?

Kui teil on vaja kiiremini tulemusi, Suurendage oma DWUs ja suurem jõudluse eest maksta.  Kui vajate vähem arvutada power, oma DWUs vähendamiseks ja maksma ainult vajalik. 

Soovitused mastaapimiseks DWUs järgmistel juhtudel:

1. Kui teie taotlus on kõikuva töökoormus, skaala DWU tasanditel üles või alla, et majutada peaks ja madala punkte. Näiteks kui teie töökoormus peaks tavaliselt kuu lõpu seisuga, plaani lisada rohkem DWUs tipp nende päeva jooksul, siis pärast tippväärtus perioodi skaalal.
2. Raske andmete laadimise või teisendus toimingute sooritamiseks skaala kuni DWUs nii, et teie andmed on saadaval kiiremini.

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Paus Arvuta

[AZURE.INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

Andmebaasi peatamiseks kasutada nende viisidega.

- [Paus Arvuta Azure'i portaal][]
- [Paus Arvuta PowerShelli abil][]
- [Paus Arvuta REST API-de abil][]

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Elulookirjelduse Arvuta

[AZURE.INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

Andmebaasi jätkamiseks kasutada kõiki neid viisidega.

- [Elulookirjelduse Arvuta Azure'i portaal][]
- [Elulookirjelduse Arvuta PowerShelli abil][]
- [Elulookirjelduse Arvuta REST API-de abil][]

## <a name="permissions"></a>Õigused

Andmebaasi skaleerimist vajavad kirjeldatud [Muuda andmebaasi][]õigused.  [SQL-i DB kaasautor][] õigus, spetsiaalselt Microsoft.Sql/servers/databases/action vajavad Peata ja Jätka.

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Järgmised sammud
Palun vaadake järgmisi artikleid, et aidata teil mõista mõned täiendavad tootluse põhimõtet:

- [Töökoormus ja kokkulangevus haldus][]
- [Tabeli kujundus ülevaade][]
- [Tabeli jaotuse.][]
- [Tabeli indekseerimine][]
- [Tabeli jagamine][]
- [Tabeli statistika][]
- [Head tavad][]

<!--Image reference-->

<!--Article references-->
[andmete ladu üksused (DWUs)]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units

[Skaala arvutada power Azure'i portaal]: ./sql-data-warehouse-manage-compute-portal.md#scale-compute-bk
[Skaala arvutada power PowerShelli abil]: ./sql-data-warehouse-manage-compute-powershell.md#scale-compute-bk
[Skaala arvutada power REST API-de abil]: ./sql-data-warehouse-manage-compute-rest-api.md#scale-compute-bk
[Skaala arvutada power TSQL abil]: ./sql-data-warehouse-manage-compute-tsql.md#scale-compute-bk

[capacity limits]: ./sql-data-warehouse-service-capacity-limits.md

[Paus Arvuta Azure'i portaal]:  ./sql-data-warehouse-manage-compute-portal.md#pause-compute-bk
[Paus Arvuta PowerShelli abil]: ./sql-data-warehouse-manage-compute-powershell.md#pause-compute-bk
[Paus Arvuta REST API-de abil]: ./sql-data-warehouse-manage-compute-rest-api.md#pause-compute-bk

[Elulookirjelduse Arvuta Azure'i portaal]:  ./sql-data-warehouse-manage-compute-portal.md#resume-compute-bk
[Elulookirjelduse Arvuta PowerShelli abil]: ./sql-data-warehouse-manage-compute-powershell.md#resume-compute-bk
[Elulookirjelduse Arvuta REST API-de abil]: ./sql-data-warehouse-manage-compute-rest-api.md#resume-compute-bk

[Töökoormus ja kokkulangevus haldus]: ./sql-data-warehouse-develop-concurrency.md
[Tabeli kujundus ülevaade]: ./sql-data-warehouse-tables-overview.md
[Tabeli jaotuse.]: ./sql-data-warehouse-tables-distribute.md
[Tabeli indekseerimine]: ./sql-data-warehouse-tables-index.md
[Tabeli jagamine]: ./sql-data-warehouse-tables-partition.md
[Tabeli statistika]: ./sql-data-warehouse-tables-statistics.md
[Head tavad]: ./sql-data-warehouse-best-practices.md 
[development overview]: ./sql-data-warehouse-overview-develop.md

[SQL-i DB kaasautor]: ../active-directory/role-based-access-built-in-roles.md#sql-db-contributor

<!--MSDN references-->
[MUUDA ANDMEBAASI]: https://msdn.microsoft.com/library/mt204042.aspx

<!--Other Web references-->
[Azure portal]: http://portal.azure.com/
