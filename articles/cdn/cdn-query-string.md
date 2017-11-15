---
title: "Kontrolowanie Azure CDN buforowanie z ciągami zapytań | Dokumentacja firmy Microsoft"
description: "Ciąg zapytania usługi Azure CDN buforowanie formanty jak buforowane pliki, które zawierają ciągi zapytań."
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
ms.date: 11/09/2017
ms.author: mazha
ms.openlocfilehash: 28e724f34c32edb0d5641b24f9ffedb7dc5f9680
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/11/2017
---
# <a name="control-azure-content-delivery-network-caching-behavior-with-query-strings"></a>Formant Azure Content Delivery Network buforowanie z ciągami zapytań
> [!div class="op_single_selector"]
> * [Standardowa](cdn-query-string.md)
> * [Azure CDN Premium from Verizon](cdn-query-string-premium.md)
> 

## <a name="overview"></a>Omówienie
Z Azure Content Delivery Network (CDN), można kontrolować, jak pliki są buforowane dla żądania sieci web, który zawiera ciąg zapytania. W ramach żądania sieci web z ciągu zapytania, ciąg zapytania jest częścią żądania, która występuje po `?` znaków. Ciąg zapytania mogą zawierać jeden lub więcej parametrów, które są rozdzielone `&` znaków. Na przykład `http://www.domain.com/content.mov?data1=true&data2=false`. Jeśli istnieje więcej niż jeden parametr ciągu zapytania w żądaniu, kolejność parametrów nie ma znaczenia. 

> [!IMPORTANT]
> Produkty sieci CDN w warstwie standardowa i premium zapewniają ten sam ciąg zapytania buforowanie funkcji, ale interfejs użytkownika jest inny.  W tym artykule opisano interfejs dla **Azure CDN Standard from Akamai** i **Azure CDN Standard from Verizon**. Dla zapytania z buforowania ciągu **Azure CDN Premium from Verizon**, zobacz [kontrolowanie zachowania buforowania CDN żądań z ciągami zapytań - Premium](cdn-query-string-premium.md).

Dostępne są trzy tryby ciągu zapytania:

- **Ignorować ciągi kwerendy**: domyślny tryb. W tym trybie węzła krawędzi CDN przekazuje ciągi zapytań z obiektu żądającego pochodzenie na pierwsze żądanie i buforuje elementu zawartości. Wszystkie kolejne żądania dla elementu zawartości, które są obsługiwane z węzłem krawędzi ignorować ciągi kwerendy do momentu wygaśnięcia elementu pamięci podręcznej zawartości.
- **Pomiń buforowanie dla ciągów zapytań**: W tym trybie żądań z ciągami zapytań nie są buforowane w sieci CDN węzła krawędzi. Węzeł brzegowy pobiera zasobu bezpośrednio ze źródła i przekazuje je do obiektu żądającego z każdym żądaniem.
- **Buforuj każdy unikatowy adres URL**: W tym trybie każde żądanie o unikatowym adresie URL, ciąg zapytania w tym jest traktowany jako unikatowy zasób ze swojej własnej pamięci podręcznej. Na przykład odpowiedzi ze źródła dla żądania `example.ashx?q=test1` jest pod węzłem krawędzi w pamięci podręcznej i zwrócony dla kolejnych pamięci podręcznych ciągiem zapytania. Żądanie `example.ashx?q=test2` jest buforowana jako osobne zasobów z własną ustawienie czasu wygaśnięcia.

## <a name="changing-query-string-caching-settings-for-standard-cdn-profiles"></a>Zmiana ustawienia profilów sieci CDN w warstwie standardowa buforowanie ciągów zapytań
1. Otwieranie profilu CDN, a następnie wybierz punkt końcowy CDN, którą chcesz zarządzać.
   
   ![Punkty końcowe profilu CDN](./media/cdn-query-string/cdn-endpoints.png)
   
2. W obszarze Ustawienia, kliknij przycisk **pamięci podręcznej**.
   
    ![Przycisk pamięci podręcznej profil CDN](./media/cdn-query-string/cdn-cache-btn.png)
   
3. W **zachowanie buforowania ciągu kwerendy** listy, wybierz tryb ciąg zapytania, a następnie kliknij przycisk **zapisać**.
   
  <!--- Replace screen shot after general caching goes live ![CDN query string caching options](./media/cdn-query-string/cdn-query-string.png) --->

> [!IMPORTANT]
> Ponieważ czas rejestracji propagację za pośrednictwem sieci CDN, zmiany ustawień pamięci podręcznej ciąg może nie być od razu widoczne. W przypadku profili usługi **Azure CDN from Akamai** propagacja zwykle trwa do minuty. W przypadku profili usługi **Azure CDN from Verizon** propagacja zwykle trwa do 90 minut, ale niekiedy może to zająć więcej czasu.


