<properties
   pageTitle="Maakond ressursi halduri Mall soovitud | Microsoft Azure'i"
   description="Ressursihaldur malli definitsiooni soovitud riik konfiguratsiooni Azure näiteid ja tõrkeotsingu"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="zjalexander"
   manager="timlt"
   editor=""
   tags="azure-service-management,azure-resource-manager"
   keywords=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="na"
   ms.date="09/15/2016"
   ms.author="zachal"/>

# <a name="windows-vmss-and-desired-state-configuration-with-azure-resource-manager-templates"></a>Windowsi VMSS ja soovitud riik konfiguratsiooni Azure'i ressursihaldur Mallid
Selles artiklis kirjeldatakse [soovitud riik konfiguratsiooni laiend sündmuseohjuri](virtual-machines-windows-extensions-dsc-overview.md)ressursihaldur mall. 

## <a name="template-example-for-a-windows-vm"></a>Windows VM malli näide

Järgmised koodilõigu läheb ressursi jaotise malli.

```json
            "name": "Microsoft.Powershell.DSC",
            "type": "extensions",
             "location": "[resourceGroup().location]",
             "apiVersion": "2015-06-15",
             "dependsOn": [
                  "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
              ],
              "properties": {
                  "publisher": "Microsoft.Powershell",
                  "type": "DSC",
                  "typeHandlerVersion": "2.20",
                  "autoUpgradeMinorVersion": true,
                  "forceUpdateTag": "[parameters('dscExtensionUpdateTagVersion')]",
                  "settings": {
                      "configuration": {
                          "url": "[concat(parameters('_artifactsLocation'), '/', variables('dscExtensionArchiveFolder'), '/', variables('dscExtensionArchiveFileName'))]",
                          "script": "dscExtension.ps1",
                          "function": "Main"
                      },
                      "configurationArguments": {
                          "nodeName": "[variables('vmName')]"
                      }
                  },
                  "protectedSettings": {
                      "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                  }

```

## <a name="template-example-for-windows-vmss"></a>Windowsi VMSS malli näide

VMSS sõlm on "atribuudid" jaotis "VirtualMachineProfile", "extensionProfile" atribuut abil. DSC lisatakse jaotises "laiendid". 

```json
"extensionProfile": {
            "extensions": [
                {
                    "name": "Microsoft.Powershell.DSC",
                    "properties": {
                        "publisher": "Microsoft.Powershell",
                        "type": "DSC",
                        "typeHandlerVersion": "2.20",
                        "autoUpgradeMinorVersion": true,
                        "forceUpdateTag": "[parameters('DscExtensionUpdateTagVersion')]",
                        "settings": {
                            "configuration": {
                                "url": "[concat(parameters('_artifactsLocation'), '/', variables('DscExtensionArchiveFolder'), '/', variables('DscExtensionArchiveFileName'))]",
                                "script": "DscExtension.ps1",
                                "function": "Main"
                            },
                            "configurationArguments": {
                                "nodeName": "localhost"
                            }
                        },
                        "protectedSettings": {
                            "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                        }
                    }
                }
            ]
```

## <a name="detailed-settings-information"></a>Sätete üksikasjalik teave

Järgmine skeem on Azure DSC pikenduse mallis Azure'i ressursihaldur jaoks sätted.

```json

"settings": {
  "wmfVersion": "latest",
  "configuration": {
    "url": "http://validURLToConfigLocation",
    "script": "ConfigurationScript.ps1",
    "function": "ConfigurationFunction"
  },
  "configurationArguments": {
    "argument1": "Value1",
    "argument2": "Value2"
  },
  "configurationData": {
    "url": "https://foo.psd1"
  },
  "privacy": {
    "dataCollection": "enable"
  },
  "advancedOptions": {
    "downloadMappings": {
      "customWmfLocation": "http://myWMFlocation"
    }
  } 
},
"protectedSettings": {
  "configurationArguments": {
    "parameterOfTypePSCredential1": {
      "userName": "UsernameValue1",
      "password": "PasswordValue1"
    },
    "parameterOfTypePSCredential2": {
      "userName": "UsernameValue2",
      "password": "PasswordValue2"
    }
  },
  "configurationUrlSasToken": "?g!bber1sht0k3n",
  "configurationDataUrlSasToken": "?dataAcC355T0k3N"
}

```

## <a name="details"></a>Üksikasjad
| Atribuudi nimi | Tüüp | Kirjeldus |
| --- | --- | --- |
| settings.wmfVersion | string | Määrab versiooni Windows Management Frameworki, mis peaks olema installitud oma VM. Säte selle atribuudi 'Viimane' installid WMF viimati värskendatud versiooni. Ainult praegune võimalikud väärtused selle atribuudi on **"4.0", "5.0", "5.0PP' ja 'Viimane'**. Nende võimalikud väärtused on värskendada. Vaikeväärtus on 'Viimane'.|
| Settings.Configuration.URL | string | Saate määrata asukoha URL, mille alla laadida oma DSC konfiguratsiooni zip-fail. Kui antud URL nõuab SAS luba juurdepääsu, peate oma SAS luba väärtus atribuudi protectedSettings.configurationUrlSasToken. Atribuut on nõutav, kui on määratletud settings.configuration.script ja/või settings.configuration.function. |
| Settings.Configuration.script | string | Määrab skripti, mis sisaldab teie DSC konfiguratsiooni määratluse faili nime. See skript peab olema juurkausta zip faili alla laadida URL, mis on määratud configuration.url atribuuti. Atribuut on nõutav, kui on määratletud settings.configuration.url ja/või settings.configuration.script. |
| Settings.Configuration.Function | string | Saate määrata oma DSC konfiguratsiooni nimi. Nimega konfiguratsiooni peavad sisaldama skripti, mis on määratletud configuration.script. Atribuut on nõutav, kui on määratletud settings.configuration.url ja/või settings.configuration.function. |
| settings.configurationArguments | Saidikogumi | Määratleb kõigi parameetrite soovite edastada oma DSC konfigureerimine. Atribuut on krüptitud. |
| settings.configurationData.url | string | Saate määrata URL, mille alla laadida konfiguratsiooni andmed (.pds1) faili kasutada sisendina teie DSC konfiguratsiooni. Kui antud URL nõuab SAS luba juurdepääsu, peate oma SAS luba väärtus atribuudi protectedSettings.configurationDataUrlSasToken.|
| settings.privacy.dataEnabled | string | Lubamine või keelamine telemeetria saidikogumi. On võimalik ainult väärtused selle atribuudi **"Lubamine", "Tühista", ", või $null**. Jätke selle atribuudi tühi ega null võimaldab telemeetria. Vaikeväärtus on ". [Lisateave](https://blogs.msdn.microsoft.com/powershell/2016/02/02/azure-dsc-extension-data-collection-2/) |
| settings.advancedOptions.downloadMappings | Saidikogumi | Määratleb, mille alla laadida WMF alternatiivse asukohad. [Lisateave](http://blogs.msdn.com/b/powershell/archive/2015/10/21/azure-dsc-extension-2-2-amp-how-to-map-downloads-of-the-extension-dependencies-to-your-own-location.aspx) |
| protectedSettings.configurationArguments | Saidikogumi | Määratleb kõigi parameetrite soovite edastada oma DSC konfigureerimine. Atribuut on krüptitud. |
| protectedSettings.configurationUrlSasToken | string | Saate määrata SAS luba juurdepääsu configuration.url määratletud URL-i. Atribuut on krüptitud. |
| protectedSettings.configurationDataUrlSasToken | string | Saate määrata SAS luba juurdepääsu configurationData.url määratletud URL-i. Atribuut on krüptitud. |

## <a name="settings-vs-protectedsettings"></a>Sätete vs ProtectedSettings
Tekstifaili sätted VM salvestatakse kõik sätted.
Atribuudid jaotises "seaded" on avaliku atribuudid, sest need pole krüptitud tekstifaili sätted.
Atribuudid jaotises "protectedSettings" krüptitud sertifikaadiga ja ei kuvata seda faili VM lihttekstina.

Kui konfiguratsiooni peab mandaat, saate need sisalduvad protectedSettings:

```json
"protectedSettings": {
    "configurationArguments": {
        "parameterOfTypePSCredential1": {
            "userName": "UsernameValue1",
            "password": "PasswordValue1"
        }
    }
}
```

## <a name="example"></a>Näide

Järgmises näites tuleneb [DSC laiend sündmuseohjuri lehel](virtual-machines-windows-extensions-dsc-overview.md)jaotises "Alustamine".
Selles näites kasutatakse ressursihaldur Mallid asemel cmdlettide juurutamiseks laiendamine. Salvestada "IisInstall.ps1" konfiguratsiooni, pange see on. ZIP-fail ja laadige siis fail puuetega inimestele juurdepääsetavate URL. Selles näites kasutatakse Azure'i bloobimälu, kuid see on võimalik alla laadida. ZIP-failide suvalise kohas.

Azure'i ressursihaldur malli, juhendab järgmine kood VM õige faili alla laadida ja käivitada PowerShelli vastava funktsiooni:

```json
"settings": {
    "configuration": {
        "url": "https://demo.blob.core.windows.net/",
        "script": "IisInstall.ps1",
        "function": "IISInstall"
    }
    } 
},
"protectedSettings": {
    "configurationUrlSasToken": "odLPL/U1p9lvcnp..."
}
```

## <a name="updating-from-the-previous-format"></a>Eelmise vormingust värskendamine
Eelmise vormingus (sisaldab avaliku atribuutide ModulesUrl, ConfigurationFunction, SasToken või atribuudid) automaatselt sätteid kohandada praegusesse failivormingusse ja käivitage ainult nii, nagu enne.

Järgmine skeem on mis eelmise sätted skeemiga vanal:

```json
"settings": {
    "WMFVersion": "latest",
    "ModulesUrl": "https://UrlToZipContainingConfigurationScript.ps1.zip",
    "SasToken": "SAS Token if ModulesUrl points to private Azure Blob Storage",
    "ConfigurationFunction": "ConfigurationScript.ps1\\ConfigurationFunction",
    "Properties":  {
        "ParameterToConfigurationFunction1":  "Value1",
        "ParameterToConfigurationFunction2":  "Value2",
        "ParameterOfTypePSCredential1": {
            "UserName": "UsernameValue1",
            "Password": "PrivateSettingsRef:Key1" 
        },
        "ParameterOfTypePSCredential2": {
            "UserName": "UsernameValue2",
            "Password": "PrivateSettingsRef:Key2"
        }
    }
},
"protectedSettings": { 
    "Items": {
        "Key1": "PasswordValue1",
        "Key2": "PasswordValue2"
    },
    "DataBlobUri": "https://UrlToConfigurationDataWithOptionalSasToken.psd1"
}
```

Siin on, kuidas eelmise vormingu kohandub praegusesse failivormingusse.

| Atribuudi nimi | Eelmise skeemi ekvivalent |
| --- | --- |
| settings.wmfVersion | sätted. WMFVersion |
| Settings.Configuration.URL | sätted. ModulesUrl |
| Settings.Configuration.script | Esimene osa sätted. ConfigurationFunction (enne '\\\\") |
| Settings.Configuration.Function | Teine osa sätted. ConfigurationFunction (pärast '\\\\") |
| settings.configurationArguments | sätted. Atribuudid |
| settings.configurationData.url | protectedSettings.DataBlobUri (ilma SAS luba) |
| settings.privacy.dataEnabled | sätted. Privacy.DataEnabled |
| settings.advancedOptions.downloadMappings | sätted. AdvancedOptions.DownloadMappings |
| protectedSettings.configurationArguments | protectedSettings.Properties |
| protectedSettings.configurationUrlSasToken | sätted. SasToken |
| protectedSettings.configurationDataUrlSasToken | SAS luba protectedSettings.DataBlobUri kaudu |


## <a name="troubleshooting---error-code-1100"></a>Tõrkeotsing – tõrkekood 1100
Tõrkekood 1100 märgitud kasutaja sisendist DSC laiend probleem.
Nende vigade tekst on muutuja ja võib muutuda.
Siin on mõned tõrked võivad tekib ja kuidas neid parandada.

### <a name="invalid-values"></a>Sobimatud väärtused
"Privacy.dataCollection on {0}. On võimalik ainult väärtused ","lubamine ja keelamine"" "WmfVersion on {0}. Ainult võimalikud väärtused on... ja 'Viimane' "

Probleem: Sisestatud väärtus pole lubatud.

Lahendus: Muuta sobimatu väärtuse sobiv väärtus. Vaadake tabelit jaotises üksikasjad.

### <a name="invalid-url"></a>Sobimatu URL
"ConfigurationData.url on {0}. See pole kehtiv URL""DataBlobUri on {0}. See pole kehtiv URL""Configuration.url on {0}. See pole kehtiv URL"

Probleem: A esitatud URL on kehtetu.

Lahendus: Kontrollige sisestatud URL. Veenduge, et kõik URL-id lahendamiseks lubatud asukohad, et kaugarvutis pääsevad laiendamine.

### <a name="invalid-configurationargument-type"></a>Sobimatu ConfigurationArgument tüüp
"Lubamatud configurationArguments tüüp {0}"

Probleem: Atribuudi ConfigurationArguments ei saa lahendada objektis Hashtable talletatakse. 

Lahendus: Teha oma ConfigurationArguments atribuut on Hashtable talletatakse. Järgige eelmises näites toodud vorming. Olge looksulud hinnapakkumised ja komad.

### <a name="duplicate-configurationarguments"></a>Dubleeritud ConfigurationArguments
"Leitud kaitstud ja kohtades configurationArguments dubleeritud argumendid {0}"

Probleem: ConfigurationArguments avaliku sätteid ja ConfigurationArguments kaitstud sätted sisaldama sama nimega atribuudid.

Lahendus: Eemaldage üks dubleeritud atribuudid.

### <a name="missing-properties"></a>Puuduvad atribuudid
"Configuration.function nõuab on määratud configuration.url või configuration.module"

"Configuration.url nõuab, et configuration.script on määratud"

"Configuration.script nõuab, et configuration.url on määratud"

"Configuration.url nõuab, et configuration.function on määratud"

"ConfigurationUrlSasToken nõuab, et configuration.url on määratud"

"ConfigurationDataUrlSasToken nõuab, et configurationData.url on määratud"

Probleem: Määratletud atribuudi peab teise atribuut, mis on puudu.

Lahendused: 
- Sisestage puuduv atribuut.
- Atribuut, mis tuleb puuduvad atribuudi eemaldada.


## <a name="next-steps"></a>Järgmised sammud
Lisateavet DSC ja virtuaalse masina skaala seab [Abil virtuaalse masina skaala määrab laiendiga Azure'i DSC](../virtual-machine-scale-sets/virtual-machine-scale-sets-dsc.md)

[DSC's turvaline mandaadi haldamine](virtual-machines-windows-extensions-dsc-credentials.md)Täpsem teave. 

Azure'i DSC laiend sündmuseohjuri kohta leiate lisateavet teemast [Azure soovitud riik konfiguratsiooni laiend sündmuseohjuri tutvustus](virtual-machines-windows-extensions-dsc-overview.md). 

PowerShelli DSC, [külastage PowerShelli dokumentatsiooni](https://msdn.microsoft.com/powershell/dsc/overview)kohta lisateavet. 
