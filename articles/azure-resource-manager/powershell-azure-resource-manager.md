---
title: "Zarządzaj rozwiązaniami Azure przy użyciu programu PowerShell | Dokumentacja firmy Microsoft"
description: "Użyj programu Azure PowerShell i Menedżera zasobów do zarządzania zasobami."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: b33b7303-3330-4af8-8329-c80ac7e9bc7f
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: powershell
ms.devlang: na
ms.topic: article
ms.date: 10/06/2017
ms.author: tomfitz
ms.openlocfilehash: ae5ccb83a0088cb7c9668f18620b74f9f3f1e9b0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="manage-resources-with-azure-powershell-and-resource-manager"></a>Zarządzanie zasobami za pomocą programu Azure PowerShell i Menedżera zasobów

W tym artykule Dowiedz się jak zarządzać rozwiązań z programu Azure PowerShell i Menedżera zasobów Azure. Jeśli nie masz doświadczenia z usługą Resource Manager, zobacz [omówienie Resource Manager](resource-group-overview.md). Ten artykuł dotyczy zadań zarządzania. Wykonasz następujące zadania:

1. Tworzenie grupy zasobów
2. Dodaj zasób do grupy zasobów
3. Dodaj tag do zasobu
4. Wykonywanie zapytań względem zasobów na podstawie nazw lub wartości tagów
5. Zastosuj i Usuń blokada na zasób
6. Usuń grupę zasobów

W tym artykule nie pokazuje sposób wdrażania szablonu usługi Resource Manager do subskrypcji. Informacje, zobacz [wdrażanie zasobów przy użyciu szablonów usługi Resource Manager i programu Azure PowerShell](resource-group-template-deploy.md).

## <a name="get-started-with-azure-powershell"></a>Wprowadzenie do programu Azure PowerShell

Jeśli nie zainstalowano programu Azure PowerShell, zobacz [jak instalowanie i konfigurowanie programu Azure PowerShell](/powershell/azure/overview).

Jeśli mieć zainstalowany program Azure PowerShell w przeszłości, ale nie zaktualizowano on niedawno, należy rozważyć zainstalowanie najnowszej wersji. Należy zaktualizować wersję za pomocą tej samej metody, które zostały użyte do zainstalowania go. Na przykład użycie Instalatora platformy sieci Web, uruchom ją ponownie i wyszukać aktualizację.

Aby sprawdzić swoją wersję modułu zasobów Azure, użyj następującego polecenia cmdlet:

```powershell
Get-Module -ListAvailable -Name AzureRm.Resources | Select Version
```

Ten artykuł został zaktualizowany na potrzeby wersji 3.3.0. Jeśli masz starszą wersję środowiska mogą nie odpowiadać kroki opisane w tym artykule. Aby uzyskać dokumentację poleceń cmdlet w tej wersji, zobacz [AzureRM.Resources modułu](/powershell/module/azurerm.resources).

## <a name="log-in-to-your-azure-account"></a>Zaloguj się do konta platformy Azure
Przed rozpoczęciem pracy rozwiązania, należy zalogować się do swojego konta.

Aby zalogować się do konta platformy Azure, użyj **Login-AzureRmAccount** polecenia cmdlet.

```powershell
Login-AzureRmAccount
```

Polecenie cmdlet wyświetla monit o podanie poświadczeń logowania dla konta platformy Azure. Po zalogowaniu pobiera ono ustawienia konta, aby były dostępne dla programu Azure PowerShell.

Polecenie cmdlet zwraca informacje dotyczące konta i subskrypcji do użycia dla zadania.

```powershell
Environment           : AzureCloud
Account               : example@contoso.com
TenantId              : {guid}
SubscriptionId        : {guid}
SubscriptionName      : Example Subscription One
CurrentStorageAccount :

```

Jeśli masz więcej niż jedną subskrypcję, możesz przełączyć się do innej subskrypcji. Po pierwsze Zobacz wszystkie subskrypcje dla swojego konta.

```powershell
Get-AzureRmSubscription
```

Zwraca włączyć i wyłączyć subskrypcje.

```powershell
SubscriptionName : Example Subscription One
SubscriptionId   : {guid}
TenantId         : {guid}
State            : Enabled

SubscriptionName : Example Subscription Two
SubscriptionId   : {guid}
TenantId         : {guid}
State            : Enabled

SubscriptionName : Example Subscription Three
SubscriptionId   : {guid}
TenantId         : {guid}
State            : Disabled
```

Aby przełączyć się do innej subskrypcji, należy podać nazwę subskrypcji z **Set-AzureRmContext** polecenia cmdlet.

```powershell
Set-AzureRmContext -SubscriptionName "Example Subscription Two"
```

## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

Przed wdrożeniem żadnych zasobów w Twojej subskrypcji, należy utworzyć grupę zasobów, która będzie zawierać zasoby.

Aby utworzyć grupę zasobów, użyj **New-AzureRmResourceGroup** polecenia cmdlet. Używa polecenia **nazwa** parametr, aby określić nazwę grupy zasobów i **lokalizacji** parametru w celu określenia lokalizacji.

```powershell
New-AzureRmResourceGroup -Name TestRG1 -Location "South Central US"
```

Dane wyjściowe znajduje się w następującym formacie:

```powershell
ResourceGroupName : TestRG1
Location          : southcentralus
ProvisioningState : Succeeded
Tags              :
ResourceId        : /subscriptions/{guid}/resourceGroups/TestRG1
```

Aby pobrać grupę zasobów później, należy użyć następującego polecenia cmdlet:

```powershell
Get-AzureRmResourceGroup -ResourceGroupName TestRG1
```

Aby uzyskać wszystkie grupy zasobów w ramach subskrypcji, nie należy określać nazwy:

```powershell
Get-AzureRmResourceGroup
```

## <a name="add-resources-to-a-resource-group"></a>Dodaj zasoby do grupy zasobów

Aby dodać zasobu do grupy zasobów, można użyć **AzureRmResource nowy** polecenia cmdlet lub polecenia cmdlet, które są specyficzne dla typu zasobu tworzysz (takich jak **New-AzureRmStorageAccount**). Użytkownik może łatwiej, użyj polecenia cmdlet, które są specyficzne dla typu zasobu, ponieważ uwzględnia on również parametry dla właściwości, które są potrzebne dla nowego zasobu. Aby użyć **AzureRmResource nowy**, musisz znać wszystkie właściwości można ustawić bez konieczności podawania dla nich.

Jednak dodawanie zasobów za pomocą poleceń cmdlet może spowodować przyszłych pomyłek, ponieważ nowy zasób nie istnieje w szablonie usługi Resource Manager. Firma Microsoft zaleca Definiowanie infrastruktury dla rozwiązania Azure w ramach szablonu usługi Resource Manager. Szablony umożliwiają niezawodne i wielokrotnego wdrażania rozwiązania. W tym artykule Utwórz konto magazynu za pomocą polecenia cmdlet programu PowerShell, ale później wygenerowanie szablonu z grupy zasobów.

Następujące polecenie cmdlet tworzy konto magazynu. Zamiast nazwy pokazano w przykładzie, Podaj unikatową nazwę konta magazynu. Nazwa musi mieć od 3 do 24 znaków i zawierać tylko cyfry i małe litery. Jeśli używasz nazwę pokazano w przykładzie, wystąpi błąd, ponieważ ta nazwa jest już używana.

```powershell
New-AzureRmStorageAccount -ResourceGroupName TestRG1 -AccountName mystoragename -Type "Standard_LRS" -Location "South Central US"
```

Jeśli potrzebujesz do odtworzenia tego zasobu, należy użyć następującego polecenia cmdlet:

```powershell
Get-AzureRmResource -ResourceName mystoragename -ResourceGroupName TestRG1
```

## <a name="add-a-tag"></a>Dodaj tag

Znaczniki umożliwiają organizowania zasobów zgodnie z innej właściwości. Na przykład może mieć kilka zasobów w różnych grup zasobów należących do tego samego działu. Tag działu i wartości można zastosować do tych zasobów, aby oznaczyć je jako należące do tej samej kategorii. Alternatywnie można oznaczyć, czy zasób jest używany w środowisku produkcyjnym lub testowym. W tym artykule tagi są stosowane tylko do jednego zasobu, ale w danym środowisku prawdopodobnie warto dodawania tagów do wszystkich zasobów.

Następujące polecenie cmdlet ma zastosowanie dwa tagi do konta magazynu:

```powershell
Set-AzureRmResource -Tag @{ Dept="IT"; Environment="Test" } -ResourceName mystoragename -ResourceGroupName TestRG1 -ResourceType Microsoft.Storage/storageAccounts
 ```

Tagi są aktualizowane jako pojedynczego obiektu. Aby dodać tag do zasobu, która zawiera już znaczników, należy najpierw pobrać istniejące znaczniki. Dodaj nowy znacznik do obiektu, który zawiera istniejące znaczniki i ponownie zastosuj wszystkie tagi do zasobu.

```powershell
$tags = (Get-AzureRmResource -ResourceName mystoragename -ResourceGroupName TestRG1).Tags
$tags += @{Status="Approved"}
Set-AzureRmResource -Tag $tags -ResourceName mystoragename -ResourceGroupName TestRG1 -ResourceType Microsoft.Storage/storageAccounts
```

## <a name="search-for-resources"></a>Wyszukaj zasoby

Użyj **AzureRmResource Znajdź** polecenia cmdlet można pobrać zasobów do wyszukiwania różnych warunków.

* Aby uzyskać za pomocą nazwy zasobu, należy podać **ResourceNameContains** parametru:

  ```powershell
  Find-AzureRmResource -ResourceNameContains mystoragename
  ```

* Aby uzyskać wszystkie zasoby w grupie zasobów, należy podać **ResourceGroupNameContains** parametru:

  ```powershell
  Find-AzureRmResource -ResourceGroupNameContains TestRG1
  ```

* Aby pobrać wszystkich zasobów o nazwie tagu i wartości, należy podać **TagName** i **TagValue** parametry:

  ```powershell
  Find-AzureRmResource -TagName Dept -TagValue IT
  ```

* Do wszystkich zasobów z określonego typu zasobu, należy podać **ResourceType** parametru:

  ```powershell
  Find-AzureRmResource -ResourceType Microsoft.Storage/storageAccounts
  ```

## <a name="get-resource-id"></a>Pobierz identyfikator zasobu

Wiele poleceń zająć identyfikator zasobu jako parametr. Aby uzyskać identyfikator zasobu i przechowywać w zmiennej, należy użyć:

```powershell
$webappID = (Get-AzureRmResource -ResourceGroupName exampleGroup -ResourceName exampleSite).ResourceId
```

## <a name="lock-a-resource"></a>Zablokowanie zasobu

Aby upewnić się, że krytyczne zasobów nie zostanie przypadkowo usunięty lub zmodyfikowany, zastosuj blokady do zasobu. Można określić **CanNotDelete** lub **tylko do odczytu**.

Aby utworzyć lub usunąć blokady zarządzania, musi mieć dostęp do `Microsoft.Authorization/*` lub `Microsoft.Authorization/locks/*` akcje. Wbudowanych ról tylko właściciel i Administrator dostępu użytkowników są przyznawane te akcje.

Aby zastosować blokady, należy użyć następującego polecenia cmdlet:

```powershell
New-AzureRmResourceLock -LockLevel CanNotDelete -LockName LockStorage -ResourceName mystoragename -ResourceType Microsoft.Storage/storageAccounts -ResourceGroupName TestRG1
```

Nie można usunąć zablokowanych zasobu w poprzednim przykładzie, aż do usunięcia blokady. Aby usunąć blokadę, należy użyć:

```powershell
Remove-AzureRmResourceLock -LockName LockStorage -ResourceName mystoragename -ResourceType Microsoft.Storage/storageAccounts -ResourceGroupName TestRG1
```

Aby uzyskać więcej informacji na temat ustawienie blokady, zobacz [blokowania zasobów z usługi Azure Resource Manager](resource-group-lock-resources.md).

## <a name="remove-resources-or-resource-group"></a>Usuwanie zasobów lub grupy zasobów
Można usunąć zasób lub grupa zasobów. Usuń grupę zasobów, należy również usunąć wszystkie zasoby w grupie zasobów.

* Aby usunąć zasób z grupy zasobów, użyj **AzureRmResource Usuń** polecenia cmdlet. To polecenie cmdlet Usuwa zasób, ale nie powoduje usunięcia grupy zasobów.

  ```powershell
  Remove-AzureRmResource -ResourceName mystoragename -ResourceType Microsoft.Storage/storageAccounts -ResourceGroupName TestRG1
  ```

* Aby usunąć grupę zasobów i wszystkie jego zasoby, używać **Remove-AzureRmResourceGroup** polecenia cmdlet.

  ```powershell
  Remove-AzureRmResourceGroup -Name TestRG1
  ```

Dla obu poleceń cmdlet zostanie wyświetlona prośba o potwierdzenie, że chcesz usunąć zasób lub grupa zasobów. Jeśli operacja pomyślnie Usuwa zasób lub grupa zasobów, zwraca **True**.

## <a name="run-resource-manager-scripts-with-azure-automation"></a>Uruchamianie skryptów Menedżera zasobów w usłudze Automatyzacja Azure

W tym artykule przedstawiono sposób wykonywania podstawowych operacji dotyczących zasobów przy użyciu programu Azure PowerShell. W przypadku bardziej zaawansowanych scenariuszy zarządzania zwykle chcesz utworzyć skrypt, a następnie ponownie użyć tego skryptu, zgodnie z potrzebami lub zgodnie z harmonogramem. [Automatyzacja Azure](../automation/automation-intro.md) zapewnia sposobem zautomatyzowania często używanych skryptów, które zarządzają rozwiązań platformy Azure.

W następujących tematach opisano, jak używać usługi Automatyzacja Azure Resource Manager i programu PowerShell do skutecznie wykonywać zadania zarządzania:

- Aby uzyskać informacje dotyczące tworzenia elementu runbook, zobacz [Moje pierwszego elementu runbook programu PowerShell](../automation/automation-first-runbook-textual-powershell.md).
- Aby uzyskać informacje na temat pracy z galerii skryptów, zobacz [galerie elementów Runbook i modułów w automatyzacji Azure](../automation/automation-runbook-gallery.md).
- Dla elementów runbook uruchamianie i zatrzymywanie maszyn wirtualnych, zobacz [scenariusz automatyzacji Azure: formacie JSON przy użyciu tagów, aby utworzyć harmonogram uruchamiania maszyny Wirtualnej platformy Azure i zamykania](../automation/automation-scenario-start-stop-vm-wjson-tags.md).
- Dla elementów runbook uruchamianie i zatrzymywanie maszyn wirtualnych po godzinach pracy, zobacz [uruchamiania/zatrzymywania maszyn wirtualnych, podczas rozwiązania poza godzinami szczytu w automatyzacji](../automation/automation-solution-vm-management.md).

## <a name="next-steps"></a>Następne kroki
* Aby uzyskać informacje dotyczące tworzenia szablonów usługi Resource Manager, zobacz [Authoring Azure Resource Manager szablony](resource-group-authoring-templates.md).
* Informacje na temat wdrażania szablonów, zobacz [wdrażanie aplikacji przy użyciu szablonu usługi Resource Manager Azure](resource-group-template-deploy.md).
* Można przenieść istniejące zasoby do nowej grupy zasobów. Aby uzyskać przykłady, zobacz [przeniesienia zasobów do nowej grupy zasobów lub subskrypcji](resource-group-move-resources.md).
* Aby uzyskać instrukcje dla przedsiębiorstw dotyczące użycia usługi Resource Manager w celu efektywnego zarządzania subskrypcjami, zobacz [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md) (Szkielet platformy Azure dla przedsiębiorstwa — narzucony nadzór subskrypcji).

