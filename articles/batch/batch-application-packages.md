<properties
    pageTitle="Lihtne rakenduse installimine ja Azure'i paketi haldus | Microsoft Azure'i"
    description="Kasutage funktsiooni rakenduse pakettide Azure'i partii hõlpsasti hallata mitut rakendused ja versioonid paketi installimiseks arvutada sõlmed."
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor="" />

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="big-compute"
    ms.date="10/21/2016"
    ms.author="marsma" />

# <a name="application-deployment-with-azure-batch-application-packages"></a>Azure'i paketi rakenduse paketid rakenduse juurutamine

Rakenduse paketid funktsioon Azure'i partii pakub lihtne tööülesande rakenduste haldamise ja Arvuta sõlmed olevate oma juurutust. Rakenduse paketid, saate üles laadida ja hallata tööülesannete käivitada, sealhulgas nende täiendavate failide rakenduste mitme versiooni. Saate siis automaatselt juurutada ühte või mitut need rakendused Arvuta sõlmed olevate.

Sellest artiklist saate teada, kuidas üles laadida ja hallata rakenduse paketid Azure'i portaalis. Seejärel saate teada, kuidas installimine on pool Arvuta sõlmed koos [Paketi .NET] [ api_net] teek.

> [AZURE.NOTE] Siin kirjeldatud rakenduse paketid funktsioon asendab "Paketi rakendused" funktsiooni teenuse varasemates versioonides saadaval.

## <a name="application-package-requirements"></a>Rakenduse paketi nõuded

Peate kasutama rakenduse paketid kontole paketi [link Azure Storage konto](#link-a-storage-account) .

Selles artiklis kirjeldatud rakenduse paketid funktsioon on ühilduvad *ainult* paketi kaustu, mis on loodud pärast 10 märts 2016 abil. Rakenduse paketid on juurutatud arvutada sõlmed kaustadesse loodud varasem kuupäev.

See funktsioon võeti kasutusele [Paketi REST API] [ api_rest] versiooni 2015-12-01.2.2 ja vastava [Paketi .NET] [ api_net] teegi versioon 3.1.0. Soovitame teil alati kasutada API uusim versioon paketi töötamisel.

> [AZURE.IMPORTANT] Ainult *CloudServiceConfiguration* kaustu toetus rakenduse paketid. Te ei saa kasutada rakenduse paketid VirtualMachineConfiguration piltide abil loodud kaustu. Jaotisest [virtuaalse masina konfiguratsioon](batch-linux-nodes.md#virtual-machine-configuration) [sätte Linux arvutada sõlmed Azure'i paketi kaustadesse](batch-linux-nodes.md) kahe eri konfiguratsioone kohta lisateavet.

## <a name="about-applications-and-application-packages"></a>Rakenduste ja rakenduse paketid kohta

Azure'i paketi, sees viitab *rakenduse* komplekti versiooniga kahendfaile, mida saab automaatselt alla laadida Arvuta sõlmed olevate. Mõne *rakendusepaketi* viitab *määratud* nende kahendfaile ja tähistab rakenduse antud *versioon* .

![Üksikasjalik skemaatilise diagrammi rakenduste ja rakenduse paketid][1]

### <a name="applications"></a>Rakenduste

Rakenduse pakett sisaldab ühte või rohkem rakenduse paketid ja määrab rakenduse konfigureerimise võimalusi. Näiteks rakenduse saate määrata vaikimisi rakenduse paketi versioon installida Arvuta sõlmed ja kas selle pakettide saate värskendada või kustutatud.

### <a name="application-packages"></a>Rakenduse paketid

Rakendusepaketi on ZIP-faili, mis sisaldab rakenduse kahendfaile ja täiendavad faile, mis on vajalik, ülesannete täitmise. Iga rakendusepaketi tähistab mõne kindla rakenduse versioon.

Saate määrata rakenduse paketid rakenduskausta ja tööülesande tasemel. Rakenduskausta või tööülesande loomisel saate määrata ühe või mitme need paketid ja (vajadusel) versioon.

* **Rakenduskausta rakenduse paketid** on juurutatud *igal* pool sõlm. Rakendused on juurutatud sõlm ühendab on ja kui see on rebooted või reimaged.

    Rakenduskausta rakenduse paketid on sobiv, kui kõik sõlmed pargis on töö ülesannete täitmiseks. Kui loote on, ja te saate lisada või värskendada olemasoleva kausta pakettide saate määrata ühe või mitme rakenduse paketid. Kui värskendate olemasoleva kausta rakenduse paketid, peate selle sõlmed uue paketi installimiseks.

* **Tööülesande taotluse paketid** on juurutatud ainult MSDN, mis on ajastatud ülesanne, just enne tööülesande käsurea töötab. Kui määratud rakendusepaketi ja versioon on juba sõlme, see on ei ja olemasolev pakett on kasutada.

    Tööülesande taotluse paketid on kasulikud ühiskasutusse antud kausta keskkonnas, kus eri töid käitatakse ühe kausta ja kausta ei kustutata, kui tööd on lõpule viidud. Kui teie töö on väiksem kui sõlmed tööülesannete kogumi, saate tööülesande rakenduse paketid minimeerida andmete edastamiseks Kuna rakenduse juurutatakse sõlmed, mis töötavad tööülesanded.

    Teiste stsenaariumide, mis saavad tööülesande rakenduse paketid on tööd, mis on eriti suur rakendust, kuid tööülesannete väike arv. Näiteks eelnevalt töötlemiseks esitusala või Ühenda tööülesande, kus eelse töötlemise või Ühenda rakendus on raskekaalu.

> [AZURE.IMPORTANT] On piiratud arv rakendusi ja rakenduse paketid paketi konto kui ka ülemmäära paketi suurus. [Kui ka limiidid Azure'i paketi teenuse](batch-quota-limit.md) kohta vaadake teavet need piirangud.

### <a name="benefits-of-application-packages"></a>Rakenduse paketid eelised

Rakenduse paketid saate lihtsustada teie paketi lahendus koodi ja vähendage pea kohal, tööülesannete töötavad rakenduste haldamiseks nõutav.

Teie pool start tööülesande pole pikk loendi üksikute ressurss faile installida sõlmed määramiseks. Te ei pea käsitsi hallata mitut versiooni rakenduste failide Azure Storage või oma sõlmed. Ning te ei pea muretsema failide salvestusruumi konto juurdepääsu [SAS URL-ide](../storage/storage-dotnet-shared-access-signature-part-1.md) genereerimine. Paketi töötab taustal Azure Storage salvestada rakenduse paketid ja neid arvutada sõlmed juurutamine.

## <a name="upload-and-manage-applications"></a>Üles- ja rakenduste haldamine

Saate kasutada [Azure portaali] [ portal] või [Paketi halduse .NET](batch-management-dotnet.md) teegi rakenduse paketid teie paketi konto haldamine. Mõni järgmistest jaotistest me esmalt link salvestusruumi konto ja seejärel arutada, lisades rakendusi ja pakettide ja hallata neid portaalis.

### <a name="link-a-storage-account"></a>Salvestusruumi konto linkimine

Rakenduse paketid kasutamiseks tuleb esmalt Azure Storage konto paketi kontoga linkida. Kui see pole veel konfigureeritud salvestusruumi konto paketi konto jaoks, kuvatakse Azure portaali esimest korda olete klõpsanud **rakenduste** **paketi konto** tera hoiatus.

> [AZURE.IMPORTANT] Partii praegu toetab *ainult* **Üldine otstarve** salvestusruumi konto tüüp, nagu on kirjeldatud juhises 5, [Loo konto salvestusruumi](../storage/storage-create-storage-account.md#create-a-storage-account) [kohta Azure salvestusruumi kontod](../storage/storage-create-storage-account.md). Kui link Azure Storage konto teie paketi konto, link *ainult* **Üldine otstarve** salvestusruumi konto.

![Salvestusruumi konto pole konfigureeritud hoiatus Azure'i portaalis][9]

Paketi teenuse kasutab hoidmiseks ja rakenduse paketid otsinguga seotud salvestusruumi konto. Pärast seda, kui olete lingitud kaks kontot, saate paketi juurutada automaatselt lingitud salvestusruumi konto, mida soovite oma Arvuta sõlmed talletatud paketid. Klõpsake nuppu **Kontosätted salvestusruumi** **hoiatus** enne ja klõpsake **Salvestusruumi konto** **Salvestusruumi konto** enne salvestusruumi kontoga linkimiseks paketi kontole.

![Valige salvestusruumi konto blade Azure'i portaalis][10]

Soovitatav on salvestusruumi konto *spetsiaalselt* kasutamiseks kontol paketi loomine ning valige see siin. Lisateavet salvestusruumi konto loomise kohta leiate teemast "Salvestusruumi konto loomine" [kohta Azure salvestusruumi kontod](../storage/storage-create-storage-account.md). Pärast seda, kui olete loonud salvestusruumi konto, saate siis linkida selle konto paketi tera **Salvestusruumi konto** abil.

> [AZURE.WARNING] Kuna paketi kasutab Azure Storage salvestada oma rakenduse paketid, olete [lisandu tavapäraselt] [ storage_pricing] plokk bloobimälu andmete jaoks. Kindlasti arvesse võtta, suurust ja teie rakenduse paketid arv ja perioodiliselt eemaldada aegunud pakette maksumus minimeerimiseks.

### <a name="view-current-applications"></a>Kuva praeguse rakendused

Teie paketi konto rakenduste kuvamiseks klõpsake nuppu vasakpoolses menüüs **rakendused** menüükäsk **paketi konto** tera kuvamisel.

![Rakenduste paan][2]

Avatakse **rakenduste** tera:

![Rakenduste loendist][3]

**Rakenduste** tera kuvatakse iga rakenduste ID-d teie konto ja järgmised atribuudid:

* **Pakettide**--funktsiooni versioonide arv.
* **Vaikimisi versioon**--versioon, kui te ei määra versioon kui seate taotlus on installitud. See säte on valikuline.
* **Luba värskendused**– väärtus, mis määrab, kas pakett värskendab, kustutamised ja täiendamine on lubatud. Kui see on seatud **ei**, keelatakse paketi värskendused ja kustutamised rakenduse. Saab lisada ainult uue rakenduse paketi versioonid. Vaikesäte on **Jah**.

### <a name="view-application-details"></a>Rakenduse üksikasjade kuvamine

Klõpsake rakenduse **rakenduste** tera tera, mis sisaldab teavet rakenduse avamiseks.

![Rakenduse üksikasjad][4]

Rakenduse üksikasjade tera, saate rakenduse konfigureerida järgmisi sätteid.

* **Luba värskendused**– saate määrata, kas selle rakenduse paketid värskendatud või kustutatud. Selle artikli teemast "Värskendada või kustutada rakendusepaketi".
* **Vaikeversiooniks**--määrata vaikimisi rakendusepaketi juurutamiseks arvutada sõlmed.
* **Kuvatav nimi**--"sõbralik" nime, mida teie paketi lahenduse saate kasutada siis, kui see kuvab teavet rakenduse, näiteks teenus, mis teie paketi klientide esitate UI määrata.

### <a name="add-a-new-application"></a>Uue rakenduse lisamine

Saate luua uue rakenduse, lisada rakendusepaketi ja määrake uue, kordumatu rakenduste ID-ga. Rakendusepaketi lisate uue rakenduse ID loob uue rakenduse.

**Rakenduste** enne **uue rakenduse** tera avamiseks klõpsake nuppu **Lisa** .

![Uus rakendus blade Azure'i portaalis][5]

**Uus rakendus** tera annab teie uus rakendus ja rakendusepaketi sätete määramiseks järgmised väljad.

**Rakenduse id**

Sellel väljal saate määrata uue rakenduse, mille on standardne Azure'i paketi ID valideerimisreeglite ID-d.

* Võivad sisaldada tähtedest ja numbritest koosnev märki, sh sidekriipse ja allakriipsutatud märkideks.
* Ei tohi sisaldada rohkem kui 64 märgist.
* Peab olema kordumatu paketi kontol.
* On juhul säilitamise ja juhul tundlik.

**Versioon**

Määrab üleslaadimiseks rakendusepaketi versiooni. Versiooni stringide kehtivad järgmised reeglid kinnitamine:

* Võivad sisaldada tähtedest ja numbritest koosnev märki, sh sidekriipsu, allakriipsutatud märkideks ja perioodid.
* Ei tohi sisaldada rohkem kui 64 märgist.
* Peab olema kordumatu rakendusest.
* Iga säilitamine ja tõstutundlikud.

**Rakendusepaketi**

Sellel väljal saate määrata, ZIP-faili, mis sisaldab rakenduse kahendfaile ja täiendavad faile, mis on nõutav rakenduse käivitada. Klõpsake väljal **Faili valimiseks** või otsige sirvides üles ja valige ZIP-faili, mis sisaldab teie taotlus faile kaustaikooni.

Kui olete valinud soovitud faili, klõpsake nuppu **OK** Azure Storage üleslaadimise alustamiseks. Kui üleslaadimine toiming on lõpule jõudnud, teavitatakse teid ja tera suletakse. Sõltuvalt faili üleslaadimisel suurust ja kiiruse võrguühendus, võib see toiming veidi aega võtta.

> [AZURE.WARNING] Enne üleslaadimise toiming on lõpule jõudnud, sulgege tera **uue rakenduse** . Seda tehes katkestab üleslaadimine.

### <a name="add-a-new-application-package"></a>Lisage uus rakendusepaketi

Uus rakendus paketi versioon taotluse lisamiseks valige rakendus **rakenduste** tera, klõpsake **pakettide**, siis avamiseks klõpsake nuppu **Lisa** **Lisa paketi** tera.

![Lisage rakenduse paketi blade Azure'i portaalis][8]

Nagu näete, väljad kattuvad tera **uue rakenduse** , kuid välja **rakenduse id** on keelatud. Nagu tegite uue rakenduse, määrake oma uue paketi **versioon** sirvides oma **rakendusepaketi** ZIP-faili ja seejärel klõpsake nuppu **OK** , et paketi üleslaadimine.

### <a name="update-or-delete-an-application-package"></a>Värskendada või rakendusepaketi kustutamine

Värskendada või kustutage olemasolev rakendusepaketi, avage rakenduse üksikasjade tera, klõpsake **pakettide** **pakettide** tera avada, klõpsake **kolmikpunkti** rea rakendusepaketi, mida soovite muuta, ja valige toiming, mida soovite teha.

![Värskendada või Azure'i portaalis paketi kustutamine][7]

**Värskendamine**

Kui klõpsate nuppu **Värskenda**, kuvatakse *pakett* tera. Selle tera on sarnane *uue rakendusepaketi* tera, kuid ainult paketi valiku väli on lubatud, mis võimaldab teil määrata uue ZIP faili üles laadida.

![Värskendage paketi blade Azure'i portaalis][11]

**Kustutamine**

Kui klõpsate nuppu **Kustuta**, küsitakse paketi versiooni kustutamise kinnitamiseks ja paketi kustutab paketi Azure Storage. Kui kustutate rakenduse vaikeversiooniks, eemaldatakse **vaikeversiooniks** sätte rakenduse.

![Rakenduse kustutamine][12]

## <a name="install-applications-on-compute-nodes"></a>Arvuta sõlmed rakenduste installimine

Nüüd, kui olete nüüd proovinud, kuidas hallata rakenduse paketid Azure'i portaalis, saame arutada juurutamise need arvutada sõlmed ja nende ülesannetega paketi käivitamine.

### <a name="install-pool-application-packages"></a>Rakenduskausta rakenduse paketid installimine

Kõik Arvuta sõlmed pargis rakendusepaketi installimiseks määrata ühe või mitme rakenduse paketi *Viited* mõeldud pool. Rakenduse paketid on määratud on installitud iga MSDN sõlme ühendab pool ja kui sõlme on rebooted või reimaged.

Paketi .net-i, saate määrata ühe või mitme [CloudPool][net_cloudpool]. [ApplicationPackageReferences] [net_cloudpool_pkgref] kui loote uue kausta või mõne olemasoleva kausta. [ApplicationPackageReference] [ net_pkgref] klassi määrab rakenduse ID ja versiooni installimiseks on pool arvutada sõlmed.

```csharp
// Create the unbound CloudPool
CloudPool myCloudPool =
    batchClient.PoolOperations.CreatePool(
        poolId: "myPool",
        targetDedicated: "1",
        virtualMachineSize: "small",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

// Specify the application and version to install on the compute nodes
myCloudPool.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference {
        ApplicationId = "litware",
        Version = "1.1001.2b" }
};

// Commit the pool so that it's created in the Batch service. As the nodes join
// the pool, the specified application package will be installed on each.
await myCloudPool.CommitAsync();
```

>[AZURE.IMPORTANT] Kui ka rakenduse paketi juurutamine nurjus mingil põhjusel, paketi teenuse märgib sõlm [kasutuskõlbmatuks][net_nodestate], ja ükski toiming on kavandatud täitmise sõlme. Sel juhul tuleks **taaskäivitage** sõlme sõltub paketi juurutamise. Taaskäivitada sõlme võimaldab ka ülesande ajastamine uuesti sõlme.

### <a name="install-task-application-packages"></a>Tööülesande taotluse pakettide installimine

Sarnaselt on, saate määrata rakenduse paketi *Viited* tööülesande. Kui sõlm on ajastatud ülesanne, paketi alla ja ekstraktimist just enne tööülesande käsurea on täidetud. Kui määratud paketi ja versioon on juba installitud sõlme, paketi laaditakse alla ja olemasoleva paketi kasutatakse.

Tööülesande taotluse paketi installimiseks konfigureerimine ülesande [CloudTask][net_cloudtask]. [ApplicationPackageReferences] [net_cloudtask_pkgref] atribuut:

```csharp
CloudTask task =
    new CloudTask(
        "litwaretask001",
        "cmd /c %AZ_BATCH_APP_PACKAGE_LITWARE%\\litware.exe -args -here");

task.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference
    {
        ApplicationId = "litware",
        Version = "1.1001.2b"
    }
};
```

## <a name="execute-the-installed-applications"></a>Installitud rakendusi käivitada

Alla laaditakse ja ekstraktimist nimega kataloogi raames pakette, mis teie määratud kausta või tööülesande jaoks soovitud `AZ_BATCH_ROOT_DIR` sõlme. Paketi loob ka keskkonna muutuja, mis sisaldab nimega kausta tee. Kui viitamine rakendus sõlme, kasutage oma tööülesande käsk read muutuja keskkonnas. Muutuja on järgmises vormingus:

`AZ_BATCH_APP_PACKAGE_APPLICATIONID#version`

`APPLICATIONID`ja `version` on väärtused, mis vastavad teie määratud juurutamiseks rakendus ja paketi versioon. Näiteks kui teie määratud versiooni 2.7 rakenduse *seguri* peaks olema installitud, teie tööülesande käsk read kasutada muutuja keskkonnas oma failidele juurde pääseda:

`AZ_BATCH_APP_PACKAGE_BLENDER#2.7`

Kui määrate vaikimisi versioon rakenduse, jätate järelliite versioon. Näiteks kui seate "2.7" vaikeversiooniks rakenduse *seguri*, tööülesannete viitamiseks järgmised keskkonna muutuja ja need käivitatakse versioon 2.7:

`AZ_BATCH_APP_PACKAGE_BLENDER`

Järgmised koodilõigu kuvatud näide tööülesande käsk rida, mis käivitab *seguri* rakenduse vaikeversiooniks.

```csharp
string taskId = "blendertask01";
string commandLine =
    @"cmd /c %AZ_BATCH_APP_PACKAGE_BLENDER%\blender.exe -args -here";
CloudTask blenderTask = new CloudTask(taskId, commandLine);
```

> [AZURE.TIP] Vaadake [keskkonna sätteid ülesannete jaoks](batch-api-basics.md#environment-settings-for-tasks) [paketi funktsioon ülevaade](batch-api-basics.md) Lisateavet Arvuta sõlm keskkonna sätteid.

## <a name="update-a-pools-application-packages"></a>Mõne kausta rakenduse paketid värskendamine

Kui mõne olemasoleva kausta on juba konfigureeritud rakendusepaketi, saate määrata uue paketi mõeldud pool. Kui te ei määra uue paketi viite mõeldud pool, kohaldatakse järgmist:

* Kõigi uute sõlmed, mis pool liitumine ja mis tahes olemasoleva sõlm, mis on rebooted või reimaged installib äsja määratud pakett.
* Arvutage sõlmed, mis on juba pool, kui värskendate paketti viiteid ei saa installida automaatselt uue rakendusepaketi. Need arvutada sõlmed tuleb rebooted või saada uue paketi reimaged.
* Uue paketi juurutamisel kajastuvad muutujate keskkond, mis on loodud uue rakenduse paketi viited.

Selles näites on olemasoleva kausta *seguri* konfigureerida oma [CloudPool]rakenduse versioon 2.7[net_cloudpool]. [ApplicationPackageReferences] [net_cloudpool_pkgref]. Määrake selle kausta sõlmed 2.76b versiooni värskendamiseks uue [ApplicationPackageReference] [ net_pkgref] uus versioon ja muudatuse kinnitamine.

```csharp
string newVersion = "2.76b";
CloudPool boundPool = await batchClient.PoolOperations.GetPoolAsync("myPool");
boundPool.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference {
        ApplicationId = "blender",
        Version = newVersion }
};
await boundPool.CommitAsync();
```

Nüüd, kui uus versioon on konfigureeritud, on *Uus* sõlm, mis ühendab pool versiooni 2.76b juurutatud seda. Installida 2.76b sõlmed, mis on *juba* kogumi, taaskäivitage või reimage neid. Pange tähele, et rebooted sõlmed säilivad eelmise paketi juurutuste faile.

## <a name="list-the-applications-in-a-batch-account"></a>Paketi konto rakenduste loendis.

Saate loetleda rakendusi ja paketipõhise paketi konto abil [ApplicationOperations][net_appops]. [ListApplicationSummaries] [net_appops_listappsummaries] meetod.

```csharp
// List the applications and their application packages in the Batch account.
List<ApplicationSummary> applications = await batchClient.ApplicationOperations.ListApplicationSummaries().ToListAsync();
foreach (ApplicationSummary app in applications)
{
    Console.WriteLine("ID: {0} | Display Name: {1}", app.Id, app.DisplayName);

    foreach (string version in app.Versions)
    {
        Console.WriteLine("  {0}", version);
    }
}
```

## <a name="wrap-up"></a>Kokkuvõte

Rakenduse paketid, aitab teil oma klientidele, valige rakendused oma töö ja määrake täpse versiooni kasutada, kui teie paketi lubatud teenusega töid. Saate luua ka võimalus üles laadida ja jälgida oma rakenduste teenust oma klientidele.

## <a name="next-steps"></a>Järgmised sammud

* [Paketi REST API] [ api_rest] toetatakse ka töötamine rakenduse paketid. Näiteks vaadata [applicationPackageReferences] [ rest_add_pool_with_packages] element [on pool konto] lisamine[ rest_add_pool] teavet selle kohta, kuidas määrata pakettide installimiseks REST API abil. Lugege teemat [rakenduste] [ rest_applications] täpsemat teavet rakenduse teabe saamiseks paketi REST API abil.

* Siit saate teada, kuidas programmiliselt [Azure'i paketi kontod ja kvootide paketi halduse .net-i haldamine](batch-management-dotnet.md). [Paketi halduse .NET] [ api_net_mgmt] teegi saate lubada loomine ja kustutamine funktsioonid teie paketi rakenduse või teenuse.

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_mgmt]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_mgmt_nuget]: https://www.nuget.org/packages/Microsoft.Azure.Management.Batch/
[github_samples]: https://github.com/Azure/azure-batch-samples
[storage_pricing]: https://azure.microsoft.com/pricing/details/storage/
[net_appops]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationoperations.aspx
[net_appops_listappsummaries]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationoperations.listapplicationsummaries.aspx
[net_cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_cloudpool_pkgref]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.applicationpackagereferences.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudtask.aspx
[net_cloudtask_pkgref]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudtask.applicationpackagereferences.aspx
[net_nodestate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.state.aspx
[net_pkgref]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationpackagereference.aspx
[portal]: https://portal.azure.com
[rest_applications]: https://msdn.microsoft.com/library/azure/mt643945.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[rest_add_pool_with_packages]: https://msdn.microsoft.com/library/azure/dn820174.aspx#bk_apkgreference

[1]: ./media/batch-application-packages/app_pkg_01.png "Rakenduse pakettide üksikasjalik skeem"
[2]: ./media/batch-application-packages/app_pkg_02.png "Azure'i portaalis rakenduste paan"
[3]: ./media/batch-application-packages/app_pkg_03.png "Rakenduste blade Azure'i portaalis"
[4]: ./media/batch-application-packages/app_pkg_04.png "Rakenduse üksikasjade blade Azure'i portaalis"
[5]: ./media/batch-application-packages/app_pkg_05.png "Uus rakendus blade Azure'i portaalis"
[7]: ./media/batch-application-packages/app_pkg_07.png "Värskendada või kustutada pakettide Azure portaali rippmenüü"
[8]: ./media/batch-application-packages/app_pkg_08.png "Uue rakenduse paketi blade Azure'i portaalis"
[9]: ./media/batch-application-packages/app_pkg_09.png "Lingitud salvestusruumi konto hoiatust"
[10]: ./media/batch-application-packages/app_pkg_10.png "Valige salvestusruumi konto blade Azure'i portaalis"
[11]: ./media/batch-application-packages/app_pkg_11.png "Värskendage paketi blade Azure'i portaalis"
[12]: ./media/batch-application-packages/app_pkg_12.png "Kustutage dialoogiboksi paketi kinnituse Azure'i portaalis"
