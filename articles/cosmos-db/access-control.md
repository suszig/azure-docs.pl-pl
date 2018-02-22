---
title: "Ustawienia kontroli dostępu w usłudze Azure DB rozwiązania Cosmos | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak ustawić kontroli dostępu w usłudze Azure DB rozwiązania Cosmos."
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: fae3af3f-4d6e-46d8-9d9b-b80a4218add9
ms.service: cosmos-db
ms.topic: article
ms.date: 02/06/2018
ms.author: mimig
ms.openlocfilehash: 5ef4a12c8229d8801a5d9749514a69c2c1e19499
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/21/2018
---
# <a name="access-control-in-azure-cosmos-db"></a>Kontrola dostępu w usłudze Azure DB rozwiązania Cosmos

Aby dodać bazy danych Azure rozwiązania Cosmos konta czytnika dostępu do konta użytkownika, należy mieć właściciela subskrypcji, wykonaj następujące kroki w portalu Azure.

1. Otwórz Azure portal, a następnie wybierz konto bazy danych Azure rozwiązania Cosmos.
2. Kliknij przycisk **(IAM) kontroli dostępu** , a następnie kliknij pozycję **+ Dodaj**.
3. W **dodać uprawnienia** okienko w **roli** wybierz opcję **rozwiązania Cosmos bazy danych konta czytnik roli**.
4. W **przypisywanie dostępu do pola**, wybierz pozycję **użytkownika usługi Azure AD, grupy lub aplikacji**.
5. Wybierz użytkownika, grupę lub aplikację w katalogu, do którego chcesz udzielić dostępu.  Można wyszukiwać według nazwy wyświetlanej, adresu e-mail lub identyfikatorów obiektów katalogu.
    Wybranego użytkownika, grupy lub aplikacji zostanie wyświetlony na liście wybranych członków.
6. Kliknij pozycję **Zapisz**.

Jednostka mogą teraz odczytywać zasobów bazy danych Azure rozwiązania Cosmos.
