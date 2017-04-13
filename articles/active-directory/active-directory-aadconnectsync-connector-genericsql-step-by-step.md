<properties
   pageTitle="Üldise SQL-i konnektor samm-sammult | Microsoft Azure'i"
   description="Selles artiklis kõnnib saate lihtsa HR süsteemis üksikasjalike üldise SQL-i kasutamisega."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="08/30/2016"
   ms.author="billmath"/>

# <a name="generic-sql-connector-step-by-step"></a>Üldine SQL-i konnektor samm-sammult
See teema on samm-sammult juhendi. See loob lihtsa näidisandmebaasi HR ja seda kasutada mõne kasutaja ja tema liikmestaatuse importimise.

## <a name="prepare-the-sample-database"></a>Näidisettevõtte andmebaasi ettevalmistamine
Serverit SQL Server, käivitage SQL-skript leitud [Lisa](#appendix-a). Skript loob näidisandmebaasi nimega GSQLDEMO. Objektimudel loodud andmebaasi näeb välja nagu järgmisel pildil:  
![Objektimudel](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\objectmodel.png)

Luua kasutaja, mida soovite kasutada andmebaasiga ühenduse loomiseks. Ülevaate, kasutaja nimega FABRIKAM\SQLUser ja asub domeenis.

## <a name="create-the-odbc-connection-file"></a>ODBC ühendusfaili loomine
Üldine SQL-i konnektor abil ODBC remote serveriga ühendust luua. Kõigepealt on vaja luua faili ODBC ühenduse teavet.

1. ODBC kasuliku serverisse käivitamine  
![ODBC](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc.png)
2. Valige vahekaart **Faili DSN-i**. Klõpsake nuppu **Lisa**.
![ODBC1](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc1.png)
3. Väljale-, juht töötab viimistlemiseks, seega valige see ja klõpsake **järgmise >**.  
![ODBC2](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc2.png)
4. Tippige faili nimi, näiteks **GenericSQL**.  
![ODBC3](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc3.png)
5. Klõpsake nuppu **valmis**.  
![ODBC4](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc4.png)
6. Aeg, kes ühenduse konfigureeriks. Pange andmeallika hea kirjeldus ja SQL serveri töötava serveri nimi.  
![ODBC5](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc5.png)
7. Valige, kuidas SQL-i autentimiseks. Selles näites me kasutame Windowsi autentimist.  
![ODBC6](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc6.png)
8. Sisestage nimi näidisandmebaasi, **GSQLDEMO**.  
![ODBC7](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc7.png)
9. Hoia kogu vaikimisi ekraanil. Klõpsake nuppu **valmis**.  
![ODBC8](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc8.png)
10. Kontrollida, kas kõik toimib ootuspäraselt, klõpsake nuppu **Testi andmeallikat**.  
![ODBC9](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc9.png)
11. Veenduge, et katse õnnestumise.  
![ODBC10](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc10.png)
12. ODBC konfiguratsioonifail peaks nüüd olema nähtav faili DSN-i.  
![ODBC11](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc11.png)

Nüüd on faili vajame ja saate asuda konnektor.

## <a name="create-the-generic-sql-connector"></a>Üldine SQL-i konnektor loomine

1. Valige sünkroonimise Service Manager UI, **konnektorid** ja **loomine**. Valige **Üldine SQL-i (Microsoft)** ja pange kirjeldav nimi.  
![Connector1](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector1.png)
2. DSN-i eelmises tööetapis loodud faili üles leidnud ja serverisse üles. Mandaat on andmebaasiga ühenduse loomiseks.  
![Connector2](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector2.png)
3. Ülevaate, on hõlbustades meile ja öelda, et on kaks objekti tüübid, **kasutaja** ja **rühma**.
![Connector3](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector3.png)
4. Atribuutide leidmiseks soovime konnektor tuvastamiseks tabel ise vaadates neid atribuute. Kuna **Kasutajad** on SQL-i reserveeritud sõna, läheb vaja seda anda nurksulgudes [].  
![Connector4](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector4.png)
5. Aja määratlemiseks ankur atribuut ja DN atribuut. **Kasutajad**, kasutame kahe atribuudi kasutajanimi ja Tabeliseose kombinatsiooni. **Rühma**kasutame GroupName (mitte realistlik tegeliku elu, kuid see töötab jaotistes).
![Connector5](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector5.png)
6. Kõik atribuudi tüüpi saab avastada SQL-andmebaasis. Atribuut viitetüübi eriti ei saa. Rühma objekti tüübi, tuleb viitamiseks omaniku ID ja MemberID muutmine.  
![Connector6](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector6.png)
7. Valisime, nagu viide atribuute eelmises etapis nõua objekti tippige need väärtused atribuudid on viide. Meie puhul kasutaja objekti tüüp.  
![Connector7](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector7.png)
8. Valige lehel globaalne parameetrite delta strateegia **vesimärk** . Tippige ka kuupäeva/kellaaja vormingus **yyyy-MM-dd HH:mm:ss**.
![Connector8](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector8.png)
9. Valige lehel **konfigureerimine sektsioonid ja hierarhiad** nii objekti tüübid.
![Connector9](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector9.png)
10. **Valige objekti tüüp** ja **Valige atribuudid**, valige nii objekti tüübid ja kõik atribuudid. Klõpsake lehel **Ankrud konfigureerimine** **lõpule**.

## <a name="create-run-profiles"></a>Käivita profiili loomine

1. Valige sünkroonimise Service Manager UI, **konnektorid**ja **Käivitage profiilid konfigureerimine**. Klõpsake nuppu **uus profiil**. Alustame **Täielik Import**.  
![Runprofile1](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\runprofile1.png)
2. Valige **Täielik Import (etapi ainult)**.  
![Runprofile2](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\runprofile2.png)
3. Valige sektsiooni **objekti = kasutaja**.  
![Runprofile3](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\runprofile3.png)
4. Valige **Tabel** ja tippige **[kasutajate]**. Liikuge kerides jaotiseni mitme väärtusega objekti tüüp ja sisestage andmed, nagu järgmisel pildil. Valige **valmis** sammu.
![Runprofile4a](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\runprofile4a.png)  
![Runprofile4b](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\runprofile4b.png)  
5. Valige **Uus toiming**. Sel ajal, valige **objekti = rühma**. Kasutage viimasel lehel konfiguratsiooni, nagu järgmisel pildil. Klõpsake nuppu **valmis**.  
![Runprofile5a](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\runprofile5a.png)  
![Runprofile5b](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\runprofile5b.png)  
6. Valikuline: Kui soovite, saate konfigureerida täiendavaid Käivita profiilid. See selgituse kasutatakse ainult täielik Import.
7. Klõpsake nuppu **OK** , et muuta Käivita profiilid.

## <a name="add-some-test-data-and-test-the-import"></a>Mõned andmed testimine ja importimise testimiseks lisamine
Täitke oma näidisandmebaasi testi andmeid. Kui olete valmis, valige **Käivita** ja **täielik import**.

Siin on kaks telefoninumbrid kasutaja ja rühma mõned liikmed.  
![CS1](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\cs1.png)  
![CS2](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\cs2.png)  

## <a name="appendix-a"></a>Lisa A
**SQL-skripti valimi andmebaasi loomine**

```SQL
---Creating the Database---------
Create Database GSQLDEMO
Go
-------Using the Database-----------
Use [GSQLDEMO]
Go
-------------------------------------
USE [GSQLDEMO]
GO
/****** Object:  Table [dbo].[GroupMembers]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[GroupMembers](
    [MemberID] [int] NOT NULL,
    [Group_ID] [int] NOT NULL
) ON [PRIMARY]

GO
/****** Object:  Table [dbo].[GROUPS]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[GROUPS](
    [GroupID] [int] NOT NULL,
    [GROUPNAME] [nvarchar](200) NOT NULL,
    [DESCRIPTION] [nvarchar](200) NULL,
    [WATERMARK] [datetime] NULL,
    [OwnerID] [int] NULL,
PRIMARY KEY CLUSTERED
(
    [GroupID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]

GO
/****** Object:  Table [dbo].[USERPHONE]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
SET ANSI_PADDING ON
GO
CREATE TABLE [dbo].[USERPHONE](
    [USER_ID] [int] NULL,
    [Phone] [varchar](20) NULL
) ON [PRIMARY]

GO
SET ANSI_PADDING OFF
GO
/****** Object:  Table [dbo].[USERS]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[USERS](
    [USERID] [int] NOT NULL,
    [USERNAME] [nvarchar](200) NOT NULL,
    [FirstName] [nvarchar](100) NULL,
    [LastName] [nvarchar](100) NULL,
    [DisplayName] [nvarchar](100) NULL,
    [ACCOUNTDISABLED] [bit] NULL,
    [EMPLOYEEID] [int] NOT NULL,
    [WATERMARK] [datetime] NULL,
PRIMARY KEY CLUSTERED
(
    [USERID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]

GO
ALTER TABLE [dbo].[GroupMembers]  WITH CHECK ADD  CONSTRAINT [FK_GroupMembers_GROUPS] FOREIGN KEY([Group_ID])
REFERENCES [dbo].[GROUPS] ([GroupID])
GO
ALTER TABLE [dbo].[GroupMembers] CHECK CONSTRAINT [FK_GroupMembers_GROUPS]
GO
ALTER TABLE [dbo].[GroupMembers]  WITH CHECK ADD  CONSTRAINT [FK_GroupMembers_USERS] FOREIGN KEY([MemberID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[GroupMembers] CHECK CONSTRAINT [FK_GroupMembers_USERS]
GO
ALTER TABLE [dbo].[GROUPS]  WITH CHECK ADD  CONSTRAINT [FK_GROUPS_USERS] FOREIGN KEY([OwnerID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[GROUPS] CHECK CONSTRAINT [FK_GROUPS_USERS]
GO
ALTER TABLE [dbo].[USERPHONE]  WITH CHECK ADD  CONSTRAINT [FK_USERPHONE_USER] FOREIGN KEY([USER_ID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[USERPHONE] CHECK CONSTRAINT [FK_USERPHONE_USER]
GO
```
