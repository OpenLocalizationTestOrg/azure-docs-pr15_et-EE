<properties
    pageTitle="Juurutamine Visual Studios Azure'i virnas malle | Microsoft Azure'i"
    description="Saate teada, kuidas juurutamine Visual Studios Azure'i virnas malle."
    services="azure-stack"
    documentationCenter=""
    authors="HeathL17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="helaw"/>

# <a name="deploy-templates-in-azure-stack-using-visual-studio"></a>Mallide abil Visual Studio Azure'i virnas juurutamine

Visual Studio abil saate juurutada Azure virnas POC Azure'i ressursihaldur mallid.

Ressursihaldur Mallid juurutada ja ettevalmistamise rakenduse ühe koordineeritud kasutusel kõik ressursid.

1.  Avage Visual Studio 2015 Update 1.

2.  Klõpsake menüüd **fail**, nuppu **Uus**ja valige dialoogiboksi **Uus projekt** **Azure'i ressursirühma**.

3.  Sisestage uue projekti **nime** ja seejärel klõpsake nuppu **OK**.

4.  **Azure'i malli valimine** dialoogiboksis nuppu **Windowsi virtuaalse masina**ning seejärel klõpsake nuppu **OK**.

  Uue projekti, näete saadaolevate mallide loendi laiendamine **Solution Exploreris** paanil sõlme **malle** .

5.  Paanil **Solution Exploreris** paremklõpsake projekti nime, klõpsake käsku **Deploy**ja klõpsake nuppu **Uus juurutamine**.

6.  Valige dialoogiboksis **Deploy ressursirühma** **tellimuse** ripploendis tellimuse Microsoft Azure'i virnas.

7.  **Ressursirühm** loendis Valige ressursi olemasolevasse rühma või looge uus.

8.  **Ressursi rühma asukohta** loendis Valige asukoht ja seejärel käsku **Deploy**.

9.  Dialoogiboksis **Redigeeri parameetrid** Sisestage väärtused parameetrid (mis erineda Mall) ja klõpsake siis nuppu **Salvesta**.

## <a name="next-steps"></a>Järgmised sammud

[Mallide käsureal juurutamine](azure-stack-deploy-template-command-line.md)
