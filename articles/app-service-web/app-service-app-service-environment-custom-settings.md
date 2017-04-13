<properties
    pageTitle="Kohandatud sätteid rakenduse teenuse keskkonnas"
    description="Kohandatud Otsingukonfiguratsiooni sätetest rakenduse teenuse keskkonnas"
    services="app-service"
    documentationCenter=""
    authors="stefsch"
    manager="nirma"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="stefsch"/>

# <a name="custom-configuration-settings-for-app-service-environments"></a>Kohandatud Otsingukonfiguratsiooni sätetest rakenduse teenuse keskkonnas

## <a name="overview"></a>Ülevaade ##
Kuna rakendus teenuse keskkonnas on eraldatud ühekordse kliendile, on teatud konfiguratsioonisätted, mis saab rakendada ainult rakenduse teenuse keskkonnas. See artikkel dokumendid teatud kohandused, on saadaval rakenduse teenuse keskkonnas.

Kui teil pole rakenduse teenuse keskkonnas, vaadake, [Kuidas luua rakenduse teenuse keskkond](app-service-web-how-to-create-an-app-service-environment.md).

Saate salvestada rakenduse teenuse keskkonna kohanduste abil massiivi **clusterSettings** uus atribuut. See atribuut on sõnastikus "Atribuudid", *hostingEnvironments* Azure ressursihaldur üksus.

Lühendatud ressursihaldur malli koodilõigu kuvatud **clusterSettings** atribuuti.


    "resources": [
    {
       "apiVersion": "2015-08-01",
       "type": "Microsoft.Web/hostingEnvironments",
       "name": ...,
       "location": ...,
       "properties": {
          "clusterSettings": [
             {
                 "name": "nameOfCustomSetting",
                 "value": "valueOfCustomSetting"
             }
          ],
          "workerPools": [ ...],
          etc...
       }
    }

Atribuut **clusterSettings** saab lisada ressursihaldur malli värskendamiseks rakenduse teenuse keskkonnas.

## <a name="use-azure-resource-explorer-to-update-an-app-service-environment"></a>Azure'i ressursi Exploreri abil värskendada rakenduse teenuse keskkonnas
Teise võimalusena saate värskendada rakenduse teenuse keskkonna [Azure'i ressursi Exploreri](https://resources.azure.com)kaudu.  

1. Ressursi Exploreris valige sõlm rakenduse teenuse keskkonnas (**tellimuste** > **resourceGroups** > **pakkujate** > **Micrososft.Web** > **hostingEnvironments**). Klõpsake kindla rakenduse teenuse keskkonnas, mida soovite värskendada.

2. Klõpsake parempoolsel paanil **Lugemis-ja kirjutamisõigusega** ülemise tööriistaribal interaktiivsed Resource Explorer redigeerimise lubamiseks.  

3. Seejärel sinine **redigeerimine** nuppu teha ressursihaldur malli redigeerida.

4. Liikuge kerides Parempoolse paani allservas. Atribuut **clusterSettings** on väga alt, kuhu saate sisestada või värskendada väärtusega.

5. Sisestage (või kopeerige ja kleepige) soovitud atribuuti **clusterSettings** konfiguratsiooni väärtuste massiivi.  

6. Klõpsake rohelise **sellele** nuppu, mis asub muudatuse kinnitamiseks rakenduse teenuse keskkonna Parempoolse paani ülaosas.

Kuid muudatuste saatmiseks, kulub esi lõpetatakse rakenduse teenuse keskkonnas muudatuse jõustumiseks arvu korrutatuna umbes 30 minutit.
Näiteks kui rakenduse teenuse keskkond on nelja esi lõpetatakse, kulub umbes kahe tunni konfiguratsiooni värskenduse lõpuleviimiseks. Samal ajal võetakse kasutusele konfiguratsiooni muuta, pole muude skaleerimise või konfiguratsiooni muutmine võib toimuda rakenduse teenuse keskkonnas.

## <a name="disable-tls-10"></a>TLS 1.0 keelamine ##
Klientide korduva küsimus, eriti klientidele, kes on tegemist PCI täitmine auditid, on konkreetselt oma rakenduste keelamine TLS 1.0.

TLS 1.0 saab keelata kaudu **clusterSettings** järgmine kirje:

        "clusterSettings": [
            {
                "name": "DisableTls1.0",
                "value": "1"
            }
        ],

## <a name="change-tls-cipher-suite-order"></a>TLS šifri komplekti järjestuse muutmine ##
Teine küsimus klientide on kui ta saab muuta oma serveri üle šifrid loendit ja selle saavutamiseks **clusterSettings** muutes, nagu allpool näidatud. Saadaval šifrikomplektid loendit saab tuua [MSDN-i artiklis] (https://msdn.microsoft.com/library/windows/desktop/aa374757(v=vs.85\).aspx).

        "clusterSettings": [
            {
                "name": "FrontEndSSLCipherSuiteOrder",
                "value": "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384_P256,TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256_P256,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384_P256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256_P256,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA_P256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA_P256"
            }
        ],

> [AZURE.WARNING]  Kui valede väärtuste šifri komplekti, mis SChannel ei saa aru, kõik TLS side oma serveri võivad jõudlust ja tööd. Sellisel juhul peate *FrontEndSSLCipherSuiteOrder* kirje eemaldamine **clusterSettings** ja esitage värskendatud ressursihaldur Mall, mida soovite naasta šifri komplekti vaikesätted.  Kasutage seda funktsiooni ettevaatusega.

## <a name="get-started"></a>Alustamine
Azure'i Kiirjuhend ressursihaldur saidi Mall sisaldab base definitsiooni [rakenduse teenuse keskkonna loomine](https://azure.microsoft.com/documentation/templates/201-web-app-ase-create/)malli.


<!-- LINKS -->

<!-- IMAGES -->
