<properties
   pageTitle="Ühenduse loomine turvaline privaatne kobar | Microsoft Azure'i"
   description="Selles artiklis kirjeldatakse, kuidas turvaline side eraldi või privaatne kobar vahel kliendid ning klaster."
   services="service-fabric"
   documentationCenter=".net"
   authors="dsk-2015"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/08/2016"
   ms.author="dkshir"/>

# <a name="secure-a-standalone-cluster-on-windows-using-x509-certificates"></a>Turvaline autonoomse kobar X.509 sertide kasutamine Windowsis

Selles artiklis kirjeldatakse kuidas tagada suhtlemine erinevate sõlmed klaster autonoomse Windows, samuti, kuidas kliendid selle kobar ühenduse abil X.509 autentida sertifikaatide. See tagab, et ainult autoriseeritud kasutajad saavad juurdepääsuks klaster, juurutatud rakendused ja haldamisega seotud toiminguid teha.  Serdi turvalisus peaks olema lubatud klaster klaster loomisel.  

Kobar turvalisus, nt sõlm sõlme turvalisus, kliendi sõlme turvalisus ja Rollipõhine juurdepääsu reguleerimine kohta leiate lisateavet teemast [kobar turvalisus stsenaariumid](service-fabric-cluster-security.md).

## <a name="which-certificates-will-you-need"></a>Millised serdid on vaja?

Alustada, [laadige autonoomse kobar](service-fabric-cluster-creation-for-windows-server.md#downloadpackage) üks sõlmed klaster sisse. Allalaaditud pakett, leiate **ClusterConfig.X509.MultiMachine.json** faili. Avage fail ja kontrollige jaotisest **Turvalisus** jaotises **Atribuudid** .

    "security": {
        "metadata": "The Credential type X509 indicates this is cluster is secured using X509 Certificates. The thumbprint format is - d5 ec 42 3b 79 cb e5 07 fd 83 59 3c 56 b9 d5 31 24 25 42 64.",
        "ClusterCredentialType": "X509",
        "ServerCredentialType": "X509",
        "CertificateInformation": {
            "ClusterCertificate": {
                "Thumbprint": "[Thumbprint]",
                "ThumbprintSecondary": "[Thumbprint]",
                "X509StoreName": "My"
            },
            "ServerCertificate": {
                "Thumbprint": "[Thumbprint]",
                "ThumbprintSecondary": "[Thumbprint]",
                "X509StoreName": "My"
            },
            "ClientCertificateThumbprints": [{
                "CertificateThumbprint": "[Thumbprint]",
                "IsAdmin": false
            }, {
                "CertificateThumbprint": "[Thumbprint]",
                "IsAdmin": true
            }],
            "ClientCertificateCommonNames": [{
                "CertificateCommonName": "[CertificateCommonName]",
                "CertificateIssuerThumbprint" : "[Thumbprint]",
                "IsAdmin": true
            }]
            "HttpApplicationGatewayCertificate":{
                "Thumbprint": "[Thumbprint]",
                "X509StoreName": "My"
            }
        }
    }

Selles jaotises kirjeldatakse tagamiseks klaster autonoomse Windowsi peate serdid. Luba serdi põhineva turvalisuse määramine *X509* **ClusterCredentialType** ja **ServerCredentialType** väärtused.

>[AZURE.NOTE] Mõne [sõrmejälje](https://en.wikipedia.org/wiki/Public_key_fingerprint) on esmane identiteedi sert. [Kuidas hankida sõrmejälje sertifikaadi](https://msdn.microsoft.com/library/ms734695.aspx) teada saada sõrmejälje loote serdid lugeda.

Järgmises tabelis on loetletud serdid, mida on vaja oma kobar häälestamine:

|**CertificateInformation säte**|**Kirjeldus**|
|-----------------------|--------------------------|
|ClusterCertificate|See tunnistus on vajalik turvaline side sõlme klaster vahel. Saate kaks erinevat sertifikaati, esmane ja teisese uuele versioonile. Määrata **sõrmejälje** jaotis ja mis teisese **ThumbprintSecondary** muutujad sõrmejälje esmane sert.|
|ServerCertificate|Selle serdi on esitatud kliendile, kui püüab selle kobar ühendamiseks. Mugavuse, saate kasutada sama serdi *ClusterCertificate* ja *ServerCertificate*. Saate kaks eri server serdid, esmane ja teisese uuele versioonile. Määrata **sõrmejälje** jaotis ja mis teisese **ThumbprintSecondary** muutujad sõrmejälje esmane sert. |
|ClientCertificateThumbprints|See on kogumi serdid, mida soovite installida autenditud kliendid. Saate määrata mitmesuguseid eri kliendi sertide arvutites, mis te soovite lubada juurdepääsu klaster installitud. Saate seada sõrmejälje iga sertifikaat **CertificateThumbprint** muutujana. Kui seate **IsAdmin** *True*, siis kliendi installitud selle serdiga saate teha administraatori tegelemise klaster. Kui **IsAdmin** on *false*, selle serdiga kliendi teha ainult kasutajate juurdepääsu õiguste tavaliselt kirjutuskaitstud lubatud toimingud. Rollide kohta lisateabe saamiseks lugege [rollipõhise juurdepääsu reguleerimine (RBAC)](service-fabric-cluster-security.md/#role-based-access-control-rbac)  |
|ClientCertificateCommonNames|Saate seada **CertificateCommonName**levinud nime esimese kliendi sert. **CertificateIssuerThumbprint** on sõrmejälje selle serdi väljaandja jaoks. Lugege [töötamine serdid](https://msdn.microsoft.com/library/ms731899.aspx) levinud nimed ja väljaandja kohta rohkem teada.|
|HttpApplicationGatewayCertificate|See on valikuline sert, mida saab määratud, kui soovite secure Http rakenduse lüüsi. Veenduge, et reverseProxyEndpointPort on seatud nodeTypes, kui kasutate seda serti.|

Siin on näide kobar konfiguratsioon, kus kobar, serveri ja kliendi serdid on antud.

 ```
 {
    "name": "SampleCluster",
    "clusterConfigurationVersion": "1.0.0",
    "apiVersion": "2016-09-26",
    "nodes": [{
        "nodeName": "vm0",
        "metadata": "Replace the localhost below with valid IP address or FQDN",
        "iPAddress": "10.7.0.5",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r0",
        "upgradeDomain": "UD0"
    }, {
      "nodeName": "vm1",
            "metadata": "Replace the localhost with valid IP address or FQDN",
        "iPAddress": "10.7.0.4",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r1",
        "upgradeDomain": "UD1"
    }, {
        "nodeName": "vm2",
      "iPAddress": "10.7.0.6",
            "metadata": "Replace the localhost with valid IP address or FQDN",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r2",
        "upgradeDomain": "UD2"
    }],
    "properties": {
        "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "FileShare",
        "IsEncrypted": "false",
        "connectionstring": "c:\\ProgramData\\SF\\DiagnosticsStore"
        }
        "security": {
            "metadata": "The Credential type X509 indicates this is cluster is secured using X509 Certificates. The thumbprint format is - d5 ec 42 3b 79 cb e5 07 fd 83 59 3c 56 b9 d5 31 24 25 42 64.",
            "ClusterCredentialType": "X509",
            "ServerCredentialType": "X509",
            "CertificateInformation": {
                "ClusterCertificate": {
                    "Thumbprint": "a8 13 67 58 f4 ab 89 62 af 2b f3 f2 79 21 be 1d f6 7f 43 26",
                    "X509StoreName": "My"
                },
                "ServerCertificate": {
                    "Thumbprint": "a8 13 67 58 f4 ab 89 62 af 2b f3 f2 79 21 be 1d f6 7f 43 26",
                    "X509StoreName": "My"
                },
                "ClientCertificateThumbprints": [{
                    "CertificateThumbprint": "c4 c18 8e aa a8 58 77 98 65 f8 61 4a 0d da 4c 13 c5 a1 37 6e",
                    "IsAdmin": false
                }, {
                    "CertificateThumbprint": "71 de 04 46 7c 9e d0 54 4d 02 10 98 bc d4 4c 71 e1 83 41 4e",
                    "IsAdmin": true
                }]
            }
        },
        "reliabilityLevel": "Bronze",
        "nodeTypes": [{
            "name": "NodeType0",
            "clientConnectionEndpointPort": "19000",
            "clusterConnectionEndpoint": "19001",
            "httpGatewayEndpointPort": "19080",
            "applicationPorts": {
                "startPort": "20001",
                "endPort": "20031"
            },
            "ephemeralPorts": {
                "startPort": "20032",
                "endPort": "20062"
            },
            "isPrimary": true
        }
         ],
        "fabricSettings": [{
            "name": "Setup",
            "parameters": [{
                "name": "FabricDataRoot",
                "value": "C:\\ProgramData\\SF"
            }, {
                "name": "FabricLogRoot",
                "value": "C:\\ProgramData\\SF\\Log"
            }]
        }]
    }
}
 ```

## <a name="aquire-the-x509-certificates"></a>Omandada X.509 serdid
Turvaline side klaster sees, et peate esmalt oma kobar sõlmed X.509 saada. Lisaks piirata volitatud masinad/kasutajatele selle kobar-ühendus, peate hankida ja installida soovitud klientarvutite serdid.

Kogumite, töötavad tootmise töökoormus, peaksite kasutama [Serdi sertimiskeskuse (CA)](https://en.wikipedia.org/wiki/Certificate_authority) väljastatud allkirjastatud klaster turvamiseks x.509 vastav sert. Need serdid saamise kohta lisateabe saamiseks minge [kohta: saamiseks serdi](http://msdn.microsoft.com/library/aa702761.aspx).

Kogumite, mida katsetamiseks kasutada, saate kasutada iseallkirjastatud sert.

## <a name="optional-create-a-self-signed-certificate"></a>Valikuline: Iseallkirjastatud serdi loomine
Üks võimalus luua iseallkirjastatud cert, mis õigesti turvatud on kasutada *CertSetup.ps1* skripti kataloogis *C:\Program Files\Microsoft SDKs\Service Fabric\ClusterSetup\Secure*teenuse struktuuri SDK kausta. Seda faili redigeerida ja selle abil saate serdi loomine sobiva nimega.

Nüüd eksportida serdi PFX parooliga kaitstud faili. Esmalt peate saamiseks serdi sõrmejälje. Käivitage rakendus certmgr.exe. Liikuge kausta, **Kohaliku Computer\Personal** ja otsige just loodud sert. Topeltklõpsake sert, avage see, valige vahekaart *üksikasjad* ja liikuge kerides jaotisse *sõrmejälje* välja. Kopeerige sõrmejälje väärtus üheks PowerShelli käsu all, eemaldades tühikuid.  Sobivust turvalise parooli kaitse see ja käivitage selle PowerShelli *$pswd* väärtuse muutmiseks järgmist.

```   
$pswd = ConvertTo-SecureString -String "1234" -Force –AsPlainText
Get-ChildItem -Path cert:\localMachine\my\<Thumbprint> | Export-PfxCertificate -FilePath C:\mypfx.pfx -Password $pswd
```

Arvutisse installitud serdi üksikasjade kuvamiseks käivitage PowerShelli järgmine käsk:

```
$cert = Get-Item Cert:\LocalMachine\My\<Thumbprint>
Write-Host $cert.ToString($true)
```

Teise võimalusena kui teil on Azure tellimuse, järgige jaotises [Lisa serdid klahvi Vault](service-fabric-cluster-creation-via-arm.md#add-certificate-to-key-vault).

## <a name="install-the-certificates"></a>Installige need
Kui teil on tähtpäev, need, saate need installida kobar sõlmed. Oma sõlmed peab olema uusim Windows PowerShelli 3.x installitud neid. Peate korrake neid juhiseid iga sõlme kobar- ja serveri serdid ja mis tahes teisene serdid.

1. Kopeerige sõlme pfx-failid.
2. Avage PowerShelli aken administraatorina ja sisestage järgmised käsud. Asendage *$pswd* parool, mida kasutasite selle serdi loomiseks. Asendage *$PfxFilePath* pfx, kopeeritud selle sõlme täielik tee.

    ```
    $pswd = "1234"
    $PfcFilePath ="C:\mypfx.pfx"
    Import-PfxCertificate -Exportable -CertStoreLocation Cert:\LocalMachine\My -FilePath $PfxFilePath -Password (ConvertTo-SecureString -String $pswd -AsPlainText -Force)
    ```

3. Järgmiseks peate määrama juurdepääsu reguleerimine selle serdi kohta, et teenuse struktuuri protsessi, mis kestab jaotises võrku teenusele konto, saate seda kasutada järgmist skripti käivitades. Sisestage sõrmejälje serti ja teenusekonto "NETWORK SERVICE". Saate kontrollida, kas ACL serdi kohta on õiged certmgr.exe tööriista abil ja vaadates haldamine privaatvõtmete cert.

    ```
    param
    (
        [Parameter(Position=1, Mandatory=$true)]
        [ValidateNotNullOrEmpty()]
        [string]$pfxThumbPrint,

        [Parameter(Position=2, Mandatory=$true)]
        [ValidateNotNullOrEmpty()]
        [string]$serviceAccount
        )

        $cert = Get-ChildItem -Path cert:\LocalMachine\My | Where-Object -FilterScript { $PSItem.ThumbPrint -eq $pfxThumbPrint; };

        # Specify the user, the permissions and the permission type
        $permission = "$($serviceAccount)","FullControl","Allow"
        $accessRule = New-Object -TypeName System.Security.AccessControl.FileSystemAccessRule -ArgumentList $permission;

        # Location of the machine related keys
        $keyPath = $env:ProgramData + "\Microsoft\Crypto\RSA\MachineKeys\";
        $keyName = $cert.PrivateKey.CspKeyContainerInfo.UniqueKeyContainerName;
        $keyFullPath = $keyPath + $keyName;

        # Get the current acl of the private key
        $acl = (Get-Item $keyFullPath).GetAccessControl('Access')

        # Add the new ace to the acl of the private key
        $acl.SetAccessRule($accessRule);

        # Write back the new acl
        Set-Acl -Path $keyFullPath -AclObject $acl -ErrorAction Stop

        #Observe the access rights currently assigned to this certificate.
        get-acl $keyFullPath| fl
        ```

4. Repeat the steps above for each server certificate. You can also use these steps to install the client certificates on the machines that you want to allow access to the cluster.

## Create the secure cluster
After configuring the **security** section of the **ClusterConfig.X509.MultiMachine.json** file, you can proceed to [Create your cluster](service-fabric-cluster-creation-for-windows-server.md#createcluster) section to configure the nodes and create the standalone cluster. Remember to use the **ClusterConfig.X509.MultiMachine.json** file while creating the cluster. For example, your command might look like the following:

```
.\CreateServiceFabricCluster.ps1 - ClusterConfigFilePath.\ClusterConfig.X509.MultiMachine.json - MicrosoftServiceFabricCabFilePath.\MicrosoftAzureServiceFabric.cab - AcceptEULA $true
```

Once you have the secure standalone Windows cluster successfully running, and have setup the authenticated clients to connect to it, follow the section [Connect to a secure cluster using PowerShell](service-fabric-connect-to-secure-cluster.md#connectsecurecluster) to connect to it. For example:

```
Ühenduse loomine ServiceFabricCluster - ConnectionEndpoint 10.7.0.4:19000 - KeepAliveIntervalInSec 10 - X509Credential - ServerCertThumbprint 057b9544a6f2733e0c8d3a60013a58948213f551 - FindType FindByThumbprint - FindValue 057b9544a6f2733e0c8d3a60013a58948213f551 - StoreLocation CurrentUser - väli StoreName minu
```

If you are logged on to one of the machines in the cluster, since this already has the certificate installed locally you can simply run the Powershell command to connect to the cluster and show a list of nodes:

```
Ühenduse loomine ServiceFabricCluster Get-ServiceFabricNode
```
To remove the cluster call the following command:

```
.\RemoveServiceFabricCluster.ps1 - ClusterConfigFilePath.\ClusterConfig.X509.MultiMachine.json - MicrosoftServiceFabricCabFilePath.\MicrosoftAzureServiceFabric.cab
```
