<properties
   pageTitle="Redgate isiku andmete platvormi Studio abil andmete laadimine SQL-i andmebaas | Microsoft Azure'i"
   description="Saate teada, kuidas kasutada andmete tolliladustamise stsenaariumid Redgate isiku andmete platvormi Studio."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="twounder"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="10/13/2016"
   ms.author="mausher;barbkess"/>


# <a name="load-data-with-redgate-data-platform-studio"></a>Redgate andmete platvormi Studio andmete laadimine

> [AZURE.SELECTOR]
- [Redgate](sql-data-warehouse-load-with-redgate.md)
- [Andmete Factory](sql-data-warehouse-get-started-load-with-azure-data-factory.md)
- [PolyBase](sql-data-warehouse-get-started-load-with-polybase.md)
- [BCP](sql-data-warehouse-load-with-bcp.md)

Selle õpetuse näidatakse, kuidas kasutada [Redgate isiku andmete platvormi Studio](http://www.red-gate.com/products/azure-development/data-platform-studio/) (DPS) kohapealne SQL Serverist andmete teisaldamine SQL Azure'i andmebaas. Andmete platvormi Studio kehtib sobivaima parandusi ja Optimeerimised, nii, et see on kõige kiiremini alustada SQL-i andmebaas.

> [AZURE.NOTE] [Redgate](http://www.red-gate.com) on pikaajaline Microsoft partner, mis pakub erinevaid SQL serveri tööriistu. See funktsioon andmete platvormi Studios on tehtud vabalt trükikojas ja mitteäriliseks kasutamiseks saadaval.

## <a name="before-you-begin"></a>Enne alustamist
### <a name="create-or-identify-resources"></a>Looge või ressursside tuvastamine

Selles õpetuses enne peab teil olema:

- **Kohapealne SQL Serveri andmebaas**: andmed, mida soovite importida SQL-i andmebaas peab olema asutusesisese SQL serverist (versioon 2008R2 või rohkem). Andmete platvormi Studio ei saa importida andmed otse Azure'i SQL-andmebaasi või tekstifailidest.
- **Azure Storage konto**: andmete platvormi Studio etapid Azure'i bloobimälu andmed enne laadimine SQL-i andmebaas. Salvestusruumi konto peate kasutama "ressursihaldur" juurutamise mudeli (vaikimisi) asemel "Classic" juurutamise mudel. Kui teil pole salvestusruumi konto, saate teada, kuidas luua konto salvestusruumi. 
- **SQL-i andmebaas**: selles õpetuses liigub andmed kohapealne SQL serveri SQL-i andmebaas, seega peab teil olema võrgus andmebaas. Kui teil on juba andmebaas, saate teada, kuidas luua Azure SQL-i andmebaas.

> [AZURE.NOTE] Jõudlust on täiustatud kui salvestusruumi konto ja andmebaas on loodud piirkonna.

## <a name="step-1-sign-in-to-data-platform-studio-with-your-azure-account"></a>Samm 1: Andmete platvormi Studio Azure kontoga sisselogimine
Avage veebibrauser ja liikuge [Andmete platvormi Studio](https://www.dataplatformstudio.com/) veebisaidile. Logige sisse sama Azure'i konto salvestusruumi konto ja andmete ladu loomiseks kasutatud. Kui teie meiliaadress on seostatud nii töö või kooli konto ja Microsofti konto, kindlasti valige konto, kellel on juurdepääs teie ressursid.

> [AZURE.NOTE] Kui kasutate andmete platvormi Studio esimest korda, palutakse teil hallata oma Azure ressursse rakenduse loa.

## <a name="step-2-start-the-import-wizard"></a>Samm 2: Käivitada
Valige põhikuval DPS importimine SQL Azure'i andmebaas link impordi viisardi käivitamiseks.

![][1]

## <a name="step-3-install-the-data-platform-studio-gateway"></a>Samm 3: Installida andmete platvormi Studio lüüsi
Kohapealne SQL Serveri andmebaasiga ühenduse peate installima DPS lüüsi. Lüüs on kliendi agent, pääsete juurde oma asutusesiseses keskkonnas, ekstraktib andmed ja seejärel lisatud see konto salvestusruumi. Andmete kunagi läbib Redgate's serverites. Lüüsi installimiseks tehke järgmist.

1.  Klõpsake linki **Lüüsi loomine**
2. Laadige alla ja installige lüüsi esitatud Installeri abil

![][2]

> [AZURE.NOTE] Lüüsi saab installida mis tahes seadme, võrgu juurdepääsu allika SQL serveri andmebaasi. See pääseb SQL Serveri andmebaasiga Windowsi autentimist kasutades praeguse kasutaja mandaat.

Kui installitud, et lüüsi olek muutub ühendatud ja valige edasi.

## <a name="step-4-identify-the-source-database"></a>Samm 4: Tuvastamine allika andmebaasi
Sisestage väljale *Sisestage serveri nimi* nimi serveris, mis majutab andmebaasi ja valige **edasi**. Seejärel valige rippmenüüst menüü andmebaasi imporditavaid andmeid.

![][3]

DPS uurib valitud andmebaasi tabelid importida. DPS impordib vaikimisi andmebaasis kõik tabelid. Saate märkige või tühjendage ruut tabelite laiendab kõik tabelid link. Valige edasiliikumiseks nuppu edasi.

## <a name="step-5-choose-a-storage-account-to-stage-the-data"></a>Juhis 5: Salvestusruumi konto etapil andmete valimine
DPS palub teil jaoks asukoht, kuhu soovite andmed etapp. Olemasoleva salvestusruumi konto tellimusest ja valige **edasi**.

> [AZURE.NOTE] DPS loob uue bloobimälu container valitud salvestusruumi konto ja iga importimiseks kasutada erinevate kausta.

![][4]

## <a name="step-6-select-a-data-warehouse"></a>Samm 6: Valige andmebaas
Järgmisena saate valida võrgus [SQL Azure'i andmebaas](http://aka.ms/sqldw) andmebaasi importida andmeid. Kui olete valinud andmebaasi, peate sisestama identimisteabe andmebaasiga ühenduse loomiseks ja valige **edasi**.

![][5]

> [AZURE.NOTE] DPS ühendab allika andmetabelite andmebaas. DPS hoiatab, kui tabeli nimi on vaja see andmebaas olemasoleva tabelid kirjutada. Võite soovi korral kustutada kõik olemasolevad objektide andmebaas märgistades Kustuta kõik olemasolevad objektid enne impordi.

## <a name="step-7-import-the-data"></a>Juhis 7: Andmed importida.
DPS kinnitab, et soovite andmed importida. Klõpsake lihtsalt andmete importimise alustamiseks nuppu Start importimine.

![][6]

DPS kuvab visualiseeringu edenemist ja üleslaadimise kohapealne SQL serveri ja edenemise SQL-i andmebaas importida andmed.

![][7]

Kui importimine on lõpule jõudnud, kuvatakse DPS andmete importimise kokkuvõte ja ühilduvus parandusi, mis on tehtud muudatuse aruande.

![][8]

## <a name="next-steps"></a>Järgmised sammud
SQL-i andmebaas oma andmeid uurima käivitamiseks kuvamine.

- [Päringu SQL Azure'i andmebaas (Visual Studio)][]
- [Power BI andmete visualiseerimine][]

Lisateavet Redgate isiku andmete platvormi Studio:

- [Külastage DPS Avaleht](http://www.dataplatformstudio.com/)
- [Vaadake demo DPS Channel9 kohta](https://channel9.msdn.com/Blogs/cloud-with-a-silver-lining/Loading-data-into-Azure-SQL-Datawarehouse-with-Redgate-Data-Platform-Studio)

Muud võimalused migreerimine ja SQL-i andmebaas andmete laadimine ülevaate leiate.

- [Teie lahendus migreerimine SQL-andmebaas][]
- [Andmete laadimine SQL Azure'i andmebaas](./sql-data-warehouse-overview-load.md)

Lisateavet arengu leiate näpunäiteid teemast [SQL-i andmebaas arengu ülevaade](./sql-data-warehouse-overview-develop.md).

<!--Image references-->
[1]: media/sql-data-warehouse-redgate/2016-10-05_15-59-56.png
[2]: media/sql-data-warehouse-redgate/2016-10-05_11-16-07.png
[3]: media/sql-data-warehouse-redgate/2016-10-05_11-17-46.png
[4]: media/sql-data-warehouse-redgate/2016-10-05_11-20-41.png
[5]: media/sql-data-warehouse-redgate/2016-10-05_11-31-24.png
[6]: media/sql-data-warehouse-redgate/2016-10-05_11-32-20.png
[7]: media/sql-data-warehouse-redgate/2016-10-05_11-49-53.png
[8]: media/sql-data-warehouse-redgate/2016-10-05_12-57-10.png

<!--Article references-->
[Päringu SQL Azure'i andmebaas (Visual Studio)]: ./sql-data-warehouse-query-visual-studio.md
[Power BI andmete visualiseerimine]: ./sql-data-warehouse-get-started-visualize-with-power-bi.md
[Teie lahendus migreerimine SQL-andmebaas]: ./sql-data-warehouse-overview-migrate.md
[Load data into Azure SQL Data Warehouse]: ./sql-data-warehouse-overview-load.md
[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md