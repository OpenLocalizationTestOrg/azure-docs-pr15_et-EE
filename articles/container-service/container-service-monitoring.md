<properties
   pageTitle="Jälgimine on Azure'i teenus kobar koos Datadog | Microsoft Azure'i"
   description="Mõne Azure'i teenus kobar koos Datadog jälgimine. Näiteks Põhiliselt/OS web Kasutajaliidese abil saate juurutada Datadog agentide klaster."
   services="container-service"
   documentationCenter=""
   authors="rbitia"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Ümbriste, näiteks Põhiliselt/OS, keskmise suurusega sülem, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure"   
   ms.date="07/28/2016"
   ms.author="t-ribhat"/>

# <a name="monitor-an-azure-container-service-cluster-with-datadog"></a>Mõne Azure'i teenus kobar koos Datadog jälgimine

Selles artiklis me juurutada Datadog agentide agent sõlmi klaster Azure'i teenus. Selle konfiguratsiooni puhul on vaja Datadog konto. 

## <a name="prerequisites"></a>Eeltingimused 

[Deploy](container-service-deployment.md) ja [ühenduse](container-service-connect.md) kobar, mis on konfigureeritud Azure'i teenus. Uurimine on [maraton UI](container-service-mesos-marathon-ui.md). Minge [http://datadoghq.com](http://datadoghq.com) Datadog kontot häälestada. 

## <a name="datadog"></a>Datadog 

Datadog on jälgimise teenus, mis kogub andmeid oma ümbriste sees klaster Azure'i teenus. Datadog on keskmise suurusega integreerimine armatuurlaud, kus näete teatud mõõdikute oma ümbriste sees. Kogutud oma ümbriste mõõdikute on korraldatud CPU, mälu, võrgu ja i/o. Datadog jagab mõõdikute ümbriste ja pilte. Allpool on näiteks UI näeb jaoks CPU hõivatus.

![Datadog kasutajaliides](./media/container-service-monitoring/datadog4.png)

## <a name="configure-a-datadog-deployment-with-marathon"></a>Konfigureerida maraton Datadog juurutamine

Need juhised näitab teile, kuidas konfigureerida ja juurutada klaster koos maraton Datadog rakendusi. 

Juurdepääs teie AV/OS Kasutajaliidese kaudu [http://localhost:80 /](http://localhost:80/). Kui näiteks Põhiliselt/OS UI "Universum", mis on all vasakul ja seejärel otsige "Datadog" ja klõpsake nuppu "Install."

![Datadog pakendi sees näiteks Põhiliselt/OS universumis](./media/container-service-monitoring/datadog1.png)

Nüüd seadistamiseks on vaja Datadog konto või tasuta prooviversiooni. Kui olete sisse logitud Datadog veebisaidi ilme vasakule ja minge integratsioon -> seejärel API. 

![Datadog API võti](./media/container-service-monitoring/datadog2.png)

Järgmine sisestamine oma API võti Datadog konfiguratsiooni AV/OS universumi sees. 

![Näiteks Põhiliselt/OS universumis Datadog konfigureerimine](./media/container-service-monitoring/datadog3.png) 

Ülaltoodud konfiguratsiooni eksemplarid on seatud 10000000 seda iga kord, kui uus sõlm lisatakse klaster Datadog automaatselt juurutada agent sõlme. See on ajutine lahendus. Kui olete installinud paketi peaksite liikuge tagasi veebisaidile Datadog ja otsige "Armatuurlaudade." Seal näete kohandatud ja integreerimise armatuurlaudade. Keskmise suurusega integreerimine armatuurlaud on container mõõdikute peate klaster jälgimiseks. 
