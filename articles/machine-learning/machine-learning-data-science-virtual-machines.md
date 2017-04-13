<properties
    pageTitle="Andmete teadus virtuaalse masinad Azure | Microsoft Azure'i"
    description="Määrake üles andmete teadus virtuaalse masina"
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard" 
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="xibingao;bradsev" />

# <a name="data-science-virtual-machines-in-azure"></a>Andmete teadus virtuaalse masinad Azure

Juhised on siin esitatud, mis on kirjeldatud, kuidas häälestada Azure'i VM-i ja SQL-i teenusega on Azure VM IPython märkmiku serverid. Windows virtual arvuti on konfigureeritud, mis toetab näiteks IPython märkmik, Azure'i salvestusruumi Explorer ja AzCopy ning muud Utiliidid, mis on abiks andmete teadus projektid. Azure'i salvestusruumi Exploreris ja AzCopy, näiteks pakkuda mugavat võimalust andmete üleslaadimiseks Azure salvestusruumi oma kohalikust arvutist või allalaadimiseks oma arvutisse salvestusruumist. 

Selle menüü erinevate andmete teadus keskkonnas kasutatavaid [Meeskonnatöö andmete teadus protsess (TDSP)](data-science-process-overview.md)häälestamise kirjeldavate teemade lingid.

[AZURE.INCLUDE [data-science-environment-setup](../../includes/cap-setup-environments.md)]

Mitut tüüpi Azure'i virtuaalmasinates saate ette valmistatud ja konfigureeritud pilvepõhist andmete teadus keskkonna osana kasutada. Tüüp ja kogus andmete modelleerida masina õppimine ja andmeid pilves sihtkoht koos sõltub otsus kohta, milliseid maitse virtuaalse masina kasutada. 

* Seda otsust tehes silmas pidada küsimused vaadake teemast [Leping teie Azure seadme õ andmete teadus keskkonnas](machine-learning-data-science-plan-your-environment.md). 
* Kataloogi tehes täiustatud analüüsi ilmneda võivate probleemide, leiate [Täpsemalt Analytics protsessi ja tehnoloogia ja Azure seadme õppimine stsenaariumid](machine-learning-data-science-plan-sample-scenarios.md)

Kahte tüüpi juhised on esitatud.

* [Azure'i virtuaalarvuti nimega serveriks IPython märkmiku täiustatud analüüsi jaoks häälestada](machine-learning-data-science-setup-virtual-machine.md) kuvatakse ettevalmistamise on Azure virtuaalse masina IPython märkmiku ja muid tööriistu, mida kasutatakse andmete teadus juhtudel, kus saab kasutada peale SQL Azure'i salvestusruumi vormi andmete talletamiseks teha.

* [Häälestamine on Azure SQL serveri virtual arvutisse kui serveriks IPython märkmiku jaoks täiustatud analüüsi](machine-learning-data-science-setup-sql-server-virtual-machine.md) kuvatakse ettevalmistamise on Azure SQL serveri virtuaalse masina IPython märkmiku ja muid tööriistu, mida kasutatakse andmete teadus juhtudel, kus saate kasutada SQL-andmebaasi andmete talletamiseks teha.

Kui ettevalmistatud ja konfigureeritud, nende virtuaalmasinates on kasutamiseks valmis IPython märkmiku serverid uurimise ja andmete töötlemise ja muude toimingute tegemiseks vaja koos Azure seadme õppimine ja meeskonnatöö andmete teadus protsess (TDSP). Järgmiste juhiste juurde andmete teadus protsessi on vastendatud [TDSP õppeteema](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) ja võivad sisaldada toiminguid, mis teisaldamine SQL serveri või HDInsight, andmete töötlemine ja kuulake seal õ andmete Azure seadme õppe ettevalmistamiseks.


> [AZURE.NOTE] Azure'i Virtuaalmasinates võetakse **maksma ainult, mida te kasutate**. Selleks, et teil on pole mille eest tuleb tasuda kasutamisel ei virtual arvuti, peab olema [Azure klassikaline portaali](http://manage.windowsazure.com/)olekus **peatamiseni (Deallocated)** . Üksikasjalikud juhised või kuidas te Deallocate virtuaalse masina, vt [sulgemine ja deallocate virtuaalse masina kui seda ei kasuta](machine-learning-data-science-setup-virtual-machine.md#shutdown)
 
