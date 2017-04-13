<properties
   pageTitle="Väljalaskemärkmed Vault .NET 2.x API võti | Microsoft Azure'i"
   description=".NET arendajad kasutab seda API koodi Azure'i klahvi Vault"
   services="key-vault"
   documentationCenter=""
   authors="BrucePerlerMS"
   manager="mbaldwin"
   editor="bruceper" />
<tags
   ms.service="key-vault"
   ms.devlang="CSharp"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/07/2016"
   ms.author="bruceper" />

# <a name="azure-key-vault-net-20---release-notes-and-migration-guide"></a>Azure'i võtme Vault .NET 2.0 – väljalaskemärkmed ja Migreerimisjuhend

Järgmises märkmed ja juhised on arendajatele töötamine Azure'i klahvi Vault .NET / C# teek. Muuda 1.0 versioonilt 2.0 versioon, värskenduste arv on tehtud nõua koodi võimaldamaks teil kasu funktsionaalne parandused ja funktsioonide nagu võti Vault serdid tugi migreerimise töö.

Klahv Vault serdid tugi pakub teie x509 haldamiseks serdid ja järgmised olukorrad:  

-   Võimaldab serdi omanik klahvi Vault loomise protsessi või olemasolev tunnistus importimise kaudu serdi loomise kohta. See hõlmab nii iseallkirjastatud ja sertimiskeskus loodud serdid.

- Klahv Vault serdi omanik rakendada turvaline salvestamine ja X509 haldamine võimaldab ilma suhtlemine privaatse võtme materjali serdid.  

-   Võimaldab serdi omanik, mis suunab klahvi Vault haldamiseks serdi elutsükli poliitika loomiseks.  

-   Võimaldab serdi omanikud edastate teatamise elutsükli sündmuste kohta aegumine ja uuendamise sert.  

-   Toetab automaatse pikendamise koos valitud emitentide - klahv Vault partneri X509 pakkujate certificate / certificate authorities.
    - Pange tähele - partneriks pakkujate/asutused on ka lubatud, kuid ei toeta funktsiooni automaatne pikendamine.


## <a name="net-support"></a>.Net-i tugi
- Azure'i klahvi Vault .NET 2.0 versioon ei toeta **.NET 4.0** ja C# teek
- Azure'i klahvi Vault .NET 2.0 versioon toetab **.NET Core** / C# teek

## <a name="namespaces"></a>Nimeruumid
- Nimeruumi **mudelid** on muutunud **Microsoft.Azure.KeyVault** , **Microsoft.Azure.KeyVault.Models**.
- Nimeruumi **Microsoft.Azure.KeyVault.Internal** katkeb.
- Azure'i SDK nimeruumi sõltuvuste muudetakse **Hyak.Common** ja **Hyak.Common.Internals** **Microsoft.Rest** ja **Microsoft.Rest.Serialization**


## <a name="type-changes"></a>Tippige muudatused
- *Salajane* muutunud *SecretBundle*
- *Sõnastiku* muutunud *IDictionary*
- *Loendi<T>, [] tekstistringi* muutunud *IListi<T> *
- *NextList* muutunud *NextPageLink*


## <a name="return-types"></a>Saatja tüübid
- Tagastab **KeyList** ja **SecretList** *IPage<T> * *ListKeysResponseMessage* asemel
- Loodud **BackupKeyAsync** tagastab *BackupKeyResult* , mis sisaldab *väärtust* (varukoopia bloobimälu). Enne oli pakitud meetodit ja esitus ainult väärtuse.

## <a name="exceptions"></a>Erandid
- *KeyVaultErrorException* *KeyVaultClientException* on muutunud
- Teenuse tõrge on muutunud *erand. Tõrge* *erandit. Body.Error.Message*.
- Eemaldatud täiendav teave kuvatakse tõrketeade: **[JsonExtensionData]**.

## <a name="constructors"></a>Ehitajatel
- Mitte nõustumine *HttpClient* ehitaja argumendiks, ehitaja aktsepteerib ainult *HttpClientHandler* või *DelegatingHandler []*.



## <a name="downloaded-packages"></a>Allalaaditud paketid  
Kui mõnda muud klienti töötleb sõltuvus klahvi Vault järgmist olid alla laadida
#### <a name="previous-package-list"></a>Eelmise paketi loend
- versiooni paketti id="Hyak.Common" = "1.0.2" targetFramework = "net45"
- versiooni paketti id="Microsoft.Azure.Common" = "2.0.4" targetFramework = "net45"
- versiooni paketti id="Microsoft.Azure.Common.Dependencies" = "1.0.0" targetFramework = "net45"
- versiooni paketti id="Microsoft.Azure.KeyVault" = "1.0.0" targetFramework = "net45"
- versiooni paketti id="Microsoft.Bcl" = "1.1.9" targetFramework = "net45"
- versiooni paketti id="Microsoft.Bcl.Async" = "1.0.168" targetFramework = "net45"
- versiooni paketti id="Microsoft.Bcl.Build" = "1.0.14" targetFramework = "net45"
- versiooni paketti id="Microsoft.Net.Http" = "2.2.22" targetFramework = "net45"

#### <a name="current-package-list"></a>Praeguse paketi loend
- versiooni paketti id="Microsoft.Azure.KeyVault" = "2.0.0-preview" targetFramework = "net45"
- versiooni paketti id="Microsoft.Rest.ClientRuntime" = "2.2.0" targetFramework = "net45"
- versiooni paketti id="Microsoft.Rest.ClientRuntime.Azure" = "3.2.0" targetFramework = "net45"


## <a name="class-changes"></a>Klassi muudatused

- Klassi **UnixEpoch** on eemaldatud
- Klassi **Base64UrlConverter** on uueks nimeks **Base64UrlJsonConverter**

## <a name="other-changes"></a>Muud muudatused

- KV toimingu uuesti poliitika siirdamiseks tõrkeid konfiguratsiooni tugi on lisatud API selles versioonis.



## <a name="microsoftazuremanagementkeyvault-nuget"></a>Microsoft.Azure.Management.KeyVault Nugeti
- Toimingute, tagastatakse *vault*, on tagastuse tüüp klassi, kus asunud Vault atribuut. Tagastuse tüüp on nüüd *Vault*.
- *PermissionsToKeys* ja *PermissionsToSecrets* on nüüd *Permissions.Keys* ja *Permissions.Secrets*
- Saatja tüüpi muudatuste rakendamine juhtelemendi punkti ka.

## <a name="microsoftazurekeyvaultextensions-nuget"></a>Microsoft.Azure.KeyVault.Extensions Nugeti
- Paketi on katki **Microsoft.Azure.KeyVault.Extensions** ja **Microsoft.Azure.KeyVault.Cryptography** krüptograafia toimingute.
