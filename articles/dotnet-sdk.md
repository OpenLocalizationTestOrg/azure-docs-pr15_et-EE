<properties
    pageTitle="Mis on Azure .NET SDK"
    description="Siit saate teada, mis sisaldub Azure'i .NET SDK."
    documentationCenter=".net"
    authors="tdykstra"
    manager="wpickett"
    editor="mollybos"
    services=""/>

<tags
    ms.service="multiple"
    ms.workload="multiple"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/30/2016"
    ms.author="rachelap"/>

# <a name="what-is-the-azure-sdk-for-net"></a>Mis on Azure SDK .net-i jaoks?

## <a name="overview"></a>Ülevaade

Azure'i SDK .net-i jaoks on komplekt tööriistu Visual Studio, käsurea tööriistad, runtime kahendfaile ja kliendi teegid, mis aitavad teil töötada, testige ja rakendused, mis töötavad Azure juurutamine. See artikkel üksikasjad, mis kuvatakse, kui installite SDK. Saate alla laadida SDK [Azure'i allalaadimiste lehe](https://azure.microsoft.com/downloads/)kaudu.

Azure'i SDK .net-i hõlmab ka [kliendi teegid nõudvate Azure'i teenuste jaoks](http://go.microsoft.com/fwlink/?LinkId=510472). Need teegid installitakse eraldi, kasutades Nugeti.

##<a id="included"></a>Mida sisaldab Azure'i SDK .net-i jaoks

.Net-i Azure SDK installib järgmistele toodetele:

- [Visual Studio ühenduse Edition 2015](#vwd)
- [Microsoft Azure'i salvestusruumi emulaator](#stgemulator)
- [Microsoft Azure'i salvestusruumi tööriistad](#stgtools)
- [Microsoft Azure'i loome tööriistad](#auth)
- [Microsoft Azure'i emulaator](#emulator)
- [Hdinsightiga Tools for Visual Studio ja Microsoft taru ODBC-draiver](#hdinsight)
- [Microsoft Azure'i teekide .net-i jaoks](#libraries)
- [Microsoft Azure'i Mobile'i rakendus SDK](#mobile)
- [Microsoft Azure'i PowerShelli](#ps)
- [Microsoft Visual Studio Microsoft Azure'i tööriistad](#tools)
- [Microsoft ASP.net-i ja Web Tools for Visual Studio](#wte)
- [Microsoft Azure'i andmed Lake Tools for Visual Studio](#datalake)

###<a id="vwd"></a>Visual Studio ühenduse Edition 2015

Kui teie arvutis pole Visual Studios, installib SDK [Visual Studio ühenduse Edition 2015](https://www.visualstudio.com/products/visual-studio-community-vs).

###<a id="stgemulator"></a>Microsoft Azure'i salvestusruumi emulaator

[Azure'i salvestusruumi emulaator](http://msdn.microsoft.com/library/hh403989.aspx) kasutab SQL serveri eksemplar ja kohaliku failisüsteemi simuleerida Azure Storage (järjekorrad, tabelid, plekid) nii, et saate testida kohapeal.

###<a id="stgtools"></a>Microsoft Azure'i salvestusruumi tööriistad

See installib [AzCopy](http://aka.ms/AzCopy), käsurea tööriista abil saate edastada andmeid ja sealt Azure Storage konto.

###<a id="auth"></a>Microsoft Azure'i loome tööriistad

See sisaldab järgmist:

* [CSPack käsurea tööriista](http://msdn.microsoft.com/library/gg432988.aspx) juurutamise pakettide loomiseks.
* [CSEncrypt käsurea tööriista](http://msdn.microsoft.com/library/hh404001.aspx) krüptimiseks paroole, mida kasutatakse kaudu juurde pääseda cloud rolli eksemplaride kaugtöölaua ühendus.
* Käitusaja kahendfaile, et projekte teenus cloud vaja suhtlemiseks oma käitusaja keskkonna ja diagnostika. Need kahendfaile ei ole saadaval NuGet-paketid.

###<a id="emulator"></a>Microsoft Azure'i emulaator

[Azure'i emulaator](http://msdn.microsoft.com/library/dn339018.aspx) jäljendab pilvepõhise teenuse keskkonnas, et testida pilvepõhise teenuse projektide kohalikult arvutis enne nende juurutamist Azure.

###<a id="hdinsight"></a>Hdinsightiga Tools for Visual Studio ja Microsoft taru ODBC-draiver

Hdinsightiga tööriistad Server Explorer võimaldavad taru andmebaasid ja lingitud salvestusruumi kontod Hdinsightiga kogumite, luua tabeleid, ja luua ja esitage taru päringud. Lisateavet leiate teemast [Hdinsightiga Hadoopi Tools for Visual Studio kasutamise alustamine](hdinsight/hdinsight-hadoop-visual-studio-tools-get-started.md).

###<a id="libraries"></a>Microsoft Azure'i teekide .net-i jaoks

See sisaldab järgmist:

* Nugeti paketid Azure Storage, teenuse siini ja vahemälu, mis on talletatud teie arvutis, et Visual Studio saate luua uue pilvepõhise teenuse projektide ühenduseta.
* Mõne Visual Studio lisandmooduli, mis võimaldab [- Rolli vahemälu](http://msdn.microsoft.com/library/dn386103.aspx) projektide käivitamiseks kohalikult Visual Studio.

###<a id="mobile"></a>Microsoft Azure'i Mobile'i rakendus SDK

[Azure'i rakenduse teenuse mobiilirakenduste](app-service-mobile/app-service-mobile-value-prop.md)töötamine tööriistad.

###<a id="ps"></a>Microsoft Azure'i PowerShelli

Azure'i PowerShelli võimaldab [Azure keskkonnas loomise ja juurutamise](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything)automatiseerimiseks.

###<a id="tools"></a>Microsoft Visual Studio Microsoft Azure'i tööriistad

See võimaldab teil Azure ressursse, peamiselt pilveteenustega ja Virtuaalmasinates töötamiseks.

* [Loo avada, ja avaldada pilve teenuse projektid](cloud-services/cloud-services-dotnet-get-started.md).
* [Pilvepõhise teenuse projektide loomine juurutamise paketid](http://msdn.microsoft.com/library/ff683672.aspx).
* [Luua Azure'i Virtuaalmasinates uus web projektide loomisel](virtual-machines/virtual-machines-windows-classic-web-app-visual-studio.md).
* [Looge PowerShelli skriptide uue virtuaalmasinates loomisel](http://msdn.microsoft.com/library/dn642480.aspx).
* [Kuvamine ja haldamine pilveteenuses projekti Teenusesätted Visual Studio projekti atribuutide Windows](http://msdn.microsoft.com/library/ee405486.aspx).
* Vaadata ja hallata [pilveteenustega](http://msdn.microsoft.com/library/ff683675.aspx), [virtuaalmasinates](http://msdn.microsoft.com/library/jj131259.aspx)ja [Teenuse siini](http://msdn.microsoft.com/library/jj149828.aspx) Server Explorer.
* [Kaugühenduse teel puhul pilveteenuste ja virtuaalmasinates silumine režiimis](http://msdn.microsoft.com/library/ff683670.aspx).
* [Ressursi ettevalmistamise Azure'i ressursi Rühmaprojekte juurutuse abil automatiseerida](https://msdn.microsoft.com/library/dn872471.aspx)

###<a id="wte"></a>Microsofti rakendus teenus Tools for Visual Studio

Võimaldab töötada Azure veebisaitide:

* [Avalda web projektide Azure veebisaitide](app-service-web/web-sites-dotnet-get-started.md).
* [Avalda konsooli rakenduse projektide Azure'i WebJobs](app-service-web/websites-dotnet-deploy-webjobs.md).
* [Veebisaidi loomine Azure ja SQL-andmebaasi ressursid uue web projekti loomisel või web projekti avaldamiseks](app-service-web/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md).
* [Juurutamise loomine PowerShelli skripti, uue veebisaitide loomisel](http://msdn.microsoft.com/library/dn642480.aspx).
* [Haldamine ja Azure veebisaitide Server Explorer tõrkeotsing](app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#sitemanagement).
* [Silumine režiimis kaugühenduse teel veebisaitide ja WebJobs jaoks](app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug).

>[AZURE.NOTE] Teil pole vaja installida Azure SDK .net-i kasutamiseks need funktsioonid; need on ka Visual Studio värskendused.

##<a id="datalake"></a>Microsoft Azure'i andmed Lake Tools for Visual Studio

Lisateavet leiate teemast [õpetus: arendamise U-SQL-i skriptide abil andmete Lake tööriistad Visual Studio](data-lake-analytics/data-lake-analytics-data-lake-tools-get-started.md).

##<a id="notincluded"></a>Mida on kaasatud Azure'i SDK installimisel .net-i jaoks

On mõned asjad, mida võiksite Azure arengu, mis pole kaasatud, kui installite SDK. Kõige olulisem neist on järgmised.

* [Kliendi teegid](http://go.microsoft.com/fwlink/?LinkId=510472).

    Azure'i SDK hõlmab kliendi teegid tarbimine Azure'i teenuste jaoks, kuid mitte kõiki installitakse SDK installimisel. Kui teie rakendus peab kliendi teek, mis SDK ei installita, saate selle toomine [NuGet.org](http://go.microsoft.com/fwlink/?LinkId=510472). Kui teie rakendus kasutab kliendi teek, mis SDK installida, on hea tava selle värskendamiseks veebisaidil NuGet.org praegune versioon.

    **Kohalike koopiate kliendi teegid.** Azure'i SDK .net-i kopeerib arvuti Nugeti pakettide jaoks mõne Azure kliendi teegid, nt salvestusruumi, teenuse siini ja vahemälu. Need kliendi teegid kaasatakse uue pilvepõhise teenuse projektide automaatselt nii, et lubada kohaliku Nugeti pakettide Visual Studio abil luua ka siis, kui teil puudub Interneti-ühendus. Kliendi teegid üldiselt värskendatud sagedamini uue SDK versioonid on antud, nii, et kliendi teegid NuGet.org juures on sageli kui SDK abil saate praeguse.

    **Projekti Mallid, mis sisaldavad kliendi teegid.** Ainult [Azure pilveteenus](cloud-services/cloud-services-dotnet-get-started.md) ja Azure'i rakendust Service [Mobiilirakenduste](./app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) projekti mallid sisaldavad automaatselt mõne kliendi teegid. Muud teegid ja muud mallid, [Kliendi teegi NuGet-paketid](http://go.microsoft.com/fwlink/?LinkId=510472) , mis teil on vaja installida.

* [Mobiilirakenduste projekti Mallid](./app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

    Mobiilirakenduste Mallid on saadaval ainult Visual Studio 2013 värskendus 2 ja uuemad versioonid. Nad ei ole saadaval rakenduses Visual Studio 2012 või varasemates versioonides, ja pole Visual Studio 2013 värskenduse 1 ja varasemate versioonidega, isegi kui installite Azure'i SDK .net-i jaoks.

##<a id="faq"></a>Korduma kippuvad küsimused

- [Visual Studio on juba palju Azure funktsioone. Vaja installida Azure SDK .net-i jaoks?](#azinvs)
- [Soovin kliendi teek. Kas ma pean installida Azure SDK .net-i hankimine](#clientlib)
- [Kust leida varasemate versioonide Azure'i SDK .net-i jaoks?](#olderversions)
- [Mis on elutsükli poliitika versioonid Azure SDK .net-i jaoks?](#lifecycle)
- [Millised Külastajate OS versioonid ühildub Azure'i SDK .net-i jaoks?](#guestos)
- [Kuidas desinstallida Azure'i SDK .net-i jaoks?](#uninstall)

###<a id="azinvs"></a>Visual Studio on juba palju Azure funktsioone. Vaja installida Azure SDK .net-i jaoks?

See on hea tava installida SDK, kui soovite välja töötada Azure uusima tööriistade abil. Kui te sooviksite pigem installi SDK, saate seda teha kui järgmised tingimused on täidetud:

* Olete installinud [Visual Studio värskenduse](http://www.visualstudio.com/downloads/download-visual-studio-vs#DownloadFamilies_5)Viimane.
* Arendate ainult Azure veebisaitide või Mobile teenused, mitte pilveteenustega või Virtuaalmasinates jaoks.
* Rakenduse ei kasutada salvestusruumi, või kasutab salvestusruumi, kuid te ei pea salvestusruumi emulaator või AzCopy tööriista abil.

###<a id="clientlib"></a>Soovin kliendi teek. Kas ma pean installida Azure SDK .net-i hankimine

SDK installib kliendi teegid ainult nii, et saate luua pilvepõhise teenuse isegi juhul, kui teil puudub Interneti-ühendus. Praeguse kliendi teegid on saadaval [NuGet.org](http://go.microsoft.com/fwlink/?LinkId=510472)Nugeti paketid. Lisateabe saamiseks vt [mis on lisatud, Azure'i SDK .net-i installimisel](#notincluded) selle dokumendi.

###<a id="olderversions"></a>Kust leida varasemate versioonide Azure'i SDK .net-i jaoks?

Vanemad versioonid leiate [Azure'i SDK .net-i](https://azure.microsoft.com/downloads/archive-net-downloads/) allalaadimiste lehe.

###<a id="lifecycle"></a>Mis on elutsükli poliitika versioonid Azure SDK .net-i jaoks?

Lugege teemat [Microsoft Azure'i pilveteenuste tugiteenuste elutsükli poliitika](http://support.microsoft.com/gp/azure-cloud-lifecycle-faq).

###<a id="guestos"></a>Millised Külastajate OS versioonid ühildub Azure'i SDK .net-i jaoks?

Leiate [Azure'i Külastajate OS väljalasete ja SDK Compatibility maatriks](http://msdn.microsoft.com/library/ee924680.aspx).

###<a id="uninstall"></a>Kuidas desinstallida Azure'i SDK .net-i jaoks?

Iga üksuse, kuvatakse selle artikli jaotises [kaasatava Azure'i SDK .net-i jaoks](#included) on Windowsi juhtpaneelil **programmid ja funktsioonid**installitud programmide loendis programmi eraldi.  Ei saa desinstallida neid rühmana; teil on eraldi iga programmi desinstallimine.

Kui teil on juba installitud .NET Azure SDK ja uue versiooni installimist, on üldiselt ei ole vaja desinstallida vana. Enamikul juhtudel SDK Installi värskendused olemasoleva programmi, mitte uue konto lisamine ja vana lahkumata.

Juhul kui soovite eemaldada pole enam-vajaliku jäänused varasemas versioonis, ainult määrata vanema versiooni programme desinstallida ning ainult need desinstallida, kui sama programmi uuema versiooniga. Näiteks installimist 2,5 2,6 võidakse kuvada nii 2.5 ja 2.6 versiooni "Microsoft Azure'i tööriistad Microsoft Visual Studio 2013", ja seejärel desinstallite 2,5 versiooni. Kuid võidakse kuvada ainult 2,5 versiooni "Microsoft Azure'i loome Tööriistad" ja see ei oleks turvaline, mis desinstallida.

Pange tähele, et numbrid programmi jaotises **programmid** ja funktsioonid saab eksitavat.  Näiteks sisaldab SDK versioon 2.6 "Microsoft Azure Mobile rakenduse SDK V1.0", kui olete installinud SDK Visual Studio 2013 ja "Microsoft Azure'i Mobile'i rakendus SDK V2.0" Visual Studio 2015; versiooni pole sel juhul SDK versioon, kuid näitaja, mis kehtib Visual Studio versioon programm.

Selles artiklis ei loetleta iga programmi, mis sisaldab iga Azure'i SDK varasem versioon; on muudes programmides, saate desinstallida SDK varasemates versioonides, kuna SDK varasemates versioonides sisalduv mõnikord programmid, mis on välja jäetud uuemad versioonid. Näiteks versiooni 2,5 installib "Azure ressursi halduri tööriistad for Visual Studio", kuid mis ei ole loendis selle artikli Kuna seda enam kuvatakse eraldi programmina **programmid**ja funktsioonid.  Selles artiklis on loetletud ainult programmid, mis sisalduvad Azure SDK .net-i versioon 2.6.

> **Märkus:** Mõned programmid, mis sisaldab SDK võib ka eraldi installitud teistes kontekstides ja võib olla vajalik isegi juhul, kui te ei pea täielik SDK. Sama võib true programmi, mis varasemates SDK abil installitud, kuid on välja jäetud SDK uuemad versioonid. Kui desinstallite programmid, olla ettevaatlik, et vältida eemaldamine midagi, mida on vaja oma arvutisse.



##<a id="versions"></a>Versioonid

[Azure'i SDK .NET versiooniajalugu](https://azure.microsoft.com/downloads/archive-net-downloads/) lehelt leiate näha, milline versioon on ajakohane või alla laadida vanemad versioonid.

##<a id="resources"></a>Ressursid

Praeguse Azure'i SDK allalaadimiseks .net-i või kliendi teek, leiate [Azure'i allalaadimislehelt](https://azure.microsoft.com/downloads/).

Leiate Azure'i SDK .NET lähtekoodi, sh kliendi teegid, jaoks [GitHub.com/Azure](https://github.com/azure/).

Teemast Azure kliendi teegi dokumentides, [Azure'i .net-i viide](https://azure.microsoft.com/documentation/api/).
