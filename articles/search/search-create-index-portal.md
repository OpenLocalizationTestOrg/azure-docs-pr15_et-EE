<properties
    pageTitle="Azure'i portaalis Azure otsingu registri loomine | Microsoft Azure'i | Majutatud pilveteenuses otsing"
    description="Azure'i portaalis registri loomine."
    services="search"
    manager="jhubbard"
    authors="ashmaka"
    documentationCenter=""/>

<tags
    ms.service="search"
    ms.devlang="NA"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="create-an-azure-search-index-using-the-azure-portal"></a>Azure'i portaalis Azure otsingu registri loomine
> [AZURE.SELECTOR]
- [Ülevaade](search-what-is-an-index.md)
- [Portaal](search-create-index-portal.md)
- [.NET-I](search-create-index-dotnet.md)
- [ÜLEJÄÄNUD](search-create-index-rest-api.md)

Selles artiklis juhendab teid Azure'i portaalis Azure otsingu [registri](search-what-is-an-index.md) loomise protsess.

Enne pärast selle juhendi ja registri loomise, peaks teil olema juba [loodud teenuse Azure otsing](search-create-service-portal.md).


## <a name="i-go-to-your-azure-search-blade"></a>Ma. Minge oma Azure'i otsingu blade
1. Klõpsake "Kõik ressursid" [Azure portaali](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) vasakus servas menüü
2. Valige oma Azure'i otsinguteenuse

## <a name="ii-add-and-name-your-index"></a>II. Lisamine ja pange oma index
1. Klõpsake nuppu "Lisa indeks"
2. Azure'i otsinguregistri nime. Kuna loome registri hotellist sellest juhendist otsida, on meil nimeks meie index "hotellist".
  * Registri nimi peab algama tähega ja sisaldavad ainult väiketähed, numbrit või kriipsud ("-").
  * Sarnaselt oma teenuse nimi, saate valida indeksi nimi on ka osa URL-i lõpp-punkti kui saadate oma HTTP päringuid Azure'i otsingu API
3. "Väljad" kirje uue blade avamiseks klõpsake

![](./media/search-create-index-portal/add-index.png)


## <a name="iii-create-and-define-the-fields-of-your-index"></a>III. Saate luua ja määratleda oma registri väljad
1. "Väljad" kirje valides avab uue blade vormi, et sisestada definitsiooni indeks.
2. Väljade lisamiseks oma vormi indeks.

  * *Klahv* välja tüüpi Edm.String on iga Azure'i otsinguregistri kohustuslik. Võtme väli luuakse vaikimisi välja nime "ID". Meil on muutunud see "hotelId" meie registrisse.
  * Teatud skeemi registri atribuudid saate määrata ainult üks kord ja ei saa värskendada tulevikus. Seetõttu ei ole skeemi värskendusi, mis nõuavad uuesti indekseerimise nagu Väljatüübid muutmine pärast esialgne konfiguratsioon praegu võimalik.
  * Valisime hoolikalt atribuudiväärtused iga välja alusel, kuidas me arvate, et neid kasutatakse rakenduses. Pidage meeles oma otsingu kasutaja kogemusi ja ettevõtte vajadustele oma registri kujundamisel, nagu iga välja peab olema määratud [sobiv atribuudid](https://msdn.microsoft.com/library/azure/dn798941.aspx). Millised väljad rakendada nende atribuutide kontrollimiseks, mis otsida funktsioonid (filtreerimine, faceting, sortimine, otsingu jne). Näiteks, on tõenäoline, et läheduses otsimise inimesed on huvitatud märksõna vasteid "kirjeldus" välja nii, et saaksime lubamine otsingu välja, seades atribuuti "Otsitav".
    * Samuti saate [keele analüsaator](https://msdn.microsoft.com/en-us/library/azure/dn879793.aspx) iga välja jaoks, klõpsates menüüs "Analyzer" tera ülaosas. Allpool näete, kas me valinud Prantsuse analüsaator välja jaoks mõeldud Prantsuse teksti registri.

3. Klõpsake nuppu **OK** "Väljad" enne oma välja määratlused kinnitamiseks
4. "Lisa indeks" enne salvestada ja lihtsalt määratletud registri luua, klõpsake nuppu **OK** .

Kuvatõmmised, mis on allpool näete, kuidas me nimega ja väljade jaoks meie "hotellist" index määratletud.

![](./media/search-create-index-portal/field-definitions.png)

![](./media/search-create-index-portal/set-analyzer.png)

## <a name="next"></a>Järgmise
Pärast Azure otsingu registri loomise, saab valmis [registrisse sisu üles laadida](search-what-is-data-import.md) nii, et saate alustada andmete otsimine.
