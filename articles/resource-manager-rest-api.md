<properties
   pageTitle="Ressursihaldur REST API-de | Microsoft Azure'i"
   description="Ressursihaldur REST API-de autentimis- ja kasutamise näited ülevaade"
   services="azure-resource-manager"
   documentationCenter="na"
   authors="navalev"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/23/2016"
   ms.author="navale;tomfitz;"/>
   
# <a name="resource-manager-rest-apis"></a>Ressursihaldur REST API-d

> [AZURE.SELECTOR]
- [Azure'i PowerShelli](powershell-azure-resource-manager.md)
- [Azure'i CLI](xplat-cli-azure-resource-manager.md)
- [Portaal](./azure-portal/resource-group-portal.md) 
- [REST API-GA](resource-manager-rest-api.md)

Iga kõne Azure'i ressursihaldur taha iga juurutatud malli taga taga iga konfigureeritud salvestusruumi konto on ühe või mitme Azure'i ressursihaldur rahulik API kõned. Selles teemas on eraldatud nende API-d ja kuidas saate helistada neid üldse mis tahes SDK kasutamata. See võib olla väga kasulik, kui soovite, et kõik kutsed Azure'i või kui SDK oma eelistatud keele jaoks pole saadaval või ei toeta toiminguid sooritada täielikud õigused.

See artikkel ei lähe läbi iga API, mis on esitatud Azure, kuid kasutab pigem mõni näide selle kohta, kuidas minna ja ühendada need. Kui teil on põhialuste saate minna ja lugege üksikasjalikku teavet selle kohta, kuidas kasutada funktsiooni API-de ülejäänud [Azure'i ressursihaldur REST API viide](https://msdn.microsoft.com/library/azure/dn790568.aspx) .

## <a name="authentication"></a>Autentimine
ARM autentimise töödeldakse, Azure Active Directory (AD). Selleks, et ühenduse mis tahes API, peate esmalt autentimiseks Azure AD saada mõne autentimise luba, mida te võite edasi iga taotluse. Nagu me mis kirjeldab puhas kõne otse REST API-de me eeldab ka, et te ei soovi autentida, kus pop-up-üles-ekraan võib teilt kasutajanime ja parooli ja isegi teiste autentimist menetlustele kasutatakse kahte tegurit autentimise stsenaariumid parooliga tavaline kasutajanimi. Seetõttu loome, mida nimetatakse Azure AD Rakenduse ja teenuse põhilise, mida kasutatakse sisse logima. Kuid pidage meeles, et Azure AD toetab mitut autentimise toimingute ja kõik need saab tuua selle autentimise luba, mida läheb vaja hilisemaks API taotlused.
Järgige [loomine Azure AD rakenduste ja teenuste põhimõtet](./resource-group-create-service-principal-portal.md) samm-sammult juhised.

### <a name="generating-an-access-token"></a>Juurdepääsu sümboolse genereerimine 
Autentimise vastu Azure AD tehakse Azure AD, tavatelefoninumbritel asub login.microsoftonline.com. Selleks, et teie autentimiseks peab olema järgmine teave:

* Azure AD rentniku ID (nimi selle Azure AD kasutate login sageli sama teie ettevõttega, kuid ei ole vajalik)
* Rakenduse ID (võetud sammu Azure AD Rakenduse loomise ajal)
* Parooli (valitud Azure AD Rakenduse loomisel)

Klõpsake selle all HTTP-päring veenduge, et "Azure AD rentniku ID", "Rakenduste ID" ja "Parooli" õigete väärtuste asendamine.

**Üldise HTTP-päring:**

```HTTP
POST /<Azure AD Tenant ID>/oauth2/token?api-version=1.0 HTTP/1.1 HTTP/1.1
Host: login.microsoftonline.com
Cache-Control: no-cache
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&resource=https%3A%2F%2Fmanagement.core.windows.net%2F&client_id=<Application ID>&client_secret=<Password>
```

… (kui autentimine õnnestus) tulemuseks sarnased vastuseks:

```json
{
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1448199959",
  "not_before": "1448196059",
  "resource": "https://management.core.windows.net/",
  "access_token": "eyJ0eXAiOiJKV1QiLCJhb...86U3JI_0InPUk_lZqWvKiEWsayA"
}
```
(Access_token ülaltoodud vastuseks on suurendamiseks loetavuse lühendatud)

**Genereerimine juurdepääsu luba Bash abil:**

```console
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "grant_type=client_credentials&resource=https://management.core.windows.net&client_id=<application id>&client_secret=<password you selected for authentication>" https://login.microsoftonline.com/<Azure AD Tenant ID>/oauth2/token?api-version=1.0
```

**Genereerimine PowerShelli kaudu juurdepääsu luba:**

```powershell
Invoke-RestMethod -Uri https://login.microsoftonline.com/<Azure AD Tenant ID>/oauth2/token?api-version=1.0 -Method Post
 -Body @{"grant_type" = "client_credentials"; "resource" = "https://management.core.windows.net/"; "client_id" = "<application id>"; "client_secret" = "<password you selected for authentication>" }
```

Vastus sisaldab mõnda Accessi Turbeloa, teavet kaua, seega on kehtiv ja teavet, mis ressursi abil saate selle luba.
Olete saanud eelmine HTTP kõne juurdepääsu luba edastada tuleb sisse kõigi taotlus ARM API päisena nimega "Autoriseerimine" väärtusega "Esitaja YOUR_ACCESS_TOKEN". Teade "Esitaja" ja Accessi Turbeloa vahele ruumi.

Nagu näete ülaltoodud HTTP tulem, luba kehtib teatud aja jooksul peaks vahemälu ja uuesti kasutada seda Samamoodi. Isegi juhul, kui see on võimalik autentimiseks Azure AD iga API kõne, on väga ebaefektiivne.

## <a name="calling-arm-rest-apis"></a>Helistaja ARM REST API-d

[Azure'i ressursihaldur REST API -d on avaldatud siin](https://msdn.microsoft.com/library/azure/dn790568.aspx) ja see on välja ulatuse selle õpetuse dokumendikomplekti iga kasutamist. Dokumentide kasutab ainult mõne API-de põhilised selgitada selle API-de ja pärast seda me teie viidata ametlike dokumentide.

### <a name="list-all-subscriptions"></a>Kõigi tellimuste loend

Lihtsaim toiminguid saate teha ühte on loendis Saadaolevad tellimused, millele pääsete juurde. Klõpsake selle all taotluse näete, kuidas Accessi Turbeloa edastatakse päisena.

(Asendage YOUR_ACCESS_TOKEN oma tegeliku juurdepääsu Turbeloa.)

```HTTP
GET /subscriptions?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json
```

… ja seetõttu, saate selle teenuse põhilise on lubatud juurdepääs tellimuste loendi

(Tellimuse ID allpool on loetavuse lühendatud)

```json
{
  "value": [
    {
      "id": "/subscriptions/3a8555...555995",
      "subscriptionId": "3a8555...555995",
      "displayName": "Azure Subscription",
      "state": "Enabled",
      "subscriptionPolicies": {
        "locationPlacementId": "Internal_2014-09-01",
        "quotaId": "Internal_2014-09-01"
      }
    }
  ]
}
```

### <a name="list-all-resource-groups-in-a-specific-subscription"></a>Loetle kõik ressursi rühmad teatud tellimus

Arm API-de kõik ressursid on pesastatud ressursirühma. Me päringu ARM olemasoleva ressursi rühmade meie tellimuse kasutamise selle all HTTP GET-päring. Pange tähele, kuidas Tellimuse ID edastatakse URL-i osana seekord.

(Asendage YOUR_ACCESS_TOKEN ja SUBSCRIPTION_ID oma tegeliku juurdepääsu sümboolse ja Tellimuse ID)

```HTTP
GET /subscriptions/SUBSCRIPTION_ID/resourcegroups?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json
```

Saate vastus sõltub sellest, kas teil on mis tahes ressursi rühmad, mis on määratletud kui jah, kui palju.

(Tellimuse ID allpool on loetavuse lühendatud)

```json
{
    "value": [
        {
            "id": "/subscriptions/3a8555...555995/resourceGroups/myfirstresourcegroup",
            "name": "myfirstresourcegroup",
            "location": "eastus",
            "properties": {
                "provisioningState": "Succeeded"
            }
        },
        {
            "id": "/subscriptions/3a8555...555995/resourceGroups/mysecondresourcegroup",
            "name": "mysecondresourcegroup",
            "location": "northeurope",
            "tags": {
                "tagname1": "My first tag"
            },
            "properties": {
                "provisioningState": "Succeeded"
            }
        }
    ]
}
```

### <a name="create-a-resource-group"></a>Ressursi rühma loomine

Siiani me olete ainult antud päringute ARM API-de teavet, aeg loome materjale ja ressursse, selle asemel ja Alustame lihtsaim need kõik, ressursirühma. Järgmised HTTP-päringu loob uue ressursirühma teie valitud piirkond/asukohta ja üks või mitu lisatakse (proovi all tegelikult ainult lisab ühe sildi).

(Asendage YOUR_ACCESS_TOKEN, SUBSCRIPTION_ID, RESOURCE_GROUP_NAME oma tegeliku juurdepääsu Turbeloa, tellimuse ID ja soovite luua ressursirühma nimi)

```HTTP
PUT /subscriptions/SUBSCRIPTION_ID/resourcegroups/RESOURCE_GROUP_NAME?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json

{
  "location": "northeurope",
  "tags": {
    "tagname1": "test-tag"
  }
}
```

Kui õnnestub, kuvatakse sarnaselt reageerimine

```json
{
  "id": "/subscriptions/3a8555...555995/resourceGroups/RESOURCE_GROUP_NAME",
  "name": "RESOURCE_GROUP_NAME",
  "location": "northeurope",
  "tags": {
    "tagname1": "test-tag"
  },
  "properties": {
    "provisioningState": "Succeeded"
  }
}
```

Teie loodud edukalt ressursirühma Azure. Palju õnne!

### <a name="deploy-resources-to-a-resource-group-using-an-arm-template"></a>Ressursside juurutama ressursirühma on ARM malli abil

Arm, saate juurutada oma ressursse ARM mallide kasutamine. Mõni ARM mall määratleb mitu materjale ja nende sõltuvusi. Selles jaotises me just endale, on teile tuttavad ARM Mallid ja lihtsalt näitame teile helistada API juurutamise ühe alustamiseks tehke järgmist. Üksikasjalikud dokumendid ARM malle leiate siit.

Juurutamise ARM malli ei erine palju kui helistate muude API-d. Üks oluline on, et malli kasutuselevõttu võib võtta päris palju aega, sõltuvalt sellest, mis on sees Mall, ja API kõne lihtsalt tagastavad ja see on teile arendaja päringusse juurutamise oleku selleks, et teada saada, kui juurutamise on lõpule jõudnud.

Selle näite puhul kasutame avalikult exposed ARM malli saadaval [github](https://github.com/Azure/azure-quickstart-templates). Mall on meil kuvatakse juurutada Linux VM Lääne USA piirkonnas. Kuigi see mall on saadaval malli avaliku hoidlas, nt GitHub, võite valida ka läbida täielik malli taotluse osana. Pange tähele, et pakume parameetrite väärtused sees kasutatud malli kasutatud taotluse osana.

(SUBSCRIPTION_ID, RESOURCE_GROUP_NAME, DEPLOYMENT_NAME, YOUR_ACCESS_TOKEN, GLOBALY_UNIQUE_STORAGE_ACCOUNT_NAME, ADMIN_USER_NAME, ADMIN_PASSWORD ja DNS_NAME_FOR_PUBLIC_IP asendamine väärtustega, mis on asjakohane teie taotlus)

```HTTP
PUT /subscriptions/SUBSCRIPTION_ID/resourcegroups/RESOURCE_GROUP_NAME/providers/microsoft.resources/deployments/DEPLOYMENT_NAME?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json

{
  "properties": {
    "templateLink": {
      "uri": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-simple-linux-vm/azuredeploy.json",
      "contentVersion": "1.0.0.0",
    },
    "mode": "Incremental",
    "parameters": {
        "newStorageAccountName": {
          "value": "GLOBALY_UNIQUE_STORAGE_ACCOUNT_NAME"
        },
        "adminUsername": {
          "value": "ADMIN_USER_NAME"
        },
        "adminPassword": {
          "value": "ADMIN_PASSWORD"
        },
        "dnsNameForPublicIP": {
          "value": "DNS_NAME_FOR_PUBLIC_IP"
        },
        "ubuntuOSVersion": {
          "value": "15.04"
        }
    }
  }
}
```

Dokumentide loetavuse parandamiseks on välja jäetud üsna pikk JSON vastus selle päringu. Vastus sisaldab teavet äsja loodud templated juurutamise kohta.

