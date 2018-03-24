---
title: Obsługa protokołu HTTP/2 w usłudze Azure CDN | Dokumentacja firmy Microsoft
description: Więcej informacji na temat obsługi protokołu HTTP/2 i CDN.
services: cdn
documentationcenter: ''
author: lichard
manager: erikre
editor: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 5/04/2017
ms.author: rli
ms.openlocfilehash: a58ab1165f3886484fb695170100422c2f1b0486
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/23/2018
---
# <a name="http2-support-in-azure-cdn"></a>Obsługa protokołu HTTP/2 w usługi Azure CDN

HTTP/2 jest znaczne zmiany do HTTP/1.1\. Zapewnia szybszy wydajności sieci web, czas odpowiedzi obniżona i lepsze środowisko, przy zachowaniu znanych metod HTTP, kodów stanu i semantyki. Chociaż HTTP/2 jest przeznaczony do pracy z protokołów HTTP i HTTPS, wiele przeglądarek sieci web klienta obsługują tylko HTTP/2 za pośrednictwem protokołu TLS.

### <a name="http2-benefits"></a>Korzyści HTTP/2

Zalety HTTP/2:

*   **Multipleksowanie i współbieżność**

    Przy użyciu protokołu HTTP 1.1, wielu wprowadzania wielu żądań zasobów wymaga wielu połączeń TCP, a każde połączenie ma zmniejszenie wydajności związane z nią. HTTP/2 umożliwia wielu zasobów wymagane dla pojedynczego połączenia TCP.

*   **Kompresja nagłówka**

    Przez kompresowanie nagłówków HTTP obsługiwane zasobów, czas przesyłania jest znacznie ograniczony.

*   **Zależności strumienia**

    Zależności strumienia pozwalają klientowi wskazują na serwer, którego zasoby mają priorytet.


## <a name="http2-browser-support"></a>Obsługa przeglądarek HTTP/2

Wszystkie główne przeglądarki wdrożono Obsługa HTTP/2 w ich bieżącej wersji. Nieobsługiwany przeglądarki zostanie automatycznie powrotu do HTTP/1.1.

|Przeglądarka|Minimalna wersja|
|-------------|------------|
|Microsoft Edge| 12|
|Google Chrome| 43|
|Mozilla Firefox| 38|
|Opera| 32|
|Safari| 9|

## <a name="enabling-http2-support-in-azure-cdn"></a>Włączanie obsługi protokołu HTTP/2 w Azure CDN

Obsługa protokołu HTTP/2 jest obecnie aktywny dla **Azure CDN from Akamai** i **Azure CDN from Verizon** profilów. Żadne dalsze akcje nie jest wymagana od klientów.

## <a name="next-steps"></a>Następne kroki

Aby zapoznać się z zalet HTTP/2 w akcji, zobacz [ten pokaz from Akamai](https://http2.akamai.com/demo).

Aby dowiedzieć się więcej na temat protokołu HTTP/2, można znaleźć w następujących zasobach:

*   [Strona główna specyfikacji HTTP/2](https://http2.github.io/)
*   [Oficjalna HTTP/2 — często zadawane pytania](https://http2.github.io/faq/)
*   [Informacje o Akamai HTTP/2](https://http2.akamai.com/)

Aby dowiedzieć się więcej na temat dostępnych funkcji usługi Azure CDN, zobacz [Omówienie usługi Azure CDN](https://azure.microsoft.com/documentation/articles/cdn-overview/).