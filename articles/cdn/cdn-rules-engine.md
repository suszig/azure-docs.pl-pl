---
title: "Zastąpienie zachowania HTTP przy użyciu usługi Azure CDN aparatu reguł | Dokumentacja firmy Microsoft"
description: "Aparat reguł pozwala dostosować sposób obsługi żądań HTTP przez usługi Azure CDN, takich jak blokowanie dostarczania niektórych typów zawartości, definiowania zasad buforowania i modyfikować nagłówki HTTP."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 625a912b-91f2-485d-8991-128cc194ee71
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: abfe283476206b181018d187675b47112dc5ad2f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="override-http-behavior-using-the-azure-cdn-rules-engine"></a>Zastąpienie zachowania HTTP przy użyciu aparatu reguł Azure CDN
[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Omówienie
Aparat reguł pozwala dostosować sposób obsługi żądań HTTP, takich jak blokowanie dostarczania niektórych typów zawartości, definiowania zasad buforowania i modyfikowanie nagłówków HTTP.  W tym samouczku zostanie pokazują, że tworzenia reguły, która spowoduje zmianę zachowania buforowania zasobów w sieci CDN.  Również jest dostępna w zawartości wideo "[Zobacz też](#see-also)" sekcji.

   > [!TIP] 
   > Odwołanie do składni szczegółowo, zobacz [odwołanie do aparatu reguł](cdn-rules-engine-reference.md).
   > 


## <a name="tutorial"></a>Samouczek
1. Blok profilu CDN, kliknij **Zarządzaj** przycisku.
   
    ![Przycisk Zarządzaj blok profilu CDN](./media/cdn-rules-engine/cdn-manage-btn.png)
   
    Zostanie otwarty w portalu zarządzania usługi CDN.
2. Polecenie **HTTP dużych** kartę, a następnie **aparatu reguł**.
   
    Opcje dla nowej reguły są wyświetlane.
   
    ![Nowe opcje reguły CDN](./media/cdn-rules-engine/cdn-new-rule.png)
   
   > [!IMPORTANT]
   > Kolejność, w którym są wyświetlane wiele reguł ma wpływ na sposób obsługi. Kolejne reguły mogą zastąpić akcje określone przez poprzednie reguły.
   > 
   > 
3. Wprowadź nazwę w **nazwę / opis** pola tekstowego.
4. Określ typ żądań, które będą dotyczyć zasady.  Domyślnie **zawsze** wybrano warunek dopasowania.  Użyjesz **zawsze** w tym samouczku, dlatego pozostaw wybrany.
   
   ![Warunek dopasowania CDN](./media/cdn-rules-engine/cdn-request-type.png)
   
   > [!TIP]
   > Istnieje wiele typów dopasowania warunków dostępnych na liście rozwijanej.  Kliknięcie ikony informacyjny niebieski z lewej strony warunku dopasowania objaśnia aktualnie wybranego warunku szczegółowo.
   > 
   >  Pełną listę wyrażeń warunkowych szczegółowo w temacie [wyrażenia warunkowe aparatu reguł](cdn-rules-engine-reference-match-conditions.md).
   >  
   > Pełną listę warunków dopasowania szczegółowo w temacie [warunki odpowiada aparatu reguł](cdn-rules-engine-reference-match-conditions.md).
   > 
   > 
5. Kliknij przycisk  **+**  znajdujący się obok **funkcje** można dodać nowych funkcji.  Na liście rozwijanej po lewej stronie wybierz **życie wewnętrzny Max-Age**.  W wyświetlonym polu tekstowym wprowadź **300**.  Pozostaw pozostałe wartości domyślne.
   
   ![Funkcja CDN](./media/cdn-rules-engine/cdn-new-feature.png)
   
   > [!NOTE]
   > Jako warunkom dopasowania, klikając niebieski informacyjną ikonę na lewo od nowa funkcja wyświetli szczegóły dotyczące tej funkcji.  W odniesieniu **życie wewnętrzny Max-Age**, możemy są zastępowanie elementu zawartości **Cache-Control** i **Expires** nagłówków, aby kontrolować, kiedy węzła krawędzi CDN odświeży zasobu ze źródła.  Naszym przykładzie 300 sekund oznacza, że węzeł brzegowy CDN będą buforowane zasobów przez 5 minut przed odświeżeniem zawartości ze swoją witryną źródłową.
   > 
   > Aby uzyskać pełną listę funkcji, które szczegółowo, zobacz [Szczegóły funkcji aparatu reguł](cdn-rules-engine-reference-features.md).
   > 
   > 
6. Kliknij przycisk **Dodaj** przycisk, aby zapisać nową regułę.  Nowa reguła teraz oczekuje na zatwierdzenie. Po jej zatwierdzeniu, stan zmieni się z **oczekujące XML** do **Active XML**.
   
   > [!IMPORTANT]
   > Zmiany zasad może potrwać do 90 minut propagację za pośrednictwem sieci CDN.
   > 
   > 

## <a name="see-also"></a>Zobacz też
* [Omówienie usługi Azure CDN](cdn-overview.md)
* [Odwołanie do aparatu reguł](cdn-rules-engine-reference.md)
* [Warunki uzgadniania aparatu reguł](cdn-rules-engine-reference-match-conditions.md)
* [Wyrażenia warunkowe aparatu reguł](cdn-rules-engine-reference-conditional-expressions.md)
* [Funkcje aparatu reguł](cdn-rules-engine-reference-features.md)
* [Zastępowanie domyślnego zachowania HTTP przy użyciu aparatu reguł](cdn-rules-engine.md)
* [Azure piątki: Usługi Azure CDN zaawansowanych nowych funkcji Premium](https://azure.microsoft.com/documentation/videos/azure-cdns-powerful-new-premium-features/) (klip wideo)