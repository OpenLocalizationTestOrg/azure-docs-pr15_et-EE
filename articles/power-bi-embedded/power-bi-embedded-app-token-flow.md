<properties
   pageTitle="Autentimist ja lubab koos Power BI manustatud"
   description="Autentimist ja lubab koos Power BI manustatud"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="authenticating-and-authorizing-with-power-bi-embedded"></a>Autentimist ja lubab koos Power BI manustatud

Power BI manustatud teenus kasutab **võtmed** ja **Rakenduse märkide** autentimise ja luba, mitte konkreetsete lõppkasutaja autentimist. See mudel rakenduse haldab autentimise ja luba oma lõppkasutajatele. Vajaduse korral rakenduse loob ja saadab rakenduse märgid, mis ütleb teenusele nõutud aruannet renderdamiseks. Seda kujundust ei nõua rakenduse kasutamiseks Azure Active Directory kasutaja autentimise ja luba, kuigi saate endiselt.

## <a name="two-ways-to-authenticate"></a>Kaks võimalust autentida

**Klahv** - klahvid saate kasutada kõigi Power BI manustatud REST API kõnede jaoks. Võtmed leiate **Azure'i portaalis** , klõpsates nuppu **Kõik sätted** ja seejärel **kiirklahvide**kohta. Alati kasutatakse tootevõti, nagu oleks parooli. Need võtmed on teha mis tahes REST API helistamiseks kindla tööruumi saidikogumi õigused.

ÜLEJÄÄNUD telefonil klahvi kasutamiseks järgmised autoriseerimine päise lisamiseks tehke järgmist.            

    Authorization: AppKey {your key}

**App luba** - rakenduse sõned kasutatakse kõigi manustamine taotlused. Nad on mõeldud käivitada kliendipoolne, et need on piiratud ühe aruande ja parimaks Määrake aegumise kellaaeg on.

Rakenduse sõned on JWT (JSON Web Turbeloa), mis on allkirjastatud oma kiirklahvidele.

Teie app luba võib olla järgmised nõuded.

| Nõue      | Kirjeldus        |
|--------------|------------|
| **ver**      | Luba rakenduse versioon. 0.2.0 on praegune versioon.       |
| **AUD**      | Luba adressaadini. Power BI manustatud kasutamiseks: "https://analysis.windows.net/powerbi/api".  |
| **ISS**      |  String, mis näitab rakendus luba andnud.    |
| **tüüp**     | App luba loodava tüüp. Praeguse ainus toetatud tüüp on **manustamine**.   |
| **WCN**      | Tööruumi saidikogumi nimi luba väljastatakse.  |
| **WID**      | Tööruumi ID luba väljastatakse.  |
| **lahti**      | Aruande ID luba väljastatakse.     |
| **kasutajanimi** (valikuline) |  RLS kasutanud, see on string, mis aitab tuvastada kasutaja kui RLS reeglite rakendamise. |
| **rollid** (valikuline)   |   String, mis sisaldab soovitud rea turvalisuse reeglite rakendamise rollid. Kui möödudes rohkem kui üks roll, peaks olema möödunud sting massiivi.    |
| **exp** (valikuline)    |   Näitab, kus luba aegub aeg. Need peaks olema edasi, nimega Unix ajatemplid.   |
| **nbf** (valikuline)    |   Näitab, kus käivitatakse luba, ei sobi aeg. Need peaks olema edasi, nimega Unix ajatemplid.   |

Valimi app luba näeb välja umbes järgmine:

![](media\power-bi-embedded-app-token-flow\power-bi-embedded-app-token-flow-sample-coded.png)


Kui dekodeerida, seda nägema selline:

![](media\power-bi-embedded-app-token-flow\power-bi-embedded-app-token-flow-sample-decoded.png)


## <a name="heres-how-the-flow-works"></a>Siin on voogu tööpõhimõtted

1. Kopeerige rakenduse API võtmed. Saate võtmed **Azure'i**portaalis.

    ![](media\powerbi-embedded-get-started-sample\azure-portal.png)

2. Luba väidab nõude ja on ka aegumise aega.

    ![](media\powerbi-embedded-get-started-sample\power-bi-embedded-token-2.png)

3. Luba saab allkirjastatud API juurdepääsu muuteklahvidega.

    ![](media\powerbi-embedded-get-started-sample\power-bi-embedded-token-3.png)

4. Kasutaja taotlused aruannet.

    ![](media\powerbi-embedded-get-started-sample\power-bi-embedded-token-4.png)

5.  Luba kinnitatakse API juurdepääsu muuteklahvidega.

    ![](media\powerbi-embedded-get-started-sample\power-bi-embedded-token-5.png)

6.  Power BI manustatud saadab aruande kasutaja.

    ![](media\powerbi-embedded-get-started-sample\power-bi-embedded-token-6.png)

Pärast kasutaja **Power BI manustatud** saadab aruande, kasutaja kohandatud rakenduse aruannet vaadata. Näiteks kui importisite [analüüsimine müük andmete PBIX valimi](http://download.microsoft.com/download/1/4/E/14EDED28-6C58-4055-A65C-23B4DA81C4DE/Analyzing_Sales_Data.pbix), valimi web appi peaksite nägema selline:

![](media\powerbi-embedded-get-started-sample\sample-web-app.png)

## <a name="see-also"></a>Vt ka
- [Microsoft Power BI manustatud valimi kasutamise alustamine](power-bi-embedded-get-started-sample.md)
- [Microsoft Power BI manustatud tavastsenaariumid](power-bi-embedded-scenarios.md)
- [Microsoft Power BI manustatud alustamine](power-bi-embedded-get-started.md)
