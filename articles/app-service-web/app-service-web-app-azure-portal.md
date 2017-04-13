<properties
    pageTitle="Viide Azure portaali navigeerimiseks"
    description="Siit saate teada, erinevate kasutuskogemuse Web Appi teenuse portaalis haldus ja Azure portaali vahel"
    services="app-service"
    documentationCenter=""
    authors="jaime-espinosa"
    manager="wpickett"
    editor="jimbe"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/26/2016"
    ms.author="jaime-espinosa"/>

# <a name="reference-for-navigating-the-azure-portal"></a>Viide Azure portaali navigeerimiseks

Azure'i veebisaite nimetatakse nüüd [Rakenduse teenuse Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714). Uuendame kogu meie dokumentatsiooni nimi selle muudatuse kajastamiseks ja Azure portaali juhised. Seni, kuni see toiming on lõpule jõudnud, saate selle dokumendi juhendina töötamiseks veebirakenduste Azure'i portaalis.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 
 
## <a name="the-future-of-the-azure-classic-portal"></a>Azure'i klassikaline portaali tulevikus

Kui te teate tootemargi muudatused Azure'i klassikaline portaalis, selle portaal on asendatakse Azure'i portaal. Klassikaline portaalis on järk-järgult, uue arengu liikumine on minnes Azure'i portaal. Azure'i portaalis tulevad kõigi uute funktsioonide Web Apps. Azure'i portaal ära uusimaid ja parimaid, et pakkuda on Web Appsi kasutamise alustamine.

## <a name="layout-differences-between-the-azure-classic-portal-and-azure-portal"></a>Azure'i klassikaline portaali ja Azure portaali paigutus erinevused

Klassikaline portaalis Azure teenuste on loetletud vasakul pool. Navigeerimise klassikaline portaalis järgib puustruktuuris, kus teenuse käivitamine ja liikuge rakendusse iga element. See struktuur töötab ka siis, kui haldamine sõltumatu komponendid. Siiski loodud Azure on ühendatud teenusekomplekt ja selle puustruktuuris pole töötamiseks nende teenuste optimaalne. 

Azure'i portaalis lihtne koostada rakenduste end-to-end mitme teenuste komponendid. Portaali on korraldatud *vedudena*. *Reisi* on *labad*, mis on erinevate osade ümbriste sarja. Näiteks, häälestada automaatse skaleerimist web app on *reisi* , mis suunab teid mitu labad, nagu on näidatud järgmises näites: **veebisaidi** tera (et tera tiitel pole veel värskendatud kasutada uusi termineid), tera **sätted** ja **skaala välja** tera. Näites muutmine seatakse sõltub CPU hõivatus, seega pole **CPU protsent** tera. Komponendid sees *labad* nimetatakse *osad*, mis näevad paanid. 

![](./media/app-service-web-app-azure-portal/AutoScaling.png)

## <a name="navigation-example-create-a-web-app"></a>Navigeerimise näide: luua veebirakenduse

Uute veebirakenduste loomine on endiselt sama lihtne kui 1-2-3. Järgmisel pildil on kujutatud klassikaline portaali ja portaali-kõrvuti näidata, ei ole palju muutunud juhiseid vaja saada web appi ja töötab arv. 

![](./media/app-service-web-app-azure-portal/CreateWebApp.png)

Portaalis saate valida veebirakenduste, sh populaarsed Galerii rakendusi nagu WordPress levinumat tüüpi. Saadaolevate rakenduste täieliku loendi leiate [Azure'i turuplatsi].

Kui loote web appi, teie määratud URL-i, rakenduse teenusleping ja asukoht, just nagu klassikaline portaalis portaalis. 

![](./media/app-service-web-app-azure-portal/CreateWebAppSettings.png)

Lisaks portaali võimaldab määratleda muud levinud sätted. Näiteks [Ressursirühma](../azure-resource-manager/resource-group-overview.md) oleks lihtne, saate kuvada ja hallata Azure seotud ressursid. 

## <a name="navigation-example-settings-and-features"></a>Navigeerimise näide: sätete ja funktsioonide

Sätete ja funktsioonide on nüüd loogiliselt rühmitatud ühe tera, kust saate liikuda.

![](./media/app-service-web-app-azure-portal/WebAppSettings.png)

Näiteks saate luua kohandatud domeene, klõpsates nuppu **kohandatud domeenid ja SSL-i** **sätted** tera.

![](./media/app-service-web-app-azure-portal/ConfigureWebApp.png)

Häälestada jälgimisega seotud teatis, klõpsake **taotlusi ja tõrgete** ja seejärel **Lisa teate**.

![](./media/app-service-web-app-azure-portal/Monitoring.png)

Diagnostika lubamiseks klõpsake **diagnostika logid** tera **sätted** .

![](./media/app-service-web-app-azure-portal/Diagnostics.png)
 
Klõpsake rakenduse sätete konfigureerimiseks **sätted** tera **rakenduse sätted** . 

![](./media/app-service-web-app-azure-portal/AppSettingsPreview.png)

Peale tootenime, paari asja portaalis on ümber nimetatud või rühmitatud erinevalt lihtsam oleks neid lihtsam leida. Allpool on näiteks vastavat lehte rakenduse kuvatõmmis klassikaline portaalis sätted (**konfigureerimine**).

![](./media/app-service-web-app-azure-portal/AppSettings.png)

## <a name="more-resources"></a>Veel ressursse

[Azure Portal]: https://portal.azure.com
[Azure'i turuplats]: /marketplace/

>[AZURE.NOTE] Kui soovite alustada Azure'i rakendust Service enne Azure'i konto kasutajaks, minge [Proovige rakenduse teenus](http://go.microsoft.com/fwlink/?LinkId=523751), kus saate kohe luua lühiajaline starter web app rakenduse teenus. Nõutav; krediitkaardid kohustusi.

## <a name="whats-changed"></a>Mis on muutunud
* Muuda juhend veebisaitide rakenduse teenusega leiate: [Azure'i rakendust Service ja selle mõju olemasoleva Azure'i teenused](http://go.microsoft.com/fwlink/?LinkId=529714)
 
