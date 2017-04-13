<properties 
    pageTitle="Azure'i WebJobs dokumentatsiooni ressursid" 
    description="Soovitatav õppida, kuidas kasutada Azure WebJobs ja Azure WebJobs SDK ressursid." 
    services="app-service" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/28/2016" 
    ms.author="tdykstra"/>

# <a name="azure-webjobs-documentation-resources"></a>Azure'i WebJobs dokumentatsiooni ressursid

## <a name="overview"></a>Ülevaade

See teema sisaldab linke dokumentatsiooni ressursid Azure WebJobs ja Azure WebJobs SDK kasutamise kohta. Azure'i WebJobs pakkuda lihtne viis skriptide või programmide käivitamise taust protsessid [rakenduse teenuse web appi, API rakenduse või mobiilirakenduses](../app-service/app-service-value-prop-what-is.md)kontekstis. Saate üles laadida ja käivitada cmd, nagu näiteks täitmisfail parima võimaliku tehnika, exe (.NET) ps1, sh, php, py, js ja jar. Järgmiste programmide osas käivitada WebJobs intervalliga (cron) või pidevalt.

[WebJobs SDK](websites-webjobs-resources.md) eesmärk lihtsustada koodi kirjutamise levinud toiminguid, mida on WebJob saate teha, näiteks pilditöötluse, järjekorda töötlemine RSS-i koondamine, faili, ja hooldus e-kirjade saatmine. WebJobs SDK on valmisfunktsioone Azure Storage ja teenuse siini töötamiseks, ajastamist ja tõrgete ja jaoks mitme levinud stsenaariumi. Lisaks see on mõeldud olema laiendatav ja on mõni [avage allikas hoidla laiendid](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview). [Azure'i funktsioonid](../azure-functions/functions-overview.md) (praegu eelvaade) põhinevad WebJobs Tarkvaraarenduskomplektist, mis töötab C# skripti, Node.js ja muudes keeltes versioon. 

Loomine ja juurutamine WebJobs haldamine on sujuvalt koos integreeritud instrumentaarium Visual Studios. Saate luua mallide põhjal WebJobs, avaldamine ja hallata (Käivita/peata/kuvar/silumine) neid. 

Azure'i portaalis WebJobs armatuurlaud pakub võimsaid halduse võimaluste, mis teile WebJobs, sh võimalus autonoomsest WebJobs üksikute ülesannete täitmise üle täielik kontroll. Armatuurlaua kuvab ka funktsioon runtimes ja logimine väljundi. 

##<a name="getstarted"></a>WebJobs ja WebJobs SDK töötamise alustamine

* [Azure'i WebJobs tutvustus](http://www.hanselman.com/blog/IntroducingWindowsAzureWebJobs.aspx)
* [Azure'i WebJobs on võimas ja peab algama, kasutades neid kohe!](http://www.troyhunt.com/2015/01/azure-webjobs-are-awesome-and-you.html) (Troy Hunt ajaveebipostitus.)
* [Azure'i WebJobs funktsioonid](/blog/2014/10/22/webjobs-goes-into-full-production/)
* [Mis on WebJobs SDK](websites-dotnet-webjobs-sdk.md)
* [Tausta töö juhiseid Microsoft mustrite ja tavad](/documentation/articles/best-practices-background-jobs/)
* [Funktsiooni 1.1.0 väljakuulutamist RTM Microsoft Azure'i WebJobs SDK](/blog/azure-webjobs-sdk-1-1-0-rtm/)
* [Azure'i WebJobs SDK kasutamise alustamine](websites-dotnet-webjobs-sdk-get-started.md)
* [Kuidas kasutada Azure järjekorda salvestusruumi WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md)
* [Kuidas kasutada WebJobs SDK Azure'i bloobimälu](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)
* [Kuidas kasutada WebJobs SDK Azure'i tabelimälu](websites-dotnet-webjobs-sdk-storage-tables-how-to.md)
* [Kuidas kasutada Azure teenuse siini WebJobs SDK](websites-dotnet-webjobs-sdk-service-bus.md)
* [Kuidas kasutada WebHooks WebJobs Tarkvaraarenduskomplektist, GitHub, IFTTT ja HTTP näiteid](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/WebHooks-Walkthrough)
* [Azure'i WebJobs Tarkvaraarenduskomplektist Lühijuhend (PDF-faili alla laadida)](http://go.microsoft.com/fwlink/?LinkID=524028&clcid=0x409)
* [WebJobs sätted dokumentatsioon GitHub](https://github.com/projectkudu/kudu/wiki/Web-jobs).
* Videod
    * [WebJobs ja WebJobs SDK](http://channel9.msdn.com/Shows/Cloud+Cover/Episode-153-WebJobs-with-Pranav-Rastogi?utm_source=dlvr.it&utm_medium=twitter)
    * [Azure'i WebJobs videosari kanali 9](http://channel9.msdn.com/Tags/azurefridaywebjobs)
    * [Visual Studio tööriistasid WebJobs tutvustus](http://channel9.msdn.com/Shows/Web+Camps+TV/Introducing-WebJobs-Tooling-for-Visual-Studio-with-Brady-Gaster) 
    * [WebJobs instrumentaarium ja Remote silumine](http://channel9.msdn.com/Shows/Web+Camps+TV/WebJobs-GA-Series-Episode-1-WebJobs-Tooling-with-Brady-Gaster)
    * [Azure'i WebJobs uuendus Pranav Rastogi - versioonis 1.1 laiendatavus](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-183-Azure-WebJobs-Update-with-Pranav-Rastogi)

Vt ka järgmistes jaotistes [WebJobs juurutamine](#deploy) ja [testimine ja silumine WebJobs](#debug).

##<a name="deploy"></a>WebJobs juurutamine

* [Kuidas juurutada Azure WebJobs Visual Studio abil](websites-dotnet-deploy-webjobs.md)
* [Juurutamise WebJobs Azure portaalis](web-sites-create-web-jobs.md)
* [Käsurea või pidev kohaletoimetamise Azure WebJobs lubamine](https://azure.microsoft.com/blog/2014/08/18/enabling-command-line-or-continuous-delivery-of-azure-webjobs/)
* [Git abil WebJobs Azure .net-i konsooli rakenduse juurutamine](http://blog.amitapple.com/post/73574681678/git-deploy-console-app/)
* [Azure'i F # WebJob juurutamine](http://blogs.msdn.com/b/dave_crooks_dev_blog/archive/2015/02/18/deploying-f-web-job-to-azure.aspx)
* [Nagu Azure'i Webjobs kohandatud teenuste juurutamine](http://withouttheloop.com/articles/2015-06-23-deploying-custom-services-as-azure-webjobs/)
* Videod
    * [Visual Studio tööriistasid WebJobs tutvustus](http://channel9.msdn.com/Shows/Web+Camps+TV/Introducing-WebJobs-Tooling-for-Visual-Studio-with-Brady-Gaster) 
    * [WebJobs instrumentaarium ja Remote silumine](http://channel9.msdn.com/Shows/Web+Camps+TV/WebJobs-GA-Series-Episode-1-WebJobs-Tooling-with-Brady-Gaster) 

##<a name="schedule"></a>WebJobs plaanimine

* [Funktsiooni Azure'i WebJob dialoog lisa](websites-dotnet-deploy-webjobs.md#configure)
* [Ajastatud WebJob Azure portaali loomine](web-sites-create-web-jobs.md#CreateScheduled)
* [Kinnihakkamine ajasti töö, mida soovite mõne WebJob](http://blog.davidebbo.com/2015/05/scheduled-webjob.html)
* [Azure'i WebJobs cron avaldistega plaanimine](http://blog.amitapple.com/post/2015/06/scheduling-azure-webjobs/)
* [Üksikute WebJob funktsioonide WebJobs SDK TimerTrigger kasutamise plaanimine](websites-dotnet-webjobs-sdk.md#schedule)

##<a name="debug"></a>Testimine ja WebJobs silumine

* [Uue arendaja ja Azure WebJobs Visual Studio funktsioone silumine](http://blogs.msdn.com/b/webdev/archive/2014/11/12/new-developer-and-debugging-features-for-azure-webjobs-in-visual-studio.aspx)
* [WebJobs armatuurlaua kuvamine](websites-dotnet-webjobs-sdk-get-started.md#view-the-webjobs-sdk-dashboard)
* [Kuidas kirjutada logid WebJobs SDK abil ja armatuurlaua kuvamine](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#logs)
* [Remote silumine WebJobs](web-sites-dotnet-troubleshoot-visual-studio.md#remotedebugwj)
* [Kes kirjutas selle bloobimälu?](http://blogs.msdn.com/b/jmstall/archive/2014/02/19/who-wrote-that-blob.aspx) 
* [Majutusteenuse interaktiivsed kood pilveteenuses](http://blogs.msdn.com/b/jmstall/archive/2014/04/26/hosting-interactive-code-in-the-cloud.aspx)
* [Azure'i WebJobs Jälita lisamine](http://blogs.msdn.com/b/mcsuksoldev/archive/2014/09/04/adding-trace-to-azure-web-sites-and-web-jobs.aspx)
* [Jälgimine, diagnoosimine ja tõrkeotsing Microsoft Azure'i Tabelimäluga](../storage/storage-monitoring-diagnosing-troubleshooting.md)
* Videod
    * [WebJobs instrumentaarium ja Remote silumine](http://channel9.msdn.com/Shows/Web+Camps+TV/WebJobs-GA-Series-Episode-1-WebJobs-Tooling-with-Brady-Gaster) 

##<a name="scale"></a>Skaleerimise WebJobs

* [Azure'i veebisaitidega veebirakenduses mastaapimine](http://msdn.microsoft.com/magazine/dn786914.aspx)
* [Azure'i rakendust Service: Architecting suuri skaala valmis For Business veebirakenduste](https://channel9.msdn.com/Events/Build/2014/3-626). Hõlmab skaleerimist web Appsi WebJobs, sh WebJobs SDK abil.
* Videod
    * [Skaala WebJobs läbi](http://channel9.msdn.com/Shows/Azure-Friday/Azure-WebJobs-105-Scaling-out-Web-Jobs)

##<a name="additional"></a>Lisaressursid WebJobs

* [Azure'i WebJobs GA ajaveebipostitus Magnus Mårtensson](http://magnusmartensson.com/azure-webjobs-ga)
* [Azure'i rakendust Service PowerShelli tõstab töötavate](http://blogs.msdn.com/b/nicktrog/archive/2014/01/22/running-powershell-web-jobs-on-azure-websites.aspx)
* [Saada teada, kui teie Azure'i vallandanud WebJobs lõpetab](http://blog.amitapple.com/post/2014/03/webjobs-notification/)
* [Lihtne Web Appi varundamise säilituspoliitika WebJobs](https://azure.microsoft.com/blog/2014/04/28/simple-web-site-backup-retention-policy-with-webjobs/)
* [Azure'i veebirakenduste ja pilveteenustega esimese taotluse aeglane](http://wp.sjkp.dk/windows-azure-websites-and-cloud-services-slow-on-first-request/). Näitab, kuidas kasutada WebJobs simuleerida AlwaysOn funktsiooni, mis on saadaval ainult Standard hinnakirjad taseme jaoks.
* [WebJobs graatsiline sulgumist](http://blog.amitapple.com/post/2014/05/webjobs-graceful-shutdown/#.U72Il_5OWUl). WebJobs SDK graatsiline sulgumist, lugege teemat [graatsiline sulgumist](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#graceful).)
* [Varukoopiate Azure WebJobs & AzCopy automatiseerimine](http://markjbrown.com/azure-webjobs-azcopy/)
* Videod
    * [Azure'i Magnus Mårtensson WebJobs videod](https://www.youtube.com/playlist?list=PLqp1ZOYYUSd81yEzMYLTw8cz91wx_LU9r)
    * [Azure'i WebJobs videosari kanali 9](http://channel9.msdn.com/Tags/azurefridaywebjobs)

##<a name="additionalsdk"></a>Lisaressursid WebJobs SDK

* [WebJobs SDK väljalaskemärkmed](https://github.com/Azure/azure-webjobs-sdk/wiki/Release-Notes)
* [Lähtekoodi WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk)
* [WebJobs SDK laiendid lähtekood](https://github.com/Azure/azure-webjobs-sdk-extensions), koos [üksikasjaliku juhendi laiendatavuse mudel](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview).  
* [Lähtekoodi WebJobs SDK skripti](https://github.com/Azure/azure-webjobs-sdk-script/) (kasutatakse [Azure](../azure-functions/functions-overview.md)funktsioonide)
* [WebJob FREB failide üleslaadimiseks Azure Storage WebJobs SDK abil](http://thenextdoorgeek.com/post/WAWS-WebJob-to-upload-FREB-files-to-Azure-Storage-using-the-WebJobs-SDK)
* [Azure'i webjobs Azure hosting, logimine kasu, on Azure majutatud webjob](http://bypassion.dk/?p=510)
* [Andmete importimine tööriista abil Azure WebJobs ehitamine](http://www.freshconsulting.com/building-data-import-tool-azure-webjobs/)
* [Azure'i funktsioonide ülevaade](../azure-functions/functions-overview.md)
* Videod
    * [Azure'i WebJobs videosari kanali 9](http://channel9.msdn.com/Tags/azurefridaywebjobs)

##<a name="samples"></a>Lihtsad WebJob rakendused

* [Valimi rakenduste WebJobs meeskonnatöö github](https://github.com/azure/azure-webjobs-sdk-samples)
* [Lihtne Azure'i veebirakenduse WebJobs kirjutamata WebJobs SDK abil](http://code.msdn.microsoft.com/Simple-Azure-Website-with-b4391eeb)
* [SiteMonitR](http://code.msdn.microsoft.com/SiteMonitR-dd4fcf77). Näitab ajastatud ja sündmuse põhinev WebJobs kasutamine. Leiate ajaveebipostitusest [SiteMonitR, kasutades Azure WebJobs SDK taastamine](http://www.bradygaster.com/post/rebuilding-the-sitemonitr-using-windows-azure-webjobs).

##<a name="blogs"></a>Ajaveebide

* [Azure'i ajaveeb](/blog)
* [Amit Apple'i ajaveebi](http://blog.amitapple.com/). Keskendub WebJobs (mitte SDK).
* [Magnus Mårtensson ajaveeb](http://magnusmartensson.com/)

##<a name="gethelp"></a>WebJobs abi saamine

* [StackOverflow WebJobs jaoks](http://stackoverflow.com/questions/tagged/azure-webjobs)
* [StackOverflow WebJobs SDK](http://stackoverflow.com/questions/tagged/azure-webjobssdk)
* [Azure'i funktsioonide StackOverflow](http://stackoverflow.com/questions/tagged/azure-functions)
* [Azure'i ja ASP.net-i Foorum](http://forums.asp.net/1247.aspx)
* [Azure'i rakenduse teenuse Web Appside Foorum](http://social.msdn.microsoft.com/Forums/azure/home?forum=windowsazurewebsitespreview)
* [Azure'i Web Appsi kasutaja kõneposti saidi](https://feedback.azure.com/forums/169385-websites/)
* [Twitter](http://twitter.com/). Kasutage funktsiooni #-sildi profiililehe #AzureWebJobs.
* [Aruande WebJobs viga või probleem](https://github.com/projectkudu/kudu/wiki/Reporting-WebJobs-issues)

