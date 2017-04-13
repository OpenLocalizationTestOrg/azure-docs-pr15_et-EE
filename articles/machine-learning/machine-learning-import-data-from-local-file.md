<properties
    pageTitle="Andmete importimine masina õ Studio kohaliku faili | Microsoft Azure'i"
    description="Kuidas importida koolitus andmete Azure seadme õ Studio kohalik fail."
    keywords="andmete vormindamine, andmetüübid, andmeallikate, koolitus andmete importimine"
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="garye;bradsev" />


# <a name="import-your-training-data-into-azure-machine-learning-studio-from-a-local-file"></a>Koolitus andmete importimine Azure seadme õ Studio kohaliku faili

[AZURE.INCLUDE [import-data-into-aml-studio-selector](../../includes/machine-learning-import-data-into-aml-studio.md)]


Kasutada oma andmeid arvuti õ Studios, te saate üles laadida kohalikul kõvakettal andmekomplekti mooduli tööruumi loomiseks andmefaili ette valmistada. 


## <a name="import-data-from-a-local-file"></a>Andmete importimine kohalik fail.

Saate importida andmeid kohalikul kõvakettal, tehes järgmist:

1. Valige **+ Uus** arvuti õ Studio akna allosas.
2. Valige **ANDMEKOMPLEKTI** ja **kohalik fail**.
3. Liikuge dialoogiboksis **Uus andmekomplekti üles** fail, mille soovite üles laadida
4. Sisestage nimi, andmetüüp tuvastamine ja soovi korral sisestage kirjeldus. Kirjeldus on soovitatav – see võimaldab salvestada mis tahes omaduste kohta andmed, mida soovite edaspidi andmete kasutamisel meeles pidada.
5. Märkeruut **see on mõne olemasoleva andmekomplekti uue versiooni** saate värskendada mõne olemasoleva andmekomplekti uute andmetega. Klõpsake lihtsalt see ruut ja seejärel sisestage mõne olemasoleva andmekomplekti nime.

Laadi, kuvatakse sõnumi faili üleslaadimisel. Laadige aeg sõltub teie andmete maht ja teenusega ühenduse kiirust.
Kui teate, et fail võtab kaua aega, mida saab teha muud asjad, mida arvuti õ Studio sees ooteajal. Sulgege brauser põhjustab andmete üleslaadimise nurjumise.

Kui teie andmed on üles laaditud, talletatakse andmekomplekti mooduli ja on saadaval katse oma tööruumis.
Kui te redigeerite katse, leiate jaotise mooduli värvipaleti **Salvestatud andmekomplektide** loendis **Minu andmekomplektide** loendis loodud kogumid. Te saate lohistada andmekomplekti katse lõuendile kui soovite kasutada andmehulga edasise analüüsi ja seadme õppimise.



