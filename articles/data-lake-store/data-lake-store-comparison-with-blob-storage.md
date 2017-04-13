<properties
   pageTitle="Azure'i andmesalve Lake võrdlus Azure'i salvestusruumi bloobimälu | Microsoft Azure'i"
   description="Azure'i andmesalve Lake Azure'i salvestusruumi bloobimälu võrdlus"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="08/15/2016"
   ms.author="nitinme"/>

# <a name="comparing-azure-data-lake-store-and-azure-blob-storage"></a>Azure'i andmed Lake poe ja Azure'i bloobimälu võrdlus

Selles artiklis tabelis on kokku võetud mööda olulisemad aspektid suur andmete töötlemise Azure'i andmesalve Lake ja Azure'i bloobimälu vahelised erinevused. Azure'i bloobimälu on üldine otstarve, scalable objekti poe, mis on mõeldud mitmesuguseid salvestusruumi stsenaariumid. Azure'i andmesalve Lake on hyper skaala hoidla, mis on optimeeritud suur andmed analytics töökoormus.

|    | Azure'i andmesalve Lake  | Azure'i bloobimälu  |
|----|-----------------------|--------------------|
| Otstarve  | Suur andmed analytics töökoormus optimeeritud salvestusruum                                                                          | Üldine otstarve objekti talletamine mitmesuguseid salvestusruumi stsenaariumid                                                                                |
| Kasutage juhul                                   | Partii, interaktiivsed, streaming analüüsi- ja masina õ andmed näiteks logifailid asjade andmeid, klõpsake voogu suurte andmekogumite | Mis tahes tüüpi teksti või binaarsed andmed, nt rakenduse uuesti end, varundatud andmete, meediumi salvestusruum andmete voogedastuse ja üldine eesmärk |
| Peamised mõisted                                | Lake andmesalve konto sisaldab kaustu, mis omakorda sisaldab failidena salvestatud andmed | Salvestusruumi konto on ümbriste, mis omakorda plekid vormi andmete |
| Struktuur | Hierarhiline failisüsteemis                                                                                                    | Tasapinnalise nimeruumi objekti pood  |
| API-GA   | Https REST API-ga | HTTP/HTTPS REST API-ga                                                                                                                            |
| Serveripoolne API                             | [WebHDFS ühilduv REST API-ga](https://msdn.microsoft.com/library/azure/mt693424.aspx)                                                                                                 | [Azure'i bloobimälu REST API-ga](https://msdn.microsoft.com/library/azure/dd135733.aspx)                                                                                                                         |
| Hadoopi fail süsteemi klient                   | Jah                                                                                                                         | Jah                                                                                                                                                 |
| Andmete toimingute - autentimine            | [Azure Active Directory identiteedid](../active-directory/active-directory-authentication-scenarios.md) põhjal | Ühiskasutusega saladused - [Konto Pääsuklahvide](../storage/storage-create-storage-account.md#manage-your-storage-account) ning [Ühiskasutusega Pääsuklahvide allkirja](../storage/storage-dotnet-shared-access-signature-part-1.md)alusel.                                                                       |
| Andmete toimingute - autentimine     | OAuth 2.0. Kõnede peab sisaldama kehtiv JWT (JSON Web Turbeloa) väljastatud Azure Active Directory                     | Sõnumi räsi-põhise autentimise koodi (HMAC-i). Kõnede peab sisaldama Base64-kodeeringuga SHA 256 räsi üle osa HTTP-päring. |
| Andmete toimingute – autoriseerimine               | POSIX pääsuloendid (ACL).  Azure Active Directory identiteedid põhjal ACL-ID, saate määrata failide ja kaustade tase. | Konto taseme autoriseerimine – kasutada [Konto kiirklahvide](../storage/storage-create-storage-account.md#manage-your-storage-account) jaoks<br>Konto, container või bloobimälu luba - kasutada [Ühiskasutusse antud allkirja kiirklahvide](../storage/storage-dotnet-shared-access-signature-part-1.md) |
| Andmete toimingute - kontrollimine                    | Saadaval. Leiate [siit](data-lake-store-diagnostic-logs.md) teavet.                                                                                                                   | Saadaval                                                                                                                                           |
| Ülejäänud andmete krüptimine                     | Läbipaistva, serveripoolne (tulekul)<ul><li>Teenuse hallatava võtmed</li><li>Kliendi hallatava võtmed Azure'i KeyVault abil</li></ul>| <ul><li>Läbipaistva, serveripoolne</li> <ul><li>Teenuse hallatava võtmed</li><li>Koos klientide hallatava võtmed Azure'i KeyVault (tulekul)</li></ul><li>Kliendipoolne krüptimine</li></ul> |
| Toimingute (nt konto loomine) | [Rollipõhine juurdepääsu reguleerimine](../active-directory/role-based-access-control-what-is.md) Konto haldamise kohta Azure esitatud (RBAC)                                                                       | [Rollipõhine juurdepääsu reguleerimine](../active-directory/role-based-access-control-what-is.md) Konto haldamise kohta Azure esitatud (RBAC)                                                                                               |
| Arendaja SDK-d                              | .NET, Java, Node.js                                                                                                         | .Net, Java, Python, Node.js, C++, Ruby                                                                                                              |
| Kasutusanalüüsi töökoormus jõudlus              | Optimeeritud jõudlus paralleelselt analytics töökoormus. Kõrge läbilaskevõime ja IOPS.                                           | Optimeeritud analytics töökoormus                                                                                                               |
| Mahupiirangud                                 | Konto suurused, failide või failide arvu piirangud                                                                   | Teatud piirangud dokumenteeritud [siin](../azure-subscription-service-limits.md#storage-limits)                                                                                                                     |
| Geo-koondamine                              | Kohalik – liigsete (andmete Azure piirkonna mitme eksemplari)                                                             | Kohalik liigsete (LRS), globaalselt liigsete (GRS), lugemisõigus – globaalselt liigsete (RA-GRS). Leiate [siit](../storage/storage-redundancy.md) veel teavet |
| Teenuse olek                               | Avaliku eelvaade                                                                                                              | Üldiselt kättesaadav                                                                                                                                 |
| Piirkondliku kättesaadavus  | Leiate [siit](https://azure.microsoft.com/regions/#services)| Leiate [siit](https://azure.microsoft.com/regions/#services) |
| Hind                                       |     Vt [hinnad](https://azure.microsoft.com/pricing/details/data-lake-store/)| Vt [hinnad](https://azure.microsoft.com/pricing/details/storage/) |

### <a name="next-steps"></a>Järgmised sammud

- [Azure'i andmesalve Lake ülevaade](data-lake-store-overview.md)
- [Lake andmesalve kasutamise alustamine](data-lake-store-get-started-portal.md)



