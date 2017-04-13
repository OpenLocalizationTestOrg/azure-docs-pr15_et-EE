<properties
   pageTitle="Andmete ladu töökoormus"
   description="SQL-i andmebaas elastsus võimaldab kasvada, Kahanda või viige Arvutage power libiseva skaala andmete ladu üksuste (DWUs) abil. Selles artiklis selgitatakse andmete ladu mõõdikute ja kuidas need on seotud DWUs. "
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
   ms.date="07/25/2016"
   ms.author="barbkess;mausher;jrj;sonyama"/>


# <a name="data-warehouse-workload"></a>Andmete ladu töökoormus
Andmete ladu töökoormus viitab kõiki toiminguid, mis vastu andmebaas. Andmete ladu töökoormus hõlmab kogu protsessi andmete laadimine ladu, analüüs ja aruandluse andmebaas, andmebaas andmete haldamise ja andmete eksportimine andmebaas. Soovitud sügavus ja nende komponentide sageli vastavuses tähtaeg tase andmebaas.


## <a name="new-to-data-warehousing"></a>Uus võrgupõhised andmebaasid?
Andmebaas on andmeid, mis on ühe või mitme andmeallikatest koormatud ja saab teha ärianalüüsi toiminguid nagu aruandlus ja andmete analüüs.

Andmete ladu iseloomustab päringud, mis skannida suuremate arvude andmete suurte vahemike, ridade ja võib tagastada suhteliselt tulemusi analüüsi ja aruandluse jaoks. Andmete ladu iseloomustab suhteliselt andmete laadimise ja small tehingu taseme lisab/värskenduste/kustutab.

- Andmebaas teostab kõige paremini, kui andmed on salvestatud nii, et optimeerib päringud, mis on vaja suure hulga read või suurte vahemike andmed. Seda tüüpi skannimine toimib kõige paremini, kui andmed on salvestatud ja ridade asemel veerge, mida otsida.

>[AZURE.NOTE] -Mälu columnstore register, mis kasutab veeru salvestusruumi, pakub kuni 10 x tihendamise kasumite ja 100 x päringu jõudluse tõstmiseks üle traditsiooniline kahendarvu puude aruandlust ja analüüsi päringud. Me lugeda columnstore registrite standard talletamine ja skannimine suurte andmete andmebaas.

- Andmebaas on erinevad nõuded kui süsteem, mis mahub online tehingu töötlemise (OLTP). OLTP süsteemi on palju lisada, värskendada ja kustutada. Neid toiminguid püüdma kindlate ridade tabelis. Tabeli eesmärk on kõige paremini teha, kui andmed on salvestatud-ridahaaval viisil. Andmeid saab sortida ja kiiresti otsida ja jagage ja vallutada lähenemine nimetatakse kahendarvu puu või btree otsingu.


## <a name="data-loading"></a>Andmete laadimine
Andmete laadimine on suur osa andmete ladu töökoormus. Ettevõtted on tavaliselt hõivatud OLTP süsteem, mis jälgib kui kliendid on business kannete loomine päeva jooksul. Aeg-ajalt sageli öösel hoolduse ajal, tehingud on teisaldatud või kopeeritud andmebaas. Kui andmebaas on andmeid, saate ärianalüütikud analüüsi ja teha äriotsuseid andmeid.

- Traditsiooniliselt laadimise protsessi nimetatakse ETL ekstrakti, transformatsioon ja laadi. Andmed tuleb tavaliselt ümber nii, et see vastab muudest andmetest andmebaas. Varem ettevõtetele kasutada spetsiaalne ETL serverite teha muutusi. Nüüd koos kiire oluliselt paralleelselt töötlemise saate andmete laadimine esmalt SQL-i andmebaas ja tehke soovitud muudatused. Selle protsessi nimetatakse ekstrakti, laadi ja transformatsioon (ELT) ja muutub uus standard andmete ladu töökoormus.

> [AZURE.NOTE] SQL Server 2016, saate nüüd teha analytics rakenduses reaalajas mõne OLTP tabeli. See ei asenda vajadust andmete talletamiseks ja andmebaas, kuid see võimalda teha analüüse reaalajas.

### <a name="reporting-and-analysis-queries"></a>Aruandlust ja analüüsi päringud
Aruandlust ja analüüsi päringud on sageli liigitada väike, Keskmine ja suur mitme kriteeriumi põhjal, kuid tavaliselt see põhineb aeg. Enamik andmete laos, on kombineeritud töökoormus kiire-töötab ja päringute kohta. Korral on oluline, et see mix määratlemiseks ja selle sagedus määratlemiseks (tunni iga päev, kuu lõpu kvartali lõpu jne). On oluline, et aru saada, et kombineeritud päringu töökoormus, koos kokkulangevus, viia õige võimsus kavandamise andmebaas.

- Võimsuse planeerimine võib olla keeruka jaoks kombineeritud päringu töökoormus, eriti siis, kui teil on vaja lisada võimsus andmebaas pikk täitmisaeg. SQL-i andmebaas eemaldab kiiresti võimsus planeerimine, kuna kasvada ja vähenda Arvuta võimsus igal ajal ja salvestusruumi ja arvutada võimsus on sõltumatult suurusega.

### <a name="data-management"></a>Andmehaldus
Andmete haldamine on oluline, eriti siis, kui te ei tea, võib piisavalt kettaruumi lähitulevikus. Andmete ladu jagada andmeid tavaliselt mõtestatud vahemikud, mis salvestatakse sektsioonid tabelis. SQL Serveri põhises toodete abil saate liikuda partitsioonid ja sealt tabel. Selle sektsiooni vahetamine võimaldab teil vanemate andmete teisaldamiseks väiksemate salvestusruumi ja hoidke uusimaid andmeid, mis on saadaval salvestusruum võrgus.

- Columnstore registrite toetavad piiretega tabelid. Columnstore registrite, kasutatakse sektsioonitud tabeli andmete haldamiseks ja arhiivimine. Tabelid, mis on talletatud--ridahaaval, sektsioonid olema suurema rolliga päringu jõudlust.  

- PolyBase on oluline roll andmete haldamine. PolyBase kasutamisel on teil võimalus arhiivida vanemad andmete Hadoopi või Azure'i bloobimälu.  See pakub mitmesuguseid võimalusi, kuna andmed on endiselt veebis.  See võib kuluda andmete toomiseks Hadoopi, kuid otsing aja Miinuseks võib suuremad salvestusruumi kulud.

### <a name="exporting-data"></a>Andmete eksportimine
Üks viis aruannete ja analüüsi jaoks kättesaadavaks andmed on saata andmeid andmebaas serverid asjakohast aruannete ja analüüsi töötab. Järgmistes serverites nimetatakse andmete marts. Näiteks võib eelnevalt aruande andmete töötlemiseks ja seejärel eksportida see andmebaas paljud serverid kogu maailmas, et kliendid ja ärianalüütikud laiemalt kättesaadavaks teha.

- Aruannete loomise kõigi teil võib asustada kirjutuskaitstud Aruandlusteenuste serverid igapäevane andmete hetktõmmist. See annab suurema läbilaskevõime klientide samal ajal vähendada Arvuta ressursi vajadused andmebaas. Kaudu turvalisus proportsioonid, andmete marts võimaldavad kasutajal on Accessi andmebaas arvu vähendamiseks.
- Analyticsi, võite koostamiseks klõpsake andmebaas on analüüsiteenuste kuubis ja analüüsi vastuolus andmebaas, või eelnevalt töödelda ja eksportimine analüüsiteenuste serveri põhjalikumaks Analyticsi.

## <a name="next-steps"></a>Järgmised sammud
Nüüd, kui teate veidi SQL-i andmebaas kohta, lugege kiiresti [luua SQL-i andmebaas][] ja [laadimine näidisandmeid][].

<!--Image references-->

<!--Article references-->
[Laadi näidisandmed]: ./sql-data-warehouse-load-sample-databases.md
[Looge SQL-i andmebaas]: ./sql-data-warehouse-get-started-provision.md

<!--MSDN references-->

<!--Other web references-->
