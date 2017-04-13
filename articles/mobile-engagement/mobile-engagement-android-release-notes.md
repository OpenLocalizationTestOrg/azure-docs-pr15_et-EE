<properties
    pageTitle="Azure'i mobiilsideseadmete kaasamine Android SDK integreerimine"
    description="Uusimad värskendused ja toiminguid Android SDK Azure Mobile kaasamine"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="piyushjo" />

# <a name="release-notes"></a>Väljalaskemärkmed

## <a name="423-08102016"></a>4.2.3 (08/10/2016)

- Lisateavet Wi-Fi-blokeering.
- Lahendada Tupiku helistamisel getDeviceId enne käivitamise (viga kasutusele 4.2.0).

## <a name="422-05172016"></a>4.2.2 (05/17/2016)

- Stabiilsuse täiustused.

## <a name="421-05102016"></a>4.2.1 (05/10/2016)

- Turvalisus: keelamine veebipääs vaade kohalik fail.
- Turvalisus: eemaldamine `EngagementPreferenceActivity` aegunud ja ebaturvalise klassi `PreferenceActivity` klassi.
- Turvalisus: REACHi tegevused on nüüd dokumenteerida kasutada `exported="false"`, see lipp saab kasutada ka SDK varasemas versioonis.

## <a name="420-03112016"></a>4.2.0 (03/11/2016)

- SDK on nüüd litsents MIT.
- Luba, määrata kohandatud seadme identifikaator SDK käivitamise ajal.

## <a name="415-02012016"></a>4.1.5 (02/01/2016)

- Stabiilsuse täiustused.

## <a name="414-01262016"></a>4.1.4 (2016/01/26)

- Stabiilsuse täiustused.

## <a name="413-1292015"></a>4.1.3 (2015/12/9)

- Stabiilsuse täiustused.

## <a name="412-11252015"></a>4.1.2 (11/25/2015)

- Stabiilsuse täiustused.

## <a name="411-11042015"></a>4.1.1 (11/04/2015)

- Stabiilsuse täiustused.

## <a name="410-08252015"></a>4.1.0 (08/25/2015)

- Androidi m toime uus õiguste mudel.
- Nüüd saate konfigureerida asukoht funktsioonide kasutamise asemel käitusajal `AndroidManifest.xml`.
- Määrata õiguse viga: kui kasutate `ACCESS_FINE_LOCATION`, siis `ACCESS_COARSE_LOCATION` ei ole enam vajalik.
- Stabiilsuse täiustused.

## <a name="400-07062015"></a>4.0.0 (07/06/2015)

-   Sisemise protokolli muudatusi teha analüüsi- ja tõuketeatised usaldusväärne.
-   Kohalikke tõuketeatised (GCM/ADM) nüüd kasutatakse ka jaoks rakenduste teatised, mis tahes tüüpi tõuketeatised turunduskampaania kohalikke tõuketeatised identimisteave tuleb konfigureerida.
-   Lahendada üldpildiga teatis: need olid kuvatud ainult 10s lükatud.
-   Vea parandamine veebivaade: lingi klõpsamisel ka käivitamisel vaikimisi toimingu URL.
-   Seotud kohaliku mäluhaldus harvaesinevate krahhi lahendada.
-   Dünaamiline konfiguratsiooni stringi juhtimine lahendada.
-   Värskendage EULA.

## <a name="300-02172015"></a>3.0.0 (02/17/2015)

-   Algse versiooni Azure mobiilsideseadmete kaasamine
-   appId konfiguratsiooni asendatakse ühenduse stringi konfiguratsiooni.
-   Sõnumite saatmiseks ja vastuvõtmiseks suvalise XMPP suvalise XMPP üksuste API eemaldada.
-   Eemaldatud API seadmete vahel sõnumite saatmiseks ja vastuvõtmiseks.
-   Turvalisuse täiustamine.
-   Google Play ja SmartAd jälitus eemaldada.
