<properties
    pageTitle="Łączenie aplikacji sieci Web w usłudze Azure App Service z pamięcią podręczną Redis za pośrednictwem protokołu Memcache | Microsoft Azure"
    description="Łączenie aplikacji sieci Web w Usłudze aplikacji Azure z pamięcią podręczną Redis przy użyciu protokołu Memcache"
    services="app-service\web"
    documentationCenter="php"
    authors="SyntaxC4"
    manager="wpickett"
    editor="riande"/>

<tags
    ms.service="app-service-web"
    ms.devlang="php"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="windows"
    ms.workload="na"
    ms.date="02/29/2016"
    ms.author="cfowler"/>

# Łączenie aplikacji sieci Web w usłudze Azure App Service z pamięcią podręczną Redis za pośrednictwem protokołu Memcache

W tym artykule dowiesz się, jak połączyć aplikację sieci Web WordPress w [usłudze Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) z [pamięcią podręczną Redis Azure][12] przy użyciu protokołu [Memcache][13]. Jeśli masz istniejącą aplikację sieci Web, która korzysta z serwera Memcached do buforowania w pamięci, możesz przeprowadzić jej migrację do usługi Azure App Service i użyć naszego rozwiązania do obsługi pamięci podręcznej na platformie Microsoft Azure z niewielkimi zmianami w kodzie aplikacji lub nawet bez wprowadzania żadnych zmian. Ponadto możesz skorzystać ze swojego doświadczenia z używaniem protokołu Memcache do tworzenia wysoce skalowalnych, rozproszonych aplikacji w usłudze Azure App Service przy użyciu pamięci podręcznej Redis Azure do buforowania w pamięci, jednocześnie korzystając z popularnych struktur aplikacji, takich jak .NET, PHP, Node.js, Java i Python.  

Aplikacje Web Apps w usłudze App Service umożliwiają wykorzystanie tego scenariusza aplikacji z podkładką Memcache aplikacji Web Apps, która jest lokalnym serwerem Memcached działającym jako serwer proxy Memcache do buforowania wywołań w pamięci podręcznej Redis Azure. Dzięki temu dowolna aplikacja komunikująca się za pośrednictwem protokołu Memcache może buforować dane w pamięci podręcznej Redis. Ta podkładka Memcache działa na poziomie protokołu, dlatego może być użyta przez dowolną aplikację lub strukturę aplikacji, jeśli tylko nawiązuje ona komunikację za pośrednictwem protokołu Memcache.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## Wymagania wstępne

Podkładki Memcache aplikacji Web Apps można użyć z dowolną aplikacją, jeśli aplikacja komunikuje się przy użyciu protokołu Memcache. W tym przykładzie aplikacja referencyjna jest skalowalną witryną WordPress, którą można uzyskać z platformy Azure Marketplace.

Wykonaj kroki opisane w tych artykułach:

* [Provision an instance of the Azure Redis Cache Service] (Aprowizowanie wystąpienia usługi pamięci podręcznej Redis Azure)[0]
* [Deploy a Scalable WordPress site in Azure] (Wdrażanie skalowalnej witryny WordPress na platformie Azure)[1]

Po wdrożeniu skalowalnej witryny WordPress i aprowizowaniu wystąpienia pamięci podręcznej Redis Azure można przejść do włączenia podkładki Memcache w aplikacjach Web Apps w usłudze Azure App Service.

## Włączenie podkładki Memcache usługi Web Apps

Aby skonfigurować podkładkę Memcache, musisz utworzyć trzy ustawienia aplikacji. Możesz to zrobić przy użyciu różnych metod, w tym za pomocą [Portalu Azure](http://go.microsoft.com/fwlink/?LinkId=529715), [klasycznego portalu][3], [poleceń cmdlet Azure PowerShell][5] lub [interfejsu wiersza polecenia platformy Azure][5]. Na potrzeby tego wpisu do skonfigurowania ustawień aplikacji użyjemy [Portalu Azure][4]. Następujące wartości można pobierać z bloku **Ustawienia** wystąpienia pamięci podręcznej Redis.

![Blok ustawień pamięci podręcznej Redis Azure](./media/web-sites-connect-to-redis-using-memcache-protocol/1-azure-redis-cache-settings.png)

### Dodawanie ustawienia aplikacji REDIS_HOST

Pierwsze ustawienie aplikacji, które należy utworzyć, to ustawienie **REDIS\_HOST**. To ustawienie określa miejsce docelowe, do którego podkładka przekierowuje informacje o pamięci podręcznej. Wartość wymaganą dla ustawienia aplikacji REDIS_HOST można pobrać z bloku **Właściwości** wystąpienia pamięci podręcznej Redis.

![Nazwa hosta pamięci podręcznej Redis Azure](./media/web-sites-connect-to-redis-using-memcache-protocol/2-azure-redis-cache-hostname.png)

Ustaw klucz ustawienia aplikacji na **REDIS\_HOST** i wartość ustawienia aplikacji na **nazwę hosta** wystąpienia pamięci podręcznej Redis.

![Ustawienie aplikacji sieci Web REDIS_HOST](./media/web-sites-connect-to-redis-using-memcache-protocol/3-azure-website-appsettings-redis-host.png)

### Dodawanie ustawienia aplikacji REDIS_KEY

Drugie ustawienie aplikacji, które należy utworzyć, to ustawienie **REDIS\_KEY**. To ustawienie zapewnia token uwierzytelniania wymagany do bezpiecznego dostępu do wystąpienia pamięci podręcznej Redis. Możesz pobrać wartość wymaganą dla ustawienia aplikacji REDIS_KEY z bloku **Klucze dostępu** wystąpienia pamięci podręcznej Redis.

![Klucz podstawowy pamięci podręcznej Redis Azure](./media/web-sites-connect-to-redis-using-memcache-protocol/4-azure-redis-cache-primarykey.png)

Ustaw klucz ustawienia aplikacji na **REDIS\_KEY** i wartość ustawienia aplikacji na **klucz podstawowy** wystąpienia pamięci podręcznej Redis.

![Ustawienie aplikacji witryny sieci Web Azure REDIS_KEY](./media/web-sites-connect-to-redis-using-memcache-protocol/5-azure-website-appsettings-redis-primarykey.png)

### Dodawanie ustawienia aplikacji MEMCACHESHIM_REDIS_ENABLE

Ostatnie ustawienie aplikacji umożliwia wykorzystanie podkładki Memcache w aplikacjach Web Apps, która używa kluczy REDIS_HOST i REDIS_KEY do łączenia się z pamięcią podręczną Redis Azure i przekierowywania wywołań pamięci podręcznej. Ustaw klucz ustawienia aplikacji na **MEMCACHESHIM\_REDIS\_ENABLE** i wartość na **true**.

![Ustawienie aplikacji sieci Web MEMCACHESHIM_REDIS_ENABLE](./media/web-sites-connect-to-redis-using-memcache-protocol/6-azure-website-appsettings-enable-shim.png)

Po zakończeniu dodawania trzech (3) ustawień aplikacji kliknij przycisk **Zapisz**.

## Włączanie rozszerzenia Memcache dla języka PHP

Aby aplikacja mogła komunikować się z protokołem Memcache, wymaga się zainstalowania rozszerzenia Memcache dla języka PHP — struktury języka dla witryny WordPress.

### Pobieranie rozszerzenia php_memcache

Przejdź do [PECL][6]. W kategorii buforowania kliknij pozycję [memcache][7]. W kolumnie materiałów do pobrania kliknij link biblioteki DLL.

![Witryna sieci Web PHP PECL](./media/web-sites-connect-to-redis-using-memcache-protocol/7-php-pecl-website.png)

Pobierz plik Non-Thread Safe (NTS) x86 dla wersji PHP włączonej w usłudze Web Apps. (Wartość domyślna to PHP 5.4)

![Pakiet Memcache dla witryny sieci Web PHP PECL](./media/web-sites-connect-to-redis-using-memcache-protocol/8-php-pecl-memcache-package.png)

### Włączanie rozszerzenia php_memcache

Po pobraniu pliku wyodrębnij i przekaż plik **php\_memcache.dll** do katalogu **d:\\home\\site\\wwwroot\\bin\\ext\\**. Po przekazaniu pliku php_memcache.dll do aplikacji sieci Web należy włączyć rozszerzenie do środowiska uruchomieniowego języka PHP. Aby włączyć rozszerzenie Memcache w Portalu Azure, otwórz blok **Ustawienia aplikacji** dla aplikacji sieci Web, a następnie dodaj nowe ustawienie aplikacji z kluczem **PHP\_EXTENSIONS** i wartością **bin\\ext\\php_memcache.dll**.


> [AZURE.NOTE] Jeśli aplikacja sieci Web musi załadować wiele rozszerzeń PHP, wartość klucza PHP_EXTENSIONS powinna być listą względnych ścieżek do plików DLL rozdzielonych przecinkami.

![Ustawienie aplikacji sieci Web PHP_EXTENSIONS](./media/web-sites-connect-to-redis-using-memcache-protocol/9-azure-website-appsettings-php-extensions.png)

Po skończeniu kliknij przycisk **Zapisz**.

## Instalowanie wtyczki Memcache WordPress

> [AZURE.NOTE] Możesz również pobrać [wtyczkę Memcached Object Cache](https://wordpress.org/plugins/memcached/) z witryny WordPress.org.

Na stronie wtyczek w witrynie WordPress kliknij przycisk **Add New** (Dodaj nowy).

![Strona wtyczek witryny WordPress](./media/web-sites-connect-to-redis-using-memcache-protocol/10-wordpress-plugin.png)

W polu wyszukiwania wpisz **memcached** i naciśnij klawisz **Enter**.

![Dodawanie nowej wtyczki WordPress](./media/web-sites-connect-to-redis-using-memcache-protocol/11-wordpress-add-new-plugin.png)

Znajdź wtyczkę **Memcached Object Cache** na liście i kliknij przycisk **Install Now** (Zainstaluj teraz).

![WordPress — instalowanie wtyczki Memcache](./media/web-sites-connect-to-redis-using-memcache-protocol/12-wordpress-install-memcache-plugin.png)

### Włączanie wtyczki Memcache WordPress

>[AZURE.NOTE] Postępuj zgodnie z instrukcjami na tym blogu w sekcji [How to enable a Site Extension in Web Apps] (Sposób włączania rozszerzeń witryny w aplikacjach sieci Web)[8], aby zainstalować usługę Visual Studio Team Services.

W pliku `wp-config.php` dodaj następujący kod nad komentarzem o zatrzymaniu edycji pod koniec pliku.

```php
$memcached_servers = array(
    'default' => array('localhost:' . getenv("MEMCACHESHIM_PORT"))
);
```

Po wklejeniu tego kodu narzędzie Monaco automatycznie zapisze dokument.

Następnym krokiem jest włączenie wtyczki object-cache. Odbywa się to poprzez przeciągnięcie i upuszczenie pliku **object-cache.php** z folderu **wp-content/plugins/memcached** do folderu **wp-content**, co włącza funkcję Memcache Object Cache.

![Lokalizowanie wtyczki memcache object-cache.php](./media/web-sites-connect-to-redis-using-memcache-protocol/13-locate-memcache-object-cache-plugin.png)

Teraz, gdy plik **object-cache.php** znajduje się w folderze **wp-content**, funkcja Memcached Object Cache jest włączona.

![Włączanie wtyczki memcache object-cache.php](./media/web-sites-connect-to-redis-using-memcache-protocol/14-enable-memcache-object-cache-plugin.png)

## Sprawdzanie funkcjonowania wtyczki Memcache Object Cache

Wszystkie kroki związane z włączaniem podkładki Memcache aplikacji Web Apps są teraz zakończone. Pozostało już tylko sprawdzenie, czy dane wypełniają wystąpienie pamięci podręcznej Redis.

### Włączanie wsparcia portu bez protokołu SSL w pamięci podręcznej Redis Azure

>[AZURE.NOTE] W momencie pisania tego artykułu interfejs wiersza polecenia usługi Redis nie obsługuje łączności SSL, dlatego też należy wykonać następujące czynności.

W Portalu Azure przejdź do wystąpienia pamięci podręcznej Redis utworzonego dla tej aplikacji sieci Web. Po otwarciu bloku pamięci podręcznej kliknij ikonę **Ustawienia**.

![Przycisk ustawień pamięci podręcznej Redis Azure](./media/web-sites-connect-to-redis-using-memcache-protocol/15-azure-redis-cache-settings-button.png)

Wybierz opcję **Porty dostępu** z listy.

![Port dostępu pamięci podręcznej Redis Azure](./media/web-sites-connect-to-redis-using-memcache-protocol/16-azure-redis-cache-access-port.png)

Kliknij wartość **Nie** dla opcji **Zezwalaj na dostęp tylko za pomocą protokołu SSL**.

![Port dostępu pamięci podręcznej Redis Azure — tylko protokół SSL](./media/web-sites-connect-to-redis-using-memcache-protocol/17-azure-redis-cache-access-port-ssl-only.png)

Zobaczysz, że został ustawiony port bez protokołu SSL. Kliknij pozycję **Zapisz**.

![Dostęp do portalu bez użycia protokołu SSL — pamięć podręczna Redis Azure](./media/web-sites-connect-to-redis-using-memcache-protocol/18-azure-redis-cache-access-port-non-ssl.png)

### Nawiązywanie połączenia z pamięcią podręczną Redis Azure za pośrednictwem interfejsu wiersza polecenia usługi Redis

>[AZURE.NOTE] W tym kroku przyjęto założenie, że na komputerze deweloperskim zainstalowano lokalnie usługę Redis. [Zainstaluj usługę Redis lokalnie, korzystając z tych instrukcji][9].

Otwórz wybraną konsolę wiersza polecenia i wpisz następujące polecenie:

```shell
redis-cli –h <hostname-for-redis-cache> –a <primary-key-for-redis-cache> –p 6379
```

Zastąp wpis **&lt;hostname-for-redis-cache&gt;** faktyczną nazwą hosta xxxxx.redis.cache.windows.net, a klucz **&lt;primary-key-for-redis-cache&gt;** kluczem dostępu dla pamięci podręcznej. Następnie naciśnij klawisz **Enter**. Po podłączeniu interfejsu wiersza polecenia do wystąpienia pamięci podręcznej Redis można wydać dowolne polecenie Redis. Na poniższych zrzutach ekranu zawarto listę kluczy.

![Nawiązywanie połączenia z pamięcią podręczną Redis Azure za pośrednictwem interfejsu wiersza polecenia usługi Redis w terminalu](./media/web-sites-connect-to-redis-using-memcache-protocol/19-redis-cli-terminal.png)

Wywołanie listy kluczy powinno zwrócić wartość. Jeśli tak nie jest, spróbuj przejść do aplikacji sieci Web i ponowić próbę.

## Podsumowanie

Gratulacje! Aplikacja WordPress ma teraz scentralizowaną pamięć podręczną w pamięci, która pomaga zwiększyć przepustowość. Pamiętaj, że podkładki Memcache aplikacji Web Apps można użyć z dowolnym klientem Memcache, niezależnie od języka programowania lub struktury aplikacji. Aby przekazać opinię lub zadać pytania dotyczące podkładki Memcache aplikacji Web Apps, opublikuj post na [forach dyskusyjnych MSDN][10] lub na stronie [Stackoverflow][11].

>[AZURE.NOTE] Jeśli chcesz zacząć korzystać z usługi Azure App Service przed utworzeniem konta platformy Azure, przejdź do artykułu [Wypróbuj usługę App Service](http://go.microsoft.com/fwlink/?LinkId=523751), w którym wyjaśniono, jak od razu utworzyć początkową aplikację sieci Web o krótkim okresie istnienia w usłudze App Service. Bez kart kredytowych i bez zobowiązań.

## Co zostało zmienione
* Przewodnik dotyczący przejścia od usługi Witryny sieci Web do usługi App Service można znaleźć w temacie [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714) (Usługa Azure App Service i jej wpływ na istniejące usługi platformy Azure).


[0]: ../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache
[1]: http://bit.ly/1t0KxBQ
[2]: http://manage.windowsazure.com
[3]: http://portal.azure.com
[4]: ../powershell-install-configure.md
[5]: /downloads
[6]: http://pecl.php.net
[7]: http://pecl.php.net/package/memcache
[8]: http://blog.syntaxc4.net/post/2015/02/05/how-to-enable-a-site-extension-in-azure-websites.aspx
[9]: http://redis.io/download#installation
[10]: https://social.msdn.microsoft.com/Forums/home?forum=windowsazurewebsitespreview
[11]: http://stackoverflow.com/questions/tagged/azure-web-sites
[12]: /services/cache/
[13]: http://memcached.org



<!--HONumber=Jun16_HO2-->


