<properties 
    pageTitle="Visual Studio loogika rakenduste koostamine | Microsoft Azure'i" 
    description="Projekti loomine luua ja juurutada loogika rakenduse Visual Studios." 
    authors="jeffhollan" 
    manager="erikre" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jehollan"/> 
    
# <a name="build-and-deploy-logic-apps-in-visual-studio"></a>Luua ja juurutada loogika rakenduste Visual Studio

Kuigi [Azure portaali](https://portal.azure.com/) annab teile hästi kavandada ja hallata loogika rakendusi, võite ka kujundada ja Juurutage loogika rakendus Visual Studio asemel.  Loogika rakendused tuleb konfigureerida rikkaliku Visual Studio tööriistad, mis võimaldab teil koostada loogika rakenduse designer, mis tahes juurutamise ja automatiseerimine Mallid ja mis tahes keskkonda juurutamine.  

## <a name="installation-steps"></a>Installimise juhiseid.

Allpool on toodud juhised installida ja konfigureerida Visual Studio tööriistad loogika rakendused.

### <a name="prerequisites"></a>Eeltingimused

- [Visual Studio 2015](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
- [Viimane Azure SDK](https://azure.microsoft.com/downloads/) (2.9.1 või suurem)
- [Azure PowerShelli] (https://github.com/Azure/azure-powershell#installation)
- Juurdepääs veebile kui manustatud Designeri kasutamine

### <a name="install-visual-studio-tools-for-logic-apps"></a>Visual Studio tools for loogika rakenduste installimine

Kui olete installinud, eeltingimused 

1. Avage menüü **Tööriistad** Visual Studio 2015 ja valige **laiendid ja värskendused**
1. Valige kategooria **Online'i** otsimine veebis
1. **Loogika rakenduste** **Azure loogika rakenduste Visual Studio tööriistad** kuvamiseks otsimine
1. Klõpsake nuppu **alla laadida** alla laadima ja installima laiendamine
1. Pärast installimist taaskäivitage Visual Studio

> [AZURE.NOTE] Saate ka alla laadida laiendamine otse [Järgmine link](https://visualstudiogallery.msdn.microsoft.com/e25ad307-46cf-412e-8ba5-5b555d53d2d9)

Kui installitud, siis saab kasutada Azure ressursirühm projekti loogika rakenduse Designer.

## <a name="create-a-project"></a>Projekti loomine

1. Minge menüüsse **fail** ja valige **Uus** >  **Project** (või saate **Lisa** ja seejärel valige **Uus projekt** lisamiseks olemasoleva lahenduse):  ![menüü fail](./media/app-service-logic-deploy-from-vs/filemenu.png)

1. Dialoogiboksis **Cloud**otsimine ja valige **Azure ressursirühma**. Tippige **nimi** ja seejärel klõpsake nuppu **OK**.
    ![Saate lisada uue projekti](./media/app-service-logic-deploy-from-vs/addnewproject.png)

1. **Loogika rakenduse** malli valimine See loob tühja loogika rakendusemall juurutamise alustada.
    ![Valige Azure Mall](./media/app-service-logic-deploy-from-vs/selectazuretemplate.png)

1. Kui olete valinud **malli**, tabas **OK**.

    Nüüd loogika rakenduse projekti lisatakse teie lahendus. Peaksite nägema juurutamise faili Solution Exploreris:  

    ![Juurutamine](./media/app-service-logic-deploy-from-vs/deployment.png)

## <a name="using-the-logic-app-designer"></a>Loogika rakenduse Designeri kasutamine

Kui teil on Azure ressursirühm projekti, mis sisaldab loogika rakenduse, saate avada designer Visual Studio töövoo loomisel teid aidata sees.  Designer nõuab Interneti-ühendus, et päringu konnektorid saadaval atribuudid ja andmeid (nt Dynamics CRM Online'i konnektor kasutamisel designer päringu oma CRM-i eksemplari saadaval kohandatud ja vaikeatribuudid).

1. Paremklõpsake selle `<template>.json` fail ja valige käsk **Ava loogika rakenduse kujundajaga** (või `Ctrl+L`)
1. Valige tellimus, ressursirühm ja juurutamise malli asukoht
    - See on oluline Pange tähele, et loogika rakenduse kujundamise loob **API -ga ühenduse** ressursse päringu atribuudid ajal kujundus.  Valitud ressursirühma on need ühendused ajal kujundusaja loomiseks kasutatud ressursirühma.  Saate vaadata või muuta mis tahes API ühendused Azure'i portaal ja sirvimise **API ühenduste**jaoks.
    ![Tellimuse valija](./media/app-service-logic-deploy-from-vs/designer_picker.png)
1. See muudab designer põhjal määratlus on `<template>.json` faili.
1. Nüüd saate luua ja kujundada rakenduse loogika ja muudatused värskendatakse juurutamise malli.
    ![Visual Studio kujundaja](./media/app-service-logic-deploy-from-vs/designer_in_vs.png)

Näete ka `Microsoft.Web/connections` ressursi faili mis tahes ühenduste funktsioon loogika rakenduse jaoks vajalik lisamisest ressursid.  Need ühenduse atribuudid saate määrata juurutamisel ja haldamine pärast juurutate **API ühendused** Azure'i portaalis.

### <a name="switching-to-the-json-code-view"></a>Koodi JSON-vaate aktiveerimine

Saate valida **Koodi** vaade allservas designer JSON kujutis loogika rakenduse aktiveerimiseks.  Aktiveerige täielik ressursi JSON, paremklõpsake selle `<template>.json` fail ja valige käsk **Ava**.

### <a name="saving-the-logic-app"></a>Loogika rakenduse salvestamine

Saate salvestada loogika app veebisaidil igal ajal kaudu nuppu **Salvesta** või `Ctrl+S`.  Kui salvestate ajal vigu oma loogika rakendusega, kuvatakse need **väljundi** aknas Visual Studio.

## <a name="deploying-your-logic-app"></a>Loogika rakenduse juurutamine

Lõpuks pärast seda, kui olete konfigureerinud oma rakenduse, saate juurutada otse Visual Studio vaid paar toimingut. 

1. Paremklõpsake Solution Exploreris projekt ja minge **Deploy** > **Uue juurutamise...** 
     ![Uue juurutamine](./media/app-service-logic-deploy-from-vs/newdeployment.png)

2. Teil palutakse, logige sisse oma Azure'i subscription(s). 

3. Nüüd peate valima ressursirühm, mida soovite loogika rakenduse juurutamine üksikasju. 
    ![Võtta kasutusele ressursirühm](./media/app-service-logic-deploy-from-vs/deploytoresourcegroup.png)

     > [AZURE.NOTE]    Kindlasti õige Mall ja parameetrite failide ressursirühm (nt kui juurutate tootmiskeskkonda soovite valida parameetrid tootmistoimikus) valimine. 
4. Valige Deploy nupp
 
    
6. Juurutamise olek kuvatakse **väljundi** aknas (võib-olla peate **Azure'i ettevalmistamise**valimiseks. 
    ![Väljund](./media/app-service-logic-deploy-from-vs/output.png)

Tulevikus, saate vaadata oma loogika rakendust andmeallika juhtelemendi ja Visual Studio abil saate juurutada uue versiooni. 

> [AZURE.NOTE] Kui muudate otse portaalis Azure määratluse, seejärel järgmine kord, kui juurutate Visual Studio need muudatused kirjutatakse.

## <a name="next-steps"></a>Järgmised sammud

- Loogika Appsi kasutamise alustamine, järgige õpetuse [loogika rakenduse loomine](app-service-logic-create-a-logic-app.md) .  
- [Vaate levinud näited ja stsenaariumid](app-service-logic-examples-and-scenarios.md)
- [Te saate loogika rakenduste abil äriprotsesside automatiseerimine](http://channel9.msdn.com/Events/Build/2016/T694) 
- [Saate teada, kuidas teie integreerida loogika rakendused](http://channel9.msdn.com/Events/Build/2016/P462)