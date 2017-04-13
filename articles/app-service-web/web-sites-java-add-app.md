<properties 
    pageTitle="Azure'i rakenduse teenuse veebirakenduste Java rakenduse lisamine" 
    description="Selle õpetuse näidatakse, kuidas lisada lehe või rakenduse oma eksemplarile Azure'i rakenduse teenuse Web Apps, mis on juba konfigureeritud kasutama Java." 
    services="app-service\web" 
    documentationCenter="java" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="add-a-java-application-to-azure-app-service-web-apps"></a>Azure'i rakenduse teenuse veebirakenduste Java rakenduse lisamine

Kui teie Java veebirakenduse [Azure'i rakendust Service][] on lähtestatud, nagu seda [Java veebirakenduse teenuses Azure rakenduse loomine](web-sites-java-get-started.md), saate üles laadida rakenduse pannes oma sõda **veebirakenduste** kausta.

**Veebirakenduste** kausta tee navigeerimine väärtus erineb sõltuvalt teie veebirakenduste eksemplari häälestamise kohta.

- Kui häälestate oma veebirakenduse abil Azure'i turuplatsi, **veebirakenduste** kausta tee on kujul **d:\home\site\wwwroot\bin\application\_server\webapps**, kus **rakenduse\_server** serveri nimi oma veebirakenduste eksemplari kohta. 
- Kui häälestate oma veebirakenduse Azure konfiguratsiooni Kasutajaliidese abil, on vormi **d:\home\site\wwwroot\webapps** **veebirakenduste** kausta tee. 

Pange tähele, et saate üles laadida teie rakendus või veebilehed, sh [pidev integratsioon stsenaariumid](app-service-continuous-deployment.md)Juhtelemendi allikas. FTP on ka teie rakendus või veebilehtede üles suvand.

Märkus Tomcat veebirakendustes: pärast üleslaadimist sõda faili **veebirakenduste** kausta, Tomcat rakenduste serveri tuvastab, et olete lisanud ja seda automaatselt laadida. Pange tähele, et kui kopeerite failid (v.a sõda failid) juurkaustas, rakenduste serveri tuleb taaskäivitada enne nende faile saate kasutada. Töötavate Azure'i Tomcat Java veebirakenduste autoload funktsionaalsus põhineb lisanud, sõda uue faili või uute failide või kataloogide **veebirakenduste** kausta lisada. 

## <a name="next-steps"></a>Järgmised sammud

Lisateavet leiate teemast [Java Arenduskeskus](/develop/java/).

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- External Links -->
[Azure'i rakendust Service]: http://go.microsoft.com/fwlink/?LinkId=529714
