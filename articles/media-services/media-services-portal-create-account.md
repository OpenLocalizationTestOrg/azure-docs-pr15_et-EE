<properties
    pageTitle=" Loo konto Azure Media Servicesi Azure portaali | Microsoft Azure'i"
    description="Selles õpetuses juhendab teid Azure'i portaalis Azure Media Servicesi konto loomise juhiseid."
    services="media-services"
    documentationCenter=""
    authors="Juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/24/2016"
    ms.author="juliako"/>


# <a name="create-an-azure-media-services-account-using-the-azure-portal"></a>Azure'i portaalis Azure Media Servicesi konto loomine

> [AZURE.SELECTOR]
- [Portaal](media-services-portal-create-account.md)
- [PowerShelli](media-services-manage-with-powershell.md)
- [ÜLEJÄÄNUD](http://msdn.microsoft.com/library/azure/dn194267.aspx)

> [AZURE.NOTE] Selle õpetuse tegemiseks peate Azure'i konto. Lisateavet leiate teemast [Azure tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/). 

Azure'i portaalis on võimalus kiiresti luua konto Azure Media Services (AMS). Saate oma kontole juurde pääseda, mis võimaldavad teil talletada, krüptida, kodeerida, haldamine ja voona meediumisisu Azure Media Servicesi. Media Servicesi konto loomine ajal saate ka seotud salvestusruumi konto loomine (või olemasoleva kasutamiseks) geograafilised piirkonna Media Servicesi kontona.

Selles artiklis selgitatakse, mõned levinud mõisted ja näidatakse, kuidas portaalis Azure Media Servicesi konto loomiseks.

## <a name="concepts"></a>Mõisted

Juurdepääs Media Servicesi nõuab kahte seotud kontod tehke järgmist.

- Media Servicesi konto. Teie konto kaudu pääsete juurde pilvepõhist Media Servicesi, mis on saadaval Azure kogum. Media Servicesi konto hoidke tegelik meediumisisu. Selle asemel talletatakse metaandmeid meediumisisu ja meedia töötlemise tööd teie konto. Saate luua konto ajal saate valida saadaval Media Servicesi piirkond. Valige piirkonnale on andmekeskuses, kus talletatakse teie konto jaoks metaandmete kirjeid.

    Saadaval Media Services (AMS) regioonid, sisaldavad järgmist: Põhja-Euroopa, Lääne Europe, Lääne USA, Ida-USA, Kagu-Aasia, Ida-Aasia Jaapan Lääne, Jaapani Ida. Media Servicesi ei kasuta osaleja rühmad.
    
    AMS on nüüd saadaval ka järgmised andmekeskuste: Brasiilia Lõuna, India Lääne, India, Lõuna ja India Kesk. Nüüd saate Azure portaali meediumi teenuse kontode loomine ja teha erinevaid toiminguid, mis on siin kirjeldatud. Siiski Live kodeerimine pole lubatud järgmised andmekeskuste. Lisaks kõik tüüpi kodeerimine reserveeritud üksused on saadaval nende andmekeskuste.
    
    - Brasiilia Lõuna: Standard- ja põhilised kodeerimine reserveeritud üksused on saadaval ainult.
    - India Lääne, India, Lõuna: 

- Azure storage konto. Salvestusruumi kontod peavad asuma samas geograafilised piirkonnas Media Servicesi konto. Media Servicesi konto loomisel saate valida kas olemasoleva salvestusruumi konto piirkonna või saate luua uue konto salvestusruumi piirkonna. Media Servicesi konto kustutamisel ei kustutata plekid seotud salvestusruumi konto.

## <a name="create-an-ams-account"></a>Looge konto AMS

Selle jaotise juhised näitab, kuidas luua konto AMS.

1. Logige sisse veebisaidil [Azure portaali](https://portal.azure.com/).
2. Valige **+ Uus** > **Web + Mobile** > **Media Services**.

    ![Media Servicesi loomine](./media/media-services-portal-vod-get-started/media-services-new1.png)

3. Sisestage **MEDIA SERVICES konto loomine** nõutav väärtused.

    ![Media Servicesi loomine](./media/media-services-portal-vod-get-started/media-services-new3.png)
    
    1. Sisestage väljale **Konto nimi**uue AMS konto nimi. Media Servicesi konto nimi on kõik väiketähtedeks või -tähed ilma tühikuteta ja on 3 kuni 24 märki.
    2. Valige tellimus, vahel eri Azure'i tellimused, mida teil on juurdepääs.
    
    2. **Ressursirühm**valige uue või olemasoleva ressurss.  Ressursirühma on ressursid, millest jagada elutsükli, õiguste ja poliitikate kogum. Lisateavet [siin](azure-resource-manager/resource-group-overview.md#resource-groups).
    3. Valige **asukoht**, talletamiseks meediumid ja metaandmete kirjeid Media Servicesi konto jaoks kasutatud piirkonnas. Selle piirkonna kasutatakse töötlemiseks ja voogesitus. Ainult saadaval Media Servicesi piirkondade kuvatakse väljal ripploendist. 
    
    3. **Salvestusruumi konto**, valige salvestusruumi konto bloobimälu Media Servicesi kontolt meediumi sisu esitada. Saate valida olemasoleva salvestusruumi konto sama geograafilise piirkonna Media Servicesi konto või luua salvestusruumi konto. Piirkonna on loodud uue konto salvestusruumi. Salvestusruumi kontonimed reeglid on samad Media Servicesi kontod.

        Lisateavet mäluruumi [siin](storage-introduction.md).

    4. Valige **Kinnita armatuurlaua** konto juurutamise edenemise kuvamiseks.
    
7. Klõpsake nuppu **Loo** vormi allservas.

    Kui konto on loodud, olek muutub **töötab**. 

    ![Media Servicesi sätted](./media/media-services-portal-vod-get-started/media-services-settings.png)

    AMS konto haldamiseks (nt videote üleslaadimine, kodeerida varad, töö edenemist jälgida) aknas **sätted** .

## <a name="manage-keys"></a>Klahvid haldamine

Peate konto nimi ja selle primaarvõtme teabe programmiliselt kontole juurde pääseda Media Services.

1. Azure'i portaalis, valige oma konto. 

    **Sätete** aken paremal. 

2. Valige aknas **sätted** **võtmed**. 

    Windowsi **haldamine** kuvatakse konto nimi ja esmaseid ja teiseseid klahvid ei kuvata. 
3. Väärtuste kopeerimiseks nuppu Kopeeri.
    
    ![Media Services võtmed](./media/media-services-portal-vod-get-started/media-services-keys.png)

## <a name="next-steps"></a>Järgmised sammud

Nüüd saate faile üles laadida AMS kontole. Lisateavet leiate teemast [failide üleslaadimine](media-services-portal-upload-files.md).

## <a name="media-services-learning-paths"></a>Media Servicesi Õppeteed

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


