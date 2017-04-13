
<properties
    pageTitle="Saada soovitused pakettidena: masina Õppekeskuse soovitused API | Microsoft Azure'i"
    description="Õppekeskuse soovitused--saada soovitused pakettidena Azure seadme"
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
    ms.date="08/17/2016"
    ms.author="luisca"/>

# <a name="get-recommendations-in-batches"></a>Hangi soovitusi pakettidena

>[AZURE.NOTE] Soovitused pakettidena on keerulisem kui saada soovitusi ühe korraga. Teavet selle kohta, kuidas saada soovitused ühe päringu API kontrollimine

> [Üksuse-üksuse soovitused](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3d4)<br>
> [Kasutaja-üksuse soovitused](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3dd)
>
> Paketi hinded toimib ainult järgud, mis on loodud pärast 21 juuli 2016.


Vahel tekkida olukordi, kus soovite saada soovitused mitme üksuse korraga. Näiteks võib olla huvitatud soovitused vahemälu loomine või isegi analüüsimine tüüpi soovitusi, mida te saada.

Hinded toimingud, nagu me kutsume neid, on asünkroonsete toimingute. Peate taotluse esitamine, ootama toimingu lõpuleviimiseks ja seejärel kogumine tulemid.  

Rohkem täpne, need on tehke järgmist:

1.  Kui teil pole seda juba mõne Azure Storage container loomine
2.  Kõik teie soovitus taotlused Azure'i bloobimälu kirjeldav Sisestuskeel-faili laadida.
3.  Alustage hinded pakett.
4.  Oodake, kuni asünkroonse toimingu lõpuleviimiseks.
5.  Kui toiming on lõpule jõudnud, kogumine bloobimälu tulemused.

Vaatame iga järgmist.

## <a name="create-a-storage-container-if-you-dont-have-one-already"></a>Salvestusruumi container loomine, kui teil pole seda juba

[Azure'i portaal](https://portal.azure.com) ja salvestusruumi uue konto loomine, kui teil pole veel üks. Liikuge selle tegemiseks **Uus** > **andmete** + **salvestusruumi** > **Salvestusruumi konto**.

Kui teil on salvestusruumi konto, peate looma bloobimälu ümbriste, kus saate talletada sisestus- ja täitmise paketi väljundit.

Laadi Sisestuskeel fail, mis kirjeldab iga teie soovitus taotleb bloobimälu--vaatame helistage faili input.json siin.
Kui olete ümbris, peate üles fail, mis kirjeldab iga taotlusi, mida tuleb teha teenuse soovitused.

Partii saate teha ainult ühte tüüpi taotluse kaudu teatud koostamine. Me selgitab, kuidas määratleda seda teavet järgmisest jaotisest. Nüüd, Oletame, et saaksime läbimiseks üksuse soovitused teatud Koosta välja. Sisestuskeel fail sisaldab Sisestuskeel teave (sel juhul seemne üksuste) iga taotluse.

See on input.json faili näeb näide:

    {
      "requests": [
      { "SeedItems": [ "C9F-00163", "FKF-00689" ] },
      { "SeedItems": [ "F34-03453" ] },
      { "SeedItems": [ "D16-3244" ] },
      { "SeedItems": [ "C9F-00163", "FKF-00689" ] },
      { "SeedItems": [ "F43-01467" ] },
      { "SeedItems": [ "BD5-06013" ] },
      { "SeedItems": [ "P45-00163", "FKF-00689" ] },
      { "SeedItems": [ "C9A-69320" ] }
      ]
    }

Nagu näete, on faili JSON faili, kus iga taotluse on teavet, mida on vaja saata soovitusi. Sarnaselt JSON faili taotlusi, mida soovite täita ja kopeerimiseks äsja loodud bloobimälu ümbrises.

## <a name="kick-start-the-batch-job"></a>Alustage pakett-töö

Järgmiseks on uus töökoht paketi esitada. Lisateabe saamiseks märkige [API viide](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/).

Koosolekukutse kehasse API tuleb määratleda kui tõrge failide sisendi, väljundi ja peavad olema salvestatud asukohad. Samuti peab määratleda identimisteavet, mida on vaja juurde pääseda need asukohad. Lisaks peate määrama mõned parameetrid, mis rakenduvad kogu pakett (tüüpi soovitusi taotlemine, mudeli/koostamine kasutamiseks kõne, kohta tulemite arv ja muudeks tegevusteks.)

See on näide, mis taotluse sisu peaks välja nägema:

    {
      "input": {
        "authenticationType": "PublicOrSas",
        "baseLocation": "https://mystorage1.blob.core.windows.net/",
        "relativeLocation": "container1/batchInput.json",
        "sasBlobToken": "?sv=2015-07_restofToken_…4Z&sp=rw"
      },
      "output": {
        "authenticationType": "PublicOrSas",
        "baseLocation": "https://mystorage1.blob.core.windows.net/",
        "relativeLocation": "container1/batchOutput.json ",
        "sasBlobToken": "?sv=2015-07_restofToken_…4Z&sp=rw"
      },
      "error": {
        "authenticationType": "PublicOrSas",
        "baseLocation": "https://mystorage1.blob.core.windows.net/",
        "relativeLocation": "container1/errors.txt",
        "sasBlobToken": "?sv=2015-07_restofToken_…4Z&sp=rw"
      },
      "job": {
        "apiName": "ItemRecommend",
        "modelId": "9ac12a0a-1add-4bdc-bf42-c6517942b3a6",
        "buildId": 1015703,
        "numberOfResults": 10,
        "includeMetadata": true,
        "minimalScore": 0.0
      }
    }

Järgnevalt mõned olulised asjad, Pange tähele, et:

-   Praegu **authenticationType** tuleks alati olema seatud **PublicOrSas**.

-   Vajate soovitusi API lugeda ja kirjutada bloobimälu salvestusruumi konto, et lubada märgiks ühiskasutusse Accessi allkirja (SAS). Lisateavet selle kohta, kuidas luua SAS sõned leiate lehel [soovituste API](../storage/storage-dotnet-shared-access-signature-part-1.md).

-   Ainult **apiName** , mis ei toeta praegu on **ItemRecommend**, üksuse – üksuse soovitused kasutatav. Partiide ei toeta praegu kasutaja-üksuse soovitusi.

## <a name="wait-for-the-asynchronous-operation-to-finish"></a>Oodake, kuni asünkroonse toimingu lõpuleviimiseks

Paketi toimingu käivitamisel tagastab vastuse toiming-asukoht päise, mis annab teile teavet, mida on vaja jälgida toiming.
Saate jälgida selle toimingu abil [Tuua toiming oleku API]( https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3da)täpselt samamoodi nagu teilgi jälgimise järk toiming toimingut.

## <a name="get-the-results"></a>Tulemuse saamiseks

Kui toiming on lõpule jõudnud, eeldades, et ilmnenud tõrkeid, tulemused saate kogumine oma väljundi Bloobivahemälu salvestusruumi.

Järgmises näites kuvatakse väljund võib välja näha umbes. Selles näites me kuvada partii tulemusi ainult kaks taotlused (lühike).

    {
      "results":
      [   
        {
          "request": { "seedItems": [ "DAF-00500", "P3T-00003" ] },
          "recommendations": [
            {
              "items": [
                {
                  "itemId": "F5U-00011",
                  "name": "L2 Ethernet Adapter-Win8Pro SC EN/XD/XX Hdwr",
                  "metadata": ""
                }
              ],
              "rating": 0.722,
              "reasoning": [ "People who like the selected items also like 'L2 Ethernet Adapter-Win8Pro SC EN/XD/XX Hdwr'" ]
            },
            {
              "items": [
                {
                  "itemId": "G5Y-00001",
                  "name": "Docking Station for Srf Pro/Pro2 SC EN/XD/ES Hdwr",
                  "metadata": ""
                }
              ],
              "rating": 0.718,
              "reasoning": [ "People who like the selected items also like 'Docking Station for Srf Pro/Pro2 SC EN/XD/ES Hdwr'" ]
            }
          ]
        },
        {
          "request": { "seedItems": [ "C9F-00163" ] },
          "recommendations": [
            {
              "items": [
                {
                  "itemId": "C9F-00172",
                  "name": "Nokia 2K Shell for Nokia Lumia 630/635 - Green",
                  "metadata": ""
                }
              ],
              "rating": 0.649,
              "reasoning": [ "People who like 'MOZO Flip Cover for Nokia Lumia 635 - White' also like 'Nokia 2K Shell for Nokia Lumia 630/635 - Green'" ]
            },
            {
              "items": [
                {
                  "itemId": "C9F-00171",
                  "name": "Nokia 2K Shell for Nokia Lumia 630/635 - Orange",
                  "metadata": ""
                }
              ],
              "rating": 0.647,
              "reasoning": [ "People who like 'MOZO Flip Cover for Nokia Lumia 635 - White' also like 'Nokia 2K Shell for Nokia Lumia 630/635 - Orange'" ]
            },
            {
              "items": [
                {
                  "itemId": "C9F-00170",
                  "name": "Nokia 2K Shell for Nokia Lumia 630/635 - Yellow",
                  "metadata": ""
                }
              ],
              "rating": 0.646,
              "reasoning": [ "People who like 'MOZO Flip Cover for Nokia Lumia 635 - White' also like 'Nokia 2K Shell for Nokia Lumia 630/635 - Yellow'" ]
            }       
          ]
        }
    ]}


## <a name="learn-about-the-limitations"></a>Piirangud

-   Ainult üks pakett saab kutsuda korraga tellimuse kohta.
-   Paketi töö Sisestuskeel faili ei saa üle 2 MB.
