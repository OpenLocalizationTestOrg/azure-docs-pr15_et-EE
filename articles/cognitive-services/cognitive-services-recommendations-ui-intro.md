<properties
    pageTitle="Hoone mudeli Recommnendations UI | Microsoft Azure'i"
    description="Azure'i masina õ soovitused - koostamise soovitused UI mudel"
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
    ms.date="10/11/2016"
    ms.author="luisca"/>

# <a name="building-a-model-with-the-recommendations-ui"></a>Soovitused UI koos mudeli loomine

See dokument on samm-sammult juhendi. Meie eesmärk on sõelub mudeli kasutamine [Soovitused UI](https://recommendations-portal.azurewebsites.net/)koolitada tarvilikud toimingud.
Kasutamise läbi võimaldab teil mõista protsessi koostamise mudeli, enne kui saate seda teha programmiliselt. Seda ka familiarizes teile kasutajaliides, mis on mugav, kui hakkate teenuse abil.

See ülesanne võtab aega umbes 30 minutit.

<a name="Step1"></a>
## <a name="step-1---sign-in-to-the-recommendations-ui"></a>Samm 1 - märk soovitusi kasutajaliides ##

1. Kui te pole seda veel teinud, peate [registreerumise](https://portal.azure.com/#create/Microsoft.CognitiveServices/apitype/Recommendations/pricingtier/S1) [Soovitused API](https://www.microsoft.com/cognitive-services/en-us/recommendations-api) uus tellimus. Saate teenuse **Azure** [http://portal.azure.com/](https://portal.azure.com/#create/Microsoft.CognitiveServices/apitype/Recommendations/pricingtier/S1) ja logige sisse logida Azure'i kontosse. Üksikasjalikud juhised selle registreerumise on toodud *tööülesande 1* [Alustusjuhend](cognitive-services-recommendations-quick-start.md) 

1. Kui olete soetanud tellimuse soovitused API **võti** , avage [Soovitused UI](https://recommendations-portal.azurewebsites.net/). 

1. Logige sisse portaali väljale **Konto võti** tootevõtme sisestamise teel, ja seejärel klõpsake nuppu **Logi sisse** .

    ![Soovitused UI: Sisselogimise dialoogiboksi.][reco_signin]


<a name="Step2"></a>
## <a name="step-2---lets-gather-some-training-data"></a>Samm 2 – vaatame koguda andmeid koolitus ##

Loomiseks ehitada, peab mootori kahte liiki andmed: kataloogi faili ja failikogumi kasutus. 

Kataloogi fail sisaldab teavet üksuste teil on pakkuda oma kliendile. Faili kasutus sisaldab teavet nende üksuste kasutamise või tehingud oma äri.

Tavaliselt soovite päringu poe andmebaasi need teabe puhul. Täna, oleme andnud Näidisandmete teile nii, et saate teada, kuidas kasutada soovituste API.

Andmeid saab alla laadida [http://aka.ms/RecoSampleData](http://aka.ms/RecoSampleData). Kopeerimine ja lahti **Books.Zip** faili kausta teie kohalikus arvutis. Näiteks **c:\data**.

Üksikasjalik teave skeemiga failide kataloogi ja kasutamise kohta leiate artiklist [kogumine andmete mudelisse koolitada](cognitive-services-recommendations-collecting-data.md) .
 
Selle toiminguga ei kavatse töötada nii, et teil pole liiga kaua ootama koolitus väga väike fail. Kui soovite proovida suurema faili, meil on paigutatud **MsStoreData.zip** , mis sisaldab valimi tehingud Microsoft Store [samasse asukohta](http://aka.ms/RecoSampleData).

<a name="Step3"></a>
## <a name="step-3---create-a-project-and-upload-catalog-and-usage-data"></a>Samm 3 - projekti loomine ja andmete kataloogi ja kasutus üleslaadimine ##

[Soovitused UI](https://recommendations-portal.azurewebsites.net/)sisse logides, näete projektide lehel. Kui olete varem loonud projektide, tuleks vaadake neid siin.
Projekt (tuntud ka kui *mudeli* API viite) on ümbris andmete kataloogi ja kasutamist. Saate luua mitme *koostab* projekti sees. Me sõelub protsessi järgmiste juhiste juurde.

1. Uue projekti loomiseks tippige nimi tekstiväljale (midagi teeks "MyFirstModel") ja klõpsake nuppu **Lisa projekt**.
 
    ![Soovitused UI: Projektide lehel.][reco_projects]

1. Kui projekt saab loonud, klõpsake jaotises **kataloogi faili lisamine** nuppu **Sirvi faili** . Laadige kataloogi teil toimingus 2. Kui salvestasite veebisaidil *c:\data*, peate selle kausta liikumiseks.

    ![Soovitused UI: Andmete lisamine projekti.][reco_firstmodel]

1. Pärast kataloogi on üles laaditud, klõpsake jaotises **Kasutus failide lisamine** nuppu **Sirvi faili** . Usage_large.txt faili lisada.

> **Mida teha, kui mul on suur fail kasutamine andmete?**
>
> Ainult kasutamine failide väiksem 200-MB saate üles laadida. See tähendab, et süsteem võib olla 2 GB väärtuses tehingu andmed, nii mitu faili saate vajadusel üles.
> Te ei pea nii palju andmeid, luua hea mudel, ei ole ainult andmeid, et küsimusi, kuid andmete kvaliteedi suurust. See on levinud kasutusandmete, kus enamik tehingute oleksid üksikud populaarsemad üksused ja seal on "vähe märku" muude üksuste kuvamiseks.

<a name="Step4"></a>
## <a name="step-4---lets-do-some-training"></a>Juhis 4 – teeme mõned koolitus! ##

Nüüd, kui olete alla laadinud nii kataloogi ja kasutuse andmeid, oleme valmis koolitada mootori nii, et seda saate teada, mustrite meie andmete põhjal.

1.  Klõpsake nuppu **Uus koostamine** .

1.  Valige **soovitused** tüübiks koostamine. Teate, et toetame järjestamine koostada ning mõne FBT (sageli ostnud koos) luua ka tüübid.

    ![Soovitused UI: Koostada dialoog.][reco_build_dialog.png]


    Mõne FBT koostamine võimaldab toodete, mis on tavaliselt ostetud ja tarbitud sama tehingu mustrite tuvastamiseks.
    Järjestuse koostamine kasutatakse funktsioonide huvi tuvastamiseks. 
    Me ei lähe väga sellesse FBT või järjestuse koostab see sisse, kuid kui olete huvitatud siis tuleb kontrollida [koostada tüübid ja mudeli dokumentatsiooni lehele](cognitive-services-recommendations-buildtypes.md).

1. Klõpsake nuppu **koostamine** . Koosta protsess võtab umbes viis minutit, kui kasutate raamatud andmed. See kulub oodatust suuremat andmekomplektide.

<a name="Step5"></a>
## <a name="step-5---lets-find-out-what-the-machine-learned"></a>Juhis 5 - vaatame teada saada, mida seadme õppinud! ##

Kui teie koostamine on lõpule viidud, märkate järgud loendist uus ehitada. Üksuse ja kasutajale soovitusi saate kasutataks selle koostamine.

1. Kui teie koostamine on lõpule jõudnud, klõpsake **Keskmine**. See võimaldab teil esitada mudeliga ja vaadake, millised üksused on soovitatav.

    ![Soovitused UI: Keskmine nupp][reco_score_button]

1. Üksuse näha, millised üksused on tagastatuna soovitused selle üksuse valimine. Pange tähele, et kui pole piisavalt tehinguid prognoosida soovituse mõne kindla üksuse, süsteemi ei tagasta selle üksuse soovitused.  Kui mingil põhjusel peate üksuse, mis tagastab soovitusi ei, proovige muudele üksustele hinded.

<a name="Step6"></a>
## <a name="step-6---next-steps"></a>Juhist 6 – järgmised sammud ##
Palju õnne! teil on koolitada mudeli ja sai seejärel seda mudelit soovitusi.  Soovitused UI on kasulik vahend, mis võimaldab teil vaadata oma projekte ja koostab. 

Nüüd, kui teil on mudeli, mida soovite saate teada, kuidas teha programmiliselt ülaltoodud juhiseid. Selleks, et õppida kõne API programmiliselt, kutsume teid [Soovitused API viide](http://go.microsoft.com/fwlink/?LinkId=759348) ja [Soovitused valimi rakenduse](http://go.microsoft.com/fwlink/?LinkID=759344)alla laadida.


[reco_signin]:../media/cognitive-services/reco_signin.PNG
[reco_projects]:../media/cognitive-services/reco_projects.PNG
[reco_firstmodel]:../media/cognitive-services/reco_firstmodel.png
[reco_build_dialog.png]:../media/cognitive-services/reco_build_dialog.png
[reco_score_button]:../media/cognitive-services/reco_score_button.png
