<properties
   pageTitle="Azure'i andmed Lake Analytics U-SQL-i kataloogi tutvustamine | Azure'i"
   description="Azure'i andmed Lake Analytics U-SQL-i kataloogi tutvustamine"
   services="data-lake-analytics"
   documentationCenter=""
   authors="edmacauley"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="use-u-sql-catalog"></a>Kasutage U-SQL-i kataloogi

A-SQL-i kataloogi kasutatakse struktureerimine andmed ja koodi nii, et ta saab jagada U-SQL-i skripte. Kataloogi võimaldab võimalike andmetega Azure'i andmed Lake kõrgeim jõudlus.

Iga Azure'i andmeanalüüsi Lake konto on täpselt ühe U-SQL-i kataloogi seotud. Kustutada ei saa A-SQL-i kataloogi. Praegu ei saa A-SQL-i kataloogid ühiselt Lake andmesalve kontod.

Iga U-SQL-i kataloogi sisaldab andmebaasi, mida nimetatakse **juhtslaidi**. Juhtslaidi andmebaasi ei saa kustutada.  Iga U-SQL-i kataloogi võib sisaldada rohkem uute andmebaaside.

A-SQL-andmebaas sisaldab:

- Assemblereid – .net-i kood vahel U-SQL-i skriptide ühiskasutusse anda.
- Tabeli väärtusi funktsioonid – A-SQL-koodi vahel U-SQL-i skriptide ühiskasutusse anda.
- Tabelite – ühiskasutus andmeid U-SQL-i skriptide vahel.
- Skeemide - tabeli skeemid vahel U-SQL-i skriptide ühiskasutusse anda.

## <a name="manage-catalogs"></a>Kataloogid haldamine
Iga Azure'i andmeanalüüsi Lake konto on seostatud vaikekonto Azure'i andmesalve Lake. Selle konto Lake andmesalve nimetatakse Lake andmesalve konto. A-SQL-i kataloogi talletatakse kaustas /catalog Lake andmesalve vaikekonto. Kustutage kõik failid kaustast /catalog.

### <a name="use-azure-portal"></a>Azure'i portaali kasutamine

Vaadake [haldamine Lake andmeanalüüsi kasutamine portaal](data-lake-analytics-manage-use-portal.md#view-u-sql-catalog)


### <a name="use-data-lake-tools-for-visual-studio"></a>Kasutage andmete Lake tööriistad Visual Studio.

Andmete Lake Tools for Visual Studio abil saate hallata kataloogi.  Tööriistade kohta leiate lisateavet teemast [Andmete Lake Tools for Visual Studio abil](data-lake-analytics-data-lake-tools-get-started.md).

**Kataloogi haldamine**

1. Avage Visual Studio ja azure ühenduse. Juhised leiate teemast [ühenduse loomine Azure](data-lake-analytics-data-lake-tools-get-started.md#connect-to-azure).
1. **Server Explorer** avamiseks vajutage **Klahvikombinatsiooni CTRL + ALT + S**.
2. **Server Explorer**, laiendamine **Azure**, laiendamine **Lake andmeanalüüsi**laiendamine Lake andmeanalüüsi konto, Laienda **andmebaase**ja seejärel laiendage **juhtslaidi**.



    - Kui soovite lisada uue andmebaasi, paremklõpsake **andmebaasi**ja klõpsake **Andmebaasi loomine**.
    - Uue komplekti lisamiseks paremklõpsake **assemblereid**ja seejärel klõpsake nuppu **Registreeri komplekti**.
    - Uue skeemi lisamiseks paremklõpsake **skeemid**, ja klõpsake "Loo skeemi **.
    - Uue tabeli lisamiseks paremklõpsake **tabeleid**, ja valige "" loomine tabeli **.
    - Uue tabeli hinnatud funktsiooni lisada, lugege teemat [arendada U-SQL-i kasutaja määratletud Lake andmeanalüüsi tööde tehtemärke](data-lake-analytics-u-sql-develop-user-defined-operators.md).


![Sirvige U-SQL-i Visual Studio kataloogid](./media/data-lake-analytics-use-u-sql-catalog/data-lake-analytics-browse-catalogs.png)


## <a name="see-also"></a>Vt ka

- Alustamine
    - [Azure'i portaalis Lake andmeanalüüsi kasutamise alustamine](data-lake-analytics-get-started-portal.md)
    - [Alustamine Lake andmeanalüüsi Azure PowerShelli abil](data-lake-analytics-get-started-powershell.md)
    - [Lake andmeanalüüsi Azure'i .NET SDK kasutamise alustamine](data-lake-analytics-get-started-net-sdk.md)
    - [Arendamise U-SQL-i skriptide abil andmete Lake tööriistad Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
    - [Azure'i andmed Lake Analytics U-SQL-i keele kasutamise alustamine](data-lake-analytics-u-sql-get-started.md)

- A-SQL- ja arendustegevus
    - [Azure'i andmed Lake Analytics U-SQL-i keele kasutamise alustamine](data-lake-analytics-u-sql-get-started.md)
    - [Azure'i andmeanalüüsi Lake projektide jaoks kasutada U-SQL-i akna funktsioonid](data-lake-analytics-use-window-functions.md)
    - [Arendamise U-SQL-i kasutaja määratletud tehtemärgid Lake andmeanalüüsi projektide jaoks](data-lake-analytics-u-sql-develop-user-defined-operators.md)

- Haldus
    - [Azure'i Lake andmeanalüüsi Azure'i portaalis haldamine](data-lake-analytics-manage-use-portal.md)
    - [Azure'i Lake andmeanalüüsi Azure PowerShelli kaudu hallata](data-lake-analytics-manage-use-powershell.md)
    - [Jälgimine ja Azure andmeanalüüsi Lake töö Azure'i portaalis tõrkeotsing](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)

- Õppeteema – lõpuni
    - [Kasutage Azure'i andmeanalüüsi Lake interaktiivsed õppematerjalid](data-lake-analytics-use-interactive-tutorials.md)
    - [Veebisaidi logid abil Azure'i andmeanalüüsi Lake analüüs](data-lake-analytics-analyze-weblogs.md)
