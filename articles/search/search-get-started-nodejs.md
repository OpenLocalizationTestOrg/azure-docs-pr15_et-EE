<properties
    pageTitle="Azure'i otsingu NodeJS alustamine | Microsoft Azure'i | Majutatud pilveteenuses otsing"
    description="Tutvustavad koostamise otsingurakenduse, klõpsake otsingu majutatud pilveteenus Azure kasutamine NodeJS oma programmeerimiskeel."
    services="search"
    documentationCenter=""
    authors="EvanBoyle"
    manager="pablocas"
    editor="v-lincan"/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.date="07/14/2016"
    ms.author="evboyle"/>

# <a name="get-started-with-azure-search-in-nodejs"></a>Azure'i otsingu NodeJS kasutamise alustamine
> [AZURE.SELECTOR]
- [Portaal](search-get-started-portal.md)
- [.NET-I](search-howto-dotnet-sdk.md)

Siit saate teada, kuidas luua kohandatud NodeJS otsingurakenduse, mis kasutab Azure otsimine oma otsingut. Selle õpetuse kasutab [Azure otsingu teenuse REST API](https://msdn.microsoft.com/library/dn798935.aspx) ehitada objektide ja selle kasutatud toimingud.

Kasutasime [NodeJS](https://nodejs.org) ja NPM, [ülev tekst 3](http://www.sublimetext.com/3)ja Windows PowerShelli opsüsteemis Windows 8.1 ja katsetamiseks järgmine kood.

Selle valimi käivitamiseks peavad teil olema Azure'i otsinguteenuse, kus te saate registreeruda [Azure portaali](https://portal.azure.com). Üksikasjalikud juhised leiate teemast [loomine Azure otsinguteenuse portaali](search-create-service-portal.md) .

## <a name="about-the-data"></a>Andmete kohta

See näide rakendus kasutab [USA geoloogilise Services (USGS)](http://geonames.usgs.gov/domestic/download_data.htm), filtreeritud Rhode Island oleku kohta andmehulga mahu vähendamiseks. Kasutame neid andmeid otsingurakenduse, mis tagastab natsionaalsotsialistlikule nagu haiglad ja koolid, samuti geoloogilise funktsioone, nagu voogu järvi ja tippkohtumise koostamiseks.

Selle rakenduse **DataIndexer** programmi koostab ja laadib registri abil [indekseerija](https://msdn.microsoft.com/library/azure/dn798918.aspx) loomiseks, mis toomine filtreeritud USGS andmekomplekti avaliku Azure SQL-i andmebaasist. Mandaadid ja ühenduse võrgus andmeallika teavet programmi kood. Pole veel konfigureerimine on vaja.

> [AZURE.NOTE] Meil on rakendatud filter tuvastatavate alla tasuta hinnakirjad taseme piirmäära 10 000 dokumenti. Kui kasutate standard taseme, ei kehti selle piirmäära. Üksikasjalikku teavet iga hinnakirjad taseme võimsus kohta leiate teemast [teenuse otsingulimiitide](search-limits-quotas-capacity.md).


<a id="sub-2"></a>
## <a name="find-the-service-name-and-api-key-of-your-azure-search-service"></a>Teenuse nimi ja api-võti oma Azure'i otsinguteenuse otsimine

Saada URL portaali naasmiseks pärast teenuse loomist või `api-key`. Ühenduste oma otsinguteenuse nõua, et teil on nii URL-i ja mõne `api-key` autentida kõne.

1. [Azure'i portaali](https://portal.azure.com)sisse logima.
2. Klõpsake hüpata **otsinguteenuse** loetleda Azure'i otsingu teenuste tellimuse eest ette valmistatud.
3. Valige teenus, mida soovite kasutada.
4. Teenuse armatuurlaual kuvatakse paanid oluline teave, samuti juurdepääsuks haldus klahve võtme ikooni.

    ![][3]

5. Kopeerige URL-i teenus, mis administraator ja päringu võti. Peate kõigi kolme hiljem, kui lisate need config.js faili.

## <a name="download-the-sample-files"></a>Näidisfailide allalaadimine

Kas üks järgmistest võimalustest abil saate alla laadida valimi.

1. Avage [AzureSearchNodeJSIndexerDemo](https://github.com/AzureSearch/AzureSearchNodeJSIndexerDemo).
2. Klõpsake nuppu **Laadi alla ZIP**, ZIP-faili salvestamine ja seejärel ekstrakti kõik failid, mida see sisaldab.

Kõigi järgnevate faili muudatused ja Käivita laused esitatakse selles kaustas olevaid faile.


## <a name="update-the-configjs-with-your-search-service-url-and-api-key"></a>Värskendage soovitud config.js. Otsinguteenuse URL ja api võti

URL-i ja api võti, mis varem kopeeritud abil määrata haldus- ja päringu-võti URL-i konfiguratsioonifail.

Haldus klahve anda toimingud, sh loomine või kustutamine registri ja dokumentide laadimine üle täielik kontroll. Seevastu päringu võtmed on kirjutuskaitstud toimingute, tavaliselt kasutatakse ühenduse Azure'i otsingu klientrakendustes.

Selles näites me päringu klahvi tugevdamine parim klientrakendustes päringu võti kasutamise hõlbustamiseks sisaldavad.

Järgmine pilt kuvatakse **config.js** avatud teksti redaktoris koos asjakohaste kirjeid nii, et saate vaadata, kuhu fail värskendada väärtustega, mis sobivad teie otsinguteenuse piiritleda.

![][5]


## <a name="host-a-runtime-environment-for-the-sample"></a>Host käitusaja keskkonna valimi

Valimi nõuab HTTP-serverisse, mille saate installida npm globaalselt abil.

PowerShelli aken kasutada järgmisi käske.

1. Liikuge kausta, mis sisaldab **package.json** faili.
2. Tippige `npm install`.
2. Tippige `npm install -g http-server`.

## <a name="build-the-index-and-run-the-application"></a>Koostada register ja käivitage rakendus

1. Tippige `npm run indexDocuments`.
2. Tippige `npm run build`.
3. Tippige `npm run start_server`.
4. Otse oma brauseri veebisaidile`http://localhost:8080/index.html`

## <a name="search-on-usgs-data"></a>USGS andmete otsimine

USGS andmehulga sisaldab kirjeid, mis on olulised Rhode Island olekus. Kui klõpsate mõnda tühja otsinguväljale **Otsi** , saate ülemise 50 kirjed, mis on vaikimisi.

Sisestada otsingusõna annab otsingumootori midagi liikuda. Proovige piirkondliku nime sisestamine. "Roger Williams" oli esimene kuberner keeleline. Mitme parkides, hoonete ja koolides on tema nime.

![][9]

Võib proovida ka kõik need tingimused

- Pawtucket
- Pembroke
- hane + Neeme


## <a name="next-steps"></a>Järgmised sammud

See on esimene Azure'i otsingu õpetuse NodeJS ja USGS andmekomplekti. Aja jooksul me saate laiendada selle õpetuse näitamaks, et võiksite kasutada oma kohandatud lahendused täiendavad otsinguvõimalused.

Kui teil on veel mõni taust Azure'i otsing, saate selle valimi hüppelauaks proovida suggesters (päringute ettetippimise või automaatteksti), filtrite ja lihvitud navigeerimine. Otsingutulemite lehe saab täiustada ka lisades loendab ja partiide dokumentide nii, et kasutajad saavad Lehitsemiseks tulemused.

Uus Azure otsing? Soovitame proovimise muud õpetused lisaandmete saamiseks, mida saate luua. Külastage meie [dokumentatsiooni lehe](https://azure.microsoft.com/documentation/services/search/) leidmiseks veel ressursse. Linke saate vaadata ka [Video ja õpetus loendi](search-video-demo-tutorial-list.md) veel teabele juurde.

<!--Image references-->
[1]: ./media/search-get-started-nodejs/create-search-portal-1.PNG
[2]: ./media/search-get-started-nodejs/create-search-portal-2.PNG
[3]: ./media/search-get-started-nodejs/create-search-portal-3.PNG
[5]: ./media/search-get-started-nodejs/AzSearch-NodeJS-configjs.png
[9]: ./media/search-get-started-nodejs/rogerwilliamsschool.png
