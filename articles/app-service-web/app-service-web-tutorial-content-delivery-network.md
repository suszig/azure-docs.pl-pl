---
title: "Dodawanie usługi Content Delivery Network w usłudze Azure App Service | Microsoft Docs"
description: "Dodaj usługę Content Delivery Network w usłudze Azure App Service w celu dostarczania plików statycznych z węzłów krawędzi."
services: app-service
author: syntaxc4
ms.author: cfowler
ms.date: 04/03/2017
ms.topic: hero-article
ms.service: app-service-web
manager: erikre
translationtype: Human Translation
ms.sourcegitcommit: 9eafbc2ffc3319cbca9d8933235f87964a98f588
ms.openlocfilehash: 7ba3737566401152a3171e8926beca188045230c
ms.lasthandoff: 04/22/2017

---
# <a name="add-a-content-deliver-network-on-an-azure-app-service"></a>Dodawanie usługi Content Delivery Network w usłudze Azure App Service

W tym samouczku dodasz usługę Content Delivery Network do usługi Azure App Service w celu udostępniania zawartości statycznej na serwerze krawędzi. Utworzysz profil CDN, który jest zbiorem maksymalnie 10 punktów końcowych usługi CDN.

Usługa Content Delivery Network (CDN) buforuje zawartość statyczną sieci Web w strategicznie rozmieszczonych lokalizacjach w celu zapewnienia maksymalnej przepływności dostarczania zawartości dla użytkowników. Zalety używania usługi CDN do buforowania zasobów witryny sieci Web obejmują:

* Lepszą wydajność i środowisko dla użytkowników końcowych, zwłaszcza w przypadku korzystania z aplikacji, w których wymagane jest wiele rund do załadowania zawartości.
* Duże skalowanie, aby lepiej obsługiwać natychmiastowe wysokie obciążenie, na przykład na początku zdarzenia uruchamiania produktu.
* Dystrybucję żądań użytkowników i obsługę zawartości z serwerów krawędzi, dzięki czemu mniej ruchu jest wysyłane do punktu początkowego.

> [!TIP]
> Przejrzyj aktualną listę [lokalizacji punktów obecności usługi Azure CDN](https://docs.microsoft.com/en-us/azure/cdn/cdn-pop-locations).
>

## <a name="deploy-the-sample"></a>Wdrażanie przykładu

Do wykonania kroków tego samouczka potrzebna będzie aplikacja wdrożona w aplikacji sieci Web. Postępuj zgodnie z instrukcjami [tworzenie statycznej witryny HTML — szybki start](app-service-web-get-started-html.md), aby zapewnić podstawę dla tego samouczka.

## <a name="step-1---login-to-azure-portal"></a>Krok 1 — Logowanie do witryny Azure Portal

Najpierw otwórz ulubioną przeglądarkę i przejdź do witryny [Azure Portal](https://portal.azure.com).

## <a name="step-2---create-a-cdn-profile"></a>Krok 2 — Tworzenie profilu CDN

Kliknij przycisk `+ New` znajdujący się w obszarze nawigacji po lewej stronie, a następnie kliknij pozycję **Sieć Web i mobilność**. W kategorii Sieć Web i aplikacje mobilne wybierz pozycję **CDN**.

Określ następujące pola:

| Pole | Wartość przykładowa | Opis |
|---|---|---|
| Nazwa | myCDNProfile | Nazwa profilu CDN. |
| Lokalizacja | Europa Zachodnia | To jest lokalizacja platformy Azure, w której będą przechowywane informacje o profilu CDN. Nie ma ona wpływu na lokalizacje punktów końcowych CDN. |
| Grupa zasobów | myResourceGroup | Więcej informacji na temat grup zasobów znajduje się w temacie [Usługa Azure Resource Manager — omówienie](../azure-resource-manager/resource-group-overview.md#resource-groups). |
| Warstwa cenowa | Standard Akamai | Porównanie warstw cenowych znajduje się w temacie [Omówienie CDN](../cdn/cdn-overview.md#azure-cdn-features). |

Kliknij przycisk **Utwórz**.

Otwórz centrum grup zasobów w obszarze nawigacji po lewej stronie i wybierz pozycję **myResourceGroup**. Z listy zasobów wybierz pozycję **myCDNProfile**.

![azure-cdn-profile-created](media/app-service-web-tutorial-content-delivery-network/azure-cdn-profile-created.png)

## <a name="step-3---create-a-cdn-endpoint"></a>Krok 3 — Tworzenie punktu końcowego CDN

Kliknij pozycję **+ Punkt końcowy** wśród poleceń obok pola wyszukiwania, co spowoduje uruchomienie bloku tworzenia punktu końcowego.

Określ następujące pola:

| Pole | Wartość przykładowa | Opis |
|---|---|
| Nazwa |  | Ta nazwa będzie używana do uzyskiwania dostępu do buforowanych zasobów w domenie `<endpointname>.azureedge.net`. |
| Typ źródła | Aplikacja sieci Web | Wybranie typu źródła zapewnia menu kontekstowe dla pozostałych pól. Wybranie źródła niestandardowego udostępni pole tekstowe do wprowadzenia nazwy hosta źródła. |
| Nazwa hosta źródła | |  Na liście rozwijanej będą wyświetlane wszystkie dostępne źródła określonego typu. W przypadku wybrania w polu Typ źródła opcji Źródło niestandardowe wpisz domenę źródła niestandardowego.  |

Kliknij pozycję **Add** (Dodaj).

Punkt końcowy zostanie utworzony. Po utworzeniu punktu końcowego usługi Content Delivery Network stan zostanie zaktualizowany na **uruchomiono**.

![azure-cdn-endpoint-created](media/app-service-web-tutorial-content-delivery-network/azure-cdn-endpoint-created.png)

## <a name="step-4---serve-from-azure-cdn"></a>Krok 4 — Udostępnianie z usługi Azure CDN

Teraz, gdy punkt końcowy usługi CDN ma stan **uruchomiono**, powinno być możliwe uzyskanie dostępu do zawartości z punktu końcowego usługi CDN.

Biorąc pod uwagę, że jako podstawy dla tego samouczka użyliśmy instrukcji [tworzenie statycznej witryny HTML — szybki start](app-service-web-get-started-html.md), w usłudze CDN powinny być dostępne następujące foldery: `css`, `img`, `js`.

Ścieżki zawartości między adresem URL aplikacji sieci Web `http://<app_name>.azurewebsites.net/img/` i adresem URL punktu końcowego usługi CDN `http://<endpointname>.azureedge.net/img/` są takie same, co oznacza, że możesz po prostu zastąpić domenę punktu końcowego CDN dla dowolnej zawartości statycznej, aby mogła być udostępniana z usługi CDN.

Ściągnijmy nasz pierwszy obraz z punktu końcowego CDN, przechodząc do następującego adresu URL w przeglądarce sieci Web:

```bash
http://<endpointname>.azureedge.net/img/03-enterprise.png
```

Teraz, kiedy zawartość statyczna jest dostępna w usłudze CDN, możesz zaktualizować aplikację, aby używać punktu końcowego usługi CDN do dostarczania zawartości do użytkownika końcowego.

W zależności od języka użytego do utworzenia lokacji dostępnych może być wiele struktur ułatwiających przełączanie rezerwowe usługi CDN. Na przykład platforma ASP.NET zapewnia obsługę [tworzenia pakietów i minifikacji](https://docs.microsoft.com/en-us/aspnet/mvc/overview/performance/bundling-and-minification#using-a-cdn), które zapewniają także możliwości przełączania rezerwowego usługi CDN.

Jeśli dany język nie ma wbudowanej możliwości lub biblioteki do obsługi przełączania awaryjnego usługi CDN, możesz skorzystać ze środowiska JavaScript, na przykład [FallbackJS](http://fallback.io/), które obsługuje ładowanie [skryptów](https://github.com/dolox/fallback/tree/master/examples/loading-scripts), [arkuszy stylów](https://github.com/dolox/fallback/tree/master/examples/loading-stylesheets) i [obrazów](https://github.com/dolox/fallback/tree/master/examples/loading-images).

## <a name="step-5---purge-the-cdn"></a>Krok 5 — Przeczyszczanie usługi CDN

Czasami może być konieczne wymuszenie przeczyszczenia usługi CDN, jeśli chcesz, aby zawartość wygasła przed czasem wygaśnięcia (TTL, time to live).

Istnieje możliwość ręcznego przeczyszczenia usługi Azure CDN z poziomu bloku Profil CDN lub bloku punktu końcowego usługi CDN. W przypadku wybrania przeczyszczania z poziomu strony profilu CDN konieczne będzie wybranie punktu końcowego, który ma zostać przeczyszczony.

Aby przeczyścić zawartość, wpisz ścieżki zawartości, które chcesz przeczyścić. Możesz podać pełną ścieżkę, aby przeczyścić pojedynczy plik, lub segment ścieżki, aby przeczyścić i odświeżyć zawartość z konkretnego folderu.

Po podaniu wszystkich ścieżek zawartości do przeczyszczenia kliknij przycisk **Przeczyść**.

![app-service-web-purge-cdn](media/app-service-web-tutorial-content-delivery-network/app-service-web-purge-cdn.png)

## <a name="step-6---map-a-custom-domain"></a>Krok 6 — Mapowanie domeny niestandardowej

Mapowanie domeny niestandardowej na punkt końcowy usługi CDN zapewnia jednolitą domenę dla aplikacji sieci Web.

Aby zamapować domenę niestandardową na punkt końcowy usługi CDN, należy utworzyć rekord CNAME u rejestratora domen.

> [!NOTE]
> Rekord CNAME jest funkcją systemu DNS, która mapuje domenę źródłową, taką jak `www.contosocdn.com` lub `static.contosocdn.com`, na domenę docelową.

W tym przypadku zostanie dodana domena źródłowa `static.contosocdn.com` i wskazana domena docelowa, która jest punktem końcowym usługi CDN.

| domena źródłowa | domena docelowa |
|---|---|
| static.contosocdn.com | &lt;nazwa punktu końcowego&gt;.azureedge.net |

W bloku Przegląd punktu końcowego CDN kliknij przycisk `+ Custom domain`.

W bloku Dodawanie domeny niestandardowej wprowadź w oknie dialogowym domenę niestandardową, w tym poddomenę. Na przykład wprowadź nazwę domeny w formacie `static.contosocdn.com`.

Kliknij pozycję **Add** (Dodaj).

## <a name="step-7---version-content"></a>Krok 7 — Wersja zawartości

W obszarze nawigacji po lewej stronie punktu końcowego usługi CDN wybierz pozycję **Pamięć podręczna** poniżej nagłówka Ustawienia.

Blok **Pamięć podręczna** umożliwia skonfigurowanie sposobu, w jaki usługa CDN obsługuje ciągi zapytań w żądaniu.

> [!NOTE]
> Opis opcji zachowania buforowania ciągów zapytań znajduje się w temacie [Control Azure CDN caching behavior with query strings](../cdn/cdn-query-string.md) (Sterowanie zachowaniem buforowania usługi CDN dla ciągów zapytań).

Wybierz pozycję **Buforuj każdy unikatowy adres URL** z listy rozwijanej dla zachowania pamięci podręcznej ciągów zapytań.

Kliknij pozycję **Zapisz**.

## <a name="next-steps"></a>Następne kroki

* [Co to jest Azure CDN](../best-practices-cdn.md?toc=%2fazure%2fcdn%2ftoc.json)
* [Włączanie protokołu HTTPS w domenie niestandardowej usługi Azure CDN](../cdn/cdn-custom-ssl.md)
* [Poprawianie wydajności poprzez kompresowanie plików w usłudze Azure CDN](../cdn/cdn-improve-performance.md)
* [Wstępne ładowanie zasobów w punkcie końcowym usługi Azure CDN](../cdn/cdn-preload-endpoint.md)

