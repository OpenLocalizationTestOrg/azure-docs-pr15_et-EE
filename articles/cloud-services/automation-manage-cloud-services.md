<properties
    pageTitle="Azure'i pilveteenuste abil Azure automatiseerimine haldamine | Microsoft Azure'i"
    description="Siit saate teada, kuidas saate kasutada Azure automatiseerimine teenuse haldamiseks Azure pilveteenustega tasandil."
    services="cloud-services, automation"
    documentationCenter=""
    authors="jodoglevy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/20/2016"
    ms.author="jolevy"/>



#<a name="managing-azure-cloud-services-using-azure-automation"></a>Azure'i pilveteenuste abil Azure automatiseerimine haldamine

Käesolev juhend tutvustab teile Azure automatiseerimine teenuse ja kuidas seda saab kasutada oma Azure'i pilveteenuste haldamise lihtsustamiseks.

## <a name="what-is-azure-automation"></a>Mis on Azure automatiseerimine?

[Azure'i automaatika](https://azure.microsoft.com/services/automation/) on Azure'i teenus lihtsustamine pilve haldus – väljaprotsesside automatiseerimine. Azure'i automaatika kasutamisel pikaajalisi, käsitsi, vigu ja sageli korduvate ülesannete automatiseerida suurendamiseks töökindlust, tõhusust ja kellaaja väärtuse oma ettevõtte jaoks.

Azure'i automaatika pakub väga usaldusväärne ja tugevalt saadaval töövoo täitmise mootori, mis teie vajadustele, kui teie asutus kasvab skaala. Azure'i automaatika, protsesside saate olla käivitati käsitsi 3-osaline süsteemide või ajastatud intervalliga nii, et tööülesanded tehakse täpselt vajadusel.

Väiksemad tegevus kohal ja selle vabastamine / DevOps töötajate keskenduda töö, mis lisab business väärtus, teisaldades cloud haldustoiminguid Azure'i automaatika käivituda.


## <a name="how-can-azure-automation-help-manage-azure-cloud-services"></a>Kuidas aitab Azure automatiseerimine Azure pilveteenustega hallata?

Azure'i pilveteenustega saab hallata Azure automatiseerimine on saadaval [Azure PowerShelli tööriistad](https://msdn.microsoft.com/library/azure/jj156055.aspx)PowerShelli cmdlet-käskude abil. Azure'i automaatika on need pilvepõhise teenuse PowerShelli cmdlet-käsud saadaval välja kasti, nii, et kõik pilvepõhise teenuse haldus tööülesanded teenuses saate teha. Nende cmdlettide Azure'i automaatika koos cmdlet-käskudega muude Azure'i teenuste kogu Azure'i teenuste ja 3 peo süsteemide keerukate ülesannete automatiseerimiseks saate siduda.

Mõned näide kasutab Azure'i automaatika haldamiseks Azure'i pilveteenustega järgmised.

- [Pidev kasutuselevõtu pilveteenuses iga kord, kui cscfg või cspkg värskendatakse Azure'i bloobimälu](https://gallery.technet.microsoft.com/scriptcenter/Continuous-Deployment-of-A-eeebf3a6)
- [Rakenduste installimine pilveteenuses eksemplarid samal ajal korraga üks versiooniuuenduse domeeni](https://gallery.technet.microsoft.com/scriptcenter/Reboot-Cloud-Service-PaaS-b337a06d)

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete õppinud põhitõdesid Azure automatiseerimine ja kuidas seda saab kasutada Azure pilveteenustega haldamiseks, järgige neid linke Lisateavet Azure automatiseerimine.

- [Azure'i automaatika ülevaade](../automation/automation-intro.md)
- [Minu esimene käitusjuhendi](../automation/automation-first-runbook-graphical.md)
- [Azure'i automaatika õ kaart](https://azure.microsoft.com/documentation/learning-paths/automation/)
 
