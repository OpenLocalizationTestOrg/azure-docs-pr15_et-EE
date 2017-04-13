<properties 
    pageTitle="Logimisel sündmuste Azure'i sündmuse jaoturi Azure'i API Management | Microsoft Azure'i" 
    description="Saate teada, kuidas logige sündmuste Azure'i sündmuse jaoturi Azure'i API haldamine." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-log-events-to-azure-event-hubs-in-azure-api-management"></a>Kuidas sündmused logida Azure'i sündmuse jaoturi Azure'i API haldamine

Azure'i sündmuse jaoturi on väga paindlik andmete sissepääsu teenus, mis võivad neelata sündmuste sekundis miljoneid nii, et saate töötlemine ja analüüsida suurel hulgal ühendatud seadmete ja rakenduste andmed. Sündmuse jaoturi toimib "esiuksest" jaoks soovitud sündmus müügivõimaluste ja kui andmeid kogutakse jaoturiga mõni sündmus, see saab muuta ja mis tahes reaalajas analytics pakkuja või partiide salvestusruumi adapterit abil salvestatud. Sündmuse jaoturi decouples voo sündmuste sündmused tarbimine koostamine sündmuse tarbijad pääseks juurde oma ajakava sündmused.

Selles artiklis on [Integreerida Azure'i API haldus – sündmuse jaoturi](https://azure.microsoft.com/documentation/videos/integrate-azure-api-management-with-event-hubs/) video lisamine companion ja kirjeldatakse, kuidas logida halduse API sündmuste Azure'i sündmuse jaoturi abil.

## <a name="create-an-azure-event-hub"></a>Mõne Azure sündmuse keskuse loomine

Uue sündmuse keskuse loomiseks sisselogimine [Azure klassikaline portaali](https://manage.windowsazure.com) ja klõpsake nuppu **Uus**->**Rakenduse teenuste**->**Teenuse siini**->**Sündmuse jaoturi**->**Kiiresti luua**. Sisestage sündmuse jaoturi nimi, piirkond, valige soovitud tellimus ja valige nimeruumi. Kui te pole varem loonud nimeruumi, tippides nimi tekstiväljale **Namespace** saate luua. Kui kõik atribuudid on konfigureeritud, klõpsake nuppu **Loo uus sündmus jaoturi** sündmuse keskuse loomiseks.

![Sündmuse keskuse loomine][create-event-hub]

Järgmiseks oma uue sündmuse jaoturi **konfigureerimine** menüüs liikumiseks ja looge kaks **ühiskasutusega juurdepääsupoliitikaid**. Esimene **saatmine** nime ja anda **saatmise** õigused.

![Poliitika saatmine][sending-policy]

Nime teine **vastu**, anda sellele **kuulata** õigused ja klõpsake nuppu **Salvesta**.

![Poliitika vastuvõtmine][receiving-policy]

Iga ühiskasutusega juurdepääs lubab saata ja vastu võtta sündmused ja sealt sündmuse jaoturi rakendused. Ühendusstringi jaoks need poliitikad juurdepääsu liikuda **armatuurlaua** vahekaarti sündmus jaoturi ja klõpsake **ühenduse teabe**.

![Ühendusstring][event-hub-dashboard]

Ühendusstringi **saatmine** kasutatakse sündmuste logimise ja ühendusstringi **vastuvõtmine** kasutatakse sündmuste allalaadimisel sündmuse keskuse kaudu.

![Ühendusstring][event-hub-connection-string]

## <a name="create-an-api-management-logger"></a>Mõne API halduse puuraidur loomine

Nüüd, kui teil on mõni sündmus jaoturi, järgmise sammuna tuleb konfigureerida [puuraidur](https://msdn.microsoft.com/library/azure/mt592020.aspx) API halduse teenust, nii, et see sündmus jaoturi sisse logida sündmused.

API halduse logeri on konfigureeritud, kasutades [API haldamine REST API](http://aka.ms/smapi). Enne REST API kasutamine esimest korda, vaadake [eeltingimused](https://msdn.microsoft.com/library/azure/dn776326.aspx#Prerequisites) ja veenduge, et teil on [lubatud juurdepääs REST API -ga](https://msdn.microsoft.com/library/azure/dn776326.aspx#EnableRESTAPI).

Puuraidur loomiseks tehke HTTP sellele taotluse järgmist malli URL-i abil.

    https://{your service}.management.azure-api.net/loggers/{new logger name}?api-version=2014-02-14-preview

-   Asendage `{your service}` koos API halduse teenuse eksemplari nimi.
-   Asendage `{new logger name}` oma uue puuraidur jaoks soovitud nimi. See nimi on viide, kui konfigureerite [log-eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) poliitika

Järgmised päiste lisamine kutse.

-   Sisutüüp: rakenduse/json
-   Luba: SharedAccessSignature uid =...
    -   Juhised loomisel on `SharedAccessSignature` leiate [Azure'i API halduse REST API autentimist](https://msdn.microsoft.com/library/azure/dn798668.aspx).

Saate määrata järgmised malli abil koosolekukutse sisusse.

    {
      "type" : "AzureEventHub",
      "description" : "Sample logger description",
      "credentials" : {
        "name" : "Name of the Event Hub from the Azure Classic Portal",
        "connectionString" : "Endpoint=Event Hub Sender connection string"
        }
    }

-   `type`peab olema seatud `AzureEventHub`.
-   `description`valikuline kirjeldus logija pakub ja null stringi pikkus võib olla, kui soovitud.
-   `credentials`sisaldab selle `name` ja `connectionString` oma Azure'i sündmuse keskuse.

Kui teete taotluse, kui luuakse logija olekukoodi `201 Created` tagastatakse. 

>[AZURE.NOTE] Muud võimalikud saatja koodid ja nende põhjused, leiate teemast [loomine puuraidur](https://msdn.microsoft.com/library/azure/mt592020.aspx#PUT). Et näha, kuidas muid toiminguid, näiteks loendi, värskendamine ja kustutamine, leiate [puuraidur](https://msdn.microsoft.com/library/azure/mt592020.aspx) üksuse dokumentatsioonist.

## <a name="configure-log-to-eventhubs-policies"></a>Log-eventhubs poliitikate konfigureerimine

Kui teie puuraidur on konfigureeritud API haldus, saate konfigureerida oma log-eventhubs poliitikaid rakendada soovitud sündmuste logi. Jaotises sissetuleva meili poliitika või jaotises Väljamineva meili poliitika saab log-eventhubs poliitika.

Poliitikate [Azure klassikaline portaali](https://manage.windowsazure.com)sisse logima konfigureerimine liikumine API halduse teenust ja klõpsake **Publisheri portaali** või **haldamine** , et Publisheri portaali.

![Publisheri portaal][publisher-portal]

Klõpsake **poliitikate** API halduse menüü vasakul, valige soovitud toote- ja API ja klõpsake nuppu **Lisa poliitika**. Selles näites me lisatava poliitika **Kaja API** **piiramatu** toote.

![Teabehalduspoliitika lisamine][add-policy]

Paigutage kursor on soovitud `inbound` jaotises poliitika ja klõpsake nuppu **Logi sisse EventHub** poliitika lisamiseks soovitud `log-to-eventhub` poliitika aruande malli.

![Poliitika redaktor][event-hub-policy]

    <log-to-eventhub logger-id ='logger-id'>
      @( string.Join(",", DateTime.UtcNow, context.Deployment.ServiceName, context.RequestId, context.Request.IpAddress, context.Operation.Name))
    </log-to-eventhub>

Asendage `logger-id` konfigureeritud eelmises etapis API halduse logija nimi.

Saate kasutada mis tahes avaldis, mis tagastab stringi väärtusena soovitud `log-to-eventhub` element. Selles näites on sisse logitud string, mis sisaldab kuupäeva ja kellaaja, teenuse nimi, päringu-id, taotluse ip-aadress ja toimingu nimi.

Klõpsake nuppu **Salvesta** värskendatud poliitika konfiguratsiooni salvestamiseks. Kui see on salvestatud poliitika on aktiivne ja määratud sündmuse jaoturi logitakse sündmused.

## <a name="next-steps"></a>Järgmised sammud

-   Lisateave Azure'i sündmuse jaoturi
    -   [Azure'i sündmuse jaoturi kasutamise alustamine](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
    -   [Sõnumeid, mille EventProcessorHost](../event-hubs/event-hubs-csharp-ephcs-getstarted.md#receive-messages-with-eventprocessorhost)
    -   [Sündmuse jaoturi programmeerimisega juhend](../event-hubs/event-hubs-programming-guide.md)
-   Lisateavet API haldus ja sündmuse jaoturi integreerimine
    -   [Puuraidur üksuse viide](https://msdn.microsoft.com/library/azure/mt592020.aspx)
    -   [log-eventhub poliitika viide](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub)
    -   [Azure'i API haldus, sündmuse jaoturi ja Runscope oma API jälgimine](api-management-log-to-eventhub-sample.md)    

## <a name="watch-a-video-walkthrough"></a>Vaadake video lühiülevaade

> [AZURE.VIDEO integrate-azure-api-management-with-event-hubs]


[publisher-portal]: ./media/api-management-howto-log-event-hubs/publisher-portal.png
[create-event-hub]: ./media/api-management-howto-log-event-hubs/create-event-hub.png
[event-hub-connection-string]: ./media/api-management-howto-log-event-hubs/event-hub-connection-string.png
[event-hub-dashboard]: ./media/api-management-howto-log-event-hubs/event-hub-dashboard.png
[receiving-policy]: ./media/api-management-howto-log-event-hubs/receiving-policy.png
[sending-policy]: ./media/api-management-howto-log-event-hubs/sending-policy.png
[event-hub-policy]: ./media/api-management-howto-log-event-hubs/event-hub-policy.png
[add-policy]: ./media/api-management-howto-log-event-hubs/add-policy.png






