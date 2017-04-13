<properties
 pageTitle="Importimine ekspordi asjade jaoturi seadme identiteetide | Microsoft Azure'i"
 description="Põhimõtet ja .net-i kood Koodilõigud asjade jaoturi seadme identiteedid hulgi haldamiseks"
 services="iot-hub"
 documentationCenter=".net"
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/05/2016"
 ms.author="dobett"/>

# <a name="bulk-management-of-iot-hub-device-identities"></a>Hulgiredigeerimiseks asjade jaoturi seadme identiteetide haldus

Iga asjade jaoturi on seadme identiteedi registri abil saate luua seadme kohta ressursid teenus, nt järjekorras, mis sisaldab parda cloud-seadme sõnumeid. Seadme identiteedi registri ka võimaldab juurdepääsu seadme suunatud lõpp-punktid. Selles artiklis kirjeldatakse, kuidas importida ja eksportida seadme identiteedid mitmekaupa ja sealt seadme identiteedi registri.

Importimine ja eksportimine toimingud toimuvad *tööd* , mis võimaldavad teil käivitada hulgi toimingud on asjade jaoturi vastu kontekstis.

Klassi **RegistryManager** sisaldab **ExportDevicesAsync** ja **ImportDevicesAsync** viise, mida kasutada **töö** raames. Nende meetodite võimaldavad eksportida, importimine ja sünkroonimine asjade jaoturi seadme registri kogu.

## <a name="what-are-jobs"></a>Mis on töö?

**Töö** süsteemi kasutada seadme identiteedi registri toimingute kui toiming:

*  On võrreldes standardse käitusaja toimingute täitmise potentsiaalselt palju aega või
*  Tagastab kasutajale suurt hulka andmeid.

Sellisel juhul ühe API kõne ootel või blokeerimine toimingu tulemuse asemel loob toiming asünkroonselt **töö** asjade jaoturi. Selle toimingu seejärel tagastab kohe **JobProperties** objekti.

Järgmised koodilõigu C# näitab, kuidas luua ka ekspordi töö:

```
// Call an export job on the IoT Hub to retrieve all devices
JobProperties exportJob = await registryManager.ExportDevicesAsync(containerSasUri, false);
```

Seejärel saate **RegistryManager** klassi päringu tagastatud **JobProperties** metaandmete **töö** olek.

Järgmised koodilõigu C# näitab, kuidas küsitlus iga viie sekundi kuvamiseks, kui on töö lõpetanud, täitmine:

```
// Wait until job is finished
while(true)
{
  exportJob = await registryManager.GetJobAsync(exportJob.JobId);
  if (exportJob.Status == JobStatus.Completed || 
      exportJob.Status == JobStatus.Failed ||
      exportJob.Status == JobStatus.Cancelled)
  {
    // Job has finished executing
    break;
  }

  await Task.Delay(TimeSpan.FromSeconds(5));
}
```

## <a name="export-devices"></a>Ekspordi seadmed

**ExportDevicesAsync** meetodi abil saate eksportida kogu asjade jaoturi seadme registri [Salvestusruumi Azure'i](https://azure.microsoft.com/documentation/services/storage/) bloobimälu container kaudu [Ühiskasutusse antud juurdepääs allkirja](https://msdn.microsoft.com/library/ee395415.aspx).

See meetod võimaldab varundamiseks bloobimälu ümbrises, mida saate määrata usaldusväärne teie seadme kohta.

**ExportDevicesAsync** meetod nõuab kahte parameetrid:

*  *String* , mis sisaldab URI bloobimälu ümbrises. Selle URI peab sisaldama SAS luba, mis annab kirjutusõiguse ümbris. Töö loob Blokeeri bloobimälu selles ümbrises sarjadesse jaotatud ekspordi seadme andmete talletamiseks. SAS luba peab sisaldama järgmisi õigusi:
    
    ```
    SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Delete
    ```

*  On *kahendväärtus* , mis näitab, kas soovite autentimise võtmed välistamine andmete eksportimine. Kui **false**, autentimine klahvid kaasatud ekspordi väljund; Vastasel korral eksporditakse klahvid nimega **null**.

Järgmised koodilõigu C# näitab, kuidas alustada ekspordi töö, mis sisaldab seadme autentimise klahvid andmete eksportimine ja seejärel lõpetamise küsitlus.

```
// Call an export job on the IoT Hub to retrieve all devices
JobProperties exportJob = await registryManager.ExportDevicesAsync(containerSasUri, false);

// Wait until job is finished
while(true)
{
    exportJob = await registryManager.GetJobAsync(exportJob.JobId);
    if (exportJob.Status == JobStatus.Completed || 
        exportJob.Status == JobStatus.Failed ||
        exportJob.Status == JobStatus.Cancelled)
    {
    // Job has finished executing
    break;
    }

    await Task.Delay(TimeSpan.FromSeconds(5));
}
```

Töö salvestab selle väljundi esitatud bloobimälu ümbrises Blokeeri bloobimälu koos nimi **devices.txt**. Andmete väljundi koosneb JSON seeriasertide seadme andmeid, ühe seadmega rea kohta.

Järgmises näites on kujutatud väljundi andmeid:

```
{"id":"Device1","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device2","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device3","eTag":"MA==","status":"disabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device4","eTag":"MA==","status":"disabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device5","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
```

Kui teil on vaja juurdepääsu andmetele koodi, te saate hõlpsasti andmeatribuutide abil **ExportImportDevice** klassi andmed. Järgmised koodilõigu C# näitab, kuidas lugeda seadme teave, mis on varem eksporditud Blokeeri bloobimälu:

```
var exportedDevices = new List<ExportImportDevice>();

using (var streamReader = new StreamReader(await blob.OpenReadAsync(AccessCondition.GenerateIfExistsCondition(), RequestOptions, null), Encoding.UTF8))
{
  while (streamReader.Peek() != -1)
  {
    string line = await streamReader.ReadLineAsync();
    var device = JsonConvert.DeserializeObject<ExportImportDevice>(line);
    exportedDevices.Add(device);
  }
}
```

> [AZURE.NOTE]  Saate **GetDevicesAsync** meetodit **RegistryManager** klassi toomiseks teie seadmete loend. Sellel lähenemisel on raske ots 1000 seadme objekte, mis tagastatakse arvu. Oodatav kasutamine puhul **GetDevicesAsync** meetod on arengu stsenaariumid abi silumine ja ei ole soovitatav tootmise töökoormus.

## <a name="import-devices"></a>Seadmete importimine

**ImportDevicesAsync** meetodit **RegistryManager** tunni võimaldab teil teha hulgi impordi- ja sünkroonimise toiminguid asjade jaoturi seadme registri. Nagu **ExportDevicesAsync** meetod, kasutab **ImportDevicesAsync** meetodit **töö** raames.

Olge ettevaatlik, sest lisaks ettevalmistamise uute seadmete oma seadme identiteedi registri, seda ka värskendada ja kustutada olemasolevate seadmete **ImportDevicesAsync** meetodi abil.

> [AZURE.WARNING]  Imporditoimingu ei saa tagasi võtta. Alati varundada oma olemasolevad andmed, kasutades **ExportDevicesAsync** meetodi abil muu bloobimälu container enne muudatuste hulgi oma seadme identiteedi registris.

**ImportDevicesAsync** meetod on kaks parameetrid:

*  *String* , mis sisaldab, on [Salvestusruumi Azure'i](https://azure.microsoft.com/documentation/services/storage/) bloobimälu container URI *Sisestuskeel* töö kasutamiseks. Selle URI peab sisaldama SAS luba, mis annab ümbris lugemisõigus. See ümbris peab sisaldama on bloobimälu nimi **devices.txt** sarjadesse jaotatud seadme andmeid importida oma seadme identiteedi registrit sisaldav abil. Andmete importimine peab sisaldama sama JSON-vormingus, kasutava **ExportImportDevice** töö loob **devices.txt** bloobimälu seadme teave. SAS luba peab sisaldama järgmisi õigusi:

    ```
    SharedAccessBlobPermissions.Read
    ```

*  *String* , mis sisaldab URI on [Salvestusruumi Azure'i](https://azure.microsoft.com/documentation/services/storage/) bloobimälu pakendi *väljundi* töö kasutamiseks. Töö loob Blokeeri bloobimälu selles ümbrises tõrgete teave **töö**valmis importida talletamiseks. SAS luba peab sisaldama järgmisi õigusi:
    
    ```
    SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Delete
    ```

> [AZURE.NOTE]  Kahe parameetrid saate osutage sama bloobimälu ümbrises. Eraldi parameetrid lihtsalt lubada rohkem kontrolli andmete väljundi container nõuab täiendavaid õigusi.

Järgmised koodilõigu C# näitab, kuidas alustada ka importimine töö:

```
JobProperties importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);
```

## <a name="import-behavior"></a>Impordi käitumine

**ImportDevicesAsync** meetodi abil saate teha oma seadme identiteedi registri hulgi:

-   Uute seadmete hulgi registreerimine
-   Olemasoleva seadmete hulgi kustutamised
-   Oleku muutumise hulgiredigeerimiseks (lubamine või keelamine seadmed)
-   Uue seadme autentimise klahvid hulgi määramine
-   Hulgi automaatne-regenereerimise seadme autentimise võtmed

Eelmise toimingud ühe **ImportDevicesAsync** kõne jooksul saate teha. Näiteks saate uue seadmed registreerida ja kustutada või värskendada olemasoleva seadmete samal ajal. Kui seda kasutatakse koos **ExportDevicesAsync** meetod, saate täielikult migreerida kõigis oma seadmetes ühe asjade keskuse kaudu teisele.

Andmete importimine sariväljaanne iga seadme valikuline **importMode** atribuudi abil määrata on importimise protsess-seadme kohta. Atribuut **importMode** on järgmised suvandid.

| importMode |  Kirjeldus |
| -------- | ----------- |
| **createOrUpdate** | Kui seade pole määratud **id**-ga, on see äsja registreeritud. <br/>Kui seade on juba olemas, kirjutatakse olemasolev teave esitatud sisendandmete arvestamata **ETag** -väärtus. |
| **loomine** | Kui seade pole määratud **id**-ga, on see äsja registreeritud. <br/>Kui seade on juba olemas, kirjutatakse tõrke logifaili. |
| **värskendamine** | Kui seadme määratud **id**juba olemas, kirjutatakse olemasolev teave esitatud sisendandmete arvestamata **ETag** -väärtus. <br/>Kui seade pole olemas, kirjutatakse tõrke logifaili. |
| **updateIfMatchETag** | Kui seadme määratud **id**juba olemas, kirjutatakse olemasolev teave esitatud sisendandmete ainult juhul, kui seal on **ETag** vastet. <br/>Kui seade pole olemas, kirjutatakse tõrke logifaili. <br/>Kui mõni **ETag** lahknevuse, kirjutatakse tõrke logifaili. |
| **createOrUpdateIfMatchETag** | Kui seade pole määratud **id**-ga, on see äsja registreeritud. <br/>Kui seade on juba olemas, kirjutatakse olemasolev teave esitatud sisendandmete ainult juhul, kui seal on **ETag** vastet. <br/>Kui mõni **ETag** lahknevuse, kirjutatakse tõrke logifaili. |
| **kustutamine** | Kui seade on juba olemas määratud **id**-ga, kustutatakse see arvestamata **ETag** -väärtus. <br/>Kui seade pole olemas, kirjutatakse tõrke logifaili. |
| **deleteIfMatchETag** | Kui seade on juba olemas määratud **id**-ga, kustutatakse see ainult juhul, kui seal on **ETag** vastet. Kui seade pole olemas, kirjutatakse tõrke logifaili. <br/>Kui mõni ETag lahknevuse, kirjutatakse tõrke logifaili. |

> [AZURE.NOTE] Kui sariväljaanne andmete Määratle konkreetselt mõne seadme jaoks **importMode** lipu, vaikimisi **createOrUpdate** imporditoimingu ajal.

## <a name="import-devices-example--bulk-device-provisioning"></a>Importimine seadmete näide – hulgiredigeerimiseks seadme ettevalmistamine 

Järgmine kood näidis C# näitab, kuidas luua identiteetide seadme mis:

- Lisage autentimise võtmed.
- Blokeeri bloobimälu kirjutada selle seadme teavet.
- Seadme identiteedi registri seadmete importida.

```
// Provision 1,000 more devices
var serializedDevices = new List<string>();

for (var i = 0; i < 1000; i++)
{
// Create a new ExportImportDevice
  var deviceToAdd = new ExportImportDevice()
  {
    Id = Guid.NewGuid().ToString(),
    Status = DeviceStatus.Enabled,
    Authentication = new AuthenticationMechanism()
    {
      SymmetricKey = new SymmetricKey()
      {
        PrimaryKey = CryptoKeyGenerator.GenerateKey(32),
        SecondaryKey = CryptoKeyGenerator.GenerateKey(32)
      }
    },
    ImportMode = ImportMode.Create
  };

  // Add device to existing list
  serializedDevices.Add(JsonConvert.SerializeObject(deviceToAdd));
}

// Write this list to the blob
var sb = new StringBuilder();
serializedDevices.ForEach(serializedDevice => sb.AppendLine(serializedDevice));
await blob.DeleteIfExistsAsync();

using (CloudBlobStream stream = await blob.OpenWriteAsync())
{
  byte[] bytes = Encoding.UTF8.GetBytes(sb.ToString());
  for (var i = 0; i < bytes.Length; i += 500)
  {
    int length = Math.Min(bytes.Length - i, 500);
    await stream.WriteAsync(bytes, i, length);
  }
}

// Call import using the same blob to add new devices!
// This normally takes 1 minute per 100 devices the normal way
JobProperties importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);

// Wait until job is finished
while(true)
{
  importJob = await registryManager.GetJobAsync(importJob.JobId);
  if (importJob.Status == JobStatus.Completed || 
      importJob.Status == JobStatus.Failed ||
      importJob.Status == JobStatus.Cancelled)
  {
    // Job has finished executing
    break;
  }

  await Task.Delay(TimeSpan.FromSeconds(5));
}
```

## <a name="import-devices-example--bulk-deletion"></a>Seadmete näide – hulgi kustutamiseks importimine

Järgmine kood näidis kuvatakse, kuidas kustutada seadmete lisasite, kasutades eelmises proovi kood.

```
// Step 1: Update each device's ImportMode to be Delete
sb = new StringBuilder();
serializedDevices.ForEach(serializedDevice =>
{
  // Deserialize back to an ExportImportDevice
  var device = JsonConvert.DeserializeObject<ExportImportDevice>(serializedDevice);

  // Update property
  device.ImportMode = ImportMode.Delete;

  // Re-serialize
  sb.AppendLine(JsonConvert.SerializeObject(device));
});

// Step 2: Write the new import data back to the block blob
await blob.DeleteIfExistsAsync();
using (CloudBlobStream stream = await blob.OpenWriteAsync())
{
  byte[] bytes = Encoding.UTF8.GetBytes(sb.ToString());
  for (var i = 0; i < bytes.Length; i += 500)
  {
    int length = Math.Min(bytes.Length - i, 500);
    await stream.WriteAsync(bytes, i, length);
  }
}

// Step 3: Call import using the same blob to delete all devices!
importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);

// Wait until job is finished
while(true)
{
  importJob = await registryManager.GetJobAsync(importJob.JobId);
  if (importJob.Status == JobStatus.Completed || 
      importJob.Status == JobStatus.Failed ||
      importJob.Status == JobStatus.Cancelled)
  {
    // Job has finished executing
    break;
  }

  await Task.Delay(TimeSpan.FromSeconds(5));
}

```

## <a name="getting-the-container-sas-uri"></a>Container SAS URI otsimine


Järgmine kood näidis kuvatakse kuidas luua [SAS URI](../storage/storage-dotnet-shared-access-signature-part-2.md) lugemine, kirjutamine ja Kustuta õiguste bloobimälu ümbrises.

```
static string GetContainerSasUri(CloudBlobContainer container)
{
  // Set the expiry time and permissions for the container.
  // In this case no start time is specified, so the
  // shared access signature becomes valid immediately.
  var sasConstraints = new SharedAccessBlobPolicy();
  sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
  sasConstraints.Permissions = 
    SharedAccessBlobPermissions.Write | 
    SharedAccessBlobPermissions.Read | 
    SharedAccessBlobPermissions.Delete;

  // Generate the shared access signature on the container,
  // setting the constraints directly on the signature.
  string sasContainerToken = container.GetSharedAccessSignature(sasConstraints);

  // Return the URI string for the container,
  // including the SAS token.
  return container.Uri + sasContainerToken;
}

```

## <a name="next-steps"></a>Järgmised sammud

Selles artiklis soovite õpitut soovitud asjade keskuses seadme identiteedi registri vastu hulgi toimingute sooritamiseks. Järgige neid linke Azure'i asjade jaoturi haldamise kohta lisateabe saamiseks:

- [Kasutus mõõdikud][lnk-metrics]
- [Toimingute jälgimine][lnk-monitor]

Asjade jaoturi võimaluste täpsemaks analüüsimiseks järgmistest teemadest.

- [Arendaja juhend][lnk-devguide]
- [Simuleerida seadme asjade lüüsi SDK][lnk-gateway]

[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md