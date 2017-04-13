<properties
    pageTitle="Azure'i otsinguteenuse Azure portaali loomine | Microsoft Azure'i | Majutatud pilveteenuses otsing"
    description="Saate teada, kuidas ette valmistada Azure'i otsinguteenuse Azure'i portaalis."
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

# <a name="create-an-azure-search-service-using-the-azure-portal"></a>Azure'i otsinguteenuse Azure portaali loomine

Sellest juhendist juhendab teid loomise protsess (või ettevalmistamise) Azure'i otsinguteenuse [Azure portaali](https://portal.azure.com/).

Sellest juhendist eeldab juba on Azure'i tellimus ja saate Azure portaali sisse logida.

## <a name="find-azure-search-in-the-azure-portal"></a>Leiate Azure'i otsingu Azure portaali
1. [Azure portaali](https://portal.azure.com/) ja logige sisse.
1. Klõpsake plussmärki ("+") ülemises vasakpoolses nurgas.
2. Valige **andmete + salvestusruumi**.
3. Valige **Azure otsida**.

![](./media/search-create-service-portal/find-search.png)

## <a name="pick-a-service-name-and-url-endpoint-for-your-service"></a>Valige teenuse nimi ja URL-i lõpp-punkti teie teenuse jaoks
1. Oma teenuse nimi on osa Azure'i otsingu teenuse lõpp-punkti URL-i suhtes, mis teete API kõnede haldamiseks ja otsingu teenust kasutada.
2. Tippige oma teenuse nimi väljale **URL** . Teenuse nimi:
  * peab sisaldama ainult väiketähed, numbrit või kriipsud ("-")
  * ei saa kasutada sidekriipsu ("-") esimese 2 märki või viimase üksikmärki
  * ei tohi sisaldada järjestikust kriipsud ("-")
  * on piiratud vahel 2 kuni 60 märki


## <a name="select-a-subscription-where-you-will-keep-your-service"></a>Valige tellimus, kui hoiate oma teenuse
Kui teil on mitu tellimust, saate valida, millise neist sisaldab selle Azure'i otsinguteenuse.

## <a name="select-a-resource-group-for-your-service"></a>Valige ressursirühma teie teenuse jaoks
Luua uue ressursirühma või valida mõne olemasoleva. Ressursirühma on Azure teenuste ja ressursse, mida kasutatakse koos kogum. Näiteks kui kasutate Azure otsingu registrisse SQL-andmebaasiga, seejärel mõlemad teenused peaks olema sama ressursirühm osa.

## <a name="select-the-location-where-your-service-will-be-hosted"></a>Valige asukoht, kus teie teenuse olema majutatud
Azure'i teenust, Azure'i otsing on saadaval kogu maailmas andmekeskuste majutada. Pange tähele, et [hinnad võivad olla erinevad](https://azure.microsoft.com/pricing/details/search/) geograafia.

## <a name="select-your-pricing-tier"></a>Valige oma hinnakirjad taseme
[Azure'i otsing on praegu saadaval hinnakirjad mitmetasandiline](https://azure.microsoft.com/pricing/details/search/): tasuta, lihtne või standardne. Iga taseme on oma [ametinimetus ja piirangud](search-limits-quotas-capacity.md). Juhised leiate [valimine hinnakirjad taseme või SKU-ga](search-sku-tier.md) .

Sel juhul valisime meie teenuste Standard taseme.

## <a name="select-the-create-button-to-provision-your-service"></a>Valige nupp "Loo" ettevalmistamise teenust

![](./media/search-create-service-portal/create-service.png)

## <a name="scale-your-service"></a>Oma teenuse skaala

Kui teie teenus on ette valmistatud, saate seda vastavalt oma vajadustele skaala. Kui olete valinud standardse taseme oma Azure'i otsinguteenuse jaoks, saate skaala teenust mõõt: koopiad ja sektsioonid. Kui olete valinud tavaline astme, saate lisada ainult koopiad.

*__Sektsioonid__* luba teenust talletada ja otsida rohkem dokumente.

*__Koopiad__* luba teenust käsitlema suurem koormus otsingupäringute - [teenus nõuab kirjutuskaitstud SLA saavutamiseks 2 koopiad ja nõuab 3 koopiad lugemis-ja kirjutamisõigusega SLA saavutamiseks](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

1. Avage Azure'i otsingu teenuse haldus blade Azure'i portaalis.
2. Valige **sätted** labale **skaala**.
3. Muudate teenust, lisades koopiad või partitsioonid.
  * Te ei saa skaala teenust viimase 36 otsingu üksused. Teie arv otsing üksused on teie koopiad ja sektsioonid (koopiad * sektsioonid = üksuste arv otsing).
  * Kui olete valinud tavaline astme, mida saab ainult skaala 3 koopiad. Põhilised teenused on seotud ühe sektsiooni.

![](./media/search-create-service-portal/scale-service.png)

## <a name="next"></a>Järgmise
Pärast ettevalmistamise teenuse Azure otsing, saab valmis [registri Azure'i otsingu määratlemine](search-what-is-an-index.md) nii, et saate üles laadida ja oma andmeid otsida.

Leiate [Azure'i otsingu portaalis alustamine](search-get-started-portal.md) kiirülevaate õpetuse.
