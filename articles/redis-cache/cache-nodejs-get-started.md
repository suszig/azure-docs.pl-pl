<properties
    pageTitle="Jak korzystać z pamięci podręcznej Redis Azure w środowisku Node.js | Microsoft Azure"
    description="Rozpoczęcie pracy z pamięcią podręczną Redis Azure za pomocą środowiska Node.js i klienta node_redis."
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
    ms.date="05/31/2016"
    ms.author="sdanie"/>

# Jak korzystać z pamięci podręcznej Redis Azure w środowisku Node.js

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [ASP.NET](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Pamięć podręczna Redis Azure umożliwia dostęp do bezpiecznej dedykowanej pamięci podręcznej Redis zarządzanej przez firmę Microsoft. Pamięć podręczna jest dostępna z poziomu dowolnej aplikacji na platformie Microsoft Azure.

W tym temacie opisano sposób rozpoczęcia pracy z pamięcią podręczną Redis Azure w środowisku Node.js. Inny przykład użycia pamięci podręcznej Redis Azure w środowisku Node.js można znaleźć w temacie [Build a Node.js Chat Application with Socket.IO on an Azure Website](../app-service-web/web-sites-nodejs-chat-app-socketio.md) (Tworzenie aplikacji czatu środowiska Node.js za pomocą biblioteki Socket.IO w witrynie sieci Web platformy Azure).


## Wymagania wstępne

Zainstaluj klienta [node_redis](https://github.com/mranney/node_redis):

    npm install redis

W tym samouczku używany jest program [node_redis](https://github.com/mranney/node_redis), ale można użyć dowolnego klienta Node.js wymienionego pod adresem [http://redis.io/clients](http://redis.io/clients).

## Tworzenie pamięci podręcznej Redis na platformie Azure

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## Pobieranie nazwy hosta i kluczy dostępu

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]


## Włączanie punktu końcowego bez obsługi protokołu SSL

Niektórzy klienci Redis nie obsługują protokołu SSL, a domyślnie [port nieobsługujący protokołu SSL jest wyłączony dla nowych wystąpień pamięci podręcznej Redis Azure](cache-configure.md#access-ports). Obecnie klient [node_redis](https://github.com/mranney/node_redis) nie obsługuje protokołu SSL. 

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-non-ssl-port.md)]


## Dodawanie elementu do pamięci podręcznej i pobieranie go

      var redis = require("redis");
    
      // Add your cache name and access key.
    var client = redis.createClient(6380,'<name>.redis.cache.windows.net', {auth_pass: '<key>', tls: {servername: '<name>.redis.cache.windows.net'}});
    
    client.set("key1", "value", function(err, reply) {
            console.log(reply);
        });
    
    client.get("key1",  function(err, reply) {
            console.log(reply);
        });

Dane wyjściowe:

    OK
    value


## Następne kroki

- [Włącz diagnostykę pamięci podręcznej](cache-how-to-monitor.md#enable-cache-diagnostics), aby móc [monitorować](cache-how-to-monitor.md) jej kondycję.
- Przeczytaj oficjalną [dokumentację magazynu Redis](http://redis.io/documentation).






<!--HONumber=jun16_HO2-->


