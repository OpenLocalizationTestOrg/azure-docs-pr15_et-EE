<properties 
    pageTitle="Azure'i portaalis Azure'i SQL-andmebaasi Geo-Dispersioonanalüüs konfigureerimine | Microsoft Azure'i" 
    description="Azure'i SQL-andmebaasi Azure'i portaalis Geo-Dispersioonanalüüs konfigureerimine" 
    services="sql-database" 
    documentationCenter="" 
    authors="stevestein" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="NA"
    ms.date="10/18/2016"
    ms.author="sstein"/>

# <a name="configure-geo-replication-for-azure-sql-database-with-the-azure-portal"></a>Azure'i portaalis Azure'i SQL-andmebaasi Geo-Dispersioonanalüüs konfigureerimine


> [AZURE.SELECTOR]
- [Ülevaade](sql-database-geo-replication-overview.md)
- [Azure'i portaal](sql-database-geo-replication-portal.md)
- [PowerShelli](sql-database-geo-replication-powershell.md)
- [T-SQL-IS](sql-database-geo-replication-transact-sql.md)

Selles artiklis kirjeldatakse, kuidas konfigureerida aktiivse Geo-kopeerimine [portaalis Azure](http://portal.azure.com)SQL-i andmebaasi.

Tõrkesiirde Azure'i portaalis algatada leiate [Azure'i SQL-andmebaasi Azure portaali jaoks planeeritud või planeerimata Tõrkesiirde algatada](sql-database-geo-replication-failover-portal.md).

>[AZURE.NOTE] Aktiivse Geo kopeerimine (loetavad sekundaaride) on nüüd saadaval kogu teenuse astme kõik andmebaasid. Aprill 2017, pensionile-loetav teisese tüüp ja -loetav olemasolevaid andmebaase uuendatakse automaatselt loetav sekundaaride.

Azure'i portaalis Geo-Dispersioonanalüüs konfigureerimiseks peate järgmisest allikast:

- Azure'i SQL-andmebaasi - esmane andmebaas, mida soovite korrata geograafiliste muule alale.

## <a name="add-secondary-database"></a>Teisene andmebaasi lisamine

Järgmised toimingud luua teisene uue andmebaasi Geo-Dispersioonanalüüs seos.  

Teisese lisamiseks peate olema tellimuse omanik või kaasomaniku. 

Teise andmebaasi on esmane andmebaasi nimi ja on vaikimisi teenuse samal tasemel. Teise andmebaasi võib olla üks andmebaas või elastne andmebaasis. Lisateavet leiate teemast [Teenuse astme](sql-database-service-tiers.md).
Kui teisese loonud ja seemnetega, hakkavad andmete imitatsiooniga esmane andmebaasist uue teise andmebaasi. 

> [AZURE.NOTE] Käsk nurjub, kui partneri andmebaas on juba olemas (nt - tulemusena lõpetab eelmise Geo-Dispersioonanalüüs seose).

### <a name="add-secondary"></a>Teisene lisamine

1. [Azure'i portaalis](http://portal.azure.com), liikuge sirvides andmebaas, mida soovite häälestada Geo-kopeerimise.
2. Lehel SQL-andmebaasi valige **Geo-kopeerimise**ja valige piirkond teise andmebaasi loomiseks. Valige mis tahes piirkond peale hosting esmane andmebaas piirkonna, kuid soovitatav on [andmepunktipaaride piirkond](../best-practices-availability-paired-regions.md) .

    ![konfigureerimine Geo-Dispersioonanalüüs](./media/sql-database-geo-replication-portal/configure-geo-replication.png)


4. Valige või serveri ja hinnakirjad taseme teisene andmebaasi konfigureerimine.

    ![teisene konfigureerimine](./media/sql-database-geo-replication-portal/create-secondary.png)

5. Soovi korral saate lisada teise andmebaasi elastne andmebaasi kohapeal on:

 Pargis teise andmebaasi loomiseks klõpsake **elastne andmebaasi kausta** ja valige pool, target serveris. On peab olemas serveris target, nagu see töövoog ei loo on.

6. Klõpsake nuppu **Loo** teisese lisada.
 
6. Teisene andmebaas on loodud ja külv protsess algab. 
 
    ![teisene konfigureerimine](./media/sql-database-geo-replication-portal/seeding0.png)

7. Kui külv protsess on lõpule jõudnud, kuvatakse teise andmebaasi selle olek.

    ![külv lõpetamine](./media/sql-database-geo-replication-portal/seeding-complete.png)


## <a name="remove-secondary-database"></a>Teisene andmebaasi eemaldamine

Toiming jäädavalt lõpeb dispersioonanalüüs teisene andmebaasi ja muutub roll teisese tavalises lugemis-ja kirjutamisõigusega andmebaasis. Kui teise andmebaasi ühenduvust on katkenud, käsk õnnestub, kuid teisene see, mis on ei saanud registri alles pärast ühenduvuse taastatakse.  

1. [Azure'i portaalis](http://portal.azure.com), liikuge Geo-Dispersioonanalüüs seos esmane andmebaas.
2. Valige lehel SQL-andmebaasi **Geo-Dispersioonanalüüs**.
3. Valige loendis **SEKUNDAARIDE** andmebaasi Geo-Dispersioonanalüüs partnerid eemaldada.
4. Klõpsake nuppu **Peata kopeerimine**.

    ![teisene eemaldamine](./media/sql-database-geo-replication-portal/remove-secondary.png)

5. Klõpsake nuppu **Peata Dispersioonanalüüs** avatakse kinnitusaknas nii nuppu **Jah** eemaldada andmebaasi Geo-Dispersioonanalüüs seos (määratud lugemis-ja kirjutamisõigusega andmebaasi mis tahes dispersioonanalüüs osa).


## <a name="next-steps"></a>Järgmised sammud

- Lisateavet kohta aktiivse Geo-kopeerimine, lugege teemat - [Aktiivse Geo-kopeerimine](sql-database-geo-replication-overview.md)
- Äritegevuse järjepidevuse ülevaade ja stsenaariumid, vaadake teemat [ettevõtetele järjepidevuse ülevaade](sql-database-business-continuity.md)

