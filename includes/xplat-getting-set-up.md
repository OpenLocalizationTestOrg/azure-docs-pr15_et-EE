<properties services="virtual-machines" title="Setting up Azure CLI for service management" authors="squillace" solutions="" manager="timlt" editor="tysonn" />

<tags
   ms.service="virtual-machine"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="linux"
   ms.workload="infrastructure"
   ms.date="04/13/2015"
   ms.author="rasquill" />

## <a name="using-azure-cli"></a>Azure'i CLI abil

Järgmised toimingud aidata teil Azure'i CLI hõlpsalt kasutamine kõige uuem versioon ja proper tellimuse. Kui teil on vaja installida Azure CLI ja esmalt kontoga ühendada, leiate [Azure'i käsurea liides (Azure'i CLI)](xplat-cli-install.md).

### <a name="step-1-update-azure-cli-version"></a>Samm 1: Update Azure'i CLI versioon

Azure'i CLI olulistel käskude ja teenuse haldus režiimis kasutamiseks peaks olema uuem versioon kui võimalik. Kontrollige oma versiooni, tippige `azure --version`. Peaksite nägema umbes:

    $ azure --version
    0.8.17 (node: 0.10.25)

Kui soovite versiooni värskendamiseks Azure'i CLI leiate [Azure'i CLI](https://github.com/Azure/azure-xplat-cli).

### <a name="step-2-set-the-azure-account-and-subscription"></a>Samm 2: Azure'i konto ja tellimuse seadmine

Kui teie Azure'i CLI on ühendatud kontoga, mida soovite kasutada, võib teil olla mitu tellimust. Kui te ei tee, peaksite saadaval tellimused teie konto jaoks, tippides `azure account list`, ja seejärel valige tellimus, mida soovite kasutada, tippides `azure account set <subscription id or name> true` kus _tellimuse id või nimi_ on tellimuse id või tellimuse nime, mida soovite praeguse seansi töötamiseks. Peaksite nägema umbes järgmine:

    $ azure account set "Visual Studio Ultimate with MSDN" true
    info:    Executing command account set
    info:    Setting subscription to "Visual Studio Ultimate with MSDN" with id "0e220bf6-5caa-4e9f-8383-51f16b6c109f".
    info:    Changes saved
    info:    account set command OK

> [AZURE.NOTE] Kui teil pole veel Azure'i konto, kuid teil on tellimus MSDN-i tellimusse, võite saada tasuta Azure tiitrid, aktiveerige oma [MSDN-i abonendi kasu siin](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) --või saate kasutada tasuta konto. Kas töötavad Azure juurdepääsu.
