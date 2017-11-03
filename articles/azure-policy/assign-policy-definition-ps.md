---
title: "Utwórz przypisanie zasad, aby zidentyfikować niezgodnych zasobów w środowisku platformy Azure przy użyciu programu PowerShell | Dokumentacja firmy Microsoft"
description: "Utwórz przypisanie zasad Azure do identyfikacji niezgodnych zasobów za pomocą programu PowerShell."
services: azure-policy
keywords: 
author: Jim-Parker
ms.author: jimpark
ms.date: 10/06/2017
ms.topic: quickstart
ms.service: azure-policy
ms.custom: mvc
ms.openlocfilehash: 3f9ef7886af20845eddc4c1e71d60911e4b22eca
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-policy-assignment-to-identify-non-compliant-resources-in-your-azure-environment-using-powershell"></a>Utwórz przypisanie zasad w celu identyfikacji niezgodnych zasobów w środowisku platformy Azure przy użyciu programu PowerShell

Pierwszym etapem opis zgodności w usłudze Azure jest znajomość, gdzie autonomiczna z zasobami bieżącej. Ta opcja szybkiego startu przeprowadza użytkownika przez proces tworzenia zasad przydziału do identyfikacji niezgodnych zasobów o definicji zasad — *wymagają programu SQL Server 12.0*. Po zakończeniu tego procesu pomyślnie zidentyfikowano jakie serwery są w innej wersji lub niezgodne.

Program PowerShell umożliwia tworzenie zasobów Azure i zarządzanie nimi z poziomu wiersza polecenia lub skryptów. Szczegóły ten przewodnik przy użyciu programu PowerShell, aby utworzyć przypisanie zasad, aby zidentyfikować niezgodnych zasobów w środowisku platformy Azure.

W tym przewodniku wymaga programu Azure PowerShell modułu w wersji 4.0 lub nowszej. Uruchom ```Get-Module -ListAvailable AzureRM``` można znaleźć wersji. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie modułu Azure PowerShell](/powershell/azure/install-azurerm-ps).

Przed rozpoczęciem upewnij się, że masz zainstalowaną najnowszą wersję programu PowerShell. Aby uzyskać szczegółowe informacje, zobacz artykuł [How to install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) (Instalowanie i konfigurowanie programu Azure PowerShell).

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne](https://azure.microsoft.com/free/) konto.

## <a name="opt-in-to-azure-policy"></a>Zgódź się na Azure zasad

Zasady usługi Azure jest teraz dostępna w ograniczony (wersja zapoznawcza), należy zarejestrować, aby zażądać dostępu.

1. Przejdź do zasad Azure https://aka.ms/getpolicy i wybierz **Utwórz konto** w okienku po lewej stronie.

   ![Wyszukaj zasady](media/assign-policy-definition/sign-up.png)

2. Zapoznać się zasadami Azure po wybraniu subskrypcji w **subskrypcji** chcesz pracować z listy. Następnie wybierz **zarejestrować**.

   ![Zgódź się na przy użyciu zasad usługi Azure](media/assign-policy-definition/preview-opt-in.png)

   Może upłynąć kilka dni firmie Microsoft zaakceptować żądanie rejestracji, w zależności od zapotrzebowania. Po zaakceptowaniu żądania pobiera dowiesz się za pośrednictwem poczty e-mail czy można rozpocząć korzystanie z usługi.

## <a name="create-a-policy-assignment"></a>Utwórz przypisanie zasad

W tego przewodnika Szybki Start, możemy utworzyć przypisania zasad i przypisać *wymagają programu SQL Server wersji 12.0* definicji. Ta definicja zasad identyfikuje zasoby, które nie spełniają warunków ustawionych w definicji zasad.

Wykonaj następujące kroki, aby utworzyć nowe przypisanie zasad.

Uruchom następujące polecenie, aby wyświetlić wszystkie definicje zasad i odnaleźć, który chcesz przypisać:

```powershell
$definition = Get-AzureRmPolicyDefinition
```

Zasady usługi Azure zawiera definicje zasad już wbudowanych, których można użyć. Definicje wbudowanych zasad będą widoczne takie jak:

- Wymuszanie tagu i jego wartość
- Stosuje tag i jego wartość
- Wymaga programu SQL Server w wersji 12.0

Następnie przydzielić definicji zasad przy użyciu żądany zakres `New-AzureRmPolicyAssignment` polecenia cmdlet.

W tym samouczku zapewniamy następujące informacje dotyczące polecenia:
- Wyświetl **nazwa** dla przypisania zasad. W takim przypadku Użyjmy wymagają programu SQL Server 12.0 przypisania.
- **Zasady** — jest to definicja zasad, na podstawie off, którego używasz utworzyć przypisanie. W takim przypadku jest definicji zasad — *wymagają programu SQL Server 12.0*
- A **zakres** — zakres Określa, jakie zasoby lub grupowanie zasobów przypisania zasad pobiera wymuszane na. Mogą obejmować z subskrypcji z grupami zasobów. W tym przykładzie mamy przypisywanie definicji zasad do **FabrikamOMS** grupy zasobów.
- **$definition** — należy podać identyfikator zasobu w definicji zasad — w takim przypadku używamy identyfikator definicji zasad - *wymagają programu SQL Server 12.0*.

```powershell
$rg = Get-AzureRmResourceGroup -Name "FabrikamOMS"
$definition = Get-AzureRmPolicyDefinition -Id /providers/Microsoft.Authorization/policyDefinitions/e5662a6-4747-49cd-b67b-bf8b01975c4c
New-AzureRMPolicyAssignment -Name Require SQL Server version 12.0 Assignment -Scope $rg.ResourceId -PolicyDefinition $definition
```

Teraz możesz zidentyfikować niezgodnych zasobów, aby zrozumieć stan zgodności danego środowiska.

## <a name="identify-non-compliant-resources"></a>Zidentyfikuj zasoby niezgodnych

1. Przejdź z powrotem do strony docelowej Azure zasad.
2. Wybierz **zgodności** w okienku po lewej stronie i poszukaj **przypisania zasad** utworzony.

   ![Zasady zgodności](media/assign-policy-definition/policy-compliance.png)

   W przypadku istniejących zasobów, które nie są zgodne z tym nowe przypisanie one będą widoczne w obszarze **niezgodne zasoby** karcie, jak pokazano powyżej.

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Przewodnikach w tej kolekcji zależą od tego przewodnika Szybki Start. Jeśli zamierzasz kontynuować pracę z kolejnych samouczkach nie wyczyścić zasoby utworzone w tym Szybki Start. Jeśli nie planujesz kontynuować, Usuń przypisanie utworzone przez uruchomienie tego polecenia:

```powershell
Remove-AzureRmPolicyAssignment -Name “Require SQL Server version 12.0 Assignment” -Scope /subscriptions/ bc75htn-a0fhsi-349b-56gh-4fghti-f84852/resourceGroups/FabrikamOMS
```

## <a name="next-steps"></a>Następne kroki

W tym szybki start można przypisać definicję zasad w celu identyfikacji niezgodnych zasobów w środowisku platformy Azure.

Aby dowiedzieć się więcej o przypisanie zasad, aby upewnić się, że **przyszłych** zasobów, które tworzone są zgodne, w dalszym ciągu samouczek dotyczący:

> [!div class="nextstepaction"]
> [Tworzenie i zarządzanie zasadami](./create-manage-policy.md)