---
ms.assetid: 
title: "Usługa Azure Key Vault ograniczania wskazówki | Dokumentacja firmy Microsoft"
ms.service: key-vault
author: lleonard-msft
ms.author: alleonar
manager: mbaldwin
ms.date: 06/21/2017
ms.openlocfilehash: cc39a5ee466d2e1661281a32010c3d3201c4a0af
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="azure-key-vault-throttling-guidance"></a>Usługa Azure Key Vault ograniczania wskazówki

Ograniczanie jest procesem zainicjowanych przez ograniczenie liczby równoczesnych wywołań do usługi Azure, aby uniemożliwić nadużycia zasobów. Magazyn kluczy Azure (AKV) jest przeznaczona do obsługi dużej liczby żądań. W przypadku utrudnione liczba żądań ograniczanie żądań klienta pomaga utrzymać optymalną wydajność i niezawodność AKV usługi.

Limity ograniczania przepustowości różnić w zależności od scenariusza. Na przykład jeśli przeprowadzasz dużą liczbę operacji zapisu, możliwość ograniczania jest wyższa niż jeśli tylko wykonywana odczytów.

## <a name="how-does-key-vault-handle-its-limits"></a>Jak usługi Key Vault obsługuje limit?

Ograniczenia usługi Key Vault istnieją uniemożliwić nadużycia zasobów i zapewnienia jakości usług dla wszystkich klientów usługi Key Vault. Po przekroczeniu progu usługi Key Vault ogranicza żadnych dalszych żądań z tego klienta w danym okresie czasu. W takim przypadku magazyn kluczy zwraca kod stanu HTTP 429 (zbyt wiele żądań), i Niepowodzenie żądania. Ponadto nie żądań zwracających 429 liczba kierunku limity przepustowości śledzone przez magazyn kluczy. 

Jeśli masz prawidłowe firm przypadku wyższe limity przepustowości, skontaktuj się z nami.


## <a name="how-to-throttle-your-app-in-response-to-service-limits"></a>Ograniczania aplikacji w odpowiedzi na ograniczenia usługi

Poniżej przedstawiono **najlepsze rozwiązania** dla ograniczania aplikacji:
- Zmniejsz liczbę operacji na żądanie.
- Zmniejsz częstotliwość żądań.
- Należy unikać bezpośredniego ponownych prób. 
    - Wszystkie żądania są naliczane względem limitów do użycia.

Podczas implementowania obsługi błędów aplikacji umożliwia kod błędu HTTP 429 wykrywać ograniczania po stronie klienta. Jeśli żądanie nie powiedzie się ponownie z kodem błędu HTTP 429, pojawiły się nadal limit usług Azure. Nadal używać zalecane po stronie klienta ograniczania metody, ponowienie do skutku.

### <a name="recommended-client-side-throttling-method"></a>Zalecana metoda ograniczania przepustowości po stronie klienta

Na kod błędu HTTP 429 Rozpocznij ograniczanie klienta przy użyciu podejścia wykładniczego wycofywania:

1. Poczekaj 1 sekundę, ponów próbę wykonania żądania
2. Jeśli nadal ograniczany poczekaj 2 sekundy, ponów próbę wykonania żądania
3. Jeśli nadal ograniczany oczekiwania 4 sekundy, ponów próbę wykonania żądania
4. Jeśli nadal ograniczany oczekiwania 8 sekund, ponów próbę wykonania żądania
5. Jeśli nadal ograniczany oczekiwania w sekundach 16, ponów próbę wykonania żądania

W tym momencie możesz powinien nie uzyskać kody odpowiedzi HTTP 429.

## <a name="see-also"></a>Zobacz także

Uzyskać lepszy orientacji ograniczania przepustowości w Microsoft Cloud, zobacz [ograniczania wzorzec](https://docs.microsoft.com/azure/architecture/patterns/throttling).

