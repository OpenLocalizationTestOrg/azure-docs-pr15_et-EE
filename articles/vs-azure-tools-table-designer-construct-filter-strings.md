<properties
   pageTitle="Tabeli Designer stringide filtri ehitamine | Microsoft Azure'i"
   description="Stringide filtri Designer tabeli loomine"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="storage"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="constructing-filter-strings-for-the-table-designer"></a>Stringide filtri Designer tabeli loomine

## <a name="overview"></a>Ülevaade

Azure'i tabeli, mis kuvatakse Visual Studio **Tabeli Designer**andmete filtreerimiseks ehitada filter stringi ja sisestage see väljale filter. Filtri stringi süntaks on määratletud WCF Data Services ja sarnaneb on SQL-i WHERE-klausel, kuid saadetakse tabeli teenuse kaudu HTTP-päring. **Tabeli Designer** tegeleb proper kodeering teile, et filtreerida on soovitud väärtus, tuleb sisestada ainult atribuudi nimi, võrdlusmärk, kriteeriumid väärtus ja soovi korral võite kahendmuutuja tehtemärk väljale filter. Sisaldavad $filter päring suvandit nii, nagu teeksite, kui teil oli ehitamine URL-i päringu tabeli kaudu [Salvestusruumi teenuste REST API viide](http://go.microsoft.com/fwlink/p/?LinkId=400447)pole vaja.

WCF Data Services põhinevad [Open Data protokolli](http://go.microsoft.com/fwlink/p/?LinkId=214805) (OData). Filtri süsteemi päring suvandit (**$filter**) kohta täpsema teabe saamiseks vt [OData URI põhimõtted määratlus](http://go.microsoft.com/fwlink/p/?LinkId=214806).

## <a name="comparison-operators"></a>Võrdluse tehtemärgid

Kõik objektid on toetatud järgmised loogikatehtemärkide:

|Loogika tehtemärkide|Kirjeldus|Näide filter string|
|---|---|---|
|EQ|Võrdne|Linna eq "Redmond"|
|gt|Suurem kui|Hind gt 20|
|GE|Suurem kui või võrdne|Hind ge 10|
|lt|Väiksem kui|Hind lt 20|
|Le|Väiksem või võrdne|Le hind 100|
|ne|Ei võrdu|Linna ne "Londoni"|
|ja|Ja|Hind le 200 ja hinna gt 3.5|
|või|Või|Hind le 3.5 või hind gt 200|
|pole|Pole|pole isAvailable|

Kui ehitamine filter stringi, on oluline järgmisi reegleid:

- Loogika tehtemärkide abil saate võrrelda atribuudi väärtuseks. Pange tähele, et pole võimalik võrrelda atribuudi väärtuseks dünaamiliseks; ühele küljele avaldis peab olema konstant.

- Filtri stringi kõik osad on tõstutundlikud.

- Selle järele konstandi väärtus peab olema sama tüüpi andmeid selleks, et filter atribuudina kehtiv tulemite tagastamiseks. Toetatud objektid kohta leiate lisateavet teemast [tabeli teenuse andmemudeli mõistmine](http://go.microsoft.com/fwlink/p/?LinkId=400448).

## <a name="filtering-on-string-properties"></a>Filtreerimine stringi atribuudid

Stringi atribuudid filtreerimisel soovitud stringikonstant ümbritsema ülakomadega.

Järgmises näites filtrid atribuudid **PartitionKey** ja **RowKey** ; täiendavad-key atribuudid võib lisada ka filtri stringi:

    PartitionKey eq 'Partition1' and RowKey eq '00001'

Saate lisada iga filtriavaldise sulgudes, kuigi see ei ole vaja:

    (PartitionKey eq 'Partition1') and (RowKey eq '00001')

Pange tähele, et ei toeta tabelis päringute metamärkide nad ei toeta tabelis Designer kas. Siiski saate teha eesliite võrdlustehete abil, klõpsake soovitud eesliite sobitamine. Järgmine näide tagastab üksused, kui perekonnanimi atribuut algab tähega "A":

    LastName ge 'A' and LastName lt 'B'

## <a name="filtering-on-numeric-properties"></a>Arvuliste atribuutide filtreerimine

Täisarv või Ujukomaarv filtreerimiseks määrata ilma jutumärkideta.

Selles näites tagastatakse kõik üksused, mille väärtus on suurem kui 30 vanus atribuudiga:

    Age gt 30

Selles näites tagastatakse kõik üksused, mille väärtus on väiksem või võrdne 100.25 AmountDue atribuudiga:

    AmountDue le 100.25

## <a name="filtering-on-boolean-properties"></a>Kahendmuutujaga atribuudid filtreerimine

Määrake filtreerimiseks loogikaväärtus **true** või **false** ilma jutumärkideta.

Järgmine näide tagastab kõik üksused, kus IsActive atribuut on seatud **tõene**:

    IsActive eq true

Samuti saate kirjutada selle filtriavaldise ilma loogika tehtemärkide. Järgmises näites ka tulemuseks tabeli teenuse kõik üksused, kus IsActive on **täidetud**:

    IsActive

Tagastada kõik üksused, kus IsActive on false, võite kasutada puudub tehtemärk:

    not IsActive

## <a name="filtering-on-datetime-properties"></a>Kuupäeva ja kellaaja atribuudid filtreerimine

Kuupäeva ja kellaaja väärtuse järgi filtreerimiseks määrata märksõna **kuupäev ja kellaaeg** , kuupäev/kellaaeg konstandi ülakomadega, millele järgneb. Kuupäeva/kellaaja konstandi peab olema ühendatud UTC-vormingus [DateTime kinnisvarahindade vormingu](http://go.microsoft.com/fwlink/p/?LinkId=400449)kirjeldatud.

Järgmine näide tagastab üksused, kus CustomerSince atribuut on võrdne 10 juuli 2008:

    CustomerSince eq datetime'2008-07-10T00:00:00Z'
