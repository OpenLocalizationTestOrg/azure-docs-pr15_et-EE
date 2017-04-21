<properties services="virtual-machines" title="Using Azure CLI with Azure Resource Manager" authors="squillace" solutions="" manager="timlt" editor="tysonn" />

<tags
   ms.service="virtual-machine"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="linux"
   ms.workload="infrastructure"
   ms.date="04/13/2015"
   ms.author="rasquill" />

## <a name="using-azure-cli-with-azure-resource-manager-arm"></a>Azure'i CLI kasutamine koos Azure ressursihaldur (ARM)

Enne ressursihaldur käsud ja mallid Azure'i CLI abil saate juurutada Azure materjale ja ressursside rühmade kasutamine töökoormus, peate konto Azure (loomulikult). Kui teil pole kontot, saate [tasuta Azure prooviversioon](https://azure.microsoft.com/pricing/free-trial/).

> [AZURE.NOTE] Kui teil pole veel Azure'i konto, kuid teil on tellimus MSDN-i tellimusse, võite saada tasuta Azure tiitrid, aktiveerige oma [MSDN-i abonendi kasu siin](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) --või saate kasutada tasuta konto. Kas töötavad Azure juurdepääsu.

### <a name="step-1-verify-the-azure-cli-version"></a>Samm 1: Kinnitage Azure'i CLI versioon

Azure'i CLI jaoks olulistel käskude ja ARM mallide kasutamiseks peate olema vähemalt versiooni 0.8.17. Kontrollige oma versiooni, tippige `azure --version`. Peaksite nägema umbes:

    $ azure --version
    0.8.17 (node: 0.10.25)

Kui teil on vaja versiooni värskendamiseks Azure'i CLI leiate [Azure'i CLI](https://github.com/Azure/azure-xplat-cli).

### <a name="step-2-verify-you-are-using-a-work-or-school-identity-with-azure"></a>Samm 2: Veenduge, et kasutate töö või kooli identiteedi ja Azure

ARM käsk režiimi saate kasutada ainult siis, kui kasutate on [Azure Active Directory rentniku](https://msdn.microsoft.com/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant) või [Teenuse põhilise nime](https://msdn.microsoft.com/library/azure/dn132633.aspx). (Need on ehk *ettevõtte ID-d*.)

Vaadake, kui teil on üks, logige sisse, tippides `azure login` ja kasutades oma töö või kooli kasutajanimi ja parool, kui seda küsitakse. Kui teil on üks, peaksite nägema umbes järgmine:

    $ azure login
    info:    Executing command login
    warn:    Please note that currently you can login only via Microsoft organizational account or service principal. For instructions on how to set them up, please read http://aka.ms/Dhf67j.
    Username: ahmet@contoso.onmicrosoft.com
    Password: *********
  	|info:    Added subscription Visual Studio Ultimate with MSDN
    info:    Setting subscription Visual Studio Ultimate with MSDN as default
    info:    Added subscription Azure Free Trial
    +
    info:    login command OK

Kui te ei näe seda, peate looma uue rentniku (või teenuse põhisumma) oma Microsofti konto identiteedi. (See on sageli isikliku MSDN-i abonemendid või tasuta prooviversiooni tellimuste puhul.) Luua töö või kooli loodud ID-ga Microsoft Azure'i konto id, lugege teemat [seostada Azure AD kataloogi Azure'i uude tellimusse](https://msdn.microsoft.com/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant). Kui arvate, et teil peaks olema ettevõtte id juba, peate konto teie jaoks loonud isiku rääkida.

### <a name="step-3-choose-your-azure-subscription"></a>Samm 3: Valige Azure tellimuse

Kui teil on ainult üks tellimuse Azure'i kontosse, Azure'i CLI seostab end tellimuse vaikimisi. Kui teil on mitu tellimust, peate valige tellimus, mida soovite kasutada, tippides `azure account set <subscription id or name> true` kus _tellimuse id või nimi_ on tellimuse id või tellimuse nime, mida soovite praeguse seansi töötamiseks.

Peaksite nägema umbes järgmine:

    $ azure account set "Azure Free Trial" true
    info:    Executing command account set
    info:    Setting subscription to "Azure Free Trial" with id "2lskd82-434-4730-9df9-akd83lsk92sa".
    info:    Changes saved
    info:    account set command OK

### <a name="step-4-place-your-azure-cli-in-the-arm-mode"></a>Samm 4: Paigutada oma Azure'i CLI ARM režiimis

Azure'i CLI Azure'i Resource Management (ARM) režiimis kasutamiseks tippige `azure config mode arm`. Peaksite nägema umbes järgmine:

    $ azure config mode arm
    info:    New mode is arm

> [AZURE.NOTE] Mida saab vahetada uuesti kasutada Azure teenuse haldamise käske, tippides `azure config mode asm`.
