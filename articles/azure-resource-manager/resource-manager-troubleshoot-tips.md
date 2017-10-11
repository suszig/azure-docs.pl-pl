---
title: "Zrozumienie błędy wdrożenia usługi Azure | Dokumentacja firmy Microsoft"
description: "Opisuje sposób dowiedzieć się więcej o błędach wdrożenia usługi Azure."
services: azure-resource-manager
documentationcenter: 
tags: top-support-issue
author: tfitzmac
manager: timlt
editor: tysonn
keywords: "Błąd wdrażania, wdrożenia usługi azure wdrażanie na platformie azure"
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: support-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/12/2017
ms.author: tomfitz
ms.openlocfilehash: b67bb30fa259fa08e37e11afec724c8b8c3eb633
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/29/2017
---
# <a name="understand-azure-deployment-errors"></a>Zrozumienie błędy wdrożenia usługi Azure
W tym temacie opisano błędy wdrożenia i jak mogą odnajdować więcej informacji o błędzie. Dla rozwiązania typowych błędów wdrażania, zobacz [Rozwiąż typowe błędy wdrożenia usługi Azure z usługą Azure Resource Manager](resource-manager-common-deployment-errors.md).

## <a name="two-types-of-errors"></a>Dwa typy błędów
Istnieją dwa typy błędów, które mogą odbierać:

* Błędy sprawdzania poprawności
* Błędy wdrożenia

Poniższa ilustracja przedstawia przykładowy dziennik aktywności subskrypcji. Reprezentuje dwa wdrożenia. Jedno wdrożenie szablonu nie powiodło się sprawdzanie poprawności (**weryfikacji**) i nie kontynuować. W ramach wdrożenia, szablon przeszły sprawdzanie poprawności, ale nie powiodło się podczas tworzenia zasobów (**zapisu wdrożeń**). 

![Pokaż kod błędu:](./media/resource-manager-troubleshoot-tips/show-activity-log.png)

Błędy sprawdzania poprawności wynikają z scenariusze, które można określić przed przystąpieniem do wdrożenia. Obejmują one błędy składniowe w szablonie lub w trakcie wdrażania zasobów, które może spowodować przekroczenie przydziałami subskrypcji. Błędy wdrożenia wynikają z warunków, które występują w trakcie procesu wdrażania. Obejmują one próby uzyskania dostępu do zasobu, który jest wdrażany jednocześnie.

Oba typy błędów zwraca kod błędu, który umożliwia rozwiązywanie problemów z wdrażaniem. Oba typy błędy są wyświetlane w [dziennik aktywności](resource-group-audit.md). Błędy sprawdzania poprawności nie pojawiają się jednak w historii wdrożenia, ponieważ wdrożenie nigdy nie jest uruchomiony.

## <a name="determine-error-code"></a>Określić kod błędu:

Informacje na temat błędu analizując komunikat o błędzie i kod błędu. [Rozwiąż typowe błędy wdrożenia usługi Azure z usługą Azure Resource Manager](resource-manager-common-deployment-errors.md) artykule wymieniono rozwiązania przez kod błędu. W tym temacie przedstawiono sposób użycia portalu Azure, aby dowiedzieć się, kod błędu.

### <a name="validation-errors"></a>Błędy sprawdzania poprawności

W przypadku wdrażania za pośrednictwem portalu, zobaczysz błąd sprawdzania poprawności po przesłaniu wartości.

![Pokaż błąd sprawdzania poprawności portalu](./media/resource-manager-troubleshoot-tips/validation-error.png)

Zaznacz wiadomość, aby uzyskać więcej informacji. Na poniższej ilustracji, zobacz **InvalidTemplateDeployment** błąd i komunikat informujący zasad zablokowane wdrożenia.

![Pokaż szczegóły sprawdzania poprawności](./media/resource-manager-troubleshoot-tips/validation-details.png)

### <a name="deployment-errors"></a>Błędy wdrożenia

Podczas operacji pozytywnej weryfikacji, ale nie powiodło się podczas wdrażania, zostanie wyświetlony błąd w powiadomieniach. Wybierz powiadomienie.

![Błąd powiadomienia](./media/resource-manager-troubleshoot-tips/notification.png)

Możesz wyświetlić więcej szczegółów dotyczących wdrożenia. Wybierz opcję, aby uzyskać więcej informacji o tym błędzie.

![Wdrażanie nie powiodło się](./media/resource-manager-troubleshoot-tips/deployment-failed.png)

Zostanie wyświetlony komunikat o błędzie i kody błędów. Należy zauważyć, że istnieją dwa kody błędów. Pierwszy kod błędu (**niepowodzenia wdrożenia**) jest błąd ogólny, który nie zapewnia szczegółowe informacje, musisz poprawić błąd. Drugi kod błędu (**StorageAccountNotFound**) zawiera szczegółowe informacje, należy. 

![Szczegóły błędu](./media/resource-manager-troubleshoot-tips/error-details.png)

## <a name="enable-debug-logging"></a>Włączenie rejestrowania debugowania
Czasami możesz uzyskać więcej informacji dotyczących żądania i odpowiedzi, aby dowiedzieć się, co poszło źle. Przy użyciu programu PowerShell lub interfejsu wiersza polecenia Azure, mogą żądać, aby uzyskać dodatkowe informacje jest rejestrowane podczas wdrażania.

- PowerShell

   W programie PowerShell, należy ustawić **DeploymentDebugLogLevel** parametr do wszystkich, obiektu ResponseContent lub RequestContent.

  ```powershell
  New-AzureRmResourceGroupDeployment -ResourceGroupName examplegroup -TemplateFile c:\Azure\Templates\storage.json -DeploymentDebugLogLevel All
  ```

   Sprawdź żądanie zawartości za pomocą następującego polecenia cmdlet:

  ```powershell
  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName storageonly -ResourceGroupName startgroup).Properties.request | ConvertTo-Json
  ```

   Lub zawartości z odpowiedzi:

  ```powershell
  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName storageonly -ResourceGroupName startgroup).Properties.response | ConvertTo-Json
  ```

   Te informacje mogą pomóc w określeniu, czy wartość w szablonie jest są ustawione niepoprawnie.

- Interfejs wiersza polecenia platformy Azure

   Sprawdź operacje wdrażania przy użyciu następującego polecenia:

  ```azurecli
  az group deployment operation list --resource-group ExampleGroup --name vmlinux
  ```

- Szablon zagnieżdżony

   Rejestrowanie informacji debugowania dla szablonu zagnieżdżonego, należy użyć **debugSetting** elementu.

  ```json
  {
      "apiVersion": "2016-09-01",
      "name": "nestedTemplate",
      "type": "Microsoft.Resources/deployments",
      "properties": {
          "mode": "Incremental",
          "templateLink": {
              "uri": "{template-uri}",
              "contentVersion": "1.0.0.0"
          },
          "debugSetting": {
             "detailLevel": "requestContent, responseContent"
          }
      }
  }
  ```


## <a name="create-a-troubleshooting-template"></a>Tworzenie szablonu rozwiązywania problemów
W pewnych sytuacjach Najprostszym sposobem Rozwiązywanie problemów z szablonu jest przetestowanie jej części. Uproszczona można utworzyć szablonu, który pozwala skupić się na części podejrzenie, że przyczyną jest błąd. Na przykład załóżmy, że otrzymujesz błąd podczas odwoływania się do zasobu. Zamiast dotyczącą całą szablonu, utworzyć szablon, który zwraca składnik, który może być przyczyną tego problemu. Możesz określić, czy jest przekazywany w parametrach prawo poprawnie, przy użyciu funkcji szablonu i pobieranie zasobu oczekiwać może pomóc.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageName": {
        "type": "string"
    },
    "storageResourceGroup": {
        "type": "string"
    }
  },
  "variables": {},
  "resources": [],
  "outputs": {
    "exampleOutput": {
        "value": "[reference(resourceId(parameters('storageResourceGroup'), 'Microsoft.Storage/storageAccounts', parameters('storageName')), '2016-05-01')]",
        "type" : "object"
    }
  }
}
```

Lub Załóżmy, że pojawiły się błędy wdrożenia, które Twoim zdaniem są związane z niepoprawnie ustawione zależności. Przetestuj szablonu przez podzielenie go na uproszczonej szablonów. Najpierw Utwórz szablon, który wdraża jednego zasobu (np. programu SQL Server). Podczas pracy upewnić się, że ten zasób zdefiniowany prawidłowo, Dodaj zasób zależy od niego (na przykład baza danych SQL). Jeśli te dwa zasoby zdefiniowany prawidłowo, należy dodać inne zasoby zależne (na przykład zasady inspekcji). Between każdego wdrożenia testu Usuń grupę zasobów, aby mieć pewność, że odpowiednio testowania zależności. 

## <a name="check-deployment-sequence"></a>Sprawdź wdrożenie sekwencji

Wiele błędów wdrażania się tak zdarzyć, gdy zasoby są wdrożone w nieoczekiwany sekwencji. Te błędy wystąpić, gdy zależności nie są poprawnie ustawione. Brak wymaganych zależności, jeden zasób próbuje użyć wartości innego zasobu, ale innych jeszcze nie istnieje. Otrzymasz komunikat o błędzie informujący, że zasób nie został znaleziony. Mogą występować sporadycznie tego typu błędu dla każdego zasobu czas wdrażania może się różnić. Na przykład pierwsza próba wdrażanie zasobów powiedzie się, ponieważ wymagany zasób jest przeprowadzany losowo w czasie. Jednak z druga próba nie powiedzie się, ponieważ wymagany zasób nie została ukończona w czasie. 

Jednak aby uniknąć ustawienie zależności, które nie są wymagane. Gdy niepotrzebne zależności, można przedłużyć czas wdrażania, zapobiegając zasoby, które nie są wzajemnie zależne od wdrażany równolegle. Ponadto można utworzyć zależności cykliczne, które blokuje wdrożenie. [Odwołania](resource-group-template-functions-resource.md#reference) funkcja tworzy zależność niejawnych na zasobu dołączonego przez odwołanie, po wdrożeniu tego zasobu w tym samym szablonie. W związku z tym może mieć zależności więcej niż określa zależności **dependsOn** właściwości. [ResourceId](resource-group-template-functions-resource.md#resourceid) funkcji nie utworzyć niejawnego zależności lub Sprawdzanie, czy zasób istnieje.

W przypadku wystąpienia problemów zależności, musisz uzyskać wgląd w kolejności wdrażania zasobów. Aby wyświetlić kolejność operacji wdrażania:

1. Wybierz historii wdrożenia dla grupy zasobów.

   ![Wybierz Historia wdrażania](./media/resource-manager-troubleshoot-tips/select-deployment.png)

2. Wybierz wdrożenie z historii, a następnie wybierz **zdarzenia**.

   ![Wybierz zdarzenia wdrożenia](./media/resource-manager-troubleshoot-tips/select-deployment-events.png)

3. Sprawdź, czy sekwencję zdarzeń dla każdego zasobu. Należy zwrócić uwagę na stan każdej operacji. Na przykład na poniższej ilustracji przedstawiono trzy konta magazynu, które wdrożone równolegle. Zwróć uwagę, że trzy konta magazynu są uruchamiane w tym samym czasie.

   ![Wdrożenie równoległe](./media/resource-manager-troubleshoot-tips/deployment-events-parallel.png)

   Na następnej ilustracji przedstawiono trzy konta magazynu, które nie są wdrażane równolegle. Pierwsze konto magazynu zależy od drugiego konta magazynu, a trzeci konta magazynu zależy od drugiego konta magazynu. W związku z tym pierwsze konto magazynu jest uruchomiona, zaakceptowane i ukończone przed uruchomieniem następnego.

   ![kolejne wdrożenia](./media/resource-manager-troubleshoot-tips/deployment-events-sequence.png)

Nawet w przypadku bardziej złożonych zadań można użyć tę samą metodę, aby dowiedzieć się, gdy wdrożenie zostanie rozpoczęte i zakończone dla każdego zasobu. Przejrzyj zdarzeń wdrażania czy sekwencja jest inny niż oczekiwany. Jeśli tak, można obliczyć ponownie zależności dla tego zasobu.

Menedżer zasobów identyfikuje zależności cykliczne podczas walidacji szablonu. Zwraca się, że istnieje komunikat o błędzie stwierdzający, w szczególności zależność cykliczną. Aby rozwiązać zależność cykliczną:

1. W szablonie można znaleźć zasobu określonego w zależność cykliczną. 
2. Dla tego zasobu, sprawdź **dependsOn** właściwości i wszelkie zastosowania **odwołania** sprawdzić zasobów, do których zależy od funkcji. 
3. Sprawdź, czy te zasoby, aby wyświetlić zasobów, do których one zależą. Wykonaj zależności, dopóki zauważysz z zasobem, który jest zależny od zasobu oryginalnego.
5. Zasoby związane z zależność cykliczną, należy dokładnie zbadać wszystkie użycia **dependsOn** właściwości, aby zidentyfikować wszelkie zależności, które nie są wymagane. Usuń te zależności. Jeśli nie wiesz, czy zależność jest potrzebny, spróbuj go usunąć. 
6. Należy ponownie wdrożyć szablon.

Usuwanie wartości z **dependsOn** właściwości, które mogą powodować błędy podczas wdrażania szablonu. Jeśli wystąpi błąd, Dodaj zależności do szablonu. 

Jeśli takie podejście nie rozwiązuje zależność cykliczną, rozważ migrację część logiki wdrażania zasobów podrzędnych (na przykład rozszerzenia lub ustawienia konfiguracji). Skonfiguruj te zasoby podrzędne do wdrożenia po zasoby związane z zależność cykliczną. Załóżmy na przykład wdrażasz dwóch maszyn wirtualnych, ale należy ustawić na każdej z nich, która odwołuje się do innych właściwości. Można je wdrożyć w następującej kolejności:

1. vm1
2. maszyny vm2
3. Rozszerzenie na vm1 zależy od vm1 i maszyny vm2. Rozszerzenie ustawia wartości vm1, który otrzymuje od maszyny vm2.
4. Rozszerzenie maszyny vm2 zależy od vm1 i maszyny vm2. Rozszerzenie ustawia wartości maszyny vm2, który otrzymuje od vm1.

Te same podejście działa w przypadku aplikacji usługi App Service. Rozważ przeniesienie wartości konfiguracji do zasobu podrzędnego zasobów aplikacji. Można wdrożyć dwie aplikacje sieci web w następującej kolejności:

1. webapp1
2. webapp2
3. Konfiguracja webapp1 zależy od webapp1 i webapp2. Zawiera ustawienia aplikacji wartościami z webapp2.
4. Konfiguracja webapp2 zależy od webapp1 i webapp2. Zawiera ustawienia aplikacji wartościami z webapp1.


## <a name="next-steps"></a>Następne kroki
* Dla rozwiązania typowych błędów wdrażania, zobacz [Rozwiąż typowe błędy wdrożenia usługi Azure z usługą Azure Resource Manager](resource-manager-common-deployment-errors.md).
* Aby dowiedzieć się więcej o inspekcji akcji, zobacz [inspekcji operacji za pomocą Menedżera zasobów](resource-group-audit.md).
* Aby dowiedzieć się więcej o akcjach, aby określić błędy podczas wdrażania, zobacz [wyświetlić operacje wdrażania](resource-manager-deployment-operations.md).
