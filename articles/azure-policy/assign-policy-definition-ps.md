---
title: "Tworzenie przypisania zasad za pomocą programu PowerShell w celu zidentyfikowania niezgodnych zasobów w środowisku Azure | Microsoft Docs"
description: "Używając programu PowerShell, utwórz przypisanie usługi Azure Policy, aby zidentyfikować niezgodne zasoby."
services: azure-policy
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 1/17/2018
ms.topic: quickstart
ms.service: azure-policy
ms.custom: mvc
ms.openlocfilehash: 67c779b96dab088d810d22ad3053ade106aec56a
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/18/2018
---
# <a name="create-a-policy-assignment-to-identify-non-compliant-resources-in-your-azure-environment-using-powershell"></a>Tworzenie przypisania zasad w celu zidentyfikowania niezgodnych zasobów w środowisku Azure za pomocą programu PowerShell

Pierwszym krokiem do zrozumienia pojęcia zgodności na platformie Azure jest określenie obecnej sytuacji dotyczącej Twoich zasobów. Ten przewodnik Szybki start przeprowadzi Cię przez proces tworzenia przypisania zasad w celu zidentyfikowania maszyn wirtualnych, które nie korzystają z dysków zarządzanych.

Po zakończeniu tego procesu pomyślnie zidentyfikujesz maszyny wirtualne, które nie korzystają z dysków zarządzanych. Są one *niezgodne* z przypisaniem zasad.

Program PowerShell umożliwia tworzenie zasobów Azure i zarządzanie nimi z poziomu wiersza polecenia lub skryptów. W tym przewodniku szczegółowo opisano tworzenie przypisania zasad za pomocą programu PowerShell w celu zidentyfikowania niezgodnych zasobów w środowisku Azure.

Dla tego przewodnika jest wymagany moduł Azure PowerShell w wersji 4.0 lub nowszej. Uruchom polecenie  `Get-Module -ListAvailable AzureRM` , aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie modułu Azure PowerShell](/powershell/azure/install-azurerm-ps).

Przed rozpoczęciem upewnij się, że masz zainstalowaną najnowszą wersję programu PowerShell. Aby uzyskać szczegółowe informacje, zobacz artykuł [Instalowanie i konfigurowanie programu Azure PowerShell](/powershell/azureps-cmdlets-docs).

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne](https://azure.microsoft.com/free/) konto.


## <a name="create-a-policy-assignment"></a>Tworzenie przypisania zasad

W tym przewodniku Szybki start utworzysz przypisanie zasad i przypiszesz definicję *Audit Virtual Machines without Managed Disks* (Przeprowadź inspekcję maszyn wirtualnych bez dysków zarządzanych). Ta definicja zasad identyfikuje zasoby, które nie spełniają warunków określonych w definicji zasad.

Wykonaj poniższe kroki, aby utworzyć nowe przypisanie zasad.

1. Aby się upewnić, że Twoja subskrypcja współpracuje z dostawcą zasobów, zarejestruj dostawcę zasobów szczegółowych informacji o zasadach. Aby zarejestrować dostawcę zasobów, musisz mieć uprawnienia do wykonywania operacji rejestrowania dostawcy zasobów. Ta operacja jest uwzględniona w rolach Współautor i Właściciel.

    Uruchom następujące polecenie, aby zarejestrować dostawcę zasobów:

    ```
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.PolicyInsights
```

    Nie można wyrejestrować dostawcy zasobów, jeśli w ramach swojej subskrypcji masz typy zasobów pochodzące od dostawcy zasobów.

    Aby uzyskać więcej szczegółów na temat rejestrowania i przeglądania dostawców zasobów, zobacz [Dostawcy zasobów i ich typy](../azure-resource-manager/resource-manager-supported-services.md).

2. Po zarejestrowaniu dostawcy zasobów uruchom następujące polecenie, aby wyświetlić wszystkie definicje zasad i znaleźć tę, którą chcesz przypisać:

    ```powershell
$definition = Get-AzureRmPolicyDefinition
```

    Usługa Azure Policy zawiera już wbudowane definicje zasad, których możesz używać. Widoczne są m.in. wbudowane definicje zasad:

    - Enforce tag and its value (Wymuś tag i jego wartość)
    - Apply tag and its value (Zastosuj tag i jego wartość)
    - Require SQL Server Version 12.0 (Wymagaj programu SQL Server w wersji 12.0)

3. Następnie przypisz definicję zasad do żądanego zakresu, używając polecenia cmdlet `New-AzureRmPolicyAssignment`.

W tym samouczku użyj następujących informacji dla polecenia:

- **Nazwa** wyświetlana przypisania zasad. W tym przypadku użyj nazwy Audit Virtual Machines without Managed Disks (Przeprowadź inspekcję maszyn wirtualnych bez dysków zarządzanych).
- **Zasady** — definicja zasad, na podstawie której tworzysz przypisanie. W tym przypadku jest to definicja zasad *Audit Virtual Machines without Managed Disks* (Przeprowadź inspekcję maszyn wirtualnych bez dysków zarządzanych)
- **Zakres** — zakres określa, jakie zasoby lub grupy zasobów są wymuszane w ramach przypisania zasad. Może obejmować zarówno subskrypcje, jak i grupy zasobów. W tym przykładzie przypiszesz definicję zasad do grupy zasobów **FabrikamOMS**.
- **$definition** — musisz podać identyfikator zasobu definicji zasad. W tym przypadku używasz identyfikatora definicji zasad *Audit Virtual Machines without Managed Disks* (Przeprowadź inspekcję maszyn wirtualnych bez dysków zarządzanych).

```powershell
$rg = Get-AzureRmResourceGroup -Name "FabrikamOMS"
$definition = Get-AzureRmPolicyDefinition -Id /providers/Microsoft.Authorization/policyDefinitions/e5662a6-4747-49cd-b67b-bf8b01975c4c
New-AzureRMPolicyAssignment -Name Audit Virtual Machines without Managed Disks Assignment -Scope $rg.ResourceId -PolicyDefinition $definition
```

Teraz możesz zidentyfikować niezgodne zasoby, aby poznać stan zgodności Twojego środowiska.

## <a name="identify-non-compliant-resources"></a>Identyfikowanie niezgodnych zasobów

1. Przejdź z powrotem do strony docelowej usługi Azure Policy.
2. Wybierz pozycję **Zgodność** w okienku po lewej stronie i wyszukaj utworzone **przypisanie zasad**.

   ![Zgodność zasad](media/assign-policy-definition/policy-compliance.png)

   Jeśli istnieją jakiekolwiek zasoby niezgodne z nowym przypisaniem, zostaną one wyświetlone na karcie **Niezgodne zasoby**.

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Inne przewodniki w tej kolekcji bazują na tym przewodniku Szybki start. Jeśli planujesz kontynuować pracę z kolejnymi samouczkami, nie usuwaj zasobów utworzonych w tym przewodniku Szybki start. Jeśli nie zamierzasz kontynuować, usuń utworzone przypisanie, uruchamiając to polecenie:

```powershell
Remove-AzureRmPolicyAssignment -Name “Audit Virtual Machines without Managed Disks Assignment” -Scope /subscriptions/ bc75htn-a0fhsi-349b-56gh-4fghti-f84852/resourceGroups/FabrikamOMS
```

## <a name="next-steps"></a>Następne kroki

W tym przewodniku Szybki start została przypisana definicja zasad mających na celu zidentyfikowanie niezgodnych zasobów w środowisku platformy Azure.

Aby dowiedzieć się więcej na temat przypisywania zasad w celu zapewnienia zgodności zasobów tworzonych w **przyszłości**, przejdź do samouczka:

> [!div class="nextstepaction"]
> [Tworzenie zasad i zarządzanie nimi](./create-manage-policy.md)
