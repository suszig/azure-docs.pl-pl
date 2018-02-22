---
title: "Akcje i NotActions — kontrola dostępu oparta na rolach na platformie Azure (RBAC) | Dokumentacja firmy Microsoft"
description: "W tym temacie opisano wbudowanych ról dla kontroli dostępu opartej na rolach (RBAC). Role są stale dodawane, więc sprawdzaj świeżości dokumentacji."
services: active-directory
documentationcenter: 
author: rolyon
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: identity
ms.date: 01/30/2018
ms.author: rolyon
ms.reviewer: rqureshi
ms.custom: it-pro
ms.openlocfilehash: 82fa6d3f04dc528c0e2d95dae82e7a7f8787ea7c
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/21/2018
---
# <a name="built-in-roles-for-azure-role-based-access-control"></a>Wbudowanych ról dla kontroli dostępu opartej na rolach na platformie Azure
Azure opartej na rolach kontroli dostępu (RBAC) zawiera następujące role wbudowane przypisane do użytkowników, grup i usług. Nie można zmodyfikować definicje ról wbudowanych. Można jednak utworzyć [niestandardowych ról w Azure RBAC](role-based-access-control-custom-roles.md) do określonych potrzeb organizacji.

## <a name="roles-in-azure"></a>Role na platformie Azure
Poniższa tabela zawiera krótkie opisy wbudowane role. Kliknij nazwę roli, aby wyświetlić szczegółowy wykaz **akcje** i **notactions** dla roli. **Akcje** właściwość określa akcje dozwolone dla zasobów platformy Azure. Ciągi akcji można używać symboli wieloznacznych. **Notactions** właściwość określa akcje, które są wykluczone z dozwolonych akcji.

Akcja definiuje, jakiego typu operacje można wykonywać na typ zasobu. Na przykład:
- **Zapis** umożliwia wykonywanie operacji PUT, POST, poprawki i DELETE.
- **Odczyt** umożliwia wykonywanie operacji GET.

W tym artykule opisano tylko różne role, które istnieją już dzisiaj. Po przypisaniu roli na użytkownika, można ograniczyć dalsze akcje dozwolone przez określenie zakresu. Jest to przydatne, jeśli mają być ktoś współautora witryny sieci Web, ale tylko dla jednej grupy zasobów.

> [!NOTE]
> Stale ewoluuje definicje ról platformy Azure. W tym artykule jest aktualizowane tak jak to możliwe, ale zawsze można znaleźć najnowsze definicje ról w programie Azure PowerShell. Użyj [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition) polecenia cmdlet, aby wyświetlić listę wszystkich bieżących ról. Użytkownik może zajrzyj dostęp do konkretnej roli za pomocą `(get-azurermroledefinition "<role name>").actions` lub `(get-azurermroledefinition "<role name>").notactions` ma zastosowanie. Użyj [Get-AzureRmProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation) do operacji listy dostawców określonych zasobów platformy Azure.


| Nazwa roli | Opis |
| --- | --- |
| [Interfejs API zarządzania usługi współautora](#api-management-service-contributor) |Można zarządzać usługą zarządzania interfejsu API i interfejsów API |
| [Rola operatora usługi zarządzania interfejsu API](#api-management-service-operator-role) | Można zarządzać usługi API Management, ale nie za pośrednictwem interfejsów API się |
| [Interfejs API zarządzania usługi czytnika roli](#api-management-service-reader-role) | Dostęp tylko do odczytu do usługi Zarządzanie interfejsami API i interfejsów API |
| [Application Insights składnika współautora](#application-insights-component-contributor) |Umożliwia zarządzanie składnikami usługi Application Insights |
| [Operator automatyzacji](#automation-operator) |Możliwość uruchamianie, zatrzymywanie, wstrzymywanie i wznawianie prac |
| [Współautor kopii zapasowej](#backup-contributor) | Można zarządzać kopiami zapasowymi w magazynie usług odzyskiwania |
| [Operator kopii zapasowych](#backup-operator) | Można zarządzać kopii zapasowej, chyba że usuwanie kopii zapasowej w magazynie usług odzyskiwania |
| [Czytnik kopii zapasowej](#backup-reader) | Można wyświetlić wszystkie usługi zarządzania kopiami zapasowymi  |
| [Czytnik rozliczeń](#billing-reader) | Można wyświetlić wszystkich informacji dotyczących rozliczeń  |
| [BizTalk Contributor](#biztalk-contributor) |Można zarządzać usługi BizTalk services |
| [Współautor ClearDB MySQL bazy danych](#cleardb-mysql-db-contributor) |Można zarządzać baz danych ClearDB MySQL |
| [Contributor](#contributor) |Mogą zarządzać wszystkim poza dostępem. |
| [Współautor fabryki danych](#data-factory-contributor) |Można tworzyć i zarządzać fabryki danych i zasoby podrzędne w nich. |
| [DevTest Labs użytkownika](#devtest-labs-user) |Można wyświetlić wszystko i połącz start, zamknięcia i ponownego uruchomienia maszyny wirtualnej |
| [Współautor strefy DNS](#dns-zone-contributor) |Można zarządzać strefy DNS i rekordów |
| [Współautor konta usługi DocumentDB](#documentdb-account-contributor) |Można zarządzać kontami bazy danych Azure rozwiązania Cosmos |
| [Współautor konta systemów inteligentnych](#intelligent-systems-account-contributor) |Można zarządzać kontami systemów inteligentnych |
| Współautor aplikacji logiki | Można zarządzać wszystkimi aspektami aplikacji logiki, ale nie Utwórz nową. |
| Operator aplikacji logiki |Można uruchomić i zatrzymać przepływów pracy zdefiniowanych w aplikacji logiki. |
| [Czytnik monitorowania](#monitoring-reader) |Może czytać wszystkie dane monitorowania |
| [Monitorowanie współautora](#monitoring-contributor) |Można odczytać danych monitorowania i edytować ustawienia monitorowania |
| [Współautor sieci](#network-contributor) |Można zarządzać wszystkich zasobów sieciowych |
| [Właściciel](#owner) |Mogą zarządzać wszystkim łącznie z dostępem |
| [Reader](#reader) |Mogą przeglądać wszystko, ale nie można wprowadzić zmian |
| [Współautor pamięci podręcznej redis](#redis-cache-contributor) |Można zarządzać pamięci podręczne Redis |
| [Harmonogram zadania kolekcje współautora](#scheduler-job-collections-contributor) |Można zarządzać harmonogramu kolekcji zadań |
| [Współautor usługi wyszukiwania](#search-service-contributor) |Można zarządzać usługi wyszukiwania |
| [Administrator zabezpieczeń](#security-administrator) | W Centrum zabezpieczeń tylko: można wyświetlić zasady zabezpieczeń, wyświetlanie stanów zabezpieczeń, Edytuj zasady zabezpieczeń, wyświetlanie alertów i zalecenia, odrzucać alerty i zalecenia |
| [Security Manager](#security-manager) | Zarządzanie składniki zabezpieczeń, zasady zabezpieczeń i maszyny wirtualne |
| [Czytnik zabezpieczeń](#security-reader) | W Centrum zabezpieczeń tylko: można wyświetlić zalecenia i alertów, widok zasady zabezpieczeń, wyświetlanie stanów zabezpieczeń, ale nie można wprowadzić zmian |
| [Współautor odzyskiwania lokacji](#site-recovery-contributor) | Można zarządzać Site Recovery w magazynie usług odzyskiwania |
| [Operator odzyskiwania lokacji](#site-recovery-operator) | Można zarządzać trybu failover i powrotu po awarii operacje odzyskiwania lokacji w magazynie usług odzyskiwania |
| [Czytnik odzyskiwania lokacji](#site-recovery-reader) | Można wyświetlić wszystkie operacje zarządzania usługi Site Recovery  |
| [Współautor bazy danych SQL](#sql-db-contributor) |Można zarządzać baz danych, ale nie ich zasad związanych z zabezpieczeniami |
| [SQL Security Manager](#sql-security-manager) |Można zarządzać zasadami związanych z zabezpieczeniami serwerów SQL i baz danych |
| [SQL Server współautora](#sql-server-contributor) |Można zarządzać serwerami programu SQL Server i baz danych, ale nie ich zasad związanych z zabezpieczeniami |
| [Współautor konta magazynu Classic](#classic-storage-account-contributor) |Można zarządzać klasycznych kont magazynu |
| [Współautor konta magazynu](#storage-account-contributor) |Można zarządzać kontami magazynu |
| [Współautor żądania obsługi](#support-request-contributor) | Można tworzyć i zarządzać żądania pomocy technicznej |
| [Administrator dostępu użytkowników](#user-access-administrator) |Można zarządzać dostępem użytkowników do zasobów platformy Azure |
| [Współautor klasyczne maszyny wirtualnej](#classic-virtual-machine-contributor) |Można zarządzać klasycznych maszyn wirtualnych, ale nie z wirtualnych sieci lub magazynu konta do którego są podłączeni |
| [Współautor maszyny wirtualnej](#virtual-machine-contributor) |Można zarządzać maszyn wirtualnych, ale nie z wirtualnych sieci lub magazynu konta do którego są podłączeni |
| [Współautor klasycznej sieci](#classic-network-contributor) |Można zarządzać klasycznych sieci wirtualnych i zarezerwowane adresy IP |
| [Współautor Plan sieci Web](#web-plan-contributor) |Można zarządzać planami sieci web |
| [Współautor witryny sieci Web](#website-contributor) |Można zarządzać witryn sieci Web, ale nie planów sieci web, z którymi są połączone |

## <a name="role-permissions"></a>Uprawnienia roli
W poniższych tabelach opisano określone uprawnienia do poszczególnych ról. Może to obejmować **akcje**, które dają uprawnienia, i **NotActions**, które ograniczają.

### <a name="api-management-service-contributor"></a>Współautor usługi API Management
Interfejs API zarządzania usługami można zarządzać

| **Akcje** |  |
| --- | --- |
| Microsoft.ApiManagement/Service/* |Tworzenie i zarządzanie nimi usługi Zarządzanie interfejsami API |
| Microsoft.Authorization/*/read |Przeczytaj autoryzacji |
| Microsoft.Insights/alertRules/* |Tworzenie i zarządzanie nimi reguły alertów |
| Microsoft.ResourceHealth/availabilityStatuses/read |Kondycja odczytu zasobów |
| Microsoft.Resources/deployments/* |Tworzenie i zarządzanie nimi wdrożenia grupy zasobów |
| Microsoft.Resources/subscriptions/resourceGroups/read |Role odczytu i przypisania ról |
| Microsoft.Support/* |Tworzenie i zarządzanie biletami pomocy technicznej |

### <a name="api-management-service-operator-role"></a>Rola Operator usługi API Management
Interfejs API zarządzania usługami można zarządzać

| **Akcje** |  |
| --- | --- |
| Microsoft.ApiManagement/Service/*/read | Wystąpienie usługi zarządzania interfejsu API odczytu |
| Microsoft.ApiManagement/Service/backup/action | Tworzenie kopii zapasowej usługi interfejsu API zarządzania określonego kontenera użytkownika podane konto magazynu |
| Microsoft.ApiManagement/Service/delete | Usuwanie wystąpienia interfejsu API usługi zarządzania |
| Microsoft.ApiManagement/Service/managedeployments/action | Zmień jednostki SKU/jednostki; Dodawanie lub usuwanie regionalnych wdrożenia usługi Management API |
| Microsoft.ApiManagement/Service/read | Odczytać metadanych dla wystąpienia interfejsu API usługi zarządzania |
| Microsoft.ApiManagement/Service/restore/action | Przywracanie usługi interfejsu API zarządzania z kontenera określonej w użytkownika podane konto magazynu |
| Microsoft.ApiManagement/Service/updatehostname/action | Konfigurowanie, zaktualizować lub usunąć nazwy domeny niestandardowej dla usługi interfejsu API zarządzania |
| Microsoft.ApiManagement/Service/write | Utwórz nowe wystąpienie interfejsu API usługi zarządzania |
| Microsoft.Authorization/*/read |Przeczytaj autoryzacji |
| Microsoft.Insights/alertRules/* |Tworzenie i zarządzanie nimi reguły alertów |
| Microsoft.ResourceHealth/availabilityStatuses/read |Kondycja odczytu zasobów |
| Microsoft.Resources/deployments/* |Tworzenie i zarządzanie nimi wdrożenia grupy zasobów |
| Microsoft.Resources/subscriptions/resourceGroups/read |Role odczytu i przypisania ról |
| Microsoft.Support/* |Tworzenie i zarządzanie biletami pomocy technicznej |

### <a name="api-management-service-reader-role"></a>Rola Czytnik usługi API Management
Interfejs API zarządzania usługami można zarządzać

| **Akcje** |  |
| --- | --- |
| Microsoft.ApiManagement/Service/*/read | Wystąpienie usługi zarządzania interfejsu API odczytu |
| Microsoft.ApiManagement/Service/read | Odczytać metadanych dla wystąpienia interfejsu API usługi zarządzania |
| Microsoft.Authorization/*/read |Przeczytaj autoryzacji |
| Microsoft.Insights/alertRules/* |Tworzenie i zarządzanie nimi reguły alertów |
| Microsoft.ResourceHealth/availabilityStatuses/read |Kondycja odczytu zasobów |
| Microsoft.Resources/deployments/* |Tworzenie i zarządzanie nimi wdrożenia grupy zasobów |
| Microsoft.Resources/subscriptions/resourceGroups/read |Role odczytu i przypisania ról |
| Microsoft.Support/* |Tworzenie i zarządzanie biletami pomocy technicznej |

### <a name="application-insights-component-contributor"></a>Współautor składników usługi Application Insights
Umożliwia zarządzanie składnikami usługi Application Insights

| **Akcje** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Role odczytu i przypisania ról |
| Microsoft.Insights/alertRules/* |Tworzenie i zarządzanie nimi reguły alertów |
| Microsoft.Insights/components/* |Tworzenie i zarządzanie nimi składniki Insights |
| Microsoft.Insights/webtests/* |Tworzenie i zarządzanie nimi testy sieci web |
| Microsoft.ResourceHealth/availabilityStatuses/read |Kondycja odczytu zasobów |
| Microsoft.Resources/deployments/* |Tworzenie i zarządzanie nimi wdrożenia grupy zasobów |
| Microsoft.Resources/subscriptions/resourceGroups/read |Odczytanie grup zasobów |
| Microsoft.Support/* |Tworzenie i zarządzanie biletami pomocy technicznej |

### <a name="automation-operator"></a>Operator usługi
Możliwość uruchamianie, zatrzymywanie, wstrzymywanie i wznawianie prac

| **Akcje** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Role odczytu i przypisania ról |
| Microsoft.Automation/automationAccounts/jobs/read |Przeczytaj zadania konta automatyzacji |
| Microsoft.Automation/automationAccounts/jobs/resume/action |Wznów zadanie usługi Automatyzacja konta |
| Microsoft.Automation/automationAccounts/jobs/stop/action |Zatrzymaj zadanie usługi Automatyzacja konta |
| Microsoft.Automation/automationAccounts/jobs/streams/read |Przeczytaj strumieni zadania konta automatyzacji |
| Microsoft.Automation/automationAccounts/jobs/suspend/action |Wstrzymaj zadanie usługi Automatyzacja konta |
| Microsoft.Automation/automationAccounts/jobs/write |Zapis zadania konta automatyzacji |
| Microsoft.Automation/automationAccounts/jobSchedules/read |Przeczytaj harmonogram zadania konta automatyzacji |
| Microsoft.Automation/automationAccounts/jobSchedules/write |Przeczytaj harmonogram zadania konta automatyzacji |
| Microsoft.Automation/automationAccounts/read |Odczytywanie kont automatyzacji |
| Microsoft.Automation/automationAccounts/runbooks/read |Odczytaj element runbook usługi Automatyzacja |
| Microsoft.Automation/automationAccounts/schedules/read |Przegląd automatyzacji arkuszy kont |
| Microsoft.Automation/automationAccounts/schedules/write |Zapis harmonogramy konta automatyzacji |
| Microsoft.Insights/components/* |Tworzenie i zarządzanie nimi składniki Insights |
| Microsoft.ResourceHealth/availabilityStatuses/read |Kondycja odczytu zasobów |
| Microsoft.Resources/deployments/* |Tworzenie i zarządzanie nimi wdrożenia grupy zasobów |
| Microsoft.Resources/subscriptions/resourceGroups/read |Odczytanie grup zasobów |
| Microsoft.Support/* |Tworzenie i zarządzanie biletami pomocy technicznej |

### <a name="backup-contributor"></a>Współautor kopii zapasowych
Można zarządzać wszystkie akcje zarządzania kopiami zapasowymi, z wyjątkiem Tworzenie magazynu usług odzyskiwania i zapewnieniu dostępu do innych użytkowników

| **Akcje** | |
| --- | --- |
| Microsoft.Network/virtualNetworks/read | Sieci wirtualne odczytu |
| Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/* | Zarządzanie wyniki operacji tworzenia kopii zapasowej zarządzania |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/* | Tworzenie i zarządzanie nimi kopii zapasowej kontenery w kopii zapasowej sieci szkieletowej magazynu usług odzyskiwania |
| Microsoft.RecoveryServices/Vaults/backupJobs/* | Tworzenie i zarządzanie nimi zadania tworzenia kopii zapasowej |
| Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Eksportowanie zadań tworzenia kopii zapasowej do programu excel |
| Microsoft.RecoveryServices/Vaults/backupManagementMetaData/* | Tworzenie i zarządzanie nimi metadane związane z zarządzaniem kopii zapasowej |
| Microsoft.RecoveryServices/Vaults/backupOperationResults/* | Utwórz i Zarządzaj wynikami operacji zarządzania kopiami zapasowymi |
| Microsoft.RecoveryServices/Vaults/backupPolicies/* | Tworzenie i zarządzanie zasadami tworzenia kopii zapasowej |
| Microsoft.RecoveryServices/Vaults/backupProtectableItems/* | Tworzenie i zarządzanie nimi elementy, które można utworzyć kopię zapasową |
| Microsoft.RecoveryServices/Vaults/backupProtectedItems/* | Tworzenie i zarządzanie nimi kopii zapasowej elementów |
| Microsoft.RecoveryServices/Vaults/backupProtectionContainers/* | Tworzenie i zarządzanie nimi kontenery zawierający elementy kopii zapasowej |
| Microsoft.RecoveryServices/Vaults/certificates/* | Tworzenie i zarządzanie certyfikatami związane z kopii zapasowej w magazynie usług odzyskiwania |
| Microsoft.RecoveryServices/Vaults/extendedInformation/* | Tworzenie i zarządzanie nimi rozszerzone informacje związane z magazynu |
| Microsoft.RecoveryServices/Vaults/read | Magazyny usług odzyskiwania do odczytu |
| Microsoft.RecoveryServices/Vaults/refreshContainers/* | Zarządzanie pobieranie operacji wykrywania nowo utworzony kontenerów |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/* | Tworzenie i Zarządzanie tożsamościami w zarejestrowany |
| Microsoft.RecoveryServices/Vaults/usages/* | Tworzenie i zarządzanie użyciem magazynu usług odzyskiwania |
| Microsoft.Resources/deployments/* | Tworzenie i zarządzanie nimi wdrożenia grupy zasobów |
| Microsoft.Resources/subscriptions/resourceGroups/read | Odczytanie grup zasobów |
| Microsoft.Storage/storageAccounts/read | Przeczytaj kont magazynu |
| Microsoft.Support/* |Tworzenie i zarządzanie biletami pomocy technicznej |

### <a name="backup-operator"></a>Operator kopii zapasowych
Można zarządzać wszystkie akcje zarządzania kopiami zapasowymi, z wyjątkiem tworzenia magazynów, usuwanie kopii zapasowej i dające dostęp do innych użytkowników

| **Akcje** | |
| --- | --- |
| Microsoft.Network/virtualNetworks/read | Sieci wirtualne odczytu |
| Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read | Przeczytaj wyniki operacji tworzenia kopii zapasowej zarządzania |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/operationResults/read | Wyniki operacji odczytu do kontenerów ochrony |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/backup/action | Wykonaj operację tworzenia kopii zapasowej na żądanie w elemencie kopii zapasowej |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationResults/read | Wynik odczytu operację na kopię zapasową elementu |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationStatus/read | Stan odczytu operację na kopię zapasową elementu |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read | Odczyt kopii zapasowej elementów |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/read | Punkt odzyskiwania odczytu elementu kopii zapasowej |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/restore/action | Operacja przywracania przy użyciu punktu odzyskiwania z kopii zapasowej elementu |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/write | Tworzenie kopii zapasowej elementu |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read | Kontenery zawierający element kopii zapasowej |
| Microsoft.RecoveryServices/Vaults/backupJobs/* | Tworzenie i zarządzanie nimi zadania tworzenia kopii zapasowej |
| Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Eksportowanie zadań tworzenia kopii zapasowej do programu excel |
| Microsoft.RecoveryServices/Vaults/backupManagementMetaData/read | Odczyt meta danych związanych z zarządzaniem kopii zapasowej |
| Microsoft.RecoveryServices/Vaults/backupOperationResults/* | Utwórz i Zarządzaj wynikami operacji zarządzania kopiami zapasowymi |
| Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read | Odczyt wyniki operacji wykonywanych na zasady tworzenia kopii zapasowej |
| Microsoft.RecoveryServices/Vaults/backupPolicies/read | Przeczytaj zasady tworzenia kopii zapasowej |
| Microsoft.RecoveryServices/Vaults/backupProtectableItems/* | Tworzenie i zarządzanie nimi elementy, które można utworzyć kopię zapasową |
| Microsoft.RecoveryServices/Vaults/backupProtectedItems/read | Odczyt kopii zapasowej elementów |
| Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read | Odczyt kopii zapasowej kontenery zawierający elementy kopii zapasowej |
| Microsoft.RecoveryServices/Vaults/extendedInformation/read | Odczyt rozszerzone informacje związane z magazynu |
| Microsoft.RecoveryServices/Vaults/extendedInformation/write | Zapis rozszerzone informacje związane z magazynu |
| Microsoft.RecoveryServices/Vaults/read | Magazyny usług odzyskiwania do odczytu |
| Microsoft.RecoveryServices/Vaults/refreshContainers/* | Zarządzanie pobieranie operacji wykrywania nowo utworzony kontenerów |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | Wyniki operacji odczytu wykonywane na elementach zarejestrowanej magazynu |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Odczytuj elementy zarejestrowanych magazynu |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/write | Zapisz zarejestrowane elementy do magazynu |
| Microsoft.RecoveryServices/Vaults/usages/read | Użycie odczytu z magazynu usług odzyskiwania |
| Microsoft.Resources/deployments/* | Tworzenie i zarządzanie nimi wdrożenia grupy zasobów |
| Microsoft.Resources/subscriptions/resourceGroups/read | Odczytanie grup zasobów |
| Microsoft.Storage/storageAccounts/read | Przeczytaj kont magazynu |
| Microsoft.Support/* | Tworzenie i zarządzanie biletami pomocy technicznej |

### <a name="backup-reader"></a>Czytelnik kopii zapasowych
Można monitorować Zarządzanie kopii zapasowej w magazynie usług odzyskiwania

| **Akcje** | |
| --- | --- |
| Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read  | Przeczytaj wyniki operacji tworzenia kopii zapasowej zarządzania |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/operationResults/read  | Wyniki operacji odczytu do kontenerów ochrony |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationResults/read  | Wynik odczytu operację na kopię zapasową elementu |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationStatus/read  | Stan odczytu operację na kopię zapasową elementu |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read  | Odczyt kopii zapasowej elementów |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read  | Kontenery zawierający element kopii zapasowej |
| Microsoft.RecoveryServices/Vaults/backupJobs/operationResults/read  | Przeczytaj wyniki zadań tworzenia kopii zapasowej |
| Microsoft.RecoveryServices/Vaults/backupJobs/read  | Odczyt zadań tworzenia kopii zapasowej |
| Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Eksportowanie zadań tworzenia kopii zapasowej do programu excel |
| Microsoft.RecoveryServices/Vaults/backupManagementMetaData/read  | Odczyt meta danych związanych z zarządzaniem kopii zapasowej |
| Microsoft.RecoveryServices/Vaults/backupOperationResults/read  | Odczytać wyników operacji zarządzania kopiami zapasowymi |
| Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read  | Odczyt wyniki operacji wykonywanych na zasady tworzenia kopii zapasowej |
| Microsoft.RecoveryServices/Vaults/backupPolicies/read  | Przeczytaj zasady tworzenia kopii zapasowej |
| Microsoft.RecoveryServices/Vaults/backupProtectedItems/read  |  Odczyt kopii zapasowej elementów |
| Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read  | Odczyt kopii zapasowej kontenery zawierający elementy kopii zapasowej |
| Microsoft.RecoveryServices/Vaults/extendedInformation/read  | Odczyt rozszerzone informacje związane z magazynu |
| Microsoft.RecoveryServices/Vaults/read  | Magazyny usług odzyskiwania do odczytu |
| Microsoft.RecoveryServices/Vaults/refreshContainers/read  | Wynik operacji odnajdywania dla pobierania nowo utworzony kontenerów do odczytu |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read  | Wyniki operacji odczytu wykonywane na elementach zarejestrowanej magazynu |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/read  | Odczytuj elementy zarejestrowanych magazynu |
| Microsoft.RecoveryServices/Vaults/usages/read  |  Użycie odczytu z magazynu usług odzyskiwania |

### <a name="billing-reader"></a>Czytelnik rozliczeń
Może wyświetlać wszystkie informacje rozliczeń

| **Akcje** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Role odczytu i przypisania ról |
| Microsoft.Billing/*/read |Odczytywanie informacji o rozliczeń |
| Microsoft.Support/* |Tworzenie i zarządzanie biletami pomocy technicznej |

### <a name="biztalk-contributor"></a>Współautor usługi BizTalk
Można zarządzać usługi BizTalk services

| **Akcje** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Role odczytu i przypisania ról |
| Microsoft.BizTalkServices/BizTalk/* |Tworzenie i zarządzanie nimi usługi BizTalk services |
| Microsoft.Insights/alertRules/* |Tworzenie i zarządzanie nimi reguły alertów |
| Microsoft.ResourceHealth/availabilityStatuses/read |Kondycja odczytu zasobów |
| Microsoft.Resources/deployments/* |Tworzenie i zarządzanie nimi wdrożenia grupy zasobów |
| Microsoft.Resources/subscriptions/resourceGroups/read |Odczytanie grup zasobów |
| Microsoft.Support/* |Tworzenie i zarządzanie biletami pomocy technicznej |

### <a name="cleardb-mysql-db-contributor"></a>Współautor bazy danych MySQL ClearDB
Można zarządzać baz danych ClearDB MySQL

| **Akcje** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Role odczytu i przypisania ról |
| Microsoft.Insights/alertRules/* |Tworzenie i zarządzanie nimi reguły alertów |
| Microsoft.ResourceHealth/availabilityStatuses/read |Kondycja odczytu zasobów |
| Microsoft.Resources/deployments/* |Tworzenie i zarządzanie nimi wdrożenia grupy zasobów |
| Microsoft.Resources/subscriptions/resourceGroups/read |Odczytanie grup zasobów |
| Microsoft.Support/* |Tworzenie i zarządzanie biletami pomocy technicznej |
| successbricks.cleardb/databases/* |Tworzenie i zarządzanie bazami danych ClearDB MySQL |

### <a name="contributor"></a>Współautor
Mogą zarządzać wszystkim poza dostępem

| **Akcje** |  |
| --- | --- |
| * |Tworzenie i zarządzanie zasobami wszystkich typów |

| NotActions |  |
| --- | --- |
| Microsoft.Authorization/*/Delete |Nie można usuwać role i przypisania ról |
| Microsoft.Authorization/*/Write |Nie można utworzyć role i przypisania ról |

### <a name="data-factory-contributor"></a>Współautor Data Factory
Utwórz i Zarządzaj fabryki danych i zasoby podrzędne w nich.

| **Akcje** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Role odczytu i przypisania roli |
| Microsoft.DataFactory/dataFactories/* |Utwórz i Zarządzaj fabryki danych i zasoby podrzędne w nich. |
| Microsoft.Insights/alertRules/* |Tworzenie i zarządzanie nimi reguły alertów |
| Microsoft.ResourceHealth/availabilityStatuses/read |Kondycja odczytu zasobów |
| Microsoft.Resources/deployments/* |Tworzenie i zarządzanie nimi wdrożenia grupy zasobów |
| Microsoft.Resources/subscriptions/resourceGroups/read |Odczytanie grup zasobów |
| Microsoft.Support/* |Tworzenie i zarządzanie biletami pomocy technicznej |

### <a name="devtest-labs-user"></a>Użytkownik usługi DevTest Labs
Można wyświetlić wszystko i połącz start, zamknięcia i ponownego uruchomienia maszyny wirtualnej

| **Akcje** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Role odczytu i przypisania roli |
| Microsoft.Compute/availabilitySets/read |Odczytywania właściwości zestawów dostępności |
| Microsoft.Compute/virtualMachines/*/read |Odczytywania właściwości maszyny wirtualnej (rozmiary maszyn wirtualnych stanu w czasie wykonywania, rozszerzeń maszyny Wirtualnej, itp.) |
| Microsoft.Compute/virtualMachines/deallocate/action |Cofnij Przydział maszyny wirtualne |
| Microsoft.Compute/virtualMachines/read |Odczytywania właściwości maszyny wirtualnej |
| Microsoft.Compute/virtualMachines/restart/action |Ponowne uruchomienie maszyn wirtualnych |
| Microsoft.Compute/virtualMachines/start/action |Uruchamianie maszyn wirtualnych |
| Microsoft.DevTestLab/*/read |Odczytywania właściwości laboratorium |
| Microsoft.DevTestLab/labs/createEnvironment/action |Tworzenie środowiska laboratoryjnego |
| Microsoft.DevTestLab/labs/formulas/delete |Usuń formuły |
| Microsoft.DevTestLab/labs/formulas/read |Formuły odczytu |
| Microsoft.DevTestLab/labs/formulas/write |Dodaje lub modyfikuje formuł |
| Microsoft.DevTestLab/labs/policySets/evaluatePolicies/action |Ocena zasad laboratorium |
| Microsoft.Network/loadBalancers/backendAddressPools/join/action |Dołącz do puli adresów zaplecza modułu równoważenia obciążenia |
| Microsoft.Network/loadBalancers/inboundNatRules/join/action |Dołącz do modułu równoważenia obciążenia przychodzącą regułę NAT |
| Microsoft.Network/networkInterfaces/*/read |Odczytywania właściwości interfejsu sieciowego (na przykład wszystkich usług równoważenia obciążenia interfejsu sieciowego jest częścią) |
| Microsoft.Network/networkInterfaces/join/action |Dołącz maszynę wirtualną do interfejsu sieciowego |
| Microsoft.Network/networkInterfaces/read |Interfejsy sieciowe odczytu |
| Microsoft.Network/networkInterfaces/write |Zapis interfejsy sieciowe |
| Microsoft.Network/publicIPAddresses/*/read |Odczytywania właściwości publicznego adresu IP |
| Microsoft.Network/publicIPAddresses/join/action |Dołącz do publicznego adresu IP |
| Microsoft.Network/publicIPAddresses/read |Przeczytaj sieci publicznych adresów IP |
| Microsoft.Network/virtualNetworks/subnets/join/action |Dołącz do sieci wirtualnej |
| Microsoft.Resources/deployments/operations/read |Operacje odczytu wdrożenia |
| Microsoft.Resources/deployments/read |Przeczytaj wdrożenia |
| Microsoft.Resources/subscriptions/resourceGroups/read |Odczytanie grup zasobów |
| Microsoft.Storage/storageAccounts/listKeys/action |Wyświetl klucze konta magazynu |

### <a name="dns-zone-contributor"></a>Współautor strefy DNS
Strefy DNS i rekordy można zarządzać.

| **Akcje** |  |
| --- | --- |
| Microsoft.Authorization/\*/read |Role odczytu i przypisania ról |
| Microsoft.Insights/alertRules/\* |Tworzenie i zarządzanie nimi reguły alertów |
| Microsoft.Network/dnsZones/\* |Tworzenie i zarządzanie strefy DNS i rekordów |
| Microsoft.ResourceHealth/availabilityStatuses/read |Przeczytaj kondycji zasobów |
| Microsoft.Resources/deployments/\* |Tworzenie i zarządzanie nimi wdrożenia grupy zasobów |
| Microsoft.Resources/subscriptions/resourceGroups/read |Odczytanie grup zasobów |
| Microsoft.Support/\* |Tworzenie i zarządzanie biletami pomocy technicznej |

### <a name="documentdb-account-contributor"></a>Współautor konta bazy danych DocumentDB
Można zarządzać kontami bazy danych Azure rozwiązania Cosmos. Azure DB rozwiązania Cosmos jest znanego wcześniej jako usługi DocumentDB.

| **Akcje** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Role odczytu i przypisania roli |
| Microsoft.DocumentDb/databaseAccounts/* |Tworzenie i zarządzanie kontami bazy danych Azure rozwiązania Cosmos |
| Microsoft.Insights/alertRules/* |Tworzenie i zarządzanie nimi reguły alertów |
| Microsoft.ResourceHealth/availabilityStatuses/read |Kondycja odczytu zasobów |
| Microsoft.Resources/deployments/* |Tworzenie i zarządzanie nimi wdrożenia grupy zasobów |
| Microsoft.Resources/subscriptions/resourceGroups/read |Odczytanie grup zasobów |
| Microsoft.Support/* |Tworzenie i zarządzanie biletami pomocy technicznej |

### <a name="intelligent-systems-account-contributor"></a>Współautor konta usługi Systemy inteligentne
Można zarządzać kontami systemów inteligentnych

| **Akcje** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Role odczytu i przypisania roli |
| Microsoft.Insights/alertRules/* |Tworzenie i zarządzanie nimi reguły alertów |
| Microsoft.IntelligentSystems/accounts/* |Tworzenie i zarządzanie kontami systemów inteligentnych |
| Microsoft.ResourceHealth/availabilityStatuses/read |Kondycja odczytu zasobów |
| Microsoft.Resources/deployments/* |Tworzenie i zarządzanie nimi wdrożenia grupy zasobów |
| Microsoft.Resources/subscriptions/resourceGroups/read |Odczytanie grup zasobów |
| Microsoft.Support/* |Tworzenie i zarządzanie biletami pomocy technicznej |

### <a name="monitoring-reader"></a>Czytelnik monitorowania
Może czytać wszystkie dane monitorowania (metryki, dzienniki itp.). Zobacz też [Rozpoczynanie pracy z rolami, uprawnienia i zabezpieczeń z monitorem Azure](/monitoring-and-diagnostics/monitoring-roles-permissions-security.md#built-in-monitoring-roles).

| **Akcje** |  |
| --- | --- |
| * / Odczyt |Przeczytaj zasoby wszystkich typów, z wyjątkiem kluczy tajnych. |
| Microsoft.OperationalInsights/workspaces/search/action |Wyszukiwanie danych analizy dzienników |
| Microsoft.Support/* |Tworzenie i zarządzanie biletami pomocy technicznej |

### <a name="monitoring-contributor"></a>Współautor monitorowania
Można odczytać wszystkich danych monitorowania i edytować ustawienia monitorowania. Zobacz też [Rozpoczynanie pracy z rolami, uprawnienia i zabezpieczeń z monitorem Azure](/monitoring-and-diagnostics/monitoring-roles-permissions-security.md#built-in-monitoring-roles).

| **Akcje** |  |
| --- | --- |
| * / Odczyt |Przeczytaj zasoby wszystkich typów, z wyjątkiem kluczy tajnych. |
| Microsoft.Insights/AlertRules/* |Odczyt/zapis/usuwania reguł alertów. |
| Microsoft.Insights/components/* |Odczyt/zapis/usuwania składników usługi Application Insights. |
| Microsoft.Insights/DiagnosticSettings/* |Ustawienia diagnostyki odczytu/zapisu/usuwania. |
| Microsoft.Insights/eventtypes/* |Wyświetl zdarzenia dziennika aktywności (zdarzeń zarządzania) w ramach subskrypcji. To uprawnienie jest dotyczy zarówno programowe, jak i portalu dostęp do dziennika aktywności. |
| Microsoft.Insights/LogDefinitions/* |To uprawnienie jest wymagane dla użytkowników, którzy potrzebują dostępu do dzienników aktywności w portalu. Lista kategorii dziennika w dzienniku aktywności. |
| Microsoft.Insights/MetricDefinitions/* |Odczytaj definicje metryki (listy dostępnych typów metryki dla zasobu). |
| Microsoft.Insights/Metrics/* |Odczytać metryki dla zasobu. |
| Microsoft.Insights/Register/Action |Zarejestruj dostawcę w elemencie Microsoft.Insights. |
| Microsoft.Insights/webtests/* |Testów sieci web usługi Application Insights dla odczytu/zapisu/usuwania. |
| Microsoft.OperationalInsights/workspaces/intelligencepacks/* |Pakiety rozwiązania analizy dzienników dla odczytu/zapisu/usuwania. |
| Microsoft.OperationalInsights/workspaces/savedSearches/* |Analiza dzienników odczytu/zapisu/usuwania zapisane wyszukiwania. |
| Microsoft.OperationalInsights/workspaces/search/action |Wyszukaj analizy dzienników obszarów roboczych. |
| Microsoft.OperationalInsights/workspaces/sharedKeys/action |Lista kluczy do obszaru roboczego analizy dzienników. |
| Microsoft.OperationalInsights/workspaces/storageinsightconfigs/* |Konfiguracje wglądu magazynu analizy dzienników odczytu/zapisu/usuwania. |

### <a name="network-contributor"></a>Współautor sieci
Można zarządzać wszystkich zasobów sieciowych

| **Akcje** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Role odczytu i przypisania roli |
| Microsoft.Insights/alertRules/* |Tworzenie i zarządzanie nimi reguły alertów |
| Microsoft.Network/* |Tworzenie i zarządzanie sieciami |
| Microsoft.ResourceHealth/availabilityStatuses/read |Kondycja odczytu zasobów |
| Microsoft.Resources/deployments/* |Tworzenie i zarządzanie nimi wdrożenia grupy zasobów |
| Microsoft.Resources/subscriptions/resourceGroups/read |Odczytanie grup zasobów |
| Microsoft.Support/* |Tworzenie i zarządzanie biletami pomocy technicznej |

### <a name="owner"></a>Właściciel
Mogą zarządzać wszystkim łącznie z dostępem

| **Akcje** |  |
| --- | --- |
| * |Tworzenie i zarządzanie zasobami wszystkich typów |

### <a name="reader"></a>Czytelnik
Mogą przeglądać wszystko, ale nie można wprowadzić zmian

| **Akcje** |  |
| --- | --- |
| * / Odczyt |Przeczytaj zasoby wszystkich typów, z wyjątkiem kluczy tajnych. |

### <a name="redis-cache-contributor"></a>Współautor pamięci podręcznej Redis
Można zarządzać pamięci podręczne Redis

| **Akcje** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Role odczytu i przypisania roli |
| Microsoft.Cache/redis/* |Tworzenie i zarządzanie nimi pamięci podręczne Redis |
| Microsoft.Insights/alertRules/* |Tworzenie i zarządzanie nimi reguły alertów |
| Microsoft.ResourceHealth/availabilityStatuses/read |Kondycja odczytu zasobów |
| Microsoft.Resources/deployments/* |Tworzenie i zarządzanie nimi wdrożenia grupy zasobów |
| Microsoft.Resources/subscriptions/resourceGroups/read |Odczytanie grup zasobów |
| Microsoft.Support/* |Tworzenie i zarządzanie biletami pomocy technicznej |

### <a name="scheduler-job-collections-contributor"></a>Współautor kolekcji zadań usługi Harmonogram
Można zarządzać harmonogramu kolekcji zadań

| **Akcje** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Role odczytu i przypisania roli |
| Microsoft.Insights/alertRules/* |Tworzenie i zarządzanie nimi reguły alertów |
| Microsoft.ResourceHealth/availabilityStatuses/read |Kondycja odczytu zasobów |
| Microsoft.Resources/deployments/* |Tworzenie i zarządzanie nimi wdrożenia grupy zasobów |
| Microsoft.Resources/subscriptions/resourceGroups/read |Odczytanie grup zasobów |
| Microsoft.Scheduler/jobcollections/* |Tworzenie i zarządzanie nimi kolekcji zadań |
| Microsoft.Support/* |Tworzenie i zarządzanie biletami pomocy technicznej |

### <a name="search-service-contributor"></a>Współautor usługi wyszukiwania
Można zarządzać usługi wyszukiwania

| **Akcje** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Role odczytu i przypisania roli |
| Microsoft.Insights/alertRules/* |Tworzenie i zarządzanie nimi reguły alertów |
| Microsoft.ResourceHealth/availabilityStatuses/read |Kondycja odczytu zasobów |
| Microsoft.Resources/deployments/* |Tworzenie i zarządzanie nimi wdrożenia grupy zasobów |
| Microsoft.Resources/subscriptions/resourceGroups/read |Odczytanie grup zasobów |
| Microsoft.Search/searchServices/* |Tworzenie i zarządzanie nimi usługi wyszukiwania |
| Microsoft.Support/* |Tworzenie i zarządzanie biletami pomocy technicznej |

### <a name="security-administrator"></a>Administrator zabezpieczeń
W Centrum zabezpieczeń tylko: można wyświetlić zasady zabezpieczeń, wyświetlanie stanów zabezpieczeń, Edytuj zasady zabezpieczeń, wyświetlanie alertów i zalecenia, odrzucać alerty i zalecenia

| **Akcje** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Role odczytu i przypisania ról |
| Microsoft.Authorization/policyAssignments/* | Tworzenie i zarządzanie nimi przypisania zasad |
| Microsoft.Authorization/policySetDefinitions/* | Tworzenie i zarządzanie nimi zestawów zasad |
| Microsoft.Authorization/policyDefinitions/* | Tworzenie i zarządzanie nimi definicje zasad |
| Microsoft.Insights/alertRules/* | Tworzenie i zarządzanie nimi reguły alertów |
| Microsoft.operationalInsights/workspaces/*/read | Wyświetl dane analizy dzienników |
| Microsoft.Resources/deployments/* |Tworzenie i zarządzanie nimi wdrożenia grupy zasobów |
| Microsoft.Resources/subscriptions/resourceGroups/read |Odczytanie grup zasobów |
| Microsoft.Security/*/read | Składniki zabezpieczeń Odczytaj i zasady |
| Microsoft.Support/* |Tworzenie i zarządzanie biletami pomocy technicznej |

### <a name="security-manager"></a>Menedżer zabezpieczeń
Zarządzanie składniki zabezpieczeń, zasady zabezpieczeń i maszyny wirtualne

| **Akcje** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Role odczytu i przypisania ról |
| Microsoft.ClassicCompute/*/read |Przeczytaj informacje o konfiguracji klasycznych maszyn wirtualnych |
| Microsoft.ClassicCompute/virtualMachines/*/write |Zapisać konfiguracji dla klasycznych maszyn wirtualnych |
| Microsoft.ClassicNetwork/*/read |Odczytać informacji o sieci klasycznego |
| Microsoft.Insights/alertRules/* | Tworzenie i zarządzanie nimi reguły alertów |
| Microsoft.ResourceHealth/availabilityStatuses/read |Kondycja odczytu zasobów |
| Microsoft.Resources/deployments/* |Tworzenie i zarządzanie nimi wdrożenia grupy zasobów |
| Microsoft.Resources/subscriptions/resourceGroups/read |Odczytanie grup zasobów |
| Microsoft.Security/* |Tworzenie i zarządzanie składników zabezpieczeń i zasady |
| Microsoft.Support/* |Tworzenie i zarządzanie biletami pomocy technicznej |

### <a name="security-reader"></a>Czytelnik zabezpieczeń
W Centrum zabezpieczeń tylko: można wyświetlić zalecenia i alertów, widok zasady zabezpieczeń, wyświetlanie stanów zabezpieczeń, ale nie można wprowadzić zmian

| **Akcje** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Role odczytu i przypisania ról |
| Microsoft.Insights/alertRules/* | Tworzenie i zarządzanie nimi reguły alertów |
| Microsoft.operationalInsights/workspaces/*/read | Wyświetl dane analizy dzienników |
| Microsoft.Resources/subscriptions/resourceGroups/read |Odczytanie grup zasobów |
| Microsoft.Security/*/read | Składniki zabezpieczeń Odczytaj i zasady |
| Microsoft.Support/* |Tworzenie i zarządzanie biletami pomocy technicznej |
| Microsoft.Resources/deployments/* |Tworzenie i zarządzanie nimi wdrożenia grupy zasobów |

### <a name="site-recovery-contributor"></a>Współautor usługi Site Recovery
Można zarządzać wszystkie akcje zarządzania odzyskiwania lokacji, z wyjątkiem Tworzenie magazynu usług odzyskiwania i przypisywanie praw dostępu do innych użytkowników

| **Akcje** | |
| --- | --- |
| Microsoft.Authorization/*/read | Role odczytu i przypisania ról |
| Microsoft.Insights/alertRules/* | Tworzenie i zarządzanie nimi reguły alertów |
| Microsoft.Network/virtualNetworks/read | Sieci wirtualne odczytu |
| Microsoft.RecoveryServices/Vaults/certificates/write | Aktualizuje certyfikat poświadczeń magazynu |
| Microsoft.RecoveryServices/Vaults/extendedInformation/* | Tworzenie i zarządzanie nimi rozszerzone informacje związane z magazynu |
| Microsoft.RecoveryServices/Vaults/monitoringAlerts/*  | Odczyt alertów dla magazynu usług odzyskiwania |
| Microsoft.RecoveryServices/Vaults/monitoringConfigurations/ notificationConfiguration/read  | Konfiguracja powiadomień w magazynie usług odzyskiwania odczytu |
| Microsoft.RecoveryServices/Vaults/read | Magazyny usług odzyskiwania odczytu |
| Microsoft.RecoveryServices/Vaults/refreshContainers/read | Zarządzanie pobieranie operacji wykrywania nowo utworzony kontenerów |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/* | Tworzenie i Zarządzanie tożsamościami w zarejestrowany |
| Microsoft.RecoveryServices/vaults/replicationAlertSettings/* | Utwórz lub zaktualizuj ustawienia alertu replikacji |
| Microsoft.RecoveryServices/vaults/replicationEvents/read | Przeczytaj zdarzenia replikacji |
| Microsoft.RecoveryServices/vaults/replicationFabrics/* | Tworzenie i zarządzanie nimi sieci szkieletowej replikacji |
| Microsoft.RecoveryServices/vaults/replicationJobs/* | Tworzenie i zarządzanie nimi zadania replikacji |
| Microsoft.RecoveryServices/vaults/replicationPolicies/* | Tworzenie i zarządzanie zasadami replikacji |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/* | Tworzenie i zarządzanie planami odzyskiwania |
| Microsoft.RecoveryServices/Vaults/storageConfig/* | Tworzenie i zarządzanie nimi magazynu konfiguracji z magazynu usług odzyskiwania |
| Microsoft.RecoveryServices/Vaults/tokenInfo/read | Magazyn usług odzyskiwania odczytu informacji o tokenie |
| Microsoft.RecoveryServices/Vaults/usages/read | Szczegóły użycia odczytu z magazynu usług odzyskiwania |
| Microsoft.ResourceHealth/availabilityStatuses/read | Kondycja odczytu zasobów |
| Microsoft.Resources/deployments/* | Tworzenie i zarządzanie nimi wdrożenia grupy zasobów |
| Microsoft.Resources/subscriptions/resourceGroups/read | Odczytanie grup zasobów |
| Microsoft.Storage/storageAccounts/read | Przeczytaj kont magazynu |
| Microsoft.Support/* |Tworzenie i zarządzanie biletami pomocy technicznej |

### <a name="site-recovery-operator"></a>Operator usługi Site Recovery
Można trybu Failover i powrotu po awarii, ale nie można wykonywać inne akcje zarządzania usługi Site Recovery lub przypisać dostęp do innych użytkowników

| **Akcje** | |
| --- | --- |
| Microsoft.Authorization/*/read | Role odczytu i przypisania ról |
| Microsoft.Insights/alertRules/* | Tworzenie i zarządzanie nimi reguły alertów |
| Microsoft.Network/virtualNetworks/read | Sieci wirtualne odczytu |
| Microsoft.RecoveryServices/Vaults/extendedInformation/read | Odczyt rozszerzone informacje związane z magazynu |
| Microsoft.RecoveryServices/Vaults/monitoringAlerts/*  | Odczyt alertów dla magazynu usług odzyskiwania |
| Microsoft.RecoveryServices/Vaults/monitoringConfigurations/ notificationConfiguration/read  | Konfiguracja powiadomień w magazynie usług odzyskiwania odczytu |
| Microsoft.RecoveryServices/Vaults/read | Magazyny usług odzyskiwania odczytu |
| Microsoft.RecoveryServices/Vaults/refreshContainers/read | Zarządzanie pobieranie operacji wykrywania nowo utworzony kontenerów |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | Odczytywanie stan operacji i wynik operacji zostało przesłane |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Kontenery zarejestrowanych dla zasobu |
| Microsoft.RecoveryServices/vaults/replicationAlertSettings/read | Przeczytaj replikacji ustawienia alertu |
| Microsoft.RecoveryServices/vaults/replicationEvents/read | Przeczytaj zdarzenia replikacji |
| Microsoft.RecoveryServices/vaults/replicationFabrics/checkConsistency/action | Sprawdzanie spójności sieci szkieletowych |
| Microsoft.RecoveryServices/vaults/replicationFabrics/read | Sieci szkieletowe odczytu replikacji |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ reassociateGateway/action | Ponowne kojarzenie brama replikacji |
| Microsoft.RecoveryServices/vaults/replicationFabrics/renewcertificate/action | Odnów certyfikat sieci szkieletowej replikacji |
| Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read | Przeczytaj replikacji sieci szkieletowej |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationNetworks/replicationNetworkMappings/read | Mapowanie sieci szkieletowej replikacji odczytu |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/read | Kontenery ochrony |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectableItems/read | Pobierz listę wszystkich elementów chronionych |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/applyRecoveryPoint/działania | Zastosuj określony punkt odzyskiwania |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/ failoverCommit/action | Zatwierdzania pracy w trybie failover nie powiodło się za pośrednictwem elementu |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/ plannedFailover/action | Uruchomić planowanego trybu failover dla chronionego elementu |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/read | Pobierz listę wszystkich chronionych elementach |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read | Pobierz listę dostępnych punktów odzyskiwania |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/ repairReplication/action | Naprawa replikacji dla chronionego elementu |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/reProtect/action | Uruchom ponownie włączyć ochronę dla chronionego elementu|
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/testFailover/action | Uruchom test pracy w trybie failover chronionego elementu |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/testFailoverCleanup/działania | Uruchom czyszczenie test trybu failover |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/ unplannedFailover/action | Uruchomić nieplanowany tryb failover chronionego elementu |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/ updateMobilityService/action | Aktualizacja usługi mobilności |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectionContainerMappings/read | Mapowanie kontenera ochrony do odczytu |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationRecoveryServicesProviders/Odczyt | Dostawcy usług odzyskiwania odczytu |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationRecoveryServicesProviders/refreshProvider/działania | Odśwież dostawcę usług odzyskiwania |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationStorageClassifications/Odczyt | Klasyfikacje magazynu dla sieci szkieletowych replikacji do odczytu |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationStorageClassifications/replicationStorageClassificationMappings/read | Przeczytaj mapowania klasyfikacji magazynu |
| Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read | Odczyt zarejestrowanych informacji o programie vCenter |
| Microsoft.RecoveryServices/vaults/replicationJobs/* | Tworzenie i zarządzanie nimi zadania replikacji |
| Microsoft.RecoveryServices/vaults/replicationPolicies/read | Przeczytaj zasady replikacji |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/ failoverCommit/działania | Zatwierdź trybu failover planu odzyskiwania w trybie failover |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/ plannedFailover/action | Uruchom tryb failover planu odzyskiwania |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read | Przeczytaj planów odzyskiwania |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/reProtect/action | Uruchom ponownie włączyć ochronę planu odzyskiwania |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailover/action | Uruchomić testowy tryb failover planu odzyskiwania |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/ testFailoverCleanup/action | Uruchom czyszczenie test trybu failover planu odzyskiwania |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/ unplannedFailover/action | Uruchomić nieplanowany tryb failover planu odzyskiwania |
| Microsoft.RecoveryServices/Vaults/storageConfig/read | Konfiguracja magazynu odczytu z magazynu usług odzyskiwania |
| Microsoft.RecoveryServices/Vaults/tokenInfo/read | Magazyn usług odzyskiwania odczytu informacji o tokenie |
| Microsoft.RecoveryServices/Vaults/usages/read | Szczegóły użycia odczytu z magazynu usług odzyskiwania |
| Microsoft.ResourceHealth/availabilityStatuses/read | Kondycja odczytu zasobów |
| Microsoft.Resources/deployments/* | Tworzenie i zarządzanie nimi wdrożenia grupy zasobów |
| Microsoft.Resources/subscriptions/resourceGroups/read | Odczytanie grup zasobów |
| Microsoft.Storage/storageAccounts/read | Przeczytaj kont magazynu |
| Microsoft.Support/* | Tworzenie i zarządzanie biletami pomocy technicznej |

### <a name="site-recovery-reader"></a>Czytelnik usługi Site Recovery
Można monitorować stan usługi Site Recovery w magazynie usług odzyskiwania i podnieść biletami pomocy technicznej

| **Akcje** | |
| --- | --- |
| Microsoft.Authorization/*/read | Role odczytu i przypisania ról |
| Microsoft.RecoveryServices/Vaults/extendedInformation/read  | Odczyt rozszerzone informacje związane z magazynu |
| Microsoft.RecoveryServices/Vaults/monitoringAlerts/read  | Odczyt alertów dla magazynu usług odzyskiwania |
| Microsoft.RecoveryServices/Vaults/monitoringConfigurations/ notificationConfiguration/read  | Konfiguracja powiadomień w magazynie usług odzyskiwania odczytu |
| Microsoft.RecoveryServices/Vaults/read  | Magazyny usług odzyskiwania odczytu |
| Microsoft.RecoveryServices/Vaults/refreshContainers/read  | Zarządzanie pobieranie operacji wykrywania nowo utworzony kontenerów |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read  | Odczytywanie stan operacji i wynik operacji zostało przesłane |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/read  | Kontenery zarejestrowanych dla zasobu |
| Microsoft.RecoveryServices/vaults/replicationAlertSettings/read | Przeczytaj replikacji ustawienia alertu |
| Microsoft.RecoveryServices/vaults/replicationEvents/read  | Przeczytaj zdarzenia replikacji |
| Microsoft.RecoveryServices/vaults/replicationFabrics/read  | Sieci szkieletowe odczytu replikacji |
| Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read  | Przeczytaj replikacji sieci szkieletowej |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationNetworks/replicationNetworkMappings/read  | Mapowanie sieci szkieletowej replikacji odczytu |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/read  |  Kontenery ochrony |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectableItems/read  | Pobierz listę wszystkich elementów chronionych |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/read  | Pobierz listę wszystkich chronionych elementach |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read  | Pobierz listę dostępnych punktów odzyskiwania |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectionContainerMappings/read  | Mapowanie kontenera ochrony do odczytu |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationRecoveryServicesProviders/Odczyt  | Dostawcy usług odzyskiwania odczytu |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationStorageClassifications/Odczyt  | Klasyfikacje magazynu dla sieci szkieletowych replikacji do odczytu |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationStorageClassifications/replicationStorageClassificationMappings/read  |  Przeczytaj mapowania klasyfikacji magazynu |
| Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read  |  Odczyt zarejestrowanych informacji o programie vCenter |
| Microsoft.RecoveryServices/vaults/replicationJobs/read  |  Odczyt stanu zadań replikacji |
| Microsoft.RecoveryServices/vaults/replicationPolicies/read  |  Przeczytaj zasady replikacji |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read  |  Przeczytaj planów odzyskiwania |
| Microsoft.RecoveryServices/Vaults/storageConfig/read  |  Konfiguracja magazynu odczytu z magazynu usług odzyskiwania |
| Microsoft.RecoveryServices/Vaults/tokenInfo/read  |  Magazyn usług odzyskiwania odczytu informacji o tokenie |
| Microsoft.RecoveryServices/Vaults/usages/read  |  Szczegóły użycia odczytu z magazynu usług odzyskiwania |
| Microsoft.Support/*  |  Tworzenie i zarządzanie biletami pomocy technicznej |

### <a name="sql-db-contributor"></a>Współautor bazy danych SQL
Można zarządzać baz danych, ale nie ich zasad związanych z zabezpieczeniami

| **Akcje** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Role odczytu i przypisania roli |
| Microsoft.Insights/alertRules/* |Tworzenie i zarządzanie nimi reguły alertów |
| Microsoft.ResourceHealth/availabilityStatuses/read |Kondycja odczytu zasobów |
| Microsoft.Resources/deployments/* |Tworzenie i zarządzanie nimi wdrożenia grupy zasobów |
| Microsoft.Resources/subscriptions/resourceGroups/read |Odczytanie grup zasobów |
| Microsoft.Sql/servers/databases/* |Tworzenie i zarządzanie nimi baz danych SQL |
| Microsoft.Sql/servers/read |Przeczytaj serwerów SQL |
| Microsoft.Support/* |Tworzenie i zarządzanie biletami pomocy technicznej |

| NotActions |  |
| --- | --- |
| Microsoft.Sql/servers/databases/auditingPolicies/* |Nie można edytować zasady inspekcji |
| Microsoft.Sql/servers/databases/auditingSettings/* |Nie można edytować ustawień inspekcji |
| Microsoft.Sql/servers/databases/auditRecords/read |Nie można odczytać wyników inspekcji |
| Microsoft.Sql/servers/databases/connectionPolicies/* |Nie można edytować zasady połączeń |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* |Nie można edytować zasady maskowania danych |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* |Nie można edytować zasady alertów zabezpieczeń |
| Microsoft.Sql/servers/databases/securityMetrics/* |Nie można edytować metryki zabezpieczeń |

### <a name="sql-security-manager"></a>Menedżer zabezpieczeń SQL
Można zarządzać zasadami związanych z zabezpieczeniami serwerów SQL i baz danych

| **Akcje** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Microsoft odczytu autoryzacji |
| Microsoft.Insights/alertRules/* |Tworzenie i zarządzanie nimi Insights reguły alertów |
| Microsoft.ResourceHealth/availabilityStatuses/read |Kondycja odczytu zasobów |
| Microsoft.Resources/deployments/* |Tworzenie i zarządzanie nimi wdrożenia grupy zasobów |
| Microsoft.Resources/subscriptions/resourceGroups/read |Odczytanie grup zasobów |
| Microsoft.Sql/servers/auditingPolicies/* |Tworzenie i zarządzanie zasadami inspekcji serwera SQL |
| Microsoft.Sql/servers/auditingSettings/* |Tworzenie i zarządzanie nimi ustawienia inspekcji serwera SQL |
| Microsoft.Sql/servers/databases/auditingPolicies/* |Tworzenie i zarządzanie zasadami inspekcji bazy danych serwera SQL |
| Microsoft.Sql/servers/databases/auditingSettings/* |Tworzenie i zarządzanie nimi ustawienia inspekcji bazy danych programu SQL server |
| Microsoft.Sql/servers/databases/auditRecords/read |Odczytywanie rekordów inspekcji |
| Microsoft.Sql/servers/databases/connectionPolicies/* |Tworzenie i zarządzanie zasadami połączenia bazy danych serwera SQL |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* |Tworzenie i zarządzanie nimi danych bazy danych programu SQL server maskowania zasad |
| Microsoft.Sql/servers/databases/read |Odczyt baz danych |
| Microsoft.Sql/servers/databases/schemas/read |Schematy bazy danych serwera SQL do odczytu |
| Microsoft.Sql/servers/databases/schemas/tables/columns/read |Kolumny tabeli bazy danych serwera SQL do odczytu |
| Microsoft.Sql/servers/databases/schemas/tables/read |Tabele bazy danych serwera SQL odczytu |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* |Utwórz i Zarządzaj zasadami alertów zabezpieczeń bazy danych serwera SQL |
| Microsoft.Sql/servers/databases/securityMetrics/* |Tworzenie i zarządzanie nimi metryki zabezpieczeń bazy danych serwera SQL |
| Microsoft.Sql/servers/read |Przeczytaj serwerów SQL |
| Microsoft.Sql/servers/securityAlertPolicies/* |Utwórz i Zarządzaj zasadami alertów zabezpieczeń serwera SQL |
| Microsoft.Support/* |Tworzenie i zarządzanie biletami pomocy technicznej |

### <a name="sql-server-contributor"></a>Współautor serwera SQL Server
Można zarządzać serwerami programu SQL Server i baz danych, ale nie ich zasad związanych z zabezpieczeniami

| **Akcje** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Role odczytu i przypisania ról |
| Microsoft.Insights/alertRules/* |Tworzenie i zarządzanie nimi Insights reguły alertów |
| Microsoft.ResourceHealth/availabilityStatuses/read |Kondycja odczytu zasobów |
| Microsoft.Resources/deployments/* |Tworzenie i zarządzanie nimi wdrożenia grupy zasobów |
| Microsoft.Resources/subscriptions/resourceGroups/read |Odczytanie grup zasobów |
| Microsoft.Sql/servers/* |Tworzenie i zarządzanie serwerami programu SQL Server |
| Microsoft.Support/* |Tworzenie i zarządzanie biletami pomocy technicznej |

| NotActions |  |
| --- | --- |
| Microsoft.Sql/servers/auditingPolicies/* |Nie można edytować zasady inspekcji serwera SQL |
| Microsoft.Sql/servers/auditingSettings/* |Nie można edytować ustawienia inspekcji serwera SQL |
| Microsoft.Sql/servers/databases/auditingPolicies/* |Nie można edytować zasady inspekcji bazy danych serwera SQL |
| Microsoft.Sql/servers/databases/auditingSettings/* |Nie można edytować ustawienia inspekcji bazy danych programu SQL server |
| Microsoft.Sql/servers/databases/auditRecords/read |Nie można odczytać wyników inspekcji |
| Microsoft.Sql/servers/databases/connectionPolicies/* |Nie można edytować zasad połączenia bazy danych serwera SQL |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* |Nie można edytować danych bazy danych programu SQL server maskowania zasad |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* |Nie można edytować zasady alertów zabezpieczeń bazy danych serwera SQL |
| Microsoft.Sql/servers/databases/securityMetrics/* |Nie można edytować metryki zabezpieczeń bazy danych serwera SQL |
| Microsoft.Sql/servers/securityAlertPolicies/* |Nie można edytować zasady alertu zabezpieczeń serwera SQL |

### <a name="classic-storage-account-contributor"></a>Współautor klasycznego konta magazynu
Można zarządzać klasycznych kont magazynu

| **Akcje** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Przeczytaj autoryzacji |
| Microsoft.ClassicStorage/storageAccounts/* |Tworzenie i zarządzanie nimi kont magazynu |
| Microsoft.Insights/alertRules/* |Tworzenie i zarządzanie nimi Insights reguły alertów |
| Microsoft.ResourceHealth/availabilityStatuses/read |Kondycja odczytu zasobów |
| Microsoft.Resources/deployments/* |Tworzenie i zarządzanie nimi wdrożenia grupy zasobów |
| Microsoft.Resources/subscriptions/resourceGroups/read |Odczytanie grup zasobów |
| Microsoft.Support/* |Tworzenie i zarządzanie biletami pomocy technicznej |

### <a name="storage-account-contributor"></a>Współautor konta magazynu
Można zarządzać kontami magazynu, ale nie możesz uzyskać do nich dostępu do.

| **Akcje** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Wszystkie autoryzacji do odczytu |
| Microsoft.Insights/alertRules/* |Tworzenie i zarządzanie nimi Insights reguły alertów |
| Microsoft.Insights/diagnosticSettings/* |Zarządzanie ustawieniami diagnostycznych |
| Microsoft.ResourceHealth/availabilityStatuses/read |Kondycja odczytu zasobów |
| Microsoft.Resources/deployments/* |Tworzenie i zarządzanie nimi wdrożenia grupy zasobów |
| Microsoft.Resources/subscriptions/resourceGroups/read |Odczytanie grup zasobów |
| Microsoft.Storage/storageAccounts/* |Tworzenie i zarządzanie nimi kont magazynu |
| Microsoft.Support/* |Tworzenie i zarządzanie biletami pomocy technicznej |

### <a name="support-request-contributor"></a>Współautor żądania pomocy technicznej
Można tworzyć i zarządzanie biletami pomocy technicznej w zakresie subskrypcji

| **Akcje** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Przeczytaj autoryzacji |
| Microsoft.Support/* | Tworzenie i zarządzanie biletami pomocy technicznej |
| Microsoft.Resources/subscriptions/resourceGroups/read | Role odczytu i przypisania ról |

### <a name="user-access-administrator"></a>Administrator dostępu użytkowników
Można zarządzać dostępem użytkowników do zasobów platformy Azure

| **Akcje** |  |
| --- | --- |
| * / Odczyt |Przeczytaj zasoby wszystkich typów, z wyjątkiem kluczy tajnych. |
| Microsoft.Authorization/* |Zarządzanie autoryzacji |
| Microsoft.Support/* |Tworzenie i zarządzanie biletami pomocy technicznej |

### <a name="classic-virtual-machine-contributor"></a>Współautor klasycznej maszyny wirtualnej
Można zarządzać, ale nie z wirtualnych sieci lub magazynu konta do którego są podłączeni klasycznych maszyn wirtualnych

| **Akcje** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Przeczytaj autoryzacji |
| Microsoft.ClassicCompute/domainNames/* |Tworzenie i zarządzanie nazwami domen klasycznego obliczeń |
| Microsoft.ClassicCompute/virtualMachines/* |Tworzenie i zarządzanie maszynami wirtualnymi |
| Microsoft.ClassicNetwork/networkSecurityGroups/join/action |Dołącz do grupy zabezpieczeń sieci |
| Microsoft.ClassicNetwork/reservedIps/link/action |Link zastrzeżonych adresów IP |
| Microsoft.ClassicNetwork/reservedIps/read |Odczyt zastrzeżonych adresów IP |
| Microsoft.ClassicNetwork/virtualNetworks/join/action |Dołączanie do sieci wirtualnej |
| Microsoft.ClassicNetwork/virtualNetworks/read |Sieci wirtualne odczytu |
| Microsoft.ClassicStorage/storageAccounts/disks/read |Odczytu dla dysków z konta magazynu |
| Microsoft.ClassicStorage/storageAccounts/images/read |Przeczytaj obrazy konta magazynu |
| Microsoft.ClassicStorage/storageAccounts/listKeys/action |Wyświetl klucze konta magazynu |
| Microsoft.ClassicStorage/storageAccounts/read |Przeczytaj klasycznych kont magazynu |
| Microsoft.Insights/alertRules/* |Tworzenie i zarządzanie nimi Insights reguły alertów |
| Microsoft.ResourceHealth/availabilityStatuses/read |Kondycja odczytu zasobów |
| Microsoft.Resources/deployments/* |Tworzenie i zarządzanie nimi wdrożenia grupy zasobów |
| Microsoft.Resources/subscriptions/resourceGroups/read |Odczytanie grup zasobów |
| Microsoft.Support/* |Tworzenie i zarządzanie biletami pomocy technicznej |

### <a name="virtual-machine-contributor"></a>Współautor maszyny wirtualnej
Można zarządzać, ale nie wirtualnych sieci lub magazynu konto do którego są podłączeni maszyny wirtualne

| **Akcje** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Przeczytaj autoryzacji |
| Microsoft.Compute/availabilitySets/* |Utwórz i Zarządzaj zestawami dostępności obliczeń |
| Microsoft.Compute/locations/* |Tworzenie i zarządzanie nimi lokalizacje obliczeń |
| Microsoft.Compute/virtualMachines/* |Tworzenie i zarządzanie maszynami wirtualnymi |
| Microsoft.Compute/virtualMachineScaleSets/* |Tworzenie i zarządzanie nimi zestawy skalowania maszyny wirtualnej |
| Microsoft.Insights/alertRules/* |Tworzenie i zarządzanie nimi Insights reguły alertów |
| Microsoft.Network/applicationGateways/backendAddressPools/join/action |Dołącz do pul adresów zaplecza bramy aplikacji sieci |
| Microsoft.Network/loadBalancers/backendAddressPools/join/action |Dołącz do puli adresów zaplecza modułu równoważenia obciążenia |
| Microsoft.Network/loadBalancers/inboundNatPools/join/action |Dołącz do modułu równoważenia obciążenia ruchu przychodzącego dla pul translacji NAT |
| Microsoft.Network/loadBalancers/inboundNatRules/join/action |Dołącz do modułu równoważenia obciążenia reguły NAT dla ruchu przychodzącego |
| Microsoft.Network/loadBalancers/read |Odczyt usługi równoważenia obciążenia |
| Microsoft.Network/locations/* |Tworzenie i zarządzanie nimi lokalizacji sieciowych |
| Microsoft.Network/networkInterfaces/* |Tworzenie i zarządzanie nimi interfejsy sieciowe |
| Microsoft.Network/networkSecurityGroups/join/action |Dołącz do grupy zabezpieczeń sieci |
| Microsoft.Network/networkSecurityGroups/read |Odczytanie grup zabezpieczeń sieci |
| Microsoft.Network/publicIPAddresses/join/action |Dołącz do sieci publicznych adresów IP |
| Microsoft.Network/publicIPAddresses/read |Przeczytaj sieci publicznych adresów IP |
| Microsoft.Network/virtualNetworks/read |Sieci wirtualne odczytu |
| Microsoft.Network/virtualNetworks/subnets/join/action |Dołącz do podsieci sieci wirtualnej |
| Microsoft.ResourceHealth/availabilityStatuses/read |Kondycja odczytu zasobów |
| Microsoft.Resources/deployments/* |Tworzenie i zarządzanie nimi wdrożenia grupy zasobów |
| Microsoft.Resources/subscriptions/resourceGroups/read |Odczytanie grup zasobów |
| Microsoft.Storage/storageAccounts/listKeys/action |Wyświetl klucze konta magazynu |
| Microsoft.Storage/storageAccounts/read |Przeczytaj kont magazynu |
| Microsoft.Support/* |Tworzenie i zarządzanie biletami pomocy technicznej |

### <a name="classic-network-contributor"></a>Współautor klasycznej sieci
Można zarządzać klasycznych sieci wirtualnych i zarezerwowane adresy IP

| **Akcje** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Przeczytaj autoryzacji |
| Microsoft.ClassicNetwork/* |Tworzenie i zarządzanie nimi klasycznych sieci |
| Microsoft.Insights/alertRules/* |Tworzenie i zarządzanie nimi Insights reguły alertów |
| Microsoft.ResourceHealth/availabilityStatuses/read |Kondycja odczytu zasobów |
| Microsoft.Resources/deployments/* |Tworzenie i zarządzanie nimi wdrożenia grupy zasobów |
| Microsoft.Resources/subscriptions/resourceGroups/read |Odczytanie grup zasobów |
| Microsoft.Support/* |Tworzenie i zarządzanie biletami pomocy technicznej |

### <a name="web-plan-contributor"></a>Współautor planów sieci Web
Można zarządzać planami sieci web

| **Akcje** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Przeczytaj autoryzacji |
| Microsoft.Insights/alertRules/* |Tworzenie i zarządzanie nimi Insights reguły alertów |
| Microsoft.ResourceHealth/availabilityStatuses/read |Kondycja odczytu zasobów |
| Microsoft.Resources/deployments/* |Tworzenie i zarządzanie nimi wdrożenia grupy zasobów |
| Microsoft.Resources/subscriptions/resourceGroups/read |Odczytanie grup zasobów |
| Microsoft.Support/* |Tworzenie i zarządzanie biletami pomocy technicznej |
| Microsoft.Web/serverFarms/* |Tworzenie i zarządzanie nimi farmy serwerów |

### <a name="website-contributor"></a>Współautor witryny sieci Web
Można zarządzać witryn sieci Web, ale nie planów sieci web, z którymi są połączone

| **Akcje** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Przeczytaj autoryzacji |
| Microsoft.Insights/alertRules/* |Tworzenie i zarządzanie nimi Insights reguły alertów |
| Microsoft.Insights/components/* |Tworzenie i zarządzanie nimi składniki Insights |
| Microsoft.ResourceHealth/availabilityStatuses/read |Kondycja odczytu zasobów |
| Microsoft.Resources/deployments/* |Tworzenie i zarządzanie nimi wdrożenia grupy zasobów |
| Microsoft.Resources/subscriptions/resourceGroups/read |Odczytanie grup zasobów |
| Microsoft.Support/* |Tworzenie i zarządzanie biletami pomocy technicznej |
| Microsoft.Web/certificates/* |Tworzenie i zarządzanie certyfikatami witryny sieci Web |
| Microsoft.Web/listSitesAssignedToHostName/read |Witryn odczytu przypisanych do nazwy hosta |
| Microsoft.Web/serverFarms/join/action |Dołącz do farmy serwerów |
| Microsoft.Web/serverFarms/read |Przeczytaj farmy serwerów |
| Microsoft.Web/sites/* |Tworzenie i zarządzanie nimi witryn sieci Web (Tworzenie witryny wymaga także uprawnienia do zapisu do skojarzonego planu usługi App Service) |

## <a name="see-also"></a>Zobacz także
* [Kontrola dostępu oparta na rolach](role-based-access-control-configure.md): rozpoczynanie pracy z RBAC w portalu Azure.
* [Role niestandardowe w Azure RBAC](role-based-access-control-custom-roles.md): Dowiedz się, jak tworzyć role niestandardowe, aby spełniały Twoje potrzeby dostępu.
* [Tworzenie raportu historii zmian dostępu](role-based-access-control-access-change-history-report.md): informacje o zmieniania przypisań ról w RBAC.
* [Rozwiązywanie kontroli dostępu opartej na rolach](role-based-access-control-troubleshooting.md): Pobierz sugestie dotyczące rozwiązywania typowych problemów.
* [Uprawnienia w Centrum zabezpieczeń Azure](../security-center/security-center-permissions.md)
