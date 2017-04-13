<properties 
    pageTitle="Kuidas kasutada Blitline pildi töötlemine - Azure'i funktsioonide juhend" 
    description="Saate teada, kuidas töödelda pilte Azure rakenduses Blitline teenust kasutada." 
    services="" 
    documentationCenter=".net" 
    authors="blitline-dev" 
    manager="jason@blitline.com" 
    editor="jason@blitline.com"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="12/09/2014" 
    ms.author="support@blitline.com"/>
# <a name="how-to-use-blitline-with-azure-and-azure-storage"></a>Kuidas kasutada Blitline Azure ja Azure Storage

Sellest juhendist selgitab, kuidas Blitline teenustele juurdepääsuks ja kuidas Blitline tööde edastamiseks.

## <a name="what-is-blitline"></a>Mis on Blitline?

Blitline on teenus, mis sisaldab ettevõtte taseme pilditöötluse, hind, et see maksab ehitada ise murdosa töötlemine pilvepõhist pilt.

Asjaolu, on see, et pilditöötluse on tehtud ikka ja jälle üles iga veebisaidi maa tavaliselt uuesti. Me aru, et see kuna oleme loonud need miljon korda liiga. Üks päev otsustasime, et ehk on aeg lihtsalt teeme selle kõigi kasutajate jaoks. Me teada, kuidas seda teha, tehke selle kiire ja tõhus loomine ja salvestada kõik töö vahepeal.

Lisateavet leiate teemast [http://www.blitline.com](http://www.blitline.com).

## <a name="what-blitline-is-not"></a>Mida Blitline ei ole...

Selgitada Blitline on kasulik, on tihti lihtsam tuvastada, mis Blitline ei ole enne edasi.

- Blitline ei saa üles laadida pilte HTML-i vidinad. Pildid on saadaval peab teil avalikult või piiratud õigustega Blitline saavutamiseks saadaval.

- Blitline ei ole reaalajas pilditöötluse, nt Aviary.com

- Blitline piltide üleslaadimist ei aktsepteeri, ei saa vajutada oma pilte Blitline otse. Peate murravad Azure Storage või muudes kohtades Blitline toetab ja siis öelda Blitline, kust neid saada.

- Blitline on oluliselt paralleelne ja ei ole sünkroonse töötlemine. Mida tähendab peab meile on postback_url ja saame öelda, kui oleme teinud töötlemiseks.

## <a name="create-a-blitline-account"></a>Blitline konto loomine

[AZURE.INCLUDE [blitline-signup](../includes/blitline-signup.md)]

## <a name="how-to-create-a-blitline-job"></a>Kuidas luua Blitline töö

Blitline kasutab JSON määratleda toimingud, mida soovite pilti teha. See JSON koosneb mõned lihtsad väljad.

Lihtsaim näide on järgmine:

        json : '{
       "application_id": "MY_APP_ID",
       "src" : "http://cdn.blitline.com/filters/boys.jpeg",
       "functions" : [ {
           "name": "resize_to_fit",
           "params" : { "width": 240, "height": 140 },
           "save" : { "image_identifier" : "external_sample_1" }
       } ]
    }'

Siin on meil JSON, mis viivad "src" pilt "… boys.jpeg" ja seejärel muutke 240 x 140 selle pildi suurust.

Rakenduse ID on midagi leiate Azure'i oma **Ühenduse teave** või **Halda** menüü. See on teie salajane identifikaator, mis võimaldab teil töö käitamist Blitline.

"Salvesta" parameeter tuvastab kohta, kuhu soovite panna pilt, kui me olema töödeldud seda teavet. Sellisel juhul triviaalne meil pole määratletud ühte. Kui ole asukoht määratletud Blitline talletab selle kohalikult ajutiselt (ja) pilve kordumatu asukohas. On võimalik saada sinna JSON, kui teete selle Blitline Blitline tagastatud. Identifikaator "pilt" on vaja ja tagastatakse teile salvestamisel tuvastamiseks see konkreetne pilt.

Lisateavet *funktsioonide* toetame siit leiate: <http://www.blitline.com/docs/functions>

Dokumentatsiooni siin töö suvandite kohta leiate: <http://www.blitline.com/docs/api>

Kui olete oma JSON tegemiseks on vaja **postituse** seda`http://api.blitline.com/job`

Saate tagasi JSON, mis näeb välja umbes selline:

    {
     "results":
         {"images":
            [{
              "image_identifier":"external_sample_1",
              "s3_url":"https://s3.amazonaws.com/dev.blitline/2011110722/YOUR_APP_ID/CK3f0xBF_2bV6wf7gEZE8w.jpg"
            }],
          "job_id":"4eb8c9f72a50ee2a9900002f"
         }
    }


See teatab, et Blitline ei saanud teie taotlus, see pani selle töötlemine kuhjuda ja lõppu pilt on saadaval veebisaidil: **https://s3.amazonaws.com/dev.blitline/2011110722/YOUR\_rakenduse\_ID/CK3f0xBF_2bV6wf7gEZE8w.jpg**

## <a name="how-to-save-an-image-to-your-azure-storage-account"></a>Kuidas salvestada pilt Azure Storage konto

Kui teil on Azure Storage konto, saate määrata hõlpsalt Blitline lükake töödeldud pildid oma Azure ümbrises. Lisades "azure_destination" määratleda asukoht ja õiguste Blitline soovite lükata.

Siin on näide:

    job : '{
      "application_id": "YOUR_APP_ID",
      "src" : "http://www.google.com/logos/2011/houdini11-hp.jpg",
         "functions" : [{
         "name": "blur",
         "save" : {
             "image_identifier" : "YOUR_IMAGE_IDENTIFIER",
             "azure_destination" : {
                 "account_name" : "YOUR_AZURE_CONTAINER_NAME",
                 "shared_access_signature" : "SAS_THAT_GIVES_BLITLINE_PERMISSION_TO_WRITE_THIS_OBJECT_TO_CONTAINER",
               }
           }
         }]
       }'


Täitke oma CAPITALIZED väärtused, saate esitada selle JSON-http://api.blitline.com/job ja töödeldakse blur filtriga "src" pilt ning seejärel lükata Azure sihtkohta.

###<a name="please-note"></a>Pidage meeles järgmist.

MUUDE peab sisaldama kogu SAS URL-i, sh sihtfail failinimi.

Näide:

    http://blitline.blob.core.windows.net/sample/image.jpg?sr=b&sv=2012-02-12&st=2013-04-12T03%3A18%3A30Z&se=2013-04-12T04%3A18%3A30Z&sp=w&sig=Bte2hkkbwTT2sqlkkKLop2asByrE0sIfeesOwj7jNA5o%3D


Võite lugeda ka Blitline's Azure Storage dokumentide uusim [siin](http://www.blitline.com/docs/azure_storage).


## <a name="next-steps"></a>Järgmised sammud

Külastage blitline.com lugeda kõiki meie muid funktsioone:

* Blitline API lõpp-punkti dokumentide <http://www.blitline.com/docs/api>
* Blitline API funktsioone <http://www.blitline.com/docs/functions>
* Blitline API näited <http://www.blitline.com/docs/examples>
* Kolmas osa Nugeti teegi <http://nuget.org/packages/Blitline.Net>
