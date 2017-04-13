<properties 
    pageTitle="Seadme õ soovitused: JavaScripti integreerimine | Microsoft Azure'i" 
    description="Azure'i masina õ soovitused - JavaScripti seos - dokumendid" 
    services="machine-learning" 
    documentationCenter="" 
    authors="LuisCabrer" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="javascript" 
    ms.topic="article" 
    ms.date="09/08/2016" 
    ms.author="luisca"/>

# <a name="azure-machine-learning-recommendations---javascript-integration"></a>Õppekeskuse soovitusi - JavaScript integreerimine Azure seadme

>[AZURE.NOTE]Soovitused API kognitiivse teenuse peaks kasutuselevõtt, selle asemel, et see versioon. Soovitused kognitiivse teenus on asendades selle teenuse ja seal arendatakse uusi funktsioone. See on uue võimalused nagu partiide tugi, parem API Explorer, cleaner API Surface'i, ühtsema registreerimine/arveldamine kogemus jne.
> Lisateavet [uue kognitiivse teenuse migreerimine](http://aka.ms/recomigrate)


Selle dokumendi kujutatakse kuidas integreerida JavaScripti abil teie saidile. JavaScripti võimaldab saata sündmuste andmete kogumine ja kasutamine soovitusi, kui koostate soovitus mudeli. Kõik toimingud, mida teha JS kaudu saab teha ka serveripoolne.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

##<a name="1-general-overview"></a>1. Üldine ülevaade
2 etapist koosnevad saidi integreerimine Azure ML soovitusi:

1.  Saata sündmuste Azure ML soovitusi. See võimaldab koostada soovitus mudel.
2.  Tarbimine soovitusi. Pärast mudeli ehitatakse saate kasutada mobiilirakendustes soovitusi. (Selle dokumendi selgitatakse, kuidas luua mudel, lugege lühijuhendi kohta lisateabe saamiseks).


<ins>Etapp</ins>

Esimesel etapil saate lisada oma HTML-lehtede väike JavaScripti teek, mis võimaldab saata sündmuste esinevad HTML-i lehel Azure'i ML soovitused servereid (Andmeturg) kaudu lehe:

![Drawing1][1]

<ins>II etapp.</ins>

Kui soovite kuvada soovitusi lehe teise etapi saate valige üks järgmistest suvanditest:

1. teie serveris (lehe renderdamise etapp) helistab Azure'i ML soovitusi Server (Andmeturg) kaudu saada soovitusi. Tulemused sisaldavad loendi üksuste ID-d. Oma serveri peab rikastamine tulemused üksustega metaandmeid (nt pildid, kirjeldus) ja saatmine brauseri loodud lehele.

![Drawing2][2]

2. Teine võimalus on soovitatav üksuste loendit lihtsa etapp väike JavaScripti faili abil. Siin saadud andmed on lihtsam, kui esimene variant.

![Drawing3][3]

##<a name="2-prerequisites"></a>2. eeltingimused

1. Uue andmemudeli loomine abil soovitud API-d. Vaadake, kuidas seda teha lühijuhendi.
2. Kodeerida oma &lt;dataMarketUser&gt;:&lt;dataMarketKey&gt; koos base64. (See kasutatakse elementaarse autentimise lubamiseks JS kood on API-d).


##<a name="3-send-data-acquisition-events-using-javascript"></a>3. saata andmeid tuua sündmuste JavaScripti kasutamine
Järgmised toimingud hõlbustada saatva sündmused:

1.  Lisage JQuery teegi oma kood. Saate selle tasuta alla Nugeti järgmist URL-i kaudu.

        http://www.nuget.org/packages/jQuery/1.8.2
2.  Kaasa soovitused JavaScripti teegi järgmine URL: http://aka.ms/RecoJSLib1

3.  Azure'i ML soovitused teegi vastav parameetritega lähtestada.

        <script>
            AzureMLRecommendationsStart("<base64encoding of username:key>",
            "<model_id>");
        </script>

4.  Saatke vastav sündmus. Üksikasjalik jaotisest all kõik tüüpi sündmused (nt klõpsake sündmuse)

        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") {      
                        AzureMLRecommendationsEvent = [];
                    }
            AzureMLRecommendationsEvent.push({ event: "click", item: "18321116" });
        </script>


###<a name="31-limitations-and-browser-support"></a>3,1. Piirangud ja Brauseritugi
See on viide rakendamist ja see on antud on. See peaks toetama kõik põhi brauserites.

###<a name="32-type-of-events"></a>3,2. Sündmuse tüüp
On 5 tüüpi sündmus, mille teegi toetab: klõpsake nuppu, klõpsake soovitust, lisa ostukorv, eemaldage Ostukorv ja ostmine. On täiendavaid sündmuse abil saate seada kasutaja kontekstis nimetatakse Logi sisse.

####<a name="321-click-event"></a>3.2.1. Klõpsake sündmuse
Sel juhul tuleks kasutada iga kord, kui kasutaja üksuse klõpsamisel. Tavaliselt siis, kui kasutaja klõpsab üksuse uus leht on avatud üksuse üksikasjad; selle lehel käivitatakse see sündmus.

Parameetrid:
- sündmuse (stringi, kohustuslik) – "klõpsake"
- üksuse (stringi, kohustuslik) – üksuse ainuidentifikaator
- üksuse nimi (stringi, valikuline) – üksuse nimi
- itemDescription (stringi, valikuline) – üksuse kirjeldus
- üksuse kategooria itemCategory (stringi, valikuline) –
        
        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "click", item: "3111718" });
        </script>

Või valikuline andmetega:

        <script>
            if (typeof AzureMLRecommendationsEvent === "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "click", item: "3111718", itemName: "Plane", itemDescription: "It is a big plane", itemCategory: "Aviation"});
        </script>


####<a name="322-recommendation-click-event"></a>3.2.2. Soovitus klõpsake sündmuse
Sel juhul tuleks kasutada iga kord, kui kasutaja on klõpsanud üksuse, mis on saadud Azure'i ML soovitusi soovitatav üksuse. Tavaliselt siis, kui kasutaja klõpsab üksuse uus leht on avatud üksuse üksikasjad; selle lehel käivitatakse see sündmus.

Parameetrid:
- sündmuse (stringi, kohustuslik) – "recommendationclick"
- üksuse (stringi, kohustuslik) – üksuse ainuidentifikaator
- üksuse nimi (stringi, valikuline) – üksuse nimi
- itemDescription (stringi, valikuline) – üksuse kirjeldus
- üksuse kategooria itemCategory (stringi, valikuline) –
- seemned (stringi massiiv, valikuline.) - seemned, mis on loodud soovitus päring.
- recoList (stringi massiiv, valikuline) – üksus, mille klõpsatud genereerinud soovitus taotluse tulemus.
        
        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "recommendationclick", item: "18899918" });
        </script>

Või valikuline andmetega:

        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: eventName, item: "198", itemName: "Plane2", itemDescription: "It is a big plane2", itemCategory: "Default2", seeds: ["Seed1", "Seed2"], recoList: ["199", "198", "197"]               });
        </script>


####<a name="323-add-shopping-cart-event"></a>3.2.3. Ostude ostukorv sündmuse lisamine
Sel juhul tuleks kasutada kui kasutaja lisada üksuse ostukorvi.
Parameetrid:
* sündmuse (stringi, kohustuslik) – "addshopcart"
* üksuse (stringi, kohustuslik) – üksuse ainuidentifikaator
* üksuse nimi (stringi, valikuline) – üksuse nimi
* itemDescription (stringi, valikuline) – üksuse kirjeldus
* üksuse kategooria itemCategory (stringi, valikuline) –
        
        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "addshopcart", item: "13221118" });
        </script>

####<a name="324-remove-shopping-cart-event"></a>3.2.4. Eemaldage ostukorv sündmuse ostmine
Sel juhul tuleks kasutada, kui kasutaja eemaldab üksuse ostukorvi.

Parameetrid:
* sündmuse (stringi, kohustuslik) – "removeshopcart"
* üksuse (stringi, kohustuslik) – üksuse ainuidentifikaator
* üksuse nimi (stringi, valikuline) – üksuse nimi
* itemDescription (stringi, valikuline) – üksuse kirjeldus
* üksuse kategooria itemCategory (stringi, valikuline) –
        
        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "removeshopcart", item: "111118" });
        </script>

####<a name="325-purchase-event"></a>3.2.5. Sündmuse ostmine
Sel juhul tuleks kasutada, kui kasutaja ostetud oma ostukorv.

Parameetrid:
* sündmuse (stringi) – "osta"
* üksuste (ostetud []) - massiiv, hoides iga üksuse ostnud kirje.<br><br>
Ostetud vorming:
    * üksuse (stringi) – üksuse ainuidentifikaator.
    * Count (int või stringi) – üksuste arv, mis on ostetud.
    * Price (hõljumine või stringi) - valikuline välja – üksuse hinna.

Järgmises näites kuvatakse 3 ostu üksuste (33, 34, 35) kaks kõik väljad täidetud (üksuse, arv, hind) ja ühte (kirje 34) ilma hinnale.

        <script>
            if ( typeof AzureMLRecommendationsEvent == "undefined"){ AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "purchase", items: [{ item: "33", count: "1", price: "10" }, { item: "34", count: "2" }, { item: "35", count: "1", price: "210" }] });
        </script>

####<a name="326-user-login-event"></a>3.2.6. Kasutaja Logi sisse sündmuse
Azure'i ML soovitused sündmuse teegi loob ja kasutada küpsis tuvastamiseks sündmused, mis pärineb samas brauseris. Selleks, et parandada mudeli Azure'i ML soovitused võimaldab seada kasutaja kordumatu ID, mis alistab küpsiste kasutamise tulemusi.

Sel juhul tuleks kasutada pärast kasutaja sisselogimisandmed saidile.

Parameetrid:
* sündmuse (stringi) – "userlogin"
* kasutaja (stringi) – kasutaja kordumatu.
        <script>
  Kui (typeof AzureMLRecommendationsEvent == "määratlemata") {AzureMLRecommendationsEvent = [];} AzureMLRecommendationsEvent.push ({sündmuse: "userlogin" Kasutaja: "ABCD10AA"});      </script>

##<a name="4-consume-recommendations-via-javascript"></a>4. tarbimine soovitused JavaScripti kaudu
Tarbib soovitust koodis käivitab kliendi veebilehe osa JavaScripti sündmus järgi. Soovitus vastus sisaldab Soovitatavad üksused ID-d, nende nimed ja nende hinnangud. See kõige paremini, kasutage seda suvandit ainult jaoks soovitatavaid üksuste loendi Kuva - keerukamaid töötlemine (nt lisamine üksuse metaandmete) tuleks teha server pool integreerimine.

###<a name="41-consume-recommendations"></a>4.1 tarbimine soovitused
Kasutamine soovitusi, peate kaasata vajalik JavaScripti teekide oma lehele ja AzureMLRecommendationsStart helistada. Jaotisest 2.

Soovitused ühe või mitme üksuse, kellele peate helistama meetodit nimetatakse kasutamine: AzureMLRecommendationsGetI2IRecommendation.

Parameetrid:
* üksused (massiiv stringid) – üks või mitu üksust, et saada soovitusi. Kui teil peab olema mõne Fbt koostamine ja seejärel saab määrata ainult ühe üksuse.
* numberOfResults (int) - nõutav tulemite arv.
* includeMetadata (kahendväärtus, valikuline) – kui väärtuseks "true" näitab, et metaandmete välja peavad olema täidetud järgmised tulemis.
* Funktsioon – funktsioon, mis tegeleb soovitusi töötlemine tagastas. Massiivina, tagastatakse andmed:
    * Üksuse – üksuse ainuidentifikaator
    * nimi – üksuse nimi (kui on olemas kataloogi)
    * hindamine – soovitus reiting
    * metaandmete - string, mis tähistab üksuse metaandmeid

Näide: Järgmine kood taotleb 8 soovitused üksuse "64f6eb0d-947a-4c18-a16c-888da9e228ba" (ja mitte määramisega includeMetadata - peidetult kuvatakse teade pole metaandmete nõutakse), see siis concatenate tulemused puhvrit sisse.

        <script>
            var reco = AzureMLRecommendationsGetI2IRecommendation(["64f6eb0d-947a-4c18-a16c-888da9e228ba"], 8, false, function (reco) {
                var buff = "";
                for (var ii = 0; ii < reco.length; ii++) {
                    buff += reco[ii].item + "," + reco[ii].name + "," + reco[ii].rating + "\n";
                }
                alert(buff);
            });
        </script>


[1]: ./media/machine-learning-recommendation-api-javascript-integration/Drawing1.png
[2]: ./media/machine-learning-recommendation-api-javascript-integration/Drawing2.png
[3]: ./media/machine-learning-recommendation-api-javascript-integration/Drawing3.png
 
