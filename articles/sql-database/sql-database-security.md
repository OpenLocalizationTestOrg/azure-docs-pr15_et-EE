<properties
   pageTitle="SQL-i andmebaasi turvalisuse ülevaade"
   description="Turvalisusest Azure SQL-andmebaas ja SQL serveri, sh pilveteenuses erinevused ja SQL serveri kohapealse autentimine, autoriseerimine, ühenduse turvalisus, krüptimise ja nõuetele vastavus kärpida."
   services="sql-database"
   documentationCenter=""
   authors="tmullaney"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management"
   ms.date="06/09/2016"
   ms.author="thmullan;jackr"/>


# <a name="securing-your-sql-database"></a>SQL-andmebaasi turvamine

## <a name="overview"></a>Ülevaade

Selles artiklis tutvustatakse põhitõdesid turvaliseks kasutav Azure'i SQL-andmebaasi andmete astme. Eelkõige artiklit aitavad teil alustada ressurssidega eest kaitsta andmeid, juurdepääsu piiramise ja jälgida tegevuste andmebaasi loodud [SQL-andmebaasi õpetuse alustamine](sql-database-get-started.md). Täieliku ülevaate turbefunktsioonid saadaval kõik maitsed SQL-i, vt [Turbekeskus andmebaasimootor SQL Server ja Azure SQL-andmebaas](https://msdn.microsoft.com/library/bb510589). Lisateave pakutakse ka [turbe- ja Azure'i SQL-andmebaasi tehnilise lühiülevaade](https://download.microsoft.com/download/A/C/3/AC305059-2B3F-4B08-9952-34CDCA8115A9/Security_and_Azure_SQL_Database_White_paper.pdf) (PDF).

## <a name="connection-security"></a>Ühenduse turvalisus

Ühenduse turvalisus viitab kuidas saate piirata ja turvalist ühendust andmebaasi tulemüüri reeglid ja ühenduse krüptimise abil.

Tulemüüri reeglid kasutatakse ühenduse katsete IP-aadressid, mis ei ole konkreetselt whitelisted hüljata nii server ja andmebaas. Uue andmebaasi ühenduse loomise katse rakenduse või kliendi seadme avaliku IP-aadressi lubamiseks peate esmalt looma serveritaseme tulemüüri reegli Azure klassikaline portaali, REST API-ga või PowerShelli abil. Hea tava, tuleks piirata võimalikult palju teie serveri tulemüüri kaudu lubatud IP-aadresside vahemikud. Lisateavet leiate teemast [Azure SQL-i andmebaasi tulemüüri](https://msdn.microsoft.com/library/ee621782).

Kõigi ühenduste Azure'i SQL-andmebaasi nõuab krüptimine (SSL/TLS) juures alati andmed on "teel" ning andmebaasist. Rakenduse ühendusstring, tuleb teil määrata parameetrid ühenduse krüptimiseks ja *pole* usaldusväärsuse Serveri sert (see on tehtud oma ühendusstring Azure'i klassikaline portaalist kopeerimisel), muidu ühendus pole identiteedi server ja võib mõjutada "mees-in-the-Lähis" eest. ADO.net-i draiveri, näiteks järgmisi ühenduse päringustringi parameetrite on **Krüpti = True** ja **TrustServerCertificate = False**. Lisateavet leiate teemast [Azure SQL-i andmebaasi ühendus krüptimine ja serdi valideerimine](https://msdn.microsoft.com/library/azure/ff394108#encryption).


## <a name="authentication"></a>Autentimine

Kuidas tuvastada teie isikut andmebaasiga ühenduse loomisel viitab autentimist. SQL-andmebaasi toetab kahte tüüpi autentimist.

 - **SQL-i autentimise**, mis kasutab kasutajanime ja parooli
 - **Azure Active Directory autentimine**, mis kasutab Azure Active Directory abil hallatavad identiteedid ja on toetatud hallatavate ja integreeritud domeenide jaoks

Kui lõite loogiline serveri andmebaasi, määratud "serveri administraator" login kasutajanime ja parooliga. Nende mandaadi abil saate autentida mis tahes andmebaasi selles serveris andmebaasi omanik või "dbo." Kui soovite kasutada Azure Active Directory autentimine, peate looma teise serveri administraator nimega "Azure AD admin", millel on lubatud Azure AD kasutajate ja rühmade haldamine. Selle administraator saab teha ka kõik toimingud, mis tavalise serveri administraator saab. Teemast [ühenduse loomine SQL-i andmebaasi, kasutades Azure Active Directory autentimise](sql-database-aad-authentication.md) lühiülevaade loomise lubamiseks Azure Active Directory autentimise Azure AD administraator.

Hea tava rakenduse tuleks kasutada mõne muu kontoga autentida – nii saate piirata rakenduse õiguste ja riske pahatahtlik tegevus juhuks, kui rakenduse koodis on juurdepääs SQL süst rünnak. Soovitatav on luua on [esitatud andmebaasi kasutaja](https://msdn.microsoft.com/library/ff929188), mis võimaldab rakenduse autentida ühe andmebaasi. Saate luua, käitades järgmine käsk T-SQL serveri administraator login andmebaasi kasutaja ühendatud SQL-i autentimist kasutava keskkonnas andmebaasi kasutaja.

```
CREATE USER ApplicationUser WITH PASSWORD = 'strong_password'; -- SQL Authentication
```

Kui olete loonud Azure AD administraator, saate luua keskkonnas andmebaasi kasutaja, täites järgmine käsk T-SQL Azure AD administraatorina andmebaasi kasutaja ühendatud Windows Azure Active Directory autentimist kasutava:

```
CREATE USER [Azure_AD_principal_name | Azure_AD_group_display_name] FROM EXTERNAL PROVIDER; -- Azure Active Directory Authentication
```

Mõlemal juhul tuleks rakenduse ühendusstringi määrata need kasutajatunnust, mitte serveri administraator login, andmebaasiga ühenduse loomiseks.

SQL-andmebaasi autentimine kohta leiate lisateavet teemast [andmebaaside haldamine ja sisselogimise Azure SQL-andmebaasis](sql-database-manage-logins.md).


## <a name="authorization"></a>Luba
Luba viitab võimalikud Azure SQL andmebaasis ja see kontrollib oma kasutajakonto rolli rühmakuuluvus ja õigused. Hea tava, tuleks anda kasutajatele vähemalt vajalikke õigusi. Azure'i SQL-andmebaasi teeb see lihtne hallata rollide T-SQL-i abil.

```
ALTER ROLE db_datareader ADD MEMBER ApplicationUser; -- allows ApplicationUser to read data
ALTER ROLE db_datawriter ADD MEMBER ApplicationUser; -- allows ApplicationUser to write data
```

Serveri administraator konto, millega loote kuulub db_owner, mis on asutuse sees andmebaasi midagi tegema. Saate salvestada selle konto kasutamise skeemi uuendamine ja muude toimingute kohta. Piiratud õigustega kontoga "ApplicationUser" vähimate õiguste, rakenduse jaoks vajalik andmebaasi rakenduse kaudu suhtlemiseks.

On võimalust täpsemaks piiritlemiseks, mida kasutaja saab teha Azure'i SQL-andmebaasi.

* [Andmebaasi rollid](https://msdn.microsoft.com/library/ms189121) peale db_datareader ja db_datawriter saab luua võimsamaid rakenduse Kasutajakontod või vähem võimas halduse kontod.
* Varundustöö [õiguste](https://msdn.microsoft.com/library/ms191291) abil saate määrata, milliseid toiminguid saate teha üksikute veergude, tabelite, vaadete, toiminguid ja muude objektide andmebaasi.
* [Isikustamine](https://msdn.microsoft.com/library/vstudio/bb669087) ja [mooduli sisselogimise](https://msdn.microsoft.com/library/bb669102) saab turvaliselt ajutiselt tõsta õigused.
* [Rea-turvalisuse](https://msdn.microsoft.com/library/dn765131) saab kasutada piirang, mis ridade kasutaja on juurdepääs.
* [Andmete Masking](sql-database-dynamic-data-masking-get-started.md) saab kasutada andmete piirata.
* [Salvestatud toimingute](https://msdn.microsoft.com/library/ms190782) saab piirata toiminguid, mida tuleks andmebaasi.

Azure'i klassikaline portaalist andmebaasid ja loogiline serverid haldamine või kasutades Azure ressursihaldur API kontrollib portaali kasutaja konto rollimääranguid. Selle teema kohta leiate lisateavet teemast [Rollipõhine juurdepääsu reguleerimine Azure'i portaalis](../active-directory./role-based-access-control-configure.md).


## <a name="encryption"></a>Krüptimine

SQL Azure'i andmebaas aitab kaitsta teie andmeid, krüptides andmete, kui see on "ülejäänud" või salvestatud andmebaasifailide ja varukoopiate, [Läbipaistev andmete krüptimise](http://go.microsoft.com/fwlink/?LinkId=526242)abil. Andmebaasi krüptimiseks andmebaasi omanik ühendust luua ja käivitada.

```
ALTER DATABASE [AdventureWorks] SET ENCRYPTION ON;
```

Muud võimalused oma andmete saladusi krüptimiseks, on soovitatav.

* [Lahtri taseme krüptimise](https://msdn.microsoft.com/library/ms179331.aspx) teatud veerud või isegi lahtrite andmete krüptimiseks erinevate krüptimise võtmed.
* Kui teil on vaja riistvara turvalisus mooduli või keskne haldamine oma krüptimise võtme hierarhia, kaaluge [Azure'i klahvi Vault SQL Server Azure'i VM-i abil](http://blogs.technet.com/b/kv/archive/2015/01/12/using-the-key-vault-for-sql-server-encryption.aspx).
* [Alati krüptitud](https://msdn.microsoft.com/library/mt163865.aspx) (eelvaade) teeb krüptimise läbipaistvaid rakendustele ja võimaldab tundliku loomuga andmete klientrakendustes ilma krüptimise võtmed ühiskasutusse SQL-andmebaasi krüptimiseks kliendid.

## <a name="auditing"></a>Auditi

Auditi ja andmebaasi sündmuste jälgimine aitab teil säilitada nõuetele vastavuse ja tuvastada kahtlase tegevuse. SQL-andmebaasi auditeerimine võimaldab teil oma andmebaasi auditit kirje sündmuste logi sisse Azure Storage konto. Microsoft Power BI hõlbustamiseks süvitsimineku aruannete ja analüüside ka SQL-andmebaasi auditeerimine integreerub. Lisateabe saamiseks lugege teemat [Alustamine SQL-andmebaasi audit](sql-database-auditing-get-started.md).

SQL-i andmebaasi ohtude tuvastamise pakub väärtpaberi Valemiauditi peal. See võimaldab ohtude vastata, andes Turbeteatiste Anomaalne tegevuste esinevad. Lisateavet leiate teemast [Alustamine SQL-i andmebaasi ohtude tuvastamise](sql-database-threat-detection-get-started.md).  

## <a name="compliance"></a>Nõuetele vastavus

Lisaks ülaltoodud funktsioone ja funktsioonid aitavad vastavad eri turvalisuse nõuetele, Azure'i SQL-andmebaasi ka rakenduse osaleb regulaarselt auditite ja on sertifitseeritud vastavusstandardeid mitme vastu. Lisateabe saamiseks lugege teemat [Microsoft Azure'i usalduskeskuses](https://azure.microsoft.com/support/trust-center/), kus leiate uusimad loendit [SQL-andmebaasi vastavuse kinnitamine](https://azure.microsoft.com/support/trust-center/services/).
