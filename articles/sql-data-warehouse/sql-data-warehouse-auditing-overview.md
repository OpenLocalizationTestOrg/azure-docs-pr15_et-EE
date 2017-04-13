<properties
   pageTitle="Auditi rakenduses SQL Azure'i andmebaas | Microsoft Azure'i"
   description="Alustamine rakenduses SQL Azure'i andmebaas kontrollimine"
   services="sql-data-warehouse"
   documentationCenter=""
   authors="ronortloff"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.workload="data-management"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="09/24/2016" 
   ms.author="rortloff;barbkess;sonyama"/>

# <a name="auditing-in-azure-sql-data-warehouse"></a>Auditi rakenduses SQL Azure'i andmebaas

> [AZURE.SELECTOR]
- [Auditi](sql-data-warehouse-auditing-overview.md)
- [Ohtude tuvastamise](sql-data-warehouse-security-threat-detection.md)

SQL-i andmebaas auditeerimine võimaldab teil salvestada oma andmebaasi auditit sündmuste logi sisse oma Azure Storage konto. Auditi aitavad teil säilitada nõuetele vastavuse, mõista andmebaasi tegevus ja saada aimu erinevused ja kõrvalekaldeid, mis võib viidata business probleemid või arvatav turvalisus rikkumised. SQL-i andmebaas auditeerimine ka integreerub Microsoft Power BI süvitsimineku aruandlust ja analüüsi.

Auditi tööriistad lubamine ja hõlbustada vastavusstandardeid järgimine, kuid ei garanteeri nõuetele vastavus. Azure'i programmid toetavad standardid nõuetele vastavuse kohta leiate lisateavet teemast <a href="http://azure.microsoft.com/support/trust-center/compliance/" target="_blank">Azure Usalduskeskus</a>.

+ [Valemiauditi ülevaade andmebaasidest]
+ [Auditi andmebaasi häälestamine]
+ [Analüüsida auditilogi aruanded]

##<a id="subheading-1"></a>Azure SQL-i andmete ladu andmebaasi auditi põhialused


SQL-i andmebaas andmebaasi auditeerimine abil saate:

- **Säilita** on valitud sündmuste auditilogi rada. Saate määratleda andmebaasi toimingute auditeeritavate kategooriad.
- **Aruande** andmebaasi tegevus. Eelkonfigureeritud aruandeid ja armatuurlaua abil saate kiiresti hakata tegevuste ja sündmuste teatamine.
- **Analüüsi** aruanded. Leiate kahtlaste sündmusi, ebatavalised tegevuste ja trende.

Saate konfigureerida sündmuse järgmiste kategooriate audit.

**Tavaline SQL-i** ja **SQL-i parameetriga** , mille kogutud auditilogid liigitatud  

- **Juurdepääs andmetele**
- **Muudatused (DDL)**
- **Andmete muutmine (piirmäära)**
- **Kontod, rollide ja õiguste (DCL)**
- **Salvestatud protseduur**, **kasutajanime** ja **tehingu haldus**.

Iga sündmuse kategooria, auditeerimine **edu** ja **tõrge** toimingud on konfigureeritud eraldi.

Lisateavet tegevuste ja sündmuste auditeerida kohta vt <a href="http://go.microsoft.com/fwlink/?LinkId=506733" target="_blank">Auditilogi Logi vorming viide (doc faili alla laadida)</a>.

Auditilogide on talletatud teie Azure storage konto. Saate määratleda auditilogi log säilitusperiood.

Auditeerimispoliitika poliitika saate määratleda kindla andmebaasi või serveri vaikepoliitika. Serveri Auditeerimispoliitika vaikepoliitika rakendatakse kõigile andmebaasidele serveris, mis on teatud olulise andmebaasi Auditeerimispoliitika määratletud.

Enne säte üles audit Auditeerimispoliitika sisse, kui kasutate mõnda muud ["alltaseme klient"](sql-data-warehouse-auditing-downlevel-clients.md).


##<a id="subheading-2"></a>Auditi andmebaasi häälestamine

1. Käivitage <a href="https://portal.azure.com" target="_blank">Azure portaali</a>.

2. Liikuge SQL-i andmebaas andmebaasi konfigureerimine tera / SQL Server, mida soovite kontrollida. Klõpsake nuppu **sätted** , üleval ja seejärel klõpsake sätet tera ja valige **audit**.

    ![][1]

3. Auditi konfigureerimine labale esmalt valimise **Päri auditeerimine sätted serverist** ruut. See võimaldab teil andmebaasi sätete määramiseks.

    ![][2]

4. Järgmiseks luba auditi, klõpsates nuppu **klõpsake** .

    ![][3]

5. Valige Auditeerimispoliitika konfiguratsiooni labale **Salvestusruumi üksikasjad** auditilogi logid salvestusruumi tera avamiseks. Valige Azure storage konto, kuhu salvestatakse logid ja säilitusperiood. **Näpunäide:** Kõik auditeeritud andmebaaside salvestusruumi sama konto abil kõige paremini eelkonfigureeritud aruannete mallid.

    ![][4]

6. Salvestusruumi üksikasjad konfiguratsiooni salvestamiseks nuppu **OK** .


7. Klõpsake **edu** ja **tõrge** kõigi sündmuste logi jaotises **Logimine sündmus**, või valige ürituste kategooriad.


8. Kui konfigureerite Valemiauditi andmebaasi, peate muutma oma klientrakenduse ja veenduge, et andmete auditeerimine õigesti jäädvustata ühendusstring. Märkige ruut teema [Muuta serveri FDQN ühendusstringi](sql-data-warehouse-auditing-downlevel-clients.md) alltaseme klientrakenduse kaudu.

9. Klõpsake nuppu **OK**.


##<a id="subheading-3">Analüüsida auditilogi aruanded</a>

Auditilogide on koondatud kogumi poe tabelite Azure storage konto häälestamise ajal valitud eesliitega **SQLDBAuditLogs** . Saate vaadata logifailid, nt <a href="http://azurestorageexplorer.codeplex.com/" target="_blank">Azure salvestusruumi Exploreri</a>tööriista abil.

Eelkonfigureeritud armatuurlaua aruandemalli on saadaval <a href="http://go.microsoft.com/fwlink/?LinkId=403540" target="_blank">allalaaditavad Exceli arvutustabeli</a> aitavad Logi andmete kiireks analüüsiks. Auditilogide oma malli kasutamiseks peab teil olema Excel 2013 või uuem versioon ja Power Query, mille saate alla laadida <a href="http://www.microsoft.com/download/details.aspx?id=39379">siin</a>.

Mall on väljamõeldud näidisandmed ja Power Query saate häälestada importida oma auditilogi otse oma Azure storage konto.

Aruandemalli töötamise kohta Täpsemate juhiste saamiseks lugege ning <a href="http://go.microsoft.com/fwlink/?LinkId=506731">Kuidas allalaadimiseks (doc)</a>.

![][5]


##<a id="subheading-4">Tavad kasutus valmistamisel</a>
Selles jaotises kirjeldus viitab ekraanipildid ülaltoodud. Kas <a href="https://portal.azure.com" target="_blank">Azure portaali</a> või <a href= "https://manage.windowsazure.com/" target="_bank">Klassikaline Azure klassikaline portaali</a> võib kasutada.


##<a id="subheading-5"></a>Salvestusruumi võtme regenereerimine

Valmistamisel tõenäoliselt värskendada oma salvestusruumi klahvid perioodiliselt. Kui värskendada oma võtmed tuleb salvestada uuesti poliitika. Protsess on järgmine:.


1. Liikuda Auditeerimispoliitika konfiguratsiooni tera (eespool kirjeldatud häälestamine jaotises auditeerimine) **Salvestusruumi kiirklahv** *esmane* *teisene* ja **salvestada**.
![][4]
2. Avage salvestusruumi konfiguratsiooni blade ja **taastada** *Accessi primaarvõtme*.

3. Minge tagasi Auditeerimispoliitika konfiguratsiooni tera, aktiveerige **Salvestusruumi kiirklahv** : *teisene* *esmane* ja vajutage **Salvesta**.

4. Minge tagasi salvestusruumi Kasutajaliidese ja **taastada** *Teisene kiirklahv* (nagu järgmisel klahvid tsükli ettevalmistamine.

##<a id="subheading-6"></a>Automatiseerimine
On mitu PowerShelli cmdlet-käskude abil saate auditi konfigureerimine Azure SQL-andmebaasis. Auditeerimispoliitika cmdlettide juurdepääsuks peate kasutama PowerShelli Azure'i ressursihaldur režiimis.

> [AZURE.NOTE] [Azure'i ressursihaldur](https://msdn.microsoft.com/library/dn654592.aspx) mooduli praegu eelvaade. See ei pruugi pakkuda Azure mooduli sama haldusvõimalusi.

Kui olete Azure'i ressursihaldur režiimis, käivitage `Get-Command *AzureSql*` loetleda saadaval cmdlet-käsud.


<!--Anchors-->
[Valemiauditi andmebaasidest]: #subheading-1
[Auditi andmebaasi häälestamine]: #subheading-2
[Analüüsida auditilogi aruanded]: #subheading-3


<!--Image references-->
[1]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing.png
[2]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-inherit.png
[3]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-enable.png
[4]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-storage-account.png
[5]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-dashboard.png


<!--Link references-->
