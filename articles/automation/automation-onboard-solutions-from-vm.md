---
title: "Dowiedz się, jak dołączyć rozwiązań do zarządzania aktualizacjami, śledzenia zmian i magazynu z maszyny wirtualnej platformy Azure"
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
ms.openlocfilehash: a850189406b394e7935763206f9e3a191b415170
ms.sourcegitcommit: 83ea7c4e12fc47b83978a1e9391f8bb808b41f97
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="onboard-update-management-change-tracking-and-inventory-solutions-from-an-azure-virtual-machine"></a>Zintegrowanego rozwiązania zarządzania aktualizacjami, śledzenia zmian i magazynu z maszyny wirtualnej platformy Azure

Automatyzacja Azure zapewnia rozwiązania do zarządzania aktualizacjami zabezpieczeń systemu operacyjnego, śledzenie zmian i magazynowych zainstalowanych w komputerach. Istnieje wiele sposobów, aby dołączyć maszyny, możesz dołączyć rozwiązania z maszyną wirtualną [z Twojego konta automatyzacji](automation-onboard-solutions-from-automation-account.md), lub [runbook](automation-onboard-solutions.md). W tym artykule omówiono dołączania tych rozwiązań z maszyny wirtualnej platformy Azure.

## <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure.

Zaloguj się do platformy Azure pod adresem https://portal.azure.com

## <a name="enable-the-solutions"></a>Włącz rozwiązania

Przejdź do istniejącej maszyny wirtualnej, a następnie wybierz opcję **zarządzanie aktualizacjami**, **spisu**, lub **śledzenie zmian** w obszarze **operacji**.

Wybierz obszar roboczy i automatyzacji konta analizy dzienników i kliknij przycisk **włączyć** umożliwiające rozwiązanie. Rozwiązanie trwa do 15 minut, aby włączyć.

![Dodaj rozwiązanie aktualizacji](media/automation-onboard-solutions-from-vm/onboard-solution.png)

Przejdź do innych rozwiązań i kliknij **włączyć**, analizy dzienników, a konto automatyzacji list rozwijanych są wyłączone, ponieważ używają tego samego konta dla obszaru roboczego automatyzacji jako rozwiązanie wcześniej włączony.

![Dodaj rozwiązanie aktualizacji](media/automation-onboard-solutions-from-vm/onboard-solutions2.png)

> [!NOTE]
> **Śledzenie zmian** i **spisu** używać tego samego rozwiązania, gdy jeden jest włączona innych także włączone.

## <a name="scope-configuration"></a>Konfiguracja zakresu

Każde rozwiązanie używa konfiguracji zakresu w obszarze roboczym pod kątem komputerów, które rozwiązanie. Konfiguracja zakresu jest grupą co najmniej jeden zapisanych wyszukiwań służy do ograniczania zakresu rozwiązanie do określonych komputerów. Do konfiguracji zakresu, na Twoim koncie automatyzacji w obszarze **powiązane zasoby**, wybierz pozycję **obszaru roboczego** następnie w obszarze roboczym **źródeł danych obszaru roboczego**, Wybierz **konfiguracji zakresu**.

Konfiguracje zakresu dwa domyślnie tworzone są **śledzenia zmian MicrosoftDefaultScopeConfig** i **MicrosoftDefaultScopeConfig aktualizacje**.

Kliknij przycisk wielokropka (...) na wszystkich konfiguracji i wybierz **Edytuj**. Na **konfigurację zakresu edycji** wybierz pozycję **wybierz grupy komputerów** otworzyć **grup komputerów** strony. Ta strona przedstawia zapisanych wyszukiwań, które są używane do tworzenia zakresu konfiguracji.

## <a name="saved-searches"></a>Zapisane wyszukiwania

Po dodaniu komputera do zarządzania aktualizacjami lub śledzenia zmian i spisu rozwiązań, ich dodawania do jednej z dwóch zapisane wyszukiwania w obszarze roboczym. Zapisane wyszukiwania są zapytania, które zawierają komputery, które są przeznaczone dla tych rozwiązań.

Przejdź do obszaru roboczego i wybierz **zapisane wyszukiwania** w obszarze **ogólne**. Dwa zapisanych wyszukiwań używane przez te rozwiązania są widoczne w poniższej tabeli:

|Name (Nazwa)     |Kategoria  |Alias  |
|---------|---------|---------|
|MicrosoftDefaultComputerGroup     |  ChangeTracking       | ChangeTracking__MicrosoftDefaultComputerGroup        |
|MicrosoftDefaultComputerGroup     | Aktualizacje        | Updates__MicrosoftDefaultComputerGroup         |

Wybierz albo zapisane wyszukiwanie, aby wyświetlić zapytania używany do wypełniania grupy. Na poniższej ilustracji przedstawiono zapytania, a jego wyniki.

![Zapisane wyszukiwania](media/automation-onboard-solutions-from-vm/logsearch.png)

## <a name="next-steps"></a>Kolejne kroki

Nadal samouczki dotyczące rozwiązań informacje na temat korzystania z nich.

* [Samouczek — Zarządzanie aktualizacji dla maszyny Wirtualnej](automation-tutorial-update-management.md)

* [Samouczek — identyfikacji oprogramowania na maszynie Wirtualnej](automation-tutorial-installed-software.md)

* [Samouczek — Rozwiązywanie problemów z zmiany na maszynie Wirtualnej](automation-tutorial-troubleshoot-changes.md)
