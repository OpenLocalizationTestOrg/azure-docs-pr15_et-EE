<properties
   pageTitle="Ühenduse loomine Hadoopi koos taru ODBC-draiver Exceli | Microsoft Azure'i"
   description="Saate teada, kuidas häälestada ja kasutada Microsofti taru ODBC-draiver Exceli päringu andmetega on Hdinsightiga kobar."
   services="hdinsight"
   documentationCenter=""
   authors="mumian"
   manager="jhubbard"
   tags="azure-portal"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/19/2016"
   ms.author="jgao"/>

#<a name="connect-excel-to-hadoop-with-the-microsoft-hive-odbc-driver"></a>Ühenduse Exceli Hadoopi Microsoft taru ODBC-draiver

[AZURE.INCLUDE [ODBC-JDBC-selector](../../includes/hdinsight-selector-odbc-jdbc.md)]

Microsofti Big Data lahenduse integreerub Apache Hadoop kogumite, Windows Azure Hdinsightiga juurutatud Microsoft Business Intelligence (BI) komponendid. Näide selle integreerimine on võimalus ühenduse taru andmebaas on Hadoopi klaster Hdinsightiga Microsoft taru avatud andmebaasipöörduse (ODBC) draiveri kasutamine rakenduses Excel.

Samuti on võimalik seotud mõne Hdinsightiga kobar ja muude andmeallikatega, sealhulgas muude (mitte Hdinsightiga) Hadoopi kogumite, Exceli lisandmooduli Microsoft Power Query for Excel abil andmete ühendamine. Installimine ja kasutamine Power Query kohta leiate teavet teemast [Ühenduse loomine Exceli Power Query Hdinsightiga][hdinsight-power-query].

> [AZURE.NOTE] Ajal selles artiklis toodud juhiseid saab kasutada Linux või Windowsi-põhiste Hdinsightiga kobar, Windows on vaja klient töökoht.

**Eeltingimused**:

Enne alustamist selles artiklis, peab teil olema järgmised:

- **An Hdinsightiga kobar**. Üks loomiseks vaadake teemat [alustamine Windows Azure Hdinsightiga][hdinsight-get-started].
- Office 2013 Professional Plus, Office 365 Pro Plusi, Excel 2013 eraldiseisev versioon või Office 2010 Professional Plus **A töökoha** .


##<a name="install-microsoft-hive-odbc-driver"></a>Installige Microsoft taru ODBC-draiver

Laadige alla ja installige Microsoft taru ODBC-draiver [Allalaadimiskeskuse]kaudu[hive-odbc-driver-download].

Selle juht saab installida nii 32-bitine või 64-bitise versiooni Windows 7, Windows 8, Windows 10, Windows Server 2008 R2 ja Windows Server 2012 ja võimaldab ühenduse Windows Azure Hdinsightiga (1,6 ja uuemad versioonid) ja Azure Hdinsightiga emulaator (v.1.0.0.0 ja uuemad versioonid). Peaksite installima versioon, mis vastab teie versioon, kui kasutate ODBC-draiver rakenduse. Selles õpetuses kasutatakse juht Office Excelis.

##<a name="create-hive-odbc-data-source"></a>Taru ODBC andmeallika loomine

Järgmised sammud näitavad, kuidas taru ODBC andmeallika loomine.

1. Windows 8 või Windows 10, vajutage Windowsi klahvi avamiseks avakuvale ja tippige **andmeallikad**.
2. Klõpsake **häälestamine ODBC andmete allikatest (32-bitine)** või **ODBC andmeallikad (64-bitine) häälestamine** sõltuvalt teie Office'i versioon. Kui kasutate Windows 7, valige **ODBC-andmeallikate (32-bitine)** või **ODBC andmeallikad (64-bitine)** **Haldustööriistad**. See käivitab dialoogiboksi **ODBC andmeallika administraator** .

    ![OBDC andmeallika administraator][img-hdi-simbahiveodbc-datasource-admin]

3. Kasutaja DNS-i, klõpsake nuppu **Lisa** **Uus andmeallikas loomine** viisardi avamiseks.
4. Valige **Microsoft taru ODBC-draiver**ja klõpsake siis nuppu **valmis**. See käivitab dialoogiboksi **Microsoft taru ODBC draiver DNS-i häälestus** .

5. Tippige või valige järgmised väärtused:

    Atribuut|Kirjeldus
    ---|---
    Andmeallika nime järgi.|Andmeallika nime andmine
    Host|Sisestage &lt;HDInsightClusterName >. azurehdinsight.net. Näiteks myHDICluster.azurehdinsight.net
    Port|Kasutage <strong>443</strong>. (See port on muudetud: 563 443.)
    Andmebaasi|Saate kasutada <strong>vaikimisi</strong>.
    Taru serveri tüüp|Valige <strong>taru Server 2</strong>
    Süsteem|Valige <strong>Azure Hdinsightiga</strong>
    HTTP-tee|Selle tühjaks jätta.
    Kasutajanimi|Sisestage Hdinsightiga kobar kasutaja kasutajanimi. See on loodud käigus kobar sätte kasutajanimi. Kui kasutasite suvandi kiire loomine, vaikimisi kasutajanimi on <strong>administraator</strong>.
    Parooli|Sisestage Hdinsightiga kobar kasutaja parooli.
    </table>

    On mõned olulised parameetrid, millega peaks arvestama, kui klõpsate nuppu **Täpsemad suvandid**.

    Parameetri|Kirjeldus
    ---|---
    Kasutage Omapäringute|Kui see on valitud, ODBC-draiver ei teisendada TSQL HiveQL proovi. Seda tuleb kasutada ainult siis, kui teil on 100% kindel esitamise puhas HiveQL laused. Kui ühendate SQL serveri või Azure'i SQL-andmebaasi, saate jätke see märkimata.
    Read toodud ploki|Kui tõmbamine suurt hulka kirjeid, see parameeter häälestamine võib vajalik optimaalse esinemiste tagamiseks.
    Vaikimisi stringipikkus veerg, kahendandmete veeruga pikkus, Decimal veerg skaala|Andmetüüp pikkusega ja andmed võib mõjutada andmete tagastamise. Need põhjustab tingitud arvutustäpsuse ja/või kärpimine tagastatakse vale teave.


    ![Täpsemad suvandid][img-HiveOdbc-DataSource-AdvancedOptions]

6. Klõpsake andmeallika testimiseks **testida** . Kui andmeallikas on õigesti konfigureeritud, kuvatakse see *KONTROLLIB on edukalt lõpule viidud!*.
7. Klõpsake nuppu **OK** , et sulgeda dialoogiboks Test. Uue andmeallika peaks nüüd loetletakse **ODBC andmeallika administraator**.
8. Klõpsake nuppu **OK** väljumiseks.

##<a name="import-data-into-excel-from-hdinsight"></a>Andmete importimine rakendusse Excel hdinsightist

Järgmised juhised kirjeldavad võimalus taru tabeli andmete importimine Exceli töövihikusse ODBC andmeallika põhjal loodud eespool antud juhiseid.

1. Uue või olemasoleva töövihiku Excelis avada.
2. Klõpsake menüüs **andmed** nuppu **Andmete muust allikast**ja klõpsake **Kaudu andmeühendusviisardi** **Andmeühenduse viisardi**käivitamiseks.

    ![Avatud andmeühendusviisardi][img-hdi-simbahiveodbc.excel.dataconnection]

3. **DSN-i ODBC** andmeallika valimine ja seejärel klõpsake nuppu **edasi**.
4. ODBC-andmeallikatest, valige eelmises etapis loodud andmeallika nimi ja klõpsake siis nuppu **edasi**.
5. Sisestage kobar viisardi parool uuesti, ja seejärel nuppu **testi** konfiguratsiooni kinnitamiseks veel kord vajadusel.
6. Klõpsake nuppu **OK** , et sulgeda dialoogiboks test.
7. Klõpsake nuppu **OK**. Oodake, kuni dialoogiboksi avamiseks **Valige andmebaas ja tabel** . See võib võtta mõne hetke.
8. Valige tabel, mille soovite importida, ja klõpsake siis nuppu **edasi**. *Hivesampletable* on valimi taru tabeli, mis sisaldab Hdinsightiga kogumite.  Te saate valida, kui te pole loonud ühte. Lisateavet Käivita taru päringud ja taru tabelite loomine leiate teemast [Kasutamine taru koos Hdinsightile][hdinsight-use-hive].
8. Klõpsake nuppu **valmis**.
9. Dialoogiboksis **Andmete importimine** saate muuta või määrata päringu. Selleks klõpsake nuppu **Atribuudid**. See võib võtta mõne hetke.
10. Klõpsake vahekaarti **määratlus** ja seejärel Lisa **limiit 200** taru select-lauses **käsu tekst** tekstiväljale. Tagastatud kirjekomplekti 200 piirab muutmist.

    ![Ühenduse atribuudid][img-hdi-simbahiveodbc-excel-connectionproperties]

11. Klõpsake nuppu **OK** , et sulgeda dialoogiboks ühenduse atribuudid.
12. Klõpsake nuppu **OK** , et sulgeda dialoogiboks **Andmete importimine** .  
13. Sisestage parool uuesti ja seejärel klõpsake nuppu **OK**. Kulub mõne hetke enne andmeid saab importida Excelisse.

##<a name="next-steps"></a>Järgmised sammud

Selles artiklis te teada, kuidas kasutada Microsoft taru ODBC-draiver andmete toomiseks Hdinsightiga teenuse Excel. Samuti saate tuua andmeid Hdinsightiga teenuse SQL-i andmebaasi. Samuti on võimalik andmete üleslaadimine teenusesse Hdinsightiga. Lisateavet leiate järgmistest teemadest.

- [Analüüsi viivitus lennuandmetega Hdinsightiga abil][hdinsight-analyze-flight-data]
- [Laadi andmed Hdinsightiga][hdinsight-upload-data]
- [Hdinsightiga Sqoop kasutamine] [hdinsight-use-sqoop]


[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-get-started]: hdinsight-hadoop-tutorial-get-started-windows.md

[hive-odbc-driver-download]: http://go.microsoft.com/fwlink/?LinkID=286698

[img-hdi-simbahiveodbc-datasource-admin]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.DataSourceAdmin1.png
[img-HiveOdbc-DataSource-AdvancedOptions]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.HiveOdbc.DataSource.AdvancedOptions1.png
[img-hdi-simbahiveodbc-excel-connectionproperties]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveODBC.Excel.ConnectionProperties1.png
[img-hdi-simbahiveodbc.excel.dataconnection]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.Excel.DataConnection1.png
