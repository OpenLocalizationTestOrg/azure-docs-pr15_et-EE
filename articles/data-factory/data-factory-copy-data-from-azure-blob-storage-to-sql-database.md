<properties
    pageTitle="Kopeerige andmed bloobimälu SQL-andmebaasiga | Microsoft Azure'i"
    description="Selle õpetuse näidatakse, kuidas kasutada Kopeeri tegevuse on Azure andmete Factory teel bloobimälu andmete kopeerimine SQL-andmebaasi."
    Keywords="bloobimälu SQL-i, bloobimälu, andmete kopeerimine"
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="09/26/2016"
    ms.author="spelluru"/>

# <a name="copy-data-from-blob-storage-to-sql-database-using-data-factory"></a>Kopeerige andmed bloobimälu abil andmete Factory SQL-andmebaasiga 
> [AZURE.SELECTOR]
- [Ülevaade ja eeltingimused](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Kopeerige viisard](data-factory-copy-data-wizard-tutorial.md)
- [Azure'i portaal](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [PowerShelli](data-factory-copy-activity-tutorial-using-powershell.md)
- [Azure'i ressursihaldur Mall](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [REST API-GA](data-factory-copy-activity-tutorial-using-rest-api.md)
- [.NET-I API-GA](data-factory-copy-activity-tutorial-using-dotnet-api.md)


Selles õpetuses loote andmete factory müügivõimaluste bloobimälu andmete kopeerimine SQL-andmebaasi.

Kopeeri tegevuse sooritab Azure'i andmed Factory andmete liikumine. See on tootja globaalselt saadaval teenust, mida saate kopeerida andmeid erinevate andmete poed turvaline, usaldusväärseid ja scalable viisil. Vt [Andmete liikumine tegevuste](data-factory-data-movement-activities.md) artikkel Kopeeri tegevuse üksikasjad.  

> [AZURE.NOTE] Andmete Factory teenuse üksikasjaliku ülevaate leiate artiklist [Sissejuhatus Azure'i andmed Factory](data-factory-introduction.md) .

##<a name="prerequisites-for-the-tutorial"></a>Õpetuse eeltingimused
Enne alustamist selles õpetuses, peab teil olema järgmised:

- **Azure'i tellimus**.  Kui teil on tellimus, saate luua tasuta prooviversiooni konto vaid paar minutit. Lisateavet leiate teemast [Tasuta prooviversioon](http://azure.microsoft.com/pricing/free-trial/) .
- **Azure Storage konto**. Kasutate selle bloobimälu **Allika** andmete poe selles õpetuses. Kui teil pole kontot Azure storage, artiklist [Loo konto salvestusruumi](../storage/storage-create-storage-account.md#create-a-storage-account) loomiseks üksikasjalikud juhised.
- **Azure'i SQL-andmebaas**. Azure'i SQL-andmebaasiga kasutada **sihtkohta** andmed säilitada selles õpetuses. Kui teil pole Azure SQL-andmebaasi, mille abil saate õpetuses, lugege teemat [loomine ja konfigureerimine Azure'i SQL-andmebaasi](../sql-database/sql-database-get-started.md) loomiseks.
- **SQL Server 2012/2014 või Visual Studio 2013**. Näidisettevõtte andmebaasi loomiseks ja andmebaasi tulemi andmete kuvamiseks kasutada SQL Server Management Studio või Visual Studio.  

## <a name="collect-blob-storage-account-name-and-key"></a>Koguda bloobimälu salvestusruumikonto nimi ja võti 
Peate konto nimi ja selle õpetuse konto Azure storage konto võti. Pange kirja **konto nimi** ja **konto võti** Azure storage konto jaoks.

1. [Azure'i portaali](https://portal.azure.com/)sisse logida.
2. Klõpsake vasakul menüüs **rohkem teenuseid** ja valige **Salvestusruumi kontod**.

    ![Sirvige - salvestusruumi kontod](media\data-factory-copy-data-from-azure-blob-storage-to-sql-database\browse-storage-accounts.png)
3. Valige **Salvestusruumi kontod** labale **Azure storage konto** , mida soovite kasutada selles õpetuses.
4. Valige jaotises **sätted**linki **kiirklahvide** .
5.  Klõpsake tekstivälja **salvestusruumi konto nime** kõrval nuppu **Kopeeri** (pilt) ja Salvesta ja kleepige see kuhugi (nt: tekstifaili).
6. Korrake eelmist toimingut kopeerida või pange kirja **võti1**.
    
    ![Salvestusruumi kiirklahv](media\data-factory-copy-data-from-azure-blob-storage-to-sql-database\storage-access-key.png)
7. Sulgege kõik labad, klõpsates nuppu **X**.

## <a name="collect-sql-server-database-user-names"></a>SQL serveri, andmebaasi, kasutajanimed kogumine
Peate Azure SQL serveri andmebaasi ja kasutaja selle õpetuse nimed. Pange kirja nimede Azure SQL-andmebaasi **server**, **andmebaas**ja **kasutajale** .

1. **Azure'i portaal**, klõpsake vasakul nuppu **rohkem teenuseid** ja valige **SQL andmebaase**.
2. Valige **SQL andmebaase blade** **andmebaas** , mida soovite kasutada selles õpetuses. Pange tähele, **andmebaasi nimi**.  
3. **SQL-andmebaasi** tera, klõpsake jaotises **sätted**nuppu **Atribuudid** .
4. Pange kirja väärtused **Serveri nimi** ja **Serveri administraator Logi sisse**.
5. Sulgege kõik labad, klõpsates nuppu **X**.

## <a name="allow-azure-services-to-access-sql-server"></a>Azure'i teenuste SQL serveri juurdepääsu lubamine 
Veenduge, **et sätte **Luba juurdepääs Azure'i teenuste** jaoks sisse lülitatud Azure SQL serveris andmete Factory teenuse pääseks juurde oma Azure SQL serveri** . Kontrollige ja lülitage see säte, tehke järgmist.

1. **Rohkem teenuseid** jaoturi vasakul ja seejärel klõpsake **SQL-i serverid**.
2. Valige oma server ja klõpsake jaotises **sätted**nuppu **tulemüüri** . 
4. Klõpsake labale **tulemüüri sätted** **ON** **Luba juurdepääs Azure'i teenuste**jaoks.
5. Sulgege kõik labad, klõpsates nuppu **X**.

## <a name="prepare-blob-storage-and-sql-database"></a>Ettevalmistused bloobimälu ja SQL-andmebaas 
Nüüd ettevalmistamine oma Azure'i bloobimälu ja Azure SQL-andmebaasi õpetuse tehes järgmist:  

1. Käivitage Notepadi, kleepige järgmine tekst ja salvestamine **emp.txt** **C:\ADFGetStarted** kausta, kõvakettale.

        John, Doe
        Jane, Doe

2. Kasutage tööriistad, nt [Azure salvestusruumi Explorer](https://azurestorageexplorer.codeplex.com/) **adftutorial** container loomiseks ja ümbris **emp.txt** faili üles laadida.

    ![Azure'i salvestusruumi Explorer. Kopeerige andmed bloobimälu SQL-andmebaasiga](./media/data-factory-copy-data-from-azure-blob-storage-to-sql-database/getstarted-storage-explorer.png)
3. Järgmise SQL-i skripti abil saate luua tabeli **emp** Azure SQL-andmebaasis.  


        CREATE TABLE dbo.emp
        (
            ID int IDENTITY(1,1) NOT NULL,
            FirstName varchar(50),
            LastName varchar(50),
        )
        GO

        CREATE CLUSTERED INDEX IX_emp_ID ON dbo.emp (ID);

    **Kui teil on installitud teie arvutisse SQL Server 2012/2014:** järgige juhiseid [Samm 2: ühenduse SQL-andmebaasi haldamine Azure SQL-i andmebaasi SQL Server Management Studio abil](../sql-database/sql-database-manage-azure-ssms.md#Step2) artiklist Azure SQL serveriga ja käivitage SQL skript. Selles artiklis kasutab tulemüüri konfigureerimine Azure SQL serveri [klassikaline Azure'i portaalis](http://manage.windowsazure.com), mitte [uue Azure portaali](https://portal.azure.com).

    Kui teie klient Azure SQL serverit pole lubatud, peate konfigureerida tulemüüri oma Azure SQL serveri juurdepääsu oma arvutist (IP-aadress). Vt [käesoleva artikli](../sql-database/sql-database-configure-firewall-settings.md) juhised tulemüüri konfigureerimine Azure SQL serveris.

Eeltingimused on lõpule viidud. Saate luua andmete factory, kasutades ühte järgmistest viisidest. Klõpsake menüüd ülemises või õpetuse sooritamiseks järgmisi linke.     

- [Azure'i portaal](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [PowerShelli](data-factory-copy-activity-tutorial-using-powershell.md)
- [REST API-GA](data-factory-copy-activity-tutorial-using-rest-api.md)
- [.NET-I API-GA](data-factory-copy-activity-tutorial-using-dotnet-api.md)
- [Kopeerige viisard](data-factory-copy-data-wizard-tutorial.md)
