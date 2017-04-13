<properties
   pageTitle="Teenuse struktuuri projekti loomine järgmised toimingud | Microsoft Azure'i"
   description="See artikkel sisaldab linke core arengu tööülesannete kogumi jaoks teenuse struktuuri"
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/08/2016"
   ms.author="seanmck"/>

# <a name="your-service-fabric-application-and-next-steps"></a>Oma teenuse struktuuri rakendus ja järgmised toimingud
Azure teenuse struktuuri rakenduse on loodud. Selles artiklis kirjeldatakse meik projekti ja võimalikke edasiseid juhiseid.

## <a name="your-application"></a>Rakenduse
Iga uue rakenduse sisaldab ka rakenduses project. Ühe või kahe täiendavate projektide sõltuvalt valitud teenuse võib.

### <a name="the-application-project"></a>Rakenduse project
Rakenduse project koosneb järgmistest elementidest:

- Viited teenuseid, mis moodustavad rakenduse kogum.

- Kaks avaldada profiilid (Kohalik ja pilveteenuse), et saate säilitada eelistused töötamiseks viibite – näiteks kobar endpoint ja kas versioonitäienduse juurutuste vaikimisi seotud eelistused.

- Kahe rakenduse parameetri failid (Kohalik ja pilveteenuse) abil saate säilitada keskkonna konkreetse rakenduse konfiguratsioone, nt sektsioonide loomine teenuse arv.

- Juurutamise skripti, mis abil saate juurutada rakendust käsurea kaudu või osana automatiseeritud pidev integratsioon ja juurutamise teel.

- Rakenduse manifesti, mis kirjeldab rakenduse. Leiate manifest ApplicationPackageRoot kaustas.

### <a name="stateless-service"></a>Kodakondsuseta teenus
Kui lisate uue kodakondsuseta teenuse, Visual Studio lisatakse teie lahenduse, mis sisaldab sellist tüüpi pärinevad teenuse project `StatelessService`. Teenuse suurendab kohalikku muutujat mõne vastupäeva.

### <a name="stateful-service"></a>Stateful teenus
Kui lisate uue stateful teenuse, Visual Studio lisatakse teie lahenduse, mis sisaldab sellist tüüpi pärinevad teenuse project `StatefulService`. Teenuse suurendab vastuolus sisse oma `RunAsync` meetod ja salvestab tulemuseks on `ReliableDictionary`.

### <a name="actor-service"></a>Näitleja teenus
Kui lisate uue usaldusväärne näitleja, Visual Studio lisatakse teie lahendus kaks projekti: mõnda näitleja projekti ja mõnda kasutajaliidese projekti.

Näitleja projekt pakub võimalust sätte ja saada vastuolus väärtus, mis on usaldusväärselt püsis Ameerika olekusse sees. Kasutajaliidese projekti pakub liidest, mis autonoomsest osaleja abil saate muude teenustega.

### <a name="stateless-web-api"></a>Kodakondsuseta veebi-API
Kodakondsuseta Veebiteenuste projekti pakub lihtsa veebiteenus, mille abil saate avada rakenduse välise klientidele. Kohta leiate lisateavet teemast liigendatud, projekti [teenuse struktuuri Web API teenustega OWIN ise hosting](service-fabric-reliable-services-communication-webapi.md).

### <a name="aspnet-core"></a>ASP.net-i core

Teenuse struktuuri SDK pakub ASP.net-i Core samu autonoomse ASP.net-i Core projektide jaoks saadaolevaid malle: tühjendamiseks [Veebi-API][aspnet-webapi], ja [Veebirakenduse][aspnet-webapp].

## <a name="next-steps"></a>Järgmised sammud
### <a name="create-an-azure-cluster"></a>Saate luua ka Azure kobar
Teenuse struktuuri SDK pakub kohaliku kobar katsetamine. Azure klaster loomiseks vaadake teemat [teenuse struktuuri kobar Azure portaalist häälestamise][create-cluster-in-portal].

### <a name="try-deploying-to-azure-for-free-with-party-clusters"></a>Proovige koos tootja kogumite tasuta Azure juurutamine

Kui soovite proovida juurutamisel ja haldamine rakenduste Azure ilma häälestamise oma kogumite, saate tasuta [kobar teenus](http://aka.ms/tryservicefabric).

### <a name="publish-your-application-to-azure"></a>Azure'i rakenduse avaldamine
Saate avaldada oma rakenduse otse Visual Studio on Azure kobar. Lisateavet leiate [Azure'i rakenduse avaldamise][publish-app-to-azure].

### <a name="use-service-fabric-explorer-to-visualize-your-cluster"></a>Teenuse struktuuri Exploreri abil visualiseerimine klaster
Teenuse struktuuri Explorer pakub lihtne viis visualiseerida klaster, sh juurutatud rakendused ja füüsilise paigutus. Lisateavet leiate teemast [klaster teenuse struktuuri Exploreri abil visualiseerimine][visualize-with-sfx].

### <a name="version-and-upgrade-your-services"></a>Versiooni ja versioonitäienduse teenuste
Teenuse struktuuri võimaldab sõltumatu Versioonimine ja sõltumatu teenuste rakenduse täiendamine. Lisateabe saamiseks lugege teemat [Versioonimine ja teenuste üleminekut][app-upgrade-tutorial].

### <a name="configure-continuous-integration-with-visual-studio-team-services"></a>Visual Studio meeskonnatöö teenustega pidev integreerimise konfigureerimine
Siit saate teada, kuidas saate häälestada pidev integratsioon protsessi rakenduse teenuse struktuuri, vaadake [konfigureerimine pidev integreerimine Visual Studio Team Services][ci-with-vso].


<!-- Links -->
[add-web-frontend]: service-fabric-add-a-web-frontend.md
[create-cluster-in-portal]: service-fabric-cluster-creation-via-portal.md
[publish-app-to-azure]: service-fabric-publish-app-remote-cluster.md
[visualize-with-sfx]: service-fabric-visualizing-your-cluster.md
[ci-with-vso]: service-fabric-set-up-continuous-integration.md
[reliable-services-webapi]: service-fabric-reliable-services-communication-webapi.md
[app-upgrade-tutorial]: service-fabric-application-upgrade-tutorial.md
[aspnet-webapi]: https://docs.asp.net/en/latest/tutorials/first-web-api.html
[aspnet-webapp]: https://docs.asp.net/en/latest/tutorials/first-mvc-app/index.html
