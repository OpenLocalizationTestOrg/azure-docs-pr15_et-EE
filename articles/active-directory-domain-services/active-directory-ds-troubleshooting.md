<properties
    pageTitle="Azure Active Directory domeeniteenused: Tõrkeotsingujuhendi | Microsoft Azure'i"
    description="Azure'i AD domeeniteenused tõrkeotsingu juhend"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="maheshu"/>

# <a name="azure-ad-domain-services---troubleshooting-guide"></a>Azure'i AD domeeniteenused - Tõrkeotsingujuhendi
Selles artiklis tõrkeotsinguvihjed loomisel või haldamise Azure Active Directory (AD) domeeniteenused võivad ilmneda probleemid.


## <a name="you-cannot-enable-azure-ad-domain-services-for-your-azure-ad-directory"></a>Ei saa lubada Azure AD domeeniteenused Azure AD kataloogi jaoks
Selles jaotises aitab teil tõrkeotsing, kui proovite lubada Azure AD domeeni teenuste kataloogi ja seda ei või saab lülitada naasmiseks "Keelatud".

Valige tõrkeotsingu juhiseid, mis vastavad ilmneda tõrketeate.

|**Tõrketeade**|**Eraldusvõime**|
|---|:---|
|*Contoso100.com nimi on juba kasutusel selles võrgus. Määrake nimi, mis ei kasuta.*|[Domeeni nimekonflikt virtuaalse võrgu](active-directory-ds-troubleshooting.md#domain-name-conflict)
|*Domeeni teenuste ei saanud seda Azure AD rentniku lubada. Teenust ei saa rakendus nimega 'Azure AD domeeni teenuste sünkroonimine' asjakohased õigused. Rakendus nimega 'Azure AD domeeni teenuste sünkroonimine' kustutamine ja proovige seejärel lubada domeeniteenused oma Azure AD rentniku jaoks.*|[Domeeni teenuste ei saa Azure AD domeeni sünkroonimisrakenduse asjakohased õigused](active-directory-ds-troubleshooting.md#inadequate-permissions)|
|*Domeeni teenuste ei saanud seda Azure AD rentniku lubada. Domeeni teenuste rakendus oma Azure AD rentniku pole vajalikke õigusi, domeeniteenused. Rakenduse abil rakenduse identifikaator d87dcbc6-a371-462e-88e3-28ad15ec4e64 kustutada ja proovige seejärel lubamiseks domeeniteenused oma Azure AD rentniku jaoks.*|[Domeeni teenuste rakendus pole teie rentnik õigesti konfigureeritud](active-directory-ds-troubleshooting.md#invalid-configuration)|
|*Domeeni teenuste ei saanud seda Azure AD rentniku lubada. Rakenduse Microsoft Azure AD keelatakse teie Azure AD rentniku. Luba rakenduse identifikaator 00000002-0000-0000-c000-000000000000 taotluse ja proovige seejärel lubada domeeniteenused oma Azure AD rentniku jaoks.*|[Microsoft Graphi rakendus on keelatud oma Azure AD rentniku](active-directory-ds-troubleshooting.md#microsoft-graph-disabled)|


### <a name="domain-name-conflict"></a>Domeeni nimekonflikt.
**Kuvatakse tõrketeade:**

*Contoso100.com nimi on juba kasutusel selles võrgus. Määrake nimi, mis ei kasuta.*

**Parandamise:**

Veenduge, et teil pole saadaval, et virtuaalse võrgus domeeni sama nimega olemasoleva domeeni. Näiteks Oletame, et teil on domeeni nimega contoso.com juba saadaval valitud virtuaalse võrgus. Hiljem proovite lubada Azure AD domeeniteenused hallatava domeeni sama domeeni nimi (ehk teisisõnu öeldes contoso.com) selles virtuaalse võrgus. Luba Azure AD domeeniteenused katsel ilmneb tõrge.

Selle tõrke põhjuseks nimekonfliktide virtuaalse võrgus selle domeeni nimi. Sellisel juhul tuleb kasutada erineva nimega Azure AD domeeniteenused hallatava domeeni häälestamiseks. Vaheldumisi, saate asutusest olemasoleva domeeni ja siis jätkake lubamiseks Azure AD domeeniteenused.


### <a name="inadequate-permissions"></a>Puudulik õigused
**Kuvatakse tõrketeade:**

*Domeeni teenuste ei saanud seda Azure AD rentniku lubada. Teenust ei saa rakendus nimega 'Azure AD domeeni teenuste sünkroonimine' asjakohased õigused. Rakendus nimega 'Azure AD domeeni teenuste sünkroonimine' kustutamine ja proovige seejärel lubada domeeniteenused oma Azure AD rentniku jaoks.*

**Parandamise:**

Kontrollige, kas rakenduse nimega "Azure AD domeeni teenuste sünkroonimine' Azure AD kataloogis. Kui see rakendus on olemas, kustutage see ja seejärel uuesti lubamiseks Azure AD domeeniteenused.

Järgmiste toimingute rakenduse olemasolu kontrollida ja kustutage see, kui rakendus on olemas:

  1. Liikuge **Azure klassikaline portaali** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).
  2. Valige vasakpoolsel paanil sõlme **Active Directory** .
  3. Valige Azure AD rentniku (directory), mille jaoks soovite lubada Azure AD domeeniteenused.
  4. Avage menüü **rakendused** .
  5. Valige suvand **minu ettevõte kuulub rakenduste** rippmenüüst.
  6. Otsige rakendus nimega **Azure AD domeeni teenuste sünkroonimine**. Kui rakendus on olemas, saate jätkata selle kustutada.
  7. Kui olete kustutanud rakendus, proovige Azure AD domeeniteenused taas lubada.


### <a name="invalid-configuration"></a>Sobimatu konfiguratsioon
**Kuvatakse tõrketeade:**

*Domeeni teenuste ei saanud seda Azure AD rentniku lubada. Domeeni teenuste rakendus oma Azure AD rentniku pole vajalikke õigusi, domeeniteenused. Rakenduse abil rakenduse identifikaator d87dcbc6-a371-462e-88e3-28ad15ec4e64 kustutada ja proovige seejärel lubamiseks domeeniteenused oma Azure AD rentniku jaoks.*

**Parandamise:**

Kontrollige, kas teil on rakenduse nimega AzureActiveDirectoryDomainControllerServices"(koos rakenduse identifikaator d87dcbc6-a371-462e-88e3-28ad15ec4e64) Azure AD kataloogis. Kui see rakendus on olemas, peate selle kustutada ja seejärel uuesti lubamiseks Azure AD domeeniteenused.

Kasutage järgmist PowerShelli skripti rakenduse otsimine ja kustutage see.

> [AZURE.NOTE] See skript kasutab **Azure AD PowerShelli versioon 2** cmdlet-käsud. Kõigi saadaolevate cmdlet-käskude ja alla moodul täieliku loendi leiate siit [AzureAD PowerShelli dokumentides](https://msdn.microsoft.com/library/azure/mt757189.aspx).

```
$InformationPreference = "Continue"
$WarningPreference = "Continue"

$aadDsSp = Get-AzureADServicePrincipal -Filter "AppId eq 'd87dcbc6-a371-462e-88e3-28ad15ec4e64'" -ErrorAction Ignore
if ($aadDsSp -ne $null)
{
    Write-Information "Found Azure AD Domain Services application. Deleting it ..."
    Remove-AzureADServicePrincipal -ObjectId $aadDsSp.ObjectId
    Write-Information "Deleted the Azure AD Domain Services application."
}

$identifierUri = "https://sync.aaddc.activedirectory.windowsazure.com"
$appFilter = "IdentifierUris eq '" + $identifierUri + "'"
$app = Get-AzureADApplication -Filter $appFilter
if ($app -ne $null)
{
    Write-Information "Found Azure AD Domain Services Sync application. Deleting it ..."
    Remove-AzureADApplication -ObjectId $app.ObjectId
    Write-Information "Deleted the Azure AD Domain Services Sync application."
}

$spFilter = "ServicePrincipalNames eq '" + $identifierUri + "'"
$sp = Get-AzureADServicePrincipal -Filter $spFilter
if ($sp -ne $null)
{
    Write-Information "Found Azure AD Domain Services Sync service principal. Deleting it ..."
    Remove-AzureADServicePrincipal -ObjectId $sp.ObjectId
    Write-Information "Deleted the Azure AD Domain Services Sync service principal."
}
```
<br>

### <a name="microsoft-graph-disabled"></a>Microsoft Graphi keelatud.
**Kuvatakse tõrketeade:**

Domeeni teenuste ei saanud seda Azure AD rentniku lubada. Rakenduse Microsoft Azure AD keelatakse teie Azure AD rentniku. Luba rakenduse identifikaator 00000002-0000-0000-c000-000000000000 taotluse ja proovige seejärel lubada domeeniteenused oma Azure AD rentniku jaoks.

**Parandamise:**

Kontrollige, kui olete rakenduse identifikaatoriga 00000002 0000-0000-c000-000000000000 keelanud. See rakendus on rakendus Microsoft Azure AD ja pakub Graph API juurdepääsu oma Azure AD rentniku. Azure'i AD domeeniteenused peab olema lubatud sünkroonida oma Azure AD rentniku hallatava domeeni selle rakenduse.

Selle tõrke, lubada see rakendus ja proovige uuesti lubamine domeeniteenused oma Azure AD rentniku jaoks.


## <a name="users-are-unable-to-sign-in-to-the-azure-ad-domain-services-managed-domain"></a>Kasutajad ei saa sisse logida Azure AD domeeniteenused hallatava domeeni
Kui teie Azure AD rentniku ühe või mitme kasutaja ei saa vastloodud hallatava domeeni sisse logida, tehke järgmisi tõrkeotsingujuhiseid.

- **Sisselogimine UPN-vormingus:** Logige sisse UPN-vormingus (nt 'joeuser@contoso.com') asemel SAMAccountName vorming (CONTOSO\joeuser"). Funktsiooni SAMAccountName võib automaatselt loodud kasutajate nime, kelle UPN-i eesliide on liiga pikk või on sama mis teise kasutaja hallatavate domeenis. UPN-i vorming on tagatud on Azure AD rentniku raames olema kordumatu.

> [AZURE.NOTE] Soovitame kasutada UPN-vormingus Azure AD domeeniteenused hallatava domeeni sisse logida.

- Veenduge, et teil on [lubatud parooli sünkroonimise](active-directory-ds-getting-started-password-sync.md) vastavalt alustamine juhend kirjeldatud juhised.

- **Välise kontod:** Tagada, et probleemse kasutajakonto ei ole Azure AD rentniku välise konto. Välise kontod näited Microsofti kontod (näiteks 'joe@live.com') või kasutajakontode välisel Azure AD kataloogi. Kuna Azure AD domeeniteenused ei ole sellise kasutajakontode mandaat, need kasutajad ei saa sisse logida hallatava domeeni.

- **Sünkroonitud kontode:** Kui probleemse Kasutajakontod sünkroonitakse kohapealse directory kaudu, siis kontrollige, kas:
    - Teil on juurutatud või värskendatud [Viimane soovitatav versiooni Azure'i AD-ühenduse](active-directory-ds-getting-started-password-sync.md#install-or-update-azure-ad-connect).

    - Teil on konfigureeritud Azure'i AD-ühenduse [täielik sünkroonimine](active-directory-ds-getting-started-password-sync.md).

    - Sõltuvalt kataloogi suurust see kasutajakontode aega võtta ja mandaadi hashes Azure AD domeeniteenused saadaval. Veenduge, et oodata piisavalt kaua proovimiseks autentimine (olenevalt kataloogi - mõne tunni pärast kuupäevaks või kahe suure kataloogid suurus).

    - Kui probleem ei lahene, pärast eelmist toimingut, taaskäivitage teenus Microsoft Azure AD sünkroonimine. Sünkroonimine arvuti käivitage käsuviip ja käivitada järgmised käsud:
      1. Lõpeta "Microsoft Azure AD sünkroonimine"
      2. net start "Microsoft Azure AD sünkroonimine"

- **Vaid kontod**: kui probleemse kasutajakonto on vaid kasutajakonto, veenduge, et kasutaja on muutunud oma parooli, kui teile on lubatud Azure AD domeeniteenused. Selles etapis tuleb põhjustab mandaadi nõutav Azure AD domeeni jaoks genereeritakse hashes.


## <a name="users-removed-from-your-azure-ad-tenant-are-not-removed-from-your-managed-domain"></a>Kasutajad, eemaldatakse teie Azure AD rentniku ei eemaldata hallatava domeeni
Azure AD kaitseb teie kasutaja objektide juhuslikku kustutamist. Kui kasutajakonto kustutamine teie Azure AD rentniku teisaldatakse vastava kasutaja objekti prügikasti. Kui hallatava domeeni, sünkroonitakse kustutamine toimingut, põhjustab vastava kasutaja konto, mida soovite märkida keelatud. Selle funktsiooni abil saate taastada või kasutajakonto hiljem taastada.

Hallatava domeeni eemaldada kasutajakonto täielikult, kustutada kasutaja jäädavalt teie Azure AD rentniku. Eemalda-MsolUser PowerShelli cmdletiga koos selle '-RemoveFromRecycleBin' valikut, nagu on kirjeldatud selles [artiklis MSDN-i](https://msdn.microsoft.com/library/azure/dn194132.aspx).


## <a name="contact-us"></a>Võtke meiega ühendust
Võtke ühendust Azure Active Directory domeeniteenused toote meeskonna [tagasiside andmine või tugiteenuste] (aktiivne-directory-ds-kontakti-us.md).
