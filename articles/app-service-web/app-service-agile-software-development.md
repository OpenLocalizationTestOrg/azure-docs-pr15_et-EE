<properties
    pageTitle="Dünaamilised tarkvara arendamine Azure'i rakenduse teenusega"
    description="Saate teada, kuidas luua suure ulatusega keerukate rakendusi Azure'i rakendust Service viisil, mis toetab dünaamilised tarkvara arendamine."
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/01/2016"
    ms.author="cephalin"/>


# <a name="agile-software-development-with-azure-app-service"></a>Dünaamilised tarkvara arendamine Azure'i rakenduse teenusega #

Selles õppetükis saate teada, kuidas luua suure ulatusega keerukate rakendusi [Azure'i rakendust Service](/services/app-service/) viisil, mis toetab [dünaamilised tarkvara arendamine](https://en.wikipedia.org/wiki/Agile_software_development). See eeldab, et teate juba juurutamise [keerukate rakenduste Azure ootuspäraselt](app-service-deploy-complex-application-predictably.md).

Piirangud on teile tehnilise protsessid saab sageli takistada eduka rakendamise dünaamilised meetodite. Azure'i rakenduse teenuse funktsioone, nagu [pidev avaldamine](app-service-continuous-deployment.md), [lavastus keskkonnas](web-sites-staged-publishing.md) (slots) ja [jälgida](web-sites-monitor.md), kui hoolikas koos korraldamise ja haldamise juurutamise [Azure'i ressursihaldur](../azure-resource-manager/resource-group-overview.md), võib olla suurepärane lahendus arendajad, kes omaks dünaamilised tarkvara arendamise osa.

Järgmises tabelis on seostatud dünaamilised arendamise ja kuidas Azure'i teenuste lubab neid nõuded lühikese loendi.

| Nõue | Kuidas Azure'i võimaldab |
|---------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| -Luua iga kinnitamine<br>-Koostada automaatselt ja kiire | Kui konfigureeritud pidev juurutus, Azure'i rakendust Service võib toimida live töötab järgud arendaja haru põhjal. Iga kord, kui koodi vajutamist selle haru, on automaatselt ehitatud ja töötab reaalajas Azure.|
| -Teha järgud omas testimine | Laadi testide, web testide jne, saab kasutada Azure ressursihaldur malli abil.|
| -Teste teha klooni tootmiskeskkonda. | Azure'i ressursihaldur malle saab luua kloonid Azure tootmiskeskkonda (sh rakenduse sätted, ühenduse stringi Mallid, skaleerimist jne) testimiseks kiiresti ja ootuspäraselt.|
| -Vaadata hõlpsalt uusimas tulem | Pidev juurutamiseks Azure'i hoidla tähendab, et testida uue koodi reaalajas rakenduses kohe pärast muudatuste kinnitamiseks. |
| -Järgida peamist haru iga päev<br>-Automatiseerimine juurutamine | Pidev integratsioon tootmise rakenduse abil soovitud hoidla peamine haru juurutamine automaatselt iga Kinnita/ühendamine peamine haru tootmisele. |

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="what-you-will-do"></a>Mida te teete ##

Saate juhendab tüüpilised arendaja testi-etapi-tekitamiseks töövoo avaldamiseks [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) valimi rakendus, mis koosneb kahest [veebirakenduste](/services/app-service/web/), üks on frontend (FE) ja teine on veebi-API taustväärtus (BE) ja [SQL-andmebaasi](/services/sql-database/)muudatusi. Töötab koos juurutamise arhitektuur allpool näidatud:

![](./media/app-service-agile-software-development/what-1-architecture.png)

Kasutusele võtta sõna pilti:

-   Juurutamise arhitektuur on jagatud kolme erinevate keskkonnas (või [Ressursirühma](../azure-resource-manager/resource-group-overview.md) Azure) iga oma [rakenduse teenusleping](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md), [skaleerimist](web-sites-scale.md) sätted ja SQL-andmebaasi. 
-   Iga keskkonna saate hallata eraldi. Ta saab isegi muu tellimuste olemas.
-   Lavastus ja tootmise on rakendada sama rakenduse teenuse rakenduse kaks Ava. Juhtslaidi haru on pidev integreerimine vahekausta pesa setup.
-   Kui soovitud juhtslaidi haru kinnitamine on kinnitatud vahekausta pesa (koos andmeid), on kinnitatud lavastus rakenduse vahetasin selle tootmise pesa [pole tööseisakute abil](web-sites-staged-publishing.md)sisse.

Tootmise ja lavastus keskkond on määratletud malli veebisaidil [ * &lt;repository_root >*/ARMTemplates/ProdandStage.json](https://github.com/azure-appservice-samples/ToDoApp/blob/master/ARMTemplates/ProdAndStage.json).

Arendaja ja testige keskkonnas on määratletud malli veebisaidil [ * &lt;repository_root >*/ARMTemplates/Dev.json](https://github.com/azure-appservice-samples/ToDoApp/blob/master/ARMTemplates/Dev.json).

Samuti saate tüüpiline hargnemisloogika strateegia, koodiga teisaldada arendaja kontorist kuni testi haru, seejärel soovitud juhtslaidi haru (teisaldamine kvaliteedi, nii et rääkida).

![](./media/app-service-agile-software-development/what-2-branches.png) 

## <a name="what-you-will-need"></a>Mida on vaja ##

-   Azure'i konto
-   [GitHub](https://github.com/) konto
-   Git Shell (paigaldatud [GitHub Windowsi jaoks](https://windows.github.com/)) – nii saate käivitada nii Git ja PowerShelli käske sama seansi 
-   Viimane [Azure PowerShelli](https://github.com/Azure/azure-powershell/releases/download/0.9.4-June2015/azure-powershell.0.9.4.msi) bittide
-   Põhiteadmisi järgmist:
    -   [Azure'i ressursihaldur](../azure-resource-manager/resource-group-overview.md) malli juurutamise (vt [Deploy keerukate rakenduses ootuspäraselt Azure](app-service-deploy-complex-application-predictably.md))
    -   [Git](http://git-scm.com/documentation)
    -   [PowerShelli](https://technet.microsoft.com/library/bb978526.aspx)

> [AZURE.NOTE] Peate selle õpetuse lõpuleviimiseks Azure'i konto.
> + Saate [avada Azure'i konto tasuta](/pricing/free-trial/) – saate krediiti abil saate proovida makstud Azure'i teenuste ja isegi juhul, kui neid kasutatakse hoiate konto ja kasutamine tasuta Azure teenuseid, näiteks Web Apps.
> + Saate [aktiveerida Visual Studio abonendi kasu](/pricing/member-offers/msdn-benefits-details/) – teie Visual Studio tellimuse annab teile krediiti iga kuu makstud Azure'i teenuste kasutatavad.
>
> Kui soovite alustada Azure'i rakendust Service enne Azure'i konto kasutajaks, minge [Proovige rakenduse teenus](http://go.microsoft.com/fwlink/?LinkId=523751), kus saate kohe luua lühiajaline starter web app rakenduse teenus. Nõutav; krediitkaardid kohustusi.

## <a name="set-up-your-production-environment"></a>Tootmise keskkonna häälestamine ##

>[AZURE.NOTE] Selles õpetuses kasutatud skript automaatselt konfigureerida pidev avaldamise kaudu oma GitHub hoidla. Selleks, et GitHub mandaat on juba salvestatud Azure, muidu skriptitud juurutamise nurjub, kui proovite veebirakenduste andmeallika juhtelemendi sätete konfigureerimine. 
>
>Azure GitHub mandaadi talletamiseks luua veebirakenduse [Azure portaali](https://portal.azure.com/) ja [konfigureerida GitHub juurutuse](app-service-continuous-deployment.md). Ainult peate selleks üks kord. 

Tüüpilised DevOps stsenaariumi puhul on rakendus, mis töötab reaalajas Azure ja soovite seda muuta pidev avaldamise kaudu. Selle stsenaariumi korral peate väljatöötatud testitud ja juurutada tootmiskeskkonda kasutatud malli. Kas olete see üles selle jaotise.

1.  Saate luua oma kahvel [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) hoidla. Toidulauani loomise kohta leiate teavet teemast [kahvel Repo](https://help.github.com/articles/fork-a-repo/). Kui teie kahvel on loodud, näete brauseris.
 
    ![](./media/app-service-agile-software-development/production-1-private-repo.png)

2.  Avage Git Shell seanss. Kui teil pole veel Git Shell, installige [Windowsi GitHub](https://windows.github.com/) .

3.  Luua kohaliku klooni oma kahvel, käitades järgmine käsk:

        git clone https://github.com/<your_fork>/ToDoApp.git 

4.  Kui olete oma kohaliku klooni, liikuge * &lt;repository_root >*\ARMTemplates ja Käivita selle deploy.ps1 skript järgmiselt:

        .\deploy.ps1 –RepoUrl https://github.com/<your_fork>/todoapp.git

4.  Vastava viiba kuvamisel tippige soovitud kasutajanimi ja parool andmebaasi juurdepääsu.

    Peaksite nägema ettevalmistamise edenemist erinevate Azure ressursse. Kui juurutamise lõpule jõudnud, kuvatakse skripti käivitage rakendus brauseris ja teile sõbralik piiks.

    ![](./media/app-service-agile-software-development/production-2-app-in-browser.png)
 
    >[AZURE.TIP] Heitke pilk * &lt;repository_root >*\ARMTemplates\Deploy.ps1, et näha, kuidas see tekitab ressursse kordumatu ID. Sama lähenemisviisi abil saate luua kloonid sama juurutamise ilma muretsema vastuoluliste ressursside nimed.
 
6.  Tagasi Git Shell seansi käivitamine:

        .\swap –Name ToDoApp<unique_string>master

    ![](./media/app-service-agile-software-development/production-4-swap.png)

7.  Kui skript lõpetab, minge tagasi liikuge sirvides soovitud frontend aadressi (http://ToDoApp*&lt;unique_string >*master.azurewebsites.net/) kuvamiseks valmistamisel rakendus.
 
5.  [Azure portaali](https://portal.azure.com/) sisse logida ja heita pilk, mis on loodud.

    Peaks oskama vt sama ressursi jaotises üks kahest veebirakenduste soovitud `Api` nime järelliide. Kui vaatate Ressursivaade rühma, kuvatakse ka SQL-andmebaasi ja server, rakenduse teenusleping ja lavastus slots web apps. Sirvige erinevate ressursside ja võrdleb neid * &lt;repository_root >*\ARMTemplates\ProdAndStage.json näha, kuidas need on konfigureeritud mall.

    ![](./media/app-service-agile-software-development/production-3-resource-group-view.png)

Nüüd olete seadistanud tootmiskeskkonda. Järgmisena saate võrgukoosolekuga rakenduse uus uuendada.

## <a name="create-dev-and-test-branches"></a>Arendaja loomine ja testimine harude ##

Nüüd, kui teil on keerukas rakendus töötab valmistamisel Azure, teha rakenduse dünaamilised meetodite kohaselt värskendust. Selles jaotises luua selle arendaja ja testida harude, mida on vaja teha vajalikud värskendused.

1.  Funktsiooni testimiskeskkonnas esmalt looma. Seansi Git Shell käivitage uus haru, mida nimetatakse **NewUpdate**keskkonna loomiseks järgmised käsud. 

        git checkout -b NewUpdate
        git push origin NewUpdate 
        .\deploy.ps1 -TemplateFile .\Dev.json -RepoUrl https://github.com/<your_fork>/ToDoApp.git -Branch NewUpdate

1.  Vastava viiba kuvamisel tippige soovitud kasutajanimi ja parool andmebaasi juurdepääsu. 

    Kui juurutamise lõpule jõudnud, kuvatakse skripti käivitage rakendus brauseris ja teile sõbralik piiks. Ja nagu et nüüd on teil uus haru koos oma testimiskeskkonnas. Mõne hetke läbi vaadata selle testimiskeskkonnas kohta paari asja:

    -   Saate luua mis tahes Azure'i tellimus. Mida tähendab, et tootmiskeskkonda saate hallata eraldi testimiskeskkonnas.
    -   Testimiskeskkonnas töötab reaalajas Azure.
    -   Teie keskkond on identne tootmiskeskkonda, välja arvatud lavastus slots ja skaleerimise sätted. Seda saate teada, kuna need on ProdandStage.json ja Dev.json ainult erinevused.
    -   Saate hallata oma testimiskeskkonnas oma rakenduse teenuse lepingut, hind taseme (nt **tasuta**).
    -   See testimiskeskkonnas kustutamine on sama lihtne kui kustutamine ressursirühma. Saate teada, kuidas teha seda [hiljem](#delete).

2.  Avage loomiseks arendaja haru käivitades järgmised käsud:

        git checkout -b Dev
        git push origin Dev
        .\deploy.ps1 -TemplateFile .\Dev.json -RepoUrl https://github.com/<your_fork>/ToDoApp.git -Branch Dev

3.  Vastava viiba kuvamisel tippige soovitud kasutajanimi ja parool andmebaasi juurdepääsu. 

    Mõne hetke läbi vaadata selle arenduskeskkond kohta paari asja: 

    -   Teie arenduskeskkond on identne testi keskkonna konfiguratsioon, kuna juurutamist sama malli abil.
    -   Iga arenduskeskkond saab luua arendaja oma Azure'i tellimus, jättes testimiskeskkonnas hallatakse eraldi.
    -   Reaalajas Azure töötab teie arenduskeskkond.
    -   Funktsiooni arenduskeskkond kustutamine on sama lihtne kui kustutamine ressursirühma. Saate teada, kuidas teha seda [hiljem](#delete).

>[AZURE.NOTE] Kui teil on mitu arendajate kallal uue update, üks neist saate hõlpsalt luua haru ja sihtotstarbeline arenduskeskkond, tehes järgmist:
>
>1. Luua oma kahvel hoidla GitHub (vt [kahvel Repo](https://help.github.com/articles/fork-a-repo/)).
>2. Klooni kahvliga oma kohalikus arvutis
>3. Käivitage luua oma arendaja haru ja keskkonnas sama käsud.

Kui olete lõpetanud, peaks toidulauani GitHub on kolm harude.

![](./media/app-service-agile-software-development/test-1-github-view.png)

Ja peaks olema kuus veebirakenduste (kolm komplekti kahe) eraldi ressursi kolme rühma:

![](./media/app-service-agile-software-development/test-2-all-webapps.png)
 
>[AZURE.NOTE] Pange tähele, et ProdandStage.json määrab tootmiskeskkonda kasutamiseks on **Standardne** hinnad taseme kohta, mis on asjakohane skaleeritavus taotluse.

## <a name="build-and-test-every-commit"></a>Koostamine ja testimine iga kinnitamine ##

Mallifailid ProdAndStage.json ja Dev.json juba Määrake allikas parameetrid, mis vaikesättena häälestab pidev avaldamise web app. Seetõttu, iga haru GitHub Kinnita käivitab mõne automaatse juurutamise Azure selle kontorist. Vaatame, kuidas häälestust töötab nüüd.

1.  Veenduge, et olete arendaja kontoris kohaliku hoidla. Selleks, käivitage järgmine käsk Git Shell:

        git checkout Dev

2.  Lihtne muudatuste tegemine rakenduse UI kiht, muutes koodi [alglaaduri](http://getbootstrap.com/components/) loendeid. Avatud * &lt;repository_root >*\src\MultiChannelToDo.Web\index.cshtml ja teete muudatuse esiletõstetud allpool:

    ![](./media/app-service-agile-software-development/commit-1-changes.png)

    >[AZURE.NOTE] Kui te ei saa ülalolevat pilti: 
    >
    >- Muutke Real 18, `check-list` et `list-group`.
    >- Rida 19, muutke `class="check-list-item"` et `class="list-group-item"`.

3.  Muudatuse salvestamiseks. Käivitage uuesti Git Shellis järgmised käsud:

        cd <repository_root>
        git add .
        git commit -m "changed to bootstrap style"
        git push origin Dev
 
    Need käsud git sarnanevad "kontrollimine koodi" mõnes muus andmeallika juhtelemendi süsteemis nagu TFS. Kui käivitate `git push`, käivitab uue kinnitamine on automaatne koodi tõuketeatised Azure'i, mis luuakse siis rakenduse arendaja keskkonnas muudatuse kajastamiseks.

4.  Veenduge, et ilmnenud on see teie arenduskeskkond kood PTT, minge oma arenduskeskkond web appi blade ja vaadata **juurutamise** osa. Peaks oskama oma uusima Kinnita teade seal.

    ![](./media/app-service-agile-software-development/commit-2-deployed.png)

5.  Seal, klõpsake **Sirvi** Azure reaalajas rakenduse uus muudatuste nägemiseks.

    ![](./media/app-service-agile-software-development/commit-3-webapp-in-browser.png)

    See on päris mõnevõrra muudatuse taotlus. Mitu korda keerukate veebirakenduse uue muudatused on siiski ootamatuid ja soovimatute küljel efektid. Ei saaks seda hõlpsalt kontrollimiseks iga Kinnita reaalajas järgud võimaldab jaole juba järgmiste probleemide korral enne klientide neid vaadata.

Nüüd, peaks olema rahul, et arendaja **NewUpdate** projekt, siis saab hõlpsalt luua mõne arenduskeskkond endale, siis koostada iga kinnitamine ja testimine iga Koosta mõistmine.

## <a name="merge-code-into-test-environment"></a>Koodi ühendamine testimiskeskkonnas ##

Kui olete valmis oma koodi push arendaja kontorist kuni NewUpdate haru, see on standardne git protsessi:

1.  Mis tahes uue võtab, et NewUpdate arendaja haru GitHub, nt võtab loodud muude arendajad ühendada. Mis tahes uue Kinnita github kood push käivitamine ja koostada arendaja keskkonnas. Saate seejärel tagada, et teie kood arendaja haru töötab jätkuvalt uusima bittide NewUpdate kontorist.

2.  Teie uus võtab arendaja kontorist NewUpdate haru github ühendada. See toiming käivitab kood tõuketeatised ja Koosta testi keskkonnas. 

Pange tähele uuesti, et kuna pideva on juba häälestamise ja nende git harude, te ei pea teisi toiminguid, nt integreerimine järgud. Lihtsalt tuleb teha standard andmeallika juhtelemendi tavade git abil ja Azure teha Koosta protsesside teile.

Nüüd vaatame push oma koodi **NewUpdate** haru. Käivitage Git Shellis järgmised käsud:

    git checkout NewUpdate
    git pull origin NewUpdate
    git merge Dev
    git push origin NewUpdate

See on õige! 

Minge web appi tera testimiskeskkonnas näha oma uue kinnitamine (liidetakse NewUpdate haru) nüüd lükata testi keskkonna jaoks. Klõpsake nuppu **Sirvi** laadi muutmine praegu töötab reaalajas Azure.

## <a name="deploy-update-to-production"></a>Värskenduse tootmise juurutamine ##

Lükkamine lavastus ja tootmise kood tuleks tundub ei erine, mida olete juba teinud kui olete lükanud kood testi keskkonnas. See on nii väga lihtne. 

Käivitage Git Shellis järgmised käsud:

    git checkout master
    git pull origin master
    git merge NewUpdate
    git push origin master

Pidage meeles, et nii lavastus ja tootmise keskkond on häälestus ProdandStage.json põhjal, teie uus kood on lükata **lavastus** pesa ja seal töötab. Nii, et kui liigute vahekausta pesa URL-i, kuvatakse uus kood ei tööta. Selleks käivitada soovitud `Show-AzureWebsite` Git Shellis cmdlet-käsu.

    Show-AzureWebsite -Name ToDoApp<unique_string>master -Slot Staging
 
Ja nüüd pärast värskenduse vahekausta pesa kinnitamist teha ainus on tootmisse või vahetada. Lihtsalt käivitage Git Shellis järgmised käsud:

    cd <repository_root>\ARMTemplates
    .\swap.ps1 -Name ToDoApp<unique_string>master

Palju õnne! Nüüd olete avaldatud uus värskendus veebi rakendus. Mida rohkem on, et olete lihtsalt teha seda hõlpsalt luua arendaja ja testida keskkondade ja koostamise ja testimine iga kinnitamine. Need on väga oluline koosteüksuste dünaamilised tarkvara arendamiseks.

<a name="delete"></a>
## <a name="delete-dev-and-test-enviroments"></a>Arendaja kustutamine ja keskkondade testimine ##

Kuna teil on teadlikult architected oma arendaja ja testige keskkonnas olema iseseisev ressursi rühmad, on väga lihtne kustutage need. Kustutada loodud selles õpetuses GitHub harude ja Azure esemeid, vaid Git Shellis käivitage järgmine käsk:

    git branch -d Dev
    git push origin :Dev
    git branch -d NewUpdate
    git push origin :NewUpdate
    Remove-AzureRmResourceGroup -Name ToDoApp<unique_string>dev-group -Force -Verbose
    Remove-AzureRmResourceGroup -Name ToDoApp<unique_string>newupdate-group -Force -Verbose

## <a name="summary"></a>Kokkuvõte ##

Dünaamilised tarkvara on must-olla paljud ettevõtted, kes soovivad Azure'i nende rakenduste platvormi vastu. Selles õpetuses te teada, kuidas luua ja pisar alla täpsed koopiad või koopiad tootmiskeskkonda lähedal lihtsalt keerukate taotluste. Te ka teada, kuidas see võimalus luua arendamise protsessi, mida saate koostada ja testida iga ühe Kinnita Azure võimendada. Selle õpetuse loodetavasti teile näidanud on kõige paremini kasutamist Azure'i rakendust Service ja Azure ressursihaldur koos DevOps lahenduse, mis näeb dünaamilised meetodite loomiseks. Järgmisena saate koostada klõpsake selle stsenaariumi täites DevOps täiustatud viise, nt [testimine valmistamisel](app-service-web-test-in-production-get-start.md). Testimine sisse-tekitamiseks stsenaarium, leiate teemast [Flighting juurutamise (beeta) teenuses Azure rakendus](app-service-web-test-in-production-controlled-test-flight.md).

## <a name="more-resources"></a>Veel ressursse ##

-   [Keerukate rakenduses ootuspäraselt Azure juurutamine](app-service-deploy-complex-application-predictably.md)
-   [Dünaamilised arengu praktikas: nõuandeid ja näpunäiteid ajakohastatud arengutsükli](http://channel9.msdn.com/Events/Ignite/2015/BRK3707)
-   [Täpsem strateegiad Azure'i veebirakenduste ressursihaldur mallide kasutamine](http://channel9.msdn.com/Events/Build/2015/2-620)
-   [Azure'i ressursihaldur mallide koostamine](../resource-group-authoring-templates.md)
-   [JSONLint - JSON süntaksi](http://jsonlint.com/)
-   [ARMClient – häälestamine GitHub avaldamise saidile](https://github.com/projectKudu/ARMClient/wiki/Setup-GitHub-publishing-to-Site)
-   [Git harude – lihtsa harude ja ühendamine](http://www.git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)
-   [David Ebbo ajaveeb](http://blog.davidebbo.com/)
-   [Azure'i PowerShelli](../powershell-install-configure.md)
-   [Azure'i platvormidel käsurea tööriistad](../xplat-cli-install.md)
-   [Kasutajate loomine ja redigeerimine Azure AD](https://msdn.microsoft.com/library/azure/hh967632.aspx#BKMK_1)
-   [Projekti Kudu viki](https://github.com/projectkudu/kudu/wiki)
