<properties
    pageTitle="Meediumifailide Azure'i portaalis analüüsida | Microsoft Azure'i"
    description="Selles teemas kirjeldatakse töötlemine meediumifailide meediumi Analytics meediumi protsessorid (tüüpi) Azure'i portaalis."
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
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="juliako"/>


# <a name="analyze-your-media-using-the-azure-portal"></a>Meediumifailide Azure'i portaalis analüüs

> [AZURE.NOTE] Selle õpetuse tegemiseks peate Azure'i konto. Lisateavet leiate teemast [Azure tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/). 

## <a name="overview"></a>Ülevaade

Azure Media Services Analytics on kogumi, kõne- ja nägemispuudega komponendid (enterprise skaala, nõuetele vastavus, turvalisus ja globaalse REACHi), mis hõlbustavad ettevõtted ja ettevõtetele tuletada vaidlustatavad ülevaateid oma videofaile. Azure Media Services Analytics üksikasjalikuma ülevaate leiate [selle](media-services-analytics-overview.md) teema. 

Selles teemas kirjeldatakse töötlemine meediumifailide meediumi Analytics meediumi protsessorid (tüüpi) Azure'i portaalis. Meediumi Analytics tüüpi aedvili MP4 või JSON failide. Kui meediumi protsessor toodeti MP4 faili, saate faili järk-järgult alla. Kui meediumi protsessor toodeti JSON faili, saate laadida alla faili Azure'i bloobimälu. 

## <a name="choose-an-asset-that-you-want-to-analyze"></a>Valige varade, mida soovite analüüsida 
 
1. Valige [Azure portaali](https://portal.azure.com/)konto Azure Media Servicesi.
2. Valige aknas **sätted** **varad**.  
.
    ![Analüüsi videod](./media/media-services-portal-analyze/media-services-portal-analyze001.png)

2. Valige varade, mida soovite analüüsida ja klõpsake nuppu **analüüsi** .
        
    ![Analüüsi videod](./media/media-services-portal-analyze/media-services-portal-analyze002.png)

3. Valige aknas **protsessi meediumi varade meediumi Analytics** protsessor. 

    Ülejäänud see artikkel selgitab, miks ja kuidas seda kasutada iga protsessor. 
   
4. Vajutage **Loo** Alusta tööd.

## <a name="azure-media-indexer"></a>Azure Media indekseerija

**Azure Media indekseerija** meediumi protsessor võimaldab teil teha meediumifailide ja sisu otsida, samuti luua suletud pealdise jälitab. See jaotiste annab mõned üksikasjad suvandid, mida saate määrata selle MP.

![Analüüsi videod](./media/media-services-portal-analyze/media-services-portal-analyze003.png)

### <a name="language"></a>Keel

Loomulikus keeles MMS faili ära tunda. Näiteks inglise ja Hispaania. 

### <a name="captions"></a>Pealdiste

Saate valida pealdise vorming, mis loob teie sisu. On indekseerimise töökohtade saate luua tiitrite failid järgmises vormingus:  

- **SAAMI**
- **TTML**
- **WebVTT**

Suletud pealdise (koopia) järgmistes vormingutes faile saab kasutada heli-ja videofailide kättesaadavaks inimestele kuulmispuudega inimestele.

### <a name="aib-file"></a>AIB fail

Valige see suvand, kui soovite luua heli Index bloobimälu faili jaoks kohandatud SQL serveri IFilter. [Lisateabe saamiseks vt.](https://azure.microsoft.com/blog/using-aib-files-with-azure-media-indexer-and-sql-server/)

### <a name="keywords"></a>Märksõnad

Valige see suvand, kui soovite luua märksõnade XML-faili. See fail sisaldab märksõnu ekstraktimist kõne sisu, sagedus ja offset teavet.

### <a name="job-name"></a>Töö nimi

Sõbralik nimi, mille abil saate töö ära tunda. [Selles](media-services-portal-check-job-progress.md) artiklis kirjeldatakse, kuidas saate töö edenemist jälgida. 

### <a name="output-file"></a>Väljundfail

Sõbralik nimi, mis võimaldab teil tuvastada väljundi sisu. 

## <a name="azure-media-hyperlapse"></a>Azure Media Liikuv stoppkaader

Azure Media Liikuv stoppkaader on MP, mis loob sujuva aja aegunud videod esimese isiku või toimingu-kaamera sisu.  Lisateavet leiate teemast [selle](media-services-hyperlapse-content.md) teema. See jaotiste annab mõned üksikasjad suvandid, mida saate määrata selle MP.

![Analüüsi videod](./media/media-services-portal-analyze/media-services-portal-analyze004.png)

### <a name="speed"></a>Kiiruse 

Määrake kiirust kiirendamiseks Sisestuskeel video. Väljund on stabiliseeritud ja kellaaja aegunud üleviimise video sisendit.

### <a name="job-name"></a>Töö nimi

Sõbralik nimi, mille abil saate töö ära tunda. [Selles](media-services-portal-check-job-progress.md) artiklis kirjeldatakse, kuidas saate töö edenemist jälgida. 

### <a name="output-file"></a>Väljundfail

Sõbralik nimi, mis võimaldab teil tuvastada väljundi sisu. 

## <a name="azure-media-face-detector"></a>Azure Media esinema detektor

**Azure Media näo detektor** meediumi protsessor (MP) võimaldab teil loendada, jälgida liikumist ja isegi hinnata sihtrühma osalemist ja reaktsioon näo avaldiste kaudu. See teenus sisaldab kahte funktsioone: 

- **Näo tuvastamise**

    Näo tuvastamise leiab ja rajad inimese nägu video sees. Mitme saab tuvastada ja hiljem jälitata, nagu nad liikuda, aeg ja asukoht metaandmed, tagastatakse JSON-failis. Ajal jälgimine, proovib see anda ühtsete ID sama näo ajal isik on liikumine kuval isegi juhul, kui need on ummistada või jätke korraks paneeli.

    >[AZURE.NOTE]See teenuste sooritada näo. Isik, kes lahkub paneeli või muutub ummistada jaoks liiga pikk antakse uue ID nad tagasi.

- **Emotsioon automaattuvastus**
    
    Emotsioon tuvastamise on valikuline osa näo tuvastamise meediumi protsessor, mis tagastab analüüsi mitme emotsionaalne atribuutide nägu tuvastatud, sh õnn, kurbus, hirm, viha ja. 

![Analüüsi videod](./media/media-services-portal-analyze/media-services-portal-analyze005.png)

### <a name="detection-mode"></a>Tuvastamise režiim

Üks järgmistest režiimidest saab Protsessor:

- näo tuvastamise
- näo emotsioon tuvastamise kohta.
- Aggregate emotsioon automaattuvastus

### <a name="job-name"></a>Töö nimi

Sõbralik nimi, mille abil saate töö ära tunda. [Selles](media-services-portal-check-job-progress.md) artiklis kirjeldatakse, kuidas saate töö edenemist jälgida. 

### <a name="output-file"></a>Väljundfail

Sõbralik nimi, mis võimaldab teil tuvastada väljundi sisu. 

## <a name="azure-media-motion-detector"></a>Azure Media liikumisandur

**Azure Media liikumisandur** meediumi protsessor (MP) võimaldab teil tuvastada tõhus jaotiste huvi muidu pikk ja sündmustevaene video. Liikumistee tuvastamise saab kasutada staatilise kaamera liikumistee toimumise video osade tuvastamiseks. See loob JSON faili, mis sisaldab soovitud metaandmete ajatemplid ja piiritlusboksi piirkond, kus sündmus toimus.

See tehnoloogia suunatud turvalisus video kanalid, on võimalik liigitada liikumistee oluline sündmused ja false positiivsed, näiteks varje ja valgustus muutusi. See võimaldab teil luua Turbeteatiste kaamera kanalite ilma seda rämpsposti lõputu oluline sündmusi, samal ajal ekstraktida astunud huvi väga kaua järelevalve videod.

![Analüüsi videod](./media/media-services-portal-analyze/media-services-portal-analyze006.png)

## <a name="azure-media-video-thumbnails"></a>Azure Media Video pisipildid

See protsessor abil saate luua kokkuvõtted pikk videod, valides automaatselt huvitav Koodilõigud allikas video. See on kasulik, kui soovite anda kiire ülevaade sellest, mida oodata pikk video. Üksikasjalikku teavet ja näited leiate teemast [Kasutamine Azure Media Video pisipildid Video kokkuvõtet loomiseks](media-services-video-summarization.md)

![Analüüsi videod](./media/media-services-portal-analyze/media-services-portal-analyze008.png)

### <a name="job-name"></a>Töö nimi

Sõbralik nimi, mille abil saate töö ära tunda. [Selles](media-services-portal-check-job-progress.md) artiklis kirjeldatakse, kuidas saate töö edenemist jälgida. 

### <a name="output-file"></a>Väljundfail

Sõbralik nimi, mis võimaldab teil tuvastada väljundi sisu. 


##<a name="next-steps"></a>Järgmised sammud

Vaate Media Servicesi õppeteemad.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


