
<properties
   pageTitle="Luua turvalist teenuse struktuuri kobar Azure'i ressursihaldur abil | Microsoft Azure'i"
   description="Selles artiklis kirjeldatakse, kuidas luua turvalist teenuse struktuuri kobar Azure kliendi autentimiseks Azure'i ressursihaldur, Azure'i klahvi Vault ja Azure Active Directory (AAD) abil."
   services="service-fabric"
   documentationCenter=".net"
   authors="chackdan"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/25/2016"
   ms.author="vturecek"/>

# <a name="create-a-service-fabric-cluster-in-azure-using-azure-resource-manager"></a>Azure'i ressursihaldur abil Azure teenuse struktuuri kobar loomine

> [AZURE.SELECTOR]
- [Azure'i ressursihaldur](service-fabric-cluster-creation-via-arm.md)
- [Azure'i portaal](service-fabric-cluster-creation-via-portal.md)

See on samm-sammult juhendi, mis juhendab teid turvaline Azure teenuse struktuuri kobar Azure abil Azure'i ressursihaldur häälestamise juhiseid. Sellest juhendist juhendab teid järgmist:

 - Klahv Vault häälestada klahvid kobar-ja rakenduste haldamine.
 - Luua turvalise kobar Azure'i Azure ressursihaldur.
 - Kasutajate ja Azure Active Directory (AAD) kobar haldamiseks autentimiseks.

Turvaline kobar on klaster, mis takistab volitamata juurdepääsu haldamise toiminguid, mis sisaldab juurutamine, üleminekut ja rakenduste ja teenuste sisalduvate andmete kustutamist. Mõne ebaturvaliste klaster on kobar, et keegi ühenduse igal ajal ja haldamise toiminguid. Kuigi see on võimalik luua ka ebaturvaliste kobar, on **soovitatav luua turvalist kobar**. Mõne ebaturvaliste kobar **ei saa turvatud hiljem** - tuleb luua uus klaster.

Mõisted on samad luua turvalist kogumite, kas rühmad on Linux kogumite või Windowsi kogumite. Rohkem teavet ja helper skriptide loomise turvalist Linux kogumite, leiate teemast [loomine turvaline kogumite Linux](#secure-linux-clusters)

## <a name="log-in-to-azure"></a>Azure'i sisse logida
Sellest juhendist kasutab [Azure PowerShelli][azure-powershell]. Uue seansi PowerShelli käivitamisel Azure'i kontosse sisse logida ja valige oma tellimuse enne Azure käsud.

Azure'i kontosse sisse logida:

```powershell
Login-AzureRmAccount
```

Valige oma tellimus:

```powershell
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

## <a name="set-up-key-vault"></a>Klahv Vault häälestamine

Selles jaotises tutvustatakse klahvi Vault teenuse struktuuri kobar Azure ja teenuse struktuuri rakenduste loomine. Täielik juhend klahvi Vault, vaadake [klahvi Vault alustamisjuhendit][key-vault-get-started].

Teenuse struktuuri kasutab secure klaster ja esitage rakenduse turbefunktsioonid X.509 serdid. Azure'i klahvi Vault abil saate hallata teenuse struktuuri kogumite Azure serdid. Klaster juurutamisel Azure teenuse struktuuri kogumite loomise eest vastutav Azure ressursi pakkuja tõmbab klahvi hoidlast serdid ja installib need kobar VMs.

Järgmine diagramm näitab klahv Vault, teenuse struktuuri kobar ja Azure ressursi pakkuja serdid talletatud klahvi Vault loob klaster kasutava seos.

![Serdi installimine][cluster-security-cert-installation]

### <a name="create-a-resource-group"></a>Ressursi rühma loomine

Esimene samm on luua ressursirühma spetsiaalselt klahvi Vault. Klahv Vault kasutusele oma ressursirühm on soovitatav. See võimaldab teil eemaldada Arvuta ja salvestusruumi ressursi rühmad, sh selle ressursirühma, mis sisaldab teie teenuse struktuuri klaster oma võtmed ja saladused kaotamata. Ressursirühm, mis sisaldab teie võti Vault peab olema sama piirkonna kobar, mis kasutab seda.

```powershell

    New-AzureRmResourceGroup -Name mycluster-keyvault -Location 'West US'
    WARNING: The output object type of this cmdlet is going to be modified in a future release.
    
    ResourceGroupName : mycluster-keyvault
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/<guid>/resourceGroups/mycluster-keyvault

```

### <a name="create-key-vault"></a>Võtme Vault loomine 

Saate luua uue ressursirühma klahvi Vault. Selle klahvi Vault **peab olema lubatud juurutamiseks** suurendamiseks teenuse struktuuri ressursi pakkuja sertide ja installi kobar sõlmed:

```powershell

    New-AzureRmKeyVault -VaultName 'myvault' -ResourceGroupName 'mycluster-keyvault' -Location 'West US' -EnabledForDeployment
    
    
    Vault Name                       : myvault
    Resource Group Name              : mycluster-keyvault
    Location                         : West US
    Resource ID                      : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault
    Vault URI                        : https://myvault.vault.azure.net
    Tenant ID                        : <guid>
    SKU                              : Standard
    Enabled For Deployment?          : False
    Enabled For Template Deployment? : False
    Enabled For Disk Encryption?     : False
    Access Policies                  :
                                       Tenant ID                :    <guid>
                                       Object ID                :    <guid>
                                       Application ID           :
                                       Display Name             :    
                                       Permissions to Keys      :    get, create, delete, list, update, import, backup, restore
                                       Permissions to Secrets   :    all
    
    
    Tags                             :
```

Kui teil on olemasolev võti Vault, saate selle abil Azure'i CLI juurutamiseks lubada.

```cli
> azure login
> azure account set "your account"
> azure config mode arm 
> azure keyvault list
> azure keyvault set-policy --vault-name "your vault name" --enabled-for-deployment true
```

<a id="add-certificate-to-key-vault"></a>
## <a name="add-certificates-to-key-vault"></a>Sertide lisamine klahvi hoidlasse

Autentimise ja krüptimise jälgib klaster ja selle rakenduste turvamiseks kasutatakse teenuse struktuuri serdid. Sertide kasutamise teenuse struktuuri kohta leiate lisateavet teemast [teenuse struktuuri kobar turvalisus stsenaariumid][service-fabric-cluster-security].

### <a name="cluster-and-server-certificate-required"></a>Kobar ja Serveri sert (nõutav) 

See tunnistus on vajalik turvaline klaster ja volitamata juurdepääsu vältimiseks. Pakub kobar Turve on mitu võimalust:
 
 - **Kobar autentimine:** Autendib sõlm sõlme side kobar ühinemise jaoks. Ainult sõlmed, mida saate oma isikut selle serdiga saate liituda klaster.
 - **Autentimine:** Autendib kobar halduse lõpp-punktid halduse klient, et halduse klient teaks, et see räägib reaal klaster. Selle serdi pakub SSL-i HTTPS-i haldamise API ja teenuse struktuuri Exploreri HTTPS.

Nende otstarvet serdi peab vastama järgmistele nõuetele:

 - Serdi peab sisaldama privaatvõti.
 - Serdi peab olema loodud võtme Exchange'i, eksporditav isikliku teabe Exchange'i (pfx) faili.
 - Selle serdi subjekti nimi peab vastama teenuse struktuuri kobar kasutada domeeni. See matchng on vaja soovitud klaster HTTPS-i halduse lõpp-punktid ja teenuse struktuuri Exploreri SSL-i pakkumiseks. Te ei saa hankida SSL-serdi sertimiskeskus (CA): selle `.cloudapp.azure.com` domeeni. Peate hankida klaster kohandatud domeeni nimi. Kui te CA serdi päring, serdi subjekti nimi peab vastama klaster kasutada kohandatud domeeni nime.

### <a name="application-certificates-optional"></a>Rakenduse serdid (valikuline)

Suvalist arvu täiendavate serdid installimist klaster turvalisuse eesmärgil. Enne luua klaster, kaaluge rakenduse turvalisus stsenaariumid, mis nõuavad sert olema installitud sõlmed, näiteks:

 - Krüptimine ja dekrüptimine väärtuste rakenduse konfigureerimine
 - Andmete üle sõlmed krüptimise ajal dispersioonanalüüs 

### <a name="formatting-certificates-for-azure-resource-provider-use"></a>Vormingu Azure ressursikasutuse pakkuja serdid

Privaatvõti failid (pfx) saab lisada ja kasutada otse klahvi Vault. Azure'i ressursi pakkuja nõuab siiski klahvid, mida talletatakse teisiti JSON-vormingus, mis sisaldab pfx nimega base-64-kodeeringuga stringi ja privaatne võti parool. Nendele nõuetele kohandamiseks klahvid paigutatakse JSON-stringi ja seejärel salvestatud *saladusi* klahvi võlvkelder.

Selle protsessi hõlbustamiseks PowerShelli moodul on [saadaval github][service-fabric-rp-helpers]. Mooduli kasutamiseks tehke järgmist

  1. Laadige alla repo kogu sisu kohalikku kataloogi. 
  2. Importida oma aknas PowerShelli mooduli.

  ```powershell
  PS C:\Users\vturecek> Import-Module "C:\users\vturecek\Documents\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1"
  ```
     
Funktsiooni `Invoke-AddCertToKeyVault` käsk PowerShelli moodul automaatselt vormindab serdi privaatvõti JSON string ja lisatud see võti Vault. Selle abil saate lisamine klahvi hoidlasse kobar serti ja mis tahes täiendavaid rakenduse serdid. Korrake seda toimingut iga täiendava sertide soovite installida klaster.

```powershell
 Invoke-AddCertToKeyVault -SubscriptionId <guid> -ResourceGroupName mycluster-keyvault -Location "West US" -VaultName myvault -CertificateName mycert -Password "<password>" -UseExistingCertificate -ExistingPfxFilePath "C:\path\to\mycertkey.pfx"
    
    Switching context to SubscriptionId <guid>
    Ensuring ResourceGroup mycluster-keyvault in West US
    WARNING: The output object type of this cmdlet is going to be modified in a future release.
    Using existing valut myvault in West US
    Reading pfx file from C:\path\to\key.pfx
    Writing secret to myvault in vault myvault
    
    
Name  : CertificateThumbprint
Value : <value>

Name  : SourceVault
Value : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault

Name  : CertificateURL
Value : https://myvault.vault.azure.net:443/secrets/mycert/4d087088df974e869f1c0978cb100e47

```

Eelmise stringid on võti Vault eeltingimused seadistamise teenuse struktuuri kobar ressursihaldur Mall, mis installitakse sõlm autentimine, turbe halduse lõpp-punkti ja autentimise jaoks serdid ja kasutada X.509 serdid täiendav taotlus turbefunktsioonid. Selles etapis peaks nüüd olema järgmised häälestamise Azure:

 - Klahv Vault ressursirühm
   - Võtme Vault.
     - Kobar serveri autentimissert
     - Rakenduse serdid

## <a name="set-up-azure-active-directory-for-client-authentication"></a>Kliendi autentimiseks Azure Active Directory häälestamine

AAD rakendusi, mis on jagatud rakenduste veebipõhine sisselogimise Kasutajaliidese ja rakenduste omakliendi kogemus kasutajate juurdepääsu haldamine võimaldab ettevõtted (tuntud rentnikud). Selles dokumendis me endale rentniku juba loonud. Kui ei, lugedes [hankimine on Azure Active Directory rentnik]alustada[active-directory-howto-tenant].

Teenuse struktuuri kobar pakub mitme kirje osutab selle funktsioonid, sh veebi-põhiste [Teenuste struktuuri Exploreri] [ service-fabric-visualizing-your-cluster] ja [Visual Studio][service-fabric-manage-application-in-visual-studio]. Selle tulemusena saate luua kaks AAD rakenduste klaster juurdepääsu piiramiseks, üks veebirakendus ja ühe kohalikus rakenduses.

Mõned juhised seadistamine AAD teenuse struktuuri kobar lihtsustamiseks oleme loonud Windows PowerShelli skriptide kogum.

>[AZURE.NOTE] Peate tegema järgmised toimingud *enne* loomist klaster nii juhul, kui skriptide oodata kobar nimed ja lõpp-punktid, need tuleks kavandatud väärtused, mitte need, mis on juba loodud.

1. [Funktsiooni skriptide allalaadimiseks] [ sf-aad-ps-script-download] arvutiga.

2. Paremklõpsake zip-fail, nuppu **Atribuudid**ja seejärel märkige ruut **Aktiveeri** ja rakendada.

3. Väljavõte zip-fail.

4. Käivitage `SetupApplications.ps1`, pakub TenantId, ClusterName ja WebApplicationReplyUrl parameetrid. Näiteks:

    ```powershell
    .\SetupApplications.ps1 -TenantId '690ec069-8200-4068-9d01-5aaf188e557a' -ClusterName 'mycluster' -WebApplicationReplyUrl 'https://mycluster.westus.cloudapp.azure.com:19080/Explorer/index.html'
    ```

    Leiate oma **TenantId** PowerShelli käsu "Get-AzureSubscription" ". See kuvab **TenantId** iga tellimuse jaoks.

    **ClusterName** kasutatakse eesliite AAD rakendustes loodud skripti. Vastavad täpselt nii, nagu see on mõeldud ainult lihtsam AAD esemeid vastendamine teenuse struktuuri kobar, mida nad ei kasutata kobar tegelikku nime pole vaja.

    **WebApplicationReplyUrl** on AAD annab kasutajatele pärast sisselogimise protsessi vaikimisi lõpp-punkti. Määrake selle teenuse struktuuri Exploreri lõpp-punkti jaoks klaster, mis vaikesättena on:

    https://&lt;cluster_domain&gt;: 19080/Explorer

    Teil palutakse, logige sisse kontoga, millel on administraatoriõigused AAD rentniku jaoks. Kui te ei tee, skripti tulu loomine veebis ja algsete rakenduste tähistada klaster teenuse struktuuri. Kui vaatate rentniku rakenduste [Azure klassikaline portaali][azure-classic-portal], kuvatakse kaks uued kirjed:

    - *ClusterName*\_kobar
    - *ClusterName*\_klient

    Skripti pildid soovitud Json nõutav Azure ressursihaldur malli klaster loomisel järgmises jaotises nii hoida PowerShelli aknas avatud.

```json
"azureActiveDirectory": {
  "tenantId":"<guid>",
  "clusterApplication":"<guid>",
  "clientApplication":"<guid>"
},
```

## <a name="create-a-service-fabric-cluster-resource-manager-template"></a>Teenuse struktuuri kobar ressursihaldur malli loomine

Selles jaotises kasutatakse eelmise PowerShelli käskude väljundi teenuse struktuuri kobar ressursihaldur malli.

Valimi ressursihaldur Mallid on saadaval [Azure kiire alustada malli Galerii github][azure-quickstart-templates]. Neid malle saab kasutada kobar malli lähtepunktina. 

### <a name="create-the-resource-manager-template"></a>Ressursihaldur malli loomine

Sellest juhendist kasutab [5-sõlm turvaline kobar] [ service-fabric-secure-cluster-5-node-1-nodetype-wad] näide Mall ja malli parameetrid. Laadige alla `azuredeploy.json` ja `azuredeploy.parameters.json` oma arvutisse ja avage mõlemad failid oma lemmik tekstiredaktoris.

### <a name="add-certificates"></a>Lisa serdid

Serdid on lisada kobar ressursihaldur malli viitamine selle klahvi hoidla, mis sisaldab serti võtmed. Soovitatav on, et need väärtused klahvi Vault paigutatakse ressursihaldur parameetrite mallifaili säilitada ressursihaldur malli faili korduvkasutatav ja tasuta väärtuste teatud juurutamine.

#### <a name="add-all-certificates-to-the-vmss-osprofile"></a>Kõik serdid VMSS osProfile lisamine

Iga sert, mida peab olema installitud ja klaster peab olema konfigureeritud osProfile jaotises VMSS ressursi (Microsoft.Compute/virtualMachineScaleSets). See teeb ressursi pakkuja serdi installimiseks VMs. See sisaldab kobar serdi kui ka rakenduse turvalisus serdid, mida plaanite kasutada rakendusi.

```json
{
  "apiVersion": "2016-03-30",
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  ...
  "properties": {
    ...
    "osProfile": {
      ...
      "secrets": [
        {
          "sourceVault": {
            "id": "[parameters('sourceVaultValue')]"
          },
          "vaultCertificates": [
            {
              "certificateStore": "[parameters('clusterCertificateStorevalue')]",
              "certificateUrl": "[parameters('clusterCertificateUrlValue')]"
            },
            {
              "certificateStore": "[parameters('applicationCertificateStorevalue')",
              "certificateUrl": "[parameters('applicationCertificateUrlValue')]"
            },
            ...
          ]
        }
      ]
    }
  }
}
```

#### <a name="configure-service-fabric-cluster-certificate"></a>Teenuse struktuuri kobar-serdi konfigureerimine

Kobar autentimissert tuleb konfigureerida ka teenuse struktuuri kobar koodiressursi (Microsoft.ServiceFabric/clusters) ning teenuse struktuuri jaoks VMSS VMSS ressurss. See võimaldab teenuse struktuuri ressursi pakkujat konfigureerimiseks kasutamiseks kobar autentimiseks ja serveri autentimise halduse lõpp-punktid.

##### <a name="vmss-resource"></a>VMSS ressurss:

```json
{
  "apiVersion": "2016-03-30",
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  ...
  "properties": {
    ...
    "virtualMachineProfile": {
      "extensionProfile": {
        "extensions": [
          {
            "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
            "properties": {
              ...
              "settings": {
                ...
                "certificate": {
                  "thumbprint": "[parameters('clusterCertificateThumbprint')]",
                  "x509StoreName": "[parameters('clusterCertificateStoreValue')]"
                },
                ...
              }
            }
          }
        ]
      }
    }
  }
}
```

##### <a name="service-fabric-resource"></a>Teenuse struktuuri ressurss:

```json
{
  "apiVersion": "2016-03-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  "location": "[parameters('clusterLocation')]",
  "dependsOn": [
    "[concat('Microsoft.Storage/storageAccounts/', variables('supportLogStorageAccountName'))]"
  ],
  "properties": {
    "certificate": {
      "thumbprint": "[parameters('clusterCertificateThumbprint')]",
      "x509StoreName": "[parameters('clusterCertificateStoreValue')]"
    },
    ...
  }
}
```

### <a name="insert-aad-config"></a>AAD config lisamine

Varem loodud AAD konfiguratsiooni saab lisada otse ressursihaldur malli, kuid soovitatav on esmalt parameetrite failiks hoida ressursihaldur malli korduvkasutatava ja tasuta väärtuste teatud juurutamine parameetrite väärtused ekstrakti.

```json
{
  "apiVersion": "2016-03-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  ...
  "properties": {
    "certificate": {
      "thumbprint": "[parameters('clusterCertificateThumbprint')]",
      "x509StoreName": "[parameters('clusterCertificateStorevalue')]"
    },
    ...
    "azureActiveDirectory": {
      "tenantId": "[parameters('aadTenantId')]",
      "clusterApplication": "[parameters('aadClusterApplicationId')]",
      "clientApplication": "[parameters('aadClientApplicationId')]"
    },
    ...
  }
}
```

### < on "konfigureerimine rühmaga" ></a>konfigureerimine ressursihaldur malli parameetrid

Lõpuks asustada parameetrite fail väljundi väärtustega klahvi hoidla ja AAD PowerShelli käskude abil.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": { 
        ...
        "clusterCertificateStoreValue": {
            "value": "My"
        },
        "clusterCertificateThumbprint": {
            "value": "<thumbprint>"
        },
        "clusterCertificateUrlValue": {
            "value": "https://myvault.vault.azure.net:443/secrets/myclustercert/4d087088df974e869f1c0978cb100e47"
        },
        "applicationCertificateStorevalue": {
            "value": "My"
        },
        "applicationCertificateUrlValue": {
            "value": "https://myvault.vault.azure.net:443/secrets/myapplicationcert/2e035058ae274f869c4d0348ca100f08"
        },
        "sourceVaultvalue": {
            "value": "/subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault"
        },
        "aadTenantId": {
            "value": "<guid>"
        },
        "aadClusterApplicationId": {
            "value": "<guid>"
        },
        "aadClientApplicationId": {
            "value": "<guid>"
        },
        ...
    }
}
```
Selles etapis peaks nüüd on järgmine:

 - Klahv Vault ressursirühm
    - Võtme Vault.
    - Kobar serveri autentimissert
    - Andmete šifreerimine sert
 - Azure Active Directory rentniku 
    - Veebipõhise haldus ja teenuse struktuuri Exploreri AAD taotlemine
    - AAD omakliendi juhtimise taotlemine
    - Kasutajad, kellel on määratud rolle 
 - Teenuse struktuuri kobar ressursihaldur Mall
    - Serdid konfigureeritud klahv Vault kaudu
    - Azure Active Directory konfigureeritud 

Järgmine diagramm näitab, kus klahvi hoidla ja AAD konfiguratsiooni mahu ressursihaldur malli.

![Ressursihaldur sõltuvus kaart][cluster-security-arm-dependency-map]

## <a name="create-the-cluster"></a>Klaster loomine

Nüüd olete valmis looma abil [ARM juurutamise]klaster[resource-group-template-deploy].

#### <a name="test-it"></a>Katsetage seda

Testimiseks ressursihaldur malli faili parameetrite abil järgmist PowerShelli käsu kasutamine

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

#### <a name="deploy-it"></a>See juurutamine

Kui ressursihaldur malli test, PowerShelli käsk abil juurutada ressursihaldur malli parameetrid faili abil:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

<a name="assign-roles"></a>
## <a name="assign-users-to-roles"></a>Kasutajate rollid määramine

Kui olete loonud rakendusi, mis kujutab klaster, peate määrama kasutajate teenuse struktuuri ei toeta rollidele: kirjutuskaitstud ja haldus. Saate seda [Azure klassikaline portaali][azure-classic-portal].

1. Liikuge oma rentniku ja valige rakendused.
2. Valige veebirakenduse, mille nimi on umbes `myTestCluster_Cluster`.
3. Klõpsake vahekaarti kasutajad.
4. Valige kasutaja ja klõpsake nuppu **Määra** ekraani allservas nuppu.

    ![Määrata kasutajatele rollid nupule][assign-users-to-roles-button]

5. Valige roll, kasutajale määrata.

    ![Kasutajate rollid määramine][assign-users-to-roles-dialog]

>[AZURE.NOTE] Rollide teenuse struktuuri kohta leiate lisateavet teemast [Rollipõhine juurdepääsu reguleerimine teenuse struktuuri klientidele](service-fabric-cluster-security-roles.md).

 <a name="secure-linux-cluster"></a> 
##  <a name="create-secure-clusters-on-linux"></a>Luua turvalist kogumite Linux

Protsessi hõlbustamiseks helper script on esitatud [allpool](http://github.com/ChackDan/Service-Fabric/tree/master/Scripts/CertUpload4Linux). Kasutamise helper skripti, eeldatakse, et teil on juba installitud Azure'i CLI ja see on teie tee. Veenduge, et script on õigused, käitades käivitada `chmod +x cert_helper.py` pärast allalaadimise kaudu. Esimese sammuna tuleb abil CLI koos oma Azure'i kontosse sisse logida selle `azure login` käsk. Pärast Azure oma kontole logimine kasutada soovitud helper CA allkirjastatud sert nagu näha järgmine käsk:

```sh
./cert_helper.py [-h] CERT_TYPE [-ifile INPUT_CERT_FILE] [-sub SUBSCRIPTION_ID] [-rgname RESOURCE_GROUP_NAME] [-kv KEY_VAULT_NAME] [-sname CERTIFICATE_NAME] [-l LOCATION] [-p PASSWORD]

The -ifile parameter can take a .pfx or a .pem file as input, with the certificate type (pfx or pem, or ss if it is a self-signed cert).
The parameter -h prints out the help text.
```

See käsk tagastab järgmised kolm stringid, kui: 

1. SourceVaultID, mis on uut KeyVault ResourceGroup seda teie jaoks loodud ID-d. 

2. CertificateUrl, juurdepääsuks sert.

3. CertificateThumbprint, mida kasutatakse autentimist.


Järgmises näites kirjeldatakse, kuidas kasutada käsku.

```sh
./cert_helper.py pfx -sub "fffffff-ffff-ffff-ffff-ffffffffffff"  -rgname "mykvrg" -kv "mykevname" -ifile "/home/test/cert.pfx" -sname "mycert" -l "East US" -p "pfxtest"
```
Eelmise käsu sisaldab kolme stringid järgmiselt:

```sh
SourceVault: /subscriptions/fffffff-ffff-ffff-ffff-ffffffffffff/resourceGroups/mykvrg/providers/Microsoft.KeyVault/vaults/mykvname
CertificateUrl: https://myvault.vault.azure.net/secrets/mycert/00000000000000000000000000000000
CertificateThumbprint: 0xfffffffffffffffffffffffffffffffffffffffff
```

 Selle serdi subjekti nimi peab vastama teenuse struktuuri kobar kasutada domeeni. See on nõutav on kobar HTTPS-i halduse lõpp-punktid ja teenuse struktuuri Exploreri SSL-i ette. Te ei saa hankida SSL-serdi sertimiskeskus (CA): selle `.cloudapp.azure.com` domeeni. Peate hankida klaster kohandatud domeeni nimi. Kui CA serdi päring serdi subjekti nimi peab vastama klaster kasutada kohandatud domeeni nime.

Need on vaja luua turvaline teenus struktuuri kobar (ilma AAD), nagu on kirjeldatud aadressil [konfigureerimine ressursihaldur malli parameetrid](#configure-arm)kirjeid. Saate luua ühenduse turvaline kobar juhiseid [autentimisel klientrakenduses klaster juurdepääsu](service-fabric-connect-to-secure-cluster.md)kaudu. Linux Preview 's kogumite ei toeta AAD autentimine. Saate määrata administraator ja kliendi rollid, nagu on kirjeldatud jaotises [määrata kasutajatele rollid](#assign-roles). Administraator ja kliendi rollid Linux eelvaade kobar määramisel peate serdi thumbprints ette autentimine (erinevalt teema nimi, kuna pole ahelas valideerimine või tühistamise sooritatakse seda eelvaateversiooni).


Kui soovite kasutada testimiseks iseallkirjastatud serdi, võite sama skripti iseallkirjastatud serdi loomine ja üleslaadimine KeyVault, andes lipu `ss` selle asemel et serdi tee ja serdi nimi. Näiteks vt jaoks luua ja üles laadida iseallkirjastatud serdi järgmine käsk:

```sh
./cert_helper.py ss -rgname "mykvrg" -sub "fffffff-ffff-ffff-ffff-ffffffffffff" -kv "mykevname"   -sname "mycert" -l "East US" -p "selftest" -subj "mytest.eastus.cloudapp.net" 
```

See käsk tagastab sama kolme stringid, SourceVault, CertificateUrl ja CertificateThumbprint, mis saab luua turvalist Linux kobar koos asukoht, kuhu oli paigutatud iseallkirjastatud sert. Peate ühenduse klaster iseallkirjastatud sert.  Saate luua ühenduse turvaline kobar juhiseid [autentimisel klientrakenduses klaster juurdepääsu](service-fabric-connect-to-secure-cluster.md)kaudu. Selle serdi subjekti nimi peab vastama teenuse struktuuri kobar kasutada domeeni. See on nõutav on kobar HTTPS-i halduse lõpp-punktid ja teenuse struktuuri Exploreri SSL-i ette. Te ei saa hankida SSL-serdi sertimiskeskus (CA): selle `.cloudapp.azure.com` domeeni. Peate hankida klaster kohandatud domeeni nimi. Kui CA serdi päring serdi subjekti nimi peab vastama klaster kasutada kohandatud domeeni nime.

Esitatud helper skripti parameetrid saate täita portaali, nagu on kirjeldatud jaotises [klaster Azure portaali loomine](service-fabric-cluster-creation-via-portal.md#create-cluster-portal).

## <a name="next-steps"></a>Järgmised sammud

Selles etapis teil turvaline kobar Azure Active Directory andev halduse autentimisega. Järgmise, [ühenduse klaster](service-fabric-connect-to-secure-cluster.md) ja saate teada, kuidas [hallata rakenduse saladusi](service-fabric-application-secret-management.md).

## <a name="troubleshoot-setting-up-azure-active-directory-for-client-authentication"></a>Kliendi autentimiseks Azure Active Directory häälestamise tõrkeotsing

Kui tekib probleeme kliendi autentimiseks Azure Active Directory häälestamise ajal, vaadake järgmist suggestings võimalike lahenduste.

### <a name="service-fabric-explorer-prompts-for-selecting-certificate"></a>Teenuse struktuuri Exploreri domeenikonto serdi valimine

#### <a name="problem"></a>Probleem

Pärast sisselogimist edukalt lehel AAD sisselogimise teenuse struktuuri Exploreris, brauseri avalehele liikumine tagastab, kuid palub dialoogis valida sert.

![Dialoogiboks SFX valige sert][sfx-select-certificate-dialog]

#### <a name="reason"></a>Põhjus

Kasutaja ei määratud rolli AAD kobar rakenduses. Seega AAD autentimine nurjub teenuse struktuuri kobar. Teenuse struktuuri Exploreri langeb tagasi serdi autentimist.

#### <a name="solution"></a>Lahendus

Järgige AAD häälestamise ja määrata kasutaja rollid. Lisaks "Kasutaja ülesande nõutav soovite ACCESSI rakenduse" on soovitatav sisse lülitada nimega `SetupApplications.ps1` ei.

### <a name="connect-with-powershell-fails-with-error-the-specified-credentials-are-invalid"></a>Kasutajaga PowerShelli nurjub tõrkega: määratud mandaadid ei sobi

#### <a name="problem"></a>Probleem

Millal kasutada PowerShelli abil "AzureActiveDirectory" Turvarežiim kobar ühenduse, pärast sisselogimist edukalt lehel AAD sisselogimine nurjub tõrkega: määratud mandaadid ei sobi näidatud.

#### <a name="solution"></a>Lahendus

Sama mis eelmine.

### <a name="service-fabric-explorer-signing-in-return-failure-aadsts50011"></a>Teenuse struktuuri Exploreri tagasi sisselogimise nurjumise: AADSTS50011

#### <a name="problem"></a>Probleem

Pärast sisselogimist AAD sisselogimislehel teenuse struktuuri Exploreris, lehe tagastab sisselogimise nurjumise - AADSTS50011: vastuse aadressi &lt;URL-i&gt; ei vasta, vasta aadressid rakendusele konfigureeritud: &lt;guid&gt;. 

![SFX vastuseaadressi ei vasta][sfx-reply-address-not-match]

#### <a name="reason"></a>Põhjus

Teenuse struktuuri Exploreri katsete autentimiseks AAD tähistav cluster(web) rakenduse osana taotluse pakub ümbersuunamise saatja URL-i. Aga seda ei kuvata loendis AAD application "Vasta URL".

#### <a name="solution"></a>Lahendus

URL-i teenuse struktuuri Exploreri lisamine "Vasta URL" konfigureerimine vahekaardil cluster(web) rakenduse või asendamine ühte üksusi loendis. Salvestage.

![Veebirakenduse vasta url][web-application-reply-url]

### <a name="can-i-reuse-the-same-aad-tenant-for-multiple-clusters"></a>Saate mitu kogumite sama AAD rentniku saab uuesti kasutada?

#### <a name="answer"></a>Answer (vastus)

Jah. Kuid pidage meeles, et lisada teenuse URL-i struktuuri Exploreri cluster(web) rakenduse muidu teenuse struktuuri Explorer ei tööta.

### <a name="why-do-i-still-need-server-certificate-while-aad-enabled"></a>Miks endiselt vaja serveri serdi ajal AAD lubatud?

#### <a name="answer"></a>Answer (vastus)

FabricClient ja FabricGateway teha vastastikune autentimine. Korral AAD autentimine, pakub AAD integreerimine kliendi identiteedi server ja serveri serdi kasutatakse serveri identiteedi kinnitamiseks. Lisateabe saamiseks serdi tööpõhimõte teenuse struktuuri, märkige ruut [X.509 serdid ja teenuse struktuuri][x509-certificates-and-service-fabric]

<!-- Links -->
[azure-powershell]:https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[key-vault-get-started]:../key-vault/key-vault-get-started.md
[aad-graph-api-docs]:https://msdn.microsoft.com/library/azure/ad/graph/api/api-catalog
[azure-classic-portal]: https://manage.windowsazure.com
[service-fabric-rp-helpers]: https://github.com/ChackDan/Service-Fabric/tree/master/Scripts/ServiceFabricRPHelpers
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[active-directory-howto-tenant]: ../active-directory/active-directory-howto-tenant.md
[service-fabric-visualizing-your-cluster]: service-fabric-visualizing-your-cluster.md
[service-fabric-manage-application-in-visual-studio]: service-fabric-manage-application-in-visual-studio.md
[sf-aad-ps-script-download]:http://servicefabricsdkstorage.blob.core.windows.net/publicrelease/MicrosoftAzureServiceFabric-AADHelpers.zip
[azure-quickstart-templates]: https://github.com/Azure/azure-quickstart-templates
[service-fabric-secure-cluster-5-node-1-nodetype-wad]: https://github.com/Azure/azure-quickstart-templates/blob/master/service-fabric-secure-cluster-5-node-1-nodetype-wad/
[resource-group-template-deploy]: https://azure.microsoft.com/documentation/articles/resource-group-template-deploy/
[x509-certificates-and-service-fabric]: service-fabric-cluster-security.md#x509-certificates-and-service-fabric

<!-- Images -->
[cluster-security-arm-dependency-map]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-arm-dependency-map.png
[cluster-security-cert-installation]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-cert-installation.png
[assign-users-to-roles-button]: ./media/service-fabric-cluster-creation-via-arm/assign-users-to-roles-button.png
[assign-users-to-roles-dialog]: ./media/service-fabric-cluster-creation-via-arm/assign-users-to-roles.png
[sfx-select-certificate-dialog]: ./media/service-fabric-cluster-creation-via-arm/sfx-select-certificate-dialog.png
[sfx-reply-address-not-match]: ./media/service-fabric-cluster-creation-via-arm/sfx-reply-address-not-match.png
[web-application-reply-url]: ./media/service-fabric-cluster-creation-via-arm/web-application-reply-url.png