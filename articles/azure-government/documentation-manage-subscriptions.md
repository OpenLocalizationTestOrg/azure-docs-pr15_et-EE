<properties
    pageTitle="Azure'i teenuste | Microsoft Azure'i"
    description="Teavet tellimuse Azure Governmenti haldamine"
    services="Azure-Government"
    cloud="gov" 
    documentationCenter=""
    authors="zakramer"
    manager="liki"
    editor="" />

<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="10/21/2016"
    ms.author="zakramer" />


#  <a name="managing-and-connecting-to-your-subscription-in-azure-government"></a>Haldamine ja ühenduse tellimuse Azure'i Government

Azure'i Government on kordumatu URL-id ja keskkonna haldamise lõpp-punktid. See on oluline õige ühendused abil saate hallata keskkonna portaali või PowerShelli kaudu. Kui olete loonud ühenduse Azure'i Government keskkonnas, tavaline toimingute haldamise teenus töötab, kui komponendi kasutusele võetud.

## <a name="connecting-via-the-portal"></a>Portaali kaudu ühenduse loomine
Portaal on esmane nii, et enamik inimesi ühenduse Azure'i Government.  Ühenduse, liikuge sirvides soovitud portaali [https://portal.azure.us](https://portal.azure.us).  Pärand versiooni Azure portaali pääseb [https://manage.windowsazure.us](https://manage.windowsazure.us).

Tellimuste saab teie konto jaoks luua ühenduse [https://account.windowsazure.us](https://account.windowsazure.us).

## <a name="connecting-via-powershell"></a>Ühenduse loomine PowerShelli kaudu

Kas te kasutate Azure PowerShelli skripti või Accessi funktsioonid, mis pole praegu saadaval, peate ühenduse Azure'i Government asemel Azure'i avaliku Azure portaali kaudu suure tellimuse haldamiseks.  Kui olete kasutanud PowerShelli Azure'i avaliku, on enamasti sama.  Azure'i Government erinevused on:

+ Teie kontoga ühenduse loomine
+ Piirkonna nimed

>[AZURE.NOTE] Kui te pole PowerShelli veel kasutanud, lugege teemat [Azure PowerShelli tutvustus](../powershell-install-configure.md).

PowerShelli käivitamisel teil kindlaks teha Azure PowerShelli ühenduse Azure'i Government määramisega on keskkonna parameeter.  Parameetri tagab, et PowerShelli loob ühenduse õige lõpp-punktid.  Lõpp-punktid kogum määratakse, kui ühendate oma kontosse sisselogimine.  Erinevate API-de nõuavad erinevaid versioone keskkonna vahetamine:

Ühenduse tüüp | Käsk
---|----
[Teenusehaldus](https://msdn.microsoft.com/library/dn708504.aspx) käsud | `Add-AzureAccount -Environment AzureUSGovernment`
[Ressursside haldamine](https://msdn.microsoft.com/library/mt125356.aspx) käsud | `Add-AzureRmAccount -EnvironmentName AzureUSGovernment`
[Azure Active Directory](https://msdn.microsoft.com/library/azure/jj151815.aspx) käsud | `Connect-MsolService -AzureEnvironment UsGovernment`
[Azure Active Directory käsk v2](https://msdn.microsoft.com/library/azure/mt757189.aspx) | `Connect-AzureAD -AzureEnvironmentName AzureUSGovernment`

Võite ka Kasutage parameetrit keskkonnas kui abil uus-AzureStorageContext salvestusruumi kontoga ühenduse ja määrake AzureUSGovernment.

### <a name="determining-region"></a>Määratlemine, piirkond

Kui olete loonud, on üks täiendavad erinevus – piirkondade kasutada teenust suunata.  Iga Azure'i pilve on erinevate piirkondade.  Saate vaadata nende loetletud lehel teenuse kättesaadavus.  Käsu kasutate tavaliselt piirkond parameetrit asukoht.

On ühe saagi.  Azure'i Government piirkondades on vaja nende levinud nimed erinevalt vormindada.

Levinud nimi | Käsk
---|----
USA valitsuse Virginia | Autoriõiguspõhimõtted Virginia
USA valitsuse Iowa | Autoriõiguspõhimõtted Iowa

>[AZURE.NOTE] Ei ole ruumi USA ja gov – vahel asukoht parameetri kasutamisel.

Kui kunagi soovite valideerida Azure Governmenti saadaval piirkondade, saate käivitage järgmised käsud ja Prindi praegune loend.

    Get-AzureLocation

Kui teil on saadaval keskkonnas skype_for_businessis üle Azure, saate kasutada.

    Get-AzureEnvironment

## <a name="next-steps"></a>Järgmised sammud

Kui otsite lisateavet, võite vaadata:

+ [PowerShelli dokumentide github](https://github.com/Azure/azure-powershell)
+ [Üksikasjalikud juhised ühenduse ressursside haldamine](https://blogs.msdn.microsoft.com/azuregov/2015/10/08/configuring-arm-on-azure-gc/)
+ [Azure'i PowerShelli dokumentide MSDN-is](https://msdn.microsoft.com/library/mt619274.aspx)

Lisateave ja värskendused tellida [Microsoft Azure'i Government ajaveebi] (https://blogs.msdn.microsoft.com/azuregov/)
