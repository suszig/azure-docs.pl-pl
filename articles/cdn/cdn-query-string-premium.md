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
ms.date: 11/09/2017
ms.author: mazha
ms.openlocfilehash: 2021b5b7602605a7c264e9cd575399077691da34
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/16/2017
---
# <a name="control-azure-content-delivery-network-caching-behavior-with-query-strings---premium"></a>Formant Azure Content Delivery Network buforowanie z ciągami zapytań - Premium
> [!div class="op_single_selector"]
> * [Standardowa](cdn-query-string.md)
> * [Azure CDN Premium from Verizon](cdn-query-string-premium.md)
> 
> 

## <a name="overview"></a>Przegląd
Z Azure Content Delivery Network (CDN), można kontrolować, jak pliki są buforowane dla żądania sieci web, który zawiera ciąg zapytania. W ramach żądania sieci web z ciągu zapytania ciąg zapytania jest częścią żądania, która występuje po znaku zapytania (?). Ciąg zapytania mogą zawierać pary klucz wartość, w których nazwy pól i wartości są oddzielone znakiem równości (=). Każda para klucz wartość oddzielona znakiem handlowego "i" (&). Na przykład `http://www.contoso.com/content.mov?field1=value1&field2=value2`. Jeśli istnieje więcej niż jedną parę klucz wartość ciągu zapytania żądania, ich kolejność nie ma znaczenia. 

> [!IMPORTANT]
> Produkty sieci CDN w warstwie standardowa i premium zapewniają ten sam ciąg zapytania buforowanie funkcji, ale interfejs użytkownika jest inny.  W tym artykule opisano interfejs dla **Azure CDN Premium from Verizon**. Dla zapytania z buforowania ciągu **Azure CDN Standard from Akamai** i **Azure CDN Standard from Verizon**, zobacz [kontrolowanie zachowania buforowania CDN żądań z ciągami zapytań](cdn-query-string.md).
>

Dostępne są trzy tryby ciągu zapytania:

- **pamięć podręczna standard**: domyślny tryb. W tym trybie węzła krawędzi CDN przekazuje ciągi zapytań z obiektu żądającego pochodzenie na pierwsze żądanie i buforuje elementu zawartości. Wszystkie kolejne żądania dla elementu zawartości, które są obsługiwane z węzłem krawędzi ignorować ciągi kwerendy do momentu wygaśnięcia elementu pamięci podręcznej zawartości.
- **pamięć podręczna nie**: W tym trybie żądań z ciągami zapytań nie są buforowane w sieci CDN węzła krawędzi. Węzeł brzegowy pobiera zasobu bezpośrednio ze źródła i przekazuje je do obiektu żądającego z każdym żądaniem.
- **Unikatowy pamięci podręcznej**: W tym trybie każde żądanie o unikatowym adresie URL, ciąg zapytania w tym jest traktowany jako unikatowy zasób ze swojej własnej pamięci podręcznej. Na przykład odpowiedzi ze źródła dla żądania `example.ashx?q=test1` jest pod węzłem krawędzi w pamięci podręcznej i zwrócony dla kolejnych pamięci podręcznych ciągiem zapytania. Żądanie `example.ashx?q=test2` jest buforowana jako osobne zasobów z własną ustawienie czasu wygaśnięcia.

## <a name="changing-query-string-caching-settings-for-premium-cdn-profiles"></a>Zmiana ustawienia profilów sieci CDN w warstwie premium buforowanie ciągów zapytań
1. Otwieranie profilu CDN, a następnie kliknij przycisk **Zarządzaj**.
   
    ![Przycisk Zarządzaj profil CDN](./media/cdn-query-string-premium/cdn-manage-btn.png)
   
    Zostanie otwarty w portalu zarządzania usługi CDN.
2. Umieść kursor nad **HTTP dużych** , a następnie umieść kursor nad **ustawienia pamięci podręcznej** wysuwane menu. Kliknij przycisk **buforowania ciągu kwerendy**.
   
    Opcje buforowania ciągu kwerendy są wyświetlane.
   
    ![Opcje buforowania ciągu kwerendy CDN](./media/cdn-query-string-premium/cdn-query-string.png)
3. Wybierz tryb ciąg zapytania, a następnie kliknij przycisk **aktualizacji**.

> [!IMPORTANT]
> Ponieważ czas rejestracji propagację za pośrednictwem sieci CDN, zmiany ustawień pamięci podręcznej ciąg może nie być od razu widoczne. Aby uzyskać **Azure CDN Premium from Verizon** profile, propagacji zazwyczaj kończy się w ciągu 90 minut, ale w niektórych przypadkach może trwać dłużej.
 

