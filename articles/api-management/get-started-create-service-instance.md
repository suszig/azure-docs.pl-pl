---
title: "Utwórz wystąpienie usługi Azure API Management | Dokumentacja firmy Microsoft"
description: "Wykonaj kroki tego samouczka do utworzenia nowego wystąpienia usługi Azure API Management."
services: api-management
documentationcenter: 
author: vladvino
manager: anneta
editor: 
ms.service: api-management
ms.workload: integration
ms.topic: article
ms.date: 08/17/2017
ms.author: apimpm
ms.openlocfilehash: 6433ea1f0eb6ad375402b998b4dfa80bded35c4b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-new-azure-api-management-service-instance"></a>Tworzenie nowego wystąpienia usługi Azure API Management

W tym samouczku opisano kroki tworzenia nowego wystąpienia usługi API Management, przy użyciu [portalu Azure](https://portal.azure.com/).

## <a name="prerequisites"></a>Wymagania wstępne

+ Aktywna subskrypcja platformy Azure.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-new-service"></a>Tworzenie nowej usługi

1. W [portalu Azure](https://portal.azure.com/), wybierz pozycję **nowy** > **integracji przedsiębiorstwa** > **zarządzanie interfejsami API**.

    Możesz też wybrać **nowy**, typ `API management` w polu wyszukiwania, a następnie naciśnij klawisz Enter. Kliknij przycisk **Utwórz**.

2. W **usługi Zarządzanie interfejsami API** okna, wprowadź unikatową **nazwa** usługi Zarządzanie interfejsami API. Ta nazwa nie można zmienić później.

    > [!TIP]
    > Nazwa usługi jest używana do generowania domyślna nazwa domeny w postaci *.azure-api.net {nazwa}.* Jeśli chcesz użyć niestandardowej nazwy domeny, zobacz [skonfigurować własną domenę niestandardową](configure-custom-domain.md). <br/>
    > Nazwa usługi jest używana do odwoływania się do usługi i odpowiednich zasobów platformy Azure.

5. Wybierz **subskrypcji** między mających dostęp do różnych subskrypcji platformy Azure.
6. W polu **Grupa zasobów** wybierz nowy lub istniejący zasób.  Grupa zasobów jest kolekcją zasobów, które mają ten sam cykl życia, uprawnienia i zasady. Więcej informacji można znaleźć [tutaj](../azure-resource-manager/resource-group-overview.md#resource-groups).
7. W **lokalizacji**, wybierz region geograficzny, w którym jest tworzony usługi Zarządzanie interfejsami API. W polu listy rozwijanej są wyświetlane tylko dostępne regiony usługi Zarządzanie interfejsami API. 
9. Wprowadź **nazwa organizacji**. Ta nazwa jest używana w wielu miejscach. Na przykład, tytuł portalu dla deweloperów i nadawcy wiadomości e-mail z powiadomieniem.
10. W **e-mail administratora**Ustaw adres e-mail, na które wszystkie powiadomienia z **zarządzanie interfejsami API** będą wysyłane.
11. W **warstwa cenowa**ustaw **Developer** warstwę, aby korzystać z usługi. Ta warstwa nie jest do użytku produkcyjnego. Aby uzyskać więcej informacji na temat skalowania warstwy usługi API Management, zobacz [uaktualnienia i skalować](upgrade-and-scale.md).
12. Wybierz pozycję **Utwórz**.

    > [!TIP]
    > Zazwyczaj trwa między 20-30 minut na utworzenie usługi Zarządzanie interfejsami API. Wybieranie **Przypnij do pulpitu nawigacyjnego** sprawia, że znajdowanie nowo utworzony usługi.

## <a name="next-steps"></a>Następne kroki

[Publikowanie interfejsu API za pomocą usługi Azure API Management](#api-management-getstarted-publish-api.md)