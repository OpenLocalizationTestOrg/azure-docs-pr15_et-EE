<properties
    pageTitle="Pärast tellimuse teisaldamine võtme vault rentniku ID muuta | Microsoft Azure'i"
    description="Saate teada, kuidas aktiveerida tootenumbri vault rentniku ID-d pärast tellimuse teisaldatakse erinevate rentniku"
    services="key-vault"
    documentationCenter=""
    authors="amitbapat"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/13/2016"
    ms.author="ambapat"/>

# <a name="change-a-key-vault-tenant-id-after-a-subscription-move"></a>Võtme vault rentniku ID muutmine pärast tellimuse teisaldamine
### <a name="q-my-subscription-was-moved-from-tenant-a-to-tenant-b-how-do-i-change-the-tenant-id-for-my-existing-key-vault-and-set-correct-acls-for-principals-in-tenant-b"></a>K: minu tellimus on teisaldatud kaudu rentniku A rentniku B. Kuidas muuta rentniku ID jaoks oma olemasoleva võtme vault ja õige ACL-ID määramine põhisumma rentniku B?

Kui loote uue tootenumbri vault tellimus, on see automaatselt seotud vaikimisi Azure Active Directory rentniku ID selle tellimuse jaoks. Kõigi Accessi poliitika kirjete sõltuvad sellest rentniku ID-ga. Azure'i tellimuse teisaldamisel rentniku A rentniku B oma olemasoleva võtme võlvid ei pääse, põhisumma (kasutajad ja rakendused) rentniku B. Selle probleemi lahendamiseks peate:

- Muuda rentniku ID seotud kõik olemasolevad võtme võlvid see tellimus rentniku B.
- Eemaldage kõik olemasolevad juurdepääsu poliitika kirjet.
- Lisage uus Accessi poliitika kirjed, mis on seotud rentniku B.

Näiteks kui teil on tellimus, mis on teisaldatud kaudu võtme vault 'myvault' sihtrentnikus A rentniku B, tehke selle võtme vault rentniku ID-d muuta ja vana juurdepääsupoliitikaid eemaldamine.

<pre>
$vaultResourceId = (get-AzureRmKeyVault - VaultName myvault). ResourceIdkasutamisel $vault = Get-AzureRmResource – ResourceIdkasutamisel $vaultResourceId - ExpandProperties $vault. Properties.TenantId = (Get-AzureRmContext). Tenant.TenantId $vault. Properties.AccessPolicies = @() Set-AzureRmResource - ResourceIdkasutamisel $vaultResourceId-$vault atribuudid. Atribuudid
</pre>

Kuna see vault oli rentniku A enne liikuda, **$vault esialgse väärtuse. Properties.TenantId** on rentniku A, samal ajal **(Get-AzureRmContext). Tenant.TenantId** on rentniku B.

Nüüd, kui teie vault on õige rentniku ID-ga seostatud ja vana Accessi poliitika kirjed on eemaldatud, määrake uue Accessi [Set-AzureRmKeyVaultAccessPolicy](https://msdn.microsoft.com/library/mt603625.aspx)poliitika kirjeid.

## <a name="next-steps"></a>Järgmised sammud

Kui teil on küsimusi Azure'i klahvi Vault, külastage [Azure'i klahvi Vault foorumeid](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault).
