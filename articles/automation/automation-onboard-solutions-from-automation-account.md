---
title: "Dowiedz się, jak dołączyć rozwiązań zarządzania aktualizacjami, śledzenia zmian i spisu w usłudze Automatyzacja Azure"
description: "Dowiedz się, jak można dołączyć wirtualnej Azure maszyny z rozwiązań zarządzania aktualizacjami, śledzenia zmian i magazynu, które są częścią usługi Automatyzacja Azure"
services: automation
keywords: 
author: georgewallace
ms.author: gwallace
ms.date: 02/28/2018
ms.topic: article
ms.service: automation
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: 670a0c2a11ebfe09406233ab4b454b2e9c2ba0e0
ms.sourcegitcommit: 83ea7c4e12fc47b83978a1e9391f8bb808b41f97
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="onboard-update-management-change-tracking-and-inventory-solutions"></a>Dołączyć rozwiązań do zarządzania aktualizacjami, śledzenia zmian i magazynu

Automatyzacja Azure zapewnia rozwiązania do zarządzania aktualizacjami zabezpieczeń systemu operacyjnego, śledzenie zmian i magazynowych zainstalowanych w komputerach. Istnieje wiele sposobów, aby dołączyć maszyny, możesz dołączyć rozwiązania [z maszyny wirtualnej](automation-onboard-solutions-from-vm.md), z Twojego konta automatyzacji lub przez [runbook](automation-onboard-solutions.md). W tym artykule omówiono dołączania tych rozwiązań z Twojego konta automatyzacji.

## <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure.

Zaloguj się do platformy Azure pod adresem https://portal.azure.com

## <a name="enable-solutions"></a>Włącz rozwiązania

Przejdź do swojego konta automatyzacji i wybierz opcję **spisu** lub **śledzenie zmian** w obszarze **zarządzania konfiguracją**.

Wybierz obszar roboczy analizy dzienników i konto usługi Automatyzacja i kliknij **włączyć** umożliwiające rozwiązanie. Rozwiązanie trwa do 15 minut, aby włączyć.

![Zintegrowanego rozwiązania magazynu](media/automation-onboard-solutions-from-automation-account/onboardsolutions.png)

Śledzenie zmian i spisu rozwiązanie zapewnia możliwość [śledzenia zmian](automation-vm-change-tracking.md) i [spisu](automation-vm-inventory.md) na maszynach wirtualnych. W tym kroku należy włączyć rozwiązania na maszynie wirtualnej.

Po zakończeniu śledzenia i spisu rozwiązania dołączania powiadomienia o zmianie, kliknij **zarządzania aktualizacjami** w obszarze **zarządzania konfiguracją**.

Rozwiązanie do zarządzania aktualizacji umożliwia zarządzanie aktualizacje i poprawki dla maszyn wirtualnych systemu Windows Azure. Można ocenić stan dostępne aktualizacje, harmonogram instalacji wymaganych aktualizacji i przegląd wyników wdrożenia Sprawdź aktualizacje zostały pomyślnie zastosowane do maszyny Wirtualnej. Ta akcja włączone rozwiązania dla maszyny Wirtualnej.

Wybierz **zarządzanie aktualizacjami** w obszarze **zarządzania AKTUALIZACJAMI**. Obszar roboczy analizy dzienników, które zostały wybrane jest tym samym obszarze roboczym w poprzednim kroku. Kliknij przycisk **włączyć** dołączyć rozwiązania do zarządzania aktualizacji. Rozwiązanie trwa do 15 minut, aby włączyć.

![Rozwiązanie dołączenia aktualizacji](media/automation-onboard-solutions-from-automation-account/onboardsolutions2.png)

## <a name="scope-configuration"></a>Konfiguracja zakresu

Każde rozwiązanie używa konfiguracji zakresu w obszarze roboczym pod kątem komputerów, które rozwiązanie. Konfiguracja zakresu jest grupą co najmniej jeden zapisanych wyszukiwań służy do ograniczania zakresu rozwiązanie do określonych komputerów. Do konfiguracji zakresu, na Twoim koncie automatyzacji w obszarze **powiązane zasoby**, wybierz pozycję **obszaru roboczego**. Następnie w obszarze roboczym **źródeł danych obszaru roboczego**, wybierz pozycję **konfiguracji zakresu**.

Domyślnie tworzone dwie konfiguracje zakresu **śledzenia zmian MicrosoftDefaultScopeConfig** i **MicrosoftDefaultScopeConfig aktualizacje**.

## <a name="saved-searches"></a>Zapisane wyszukiwania

Po dodaniu komputera do zarządzania aktualizacjami lub śledzenia zmian i spisu rozwiązań, ich dodawania do jednej z dwóch zapisane wyszukiwania w obszarze roboczym. Zapisane wyszukiwania są zapytania, które zawierają komputery, które są przeznaczone dla tych rozwiązań.

Przejdź do swojego konta automatyzacji i wybierz **zapisane wyszukiwania** w obszarze **ogólne**. Dwa zapisanych wyszukiwań używane przez te rozwiązania są widoczne w poniższej tabeli:

|Name (Nazwa)     |Kategoria  |Alias  |
|---------|---------|---------|
|MicrosoftDefaultComputerGroup     |  ChangeTracking       | ChangeTracking__MicrosoftDefaultComputerGroup        |
|MicrosoftDefaultComputerGroup     | Aktualizacje        | Updates__MicrosoftDefaultComputerGroup         |

Wybierz albo zapisane wyszukiwanie, aby wyświetlić zapytania używany do wypełniania grupy. Na poniższej ilustracji przedstawiono zapytania, a jego wyniki:

![Zapisane wyszukiwania](media/automation-onboard-solutions-from-automation-account/savedsearch.png)

## <a name="onboard-an-azure-machine"></a>Dołączyć maszynę platformy Azure

Twoje automatyzacji konta wybierz **spisu** lub **śledzenie zmian** w obszarze **zarządzania konfiguracją**, lub **zarządzanie aktualizacjami** w obszarze **zarządzania AKTUALIZACJAMI**.

Kliknij przycisk **+ Dodaj maszyny Wirtualnej Azure**, wybierz maszynę Wirtualną z listy. Na **zarządzania aktualizacjami** kliknij przycisk **włączyć**. Spowoduje to dodanie bieżącej maszyny Wirtualnej do grupy komputerów zapisanego wyszukiwania dla rozwiązania.

## <a name="onboard-a-non-azure-machine"></a>Dodać komputer z systemem innym niż Azure

Twoje automatyzacji konta wybierz **spisu** lub **śledzenie zmian** w obszarze **zarządzania konfiguracją**, lub **zarządzanie aktualizacjami** w obszarze **zarządzania AKTUALIZACJAMI**.

Kliknij przycisk **maszynę z systemem innym niż Azure Dodaj**. Spowoduje to otwarcie się nowe okno przeglądarki z instrukcjami dotyczącymi sposobu instalowania i konfigurowania programu Microsoft Monitoring Agent na komputerze, komputer można rozpocząć raportowania do rozwiązania. W przypadku przechodzenia do komputera, który obecnie zarządzany przez program System Center Operations Manager nie jest wymagany nowy agent, informacje o obszarze roboczym jest wprowadzany do istniejącego agenta.

## <a name="onboard-machines-in-the-workspace"></a>Dołączenia maszyny w obszarze roboczym

Twoje automatyzacji konta wybierz **spisu** lub **śledzenie zmian** w obszarze **zarządzania konfiguracją**, lub **zarządzanie aktualizacjami** w obszarze **zarządzania AKTUALIZACJAMI**.

Wybierz **Zarządzanie maszynami**. Spowoduje to otwarcie **Zarządzanie maszyny** strony. Ta strona umożliwia rozwiązanie na wybierz zestaw komputerów, wszystkie dostępne maszyny, Włącz lub Włącz rozwiązania na wszystkich komputerach bieżąca i włączenie go dla wszystkich przyszłych maszyn.

![Zapisane wyszukiwania](media/automation-onboard-solutions-from-automation-account/managemachines.png)

### <a name="selected-machines"></a>Wybrane maszyny

Aby włączyć rozwiązania dla co najmniej jednej maszyny, zaznacz **włączyć na wybranych komputerach** i kliknij przycisk **dodać** obok każdego komputera ma zostać dodany do rozwiązania. To zadanie dodaje nazwy wybranych komputerów do grupy komputerów zapisane kwerendy wyszukiwania dla rozwiązania.

### <a name="all-available-machines"></a>Wszystkie dostępne maszyny

Aby włączyć rozwiązania dla wszystkich maszyn dostępna, zaznacz **Włącz na wszystkich komputerach dostępne**. Powoduje wyłączenie formantu można pojedynczo dodać maszyny. To zadanie dodaje wszystkie nazwy komputerów, raportowanie do obszaru roboczego do grupy komputerów zapisane kwerendy wyszukiwania.

### <a name="all-available-and-future-machines"></a>Wszystkie dostępne i przyszłych maszyny

Aby włączyć rozwiązania dla wszystkich dostępnych maszyn i wszystkie maszyny w przyszłości, zaznacz **Włącz na wszystkich komputerach dostępne i przyszłych**. Ta opcja usuwa zapisanych wyszukiwań i konfiguracji zakresu, w obszarze roboczym. Spowoduje to otwarcie rozwiązania do wszystkich Azure i maszyny z systemem innym niż Azure, które raporty do obszaru roboczego.

## <a name="next-steps"></a>Kolejne kroki

Nadal samouczki dotyczące rozwiązań informacje na temat korzystania z nich.

* [Samouczek — Zarządzanie aktualizacji dla maszyny Wirtualnej](automation-tutorial-update-management.md)

* [Samouczek — identyfikacji oprogramowania na maszynie Wirtualnej](automation-tutorial-installed-software.md)

* [Samouczek — Rozwiązywanie problemów z zmiany na maszynie Wirtualnej](automation-tutorial-troubleshoot-changes.md)