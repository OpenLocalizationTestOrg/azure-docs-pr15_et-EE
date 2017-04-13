<properties
    pageTitle="Azure'i SQL-i andmebaasid Azure automatiseerimine abil hallata | Microsoft Azure'i"
    description="Siit saate teada, kuidas saate kasutada Azure automatiseerimine teenuse tasandil Azure SQL-i andmebaaside haldamine."
    services="sql-database, automation"
    documentationCenter=""
    authors="jodoglevy"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/26/2016"
    ms.author="jolevy"/>



#<a name="managing-azure-sql-databases-using-azure-automation"></a>Azure'i SQL-i andmebaasid abil Azure automatiseerimine haldamine

Käesolev juhend tutvustab teile Azure automatiseerimine teenuse ja kuidas seda saab kasutada SQL Azure'i andmebaasi haldamise lihtsustamiseks.


## <a name="what-is-azure-automation"></a>Mis on Azure automatiseerimine?

[Azure'i automaatika](https://azure.microsoft.com/services/automation/) on Azure'i teenus lihtsustamine pilve haldus – väljaprotsesside automatiseerimine. Azure'i automaatika kasutamisel pikaajalisi, käsitsi, vigu ja sageli korduvate ülesannete automatiseerida suurendamiseks töökindlust, tõhusust ja kellaaja väärtuse oma ettevõtte jaoks.

Azure'i automaatika pakub väga usaldusväärne ja tugevalt saadaval töövoo täitmise mootori, mis teie vajadustele, kui teie asutus kasvab skaala. Azure'i automaatika, protsesside saate olla käivitati käsitsi, 3-osaline süsteemide või ajastatud intervalliga nii, et tööülesanded tehakse täpselt vajadusel.

Väiksemad tegevus kohal ja selle vabastamine / DevOps töötajate keskenduda töö, mis lisab business väärtus, teisaldades cloud haldustoiminguid Azure'i automaatika käivituda.


## <a name="how-can-azure-automation-help-manage-azure-sql-databases"></a>Kuidas aitab Azure automatiseerimine Azure SQL-i andmebaaside haldamine?

Azure'i SQL-andmebaasi saab hallata Azure automatiseerimine [Azure SQL-i andmebaasi PowerShelli cmdlet-käsud](https://msdn.microsoft.com/library/dn546723.aspx) on saadaval [Azure PowerShelli tööriistade](https://msdn.microsoft.com/library/azure/jj156055.aspx)abil. Azure'i automaatika on need Azure SQL-i andmebaasi PowerShelli cmdlet-käsud saadaval välja kasti, nii, et kõik teie DB SQL-i haldamise toiminguid teenuses saate teha. Nende cmdlettide Azure'i automaatika koos cmdlet-käskudega muude Azure'i teenuste Azure'i teenuste ja 3 tootja süsteemid üle keerukate ülesannete automatiseerimiseks saate siduda.

Azure'i automaatika on ka võimalus suhelda SQL-i serverid otse väljastanud SQL-i käskude PowerShelli kaudu.

[Azure'i automaatika käitusjuhendi Galerii](https://azure.microsoft.com/blog/2014/10/07/introducing-the-azure-automation-runbook-gallery/) sisaldab mitmesuguseid toote meeskonna ja ühenduse tegevusraamatud alustada automatiseerimine Azure SQL-andmebaase, muud Azure'i teenuste ja 3 tootja süsteemide haldamine. Galerii tegevusraamatud on järgmised.

 * [Käivitage SQL serveri andmebaasi SQL-päringud](https://gallery.technet.microsoft.com/scriptcenter/How-to-use-a-SQL-Command-be77f9d2)
 * [Vertikaalselt mastaapimiseks (üles või alla) Azure SQL-andmebaasi ajakava](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-e957354f)
 * [Kui oma andmebaasi lähenedes selle maksimaalne maht, kärpige SQL tabel](https://gallery.technet.microsoft.com/scriptcenter/Azure-Automation-Your-SQL-30f8736b)
 * [Azure'i SQL-andmebaasi tabelid indekseerida, kui need on väga killustatud](https://gallery.technet.microsoft.com/scriptcenter/Indexes-tables-in-an-Azure-73a2a8ea)

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete õppinud põhitõdesid Azure automatiseerimine ja kuidas seda saab kasutada Azure SQL-i andmebaaside haldamine, järgige neid linke Lisateavet Azure automatiseerimine.

- [Azure'i automaatika ülevaade](../automation/automation-intro.md)
- [Minu esimene käitusjuhendi](../automation/automation-first-runbook-graphical.md)
- [Azure'i automaatika õ kaart](https://azure.microsoft.com/documentation/learning-paths/automation/)
- [Azure'i automaatika: Teie SQL-i Agent pilveteenuses](https://azure.microsoft.com/blog/2014/06/26/azure-automation-your-sql-agent-in-the-cloud/) 
 
