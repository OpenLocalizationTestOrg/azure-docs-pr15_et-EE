<properties
   pageTitle="Azure'i Container tutvustus | Microsoft Azure'i"
   description="Azure'i teenus pakub võimalust lihtsustada loomise, konfigureerimise ja haldamise klaster virtuaalmasinates, mis on eelkonfigureeritud konteinerite rakenduste käivitamiseks."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Keskmise suurusega, ümbriste, mikro-teenuste Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/13/2016"
   ms.author="rogardle"/>

# <a name="azure-container-service-introduction"></a>Azure'i teenus tutvustus

Azure'i teenus muudab lihtsam, saate luua, konfigureerimise ja haldamise klaster virtuaalmasinates, mis on eelkonfigureeritud konteinerite rakenduste käitamiseks. Optimeeritud konfigureerimisel populaarsed avatud lähtekoodi plaanimis- ja korraldamise tööriistade kasutab. See võimaldab teil kasutada oma olemasolevaid oskusi või suur ja kasvab keha ühenduse oskused, juurutada ja hallata Microsoft Azure container põhiseid rakendusi kasutada.


![Azure'i teenus pakub võimalust mitme hosts Azure konteinerite rakenduste haldamiseks.](./media/acs-intro/acs-cluster.png)


Azure'i teenus mõjutab tagamaks, et teie taotlus on täielikult portable keskmise suurusega container vorming. Samuti toetab maraton ja AV/OS või keskmise suurusega sülem nii, et need rakendused tuhandete ümbriste või isegi kümneid tuhandeliste muudate.

Azure'i teenus abil saate teha ettevõtte tasemel funktsioone Azure, säilitades siiski rakenduse teisaldamist – sealhulgas teisaldamist korraldamise kihid veebisaidil.

<a name="using-azure-container-service"></a>Azure'i teenus abil
-----------------------------

Meie eesmärk on Azure'i teenus on ümbris majutuskeskkond avatud lähtekoodi tööriistad ja tehnoloogiaid, mis on juba täna populaarne oma klientidele abil esitada. Selleks me jätke standard API lõpp-punktid teie valitud orchestrator (näiteks Põhiliselt/OS või keskmise suurusega sülem). Nende lõpp-punktid abil saate kasutada mis tahes tarkvara, mis suudab rääkida nende lõpp-punktid. Näiteks puhul keskmise suurusega sülem lõpp-punkti, võite kasutada keskmise suurusega käsurea liides (CLI). Näiteks Põhiliselt/OS, võite kasutada DCOS CLI.

<a name="creating-a-docker-cluster-by-using-azure-container-service"></a>Luua keskmise suurusega klaster Azure'i Container teenuse abil
-------------------------------------------------------

Azure'i teenus kasutamise alustamiseks juurutamist Azure ressursihaldur malli, ([Keskmise suurusega sülem](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm) või [Näiteks Põhiliselt/OS](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos)) abil või [CLI](/documentation/articles/xplat-cli-install/)on Azure'i teenus kobar (otsing "Azure'i teenus"), portaali kaudu. Lisa- või täiustatud Azure konfiguratsiooni saab muuta esitatud Kiirjuhend mallid. Azure'i teenus on kobar juurutamine kohta leiate lisateavet teemast [Deploy mõne Azure'i teenus kobar](container-service-deployment.md).

<a name="deploying-an-application"></a>Rakenduse juurutamine
------------------------

Azure'i teenus pakub korraldamise sülem keskmise suurusega või näiteks Põhiliselt/OS valik. Rakenduse juurutamise sõltub teie valitud orchestrator.

### <a name="using-dcos"></a>Näiteks Põhiliselt/OS abil

Näiteks Põhiliselt/OS on jaotatud operatsioonisüsteem Apache Mesos hajutatud süsteemide tuum põhjal. Apache Mesos on majutatud veebisaidil Apache tarkvara Foundation ja on loetletud mõned [suurim nimedes](http://mesos.apache.org/documentation/latest/powered-by-mesos/) kasutajate ja osaliste.

![Azure'i teenus sülem agentide ja juhtslaidi jaoks konfigureeritud.](media/acs-intro/dcos.png)

Näiteks Põhiliselt/OS ja Apache Mesos järgmised on muljetavaldava funktsioonikomplekti.

-   Tõestatud skaleeritavus.

-   Tõrketaluvusega kopeeritud juhtslaidi ja orjad Apache ZooKeeper abil

-   Keskmise suurusega vormindatud ümbriste tugi

-   Kohalikke eraldamise Linux ümbriste ülesannete vahel

-   Multiresource kavandamine (mälu, CPU, ketta ja pordid)

-   Java, Python ja uute paralleelselt rakenduste arendamise C++ API-d

-   Web UI kobar oleku vaatamine

Vaikimisi sisaldab näiteks Põhiliselt/OS töötab Azure'i teenus maraton korraldamise platvormi plaanimiseks töökoormus. Näiteks Põhiliselt/OS kasutuselevõtu ACS kaasas aga mesosfäärini universumi teenused, mida saate lisada oma teenuse säde, Hadoop, Cassandra ja palju muud.

![Näiteks Põhiliselt/OS universumi Azure'i teenus](media/dcos/universe.png)

#### <a name="using-marathon"></a>Maraton abil

Maraton on kobar kogu käivitamise ja süsteemile teenuste cgroups--või keskmise suurusega vormindatud ümbriste Azure'i Container teenus. Maraton pakub web UI, millest saate juurutada rakenduste. Pääsete see URL, mis näeb välja umbes `http://DNS_PREFIX.REGION.cloudapp.azure.com` kui DNS-i\_ees- ja PIIRKONNASÄTETE on nii määratletud juurutamise ajal. Muidugi, saate sisestada oma DNS-i nimi. Ümbris abil maraton web UI käitamise kohta lisateabe saamiseks vt [Container juhtimine web Kasutajaliidese kaudu](container-service-mesos-marathon-ui.md).

![Maraton rakenduste loend](media/dcos/marathon-applications-list.png)

Samuti saate REST API-de maraton suhtlemiseks. On mitmeid kliendi teegid, mis on saadaval iga tööriista kohta. Erinevate keelte--hõlmab ja muidugi, saate kasutada mis tahes keeleks, HTTP-protokolli. Lisaks paljude populaarsete DevOps tööriistad toetada maraton. See pakub suurima paindlikkust toimingute meeskonna jaoks, kui töötate mõne Azure'i teenus kobar. Ümbris maraton REST API abil käitamise kohta leiate lisateavet teemast [Container haldus REST API-ga](container-service-mesos-marathon-rest.md).

### <a name="using-docker-swarm"></a>Keskmise suurusega sülem abil

Keskmise suurusega sülem pakub kohalikke rühmitamise jaoks keskmise suurusega. Kuna keskmise suurusega sülem standard keskmise suurusega API, mis tahes tööriista, mis juba suhtleb keskmise suurusega daemon saate kasutada sülem läbipaistev skaala mitme hosts Azure'i teenus.

![Azure'i teenus, mis on konfigureeritud kasutama näiteks Põhiliselt/OS--jumpbox, agentide ja juhtslaidi nähtaval.](media/acs-intro/acs-swarm2.png)

Toetatud tööriistade haldamise ümbriste sülem klaster kaasata, kuid mitte ainult järgmist:

-   Dokku

-   Keskmise suurusega CLI ja keskmise suurusega koostamine

-   Krane

-   Jenkins

<a name="videos"></a>Videod
------

Alustamine: Azure'i teenus (101):  

> [AZURE.VIDEO azure-container-service-101]

Koosteüksuse rakenduste Azure Container teenuse (Koosta 2016)

> [AZURE.VIDEO build-2016-building-applications-using-the-azure-container-service]
