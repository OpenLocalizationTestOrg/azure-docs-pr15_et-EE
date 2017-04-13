<properties
pageTitle="Kaugtöölaua ühendus lubamine rolli Azure'i pilveteenustega PowerShelli abil"
description="Kuidas konfigureerida azure pilveteenuse otsinguteenuse rakenduse luba Kaugtöölaua ühendused PowerShelli abil"
services="cloud-services"
documentationCenter=""
authors="thraka"
manager="timlt"
editor=""/>
<tags
ms.service="cloud-services"
ms.workload="tbd"
ms.tgt_pltfrm="na"
ms.devlang="na"
ms.topic="article"
ms.date="08/05/2016"
ms.author="adegeo"/>

# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services-using-powershell"></a>Kaugtöölaua ühendus lubamine rolli Azure'i pilveteenustega PowerShelli abil

>[AZURE.SELECTOR]
- [Azure'i klassikaline portaal](cloud-services-role-enable-remote-desktop.md)
- [PowerShelli](cloud-services-role-enable-remote-desktop-powershell.md)
- [Visual Studio](../vs-azure-tools-remote-desktop-roles.md)


Kaugtöölaua võimaldab teil juurde pääseda töölaua töötab Azure rolli. Kaugtöölaua ühendus abil saate diagnoosida probleeme rakenduse töötamise ajal ja tõrkeotsing.

Selles artiklis kirjeldatakse, kuidas lubada kaugtöölaua oma Cloud rolli PowerShelli kaudu. Vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md) jaoks eeltingimused, mis on vaja seda artiklit. PowerShelli kasutab Remote'i töölaua laiend, saate lubada kaugtöölaua pärast rakenduse juurutamist.


## <a name="configure-remote-desktop-from-powershell"></a>Kaugtöölaua PowerShelli kaudu konfigureerimine

Cmdlet [Set-AzureServiceRemoteDesktopExtension](https://msdn.microsoft.com/library/azure/dn495117.aspx) võimaldab kaugtöölaua määratud rolle või kõikide rollide pilvepõhise teenuse kasutamise lubamine. Cmdlet saate määrata oma kasutajanime ja parooliga Remote'i töölaua kasutaja *mandaati* parameeter, mis aktsepteerib PSCredential objekti kaudu.

Kui kasutate PowerShelli interaktiivseks, saate seada PSCredential objekti hõlpsalt helistades cmdleti [Get-mandaat](https://technet.microsoft.com/library/hh849815.aspx) .

```
$remoteusercredentials = Get-Credential
```

See käsk kuvab dialoogiboksi, mis võimaldab teil sisestada kasutajanimi ja parool serveri kasutaja turvaline viisil.

Kuna PowerShelli aitab automatiseerimise stsenaariumid, saate häälestada ka nii, et ei nõua kasutaja suhtluse **PSCredential** objekti. Esmalt peate loodud turvaline parool. Teil Alustuseks täpsustades Lihttekst parooli teisendage see turvaline stringi [ConvertTo-SecureString](https://technet.microsoft.com/library/hh849818.aspx)abil. Järgmiseks peate turvaline stringile teisendamine krüptitud tavalise string, mis on [ConvertFrom-SecureString](https://technet.microsoft.com/library/hh849814.aspx)abil. Nüüd saate salvestada krüptitud tavalise stringile faili [Set-sisu](https://technet.microsoft.com/library/ee176959.aspx)abil.

Turvalise parooli faili saate luua ka nii, et te ei peaks iga kord parooli tippima. Ka turvalise parooli faili on paremad tekstifaili. Kasutage järgmist PowerShelli turvalise parooli faili loomiseks:

```
ConvertTo-SecureString -String "Password123" -AsPlainText -Force | ConvertFrom-SecureString | Set-Content "password.txt"
```

>[AZURE.IMPORTANT] Parooli määramisel veenduge, et vastate [keerukuse nõuetele](https://technet.microsoft.com/library/cc786468.aspx).

Turvalise parooli faili mandaadi objekti loomiseks peate faili sisu lugemine ja teisendada need turvaline stringi [ConvertTo-SecureString](https://technet.microsoft.com/library/hh849818.aspx)abil.

Cmdlet [Set-AzureServiceRemoteDesktopExtension](https://msdn.microsoft.com/library/azure/dn495117.aspx) aktsepteerib ka *aegumise* parameeter, mis määrab **kuupäeva ja kellaaja** , kus kasutajakonto aegub. Näiteks saate väärtuseks konto mõne päeva praeguse kuupäeva ja kellaaja.

Selles näites PowerShelli näidatakse, kuidas seada Remote'i töölaua laiend pilveteenus:

```
$servicename = "cloudservice"
$username = "RemoteDesktopUser"
$securepassword = Get-Content -Path "password.txt" | ConvertTo-SecureString
$expiry = $(Get-Date).AddDays(1)
$credential = New-Object System.Management.Automation.PSCredential $username,$securepassword
Set-AzureServiceRemoteDesktopExtension -ServiceName $servicename -Credential $credential -Expiration $expiry
```
Saate määrata ka soovi juurutamine pesa ja rollid, mida soovite kaugtöölaua lubamine. Kui need parameetrid pole määratud, võimaldab cmdlet kaugtöölaua kõikide rollide **tootmise** juurutamise pesa.

Kaugtöölaua laiend on seostatud juurutamine. Kui loote uue juurutamise teenuse, tuleb lubada kaugtöölaua selle juurutuse. Kui soovite alati olema Kaugtöölaud lubatud, siis kaaluge PowerShelli skriptide integreerimine juurutamise töövoog.


## <a name="remote-desktop-into-a-role-instance"></a>Kaugtöölaua üheks rolli eksemplari
[Get-AzureRemoteDesktopFile](https://msdn.microsoft.com/library/azure/dn495261.aspx) cmdlet kasutatakse kaugtöölaua sisse kindla rolliga eksemplari oma pilveteenuses. *LocalPath* parameetri abil saate kohalikult RDP faili alla laadida. Või kasutage parameetrit *käivitada* otse käivitada kaugtöölaua ühendus dialoogiboksi rolli eksemplari pilvepõhise juurdepääsu.

```
Get-AzureRemoteDesktopFile -ServiceName $servicename -Name "WorkerRole1_IN_0" -Launch
```


## <a name="check-if-remote-desktop-extension-is-enabled-on-a-service"></a>Märkige ruut, kui teenus on lubatud kaugtöölaua laiend
[Get-AzureServiceRemoteDesktopExtension](https://msdn.microsoft.com/library/azure/dn495261.aspx) cmdlet kuvab mis kaugtöölaua on lubatud või keelatud teenuse juurutuse. Selle cmdlet kuvab kasutajanimi serveri töölaua kasutaja ja rollide Remote'i töölaua laiend on lubatud. Vaikimisi see juhtub juurutamise pesa ja soovi korral saate kasutada hoopis vahekausta pesa.

```
Get-AzureServiceRemoteDesktopExtension -ServiceName $servicename
```

## <a name="remove-remote-desktop-extension-from-a-service"></a>Kaugtöölaua laiend teenuse eemaldamine
Kui juba lubatud Remote'i töölaua laiend positsioonidel, ja selle kaugtöölaua sätted värskendama, esmalt eemaldada laiendamine. Ja uute sätetega uuesti lubada. Näiteks, kui soovite määrata uue remote kasutajakonto parooli või konto aegunud. Selleks on vaja olemasoleva juurutuste Remote'i töölaua laiendiga lubatud. Uue juurutuste korral saate lihtsalt rakendada laiendamine otse.

Juurutamise Remote'i töölaua laiend eemaldamiseks saate kasutada cmdlet-käsu [Eemalda-AzureServiceRemoteDesktopExtension](https://msdn.microsoft.com/library/azure/dn495280.aspx) . Saate määrata ka soovi juurutamine pesa ja roll, millest soovite eemaldada Remote'i töölaua laiendamine.

```
Remove-AzureServiceRemoteDesktopExtension -ServiceName $servicename -UninstallConfiguration
```

>[AZURE.NOTE] Täielikuks eemaldamiseks laiend konfiguratsiooni, peaksite helistama *eemaldamine* cmdlet koos parameetriga **UninstallConfiguration** .
>
>Parameetri **UninstallConfiguration** desinstallitakse mis tahes laiend konfiguratsiooni, mis on rakendatud teenuse. Iga laiend konfigureerimine on seostatud teenuse konfigureerimine. Helistamiseks cmdlet *eemaldada* ilma **UninstallConfiguration** disassociates <mark>juurutamise</mark> laiend konfiguratsioonist, seega tõhus eemaldamine laiendamine. Teenusega seotud jääb siiski laiend konfigureerimine.



## <a name="additional-resources"></a>Lisaressursid

[Kuidas seadistada pilveteenustega](cloud-services-how-to-configure.md)
