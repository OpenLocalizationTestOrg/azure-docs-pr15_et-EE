<properties
   pageTitle="Microsoft Power BI Preview manustatud tõrkeotsing"
   description="Microsoft Power BI Preview manustatud tõrkeotsing"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="microsoft-power-bi-embedded-preview-troubleshooting"></a>Microsoft Power BI Preview manustatud tõrkeotsing
Sellest artiklist leiate vastused **Power BI manustatud**tõrkeotsingu jaoks.

<a name="connection-string"/>
## <a name="setting-sql-server-connection-strings"></a>SQL serveri ühendusstringi seadmine
SQL Serveri ühenduse string määramiseks peate järgima kindlas vormingus. Allpool on näide ühenduse string SQL serveri korral.

```
"Persist Security Info=False;Integrated Security=true;Initial Catalog=Northwind;server=(local)"
```

SQL serveri ühendusstringi kohta lisateabe saamiseks lugege järgmisi artikleid:

-   [SQL serveri ühendusstringi](https://msdn.microsoft.com/library/jj653752.aspx)
-   [SqlConnection.ConnectionString](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.connectionstring.aspx)

<a name="credentials"/>
## <a name="setting-credentials"></a>Mandaadi seadmine
Juhul, kui teil on mandaat arengu või lavastus keskkonnas, nt kasutajanimi ja parool, peate värskendama identimisteave, mis vastavad tootmise lahenduse.

## <a name="see-also"></a>Vt ka
- [Valimi kasutamise alustamine](power-bi-embedded-get-started-sample.md)
- [Mis on Power BI manustatud](power-bi-embedded-what-is-power-bi-embedded.md)
