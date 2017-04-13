<properties
    pageTitle="Alustamine: SQL-i andmebaasi andmete sünkroonimine"
    description="Selles õpetuses aitab teil alustada Azure SQL-i andmete sünkroonimine (eelvaade)."
    services="sql-database"
    documentationCenter=""
    authors="jennieHubbard"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/11/2016"
    ms.author="jhubbard"/>


#<a name="getting-started-with-azure-sql-data-sync-preview"></a>Alustamine: Azure SQL-i andmete sünkroonimine (eelvaade)
Selles õppetükis saate teada, põhialuste Azure SQL-i andmete sünkroonimine Azure'i klassikaline portaalis.

Selle õpetuse eeldab minimaalse eelneva kogemus ja SQL Server Azure'i SQL-andmebaasi. Selles õpetuses hübriid (SQL Server ja SQL-andmebaasi eksemplarid) sünkroonimise rühma loomist täielikult konfigureeritud ja graafiku sünkroonita.

> [AZURE.NOTE] Täielik dokumentatsioon määramine Azure SQL-i andmete sünkroonimine varem asuv MSDN-i, on saadaval ka PDF-ina. Laadige see [siit](http://download.microsoft.com/download/4/E/3/4E394315-A4CB-4C59-9696-B25215A19CEF/SQL_Data_Sync_Preview.pdf).

## <a name="step-1-connect-to-the-azure-sql-database"></a>Samm 1: Azure'i SQL-andmebaasiga ühenduse loomine

1. [Klassikaline portaali](http://manage.windowsazure.com)sisse logima.

2. Klõpsake vasakpoolsel paanil **SQL ANDMEBAASE** .

3. Klõpsake nuppu **SÜNKROONI** lehe allosas. SÜNKROONI klõpsamisel kuvatakse loend näitab asju, mida saate lisada – **Uue sünkroonimine rühma** ja **Uue sünkroonimise Agent**.

4. Uue SQL-i sünkroonimise Agent viisardi käivitamiseks klõpsake nuppu **Uus sünkroonimise Agent**.

5. Kui te pole lisanud agent ette, **klõpsake nuppu laadige see siit**.

    ![Image1](./media/sql-database-get-started-sql-data-sync/SQLDatabaseScreen-Figure1.PNG)


## <a name="step-2-add-a-client-agent"></a>Samm 2: Lisage kliendi Agent
See on vajalik ainult juhul, kui te ei kavatse on kaasatud sünkroonimine rühma asutusesisese SQL serveri andmebaasi. Jätkake samm 4 Kui sünkroonimine rühmas on ainult SQL-andmebaasi eksemplarid.

<a id="InstallRequiredSoftware"></a>
### <a name="step-2a-install-the-required-software"></a>Samm 2a: nõutud tarkvara installimine
Veenduge, et teil on installitud arvuti, kuhu installitakse kliendi Agent.

- **.NET framework 4.0**

 Installige .NET Framework 4.0 [siin](http://go.microsoft.com/fwlink/?linkid=205836).

- **Microsoft SQL Server 2008 R2 SP1 CLR-i tüüpi (x86)**

 Installige Microsoft SQL Server 2008 R2 SP1 süsteemi CLR-i failitüübid (x86) [siin](http://www.microsoft.com/download/en/details.aspx?id=26728)

- **Microsoft SQL Server 2008 R2 SP1 jagatud halduse objektide (x86)**

 Installige Microsoft SQL Server 2008 R2 SP1 jagatud halduse objektide (x86) [siin](http://www.microsoft.com/download/en/details.aspx?id=26728)



<a id="InstallClient"></a>
### <a name="step-2b-install-a-new-client-agent"></a>Samm 2b: uus klient Agent installimine

Järgige juhiseid [installida kliendi Agent (SQL-i andmete sünkroonimine)](http://download.microsoft.com/download/4/E/3/4E394315-A4CB-4C59-9696-B25215A19CEF/SQL_Data_Sync_Preview.pdf) agent installimiseks.



<a id="RegisterSSDb"></a>
### <a name="step-2c-finish-the-new-sql-data-sync-agent-wizard"></a>Samm 2c: uue SQL-i sünkroonimise Agent viisardi juhiste täitmist

1.  Tagasi lehele uus SQL-i sünkroonimise Agent viisard.
2.  Pange agent tähenduslik nimi.
3.  Valige rippmenüü, **piirkond** (andmekeskuse) See agent majutada.
4.  Valige rippmenüüst, **tellimuse** see agent majutada.
5.  Klõpsake soovitud paremnool.



## <a name="step-3-register-a-sql-server-database-with-the-client-agent"></a>Samm 3: SQL serveri andmebaasi registreerida kliendi Agent

Pärast installimist kliendi Agent registreerida iga asutusesisese SQL Serveri andmebaas, mida kavatsete kaasa sünkroonimine rühma agendi.
Andmebaasi registreerida agent, järgige veebisaidil [registreerida kliendi agent SQL serveri andmebaasi](http://download.microsoft.com/download/4/E/3/4E394315-A4CB-4C59-9696-B25215A19CEF/SQL_Data_Sync_Preview.pdf).



## <a name="step-4-create-a-sync-group"></a>Samm 4: Sünkroonimine rühma loomine


<a id="StartNewSGWizard"></a>
### <a name="step-4a-start-the-new-sync-group-wizard"></a>Juhis 4a: alustamine jaotises Uus sünkroonimise viisard

1.  [Klassikaline portaali](http://manage.windowsazure.com)naasmiseks.
2.  Klõpsake nuppu **SQL-i andmebaasid**.
3.  Klõpsake lehe allosas **Lisa sünkroonimine** , siis valige Sünkrooni uus rühm sahtlis.

    ![Image2](./media/sql-database-get-started-sql-data-sync/NewSyncGroup-Figure2.png)



<a id=""></a>
### <a name="step-4b-enter-the-basic-settings"></a>4b juhis: Sisestage soovitud põhisätted


1.  Sisestage sünkroonimise rühma tähendusrikas nimi.
2.  Rippmenüüst, valige **piirkond** (andmekeskuse) majutada selle rühma sünkroonimine.
3. Klõpsake soovitud paremnool.

    ![Image3](./media/sql-database-get-started-sql-data-sync/NewSyncGroupName-Figure3.PNG)


<a id="DefineHubDB"></a>
### <a name="step-4c-define-the-sync-hub"></a>Juhis 4c: sünkroonimine jaoturi määratlemine

1. Valige rippmenüüst, SQL-andmebaasi eksemplar sünkroonimine rühma jaoturi olla.
2. Sisestage mandaat eksemplar SQL-andmebaasi - **JAOTURI KASUTAJANIME** ja **Parooli JAOTURI**.
3. Oodake, kuni SQL-i andmete sünkroonimine kinnitamiseks kasutajanimi ja parool. Kuvatakse roheline märge kuvatakse paremal pool parooli, kui mandaadi kinnitatakse.
4. Valige rippmenüüst **KONFLIKTILAHENDUS** poliitika.

 **Jaoturi võitis** - muudatuste kirjutada jaoturi andmebaasi kirjutage viide andmebaasidele, kirjutades muudatused samal viidata andmebaasi kirje. Funktsionaalselt, see tähendab, et esimene muudatus kirjutada keskus levib muid andmebaase.


 **Kliendi võitis** - jaoturi kirjutatud muudatused kirjutatakse viide andmebaaside muudatusi. Funktsionaalselt, see tähendab, et kirjutada keskus Viimane muudatus on üks hoida ja muid andmebaase olevatesse.

5.  Klõpsake soovitud paremnool.

    ![Image4](./media/sql-database-get-started-sql-data-sync/NewSyncGroupHub-Figure4.PNG)

<a id="AddRefDB"></a>
### <a name="step-4d-add-a-reference-database"></a>Juhis 4d: viide andmebaasi lisamine


Korrake seda toimingut iga täiendava andmebaasi lisamiseks rühma sünkroonimine.

1. Valige rippmenüüst andmebaasi lisada.

    Andmebaaside rippmenüüst kaasata nii SQL serveri andmebaase, mis on registreeritud agent ja SQL-andmebaasi eksemplarid.
2.  Sisestage mandaat selle andmebaasi - **KASUTAJANIME** ja **PAROOLIGA**.
3.  Valige rippmenüüst **Sünkroonimise SUUNA** see andmebaas.

    **Kahesuunalise** - muudatused viide andmebaasi kirjutada jaoturi andmebaasi ja muudatuste jaoturi andmebaasi kirjutada viide andmebaasi.

    **Sünkroonimine keskuse kaudu** - andmebaasi saab värskendusi keskuse kaudu. See ei saada jaoturi muudatused.

    **Jaoturi sünkroonimine** - andmebaas saadab jaoturi värskendused. See andmebaas pole kirjutatud muudatused keskuses.

4.  Sünkroonimine rühma loomise lõpetanud, klõpsake nuppu märge viisard paremas allnurgas. Oodake, kuni SQL-i andmete sünkroonimine mandaadi kinnitamiseks. Roheline märge tähistab identimisteabe kinnitatakse.

5.  Klõpsake märkeruutu teist korda. See teid tagasi lehele **sünkroonimine** SQL andmebaase. Selle rühma sünkroonimine on nüüd loetletud oma sünkroonimine rühmad ja agentide.

    ![Image5](./media/sql-database-get-started-sql-data-sync/NewSyncGroupReference-Figure5.PNG)


## <a name="step-5-define-the-data-to-sync"></a>Juhis 5: Määratleda andmete sünkroonimine

Azure SQL-i andmete sünkroonimine võimaldab valida tabelite ja veergude sünkroonida. Kui soovite veeru filtreerimine, nii et ainult kindlate väärtustega ridu (näiteks vanus > = 65) sünkroonima, kasutage Azure SQL-i andmete sünkroonimine portaali ja dokumentatsioonis valige tabelite, veergude ja ridade sünkroonimine määratleda andmete sünkroonimine.

1.  [Klassikaline portaali](http://manage.windowsazure.com)naasmiseks.
2.  Klõpsake nuppu **SQL-i andmebaasid**.
3.  Klõpsake vahekaarti **sünkroonimine** .
4.  Klõpsake nuppu Sünkrooni selle rühma nime.
5.  Klõpsake vahekaarti **Sünkroonimine reeglid** .
6.  Valige soovite sisestada sünkroonimine rühma skeemi andmebaas.
7.  Klõpsake soovitud paremnool.
8.  Klõpsake nuppu **VÄRSKENDA skeem**.
9.  Iga tabeli andmebaasi, valige veerud, kuvatakse sünkroonimise kaasamiseks.
    - Mittetoetatavad andmetüüpidega veergude ei saa valida.
    - Kui valitud on tabelis veerge, tabel on kaasatud jaotise sünkroonimine.
    - Vali/Tühista kõik tabelid, klõpsake nuppu Vali ekraani allosas.
10. Klõpsake nuppu **Salvesta**, siis oodake sünkroonimine rühma ettevalmistamise lõpuleviimiseks.
11. Andmete sünkroonimine sihtleht naasmiseks klõpsake nuppu tagasi noolega (üle sünkroonimisprobleemide rühma nime) ekraani vasakus ülanurgas.

    ![Image6](./media/sql-database-get-started-sql-data-sync/NewSyncGroupSyncRules-Figure6.PNG)

## <a name="step-6-configure-your-sync-group"></a>Samm 6: Konfigureerimine rühma sünkroonimine

Saate sünkroonida alati sünkroonimine rühma, klõpsates nuppu SÜNKROONI andmete sünkroonimine sihtleht allosas.
Ajakava sünkroonida, saate konfigureerida jaotise sünkroonimine.

1.  [Klassikaline portaali](http://manage.windowsazure.com)naasmiseks.
2.  Klõpsake nuppu **SQL-i andmebaasid**.
3.  Klõpsake vahekaarti **sünkroonimine** .
4.  Klõpsake nuppu Sünkrooni selle rühma nime.
5.  Klõpsake vahekaarti **KONFIGUREERIMINE** .
6.  **AUTOMAATNE SÜNKROONIMINE**
    - Jaotise sünkroonimise sünkroonimiseks klõpsake Vali sagedus konfigureerimiseks klõpsake **ON**. Siiski saate vajadusel sünkroonida, klõpsates nuppu SÜNKROONI.
    - Klõpsake **väljas** konfigureerida sünkroonimine rühma sünkroonida ainult siis, kui klõpsate nuppu SÜNKROONI.
7.  **SÜNKROONIMISE SAGEDUS**
    - Kui automaatne sünkroonimine on sees, määrata sünkroonimise sagedus. Peab olema 5 minutit kuni 1 kuu.
8.  Klõpsake nuppu **Salvesta**.

![Image7](./media/sql-database-get-started-sql-data-sync/NewSyncGroupConfigure-Figure7.PNG)

Palju õnne. Sünkroonimine rühma, mis sisaldab nii SQL-andmebaasi eksemplar SQL serveri andmebaasi loomist.

## <a name="next-steps"></a>Järgmised sammud
Lisateabe saamiseks vt SQL-andmebaas ja SQL-i andmete sünkroonimine:

* [Laadige alla täielik SQL-i andmete sünkroonimine dokumentatsioon](http://download.microsoft.com/download/4/E/3/4E394315-A4CB-4C59-9696-B25215A19CEF/SQL_Data_Sync_Preview.pdf)
* [SQL-i andmebaasi ülevaade](sql-database-technical-overview.md)
* [Andmebaasi elutsükli haldus](https://msdn.microsoft.com/library/jj907294.aspx)
 

 
