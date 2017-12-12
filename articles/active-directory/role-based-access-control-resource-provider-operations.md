---
title: "Operacje dostawca usługi Azure Resource Manager | Dokumentacja firmy Microsoft"
description: "Szczegóły operacje dostępne dla dostawców zasobów Microsoft Azure Resource Manager"
services: active-directory
documentationcenter: 
author: jboeshart
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/28/2017
ms.author: jaboes
ms.openlocfilehash: 27880402d377701448d095a1295ece875729cd67
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-resource-manager-resource-provider-operations"></a>Operacje Menedżera zasobów dostawcy zasobów platformy Azure

Ten dokument zawiera listę czynności dla każdego dostawcy zasobów programu Microsoft Azure Resource Manager. Mogą one używane w niestandardowych ról do zapewniają szczegółowe uprawnienia kontroli dostępu opartej na rolach (RBAC) do zasobów na platformie Azure. Należy pamiętać, nie jest to kompleksowe i operacje może dodać lub usunąć każdy dostawca jest aktualizowana. Operacja ciągów wykonaj format `Microsoft.<ProviderName>/<ChildResourceType>/<action>`. 

> [!NOTE]
> Lista bieżących i wszechstronne użyj `Get-AzureRmProviderOperation` (w programie PowerShell) lub `az provider operation list` (w wiersza polecenia platformy Azure w wersji 2) do operacji listy dostawców zasobów platformy Azure.

## <a name="microsoftadhybridhealthservice"></a>Microsoft.ADHybridHealthService

| Operacja | Opis |
|---|---|
|/ configuration/działania|Aktualizacje konfiguracji dzierżawy.|
|/ services/działania|Aktualizuje wystąpienie usługi, w dzierżawie.|
|/ configuration/zapisu|Tworzy konfiguracji dzierżawy.|
|/Configuration/Read|Odczytuje konfiguracji dzierżawy.|
|/ services/zapisu|Tworzy wystąpienie usługi w dzierżawie.|
|/Services/Read|Odczytuje wystąpień usługi w dzierżawie.|
|/Services/DELETE|Usuwa wystąpienie usługi, w dzierżawie.|
|/Services/servicemembers/Action|Tworzy wystąpienie elementu członkowskiego usługi w usłudze.|
|/Services/servicemembers/Read|Odczytuje element członkowski wystąpienia usługi w usłudze.|
|/Services/servicemembers/DELETE|Usuwa wystąpienie elementu członkowskiego usługi w usłudze.|
|/Services/servicemembers/Alerts/Read|Odczytuje alerty dla elementu członkowskiego usługi.|
|/Services/Alerts/Read|Odczytuje alerty dla usługi.|
|/Services/Alerts/Read|Odczytuje alerty dla usługi.|

## <a name="microsoftadvisor"></a>Microsoft.Advisor

| Operacja | Opis |
|---|---|
|/ generateRecommendations/działania|Generuje zalecenia|
|/ pominięć/działania|Tworzy aktualizacje pominięć|
|/ register/działania|Rejestruje subskrypcję Microsoft Advisor|
|/generateRecommendations/Read|Pobiera Generowanie zaleceń stanu|
|/recommendations/Read|Odczytuje zalecenia|
|/suppressions/Read|Pobiera pominięć|
|/suppressions/DELETE|Usuwa pominięć|

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices

| Operacja | Opis |
|---|---|
|/Servers/Read|Pobiera informacje o określonym Analysis Server.|
|/ serwery/zapisu|Tworzy lub aktualizuje określony Analysis Server.|
|/Servers/DELETE|Usuwa Analysis Server.|
|/Servers/suspend/Action|Wstrzymuje Analysis Server.|
|/Servers/resume/Action|Wznawia Analysis Server.|
|/ serwery/checkNameAvailability<br>Action|Sprawdza, czy dany serwer analiz nazwa jest prawidłowa, a nie korzystać.|

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement

| Operacja | Opis |
|---|---|
|/ checkNameAvailability/działania|Sprawdza, czy pod warunkiem, że nazwa usługi jest niedostępna|
|/ register/działania|Zarejestruj subskrypcję dostawcy zasobów Microsoft.ApiManagement|
|/ unregister/działania|Wyrejestrowanie subskrypcji dla dostawcy zasobów Microsoft.ApiManagement|
|/ service/zapisu|Utwórz nowe wystąpienie interfejsu API usługi zarządzania|
|/Service/Read|Odczytać metadanych dla wystąpienia interfejsu API usługi zarządzania|
|/Service/DELETE|Usuń wystąpienia usługi interfejsu API zarządzania|
|/Service/updatehostname/Action|Konfiguracja, zaktualizować lub usunąć nazwy domeny niestandardowej dla usługi interfejsu API zarządzania|
|/Service/uploadcertificate/Action|Przekaż certyfikat protokołu SSL dla interfejsu API usługi zarządzania|
|/Service/Backup/Action|Usługi zarządzania interfejsu API tworzenia kopii zapasowej do określonego kontenera użytkownika podane konto magazynu|
|/Service/Restore/Action|Przywracanie usługi interfejsu API zarządzania z kontenera określonej w użytkownika podane konto magazynu|
|/Service/managedeployments/Action|Zmień jednostki SKU/jednostki, dodawania i usuwania wdrożenia regionalnych interfejsu API usługi zarządzania|
|/Service/getssotoken/Action|Pobiera token logowania jednokrotnego, który może służyć do logowania do portalu starsza wersja usługi zarządzania interfejsu API z uprawnieniami administratora|
|/Service/applynetworkconfigurationupdates/Action|Aktualizuje zasoby Microsoft.ApiManagement systemem w sieci wirtualnej w celu zastosowania zaktualizowanych ustawień sieciowych.|
|/Service/operationresults/Read|Pobiera bieżący stan długotrwałej operacji|
|/Service/networkStatus/Read|Pobiera stan dostępu do sieci, zasobów.|
|/Service/loggers/Read|Pobierz listę rejestratorów lub pobrać szczegółów rejestratora|
|/Service/loggers/Write|Dodać nowego rejestratora lub zaktualizować istniejący szczegóły rejestratora|
|/Service/loggers/DELETE|Usuń istniejące rejestratora|
|/Service/Users/Read|Pobierz listę zarejestrowanych użytkowników lub pobrać szczegółów konta użytkownika|
|/Service/Users/Write|Zarejestrować nowego użytkownika lub szczegóły konta aktualizacji istniejącego użytkownika|
|/Service/Users/DELETE|Usuwanie konta użytkownika|
|/Service/Users/generateSsoUrl/Action|Generowania adresu URL logowania jednokrotnego. Adres URL można uzyskać dostęp do portalu administratora|
|/Service/Users/Subscriptions/Read|Pobierz listę subskrypcji użytkownika|
|/Service/Users/keys/Read|Pobierz listę kluczy użytkownika|
|/Service/Users/groups/Read|Pobierz listę grup użytkowników|
|/Service/tenant/operationResults/Read|Pobierz listę wyników operacji lub uzyskanie wyniku operacji|
|/Service/tenant/Policy/Read|Uzyskiwanie konfiguracji zasad dla dzierżawcy|
|/Service/tenant/Policy/Write|Ustawienia zasad konfiguracji dla dzierżawcy|
|/Service/tenant/Policy/DELETE|Usuń konfigurację zasad dla dzierżawcy|
|/Service/tenant/Configuration/Save/Action|Tworzy zatwierdzenia z konfiguracji migawek dla określonej gałęzi w repozytorium|
|/Service/tenant/Configuration/Deploy/Action|Uruchamia zadanie wdrażania można zastosować zmian z gałęzi git określony w konfiguracji w bazie danych.|
|/Service/tenant/Configuration/Validate/Action|Weryfikuje zmian z gałęzi git określony|
|/Service/tenant/Configuration/operationResults/Read|Pobierz listę wyników operacji lub uzyskanie wyniku operacji|
|/Service/tenant/Configuration/syncState/Read|Pobierz stan ostatniej synchronizacji git|
|/Service/tenant/Access/Read|Uzyskaj dostęp do dzierżawy szczegółowe informacje o|
|/Service/tenant/Access/Write|Szczegółowe informacje dotyczące dzierżawy dostępu do informacji o aktualizacji|
|/Service/tenant/Access/regeneratePrimaryKey/Action|Ponownie wygenerować podstawowy klucz dostępu|
|/Service/tenant/Access/regenerateSecondaryKey/Action|Wygeneruj ponownie pomocniczy klucz dostępu|
|/Service/identityProviders/Read|Pobierz listę dostawców tożsamości lub Uzyskaj szczegóły dostawcy tożsamości|
|/Service/identityProviders/Write|Utwórz nowe szczegóły dostawcy tożsamości lub aktualizację istniejącego dostawcy tożsamości|
|/Service/identityProviders/DELETE|Usuń istniejące dostawcy tożsamości|
|/Service/Subscriptions/Read|Pobiera listę subskrypcji produktu lub uzyskać szczegółów subskrypcji produktu|
|/Service/Subscriptions/Write|Subskrypcja istniejącego użytkownika do istniejącego produktu lub zaktualizować istniejący Szczegóły subskrypcji. Ta operacja może służyć do odnowienia subskrypcji|
|/Service/Subscriptions/DELETE|Usuń subskrypcję. Ta operacja może służyć do usuwania subskrypcji|
|/Service/Subscriptions/regeneratePrimaryKey/Action|Ponowne generowanie klucza podstawowego subskrypcji|
|/Service/Subscriptions/regenerateSecondaryKey/Action|Ponowne wygenerowanie klucza pomocniczego subskrypcji|
|/Service/backends/Read|Pobierz listę zapleczy lub uzyskać szczegółowe informacje o wewnętrznej bazy danych|
|/Service/backends/Write|Dodać nowego zaplecza lub zaktualizować istniejący szczegóły wewnętrznej bazy danych|
|/Service/backends/DELETE|Usuń istniejące wewnętrznej bazy danych|
|/Service/APIs/Read|Pobierz listę wszystkich zarejestrowanych szczegółów interfejsów API lub Get interfejsu API|
|/Service/APIs/Write|Utwórz nowy interfejs API lub zaktualizuj istniejącą szczegóły interfejsu API|
|/Service/APIs/DELETE|Usuń istniejące interfejsu API|
|/Service/APIs/Policy/Read|Uzyskiwanie szczegółów konfiguracji zasad interfejsu API|
|/Service/APIs/Policy/Write|Ustaw szczegóły konfiguracji zasad interfejsu API|
|/Service/APIs/Policy/DELETE|Usuń konfigurację zasad z interfejsu API|
|/Service/APIs/Operations/Read|Pobierz listę istniejących operacji interfejsu API lub uzyskać szczegółowe informacje o operacji interfejsu API|
|/Service/APIs/Operations/Write|Tworzenie nowej operacji interfejsu API lub zaktualizować istniejący operacji interfejsu API|
|/Service/APIs/Operations/DELETE|Usuń istniejące operacji interfejsu API|
|/Service/APIs/Operations/Policy/Read|Uzyskiwanie szczegółów konfiguracji zasad dla operacji|
|/Service/APIs/Operations/Policy/Write|Ustaw szczegóły konfiguracji zasad dla operacji|
|/Service/APIs/Operations/Policy/DELETE|Usuń konfigurację zasad z operacji|
|/Service/Products/Read|Pobierz listę produktów lub uzyskać szczegółowe informacje o produkcie|
|/Service/Products/Write|Tworzenie nowego produktu lub zaktualizować istniejące informacje szczegółowe|
|/Service/Products/DELETE|Usuń istniejące produkt|
|/Service/Products/Subscriptions/Read|Pobierz listę subskrypcji produktu|
|/Service/Products/APIs/Read|Pobierz listę dodane do istniejących interfejsów API|
|/Service/Products/APIs/Write|Dodaj istniejący interfejs API do istniejącego produktu|
|/Service/Products/APIs/DELETE|Usuń istniejące interfejsu API z istniejącego produktu|
|/Service/Products/Policy/Read|Uzyskiwanie konfiguracji zasad istniejącego produktu|
|/Service/Products/Policy/Write|Ustaw konfigurację zasad dla istniejącego produktu|
|/Service/Products/Policy/DELETE|Usuń zasady konfiguracji z istniejącego produktu|
|/Service/Products/groups/Read|Pobierz listę grup developer skojarzonych z produktu|
|/Service/Products/groups/Write|Skojarz istniejąca grupa deweloperów z istniejącego produktu|
|/Service/Products/groups/DELETE|Usuń skojarzenie istniejąca grupa deweloperów z istniejącego produktu|
|/Service/openidConnectProviders/Read|Pobierz listę dostawców OpenID Connect lub pobranie szczegółów OpenID Connect dostawcy|
|/Service/openidConnectProviders/Write|Utwórz nowe szczegóły OpenID Connect dostawcy lub aktualizację istniejącego dostawcy uwierzytelniania OpenID Connect|
|/Service/openidConnectProviders/DELETE|Usuń istniejące OpenID Connect dostawcy|
|/Service/Certificates/Read|Pobierz listę certyfikatów lub Pobierz szczegóły certyfikatu|
|/Service/Certificates/Write|Dodaj nowy certyfikat|
|/Service/Certificates/DELETE|Usuń istniejący certyfikat|
|/Service/Properties/Read|Pobiera listę właściwości wszystkich lub pobiera szczegóły określonej właściwości|
|/Service/Properties/Write|Tworzy nową właściwość lub aktualizuje wartość dla określonej właściwości|
|/Service/Properties/DELETE|Usuwa istniejące właściwości|
|/Service/groups/Read|Pobierz listę grup lub pobiera szczegóły grupy|
|/Service/groups/Write|Utwórz nową grupę lub zaktualizować istniejący szczegóły grupy|
|/Service/groups/DELETE|Usuń istniejącą grupę|
|/Service/groups/Users/Read|Pobierz listę grupy użytkowników|
|/Service/groups/Users/Write|Dodaj istniejącego użytkownika do istniejącej grupy|
|/Service/groups/Users/DELETE|Usuwanie istniejącego użytkownika z istniejącej grupy|
|/Service/authorizationServers/Read|Pobierz listę serwerów autoryzacji lub pobrać szczegółów serwera autoryzacji|
|/Service/authorizationServers/Write|Tworzenie nowego serwera autoryzacji lub szczegóły aktualizacji istniejącego serwera autoryzacji|
|/Service/authorizationServers/DELETE|Usuń istniejący serwer autoryzacji|
|/Service/Reports/bySubscription/Read|Pobierz raportu w subskrypcji.|
|/Service/Reports/byRequest/Read|Pobrać żądań danych raportowania|
|/Service/Reports/byOperation/Read|Raport w operacji|
|/Service/Reports/byGeo/Read|Raport w regionie geograficznym|
|/Service/Reports/byUser/Read|Pobierz raport agregowana przez deweloperów.|
|/Service/Reports/byTime/Read|Raport w okresach czasu|
|/Service/Reports/byApi/Read|Raport agregowana przez interfejsy API|
|/Service/Reports/byProduct/Read|Pobierz raport w produktów.|

## <a name="microsoftauthorization"></a>Microsoft.Authorization

| Operacja | Opis |
|---|---|
|/ elevateAccess/działania|Przyznaje osobie wywołującej uprawnienia administratora dostępu użytkowników w zakresie dzierżawy|
|/classicAdministrators/Read|Odczytuje administratorów dla subskrypcji.|
|/ classicAdministrators/zapisu|Dodaje lub modyfikuje administratora dla subskrypcji.|
|/classicAdministrators/DELETE|Usuwa administratora z subskrypcji.|
|/Locks/Read|Pobiera blokady w podanym zakresie.|
|/ blokady zapisu|Dodaje blokady w podanym zakresie.|
|/Locks/DELETE|Usuwa blokady w podanym zakresie.|
|/policyAssignments/Read|Pobierz informacje o przypisaniu zasad.|
|/ policyAssignments/zapisu|Utwórz przypisanie zasad w podanym zakresie.|
|/policyAssignments/DELETE|Usuń przypisanie zasad w podanym zakresie.|
|/permissions/Read|Wyświetla wszystkie uprawnienia obiektu wywołującego w danym zakresie.|
|/roleDefinitions/Read|Pobiera informacje o definicji roli.|
|/ roleDefinitions/zapisu|Tworzy lub aktualizuje niestandardową definicję roli z określonymi uprawnieniami i zakresami możliwymi do przypisania.|
|/roleDefinitions/DELETE|Usuwa określoną niestandardową definicję roli.|
|/providerOperations/Read|Pobiera operacje dla wszystkich dostawców zasobów do użycia w definicjach ról.|
|/policyDefinitions/Read|Pobierz informacje o definicji zasad.|
|/ policyDefinitions/zapisu|Utwórz niestandardową definicję zasad.|
|/policyDefinitions/DELETE|Usuń definicję zasad.|
|/roleAssignments/Read|Pobiera informacje o przypisaniu roli.|
|/ roleAssignments/zapisu|Tworzy przypisanie roli w podanym zakresie.|
|/roleAssignments/DELETE|Usuwa przypisanie roli w podanym zakresie.|

## <a name="microsoftautomation"></a>Microsoft.Automation

| Operacja | Opis |
|---|---|
|/automationAccounts/Read|Pobiera konto usługi Automatyzacja Azure|
|/ automationAccounts/zapisu|Tworzy lub aktualizuje konto usługi Automatyzacja Azure|
|/automationAccounts/DELETE|Usuwa konto usługi Automatyzacja Azure|
|/automationAccounts/Configurations/readContent/Action|Pobiera zawartość DSC automatyzacji Azure|
|/automationAccounts/hybridRunbookWorkerGroups/Read|Odczytuje zasoby hybrydowego procesu roboczego elementu Runbook|
|/automationAccounts/hybridRunbookWorkerGroups/DELETE|Usuwa zasoby hybrydowego procesu roboczego elementu Runbook|
|/automationAccounts/jobSchedules/Read|Pobiera harmonogram zadań usługi Automatyzacja Azure|
|/automationAccounts/jobSchedules/Write|Tworzy harmonogram zadań usługi Automatyzacja Azure|
|/automationAccounts/jobSchedules/DELETE|Usuwa harmonogram zadań usługi Automatyzacja Azure|
|/automationAccounts/connectionTypes/Read|Pobiera zasób typu połączenia usługi Automatyzacja Azure|
|/automationAccounts/connectionTypes/Write|Tworzy zasób typu połączenia usługi Automatyzacja Azure|
|/automationAccounts/connectionTypes/DELETE|Usuwa zasób typu połączenia usługi Automatyzacja Azure|
|/automationAccounts/Modules/Read|Pobiera moduł usługi Automatyzacja Azure|
|/automationAccounts/Modules/Write|Tworzy lub aktualizuje moduł usługi Automatyzacja Azure|
|/automationAccounts/Modules/DELETE|Usuwa moduł usługi Automatyzacja Azure|
|/automationAccounts/Credentials/Read|Pobiera zasób poświadczenia usługi Automatyzacja Azure|
|/automationAccounts/Credentials/Write|Tworzy lub aktualizuje zasób poświadczenia usługi Automatyzacja Azure|
|/automationAccounts/Credentials/DELETE|Usuwa zasób poświadczenia usługi Automatyzacja Azure|
|/automationAccounts/Certificates/Read|Pobiera zasób certyfikatu usługi Automatyzacja Azure|
|/automationAccounts/Certificates/Write|Tworzy lub aktualizuje zasób certyfikatu usługi Automatyzacja Azure|
|/automationAccounts/Certificates/DELETE|Usuwa zasób certyfikatu usługi Automatyzacja Azure|
|/automationAccounts/Schedules/Read|Pobiera zasób harmonogramu usługi Automatyzacja Azure|
|/automationAccounts/Schedules/Write|Tworzy lub aktualizuje zasób harmonogramu usługi Automatyzacja Azure|
|/automationAccounts/Schedules/DELETE|Usuwa zasób harmonogramu usługi Automatyzacja Azure|
|/automationAccounts/Jobs/Read|Pobiera zadanie usługi Automatyzacja Azure|
|/automationAccounts/Jobs/Write|Tworzy zadanie usługi Automatyzacja Azure|
|/automationAccounts/Jobs/Stop/Action|Zatrzymuje zadanie usługi Automatyzacja Azure|
|/automationAccounts/Jobs/suspend/Action|Wstrzymuje zadanie usługi Automatyzacja Azure|
|/automationAccounts/Jobs/resume/Action|Wznawia zadanie usługi Automatyzacja Azure|
|/automationAccounts/Jobs/runbookContent/Action|Pobiera zawartość elementu runbook usługi Automatyzacja Azure w czasie wykonywania zadania|
|/automationAccounts/Jobs/Output/Action|Pobiera dane wyjściowe zadania|
|/automationAccounts/Jobs/Read|Pobiera zadanie usługi Automatyzacja Azure|
|/automationAccounts/Jobs/Write|Tworzy zadanie usługi Automatyzacja Azure|
|/automationAccounts/Jobs/Stop/Action|Zatrzymuje zadanie usługi Automatyzacja Azure|
|/automationAccounts/Jobs/suspend/Action|Wstrzymuje zadanie usługi Automatyzacja Azure|
|/automationAccounts/Jobs/resume/Action|Wznawia zadanie usługi Automatyzacja Azure|
|/automationAccounts/Jobs/streams/Read|Pobiera strumień zadań usługi Automatyzacja Azure|
|/automationAccounts/Connections/Read|Pobiera zasób połączenia usługi Automatyzacja Azure|
|/automationAccounts/Connections/Write|Tworzy lub aktualizuje zasób połączenia usługi Automatyzacja Azure|
|/automationAccounts/Connections/DELETE|Usuwa zasób połączenia usługi Automatyzacja Azure|
|/automationAccounts/variables/Read|Odczytuje zasób zmiennej usługi Automatyzacja Azure|
|/automationAccounts/variables/Write|Tworzy lub aktualizuje zasób zmiennej usługi Automatyzacja Azure|
|/automationAccounts/variables/DELETE|Usuwa zasób zmiennej usługi Automatyzacja Azure|
|/automationAccounts/runbooks/readContent/Action|Pobiera zawartość elementu runbook usługi Automatyzacja Azure|
|/automationAccounts/runbooks/Read|Pobiera element runbook usługi Automatyzacja Azure|
|/automationAccounts/runbooks/Write|Tworzy lub aktualizuje element runbook usługi Automatyzacja Azure|
|/automationAccounts/runbooks/DELETE|Usuwa element runbook usługi Automatyzacja Azure|
|/automationAccounts/runbooks/draft/readContent/Action|Pobiera zawartość wersji roboczej elementu runbook usługi Automatyzacja Azure|
|/automationAccounts/runbooks/draft/writeContent/Action|Tworzy zawartość wersji roboczej elementu runbook usługi Automatyzacja Azure|
|/automationAccounts/runbooks/draft/Read|Pobiera wersję roboczą elementu runbook usługi Automatyzacja Azure|
|/automationAccounts/runbooks/draft/publish/Action|Publikuje wersję roboczą elementu runbook usługi Automatyzacja Azure|
|/automationAccounts/runbooks/draft/undoEdit/Action|Cofnij operacje edycji próbnej wersji elementu runbook usługi Automatyzacja Azure|
|/automationAccounts/runbooks/draft/testJob/Read|Pobiera zadanie testowe wersji roboczej elementu runbook usługi Automatyzacja Azure|
|/automationAccounts/runbooks/draft/testJob/Write|Tworzy zadanie testowe wersji roboczej elementu runbook usługi Automatyzacja Azure|
|/automationAccounts/runbooks/draft/testJob/Stop/Action|Zatrzymuje zadanie testowe wersji roboczej elementu runbook usługi Automatyzacja Azure|
|/automationAccounts/runbooks/draft/testJob/suspend/Action|Wstrzymuje zadanie testowe wersji roboczej elementu runbook usługi Automatyzacja Azure|
|/automationAccounts/runbooks/draft/testJob/resume/Action|Wznawia zadanie testowe wersji roboczej elementu runbook usługi Automatyzacja Azure|
|/automationAccounts/webhooks/Read|Odczytuje element webhook usługi Automatyzacja Azure|
|/automationAccounts/webhooks/Write|Tworzy lub aktualizuje element webhook usługi Automatyzacja Azure|
|/automationAccounts/webhooks/DELETE|Usuwa element webhook usługi Automatyzacja Azure |
|/automationAccounts/webhooks/generateUri/Action|Generuje identyfikator URI dla elementu webhook usługi Automatyzacja Azure|

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory

Ten dostawca nie jest pełną dostawcy ARM i nie zapewnia żadnych operacji ARM.

## <a name="microsoftbatch"></a>Microsoft.Batch

| Operacja | Opis |
|---|---|
|/ register/działania|Rejestruje subskrypcję dostawcy zasobów partii i umożliwia tworzenie konta usługi partia zadań|
|/ batchAccounts/zapisu|Tworzy nowe konto wsadowe lub aktualizuje istniejące konto usługi partia zadań|
|/batchAccounts/Read|Lista kont usługi partia zadań lub pobiera właściwości konta usługi partia zadań|
|/batchAccounts/DELETE|Usuwa konto usługi partia zadań|
|/batchAccounts/listkeys/Action|Wyświetla uzyskać dostęp do kluczy dla konta usługi partia zadań|
|/batchAccounts/regeneratekeys/Action|Odtwarza uzyskać dostęp do kluczy dla konta usługi partia zadań|
|/batchAccounts/syncAutoStorageKeys/Action|Synchronizuje klucze dostępu dla konta magazynu automatycznie skonfigurowany dla konta usługi partia zadań|
|/batchAccounts/Applications/Read|Wyświetla listę aplikacji lub pobiera właściwości aplikacji|
|/batchAccounts/Applications/Write|Tworzy nową aplikację lub aktualizuje istniejącą aplikację|
|/batchAccounts/Applications/DELETE|Usuwa aplikacji|
|/batchAccounts/Applications/Versions/Read|Pobiera właściwości pakietu aplikacji|
|/batchAccounts/Applications/Versions/Write|Tworzy nowy pakiet aplikacji lub aktualizuje istniejący pakiet aplikacji|
|/batchAccounts/Applications/Versions/Activate/Action|Aktywuje pakietu aplikacji|
|/batchAccounts/Applications/Versions/DELETE|Usuwa pakiet aplikacji|
|/Locations/Quotas/Read|Pobiera limity przydziału partii określonej subskrypcji w wybranym regionie Azure|

## <a name="microsoftbilling"></a>Microsoft.Billing

| Operacja | Opis |
|---|---|
|/Invoices/Read|Wyświetla dostępne faktury|

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps

| Operacja | Opis |
|---|---|
|/ mapApis/odczytu|Operacja odczytu|
|/ mapApis/zapisu|Operacja zapisu|
|/ mapApis/Delete|Operacja usuwania|
|/mapApis/regenerateKey/Action|Generuje ponownie klucz|
|/mapApis/listSecrets/Action|Wyświetl klucze tajne|
|/mapApis/listSingleSignOnToken/Action|Odczyt tokenu autoryzacji logowania jednokrotnego dla zasobu|
|Operacje/odczytu|Opis operacji.|

## <a name="microsoftcache"></a>Microsoft.Cache

| Operacja | Opis |
|---|---|
|/ checknameavailability/działania|Sprawdza, czy nazwa jest dostępna do użycia z nowej pamięci podręcznej Redis|
|/ register/działania|Rejestruje subskrypcję dostawcy zasobów "Microsoft.Cache"|
|/ unregister/działania|Wyrejestrowuje "Microsoft.Cache" dostawcy zasobów z subskrypcją|
|/ redis/zapisu|Zmodyfikuj ustawienia i konfigurację w portalu zarządzania pamięci podręcznej Redis|
|/redis/Read|Wyświetl ustawienia i konfigurację pamięci podręcznej Redis w portalu zarządzania|
|/redis/DELETE|Usuń całą pamięć podręczną Redis|
|/redis/listKeys/Action|Wyświetl wartości kluczy dostępu pamięci podręcznej Redis w portalu zarządzania|
|/redis/regenerateKey/Action|Zmień wartość kluczy dostępu pamięci podręcznej Redis w portalu zarządzania|
|/redis/Import/Action|Zaimportuj dane w podanym formacie z wielu obiektów blob do usługi Redis|
|/redis/Export/Action|Wyeksportuj dane usługi Redis do określonych prefiksem obiektów blob magazynu w podanym formacie|
|/redis/forceReboot/Action|Wymuś ponowny rozruch wystąpienia pamięci podręcznej, nawet z utratą danych.|
|/redis/Stop/Action|Zatrzymanie wystąpienia pamięci podręcznej.|
|/redis/Start/Action|Uruchom wystąpienie pamięci podręcznej.|
|/redis/metricDefinitions/Read|Pobiera dostępne metryki pamięci podręcznej Redis|
|/redis/firewallRules/Read|Pobieranie reguł zapory IP pamięci podręcznej Redis|
|/redis/firewallRules/Write|Edytowanie reguły zapory IP pamięci podręcznej Redis|
|/redis/firewallRules/DELETE|Usuwanie reguły zapory IP pamięci podręcznej Redis|
|/redis/listUpgradeNotifications/Read|Lista najnowszych powiadomień o uaktualnieniach dla dzierżawy pamięci podręcznej.|
|/redis/linkedservers/Read|Pobierz połączone serwery skojarzone z pamięci podręcznej redis.|
|/redis/linkedservers/Write|Dodaj serwer połączony do pamięci podręcznej Redis|
|/redis/linkedservers/DELETE|Usuń serwer połączony z pamięci podręcznej Redis|
|/redis/patchSchedules/Read|Pobiera harmonogram stosowania poprawek pamięci podręcznej Redis|
|/redis/patchSchedules/Write|Modyfikuj harmonogram stosowania poprawek pamięci podręcznej Redis|
|/redis/patchSchedules/DELETE|Usuń harmonogram poprawki pamięci podręcznej Redis|

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration

| Operacja | Opis |
|---|---|
|/ provisionGlobalAppServicePrincipalInUserTenant/działania|Zainicjuj obsługę nazwy głównej usługi dla nazwy głównej usługi aplikacji|
|/ validateCertificateRegistrationInformation/działania|Sprawdzanie poprawności obiektu zakupu certyfikatu bez przesyłania|
|/ register/działania|Rejestrowanie dostawcy zasobów Certificates firmy Microsoft dla subskrypcji|
|/ certificateOrders/zapisu|Dodaj nowy certificateOrder lub zaktualizuj istniejącą|
|/ certificateOrders/Delete|Usuń istniejące AppServiceCertificate|
|/ certificateOrders/odczytu|Pobierz listę CertificateOrders|
|/certificateOrders/reissue/Action|Wydaj ponownie istniejących certificateorder|
|/certificateOrders/renew/Action|Odnów istniejący certificateorder|
|/certificateOrders/retrieveCertificateActions/Action|Pobierz listę akcji certyfikatu|
|/certificateOrders/retrieveEmailHistory/Action|Pobieranie certyfikatu historii wiadomości e-mail|
|/certificateOrders/resendEmail/Action|Ponowne wysyłanie wiadomości e-mail certyfikatu|
|/certificateOrders/verifyDomainOwnership/Action|Zweryfikuj prawo własności do domeny|
|/certificateOrders/resendRequestEmails/Action|Ponowne wysyłanie wiadomości e-mail żądania do innego adresu e-mail|
|/certificateOrders/resendRequestEmails/Action|Pobrać zamknięcia lokacji w celu wystawiony certyfikat usługi aplikacji|
|/certificateOrders/Certificates/Write|Dodaj nowy certyfikat lub zaktualizuj istniejącą|
|/certificateOrders/Certificates/DELETE|Usuń istniejący certyfikat|
|/certificateOrders/Certificates/Read|Pobierz listę certyfikatów|

## <a name="microsoftclassiccompute"></a>Obszar Microsoft.ClassicCompute

| Operacja | Opis |
|---|---|
|/ register/działania|Zarejestruj się w klasycznym obliczeń|
|/ checkDomainNameAvailability/działania|Sprawdza dostępność danej nazwy domeny.|
|/ moveSubscriptionResources/działania|Przenieś wszystkie zasoby klasyczne do innej subskrypcji.|
|/ validateSubscriptionMoveAvailability/działania|Sprawdź dostępność subskrypcji dla operacji przenoszenia klasycznego.|
|/operatingSystemFamilies/Read|Zawiera listę rodzin systemu operacyjnego gościa dostępnych w Microsoft Azure, a także wymieniono wersje systemu operacyjnego, dostępne dla każdego f
|/Capabilities/Read|Pokazuje możliwości|
|/operatingSystems/Read|Wyświetla listę wersji systemu operacyjnego gościa, które są obecnie dostępne w systemie Microsoft Azure.|
|/resourceTypes/skus/Read|Pobiera listę jednostek Sku dla obsługiwanych typów zasobów.|
|/domainNames/Read|Zwraca nazwy domen dla zasobów.|
|/ domainNames/zapisu|Dodaje lub modyfikuje nazwy domen dla zasobów.|
|/domainNames/DELETE|Usuwa nazwy domen dla zasobów.|
|/domainNames/swap/Action|Zamienia miejsce przejściowe z miejscem produkcyjnym.|
|/domainNames/serviceCertificates/Read|Zwraca używane certyfikaty usługi.|
|/domainNames/serviceCertificates/Write|Dodaje lub modyfikuje używane certyfikaty usługi.|
|/domainNames/serviceCertificates/DELETE|Usuń używane certyfikaty usługi.|
|/domainNames/serviceCertificates/operationStatuses/Read|Odczytuje stan operacji dla domeny certyfikatów usługi nazw.|
|/domainNames/Capabilities/Read|Pokazuje możliwości nazwy domeny|
|/domainNames/Extensions/Read|Zwraca rozszerzenia nazwy domeny.|
|/domainNames/Extensions/Write|Dodanie rozszerzenia nazwy domeny.|
|/domainNames/Extensions/DELETE|Usuń rozszerzenia nazwy domeny.|
|/domainNames/Extensions/operationStatuses/Read|Odczytuje stan operacji dla domeny rozszerzenia nazw.|
|/domainNames/Active/Write|Ustawia nazwę aktywnej domeny.|
|/domainNames/Slots/Read|Pokazuje miejsc wdrożenia.|
|/domainNames/Slots/Write|Tworzy lub aktualizuje wdrożenie.|
|/domainNames/Slots/DELETE|Usuwa miejsce danego wdrożenia.|
|/domainNames/Slots/Start/Action|Uruchamia miejsca wdrożenia.|
|/domainNames/Slots/Stop/Action|Wstrzymuje miejsca wdrożenia.|
|/domainNames/Slots/operationStatuses/Read|Odczytuje stan operacji dla domeny gniazd nazw.|
|/domainNames/Slots/Roles/Read|Pobiera rolę dla miejsca wdrożenia.|
|/domainNames/Slots/Roles/extensionReferences/Read|Zwraca odwołanie do rozszerzenia dla roli miejsca wdrożenia.|
|/domainNames/Slots/Roles/extensionReferences/Write|Dodaje lub modyfikuje odwołanie do rozszerzenia dla roli miejsca wdrożenia.|
|/domainNames/Slots/Roles/extensionReferences/DELETE|Usuń odwołanie do rozszerzenia dla roli miejsca wdrożenia.|
|/domainNames/Slots/Roles/extensionReferences/operationStatuses/Read|Odczytuje stan operacji dla odwołań rozszerzenia ról miejsc nazwy domeny.|
|/domainNames/Slots/Roles/roleInstances/Read|Pobierz wystąpienia roli.|
|/domainNames/Slots/Roles/roleInstances/restart/Action|Ponowne uruchomienie wystąpienia roli.|
|/domainNames/Slots/Roles/roleInstances/reimage/Action|Reimages wystąpienia roli.|
|/domainNames/Slots/Roles/roleInstances/operationStatuses/Read|Odczytuje stan operacji dla wystąpień roli ról gniazd nazw domeny.|
|/domainNames/Slots/State/Start/Write|Zmienia stan miejsca wdrożenia na zatrzymane.|
|/domainNames/Slots/State/Stop/Write|Zmienia stan miejsca wdrożenia na uruchomione.|
|/domainNames/Slots/upgradeDomain/Write|Uaktualnianie jest pomijane domeny.|
|/domainNames/internalLoadBalancers/Read|Pobiera wewnętrzne usługi równoważenia obciążenia.|
|/domainNames/internalLoadBalancers/Write|Tworzy nową usługę równoważenia obciążenia wewnętrznego.|
|/domainNames/internalLoadBalancers/DELETE|Usuń nową usługę równoważenia obciążenia wewnętrznego.|
|/domainNames/internalLoadBalancers/operationStatuses/Read|Odczytuje stan operacji dla wewnętrzne moduły równoważenia obciążenia nazw domen.|
|/domainNames/loadBalancedEndpointSets/Read|Pokazuje zestawy punktów końcowych ze zrównoważonym obciążeniem|
|/domainNames/loadBalancedEndpointSets/operationStatuses/Read|Odczytuje stan operacji dla nazw domen zestawy punktów końcowych ze zrównoważonym obciążeniem.|
|/domainNames/availabilitySets/Read|Pokaż zbiór dostępności dla zasobu.|
|/Quotas/Read|Pobiera przydział dla subskrypcji.|
|/virtualMachines/Read|Pobiera listę maszyn wirtualnych.|
|/ virtualMachines/zapisu|Dodaje lub modyfikuje maszyny wirtualne.|
|/virtualMachines/DELETE|Usuwa maszyn wirtualnych.|
|/virtualMachines/Start/Action|Uruchom maszynę wirtualną.|
|/virtualMachines/redeploy/Action|Wdraża ponownie maszyny wirtualnej.|
|/virtualMachines/restart/Action|Ponowne uruchomienie maszyn wirtualnych.|
|/virtualMachines/Stop/Action|Zatrzymuje maszynę wirtualną.|
|/virtualMachines/shutdown/Action|Zamknij maszynę wirtualną.|
|/virtualMachines/attachDisk/Action|Dołączenie dysku danych do maszyny wirtualnej.|
|/virtualMachines/detachDisk/Action|Odłącza dysk z danymi od maszyny wirtualnej.|
|/virtualMachines/downloadRemoteDesktopConnectionFile/Action|Pobiera plik RDP dla maszyny wirtualnej.|
|/virtualMachines/Networkinterface /<br>associatedNetworkSecurityGroups/Odczyt|Pobiera sieciową grupę zabezpieczeń skojarzoną z interfejsu sieciowego.|
|/virtualMachines/Networkinterface /<br>associatedNetworkSecurityGroups/zapisu|Dodaje grupę zabezpieczeń sieci skojarzonych z interfejsem sieciowym.|
|/virtualMachines/Networkinterface /<br>associatedNetworkSecurityGroups/usuwania|Usuwa sieciową grupę zabezpieczeń skojarzoną z interfejsu sieciowego.|
|/virtualMachines/Networkinterface /<br>associatedNetworkSecurityGroups operationStatuses/odczytu|Odczytuje stan operacji dla maszyn wirtualnych skojarzonych grup zabezpieczeń sieci.|
|/virtualMachines/Providers/Microsoft.Insights/metricDefinitions/Read|Pobiera definicje metryk.|
|/virtualMachines/Providers/Microsoft.Insights/diagnosticSettings/Read|Pobierz ustawienia diagnostyki.|
|/virtualMachines/Providers/Microsoft.Insights/diagnosticSettings/Write|Dodaje lub modyfikuje ustawienia diagnostyczne.|
|/virtualMachines/Metrics/Read|Pobiera metryki.|
|/virtualMachines/operationStatuses/Read|Odczytuje stan operacji dla maszyn wirtualnych.|
|/virtualMachines/Extensions/Read|Pobiera rozszerzenie maszyny wirtualnej.|
|/virtualMachines/Extensions/Write|Umieszcza rozszerzenie maszyny wirtualnej.|
|/virtualMachines/Extensions/operationStatuses/Read|Odczytuje stan operacji dla rozszerzeń maszyny wirtualnej.|
|/virtualMachines/asyncOperations/Read|Pobiera dostępne operacje asynchroniczne|
|/virtualMachines/Disks/Read|Pobiera listę dysków z danymi|
|/virtualMachines/associatedNetworkSecurityGroups/Read|Pobiera sieciową grupę zabezpieczeń skojarzoną z maszyną wirtualną.|
|/virtualMachines/associatedNetworkSecurityGroups/Write|Dodaje sieciową grupę zabezpieczeń skojarzoną z maszyną wirtualną.|
|/virtualMachines/associatedNetworkSecurityGroups/DELETE|Usuwa sieciową grupę zabezpieczeń skojarzoną z maszyną wirtualną.|
|/virtualMachines/associatedNetworkSecurityGroups/operationStatuses/Read|Odczytuje stan operacji dla maszyn wirtualnych skojarzonych grup zabezpieczeń sieci.|

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork

| Operacja | Opis |
|---|---|
|/ register/działania|Zarejestruj się w klasycznej sieci|
|/gatewaySupportedDevices/Read|Pobiera listę obsługiwanych urządzeń.|
|/reservedIps/Read|Pobiera zastrzeżonych adresów IP|
|/ zastrzeżonych adresów IP/zapisu|Dodaj nowy zastrzeżony adres Ip|
|/reservedIps/DELETE|Usuwanie zastrzeżonego adresu Ip.|
|/reservedIps/Link/Action|Link zastrzeżonego adresu Ip|
|/reservedIps/JOIN/Action|Dołącz do zastrzeżonego adresu Ip|
|/reservedIps/operationStatuses/Read|Odczytuje stan operacji dla zarezerwowanych adresów IP.|
|/virtualNetworks/Read|Pobierz sieci wirtualnej.|
|/ virtualNetworks/zapisu|Dodaj nową sieć wirtualną.|
|/virtualNetworks/DELETE|Usuwa sieci wirtualnej.|
|/virtualNetworks/peer/Action|Równorzędnymi użytkownikami sieci wirtualnej z inną siecią wirtualną.|
|/virtualNetworks/JOIN/Action|Tworzy sprzężenie sieci wirtualnej.|
|/virtualNetworks/checkIPAddressAvailability/Action|Sprawdza dostępność danego adresu IP w sieci wirtualnej.|
|/virtualNetworks/Capabilities/Read|Pokazuje możliwości|
|/virtualNetworks/podsieci /<br>associatedNetworkSecurityGroups/Odczyt|Pobiera sieciową grupę zabezpieczeń skojarzoną z podsiecią.|
|/virtualNetworks/podsieci /<br>associatedNetworkSecurityGroups/zapisu|Dodaje sieciową grupę zabezpieczeń skojarzoną z podsiecią.|
|/virtualNetworks/podsieci /<br>associatedNetworkSecurityGroups/usuwania|Usuwa sieciową grupę zabezpieczeń skojarzoną z podsiecią.|
|/virtualNetworks/podsieci /<br>associatedNetworkSecurityGroups operationStatuses/odczytu|Odczytuje stan operacji dla grupy zabezpieczeń sieci skojarzonej z podsiecią podsieci sieci wirtualnej.|
|/virtualNetworks/operationStatuses/Read|Odczytuje stan operacji dla sieci wirtualnych.|
|/virtualNetworks/Gateways/Read|Pobiera bramy sieci wirtualnej.|
|/virtualNetworks/Gateways/Write|Dodaje bramy sieci wirtualnej.|
|/virtualNetworks/Gateways/DELETE|Usuwa bramę sieci wirtualnej.|
|/virtualNetworks/Gateways/startDiagnostics/Action|Uruchamia diagnostykę bramy sieci wirtualnej.|
|/virtualNetworks/Gateways/stopDiagnostics/Action|Zatrzymuje diagnostykę bramy sieci wirtualnej.|
|/virtualNetworks/Gateways/downloadDiagnostics/Action|Pobiera diagnostyki bramy.|
|/virtualNetworks/Gateways/listCircuitServiceKey/Action|Pobiera klucz usługi obwodu.|
|/virtualNetworks/Gateways/downloadDeviceConfigurationScript/Action|Pobiera skrypt konfiguracji urządzenia.|
|/virtualNetworks/Gateways/listPackage/Action|Wyświetla pakiet bramy sieci wirtualnej.|
|/virtualNetworks/Gateways/operationStatuses/Read|Odczytuje stan operacji dla bram sieci wirtualnych.|
|/virtualNetworks/Gateways/Packages/Read|Pobiera pakiet bramy sieci wirtualnej.|
|/virtualNetworks/Gateways/Connections/Read|Pobiera listę połączeń.|
|/virtualNetworks/Gateways/Connections/Connect/Action|Umożliwia nawiązanie połączenia bramy do lokacji.|
|/virtualNetworks/Gateways/Connections/Disconnect/Action|Rozłącza połączenie bramy do lokacji.|
|/virtualNetworks/Gateways/Connections/test/Action|Testuje połączenie bramy do lokacji.|
|/virtualNetworks/Gateways/clientRevokedCertificates/Read|Przeczytaj cofnięte certyfikaty klienta.|
|/virtualNetworks/Gateways/clientRevokedCertificates/Write|Odwołuje certyfikatu klienta.|
|/virtualNetworks/Gateways/clientRevokedCertificates/DELETE|Unrevokes certyfikat klienta.|
|/virtualNetworks/Gateways/clientRootCertificates/Read|Znajdź certyfikaty główne klienta.|
|/virtualNetworks/Gateways/clientRootCertificates/Write|Przekazuje nowy certyfikat główny klienta.|
|/virtualNetworks/Gateways/clientRootCertificates/DELETE|Usuwa certyfikat klienta bramy sieci wirtualnej.|
|/virtualNetworks/Gateways/clientRootCertificates/Download/Action|Pobiera certyfikat przez odcisk palca.|
|/virtualNetworks/Gateways/clientRootCertificates/listPackage/Action|Wyświetla pakiet certyfikatu bramy sieci wirtualnej.|
|/networkSecurityGroups/Read|Pobiera sieciową grupę zabezpieczeń.|
|/ Networksecuritygroup/zapisu|Dodaje nową sieciową grupę zabezpieczeń.|
|/networkSecurityGroups/DELETE|Usuwa sieciową grupę zabezpieczeń.|
|/networkSecurityGroups/operationStatuses/Read|Odczytuje stan operacji dla grupy zabezpieczeń sieci.|
|/networkSecurityGroups/securityRules/Read|Pobiera reguły zabezpieczeń.|
|/networkSecurityGroups/securityRules/Write|Dodaje lub aktualizuje regułę zabezpieczeń.|
|/networkSecurityGroups/securityRules/DELETE|Usuwa regułę zabezpieczeń.|
|/networkSecurityGroups/securityRules/operationStatuses/Read|Odczytuje stan operacji dla reguł zabezpieczeń grupy zabezpieczeń sieci.|
|/Quotas/Read|Pobiera przydział dla subskrypcji.|

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage

| Operacja | Opis |
|---|---|
|/ register/działania|Zarejestruj się w klasycznym magazynu|
|/ checkStorageAccountAvailability/działania|Sprawdza dostępność konta magazynu.|
|/Capabilities/Read|Pokazuje możliwości|
|/publicImages/Read|Pobiera obraz publicznego maszyny wirtualnej.|
|/images/Read|Zwraca obraz.|
|/storageAccounts/Read|Zwraca konto magazynu z podanym kontem.|
|/ storageAccounts/zapisu|Dodaje nowe konto magazynu.|
|/storageAccounts/DELETE|Usunięcie konta magazynu.|
|/storageAccounts/listKeys/Action|Wyświetla klucze dostępu dla konta magazynu.|
|/storageAccounts/regenerateKey/Action|Generuje ponownie istniejące klucze dostępu dla konta magazynu.|
|/storageAccounts/operationStatuses/Read|Odczytuje stan operacji dla zasobu.|
|/storageAccounts/images/Read|Zwraca obraz konta magazynu.|
|/storageAccounts/images/DELETE|Usuwa określony obraz konta magazynu.|
|/storageAccounts/Disks/Read|Zwraca dysku konta magazynu.|
|/storageAccounts/Disks/Write|Dodaje dysk konta magazynu.|
|/storageAccounts/Disks/DELETE|Usuwa dysk magazynu danego konta.|
|/storageAccounts/Disks/operationStatuses/Read|Odczytuje stan operacji dla zasobu.|
|/storageAccounts/osImages/Read|Zwraca obraz systemu operacyjnego konta magazynu.|
|/storageAccounts/osImages/DELETE|Usuwa określony obraz konta magazynu systemu operacyjnego.|
|/storageAccounts/Services/Read|Pobierz dostępne usługi.|
|/storageAccounts/Services/metricDefinitions/Read|Pobiera definicje metryk.|
|/storageAccounts/Services/Metrics/Read|Pobiera metryki.|
|/storageAccounts/Services/diagnosticSettings/Read|Pobierz ustawienia diagnostyki.|
|/storageAccounts/Services/diagnosticSettings/Write|Dodaje lub modyfikuje ustawienia diagnostyczne.|
|/Disks/Read|Zwraca dysku konta magazynu.|
|/osImages/Read|Zwraca obraz systemu operacyjnego.|
|/Quotas/Read|Pobiera przydział dla subskrypcji.|

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices

| Operacja | Opis |
|---|---|
|/accounts/Read|Odczytuje konta interfejsów API.|
|/ kont/zapisu|Zapisuje konta interfejsów API.|
|/accounts/DELETE|Usuwa konta interfejsów API|
|/accounts/listKeys/Action|Wylicz klucze|
|/accounts/regenerateKey/Action|Wygeneruj ponownie klucz|
|/accounts/skus/Read|Odczytuje jednostki SKU dostępne dla istniejącego zasobu.|
|/accounts/Usages/Read|Pobierz użycie przydziału dla istniejącego zasobu.|
|Operacje/odczytu|Opis operacji.|

## <a name="microsoftcommerce"></a>Microsoft.Commerce

| Operacja | Opis |
|---|---|
|RateCard/odczytu|Zwraca oferują danych, metadane Miernik/zasobów i szybkości dla danej subskrypcji.|
|UsageAggregates/odczytu|Pobiera Microsoft Azure — użytek przez subskrypcję. Wynik zawiera agreguje dane dotyczące użycia, subskrypcji i zasobu powiązane informacje, w określonym czasie.|

## <a name="microsoftcompute"></a>Microsoft.Compute

| Operacja | Opis |
|---|---|
|/ register/działania|Rejestruje subskrypcję za pomocą dostawcy zasobów Microsoft.Compute|
|/restorePointCollections/Read|Pobierz właściwości kolekcji punktu przywracania|
|/ restorePointCollections/zapisu|Tworzy nową kolekcję punkt przywracania, lub aktualizuje istniejący zestaw|
|/restorePointCollections/DELETE|Usuwa przywracania punktu kolekcji i zawiera punkty przywracania|
|/restorePointCollections/restorePoints/Read|Pobierz właściwości punktu przywracania|
|/restorePointCollections/restorePoints/Write|Tworzy nowy punkt przywracania|
|/restorePointCollections/restorePoints/DELETE|Usuwa punkt przywracania|
|/restorePointCollections/restorePoints/retrieveSasUris/Action|Pobierz właściwości punktu przywracania oraz identyfikatorów URI SAS obiektu blob|
|/virtualMachineScaleSets/Read|Pobiera właściwości zestawu skali maszyny wirtualnej|
|/ virtualMachineScaleSets/zapisu|Tworzy nowy zestaw skali maszyny wirtualnej lub aktualizuje istniejący zestaw|
|/virtualMachineScaleSets/DELETE|Usuwa zestaw skali maszyny wirtualnej|
|/virtualMachineScaleSets/Start/Action|Uruchamia wystąpienia zestawu skali maszyny wirtualnej|
|/virtualMachineScaleSets/powerOff/Action|Wyłącza wystąpienia zestawu skali maszyny wirtualnej|
|/virtualMachineScaleSets/restart/Action|Uruchamia ponownie wystąpienia zestawu skali maszyny wirtualnej|
|/virtualMachineScaleSets/deallocate/Action|Wyłącza i zwalnia zasoby obliczeniowe dla wystąpień zestawu skali maszyny wirtualnej |
|/virtualMachineScaleSets/manualUpgrade/Action|Ręcznie aktualizuje wystąpienia przy użyciu najnowszego modelu zestawu skali maszyny wirtualnej|
|/virtualMachineScaleSets/Scale/Action|Skalowanie w / set Skaluj w poziomie liczby wystąpień istniejących skalowania maszyny wirtualnej|
|/virtualMachineScaleSets/instanceView/Read|Pobiera widok wystąpienia zestawu skalowania maszyn wirtualnych|
|/virtualMachineScaleSets/skus/Read|Wyświetla prawidłowe jednostki SKU dla istniejącego zestawu skalowania maszyn wirtualnych|
|/virtualMachineScaleSets/virtualMachines/Read|Pobiera właściwości maszyny wirtualnej w zestawie skalowania maszyn wirtualnych|
|/virtualMachineScaleSets/virtualMachines/DELETE|Usuń konkretną maszynę wirtualną z zestawu skalowania maszyn wirtualnych.|
|/virtualMachineScaleSets/virtualMachines/Start/Action|Uruchamia wystąpienie maszyny wirtualnej w zestawie skalowania maszyn wirtualnych.|
|/virtualMachineScaleSets/virtualMachines/powerOff/Action|Wyłącza wystąpienie maszyny wirtualnej w zestawie skalowania maszyn wirtualnych.|
|/virtualMachineScaleSets/virtualMachines/restart/Action|Uruchamia ponownie wystąpienie maszyny wirtualnej w zestawie skalowania maszyn wirtualnych.|
|/virtualMachineScaleSets/virtualMachines/deallocate/Action|Wyłącza i zwalnia zasoby obliczeniowe dla maszyny wirtualnej w zestawie skalowania maszyn wirtualnych.|
|/virtualMachineScaleSets/virtualMachines/instanceView/Read|Pobiera widok wystąpienia maszyny wirtualnej w zestawie skalowania maszyn wirtualnych.|
|/images/Read|Pobierz właściwości obrazu|
|/ obrazów/zapisu|Tworzy nowy obraz lub aktualizuje istniejący zestaw|
|/images/DELETE|Usuwa obraz|
|/Operations/Read|Wyświetla operacje dostępne dla dostawcy zasobów Microsoft.Compute|
|/Disks/Read|Pobierz właściwości dysku|
|/ dysków/zapisu|Tworzy nowy dysk lub aktualizuje istniejący|
|/Disks/DELETE|Usuwa dysk|
|/Disks/beginGetAccess/Action|Pobierz identyfikator URI sygnatury dostępu współdzielonego dysku na potrzeby uzyskiwania dostępu do obiektu blob|
|/Disks/endGetAccess/Action|Wycofaj identyfikator URI sygnatury dostępu współdzielonego dysku|
|/snapshots/Read|Pobierz właściwości migawki|
|/ migawek/zapisu|Utwórz nową migawkę lub zaktualizuj istniejącą|
|/snapshots/DELETE|Usuń migawkę|
|/availabilitySets/Read|Pobierz właściwości zestawu dostępności|
|/ availabilitySets/zapisu|Tworzy nowy zestaw dostępności lub aktualizuje istniejący zestaw|
|/availabilitySets/DELETE|Usuwa zestaw dostępności|
|/availabilitySets/vmSizes/Read|Wyświetl dostępne rozmiary na potrzeby tworzenia lub aktualizowania maszyny wirtualnej w zestawie dostępności|
|/virtualMachines/Read|Pobierz właściwości maszyny wirtualnej|
|/ virtualMachines/zapisu|Tworzy nową maszynę wirtualną lub aktualizuje istniejącą maszynę wirtualną|
|/virtualMachines/DELETE|Usuwa maszynę wirtualną|
|/virtualMachines/Start/Action|Uruchamia maszynę wirtualną|
|/virtualMachines/powerOff/Action|Wyłącza maszynę wirtualną. Należy pamiętać, że maszyna wirtualna nadal będą naliczane opłaty.|
|/virtualMachines/redeploy/Action|Ponownie wdraża maszynę wirtualną|
|/virtualMachines/restart/Action|Uruchamia ponownie maszynę wirtualną|
|/virtualMachines/deallocate/Action|Wyłącza maszynę wirtualną i zwalnia zasoby obliczeniowe|
|/virtualMachines/generalize/Action|Ustawia stan maszyny wirtualnej „Uogólniono” i przygotowuje maszynę wirtualną do przechwycenia|
|/virtualMachines/Capture/Action|Przechwytuje maszynę wirtualną przez skopiowanie wirtualnych dysków twardych oraz generuje szablon umożliwiający tworzenie podobnych maszyn wirtualnych|
|/virtualMachines/convertToManagedDisks/Action|Konwertuje obiekt blob na podstawie dyski maszyny wirtualnej do zarządzanych dysków|
|/virtualMachines/vmSizes/Read|Wyświetla dostępne rozmiary, do których można zaktualizować maszynę wirtualną|
|/virtualMachines/instanceView/Read|Pobiera szczegółowy stan środowiska uruchomieniowego dla maszyny wirtualnej i jej zasobów|
|/virtualMachines/Extensions/Read|Pobierz właściwości rozszerzenia maszyny wirtualnej|
|/virtualMachines/Extensions/Write|Tworzy nowe rozszerzenie maszyny wirtualnej lub aktualizuje istniejące rozszerzenie|
|/virtualMachines/Extensions/DELETE|Usuwa rozszerzenie maszyny wirtualnej|
|/Locations/vmSizes/Read|Wyświetla listę dostępnych rozmiarów maszyny wirtualnej w danej lokalizacji|
|/Locations/Usages/Read|Pobiera limity usług i bieżące dane użycia zasobów obliczeniowych subskrypcji w danej lokalizacji|
|/Locations/Operations/Read|Pobiera stan operacji asynchronicznej|

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

| Operacja | Opis |
|---|---|
|/ register/działania|Rejestruje subskrypcję dostawcy zasobów rejestru kontenera i umożliwia tworzenie kontenera rejestrów.|
|/checknameavailability/Read|Sprawdza się, że nazwa rejestru jest prawidłowa i nie jest używany.|
|/registries/Read|Zwraca listę rejestrów kontenera lub pobiera właściwości dla określonego kontenera rejestru.|
|/ rejestrów/zapisu|Tworzy rejestru kontenera z określonymi parametrami lub aktualizuje właściwości bądź tagi rejestru określonego kontenera.|
|/registries/DELETE|Usuwa istniejące rejestru kontenera.|
|/registries/listCredentials/Action|Wyświetla listę poświadczenia logowania dla określonego kontenera rejestru.|
|/registries/regenerateCredential/Action|Generuje ponownie poświadczenia logowania dla określonego kontenera rejestru.|

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService

| Operacja | Opis |
|---|---|
|/containerServices/Subscriptions/Read|Pobierz określone usługi kontenerów na podstawie subskrypcji|
|/containerServices/resourceGroups/Read|Pobierz określone usługi kontenerów na podstawie grupy zasobów|
|/containerServices/resourceGroups/ContainerServiceName/Read|Pobiera określoną usługę kontenera|
|/containerServices/resourceGroups/ContainerServiceName/Write|Umieść lub zaktualizuj określoną usługę kontenera|
|/containerServices/resourceGroups/ContainerServiceName/DELETE|Usuwa określoną usługę kontenera|

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator

| Operacja | Opis |
|---|---|
|/ updateCommunicationPreference/działania|Preferencji komunikacji aktualizacji|
|/ listCommunicationPreference/działania|Lista komunikacji preferencji|
|/Applications/Read|Operacja odczytu|
|/ applications/zapisu|Operacja zapisu|
|/ applications/zapisu|Operacja zapisu|
|/Applications/DELETE|Operacja usuwania|
|/Applications/listSecrets/Action|Wyświetl klucze tajne|
|/Applications/listSingleSignOnToken/Action|Odczyt jednokrotnego tokenów|
|/Operations/Read|operacje odczytu|

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights

| Operacja | Opis |
|---|---|
|/hubs/Read|Przeczytaj dowolnego Centrum Insights Azure klienta|
|/ hubs/zapisu|Utwórz lub zaktualizuj Centrum Insights dowolnego klienta usługi Azure|
|/hubs/DELETE|Usunąć wszelkie Centrum Insights klienta platformy Azure|
|/hubs/Providers/Microsoft.Insights/metricDefinitions/Read|Pobiera dostępne metryki dla zasobu|
|/hubs/Providers/Microsoft.Insights/diagnosticSettings/Read|Pobiera ustawienie diagnostyczne dla zasobu|
|/hubs/Providers/Microsoft.Insights/diagnosticSettings/Write|Tworzy lub aktualizuje ustawienie diagnostyczne dla zasobu|
|/hubs/Providers/Microsoft.Insights/logDefinitions/Read|Pobiera dostępne dzienniki dla zasobu|
|/hubs/authorizationPolicies/Read|Przeczytaj zasady podpisu dostępu udostępniony żadnych informacji szczegółowych Azure klienta|
|/hubs/authorizationPolicies/Write|Utwórz lub zaktualizuj dowolne zasady sygnatury dostępu współdzielonego Insights klienta Azure|
|/hubs/authorizationPolicies/DELETE|Usuń dowolne zasady sygnatury dostępu współdzielonego Insights klienta platformy Azure|
|/hubs/authorizationPolicies/regeneratePrimaryKey/Action|Ponowne wygenerowanie klucza podstawowego Azure klienta Insights zasad dostępu współużytkowanego podpisu|
|/hubs/authorizationPolicies/regenerateSecondaryKey/Action|Ponowne wygenerowanie klucza pomocniczego Azure klienta Insights zasad dostępu współużytkowanego podpisu|
|/hubs/Profiles/Read|Przeczytaj dowolnego profilu Insights klienta platformy Azure|
|/hubs/Profiles/Write|Zapis dowolnego profilu Insights klienta platformy Azure|
|/hubs/KPI/Read|Przeczytaj dowolnego klienta Azure Insights kluczowy wskaźnik wydajności|
|/hubs/KPI/Write|Utwórz lub zaktualizuj dowolnego klienta Azure Insights kluczowy wskaźnik wydajności|
|/hubs/KPI/DELETE|Usuń wszelkie klienta Azure Insights kluczowy wskaźnik wydajności|
|/hubs/views/Read|Przeczytaj dowolnym widoku App Insights klienta usługi Azure|
|/hubs/views/Write|Utwórz lub zaktualizuj dowolnym widoku App Insights klienta usługi Azure|
|/hubs/views/DELETE|Usuń wszelkie widok aplikacji Insights klienta usługi Azure|
|/hubs/Interactions/Read|Przeczytaj interakcji Insights klienta platformy Azure|
|/hubs/Interactions/Write|Utwórz lub zaktualizuj interakcji Insights klienta platformy Azure|
|/hubs/roleAssignments/Read|Przeczytaj przydziału Rbac Insights klienta platformy Azure|
|/hubs/roleAssignments/Write|Utwórz lub zaktualizuj wszystkie przypisania Rbac Insights klienta Azure|
|/hubs/roleAssignments/DELETE|Usuń wszelkie przydział Rbac Insights klienta Azure|
|/hubs/Connectors/Read|Każdy łącznik Insights Azure klienta do odczytu|
|/hubs/Connectors/Write|Utwórz lub zaktualizuj każdy łącznik Insights klienta platformy Azure|
|/hubs/Connectors/DELETE|Usunięcie łącznika Insights dowolnego klienta platformy Azure|
|/hubs/Connectors/Mappings/Read|Przeczytaj żadnego mapowania łącznika Insights klienta platformy Azure|
|/hubs/Connectors/Mappings/Write|Utwórz lub zaktualizuj żadnego mapowania łącznika Insights klienta platformy Azure|
|/hubs/Connectors/Mappings/DELETE|Usunąć wszystkie mapowania łącznika Insights klienta Azure|

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog

| Operacja | Opis |
|---|---|
|/ checkNameAvailability/działania|Służy do sprawdzania dostępności nazwy katalogu dla dzierżawcy.|
|/Catalogs/Read|Pobierz właściwości dla katalogu lub katalogów w ramach subskrypcji lub grupy zasobów.|
|/ wykazów/zapisu|Tworzy katalog lub aktualizuje znaczniki i właściwości dla katalogu.|
|/Catalogs/DELETE|Usuwa katalogu.|

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory

| Operacja | Opis |
|---|---|
|/datafactories/Read|Odczytuje fabryki danych.|
|/ datafactories/zapisu|Utwórz lub zaktualizuj fabryki danych|
|/datafactories/DELETE|Usuwa fabryki danych.|
|/datafactories/datapipelines/Read|Odczytuje potoku.|
|/datafactories/datapipelines/DELETE|Usuwa potoku.|
|/datafactories/datapipelines/pause/Action|Potok pauzy.|
|/datafactories/datapipelines/resume/Action|Wznawia potoku.|
|/datafactories/datapipelines/Update/Action|Potoku aktualizacji.|
|/datafactories/datapipelines/Write|Utwórz lub zaktualizuj potoku|
|/datafactories/linkedServices/Read|Odczytuje połączonej usługi.|
|/datafactories/linkedServices/DELETE|Usuwa połączonej usługi.|
|/datafactories/linkedServices/Write|Tworzenie lub aktualizacja połączonej usługi|
|/datafactories/Tables/Read|Odczytuje tabeli.|
|/datafactories/Tables/DELETE|Usuwa tabelę.|
|/datafactories/Tables/Write|Utwórz lub zaktualizuj tabelę|

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics

| Operacja | Opis |
|---|---|
|/accounts/Read|Pobierz informacje o koncie DataLakeAnalytics.|
|/ kont/zapisu|Utwórz lub zaktualizuj konto DataLakeAnalytics.|
|/accounts/DELETE|Usuń konto DataLakeAnalytics.|
|/accounts/firewallRules/Read|Pobierz informacje dotyczące reguły zapory.|
|/accounts/firewallRules/Write|Utwórz lub zaktualizuj regułę zapory.|
|/accounts/firewallRules/DELETE|Usuwanie reguły zapory.|
|/accounts/storageAccounts/Read|Pobierz połączonego konta magazynu dla konta DataLakeAnalytics.|
|/accounts/storageAccounts/Write|Konto magazynu należy połączyć konto DataLakeAnalytics.|
|/accounts/storageAccounts/DELETE|Odłączanie konta magazynu z konta DataLakeAnalytics.|
|/accounts/storageAccounts/containers/Read|Pobierz kontenery w ramach konta magazynu|
|/accounts/storageAccounts/containers/listSasTokens/Action|Listy tokenów SAS dla kontenera magazynu|
|/accounts/dataLakeStoreAccounts/Read|Pobrać połączonego konta DataLakeStore konta DataLakeAnalytics.|
|/accounts/dataLakeStoreAccounts/Write|Połącz konto DataLakeStore DataLakeAnalytics konta.|
|/accounts/dataLakeStoreAccounts/DELETE|Odłącz od konta DataLakeAnalytics przy użyciu konta DataLakeStore.|

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore

| Operacja | Opis |
|---|---|
|/accounts/Read|Pobiera informacje o istniał już konto DataLakeStore.|
|/ kont/zapisu|Utwórz nowe konto DataLakeStore lub zaktualizuj konto istniał już DataLakeStore.|
|/accounts/DELETE|Usuń konto istniał już DataLakeStore.|
|/accounts/firewallRules/Read|Pobierz informacje dotyczące reguły zapory.|
|/accounts/firewallRules/Write|Utwórz lub zaktualizuj regułę zapory.|
|/accounts/firewallRules/DELETE|Usuwanie reguły zapory.|
|/accounts/trustedIdProviders/Read|Pobiera informacje o zaufanego dostawcę tożsamości.|
|/accounts/trustedIdProviders/Write|Utwórz lub zaktualizuj zaufanego dostawcę tożsamości.|
|/accounts/trustedIdProviders/DELETE|Usuń zaufanego dostawcę tożsamości.|

## <a name="microsoftdevices"></a>Microsoft.Devices

| Operacja | Opis |
|---|---|
|/ register/działania|Zarejestruj subskrypcję dostawcy zasobów Centrum IotHub i umożliwia tworzenie zasobów Centrum IotHub|
|/ checkNameAvailability/działania|Sprawdź, czy jest dostępna nazwa Jeśli Centrum IotHub|
|/ użycia/odczytu|Uzyskanie subskrypcji Szczegóły obciążenia dla tego dostawcy.|
|/ operations/odczytu|Pobierz wszystkie operacje ResourceProvider|
|/ iotHubs/odczytu|Pobiera zasoby Centrum IotHub|
|/ iotHubs/zapisu|Utwórz lub zaktualizuj zasób z Centrum IotHub|
|/ iotHubs/Delete|Usuń zasób z Centrum IotHub|
|/iotHubs/listkeys/Action|Pobierz wszystkie klucze Centrum IotHub|
|/iotHubs/exportDevices/Action|Eksportuj urządzeń|
|/iotHubs/importDevices/Action|Importuj urządzeń|
|IotHubs/metricDefinitions/odczytu|Pobiera dostępne metryki dla usługi Centrum IotHub|
|/iotHubs/iotHubKeys/listkeys/Action|Pobierz klucz Centrum IotHub dla podanej nazwie|
|/iotHubs/iotHubStats/Read|Uzyskać statystyki Centrum IotHub|
|/iotHubs/quotaMetrics/Read|Pobieraj metryki przydziału|
|/iotHubs/eventHubEndpoints/consumerGroups/Write|Tworzenie grupy konsumentów EventHub|
|/iotHubs/eventHubEndpoints/consumerGroups/Read|Pobierz grupy konsumentów EventHub|
|/iotHubs/eventHubEndpoints/consumerGroups/DELETE|Usuwanie grupy konsumentów EventHub|
|polecenie testall /iotHubs/Routing/Routes/$/działania|Testowa wiadomość przed wszystkie istniejące trasy|
|/iotHubs/Routing/Routes/$ testnew/działania|Testowa wiadomość przed podany test trasy|
|IotHubs/diagnosticSettings/odczytu|Pobiera ustawienie diagnostyczne dla zasobu|
|/ IotHubs/diagnosticSettings/zapisu|Tworzy lub aktualizuje ustawienie diagnostyczne dla zasobu|
|/iotHubs/skus/Read|Pobierz prawidłowe jednostki SKU z Centrum IotHub|
|/iotHubs/Jobs/Read|Pobierz zadania szczegóły przesyłane na podany Centrum IotHub|
|/iotHubs/routingEndpointsHealth/Read|Pobiera stan wszystkich punktów końcowych routingu dla Centrum IotHub|

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab

| Operacja | Opis |
|---|---|
|/ Subskrypcji/register/działania|Rejestruje subskrypcję|
|/Labs/DELETE|Usuń labs.|
|/Labs/Read|Laboratoria odczytu.|
|/ labs/zapisu|Dodaje lub modyfikuje labs.|
|/Labs/ListVhds/Action|Listy obrazów dysku dostępne na potrzeby tworzenia niestandardowego obrazu.|
|/Labs/GenerateUploadUri/Action|Generuj identyfikator URI dla przesyłania obrazów dysku niestandardowych do laboratorium.|
|/Labs/CreateEnvironment/Action|Tworzenie maszyn wirtualnych w laboratorium.|
|/Labs/ClaimAnyVm/Action|Oświadczenia losowe claimable maszynę wirtualną w laboratorium.|
|/Labs/ExportResourceUsage/Action|Eksportuje użycia zasobów laboratorium do konta magazynu|
|/Labs/Users/DELETE|Usuń profile użytkowników.|
|/Labs/Users/Read|Przeczytaj profile użytkowników.|
|/Labs/Users/Write|Dodaje lub modyfikuje profile użytkowników.|
|/Labs/Users/secrets/DELETE|Usuwanie kluczy tajnych.|
|/Labs/Users/secrets/Read|Odczyt kluczy tajnych.|
|/Labs/Users/secrets/Write|Dodaje lub modyfikuje kluczy tajnych.|
|/Labs/Users/environments/DELETE|Usuwanie środowiska.|
|/Labs/Users/environments/Read|Przeczytaj środowisk.|
|/Labs/Users/environments/Write|Dodaje lub modyfikuje środowisk.|
|/Labs/Users/Disks/DELETE|Usuń dyski.|
|/Labs/Users/Disks/Read|Przeczytaj dysków.|
|/Labs/Users/Disks/Write|Dodaje lub modyfikuje dysków.|
|/Labs/Users/Disks/Attach/Action|Dołącz i tworzenia dzierżawy dysku do maszyny wirtualnej.|
|/Labs/Users/Disks/detach/Action|Odłącz i przerwie dzierżawy dysków dołączonych do maszyny wirtualnej.|
|/Labs/customImages/DELETE|Usuwanie obrazów niestandardowych.|
|/Labs/customImages/Read|Niestandardowe obrazy.|
|/Labs/customImages/Write|Dodaje lub modyfikuje niestandardowych obrazów.|
|/Labs/serviceRunners/DELETE|Usuń używanych modułów uruchamiających usługi.|
|/Labs/serviceRunners/Read|Przeczytaj używanych modułów uruchamiających usługi.|
|/Labs/serviceRunners/Write|Dodaje lub modyfikuje używanych modułów uruchamiających usługi.|
|/Labs/artifactSources/DELETE|Usuń źródła artefaktu.|
|/Labs/artifactSources/Read|Przeczytaj źródeł artefaktu.|
|/Labs/artifactSources/Write|Dodaje lub modyfikuje źródeł artefaktu.|
|/Labs/artifactSources/artifacts/Read|Przeczytaj artefaktów.|
|/Labs/artifactSources/artifacts/GenerateArmTemplate/Action|Generuje szablonu usługi ARM dla danego artefaktu, przekazuje wymaganych plików do konta magazynu i weryfikuje wygenerowanego artefaktu.|
|/Labs/artifactSources/armTemplates/Read|Przeczytaj szablony Menedżera zasobów platformy azure.|
|/Labs/Costs/Read|Przeczytaj kosztów.|
|/Labs/Costs/Write|Dodaje lub modyfikuje kosztów.|
|/Labs/virtualNetworks/DELETE|Usuwanie sieci wirtualnych.|
|/Labs/virtualNetworks/Read|Przeczytaj sieci wirtualnych.|
|/Labs/virtualNetworks/Write|Dodaje lub modyfikuje sieci wirtualnych.|
|/Labs/Formulas/DELETE|Usuń formuły.|
|/Labs/Formulas/Read|Przeczytaj formuły.|
|/Labs/Formulas/Write|Dodaje lub modyfikuje formuły.|
|/Labs/Schedules/DELETE|Usuń harmonogramów.|
|/Labs/Schedules/Read|Przeczytaj harmonogramów.|
|/Labs/Schedules/Write|Dodaje lub modyfikuje harmonogramów.|
|/Labs/Schedules/EXECUTE/Action|Wykonanie zgodnie z harmonogramem.|
|/Labs/Schedules/ListApplicable/Action|Wyświetla listę wszystkich odpowiednich harmonogramów|
|/Labs/galleryImages/Read|Przeczytaj obrazów w galerii.|
|/Labs/policySets/EvaluatePolicies/Action|Ocenia zasady laboratorium.|
|/Labs/policySets/policies/DELETE|Usuwanie zasad.|
|/Labs/policySets/policies/Read|Przeczytaj zasady.|
|/Labs/policySets/policies/Write|Dodaje lub modyfikuje zasady.|
|/Labs/virtualMachines/DELETE|Usuń maszyny wirtualne.|
|/Labs/virtualMachines/Read|Maszyny wirtualne do odczytu.|
|/Labs/virtualMachines/Write|Dodaje lub modyfikuje maszyny wirtualne.|
|/Labs/virtualMachines/Start/Action|Uruchom maszynę wirtualną.|
|/Labs/virtualMachines/Stop/Action|Zatrzymaj maszynę wirtualną|
|/Labs/virtualMachines/ApplyArtifacts/Action|Zastosuj artefakty do maszyny wirtualnej.|
|/Labs/virtualMachines/AddDataDisk/Action|Dołączenie dysku danych nowej lub istniejącej maszyny wirtualnej.|
|/Labs/virtualMachines/DetachDataDisk/Action|Odłączyć określonego dysku od maszyny wirtualnej.|
|/Labs/virtualMachines/Claim/Action|Przejmowanie na własność istniejącej maszyny wirtualnej|
|/Labs/virtualMachines/ListApplicableSchedules/Action|Wyświetla listę wszystkich odpowiednich harmonogramów|
|/Labs/virtualMachines/Schedules/DELETE|Usuń harmonogramów.|
|/Labs/virtualMachines/Schedules/Read|Przeczytaj harmonogramów.|
|/Labs/virtualMachines/Schedules/Write|Dodaje lub modyfikuje harmonogramów.|
|/Labs/virtualMachines/Schedules/EXECUTE/Action|Wykonanie zgodnie z harmonogramem.|
|/Labs/notificationChannels/DELETE|Usuń notificationchannels.|
|/Labs/notificationChannels/Read|Przeczytaj notificationchannels.|
|/Labs/notificationChannels/Write|Dodaje lub modyfikuje notificationchannels.|
|/Labs/notificationChannels/Notify/Action|Wyślij powiadomienia do podanego kanału.|
|/Schedules/DELETE|Usuń harmonogramów.|
|/Schedules/Read|Przeczytaj harmonogramów.|
|/ harmonogramy/zapisu|Dodaje lub modyfikuje harmonogramów.|
|/Schedules/EXECUTE/Action|Wykonanie zgodnie z harmonogramem.|
|/Schedules/Retarget/Action|Aktualizuje zasób docelowy zgodnie z harmonogramem identyfikatora.|
|/Locations/Operations/Read|Operacje odczytu.|

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

| Operacja | Opis |
|---|---|
|/databaseAccountNames/Read|Sprawdza dostępność nazwy.|
|/databaseAccounts/Read|Odczytuje konta bazy danych.|
|/ databaseAccounts/zapisu|Aktualizacja konta bazy danych.|
|/databaseAccounts/listKeys/Action|Listy kluczy konta bazy danych|
|/databaseAccounts/regenerateKey/Action|Obróć klucze konta bazy danych|
|/databaseAccounts/listConnectionStrings/Action|Pobrać parametry połączenia dla konta bazy danych|
|/databaseAccounts/changeResourceGroup/Action|Zmiana grupy zasobów dla konta bazy danych|
|/databaseAccounts/failoverPriorityChange/Action|Zmiany priorytetów trybu failover regionów konta bazy danych. To jest używana do wykonywania operacji ręcznego przełączania trybu failover|
|/databaseAccounts/DELETE|Usuwa konta bazy danych.|
|/databaseAccounts/metricDefinitions/Read|Odczytuje definicje metryk konta bazy danych.|
|/databaseAccounts/Metrics/Read|Odczytuje metryki konta bazy danych.|
|/databaseAccounts/Usages/Read|Odczytuje użycia konta bazy danych.|
|/databaseAccounts/Databases/Collections/metricDefinitions/Read|Odczytuje kolekcję definicji metryk.|
|/databaseAccounts/Databases/Collections/Metrics/Read|Odczytuje metryki kolekcji.|
|/databaseAccounts/Databases/Collections/Usages/Read|Odczytuje użycia kolekcji.|
|/databaseAccounts/Databases/metricDefinitions/Read|Odczytuje bazę danych definicji metryk|
|/databaseAccounts/Databases/Metrics/Read|Odczytuje metryki bazy danych.|
|/databaseAccounts/Databases/Usages/Read|Odczytuje użycia bazy danych.|
|/databaseAccounts/readonlykeys/Read|Odczytuje klucze tylko do odczytu konta bazy danych.|

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration

| Operacja | Opis |
|---|---|
|/ generateSsoRequest/działania|Generuje żądanie podpisywania domeny control Center.|
|/ validateDomainRegistrationInformation/działania|Sprawdzanie poprawności obiektu zakupu domeny bez przesyłania|
|/ checkDomainAvailability/działania|Sprawdź, czy domeny jest dostępne do zakupu|
|/ listDomainRecommendations/działania|Pobieranie listy zaleceń domeny na podstawie słów kluczowych|
|/ register/działania|Rejestrowanie dostawcy zasobów Domains firmy Microsoft dla subskrypcji|
|/ domen/odczytu|Pobierz listę domen|
|/ domen/zapisu|Dodaj nową domenę lub zaktualizuj istniejącą|
|/ domen/Delete|Usuń istniejącą domenę.|
|/Domains/operationresults/Read|Operacja domeny pobierania|

## <a name="microsoftdynamicslcs"></a>Microsoft.DynamicsLcs

| Operacja | Opis |
|---|---|
|/lcsprojects/Read|Wyświetl projekty usług Microsoft Dynamics cyklu życia, które należą do użytkownika|
|/ lcsprojects/zapisu|Tworzenie i aktualizowanie projektów usług Microsoft Dynamics cyklu życia, które należą do użytkownika. Można aktualizować tylko nazwę i opis właściwości. Nie można zaktualizować subskrypcji i lokalizacji skojarzony z projektem po utworzeniu|
|/lcsprojects/DELETE|Usuwanie usług programu Microsoft Dynamics cyklu życia projektów, które należą do użytkownika|
|/lcsprojects/clouddeployments/Read|Wyświetl Microsoft Dynamics AX 2012 R3 oceny wdrożenia w projekcie Microsoft Dynamics cyklu życia usług, które należą do użytkownika|
|/lcsprojects/clouddeployments/Write|Utwórz Microsoft Dynamics AX 2012 R3 oceny wdrożenia w projekcie Microsoft Dynamics cyklu życia usług, które należą do użytkownika. Wdrożeń można zarządzać za pomocą portalu zarządzania Azure|
|/lcsprojects/Connectors/Read|Łączniki odczytu, które należą do usług Microsoft Dynamics cyklu życia projektu|
|/lcsprojects/Connectors/Write|Tworzenie i aktualizowanie łączniki, które należą do usług Microsoft Dynamics cyklu życia projektu|

## <a name="microsofteventhub"></a>Microsoft.EventHub

| Operacja | Opis |
|---|---|
|/ checkNameAvailability/działania|Sprawdzanie dostępności przestrzeni nazw w podanej subskrypcji.|
|/ register/działania|Rejestruje subskrypcję dostawcy zasobów usługi EventHub i włącza funkcję tworzenia zasobów usługi EventHub|
|/ przestrzenie nazw/zapisu|Utwórz zasób Namespace i zaktualizuj jego właściwości. Znaczniki i stan Namespace są właściwości, które mogą być aktualizowane.|
|/Namespaces/Read|Pobierz listę opisów zasobów przestrzeni nazw|
|przestrzenie nazw/Delete|Usuń zasób przestrzeni nazw|
|/Namespaces/metricDefinitions/Read|Pobierz listę metryki Namespace opisów zasobów|
|/Namespaces/authorizationRules/Read|Pobierz listę opisów reguł autoryzacji przestrzeni nazw.|
|/Namespaces/authorizationRules/Write|Tworzenie reguł autoryzacji z poziomu Namespace i aktualizowanie jej właściwości. Można zaktualizować reguły autoryzacji uprawnienia, serwer podstawowy i klucze pomocnicze.|
|/Namespaces/authorizationRules/DELETE|Usuń regułę autoryzacji Namespace. Nie można usunąć reguły autoryzacji Namespace domyślnej. |
|/Namespaces/authorizationRules/listkeys/Action|Pobierz parametry połączenia z przestrzenią nazw|
|/Namespaces/authorizationRules/regenerateKeys/Action|Ponownie wygeneruj klucz podstawowy lub pomocniczy dla zasobu|
|/Namespaces/eventhubs/Write|Tworzenie lub aktualizacja EventHub właściwości.|
|/Namespaces/eventhubs/Read|Pobierz listę opisów zasobów EventHub|
|/Namespaces/eventhubs/DELETE|Operacja usunąć EventHub zasobu|
|/Namespaces/eventHubs/consumergroups/Write|Utwórz lub właściwości grupy konsumentów aktualizacji.|
|/Namespaces/eventHubs/consumergroups/Read|Pobierz listę opisów zasobów grupy konsumentów|
|/Namespaces/eventHubs/consumergroups/DELETE|Operację można usunąć grupy konsumentów zasobów|
|/Namespaces/eventhubs/authorizationRules/Read| Pobierz listę reguł autoryzacji EventHub|
|/Namespaces/eventhubs/authorizationRules/Write|Tworzenie reguł autoryzacji EventHub i aktualizowanie jej właściwości. Można zaktualizować reguły autoryzacji uprawnienia, serwer podstawowy i klucze pomocnicze.|
|/Namespaces/eventhubs/authorizationRules/DELETE|Operację można usunąć reguły autoryzacji EventHub|
|/Namespaces/eventhubs/authorizationRules/listkeys/Action|Pobierz ciąg połączenia EventHub|
|/Namespaces/eventhubs/authorizationRules/regenerateKeys/Action|Ponownie wygeneruj klucz podstawowy lub pomocniczy dla zasobu|
|/Namespaces/diagnosticSettings/Read|Pobierz listę Namespace ustawień diagnostycznych zasobu opisów|
|/Namespaces/diagnosticSettings/Write|Pobierz listę Namespace ustawień diagnostycznych zasobu opisów|
|/Namespaces/logDefinitions/Read|Pobierz listę dzienników Namespace opisów zasobów|

## <a name="microsoftfeatures"></a>Microsoft.Features

| Operacja | Opis |
|---|---|
|/Providers/Features/Read|Pobiera funkcję subskrypcji danego dostawcy zasobów.|
|/Providers/Features/Register/Action|Rejestruje funkcję subskrypcji w danego dostawcy zasobów.|
|/Features/Read|Pobiera funkcje subskrypcji.|

## <a name="microsofthdinsight"></a>Microsoft.HDInsight

| Operacja | Opis |
|---|---|
|/ klastrów/zapisu|Utwórz lub zaktualizuj klastra usługi HDInsight|
|/Clusters/Read|Uzyskiwanie szczegółowych informacji dotyczących klastra usługi HDInsight|
|/Clusters/DELETE|Usuwanie klastra usługi HDInsight|
|/Clusters/changerdpsetting/Action|Zmień ustawienie protokołu RDP dla klastra usługi HDInsight|
|/Clusters/Configurations/Action|Zaktualizuj konfigurację klastra usługi HDInsight|
|/Clusters/Configurations/Read|Pobierz konfiguracje klastrów usługi HDInsight|
|/Clusters/Roles/Resize/Action|Zmień rozmiar klastra usługi HDInsight|
|/Locations/Capabilities/Read|Pobrać możliwości subskrypcji|
|/Locations/checkNameAvailability/Read|Sprawdź dostępność nazwy|

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

| Operacja | Opis |
|---|---|
|/ register/działania|Rejestruje subskrypcję dostawcy zasobów importu/eksportu i umożliwia tworzenie zadania importu/eksportu.|
|/ zadania/zapisu|Tworzy zadanie z określonymi parametrami lub aktualizuje właściwości bądź tagi dla określonego zadania.|
|/Jobs/Read|Pobiera właściwości dla określonego zadania lub zwraca listę zadań.|
|/Jobs/listBitLockerKeys/Action|Pobiera klucze funkcji BitLocker dla określonego zadania.|
|/Jobs/DELETE|Usuwa istniejące zadanie.|
|/Locations/Read|Pobiera właściwości dla określonej lokalizacji lub zwraca listę lokalizacji.|

## <a name="microsoftinsights"></a>Elemencie Microsoft.Insights

| Operacja | Opis |
|---|---|
|/ Register/działania|Zarejestruj dostawcę usługi microsoft insights|
|/ AlertRules/zapisu|Zapisywanie w konfiguracji reguły alertu|
|/ AlertRules/usuwania|Usuwanie konfiguracji reguły alertu|
|AlertRules/odczytu|Odczytywanie konfiguracji reguły alertu|
|/ AlertRules/aktywowany/działania|Reguła alertu aktywowany|
|/ AlertRules/rozpoznać/działania|Reguła alertu rozwiązany|
|/ AlertRules/ograniczany/działania|Reguła alertu jest ograniczany.|
|AlertRules/zdarzenia/odczytu|Odczytywanie konfiguracji zdarzenia platformy reguły alertu|
|MetricDefinitions/odczytu|Odczyt definicji metryk|
|/eventtypes/VALUES/Read|Przeczytaj Zarządzanie wartości typu zdarzenia|
|/eventtypes/digestevents/Read|Odczyt zarządzania zdarzeń typu szyfrowanego|
|Metryki/odczytu|Odczytać metryki|
|/ LogProfiles/zapisu|Zapisywanie w konfiguracji profilu dziennika|
|/ LogProfiles/usuwania|Usuń konfigurację profilów dziennika|
|LogProfiles/odczytu|Profile dziennika odczytu|
|/ AutoscaleSettings/zapisu|Zapisywanie konfiguracji Ustawienia skalowania automatycznego|
|/ AutoscaleSettings/usuwania|Usuwanie konfiguracji Ustawienia skalowania automatycznego|
|AutoscaleSettings/odczytu|Odczytywanie z konfiguracji Ustawienia skalowania automatycznego|
|/ AutoscaleSettings/Scaleup/działania|Skalowania automatycznego skalowania w górę operacji|
|/ AutoscaleSettings/Scaledown/działania|Skalowania automatycznego skalowania dół operacji|
|/AutoscaleSettings/Providers/Microsoft.Insights/MetricDefinitions/Read|Odczyt definicji metryk|
|/ ActivityLogAlerts/aktywowany/działania|Wyzwolony Alert dziennika aktywności|
|/ DiagnosticSettings/zapisu|Zapisywanie konfiguracji ustawień diagnostycznych|
|/ DiagnosticSettings/usuwania|Usuwanie konfiguracji ustawień diagnostycznych|
|DiagnosticSettings/odczytu|Odczytywanie konfiguracji ustawień diagnostycznych|
|LogDefinitions/odczytu|Odczytaj definicje dzienników|
|/ ExtendedDiagnosticSettings/zapisu|Zapisywanie ustawień diagnostycznych rozszerzonej konfiguracji|
|/ ExtendedDiagnosticSettings/usuwania|Usuwanie konfiguracji rozszerzonej ustawień diagnostycznych|
|ExtendedDiagnosticSettings/odczytu|Odczytywanie konfiguracji rozszerzonej ustawień diagnostycznych|

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault

| Operacja | Opis |
|---|---|
|/ register/działania|Rejestruje subskrypcję|
|/checkNameAvailability/Read|Sprawdza, czy nazwa magazynu kluczy jest prawidłowa i nie jest w użyciu|
|/vaults/Read|Wyświetlanie właściwości magazynu kluczy|
|/ Magazyny/zapisu|Utwórz nowy magazyn kluczy lub zaktualizować właściwości istniejący magazyn kluczy|
|/vaults/DELETE|Usuń magazyn kluczy|
|/vaults/Deploy/Action|Zapewnia dostęp do kluczy tajnych w magazynie kluczy, podczas wdrażania zasobów platformy Azure|
|/vaults/secrets/Read|Wyświetlanie właściwości klucz tajny, ale nie jej wartość|
|/vaults/secrets/Write|Utwórz nowy klucz tajny lub zaktualizuj tę wartość z istniejącym kluczem tajnym|
|/vaults/accessPolicies/Write|Zaktualizuj istniejących zasad dostępu przez scalanie lub wymiana lub Dodaj nowe zasady dostępu do magazynu.|
|/deletedVaults/Read|Wyświetlanie właściwości nietrwałego usunięto magazynów kluczy|
|/Locations/operationResults/Read|Sprawdź wynik operacji długoterminowym|
|/Locations/deletedVaults/Read|Wyświetlanie właściwości nietrwałego usunięto magazyn kluczy|
|/Locations/deletedVaults/PURGE/Action|Przeczyść nietrwałego usunięto magazyn kluczy|

## <a name="microsoftlogic"></a>Microsoft.Logic

| Operacja | Opis |
|---|---|
|/Workflows/Read|Odczytuje przepływ pracy.|
|/ przepływy pracy/zapisu|Tworzy lub modyfikuje przepływ pracy.|
|/Workflows/DELETE|Usuwa przepływ pracy.|
|/Workflows/Run/Action|Uruchamia przebieg przepływu pracy.|
|/Workflows/disable/Action|Wyłącza przepływ pracy.|
|/Workflows/enable/Action|Włącza przepływ pracy.|
|/Workflows/Validate/Action|Sprawdza poprawność przepływu pracy.|
|/Workflows/Move/Action|Przenosi przepływ pracy z jego obecnego identyfikatora subskrypcji, grupy zasobów i/lub nazwy do innego identyfikatora subskrypcji, grupy zasobów i/lub nazwy.|
|/Workflows/listSwagger/Action|Pobiera struktury swagger definicji przepływu pracy.|
|/Workflows/regenerateAccessKey/Action|Generuje ponownie wpisy tajne klucza dostępu.|
|/Workflows/listCallbackUrl/Action|Pobiera adres URL wywołania zwrotnego dla przepływu pracy.|
|/Workflows/Versions/Read|Odczytuje wersję przepływu pracy.|
|/Workflows/Versions/Triggers/listCallbackUrl/Action|Pobiera adres URL wywołania zwrotnego dla wyzwalacza.|
|przepływy pracy/uruchamia/Odczyt|Odczytuje przebieg przepływu pracy.|
|/Workflows/runs/Cancel/Action|Anuluje przebieg przepływu pracy.|
|/Workflows/runs/Actions/Read|Odczytuje akcję przebiegu przepływu pracy.|
|/Workflows/runs/Operations/Read|Odczytuje stan operacji przebiegu przepływu pracy.|
|/Workflows/Triggers/Read|Odczytuje wyzwalacz.|
|/Workflows/Triggers/Run/Action|Uruchamia wyzwalacz.|
|/Workflows/Triggers/listCallbackUrl/Action|Pobiera adres URL wywołania zwrotnego dla wyzwalacza.|
|/Workflows/Triggers/histories/Read|Odczytuje historie wyzwalacza.|
|/Workflows/Triggers/histories/resubmit/Action|Przesyła ponownie wyzwalacz przepływu pracy.|
|/Workflows/accessKeys/Read|Odczytuje klucz dostępu.|
|/Workflows/accessKeys/Write|Tworzy lub aktualizuje klucz dostępu.|
|/Workflows/accessKeys/DELETE|Usuwa klucz dostępu.|
|/Workflows/accessKeys/list/Action|Wyświetla wpisy tajne klucza dostępu.|
|/Workflows/accessKeys/regenerate/Action|Generuje ponownie wpisy tajne klucza dostępu.|
|/Locations/Workflows/Validate/Action|Sprawdza poprawność przepływu pracy.|

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning

| Operacja | Opis |
|---|---|
|/ register/działania|Rejestruje subskrypcję dostawcy zasobów usługi sieci web uczenia maszynowego i umożliwia tworzenie usług sieci web.|
|/ usług sieci Web/działania|Utwórz regionalnych właściwości obsługiwanych regionów usługi sieci Web|
|/commitmentPlans/Read|Przeczytaj maszyn zobowiązań Plan nauki|
|/ commitmentPlans/zapisu|Utwórz lub zaktualizuj żadnych zobowiązań Plan nauki maszyny|
|/commitmentPlans/DELETE|Usuń wszystkie maszyny zobowiązań Plan nauki|
|/commitmentPlans/JOIN/Action|Dołącz do dowolnej maszyny zobowiązań Plan nauki|
|/commitmentPlans/commitmentAssociations/Read|Przeczytaj maszyn zobowiązań skojarzenia Plan nauki|
|/commitmentPlans/commitmentAssociations/Move/Action|Przenieś wszystkie Machine Learning skojarzenia planu zobowiązań|
|Obszary robocze/odczytu|Wszelkie Machine Learning obszaru roboczego do odczytu|
|/ Workspaces/zapisu|Utwórz lub zaktualizuj wszystkie Machine Learning obszaru roboczego|
|/ Workspaces/usuwania|Usuń wszelkie Machine Learning obszaru roboczego|
|/ Workspaces/listworkspacekeys/działania|Lista kluczy dla obszaru roboczego Machine Learning|
|/ Workspaces/resyncstoragekeys/działania|Zsynchronizowanie klucze konta magazynu skonfigurowanego dla obszaru roboczego Machine Learning|
|/webServices/Read|Usługi sieci Web Machine Learning do odczytu|
|/ usług sieci Web/zapisu|Utwórz lub zaktualizuj usługi sieci Web Machine Learning|
|/webServices/DELETE|Usuń usługi sieci Web Machine Learning|

## <a name="microsoftmarketplaceordering"></a>Microsoft.MarketplaceOrdering

| Operacja | Opis |
|---|---|
|/agreements/offers/plans/Read|Zwraca umowę dla elementu danej witryny marketplace|
|/agreements/offers/plans/sign/Action|Podpisać umowę dla elementu danej witryny marketplace|
|/agreements/offers/plans/Cancel/Action|Anuluj umowę dla elementu danej witryny marketplace|

## <a name="microsoftmedia"></a>Microsoft.Media

| Operacja | Opis |
|---|---|
|/mediaservices/Read||
|/ mediaservices/zapisu||
|/mediaservices/DELETE||
|/mediaservices/regenerateKey/Action||
|/mediaservices/listKeys/Action||
|/mediaservices/syncStorageKeys/Action||

## <a name="microsoftnetwork"></a>Microsoft.Network

| Operacja | Opis |
|---|---|
|/ register/działania|Rejestruje subskrypcję|
|/ unregister/działania|Wyrejestrowuje subskrypcji|
|/ checkTrafficManagerNameAvailability/działania|Sprawdza dostępność nazwy DNS względem usługi Traffic Manager.|
|/dnszones/Read|Pobierz strefę DNS, w formacie JSON. Właściwości strefy obejmują tag, etag, numberOfRecordSets i maxNumberOfRecordSets. Należy pamiętać, że to polecenie nie pobiera zestawów rekordów znajdujących się w strefie.|
|/ dnszones/zapisu|Utwórz lub zaktualizuj strefę DNS w ramach grupy zasobów.  Używane do aktualizowania elementów tag w zasobie strefy DNS. Należy pamiętać, że to polecenie nie może służyć do utworzenia ani zaktualizowania zestawów rekordów w strefie.|
|/dnszones/DELETE|Usuń strefę DNS, w formacie JSON. Właściwości strefy obejmują tag, etag, numberOfRecordSets i maxNumberOfRecordSets.|
|/dnszones/MX/Read|Pobierz zestaw rekordów typu "MX" w formacie JSON. Zestaw rekordów zawiera listę rekordów oraz TTL, znaczników i etag.|
|/dnszones/MX/Write|Utwórz lub zaktualizuj zestaw rekordów typu "MX" w strefie DNS. Określone rekordy zastąpią bieżące rekordy w zestawie rekordów.|
|/dnszones/MX/DELETE|Usuń zestaw rekordów o danej nazwie i typu "MX" ze strefy DNS.|
|/dnszones/NS/Read|Pobiera zestaw rekordów DNS typu NS|
|/dnszones/NS/Write|Tworzy lub aktualizuje zestaw rekordów DNS typu NS|
|/dnszones/NS/DELETE|Usuwa zestaw rekordów DNS typu NS|
|/dnszones/AAAA/Read|Pobierz zestaw rekordów typu "AAAA" w formacie JSON. Zestaw rekordów zawiera listę rekordów oraz TTL, znaczników i etag.|
|/dnszones/AAAA/Write|Utwórz lub zaktualizuj zestaw rekordów typu "AAAA" w strefie DNS. Określone rekordy zastąpią bieżące rekordy w zestawie rekordów.|
|/dnszones/AAAA/DELETE|Usuń zestaw rekordów o danej nazwie i typu "AAAA" ze strefy DNS.|
|/dnszones/CNAME/Read|Pobierz zestaw rekordów typu "CNAME" w formacie JSON. Zestaw rekordów zawiera TTL, tag i etag.|
|/dnszones/CNAME/Write|Utwórz lub zaktualizuj zestaw rekordów typu "CNAME" w strefie DNS. Określone rekordy zastąpią bieżące rekordy w zestawie rekordów.|
|/dnszones/CNAME/DELETE|Usuń zestaw rekordów o danej nazwie i typu "CNAME" ze strefy DNS.|
|/dnszones/SOA/Read|Pobiera zestaw rekordów DNS typu SOA|
|/dnszones/SOA/Write|Tworzy lub aktualizuje zestaw rekordów DNS typu SOA|
|/dnszones/SRV/Read|Pobierz zestaw rekordów typu "SRV" w formacie JSON. Zestaw rekordów zawiera listę rekordów oraz TTL, znaczników i etag.|
|/dnszones/SRV/Write|Utwórz lub zaktualizuj zestaw rekordów typu SRV|
|/dnszones/SRV/DELETE|Usuń zestaw rekordów o danej nazwie i typu "SRV" ze strefy DNS.|
|/dnszones/PTR/Read|Pobierz zestaw rekordów typu "PTR" w formacie JSON. Zestaw rekordów zawiera listę rekordów oraz TTL, znaczników i etag.|
|/dnszones/PTR/Write|Utwórz lub zaktualizuj zestaw rekordów typu "PTR" w strefie DNS. Określone rekordy zastąpią bieżące rekordy w zestawie rekordów.|
|/dnszones/PTR/DELETE|Usuń zestaw rekordów o danej nazwie i typu "PTR" ze strefy DNS.|
|/dnszones/A/Read|Pobierz zestaw rekordów typu "A", w formacie JSON. Zestaw rekordów zawiera listę rekordów oraz TTL, znaczników i etag.|
|/dnszones/A/Write|Utwórz lub zaktualizuj zestaw rekordów typu "A" w strefie DNS. Określone rekordy zastąpią bieżące rekordy w zestawie rekordów.|
|/dnszones/A/DELETE|Usuń zestaw rekordów o danej nazwie i typu "A" ze strefy DNS.|
|/dnszones/txt/Read|Pobierz zestaw rekordów typu "TXT" w formacie JSON. Zestaw rekordów zawiera listę rekordów oraz TTL, znaczników i etag.|
|/dnszones/txt/Write|Utwórz lub zaktualizuj zestaw rekordów typu "TXT" w strefie DNS. Określone rekordy zastąpią bieżące rekordy w zestawie rekordów.|
|/dnszones/txt/DELETE|Usuń zestaw rekordów o danej nazwie i typu "TXT" ze strefy DNS.|
|/dnszones/Recordsets/Read|Pobiera różnych typów zestawów rekordów DNS|
|/networkInterfaces/Read|Pobiera definicję interfejsu sieciowego. |
|/ Networkinterface/zapisu|Tworzy interfejs sieciowy lub aktualizuje istniejący interfejs sieciowy. |
|/networkInterfaces/JOIN/Action|Dołącza maszynę wirtualną do interfejsu sieciowego|
|/networkInterfaces/DELETE|Usuwa interfejs sieciowy|
|/networkInterfaces/effectiveRouteTable/Action|Pobierz tabelę tras skonfigurowane dla interfejsu sieciowego maszyny wirtualnej|
|/networkInterfaces/effectiveNetworkSecurityGroups/Action|Pobieranie grup zabezpieczeń sieci skonfigurowane w sieci interfejsu z maszyny wirtualnej|
|/networkInterfaces/loadBalancers/Read|Pobiera wszystkich usług równoważenia obciążenia interfejsu sieciowego jest częścią|
|/networkInterfaces/ipconfigurations/Read|Pobiera definicję konfiguracji adresu ip interfejsu sieciowego. |
|/publicIPAddresses/Read|Pobiera definicję adres publiczny adres ip.|
|/ elementów Publicipaddress/zapisu|Tworzy publiczny adres Ip lub aktualizuje istniejący publiczny adres Ip. |
|/publicIPAddresses/DELETE|Usuwa publiczny adres Ip.|
|/publicIPAddresses/JOIN/Action|Dołącza do publicznego adresu ip|
|/routeFilters/Read|Pobiera definicję filtru tras|
|/routeFilters/JOIN/Action|Dołącza filtr tras|
|/routeFilters/DELETE|Usuwa definicję filtru tras|
|/ routeFilters/zapisu|Tworzy filtr tras lub aktualizuje istniejący filtr tras|
|/routeFilters/Rules/Read|Pobiera definicję reguły filtru trasy|
|/routeFilters/Rules/Write|Tworzy regułę filtru tras lub aktualizuje istniejącą regułę filtru trasy|
|/routeFilters/Rules/DELETE|Usuwa definicję reguły filtru trasy|
|/networkWatchers/Read|Pobierz definicję obserwatora sieciowego|
|/ networkWatchers/zapisu|Tworzy obserwatora sieciowego lub aktualizuje istniejącą obserwatora sieciowego|
|/networkWatchers/DELETE|Usuwa obserwatora sieciowego|
|/networkWatchers/configureFlowLog/Action|Konfiguruje przepływu rejestrowanie dla zasobu docelowego.|
|/networkWatchers/ipFlowVerify/Action|Zwraca czy pakiet jest dozwolony lub odmowa dostępu do lub z danego przeznaczenia.|
|/networkWatchers/nextHop/Action|Określony obiekt docelowy oraz docelowy adres IP zwracany typ następnego przeskoku i dalej nadzieję, że adres IP.|
|/networkWatchers/queryFlowLogStatus/Action|Pobiera stan przepływu rejestrowania w zasobie.|
|/networkWatchers/queryTroubleshootResult/Action|Pobiera wynik rozwiązywania problemów z poprzednio uruchomionymi lub aktualnie uruchomiona operacja rozwiązywania problemów.|
|/networkWatchers/securityGroupView/Action|Wyświetl reguły grupy zabezpieczeń sieci skonfigurowane i skuteczne, stosowane na maszynie Wirtualnej.|
|/networkWatchers/Topology/Action|Pobiera widok poziomu sieci, zasobów i ich relacje w grupie zasobów.|
|/networkWatchers/Troubleshoot/Action|Uruchamia Rozwiązywanie problemów w zasobie sieci na platformie Azure.|
|/networkWatchers/packetCaptures/queryStatus/Action|Pobiera informacje o właściwościach i stan zasobu przechwytywania pakietów.|
|/networkWatchers/packetCaptures/Stop/Action|Zatrzymaj uruchomiona sesja przechwytywania pakietów.|
|/networkWatchers/packetCaptures/Read|Pobierz definicję przechwytywania pakietów|
|/networkWatchers/packetCaptures/Write|Tworzy przechwytywania pakietów|
|/networkWatchers/packetCaptures/DELETE|Usuwa przechwytywania pakietów|
|/loadBalancers/Read|Pobiera definicję modułu równoważenia obciążenia|
|/ loadBalancers/zapisu|Tworzy moduł równoważenia obciążenia lub aktualizuje istniejący moduł równoważenia obciążenia|
|/loadBalancers/DELETE|Usuwa usługę równoważenia obciążenia|
|/loadBalancers/networkInterfaces/Read|Pobiera odwołania do wszystkich interfejsów sieciowych należących do modułu równoważenia obciążenia|
|/loadBalancers/loadBalancingRules/Read|Pobiera definicję modułu równoważenia obciążenia obciążenia równoważenia reguły|
|/loadBalancers/backendAddressPools/Read|Pobiera definicję puli adresów zaplecza modułu równoważenia obciążenia|
|/loadBalancers/backendAddressPools/JOIN/Action|Dołącza do puli adresów zaplecza modułu równoważenia obciążenia|
|/loadBalancers/inboundNatPools/Read|Pobiera moduł równoważenia obciążenia ruchu przychodzącego definicję puli translacji nat|
|/loadBalancers/inboundNatPools/JOIN/Action|Tworzy sprzężenie modułu równoważenia obciążenia ruchu przychodzącego pulę translacji nat|
|/loadBalancers/inboundNatRules/Read|Pobiera moduł równoważenia obciążenia definicję przychodzącej reguły nat|
|/loadBalancers/inboundNatRules/Write|Tworzy regułę nat dla ruchu przychodzącego modułu równoważenia obciążenia lub aktualizuje istniejącą regułę nat dla ruchu przychodzącego modułu równoważenia obciążenia|
|/loadBalancers/inboundNatRules/DELETE|Usuwa regułę nat dla ruchu przychodzącego modułu równoważenia obciążenia|
|/loadBalancers/inboundNatRules/JOIN/Action|Tworzy sprzężenie reguły nat dla ruchu przychodzącego modułu równoważenia obciążenia|
|/loadBalancers/outboundNatRules/Read|Pobiera definicję reguły nat ruchu wychodzącego modułu równoważenia obciążenia|
|/loadBalancers/probes/Read|Pobiera sondę modułu równoważenia obciążenia|
|/loadBalancers/virtualMachines/Read|Pobiera odwołania do wszystkich maszyn wirtualnych należących do modułu równoważenia obciążenia|
|/loadBalancers/frontendIPConfigurations/Read|Pobiera definicję konfiguracji IP frontonu modułu równoważenia obciążenia|
|/trafficManagerGeographicHierarchies/Read|Pobiera zawierający regionów, które mogą być używane z metody routingu ruchu geograficznego hierarchii Geographic Menedżera ruchu|
|/bgpServiceCommunities/Read|Pobierz społeczności usługi protokołu Bgp|
|/applicationGatewayAvailableWafRuleSets/Read|Pobiera bramę aplikacji, ustawia dostępne reguły zapory aplikacji sieci Web|
|/virtualNetworks/Read|Pobierz definicję sieci wirtualnej|
|/ virtualNetworks/zapisu|Tworzy sieć wirtualną lub aktualizuje istniejącą sieć wirtualną|
|/virtualNetworks/DELETE|Usuwa sieci wirtualnej|
|/virtualNetworks/peer/Action|Równorzędnymi użytkownikami sieci wirtualnej z inną siecią wirtualną|
|/virtualNetworks/virtualNetworkPeerings/Read|Pobiera definicję sieci wirtualnej komunikacji równorzędnej|
|/virtualNetworks/virtualNetworkPeerings/Write|Tworzy element równorzędny sieci wirtualnej lub aktualizuje istniejące sieci wirtualnej komunikacji równorzędnej|
|/virtualNetworks/virtualNetworkPeerings/DELETE|Usuwa element równorzędny sieci wirtualnej|
|/virtualNetworks/Subnets/Read|Pobiera definicję podsieci sieci wirtualnej|
|/virtualNetworks/Subnets/Write|Tworzy podsieć sieci wirtualnej lub aktualizuje istniejącą podsieć sieci wirtualnej|
|/virtualNetworks/Subnets/DELETE|Usuwa podsieć sieci wirtualnej|
|/virtualNetworks/Subnets/JOIN/Action|Tworzy sprzężenie sieci wirtualnej|
|/virtualNetworks/Subnets/joinViaServiceTunnel/Action|Tworzy sprzężenie zasobów, takich jak konta magazynu lub bazy danych SQL do podsieci usługi tunelowania włączone.|
|/virtualNetworks/Subnets/virtualMachines/Read|Pobiera odwołania do wszystkich maszyn wirtualnych w podsieci sieci wirtualnej|
|/virtualNetworks/checkIpAddressAvailability/Read|Sprawdź, czy adres Ip jest dostępna w określonej sieci wirtualnej|
|/virtualNetworks/virtualMachines/Read|Pobiera odwołania do wszystkich maszyn wirtualnych w sieci wirtualnej|
|/expressRouteServiceProviders/Read|Pobiera dostawców usługi Expressroute|
|/dnsoperationresults/Read|Pobiera wyniki operacji DNS|
|/localnetworkgateways/Read|Pobiera LocalNetworkGateway|
|/ localnetworkgateways/zapisu|Tworzy lub aktualizuje istniejące LocalNetworkGateway|
|/localnetworkgateways/DELETE|Usuwa LocalNetworkGateway|
|/trafficManagerProfiles/Read|Pobierz konfigurację profilu Menedżera ruchu. W tym ustawienia DNS, ustawienia kierowania ruchu, ustawienia monitorowania punktu końcowego i listę punktów końcowych kierowanych przez ten profil Menedżera ruchu.|
|/ trafficManagerProfiles/zapisu|Tworzenie profilu Menedżera ruchu lub zmodyfikować konfigurację istniejącego profilu Menedżera ruchu. Dotyczy to również włączenia lub wyłączenia profilu i modyfikowanie ustawień DNS, ustawienia kierowania ruchu lub ustawień monitorowania dla punktu końcowego. Punkty końcowe kierowane przez profil Menedżera ruchu można dodać, usunąć, włączone lub wyłączone.|
|/trafficManagerProfiles/DELETE|Usuń profil Menedżera ruchu. Wszystkie ustawienia skojarzone z profilem Menedżera ruchu zostaną utracone i profilu nie może służyć do kierowania ruchem.|
|/dnsoperationstatuses/Read|Pobiera stan operacji DNS |
|/Operations/Read|Pobiera dostępne operacje|
|/expressRouteCircuits/Read|Pobierz ExpressRouteCircuit|
|/ expressRouteCircuits/zapisu|Tworzy lub aktualizuje istniejące ExpressRouteCircuit|
|/expressRouteCircuits/DELETE|Usuwa ExpressRouteCircuit|
|/expressRouteCircuits/stats/Read|Pobiera stan ExpressRouteCircuit|
|/expressRouteCircuits/peerings/Read|Pobiera ExpressRouteCircuit komunikacji równorzędnej|
|/expressRouteCircuits/peerings/Write|Tworzy lub aktualizuje istniejącą ExpressRouteCircuit komunikacji równorzędnej|
|/expressRouteCircuits/peerings/DELETE|Usuwa ExpressRouteCircuit komunikacji równorzędnej|
|/expressRouteCircuits/peerings/arpTables/Action|Pobiera ExpressRouteCircuit ArpTable komunikacji równorzędnej|
|/expressRouteCircuits/peerings/routeTables/Action|Pobiera stan równorzędna ExpressRouteCircuit|
|/expressRouteCircuits/komunikacji równorzędnych /<br>routeTablesSummary/działania|Pobiera ExpressRouteCircuit równorzędna stan podsumowanie|
|/expressRouteCircuits/peerings/stats/Read|Pobiera ExpressRouteCircuit równorzędna Stat|
|/expressRouteCircuits/authorizations/Read|Pobiera autoryzacji ExpressRouteCircuit|
|/expressRouteCircuits/authorizations/Write|Tworzy lub aktualizuje istniejącą autoryzacji ExpressRouteCircuit|
|/expressRouteCircuits/authorizations/DELETE|Usuwa ExpressRouteCircuit autoryzacji|
|/Connections/Read|Pobiera element VirtualNetworkGatewayConnection|
|/ połączeń/zapisu|Tworzy lub aktualizuje istniejący element VirtualNetworkGatewayConnection|
|/Connections/DELETE|Usuwa element VirtualNetworkGatewayConnection|
|/Connections/sharedKey/Read|Pobiera element VirtualNetworkGatewayConnection SharedKey|
|/Connections/sharedKey/Write|Tworzy lub aktualizuje istniejącą SharedKey element VirtualNetworkGatewayConnection|
|/networkSecurityGroups/Read|Pobiera definicję grupy zabezpieczeń sieci|
|/ Networksecuritygroup/zapisu|Tworzy sieciową grupę zabezpieczeń lub aktualizuje istniejącą sieciową grupę zabezpieczeń|
|/networkSecurityGroups/DELETE|Usuwa sieciową grupę zabezpieczeń|
|/networkSecurityGroups/JOIN/Action|Dołącza do sieciowej grupy zabezpieczeń|
|/networkSecurityGroups/defaultSecurityRules/Read|Pobiera domyślną definicję reguły zabezpieczeń|
|/networkSecurityGroups/securityRules/Read|Pobiera definicję reguły zabezpieczeń|
|/networkSecurityGroups/securityRules/Write|Tworzy regułę zabezpieczeń lub aktualizuje istniejącą regułę zabezpieczeń|
|/networkSecurityGroups/securityRules/DELETE|Usuwa regułę zabezpieczeń|
|/applicationGateways/Read|Pobiera bramę aplikacji|
|/ applicationGateways/zapisu|Tworzy bramę aplikacji lub aktualizuje istniejącą bramę aplikacji|
|/applicationGateways/DELETE|Usuwa bramę aplikacji|
|/applicationGateways/backendhealth/Action|Pobiera kondycji zaplecza bramy aplikacji|
|/applicationGateways/Start/Action|Uruchamia bramy aplikacji|
|/applicationGateways/Stop/Action|Zatrzymuje bramy aplikacji|
|/applicationGateways/backendAddressPools/JOIN/Action|Dołącza do puli adresów zaplecza bramy aplikacji|
|/routeTables/Read|Pobiera definicję tabeli tras|
|/ routeTables/zapisu|Tworzy tabelę tras lub aktualizuje istniejącą tabelę tras|
|/routeTables/DELETE|Usuwa definicję tabeli tras|
|/routeTables/JOIN/Action|Tworzy sprzężenie tabeli tras|
|/routeTables/Routes/Read|Pobiera definicję trasy|
|/routeTables/Routes/Write|Tworzy trasę lub aktualizuje istniejącą trasę|
|/routeTables/Routes/DELETE|Usuwa definicję trasy|
|/Locations/operationResults/Read|Pobiera wynik operacji asynchronicznej POST lub operacja usuwania|
|/Locations/checkDnsNameAvailability/Read|Sprawdza, czy etykieta dns jest dostępna w określonej lokalizacji|
|/Locations/Usages/Read|Pobiera metryki użycia zasobów|
|/Locations/Operations/Read|Pobiera zasób operacji reprezentujący stan operacji asynchronicznej|

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs

| Operacja | Opis |
|---|---|
|/ register/działania|Rejestruje subskrypcję dostawcy zasobów usługi NotifciationHubs i włącza funkcję tworzenia przestrzeni nazw i centrów NotificationHub|
|/ CheckNamespaceAvailability/działania|Sprawdza, czy dana nazwa zasobu przestrzeni nazw jest dostępna w usłudze NotificationHub.|
|Przestrzenie nazw/zapisu|Utwórz zasób Namespace i zaktualizuj jego właściwości. Znaczniki i stan Namespace są właściwości, które mogą być aktualizowane.|
|Przestrzenie nazw/odczytu|Pobierz listę opisów zasobów przestrzeni nazw|
|/ Przestrzenie nazw/usuwania|Usuń zasób przestrzeni nazw|
|/ Przestrzenie nazw/authorizationRules/działania|Pobierz listę opisów reguł autoryzacji przestrzeni nazw.|
|/ Przestrzenie nazw/CheckNotificationHubAvailability/działania|Sprawdza, czy dana nazwa usługi NotificationHub jest dostępna w przestrzeni nazw.|
|Przestrzenie nazw/authorizationRules/zapisu|Tworzenie reguł autoryzacji z poziomu Namespace i aktualizowanie jej właściwości. Można zaktualizować reguły autoryzacji uprawnienia, serwer podstawowy i klucze pomocnicze.|
|Przestrzenie nazw/authorizationRules/odczytu|Pobierz listę opisów reguł autoryzacji przestrzeni nazw.|
|/ Przestrzenie nazw/authorizationRules/usuwania|Usuń regułę autoryzacji Namespace. Nie można usunąć reguły autoryzacji Namespace domyślnej. |
|/ Przestrzenie nazw/authorizationRules/listkeys/działania|Pobierz parametry połączenia z przestrzenią nazw|
|/ Przestrzenie nazw/authorizationRules/regenerateKeys/działania|Reguła autoryzacji przestrzeni nazw: wygeneruj ponownie klucz podstawowy/pomocniczy. Określ klucz do ponownego wygenerowania|
|Przestrzenie nazw/NotificationHubs/zapisu|Tworzenie Centrum powiadomień i aktualizowanie jej właściwości. Jego właściwości obejmują głównie poświadczeń systemu powiadomień platformy. Reguły autoryzacji i czas wygaśnięcia|
|Przestrzenie nazw/NotificationHubs/odczytu|Pobierz listę opisów zasobów centrum powiadomień|
|/ Przestrzenie nazw/NotificationHubs/usuwania|Usuń zasób centrum powiadomień|
|/ Przestrzenie nazw/NotificationHubs/authorizationRules/działania|Pobierz listę reguł autoryzacji centrum powiadomień|
|/ Przestrzenie nazw/NotificationHubs/pnsCredentials/działania|Pobierz wszystkie poświadczenia systemu powiadomień platformy Centrum powiadomień. Obejmuje to poświadczenia usługi WNS, MPNS, APNS, usługi GCM i Baidu|
|/ Przestrzenie nazw/NotificationHubs/debugSend/działania|Wyślij testowe powiadomienie wypychane.|
|Przestrzenie nazw/NotificationHubs/metricDefinitions/odczytu|Pobierz listę metryki Namespace opisów zasobów|
|/Namespaces/NotificationHubs /<br>authorizationRules/zapisu|Tworzenie reguł autoryzacji Centrum powiadomień i aktualizowanie jej właściwości. Można zaktualizować reguły autoryzacji uprawnienia, serwer podstawowy i klucze pomocnicze.|
|/Namespaces/NotificationHubs /<br>authorizationRules/Odczyt|Pobierz listę reguł autoryzacji centrum powiadomień|
|/Namespaces/NotificationHubs /<br>authorizationRules/usuwania|Usuń reguły autoryzacji centrum powiadomień|
|/Namespaces/NotificationHubs /<br>Akcja authorizationRules/listkeys|Pobierz parametry połączenia z centrum powiadomień|
|/Namespaces/NotificationHubs /<br>Akcja authorizationRules/regenerateKeys|Reguła autoryzacji centrum powiadomień: wygeneruj ponownie klucz główny/pomocniczy. Określ klucz do ponownego wygenerowania|

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

| Operacja | Opis |
|---|---|
|/ register/działania|Zarejestruj subskrypcję dostawcy zasobów.|
|/linkTargets/Read|Wyświetla listę istniejących kont, które nie są skojarzone z subskrypcją platformy Azure. Aby połączyć tej subskrypcji platformy Azure do obszaru roboczego, użyj identyfikator klienta zwracane przez tę operację we właściwości klienta identyfikator operacji Utwórz obszar roboczy.|
|/ workspaces/zapisu|Tworzy nowy obszar roboczy lub łączy z istniejącym obszarem roboczym, podając identyfikator klienta z istniejącym obszarem roboczym.|
|/workspaces/Read|Pobiera istniejący obszar roboczy|
|/workspaces/DELETE|Usuwa obszaru roboczego. Jeśli obszar roboczy został połączony z istniejącym obszarem roboczym w czasie tworzenia obszaru roboczego, który był połączony z nie zostanie usunięta.|
|/workspaces/generateregistrationcertificate/Action|Generuje certyfikatu rejestracji dla obszaru roboczego. Ten certyfikat służy do nawiązania połączenia obszaru roboczego programu Microsoft System Center Operations Manager.|
|/workspaces/sharedKeys/Action|Pobiera klucze udostępnionego dla obszaru roboczego. Klucze te są używane do połączenia agentów Microsoft Operational Insights do obszaru roboczego.|
|/workspaces/Search/Action|Wykonuje zapytanie wyszukiwania|
|/workspaces/Datasources/Read|Pobierz źródeł danych w obszarze roboczym.|
|/workspaces/Datasources/Write|Utwórz/Aktualizuj źródeł danych w obszarze roboczym.|
|/workspaces/Datasources/DELETE|Usuwanie źródeł danych w obszarze roboczym.|
|/workspaces/managementGroups/Read|Pobiera nazwy i metadanych dla grupy zarządzania programu System Center Operations Manager podłączone do tego obszaru roboczego.|
|/workspaces/Schema/Read|Pobiera schematu wyszukiwania dla obszaru roboczego.  Schematu wyszukiwania zawiera narażonych pól i ich typów.|
|/workspaces/Usages/Read|Pobiera dane użycia dla obszaru roboczego, w tym ilość danych do odczytu w obszarze roboczym.|
|/workspaces/intelligencepacks/Read|Wyświetla listę wszystkich pakietów analizy, które są widoczne dla danego worksapce, a także wyświetla informację, czy pakiet jest włączone lub wyłączone dla tego obszaru roboczego.|
|/workspaces/intelligencepacks/enable/Action|Włącza pakiet analizy dla danego obszaru roboczego.|
|/workspaces/intelligencepacks/disable/Action|Wyłącza pakietu intelligence pack dla danego obszaru roboczego.|
|/workspaces/sharedKeys/Read|Pobiera klucze udostępnionego dla obszaru roboczego. Klucze te są używane do połączenia agentów Microsoft Operational Insights do obszaru roboczego.|
|/workspaces/savedSearches/Read|Pobiera zapytanie zapisanego wyszukiwania|
|/workspaces/savedSearches/Write|Tworzy zapytanie zapisanego wyszukiwania|
|/workspaces/savedSearches/DELETE|Usuwa kwerendę zapisanego wyszukiwania|
|/workspaces/storageinsightconfigs/Write|Tworzy nową konfigurację magazynu. Te konfiguracje są używane do pobierania danych z lokalizacji w istniejącego konta magazynu.|
|/workspaces/storageinsightconfigs/Read|Pobiera konfiguracji magazynu.|
|/workspaces/storageinsightconfigs/DELETE|Usuwa konfiguracji magazynu. Przestanie Microsoft Operational Insights odczytywanie danych z konta magazynu.|
|/workspaces/configurationScopes/Read|Pobierz konfigurację zakresu|
|/workspaces/configurationScopes/Write|Ustawianie zakresu konfiguracji|
|/workspaces/configurationScopes/DELETE|Usuwanie konfiguracji zakresu|

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement

| Operacja | Opis |
|---|---|
|/ register/działania|Zarejestruj subskrypcję dostawcy zasobów.|
|/ rozwiązań/zapisu|Utwórz nowe rozwiązanie OMS|
|/Solutions/Read|Pobierz Kończenie rozwiązania OMS|
|/Solutions/DELETE|Usuń istniejące rozwiązanie OMS|

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices

| Operacja | Opis |
|---|---|
|/ Magazyny/backupJobsExport/działania|Eksportowanie zadań|
|/ Magazyny/zapisu|Operacja Utwórz magazyn tworzy zasób platformy Azure typu „magazyn”|
|Magazyny/odczytu|Operacja Get magazynu pobiera obiekt reprezentujący zasobów platformy Azure typu "magazynu"|
|/ Magazyny/usuwania|Operacja usuwania magazynu usuwa określony zasób Azure typu "magazynu"|
|Magazyny/refreshContainers/odczytu|Odświeża listę kontenera|
|Magazyny/backupJobsExport/operationResults/odczytu|Zwraca wynik operacji eksportowania zadania.|
|Magazyny/backupOperationResults/odczytu|Zwraca wynik operacji tworzenia kopii zapasowej magazynu usług Recovery Services.|
|Magazyny/monitoringAlerts/odczytu|Pobiera alertów dla magazynu usług odzyskiwania.|
|/Vaults/monitoringAlerts / {uniqueAlertId} / Odczyt|Pobiera szczegóły alertu.|
|Magazyny/backupSecurityPIN/odczytu|Zwraca zabezpieczający numer PIN informacji odzyskiwania usług magazynu.|
|/vaults/replicationEvents/Read|Przeczytaj żadnych zdarzeń|
|Magazyny/backupProtectableItems/odczytu|Zwraca listę wszystkich elementów podlegających ochronie|
|/vaults/replicationFabrics/Read|Żadnych sieci szkieletowych do odczytu|
|/vaults/replicationFabrics/Write|Utwórz lub zaktualizuj żadnych sieci szkieletowych|
|/vaults/replicationFabrics/Remove/Action|Usunięcie sieci szkieletowej|
|/vaults/replicationFabrics/checkConsistency/Action|Sprawdza spójność sieci szkieletowej|
|/vaults/replicationFabrics/DELETE|Usuń wszystkie sieci szkieletowe|
|/vaults/replicationFabrics/renewcertificate/Action||
|/vaults/replicationFabrics/deployProcessServerImage/Action|Wdróż obraz serwera przetwarzania|
|/vaults/replicationFabrics/reassociateGateway/Action|Ponownie Skojarz bramy|
|/ Magazyny/replicationFabrics/replicationRecoveryServicesProviders /<br>odczytywanie|Przeczytaj wszystkich dostawców usług odzyskiwania|
|/ Magazyny/replicationFabrics/replicationRecoveryServicesProviders /<br>Usuń/działania|Usuń dostawcę usług odzyskiwania|
|/ Magazyny/replicationFabrics/replicationRecoveryServicesProviders /<br>usuń|Usuwanie wszystkich dostawców usług odzyskiwania|
|/ Magazyny/replicationFabrics/replicationRecoveryServicesProviders /<br>refreshProvider/działania|Odśwież dostawcę|
|/vaults/replicationFabrics/replicationStorageClassifications/Read|Przeczytaj wszystkie klasyfikacje magazynu|
|/ Magazyny/replicationFabrics/replicationStorageClassifications /<br>replicationStorageClassificationMappings/Odczyt|Wszelkie mapowania klasyfikacji magazynu do odczytu|
|/ Magazyny/replicationFabrics/replicationStorageClassifications /<br>replicationStorageClassificationMappings/zapisu|Utwórz lub zaktualizuj wszystkie mapowania klasyfikacji magazynu|
|/ Magazyny/replicationFabrics/replicationStorageClassifications /<br>replicationStorageClassificationMappings/usuwania|Usuń wszelkie mapowania klasyfikacji magazynu|
|/vaults/replicationFabrics/replicationvCenters/Read|Wszystkie zadania do odczytu|
|/vaults/replicationFabrics/replicationvCenters/Write|Utwórz lub zaktualizuj wszystkie zadania|
|/vaults/replicationFabrics/replicationvCenters/DELETE|Usuń wszystkie zadania|
|/vaults/replicationFabrics/replicationNetworks/Read|Przeczytaj żadnych sieci|
|/ Magazyny/replicationFabrics/replicationNetworks /<br>replicationNetworkMappings/Odczyt|Przeczytaj wszelkie mapowania sieci|
|/ Magazyny/replicationFabrics/replicationNetworks /<br>replicationNetworkMappings/zapisu|Utwórz lub zaktualizuj wszystkie mapowania sieci|
|/ Magazyny/replicationFabrics/replicationNetworks /<br>replicationNetworkMappings/usuwania|Usuń wszelkie mapowania sieci|
|/ Magazyny/replicationFabrics/replicationProtectionContainers /<br>odczytywanie|Wszystkie kontenery ochrony|
|/ Magazyny/replicationFabrics/replicationProtectionContainers /<br>discoverProtectableItem/działania|Odkryj element chronione|
|/ Magazyny/replicationFabrics/replicationProtectionContainers /<br>zapisu|Utwórz lub zaktualizuj kontenerach ochrony|
|/ Magazyny/replicationFabrics/replicationProtectionContainers /<br>Usuń/działania|Usunięcie kontenera ochrony|
|/ Magazyny/replicationFabrics/replicationProtectionContainers /<br>switchprotection/działania|Kontener ochrony przełącznika|
|/ Magazyny/replicationFabrics/replicationProtectionContainers /<br>replicationProtectableItems/Odczyt|Wszystkie chronione elementy do odczytu|
|/ Magazyny/replicationFabrics/replicationProtectionContainers /<br>replicationProtectionContainerMappings/Odczyt|Przeczytaj mapowań kontenera ochrony|
|/ Magazyny/replicationFabrics/replicationProtectionContainers /<br>replicationProtectionContainerMappings/zapisu|Utwórz lub zaktualizuj mapowań kontenera ochrony|
|/ Magazyny/replicationFabrics/replicationProtectionContainers /<br>replicationProtectionContainerMappings/usuwanie/działania|Usuń mapowanie kontenera ochrony|
|/ Magazyny/replicationFabrics/replicationProtectionContainers /<br>replicationProtectionContainerMappings/usuwania|Usuwanie mapowań kontenera ochrony|
|/ Magazyny/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems/Odczyt|Wszystkie chronione elementy do odczytu|
|/ Magazyny/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems/zapisu|Utwórz lub zaktualizuj wszystkie chronione elementy.|
|/ Magazyny/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems/usuwania|Usuń wszystkie chronione elementy|
|/ Magazyny/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems/usuwanie/działania|Usuwanie chronionego elementu|
|/ Magazyny/replicationFabrics/replicationProtectionContainers /<br>Akcja replicationProtectedItems/plannedFailover|Planowany tryb Failover|
|/ Magazyny/replicationFabrics/replicationProtectionContainers /<br>Akcja replicationProtectedItems/unplannedFailover|Tryb failover|
|/ Magazyny/replicationFabrics/replicationProtectionContainers /<br>Akcja replicationProtectedItems/testFailover|Testowanie trybu Failover|
|/ Magazyny/replicationFabrics/replicationProtectionContainers /<br>Akcja replicationProtectedItems/testFailoverCleanup|Wyczyszczenia testu pracy awaryjnej|
|/ Magazyny/replicationFabrics/replicationProtectionContainers /<br>Akcja replicationProtectedItems/failoverCommit|Zatwierdzanie trybu failover|
|/ Magazyny/replicationFabrics/replicationProtectionContainers /<br>Akcja replicationProtectedItems/ponownej ochrony|Włącz ponownie ochronę chronionego elementu|
|/ Magazyny/replicationFabrics/replicationProtectionContainers /<br>Akcja replicationProtectedItems/updateMobilityService|Aktualizacja usługi mobilności|
|/ Magazyny/replicationFabrics/replicationProtectionContainers /<br>Akcja replicationProtectedItems/repairReplication|Naprawa replikacji|
|/ Magazyny/replicationFabrics/replicationProtectionContainers /<br>Akcja replicationProtectedItems/applyRecoveryPoint|Zastosuj punktu odzyskiwania|
|/ Magazyny/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems recoveryPoints/odczytu|Przeczytaj punktów odzyskiwania replikacji|
|/vaults/replicationPolicies/Read|Wszystkie zasady do odczytu|
|/vaults/replicationPolicies/Write|Utwórz lub zaktualizuj wszystkie zasady|
|/vaults/replicationPolicies/DELETE|Usuń wszystkie zasady|
|/vaults/replicationRecoveryPlans/Read|Wszystkie plany odzyskiwania do odczytu|
|/vaults/replicationRecoveryPlans/Write|Utwórz lub zaktualizuj wszystkie plany odzyskiwania|
|/vaults/replicationRecoveryPlans/DELETE|Usuń wszystkie plany odzyskiwania|
|/vaults/replicationRecoveryPlans/plannedFailover/Action|Plan odzyskiwania usługi planowanego trybu Failover|
|/vaults/replicationRecoveryPlans/unplannedFailover/Action|Plan odzyskiwania trybu failover|
|/vaults/replicationRecoveryPlans/testFailover/Action|Plan odzyskiwania trybu Failover testu|
|/vaults/replicationRecoveryPlans/testFailoverCleanup/Action|Plan odzyskiwania testu czyszczenia trybu Failover|
|/vaults/replicationRecoveryPlans/failoverCommit/Action|Zatwierdzanie trybu failover planu odzyskiwania|
|/vaults/replicationRecoveryPlans/reProtect/Action|Włącz ponownie ochronę planu odzyskiwania|
|Magazyny/extendedInformation/odczytu|Operacja Pobierz rozszerzone informacje pobiera rozszerzone informacje o obiekcie reprezentujące zasób platformy Azure typu „magazyn”|
|/ Magazyny/extendedInformation/zapisu|Operacja Pobierz rozszerzone informacje pobiera rozszerzone informacje o obiekcie reprezentujące zasób platformy Azure typu „magazyn”|
|/ Magazyny/extendedInformation/usuwania|Operacja Pobierz rozszerzone informacje pobiera rozszerzone informacje o obiekcie reprezentujące zasób platformy Azure typu „magazyn”|
|Magazyny/backupManagementMetaData/odczytu|Zwraca metadane zarządzania kopiami zapasowymi magazynu usług Recovery Services.|
|Magazyny/backupProtectionContainers/odczytu|Zwraca wszystkie kontenery należących do subskrypcji|
|Magazyny/backupFabrics/operationResults/odczytu|Zwraca stan operacji|
|Magazyny/backupFabrics/protectionContainers/odczytu|Zwraca wszystkich zarejestrowanych kontenerów|
|/ Magazyny/backupFabrics/protectionContainers /<br>operationResults/Odczyt|Pobiera wynik operacji wykonanej na kontenerze ochrony.|
|/ Magazyny/backupFabrics/protectionContainers /<br>protectedItems/Odczyt|Zwraca szczegóły obiektu elemencie chroniony|
|/ Magazyny/backupFabrics/protectionContainers /<br>protectedItems/zapisu|Tworzenie kopii zapasowej elementu chronione|
|/ Magazyny/backupFabrics/protectionContainers /<br>protectedItems/usuwania|Usuwa chronionego elementu|
|/ Magazyny/backupFabrics/protectionContainers /<br>Akcja protectedItems/tworzenia kopii zapasowej|Wykonuje kopię zapasową elementu chronionego.|
|/ Magazyny/backupFabrics/protectionContainers /<br>protectedItems operationResults/odczytu|Pobiera wynik operacji wykonanej na elementach chronionych.|
|/ Magazyny/backupFabrics/protectionContainers /<br>protectedItems operationStatus/odczytu|Zwraca stan operacji wykonanej na elementach chronionych.|
|/ Magazyny/backupFabrics/protectionContainers /<br>protectedItems recoveryPoints/odczytu|Pobierz punkty odzyskiwania dla elementów chronionych.|
|/ Magazyny/backupFabrics/protectionContainers /<br>protectedItems/recoveryPoints /<br>Akcja/przywracania|Przywróć punkty odzyskiwania dla elementów chronionych.|
|/ Magazyny/backupFabrics/protectionContainers /<br>protectedItems/recoveryPoints /<br>provisionInstantItemRecovery/działania|Odzyskiwanie elementów błyskawicznych udostępniania dla chronionego elementu|
|/ Magazyny/backupFabrics/protectionContainers /<br>protectedItems/recoveryPoints /<br>revokeInstantItemRecovery/działania|Odwołanie elementu błyskawicznych odzyskiwania dla chronionego elementu|
|Magazyny/użycia/odczytu|Zwraca szczegóły użycia magazynu usług Recovery Services.|
|/vaults/Usages/Read|Przeczytaj również użycia magazynu|
|/ Magazyny/certyfikatów/zapisu|Operacja aktualizacji zasobów certyfikatu aktualizuje certyfikat poświadczeń zasobów/magazynu.|
|Magazyny/tokenInfo/odczytu|Zwraca token informacje magazynie usług odzyskiwania.|
|/vaults/replicationAlertSettings/Read|Wszystkie ustawienia alertów do odczytu|
|/vaults/replicationAlertSettings/Write|Utwórz lub zaktualizuj ustawienia alertów|
|Magazyny/backupOperations/odczytu|Zwraca operacji tworzenia kopii zapasowej odzyskiwania stanu usługi magazynu.|
|Magazyny/storageConfig/odczytu|Zwraca konfigurację magazynu dla odzyskiwania usług magazynu.|
|/ Magazyny/storageConfig/zapisu|Aktualizacje konfiguracji magazynu dla odzyskiwania usług magazynu.|
|Magazyny/backupUsageSummaries/odczytu|Zwraca podsumowania dla chronionych serwerów i chronione elementy usług odzyskiwania.|
|Magazyny/backupProtectedItems/odczytu|Zwraca listę wszystkich elementów chronionych.|
|Magazyny/backupconfig/vaultconfig/odczytu|Zwraca konfigurację Recovery usług magazynu.|
|/ Magazyny/backupconfig/vaultconfig/zapisu|Konfigurowanie aktualizacji odzyskiwania usług magazynu.|
|/ Magazyny/registeredIdentities/zapisu|Operacji zarejestrować kontenera usług można używać do rejestrowania kontener z usługą odzyskiwania.|
|Magazyny/registeredIdentities/odczytu|Kontenery pobrać operacji można używać Pobierz kontenery zarejestrowanych dla każdego zasobu.|
|/ Magazyny/registeredIdentities/usuwania|Operacja wyrejestrować kontenera można wyrejestrować kontenera.|
|Magazyny/registeredIdentities/operationResults/odczytu|Wyniki operacji Get operacji można używać Pobierz stan operacji i wynik operacji asynchronicznie przesłane|
|/vaults/replicationJobs/Read|Wszystkie zadania do odczytu|
|/vaults/replicationJobs/Cancel/Action|Anuluj zadanie|
|/vaults/replicationJobs/restart/Action|Uruchom ponownie zadanie|
|/vaults/replicationJobs/resume/Action|Wznów zadanie|
|Magazyny/backupPolicies/odczytu|Zwraca wszystkie zasady ochrony|
|/ Magazyny/backupPolicies/zapisu|Tworzy zasady ochrony|
|/ Magazyny/backupPolicies/usuwania|Usuń zasady ochrony|
|Magazyny/backupPolicies/operationResults/odczytu|Pobierz wyniki operacji zasad.|
|Magazyny/backupPolicies/operationStatus/odczytu|Pobierz stan operacji zasad.|
|Magazyny/vaultTokens/odczytu|Magazyn Token operacji można uzyskać tokenu magazynu dla operacji poziomu wewnętrznej bazy danych magazynu.|
|Magazyny/monitoringConfigurations/notificationConfiguration/odczytu|Pobiera konfigurację powiadomień magazynu usług odzyskiwania.|
|Magazyny/backupJobs/odczytu|Zwraca wszystkie obiekty zadania|
|/ Magazyny/backupJobs / / akcję anulowania|Anulowanie zadania|
|Magazyny/backupJobs/operationResults/odczytu|Zwraca wynik operacji zadania.|
|/Locations/allocateStamp/Action|AllocateStamp jest używane przez usługę operacji wewnętrznych|
|/Locations/allocatedStamp/Read|Operacja GetAllocatedStamp to operacja wewnętrzna używana przez usługę|

## <a name="microsoftrelay"></a>Microsoft.Relay

| Operacja | Opis |
|---|---|
|/ checkNamespaceAvailability/działania|Sprawdzanie dostępności przestrzeni nazw w podanej subskrypcji.|
|/ register/działania|Rejestruje subskrypcję dostawcy zasobów usługi Relay i włącza funkcję tworzenia zasobów usługi Relay|
|/ przestrzenie nazw/zapisu|Utwórz zasób Namespace i zaktualizuj jego właściwości. Znaczniki i stan Namespace są właściwości, które mogą być aktualizowane.|
|/Namespaces/Read|Pobierz listę opisów zasobów przestrzeni nazw|
|przestrzenie nazw/Delete|Usuń zasób przestrzeni nazw|
|/Namespaces/authorizationRules/Write|Tworzenie reguł autoryzacji z poziomu Namespace i aktualizowanie jej właściwości. Można zaktualizować reguły autoryzacji uprawnienia, serwer podstawowy i klucze pomocnicze.|
|/Namespaces/authorizationRules/DELETE|Usuń regułę autoryzacji Namespace. Nie można usunąć reguły autoryzacji Namespace domyślnej. |
|/Namespaces/authorizationRules/listkeys/Action|Pobierz parametry połączenia z przestrzenią nazw|
|/Namespaces/HybridConnections/Write|Tworzenie lub aktualizacja HybridConnection właściwości.|
|/Namespaces/HybridConnections/Read|Pobierz listę opisów zasobów HybridConnection|
|/Namespaces/HybridConnections/DELETE|Operacja usunąć HybridConnection zasobu|
|/Namespaces/HybridConnections/authorizationRules/Write|Tworzenie reguł autoryzacji HybridConnection i aktualizowanie jej właściwości. Można zaktualizować reguły autoryzacji uprawnienia, serwer podstawowy i klucze pomocnicze.|
|/Namespaces/HybridConnections/authorizationRules/DELETE|Operację można usunąć reguły autoryzacji HybridConnection|
|/Namespaces/HybridConnections/authorizationRules/listkeys/Action|Pobierz ciąg połączenia HybridConnection|
|/Namespaces/WcfRelays/Write|Tworzenie lub aktualizacja WcfRelay właściwości.|
|/Namespaces/WcfRelays/Read|Pobierz listę opisów zasobów WcfRelay|
|/Namespaces/WcfRelays/DELETE|Operacja usunąć WcfRelay zasobu|
|/Namespaces/WcfRelays/authorizationRules/Write|Tworzenie reguł autoryzacji WcfRelay i aktualizowanie jej właściwości. Można zaktualizować reguły autoryzacji uprawnienia, serwer podstawowy i klucze pomocnicze.|
|/Namespaces/WcfRelays/authorizationRules/DELETE|Operację można usunąć reguły autoryzacji WcfRelay|
|/Namespaces/WcfRelays/authorizationRules/listkeys/Action|Pobierz ciąg połączenia WcfRelay|

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth

| Operacja | Opis |
|---|---|
|AvailabilityStatuses/odczytu|Pobiera stany dostępności wszystkich zasobów w określonym zakresie|
|AvailabilityStatuses/bieżącego/odczytu|Pobiera stan dostępności określonego zasobu|

## <a name="microsoftresources"></a>Microsoft.Resources

| Operacja | Opis |
|---|---|
|/ checkResourceName/działania|Sprawdź poprawność nazwy zasobu.|
|/Providers/Read|Pobierz listę dostawców.|
|/Subscriptions/Read|Pobiera listę subskrypcji.|
|/Subscriptions/operationresults/Read|Pobierz wyniki operacji subskrypcji.|
|/Subscriptions/Providers/Read|Pobiera lub wyświetla ich listę dostawców zasobów.|
|/Subscriptions/tagNames/Read|Pobiera lub wyświetla ich listę tagi subskrypcji.|
|/Subscriptions/tagNames/Write|Dodaje tag subskrypcji.|
|/Subscriptions/tagNames/DELETE|Usuwa tagu subskrypcji.|
|/Subscriptions/tagNames/tagValues/Read|Pobiera lub wyświetla ich listę wartości tagów subskrypcji.|
|/Subscriptions/tagNames/tagValues/Write|Dodaje wartości tagów subskrypcji.|
|/Subscriptions/tagNames/tagValues/DELETE|Usuwa wartość tagu subskrypcji.|
|/Subscriptions/Resources/Read|Pobiera zasoby subskrypcji.|
|/Subscriptions/resourceGroups/Read|Pobiera lub wyświetla listę grup zasobów.|
|/Subscriptions/resourceGroups/Write|Tworzy lub aktualizuje grupę zasobów.|
|/Subscriptions/resourceGroups/DELETE|Usuwa grupę zasobów i wszystkie jego zasoby.|
|/Subscriptions/resourceGroups/moveResources/Action|Przenosi zasoby z jednej grupy zasobów.|
|/Subscriptions/resourceGroups/validateMoveResources/Action|Waliduj przeniesienie zasobów między grupami zasobów do innego.|
|/Subscriptions/resourcegroups/Resources/Read|Pobiera zasoby grupy zasobów.|
|/Subscriptions/resourcegroups/Deployments/Read|Pobiera lub wyświetla listę wdrożeń.|
|/Subscriptions/resourcegroups/Deployments/Write|Tworzy lub aktualizuje wdrożenie.|
|/Subscriptions/resourcegroups/Deployments/operationstatuses/Read|Pobiera lub wyświetla stany operacji wdrażania.|
|/Subscriptions/resourcegroups/Deployments/Operations/Read|Pobiera lub wyświetla ich listę operacji wdrażania.|
|/Subscriptions/Locations/Read|Pobiera listę obsługiwanych lokalizacji.|
|/Links/Read|Pobiera lub zawiera linki do zasobów.|
|/ łącza/zapisu|Tworzy lub aktualizuje link do zasobu.|
|/Links/DELETE|Usuwa link do zasobu.|
|/tenants/Read|Pobiera listę dzierżawców.|
|/Resources/Read|Pobierz listę zasobów zgodnie z filtrami.|
|/Deployments/Read|Pobiera lub wyświetla listę wdrożeń.|
|/ wdrożeń/zapisu|Tworzy lub aktualizuje wdrożenie.|
|/Deployments/DELETE|Usuwa wdrożenia.|
|/Deployments/Cancel/Action|Anuluje wdrożenia.|
|/Deployments/Validate/Action|Weryfikuje wdrożenia.|
|/Deployments/Operations/Read|Pobiera lub wyświetla ich listę operacji wdrażania.|

## <a name="microsoftscheduler"></a>Microsoft.Scheduler

| Operacja | Opis |
|---|---|
|/jobcollections/Read|Pobierz kolekcję zadań|
|/ kolekcjach zadań/zapisu|Tworzy lub aktualizuje kolekcję zadań.|
|/jobcollections/DELETE|Usuwa kolekcję zadań.|
|/jobcollections/enable/Action|Włącza kolekcję zadań.|
|/jobcollections/disable/Action|Wyłącza kolekcję zadań.|
|/jobcollections/Jobs/Read|Pobiera zadanie.|
|/jobcollections/Jobs/Write|Tworzy lub aktualizuje zadanie.|
|/jobcollections/Jobs/DELETE|Usuwa zadanie.|
|/jobcollections/Jobs/Run/Action|Uruchamia zadanie.|
|/jobcollections/Jobs/generateLogicAppDefinition/Action|Generuje definicję aplikacji logiki na podstawie zadania usługi Scheduler.|
|/jobcollections/Jobs/jobhistories/Read|Pobiera historię zadania.|

## <a name="microsoftsearch"></a>Microsoft.Search

| Operacja | Opis |
|---|---|
|/ register/działania|Rejestruje subskrypcję dostawcy zasobów wyszukiwania i umożliwia tworzenie usługi wyszukiwania.|
|/ checkNameAvailability/działania|Służy do sprawdzania dostępności nazwy usługi.|
|/ searchServices/zapisu|Tworzy lub aktualizuje usługę wyszukiwania.|
|/searchServices/Read|Odczytuje usługi wyszukiwania.|
|/searchServices/DELETE|Usuwa usługę wyszukiwania.|
|/searchServices/Start/Action|Uruchamia usługę wyszukiwania.|
|/searchServices/Stop/Action|Zatrzymuje usługę wyszukiwania.|
|/searchServices/listAdminKeys/Action|Odczytuje kluczy administratora.|
|/searchServices/regenerateAdminKey/Action|Generuje klucz administratora.|
|/searchServices/createQueryKey/Action|Tworzy klucza zapytania.|
|/searchServices/queryKey/Read|Odczytuje klucze zapytania.|
|/searchServices/queryKey/DELETE|Usuwa klucza zapytania.|

## <a name="microsoftsecurity"></a>Microsoft.Security

| Operacja | Opis |
|---|---|
|/jitNetworkAccessPolicies/Read|Pobiera zasady dostępu do sieci w czasie|
|/ jitNetworkAccessPolicies/zapisu|Tworzy nowe zasady dostępu do sieci w czasie, lub aktualizuje istniejący zestaw|
|/jitNetworkAccessPolicies/initiate/Action|Inicjuje zasady dostępu do sieci w czasie|
|/securitySolutionsReferenceData/Read|Pobiera dane referencyjne rozwiązań zabezpieczeń|
|/securityStatuses/Read|Pobiera stany kondycji zabezpieczeń zasobów platformy Azure|
|/webApplicationFirewalls/Read|Pobiera zapory aplikacji sieci web|
|/ webApplicationFirewalls/zapisu|Nowa Zapora aplikacji sieci web tworzy lub aktualizuje istniejący zestaw|
|/webApplicationFirewalls/DELETE|Usuwa zapory aplikacji sieci web|
|/securitySolutions/Read|Pobiera rozwiązań zabezpieczeń|
|/ securitySolutions/zapisu|Tworzy nowe rozwiązanie zabezpieczeń lub aktualizuje istniejący zestaw|
|/securitySolutions/DELETE|Usuwa rozwiązania zabezpieczeń|
|/Tasks/Read|Pobiera wszystkie zalecenia dotyczące zabezpieczeń dostępne|
|/Tasks/dismiss/Action|Odrzuć zalecana ze względów bezpieczeństwa|
|/Tasks/Activate/Action|Aktywuj zalecana ze względów bezpieczeństwa|
|/Policies/Read|Pobiera zasady zabezpieczeń|
|/ zasady/zapisu|Aktualizacje zasad zabezpieczeń|
|/applicationWhitelistings/Read|Pobiera whitelistings aplikacji|
|/ applicationWhitelistings/zapisu|Tworzy nowy listę dozwolonych aplikacji podobnej lub aktualizuje istniejący zestaw|

## <a name="microsoftservermanagement"></a>Microsoft.ServerManagement

| Operacja | Opis |
|---|---|
|/ subscriptions/zapisu|Tworzy lub aktualizuje subskrypcji|
|/ bram/zapisu|Tworzy lub aktualizuje bramy|
|/Gateways/DELETE|Usuwa bramę|
|/Gateways/Read|Pobiera bramy|
|/Gateways/regenerateprofile/Action|Generuje ponownie profilu bramy|
|/Gateways/upgradetolatest/Action|Uaktualnia bramy do najnowszej wersji|
|/ węzły/zapisu|Tworzy lub aktualizuje węzła|
|/nodes/DELETE|Usuwa węzła|
|/nodes/Read|Pobiera węzeł|
|/ sesje/zapisu|Tworzy lub aktualizuje sesji|
|/Sessions/Read|Pobiera sesję|
|/Sessions/DELETE|Usuwa sesssion|

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

| Operacja | Opis |
|---|---|
|/ checkNameAvailability/działania|Sprawdzanie dostępności przestrzeni nazw w podanej subskrypcji.|
|/ register/działania|Rejestruje subskrypcję dostawcy zasobów usługi ServiceBus i włącza funkcję tworzenia zasobów usługi ServiceBus|
|/ przestrzenie nazw/zapisu|Utwórz zasób Namespace i zaktualizuj jego właściwości. Znaczniki i stan Namespace są właściwości, które mogą być aktualizowane.|
|/Namespaces/Read|Pobierz listę opisów zasobów przestrzeni nazw|
|przestrzenie nazw/Delete|Usuń zasób przestrzeni nazw|
|/Namespaces/metricDefinitions/Read|Pobierz listę metryki Namespace opisów zasobów|
|/Namespaces/authorizationRules/Write|Tworzenie reguł autoryzacji z poziomu Namespace i aktualizowanie jej właściwości. Można zaktualizować reguły autoryzacji uprawnienia, serwer podstawowy i klucze pomocnicze.|
|/Namespaces/authorizationRules/Read|Pobierz listę opisów reguł autoryzacji przestrzeni nazw.|
|/Namespaces/authorizationRules/DELETE|Usuń regułę autoryzacji Namespace. Nie można usunąć reguły autoryzacji Namespace domyślnej. |
|/Namespaces/authorizationRules/listkeys/Action|Pobierz parametry połączenia z przestrzenią nazw|
|/Namespaces/authorizationRules/regenerateKeys/Action|Ponownie wygeneruj klucz podstawowy lub pomocniczy dla zasobu|
|/Namespaces/diagnosticSettings/Read|Pobierz listę Namespace ustawień diagnostycznych zasobu opisów|
|/Namespaces/diagnosticSettings/Write|Pobierz listę Namespace ustawień diagnostycznych zasobu opisów|
|/Namespaces/Queues/Write|Utwórz lub właściwości kolejki aktualizacji.|
|/Namespaces/Queues/Read|Pobierz listę opisów zasobów kolejki|
|/Namespaces/Queues/DELETE|Operację można usunąć kolejki zasobów|
|/Namespaces/Queues/authorizationRules/Write|Tworzenie reguł autoryzacji kolejki i aktualizowanie jej właściwości. Można zaktualizować reguły autoryzacji uprawnienia, serwer podstawowy i klucze pomocnicze.|
|/Namespaces/Queues/authorizationRules/Read| Pobierz listę reguł autoryzacji kolejki|
|/Namespaces/Queues/authorizationRules/DELETE|Operację można usunąć reguły autoryzacji kolejki|
|/Namespaces/Queues/authorizationRules/listkeys/Action|Pobrać parametry połączenia do kolejki|
|/Namespaces/Queues/authorizationRules/regenerateKeys/Action|Ponownie wygeneruj klucz podstawowy lub pomocniczy dla zasobu|
|/Namespaces/logDefinitions/Read|Pobierz listę dzienników Namespace opisów zasobów|
|/Namespaces/topics/Write|Tworzenie lub aktualizacja tematu właściwości.|
|/Namespaces/topics/Read|Pobierz listę opisów zasobów tematu|
|/Namespaces/topics/DELETE|Operację można usunąć zasobu tematu|
|/Namespaces/topics/authorizationRules/Write|Tworzenie reguł autoryzacji tematu i aktualizowanie jej właściwości. Można zaktualizować reguły autoryzacji uprawnienia, serwer podstawowy i klucze pomocnicze.|
|/Namespaces/topics/authorizationRules/Read| Pobierz listę reguł autoryzacji tematu|
|/Namespaces/topics/authorizationRules/DELETE|Operację można usunąć reguły autoryzacji w temacie|
|/Namespaces/topics/authorizationRules/listkeys/Action|Pobrać parametry połączenia do tematu|
|/Namespaces/topics/authorizationRules/regenerateKeys/Action|Ponownie wygeneruj klucz podstawowy lub pomocniczy dla zasobu|
|/Namespaces/topics/Subscriptions/Write|Tworzenie lub aktualizacja TopicSubscription właściwości.|
|/Namespaces/topics/Subscriptions/Read|Pobierz listę opisów zasobów TopicSubscription|
|/Namespaces/topics/Subscriptions/DELETE|Operacja usunąć TopicSubscription zasobu|
|/Namespaces/topics/Subscriptions/Rules/Write|Tworzenie lub aktualizacja reguły właściwości.|
|/Namespaces/topics/Subscriptions/Rules/Read|Pobierz listę reguł opisów zasobów|
|/Namespaces/topics/Subscriptions/Rules/DELETE|Operację można usunąć zasobu reguły|

## <a name="microsoftsql"></a>Microsoft.Sql

| Operacja | Opis |
|---|---|
|/Servers/Read|Zwraca listę serwerów w grupie zasobów w subskrypcji|
|/ serwery/zapisu|Utwórz nowy serwer lub zmodyfikować właściwości istniejącego serwera w grupie zasobów w subskrypcji|
|/Servers/DELETE|Usuń serwer i wszystkie zawarte bazy danych i pul elastycznych|
|/Servers/Import/Action|Utwórz nową bazę danych na serwerze i wdrażanie schemat i dane z pakietu DacPac|
|/Servers/Upgrade/Action|Włącz nowe funkcje dostępne w najnowszej wersji serwera i określ mapy konwersji wersji bazy danych|
|/Servers/VulnerabilityAssessmentScans/Action|Wykonywanie skanowania serwera w celu oceny luk w zabezpieczeniach|
|/Servers/operationResults/Read|Operacja jest używana śledzić postępy uaktualniania serwera ze starszej wersji do nowszej|
|/Servers/operationResults/DELETE|Przerwanie uaktualniania wersji serwera w toku|
|/Servers/securityAlertPolicies/Read|Pobieranie szczegółów serwera zasad wykrywania zagrożeń skonfigurowane na danym serwerze|
|/Servers/securityAlertPolicies/Write|Zmiana serwera wykrywanie zagrożeń dla danego serwera|
|/Servers/securityAlertPolicies/operationResults/Read|Pobierz wyniki serwera zasady wykrywania zagrożeń operacji Set|
|/Servers/Administrators/Read|Pobieranie szczegółów administratora serwera|
|/Servers/Administrators/Write|Utwórz lub zaktualizuj administratora serwera|
|/Servers/Administrators/DELETE|Usuwa administratora z serwera|
|/Servers/recoverableDatabases/Read|Ta operacja jest używana do odzyskiwania po awarii na żywo bazy danych, aby przywrócić ostatnią znaną dobrą punktu kopii zapasowej bazy danych. Zwraca informacje o ostatniej dobrej kopii zapasowej, ale faktycznie nie go przywrócić bazę danych.|
|/Servers/serviceObjectives/Read|Pobieranie listy celów poziomu usługi (znanej także jako warstwy wydajności) dostępnych na danym serwerze|
|/Servers/firewallRules/Read|Pobieranie szczegółów reguły zapory serwera|
|/Servers/firewallRules/Write|Utwórz lub zaktualizuj reguły zapory serwera, która określa zakres adresów IP, może nawiązać połączenia z serwerem|
|/Servers/firewallRules/DELETE|Usuń regułę zapory z serwera|
|/Servers/administratorOperationResults/Read|Pobiera wyniki operacji administratora serwera|
|/Servers/recommendedElasticPools/Read|Pobierz zalecenia dla elastycznych pul baz danych na zmniejszenie kosztów oraz pomagają w poprawieniu wydajności na podstawie wykorzystania zasobów historica|
|/Servers/recommendedElasticPools/Metrics/Read|Pobrać metryki dla zalecane elastyczne pule baz danych dla danego serwera|
|/Servers/recommendedElasticPools/Databases/Read|Pobrać baz danych, które mają zostać dodane do zalecane elastyczne pule baz danych dla danego serwera|
|/Servers/elasticPools/Read|Pobieranie szczegółów elastycznej puli baz danych na danym serwerze|
|/Servers/elasticPools/Write|Utwórz nową lub zmodyfikować właściwości istniejącej puli elastycznej bazy danych|
|/Servers/elasticPools/DELETE|Usuń istniejącej puli elastycznej bazy danych|
|/Servers/elasticPools/operationResults/Read|Pobieranie szczegółów w operacji puli elastycznej bazy danych|
|/Servers/elasticPools/Providers/Microsoft.Insights/<br>metricDefinitions/Odczyt|Zwracane typy metryk, które są dostępne dla elastycznych pul baz danych|
|/Servers/elasticPools/Providers/Microsoft.Insights/<br>diagnosticSettings/Odczyt|Pobiera ustawienie diagnostyczne dla zasobu|
|/Servers/elasticPools/Providers/Microsoft.Insights/<br>diagnosticSettings/zapisu|Tworzy lub aktualizuje ustawienie diagnostyczne dla zasobu|
|/Servers/elasticPools/Metrics/Read|Zwraca metryki użycia zasobów puli elastycznej bazy danych|
|/Servers/elasticPools/elasticPoolDatabaseActivity/Read|Pobieranie działań i szczegóły dotyczące danego bazy danych, która jest częścią puli elastycznej bazy danych|
|/Servers/elasticPools/advisors/Read|Zwraca listę doradcy jest dostępna dla puli elastycznej|
|/Servers/elasticPools/advisors/Write|Aktualizacja automatycznego wykonywania stanu usługi advisor na poziomie puli elastycznej.|
|/Servers/elasticPools/advisors/recommendedActions/Read|Zwraca listę zalecane działania advisor określony dla puli elastycznej|
|/Servers/elasticPools/advisors/recommendedActions/Write|Zastosuj akcję zalecaną dla elastycznej puli|
|/Servers/elasticPools/elasticPoolActivity/Read|Pobieranie działań i szczegółowe informacje o puli elastycznej bazy danych|
|/Servers/elasticPools/Databases/Read|Pobieranie listy i szczegółów baz danych, które są częścią puli elastycznej bazy danych na danym serwerze|
|/Servers/auditingPolicies/Read|Pobieranie szczegółów tabeli serwera domyślne zasady skonfigurowane na danym serwerze inspekcji|
|/Servers/auditingPolicies/Write|Zmień domyślny serwer tabeli inspekcji dla danego serwera|
|/Servers/disasterRecoveryConfiguration/operationResults/Read|Pobierz wyniki operacji konfiguracji odzyskiwania po awarii|
|/Servers/advisors/Read|Zwraca listę doradcy jest dostępna dla serwera|
|/Servers/advisors/Write|Aktualizacje automatyczne execute stanu usługi advisor na poziomie serwera.|
|/Servers/advisors/recommendedActions/Read|Zwraca listę zalecane działania advisor określonego serwera|
|/Servers/advisors/recommendedActions/Write|Zastosuj akcję zalecaną na serwerze|
|/Servers/Usages/Read|Zwraca limit przydziału jednostek dtu w warstwie serwera i bieżący consuption jednostek dtu w warstwie przez wszystkie bazy danych w ramach serwera|
|/Servers/elasticPoolEstimates/Read|Zwraca listę już utworzone dla tego serwera oszacowania puli elastycznej|
|/Servers/elasticPoolEstimates/Write|Tworzy nowego szacowania puli elastycznej dla listy baz danych, pod warunkiem|
|/Servers/auditingSettings/Read|Pobieranie szczegółów zasady inspekcji serwera obiektu blob skonfigurowane na danym serwerze|
|/Servers/auditingSettings/Write|Zmienianie serwera Inspekcja obiektów blob dla danego serwera|
|/Servers/auditingSettings/operationResults/Read|Pobieranie wyników inspekcji operacji zestawu zasad obiektu blob serwera|
|/Servers/backupLongTermRetentionVaults/Read|Ta operacja jest używany do pobierania magazyn przechowywania długoterminowej kopii zapasowej. Zwraca informacje o magazynie zarejestrowane na tym serwerze.|
|/Servers/backupLongTermRetentionVaults/Write|Zarejestruj w magazynie przechowywania długoterminowej kopii zapasowej|
|/Servers/restorableDroppedDatabases/Read|Pobieranie listy baz danych, które zostały usunięte na danym serwerze znajdujące się nadal w ramach zasad przechowywania. Ta operacja zwraca listę baz danych i skojarzonymi metadanymi, takich jak Data usunięcia.|
|/Servers/Databases/Read|Zwraca listę serwerów w grupie zasobów w subskrypcji|
|/Servers/Databases/Write|Utwórz nowy serwer lub zmodyfikować właściwości istniejącego serwera w grupie zasobów w subskrypcji|
|/Servers/Databases/DELETE|Usuń serwer i wszystkie zawarte bazy danych i pul elastycznych|
|/Servers/Databases/Export/Action|Utwórz nową bazę danych na serwerze i wdrażanie schemat i dane z pakietu DacPac|
|/Servers/Databases/VulnerabilityAssessmentScans/Action|Wykonywanie skanowania bazy danych w celu oceny luk w zabezpieczeniach.|
|/Servers/Databases/pause/Action|Wstrzymaj wersji bazy danych magazynu danych|
|/Servers/Databases/resume/Action|Wznów wersji bazy danych magazynu danych|
|/Servers/Databases/operationResults/Read|Operacja jest używana w celu śledzenia postępu długotrwałej operacji bazy danych, takie jak zwiększanie.|
|/Servers/Databases/replicationLinks/Read|Zwracany szczegółów dotyczących łącz replikacji dla określonej bazy danych|
|/Servers/Databases/replicationLinks/DELETE|Zakończenie relacji replikacji wymuszone i o utracie danych|
|/Servers/Databases/replicationLinks/unlink/Action|Zakończenie relacji replikacji wymuszone lub po synchronizacji z partnera|
|/Servers/Databases/replicationLinks/Failover/Action|Trybu failover po zsynchronizowaniu wszystkich zmian z serwera podstawowego udostępniania tej bazy danych w podstawowej relacji replikacji i wprowadzania zdalnego podstawowej do dodatkowej|
|/Servers/Databases/replicationLinks/forceFailoverAllowDataLoss/Action|Trybu failover natychmiast o utracie danych, co ta baza danych w podstawowej relacji replikacji i wprowadzania zdalnego podstawowej do dodatkowej|
|/Servers/Databases/replicationLinks/updateReplicationMode/Action|Tryb aktualizacji replikacji dla łącza do trybu synchronicznego lub asynchronicznego|
|/Servers/Databases/replicationLinks/operationResults/Read|Pobierz stan operacji długotrwałych w łączach replikacji bazy danych|
|/Servers/Databases/dataMaskingPolicies/Read|Pobieranie szczegółów danych maskowania zasady skonfigurowane na danym bazy danych|
|/Servers/Databases/dataMaskingPolicies/Write|Zmień zasady dla danych maskowania danych|
|/Servers/Databases/dataMaskingPolicies/Rules/Read|Pobieranie szczegółów danych maskowania regułę skonfigurowany w danej bazie danych|
|/Servers/Databases/dataMaskingPolicies/Rules/Write|Zmień dane maskowania reguły określonej bazy danych|
|/Servers/Databases/securityAlertPolicies/Read|Pobieranie szczegółów zasady wykrywania zagrożeń skonfigurowany w danej bazie danych|
|/Servers/Databases/securityAlertPolicies/Write|Zmień zasady wykrywania zagrożeń określonej bazy danych|
|/Servers/Databases/Providers/Microsoft.Insights/<br>metricDefinitions/Odczyt|Zwracane typy metryk, które są dostępne dla baz danych|
|/Servers/Databases/Providers/Microsoft.Insights/<br>diagnosticSettings/Odczyt|Pobiera ustawienie diagnostyczne dla zasobu|
|/Servers/Databases/Providers/Microsoft.Insights/<br>diagnosticSettings/zapisu|Tworzy lub aktualizuje ustawienie diagnostyczne dla zasobu|
|/Servers/Databases/Providers/Microsoft.Insights/<br>logDefinitions/Odczyt|Pobiera dostępne dzienniki dla baz danych|
|/Servers/Databases/topQueries/Read|Zwraca zagregowane statystyk czasu wykonywania dla wybranego zapytania w wybranym okresie|
|/Servers/Databases/topQueries/queryText/Read|Zwraca tekst języka Transact-SQL dla wybranego zapytania o identyfikatorze|
|/Servers/Databases/topQueries/statistics/Read|Zwraca zagregowane statystyk czasu wykonywania dla wybranego zapytania w wybranym okresie|
|/Servers/Databases/connectionPolicies/Read|Pobieranie szczegółów zasady połączeń skonfigurowany w danej bazie danych|
|/Servers/Databases/connectionPolicies/Write|Zmień zasady połączenia dla danego bazy danych|
|/Servers/Databases/Metrics/Read|Zwraca metryki użycia zasobów bazy danych|
|/Servers/Databases/auditRecords/Read|Pobieranie rekordów inspekcji bazy danych obiektów blob|
|/Servers/Databases/transparentDataEncryption/Read|Pobieranie stanu i szczegółowe informacje o funkcji zabezpieczeń szyfrowania danych przezroczysty określonej bazy danych|
|/Servers/Databases/transparentDataEncryption/Write|Włącz lub wyłącz przezroczystego szyfrowania danych określonej bazy danych|
|/Servers/Databases/transparentDataEncryption/operationResults/Read|Pobieranie stanu i szczegółowe informacje o funkcji zabezpieczeń szyfrowania danych przezroczysty określonej bazy danych|
|/Servers/Databases/auditingPolicies/Read|Pobieranie szczegółów tabeli zasad inspekcji skonfigurowany w danej bazie danych|
|/Servers/Databases/auditingPolicies/Write|Zmień zasady inspekcji tabeli określonej bazy danych|
|/Servers/Databases/dataWarehouseQueries/Read|Zwraca informacje zapytania dystrybucji magazynu danych dla Identyfikatora wybranego zapytania|
|/ serwery baz danych/dataWarehouseQueries /<br>dataWarehouseQuerySteps/Odczyt|Zwraca informacje krok zapytań rozproszonych zapytania magazynu danych dla Identyfikatora wybrany krok|
|/Servers/Databases/serviceTierAdvisors/Read|Zwraca sugestii dotyczących skalowania bazy danych w górę lub w dół w oparciu statystyk wykonywania zapytań do zwiększenia wydajności i zmniejszenie kosztów|
|/Servers/Databases/advisors/Read|Zwraca listę doradcy jest dostępna dla bazy danych|
|/Servers/Databases/advisors/Write|Aktualizacja automatycznego wykonywania stanu usługi advisor na poziomie bazy danych.|
|/Servers/Databases/advisors/recommendedActions/Read|Zwraca listę zalecane działania advisor określonej bazy danych|
|/Servers/Databases/advisors/recommendedActions/Write|Zastosuj zalecaną akcję w bazie danych|
|/Servers/Databases/Usages/Read|Zwraca rozmiar maksymalny bazy danych, która może być osiągnięta i bieżący rozmiar zajmowane przez|
|/Servers/Databases/queryStore/Read|Zwraca bieżące wartości ustawień magazyn zapytań dla bazy danych|
|/Servers/Databases/queryStore/Write|Aktualizuje ustawienia magazynu zapytań dla bazy danych|
|/Servers/Databases/auditingSettings/Read|Pobieranie szczegółów obiektu blob zasad inspekcji skonfigurowany w danej bazie danych|
|/Servers/Databases/auditingSettings/Write|Zmień zasady inspekcji obiektu blob określonej bazy danych|
|/Servers/Databases/schemas/Tables/recommendedIndexes/Read|Pobieranie listy zalecenia dotyczące indeksu dla bazy danych|
|/Servers/Databases/schemas/Tables/recommendedIndexes/Write|Zastosuj zalecenie dotyczące indeksu|
|/Servers/Databases/schemas/Tables/Columns/Read|Pobieranie listy kolumn tabeli.|
|/Servers/Databases/missingindexes/Read|Zwraca sugestie dotyczące indeksy bazy danych, aby utworzyć, zmodyfikować lub usunąć w celu poprawy wydajności zapytań|
|/Servers/Databases/missingindexes/Write|Użyj bazy danych zalecenie dotyczące indeksu w określonej bazy danych|
|/Servers/Databases/importExportOperationResults/Read|Zwraca szczegółowe informacje o importu bazy danych lub operacji eksportowania z DacPac znajdujących się na koncie magazynu|
|/Servers/importExportOperationResults/Read|Zwraca listę w szczegółowych informacjach dotyczących operacji importu bazy danych z konta magazynu na danym serwerze|

## <a name="microsoftstorage"></a>Microsoft.Storage

| Operacja | Opis |
|---|---|
|/ register/działania|Rejestruje subskrypcję dostawcy zasobów magazynu i umożliwia tworzenie kont magazynu.|
|/checknameavailability/Read|Sprawdza, czy nazwa konta jest prawidłowa i czy nie jest używana.|
|/ storageAccounts/zapisu|Tworzy konto magazynu z podanymi parametrami lub aktualizuje właściwości bądź tagi albo dodaje niestandardową domenę dla podanego konta magazynu.|
|/storageAccounts/DELETE|Usuwa istniejące konto magazynu.|
|/storageAccounts/listkeys/Action|Zwraca klucze dostępu dla podanego konta magazynu.|
|/storageAccounts/regeneratekey/Action|Generuje ponownie klucze dostępu dla podanego konta magazynu.|
|/storageAccounts/Read|Zwraca listę kont magazynu bądź pobiera właściwości dla podanego konta magazynu.|
|/storageAccounts/listAccountSas/Action|Zwraca token sygnatury dostępu współdzielonego konta dla określonego konta magazynu.|
|/storageAccounts/listServiceSas/Action|Token sygnatury dostępu współdzielonego usługi Storage|
|/storageAccounts/Services/diagnosticSettings/Write|Tworzenie/aktualizowanie ustawień diagnostycznych na konto magazynu.|
|/skus/Read|Wyświetla listę SKU obsługiwana przez Microsoft.Storage.|
|/Usages/Read|Zwraca limit i bieżącą liczbę użyć dla zasobów w określonej subskrypcji|
|/Operations/Read|Sonduje stan operacji asynchronicznej.|
|/Locations/deleteVirtualNetworkOrSubnets/Action|Powiadamia element Microsoft.Storage o usuwaniu sieci wirtualnej lub podsieci|

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple

| Operacja | Opis |
|---|---|
|/managers/clearAlerts/Action|Wyczyść wszystkie alerty skojarzonych z Menedżerem urządzeń.|
|/managers/getActivationKey/Action|Menedżer urządzeń, należy pobrać klucza aktywacji.|
|/managers/regenerateActivationKey/Action|Menedżer urządzeń, należy ponownie wygenerować klucza aktywacji.|
|/managers/regenarateRegistationCertificate/Action|Ponowne generowanie certyfikatu rejestracji dla menedżerów urządzeń.|
|/managers/getEncryptionKey/Action|Pobierz klucz szyfrowania dla Menedżera urządzeń.|
|/managers/Read|Wyświetla listę lub pobiera menedżerów urządzeń|
|/managers/DELETE|Usuwa menedżerów urządzeń|
|/ menedżerów/zapisu|Utwórz lub zaktualizuj Menedżera urządzeń|
|/managers/configureDevice/Action|Konfiguruje urządzenia|
|/managers/listActivationKey/Action|Pobiera klucz aktywacji Menedżera urządzeń StorSimple.|
|/managers/listPublicEncryptionKey/Action|Lista kluczy szyfrowania publicznego z Menedżera urządzeń StorSimple.|
|/managers/listPrivateEncryptionKey/Action|Pobiera klucz prywatny szyfrowania dla Menedżera urządzeń StorSimple.|
|/managers/provisionCloudAppliance/Action|Utwórz nowe urządzenia chmury.|
|Menedżerowie/zapisu|Operacja Utwórz magazyn tworzy zasób platformy Azure typu „magazyn”|
|Menedżerowie/odczytu|Operacja Get magazynu pobiera obiekt reprezentujący zasobów platformy Azure typu "magazynu"|
|/ Menedżerów/usuwania|Operacja usuwania magazynu usuwa określony zasób Azure typu "magazynu"|
|/managers/storageAccountCredentials/Write|Utwórz lub zaktualizuj poświadczenia konta magazynu|
|/managers/storageAccountCredentials/Read|Wyświetla listę lub pobiera poświadczeń konta magazynu|
|/managers/storageAccountCredentials/DELETE|Usuwa poświadczenia konta magazynu|
|/managers/storageAccountCredentials/listAccessKey/Action|Listy kluczy dostępu do poświadczeń konta magazynu|
|/managers/accessControlRecords/Read|Wyświetla listę lub pobiera rekordy kontroli dostępu|
|/managers/accessControlRecords/Write|Utwórz lub zaktualizuj rekordy kontroli dostępu|
|/managers/accessControlRecords/DELETE|Usuwa rekordy kontroli dostępu|
|/managers/Metrics/Read|Wyświetla listę lub pobiera metryki|
|/managers/bandwidthSettings/Read|Wyświetl listę ustawień przepustowości (tylko 8000 Series)|
|/managers/bandwidthSettings/Write|Tworzy nowy lub aktualizuje ustawienia przepustowości (8000 serii tylko)|
|/managers/bandwidthSettings/DELETE|Usuwa istniejące ustawienia przepustowości (8000 serii tylko)|
|Menedżerowie/extendedInformation/odczytu|Operacja Pobierz rozszerzone informacje pobiera rozszerzone informacje o obiekcie reprezentujące zasób platformy Azure typu „magazyn”|
|Menedżerowie/extendedInformation/zapisu|Operacja Pobierz rozszerzone informacje pobiera rozszerzone informacje o obiekcie reprezentujące zasób platformy Azure typu „magazyn”|
|/ Menedżerów/extendedInformation/usuwania|Operacja Pobierz rozszerzone informacje pobiera rozszerzone informacje o obiekcie reprezentujące zasób platformy Azure typu „magazyn”|
|/managers/Alerts/Read|Wyświetla listę lub pobiera alerty|
|/managers/storageDomains/Read|Wyświetla listę lub pobiera domen magazynu|
|/managers/storageDomains/Write|Utwórz lub zaktualizuj domen magazynu|
|/managers/storageDomains/DELETE|Usuwa domen magazynu|
|/managers/Devices/scanForUpdates/Action|Skanowanie w poszukiwaniu aktualizacji na urządzeniu.|
|/managers/Devices/Download/Action|Pobierania aktualizacji dla urządzenia.|
|/managers/Devices/Install/Action|Zainstaluj aktualizacje na urządzeniu.|
|/managers/Devices/Read|Wyświetla listę lub pobiera urządzenia|
|/managers/Devices/Write|Utwórz lub zaktualizuj urządzenia|
|/managers/Devices/DELETE|Usuwa urządzenia|
|/managers/Devices/Deactivate/Action|Dezaktywuje urządzenia.|
|/managers/Devices/publishSupportPackage/Action|Publikowanie pakietu obsługi urządzenia do firmy Microsoft Support rozwiązywania problemów.|
|/managers/Devices/Failover/Action|Tryb pracy awaryjnej urządzenia.|
|/managers/Devices/sendTestAlertEmail/Action|Wyślij testową wiadomość e-mail alertów do adresatów wiadomości e-mail skonfigurowany.|
|/managers/Devices/installUpdates/Action|Instaluje aktualizacje na urządzeniach|
|/managers/Devices/listFailoverSets/Action|Lista zestawów trybu failover do istniejącego urządzenia.|
|/managers/Devices/listFailoverTargets/Action|Cele trybu failover listy urządzeń|
|/managers/Devices/publicEncryptionKey/Action|Lista publicznego klucza szyfrowania Menedżera urządzeń|
|/ Menedżera urządzeń/hardwareComponentGroups /<br>odczytywanie|Lista grup składników sprzętowych|
|/ Menedżera urządzeń/hardwareComponentGroups /<br>changeControllerPowerState/działania|Zmiany stanu zasilania kontrolera grup składników sprzętowych|
|/managers/Devices/Metrics/Read|Wyświetla listę lub pobiera metryki|
|/managers/Devices/chapSettings/Write|Utwórz lub zaktualizuj ustawienia protokołu Chap|
|/managers/Devices/chapSettings/Read|Wyświetla listę lub pobiera ustawienia protokołu Chap|
|/managers/Devices/chapSettings/DELETE|Usuwa ustawienia protokołu Chap|
|/managers/Devices/backupScheduleGroups/Read|Wyświetla listę lub pobiera grup harmonogramu tworzenia kopii zapasowej|
|/managers/Devices/backupScheduleGroups/Write|Utwórz lub zaktualizuj grupy harmonogramu tworzenia kopii zapasowej|
|/managers/Devices/backupScheduleGroups/DELETE|Usuwa harmonogram tworzenia kopii zapasowych grupy|
|/managers/Devices/updateSummary/Read|Wyświetla listę lub pobiera aktualizacji — podsumowanie|
|/ Menedżera urządzeń/migrationSourceConfigurations /<br>Import/działania|Importowanie konfiguracji źródłowego do migracji|
|/ Menedżera urządzeń/migrationSourceConfigurations /<br>startMigrationEstimate/działania|Uruchom zadanie, aby oszacować w czasie trwania procesu migracji.|
|/ Menedżera urządzeń/migrationSourceConfigurations /<br>startMigration/działania|Uruchom migrację za pomocą konfiguracji źródła|
|/ Menedżera urządzeń/migrationSourceConfigurations /<br>confirmMigration/działania|Potwierdza udanej migracji, a następnie przekazać go.|
|/ Menedżera urządzeń/migrationSourceConfigurations /<br>fetchMigrationEstimate/działania|Pobierz stan zadania migracji szacowania.|
|/ Menedżera urządzeń/migrationSourceConfigurations /<br>fetchMigrationStatus/działania|Pobierz stan migracji.|
|/ Menedżera urządzeń/migrationSourceConfigurations /<br>fetchConfirmMigrationStatus/działania|Pobierz stan potwierdzenia migracji.|
|/managers/Devices/alertSettings/Read|Wyświetla listę lub pobiera ustawienia alertu|
|/managers/Devices/alertSettings/Write|Utwórz lub zaktualizuj ustawienia alertu|
|/managers/Devices/networkSettings/Read|Wyświetla listę lub pobiera ustawienia sieci|
|/managers/Devices/networkSettings/Write|Tworzy nowy lub aktualizuje ustawienia sieci|
|/managers/Devices/Jobs/Read|Wyświetla listę lub pobiera zadania|
|/managers/Devices/Jobs/Cancel/Action|Anuluj uruchomione zadania|
|/managers/Devices/metricsDefinitions/Read|Wyświetla listę lub pobiera definicje metryk|
|/managers/Devices/volumeContainers/Write|Tworzy nowy lub aktualizuje kontenery woluminów (8000 serii tylko)|
|/managers/Devices/volumeContainers/Read|Lista kontenery woluminów (tylko 8000 Series)|
|/managers/Devices/volumeContainers/DELETE|Usuwa istniejące kontenery woluminów (8000 serii tylko)|
|/managers/Devices/volumeContainers/listEncryptionKeys/Action|Lista kluczy szyfrowania woluminu kontenerów|
|/managers/Devices/volumeContainers/rolloverEncryptionKey/Action|Klucze szyfrowania przerzucania kontenery woluminów|
|/managers/Devices/volumeContainers/Metrics/Read|Lista metryk|
|/managers/Devices/volumeContainers/Volumes/Read|Wyświetl listę woluminów|
|/managers/Devices/volumeContainers/Volumes/Write|Tworzy nowy lub aktualizuje woluminów|
|/managers/Devices/volumeContainers/Volumes/DELETE|Usuwa istniejące woluminy|
|/managers/Devices/volumeContainers/Volumes/Metrics/Read|Lista metryk|
|/managers/Devices/volumeContainers/Volumes/metricsDefinitions/Read|Lista definicji metryk|
|/managers/Devices/volumeContainers/metricsDefinitions/Read|Lista definicji metryk|
|/managers/Devices/iscsiservers/Read|Wyświetla listę lub pobiera iSCSI serwerów|
|/managers/Devices/iscsiservers/Write|Utwórz lub zaktualizuj iSCSI serwerów|
|/managers/Devices/iscsiservers/DELETE|Usuwa iSCSI serwerów|
|/managers/Devices/iscsiservers/Backup/Action|Pobierz kopię zapasową serwera iSCSI.|
|/managers/Devices/iscsiservers/Metrics/Read|Wyświetla listę lub pobiera metryki|
|/managers/Devices/iscsiservers/Disks/Read|Wyświetla listę lub pobiera dysków|
|/managers/Devices/iscsiservers/Disks/Write|Utwórz lub zaktualizuj dysków|
|/managers/Devices/iscsiservers/Disks/DELETE|Usuwa dysków|
|/managers/Devices/iscsiservers/Disks/Metrics/Read|Wyświetla listę lub pobiera metryki|
|/managers/Devices/iscsiservers/Disks/metricsDefinitions/Read|Wyświetla listę lub pobiera definicje metryk|
|/managers/Devices/iscsiservers/metricsDefinitions/Read|Wyświetla listę lub pobiera definicje metryk|
|/managers/Devices/Backups/Read|Wyświetla listę lub pobiera zestawu kopii zapasowych|
|/managers/Devices/Backups/DELETE|Usuwa zestaw kopii zapasowych|
|/managers/Devices/Backups/Restore/Action|Przywróć wszystkie woluminy z zestawu kopii zapasowych.|
|/managers/Devices/Backups/Elements/clone/Action|Klonowanie udziale lub woluminie przy użyciu kopii zapasowej elementu.|
|/managers/Devices/backupPolicies/Write|Tworzy nowy lub aktualizuje zasady tworzenia kopii zapasowej (8000 serii tylko)|
|/managers/Devices/backupPolicies/Read|Tworzenie kopii zapasowej listy zasad (tylko 8000 Series)|
|/managers/Devices/backupPolicies/DELETE|Usuwa istniejące zasady tworzenia kopii zapasowej (8000 serii tylko)|
|/managers/Devices/backupPolicies/Backup/Action|Tworzenie kopii zapasowej ręcznie utworzyć na żądanie kopii zapasowych wszystkich woluminów, które są chronione przez zasady.|
|/managers/Devices/backupPolicies/Schedules/Write|Tworzy nowy lub aktualizuje harmonogramów|
|/managers/Devices/backupPolicies/Schedules/Read|Lista harmonogramów|
|/managers/Devices/backupPolicies/Schedules/DELETE|Usuwa istniejących harmonogramów|
|/managers/Devices/securitySettings/Update/Action|Zaktualizuj ustawienia zabezpieczeń.|
|/managers/Devices/securitySettings/Read|Wyświetl listę ustawień zabezpieczeń|
|/ Menedżera urządzeń/securitySettings /<br>syncRemoteManagementCertificate/działania|Synchronizuj certyfikat zdalnego zarządzania dla urządzenia.|
|/managers/Devices/securitySettings/Write|Tworzy nowy lub aktualizuje ustawienia zabezpieczeń|
|/managers/Devices/fileservers/Read|Wyświetla listę lub pobiera na serwerach plików|
|/managers/Devices/fileservers/Write|Utwórz lub zaktualizuj na serwerach plików|
|/managers/Devices/fileservers/DELETE|Usuwa na serwerach plików|
|/managers/Devices/fileservers/Backup/Action|Pobierz kopię zapasową serwera plików.|
|/managers/Devices/fileservers/Metrics/Read|Wyświetla listę lub pobiera metryki|
|/managers/Devices/fileservers/shares/Write|Utwórz lub zaktualizuj akcji|
|/managers/Devices/fileservers/shares/Read|Wyświetla listę lub pobiera akcji|
|/managers/Devices/fileservers/shares/DELETE|Usuwa akcji|
|/managers/Devices/fileservers/shares/Metrics/Read|Wyświetla listę lub pobiera metryki|
|/managers/Devices/fileservers/shares/metricsDefinitions/Read|Wyświetla listę lub pobiera definicje metryk|
|/managers/Devices/fileservers/metricsDefinitions/Read|Wyświetla listę lub pobiera definicje metryk|
|/managers/Devices/timeSettings/Read|Wyświetla listę lub pobiera ustawienia czasu|
|/managers/Devices/timeSettings/Write|Tworzy nowy lub aktualizuje ustawienia czasu|
|Menedżerowie/certyfikatów/zapisu|Operacja aktualizacji zasobów certyfikatu aktualizuje certyfikat poświadczeń zasobów/magazynu.|
|/managers/cloudApplianceConfigurations/Read|Lista urządzenia chmury obsługiwane konfiguracje|
|/managers/metricsDefinitions/Read|Wyświetla listę lub pobiera definicje metryk|
|/managers/encryptionSettings/Read|Wyświetla listę lub pobiera ustawienia szyfrowania|

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics

| Operacja | Opis |
|---|---|
|/streamingjobs/Start/Action|Uruchom zadania usługi analiza strumienia|
|/streamingjobs/Stop/Action|Zatrzymanie zadania usługi analiza strumienia|
|/ streamingjobs/odczytu|Zadania usługi analiza strumienia odczytu|
|/ streamingjobs/zapisu|Zapis zadania usługi analiza strumienia|
|/ streamingjobs/Delete|Usuwanie zadania usługi analiza strumienia|
|/streamingjobs/Providers/Microsoft.Insights/metricDefinitions/Read|Pobiera dostępne metryki dla streamingjobs|
|/streamingjobs/Providers/Microsoft.Insights/diagnosticSettings/Read|Ustawienie diagnostyczne dla odczytu.|
|/streamingjobs/Providers/Microsoft.Insights/diagnosticSettings/Write|Zapis ustawienie diagnostyczne.|
|/streamingjobs/Providers/Microsoft.Insights/logDefinitions/Read|Pobiera dostępne dzienniki dla streamingjobs|
|/streamingjobs/Transformations/Read|Odczyt Stream Analytics zadania przekształcania|
|/streamingjobs/Transformations/Write|Zapis przekształcenie zadania usługi analiza strumienia|
|/streamingjobs/Transformations/DELETE|Usuń przekształcenie zadania usługi analiza strumienia|
|/streamingjobs/inputs/Read|Wejścia zadania usługi analiza strumienia odczytu|
|/streamingjobs/inputs/Write|Zapisu danych wejściowych zadania usługi analiza strumienia|
|/streamingjobs/inputs/DELETE|Usuń dane wejściowe zadania usługi analiza strumienia|
|/streamingjobs/outputs/Read|Odczytu Stream Analytics — dane wyjściowe zadania|
|/streamingjobs/outputs/Write|Zapisywania danych wyjściowych zadania usługi analiza strumienia|
|/streamingjobs/outputs/DELETE|Usuń dane wyjściowe zadania usługi analiza strumienia|

## <a name="microsoftsupport"></a>Microsoft.Support

| Operacja | Opis |
|---|---|
|/ register/działania|Przeprowadza rejestrację u dostawcy zasobów pomocy technicznej|
|/supportTickets/Read|Pobiera szczegóły biletu pomocy technicznej (w tym stan, ważność, szczegóły dotyczące kontaktu i wiadomości) lub pobiera listę biletów pomocy technicznej dla różnych subskrypcji.|
|/ supportTickets/zapisu|Tworzy lub aktualizuje bilet pomocy technicznej. Można utworzyć biletu pomocy technicznej dla technicznych, rozliczeń, limity przydziału lub zarządzaniem subskrypcjami problemy związane z. Możesz zaktualizować ważność, szczegóły dotyczące kontaktu i komunikacji dla istniejących biletów pomocy technicznej.|

## <a name="microsoftweb"></a>Microsoft.Web

| Operacja | Opis |
|---|---|
|/ unregister/działania|Wyrejestruj Microsoft.Web dostawcy zasobów dla subskrypcji.|
|Zweryfikuj/działania|Sprawdzanie poprawności.|
|/ register/działania|Rejestrowanie dostawcy zasobów Microsoft.Web dla subskrypcji.|
|/ hostingEnvironments/odczytu|Pobierz właściwości środowiska usługi aplikacji|
|/ hostingEnvironments/zapisu|Tworzenie nowego środowiska usługi aplikacji lub zaktualizować istniejący|
|/ hostingEnvironments/Delete|Usuwanie środowiska usługi aplikacji|
|/hostingEnvironments/reboot/Action|Ponownie wszystkie maszyny w środowisku usługi aplikacji|
|/hostingenvironments/resume/Action|Wznów środowiskach hostingu.|
|/hostingenvironments/suspend/Action|Wstrzymaj środowiskach hostingu.|
|/hostingenvironments/metricdefinitions/Read|Pobierz Hosting definicji metryk środowisk.|
|/hostingEnvironments/workerPools/Read|Pobierz właściwości puli procesów roboczych w środowisku usługi aplikacji|
|/hostingEnvironments/workerPools/Write|Tworzenie nowej puli procesów roboczych w środowisku usługi aplikacji lub zaktualizuj istniejącą|
|/hostingenvironments/workerpools/metricdefinitions/Read|Pobierz Hosting definicje Metryka Workerpools środowisk.|
|/hostingenvironments/workerpools/Metrics/Read|Pobierz Hosting środowisk Workerpools metryki.|
|/hostingenvironments/workerpools/skus/Read|Pobierz Hosting jednostki SKU Workerpools środowisk.|
|/hostingenvironments/workerpools/Usages/Read|Pobierz Hosting środowisk Workerpools użycia.|
|/hostingenvironments/Sites/Read|Pobierz aplikacje sieci Web środowiskach hostingu.|
|/hostingenvironments/serverfarms/Read|Pobierz Hosting środowisk planów usługi aplikacji.|
|/hostingenvironments/Usages/Read|Pobierz Hosting użycia środowiska.|
|/hostingenvironments/capacities/Read|Pobierz Hosting możliwości środowiska.|
|/hostingenvironments/Operations/Read|Pobierz Hosting operacji środowisk.|
|/hostingEnvironments/multiRolePools/Read|Pobierz właściwości puli frontonu w środowisku usługi aplikacji|
|/hostingEnvironments/multiRolePools/Write|Tworzenie nowej puli frontonu w środowisku usługi aplikacji lub zaktualizuj istniejącą|
|/hostingenvironments/multirolepools/metricdefinitions/Read|Pobierz Hosting definicji metryk pełniących wiele pul środowisk.|
|/hostingenvironments/multirolepools/Metrics/Read|Pobierz Hosting środowisk pełniących wiele pul metryki.|
|/hostingenvironments/multirolepools/skus/Read|Pobierz Hosting jednostki SKU pełniących wiele pul środowisk.|
|/hostingenvironments/multirolepools/Usages/Read|Pobierz Hosting środowisk pełniących wiele pul użycia.|
|/hostingenvironments/Diagnostics/Read|Pobierz Hosting środowisk diagnostyki.|
|/publishingusers/Read|Pobierz publikowanie użytkowników.|
|/ publishingusers/zapisu|Aktualizacja publikowania użytkowników.|
|/checknameavailability/Read|Sprawdź, czy nazwa zasobu jest dostępna.|
|/ geoRegions/odczytu|Pobierz listę regionów geograficznych.|
|/ lokacji/odczytu|Pobierz właściwości aplikacji sieci Web|
|/ lokacji/zapisu|Utwórz nową aplikację sieci Web lub zaktualizuj istniejącą|
|/ lokacji/Delete|Usuń istniejącą aplikację sieci Web|
|/Sites/Backup/Action|Utwórz nową kopię zapasową aplikacji sieci web|
|/Sites/publishxml/Action|Pobierz publikowania format xml profilu dla aplikacji sieci Web|
|/Sites/publish/Action|Publikowanie aplikacji sieci Web|
|/Sites/restart/Action|Ponowne uruchomienie aplikacji sieci Web|
|/Sites/Start/Action|Uruchamiają aplikację sieci Web|
|/Sites/Stop/Action|Zatrzymać aplikacji sieci Web|
|/Sites/slotsswap/Action|Zamienić miejsc wdrażania aplikacji sieci Web|
|/Sites/slotsdiffs/Action|Pobierz różnic w konfiguracji aplikacji sieci web i gniazd|
|/Sites/applySlotConfig/Action|Zastosuj konfigurację sieci web aplikacji gniazda z miejsca docelowego w bieżącej aplikacji sieci web|
|/Sites/resetSlotConfig/Action|Zresetuj konfigurację aplikacji sieci web|
|/Sites/Functions/Action|Funkcje aplikacji sieci Web.|
|/Sites/listsyncfunctiontriggerstatus/Action|Lista synchronizacji funkcja wyzwalacz stanu aplikacji sieci Web.|
|/Sites/networktrace/Action|Aplikacje sieci Web śledzenia sieci.|
|/Sites/newpassword/Action|Aplikacje sieci Web NoweHasło.|
|/Sites/Sync/Action|Aplikacje sieci Web synchronizacji.|
|/Sites/operationresults/Read|Pobierz wyniki operacji aplikacji sieci Web.|
|/Sites/webjobs/Read|Pobrania zadań Webjob aplikacji sieci Web.|
|/ lokacji/tworzenia kopii zapasowej/odczytu|Pobierz kopii zapasowej aplikacji sieci Web.|
|/Sites/Backup/Write|Aktualizacja kopii zapasowej aplikacji sieci Web.|
|/Sites/metricdefinitions/Read|Pobierz definicje Metryka aplikacji sieci Web.|
|/Sites/Metrics/Read|Pobieraj metryki aplikacji sieci Web.|
|/Sites/continuouswebjobs/DELETE|Usuń zadania ciągłego sieci Web aplikacji sieci Web.|
|/Sites/continuouswebjobs/Read|Pobierz zadania ciągłego sieci Web aplikacji sieci Web.|
|/Sites/continuouswebjobs/Start/Action|Rozpocznij zadania ciągłego sieci Web aplikacji sieci Web.|
|/Sites/continuouswebjobs/Stop/Action|Zatrzymanie zadania ciągłego sieci Web aplikacji sieci Web.|
|/Sites/domainownershipidentifiers/Read|Pobierz identyfikatory własność domeny aplikacji sieci Web.|
|/Sites/domainownershipidentifiers/Write|Zaktualizuj identyfikatory własność domeny aplikacji sieci Web.|
|/Sites/premieraddons/DELETE|Usuń dodatków Premier aplikacji sieci Web.|
|/Sites/premieraddons/Read|Pobierz dodatki Premier aplikacji sieci Web.|
|/Sites/premieraddons/Write|Aktualizowanie dodatków Premier aplikacji sieci Web.|
|/Sites/triggeredwebjobs/DELETE|Usuń wyzwalanych zadań Webjob aplikacji sieci Web.|
|/Sites/triggeredwebjobs/Read|Pobierz wyzwalanych zadań Webjob aplikacji sieci Web.|
|/Sites/triggeredwebjobs/Run/Action|Uruchom wyzwalanych zadań Webjob aplikacji sieci Web.|
|/Sites/hostnamebindings/DELETE|Usuń powiązania nazwę hosta aplikacji sieci Web.|
|/Sites/hostnamebindings/Read|Pobierz powiązania nazwę hosta aplikacji sieci Web.|
|/Sites/hostnamebindings/Write|Zaktualizuj powiązania nazwę hosta aplikacji sieci Web.|
|/Sites/virtualnetworkconnections/DELETE|Usuwanie połączenia wirtualnej sieci aplikacji sieci Web.|
|/Sites/virtualnetworkconnections/Read|Pobrać połączeń sieci wirtualnej aplikacji sieci Web.|
|/Sites/virtualnetworkconnections/Write|Zaktualizuj połączenia wirtualnej sieci aplikacji sieci Web.|
|/Sites/virtualnetworkconnections/Gateways/Read|Pobierz bram połączeń sieci wirtualnej aplikacji sieci Web.|
|/Sites/virtualnetworkconnections/Gateways/Write|Zaktualizuj bramy połączenia wirtualnej sieci aplikacji sieci Web.|
|/Sites/publishxml/Read|Pobierz aplikacje sieci Web publikowanie XML.|
|/Sites/hybridconnectionrelays/Read|Pobierz przekaźników połączeń hybrydowych aplikacji sieci Web.|
|/Sites/perfcounters/Read|Pobierz liczniki wydajności aplikacji sieci Web.|
|/Sites/Usages/Read|Pobierz użycia aplikacji sieci Web.|
|/Sites/Slots/Write|Utwórz nowe miejsce aplikacji sieci Web lub zaktualizuj istniejącą|
|/Sites/Slots/DELETE|Usuwanie istniejącego miejsca aplikacji sieci Web|
|/Sites/Slots/Backup/Action|Utwórz nową kopię zapasową miejsca aplikacji sieci Web.|
|/Sites/Slots/publishxml/Action|Pobierz publikowanie format xml profilu dla miejsca aplikacji sieci Web|
|/Sites/Slots/publish/Action|Publikowanie miejsca aplikacji sieci Web|
|/Sites/Slots/restart/Action|Uruchom ponownie miejsca aplikacji sieci Web|
|/Sites/Slots/Start/Action|Uruchom miejsca aplikacji sieci Web|
|/Sites/Slots/Stop/Action|Zatrzymaj miejsca aplikacji sieci Web|
|/Sites/Slots/slotsswap/Action|Zamienić miejsc wdrażania aplikacji sieci Web|
|/Sites/Slots/slotsdiffs/Action|Pobierz różnic w konfiguracji aplikacji sieci web i gniazd|
|/Sites/Slots/applySlotConfig/Action|Zastosuj konfigurację sieci web aplikacji gniazda z miejsca docelowego do bieżącego miejsca.|
|/Sites/Slots/resetSlotConfig/Action|Zresetuj konfigurację miejsca aplikacji sieci web|
|/Sites/Slots/Read|Pobierz właściwości miejsce wdrożenia aplikacji sieci Web|
|/Sites/Slots/newpassword/Action|Miejsc aplikacji sieci Web NoweHasło.|
|/Sites/Slots/Sync/Action|Synchronizacja miejsc aplikacji sieci Web.|
|/Sites/Slots/operationresults/Read|Pobierz wyniki operacji miejsc aplikacji sieci Web.|
|/Sites/Slots/webjobs/Read|Uzyskać zadań Webjob miejsc aplikacji sieci Web.|
|/Sites/Slots/Backup/Write|Zaktualizuj kopii zapasowej miejsc aplikacji sieci Web.|
|/Sites/Slots/metricdefinitions/Read|Pobierz definicje metryk miejsc aplikacji sieci Web.|
|/Sites/Slots/Metrics/Read|Pobieraj metryki miejsc aplikacji sieci Web.|
|/Sites/Slots/continuouswebjobs/DELETE|Usuń zadania ciągłego Web miejsc aplikacji sieci Web.|
|/Sites/Slots/continuouswebjobs/Read|Pobierz zadania ciągłego Web miejsc aplikacji sieci Web.|
|/Sites/Slots/continuouswebjobs/Start/Action|Uruchom zadania ciągłego Web miejsc aplikacji sieci Web.|
|/Sites/Slots/continuouswebjobs/Stop/Action|Zatrzymanie zadania ciągłego Web miejsc aplikacji sieci Web.|
|/Sites/Slots/premieraddons/DELETE|Usuń dodatków Premier miejsc aplikacji sieci Web.|
|/Sites/Slots/premieraddons/Read|Pobierz dodatki Premier miejsc aplikacji sieci Web.|
|/Sites/Slots/premieraddons/Write|Zaktualizuj dodatków Premier miejsc aplikacji sieci Web.|
|/Sites/Slots/triggeredwebjobs/DELETE|Usuwanie zadań Webjob wyzwalanych miejsc aplikacji sieci Web.|
|/Sites/Slots/triggeredwebjobs/Read|Pobrania zadań Webjob wyzwalanych miejsc aplikacji sieci Web.|
|/Sites/Slots/triggeredwebjobs/Run/Action|Uruchamianie zadań Webjob wyzwalanych miejsc aplikacji sieci Web.|
|/Sites/Slots/hostnamebindings/DELETE|Usuń powiązań z nazwami hostów miejsc aplikacji sieci Web.|
|/Sites/Slots/hostnamebindings/Read|Pobierz powiązań z nazwami hostów miejsc aplikacji sieci Web.|
|/Sites/Slots/hostnamebindings/Write|Zaktualizować powiązań z nazwami hostów miejsc aplikacji sieci Web.|
|/Sites/Slots/phplogging/Read|Pobierz Phplogging miejsc aplikacji sieci Web.|
|/Sites/Slots/virtualnetworkconnections/DELETE|Usuwanie połączenia wirtualnej sieci miejsc aplikacji sieci Web.|
|/Sites/Slots/virtualnetworkconnections/Read|Pobierz połączenia wirtualnej sieci miejsc aplikacji sieci Web.|
|/Sites/Slots/virtualnetworkconnections/Write|Zaktualizuj połączenia wirtualnych sieci miejsc aplikacji sieci Web.|
|/Sites/Slots/virtualnetworkconnections/Gateways/Write|Zaktualizuj bramy połączeń sieci wirtualnej miejsc aplikacji sieci Web.|
|/Sites/Slots/Usages/Read|Pobierz użycia miejsc aplikacji sieci Web.|
|/Sites/Slots/hybridconnection/DELETE|Usuwanie połączenia hybrydowego miejsc aplikacji sieci Web.|
|/Sites/Slots/hybridconnection/Read|Pobierz połączenia hybrydowego miejsc aplikacji sieci Web.|
|/Sites/Slots/hybridconnection/Write|Zaktualizuj połączenie hybrydowe miejsc aplikacji sieci Web.|
|/Sites/Slots/config/Read|Pobierz ustawienia konfiguracji miejsca aplikacji sieci Web|
|/Sites/Slots/config/list/Action|Lista miejsca aplikacji sieci Web poufne ustawienia zabezpieczeń, takie jak publikowania poświadczeń, ustawienia aplikacji i parametry połączenia|
|/Sites/Slots/config/Write|Aktualizowanie ustawień konfiguracji miejsca aplikacji sieci Web|
|/Sites/Slots/config/DELETE|Usuwanie konfiguracji miejsc aplikacji sieci Web.|
|/Sites/Slots/instances/Read|Pobierz wystąpienia miejsc aplikacji sieci Web.|
|/Sites/Slots/instances/processes/Read|Pobierz procesów wystąpień miejsc aplikacji sieci Web.|
|/Sites/Slots/instances/Deployments/Read|Pobierz wdrożeń wystąpień miejsc aplikacji sieci Web.|
|/Sites/Slots/sourcecontrols/Read|Pobierz ustawienia konfiguracji miejsca aplikacji sieci Web do kontroli źródła|
|/Sites/Slots/sourcecontrols/Write|Zaktualizuj ustawienia konfiguracji kontroli źródła miejsca aplikacji sieci Web|
|/Sites/Slots/sourcecontrols/DELETE|Usuń ustawienia konfiguracji kontroli źródła miejsca aplikacji sieci Web|
|/Sites/Slots/Restore/Read|Pobierz przywracania miejsc aplikacji sieci Web.|
|/Sites/Slots/analyzecustomhostname/Read|Pobierz Web miejsc aplikacji analizowanie niestandardową nazwę hosta.|
|/Sites/Slots/Backups/Read|Pobierz właściwości kopii zapasowej miejsc aplikacji sieci web|
|/Sites/Slots/Backups/list/Action|Lista sieci Web aplikacji miejsc wykonywanie kopii zapasowych.|
|/Sites/Slots/Backups/Restore/Action|Przywracanie kopii zapasowych miejsc aplikacji sieci Web.|
|/Sites/Slots/Deployments/DELETE|Usunięcie wdrożeń miejsc aplikacji sieci Web.|
|/Sites/Slots/Deployments/Read|Pobierz wdrożeń miejsc aplikacji sieci Web.|
|/Sites/Slots/Deployments/Write|Zaktualizuj wdrożeń miejsc aplikacji sieci Web.|
|/Sites/Slots/Deployments/log/Read|Pobierz dziennik wdrożeń miejsc aplikacji sieci Web.|
|/Sites/hybridconnection/DELETE|Usuwanie połączenia hybrydowego aplikacji sieci Web.|
|/Sites/hybridconnection/Read|Pobierz dane o połączeniu hybrydowych aplikacji sieci Web.|
|/Sites/hybridconnection/Write|Zaktualizuj połączenie hybrydowe aplikacji sieci Web.|
|/Sites/recommendationhistory/Read|Pobierz historię zalecenie aplikacji sieci Web.|
|/Sites/recommendations/Read|Pobierz listę zaleceń dotyczących aplikacji sieci web.|
|/Sites/recommendations/disable/Action|Wyłącz zalecenia dotyczące aplikacji sieci Web.|
|/Sites/config/Read|Pobierz ustawienia konfiguracji aplikacji sieci Web|
|/Sites/config/list/Action|Wyświetl listę aplikacji sieci Web poufne ustawienia zabezpieczeń, takie jak publikowania poświadczeń, ustawienia aplikacji i parametry połączenia|
|/Sites/config/Write|Zaktualizuj ustawienia konfiguracji aplikacji sieci Web|
|/Sites/config/DELETE|Usuwanie konfiguracji aplikacji sieci Web.|
|/Sites/instances/Read|Pobierz wystąpienia aplikacji sieci Web.|
|/Sites/instances/processes/DELETE|Usuń procesów wystąpienia aplikacji sieci Web.|
|/Sites/instances/processes/Read|Pobierz procesów wystąpienia aplikacji sieci Web.|
|/Sites/instances/Deployments/Read|Pobierz wdrożeń wystąpienia aplikacji sieci Web.|
|/Sites/sourcecontrols/Read|Pobierz ustawienia konfiguracji aplikacji sieci Web do kontroli źródła|
|/Sites/sourcecontrols/Write|Zaktualizuj ustawienia konfiguracji kontroli źródła dla aplikacji sieci Web|
|/Sites/sourcecontrols/DELETE|Usuń ustawienia konfiguracji kontroli źródła dla aplikacji sieci Web|
|/Sites/Restore/Read|Pobierz przywracania aplikacji sieci Web.|
|/Sites/analyzecustomhostname/Read|Przeanalizuj niestandardową nazwę hosta.|
|/Sites/Backups/Read|Pobierz właściwości kopii zapasowej aplikacji sieci web|
|/Sites/Backups/list/Action|Lista kopie zapasowe aplikacji sieci Web.|
|/Sites/Backups/Restore/Action|Przywracać kopie zapasowe aplikacji sieci Web.|
|/Sites/snapshots/Read|Pobrać migawek aplikacji sieci Web.|
|/Sites/Functions/DELETE|Usuń funkcje aplikacji sieci Web.|
|/Sites/Functions/listsecrets/Action|Listy kluczy tajnych funkcje aplikacji sieci Web.|
|/Sites/Functions/Read|Pobierz funkcje aplikacji sieci Web.|
|/Sites/Functions/Write|Zaktualizuj funkcje aplikacji sieci Web.|
|/Sites/Deployments/DELETE|Usunięcie wdrożenia aplikacji sieci Web.|
|/Sites/Deployments/Read|Pobierz wdrożenia aplikacji sieci Web.|
|/Sites/Deployments/Write|Aktualizacji wdrożenia aplikacji sieci Web.|
|/Sites/Deployments/log/Read|Pobierz dziennik wdrożenia aplikacji sieci Web.|
|/Sites/Diagnostics/Read|Pobierz diagnostyki aplikacji sieci Web.|
|/Sites/Diagnostics/workerprocessrecycle/Read|Pobierz odtworzenia procesu roboczego diagnostyki aplikacji sieci Web.|
|/Sites/Diagnostics/workeravailability/Read|Pobierz Workeravailability diagnostyki aplikacji sieci Web.|
|/Sites/Diagnostics/runtimeavailability/Read|Pobierz dostępności środowiska uruchomieniowego diagnostyki aplikacji sieci Web.|
|/Sites/Diagnostics/cpuanalysis/Read|Pobierz Cpuanalysis diagnostyki aplikacji sieci Web.|
|/Sites/Diagnostics/servicehealth/Read|Pobierz kondycję usługi sieci Web diagnostyki aplikacji.|
|/Sites/Diagnostics/frebanalysis/Read|Pobierz analizy FREB diagnostyki aplikacji sieci Web.|
|/availablestacks/Read|Pobierz stosy dostępne.|
|/isusernameavailable/Read|Sprawdź, czy nazwa użytkownika jest dostępna.|
|/Microsoft.Web/apiManagementAccounts/<br>interfejsy API odczytu|Pobierz listę interfejsów API.|
|/Microsoft.Web/apiManagementAccounts/<br>interfejsy API/zapisu|Dodaj nowy interfejs Api lub zaktualizować istniejący.|
|/Microsoft.Web/apiManagementAccounts/<br>interfejsy API/usuwania|Usuń istniejące interfejsu Api.|
|/Microsoft.Web/apiManagementAccounts/<br>interfejsy API połączeń/odczytu|Pobierz listę połączeń.|
|/Microsoft.Web/apiManagementAccounts/<br>interfejsy API i połączeń/zapisu|Zapisz nowe połączenie, lub zaktualizuj istniejącą.|
|/Microsoft.Web/apiManagementAccounts/<br>interfejsy API/połączeń/usuwania|Usuń istniejące połączenie.|
|/Microsoft.Web/apiManagementAccounts/<br>interfejsy API połączeń/connectionAcls/odczytu|ConnectionAcls odczytu|
|/Microsoft.Web/apiManagementAccounts/<br>interfejsy API i połączeń/connectionAcls/zapisu|Dodaj lub zaktualizuj ConnectionAcl|
|/Microsoft.Web/apiManagementAccounts/<br>interfejsy API/połączeń/connectionAcls/usuwania|Usuń ConnectionAcl|
|/Microsoft.Web/apiManagementAccounts/<br>interfejsy API connectionAcls/odczytu|ConnectionAcls odczytu dla interfejsu Api|
|/Microsoft.Web/apiManagementAccounts/<br>interfejsy API apiAcls/odczytu|ConnectionAcls odczytu|
|/Microsoft.Web/apiManagementAccounts/<br>interfejsy API i apiAcls/zapisu|Utwórz lub zaktualizuj interfejs Api listy kontroli dostępu|
|/Microsoft.Web/apiManagementAccounts/<br>interfejsy API/apiAcls/usuwania|Usuń listy ACL interfejsu Api|
|/ serverfarms/odczytu|Pobiera właściwości planu usługi App Service|
|/ serverfarms/zapisu|Tworzenie planu usługi aplikacji lub zaktualizuj istniejącą|
|/ serverfarms/Delete|Usuń istniejący Plan usługi App Service|
|/serverfarms/restartSites/Action|Uruchom ponownie wszystkie aplikacje sieci Web w planie usługi aplikacji|
|/serverfarms/operationresults/Read|Pobierz wyniki operacji planów usługi aplikacji.|
|/serverfarms/Capabilities/Read|Skorzystaj z możliwości planów usługi aplikacji.|
|/serverfarms/metricdefinitions/Read|Pobierz definicje metryki planów usługi aplikacji.|
|/serverfarms/Metrics/Read|Pobieraj metryki planów usługi aplikacji.|
|/serverfarms/hybridconnectionplanlimits/Read|Pobiera limity planu połączeń hybrydowych planów usługi aplikacji.|
|/serverfarms/virtualnetworkconnections/Read|Pobrać połączeń sieci wirtualnej planów usługi aplikacji.|
|/serverfarms/virtualnetworkconnections/Routes/DELETE|Usuń trasy połączenia wirtualnej sieci planów usługi aplikacji.|
|/serverfarms/virtualnetworkconnections/Routes/Read|Pobierz trasy połączenia wirtualnej sieci planów usługi aplikacji.|
|/serverfarms/virtualnetworkconnections/Routes/Write|Aktualizowanie tras połączenia wirtualnej sieci planów usługi aplikacji.|
|/serverfarms/virtualnetworkconnections/Gateways/Write|Zaktualizuj bramy połączenia wirtualnej sieci planów usługi aplikacji.|
|/serverfarms/firstpartyapps/Settings/DELETE|Usuń ustawienia aplikacje firm pierwszy planów usługi aplikacji.|
|/serverfarms/firstpartyapps/Settings/Read|Pobierz ustawienia aplikacje firm pierwszy planów usługi aplikacji.|
|/serverfarms/firstpartyapps/Settings/Write|Zaktualizuj ustawienia aplikacje firm pierwszy planów usługi aplikacji.|
|/serverfarms/Sites/Read|Pobierz aplikacje sieci Web planów usługi aplikacji.|
|/serverfarms/workers/reboot/Action|Ponowny rozruch pracowników planów usługi aplikacji.|
|/serverfarms/hybridconnectionrelays/Read|Pobierz przekaźników połączenia hybrydowe planów usługi aplikacji.|
|/serverfarms/skus/Read|Pobierz jednostki SKU planów usługi aplikacji.|
|/serverfarms/Usages/Read|Pobierz użycia planów usługi aplikacji.|
|/serverfarms/hybridconnectionnamespaces/relays/Sites/Read|Pobierz hybrydowego planów usługi aplikacji połączenia aplikacji sieci Web przekaźników przestrzeni nazw.|
|/ishostnameavailable/Read|Sprawdź, czy nazwa hosta jest dostępny.|
|/ connectionGateways/odczytu|Pobierz listę połączenia bramy.|
|/ connectionGateways/zapisu|Tworzy lub aktualizuje bramy połączenia.|
|/ connectionGateways/Delete|Usuwa bramę połączenia.|
|/connectionGateways/JOIN/Action|Tworzy sprzężenie bramy połączenia.|
|/classicmobileservices/Read|Pobierz klasycznego usług mobilnych.|
|/skus/Read|Pobierz jednostki SKU.|
|/ certyfikatów/odczytu|Pobierz listę certyfikatów.|
|/ certyfikatów/zapisu|Dodaj nowy certyfikat lub zaktualizuj istniejącą.|
|/ certyfikatów/Delete|Usuwanie istniejącego certyfikatu.|
|/Operations/Read|Pobierz operacje.|
|/ zalecenia/odczytu|Pobierz listę zaleceń dotyczących subskrypcji.|
|/ishostingenvironmentnameavailable/Read|GET, jeśli jest dostępna nazwa środowiska hostingu.|
|/ apiManagementAccounts/odczytu|Pobierz listę ApiManagementAccounts.|
|/ apiManagementAccounts/zapisu|Dodaj nowy ApiManagementAccount lub zaktualizuj istniejącą|
|/ apiManagementAccounts/Delete|Usuń istniejące ApiManagementAccount|
|/apiManagementAccounts/connectionAcls/Read|Pobierz listę ACL połączenia.|
|/apiManagementAccounts/apiAcls/Read|ConnectionAcls odczytu|
|/ połączeń/odczytu|Pobierz listę połączeń.|
|/ połączeń/zapisu|Tworzy lub aktualizuje połączenia.|
|/ połączeń/Delete|Usuwa połączenie.|
|/Connections/JOIN/Action|Tworzy sprzężenie połączenia.|
|/Connections/confirmconsentcode/Action|Potwierdź kod zgody połączenia.|
|/Connections/listconsentlinks/Action|Lista łączy zgody dla połączeń.|
|/deploymentlocations/Read|Uzyskanie lokalizacji wdrożenia.|
|/sourcecontrols/Read|Pobierz kontroli źródła.|
|/ sourcecontrols/zapisu|Zaktualizuj kontrolki źródła.|
|/managedhostingenvironments/Read|Pobierz zarządzanych środowiskach hostingu.|
|/managedhostingenvironments/Sites/Read|Pobierz zarządzane Hosting aplikacji sieci Web środowiska.|
|/managedhostingenvironments/serverfarms/Read|Pobierz zarządzane Hosting środowisk planów usługi aplikacji.|
|/Locations/managedapis/Read|Pobierz lokalizacje zarządzanych interfejsów API.|
|/Locations/apioperations/Read|Pobierz operacje lokalizacje interfejsu API.|
|/Locations/connectiongatewayinstallations/Read|Pobierz lokalizacje połączenia bramy instalacji.|
|/ listSitesAssignedToHostName/odczytu|Pobierz nazwy lokacji przypisane do nazwy hosta.|

## <a name="next-steps"></a>Następne kroki

- Dowiedz się, jak [utworzyć niestandardową rolę](role-based-access-control-custom-roles.md).
- Przegląd [wbudowane role RBAC](role-based-access-built-in-roles.md).
- Informacje o sposobie zarządzania przypisywaniem dostępu [przez użytkownika](role-based-access-control-manage-assignments.md) lub [przez zasób](role-based-access-control-configure.md) 
- Dowiedz się, jak [wyświetlać dzienniki aktywności inspekcji akcje zasobów](~/articles/azure-resource-manager/resource-group-audit.md)
