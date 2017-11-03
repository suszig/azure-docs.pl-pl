---
title: Archiwizowanie danych monitorowania Azure | Dokumentacja firmy Microsoft
description: "Archiwizowanie danych dziennika i metrykę, utworzone w obrębie platformy Azure na konto magazynu."
author: johnkemnetz
manager: orenr
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.service: monitoring-and-diagnostics
ms.topic: tutorial
ms.date: 09/25/2017
ms.author: johnkem
ms.custom: mvc
ms.openlocfilehash: 445901a740920a74f259aaa9c6b862680c1c807e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="archive-azure-monitoring-data"></a>Azure archiwum danych monitorowania

Kilku warstw środowiska Azure utworzyć dziennika i metryki danych, które mogą być archiwizowane, do konta magazynu platformy Azure. Można to zrobić, aby zachować historii danych monitorowania w czasie w magazynie niedrogich, niemożliwych po upływie jego okres przechowywania w Log Analytics lub Azure monitora tych danych. Ten samouczek kroki przez proces konfigurowania środowiska Azure do archiwum danych na konto magazynu.

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne](https://azure.microsoft.com/free/) konto.

## <a name="sign-in-to-the-azure-portal"></a>Logowanie się do witryny Azure Portal

Zaloguj się w witrynie [Azure Portal](https://portal.azure.com/).

## <a name="create-a-storage-account"></a>Tworzenie konta magazynu

Najpierw należy skonfigurować konto magazynu, do którego mają być archiwizowane dane monitorowania. Aby to zrobić, [wykonaj kroki opisane w tym miejscu](../storage/common/storage-create-storage-account.md).

## <a name="route-subscription-logs-to-the-storage-account"></a>Dzienniki subskrypcji trasy do konta magazynu

Teraz można przystąpić do rozpoczęcia konfigurowania środowiska Azure można przekierować danych monitorowania na koncie magazynu. Firma Microsoft skonfiguruj dane poziom subskrypcji (znajdujące się w dzienniku aktywności platformy Azure) do konta magazynu. [ **Dziennika aktywności platformy Azure** ](monitoring-overview-activity-logs.md) zawiera historię zdarzenia na poziomie subskrypcji platformy Azure. Istnieje możliwość przeglądania w portalu Azure w celu określenia *kto* utworzone, zaktualizować lub usunąć *co* zasobów i *podczas* ich działanie.

1. Kliknij przycisk **Monitor** znaleziono przycisku na liście nawigacji po lewej stronie, następnie na **dziennik aktywności**.

   ![Sekcja dziennika aktywności](media/monitor-tutorial-archive-monitoring-data/activity-log-home.png)

2. W sekcji dziennik aktywności, która jest wyświetlana, kliknij na **wyeksportować** przycisku.

3. W **Eksport dziennika aktywności** sekcja, która jest wyświetlana, sprawdź pole **wyeksportować do konta magazynu** i kliknij przycisk **wybierz konto magazynu.**

   ![Eksport dziennika aktywności](media/monitor-tutorial-archive-monitoring-data/activity-log-export.png)

4. W sekcji Użyj **konta magazynu** listy rozwijanej, aby wybrać nazwę na koncie magazynu utworzonym w poprzednim **Utwórz konto magazynu** kroku, a następnie kliknij przycisk **OK**.

   ![Wybierz konto magazynu](media/monitor-tutorial-archive-monitoring-data/activity-log-storage.png)

5. Ustaw **przechowywania (dni)** suwak do 30. Suwak Ustawia liczbę dni, aby zachować dane monitorowania na koncie magazynu. Azure Monitor automatycznie usuwa dane starsze niż liczba dni korzystania z określonego. Przechowywanie zero dni przechowuje dane bezterminowo.

6. Kliknij przycisk **zapisać** i Zamknij w tej sekcji.

Dane monitorowania z subskrypcji jest teraz otrzymywanych przez konta magazynu.

## <a name="route-resource-data-to-the-storage-account"></a>Dane zasobu trasy do konta magazynu

Teraz możemy skonfigurować poziom zasobów danych (zasobu metryki i dzienników diagnostycznych), który będzie kierowany do konta magazynu, konfigurując **ustawień diagnostycznych zasobu**.

1. Kliknij przycisk **Monitor** znaleziono przycisku na liście nawigacji po lewej stronie, następnie na **ustawień diagnostycznych**. W tym miejscu można wyświetlić listę wszystkich zasobów w ramach subskrypcji, które tworzą dane monitorowania za pośrednictwem Monitora Azure. Jeśli nie masz żadnych zasobów na tej liście, możesz [tworzenie aplikacji logiki](../logic-apps/logic-apps-create-a-logic-app.md) przed kontynuowaniem, tak aby było możliwe z zasobem, który można skonfigurować ustawienie diagnostyczne na.

2. Kliknij zasób na liście, a następnie kliknij przycisk **Włącz diagnostykę**.
   
   ![Włącz diagnostykę](media/monitor-tutorial-archive-monitoring-data/diagnostic-settings-turn-on.png)

   Jeśli jest już skonfigurowane ustawienie, możesz zamiast tego zobacz istniejących ustawień i przycisk, aby **Dodaj ustawienie diagnostyczne**. Kliknij ten przycisk.

   Ustawienie diagnostyczne zasobu jest definicją *co* danych monitorowania powinny być kierowane do określonego zasobu i *gdzie* czy dane monitorowania należy przejść.

3. W sekcji zostanie wyświetlona, podać ustawienia **nazwa** i pole wyboru dla **archiwum na konto magazynu**.

   ![Sekcja ustawień diagnostycznych](media/monitor-tutorial-archive-monitoring-data/diagnostic-settings-home.png)

4. Polecenie **Konfiguruj** przycisku w obszarze **archiwum na konto magazynu** i wybierz konto magazynu, został utworzony w poprzedniej sekcji. Kliknij przycisk **OK**.

   ![Konto magazynu ustawień diagnostycznych](media/monitor-tutorial-archive-monitoring-data/diagnostic-settings-storage.png)

5. Zaznacz wszystkie pola w obszarze **dziennika** i **Metryka**. W zależności od typu zasobu użytkownik może mieć tylko jedną z następujących opcji. Te pola wyboru kontrolować kategorie danych dziennika i metrykę, dostępne dla typu zasobu wysłaniem wybrane, w tym przypadku miejsce docelowe konto magazynu.

   ![Kategorie ustawień diagnostycznych](media/monitor-tutorial-archive-monitoring-data/diagnostic-settings-categories.png)
   
6. Ustaw **przechowywania (dni)** suwak do 30. Suwak Ustawia liczbę dni, aby zachować dane monitorowania na koncie magazynu. Azure Monitor automatycznie usuwa dane starsze niż liczba dni korzystania z określonego. Przechowywanie zero dni przechowuje dane bezterminowo.

7. Kliknij pozycję **Zapisz**.

Dane monitorowania z zasobu jest teraz otrzymywanych przez konta magazynu.

## <a name="route-virtual-machine-guest-os-data-to-the-storage-account"></a>Dane trasy maszyna wirtualna (system operacyjny gościa) do konta magazynu

1. Jeśli nie masz już maszyny wirtualnej w ramach subskrypcji, [Utwórz maszynę wirtualną](../virtual-machines/windows/quick-create-portal.md).

2. Na liście nawigacji po lewej stronie portalu kliknij **maszyn wirtualnych**.

3. Na liście maszyny wirtualnej jest wyświetlana kliknij na maszynie wirtualnej, który został utworzony.

4. W sekcji zostanie wyświetlona, kliknij polecenie **ustawień diagnostycznych** w nawigacji po lewej stronie. W tej sekcji można skonfigurować rozszerzenia monitorowania out-of-box z monitora Azure na maszynie wirtualnej i dane trasy tworzonym przez system Windows lub Linux do konta magazynu.

   ![Przejdź do ustawień diagnostycznych](media/monitor-tutorial-archive-monitoring-data/guest-navigation.png)

5. Kliknij przycisk **Włącz monitorowanie na poziomie gościa** który pojawia się w sekcji.

   ![Włącz ustawienia diagnostyki](media/monitor-tutorial-archive-monitoring-data/guest-enable.png)

6. Poprawnie zapisane ustawienie diagnostyczne **omówienie** karta zawiera listę zbieranych danych i gdy jest magazynowana. Polecenie **liczniki wydajności** liczników sekcji, aby przejrzeć zestaw wydajność systemu Windows są zbierane.

   ![Ustawienia liczniki wydajności](media/monitor-tutorial-archive-monitoring-data/guest-perf-counters.png)
   
7. Polecenie **dzienniki** i sprawdź pól wyboru dla **informacji** poziom Dzienniki aplikacji i dzienniki systemu.

   ![Ustawienia dzienników](media/monitor-tutorial-archive-monitoring-data/guest-logs.png)

8. Polecenie **agenta** kartę i w obszarze **konta magazynu** kliknij nazwę konta magazynu wyświetlane.

   ![Aktualizacja konta magazynu](media/monitor-tutorial-archive-monitoring-data/guest-storage-home.png)

9. W sekcji zostanie wyświetlona, wybierz na koncie magazynu utworzonym w poprzednim **Utwórz konto magazynu** kroku.

10. Kliknij pozycję **Zapisz**.

Dane monitorowania z maszyn wirtualnych jest teraz otrzymywanych przez konta magazynu.

## <a name="view-the-monitoring-data-in-the-storage-account"></a>Widok danych monitorowania na koncie magazynu

Po wykonaniu powyższych kroków, danych zostało uruchomione przepływać do konta magazynu.

1. W przypadku niektórych typów danych, na przykład dziennika aktywności, musi istnieć niektóre działania, które generuje zdarzenie na koncie magazynu. Aby wygenerować działania w dzienniku aktywności, wykonaj [tych instrukcji](./monitor-quick-audit-notify-action-in-subscription.md). Może być konieczne odczekanie do pięciu minut przed zdarzeniem pojawia się na koncie magazynu.

2. W portalu, przejdź do **kont magazynu** sekcji znajdując na pasku nawigacyjnym po lewej stronie.

3. Określ konto magazynu, który został utworzony w poprzedniej sekcji i kliknij go.

4. Kliknij **obiekty BLOB**, a następnie w kontenerze etykietą **insights operacyjne — dzienniki** a wreszcie w kontenerze **nazwa = domyślnie**. Jest to kontener, który zawiera dziennik aktywności w nim. Monitorowanie danych jest podzielone w kontenerach wg Identyfikatora zasobu (tylko identyfikator subskrypcji dla dziennika aktywności), a następnie według daty i godziny. Pełny format dla tych obiektów blob jest:

   szczegółowe informacje operacyjne dzienniki/nazwa-= domyślne/resourceId = / SUBSKRYPCJI / {identyfikator subskrypcji} / y = {czterocyfrowy rok liczbowych} / m = {dwucyfrowe liczbowych month} / d = {dwucyfrowe liczbą dzień} / h = {dwucyfrowe 24-godzinnym hour}/m=00/PT1H.json

5. Przejdź do pliku PT1H.json, klikając do kontenerów identyfikator zasobu, daty i godziny. Kliknij plik PT1H.json, a następnie kliknij przycisk **Pobierz**. Każdy obiekt blob PT1H.json zawiera obiektu blob JSON zdarzeń, które wystąpiły w ciągu godziny określonego w adresie URL obiektu blob (na przykład h = 12). Podczas obecny godzinę zdarzenia są dołączane do pliku PT1H.json miarę ich występowania. Wartości minutowe (m = 00) jest zawsze 00, ponieważ zdarzenia dziennika są podzielone na poszczególne obiekty BLOB na godzinę.

   Teraz można wyświetlać zdarzenia JSON, które były przechowywane na koncie magazynu. W przypadku dzienników diagnostycznych zasobu ma format obiektów blob:

   insights — dzienniki — {nazwa kategorii dziennika} / resourceId = / {identyfikator zasobu} / y = {czterocyfrowy rok liczbowych} / m = {dwucyfrowe liczbowych month} / d = {dwucyfrowe liczbą dzień} / h = {dwucyfrowe 24-godzinnym hour}/m=00/PT1H.json

6. Dane monitorowania systemu operacyjnego gościa są przechowywane w tabelach. Przejdź z powrotem do macierzystego konta magazynu, a następnie kliknij przycisk **tabel**. Brak tabel metryki, liczniki wydajności i dzienniki zdarzeń.

Możesz teraz pomyślnie skonfigurowano monitorowanie danych do archiwizacji na konto magazynu.

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

1. Przejdź z powrotem do **Eksportuj Dziennik aktywności** sekcji z poprzednim **przekierowywać dzienniki subskrypcji do konta magazynu** kroku i kliknij przycisk **zresetować**.

2. Przejdź do **ustawień diagnostycznych** kliknij zasób, na którym utworzono ustawienie diagnostyczne w poprzednim **trasy danych zasobów na koncie magazynu** kroku, a następnie znajdź ustawienie możesz utworzone, kliknij przycisk **Edytuj ustawienie** przycisk **usunąć**.

3. Przejdź do **ustawień diagnostycznych** sekcję maszyna wirtualna skonfigurowana w poprzednim **kierować danych maszyny wirtualnej (system operacyjny gościa) do konta magazynu** kroku i w obszarze  **Agent** kliknij kartę **Usuń** (poniżej **agenta diagnostyki Azure Usuń** sekcji).

4. Przejdź do konta magazynu, został utworzony w poprzednim **Utwórz konto magazynu** kroku i kliknij przycisk **usunięcie konta magazynu**. Wpisz nazwę konta magazynu, a następnie kliknij przycisk **usunąć**.

5. Jeśli utworzono maszynę wirtualną lub aplikację logiki dla powyższych kroków, usuń te również.

## <a name="next-steps"></a>Następne kroki

W tym samouczku przedstawiono sposób monitorowania danych z Twoim środowiskiem platformy Azure (subskrypcji, zasobów i system operacyjny gościa), które mają być archiwizowane na konto magazynu. Aby wprowadzić więcej znaczenie poza danych i uzyskania szczegółowych informacji, spróbuj również wysyłanie danych do analizy dzienników.

> [!div class="nextstepaction"]
> [Wprowadzenie do analizy dzienników](../log-analytics/log-analytics-get-started.md)
