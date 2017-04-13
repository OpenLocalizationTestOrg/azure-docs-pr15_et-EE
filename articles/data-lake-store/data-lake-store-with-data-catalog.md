<properties
   pageTitle="Registriandmete andmete Lake poest Azure'i andmekataloogi | Microsoft Azure'i"
   description="Andmete Lake poest Azure'i andmekataloogi registriandmete"
   services="data-lake-store,data-catalog" 
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/28/2016"
   ms.author="nitinme"/>

# <a name="register-data-from-data-lake-store-in-azure-data-catalog"></a>Andmete Lake poest Azure'i andmekataloogi registriandmete

Selles artiklis saate teada, kuidas Azure'i andmesalve Lake integreerimine Azure andmekataloogi hõlpsamaks leidmiseks andmete ettevõttes integreerimise andmekataloogi abil. Kataloogimise andmete kohta leiate lisateavet teemast [Azure andmekataloogi](../data-catalog/data-catalog-what-is-data-catalog.md). Stsenaariumid, kus saate kasutada andmete kataloogi, vt [Azure'i andmekataloogi levinud stsenaariumi](../data-catalog/data-catalog-common-scenarios.md).

## <a name="prerequisites"></a>Eeltingimused

Enne alustamist selles õpetuses, peab teil olema järgmised:

- **An Azure'i tellimus**. Leiate [Azure'i saada tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/).

- **Luba Azure tellimuse** andmete Lake poe avaliku eelvaate. Vaadake [juhiseid](data-lake-store-get-started-portal.md#signup).

- **Azure'i andmesalve Lake konto**. Järgige veebisaidil [Alustamine Azure'i Lake andmesalve Azure'i portaalis](data-lake-store-get-started-portal.md). Selles õpetuses mõeldud andke meile luua nimega **datacatalogstore**Lake andmesalve konto. 

    Kui olete loonud kontot, valimi andmehulgas üleslaadimine. Selles õpetuses andke meile laadida CSV failide [Azure'i andmed Lake Git hoidla](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/)kaustas **AmbulanceData** . Erinevate kliendid, nagu [Azure'i salvestusruumi Exploreri](http://storageexplorer.com/)abil saate andmete üleslaadimine bloobimälu ümbrises.

- **Azure'i andmete kataloogi**. Ettevõtte peab Azure'i andmekataloogi luua oma ettevõtte jaoks. Iga ettevõtte jaoks on lubatud ainult üks kataloogi.

## <a name="register-data-lake-store-as-a-source-for-data-catalog"></a>Register Lake andmesalve andmekataloogi allikas

>[AZURE.VIDEO adcwithadl] 

1. Minge `https://azure.microsoft.com/services/data-catalog`, ja klõpsake nuppu **Alustamine**.

2. Azure'i andmekataloogi portaali sisse logida, ja klõpsake nuppu **Avalda andmed**.

    ![Andmeallika registreerimine] (./media/data-lake-store-with-data-catalog/register-data-source.png "Andmeallika registreerimine")

3. Järgmisel lehel nuppu **Käivita rakendus**. Laadige alla rakendus Avaldamisfail teie arvutis. Topeltklõpsake Avaldamisfail rakenduse käivitamiseks.

4. Lehel Tere tulemast, klõpsake linki **Logi sisse**ja sisestage oma kasutajanimi ja parool.

    ![Tervituskuva] (./media/data-lake-store-with-data-catalog/welcome.screen.png "Tervituskuva")

5. Lehel Valige andmeallikas valige **Azure'i andmed Lake**ja seejärel klõpsake nuppu **edasi**.

    ![Andmeallika valimine] (./media/data-lake-store-with-data-catalog/select-source.png "Andmeallika valimine")

6. Sisestage järgmisel lehel Lake andmesalve konto nimi, mida soovite andmekataloogis registreerida. Jätke vaikimisi muud suvandid ja seejärel nuppu **Ühenda**.

    ![Andmeallikaga ühenduse loomine] (./media/data-lake-store-with-data-catalog/connect-to-source.png "Andmeallikaga ühenduse loomine")

7. Järgmisel lehel saab jagada järgmised lõigud.

    lisamine. Väljale **Serveri hierarhia** tähistab Lake andmesalve konto kausta ülesehitust. **$Root** tähistab Lake andmesalve konto juurkausta ja **AmbulanceData** Lake andmesalve konto juurkaustas loodud kausta.

    b. Väljal **Saadaolevad objektid** on loetletud failid ja kaustad **AmbulanceData** kausta.

    c. **Objektide registreeritud märkeruut** on loetletud failid ja kaustad, mida soovite Azure'i andmekataloogis registreerida.

    ![Vaate andmestruktuur] (./media/data-lake-store-with-data-catalog/view-data-structure.png "Vaate andmestruktuur")

8. Selles õpetuses peaks kataloogis registreeruda kõik failid. Selleks nuppu (![objektide nihutamine](./media/data-lake-store-with-data-catalog/move-objects.png "objektide nihutamine")) kõik failid **objektide registreerida** väljale liikumiseks. 

    Kuna andmed registreeritakse on kogu ettevõtte andmekataloogi, on recommened lähenemine lisada mõned metaandmed, mille abil saate hiljem andmete leidmiseks. Näiteks saate lisada e-posti aadressi andmete omanik (nt selline, kes üles laaditakse andmed) või lisada sildi andmete tuvastamiseks. Ekraanipildi all kuvatakse sildi lisame andmetega.

    ![Vaate andmestruktuur] (./media/data-lake-store-with-data-catalog/view-selected-data-structure.png "Vaate andmestruktuur")

    Klõpsake nuppu **registreeru**.

8. Järgmised ekraanipildi – näitab, et andmed on edukalt registreeritud andmekataloogis.

    ![Registreerimine lõpetatud] (./media/data-lake-store-with-data-catalog/registration-complete.png "Vaate andmestruktuur")

9. Klõpsake **Vaate portaali** andmekataloogi portaali naasmiseks ja veenduge, et pääsete nüüd registreeritud andmete portaalist. Andmete otsimiseks saate kasutada andmete registreerimisel kasutatud silt.

    ![Otsingu andmete kataloogi] (./media/data-lake-store-with-data-catalog/search-data-in-catalog.png "Otsingu andmete kataloogi")

10. Nüüd saate teha toiminguid nagu marginaalid ja dokumentide lisamine andmed. Lisateabe saamiseks vaadake järgmisi linke.
    * [Marginaalide andmekataloogi andmeallikad](../data-catalog/data-catalog-how-to-annotate.md)
    * [Dokumendi andmekataloogi andmeallikad](../data-catalog/data-catalog-how-to-documentation.md)

## <a name="see-also"></a>Vt ka

* [Marginaalide andmekataloogi andmeallikad](../data-catalog/data-catalog-how-to-annotate.md)
* [Dokumendi andmekataloogi andmeallikad](../data-catalog/data-catalog-how-to-documentation.md)
* [Sujuv Lake andmesalve Azure muude teenustega](data-lake-store-integrate-with-other-services.md)
