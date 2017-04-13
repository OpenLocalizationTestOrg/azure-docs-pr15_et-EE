<properties
   pageTitle="Visual Studio ja SSDT installida SQL-i andmebaas | Microsoft Azure'i"
   description="Installige Visual Studio ja SQL Server arengu Tools (SSDT) SQL Azure'i andmebaas"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/16/2016"
   ms.author="sonyama;barbkess"/>

# <a name="install-visual-studio-2015-and-ssdt-for-sql-data-warehouse"></a>Visual Studio 2015 ja SSDT installida SQL-andmebaas

Koostada SQL-i andmebaas, soovitame kasutada Visual Studio 2015 uusima versiooni SQL serveri andmete tööriistad (SSDT).  Visual Studio 2013 värskenduse 5 SSDT toetatakse ka tagasiühilduvuse tagamiseks.  

SSDT Visual Studio abil saate visuaalselt uurimine tabelite, vaadete, Salvestatud toimingute ja palju veel objektide oma SQL-i andmebaas kui ka päringute sooritamine SQL serveri objekti Exploreri abil.

> [AZURE.NOTE] SQL-i andmebaas Visual Studio andmebaasi projektid veel ei toeta.  See funktsioon lisatakse tulevase versiooni.

## <a name="step-1-install-visual-studio-2015"></a>Samm 1: Installida Visual Studio 2015

Järgige neid linke alla laadima ja installima Visual Studio 2015. Kui teil on juba Visual Studio 2013 või 2015 installitud, saate jätkake juhisega 2, installige SSDT.

1. [Visual Studio 2015 alla laadida][].
2. Järgige [Visual Studio installimine][] juhend MSDN-i ja valige Vaikimisi kuvatakse.

## <a name="step-2-install-ssdt"></a>Samm 2: Installi SSDT

Installimiseks SSDT Visual Studio lihtsalt otsida SSDT update Visual Studio sees järgmiste juhiste järgi.

1. Klõpsake Visual Studio **Tööriistad** / **laiendid ja värskendustega...**  /  **Värskendused**
2. Valige **Tootevärskenduste** ja seejärel otsige **Microsoft SQL serveri andmebaasi värskendus instrumentaarium**

Kui värskendus ei leita, peaks olema installitud uusim versioon.  Kinnitage SSDT on installitud, klõpsake **Spikker** / **Kohta Microsoft Visual Studio** ja SQL Server Data Tools vaadake loendis.  SSDT uusim versioon on 14.0.60525.0.  Kui installimiseks suvandi valik pole saadaval, Visual Studio, võite te saate lehelt [SSDT alla laadida][] alla laadima ja installima SSDT käsitsi.

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui teil on SSDT uusim versioon, olete valmis [ühenduse loomiseks][] oma SQL-andmebaas.

<!--Anchors-->

<!--Image references-->

<!--Articles-->
[ühenduse loomine]: ./sql-data-warehouse-query-visual-studio.md

<!--Other-->
[Visual Studio 2015 allalaadimine]: https://www.visualstudio.com/downloads/
[Visual Studio installimisel]: https://msdn.microsoft.com/library/e2h7fzkw.aspx
[SSDT allalaadimine]: https://msdn.microsoft.com/library/mt204009.aspx
