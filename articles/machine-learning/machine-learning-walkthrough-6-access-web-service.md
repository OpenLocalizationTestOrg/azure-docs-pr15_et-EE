<properties
    pageTitle="Samm 6: Pääseda arvuti õ veebiteenuse | Microsoft Azure'i"
    description="Töötada lühiülevaade sõnastikupõhise lahenduse juhist 6: aktiivne Azure seadme õ veebiteenuse juurdepääs."
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="garye"/>


# <a name="walkthrough-step-6-access-the-azure-machine-learning-web-service"></a>Kiirtutvustus juhist 6: Juurdepääs Azure'i masina õ veebiteenuse

See on viimases etapis kiirtutvustus, [töötada ennustav lahenduse Azure seadme õppe](machine-learning-walkthrough-develop-predictive-solution.md)


1.  [Seadme õ tööruumi loomine](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  [Laadige üles olemasolevad andmed](machine-learning-walkthrough-2-upload-data.md)
3.  [Looge uus katse](machine-learning-walkthrough-3-create-new-experiment.md)
4.  [Koolitada ja hindate mudelid](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  [Veebiteenuse juurutamine](machine-learning-walkthrough-5-publish-web-service.md)
6.  **Accessi veebiteenuse**

----------

Ülevaate eelmises etapis me juurutatud veebiteenus, mis kasutab meie krediitkaardiga risk ennustamine mudel. Nüüd kasutajad peavad saama andmeid saata ja vastu võtta tulemused. 

Veebiteenuse on Azure veebiteenus, mille saate vastu võtta ja tagastada kahel viisil REST API-de kasutamine andmete:  

-   **Taotluse/vastuse** - kasutaja saadab üks või mitu rida krediitkaardi andmed teenuse abil HTTP-protokolli ja teenuse vastab ühe või mitme tulemuste komplekti.
-   **Paketi täitmist** – kasutaja talletab krediitkaardi andmed üks või mitu rida on Azure'i bloobimälu ja saadab teenuse bloobimälu asukoht. Teenuse andmete kõik read järgnevates Sisestuskeel bloobimälu, talletab tulemused teise bloobimälu ja tagastab selle container URL-i.  

Kiireim ja lihtsaim viis veebiteenuse juurdepääsuks on Web Appi mallide saadaval [Azure'i Web Appi turuplatsi](https://azure.microsoft.com/marketplace/web-applications/all/)kaudu.
Neid web appi malle saate koostada kohandatud veebirakenduse, teab oma veebiteenuse sisendandmete ja see ei annaks. Kõik, mida peate tegema on veebiteenuse ja andmetele juurdepääsu ja malli ei ülejäänud.

Web appi mallide kasutamise kohta leiate lisateavet teemast [tarbida Azure seadme õ veebiteenuse, web appi malli abil](machine-learning-consume-web-service-with-web-app-template.md).

Samuti saate arendada kohandatud rakenduse veebiteenuse, kasutades starter kood antakse teile R, C# ja Python programmeerimiskeele juurdepääsu.
Kõigi üksikasjade leiate [Azure'i masina õ veebiteenus, mis on avaldatud Õppekeskuse katse seadmes peab olema kohta](machine-learning-consume-web-services.md).
