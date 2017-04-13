<properties
    pageTitle="Teksti analytics mudelite loomine Azure seadme õ Studio | Microsoft Azure'i"
    description="Kuidas luua teksti analytics mudelite Azure seadme õ Studio abil teksti eeltöötlus, g-N või funktsiooni räsi moodulid"
    services="machine-learning"
    documentationCenter=""
    authors="rastala"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="roastala" />


#<a name="create-text-analytics-models-in-azure-machine-learning-studio"></a>Teksti analytics mudelite loomine Azure seadme õ Studio

Azure'i masina õ abil saate koostada ja teksti analytics mudelite kiireks. Nende abil saate näiteks dokumendi liigitamine või meeleolu analüüsi probleeme lahendada.

Teksti analytics katse, tavaliselt samamoodi:

 1. Puhastada ja preprocess teksti andmekomplekti
 2. Arvuline funktsiooni vektorite eelnevalt töödeldud teksti eraldamiseks
 3. Rongi liigitamine või regressiooni mudel
 4. Keskmine ja kinnitage mudel
 5. Mudeli tootmisse juurutamine

Selles õppetükis saate teada järgmist nagu tutvustame meeleolu analüüsi mudeli Amazon raamat ülevaateid andmekomplekti abil (vt käesoleva uurimistöö dokumendi "Elulood, stiil, Boom-väljade ja Blenderid: domeeni kohandamine meeleolu liigitamiseks" John Blitzer, Mark Dredze ja Fernando Pereira; Assotsiatsiooni arvutuslik lingvistika (ACL), 2007.) Tuvastatavate koosneb läbivaatus hinded (1-2 või 4-5) ja vabakujulise teksti. Eesmärk on prognoosida Keskmine läbivaatus: madal (1 - 2) või kõrge (4-5).

Selles õpetuses Cortana ärianalüüsi Galerii kaetud katsete otsimine

[Prognoosida broneerida kommentaare] (https://gallery.cortanaintelligence.com/Experiment/Predict-Book-Reviews-1)

[Prognoosida raamat ülevaateid - sõnastikupõhise katse] (https://gallery.cortanaintelligence.com/Experiment/Predict-Book-Reviews-Predictive-Experiment-1)

## <a name="step-1-clean-and-preprocess-text-dataset"></a>Samm 1: Puhastada ja preprocess teksti andmekomplekti

Alustame katse, jagades läbivaatus hinded kategooriline kõrge ja madala ämbrid koostada probleemi kaks klassi liigitamine nimega. Kasutame [redigeerimine metaandmete] (https://msdn.microsoft.com/library/azure/dn905986.aspx) ja moodulid [rühma kategooriate väärtuste] (https://msdn.microsoft.com/library/azure/dn906014.aspx).

![Sildi loomine](./media/machine-learning-text-analytics-module-tutorial/create-label.png)

Seejärel me puhastada tekst [Preprocess tekst] (https://msdn.microsoft.com/library/azure/mt762915.aspx) mooduli kasutamise. Puhastamine vähendab müra andmekomplekti, aitavad teil otsida kõige olulisemad funktsioonid ja lõplik mudel täpsuse suurendamiseks. Stopwords – levinud sõnu, nt "on" või "a" - ja arvud, erimärke, dubleeritakse märgid, e-posti aadressid ja URL-ide eemaldamine Me teisendada teksti väiketähed, lemmatize sõnad, ja tuvasta lause piirmäärad, mis tähistab siis "|||" eelnevalt töödeldud teksti sümbol.

![Teksti PreProcess](./media/machine-learning-text-analytics-module-tutorial/preprocess-text.png)

Mida teha, kui soovite kasutada kohandatud loendi stopwords? See saab edastada valikuline sisendina. Saate ka kasutada kohandatud C# süntaks Lihtavaldise asendamiseks kasutatavat võrdluslaadi ja sõnade eemaldamiseks kõneviisi: nimisõnad, verbide või omadussõnu.

Kui soovitud eeltöötlus on lõpule jõudnud, andmed jagatud rongi ja testida komplektid.

## <a name="step-2-extract-numeric-feature-vectors-from-pre-processed-text"></a>Samm 2: Arvuline funktsiooni vektorite ekstraktida eelnevalt töödeldud tekst

Koostamiseks tekstandmeid näidis, peate tavaliselt vabakujulise teksti teisendamine arvuline funktsiooni vektorite. Selles näites me kasutame [ekstraktida N-g funktsioonid tekstist] (https://msdn.microsoft.com/library/azure/mt762916.aspx) mooduli vormis teksti andmeid muuta. Selle mooduli võtab veeru tühiku eraldatud sõnade ja arvutab sõnastikus sõnade või sõnad, mis kuvatakse teie andmekogumis N-g. Seejärel see loendab, mitu korda iga sõna või N-g, kuvatakse iga kirje ja loob funktsiooni vektorite mandaadist loendab. Selles õpetuses me määrata N-g suurus 2, meie funktsiooni vektorite sisaldavad üksikuid sõnu ja kahe sõnast kombinatsioonid.

![Ekstraktida N-g](./media/machine-learning-text-analytics-module-tutorial/extract-ngrams.png)

Me rakendada TF * IDFI (Termini sagedus pööratud dokumendi sagedus) abil N-g kaal loendab. Seda moodust lisab jämeduse sõnad, mis kuvatakse sageli üheks kirjeks, kuid on kogu kogu andmekomplekti harva. Muude suvandite kaasata kahendarvuks TF, ja kaal põhinema.

Sellise teksti funktsioonid on sageli kõrge dimensionaalsus. Näiteks kui teie corpus on 100 000 kordumatu sõnu, veebiruumi funktsioon oleks 100 000 mõõtmed või rohkem N-g kasutamisel. Ekstraktida N-g funktsioonid mooduli annab teile vähendada dimensionaalsus soovitud suvandeid. Saate valida välistamine sõnad, mis on lühike või pikk, või liiga aeg-ajalt või liiga sageli, on oluline väärtuse. Selles õpetuses me välistada N g, mis kuvatakse vähem kui 5 kirjeid või rohkem kui 80% kirjed.

Samuti saate funktsioonide valik ainult funktsioone, mis on kõige seotud oma ennustamine target valimiseks. Hii-funktsioonide valik abil valige 1000 funktsioone. Saate vaadata valitud sõnu või N-g sõnavara, klõpsates nuppu ekstrakti N-g mooduli õige väljund.

Alternatiivne lähenemisviisi ekstraktida N-g funktsioonide abil saate funktsiooni räsi mooduli. Märkus. Kuigi see [funktsioon räsi] (https://msdn.microsoft.com/library/azure/dn906018.aspx) ei saa Koosta funktsioon valiku võimaluste või TF * IDFI kaal.

## <a name="step-3-train-classification-or-regression-model"></a>Samm 3: Koolitada liigitamine või regressiooni mudel

Nüüd teksti ümber arvuline funktsiooni veergudesse. Andmekomplekti sisaldab stringi veerge eelmises etapis, me kasutame andmekomplekti jätta nende veergude valimine.

Siis kasutame [kaks klassi logistilise regressiooni] (https://msdn.microsoft.com/library/azure/dn905994.aspx) prognoosida meie sihtkoht: kõrge või madala Keskmine. Selles etapis teksti analytics probleemi on ümber tavaline liigitamine probleem. Azure'i masina õ saadaolevate tööriistade abil saate parandada mudelit. Näiteks saate proovida erinevaid liigitused, et teada saada, kuidas õigete tulemuste annavad või hyperparameter häälestamine täpsuse parandamiseks kasutada.

![Raudtee- ja Keskmine](./media/machine-learning-text-analytics-module-tutorial/scoring-text.png)

## <a name="step-4-score-and-validate-the-model"></a>Samm 4: Keskmine ja kinnitage mudel

Kuidas kinnitamist koolitatud mudeli? Me Keskmine selle testi andmekomplekti vastu ja hindab täpsust. Siiski mudeli õpitut sõnavara N-g ja nende kaal koolitus andmekomplekti. Seetõttu peaks kasutame selle sõnavara ja nende kaalu kui ekstraktimiseks testi andmete asemel looma uue sõnavara funktsioone. Seetõttu me lisada ekstraktida N-g funktsioonid mooduli hinded haru katse, ühenduse väljundi sõnavara koolitus kontorist ja kirjutuskaitstud režiimis sõnavara määramine. Lisaks keelamine filtri N-g, sagedus, seades minimaalne 1 eksemplar ja kuni 100% ja funktsioonide valik välja lülitada.

Pärast teksti veeru andmed ümber arvuline funktsiooni veergudele, me ei sisalda stringi veergude eelmiste etappide nagu koolitus kontoris. Siis kasutame Keskmine mudeli mooduli teha prognoose ja hinnata mudeli mooduli hinnata täpsust.

## <a name="step-5-deploy-the-model-to-production"></a>Juhis 5: Mudeli juurutamiseks tootmise

Mudeli on peaaegu valmis tootmise kasutusele võtta. Kui juurutatud veebiteenuse, see võtab vabakujulise tekstistringi sisendina ja tagastada ennustamiseks "suur" või "madal." Õppinud N-g sõnavara kasutab funktsioonidele ja koolitatud logistilise regressiooni mudeli veendumaks, et need funktsioonid kaudu ennustamiseks teksti muuta. 

Häälestada sõnastikupõhise katse, me esmalt salvestada N-g sõnavara nimega andmekomplekti ja koolitatud logistilise regressiooni mudeli koolitus kontorist katse. Salvestage me "Salvesta nimega" abil luua mõne katse graafik sõnastikupõhise katse katse. Katse kaudu eemaldada tükeldatud andmetega mooduli ja koolitus haru. Me siis ühendada varem salvestatud N-g sõnavara ja mudeli ekstraktida N-g funktsioonid ja Keskmine mudeli moodulid, vastavalt. Me ka eemaldada mooduli hindamine mudel.

Me andmekomplekti moodulis enne Preprocess teksti mooduli veeru sildi eemaldamiseks valige veergude lisamine ja valimise "Lisa Keskmine veerg andmekomplekti" suvand Keskmine moodulis. Nii veebiteenuse taotleda soovitakse prognoosida silt ja teeb ei sisend funktsioonid vastuseks kaja.

![Sõnastikupõhise katse](./media/machine-learning-text-analytics-module-tutorial/predictive-text.png)

Nüüd on katse saab avaldatud veebiteenuse ja nimega abil päringu vastuse või paketi täitmise API-d.

## <a name="next-steps"></a>Järgmised sammud

Lisateavet teksti analytics moodulid: [MSDN-i dokumentatsiooni] (https://msdn.microsoft.com/library/azure/dn905886.aspx).
