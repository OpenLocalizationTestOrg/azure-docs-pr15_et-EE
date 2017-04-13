<properties
    pageTitle="Pidev kohaletoimetamise Visual Studio meeskonnatöö teenuste Azure | Microsoft Azure'i"
    description="Saate teada, kuidas oma Visual Studio Team Services meeskond projektid automaatselt luua ja võtta kasutusele veebirakenduse funktsiooni Azure'i rakendust Service või cloud services konfigureerimine."
    services="cloud-services"
    documentationCenter=".net"
    authors="mlearned"
    manager="douge"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="07/06/2016"
    ms.author="mlearned"/>

# <a name="continuous-delivery-to-azure-using-visual-studio-team-services"></a>Pidev kohaletoimetamise Azure Visual Studio meeskonnatöö teenuste kaudu

Saate konfigureerida oma Visual Studio Team Services meeskond projektid automaatselt koostamine ja Azure veebirakenduste kasutuselevõtuks või pilveteenused.  (Pidev koostamine luua ja juurutada abil *kohapealse* Team Foundation Server süsteemi kohta leiate teavet teemast [Pidev kohaletoimetamise puhul pilveteenuste Azure](cloud-services-dotnet-continuous-delivery.md).)

Selle õpetuse eeldab, et teil on Visual Studio 2013 ja Azure SDK installitud. Kui teil pole veel Visual Studio 2013 alla laadida, valides **tasuta alustamine** veebisaidil [www.visualstudio.com](http://www.visualstudio.com)link. Installige Azure'i SDK [siin](http://go.microsoft.com/fwlink/?LinkId=239540).

> [AZURE.NOTE]Peate selle õpetuse lõpuleviimiseks konto Visual Studio Team Services: saate [avada Visual Studio Team Services konto tasuta](http://go.microsoft.com/fwlink/p/?LinkId=512979).

Pilveteenus automaatselt luua ja juurutada Azure Visual Studio Team Services abil häälestada, tehke järgmist.

## <a name="1-create-a-team-project"></a>1: meeskonnatöö projekti loomine

Järgige juhiseid [siin](http://go.microsoft.com/fwlink/?LinkId=512980) luua oma projekti ja linkida selle Visual Studio. Juhendis eeldab, et kasutate andmeallika juhtelemendi lahendusena Team Foundation versiooni kontroll (TFVC). Kui soovite kasutada Git versiooni kontrollimine, lugege teemat [juhendis Git versiooni](http://go.microsoft.com/fwlink/p/?LinkId=397358).

## <a name="2-check-in-a-project-to-source-control"></a>2: projekti andmeallika juhtelemendi sissemöllimine

1. Visual Studio, avage lahendus üles juurutatav või looge uus.
Ülevaate juhiseid järgides saate juurutada web Appi kui ka mõnda pilveteenusesse (Azure'i rakendus).
Kui soovite luua uue lahenduse, luua uue projekti Azure'i pilveteenuses või ASP.net-i MVC uue projekti. Veenduge, et projekti eesmärgid .NET Framework 4 või 4.5 ja kui teenus cloud projekti loomist lisada ASP.net-i MVC web rolli ja töötaja roll ja valige Interneti rakenduse web rolli. Vastava viiba kuvamisel valige **Interneti rakendus**.
Kui soovite luua veebirakenduse, ASP.net-i veebirakenduse projekti malli valimine ja seejärel valige MVC. Lugege teemat [loomine Azure'i rakendust Service ASP.net-i web app](../app-service-web/web-sites-dotnet-get-started.md).

    > [AZURE.NOTE] Visual Studio meeskonnatöö teenused toetavad ainult praegu CI juurutuste Visual Studio veebirakendusi. Veebisaidi projektid on väljas.

1. Lahendus kontekstimenüü avamiseks ja valige **Andmeallika juhtelemendi lahenduse lisada**.

    ![][5]

1. Nõustuge või vaikesätete muutmiseks ja klõpsake nuppu **OK** . Kui protsess jõuab lõpule, kuvatakse **Solution**Exploreris andmeallika juhtelemendi ikoonid.

    ![][6]

1. Lahendus kiirmenüü avamine ja valige **Mölli sisse**.

    ![][7]

1. **Team Explorer**alal **Ootel muudatusi** tippige soovitud sisseregistreerimise kommentaari ja klõpsake nuppu **Mölli sisse** .

    ![][8]

    Pange tähele suvandid kaasamiseks või välistamiseks kindlaid muudatusi, kui märgite ruudu. Kui soovitud muudatused on välja jäetud, valige **Kaasata kõik** link.

    ![][9]

## <a name="3-connect-the-project-to-azure"></a>3: Azure'i projekti ühendamine

1. Nüüd, kui teil on mõne VS meeskonnatöö teenuste projekti koos mõne lähtekoodi, olete valmis oma projekti ühenduse Azure'i.  [Azure'i klassikaline portaalis](http://go.microsoft.com/fwlink/?LinkID=213885)valige pilvepõhise teenuse või web appi või looge uus, valides soovitud **+** ikoon vasakus allnurgas ja valides **Pilveteenuses** või **Web App** ja seejärel **Kiiresti luua**. Valige link **häälestamine avaldamise Visual Studio meeskonnatöö teenustega** .

    ![][10]

1. Viisardi, tippige tekstiväljale oma Visual Studio Team Services konto nimi ja klõpsake linki **Lubada kohe** . Teil palutakse sisse logida.

    ![][11]

1. **Ühenduse taotlemine** hüpikakende dialoogiboksi nuppu **Aktsepteeri** lubada Azure konfigureerimine oma projekti VS meeskonnatöö teenused.

    ![][12]

1. Kui autoriseerimine õnnestub, kuvatakse teie Visual Studio Team Services meeskonna projektide loend sisaldavad rippmenüüst. Valige eelmiste juhiste järgi loodud projekti nime ja seejärel valige viisardinupp märke.

    ![][13]

1. Pärast projekti on lingitud, kuvatakse mõned juhised muudatused Visual Studio Team Services meeskonna projekti kontrollimine.  Klõpsake oma järgmise sisse, kuvatakse Visual Studio Team Services Koostage ja Juurutage projekti Azure.  Proovige seda kohe mõõt klõpsates linki **Mölli sisse Visual Studio** ja seejärel **Käivitage Visual Studio** link (või **Visual Studio** võrdväärse nupp portaali ekraani allosas).

    ![][14]

## <a name="4-trigger-a-rebuild-and-redeploy-your-project"></a>4: käivitamine taastada ja projekti ümberkorraldamine

1. Visual Studio **Team Explorer** **Andmeallika juhtelemendi Exploreri** lingi valimine.

    ![][15]

1. Liikuge oma lahenduse fail ja avage see.

    ![][16]

1. **Solution Exploreris**faili avada ja muuta. Näiteks muuta faili `_Layout.cshtml` jaotises vaated\\rolli MVC web jagatud kaust.

    ![][17]

1. Redigeerige saidi logo ja valige faili salvestamiseks **Klahvikombinatsiooni Ctrl + S** .

    ![][18]

1. Valige **Meeskonnatöö Exploreri** **Ootel muudatusi** link.

    ![][19]

1. Sisestage kommentaari ja seejärel klõpsake nuppu **Mölli sisse** .

    ![][20]

1. Klõpsake nuppu **Avaleht** **Team Explorer** avalehele naasmiseks.

    ![][21]

1. Valige **koostab** linki, et vaadata parandused on pooleli.

    ![][22]

    **Meeskonnatöö Explorer** näitab, et ehitada on käivitanud teie sisseregistreerimise.

    ![][23]

1. Topeltklõpsake koostamine on pooleli vaadata üksikasjalikku Logi koostamine edenedes nime.

    ![][24]

1. Kuigi koostamine on pooleli, tutvuge Koosta määratluse Azure'i TFS seotud viisardi abil loodud.  Koosta määratluse kiirmenüü avamine ja valige **Redigeeri koostada määratlus**.

    ![][25]

    **Päästik** menüüs, näete, et Koosta määratlus on vaikimisi koostamiseks iga sisse-ja.

    ![][26]

    Klõpsake vahekaarti **protsess** saate vaadata juurutamine keskkonnas on seatud pilve teenuse või web rakenduse nimi. Kui töötate web apps, näete atribuutide on erinevad näidatud siin.

    ![][27]

1. Kui soovite muu väärtus kui vaikeväärtused, määrake atribuutide väärtused. Azure'i avaldamise atribuutide on jaotises **juurutamise** .

    Järgmine tabel sisaldab saadaval atribuudid jaotises **juurutamise** .

  	|Atribuut|Vaikeväärtus|
  	|---|---|
  	|Luba ebausaldusväärsete serdid|Kui väärtus on false, peab SSL-sertide allkirjastatud juurorgan.|
  	|Luba täiendamine|Võimaldab juurutamise värskendada olemasoleva juurutuse asemel luua uue eksemplari. Säilitab IP-aadress.|
  	|Kustutamine|Kui väärtus on true, kirjutada olemasoleva seostamata juurutuse (täiendamine on lubatud).|
  	|Tee juurutamise sätted|Web app, suhtes repo juurkausta .pubxml faili tee. Ignoreerinud puhul pilveteenuste.|
  	|SharePointi juurutamise keskkonnas|Sama, mis teenuse nimi.|
  	|Azure'i juurutamise keskkonnas|Web appi või pilvepõhises teenuse nimi.|

1. Kui kasutate mitut teenuse konfiguratsioone (.cscfg failid), saate **koostada, täpsemalt MSBuild argumendid** , milles soovitud teenuse konfigureerimine. Näiteks ServiceConfiguration.Test.cscfg kasutamiseks seada MSBuild argumendid rea valik `/p:TargetProfile=Test`.

    ![][38]

    Sel ajal, peaks teie koostamine lõpule viidud.

    ![][28]

1. Kui topeltklõpsate Koosta nime, kuvatakse Visual Studio **Build Kokkuvõte**, sh mis tahes seotud ühiku test projektide testi tulemused.

    ![][29]

1. [Azure'i klassikaline portaali](http://go.microsoft.com/fwlink/?LinkID=213885), saate vaadata seotud juurutamise menüü **juurutuste** lavastus keskkonna valimisel.

    ![][30]

1.  Liikuge sirvides oma veebisaidi URL. Web app, klõpsake lihtsalt käsuribal nuppu **Sirvi** . Pilveteenus valida URL-i **armatuurlaua** leht, mis näitab lavastus keskkonna pilveteenus **Kiirülevaate lühidalt** osas. Pidev integratsioon puhul pilveteenuste kasutuselevõttu on avaldatud lavastus keskkonna vaikimisi. Saate muuta seda, seades **Alternatiivse pilvepõhise teenuse keskkonna** atribuuti **tootmisele**. Kui saidi URL-i on lehel soovitud pilveteenuses armatuurlaua kuvatõmmis kuvatakse.

    ![][31]

    Veebibrauseri uuel vahekaardil avatakse esile töötava saidile.

    ![][32]

    Pilveteenustega, kui teete teiste muudatuste projekti, saate käivitada veel koostab ja teil võib mitme juurutuste. Viimane tähisega aktiivne.

    ![][33]

## <a name="5-redeploy-an-earlier-build"></a>5: varasema ümberkorraldamine

Selles etapis tuleb kehtib pilveteenustega ja pole kohustuslik. Azure'i klassikaline portaalis, valige mõni varasem juurutamise ja seejärel valige tagasikerimine saidi mõne varasema sisse-ja **Juurutage uuesti** nuppu.  Pange tähele, et see käivitab uue Koosta TFS ja uue kirje loomine teie juurutamise ajalugu.

![][34]

## <a name="6-change-the-production-deployment"></a>6: tootmise juurutamise muutmine

Selles etapis tuleb kehtib ainult pilveteenustega, mitte veebirakenduste. Kui olete valmis, saate reklaamida lavastus keskkonna tootmiskeskkonda valides nupu **Vaheta** Azure klassikaline portaalis. Äsja juurutatud lavastus keskkond on esiletõstetud tootmisele ja eelmise tootmiskeskkonda, kui muutub lavastus keskkonnas. Aktiivse juurutamise võivad olla erinevad ja lavastus keskkonnas, kuid juurutamise ajalugu tehtud järgud on samad sõltumata keskkonnas.

![][35]

## <a name="7-run-unit-tests"></a>7: ühiku analüüse

Selles etapis tuleb kehtib ainult veebirakendusi, mitte cloud services. Panemiseks juurutuse kvaliteedi gate käivitada üksuse kontrollib ja kui ei õnnestu, võite peatada juurutamise.

1.  Visual Studio, lisage ühiku test projekti.

    ![][39]

1.  Saate lisada projekti viited projekti, mida soovite testida.

    ![][40]

1.  Lisage mõni üksus kontrollib. Töö alustamine, proovige fiktiivne test, mis läheb alati.

        ```
        using System;
        using Microsoft.VisualStudio.TestTools.UnitTesting;

        namespace UnitTestProject1
        {
            [TestClass]
            public class UnitTest1
            {
                [TestMethod]
                [ExpectedException(typeof(NotImplementedException))]
                public void TestMethod1()
                {
                    throw new NotImplementedException();
                }
            }
        }
        ```

1.  Koosta definitsiooni redigeerimine, klõpsake vahekaarti **protsess** ja laiendage **Test** .

1.  **Fail Koosta testi korral** väärtuseks True. See tähendab, et juurutamise ei ilmneda juhul, kui testide edasi.

    ![][41]

1.  Uus ehitada järjekorda.

    ![][42]

    ![][43]

1. Kuigi koostamine edeneb, märkige ruut edenemisega.

    ![][44]

    ![][45]

1. Kui koostamine on lõpule jõudnud, kontrollige, kas testi tulemused.

    ![][46]

    ![][47]

1.  Proovige luua test, mis ei õnnestu. Lisamine uus katse esimene kopeerides, ümber nimetada ja välja koodi, mis kinnitab autori NotImplementedException on oodatud erandi kommentaar.

        ```
        [TestMethod]
        //[ExpectedException(typeof(NotImplementedException))]
        public void TestMethod2()
        {
            throw new NotImplementedException();
        }
        ```

1. Märkige ruut Muuda järjekorda uue koostamine.

    ![][48]

1. Selle tõrke üksikasjade kuvamiseks testi tulemusi vaadata.

    ![][49]

    ![][50]

## <a name="next-steps"></a>Järgmised sammud
Üksus katsetamine Visual Studio meeskonnatöö teenuste kohta leiate lisateavet teemast [käivitada üksuse katsed oma koostamine](http://go.microsoft.com/fwlink/p/?LinkId=510474). Kui kasutate Git, lugege teemat [ühiskasutus Git oma kood](http://www.visualstudio.com/get-started/share-your-code-in-git-vs.aspx) ja [pidev juurutamine Azure'i rakendust Service](../app-service-web/app-service-continuous-deployment.md).  Visual Studio meeskonnatöö teenuste kohta leiate lisateavet teemast [Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkId=253861).

[0]: ./media/cloud-services-continuous-delivery-use-vso/tfs0.PNG
[1]: ./media/cloud-services-continuous-delivery-use-vso/tfs1.png
[2]: ./media/cloud-services-continuous-delivery-use-vso/tfs2.png

[5]: ./media/cloud-services-continuous-delivery-use-vso/tfs5.png
[6]: ./media/cloud-services-continuous-delivery-use-vso/tfs6.png
[7]: ./media/cloud-services-continuous-delivery-use-vso/tfs7.png
[8]: ./media/cloud-services-continuous-delivery-use-vso/tfs8.png
[9]: ./media/cloud-services-continuous-delivery-use-vso/tfs9.png
[10]: ./media/cloud-services-continuous-delivery-use-vso/tfs10.png
[11]: ./media/cloud-services-continuous-delivery-use-vso/tfs11.png
[12]: ./media/cloud-services-continuous-delivery-use-vso/tfs12.png
[13]: ./media/cloud-services-continuous-delivery-use-vso/tfs13.png
[14]: ./media/cloud-services-continuous-delivery-use-vso/tfs14.png
[15]: ./media/cloud-services-continuous-delivery-use-vso/tfs15.png
[16]: ./media/cloud-services-continuous-delivery-use-vso/tfs16.png
[17]: ./media/cloud-services-continuous-delivery-use-vso/tfs17.png
[18]: ./media/cloud-services-continuous-delivery-use-vso/tfs18.png
[19]: ./media/cloud-services-continuous-delivery-use-vso/tfs19.png
[20]: ./media/cloud-services-continuous-delivery-use-vso/tfs20.png
[21]: ./media/cloud-services-continuous-delivery-use-vso/tfs21.png
[22]: ./media/cloud-services-continuous-delivery-use-vso/tfs22.png
[23]: ./media/cloud-services-continuous-delivery-use-vso/tfs23.png
[24]: ./media/cloud-services-continuous-delivery-use-vso/tfs24.png
[25]: ./media/cloud-services-continuous-delivery-use-vso/tfs25.png
[26]: ./media/cloud-services-continuous-delivery-use-vso/tfs26.png
[27]: ./media/cloud-services-continuous-delivery-use-vso/tfs27.png
[28]: ./media/cloud-services-continuous-delivery-use-vso/tfs28.png
[29]: ./media/cloud-services-continuous-delivery-use-vso/tfs29.png
[30]: ./media/cloud-services-continuous-delivery-use-vso/tfs30.png
[31]: ./media/cloud-services-continuous-delivery-use-vso/tfs31.png
[32]: ./media/cloud-services-continuous-delivery-use-vso/tfs32.png
[33]: ./media/cloud-services-continuous-delivery-use-vso/tfs33.png
[34]: ./media/cloud-services-continuous-delivery-use-vso/tfs34.png
[35]: ./media/cloud-services-continuous-delivery-use-vso/tfs35.png
[36]: ./media/cloud-services-continuous-delivery-use-vso/tfs36.PNG
[37]: ./media/cloud-services-continuous-delivery-use-vso/tfs37.PNG
[38]: ./media/cloud-services-continuous-delivery-use-vso/AdvancedMSBuildSettings.PNG
[39]: ./media/cloud-services-continuous-delivery-use-vso/AddUnitTestProject.PNG
[40]: ./media/cloud-services-continuous-delivery-use-vso/AddProjectReferences.PNG
[41]: ./media/cloud-services-continuous-delivery-use-vso/EditBuildDefinitionForTest.PNG
[42]: ./media/cloud-services-continuous-delivery-use-vso/QueueNewBuild.PNG
[43]: ./media/cloud-services-continuous-delivery-use-vso/QueueBuildDialog.PNG
[44]: ./media/cloud-services-continuous-delivery-use-vso/BuildInTeamExplorer.PNG
[45]: ./media/cloud-services-continuous-delivery-use-vso/BuildRequestInfo.PNG
[46]: ./media/cloud-services-continuous-delivery-use-vso/BuildSucceeded.PNG
[47]: ./media/cloud-services-continuous-delivery-use-vso/TestResultsPassed.PNG
[48]: ./media/cloud-services-continuous-delivery-use-vso/CheckInChangeToMakeTestsFail.PNG
[49]: ./media/cloud-services-continuous-delivery-use-vso/TestsFailed.PNG
[50]: ./media/cloud-services-continuous-delivery-use-vso/TestsResultsFailed.PNG
