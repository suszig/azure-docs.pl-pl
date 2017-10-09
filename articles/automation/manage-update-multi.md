---
title: "Zarządzanie aktualizacjami dla wielu maszyn wirtualnych platformy Azure | Microsoft Docs"
description: "Dowiedz się, jak dołączyć maszyny wirtualne platformy Azure do zarządzania aktualizacjami."
services: operations-management-suite
documentationcenter: 
author: eslesar
manager: carmonm
editor: 
ms.assetid: 
ms.service: operations-management-suite
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/25/2017
ms.author: eslesar
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 89bf87f27fdf276068cba261fc6ae1660307e0b7
ms.contentlocale: pl-pl
ms.lasthandoff: 09/25/2017

---

# <a name="manage-updates-for-multiple-azure-virtual-machines"></a>Zarządzanie aktualizacjami dla wielu maszyn wirtualnych platformy Azure

Rozwiązanie Update Management pozwala zarządzać aktualizacjami i poprawkami dla maszyn wirtualnych platformy Azure.
Na koncie usługi [Azure Automation](automation-offering-get-started.md) można szybko dołączać maszyny wirtualne, oceniać stan dostępnych aktualizacji, planować instalację wymaganych aktualizacji oraz przeglądać wyniki wdrożenia w celu sprawdzenia, czy aktualizacje zostały pomyślnie zastosowane do wszystkich maszyn wirtualnych, dla których włączono rozwiązanie Update Management.

## <a name="prerequisites"></a>Wymagania wstępne

Do wykonania czynności przedstawionych w tym przewodniku są potrzebne następujące elementy:

* Konto usługi Azure Automation. Aby uzyskać instrukcje dotyczące tworzenia konta Uruchom jako usługi Azure Automation, zobacz [Konto Uruchom jako platformy Azure](automation-sec-configure-azure-runas-account.md).
* Maszyna wirtualna usługi Azure Resource Manager (nie klasyczna). Aby uzyskać instrukcje dotyczące tworzenia maszyny wirtualnej, zobacz [Tworzenie pierwszej maszyny wirtualnej z systemem Windows w witrynie Azure Portal](../virtual-machines/virtual-machines-windows-hero-tutorial.md)

## <a name="enable-update-management-for-azure-virtual-machines"></a>Włączanie rozwiązania Update Management dla maszyn wirtualnych platformy Azure

1. W witrynie Azure Portal otwórz konto usługi Automation.
2. Po lewej stronie ekranu wybierz pozycję **Update Management**.
3. W górnej części ekranu kliknij pozycję **Dodaj maszynę wirtualną Azure**.
    ![Dołączanie maszyn wirtualnych](./media/manage-update-multi/update-onboard-vm.png)
4. Wybierz maszynę wirtualną, którą chcesz dołączyć. Zostanie wyświetlony ekran **Włączanie rozwiązania Update Management**.
5. Kliknij przycisk **Włącz**.

   ![Włączanie rozwiązania Update Management](./media/manage-update-multi/update-enable.png)

Rozwiązanie Update Management zostanie włączone dla maszyny wirtualnej.

## <a name="view-update-assessment"></a>Wyświetlanie oceny aktualizacji

Po włączeniu rozwiązania **Update Management** zostanie wyświetlony ekran **Update Management**. Możesz wyświetlić listę brakujących aktualizacji na karcie **Brakujące aktualizacje**.

## <a name="schedule-an-update-deployment"></a>Planowanie wdrożenia aktualizacji

Aby zainstalować aktualizacje, zaplanuj wdrożenie zgodnie z harmonogramem wydawania i oknem obsługi.
Możesz wybrać typy aktualizacji, które mają zostać uwzględnione we wdrożeniu. Możesz na przykład uwzględnić aktualizacje krytyczne lub aktualizacje zabezpieczeń i wykluczyć pakiety zbiorcze aktualizacji.

Zaplanuj nowe wdrożenie aktualizacji dla co najmniej jednej maszyny wirtualnej, klikając pozycję **Zaplanuj wdrażanie aktualizacji** w górnej części ekranu **Update Management**. Na ekranie **Nowe wdrożenie aktualizacji** podaj następujące informacje:

* **Nazwa** — wprowadź unikatową nazwę identyfikującą wdrożenie aktualizacji.
* **Typ systemu operacyjnego** — wybierz system Windows lub Linux.
* **Komputery do zaktualizowania** — wybierz maszyny wirtualne, które chcesz zaktualizować.

  ![Wybieranie maszyn wirtualnych do zaktualizowania](./media/manage-update-multi/update-select-computers.png)

* **Klasyfikacja aktualizacji** — wybierz typy oprogramowania, które zostaną uwzględnione we wdrożeniu aktualizacji. Dostępne są następujące typy klasyfikacji:
  * Aktualizacje krytyczne
  * Aktualizacje zabezpieczeń
  * Pakiety zbiorcze aktualizacji
  * Pakiety funkcji
  * Dodatki Service Pack
  * Aktualizacje definicji
  * Narzędzia
  * Aktualizacje
* **Ustawienia harmonogramu** — możesz zaakceptować domyślną datę i godzinę (30 minut po bieżącej godzinie) lub określić inny czas.
   Możesz też określić, czy wdrożenie ma występować raz, czy zgodnie z ustawionym harmonogramem cyklicznym. Kliknij opcję Cyklicznie w obszarze Cykl, aby skonfigurować harmonogram cykliczny.

   ![Ekran ustawień harmonogramu aktualizacji](./media/manage-update-multi/update-set-schedule.png)

* **Okno konserwacji (w minutach)** — podaj okres, w którym ma zostać przeprowadzone wdrażanie aktualizacji.  Pozwala to zagwarantować, że zmiany będą wprowadzane w ramach zdefiniowanych okien obsługi.

Po ukończeniu konfigurowania harmonogramu kliknij przycisk **Utwórz**, aby wrócić do pulpitu nawigacyjnego stanu.
Tabela **Zaplanowane** zawiera utworzony właśnie harmonogram wdrożenia.

> [!WARNING]
> W przypadku aktualizacji wymagających ponownego uruchomienia maszyna wirtualna zostanie automatycznie uruchomiona ponownie.

## <a name="view-results-of-an-update-deployment"></a>Wyświetlanie wyników wdrażania aktualizacji

Po rozpoczęciu zaplanowanego wdrażania stan tego wdrożenia można sprawdzić na karcie **Wdrożenia aktualizacji** na ekranie **Update Management**.
Jeśli wdrażanie trwa, wdrożenie ma stan **W toku**. Po pomyślnym ukończeniu wdrażania stan zmienia się na **Powodzenie**.
W przypadku błędu co najmniej jednej aktualizacji w ramach wdrożenia jest wyświetlany stan **Częściowe niepowodzenie**.

![Stan wdrożenia aktualizacji ](./media/manage-update-multi/update-view-results.png)

Kliknij ukończone wdrożenie aktualizacji, aby wyświetlić pulpit nawigacyjny tego wdrożenia.

Kafelek **Wyniki aktualizacji** zawiera podsumowanie z łączną liczbą aktualizacji i wynikami wdrożenia na maszynie wirtualnej.
Tabela po prawej stronie zawiera szczegółowy podział każdej aktualizacji i wyniki instalacji, które mogą mieć jedną z następujących wartości:

* Nie podjęto próby — nie zainstalowano aktualizacji z powodu niewystarczającego czasu w zdefiniowanym oknie konserwacji.
* Powodzenie — aktualizacja powiodła się
* Niepowodzenie — aktualizacja nie powiodła się

Kliknij pozycję **Wszystkie dzienniki**, aby wyświetlić wszystkie wpisy dziennika utworzone przez wdrożenie.

Kliknij kafelek **Dane wyjściowe**, aby wyświetlić strumień zadań elementu runbook odpowiedzialnego za zarządzanie wdrożeniem aktualizacji na docelowej maszynie wirtualnej.

Kliknij pozycję **Błędy**, aby wyświetlić szczegółowe informacje o błędach związanych z wdrożeniem.

## <a name="next-steps"></a>Następne kroki

* Aby dowiedzieć się więcej na temat rozwiązania Update Management, zobacz [Update Management](../operations-management-suite/oms-solution-update-management.md).
