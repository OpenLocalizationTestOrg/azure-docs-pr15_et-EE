<properties
    pageTitle="Ressursihaldur Mallid töötlemine riigi | Microsoft Azure'i"
    description="Kuvab soovitatav keerukate objektide andmete jagamiseks Azure'i ressursihaldur Mallid ja lingitud mallide kasutamise võimalused"
    services="azure-resource-manager"
    documentationCenter=""
    authors="tfitzmac"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="azure-resource-manager"
    ms.workload="multiple"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="tomfitz"/>

# <a name="sharing-state-in-azure-resource-manager-templates"></a>Ühiskasutuse riigi Azure'i ressursihaldur Mallid

Selles teemas kirjeldatakse head tavad haldamine ja ühiskasutus jooksul mallid. Parameetrite ja muutujate näidatud selles teemas on tüüp, saate määratleda objektide näited mugavalt korraldada oma juurutamise nõuetele. Näidetes, saate rakendada oma objektide atribuudi väärtustega, mis muudavad teie keskkonda.

Selles teemas on suurem lühiülevaade osa. Täielik paberi lugemiseks alla laadida [maailma klassi ressursi halduri Mallid kasutuse ja tõendada juhistega](http://download.microsoft.com/download/8/E/1/8E1DBEFA-CECE-4DC9-A813-93520A5D7CFE/World Class ARM Templates - Considerations and Proven Practices.pdf).


## <a name="provide-standard-configuration-settings"></a>Sisestage standardse konfiguratsioonisätted

Selle asemel, et pakkuda Mall, mis pakub täielikku paindlikkuse ja lugematuid variatsioonid, levinud mustri on valiku teadaolevad konfiguratsiooni. Kasutajad saavad, valida näiteks Liivakasti, väike, Keskmine ja suur t-särk suurusega. Teisi näiteid t-särk suurused on toote pakkumisi, nt ühenduse edition või enterprise edition. Muudel juhtudel võib olla töökoormus kohased konfiguratsioone tehnoloogia – nagu kaarti vähendada või pole SQL-i.

Keerukate objektidega, saate luua muutujat sisaldavate andmete, tuntud ka kui "Atribuudikotid" ja ressursside deklareerimise juhtida seda malli andmete kasutamiseks. Seda moodust pakub klientidele hea, tuntud konfiguratsioone erineva suurusega, mis on eelkonfigureeritud. Ilma teadaolevad konfiguratsioone, peab malli kasutajatele määrata oma sortimine kobar, tegur platvormi ressursipiirangute ja arvutustehted tuvastamiseks tulemuseks eraldamine salvestusruumi kontod ja muud ressursid (kobar suurus ja ressursside piirangute) tõttu. Lisaks kliendi jaoks mugavamaks tegemist on mõned teadaolevad konfiguratsioone lihtsam toetavad ja aitavad teil esitamisel tihedusfunktsiooni kõrgemal tasemel.

Järgmises näites on kujutatud muutujat, mis sisaldavad keerukate objektide jaoks, mis tähistab andmete määratlemine. Kogumite määratleda väärtused, mida kasutatakse virtuaalse masina suurus, võrgu sätteid, operatsioonisüsteemi sätted ja kättesaadavus sätted.

    "variables": {
      "tshirtSize": "[variables(concat('tshirtSize', parameters('tshirtSize')))]",
      "tshirtSizeSmall": {
        "vmSize": "Standard_A1",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
        "vmCount": 2,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 1,
          "pool": "db",
          "map": [0,0],
          "jumpbox": 0
        }
      },
      "tshirtSizeMedium": {
        "vmSize": "Standard_A3",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-8disk-resources.json')]",
        "vmCount": 2,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 2,
          "pool": "db",
          "map": [0,1],
          "jumpbox": 0
        }
      },
      "tshirtSizeLarge": {
        "vmSize": "Standard_A4",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-16disk-resources.json')]",
        "vmCount": 3,
        "slaveCount": 2,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 2,
          "pool": "db",
          "map": [0,1,1],
          "jumpbox": 0
        }
      },
      "osSettings": {
        "scripts": [
          "[concat(variables('templateBaseUrl'), 'install_postgresql.sh')]",
          "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/shared_scripts/ubuntu/vm-disk-utils-0.1.sh"
        ],
        "imageReference": {
          "publisher": "Canonical",
          "offer": "UbuntuServer",
          "sku": "14.04.2-LTS",
          "version": "latest"
        }
      },
      "networkSettings": {
        "vnetName": "[parameters('virtualNetworkName')]",
        "addressPrefix": "10.0.0.0/16",
        "subnets": {
          "dmz": {
            "name": "dmz",
            "prefix": "10.0.0.0/24",
            "vnet": "[parameters('virtualNetworkName')]"
          },
          "data": {
            "name": "data",
            "prefix": "10.0.1.0/24",
            "vnet": "[parameters('virtualNetworkName')]"
          }
        }
      },
      "availabilitySetSettings": {
        "name": "pgsqlAvailabilitySet",
        "fdCount": 3,
        "udCount": 5
      }
    }

Pange tähele, et **tshirtSize** muutuja ühendab soovite teksti **tshirtSize**t-särgi suurus, mida andnud parameetri (**väike**, **Keskmine**, **suur**) kaudu. Saate tuua, et t-särk suurus seotud keerukate objektimuutuja muutuja.

Seejärel saate otsida nende muutujate hiljem malli. Võimalus viide nimega muutujate ja nende atribuutide lihtsustab malli süntaks ja on lihtne mõista kontekstis. Järgmises näites määratleb ressursi juurutamiseks väärtused varem näidatud objektide abil. Näiteks VM suurus määratud väärtuse toomine `variables('tshirtSize').vmSize` samas ketta suurus tuuakse väärtus `variables('tshirtSize').diskSize`. Lisaks lingitud malli URI on seadistatud väärtus `variables('tshirtSize').vmTemplate`.

    "name": "master-node",
    "type": "Microsoft.Resources/deployments",
    "apiVersion": "2015-01-01",
    "dependsOn": [
        "[concat('Microsoft.Resources/deployments/', 'shared')]"
    ],
    "properties": {
        "mode": "Incremental",
        "templateLink": {
          "uri": "[variables('tshirtSize').vmTemplate]",
          "contentVersion": "1.0.0.0"
        },
        "parameters": {
          "adminPassword": {
            "value": "[parameters('adminPassword')]"
          },
          "replicatorPassword": {
            "value": "[parameters('replicatorPassword')]"
          },
          "osSettings": {
            "value": "[variables('osSettings')]"
          },
          "subnet": {
            "value": "[variables('networkSettings').subnets.data]"
          },
          "commonSettings": {
            "value": {
              "region": "[parameters('region')]",
              "adminUsername": "[parameters('adminUsername')]",
              "namespace": "ms"
            }
          },
          "storageSettings": {
            "value":"[variables('tshirtSize').storage]"
          },
          "machineSettings": {
            "value": {
              "vmSize": "[variables('tshirtSize').vmSize]",
              "diskSize": "[variables('tshirtSize').diskSize]",
              "vmCount": 1,
              "availabilitySet": "[variables('availabilitySetSettings').name]"
            }
          },
          "masterIpAddress": {
            "value": "0"
          },
          "dbType": {
            "value": "MASTER"
          }
        }
      }
    }

## <a name="pass-state-to-a-template"></a>Liigu olekus lisamine mallile

Malli otse juurutamisel varustavate parameetrite kaudu ühiskasutusse olekus.

Järgmises tabelis on loetletud levinumaid parameetrite mallid.

Nimi | Väärtus | Kirjeldus
---- | ----- | -----------
asukoht    | Azure'i piirkondade piiratud loendist string   | Kus on juurutatud ressursside asukoht.
storageAccountNamePrefix    | String    | Kui soovitud VM ketast suunatakse salvestusruumi konto DNS-i kordumatu nimi
Domeeninimi  | String    | Domeeninime avalikult juurdepääsetava jumpbox VM vormingus: **{domainName}. {} asukoht}.cloudapp.com** näiteks: **mydomainname.westus.cloudapp.azure.com**
adminUsername   | String    | Kasutajanimi VMs
adminPassword   | String    | Parooli VMs
tshirtSize  | Stringi piiratud loendist pakutud t-särk suurus   | Nimega skaala ühiku suurus ettevalmistamine. Näiteks "Väike", "Keskmine", "Suur"
virtualNetworkName  | String    | Mis tarbija soovib kasutada virtuaalse võrgu nimi.
enableJumpbox   | Tekstistringi piiratud loendist (lubatud või keelatud) | Parameeter, mis määrab, kas lubada jumpbox, keskkonnas. Väärtused: "lubatud", "keelatud"

Eelmises jaotises kasutatud **tshirtSize** parameetri defineeritakse järgmiselt:

    "parameters": {
      "tshirtSize": {
        "type": "string",
        "defaultValue": "Small",
        "allowedValues": [
          "Small",
          "Medium",
          "Large"
        ],
        "metadata": {
          "Description": "T-shirt size of the MongoDB deployment"
        }
      }
    }


## <a name="pass-state-to-linked-templates"></a>Edastama olekus lingitud Mallid

Lingitud Mallid ühendamisel sageli kasutate kombinatsiooni staatiline ja loodud muutujate.

### <a name="static-variables"></a>Staatilised muutujad

Staatilised muutujad kasutatakse sageli väärtused, nt URL, mida kasutatakse kogu malli.

Klõpsake järgmist malli väljavõte, `templateBaseUrl` saate määrata GitHub juurkausta malli jaoks soovitud asukoht. Järgmisele reale koostab uue `sharedTemplateUrl` mis ühendab base URL-i teadaolevad jagatud ressursid malli nimi. Allpool sellel real kasutatakse keerukate objektimuutuja t-särgi suurus, kus base URL-i formeerimiseks teadaolevad konfiguratsiooni malli asukoht ja talletatud talletamiseks soovitud `vmTemplate` atribuut.

Seda moodust kasu on, et kui malli asukoht muutub, tuleb ainult muuta staatilise muutuja edastab selle kogu lingitud Mallid ühes kohas.

    "variables": {
      "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
      "sharedTemplateUrl": "[concat(variables('templateBaseUrl'), 'shared-resources.json')]",
      "tshirtSizeSmall": {
        "vmSize": "Standard_A1",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
        "vmCount": 2,
        "slaveCount": 1,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 1,
          "pool": "db",
          "map": [0,0],
          "jumpbox": 0
        }
      }
    }

### <a name="generated-variables"></a>Loodud muutujad

Lisaks staatilised muutujad, luuakse dünaamiliselt mitut muutujat. Selles jaotises tuvastab mõned levinud liiki loodud muutujat.

#### <a name="tshirtsize"></a>tshirtSize

Olete tuttav näidetes loodud muutuja.

#### <a name="networksettings"></a>networkSettings

Võimsus, võimalus või-lõpuni jäävates lahenduse malli lingitud Mallid luua tavaliselt ressursse, mis on olemas võrgus. Ühe lihtne lähenemine on võrgu sätteid talletada ja lingitud Mallid edastada keerulise objekti abil.

Allpool on näha näide suhtlemine võrgu sätteid.

    "networkSettings": {
      "vnetName": "[parameters('virtualNetworkName')]",
      "addressPrefix": "10.0.0.0/16",
      "subnets": {
        "dmz": {
          "name": "dmz",
          "prefix": "10.0.0.0/24",
          "vnet": "[parameters('virtualNetworkName')]"
        },
        "data": {
          "name": "data",
          "prefix": "10.0.1.0/24",
          "vnet": "[parameters('virtualNetworkName')]"
        }
      }
    }

#### <a name="availabilitysettings"></a>availabilitySettings

Sageli paigutatakse ressursse, mis on loodud mallide lingitud mõne kättesaadavus määramine. Järgmises näites on määratud kättesaadavus hulga nimi ja ka viga domeeni ja update domain loendamiseks kasutage.

    "availabilitySetSettings": {
      "name": "pgsqlAvailabilitySet",
      "fdCount": 3,
      "udCount": 5
    }

Kui teil on vaja mitut kättesaadavus komplekti (nt üks juhtslaidi sõlmed) ja teise andmete sõlmed, saate kasutada nime eesliide, määrata kättesaadavus mitmele kriteeriumikogumile või järgige varem esitatud loomise muutuja jaoks teatud t-särgi suurus.

#### <a name="storagesettings"></a>storageSettings

Salvestusruumi üksikasjad ühiskasutusse antud sageli lingitud mallid. Alltoodud näites *storageSettings* objekti üksikasju salvestusruumi konto ja container nimed.

    "storageSettings": {
        "vhdStorageAccountName": "[parameters('storageAccountName')]",
        "vhdContainerName": "[variables('vmStorageAccountContainerName')]",
        "destinationVhdsContainer": "[concat('https://', parameters('storageAccountName'), variables('vmStorageAccountDomain'), '/', variables('vmStorageAccountContainerName'), '/')]"
    }

#### <a name="ossettings"></a>osSettings

Lingitud malle, peate operatsioonisüsteemi sätted edastamiseks erinevaid sõlmed üle erinevate teadaolevad konfiguratsiooni puhul. Keerulise objekti on lihtne viis talletamiseks ja ühiskasutuseks opsüsteemi teavet ja ka on hõlpsam toetab mitut operatsioonisüsteemi Valikud juurutamiseks.

Järgmises näites on kujutatud objekti *osSettings*jaoks:

    "osSettings": {
      "imageReference": {
        "publisher": "Canonical",
        "offer": "UbuntuServer",
        "sku": "14.04.2-LTS",
        "version": "latest"
      }
    }

#### <a name="machinesettings"></a>machineSettings

Loodud muutuja, *machineSettings* on keerukas objekti, mis sisaldavad kombinatsiooni core muutujate loomise VM. Muutujaid sisaldavad administraatori kasutajanime ja parooli, eesliite VM nimed ja on operatsioonisüsteem pilt viide.

    "machineSettings": {
        "adminUsername": "[parameters('adminUsername')]",
        "adminPassword": "[parameters('adminPassword')]",
        "machineNamePrefix": "mongodb-",
        "osImageReference": {
            "publisher": "[variables('osFamilySpec').imagePublisher]",
            "offer": "[variables('osFamilySpec').imageOffer]",
            "sku": "[variables('osFamilySpec').imageSKU]",
            "version": "latest"
        }
    },

Pange tähele, et selle *osImageReference* toob väärtused *osSettings* muutuja määratletud peamised malli. See tähendab, et saate hõlpsalt muuta operatsioonisüsteemi VM – täielikult või eelistus tarbija malli põhjal.

#### <a name="vmscripts"></a>vmScripts

*VmScripts* objekt sisaldab üksikasjalikku teavet skriptide allalaadimiseks ja käivitada VM eksemplari, sh ka väljaspool viited. Viited sisaldavad väljaspool infrastruktuuri. Sisemine viited sisaldavad installitud tarkvara ja konfiguratsiooni.

Atribuudi *scriptsToDownload* abil saate loendi skriptide allalaadimiseks VM. See objekt sisaldab ka viiteid käsurea argumendid erinevat tüüpi toiminguid. Neid toiminguid kaasata iga üksiku sõlme vaikimisi installi, installimise, mida käitatakse pärast kõik sõlmed on juurutatud ja täiendavad skriptide, mis võivad olla antud malli eriomased.

Selles näites on kasutatud juurutamiseks MongoDB, mis nõuab arbiter esitamisel kõrge-saadavus malli põhjal. *ArbiterNodeInstallCommand* on lisatud *vmScripts* vahekohtunik installimiseks.

Jaotise muutujat on, kus muutujat, mis määratlevad kindla teksti käivitada skripti proper väärtustega.

    "vmScripts": {
        "scriptsToDownload": [
            "[concat(variables('scriptUrl'), 'mongodb-', variables('osFamilySpec').osName, '-install.sh')]",
            "[concat(variables('sharedScriptUrl'), 'vm-disk-utils-0.1.sh')]"
        ],
        "regularNodeInstallCommand": "[variables('installCommand')]",
        "lastNodeInstallCommand": "[concat(variables('installCommand'), ' -l')]",
        "arbiterNodeInstallCommand": "[concat(variables('installCommand'), ' -a')]"
    },


## <a name="return-state-from-a-template"></a>Saatja riigi malli põhjal

Mitte ainult te kaotate andmed malli, saate jagada ka andmed tagasi helistaja mall. Jaotises **väljundid** lingitud malli, saate sisestada /-väärtuse paarideks, saate tarbitud andmeallika mall.

Järgmises näites kujutatakse edasi privaatne IP-aadress, mis on loodud lingitud malli.

    "outputs": {
        "masterip": {
            "value": "[reference(concat(variables('nicName'),0)).ipConfigurations[0].properties.privateIPAddress]",
            "type": "string"
         }
    }

Peamised Mall, saate need andmed järgmist süntaksit:

    "[reference('master-node').outputs.masterip.value]"

See avaldis väljundeid jaotis või ressursse jaotise peamised malli saate kasutada. Ei saa kasutada avaldise muutujate jaotises, kuna see sõltub käitusaja olekus. Tagastusväärtus see peamine malli kasutada.

    "outputs": { 
      "masterIpAddress": {
        "value": "[reference('master-node').outputs.masterip.value]",
        "type": "string"
      }
     
Näitena kasutades väljundeid jaotist lingitud malli juurde naasmiseks andmete ketast virtuaalse masina jaoks, lugege teemat [loomine mitme andmete ketast virtuaalse masina jaoks](resource-group-create-multiple.md#creating-multiple-data-disks-for-a-virtual-machine).

## <a name="define-authentication-settings-for-virtual-machine"></a>Virtuaalse masina autentimissätted määratlemine

Sama muster kasutada eelnevalt konfiguratsiooni sätete abil saate määrata virtuaalse masina autentimissätted. Saate luua parameeter on läbimise autentimise tüüp.

    "parameters": {
      "authenticationType": {
        "allowedValues": [
          "password",
          "sshPublicKey"
        ],
        "defaultValue": "password",
        "metadata": {
          "description": "Authentication type"
        },
        "type": "string"
      }
    }

Muutujate jaoks eri autentimistüüpidest lisamist ja muutuja, mis tüüpi talletamiseks kasutatakse selle juurutamiseks parameetri väärtuse.

    "variables": {
      "osProfile": "[variables(concat('osProfile', parameters('authenticationType')))]",
      "osProfilepassword": {
        "adminPassword": "[parameters('adminPassword')]",
        "adminUsername": "notused",
        "computerName": "[parameters('vmName')]",
        "customData": "[base64(variables('customData'))]"
      },
      "osProfilesshPublicKey": {
        "adminUsername": "notused",
        "computerName": "[parameters('vmName')]",
        "customData": "[base64(variables('customData'))]",
        "linuxConfiguration": {
          "disablePasswordAuthentication": "true",
          "ssh": {
            "publicKeys": [
              {
                "keyData": "[parameters('sshPublicKey')]",
                "path": "/home/notused/.ssh/authorized_keys"
              }
            ]
          }
        }
      }
    }

Virtuaalse masina määratlemisel määrata loodud muutuja **osProfile** .

    {
      "type": "Microsoft.Compute/virtualMachines",
      ...
      "osProfile": "[variables('osProfile')]"
    }


## <a name="next-steps"></a>Järgmised sammud
- Malli jaotiste kohta leiate teemast [Azure ressursihaldur mallide koostamine](resource-group-authoring-templates.md)
- Funktsioonid, mis on saadaval malli leiate [Azure'i ressursihaldur malli funktsioonid](resource-group-template-functions.md)

