# Omówienie
## [Co to jest usługa Resource Manager?](resource-group-overview.md)
## [Dostawcy zasobów i ich typy](resource-manager-supported-services.md)
## [Wdrażanie za pomocą usługi Resource Manager i klasyczne](resource-manager-deployment-model.md)
## [Nadzór subskrypcji](resource-manager-subscription-governance.md)

# Rozpoczynanie pracy
## [Szablon tworzenia i wdrażania](resource-manager-create-first-template.md)
## [Rozszerzenia kodu VS dla szablonów](resource-manager-vscode-extension.md)
## [Program Visual Studio z usługą Resource Manager](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md)

# Przykłady
## [Przykłady kodu](https://azure.microsoft.com/en-us/resources/samples/?service=azure-resource-manager)
## Azure PowerShell
### [Wdrażanie szablonu](resource-manager-samples-powershell-deploy.md)

## Interfejs wiersza polecenia platformy Azure
### [Wdrażanie szablonu](resource-manager-samples-cli-deploy.md)

# Instrukcje
## Tworzenie szablonów
### [Sekcje szablonu](resource-group-authoring-templates.md)
### [Najlepsze rozwiązania dotyczące szablonów](resource-manager-template-best-practices.md)
### [Łączenie z innymi szablonami](resource-group-linked-templates.md)
### [Definiowanie zależności między zasobami](resource-group-define-dependencies.md)
### [Tworzenie wielu wystąpień](resource-group-create-multiple.md)
### [Ustawianie lokalizacji](resource-manager-template-location.md)
### [Przypisywanie tagów](resource-manager-template-tags.md)
### [Ustawianie nazwy i typu zasobu podrzędnego](resource-manager-template-child-resource.md)
### [Aktualizowanie zasobu](resource-manager-update.md)
### [Używanie obiektów dla parametrów](resource-manager-objects-as-parameters.md)
### [Udostępnianie stanu między połączonymi szablonami](best-practices-resource-manager-state.md)
### [Wzorce projektowania szablonów](best-practices-resource-manager-design-templates.md)


## Wdrażanie
### Azure PowerShell
#### [Wdrażanie szablonu](resource-group-template-deploy.md)
#### [Wdrażanie szablonu prywatnego przy użyciu tokenu sygnatury dostępu współdzielonego](resource-manager-powershell-sas-token.md)
#### [Eksportowanie szablonu i ponowne wdrażanie](resource-manager-export-template-powershell.md)
### Interfejs wiersza polecenia platformy Azure
#### [Wdrażanie szablonu](resource-group-template-deploy-cli.md)
#### [Wdrażanie szablonu prywatnego przy użyciu tokenu sygnatury dostępu współdzielonego](resource-manager-cli-sas-token.md)
#### [Eksportowanie szablonu i ponowne wdrażanie](resource-manager-export-template-cli.md)
### Azure Portal
#### [Wdrażanie zasobów](resource-group-template-deploy-portal.md)
#### [Eksportowanie szablonu](resource-manager-export-template.md)
### [Interfejs API REST](resource-group-template-deploy-rest.md)
### [Wdrażanie krzyżowe w grupach zasobów](resource-manager-cross-resource-group-deployment.md)
### [Ciągła integracja z usługą Visual Studio Team Services](../vs-azure-tools-resource-groups-ci-in-vsts.md?toc=%2fazure%2fazure-resource-manager%2ftoc.json)
### [Przekazywanie bezpiecznych wartości podczas wdrażania](resource-manager-keyvault-parameter.md)

## Zarządzanie
### [Azure PowerShell](powershell-azure-resource-manager.md)
### [Interfejs wiersza polecenia platformy Azure](xplat-cli-azure-resource-manager.md)
### [Witryna Azure Portal](resource-group-portal.md)
### [Interfejs API REST](resource-manager-rest-api.md)
### [Organizowanie zasobów przy użyciu tagów](resource-group-using-tags.md)
### [Przenoszenie zasobów do nowej grupy lub subskrypcji](resource-group-move-resources.md)
### [Organizowanie subskrypcji przy użyciu grup zarządzania](../billing/billing-enterprise-mgmt-group-overview.md?toc=%2fazure%2fazure-resource-manager%2ftoc.json)
### [Przykłady nadzoru](resource-manager-subscription-examples.md)
### [Aplikacje zarządzane](../managed-applications/overview.md)

## Kontrola dostępu
### Tworzenie jednostki usługi
#### [Azure PowerShell](resource-group-authenticate-service-principal.md)
#### [Interfejs wiersza polecenia platformy Azure](/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json)
#### [Witryna Azure Portal](resource-group-create-service-principal-portal.md)
### [Interfejs API uwierzytelniania na potrzeby dostępu do subskrypcji](resource-manager-api-authentication.md)
### [Blokowanie zasobów](resource-group-lock-resources.md)

## Ustawianie zasad zasobów
### [Co to są zasady zasobów?](resource-manager-policy.md)
### [Konwencje nazewnictwa](resource-manager-policy-naming-convention.md)

## Inspekcja
### [Wyświetlanie dzienników aktywności](resource-group-audit.md)
### [Wyświetlanie operacji wdrażania](resource-manager-deployment-operations.md)

## Rozwiązywanie problemów
### [Typowe błędy związane z wdrażaniem](resource-manager-common-deployment-errors.md)
### [Understand deployment errors (Informacje o błędach wdrażania)](resource-manager-troubleshoot-tips.md)
### Usuwanie błędów
#### [AccountNameInvalid](resource-manager-storage-account-name-errors.md)
#### [InvalidTemplate](resource-manager-invalid-template-errors.md)
#### [NoRegisteredProviderFound](resource-manager-register-provider-errors.md)
#### [NotFound](resource-manager-not-found-errors.md)
#### [ParentResourceNotFound](resource-manager-parent-resource-errors.md)
#### [RequestDisallowedByPolicy](resource-manager-policy-requestdisallowedbypolicy-error.md)
#### [ReservedResourceName](resource-manager-reserved-resource-name.md)
#### [ResourceQuotaExceeded](resource-manager-quota-errors.md)
#### [SkuNotAvailable](resource-manager-sku-not-available-errors.md)
### Błędy wdrożenia maszyny wirtualnej
#### Linux
##### [Problemy z wdrażaniem](../virtual-machines/linux/troubleshoot-deploy-vm.md)
##### [Problemy z aprowizowaniem i alokacją](../virtual-machines/linux/troubleshoot-deployment-new-vm.md)
##### [Typowe komunikaty o błędach](../virtual-machines/linux/error-messages.md)
#### Windows
##### [Problemy z wdrażaniem](../virtual-machines/windows/troubleshoot-deploy-vm.md)
##### [Problemy z aprowizowaniem i alokacją](../virtual-machines/windows/troubleshoot-deployment-new-vm.md)
##### [Typowe komunikaty o błędach](../virtual-machines/windows/error-messages.md)

# Dokumentacja
## [Format szablonu](/azure/templates/)
## [Funkcje szablonów](resource-group-template-functions.md)
### [Funkcje tablicy i obiektów](resource-group-template-functions-array.md)
### [Funkcje porównania](resource-group-template-functions-comparison.md)
### [Funkcje wdrażania](resource-group-template-functions-deployment.md)
### [Funkcje logiczne](resource-group-template-functions-logical.md)
### [Funkcje liczbowe](resource-group-template-functions-numeric.md)
### [Funkcje zasobów](resource-group-template-functions-resource.md)
### [Funkcje ciągów](resource-group-template-functions-string.md)
## [PowerShell](/powershell/module/azurerm.resources)
## [Interfejs wiersza polecenia platformy Azure](/cli/azure/resource)
## [.NET](/dotnet/api/microsoft.azure.management.resourcemanager)
## [Java](/java/api/com.microsoft.azure.management.resources)
## [Python](http://azure-sdk-for-python.readthedocs.io/en/latest/resourcemanagement.html)
## [REST](/rest/api/resources/)

# Zasoby
## [Harmonogram działania dla platformy Azure](https://azure.microsoft.com/roadmap/?category=monitoring-management)
## [Kalkulator cen](https://azure.microsoft.com/pricing/calculator/)
## [Aktualizacje usług](https://azure.microsoft.com/updates/?product=azure-resource-manager)
## [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-resource-manager)
## [Ograniczanie żądań](resource-manager-request-limits.md)
## [Śledzenie operacji asynchronicznych](resource-manager-async-operations.md)
## [Filmy wideo](https://azure.microsoft.com/documentation/videos/index/?services=azure-resource-manager)
