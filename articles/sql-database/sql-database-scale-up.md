<properties
    pageTitle="Azure'i SQL-andmebaasiga teenuse taseme- ja jõudluse taseme muutmine | Microsoft Azure'i"
    description="Muuta teenuse kiht ja jõudluse taseme Azure SQL-andmebaasi näitab, kuidas mastaapimiseks SQL-andmebaasi üles või alla. SQL Azure'i andmebaasi hinnakirjad taseme muutmine."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="10/12/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="change-the-service-tier-and-performance-level-pricing-tier-of-a-sql-database-using-the-azure-portal"></a>Teenuse taseme- ja jõudluse taseme muutmine (hinnad taseme) portaalis Azure SQL-andmebaasiga


> [AZURE.SELECTOR]
- [**Azure'i portaal**](sql-database-scale-up.md)
- [PowerShelli](sql-database-scale-up-powershell.md)


Teenuse astme ja jõudlust on kirjeldatud funktsioone ja ressursse, mis on saadaval SQL-andmebaasi ja värskendatakse teie taotlus muutmine vajadustega. Lisateavet leiate teemast [Teenuse astme](sql-database-service-tiers.md).

Pange tähele, et muuta teenuse kiht ja/või jõudlusega andmebaasi algse andmebaasi koopiat loob uue tasemel jõudlus ja aktiveerib seejärel ühendused koopia. Andmed on kadunud seda toimingut, kuid ajal kui me üle minna koopia lühike praegu ühendusi andmebaasi on keelatud, nii, et mõned tehingute flight võib tagasi pöörata. See aken sõltub, kuid on keskmiselt 4-sekundi ja üle 99% juhtudel on vähem kui 30 sekundit. Väga harva, eriti siis, kui seal on suur flight veebisaidil praegu ühendusi tehingute arvud on keelatud, see aken võib olla pikem.  

Kogu skaala üles protsessi kestus sõltub andmebaasi suurus ja teenus taseme enne ja pärast muutmine. Näiteks 250 GB andmebaasi, mis muutub, kaudu, või piires Standard teenuse kiht, tuleb täita 6 tunni jooksul. Andmebaasi sama suur, mis muutub jõudluse taseme Premium teenuse kiht, tuleb täita 3 tunni jooksul.


Kasutage teabe määratlemiseks taseme- ja jõudluse vastav teenusetaseme SQL Azure'i andmebaasi [Azure SQL-i andmebaasi teenuse astme ja jõudlust](sql-database-service-tiers.md) .

- Alandada andmebaasi, andmebaasi peab olema väiksem kui target teenuse kiht lubatud maksimaalmahu. 
- Täiendamisel andmebaasi koos [Geo-Dispersioonanalüüs](sql-database-geo-replication-overview.md) lubatud, tuleb esmalt täiendada selle teisene andmebaaside soovitud jõudluse taseme enne üleminekut esmane andmebaas.
- Kui kasutuselevõttu teenuse kiht, tuleb esmalt lõpetamiseks Geo-Dispersioonanalüüs kõik seosed. 
- Erinevate tasemete seas teenuse jaoks erinevad taastamine teenuse pakkumised. Kui teil on teil kasutuselevõttu võimalus taastamiseks punkti ajas kaotsi minna, või on väiksem varukoopia säilitusperiood. Lisateavet leiate teemast [Azure SQL-i andmebaasi varundamine ja taastamine](sql-database-business-continuity.md).
- Andmebaasi hinnad taseme muutmine ei muuda andmebaasi maksimaalne maht. Andmebaasi maksimaalne maht saate muuta [Transact-SQL-i (T-SQL-i)](https://msdn.microsoft.com/library/mt574871.aspx) või [PowerShelli](https://msdn.microsoft.com/library/mt619433.aspx).
- Uue andmebaasi atribuutide ei rakendata seni, kuni muudatused on lõpule viidud.



**Selles artiklis lõpuleviimiseks vajate järgmist:**

- Azure'i SQL-andmebaasiga. Kui te ei ole SQL-andmebaasi, luua ühe selles artiklis toodud juhiseid järgides: [esimese Azure SQL-i andmebaasi loomine](sql-database-get-started.md).


## <a name="change-the-service-tier-and-performance-level-of-your-database"></a>Andmebaasi teenuse taseme- ja jõudluse taseme muutmine


Avage SQL-andmebaasi tera andmebaasi, mida soovite mastaapimiseks üles või alla.

1.  [Azure'i portaal](https://portal.azure.com), klõpsake nuppu **rohkem teenuseid** > **SQL andmebaase**.
2.  Klõpsake andmebaasi, mida soovite muuta.
3.  Klõpsake **SQL-andmebaasi** enne **hinnakirjad taseme (skaala DTUs)**.

    ![esimese taseme hinnad][1]

1.  Valige uue taseme ja klõpsake nuppu **Vali**.

    **Valige** klõpsates esitab skaala muutmine hinnakirjad taseme. Olenevalt teie andmebaasi mahtu skaala toiming võib võtta aega täieliku (vt käesoleva artikli ülaosas olevat teavet).

    > [AZURE.NOTE] Andmebaasi hinnad taseme muutmine ei muuda andmebaasi maksimaalne maht. Andmebaasi maksimaalne maht saate muuta [Transact-SQL-i (T-SQL-i)](https://msdn.microsoft.com/library/mt574871.aspx) või [PowerShelli](https://msdn.microsoft.com/library/mt619433.aspx).

    ![Valige hinnakirjad taseme][2]

3.  Klõpsake paremas ülanurgas teavitusikooni (Gaussi).

    ![teatised][3]

    Klõpsake teate teksti avamiseks klõpsake üksikasjade paanil näete taotluse olekut.




## <a name="next-steps"></a>Järgmised sammud

- Saate muuta oma andmebaasi maksimaalne maht [Transact-SQL-i (T-SQL-i)](https://msdn.microsoft.com/library/mt574871.aspx) või [PowerShelli](https://msdn.microsoft.com/library/mt619433.aspx)abil.
- [Skaala ja](sql-database-elastic-scale-get-started.md)
- [Ühenduse loomine ja päringu SQL-i andmebaasi SSMS](sql-database-connect-query-ssms.md)
- [Ekspordi Azure'i SQL-andmebaasiga](sql-database-export.md)

## <a name="additional-resources"></a>Lisaressursid

- [Äritegevuse järjepidevuse ülevaade](sql-database-business-continuity.md)
- [SQL-andmebaasi dokumentatsioon](https://azure.microsoft.com/documentation/services/sql-database/)


<!--Image references-->
[1]: ./media/sql-database-scale-up/new-tier.png
[2]: ./media/sql-database-scale-up/choose-tier.png
[3]: ./media/sql-database-scale-up/scale-notification.png
[4]: ./media/sql-database-scale-up/new-tier.png
