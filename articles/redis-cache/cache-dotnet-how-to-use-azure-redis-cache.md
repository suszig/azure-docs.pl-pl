---
title: "Jak używać usługi Azure Redis Cache | Microsoft Docs"
description: "Informacje na temat zwiększania wydajności aplikacji Azure za pomocą usługi Azure Redis Cache"
services: redis-cache,app-service
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: c502f74c-44de-4087-8303-1b1f43da12d5
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/25/2016
ms.author: sdanie
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 209d4f610f0d5199d9018c506acef3b7328478ef


---
# <a name="how-to-use-azure-redis-cache"></a>Jak używać usługi Azure Redis Cache
> [!div class="op_single_selector"]
> * [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [ASP.NET](cache-web-app-howto.md)
> * [Node.js](cache-nodejs-get-started.md)
> * [Java](cache-java-get-started.md)
> * [Python](cache-python-get-started.md)
> 
> 

 W tym przewodniku przedstawiono, jak rozpocząć pracę z usługą **Azure Redis Cache**. Usługa Microsoft Azure Redis Cache jest oparta na popularnej pamięci podręcznej Redis typu open source. Umożliwia dostęp do bezpiecznej, dedykowanej pamięci podręcznej Redis, zarządzanej przez firmę Microsoft. Pamięć podręczna utworzona przy użyciu usługi Azure Redis Cache jest dostępna z poziomu dowolnej aplikacji w ramach platformy Microsoft Azure.

Usługa Microsoft Azure Redis Cache jest dostępna w następujących warstwach:

* **Podstawowa** — jeden węzeł. Wiele rozmiarów do 53 GB.
* **Standardowa** — dwa węzły (węzeł podstawowy i węzeł repliki). Wiele rozmiarów do 53 GB. Umowa SLA na poziomie 99,9%.
* **Premium** — dwa węzły (węzeł podstawowy i węzeł repliki) zawierające do 10 fragmentów. Różne rozmiary od 6 GB do 530 GB (skontaktuj się z nami, aby uzyskać więcej informacji). Wszystkie funkcje warstwy Standardowej i dodatkowe funkcje, m.in. obsługa [klastra Redis](cache-how-to-premium-clustering.md), [stanu trwałego pamięci podręcznej Redis](cache-how-to-premium-persistence.md) oraz usługi [Azure Virtual Network](cache-how-to-premium-vnet.md). Umowa SLA na poziomie 99,9%.

Poszczególne warstwy różnią się od siebie pod względem funkcji i cen. Informacje dotyczące cen można znaleźć w artykule [Redis Cache — cennik][Redis Cache — cennik].

W tym przewodniku przedstawiono, jak korzystać z klienta [StackExchange.Redis][StackExchange.Redis] przy użyciu kodu C\#. Opisane scenariusze obejmują **tworzenie i konfigurowanie pamięci podręcznej**, **konfigurowanie klientów pamięci podręcznej** oraz **dodawanie i usuwanie obiektów z pamięci podręcznej**. Więcej informacji na temat korzystania z usługi Azure Redis Cache znajduje się w sekcji [Następne kroki][Następne kroki]. Samouczek krok po kroku dotyczący tworzenia aplikacji sieci Web ASP.NET MVC z użyciem pamięci podręcznej Redis znajduje się w artykule [Tworzenie aplikacji sieci Web z użyciem pamięci podręcznej Redis](cache-web-app-howto.md).

<a name="getting-started-cache-service"></a>

## <a name="get-started-with-azure-redis-cache"></a>Rozpoczęcie pracy z usługą Azure Redis Cache
Rozpoczęcie pracy z usługą Azure Redis Cache jest proste. Aby rozpocząć, należy aprowizować i skonfigurować pamięć podręczną. Następnie należy skonfigurować klientów pamięci podręcznej, aby mogli uzyskać dostęp do pamięci podręcznej. Po skonfigurowaniu klientów pamięci podręcznej można rozpocząć pracę.

* [Tworzenie pamięci podręcznej][Tworzenie pamięci podręcznej]
* [Konfigurowanie klientów pamięci podręcznej][Konfigurowanie klientów pamięci podręcznej]

<a name="create-cache"></a>

## <a name="create-a-cache"></a>Tworzenie pamięci podręcznej
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

### <a name="to-access-your-cache-after-its-created"></a>Aby uzyskać dostęp do pamięci podręcznej po jej utworzeniu
[!INCLUDE [redis-cache-create](../../includes/redis-cache-browse.md)]

Więcej informacji na temat konfigurowania pamięci podręcznej znajduje się w temacie [How to configure Azure Redis Cache](cache-configure.md) (Konfigurowanie usługi Azure Redis Cache).

<a name="NuGet"></a>

## <a name="configure-the-cache-clients"></a>Konfigurowanie klientów pamięci podręcznej
[!INCLUDE [redis-cache-configure](../../includes/redis-cache-configure-stackexchange-redis-nuget.md)]

Po skonfigurowaniu projektu klienta do buforowania będzie można pracować z pamięcią podręczną przy użyciu metod opisanych w poniższych sekcjach.

<a name="working-with-caches"></a>

## <a name="working-with-caches"></a>Praca z pamięciami podręcznymi
W tej sekcji opisano sposób wykonywania typowych zadań z pamięcią podręczną.

* [Łączenie z pamięcią podręczną][Łączenie z pamięcią podręczną]
* [Dodawanie i pobieranie obiektów z pamięci podręcznej][Dodawanie i pobieranie obiektów z pamięci podręcznej]
* [Praca z obiektami platformy .NET w pamięci podręcznej](#work-with-net-objects-in-the-cache)

<a name="connect-to-cache"></a>

## <a name="connect-to-the-cache"></a>Łączenie z pamięcią podręczną
Aby programowo pracować z pamięcią podręczną, potrzebujesz odwołania do pamięci podręcznej. Dodaj poniższy kod na początku każdego pliku, z którego chcesz użyć klienta StackExchange.Redis, aby uzyskać dostęp do pamięci podręcznej Redis Azure.

    using StackExchange.Redis;

> [!NOTE]
> Klient StackExchange.Redis wymaga programu .NET Framework 4 lub jego nowszej wersji.
> 
> 

Połączenie z usługą Azure Redis Cache jest zarządzane przez klasę `ConnectionMultiplexer`. Ta klasa jest przeznaczona do współużytkowania i ponownego użycia w aplikacji klienta i nie trzeba jej tworzyć w oparciu o operację. 

Aby połączyć się z usługą Azure Redis Cache i otrzymać wystąpienie połączonej klasy `ConnectionMultiplexer`, wywołaj statyczną metodę `Connect` i przekaż punkt końcowy pamięci podręcznej oraz klucz jak w przykładzie poniżej. Użyj klucza wygenerowanego w portalu Azure jako parametru hasła.

    ConnectionMultiplexer connection = ConnectionMultiplexer.Connect("contoso5.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");

> [!IMPORTANT]
> Ostrzeżenie: Nigdy nie przechowuj poświadczeń w kodzie źródłowym. Tutaj zostały one pokazane w kodzie źródłowym, aby uprościć przykład. Informacje na temat przechowywania poświadczeń znajdują się w artykule [How Application Strings and Connection Strings Work][How Application Strings and Connection Strings Work] (Sposób działania ciągów aplikacji i parametrów połączenia).
> 
> 

Jeśli nie chcesz używać protokołu SSL, ustaw wartość `ssl=false` lub pomiń parametr `ssl`.

> [!NOTE]
> Port bez obsługi protokołu SSL jest domyślnie wyłączony w przypadku nowych pamięci podręcznych. Instrukcje dotyczące włączania portu bez obsługi protokołu SSL znajdują się w sekcji [Access Ports](cache-configure.md#access-ports) (Porty dostępu).
> 
> 

Jednym z rozwiązań w zakresie udostępniania wystąpienia klasy `ConnectionMultiplexer` w aplikacji jest korzystanie z właściwości statycznej, która zwraca połączone wystąpienie podobnie jak w poniższym przykładzie. Jest to bezpieczny wątkowo sposób na inicjowanie tylko jednego połączonego wystąpienia klasy `ConnectionMultiplexer` W tych przykładach parametr `abortConnect` ma wartość false, co oznacza, że wystąpienie zostanie wykonane pomyślnie, nawet jeśli połączenie z usługą Azure Redis Cache nie zostanie nawiązane. Kluczowa funkcja klasy `ConnectionMultiplexer` polega na automatycznym przywracaniu łączności z pamięcią podręczną po rozwiązaniu problemu z siecią lub usunięciu innych przyczyn.

    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        return ConnectionMultiplexer.Connect("contoso5.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");
    });

    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }

Więcej informacji na temat opcji zaawansowanej konfiguracji połączenia znajduje się w opisie [modelu konfiguracji StackExchange.Redis][modelu konfiguracji StackExchange.Redis].

[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

Po nawiązaniu połączenia zwróć odwołanie do bazy danych pamięci podręcznej Redis przez wywołanie metody `ConnectionMultiplexer.GetDatabase`. Obiekt zwracany z metody `GetDatabase` jest lekkim obiektem przekazującym i nie wymaga przechowywania.

    // Connection refers to a property that returns a ConnectionMultiplexer
    // as shown in the previous example.
    IDatabase cache = Connection.GetDatabase();

    // Perform cache operations using the cache object...
    // Simple put of integral data types into the cache
    cache.StringSet("key1", "value");
    cache.StringSet("key2", 25);

    // Simple get of data types from the cache
    string key1 = cache.StringGet("key1");
    int key2 = (int)cache.StringGet("key2");

Teraz gdy wiesz, jak połączyć się z wystąpieniem usługi Azure Redis Cache i zwrócić odwołanie do bazy danych pamięci podręcznej, przyjrzyjmy się pracy z pamięcią podręczną.

<a name="add-object"></a>

## <a name="add-and-retrieve-objects-from-the-cache"></a>Dodawanie i pobieranie obiektów z pamięci podręcznej
Elementy można zapisywać do pamięci podręcznej i pobierać z niej za pomocą metod `StringSet` i `StringGet`.

    // If key1 exists, it is overwritten.
    cache.StringSet("key1", "value1");

    string value = cache.StringGet("key1");

Usługa Redis przechowuje większość danych w formie ciągów Redis, ale ciągi te mogą zawierać wiele typów danych, w tym serializowane dane binarne, które mogą być używane podczas przechowywania obiektów platformy .NET w pamięci podręcznej.

Podczas wywoływania metody `StringGet`, jeśli obiekt istnieje, jest zwracany, a jeśli nie, zwracana jest wartość `null`. W takim przypadku można pobrać wartość z żądanego źródła danych i zapisać ją w pamięci podręcznej do późniejszego użytku. Nazywamy to wzorcem z odkładaniem do pamięci podręcznej.

    string value = cache.StringGet("key1");
    if (value == null)
    {
        // The item keyed by "key1" is not in the cache. Obtain
        // it from the desired data source and add it to the cache.
        value = GetValueFromDataSource();

        cache.StringSet("key1", value);
    }

Aby określić wygaśnięcie elementu w pamięci podręcznej, użyj parametru `TimeSpan` metody `StringSet`.

    cache.StringSet("key1", "value1", TimeSpan.FromMinutes(90));

## <a name="work-with-net-objects-in-the-cache"></a>Praca z obiektami platformy .NET w pamięci podręcznej
Usługa Azure Redis Cache może buforować obiekty platformy .NET oraz pierwotne typy danych, ale zanim będzie możliwe buforowanie obiektu platformy .NET, trzeba go serializować. Odpowiedzialność za tę operację spoczywa na deweloperze aplikacji, który ma możliwość wybrania serializatora.

Prostym sposobem na wykonywanie serializacji obiektów jest użycie metod serializacji `JsonConvert` w środowisku [Newtonsoft.Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/8.0.1-beta1) oraz serializacja do i z formatu JSON. W poniższym przykładzie pokazano metody get i set używające wystąpienia obiektu `Employee`.

    class Employee
    {
        public int Id { get; set; }
        public string Name { get; set; }

        public Employee(int EmployeeId, string Name)
        {
            this.Id = EmployeeId;
            this.Name = Name;
        }
    }

    // Store to cache
    cache.StringSet("e25", JsonConvert.SerializeObject(new Employee(25, "Clayton Gragg")));

    // Retrieve from cache
    Employee e25 = JsonConvert.DeserializeObject<Employee>(cache.StringGet("e25"));

<a name="next-steps"></a>

## <a name="next-steps"></a>Następne kroki
Teraz, kiedy znasz już podstawy, skorzystaj z poniższych linków i dowiedz się więcej na temat usługi Azure Redis Cache.

* Sprawdź dostawców programu ASP.NET dla usługi Azure Redis Cache.
  * [Dostawca stanu sesji usługi Azure Redis](cache-aspnet-session-state-provider.md)
  * [Dostawca wyjściowej pamięci podręcznej programu ASP.NET w usłudze Azure Redis Cache](cache-aspnet-output-cache-provider.md)
* [Włącz diagnostykę pamięci podręcznej](cache-how-to-monitor.md#enable-cache-diagnostics), aby móc [monitorować](cache-how-to-monitor.md) jej kondycję. Możesz wyświetlać metryki w witrynie Azure Portal oraz [pobierać i przeglądać](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) je przy użyciu wybranych przez siebie narzędzi.
* Zapoznaj się z [dokumentacją dotyczącą klienta pamięci podręcznej StackExchange.Redis][dokumentacją dotyczącą klienta pamięci podręcznej StackExchange.Redis].
  * Dostęp do usługi Azure Redis Cache można uzyskać z wielu klientów Redis i przy użyciu wielu języków programowania. Więcej informacji znajduje się na stronie [http://redis.io/clients][http://redis.io/clients].
* Usługi Azure Redis Cache można również używać z usługami i narzędziami innych firm, takimi jak Redsmin i Redis Desktop Manager.
  * Więcej informacji na temat usługi Redsmin znajduje się w artykule [How to retrieve an Azure Redis connection string and use it with Redsmin][How to retrieve an Azure Redis connection string and use it with Redsmin] (Jak pobrać parametry połączenia usługi Azure Redis Cache i używać ich w usłudze Redsmin).
  * Dostęp do danych w usłudze Azure Redis Cache oraz możliwość ich inspekcji można uzyskać za pomocą graficznego interfejsu użytkownika, używając usługi [RedisDesktopManager](https://github.com/uglide/RedisDesktopManager).
* Zapoznaj się z dokumentacją dotyczącą usługi [redis][redis], przeczytaj o [typach danych usługi redis][typach danych usługi redis] oraz poznaj [piętnastominutowe wprowadzenie do typów danych usługi Redis][piętnastominutowe wprowadzenie do typów danych usługi Redis].

<!-- INTRA-TOPIC LINKS -->
[Następne kroki]: #next-steps
[Wprowadzenie do usługi Azure Redis Cache (film wideo)]: #video
[Co to jest usługa Azure Redis Cache?]: #what-is
[Tworzenie pamięci podręcznej Azure]: #create-cache
[Który rodzaj buforowania jest odpowiedni dla mnie?]: #choosing-cache
[Przygotowanie projektu programu Visual Studio do używania buforowania Azure]: #prepare-vs
[Konfigurowanie aplikacji do użycia buforowania]: #configure-app
[Rozpoczęcie pracy z usługą Azure Redis Cache]: #getting-started-cache-service
[Tworzenie pamięci podręcznej]: #create-cache
[Konfigurowanie pamięci podręcznej]: #enable-caching
[Konfigurowanie klientów pamięci podręcznej]: #NuGet
[Praca z pamięciami podręcznymi]: #working-with-caches
[Łączenie z pamięcią podręczną]: #connect-to-cache
[Dodawanie i pobieranie obiektów z pamięci podręcznej]: #add-object
[Określania daty wygaśnięcia obiektu w pamięci podręcznej]: #specify-expiration
[Przechowywanie stanu sesji programu ASP.NET w pamięci podręcznej]: #store-session


<!-- IMAGES -->


[StackExchangeNuget]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-stackexchange-redis.png

[NuGetMenu]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-manage-nuget-menu.png

[CacheProperties]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-properties.png

[Zarządzanie kluczami]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-manage-keys.png

[SessionStateNuGet]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-session-state-provider.png

[BrowseCaches]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-browse-caches.png

[Pamięci podręczne]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-caches.png







<!-- LINKS -->
[http://redis.io/clients]: http://redis.io/clients
[Programowanie w innych językach dla usługi Azure Redis Cache]: http://msdn.microsoft.com/library/azure/dn690470.aspx
[How to retrieve an Azure Redis connection string and use it with Redsmin]: https://redsmin.uservoice.com/knowledgebase/articles/485711-how-to-connect-redsmin-to-azure-redis-cache
[Dostawca stanu sesji usługi Azure Redis]: http://go.microsoft.com/fwlink/?LinkId=398249
[Porady: konfigurowanie programowe klienta pamięci podręcznej]: http://msdn.microsoft.com/library/windowsazure/gg618003.aspx
[Dostawca stanu sesji dla pamięci podręcznej Azure]: http://go.microsoft.com/fwlink/?LinkId=320835
[Pamięć podręczna Azure AppFabric: stan sesji buforowania]: http://www.microsoft.com/showcase/details.aspx?uuid=87c833e9-97a9-42b2-8bb1-7601f9b5ca20
[Dostawca wyjściowej pamięci podręcznej dla pamięci podręcznej Azure]: http://go.microsoft.com/fwlink/?LinkId=320837
[Azure Shared Caching]: http://msdn.microsoft.com/library/windowsazure/gg278356.aspx
[Blog zespołu]: http://blogs.msdn.com/b/windowsazure/
[Buforowanie Azure]: http://www.microsoft.com/showcase/Search.aspx?phrase=azure+caching
[Jak skonfigurować rozmiary maszyn wirtualnych]: http://go.microsoft.com/fwlink/?LinkId=164387
[Kwestie dotyczące planowania pojemności pamięci podręcznej Azure]: http://go.microsoft.com/fwlink/?LinkId=320167
[Buforowanie Azure]: http://go.microsoft.com/fwlink/?LinkId=252658
[Porady: deklaratywne ustawianie buforowania strony programu ASP.NET]: http://msdn.microsoft.com/library/zd1ysf1y.aspx
[Porady: programowe ustawianie buforowania strony]: http://msdn.microsoft.com/library/z852zf6b.aspx
[Konfigurowanie pamięci podręcznej w usłudze Azure Redis Cache]: http://msdn.microsoft.com/library/azure/dn793612.aspx

[modelu konfiguracji StackExchange.Redis]: http://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Configuration.md

[Praca z obiektami platformy .NET w pamięci podręcznej]: http://msdn.microsoft.com/library/dn690521.aspx#Objects


[Instalacja Menedżera pakietów NuGet]: http://go.microsoft.com/fwlink/?LinkId=240311
[Redis Cache — cennik]: http://www.windowsazure.com/pricing/details/cache/
[Azure Portal]: https://portal.azure.com/

[Omówienie usługi Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=320830
[Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=398247

[Migracja do usługi Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=317347
[Przykłady użycia usługi Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=320840
[Używanie grup zasobów do zarządzania zasobami platformy Azure]: ../azure-resource-manager/resource-group-overview.md

[StackExchange.Redis]: http://github.com/StackExchange/StackExchange.Redis
[dokumentacją dotyczącą klienta pamięci podręcznej StackExchange.Redis]: http://github.com/StackExchange/StackExchange.Redis#documentation

[Redis]: http://redis.io/documentation
[typach danych usługi redis]: http://redis.io/topics/data-types
[piętnastominutowe wprowadzenie do typów danych usługi Redis]: http://redis.io/topics/data-types-intro

[How Application Strings and Connection Strings Work]: http://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/





<!--HONumber=Nov16_HO2-->


