<properties
   pageTitle="Avaldamine rakenduste üksikkasutajale Azure RemoteApp kogumis (eelvaade) | Microsoft Azure'i"
   description="Siit saate teada, kuidas saate avaldada rakenduste üksikkasutajale, mitte sõltuvalt rühmade Azure RemoteApp."
   services="remoteapp-preview"
   documentationCenter=""
   authors="piotrci"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="piotrci"/>

# <a name="publish-applications-to-individual-users-in-an-azure-remoteapp-collection-preview"></a>Avaldamine rakenduste üksikkasutajale Azure RemoteApp kogumis (eelvaade)

> [AZURE.IMPORTANT]
> Azure RemoteApp katkeb. Lugege lisateavet [teadaanne](https://go.microsoft.com/fwlink/?linkid=821148) .

Selles artiklis selgitatakse, kuidas avaldada rakenduste üksikkasutajale on Azure RemoteApp saidikogumi. See on uus funktsioon Azure RemoteApp, praegu "Privaatne eelvaade" ja saadaval ainult valige ehituseeskirjad hindamiseks.

Algselt Azure RemoteApp lubatud ainult üks viis "avaldamise" rakenduste: administraator avaldada rakenduste pilt ja oleks nähtav kõigile kasutajatele kogumist.

Stsenaarium on palju rakendusi kaasamine ühe pildi ja juurutamine ühe saidikogumi halduskulud vähendamiseks. Sageli kõik rakendused on kõigi kasutajate jaoks oluline – administraatorid soovi avaldada rakenduste üksikud kasutajad, et nad ei näe oma rakenduses kanali mittevajalike rakenduste.

See on nüüd võimalik Azure RemoteApp – praegu piiratud eelvaate funktsiooni. Siin on lühike Kokkuvõte uued funktsioonid:

1. Kogumi saate seada ühte kahel viisil:
 
  - Algne "saidikogumi mode", kus näevad kõik kasutajad kogumi kõiki avaldatud rakendused. See on vaikimisi režiimi.
  - uue "rakenduse mode", kus kasutajad näevad ainult rakendusi, mis on selgesõnaliselt määratud

2. Hetkel taotluse režiimis saab ainult lubada Azure RemoteApp PowerShelli cmdlet-käskude abil.

  - Kui valitud taotluse režiimis, ei saa hallata kasutajate määramine kogumine Azure portaali kaudu. Kasutajate määramine peab hallatakse PowerShelli cmdlet-käsud.

3. Kasutajad näevad ainult need otse avaldatud rakendusi. Siiski endiselt võib olla kasutaja, kasutades need otse opsüsteemis pilt saadaval teisi rakendusi käivitada.
  - See funktsioon ei paku turvaline lukustatud kohta; ainult on piiratud nähtavus kanali rakenduse.
  - Kui teil on vaja teostavad kasutajad rakendustest, peate selle jaoks eraldi saidikogumid kasutada.

## <a name="how-to-get-azure-remoteapp-powershell-cmdlets"></a>Kuidas saada Azure RemoteApp PowerShelli cmdlet-käsud

Proovige uusi eelvaade funktsioone, peate Azure PowerShelli cmdlet-käskude kasutamine. Praegu ei ole võimalik Azure haldusportaali abil saate lubada uue rakenduse avaldamise režiimi.

Esmalt veenduge, et teil on installitud [Azure PowerShelli mooduli](../powershell-install-configure.md) .

Seejärel käivitage PowerShelli konsooli administraatoriõigustes ja käivitage järgmine cmdlet-käsk:

        Add-AzureAccount

Teil palutakse Azure kasutajanime ja parooli. Kui olete sisse loginud, saab käitamine Azure RemoteApp Azure tellimuste suhtes.

## <a name="how-to-check-which-mode-a-collection-is-in"></a>Kuidas saan kontrollida, millist režiimi kogumik on

Käivitage järgmine cmdlet-käsk:

        Get-AzureRemoteAppCollection <collectionName>

![Märkige ruut selle saidikogumi režiim](./media/remoteapp-perapp/araacllelvel.png)

Atribuudi AclLevel võivad olla järgmised väärtused:

- Saidikogumi: algse avaldamise režiimi. Kõik kasutajad vaadata kõiki avaldatud rakendused.
- Rakendus: uus avaldamise režiimi. Kasutajad näevad ainult need otse avaldatud rakendused.

## <a name="how-to-switch-to-application-publishing-mode"></a>Kuidas rakendus avaldamise lülitumine

Käivitage järgmine cmdlet-käsk:

        Set-AzureRemoteAppCollection -CollectionName -AclLevel Application

Rakenduse avaldamise olekus säilitatakse: algselt kõik kasutajad näevad kõik algse avaldatud rakendused.

## <a name="how-to-list-users-who-can-see-a-specific-application"></a>Loendi kasutajad, kes saab vaadata mõne kindla rakenduse kohta

Käivitage järgmine cmdlet-käsk:

        Get-AzureRemoteAppUser -CollectionName <collectionName> -Alias <appAlias>

See on loetletud kõik kasutajad, kes saab vaadata rakenduse.

Märkus: Saate vaadata rakenduse pseudonüümid, (nn "rakenduse alias" ülaltoodud süntaksit) käitades Get-AzureRemoteAppProgram - CollectionName <collectionName>.

## <a name="how-to-assign-an-application-to-a-user"></a>Kuidas määrata rakenduse kasutajale

Käivitage järgmine cmdlet-käsk:

        Add-AzureRemoteAppUser -CollectionName <collectionName> -UserUpn <user@domain.com> -Type <OrgId|MicrosoftAccount> -Alias <appAlias>

Kasutaja kuvatakse nüüd rakenduse Azure RemoteApp klient ja saab ühenduse loomiseks.

## <a name="how-to-remove-an-application-from-a-user"></a>Kuidas kasutaja rakenduse eemaldamine

Käivitage järgmine cmdlet-käsk:

        Remove-AzureRemoteAppUser -CollectionName <collectionName> -UserUpn <user@domain.com> -Type <OrgId|MicrosoftAccount> -Alias <appAlias>

## <a name="providing-feedback"></a>Tagasiside
Oleme teile tänulikud teie tagasiside ja soovituste selle eelvaate funktsiooni. Täitke [küsitlus](http://www.instant.ly/s/FDdrb) , andke meile teada, mida te arvate.

## <a name="havent-had-a-chance-to-try-the-preview-feature"></a>Võimalus proovida eelvaate funktsioon ei ole olnud?
Kui te pole osalenud eelvaate veel, kasutage selle [küsitluse](http://www.instant.ly/s/AY83p) taotleda juurdepääsu.
