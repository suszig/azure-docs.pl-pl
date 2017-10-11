---
title: "Jak skalować pamięć podręczna Azure Redis | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak skalować swoich wystąpień w pamięci podręcznej Redis Azure"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 350db214-3b7c-4877-bd43-fef6df2db96c
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 04/11/2017
ms.author: sdanie
ms.openlocfilehash: 91b3580491a1e3504a3891b66606a9bd18c0638f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-scale-azure-redis-cache"></a>Jak skalować pamięć podręczna Azure Redis
Pamięć podręczna Redis Azure ma inną pamięci podręcznej oferty, które zapewniają elastyczność w wyborze rozmiar pamięci podręcznej i funkcje. Po utworzeniu pamięci podręcznej można skalować rozmiar i warstwę cenową pamięci podręcznej w przypadku zmiany wymagań aplikacji. W tym artykule przedstawiono sposób skalowania pamięci podręcznej w portalu Azure i przy użyciu narzędzi, takich jak Azure PowerShell i interfejsu wiersza polecenia Azure.

## <a name="when-to-scale"></a>Kiedy skalować
Można użyć [monitorowania](cache-how-to-monitor.md) funkcje pamięci podręcznej Redis Azure do monitorowania kondycji i wydajności pamięci podręcznej i ułatwić określenie, kiedy skalować pamięci podręcznej. 

Można monitorować następujące metryki w celu określenia, jeśli potrzebujesz do skalowania.

* Obciążenie serwera redis
* Użycie pamięci
* Przepustowość sieci
* Użycie procesora CPU

Jeśli okaże się, czy pamięć podręczna jest już spełniający wymagania aplikacji, można skalować do pamięci podręcznej większy lub mniejszy cenowym, które jest odpowiednie dla aplikacji. Aby uzyskać więcej informacji na temat określania pamięci podręcznej, które cenowym do użycia, zobacz [jakie oferty pamięć podręczna Redis i rozmiar użyć](cache-faq.md#what-redis-cache-offering-and-size-should-i-use).

## <a name="scale-a-cache"></a>Skala pamięci podręcznej
Aby skalować pamięć podręczną [przejdź do pamięci podręcznej](cache-configure.md#configure-redis-cache-settings) w [portalu Azure](https://portal.azure.com) i kliknij przycisk **skali** z **zasobów menu**.

![Skalowanie](./media/cache-how-to-scale/redis-cache-scale-menu.png)

Wybierz żądaną warstwę cenową z **wybierz warstwę cenową** bloku i kliknij przycisk **wybierz**.

![Warstwa cenowa][redis-cache-pricing-tier-blade]


Można skalować do innej warstwy cenowej z następującymi ograniczeniami:

* Nie można skalować z wyższej warstwy cenowej do dolnej warstwy cenowej.
  * Nie można skalować z **Premium** pamięci podręcznej w dół do **standardowe** lub **podstawowe** pamięci podręcznej.
  * Nie można skalować z **standardowe** pamięci podręcznej w dół do **podstawowe** pamięci podręcznej.
* Możesz skalować z **podstawowe** pamięci podręcznej do **standardowe** pamięci podręcznej, ale nie można zmienić rozmiar w tym samym czasie. Jeśli potrzebujesz zmienić rozmiar, należy na żądany rozmiar kolejnych operacji skalowania.
* Nie można skalować z **podstawowe** bezpośrednio do pamięci podręcznej **Premium** pamięci podręcznej. Należy skalować z **podstawowe** do **standardowe** w jednej operacji skalowania, a następnie z **standardowe** do **Premium** w kolejnych operacji skalowania.
* Nie można skalować z większego rozmiaru w dół do **C0 (250 MB)** rozmiar.
 
Gdy pamięć podręczna jest skalowanie do nowej warstwy cenowej **skalowanie** stan jest wyświetlany w **pamięci podręcznej Redis** bloku.

![Skalowanie][redis-cache-scaling]

Po zakończeniu skalowanie stan zmieni się z **skalowanie** do **systemem**.

## <a name="how-to-automate-a-scaling-operation"></a>Jak zautomatyzować operacji skalowania
Oprócz skalowania swoich wystąpień w pamięci podręcznej w portalu Azure, można skalować przy użyciu poleceń cmdlet programu PowerShell, interfejsu wiersza polecenia Azure i przy użyciu programu Microsoft Azure Management bibliotek (MAML). 

* [Skala przy użyciu programu PowerShell](#scale-using-powershell)
* [Skala przy użyciu wiersza polecenia platformy Azure](#scale-using-azure-cli)
* [Przy użyciu MAML skali](#scale-using-maml)

### <a name="scale-using-powershell"></a>Skala przy użyciu programu PowerShell
Możesz skalować swoich wystąpień pamięci podręcznej Redis Azure przy użyciu programu PowerShell przy użyciu [AzureRmRedisCache zestaw](https://msdn.microsoft.com/library/azure/mt634518.aspx) polecenia cmdlet podczas `Size`, `Sku`, lub `ShardCount` są modyfikowane właściwości. Poniższy przykład przedstawia sposób skalowania pamięci podręcznej o nazwie `myCache` do 2,5 GB pamięci podręcznej. 

    Set-AzureRmRedisCache -ResourceGroupName myGroup -Name myCache -Size 2.5GB

Aby uzyskać więcej informacji na temat skalowania przy użyciu programu PowerShell, zobacz [można skalować pamięć podręczną Redis przy użyciu programu Powershell](cache-howto-manage-redis-cache-powershell.md#scale).

### <a name="scale-using-azure-cli"></a>Skala przy użyciu wiersza polecenia platformy Azure
Aby skalować swoich wystąpień pamięci podręcznej Redis Azure za pomocą interfejsu wiersza polecenia Azure, należy wywołać `azure rediscache set` poleceń i podaj żądane zmiany w konfiguracji obejmujących nowy rozmiar, jednostki sku lub rozmiar klastra, w zależności od żądanej operacji skalowania.

Aby uzyskać więcej informacji na temat skalowania z wiersza polecenia platformy Azure, zobacz [zmienić ustawienia istniejących pamięci podręcznej Redis](cache-manage-cli.md#scale).

### <a name="scale-using-maml"></a>Przy użyciu MAML skali
Aby skalować pamięć podręczna Redis Azure wystąpienia przy użyciu [Microsoft Azure Management bibliotek (MAML)](http://azure.microsoft.com/updates/management-libraries-for-net-release-announcement/), wywołaj `IRedisOperations.CreateOrUpdate` — metoda i Przekaż nowy rozmiar `RedisProperties.SKU.Capacity`.

    static void Main(string[] args)
    {
        // For instructions on getting the access token, see
        // https://azure.microsoft.com/documentation/articles/cache-configure/#access-keys
        string token = GetAuthorizationHeader();

        TokenCloudCredentials creds = new TokenCloudCredentials(subscriptionId,token);

        RedisManagementClient client = new RedisManagementClient(creds);
        var redisProperties = new RedisProperties();

        // To scale, set a new size for the redisSKUCapacity parameter.
        redisProperties.Sku = new Sku(redisSKUName,redisSKUFamily,redisSKUCapacity);
        redisProperties.RedisVersion = redisVersion;
        var redisParams = new RedisCreateOrUpdateParameters(redisProperties, redisCacheRegion);
        client.Redis.CreateOrUpdate(resourceGroupName,cacheName, redisParams);
    }

Aby uzyskać więcej informacji, zobacz [zarządzania pamięcią podręczną Redis przy użyciu MAML](https://github.com/rustd/RedisSamples/tree/master/ManageCacheUsingMAML) próbki.

## <a name="scaling-faq"></a>Skalowanie — często zadawane pytania
Poniższa lista zawiera odpowiedzi na często zadawane pytania dotyczące pamięci podręcznej Redis Azure skalowania.

* [Aby z lub w pamięci podręcznej Premium I skalowania?](#can-i-scale-to-from-or-within-a-premium-cache)
* [Po skalowania, muszę zmienić klucze nazwy lub dostępu do pamięci podręcznej?](#after-scaling-do-i-have-to-change-my-cache-name-or-access-keys)
* [Jak działa skalowania?](#how-does-scaling-work)
* [Spowoduje utratę danych z mojej pamięci podręcznej podczas skalowania?](#will-i-lose-data-from-my-cache-during-scaling)
* [Jest niestandardowe baz danych zmodyfikowane ustawienie podczas skalowania?](#is-my-custom-databases-setting-affected-during-scaling)
* [Moje pamięci podręcznej będzie dostępna podczas skalowania?](#will-my-cache-be-available-during-scaling)
* [Operacje, które nie są obsługiwane](#operations-that-are-not-supported)
* [Jak długo skalowanie podąża?](#how-long-does-scaling-take)
* [Jak sprawdzić, kiedy jest ukończone skalowania?](#how-can-i-tell-when-scaling-is-complete)

### <a name="can-i-scale-to-from-or-within-a-premium-cache"></a>Aby z lub w pamięci podręcznej Premium I skalowania?
* Nie można skalować z **Premium** pamięci podręcznej w dół do **podstawowe** lub **standardowe** warstwy cenowej.
* Możesz skalować z jednego **Premium** pamięci podręcznej cenowym do innego.
* Nie można skalować z **podstawowe** bezpośrednio do pamięci podręcznej **Premium** pamięci podręcznej. Użytkownik musi najpierw skali: od **podstawowe** do **standardowe** w jednej operacji skalowania, a następnie z **standardowe** do **Premium** w kolejnych operacji skalowania.
* Jeśli włączono klaster podczas tworzenia Twojej **Premium** pamięci podręcznej, możesz [zmienić rozmiar klastra](cache-how-to-premium-clustering.md#cluster-size). Jeśli pamięć podręczna została utworzona bez klastra włączone, nie można skonfigurować klaster w późniejszym czasie.
  
  Aby uzyskać więcej informacji, zobacz temat [Konfigurowanie usługi klastrowania dla usługi Azure Redis Cache w warstwie Premium](cache-how-to-premium-clustering.md).

### <a name="after-scaling-do-i-have-to-change-my-cache-name-or-access-keys"></a>Po skalowania, muszę zmienić klucze nazwy lub dostępu do pamięci podręcznej?
Nie, nazwa pamięci podręcznej, a klucze nie uległy zmianie podczas operacji skalowania.

### <a name="how-does-scaling-work"></a>Jak działa skalowania?
* Gdy **podstawowe** skalowania do innego rozmiaru pamięci podręcznej, zostanie zamknięta i nową pamięć podręczną zostanie zainicjowana przy użyciu nowego rozmiaru. W tym czasie pamięci podręcznej jest niedostępna i nie zostały utracone wszystkie dane w pamięci podręcznej.
* Gdy **podstawowe** skalowania do pamięci podręcznej **standardowe** pamięci podręcznej, zainicjowaniu obsługi pamięci podręcznej repliki, a dane są kopiowane z głównej pamięci podręcznej w pamięci podręcznej repliki. Pamięci podręcznej pozostaje dostępna w trakcie skalowania.
* Gdy **standardowe** pamięci podręcznej jest skalowana do innego rozmiaru lub do **Premium** pamięci podręcznej, jednej z replik jest zamknięte i ponownie zainicjować obsługi administracyjnej do nowego rozmiaru i dane przesyłane przez, a następnie innej repliki wykonuje trybu failover przed jej ponownym zainicjowaniu obsługi, podobny do procesu, która występuje podczas awarii jednego z węzłów pamięci podręcznej.

### <a name="will-i-lose-data-from-my-cache-during-scaling"></a>Spowoduje utratę danych z mojej pamięci podręcznej podczas skalowania?
* Gdy **podstawowe** skalowania do nowego rozmiaru pamięci podręcznej, wszystkie dane zostaną utracone i pamięci podręcznej jest niedostępny podczas operacji skalowania.
* Gdy **podstawowe** skalowania do pamięci podręcznej **standardowe** pamięci podręcznej, dane w pamięci podręcznej zwykle są zachowywane.
* Podczas **standardowe** pamięci podręcznej jest skalowana większy rozmiar lub warstwy, lub **Premium** skalowania do większy rozmiar pamięci podręcznej, wszystkie dane zwykle są zachowywane. Podczas skalowania **standardowe** lub **Premium** pamięci podręcznej w dół mniejszy rozmiar, dane mogą zostać utracone w zależności od tego, jak dużo danych jest w pamięci podręcznej związane z nowy rozmiar podczas jego skalowania. Jeśli dane zostaną utracone podczas skalowania, klucze są wykluczony przy użyciu [allkeys lru](http://redis.io/topics/lru-cache) zasady wykluczania. 

### <a name="is-my-custom-databases-setting-affected-during-scaling"></a>Jest niestandardowe baz danych zmodyfikowane ustawienie podczas skalowania?
Niektóre warstw cenowych mają różne [baz danych limity](cache-configure.md#databases), więc istnieją pewne zagadnienia dotyczące skalowania Jeśli skonfigurowana wartość niestandardową dla `databases` ustawienie podczas tworzenia pamięci podręcznej.

* Podczas skalowania do warstwy cenowej o niższej `databases` limit niż warstwa bieżąca:
  * Jeśli używasz domyślną liczbę `databases` czyli 16 dla wszystkich warstw cenowych zostały utracone żadne dane.
  * Jeśli używasz niestandardowego liczba `databases` który mieści się w granicach na warstwę, do której należy są skalowania, to `databases` ustawienie jest zachowywane i nie zostały utracone żadne dane.
  * Jeśli używasz niestandardowego liczba `databases` przekraczający limit nową warstwę `databases` ustawienie jest obniżona z limitami nową warstwę, a wszystkie dane w usunięte bazy danych zostaną utracone.
* Podczas skalowania do warstwy cenowej z tego samego lub wyższego `databases` limit niż warstwa bieżąca Twojej `databases` ustawienie jest zachowywane i nie zostały utracone żadne dane.

Zwróć uwagę, że chociaż standardowa i Premium pamięci podręcznych SLA 99,9%, dostępności, nie umowy dotyczącej poziomu usług dla utraty danych.

### <a name="will-my-cache-be-available-during-scaling"></a>Moje pamięci podręcznej będzie dostępna podczas skalowania?
* **Standardowe** i **Premium** pamięci podręcznych pozostają dostępne podczas operacji skalowania.
* **Podstawowe** pamięci podręcznych są w trybie offline podczas skalowania na inny rozmiar operacji, ale pozostają dostępne podczas skalowania z **podstawowe** do **standardowe**.

### <a name="operations-that-are-not-supported"></a>Operacje, które nie są obsługiwane
* Nie można skalować z wyższej warstwy cenowej do dolnej warstwy cenowej.
  * Nie można skalować z **Premium** pamięci podręcznej w dół do **standardowe** lub **podstawowe** pamięci podręcznej.
  * Nie można skalować z **standardowe** pamięci podręcznej w dół do **podstawowe** pamięci podręcznej.
* Możesz skalować z **podstawowe** pamięci podręcznej do **standardowe** pamięci podręcznej, ale nie można zmienić rozmiar w tym samym czasie. Jeśli potrzebujesz zmienić rozmiar, należy na żądany rozmiar kolejnych operacji skalowania.
* Nie można skalować z **podstawowe** bezpośrednio do pamięci podręcznej **Premium** pamięci podręcznej. Użytkownik musi najpierw skali: od **podstawowe** do **standardowe** w jednej operacji skalowania, a następnie z **standardowe** do **Premium** w kolejnych operacji skalowania.
* Nie można skalować z większego rozmiaru w dół do **C0 (250 MB)** rozmiar.

W przypadku niepowodzenia operacji skalowania usługi spróbuje przywrócić działanie i pamięci podręcznej zostanie przywrócony oryginalny rozmiar.

### <a name="how-long-does-scaling-take"></a>Jak długo skalowanie podąża?
Skalowanie trwa około 20 minut w zależności od tego, jak dużo danych jest w pamięci podręcznej.

### <a name="how-can-i-tell-when-scaling-is-complete"></a>Jak sprawdzić, kiedy jest ukończone skalowania?
W portalu Azure może zobaczyć trwających operacji skalowania. Po zakończeniu skalowanie zmiany stanu pamięci podręcznej na **systemem**.

<!-- IMAGES -->

[redis-cache-pricing-tier-blade]: ./media/cache-how-to-scale/redis-cache-pricing-tier-blade.png

[redis-cache-scaling]: ./media/cache-how-to-scale/redis-cache-scaling.png



