---
title: "Kontrolowanie Azure CDN buforowanie z ciągami zapytań | Dokumentacja firmy Microsoft"
description: "Ciąg zapytania usługi Azure CDN buforowanie kontroli, jak pliki mają być buforowane, które zawierają ciągi zapytań."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 17410e4f-130e-489c-834e-7ca6d6f9778d
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 8d79626fa8516f226a82d3dac693c2033904c91d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="control-azure-cdn-caching-behavior-with-query-strings"></a>Formant Azure CDN buforowanie z ciągami zapytań
> [!div class="op_single_selector"]
> * [Standardowa](cdn-query-string.md)
> * [Azure CDN Premium from Verizon](cdn-query-string-premium.md)
> 
> 

## <a name="overview"></a>Omówienie
Ciąg zapytania buforowanie kontroli, jak pliki mają być buforowane, które zawierają ciągi zapytań.

> [!IMPORTANT]
> Produkty Standard i Premium CDN zawierają ten sam ciąg zapytania buforowanie funkcji, ale różni się w interfejsie użytkownika.  W tym dokumencie opisano interfejs dla **Azure CDN Standard from Akamai** i **Azure CDN Standard from Verizon**.  Dla zapytania z buforowania ciągu **Azure CDN Premium from Verizon**, zobacz [kontrolowanie zachowania buforowania CDN żądań z ciągami zapytań - Premium](cdn-query-string-premium.md).
> 
> 

Dostępne są trzy tryby:

* **Ignorować ciągi kwerendy**: jest to tryb domyślny.  Węzeł brzegowy CDN zostaną spełnione ciąg zapytania z żądającego do źródła na pierwsze żądanie i pamięci podręcznej elementu zawartości.  Wszystkie kolejne żądania dla tego zasobu, które są obsługiwane z węzłem krawędzi zignoruje ciąg zapytania do momentu wygaśnięcia elementu pamięci podręcznej zawartości.
* **Pomiń buforowanie adresu URL z ciągami zapytań**: W tym trybie żądań z ciągami zapytań nie są buforowane w sieci CDN węzła krawędzi.  Węzeł brzegowy pobiera zasobu bezpośrednio ze źródła i przekazuje je do obiektu żądającego z każdym żądaniem.
* **Buforuj każdy unikatowy adres URL**: w tym trybie traktuje każde żądanie zawierające ciąg zapytania jako unikatowy zasób ze swojej własnej pamięci podręcznej.  Na przykład odpowiedzi ze źródła dla żądania *foo.ashx?q=bar* będą buforowane w węźle krawędzi i zwrócony dla kolejnych pamięci podręcznych z tym samym ciągu zapytania.  Żądanie *foo.ashx?q=somethingelse* będzie buforowana jako osobne zasobów własny czas wygaśnięcia.

## <a name="changing-query-string-caching-settings-for-standard-cdn-profiles"></a>Zmiana ustawienia profilów sieci CDN w warstwie standardowa buforowanie ciągów zapytań
1. Blok profilu CDN kliknij punkt końcowy CDN, którą chcesz zarządzać.
   
    ![Punkty końcowe blok profilu CDN](./media/cdn-query-string/cdn-endpoints.png)
   
    Zostanie otwarty blok punktu końcowego CDN.
2. Kliknij przycisk **Konfiguruj** przycisku.
   
    ![Przycisk Zarządzaj blok profilu CDN](./media/cdn-query-string/cdn-config-btn.png)
   
    Zostanie otwarty blok konfiguracji sieci CDN.
3. Wybierz ustawienie z **zachowanie buforowania ciągu kwerendy** listy rozwijanej.
   
    ![Opcje buforowania ciągu kwerendy CDN](./media/cdn-query-string/cdn-query-string.png)
4. Po dokonaniu wyboru kliknij przycisk **zapisać** przycisku.

> [!IMPORTANT]
> Zmiany ustawienia mogą nie być od razu widoczne, czas rejestracji propagację za pośrednictwem sieci CDN.  W przypadku profilów usługi <b>Azure CDN from Akamai</b> propagacja zwykle zostanie ukończona w ciągu minuty.  W przypadku profilów usługi <b>Azure CDN from Verizon</b> propagacja zwykle zostanie ukończona w ciągu 90 minut, ale niekiedy może to zająć więcej czasu.
> 
> 

