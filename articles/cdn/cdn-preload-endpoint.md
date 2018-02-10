---
title: "Wstępne ładowanie zasobów dla punktu końcowego usługi Azure CDN | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak wstępne ładowanie pamięci podręcznej zawartości dla punktu końcowego usługi Azure CDN."
services: cdn
documentationcenter: 
author: dksimpson
manager: erikre
editor: 
ms.assetid: 5ea3eba5-1335-413e-9af3-3918ce608a83
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/02/2018
ms.author: mazha
ms.openlocfilehash: acd6eae12ff338c64cc8879aa8c27b226e3d2f84
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/09/2018
---
# <a name="pre-load-assets-on-an-azure-cdn-endpoint"></a>Wstępne ładowanie zasobów w punkcie końcowym usługi Azure CDN
[!INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

Domyślnie zasoby są buforowane tylko wtedy, gdy są one wymagane. W związku z tym pierwsze żądanie z każdego regionu może trwać dłużej niż kolejnych żądań. Dzieje się tak, ponieważ serwery krawędzi zawartość nie jest jeszcze buforowane i trzeba przesyła żądanie do serwera pochodzenia. Wstępnego ładowania zawartości, można uniknąć tego opóźnienia pierwszy trafień.

Oprócz zapewnienia lepszej obsługi klienta, wstępne ładowanie pamięci podręcznej zasobów można również zmniejszenie ruchu w sieci na serwerze źródłowym.

> [!NOTE]
> Wstępne ładowanie zasobów jest przydatna przy dużych zdarzenia lub zawartości, która staje się dostępna jednocześnie do wielu użytkowników, takich jak nowe wydanie filmu lub aktualizacji oprogramowania.
> 
> 

W tym samouczku przedstawiono wstępnego ładowania zawartości w pamięci podręcznej na wszystkich węzłach krawędzi Azure CDN.

## <a name="to-pre-load-assets"></a>Aby wstępne ładowanie zasobów
1. W [portalu Azure](https://portal.azure.com), przejdź do profilu CDN zawierający punkt końcowy do wstępnego załadowania. Zostanie otwarte okienko profilu.
    
2. Kliknij punkt końcowy na liście. Zostanie otwarte okienko punktu końcowego.
3. Wybierz z okienka punktu końcowego CDN **obciążenia**.
   
    ![Okienko punktu końcowego CDN](./media/cdn-preload-endpoint/cdn-endpoint-blade.png)
   
    **Obciążenia** zostanie otwarte okienko.
   
    ![Okienko obciążenia sieci CDN](./media/cdn-preload-endpoint/cdn-load-blade.png)
4. Aby uzyskać **ścieżki zawartości**, wprowadź pełną ścieżkę każdego zasobu do załadowania (na przykład `/pictures/kitten.png`).
   
   > [!TIP]
   > Więcej **ścieżki zawartości** pola tekstowe pojawią się po rozpoczęciu wprowadzania tekstu, co pozwala na utworzenie listy wiele zasobów. Aby usunąć zasoby z listy, kliknij przycisk wielokropka (...), a następnie wybierz **usunąć**.
   > 
   > Każda ścieżka zawartości musi być względnym adresem URL, który pasuje do następujących [wyrażeń regularnych](https://msdn.microsoft.com/library/az24scfc.aspx):  
   > - Załadować ścieżki pojedynczy plik: `@"^(?:\/[a-zA-Z0-9-_.%=\u0020]+)+$"`;  
   > - Załaduj pojedynczy plik z ciągu zapytania:`@"^(?:\?[-_a-zA-Z0-9\/%:;=!,.\+'&\u0020]*)?$";` 
   > 
   > Każdego zasobu musi mieć własny ścieżkę. Nie ma żadnych funkcji symboli wieloznacznych dla wstępnego ładowania zasobów.
   > 
   > 
   
    ![Przycisk obciążenia](./media/cdn-preload-endpoint/cdn-load-paths.png)
5. Po zakończeniu wprowadzania ścieżek zawartości wybierz **obciążenia**.
   

> [!NOTE]
> Istnieje ograniczenie 10 obciążenia żądań na minutę na profilu CDN. 50 ścieżek równoczesnych mogą być przetwarzane w tym samym czasie. Każda ścieżka ma limit długość ścieżki 1024 znaków.
> 
> 

## <a name="see-also"></a>Zobacz także
* [Przeczyszczanie punktu końcowego usługi Azure CDN](cdn-purge-endpoint.md)
* [Azure dokumentacji interfejsu API REST usługi CDN: wstępnie załadować zawartość dla punktu końcowego](https://docs.microsoft.com/en-us/rest/api/cdn/endpoints/loadcontent)
* [Azure dokumentacji interfejsu API REST usługi CDN: przeczyścić zawartości z punktu końcowego](https://docs.microsoft.com/en-us/rest/api/cdn/endpoints/purgecontent)

