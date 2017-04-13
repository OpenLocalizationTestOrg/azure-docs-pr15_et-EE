<properties
    pageTitle="Turvaline teie olulisi vault | Microsoft Azure'i"
    description="Võtme vault haldamise võlvid ja võtmed ja saladused juurdepääsu õiguste haldamine. Võtme hoidla ja kuidas tagada teie olulisi vault autentimise ja luba näidis"
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
    ms.date="10/07/2016"
    ms.author="ambapat"/>


# <a name="secure-your-key-vault"></a>Turvaline teie olulisi vault

Azure'i klahvi Vault on pilveteenuses, mida kaitseb krüptimise võtmed ja saladused cloud rakenduste (nt serdid, ühendusstringi, paroolid). Kuna andmed on tundlik ja business kriitilised, soovite turvaline juurdepääs teie olulisi võlvid nii, et ainult volitatud rakenduste ja kasutajad pääsevad võtme vault. Selles artiklis antakse ülevaade põhilistest vault Accessi mudeli selgitatakse autentimise ja luba ja kirjeldatakse, kuidas turvaline juurdepääs võtme vault pilveteenuste rakendusi ja näide.

## <a name="overview"></a>Ülevaade

Võtme vault juurdepääsu juhitakse kahte eraldi liidesed: halduse suhtes ning andmeid lennuk. Nii lennukite proper autentimise ja luba enne on vaja helistaja (kasutaja või rakendus) pääseksid juurde võtme vault. Autentimise luuakse helistaja nime, kuigi autoriseerimine määratleb, milliseid toiminguid helistaja on lubatud teha.

Autentimise halduse lennuk nii andmete lennuk kasutada Azure Active Directory. Luba, halduse lennuk kasutab Rollipõhine juurdepääsu reguleerimine (RBAC) ajal andmete lennuk kasutab võtme vault juurdepääsu poliitika.

Siin on lühiülevaade teemasid:

[Autentimist kasutades Azure Active Directory](#authentication-using-azure-active-direcrory) – see jaotis selgitab, kuidas helistaja autendib Azure Active Directory võtme vault halduse suhtes ning andmeid lennuk kaudu juurdepääsu. 

[Juhtimise suhtes ning andmeid lennuk](#management-plane-and-data-plane) - halduse suhtes ning andmeid lennuk on kahe Accessi tasandi oma võtme vault juurdepääsuks kasutada. Iga lennuk access toetab teatud toiminguid. Selles jaotises kirjeldatakse Accessi lõpp-punktid, toimingud toetatud ja juurdepääsu juhtimine meetodit kasutada iga parkimine. 

[Juhtimise lennuk juurdepääsu reguleerimine](#management-plane-access-control) – selles jaotises vaatame, mis võimaldab juurdepääsu lennuk toimingute abil Rollipõhine juurdepääsu reguleerimine.

[Andmete lennuk juurdepääsu reguleerimine](#data-plane-access-control) – selles jaotises kirjeldatakse, kuidas andmeid lennuk juurdepääsu kontrollimiseks kasutate tootenumbri vault juurdepääsu poliitika.

[Näide](#example) – selles näites kirjeldatakse, kuidas Setup juurdepääsu kontrolli teie olulisi vault lubamiseks kolme eri meeskondadel (Turvalisus meeskonnatöö, arendajad/tehtemärgid ja audiitorite) arendada, hallata ja jälgida rakenduse Azure kindlate toimingute sooritamiseks.


## <a name="authentication-using-azure-active-directory"></a>Azure Active Directory abil autentimine

Azure tellimuse võtme vault loomisel on seostatud automaatselt selle tellimuse Azure Active Directory rentniku. Kõik helistajad (kasutajad ja rakendused) peab olema seda rentniku juurdepääsu selle võtme vault registreeritud. Rakenduse või kasutaja peab autentimiseks Azure Active Directory võtme vault juurdepääsu. See kehtib nii halduse lennuk ja lennuk juurdepääsu andmetele. Mõlemal juhul pääseb rakenduse tootenumbri vault kahel viisil:

-  **kasutaja + rakenduse access** - tavaliselt see on rakendusi, et juurdepääs võtme vault sisselogitud kasutaja nimel. Azure'i PowerShelli, Azure portaali näited seda tüüpi juurdepääsu. On kasutajatele juurdepääsu andmiseks kaks võimalust: üks võimalus on nii, et klahv vault juurdepääsu mis tahes rakendusest ja teine võimalus on kasutajate juurdepääsu võtme vault anda ainult siis, kui nad kasutavad (nimetatakse liitindeksi identiteedi) konkreetse rakenduse kasutajatele juurdepääsu lubamine. 
-  **ainult rakenduse** - rakendusi, mis käivitada daemon teenused, tausta töö jne. Rakenduse identiteedi on antud võtme vault.

Mõlemat tüüpi rakendusi rakenduse autendib Azure Active Directory abil, mis tahes [toetatud autentimismeetodite](../active-directory/active-directory-authentication-scenarios.md) ja teenust märgiks. Sõltub rakenduse autentimise meetodit kasutada. Seejärel rakendus kasutab seda luba ja saadab taotluse REST API võti vault. Juhtimise lennuk juurdepääsu korral taotlused marsruuditakse Azure'i ressursihaldur lõpp-punkti kaudu. Andmete lennuk kasutamisel rakenduste räägib otse klahvi võlvkelder lõpp-punkti. [Kogu autentimise meilivoo](../active-directory/active-directory-protocols-oauth-code.md)kohta täpsema teabe kuvamiseks. 

Ressursi nimi taotleb rakenduse märgiks erineb sõltuvalt sellest, kas rakendus on juurdepääs haldus lennuk või andmete lennukiga. Seega ressursi nimi on kas halduse lennuk või andmete lennuk lõpp-punkti tabelis hiljem jaotises sõltuvalt Azure keskkonnas.

On ühe ühe süsteem nii haldus ja andmete lennuk autentimine on oma eelised.

- Ettevõtted saavad ühes keskses kohas kõik võtme võlvid oma ettevõtte juurdepääsu reguleerimine
- Kui kasutaja lahkub, nad kaotavad kiire juurdepääsu kõik võtme võlvid ettevõte
- Ettevõtted, saate kohandada autentimise kaudu Azure Active Directory (nt, mis võimaldab mitmikautentimise turvalisuse) suvandid

## <a name="management-plane-and-data-plane"></a>Juhtimise suhtes ning andmeid lennuk

Azure'i klahvi Vault on saadaval Azure ressursihaldur juurutamise mudeli Azure teenuse. Võtme vault loomisel saate virtuaalse ümbris sees, kus saate luua muude objektide, näiteks klahvid, saladusi ja serdid. Seejärel pääsete oma võtme vault halduse suhtes ning andmeid lennuk abil teatud toiminguid. Juhtimise lennuk kasutajaliidese abil saate hallata oma võtme vault, nt loomine, kustutamine, võtme vault atribuutide värskendamine ja juurdepääsupoliitikaid andmete lennuk seadmine. Andmete lennuk kasutajaliidese kasutatakse lisamine, kustutamine, muutmine ja klahvid, saladusi ja salvestatakse teie olulisi vault serdid.

Halduse lennuk ja andmete lennuk liidesed pääseb kaudu erinevate lõpp-punktid (vt tabel). Teine veerg tabelis kirjeldatakse nende lõpp-punktid viibite Azure DNS-i nimed. Kolmanda veeru kirjeldatakse toiminguid, mida saate teha iga Accessi kaugusel. Iga Accessi lennuk on ka eraldi juurdepääsu juhtimine süsteem: jaoks halduse lennuk juurdepääsu reguleerimine on seatud Azure Resource Manager Role-Based juurdepääsu juhtimine (RBAC) abil, samas andmete lennuk juurdepääsu reguleerimine on seatud võtme vault juurdepääsu poliitika abil.

| Accessi lennuk | Accessi lõpp-punktid | Toimingud | Süsteemi juurdepääsu juhtimine|
|--------------|------------------|--------------------|--------|
| Juhtimise lennuk|**Globaalne:**<br> Management.Azure.com:443<br><br> **Azure'i Hiina:**<br> Management.chinacloudapi.CN:443<br><br> **Azure'i USA valitsuse:**<br> Management.usgovcloudapi.net:443<br><br> **Azure'i Saksamaa:**<br> Management.microsoftazure.de:443 | Loomine ja lugemine/värskendamise ja kustutamise klahv vault <br> Võtme vault Accessi poliitika määramine<br>Võtme vault määramine Sildid | Azure'i ressursi halduri Rollipõhine juurdepääsu reguleerimine (RBAC) |
| Andmete lennuk | **Globaalne:**<br> &lt;Vault-nime&gt;. vault.azure.net:443<br><br> **Azure'i Hiina:**<br> &lt;Vault-nime&gt;. vault.azure.cn:443<br><br> **Azure'i USA valitsuse:**<br> &lt;Vault-nime&gt;. vault.usgovcloudapi.net:443<br><br> **Azure'i Saksamaa:**<br> &lt;Vault-nime&gt;. vault.microsoftazure.de:443 | Võtmed: Dekrüptida, krüptida, UnwrapKey, WrapKey, kinnitamine, logige, saamine, loendi, värskendada, luua, importida, kustutada, varundamine, taastamine<br><br> Saladusi: saamiseks loendis Sea kustutamine | Võtme vault juurdepääsu poliitika|

Juhtimise lennuk ja andmete lennuk Accessi juhtelemendid töötavad sõltumatult. Näiteks kui soovite rakenduse juurdepääsu klahvidega võtme võlvkelder, peate ainult võtme vault juurdepääsupoliitikaid abil andmete lennuk juurdepääsuõiguste andmiseks ja selle rakenduse jaoks on vaja halduse lennuk juurdepääs puudub. Ja vastupidi, kui soovite, et kasutaja saaks lugeda vault atribuudid ja sildid, kuid ei tohi olla mis tahes juurdepääsu klahvid, saladusi või serdid, saate anda sellele kasutajale 'Loe' juurdepääsu abil RBAC ja puudub juurdepääs, mis andmeid on vaja.

## <a name="management-plane-access-control"></a>Juhtimise lennuk juurdepääsu reguleerimine

Juhtimise lennuk koosneb võtme vault ise mõjutavad toimingud. Näiteks saate luua või kustutada võtme vault. Saate avada loendi võlvid tellimus. Saate tuua võtme vault atribuutide (nt SKU-ga, sildid) ja määrata klahv vault juurdepääsupoliitikaid, mis juhib kasutajate ja rakendused, mis pääsevad võtmed ja saladused võtme võlvkelder. Juhtimise lennuk juurdepääsu reguleerimine kasutab RBAC. Võtme vault toimingute kohta, mida saab teha kaudu halduse lennuki tabelis eelmise jaotise täieliku loendi leiate. 

### <a name="role-based-access-control-rbac"></a>Rollipõhine juurdepääsu reguleerimine (RBAC)
Iga Azure'i tellimus on Azure Active Directory. Kasutajate, rühmade ja rakenduste selle kataloogist saate anda juurdepääsu haldamine ressursid Azure'i tellimus, millest Azure'i ressursihaldur juurutamise mudeli kasutamine. Seda tüüpi juurdepääsu reguleerimine edaspidi Rollipõhine juurdepääsu reguleerimine (RBAC). Selle juurdepääsu haldamiseks saate kasutada [Azure portaali](https://portal.azure.com/), [Azure'i CLI tööriistad](../xplat-cli-install.md), [PowerShelli](../powershell-install-configure.md)või [Azure ressursihaldur REST API -d](https://msdn.microsoft.com/library/azure/dn906885.aspx).

Mudeliga Azure'i ressursihaldur loote oma võtme vault ressursi rühma ja juurdepääsu halduse tasapinnaga selle võtme vault Azure Active Directory abil. Näiteks saate anda kasutajatele või rühma võimalus võtme võlvid kindla ressursi rühma haldamine.

Sobiva RBAC rolli määramine anda juurdepääsu kasutajate, rühmade ja rakenduste teatud ulatuses. Näiteks haldamiseks võtme võlvid kasutajale juurdepääsu andmiseks soovite eelmääratletud rolli "klahv vault kaasautor" määrata sellele kasutajale teatud ulatuses. Ulatuse sel juhul oleks tellimus, ressursirühma või ainult teatud võtme vault. Rollile määratud tellimuse tasemel kehtib kõigi ressursside rühmad ja ressursside tellimuse. Kõik selle ressursirühma ressursside kehtib rollile määratud ressursi rühma tasemel. Mõne kindla ressursi roll kehtib ainult selle ressursi. Administraatorirolle on mitu eelmääratletud (vt [RBAC: sisseehitatud rollid](../active-directory/role-based-access-built-in-roles.md)), ja kui eelmääratletud rollid ei vasta teie vajadustele võite määrata ka oma rollid.

>[AZURE.IMPORTANT] Pange tähele, et kui kasutaja on kaasautori õigused (RBAC) klahv vault halduse punkti ta saab anda ise juurde, mis andmete seadmisega võtme vault juurdepääsu poliitikat, mis määrab juurdepääsu mis andmeid. Seetõttu on soovitatav kindlalt juhtelemendile, kellel on juurdepääs teie olulisi võlvid tagada ainult volitatud isikud pääsete juurde ja saate hallata oma võtme võlvid klahvid saladusi ja serdid "Kaasautor".

## <a name="data-plane-access-control"></a>Andmete lennuk juurdepääsu reguleerimine

Võtme vault andmete lennuk koosneb mis mõjuta objektide võtme võlvkelder, nt klahvid, saladusi ja serdid.  See hõlmab klahvi toimingud nagu loomise, importimine, värskendus-, loendi, varundamine ja taastamine klahvid, cryptographic toiminguid nagu märk, veenduge, krüptida, dekrüptida, Murra, ja lahtipakkimiseks ja määratud sildid ja muud atribuudid võtmed. Samuti saladusi see sisaldab, saamiseks määramine loendis käsku Kustuta.

Juurdepääs andmetele lennuk antakse seadmisega juurdepääsupoliitikaid võtme vault jaoks. Kasutaja, rühma või rakendus peab olema osalusõigused (RBAC) halduse lennuk võtme vault saama access selle võtme vault poliitika määramine. Kasutaja, rühma või rakendus võib anda juurdepääsu võtme võlvkelder klahvid või saladusi teatud toiminguid. võtme vault kasutajatugi võtme vault kuni 16 juurdepääsu poliitika kirjet. Azure Active Directory turvalisuse rühma loomine ja kasutajate lisamine rühma andmete lennuk juurdepääsu andmiseks mitmele kasutajale, et klahv vault.

### <a name="key-vault-access-policies"></a>võtme vault Juurdepääsupoliitikaid

võtme vault juurdepääsupoliitikaid klahvid, saladusi ja sertifikaatide õiguste andmine eraldi. Näiteks saate anda kasutaja juurdepääs ainult klahvid, kuid saladusi õigusi pole. Kuid juurdepääsuõigused klahvid või saladusi või serdid on tasemel vault. Teisisõnu, ei toeta võtme vault juurdepääsu poliitika objekti taseme õigusi. Abil saate [Azure portaali](https://portal.azure.com/), [Azure'i CLI tööriistad](../xplat-cli-install.md), [PowerShelli](../powershell-install-configure.md)või [klahv vault halduse REST API-de](https://msdn.microsoft.com/library/azure/mt620024.aspx) võtme vault Accessi poliitika määramine.

>[AZURE.IMPORTANT] Pange tähele, et klahv vault juurdepääsupoliitikaid vault tasemel. Näiteks, kui kasutaja on andnud õiguse loomine ja kustutamine klahvid, ta saab toiminguid need kõik võtmed selle võtme võlvkelder.

## <a name="example"></a>Näide

Oletame, et rakendus, mis kasutab SSL-i Azure storage andmete talletamiseks serti ja kasutab mõnda RSA 2048-bitine võti Logi toimingute arendate. Oletame, et see rakendus töötab VM (või VM skaala seadmine). Võtme vault abil saate talletada rakenduse saladusi ja võtme vault abil saate talletada bootstrap sert, mida kasutatakse autentimiseks Azure Active Directory abil.

Jah, siin on võtmed ja saladusi talletamise võtme võlvkelder kokkuvõte.

- **SSL-i Cert** - kasutada SSL
- **Salvestusruumi klahv** - kasutatud saada juurdepääsu salvestusruumi konto
- **RSA 2048-bitine võti** - märk toimingute puhul kasutada
- **Bootstrap serdi** – autentimiseks Azure Active Directory saada juurdepääsu võtme vault tõmmata salvestusruumi klahvi ja klahvi RSA allkirjastamiseks kasutatav.

Nüüd saame kokku inimesed, kes haldamise juurutamine ja auditeerimine see rakendus. Selles näites kasutame kolme rollid.

- **Turvalisus meeskonnatöö** – need on tavaliselt personali CSO (juht turvalisus) Office'ist või samaväärne, kes vastutab proper hoidmine saladusi, nt SSL-sertide RSA allkirjastamiseks ühendusstringi andmebaaside salvestusruumi konto klahvide abil.
- **Arendajad/tehtemärgid** – need inimesed, kes tekib see rakendus ja seejärel juurutada Azure. Tavaliselt nad ei kuulu Turvalisus meeskond, ja seega nad peaks olema juurdepääs tundlikke andmeid, nt SSL-sertide RSA võtmed, kuid need juurutamine rakendus peaks olema juurdepääs neile.
- **Audiitorite** – see on tavaliselt inimesed, arendajad ja personali üldine eraldatud teistsugused. Nende on õige kasutamise ja haldamise serdid, võtmed jne ja veenduge, et andmete turvalisus järgimise. 

On ühe rohkem rolli, mis on väljaspool selle rakenduse, kuid oluline siin nimetada, ja see oleks tellimuse (või ressursirühm) administraator. Tellimuse administraator häälestab algse juurdepääsuõigused turvalisus meeskonna jaoks. Siin me endale tellimuse administraator andnud juurdepääsu turvalisus meeskonnatöö mis kõigi vajalike ressursside selle rakenduse elukohaliikmesriigi ressursside rühma.

Nüüd vaatame, milliseid toiminguid iga rolli teeb selle rakenduse kontekstis.

- **Turvalisus meeskonnatöö**
    - Võtme võlvid loomine
    - Lülitab klahvi võlvkelder logimine
    - Klahvid/saladusi lisamine
    - Klahvid avariitaastet varukoopia loomine
    - Võtme vault Accessi poliitika õigused anda kasutajatele ja rakendustele teatud toiminguid määramine
    - Perioodiliselt tööks klahvid/saladused
- **Arendajad/tehtemärgid**
    - Viited bootstrap ja SSL-i certs (thumbprints) salvestusruumi võti (salajane URI) ja logida turvalisus meeskond key (võti URI)
    - Arendamise ja rakendus, mis kasutab võtmed ja saladused programmiliselt juurutamine
- **Audiitorite**
    - Läbivaatus kasutus logid õige võti/salajane kasutamise ja andmete turvalisus järgimise kinnitamiseks

Nüüd vaatame, milliseid põhilistest vault juurdepääsuõiguste iga rolli (ja rakenduse) oma tööülesannete täitmiseks vajavad. 

| Kasutajaroll    | Õiguste halduse lennuk | Andmete lennuk õigused |
|--------------|------------------------------|------------------------|
|Turvalisus meeskonnatöö|võtme vault kaasautor|Klahvid: varundada, loomine, kustutamine, saamine, importida, loendi, taastamine <br> Saladused: kõik|
|Arendajad/tehtemärk| võtme vault juurutada õigus nii, et nad juurutamine VMs saate toomiseks saladusi võtme hoidlast | Ükski |
|Audiitorite| Ükski | Klahvid: loend<br>Saladused: loend|
|Rakenduse| Ükski | Klahvid: märk<br>Saladused: hankimine |

>[AZURE.NOTE] Audiitorite vaja loendis luba võtmed ja saladused nii, et ta saab kontrollida võtmed ja saladusi, mis pole paisatakse logid, nt sildid, aktiveerimise ja aegumise kuupäeva.

Võtme vault õigus, lisaks kõigi kolme rolli vaja juurdepääsu muud ressursid. Näiteks saama juurutamine VMs (või Web Appsi jne). Samuti vajate arendajad ja tehtemärke 'Kaasautor' juurdepääsu nende ressursside puhul. Audiitorite vaja salvestusruumi konto võtme vault logid talletamiseks lugemisõigus.

Kuna selles artiklis keskendutakse on juurdepääs teie olulisi vault turvaliseks, me ainult kirjeldavad teemaga seotud asjakohaseid osi ja üksikasju juurutamine serdid, võtmed ja saladusi jne programmiliselt juurde pääsemiseks vahele jätta. Need andmed asuvad juba mujal. Juurutamine võtme vault vms talletatud serdid on kaetud [ajaveebipostituse](https://blogs.technet.microsoft.com/kv/2016/09/14/updated-deploy-certificates-to-vms-from-customer-managed-key-vault/)ja on [proovi kood](https://www.microsoft.com/download/details.aspx?id=45343) saadaval mis kujutab autentimiseks Azure AD saada juurdepääsu võtme vault bootstrap serdi kasutamise kohta.

Enamik juurdepääsu õigusi saab anda Azure'i portaalis, kuid Varundustöö õiguste andmine peate kasutama Azure'i PowerShelli (või Azure CLI) soovitud tulemuse saavutamiseks. 

Järgmine PowerShelli Koodilõigud endale:

- Azure Active Directory administraator on loonud turberühmad, mis tähistab kolme rollid, milleks Contoso Turvalisus meeskond, Contoso rakenduse Devops, Contoso rakenduse audiitorite. Administraator on lisatud ka kasutajate rühmad, nad kuuluvad.

- **ContosoAppRG** on ressursirühma, kus asuvad kõik ressursid. **contosologstorage** on, kus on salvestatud logid. 

- Võtme vault **ContosoKeyVault** ja salvestusruumi konto kasutatakse võtme vault logid **contosologstorage** peab olema Azure paigal


Esmalt tellimuse administraator määrab "Sisestage vault kaasautor" ja kasutajale juurdepääs administraatori rollid meeskonnatöö turvalisus. See võimaldab hallata muud ressursid juurdepääsu ja hallata võtme võlvid ressursi jaotises ContosoAppRG Turvalisus meeskond.

```
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "key vault Contributor" -ResourceGroupName ContosoAppRG
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "User Access Administrator" -ResourceGroupName ContosoAppRG
```

Järgmise skripti näitab, kuidas luua meeskonnatöö turvalisus võtme võlvikus logimine ja seadke juurdepääsuõigused teiste rollide ja rakenduse häälestamine. 

```
# Create key vault and enable logging
$sa = Get-AzureRmStorageAccount -ResourceGroup ContosoAppRG -Name contosologstorage
$kv = New-AzureRmKeyVault -VaultName ContosoKeyVault -ResourceGroup ContosoAppRG -SKU premium -Location 'westus' -EnabledForDeployment
Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent

# Data plane permissions for Security team
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoKeyVault -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -PermissionToKeys backup,create,delete,get,import,list,restore -PermissionToSecrets all

# Management plane permissions for Dev/ops
# Create a new role from an existing role
$devopsrole = Get-AzureRmRoleDefinition -Name "Virtual Machine Contributor"
$devopsrole.Id = $null
$devopsrole.Name = "Contoso App Devops"
$devopsrole.Description = "Can deploy VMs that need secrets from key vault"
$devlopsrole.AssignableScopes = @("/subscriptions/<SUBSCRIPTION-GUID>")

# Add permission for dev/ops so they can deploy VMs that have secrets deployed from key vaults
$devopsrole.Actions.Add("Microsoft.KeyVault/vaults/deploy/action")
New-AzureRmRoleDefinition -Role $role

# Assign this newly defined role to Dev ops security group
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso App Devops')[0].Id -RoleDefinitionName "Contoso App Devops" -ResourceGroupName ContosoAppRG

# Data plane permissions for Auditors
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoKeyVault -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso App Auditors')[0].Id -PermissionToKeys list -PermissionToSecrets list
```

Kohandatud rolli määratud, on ainult määratav tellimusega, kus luuakse ContosoAppRG ressursirühma. Kui sama kohandatud rollid, mida kasutatakse muude tellimuste teiste projektide, see on ulatus on lisatud mitu tellimust.

Kohandatud Rollimäärang arendajad/ettevõtjatele õiguse "juurutamine/toimingut" on rakendatud ressursirühma. Sellisel viisil ainult ressursirühma 'ContosoAppRG' loodud VMs saavad saladusi (SSL cert ja bootstrap cert). Mis tahes muud ressursirühm loob arendaja/ops meeskonna liikme VMs ei saa neid saladusi saada isegi siis, kui nad teadsid salajane URI-d.

Selles näites on kujutatud lihtsa stsenaariumi. Reaal elavamaks stsenaariumid võib keerukamaid ja vajalikud õigused teie olulisi vault vastavalt oma vajadustele kohandada. Oletame näiteks, selles näites me selle Turvalisus meeskond annab võtme ja salajane viited (URI-d ja thumbprints) selle arendajad/tehtemärgid meeskonnatöö tuleb viitamiseks oma rakendustes. Seega ei pea mis tahes andmete lennuk juurdepääsu andmiseks arendajad ja tehtemärke. Samuti võtke arvesse, et selles näites keskendub turvaliseks teie olulisi vault. Sarnaselt kaaluda tuleks tagada [teie VMs](https://azure.microsoft.com/services/virtual-machines/security/), [salvestusruumi kontod](../storage/storage-security-guide.md) ja muud ressursid, Azure liiga.

>[AZURE.NOTE] Märkus: See näitab, kuidas võtme vault Accessi lukustatud alla valmistamisel. Arendajad peaks olema oma tellimuse või resourcegroup, mille jaoks on neil täielikud õigused nende võlvid, VMs ja salvestusruumi konto haldamine kuhu nad välja rakenduse.


## <a name="resources"></a>Ressursid

-   [Azure Active Directory Rollipõhine juurdepääsu reguleerimine](../active-directory/role-based-access-control-configure.md)

    Selles artiklis selgitatakse Azure Active Directory Rollipõhine juurdepääsu reguleerimine ja kuidas see toimib.

-   [RBAC: Sisseehitatud rollid](../active-directory/role-based-access-built-in-roles.md)

    See artikkel üksikasjad saadaval RBAC sisseehitatud rollid.

-   [Ressursihaldur kasutuselevõtu ja juurutamise klassikaline mõistmine](../resource-manager-deployment-model.md)

    Selles artiklis selgitatakse ressursihaldur juurutus- ja klassikaline juurutamise mudeleid ja selgitatakse jaotiste ressursihaldur ja ressursi kasutamise eelised

-    [Rollipõhine juurdepääsu reguleerimine Azure PowerShelliga haldamine](../active-directory/role-based-access-control-manage-access-powershell.md)

     Selles artiklis selgitatakse, kuidas hallata Rollipõhine juurdepääsu reguleerimine Azure PowerShelli abil

-   [Rollipõhine juurdepääsu reguleerimine REST API-ga haldamine](../active-directory/role-based-access-control-manage-access-rest.md)

    Selles artiklis kirjeldatakse haldamiseks RBAC REST API kasutamise kohta.

-   [Rollipõhine juurdepääsu reguleerimine Microsoft Azure Ignite kaudu](https://channel9.msdn.com/events/Ignite/2015/BRK2707)

    See on link video kanali 9 2015 MS Ignite konverentsi. Selle seansi rääkida need juurdepääs haldus ja Azure aruandlusvõimalused ja analüüsida head tavad ümber juurdepääsu Azure tellimuste Azure Active Directory abil.

-   [Luba juurdepääs veebirakenduste OAuth 2.0 ja Azure Active Directory abil](../active-directory/active-directory-protocols-oauth-code.md)

    Selles artiklis kirjeldatakse täieliku OAuth 2.0 meilivoo Azure Active Directory autentimiseks.

-   [võtme vault halduse REST API-d](https://msdn.microsoft.com/library/azure/mt620024.aspx)

    Selles dokumendis on viide REST API-de haldamine oma võtme vault programmiliselt, sh seada võtme vault juurdepääsu poliitika.

-   [võtme vault REST API-d](https://msdn.microsoft.com/library/azure/dn903609.aspx)

    Võtme vault REST API dokumentides link.

-   [Võtme juurdepääsu reguleerimine](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_KeyAccessControl)

    Link võti juurdepääsu juhtimine dokumentides.

-   [Salajane juurdepääsu reguleerimine](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_SecretAccessControl)

    Link võti juurdepääsu juhtimine dokumentides.

-   [Määramine](https://msdn.microsoft.com/library/mt603625.aspx) ja [eemaldamine](https://msdn.microsoft.com/library/mt619427.aspx) võtme vault juurdepääsu poliitika PowerShelli abil

    Lingid viitavad PowerShelli cmdlet-käskude haldamine võtme vault juurdepääsu poliitika dokumentatsioonist.

## <a name="next-steps"></a>Järgmised sammud

Saamisega alustamise õpetus administraator, leiate [Azure'i võtme vault alustamine](key-vault-get-started.md).

Logimise võtme vault kasutuse kohta leiate lisateavet teemast [Azure võtme vault logimine](key-vault-logging.md).

Azure'i võtme vault võtmed ja saladused kasutamise kohta leiate lisateavet teemast [kohta võtmed ja saladused](https://msdn.microsoft.com/library/azure/dn903623.aspx).

Kui teil on küsimusi tootenumbri vault, külastage [Azure võtme vault Foorumid](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault)
