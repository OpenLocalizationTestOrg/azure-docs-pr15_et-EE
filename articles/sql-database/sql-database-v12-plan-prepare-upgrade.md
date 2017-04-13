<properties
    pageTitle="SQL-i andmebaasi V12 teenusele leping | Microsoft Azure'i"
    description="Kirjeldatakse ettevalmistused ja versioonilt Azure SQL-i andmebaasi V12 seotud piirangud."
    services="sql-database"
    documentationCenter=""
    authors="MightyPen"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="genemi"/>


# <a name="plan-and-prepare-to-upgrade-to-sql-database-v12"></a>Plaanimine ja võtta kasutusele SQL-i andmebaasi V12 ettevalmistamine


Selles teemas kirjeldatakse plaanimine ja ettevalmistused, peate tegema oma Azure SQL-i andmebaaside versiooni V11 operatsioonisüsteemile V12.


Uue [Azure portaali](https://portal.azure.com/) on saadaval toetada V12 versiooniuuendus.


Järgmises tabelis on loetletud teiste spikriteemade V12 jaoks.


| Pealkiri ja link | Sisu kirjeldus |
| :--- | :--- |
| [Mis on uut rakenduses SQL-i andmebaasi V12](sql-database-v12-whats-new.md) | Kirjeldab, kuidas V12 toob täielik tarkvarakomplektiga Microsoft SQL Server Azure'i SQL-andmebaasi lähemale üksikasju. |
| [SQL-i andmebaasi V12 andmebaasi loomine](sql-database-get-started.md) | Kirjeldab, kuidas saate luua uue SQL Azure'i andmebaasi versiooni V12. See kirjeldab erinevaid võimalusi Lisaks vaid mõnda tühja andmebaasi. |


## <a name="plan-ahead"></a>Plaanima


Alltoodud kirjeldada õppimine ja otsustamine, peab tegelema, enne soovitud suunas üleminekut Azure SQL-i andmebaasi v12 toimingud.

### <a name="version-clarification"></a>Versioon selgitus


Selle dokumendi käsitleb Microsoft Azure'i SQL-andmebaasi Uuenda versiooni V11 V12. Ametlikumalt versioonide numbrid on lähedale järgmisest kahest väärtusest, nagu Transact-SQL-lause **Valige @@version; ** :


- 12.0.2000.8 *(või natuke suurem, V12)*
- 11.0.9228.18 *(V11)*
 - Mõnikord V11 on ka edaspidi SAWA v2.

### <a name="service-tier-planning"></a>Teenuse taseme kavandamine


Azure'i SQL-andmebaasi alustades V12, toetab ainult nimega Basic, Standard, ja teenuse astme. Veebi ja teenuse kiht V12 ei toetata. Seetõttu, kui plaanite uuendada oma Azure'i SQL-andmebaasiga, mis on praegu veebi- ja teenuse kiht, saate otsustada, selle uue teenuse kiht peaks olema.


Astme Basic, Standard, ja teenuste kohta üksikasjaliku teabe saamiseks vaadake:

- [SQL-andmebaasi teenuse astme](sql-database-service-tiers.md)
- [Uus teenus astme SQL-andmebaasi Web/Business andmebaase versioonile](sql-database-upgrade-server-portal.md)



### <a name="review-the-geo-replication-configuration"></a>Vaadake üle Geo-Dispersioonanalüüs konfigureerimine


Kui SQL Azure'i andmebaasi on konfigureeritud Geo-kopeerimise, peaks dokumendi oma praeguse konfiguratsiooni ja Lõpeta Geo-Dispersioonanalüüs enne alustamist versioonitäienduse ettevalmistamiseks toimingud. Pärast versioonitäienduse lõpulejõudmist peate oma andmebaasi Geo-kopeerimise ümber konfigureerida.


Strateegia on jäta allika puutumata ja testimine andmebaasi koopia.


## <a name="preparation-actions"></a>Ettevalmistamise toimingud


Kui olete lõpetanud oma kavandamise, saate teha toimingu juhised, mis on kirjeldatud järgmistes osades viimases etapis versioonitäienduse ettevalmistamiseks.


Üksikasjalikku kirjeldust versioonitäienduse viimases etapis on saadaval spikri teemad, mis on lingitud spikriteema ülaosas.


### <a name="service-tier-actions"></a>Esimese taseme kinnitamine.


Esimese taseme hinnad veebi ja teenuse V12 ei toetata.


Veebi- ja andmebaasi korral V11 Azure SQL-i andmebaasi versiooniuuendus pakub andmebaasi kasutusele toetatud taseme. Uuele versioonile soovitab kiht, mis sobib teie andmebaasi töökoormus ajalugu. Siiski saate mis tahes toetatud kiht, mis teile meeldib.


Saate vähendada uuendamisel tarvilikud toimingud, saate aktiveerida eemale Web Business taseme andmebaasi V11 enne uuele versioonile. Selleks uue [Azure portaali](https://portal.azure.com/).


Kui te pole kindel, millist teenuse kiht üle minna, Standard teise taseme S2 võib olla mõistlikum esialgne valik. Mis tahes madalama taseme on vähem kui taseme veebi ja ressursse oli.


### <a name="suspend-geo-replication-during-upgrade"></a>Peatada uuendamisel Geo-Dispersioonanalüüs


Uuele versioonile v12 ei saa käivitada, kui Geo-Dispersioonanalüüs on aktiivne andmebaas. Esmalt peate konfigureerima andmebaasi Geo-Dispersioonanalüüs kasutamise lõpetada.


Pärast versioonitäienduse lõpulejõudmist saate konfigureerida andmebaasi Geo-Dispersioonanalüüs uuesti kasutama.


### <a name="open-ports-on-an-azure-vm-for-client-connectivity"></a>Avatud pordid on Azure VM kliendi Ühenduvus


Kui teie klient programmi loob ühenduse SQL-i andmebaasi V12 ajal Azure virtuaalse masina (VM) oma kliendi töötab, peate avama järgmisi pordi vahemikke VM:

- 11000-11999
- 14000-14999


Klõpsake [siin](sql-database-develop-direct-route-ports-adonet-v12.md) V12 SQL-i andmebaasi Porte üksikasjad. Jõudlustäiustused SQL-i andmebaasi v12 vajavad pordid.


##<a id="limitations"></a>Ajal ja pärast versioonile V12 piirangud


### <a name="portals-for-v12"></a>Portaalide V12 jaoks


On kolme portaalide Azure ja iga on erinevate oskusi SQL-i andmebaasi V12 kohta.


- [http://Portal.Azure.com/](https://portal.azure.com/)<br/>Azure'i portaalis on uus ja on endiselt eelvaade olek. See portaal pole päris veel veebisaidil täielik General Availability (GA). See portaal:
 - Saate hallata oma V12 server ja andmebaas.
 - Saate värskendada oma V11 andmebaasi V12.


- [http://Manage.windowsazure.com/](http://manage.windowsazure.com/)<br/>See klassikaline Azure portaali võib lõpuks järk-järgult See portaal:
 - Saate hallata oma V12 server ja andmebaas.
 - Saab *pole* teie V11 V12 andmebaasi täiendamine.


- (http://*yourservername*. database.windows.net)<br/>SQL Azure'i andmebaasi klassikaline portaal:
 - Saate*pole* V12 serverite haldamine.


Soovitame teil ühenduse loomiseks oma Azure SQL-i andmebaasid Visual Studio 2015 (VS2015). VS2015 saab kasutada näiteks järgmised toimingud:


- Transact-SQL-i käitada.
- Kujundamise skeem.
- Arendada andmebaasi, kas võrguühenduse lubamine või keelamine.


Või selle asemel, tasuta alla laadida [Visual Studio ühenduse 2015](https://www.visualstudio.com/vs/community/), mis on VS2015 kõiki võimalusi pakkuva versioon.


Vanema Azure klassikaline portaalis, klõpsake lehel andmebaasi, saate klõpsata **avatud Visual Studio** käivitada VS2015 Azure SQL-andmebaasiga ühenduse arvutis.


Teise võimalusena saate SQL Server Management Studio (SSMS) 2014 koos [CU6](http://support.microsoft.com/kb/3031047/) Azure SQL-i andmebaasiga ühenduse loomiseks. Lisateavet on sellest ajaveebipostitusest:<br/>[Kliendi instrumentaarium Azure'i SQL-andmebaasi värskendusi](https://azure.microsoft.com/blog/2014/12/22/client-tooling-updates-for-azure-sql-database/).


> [AZURE.IMPORTANT] On soovitatav alati kasutada uusimat versiooni Management Studio jäävad sünkroonitud värskendusega ning Microsoft Azure'i SQL-andmebaasi. [SQL Server Management Studio värskendada](https://msdn.microsoft.com/library/mt238290.aspx).


### <a name="limitation-during-upgrade-to-v12"></a>Piirangud *ajal* Uuenda V12


V11 andmebaasi endiselt saadaval andmepääsu V12 versiooniuuenduse käigus. Veel on paar piiranguid silmas pidada.


| Piirangud | Kirjeldus |
| :--- | :--- |
| Versiooniuuenduse kestus | Versiooniuuenduse kestus sõltub suurust, väljaande ja arv: andmebaase serveri. Versiooniuuenduse saab käitada tundi päevade serverite eriti serverite, mis on andmebaaside jaoks:<br/><br/>*Suurem kui 50 GB või<br/> * Veebisaidil-premium teenuse kiht<br/><br/>Luua uusi andmebaase serveri versiooni täiendamise käigus saate suurendada ka versioonitäienduse kestus. |
| Pole Geo-Dispersioonanalüüs | Geo-Dispersioonanalüüs V12 serveris, mis on nüüd seotud V11 täiendamist ei toetata. |
| Andmebaas on saadaval lühidalt viimases etapis versiooniuuenduse v12 | Versiooniuuenduse ajal jäävad saadaval serverisse V11 andmebaasidele. Ühenduse server ja andmebaaside aga viimases etapis, kui parameetrit üle algab V11 valmis v12 lühidalt pole saadaval.<br/><br/>Parameetrit perioodi jooksul saate vahemikus 40 sekundi 5 minutit. Enamik servereid, peaks Lülituge lõpuleviimine 90 sekundi pärast. Vahetamise aja jooksul suurendab serverid, mis on suure hulga andmebaase või kui andmebaasid on raske kirjutamine töökoormus. |


### <a name="limitation-after-upgrade-to-v12"></a>Piirangud *pärast* versioonile V12


| Piirangud | Kirjeldus |
| :--- | :--- |
| Ei saa pöörduda V11 | Pärast versioonitäienduse kohapealne, ei saa tulemus taastatud või tagasi võtta. |
| Veebi- ja esimese taseme | Pärast versioonitäienduse algust uue V12 andmebaasi server enam tuvasta ega Aktsepteeri veebi- ja teenuse kiht. |



### <a name="export-and-import-after-upgrade-to-v12"></a>Eksportida ja importida *pärast* versioonitäienduse V12


Saate eksportida ja importida V12 andmebaasi [Azure portaali](https://portal.azure.com/)kaudu. Või saate eksportimine või importimine ühte järgmistest tööriistade abil tehke järgmist.


- SQL Server Management Studio (SSMS)
- Visual Studio 2015
- Andmete taseme rakenduse raames (DacFx)


Siiski kasutada tööriistu, peate esmalt on või installida oma uusimate värskenduste tagamaks, et nad toetavad V12 uusi funktsioone:


- [SQL Server Management Studio 2014 koondvärskenduse 6](http://support2.microsoft.com/kb/3031047)
- [SQL serveri andmebaasi instrumentaarium Visual Studio 2013 värskendus veebruar 2015](https://msdn.microsoft.com/data/hh297027)
- [Veebruar 2015 andmete taseme rakenduse raames (DacFx) SQL Azure'i andmebaasi V12](http://www.microsoft.com/download/details.aspx?id=45886)


> [AZURE.NOTE] Eelmise tööriistalinkide värskendati või pärast 2 märts 2015. Soovitame kasutada nende tööriistade uuemad värskendused.


#### <a name="automated-export"></a>Automatiseeritud eksportimine


Automatiseeritud ekspordi endiselt saadaval eelvaade.  Kliendi tööriista värskendusi pole vaja kasutamisel automatiseeritud ekspordi.  Kui valite fail ja kohapealne server importida, eelnimetatud instrumentaarium värskendusi on vaja edukalt importida.


### <a name="restore-to-v12-of-a-deleted-v11-database"></a>V12 kustutatud V11 andmebaasi taastamine

Järgmistel selgitatakse, et kustutatud V11 Azure SQL-andmebaasi saab taastada V12 Azure'i SQL-andmebaasi serverisse.

1. Oletame, et teil on V11 Azure'i SQL-andmebaasi server. <br/> Server teil on aegunud Web Business teenuse kiht andmebaasi.
2. Andmebaasi kustutamine.
3. Serveri versiooniks V12.
4. Järgmine taastada andmebaasi serverisse. <br/> Andmebaasi seeläbi uuendamist v12 Standard teenuse kiht S0 tasemel.
5. Mida saab vahetada andmebaasi toetatud esimese taseme, kui S0 ei ole oma eelistused.


### <a name="powershell-cmdlets"></a>PowerShelli cmdlet-käsud


PowerShelli cmdlet-käsud on saadaval käivitamiseks, peatamiseks või jälgida versiooniuuenduse Azure SQL-i andmebaasi v12 V11 või mõni muu eel-V12 versioon.

- [Versioonitäienduse SQL-i andmebaasi v12 PowerShelli abil](sql-database-upgrade-server-powershell.md)

Viide dokumendid nende PowerShelli cmdlet-käskude kohta vaadake:


- [Get-AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603582.aspx)
- [Algus-AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt619403.aspx)
- [Lõpeta – AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603589.aspx)


Cmdlet Peata tähendab, et tühistada, ei saa peatada. Ei ole nii jätkamiseks versiooniuuenduse uuesti alates algusest peale. Cmdlet Peata migreerimispaketiga ja väljastab kõik asjakohased ressursid.


## <a name="failure-resolution"></a>Tõrke lahendamine


Uuele versioonile nurjumisel paaritu mingil põhjusel andmebaasi V11 on aktiivne ja saadaval, nagu tavaliselt.


## <a name="related-links"></a>Seotud lingid


- Microsoft Azure'i [Eelvaade funktsioonid](https://azure.microsoft.com/services/preview/)


<!--Anchors-->
[Subheading 1]: #subheading-1
