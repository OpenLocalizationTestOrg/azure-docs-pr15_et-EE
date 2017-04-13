<properties
    pageTitle="Azure'i masina õ klassikaline Web teenuse ümberõppele tõrkeotsing | Microsoft Azure'i"
    description="Leida ja parandada levinud probleemide encounted, kui teil on ümberõpe mudeli Azure seadme õ veebiteenus."
    services="machine-learning"
    documentationCenter=""
    authors="VDonGlover"
   manager="raymondl"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="v-donglo"/>

# <a name="troubleshooting-the-retraining-of-an-azure-machine-learning-classic-web-service"></a>Teenuse Azure seadme õ klassikaline Web ümberõpet tõrkeotsing

## <a name="retraining-overview"></a>Ümberõpe ülevaade

Sõnastikupõhise katse nimega hinded veebiteenuse juurutamisel see on staatiline mudel. Uute andmete vabanemisest teavitamise või kui tarbija API on oma andmete mudelisse peab koolitada ümber. 

Klassikaline veebiteenuse ümberõpe protsessi lõpetamine lühiülevaade leiate [Ümber masina õ mudelite programmiliselt](machine-learning-retrain-models-programmatically.md).

## <a name="retraining-process"></a>Ümberõpe protsess

Kui teil on vaja koolitada veebiteenuse, peate lisama täiendava mõningaid.

* Veebiteenuse juurutatud koolitus katse kaudu. Katse peab olema lisatud väljundi **Rongi mudeli** mooduli **Web teenuse väljundi** mooduli.  

    ![Kinnitage web teenuse väljundi rongi mudel.][image1]

* Uus näitaja, mis lisatakse teie hinded veebiteenus.  Lõpp-punkti proovi kood viidatud ümber masina õ mudelite programmiliselt programmiliselt abil saate lisada teema või Azure klassikaline portaali kaudu.

Seejärel saate näidis C# koodi koolitus veebiteenuse API abi lehelt koolitada mudel. Kui teil on hinnatud tulemid ja on rahul, saate värskendada koolitatud mudeli hinded veebiteenuse abil uue lõpp-punkti, mille äsja lisasite.

Kõik osad kohas, kus põhietappi, peate tegema, mudeli koolitada on järgmised:

1.  Kõne veebiteenuse koolitus: kõne on et paketi täitmise teenuse (BES), mitte taotleda vastus teenuse (PVR). Saate näidis C# koodi lehel API abi helistamiseks. 
2.  Otsige *BaseLocation*, *RelativeLocation*ja *SasBlobToken*väärtused: need väärtused on tagastatud väljund koolitus veebiteenuse ümbersuunamine. 
      ![kus on kuvatud ümberõpe valimi ning BaseLocation, RelativeLocation ja SasBlobToken väärtused.][image6]
3.  Värskendage lisatud lõpp-punkti hinded veebiteenusest uue koolitatud mudeliga: proovi kood lähtutud ümber masina õ mudelite programmiliselt, kasutades värskendada uue lõpp-punkti lisatud hinded mudeli äsja koolitatud mudeliga koolitus veebiteenusest.

## <a name="common-obstacles"></a>Levinud takistused

### <a name="check-to-see-if-you-have-the-correct-patch-url"></a>Kontrollige, kas teil on õige paik URL

Kasutate paik URL peab olema seostatud hinded veebiteenuse lisatud uus hinded lõpp. On mitmel viisil paik URL-i hankimiseks.

**Variant 1: Programatically**

Saada õige paik URL:

1.  Käivitage [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) proovi kood.
2.  Väljundi AddEndpoint, otsige üles *HelpLocation* väärtus ja kopeerige URL.

    ![HelpLocation väljund addEndpoint proovi.][image2]

3.  Kleepige URL brauseri lehele, mis pakub Spikrilingid veebiteenuse liikumiseks.
4.  Paikade abi lehe avamiseks linki **Värskenda ressursi** .

**Variant 2: Kasutada Azure klassikaline portaali**

1.  [Azure'i klassikaline portaali](https://manage.windowsazure.com)sisse logida.
2.  Avage vahekaart masina õ. 
     ![Seadme viltune menüü.][image4]
3.  Klõpsake tööruumi nime, siis **Veebiteenused**.
4.  Klõpsake hinded veebiteenus, millega töötate. (Kui te ei muuda veebiteenuse vaikenime, see lõpeb [Exp hinded.].)
5.  Klõpsake **Lisa lõpp-punkti**.
6.  Lõpp-punkti on lisatud, klõpsake nuppu lõpp-punkti nimi. Klõpsake nuppu **Värskenda ressursi** lappimine abi lehe avamiseks.

>[AZURE.NOTE] Kui olete lisanud lõpp-punkti asemel sõnastikupõhise veebiteenuse veebiteenuse koolitus, saate järgmine tõrketeade kui klõpsate linki **Värskenda ressurss** : Kahjuks, kuid see funktsioon pole toetatud või sellega saadaval. See veebiteenus on värskendatav ressursse. Palume vabandust ebamugavuste ja parandada see töövoog töötab.

![Lõpp-punkti armatuurlaud.][image3]

Spikrilehele PAIKADE sisaldab tuleb kasutada paik URL ja pakub proovi kood abil saate kutsuda.

![Paikade URL.][image5]

### <a name="check-to-see-that-you-are-updating-the-correct-scoring-endpoint"></a>Kontrollige, et te värskendate hinded õige lõpp-punkti

* Paik veebiteenuse koolitus: paik toimingut tuleb teha hinded veebiteenus.
* Paik vaikimisi lõpp-punkti veebiteenuse: paik toimingut tuleb teha uue hinded Web teenuse lõpp-punkti lisatud.

Saate kontrollida, millist veebiteenuse Azure klassikaline portaali külastades on lõpp-punkti. 

>[AZURE.NOTE] Pidage meeles, et lisate sõnastikupõhise veebiteenus pole koolitus veebiteenuse lõpp-punkti. Kui olete õigesti juurutanud koolitus nii sõnastikupõhise veebiteenuse, peaksite nägema kahte eraldi veebiteenuste loetletud. Sõnastikupõhise veebiteenuse peaks lõpus "[sõnastikupõhise exp.]".

1.  [Azure'i klassikaline portaali](https://manage.windowsazure.com)sisse logida.
2.  Avage vahekaart masina õ. 
     ![Seadme õ tööruumi UI.][image4]
3.  Valige oma tööruumi.
4.  Klõpsake **veebiteenused**.
5.  Valige oma sõnastikupõhise veebiteenus.
6.  Veenduge, et uue lõpp-punkti lisati veebiteenuse.

### <a name="check-the-workspace-that-your-web-service-is-in-to-ensure-it-is-in-the-correct-region"></a>Märkige ruut tööruum, mis see on õige piirkonnas on teie veebiteenuse

1.  [Azure'i klassikaline portaali](https://manage.windowsazure.com)sisse logida.
2.  Valige menüüst käsk arvuti õ.
      ![Seadme õ piirkond UI.][image4]
3.  Kinnitage oma tööruumi asukoht.

<!-- Image Links -->

[image1]: ./media/machine-learning-troubleshooting-retraining-a-model/ml-studio-tm-connnected-to-web-service-out.png
[image2]: ./media/machine-learning-troubleshooting-retraining-a-model/addEndpoint-output.png
[image3]: ./media/machine-learning-troubleshooting-retraining-a-model/azure-portal-update-resource.png
[image4]: ./media/machine-learning-troubleshooting-retraining-a-model/azure-portal-machine-learning-tab.png
[image5]: ./media/machine-learning-troubleshooting-retraining-a-model/ml-help-page-patch-url.png
[image6]: ./media/machine-learning-troubleshooting-retraining-a-model/retraining-output.png
[image7]: ./media/machine-learning-troubleshooting-retraining-a-model/web-services-tab.png