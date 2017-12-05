---
title: "Edytowanie interfejsu API za pomocą portalu Azure | Dokumentacja firmy Microsoft"
description: "Ten samouczek pokazuje, jak edytować interfejsu API przy użyciu interfejsu API zarządzania (APIM)."
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 11/08/2017
ms.author: apimpm
ms.openlocfilehash: 362c36181da706e3fe0a27cc5ab262271c2a1e57
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2017
---
# <a name="edit-an-api"></a>Edytowanie interfejsu API

Kroki opisane w tym samouczku pokazano, jak użyć interfejsu API zarządzania (APIM), aby edytować interfejsu API. 

+ Możesz zrobić to przez dodawanie, usuwanie, zmiana nazwy operacji w wystąpieniu APIM. 
+ Można edytować Twój interfejs API programu swagger.

## <a name="prerequisites"></a>Wymagania wstępne

+ [Utwórz wystąpienie usługi Azure API Management](get-started-create-service-instance.md)
+ [Importowanie i opublikować swój pierwszy interfejs API](import-and-publish.md)

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="edit-an-api-in-apim"></a>Edytowanie interfejsu API w APIM

![Edytowanie interfejsu api](./media/edit-api/edit-api001.png)

1. Kliknij przycisk **interfejsów API** kartę.
2. Wybierz jeden z interfejsów API, które zostały wcześniej zaimportowane.
3. Wybierz **projekt** kartę.
4. Wybierz operację, którą chcesz edytować.
5. Aby zmienić operację, wybierz **ołówka** w **frontonu** okna.

## <a name="update-the-swagger"></a>Aktualizacja programu swagger

Można aktualizować backbend Twojego interfejsu API w portalu Azure, wykonaj następujące czynności:

1. Wybierz **wszystkie operacje**
2. Kliknij przycisk ołówka w **frontonu** okna.

    ![Edytowanie interfejsu api](./media/edit-api/edit-api002.png)

    Pojawi się Twój interfejs API programu swagger.

    ![Edytowanie interfejsu api](./media/edit-api/edit-api003.png)

3. Aktualizacja programu swagger.
4. Naciśnij klawisz **zapisać**.

[!INCLUDE [api-management-define-api-topics.md](../../includes/api-management-define-api-topics.md)]

## <a name="next-steps"></a>Następne kroki

> [!div class="nextstepaction"]
> [Przykłady zasad APIM](policy-samples.md)
> [transformacji i chronić opublikowanych interfejsu API](transform-api.md)