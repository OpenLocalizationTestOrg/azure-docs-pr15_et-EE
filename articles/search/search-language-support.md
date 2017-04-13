<properties
   pageTitle="Registri dokumentide loomine Azure otsingu mitmes keeles | Microsoft Azure'i | Majutatud pilveteenuses otsing"
   description=" Azure'i otsingu toetab 56 keeles, keele analüsaatorid Lucene ja loomulikus keeles töötlemine Microsofti tehnoloogia kasutamine."
   services="search"
   documentationCenter=""
   authors="yahnoosh"
   manager="pablocas"
   editor=""/>

<tags
   ms.service="search"
   ms.devlang="na"
   ms.workload="search"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.date="07/14/2016"
   ms.author="jlembicz"/>

# <a name="create-an-index-for-documents-in-multiple-languages-in-azure-search"></a>Azure'i otsingu mitmes keeles registri dokumentide loomine
> [AZURE.SELECTOR]
- [Portaal](search-language-support.md)
- [ÜLEJÄÄNUD](https://msdn.microsoft.com/library/azure/dn879793.aspx)
- [.NET-I](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.analyzername.aspx)

Unleashing keele analüsaatorid power on sama lihtne kui säte ühe atribuudi otsitavate välja indeksi määratlus. Nüüd saate teha selle etapi portaalis.

Allpool on Azure portaali labad kuvatõmmised Azure otsingu, mis võimaldab kasutajatel määratleda index skeemiga. See blade kaudu kasutajad saate luua kõik väljad ja neid atribuudi analyzer.

> [AZURE.IMPORTANT] Ainult saate keele analüsaator ajal välja määratlus, nagu üles uus indeks maa loomisel või olemasoleva registri uue välja lisamisel. Veenduge, et teie määratud täielikult välja loomisel analyzer, sh kõik atribuudid. Te ei saa redigeerida atribuute tüübi või seda muuta analüsaator pärast muudatuste salvestamiseks.

## <a name="define-a-new-field-definition"></a>Uue välja määratluse määratlemine

1. [Azure portaali](https://portal.azure.com) sisse ja avage oma otsinguteenuse teenuse tera.
2. Klõpsake nuppu **Lisa register** käsuriba alustamiseks uus indeks teenuse armatuurlaua ülaosas või avada olemasoleva registri seada mõne analüsaator lisate olemasolevat registrit uusi välju.
3. Tera väljad kuvatakse, võimaldades suvandite määratlemiseks skeemi registri, sh analüsaator menüü kasutada valimise keele analüsaator.
4. Väljad, hakake määratlus välja nimi, valides andmetüüpi, ja märkida välja täisteksti otsitavate, tagastatava otsingutulemites kasutatavad tahukas navigeerimise struktuuri, sorditav ja VM atribuutide seadmiseks. 
5. Enne liigub järgmisele väljale, avage vahekaart **analüsaator** . 

   
![][1]
*Mõne analyzer valimiseks klõpsake vahekaarti Analyzer väljad enne*

## <a name="choose-an-analyzer"></a>Valige soovitud analüsaator

6. Leidke väli määratlemisel. 
7. Kui välja otsitavate pole märgitud, märkige ruut kohe, et märkida see **Otsitav**.
8. Klõpsake loendis Saadaolevad analüsaatorid kuvamiseks analüsaator ala.
9. Valige analüsaator, mida soovite kasutada.

![][2]
*Valige üks järgmistest toetatud analüsaatorid iga välja jaoks*

Vaikimisi kasutavad kõik otsitavad väljad [Standard Lucene analüsaator](http://lucene.apache.org/core/4_10_0/analyzers-common/org/apache/lucene/analysis/standard/StandardAnalyzer.html) , mis on keele agnostik. Täieliku loendi toetatud analüsaatorid, leiate [Azure'i otsingus keelte tugi](https://msdn.microsoft.com/library/azure/dn879793.aspx).

Kui keel analüsaator on valitud välja jaoks, seda kasutatakse iga indekseerimine ja otsing taotlusega välja. Kui päring on antud kasutamine eri analüsaatorid mitmest väljast, päringu töötleb sõltumatult õige analüsaatorid iga välja jaoks.

Paljud web ja mobiilirakenduste olla kasutajatele üle maailma erinevates keeltes. On võimalik määratlemine registri jaoks stsenaarium järgmiselt loomise välja iga keele toetatud.

![][3]
*Registri määratlemine iga keele toetatud väljaga kirjeldus*

Kui keele agent, emissiooni päring on teada, Otsingutaotluse rakendatud **searchFields** Päringuparameetri kasutamine konkreetsel väljal. Järgmine päring antakse ainult vastu Poola kirjeldus:

`https://[service name].search.windows.net/indexes/[index name]/docs?search=darmowy&searchFields=description_pl&api-version=2015-02-28`

Saate päringu oma registri portaalis **Otsingu** Exploreriga sarnaneb eeltoodud päringu kleepida. Otsingu explorer on saadaval käsuriba teenuse tera. Üksikasjad leiate [Azure'i otsinguregistri portaalis päringu](search-explorer.md) .

Mõnikord agent, emissiooni päringu keele pole teada, millisel juhul päring saab üheaegselt vastu kõik väljad. Vajadusel saate eelistust tulemused teatud keeles määratletud abil [hinded profiilid](https://msdn.microsoft.com/library/azure/dn798928.aspx). Järgmises näites kuvatakse vasteid leitud kirjeldus inglise keeles reageerima suhtes vasteid Poola ja Prantsuse kõrgem:

    "scoringProfiles": [
      {
        "name": "englishFirst",
        "text": {
          "weights": { "description_en": 2 }
        }
      }
    ]

`https://[service name].search.windows.net/indexes/[index name]/docs?search=Microsoft&scoringProfile=englishFirst&api-version=2015-02-28`

Kui olete .NET arendaja, Pange tähele keele analüsaatorid [Azure'i otsingu .NET SDK](http://www.nuget.org/packages/Microsoft.Azure.Search)abil saate konfigureerida. Uusim versioon sisaldab ka analüsaatorid Microsoft keele tuge.

<!-- Image References -->
[1]: ./media/search-language-support/AnalyzerTab.png
[2]: ./media/search-language-support/SelectAnalyzer.png
[3]: ./media/search-language-support/IndexDefinition.png
