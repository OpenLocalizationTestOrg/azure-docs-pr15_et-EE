<properties 
    pageTitle="Avaldamine masina õ veebiteenuse Azure'i turuplatsi | Microsoft Azure'i" 
    description="Kuidas avaldada oma Azure seadme õ veebiteenuse Azure turuplatsiga" 
    services="machine-learning" 
    documentationCenter="" 
    authors="BharathS" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/08/2016" 
    ms.author="bharaths"/>

# <a name="publish-azure-machine-learning-web-service-to-the-azure-marketplace"></a>Azure'i masina õ veebiteenuse avaldamine Azure'i turuplats 

Azure'i turuplatsi pakub võimalust avaldada Azure seadme õ veebiteenuste makstud või tasuta teenuste tarbeks väliskliente. Selles artiklis antakse ülevaade koos linkidega juhised, võite alustada selle protsessi. Selle protsessi abil saate oma veebiteenused saadaval muude arendajatele kasutamine oma rakendustes.


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="overview-of-the-publishing-process"></a>Avaldamise ülevaade 

Järgnevalt on toodud juhised teenuse Azure seadme õ web avaldamine Azure'i turuplatsilt.

1. Luua ja avaldada masina õ päringu vastuse teenuse (RRS)
2. Teenuse juurutamiseks tootmise ja API võti ja OData lõpp-punkti teabe saamiseks.
3. Avaldatud veebiteenuse URL-i abil saate avaldada [Azure'i turuplatsi (Andmeturg)](https://publish.windowsazure.com/workspace/) 
4. Esitatud teie pakkumise vaadatakse ja peab enne kui saate alustada oma klientidele, seda. Avaldamise protsess võib võtta mõne tööpäeva. 

## <a name="walk-through"></a>Läbi
###<a name="step-1-create-and-publish-a-machine-learning-request-response-service-rrs"></a>Samm 1: Luua ja avaldada masina õ päringu vastuse teenuse (RRS)###
 Kui te pole seda veel teinud, võtke selle [läbi](machine-learning-walkthrough-5-publish-web-service.md)vaadata.

###<a name="step-2-deploy-the-service-to-production-and-obtain-the-api-key-and-odata-endpoint-information"></a>Samm 2: Teenuse juurutamiseks tootmise ja saada API võti ja OData lõpp-punkti teave###
1. [Azure klassikaline portaali](http://manage.windowsazure.com), valige suvand **Arvuti õ** vasakpoolsel navigeerimisribal ja valige tööruumis. 

2. Klõpsake vahekaarti **VEEBITEENUSTE** ja valige veebiteenuse soovite avaldada kuvatakse Marketplace'ist.

    ![Azure'i turuplats][workspace]

3. Valige soovite on turuplats, peab olema lõpp-punkti. Kui te pole loonud mis tahes täiendavaid lõpp-punktid, saate valida **vaikimisi** lõpp-punkti.

4. Kui olete klõpsanud lõpp-punkti, mida saab näha **API võti**. Peate selle teabe hiljem samm 3, nii et sellest koopia.

    ![Azure'i turuplats][apikey]

5. Klõpsake **Taotluse/vastuse** meetod, sel hetkel me ei toeta avaldamise paketi täitmise teenuste kuvatakse Marketplace'ist. Mis viivad teid API abi lehe taotluse/vastuse meetod.

6. Kopeerige **OData lõpp-punkti aadress**, peate selle teabe hiljem samm 3.

    ![Azure'i turuplats][odata]




teenuse kasutuselevõtuks tootmisse.



###<a name="step-3-use-the-url-of-the-published-web-service-to-publish-to-azure-marketplace-datamarket"></a>Samm 3: Avaldatud veebiteenuse URL-i abil avaldada Azure'i turuplatsi (Datamarketi)###

1.  Liikuge [Azure'i turuplats (Andmeturg)](http://datamarket.azure.com/home) 
2.  Klõpsake lehe ülaosas linki **Avalda** . See viib teid [Microsoft Azure'i Avaldamisportaal](https://publish.windowsazure.com)
3.  Klõpsake jaotise **avaldajad** avaldaja registreeruda.
4.  Kui loote uue pakkumise, valige **Data Services**ja seejärel käsku **Loo uus Data Service**. 
 
    ![Azure'i turuplats][image1]

    <br />


5.  Klõpsake jaotises **lepingu** teavet teie pakkumise, sh hinnakirjad leping. Otsustage, kui te Pakume tasuta või tasulise teenuse. Sisestage saada makstud, nt panga- ja teabe andmed.

6.  Klõpsake jaotises **turundus** teavet oma pakkumine, nt pealkiri ja kirjeldus teie pakkumise.

7.  Jaotises **hinnakirjad** saate hinna määramine teie plaanid teatud riigis või süsteemi "autoprice" teie pakkumise andke.

8. Klõpsake menüü **Andmed teenuse** **Andmeallika** **Veebiteenus** .

    ![Azure'i turuplats][image2]

9.  Veebi teenuse URL-i ja API võti toomine Azure klassikaline portaali etappi 2 eespool kirjeldatud.

10. Turuplatsi andmed teenuse häälestus dialoogiboksi kleepige tekstiväljale **URL-i** OData lõpp-punkti aadress.

11. **Autentimine**, valige **päis** **Autentimise skeem**.

    - Sisestage "Autoriseerimine" **tabelipäise nimi**.
    - **Päise väärtus**, sisestage "Esitaja" (ilma jutumärkideta), klõpsake nuppu **tühikut** ja seejärel kleepige API võti.
    - Märkige ruut **see teenus on OData** .
    - Klõpsake nuppu **Testi ühendust** , et testida ühendus.

12. Klõpsake jaotises **Kategooriad**tagada **Masina õ** on valitud.

13. Kui olete valmis sisestada oma pakkumine metaandmeid nuppu **Avalda**ja seejärel **vajutage lavastus**. Selles etapis teavitatakse teid ülejäänud probleemidest, mida soovite parandada.

14. Pärast on tagatud lõpetamist tasumata probleeme, klõpsake **push tootmisele kinnitamise taotlemiseks**. Avaldamise protsess võib võtta mõne tööpäeva. 


[image1]:./media/machine-learning-publish-web-service-to-azure-marketplace/image1.png
[image2]:./media/machine-learning-publish-web-service-to-azure-marketplace/image2.png
[workspace]:./media/machine-learning-publish-web-service-to-azure-marketplace/selectworkspace.png
[apikey]:./media/machine-learning-publish-web-service-to-azure-marketplace/apikey.png
[odata]:./media/machine-learning-publish-web-service-to-azure-marketplace/odata.png
 
