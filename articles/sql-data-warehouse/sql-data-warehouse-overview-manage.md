<properties
   pageTitle="SQL Azure'i andmebaas andmebaaside haldamine | Microsoft Azure'i"
   description="Ülevaade SQL-i andmebaas andmebaaside haldamine. Sisaldab Haldusriistad, DWUs ja tõrkeotsingu päringu jõudluse, millega hea turbepoliitikate ja andmebaasi taastamine andmete rikkumist või piirkondliku katkestuste skaala-out jõudlust."
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
   ms.date="08/16/2016"
   ms.author="barbkess;sonyama;"/>

# <a name="manage-databases-in-azure-sql-data-warehouse"></a>SQL Azure'i andmebaas andmebaaside haldamine

SQL-i andmebaas automatiseerib mitme andmebaaside haldamine. Näiteks mastaapimiseks jõudluse ainult peate reguleerimine ja Arvuta ressursid õigel eest maksta ja laske SQL-i andmebaas kõik skaleerimist välja ja uuesti mastaapimist tööd teha. 

Kindlasti soovite jälgida oma töökoormus tuvastada teie vajadustele jõudlust ning samuti päringute tõrkeotsing. Samuti peate teha mõned turvalisus toiminguid kasutajate ja sisselogimise õiguste haldamiseks.

See ülevaade hõlmab järgmisi aspekte haldamise SQL-i andmebaas.

- Haldusriistad
- Arvuta skaala
- Paus ja elulookirjeldus
- Jõudluse head tavad
- Päringu jälgimine
- Turvalisus
- Varundus ja taaste

## <a name="management-tools"></a>Haldusriistad

Erinevate tööriistade abil saate SQL-i andmebaas andmebaaside haldamine. Kui haldate andmebaase, arendate tööriista eelistuste seadmine iga ülesande tüüp, mida peate tegema.

### <a name="azure-portal"></a>Azure'i portaal
[Azure'i portaalis][] on veebipõhine portaal, kus saate luua, värskendada, ja Kustuta andmebaasid ja jälgida andmebaasi ressursid. See tööriist on suurepärane, kui te alles alustate Azure, väheste andmete ladu andmebaaside haldamine või vaja kiiresti midagi.

Teemast Azure portaali alustada [loomine SQL-andmebaas (Azure'i portaal)][].

### <a name="sql-server-data-tools-in-visual-studio"></a>SQL Server Data Tools Visual Studio
[SQL Server Data Tools][] (SSDT) Visual Studio abil saab ühenduse luua, hallata ja arendada oma andmebaasi. Kui olete tuttav Visual Studio või muude integreeritud keskkonnas (interaktiivset) rakendus arendaja, proovige kasutada SSDT Visual Studio.

SSDT sisaldab SQL Server objekti Explorer, mis võimaldab teil visualiseerida, ühendamine ja käivitada skriptide vastu andmebaase SQL-i andmebaas. Kiiresti ühenduse SQL-i andmebaas, et te saate lihtsalt klõpsake nuppu **Ava Visual Studio** käsuriba kuvamisel andmebaasi üksikasjad Azure'i klassikaline portaalis.  

Alustamine SSDT Visual Studios, lugege teemat [Päringu Azure SQL-i andmebaas Visual Studio][].

### <a name="command-line-tools"></a>Käsurea tööriistad
Käsurea tööriistade on optimaalne automatiseerimiseks oma töökoormus.  PowerShelli ja sqlcmd on kaks suuri võimalusi oma protsesside automatiseerimiseks.  Soovitame nende tööriistade suure hulga loogilised serverid haldamine ja juurutamine ressursi muudatused tootmiskeskkonnas nagu vajalikud toimingud saate kirja panna ning seejärel automatiseeritud.

### <a name="dynamic-management-views"></a>Dünaamilise halduse vaadete 

DMVs on leivanumbriks haldamiseks SQL-i andmebaas. Peaaegu kõik andmed, mis toob esile portaalis tugineb DMVs. SQL-i andmete ladu DMVs loendi leiate artiklist [SQL-i andmebaas süsteemi vaated][].

Alustamiseks, vt [ühenduse loomine ja päringu sqlcmd][]ja [loomine andmebaasi (PowerShelli)][].

## <a name="scale-compute"></a>Arvuta skaala

Klõpsake SQL-i andmebaas, saate kiiresti skaala välja või tagasi suureneb või väheneb Arvuta ressursse CPU, mälu ja I/O läbilaskevõime jõudlus. Jõudluse mastaapida, teha pole vaja reguleerida andmete ladu ühikute (DWUs), mis eraldab andmebaasi SQL-i andmebaas. SQL-i andmebaas kiiresti muudatuse ja tegeleb kõikide aluseks muudatuste või riistvara.

Skaleerimist DWUs kohta leiate lisateavet teemast [skaala jõudlust][].

##  <a name="pause-and-resume"></a>Paus ja elulookirjeldus

Kulude salvestamiseks saate viige ja Arvuta ressursid tellitavate jätkata. Näiteks, kui te ei kasuta andmebaasi öösel ja nädalavahetustel, saate ajal peatada, ja jätkata selle päeva jooksul. Te ei lisandu DWUs, kui andmebaas on peatatud.

Lisateabe saamiseks vt [paus arvutada][]ja [elulookirjelduse arvutada][].

## <a name="performance-best-practices"></a>Jõudluse head tavad

Kui uus tehnoloogia alustamine avastamine nõuandeid ja näpunäiteid, mis töötavad parim kohe algusest saate säästa palju aega.  Siit leiate põhitõed kogu paljude meie teemade.

Palju kokkuvõtte kõige olulisemad kaalutlused oma töökoormus väljatöötamisel, leiate artiklist [Andmete ladu SQL-i põhitõed][].

## <a name="query-monitoring"></a>Päringu jälgimine

Mõnikord päring töötab liiga pikk, kuid te pole kindel, kuhu on süüdlane. SQL-i andmebaas on dünaamilise halduse vaadete (DMVs), mille abil saate aru saada, millist päringu võtab liiga kaua aega. 

Päringute leidmiseks vaadake jaotist [jälgida oma töökoormus DMVs abil][].

## <a name="security"></a>Turvalisus

Turvaline süsteemi säilitamiseks peab olema teatise ja mis tahes tüüpi volitamata juurdepääsu eest. Turvalisus süsteemi peab veenduge, et tulemüüri reeglid on olemas ainult lubatud IP-aadresside saab ühendada. See peab kasutaja mandaadi proper autentimine. Pärast seda, kui kasutaja on ühendatud andmebaasi, kasutaja ainult peaks olema õigused minimaalsete arvu toimingute tegemiseks. Kaitsta andmeid, saate kasutada krüptimine. Samuti on oluline, et auditeerimine ja jälitamine nii, et saate jälgida sündmusi, kui on kahtlasest tegevusest.

Lisateavet haldamise turvalisus, pea üle [turvalisuse ülevaade][].

## <a name="backup-and-restore"></a>Varundus ja taaste

Mis tahes tootmisandmebaasile oluline osa on usaldusväärne backps andmete. SQL-i andmebaas hoiab teie andmete turvalised automaatselt Varundage oma aktiivse andmebaasi regulaarse intervalliga. Nende varukoopiate võimaldavad stsenaariumid, kus olete rikutud andmete või kogemata lähevad oma andmeid või andmebaasi taastamine.  Andmete varukoopia ajakava säilituspoliitika ja andmebaasi taastamise kohta leiate [taastamine hetktõmmis][].

## <a name="next-steps"></a>Järgmised sammud
Kasutades hea andmebaasi kujunduse põhimõtted hõlbustab SQL-i andmebaas oma andmebaaside haldamine. Lisateavet, pea üle [arengu ülevaade][].

<!--Image references-->

<!--Article references-->
[SQL-andmebaas (Azure'i portaal) loomine]: sql-data-warehouse-get-started-provision.md
[Andmebaasi (PowerShelli) loomine]: sql-data-warehouse-get-started-provision-powershell
[connection]: sql-data-warehouse-develop-connections.md
[Päringu SQL Azure'i andmebaas Visual Studio]: sql-data-warehouse-query-visual-studio.md
[Ühenduse loomine ja päringu sqlcmd abil]: sql-data-warehouse-get-started-connect-sqlcmd.md
[Arengu ülevaade]: sql-data-warehouse-overview-develop.md
[Jälgida oma töökoormus DMVs abil]: sql-data-warehouse-manage-monitor.md
[Paus Arvuta]: sql-data-warehouse-manage-compute-overview.md#pause-compute-bk
[Hetktõmmis taastamine]: sql-data-warehouse-restore-database-overview.md
[Elulookirjelduse Arvuta]: sql-data-warehouse-manage-compute-overview.md#resume-compute-performance-bk
[Skaala jõudlus]: sql-data-warehouse-manage-compute-overview.md#scale-performance-bk
[Turvalisuse ülevaade]: sql-data-warehouse-overview-manage-security.md
[SQL-i andmete ladu head tavad]: sql-data-warehouse-best-practices.md
[SQL-i andmebaas süsteemi vaated]: sql-data-warehouse-reference-tsql-system-views.md

<!--MSDN references-->
[SQL Server Data Tools]: https://msdn.microsoft.com/library/mt204009.aspx

<!--Other web references-->
[Azure'i portaal]: http://portal.azure.com/
