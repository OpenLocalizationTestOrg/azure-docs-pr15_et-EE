<properties
    pageTitle="Kuidas kasutada Azure Redis vahemälu Node.js | Microsoft Azure'i"
    description="Azure'i Redis vahemälu Node.js ja node_redis kasutamise alustamine."
    services="redis-cache"
    documentationCenter=""
    authors="steved0x"
    manager="douge"
    editor="v-lincan"/>

<tags
    ms.service="cache"
    ms.devlang="nodejs"
    ms.topic="hero-article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="10/25/2016"
    ms.author="sdanie"/>

# <a name="how-to-use-azure-redis-cache-with-nodejs"></a>Kuidas kasutada Azure Redis vahemälu Node.js

> [AZURE.SELECTOR]
- [.NET-I](cache-dotnet-how-to-use-azure-redis-cache.md)
- [ASP.NET-I](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Turvaline, sihtotstarbeline Redis vahemälu, haldab Microsoft Azure'i Redis vahemälu annab teile juurdepääsu. Vahemälu pääseb mis tahes rakenduses Microsoft Azure'i sees.

Selles teemas näidatakse, kuidas alustada Azure'i Redis vahemälu Node.js abil. Teine näide Node.js Azure'i Redis vahemälu abil, vt [koostamine on Node.js vestlus taotluse Socket.IO Azure veebilehel](../app-service-web/web-sites-nodejs-chat-app-socketio.md).


## <a name="prerequisites"></a>Eeltingimused

Installige [node_redis](https://github.com/mranney/node_redis).

    npm install redis

Selle õpetuse kasutab [node_redis](https://github.com/mranney/node_redis). Näiteid kasutamise muud Node.js kliendid, dokumentatsioonist üksikute Node.js klientidele loetletud [Node.js Redis kliendid](http://redis.io/clients#nodejs).

## <a name="create-a-redis-cache-on-azure"></a>Azure Redis vahemälu luua

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-the-host-name-and-access-keys"></a>Hosti nimi ja Accessi klahvid tuua

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

## <a name="connect-to-the-cache-securely-using-ssl"></a>Ühenduse loomine turvaliselt SSL-i vahemälu

Uusima järgud [node_redis](https://github.com/mranney/node_redis) toetada ühenduse Azure'i Redis vahemälu SSL-i. Järgmises näites on kujutatud ühendamiseks Azure'i Redis vahemälu abil 6380 SSL-i lõpp-punktile. Asendage `<name>` vahemälu nimi ja `<key>` kas teie põhi- või klahvi kirjeldatud eelmises jaotises [tuua hosti nimi ja Accessi võtmed](#retrieve-the-host-name-and-access-keys) .

     var redis = require("redis");
    
      // Add your cache name and access key.
    var client = redis.createClient(6380,'<name>.redis.cache.windows.net', {auth_pass: '<key>', tls: {servername: '<name>.redis.cache.windows.net'}});


## <a name="add-something-to-the-cache-and-retrieve-it"></a>Vahemälu midagi lisada ja neid

Järgmises näites näidatakse, kuidas Azure'i Redis vahemälu ühendust, salvestada ja üksuse toomine vahemälu. Veel näiteid [node_redis](https://github.com/mranney/node_redis) kliendi Redis abil, vt [http://redis.js.org/](http://redis.js.org/).

     var redis = require("redis");
    
      // Add your cache name and access key.
    var client = redis.createClient(6380,'<name>.redis.cache.windows.net', {auth_pass: '<key>', tls: {servername: '<name>.redis.cache.windows.net'}});
    
    client.set("key1", "value", function(err, reply) {
            console.log(reply);
        });
    
    client.get("key1",  function(err, reply) {
            console.log(reply);
        });

Väljund:

    OK
    value


## <a name="next-steps"></a>Järgmised sammud

- [Luba vahemälu diagnostika](cache-how-to-monitor.md#enable-cache-diagnostics) saate vahemälu seisundi [jälgimine](cache-how-to-monitor.md) .
- Lugege ametlik [Redis dokumentatsiooni](http://redis.io/documentation).



