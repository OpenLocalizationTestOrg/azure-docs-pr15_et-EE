<properties 
    pageTitle="Kuidas konfigureerida mõnda pilveteenusesse (klassikaline portaal) | Microsoft Azure'i" 
    description="Saate teada, kuidas konfigureerida Azure pilveteenustega. Siit saate teada, värskendamine pilvepõhise teenuse konfigureerimine ja konfigureerige kaugjuurdepääs rolli aknad." 
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




# <a name="how-to-configure-cloud-services"></a>Pilveteenustega konfigureerimine

> [AZURE.SELECTOR]
- [Azure'i portaal](cloud-services-how-to-configure-portal.md)
- [Azure'i klassikaline portaal](cloud-services-how-to-configure.md)

Saate konfigureerida Azure klassikaline portaalis pilveteenus kõige sagedamini kasutatavad sätted. Või kui soovite värskendada oma failid otse, laadige teenuse konfiguratsioonifail värskendada, ja seejärel värskendatud faili üles laadida ja pilveteenusesse konfiguratsiooni muudatustega värskendada. Mõlemal juhul konfiguratsiooni värskendused on lükata kõik rolli eksemplaridesse.

Azure'i klassikaline portaali võimaldab teil lubada [Kaugtöölaua ühendus rollist Azure'i pilveteenustega](cloud-services-role-enable-remote-desktop.md)

Azure'i saate tagada ainult 99,95% teenuse kättesaadavus konfiguratsiooni värskendamise ajal, kui teil on vähemalt kaks rolli eksemplari iga rolli. Mis võimaldab ühe virtuaalse masina töödelda kliendi taotlusi teise värskendamise ajal. Lisateavet leiate teemast [Taseme lepingute](https://azure.microsoft.com/support/legal/sla/).

## <a name="change-a-cloud-service"></a>Pilveteenus muutmine

1. [Azure'i klassikaline portaali](http://manage.windowsazure.com/), klõpsake **Pilveteenustega**, klõpsake selle nime pilveteenusesse, ja klõpsake nuppu **Konfigureeri**.

    ![Lehel konfigureerimine](./media/cloud-services-how-to-configure/CloudServices_ConfigurePage1.png)
    
    Lehe **konfigureerimine** saate konfigureerida jälgimine, rolli sätete värskendamine ja valige Külastajate operatsioonisüsteemi ja perekonna rollirühma eksemplaride. 

2. **Jälgimine**, määrata jälgimisega seotud Verbose või minimaalsete ja konfigureerida Paljusõnaline jälgimiseks vajalike diagnostika ühendusstringi.

3. Teenuse rollide (rolli alusel rühmitatud), saate värskendada järgmised sätted:
    
    >**Sätted**  
    >Saate muuta väärtuste mitmesugused konfiguratsioonisätted, mis on määratud *ConfigurationSettings* elementide teenuse konfiguratsioonifail (.cscfg).
    >
    >**Serdid**  
    >Saate muuta sõrmejälje sert, mida kasutatakse SSL-krüptimist rolli. Serdi muutmiseks tuleb esmalt uut serti (lehel **serdid** ) üles laadida. Seejärel värskendage sõrmejälje rolli sätted kuvatakse serdi string.

4. **Operatsioonisüsteem**, saate muuta operatsioonisüsteemi pere või rolli eksemplaride versiooni või valida **automaatse** luba automaatne uuendamine praegune operatsioonisüsteemi versiooni. Operatsioonisüsteemi sätete rakendada web rollid ja töötaja rollid, kuid ei mõjuta Virtuaalmasinates.

    Juurutamisel, operatsioonisüsteemi kõige uuem versioon on installitud kõik rolli aknad ja opsüsteemide värskendatakse automaatselt vaikimisi. 
    
    Kui teil on vaja operatsioonisüsteemi versiooni tõttu ühilduvusnõuded koodi käivitada oma pilveteenuses, saate valida mõne operatsioonisüsteemi pere ja versioon. Kui valite kindla operatsioonisüsteemi versiooni, peatatakse automaatse operatsioonisüsteemi värskendusi pilveteenusesse. Peate tagada opsüsteemide värskendusi.
    
    Kui kõik ühilduvusega seotud probleemid, mis teie rakendused on viimase operatsioonisüsteemi versiooni lahendamiseks saate lubada automaatse operatsioonisüsteemi värskenduste määrates operatsioonisüsteemi versiooni **Automaatne**. 
    
    ![OS sätted](./media/cloud-services-how-to-configure/CloudServices_ConfigurePage_OSSettings.png)

5. Konfiguratsiooni sätete salvestamiseks ja murravad rolli aknad, klõpsake nuppu **Salvesta**. (Klõpsake tühistamiseks muudatuste **hülgamine** .) **Salvestamine** ja **Hülga** lisatakse käsuriba pärast mõnda sätet muuta.

## <a name="update-a-cloud-service-configuration-file"></a>Värskendus konfiguratsioonifail teenus cloud

1. Laadige pilvepõhise teenuse konfigureerimine faili (.cscfg) praegune konfiguratsioon. Klõpsake lehel **konfigureerimine** pilveteenusesse jaoks **alla laadida**. Seejärel klõpsake nuppu **Salvesta**või **Salvesta nimega** faili salvestamiseks klõpsake nuppu.

2. Pärast värskendamist teenuse konfiguratsioonifail, üles laadida ja värskendused konfigureerimine.

    1. Klõpsake lehe **konfigureerimine** **üles laadida**.
    
        ![Laadige konfigureerimine](./media/cloud-services-how-to-configure/CloudServices_UploadConfigFile.png)
    
    2. **Konfiguratsioonifail**, **sirvige** valimiseks kasutage värskendatud .cscfg faili.
    
    3. Kui teie pilveteenuses sisaldab rollide, mis sisaldavad ainult ühte eksemplari, märkige ruut **konfiguratsiooni rakendada ka siis, kui üks või mitu rollid sisaldavad ühte eksemplari** , rollide jätkamiseks konfiguratsiooni värskendused.
    
        Kui määrate vähemalt kaks eksemplari iga rolli, Azure ei saa tagada vähemalt 99,95% teie pilveteenuses kättesaadavus teenusevärskendused konfigureerimise käigus. Lisateavet leiate teemast [Taseme lepingute](https://azure.microsoft.com/support/legal/sla/).
    
    4. Klõpsake nuppu **OK** (märge). 


## <a name="next-steps"></a>Järgmised sammud

* Siit saate teada, juurutamise [mõnda pilveteenusesse](cloud-services-how-to-create-deploy.md).
* Saate konfigureerida [kohandatud domeeni nime](cloud-services-custom-domain-name.md).
* [Halda oma pilveteenuses](cloud-services-how-to-manage.md).
* [Kaugtöölaua ühendus rollist Azure pilveteenustega lubamine](cloud-services-role-enable-remote-desktop.md)
* [Ssl-sertide](cloud-services-configure-ssl-certificate.md)konfigureerimine.
