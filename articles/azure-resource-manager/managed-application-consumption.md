---
title: "Korzystanie z platformy Azure zarządzanych aplikacji | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano, jak klient tworzy platformy Azure zarządzanych aplikacji z plików publikowanych."
services: azure-resource-manager
author: ravbhatnagar
manager: rjmax
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 08/23/2017
ms.author: gauravbh; tomfitz
ms.openlocfilehash: ed8fbaf2a4546c8e31eeced11cd0b5627fd62c0c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/29/2017
---
# <a name="consume-an-internal-managed-application"></a>Korzystanie z wewnętrznej zarządzanej aplikacji

Będzie można korzystać z usługi Azure [zarządzane aplikacje](managed-application-overview.md) które są przeznaczone do członków organizacji. Na przykład można wybrać aplikacje zarządzane przez dział IT, która zapewnić zgodność ze standardami w organizacji. Te aplikacje zarządzane są dostępne za pośrednictwem katalogu usług, a nie w portalu Azure Marketplace.

Przed kontynuowaniem w tym artykule, musi mieć zarządzanej aplikacji dostępnych w katalogu usług dla Twojej subskrypcji. Jeśli ktoś w organizacji nie już utworzony zarządzanej aplikacji, zobacz [publikowania aplikacji zarządzanej na użytek wewnętrzny](managed-application-publishing.md).

Użycie aplikacji zarządzanej można obecnie używać interfejsu wiersza polecenia Azure lub w portalu Azure.

## <a name="create-the-managed-application-by-using-the-portal"></a>Tworzenie aplikacji zarządzanych za pomocą portalu

Aby wdrożyć zarządzanych aplikacji za pośrednictwem portalu, wykonaj następujące kroki:

1. Przejdź do portalu Azure. Wyszukaj **katalogu usług zarządzanych aplikacji**.

   ![Katalog usług zarządzanych aplikacji](./media/managed-application-consumption/create-service-catalog-managed-application.png)

1. Wybierz aplikację, którą ma zostać utworzona z listy dostępnych rozwiązań. Wybierz pozycję **Utwórz**.

   ![Wybór aplikacji zarządzanej](./media/managed-application-consumption/select-offer.png)

1. Podaj wartości parametrów, które są wymagane w celu zapewnienia zasobów. Wybierz **zachodnie centralnej nam** dla lokalizacji. Kliknij przycisk **OK**.

   ![Parametry aplikacji zarządzanych](./media/managed-application-consumption/input-parameters.png)

1. Szablon weryfikuje podanych wartości. Jeśli weryfikacja zakończy się powodzeniem, wybierz **OK** do wdrożenia.

   ![Sprawdzanie poprawności zarządzanej aplikacji](./media/managed-application-consumption/validation.png)

Po zakończeniu wdrożenia odpowiednie zasoby zdefiniowane w szablonie są udostępniane w grupie zasobów zarządzanych podane.

## <a name="create-the-managed-application-by-using-azure-cli"></a>Tworzenie aplikacji zarządzanych za pomocą wiersza polecenia platformy Azure

Istnieją dwa sposoby tworzenia aplikacji zarządzanej przy użyciu wiersza polecenia platformy Azure:

* Polecenie służące do tworzenia aplikacji zarządzanych.
* Polecenie wdrożenia regularne szablonu.

### <a name="use-the-template-deployment-command"></a>Polecenie wdrożenia szablonu

Wdróż plik applianceMainTemplate.json, utworzony przez dostawcę.

Następnie należy utworzyć dwie grupy zasobów. Pierwsza grupa zasobów jest miejsce tworzenia zasobów zarządzanych aplikacji: Microsoft.Solutions/appliances. Drugi grupa zasobów zawiera wszystkie zasoby zdefiniowane w mainTemplate.json. Ta grupa zasobów jest zarządzana przez niezależnego dostawcy oprogramowania.

```azurecli
az group create --name mainResourceGroup --location westcentralus
az group create --name managedResourceGroup --location westcentralus
```

> [!NOTE]
> Użyj `westcentralus` jako lokalizacja grupy zasobów.
>

Aby wdrożyć applianceMainTemplate.json w mainResourceGroup, użyj następującego polecenia:

```azurecli
az group deployment create --name managedAppDeployment --resourceGroup mainResourceGroup --templateUri
```

Po uruchomieniu Powyższy szablon monituje o wartości parametrów, które są zdefiniowane w szablonie. Oprócz parametrów, które są wymagane w celu zapewnienia zasobów w szablonie potrzebne są dwie wartości parametrów klucza:

- **managedResourceGroupId**: identyfikator zawierającego zasoby zdefiniowane w applianceMainTemplate.json grupy zasobów. Identyfikator ma postać `/subscriptions/{subscriptionId}/resourceGroups/{resoureGroupName}`. W powyższym przykładzie jest Identyfikatorem `managedResourceGroup`.
- **applianceDefinitionId**: identyfikator zasobu definicji zarządzanej aplikacji. Ta wartość jest dostarczana przez niezależnego dostawcy oprogramowania.

> [!NOTE]
> Wydawca muszą udzielić dostępu do grupy zasobów, który zawiera definicję aplikacji zarządzanych. Definicja zasobu jest tworzony w subskrypcji wydawcy. W związku z tym użytkownika, grupy użytkowników lub aplikacji w dzierżawie klienta musi dostęp do odczytu do tego zasobu.

Po pomyślnym wdrożeniu, zobacz tworzone aplikacja zarządzana w mainResourceGroup. Zasób konto magazynu jest tworzony w managedResourceGroup.

### <a name="use-the-create-command"></a>Użyj polecenia create

Można użyć `az managedapp create` polecenie, aby utworzyć zarządzaną aplikację z definicji zarządzanych aplikacji.

```azurecli
az managedapp create --name ravtestappliance401 --location "westcentralus"
    --kind "Servicecatalog" --resource-group "ravApplianceCustRG401"
    --managedapp-definition-id "/subscriptions/{guid}/resourceGroups/ravApplianceDefRG401/providers/Microsoft.Solutions/applianceDefinitions/ravtestAppDef401"
    --managed-rg-id "/subscriptions/{guid}/resourceGroups/ravApplianceCustManagedRG401"
    --parameters "{\"storageAccountName\": {\"value\": \"ravappliancedemostore1\"}}"
    --debug
```

* **Identyfikator urządzenia definicji**: identyfikator zasobu w definicji zarządzanych aplikacji utworzony w poprzednim kroku. Aby uzyskać ten identyfikator, uruchom następujące polecenie:

  ```azurecli
  az appliance definition show -n ravtestAppDef1 -g ravApplianceRG2
  ```

  To polecenie zwraca definicji zarządzanych aplikacji. Należy wartość właściwości identyfikator.

* **zarządzane-zarządcy zasobów id**: Nazwa grupy zasobów, zawierające zasoby zdefiniowane w applianceMainTemplate.json. Ta grupa zasobów to grupa zasobów zarządzanych. Jest on zarządzany przez wydawcę. Jeśli nie istnieje, jest tworzony automatycznie.
* **Grupa zasobów**: grupy zasobów, w którym utworzono zasób zarządzanej aplikacji. Zasób Microsoft.Solutions/appliance znajduje się w tej grupie zasobów.
* **Parametry**: parametry, które są wymagane przez zasoby zdefiniowane w applianceMainTemplate.json.

## <a name="known-issues"></a>Znane problemy

Ta wersja zapoznawcza zawiera następujące problemy:

* Podczas tworzenia aplikacji zarządzanej pojawia się 500 Wewnętrzny błąd serwera. Jeśli napotkasz problem prawdopodobnie występować sporadycznie. Spróbuj ponownie wykonać operację.
* Nowa grupa zasobów jest wymagany dla grupy zasobów zarządzanych. Jeśli używasz istniejącej grupy zasobów, wdrożenie zakończy się niepowodzeniem.
* Należy utworzyć grupę zasobów, która zawiera zasób Microsoft.Solutions/appliances w **westcentralus** lokalizacji.

## <a name="next-steps"></a>Następne kroki

* Aby obejrzeć wprowadzenie do aplikacji zarządzanych, zobacz [Omówienie aplikacji zarządzanych](managed-application-overview.md).
* Aby dowiedzieć się, jak publikowanie aplikacji zarządzanych katalogu usług, zobacz [Utwórz i publikowanie aplikacji katalogu usług zarządzanych](managed-application-publishing.md).
* Informacje dotyczące publikowania zarządzanych aplikacji w portalu Azure Marketplace, zobacz [Azure zarządzanych aplikacji w witrynie Marketplace](managed-application-author-marketplace.md).
* Aby dowiedzieć się, jak korzystanie z zarządzanych aplikacji z portalu Marketplace, zobacz [korzystać z platformy Azure zarządzanych aplikacji w witrynie Marketplace](managed-application-consume-marketplace.md).
