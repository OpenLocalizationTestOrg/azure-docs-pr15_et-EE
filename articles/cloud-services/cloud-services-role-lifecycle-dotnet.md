<properties 
pageTitle="Toime pilveteenusesse elutsükli sündmused | Microsoft Azure'i" 
description="Siit saate teada, kuidas saab pilveteenuses rolli elutsükli meetodid .net-i" 
services="cloud-services" 
documentationCenter=".net" 
authors="Thraka" 
manager="timlt" 
editor=""/>
<tags 
ms.service="cloud-services" 
ms.workload="tbd" 
ms.tgt_pltfrm="na" 
ms.devlang="na" 
ms.topic="article" 
ms.date="09/06/2016" 
ms.author="adegeo"/>

# <a name="customize-the-lifecycle-of-a-web-or-worker-role-in-net"></a>.Net-i veebis või töötaja rolli elutsükli kohandamine

Kui loote töötaja roll, saate laiendamine [RoleEntryPoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx) klassi, mis pakub meetodid teil alistada, mis võimaldavad teil vastata elutsükli sündmused. Web rollide see tund pole kohustuslik, et tuleb kasutada seda elutsükli sündmuste vastata.

## <a name="extend-the-roleentrypoint-class"></a>Klassi RoleEntryPoint laiendamine

[RoleEntryPoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx) klassi kuuluvad viise, mida nimetatakse Azure, kui see **käivitub**, **töötab**või **lõpetab** veebis või töötaja roll. Soovi korral saate alistada haldamiseks rolli lähtestamine, rolli sulgumist järjestuse või täitmise jutulõnga rolli järgmised võimalused. 

Kui ulatub **RoleEntryPoint**, arvestage järgmist käitumist viisidest:

-   [OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) - ja [OnStop](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx) meetodite tagastada loogikaväärtus, seega on võimalik **false** tulu järgmised võimalused.

     Kui teie kood tagastab väärtuse **false**, rolli protsessi järsku lõpetatakse, mis tahes kohas peate sulgumist jada käivitamata. Üldiselt Vältige **false** naasnud **OnStart** meetod.
     
-   Ülekoormuse **RoleEntryPoint** meetodi sees tabamatu erandite käsitletakse töötlemata erandi.

     Erandi juhul meetodite elutsükli jooksul Azure'i tõstab [UnhandledException](https://msdn.microsoft.com/library/system.appdomain.unhandledexception.aspx) sündmus ja seejärel protsess on lõpetatud. Pärast oma rolli on võetud ühenduseta, on tuleb taaskäivitada Azure. Töötlemata erandi ilmnemisel [peatamine](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.stopping.aspx) sündmus ei tõstetakse ja **OnStop** meetodit nimetatakse.

Kui teie roll ei käivitu või on ringlussevõtu hõivatud, käivitamine ja peatamine riikide vahel, teie koodi korrutamine töötlemata erandi ühe elutsükli sündmuste iga kord, kui roll taaskäivitamist. Sel juhul kasutada [UnhandledException](https://msdn.microsoft.com/library/system.appdomain.unhandledexception.aspx) sündmuse põhjuse erandi ja hakkama õigesti. Teie roll võib naasnud ka [käivitada](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) meetod, mis põhjustab rolli taaskäivitama. Juurutamise Ühendriigid kohta leiate lisateavet teemast [Levinud probleemid mis põhjuse rolle prügikast](cloud-services-troubleshoot-common-issues-which-cause-roles-recycle.md).

> [AZURE.NOTE] Kui kasutate arendada rakenduse **Microsoft Visual Studio Azure'i tööriistad** , rolli projekti Mallid automaatselt laiendamine **RoleEntryPoint** ainekursuse, *WebRole.cs* ja *WorkerRole.cs* failid.

## <a name="onstart-method"></a>OnStart meetod

**OnStart** meetodit nimetatakse, kui teie roll eksemplari tuuakse online Azure. Teostab OnStart koodi, rolli eksemplari on märgitud **hõivatud** ja ei välise liikluse liiklus selle laadi koormusetasakaalustusteenuse. Saate alistada, seda meetodit lähtestamine tööd teha, nt sündmuseohjur rakendamise ja [Azure diagnostika](cloud-services-how-to-monitor.md)käivitamine.

Kui **OnStart** tagastab **väärtuse true**, näiteks on lähtestatud ja Azure nõuab **RoleEntryPoint.Run** meetod. Kui **OnStart** tagastab väärtuse **false**, roll lõpeb kohe, ilma et täitmise mis tahes kavandatud sulgumist järjestuse.

Koodi järgmises näites kujutatakse alistada **OnStart** meetod. See meetod konfigureerib ja algab diagnostika kuvari rolli eksemplari käivitub ja häälestab salvestusruumi kontole logimine andmete kandmine.

```csharp
public override bool OnStart()
{
    var config = DiagnosticMonitor.GetDefaultInitialConfiguration();

    config.DiagnosticInfrastructureLogs.ScheduledTransferLogLevelFilter = LogLevel.Error;
    config.DiagnosticInfrastructureLogs.ScheduledTransferPeriod = TimeSpan.FromMinutes(5);

    DiagnosticMonitor.Start("DiagnosticsConnectionString", config);

    return true;
}
```

## <a name="onstop-method"></a>OnStop meetod

**OnStop** meetodit nimetatakse pärast rolli eksemplari vastu võtnud ühenduseta Azure ja enne protsessi väljub. Saate alistada, seda meetodit helistamiseks koodi vaja oma rolli eksemplari puhtalt sulgeda.

> [AZURE.IMPORTANT] Koodi **OnStop** meetod on piiratud aja jooksul lõpetada, kui seda nimetatakse kasutaja algatatud sulgemise muudel põhjustel. Pärast seekord kaitseaega, protsess on lõpetatud, et te peate veenduge, et selle koodi **OnStop** meetodit saab kiiresti või talub valmimiseni ei tööta. **OnStop** meetodit nimetatakse pärast **lõpetamist** sündmuse tõstetakse.


## <a name="run-method"></a>Meetod

Saate rakendada oma rolli eksemplari pikaajalisi teemat **käivitada** meetodit alistada.

**Käivitage** meetodit alistamine pole vaja; Vaikimisi rakendamine algab mis majutab terve igavik. Kui alistada, **käivitage** meetod, teie kood tuleks blokeerida lõpmatuseni. Kui **käitada** meetod tagastab, on automaatselt nõtkelt taaskasutada; Teisisõnu, Azure'i tekitab **peatamine** sündmus, ja kutsub **OnStop** meetod, et teie sulgumist järjestuse täita enne roll ühenduseta.


### <a name="implementing-the-aspnet-lifecycle-methods-for-a-web-role"></a>ASP.net-i elutsükli meetodid web rolli rakendamine

ASP.net-i elutsükli meetodid, lisaks **RoleEntryPoint** ainekursuse, kui abil saate hallata lähtestamine ja sulgumist järjestuse web rolli. See võib olla kasulik ühilduvuse huvides, kui teil on teisaldamise ASP.net-i taotluse Azure. ASP.net-i elutsükli meetodite nimetatakse sees **RoleEntryPoint** meetodite kaudu. Funktsiooni **rakenduse\_käivitamine** meetodit nimetatakse kui **RoleEntryPoint.OnStart** meetod on lõpule viidud. Funktsiooni **rakenduse\_End** meetodit nimetatakse enne **RoleEntryPoint.OnStop** meetodit nimetatakse.

## <a name="next-steps"></a>Järgmised sammud
Siit saate teada, [pilveteenuse pakett loomise](cloud-services-model-and-package.md)kohta.