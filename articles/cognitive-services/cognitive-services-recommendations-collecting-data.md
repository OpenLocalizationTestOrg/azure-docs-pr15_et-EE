<properties
    pageTitle="Andmete kogumine, et koolitada mudelisse: masina õ soovitused API | Microsoft Azure'i"
    description="Azure'i masina õ soovitused - koolitada mudeli andmete kogumine"
    services="cognitive-services"
    documentationCenter=""
    authors="luiscabrer"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="luisca"/>

#  <a name="collecting-data-to-train-your-model"></a>Mudelisse koolitada andmete kogumine #

Soovitused API õpib viimase tehinguid leidmiseks, millised üksused tuleks soovitatav teatud kasutaja.

Pärast mudeli loomist peate esitada kaks osa teabest, enne kui saate teha mõnda koolitus: kataloogi faili ja kasutusandmete.

>   Kui te pole teinud juba, soovitame teil [Lühijuhend juhend](cognitive-services-recommendations-quick-start.md)lõpuleviimiseks.


## <a name="catalog-data"></a>Kataloogi andmed ##

### <a name="catalog-file-format"></a>Kataloogi failivorming ###

Kataloogi fail sisaldab teavet üksuste teil on pakkuda oma kliendile.
Andmete kataloogi peaks järgige järgmises vormingus:

- Funktsioonid - ilma`<Item Id>,<Item Name>,<Item Category>[,<Description>]`

- Funktsioonidega-`<Item Id>,<Item Name>,<Item Category>,[<Description>],<Features list>`

#### <a name="sample-rows-in-a-catalog-file"></a>Valimi ridade kataloogi faili

Ilma funktsioonid.

    AAA04294,Office Language Pack Online DwnLd,Office
    AAA04303,Minecraft Download Game,Games
    C9F00168,Kiruna Flip Cover,Accessories

Funktsioone:

    AAA04294,Office Language Pack Online DwnLd,Office,, softwaretype=productivity, compatibility=Windows
    BAB04303,Minecraft DwnLd,Games, softwaretype=gaming,, compatibility=iOS, agegroup=all
    C9F00168,Kiruna Flip Cover,Accessories, compatibility=lumia,, hardwaretype=mobile

#### <a name="format-details"></a>Vormingu üksikasjad

| Nimi | Kohustusliku | Tüüp |  Kirjeldus |
|:---|:---|:---|:---|
| Üksuse Id |Jah | [A – z], [a – z], [0 – 9], [_] & #40; Allkriipsu & #41; [-] & #40; mõttekriipsu & #41;<br> Max pikkus: 50 | Üksuse ainuidentifikaator. |
| Üksuse nimi | Jah | Mis tahes tähtnumbrilised märgid<br> Max pikkus: 255 | Üksuse nimi. |
| Üksuse kategooria | Jah | Mis tahes tähtnumbrilised märgid <br> Max pikkus: 255 | Kategooria, mis kuulub selle üksuse (nt Cooking raamatud, draama...); võib olla tühi. |
| Kirjeldus | Ei, v.a juhul, kui need funktsioonid on esitamine (kuid võib olla tühi) | Mis tahes tähtnumbrilised märgid <br> Max pikkus: 4000 | Selle üksuse kirjeldus. |
| Funktsioonide loend | Ei | Mis tahes tähtnumbrilised märgid <br> Max pikkus: 4000; Max arv funktsioonid: 20 | Funktsiooni nimi komaga eraldatud loend = funktsiooni väärtus, mida saab kasutada mudeli soovitus täiustamiseks.|

#### <a name="uploading-a-catalog-file"></a>Kataloogi faili üleslaadimine

Vaadake [API viide](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f316efeda5650db055a3e1) kataloogi faili üleslaadimisel.  

Pange tähele, et kataloogi faili sisu tuleks sisuna kutse edasi anda.

Kui mitu kataloogi failide üleslaadimine mitu kõned sama mudel, me lisatakse ainult kataloogi uued üksused. Olemasolevate üksuste jäävad algsed väärtused. Kataloogi andmeid ei saa värskendada selle meetodi abil.

>   Märkus: Failide maksimummaht on 200MB.
>   Toetatud kataloogis olevad üksused maksimum on 100 000 üksused.


## <a name="why-add-features-to-the-catalog"></a>Miks funktsioonide lisamine kataloogi?

Soovitused engine loob statistika mudelit, mis ütleb teile, millised üksused tõenäoliselt meeldis või kliendi poolt ostetud. Kui teil on uus toode, mis on kunagi on juba see ei ole võimalik luua mudel kaasautorluse juhud eraldi. Oletame, et alustamist pakkuv uus "laste viiul" oma poes, kuna olete kunagi müünud selle viiul enne, kui te ei saa öelda, mis muude üksuste soovitada selle viiul.

See tähendab, et kui mootori teab teave selle viiul (st on muusikaliste dokumendi, on laste aegade 7-10, ei ole kallis viiul jne), siis mootori kas ja kuidas muude toodete sarnaseid funktsioone. Näiteks olete varem müünud viiul 's ja tavaliselt inimesi, kes ostavad viiulid pigem klassikalise muusika CD-d ja lehe muusika osta.  Süsteemi leiate need funktsioonid seoseid ja soovitusi, mis põhineb funktsioonide samal ajal, kui teie uus viiul on vähe kasutust.

Funktsioonide imporditakse kataloogi andmed ja seejärel oma asukoht (või funktsiooni mudeli prioriteedi) on seotud kui rank koostamine on lõpule jõudnud. Funktsioon rank muutmiseks vastavalt muster kasutusandmete ja tüüpi üksused. Kuid kasutus üksused, ühtsete reiting peaks olema ainult väikeste kõikumistega. Asukoha funktsioonid on negatiivne arv. Arvu 0 tähendab, et see funktsioon pole oli kohal (juhtub siis, kui kutsute esimese rank koostamine enne selle API). Kuupäev, kus on antud reiting nimetatakse Keskmine värskus.


###<a name="features-are-categorical"></a>Funktsioonid on Categorical

See tähendab, et loote funktsioonid, mis sarnaneb kategooria. Näiteks hind = 9.34 ei ole kategooriline funktsioon. Teisalt, funktsioon nagu priceRange = Under5Dollars on kategooriline funktsioon. Teine levinud viga on kasutada üksuse nimi funktsioonina. See põhjustaks üksuse olema kordumatu nii, et see oleks kirjeldada kategooria nime. Veenduge, et tähistada funktsioonide Kategooriad üksust.


###<a name="how-manywhich-features-should-i-use"></a>Kuidas mitu/kuhu funktsioone kasutada?


Lõpuks koostada soovitusi toetab kuni 20 funktsioonidega mudeli loomine. Võite määrata rohkem kui 20 funktsioonid üksuste kataloogis, kuid oodatud teha järjestuse koostamine ja valige ainult funktsioone, mis asuvad kõrge. (Funktsiooni, mille arvu 2.0 või rohkem on väga head funktsioonide kasutamiseks!). 


###<a name="when-are-features-actually-used"></a>Kui funktsioonid tegelikult kasutatakse?

Kui pole piisavalt tehingu andmete soovituste tehingu teavet eraldi kasutavad funktsioonid mudel. Nii funktsioonid on parima mõju "külma üksuste" – üksused, mille mõni tehingud. Kui kõik teie üksused on piisavalt tehingu teavet võib pole vaja rikastavad mudelisse funktsioone.


###<a name="using-product-features"></a>Toote funktsioonide abil

Funktsioonide kasutamiseks osana oma Koosta peate:

1. Veenduge, et teie kataloogi sisaldab funktsioone, kui te seda üles laadida.

2. Käivitab järjestuse koostamine. Seda tuleb teha analüüsi funktsioonide tähtsuse ja järjestus.

3. Soovitused Koosta käivitada, seada alltoodud koostada parameetrid: seatud väärtusele tõene, tõene, allowColdItemPlacement useFeaturesInModel ja modelingFeatureList seadma funktsioonid, mida soovite kasutada oma mudeli täiustamine komaga eraldatud loend. Lisateabe saamiseks vaadake [soovitused koostada tüüp parameetrid](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3d0) .





## <a name="usage-data"></a>Andmete kasutamine ##
Faili kasutus sisaldab teavet nende üksuste kasutamise või tehingud oma äri.

#### <a name="usage-format-details"></a>Vormingu kasutusteavet.
Kasutus fail on CSV (komaeraldusega) faili, kus iga rea kasutus faili tähistab kasutaja ja üksuse vahelise suhtluse. Iga rida on vormindatud järgmiselt:<br>
`<User Id>,<Item Id>,<Time>,[<Event>]`



| Nimi  | Kohustusliku | Tüüp | Kirjeldus
|-------|------------|------|---------------
|Kasutaja-Id|         Jah|[A – z], [a – z], [0 – 9], [_] & #40; Allkriipsu & #41; [-] & #40; mõttekriipsu & #41;<br> Max pikkus: 255 |Kasutaja ainuidentifikaator.
|Üksuse Id|Jah|[A – z], [a – z], [0 – 9], [& #95;] & #40; Allkriipsu & #41; [-] & #40; mõttekriipsu & #41;<br> Max pikkus: 50|Üksuse ainuidentifikaator.
|Aeg|Jah|Kuupäev vormingus: YYYY/MM/DDTHH:MM:SS (nt 06 2013/20T10:00:00)|Andmete aeg.
|Sündmuse|Ei | Ühte järgmistest:<br>• Klõpsake<br>• RecommendationClick<br>• AddShopCart<br>• RemoveShopCart<br>• Ostmine| Kande tüüp. |

#### <a name="sample-rows-in-a-usage-file"></a>Valimi ridade faili kasutamine

    00037FFEA61FCA16,288186200,2015/08/04T11:02:52,Purchase
    0003BFFDD4C2148C,297833400,2015/08/04T11:02:50,Purchase
    0003BFFDD4C2118D,297833300,2015/08/04T11:02:40,Purchase
    00030000D16C4237,297833300,2015/08/04T11:02:37,Purchase
    0003BFFDD4C20B63,297833400,2015/08/04T11:02:12,Purchase
    00037FFEC8567FB8,297833400,2015/08/04T11:02:04,Purchase

#### <a name="uploading-a-usage-file"></a>Kasutus faili üleslaadimine

Vaadake [API viide](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f316efeda5650db055a3e2) kasutamine failide üleslaadimise.
Pange tähele, et te kasutuse faili sisu kehana HTTP kõne edasi.

>  Märkus:

>  Faili suurim lubatud maht: 200MB. Võib-olla mitu kasutus faile üles laadida.

>  Peate enne alustamist kasutamine andmete lisamine mudelisse kataloogi faili üleslaadimine. Ainult üksuste kataloogi failis kasutatakse faasis koolitus. Kõigi muude üksuste ignoreeritakse.

## <a name="how-much-data-do-you-need"></a>Kui palju andmeid on vaja?

Mudelisse kvaliteet on väga sõltuvad teie andmete kogus ja kvaliteet.
Süsteemi õpib, kui kasutajad osta erinevatele üksustele (me kutsume selles osaleda juhud). Jaoks FBT järgud, on oluline teada, millised üksused on ostetud sama tehingud. 

Hea rusikareegel on, et enamike üksuste jaoks tuleb 20 tehingud või rohkem, et kui oleksite kataloogis 10 000 üksust, soovitame, et teil on vähemalt 20 korda numbrit tehingute või umbes 200 000 tehingud. See on taas rusikareegel. Peate eksperimenteerida oma andmed.

Kui olete loonud mudeli, saate teha [ühenduseta hindamise](cognitive-services-recommendations-buildtypes.md) kontrollida, kuidas mudelisse tõenäoliselt teha.
