<properties
   pageTitle="Testige oma andmete teenuse pakkumine turuplatsil | Microsoft Azure'i"
   description="Mõista, kuidas testida oma Azure'i turuplatsi andmed teenuse pakkumine."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/26/2016"
   ms.author="hascipio; avikova" />

# <a name="testing-your-data-service-offer-in-staging"></a>Oma andmete teenuse pakkumine lavastus katsetamine

>[AZURE.IMPORTANT] **Sel ajal me ei ole enam kasutuselevõtt mis tahes uute andmete teenuse tootjad. Uue dataservices ei saada heaks kirjet.** Kui teil on SaaS ärirakendusi soovite avaldada AppSource leiate lisateavet [siin](https://appsource.microsoft.com/partners). Kui teil on mõni IaaS rakendusi või arendaja teenuse soovite avaldatava Azure'i turuplatsi võite leida rohkem teavet [siin](https://azure.microsoft.com/marketplace/programs/certified/).

Pärast kaks esimest juhiste [arendaja Microsofti konto loomise](marketplace-publishing-accounts-creation-registration.md) ja [luua oma andmete teenuse pakkumine Avaldamisportaal](marketplace-publishing-data-service-creation.md) olete valmis oma pakkumine Azure'i turuplatsil kättesaadavaks tegemiseks. Selles teemas juhendab teid esmalt vahelink, samm nimega "Lavastus"

Lavastus tähendab, et teie pakkumise omaette "Liivakasti", kus saate testida ja kontrollida selle funktsiooni enne kinni tootmisse juurutamine. Pakkumine kuvatakse lavastus, nagu oleks kliendile, kes on kasutusele võetud.

## <a name="step-1-pushing-your-offer-to-staging"></a>Samm 1. Teie pakkumise lavastus lükkamine
Teie pakkumise lavastus lükkamine võimaldab teil testida pakkumine, kuni see muutub tulevaste tellijate.  Näete, kuidas teie pakkumise kuvatakse ning nende andmete tellimise funktsioon.  

  ![Joonis](media/marketplace-publishing-data-service-test-in-staging/step-1.1.png)

1.  [Avaldamise portaali](https://publish.windowsazure.com) sisse logida
2.  Valige vasakpoolsel navigeerimisribal aknas **Andmeteenused**
3.  Valige oma pakkumine soovite lükata lavastus. Kuvatakse ülaltoodud ekraani.
4.  Klõpsake nuppu **Push lavastus** .  
5.  Kui on vaja peab olema täidetud enne lükkamine, et lavastus pakkumise probleeme, kuvatakse loendi kuvada.  Klõpsake loendis iga üksuse õige neid üksusi. Kui kõik parandused teinud, klõpsake uuesti nuppu **Push lavastus** .

Kui teie pakkumine pole probleeme kuvatakse allpool hüpikakna.  

Pole kavandamise/mitte kinnitamise, et kuvada teie pakkumise Azure'i portaalis (praegu on piiratud võimsus), siis sulgege hüpikaknas.

Kontrollimiseks andmete teenust Azure portaali (lisaks DataMarket portaali), peate Azure tellimuse ID test.  Tellimuse ID tuvastab konto, mis on lubatud teie pakkumise testida.  

Lõikamine ja kleepige oma tellimuse ID ja valige jätkamiseks märke.

  ![Joonis](media/marketplace-publishing-data-service-test-in-staging/step-1.2.png)

> [AZURE.NOTE] ID-d on ainult need Azure'i tellimused vajalikud testimiseks ja [Azure'i haldusportaal](https://manage.windowsazure.com)lavastus. Ta ei pea Azure'i turuplatsi kontrollimiseks.

Järgmisel kuval, mis näitab, et avaldamine toimub, kuvades "pooleli" ikoon kuvatakse allpool kollane esile tõstetud. Et lavastus lükkamine võtab vahemikku 10 – 15 minutit.  Kui kulub oodatust, esmalt värskendage brauseriakent (IE, vajutage klahvi F5).  Harva, kus teie pakkumise on endiselt lükkamine lavastus tunni pärast, klõpsake kontakti meile link, andke meile teada, et seal on probleeme.

  ![Joonis](media/marketplace-publishing-data-service-test-in-staging/step-1.3.png)

Kui lavastus PTT on lõpule jõudnud ikooni "pooleli" Peata teisaldamine ja olek kuvatakse värskendatakse "Etapiviisiline" abil.  Nüüd olete valmis, et testida teie pakkumise.  

## <a name="step-2-test-your-staged-offer-in-datamarket"></a>Samm 2. Testige oma etapiviisilise pakkumine Datamarketi

Klõpsake linki pärast teksti **"See teenust pakub vastuvõtmise"** et abonendi kuvatakse, kui teie pakkumise läheb tootmisse ja Datamarketi kuvatakse ekraani kuvamiseks.

  ![Joonis](media/marketplace-publishing-data-service-test-in-staging/step-2.2.png)

Testige või veenduge, et iga üksuse 12 märgitud kohal tagada kõigi logod, hinnad tehingud, teksti, pilte, dokumente ja lingid on õiged ja töötavad õigesti.  See on hea aeg tagamaks, mis tahes testi väärtused sisestatud teie pakkumise loomisel on asendatud tegelike väärtuste.

1. Pakkumine logo
2. Pakkumise nimi
3. Ettevõtte veebisaidi Publisheri nime/link
4. Otsing kategooriate teie pakkumise jaoks
5. Teie pakkumise abistamine tellijad tugiteenuste link
6. Kontekstipõhine teie pakkumise kirjeldus
7. Pakkumine leping, mis näitab arve üksikasjad
8. Linkimine lehega rakendamist kood
9. Proovi pildid, mis illustreerivad pakkumise andmete kasutamine
10. Sisend metaandmete sees pakkumine iga teenuse jaoks
11. Pakkumise kasutustingimusi.
12. Pakkumise andmete eelvaade


Lõpetuseks, märkige teenuse töötavad selle Datamarketi kaudu, klõpsates linki "Uurimine see ANDMEKOMPLEKTI".  Uus aken tööriista kutsume "Teenuse Explorer" nii, et teie teenuse vastu, saate kuvada eelvaate päringu tulemused.  Selles aknas saate kuvada päring teenust tulemuste vaatamiseks ja sisestage vajalikud andmed.   Kuvatud on ka URL-i päringu.  

> [AZURE.NOTE] Kindlasti vaadake üle teksti kohal kuvatavad teenuse kirjeldus.  Ja kui teie pakkumine sisaldab rohkem kui ühe teenuse helistada, klõpsake nuppu tabelduskohad allosas järgmise vaatama ja testida teenuse aktiveerimiseks.



## <a name="next-step"></a>Järgmise juhise juurde
Kui teil on probleeme ja vajate nende lahendamiseks võtke ühendust [Toega Azure'i Publisher]( http://go.microsoft.com/fwlink/?LinkId=272975).

Kui olete rahul ja avaldada oma pakkumine lugege [Push tootmise kinnituse taotlemise](marketplace-publishing-push-to-production.md) dokumentatsiooni kohta.

## <a name="see-also"></a>Vt ka
- [Alustamine: Kuidas Azure'i turuplatsi pakkumise avaldamine](marketplace-publishing-getting-started.md)
