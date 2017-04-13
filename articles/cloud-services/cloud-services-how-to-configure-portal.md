<properties 
    pageTitle="Kuidas konfigureerida pilveteenusesse (portaal) | Microsoft Azure'i" 
    description="Saate teada, kuidas konfigureerida Azure pilveteenustega. Siit saate teada, värskendamine pilvepõhise teenuse konfigureerimine ja konfigureerige kaugjuurdepääs rolli aknad. Nendes näidetes kasutatakse Azure portaali." 
    services="cloud-services" 
    documentationCenter="" 
    authors="Thraka" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/11/2016"
    ms.author="adegeo"/>

# <a name="how-to-configure-cloud-services"></a>Kuidas seadistada pilveteenustega

> [AZURE.SELECTOR]
- [Azure'i portaal](cloud-services-how-to-configure-portal.md)
- [Azure'i klassikaline portaal](cloud-services-how-to-configure.md)

Azure'i portaalis saate konfigureerida pilveteenus kõige sagedamini kasutatavad sätted. Või kui soovite värskendada oma failid otse, laadige teenuse konfiguratsioonifail värskendada, ja seejärel värskendatud faili üles laadida ja pilveteenusesse konfiguratsiooni muudatustega värskendada. Mõlemal juhul konfiguratsiooni värskendused on lükata kõik rolli eksemplaridesse.

Saate hallata pilvepõhise teenuse rollid või kaugtöölaua eksemplarid neisse.

Azure'i saate tagada ainult 99,95% teenuse kättesaadavus konfiguratsiooni värskendamise ajal, kui teil on vähemalt kaks rolli eksemplari iga rolli. Mis võimaldab ühe virtuaalse masina töödelda kliendi taotlusi teise värskendamise ajal. Lisateavet leiate teemast [Taseme lepingute](https://azure.microsoft.com/support/legal/sla/).

## <a name="change-a-cloud-service"></a>Pilveteenus muutmine

Pärast avamist [Azure portaali](https://portal.azure.com/), liikuge oma pilveteenuses. Siit saate hallata paljude aspektide. 

![Lehe sätted](./media/cloud-services-how-to-configure-portal/cloud-service.png)

**Sätted** või **Kõik sätted** linkide avab **sätted** blade, kus saate muuta **Atribuudid**, muuta **konfiguratsiooni**, haldamine **serdid**, **teatiste reeglite**häälestamise ja **Kasutajad** , kellel on juurdepääs selle pilveteenuses haldamine.

![Azure'i pilvepõhise teenuse sätted blade](./media/cloud-services-how-to-configure-portal/cs-settings-blade.png)

>[AZURE.NOTE]
>Operatsioonisüsteem, mida kasutatakse pilveteenusesse jaoks ei saa muuta **Azure portaali**, saate muuta ainult selle sätte [Azure klassikaline portaali](http://manage.windowsazure.com/)kaudu. See on üksikasjalikult [siin](cloud-services-how-to-configure.md#update-a-cloud-service-configuration-file).

## <a name="monitoring"></a>Jälgimine

Saate lisada oma pilveteenuses teatised. Klõpsake nuppu **sätted** > **Teatise reeglite** > **Lisa teatis**. 

![](./media/cloud-services-how-to-configure-portal/cs-alerts.png)

Siit saate häälestada teatise. **Mertic** rippmenüüst abil saate häälestada teatise järgmist tüüpi andmeid.

- Ketas lugemine
- Kettale kirjutamine
- Võrgu
- Võrgu välja
- CPU protsent 

![](./media/cloud-services-how-to-configure-portal/cs-alert-item.png)

### <a name="configure-monitoring-from-a-metric-tile"></a>Argumendil paani jälgimiseks konfigureerimine

**Sätete**kasutamise asemel > **Teatiste reegleid**, võite klõpsata ühele argumendil paanid **pilveteenuses** tera jaotises **jälgimine** .

![Pilveteenuses jälgimine](./media/cloud-services-how-to-configure-portal/cs-monitoring.png)

Siit saate kasutada paani diagrammi kohandamine või reegli lisamine.


## <a name="reboot-reimage-or-remote-desktop"></a>Taaskäivitage arvuti, reimage või kaugtöölaua

Praegu ei saa konfigureerida **Azure portaali**Kaugtöölaud. Siiski saate häälestada selle [Azure klassikaline portaali](cloud-services-role-enable-remote-desktop.md), [PowerShelli](cloud-services-role-enable-remote-desktop-powershell.md)või [Visual Studio](../vs-azure-tools-remote-desktop-roles.md)kaudu. 

Klõpsake esmalt cloud eksemplari.

![Pilveteenuse eksemplari](./media/cloud-services-how-to-configure-portal/cs-instance.png)

Keelest, et avatakse uou saab algatada kaugtöölaua ühendus, taaskäivitage kaugühenduse teel eksemplari või kaugühenduse teel reimage (alustada värske pilt) eksemplari.

![Pilveteenuse teenuse eksemplari nupud](./media/cloud-services-how-to-configure-portal/cs-instance-buttons.png)



## <a name="reconfigure-your-cscfg"></a>Konfigureerige oma .cscfg

Võimalik, et konfigureerida te peate teenus cloud [teenuse config (cscfg)](cloud-services-model-and-package.md#cscfg) faili kaudu. Esmalt peate oma .cscfg faili alla laadida, muuta seda, siis laadige see.

1. Klõpsake menüü **sätted** ikoon või **Kõik sätted** linki avada **sätted** tera.

    ![Lehe sätted](./media/cloud-services-how-to-configure-portal/cloud-service.png)

2. Klõpsake üksuse **konfigureerimine** .

    ![Konfiguratsiooni Blade](./media/cloud-services-how-to-configure-portal/cs-settings-config.png)

3. Klõpsake nuppu **Laadi alla** .

    ![Laadi alla](./media/cloud-services-how-to-configure-portal/cs-settings-config-panel-download.png)

4. Pärast värskendamist teenuse konfiguratsioonifail, üles laadida ja konfiguratsiooni värskendused.

    ![Laadi üles](./media/cloud-services-how-to-configure-portal/cs-settings-config-panel-upload.png) 
    
5. Valige .cscfg fail ja klõpsake nuppu **OK**.

            
## <a name="next-steps"></a>Järgmised sammud

* Siit saate teada, juurutamise [mõnda pilveteenusesse](cloud-services-how-to-create-deploy-portal.md).
* Saate konfigureerida [kohandatud domeeni nime](cloud-services-custom-domain-name-portal.md).
* [Halda oma pilveteenuses](cloud-services-how-to-manage-portal.md).
* [Ssl-sertide](cloud-services-configure-ssl-certificate-portal.md)konfigureerimine.