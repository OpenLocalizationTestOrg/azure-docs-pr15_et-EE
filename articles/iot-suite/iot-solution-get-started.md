<properties
    pageTitle="MyDriving Azure asjade näide: Lühijuhend | Microsoft Azure'i"
    description="Alustamine rakenduse, mis on täielik tutvustava arhitekt soovitud asjade süsteemi Microsoft Azure'i, sh voo Analytics, seadme õppimine ja sündmuse jaoturi abil."
    services=""
    documentationCenter=".net"
    suite=""
    authors="harikmenon"
    manager="douge"/>

<tags
    ms.service="multiple"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="03/25/2016"
    ms.author="harikm"/>

# <a name="mydriving-iot-system-quick-start"></a>MyDriving asjade süsteemi: Lühijuhend

MyDriving on süsteem, mis näitab kujundus ja tüüpilised [Asjade Interneti](iot-suite-overview.md) (asjade) lahenduse, mis kogutakse telemeetria seadmest, töötleb andmeid pilves ja kehtib seadme Õppekeskuse esitada vastusele rakendamine. Tutvustamise logib andmeid oma auto reisi, kasutades mobiiltelefoni nii adapterit, mille kogub teavet oma auto süsteemi andmeid. See kasutab neid andmeid anda tagasisidet teie võrreldes teiste kasutajate juhtimise laad.

MyDriving tegelik eesmärk on võite alustada oma asjade lahenduse loomisel. Kuid enne seda, mida läheb meie testi kasutaja meeskonna liikmena ise--MyDriving rakenduse. See annab teile kogemus rakendus ja süsteemi taga tarbija, enne kui saate süveneda arhitektuur. See ka tutvustab teile HockeyApp, alfa ja beeta jaotamine rakenduste haldamiseks lahedad viis kasutajate testi.

## <a name="use-the-mobile-experience"></a>Kasutage kasutamise veelgi mugavamaks

Saate rakenduse MyDriving, kui teil on Androidi, iOS-i või Windows 10 seade.

### <a name="android-and-windows-10-mobile-installation"></a>Androidi ja Windows 10 Mobile'i installimine

Seadme:

1.  Rakenduste arendamise lubamiseks tehke järgmist.

    -   Android: Jaotises **sätted** > **Turvalisus**, luba rakendusi **tundmatutest**allikatest.

    -   Windows 10: Klõpsake **sätted** > **värskenduste** > **Jaoks arendajate**, määrake **arendaja režiimis**.

2.  Liitu meie meeskond beeta testi registreerumist või sisselogimisega, [HockeyApp](https://rink.hockeyapp.net). HockeyApp hõlbustab ennetähtaegse versioonides rakenduse testimiseks kasutajatele levitada.

    Kui kasutate Windows 10, kasutage Edge'i brauseri.

    Kui koostamine 2016 osaleja, logige sisse sama Microsofti konto meiliaadressi, et teil registreerida konverentsi, kasutades ühte järgmistest nuppudest Microsoft. Olete juba registreerunud HockeyApp abil.

    ![HockeyApp sisselogimiskuva](./media/iot-solution-get-started/image1.png)

3.  Laadige alla ja installige rakendus siit.

    -   [Android](http://rink.io/spMyDrivingAndroid)

    -   [Windows 10](http://rink.io/spMyDrivingUWP)

    On kaks punkti. Installige serdi **Usaldusväärsed**inimesed. Installige rakendus.

*Probleemidest, alustades rakenduse operatsioonisüsteemis Windows 10 Mobile?* Telefonis võib olla värskendust või kaks taha. Veenduge, et loete uusimate värskenduste või installida.

 - [Microsoft.NET.Native.Framework.1.2.appx](https://download.hockeyapp.net/packages/win10/Microsoft.NET.Native.Framework.1.2.appx) 

 - [Microsoft.NET.Native.Runtime.1.1.appx](https://download.hockeyapp.net/packages/win10/Microsoft.NET.Native.Runtime.1.1.appx) 

 - [Microsoft.VCLibs.ARM.14.00.appx](https://download.hockeyapp.net/packages/win10/Microsoft.VCLibs.ARM.14.00.appx)


### <a name="ios-installation"></a>iOS-i installimine

Kui te osalesid koostamine 2016, laadige rakendus liikmena meie meeskond testi HockeyApp:

1.  [HockeyApp](https://rink.hockeyapp.net)sisselogimiseks oma iOS-seadmes.
    Kasutage ühte Microsoft sisselogimise nupud ja logida sisse sama Microsofti konto meiliaadressi abil konverentsi registreeritud. (Ärge kasutage meiliaadressi ja parooliga väljad.)

    ![HockeyApp sisselogimiskuva](./media/iot-solution-get-started/image1.png)

2.  Armatuurlaua HockeyApp, valige MyDriving ja alla laadida.

3.  Lubada beetaversiooni HockeyApp kaudu:

    lisamine. Avage **sätted** > **Üldine** > **Profiilid ja mobiilsideseadmete halduse.**

    b. **Bit Stadium GmbH** serti usaldada.

Kui te ei osaleda koostamine 2016, saate luua ja juurutada rakenduse ise:

1.   Laadige alla kood [GitHub kaudu].

2.   Koostage ja Juurutage [Xamarin]abil.

Täpsem teave [MyDriving Lühijuhend](http://aka.ms/mydrivingdocs)

## <a name="get-an-obd-adapter-optional"></a>Saada OBD adapterit (valikuline)

See on osa, mis muudab see asjade Interneti tegelik süsteem! Saate kasutada ühte ilma rakendust, kuid on veel päris lõbus ja nad ei ole kallis.

Sisseehitatud diagnostika (OBD) funktsioon auto trimmida ja diagnoosimine paaritu müra ja hoiatuslamp kasutava garaaž auto. Juhul, kui teie auto on suur antiikajast, leiate mõne turvasoklite kuskil salongis tavaliselt taha klapp jaotises armatuurlaud. Õige konnektor, saate hankida mõõdikute mootori jõudluse ja teatud kohandused. Mõne OBD konnektor saab osta odavalt tavalisi. See loob Bluetooth või Wi-Fi abil oma telefonis rakendus.

Selles näites me ei kavatse auto ühendada pilveteenustega. Funktsiooni OBD otse ühendust on oma telefoni, kuid meie rakendus töötab ka relay. Oma auto telemeetria saadetakse otse MyDriving asjade jaoturi, kus toimub töötlemine logima oma tee reisi ja hinnake oma juhtimise laad.

Ühenduse OBD seadmes:

1.  Kontrollige, et teie auto on mõni OBD pesa.

2.  Saada OBD adapterit:

    -   Kui kasutate mõnda Android- või Windows phone, peate Bluetooth-toega OBD II adapterit. Kasutasime [BAFX toodete 34t5 Bluetooth OBDII skannida tööriist].

    -   Kui kasutate iOS-telefon, peate Wi-Fi-enabled OBD adapterit. Kasutasime [ScanTool OBDLink MX WiFi-ühendust: OBD adapterit/diagnostika skanneri].

3.  Järgige tulla adapterit OBD ühenduse oma telefoni. Pidage meeles järgmist:

    -   Bluetooth-adapterit, peab olema telefoniga, klõpsake lehe **sätted** .

    -   Wi-Fi adapterit peab olema vahemiku 192.168.xxx.xxx aadressi.

4.  Kui teil on mitu auto, saate kasutada iga (kuni kolm).

Kui teil pole OBD adapterit, rakendus on ikka saata asukoht ja kiiruse andmeid telefoni kaardid tagasi lõppu ja küsib, kas soovite mõne OBD simuleerida.

Siit leiate lisateavet kohta, kuidas rakendus kasutab OBD adapterit andmeid ja luua oma OBD-seadme jaotises 2.1, "Asjade seadmed," [MyDriving Lühijuhend](http://aka.ms/mydrivingdocs)võimaluste kohta.

## <a name="use-the-app"></a>Rakenduse kasutamine

Käivitage rakendus. On ka algsel Kiirjuhend teid häälestusprotsessis kuidas see toimib.

### <a name="track-your-trips"></a>Jälgida oma reisi

Koputage salvestusnupp (suur punase ringiga ekraani allosas) reisi alustamiseks ja lõpetamiseks koputage uuesti nuppu.

![Illustratsioon salvestusnupp reisi jälgimine](./media/iot-solution-get-started/image2.png)

Iga kord, kui alustate reisi, kui ei ole OBD seade, peate sisestama kui soovite kasutada funktsiooni simulaatorit.

Reisi lõpus puudutage nuppu Lõpeta ja saad kokkuvõte.

![Näide: reisi Kokkuvõte](./media/iot-solution-get-started/image3.png)

### <a name="review-your-trips"></a>Vaadake üle oma reisi

![Viimase reisi näide](./media/iot-solution-get-started/image4.png)

### <a name="review-your-profile"></a>Vaadake üle oma profiilile

![Näide juhtimise laadi profiil](./media/iot-solution-get-started/image5.png)

## <a name="send-us-your-test-feedback"></a>Saatke meile tagasisidet test

Kuna me loodud MyDriving kiire abil oma asjade süsteemide, kindlasti Soovime kuulda teie kohta, kuidas see töötab. Andke meile teada, kui:

- Tekib probleeme või probleeme.

- On laiend punkti, et oleks rohkem sobilik stsenaariumist.

- Te ei leia tõhusam viis täita teatud vajadustele.

- Teil on muid ettepanekuid parandada MyDriving või need dokumendid.

MyDriving rakenduses endas, saate kasutada sisseehitatud HockeyApp tagasiside süsteem: iOS ja Android, lihtsalt raputage telefoni või kasutage menüü käsk **tagasiside** . See automaatselt lisada kuvatõmmis, et teada, mida te ei räägi. Ja kui mis tahes kahetsusväärne jookseb, HockeyApp kogub krahh logid meile öelda nende kohta. Samuti saate anda tagasisidet [HockeyApp portaali]kaudu.

Saate ka faili [probleemi github]või kommentaar allpool (en-us edition).

Vaatame edasi kuulamine teie!

## <a name="next-steps"></a>Järgmised sammud

-   Tutvuge [MyDriving Lühijuhend](http://aka.ms/mydrivingdocs) mõistmaks, kuidas oleme loodud ja ehitatud kogu MyDriving süsteem.

-   [Loomine ja juurutamine süsteemi oma](iot-solution-build-system.md) meie Azure'i ressursihaldur skriptide abil. [MyDriving Lühijuhend](http://aka.ms/mydrivingdocs) juhendab teid ka ala, kus saate teha enamik kohandused.

  [GitHub kaudu]: https://github.com/Azure-Samples/MyDriving
  [Xamarin abil]: https://developer.xamarin.com/guides/ios/getting_started/installation/
  [BAFX toodete 34t5 Bluetooth OBDII kontrolli tööriista]: http://www.amazon.com/gp/product/B005NLQAHS
  [ScanTool OBDLink MX WiFi-ühendust: OBD adapterit/diagnostika skanner]: http://www.amazon.com/gp/product/B00OCYXTYY/ref=s9_simh_gw_g263_i1_r?pf_rd_m=ATVPDKIKX0DER&pf_rd_s=desktop-2&pf_rd_r=1MWRMKXK4KK9VYMJ44MP
  [HockeyApp portaal]: https://rink.hockeyapp.org
  [probleemi github]: https://github.com/Azure-Samples/MyDriving/issues
