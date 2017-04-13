<properties 
   pageTitle="Arendamise U-SQL-i kasutaja määratletud tehtemärgid Azure'i andmeanalüüsi Lake töökohta | Azure'i" 
   description="Saate teada, kuidas luua kasutaja määratletud tehtemärke kasutada ning taaskasutada Lake andmeanalüüsi tööde haldamine. " 
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


# <a name="develop-u-sql-user-defined-operators-for-azure-data-lake-analytics-jobs"></a>Arendada Azure'i andmeanalüüsi Lake tööde U-SQL-i kasutaja määratletud tehtemärgid

Saate teada, kuidas luua kasutaja määratletud tehtemärke kasutada ning taaskasutada Lake andmeanalüüsi tööde haldamine. Saate välja kohandatud tehtemärk teisendada riikide nimed.

##<a name="prerequisites"></a>Eeltingimused

- Visual Studio 2015, Visual Studio 2013 update 4 või Visual Studio 2012 ja Visual C++ installitud 
- Microsoft Azure'i SDK .net-i versiooni 2.5 või kohale.  Installige see Web platvormi Installeri abil.
- Andmeanalüüsi Lake konto.  Teemast [Azure Lake andmeanalüüsi Azure'i portaalis alustamine](data-lake-analytics-get-started-portal.md).
- Õpetusega [Alustamine Azure'i andmete Lake Analytics U-SQL Studio](data-lake-analytics-u-sql-get-started.md) läbida.
- Azure'i ühendada, leiate [Azure'i andmete Lake Analytics U-SQL Studio alustamine](data-lake-analytics-u-sql-get-started.md#connect-to-azure). 
- Lähteandmete üles laadida, leiate [Azure'i andmete Lake Analytics U-SQL Studio alustamine](data-lake-analytics-u-sql-get-started.md#upload-source-data-files). 

## <a name="define-and-use-user-defined-operator-in-u-sql"></a>Ülesande määratlemine ja kasutaja määratletud tehtemärk U-SQL-i kasutamine

**Luua ja esitada U-SQL-i töö** 

1. Klõpsake menüü **fail** nuppu **Uus**ja seejärel klõpsake nuppu **projekti**.
2. Valige **U-SQL-i projekti** tüüp.

    ![uue U-SQL-i Visual Studio projekti](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-new-project.png)

3. Klõpsake nuppu **OK**. Visual studio loob lahenduse Script.usql faili abil.
4. **Solution Exploreris**, laiendage Script.usql ja seejärel topeltklõpsake **Script.usql.cs**.
5. Kleepige fail järgmine kood:

        using Microsoft.Analytics.Interfaces;
        using System.Collections.Generic;
        
        namespace USQL_UDO
        {
            public class CountryName : IProcessor
            {
                private static IDictionary<string, string> CountryTranslation = new Dictionary<string, string>
                {
                    {
                        "Deutschland", "Germany"
                    },
                    {
                        "Schwiiz", "Switzerland"
                    },
                    {
                        "UK", "United Kingdom"
                    },
                    {
                        "USA", "United States of America"
                    },
                    {
                        "中国", "PR China"
                    }
                };
        
                public override IRow Process(IRow input, IUpdatableRow output)
                {
        
                    string UserID = input.Get<string>("UserID");
                    string Name = input.Get<string>("Name");
                    string Address = input.Get<string>("Address");
                    string City = input.Get<string>("City");
                    string State = input.Get<string>("State");
                    string PostalCode = input.Get<string>("PostalCode");
                    string Country = input.Get<string>("Country");
                    string Phone = input.Get<string>("Phone");
        
                    if (CountryTranslation.Keys.Contains(Country))
                    {
                        Country = CountryTranslation[Country];
                    }
                    output.Set<string>(0, UserID);
                    output.Set<string>(1, Name);
                    output.Set<string>(2, Address);
                    output.Set<string>(3, City);
                    output.Set<string>(4, State);
                    output.Set<string>(5, PostalCode);
                    output.Set<string>(6, Country);
                    output.Set<string>(7, Phone);
        
                    return output.AsReadOnly();
                }
            }
        }

5. Avage Script.usql ja järgmine U-SQL kirjutuskaitstuks.

        @drivers =
            EXTRACT UserID      string,
                    Name        string,
                    Address     string,
                    City        string,
                    State       string,
                    PostalCode  string,
                    Country     string,
                    Phone       string
            FROM "/Samples/Data/AmbulanceData/Drivers.txt"
            USING Extractors.Tsv(Encoding.Unicode);
        
        @drivers_CountryName =
            PROCESS @drivers
            PRODUCE UserID string,
                    Name string,
                    Address string,
                    City string,
                    State string,
                    PostalCode string,
                    Country string,
                    Phone string
            USING new USQL_UDO.CountryName();    
        
        OUTPUT @drivers_CountryName
            TO "/Samples/Outputs/Drivers.csv"
            USING Outputters.Csv(Encoding.Unicode);

6. **Lahenduste Explorer**, paremklõpsake **Script.usql**ja klõpsake **Skripti koostamine**.
6. **Lahenduste Explorer**, paremklõpsake **Script.usql**ja klõpsake **Skripti esitada**.
7. Kui te pole ühenduse Azure tellimuse, saab kiiresti sisestada mandaat Azure'i konto.
7. Klõpsake **esitada**. Esitamise tulemused ja töö link on saadaval aknas tulemused kui esitamise on lõpule viidud.
8. Peate klõpsama menüüd uusima töö oleku kuvamine ja kuva värskendamiseks nuppu Värskenda.

**Töö väljundi kuvamiseks**

1. **Server Explorer**, laiendamine **Azure**, laiendamine **Lake andmeanalüüsi**, laiendamine Lake andmeanalüüsi kontole, laiendage **Salvestusruumi kontod**, paremklõpsake vaikimisi salvestusruumi, ja klõpsake **Explorer**. 
2. Laiendage näidised, väljundeid laiendamine ja seejärel topeltklõpsake **Drivers.csv**.


##<a name="see-also"></a>Vt ka

- [Alustamine Lake andmeanalüüsi PowerShelli abil](data-lake-analytics-get-started-powershell.md)
- [Azure'i portaalis Lake andmeanalüüsi kasutamise alustamine](data-lake-analytics-get-started-portal.md)
- [Visual Studio U-SQL-i rakenduste arendamise kasutamine andmete Lake tööriistad](data-lake-analytics-data-lake-tools-get-started.md)
