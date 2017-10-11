---
title: "Dodaj łącznik usługi Facebook w aplikacji logiki | Dokumentacja firmy Microsoft"
description: "Omówienie łącznik usługi Facebook z parametrami interfejsu API REST"
services: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: f4d6f0ed-c09b-488c-be1c-8cf2b5b1d4b8
ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/07/2016
ms.author: mandia; ladocs
ms.openlocfilehash: e10a30ccef3e81cb3d7749696453d82b8958d076
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-facebook-connector"></a>Rozpoczynanie pracy z łącznikiem usługi Facebook
Nawiązać połączenia z serwisem Facebook i publikować na osi czasu, Pobierz strony źródła danych i inne. Za pomocą usługi Facebook można:

* Tworzenie sieci przepływu biznesowe na podstawie danych otrzymywanych z usługi Facebook. 
* Po odebraniu nowego wpisu, należy użyć wyzwalacza.
* Użyj akcji, które post do osi czasu, Pobierz strony źródła danych i inne. Te akcje uzyskać odpowiedzi, a następnie udostępnić dane wyjściowe do innych działań. Na przykład gdy istnieje nowy wpis na osi czasu, można wykonać tego post i wypchnąć go do Twojego kanału informacyjnego usługi Twitter. 

Rozpoczynanie pracy przez teraz tworzenie aplikacji logiki, zobacz [tworzenie aplikacji logiki](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="create-a-connection-to-facebook"></a>Utwórz połączenie z usługi Facebook
Po dodaniu tego łącznika do aplikacji logiki, należy zezwolić aplikacji logiki do nawiązania połączenia z serwisem Facebook.

1. Zaloguj się do swojego konta w serwisie Facebook
2. Wybierz **autoryzacji**i umożliwia aplikacji logiki, aby podłączyć się i korzystać z usługi Facebook. 

> [!INCLUDE [Steps to create a connection to Facebook](../../includes/connectors-create-api-facebook.md)]
> 


## <a name="connector-specific-details"></a>Szczegóły dotyczące łącznika

Wyświetl wszystkie wyzwalacze i akcje zdefiniowane w swagger i zobacz też żadnych limitów w [szczegóły łącznika](/connectors/facebook/).

## <a name="more-connectors"></a>Więcej łączników
Wróć do [listy interfejsów API](apis-list.md).