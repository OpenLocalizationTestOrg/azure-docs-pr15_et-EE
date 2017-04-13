<properties
    pageTitle="Asjade jaoturi abil seadmest faile üles laadida | Microsoft Azure'i"
    description="Järgige selles õppetükis saate teada, kuidas Azure'i asjade jaoturi abil C# seadmest faile üles laadida."
    services="iot-hub"
    documentationCenter=".net"
    authors="fsautomata"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="dotnet"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="06/21/2016"
     ms.author="elioda"/>

# <a name="tutorial-how-to-upload-files-from-devices-to-the-cloud-with-iot-hub"></a>Õpetus: Kuidas asjade jaoturi pilve seadmest faile üles laadida

## <a name="introduction"></a>Sissejuhatus

Azure'i asjade jaoturi on täielikult hallatav teenus, mis võimaldab usaldusväärsest ja turvaline kahesuunalise side miljoneid asjade seadmed ja rakenduse uuesti end. Eelmise õpetused ([Alustamine asjade jaoturi] ja [saatke Cloud-seadme asjade jaoturi]) illustreerida lihtsa seadme-to-cloud ja pilveteenuse-seadme sõnumite funktsioonile asjade jaoturi ja [protsessi seadme Cloud sõnumite] õpetuse kirjeldatakse nii usaldusväärselt salvestada seadme pilve sõnumite Azure'i bloobimälu. Siiski mõnel juhul ei saa hõlpsalt vastendada seadmetes saatmine suhteliselt seadme pilve sõnumitele, mis asjade jaoturi aktsepteerib andmed. Näiteks suuri faile, mis sisaldavad pilte, videoid, vibratsiooni andmete lahutusvõimega kõrge sagedus või mingi eeltöödeldud andmed sisaldavad. Need failid on tavaliselt paketi töödeldud tööriistadega nagu [Azure andmete Factory] või [Hadoopi] virnas pilveteenuses. Kui faili üleslaadimine seadmest, eelistatud saatmine sündmusi, saate siiski kasutada asjade jaoturi turvalisus ja usaldusväärsus funktsioone.

Selle õpetuse põhineb koodi [saata Cloud-seadme erineva asjade jaoturi] õpetuses kuvamiseks asjade jaoturi faili üleslaadimine võimaluste kasutamise kohta. See näitab, kuidas anda turvaliselt seade on Azure Bloobivahemälu URI üleslaadimise faili, ja kuidas kasutada asjade jaoturi faili üleslaadimine teatised, käivitamiseks faili oma rakenduse tagasi end töötlemine.

Õppeteema lõpus käivitate kaks Windows konsooli rakendusi:

* **SimulatedDevice**, [saatke Cloud-seadme asjade jaoturi] õpetuses, mis on lisatud faili salvestusruumi SAS URI poolt teie asjade jaoturi abil loodud rakendust muudetud versiooni.
* **ReadFileUploadNotification**, kes saab faili teated üleslaadimise kohta oma asjade keskuse kaudu.

> [AZURE.NOTE] Asjade jaoturi toetab paljusid seadme platvormide ja keelte (sh C, Java ja Javascript) kaudu Azure'i asjade seadme SDK-d. [Azure'i asjade Arenduskeskus] samm-sammult juhised, ühendage seade selles õpetuses näidatud koodi ja üldiselt Azure'i asjade jaoturi viidata.

Selle õpetuse lõpuleviimiseks vajate järgmist:

+ Microsoft Visual Studio 2015

+ Aktiivne Azure'i konto. (Kui teil pole kontot, saate luua [tasuta konto] [ lnk-free-trial] vaid paar minutit.)

## <a name="associate-an-azure-storage-account-to-iot-hub"></a>Azure Storage konto asjade jaoturi seostada.

Kuna jäljendatud seadme lisatud faili lisamine bloobimälu, peab teil olema seotud asjade jaoturi [Azure Storage] konto. Kui konto Azure Storage soovitud asjade keskuses seostada jaoturi saate luua SAS URI, mis seadme abil saate turvaliselt faili üleslaadimine bloobimälu ümbrises. Asjade jaoturi teenuse ja seadme SDK-d koordineerimine protsess, mis loob SAS URI ja teeb selle kättesaadavaks seadme abil faili üles laadida.

Järgige juhiseid teemas [konfigureerimine faili lisatud Azure'i portaalis] [ lnk-configure-upload] Azure Storage konto, et teie asjade keskuses seostada.

## <a name="upload-a-file-from-a-simulated-device"></a>Jäljendatud seadmest faili üleslaadimine

Selles jaotises saate muuta jäljendatud seadme rakenduse loodud [saatke Cloud-seadme asjade jaoturi] cloud-seadme sõnumeid asjade keskuse kaudu.

1. Visual Studio, paremklõpsake **SimulatedDevice** projekt, klõpsake nuppu **Lisa**ja klõpsake **Olemasoleva kirje**. Liikuge pildifaili ja lisada selle oma projekti. Selle õpetuse eeldab, et pilt nimega `image.jpg`.

2. Paremklõpsake pilti ja seejärel klõpsake käsku **Atribuudid**. Veenduge, et **kataloogi väljund** on määratud **Kopeeri alati**.

    ![][1]

3. Lisage faili **Program.cs** faili ülaosas järgmistest:

        using System.IO;

4. Saate lisada **programmi** klassi järgmisel viisil:
         
        private static async void SendToBlobAsync()
        {
            string fileName = "image.jpg";
            Console.WriteLine("Uploading file: {0}", fileName);
            var watch = System.Diagnostics.Stopwatch.StartNew();

            using (var sourceData = new FileStream(@"image.jpg", FileMode.Open))
            {
                await deviceClient.UploadToBlobAsync(fileName, sourceData);
            }

            watch.Stop();
            Console.WriteLine("Time to upload file: {0}ms\n", watch.ElapsedMilliseconds);
        }

    Funktsiooni `UploadToBlobAsync` meetod võtab faili nimi ja voo allikas faili üles laadida ja tegeleb üleslaadimise salvestusruumi. Konsooli rakendus kuvab aega kulub faili üles laadida.

5. Enne järgmise meetodi **põhi** meetodi lisamine soovitud `Console.ReadLine()` joon:

        SendToBlobAsync();

> [AZURE.NOTE] Selles õpetuses ei rakenda lihtsuse huvides, mis tahes uuesti poliitika. Kood, tuleks rakendate uuesti poliitikate (nt eksponentsiaalse backoff) soovitatud [Siirdamiseks viga töötlemise]MSDN-i artiklist.

## <a name="receive-a-file-upload-notification"></a>Teate faili üleslaadimine

Selles jaotises saate kirjutada Windows konsooli rakendus, mis saab faili üleslaadimine teated asjade keskuse kaudu.

1. Praeguse lahenduse Visual Studios luua uue projekti Visual C# Windows **Konsooli rakendus** projekti malli abil. Projekti **ReadFileUploadNotification**nimi.

    ![Uue projekti Visual Studio][2]

2. Solution Exploreris Paremklõpsake **ReadFileUploadNotification** projekti ja seejärel klõpsake nuppu **Halda NuGet-paketid**.

    Kuvatakse aken haldamine NuGet-paketid.

2. Otsige `Microsoft.Azure.Devices`, klõpsake nuppu **Installi**ja nõustuge kasutustingimustega. 

    See allalaadimist, installib ja lisab [Azure'i asjade Interneti - teenuse SDK Nugeti pakett] viide **ReadFileUploadNotification** Projectis.

3. Lisage faili **Program.cs** faili ülaosas järgmistest:

        using Microsoft.Azure.Devices;

4. Saate lisada **programmi** klassi järgmised väljad. Asendada asjade jaoturi ühendusstringi [Alustamine asjade jaoturi]kohatäite väärtus:

        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";
        
5. Saate lisada **programmi** klassi järgmisel viisil:
   
        private async static Task ReceiveFileUploadNotificationAsync()
        {
            var notificationReceiver = serviceClient.GetFileNotificationReceiver();

            Console.WriteLine("\nReceiving file upload notification from service");
            while (true)
            {
                var fileUploadNotification = await notificationReceiver.ReceiveAsync();
                if (fileUploadNotification == null) continue;

                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received file upload noticiation: {0}", string.Join(", ", fileUploadNotification.BlobName));
                Console.ResetColor();

                await notificationReceiver.CompleteAsync(fileUploadNotification);
            }
        }

    Pöörake tähelepanu sellele, vastuvõtu mustri sama kasutasite seadme rakenduse pilve-seadme sõnumeid vastu.

6. Lõpuks lisage järgmised read **Esilehele** meetod.

        Console.WriteLine("Receive file upload notifications\n");
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
        ReceiveFileUploadNotificationAsync().Wait();
        Console.ReadLine();

## <a name="run-the-applications"></a>Rakenduste käivitamine

Nüüd olete valmis rakendusi käivitada.

1. Visual Studios, teie lahendus paremklõpsake ja valige **käivitamisel määrata projektid**. Valige **käivitus mitmed projektid**, valige **ReadFileUploadNotification** ja **SimulatedDevice**toimingu **käivitamine** .

2. Vajutage **klahvi F5**. Mõlemad rakendused peab algama. Peaksite nägema üles ühe konsooli rakenduste ja sai konsooli rakendus üles teavitussõnumi lõpule. Saate [Azure portaali] või Visual Studio Server Explorer Azure Storage konto üleslaaditud faili olemasolu kontrollida.

  ![][50]


## <a name="next-steps"></a>Järgmised sammud

Selles õpetuses te teada, kuidas võimendada faili üleslaadimine võimaluste asjade jaoturi lihtsustamiseks faili üleslaadimiseks seadmetest. Saate jätkata uurimine asjade jaoturi funktsioonid ja stsenaariumid järgmistest artiklitest:

- [Soovitud asjade jaoturi programmiliselt loomine][lnk-create-hub]
- [C SDK tutvustus][lnk-c-sdk]
- [Asjade jaoturi SDK-d][lnk-sdks]

Asjade jaoturi võimaluste täpsemaks analüüsimiseks järgmistest teemadest.

- [Simuleerida seadme asjade lüüsi SDK][lnk-gateway]

<!-- Images. -->

[50]: ./media/iot-hub-csharp-csharp-file-upload/run-apps1.png
[1]: ./media/iot-hub-csharp-csharp-file-upload/image-properties.png
[2]: ./media/iot-hub-csharp-csharp-file-upload/create-identity-csharp1.png

<!-- Links -->

[Azure'i portaal]: https://portal.azure.com/

[Azure'i andmed Factory]: https://azure.microsoft.com/documentation/services/data-factory/
[Hadoopi]: https://azure.microsoft.com/documentation/services/hdinsight/

[Saatke asjade jaoturi Cloud-seade]: iot-hub-csharp-csharp-c2d.md
[Protsess seadme pilve sõnumid]: iot-hub-csharp-csharp-process-d2c.md
[Asjade jaoturi kasutamise alustamine]: iot-hub-csharp-csharp-getstarted.md
[Azure'i asjade Arenduskeskus]: http://www.azure.com/develop/iot

[Siirdamiseks viga töötlemine]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure'i salvestusruum]: ../storage/storage-create-storage-account.md#create-a-storage-account
[lnk-configure-upload]: iot-hub-configure-file-upload.md
[Azure'i asjade Interneti - teenuse SDK Nugeti pakett]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md


