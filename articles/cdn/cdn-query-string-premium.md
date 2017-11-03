---
title: "Kontrolowanie Azure CDN buforowanie z ciągami zapytań - Premium | Dokumentacja firmy Microsoft"
description: "Ciąg zapytania usługi Azure CDN buforowanie kontroli, jak pliki mają być buforowane, które zawierają ciągi zapytań."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 99db4a85-4f5f-431f-ac3a-69e05518c997
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 145067c2ce50b41c4783f4de4052c0e7cb529fc7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="control-azure-cdn-caching-behavior-with-query-strings---premium"></a>Formant Azure CDN buforowanie z ciągami zapytań - Premium
> [!div class="op_single_selector"]
> * [Standardowa](cdn-query-string.md)
> * [Azure CDN Premium from Verizon](cdn-query-string-premium.md)
> 
> 

## <a name="overview"></a>Omówienie
Ciąg zapytania buforowanie kontroli, jak pliki mają być buforowane, które zawierają ciągi zapytań.

> [!IMPORTANT]
> Produkty Standard i Premium CDN zawierają ten sam ciąg zapytania buforowanie funkcji, ale różni się w interfejsie użytkownika.  W tym dokumencie opisano interfejs dla **Azure CDN Premium from Verizon**.  Dla zapytania z buforowania ciągu **Azure CDN Standard from Akamai** i **Azure CDN Standard from Verizon**, zobacz [kontrolowanie zachowania buforowania CDN żądań z ciągami zapytań](cdn-query-string.md).
> 
> 

Dostępne są trzy tryby:

* **pamięć podręczna standard**: jest to tryb domyślny.  Węzeł brzegowy CDN zostaną spełnione ciąg zapytania z żądającego do źródła na pierwsze żądanie i pamięci podręcznej elementu zawartości.  Wszystkie kolejne żądania dla tego zasobu, które są obsługiwane z węzłem krawędzi zignoruje ciąg zapytania do momentu wygaśnięcia elementu pamięci podręcznej zawartości.
* **pamięć podręczna nie**: W tym trybie żądań z ciągami zapytań nie są buforowane w sieci CDN węzła krawędzi.  Węzeł brzegowy pobiera zasobu bezpośrednio ze źródła i przekazuje je do obiektu żądającego z każdym żądaniem.
* **Unikatowy pamięci podręcznej**: w tym trybie traktuje każde żądanie zawierające ciąg zapytania jako unikatowy zasób ze swojej własnej pamięci podręcznej.  Na przykład odpowiedzi ze źródła dla żądania *foo.ashx?q=bar* będą buforowane w węźle krawędzi i zwrócony dla kolejnych pamięci podręcznych z tym samym ciągu zapytania.  Żądanie *foo.ashx?q=somethingelse* będzie buforowana jako osobne zasobów własny czas wygaśnięcia.

## <a name="changing-query-string-caching-settings-for-premium-cdn-profiles"></a>Zmiana ustawienia profilów sieci CDN w warstwie premium buforowanie ciągów zapytań
1. Blok profilu CDN, kliknij **Zarządzaj** przycisku.
   
    ![Przycisk Zarządzaj blok profilu CDN](./media/cdn-query-string-premium/cdn-manage-btn.png)
   
    Zostanie otwarty w portalu zarządzania usługi CDN.
2. Umieść kursor nad **HTTP dużych** , a następnie umieść kursor nad **ustawienia pamięci podręcznej** wysuwane okno.  Polecenie **buforowania ciągu kwerendy**.
   
    Opcje buforowania ciągu kwerendy są wyświetlane.
   
    ![Opcje buforowania ciągu kwerendy CDN](./media/cdn-query-string-premium/cdn-query-string.png)
3. Po dokonaniu wyboru kliknij przycisk **aktualizacji** przycisku.

> [!IMPORTANT]
> Zmiany ustawienia mogą nie być od razu widoczne, czas rejestracji propagację za pośrednictwem sieci CDN.  W przypadku profilów usługi <b>Azure CDN from Verizon</b> propagacja zwykle zostanie ukończona w ciągu 90 minut, ale niekiedy może to zająć więcej czasu.
> 
> 

