<properties
   pageTitle="Azure'i salvestusruumi turvalisuse ülevaade | Microsoft Azure'i"
   description=" Azure'i salvestusruumi on pilveteenuse salvestusruumi lahendus tänapäevased rakendused kestvus, kättesaadavus ja nende klientide vajaduste rahuldamiseks skaleeritavus. Selles artiklis antakse ülevaade tuum Azure turbefunktsioonid, mida saab kasutada Azure Storage. "
   services="security"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/16/2016"
   ms.author="terrylan"/>

# <a name="azure-storage-security-overview"></a>Azure'i salvestusruumi turvalisuse ülevaade

Azure'i salvestusruumi on pilveteenuse salvestusruumi lahendus tänapäevased rakendused kestvus, kättesaadavus ja nende klientide vajaduste rahuldamiseks skaleeritavus. Azure'i salvestusruumi pakub turvalisus võimaluste täielik kogum.

- Salvestusruumi konto saate turvatud Rollipõhine juurdepääsu reguleerimine ja Azure Active Directory abil.
- Andmeid saab turvatud vahelise rakendus ja Azure kliendipoolne krüptimist, HTTPS või SMB 3.0 abil.
- Andmeid saab seada automaatselt krüptimist kui kirjutatud Azure Storage salvestusruumi krüptimine.
- Saate seada OS- ja ketast, mis kasutavad virtuaalmasinates olema krüptitud Azure'i ketas.
- Delegeeritud juurdepääsu andmeobjektid Azure Storage saab anda ühiskasutusse Accessi allkirjade abil.
- Autentimise meetodit kasutada keegi salvestusruumi juurdepääsemisel saab jälgida salvestusruumi Kasutusanalüüsi abil.

Täpsemat teavet turvalisuse Azure Storage, leiate [Azure'i salvestusruumi turvalisuse juhend](../storage/storage-security-guide.md). Sellest juhendist leiate sügav sukelduvad Azure Storage turbefunktsioonid salvestusruumi konto klahvid, nt ning ülejäänud ja salvestusruumi analytics andmete krüptimine.

Selles artiklis antakse ülevaade Azure turbefunktsioonid, mida saab kasutada Azure Storage. Lingid on esitatud artiklitele, mis üksikasjad iga funktsiooni nii saate lisateavet.

Siin on selles artiklis hõlmatud põhilisi funktsioone.

- Rollipõhine juurdepääsu reguleerimine
- Delegeeritud juurdepääsu salvestusruumi objektid
- Krüptimine
- Krüptimist ja ülejäänud/salvestusruumi teenuse krüptimine
- Azure'i ketta krüptimine
- Azure'i võtme Vault

## <a name="role-based-access-control-rbac"></a>Rollipõhine juurdepääsu reguleerimine (RBAC)

Saate tagada salvestusruumi konto Rollipõhine juurdepääsu reguleerimine (RBAC). [Teadma](https://en.wikipedia.org/wiki/Need_to_know) ja [vähimate](https://en.wikipedia.org/wiki/Principle_of_least_privilege) turvalisus põhimõtteid juurdepääsu piiramine on väga oluline ettevõtted, mida soovite rakendada turbepoliitikate andmepääsu. Järgmised juurdepääsuõigused on antud vastav RBAC rolli määramine, rühmad ja teatud ulatusega rakendused. [Sisseehitatud RBAC-rollid](../active-directory/role-based-access-built-in-roles.md), nagu salvestusruumi konto kaasautor, saate määrata kasutajatele õigusi.

Lisateave:

- [Azure Active Directory Rollipõhine juurdepääsu reguleerimine](../active-directory/role-based-access-control-configure.md)

## <a name="delegated-access-to-storage-objects"></a>Delegeeritud juurdepääsu salvestusruumi objektid

Ühiskasutusega juurdepääsu signatuuri (SAS) pakub delegeeritud juurdepääsu teie salvestusruumi konto ressursid. MUUDE tähendab, et saate anda mõnda muud klienti piiratud õiguste objektidele kontol salvestusruumi ja määratud õiguste kogumi määratud perioodi kohta. Saate anda ühiskasutusse anda oma konto kiirklahvide ilma neid piiratud õiguste. MUUDE on URI, mis hõlmab kogu teavet, mis on vajalikud autenditud juurdepääsu ressursile salvestusruumi oma päringu parameetrid. Salvestusruumi ressursid koos muude juurdepääsu kliendi ainult tuleb läbida muude vastava konstruktori või meetod.

Lisateave:

- [SAS andmemudeli mõistmine](../storage/storage-dotnet-shared-access-signature-part-1.md)
- [Loomine ja kasutamine on SAS bloobimälu](../storage/storage-dotnet-shared-access-signature-part-2.md)

## <a name="encryption-in-transit"></a>Krüptimine
Krüptimine on andmete kaitsmiseks, kui see on edastatud võrkude. Azure Storage saate turvaline andmete abil:

- [Transport taseme krüptimist](../storage/storage-security-guide.md#encryption-in-transit), näiteks HTTPS andmete edastamisel või sealt välja Azure Storage.
- [Kaabel krüptimist](../storage/storage-security-guide.md#using-encryption-during-transit-with-azure-file-shares), nt Azure Failikettad SMB 3.0 krüptimine.
- [Kliendipoolne krüptimist](../storage/storage-security-guide.md#using-client-side-encryption-to-secure-data-that-you-send-to-storage), andmete krüptimiseks enne, kui see on üle salvestusruumi ja andmeid dekrüptida, kui see on üle välja salvestusruumi.

Lisateave kliendipoolne krüptimine.

- [Kliendipoolne krüptimise Microsoft Azure Storage](https://blogs.msdn.microsoft.com/windowsazurestorage/2015/04/28/client-side-encryption-for-microsoft-azure-storage-preview/)
- [Cloud security määrab sarja: töödeldavate andmete krüptimine](http://blogs.microsoft.com/cybertrust/2015/08/10/cloud-security-controls-series-encrypting-data-in-transit/)

## <a name="encryption-at-rest"></a>Ülejäänud krüptimist

Paljud ettevõtted, [krüptimist ja ülejäänud andmed](https://blogs.microsoft.com/cybertrust/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/) on kohustuslikud samm andmete privaatsust, nõuetele vastavus ja andmete suveräänsuse. On kolme Azure funktsioone, mis pakuvad krüptimise andmeid, mis on "ülejäänud".

- [Salvestusruumi teenuse krüptimise](../storage/storage-security-guide.md#encryption-at-rest) saate nõuda, et teenuse salvestusruumi automaatselt krüptida andmete Azure'i mäluruumi kirjutamisel.
- [Kliendipoolne krüptimise](../storage/storage-security-guide.md#client-side-encryption) pakub krüptimist ja ülejäänud funktsiooni.
- [Azure'i ketta krüptimise](../storage/storage-security-guide.md#using-azure-disk-encryption-to-encrypt-disks-used-by-your-virtual-machines) võimaldab krüptida OS ketast ja andmete ketast kasutada ka IaaS virtual masina järgi.

Lugege lisateavet mäluruumi teenuse krüptimise:

- [Azure'i salvestusruumi teenuse krüptimine](https://azure.microsoft.com/services/storage/) on saadaval [Azure'i bloobimälu](https://azure.microsoft.com/services/storage/blobs/). Muud tüüpi Azure salvestusruumi kohta täpsema teabe saamiseks vt [faili](https://azure.microsoft.com/services/storage/files/), [ketta (Premium salvestusruumi)](https://azure.microsoft.com/services/storage/premium-storage/), [tabeli](https://azure.microsoft.com/services/storage/tables/)ja [järjekorda](https://azure.microsoft.com/services/storage/queues/).
- [Azure'i salvestusruumi teenuse krüptimine ülejäänud andmed](../storage/storage-service-encryption.md)

## <a name="azure-disk-encryption"></a>Azure'i ketta krüptimine

Azure'i virtuaalmasinates (VM) ketta krüptimine aitab teil aadressi ettevõtte turvalisus ja nõuetele oma VM ketast (sh buutimine ja andmete ketast) võtmed ja poliitikate juhite [Azure'i klahvi](https://azure.microsoft.com/services/key-vault/)võlvkelder krüptides.

Ketta krüptimise vms töötab Linuxi ja Windowsi operatsioonisüsteemi. Samuti kasutab klahvi Vault aitab kaitsta, hallata ja audit kasutamine oma ketta krüptimise võtmed. VM-ketta kõik andmed on krüptitud veebisaidil ülejäänud kontode Azure Storage industry-standard krüptimistehnoloogiale abil. Windowsi ketta krüptimise lahendus põhineb [Microsoft BitLockeri draivi krüptimine](https://technet.microsoft.com/library/cc732774.aspx)ja Linux lahendus põhineb [dm-crypt](https://en.wikipedia.org/wiki/Dm-crypt).

Lisateave:

- [Azure'i ketta krüptimise Windowsi ja Linux IaaS Virtuaalmasinates](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0)

## <a name="azure-key-vault"></a>Azure'i võtme Vault

Azure'i ketta krüptimine kasutab [Azure klahvi Vault](https://azure.microsoft.com/services/key-vault/) aitab teil määrata, ja ketta krüptimise võtmed ja saladusi tellimuse võtme vault, tagades, et virtuaalse masina ketta kõik andmed on krüptitud korral Azure'i salvestusruumi haldamine. Kasutage klahvi Vault auditeeritavad võtmed ja Poliitikakasutuse.

Lisateave:

- [Mis on Azure klahvi Vault?](../key-vault/key-vault-whatis.md)
- [Azure'i klahvi Vault kasutamise alustamine](../key-vault/key-vault-get-started.md)
