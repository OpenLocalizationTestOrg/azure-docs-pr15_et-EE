<properties
   pageTitle="Levinumad põhjused pilveteenuses rollid ringlussevõtu | Microsoft Azure'i"
   description="Pilveteenuse rolli, mis äkki ringlusse võib põhjustada märgatavat tööseisakute. Siin on mõned levinud probleeme, mis põhjustavad taaskasutada, rollid, mis võib aidata teil vähendada tööseisakute."
   services="cloud-services"
   documentationCenter=""
   authors="simonxjx"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd"
   ms.date="09/02/2016"
   ms.author="v-six" />

# <a name="common-issues-that-cause-roles-to-recycle"></a>Levinud probleemid, mis põhjustavad rollid taaskasutada

Selles artiklis käsitletakse mõnda juurutamise probleemide levinud põhjused ja pakub tõrkeotsingu näpunäiteid, mis aitavad teil nende probleemide lahendamiseks. Probleem on olemas rakendus on rolli eksemplari ei käivitu, kui see tsüklit lähtestamisel, hõivatud ja peatamine riikide vahel.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="missing-runtime-dependencies"></a>Käitusaja sõltuvustega

Kui teie taotlus on osa sõltub sellest, mis tahes komplekti, mis pole osa .NET Frameworki või selle Azure'i õnnestus Raamatukogu, peate lisama konkreetselt selle asemel rakenduse pakett. Pidage meeles, et muude Microsoft raamistikku ei ole saadaval Azure vaikimisi. Kui teie roll sõltub sellise raamistiku, peate lisama need assemblereid rakenduse pakett.

Enne koostamine ja rakenduse paketti, siis kontrollige järgmist.

- Kui rakendust Visual Studios, veenduge, et **Kohaliku eksemplari** atribuut on seatud **tõene** iga viidatud komplekti, mis pole osa Azure'i SDK või .NET Framework projekti jaoks.

- Veenduge, et fail ei viita mis tahes kasutamata komplektide koostamine element.

- **Toimingu koostada** .cshtml failid on seatud **sisu**. See tagab failid kuvatakse õigesti paketi ja muude viidatud failide kuvamiseni pakkimine võimaldab.

## <a name="assembly-targets-wrong-platform"></a>Komplekti sihtkohtade valesti platvorm

Azure'i on 64-bitine keskkonnas. Seetõttu ei tööta .NET assemblereid koostada 32-bitine target Azure.

## <a name="role-throws-unhandled-exceptions-while-initializing-or-stopping"></a>Roll põhjustab töötlemata erandid käivitamine või peatamine

Kõik visatakse [RoleEntryPoint] ainekursust, mis sisaldab [OnStart], [OnStop], ja [käivitage] meetodid, meetodite erandid on töötlemata erandid. Kui töötlemata erandi esineb üht järgmistest meetoditest, kas prügikastis roll. Kui roll on ringlussevõtu seni, võib see korrutamine töötlemata erandi iga kord, kui püüab alustada.

## <a name="role-returns-from-run-method"></a>Roll tagastab Ava_moodul

[Käivitage] meetod on mõeldud lõputult käivitamiseks. Kui teie kood alistab [käivitada] meetod, see peaks magama lõpmatuseni. Kui [käitada] meetod tagastab, ringlusse roll.

## <a name="incorrect-diagnosticsconnectionstring-setting"></a>Vale DiagnosticsConnectionString seadmine

Kui rakendus kasutab Azure diagnostika, peate määrama teenuse konfiguratsiooni fail soovitud `DiagnosticsConnectionString` Otsingukonfiguratsiooni säte. Seda sätet tuleks täpsustada Azure HTTPS-ühenduse salvestusruumi kontole.

Tagada, et teie `DiagnosticsConnectionString` säte on õige enne juurutamist Azure oma rakendusepaketi, kontrollige järgmist:  

- Funktsiooni `DiagnosticsConnectionString` säte punktide lubatud salvestusruumi konto Azure.  
  Vaikimisi seda sätet suunab Emuleeritud salvestusruumi konto, seega peab konkreetselt selle sätte muutmist enne juurutamist oma rakendusepaketi. Kui muudate seda sätet, Expression.Error erandi kui rolli eksemplari proovib alustada diagnostika kuvar. See võib põhjustada rolli eksemplari lõputult taaskasutada.

- Ühendusstringi on määratud järgmises [vormingus](../storage/storage-configure-connection-string.md). (Protokoll peab olema määratud HTTPS.) Asendage *MyAccountName* nimi oma konto salvestusruumi ja *MyAccountKey* Accessi abil:    

        DefaultEndpointsProtocol=https;AccountName=MyAccountName;AccountKey=MyAccountKey

  Kui arendate rakenduse Microsoft Visual Studio Azure'i tööriistade abil, saate Määrake selle väärtuseks [lehed](https://msdn.microsoft.com/library/ee405486) .

## <a name="exported-certificate-does-not-include-private-key"></a>Eksporditud serti ei sisalda privaatvõti

Web rolli alusel SSL-i käivitamiseks peate veenduma, sisaldab teie eksporditud halduse serdi privaatvõti. Kui kasutate *Windows serdi haldur* eksportida serdi, kindlasti **ekspordi privaatvõti** suvandi valige **Jah** . Serdi tuleb eksportida PFX-vormingus, mis pole praegu toetatud ainult vorming.

## <a name="next-steps"></a>Järgmised sammud

Saate vaadata rohkem [artikleid tõrkeotsingu](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) pilveteenustega.

Saate vaadata rohkem rolli ringlussevõtu stsenaariumid veebisaidil [Kevin Williamson ajaveebi sarja](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).

[RoleEntryPoint]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx
[OnStart]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx
[OnStop]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx
[Käivita]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx
