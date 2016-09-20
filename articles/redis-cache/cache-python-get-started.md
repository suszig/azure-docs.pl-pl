<properties
    pageTitle="Jak korzystać z pamięci podręcznej Redis Azure za pomocą języka Python | Microsoft Azure"
    description="Rozpoczęcie pracy z pamięcią podręczną Redis Azure za pomocą języka Python"
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

# Jak korzystać z pamięci podręcznej Redis Azure za pomocą języka Python

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [ASP.NET](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

W tym temacie opisano sposób rozpoczęcia pracy z pamięcią podręczną Redis Azure za pomocą języka Python.


## Wymagania wstępne

Zainstaluj klienta [redis-py](https://github.com/andymccurdy/redis-py).


## Tworzenie pamięci podręcznej Redis na platformie Azure

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## Pobieranie nazwy hosta i kluczy dostępu

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]


## Włączanie punktu końcowego bez obsługi protokołu SSL

Niektórzy klienci Redis nie obsługują protokołu SSL, a domyślnie [port nieobsługujący protokołu SSL jest wyłączony dla nowych wystąpień pamięci podręcznej Redis Azure](cache-configure.md#access-ports). Obecnie klient [redis-py](https://github.com/andymccurdy/redis-py) nie obsługuje protokołu SSL. 

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-non-ssl-port.md)]


## Dodawanie elementu do pamięci podręcznej i pobieranie go


    >>> import redis
    >>> r = redis.StrictRedis(host='<name>.redis.cache.windows.net',
          port=6380, db=0, password='<key>', ssl=True)
    >>> r.set('foo', 'bar')
    True
    >>> r.get('foo')
    b'bar'


Zastąp element `<name>` swoją nazwą pamięci podręcznej i element `key` swoim kluczem dostępu.


<!--Image references-->
[1]: ./media/cache-python-get-started/redis-cache-new-cache-menu.png
[2]: ./media/cache-python-get-started/redis-cache-cache-create.png



<!--HONumber=sep16_HO1-->


