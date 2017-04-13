<properties 
    pageTitle="Microsoft Azure'i mitme teguri autentimist kasutaja Ühendriigid"
    description="Teave kasutaja riikide Azure'i MFA."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="user-states-in-azure-multi-factor-authentication"></a>Kasutaja riikide Azure'i Mitmikautentimise

Azure'i Mitmikautentimise Kasutajakontod on järgmised kolm erinevate olekut:

Olek | Kirjeldus |Mitte-brauseri rakenduste mõjutatud| Märkmete
:-------------: | :-------------: |:-------------: |:-------------: |
Keelatud | Uue kasutaja pole registreeritud rakenduses mitmikautentimise vaikeolekusse.|Ei|Kasutaja ei kasuta mitmikautentimise.
Lubatud |Kasutaja on antud õpib mitmikautentimise.|Ei.  Need jätkama, kuni registreerimise protsess on lõpule viidud.|Kasutaja on lubatud, kuid on lõppenud registreerimise käigus. Pakutakse lõpule viia veebisaidil järgmise Logi sisse.
Jõustatud|Kasutaja on antud registreeritud ja on lõppenud registreerimise käigus mitmikautentimise kasutamise kohta.|Jah.  Rakenduse parooli küsimiseks rakendused. | Kasutaja võib või võib olla lõpetatud registreerimise. Kui nad registreerimise protsess lõpule viinud, siis nad kasutavad mitmikautentimise. Muul juhul kasutaja palutakse järgmise sisselogimist kell protsessi lõpuleviimine.

## <a name="changing-a-user-state"></a>Kasutaja oleku muutmine
Kasutajate olek muutub sõltuvalt sellest, kas see on MFA jaoks häälestamise ja kas kasutajal on lõpule.  Kui lülitate MFA kasutaja, kasutajate olek muutub keelatud lubatud.  Kui kasutaja, kelle olek on muutunud lubatud, sinna sisse logib ja protsess on lõpule jõudnud, nende olek muutub jõustatud.  

### <a name="to-view-a-users-state"></a>Kasutaja oleku vaatamiseks
--------------------------------------------------------------------------------
1.  Logige sisse **Azure klassikaline portaali** administraatorina.
2.  Klõpsake vasakul nuppu **Active Directory**.
3.  Jaotises, klõpsake **Directory** kataloogi kasutaja soovite lubada.
![Klõpsake nimistust](./media/multi-factor-authentication-get-started-cloud/directory1.png)
4.  Ülaosas nuppu **Kasutajad**.
5.  Klõpsake lehe allosas nuppu **Hallata mitut tegurit Auth**.
![Klõpsake nimistust](./media/multi-factor-authentication-get-started-cloud/manage1.png)
6.  See avab veebibrauseri uuel vahekaardil.  On võimalik vaadata kasutajate olek.
![Klõpsake nimistust](./media/multi-factor-authentication-get-started-user-states/userstate1.png)

###<a name="to-change-the-state-from-disabled-to-enabled"></a>Keelatud muuta kaudu juurde lubatud
1.  Logige sisse **Azure klassikaline portaali** administraatorina.
2.  Klõpsake vasakul nuppu **Active Directory**.
3.  Jaotises, klõpsake **Directory** kataloogi kasutaja soovite lubada.
![Klõpsake nimistust](./media/multi-factor-authentication-get-started-cloud/directory1.png)
4.  Ülaosas nuppu **Kasutajad**.
5.  Klõpsake lehe allosas nuppu **Hallata mitut tegurit Auth**.
![Klõpsake nimistust](./media/multi-factor-authentication-get-started-cloud/manage1.png)
6.  See avab veebibrauseri uuel vahekaardil.  Leidke kasutaja, mida soovite mitmikautentimise lubamine. Kui peate vaate ülaosas. Veenduge, et olek on **keelatud.** 
 ![Luba kasutaja](./media/multi-factor-authentication-get-started-cloud/enable1.png)
7.  **Märkige ruut** paigutamiseks tema nime kõrval olev ruut.
7.  Klõpsake parempoolsel paanil nuppu **Luba**.
![Kasutaja lubamine](./media/multi-factor-authentication-get-started-cloud/user1.png)
8.  Klõpsake nuppu **Luba mitme teguri auth**.
![Kasutaja lubamine](./media/multi-factor-authentication-get-started-cloud/enable2.png)
9.  Märkate peaks kasutaja on muutunud **keelatud** , **lubatud**.
![Lubamine](./media/multi-factor-authentication-get-started-cloud/user.png)
10.  Kui olete lubanud kasutajaid, on soovitatav need teavitamise e-posti teel.  Samuti teavitama nende kasutamise oma-brauseri rakenduste vältimiseks failist välja lukustatud.

### <a name="to-change-the-state-from-enabledenforced-to-disabled"></a>Muutmiseks olek lubatud/jõustatud keelatud
1.  Logige sisse **Azure klassikaline portaali** administraatorina.
2.  Klõpsake vasakul nuppu **Active Directory**.
3.  Jaotises, klõpsake **Directory** kataloogi kasutaja soovite lubada.
![Klõpsake nimistust](./media/multi-factor-authentication-get-started-cloud/directory1.png)
4.  Ülaosas nuppu **Kasutajad**.
5.  Klõpsake lehe allosas nuppu **Hallata mitut tegurit Auth**.
![Klõpsake nimistust](./media/multi-factor-authentication-get-started-cloud/manage1.png)
6.  See avab veebibrauseri uuel vahekaardil.  Leidke kasutaja, mida soovite blokeerida. Kui peate vaate ülaosas. Veenduge, et olek on kas **lubatud** või **jõustatud.**
7.  **Märkige ruut** paigutamiseks tema nime kõrval olev ruut.
7.  Klõpsake paremal nuppu **Keela**.
![Kasutaja keelamine](./media/multi-factor-authentication-get-started-user-states/userstate2.png)
8.  Teil palutakse kinnitada.  Klõpsake nuppu **Jah**.
![Kasutaja keelamine](./media/multi-factor-authentication-get-started-user-states/userstate3.png)
9.  Peaksite nägema siis selle õnnestus.  Klõpsake **sulgemiseks.** 
 ![Keela kasutaja](./media/multi-factor-authentication-get-started-user-states/userstate4.png)
