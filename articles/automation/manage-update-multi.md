---
title: "Zarządzanie aktualizacjami dla wielu maszyn wirtualnych platformy Azure | Microsoft Docs"
description: "W tym temacie przedstawiono sposób zarządzania aktualizacjami dla maszyn wirtualnych platformy Azure."
services: automation
documentationcenter: 
author: eslesar
manager: carmonm
editor: 
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/31/2017
ms.author: magoedte;eslesar
ms.openlocfilehash: 80a6caff51631637825d560d270198be0336e806
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/01/2017
---
# <a name="manage-updates-for-multiple-azure-virtual-machines"></a>Zarządzanie aktualizacjami dla wielu maszyn wirtualnych platformy Azure

Rozwiązanie Update Management pozwala zarządzać aktualizacjami i poprawkami dla maszyn wirtualnych platformy Azure.
Na koncie usługi [Azure Automation](automation-offering-get-started.md) można szybko dołączać maszyny wirtualne, oceniać stan dostępnych aktualizacji, planować instalację wymaganych aktualizacji oraz przeglądać wyniki wdrożenia w celu sprawdzenia, czy aktualizacje zostały pomyślnie zastosowane do wszystkich maszyn wirtualnych, dla których włączono rozwiązanie Update Management.

## <a name="prerequisites"></a>Wymagania wstępne

Do korzystania z rozwiązania Update Management są wymagane następujące elementy:

* Konto usługi Azure Automation. Aby uzyskać instrukcje dotyczące tworzenia konta Uruchom jako usługi Azure Automation, zobacz [Rozpoczynanie pracy z usługą Azure Automation](automation-offering-get-started.md).

* Maszyna wirtualna lub komputer z zainstalowanym obsługiwanym systemem operacyjnym.

## <a name="supported-operating-systems"></a>Obsługiwane systemy operacyjne

Rozwiązanie Update Management jest obsługiwane w następujących systemach operacyjnych:

### <a name="windows"></a>Windows

* Windows Server 2008 lub nowszy i wdrożenia aktualizacji dla systemu Windows Server 2008 R2 z dodatkiem SP1 lub nowszego.  Opcje instalacji Server Core i Nano Server nie są obsługiwane.

    > [!NOTE]
    > Obsługa wdrażania aktualizacji dla systemu Windows Server 2008 R2 SP1 wymaga programu .NET Framework 4.5 i programu WMF 5.0 lub nowszego.
    > 
* Systemy operacyjne klienta systemu Windows nie są obsługiwane.

Agenci dla systemu Windows muszą być skonfigurowani do komunikowania się z serwerem Windows Server Update Services (WSUS) albo mieć dostęp do usługi Microsoft Update.

> [!NOTE]
> Agent dla systemu Windows nie może być zarządzany współbieżnie przez program System Center Configuration Manager.
>

### <a name="linux"></a>Linux

* CentOS 6 (x86/x64) i 7 (x64)  
* Red Hat Enterprise 6 (x86/x64) i 7 (x64)  
* SUSE Linux Enterprise Server 11 (x86/x64) i 12 (x64)  
* Ubuntu 12.04 LTS i nowsze x86/x64   

> [!NOTE]  
> Aby w przypadku systemu Ubuntu uniknąć stosowania aktualizacji poza oknem obsługi, zmień konfigurację pakietu Unattended-Upgrade tak, aby wyłączyć automatyczne aktualizacje. Aby uzyskać informacje na temat sposobu konfigurowania tego, zobacz [temat poświęcony aktualizacjom automatycznym w podręczniku systemu Ubuntu Server](https://help.ubuntu.com/lts/serverguide/automatic-updates.html).

Agenci dla systemu Linux muszą mieć dostęp do repozytorium aktualizacji.

> [!NOTE]
> Agent usługi OMS dla systemu Linux skonfigurowany pod kątem raportowania do wielu obszarów roboczych usługi OMS nie jest obsługiwany przez to rozwiązanie.  
>

## <a name="enable-update-management-for-azure-virtual-machines"></a>Włączanie rozwiązania Update Management dla maszyn wirtualnych platformy Azure

1. W witrynie Azure Portal otwórz konto usługi Automation.
2. Po lewej stronie ekranu wybierz pozycję **Update Management**.
3. W górnej części ekranu kliknij pozycję **Dodaj maszynę wirtualną Azure**.
    ![Dołączanie maszyn wirtualnych](./media/manage-update-multi/update-onboard-vm.png)
4. Wybierz maszynę wirtualną, którą chcesz dołączyć. Zostanie wyświetlony ekran **Włączanie rozwiązania Update Management**.
5. Kliknij przycisk **Włącz**.

   ![Włączanie rozwiązania Update Management](./media/manage-update-multi/update-enable.png)

Rozwiązanie Update Management zostanie włączone dla maszyny wirtualnej.

## <a name="enable-update-management-for-non-azure-virtual-machines-and-computers"></a>Włączanie rozwiązania Update Management dla maszyn wirtualnych i komputerów spoza platformy Azure

Aby uzyskać instrukcje dotyczące włączania rozwiązania Update Management dla maszyn wirtualnych i komputerów z systemem Windows spoza platformy Azure, zobacz [Łączenie komputerów z systemem Windows z usługą Log Analytics na platformie Azure](../log-analytics/log-analytics-windows-agents.md).

Aby uzyskać instrukcje dotyczące włączania rozwiązania Update Management dla maszyn wirtualnych i komputerów z systemem Linux spoza platformy Azure, zobacz [Connect your Linux Computers to Operations Management Suite (OMS) (Łączenie komputerów z systemem Linux z pakietem Operations Management Suite — OMS)](../log-analytics/log-analytics-agent-linux.md).

## <a name="view-update-assessment"></a>Wyświetlanie oceny aktualizacji

Po włączeniu rozwiązania **Update Management** zostanie wyświetlony ekran **Update Management**. Możesz wyświetlić listę brakujących aktualizacji na karcie **Brakujące aktualizacje**.

## <a name="data-collection"></a>Zbieranie danych

Agenci zainstalowani na maszynach wirtualnych i komputerach na potrzeby zbierania danych o aktualizacjach i wysyłania ich do rozwiązania Azure Update Management.

### <a name="supported-agents"></a>Obsługiwani agenci

W poniższej tabeli opisano połączone źródła, które obsługuje to rozwiązanie.

| Połączone źródło | Obsługiwane | Opis |
| --- | --- | --- |
| Agenci dla systemu Windows |Tak |Rozwiązanie Update Management zbiera informacje o aktualizacjach systemu z agentów dla systemu Windows i inicjuje instalowanie wymaganych aktualizacji. |
| Agenci dla systemu Linux |Tak |Rozwiązanie Update Management zbiera informacje o aktualizacjach systemu z agentów dla systemu Linux i inicjuje instalowanie wymaganych aktualizacji w obsługiwanych dystrybucjach. |
| Grupa zarządzania programu Operations Manager |Tak |Rozwiązanie Update Management zbiera informacje o aktualizacjach systemu z agentów w połączonej grupie zarządzania. |
| Konto magazynu Azure |Nie |Magazyn Azure nie zawiera informacji o aktualizacjach systemu. |

### <a name="collection-frequency"></a>Częstotliwość zbierania

Skanowanie każdego zarządzanego komputera z systemem Windows odbywa się dwa razy dziennie. Co 15 minut wywoływany jest interfejs API systemu Windows, aby wykonać zapytanie o czas ostatniej aktualizacji w celu sprawdzenia, czy stan się zmienił, a jeśli tak, inicjowane jest skanowanie pod kątem zgodności.  Skanowanie każdego zarządzanego komputera z systemem Linux odbywa się co 3 godziny.

Wyświetlenie zaktualizowanych danych z zarządzanych komputerów na pulpicie nawigacyjnym może potrwać od 30 minut do 6 godzin.

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

Aby uzyskać szczegółowe informacje o dziennikach, danych wyjściowych i błędach, zobacz [Update Management](../operations-management-suite/oms-solution-update-management.md).

## <a name="next-steps"></a>Następne kroki

* Aby dowiedzieć się więcej na temat rozwiązania Update Management, zobacz [Update Management](../operations-management-suite/oms-solution-update-management.md).