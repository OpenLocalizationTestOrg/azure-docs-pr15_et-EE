<properties
    pageTitle="Azure'i virnas Web Appsi ülevaade | Microsoft Azure'i"
    description="Veebirakenduste Azure virnas ülevaade"
    services="azure-stack"
    documentationCenter=""
    authors="apwestgarth"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="app-service"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="anwestg"/>
    
# <a name="azure-stack-web-apps-overview"></a>Azure'i virnas Web Appsi ülevaade
    
> [AZURE.NOTE] Järgmine teave kehtib ainult Azure virnas TP1 juurutuste.

Azure'i virnas Web Apps on Azure rakenduse teenuse pakkumine, mis on toodud Azure'i virnas esimene element. Azure'i virnas veebirakendustes installiprogrammi loob eksemplari viis vajalikud rolli tüüpide ja loob ka failiserverisse. Kuigi saate lisada mitu eksemplari iga tüüpi roll, pidage meeles, ei ole palju ruumi vms tehnilise eelvaate 1. Praeguse võimaluste Azure'i virnas Web Apps on peamiselt foundation võimalusi, mida on vaja süsteem ja host veebirakenduste haldamine.

![Azure'i virnas rakenduse teenuse Web Apps on Azure virnastada portaal][1]

## <a name="limitations-of-the-technical-preview"></a>Piirangud tehniline eelvaade

Ei toetata Azure'i virnas rakenduse teenus eelvaade viimine. Selle release preview ei tootmise töökoormus sellele. Olemas on ka pole uuendus Azure'i virnas rakenduse teenus eelvaade viimine vahel. Esmane nende eelvaade viimine on kuvamiseks, mis on pakkuda ja saada tagasisidet. 

## <a name="what-is-an-app-service-plan"></a>Mis on rakenduse teenuse leping?

Azure'i virnas veebirakenduste ressursi pakkuja kasutab sama mis kasutab funktsiooni Azure'i veebirakenduste teenuses Azure rakenduse. Seetõttu on mõned levinud mõisted väärt kirjeldamiseks. Veebirakenduste, nimetatakse hinnakirjad container veebirakendustes rakenduse teenusleping. See tähistab spetsiaalne virtuaalmasinates kasutada hoidke rakenduste kogum. Jooksul antud tellimus, võib olla mitu rakendust Service lepingud. See kehtib ka Azure virnas veebirakendustes. 

Azure'is, on ühis- ja asjakohast töötajate. Ühiskasutusega töötaja toetab tihedusega rentnikuga web appi majutuse ja on ainult üks komplekt ühiskasutusega töötajate. Spetsiaalne serverite ainult ühest rentnikust kasutatavaid ja saadaval kolme suuruses: väike, Keskmine ja suur. Kohapealse klientide vajaduste rahuldamiseks ei saa kirjeldatud alati nende terminite abil. Azure'i virnas veebirakendustes ressursi pakkuja saavad administraatorid määratleda kasutajad soovivad kättesaadavaks teha nii, et neil on mitmele kriteeriumikogumile ühiskasutusega töötajate või erinevaid asjakohast töötajate vastavalt oma vajadustele majutusteenuse kordumatu web töötaja tasemete seas. Kasutades töötaja taseme määratlused, ta saab siis määrata oma hinnad SKU-de jaoks.

## <a name="portal-features"></a>Portaali funktsioonid

Nagu kehtib ka tagasi lõpuks, Azure'i virnas veebirakenduste kasutab sama Kasutajaliidese Azure veebirakenduste kasutava. Mõned funktsioonid on keelatud ja ei ole veel otstarbekas Azure'i virnas. See on Azure'i seotud ootustele või teenuseid, mida nende funktsioonide jaoks on vaja pole veel saadaval Azure virnas. 

## <a name="next-steps"></a>Järgmised sammud

- [Enne alustamist Azure'i virnas Web Apps](azure-stack-webapps-before-you-get-started.md)
- [Web Apps ressursi Provideri installimine](azure-stack-webapps-deploy.md)

Võite proovida ka muud [service (PaaS) teenused platvorm](azure-stack-tools-paas-services.md), nagu [SQL serveri ressursi pakkuja](azure-stack-sql-rp-deploy-short.md) ja [MySQL-i ressursikeskuse pakkuja](azure-stack-mysql-rp-deploy-short.md).

<!--Image references-->
[1]: ./media/azure-stack-webapps-overview/AppService_Portal.png
