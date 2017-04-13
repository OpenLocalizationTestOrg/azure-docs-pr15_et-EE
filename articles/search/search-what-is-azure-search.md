<properties
    pageTitle="Mis on Azure otsing | Microsoft Azure'i | Majutatud pilveteenuses otsing"
    description="Azure'i otsing on täielikult hallatav majutatud cloud otsinguteenuse. Lisateavet selle funktsiooni ülevaade."
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

# <a name="what-is-azure-search"></a>Mis on Azure otsing?

Azure'i otsing on cloud otsing-kui-a-service lahenduse delegaatide serveri ja taristu haldamise Microsoft, jättes teile valmis kasutada teenust, mida saate oma andmetega asustada ja seejärel kasutage otsingu lisamiseks oma veebis või mobiilirakenduses. Azure otsing saate hõlpsasti lisada robustne otsingut lihtsa [REST API -ga](https://msdn.microsoft.com/library/azure/dn798935.aspx) või [.NET SDK](search-howto-dotnet-sdk.md) ilma otsingu taristu haldamise või muutudes ekspert otsingu kasutamine rakenduste.

## <a name="give-your-users-a-powerful-search-experience"></a>Andke kasutajatele võimsaid otsingut

**Võimas päringud** saate välja töötada [lihtsa päringusüntaksit](https://msdn.microsoft.com/library/azure/dn798920.aspx), mis pakub loogikatehtemärkide, fraasi otsimine tehtemärgid, järelliite tehtemärgid, tehtemärkide järgnevus. Lisaks saate [Lucene päringusüntaksit](https://msdn.microsoft.com/library/azure/mt589323.aspx) lubada udune otsing, lähedus search, Termini suurendada ja regulaaravaldised. Azure'i otsingu toetab ka kohandatud leksikaalse analüsaatorid lubamiseks rakenduse abil foneetiline sobitamine keerukaid otsingupäringuid ja regulaaravaldised.

**Keele tugi** on [sisalduv 56 keelte jaoks](https://msdn.microsoft.com/library/azure/dn879793.aspx). Kasutades nii Lucene analüsaatorid ja Microsoft analüsaatorid (rafineeritud loomulikus keeles Office'i ja Bingi töötlemine-aastane), saate Azure'i otsingu analüüsida Keelekohased lingvistika ajavormid, sugu, korrapäratu mitmuse nimisõnad (nt "mouse" vs "hiir"), Wordi tühistage liitmine, poolitus (keelte ilma tühikuteta) jaoks jm arukalt käsitlema rakenduse otsinguväljale tekst.

**Otsingusoovitused** saab lubada autocompleted otsingu ribad ja ettetippimise päringud. Kui kasutajad [oma registri tegelik dokumendid on soovitatav](https://msdn.microsoft.com/library/azure/dn798936.aspx) sisestage osalist otsingut sisestusteade.

**Tulemus esiletõstmine** [võimaldab](https://msdn.microsoft.com/library/azure/dn798927.aspx) kasutajatel vaadata koodilõigu iga tulemus, mis sisaldab vasteid oma päringu teksti. Te saate valida millised väljad tagastada esiletõstetud Koodilõigud.

**Lihvitud navigeerimine** lihtsalt lisatakse teie otsingutulemuste lehel Azure'i otsingu abil. [Ainult ühe Päringuparameetri](https://msdn.microsoft.com/library/azure/dn798927.aspx)kasutamisel Azure'i otsing tagastab kogu vajaliku teabe ehitada lihvitud otsingut oma rakenduse UI süvitsimineku ja filtreerida Otsingu tulemused (nt filtri kataloogiüksuste hinnaga vahemikus või kaubamärgi) kasutajad.

**Geo ruumilise** tugi võimaldab arukalt töötlemine, filtreerimine ja kuvamine geograafilised asukohad. Azure'i otsingu abil andmete põhjal otsingutulemus lähedus määratud asukohta või kindla geograafilise asukoha uurimine kasutajatele. Selles videos selgitatakse, kuidas see toimib: [kanali 9: Azure'i otsingu ja ruumilisel andmete](https://channel9.msdn.com/Shows/Data-Exposed/Azure-Search-and-Geospatial-Data).

**Filtrite** saab hõlpsasti lisada oma rakenduste Kasutajaliidese lihvitud navigeerimine, täiustamiseks päringu kujundamisel ja filtri vastavalt klõpsake kasutaja või arendaja-määratud kriteeriumidele. Luua võimsaid filtrite abil [OData süntaks](https://msdn.microsoft.com/library/azure/dn798921.aspx).

## <a name="empower-your-developers-with-an-easy-to-use-service"></a>Anda oma arendajatele teenuse lihtne kasutamine

**Kõrge-saadavus** tagab äärmiselt usaldusväärne otsingu teenuse kogemus. Kui mastaabitud õigesti, [Azure'i otsing pakub 99,9% SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

**Täielikult hallatavad** lahendusena lõpuni, Azure'i otsingu jaoks on vaja täiesti pole taristu haldamise. Teie vajadustele saab teenust hõlpsasti kohandada kahes dimensioonis käsitlema salvestusruumi dokumendi, kõrgema päringu laadimise või mõlemad skaala.

**Andmete integreerimise** abil [indexers](https://msdn.microsoft.com/library/azure/dn946891.aspx) võimaldab Azure'i otsing automaatselt analüüsima Azure'i SQL-andmebaasi, Azure'i DocumentDB või [Azure'i bloobimälu](search-howto-indexing-azure-blob-storage.md) oma peamine andmesalve oma otsinguregistri sisu sünkroonimiseks.

**Dokumendi lõhenemist** on saadaval (praegu eelvaade) [lugeda ja indekseerida suuri failivormingud](search-howto-indexing-azure-blob-storage.md) sealhulgas Microsoft Office'i dokumente PDF- ja HTML-vormingus.

**Otsingu liikluse analytics** on [lihtne koguda ja analüüsida](search-traffic-analytics.md) avada ülevaateid, mis kasutajad on Tippige väljale Otsing.

**Lihtne hinded** on põhieelis Azure'i otsing. [Hinded profiilid](https://msdn.microsoft.com/library/azure/dn798928.aspx) kasutatakse lubada asutustel mudeli asjakohasuse funktsioonina väärtuste dokumendid ise. Näiteks võiksite uuem toodete või diskonteeritud toodete kõrgema otsingutulemites kuvada. Samuti saate koostada isikupärastatud hinded vastavalt kliendi Otsingu eelistused olete jälitatud ja salvestada eraldi siltide kasutamise hinded profiilid.

**Sortimine** mitme välja indeksi skeemi kaudu pakutakse ja seejärel lülitada päringu-ajal ühe otsing parameeter.

**Piipar** ja ahendamise otsingutulemite on [häälestatud juhtelemendi abil](search-pagination-page-layout.md) Azure'i otsing pakub otsingutulemite üle.  

**Otsingu explorer** võimaldab teil probleemi päringute kõigi oma registrite õige teie konto Azure portaalist nii, et saate testida päringud ja täpsustamise hinded profiilid.

## <a name="how-it-works"></a>Kuidas see toimib

### <a name="1-provision-service"></a>1. teenuse säte
Te saate tööasendisse Azure'i otsinguteenuse [Azure portaali](https://portal.azure.com/) või [Azure ressursside haldamine API](https://msdn.microsoft.com/library/azure/dn832684.aspx).

Sõltuvalt sellest, kuidas otsinguteenuse konfigureerimine, peate kasutama kas tasuta taseme muude Azure'i otsingu tellijad ühiskasutusse antud teenuse või mõne [makstud taseme](https://azure.microsoft.com/pricing/details/search/) mis pühendab ressursse, mis kasutavad ainult teenust. Kui teie teenuse ettevalmistamise, valida ka teenust majutava andmekeskuse piirkond.

Sõltuvalt sellest, millised taseme teenuse valite, saate mastaapimiseks teenust mõõt: 1) lisada koopiad kasvada oma võimet toime raske päringu laadimise ja 2) sektsioonide lisada rohkem dokumente lisada. Dokumendi salvestus- ja päringu läbilaskevõime eraldi teostavaid, saate kohandada oma otsinguteenuse teie vajadustele.

### <a name="2-create-index"></a>2 registri loomine
Enne sisu saate oma Azure'i otsinguteenuse üles laadida, peate esmalt määrama registri Azure'i otsing. Registri on nagu andmebaasi tabeli, mis hoiab teie andmeid ja saate aktsepteerida otsingupäringuid. Määratlege index skeemi vastendamiseks dokumendid, kui soovite otsida, sarnast andmebaasi väljadele struktuur.

Nende indeksite skeemiga kas loomist Azure'i portaalis või programmiliselt [kasutades .NET SDK](search-howto-dotnet-sdk.md) või [REST API -ga](https://msdn.microsoft.com/library/azure/dn798941.aspx). Kui indeks on määratletud, saate andmete Azure'i otsinguteenuse, kust selle hiljem indekseeritud üles laadida siis.

### <a name="3-index-data"></a>3. viiteandmete
Kui olete määratlenud väljade ja atribuutide indeks, olete valmis oma sisu üleslaadimiseks registrisse. Saate üles laadida andmete registri push- ja pull mudeli.

Mudeli pull on esitatud indexers, mida saab konfigureerida nõudmisel või ajastatud värskendused (vt [indekseerija toimingud (Azure'i otsingu teenuse REST API)](https://msdn.microsoft.com/library/azure/dn946891.aspx)), mis võimaldab teil hõlpsasti neelata andmed ja andmete muutuste Azure'i DocumentDB, Azure'i SQL-andmebaasi, Azure'i bloobimälu või majutatud on Azure VM SQL serveri kaudu.

Tõuketeatised mudel on esitatud SDK või REST API-d kasutatakse värskendatud dokumente saata registri kaudu. Vajutada andmeid praktiliselt kõigist andmekomplekti JSON-vormingus kaudu. Vt [lisamine, värskendamine, dokumente või kustutada](https://msdn.microsoft.com/library/azure/dn798930.aspx) või [kasutamine .NET SDK)](search-howto-dotnet-sdk.md) juhised andmete laadimine.

### <a name="4-search"></a>4. otsingu
Kui teil on täidetud Azure otsinguregistri, saate nüüd [probleemi otsingupäringute](https://msdn.microsoft.com/library/azure/dn798927.aspx) oma teenuse lõpp-punkti REST API-ga või .NET SDK lihtsa HTTP päringuid kasutamine.

## <a name="try-it-now-for-free"></a>Proovige järele (tasuta!)
Võite proovida Azure'i otsingu juba täna! Kui teil on juba Azure'i konto, saate [sätte teenuse tasuta astme](search-create-service-portal.md).

Kui teil pole Azure'i konto, võite proovida ei tasuta, 60-minutilised seansi registreeruda nõutav. Minge [Proovige Azure'i rakendust Service](http://go.microsoft.com/fwlink/p/?LinkId=618214) ja valige "Web Appi." Valige mall "ASP.net-i + Azure'i otsing" alustada.
