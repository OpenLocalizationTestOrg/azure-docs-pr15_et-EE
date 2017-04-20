<properties
    pageTitle="Valige parameetrid optimeerida oma algoritme Azure'i masinõppe | Microsoft Azure"
    description="Selgitab, kuidas valida optimaalsete parameetrite seadmine Azure'i masinõppe algoritm."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/12/2016"
    ms.author="bradsev" />


# <a name="choose-parameters-to-optimize-your-algorithms-in-azure-machine-learning"></a>Valige parameetrid optimeerida oma algoritme Azure'i masinõppe

See teema kirjeldab, kuidas valida õige hyperparameter seatud Azure'i masinõppe algoritm. Enamik masin õppe algoritme on parameetrite seadmiseks. Kui sa treenid mudel, peate sisestama väärtused nende parameetrid. Koolitatud mudel efektiivsus sõltub mudeli parameetrid, mida valida. Optimaalsete parameetrite kogum leidmise protsess on tuntud *mudeli valik*.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

On mitmeid viise mudeli valik. Kohapeal õpet, ristkontrolli on üks enim kasutatud meetodite valiku ja see on vaikimisi mudeli valiku mehhanism, Azure masin õppe. Kuna Azure'i masinõppe toetab R ja Python, saate rakendada oma mudel valikumehhanismid alati R või Python.

Pole parimat parameetrikogum leidmisel järgmisi toiminguid:

1.  **Määratle parameeter ruumi**: algoritmile, kõigepealt otsustada, siis võiksite kaaluda täpne parameetriväärtuste.
2.  **Määratle selle ristkontrolli seaded**: Kuidas valida ristkontrolli voldid andmekogumi.
3.  **Määratle mõõdik**: otsustada, milline mõõdik parim komplekt parameetreid, nagu täpsuse määramiseks kasutada, root Keskmine ruuduline viga, täpsus, tagasikutsumise või f-Keskmine.
4.  **Rong, arvestada, ja võrrelda**: iga unikaalne kombinatsioon parameetri väärtused ristkontrolli poolt ja põhineb määratlete viga mõõdik. Pärast hindamis- ja võrdlusmeetodit, saate lõpus mudel.

Järgmine pilt illustreerib näitab, kuidas see on saavutatav Azure'i masinõppe.

![Leia parim parameetrikogum](./media/machine-learning-algorithm-parameters-optimize/fig1.png)

## <a name="define-the-parameter-space"></a>Määratleda parameeter ruumi
Saate määratleda parameetrivalik mudel käivitamise etapis. Kõik kohapeal õppe algoritme parameetrit paan on kaks treener režiimi: *Ühe parameetri* ja *Parameeter vahemikus*. Vali parameeter ulatuse režiim. Parameetri erinevatele režiimis, saate sisestada mitu väärtust iga näitaja. Väljale saate sisestada komaga eraldatud väärtused.

![Kaks klassi võimendatud Otsustepuu, ühe parameetri](./media/machine-learning-algorithm-parameters-optimize/fig2.png)

 Jooksvad saate määratleda maksimaalse ja minimaalse punkti ja genereerimist **kasutamine vahemikus**ehitaja punktide koguarv. Vaikimisi luuakse parameetri väärtused lineaarskaala näit. Kuid selle **Logaritmilise skaala** on märgitud, luuakse väärtusi logaritmilist skaalat (s.o külgnevad punktid suhe on konstantne asemel nende erinevus). Täisarv parameetrid saate määratleda vahemiku sidekriipsuga. Näiteks "1-10" tähendab, et kõik täisarvud vahemikus 1 kuni 10 (mõlemad kuupäevad kaasa arvatud) moodustavad parameetri set. Mixed mode ka toeta. Näiteks parameetri määratud "1-10, 20, 50" hõlmab täisarvud 1-10, 20 ja 50.

![Kaks klassi võimendatud Otsustepuu, parameetri erinevatele](./media/machine-learning-algorithm-parameters-optimize/fig3.png)

## <a name="define-cross-validation-folds"></a>Määratleda ristkontrolli voldid
[Sektsioon ja proovi] [ partition-and-sample] moodul saab juhuslikult määrata voldid andmetele. Järgmine proov konfiguratsioon mooduli, määratleda viis voldid ja juhuslikult määramiseks korda proovi juhtudel.

![Sektsioon ja proovi](./media/machine-learning-algorithm-parameters-optimize/fig4.png)


## <a name="define-the-metric"></a>Mõõdiku määratlemine
[Tune mudel Hyperparameters] [ tune-model-hyperparameters] moodul toetab empiiriliselt valides parim komplekt parameetreid antud algoritmi ja Andmekomplekt. Lisaks muudele andmetele seotud koolitus mudel **omadustega** pane see moodul hõlmab määramiseks parim parameetrikogum mõõdik. See on kaks erinevat ripploendist kasti klassifikatsioon ja regressiooni algoritme, vastavalt. Algoritmi alusel on liigitus algoritm, regressiooni mõõdik ignoreeritakse ja vastupidi. Selle konkreetse näite puhul on mõõdik **täpsus**.   

![Pühkima parameetrid](./media/machine-learning-algorithm-parameters-optimize/fig5.png)

## <a name="train-evaluate-and-compare"></a>Koolitada, hinnata ja võrrelda  
Sama [Tune mudel Hyperparameters] [ tune-model-hyperparameters] moodul koolitab Kõik mudelid, mis vastavad parameetri set, hindab erinevate mõõdikute ja seejärel loob parima väljaõppega mudel põhineb valite mõõdik. Moodulil on kaks kohustuslikku sisendeid:

* Treenimata õppija
* Andmekomplekti

Moodul on valikuline andmekomplekti, sisend. Andmekogumi kasutajaga korda teabe kohustuslik andmekogumi sisendisse. Kui andmekomplekti on määratud klapp teavet, seejärel 10 korda ristkontrolli automaatselt täidetakse vaikimisi. Kui määramise korda ei tehta ja valideerimise andmekogumis on saadaval valikuline andmekogumi sadama, siis valitakse rongi testrežiim ja esimese andmekogumi saab rongi mudel iga parameetri kombinatsiooni.

![Võimendatud otsuse puu klassifitseerijale](./media/machine-learning-algorithm-parameters-optimize/fig6a.png)

Mudel on siis hinnata valideerimise andmekomplekti. Moodul vasakul väljundport näitab erinevate mõõdikute funktsioone parameetreid. Õige väljund porti annab vastava väljaõppega näidise lõpus mudel vastavalt valitud mõõdik (antud juhul**täpsus** ).  

![Valideerimise andmekomplekti](./media/machine-learning-algorithm-parameters-optimize/fig6b.png)

Saate vaadata valitud õige väljund porti visualiseerides täpselt parameetreid. See mudel saab hinded testikomplekt või operationalized veebiteenus, peale salvestamist koolitatud mudel.

<!-- Module References -->
[partition-and-sample]: https://msdn.microsoft.com/library/azure/a8726e34-1b3e-4515-b59a-3e4a475654b8/
[tune-model-hyperparameters]: https://msdn.microsoft.com/library/azure/038d91b6-c2f2-42a1-9215-1f2c20ed1b40/
