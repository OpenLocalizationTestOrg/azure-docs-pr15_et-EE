<properties
    pageTitle="Azure'i portaali abil saate luua teatiste Azure'i teenuste jaoks | Microsoft Azure'i"
    description="Azure portaali abil saate luua Azure teatised, mis käivitab teatisi või automatiseerimise, kui teie määratud tingimustele."
    authors="rboucher"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/23/2016"
    ms.author="robb"/>

# <a name="use-azure-portal-to-create-alerts-for-azure-services"></a>Azure'i portaali abil saate luua Azure'i teenuste teatised

> [AZURE.SELECTOR]
- [Portaal](insights-alerts-portal.md)
- [PowerShelli](insights-alerts-powershell.md)
- [CLI](insights-alerts-command-line-interface.md)

## <a name="overview"></a>Ülevaade

Selles artiklis näidatakse, kuidas häälestada Azure'i portaalis Azure teatised.   

Saate teatise jälgimisega seotud mõõdikud või sündmuste oma Azure'i teenuste kohta.

- **Argumendil väärtused** - teatis päästikute, kui määratud mõõtühiku väärtus ületab läve määrate suunda. Mis on see käivitab nii kui tingimus on täidetud esmalt ja seejärel hiljem kui see tingimus on ei ole enam täidetud.    
- **Tegevuste ja sündmuste** - teatise käivitab *iga* sündmuse, või ainult siis, kui sündmuste arv ilmneda.


Saate konfigureerida tehke järgmist, kui see käivitab teatis.

- Meiliteatiste saatmine teenuseadministraator ja kaasadministraatorite
- täiendavad meilisõnumid teie määratud e-posti saata.
- kõne on webhook
- Käivitage täitmine on Azure käitusjuhendi (ainult portaalist Azure).

Saate konfigureerida ja teatiste reeglite kasutamise kohta teabe saamine

- [Azure'i portaal](insights-alerts-portal.md)
- [PowerShelli](insights-alerts-powershell.md)
- [käsurea liides (CLI)](insights-alerts-command-line-interface.md)
- [Azure'i kuvari REST API-ga](https://msdn.microsoft.com/library/azure/dn931945.aspx)


## <a name="create-an-alert-rule-on-a-metric-with-the-azure-portal"></a>Azure'i portaalis mõõdiku reegli loomine

1. Leidke [portaali](https://portal.azure.com/)ressursi olete huvitatud jälgimine ja valige see.

2. Valige jaotises jälgimine **teatiste** või **teatiste reeglid** . Teksti ja ikoon võib olla veidi erinevaid ressursse.  

    ![Jälgimine](./media/insights-alerts-portal/AlertRulesButton.png)


3. Valige käsk **Lisa teatise** ja täitke väljad.

    ![Teatise lisamine](./media/insights-alerts-portal/AddAlertOnlyParamsPage.png)

4. **Nimi** oma teatise reegel ja valige **Kirjeldus**, mis näitab ka teatise e-kirju.
5. Valige **meetermõõdustik** soovite jälgida, seejärel valige **tingimus** ja **lävi** mõõdiku väärtus. Ka valitud **perioodi** enne teatiste päästikute peab vastama argumendil reegel. Kui kasutate perioodi "PT5M" ja oma teatise otsitakse CPU üle 80%, käivitab teatise nii näiteks, kui CPU on pidevalt ülaltoodud 80% 5 minutit. Kui esineb esimese päästik, uuesti käivitab kui CPU jääb väiksem kui 80% 5 minutit. CPU mõõtühikute ilmneb iga 1 minuti.   

6. Kui soovite administraatorid ja kaasadministraatorite saadetakse kui teatise käivitub, märkige ruut **e-posti omanikud …** .

7. Kui soovite täiendavaid kirju saadetakse teile teatis, kui teatise käivitub, lisage need **täiendavad administraator email(s)** välja. Eraldamiseks semikoolonit - meiliaadressid*email@contoso.com;email2@contoso.com*

8. Kui soovite, et seda nimetatakse, kui teatise tule kehtiv URL väljale **Webhook** panna.

9. Kui kasutate Azure automatiseerimine, saate valida mõne Käitusjuhendi käivitamise teatise käivitub.

10. Klõpsake nuppu **OK** kui valmis teatise luua.   

Paari minutiga teatise on aktiivne ja käivitab eelnevalt kirjeldatud.

## <a name="managing-your-alerts"></a>Teatiste haldamine

Kui olete loonud teatise, saate selle valida ja:

- Graafik argumendil lävi ja tegelike väärtuste eelmise päeva kuvamine
- Redigeerida või kustutada.
- **Keelamine** või **lubamine** selle kui soovite, et ajutiselt peatada või selle teatise teatiste elulookirjelduse.



## <a name="next-steps"></a>Järgmised sammud

* [Ülevaate Azure jälgimine,](monitoring-overview.md) sh tüüpi teavet saate koguda ja jälgida.
* Lisateavet [teatiste seadistamine webhooks](insights-webhooks-alerts.md).
* Lisateave [Azure'i automaatika tegevusraamatud](..\automation\automation-starting-a-runbook.md).
* Siit saate [ülevaate diagnostikalogid](monitoring-overview-of-diagnostic-logs.md) ja koguda üksikasjalikku suure sagedusega mõõdikute teenust.
* Siit saate [ülevaate mõõdikute saidikogumi](insights-how-to-customize-monitoring.md) veendumaks, et teie teenus on saadaval ja reageeri.
