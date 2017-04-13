<properties
    pageTitle="Kuidas kasutada Azure Redis vahemälu Python | Microsoft Azure'i"
    description="Azure'i Redis vahemälu Python kasutamise alustamine"
    services="redis-cache"
    documentationCenter=""
    authors="steved0x"
    manager="douge"
    editor="v-lincan"/>

<tags
    ms.service="cache"
    ms.devlang="python"
    ms.topic="hero-article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="08/16/2016"
    ms.author="sdanie"/>

# <a name="how-to-use-azure-redis-cache-with-python"></a>Kuidas kasutada Azure Redis vahemälu Python

> [AZURE.SELECTOR]
- [.NET-I](cache-dotnet-how-to-use-azure-redis-cache.md)
- [ASP.NET-I](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Selles teemas näidatakse, kuidas alustada Azure'i Redis vahemälu Python abil.


## <a name="prerequisites"></a>Eeltingimused

Installige [redis-py](https://github.com/andymccurdy/redis-py).


## <a name="create-a-redis-cache-on-azure"></a>Azure Redis vahemälu luua

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-the-host-name-and-access-keys"></a>Hosti nimi ja Accessi klahvid tuua

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]


## <a name="enable-the-non-ssl-endpoint"></a>Mitte-SSL-i lõpp-punkti lubamine

Mõned Redis kliendid ei toeta SSL-i, ja poolt vaikimisi [-SSL port on keelatud uue Azure Redis vahemälu eksemplarid](cache-configure.md#access-ports). Kirjutamise ajal [redis-py](https://github.com/andymccurdy/redis-py) klient ei toeta SSL-i. 

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-non-ssl-port.md)]


## <a name="add-something-to-the-cache-and-retrieve-it"></a>Vahemälu midagi lisada ja neid


    >>> import redis
    >>> r = redis.StrictRedis(host='<name>.redis.cache.windows.net',
          port=6380, db=0, password='<key>', ssl=True)
    >>> r.set('foo', 'bar')
    True
    >>> r.get('foo')
    b'bar'


Asendage `<name>` koos teie nimega vahemälu ja `key` Accessi abil.


<!--Image references-->
[1]: ./media/cache-python-get-started/redis-cache-new-cache-menu.png
[2]: ./media/cache-python-get-started/redis-cache-cache-create.png
