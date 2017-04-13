<properties
   pageTitle="Jälgimine on Azure'i teenus kobar koos Sysdig | Microsoft Azure'i"
   description="Mõne Azure'i teenus kobar koos Sysdig jälgimine."
   services="container-service"
   documentationCenter=""
   authors="rbitia"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Ümbriste, näiteks Põhiliselt/OS Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/08/2016"
   ms.author="t-ribhat"/>

# <a name="monitor-an-azure-container-service-cluster-with-sysdig"></a>Mõne Azure'i teenus kobar koos Sysdig jälgimine

Selles artiklis me juurutada Sysdig agentide agent sõlmi klaster Azure'i teenus. Peate selle konfiguratsiooni puhul Sysdig konto. 

## <a name="prerequisites"></a>Eeltingimused 

[Deploy](container-service-deployment.md) ja [ühenduse](container-service-connect.md) kobar, mis on konfigureeritud Azure'i teenus. Uurimine on [maraton UI](container-service-mesos-marathon-ui.md). Minge [http://app.sysdigcloud.com](http://app.sysdigcloud.com) Sysdig pilvepõhise konto häälestamine. 

## <a name="sysdig"></a>Sysdig

Sysdig on jälgimise teenus, mis võimaldab teil jälgida oma ümbriste klaster sees. Sysdig on teada, et tõrkeotsingu, kuid on ka teie tavaline jälgimisega seotud mõõdikute CPU, võrgunduse, mälu ja I/O. Sysdig lihtne näha, milline ümbriste töötavad kõige raskem või põhiosas kõige rohkem mälu ja CPU abil. See vaade on jaotises "Ülevaade", mis on praegu beeta. 

![Sysdig kasutajaliides](./media/container-service-monitoring-sysdig/sysdig6.png) 

## <a name="configure-a-sysdig-deployment-with-marathon"></a>Sysdig juurutamise ja maraton konfigureerimine

Need juhised näitab teile, kuidas konfigureerida ja juurutada Sysdig rakenduste klaster koos maraton. 

Juurdepääs teie AV/OS Kasutajaliidese kaudu [http://localhost:80 /](http://localhost:80/) üks kord AV/OS UI liikuge "Universum", mis on all vasakul ja seejärel otsige "Sysdig."

![Näiteks Põhiliselt/OS universumis Sysdig](./media/container-service-monitoring-sysdig/sysdig1.png)

Nüüd tuleb seadistamiseks Sysdig pilvepõhise konto või tasuta prooviversiooni. Kui olete sisse loginud Sysdig cloud veebisaidile, klõpsake oma kasutajanime, ja lehel peaksite nägema oma "juurdepääsu võti." 

![Sysdig API võti](./media/container-service-monitoring-sysdig/sysdig2.png) 

Järgmine sisestamine oma kiirklahv Sysdig konfiguratsiooni AV/OS universumi sees. 

![Näiteks Põhiliselt/OS universumis Sysdig konfigureerimine](./media/container-service-monitoring-sysdig/sysdig3.png)

Nüüd Sea eksemplarid, et 10000000 nii iga kord, kui lisatakse uus sõlm kobar Sysdig automaatselt juurutada agent uue sõlme. See on ajutine lahendus veendumaks, et Sysdig saavad võtta kasutusele kõigi uute agentide klaster sees. 

![Näiteks Põhiliselt/OS universumiga-eksemplari Sysdig konfigureerimine](./media/container-service-monitoring-sysdig/sysdig4.png)

Kui olete installinud paketi liikuge tagasi Sysdig Kasutajaliidese ja jagamisega uurimiseks eri kasutus mõõdikute ümbriste sees klaster jaoks. 