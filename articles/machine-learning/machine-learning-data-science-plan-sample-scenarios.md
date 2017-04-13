<properties
    pageTitle="Täiustatud analüüsi võtted protsessi ja tehnoloogia ja Azure seadme õppimine | Microsoft Azure'i"
    description="Valige sobiv võtted ennustav andmete meeskonna teadus protsessi täpsemalt teha."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na" 
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016" 
    ms.author="bradsev" />


# <a name="scenarios-for-advanced-analytics-in-azure-machine-learning"></a>Täiustatud analüüsi Azure seadme õppe stsenaariumid

See artikkel kirjeldab erinevaid valimi andmeallikate ja target stsenaariumi, mis võib käsitleda [Meeskonnatöö andmete teadus protsess (TDSP)](data-science-process-overview.md)alusel. Funktsiooni TDSP pakub süsteemne lähenemine meeskondade koostöötamiseks nutikad rakenduste loomine. Siin esitatud stsenaariumide illustreerimine suvandid saadaval töötlemise töövoo andmete omadused, allikas asukohad ja target hoidlate Azure sõltuvad.

**Otsusepuu kuvamine** valimise valimi stsenaariumid, mis sobib teie andmete ja eesmärk on esitatud viimases osas.

>[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


Iga järgmistes jaotistes esitatakse valimi stsenaariumi. Iga stsenaariumi puhul, on võimalik andmete teadus- või täiustatud analüüsi kulgemist ja toetavad Azure ressursid on loetletud.

>[AZURE.NOTE]**Kõigi järgmistel juhtudel peate:**
><br/>
>* [Salvestusruumi konto loomine](../storage/storage-create-storage-account.md)
><br/>
>* [Mõne Azure seadme õ tööruumi loomine](machine-learning-create-workspace.md)


## <a name="smalllocal"></a>Stsenaarium \#1: väike keskmise tabeli andmekomplekti kohaliku failid

![Väike, Keskmine kohalike failide][1]

#### <a name="additional-azure-resources-none"></a>Lisaressursid Azure: pole

1.  [Azure'i masina Õppekeskuse Studio](https://studio.azureml.net/)sisselogimine.

2.  Laadige Andmekomplekt.

3.  Azure'i masina õ katse meilivoo alustades üleslaaditud andmehulgad koostada.

## <a name="smalllocalprocess"></a>Stsenaarium \#2: väike Keskmine andmekomplekti kohaliku faile, mis nõuavad töötlemine

![Väike, Keskmine kohalike failide töötlemine][2]

#### <a name="additional-azure-resources-azure-virtual-machine-ipython-notebook-server"></a>Lisaressursid Azure: Azure virtuaalse masina (IPython märkmiku server)

1.  Looge Azure virtuaalse masina IPython märkmik käitamise.

2.  Laadi andmed on Azure storage ümbrises.

3.  Eelnevalt protsessi ja puhastada andmete juurdepääsu andmeid Azure storage container IPython märkmikus.

4.  Muuta andmete puhastamiseks, tabelina.

5.  Azure'i plekid ümber andmed salvestada.

6.  [Azure'i masina Õppekeskuse Studio](https://studio.azureml.net/)sisselogimine.

7.  [Andmete importimine] Azure plekid andmeid lugeda[ import-data] mooduli.

8. Azure'i masina õ katse meilivoo alustades sissevõetava andmehulgad koostada.

## <a name="largelocal"></a>Stsenaarium \#3: suure andmekogumi kohaliku faili, suunamise Azure plekid

![Kohaliku suuri faile][3]

#### <a name="additional-azure-resources-azure-virtual-machine-ipython-notebook-server"></a>Lisaressursid Azure: Azure virtuaalse masina (IPython märkmiku server)

1.  Looge Azure virtuaalse masina IPython märkmik käitamise.

2.  Laadi andmed on Azure storage ümbrises.

3.  Eelnevalt protsessi ja andmete IPython märkmikku, juurdepääsu andmeid Azure plekid puhastada.

4.  Muuta andmete puhastamiseks, tabelina, vajaduse korral.

5.  Andmete uurimine ja luua funktsioone vastavalt vajadusele.

6.  Väikestele ja keskmise andmete valimi eraldamiseks.

7.  Azure'i plekid valimisse andmed salvestada.

8. [Azure'i masina Õppekeskuse Studio](https://studio.azureml.net/)sisselogimine.

9. [Andmete importimine] Azure plekid andmeid lugeda[ import-data] mooduli.

10. Azure'i masina õ katse meilivoo alustades sissevõetava andmehulgad koostada.


## <a name="smalllocaltodb"></a>Stsenaarium \#4: Keskmine andmekomplekti kohaliku faili, SQL Server on Azure Virtal masina suunamise väike

![Väike Keskmine kohaliku failide SQL Azure'i DB.][4]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Lisaressursid Azure: Azure virtuaalse masina (SQL serveri / IPython märkmiku server)

1.  Azure virtuaalse masina töötab SQL serveri + IPython märkmiku loomine

2.  Laadi andmed on Azure storage ümbrises.

3.  Eelnevalt protsessi ja Azure storage container IPython märkmiku abil andmete puhastada.

4.  Muuta andmete puhastamiseks, tabelina, vajaduse korral.

5.  Andmete salvestamiseks VM kohaliku failid (IPython märkmiku töötab VM, kohalikud draivid viidata VM draivid).

6.  SQL Serveri andmebaasiga, mis töötab ka Azure VM andmete laadimine.

    Suvand \#1: SQL Server Management Studio abil.

    - SQL serveri VM sisse logida
    - Käivitage SQL Server Management Studio.
    - Andmebaasi- ja sihtsaitide tabelite loomine.
    - Kasutage ühte hulgilisamine importida viise andmete laadimiseks VM kohaliku failidest.

    Suvand \#2: abil IPython märkmiku – pole soovitatav keskmise suurusega ja suurem andmekomplektide<!-- -->    
    - ODBC-ühendusstring kasutada SQL serveri VM.
    - Andmebaasi- ja sihtsaitide tabelite loomine.
    - Kasutage ühte hulgilisamine importida viise andmete laadimiseks VM kohaliku failidest.

7.  Andmete uurimine, luua funktsioone vastavalt vajadusele. Pange tähele, et funktsioonid ei pea olema juhtumiga andmebaasitabelites. Märkus ainult nende loomiseks vajalikud päring.

8. Otsustamine andmete valimi suurus, kui vaja ja/või soovitud.

9. [Azure'i masina Õppekeskuse Studio](https://studio.azureml.net/)sisselogimine.

10. Otse [Andmete importimine] SQL serveri andmeid lugeda[ import-data] mooduli. Kleepige vajalikud päring, mis ekstraktib väljad, loob funktsioonid ja näidised andmed otse sisse [Andmete importimine] vajadusel[ import-data] päringu.

11. Azure'i masina õ katse meilivoo alustades sissevõetava andmehulgad koostada.

## <a name="largelocaltodb"></a>Stsenaarium \#5: suure andmekogumi kohaliku failides suunata SQL Server Azure'i VM

![Kohaliku suuri faile SQL Azure'i DB.][5]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Lisaressursid Azure: Azure virtuaalse masina (SQL serveri / IPython märkmiku server)

1.  Azure virtuaalse masina töötab SQL serveri ja IPython märkmiku loomine

2.  Laadi andmed on Azure storage ümbrises.

3.  (Valikuline) Andmete eelnevalt protsess ja selge.

    lisamine.  Eelnevalt protsessi ja andmete IPython märkmikku, juurdepääsu andmeid Azure plekid puhastada.

    b.  Muuta andmete puhastamiseks, tabelina, vajaduse korral.

    c.  Andmete salvestamiseks VM kohaliku failid (IPython märkmiku töötab VM, kohalikud draivid viidata VM draivid).

4.  SQL Serveri andmebaasiga, mis töötab ka Azure VM andmete laadimine.

    lisamine.  Logi SQL serveri VM.

    b.  Kui andmed ei ole juba salvestanud, andmete faile alla laadida Azure storage container kohalik-VM kausta.

    c.  Käivitage SQL Server Management Studio.

    d.  Andmebaasi- ja sihtsaitide tabelite loomine.

    e.  Kasutage ühte hulgilisamine importida viise andmete laadimine.

    f.  Kui tabeli ühendused on nõutavad, luua registrite kiirendamiseks ühendused.

     > [AZURE.NOTE] Kiirem laadimine suure andmed suuruses, on soovitatav, et loote piiretega tabelid ja hulgi paralleelselt andmed importida. Lisateabe saamiseks vt [Paralleelselt SQL-i liigendatud tabelite andmete importimist](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).

5.  Andmete uurimine, luua funktsioone vastavalt vajadusele. Pange tähele, et funktsioonid ei pea olema juhtumiga andmebaasitabelites. Märkus ainult nende loomiseks vajalikud päring.

6.  Otsustamine andmete valimi suurus, kui vaja ja/või soovitud.

7.  [Azure'i masina Õppekeskuse Studio](https://studio.azureml.net/)sisselogimine.

8. Otse [Andmete importimine] SQL serveri andmeid lugeda[ import-data] mooduli. Kleepige vajalikud päring, mis ekstraktib väljad, loob funktsioonid ja näidised andmed otse sisse [Andmete importimine] vajadusel[ import-data] päringu.

9. Lihtne Azure seadme õ katsetamiseks meilivoo alustades üleslaaditud andmekomplekti

## <a name="largedbtodb"></a>Stsenaarium \#6: klõpsake mõnda SQL serveri andmebaasi kohapeal, SQL Server on Azure virtuaalse masina suunatud suurele andmekomplekti

![SQL suure DB kohapeal Azure SQL-i dB][6]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Lisaressursid Azure: Azure virtuaalse masina (SQL serveri / IPython märkmiku server)

1.  Azure virtuaalse masina töötab SQL serveri ja IPython märkmiku loomine

2.  Kasutage ühte andmed eksportida SQL serveri andmete eksportimine gigabaiti võimalusi.

    > [AZURE.NOTE] Kui te ei soovi andmebaasist kohapeal (rohkem) muul viisil täieliku andmebaasi liikumiseks Azure SQL serveri eksemplar kõigi andmete teisaldamine. Andmete eksportimine, andmebaasi luua ja laadi/impordi andmed sihtandmebaasi ja tehke muu meetodi sammu vahele jätta.

3.  Laadige gigabaiti Azure storage ümbrises.

4.  Klõpsake soovitud Azure virtuaalse masina töötab SQL serveri andmebaasi andmeid laadida.

    lisamine.  Logi SQL serveri VM.

    b.  Kausta local-VM alla laadida ka Azure storage container andmefailid.

    c.  Käivitage SQL Server Management Studio.

    d.  Andmebaasi- ja sihtsaitide tabelite loomine.

    e.  Kasutage ühte hulgilisamine importida viise andmete laadimine.

    f.  Kui tabeli ühendused on nõutavad, luua registrite kiirendamiseks ühendused.

    > [AZURE.NOTE] Kiirem laadimine suure andmed suuruses, sektsioonitud tabelite loomine ja et hulgi paralleelselt andmed importida. Lisateabe saamiseks vt [Paralleelselt SQL-i liigendatud tabelite andmete importimist](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).

5.  Andmete uurimine, luua funktsioone vastavalt vajadusele. Pange tähele, et funktsioonid ei pea olema juhtumiga andmebaasitabelites. Märkus ainult nende loomiseks vajalikud päring.

6.  Otsustamine andmete valimi suurus, kui vaja ja/või soovitud.

7.  [Azure'i masina Õppekeskuse Studio](https://studio.azureml.net/)sisselogimine.

8. Otse [Andmete importimine] SQL serveri andmeid lugeda[ import-data] mooduli. Kleepige vajalikud päring, mis ekstraktib väljad, loob funktsioonid ja näidised andmed otse sisse [Andmete importimine] vajadusel[ import-data] päringu.

9. Lihtne Azure seadme õ proovida alustades üleslaaditud andmekomplekti kulgemist.

### <a name="alternate-method-to-copy-a-full-database-from-an-on-premises--sql-server-to-azure-sql-database"></a>Mõnel muul viisil kopeerimiseks asutusesisese SQL serveri täielik andmebaasi Azure SQL-andmebaas

![Kohaliku DB eemaldada ja manustada SQL Azure'i DB.][7]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Lisaressursid Azure: Azure virtuaalse masina (SQL serveri / IPython märkmiku server)

Kopeerida oma SQL serveri VM kogu SQL serveri andmebaasi, saate tuleks andmebaasi ühe asukoha ja serveri teise kopeerida, eeldades, et andmebaas võib võtta ajutiselt ühenduseta. Tehke seda SQL Server Management Studio objekti Exploreri või samaväärne Transact-SQL-i käskude abil.

1. Lahti andmebaasi source asukohas. Lisateavet leiate teemast [lahti andmebaasi](https://technet.microsoft.com/library/ms191491(v=sql.110).aspx).
2. Windows Exploreris või Windowsi käsuviiba aken, kopeerige eraldiseisva andmebaasi faili või failide ja logifaili või failide Azure SQL serveri VM target asukohta.
3. Kopeeritud failide manustamine target SQL serveri eksemplar. Lisateavet leiate teemast [manustamine andmebaasi](https://technet.microsoft.com/library/ms190209(v=sql.110).aspx).

[Liikumine on andmebaasi abil eemaldada ja manustada (Transact-SQL)](https://technet.microsoft.com/library/ms187858(v=sql.110).aspx)

## <a name="largedbtohive"></a>Stsenaarium \#7: Big data kohaliku failides suunata taru andmebaasi Azure Hdinsightiga Hadoopi rühmades

![Suur andmete kohaliku target taru][9]

#### <a name="additional-azure-resources-azure-hdinsight-hadoop-cluster-and-azure-virtual-machine-ipython-notebook-server"></a>Lisaressursid Azure: Azure Hdinsightiga Hadoopi kobar ja Azure virtuaalse masina (IPython märkmiku server)

1.  Looge Azure virtuaalse masina IPython märkmiku server töötab.

2.  Saate luua ka Azure Hdinsightiga Hadoopi kobar.

3.  (Valikuline) Andmete eelnevalt protsess ja selge.

    lisamine.  Eelnevalt protsessi ja andmete IPython märkmikku, juurdepääsu andmeid Azure plekid puhastada.

    b.  Muuta andmete puhastamiseks, tabelina, vajaduse korral.

    c.  Andmete salvestamiseks VM kohaliku failid (IPython märkmiku töötab VM, kohalikud draivid viidata VM draivid).

4.  Laadi andmed vaikimisi ümbris, Hadoop klaster valitud etappi 2.

5.  Azure Hdinsightiga Hadoopi kobar taru andmebaasi andmete laadimine.

    lisamine.  Hadoopi kobar pea sõlme sisse logida

    b.  Avage Hadoopi käsurea.

    c.  Sisestage taru juurkaust käsuga `cd %hive_home%\bin` Hadoopi käsurea.

    d.  Taru päringute loomine andmebaas ja tabelid ja laadida andmeid bloobimälu taru tabelitele.

    > [AZURE.NOTE] Kui andmed on suur, kasutajad saavad luua taru tabeli sektsioonid. Seejärel kasutajad saavad kasutada mõne `for` tsükkel rakenduses Hadoopi käsurea pea sõlme taru tabeli partition Liigendatud andmete laadimiseks.

6.  Andmete uurimine ja looge funktsioone vastavalt vajadusele Hadoopi käsurea. Pange tähele, et funktsioonid ei pea olema juhtumiga andmebaasitabelites. Märkus ainult nende loomiseks vajalikud päring.

    lisamine.  Hadoopi kobar pea sõlme sisse logida

    b.  Avage Hadoopi käsurea.

    c.  Sisestage taru juurkaust käsuga `cd %hive_home%\bin` Hadoopi käsurea.

    d.  Käivitage taru päringute Hadoopi käsurea Hadoopi kobar andmete uurimine ja luua funktsioonid vastavalt vajadusele pea sõlme.

7.  Kui vaja ja/või soovitud, näidisandmed mahutamiseks Azure seadme õ Studios.

8.  [Azure'i masina Õppekeskuse Studio](https://studio.azureml.net/)sisselogimine.

9. Lugege andmed otse soovitud `Hive Queries` [Andmete importimine] [ import-data] mooduli. Kleepige vajalikud päring, mis ekstraktib väljad, loob funktsioonid ja näidised andmed otse sisse [Andmete importimine] vajadusel[ import-data] päringu.

10. Lihtne Azure seadme õ proovida alustades üleslaaditud andmekomplekti kulgemist.

## <a name="decisiontree"></a>Otsusepuu kuvamine stsenaarium valiku
------------------------

Järgmine diagramm kirjeldab eespool kirjeldatud stsenaariumid ja täiustatud Analytics protsessi ja tehnoloogia valikud teinud, mis viivad teid iga esitate stsenaariumid. Pange tähele, et andmete töötlemise, avastamine, funktsioon matemaatika ja valimite võib kuluda meetod/keskkonnas allikas, vahe, või target keskkonnas – koht ja võivad asuda korduvalt vastavalt vajadusele. Skeemi ainult toimib joonise mõned võimalikud puhul ja sisestage täielik loend.

![Valimi DS protsessi kiirtutvustus stsenaariumid][8]

### <a name="advanced-analytics-in-action-examples"></a>Täiustatud Analytics tegelikkuses näited

Lõpuni Azure seadme õ juhendavad tutvustused kasutavad täiustatud Analytics protsessi ja tehnoloogia avalikest andmekomplektidest abil, vt:


* [Meeskonnatöö andmete teadus protsessi tegelikkuses: SQL serveri kasutamine](machine-learning-data-science-process-sql-walkthrough.md).
* [Meeskonnatöö andmete teadus protsessi tegelikkuses: Hdinsightiga Hadoopi kogumite abil](machine-learning-data-science-process-hive-walkthrough.md).


[1]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-small-in-aml.png
[2]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-with-processing.png
[3]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-large.png
[4]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-to-db.png
[5]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-large-to-db.png
[6]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-db-to-db.png
[7]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-attach-db.png
[8]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-sample-scenarios.png
[9]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-to-hive.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
