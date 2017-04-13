<properties
    pageTitle="Azure'i salvestusruumi dispersioonanalüüs | Microsoft Azure'i"
    description="Microsoft Azure salvestusruumi konto andmed on kopeeritud kestvus ja kõrge kättesaadavus. Tiražeerimissuvandid sisaldavad kohalikult liigsete salvestusruumi (LRS), zone liigsete mälu (ZRS), geograafilise liigne salvestusruumi (GRS) ja lugemisõigus – geograafilise liigne salvestusruumi (RA GRS)."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="tamram"/>

# <a name="azure-storage-replication"></a>Azure'i salvestusruumi dispersioonanalüüs

Microsoft Azure salvestusruumi konto andmed on alati kopeeritud kestvus ja kättesaadavuse tagamiseks. Dispersioonanalüüs kopeerib andmete sama andmekeskuse või teise andmekeskuse, sõltuvalt sellest, millist dispersioonanalüüs suvandi valite. Dispersioonanalüüs kaitseb teie andmeid ja säilitab rakendus üles-aja siirdamiseks riistvara ebaõnnestumise korral. Kui teie andmed on kopeeritud teise andmekeskuses, mis ka kaitseb teie andmeid katastroofiline tõrge esmane asukohta.

Dispersioonanalüüs tagab, et konto salvestusruumi vastab [-Teenindustaseme leping (SLA) Storage](https://azure.microsoft.com/support/legal/sla/storage/) isegi ees ebaõnnestumist. Vaadake lisateavet Azure Storage SLA tagab kestvus ja kättesaadavuseks. 

Salvestusruumi konto loomisel valige üks järgmistest Tiražeerimissuvandid:  

- [Kohalik liigsete salvestusruumi (LRS)](#locally-redundant-storage)
- [Tsooni liigsete mälu (ZRS)](#zone-redundant-storage)
- [Geograafilise liigne salvestusruumi (GRS)](#geo-redundant-storage)
- [Lugemisõigus – geograafilise liigne salvestusruumi (RA GRS)](#read-access-geo-redundant-storage)

Lugemisõigus – geograafilise liigne salvestusruumi (RA GRS) on vaikevalik salvestusruumi uue konto loomisel.

Järgmises tabelis antakse kiirülevaade erinevused LRS, ZRS, GRS ja RA GRS, kuigi järgmistes lõikudes igat tüüpi dispersioonanalüüs üksikasjalikumalt.

| Strateegia dispersioonanalüüs                                                               | LRS | ZRS | GRS | RA-GRS |
|:----------------------------------------------------------------------------------|:---|:---|:---|:------|
| Andmed on kopeeritud üle mitme andmekeskuste.                                     | Ei  | Jah | Jah | Jah    |
| Andmeid saab lugeda teisene asukoht ning esmane asukoht. | Ei  | Ei  | Ei  | Jah    |
| Andmeid hoitakse eraldi sõlmed eksemplaride arv.                             | 3   | 3   | 6   | 6      |

Leiate [Azure'i salvestusruumi hinnad](https://azure.microsoft.com/pricing/details/storage/) hinnateavet erinevate koondamise suvandid.

>[AZURE.NOTE] Premium mälu toetab ainult kohalikult liigsete salvestusruumi (LRS). Premium salvestusruumi kohta leiate teavet teemast [Premium mälu: suure jõudlusega salvestusruumi Azure virtuaalse masina töökoormus](storage-premium-storage.md).

## <a name="locally-redundant-storage"></a>Kohalik liigsete salvestusruum

Kohalik liigsete salvestusruumi (LRS) tiražeerib oma andmeid kolm korda ladu skaala, mis on majutatud andmekeskuses, kus teie konto salvestusruumi piirkonnas. Taotluse täitmine tagastab edukalt ainult siis, kui see on kirjutatud kõigi kolme koopiate. Need kolm koopiad elavad eraldi viga domeenid ja täiendamine domeenide ühe salvestusruumi skaala üksuses. 

Salvestusruumi skaala üksus on raamile salvestusruumi sõlmed kogum. Viga domain (FD) on sõlmed, mis tähistavad esinevate füüsiline üksus ja võib käsitleda sõlmed kuuluvad sama füüsilise seadme ja rühm. Mõne täiendamine domeeni (UD) on rühm sõlmed, mis on täiendatud koos käigus täiendatud (levitamise kohta.). Kolm koopiad on laiali UDs ja FDs üksuses ühe salvestusruumi skaala tagamaks, et andmed on saadaval ka siis, kui riistvara tõrke mõjutab ühe sektsioon või sõlmed uuendamisel ajal on levitamise kohta.. 

LRS on kõige väiksemad maksumuse suvand ja vähemalt kestvus võrreldes muid võimalusi. Andmekeskuse taseme katastroofi (fire üleujutused jne) korral võib kõigi kolme koopiate kaotsi läinud või parandamatu. Selle riski vähendamiseks Geo liigsete salvestusruumi (GRS) on soovitatav Enamik rakendusi.

Kohalik liigsete salvestusruumi endiselt võib olla soovitav teatud olukordades: 

- Pakub kõrgeim maksimaalse läbilaskevõime Azure Storage dispersioonanalüüs suvanditest.

- Kui rakenduse talletab andmeid, mida saab hõlpsalt taastatud, siis võib valida LRS.

- Mõned rakendused on piiratud imitatsiooniga andmete üksnes riik tõttu andmete haldamise nõuetele. Andmepunktipaaride piirkonnas võib olla teises riigis; piirkonna paari teavet leiate [Azure'i piirkondade](https://azure.microsoft.com/regions/) .

## <a name="zone-redundant-storage"></a>Tsooni liigsete salvestusruum

Tsooni liigsete mälu (ZRS) tiražeerib andmete asünkroonselt üle ühe- või piirkondadesse Lisaks talletamise kolme koopiad sarnane LRS, tagades seega suurem kui LRS kestvus andmekeskuste. Andmed salvestatakse ZRS on püsival isegi juhul, kui esmane andmekeskuse on saadaval või parandamatu.
Klientidele, kes plaanite kasutada ZRS peaks olema teadlik mis: 

- ZRS on saadaval ainult jaoks Blokeeri plekid üldine otstarve salvestusruumi kontod ja on toetatud ainult versioonides salvestusruumi teenuse 2014-02-14 ja uuemad versioonid. 

- Kuna asünkroonne dispersioonanalüüs hõlmab viivituse, kohaliku õnnetuste on võimalik, et muudatused, mida pole veel teisese soovite kopeeritud kaotsi, kui andmeid ei saa taastada alates esmane.

- Koopia ei pruugi olla saadaval, kuni Microsofti käivitab teisese Tõrkesiirde.

- ZRS kontod ei saa teisendada hiljem LRS või GRS. Samuti ei saa teisendada LRS või GRS konto ZRS kontole.

- ZRS kontod ei ole mõõdikute või logimise võimalus. 

## <a name="geo-redundant-storage"></a>Geograafilise liigne salvestusruum

Geograafilise liigne salvestusruumi (GRS) tiražeerib andmete teisese alale, mis on sadu miili eemale esmane piirkond. Kui konto salvestusruumi on lubatud GRS, siis teie andmed on püsival isegi korral täieliku piirkondliku katkestuste või mõne tõttu, esmane piirkond ei ole Taastatavad.

Salvestusruumi konto koos GRS lubatud esmalt sooritamist värskendust esmane alale, kus on kopeeritud kolm korda. Seejärel värskendus on kopeeritud asünkroonselt teisene piirkond, kus on ka kopeeritud kolm korda. 

GRS esmaseid ja teiseseid piirkondade haldamine eraldi viga domeenides koopiad ja täiendamine domeenide salvestusruumi skaala üksuse kirjeldatud LRS sees.

Kaalutlused

- Kuna asünkroonne dispersioonanalüüs hõlmab viivituse, piirkondliku katastroofi korral on võimalik, et muudatused, mida pole veel teisene alale kopeeritud kaotsi, kui andmeid ei saa taastada esmane piirkonnast.

- Koopia pole saadaval, kui Microsoft käivitab Tõrkesiirde teisene alale.

- Kui rakendus soovib kasutaja tuleks lubada RA-GRS teisene piirkonnast lugeda. 

Kui salvestusruumi konto loomiseks valige esmane piirkond konto. Teisene piirkond määratakse esmane asukoha ja ei saa muuta. Järgmine tabel näitab põhi- ja piirkonnasätete sidumiste.

| Esmane             | Teisese           |
|---------------------|---------------------|
| Põhja Kesk-USA    | Lõuna-, Kesk-USA    |
| Lõuna-, Kesk-USA    | Põhja Kesk-USA    |
| Ida-USA             | Lääne USA.             |
| Lääne USA.             | Ida-USA             |
| USA Ida 2           | Kesk-USA          |
| Kesk-USA          | USA Ida 2           |
| Põhja-Euroopa        | Lääne Euroopa         |
| Lääne Euroopa         | Põhja-Euroopa        |
| Kagu-Aasia     | Ida-Aasia           |
| Ida-Aasia           | Kagu-Aasia     |
| Ida-Hiina          | Põhja-Hiinas         |
| Põhja-Hiinas         | Ida-Hiina          |
| Jaapan Ida          | Jaapan Lääne          |
| Jaapan Lääne          | Jaapan Ida          |
| Brasiilia Lõuna        | Lõuna-, Kesk-USA    |
| Austraalia Ida      | Austraalia kodutee |
| Austraalia kodutee | Austraalia Ida      |
| India, Lõuna         | India Central       |
| India Central       | India, Lõuna         |
| USA valitsuse Iowa         | USA valitsuse Virginia     |
| USA valitsuse Virginia     | USA valitsuse Iowa         |
| Kanada Central      | Kanada Ida          |
| Kanada Ida         | Kanada Central      |
| Suurbritannia Lääne             | Suurbritannia Lõuna            |
| Suurbritannia Lõuna            | Suurbritannia Lääne             |
| Saksamaa Central     | Saksamaa raudtee   |
| Saksamaa raudtee   | Saksamaa Central     |
| Lääne USA 2           | Kesk Lääne USA     |
| Kesk Lääne USA     | Lääne USA 2           |

Ajakohasemat teavet ei toeta Azure regioonid, leiate [Azure'i piirkondade](https://azure.microsoft.com/regions/).
 
## <a name="read-access-geo-redundant-storage"></a>Lugemisõigus – geograafilise liigne salvestusruum

Lugemisõigus – geograafilise liigne salvestusruumi (RA GRS) maksimeerib kättesaadavus salvestusruumi konto teisene asukoht, lisaks dispersioonanalüüs üle kahe piirkonna GRS esitatud andmed kirjutuskaitstud juurdepääsu võimaldamine. 

Kui kirjutuskaitstud juurdepääsu lubamine oma andmetega teisene piirkonna, teie andmed on sekundaarne näitaja, lisaks esmane lõpp-punkti salvestusruumi konto jaoks saadaval. Teisene lõpp-punkti on sarnane esmane lõpp-punkti, kuid lisab järelliite `–secondary` konto nimi. Näiteks kui teie peamine lõpp-punkti bloobimälu teenus on `myaccount.blob.core.windows.net`, siis on teie teisene lõpp-punkti `myaccount-secondary.blob.core.windows.net`. Kiirklahvide salvestusruumi konto jaoks on sama nii põhi- ja lõpp-punktid.

Kaalutlused

- Teie taotlus on haldamiseks, mille lõpp-punkti see on suheldes RA-GRS kasutamisel. 

- RA-GRS on mõeldud kõrge-saadavus eesmärgil. Skaleeritavus juhiseid, palun vaadake üle [jõudluse kontroll-loend](storage-performance-checklist.md).

## <a name="next-steps"></a>Järgmised sammud

- [Azure'i salvestusruumi hinnad](https://azure.microsoft.com/pricing/details/storage/)
- [Azure'i salvestusruumi kontode kohta](storage-create-storage-account.md)
- [Azure'i salvestusruumi skaleeritavus ja jõudluse sihtkohad](storage-scalability-targets.md)
- [Microsoft Azure'i talletussuvandite koondamise ja lugemisõigus Geo liigsete salvestusruum](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx)  
- [SOSP paberi - Azure Storage: Tugevalt saadaval salvestusruumi pilveteenus tugev järjepidevus](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)  
