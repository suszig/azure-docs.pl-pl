---
title: "Kontrolowanie Azure Content Delivery Network buforowanie z ciągami zapytań | Dokumentacja firmy Microsoft"
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
ms.openlocfilehash: 9ffd05a0eb4d976dc40a1c5d45fd22ebf9bd4db1
ms.sourcegitcommit: 4ac89872f4c86c612a71eb7ec30b755e7df89722
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/07/2017
---
# <a name="control-azure-content-delivery-network-caching-behavior-with-query-strings"></a>Formant Azure Content Delivery Network buforowanie z ciągami zapytań
> [!div class="op_single_selector"]
> * [Standardowa](cdn-query-string.md)
> * [Azure CDN Premium from Verizon](cdn-query-string-premium.md)
> 

## <a name="overview"></a>Omówienie
Z Azure Content Delivery Network (CDN), można kontrolować, jak pliki są buforowane dla żądania sieci web, który zawiera ciąg zapytania. W ramach żądania sieci web z ciągu zapytania ciąg zapytania jest częścią żądania, która występuje po znaku zapytania (?). Ciąg zapytania mogą zawierać pary klucz wartość, w których nazwy pól i wartości są oddzielone znakiem równości (=). Każda para klucz wartość oddzielona znakiem handlowego "i" (&). Na przykład: `http://www.contoso.com/content.mov?field1=value1&field2=value2`. Jeśli istnieje więcej niż jedną parę klucz wartość ciągu zapytania żądania, ich kolejność nie ma znaczenia. 

> [!IMPORTANT]
> Produkty sieci CDN w warstwie standardowa i premium zapewniają ten sam ciąg zapytania buforowanie funkcji, ale interfejs użytkownika jest inny.  W tym artykule opisano interfejs dla **Azure CDN Standard from Akamai** i **Azure CDN Standard from Verizon**. Dla zapytania z buforowania ciągu **Azure CDN Premium from Verizon**, zobacz [kontrolowanie zachowania buforowania CDN żądań z ciągami zapytań - Premium](cdn-query-string-premium.md).

Dostępne są trzy tryby ciągu zapytania:

- **Ignorować ciągi kwerendy**: domyślny tryb. W tym trybie węzła krawędzi CDN przekazuje ciągi zapytań z obiektu żądającego pochodzenie na pierwsze żądanie i buforuje elementu zawartości. Wszystkie kolejne żądania dla elementu zawartości, które są obsługiwane z węzłem krawędzi ignorować ciągi kwerendy do momentu wygaśnięcia elementu pamięci podręcznej zawartości.
- **Pomiń buforowanie dla ciągów zapytań**: W tym trybie żądań z ciągami zapytań nie są buforowane w sieci CDN węzła krawędzi. Węzeł brzegowy pobiera zasobu bezpośrednio ze źródła i przekazuje je do obiektu żądającego z każdym żądaniem.
- **Buforuj każdy unikatowy adres URL**: W tym trybie każde żądanie o unikatowym adresie URL, ciąg zapytania w tym jest traktowany jako unikatowy zasób ze swojej własnej pamięci podręcznej. Na przykład odpowiedzi ze źródła dla żądania `example.ashx?q=test1` jest pod węzłem krawędzi w pamięci podręcznej i zwrócony dla kolejnych pamięci podręcznych ciągiem zapytania. Żądanie `example.ashx?q=test2` jest buforowana jako osobne zasobów z własną ustawienie czasu wygaśnięcia.

## <a name="changing-query-string-caching-settings-for-standard-cdn-profiles"></a>Zmiana ustawienia profilów sieci CDN w warstwie standardowa buforowanie ciągów zapytań
1. Otwieranie profilu CDN, a następnie wybierz punkt końcowy CDN, którą chcesz zarządzać.
   
   ![Punkty końcowe profilu CDN](./media/cdn-query-string/cdn-endpoints.png)
   
2. W lewym okienku w obszarze Ustawienia, kliknij przycisk **buforowanie reguły**.
   
    ![Przycisk reguły buforowania CDN](./media/cdn-query-string/cdn-caching-rules-btn.png)
   
3. W **zachowanie buforowania ciągu kwerendy** listy, wybierz tryb ciąg zapytania, a następnie kliknij przycisk **zapisać**.
   
   ![Opcje buforowania ciągu kwerendy CDN](./media/cdn-query-string/cdn-query-string.png)

> [!IMPORTANT]
> Ponieważ czas rejestracji propagację za pośrednictwem sieci CDN, zmiany ustawień pamięci podręcznej ciąg może nie być od razu widoczne. W przypadku profili usługi **Azure CDN from Akamai** propagacja zwykle trwa do minuty. W przypadku profili usługi **Azure CDN from Verizon** propagacja zwykle trwa do 90 minut, ale niekiedy może to zająć więcej czasu.


