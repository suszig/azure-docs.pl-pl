---
title: "Wstępne ładowanie zasobów dla punktu końcowego usługi Azure CDN | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak wstępne ładowanie pamięci podręcznej zawartości dla punktu końcowego usługi Azure CDN."
services: cdn
documentationcenter: 
author: smcevoy
manager: erikre
editor: 
ms.assetid: 5ea3eba5-1335-413e-9af3-3918ce608a83
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 1f2dcd9a91bb6e883cbef06373c1acd98bf8d45f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="pre-load-assets-on-an-azure-cdn-endpoint"></a>Wstępne ładowanie zasobów w punkcie końcowym usługi Azure CDN
[!INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

Domyślnie zasoby najpierw są buforowane, ponieważ są one wymagane. Oznacza to, że pierwsze żądanie z każdego regionu może trwać dłużej, ponieważ serwery krawędzi nie będą mieć zawartość w pamięci podręcznej i będzie musiał przesyła żądanie do serwera pochodzenia. Wstępnego ładowania zawartości pozwala uniknąć ten pierwszy opóźnienia trafień.

Oprócz zapewnienia lepszej obsługi klienta, wstępne ładowanie pamięci podręcznej zasobów można również zmniejszenie ruchu w sieci na serwerze źródłowym.

> [!NOTE]
> Wstępne ładowanie zasobów jest przydatna przy dużych zdarzenia lub zawartości, która staje się dostępna jednocześnie do wielu użytkowników, takich jak nowe wydanie filmu lub aktualizacji oprogramowania.
> 
> 

W tym samouczku przedstawiono wstępnego ładowania zawartości w pamięci podręcznej na wszystkich węzłach krawędzi Azure CDN.

## <a name="walkthrough"></a>Przewodnik
1. W [Azure Portal](https://portal.azure.com), przejdź do profilu CDN zawierający punkt końcowy do wstępnego załadowania.  Zostanie otwarty blok profilu.
2. Kliknij punkt końcowy na liście.  Zostanie otwarty blok końcowy.
3. W bloku punktu końcowego usługi CDN kliknij przycisk obciążenia.
   
    ![Blok punktu końcowego CDN](./media/cdn-preload-endpoint/cdn-endpoint-blade.png)
   
    Zostanie otwarty blok obciążenia.
   
    ![Blok obciążenia CDN](./media/cdn-preload-endpoint/cdn-load-blade.png)
4. Wprowadź pełną ścieżkę każdego zasobu do załadowania (np. `/pictures/kitten.png`) w **ścieżki** pola tekstowego.
   
   > [!TIP]
   > Więcej **ścieżki** pola tekstowe pojawią się po wprowadź tekst, który pozwala na utworzenie listy wiele zasobów.  Zasoby można usunąć z listy, klikając przycisk wielokropka (...).
   > 
   > Ścieżki musi być względnym adresem URL, który pasuje do następujących [wyrażenia regularnego](https://msdn.microsoft.com/library/az24scfc.aspx):  
   > >Załadować ścieżki pojedynczy plik `@"^(?:\/[a-zA-Z0-9-_.%=\u0020]+)+$"`;  
   > >Załaduj pojedynczy plik z ciągu zapytania`@"^(?:\?[-_a-zA-Z0-9\/%:;=!,.\+'&\u0020]*)?$";`  
   > 
   > Każdego zasobu musi mieć własny ścieżkę.  Nie ma żadnych funkcji symboli wieloznacznych dla wstępnego ładowania zasobów.
   > 
   > 
   
    ![Przycisk obciążenia](./media/cdn-preload-endpoint/cdn-load-paths.png)
5. Kliknij przycisk **obciążenia** przycisku.
   
    ![Przycisk obciążenia](./media/cdn-preload-endpoint/cdn-load-button.png)

> [!NOTE]
> Istnieje ograniczenie 10 obciążenia żądań na minutę na profilu CDN. 50 ścieżek są dozwolone na żądanie. Każda ścieżka ma limit długość ścieżki 1024 znaków.
> 
> 

## <a name="see-also"></a>Zobacz też
* [Przeczyszczanie punktu końcowego usługi Azure CDN](cdn-purge-endpoint.md)
* [Azure dokumentacji interfejsu API REST usługi CDN - przeczyścić lub wstępnie załadować punktu końcowego](https://msdn.microsoft.com/library/mt634451.aspx)

