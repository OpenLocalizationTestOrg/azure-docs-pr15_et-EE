<properties
   pageTitle="Pilvepõhine järjepidevuse – kustutatud andmebaasi - SQL-andmebaasi taastamine | Microsoft Azure'i"
   description="Lisateavet punkti õigel ajal taastada, mis võimaldab tagasi pöörata Azure'i SQL-andmebaasi eelmises punktis ajas (kuni 35 päeva)."
   services="sql-database"
   documentationCenter=""
   authors="stevestein"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/01/2016"
   ms.author="sstein"/>

# <a name="recover-an-azure-sql-database-using-automated-database-backups"></a>Azure'i SQL-andmebaasiga, kasutades automatiseeritud andmebaasi taastamine

SQL-andmebaasi pakub kolm võimalust andmebaasi taastamine [SQL-andmebaasi automaatse varundamise funktsiooni](sql-database-automated-backups.md)abil. Andmebaasi saab taastada varukoopiatest teenuse algatatud nende [säilitusperiood](sql-database-service-tiers.md) :

- Uue andmebaasi samas loogiline serveris taastatud määratud punkti ajas säilitamise aja jooksul. 
- Andmebaasi taastada kustutatud andmebaasi kustutamise aeg samas loogiline serveris.
- Uue andmebaasi mis tahes loogikaväärtused serveris taastada viimase päeva varukoopiate geo kopeeritud bloobimälu (RA GRS) klõpsake mis tahes piirkonnas.

Samuti saate [SQL-andmebaasi automaatse varundamise funktsiooni](sql-database-automated-backups.md) mis tahes loogikaväärtused serveris piirkonnas, mis vastaks ülekandega praeguse SQL-andmebaasi luua [andmebaasi kopeerida](sql-database-copy.md) . Abil saate andmebaasi kopeerimine ja [eksportimine mõnda BACPAC](sql-database-export.md) arhiivida ülekandega ühtsete koopia pikemaks Lisaks teie säilitusperiood andmebaasi või koopia andmebaasi edastada kohapealses või Azure VM SQL serveri eksemplar.

## <a name="recovery-time"></a>Aeg

Taastamise ajal abil automatiseeritud andmebaasi varundamine andmebaasi taastamine mõjutab mitmest tegurist: 
 - Andmebaasi maht
 - Andmebaasi jõudluse tase
 - Tehingu logid seotud arv
 - Tegevuse, mis peab olema uuesti esitada taastamiseks taastamise summa
 - Võrgu läbilaskevõime kui muule alale on taastamine 
 - Piirkonna sihtkoht töödeldavate samaaegseid taastamine taotluste arv. 
 
 Väga suurte või aktiivsete andmebaasi taastamine võib kuluda mitu tundi. Kui piirkonnas on pikaajaline katkestuste, on võimalik, saab suure hulga Geo-taastamine kutsed ei töödelda muude piirkondade. Kui seal on suur hulk taotlusi see võib suurendada taastamise aeg andmebaaside selle piirkonna jaoks. Enamik andmebaasi taastab täieliku 12 tunni jooksul.

 On hulgi taastamine pole sisseehitatud funktsioonid. Funktsiooni [Azure'i SQL-andmebaasi: täielikult serveri](https://gallery.technet.microsoft.com/Azure-SQL-Database-Full-82941666) script on üks võimalus, aga mitte selle ülesande näide.

> [AZURE.IMPORTANT] Automaatse varundamise funktsiooni abil taastamiseks peab kuuluma SQL serveri kaasautori roll tellimus või tellimuse omanik. Saate taastada Azure'i portaalis, PowerShelli või REST API-ga. Ei saa kasutada Transact-SQL-i. 

## <a name="point-in-time-restore"></a>Punkti õigel ajal taastamine

Punkti õigel ajal taastamine võimaldab teil taastada olemasoleva andmebaasiga uue andmebaasi varasema nimega sama loogiline serveri [SQL-andmebaasi automaatse varundamise funktsiooni](sql-database-automated-backups.md)abil. Olemasoleva andmebaasi ei saa üle kirjutada. Saate taastada varasemasse ajas [Azure portaali](sql-database-point-in-time-restore-portal.md), [PowerShelli](sql-database-point-in-time-restore-powershell.md) või [REST API -ga](https://msdn.microsoft.com/library/azure/mt163685.aspx).

> [AZURE.SELECTOR]
- [Punkti õigel ajal taastamine: Azure'i portaal](sql-database-point-in-time-restore-portal.md)
- [Punkti õigel ajal taastamine: PowerShelli](sql-database-point-in-time-restore-powershell.md)

Andmebaasi saab taastada jõudlusega või elastne pool. Peate veenduge, et teil on piisavalt DTU kvoodi loogiline serveri või elastne pool. Pidage meeles taastamine loob uue andmebaasi ja teenuse taseme- ja jõudluse tase taastatud andmebaasi võib olla erinevas hetkeseisu reaalajas andmebaas. Kui olete lõpetanud, on taastatud andmebaasi maksta teenuste taseme- ja jõudluse taseme põhjal tavaline määrade tavaline täielikult juurdepääsetav online andmebaas. Teil tekkida kulude, kuni andmebaasi taastamine on lõpule viidud.

Andmebaasi taastada üldiselt earler punkti taastamise eesmärgil. Seda tehes saate kohelge taastatud andmebaasi asendusena algse andmebaasi või selle abil saate tuua andmeid ja seejärel värskendage algse andmebaasi. 

- ***Andmebaasi asendamine:*** Kui taastatud andmebaasi on mõeldud asendusena algse andmebaasi, kontrollige tulemuslikkuse ja/või teenuse kiht sobivad ja vajadusel andmebaasi skaala. Saate algse andmebaasi ümber nimetada ja seejärel andke taastatud andmebaasi T-SQL-i käsuga Muuda andmebaasi algse nimega. 
- ***Andmete taastamine:*** Kui plaanite andmete toomiseks taastatud andmebaasi taastamine kasutaja või rakendus tõrge, pead eraldi kirjutamine ja mis tahes andmete taastamine skriptide peate andmete eraldamiseks taastatud andmebaasi algse andmebaasi käivitada. Kuigi taastetoimingu võib võtta kaua aega, andmebaasi taastamine andmebaasi loendis kogu nähtav. Kui kustutate andmebaasi taastamine ajal, see toimingust ja te ei lisandu andmebaas, mida ei viida taastamine. 

Üksikasjalikku teavet kasutades punkti õigel ajal taastamine kasutaja ja rakenduse tõrked taastada, vaadake [punkti /-kellaaja taastamine](sql-database-recovery-using-backups.md#point-in-time-restore)

## <a name="deleted-database-restore"></a>Kustutatud andmebaasi taastamine

Kustutatud andmebaasi taastamine saate taastada kustutatud andmebaasi kustutamise aeg kustutatud andmebaasi sama loogiline serveri [SQL-andmebaasi automaatse varundamise funktsiooni](sql-database-automated-backups.md)abil. 

> [AZURE.IMPORTANT] Kui kustutate mõne Azure'i SQL-andmebaasi serveri eksemplar, kõik oma andmebaase kustutatakse ka ja ei saa taastada. Ei toetata taastamiseks server kustutatud sel ajal.

Saate kasutada sama või taastatud andmebaasi uue andmebaasi nimi. Saate kasutada [Azure portaali](sql-database-restore-deleted-database-portal.md), [PowerShelli](sql-database-restore-deleted-database-powershell.md) või [ülejäänud (createMode = Taasta)](https://msdn.microsoft.com/library/azure/mt163685.aspx). 

> [AZURE.SELECTOR]
- [Andmebaasi taastamine kustutatud: Azure'i portaal](sql-database-restore-deleted-database-portal.md)
- [Andmebaasi taastamine kustutatud: PowerShelli](sql-database-restore-deleted-database-powershell.md)

## <a name="geo-restore"></a>Geo-taastamine

Geo-taastamine võimaldab teil iga server Azure'i piirkonnast SQL-andmebaasi taastamine Viimane geo kopeeritud [automatiseeritud päev varukoopia](sql-database-automated-backups.md). Geo-taastamine kasutab geograafilise liigne varukoopia lähtefaili ja seda saab kasutada andmebaasi taastada ka siis, kui andmebaas või andmekeskuse ei pääse juurde ka katkestuste tõttu. Saate kasutada [Azure portaali](sql-database-geo-restore-portal.md), [PowerShelli](sql-database-geo-restore-powershell.md)või [ülejäänud (createMode = taastamine)](https://msdn.microsoft.com/library/azure/mt163685.aspx) 

> [AZURE.SELECTOR]
- [Geo-taastamine: Azure'i portaal](sql-database-geo-restore-portal.md)
- [Geo-taastamine: PowerShelli](sql-database-geo-restore-powershell.md)

Geo-taastamine on vaikesuvand taastamine, kui teie andmebaas on saadaval ala, kus andmebaasi majutatakse juhtum. Kui suuremahuliste juhtum piirkond tulemuseks andnud rakenduse andmebaasi, saate Geo-taastamine serveris muude piirkonnas Viimane varukoopia andmebaasi taastamine. Kõik varukoopiate on geo kopeeritud ja võib-olla vahel, kui varundus on võetud ja geo kopeeritud selle Azure'i bloobimälu teises regioonis. See viivitus võib olla kuni tund nii õnnetuste võib olla kuni 1 tund andmete kadu, st RPO kuni 1 tund. Järgmisel joonisel on kujutatud andmebaasi taastamine varukoopia viimase päeva.

![Geo-taastamine](./media/sql-database-geo-restore/geo-restore-2.png)

Üksikasjalikku teavet kasutades Geo-taastamine on katkestuste taastada, leiate teemast [on katkestuste taastamine](sql-database-disaster-recovery.md)

> [AZURE.IMPORTANT] Kuigi Geo-taastamine on saadaval kõigi teenuse astme, on põhilised katastroofi taastamine lahendused saadaval SQL-andmebaasis pikima RPO ja hinnangu taastamise aeg (ERT). Tavaline andmebaaside maksimaalse suurusega 2 GB Geo-taastamine pakub DR lahenduse koos mõne ERT 12 tundi. Suurem Standard või Premium andmebaaside jaoks, kui oluliselt lühem taastamine on soovitud või vähendada andmekao tõenäosuse võiksite kasutada aktiivse Geo-kopeerimine. Aktiivse Geo-kopeerimine pakub palju väiksem RPO ja ERT, mis nõuab ainult te pidevalt tiražeeritud teisese Tõrkesiirde algatada. Lisateavet leiate teemast [Aktiivse Geo-kopeerimine](sql-database-geo-replication-overview.md).

## <a name="programmatically-performing-recovery-using-automated-backups"></a>Programmiliselt läbimiseks taastamine automaatse varundamise funktsiooni abil

Nagu eespool addiition Azure portaali andmebaasi taastamine saab teha programmically kasutades Azure PowerShelli ja REST API-ga. Järgmistes tabelites on kirjeldatud käskude saadaval.

### <a name="powershell"></a>PowerShelli

|Cmdlet-käsk|Kirjeldus|
|------|-----------|
|[Get-AzureRmSqlDatabase](https://msdn.microsoft.com/en-us/library/azure/mt603648.aspx)|Saab üks või mitu andmebaasid.|
|[Get-AzureRMSqlDeletedDatabaseBackup](https://msdn.microsoft.com/en-us/library/azure/mt693387.aspx)|Saate taastada kustutatud andmebaasi saab.|
|[Get-AzureRmSqlDatabaseGeoBackup](https://msdn.microsoft.com/library/azure/mt693388.aspx)|Saab geograafilise liigne andmebaasi varukoopia.|
|[Taastamine-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt693390.aspx)|Taastab SQL-andmebaasi.|
||||

### <a name="rest-api"></a>REST API-GA

|API-GA|Kirjeldus|
|---|-----------|
|[ÜLEJÄÄNUD (createMode = taastamine)](https://msdn.microsoft.com/library/azure/mt163685.aspx)|Taastab andmebaasi|
|[Saada loomine või andmebaasi oleku värskendamine](https://msdn.microsoft.com/library/azure/mt643934.aspx)|Annab vastuseks oleku taastamine töötamise ajal|
||||



## <a name="summary"></a>Kokkuvõte

Automaatvarunduse kaitsta oma andmebaasi kasutaja ja rakenduse tõrked, juhusliku andmebaasi kustutamise ja pikaajaline katkestuste. See null-maksumus null-administraatori lahendus on saadaval kõigi SQL andmebaase. 

## <a name="next-steps"></a>Järgmised sammud

- Äri järjepidevuse ülevaade ja stsenaariumid, vaadake teemat [äri järjepidevuse ülevaade](sql-database-business-continuity.md)
- Azure'i SQL-andmebaasi automaatse varundamise funktsiooni kohta lisateabe saamiseks vt [SQL-andmebaasi automaatse varundamise funktsiooni](sql-database-automated-backups.md)
- Kiirem taaste suvandite kohta leiate teemast [Aktiivne-Geo-Dispersioonanalüüs](sql-database-geo-replication-overview.md)  
- Arhiivimine automaatse varundamise funktsiooni kasutamise kohta lisateabe saamiseks lugege artiklit [andmebaasi kopeerimine](sql-database-copy.md)
