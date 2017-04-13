<properties
    pageTitle="Ressursside haldus – paketi halduse .net-i konto | Microsoft Azure'i"
    description="Luua, kustutada ja muuta Azure'i paketi konto ressursid teegiga paketi halduse .net-i."
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="big-compute"
    ms.date="10/19/2016"
    ms.author="marsma"/>

# <a name="manage-azure-batch-accounts-and-quotas-with-batch-management-net"></a>Azure'i paketi kontod ja kvootide paketi halduse .net-i haldamine

> [AZURE.SELECTOR]
- [Azure'i portaal](batch-account-create-portal.md)
- [Paketi halduse .net-i](batch-management-dotnet.md)

Võite vähendada hooldustööde kohal oma Azure'i paketi rakendused [Paketi halduse .net-i] abil[ api_mgmt_net] teegi paketi loomine, kustutamine, võtme haldamise ja kvoodi discovery automatiseerimiseks.

- **Loomine ja kustutamine paketi kontod** mis tahes piirkonnas. Kui kui ka sõltumatute tarkvara müüja (ISV) näiteks saate pakkuda oma klientidele, kus iga määratud eraldi paketi konto arvete koostamiseks, saate lisada oma kliendi portaali konto loomine ja kustutamine võimalusi.
- **Laadi alla ja uuesti luua konto võtmed** programmiliselt paketi kontot. See aitab teil vastavad turbepoliitikate, mis kehtestavad perioodiliste edastamiseks või konto klahvid lõppu. Kui teil on mitu paketi kontod Azure eri piirkondade, suurendab automatiseerimise edastamiseks selle protsessi teie lahendus tõhusust.
- **Märkige ruut konto kvootide** ja võta prooviversiooni-vea ebaselgust välja kindlaks teha, millised paketi kontodel on millised piirangud. Kontrollides oma konto enne alustamist tööde haldamine, kaustu luua ja lisada Arvuta sõlmed, kus saate aktiivselt reguleerida või kui need arvutada ressursid on loodud. Saate määrata, mis moodustab nõua kvoodi suureneb enne nende aruannete eraldamise.
- Kõiki võimalusi pakkuva halduse kogemus – paketi halduse .net-i, [Azure Active Directory]abil **ühendada muude Azure'i teenuste funktsioonide** [aad_about], ja [Azure ressursihaldur] [ resman_overview] koos sama rakenduse. Nende funktsioonide ja nende API-de abil saate sisestada frictionless autentimise kogemuse, võimalus ressursi rühmad ja võimalusi, mida on kirjeldatud ülal lõpuni halduse lahenduse loomine ja kustutamine.

> [AZURE.NOTE] Kuigi see artikkel keskendub programmiline haldamise paketi kontod, võtmed ja kvootide, saate teha paljude nende toimingute abil [Azure portaali][azure_portal]. Lisateabe saamiseks leiate [Azure'i paketi konto abil Azure portaali](batch-account-create-portal.md) ja [kvootide ja Azure'i paketi teenuse piirangud](batch-quota-limit.md).

## <a name="create-and-delete-batch-accounts"></a>Loomine ja kustutamine paketi kontod

Nagu mainitud, on üks pakett haldamine API esmane funktsioonid on loomine ja kustutamine Azure piirkonnas paketi kontod. Kasutage selleks [BatchManagementClient.Account.CreateAsync] [ net_create] ja [DeleteAsync][net_delete], nende sünkroonse kolleegid.

Järgmised koodilõigu loob konto, hangib paketi teenuse uue konto ja seejärel kustutab selle. See koodilõigu ja teisi selle artikli `batchManagementClient` on täielikult lähtestatud eksemplari [BatchManagementClient][net_mgmt_client].

```csharp
// Create a new Batch account
await batchManagementClient.Account.CreateAsync("MyResourceGroup",
    "mynewaccount",
    new BatchAccountCreateParameters() { Location = "West US" });

// Get the new account from the Batch service
AccountResource account = await batchManagementClient.Account.GetAsync(
    "MyResourceGroup",
    "mynewaccount");

// Delete the account
await batchManagementClient.Account.DeleteAsync("MyResourceGroup", account.Name);
```

> [AZURE.NOTE] Rakendustes, mis kasutavad paketi halduse .NET teek ja BatchManagementClient klassi vaja **administraator** või **coadministrator** juurdepääsu tellimuse omanik paketi konto, mida soovite hallata. Lisateabe saamiseks lugege teemat [Azure Active Directory](#azure-active-directory) jaotis ja [AccountManagement] [ acct_mgmt_sample] proovi kood.

## <a name="retrieve-and-regenerate-account-keys"></a>Laadi alla ja uuesti luua konto võtmed

Saada esmaseid ja teiseseid konto klahvid paketi konto tellimuse abil [ListKeysAsync][net_list_keys]. Saate uuesti looma nende klahvide abil [RegenerateKeyAsync][net_regenerate_keys].

```csharp
// Get and print the primary and secondary keys
BatchAccountListKeyResult accountKeys =
    await batchManagementClient.Account.ListKeysAsync(
        "MyResourceGroup",
        "mybatchaccount");
Console.WriteLine("Primary key:   {0}", accountKeys.Primary);
Console.WriteLine("Secondary key: {0}", accountKeys.Secondary);

// Regenerate the primary key
BatchAccountRegenerateKeyResponse newKeys =
    await batchManagementClient.Account.RegenerateKeyAsync(
        "MyResourceGroup",
        "mybatchaccount",
        new BatchAccountRegenerateKeyParameters() {
            KeyName = AccountKeyType.Primary
            });
```

> [AZURE.TIP] Sujuv ühendus töövoo saate luua oma rakenduste haldamine. Saada esmalt konto alus paketi konto, mida soovite hallata [ListKeysAsync][net_list_keys]. Seejärel kasutage selle klahvi paketi .NET teegi [BatchSharedKeyCredentials] lähtestamisel[ net_sharedkeycred] klassi, mida kasutatakse [BatchClient]lähtestamisel[net_batch_client].

## <a name="check-azure-subscription-and-batch-account-quotas"></a>Azure'i tellimus ja paketi konto kvootide kontrollimine

Azure'i tellimused ja üksikute Azure'i teenuste nagu kõik partii on vaikimisi piirmäära, et teatud üksuste arvu piiramine. Vaikimisi kvootide Azure'i tellimused, teemast [Azure tellimuse ja teenuste piirangud, kvootide ja piiranguid](./../azure-subscription-service-limits.md). Vaikimisi kvootide paketi teenuse, vt [kvootide ja Azure'i paketi teenuse piirangud](batch-quota-limit.md). Paketi halduse .NET teegi abil saate vaadata nende kvootide rakenduste. See võimaldab teil teha eraldamise enne kontosid lisada või arvutada ressursid nagu kaustu ja arvutada sõlmed.

### <a name="check-an-azure-subscription-for-batch-account-quotas"></a>Märkige ruut Azure tellimuse jaoks paketi konto kvoote

Enne piirkonnas paketi konto loomist saate näha, kas teil on võimalik konto lisamine selle piirkonna Azure tellimuse.

Allpool koodilõigu me kasutame esmalt [BatchManagementClient.Account.ListAsync] [ net_mgmt_listaccounts] saada kõik paketi kontod, mis jäävad tellimuse kogum. Kui me olen selle saidikogumi saadud, me määratleda piirkonna sihtkoht on mitme kontoga. Siis kasutame [BatchManagementClient.Subscriptions] [ net_mgmt_subscriptions] hankida paketi konto kvoodi ja selle piirkonna saab luua mitu kontod (vajaduse korral).

```csharp
// Get a collection of all Batch accounts within the subscription
BatchAccountListResponse listResponse =
        await batchManagementClient.Account.ListAsync(new AccountListParameters());
IList<AccountResource> accounts = listResponse.Accounts;
Console.WriteLine("Total number of Batch accounts under subscription id {0}:  {1}",
    creds.SubscriptionId,
    accounts.Count);

// Get a count of all accounts within the target region
string region = "westus";
int accountsInRegion = accounts.Count(o => o.Location == region);

// Get the account quota for the specified region
SubscriptionQuotasGetResponse quotaResponse = await batchManagementClient.Subscriptions.GetSubscriptionQuotasAsync(region);
Console.WriteLine("Account quota for {0} region: {1}", region, quotaResponse.AccountQuota);

// Determine how many accounts can be created in the target region
Console.WriteLine("Accounts in {0}: {1}", region, accountsInRegion);
Console.WriteLine("You can create {0} accounts in the {1} region.", quotaResponse.AccountQuota - accountsInRegion, region);
```

Klõpsake eeltoodud koodilõigu `creds` on [TokenCloudCredentials]eksemplari[azure_tokencreds]. Näiteks luua selle objekti, leiate artiklist [AccountManagement] [ acct_mgmt_sample] kood valimi github.

### <a name="check-a-batch-account-for-compute-resource-quotas"></a>Märkige ruut Arvuta serveriressursikvootide paketi konto lisamine

Enne Arvuta ressursside teie paketi lahendus suurendamist, saate tagada ressursid, mida soovite eraldada ei ületa konto kvootide. Klõpsake allpool koodilõigu, saate printida kvoodi teave paketi kontot nimega `mybatchaccount`. Rakenduse, võite selline teave kindlaks teha, kas saate konto käsitlemiseks luua lisaressursid.

```csharp
// First obtain the Batch account
BatchAccountGetResponse getResponse =
    await batchManagementClient.Account.GetAsync("MyResourceGroup", "mybatchaccount");
AccountResource account = getResponse.Resource;

// Now print the compute resource quotas for the account
Console.WriteLine("Core quota: {0}", account.Properties.CoreQuota);
Console.WriteLine("Pool quota: {0}", account.Properties.PoolQuota);
Console.WriteLine("Active job and job schedule quota: {0}", account.Properties.ActiveJobAndJobScheduleQuota);
```

> [AZURE.IMPORTANT] Samas on vaikimisi kvootide Azure'i tellimused ja teenuste jaoks, paljude need piirangud tõsta väljastanud taotluse [Azure portaali][azure_portal]. Näiteks vt [kvootide ja limiidid Azure'i paketi teenuse](batch-quota-limit.md) suurendab teie paketi konto kvootide juhised.

## <a name="batch-management-net-azure-ad-and-resource-manager"></a>Partii halduse .NET, Azure AD, ja ressursihaldur

Paketi halduse .NET Raamatukogu töötamisel tavaliselt kasutada ka [Azure Active Directory] [ aad_about] (Azure AD) ja [Azure ressursihaldur][resman_overview]. Allpool valimi projekti kasutab nii Azure Active Directory ja ressursihaldur see näitab, et paketi haldamine .NET API.

### <a name="azure-active-directory"></a>Azure Active Directory

Azure'i ise kasutab Azure AD oma klientidele, teenuse administraatorid ja organisatsiooni kasutajad autentimist. Paketi halduse .NET kontekstis, kasutage Azure AD autentida tellimuse administraator või koostöö administraator. See võimaldab teegis päringu paketi teenuse ja selles artiklis kirjeldatud toimingute tegemiseks.

Proovi projekti allpool, Azure [Active Directory Authentication Library] [ aad_adal] (ADAL) kasutatakse küsi kasutajalt volituste Microsoft. Kui teenuse administraator või coadministrator mandaadi saanud, rakenduse saate päringu Azure'i tellimused--loendi ja saate luua ja kustutada nii ressursi rühmad ja paketi kontod.

### <a name="resource-manager"></a>Ressursihaldur

Paketi kontod loomisel teegiga paketi halduse .NET te tavaliselt loome nende [ressursirühm][resman_overview]. Saate luua ressursirühma programmiliselt, kasutades [ResourceManagementClient] [ resman_client] [.net-i ressursihaldur] klassi[ resman_api] teek. Või olemasoleva ressursirühm loodud varem kasutades [Azure portaali]konto lisamine[azure_portal].

## <a name="sample-project-on-github"></a>Proovi projekti github

Toimingu paketi halduse .NET vaatamiseks lugege teemat [AccountManagment] [ acct_mgmt_sample] valimi projekti github. Selle rakenduse konsooli kuvatakse loomist ja kasutamist [BatchManagementClient] [ net_mgmt_client] ja [ResourceManagementClient][resman_client]. See näitab ka Azure [Active Directory Authentication Library] kasutamist[ aad_adal] (ADAL), mis on vajalik nii kliendid.

Edukalt valimi rakenduse käivitamiseks peate registreeruma seda Azure AD Azure portaali kaudu. Järgige jaotise [lisamine rakenduse](../active-directory/active-directory-integrating-applications.md#adding-an-application) [integreerimine Azure Active Directory rakenduste] [ aad_integrate] valimi jooksul oma konto registreerimiseks on vaikimisi kataloogi. Veenduge, et valida **Kohalikke klientrakendusega** tüüpi rakendus ja saate määrata mis tahes kehtiv URL (nt `http://myaccountmanagementsample`) **Ümber suunata URI**– see pole peavad olema otse lõpp-punkti.

Pärast lisamist rakenduse, delegaadi **Juurdepääs Azure'i Teenusehaldus nimega ettevõtte** õiguste jaotises rakenduse sätted portaalis rakendusse *Windows Azure'i teenuse juhtimise API* :

![Teenuserakenduse õiguste Azure'i portaalis][2]

> [AZURE.TIP] Kui jaotises *õigused muudes rakendustes* **Windows Azure'i teenuse juhtimise API** ei kuvata, klõpsake käsku **Lisa rakendus**, valige **Windows Azure'i teenuse juhtimise API**, siis nuppu märge. Klõpsake volitatud esindaja õigused, nagu eespool kirjeldatud.

Kui olete lisanud rakenduse eespool kirjeldatud, värskendage `Program.cs` sisse [AccountManagment] [ acct_mgmt_sample] valimi projekti rakenduse URI ümber suunata ja kliendi ID-ga. Rakenduse **konfigureerimine** menüüs leiate need väärtused:

![Rakenduse konfigureerimise Azure'i portaalis][3]

[AccountManagment] [ acct_mgmt_sample] proovi taotluse näitab järgmisi toiminguid:

1. Hankida Turbeloa, Azure AD kaudu, kasutades [ADAL][aad_adal]. Kui kasutaja on pole juba sisse logitud, siis palutakse Azure volituste.
2. Saadud Azure AD Turbeloa abil luua mõne [SubscriptionClient] [ resman_subclient] päringule Azure'i kontoga seotud tellimuste loendi. See võimaldab valik, kui mitu tellimust ei leitud.
3. Valitud tellimusega seostatud mandaadi objekti loomine.
4. Looge [ResourceManagementClient] [ resman_client] mandaadi abil.
5. Kasutage [ResourceManagementClient] [ resman_client] ressursirühma loomiseks.
6. Kasutage [BatchManagementClient] [ net_mgmt_client] mitme paketi konto toimingute tegemiseks:
  - Uue ressursirühma paketi konto luua.
  - Uue konto toomine paketi teenus.
  - Uue konto konto klahvid printida.
  - Uue konto primaarvõtme taastada.
  - Kvoodi konto printida.
  - Saate printida tellimust kvoot teave.
  - Prindi kõik kontod sees tellimus.
  - Äsja loodud konto kustutada.
7. Kustutage ressursirühma.

Enne vastloodud paketi konto ja ressursside rühma kustutada saab kontrollida nii [Azure portaali][azure_portal]:

![Kus on kuvatud ressursirühm ja paketi konto Azure portaali][1]
<br />
*Azure'i portaali, kus on kuvatud uue ressursirühma ja paketi konto*

[aad_about]: ../active-directory/active-directory-whatis.md "Mis on Azure Active Directory?"
[aad_adal]: ../active-directory/active-directory-authentication-libraries.md
[aad_auth_scenarios]: ../active-directory/active-directory-authentication-scenarios.md "Azure AD autentimise stsenaariumid"
[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Rakenduste integreerimine Azure Active Directory"
[acct_mgmt_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/AccountManagement
[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_mgmt_net]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[azure_portal]: http://portal.azure.com
[azure_storage]: https://azure.microsoft.com/services/storage/
[azure_tokencreds]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.tokencloudcredentials.aspx
[batch_explorer_project]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[net_batch_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_list_keys]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.listkeysasync.aspx
[net_create]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.createasync.aspx
[net_delete]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.deleteasync.aspx
[net_regenerate_keys]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.regeneratekeyasync.aspx
[net_sharedkeycred]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.auth.batchsharedkeycredentials.aspx
[net_mgmt_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.batchmanagementclient.aspx
[net_mgmt_subscriptions]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.batchmanagementclient.subscriptions.aspx
[net_mgmt_listaccounts]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.iaccountoperations.listasync.aspx
[resman_api]: https://msdn.microsoft.com/library/azure/mt418626.aspx
[resman_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.resourcemanagementclient.aspxs
[resman_subclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.subscriptions.subscriptionclient.aspx
[resman_overview]: ../azure-resource-manager/resource-group-overview.md

[1]: ./media/batch-management-dotnet/portal-01.png
[2]: ./media/batch-management-dotnet/portal-02.png
[3]: ./media/batch-management-dotnet/portal-03.png
