<properties
   pageTitle="Kuidas kasutada Azure Redis vahemälu Java | Microsoft Azure'i"
    description="Azure'i Redis vahemälu Java kasutamise alustamine"
    services="redis-cache"
    documentationCenter=""
    authors="steved0x"
    manager="douge"
    editor=""/>

<tags
    ms.service="cache"
    ms.devlang="java"
    ms.topic="hero-article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="08/24/2016"
    ms.author="sdanie"/>

# <a name="how-to-use-azure-redis-cache-with-java"></a>Kuidas kasutada Azure Redis vahemälu Java

> [AZURE.SELECTOR]
- [.NET-I](cache-dotnet-how-to-use-azure-redis-cache.md)
- [ASP.NET-I](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Azure'i Redis vahemälu annab teile juurdepääsu spetsiaalne Redis vahemälu, haldab Microsoft. Vahemälu pääseb mis tahes rakenduses Microsoft Azure'i sees.

Selles teemas näidatakse, kuidas alustada Azure'i Redis vahemälu Java abil.

## <a name="prerequisites"></a>Eeltingimused

[Jedis](https://github.com/xetorthio/jedis) - Java kliendi jaoks Redis

Selle õpetuse kasutab Jedis, kuid saate kasutada mis tahes Java klient, mis on loetletud [http://redis.io/clients](http://redis.io/clients).

## <a name="create-a-redis-cache-on-azure"></a>Azure Redis vahemälu luua

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-the-host-name-and-access-keys"></a>Hosti nimi ja Accessi klahvid tuua

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]


## <a name="enable-the-non-ssl-endpoint"></a>Mitte-SSL-i lõpp-punkti lubamine

Mõned Redis kliendid ei toeta SSL-i, ja poolt vaikimisi [-SSL port on keelatud uue Azure Redis vahemälu eksemplarid](cache-configure.md#access-ports). Kirjutamise ajal [Jedis](https://github.com/xetorthio/jedis) klient ei toeta SSL-i. 

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-non-ssl-port.md)]




## <a name="add-something-to-the-cache-and-retrieve-it"></a>Vahemälu midagi lisada ja neid

    package com.mycompany.app;
    import redis.clients.jedis.Jedis;
    import redis.clients.jedis.JedisShardInfo;

    /* Make sure you turn on non-SSL port in Azure Redis using the Configuration section in the Azure Portal */
    public class App
    {
      public static void main( String[] args )
      {
        /* In this line, replace <name> with your cache name: */
        JedisShardInfo shardInfo = new JedisShardInfo("<name>.redis.cache.windows.net", 6379);
        shardInfo.setPassword("<key>"); /* Use your access key. */
        Jedis jedis = new Jedis(shardInfo);
        jedis.set("foo", "bar");
        String value = jedis.get("foo");
      }
    }


## <a name="next-steps"></a>Järgmised sammud

- [Luba vahemälu diagnostika](https://msdn.microsoft.com/library/azure/dn763945.aspx#EnableDiagnostics) saate vahemälu seisundi [jälgimine](https://msdn.microsoft.com/library/azure/dn763945.aspx) .
- Lugege ametlik [Redis dokumentatsiooni](http://redis.io/documentation).

