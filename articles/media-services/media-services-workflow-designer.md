<properties 
    pageTitle="Luua täpsema kodeering töövoogude töövoodisaineris | Microsoft Azure'i" 
    description="Lugege selle kohta, kuidas luua täpsema kodeering töövoogude töövoodisaineris." 
    services="media-services" 
    documentationCenter="" 
    authors="anilmur" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/15/2016"
    ms.author="juliako;johndeu;anilmur"/>


#<a name="create-advanced-encoding-workflows-with-workflow-designer"></a>Töövoo kujundaja täpsemalt kodeering töövoogude loomine

##<a name="overview"></a>Ülevaade

**Töövoodisaineris** on Windowsi töölaua tööriist, mida kasutatakse kujundada ja koostada kodeerimine **Media kodeerija Premium**töövoo jaoks kohandatud töövoogude.
Power töövoo kujundaja tööriista abil saate kujundada ja keerukate töövoogude **Media kodeerija Premium**töötavad loomine.  

Töövood võivad hõlmata kliendi otsust loogika ja harude Sisestuskeel lähtefail atribuutide alusel. Saate luua töövood overridable atribuudid ja dünaamiline väärtused, et muuta ka keerukamaid kodeering ülesannetega lihtne korrata ja kohandada pilveteenuses.

Näiteks saate luua töövood on järgmised.

- Otsus kontrolli andmeallika sisu eraldusvõimet ja kodeerida ainult soovitud väljund jälitab töövood.  See on helfpul kõrvaldamise raisatud lugusid, mis tekitaks upscaling andmeallika sisu inadvertantly.
- Pealdiste, ülekatete ja õmblemisega koos sisu saab kasutada mitut Sisestuskeel faili. 

See tööriist saate kasutada ka muuta ühte meie [avaldatud töövood](media-services-workflow-designer.md#existing_workflows). 

>[AZURE.NOTE]Tööriista töövoodisaineris koopia saamiseks pöörduge mepd@microsoft.com.


Pärast töövoo fail on loodud, saate üles laadida aktiva ja seejärel kasutada kodeeringut meediumifaile. Kodeerida **Media kodeerija Premium töövoo** kasutades **.net-i**kohta leiate teemast [Täpsemad kodeerimine Media kodeerija Premium töövooga](media-services-encode-with-premium-workflow.md).

##<a id="existing_workflows"></a>Muutke olemasoleva töövood

Vaikimisi [avaldatud töövoogude](media-services-workflow-designer.md#existing_workflows) saab tööriista kujundaja abil muuta. Saate vaikimisi töövoo failid [siia](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows). See kaust sisaldab ka need failid kirjeldust.

Järgmisi videoid näitavad, kuidas kasutada designer.

###<a name="day-1--getting-started"></a>Päev 1 – alustamine

Päeva 1 video hõlmab:

- Kujundaja ülevaade
- Tavaline töövoogude – "Tere, maailm"
- Mitme loomise väljundi MP4 faili Azure Media Servicesi Streaming kasutamiseks

> [AZURE.VIDEO azure-premium-encoder-workflow-designer-training-videos-day-1]

###<a name="day-2"></a>2 päeva

2 päeva video hõlmab:

- Erineva andmeallika faili stsenaariumid – töötlemise heli
- Täiustatud loogika töövood
- Graafik etappi

> [AZURE.VIDEO azure-premium-encoder-workflow-designer-training-videos-day-2]

###<a name="day-3"></a>3 päeva

3 päeva video hõlmab:

- Skriptimise töövoogude/jooniste sees
- Praeguse kodeerija piirangud
- K & v
 
> [AZURE.VIDEO azure-premium-encoder-workflow-designer-training-videos-day-3]


## <a name="next-step"></a>Järgmise juhise juurde

Vaadake üle Media Servicesi õppeteemad.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


Kui vajate toetavad või on loomise tööriista kujundaja töövoo kohandatud töövoogude kohta küsimusi, saatke meile e-posti mepd@microsoft.com.

##<a name="see-also"></a>Vt ka

[Azure'i Premium kodeerija töövoo kujundaja Õppevideod](http://johndeutscher.com/2015/07/06/azure-premium-encoder-workflow-designer-training-videos/)
