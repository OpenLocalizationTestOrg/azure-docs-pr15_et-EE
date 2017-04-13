<properties
 pageTitle="Azure'i CDN Azure Web Apps/pilveteenustega ja ASP.net-i IIS-i sisu aegumise haldamine | Microsoft Azure'i"
 description="Kirjeldab, kuidas hallata pilvepõhise teenuse sisu Azure'i CDN möödumist"
 services="cdn"
 documentationCenter=".NET"
 authors="camsoper"
 manager="erikre"
 editor=""/>
<tags
 ms.service="cdn"
 ms.workload="media"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.date="09/19/2016"
 ms.author="casoper"/>

# <a name="how-to-manage-expiration-of-azure-web-appscloud-services-aspnet-or-iis-content-in-azure-cdn"></a>Kuidas hallata aegumise Azure Web Apps/pilveteenustega, ASP.net-i või IIS-i sisu Azure'i CDN-ID

> [AZURE.SELECTOR]
- [Rakenduste Azure Web pilveteenustega, ASP.net-i või IIS-i](cdn-manage-expiration-of-cloud-service-content.md)
- [Azure'i bloobimälu salvestusteenus](cdn-manage-expiration-of-blob-content.md)

Mis tahes avalikult juurdepääsetava origin veebiserver faile saab kopeerida Azure'i CDN kuni selle aja live (TTL) kaitseaega.  TTL määratakse [ *Olla* päise](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) origin serveri HTTP vastus.  Selles artiklis kirjeldatakse, kuidas määrata `Cache-Control` Azure veebirakenduste, Azure'i pilveteenustega, ASP.net-i rakendused ja Internet Information Services saidid, mis on konfigureeritud ka päised.

>[AZURE.TIP] Võite määrata pole TTL faili.  Sel juhul kehtib Azure'i CDN automaatselt vaikimisi TTL seitse päeva.
>
>Azure'i CDN kiirendada juurdepääs failidele ja muud ressursid tööpõhimõtete kohta leiate lisateavet teemast [Azure CDN ülevaade](./cdn-overview.md).

## <a name="setting-cache-control-headers-in-configuration"></a>Sätte olla päiste konfigureerimine

Staatiliseks sisuks, nt pilte ja laadilehti, saate määrata Värskenda sagedus, muutes oma veebirakenduse **applicationHost.config** või **web.config** faile.  Loob **system.webServer\staticContent\clientCache** elemendi konfiguratsioonifailis on `Cache-Control` päise sisu jaoks. Jaoks **web.config**, konfiguratsioonisätted mõjutavad kõik soovitud kaust ja kõik alamkaustad, kui alistada alamkausta tasemel.  Näiteks saate määrata vaikimisi aja live keskmes on kõik staatiliseks sisuks vahemälus talletatud 3 päeva, kuid on alamkaust, mis on veel muutuv sisu vahemälu säte on 6 tunni.  **ApplicationHost.config**, kõik rakendused saidil mõjutab, kuid saate alistada faile **web.config** rakendustes.

Järgmine XML-i kuvatakse ja 3 päeva vanuse määramiseks säte **clientCache** näide:  

```xml
<configuration>
    <system.webServer>
        <staticContent>
            <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00" />
        </staticContent>
    </system.webServer>
</configuration>
```

Määrab **UseMaxAge** lisab on `Cache-Control: max-age=<nnn>` päist vastuse alusel määratud **CacheControlMaxAge** atribuudi väärtus. Vormingu kuuline ajavahemik on **cacheControlMaxAge** atribuut on <days>. <hours>:<min>:<sec>. **ClientCache** sõlm kohta leiate lisateavet teemast [Kliendi vahemälu <clientCache> ](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache).  

## <a name="setting-cache-control-headers-in-code"></a>Sätte olla päiste kood

ASP.net-i rakendused, saate määrata CDN-ID vahemällu käitumise programmiliselt, seades atribuuti **HttpResponse.Cache** . Atribuudi **HttpResponse.Cache** kohta leiate lisateavet teemast [HttpResponse.Cache atribuudi](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) ja [HttpCachePolicy klassi](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).  

Kui soovite programmiliselt vahemälu sisu ASP.net-i, veenduge, et sisu on märgitud cacheable seadmisega HttpCacheability *suhtes*. Lisaks veenduge, et vahemälu süntaksi on määratud. Vahemälu süntaksi võib olla viimati muudetud ajatempli seadmine, helistades SetLastModified või etag väärtus määramine, helistades SetETag. Soovi korral saate määrata ka vahemälu aegumise aeg, helistades SetExpires või saate alati kasutada vaikimisi vahemälu heuristika eespool kirjeldatud selles dokumendis.  

Näiteks vahemälu sisu üks tund, et lisada järgmist:  

```csharp
// Set the caching parameters.
Response.Cache.SetExpires(DateTime.Now.AddHours(1));
Response.Cache.SetCacheability(HttpCacheability.Public);
Response.Cache.SetLastModified(DateTime.Now);
```

## <a name="next-steps"></a>Järgmised sammud

- [Lugege üksikasjalikumat teavet **clientCache** elementide kohta](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache)
- [Lugege dokumentatsiooni **HttpResponse.Cache** atribuut](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) 
- [Lugege **HttpCachePolicy klassi**dokumentatsioonist](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).  
