<properties
   pageTitle="Kasutades emulaator kiire käivitamiseks ja silumine pilveteenus kohalikus arvutis | Microsoft Azure'i"
   description="Käivita ja silumine pilveteenus kohalikus arvutis emulaator kiire kasutamine"
   services="visual-studio-online"
   documentationCenter="n/a"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="visual-studio-online"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />


# <a name="using-emulator-express-to-run-and-debug-a-cloud-service-on-a-local-machine"></a>Käivita ja silumine pilveteenus kohalikus arvutis emulaator kiire kasutamine

Emulaator Expressi abil saate testida ja silumine pilveteenus käivitamata Visual Studio administraatorina. Saate määrata projekti sätteid nii, et kasutada emulaator kiire või täielik emulaator, sõltuvalt teie pilveteenuses nõuetele. Täielik emulaator kohta leiate lisateavet teemast [käivitage rakendus Azure emulaator arvutada](./storage/storage-use-emulator.md). Azure'i SDK 2.1 sisaldas emulaator Express ja Azure SDK 2.3, see on vaikimisi emulaator.

## <a name="using-emulator-express-in-the-visual-studio-ide"></a>Visual Studio IDE emulaator Express kasutamine

Kui loote uue projekti Azure'i SDK 2.3 või uuema versiooniga, emulaator kiire on juba valitud. Olemasoleva SDK varasema versiooniga loodud projekte, tehke emulaator kiire valimiseks.

### <a name="to-configure-a-project-to-use-emulator-express"></a>Projekti kasutada emulaator kiire konfigureerimine

1. Kiirmenüü Azure'i projekti valige **Atribuudid**ja seejärel menüüd **Web** .

1. Valige jaotises **Kohaliku arengu Server**, **Kasutage IIS-i kiire** nupp. Emulaator Express ei ühildu IIS veebiserver.

1. Valige jaotises **emulaator**, **Kasutage emulaator kiire** nupp.

    ![Emulaator Express](./media/vs-azure-tools-emulator-express-debug-run/IC673363.gif)

## <a name="launching-emulator-express-at-a-command-prompt"></a>Käivitades käsureale emulaator Express

Käsuviip, võite käivitada kiire versiooni Azure'i arvutada emulaator, csrun.exe, /useemulatorexpress suvandi abil.

## <a name="limitations"></a>Piirangud

Enne kasutada emulaator kiire, peaks olema teadlik mõned piirangud.

- Emulaator Express ei ühildu IIS veebiserver.

- Oma pilveteenuses võib sisaldada mitut rollid, kuid iga rolli on piiratud ühe eksemplari.

- Te ei pääse pordinumbrid alla 1000. Näiteks kui kasutate autentimisteenuse, mis tavaliselt kasutab porti alla 1000, võib vaja väärtuseks on üle 1000 pordinumber.

- Mingeid piiranguid, mis rakenduvad Azure'i arvutada emulaator rakendada ka emulaator kiire. Näiteks ei tohi olla pikem kui 50 rolli aknad juurutamise kohta. Teemast [Azure Arvuta emulaator rakenduse käivitamine](http://go.microsoft.com/fwlink/p/?LinkId=623050)

## <a name="next-steps"></a>Järgmised sammud

[Pilveteenustega silumine](https://msdn.microsoft.com/library/azure/ee405479.aspx)
