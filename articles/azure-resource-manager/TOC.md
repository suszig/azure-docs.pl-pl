# Omówienie
## [Co to jest usługa Resource Manager?](resource-group-overview.md)
## [Dostawcy zasobów i ich typy](resource-manager-supported-services.md)
## [Wdrażanie za pomocą usługi Resource Manager i klasyczne](resource-manager-deployment-model.md)
## [Nadzór subskrypcji](resource-manager-subscription-governance.md)
## [Aplikacje zarządzane](managed-application-overview.md)

# Rozpoczęcie pracy
## [Eksportowanie szablonu](resource-manager-export-template.md)
## [Tworzenie pierwszego szablonu](resource-manager-create-first-template.md)
## [Program Visual Studio z usługą Resource Manager](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md)

# Przykłady
## PowerShell
### [Wdrażanie szablonu](resource-manager-samples-powershell-deploy.md)

## Interfejs wiersza polecenia platformy Azure
### [Wdrażanie szablonu](resource-manager-samples-cli-deploy.md)

# Instrukcje
## Tworzenie szablonów
### [Najlepsze rozwiązania dotyczące szablonów](resource-manager-template-best-practices.md)
### [Sekcje szablonu](resource-group-authoring-templates.md)
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
### PowerShell
#### [Wdrażanie szablonu](resource-group-template-deploy.md)
#### [Wdrażanie szablonu prywatnego przy użyciu tokenu sygnatury dostępu współdzielonego](resource-manager-powershell-sas-token.md)
#### [Eksportowanie szablonu i ponowne wdrażanie](resource-manager-export-template-powershell.md)
### Interfejs wiersza polecenia platformy Azure
#### [Wdrażanie szablonu](resource-group-template-deploy-cli.md)
#### [Wdrażanie szablonu prywatnego przy użyciu tokenu sygnatury dostępu współdzielonego](resource-manager-cli-sas-token.md)
#### [Eksportowanie szablonu i ponowne wdrażanie](resource-manager-export-template-cli.md)
### [Portal](resource-group-template-deploy-portal.md)
### [Interfejs API REST](resource-group-template-deploy-rest.md)
### [Wdrażanie krzyżowe w grupach zasobów](resource-manager-cross-resource-group-deployment.md)
### [Ciągła integracja z usługą Visual Studio Team Services](../vs-azure-tools-resource-groups-ci-in-vsts.md?toc=%2fazure%2fazure-resource-manager%2ftoc.json)
### [Przekazywanie bezpiecznych wartości podczas wdrażania](resource-manager-keyvault-parameter.md)

## Zarządzanie
### [Program PowerShell](powershell-azure-resource-manager.md)
### [Interfejs wiersza polecenia platformy Azure](xplat-cli-azure-resource-manager.md)
### [Portal](resource-group-portal.md)
### [Interfejs API REST](resource-manager-rest-api.md)
### [Organizowanie zasobów przy użyciu tagów](resource-group-using-tags.md)
### [Przenoszenie zasobów do nowej grupy lub subskrypcji](resource-group-move-resources.md)
### [Przykłady nadzoru](resource-manager-subscription-examples.md)

## Kontrola dostępu
### Tworzenie jednostki usługi
#### [PowerShell](resource-group-authenticate-service-principal.md)
#### [Interfejs wiersza polecenia platformy Azure 2.0](/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json)
#### [Interfejs wiersza polecenia platformy Azure 1.0](resource-group-authenticate-service-principal-cli.md)
#### [Portal](resource-group-create-service-principal-portal.md)
### [Interfejs API uwierzytelniania na potrzeby dostępu do subskrypcji](resource-manager-api-authentication.md)
### [Blokowanie zasobów](resource-group-lock-resources.md)

## Ustawianie zasad zasobów
### [Co to są zasady zasobów?](resource-manager-policy.md)
### [Przypisywanie zasad za pomocą portalu](resource-manager-policy-portal.md)
### [Przypisywanie zasad za pomocą skryptów](resource-manager-policy-create-assign.md)
### Przykłady
#### [Tagi](resource-manager-policy-tags.md)
#### [Konwencje nazewnictwa](resource-manager-policy-naming-convention.md)
#### [Sieć](resource-manager-policy-network.md)
#### [Storage](resource-manager-policy-storage.md)
#### [Maszyna wirtualna z systemem Linux](../virtual-machines/linux/policy.md?toc=%2fazure%2fazure-resource-manager%2ftoc.json)
#### [Maszyna wirtualna z systemem Windows](../virtual-machines/windows/policy.md?toc=%2fazure%2fazure-resource-manager%2ftoc.json)

## Korzystanie z aplikacji zarządzanych
### [Publish service catalog application (Publikowanie aplikacji w katalogu usług)](managed-application-publishing.md)
### [Consume service catalog application (Używanie aplikacji w katalogu usług)](managed-application-consumption.md)
### [Publish marketplace application (Publikowanie aplikacji w witrynie Marketplace)](managed-application-author-marketplace.md)
### [Consume marketplace application (Używanie aplikacji w witrynie Marketplace)](managed-application-consume-marketplace.md)
### [Tworzenie definicji interfejsu użytkownika](managed-application-createuidefinition-overview.md)

## Inspekcja
### [Wyświetlanie dzienników aktywności](resource-group-audit.md)
### [Wyświetlanie operacji wdrażania](resource-manager-deployment-operations.md)

## Rozwiązywanie problemów
### [Typowe błędy związane z wdrażaniem](resource-manager-common-deployment-errors.md)
### [Understand deployment errors (Informacje o błędach wdrażania)](resource-manager-troubleshoot-tips.md)
### [RequestDisallowedByPolicy error (Błąd RequestDisallowedByPolicy)](resource-manager-policy-requestdisallowedbypolicy-error.md)
### Błędy wdrożenia maszyny wirtualnej
#### [Linux](../virtual-machines/linux/troubleshoot-deploy-vm.md)
#### [Windows](../virtual-machines/windows/troubleshoot-deploy-vm.md)

# Dokumentacja
## [Format szablonu](/azure/templates/)
## [Funkcje szablonów](resource-group-template-functions.md)
### [Funkcje tablicy i obiektów](resource-group-template-functions-array.md)
### [Funkcje porównania](resource-group-template-functions-comparison.md)
### [Funkcje wdrażania](resource-group-template-functions-deployment.md)
### [Funkcje liczbowe](resource-group-template-functions-numeric.md)
### [Funkcje zasobów](resource-group-template-functions-resource.md)
### [Funkcje ciągów](resource-group-template-functions-string.md)
## [Funkcje definicji interfejsu użytkownika](managed-application-createuidefinition-functions.md)
## [Elementy definicji interfejsu użytkownika](managed-application-createuidefinition-elements.md)
### [Microsoft.Common.DropDown](managed-application-microsoft-common-dropdown.md)
### [Microsoft.Common.FileUpload](managed-application-microsoft-common-fileupload.md)
### [Microsoft.Common.OptionsGroup](managed-application-microsoft-common-optionsgroup.md)
### [Microsoft.Common.PasswordBox](managed-application-microsoft-common-passwordbox.md)
### [Microsoft.Common.Section](managed-application-microsoft-common-section.md)
### [Microsoft.Common.TextBox](managed-application-microsoft-common-textbox.md)
### [Microsoft.Compute.CredentialsCombo](managed-application-microsoft-compute-credentialscombo.md)
### [Microsoft.Compute.SizeSelector](managed-application-microsoft-compute-sizeselector.md)
### [Microsoft.Compute.UserNameTextBox](managed-application-microsoft-compute-usernametextbox.md)
### [Microsoft.Network.PublicIpAddressCombo](managed-application-microsoft-network-publicipaddresscombo.md)
### [Microsoft.Network.VirtualNetworkCombo](managed-application-microsoft-network-virtualnetworkcombo.md)
### [Microsoft.Storage.MultiStorageAccountCombo](managed-application-microsoft-storage-multistorageaccountcombo.md)
### [Microsoft.Storage.StorageAccountSelector](managed-application-microsoft-storage-storageaccountselector.md)
## [PowerShell](/powershell/module/azurerm.resources)
## [Interfejs wiersza polecenia platformy Azure](/cli/azure/resource)
## [.NET](/dotnet/api/microsoft.azure.management.resourcemanager)
## [Java](/java/api/com.microsoft.azure.management.resources)
## [Python](http://azure-sdk-for-python.readthedocs.io/en/latest/resourcemanagement.html)
## [REST](/rest/api/resources/)

# Zasoby
## [Harmonogram działania dla platformy Azure](https://azure.microsoft.com/roadmap/)
## [Aktualizacje usług](https://azure.microsoft.com/updates/?product=azure-resource-manager)
## [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-resource-manager)
## [Ograniczanie żądań](resource-manager-request-limits.md)
## [Śledzenie operacji asynchronicznych](resource-manager-async-operations.md)
## [Filmy wideo](https://azure.microsoft.com/documentation/videos/index/?services=azure-resource-manager)
