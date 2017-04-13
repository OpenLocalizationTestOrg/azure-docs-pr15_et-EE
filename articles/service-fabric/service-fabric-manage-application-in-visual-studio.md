<properties
   pageTitle="Visual Studio rakenduste haldamine | Microsoft Azure'i"
   description="Visual Studio abil saate luua, arendada, paketti, juurutada ja silumine oma teenuse struktuuri rakenduste ja teenuste."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/09/2016"
   ms.author="seanmck;mikhegn"/>

# <a name="use-visual-studio-to-simplify-writing-and-managing-your-service-fabric-applications"></a>Visual Studio abil lihtsustada kirjutamine ja teenuse struktuuri rakenduste haldamine

Saate hallata oma Azure teenuse struktuuri rakenduste ja teenuste Visual Studio kaudu. Kui olete [oma arenduskeskkond häälestada](service-fabric-get-started.md), saate teenuse struktuuri rakenduste loomine, lisage teenuseid või paketti, register ja kasutada rakendusi klaster kohaliku arengu Visual Studio.

## <a name="deploy-your-service-fabric-application"></a>Teenuse struktuuri rakenduse juurutamine

Vaikimisi rakenduse juurutamine ühendab ühe lihtsa toimingu järgmist:

1. Rakenduse paketi loomine
2. Pildi poe rakenduse paketi üleslaadimine
3. Registreerimisel rakenduse tüüp
4. Eemaldada kõik töötavad rakenduse eksemplarid
5. Uue rakenduse eksemplari loomine

Visual Studio, vajutage **klahvi F5** ka juurutada rakendust ja manustamine siluri läbivalt kogu rakenduse. **Ctrl + F5** abil saate juurutada rakenduse ilma silumine või avalda profiili abil saate avaldada kohaliku või kaugandmebaasiga kobar. Lisateabe saamiseks vt [rakenduse Visual Studio abil remote arvutikobaras avalda](service-fabric-publish-app-remote-cluster.md).

### <a name="application-debug-mode"></a>Rakenduse silumine režiim

Vaikimisi Visual Studio eemaldab olemasoleva eksemplari oma rakenduse tüüp, kui lõpetate silumine või (kui olete juurutanud rakenduse siluri manustamata), kui te ümberkorraldamine rakendus. Sel juhul eemaldatakse kõik rakenduse andmed. Ajal silumine kohalikult, soovite säilitada andmeid, mida olete juba loonud, kui rakenduse uus versioon, soovite hoida rakendus töötab või soovite järgmise silumine seansid rakenduse versioonile. Visual Studio teenuse struktuuri tööriistad pakuvad atribuudi **Rakenduse silumine režiimi**, mis kontrollib kas **F5** tuleks eemaldada rakendus, hoidke pärast silumine seansi lõpeb või taotluse uuendata edaspidiste silumine seansside, mitte eemaldada ja ümberpaigutatud rakendus.

#### <a name="to-set-the-application-debug-mode-property"></a>Atribuudi rakenduse silumine režiim

1. Rakenduse project kiirmenüü, valige **Atribuudid** (või vajutage klahvi **F4** ).
2. Aknas **Atribuudid** atribuudi **Rakenduse silumine režiim** .

    ![Rakenduse silumine režiimi atribuudi][debugmodeproperty]

Need on **Rakenduse silumine režiimi** suvandid saadaval.

1. **Automaatne versioonitäiendus**: rakenduse jätkuvalt lõppemisel silumine seansi käivitamine. Järgmise **F5** kohtleb juurutamise versioonitäienduse unmonitored automaatse abil kiiresti rakenduse versiooni täiendamine kuupäeva stringi. Versiooniuuenduse säilitab kõik eelmise silumine seansi sisestatud andmed.

2. **Säilita rakenduse**: rakendus hoiab tööta klaster silumine seansi lõppemisel. Klõpsake järgmise **F5** , eemaldatakse rakendus ja äsja ehitatud rakendus juurutatakse klaster.

3. **Rakenduse eemaldamine** põhjustab rakenduse silumine seansi lõppemisel eemaldada.

**Automaatne versioonitäiendus** on andmed säilitada, rakendades rakenduse versioonitäienduse võimaluste teenuse struktuuri, kuid see on häälestatud optimeerimine jõudlusega, mitte Turve. Rakenduste ja kuidas võib versiooniuuenduse reaal-keskkonnas üleminekut kohta leiate lisateavet teemast [teenuse struktuuri rakenduse täiendamine](service-fabric-application-upgrade.md).

![Uue rakenduse versioon kuupäevaga lisatud näide][preservedata]

>[AZURE.NOTE] Atribuut pole enne versiooni 1.1 teenuse struktuuri tööriistad Visual Studio. Enne 1.1, kasutage atribuudi **Säilita andmete alustada** saavutada sama käitumine. Suvand "Hoida rakendus" võeti kasutusele teenuse struktuuri tööriistad versioon 1.2 Visual Studio.

## <a name="add-a-service-to-your-service-fabric-application"></a>Teenuse teenuse struktuuri rakenduse lisamine

Laiendada selle funktsionaalsus rakenduse saate lisada uusi teenuseid.  Tagada, et teenuse oma rakendusepaketi, lisada **Uue struktuuri teenuse...** menüükäsk vahendusel.

![Uue struktuuri teenuse rakenduse lisamine][newservice]

Valige teenuse struktuuri projekti tüüp rakenduse lisamiseks ja Määrake teenuse nimi.  Vt [valimise teenust raamistik](service-fabric-choose-framework.md) aitab teil otsustada, millist teenuste kasutamiseks.

![Valige lisamiseks rakenduse struktuuri teenuse projekti tüüp][addserviceproject]

Uus teenus lisatakse teie lahendus ja olemasoleva rakendusepaketi. Teenuse viited vaikimisi teenuse näiteks lisatakse Rakendusmanifest. Teenuse luuakse ja järgmine kord, kui rakenduse alustamine.

![Uus teenus lisatakse teie Rakendusmanifest][newserviceapplicationmanifest]

## <a name="package-your-service-fabric-application"></a>Teenuse struktuuri rakenduse paketti

Juurutada arvutikobaras rakendus ja teenuseid, peate looma rakendusepaketi.  Paketi korraldab Rakendusmanifest, teenuse manifest(s) ja muud vajalikud failid konkreetse paigutuse.  Visual Studio häälestab ja haldab paketi kataloogis 'pkg' rakendus projekti kaustas.  Klõpsates **paketi** kontekstimenüüst **rakenduse** loob või värskendab rakenduse pakett.  Kui soovite seda teha, kui rakenduse juurutamine kohandatud PowerShelli skriptide abil.

## <a name="remove-applications-and-application-types-using-cloud-explorer"></a>Rakenduste ja rakenduse tüüpi Cloud Exploreriga eemaldamine

Saate teha lihtsa kobar halduse toimingute Visual Studio abil Cloud Explorer, mida saate käivitada, klõpsake menüü **Vaade** kaudu. Näiteks saate kustutada rakendusi ja ettevalmistamise tühistamine rakenduse tüüpide kogumite kohaliku või kaugandmebaasiga.

![Rakenduse eemaldamine](./media/service-fabric-manage-application-in-visual-studio/removeapplication.png)

>[AZURE.TIP] Rikkalikumat kobar funktsioonid, leiate teemast [visualiseerimine klaster teenuse struktuuri Exploreriga](service-fabric-visualizing-your-cluster.md).


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Järgmised sammud

- [Teenuse struktuuri mudel](service-fabric-application-model.md)
- [Teenuse struktuuri rakenduse juurutamine](service-fabric-deploy-remove-applications.md)
- [Rakenduse parameetrid mitme keskkonnas haldamine](service-fabric-manage-multiple-environment-app-configuration.md)
- [Teenuse struktuuri rakenduse silumine](service-fabric-debugging-your-application.md)
- [Visualiseerimine klaster teenuse struktuuri Exploreri kaudu](service-fabric-visualizing-your-cluster.md)

<!--Image references-->
[addserviceproject]:./media/service-fabric-manage-application-in-visual-studio/addserviceproject.png
[manageservicefabric]: ./media/service-fabric-manage-application-in-visual-studio/manageservicefabric.png
[newservice]:./media/service-fabric-manage-application-in-visual-studio/newservice.png
[newserviceapplicationmanifest]:./media/service-fabric-manage-application-in-visual-studio/newserviceapplicationmanifest.png
[preservedata]:./media/service-fabric-manage-application-in-visual-studio/preservedata.png
[debugmodeproperty]:./media/service-fabric-manage-application-in-visual-studio/debugmodeproperty.png
