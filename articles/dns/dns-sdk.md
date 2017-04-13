<properties 
   pageTitle="DNS-i tsoonid loomine ja salvestamine komplekti Azure'i DNS-i kasutades .NET SDK | Microsoft Azure'i" 
   description=".NET SDK abil seab Azure'i DNS-i DNS-i tsoonid ja kirje loomise kohta." 
   services="dns" 
   documentationCenter="na" 
   authors="jtuliani" 
   manager="carmonm" 
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="09/19/2016"
   ms.author="jtuliani"/>


# <a name="create-dns-zones-and-record-sets-using-the-net-sdk"></a>DNS-i tsoonid ja kasutades .NET SDK kirje komplekti loomine

Toimingute loomine, kustutamine või värskendada DNS-i tsoonid, kirje komplekti ja kirjete teegiga .net-i DNS-i haldamise DNS-i SDK abil automatiseerida. Täielik Visual Studio projekt on saadaval [siin.](https://www.microsoft.com/en-us/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True)

## <a name="create-a-service-principal-account"></a>Teenuse põhisumma konto loomine

Tavaliselt antakse Programmeerimisjuurdepääs Azure ressursside kaudu sihtotstarbeline konto, mitte oma kasutajatunnust. Sihtotstarbeline aruande nimetatakse "teenuse põhisumma" kontod. Azure'i DNS-i SDK valimi projekti kasutamiseks peate esmalt põhisumma teenusekonto loomine ja selle määramine vajalikke õigusi.

1. Järgige [neid juhiseid](../resource-group-authenticate-service-principal.md) , et põhisumma teenusele konto (Azure'i DNS-i SDK valimi projekti eeldab parooli autentimise.)

2. Ressursi rühma loomine ([siin on kuidas](../azure-portal/resource-group-portal.md)).

3. Azure'i RBAC abil saate anda põhisumma teenusekonto "DNS-i tsooni kaasautor" õigused ressursirühma ([siin on kuidas](../active-directory/role-based-access-control-configure.md).)

4. Kui Azure DNS-i SDK valimi projekt, "program.cs" faili redigeerimine järgmiselt:
    * Lisage õigete väärtuste tenantId, clientId (nimetatakse ka konto ID), salajane (teenuse põhisumma konto parool) ja subscriptionId nagu juhises 1.
    * Sisestage valitud toimingus 2 ressursi rühma nimi.
    * Sisestage DNS-tsooni nimi oma valik.

## <a name="nuget-packages-and-namespace-declarations"></a>Nugeti pakettide ja deklaratsiooni nimeruum

Azure'i DNS-i .NET SDK kasutamiseks on vaja installida **Azure DNS-i teegis** Nugeti paketi ja muude nõutav Azure paketid.
 
1. Projekti või uue projekti avamine rakenduses **Visual Studio**. 

2. Klõpsake **menüü Tööriistad** **>** **Nugeti Package Manager** **>** **haldamine Nugeti pakettide lahenduse …**. 

3. Klõpsake nuppu **Sirvi**, lubamine **väljalaske-eelne kaasata** ruut ja tippige väljale Otsing **Microsoft.Azure.Management.Dns** .

4. Valige pakett ja klõpsake **installida** Visual Studio projekti lisamiseks.
 
5. Korrake ülaltoodud installida ka järgmised protsessi: **Microsoft.Rest.ClientRuntime.Azure.Authentication** ja **Microsoft.Azure.Management.ResourceManager**.

## <a name="add-namespace-declarations"></a>Nimeruumi deklaratsiooni lisamine

Lisage järgmised nimeruum deklaratsiooni

    using Microsoft.Rest.Azure.Authentication;
    using Microsoft.Azure.Management.Dns;
    using Microsoft.Azure.Management.Dns.Models;

## <a name="initialize-the-dns-management-client"></a>DNS-i halduse kliendi lähtestamine

*DnsManagementClient* sisaldab meetodite ja DNS-i tsoonid ja kirjekomplekti haldamiseks vajalikud atribuudid.  Järgmine kood teenuse põhisumma kontole sisse ja loob DnsManagementClient objekti.

    // Build the service credentials and DNS management client
    var serviceCreds = await ApplicationTokenProvider.LoginSilentAsync(tenantId, clientId, secret);
    var dnsClient = new DnsManagementClient(serviceCreds);
    dnsClient.SubscriptionId = subscriptionId;

## <a name="create-or-update-a-dns-zone"></a>Luua või värskendada DNS-i tsooni

DNS-i tsooni loomiseks esmalt "Tsooni" objekt on loodud sisaldavad DNS-i tsooni parameetrid. Kuna DNS-i tsoonid ei ole seotud konkreetse piirkonna, asukoht on seatud "global". Selles näites on [Azure ressursihaldur "tag"](https://azure.microsoft.com/updates/organize-your-azure-resources-with-tags/) lisatakse ka tsooni.

Tegelikult luua või värskendada Azure'i DNS-i tsooni, edastatakse zone objekti sisaldava tsooni parameetrid *DnsManagementClient.Zones.CreateOrUpdateAsyc* meetod.

>[AZURE.NOTE] DnsManagementClient toetab kolme režiimi toimingu: sünkroonse ("CreateOrUpdate"), asünkroonne ("CreateOrUpdateAsync"), või asünkroonne juurdepääsu HTTP-vastuse (CreateOrUpdateWithHttpMessagesAsync").  Mõni järgmistest režiimidest, saate valida vastavalt vajadusele rakendus.

Azure'i DNS-i toetab loodetav kokkulangevus, mida nimetatakse [Etags](dns-getstarted-create-dnszone.md). Selles näites täpsustades "*" "If-pole-Match" päise ütleb Azure'i DNS-i DNS-i tsooni loomiseks, kui üks juba olemas.  Kõne nurjub, kui antud ressursirühm tsooni nimega on juba olemas.

    // Create zone parameters
    var dnsZoneParams = new Zone("global"); // All DNS zones must have location = "global"
    
    // Create a Azure Resource Manager 'tag'.  This is optional.  You can add multiple tags
    dnsZoneParams.Tags = new Dictionary<string, string>();
    dnsZoneParams.Tags.Add("dept", "finance");
    
    // Create the actual zone.
    // Note: Uses 'If-None-Match *' ETAG check, so will fail if the zone exists already.
    // Note: For non-async usage, call dnsClient.Zones.CreateOrUpdate(resourceGroupName, zoneName, dnsZoneParams, null, "*")
    // Note: For getting the http response, call dnsClient.Zones.CreateOrUpdateWithHttpMessagesAsync(resourceGroupName, zoneName, dnsZoneParams, null, "*")
    var dnsZone = await dnsClient.Zones.CreateOrUpdateAsync(resourceGroupName, zoneName, dnsZoneParams, null, "*");

## <a name="create-dns-record-sets-and-records"></a>DNS-i kirje komplekti ja kirjete loomine

DNS-i kirjete hallatakse kirjete kogumina. Kirje on kirjekomplekti sama nime ja kirjetüübiga tsoonis.  Kirje hulga nimi on sõltuv tsooni nimi, pole täielikult kvalifitseeritud DNS-i nimi.

Luua või värskendada kirje määratud, "Kirjekomplekti" parameetrite objekt on loodud ja *DnsManagementClient.RecordSets.CreateOrUpdateAsync*edasi. Kui DNS-tsooni, kus on kolm viisi toimingu: sünkroonse ("CreateOrUpdate"), asünkroonne ("CreateOrUpdateAsync"), või asünkroonne juurdepääsu HTTP-vastuse (CreateOrUpdateWithHttpMessagesAsync").

DNS-i tsoonid, sarnaselt toimingute kirje komplekti toetavad loodetav kokkulangevus.  Selles näites, kuna "If Match" ega "If pole-Match" on määratud, kirjekomplekti alati loodud.  Selle kõne kirjutab mis tahes olemasoleva kirje sama nime ja tüüpi kirje selle DNS-i tsooni määramine.

    // Create record set parameters
    var recordSetParams = new RecordSet();
    recordSetParams.TTL = 3600;

    // Add records to the record set parameter object.  In this case, we'll add a record of type 'A'
    recordSetParams.ARecords = new List<ARecord>();
    recordSetParams.ARecords.Add(new ARecord("1.2.3.4"));

    // Add metadata to the record set.  Similar to Azure Resource Manager tags, this is optional and you can add multiple metadata name/value pairs
    recordSetParams.Metadata = new Dictionary<string, string>();
    recordSetParams.Metadata.Add("user", "Mary");

    // Create the actual record set in Azure DNS
    // Note: no ETAG checks specified, will overwrite existing record set if one exists
    var recordSet = await dnsClient.RecordSets.CreateOrUpdateAsync(resourceGroupName, zoneName, recordSetName, RecordType.A, recordSetParams);

## <a name="get-zones-and-record-sets"></a>Tsoonid ja kirje väärtuse hankimine

*DnsManagementClient.Zones.Get* ja *DnsManagementClient.RecordSets.Get* meetodite tuua üksikuid alad ja kirje komplektid. Kirjekomplekti tuvastatakse nende tüüp, nimi ja need on olemas ajavööndi- ja ressursside rühma. Tsoonid tuvastatakse tema nime ja need on olemas ressursirühma.

    var recordSet = dnsClient.RecordSets.Get(resourceGroupName, zoneName, recordSetName, RecordType.A);
    
## <a name="update-an-existing-record-set"></a>Mõne olemasoleva kirjekomplekti värskendamine

DNS-i olemasoleva kirjekomplekti värskendamiseks esmalt kirjekomplekti tuua, siis kirje määramine sisu värskendamine, siis muudatuste saatmiseks.  Selles näites me määrata Etag If Match parameeter välja kirjekomplekti kaudu. Kõne nurjub, kui seadmine vahepeal kirjet samaaegseid toiming muutnud.

    var recordSet = dnsClient.RecordSets.Get(resourceGroupName, zoneName, recordSetName, RecordType.A);

    // Add a new record to the local object.  Note that records in a record set must be unique/distinct
    recordSet.ARecords.Add(new ARecord("5.6.7.8"));

    // Update the record set in Azure DNS
    // Note: ETAG check specified, update will be rejected if the record set has changed in the meantime
    recordSet = await dnsClient.RecordSets.CreateOrUpdateAsync(resourceGroupName, zoneName, recordSetName, RecordType.A, recordSet, recordSet.Etag);

## <a name="list-zones-and-record-sets"></a>Loendi alad ja kirje komplektid

Tsoonid loendi kuvamiseks kasutage *DnsManagementClient.Zones.List...* meetodid, mis toetavad loetelu kas kõiki alasid antud ressursirühm või kõiki alasid antud Azure tellimuse (üle ressursi rühmi.) Loendi kirje komplekti, kasutage *DnsManagementClient.RecordSets.List...* meetodid, mida toetavad kas loetelu kõigi kirjete komplekti antud tsooni või ainult neid teatud tüüpi kirje hulki.

Märkus Kui loetletakse tsoonid ja kirje komplekti, mis võivad nummerdatud.  Järgmises näites kujutatakse kordamiseks tulemuste lehtede kaudu. (Kunstlikult väikese lehe suurust "2" kasutatakse jõustamine Piipar; praktikas see parameeter vahele jätta ja kasutatud leheküljeformaat.)

    // Note: in this demo, we'll use a very small page size (2 record sets) to demonstrate paging
    // In practice, to improve performance you would use a large page size or just use the system default
    int recordSets = 0;
    var page = await dnsClient.RecordSets.ListAllInResourceGroupAsync(resourceGroupName, zoneName, "2");
    recordSets += page.Count();

    while (page.NextPageLink != null)
    {
        page = await dnsClient.RecordSets.ListAllInResourceGroupNextAsync(page.NextPageLink);
        recordSets += page.Count();
    }

## <a name="next-steps"></a>Järgmised sammud

Laadige alla [Azure DNS-i .NET SDK valimi projekti](https://www.microsoft.com/en-us/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True), mis sisaldab veel näiteid, kuidas kasutada Azure DNS-i .NET SDK, sh muid DNS-i kirjetüüpide näited.
