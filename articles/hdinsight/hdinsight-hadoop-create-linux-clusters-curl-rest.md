<properties
    pageTitle="Luua Hadoopi, HBase või torm kogumite Linux Hdinsightiga cURL ja Azure REST API abil | Microsoft Azure'i"
    description="Saate teada, kuidas luua Linux-põhine Hdinsightiga kogumite cURL, Azure'i ressursihaldur Mallid ja Azure REST API kasutamine. Saate määrata kobar tüüp (Hadoopi, HBase või Storm) või skriptide abil saate installida kohandatud komponendid."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="10/11/2016"
    ms.author="larryfr"/>

#<a name="create-linux-based-clusters-in-hdinsight-using-curl-and-the-azure-rest-api"></a>Luua Linux-põhine kogumite Hdinsightiga cURL ja Azure REST API abil

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Azure'i REST API võimaldab teenuseid Azure'i platvormi, sealhulgas uusi ressursse, nt Linuxi-põhiste Hdinsightiga kogumite loomine haldamise toiminguid. Selles dokumendis saate teada, kuidas Azure'i ressursihaldur mallide Hdinsightiga kobar- ja seotud salvestusruumi konfigureerimine ja seejärel cURL abil saate juurutada Azure REST API luua uus Hdinsightiga klaster malli loomiseks.

> [AZURE.IMPORTANT] Juhised selle dokumendi kasutamiseks on Hdinsightiga kobar vaikearvu töötaja sõlmed (4). Kui plaanite rohkem kui 32 töötaja sõlmed kobar loomisel või skaala klaster pärast loomist, siis peate valima pea sõlme suurus koos vähemalt 8 14GB RAM-i ja -vormid.
>
> Sõlm suurused ja seotud kulude kohta leiate lisateavet teemast [Hdinsightiga hinnad](https://azure.microsoft.com/pricing/details/hdinsight/).

##<a name="prerequisites"></a>Eeltingimused

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


- **An Azure'i tellimus**. Leiate [Azure'i saada tasuta prooviversioon](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- __Azure'i CLI__. Azure'i CLI saab luua teenuse põhisumma, mida kasutatakse luua autentimine märkide taotluste Azure'i REST API.

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

- __cURL__. Selle kasuliku kaudu oma pakettide süsteem või saab alla laadida [http://curl.haxx.se/](http://curl.haxx.se/).

    > [AZURE.NOTE] Kui kasutate PowerShelli käskude käivitamiseks selles dokumendis, peate eemaldama selle `curl` pseudonüümi, mida luuakse vaikimisi. Selle pseudonüümi kasutab Invoke-WebRequest, PowerShelli cmdlet-käsu, mitte cURL kasutamisel on `curl` käsu PowerShelli käsuviibas ja tulemuseks tõrgete paljude selles dokumendis kasutatud käsud.
    > 
    > Eemaldada see alias (pseudonüüm), kasutage järgmist PowerShelli käsuviiba.
    >
    > `Remove-item alias:curl`
    >
    > Kui pseudonüüm on eemaldatud, peaks saama kasutada cURL, mis on teie arvutisse installitud Office'i versiooni.

### <a name="access-control-requirements"></a>Accessi kontrolli nõuded

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

##<a name="create-a-template"></a>Malli loomine

Azure'i ressursihalduse Mallid on JSON dokumendid, mis kirjeldavad __Ressursirühma__ ja seda kõik ressursid (nt Hdinsightiga.) Selle malli põhjal lähenemine võimaldab määratleda ressursse, mis te vajate Hdinsightile üks Mall ja __juurutuste__ rühma muudatused rakenduvad kogu muutuste rühma haldamiseks.

Mallide pakutakse tavaliselt kaks osa; malli ja parameetrite faili, mida te asustada väärtused teatud konfiguratsioonist. Näide, kobar nimi, administraatori nimi ja parool. Otse kasutades REST API-ga, peate need ühte faili ühendada. Selle dokumendi JSON vorming on:

    {
        "properties": {
            "template": {
                contents of template file
            },
            "mode": "deploymentmode",
            "Parameters": {
                contents of the parameters element from parameters file
            }
        }
    }

Näiteks järgmine on ühinemise Mall ja parameetrite failid [https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password](https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password), mis loob Linux-põhine kobar, secure SSH kasutajakonto parooli abil.

    {
        "properties": {
            "template": {
                "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                "contentVersion": "1.0.0.0",
                "parameters": {
                    "location": {
                        "type": "string",
                        "allowedValues": ["Central US",
                        "East Asia",
                        "East US",
                        "Japan East",
                        "Japan West",
                        "North Europe",
                        "South Central US",
                        "Southeast Asia",
                        "West Europe",
                        "West US"],
                        "metadata": {
                            "description": "The location where all azure resources will be deployed."
                        }
                    },
                    "clusterType": {
                        "type": "string",
                        "allowedValues": ["hadoop",
                        "hbase",
                        "storm",
                        "spark"],
                        "metadata": {
                            "description": "The type of the HDInsight cluster to create."
                        }
                    },
                    "clusterName": {
                        "type": "string",
                        "metadata": {
                            "description": "The name of the HDInsight cluster to create."
                        }
                    },
                    "clusterLoginUserName": {
                        "type": "string",
                        "metadata": {
                            "description": "These credentials can be used to submit jobs to the cluster and to log into cluster dashboards."
                        }
                    },
                    "clusterLoginPassword": {
                        "type": "securestring",
                        "metadata": {
                            "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
                        }
                    },
                    "sshUserName": {
                        "type": "string",
                        "metadata": {
                            "description": "These credentials can be used to remotely access the cluster."
                        }
                    },
                    "sshPassword": {
                        "type": "securestring",
                        "metadata": {
                            "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
                        }
                    },
                    "clusterStorageAccountName": {
                        "type": "string",
                        "metadata": {
                            "description": "The name of the storage account to be created and be used as the cluster's storage."
                        }
                    },
                    "clusterWorkerNodeCount": {
                        "type": "int",
                        "defaultValue": 4,
                        "metadata": {
                            "description": "The number of nodes in the HDInsight cluster."
                        }
                    }
                },
                "variables": {
                    "defaultApiVersion": "2015-05-01-preview",
                    "clusterApiVersion": "2015-03-01-preview"
                },
                "resources": [{
                    "name": "[parameters('clusterStorageAccountName')]",
                    "type": "Microsoft.Storage/storageAccounts",
                    "location": "[parameters('location')]",
                    "apiVersion": "[variables('defaultApiVersion')]",
                    "dependsOn": [],
                    "tags": {
                        
                    },
                    "properties": {
                        "accountType": "Standard_LRS"
                    }
                },
                {
                    "name": "[parameters('clusterName')]",
                    "type": "Microsoft.HDInsight/clusters",
                    "location": "[parameters('location')]",
                    "apiVersion": "[variables('clusterApiVersion')]",
                    "dependsOn": ["[concat('Microsoft.Storage/storageAccounts/',parameters('clusterStorageAccountName'))]"],
                    "tags": {
                        
                    },
                    "properties": {
                        "clusterVersion": "3.2",
                        "osType": "Linux",
                        "clusterDefinition": {
                            "kind": "[parameters('clusterType')]",
                            "configurations": {
                                "gateway": {
                                    "restAuthCredential.isEnabled": true,
                                    "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
                                    "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
                                }
                            }
                        },
                        "storageProfile": {
                            "storageaccounts": [{
                                "name": "[concat(parameters('clusterStorageAccountName'),'.blob.core.windows.net')]",
                                "isDefault": true,
                                "container": "[parameters('clusterName')]",
                                "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('clusterStorageAccountName')), variables('defaultApiVersion')).key1]"
                            }]
                        },
                        "computeProfile": {
                            "roles": [{
                                "name": "headnode",
                                "targetInstanceCount": "2",
                                "hardwareProfile": {
                                    "vmSize": "Standard_D3"
                                },
                                "osProfile": {
                                    "linuxOperatingSystemProfile": {
                                        "username": "[parameters('sshUserName')]",
                                        "password": "[parameters('sshPassword')]"
                                    }
                                }
                            },
                            {
                                "name": "workernode",
                                "targetInstanceCount": "[parameters('clusterWorkerNodeCount')]",
                                "hardwareProfile": {
                                    "vmSize": "Standard_D3"
                                },
                                "osProfile": {
                                    "linuxOperatingSystemProfile": {
                                        "username": "[parameters('sshUserName')]",
                                        "password": "[parameters('sshPassword')]"
                                    }
                                }
                            }]
                        }
                    }
                }],
                "outputs": {
                    "cluster": {
                        "type": "object",
                        "value": "[reference(resourceId('Microsoft.HDInsight/clusters',parameters('clusterName')))]"
                    }
                }
            },
            "mode": "incremental",
            "Parameters": {
                "location": {
                    "value": "North Europe"
                },
                "clusterName": {
                    "value": "newclustername"
                },
                "clusterType": {
                    "value": "hadoop"
                },
                "clusterStorageAccountName": {
                    "value": "newstoragename"
                },
                "clusterLoginUserName": {
                    "value": "admin"
                },
                "clusterLoginPassword": {
                    "value": "changeme"
                },
                "sshUserName": {
                    "value": "sshuser"
                },
                "sshPassword": {
                    "value": "changeme"
                }
            }
        }
    }

Selles näites kasutatakse juhistes selles dokumendis. Väärtused, mida soovite kasutada klaster peate asendama kohatäite _väärtused_ __Parameetrid__ jaotises dokumendi lõpus.

##<a name="login-to-your-azure-subscription"></a>Azure'i tellimuse sisse logida

Järgige dokumenteerida [ühenduse Azure tellimuse: Azure'i käsurea liides (Azure'i CLI)](../xplat-cli-connect.md) ja luua ühenduse oma tellimuse abil soovitud `azure login` käsk.

##<a name="create-a-service-principal"></a>Teenuse põhilise loomine

> [AZURE.NOTE] Need juhised kehtivad teave jaotises _autentimise teenus põhisumma parooliga - Azure'i CLI_ [autentimisel teenuse põhilise Azure'i ressursihaldur](../resource-group-authenticate-service-principal.md#authenticate-service-principal-with-password---azure-cli) dokumendi lühendatud versiooni. Need juhised luua uus teenus põhisumma, mida saab kasutada autentida Azure ressursse, nt mõne Hdinsightiga kobar loomiseks kasutatud REST API taotlused.

1. Käsuviip, terminal seansi või shell abil järgmine käsk Azure tellimuste loend.

        azure account list
        
    Valige loendist tellimus, mida soovite kasutada, ja pange tähele veerust __Id__ . See on __Tellimuse ID__ ja kasutatakse enamikus juhiseid selles dokumendis.

2. Loo uus rakendus Azure Active Directory.

        azure ad app create --name "exampleapp" --home-page "https://www.contoso.org" --identifier-uris "https://www.contoso.org/example" --password <Your_Password>
        
    Väärtuste asendamine soovitud `--name`, `--home-page`, ja `--identifier-uris` oma väärtustega. Parooli ette Active Directory uus kirje.
    
    > [AZURE.NOTE] Kuna loote selle rakenduse autentimise teenuse põhilise, kuni soovitud `--home-page` ja `--identifier-uris` väärtused ei pea viidata tegeliku veebilehe majutatud Internetis; need lihtsalt peavad olema kordumatud URI-d.
    
    Tagastatud andmete salvestamine __AppId__ väärtus.
    
        data:    AppId:          4fd39843-c338-417d-b549-a545f584a745
        data:    ObjectId:       4f8ee977-216a-45c1-9fa3-d023089b2962
        data:    DisplayName:    exampleapp
        ...
        info:    ad app create command OK
    
3. Looge põhisumma __AppId__ väärtuse kasutamine tagastatud varem teenus.

        azure ad sp create 4fd39843-c338-417d-b549-a545f584a745
        
     Tagastatud andmete salvestamine __Objekti Id__ väärtus.
     
        info:    Executing command ad sp create
        - Creating service principal for application 4fd39843-c338-417d-b549-a545f584a74+
        data:    Object Id:        7dbc8265-51ed-4038-8e13-31948c7f4ce7
        data:    Display Name:     exampleapp
        data:    Service Principal Names:
        data:                      4fd39843-c338-417d-b549-a545f584a745
        data:                      https://www.contoso.org/example
        info:    ad sp create command OK
        
4. Määrata __omaniku__ roll põhisumma __Objekti ID__ väärtuse kasutamine tagastatud varem teenus. Peate kasutama ka __Tellimuse ID__ hankisite varasemas versioonis.
    
        azure role assignment create --objectId 7dbc8265-51ed-4038-8e13-31948c7f4ce7 -o Owner -c /subscriptions/{SubscriptionID}/
        
    Kui see käsk on lõpule jõudnud, peamine teenus on nüüd omanik juurdepääs määratud tellimuse ID-ga.

##<a name="get-an-authentication-token"></a>Hankida soovitud autentimise luba

1. Tellimuse __ID rentniku__ leidmiseks kasutage järgmist.

        azure account show -s <subscription ID>
        
    Tagastatud andmetest __Rentniku ID__leidmine.
    
        info:    Executing command account show
        data:    Name                        : MyAzureAccount
        data:    ID                          : 45a1014d-0f27-25d2-b838-b8f373d6d52e
        data:    State                       : Enabled
        data:    Tenant ID                   : 22f988bf-56f1-41af-91ab-3d7cd011db47
        data:    Is Default                  : true
        data:    Environment                 : AzureCloud
        data:    Has Certificate             : No
        data:    Has Access Token            : Yes
        data:    User name                   : myname@contoso.org
        data:    
        info:    account show command OK

2. Saate luua uue loa Azure'i REST API abil.

        curl -X "POST" "https://login.microsoftonline.com/TenantID/oauth2/token" \
        -H "Cookie: flight-uxoptin=true; stsservicecookie=ests; x-ms-gateway-slice=productionb; stsservicecookie=ests" \
        -H "Content-Type: application/x-www-form-urlencoded" \
        --data-urlencode "client_id=AppID" \
        --data-urlencode "grant_type=client_credentials" \
        --data-urlencode "client_secret=password" \
        --data-urlencode "resource=https://management.azure.com/"
    
    Asendage väärtused saadud või varem kasutatud __TenantID__, __AppID__ja __parool__ .

    Kui see taotlus õnnestus, saate 200 sarja vastuse ja vastuse sisu sisaldab JSON dokumendi.

    Selle päringu tagastatud JSON dokument sisaldab element nimega __access_token__; – See element väärtus juurdepääsu luba, peate kasutama autentimist taotlusi, kasutatakse selle dokumendi järgmistest jaotistest.
    
        {
            "token_type":"Bearer",
            "expires_in":"3599",
            "expires_on":"1463409994",
            "not_before":"1463406094",
            "resource":"https://management.azure.com/","access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWoNBVGZNNXBPWWlKSE1iYTlnb0VLWSIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiJodHRwczovL21hbmFnZW1lbnQuYXp1cmUuY29tLyIsImlzcyI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI2Ny8iLCJpYXQiOjE0NjM0MDYwOTQsIm5iZiI6MTQ2MzQwNjA5NCwiZXhwIjoxNDYzNDA5OTk5LCJhcHBpZCI6IjBlYzcyMzM0LTZkMDMtNDhmYi04OWU1LTU2NTJiODBiZDliYiIsImFwcGlkYWNyIjoiMSIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0Ny8iLCJvaWQiOiJlNjgxZTZiMi1mZThkLTRkZGUtYjZiMS0xNjAyZDQyNWQzOWYiLCJzdWIiOiJlNjgxZTZiMi1mZThkLTRkZGUtYjZiMS0xNjAyZDQyNWQzOWYiLCJ0aWQiOiI3MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMWRiNDciLCJ2ZXIiOiIxLjAifQ.nJVERbeDHLGHn7ZsbVGBJyHOu2PYhG5dji6F63gu8XN2Cvol3J1HO1uB4H3nCSt9DTu_jMHqAur_NNyobgNM21GojbEZAvd0I9NY0UDumBEvDZfMKneqp7a_cgAU7IYRcTPneSxbD6wo-8gIgfN9KDql98b0uEzixIVIWra2Q1bUUYETYqyaJNdS4RUmlJKNNpENllAyHQLv7hXnap1IuzP-f5CNIbbj9UgXxLiOtW5JhUAwWLZ3-WMhNRpUO2SIB7W7tQ0AbjXw3aUYr7el066J51z5tC1AK9UC-mD_fO_HUP6ZmPzu5gLA6DxkIIYP3grPnRVoUDltHQvwgONDOw"
        }

##<a name="create-a-resource-group"></a>Ressursi rühma loomine

Järgmine abil saate luua uue ressursirühma. Peate looma rühma esmalt ressursid, nt Hdinsightiga kobar loomiseks. 

* Asendage __SubscriptionID__ Tellimuse ID, mis on saanud teenuse põhilise loomisel.
* Asendage __AccessToken__ juurdepääsu luba saadud eelmist toimingut.
* Asendage __DataCenterLocation__ soovite luua ressursirühm ja ressursse, data Centeri kaudu. Näiteks "Lõuna keskse meie". 
* Nimi, mida soovite kasutada selle rühma __ResourceGroupName__ asendamiseks tehke järgmist.

```
curl -X "PUT" "https://management.azure.com/subscriptions/SubscriptionID/resourcegroups/ResourceGroupName?api-version=2015-01-01" \
    -H "Authorization: Bearer AccessToken" \
    -H "Content-Type: application/json" \
    -d $'{
"location": "DataCenterLocation"
}'
```

Kui see taotlus õnnestus, saate 200 sarja vastuse ja vastuse sisu sisaldab JSON dokument, mis sisaldab teavet rühma. Funktsiooni `"provisioningState"` element sisaldab väärtus `"Succeeded"`.

##<a name="create-a-deployment"></a>Looge juurutamine

Kasutage järgmist konfiguratsiooni kobar (mall ja parameetrite väärtused,) ressursi rühma.

* Asendage __SubscriptionID__ ja __AccessToken__ varem kasutatud väärtused. 
* Asendage __ResourceGroupName__ eelmises tööetapis loodud ressursi rühma nime.
* Asendage nimi, mida soovite kasutada seda juurutamiseks __DeploymentName__ .

```
curl -X "PUT" "https://management.azure.com/subscriptions/SubscriptionID/resourcegroups/ResourceGroupName/providers/microsoft.resources/deployments/DeploymentName?api-version=2015-01-01" \
-H "Authorization: Bearer AccessToken" \
-H "Content-Type: application/json" \
-d "{set your body string to the template and parameters}"
```

> [AZURE.NOTE] Kui mall ja parameetrite faili sisaldavad JSON dokumendi salvestanud, saate kasutada järgmist asemel `-d "{ template and parameters}"`:
>
> `--data-binary "@/path/to/file.json"`

Kui see taotlus õnnestus, saate 200 sarja vastuse ja vastuse sisu sisaldab JSON dokument, mis sisaldab teavet juurutamine toiming.

> [AZURE.IMPORTANT] Pange tähele, et juurutamise on esitatud, kuid pole praegu lõpule. Võib kuluda mitu minutit, tavaliselt umbes 15 juurutamiseks lõpuleviimiseks.

##<a name="check-the-status-of-a-deployment"></a>Värskendustoimingu oleku juurutamine

Juurutamise oleku, kasutage järgmist:

* Asendage __SubscriptionID__ ja __AccessToken__ varem kasutatud väärtused. 
* Asendage __ResourceGroupName__ eelmises tööetapis loodud ressursi rühma nime.

```
curl -X "GET" "https://management.azure.com/subscriptions/SubscriptionID/resourcegroups/ResourceGroupName/providers/microsoft.resources/deployments/DeploymentName?api-version=2015-01-01" \
-H "Authorization: Bearer AccessToken" \
-H "Content-Type: application/json"
```

See tagastama teavet JSON dokument, mis sisaldab teavet juurutamine toiming. Funktsiooni `"provisioningState"` element sisaldab olekut juurutamise; kui see sisaldab väärtust `"Succeeded"`, siis juurutamise on lõpule viidud. Selles etapis tuleb klaster kasutamiseks saadaval.

##<a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete loonud mõne Hdinsightiga kobar, kasutage järgmist saate teada, kuidas töötada klaster. 

###<a name="hadoop-clusters"></a>Hadoopi kogumite

* [Hdinsightiga taru kasutamine](hdinsight-use-hive.md)
* [Kasutage siga Hdinsightiga](hdinsight-use-pig.md)
* [Hdinsightiga MapReduce kasutamine](hdinsight-use-mapreduce.md)

###<a name="hbase-clusters"></a>HBase kogumite

* [Klõpsake Hdinsightiga HBase kasutamise alustamine](hdinsight-hbase-tutorial-get-started-linux.md)
* [Arendamise kohta Hdinsightiga HBase taotlused](hdinsight-hbase-build-java-maven-linux.md)

###<a name="storm-clusters"></a>Torm kogumite

* [Java topoloogiatest arendamise Storm Hdinsightiga kohta](hdinsight-storm-develop-java-topology.md)
* [Kasutage Python komponendid Storm Hdinsightiga kohta](hdinsight-storm-develop-python-topology.md)
* [Juurutada ja kontrollida topoloogiatest torm Hdinsightiga kohta](hdinsight-storm-deploy-monitor-topology-linux.md)
