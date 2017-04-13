<properties 
   pageTitle="Juurdepääs privaatne Azure pilve sees Visual Studio | Microsoft Azure'i"
   description="Saate teada, kuidas juurdepääsu privaatne cloud ressursid Visual Studio abil."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="accessing-private-azure-clouds-with-visual-studio"></a>Juurdepääs privaatne Azure pilve sees Visual Studio

##<a name="overview"></a>Ülevaade

Vaikimisi Visual Studio toetab avaliku Azure pilveteenuse ülejäänud lõpp-punktid. See võib olla probleem, küll, kui kasutate Visual Studio Azure kohaselt. Sertide abil saate konfigureerida Visual Studio juurdepääsu privaatne Azure pilveteenuse ülejäänud lõpp-punktid. Saate need serdid kaudu oma Azure'i avalda sätete fail.

## <a name="to-access-a-private-azure-cloud-in-visual-studio"></a>Privaatne Azure'i juurdepääsu pilve Visual Studio

1. [Azure'i klassikaline portaali](http://go.microsoft.com/fwlink/?LinkID=213885) privaatne Cloud avalda sätted faili alla laadida või pöörduge oma administraatori poole avalda sätete fail. Azure'i avaliku versiooni, laadige see link on [https://manage.windowsazure.com/publishsettings/](https://manage.windowsazure.com/publishsettings/). (Saate alla laadida faili peab olema .publishsettings laiend).

1. **Server Explorer** Visual Studios, valige **Azure** sõlm ja kiirmenüü, valige käsk **Tellimuste haldamine** .

    ![Käsk tellimuste haldamine](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790778.png)

1. Dialoogiboksis **Microsoft Azure'i tellimuste haldamine** **serdid** menüüd ja seejärel klõpsake nuppu **impordi** .

    ![Azure'i sertide importimine](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790779.png)

1. Dialoogiboksis **Importimine Microsoft Azure'i tellimused** liikuge kausta, kuhu salvestatud avalda sätete fail ja valige fail, siis klõpsake nuppu **impordi** . See impordib serdid avalda sätete fail Visual Studio. Nüüd peaks saama suhelda oma privaatne cloud ressursse.

    ![Importimise avaldamine sätted](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790780.png)

## <a name="next-steps"></a>Järgmised sammud

[Avaldamine on Azure pilveteenuses Visual Studio](https://msdn.microsoft.com/library/azure/ee460772.aspx)

[Kuidas: alla laadida ja impordi avaldamine sätted ja tellimuse teave](https://msdn.microsoft.com/library/dn385850(v=nav.70).aspx)

