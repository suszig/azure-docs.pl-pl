---
title: "Pojęcia dotyczące usługi Azure IoT Hub urządzenia inicjowania obsługi usługi | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano inicjowania obsługi administracyjnej pojęcia dotyczące usługi specyficzne dla urządzeń z punktu dystrybucji i Centrum IoT"
services: iot-dps
keywords: 
author: nberdy
ms.author: nberdy
ms.date: 10/03/2017
ms.topic: article
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 96c63e5d0379150ea619dbbe912a21e373f808af
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="iot-hub-device-provisioning-service-concepts"></a>Pojęcia dotyczące inicjowania obsługi usługi IoT Hub urządzeń

Usługa inicjowania obsługi urządzeń Centrum IoT to usługa pomocnika do Centrum IoT, które są używane do konfigurowania urządzenia bezobsługową alokacji dla określonego Centrum IoT. Przy użyciu usługi inicjowania obsługi urządzeń można udostępnić milionów urządzeń w sposób bezpieczny i skalowalności.

Inicjowanie obsługi administracyjnej urządzeń jest procesem dwie części. Pierwsza część jest nawiązywania początkowego połączenia między urządzeniem a rozwiązaniem IoT przez *rejestrowanie* urządzenia. Druga część stosuje poprawne *konfiguracji* do urządzenia, zależnie od konkretnych wymogów obowiązujących w rozwiązaniu. Po wykonaniu obu czynności można mówi się, że urządzenie zostało całkowicie *elastycznie*. Usługa inicjowania obsługi urządzeń automatyzuje obu czynności, aby zapewnić bezproblemowe inicjowania obsługi administracyjnej dla urządzenia.

Ten artykuł zawiera omówienie pojęć inicjowania obsługi administracyjnej najbardziej odpowiednią do zarządzania *usługi*. W tym artykule jest najodpowiedniejsze do osoby objętego [kroku konfiguracji chmury](about-iot-dps.md#cloud-setup-step) pobierania urządzenie jest gotowe do wdrożenia.

## <a name="service-operations-endpoint"></a>Punkt końcowy operacji usługi

Punkt końcowy usługi operacji jest punkt końcowy Zarządzanie ustawieniami usługi i konserwowania listy rejestracji. Ten punkt końcowy jest używana tylko przez administratora usługi; nie jest on używany przez urządzenia.

## <a name="device-provisioning-endpoint"></a>Urządzenie inicjowania obsługi administracyjnej punktu końcowego

Urządzenie inicjowania obsługi administracyjnej punktu końcowego jest centralna punktu końcowego, który urządzeniom komunikować się do inicjowania obsługi. Adres URL jest taka sama dla wszystkich usług inicjowania obsługi administracyjnej wyeliminować potrzebę reflash urządzeniami przy użyciu nowych informacji o połączeniu w scenariuszach łańcucha dostaw. [Identyfikator zakresu](#id-scope) zapewnia izolacji dzierżawców.

## <a name="linked-iot-hubs"></a>Połączone centra IoT

Usługa inicjowania obsługi urządzeń można udostępnić tylko urządzenia do centra IoT, które zostały połączone z jej. Połączenie usługi inicjowania obsługi urządzeń Centrum IoT zapewnia usługi uprawnienia odczytu/zapisu w rejestrze urządzenia Centrum IoT; łącze usługi inicjowania obsługi urządzeń można zarejestrować identyfikator urządzenia i ustawienia początkowej konfiguracji w dwie urządzenia. Połączone centra IoT można w dowolnym regionie Azure. Koncentratory w innych subskrypcji może połączyć inicjowania obsługi usługi.

## <a name="allocation-policy"></a>Zasady alokacji

Ustawienie poziomu usługi określa, jak usługa inicjowania obsługi urządzeń przypisuje urządzenia do Centrum IoT. Istnieją trzy zasady alokacji obsługiwane:
* **Równomiernie ważone dystrybucji**: połączone centra IoT prawdopodobnie jednakowo urządzenia udostępnione do nich. Ustawieniem domyślnym. W przypadku udostępniania urządzeń tylko jedno centrum IoT można zachować to ustawienie.
* **Można uzyskać najmniejsze opóźnienia**: urządzeń są udostępniane w Centrum IoT z najniższym opóźnieniu na urządzeniu. Jeśli wiele połączone centra IoT zapewni tego samego uzyskać najmniejsze opóźnienia, inicjowania obsługi usługi skróty urządzeń między tymi koncentratorami
* **Statyczne konfiguracji za pomocą listy rejestracji**: specyfikacja żądaną Centrum IoT na liście rejestracji ma wyższy priorytet niż zasady alokacji poziomu usług.

## <a name="enrollment"></a>Rejestracji

Rejestracja jest rekord urządzeń lub grup urządzeń, które mogą dla niektórych wskazać rejestru. Rekord rejestracji zawiera informacje o urządzeniu lub grupy urządzeń, takich jak metoda zaświadczania urządzenia i opcjonalnie początkowej wymaganą konfiguracją, potrzeby IoT koncentratora i urządzeń identyfikatora. Istnieją dwa rodzaje rejestracji obsługiwane przez usługi inicjowania obsługi urządzeń.

### <a name="enrollment-group"></a>Grupa rejestracji

Grupa rejestracji jest grupy urządzeń mających mechanizm określone poświadczenie. Wszystkie urządzenia w grupie rejestracji stanowi certyfikatów X.509, które zostały podpisane przez tego samego głównego urzędu certyfikacji. Grupy rejestracji można używać tylko mechanizm zaświadczania X.509. Nazwa grupy rejestracji i nazwę certyfikatu musi być alfanumeryczny, małe litery i może zawierać łączników.

> [!TIP]
> Firma Microsoft zaleca używanie grupy rejestracji dużej liczby urządzeń mających żądanej konfiguracji początkowej lub urządzeń wszystkich przejść do tej samej dzierżawy.

### <a name="individual-enrollment"></a>Poszczególne rejestracji

Wpis dla pojedynczego urządzenia, które mogą zarejestrować jest poszczególnych rejestracji. Poszczególne rejestracji może używać certyfikatów X.509 lub tokeny sygnatury dostępu Współdzielonego (w module TPM rzeczywistymi lub wirtualnymi) jako mechanizmy zaświadczania. Identyfikator rejestracji w poszczególnych rejestracji jest alfanumeryczne, małe litery i może zawierać łączniki. Poszczególne rejestracji może mieć identyfikator urządzenia żądaną Centrum IoT określony.

> [!TIP]
> Zalecamy używanie poszczególnych rejestracji dla urządzeń, które wymagają szczególnej konfiguracji początkowej lub dla urządzeń, których można używać tylko tokeny sygnatury dostępu Współdzielonego, za pomocą modułu TPM lub wirtualne modułu TPM jako mechanizm zaświadczania.

## <a name="registration"></a>Rejestracja

Rejestracja jest rekord pomyślnie rejestrowanie/inicjowania obsługi urządzeń do Centrum IoT za pomocą usługi udostępniania urządzeń. Rejestrację rekordów są tworzone automatycznie. można go usunąć, ale nie można zaktualizować.

## <a name="operations"></a>Operacje

Operacje są rozliczeniowym jednostki usługi inicjowania obsługi urządzeń. Jedna operacja jest pomyślne zakończenie jedną instrukcję do usługi. Operacje obejmują rejestrowanie i ponowne rejestrowanie urządzeń, a także zmiany po stronie serwera, takie jak dodawanie i aktualizowanie wpisów na liście rejestracji.
