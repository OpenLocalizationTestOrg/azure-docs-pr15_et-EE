<properties
   pageTitle="Luua server Analysis Services Azure | Microsoft Azure'i"
   description="Saate teada, kuidas Azure analüüsiteenuste serveri eksemplari loomine."
   services="analysis-services"
   documentationCenter=""
   authors="minewiskan"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="analysis-services"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="owend"/>

# <a name="create-an-analysis-services-server"></a>Luua server Analysis Services
Selles artiklis tutvustatakse uue analüüsiteenuste serveri ressursi Azure tellimuse loomine.

## <a name="before-you-begin"></a>Enne alustamist
Enne alustamist peate.

- **Azure'i tellimus**: külastage [Azure'i tasuta prooviversiooni](https://azure.microsoft.com/offers/ms-azr-0044p/) konto loomiseks.
- **Ressursirühm**: ressursirühm teil on juba olemas või [looge uus](../azure-resource-manager/resource-group-overview.md).

> [AZURE.NOTE] Ettevõtte analüüsiteenuste serveris loomine võib põhjustada arveldatavate uus teenus. Lisateavet leiate teemast analüüsiteenuste hinnad.

## <a name="create-an-analysis-services-server"></a>Luua server Analysis Services

1. [Azure'i portaali](https://portal.azure.com)sisse logida.

2. Valige **+ Uus** > **ärianalüüsi + analytics** > **Analysis Services**.

3. **Analüüsiteenuste** labale nõutavad väljad ja vajutage **loomine**.

    ![Serveri loomine](./media/analysis-services-create-server/aas-create-server-blade.png)

    - **Serveri nimi**: tippige viitav server kordumatu nimi.

    - **Tellimus**: valige see server arved, et tellimus.

    - **Ressursirühm**: need ümbriste aitab teil haldamiseks Azure ressursse. Lisateavet leiate teemast [Ressursi rühmade](../resource-group-overview.md).

    - **Asukoht**: Azure'i selle andmekeskuse asukoha majutab server. Valige oma suurim kasutaja alus lähima asukoht.

    - **Hinnakirjad taseme**: valige hinnakirjad taseme. Tabelina mudelid kuni 100 GB on toetatud. Saate alati muuta oma hinnakirjad taseme hiljem.

4. Klõpsake nuppu **Loo**.

Looge kulub tavaliselt vähem kui minutiga; sageli paar minutit. Kui valisite **Lisa portaali**, liikuge oma portaalis teie uus server kuvamiseks. Või liikuge **rohkem teenuseid** > **Analüüsiteenuste** kuvamiseks, kui teie server on valmis. Kui seda ei kuvata, värskendage loendit.

 ![Armatuurlaua](./media/analysis-services-create-server/aas-create-server-dashboard.png)


## <a name="next-steps"></a>Järgmised sammud
Kui olete loonud oma serveri, saate selle [juurutamine mudeli](analysis-services-deploy.md) SSDT abil või SSMS abil.

Kui juurutate oma mudeli kohapealse andmeallikaga, peate mõne [kohapealse andmete lüüsi](analysis-services-gateway.md) teie võrku arvutisse installida.
