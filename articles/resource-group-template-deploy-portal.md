<properties 
    pageTitle="Azure'i portaali abil saate juurutada Azure ressursid | Microsoft Azure'i" 
    description="Kasutage oma ressursse Azure portaali ja Azure ressursside haldamine." 
    services="azure-resource-manager,azure-portal" 
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
    ms.date="09/15/2016" 
    ms.author="tomfitz"/>

# <a name="deploy-resources-with-resource-manager-templates-and-azure-portal"></a>Ressursside ressursihaldur mallide ja Azure portaali juurutamine

> [AZURE.SELECTOR]
- [PowerShelli](resource-group-template-deploy.md)
- [Azure'i CLI](resource-group-template-deploy-cli.md)
- [Portaal](resource-group-template-deploy-portal.md)
- [REST API-GA](resource-group-template-deploy-rest.md)

Selles teemas kirjeldatakse, kuidas kasutada [Azure portaali](https://portal.azure.com) [Azure'i ressursihaldur](azure-resource-manager/resource-group-overview.md) juurutada oma Azure ressursse. Oma ressursse haldamise kohta leiate teemast [Azure haldamine ressursid portaali kaudu](./azure-portal/resource-group-portal.md).

Praegu pole igal teenusel toetab portaali või ressursihaldur. Nende teenuste jaoks peate kasutama [klassikaline portaalis](https://manage.windowsazure.com). Iga teenuse olekut leiate [Azure'i portaali-saadavus diagrammi](https://azure.microsoft.com/features/azure-portal/availability/).

## <a name="create-resource-group"></a>Ressursi rühma loomine

1. Ka tühjad ressursikeskuse rühma loomiseks valige **Uus** > **halduse** > **Ressursirühma**.

    ![tühjad ressursikeskuse rühma loomine](./media/resource-group-template-deploy-portal/create-empty-group.png)

2. Anda sellele nimi ja asukoht ja vajaduse korral valige tellimus. Tuleb sisestada ressursirühma asukoht, kuna ressursirühma talletab metaandmete ressursside kohta. Nõuetele vastavuse tagamiseks, võite määrata, et metaandmete talletuskoht. Soovitame üldiselt, määrake asukoht, kus enamik oma ressursse hakkavad asuma. Samas kohas abil saate lihtsustada malli.

    ![rühma väärtused](./media/resource-group-template-deploy-portal/set-group-properties.png)

## <a name="deploy-resources-from-marketplace"></a>Ressursse turuplatsilt juurutamine

Kui olete loonud ressursirühma, saate juurutada ressursid selle turuplatsilt. Kuvatakse Marketplace'ist pakub eelmääratletud lahendusi levinud stsenaariumi.

1. Juurutamise alustamiseks valige **Uus** ja tüüp ressurss, mida soovite kasutada. Seejärel otsige kindla versiooni ressurss, mida soovite kasutada.

    ![ressursi juurutamine](./media/resource-group-template-deploy-portal/deploy-resource.png)

2. Kui teile ei kuvata teatud lahenduse juurutamine soovite, saate seda otsida kuvatakse Marketplace'ist.

    ![Otsingu turuplats](./media/resource-group-template-deploy-portal/search-resource.png)

3. Sõltuvalt valitud ressursi, on oluline atribuutide seadmine enne juurutamist kogumik. Need suvandid on näidatud siin, nagu need sõltuvad ressursi tüüp. Kõigi tüüpide, valige sihtkoht ressursirühma. Järgmisel pildil on kujutatud, kuidas luua veebirakenduse ja juurutama loodud ressursirühma.

    ![ressursi rühma loomine](./media/resource-group-template-deploy-portal/select-existing-group.png)

    Teise võimalusena saate otsustada, kui teie ressursse rakendades ressursirühma loomiseks. Valige **Loo uus** ja tippige selle nimi.

    ![uue ressursirühma loomine](./media/resource-group-template-deploy-portal/select-new-group.png)

4. Juurutamise hakkab. Juurutamise võib kuluda mõni minut. Kui juurutamise on lõpule jõudnud, kuvatakse teade.

    ![Kuva teatis](./media/resource-group-template-deploy-portal/view-notification.png)

5. Pärast teie ressursse rakendades, saate lisada veel ressursse ressursirühma ressursi rühma enne käsu **Lisa** .

    ![ressursi lisamine](./media/resource-group-template-deploy-portal/add-resource.png)

## <a name="deploy-resources-from-custom-template"></a>Ressursside kohandatud malli juurutamine

Kui soovite täide viia, kuid mitte kasutada kõiki malle kuvatakse Marketplace'ist, saate luua kohandatud malli, mis määratleb infrastruktuuri teie lahendus. Mallide loomise kohta leiate lisateavet teemast [Azure ressursihaldur loome Mallid](resource-group-authoring-templates.md).

1. Juurutada kohandatud malli portaali kaudu, valige **Uus**ja otsima **Malli juurutamise** seni, kuni saate valida soovitud suvandid.

    ![Otsingu malli juurutamine](./media/resource-group-template-deploy-portal/search-template.png)

2. Valige **Mall juurutamise** saadaval ressursse.

    ![Valige mall juurutamine](./media/resource-group-template-deploy-portal/select-template.png)

3. Pärast käivitamist malli juurutus, avage tühi Mall, mis on saadaval kohandamine.

    ![malli loomine](./media/resource-group-template-deploy-portal/show-custom-template.png)

    Lisage redaktoris JSON süntaks, mis määratleb ressursid, mida soovite kasutada. Valige **Salvesta** , kui valmis. Juhised kirjutamise JSON süntaksit, leiate teemast [ressursihaldur Mall ülevaade](resource-manager-template-walkthrough.md).

    ![malli redigeerimine](./media/resource-group-template-deploy-portal/edit-template.png)

4. Või saate valida olemasoleva malli [Azure Kiirjuhend mallide](https://azure.microsoft.com/documentation/templates/)põhjal. Need mallid on toetada ühenduse. Need hõlmavad mitme levinud stsenaariumi ja keegi lisatud malli, mis on sarnane, mida soovite kasutada. Leida midagi, mis vastab teie stsenaariumi malle saate otsida.

    ![Valige mall Kiirjuhend](./media/resource-group-template-deploy-portal/select-quickstart-template.png)

    Saate vaadata valitud mall redaktoris.

5. Pärast kõik muud väärtused, valige malli **loomine** . 

    ![malli juurutamine](./media/resource-group-template-deploy-portal/create-custom-deploy.png)

## <a name="deploy-resources-from-a-template-saved-to-your-account"></a>Teie kontol salvestatud mallist ressursid juurutamine

Portaalis saab salvestada mallina Azure'i kontosse ja Juurutage hiljem uuesti. Lisateabe saamiseks nende salvestatud Mallid, [alustage isiklike mallide Azure'i portaalis](./marketplace-consumer/mytemplates-getstarted.md)töötamise kohta.

1. Salvestatud mallide otsimiseks valige **Sirvi** > **Mallid**.

    ![Saate valida malli](./media/resource-group-template-deploy-portal/browse-templates.png)

2. Valige loendist oma kontol salvestatud malli soovite töötada.

    ![Salvestatud Mallid](./media/resource-group-template-deploy-portal/saved-templates.png)

3. Valige **Deploy** ümberkorraldamine salvestatud malli abil.

    ![Salvestatud malli juurutamine](./media/resource-group-template-deploy-portal/deploy-saved-template.png)

## <a name="next-steps"></a>Järgmised sammud

- Auditilogide vaatamiseks lugege teemat [auditi toimingud koos ressursihaldur](resource-group-audit.md).
- Juurutamise tõrkeotsing, leiate [tõrkeotsingu ressursside rühma juurutuste Azure'i portaalis](resource-manager-troubleshoot-deployments-portal.md).
- Malli toomiseks juurutamine või ressursirühm teemast [Azure ressursihaldur eksportimine malli olemasoleva ressursid](resource-manager-export-template.md).





