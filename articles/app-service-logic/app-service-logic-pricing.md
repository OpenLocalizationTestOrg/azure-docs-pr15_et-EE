<properties 
    pageTitle="Loogika rakenduste hinnad mudeli | Microsoft Azure'i" 
    description="Hinnad tööpõhimõte loogika rakendustes üksikasjad" 
    authors="kevinlam1" 
    manager="dwrede" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="10/12/2016"
    ms.author="klam"/>

# <a name="logic-apps-pricing-model"></a>Loogika rakenduste hinnad mudel

Azure'i loogika rakenduste võimaldab skaala ja käivitada integreerimine töövoogude pilveteenuses.  Allpool leiate täpsemat teavet arveldamine ja paketid loogika rakendused.

## <a name="consumption-pricing"></a>Tarbimine hinnad

Äsja loodud loogika rakendusi kasutada tarbimine. Loogika rakenduste tarbimine hinnakirjad mudel maksate ainult mida te kasutate.  Loogika rakendused on rakendus pole tarbimine režiimi kasutamisel.
Kõik toimingud, mis on täidetud Käivita loogika rakenduse eksemplari on Mahupõhised.

### <a name="what-are-action-executions"></a>Mis on toiming täitmised?

Loogika rakenduse määratluse iga etapp on toiming.  See hõlmab päästikute, juhtelemendi meilivoo juhised nagu tingimuste otsinguulatuste iga silmuseid teha kuni silmusmärgistite konnektorid ja kõnede levinud toiminguid.
Päästikute on eriline toiminguid, mis on loodud uue eksemplari loogika rakenduse väärtustada kindla sündmuse ilmnemisel.  On mitme erineva käitumise päästikute, mis võivad mõjutada, kuidas rakenduse loogika on Mahupõhised.

-   Lõpp-punkti **küsitlused päästik** – see päästik pidevalt küsitlused, kuni ta saab teate, mis vastab luua uue eksemplari loogika rakenduse.  Käivitab loogika rakenduste kujundaja saab konfigureerida sagedust.  Iga küsitlused taotluse isegi juhul, kui see ei luua uue eksemplari loogika rakendus, arvestatakse soovitud toimingu käivitamine.

-   **Webhook päästik** – see päästik ootab mõnda muud klienti saata selle taotluse kindla lõpp-punkti.  Iga kutse saatmiseks webhook lõpp-punkti loeb soovitud toimingu käivitamine. Taotluse ja HTTP Webhook päästik on nii webhook päästikute.

-   **Korduvus päästik** – see päästik loob loogika rakenduse põhjal Korduvus intervalli konfigureeritud käivitab uue eksemplari.  Näiteks saab konfigureerida Korduvus päästik käivitamiseks iga 3 päeva või isegi iga minut.

Päästik täitmised näha loogika rakenduste ressursi tera päästik ajaloo osas.

Kõik toimingud, mis olid täidetud, kas need on õnnestunud või nurjunud on Mahupõhised nimega soovitud toimingu käivitamine.  Toimingud, mis olid vahele tõttu tingimus on täidetud või toiminguid, mis ei käivitada, kuna loogika rakenduse lõpetada enne lõpetamist ei loeta toimingu täitmised.

Toimingud, mille jooksul silmuseid loendatakse kursis iteratsiooni kohta.  Näiteks ühe toimingu, klõpsake soovitud jaoks iga tsükkel iterating 10 üksuste loend, arvestatakse üksuste loendi (10) toimingud tsükkel (1) arvu korrutatuna arv pluss üks algatamiseks tsükkel, mis selles näites oleks (10 * 1) + 1 = 11 toimingu täitmised.

Loogika rakendusi, mis on keelatud, ei tohi olla käivitatud uued eksemplarid ja seetõttu ajal, et neil on keelatud, ei saada tasu.  Olema teadlik, et pärast loogika rakenduse keelamine võib kuluda veidi aega eksemplaride, et jõudeolekusse viimine täielikult keelamise.

## <a name="app-service-plans"></a>Rakenduse teenuse lepingud

Rakenduse teenuse lepingud peavad enam loogika rakenduse loomine.  Saate otsida ka on rakenduse teenuse kava olemasoleva loogika rakendusega.  Loogika rakenduste varem loodud koos rakenduse teenuse plaan on jätkuvalt käitumine, enne kui leping sõltuvalt valitud on saada rakendus pärast arvu igapäevane täitmised on ületanud ja ei arve toimingu täitmise arvesti abil.

Rakenduse teenuse lepingud ja nende igapäevane lubatud toimingu täitmised.

| |Vaba/jagatud/Basic|Standard|Premium|
|---|---|---|---|
|Toimingu täitmised päevas| 200|10 000|50,000|

### <a name="convert-from-consumption-to-app-service-plan-pricing"></a>Tarbimine teisendada hinnad rakenduse teenuse kavandamine

Viide on rakenduse teenuse kava tarbeks loogika rakenduse, saate lihtsalt [käivitage selle all PowerShelli skripti](https://github.com/logicappsio/ConsumptionToAppServicePlan).  Veenduge, et peate esmalt installitud [Azure PowerShelli tööriistad](https://github.com/Azure/azure-powershell) .

``` powershell
Param(
    [string] $AppService_RG = '<app-service-resource-group>',
    [string] $AppService_Name = '<app-service-name>',
    [string] $LogicApp_RG = '<logic-app-resource-group>',
    [string] $LogicApp_Name = '<logic-app-name>',
    [string] $subscriptionId = '<azure-subscription-id>'
)

Login-AzureRmAccount 
$subscription = Get-AzureRmSubscription -SubscriptionId $subscriptionId
$appserviceplan = Get-AzureRmResource -ResourceType "Microsoft.Web/serverFarms" -ResourceGroupName $AppService_RG -ResourceName $AppService_Name
$logicapp = Get-AzureRmResource -ResourceType "Microsoft.Logic/workflows" -ResourceGroupName $LogicApp_RG -ResourceName $LogicApp_Name

$sku = @{
    "name" = $appservicePlan.Sku.tier;
    "plan" = @{
      "id" = $appserviceplan.ResourceId;
      "type" = "Microsoft.Web/ServerFarms";
      "name" = $appserviceplan.Name  
    }
}

$updatedProperties = $logicapp.Properties | Add-Member @{sku = $sku;} -PassThru

$updatedLA = Set-AzureRmResource -ResourceId $logicapp.ResourceId -Properties $updatedProperties -ApiVersion 2015-08-01-preview
```

### <a name="convert-from-app-service-plan-pricing-to-consumption"></a>Teisendada rakenduse teenuse leping hinnad tarbimine

Kui soovite muuta loogika rakendus, mis sisaldab soovitud rakenduse teenuse kavandamine tarbimine mudeli seotud eemaldada rakenduse teenuse kavandada loogika rakenduse määratluse.  Seda saab teha kõne PowerShelli cmdlet-käsk:

`Set-AzureRmLogicApp -ResourceGroupName ‘rgname’ -Name ‘wfname’ –UseConsumptionModel -Force`

## <a name="pricing"></a>Hinnad

Hinnad üksikasjad leiate [Loogika rakenduste hinnad](https://azure.microsoft.com/pricing/details/logic-apps/).

## <a name="next-steps"></a>Järgmised sammud

- [Loogika rakenduste ülevaade][whatis]
- [Oma esimese loogika rakenduse loomine][create]

[pricing]: https://azure.microsoft.com/pricing/details/logic-apps/
[whatis]: app-service-logic-what-are-logic-apps.md
[create]: app-service-logic-create-a-logic-app.md

