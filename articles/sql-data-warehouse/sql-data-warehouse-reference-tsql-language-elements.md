<properties
   pageTitle="SQL-i andmete ladu Transact-SQL-i keele elemendid | Microsoft Azure'i"
   description="Viide sisu Transact-SQL-i keele elemendid, mida kasutatakse SQL-i andmebaas linkide loend."
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
   ms.date="09/08/2016"
   ms.author="barbkess;sonyama;kevin"/>

# <a name="language-elements"></a>Keele elemendid

## <a name="core-elements"></a>Peamised

- [süntaks: põhimõtted](https://msdn.microsoft.com/library/ms177563.aspx)
- [objekti nime reeglid](https://msdn.microsoft.com/library/ms175874.aspx)
- [reserveeritud märksõnad](https://msdn.microsoft.com/library/ms189822.aspx)
- [kogumite](https://msdn.microsoft.com/library/ff848763.aspx)
- [Kommentaarid](https://msdn.microsoft.com/library/ms181627.aspx)
- [konstantide](https://msdn.microsoft.com/library/ms179899.aspx)
- [andmetüübid](https://msdn.microsoft.com/library/ms187752.aspx)
- [KÄIVITADA](https://msdn.microsoft.com/library/ms188332.aspx)
- [avaldised](https://msdn.microsoft.com/library/ms190286.aspx)
- [TAPPA](https://msdn.microsoft.com/library/ms173730.aspx)
- [IDENTITEEDI atribuudi lahendus](https://msdn.microsoft.com/library/ms186775.aspx)
- [PRINTIMINE](https://msdn.microsoft.com/library/ms176047.aspx)
- [KASUTAMINE](https://msdn.microsoft.com/library/ms188366.aspx)

## <a name="batches-control-of-flow-and-variables"></a>Töölehed, juhtelemendi andmevoo ja muutujad

- [ALUSTAGE... LÕPETA](https://msdn.microsoft.com/library/ms190487.aspx)
- [LEHEKÜLJEPIIR](https://msdn.microsoft.com/library/ms181271.aspx)
- [DEKLAREERIDA@local_variable](https://msdn.microsoft.com/library/ms188927.aspx)
- [KUI... VEEL](https://msdn.microsoft.com/library/ms182717.aspx)
- [RAISERROR](https://msdn.microsoft.com/library/ms178592.aspx)
- [SET@local_variable](https://msdn.microsoft.com/library/ms189484.aspx)
- [PÕHJUSTADA](https://msdn.microsoft.com/library/ee677615.aspx)
- [PROOVIGE... JAOLE JUBA](https://msdn.microsoft.com/library/ms175976.aspx)
- [AEGA](https://msdn.microsoft.com/library/ms178642.aspx)

## <a name="operators"></a>Tehtemärgid
- [+ (Lisa)](https://msdn.microsoft.com/library/ms178565.aspx)
- [+ (Stringi ühendamine)](https://msdn.microsoft.com/library/ms177561.aspx)
- [– (Negatiivne)](https://msdn.microsoft.com/library/ms189480.aspx)
- [-(Lahutamine)](https://msdn.microsoft.com/library/ms189518.aspx)
- [* (Korrutamine)](https://msdn.microsoft.com/library/ms176019.aspx)
- [/ (Jagamine)](https://msdn.microsoft.com/library/ms175009.aspx)
- [Modulo](https://msdn.microsoft.com/library/ms190279.aspx)

## <a name="wildcard-characters-to-match"></a>Metamärkide nimevälja sobitamiseks
- [= (Võrdne)](https://msdn.microsoft.com/library/ms175118.aspx)
- [> (Suurem kui)](https://msdn.microsoft.com/library/ms178590.aspx)
- [< (Vähem kui)](https://msdn.microsoft.com/library/ms179873.aspx)
- [> = (suur kui või võrdne)](https://msdn.microsoft.com/library/ms181567.aspx)
- [< = (väiksem või võrdne)](https://msdn.microsoft.com/library/ms174978.aspx)
- [<> (Pole võrdne väärtusega)](https://msdn.microsoft.com/library/ms176020.aspx)
- [! = (Pole võrdne väärtusega)](https://msdn.microsoft.com/library/ms190296.aspx)
- [JA](https://msdn.microsoft.com/library/ms188372.aspx)
- [VAHEL](https://msdn.microsoft.com/library/ms187922.aspx)
- [ON OLEMAS](https://msdn.microsoft.com/library/ms188336.aspx)
- [TOLLI](https://msdn.microsoft.com/library/ms177682.aspx)
- [[POLE] ON NULL](https://msdn.microsoft.com/library/ms188795.aspx)
- [NAGU](https://msdn.microsoft.com/library/ms179859.aspx)
- [POLE](https://msdn.microsoft.com/library/ms189455.aspx)
- [VÕI](https://msdn.microsoft.com/library/ms188361.aspx)

### <a name="bitwise-operators"></a>Bititaseme tehtemärgid

- [& (Bititaseme ja)](https://msdn.microsoft.com/library/ms174965.aspx)
- [| (Bititaseme või)](https://msdn.microsoft.com/library/ms186714.aspx)
- [^ (Bititaseme eksklusiivse OR)](https://msdn.microsoft.com/library/ms190277.aspx)
- [~ (Bititaseme mitte)](https://msdn.microsoft.com/library/ms173468.aspx)
- [^ = (Bititaseme eksklusiivse või VÕRDUB)](https://msdn.microsoft.com/library/cc627413.aspx)
- [| = (Bititaseme või võrdne)](https://msdn.microsoft.com/library/cc627409.aspx)
- [& = (bititaseme ja VÕRDUB)](https://msdn.microsoft.com/library/cc627427.aspx)

## <a name="functions"></a>Funktsioonid

- [@@DATEFIRST](https://msdn.microsoft.com/library/ms187766.aspx)
- [@@ERROR](https://msdn.microsoft.com/library/ms188790.aspx)
- [@@LANGUAGE](https://msdn.microsoft.com/library/ms177557.aspx)
- [@@SPID](https://msdn.microsoft.com/library/ms189535.aspx)
- [@@TRANCOUNT](https://msdn.microsoft.com/library/ms187967.aspx)
- [@@VERSION](https://msdn.microsoft.com/library/ms177512.aspx)
- [ABS](https://msdn.microsoft.com/library/ms189800.aspx)
- [ACOS](https://msdn.microsoft.com/library/ms178627.aspx)
- [ASCII](https://msdn.microsoft.com/library/ms177545.aspx)
- [ASIN](https://msdn.microsoft.com/library/ms181581.aspx)
- [ATAN](https://msdn.microsoft.com/library/ms181746.aspx)
- [ATN2](https://msdn.microsoft.com/library/ms173854.aspx)
- [BINARY_CHECKSUM](https://msdn.microsoft.com/library/ms173784.aspx)
- [JUHUL](https://msdn.microsoft.com/library/ms181765.aspx)
- [CAST ja teisendamine](https://msdn.microsoft.com/library/ms187928.aspx)
- [CEILING](https://msdn.microsoft.com/library/ms189818.aspx)
- [CHAR](https://msdn.microsoft.com/library/ms187323.aspx)
- [CHARINDEX](https://msdn.microsoft.com/library/ms186323.aspx)
- [KONTROLLSUMMA](https://msdn.microsoft.com/library/ms189788.aspx)
- [LIITUMIST](https://msdn.microsoft.com/library/ms190349.aspx)
- [COL_NAME](https://msdn.microsoft.com/library/ms174974.aspx)
- [COLLATIONPROPERTY](https://msdn.microsoft.com/library/ms190305.aspx)
- [CONCAT](https://msdn.microsoft.com/library/hh231515.aspx)
- [COS](https://msdn.microsoft.com/library/ms188919.aspx)
- [COT](https://msdn.microsoft.com/library/ms188921.aspx)
- [COUNT](https://msdn.microsoft.com/library/ms175997.aspx)
- [COUNT_BIG](https://msdn.microsoft.com/library/ms190317.aspx)
- [CUME_DIST](https://msdn.microsoft.com/library/hh231078.aspx)
- [CURRENT_TIMESTAMP](https://msdn.microsoft.com/library/ms188751.aspx)
- [CURRENT_USER](https://msdn.microsoft.com/library/ms176050.aspx)
- [DATABASEPROPERTYEX](https://msdn.microsoft.com/library/ms186823.aspx)
- [DATALENGTH](https://msdn.microsoft.com/library/ms173486.aspx)
- [DATEADD](https://msdn.microsoft.com/library/ms186819.aspx)
- [DATEDIFF](https://msdn.microsoft.com/library/ms189794.aspx)
- [DATEFROMPARTS](https://msdn.microsoft.com/library/hh213228.aspx)
- [DATENAME](https://msdn.microsoft.com/library/ms174395.aspx)
- [DATEPART](https://msdn.microsoft.com/library/ms174420.aspx)
- [DATETIME2FROMPARTS](https://msdn.microsoft.com/library/hh213312.aspx)
- [DATETIMEFROMPARTS](https://msdn.microsoft.com/library/hh213233.aspx)
- [DATETIMEOFFSETFROMPARTS](https://msdn.microsoft.com/library/hh231077.aspx)
- [PÄEVA](https://msdn.microsoft.com/library/ms176052.aspx)
- [DB_ID](https://msdn.microsoft.com/library/ms186274.aspx)
- [DB_NAME](https://msdn.microsoft.com/library/ms189753.aspx)
- [DEGREES](https://msdn.microsoft.com/library/ms178566.aspx)
- [DENSE_RANK](https://msdn.microsoft.com/library/ms173825.aspx)
- [ERINEVUS](https://msdn.microsoft.com/library/ms188753.aspx)
- [EOMONTH](https://msdn.microsoft.com/library/hh213020.aspx)
- [ERROR_MESSAGE](https://msdn.microsoft.com/library/ms190358.aspx)
- [ERROR_NUMBER](https://msdn.microsoft.com/library/ms175069.aspx)
- [ERROR_PROCEDURE](https://msdn.microsoft.com/library/ms188398.aspx)
- [ERROR_SEVERITY](https://msdn.microsoft.com/library/ms178567.aspx)
- [ERROR_STATE](https://msdn.microsoft.com/library/ms180031.aspx)
- [EXP](https://msdn.microsoft.com/library/ms179857.aspx)
- [FIRST_VALUE](https://msdn.microsoft.com/library/hh213018.aspx)
- [FLOOR](https://msdn.microsoft.com/library/ms178531.aspx)
- [GETDATE](https://msdn.microsoft.com/library/ms188383.aspx)
- [GETUTCDATE](https://msdn.microsoft.com/library/ms178635.aspx)
- [HAS_DBACCESS](https://msdn.microsoft.com/library/ms187718.aspx)
- [HASHBYTES](https://msdn.microsoft.com/library/ms174415.aspx)
- [INDEXPROPERTY](https://msdn.microsoft.com/library/ms187729.aspx)
- [ISDATE](https://msdn.microsoft.com/library/ms187347.aspx)
- [ISNULL](https://msdn.microsoft.com/library/ms184325.aspx)
- [ISNUMERIC](https://msdn.microsoft.com/library/ms186272.aspx)
- [LAG](https://msdn.microsoft.com/library/hh231256.aspx)
- [LAST_VALUE](https://msdn.microsoft.com/library/hh231517.aspx)
- [JUHI](https://msdn.microsoft.com/library/hh213125.aspx)
- [VASAKULE](https://msdn.microsoft.com/library/ms177601.aspx)
- [FUNKTSIOON LEN](https://msdn.microsoft.com/library/ms190329.aspx)
- [LOG](https://msdn.microsoft.com/library/ms190319.aspx)
- [LOG10](https://msdn.microsoft.com/library/ms175121.aspx)
- [LOWER](https://msdn.microsoft.com/library/ms174400.aspx)
- [FUNKTSIOONID LTRIM](https://msdn.microsoft.com/library/ms177827.aspx)
- [MAX](https://msdn.microsoft.com/library/ms187751.aspx)
- [MIN](https://msdn.microsoft.com/library/ms179916.aspx)
- [KUU](https://msdn.microsoft.com/library/ms187813.aspx)
- [NCHAR](https://msdn.microsoft.com/library/ms182673.aspx)
- [NTILE](https://msdn.microsoft.com/library/ms175126.aspx)
- [NULLIF](https://msdn.microsoft.com/library/ms177562.aspx)
- [OBJECT_ID](https://msdn.microsoft.com/library/ms190328.aspx)
- [OBJEKTI NIMI](https://msdn.microsoft.com/library/ms186301.aspx)
- [OBJECTPROPERTY](https://msdn.microsoft.com/library/ms176105.aspx)
- [OIBJECTPROPERTYEX](https://msdn.microsoft.com/library/ms188390.aspx)
- [ODBCS skalaarfunktsioonid](https://msdn.microsoft.com/library/bb630290.aspx)
- [Klausel üle](https://msdn.microsoft.com/library/ms189461.aspx)
- [PARSENAME](https://msdn.microsoft.com/library/ms188006.aspx)
- [PATINDEX](https://msdn.microsoft.com/library/ms188395.aspx)
- [PERCENTILE_CONT](https://msdn.microsoft.com/library/hh231473.aspx)
- [PERCENTILE_DISC](https://msdn.microsoft.com/library/hh231327.aspx)
- [PERCENT_RANK](https://msdn.microsoft.com/library/hh213573.aspx)
- [PII](https://msdn.microsoft.com/library/ms189512.aspx)
- [POWER](https://msdn.microsoft.com/library/ms174276.aspx)
- [QUOTENAME](https://msdn.microsoft.com/library/ms176114.aspx)
- [RADIANS](https://msdn.microsoft.com/library/ms189742.aspx)
- [RAND](https://msdn.microsoft.com/library/ms177610.aspx)
- [RANK](https://msdn.microsoft.com/library/ms176102.aspx)
- [ASENDAMINE](https://msdn.microsoft.com/library/ms186862.aspx)
- [JUHENDIST](https://msdn.microsoft.com/library/ms174383.aspx)
- [VASTUPIDI](https://msdn.microsoft.com/library/ms180040.aspx)
- [PAREMALE](https://msdn.microsoft.com/library/ms177532.aspx)
- [ROUND](https://msdn.microsoft.com/library/ms175003.aspx)
- [REA_NR](https://msdn.microsoft.com/library/ms186734.aspx)
- [RTRIM](https://msdn.microsoft.com/library/ms178660.aspx)
- [SCHEMA_ID](https://msdn.microsoft.com/library/ms188797.aspx)
- [SCHEMA_NAME](https://msdn.microsoft.com/library/ms175068.aspx)
- [SERVERPROPERTY](https://msdn.microsoft.com/library/ms174396.aspx)
- [SESSION_USER](https://msdn.microsoft.com/library/ms177587.aspx)
- [LOGI](https://msdn.microsoft.com/library/ms188420.aspx)
- [SIN](https://msdn.microsoft.com/library/ms188377.aspx)
- [SMALLDATETIMEFROMPARTS](https://msdn.microsoft.com/library/hh213396.aspx)
- [SOUNDEX](https://msdn.microsoft.com/library/ms187384.aspx)
- [RUUMI](https://msdn.microsoft.com/library/ms187950.aspx)
- [SQL_VARIANT_PROPERTY](https://msdn.microsoft.com/library/ms178550.aspx)
- [SQRT](https://msdn.microsoft.com/library/ms176108.aspx)
- [RUUTU](https://msdn.microsoft.com/library/ms173569.aspx)
- [STATS_DATE](https://msdn.microsoft.com/library/ms190330.aspx)
- [FUNKTSIOON STDEV](https://msdn.microsoft.com/library/ms190474.aspx)
- [FUNKTSIOON STDEVP](https://msdn.microsoft.com/library/ms176080.aspx)
- [STR](https://msdn.microsoft.com/library/ms189527.aspx)
- [ASJADE](https://msdn.microsoft.com/library/ms188043.aspx)
- [ALAMSTRINGI](https://msdn.microsoft.com/library/ms187748.aspx)
- [SUMMA](https://msdn.microsoft.com/library/ms187810.aspx)
- [SUSER_SNAME](https://msdn.microsoft.com/library/ms174427.aspx)
- [SWITCHOFFSET](https://msdn.microsoft.com/library/bb677244.aspx)
- [SYSDATETIME](https://msdn.microsoft.com/library/bb630353.aspx)
- [SYSDATETIMEOFFSET](https://msdn.microsoft.com/library/bb677334.aspx)
- [SYSTEM_USER](https://msdn.microsoft.com/library/ms179930.aspx)
- [SYSUTCDATETIME](https://msdn.microsoft.com/library/bb630387.aspx)
- [TAN](https://msdn.microsoft.com/library/ms190338.aspx)
- [TERTIARY_WEIGHTS](https://msdn.microsoft.com/library/ms186881.aspx)
- [TIMEFROMPARTS](https://msdn.microsoft.com/library/hh213398.aspx)
- [TODATETIMEOFFSET](https://msdn.microsoft.com/library/bb630335.aspx)
- [TYPE_ID](https://msdn.microsoft.com/library/ms181628.aspx)
- [TYPE_NAME](https://msdn.microsoft.com/library/ms189750.aspx)
- [TYPEPROPERTY](https://msdn.microsoft.com/library/ms188419.aspx)
- [UNICODE](https://msdn.microsoft.com/library/ms180059.aspx)
- [UPPER](https://msdn.microsoft.com/library/ms180055.aspx)
- [KASUTAJA](https://msdn.microsoft.com/library/ms186738.aspx)
- [USER_NAME](https://msdn.microsoft.com/library/ms188014.aspx)
- [VAR](https://msdn.microsoft.com/library/ms186290.aspx)
- [VARP](https://msdn.microsoft.com/library/ms188735.aspx)
- [AASTA](https://msdn.microsoft.com/library/ms186313.aspx)
- [XACT_STATE](https://msdn.microsoft.com/library/ms189797.aspx)

## <a name="transactions"></a>Tehinguid

- [tehinguid](https://msdn.microsoft.com/library/mt204031.aspx)

## <a name="diagnostic-sessions"></a>Diagnostika seansid

- [DIAGNOSTIKA SEANSI LOOMINE](https://msdn.microsoft.com/library/mt204029.aspx)

## <a name="procedures"></a>Juhised

- [sp_addrolemember](https://msdn.microsoft.com/library/ms187750.aspx)
- [sp_columns](https://msdn.microsoft.com/library/ms176077.aspx)
- [sp_configure](https://msdn.microsoft.com/library/ms188787.aspx)
- [sp_datatype_info_90](https://msdn.microsoft.com/library/mt204014.aspx)
- [sp_droprolemember](https://msdn.microsoft.com/library/ms188369.aspx)
- [sp_execute](https://msdn.microsoft.com/library/ff848746.aspx)
- [sp_executesql Klõpsake](https://msdn.microsoft.com/library/ms188001.aspx)
- [sp_fkeys](https://msdn.microsoft.com/library/ms175090.aspx)
- [sp_pdw_add_network_credentials](https://msdn.microsoft.com/library/mt204011.aspx)
- [sp_pdw_database_encryption](https://msdn.microsoft.com/library/mt219360.aspx)
- [sp_pdw_database_encryption_regenerate_system_keys](https://msdn.microsoft.com/library/mt204033.aspx)
- [sp_pdw_log_user_data_masking](https://msdn.microsoft.com/library/mt204023.aspx)
- [sp_pdw_remove_network_credentials](https://msdn.microsoft.com/library/mt204038.aspx)
- [sp_pkeys](https://msdn.microsoft.com/library/ms189813.aspx)
- [sp_prepare](https://msdn.microsoft.com/library/ff848808.aspx)
- [sp_spaceused](https://msdn.microsoft.com/library/ms188776.aspx)
- [sp_special_columns_100](https://msdn.microsoft.com/library/mt204025.aspx)
- [sp_sproc_columns](https://msdn.microsoft.com/library/ms182705.aspx)
- [sp_statistics](https://msdn.microsoft.com/library/ms173842.aspx)
- [sp_tables](https://msdn.microsoft.com/library/ms186250.aspx)
- [sp_unprepare](https://msdn.microsoft.com/library/ff848735.aspx)



## <a name="set-statements"></a>Laused määramine

- [ANSI_DEFAULTS MÄÄRAMINE](https://msdn.microsoft.com/library/ms188340.aspx)
- [ANSI_NULL_DFLT_OFF MÄÄRAMINE](https://msdn.microsoft.com/library/ms187356.aspx)
- [ANSI_NULL_DFLT_ON MÄÄRAMINE](https://msdn.microsoft.com/library/ms187375.aspx)
- [ANSI_NULLS MÄÄRAMINE](https://msdn.microsoft.com/library/ms188048.aspx)
- [ANSI_PADDING MÄÄRAMINE](https://msdn.microsoft.com/library/ms187403.aspx)
- [ANSI_WARNINGS MÄÄRAMINE](https://msdn.microsoft.com/library/ms190368.aspx)
- [ARITHABORT MÄÄRAMINE](https://msdn.microsoft.com/library/ms190306.aspx)
- [ARITHIGNORE MÄÄRAMINE](https://msdn.microsoft.com/library/ms184341.aspx)
- [CONCAT_NULL_YIELDS_NULL MÄÄRAMINE](https://msdn.microsoft.com/library/ms176056.aspx)
- [DATEFIRST MÄÄRAMINE](https://msdn.microsoft.com/library/ms181598.aspx)
- [DATEFORMAT MÄÄRAMINE](https://msdn.microsoft.com/library/ms189491.aspx)
- [FMTONLY MÄÄRAMINE](https://msdn.microsoft.com/library/ms173839.aspx)
- [IMPLICIT_TRANSACITONS MÄÄRAMINE](https://msdn.microsoft.com/library/ms187807.aspx)
- [LOCK_TIMEOUT MÄÄRAMINE](https://msdn.microsoft.com/library/ms189470.aspx)
- [NUMBERIC_ROUNDABORT MÄÄRAMINE](https://msdn.microsoft.com/library/ms188791.aspx)
- [QUOTED_IDENTIFIER MÄÄRAMINE](https://msdn.microsoft.com/library/ms174393.aspx)
- [ROWCOUNT MÄÄRAMINE](https://msdn.microsoft.com/library/ms188774.aspx)
- [TEXTSIZE MÄÄRAMINE](https://msdn.microsoft.com/library/ms186238.aspx)
- [SEADMINE TEHINGU ERALDAMISE TASEMEKS](https://msdn.microsoft.com/library/ms173763.aspx)
- [XACT_ABORT MÄÄRAMINE](https://msdn.microsoft.com/library/ms188792.aspx)


## <a name="next-steps"></a>Järgmised sammud
Viide lisateabe saamiseks vt [SQL-i andmebaas viide ülevaade][].

<!--Image references-->

<!--Article references-->
[SQL-i andmebaas viide ülevaade]: sql-data-warehouse-overview-reference.md

<!--MSDN references-->
