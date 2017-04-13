<properties
    pageTitle="Azure'i otsingu abil indexers Azure'i portaalis andmete importimine | Microsoft Azure'i | Majutatud pilveteenuses otsing"
    description="Azure'i otsingu impordi andmed viisardi abil Azure'i portaalis analüüsi andmete Azure'i bloobimälu salvestusruumi, tabeli stroage, SQL-andmebaas ja SQL Server Azure'i VMs."
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="jhubbard"
    editor=""
    tags="Azure Portal"/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="heidist"/>

# <a name="import-data-to-azure-search-using-the-portal"></a>Andmete importimine portaalis Azure otsimiseks

Registri andmete laadimine annab Azure portaali armatuurlaual Azure'i otsing on **Andmete importimise** viisard. 

  ![Andmete importimine käsku][1]

Sees, viisardi konfigureerib ja käivitab mõne *indekseerija*mitu toimingut indekseerimise protsessi automatiseerimine. 

- Praeguse tellimuse Azure'i välise andmeallikaga ühenduse loomine
- Tabelisse skeemiga registri põhjal andmeallika andmestruktuur
- Dokumentide lisamine soovitud andmeallikast Reahulk põhjal loomine
- Registri oma otsinguteenuse dokumentide üleslaadimine

Võite proovida selle töövoo DocumentDB Näidisandmete abil. Külastage [Alustamine Azure'i otsingu Azure'i portaalis](search-get-started-portal.md) juhised.

## <a name="data-sources-supported-by-the-import-data-wizard"></a>Andmete importimise viisard ei toeta andmeallikad

Andmete importimine viisardi toetab järgmiste andmeallikatega: 

- Azure'i SQL-andmebaas
- SQL serveri relatsiooniandmete on Azure VM
- Azure'i DocumentDB
- Azure'i bloobimälu (jaotises Eelvaade)
- Azure'i tabelimälu (jaotises Eelvaade)

Lapik andmekomplekti on nõutavad andmed. Saate importida ainult ühe tabeli, andmebaasi vaade või samaväärne andmestruktuur. Enne viisardi käivitamist tuleks luua selle andmestruktuur.

Pange tähele, et mõne funktsiooni indexers on endiselt eelvaade, mis tähendab, et indekseerija määratluse toetavad API eelvaateversiooni. Vt lisateavet ja linke [indekseerija ülevaade](search-indexer-overview.md) .

## <a name="connect-to-your-data"></a>Oma andmetega ühenduse loomine

1. Logige sisse [Azure portaali](https://portal.azure.com) ja avatud teenuste armatuurlaud. Võite klõpsata **Otsingu teenuste** hüpata riba kuvamiseks praeguse tellimuse olemasolevad teenused. 

2. Klõpsake slaidi Ava andmete importimine tera käsuriba **Andmete importimine** .  

3. Klõpsake nuppu **ühenduse andmete** määramiseks on kasutatud indekseerija andmeallika määratlus. Siseste-tellimuse andmeallikate viisardi tavaliselt tuvastab ja lugeda ühenduseteavet, minimeerimine üldine konfiguratsiooni nõuded.

| | |
|--------|------------|
|**Olemasolevas andmeallikas** | Kui teil on juba teie otsinguteenuse määratletud indexers, saate valida mõne olemasoleva andmeallika määratluse teise importimiseks.|
|**Azure'i SQL-andmebaas** | Lehe või mõne ühendusstringi ADO.net-i kaudu saab määrata lugemisõigus andmebaasi kasutaja mandaat teenuse nimi ja andmebaasi nimi. Valige suvand ühenduse stringi vaatamiseks või kohandamine atribuudid. <br/><br/>Klõpsake lehel peab olema määratud tabeli või vaate, mis sisaldab soovitud Reahulk. See suvand kuvatakse, kui ühendus on loodud, andes rippmenüü loendi nii, et tehke valik.|
|**SQL Server Azure'i VM** | Määrata teenuse nõuetele täielikult vastav nimi, kasutajanimi ja parool ja andmebaasi ühendusstringi. Selle andmeallika kasutamiseks peab teil olema eelnevalt installitud serdi kohaliku salvestada, mis krüptib ühendus. <br/><br/>Klõpsake lehel peab olema määratud tabeli või vaate, mis sisaldab soovitud Reahulk. See suvand kuvatakse, kui ühendus on loodud, andes rippmenüü loendi nii, et tehke valik.
|**DocumentDB** |Nõuded sisaldavad konto, andmebaasi ja saidikogumi. Kõigi dokumentide kogumine kaasatakse indeks. Saate määratleda päringu Ühenda või filtreerimiseks on Reahulk või muudetud dokumendid järgmiste andmete värskendamise toimingute tuvastamiseks.|
|**Azure'i bloobimälu** | Nõuded sisaldavad salvestusruumi konto ja ümbris. Soovi korral võite kui rühmitamise eesmärgil virtuaalse nimereeglistik bloobimälu nimed täitmist saate määrata virtuaalse directory osa nimi jaotises container kausta. Lisateabe saamiseks vaadake [Indekseerimise bloobimälu (eelvaade)](search-howto-indexing-azure-blob-storage.md) . |
|**Azure'i Tabelimälu** | Nõuded sisaldavad salvestusruumi konto ja tabeli nimi. Soovi korral saate määrata päringu tabelid alamhulga toomiseks. Lisateabe saamiseks vaadake [Indekseerimise Tabelimälu (eelvaade)](search-howto-indexing-azure-tables.md) . |

## <a name="customize-target-index"></a>Sihtrakenduse index kohandamine

Esialgne indeks on tavaliselt tuletatud andmekomplekti. Lisamine, redigeerimine või kustutamine väljade skeemi lõpuleviimiseks. Lisaks määrata atribuute välja määratlemiseks selle hilisema otsingu käitumist.

1. Määrake **kohandamine target index**nime ja kordumatult tuvastada iga dokumendi **võti** . Võti peab olema string. Kui väljaväärtuste kaasata tühikute või sidekriipsudena kindlasti **andmete importimine** mis tahes valideerimine sisse need märgid täpsemate suvandite määramine.

2. Vaadake üle ja parandage ülejäänud väljad. Välja nimi ja tüüp on tavaliselt teie eest täidetud. Saate muuta selle andmetüüpi.

3. Registri atribuudid iga välja seadmiseks tehke järgmist.

 - Otsingutulemite tagastatava tagastab välja.
 - Filtreeritavad võimaldab filtriavaldiste viidatakse välja.
 - Sorditav võimaldab kasutada sortimine välja.
 - Facetable võimaldab välja lihvitud navigeerimiseks.
 - Otsitav võimaldab otsingu.
  
4. Klõpsake vahekaarti **analüsaator** , kui soovite määrata keele analüsaator tasemel välja. Praegu saab määrata ainult keele analüsaatorid. Kohandatud analüsaator või-keele analüsaator märksõna, mustri ja nii edasi, nagu on vaja koodi.

 - Klõpsake otsingu välja ja lubada analüsaator ripploendist **Otsitav** .
 - Valige soovitud analüsaator. Lisateavet leiate teemast [loomine dokumentide mitmes keeles register](search-language-support.md) .

5. Klõpsake **Suggester** ettetippimise päringusoovituste valitud väljade lubamiseks.


## <a name="import-your-data"></a>Andmete importimine

1. **Andmete importimine**, sisestage selle indekseerija nimi. Meelde, et andmete importimine viisardi toode on indekseerija. Hiljem, kui soovite seda vaadata või redigeerida, valite selle portaalis, mitte taaskäivitamisel viisard. 

2. Määrake ajakava, mille aluseks on teenus on ette valmistatud piirkondliku ajavöönd.

3. Täpsemate suvandite määramiseks klõpsake kas Indekseerimise jätkamiseks kui dokumendi katkeb thresholds seadmine. Lisaks saate määrata, kas **klahvi** väljad on lubatud sisaldada tühikuid ja kaldkriipsud.  

## <a name="edit-an-existing-indexer"></a>Olemasoleva indekseerija redigeerimine

Teenuste armatuurlaua, topeltklõpsake indekseerija paani kõik indexers loodud tellimuse loetelu slaidile. Topeltklõpsake mõnda indexers käivitada, redigeerida või kustutada. Saate registri asendamiseks mõne muu olemasoleva, andmeallika muutmine ja indekseerimise ajal tõrge thresholds suvandite seadmine.

## <a name="edit-an-existing-index"></a>Olemasoleva registri redigeerimine

Azure'i otsinguväljale strukturaalset värskendused registri vajavad selle indeks, mis koosneb registri kustutada, taasloomine registri ja pealelaadimisel andmeid taastada. Strukturaalset värskendused hõlmavad andmete tüübi muutmine ja ümber nimetada või kustutada välja.

Muudatused, mis ei nõua taastada hulka lisada uue välja muutmise muutmine suggesters või muuta keele analüsaatorid hinded profiilid. Lisateabe saamiseks vaadake [Värskenda register](https://msdn.microsoft.com/library/azure/dn800964.aspx) .

## <a name="next-step"></a>Järgmise juhise juurde

Vaadake lisateavet indexers linkidest:

- [Indekseerimise Azure'i SQL-andmebaas](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md)
- [Indekseerimise DocumentDB](../documentdb/documentdb-search-indexer.md)
- [Indekseerimise bloobimälu (eelvaade)](search-howto-indexing-azure-blob-storage.md)
- [Indekseerimise Tabelimälu (eelvaade)](search-howto-indexing-azure-tables.md)



<!--Image references-->
[1]: ./media/search-import-data-portal/search-import-data-command.png

