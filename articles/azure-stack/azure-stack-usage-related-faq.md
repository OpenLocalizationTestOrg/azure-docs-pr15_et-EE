<properties
    pageTitle="Kasutus-seotud KKK | Microsoft Azure'i"
    description="Azure'i virnas meetrit, võrdlus Azure kasutuse API, kasutus ja aja teatatud tõrkekoodid loendit."
    services="azure-stack"
    documentationCenter=""
    authors="AlfredoPizzirani"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="alfredop"/>

# <a name="azure-stack-usage-api-faqs"></a>Azure'i virnas kasutus API korduma kippuvad küsimused
Sellest artiklist leiate vastused korduma kippuvatele küsimustele API Azure'i virnas kasutuse kohta.

## <a name="what-meter-ids-can-i-see"></a>Milliseid meter ID-d saavad vaadata?

Praegu on kasutus teatatud võrgu, salvestusruumi ja Arvuta ressursi pakkujad.

| **Ressursi pakkuja** | **Arvesti ID** |**Arvesti nimi** | **Ühiku** | **Täiendav teave** |
| --------------------------- | --------------------------------------- | -------------------------- | ---------------------------- | ----------------------------------------- |
| **Võrgu** | f114cb19-ea64-40b5-bcd7-aee474b62853 | Avaliku IP-aadressi kasutamine | IP-aadress |                    
| **Salvestusruumi**  | B4438D5D-453B-4EE1-B42A-DC72E377F1E4 | TableCapacity | GB\*tundi | Tabelite tarbitud koguvõimsus |
|              | B5C15376-6C94-4FDD-B655-1A69D138ACA3 | PageBlobCapacity | GB\*tundi | Lehe plekid tarbitud koguvõimsus |
|              | B03C6AE7-B080-4BFA-84A3-22C800F315C6 | QueueCapacity  | GB\*tundi  | Koguvõimsus tarbitud järjekord |
| | 09F8879E-87E9-4305-A572-4B7BE209F857 | BlockBlobCapacity | GB\*tundi  | Blokeeri plekid tarbitud koguvõimsus |
| | B9FF3CD0-28AA-4762-84BB-FF8FBAEA6A90 | TableTransactions  | Taotluse arvu 10,000s   | Tabeli hooldustaotlused (jaotises 10,000s) |
| | 50A1AEAF-8ECA-48A0-8973-A5B3077FEE0D | TableDataTransIn | Sissepääsu andmete GB | Tabeli teenuse andmete hõlmav GB |
| | 1B8C1DEC-EE42-414B-AA36-6229CF199370 | TableDataTransOut | Outgress GB | Tabeli teenuse andmete sealt GB |
| | 43DAF82B-4618-444A-B994-40C23F7CD438 | BlobTransactions | Taotluste arvu 10,000s | Bloobimälu päringuid (jaotises 10,000s) |
| | 9764F92C-E44A-498E-8DC1-AAD66587A810   | BlobDataTransIn    | Sissepääsu andmete GB          | Bloobimälu teenuse andmete hõlmav GB 
| | 3023FEF4-ECA5-4D7B-87B3-CFBC061931E8   | BlobDataTransOut   | Outgress GB              | Bloobimälu teenuse andmete sealt GB 
| | EB43DD12-1AA6-4C4B-872C-FAF15A6785EA   | QueueTransactions  | Taotluste arvu 10,000s   | Järjekorda päringuid (jaotises 10,000s) 
| | E518E809-E369-4A45-9274-2017B29FFF25   | QueueDataTransIn          | Sissepääsu andmete GB         | Järjekorra teenuse andmete hõlmav GB 
| | DD0A10BA-A5D6-4CB6-88C0-7D585CEF9FC2   | QueueDataTransOut         | Outgress GB  | Järjekorda teenuse andmete sealt GB 
| **Arvuta** | 6DAB500F-A4FD-49C4-956D-229BB9C8C793 | Tunni VM suurus | Virtuaalse masina suurus |



## <a name="how-do-the-azure-stack-usage-apis-compare-to-the-azure-usage-apihttpsmsdnmicrosoftcomlibraryazure1ea5b323-54bb-423d-916f-190de96c6a3c-currently-in-public-preview"></a>Kuidas Azure'i virnas kasutus API-de võrrelda [Azure kasutuse API](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) (praegu avaliku eelvaade)?

-   Rentniku kasutus API vastab Azure'i API, ühe erandiga: *showDetails* lipu praegu ei toetata Azure'i virnas.

-   Pakkuja kasutuse API kehtib ainult Azure virnas.

-   Praegu pole saadaval, Azure'i virnas [RateCard API](https://msdn.microsoft.com/en-us/library/azure/mt219004.aspx) , mis on saadaval Azure.

## <a name="what-is-the-difference-between-usage-time-and-reported-time"></a>Mis on kasutus ja teatatud aja erinevus?

Kasutusaruannete andmeid on kaks peamist kellaajaväärtuse.

-   **Teatatud aeg**. Kui sündmus kasutus Sisestatud kasutus süsteemi aeg

-   **Kasutusaeg**. Kui Azure virnas ressursi oli tarbitud aeg

Teatud kasutus sündmuse võidakse kuvada jaoks kasutus ja teatatud aja erinevus väärtused. Viivitus võib olla nii kaua, kui mitu tundi mis tahes keskkonnas.

Praegu saate päringu *ainult teatatud kellaaja alusel*.

## <a name="what-do-these-usage-api-error-codes-mean"></a>Mida tähendavad need kasutus API tõrkekoodid?

| **HTTP olekukoodi** | **Tõrkekood** | **Kirjeldus** |
| ---------------------- | ------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------ |
| 400/vigane päring        | *NoApiVersion*     | Päringuparameetri *api-versioon* on puudu.
| 400/vigane päring        | *InvalidProperty*  | Atribuut puudub või on sobimatu väärtuse. Puuduvate atribuudi tuvastamiseks tõrkekood vastuse kehasse sõnum.
| 400/vigane päring        | *RequestEndTimeIsInFuture*  | *ReportedEndTime* väärtus tulevikus. Väärtused tulevikus pole lubatud selle argumendi.
| 400/vigane päring        | *SubscriberIdIsNotDirectTenant*    | Pakkuja API kõne kasutada tellimuse ID, mis pole kehtiv rentnik helistaja.
| 400/vigane päring        | *SubscriptionIdMissingInRequest*   | Tellimuse ID helistaja on puudu.
| 400/vigane päring        | *InvalidAggregationGranularity*   | Mõne sobimatu koondamine granulaarsus on nõutud. Sobivad väärtused on iga päev ja kord tunnis.
| 503                    | *ServiceUnavailable*   | Retryable tõrge ilmnes, kuna teenus on hõivatud või kõne on on rakendus. |

## <a name="next-steps"></a>Järgmised sammud
[Kliendi arve ja tagasinõude Azure'i virnas](azure-stack-billing-and-chargeback.md)

[Pakkuja ressursikasutus API](azure-stack-provider-resource-api.md)

[Rentniku ressursikasutus API](azure-stack-tenant-resource-usage-api.md)
