---
title: "Śledzenie zmian w maszynach wirtualnych platformy Azure | Dokumentacja firmy Microsoft"
description: "Użyj śledzenie zmian, aby śledzić zmiany w tych plików i rejestru na maszynach wirtualnych."
services: automation
documentationcenter: automation
author: georgewallace
manager: carmonm
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: automation
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 09/25/2017
ms.author: gwallace
ms.custom: 
ms.openlocfilehash: 3a661fada2e768c2206183c125593d019b557c1d
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/14/2017
---
# <a name="track-changes-in-your-azure-virtual-machines"></a>Śledzenie zmian w maszynach wirtualnych platformy Azure

Przez włączenie śledzenia zmian, można śledzić zmiany do plików i kluczy rejestru systemu Windows na maszynach wirtualnych. Identyfikowanie zmian konfiguracji może pomóc w określeniu problemy z działaniem.

Można włączyć śledzenie bezpośrednio z sieci maszyny wirtualnej platformy Azure zmian.

Jeśli nie ma maszyny wirtualnej platformy Azure, możesz utworzyć jedną zgodnie z instrukcjami w [Szybki Start systemu Windows](../virtual-machines/windows/quick-create-portal.md) lub [szybkiego startu Linux](../virtual-machines/linux/quick-create-portal.md) artykułu.

## <a name="sign-in-to-the-azure-portal"></a>Logowanie się do witryny Azure Portal
Zaloguj się w witrynie [Azure Portal](https://portal.azure.com/).

## <a name="enable-change-tracking-for-an-azure-virtual-machine"></a>Włączanie śledzenia zmian dla maszyny wirtualnej platformy Azure

1. W okienku po lewej stronie witryny Azure Portal wybierz pozycję **Maszyny wirtualne**.
2. Na liście wybierz maszynę wirtualną.
3. W maszyny wirtualnej okna, w obszarze **operacji**, wybierz pozycję **śledzenia zmian**. 

   ![Maszyna wirtualna dołączyć śledzenia zmian](./media/automation-vm-change-tracking/change-onboard-vm-blade.png)  
    **Włączyć zarządzanie aktualizacjami** zostanie otwarte okno.

    Weryfikacja jest przeprowadzana w celu ustalenia, czy jest włączone śledzenie zmian dla tej maszyny wirtualnej. Jeśli nie jest włączone śledzenie zmian, transparent jest wyświetlone, można włączyć rozwiązania.

   ![Transparent dołączyć konfiguracji śledzenia zmian](./media/automation-vm-change-tracking/change-onboard-banner.png)

4. Aby włączyć rozwiązanie, wybierz baner. Nie masz następujące elementy, które są dodawane automatycznie:

   * [Zaloguj się Analytics](../log-analytics/log-analytics-overview.md) obszaru roboczego
   * [Automatyzacja](../automation/automation-offering-get-started.md) konta

5. Wybierz obszar roboczy analizy dzienników, aby przechowywać dzienniki danych ze śledzenia zmian, wybierz konto usługi Automatyzacja, aby śledzić zmiany, a następnie wybierz **włączyć**.  
    Pojawi się pasek stanu z informacją o tym, że trwa włączanie rozwiązania. Ten proces może potrwać do 15 minut.

## <a name="configure-change-tracking"></a>Konfigurowanie śledzenia zmian

Po włączeniu śledzenia zmian **śledzenie zmian** zostanie wyświetlone okno. 

Aby wybrać, które pliki i klucze rejestru, aby śledzić, wybierz **edytować ustawienia**.

   ![Śledzenie zmian edytować ustawienia](./media/automation-vm-change-tracking/change-edit-settings.png)

   **Konfiguracji obszaru roboczego** zostanie otwarte okno. 

W **konfiguracji obszaru roboczego** okno Dodawanie kluczy rejestru systemu Windows, plików systemu Windows lub Linux pliki mają być śledzone, zgodnie z opisem w trzech kolejnych sekcjach.

### <a name="add-a-windows-registry-key"></a>Dodawanie klucza rejestru systemu Windows

1. Na **rejestru systemu Windows** wybierz opcję **Dodaj**.  
    **Dodać rejestru systemu Windows dla śledzenia zmian** zostanie otwarte okno.

   ![Śledzenie zmian Dodaj rejestru](./media/automation-vm-change-tracking/change-add-registry.png)

2. W obszarze **włączone**, wybierz pozycję **True**.
3. W **nazwa elementu** Wprowadź przyjazną nazwę.
4. (Opcjonalnie) W **grupy** wprowadź nazwę grupy.
5. W **klucza rejestru systemu Windows** wpisz nazwę klucza rejestru, które mają być śledzone.
6. Wybierz pozycję **Zapisz**.

### <a name="add-a-windows-file"></a>Dodawanie pliku systemu Windows

1. Na **plików systemu Windows** wybierz opcję **Dodaj**.  
    **Dodawanie pliku systemu Windows dla śledzenia zmian** zostanie otwarte okno.

   ![Śledzenie zmian Dodaj plik systemu Windows](./media/automation-vm-change-tracking/change-add-win-file.png)

2. W obszarze **włączone**, wybierz pozycję **True**.
3. W **nazwa elementu** Wprowadź przyjazną nazwę.
4. (Opcjonalnie) W **grupy** wprowadź nazwę grupy.
5. W **wprowadź ścieżkę** wprowadź pełną ścieżkę i nazwę pliku, mają być śledzone.
6. Wybierz pozycję **Zapisz**.

### <a name="add-a-linux-file"></a>Dodaj plik systemu Linux

1. Na **pliki Linux** wybierz opcję **Dodaj**.  
    **Dodaj plik Linux do śledzenia zmian** zostanie otwarte okno.

   ![Śledzenie zmian Dodaj plik systemu Linux](./media/automation-vm-change-tracking/change-add-linux-file.png)

2. W obszarze **włączone**, wybierz pozycję **True**.
3. W **nazwa elementu** Wprowadź przyjazną nazwę.
4. (Opcjonalnie) W **grupy** wprowadź nazwę grupy.
5. W **wprowadź ścieżkę** wprowadź pełną ścieżkę i nazwę pliku, mają być śledzone.
6. W **typ ścieżki** albo wybierz **pliku** lub **katalogu**.
7. W obszarze **rekursji**, aby śledzić zmiany w określonej ścieżce i wszystkich plików i ścieżek w nim, wybierz **na**. Aby śledzić tylko wybrane ścieżka lub nazwa pliku, wybierz **poza**.
8. W obszarze **Sudo użyj**, aby śledzić pliki, które wymagają `sudo` dostępu, wybierz polecenie **na**. W przeciwnym razie wybierz **poza**.
9. Wybierz pozycję **Zapisz**.

## <a name="view-changes"></a>Przeglądanie zmian

W **śledzenie zmian** okna, można wyświetlić zmiany w każdej z różnych kategorii dla maszyny wirtualnej w czasie.

   ![Śledzenie zmian dodać zmiany widoku](./media/automation-vm-change-tracking/change-view-changes.png)

Można wybrać kategorie i zmiany, aby wyświetlić przedział czasu. W górnej części okna można wyświetlić graficzną reprezentację zmian w czasie. W dolnej części okna można wyświetlić listę najnowszych zmian.

## <a name="view-change-tracking-log-data"></a>Wyświetl dane dziennika śledzenia zmian

Śledzenie zmian generuje dane dziennika, które są wysyłane do analizy dzienników. Wyszukiwanie w dziennikach uruchamiania zapytań, wybierz **analizy dzienników** w górnej części **śledzenie zmian** okna.

   ![Analiza dzienników śledzenia zmian](./media/automation-vm-change-tracking/change-log-analytics.png)

Aby dowiedzieć się więcej na temat uruchamiania i wyszukiwania plików dziennika w analizy dzienników, zobacz [analizy dzienników](../log-analytics/log-analytics-overview.md).

## <a name="next-steps"></a>Następne kroki

* Aby dowiedzieć się więcej na temat śledzenia zmian, zobacz [śledzić zmiany oprogramowania w środowisku dzięki rozwiązaniu do śledzenia zmiany](../log-analytics/log-analytics-change-tracking.md).
* Aby dowiedzieć się więcej na temat zarządzania aktualizacjami dla maszyn wirtualnych, zobacz [rozwiązania zarządzania aktualizacjami w OMS](../operations-management-suite/oms-solution-update-management.md).
