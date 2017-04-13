<properties 
    pageTitle="Liikumine ressursid uue ressursirühma | Microsoft Azure'i" 
    description="Kasutage Azure'i ressursihaldur liikumiseks ressursid uue ressursirühma või tellimusele." 
    services="azure-resource-manager" 
    documentationCenter="" 
    authors="tfitzmac" 
    manager="timlt" 
    editor="tysonn"/>

<tags 
    ms.service="azure-resource-manager" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/21/2016" 
    ms.author="tomfitz"/>

# <a name="move-resources-to-new-resource-group-or-subscription"></a>Ressursid uue ressursirühma või tellimuse juurde liikumine

See teema näitab, kuidas teisaldada ressursid uus tellimus või tellimuse sama uue ressursirühma. Saate liikuda ressursside portaali, PowerShelli, Azure'i CLI või REST API. Move toiminguid selles teemas on saadaval teile ilma abi Azure'i tugiteenuse töötajaga.

Kui otsustate, et teisaldate tavaliselt ressursse:

- Arvete koostamiseks peab ressursi live erinevate tellimus.
- Ressursi enam jagab sama elutsükli varem koos rühmitatud ressurssidena. Soovite teisaldada uue ressursirühma nii, et saate hallata selle ressursi eraldi muud ressursid.

Ressursside liikudes nii andmeallika rühma ja sihtrühm on lukus, selle toimingu käigus. Kirjutamine ja kustutada toimingud on blokeeritud rühmad kuni teisaldamist on lõpule viidud.

Ressursi asukohta ei saa muuta. Ressursi teisaldamine ainult teisaldab selle uue ressursirühma. Uue ressursirühma võib-olla mõnda muusse asukohta, kuid mis ei ressursile asukoha muutmiseks.

> [AZURE.NOTE] Selles artiklis kirjeldatakse, kuidas liikuda mõne olemasoleva Azure'is ressursid konto pakub. Kui soovite muuta jätkates töötamiseks oma olemasoleva ressursse (nt üleminekut grupikindlustusleping ette maksta) pakub Azure'i kontosse, teemast [üleminek teise pakkumise Azure tellimust](billing-how-to-switch-azure-offer.md). 

## <a name="checklist-before-moving-resources"></a>Enne ressursside kontroll-loend

On mõned olulised toimingud enne ressursi teha. Kinnitatava need tingimused, saate vältida tõrkeid.

1. Teenuse tuleb lubada võimalus teisaldamine ressursid. Siit leiate loendi all teavet, millised [teenused võimaldavad jooksva ressursid](#services-that-enable-move).
1. Lähte-kui ka tellimuste peab olemas olema sama [Active Directory rentniku](./active-directory/active-directory-howto-tenant.md)sees. Uue rentniku liikumiseks helistamine tugiteenuste osutajale.
2. Sihtkoha tellimuse peab olema registreeritud ressursi pakkuja ei teisaldataks ressursi. Kui ei, saate tõrketeate selle kohta, et **tellimus pole registreeritud ressursi tüüp**. Ilmneda võivaid probleemi ressursi teisaldamine uude tellimusse, kuid, et tellimus pole kunagi kasutatud selle ressursi tüübiga. Registreerimise oleku vaatamine ja registreerimist ressursi pakkujate kohta leiate teavet teemast [Ressursi pakkujad ja tüübid](../resource-manager-supported-services.md#resource-providers-and-types).
4. Kui te olete liigub rakenduse teenuse rakendus, on [rakenduse teenusepiirangud](#app-service-limitations)läbi vaadata.
4. Kui teil liiguvad taastamine teenustega seotud ressursid, olete läbi [taastamine teenuste piirangud](#recovery-services-limitations)
5. Kui liigub läbi klassikaline mudeli juurutatud ressursid, olete läbi [klassikaline juurutamise piirangud](#classic-deployment-limitations).

## <a name="when-to-call-support"></a>Kui helistada tugi

Saate teisaldada kõige ressursse näidatud selles teemas iseteenindusliku toimingute kaudu. Iseteeninduslik toimingute kasutamine

- Liikumine ressursihaldur ressursid.
- Liikumine klassikalises ressursside vastavalt [klassikaline juurutamise piirangud](#classic-deployment-limitations). 

Toele helistamine, kui teil on vaja:

- Uue Azure'i konto (ja Active Directory rentniku) oma ressursse liikuda.
- Klassikalises ressursside teisaldada, kuid on probleeme piirangud.

## <a name="services-that-enable-move"></a>Teenuseid, mis võimaldavad teisaldamine

Nüüd, mis võimaldavad nii uue ressursirühma ja tellimuse teenused on:

- API haldus
- Rakenduse rakendused (veebirakenduste) – lugege teemat [rakenduse teenusepiirangud](#app-service-limitations)
- Automatiseerimine
- Paketi
- CDN-ID
- Pilveteenused - vt [klassikaline juurutamise piirangud](#classic-deployment-limitations)
- Andmete Factory
- DNS-I
- DocumentDB
- Kogumite Hdinsightiga
- Asjade jaoturi
- Võtme Vault 
- Media Services
- Mobiilse kaasamine
- Teatis jaoturi
- Töö ülevaated
- Redis vahemälu
- Ajasti
- Otsing
- Teenuse siini
- Salvestusruumi
- Salvestusruumi (klassikaline) – lugege teemat [klassikaline juurutamise piirangud](#classic-deployment-limitations)
- SQL-andmebaasi server - andmebaas ja server peab asuma samas ressursirühma. SQL serveri teisaldamisel kõigi oma andmebaase teisaldatakse ka.
- Virtuaalmasinates - siiski ei toeta liikumine uus tellimus, kui selle serdid on talletatud klahvi Vault
- Virtuaalmasinates (klassikaline) – lugege teemat [klassikaline juurutamise piirangud](#classic-deployment-limitations)
- Virtuaalne võrkude

## <a name="services-that-do-not-enable-move"></a>Teenuseid, mis võimaldavad teisaldamine

Teenused, mida praegu ei luba teisaldamine ressursi on:

- Rakenduse lüüsi
- Rakenduse ülevaated
- Kiire marsruutimiseks
- Taastamise teenused võlvkelder - ka teisaldada Arvuta, võrgu ja salvestusruumi ressursse, mis on seotud taastamise teenused vault, vt [taastamine teenuste piirangud](#recovery-services-limitations).
- Klahv Vault talletatud serdiga Virtuaalmasinates
- Virtuaalmasinates skaala komplektid
- Virtuaalne võrkude (klassikaline) – lugege teemat [klassikaline juurutamise piirangud](#classic-deployment-limitations)
- VPN-lüüsi

## <a name="app-service-limitations"></a>Rakenduse teenusepiirangud

Kui töötate rakenduse rakendused, ei saa teisaldada ainult rakenduse teenuse leping. Rakenduse rakendused teisaldamiseks on koosolekute korraldamiseks vajalikke suvandeid.

- Rakenduse teenusleping ja muud rakenduse teenuse ressursid uue ressursirühma, mis pole veel rakendust Service ressursid selle ressursirühma liikumine. See tähendab teisaldate isegi rakenduse teenuse ressursse, mis ei ole seotud rakenduse teenusleping. 
- Liikumine erinevate ressursirühm rakendused, kuid pidage rakenduse teenuse lepingute algse ressursirühma.

Kui teie algne ressursirühm sisaldab ka rakenduse ülevaated on ressurss, ei saa teisaldada selle ressursi, kuna rakenduse ülevaated ei võimalda praegu teisaldamise toiming. Kui lisate rakenduse ülevaated ressursi liikudes rakendused rakendus, kogu teisaldamiseks nurjub. Siiski rakenduse ülevaated ja rakenduse teenusleping pole vaja asu sama ressursirühm rakenduse rakendusena töötaks õigesti.

Näiteks kui teie ressursirühm sisaldab:

- **Web a** mis on seotud **lepingu a** ja **rakenduse-ülevaateid-a**
- **veebi-b** , mis on seotud **lepingu-b** ja **rakenduse-ülevaateid-b**

Teie valikud on:

- Liikumine **web a**, **leping a**, **veebi-b**ja **leping-b**
- Liikumine **web a** ja **web-b**
- Liikumine **web a**
- Liikumine **web-b**

Kõigi muude kombinatsioonide kaasata teisaldamine ressursi tüüp, mida ei saa teisaldada (rakenduse ülevaated) või jättes maha ressursi tüüp, mida ei saa jäänud liikudes rakenduse teenusleping (mis tahes tüüpi rakendust Service ressursi).

Kui teie veebirakendus asub eri ressursirühm, kui selle rakenduse teenusleping, kuid soovite teisaldada, nii et uue ressursirühma, peate tegema liikuda kaks toimingut. Näiteks:

- **Web a** **web** jaotises asub
- **lepingu a** asub jaotise **kavandamine**
- Soovite **web a** ja **lepingu a** mis **kombineeritud rühma**

Selle teisaldamine täita toiminguid kahte eraldi Teisalda järgmises järjestuses:

1. Liikumine on **web a** **leping** rühmale
2. Liikumine **web a** ja **leping on** **ühendatud**rühma.

Saate teisaldada rakenduse tunnistus uue ressursirühma või tellimuse ilma küsimusi. Juhul, kui oma veebirakenduse sisaldab SSL-sert, mille ostsite väliselt ja rakenduse üles laadida, peate kustutama serdi enne veebirakenduse. Näiteks saate teha järgmist:

1. Web appi kaudu üles laaditud serdi kustutamine
2. Veebirakenduse teisaldamine
3. Veebirakenduse serdi üleslaadimine

## <a name="recovery-services-limitations"></a>Taastamine teenuste piirangud

Liikumine salvestusruum võrgus, pole lubatud või arvutada ressurssidega häälestamine avariitaastet Azure saidi taastamine. 

Oletame näiteks, et olete seadistanud oma kohapealse masinad salvestusruumi konto (Storage1) kopeerimine ja kaitstud masina tulla pärast Tõrkesiirde Azure virtuaalse masina (VM1) virtuaalse võrku (võrgustikule1) Kui soovite. Ei saa teisaldada mis tahes Azure'i ressursid - Storage1 VM1 ja võrgustikule1 - ressursside rühma sama tellimus üle ühest või mitmest tellimused.

## <a name="classic-deployment-limitations"></a>Klassikaline juurutamise piirangud

Suvandid – klassikaline juurutatud ressursside erineb sõltuvalt kas teisaldatava ressursside tellimuse või ümber uude tellimusse. 

### <a name="same-subscription"></a>Sama tellimuse

Kui ühte ressursirühma ressursside teisaldamine teise ressursirühm ühe tellimuse sees kehtivad järgmised piirangud.

- Virtuaalne võrkude (klassikaline) ei saa teisaldada.
- Virtuaalmasinates (klassikaline) peab koos pilveteenusesse teisaldada. 
- Pilveteenuses saab teisaldada ainult siis, kui liikuda sisaldab kõiki virtuaalmasinates.
- Korraga saab teisaldada ainult üks pilveteenuses.
- Korraga saab teisaldada ainult üks salvestusruumi konto (klassikaline).
- Salvestusruumi konto (klassikaline) ei saa sama toiminguga virtuaalse masina või mõnda pilveteenusesse teisaldada.

Sees ühe tellimuse uue ressursirühma klassikalises ressursside liikumiseks kasutage standardne teisaldamine toimingute kaudu [portaali](#use-portal), [Azure PowerShelli](#use-powershell), [Azure'i CLI](#use-azure-cli)või [REST API](#use-rest-api). Kasutate samu toiminguid nagu ressursihaldur ressursside kasutamiseks.

### <a name="new-subscription"></a>Uus tellimus

Liikudes ressursid ümber uude tellimusse, kehtivad järgmised piirangud.

- Kõik klassikalises ressursside tellimuse peab sama toiminguga teisaldada.
- Target (sihtkoht) tellimus ei tohi sisaldada klassikalises ressursside.
- Liikuda saab taotleda ainult eraldi REST API klassikaline liigub läbi. Standardse ressursihaldur Teisalda käsud ei tööta klassikalises ressursside liikudes ümber uude tellimusse.

Uus tellimus klassikalises ressursside liikumiseks kasutage ülejäänud toimingud, mis on omased klassikalises ressursside. Järgmiste toimingute klassikalises ressursside teisaldamiseks uude tellimusse.

1. Märkige ruut, kui andmeallika tellimuse saavad osaleda rist-tellimuse Teisalda. Kasutage järgmist toimingut:

         POST https://management.azure.com/subscriptions/{sourceSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
    
     Koosolekukutse kehasse on järgmised.

         {
           "role": "source"
         }

     Vastuse valideerimine toimingu jaoks on järgmises vormingus:

         {
           "status": "{status}",
           "reasons": [
             "reason1",
             "reason2"
           ]
         }

2. Märkige ruut, kui sihtkoht tellimuse saavad osaleda rist-tellimuse Teisalda. Kasutage järgmist toimingut:

         POST https://management.azure.com/subscriptions/{destinationSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01

     Koosolekukutse kehasse on järgmised.

         {
           "role": "target"
         }

     Vastus on sama vormindamine allika tellimuse valideerimine.

3. Kui nii tellimuste läbib kontrolli, Teisalda kõik klassikalises ressursside ühe tellimuselt teise tellimuse järgmine toiming:

         POST https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.ClassicCompute/moveSubscriptionResources?api-version=2016-04-01

    Koosolekukutse kehasse on järgmised.

         {
           "target": "/subscriptions/{target-subscription-id}"
         }

Selle toimingu käivitada mitu minutit. 

## <a name="use-portal"></a>Portaali kasutamine

Ressursid uue ressursirühma **sama tellimuse**teisaldamiseks valige ressursirühm, mis sisaldab nende ressursside ja seejärel klõpsake nuppu **Teisalda** .

![liikumine ressursid](./media/resource-group-move-resources/edit-rg-icon.png)

Või ressursse ümber **uude tellimusse**teisaldamiseks valige ressursirühm, mis sisaldab nende ressursside ja valige tellimuse redigeerimisikooni.

![liikumine ressursid](./media/resource-group-move-resources/change-subscription.png)

Valige ressursid liikumiseks ja sihtkoha ressursirühma. Kinnitage, et peate need ressursid skriptide värskendada, ja klõpsake nuppu **OK**. Kui valisite eelmises etapis redigeerimisikooni tellimus, peate valima ka sihtkoha tellimus.

![valige sihtkoht](./media/resource-group-move-resources/select-destination.png)

**Teatised**, näete teisaldamise toiming töötab.

![Teisalda oleku kuvamine](./media/resource-group-move-resources/show-status.png)

Kui see on lõpule viidud, teavitatakse teid tulemuse.

![Teisalda tulemi kuvamine](./media/resource-group-move-resources/show-result.png)

## <a name="use-powershell"></a>PowerShelli kasutamine

Olemasolevate ressursside teisaldamiseks teise ressursirühm või tellimuse käsu **Teisalda-AzureRmResource** .

Esimese näites kujutatakse üks ressurss liikumiseks uue ressursirühma.

    $resource = Get-AzureRmResource -ResourceName ExampleApp -ResourceGroupName OldRG
    Move-AzureRmResource -DestinationResourceGroupName NewRG -ResourceId $resource.ResourceId

Teine näide teisaldamise mitme ressursid uue ressursirühma.

    $webapp = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExampleSite
    $plan = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExamplePlan
    Move-AzureRmResource -DestinationResourceGroupName NewRG -ResourceId $webapp.ResourceId, $plan.ResourceId

Uus tellimus liikumiseks kaasata **DestinationSubscriptionId** parameetri väärtus.

Teil palutakse kinnitada, et soovite teisaldada määratud ressursid.

    Confirm
    Are you sure you want to move these resources to the resource group
    '/subscriptions/{guid}/resourceGroups/newRG' the resources:

    /subscriptions/{guid}/resourceGroups/destinationgroup/providers/Microsoft.Web/serverFarms/exampleplan
    /subscriptions/{guid}/resourceGroups/destinationgroup/providers/Microsoft.Web/sites/examplesite
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): y

## <a name="use-azure-cli"></a>Azure'i CLI kasutamine

Olemasolevate ressursside teisaldamiseks teise ressursirühm või tellimuse käsu **azure ressursi teisaldada** . Peate esitada ressursse liikumiseks ressursi ID-d. Saate ressursi ID-d järgmine käsk:

    azure resource list -g sourceGroup --json

Mis annab järgmises vormingus:

    [
      {
        "id": "/subscriptions/{guid}/resourceGroups/sourceGroup/providers/Microsoft.Storage/storageAccounts/storagedemo",
        "name": "storagedemo",
        "type": "Microsoft.Storage/storageAccounts",
        "location": "southcentralus",
        "tags": {},
        "kind": "Storage",
        "sku": {
          "name": "Standard_RAGRS",
          "tier": "Standard"
        }
      }
    ]

Järgmises näites on kujutatud teisaldamise salvestusruumi konto uue ressursirühma. Sisestage parameetri **-i** ressursi ID-d komaga eraldatud loend on liikuda.

    azure resource move -i "/subscriptions/{guid}/resourceGroups/sourceGroup/providers/Microsoft.Storage/storageAccounts/storagedemo" -d "destinationGroup"
    
Teil palutakse kinnitada, et soovite teisaldada määratud ressursi.

## <a name="use-rest-api"></a>REST API kasutamine

Olemasolevate ressursside teisaldamiseks teise ressursirühm või tellimuse, käivitage:

    POST https://management.azure.com/subscriptions/{source-subscription-id}/resourcegroups/{source-resource-group-name}/moveResources?api-version={api-version} 

Koosolekukutse kehasse Määrake ressursi sihtrühm ja ressursside liikuda. ÜLEJÄÄNUD teisaldamiseks kohta leiate lisateavet teemast [teisaldamine ressursid](https://msdn.microsoft.com/library/azure/mt218710.aspx).


## <a name="next-steps"></a>Järgmised sammud
- PowerShelli cmdlet-käskude tellimuse haldamise kohta leiate teemast [Azure PowerShelli kasutamine koos ressursihaldur](powershell-azure-resource-manager.md).
- Azure'i CLI käsud tellimuse haldamise kohta leiate teemast [Azure CLI koos ressursihaldur abil](xplat-cli-azure-resource-manager.md).
- Portaali funktsioonide tellimuse haldamise kohta leiate teemast [Azure portaalis haldamiseks ressursse](./azure-portal/resource-group-portal.md).
- Loogika ettevõtte oma ressursse rakendamise kohta leiate teemast [kasutamine siltide korraldamiseks oma ressursse](resource-group-using-tags.md).
