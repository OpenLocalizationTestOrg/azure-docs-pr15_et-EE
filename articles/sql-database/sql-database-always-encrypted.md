<properties
    pageTitle="Alati krüptitud: Kaitsta tundlikku andmetes Azure'i SQL-andmebaasi andmebaasi krüptimine | Microsoft Azure'i"
    description="Kaitsta tundlikku andmete SQL-andmebaasis minutites."
    keywords="Krüpti, SQL-i krüptimist, andmebaasi krüptimine, tundliku loomuga andmete, alati krüptitud"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor="cgronlun"/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/18/2016"
    ms.author="sstein"/>

# <a name="always-encrypted-protect-sensitive-data-in-sql-database-and-store-your-encryption-keys-in-the-windows-certificate-store"></a>Alati krüptitud: Kaitsta tundlikku andmeid SQL-andmebaasis ja salvestada oma krüptimise võtmed serdi Windowsi poest

> [AZURE.SELECTOR]
- [Azure'i võtme Vault](sql-database-always-encrypted-azure-key-vault.md)
- [Serdi poes](sql-database-always-encrypted.md)


Selles artiklis kirjeldatakse, kuidas kaitsta tundlikku andmeid SQL-andmebaasis andmebaasi krüptimine [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/hh213248.aspx) [Alati krüptitud viisardi](https://msdn.microsoft.com/library/mt459280.aspx) abil. Seda ka näitab, kuidas salvestada oma krüptimise võtmed serdi Windowsi poest.

Alati krüptitud on uute andmete krüptimistehnoloogiale Azure SQL-andmebaasis SQL Server, mis aitab kaitsta tundlikku andmete ülejäänud server, kliendi ja serveri vahel liikumise ajal ja samal ajal, kui andmed on kasutusel, tagada, et tundlikku kunagi kuvatakse Lihttekst sees andmebaasi süsteem. Pärast andmed krüptida ainult klientrakendustes või rakenduse serverid, mis on toodud klahvid juurdepääsu kasutada Lihttekst andmeid. Üksikasjalikku teavet leiate teemast [Alati krüptitud (andmebaasimootor)](https://msdn.microsoft.com/library/mt163865.aspx).

Pärast andmebaasi kasutama alati krüptitud konfigureerimine loote kliendi rakendus C# koos Visual Studio krüptitud andmetega töötamiseks.

Sellest artiklist saate teada, kuidas häälestada alati krüptitud Azure SQL-andmebaasi juhised. Sellest artiklist saate teada, kuidas teha järgmisi toiminguid:

- SSMS alati krüptitud viisardi abil luua [Alati krüptitud võtmed](https://msdn.microsoft.com/library/mt163865.aspx#Anchor_3).
    - Loo [Veerg juhtslaidi klahv (CMK)](https://msdn.microsoft.com/library/mt146393.aspx).
    - Loo [veerg krüptovõtme (CEK)](https://msdn.microsoft.com/library/mt146372.aspx).
- Andmebaasi tabeli loomine ja veergude krüptimine.
- Rakendus, mis lisab, valitakse ja kuvatakse andmed krüptitud veergude loomine

## <a name="prerequisites"></a>Eeltingimused

Selles õpetuses peate:

- Azure'i konto ja tellimus. Kui teil pole ühte [tasuta prooviversiooni](https://azure.microsoft.com/pricing/free-trial/)kasutajaks.
- [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) versioon 13.0.700.242 või uuem versioon.
- [.NET Frameworki 4.6](https://msdn.microsoft.com/library/w0x726c2.aspx) või uuem versioon (klientarvutis).
- [Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).



## <a name="create-a-blank-sql-database"></a>Looge tühi SQL-i andmebaas
1. [Azure'i portaali](https://portal.azure.com/)sisse logida.
2. Klõpsake nuppu **Uus** > **andmete + salvestusruumi** > **SQL-andmebaasi**.
3. Looge **tühi** andmebaas nimega **kliinikus** uude või olemasolevasse serveris. Üksikasjalikud juhised Azure portaali andmebaasi loomise kohta leiate teemast [loomine SQL-andmebaasi minutites](sql-database-get-started.md).

    ![Looge tühi andmebaas](./media/sql-database-always-encrypted/create-database.png)

Peate allpool olevat õpetuse ühendusstring. Kui andmebaas on loodud, minge kliinikus uue andmebaasi ja kopeerige ühendusstring. Ühendusstringi saate igal ajal, kuid on lihtne kopeerige see, kui olete Azure'i portaalis.

1. Klõpsake nuppu **SQL andmebaase** > **kliinikus** > **Kuva andmebaasi ühendusstringi**.
2. Kopeerige ühendusstring **ADO.net-i**jaoks.

    ![Kopeerige ühendusstring](./media/sql-database-always-encrypted/connection-strings.png)


## <a name="connect-to-the-database-with-ssms"></a>Koos SSMS andmebaasiga ühenduse loomiseks

Avage SSMS ja kliinikus andmebaasi serveriga.


1. Avage SSMS. (Klõpsake nuppu **Ühenda** > **Andmebaasimootor** **ühenduse Server** akna avamiseks, kui see on avatud).
2. Sisestage oma serveri nimi ja mandaadi. SQL-i andmebaasi enne leiate serveri nimi ja ühendusstringi varem kopeeritud. Sisestage serveri nimi, sh *database.windows.net*.

    ![Kopeerige ühendusstring](./media/sql-database-always-encrypted/ssms-connect.png)

Kui avatakse aken **Uus tulemüüri reegel** , Azure'i sisse logida ja lasta SSMS tulemüüri uut reeglit luua teie jaoks.


## <a name="create-a-table"></a>Tabeli loomine

Selles jaotises loote tabeli andmete mahutamiseks. See on tavaline tabeli algselt--krüptimise tuleb konfigureerida järgmises jaotises.

1. Laienda **andmebaase**.
1. Paremklõpsake **kliinikus** andmebaas ja klõpsake nuppu **Uus päring**.
2. Päringu uues aknas ja **Käivita** järgmine Transact-SQL (T-SQL-i) kleepida selle.


        CREATE TABLE [dbo].[Patients](
         [PatientId] [int] IDENTITY(1,1),
         [SSN] [char](11) NOT NULL,
         [FirstName] [nvarchar](50) NULL,
         [LastName] [nvarchar](50) NULL,
         [MiddleName] [nvarchar](50) NULL,
         [StreetAddress] [nvarchar](50) NULL,
         [City] [nvarchar](50) NULL,
         [ZipCode] [char](5) NULL,
         [State] [char](2) NULL,
         [BirthDate] [date] NOT NULL
         PRIMARY KEY CLUSTERED ([PatientId] ASC) ON [PRIMARY] );
         GO


## <a name="encrypt-columns-configure-always-encrypted"></a>Krüpti veerud (konfigureerimine alati krüptitud)

SSMS pakub viisardi hõlpsalt konfigureerimiseks alati krüptitud häälestamine CMK, CEK ja krüptitud veergude jaoks.

1. Laienda **andmebaase** > **kliinikus** > **tabelid**.
2. Paremklõpsake tabelit **patsientide** ja valige **Krüpti veergude** alati krüptitud viisardi avamiseks.

    ![Veergude krüptimine](./media/sql-database-always-encrypted/encrypt-columns.png)

Alati krüptitud viisardi sisaldab järgmistest jaotistest: **Veeru valimine**, **Juhtslaidi klahvi konfigureerimine** (CMK), **valideerimise**ja **Kokkuvõte**.

### <a name="column-selection"></a>Veeru valimine ###

Klõpsake nuppu **Järgmine** lehe **Veeru valimine** avamiseks lehel **Sissejuhatus** . Sellel lehel milliseid veerge, mida soovite krüptida, valite [krüptimist, tüüp ja millised veeru krüptovõtme (CEK)](https://msdn.microsoft.com/library/mt459280.aspx#Anchor_2) kasutada.

Krüptige iga **SSN** ja **Sünnipäev** teave. **SSN** veergu kasutatakse tarkadeks krüptimist, mis toetab võrdse otsingud, ühendused ja Rühmita. **Sünnipäev** veergu kasutatakse randomiseeritud krüptimist, mis ei toeta toiminguid.

Seadke sätte **Krüptimise tüüp** **SSN** veeru **Deterministic** ja **randomiseeritud** **Sünnipäev** veergu. Klõpsake nuppu **edasi**.

![Veergude krüptimine](./media/sql-database-always-encrypted/column-selection.png)

### <a name="master-key-configuration"></a>Juhtslaidi klahvi konfigureerimine###

Lehel **Juhtslaidi klahvi konfigureerimine** on, kus häälestamine oma CMK ja valige pakkuja võtme poest soovitud CMK talletuskohaks. Praegu saate talletada on CMK serdi Windowsi poest, Azure'i klahvi Vault või riistvara turvalisus mooduli (HSM). Selle õpetuse näitab, kuidas salvestada oma klahvid serdi Windowsi poest.

Veenduge, et **poes sert** ja klõpsake nuppu **edasi**.

![Juhtslaidi klahvi konfigureerimine](./media/sql-database-always-encrypted/master-key-configuration.png)


### <a name="validation"></a>Valideerimine###

Saate krüptida veergude kohe või salvestada PowerShelli skripti käivitamiseks hiljem. Selles õpetuses valige **Jätka nüüd lõpuleviimiseks** ja klõpsake nuppu **edasi**.

### <a name="summary"></a>Kokkuvõte###

Kontrollige, kas sätted on kõik õige ja klõpsake nuppu **valmis** alati krüptitud jaoks häälestamise lõpuleviimiseks.

![Kokkuvõte](./media/sql-database-always-encrypted/summary.png)


### <a name="verify-the-wizards-actions"></a>Veenduge, et viisardi toimingud

Kui viisard on lõpule jõudnud, andmebaasi on seadistatud alati krüptitud. Viisardi teha järgmisi toiminguid:

- Loodud on CMK.
- Loodud on CEK.
- Valitud veergude krüptimiseks konfigureeritud. Teie **patsientide** tabelis on praegu pole andmeid, kuid mis tahes olemasolevad andmed on valitud veerud on nüüd krüptitud.

Saate kontrollida klahvid SSMS loomine minnes **kliinikus** > **Turvalisus** > **Alati krüptitud võtmed**. Nüüd näete viisardi loodud teile uus kiirklahve.


## <a name="create-a-client-application-that-works-with-the-encrypted-data"></a>Kliendi rakendus, mis töötab krüptitud andmed loomine

Nüüd, kui alati krüptitud on häälestatud, saate koostada rakendus, mis teeb *lisab* ja *valib* krüptitud veergude. Valimi rakenduse käivitamine õnnestub, peate käivitama see samasse arvutisse kui käivitasite viisardi alati krüptitud. Käivitage rakendus mõnes muus arvutis, peab juurutama rakenduse kliendi arvuti sertide alati krüptitud.  

> [AZURE.IMPORTANT] Rakenduse peate kasutama [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx) objektid, läbides Lihttekst andmete serverisse veergudega alati krüptitud. Erandi tulemuseks läbides väärtused SqlParameter objektide kasutamata.


1. Avage Visual Studio ja looge uus konsool rakendus C#. Veenduge, et projekti on seatud **.NET Frameworki 4.6** või uuem versioon.
2. Projekti **AlwaysEncryptedConsoleApp** nimi ja klõpsake nuppu **OK**.

![Uus konsool rakendus](./media/sql-database-always-encrypted/console-app.png)



## <a name="modify-your-connection-string-to-enable-always-encrypted"></a>Muutke oma ühendusstring lubamiseks alati krüptitud

Selles jaotises selgitatakse, kuidas lubada alati krüptitud oma andmebaasi ühendusstring. Muudate konsooli rakendus vastloodud järgmise jaotises "Alati krüptitud valimi konsooli rakendus".


Alati krüptitud lubamiseks peate **Veeru krüptimise säte** märksõnade lisamine oma ühendusstring ja määrake selle väärtuseks **lubatud**.

Saate seda otse ühendusstringis või saate häälestada selle mõne [SqlConnectionStringBuilder](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.aspx)abil. Järgmises jaotises rakenduse valimi näitab, kuidas kasutada **SqlConnectionStringBuilder**.

> [AZURE.NOTE] See on nõutav kliendi rakendus teatud alati krüptitud ainult muutmine. Kui teil on olemasolev rakendus, mis talletab selle ühendusstringi väliselt (ehk teisisõnu öeldes Otsingukonfiguratsiooni failis), võimalik Luba alati krüptitud koodi muutmata.


### <a name="enable-always-encrypted-in-the-connection-string"></a>Luba alati krüptitud ühendusstring

Oma ühendusstring järgmist märksõna lisamiseks tehke järgmist.

    Column Encryption Setting=Enabled


### <a name="enable-always-encrypted-with-a-sqlconnectionstringbuilder"></a>Luba alati on SqlConnectionStringBuilder krüptitud

Järgmine kood näitab, kuidas lubada alati krüptitud määrates [SqlConnectionStringBuilder.ColumnEncryptionSetting](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting.aspx) [lubatud](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectioncolumnencryptionsetting.aspx).

    // Instantiate a SqlConnectionStringBuilder.
    SqlConnectionStringBuilder connStringBuilder =
       new SqlConnectionStringBuilder("replace with your connection string");

    // Enable Always Encrypted.
    connStringBuilder.ColumnEncryptionSetting =
       SqlConnectionColumnEncryptionSetting.Enabled;



## <a name="always-encrypted-sample-console-application"></a>Alati krüptitud valimi konsooli rakendus

See näide näitab, kuidas:

- Muutke oma ühendusstring lubamiseks alati krüptitud.
- Sisestage andmed krüptitud veerud.
- Valige kirje, filtreerides teatud väärtust krüptitud veerus.

Asendage **Program.cs** sisu järgmine kood. Ühendusstringi globaalne connectionString muutuja kohal Main meetod Real asendada oma Azure portaali kehtiv ühendusstring. See on ainult muuta, peate tegema, et järgmine kood.

Käivitage rakendus, et näha alati krüptitud toiming.

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using System.Data;
    using System.Data.SqlClient;

    namespace AlwaysEncryptedConsoleApp
    {
    class Program
    {
        // Update this line with your Clinic database connection string from the Azure portal.
        static string connectionString = @"Replace with your connection string";

        static void Main(string[] args)
        {
            Console.WriteLine("Original connection string copied from the Azure portal:");
            Console.WriteLine(connectionString);

            // Create a SqlConnectionStringBuilder.
            SqlConnectionStringBuilder connStringBuilder =
                new SqlConnectionStringBuilder(connectionString);

            // Enable Always Encrypted for the connection.
            // This is the only change specific to Always Encrypted
            connStringBuilder.ColumnEncryptionSetting =
                SqlConnectionColumnEncryptionSetting.Enabled;

            Console.WriteLine(Environment.NewLine + "Updated connection string with Always Encrypted enabled:");
            Console.WriteLine(connStringBuilder.ConnectionString);

            // Update the connection string with a password supplied at runtime.
            Console.WriteLine(Environment.NewLine + "Enter server password:");
            connStringBuilder.Password = Console.ReadLine();


            // Assign the updated connection string to our global variable.
            connectionString = connStringBuilder.ConnectionString;


            // Delete all records to restart this demo app.
            ResetPatientsTable();

            // Add sample data to the Patients table.
            Console.Write(Environment.NewLine + "Adding sample patient data to the database...");

            InsertPatient(new Patient() {
                SSN = "999-99-0001", FirstName = "Orlando", LastName = "Gee", BirthDate = DateTime.Parse("01/04/1964") });
            InsertPatient(new Patient() {
                SSN = "999-99-0002", FirstName = "Keith", LastName = "Harris", BirthDate = DateTime.Parse("06/20/1977") });
            InsertPatient(new Patient() {
                SSN = "999-99-0003", FirstName = "Donna", LastName = "Carreras", BirthDate = DateTime.Parse("02/09/1973") });
            InsertPatient(new Patient() {
                SSN = "999-99-0004", FirstName = "Janet", LastName = "Gates", BirthDate = DateTime.Parse("08/31/1985") });
            InsertPatient(new Patient() {
                SSN = "999-99-0005", FirstName = "Lucy", LastName = "Harrington", BirthDate = DateTime.Parse("05/06/1993") });


            // Fetch and display all patients.
            Console.WriteLine(Environment.NewLine + "All the records currently in the Patients table:");

            foreach (Patient patient in SelectAllPatients())
            {
                Console.WriteLine(patient.FirstName + " " + patient.LastName + "\tSSN: " + patient.SSN + "\tBirthdate: " + patient.BirthDate);
            }

            // Get patients by SSN.
            Console.WriteLine(Environment.NewLine + "Now let's locate records by searching the encrypted SSN column.");

            string ssn;

            // This very simple validation only checks that the user entered 11 characters.
            // In production be sure to check all user input and use the best validation for your specific application.
            do
            {
                Console.WriteLine("Please enter a valid SSN (ex. 123-45-6789):");
                ssn = Console.ReadLine();
            } while (ssn.Length != 11);

            // The example allows duplicate SSN entries so we will return all records
            // that match the provided value and store the results in selectedPatients.
            Patient selectedPatient = SelectPatientBySSN(ssn);

            // Check if any records were returned and display our query results.
            if (selectedPatient != null)
            {
                Console.WriteLine("Patient found with SSN = " + ssn);
                Console.WriteLine(selectedPatient.FirstName + " " + selectedPatient.LastName + "\tSSN: "
                    + selectedPatient.SSN + "\tBirthdate: " + selectedPatient.BirthDate);
            }
            else
            {
                Console.WriteLine("No patients found with SSN = " + ssn);
            }

            Console.WriteLine("Press Enter to exit...");
            Console.ReadLine();
        }


        static int InsertPatient(Patient newPatient)
        {
            int returnValue = 0;

            string sqlCmdText = @"INSERT INTO [dbo].[Patients] ([SSN], [FirstName], [LastName], [BirthDate])
         VALUES (@SSN, @FirstName, @LastName, @BirthDate);";

            SqlCommand sqlCmd = new SqlCommand(sqlCmdText);


            SqlParameter paramSSN = new SqlParameter(@"@SSN", newPatient.SSN);
            paramSSN.DbType = DbType.AnsiStringFixedLength;
            paramSSN.Direction = ParameterDirection.Input;
            paramSSN.Size = 11;

            SqlParameter paramFirstName = new SqlParameter(@"@FirstName", newPatient.FirstName);
            paramFirstName.DbType = DbType.String;
            paramFirstName.Direction = ParameterDirection.Input;

            SqlParameter paramLastName = new SqlParameter(@"@LastName", newPatient.LastName);
            paramLastName.DbType = DbType.String;
            paramLastName.Direction = ParameterDirection.Input;

            SqlParameter paramBirthDate = new SqlParameter(@"@BirthDate", newPatient.BirthDate);
            paramBirthDate.SqlDbType = SqlDbType.Date;
            paramBirthDate.Direction = ParameterDirection.Input;

            sqlCmd.Parameters.Add(paramSSN);
            sqlCmd.Parameters.Add(paramFirstName);
            sqlCmd.Parameters.Add(paramLastName);
            sqlCmd.Parameters.Add(paramBirthDate);

            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    sqlCmd.ExecuteNonQuery();
                }
                catch (Exception ex)
                {
                    returnValue = 1;
                    Console.WriteLine("The following error was encountered: ");
                    Console.WriteLine(ex.Message);
                    Console.WriteLine(Environment.NewLine + "Press Enter key to exit");
                    Console.ReadLine();
                    Environment.Exit(0);
                }
            }
            return returnValue;
        }


        static List<Patient> SelectAllPatients()
        {
            List<Patient> patients = new List<Patient>();


            SqlCommand sqlCmd = new SqlCommand(
              "SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients]",
                new SqlConnection(connectionString));


            using (sqlCmd.Connection = new SqlConnection(connectionString))

            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    SqlDataReader reader = sqlCmd.ExecuteReader();

                    if (reader.HasRows)
                    {
                        while (reader.Read())
                        {
                            patients.Add(new Patient()
                            {
                                SSN = reader[0].ToString(),
                                FirstName = reader[1].ToString(),
                                LastName = reader["LastName"].ToString(),
                                BirthDate = (DateTime)reader["BirthDate"]
                            });
                        }
                    }
                }
                catch (Exception ex)
                {
                    throw;
                }
            }

            return patients;
        }


        static Patient SelectPatientBySSN(string ssn)
        {
            Patient patient = new Patient();

            SqlCommand sqlCmd = new SqlCommand(
                "SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [SSN]=@SSN",
                new SqlConnection(connectionString));

            SqlParameter paramSSN = new SqlParameter(@"@SSN", ssn);
            paramSSN.DbType = DbType.AnsiStringFixedLength;
            paramSSN.Direction = ParameterDirection.Input;
            paramSSN.Size = 11;

            sqlCmd.Parameters.Add(paramSSN);


            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    SqlDataReader reader = sqlCmd.ExecuteReader();

                    if (reader.HasRows)
                    {
                        while (reader.Read())
                        {
                            patient = new Patient()
                            {
                                SSN = reader[0].ToString(),
                                FirstName = reader[1].ToString(),
                                LastName = reader["LastName"].ToString(),
                                BirthDate = (DateTime)reader["BirthDate"]
                            };
                        }
                    }
                    else
                    {
                        patient = null;
                    }
                }
                catch (Exception ex)
                {
                    throw;
                }
            }
            return patient;
        }


        // This method simply deletes all records in the Patients table to reset our demo.
        static int ResetPatientsTable()
        {
            int returnValue = 0;

            SqlCommand sqlCmd = new SqlCommand("DELETE FROM Patients");
            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    sqlCmd.ExecuteNonQuery();

                }
                catch (Exception ex)
                {
                    returnValue = 1;
                }
            }
            return returnValue;
        }
    }

    class Patient
    {
        public string SSN { get; set; }
        public string FirstName { get; set; }
        public string LastName { get; set; }
        public DateTime BirthDate { get; set; }
    }
    }


## <a name="verify-that-the-data-is-encrypted"></a>Veenduge, et andmed on krüptitud.

Saate kiiresti, et väljal tegelike andmete serveris krüptitakse päringute SSMS **patsientide** andmeid. (Kasutada oma praeguse ühenduse kus veeru krüptimise säte ei ole veel lubatud).

Käivitage järgmine päring kliinikus andmebaasi.

    SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

Saate vaadata, ega sisalda krüptitud veergude Lihttekst andmeid.

   ![Uus konsool rakendus](./media/sql-database-always-encrypted/ssms-encrypted.png)


Lihttekst andmetele juurdepääsuks kasutada SSMS, saate lisada soovitud **veeru krüptimise säte = lubatud** ühenduse-parameeter.

1. Paremklõpsake oma serveri **Objekti**Explorer SSMS, ja seejärel nuppu **Katkesta ühendus**.
2. Klõpsake nuppu **Ühenda** > **Andmebaasimootor** **ühenduse Server** akna avamiseks, ja seejärel klõpsake nuppu **Suvandid**.
3. Klõpsake **Täiendavate ühenduse parameetrid** ja tippige **veeru krüptimise säte = lubatud**.

    ![Uus konsool rakendus](./media/sql-database-always-encrypted/ssms-connection-parameter.png)

4. Käivitage järgmine päring **kliinikus** andmebaasi.

        SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

     Nüüd näete krüptitud veeru andmed Lihttekst.


    ![Uus konsool rakendus](./media/sql-database-always-encrypted/ssms-plaintext.png)



> [AZURE.NOTE] Kui ühendate SSMS (või mis tahes klient) mõnest muust arvutist, neil pole juurdepääsu krüptimise võtmed ja ei saa dekrüptida andmed.



## <a name="next-steps"></a>Järgmised sammud
Kui loote andmebaasi, mis kasutab alati krüptitud, võite teha järgmist:

- Käivitage see valimi mõnes teises arvutis. See ei pääse krüptimise võtmed, nii et see ei ole Lihttekst andmetele juurdepääsu ja ei tööta edukalt.
- [Pööra ja teie võtmed puhastamiseks](https://msdn.microsoft.com/library/mt607048.aspx).
- [Migreerimine andmeid, mis on juba krüptitud alati krüptitud](https://msdn.microsoft.com/library/mt621539.aspx).
- [Muud klientarvutite serdid juurutamine alati krüptitud](https://msdn.microsoft.com/library/mt723359.aspx#Anchor_1) (jaotisest "Tegemist serdid saadaval abil rakenduste ja kasutajate").

## <a name="related-information"></a>Seotud teave

- [Alati krüptitud (klient arendamine)](https://msdn.microsoft.com/library/mt147923.aspx)
- [Läbipaistva andmete krüptimine](https://msdn.microsoft.com/library/bb934049.aspx)
- [SQL serveri krüptimist](https://msdn.microsoft.com/library/bb510663.aspx)
- [Alati krüptitud viisard](https://msdn.microsoft.com/library/mt459280.aspx)
- [Alati krüptitud ajaveeb](http://blogs.msdn.com/b/sqlsecurity/archive/tags/always-encrypted/)
