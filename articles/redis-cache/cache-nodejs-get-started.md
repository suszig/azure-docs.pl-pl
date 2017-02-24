---
title: "Jak korzystać z usługi Azure Redis Cache w środowisku Node.js | Microsoft Docs"
description: "Rozpoczęcie pracy z pamięcią podręczną Redis Azure za pomocą środowiska Node.js i klienta node_redis."
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: v-lincan
ms.assetid: 06fddc95-8029-4a8d-83f5-ebd5016891d9
ms.service: cache
ms.devlang: nodejs
ms.topic: hero-article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 02/10/2017
ms.author: sdanie
translationtype: Human Translation
ms.sourcegitcommit: 5369dcd6ad1ec93c63eb442db9fc5ffdcca37375
ms.openlocfilehash: 530191637b1aa91ee1d7fe5b5bb032c60983f7dc


---
# <a name="how-to-use-azure-redis-cache-with-nodejs"></a>Jak korzystać z pamięci podręcznej Redis Azure w środowisku Node.js
> [!div class="op_single_selector"]
> * [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [ASP.NET](cache-web-app-howto.md)
> * [Node.js](cache-nodejs-get-started.md)
> * [Java](cache-java-get-started.md)
> * [Python](cache-python-get-started.md)
> 
> 

Pamięć podręczna Redis Azure umożliwia dostęp do bezpiecznej dedykowanej pamięci podręcznej Redis zarządzanej przez firmę Microsoft. Pamięć podręczna jest dostępna z poziomu dowolnej aplikacji na platformie Microsoft Azure.

W tym temacie opisano sposób rozpoczęcia pracy z pamięcią podręczną Redis Azure w środowisku Node.js. Inny przykład użycia pamięci podręcznej Redis Azure w środowisku Node.js można znaleźć w temacie [Build a Node.js Chat Application with Socket.IO on an Azure Website](../app-service-web/web-sites-nodejs-chat-app-socketio.md) (Tworzenie aplikacji czatu środowiska Node.js za pomocą biblioteki Socket.IO w witrynie sieci Web platformy Azure).

## <a name="prerequisites"></a>Wymagania wstępne
Zainstaluj klienta [node_redis](https://github.com/mranney/node_redis):

    npm install redis

W tym samouczku jest używany klient [node_redis](https://github.com/mranney/node_redis). Przykłady użycia innych klientów Node.js można znaleźć w dokumentacji poszczególnych klientów Node.js wymienionych na stronie [klientów Node.js usługi Redis](http://redis.io/clients#nodejs).

## <a name="create-a-redis-cache-on-azure"></a>Tworzenie pamięci podręcznej Redis na platformie Azure
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-the-host-name-and-access-keys"></a>Pobieranie nazwy hosta i kluczy dostępu
[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

## <a name="connect-to-the-cache-securely-using-ssl"></a>Bezpieczne łączenie się z pamięcią podręczną przy użyciu protokołu SSL
Najnowsze kompilacje klienta [node_redis](https://github.com/mranney/node_redis) umożliwiają łączenie się z usługą Azure Redis Cache przy użyciu protokołu SSL. Poniższy przykład przedstawia, jak nawiązać połączenie z usługą Azure Redis Cache przy użyciu punktu końcowego 6380 protokołu SSL. Zastąp parametr `<name>` nazwą Twojej pamięci podręcznej, a parametr `<key>` kluczem podstawowym lub dodatkowym, jak opisano w poprzedniej sekcji [Pobieranie nazwy hosta i kluczy dostępu](#retrieve-the-host-name-and-access-keys).

     var redis = require("redis");

      // Add your cache name and access key.
    var client = redis.createClient(6380,'<name>.redis.cache.windows.net', {auth_pass: '<key>', tls: {servername: '<name>.redis.cache.windows.net'}});

> [!NOTE]
> Port inny niż SSL jest wyłączony w przypadku nowych wystąpień usługi Azure Redis Cache. Jeśli używasz innego klienta, który nie obsługuje protokołu SSL, zobacz [How to enable the non-SSL port](cache-configure.md#access-ports) (Jak włączyć port inny niż SSL).
> 
> 

## <a name="add-something-to-the-cache-and-retrieve-it"></a>Dodawanie elementu do pamięci podręcznej i pobieranie go
Poniższy przykład pokazuje, jak połączyć się z wystąpieniem usługi Azure Redis Cache oraz jak zapisać i pobrać element z pamięci podręcznej. Aby uzyskać więcej przykładów użycia usługi Redis z klientem [node_redis](https://github.com/mranney/node_redis), zobacz [http://redis.js.org/](http://redis.js.org/).

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


## <a name="next-steps"></a>Następne kroki
* [Włącz diagnostykę pamięci podręcznej](cache-how-to-monitor.md#enable-cache-diagnostics), aby móc [monitorować](cache-how-to-monitor.md) jej kondycję.
* Przeczytaj oficjalną [dokumentację magazynu Redis](http://redis.io/documentation).




<!--HONumber=Feb17_HO2-->


