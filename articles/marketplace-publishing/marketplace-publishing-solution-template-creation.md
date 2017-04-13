<properties
   pageTitle="Loomise lahendus malli turuplatsil juhend | Microsoft Azure'i"
   description="Üksikasjalikud juhised, kuidas luua, kinnitamine ja juurutada mitme-VM pilt lahenduse malli osta Azure'i turuplatsilt."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

   <tags
      ms.service="marketplace"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="na"
      ms.date="07/27/2016"
      ms.author="hascipio; v-divte" />

# <a name="guide-to-create-a-solution-template-for-azure-marketplace"></a>Teejuht: Azure'i turuplatsi lahenduse malli loomine
Pärast [konto loomine ja registreerimise]punkti 1;[link-acct-creation], me juhendamisega saate veebisaidil [tehnilise eeltingimuste lahenduse malli loomine](marketplace-publishing-solution-template-creation-prerequisites.md)Azure ühilduv lahenduse malli loomine. Nüüd kuvatakse tutvustame teile juhised lahenduse malli loomine mitme vms [Avaldamisportaal] [ link-pubportal] Azure'i turuplatsil.

## <a name="create-your-solution-template-offer-in-the-publishing-portal"></a>Teie lahendus malli pakkumise avaldamise portaali loomine
Avage [https://publish.windowsazure.com](http://publish.windowsazure.com). Kui logite sisse esimest korda [Avaldamisportaal](https://publish.windowsazure.com/), kasutage sama kontoga, mis on registreeritud oma ettevõtte müüja profiili. Hiljem saate lisada co admin avaldamise portaalis teie ettevõtte töötaja.

### <a name="1-select-solution-templates"></a>1. Valige "Lahenduse Mallid"

  ![Joonis][img-pubportal-menu-sol-templ]

### <a name="2-create-a-new-solution-template"></a>2 lahenduse uue malli loomine

  ![Joonis][img-pubportal-sol-templ-new]

### <a name="3-start-with-topologies"></a>3. topoloogiatest alustamine
Lahendus mall on "parent" kõik selle topoloogiatest. Saate määratleda mitme topoloogiatest ühe pakkumise/lahenduse malli. Kui soovite lavastus vajutamist pakkumise, on see lükata kõik selle topoloogiatest. Allpool teie pakkumise määratlemiseks tehke järgmist.     

- Looge topoloogia: "Topoloogia identifikaator" on tavaliselt topoloogia lahenduse malli nimi. URL-i kasutatakse topoloogia identifikaator, nagu allpool näidatud:

  Azure'i turuplats: http://azure.microsoft.com/marketplace/partners/ {PublisherNamespace} / {OfferIdentifier} {TopologyIdentifier}

  Azure'i portaal: https://portal.azure.com/#gallery/ {PublisherNamespace}. {OfferIdentifier} {TopologyIdentifier}

- Lisage uus versioon.

### <a name="4-get-your-topology-versions-certified"></a>4. oma sertifitseeritud topoloogia versiooni hankimine
Laadige zip-fail, mis sisaldab kõiki nõutud faile ettevalmistamise topoloogia selle konkreetse versiooni. Zip-fail peab sisaldama järgmist:

- selle juurkaust *mainTemplate.json* ja *createUiDefinition.json* faili.
- Mis tahes lingitud Mallid ja kõiki vajalikke skriptide.

  > [AZURE.TIP] Kui teie arendajate tööd loomise lahendus malli topoloogiatest ja nende sertifitseeritud, äri, turundus, ja/või juriidilise osakondade teie ettevõtte töö turundus- ja sisu.

## <a name="next-steps"></a>Järgmised sammud
Nüüd, kui olete loonud malli lahenduse ja üles laadida zip-fail, järgige juhiseid teemas [turuplatsi turundusmaterjalide sisu juhend](marketplace-publishing-push-to-staging.md) enne lükkamine lavastus pakkuda. Täiskomplekti turuplatsi avaldamise artiklitest leiate [Alustamine: Kuidas avaldada pakkumise Azure'i turuplatsi](marketplace-publishing-getting-started.md).

Teile võib huvi pakkuda ka neid seotud artikleid:

- VM pildid: [Kohta virtuaalse masina pilte Azure](https://msdn.microsoft.com/library/azure/dn790290.aspx)
- VM laiendid: [VM Agent ja VM laiendid ülevaade](https://msdn.microsoft.com/library/azure/dn832621.aspx) ja [Azure VM laiendid ja -funktsioonid](https://msdn.microsoft.com/library/azure/dn606311.aspx)
- Azure'i ressursihaldur: [Azure'i ARM mallide koostamine](../resource-group-authoring-templates.md) ja [lihtsat ARM malli näited](https://github.com/rjmax/ArmExamples)
- Salvestusruumi konto ahendab: [Kuidas jälgida salvestusruumi konto pidurdamise](http://blogs.msdn.com/b/mast/archive/2014/08/02/how-to-monitor-for-storage-account-throttling.aspx) ja [Premium salvestusruum](../storage/storage-premium-storage.md#scalability-and-performance-targets-when-using-premium-storage)

[img-pubportal-menu-sol-templ]:media/marketplace-publishing-solution-template-creation/pubportal-menu-solution-templates.png
[img-pubportal-sol-templ-new]:media/marketplace-publishing-solution-template-creation/pubportal-solution-template-new.png
[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
[link-pubportal]:https://publish.windowsazure.com
