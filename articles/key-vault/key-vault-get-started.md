<properties
    pageTitle="Azure'i klahvi Vault alustamine | Microsoft Azure'i"
    description="Selle õpetuse abil saate alustamine Azure'i klahvi Vault Azure talletamiseks ja haldamiseks cryptographic võtmed ja Azure saladused kõva container loomiseks."
    services="key-vault"
    documentationCenter=""
    authors="cabailey"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/24/2016"
    ms.author="cabailey"/>

# <a name="get-started-with-azure-key-vault"></a>Azure'i klahvi Vault kasutamise alustamine #
Azure'i klahvi Vault on saadaval enamikes piirkondades. Lisateabe saamiseks lugege teemat [klahvi Vault hinnad lehe](https://azure.microsoft.com/pricing/details/key-vault/).

## <a name="introduction"></a>Sissejuhatus  
Selle õpetuse abil saate alustamine Azure'i klahvi Vault Azure talletamiseks ja haldamiseks cryptographic võtmed ja Azure saladused kõva container (võlvkelder) loomiseks. See juhatab teid läbi protsessi loomine on hoidla, mis sisaldab võti või parool, mida saate kasutada Azure rakendusega Azure PowerShelli abil. See siis näitab teile, kuidas rakenduse saate kasutada seda klahvi või parool.

**Hinnanguline aega:** 20 minutit

>[AZURE.NOTE]  Selle õpetuse juhised, kuidas kirjutada Azure rakenduse, et üks juhiseid sisaldab täpsemalt, kuidas lubada taotluse kasutada klahvi või salajane võtme võlvkelder ei sisalda.
>
>Selle õpetuse kasutab Azure PowerShelli. Mitu platvormi käsurea liides juhiste saamiseks vaadake teemat [õppeteema samaväärsed](key-vault-manage-with-cli.md).

Azure'i klahvi Vault kohta ülevaate leiate teemast [mis on Azure klahvi Vault?](key-vault-whatis.md)

## <a name="prerequisites"></a>Eeltingimused

Selle õpetuse lõpuleviimiseks peab olema järgmised:

- Microsoft Azure'i tellimust. Kui te ei ole, mida saate registreeruda [tasuta konto](https://azure.microsoft.com/pricing/free-trial/).
- Azure'i PowerShelli **minimaalne versiooni 1.1.0**. Azure'i PowerShelli installimine ja seostada Azure tellimuse, vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md). Kui teil on juba installitud Azure PowerShelli ja ei tea selle versiooni Azure PowerShelli konsooli, tippige `(Get-Module azure -ListAvailable).Version`. Kui teil on Azure PowerShelli versiooni 0.9.1 kaudu 0.9.8 installitud, saate siiski kasutada selles õpetuses mõned väikesed muudatused. Näiteks, kasutage funktsiooni `Switch-AzureMode AzureResourceManager` käsk ja mõned Azure'i klahvi Vault käsud on muutunud. Loendi klahvi Vault cmdlettide versioonid 0.9.1 0.9.8 kaudu, leiate [Azure'i klahvi Vault cmdlet-käsud](https://msdn.microsoft.com/library/azure/dn868052\(v=azure.98\).aspx). 
- Rakendus, mis konfigureeritakse kasutage või parool, mida selles õpetuses loote. Valimi rakendus on saadaval [Microsofti allalaadimiskeskuse](http://www.microsoft.com/en-us/download/details.aspx?id=45343)kaudu. Lisateabe saamiseks vaadake lisatud Readme-faili.


Selles õpetuses on mõeldud Azure PowerShelli algajatele, kuid eeldatakse, et mõistate põhimõtted, nt moodulid, cmdlet-käskude ja seansid. Lisateavet leiate teemast [alustamine Windows PowerShelli abil](https://technet.microsoft.com/library/hh857337.aspx).

Mis tahes cmdlet, mis kuvatakse selles õpetuses üksikasjalikku abi saamiseks kasutage cmdlet **Abi saamiseks** .

    Get-Help <cmdlet-name> -Detailed

Näiteks tippige **Login-AzureRmAccount** cmdlet-käsu jaoks abi saamiseks:

    Get-Help Login-AzureRmAccount -Detailed

Samuti saate lugeda järgmised õppetükid tutvumine Azure'i ressursihaldur Azure PowerShelli abil:

- [Kuidas installida ja konfigureerida Azure PowerShell](../powershell-install-configure.md)
- [Ressursihaldur koos Azure PowerShelli abil](../powershell-azure-resource-manager.md)


## <a id="connect"></a>Ühenduse loomine oma tellimused ##

Mõne Azure PowerShelli seansi käivitamine ja logige sisse Azure'i kontosse järgmine käsk:  

    Login-AzureRmAccount 

Pange tähele, et kui te kasutate teatud eksemplari Azure, näiteks Azure'i Government, kasutage parameetrit - keskkonnas see käsk. Näiteks:`Login-AzureRmAccount –Environment (Get-AzureRmEnvironment –Name AzureUSGovernment)`

Hüpikakende brauseriaknas, sisestage oma Azure'i konto kasutajanimi ja parool. Azure'i PowerShelli saab tellimused, mis on seotud selle kontoga ja vaikimisi, kasutab esimene.

Kui teil on mitu tellimust ja soovite määrata teatud ühe Azure'i klahvi Vault jaoks, tippige oma konto tellimused kuvamiseks järgmist:

    Get-AzureRmSubscription

Tellimuse kasutada määramiseks tippige:

    Set-AzureRmContext -SubscriptionId <subscription ID>

Azure'i PowerShelli konfigureerimise kohta lisateabe saamiseks vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md).


## <a id="resource"></a>Uue ressursirühma loomine ##

Azure'i ressursihaldur kasutamisel luuakse kõik seotud ressursid ressursirühma sees. Loome uue ressursirühma nimega **ContosoResourceGroup** õppeteema:

    New-AzureRmResourceGroup –Name 'ContosoResourceGroup' –Location 'East Asia'


## <a id="vault"></a>Võtme vault loomine ##

[Uus-AzureRmKeyVault](https://msdn.microsoft.com/library/azure/mt603736\(v=azure.300\).aspx) cmdlet-käsu abil saate luua põhilistest vault. Selle cmdlet-käsu on kolm kohustuslikud parameetrid: **Ressursirühma nimi**, **võtme vault nimi**ja **geograafiline asukoht**.

Näiteks kui kasutate **ContosoKeyVault**vault nime, **ContosoResourceGroup**ressursi rühma nime ja asukoha **Ida-Aasia**, tippige:

    New-AzureRmKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia'

Selle cmdlet-käsu väljund kuvatakse soovitud klahv hoidla, mis äsja loodud atribuudid. On kaks kõige olulisemad atribuudid.

- **Vault nimi**: näites on **ContosoKeyVault**. Kasutate selle klahvi Vault cmdletid nimi.
- **Vault URI**: näites on https://contosokeyvault.vault.azure.net/. Rakendustes, mis kasutavad teie vault kaudu oma REST API peate kasutama selle URI.

Azure'i konto on nüüd volitatud toiminguid selle võtme vault. Veel on keegi teine.

>[AZURE.NOTE]  Kui näete viga **tellimus pole registreeritud kasutada nimeruum "Microsoft.KeyVault"** , kui proovite luua oma uue tootenumbri vault, käivitage `Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.KeyVault"` ja seejärel käivitage uuesti oma käsu New-AzureRmKeyVault. Lisateavet leiate teemast [Register-AzureRmResourceProvider](https://msdn.microsoft.com/library/azure/mt759831\(v=azure.300\).aspx).
>

## <a id="add"></a>Klahv või salajane lisamine võtme hoidlasse ##

Azure'i klahvi Vault tarkvara kaitstud võtme loomiseks saate soovi korral [Lisa-AzureKeyVaultKey](https://msdn.microsoft.com/library/azure/dn868048\(v=azure.300\).aspx) cmdlet-käsu kasutamine ja tippige järgmine:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey' -Destination 'Software'

Juhul, kui teil on olemasolev tarkvara kaitstud võti lisamine. PFX-fail nimega softkey.pfx, mida soovite üles laadida Azure'i klahvi Vault, faili C:\ kettale salvestatud tippige muutuvate **securepfxpwd** **123** jaoks parooli seadmiseks järgmist funktsiooni. PFX-fail:

    $securepfxpwd = ConvertTo-SecureString –String '123' –AsPlainText –Force

Sisestage järgmine võtme importimise funktsiooni. PFX fail, mida kaitseb võti tarkvara klahvi Vault teenus:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey' -KeyFilePath 'c:\softkey.pfx' -KeyFilePassword $securepfxpwd


Nüüd saate selle klahvi loodud või üles laadida, kasutades oma URI Azure'i klahvi Vault, otsida. Kasutage **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** alati saada praegune versioon ning **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87** saada see teatud versioon.  

Tippige selle klahvi URI kuvamiseks:

    $Key.key.kid

Salajane lisamine soovitud hoidlasse, mis on nimega SQLPassword parooli ja Azure klahvi Vault Pa$ $w0rd väärtus on, esmalt teisendamine väärtus Pa$ $w0rd turvaline stringi tippides:

    $secretvalue = ConvertTo-SecureString 'Pa$$w0rd' -AsPlainText -Force

Tippige järgmine:

    $secret = Set-AzureKeyVaultSecret -VaultName 'ContosoKeyVault' -Name 'SQLPassword' -SecretValue $secretvalue

Nüüd saate otsida selle parool, mida lisasite Azure'i klahvi Vault, abil oma URI. Kasutage **https://ContosoVault.vault.azure.net/secrets/SQLPassword** alati saada praegune versioon ning **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d** selles teatud versiooni.

Tippige selle salajase URI kuvamiseks:

    $secret.Id

Klahv või äsja loodud salajane vaatame vaadata:

- Tootenumbri vaatamiseks tippige:`Get-AzureKeyVaultKey –VaultName 'ContosoKeyVault'`
- Tippige oma salajane kuvamiseks:`Get-AzureKeyVaultSecret –VaultName 'ContosoKeyVault'`

Teie olulisi vault ja klahvi või salajane saab nüüd rakenduste kasutamiseks valmis. Tuleb lubada rakenduste neid kasutada.  

## <a id="register"></a>Azure Active Directory rakenduse registreerimine ##

Selles etapis tuleb tavaliselt oleksid arendaja, eraldi arvutis. See ei ole eriti Azure'i klahvi Vault, kuid on kaasatud siin täiendavalt.


>[AZURE.IMPORTANT] Õpetuse, oma konto, vault ja selles etapis tuleb registreerida rakenduse lõpuleviimiseks kõik tuleb Azure samas kaustas.

Rakendustes, mis kasutavad võtme vault peab autentida märgiks Azure Active Directory abil. Selle tegemiseks rakenduse omanik peate registreeruma rakendus oma Azure Active Directory. Registreerimise lõpus rakenduse omanik saab järgmised väärtused:


- **Rakenduse ID** (tuntud ka kui kliendi ID) ja **autentimise võti** (nimetatakse ka ühiskasutuses salajane). Rakenduse peate esitama Azure Active Directory, et leida märgiks mõlemad järgmised väärtused. Kuidas rakendus on konfigureeritud selleks sõltub rakendusest. Klahv Vault valimi rakenduse, rakenduse omanik määrab need väärtused app.config faili.

Azure Active Directory rakenduse registreerimine

1. Azure'i klassikaline portaali sisse logida.
2. Klõpsake vasakul nuppu **Active Directory**ja valige, kus registreerimist rakenduse kataloogi. <br> <br> **Märkus:** Valige sama kataloog, mis sisaldab Azure'i tellimus, millega olete loonud teie olulisi vault. Kui te ei tea, milline directory see on, klõpsake nuppu **sätted**ja tuvastamine, millega olete loonud teie olulisi vault tellimuse kataloogi viimases veerus kuvatakse nimi.

3. Klõpsake nuppu **rakendused**. Kui pole rakendused on lisatud kataloogi, sellel lehel kuvatakse ainult linki **Lisa rakendus** . Klõpsake linki või teise võimalusena võite klõpsata **lisamine** käsuriba.
4.  Viisardi **Lisamine rakenduse** kohta on **mida te soovite teha?** leht, klõpsake käsku **Lisa rakendus minu ettevõte on arendamise**.
5.  Klõpsake lehel **meile rakenduse kohta** Määrake oma rakenduse nimi ja valige **WEB rakenduse ja/või WEB API** (vaikesäte). Klõpsake ikooni **edasi** .
6.  **Rakenduse atribuutide** lehel, määrake **Logi-ON URL-i** ja **Rakenduse ID URI** veebirakenduses. Kui teie rakendus ei saa neid väärtusi, saate neid üles selle etapi (näiteks saate määrata http://test1.contoso.com mõlemale jaoks). Pole oluline, kui nende saitide olemas. Oluline on see, et iga rakenduse ID URI rakendus on kataloogis iga rakenduse jaoks erinevad. Kataloogi kasutab stringile rakenduse tuvastamiseks.
7.  Klõpsake muudatuste salvestamiseks viisardi **lõpuleviimine** ikooni.
8.  Klõpsake lehel **Kiirkäivituse** **KONFIGUREERIMINE**.
9.  Liikuge kerides jaotisse **klahvid** , valige kestus ja klõpsake siis nuppu **Salvesta**. Lehe värskendab ja nüüd kuvatakse soovitud väärtus. Konfigureerige rakenduse ning selle võtmeväärtuse **Kliendiid** väärtuse. (Juhised selle konfiguratsioon on rakenduse kohased).
10. Kopeerige kliendi ID väärtus sellele lehele, mida te kasutate järgmise juhise juurde oma vault õiguste seadmiseks.

## <a id="authorize"></a>Lubada kasutada klahv või salajane ##

Lubada juurdepääsu (TAB) või salajane autoriloomingut, kasutage cmdlet  [Set-AzureRmKeyVaultAccessPolicy](https://msdn.microsoft.com/library/azure/mt603625\(v=azure.300\).aspx) .

Näiteks kui teie vault nimi on **ContosoKeyVault** ja te soovite lubada rakendus on Kliendiid 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed, ja soovite lubada dekrüptida ning logige sisse oma võlvkelder abil, käivitage järgmised:


    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToKeys decrypt,sign

Kui soovite lubada sama rakenduse lugeda saladusi oma võlvkelder, käivitage järgmised:


    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToSecrets Get

## <a id="HSM"></a>Kui soovite kasutada riistvara turvalisus mooduli (HSM) ##

Lisatud assurance, saate importida või riistvara turvalisus moodulid (HSMs), mis jäta HSM äärist võtmed. Funktsiooni HSMs on FIPS 140-2 tase 2 kinnitatud. Kui see nõue ei kehti, selle sammu vahele ja minge [tootenumbri vault kustutamine ja seotud võtmed ja saladused](#delete).

Need HSM kaitstud võtmed loomiseks peate kasutama [Azure klahvi Vault Premium teenuse kiht toetamiseks HSM kaitstud võtmed](https://azure.microsoft.com/pricing/free-trial/). Lisaks Pange tähele, et see funktsioon pole saadaval Azure Hiina.


Võtme vault loomisel lisage **- SKU** parameeter.


    New-AzureRmKeyVault -VaultName 'ContosoKeyVaultHSM' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia' -SKU 'Premium'



Saate selle võtme vault tarkvara kaitstud klahvid (nagu on näidatud varem) ja HSM kaitstud võtmed lisada. Luua mõne HSM kaitstud vajutamisega, seadke soovitud **-sihtkoha** "HSM" parameeter:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -Destination 'HSM'

Järgmise käsu abil importida võtit lisamine. PFX-fail oma arvutis. See käsk impordib klahvi Vault teenus HSMs võti:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -KeyFilePath 'c:\softkey.pfx' -KeyFilePassword $securepfxpwd -Destination 'HSM'


Järgmise käsu impordi "tuua oma klahv" (BYOK) paketi. Sel juhul saate luua tootevõtit teie kohalikku HSM ja selle üle HSMs klahvi Vault teenus, ilma jättes HSM äärist võti:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -KeyFilePath 'c:\ITByok.byok' -Destination 'HSM'

Vt täpsemaid juhiseid selle BYOK paketi loomise kohta, [Kuidas luua ja edastamine HSM kaitstud Azure klahvi Vault klahvid](key-vault-hsm-protected-keys.md).

## <a id="delete"></a>Võtme vault ja seotud võtmed ja saladusi kustutamine ##

Kui te enam ei vaja, et klahv vault ja klahvi või salajane, mis sisaldab seda, saate kustutada võtme vault abil [Eemalda-AzureRmKeyVault](https://msdn.microsoft.com/library/azure/mt619485\(v=azure.300\).aspx) cmdlet-käsk:

    Remove-AzureRmKeyVault -VaultName 'ContosoKeyVault'

Või saate kustutada kogu Azure ressursi rühma, mis sisaldab olulisi hoidla ja muud ressursid, mida te sellesse rühma.

    Remove-AzureRmResourceGroup -ResourceGroupName 'ContosoResourceGroup'


## <a id="other"></a>Azure'i PowerShelli cmdletid ##

Muud käsud, mis võib olla kasulik haldamise Azure'i klahvi Vault:

- `$Keys = Get-AzureKeyVaultKey -VaultName 'ContosoKeyVault'`: See käsk saab tabeli kuvamine kõigi võtmed ja valitud atribuudid.
- `$Keys[0]`: See käsk kuvab täieliku loendi atribuutide määratud võti
- `Get-AzureKeyVaultSecret`: See käsk on loetletud tabeli kuvamine kõigi salajane nimed ja valitud atribuudid.
- `Remove-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey'`: Näide kindla klahvi eemaldamise kohta.
- `Remove-AzureKeyVaultSecret -VaultName 'ContosoKeyVault' -Name 'SQLPassword'`: Näide kindla salajane eemaldamise kohta.


## <a id="next"></a>Järgmised sammud ##

Järeltegevuse juhendi kasutava Azure'i klahvi Vault veebirakenduse, leiate [Kasutamine Azure klahvi Vault veebirakenduse kaudu](key-vault-use-from-web-application.md).

Kuidas oma põhilistest vault kasutatakse, leiate teemast [Azure klahvi Vault logimine](key-vault-logging.md).

Loendi uusim Azure PowerShelli cmdletid Azure'i klahvi Vault jaoks, leiate [Azure'i klahvi Vault cmdlet-käsud](https://msdn.microsoft.com/library/azure/dn868052\(v=azure.300\).aspx). 
 

Viited kavandamiseks leiate [Azure'i klahvi Vault arendaja juhendist](key-vault-developers-guide.md).
