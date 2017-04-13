<properties
   pageTitle="Lahenduste haldus komplektis toimingud (OMS) | Microsoft Azure'i"
   description="Lahenduste laiendavad toimingud halduse komplekti (OMS) esitada pakkimisel halduse stsenaariumi, mis kliendid saate lisada oma OMS tööruumi.  See artikkel annab üksikasjad loodud klientide ja partnerite kuidas kohandatud lahendused."
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/17/2016"
   ms.author="bwren" />

# <a name="management-solutions-in-operations-management-suite-oms-preview"></a>Lahenduste toimingud halduse komplekti (OMS) (eelvaade)

>[AZURE.NOTE]See on esialgse dokumentatsiooni halduse lahendusi OMS, mis on praegu eelvaade.    

Lahenduste laiendavad toimingud halduse komplekti (OMS) esitada pakkimisel halduse stsenaariumi, mis kliendid saate lisada oma keskkonnas.  Lisaks [Microsofti antud lahendusi](../log-analytics/log-analytics-add-solutions.md), partnerid ja kliendid saate luua halduse lahendusi kasutada oma keskkonda või ühenduse kaudu kättesaadavaks tehtud.

## <a name="finding-and-installing-management-solutions"></a>Otsimine ja lahenduste installimine
On mitu võimalust leidmiseks ja installimiseks lahendusi, nagu on kirjeldatud järgmistes lõikudes.

### <a name="azure-marketplace"></a>Azure'i turuplats
Microsofti lahenduste antud ja usaldusväärsete partnerite võib olla installitud Azure'i portaalis Azure'i turuplatsilt.

1. Azure portaali sisse logida.
2. Valige vasakul paanil **rohkem teenuseid**.
3. Liikuge kerides allapoole jaotiseni **lahenduste** või Tippige dialoogiboksi **filtreerimine** *lahendusi* .
4. Klõpsake nuppu **+ Lisa** .
5. Otsige lahendusi, mis teile huvi kas sirvimise, klõpsates nuppu **Filter** või tippimist väljale **Otsi Everthing** .
6. Klõpsake käsku turuplatsi üksus oma täpsema teabe kuvamiseks.
4. Klõpsake nuppu **Loo** **Lisamine lahenduse** paani avamiseks.
5. Teil palutakse vajalik teave, nt [OMS tööruumi ja automatiseerimise konto](#oms-workspace-and-automation-account) Lisaks lahendus kõigi parameetrite väärtused.
6. Klõpsake nuppu **Loo** lahendust installida.

### <a name="oms-portal"></a>OMS portaal
Microsofti lahenduste võib olla installitud lahendusegaleriist OMS portaalis.

1. OMS portaali sisse logida.
2. Klõpsake paani **Lahendusegaleriisse** .
2. Klõpsake lehel OMS lahendusegaleriisse teavet iga võimalik lahendus. Klõpsake lahendusele, mille soovite lisada OMS nime.
3. Lahendusele, mille valisite lehel kuvatakse lahenduse üksikasjalikku teavet. Klõpsake nuppu **Lisa**.
4. Uue paani lahendusele, mille olete lisanud, kuvatakse lehe OMS ja te saate kasutuselevõtmine pärast OMS teenuse töötleb andmete ülevaade.

### <a name="azure-quickstart-templates"></a>Azure'i Kiirjuhend Mallid
Kogukonna liikmete saate esitada Azure'i Kiirjuhend malle lahendusi.  Saate neid hiljem installi malle alla laadida või kontrolli need siit saate teada, kuidas [luua oma lahendusi](#creating-a-solution).

1. Järgige kirjeldatud [OMS tööruumi ja automatiseerimise kontoga](#oms-workspace-and-automation-account) linkimiseks tööruumi ja konto.
2. Avage [Azure'i Kiirjuhend Mallid](https://azure.microsoft.com/documentation/templates/).  
3. Otsige lahenduse, mis teile huvi.
4. Tulemuste üksikasjade kuvamiseks valige lahendus.
5. Klõpsake nuppu **Deploy Azure** .
6. Teil palutakse teavet nagu ressursirühm ja lisaks väärtused asukohta lahendus parameetrid.
7. Klõpsake nuppu **osta** installimiseks lahendus.

### <a name="deploy-azure-resource-manager-template"></a>Azure'i ressursihaldur malli juurutamine
Lahenduste, et saate ühenduse või selle [ise luua](#creating-a-solution) rakendatakse ressursihaldur mallina ja normaliseeritud meetodite saate kasutada [malli kasutamise](../resource-group-template-deploy-portal.md)kohta.  Pange tähele, et enne installimist lahenduse, peate looma ja link [OMS tööruumi ja automatiseerimise konto](#oms-workspace-and-automation-account).

## <a name="oms-workspace-and-automation-account"></a>OMS tööruumi ja automatiseerimise konto
Enamik lahenduste jaoks on vaja [OMS tööruumi](../log-analytics/log-analytics-manage-access.md) sisaldama vaadete ja [automatiseerimise konto](../automation/automation-security-overview.md#automation-account-overview) tegevusraamatud ja seotud ressursid. Tööruumi ja konto peab vastama järgmistele nõuetele.

- Lahenduse saate kasutada ainult ühe OMS tööruumi ja automatiseerimise ühe kontoga.  
- OMS tööruumi ja automatiseerimise lahenduse kasutatav konto peab olema seotud ühe teise. Mõne OMS tööruumi võib olla seotud ainult ühe automatiseerimise konto ja automatiseerimise konto võib olla seotud ainult ühe OMS tööruumi.
- Lingitud, OMS tööruumi ja automatiseerimise konto peab olema sama ressursirühm ja piirkond.  Erandiks on mõne OMS tööruumi Ida-USA piirkonnas ja ja automatiseerimise konto Ida-USA 2.

### <a name="creating-a-link-between-an-oms-workspace-and-automation-account"></a>Mõne OMS tööruumi ja automatiseerimise konto vahel lingi loomine
Kui määrate OMS tööruumi ja automatiseerimise konto sõltub installimeetod oma lahenduse.

- Kui installite Microsoft lahenduse OMS portaali kaudu, see on installitud praeguse OMS tööruumis ja pole automatiseerimise konto on nõutav.

- Azure'i turuplatsi lahenduse installimisel küsitakse OMS tööruumi ja automatiseerimise konto ja nende vahel seose on teie jaoks loodud.  

- Azure'i turuplatsi väljaspool lahendusi, tuleb linkida OMS tööruumi ja automatiseerimise konto enne installimist lahendus.  Saate seda teha iga lahenduse Azure'i turuplatsil ja klõpsake OMS tööruumi ja automatiseerimise konto.  Teil pole lahendust tegelikult installida, kuna link luuakse kohe, kui on valitud OMS tööruumi ja automatiseerimise konto.  Kui link on loodud, siis selle OMS tööruumi ja automatiseerimise konto saate kasutada mis tahes lahenduse. 

### <a name="verifying-the-link-between-an-oms-workspace-and-automation-account"></a>Kinnitatava seos on OMS tööruumi ja automatiseerimise konto
Saate kontrollida seost mõne OMS tööruumi ja automatiseerimise konto kaudu järgmist.

1. Valige konto, automatiseerimise Azure'i portaalis.
2. Liikuge **sätted** paani allservas.
3. Kui jaotis **OMS ressursid** paanil **sätted** , siis see konto on lisatud OMS tööruumi.
4. Valige **tööruumi** OMS tööruumi selle automatiseerimise kontoga seotud üksikasju soovite vaadata.


## <a name="listing-management-solutions"></a>Loetelu lahenduste haldus
Kasutada järgmist juurde kuvamiseks lingitud Azure tellimuse tööruumides lahenduste haldus.

1. Azure portaali sisse logida.
2. Valige vasakul paanil **rohkem teenuseid**.
3. Liikuge kerides allapoole jaotiseni **lahenduste** või Tippige dialoogiboksi **filtreerimine** *lahendusi* .
4. Lahenduste kõik tööruumide installitud on loetletud.

Pange tähele, et saate vaadata ainult praegune tööruumis OMS portaalis installitud Microsoft lahendusi.

## <a name="removing-a-management-solution"></a>Halduse lahenduse eemaldamine
Halduse lahenduse eemaldamisel eemaldatakse ka kõik ressursid lahendus.  

1. Leidke lahendus Azure portaali sisse, [loetledes lahenduste](#listing-solutions)protseduuri abil.
2. Valige lahenduse, mille soovite eemaldada.
3. Klõpsake nuppu **Kustuta** .

## <a name="creating-a-management-solution"></a>Halduse lahenduse loomine
Täieliku juhised halduse lahenduste loomine on saadaval [loomine lahenduste toimingud halduse komplekti (OMS)](operations-management-suite-solutions-creating.md). 


## <a name="next-steps"></a>Järgmised sammud

- Otsingu [Azure'i Kiirjuhend mallide](https://azure.microsoft.com/documentation/templates) näidised eri ressursihaldur malle.
- Saate luua oma [lahendusi](operations-management-suite-solutions-creating.md).
 