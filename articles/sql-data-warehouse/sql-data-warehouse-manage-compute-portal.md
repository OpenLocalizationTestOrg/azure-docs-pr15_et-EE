<properties
   pageTitle="Arvuta power (Azure'i portaal) SQL Azure'i andmebaas haldamine | Microsoft Azure'i"
   description="Azure'i portaali ülesannete haldamiseks Arvutage power. Skaala Arvuta ressursid, reguleerides DWUs. Või Peata ja Jätka arvutada ressursside kulude salvestamiseks."
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
   ms.date="08/22/2016"
   ms.author="barbkess;sonyama"/>

# <a name="manage-compute-power-in-azure-sql-data-warehouse-azure-portal"></a>Arvuta power (Azure'i portaal) SQL Azure'i andmebaas haldamine

> [AZURE.SELECTOR]
- [Ülevaade](sql-data-warehouse-manage-compute-overview.md)
- [Portaal](sql-data-warehouse-manage-compute-portal.md)
- [PowerShelli](sql-data-warehouse-manage-compute-powershell.md)
- [ÜLEJÄÄNUD](sql-data-warehouse-manage-compute-rest-api.md)
- [TSQL](sql-data-warehouse-manage-compute-tsql.md)


Skaala jõudluse skaala läbi arvutada materjale ja mälu muutmisega nõudmistele oma töökoormus. Salvestada kulud skaleerimise tagasi ressursid-tippväärtus korda või otsingurakenduse Arvuta ajal täielikult eemaldada. 

Selle saidikogumi tööülesannete kasutab Azure'i portaalis.

- Arvuta skaala
- Paus Arvuta
- Elulookirjelduse Arvuta

Lisateavet leiate teemast [haldamine arvutada ülevaade][].

<a name="scale-performance-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute-power"></a>Skaala Arvuta power

[AZURE.INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

Arvuta ressursid muutmiseks tehke järgmist.

1. Avage [Azure portaali][], Avage andmebaas ja **klõpsake käsku**.

    ![Klõpsake käsku][1]

1. Skaala tera, nihutage liugurit vasakule või paremale DWU sätet muuta.

    ![Nihutage liugurit][2]

1. Klõpsake nuppu **Salvesta**. Kuvatakse kinnitusteade. Klõpsake nuppu **Jah** kinnitamiseks või **pole** tühistada.

    ![Klõpsake nuppu Salvesta][3]

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Paus Arvuta

[AZURE.INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

Kui soovite Viige andmebaasi.

1. Avage [Azure portaali][] ja Avage andmebaas. Pange tähele, et olek on **Online**. 

    ![Võrgusolek][6]

1. Arvuta ja mälu ressursse peatada, klõpsake nuppu **paus**ja seejärel kinnitusteade kuvatakse. Klõpsake nuppu **Jah** kinnitamiseks või **pole** tühistada.

    ![Kinnitage paus][7]

1. Kui SQL-i andmebaas käivitub andmebaasi, olek on **Peatan**.
2. Kui olek on **peatatud**, Peata toiming on lõpule jõudnud ja te enam ei ostmisega DWUs.

    ![Paus olek][4]

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Elulookirjelduse Arvuta

[AZURE.INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]Kui soovite jätkata andmebaasi:

1. Avage [Azure portaali][] ja Avage andmebaas. Pange tähele, et olek on **peatatud**. 

    ![Paus andmebaas][4]

1. Jätkamiseks klõpsake andmebaasi, **käivitamine**ja seejärel kuvatakse kinnitusteade. Klõpsake nuppu **Jah** kinnitamiseks või **pole** tühistada.

    ![Kinnitage elulookirjeldus][5]

1. Kui SQL-i andmebaas käivitub andmebaasi, olek on "Jätkamine".
2. Kui olek on **võrgus**, andmebaas on valmis.

    ![Võrgusolek][6]

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Järgmised sammud
Lisateavet leiate teemast [halduse ülevaade][].

<!--Image references-->
[1]: ./media/sql-data-warehouse-manage-compute-portal/click-scale.png
[2]: ./media/sql-data-warehouse-manage-compute-portal/move-slider.png
[3]: ./media/sql-data-warehouse-manage-compute-portal/click-save.png
[4]: ./media/sql-data-warehouse-manage-compute-portal/resume-database.png
[5]: ./media/sql-data-warehouse-manage-compute-portal/resume-confirm.png
[6]: ./media/sql-data-warehouse-manage-compute-portal/pause-database.png
[7]: ./media/sql-data-warehouse-manage-compute-portal/pause-confirm.png

<!--Article references-->
[Juhtimise ülevaade]: ./sql-data-warehouse-overview-manage.md
[Arvuta ülevaade haldamine]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->


<!--Other Web references-->

[Azure'i portaal]: http://portal.azure.com/
