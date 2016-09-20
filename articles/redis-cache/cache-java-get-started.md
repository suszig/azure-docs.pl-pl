<properties
   pageTitle="Jak używać usługi Azure Redis Cache za pomocą języka Java | Microsoft Azure"
    description="Rozpoczęcie pracy z usługą Azure Redis Cache za pomocą języka Java"
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

# Jak używać pamięci podręcznej Redis Azure za pomocą języka Java

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [ASP.NET](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Usługa Azure Redis Cache umożliwia dostęp do dedykowanej pamięci podręcznej Redis, zarządzanej przez firmę Microsoft. Pamięć podręczna jest dostępna z poziomu dowolnej aplikacji na platformie Microsoft Azure.

W tym temacie opisano sposób rozpoczęcia pracy z usługą Azure Redis Cache za pomocą języka Java.

## Wymagania wstępne

[Jedis](https://github.com/xetorthio/jedis) — klient Java dla usługi Redis

W tym samouczku używany jest klient Jedis, ale można użyć dowolnego klienta Java wymienionego pod adresem [http://redis.io/clients](http://redis.io/clients).

## Tworzenie pamięci podręcznej Redis na platformie Azure

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## Pobieranie nazwy hosta i kluczy dostępu

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]


## Włączanie punktu końcowego bez obsługi protokołu SSL

Niektórzy klienci Redis nie obsługują protokołu SSL, a domyślnie [port nieobsługujący protokołu SSL jest wyłączony dla nowych wystąpień pamięci podręcznej Redis Azure](cache-configure.md#access-ports). W czasie tworzenia tego dokumentu klient [Jedis](https://github.com/xetorthio/jedis) nie obsługuje protokołu SSL. 

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-non-ssl-port.md)]




## Dodawanie elementu do pamięci podręcznej i pobieranie go

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


## Następne kroki

- [Włącz diagnostykę pamięci podręcznej](https://msdn.microsoft.com/library/azure/dn763945.aspx#EnableDiagnostics), aby móc [monitorować](https://msdn.microsoft.com/library/azure/dn763945.aspx) jej kondycję.
- Przeczytaj oficjalną [dokumentację magazynu Redis](http://redis.io/documentation).




<!--HONumber=sep16_HO1-->


