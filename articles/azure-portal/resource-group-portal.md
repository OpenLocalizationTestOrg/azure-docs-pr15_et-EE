<properties 
    pageTitle="Azure'i portaali abil saate hallata Azure ressursid | Microsoft Azure'i" 
    description="Azure'i portaal ja Azure ressursside haldamine abil saate hallata oma ressursse. Näitab, kuidas töötada armatuurlaudade ressursid jälgimiseks." 
    services="azure-resource-manager,azure-portal" 
    documentationCenter="" 
    authors="tfitzmac" 
    manager="timlt" 
    editor="tysonn"/>

<tags 
    ms.service="azure-resource-manager" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="tomfitz"/>

# <a name="manage-azure-resources-through-portal"></a>Azure'i ressursid portaali kaudu hallata

> [AZURE.SELECTOR]
- [Azure'i PowerShelli](../powershell-azure-resource-manager.md)
- [Azure'i CLI](../xplat-cli-azure-resource-manager.md)
- [Portaal](resource-group-portal.md) 
- [REST API-GA](../resource-manager-rest-api.md)

Selles teemas kirjeldatakse, kuidas kasutada [Azure portaali](https://portal.azure.com) [Azure'i ressursihaldur](../azure-resource-manager/resource-group-overview.md) hallata oma Azure ressursse. Portaali kaudu ressursse rakendades kohta leiate teemast [Deploy ressursid ressursihaldur mallide ja Azure portaali](../resource-group-template-deploy-portal.md).

Praegu pole igal teenusel toetab portaali või ressursihaldur. Nende teenuste jaoks peate kasutama [klassikaline portaalis](https://manage.windowsazure.com). Iga teenuse olekut leiate [Azure'i portaali-saadavus diagrammi](https://azure.microsoft.com/features/azure-portal/availability/).

## <a name="manage-resource-groups"></a>Ressursi rühmade haldamiseks

1. Ressursi rühmade tellimuse vaatamiseks valige **Ressursi rühmad**.

    ![Sirvige ressursi rühmad](./media/resource-group-portal/browse-groups.png)

1. Ka tühjad ressursikeskuse rühma loomiseks valige **Lisa**.

    ![ressursirühm lisamine](./media/resource-group-portal/add-resource-group.png)

1. Sisestage nimi ja asukoht uue ressursirühma. Valige **Loo**.

    ![ressursi rühma loomine](./media/resource-group-portal/create-empty-group.png)

1. Võib-olla peate valima **värskendamine** kuvamiseks vastloodud ressursirühma.

    ![ressursirühm värskendamine](./media/resource-group-portal/refresh-resource-groups.png)

1. Kuvatakse teie ressursi rühma teabe kohandamiseks valige **veerud**.

    ![Kohanda veerud](./media/resource-group-portal/select-columns.png)

1. Valige lisatavad veerud ja seejärel valige **Update**.

    ![veergude lisamine](./media/resource-group-portal/add-columns.png)

1. Teenuse juurutamisel oma uue ressursirühma ressursside kohta lisateabe saamiseks vt [Deploy ressursid ressursihaldur mallide ja Azure portaali](../resource-group-template-deploy-portal.md).

1. Kiireks juurdepääsuks ressursirühma, saate kinnitada tera armatuurlauale.

    ![PIN-koodi ressursirühm](./media/resource-group-portal/pin-group.png)

1. Armatuurlaua kuvatakse ressursirühma ja oma ressursse. Saate valida, kas ressursi rühmade või mõni oma ressursse üksusele liikumine.

    ![PIN-koodi ressursirühm](./media/resource-group-portal/show-resource-group-dashboard.png)

## <a name="tag-resources"></a>Sildi ressursid

Saate rakendada ressursi rühmad ja ressursside loogiliselt korraldada oma varasid sildid. Siltide töötamise kohta leiate artiklist [kasutamine siltide korraldamiseks oma Azure ressursse](../resource-group-using-tags.md).

[AZURE.INCLUDE [resource-manager-tag-resource](../../includes/resource-manager-tag-resources.md)]

## <a name="monitor-resources"></a>Kuvari ressursid

Kui valite ressursi, ressursside tera esitab vaikimisi diagrammid ja tabelid, jälgida selle ressursi tüüp.

1. Valige ressursi ja pange tähele jaotise **jälgimine** . See sisaldab graafikud, mis on seotud ressursside tüüp. Järgmisel pildil on kujutatud jälgida andmeid salvestusruumi konto jaoks vaikesätteid.

    ![Kuva jälgimine](./media/resource-group-portal/show-monitoring.png)

1. Saate kinnitada jaotise tera armatuurlauale, valides kolmikpunkti (…) üles jaotis. Saate kohandada suurus jaotise tera või selle täielikult eemaldada. Järgmisel pildil on kujutatud kinnitamine, kohandamine ja CPU ja mälu jaotise eemaldada.

    ![PIN-koodi jaotis](./media/resource-group-portal/pin-cpu-section.png)

1. Pärast kinnitamine jaotise armatuurlaud, näete kokkuvõttele armatuurlaual. Ja valige see kohe suunab teid andmete kohta rohkem üksikasju.

    ![armatuurlaua kuvamine](./media/resource-group-portal/view-startboard.png)

1. Täielikult kohandada portaali kaudu jälgida andmeid, liikuge vaikimisi armatuurlauale ja valige **armatuurlaud**.

    ![Armatuurlaua](./media/resource-group-portal/dashboard.png)

1. Tippige oma uue armatuurlaua nimi ja lohistage armatuurlaua paanid. Paanid on filtreeritud erinevaid võimalusi.

    ![Armatuurlaua](./media/resource-group-portal/create-dashboard.png)

     Töötamine armatuurlaudade kohta lisateabe saamiseks vaadake [loomine ja ühiskasutus armatuurlaudade Azure'i portaalis](azure-portal-dashboards.md).

## <a name="manage-resources"></a>Ressursside haldamine

Ressursi labale näete ressursi haldamise võimalusi. Portaalis esitatud halduse valikute ressursile jaoks. Näete ülaserva ressursi tera ja vasakul küljel haldamise käske.

![ressursside haldamine](./media/resource-group-portal/manage-resources.png)

Järgmiste võimaluste hulgast, saate teha toiminguid nagu käivitamine ja peatamine virtuaalse masina või konfigureerimist virtuaalse masina atribuudid.

## <a name="move-resources"></a>Teisalda ressursid

Kui vajate ressursid teisaldamiseks teise ressursirühm või mõne muu tellimuse, vt [teisaldamine ressursid uue ressursirühma või tellimusele](../resource-group-move-resources.md).

## <a name="lock-resources"></a>Lukusta ressursid

Saate lukustada tellimuse, ressursirühm või ressursside vältimaks teiste kasutajate kogemata kustutamise või muutmine kriitiliste ressursid ettevõttes. Lisateabe saamiseks vt [Lock ressursse Azure'i ressursihaldur](../resource-group-lock-resources.md).

[AZURE.INCLUDE [resource-manager-lock-resources](../../includes/resource-manager-lock-resources.md)]

## <a name="view-your-subscription-and-costs"></a>Saate vaadata oma tellimust ja kulud

Saate vaadata teavet oma tellimuse ja selle kulude kohta kõigi oma ressursse. Valige **tellimuste** ja see tellimus, mida soovite näha. Teil olla ainult üks tellimus valimiseks.

![tellimuse](./media/resource-group-portal/select-subscription.png)

Sees tellimuse tera, näete kirjuta määr.

![kirjutamine määr](./media/resource-group-portal/burn-rate.png)

Ja jaotus kulude ressursside tüübi järgi.

![ressursside maksumus](./media/resource-group-portal/cost-by-resource.png)

## <a name="export-template"></a>Ekspordi Mall

Pärast häälestamist ressursirühma, soovite vaadata ressursirühma ressursihaldur mall. Malli eksportimise pakub kahte eelised:

1. Saate hõlpsasti automatiseerida tulevaste juurutustes lahendus Kuna Mall sisaldab täielik infrastruktuur.

2. Võite saada tuttav malli süntaks vaadates juures on JavaScript Object märke (JSON) mis tähistab teie lahendus.

Üksikasjalikud juhised leiate teemast [Azure ressursihaldur eksportimine malli olemasoleva ressursid](../resource-manager-export-template.md).

## <a name="delete-resource-group-or-resources"></a>Ressursirühm või ressursse kustutamine

Ressursi rühma kustutamisel kustutatakse kõik sisalduvate ressursid. Saate ka kustutada üksikuid vahendeid ressursirühma. Soovite ettevaatlik ressursirühma kustutamisel, kuna muud ressursi rühmad, mis on seotud võib olla ressursid. Ressursihaldur ei kustuta lingitud ressursse, kuid need ei pruugi õigesti töötada ilma oodatud ressursid.

![rühma kustutamine](./media/resource-group-portal/delete-group.png)

## <a name="next-steps"></a>Järgmised sammud

- Auditilogide vaatamiseks lugege teemat [auditi toimingud koos ressursihaldur](../resource-group-audit.md).
- Juurutamise tõrkeotsing, leiate [tõrkeotsingu ressursside rühma juurutuste Azure'i portaalis](../resource-manager-troubleshoot-deployments-portal.md).
- Juurutamine portaali kaudu, lugege teemat [Deploy ressursid ressursihaldur mallide ja Azure portaali](../resource-group-template-deploy-portal.md).
- Ressursside juurdepääsu haldamiseks vaadake teemat [kasutamine rollimääranguid juurdepääsu oma ressursse Azure tellimuse haldamiseks](../active-directory/role-based-access-control-configure.md).






