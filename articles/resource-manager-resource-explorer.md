<properties
   pageTitle="Azure'i Resource Explorer | Microsoft Azure'i"
   description="Kirjeldatakse Azure'i Resource Explorer ja kuidas seda saab vaadata ja värskendada juurutuste Azure'i ressursihaldur kaudu"
   services="azure-resource-manager"
   documentationCenter="na"
   authors="stuartleeks"
   manager="ankodu"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/01/2016"
   ms.author="stuartle;tomfitz"/>

# <a name="use-azure-resource-explorer-to-view-and-modify-resources"></a>Azure'i ressursi Exploreri abil saate vaadata ja muuta ressursid
[Azure'i Resource Explorer](https://resources.azure.com) on suurepärase tööriista vaadates ressursse, mis on juba loodud teie tellimus. Selle tööriista abil saavad aru, kuidas ressursse on struktureeritud ja iga ressursile määratud atribuutide kuvamiseks. REST API toimingud ja ressursside tüübi jaoks saadaolevad PowerShelli cmdlet-käskude kohta leiate teavet teemast ja saate väljastada kasutajaliidese käsud. Ressursi Explorer võib olla eriti abiks, kui loote ressursihaldur Mallid, kuna võimaldab olemasolevate atribuutide vaatamine.

Allika ressursi Exploreri tööriista on saadaval [github](https://github.com/projectkudu/ARMExplorer), mis pakub väärtuslik viide, kui teil on vaja rakendada sarnane asi oma rakendustes.

## <a name="view-resources"></a>Kuva ressursid
Liikuge [https://resources.azure.com](https://resources.azure.com) ja logige sisse sama identimisteavet, mida kasutaksite [Azure portaali](https://portal.azure.com).

Pärast laadimist treeview vasakul võimaldab teil oma tellimuste ja ressursside rühmade süvitsiminek.

![TreeView](./media/resource-manager-resource-explorer/are-01-treeview.png)

Kui te üldiseks ressursirühma kuvatakse pakkuja, mille on ressursid selle rühma.

![pakkujad](./media/resource-manager-resource-explorer/are-02-treeview-providers.png)

Seal saate alustada süveneda just ressursikeskuse eksemplari sisse. Pildil näete selle `sltest` SQL serveri eksemplar, klõpsake soovitud treeview. Paremal pool, näete teavet saate kasutada koos selle ressursi REST API päringute kohta. Ressursi sõlm navigeerides teeb Resource Explorer automaatselt GET-päringu ressursi kohta teabe toomiseks. Suure tekstiala all URL-i, kuvatakse API vastus. 

Kui saate tutvuda ressursihaldur mallide sisu hakkab otsima tuttavad! Jaotist **Atribuudid** ja tagasiside vastab malli jaotises **Atribuudid** saate sisestada väärtused.

![SQL serveri](./media/resource-manager-resource-explorer/are-03-sqlserver-with-response.png)

Ressursi Explorer võimaldab teil hoida süvitsiminek uurida lapse ressursse, SQL-andmebaasi serveri puhul, on näiteks andmebaasid ja tulemüüri reeglid lapse ressursse.

Andmebaasi uurimine näitab meile selle andmebaasi atribuudid. Pildil näeme, et andmebaasi `edition` on `Standard` ja `serviceLevelObjective` (või andmebaasi taseme) on `S1`.

![SQL-andmebaas](./media/resource-manager-resource-explorer/are-04-database-get.png)

## <a name="change-resources"></a>Ressursside muutmine

Kui te viibite, ressursi, saate teha JSON sisu redigeerida nuppu Redigeeri. Seejärel saate ressursi Exploreri redigeerimine soovitud JSON ja muuta ressursi saata panna. Näiteks alloleval pildil on näha andmebaasi kiht, mis on muutunud `S0`:

![andmebaasi - pane taotlus](./media/resource-manager-resource-explorer/are-05-database-put.png)

Valides **sellele** saadate kutse. 

Kui kutse on esitatud ressursi Exploreri probleemide uuesti GET-päringu oleku värskendamiseks. Selles näites me näeme, et selle `requestedServiceObjectiveId` on värskendatud ja erineb funktsiooni `currentServiceObjectiveId` , mis näitab, et skaleerimise toiming on pooleli. Võite klõpsata nuppu HANGI oleku käsitsi värskendada.

![andmebaasi - GET request2](./media/resource-manager-resource-explorer/are-06-database-get2.png)

## <a name="performing-actions-on-resources"></a>Toimingute ressursid

Vahekaart **toimingud** võimaldab teil vaadata ja ülejäänud täiendavaid toiminguid. Kui olete valinud veebisaidi ressursi, esitatakse vahekaarti toimingud hulgaliselt toimingud, mis on allpool näidatud.

![veebi - POST taotlus](./media/resource-manager-resource-explorer/are-web-post.png)

## <a name="invoking-the-api-via-powershell"></a>Kasutada API PowerShelli kaudu
PowerShelli menüü Resource Explorer kuvatakse cmdlet-käskude abil saate suhelda ressurss, mis on praegu uurida. Sõltuvalt valitud ressursi, kuvatakse PowerShelli skripti vahemikus lihtsa cmdlet-käskude (näiteks `Get-AzureRmResource` ja `Set-AzureRmResource`) keerulisem cmdletid (nt vahetamine slots veebisaidil). 

![PowerShelli](./media/resource-manager-resource-explorer/are-07-powershell.png)

The Azure PowerShelli cmdlet-käskude kohta leiate lisateavet teemast [Azure ressursihaldur Azure PowerShelli kasutamine](powershell-azure-resource-manager.md)

## <a name="summary"></a>Kokkuvõte
Ressursihaldur koos töötades Resource Explorer võib olla väga kasulik. See on suurepärane viis leida võimalusi PowerShelli abil päringu ja muuta. Kui töötate REST API-ga on suurepärane viis alustamine ja kiiresti testi API kõned enne alustamist koodi kirjutamist. Ja kui kirjutate malle võib olla suurepärane viis mõista ressursi hierarhia ja leida kui panna konfiguratsiooni - portaalis muudatuste tegemine ja seejärel otsige vastavate kirjete ressursi Exploreris!

Lisateabe saamiseks vaadake [kanali 9 Scott Hanselman ja David Ebbo video](https://channel9.msdn.com/Shows/Azure-Friday/Azure-Resource-Manager-Explorer-with-David-Ebbo)


