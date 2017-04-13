<properties
    pageTitle="Hdinsightiga rakenduste avaldamine | Microsoft Azure'i"
    description="Saate teada, kuidas luua ja avaldada Hdinsightiga rakendused."
    services="hdinsight"
    documentationCenter=""
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="10/18/2016"
    ms.author="jgao"/>

# <a name="publish-hdinsight-applications-into-the-azure-marketplace"></a>Azure'i turuplatsi rakendused Hdinsightiga avaldamine

Rakenduse Hdinsightiga on rakendus, mida kasutajad saavad installida Linux-põhine Hdinsightiga klaster. Need rakendused saate välja töötada Microsoft sõltumatu tarkvara tarnijate (ISV) või ise. Sellest artiklist saate teada, kuidas avaldada üheks Azure'i turuplatsi rakenduse Hdinsightile.  Üldist teavet üheks Azure'i turuplatsi avaldamise kohta leiate teemast [pakkumise Azure'i turuplatsi avaldada](../marketplace-publishing/marketplace-publishing-getting-started.md).

Hdinsightiga rakendusi kasutada *Tuua teie enda litsents (BYOL)* mudel, kus rakenduse pakkuja vastutab litsentsimise lõppkasutajatele rakendus ja lõppkasutajate ainult küsitud Azure'i luua, nt Hdinsightiga kobar ja selle VMs/sõlmed ressursside kohta. Sel ajal, on valmis arveldamine ise rakenduse kaudu Azure'i.

Muude rakenduste Hdinsightiga seotud artikkel:

- [Rakenduste installimine Hdinsightiga](hdinsight-apps-install-applications.md): saate teada, kuidas installida rakendus Hdinsightile oma kogumite.
- [Kohandatud Hdinsightiga rakenduste installimine](hdinsight-apps-install-custom-applications.md): saate teada, kuidas installida ja testida kohandatud Hdinsightiga rakendusi.

 
## <a name="prerequisites"></a>Eeltingimused

Kohandatud rakenduse kuvatakse Marketplace'ist esitamiseks peab teil on loodud ja testitud kohandatud rakenduse. Lugege järgmisi artikleid:

- [Kohandatud Hdinsightiga rakenduste installimine](hdinsight-apps-install-custom-applications.md): saate teada, kuidas installida ja testida kohandatud Hdinsightiga rakendusi.

Peate ka on registreerige arendaja kontole. Leiate [Azure'i turuplatsi pakkumise avaldamine](../marketplace-publishing/marketplace-publishing-getting-started.md) ja [Loo konto Microsoft arendaja](../marketplace-publishing/marketplace-publishing-accounts-creation-registration.md).

## <a name="define-application"></a>Rakenduse määratlemine

Seotud rakendusi Azure'i turuplatsi avaldamiseks on kaks toimingut.  Esmalt määratlete **createUiDef.json** faili näitamaks, millised kogumite teie rakendus ühildub; ja avaldage Mall Azure portaalist. Järgmises createUiDef.json näidisfaili.

    {
        "handler": "Microsoft.HDInsight",
        "version": "0.0.1-preview",
        "clusterFilters": {
            "types": ["Hadoop", "HBase", "Storm", "Spark"],
            "tiers": ["Standard", "Premium"],
            "versions": ["3.4"]
        }
    }


|Väli  | Kirjeldus   | Võimalikud väärtused|
|-------|---------------|----------------|
|tüübid  | Kobar tüübid, mis rakenduse ühildub.    |Hadoopi, HBase, Storm, säde (või nende kombinatsioon)|
|astme  | Mis on rakendus ühildub kobar astme.    |Standard, Premium (või mõlemad)|
|versioonid|  Hdinsightiga kobar tüübid, mis rakenduse ühildub.    |3.4|

## <a name="package-application"></a>Paketi rakendus

Looge zip-fail, mis sisaldab kõiki nõutud faile Hdinsightiga rakenduste installimist. Peate [Avalda](#publish-application)rakenduses zip-fail.

- [createUiDefinition.json](#define-application).
- mainTemplate.json. Lugege teemat [kohandatud Hdinsightiga rakenduste installimine](hdinsight-apps-install-custom-applications.md)proovi.

    >[AZURE.IMPORTANT] Rakenduse installimise skripti nimede nimi peab olema kordumatu kindla kobar järgmises vormingus. Lisaks mis tahes installida ja desinstallida skripti toimingud tuleks idempotent, s.t skriptide saab kutsuda repeatly ajal sama tulemuse.
    
    >   nimi":" [concat (' tooni-installi-v0 ","-", uniquestring('applicationName')]"
        
    >Võtke arvesse, et kolmest osast skripti nimi.
        
    >   1. Skripti nime eesliite, mis sisaldab nimi, mis on seotud rakenduse või rakenduse nimi.
    >   2. A "-" loetavuse.
    >   3. Funktsioon kordumatu string koos parameetriga rakenduse nimi.

    >   Näide on ülaltoodud jõuab muutumas: tooni-installi-v0-4wkahss55hlas loendis nõutud skripti toiming. Valimi JSON last, vt [https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/Hue/azuredeploy.json](https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/Hue/azuredeploy.json).

- Kõiki vajalikke skriptide.

> [AZURE.NOTE] Rakenduse failid (sh veebirakenduse faile, kui on olemas) võib asuda mis tahes avalikult juurdepääsetava lõpp-punkti.

## <a name="publish-application"></a>Teenuserakenduse avaldamine

Tehke avaldada rakenduse Hdinsightiga tehke järgmist.

1. [Azure'i avaldamisportaal](https://publish.windowsazure.com/)sisse logida.
2. Klõpsake **lahenduse Mallid** vasakult lahenduse uue malli loomine.
3. Sisestage tiitel ja seejärel klõpsake käsku **Loo uus mall lahendus**.
3. Klõpsake **Konto ja Azure programmiga loomine Arenduskeskus** ettevõtte registreerida, kui te pole seda veel teinud.  Vaadake [Microsoft arendaja konto](../marketplace-publishing/marketplace-publishing-accounts-creation-registration.md).
4. Klõpsake **määratleda mõned topoloogiatest alustada**. Lahendus mall on "parent" kõik selle topoloogiatest. Saate määratleda mitme topoloogiatest ühe pakkumise/lahenduse malli. Kui soovite lavastus vajutamist pakkumise, on see lükata kõik selle topoloogiatest. 
4. Sisestage topoloogia nimi ja seejärel klõpsake plussmärki.
5. Sisestage uue versiooni ja seejärel klõpsake plussmärki.
6. Laadige [pakett](#package-application)rakenduses valmis zip-fail.  
7. Klõpsake **koosolekukutses sertimine**. Microsoft sertimine meeskond failid üle vaadata ja selle topoloogia kinnitamine.

## <a name="next-steps"></a>Järgmised sammud

- [Rakenduste installimine Hdinsightiga](hdinsight-apps-install-applications.md): saate teada, kuidas installida rakendus Hdinsightile oma kogumite.
- [Kohandatud Hdinsightiga rakenduste installimine](hdinsight-apps-install-custom-applications.md): saate teada, kuidas un avaldatud Hdinsightiga taotluse Hdinsightiga juurutamine.
- [Kohandamine Linux-põhine Hdinsightiga kogumite skripti toimingu abil](hdinsight-hadoop-customize-cluster-linux.md): saate teada, kuidas skripti toimingu abil saate installida täiendavad rakendused.
- [Hadoopi loomine Linux-põhine le Hdinsightiga Azure ressursihaldur mallide kasutamine](hdinsight-hadoop-create-linux-clusters-arm-templates.md): saate teada, kuidas kõne ressursihaldur Mallid Hdinsightiga kogumite loomiseks.
- [Kasutage tühja serva sõlmed Hdinsightile](hdinsight-apps-use-edge-node.md): saate teada, kuidas kasutada mõnda tühja serva sõlm juurdepääs Hdinsightiga kobar, Hdinsightiga rakenduste testimine ja majutusteenuse Hdinsightiga rakendused.

