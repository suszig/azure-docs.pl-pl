---
title: "Rozwiąż typowe błędy wdrożenia usługi Azure | Dokumentacja firmy Microsoft"
description: "Opisuje sposób rozwiązania typowych błędów, podczas wdrażania zasobów na platformie Azure przy użyciu usługi Azure Resource Manager."
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
ms.date: 12/20/2017
ms.author: tomfitz
ms.openlocfilehash: ca7e3cb541948e6cc0b8d077616f3611e3ab2477
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/20/2017
---
# <a name="troubleshoot-common-azure-deployment-errors-with-azure-resource-manager"></a>Rozwiąż typowe błędy wdrożenia usługi Azure z usługą Azure Resource Manager

W tym artykule opisano niektóre typowe błędy wdrożenia usługi Azure może wystąpić, a także informacje umożliwiające rozwiązanie błędów. Jeśli nie możesz znaleźć kod błędu dla błędu wdrożenia, zobacz [znaleźć kodu błędu](#find-error-code).

## <a name="error-codes"></a>Kody błędów

| Kod błędu | Środki zaradcze | Więcej informacji |
| ---------- | ---------- | ---------------- |
| AccountNameInvalid | Wykonaj ograniczenia nazewnictwa dla kont magazynu. | [Rozpoznanie nazwy konta magazynu](resource-manager-storage-account-name-errors.md) |
| AccountPropertyCannotBeSet | Sprawdź właściwości konta magazynu dostępne. | [storageAccounts](/azure/templates/microsoft.storage/storageaccounts) |
| AllocationFailed | Klaster lub regionie nie ma dostępu do zasobów lub nie może obsługiwać żądany rozmiar maszyny Wirtualnej. Ponów żądanie w późniejszym czasie, lub zwróć inny rozmiar maszyny Wirtualnej. | [Problemy z inicjowaniem obsługi administracyjnej i alokacji dla systemu Linux](../virtual-machines/linux/troubleshoot-deployment-new-vm.md) i [problemy z inicjowaniem obsługi administracyjnej i alokacji dla systemu Windows](../virtual-machines/windows/troubleshoot-deployment-new-vm.md) |
| AnotherOperationInProgress | Poczekaj na zakończenie operacji współbieżnych. | |
| AuthorizationFailed | Twoje konto lub nazwy głównej usługi nie ma wystarczające prawa dostępu do wdrażania. Sprawdź używane konto należy do roli, a jego dostępu dla zakresu wdrożenia. | [Kontrola dostępu oparta na rolach na platformie Azure](../active-directory/role-based-access-control-configure.md) |
| BadRequest | Wysłano wartości wdrożenia, które nie są zgodne, czego oczekuje się za pomocą Menedżera zasobów. Sprawdź komunikat o stanie wewnętrzny Aby uzyskać pomoc dotyczącą rozwiązywania problemów. | [Odwołanie do szablonu](/azure/templates/) i [obsługiwane lokalizacje](resource-manager-templates-resources.md#location) |
| Konflikt | Zażądano operacji, które nie są dozwolone w bieżącym stanie zasobu. Na przykład zmiana rozmiaru dysku jest dozwolona tylko w przypadku tworzenia maszyny Wirtualnej lub po cofnięciu przydziału maszyny Wirtualnej. | |
| DeploymentActive | Poczekaj, aż równoczesnych wdrożenia do tej grupy zasobów, aby zakończyć. | |
| DnsRecordInUse | Nazwa rekordu DNS musi być unikatowa. Podaj inną nazwę, albo zmodyfikować istniejący rekord. | |
| ImageNotFound | Sprawdź ustawienia obrazu maszyny Wirtualnej. |  |
| InUseSubnetCannotBeDeleted | Błąd może się pojawić podczas próby zaktualizowania zasobu, ale żądanie jest przetwarzane przez usunięcie i utworzenie zasobu. Upewnij się określić wszystkie wartości bez zmian. | [Aktualizowanie zasobu](/azure/architecture/building-blocks/extending-templates/update-resource) |
| InvalidAuthenticationTokenTenant | Uzyskaj token dostępu dla odpowiedniego dzierżawcy. Tokenu można uzyskać tylko konta należącego do dzierżawy. | |
| InvalidContentLink | Prawdopodobnie podjęto próbę połączyć zagnieżdżony szablon, który nie jest dostępny. Sprawdź identyfikator URI przewidzianych szablon zagnieżdżony. Jeśli szablon istnieje w ramach konta magazynu, upewnij się, że identyfikator URI jest dostępny. Należy przekazać tokenu sygnatury dostępu Współdzielonego. | [Szablony połączonego](resource-group-linked-templates.md) |
| InvalidParameter | Jedna z wartości podane dla zasobu nie jest zgodna z oczekiwaną wartością. Ten błąd może wynikać z wielu różnych warunkach. Na przykład hasła mogą być niewystarczające lub nazwa obiektu blob może być nieprawidłowa. Sprawdź komunikat o błędzie, aby określić wartość, która musi zostać usunięty. | |
| InvalidRequestContent | Wartości wdrożenia zawierają wartości, które nie powinny lub brakuje wymaganych wartości. Potwierdź wartości dla danego typu zasobu. | [Odwołanie do szablonu](/azure/templates/) |
| InvalidRequestFormat | Włączenie rejestrowania debugowania, podczas wykonywania wdrożenia i Sprawdź treść żądania. | [Funkcję rejestrowania debugowania](resource-manager-troubleshoot-tips.md#enable-debug-logging) |
| InvalidResourceNamespace | Sprawdź przestrzeni nazw zasobu określony w **typu** właściwości. | [Odwołanie do szablonu](/azure/templates/) |
| InvalidResourceReference | Zasób jeszcze nie istnieje lub jest niepoprawnie odwołuje się do. Sprawdź, czy konieczne jest dodanie zależności. Upewnij się, że korzystanie z **odwołania** funkcja zawiera parametry wymagane dla danego scenariusza. | [Rozwiąż zależności](resource-manager-not-found-errors.md) |
| InvalidResourceType | Typ wyboru zasób określony w **typu** właściwości. | [Odwołanie do szablonu](/azure/templates/) |
| InvalidTemplate | Sprawdź składnię szablonu błędów. | [Nieprawidłowy szablon rozwiązania](resource-manager-invalid-template-errors.md) |
| LinkedAuthorizationFailed | Sprawdź, czy używane konto należy do tej samej dzierżawy jako grupa zasobów, które wdrażasz. | |
| LinkedInvalidPropertyId | Identyfikator zasobu dla zasobu nie jest poprawnie rozpoznawania. Sprawdź, podaj wartości wszystkich wymaganych identyfikatorów zasobów, w tym identyfikator subskrypcji, nazwa grupy zasobów, typ zasobu, nazwa zasobu nadrzędnego (Jeśli to konieczne) i nazwa zasobu. | |
| LocationRequired | Podaj lokalizację dla zasobu. | [Ustawianie lokalizacji](resource-manager-templates-resources.md#location) |
| MissingRegistrationForLocation | Sprawdź stan rejestracji dostawcy zasobów i obsługiwane lokalizacje. | [Rozwiąż rejestracji](resource-manager-register-provider-errors.md) |
| MissingSubscriptionRegistration | Zarejestrować subskrypcji u dostawcy zasobów. | [Rozwiąż rejestracji](resource-manager-register-provider-errors.md) |
| NoRegisteredProviderFound | Sprawdzaj stan rejestracji dostawcy zasobów. | [Rozwiąż rejestracji](resource-manager-register-provider-errors.md) |
| Nie odnaleziono | Może być próby wdrożenia zasób zależny równolegle z zasobu nadrzędnego. Sprawdź, czy konieczne jest dodanie zależności. | [Rozwiąż zależności](resource-manager-not-found-errors.md) |
| OperationNotAllowed | Wdrożenie próbuje operację, która przekracza limit przydziału dla subskrypcji, grupy zasobów lub regionu. Jeśli to możliwe Popraw danego wdrożenia, aby pozostać w przydziałów. W przeciwnym razie należy wziąć pod uwagę żądania zmiany przydziałami. | [Rozwiąż przydziałów](resource-manager-quota-errors.md) |
| ParentResourceNotFound | Upewnij się, że istnieje zasób nadrzędnego przed utworzeniem zasoby podrzędne. | [Rozwiąż zasobu nadrzędnego](resource-manager-parent-resource-errors.md) |
| PrivateIPAddressInReservedRange | Określony adres IP zawiera zakres adresów wymagane przez platformę Azure. Zmienianie adresu IP, aby uniknąć zarezerwowany zakres. | [Adresy IP](../virtual-network/virtual-network-ip-addresses-overview-arm.md) |
| PrivateIPAddressNotInSubnet | Określony adres IP jest spoza zakresu podsieci. Zmienianie adresu IP, aby przypadał zakresu podsieci. | [Adresy IP](../virtual-network/virtual-network-ip-addresses-overview-arm.md) |
| PropertyChangeNotAllowed | Niektóre właściwości nie można zmienić na wdrożonych zasobów. Podczas aktualizowania zasobu, należy ograniczyć zmiany właściwości dozwolone. | [Aktualizowanie zasobu](/azure/architecture/building-blocks/extending-templates/update-resource) |
| RequestDisallowedByPolicy | Twoja subskrypcja obejmuje zasadę zasobu, która uniemożliwia akcję, którą próbujesz wykonać podczas wdrażania. Znajdź zasad, która blokuje akcji. Jeśli to możliwe zmodyfikuj wdrożenie do spełnienia ograniczenia z zasad. | [Zasady rozwiązania](resource-manager-policy-requestdisallowedbypolicy-error.md) |
| ReservedResourceName | Podaj nazwę zasobu, która nie zawiera zastrzeżoną nazwą. | [Nazw zarezerwowanych zasobów](resource-manager-reserved-resource-name.md) |
| ResourceGroupBeingDeleted | Poczekaj do usunięcia zakończyć. | |
| ResourceGroupNotFound | Sprawdź nazwę grupy zasobów docelowych wdrożenia. Musi już istnieć w ramach subskrypcji. Sprawdź kontekst subskrypcji. | [Azure CLI](/cli/azure/account?#az_account_set) [programu PowerShell](/powershell/module/azurerm.profile/set-azurermcontext) |
| ResourceNotFound | Wdrożenie odwołuje się do zasobu, którego nie można rozwiązać. Upewnij się, że korzystanie z **odwołania** funkcja zawiera parametrów wymaganych dla danego scenariusza. | [Rozpoznawania odwołań](resource-manager-not-found-errors.md) |
| ResourceQuotaExceeded | Wdrożenie próbuje utworzyć zasoby, które przekraczają limit przydziału dla subskrypcji, grupy zasobów lub regionu. Jeśli to możliwe Popraw infrastruktury ma pozostać w ramach przydziałów. W przeciwnym razie należy wziąć pod uwagę żądania zmiany przydziałami. | [Rozwiąż przydziałów](resource-manager-quota-errors.md) |
| SkuNotAvailable | Wybierz jednostki SKU (takich jak rozmiar maszyny Wirtualnej) dostępnej dla wybranej lokalizacji. | [Rozwiąż jednostki SKU](resource-manager-sku-not-available-errors.md) |
| StorageAccountAlreadyExists | Podaj unikatową nazwę konta magazynu. | [Rozpoznanie nazwy konta magazynu](resource-manager-storage-account-name-errors.md)  |
| StorageAccountAlreadyTaken | Podaj unikatową nazwę konta magazynu. | [Rozpoznanie nazwy konta magazynu](resource-manager-storage-account-name-errors.md) |
| StorageAccountNotFound | Sprawdź subskrypcji, grupy zasobów i nazwy konta magazynu, którego próbujesz użyć. | |
| SubnetsNotInSameVnet | Maszyna wirtualna może mieć tylko jedną sieć wirtualną. W przypadku wdrażania wielu kart sieciowych, upewnij się, że należą one do tej samej sieci wirtualnej. | [Wiele kart sieciowych](../virtual-machines/windows/multiple-nics.md) |

## <a name="find-error-code"></a>Znajdź kod błędu:

Gdy w przypadku wystąpienia błędu podczas wdrażania usługi Resource Manager zwraca kod błędu. Komunikat o błędzie za pośrednictwem portalu, programu PowerShell lub wiersza polecenia platformy Azure. Zewnętrzne komunikat może być zbyt ogólne do rozwiązywania problemów. Poszukaj wewnętrzny wiadomość zawierającą szczegółowe informacje o tym błędzie. Aby uzyskać więcej informacji, zobacz [określić kod błędu](resource-manager-troubleshoot-tips.md#determine-error-code).

## <a name="next-steps"></a>Kolejne kroki
* Aby dowiedzieć się więcej o inspekcji akcji, zobacz [inspekcji operacji za pomocą Menedżera zasobów](resource-group-audit.md).
* Aby dowiedzieć się więcej o akcjach, aby określić błędy podczas wdrażania, zobacz [wyświetlić operacje wdrażania](resource-manager-deployment-operations.md).
