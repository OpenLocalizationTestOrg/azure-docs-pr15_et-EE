<properties 
    pageTitle="Muuta teenuse taseme- ja jõudluse PowerShelli kaudu Azure SQL-andmebaasi | Microsoft Azure'i" 
    description="Muuta teenuse kiht ja jõudluse taseme Azure SQL-andmebaasi näitab, kuidas mastaapimiseks PowerShelliga SQL-andmebaasi üles või alla. SQL Azure'i andmebaasi PowerShelliga hinnakirjad taseme muutmine." 
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


# <a name="change-the-service-tier-and-performance-level-pricing-tier-of-a-sql-database-with-powershell"></a>Teenuse taseme- ja jõudluse taseme muutmine (hinnad taseme) SQL-andmebaasi PowerShelliga


> [AZURE.SELECTOR]
- [Azure'i portaal](sql-database-scale-up.md)
- [**PowerShelli**](sql-database-scale-up-powershell.md)


Teenuse astme ja jõudlust on kirjeldatud funktsioone ja ressursse, mis on saadaval SQL-andmebaasi ja värskendatakse teie taotlus muutmine vajadustega. Lisateavet leiate teemast [Teenuse astme](sql-database-service-tiers.md).

Pange tähele, et muuta teenuse kiht ja/või jõudlusega andmebaasi algse andmebaasi koopiat loob uue tasemel jõudlus ja aktiveerib seejärel ühendused koopia. Andmed on kadunud selle käigus, kuid lühike hetkel kui me üle minna koopia, ühendused andmebaasi on keelatud, nii, et mõned tehingute flight võib tagasi pöörata. See aken sõltub, kuid on keskmiselt 4-sekundi ja üle 99% juhtudel on vähem kui 30 sekundit. Väga harva, eriti siis, kui seal on suur flight veebisaidil praegu ühendusi tehingute arvud on keelatud, see aken võib olla pikem.  

Kogu skaala üles protsessi kestus sõltub andmebaasi suurus ja teenus taseme enne ja pärast muutmine. Näiteks 250 GB andmebaasi, mis muutub, kaudu, või piires Standard teenuse kiht, tuleb täita 6 tunni jooksul. Andmebaasi sama suur, mis muutub jõudluse taseme Premium teenuse kiht, tuleb täita 3 tunni jooksul.


- Alandada andmebaasi, andmebaasi peab olema väiksem kui target teenuse kiht lubatud maksimaalmahu. 
- Täiendamisel andmebaasi koos [Geo-Dispersioonanalüüs](sql-database-geo-replication-portal.md) lubatud, tuleb esmalt täiendada selle teisene andmebaaside soovitud jõudluse taseme enne üleminekut esmane andmebaas.
- Kui allavahetamine Premium teenuse kiht, te peate esmalt lõpetada Geo-Dispersioonanalüüs kõik seosed. Te saate [taastada ka katkestuste](sql-database-disaster-recovery.md) dispersioonanalüüs protsessi esmast ja aktiivne teisene andmebaaside vahel teemas kirjeldatud juhised.
- Erinevate tasemete seas teenuse jaoks erinevad taastamine teenuse pakkumised. Kui teil on teil kasutuselevõttu võimalus taastamiseks punkti ajas kaotsi minna, või on väiksem varukoopia säilitusperiood. Lisateavet leiate teemast [Azure SQL-i andmebaasi varundamine ja taastamine](sql-database-business-continuity.md).
- Uue andmebaasi atribuutide ei rakendata seni, kuni muudatused on lõpule viidud.



**Selles artiklis lõpuleviimiseks vajate järgmist:**

- Azure'i tellimuse. Kui teil on vaja Azure tellimuse lihtsalt **Tasuta konto** selle lehe ülaosas nuppu ja siis naaske artiklit lõpuleviimiseks.
- Azure'i SQL-andmebaasiga. Kui te ei ole SQL-andmebaasi, luua ühe selles artiklis toodud juhiseid järgides: [esimese Azure SQL-i andmebaasi loomine](sql-database-get-started.md).
- Azure'i PowerShelli.


[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]



## <a name="change-the-service-tier-and-performance-level-of-your-sql-database"></a>SQL-andmebaasi teenuse taseme- ja jõudluse taseme muutmine

Käivitage [Set-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt619433(v=azure.300\).aspx) cmdlet ja set **-RequestedServiceObjectiveName** jõudluse tasemele soovitud hinnakirjad tasandi; näiteks *S0*, *S1*, *S2*, *S3*, *P1*, *P2*,...

```
$ResourceGroupName = "resourceGroupName"
    
$ServerName = "serverName"
$DatabaseName = "databaseName"

$NewEdition = "Standard"
$NewPricingTier = "S2"

Set-AzureRmSqlDatabase -DatabaseName $DatabaseName -ServerName $ServerName -ResourceGroupName $ResourceGroupName -Edition $NewEdition -RequestedServiceObjectiveName $NewPricingTier
```

  

   


## <a name="sample-powershell-script-to-change-the-service-tier-and-performance-level-of-your-sql-database"></a>Proovi PowerShelli skripti SQL-andmebaasi teenuse taseme- ja jõudluse taseme muutmine

Asendage ```{variables}``` oma väärtustega (ei sisalda soovitud looksulud).

```
$SubscriptionId = "{4cac86b0-1e56-bbbb-aaaa-000000000000}"
    
$ResourceGroupName = "{resourceGroup}"
$Location = "{AzureRegion}"
    
$ServerName = "{server}"
$DatabaseName = "{database}"
    
$NewEdition = "{Standard}"
$NewPricingTier = "{S2}"
    
Add-AzureRmAccount
Set-AzureRmContext -SubscriptionId $SubscriptionId
    
Set-AzureRmSqlDatabase -DatabaseName $DatabaseName -ServerName $ServerName -ResourceGroupName $ResourceGroupName -Edition $NewEdition -RequestedServiceObjectiveName $NewPricingTier
```
        


## <a name="next-steps"></a>Järgmised sammud

- [Skaala ja](sql-database-elastic-scale-get-started.md)
- [Ühenduse loomine ja päringu SQL-i andmebaasi SSMS](sql-database-connect-query-ssms.md)
- [Ekspordi Azure'i SQL-andmebaasiga](sql-database-export-powershell.md)

## <a name="additional-resources"></a>Lisaressursid

- [Äritegevuse järjepidevuse ülevaade](sql-database-business-continuity.md)
- [SQL-andmebaasi dokumentatsioon](http://azure.microsoft.com/documentation/services/sql-database/)
- [SQL Azure'i andmebaasi cmdlettide] (http://msdn.microsoft.com/library/azure/mt574084https :/ / msdn.microsoft.com/teegi/Azure'i/mt619433(v=azure.300\).aspx.aspx)