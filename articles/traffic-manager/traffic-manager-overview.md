<properties
    pageTitle="Mis on liikluse haldur | Microsoft Azure'i"
    description="See artikkel aitab teil mõista, mis on liikluse haldur ja kas see on õige liikluse marsruutimise valik rakenduse"
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="overview-of-traffic-manager"></a>Liikluse haldur ülevaade

Microsoft Azure'i liikluse haldur võimaldab teil määrata jaotuse kasutaja suunatakse eri andmekeskuste teenuse lõpp-punktid. Teenuse lõpp-punktid liikluse haldur toetab kaasata Azure VMs veebirakenduste, ning pilveteenustega. Samuti saate liikluse haldur abil välise, Azure'i lõpp-punktid.

Liikluse haldur kasutab Domain Name System (DNS) otse kliendi taotluste kõige paremini lõpp-punkti [meetod liikluse marsruutimist](traffic-manager-routing-methods.md) ja seisundi lõpp-punktid. Liikluse haldur pakub mitmesuguseid eri rakenduse vajadustele, lõpp-punkti seisundi [jälgimine](traffic-manager-monitoring.md)ja automaatne Tõrkesiirde meetodite liikluse marsruutimist. Liikluse Manager on olles tõrge, sh kogu Azure piirkonna tõrge.

## <a name="traffic-manager-benefits"></a>Liikluse haldur eelised

Liikluse haldur aitavad teil.

- **Parandada kriitiliste rakenduste kättesaadavus**

    Liikluse haldur pakub kõrge-saadavus oma rakenduste jälgimine lõpp-punktide ning esitada automaatselt Tõrkesiirde kui lõpp läheb alla.

- **Suure jõudlusega rakenduste tundlikkuse täiustada**

    Azure'i võimaldab käivitada andmekeskuste asub maailma pilveteenustega või veebisaiti. Liikluse haldur parandab rakenduse tundlikkuse suunavad liiklust lõpp-punkti koos madalaimate Võrgu latentsuse kliendi jaoks.

- **Teenuse hooldustööde ilma tööseisakute teha**

    Te saate toiminguid kavandatud hooldustööde rakenduste ilma tööseisakute. Hoolduse ajal, suunab liikluse haldur liikluse Alternatiivne lõpp-punktid.

- **Kohapealse ja pilvepõhise rakenduste ühendamine**

    Liikluse haldur toetab väline, mis võimaldab seda kasutada koos hübriid-Azure'i lõpp-punktid cloud ja kohapealse juurutuse, sh "burst-et-pilves," "migreerimine-to-cloud," ja "Tõrkesiirde cloud" stsenaariumid.

- **Suur, keerukate juurutuste liikluse levitamine**

    [Pesastatud liikluse haldur profiilide](traffic-manager-nested-profiles.md)kasutamisel saab kombineerida meetodite liikluse marsruutimist suurema ja keerukamaid juurutuste vajadustele keerukaid ja paindlikus reeglite loomiseks.

[AZURE.INCLUDE [load-balancer-compare-tm-ag-lb-include.md](../../includes/load-balancer-compare-tm-ag-lb-include.md)]

## <a name="next-steps"></a>Järgmised sammud

- Lugege lisateavet [liikluse haldur tööpõhimõtete](traffic-manager-how-traffic-manager-works.md)kohta.

- Saate teada, kuidas töötada kõrge-saadavus rakenduste abil [liikluse haldur lõpp-punkti jälgimine](traffic-manager-monitoring.md).

- Lisateave [meetodite liikluse marsruutimist](traffic-manager-routing-methods.md) liikluse haldur toetab.

- [Loo liikluse haldur profiili](traffic-manager-manage-profiles.md).

