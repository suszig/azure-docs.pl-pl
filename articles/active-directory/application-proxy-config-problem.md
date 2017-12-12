---
title: Problem z tworzeniem aplikacji serwera Proxy aplikacji | Dokumentacja firmy Microsoft
description: "Jak rozwiązywać problemy dotyczące tworzenia aplikacji serwera Proxy aplikacji w portalu usługi Azure AD administratora"
services: active-directory
documentationcenter: 
author: ajamess
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 5b8346ee2e02ea62b7a11b88a790cff56a7d13f4
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2017
---
# <a name="problem-creating-an-application-proxy-application"></a>Problem z tworzeniem aplikacji serwera Proxy aplikacji 

Poniżej przedstawiono niektóre typowe problemy krój osoby podczas tworzenia nowej aplikacji serwera proxy aplikacji.

## <a name="recommended-documents"></a>Zalecane dokumenty 

Aby dowiedzieć się więcej na temat tworzenia aplikacji serwera Proxy aplikacji za pośrednictwem portalu administratora, zobacz [publikowanie aplikacji przy użyciu serwera Proxy aplikacji usługi Azure AD](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal).

Jeśli są następujące kroki w tym dokumencie i uzyskiwania błąd podczas tworzenia aplikacji, zobacz szczegóły błędu, aby uzyskać informacje i sugestie dotyczące sposobu usunięcia aplikacji. Większość komunikaty o błędach obejmują sugerowanej poprawki. 

## <a name="specific-things-to-check"></a>Określone czynności do wykonania

Aby uniknąć typowych błędów, sprawdzić:

-   Administratorzy z uprawnieniami do tworzenia aplikacji serwera Proxy aplikacji

-   Wewnętrzny adres URL jest unikatowa

-   Zewnętrzny adres URL jest unikatowa

-   Adresy URL rozpoczynać się od http lub https i kończyć się "/"

-   Adres URL powinien być nazwą domeny, a nie adres IP

Komunikat o błędzie powinien być wyświetlany w prawym górnym rogu podczas tworzenia aplikacji. Możesz również wybrać ikonę powiadomienia, aby wyświetlić komunikaty o błędach.

   ![Wiersz powiadomień](./media/application-proxy-config-problem/error-message.png)

## <a name="next-steps"></a>Następne kroki
[Włącz serwer Proxy aplikacji w portalu Azure](active-directory-application-proxy-enable.md)
