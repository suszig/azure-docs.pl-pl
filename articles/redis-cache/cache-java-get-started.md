---
title: "Jak używać usługi Azure Redis Cache za pomocą języka Java | Microsoft Docs"
description: "Rozpoczęcie pracy z usługą Azure Redis Cache za pomocą języka Java"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 29275a5e-2e39-4ef2-804f-7ecc5161eab9
ms.service: cache
ms.devlang: java
ms.topic: hero-article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 08/24/2016
ms.author: sdanie
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 907f75dc02bff7e25712a564410c1974e22f0d99


---
# <a name="how-to-use-azure-redis-cache-with-java"></a>Jak używać pamięci podręcznej Redis Azure za pomocą języka Java
> [!div class="op_single_selector"]
> * [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [ASP.NET](cache-web-app-howto.md)
> * [Node.js](cache-nodejs-get-started.md)
> * [Java](cache-java-get-started.md)
> * [Python](cache-python-get-started.md)
> 
> 

Usługa Azure Redis Cache umożliwia dostęp do dedykowanej pamięci podręcznej Redis, zarządzanej przez firmę Microsoft. Pamięć podręczna jest dostępna z poziomu dowolnej aplikacji na platformie Microsoft Azure.

W tym temacie opisano sposób rozpoczęcia pracy z usługą Azure Redis Cache za pomocą języka Java.

## <a name="prerequisites"></a>Wymagania wstępne
[Jedis](https://github.com/xetorthio/jedis) — klient Java dla usługi Redis

W tym samouczku używany jest klient Jedis, ale można użyć dowolnego klienta Java wymienionego pod adresem [http://redis.io/clients](http://redis.io/clients).

## <a name="create-a-redis-cache-on-azure"></a>Tworzenie pamięci podręcznej Redis na platformie Azure
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-the-host-name-and-access-keys"></a>Pobieranie nazwy hosta i kluczy dostępu
[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

## <a name="enable-the-nonssl-endpoint"></a>Włączanie punktu końcowego bez obsługi protokołu SSL
Niektórzy klienci Redis nie obsługują protokołu SSL, a domyślnie [port nieobsługujący protokołu SSL jest wyłączony dla nowych wystąpień pamięci podręcznej Redis Azure](cache-configure.md#access-ports). W czasie tworzenia tego dokumentu klient [Jedis](https://github.com/xetorthio/jedis) nie obsługuje protokołu SSL. 

[!INCLUDE [redis-cache-create](../../includes/redis-cache-non-ssl-port.md)]

## <a name="add-something-to-the-cache-and-retrieve-it"></a>Dodawanie elementu do pamięci podręcznej i pobieranie go
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


## <a name="next-steps"></a>Następne kroki
* [Włącz diagnostykę pamięci podręcznej](https://msdn.microsoft.com/library/azure/dn763945.aspx#EnableDiagnostics), aby móc [monitorować](https://msdn.microsoft.com/library/azure/dn763945.aspx) jej kondycję.
* Przeczytaj oficjalną [dokumentację magazynu Redis](http://redis.io/documentation).




<!--HONumber=Nov16_HO2-->


