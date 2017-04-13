<properties
    pageTitle="Migreerimine rakenduste loogika skeemi versioon 2015-08-01-eelvaade | Microsoft Azure'i rakendust Service"
    description="Loogika rakendusi saate migreerida hõlpsalt skeemi uusim versioon. Järgige neid juhiseid."
    services="logic-apps"
    documentationCenter=""
    authors="MSFTMAN"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/23/2016"
    ms.author="deonhe"/>

# <a name="how-to-migrate-logic-apps-to-schema-version-2015-08-01-preview"></a>Kuidas migreerida loogika rakenduste skeemi versioon 2015-08-01-eelvaade

Uue skeemi olemasoleva loogika rakenduste teisaldamiseks tehke järgmist.  
1. Avage rakenduse loogika Azure'i portaalis  
2. Klõpsake nuppu Värskenda skeem:

 ![API ikoon][step1]   
Lehe Update skeemi kuvab ja pakub lingi dokumendile üksikasjad uue skeemi parandusi: ![API ikoon][step2]

>[AZURE.NOTE] **Värskenduse skeemi**valimisel me automaatselt käivitada migreerimise juhiseid ja sisestage kood väljundi teile. Saate selle definitsiooni, kuid värskendama, veenduge, et järgite kodeerimine heade tavade, nagu on kirjeldatud allpool jaotises **head tavad** .

## <a name="best-practices-when-migrating-your-logic-apps-to-the-latest-schema-version"></a>Head tavad kui migreerimine rakenduste loogika skeemi uusim versioon:  

- Uue loogika rakenduse migreeritud skripti kopeerimiseks – ei vanade üks seni, kuni olete lõpetanud oma testimine ja kinnitanud, et migreeritud rakendus töötab ootuspäraselt.
- Testige oma loogika rakenduse **enne** valmistamisel
- Pärast migreerimise lõpuleviimist, käivitage värskendamine loogika rakendusi [hallatav API-de](./apis-list.md) kasutamise võimaluse. Näiteks saate hakata Dropboxi v2, kui kasutate Dropboxi v1.


## <a name="whats-next"></a>Mis saab edasi
-  [Saate teada, kuidas käsitsi migreerimine rakenduste loogika](../app-service-logic/app-service-logic-schema-2015-08-01.md)


<!--Icon references-->
[step1]: ./media/connectors-schema-migration/migrateschema1.png
[step2]: ./media/connectors-schema-migration/migrateschema2.png






