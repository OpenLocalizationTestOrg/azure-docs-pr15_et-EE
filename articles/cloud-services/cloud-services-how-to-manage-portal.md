<properties 
    pageTitle="Levinud cloud teenuste haldamisega seotud toiminguid | Microsoft Azure'i" 
    description="Saate teada, kuidas hallata pilveteenustega Azure'i portaalis. Nendes näidetes kasutatakse Azure portaali." 
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
    ms.date="08/02/2016"
    ms.author="adegeo"/>


# <a name="how-to-manage-cloud-services"></a>Kuidas hallata pilveteenustega

> [AZURE.SELECTOR]
- [Azure'i portaal](cloud-services-how-to-manage-portal.md)
- [Azure'i klassikaline portaal](cloud-services-how-to-manage.md)

Oma pilveteenuses hallatakse Azure portaali alal **Pilveteenustega (klassikaline)** . Selles artiklis kirjeldatakse mõned levinud toiminguid, mida soovite teha oma pilveteenustega hallates saate ühtlasi. Mis sisaldab värskendamine, kustutamine, suurust ja edendada tootmise etapiviisilise juurutamine.

Lisateavet selle kohta, kuidas oma pilveteenuses skaala [siin](cloud-services-how-to-scale-portal.md).

## <a name="how-to-update-a-cloud-service-role-or-deployment"></a>Kuidas: värskendamine pilvepõhise teenuse rolli või juurutamine

Kui peate värskendama oma pilveteenuses rakenduse kood, kasutage pilvepõhise teenuse enne **värskendada** . Saate värskendada kindla rolliga või kõikide rollide. Värskendamiseks saate uue teenuse paketi või teenuse konfiguratsiooni faili üles laadida.

1. [Azure'i portaalis][]valige pilveteenuses, mida soovite värskendada. Selles etapis tuleb avatakse pilvepõhise teenuse eksemplari tera.

2. Tera, klõpsake nuppu **Värskenda** .

    ![Nupp Värskenda](./media/cloud-services-how-to-manage-portal/update-button.png)

3. Värskendage juurutamise uue alla fail (.cspkg) ja teenuse konfiguratsioonifail (.cscfg).

    ![UpdateDeployment](./media/cloud-services-how-to-manage-portal/update-blade.png)

4. **Soovi korral** värskendage juurutamise silt ja salvestusruumi konto. 

5. Kui mis tahes rollid on ainult üks rolli eksemplari, valige soovitud **juurutada isegi juhul, kui üks või mitu rollid sisaldavad ühekordsest** jätkamiseks versiooniuuenduse lubamine. 

    Azure'i saate garanteeri ainult 99,95% teenuse kättesaadavus pilvepõhise teenuse värskendamise ajal, kui iga rolli on vähemalt kaks rolli aknad (virtuaalmasinates). Kahe rolli aknad, kus ühe virtuaalse masina töötleb klientide päringutele ajal teise värskendatakse.

6. Kontrollige **käivitamine juurutuse** värskendus rakendatakse siis, kui paketi üleslaadimine on lõpule jõudnud.

7. Klõpsake nuppu **OK** , et alustada teenuse värskendamine.



## <a name="how-to-swap-deployments-to-promote-a-staged-deployment-to-production"></a>Kuidas: Vaheta juurutuste edendada tootmise etapiviisilise juurutamine

Kui otsustate uue versiooni pilveteenus etapi juurutada ja testige oma uus versioon teie pilvepõhise teenuse lavastus keskkonnas. Saate **vahetada** URL-id, mille kahe juurutuse on adresseeritud ja edendada uus versioon tootmisele liikuda. 

Saate vahetada juurutuste **Pilveteenustega** lehelt või armatuurlaud.

1. [Azure'i portaalis][]valige pilveteenuses, mida soovite värskendada. Selles etapis tuleb avatakse pilvepõhise teenuse eksemplari tera.

2. Labale nuppu **Vaheta** .

    ![Cloud Services vahetamine](./media/cloud-services-how-to-manage-portal/swap-button.png)

3. Kinnituse järgmine viip avatakse.

    ![Cloud Services vahetamine](./media/cloud-services-how-to-manage-portal/swap-prompt.png)

4. Pärast kontrollida juurutamise teavet, klõpsake nuppu **OK** kasutuselevõttu vahetada.

    Juurutamise Vaheta juhtub kiiresti kuna ainus asi, mis muudab on virtuaalse IP-aadressid (VIP) soovitud juurutuste.

    Arvuta kulud salvestamiseks saate kustutada lavastus juurutamise pärast kontrollimist juurutamise tootmise töötab ootuspäraselt.

## <a name="how-to-link-a-resource-to-a-cloud-service"></a>Kuidas: Link ressursi pilveteenusesse

Azure portaali link ressursid koos nagu praeguse Azure klassikaline portaali ei. Selle asemel juurutada lisaressursid pilveteenusesse, mida kasutavad ühte ressursirühma.

## <a name="how-to-delete-deployments-and-a-cloud-service"></a>Kuidas: juurutuste ja pilveteenus kustutamine

Enne kustutamist mõnda pilveteenusesse, siis kustutage iga olemasoleva juurutamise.

Arvuta kulud salvestamiseks saate kustutada lavastus juurutamise pärast kontrollimist juurutamise tootmise töötab ootuspäraselt. On arve Arvuta kulud juurutatud rolli eksemplarid, mis on peatatud.

Järgmiste toimingute abil saate kustutada juurutamine või oma pilveteenuses. 

1. [Azure'i portaalis][]valige pilveteenusesse, mille soovite kustutada. Selles etapis tuleb avatakse pilvepõhise teenuse eksemplari tera.

2. Tera, klõpsake nuppu **Kustuta** .

    ![Cloud Services vahetamine](./media/cloud-services-how-to-manage-portal/delete-button.png)

3. Saate kustutada kogu pilveteenuses, märkides ruudu **pilveteenus ja selle kasutuselevõttu** või valida, kas **tootmise juurutamise** või **lavastus juurutamise**.

    ![Cloud Services vahetamine](./media/cloud-services-how-to-manage-portal/delete-blade.png) 

4. Klõpsake allosas nuppu **Kustuta** .

5. Pilveteenusesse kustutamiseks valige **Kustuta pilveteenuses**. Klõpsake kinnitusviibas, nuppu **Jah**.

> [AZURE.NOTE]
> Kui pilveteenus kustutatakse ja Paljusõnaline jälgimine on konfigureeritud, peate andmed käsitsi kustutada, salvestusruumi kontolt. Lisateabe saamiseks üles leida mõõdikute tabelid, lugege [artiklit](cloud-services-how-to-monitor.md) .

[Azure'i portaal]: https://portal.azure.com

## <a name="next-steps"></a>Järgmised sammud

* [Üldine konfigureerimine oma pilveteenuses](cloud-services-how-to-configure-portal.md).
* Siit saate teada, juurutamise [mõnda pilveteenusesse](cloud-services-how-to-create-deploy-portal.md).
* Saate konfigureerida [kohandatud domeeni nime](cloud-services-custom-domain-name-portal.md).
* [Ssl-sertide](cloud-services-configure-ssl-certificate-portal.md)konfigureerimine.