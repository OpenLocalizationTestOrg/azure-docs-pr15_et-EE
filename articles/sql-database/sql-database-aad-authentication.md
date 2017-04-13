<properties
   pageTitle="Ühenduse SQL-andmebaasi või SQL-i andmebaas Azure Active Directory autentimist kasutades | Microsoft Azure'i"
   description="Saate teada, kuidas SQL-andmebaasiga ühenduse loomine Windows Azure Active Directory autentimist kasutades."
   services="sql-database"
   documentationCenter=""
   authors="BYHAM"
   manager="jhubbard"
   editor=""
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/16/2016"
   ms.author="rick.byham@microsoft.com"/>

# <a name="connecting-to-sql-database-or-sql-data-warehouse-by-using-azure-active-directory-authentication"></a>Ühenduse loomine SQL-andmebaasi või SQL-i andmebaas Azure Active Directory autentimist kasutades

Azure Active Directory autentimine on ühenduse loomine Microsoft Azure'i SQL-andmebaasi ja [SQL-i andmebaas](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) , kasutades identiteedid Azure Active Directory (Azure AD). Azure Active Directory autentimine, saate ühes keskses kohas hallata andmebaasi kasutajate identiteet ja muude Microsofti teenuste ühes keskses kohas. Keskse ID haldamise ühe koha, andmebaasi kasutajate haldamine ja lihtsustab õiguste haldust. Eelised on järgmised:

- SQL serveri autentimine alternatiiv pakub.
- Aitab peatada kasutaja identiteedid leviku andmebaasi serverites.
- Lubab parooli pööre ühes kohas
- Kliente saate hallata andmebaasi õiguste abil välise (AAD) rühmad.
- Saate tühistada salvestamist paroole, võimaldades integreeritud Windowsi autentimist ja muud liiki autentimine, ei toeta Azure Active Directory.
- Azure Active Directory autentimist kasutava sisalduvad andmebaasi kasutajate identiteetide andmebaasi tasemel autentida.
- Azure Active Directory toetab luba autentimise rakenduste SQL-andmebaasiga ühenduse loomine.
- Azure Active Directory autentimine toetab kohalik Azure Active Directory domeeni sünkroonimise ilma ADFS-i (domain federation) või kohalikke kasutaja/Paroolautentimine.  
- Azure Active Directory toetab SQL Server Management Studio ühendusi, mis kasutavad Active Directory ühes kohas autentimist, mis sisaldab mitme teguriga autentimine (MFA).  MFA sisaldab mitmesuguseid võimalusi lihtne kontrollimise tugev autentimine – telefonikõne, tekstsõnumi, smart kaardid PIN-koodi või mobiilirakenduses teatis. Lisateabe saamiseks lugege teemat [SSMS kasutajatugi Azure AD MFA SQL-andmebaas ja SQL-i andmebaas](sql-database-ssms-mfa-authentication.md).

Funktsiooni konfigureerimistoimingute kaasata konfigureerimine ja kasutamine Azure Active Directory autentimine järgmistest toimingutest.

1. Luua ja määrata Azure Active Directory.
2. Veenduge, et teie andmebaas on Azure SQL-i andmebaasi V12. (Pole vajalik SQL-i andmebaas.)
3. Valikuline: Seostada või mis on seostatud tellimuse Azure active directory muuta.
4. Azure Active Directory administraator loomine Azure SQL serveri või [SQL Azure'i andmebaas](https://azure.microsoft.com/services/sql-data-warehouse/).
5. Konfigureerige oma klientarvutite.
6. Andmebaasi Azure AD identiteedid vastendatud keskkonnas andmebaasi kasutajate loomiseks.
7. Ühenduse loomine oma andmebaasi Azure AD identiteedid abil.


## <a name="trust-architecture"></a>Usalda arhitektuur

Järgmine üksikasjalik Diagramm kirjeldab lahenduse arhitektuur Azure AD autentimist kasutades Azure SQL-andmebaas. SQL-i andmebaas rakendada sama põhimõtet. Azure Active Directory kohalikke kasutaja parooli toetamiseks käsitletakse pilveteenuse osa ja Azure AD/Azure'i SQL-andmebaasi. Mikroneesia autentimine (või kasutaja ja parooliga Windowsi identimisteabe) suhtlemine ADFS-i plokk on nõutav. Nooled viitavad sellele suhtlusvahendite kaudu.

![AAD auth skeem][1]

Järgmine diagramm näitab federation, usalda ja majutusteenuse seosed, mis võimaldavad kliendi ühenduse loomiseks andmebaasi, esitades märgiks. Luba autendib Azure AD ja andmebaas on usaldusväärne. 1 klient võib tähistada Azure Active Directory kohalikke kasutajatega või Azure Active Directory ühendatud kasutajatega. Kliendi 2 tähistab võimalik lahendus, sh imporditud kasutajate; selles näites on pärit ühendatud Azure Active Directory ADFS-i Azure Active Directory sünkroonimise. On oluline, et aru saada, kas Accessi andmebaasi Azure AD autentimist kasutades eeldab, et majutusteenuse tellimus on seostatud Azure Active Directory. SQL Server Azure'i SQL-andmebaasi või SQL-i andmebaas hosting loomiseks tuleb kasutada sama tellimus.

![tellimuse seos][2]

## <a name="administrator-structure"></a>Administraatori struktuur

Kui Azure AD autentimist kasutades on kaks administraatorikontode SQL-andmebaasi server. Algne SQL serveri administraator ja Azure AD administraator. SQL-i andmebaas rakendada sama põhimõtet. Ainult administraator Azure AD kontol põhinev saate luua esimese Azure AD sisalduvad andmebaasi kasutaja kasutaja andmebaasi. Login Azure AD administraator saab Azure AD kasutaja või Azure AD rühma. Kui administraator on jaotises konto, saate seda kasutada iga rühma liige lubada mitme Azure AD administraatorite SQL serveri eksemplar. Rühma kontoga administraatorina suurendab hallatavust võimaldab ühes keskses kohas lisada ja eemaldada rühma liikmeid Azure AD kasutajad ja õigused SQL-andmebaasis muutmata. Igal ajal saab konfigureerida ainult üks Azure AD administraator (kasutaja või rühma).

![administraator struktuur][3]

## <a name="permissions"></a>Õigused

Luua uusi kasutajaid, peab teil olema selle `ALTER ANY USER` õiguse andmebaasis. Funktsiooni `ALTER ANY USER` andmebaasi kasutaja saab anda õiguse. Funktsiooni `ALTER ANY USER` õigus vastutab serveri administraator kontode ja andmebaasi kasutajad, kellel on `CONTROL ON DATABASE` või `ALTER ON DATABASE` õigusi selle andmebaasi ja liikmete soovitud `db_owner` andmebaasi rolli.

Azure'i SQL-andmebaasi või SQL-i andmebaas keskkonnas andmebaasi kasutaja loomiseks tuleb ühendada kasutades Azure AD identiteedi andmebaasist. Esimene keskkonnas andmebaasi kasutaja loomiseks peab ühenduse andmebaasi Azure Active Directory administraator (on andmebaasi omanik) abil. See on juhiseid 4 ja 5 allpool näidatud. Mis tahes Azure Active Directory autentimine on võimalik ainult juhul Azure Active Directory administraator on loodud Azure'i SQL-andmebaasi või serveri SQL-i andmebaas. Kui Azure Active Directory administraator on eemaldatud serverist, olemasolevate Azure Active Directory kasutajate loodud varem SQL serveri sees saab enam andmebaasiga on ühenduse Azure Active Directory mandaadi abil.

## <a name="azure-ad-features-and-limitations"></a>Azure'i AD funktsioonid ja piirangud

Azure Active Directory järgmised liikmed saavad ette valmistatud Azure SQL serveri või SQL-i andmebaas:

- Kohalikke liikmed: liikme loodud Azure AD hallatava domeeni või kliendi domeeni. Lisateavet leiate teemast [lisamine oma domeeni nimega Azure AD](../active-directory/active-directory-add-domain.md).

- Ühendatud domeeni liikmete: liikme loodud Azure AD ühendatud domeenis. Lisateavet leiate teemast [Microsoft Azure'i nüüd toetab Windows Server Active Directory federation](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/).

- Imporditud liikmed muude Azure Active kataloogid, kes on kohalikke või ühendatud domeeni liikmed.

- Active Directory rühmad, turberühmad loodud.

Microsofti kontod (nt outlook.com, hotmail.com, live.com) või muude külaliskontod (nt gmail.com, yahoo.com) ei toetata. Kui saate [https://login.live.com](https://login.live.com) konto ja parooliga logida, siis kasutate Microsofti kontoga, mida ei toetata Azure AD autentimise Azure'i SQL-andmebaasi või SQL Azure'i andmebaas.

### <a name="additional-considerations"></a>Täiendavad kaalutlused

- Täiustamiseks hallatavust, soovitame teil ettevalmistamise spetsiaalne Azure Active Directory rühma administraator.
- Ainult üks Azure AD administraator (kasutaja või rühma) saate konfigureerida Azure SQL serveri või SQL Azure'i andmebaas igal ajal.
- Ainult Azure Active Directory administraator SQL serveri korral saate luua ühenduse algselt Azure SQL serveri või Azure SQL-i andmebaas Azure Active Directory kontot kasutades. Active Directory administraator saab konfigureerida järgmised Azure Active Directory andmebaasi kasutajad.
- Soovitame ühenduse ajalõpp kuni 30 sekundit.
- SQL Server 2016 Management Studio ja SQL Server Data Tools for Visual Studio 2015 (versioon 14.0.60311.1April 2016 või uuem versioon) tugi Azure Active Directory autentimine. (Azure Active Directory autentimine on toetatud **.NET raamistiku andmepakkuja SqlServer**; veebisaidil vähemalt versiooni .NET Frameworki 4.6). Seetõttu nende tööriistade ja andmete taseme rakendused (kui ka .bacpac) uusima versiooni saate kasutada Azure Active Directory autentimine.
- [ODBC versioon 13.1](https://www.microsoft.com/download/details.aspx?id=53339) toetab Azure Active Directory autentimine, kuid `bcp.exe` ei saa ühendust luua, Azure Active Directory autentimist kasutades, kuna see mõne vanema pakkujat ODBC kasutab.
- `sqlcmd`toetab Azure Active Directory autentimine alguses versiooniga 13.1 saadaval [Allalaadimiskeskuse](http://go.microsoft.com/fwlink/?LinkID=825643)kaudu.  
- SQL Server Data Tools for Visual Studio 2015 jaoks on vaja vähemalt versiooni 2016 aprilli Data Tools (versioon 14.0.60311.1). Praegu ei kuvata SSDT objekti Exploreri Azure Active Directory kasutajad. Lahendusena saate vaadata nende kasutajate [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx).
- [Microsoft JDBC draiveri 6.0 SQL Server](https://www.microsoft.com/en-us/download/details.aspx?id=11774) toetab Azure Active Directory autentimine. Vaadake [säte ühenduse atribuudid](https://msdn.microsoft.com/library/ms378988.aspx).
- PolyBase ei saa autentida Azure Active Directory autentimise abil.
- Mõned tööriistad nagu BI ja Excel ei toetata.
- Azure Active Directory autentimine on toetatud SQL-andmebaasi Azure portaali **Andmebaasi importimine** ja **Eksportimine andmebaasi** labad järgi. Importimise ja eksportimise Azure Active Directory autentimist kasutades toetatakse ka PowerShelli käsu kaudu.


## <a name="1-create-and-populate-an-azure-ad"></a>1. luua ja määrata Azure AD

Looge Azure Active Directory ja asustada kasutajad ja rühmad. Azure Active Directory saab kasutada algset domeeni Azure AD hallatava domeeni. Azure Active Directory võib olla ka mõni kohapealse Active Directory domeeniteenused mis on ühendatud Azure Active Directory.

Lisateabe saamiseks vt [integreerimine Azure Active Directory oma kohapealse identiteedid](../active-directory/active-directory-aadconnect.md), [lisada oma domeeninime Azure AD](../active-directory/active-directory-add-domain.md), [toetab nüüd Microsoft Azure'i Windows Server Active Directory federation](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/), [haldamise Azure AD kataloogi](https://msdn.microsoft.com/library/azure/hh967611.aspx)ja [haldamine Windows PowerShelli kaudu Azure AD](https://msdn.microsoft.com/library/azure/jj151815.aspx).

## <a name="2-ensure-your-sql-database-is-version-12"></a>2 tagada SQL-andmebaasi on 12

Azure Active Directory autentimine on toetatud uusim SQL-i andmebaasi V12. Täpsemat teavet SQL-i andmebaasi V12 ja saate teada, kas see on teie regioonis saadaval, [mis on uut uusim SQL-i andmebaasi Update V12](sql-database-v12-whats-new.md). See toiming ei ole tarvis SQL Azure'i andmebaas, kuna SQL-i andmebaas on saadaval v12 ainult.

Kui teil on olemasoleva andmebaasiga, veenduge, et see on majutatud SQL-i andmebaasi v12 järgi (nt abil SQL Server Management Studio) andmebaasiga ühenduse loomisel ja käivitamisel `SELECT @@VERSION;`. SQL-i andmebaasi V12 andmebaasi oodatud väljund on vähemalt **Microsoft SQL Azure'i (RTM) - 12,0**. Kui andmebaas pole majutab SQL-i andmebaasi v12, lugege teemat [leping ning valmistada ette võtta kasutusele SQL-i andmebaasi V12](sql-database-v12-plan-prepare-upgrade.md), ja seejärel külastage Azure'i SQL-i andmebaasi V12 andmebaasi migreerimine klassikaline portaal.

Teise võimalusena saate luua uue andmebaasi SQL-i andmebaasi v12 loetletud [esimese Azure SQL-i andmebaasi loomine](sql-database-get-started.md)juhiste järgi. **Näpunäide**: lugeda järgmise juhise juurde, enne kui valite tellimuse uue andmebaasi jaoks.

## <a name="3-optional-associate-or-change-the-active-directory-that-is-currently-associated-with-your-azure-subscription"></a>3. Valikuline: Seostada või mis on seostatud tellimuse Azure active directory muutmine

Andmebaasi seostamiseks ettevõtte kataloogist Azure AD teha kataloogi usaldusväärsete directory Azure tellimuse majutusteenuse andmebaas. Lisateabe saamiseks lugege teemat [Kuidas Azure'i tellimused on seostatud Azure AD](https://msdn.microsoft.com/library/azure/dn629581.aspx).

**Täiendav teave:** Iga Azure'i tellimus on usaldusseos Azure AD eksemplariga. See tähendab, et ta loodab selle autentida kasutajate, teenuste ja seadmete directory. Mitu tellimust saab usaldada samas kaustas, kuid tellimuse loodab ainult ühe kausta. Saate vaadata kataloogis on usaldusväärne tellimuse veebisaidil [https://manage.windowsazure.com/](https://manage.windowsazure.com/)vahekaardil **sätted** . See usaldusseos, mis tellimus sisaldab kataloog on erinevalt suhe, et tellimus on kõik muud ressursid Azure (veebisaidid, andmebaasid ja jne), mis on nagu lapse ressursid tellimuse abil. Kui tellimus aegub, siis need muud ressursid tellimusega seostatud juurdepääsu ka lõpetab. Kuid kataloogi jääb Azure ja saate selle kausta teise tellimuse seostada ja jätkake directory kasutajate haldamine. Ressursside kohta leiate lisateavet teemast [Azure mõistmine ressursi juurdepääsu](https://msdn.microsoft.com/library/azure/dn584083.aspx).

Järgmistest toimingutest samm-sammult juhiste andmine seotud kataloogi antud tellimuse muutmise kohta.

1. Ühenduse loomine oma [Azure'i klassikaline portaalis](https://manage.windowsazure.com/) administraator Azure tellimuse abil.
2. Valige vasakul banner **sätted**.
3. Teie tellimused kuvatakse sätete kuva. Kui soovitud tellimuse ei kuvata, klõpsake ülaosas nuppu **tellimused** , **FILTER, DIRECTORY** rippmenüüst ja valige tellimuste sisaldava kausta ja seejärel klõpsake nuppu **Rakenda**.

    ![Valige tellimus][4]
4. Jaotises **sätted** klõpsake tellimuse ja seejärel klõpsake nuppu **Redigeeri DIRECTORY** lehe allosas.

    ![AD-sätted – portaal][5]
5. **DIRECTORY redigeerimine** väljale valige Azure Active Directory on seotud teie SQL serveri või SQL-i andmebaas ja seejärel klõpsake noolt edasi.

    ![Redigeeri-directory-valimine][6]
6. Dialoogiboksi **Kinnita** directory vastenduse kinnitada, et "**eemaldatakse kõik kaasadministraatorite.**"

    ![Redigeeri directory kinnitada][7]
7. Klõpsake nuppu kontrolli uuesti portaali.

> [AZURE.NOTE] Kui muudate kataloogi, juurdepääsu kõik kaasadministraatorite, Azure AD kasutajad ja rühmad, eemaldatakse directory tagatud ressursside kasutajate ja neil pole enam juurdepääs see tellimus või oma ressursse. Ainult, teenuse administraatorina saate konfigureerida Accessi põhisumma põhjal uus kaust. See muudatus võib võtta palju aega kajastuma kõik ressursid. Kaust, muutmist ka Azure AD administraator muudab SQL-andmebaas ja SQL-i andmebaas ja keelamiseks andmebaasi juurdepääsu mis tahes Azure AD olemasolevatele kasutajatele. Azure AD administraator peab olema reset (nagu allpool kirjeldatud) ja uue Azure AD kasutajad peavad olema loodud.

## <a name="4-create-an-azure-ad-administrator-for-azure-sql-server"></a>4. Azure SQL serveri administraator Azure AD loomine

Serveri administraator kontoga, mis on kogu Azure SQL serveri administraator käivitab iga Azure SQL serveri (mis majutab SQL-andmebaasi või SQL-i andmebaas). Teine SQL serveri administraator peab olema loodud, mis on Azure AD konto. See peamine luuakse keskkonnas andmebaasi kasutaja põhi andmebaasi. Administraatorid, kui server administraatorikontode on iga kasutaja andmebaasi rolli **db_owner** liikmed ja sisestage iga kasutaja andmebaasi **dbo** kasutajana. Serveri administraator kontode kohta leiate lisateavet teemast [andmebaaside haldamine ja Azure SQL-andmebaasis sisselogimise](sql-database-manage-logins.md) ja [Azure SQL-i andmebaasi turvalisuse juhised ja piirangud](sql-database-security-guidelines.md) **sisselogimise ja kasutajate** osas.

Azure Active Directory kasutamisel Geo-Dispersioonanalüüs Azure Active Directory administraator peab olema konfigureeritud nii esmast ja teisene serverid. Kui server ei ole Azure Active Directory administraator, siis Azure Active Directory sisselogimise ja kasutajad saavad on "ei saa ühendust" serveritõrge.

> [AZURE.NOTE] Kasutajad, mis põhinevad konto Azure AD (sh Azure SQL serveri administraatori konto), Azure'i AD-põhiste kasutajad ei saa luua, kuna nad ei saa kinnitada pakutud andmebaasi kasutajate Azure AD õiguste.

### <a name="provision-an-azure-active-directory-administrator-for-your-azure-sql-server-by-using-the-azure-portal"></a>Ettevalmistamise Azure Active Directory administraator oma Azure SQL Server Azure'i portaali abil

1. [Azure'i portaalis](https://portal.azure.com/)paremas ülanurgas nuppu ühenduse loomiseks ripploendis võimalike aktiivne kataloogid. Valige õige Active Directory Azure AD vaikeväärtuseks. Selles etapis tuleb lingid tellimuse seos Active Directory Azure SQL serveri kindlasti, et sama tellimuse kasutatakse nii Azure AD ja SQL serveris. (Azure SQL serveri saate majutada Azure'i SQL-andmebaasi või SQL Azure'i andmebaas.)

    ![Valige ad][8]
2. Vasakul banner valige **SQL-i serverid**, valige oma **SQL serveri**ja seejärel klõpsake **SQL serveri** labale ülaservas nuppu **sätted**.

    ![AD sätted][9]
3. Labale **sätted** nuppu ** Active Directory administraator.
4. **Active Directory administraator** tera, valige **Active Directory administraator**ja seejärel ülaosas, klõpsake käsku **Sea administraator**.
5. **Lisa administraator** labale otsimine kasutaja, valige selle kasutaja või rühma olema administraator ja **nuppu**. (Active Directory administraator tera kuvatakse kõigile liikmetele ja teie Active Directory rühmad. Kasutajate või rühmade, mis on hallid ei saa valida, kuna neid ei toetata Azure AD administraatorid kujul. (Vt toetatud administraatorid **Azure AD funktsioonid ja piirangud** ülaltoodud loendis). Rollipõhine juurdepääsu reguleerimine (RBAC) kehtib ainult portaali ja on SQL Server pole levitatud.
6. **Active Directory administraator** tera ülaosas nuppu **Salvesta**.
    ![Valige administraator][10]

    Administraator muutmise protsess võib kuluda mitu minutit. Seejärel uus administraator kuvatakse väljal **Active Directory administraator** .

> [AZURE.NOTE] Azure AD administraator häälestamisel uus administraatori nimi (kasutaja või rühma) veel ei saa virtuaalse juhtslaidi andmebaasis kasutajana SQL serveri autentimine. Kui see on olemas, ei õnnestu Azure AD administraator häälestamine; tagasipööramine selle loomise ja, mis näitab, et sellist administraator (nimi) juba olemas. Kuna selline SQL serveri autentimine kasutaja ei ole Azure AD osa, mis tahes peegeldav Azure AD autentimist kasutades serveriga ühendust nurjub.

Administraator labale **Active Directory administraator** ülaosas hiljem eemaldamiseks klõpsake nuppu **Eemalda administraator**ja klõpsake siis nuppu **Salvesta**.

### <a name="provision-an-azure-ad-administrator-for-azure-sql-server-by-using-powershell"></a>Ettevalmistamise Azure SQL serveri administraator Azure AD PowerShelli abil

PowerShelli cmdlettide käitamiseks peate olema installitud ja töötab Azure PowerShelli. Üksikasjalikku teavet, vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md).

Ettevalmistamise Azure AD administraator, käivitada Azure PowerShelli järgmised käsud:

- Lisage AzureRmAccount
- Valige-AzureRmSubscription


Kasutada ettevalmistamine ja hallata Azure AD administraator:

| Cmdlet-käsu nimi                                       | Kirjeldus                                                                                                     |
|---------------------------------------------------|-----------------------------------------------------------------------------------------------------------------|
| [Set-AzureRmSqlServerActiveDirectoryAdministrator](https://msdn.microsoft.com/library/azure/mt603544.aspx)    | Azure Active Directory administraator sätteid Azure SQL serveri või SQL Azure'i andmebaas. (Peab olema praeguselt tellimuselt.) |
| [Eemalda – AzureRmSqlServerActiveDirectoryAdministrator](https://msdn.microsoft.com/library/azure/mt619340.aspx) | Eemaldab Azure Active Directory administraator Azure SQL serveri või SQL Azure'i andmebaas. |
| [Get-AzureRmSqlServerActiveDirectoryAdministrator](https://msdn.microsoft.com/library/azure/mt603737.aspx)    | Annab vastuseks teabe kohta praegu konfigureeritud Azure SQL serveri või SQL Azure'i andmebaas Azure Active Directory administraator. |

Kasutage PowerShelli käsku get-abi näha rohkem üksikasju käskude, näiteks ``get-help Set-AzureRmSqlServerActiveDirectoryAdministrator``.

Skripti järgmises Azure AD administraator rühma nimega **DBA_Group** (objekti id `40b79501-b343-44ed-9ce7-da4c8cc7353f`) jaoks **demo_server** server ressursirühma nimega **rühma – 23**:

```
Set-AzureRmSqlServerActiveDirectoryAdministrator –ResourceGroupName "Group-23"
–ServerName "demo_server" -DisplayName "DBA_Group"
```

**DisplayName** sisendparameetrile nõustub kasutaja turvasubjektinimi või Azure AD kuvatav nimi. Näiteks ``DisplayName="John Smith"`` ja ``DisplayName="johns@contoso.com"``. Azure AD rühmade ainult Azure AD kuvatav nimi on toetatud.

> [AZURE.NOTE] Azure'i PowerShelli käsu ```Set-AzureRmSqlServerActiveDirectoryAdministrator``` ei takista teil ettevalmistamise Azure AD administraatorid toetamata kasutajate jaoks. Mittetoetatavad rakendust saate ette valmistatud, kuid ei saa ühendust andmebaasi. (Vt toetatud administraatorid **Azure AD funktsioonid ja piirangud** ülaltoodud loendis).

Järgmises näites kasutatakse valikuline **ObjectId väärtuse**:

```
Set-AzureRmSqlServerActiveDirectoryAdministrator –ResourceGroupName "Group-23"
–ServerName "demo_server" -DisplayName "DBA_Group" -ObjectId "40b79501-b343-44ed-9ce7-da4c8cc7353f"
```

> [AZURE.NOTE] Azure'i AD **ObjectId väärtuse** on nõutav, kui **DisplayName** pole kordumatu. **ObjectId väärtuse** ja **kuvatava** väärtused toomiseks kasutada klassikaline portaalis Azure Active Directory osa ja kasutaja või rühma atribuute.

Järgmine näide tagastab teabe praeguse Azure AD Azure SQL serveri administraator:

```
Get-AzureRmSqlServerActiveDirectoryAdministrator –ResourceGroupName "Group-23" –ServerName "demo_server" | Format-List
```

Järgmises näites eemaldab Azure AD administraator:
```
Remove-AzureRmSqlServerActiveDirectoryAdministrator -ResourceGroupName "Group-23" –ServerName "demo_server"
```

Azure Active Directory administraator saab ka REST API-de abil ette valmistada. Lisateabe saamiseks vt [teenuse haldus REST API viide ja Azure SQL-i andmebaasid toimingute Azure SQL-i andmebaaside jaoks toimingud](https://msdn.microsoft.com/library/azure/dn505719.aspx)

## <a name="5-configure-your-client-computers"></a>5. oma klientarvutite konfigureerimine

Klõpsake kõigi klientarvutite, kust teie või ühenduse Azure'i SQL-andmebaasi või Azure SQL-i andmebaas abil Azure AD identiteedid, tuleb teil installida järgmine tarkvara:

- .NET Frameworki 4.6 või uuem [https://msdn.microsoft.com/library/5a4x27ek.aspx](https://msdn.microsoft.com/library/5a4x27ek.aspx)kaudu.
- Azure Active Directory Authentication Library SQL serveri (**ADALSQL. DLL**) on saadaval mitmes keeles (x86 ja amd64) veebisaidil [Microsoft Active Directory Authentication Library Microsoft SQL Server](http://www.microsoft.com/download/details.aspx?id=48742)allalaadimiskeskuse kaudu.

### <a name="tools"></a>Tööriistad

- Kas [SQL Server 2016 Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) või [SQL Server Data Tools for Visual Studio 2015](https://msdn.microsoft.com/library/mt204009.aspx) installimise vastab .NET Frameworki 4.6.
- SSMS installe x86 versiooni **ADALSQL. DLL**.
- SSDT installib **ADALSQL amd64 versiooni. DLL**.
- Uusim Visual Studio [Visual Studio allalaadimine](https://www.visualstudio.com/downloads/download-visual-studio-vs) : vastab .NET Frameworki 4.6, kuid ei saa installida **ADALSQL nõutav amd64 versiooni. DLL**.

## <a name="6-create-contained-database-users-in-your-database-mapped-to-azure-ad-identities"></a>6. keskkonnas andmebaasi kasutajate loomiseks andmebaasi vastendatud Azure AD identiteedid

### <a name="about-contained-database-users"></a>Suletud andmebaasi kasutajate kohta

Azure Active Directory autentimine nõuab andmebaasi kasutajatel keskkonnas andmebaasi kasutajate luua. Suletud andmebaasi kasutaja identiteedi Azure AD põhjal on andmebaasi kasutaja, mis ei sisalda sisselogimist põhi andmebaasi ja mis kaardid identiteedi Azure AD kataloogis, mis on seotud andmebaasi. Azure AD identiteedi võib olla konto kasutaja või rühma. Keskkonnas andmebaasi kasutajate kohta leiate lisateavet teemast [Sisalduvad andmebaasi kasutajate tegemise teie andmebaasi Portable](https://msdn.microsoft.com/library/ff929188.aspx).

> [AZURE.NOTE] Andmebaasi kasutajate (välja arvatud administraatorid) ei saa luua portaalis. RBAC rollid paljundatakse pole SQL Server, SQL-andmebaasi või SQL-i andmebaas. Azure'i RBAC rollid kasutatakse Azure ressursse haldamiseks ja ei rakendata andmebaasi õigused. Näiteks **SQL serveri kaasautori** roll ei anna juurdepääsu ühenduse SQL-andmebaasi või SQL-i andmebaas. Luba juurdepääs peab olema antud otse andmebaasis Transact-SQL-lausete abil.

### <a name="connect-to-the-user-database-or-data-warehouse-by-using-sql-server-management-studio-or-sql-server-data-tools"></a>Kasutaja andmebaasi või andmete ladu ühenduse SQL Server Management Studio või SQL Server Data Tools abil

Kinnitage Azure AD administraator on õigesti seadistatud, kasutades Azure AD administraatorikontot **juhtslaidi** andmebaasiga ühenduse loomiseks.
Ettevalmistamise Azure'i AD-põhiste sisaldas (v.a serveri administraator andmebaasi omanik), andmebaasi kasutaja andmebaasiga ühenduse loomiseks identiteediga Azure AD, mis on juurdepääs andmebaasile.

> [AZURE.IMPORTANT] Azure Active Directory autentimine tugi on saadaval [SQL Server 2016 Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) ja [SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx) Visual Studio 2015. August 2016 vabastamist SSMS sisaldab ka Active Directory ühes kohas autentimist, mis saavad administraatorid nõudma abil telefonikõne, sõnumi, smart kaartide PIN-koodi või mobiilirakenduses teatise Mitmikautentimise tugi.

#### <a name="connect-using-active-directory-integrated-authentication"></a>Ühenduse loomine Active Directory integreeritud autentimist kasutades

Seda meetodit kasutada siis, kui olete logitud Windows Azure Active Directory mandaat kaudu ühendatud domeenis.

1. Käivitage Management Studio või Data Tools ja valige dialoogiboksi **ühenduse Server** (või **ühenduse andmebaasimootor**) väljale **autentimine** **Active Directory integreeritud autentimise**. Parooli on vaja või saate sisestada, kuna olemasoleva mandaat esitatakse ühenduse.
    ![Valige AD integreeritud autentimine][11]

2. Klõpsake nuppu **Suvandid** ja klõpsake lehel **Ühenduse atribuudid** **andmebaasiga ühenduse loomiseks** , tippige nimi kasutajate andmebaasi, mida soovite ühendada.
    ![Valige andmebaasi nimi][13]


#### <a name="connect-using-active-directory-password-authentication"></a>Ühenduse loomine Active Directory parooli autentimist kasutades

Kasutage seda meetodit ühendavad Azure AD turvasubjektinimi Azure AD abil hallatavad domeeni. Samuti saate seda kasutada ühendatud konto puudub juurdepääs domeeni, näiteks töötades kaugühenduse teel.

Kasutage seda meetodit, kui on Windowsi domeeni, mis on ühendatud Azure'i mandaadi abil sisse logitud, või kui Azure AD autentimist kasutades Azure AD kasutades algse või kliendi domeeni põhjal.

1. Käivitage Management Studio või Data Tools ja valige dialoogiboksi **ühenduse Server** (või **ühenduse andmebaasimootor**) väljale **autentimine** **Active Directory Paroolautentimine**.
2. Tippige väljale **kasutajanimi** oma kasutajanimi Azure Active Directory vormingus **username@domain.com**. See peab olema Azure Active Directory kontot või konto domeeni liita Azure Active Directory.
3. Tippige väljale **parool** oma konto Azure Active Directory kasutaja parool või domeeni kontoga ühendatud.
    ![Valige AD Paroolautentimine][12]

4. Klõpsake nuppu **Suvandid** ja klõpsake lehel **Ühenduse atribuudid** **andmebaasiga ühenduse loomiseks** , tippige nimi kasutajate andmebaasi, mida soovite ühendada. (Vt pilti Eelmisele suvandile.)


### <a name="create-an-azure-ad-contained-database-user-in-a-user-database"></a>Azure'i AD sisalduvad andmebaasi kasutaja kasutaja andmebaasi loomine

Azure'i AD-põhiste sisaldas (v.a serveri administraator andmebaasi omanik) andmebaasi kasutaja loomiseks andmebaasiga ühenduse loomiseks Azure AD identiteediga, kui kasutaja vähemalt **muuta kõik** kasutajaõiguse. Seejärel kasutage Transact-SQL-i järgmist süntaksit:

    CREATE USER <Azure_AD_principal_name>
    FROM EXTERNAL PROVIDER;


*Azure_AD_principal_name* saab kasutaja nime Azure AD kasutaja või Azure AD rühma kuvatav nimi.

**Näited:** Suletud andmebaasi loomiseks kasutaja tähistav Azure AD ühendatud või hallatavate domeeni kasutaja.

    CREATE USER [bob@contoso.com] FROM EXTERNAL PROVIDER;
    CREATE USER [alice@fabrikam.onmicrosoft.com] FROM EXTERNAL PROVIDER;

Suletud andmebaasi kasutaja tähistav Azure AD või ühendatud domeeni rühma loomiseks sisestage turberühma kuvatav nimi.

    CREATE USER [ICU Nurses] FROM EXTERNAL PROVIDER;

Rakendus, mis kasutab mõni Azure AD luba tähistav keskkonnas andmebaasi kasutaja loomiseks tehke järgmist.

    CREATE USER [appName] FROM EXTERNAL PROVIDER;

Keskkonnas andmebaasi kasutajate Azure Active Directory identiteedid põhjal loomise kohta leiate lisateavet teemast [Loomine kasutaja (Transact-SQL-i)](http://msdn.microsoft.com/library/ms173463.aspx).


> [AZURE.NOTE] Azure Active Directory administraator Azure SQL serveri eemaldamine takistab Azure AD kasutaja autentimise serveriga. Vajadusel kasutuskõlbmatuks Azure AD kasutajad käsitsi lähevad SQL-andmebaasi administraator.

Kui loote andmebaasi kasutaja, kasutaja saab **ühenduse loomine** õiguste ja saate luua ühenduse andmebaasi **avaliku** rolli liige. Esialgu kasutajale saadaval ainult õigused on mis tahes **avaliku** rolli õigused või mis tahes õiguste kõik Windowsi rühmad, et nad on liige. Kui olete ette Azure'i AD-põhiste sisalduvad andmebaasi kasutaja, saate anda kasutaja täiendavad õigused samamoodi nagu saate anda õiguse kasutaja tüüp. Tavaliselt andmebaasi rollidele õiguste andmine ja kasutajate lisamine rollid. Lisateabe saamiseks lugege [Andmebaasi mootor õiguse põhitõed](http://social.technet.microsoft.com/wiki/contents/articles/4433.database-engine-permission-basics.aspx). Teisiti SQL-andmebaasi rollid kohta leiate lisateavet teemast [andmebaaside haldamine ja sisselogimise Azure SQL-andmebaasis](sql-database-manage-logins.md).
Ühendatud domeenis kasutaja, mis on importida haldamine domeen, peate kasutama hallatava domeeni identiteeti.

> [AZURE.NOTE] Azure'i AD kasutajad on tähistatud andmebaasi metaandmete E (EXTERNAL_USER) ja rühmade tüüp X (EXTERNAL_GROUPS). Lisateavet leiate teemast [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx).


## <a name="7-connect-by-using-azure-ad-identities"></a>7. ühenduse Azure AD identiteedid abil

Azure Active Directory autentimine toetab abil Azure AD identiteedid andmebaasiga ühenduse loomisel järgmistest:

- Integreeritud Windowsi autentimist kasutades
- Mõni Azure AD turvasubjektinimi ja parooli abil
- Rakenduse Turbeloa autentimist kasutades

### <a name="71-connecting-using-integrated-windows-authentication"></a>7.1. Ühenduse loomine (Windows) integreeritud autentimist kasutades

Integreeritud Windowsi autentimise kasutamiseks peab teie domeeni Active Directory ühendatud Azure Active Directory. Kasutaja domeeni identimisteabe jaotises domeeni ühendatud arvutisse peate kasutama andmebaasiga ühenduse loomisel klientrakenduse (või teenuse).

Abil integreeritud autentimis- ja Azure AD identiteedi andmebaasi ühenduse autentimist märksõna andmebaasi ühendusstringi peab olema seatud Active Directory integreeritud. Järgmine kood näidis C# kasutab ADO .net-i.

    string ConnectionString =
    @"Data Source=n9lxnyuzhv.database.windows.net; Authentication=Active Directory Integrated; Initial Catalog=testdb;";
    SqlConnection conn = new SqlConnection(ConnectionString);
    conn.Open();

Teate, et ühenduse string märksõna ``Integrated Security=True`` Azure SQL-andmebaasiga ühenduse loomiseks ei toetata.
Pange tähele, et ODBC-ühendus tegemisel saate eemaldada tühikuid ja määra 'ActiveDirectoryIntegrated' autentimine.

### <a name="72-connecting-with-an-azure-ad-principal-name-and-a-password"></a>7.2. Ühenduse loomine mõne Azure AD nimi ja parool
Integreeritud autentimist ja Azure AD identiteedi abil andmebaasi ühenduse autentimist märksõna peate seadma Active Directory parool. Ühendusstringi peab sisaldama kasutaja ID ja UID ja parooli/PWD märksõnade ja väärtused. Järgmine kood näidis C# kasutab ADO .net-i.

    string ConnectionString =
      @"Data Source=n9lxnyuzhv.database.windows.net; Authentication=Active Directory Password; Initial Catalog=testdb;  UID=bob@contoso.onmicrosoft.com; PWD=MyPassWord!";
    SqlConnection conn = new SqlConnection(ConnectionString);
    conn.Open();

Lugege lisateavet Azure AD autentimismeetodite abil [Azure AD autentimise GitHub](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/security/azure-active-directory-auth)demo demo koodinäiteid saadaval.


### <a name="73-connecting-with-an-azure-ad-token"></a>Mõni Azure AD luba 7.3 ühendamine
See autentimise meetodit võimaldab Kesk-astme teenuste ühendamine Azure'i SQL-andmebaasi või SQL Azure'i andmebaas saamisega märgiks: Azure Active Directory (AAD). See võimaldab keerukaid stsenaariumid, sh sertifikaadi-põhine autentimine. Peate tegema neli põhietappi Azure AD Turbeloa autentimist.

1. Rakenduse registreerida Azure Active Directory ja kliendi id hankimine teie kood. 
2. Andmebaasi kasutaja tähistav rakenduse loomine. (Valmis eelnevalt kirjeldatud toiminguga 6).
3. Serdi loomine kliendi arvuti jookseb rakendus.
4. Serdi kuuluva rakenduse klahvi.

Valimi ühendusstringi:

```
string ConnectionString =@"Data Source=n9lxnyuzhv.database.windows.net; Initial Catalog=testdb;"
SqlConnection conn = new SqlConnection(ConnectionString);
connection.AccessToken = "Your JWT token"
conn.Open();
```

Lisateabe saamiseks vt [SQL serveri turvalisus ajaveebi](https://blogs.msdn.microsoft.com/sqlsecurity/2016/02/09/token-based-authentication-support-for-azure-sql-db-using-azure-ad-auth/).

### <a name="connecting-with-sqlcmd"></a>Sqlcmd ühendamine  
Järgmistest, ühendage versioonis 13.1 sqlcmd, mis on saadaval [Allalaadimiskeskuse](http://go.microsoft.com/fwlink/?LinkID=825643)kaudu.

```
sqlcmd -S Target_DB_or_DW.testsrv.database.windows.net  -G  
sqlcmd -S Target_DB_or_DW.testsrv.database.windows.net -U bob@contoso.com -P MyAADPassword -G -l 30
```

## <a name="see-also"></a>Vt ka

[Andmebaasid ja Azure SQL-andmebaasi sisselogimise haldamine](sql-database-manage-logins.md)

[Suletud andmebaasi kasutajad](https://msdn.microsoft.com/library/ff929071.aspx)

[LUUA kasutaja (Transact-SQL)](http://msdn.microsoft.com/library/ms173463.aspx)


<!--Image references-->

[1]: ./media/sql-database-aad-authentication/1aad-auth-diagram.png
[2]: ./media/sql-database-aad-authentication/2subscription-relationship.png
[3]: ./media/sql-database-aad-authentication/3admin-structure.png
[4]: ./media/sql-database-aad-authentication/4select-subscription.png
[5]: ./media/sql-database-aad-authentication/5ad-settings-portal.png
[6]: ./media/sql-database-aad-authentication/6edit-directory-select.png
[7]: ./media/sql-database-aad-authentication/7edit-directory-confirm.png
[8]: ./media/sql-database-aad-authentication/8choose-ad.png
[9]: ./media/sql-database-aad-authentication/9ad-settings.png
[10]: ./media/sql-database-aad-authentication/10choose-admin.png
[11]: ./media/sql-database-aad-authentication/11connect-using-int-auth.png
[12]: ./media/sql-database-aad-authentication/12connect-using-pw-auth.png
[13]: ./media/sql-database-aad-authentication/13connect-to-db.png

