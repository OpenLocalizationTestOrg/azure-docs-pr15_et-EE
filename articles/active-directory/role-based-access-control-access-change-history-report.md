<properties
    pageTitle="Muutuste ajalugu Accessi aruande loomine | Microsoft Azure'i"
    description="Saate luua aruande, kus on loetletud kõik muutused juurdepääsu Azure tellimuste ja Rollipõhine juurdepääsu reguleerimine viimase 90 päeva jooksul."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/03/2016"
    ms.author="kgremban"/>

# <a name="create-an-access-change-history-report"></a>Muutuste ajalugu Accessi aruande loomine

Iga kord, kui keegi annab või tühistab juurdepääs oma tellimuste muudatused logitakse Azure piires. Saate luua Accessi muutuste ajalugu aruannete kõigi muudatuste nägemiseks viimase 90 päeva jooksul.

## <a name="create-a-report-with-azure-powershell"></a>Aruande loomine Azure PowerShelli abil
Kasutage Accessi muutuste ajalugu aruande loomiseks PowerShellis olevat `Get-AzureRMAuthorizationChangeLog` käsk. Täpsemat teavet cmdlet-käsu on saadaval [PowerShelli Galerii](https://www.powershellgallery.com/packages/AzureRM.Storage/1.0.6/Content/ResourceManagerStartup.ps1).

Kui helistate selle käsu, saate määrata atribuudist ülesanded, soovite loetletud, sh järgmised:

| Atribuut | Kirjeldus |
| -------- | ----------- |
| **Toiming** | Kas juurdepääsu anda ega tühistada |
| **Helistaja** | Accessi muutmine eest vastutav omanik |
| **Kuupäev** | Kuupäeva ja kellaaja, mis on muudetud juurdepääs |
| **DirectoryName** | Azure Active Directory kataloogis |
| **PrincipalName** | Kasutaja, rühma või rakenduse nimi |
| **PrincipalType** | Kas kasutaja, rühma või rakendus on ülesande |
| **RoleId** | Roll, mis anti, või tühistada GUID |
| **RoleName** | Mis anti, või tühistamise roll |
| **ScopeName** | See tellimus, ressursirühm või ressursi nimi |
| **ScopeType** | Kas ülesanne oli tellimus, ressursirühm või ressursside ulatus |
| **SubscriptionId** | Azure'i tellimuse GUID |
| **SubscriptionName** | Azure'i tellimuse nime |

See näide käsk on loetletud kõik Accessi muutused seitsme viimase päeva tellimus:

```
Get-AzureRMAuthorizationChangeLog -StartTime ([DateTime]::Now - [TimeSpan]::FromDays(7)) | FT Caller,Action,RoleName,PrincipalType,PrincipalName,ScopeType,ScopeName
```

![PowerShelli Get-AzureRMAuthorizationChangeLog - kuvatõmmis](./media/role-based-access-control-configure/access-change-history.png)

## <a name="create-a-report-with-azure-cli"></a>Azure'i CLI aruande loomine
Luua Accessi muutuste ajalugu aruande Azure käsurea liides (CLI), kasutage funktsiooni `azure role assignment changelog list` käsk.

## <a name="export-to-a-spreadsheet"></a>Ekspordi arvutustabelisse
Salvestage aruanne või töödelda andmeid, access muudatused CSV-faili eksportida. Saate vaadata aruande läbivaatamiseks arvutustabelis.

![Muutus vaadelda arvutustabeli - kuvatõmmis](./media/role-based-access-control-configure/change-history-spreadsheet.png)

## <a name="see-also"></a>Vt ka
- Alustamine [Azure Role-Based juurdepääsu reguleerimine](role-based-access-control-configure.md)
- [Kohandatud rollid Azure'i RBAC](role-based-access-control-custom-roles.md) töötamine
