---
title: "Zasady usługi Azure umożliwia tworzenie i zarządzanie zasadami, by wymuszał zgodność organizacji | Dokumentacja firmy Microsoft"
description: "Zasady usługi Azure umożliwia wymuszanie standardy, spełnia wymagania dotyczące przepisów dotyczących zgodności i inspekcji kontrolę kosztów, zachowanie spójności bezpieczeństwo i wydajność i nałożyć zasady szeroki projektu przedsiębiorstwa."
services: azure-policy
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 12/20/2017
ms.topic: tutorial
ms.service: azure-policy
ms.custom: mvc
ms.openlocfilehash: 27056d5be99a6d2737e5583b0cda6f2e24895901
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/21/2017
---
# <a name="create-and-manage-policies-to-enforce-compliance"></a>Tworzenie i zarządzanie zasadami, by wymuszał zgodność

Opis sposobu tworzenia i zarządzania zasadami w usłudze Azure jest ważne dla pozostaje zgodne ze standardami firmy, a umów dotyczących poziomu usług. W tym samouczku nauczysz za pomocą zasad usługi Azure wykonywać niektórych bardziej popularne zadania związane z tworzenia, przypisywania i zarządzania zasadami w organizacji, takich jak:

> [!div class="checklist"]
> * Przypisanie zasad, aby wymusić warunek dla zasobów tworzonych w przyszłości
> * Utwórz i przypisz inicjatywy definicji w celu śledzenia zgodności dla wielu zasobów
> * Rozwiąż niezgodnych lub odmówiono zasobu
> * Wdrożenie nowych zasad w organizacji

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="assign-a-policy"></a>Przypisać zasady

Pierwszym etapem Wymuszanie zgodności z zasadami Azure jest można przypisać definicji zasad. Definiuje definicję zasad w obszarze jakie warunek, a zasady są wymuszane i jaka akcja ma być. W tym przykładzie mamy przypisać definicję wbudowanych zasad o nazwie *wymagają programu SQL Server wersji 12.0*, aby wymusić warunku, że wszystkie bazy danych programu SQL Server musi być v12.0, aby było zgodne.

1. Uruchom usługę Azure zasad w portalu Azure wyszukiwanie i wybierając **zasad** w okienku po lewej stronie.

   ![Wyszukiwanie zasad](media/assign-policy-definition/search-policy.png)

2. Wybierz pozycję **Przypisania** w lewym okienku na stronie Azure Policy. Przypisanie jest zasada, która została przypisana do odbywać się w określonym zakresie.
3. Wybierz pozycję **Przypisz zasady** w górnej części okienka **Przypisania**.

   ![Przypisywanie definicji zasad](media/create-manage-policy/select-assign-policy.png)

4. Na stronie **Przypisz zasady** kliknij ![przycisk definicji zasad](media/assign-policy-definition/definitions-button.png) obok pola **Zasady**, aby otworzyć listę dostępnych definicji.

   ![Otwieranie dostępnych definicji zasad](media/create-manage-policy/open-policy-definitions.png)

5. Wybierz **wymagają programu SQL Server w wersji 12.0**.

   ![Zlokalizuj zasady](media/create-manage-policy/select-available-definition.png)

6. Określ właściwość **Nazwa** przypisania zasad. W takim przypadku można użyć *wymagają programu SQL Server 12.0*. Można również dodać opcjonalny **Opis**. Opis zawiera szczegóły dotyczące sposobu przypisania zasad zapewnia wszystkie serwery SQL utworzone w tym środowisku są wersji 12.0.
7. Zmień warstwę cenową na **Standardowa**, aby upewnić się, że zasady zostaną zastosowane do istniejących zasobów.

   W ramach usługi Azure Policy dostępne są dwie warstwy cenowe — *Bezpłatna* i *Standardowa*. W warstwie Bezpłatna można wprowadzać zasady tylko dla przyszłych zasobów, podczas gdy w warstwie Standardowa można stosować je także do istniejących zasobów, co zapewnia lepsze zrozumienie stanu zgodności z przepisami. Usługa jest nadal dostępna w ograniczonej wersji zapoznawczej, dlatego nie wprowadziliśmy jeszcze modelu cenowego, w związku z czym nie otrzymasz rachunku za wybranie warstwy *Standardowa*. Aby dowiedzieć się więcej o cenach, zobacz: [Cennik usługi Azure Policy](https://acom-milestone-ignite.azurewebsites.net/pricing/details/azure-policy/).

8. Wybierz **zakres** -subskrypcji (lub grupy zasobów) został wcześniej zarejestrowany. Zakres określa, jakie zasoby lub grupy zasobów są wymuszane w ramach przypisania zasad. Może obejmować zarówno subskrypcje, jak i grupy zasobów.

   W tym przykładzie używamy tej subskrypcji - **Azure Analytics pojemności deweloperów**. Subskrypcji będą się różnić.

10. Wybierz opcję **Przypisz**.

## <a name="implement-a-new-custom-policy"></a>Wdrożenie nowych zasad niestandardowych

Teraz, możemy przypisane definicji zasad, zamierzamy utworzyć nowe zasady w celu ograniczenia kosztów przez zapewnienie im, które maszyny wirtualne utworzone w całym środowisku nie może być w serii G. W ten sposób za każdym razem, gdy użytkownik w Twojej organizacji próbuje utworzyć maszynę Wirtualną w serii G, żądanie zostanie uzyskać odrzucone.

1. Wybierz **definicji** w obszarze **tworzenie** w okienku po lewej stronie.

   ![Definicja w obszarze tworzenia](media/create-manage-policy/definition-under-authoring.png)

2. Wybierz **+ definicji zasad**.
3. Wprowadź następujące informacje:

   - Nazwa definicji zasad - *wymagają wirtualna jednostki SKU mniejszy niż serii G*
   - Opis definicji zasad ma na celu — tej definicji zasad wymusza, że wszystkie maszyny wirtualne utworzone w tym zakresie mają jednostki SKU mniejszy niż serii G będzie zmniejszenie kosztów.
   - Subskrypcji, w którym definicji zasad będzie funkcjonować — w takim przypadku naszej definicji zasad będzie funkcjonować **Advisor Analytics pojemności deweloperów**. Listy subskrypcji będą się różnić.
   - Skopiuj poniższy kod json, a następnie zaktualizuj go do potrzeb z:
      - Parametry zasad.
      - Zasady reguły/warunków, w tym przypadku — równa serii G rozmiar jednostki SKU maszyny Wirtualnej
      - Wpływ zasad, w tym przypadku — **Odmów**.

    Oto, jak powinna wyglądać json. Wklej poprawione kodu do portalu Azure.

    ```json
{
    "policyRule": {
      "if": {
        "allOf": [
          {
            "field": "type",
            "equals": "Microsoft.Compute/virtualMachines"
          },
          {
            "field": "Microsoft.Compute/virtualMachines/sku.name",
            "like": "Standard_G*"
          }
        ]
      },
      "then": {
        "effect": "deny"
      }
    }
}
    ```

    Aby wyświetlić przykłady kodu json, przeczytaj [szablony zasad Azure](json-samples.md) artykułu.

4. Wybierz pozycję **Zapisz**.

## <a name="create-a-policy-definition-with-rest-api"></a>Utwórz definicję zasad z interfejsu API REST

Zasady można utworzyć przy użyciu interfejsu API REST dla definicji zasad. Interfejs API REST umożliwia tworzenie i usuwanie definicji zasad oraz uzyskać informacje o istniejących definicji.
Aby utworzyć definicję zasad, skorzystaj z następującego przykładu:

```
PUT https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.authorization/policydefinitions/{policyDefinitionName}?api-version={api-version}

```
Dołącz treści żądania, podobnie do poniższego przykładu:

```
{
  "properties": {
    "parameters": {
      "allowedLocations": {
        "type": "array",
        "metadata": {
          "description": "The list of locations that can be specified when deploying resources",
          "strongType": "location",
          "displayName": "Allowed locations"
        }
      }
    },
    "displayName": "Allowed locations",
    "description": "This policy enables you to restrict the locations your organization can specify when deploying resources.",
    "policyRule": {
      "if": {
        "not": {
          "field": "location",
          "in": "[parameters('allowedLocations')]"
        }
      },
      "then": {
        "effect": "deny"
      }
    }
  }
}
```

## <a name="create-a-policy-definition-with-powershell"></a>Utwórz definicję zasad przy użyciu programu PowerShell

Przed kontynuowaniem w przykładzie programu PowerShell, upewnij się, że zainstalowano najnowszą wersję programu Azure PowerShell. Parametry zasad zostały dodane w wersji 3.6.0. Jeśli masz starszą wersję przykłady zwrócony błąd wskazujący, że nie można odnaleźć parametru.

Można utworzyć definicji zasad przy użyciu opcji `New-AzureRmPolicyDefinition` polecenia cmdlet.

Aby utworzyć definicję zasad z pliku, należy przekazać ścieżkę do pliku. Dla pliku zewnętrznego skorzystaj z następującego przykładu:

```
$definition = New-AzureRmPolicyDefinition `
    -Name denyCoolTiering `
    -DisplayName "Deny cool access tiering for storage" `
    -Policy 'https://raw.githubusercontent.com/Azure/azure-policy-samples/master/samples/Storage/storage-account-access-tier/azurepolicy.rules.json'
```

Do użytku lokalnego pliku skorzystaj z następującego przykładu:

```
$definition = New-AzureRmPolicyDefinition `
    -Name denyCoolTiering `
    -Description "Deny cool access tiering for storage" `
    -Policy "c:\policies\coolAccessTier.json"
```

Aby utworzyć definicję zasad przy użyciu reguły z wbudowanego, skorzystaj z następującego przykładu:

```
$definition = New-AzureRmPolicyDefinition -Name denyCoolTiering -Description "Deny cool access tiering for storage" -Policy '{
  "if": {
    "allOf": [
      {
        "field": "type",
        "equals": "Microsoft.Storage/storageAccounts"
      },
      {
        "field": "kind",
        "equals": "BlobStorage"
      },
      {
        "not": {
          "field": "Microsoft.Storage/storageAccounts/accessTier",
          "equals": "cool"
        }
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}'
```

Dane wyjściowe są przechowywane w `$definition` obiektu, który jest używany podczas przypisywania zasad.
Poniższy przykład tworzy definicji zasad, które zawiera parametry:

```
$policy = '{
    "if": {
        "allOf": [
            {
                "field": "type",
                "equals": "Microsoft.Storage/storageAccounts"
            },
            {
                "not": {
                    "field": "location",
                    "in": "[parameters(''allowedLocations'')]"
                }
            }
        ]
    },
    "then": {
        "effect": "Deny"
    }
}'

$parameters = '{
    "allowedLocations": {
        "type": "array",
        "metadata": {
          "description": "The list of locations that can be specified when deploying storage accounts.",
          "strongType": "location",
          "displayName": "Allowed locations"
        }
    }
}'

$definition = New-AzureRmPolicyDefinition -Name storageLocations -Description "Policy to specify locations for storage accounts." -Policy $policy -Parameter $parameters
```

## <a name="view-policy-definitions"></a>Wyświetlanie definicji zasad

Aby wyświetlić wszystkie definicje zasad w ramach subskrypcji, użyj następującego polecenia:

```
Get-AzureRmPolicyDefinition
```

Zwraca wszystkie definicje dostępnych zasad, w tym wbudowane zasad. Każda zasada jest zwracany w następującym formacie:

```
Name               : e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceId         : /providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceName       : e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceType       : Microsoft.Authorization/policyDefinitions
Properties         : @{displayName=Allowed locations; policyType=BuiltIn; description=This policy enables you to
                     restrict the locations your organization can specify when deploying resources. Use to enforce
                     your geo-compliance requirements.; parameters=; policyRule=}
PolicyDefinitionId : /providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c
```

## <a name="create-a-policy-definition-with-azure-cli"></a>Utwórz definicję zasad z wiersza polecenia platformy Azure

Możesz utworzyć definicję zasad przy użyciu wiersza polecenia platformy Azure przy użyciu polecenia definicji zasad.
Aby utworzyć definicję zasad przy użyciu reguły z wbudowanego, skorzystaj z następującego przykładu:

```
az policy definition create --name denyCoolTiering --description "Deny cool access tiering for storage" --rules '{
  "if": {
    "allOf": [
      {
        "field": "type",
        "equals": "Microsoft.Storage/storageAccounts"
      },
      {
        "field": "kind",
        "equals": "BlobStorage"
      },
      {
        "not": {
          "field": "Microsoft.Storage/storageAccounts/accessTier",
          "equals": "cool"
        }
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}'
```

## <a name="view-policy-definitions"></a>Wyświetlanie definicji zasad

Aby wyświetlić wszystkie definicje zasad w ramach subskrypcji, użyj następującego polecenia:

```
az policy definition list
```

Zwraca wszystkie definicje dostępnych zasad, w tym wbudowane zasad. Każda zasada jest zwracany w następującym formacie:

```
{                                                            
  "description": "This policy enables you to restrict the locations your organization can specify when deploying resources. Use to enforce your geo-compliance requirements.",                      
  "displayName": "Allowed locations",
  "id": "/providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c",
  "name": "e56962a6-4747-49cd-b67b-bf8b01975c4c",
  "policyRule": {
    "if": {
      "not": {
        "field": "location",
        "in": "[parameters('listOfAllowedLocations')]"
      }
    },
    "then": {
      "effect": "Deny"
    }
  },
  "policyType": "BuiltIn"
}
```

## <a name="create-and-assign-an-initiative-definition"></a>Utwórz i przypisz inicjatywy definicji

Z inicjatywy definicji można grupować kilka definicje zasad jednego celu nadrzędna. Możesz utworzyć inicjatywy definicji, aby upewnić się, że zasoby w zakresie definicji pozostają zgodne z definicjami zasad, składające się inicjatywy definicji.  Zobacz [Przegląd zasad Azure](./azure-policy-introduction.md) uzyskać więcej informacji na temat inicjatywy definicje.

### <a name="create-an-initiative-definition"></a>Utwórz definicję inicjatywy

1. Wybierz **definicje** w obszarze **tworzenie** w lewym okienku.

   ![Wybierz definicje](media/create-manage-policy/select-definitions.png)

2. Wybierz **definicji inicjatywy** w górnej części strony, wybranie tej opcji powoduje przejście do **definicji inicjatywy** formularza.
3. Wprowadź nazwę i opis tej inicjatywy.

   W tym przykładzie chcemy zapewnić, że zasoby są z aktualizacjami definicji zasad uzyskiwanie bezpiecznego, nazwą tej inicjatywy będzie **uzyskać bezpieczny**, oraz opis byłoby: **został tej inicjatywy utworzone w celu obsługi wszystkie definicje zasady skojarzone z zabezpieczanie zasobów**.

   ![Definicja inicjatywy](media/create-manage-policy/initiative-definition.png)

4. Przejrzyj listę **dostępnych definicji** i wybierz definicje zasad, które chcesz dodać do tej inicjatywy. Dla naszych **uzyskać bezpieczny** inicjatywy, Dodaj następujące wbudowane definicje zasad:
   - Wymaga programu SQL Server w wersji 12.0
   - Monitorowanie aplikacji sieci web niechronione w Centrum zabezpieczeń.
   - Monitor ograniczająca sieci w w Centrum zabezpieczeń.
   - Monitoruj aplikację możliwe listę dozwolonych podobnej w Centrum zabezpieczeń.
   - Monitorowanie niezaszyfrowane dyski maszyny Wirtualnej w Centrum zabezpieczeń.

   ![Definicje inicjatywy](media/create-manage-policy/initiative-definition-2.png)

   Po wybraniu definicje zasad z listy zobaczysz go w obszarze **zasad i parametry**, jak pokazano powyżej.

5. Wybierz pozycję **Utwórz**.

### <a name="assign-an-initiative-definition"></a>Przypisz inicjatywy definicji

1. Przejdź do **definicje** w obszarze **tworzenie**.
2. Wyszukaj **uzyskać bezpieczny** inicjatywy definicji został utworzony.
3. Wybierz inicjatywy definicji, a następnie wybierz **przypisać**.

   ![Przypisz definicji](media/create-manage-policy/assign-definition.png)

4. Wypełnianie **przypisania** formularza, wpisując:
   - Nazwa: Pobierz bezpieczny przypisania
   - Opis: inicjatywy przypisania jest dostosowane do wymuszania tej grupy definicji zasad w **Azure Advisor pojemności deweloperów** subskrypcji
   - Warstwa cenowa: standardowy
   - zakres, które chcesz zastosować do przypisania: **deweloperów pojemności Advisor Azure**

5. Wybierz opcję **Przypisz**.

## <a name="resolve-a-non-compliant-or-denied-resource"></a>Rozwiąż niezgodnych lub odmówiono zasobu

Następujący przykład powyżej, po przypisaniu do wymagają programu SQL server w wersji 12.0 definicji zasad utworzonych za pomocą innej wersji programu SQL server może pobrać odmowa. W tej sekcji możemy Cię Instruktaż rozpoznawania odmowy próba utworzenia programu SQL server w innej wersji przez zażądanie wykluczenie.

1. Wybierz pozycję **Przypisania** w lewym okienku.
2. Przeglądaj wszystkie przypisania zasady, a następnie uruchom *wymagają programu SQL Server 12.0* przypisania.
3. Żądanie wykluczenie dla grupy zasobów, w których chcesz utworzyć program SQL server. W tym przypadku są bez Microsoft.Sql/servers/databases: *baconandbeer/Cheetos* i *baconandbeer/Chorizo*.

   ![Żądanie wykluczeń](media/create-manage-policy/request-exclusion.png)

   Inne metody można rozpoznać odmowy zasobu to: osiągnięcia do kontaktu skojarzonych z zasadami, jeśli masz silne uzasadnienie konieczności utworzone w programie SQL server i bezpośredniego edytowania zasad, jeśli użytkownik ma dostęp do.

4. Wybierz pozycję **Zapisz**.

W tej sekcji możesz rozwiązać odmowa próby utworzenia programu SQL server w wersji 12.0, żądając wykluczenie do zasobów.

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Jeśli zamierzasz kontynuować pracę z kolejnych samouczkach nie wyczyścić zasoby utworzone w tym przewodniku. Jeśli nie zamierzasz kontynuować, następujące kroki umożliwiają usunięcie wszystkich przypisań lub definicje utworzone powyżej:

1. Wybierz **definicje** (lub **przypisania** Jeśli próbujesz usunąć przypisanie) w lewym okienku.
2. Wyszukaj nowe inicjatywy lub zasad definicji (lub przydziału), który został właśnie utworzony.
3. Wybierz Wielokropek na końcu definicji lub przypisania, a następnie wybierz **usunąć definicji** (lub **usunąć przypisanie**).

## <a name="next-steps"></a>Kolejne kroki

W tym samouczku pomyślnie wykonano następujące czynności:

> [!div class="checklist"]
> * Przypisane zasad, aby wymusić warunek dla zasobów tworzonych w przyszłości
> * Utworzyć i przypisać inicjatywy definicji w celu śledzenia zgodności dla wielu zasobów
> * Rozwiązane niezgodnych lub odmówiono zasobu
> * Wdrożone nowe zasady w organizacji

Aby dowiedzieć się więcej na temat struktury definicje zasad, przyjrzeć się w tym artykule:

> [!div class="nextstepaction"]
> [Struktura definicji zasad Azure](policy-definition.md)
