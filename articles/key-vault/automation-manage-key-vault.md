<properties
    pageTitle="Azure'i klahvi Vault Azure automatiseerimine abil hallata | Microsoft Azure'i"
    description="Siit saate teada, kuidas Azure'i klahvi Vault haldamiseks saate kasutada Azure automatiseerimine teenuse."
    services="Key-Vault, automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/29/2016"
    ms.author="magoedte;csand"/>

#<a name="managing-azure-key-vault-using-azure-automation"></a>Azure'i klahvi Vault abil Azure automatiseerimine haldamine

Käesolev juhend tutvustab teile Azure automatiseerimine teenuse ja kuidas seda saab kasutada oma võtmed ja Azure klahvi võlvkelder saladusi haldamise lihtsustamiseks.

## <a name="what-is-azure-automation"></a>Mis on Azure automatiseerimine?

[Azure'i automaatika](../automation/automation-intro.md) on Azure'i teenus lihtsustamine pilve haldus väljaprotsesside automatiseerimine ja soovitud olek konfigureerimise kaudu. Azure'i automaatika, käsitsi, kasutades korrata, pikaajalisi ja vigu tööülesannete automatiseerida suurendamiseks töökindlust, tõhusust ja kellaaja väärtuse oma ettevõtte jaoks.

Azure'i automaatika pakub väga usaldusväärne, mis on väga saadaval töövoo täitmise mootori, mis teie vajadustele skaala. Azure'i automaatika, protsesside saate olla käivitati käsitsi, 3-osaline süsteemide või ajastatud intervalliga nii, et tööülesanded tehakse täpselt vajadusel.

Vähendage funktsionaalseid pea kohal ja vabastage see ja DevOps töötajate keskenduda töö, mis lisab väärtusega, teisaldades cloud haldustoiminguid Azure'i automaatika käivituda.


## <a name="how-can-azure-automation-help-manage-azure-key-vault"></a>Kuidas aitab Azure automatiseerimine haldamine Azure'i klahvi Vault?

Klahv Vault saab hallata Azure automatiseerimine AzureRM klahvi Vault cmdlettide abil (https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) ja [Azure klassikaline klahvi Vault cmdlet-käsud](https://msdn.microsoft.com/library/azure/dn868052.aspx). Azure'i mooduli haldamise klassikaline klahvi Vault on automaatselt saadaval Azure automatiseerimine ja saate importida [AzureRM-KeyVault mooduli](https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) Azure'i automaatika, nii, et saate teha paljude klahvi Vault haldustoiminguid teenuses. Nende cmdlettide Azure'i automaatika koos cmdlet-käskudega muude Azure'i teenuste Azure'i teenuste ja 3 tootja süsteemid üle keerukate ülesannete automatiseerimiseks saate siduda.

Azure'i klahvi Vault cmdlet-käskude abil saate teha hulgas järgmised toimingud: 

- Loomine ja konfigureerimine võtme vault
- Loomise või importimise klahvi
- Loomine ja värskendamine salajase
- Klahvi atribuutide värskendamine
- Klahv või salajane
- Klahv või salajane kustutamine

Siin on mõned näited PowerShelli kaudu haldamise klahvi Vault:  

* [Azure'i võtme Vault - samm-sammult](https://blogs.technet.microsoft.com/kv/2015/06/02/azure-key-vault-step-by-step)
* [Häälestamise ja konfigureerimise on Azure võtme Vault](https://www.simple-talk.com/cloud/platform-as-a-service/setting-up-and-configuring-an-azure-key-vault)


## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete õppinud põhitõdesid Azure automatiseerimine ja kuidas seda saab kasutada Azure klahvi Vault haldamiseks, järgige neid linke Lisateavet Azure automatiseerimine.

* Teemast Azure automatiseerimine [Kuueosalisest alustamine](../automation/automation-first-runbook-graphical.md).
* Leiate [Azure'i klahvi Vault PowerShelli skriptide](https://gallery.technet.microsoft.com/scriptcenter/site/search?query=azure%20key%20vault&f%5B0%5D.Value=azure%20key%20vault&f%5B0%5D.Type=SearchText&ac=5).
