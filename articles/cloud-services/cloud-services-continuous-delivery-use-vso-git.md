<properties
    pageTitle="Pidev kohaletoimetamise Git ja Visual Studio Team Services Azure | Microsoft Azure'i"
    description="Saate teada, kuidas oma Visual Studio Team Services meeskond projektid Git abil automaatselt koostamine ja võtta kasutusele veebirakenduse funktsiooni Azure'i rakendust Service või cloud services konfigureerimine."
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

# <a name="continuous-delivery-to-azure-using-visual-studio-team-services-and-git"></a>Pidev kohaletoimetamise Azure Visual Studio Team Services ja Git abil

Saate majutada oma lähtekoodi jaoks Git hoidla ja automaatselt koostamine ja Azure veebirakenduste kasutuselevõtuks või pilveteenused alati vajutada soovitud Kinnita hoidla Visual Studio Team Services meeskond projektid.

Peate Visual Studio 2013 ja Azure SDK installitud. Kui teil pole veel Visual Studio 2013 alla laadida, valides **tasuta alustamine** veebisaidil [www.visualstudio.com](http://www.visualstudio.com)link. Installige Azure'i SDK [siin](http://go.microsoft.com/fwlink/?LinkId=239540).


> [AZURE.NOTE]Peate selle õpetuse lõpuleviimiseks konto Visual Studio Team Services: saate [avada Visual Studio Team Services konto tasuta](http://go.microsoft.com/fwlink/p/?LinkId=512979).

Pilveteenus automaatselt luua ja juurutada Azure Visual Studio Team Services abil häälestada, tehke järgmist.

## <a name="1-create-a-git-repository"></a>1: Git hoidla loomine

1. Kui te pole veel Visual Studio Team Services konto, saate hankida [siin](http://go.microsoft.com/fwlink/?LinkId=397665). Oma projekti loomisel valida Git arvutisüsteemi Juhtelemendi allikas. Järgige oma projekti Visual Studio ühenduse.

2. Valige **Meeskonnatöö Exploreri** **klooni selle hoidla** link.

    ![][3]

3. Määrake kohaliku eksemplari asukoht ja seejärel valige nupp **klooni** .

## <a name="2-create-a-project-and-commit-it-to-the-repository"></a>2: projekti loomine ja kinnitage see hoidla

1. Valige **Meeskonnatöö Exploreri** **lahenduste** jaotises **Uus** link kohaliku hoidla uue projekti loomiseks.

    ![][4]

2. Ülevaate juhiseid järgides saate juurutada web Appi kui ka mõnda pilveteenusesse (Azure'i rakendus). Azure'i pilveteenusesse uue projekti või ASP.net-i MVC uue projekti loomine. Veenduge, veenduge, et projekti eesmärgid .NET Framework 4 või uuem versioon. Kui loote pilvepõhise teenuse projekti, lisada ASP.net-i MVC web rolli ja töötaja roll.
Kui soovite luua veebirakenduse, **ASP.net-i veebirakenduse** projekti malli valimine ja seejärel valige **MVC**. Lisateavet leiate [Azure'i rakenduse teenuses web ASP.net-i rakenduse loomine](../app-service-web/web-sites-dotnet-get-started.md) .

3. Lahendus kiirmenüü avamine ja valige **Kinnita**.

    ![][7]

4. Kui olete kasutanud Git Visual Studio Team Services esimest korda, peate turvalisem git osa teabest. **Meeskonnatöö Exploreri**alal **Ootel muudatusi** sisestage oma kasutajanimi ja e-posti aadress. Sisestage kommentaari kinnitus ja seejärel klõpsake nuppu **Kinnita** .

    ![][8]

5. Pange tähele suvandid kaasamiseks või välistamiseks kindlaid muudatusi, kui märgite ruudu. Kui soovitud muudatused on välja jäetud, siis valige **Kaasata kõik**.

6. Nüüd olete kinnitatud muudatusi, hoidla kohaliku eksemplari. Järgmiseks nende muudatuste sünkroonimine serveriga, valides **sünkroonimine** link

## <a name="3-connect-the-project-to-azure"></a>3: Azure'i projekti ühendamine

1. Nüüd, kui teil on Git hoidla Visual Studio meeskonnatöö teenuseid koos mõne lähtekoodi ei, olete valmis oma git hoidla ühenduse Azure'i.  [Azure'i klassikaline portaalis](http://go.microsoft.com/fwlink/?LinkID=213885)valige pilvepõhise teenuse või web appi või looge uus, valides selle + ikooni vasakus allnurgas ja valides **Pilveteenuses** või **Web App** ja seejärel **Kiiresti luua**.

    ![][9]

3. Puhul pilveteenuste, valige **häälestamine avaldamise Visual Studio meeskonnatöö teenustega** link. Valige veebirakendustes **häälestamine juurutamine andmeallika juhtelemendi** link.

    ![][10]

2. Viisardi, tippige tekstiväljale oma Visual Studio Team Services konto nimi ja valige link **Lubada kohe** . Teil palutakse sisse logida.

    ![][11]

3. **Ühenduse taotlemine** hüpikakende dialoogiboksis nuppu **Aktsepteeri** lubada Azure konfigureerimine oma projekti Visual Studio meeskonnatöö teenused.

    ![][12]

4. Pärast autoriseerimine õnnestub, kuvatakse ripploendi, mis sisaldab teie Visual Studio Team Services meeskond projektid.  Valige eelmiste juhiste järgi loodud projekti nimi ja valige viisardinupp märke.

    ![][13]

    Järgmine kord, kui vajutada soovitud Kinnita oma andmebaasi Visual Studio Team Services koostamine ja juurutamine projekti Azure.

## <a name="4-trigger-a-rebuild-and-redeploy-your-project"></a>4: käivitamine taastada ja projekti ümberkorraldamine

1. Visual Studio, faili avada ja muuta. Näiteks muuta faili `_Layout.cshtml` jaotises vaated\\rolli MVC web jagatud kaust.

    ![][17]

2. Saidi jaluse teksti redigeerida, ja salvestage fail.

    ![][18]

3. **Solution Exploreris**lahenduse sõlm, projekti sõlm või muudetud faili kiirmenüü avamine ja klõpsake nuppu **Kinnita**.

4. Tippige kommentaari ja valige **Kinnita**.

    ![][20]

5. Valige **Sünkrooni** link.

    ![][38]

6. Valige **tõuketeatised** link push oma Kinnita hoidlasse Visual Studio meeskonnatöö teenused. (Nupp **Sünkrooni** saate kasutada ka oma võtab kopeerimiseks hoidla. Erinevus on, et **sünkroonimine** tõmbab viimaste muudatuste hoidla.)

    ![][39]

7. Klõpsake nuppu **Home** **Team Explorer** avalehele naasmiseks.

    ![][21]

8. Valige **koostab** parandused edenemise kuvamiseks.

    ![][22]

    **Meeskonnatöö Explorer** näitab, et ehitada on käivitanud teie sisseregistreerimise.

    ![][23]

9. Koosta edenedes üksikasjalikku Logi kuvamiseks topeltklõpsake nime koostamine on pooleli.

10. Koostamine on pooleli, tutvuge koostamine määratlus, kui kasutasite viisardi linkimiseks Azure'i loodud.  Koosta määratluse kiirmenüü avamine ja valige **Redigeeri koostada määratlus**.

    ![][25]

11. **Päästik** menüüs näete, et Koosta määratluse on vaikimisi koostamiseks iga sisse. (Pilveteenus Visual Studio Team Services koostab ja juurutamine juhtslaidi haru lavastus keskkonna automaatselt. Tuleb teha käsitsi sammu juurutada saidil. Web app, mis ei sisalda seadistamiskeskkonnast selle juurutamine juhtslaidi haru otse saidil.

    ![][26]

1. Klõpsake vahekaarti **protsess** saate vaadata juurutamine keskkonnas on seatud pilve teenuse või web rakenduse nimi.

    ![][27]

1. Kui soovite muu väärtus kui vaikeväärtused, määrake atribuutide väärtused. Azure'i avaldamise atribuutide on jaotises **juurutamise** ja võib ka peate määrama MSBuild parameetrid. Näiteks pilv teenus projekti määramiseks teenuse konfigureerimine "Pilve" peale määratud MSbuild parameetrid `/p:TargetProfile=[YourProfile]` kus *[YourProfile]* vastab teenuse konfiguratsiooni faili nime nagu ServiceConfiguration. *YourProfile*.cscfg.

    Järgmine tabel sisaldab saadaval atribuudid jaotises **juurutamise** .

  	|Atribuut|Vaikeväärtus|
  	|---|---|
  	|Luba ebausaldusväärsete serdid|Kui väärtus on false, peab SSL-sertide allkirjastatud juurorgan.|
  	|Luba täiendamine|Võimaldab juurutamise värskendada olemasoleva juurutuse asemel luua uue eksemplari. Säilitab IP-aadress.|
  	|Kustutamine|Kui väärtus on true, kirjutada olemasoleva seostamata juurutuse (täiendamine on lubatud).|
  	|Tee juurutamise sätted|Web app, suhtes repo juurkausta .pubxml faili tee. Ignoreeritud puhul pilveteenuste.|
  	|SharePointi juurutamise keskkonnas|Sama, mis teenuse nimi.|
  	|Azure'i juurutamise keskkonnas|Web appi või pilvepõhises teenuse nimi.|

1. Sel ajal, peaks teie koostamine lõpule viidud.

    ![][28]

1. Kui topeltklõpsate Koosta nime, kuvatakse Visual Studio **Build Kokkuvõte**, sh mis tahes seotud ühiku test projektide testi tulemused.

    ![][29]

1. [Azure'i klassikaline portaali](http://go.microsoft.com/fwlink/?LinkID=213885), saate vaadata seotud juurutamise menüü **juurutuste** lavastus keskkonna valimisel.

    ![][30]

1.  Liikuge sirvides oma veebisaidi URL. Web appi, valige lihtsalt nuppu **Sirvi** portaalis. Pilveteenus valida URL-i **Kiirülevaate lühidalt** osas **armatuurlaua** leht, mis näitab lavastus keskkonnas.

    Pidev integratsioon puhul pilveteenuste kasutuselevõttu on avaldatud lavastus keskkonna vaikimisi. Saate muuta seda, seades **Alternatiivse pilvepõhise teenuse keskkonna** atribuuti **tootmisele**. Siin on saidi URL-i sisaldavasse on pilveteenuses Armatuurlaua leht.

    ![][31]

    Esile töötava saidi avatakse veebibrauseri uuel vahekaardil.

    ![][32]

1.  Kui teete teiste muudatuste projekti, saate käivitada veel koostab ja teil võib mitme juurutuste. Viimati aktiivne märgitud.

    ![][33]

## <a name="5-redeploy-an-earlier-build"></a>5: varasema ümberkorraldamine

See toiming on valikuline. Azure'i klassikaline portaalis, valida mõne varasema juurutamise ja valige tagasikerimine saidi mõne varasema sisse-ja **Juurutage uuesti** . Pange tähele, et see käivitab uue Koosta TFS ja uue kirje loomine teie juurutamise ajalugu.

![][34]

## <a name="6-change-the-production-deployment"></a>6: tootmise juurutamise muutmine

Kui olete valmis, saate reklaamida lavastus keskkonna tootmiskeskkonda, valides **Vaheta** Azure klassikaline portaalis. Äsja juurutatud lavastus keskkond on esiletõstetud tootmisele ja eelmise tootmiskeskkonda, kui muutub lavastus keskkonnas. Aktiivse juurutamise võivad olla erinevad ja lavastus keskkonnas, kuid juurutamise ajalugu tehtud järgud on samad sõltumata keskkonnas.

![][35]

## <a name="7-deploy-from-a-working-branch"></a>7: kontorist töötamise juurutamine.

Git kasutamisel saate tavaliselt muuta töötamise kontoris ja integreerida juhtslaidi haru, kui teie arengu jõuab lõpule jõudnud olekus. Projekti väljatöötamise, käigus peaksite luua ja juurutada töötamise haru Azure.

1. **Meeskonnatöö Explorer**, klõpsake nuppu **Avaleht** ja seejärel valige nupp **harude** .

    ![][40]

2. Valige **Uus haru** link.

    ![][41]

3. Sisestage nimi haru, näiteks "töö," ja valige **Haru loomine**. See loob uue kohaliku haru.

    ![][42]

4. Avaldamine on kontoris. Valige **avaldamata harude**haru nime ja valige käsk **Avalda**.

    ![][44]

6. Vaikimisi käivitada ainult juhtslaidi haru muudatuste pidev koostamine. Pidev koostamine töötamise haru jaoks häälestada, valige lehe **koostab** **Team Explorer**ja valige **Koostada definitsiooni redigeerimine**.

7. Avage vahekaart **Andmeallika sätted** . Valige jaotises **jälgitud haru pidev integratsioon ja koostamine**, **uue rea lisamiseks klõpsake siin**.

    ![][47]

8. Määrake haru, mis on loodud, näiteks töö/refs/manustama.

    ![][48]

9. Muudatuste tegemine kood, muudetud faili kiirmenüü avamine ja klõpsake nuppu **Kinnita**.

    ![][43]

10. Valige link **Sünkroonimata võtab** ja valige nupp **Sünkrooni** või **Push** muudatused kopeerimiseks töötamise haru Visual Studio meeskonnatöö teenuste koopia link.

    ![][45]

11. Liikuge **koostab** vaade ja töötamise haru käivitanud lihtsalt sai koostamine otsimine.

## <a name="next-steps"></a>Järgmised sammud

Veel näpunäiteid Visual Studio meeskonnatöö teenustega Git kasutamise kohta leiate lisateavet teemast [arendada ja jagada oma kood Visual Studio abil Git](http://www.visualstudio.com/get-started/share-your-code-in-git-vs.aspx) ja Git hoidla, mida haldab Visual Studio Team Services avaldada Azure kasutamise kohta leiate teemast [Pidev juurutamise Azure'i rakenduse teenusega](../app-service-web/app-service-continuous-deployment.md). Visual Studio meeskonnatöö teenuste kohta leiate lisateavet teemast [Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkId=253861).

[0]: ./media/cloud-services-continuous-delivery-use-vso/tfs0.PNG
[1]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateTeamProjectInGit.PNG
[2]: ./media/cloud-services-continuous-delivery-use-vso/tfs2.png
[3]: ./media/cloud-services-continuous-delivery-use-vso-git/CloneThisRepository.PNG
[4]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateNewSolutionInClonedRepo.PNG
[7]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitMenuItem.PNG
[8]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitAChange2.PNG
[9]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateCloudService.PNG
[10]: ./media/cloud-services-continuous-delivery-use-vso-git/SetUpPublishingWithVSO.PNG
[11]: ./media/cloud-services-continuous-delivery-use-vso-git/AuthorizeConnection.PNG
[12]: ./media/cloud-services-continuous-delivery-use-vso-git/ConnectionRequest.PNG
[13]: ./media/cloud-services-continuous-delivery-use-vso-git/ChooseARepo3.PNG
[14]: ./media/cloud-services-continuous-delivery-use-vso/tfs14.png
[15]: ./media/cloud-services-continuous-delivery-use-vso/tfs15.png
[16]: ./media/cloud-services-continuous-delivery-use-vso/tfs16.png
[17]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs17.png
[18]: ./media/cloud-services-continuous-delivery-use-vso-git/MakeACodeChange.PNG
[20]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitAChange2.PNG
[21]: ./media/cloud-services-continuous-delivery-use-vso-git/TeamExplorerHome.png
[22]: ./media/cloud-services-continuous-delivery-use-vso-git/TeamExplorerBuilds.PNG
[23]: ./media/cloud-services-continuous-delivery-use-vso-git/BuildInQueue.png
[24]: ./media/cloud-services-continuous-delivery-use-vso/tfs24.png
[25]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs25.png
[26]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs26.png
[27]: ./media/cloud-services-continuous-delivery-use-vso-git/ProcessTab.PNG
[28]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs28.png
[29]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs29.png
[30]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs30.png
[31]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs31.png
[32]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs32.png
[33]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs33.png
[34]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs34.png
[35]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs35.png
[36]: ./media/cloud-services-continuous-delivery-use-vso/tfs36.PNG
[37]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateANewAccount.PNG
[38]: ./media/cloud-services-continuous-delivery-use-vso-git/SyncChanges2.PNG
[39]: ./media/cloud-services-continuous-delivery-use-vso-git/PushCurrentBranch.PNG
[40]: ./media/cloud-services-continuous-delivery-use-vso-git/BranchesInTeamExplorer.PNG
[41]: ./media/cloud-services-continuous-delivery-use-vso-git/NewBranch.PNG
[42]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateBranch.PNG
[43]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitAChange2.PNG
[44]: ./media/cloud-services-continuous-delivery-use-vso-git/PublishBranch.PNG
[45]: ./media/cloud-services-continuous-delivery-use-vso-git/SyncChanges2.PNG
[47]: ./media/cloud-services-continuous-delivery-use-vso-git/SourceSettingsPage.PNG
[48]: ./media/cloud-services-continuous-delivery-use-vso-git/IncludeWorkingBranch.PNG
