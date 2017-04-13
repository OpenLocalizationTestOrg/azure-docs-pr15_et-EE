<properties
   pageTitle="Azure'i pilves rollide haldamine teenuste projektidega töötamiseks Visual Studio | Microsoft Azure'i"
   description="Saate teada, kuidas soovite lisada uue rollid Azure pilveteenuse teenuse projekti või eemaldada see olemasoleva rollid Visual Studio abil."
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

# <a name="managing-roles-in-the-azure-cloud-services-projects-with-visual-studio"></a>Azure'i cloud services projektide Visual Studio rollide haldamine

Pärast loomist Azure pilveteenuse teenuse projekti, saate uute rollide lisamine või olemasolevate rollide eemaldamine. Saate importida olemasoleva projekti ja teisendage see rollid. Näiteks saate importida ka ASP.net-i veebirakenduse ja web rollile määrata.

## <a name="adding-or-removing-roles"></a>Lisamise või eemaldamise rollid

**Lisada rollile**

**Solution Exploreris**oma pilveteenuses projektis **rollid** sõlme kiirmenüü avamine ja valige **Lisa**. Saate valida olemasoleva web rolli või töötaja rolli praegusest lahendusest või luua uue veebis või töötaja rolli projekti. Või valige vastav projekt, nt ASP.net-i web rakenduse project ja seostada projekti roll.

**Roll seos eemaldamine**

Teenus cloud projekti Solution Exploreris sõlme **rollid** , rolli ja valige **Eemalda**kiirmenüü avamine.

## <a name="removing-and-adding-roles-in-your-cloud-service"></a>Eemaldades ja lisades oma pilveteenuses rollid

Kui teenus cloud projekti rolli eemaldamine, kuid otsustate hiljem roll uuesti lisada projekti, lisatakse ainult rolli deklareerimise ja põhilised atribuudid, lõpp-punktid ja diagnostika teave, näiteks. ServiceDefinition.csdef faili või ServiceConfiguration.cscfg fail on lisatud pole lisaressursid või viited. Kui soovite selle teabe lisada, peate selle käsitsi lisada need failidesse.

Näiteks eemaldate võib web teenuse roll ja otsustate hiljem lisada selle rolli tagasi teie lahendus. Kui te seda teete, ilmneb tõrge. Selleks, et see tõrge, peate lisama selle `<LocalResources>` element, mis on näidatud järgmises XML-i uuesti sisse ServiceDefinition.csdef faili. Kasutage nimi, lisatud projekt taas atribuut name osana teenuse osa on **<LocalStorage>** element. Selles näites on web teenuse roll nimeks **WCFServiceWebRole1**.

    <WebRole name="WCFServiceWebRole1">
        <Sites>
          <Site name="Web">
            <Bindings>
              <Binding name="Endpoint1" endpointName="Endpoint1" />
            </Bindings>
          </Site>
        </Sites>
        <Endpoints>
          <InputEndpoint name="Endpoint1" protocol="http" port="80" />
        </Endpoints>
        <Imports>
          <Import moduleName="Diagnostics" />
        </Imports>
       <LocalResources>
          <LocalStorage name="WCFServiceWebRole1.svclog" sizeInMB="1000" cleanOnRoleRecycle="false" />
       </LocalResources>
    </WebRole>

## <a name="next-steps"></a>Järgmised sammud

Teada, kuidas konfigureerida rollid Visual Studio abil lugemise [konfigureerimine rollide jaoks Azure'i pilveteenuses, Visual Studio abil](vs-azure-tools-configure-roles-for-cloud-service.md).
